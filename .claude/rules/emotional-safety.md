---
paths:
  - "**/content/**"
  - "**/media/**"
  - "**/display/**"
  - "**/upload/**"
---
# Emotional Safety Rules for Animal Advocacy Projects

Animal advocacy software routinely handles content documenting extreme suffering: factory farm conditions, slaughterhouse footage, animal testing documentation, and witness testimony of abuse. This content is necessary for the movement's work — it drives investigations, legal cases, public campaigns, and policy change. But uncontrolled exposure to this content causes measurable psychological harm. Every display decision must balance the operational need for access against the human cost of exposure. Emotional safety is not a UX preference — it is a duty of care to the people doing this work.

## Progressive Disclosure of Traumatic Content

NEVER display graphic content by default. Every piece of investigation footage, slaughter documentation, or exploitation imagery must be behind at least one intentional interaction. The default state is always safe: blurred, hidden, or represented by a text description. Users escalate to more graphic content through deliberate choices, never through automatic loading, scrolling, or navigation.

## Configurable Detail Levels

Implement user-controlled detail settings that persist across sessions. At minimum, provide three tiers: (1) text-only descriptions with no imagery, (2) blurred or low-detail representations with contextual descriptions, (3) full-resolution content. Each user chooses their own default level. The system MUST remember this preference and never reset it. Different roles need different defaults: a legal reviewer may need full-resolution evidence access; a campaign coordinator may only need text summaries.

## Content Warnings — Mandatory Before Display

Every piece of content involving animal suffering, investigation footage, or slaughter documentation MUST be preceded by a specific content warning describing what the content contains. Generic warnings like "sensitive content" are insufficient — the warning must indicate whether the content includes: graphic injury, death, distress vocalizations, confined living conditions, or slaughter processes. Users must be able to make an informed decision about whether to view specific content, not just "something sensitive."

## Investigation Footage Handling

Investigation footage is the most operationally important and psychologically dangerous content in the system. Implementation requirements:
- NEVER auto-play video or audio from investigations
- ALWAYS display footage in blurred state by default
- Require explicit opt-in for full resolution — a deliberate click, not a hover or scroll
- Provide frame-by-frame navigation for reviewers who need to examine specific moments without watching continuous footage
- Strip audio by default — distress vocalizations cause acute stress responses; audio should be a separate opt-in from video
- Support annotation without full-resolution viewing (reviewers can mark timestamps and regions on blurred preview)

## Witness Testimony Display

Before displaying any witness testimony: (1) verify that display consent is current and has not been withdrawn, (2) anonymize by default — display pseudonyms, not legal names, (3) require explicit opt-in to view identifying details, (4) log access to testimony for audit purposes while protecting the identity of who accessed it. When testimony includes descriptions of animal suffering, apply the same progressive disclosure and content warning rules as for visual media.

## Burnout Prevention Patterns

Advocacy software should actively support user wellbeing during extended content review sessions:
- **Session time awareness** — track continuous exposure time to traumatic content and surface non-intrusive reminders after configurable intervals (default: 30 minutes of active content review)
- **"Take a break" prompts** — for content reviewers who have been processing investigation footage or testimony for extended periods; these are suggestions, not blocks
- **Session summaries** — at the end of a content review session, provide a summary of what was reviewed so the reviewer does not need to re-expose themselves to verify completeness
- **Workload distribution** — when multiple reviewers are available, the system should support distributing traumatic content review across the team rather than concentrating it

## Secondary Trauma Mitigation

Secondary trauma affects not just end users but also **developers** building and testing this software. Design the development workflow to minimize unnecessary exposure: use abstract test data (described references, not actual footage) in automated tests, provide mock data generators that produce realistic metadata without graphic content, and document which test suites involve real content so developers can prepare. The CI/CD pipeline must never display graphic content in test output, logs, or failure reports.

## Opt-In Escalation of Graphic Content

When a user needs to access full-resolution graphic content, require multiple confirmation steps proportional to content severity. A single click is insufficient for the most graphic content. Implement a confirmation dialog that names what the user is about to see, requires an explicit "I understand" acknowledgment, and provides an alternative (text description, blurred summary) alongside the full-access option. This is not friction for friction's sake — it is informed consent applied to content exposure.
