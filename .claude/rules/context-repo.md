# Context Repo — Org-Wide Read Safety

Activate this rule when working on `github.com/Open-Paws/context` or proposing any change that may be merged into it. The context repo is the org's single source of truth, readable by every staff member and every AI agent across the Open Paws ecosystem. That reach is what makes it valuable — and what makes misclassification costly. Material that leaks into the context repo propagates to every future agent session across the org, with no clean retraction.

## The Org-Wide Read Test

Before proposing, writing, reviewing, or merging any content in the context repo, ask:

> Imagine this change merged. Every staff member including brand-new contractors, every intern in the May cohort, every AI agent in every repo, every persona QA agent, every scheduled Claude Code task — all of them can now read this. Is that okay?

If not clearly okay, the content does not belong in the context repo. Redirect to a private location. There is no "technically private but in the context repo" category.

## What Must Not Go In

Non-exhaustive. Reject at plan review and redirect:

- Individual personal information — salaries, compensation, health, neurodivergence disclosures, recovery status, family, relationships
- HR / performance matters — performance feedback, interpersonal conflicts, hiring/firing discussions, contract negotiations with specific people
- Active sensitive funder dynamics — criticism from specific funders, donor red flags, active grant negotiation positions, internal funder assessments
- Legal matters in progress — contract disputes, IP issues, regulatory inquiries, anything a lawyer is or should be involved in
- Unannounced partnerships or programs — anything whose premature leak would damage. Once announced, it can move in
- Security-sensitive operational details — threat models, unpatched vulnerabilities, defense-in-depth specifics that would help an attacker
- Credentials, secrets, API keys — never, regardless of perceived convenience
- Personal context about individuals beyond what they've publicly chosen to share — history, politics, family situation, country-of-origin dynamics
- Active campaign intelligence that could tip off opposition — specific corporate targets mid-campaign, undercover operation plans, timing-sensitive material
- Sam's personal notes, journal, or private decision-making — belongs in the personal workspace repo
- Anything treated as confidential in its originating context — Slack DMs, CryptPad docs, private channels — even if innocuous out of context

Rule of thumb: gossip, personnel, legal, or "don't forward this email" — not context-repo material.

## What Belongs In

- Org identity, mission, public frame
- Settled decisions, after they're settled and shareable
- Current priorities every staff member should be aligned on
- Program structures, playbooks, frameworks, methodology
- Technical conventions and architecture principles that apply across repos
- Published or ready-to-publish work
- Glossaries, onboarding material, routing tables
- Links out to where more detail lives
- Decision *outcomes* and *rationale*, without embedding the sensitive discussion that produced them

Test: a fresh contractor or new agent session reading cold should get (a) valuable context and (b) see nothing that would make a staff member uncomfortable. Both must be yes.

## Where Sensitive Material Actually Lives

| Material | Correct location |
|---|---|
| Personal notes, journal, draft strategic thinking | Sam's personal workspace repo |
| Individual grant tracker, funder contact details, internal funder assessments | `private/grants/` in personal repo, or locked CryptPad |
| HR / people matters | Outside version control — direct comms, locked docs, legal counsel where appropriate |
| Active campaign intelligence | Per-campaign private repos or CryptPad with explicit access list |
| Credentials | Password manager / secrets manager |
| Early-stage partnership discussions | DM or locked doc until announced; summary can move in post-announcement |
| Staff-specific feedback | 1:1 docs outside the context repo |

If no private home exists for the rejected material, file a separate issue to establish one — in the personal workspace or ops repo, not the context repo.

## Pipeline Additions For Context-Repo Changes

**STAGE 2 (Triage) — classify every issue with a `sensitivity:` label:**

- `sensitivity:public-ok` — already public or trivially shareable
- `sensitivity:staff-ok` — fine for all staff + all agents (the bar for this repo)
- `sensitivity:private` — belongs elsewhere; redirect, do not advance

Issues labeled `sensitivity:private` never enter the plan wave.

**STAGE 4 (Plan Review) — run the org-wide read test explicitly.** If not clearly okay, reject with guidance on what to strip or redirect.

**STAGE 13 (Adversarial) — add a 7th check:**

7. **Confidentiality leak** — does this merge expose any individual's personal information, any sensitive relationship dynamic, any active negotiation, any unannounced plan, or any material that originated in a confidential context? If yes, `major+` severity, back to fix loop.

Adversarial patterns to flag (content that "seems fine" but leaks in context):

- Closed decision citing a specific funder's objection as the reason (strip identity, keep principle)
- Priority justified by "we lost trust with X partner" (strip dynamic, keep strategic implication)
- Program doc listing specific individuals as "struggling" or "not meeting expectations"
- Playbook referencing active campaign targets by name before launch

## Default Direction Is Out, Not In

When uncertain, keep it out. Context flows from private to public, not back. Once merged, downstream agents have already consumed it.

If material genuinely belongs but currently leaks something, **rewrite at a higher level of abstraction** — keep the principle, strip the specifics.

- Good: "Decision: prefer multi-year unrestricted funding over single-year restricted"
- Bad: "Decision: avoid Funder X because they demanded reporting we found unreasonable"

Same principle, different safety profile.

## Decision Tree

```
Is every fact in this change something I'd say out loud in an all-staff meeting
with interns, contractors, and partner org reps present?
│
├── YES → proceed through normal pipeline
│
└── NO → which category?
    │
    ├── Personal / HR / relationship → personal repo or external tool
    ├── Active sensitive operation → locked CryptPad with access list
    ├── Legal / contractual → outside version control, with counsel
    ├── Credentials → secrets manager
    ├── Sensitive but abstractable → rewrite at higher level, retry pipeline
    └── Unclear → ask Sam before filing the issue
```

Misclassification is costly in both directions. Too restrictive and the repo becomes useless. Too loose and it leaks. When genuinely unsure, ping rather than guess.
