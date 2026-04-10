# Contributing to Open Paws Documentation

This repo contains developer documentation, examples, and guides for building AI implementations that support animal advocacy using Open Paws models, data, and tools. It is a pure docs repo — no build step, no runnable application code.

## What belongs here

- **Reference documentation** for Open Paws infrastructure (vector database, prediction models, language models, workflow automations)
- **Copy-pasteable code examples** showing how to connect to and use Open Paws services
- **Integration guides** for common patterns (RAG, batch inference, n8n automations)
- **HuggingFace dataset documentation** for fine-tuning workflows

What does not belong here: application code, deployment configuration, runnable services, or content that duplicates the canonical source in another repo. If you are documenting something that lives in another Open Paws repo, link to it rather than copying it.

## Directory structure

```text
Knowledge/       Weaviate vector-graph database (connection, search, RAG, schema)
Predictions/     HuggingFace text regression models (performance + preference prediction)
Generation/      8B language models (Llama 3.1 base, continual pre-training + instruct)
Automation/      n8n workflow templates for advocacy automation
Infrastructure/  Clean-room agent runtime, orchestration, and tool-safety patterns
```

Each section has a `README.md` as its entry point. Add new topics as a new directory with a `README.md`, or as additional markdown files within an existing directory if they are closely related to what is already there.

## File naming

- Use lowercase with hyphens: `batch-inference.md`, not `BatchInference.md` or `batch_inference.md`
- Entry point for every directory is `README.md`
- Keep filenames descriptive but short — the directory provides context

## Writing style

Write in plain, direct prose. First-person plural ("we") is appropriate when describing Open Paws practices. Avoid passive constructions that obscure who does what.

**Be self-contained.** Each section should include enough context to be useful without requiring the reader to cross-reference other sections. Include connection details, working code samples, and known limitations in the same document.

**Keep examples copy-pasteable.** Code samples should run with minimal modification — typically just substituting API keys from environment variables.

**Use movement terminology.** This documentation exists to help developers build tools for animal liberation. Use precise advocacy language:

- **Farmed animal** — not "livestock" (industry framing)
- **Factory farm** — not "farm" or "production facility"
- **Campaign** — an organized advocacy effort with defined goals
- **Investigation** — covert documentation (all data is potential evidence)
- **Sanctuary** — permanent care facility, not "shelter"

The full language guide and automated enforcement tools are at [github.com/Open-Paws/no-animal-violence](https://github.com/Open-Paws/no-animal-violence). The pre-commit hook runs on every commit — if it flags something, fix it rather than bypassing it.

**No speciesist idioms.** Avoid idioms that normalize animal violence. Common ones to watch for: "kill two birds with one stone" (use "accomplish two things at once"), "guinea pig" as a test-subject metaphor (use "test subject"), "cattle vs. pets" distinctions (use "ephemeral vs. persistent").

## Referencing Open Paws tools and APIs

When documenting an API or service:

1. State the service name and its role in one sentence
2. Show how to authenticate using environment variables (never hardcode credentials, even read-only ones)
3. Provide a minimal working example before moving to advanced usage
4. List known limitations and edge cases explicitly

Read-only Weaviate keys used in examples are covered by `.gitleaksignore`. Do not add write-access credentials to examples under any circumstances.

## PR guidelines

- **One topic per PR.** Documentation PRs should be easy to review. If you are adding a new section and fixing an existing one, open two PRs.
- **Keep PRs under 200 lines** where possible. Longer PRs are harder to review and more likely to introduce terminology drift.
- **Run the language check** before opening a PR: run semgrep using the rules from [github.com/Open-Paws/semgrep-rules-no-animal-violence](https://github.com/Open-Paws/semgrep-rules-no-animal-violence) on any `.md` files you changed. The CI action will catch violations, but it is faster to fix them locally.
- **Link the upstream source** if your documentation describes a feature defined in another repo. Docs that duplicate source-of-truth content in another repo will drift — link instead.

## Quality gate

This repo runs [desloppify](https://github.com/Open-Paws/desloppify) with a minimum score of 85. If your PR drops the score below that threshold, the CI check will fail. Run `desloppify scan --path .` locally before pushing to catch issues early.
