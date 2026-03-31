---
name: plan-first-development
description: Plan-before-code workflow — read existing code, write spec, decompose into subtasks, implement and test one at a time, comprehension check before committing
---
# Plan-First Development

## When to Use
- Starting any significant implementation work
- Beginning a new coding session
- When a task involves changes across multiple files or modules
- When context window usage is approaching 50%

## Process

### Step 1: Read Existing Code
Before writing anything, read the code relevant to the change. Understand the current structure, naming conventions, existing utilities, and architectural patterns. AI agents generate duplicate functions and violate DRY at 4x the normal rate because they lack full codebase awareness. Searching first prevents this.

### Step 2: Identify What Needs to Change
State the change in one sentence. If you cannot describe it concisely, the task needs further decomposition. For advocacy projects, also identify: which bounded context is affected (investigation ops, public campaigns, coalition coordination, legal defense), and whether the change crosses context boundaries.

### Step 3: Write a Specification
Write requirements before implementation. Even a brief spec is better than none. The spec should include: what the code does, what inputs it accepts, what outputs it produces, what error conditions exist, and what security or safety properties it must maintain. For advocacy software, add: what data sensitivity classification applies, what happens under device seizure, and what coalition data boundaries must hold.

This is the "construction prerequisites" principle: problem definition clear, requirements explicit, architecture solid — before writing code.

### Step 4: Break into Subtasks
Decompose the spec into subtasks small enough that each can complete within half the remaining context window. Each subtask should produce a testable, committable result. If a subtask feels too large, break it further. The decomposition should follow conceptual boundaries, not execution order — temporal decomposition (structuring code by when things happen) is an Ousterhout red flag.

### Step 5: For Each Subtask — Plan, Test, Implement, Verify
Execute one subtask at a time:
1. **Plan** — describe what this subtask will do
2. **Test** — write a failing test that encodes the expected behavior
3. **Implement** — write the minimum code to make the test pass
4. **Verify** — run tests, confirm the change works in context

Do not start the next subtask until the current one passes tests and is committed.

### Step 6: Comprehension Check
After the AI generates code, explain what it does in your own words before committing. This is not optional. AI-assisted developers score 17 percentage points lower on comprehension tests compared to unassisted developers (Anthropic, 2026 study of 52 professional developers). The comprehension gap is equivalent to nearly two letter grades.

Use the **generation-then-comprehension pattern**: generate code, then immediately ask the AI to explain it, then verify your understanding matches. Six usage patterns exist on a spectrum from full delegation (worst comprehension at 50%) to conceptual inquiry (best at 86%). The goal is to stay in the "generate then understand" zone that preserves learning while leveraging AI speed.

If you cannot explain what the code does and why, do not commit it. Read it again. Ask questions. Understanding the code you ship is a non-negotiable professional responsibility — especially in advocacy software where misunderstood code can silently leak data or expose activists.

### Step 7: Commit
Commit after each completed subtask. Write a commit message explaining the WHY. Then move to the next subtask.

### Context Management
- Start each session fresh rather than extending a degraded conversation
- Compact at approximately 50% context usage
- Break work into chunks that complete within half the context window
- After two failed fix attempts, clear the conversation and restart with a better prompt incorporating what you learned — do not compound errors
