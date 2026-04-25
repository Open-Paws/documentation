# Wave Parallelization and File Ownership

Loaded every session. Refines the hard-nevers for concurrent agent work. Most of the file-level isolation Sam hand-rolled in earlier CLAUDE.md drafts is now handled by Claude Code's native `isolation: worktree` primitive on write-stage subagents — this file covers what the worktree primitive doesn't.

## What Worktree Isolation Already Handles

These subagents have `isolation: worktree` in their frontmatter and get an automatic fresh git worktree per invocation, so file-level collision between parallel instances is impossible:

- `test-writer` (STAGE 5)
- `implementer` (STAGE 7)
- `desloppifier` (STAGE 9)

The `.worktreeinclude` file at repo root (gitignored patterns to copy into each new worktree) handles env files, config secrets, and local state. No hand-rolled file-ownership discipline needed for these agents.

## What Worktree Does NOT Isolate

Shared state that lives outside the git tree stays contentious even with worktree isolation:

- **GitHub issue comments / labels** — two agents both commenting on the same issue produce a race. Use idempotent labels (set, don't increment counters) and use comment headers to de-duplicate.
- **Branch push contention** — write-stage worktrees all push back to the same branch. Serialize pushes within a wave; a rebase-then-push pattern prevents non-fast-forward rejections.
- **External APIs** — any coalition-partner API, LLM provider, deployment target. Rate limits are shared.
- **Database migrations** — any stage touching schema runs serially, never parallel.
- **Dependency manifest changes** — `package.json`, `Cargo.toml`, `pyproject.toml`. Parallel dep bumps collide in the lockfile.
- **`.github/workflows/` edits** — workflow changes affect every repo's CI; serialize them.

## Safe-Parallel vs Unsafe-Parallel

| Safe to parallelize | Unsafe — serialize |
|---|---|
| Different repos, same wave | Same repo + overlapping file sets (worktree handles this for write-stages; other stages still discipline) |
| Different components in a monorepo | Migrations (DB schema) |
| Docs + code + tests (separate agents) | Dependency manifest changes |
| Read-only agents (scout, plan-reviewer, adversarial, persona-qa, verifier) | Release tagging |
| Persona QA read-only sweeps | `.github/workflows/` edits |

## Wave Gates

A wave is complete only when **every agent in the wave has written a completion comment** to the issue (or parent orchestration thread). The orchestrator waits for all completions before spawning the next wave. No partial advancement.

In-progress state: issues get the `auto:in-progress` label plus an assignee representing the agent session. **TTL 4 hours of no commits → claim released automatically** by the watchdog workflow. If a wave stalls beyond TTL, the orchestrator re-dispatches to a fresh agent.

## Branch Discipline

One issue = one branch = one PR. Branch naming: `<type>/<issue-number>-<slug>` (e.g. `fix/1234-quest-sync-silent-errors`).

Before any write work:
```bash
git fetch origin
git checkout <branch> || git checkout -b <branch>
git pull --rebase origin <branch>
```

After:
```bash
git add <owned-files-only>      # enforced by hook; never wildcards
git commit -m "<conventional>"
git push origin <branch>
```

## Read-Only Agents Share the Main Tree

Agents without `isolation: worktree` (scout, planner, plan-reviewer, test-reviewer, verifier, adversarial, persona-qa) run in the main checkout. They're read-only by design. If a read-only agent discovers it needs to write, it stops and files a new issue rather than promoting itself to a write-stage.

## Orchestrator cwd preflight — `isolation: worktree` requires a git repo cwd

Before dispatching any subagent with `isolation: worktree` in its frontmatter, the orchestrator MUST be in a cwd that resolves to a git repo. If cwd is outside any git repo (e.g. `$HOME`, a temp dir), the Claude Code harness can't create a fresh worktree and silently degrades to either (a) reusing a stale sibling-agent worktree or (b) operating in the main tree — both of which **break the isolation guarantee** the pipeline's wave-gate semantics rely on.

**Preflight:** `cd "$OP_CONTEXT_REPO"` (or the relevant repo) before the first worktree-isolated dispatch in a session. Verify with `git rev-parse --show-toplevel` — error out if it returns nothing.

This matters specifically for in-session manual orchestration. The autoagent-* GitHub Actions runners (where applicable) always invoke from inside their repo, so the problem doesn't exist there.

## Bootstrap mode — when the pipeline taxonomy isn't wired yet

The pipeline uses labels (`stage:triaged`, `stage:ready-for-plan`, `auto:auto-fixable`, `sensitivity:staff-ok`, etc.) as wave-gate signals — each stage watches for its trigger label on an issue. **On first pipeline run in a repo that hasn't yet had sync-labels applied**, those labels don't exist; label-driven dispatch can't fire.

**Bootstrap-mode substitution:** each stage ends its completion comment with a short explicit sentence stating the next stage, e.g. `Advancing to STAGE 3 (plan).` The orchestrator watches for these sentences instead of labels. This is a first-run-only fallback — once sync-labels lands in a repo, the taxonomy is live and label-based dispatch resumes.

Stage playbooks don't individually repeat this rule; they inherit it from here. If a stage's completion output in the first run doesn't include an "Advancing to STAGE N" (or "Back to STAGE N") sentence, the wave is stalled — orchestrator should re-prompt or kick back.

Exit criteria: bootstrap mode ends when the first `sync-labels --apply` run completes in the repo. From that point the label taxonomy is the canonical signal; bootstrap-mode sentences are redundant.

## Subagent recovery — what to do when a write-stage agent times out mid-run

Write-stage subagents (`test-writer`, `implementer`, `desloppifier`) operate in their own worktrees at `$REPO/.claude/worktrees/agent-<id>/` and may time out or lose connection mid-run with uncommitted work sitting in that worktree. Observed once per pipeline run as of 2026-04-24.

**Recovery procedure** (orchestrator runs):

1. Check the subagent's worktree state: `cd $REPO/.claude/worktrees/agent-*/ && git status --short && git log --oneline -1`
2. If uncommitted work exists: inspect the diff (`git diff`), run any tests the agent would have run to verify it's functional, then commit from the orchestrator session using the bot identity:
   ```bash
   git add <enumerated files>
   git -c user.name="Original Gary" -c user.email="276612211+OpenGaryBot@users.noreply.github.com" \
     commit -m "<conventional prefix per pipeline-nevers>: <description>"
   git push origin <branch>
   ```

   **Always the bot identity. Never Sam's `stuckvgn` identity.** Per `~/.claude/rules/git-identity.md`. The temptation to "author under Sam so the non-last-pusher branch protection lets him self-approve" is the wrong direction — branch protection is by design, not a bug to route around. If a PR needs to merge but bot-authored commits block via non-last-pusher rule, file a `[process]` issue asking Sam to either approve or re-think the rule. Do not impersonate Sam's identity.
3. If the subagent left a stale LOCKED worktree after its branch pushed: `git worktree unlock <path>` then `git worktree remove -f <path>` to free the branch for subsequent stages.
4. Post a completion comment on the issue explaining the rescue (so adversarial and downstream stages have correct context).
5. Continue the pipeline from the next stage.

Do NOT re-dispatch the same subagent with a fresh invocation if uncommitted work exists — you'll lose it. Rescue first, re-dispatch only if the worktree is genuinely empty.

---

See `/pipeline-reference` for the full 14-stage sequence and per-stage agent assignments.
