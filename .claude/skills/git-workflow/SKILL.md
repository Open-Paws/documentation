---
name: git-workflow
description: Git workflow for AI-assisted advocacy development — atomic commits per subtask, ephemeral branches, PR curation into reviewable chunks, AI-Assisted tagging
---
# Git Workflow

## When to Use
- Before committing, branching, or creating a pull request
- When reviewing commit history or deciding on merge strategy
- After an AI agent has generated a batch of changes that need to be broken into logical units

## Process

### Step 1: Create an Ephemeral Branch
Create a short-lived branch for the current task. Trunk-based development remains the goal — this branch is a safety net, not a long-lived workspace. If the agent has not produced mergeable work within one session, delete the branch and reconsider the approach. Name the branch for the task, not for a feature epic.

### Step 2: Implement One Subtask
Break the overall task into the smallest logical subtasks. Each subtask is one commit. If the task decomposes into "extract interface, implement adapter, update callers," those are three separate commits. Never let the agent complete an entire multi-step task before committing.

### Step 3: Test Before Committing
Run the relevant test subset before each commit. Every commit must leave the codebase in a passing state. If tests fail, fix the issue before committing — do not push broken commits to shared branches. For advocacy code handling investigation or evidence data, also verify that no sensitive data has leaked into test output, logs, or error messages.

### Step 4: Write the Commit Message
Write commit messages that explain WHY, not WHAT — the code shows what changed. First line: 50 characters, imperative mood. Reference the issue or ticket. Add appropriate AI attribution trailers so the team knows which code was agent-generated.

### Step 5: Repeat for Each Subtask
Continue the cycle: implement one subtask, test, commit. Each commit should be independently understandable. If you read the commit in isolation six months from now, you should know what it does and why.

### Step 6: Curate the Pull Request
PR curation is the critical human skill in AI-assisted development. AI adoption inflated PR size by 154%. Do not submit the agent's full output as one PR. Split into reviewable chunks:
- Target under 200 lines changed per PR, ideally under 100
- Use stacked PRs for large changes (PR1, PR2, PR3 — each independently reviewable)
- Each PR tells a coherent story with a clear description explaining the reasoning

### Step 7: Tag and Request Review
- Tag every PR containing AI-generated code as **AI-Assisted**
- Require two human approvals for primarily AI-generated PRs
- Call out areas needing close review — especially security boundaries, error handling, and any code touching investigation or coalition data

### Step 8: Track Quality Signals
- **Code Survival Rate** — how much AI-generated code remains unchanged 48 hours after merge. Low survival means the agent is generating code that humans immediately rewrite.
- **Suggestion acceptance rate** — healthy range is 25-35%. Higher may indicate over-reliance on AI suggestions without critical review. In advocacy projects, over-reliance means unreviewed security assumptions.

### Merge Strategy
Squash-merge ephemeral branches to keep trunk history clean. Delete branches immediately after merge. If a branch has lived longer than one working session, evaluate whether the approach needs to change rather than extending the branch's life.
