# External Contribution Safety

When helping a developer contribute to a third-party repository — any repo whose git remote does not belong to your organization — the AI tool must suppress all identity, attribution, and advocacy framing from commits, pull requests, and branches. This is last-line-of-defense enforcement: configure your tool to disable attribution trailers as the first line, and rely on these instructions when configuration alone is insufficient.

## Repo Classification

Before producing any commit message, PR description, or branch name, check the remote:

```bash
git remote get-url origin
```

- If the remote matches `github.com/Open-Paws/` or `github.com:Open-Paws/` (SSH), you are in an **internal repo** — full identity is fine.
- If the remote is anything else, treat it as **external** — apply all rules below.
- If there is no remote configured, treat it as **external** (safe default).

Confidence must be positive that you are in an internal repo. Absence of a matching remote is not ambiguous — it defaults to external mode.

## What to Suppress in External Repos

**Tool identity** — Do not mention Claude Code, Cursor, Copilot, Windsurf, Cline, or any other AI coding tool in commits, PR descriptions, branch names, or code comments.

**Organization** — Do not mention Open Paws, any Open Paws project name, or any advocacy organization in any contribution artifact.

**Mission framing** — Do not use advocacy language (liberation, compassionate, animal rights, plant-based, vegan) in code, commits, PR titles, or PR bodies unless the target project is explicitly about those topics.

**Attribution trailers** — No `Co-Authored-By: Claude` lines. No "Generated with Claude Code" footers. No AI attribution in commit messages or PR descriptions.

## Commit Message Rules in External Repos

Read the last five commits in the target repo before writing a commit message:

```bash
git log --oneline -5
```

Match their exact style: length, format (conventional vs prose vs terse), tense, specificity. Then apply these constraints:

- **Proportional to diff** — a one-line change gets a one-line commit. A 50-line change gets two or three sentences maximum.
- **Imperative mood** — "Fix", "Add", "Update", not "Fixed", "Added", "Updated".
- **No AI-generated patterns** — reject "This commit introduces...", "Updated X to support Y", "Added support for Z", "Improve overall...".
- **Self-check** — Read the message. Does it look indistinguishable from the existing commits in this repo? If not, shorten it and remove any summary language.

## PR Description Rules in External Repos

Check how merged PRs are described in the target repo before writing a PR body:

```bash
gh pr list --state merged --limit 5
```

Then apply:

- **Match the target repo's style** — if merged PRs are two sentences, write two sentences. If they use headers, use headers. If they use none, use none.
- **No section headers for small changes** — omit `##`, `###`, "Summary:", "Motivation:", "Background:", "Approach:" unless the target repo uses them.
- **No bullet lists of benefits** — a list of what this improves is an AI tell. One explanation of what changed and why is sufficient.
- **Length** — most good external PRs are one to three sentences. Longer is rarely better.
- **Self-check** — read it aloud. Does it sound like a developer who works on this codebase, without a toolkit or an agenda? If not, cut it by half.

## Branch Naming in External Repos

Check existing open PRs for branch naming conventions:

```bash
gh pr list --state open --limit 10
```

Use that convention. If no clear pattern exists, default to `fix/short-description` or `add/short-description`. Keep the branch name under 40 characters. Do not include advocacy language, org identifiers, or tool names in branch names.

## Defense-in-Depth Principle

These instructions are the last line of defense, not the first. Before contributing to any external repo, configure your tool to disable attribution trailers:

- Claude Code: set `"attribution": { "commit": "", "pr": "" }` in `~/.claude/settings.json` (deprecates `includeCoAuthoredBy`; already wired on this machine)
- Cursor: disable "Add AI attribution" in settings
- Copilot: no attribution trailers are inserted by default in commit flows

Git author identity (who shows up in `git log`, not the AI-tool trailer) is governed separately by `git-identity.md` — external repos inherit the same bot identity by default. If a target repo requires a different author for some reason, raise it before committing; do not quietly switch.

Instructions to the AI are what you rely on when tool configuration fails or when the tool generates surrounding prose (PR descriptions, branch names) that configuration does not control.
