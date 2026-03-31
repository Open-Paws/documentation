---
paths:
  - "**/security/**"
  - "**/*auth*"
  - "**/*crypto*"
  - "**/*encrypt*"
---
# Security Rules for Animal Advocacy Projects

Advocacy software faces three distinct adversaries, each requiring different countermeasures: **state surveillance** (law enforcement using ag-gag statutes, warrants, and subpoenas), **industry infiltration** (corporate investigators posing as volunteers, social engineering attacks against coalition members), and **AI model bias** (training data that encodes industry framing, models that refuse to assist with certain advocacy operations, or that leak investigation details through telemetry). Security is not a feature layer — it is the structural foundation of every design decision.

## Zero-Retention APIs

NEVER send sensitive data to external services that retain inputs. Investigation footage, witness identities, activist communications, and coalition coordination data must only flow through zero-retention API configurations. Verify retention policies contractually, not by assumption. Telemetry to third parties is a data exfiltration vector in adversarial legal discovery — not just a privacy preference.

## Encrypted Local Storage with Plausible Deniability

All locally stored investigation data, evidence, and activist records MUST use encrypted volumes. Design storage so that the existence of sensitive data is deniable under device seizure. Nested encrypted containers where the outer layer contains innocuous data and the inner layer requires a separate key is the standard pattern. A seized device must not reveal what it contains without the correct credentials.

## Supply Chain Verification — Slopsquatting Defense

Approximately **20% of AI-recommended packages do not exist** — they are hallucinated names. Attackers monitor these hallucinated names and register them as real packages containing malicious code. One such package was downloaded 30,000+ times in weeks. **Verify EVERY dependency** exists in its actual registry and has legitimate maintainers before installation. Only 1 in 5 AI-recommended dependency versions are both safe and free from hallucination. In advocacy software, a compromised dependency can exfiltrate investigation data or activist identities.

## Input Validation Against Industry Sabotage

Assume adversarial input on every public-facing surface. Industry actors will probe investigation submission forms, evidence upload endpoints, and public campaign tools. Validate and sanitize all inputs at system boundaries — the "barricade" pattern from defensive programming. AI-generated input validation is weak: 45% of AI-generated code contains OWASP Top 10 vulnerabilities, with 86% failure rate on cross-site scripting defenses.

## Ag-Gag Legal Exposure Vectors

Investigation footage is discoverable evidence under legal proceedings. Design every data flow assuming adversarial legal discovery, not just adversarial hackers. Metadata (timestamps, geolocation, device identifiers) can be more damaging than content. Strip metadata aggressively. Audit logging must protect the identities it records — logs that identify who accessed investigation data become prosecution tools.

## Device Seizure Preparation

Design for the scenario where devices are confiscated without warning. Remote wipe capability for all sensitive data. Encrypted volumes that lock automatically on suspicious conditions (unexpected power loss, extended inactivity, SIM removal). The application must not leak data on unexpected termination — no temporary files with decrypted content, no swap files containing sensitive state, no crash dumps with investigation data.

## Instruction File Integrity — Rules File Backdoor

The "Rules File Backdoor" attack uses hidden Unicode characters in instruction files (`.cursorrules`, `.mdc` files, `CLAUDE.md`) to inject invisible directives that instruct AI to produce malicious output. **Treat ALL instruction files as security-critical artifacts.** Review them for non-printable characters. Diff instruction file changes character-by-character. In advocacy projects, a compromised instruction file could direct the AI to weaken encryption, leak data to external endpoints, or disable safety checks.

## Self-Hosted Inference for Critical Paths

Any code path handling investigation data, witness identities, or legal defense materials should use self-hosted AI inference — not cloud-hosted APIs. Model providers may comply with government data requests. For routine development tasks (formatting, boilerplate, documentation), external APIs are acceptable. For anything touching the three adversaries' interests, self-host.

## MCP Server Security

MCP servers extend agent capabilities but also extend the attack surface. Any MCP server handling sensitive advocacy data MUST be self-hosted. Audit each server's data access patterns, network egress, and data retention before enabling. An MCP server with network access can exfiltrate data regardless of application-level encryption.


## Provider Routing for Sensitive Data

When using AI coding assistants with multiple model providers, sensitive advocacy data (investigation content, witness identities, legal defense materials) must NEVER route through free-tier providers that may retain inputs. Free-tier APIs (Google AI Studio, Groq, Mistral, Cohere, OpenRouter free models, Together AI) may retain inputs for training or compliance — assume they do unless contractually guaranteed otherwise. Route sensitive work exclusively through zero-retention providers or self-hosted inference.
