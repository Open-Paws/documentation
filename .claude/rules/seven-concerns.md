# Seven Concerns — Canonical Index

Loaded every session. Every change — code, docs, config — must address all seven concerns. This file is the canonical index. Each concern points to the rule file with the full text.

## The Seven

1. **Testing** — assertion quality over coverage percentage; every test must fail when the behavior it covers breaks. RED-first TDD, mutation-resistant assertions, error-path coverage.
   Full rule: [`testing.md`](testing.md). Path-scoped — fires on test files. Skill: `/testing-review`.

2. **Security** — three-adversary threat model (state surveillance, industry infiltration, AI model bias), zero-retention APIs, supply chain verification, encrypted local storage, device seizure preparation, instruction-file integrity, MCP server isolation.
   Full rule: [`security.md`](security.md). Path-scoped — fires on auth/crypto/encrypt/security paths. Skill: `/security-review`. Org tiers: `$OP_CONTEXT_REPO/handbook/security.md`.

3. **Privacy** — activist identity protection, real deletion (not soft-delete), whistleblower protection, coalition data sharing under strictest partner's policy, consent verification.
   Full rule: [`privacy.md`](privacy.md). Path-scoped — fires on data-model, profile, PII, consent paths. Skill: `/privacy-review`.

4. **Cost optimization** — model routing (Haiku for cheap, Sonnet for mid, Opus for hard), token budget discipline, prompt cache discipline (target 80%+ hits), 40/30/20/10 budget split, vendor abstraction, self-hosted fallback.
   Full rule: [`cost-optimization.md`](cost-optimization.md). Always-loaded.

5. **Advocacy domain** — ubiquitous language (Campaign / Investigation / Coalition / Witness / Sanctuary / Rescue / Liberation / Direct Action / Undercover Operation / Ag-Gag / Factory Farm / Slaughterhouse / Companion Animal / Farmed Animal / Evidence), bounded contexts (Investigation Operations / Public Campaigns / Coalition Coordination / Legal Defense), explicit anti-corruption layers between contexts, no speciesist idioms.
   Full rule: [`advocacy-domain.md`](advocacy-domain.md). Always-loaded.

6. **Accessibility** — internationalization (CLDR plural categories, RTL, lang+dir attributes), low-bandwidth defaults, offline-first capability, low-literacy design, screen-reader correctness, keyboard navigation.
   Full rule: [`accessibility.md`](accessibility.md). Path-scoped — fires on UI / frontend / i18n / l10n paths. Skill: `/accessibility-review`.

7. **Emotional safety** — progressive disclosure of traumatic content, configurable detail levels, content warnings, burnout prevention for moderators and investigators, opt-in graphic content, secondary trauma awareness.
   Full rule: [`emotional-safety.md`](emotional-safety.md). Path-scoped — fires on traumatic-content / display / upload / moderation paths. Skill: `/emotional-safety-review`.

## How To Apply

- At plan time, the planner explicitly addresses each of the seven against the proposed change.
- At review time, the reviewer checks each of the seven was actually addressed (not just acknowledged).
- For path-scoped concerns (1, 2, 3, 6, 7), the corresponding rule file auto-loads when matching files are touched. The concern still applies even when the rule file doesn't auto-load — the concern is universal; the rule file is a deeper reference.
- For the always-loaded concerns (4, 5), the rule file content is in context every session.

## Cross-References

- Org-wide tier definitions and threat model: `$OP_CONTEXT_REPO/handbook/security.md`
- Context repo's view of the seven concerns: `$OP_CONTEXT_REPO/AGENTS.md` §Seven Concerns
- Pipeline stage where each concern is checked: `/pipeline-reference`
