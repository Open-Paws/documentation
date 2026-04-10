# Documentation

Developer documentation for the Open Paws AI ecosystem: vector database, prediction models, generative language models, workflow automations, and HuggingFace datasets. This is the central reference for anyone building AI tools for animal advocacy using Open Paws infrastructure.

## Quick Start

No build step -- this is a pure documentation repo. Browse by topic:

- **Vector database (Weaviate):** `Knowledge/README.md`
- **Prediction models:** `Predictions/README.md`
- **Language models (8B):** `Generation/README.md`
- **n8n workflow automations:** `Automation/README.md`

## Architecture

```
Knowledge/       Weaviate vector-graph database docs (connection, search, RAG, schema)
Predictions/     HuggingFace text regression models (performance + preference prediction)
Generation/      8B language models (Llama 3.1 base, continual pre-training + instruct)
Automation/      n8n workflow templates for advocacy automation
.github/         Dependabot config + CI workflows
```

## File Descriptions

### Knowledge (Weaviate Vector Database)

| File | Purpose |
|------|---------|
| `Knowledge/README.md` | Weaviate connection details, search operations, RAG patterns, Content schema — primary reference for querying the Open Paws vector-graph database |

### Predictions (HuggingFace Models)

| File | Purpose |
|------|---------|
| `Predictions/README.md` | HuggingFace text regression model usage — performance and preference prediction, batch processing patterns, score clipping |

### Generation (Language Models)

| File | Purpose |
|------|---------|
| `Generation/README.md` | 8B language model usage — Llama 3.1 base with continual pre-training and instruct tuning, generation parameters, known limitations |

### Automation (n8n Workflows)

| File | Purpose |
|------|---------|
| `Automation/README.md` | n8n workflow automation — hosting options (cloud/self-hosted), workflow import/export, activation, advocacy-specific workflow templates |

### Infrastructure

| File | Purpose |
|------|---------|
| `Infrastructure/README.md` | Clean-room agent architecture reference — shared runtime patterns, tool registry, operator controls, and safety boundaries across Open Paws repos. Canonical source for the 2026-04-01 clean-room reuse decision |

### Root

| File | Purpose |
|------|---------|
| `README.md` | Human-facing overview — quick start, architecture summary, HuggingFace dataset links |
| `CONTRIBUTING.md` | Contribution guidelines for documentation PRs |
| `.gitleaksignore` | Secret scanning exclusions — covers read-only Weaviate API keys that appear in code examples |

## External Dependencies

| Service | Purpose | Docs |
|---------|---------|------|
| Weaviate Cloud | Vector-graph database (read-only access) | [weaviate.io](https://weaviate.io/developers/weaviate) |
| HuggingFace | Model hosting + datasets | [huggingface.co/open-paws](https://huggingface.co/open-paws) |
| OpenAI API | Embeddings for Weaviate search | [platform.openai.com](https://platform.openai.com/docs) |
| OpenRouter | LLM routing (used by downstream tools) | [openrouter.ai](https://openrouter.ai) |
| n8n | Workflow automation platform | [docs.n8n.io](https://docs.n8n.io) |

## HuggingFace Datasets

- [Conversational Fine-Tuning](https://huggingface.co/datasets/open-paws/conversational-finetuning)
- [Continued Pre-Training](https://huggingface.co/datasets/open-paws/continued-pretraining)
- [Visual Q&A](https://huggingface.co/datasets/open-paws/visual-qa)
- [Animal Alignment Feedback](https://huggingface.co/datasets/open-paws/animal-alignment-feedback)
- [Reasoning](https://huggingface.co/datasets/open-paws/reasoning)
- [Tool Use](https://huggingface.co/datasets/open-paws/tool-use)

## Development

- **Adding documentation:** Create a new directory with a `README.md` following the existing pattern
- **Code examples:** Python with `transformers` or `weaviate` client libraries -- keep examples copy-pasteable
- **Style:** Each section should be self-contained with connection details, code samples, and best practices

## Organizational Context

**Layer:** 1 | **Lever:** Strengthen | **Integration:** Reference material for platform and ecosystem

This repo documents the AI infrastructure layer that Open Paws tools are built on. It is a reference for Guild developers, bootcamp students, and coalition partners building on the platform.

**Settled decisions affecting this repo:**
- **2026-04-01: Clean-room agent architecture** — `documentation` owns the shared infrastructure note for the clean-room reuse decision. PR #7 merged (part of the 3/4 clean-room rollout completed 2026-04-09: PCC#13, platform#42, docs#7 merged). Remaining: Tools-Platform#1 repo name needs verification before the rollout is marked complete. See `closed-decisions.md` 2026-04-01.

**Relevant strategy documents:**
- `ecosystem/repos.md` — documentation listed as reference material
- `programs/developer-training-pipeline/guild/operations.md` — Guild developers use this reference

**Current status (as of 2026-04-09):** Active reference. Clean-room architecture PR #7 merged. The shared infrastructure note is live. Tools-Platform#1 is the outstanding item in the 4-repo rollout.

## Development Standards

### 10-Point Review Checklist (ranked by AI violation frequency)

1. **DRY** — AI clones code at 4x the human rate. Search before writing anything new
2. **Deep modules** — Reject shallow wrappers and pass-through methods
3. **Single responsibility** — Each function does one thing at one level of abstraction
4. **Error handling** — Never catch-all
5. **Information hiding** — Don't expose internal state. Mask API keys (last 4 chars only)
6. **Ubiquitous language** — Use movement terminology consistently
7. **Design for change** — Abstraction layers and loose coupling
8. **Legacy velocity** — Use characterization tests before modifying existing code
9. **Over-patterning** — Simplest structure that works
10. **Test quality** — Every test must fail when the covered behavior breaks

### Quality Gates

- **Desloppify:** `desloppify scan --path .` — minimum score ≥85
- **Speciesist language:** `semgrep --config semgrep-no-animal-violence.yaml` on all docs edits
- **Two-failure rule:** After two failed fixes on the same problem, stop and restart

### Seven Concerns — Critical for This Repo

All 7 concerns apply. Highlighted critical ones:

- **Privacy** (critical) — API keys documented here are read-only Weaviate keys. `.gitleaksignore` covers these. Never commit write-access keys. Check `.gitleaksignore` is current before adding new examples.
- **Security** — Code examples must use environment variables for any keys. Never hardcode credentials.
- **Advocacy domain** — All documentation must use movement terminology. Examples should reference **farmed animals** and **factory farms**, not industry euphemisms.
- **Accessibility** — Documentation must work for developers on low-bandwidth connections. Avoid large embedded images.
- **Emotional safety** — If documentation examples include advocacy content (animal welfare data, investigation statistics), apply content warnings.

### Advocacy Domain Language

Never introduce synonyms for:
- **Farmed animal** — not "livestock" in code examples or documentation
- **Factory farm** — not "farm" or "production facility"
- **Campaign** — organized advocacy effort
- **Investigation** — covert documentation (all data is potential evidence)

### Structured Coding Reference

For tool-specific AI coding instructions (Claude Code rules, Cursor MDC, Copilot, Windsurf, etc.), copy the corresponding directory from `structured-coding-with-ai` into this project root.

## Decisions Reviewed

Last reviewed: 2026-04-11
