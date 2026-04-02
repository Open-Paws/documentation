# Clean-Room Agent Infrastructure

This guide describes how Open Paws can adopt stronger agent-runtime patterns without copying proprietary implementations. The goal is to reuse the ideas that are broadly useful, then implement them from scratch in our own systems, data models, and interfaces.

## Core Principles

1. Build from patterns, not copied code
2. Keep sensitive workflows explicit and reviewable
3. Centralize tool definitions, permissions, and metadata
4. Separate long-running orchestration from user-facing chat
5. Preserve source provenance for scanner findings, quests, and automations

## Shared Runtime Pattern

The target shape across Open Paws projects is:

1. **Scanner and external signal ingestion**
2. **Structured export and normalization**
3. **Platform-side task, quest, and conversation orchestration**
4. **Tool registry with sensitivity-aware UI and routing**
5. **Human review and approval before high-impact actions**

This keeps research, execution, and user interfaces aligned while still letting each repo own a clear slice of the system.

## Current Implementation Tracks

### `Open-Paws/project-compassionate-code`

The scanner is the best place to normalize repository findings into a stable export format. That export should include:

- deterministic finding identifiers
- repository provenance
- merge-tier or effort metadata
- stable payloads that downstream systems can ingest without re-parsing scanner output

This repo should continue to act as the source of truth for machine-readable contribution opportunities.

### `Open-Paws/open-paws-platform`

The platform should own guild-facing orchestration and persistent state. Current and near-term responsibilities include:

- ingesting scanner findings into draft guild quests
- storing quest source provenance
- supporting dry-run previews before import
- acting as the eventual home for cleaner plan, task, and background-session orchestration

This is the right layer for approval boundaries, operator controls, and durable workflow state.

### `LarytheLord/Open-Paws-Tools-Platform`

The tools platform is the right sandbox for experimenting with more agentic UX before it is promoted into production systems. The first clean-room pattern to establish here is a central tool registry that can drive:

- quick actions
- intent detection
- sensitivity labels
- future approval prompts
- future policy hooks for restricted workflows

This keeps the UI and runtime metadata from drifting apart as more tools are added.

## Recommended Next Steps

### Layer 1: Shared Definitions

- standardize tool metadata fields across repos
- align sensitivity levels for public, medium-risk, and high-risk workflows
- document a shared scanner export contract

### Layer 2: Orchestration

- add explicit background-task lifecycle states
- separate draft generation from action execution
- add review checkpoints before external writes or sensitive research flows

### Layer 3: Operator Experience

- show sensitivity cues directly in the UI
- make provenance visible for imported quests and generated tasks
- support dry-run previews wherever automation can create records or tasks

## Safety Boundaries

Open Paws should not copy source files, prompts, comments, or proprietary internal naming from third-party leaked codebases. Safe reuse means:

- extracting product patterns
- rewriting implementations from scratch
- documenting trust boundaries
- validating that sensitive workflows have clear operator review paths

## Success Criteria

This infrastructure direction is working when:

1. scanner findings move into the guild pipeline with stable provenance
2. tooling interfaces share one source of truth for intent and risk metadata
3. higher-sensitivity actions are visible, reviewable, and easier to gate
4. repo-specific experimentation can graduate into the platform without rewriting the architecture each time
