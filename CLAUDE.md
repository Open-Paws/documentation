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

## Key Files

| File | Purpose |
|------|---------|
| `Knowledge/README.md` | Weaviate connection details, search ops, RAG patterns, Content schema |
| `Predictions/README.md` | Prediction model usage, batch processing, score clipping |
| `Generation/README.md` | 8B model usage, generation parameters, known limitations |
| `Automation/README.md` | n8n hosting options, workflow import, activation |
| `.gitleaksignore` | Secret scanning exclusions (read-only API keys in docs) |

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
