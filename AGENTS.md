# AGENTS.md — Open Paws Documentation

This is the central documentation hub for the Open Paws AI ecosystem. It is a pure Markdown repo — no build step, no runnable application code — covering the vector knowledge base (Weaviate), prediction models, 8B language models, n8n workflow automations, agent infrastructure patterns, and HuggingFace training datasets. Developers building advocacy tools with Open Paws infrastructure start here.

## Status

**Active Development** — content is actively maintained and reflects current infrastructure. The clean-room agent architecture (PR #7, merged 2026-04-09) is the most recent structural change. The outstanding item in the 4-repo clean-room rollout is Tools-Platform#1 (repo name verification needed before the rollout is marked complete).

Changes here are low-risk (documentation only), but terminology drift and outdated code examples are the primary failure modes to watch for. Any change to API connection details, model identifiers, or dataset names must be verified against live services before committing.

## Directory structure

```
Knowledge/        Weaviate vector-graph database
                  - Connection details (REST + gRPC endpoints, read-only API key)
                  - Search patterns: vector, hybrid, filtered
                  - RAG patterns: single-prompt and grouped-task generation
                  - Database schema for the Content collection
                  - Advanced retrieval: re-ranking, multi-score, progressive filtering

Predictions/      HuggingFace text regression models
                  - Performance prediction (social media and email marketing metrics)
                  - Preference prediction (cultural sensitivity, emotional impact, persuasiveness)
                  - Usage via transformers pipeline
                  - Score clipping pattern for out-of-range outputs

Generation/       8B language models (Llama 3.1 base)
                  - Continual pre-training base model
                  - Instruction-tuned chat model
                  - Generation parameters for creative vs. focused responses
                  - Known limitations and alignment guidance

Automation/       n8n workflow automations
                  - Hosting options: Cloud, self-hosted (RepoCloud/Elestio), local, no-KYC VPS
                  - Workflow import/export procedure
                  - Link to n8n Creator Hub template library

Infrastructure/   Clean-room agent architecture reference
                  - Shared runtime pattern across Open Paws projects
                  - Tool registry design
                  - Orchestration layers: scanner ingestion → platform orchestration → tool registry
                  - Safety boundaries and operator controls
                  - Per-repo implementation tracks (PCC, platform, Tools-Platform)

.claude/          Claude Code configuration (rules/, skills/)
.github/          CI workflows: auto-merge, gitleaks secret scanning, no-animal-violence language check, label setup
CONTRIBUTING.md   Full contribution guide: structure, naming, style, PR guidelines, quality gates
CLAUDE.md         Detailed agent instructions (architecture, file descriptions, external dependencies, settled decisions)
README.md         Human-facing overview and quick navigation
```

## How documentation is organized

Each top-level directory maps to one infrastructure concern. The organizing principle is that each section must be self-contained: a developer reading `Knowledge/README.md` should find everything they need without cross-referencing other sections. Connection details, working code samples, and known limitations all live in the same file as the conceptual overview.

The `Infrastructure/` directory is the exception — it is an architectural reference that deliberately spans multiple repos. It documents patterns rather than a single deployable service.

## How to add new documentation

1. New infrastructure area: create a new top-level directory with a `README.md` as the entry point. Follow the existing pattern: one-sentence service description, authentication via environment variables, minimal working example before advanced usage, known limitations at the end.

2. Additional topics within an existing area: add a new `.md` file inside the relevant directory if the content is closely related to what is already there. Use lowercase-with-hyphens naming (`batch-inference.md`, not `BatchInference.md`).

3. Never duplicate content that lives canonically in another repo. Link to it instead — docs that copy source-of-truth content will drift.

4. All code examples must use environment variables for credentials. Read-only Weaviate keys that appear in examples are covered by `.gitleaksignore` — do not add write-access credentials under any circumstances.

5. Before opening a PR: run `semgrep --config semgrep-no-animal-violence.yaml` on all changed `.md` files, and run `desloppify scan --path .` to confirm the score is 85 or above.

## Integration points with other repos

| Repo | Relationship |
|------|-------------|
| [`platform`](https://github.com/Open-Paws/platform) | Platform ingests Weaviate knowledge base and prediction model scores; this repo documents those APIs |
| [`project-compassionate-code`](https://github.com/Open-Paws/project-compassionate-code) | `Infrastructure/README.md` documents the clean-room scanner export contract PCC must implement |
| [`no-animal-violence`](https://github.com/Open-Paws/no-animal-violence) | Language rules enforced by CI on every PR to this repo |
| [`context`](https://github.com/Open-Paws/context) | Strategy repo; check before starting any session to pick up current priorities |

## Quality gates

- **Speciesist language:** `semgrep --config semgrep-no-animal-violence.yaml` — enforced by CI on all `.md` edits
- **Secret scanning:** gitleaks runs on every push; read-only Weaviate keys in examples are allowlisted in `.gitleaksignore`
- **Desloppify:** minimum score 85; run `desloppify scan --path .` locally before pushing

## Known issues and TODOs

- `Automation/README.md` links to n8n Creator Hub but does not list or describe specific workflow templates by name. A workflow index would make the section more useful.
- `Generation/README.md` references only the 8B models. If larger or multimodal models are added to HuggingFace, this section needs updating.
- `Infrastructure/README.md` references `LarytheLord/Open-Paws-Tools-Platform` — verify the canonical repo name once Tools-Platform#1 is resolved (the outstanding item in the 4-repo clean-room rollout).
- No `CHANGELOG.md` exists. Significant infrastructure changes (new model releases, schema updates, API endpoint changes) are currently only tracked via commit messages and `CLAUDE.md` settled decisions.
- The `Predictions/` section would benefit from a table listing all available models by name with a one-line description of what each predicts.

## Advocacy domain language

Use precise movement terminology throughout. Never use industry framing:

- **Farmed animal** not "livestock"
- **Factory farm** not "farm" or "production facility"
- **Campaign** — organized advocacy effort
- **Investigation** — covert documentation (legally sensitive)
- **Sanctuary** — permanent care facility, not "shelter"

Full term list: [CONTRIBUTING.md](CONTRIBUTING.md) and [no-animal-violence](https://github.com/Open-Paws/no-animal-violence).

## Security notes for agents

- The read-only Weaviate API key in `Knowledge/README.md` is intentionally public — it grants read-only access only. Do not treat it as a secret.
- Never introduce write-access credentials into any example, even in comments.
- Supply chain note: all HuggingFace model identifiers (`open-paws/8B-instruct-chat`, etc.) should be verified against [huggingface.co/open-paws](https://huggingface.co/open-paws) before use in downstream tools — do not assume model names from training data are current.
