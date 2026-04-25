# Research Methodology — Cross-Check Before Acting

Loaded every session. The epistemic rule the overnight optimization loop established the hard way: **single-source claims about Claude Code config field formats or supported values must be cross-checked against concrete examples before they're acted on.**

## Canonical sources for Claude Code config

The authoritative source for Claude Code field names, formats, hook event names, and frontmatter keys is the Anthropic-maintained `plugin-dev` skill bundled with the official plugins marketplace, plus the official docs at `docs.anthropic.com/claude-code/`. Concrete reference material:

- `~/.claude/plugins/marketplaces/claude-plugins-official/plugins/plugin-dev/references/frontmatter-reference.md` — definitive field reference for agents/skills/commands
- The same plugin's `SKILL.md` and per-component example files — concrete worked examples
- `~/.claude/cache/changelog.md` — authoritative event/feature additions per release

## Cross-check rule

When a subagent (including `claude-code-guide`) quotes a field name, format, or supported-value list, **do not act on the quote alone.** Open the canonical reference and confirm against a concrete example. If the two disagree, concrete examples in `plugin-dev` win over prose descriptions anywhere else.

**Precedent:** the overnight loop hit this exact failure twice in one session.

- **Iteration 2** — subagent reported `allowed-tools:` as space-separated. Wrong. Concrete examples in `plugin-dev/SKILL.md` show comma-separated. Caught at iteration 8 only after a full re-audit. Six agents had to be re-fixed.
- **Iteration 6** — subagent reported `PreCompact` / `WorktreeCreate` / `InstructionsLoaded` hooks as nonexistent. Wrong. All documented in `~/.claude/cache/changelog.md`. Caught same iteration only by cross-checking the changelog directly.
- **Task 5 of the 2026-04-24 consolidation session** — subagent docs we fetched at the time did not list `memory: project` as a supported subagent frontmatter field. Concrete test confirmed it was fully functional: the harness auto-creates per-agent directories at `~/.claude/agent-memory/<agent>/` and injects the path + usage instructions into the subagent's system prompt. **Docs have since caught up — `memory: project` is now first-party documented in `sub-agents.md` (lines 250 + 423-427), so the "undocumented but real" status no longer applies.** The underlying lesson still stands: **absence of documentation ≠ absence of feature. Test concretely before treating something as unsupported.**

Three failures, three different shapes — prose-vs-format, prose-vs-event-list, docs-silent-but-feature-real. The fix is the same in all directions: **never let a research subagent's prose summary be the last word on a Claude Code config detail.**

## Framework research

When surveying external frameworks (metaswarm, barkain, VoltAgent, wshobson, vanzan01, etc.), treat them as **idea sources, not authority sources.** Their patterns may inspire adoptions, but their specific syntax, field names, or hook conventions are not authoritative for our setup. If a framework demonstrates a pattern we want to adopt, the implementation must be re-derived against canonical Claude Code references — not copy-pasted from the framework.

## Tie-breaker

When two sources disagree on a Claude Code detail:

1. Concrete worked example in `plugin-dev/` wins over
2. Prose description in official docs, which wins over
3. Subagent summary, which wins over
4. Framework convention, which wins over
5. Memory of how it worked in a prior session

Always read down the chain when uncertain. The cost of one extra read is far below the cost of fanning out wrong config to 40+ repos.

## Token cost — measure live, don't estimate

Byte-based estimates of always-loaded token cost are unreliable for markdown-heavy rule files. The overnight loop's `bytes ÷ 4` proxy was off by 2× — real ratio is closer to `bytes ÷ 2.5` for this content (short lines, code spans, tables, YAML frontmatter). **Always use live `/context` output as the source of truth** for always-loaded token cost. Use the proxy for relative comparisons within a single session if you must, but never quote it as an absolute number.

**Concrete numbers, measured post-overnight-loop on 2026-04-24:**

| Metric | Value |
|---|---:|
| Always-loaded bytes (rules + CLAUDE.md + skill descriptions) | 41,805 |
| `bytes ÷ 4` proxy | ~10,450 tokens (wrong, under by ~2×) |
| `bytes ÷ 2.5` corrected ratio | ~16,700 tokens (closer) |
| Real `/context` measurement | ~20,700 tokens (authoritative) |

The corrected ratio (`÷ 2.5`) is still a proxy and still underestimates real cost because it doesn't account for the full prompt-frame overhead Claude Code injects (tool schemas, hook definitions, skill metadata, system reminders). The real measurement is always larger than any byte-based estimate. Treat corrected-ratio as an upper bound on "what the rules files themselves cost" and `/context` as the bound on "what actually loads."

## Known optimization candidates (deferred)

These four were evaluated for context-cost reduction in the 2026-04-24 consolidation session and **deliberately deferred**. Don't re-evaluate from scratch; the reasoning below stands until either the always-loaded budget actually pinches or the underlying assumption changes.

- **`rules/README.md` (1.7k tokens)** — index/metadata for the rules dir. Tempting to trim. Deferred because agents reading the rules dir benefit from the index too, not just humans; first on the cut list if real context pressure appears.
- **`CLAUDE.md` (2.8k tokens)** — already minimized in the consolidation. Identity layer needs to stay discoverable. Don't touch.
- **`rules/advocacy-domain.md` (2.6k tokens) — path-scoping** — tempting to scope to `**/*.md`/copy/i18n paths. Deferred because the ubiquitous-language dictionary and speciesist-idiom list must fire on any content generation, including user-facing strings inside `.ts`/`.tsx`. False negatives cost more than 2.6k tokens.
- **`rules/context-repo.md` (2.3k tokens) — path-scoping** — tempting to scope to `**/Open-Paws/context/**`. Deferred because the open-decision escalation rule applies across all Open Paws pipeline work, not just work inside the context repo dir. Path-scoping would silently break the escalation rule for adjacent work.
