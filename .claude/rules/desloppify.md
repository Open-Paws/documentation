# Code Quality — desloppify

Run desloppify to systematically identify and fix code quality issues. Install from the **Open Paws fork** (Python 3.11+):

```bash
# Install from this fork — NEVER from PyPI / upstream
pip install "git+https://github.com/Open-Paws/desloppify.git#egg=desloppify[full]"
desloppify update-skill claude
```

**Canonical install command.** This file is the single source of truth for how to install desloppify. Every other file in this stack (`skills/desloppify-playbook/SKILL.md`, `agents/desloppifier.md`, `$OP_CONTEXT_REPO/.claude/rules/desloppify.md`, every `Open-Paws/*/.claude/rules/desloppify.md`) links here rather than restating the command — duplication is how the multi-repo drift that existed pre-2026-04-25 happened. If you're editing an install command anywhere other than this file, stop.

**OP fork only — never upstream.** The git install above pulls from `github.com/Open-Paws/desloppify`, which carries the movement conventions (no-speciesist-language rules, type-safety patterns, gateway response shape discipline, compassionate language enforcement, persona-QA browser testing) that upstream desloppify lacks. `pip install desloppify` from PyPI pulls upstream and is a hard-rule violation per `~/.claude/rules/pipeline-nevers.md`.

Add `.desloppify/` to `.gitignore` — it contains local state that should not be committed. Before scanning, exclude generated / vendor / build dirs:

```bash
# Exclude generated directories, then scan
desloppify exclude node_modules dist
desloppify scan --path .
```

`--path` is the directory to scan (use `.` for the whole project, or a subdirectory like `src/`). Your goal is to get the strict score as high as possible. The scoring resists gaming — the only way to improve it is to actually make the code better.

## The fix loop

Run `next` → fix → resolve → repeat:

```bash
desloppify next          # get the top-priority item; shows which file and the resolve command
# fix the code
desloppify plan resolve  # mark it done
desloppify next          # get the next item
```

It is the execution queue from the living plan, not the whole backlog. It tells you what to fix now, which file, and the resolve command to run when done. Use `desloppify backlog` only when you need to inspect broader open work not currently driving execution.

Do not be lazy. Large refactors and small detailed fixes — do both with equal energy. No task is too big or too small. Fix things properly, not minimally.

Use `plan` / `plan queue` to reorder priorities or cluster related issues. Rescan periodically. The scan output includes agent instructions — follow them, do not substitute your own analysis.

## Persona-QA workflow (UI repos with persona-driven testing)

```bash
desloppify persona-qa --prepare --url https://example.com   # generate agent instructions
# agent runs browser testing and captures findings in JSON
desloppify persona-qa --import findings.json                 # merge into state
desloppify persona-qa --status                               # per-persona summary
desloppify next                                              # persona QA items now appear in the queue
```

## Baseline Capture Process

**At plan time (STAGE 3):** Capture desloppify baseline against branch point:

```bash
desloppify status --json > .desloppify/baseline.json
```

Post baseline JSON as GitHub issue comment for durable storage (`.desloppify/` is gitignored).

**Recovery if missing:** STAGE 9 uses `git merge-base HEAD main` to recapture against the branch point.

**Score-cannot-regress gate.** STAGE 9 blocks merge if the strict score drops below baseline. Regression requires `override:allow-score-drop` label (human-only — agents cannot apply it).
