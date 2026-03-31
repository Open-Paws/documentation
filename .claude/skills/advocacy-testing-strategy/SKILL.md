---
name: advocacy-testing-strategy
description: Spec-first test generation, assertion quality review, mutation testing, five anti-patterns to avoid — for AI-assisted advocacy development where silent test failures mean lost evidence or exposed activists
---
# Testing Strategy

## When to Use
- Writing or generating any tests
- Reviewing AI-generated test code
- Setting up test infrastructure for a new feature
- When test suite quality is in question (flaky tests, low mutation scores, false confidence)

## Process

### Step 1: Read the Specification
Before writing any test, identify the specification or acceptance criteria for the behavior under test. If no spec exists, write one — even a brief description of what the code should do and what constitutes failure. Without a spec, AI generates tests that mirror the implementation rather than the intent, producing circular validation.

### Step 2: Write Failing Tests from the Spec (Pattern 2 — Spec-First)
Generate tests from the specification BEFORE writing implementation. Each test should encode a business rule you can state in words. For advocacy software: "investigation records must be anonymized before export," "coalition data must not cross organizational boundaries without explicit agreement," "graphic content must never display without a content warning." Write the test. Verify it fails. This is the preferred generation pattern.

### Step 3: Verify Tests Fail for the Right Reason
A failing test is only useful if it fails because the behavior is absent — not because of a setup error, typo, or misconfigured test environment. Read each failure message. Confirm it describes the missing behavior, not a broken test.

### Step 4: Implement Until Tests Pass
Write the minimum implementation that makes the failing tests pass. Do not write more code than the tests demand.

### Step 5: Review Assertions Against the Spec, Not the Code
This is the critical step for AI-generated tests. Ask three questions of every assertion:
1. **Does this test fail if the code is wrong?** If you break the implementation and the test still passes, it is worthless.
2. **Does the assertion encode a domain rule?** If you cannot name the rule, it is a snapshot, not a test.
3. **Would mutation testing kill this?** If changing `+` to `-` leaves the test green, the assertion is weak.

NEVER accept tautological assertions — tests that assert output equals the output of the same function call. This is the single most dangerous pattern in AI-generated tests.

### Step 6: Run Mutation Testing
Run a mutation testing tool against the test suite. Surviving mutants reveal assertions that look thorough but verify nothing. Feed surviving mutants to the AI and ask it to write tests that kill them. Mutation score is the primary quality metric — not coverage percentage. A suite with 90% coverage and 40% mutation score is a false sense of security.

### Step 7: Fix Weak Tests
For each surviving mutant, write a targeted test that kills it. This closes the loop between test generation and test quality.

## Five Generation Patterns (Know When to Use Each)
1. **Implementation-first** — generate tests from existing code. Dangerous: tests mirror code, not intent. Use only when no spec exists and you need characterization tests.
2. **Spec-first** — generate tests from specification before coding. Preferred pattern. Produces tests that encode intent.
3. **Edge-case generation** — give the AI a function signature and ask specifically for: empty inputs, boundary values, null/undefined, unicode, timezone boundaries, concurrent access, overflow. AI excels here.
4. **Characterization tests** — for legacy or AI-generated code that lacks tests: capture current behavior before changing anything. Cover before you change (Feathers).
5. **Mutation-guided improvement** — run mutation testing, feed surviving mutants to AI, generate targeted tests.

## Five Anti-Patterns to Reject on Sight
1. **Snapshot trap** — tests that snapshot current output and assert against it. They pass today and break on any correct change. They verify nothing about correctness.
2. **Mock everything** — over-mocked tests verify that mocks behave as expected, not that real code works. Mock only at system boundaries: external APIs, databases, file systems.
3. **Happy path only** — AI-generated tests overwhelmingly test the success path. Explicitly request error path, boundary condition, and adversarial input tests. In advocacy software, error paths are where people get hurt.
4. **Test-after-commit** — writing tests after code is committed defeats the feedback loop. Tests must exist during development.
5. **Coverage theater** — chasing coverage numbers with meaningless assertions. A line "covered" by a test with no assertion is not tested.

## Advocacy-Specific Testing
- Contract tests at every service boundary, especially coalition cross-organization APIs where different groups have different security postures
- Test adversarial inputs: SQL injection through investigation search, XSS through testimony display, path traversal through evidence uploads
- Verify progressive disclosure: graphic content must not render without explicit opt-in
- Test offline behavior: what happens when connectivity drops during evidence sync
- Fast test execution is non-negotiable for AI agent loops — a 10-minute suite across 15 iterations burns 2.5 hours
