---
name: advocacy-code-review
description: Layered code review pipeline — automated checks first, then AI-assisted review, then human review focused on Ousterhout red flags, AI failure patterns, silent failures, and advocacy-specific concerns
---
# Code Review

## When to Use
- Reviewing any code before merge — especially AI-generated code
- Preparing your own code for review
- When a PR is tagged AI-Assisted
- When changes touch investigation data, coalition boundaries, or emotional safety features

## Process

### Layer 1: Automated Checks (Zero Human Effort)
Before any human looks at the code, verify these pass:
- Formatting and linting — automated, enforced in CI, not discussed in review
- Static analysis and type checking — catches structural issues, type errors, known vulnerability patterns
- Security scanning — detects hardcoded secrets, known vulnerability patterns, dependency issues
- Test suite — all tests pass, no regressions

If any automated check fails, fix it before requesting review. Do not submit "I'll fix the tests later" PRs.

### Layer 2: AI-Assisted First-Pass Review
Use AI to flag potential issues. AI catches well: inconsistent error handling, missing null checks, unused imports, common security patterns, deviations from project conventions, performance anti-patterns. AI misses: whether the approach is correct, whether business logic matches requirements, whether the code will be maintainable, whether tests verify meaningful properties, subtle concurrency issues. Treat AI review flags as suggestions, not verdicts.

### Layer 3: Human Review — Design Quality Red Flags
Walk through the Ousterhout red flags checklist. These are the structural problems most common in AI-generated code:

- **Shallow module** — interface is as complex as the implementation; the abstraction hides nothing
- **Information leakage** — implementation details escape through the interface; callers depend on internals
- **Temporal decomposition** — code structured by execution order rather than conceptual boundaries
- **Pass-through method** — method that does nothing except call another method with the same signature
- **Repetition** — same logic in multiple places; AI duplicates at 4x the normal rate
- **Special-general mixture** — general-purpose code polluted with special-case handling

### Layer 4: Human Review — AI-Specific Failure Patterns
Check specifically for patterns AI agents introduce:

- **DRY violations** — does this duplicate something that already exists in the codebase? Search before accepting.
- **Multi-responsibility functions** — does any function do more than one thing at one level of abstraction? Split it.
- **Suppressed errors** — has the AI removed safety checks, caught exceptions too broadly, or silently swallowed failures? Review every error handling path.
- **Hallucinated APIs** — does the code call libraries, methods, or endpoints that do not exist? Verify every external dependency.
- **Over-patterning** — has the AI applied Strategy, Factory, or Observer where a plain function and conditional would suffice?
- **Silent failure pattern** — AI may remove safety checks to make code appear to work, create fake output matching desired formats, or edit tests to pass rather than fixing the underlying code. Verify ALL safety checks from the original code are preserved in the new code. Compare the error handling paths between old and new versions explicitly.

### Layer 5: Advocacy-Specific Review
For any code in an advocacy project, also verify:

- **Data leak vectors** — does this change create any new path for sensitive data to leave the system? Check logging, error messages, telemetry, API responses, and serialization output for investigation data, witness identities, or activist PII.
- **Surveillance surface area** — does this change increase the metadata footprint? New timestamps, access logs, IP recording, or device fingerprinting that could be used to identify activists under legal discovery.
- **Emotional safety** — if this code displays content to users, does it respect progressive disclosure? Is graphic content behind explicit opt-in? Are content warnings specific enough?
- **Coalition boundary violations** — does this change allow data to cross organizational boundaries without going through an anti-corruption layer? AI agents optimize for expedience and import directly across bounded contexts.

### Step: Render Verdict
Summarize findings by layer. Distinguish between blocking issues (security vulnerabilities, data leaks, silent failures, broken tests) and suggestions (style, naming, minor refactoring). For primarily AI-generated PRs, require two human approvals.
