# Advocacy Domain Language and Bounded Contexts

This is the domain language reference for animal advocacy software. AI agents drift from domain terminology toward generic synonyms — "order" instead of "campaign," "user" instead of "activist," "report" instead of "investigation." Language drift in advocacy software is not a style issue; it causes miscommunication across coalition partners who rely on precise terminology, and it obscures the legal and ethical distinctions between different types of operations.

## Ubiquitous Language Dictionary

Use these terms consistently in code, documentation, conversations, and AI prompts. NEVER introduce synonyms.

- **Campaign** — An organized effort to achieve a specific advocacy goal (legislative change, corporate policy reform, public awareness). Has defined start, milestones, and success criteria.
- **Investigation** — Covert documentation of animal exploitation conditions. Legally sensitive. All data classified as potential evidence. Distinguished from "research" or "reporting."
- **Coalition** — A formal or informal alliance of multiple organizations working toward a shared goal. Each member has its own risk profile, data policies, and operational boundaries.
- **Witness** — A person who provides testimony about animal exploitation conditions. May be an investigator, a whistleblower, or a bystander. Identity requires maximum protection.
- **Testimony** — A witness's account of observed conditions. Subject to consent verification before any use or display.
- **Sanctuary** — A facility providing permanent care for rescued animals. Distinguished from "shelter" (temporary) or "foster" (individual-based).
- **Rescue** — The act of removing animals from exploitative conditions. May have distinct legal status depending on jurisdiction.
- **Liberation** — Direct action to free animals. Carries specific legal implications distinct from "rescue."
- **Direct Action** — Physical intervention in animal exploitation. Legally distinct from campaigning, lobbying, or public education.
- **Undercover Operation** — An investigation conducted by an operative embedded within an exploitative facility. Highest legal risk category.
- **Ag-Gag** — Laws criminalizing undercover investigation of agricultural operations. Determines legal exposure for investigation data.
- **Factory Farm** — Industrial animal agriculture facility. Use this term, not euphemisms like "farm" or "production facility."
- **Slaughterhouse** — Facility where animals are killed for commercial purposes. Use this term precisely.
- **Companion Animal** — Animals kept primarily for companionship (dogs, cats). Distinct legal and ethical framework from farmed animals.
- **Farmed Animal** — Animals raised for food, fiber, or other commercial products. Distinguished from "livestock" (industry framing).
- **Evidence** — Documentation (footage, records, testimony) of animal exploitation conditions with potential legal use.

## Bounded Contexts

These are DIFFERENT domains with different models, different rules, and different security requirements. Do not merge them. Do not allow data to flow between them without explicit anti-corruption layers.

**Investigation Operations** — Covert data collection, evidence management, investigator identity protection, chain of custody. Highest security classification. Data never flows outward without explicit declassification. Entities: Investigator, Operation, Evidence, Facility, ChainOfCustody.

**Public Campaigns** — Public-facing advocacy actions, supporter engagement, media relations, petition management. Lower security requirements but high visibility. An "activist" in a public campaign is a fundamentally different entity than an "investigator" in an undercover operation — different data model, different risk profile, different access controls. Entities: Campaign, Supporter, Action, Petition, MediaAsset.

**Coalition Coordination** — Multi-organization planning, shared resource management, joint strategy development. Data crosses organizational boundaries and must be governed by the strictest partner's policies. Entities: Coalition, PartnerOrganization, SharedResource, JointAction, DataSharingAgreement.

**Legal Defense** — Legal case management, attorney-client privileged communications, court filings, expert witness coordination. Attorney-client privilege imposes its own data handling requirements that override other policies. Entities: Case, Attorney, Defendant, Filing, PrivilegedCommunication.

## Anti-Corruption Layers

When data must cross bounded context boundaries, use explicit translation layers. An investigation's raw evidence becomes a "media asset" in a public campaign only through a deliberate transformation that strips operational metadata. A coalition partner's shared intelligence becomes an investigation lead only through a documented intake process. NEVER allow direct imports between contexts — AI agents will optimize for expedience and import directly rather than translating through the anti-corruption layer. Each boundary crossing must be auditable.

## Entity Definitions

Core entities shared across the domain (each context may have its own projections):
- **Activist** — A person engaged in advocacy work. Base identity entity.
- **Organization** — An advocacy group. Has its own policies, risk profile, and operational scope.
- **Campaign** — A coordinated advocacy effort with defined goals.
- **Investigation** — A covert documentation operation.
- **Witness** — A person providing testimony. Identity is maximally protected.
- **Evidence** — Documented proof of exploitation conditions.
- **Coalition** — An alliance of organizations.
- **Sanctuary** — A permanent animal care facility.


## Compassionate Language — No Speciesist Idioms

Code, documentation, comments, commit messages, and variable names must use compassionate, non-speciesist language. AI agents frequently generate common English idioms that normalize animal violence — reject these and use alternatives.

**Prohibited patterns** (with alternatives):
- "kill two birds with one stone" → "accomplish two things at once"
- "beat a dead horse" → "belabor the point"
- "more than one way to skin a cat" → "more than one way to solve this"
- "let the cat out of the bag" → "reveal the secret"
- "like shooting fish in a barrel" → "effortless task"
- "guinea pig" (as test subject) → "test subject" or "early adopter"
- "open a can of worms" → "open a difficult topic"
- "wild goose chase" → "futile search"
- "cattle vs. pets" → "ephemeral vs. persistent"
- "pet project" → "side project"
- "master/slave" → "primary/replica"
- "whitelist/blacklist" → "allowlist/denylist"
- "grandfathered" → "legacy"

This is not exhaustive — 60+ patterns are enforced by the Open Paws no-animal-violence tooling ecosystem:
- Semgrep rules: https://github.com/Open-Paws/semgrep-rules-no-animal-violence
- ESLint plugin: https://github.com/Open-Paws/eslint-plugin-no-animal-violence
- Vale style rules: https://github.com/Open-Paws/vale-no-animal-violence
- Pre-commit hook: https://github.com/Open-Paws/no-animal-violence-pre-commit
- GitHub Action: https://github.com/Open-Paws/no-animal-violence-action
- VS Code extension: https://github.com/Open-Paws/vscode-no-animal-violence
- Reviewdog runner: https://github.com/Open-Paws/reviewdog-no-animal-violence

Core rule definitions: https://github.com/Open-Paws/no-animal-violence
