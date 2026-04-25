# Open Paws Pipeline — Hard Nevers

Loaded every session (no `paths:` frontmatter). Contains the non-negotiable rules that apply to every agent, every stage, every commit, every push, in every Open Paws repo. Full 14-stage pipeline reference lives in `/pipeline-reference` skill — invoke it on demand when you need stage detail.

## The Hard Nevers

- **Never skip the pipeline.** No "this is a tiny fix, let's go direct to PR." Tiny fixes are the highest-risk class because nobody scrutinizes them.
- **Never fix without an issue first.** File the issue, even for "while I'm here" cleanups. No issue = no work.
- **Never use wildcard `git add`.** `git add .` / `git add -A` / `git add --all` / `git add *` collide with other agents' owned file sets and silently commit out-of-scope files. Enumerate owned files explicitly. (Enforced by the `~/.claude/hooks/block-dangerous-bash.sh` PreToolUse hook.)
- **Never write production code before tests.** RED → GREEN. Tests must be mutation-resistant.
- **Never let the desloppify strict score drop.** Score regression requires a human override label (`override:allow-score-drop`) that agents cannot apply.
- **Never use upstream desloppify.** Always `github.com/Open-Paws/desloppify` — the fork has movement conventions (no-speciesist-language, type-safety, gateway response discipline) the upstream lacks.
- **Never scope-creep mid-wave.** Adjacent bugs, refactor opportunities, test gaps → file new issues. Do not fold into the current change.
- **Never modify a test file during the implementation wave.** Test files are locked; forcing a test green is a pipeline-integrity violation.
- **Never touch files outside your declared owned set.** Subagents enforce this via `isolation: worktree` (test-writer, implementer, desloppifier); other stages enforce by discipline.
- **Never merge without adversarial clearance.** STAGE 13 runs LAST, AFTER CI+CodeQL+CodeRabbit+review subagents all green. Only a clean adversarial pass unblocks merge.
- **Never silently resolve an open decision.** If `$OP_CONTEXT_REPO/decisions.md` or `$OP_CONTEXT_REPO/proposals/` touches the proposed change, file a `[decision]` issue and block. Plans that pick a design choice without surfacing alternatives commit the org to that path invisibly.
- **Never contradict a settled decision.** `decisions.md` entries are constraints. Relitigating openly is expensive; relitigating silently via implementation is worse. File a reconsideration issue instead.
- **Never merge to `main` directly.** Branch, PR, review, merge.
- **Never share a branch between parallel agents.** Each write-stage agent runs in its own worktree.

## Conventional Commits (required)

Prefix every commit: `feat:` `fix:` `refactor:` `test:` `docs:` `chore:` `perf:` `ci:`. Scope matches the component label on the issue. Description is direct and real — per the voice rules in `~/.claude/CLAUDE.md`, profanity is fine when it fits; corporate filler is not.

## Close Issues Via PR Body

`Closes #N` in the PR body links the PR to the issue. Squash merge by default; the issue auto-closes when the PR merges.

## Emergency Overrides (human-only)

- `override:skip-adversarial` — requires human comment with justification
- `override:allow-score-drop` — requires human comment with numeric delta

Agents cannot apply these labels. If an agent thinks an override is warranted, file a comment requesting human intervention and halt.

## Context-Repo Primacy

If context-repo guidance (`$OP_CONTEXT_REPO/decisions.md`, `priorities.md`, `org-overview.md`) conflicts with any rule or playbook, context-repo wins. This stack is HOW; the context repo is WHY.

---

For the full 14-stage pipeline with wave-gate semantics, file-ownership rules, subagent definitions, and per-stage workflow: `/pipeline-reference`.
