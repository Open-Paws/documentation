# Git Commit Identity

Loaded every session. Governs the author identity on every commit, push, and PR the pipeline creates.

## The Rule

**Commits are authored by the GitHub account `gh auth status` shows as active — not by Sam.** Current active account: `OpenGaryBot` (id `276612211`). Sam's personal account `stuckvgn` has been logged out of every auth surface on this machine (gh keyring, Git Credential Manager, Windows Credential Manager, git config, env vars) and must not reappear as a commit author, committer, or co-author.

Sam is moving to bot-driven automation. Authorship is part of the contract — audit trails, signed-commit policies, branch-protection rules, and future revocation all assume the bot identity is stable and distinct from his.

## Required git config

Before any commit in any repo, `user.name` and `user.email` must match the active gh account. Use the privacy-preserving noreply email — not a personal address — so the commits expose no extra PII and verify as "from GitHub" on the profile.

```
Name:  Original Gary
Email: 276612211+OpenGaryBot@users.noreply.github.com
```

Set globally once:

```bash
git config --global user.name "Original Gary"
git config --global user.email "276612211+OpenGaryBot@users.noreply.github.com"
```

Or per-repo on first touch (same two lines without `--global`). If the bot identity ever rotates, re-derive from `gh api user -q '.id, .name, .login'` — never hardcode without cross-checking.

## Never under Sam's identity

- Never `git commit --author="Sam ..."`.
- Never set `user.email` to `sam@openpaws.ai` for automation commits.
- Never add `Co-Authored-By: Sam Tucker-Davis` or similar trailers.
- Never push with credentials belonging to `stuckvgn` even if they briefly reappear in some credential store.

If Sam genuinely needs to be the committer — e.g. a hand-authored fix he asks you to land verbatim — he'll say so explicitly. Default is always the bot.

## Tool-attribution is a separate concern

AI-tool attribution (`Co-Authored-By: Claude`, "Generated with Claude Code" footers) is governed by `~/.claude/settings.json`:

```json
"attribution": { "commit": "", "pr": "" }
```

Already neutralized — don't re-enable. For non-Open-Paws repos, `external-contribution-safety.md` adds the additional layer of suppressing all tool and org identity.

## Pre-push verification

Before pushing, confirm:

```bash
git log -1 --format='%an <%ae>'
```

Must return `Original Gary <276612211+OpenGaryBot@users.noreply.github.com>`. Anything else — abort the push, fix the config, amend or reset the offending commits, then retry.
