---
paths:
  - "**/*.test.*"
  - "**/*.spec.*"
  - "**/test/**"
  - "**/tests/**"
  - "**/__tests__/**"
---
# Testing Rules for Animal Advocacy Projects

Testing is the keystone of AI-assisted advocacy development. Without tests, AI agents drift silently — and in advocacy software, silent drift means lost evidence, exposed activists, or traumatic content displayed without safeguards. Every other practice depends on this foundation.

## Assertion Quality — The Non-Negotiable

NEVER write tautological assertions — tests that assert output equals output. This is the single most dangerous pattern in AI-generated tests. A test that calls a function and asserts the result equals the same function call tests nothing. Every assertion must encode a business rule you can explain in words: "this total equals the sum of line items because the pricing rule says X."

Ask three questions of every AI-generated test:
1. **Does this test fail if the code is wrong?** If you break the implementation and the test still passes, it is worthless.
2. **Does the assertion encode a domain rule?** If you cannot name the rule being verified, it is a snapshot, not a test. In advocacy software, unnamed rules mean unverified safety properties.
3. **Would mutation testing kill this?** If changing `+` to `-` in the implementation leaves the test green, the test is weak.

Quality metric: **mutation score over coverage percentage.** A suite with 90% coverage and 40% mutation score is a false sense of security. Track mutation score as the primary quality signal.

## Spec-First Test Generation

ALWAYS prefer generating tests from specifications or acceptance criteria before writing implementation — not after. Tests generated from existing implementation tend to mirror the code rather than the intent, producing circular validation. When the spec says "investigation records must be anonymized before export," write that test first. Then make it pass.

## Property-Based Testing for Invariants

Use property-based testing to verify that invariants hold across random inputs. Specific example-based tests miss edge cases that property-based tests catch systematically. Critical invariants in advocacy software: anonymization must be irreversible, encryption must not leak plaintext length, coalition data boundaries must hold under arbitrary input combinations.

## Test Error Paths Explicitly

AI-generated tests overwhelmingly cover happy paths. In advocacy software, the error paths are where people get hurt — failed encryption, leaked identity, broken anonymization, missing content warnings. Explicitly request and verify: error propagation, cleanup on failure, meaningful error messages, graceful degradation under hostile conditions. Test what happens when the network drops during evidence upload. Test what happens when storage is seized mid-write.

## Contract Tests at Service Boundaries

AI hallucinates API contracts — approximately 20% of AI-recommended packages do not exist. At every service boundary, especially coalition cross-organization APIs where different groups have different security postures, use consumer-driven contract tests. Do not trust AI-generated API client code without contract verification.

## Test Infrastructure Requirements

**Fast execution is non-negotiable.** AI agents run tests in tight loops. A 10-minute suite across 15 agent iterations burns 2.5 hours. Invest in parallel execution, test isolation with no shared state, and selective test running. **Flaky tests poison the AI feedback loop** — agents cannot distinguish flaky failures from real ones. Track and eliminate flaky tests aggressively. Maintain a test-to-code ratio of 1:1 or higher.

## Adversarial Input Testing

Test inputs crafted to exploit advocacy-specific vulnerabilities: SQL injection through investigation search fields, XSS through witness testimony display, path traversal through evidence file uploads, oversized payloads designed to crash offline-first sync. Advocacy software faces adversarial users — not just careless ones.
