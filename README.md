# Open Paws Documentation

**Status: Active Development**

The central reference for building AI tools that support animal advocacy using Open Paws infrastructure. This repo covers the full AI stack: vector knowledge base, prediction models, language models, workflow automations, and training datasets.

Ecosystem: [huggingface.co/open-paws](https://huggingface.co/open-paws) | [github.com/Open-Paws](https://github.com/Open-Paws)

---

## What this repo is

Open Paws provides AI infrastructure purpose-built for animal advocacy — not adapted from generic tools. This repo is the developer-facing documentation hub for that infrastructure. It is read-only: no build step, no application code, no deployable services.

If you are building an advocacy tool, a campaign automation, or a fine-tuned model using Open Paws resources, start here.

---

## Content map

```
Knowledge/        Weaviate vector-graph database — connection, search, RAG, schema
Predictions/      HuggingFace text regression models — performance and preference prediction
Generation/       8B language models — Llama 3.1 base with continual pre-training and instruct tuning
Automation/       n8n workflow templates — advocacy automation patterns
Infrastructure/   Clean-room agent architecture — shared runtime, tool registry, safety boundaries
CONTRIBUTING.md   How to add or improve documentation
```

Each directory has a `README.md` as its entry point. Every section is self-contained: connection details, working code samples, known limitations, and best practices live together in one place.

---

## Quick navigation

### Knowledge infrastructure (Weaviate)

The Open Paws vector-graph database holds advocacy content — articles, reports, campaign materials, investigation findings — with per-document scoring across dimensions like trustworthiness, insight, relevance, and predicted performance.

- [Overview and quick start](Knowledge/README.md)
- Hosted on Weaviate Cloud (read-only public access)
- Supports vector search, hybrid search, filtered search, and RAG
- Requires your own OpenAI API key for embedding generation

### Prediction models

Text regression models that score advocacy content on performance metrics and human-preference dimensions. Useful for ranking candidate messages, evaluating campaign materials, and building feedback loops.

- [Overview and quick start](Predictions/README.md)
- All models on HuggingFace: [open-paws ranking models collection](https://huggingface.co/collections/open-paws/ranking-models-67b94c024535b84d0b73648b)
- Key models: `perceived_trustworthiness_prediction` and the full performance/preference suite
- Scores are 0–1 continuous; clip outputs to range before using downstream

### Language models (generation)

Small language models (8B parameters) specialized for animal advocacy through continual pre-training and instruction tuning on Llama 3.1. Optimized for local and low-memory deployment.

- [Overview and quick start](Generation/README.md)
- [`open-paws/8B-base-model`](https://huggingface.co/open-paws/8B-base-model) — pre-trained base, no instruction tuning
- [`open-paws/8B-instruct-chat`](https://huggingface.co/open-paws/8B-instruct-chat) — chat-tuned instruct model
- Use explicit animal alignment context in system prompts for best results

### n8n workflow automations

Ready-to-use n8n workflow templates for common advocacy automation tasks.

- [Overview, hosting options, and import guide](Automation/README.md)
- Templates on [n8n Creator Hub](https://n8n.partnerlinks.io/open-paws)
- Hosting options: n8n Cloud (teams), self-hosted via RepoCloud/Elestio (individuals), local install (high-security users)

### Agent infrastructure

Architectural reference for the clean-room agent runtime shared across Open Paws projects. Covers shared patterns for tool registries, orchestration, operator controls, and safety boundaries.

- [Overview and design decisions](Infrastructure/README.md)
- Canonical source for the 2026-04-01 clean-room reuse decision
- Relevant to: `project-compassionate-code`, `open-paws-platform`, Tools-Platform

---

## Training datasets

All datasets are on HuggingFace under [open-paws](https://huggingface.co/open-paws):

| Dataset | Purpose |
|---------|---------|
| [conversational-finetuning](https://huggingface.co/datasets/open-paws/conversational-finetuning) | Supervised fine-tuning on advocacy conversations |
| [continued-pretraining](https://huggingface.co/datasets/open-paws/continued-pretraining) | Domain-adaptive pre-training corpus |
| [visual-qa](https://huggingface.co/datasets/open-paws/visual-qa) | Visual questions and answers for multimodal models |
| [animal-alignment-feedback](https://huggingface.co/datasets/open-paws/animal-alignment-feedback) | Preference feedback for alignment training |
| [reasoning](https://huggingface.co/datasets/open-paws/reasoning) | Chain-of-thought reasoning examples |
| [tool-use](https://huggingface.co/datasets/open-paws/tool-use) | Structured tool-use demonstrations |

---

## External dependencies

| Service | Role | Docs |
|---------|------|------|
| Weaviate Cloud | Vector-graph database (read-only public access) | [weaviate.io](https://weaviate.io/developers/weaviate) |
| HuggingFace | Model and dataset hosting | [huggingface.co/open-paws](https://huggingface.co/open-paws) |
| OpenAI API | Embeddings for Weaviate search | [platform.openai.com](https://platform.openai.com/docs) |
| n8n | Workflow automation platform | [docs.n8n.io](https://docs.n8n.io) |
| OpenRouter | LLM routing used by downstream tools | [openrouter.ai](https://openrouter.ai) |

---

## How to contribute documentation

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide. Short version:

1. One topic per PR, under 200 lines where possible
2. New section = new directory with a `README.md`
3. Code examples must be copy-pasteable — use environment variables for all credentials, never hardcode them
4. Run `semgrep --config semgrep-no-animal-violence.yaml` on any `.md` files before opening a PR
5. Run `desloppify scan --path .` and confirm score is 85 or above
6. Use movement terminology throughout (see [language guide](#language-and-style))

### Language and style

This documentation exists to support animal liberation. Use precise advocacy language:

- **Farmed animal** — not "livestock" (industry framing)
- **Factory farm** — not "farm" or "production facility"
- **Campaign** — an organized advocacy effort with defined goals
- **Investigation** — covert documentation (all data is potential evidence)
- **Sanctuary** — permanent care facility, not "shelter"

Avoid idioms that normalize harm to animals. The CI workflow enforces this automatically via [no-animal-violence](https://github.com/Open-Paws/no-animal-violence) rules — if it flags something, fix it rather than bypassing it.

### Documentation structure conventions

- Entry point for every directory: `README.md`
- File names: lowercase with hyphens (`batch-inference.md`, not `BatchInference.md`)
- Each section must be self-contained: include connection details, working examples, and known limitations together
- Link to canonical sources in other repos rather than duplicating content that will drift

---

## Relation to the Open Paws ecosystem

This repo is the infrastructure documentation layer. The code it documents lives in:

- [`platform`](https://github.com/Open-Paws/platform) — the Astro 5 + React 19 + Supabase platform
- [`gary`](https://github.com/Open-Paws/gary) — the 24/7 autonomous advocacy agent
- [`project-compassionate-code`](https://github.com/Open-Paws/project-compassionate-code) — submits animal-friendly PRs to open-source repos at scale
- [`graze-cli`](https://github.com/Open-Paws/graze-cli) — agentic coding CLI with advocacy-aware prompts

Strategy and priorities: [github.com/Open-Paws/context](https://github.com/Open-Paws/context)
