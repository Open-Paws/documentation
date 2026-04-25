# Open Paws Tooling Reference

Loaded every session. Where the Open Paws dev stack lives and what each piece is for. Coverage scope: the integration points that don't have their own dedicated rule. For desloppify usage see `desloppify.md`; for emergency override labels and the pipeline rule set see `pipeline-nevers.md`; for the context repo and the org-wide read test see `context-repo.md`.

## MCP config file locations (three scopes)

Claude Code resolves MCP servers from three scopes, with **local > project > user** precedence (local wins on conflict):

- **Local scope** — per-project, private to this machine. Stored under the `projects` key of `~/.claude.json` (single file, project-keyed). Not committed, not shared.
- **Project scope** — per-project, shared with the team. Stored in `.mcp.json` at the project root. Committed to the repo.
- **User scope** — global across all projects for this user. Stored at the top level of `~/.claude.json`.

There is **no `~/.claude/.mcp.json`** — that path does not exist in the Claude Code resolution chain. If a script or doc references it, that reference is wrong.

## CodeRabbit

Per-repo wiring lives in `.coderabbit.yaml` at each repo root.

- Profile: `chill`
- Auto-review: enabled
- AST-grep packages sourced from `github.com/Open-Paws/shared-lint-rules`
- Learnings dashboard: `app.coderabbit.ai/learnings`

Learnings accumulate via natural-language replies on PR threads — **do not** override CodeRabbit by editing prompt configs. Reply in the thread; CodeRabbit incorporates the feedback into future reviews repo-wide.

## Persona QA library

`qa/personas/*.md` per repo. 33-archetype taxonomy organized into clusters: execution, leadership, product/eng, finance, movement, epistemic, and Indian-context. Each persona is a markdown file the `persona-qa` subagent absorbs before navigating the app via Playwright MCP.

## Browser control

**Playwright MCP only.** Any task that needs to drive a browser — persona QA runs, smoke-testing a deployed preview, scraping a site Claude needs to understand, automating a form submission — goes through the Playwright MCP server. `mcp__playwright__*` tools are wired globally and listed in `persona-qa` subagent `tools:` frontmatter; extend per-agent as needed.

**No BrowserOS, no raw Selenium, no headless-chromium subprocesses, no browser-agent frameworks layered on top.** BrowserOS was evaluated and dropped in April 2026 — kept the policy statement here so the decision doesn't get relitigated by a future session that finds the BrowserOS docs and wonders. If a Playwright MCP capability is missing for a real use case, surface that gap as an issue rather than reaching for another tool.

## Persona QA tracking

Per-repo, in `qa/persona-tests/`:

- `PROGRESS.md` — coverage matrix (which personas have run against which builds)
- `<persona>.md` — per-persona findings, first-person voice, written by the persona-qa subagent
- `SUMMARY.md` — cross-persona patterns rolled up across runs

Always-flagged findings route through `scout-playbook` for issue filing (STAGE 1 of the pipeline).

## Workflow set (per repo)

Open Paws repos MAY carry a set of GitHub Actions that map to pipeline stages:

```
autoagent-scout.yml
autoagent-triage.yml
autoagent-plan.yml
autoagent-check-plan.yml
autoagent-implement.yml
autoagent-review-pr.yml
autoagent-fix.yml
autoagent-adversarial.yml
autoagent-merger.yml
```

**Not universal.** The context repo, for instance, runs pipeline stages via in-session Claude orchestration (same dispatch model as the repo's existing `/weekly-review`, `/audit-all`, etc.) and does NOT carry these workflow files. Both models are valid: workflow-driven dispatch for repos that want CI-triggered pipeline runs, session-driven dispatch for repos where the orchestrator is a Claude Code session. Per-repo call; no one-size-fits-all.

Workflow edits are unsafe-parallel — serialize per `parallelization.md`.

## Per-repo Claude Code config

Each repo carries:

- `.claude/agents/` — repo-specific subagent definitions (overrides `~/.claude/agents/`)
- `.claude/skills/` — repo-specific skill packages (overrides `~/.claude/skills/`)

Standard skill set per repo: `scout-playbook`, `triage-playbook`, `planning-playbook`, `plan-review-playbook`, `test-writer-playbook`, `implementer-playbook`, `qa-persona-playbook`, `adversarial-playbook`, `desloppify-playbook`. Plus `pipeline-reference` for full stage detail on demand.

## Memory systems — two distinct stores

Claude Code has two unrelated memory systems. Don't conflate them.

**Cwd-scoped session memory** — `~/.claude/projects/<cwd-slug>/memory/MEMORY.md` plus topic files in the same dir. Claude's own learnings from sessions in a specific working directory. Auto-written by Claude during/between sessions. Only loads when cwd matches the slug. First 200 lines (or ~25 KB) of `MEMORY.md` plus referenced files load at session start.

**Subagent-scoped persistent memory** — `<cwd>/.claude/agent-memory/<agent-name>/` (cwd-relative, per-repo). Created when a subagent has `memory: project` in its frontmatter. Harness auto-creates the dir on first invocation and injects the path plus usage instructions into the subagent's system prompt. The subagent reads/writes its own `MEMORY.md` index plus topic files. **Scoped per-repo (one set of accumulated learnings per repo the subagent works in)**, not global per-subagent.

Observed 2026-04-24: when orchestrator cwd is a git repo (e.g. `$OP_CONTEXT_REPO`), the harness places agent memory INSIDE that repo's working tree. When cwd is home dir, the harness fails to create worktrees at all (see `parallelization.md` §Orchestrator cwd preflight). The earlier doc claim that memory lives at `~/.claude/agent-memory/` "regardless of cwd" was incorrect.

**Gitignore implication:** every Open Paws repo's `.gitignore` must include `.claude/agent-memory/` and `.claude/worktrees/` to prevent accidental commit of agent state into shared history. This is a standard per-repo hygiene convention — should ship with the repo's initial `.gitignore` and be part of the sync-labels-style tooling bootstrap.

Different purposes:
- Session memory = Claude's notes to **future-Claude-in-this-dir**
- Agent memory = Subagent's notes to **future-invocations-of-itself**

Subagents with `memory: project` in this setup: `scout`, `triage`, `planner`, `plan-reviewer`, `adversarial`, `persona-qa`. Stages where pattern recognition across many runs helps; not enabled on `test-writer` / `test-reviewer` / `implementer` / `verifier` / `desloppifier` because those should be driven by current plan + current code, not accumulated preference.

## Context repo

`github.com/Open-Paws/context` (env var: `$OP_CONTEXT_REPO`). Single source of truth for WHY (decisions, priorities, org overview, proposals). This stack is HOW. Context repo wins conflicts. Org-wide read safety rules in `context-repo.md`. Key files inside the repo:

- `decisions.md` — settled constraints
- `priorities.md` — current frame
- `proposals/` — in-flight strategic decisions
- `stakeholders.md`, `org-overview.md`, `handbook/` — onboarding and routing material
