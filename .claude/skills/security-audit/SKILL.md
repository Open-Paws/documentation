---
name: security-audit
description: Security audit workflow for advocacy projects — dependency verification, zero-retention compliance, slopsquatting defense, encrypted storage, instruction file integrity, device seizure readiness, ag-gag exposure assessment
---
# Security Audit

## When to Use
- Before deploying any change to production
- When new dependencies are added
- When code touches investigation data, witness identities, or coalition coordination
- After AI-generated code has been added to security-sensitive paths
- Periodically as a scheduled review of the full codebase

## Process

### Step 1: Dependency Audit — Slopsquatting Defense
Approximately 20% of AI-recommended packages do not exist. Attackers register these hallucinated names as real packages containing malicious code — one was downloaded 30,000+ times in weeks. For EVERY dependency: verify the package exists in its registry, has legitimate maintainers with real commit history, and the version is published. Only 1 in 5 AI-recommended dependency versions are both safe and free from hallucination. In advocacy software, a compromised dependency can exfiltrate investigation data or activist identities.

### Step 2: API Retention Policy Audit
For every external API: verify the data retention policy contractually, not by assumption. Confirm zero-retention configuration is in effect for all sensitive data flows. Check whether the API retains inputs, logs request metadata, or stores conversation history. Telemetry to third parties is a data exfiltration vector under adversarial legal discovery. Any API handling investigation footage, witness identities, or activist communications must be zero-retention.

### Step 3: Storage Encryption Audit
Verify all locally stored investigation data, evidence, and activist records use encrypted volumes with plausible deniability — the existence of sensitive data must be deniable under device seizure. Check for temporary files, swap files, crash dumps, or caches containing decrypted content. Verify encryption keys are not stored alongside encrypted data. Test: if the device powers off unexpectedly, is any sensitive data recoverable without credentials?

### Step 4: Input Validation Review
AI-generated code contains OWASP Top 10 vulnerabilities in 45% of cases — 2.74x more than human code. For every input boundary:
- Verify SQL injection defenses on all database-facing inputs
- Verify XSS protection on all content display paths, especially witness testimony and investigation notes
- Verify path traversal protection on evidence file uploads
- Verify authentication and authorization checks on every endpoint
- Assume adversarial input on every public-facing surface — industry actors will probe investigation tools

### Step 5: Instruction File Integrity Check
The "Rules File Backdoor" attack uses hidden Unicode characters in instruction files to inject invisible directives that make AI agents produce malicious output. Inspect all instruction files for non-printable characters beyond standard whitespace. Diff changes character-by-character, not just visually. Verify no instruction file weakens encryption, disables safety checks, or sends data to external endpoints. Treat instruction files as security-critical artifacts — in advocacy projects, a compromised instruction file could direct the AI to leak investigation data.

### Step 6: MCP Server Audit
For every MCP server: verify servers handling sensitive advocacy data are self-hosted. Audit each server's data access patterns, network egress, and data retention. MCP extends agent capabilities but also extends the attack surface — check whether any server can exfiltrate data regardless of application-level encryption.

### Step 7: Device Seizure Readiness
Verify remote wipe capability exists for all sensitive data. Verify encrypted volumes lock automatically on suspicious conditions (unexpected power loss, extended inactivity). Check that the application does not leak data on unexpected termination — no temp files with decrypted content, no swap files with sensitive state, no crash dumps with investigation data. Test: kill the process unexpectedly and examine what remains on disk.

### Step 8: Ag-Gag Exposure Assessment
Investigation footage is discoverable evidence under legal proceedings:
- Audit every data flow assuming adversarial legal discovery, not just adversarial hackers
- Verify metadata stripping on all investigation content (timestamps, geolocation, device identifiers)
- Verify audit logs protect the identities they record — logs identifying who accessed investigation data become prosecution tools
- Check: if a court subpoena targeted this system, what would be disclosed? Minimize that surface.

### Step 9: Coalition Data Boundary Verification
- Verify data isolation between coalition partners with different risk profiles
- Verify anti-corruption layers exist at every boundary crossing between bounded contexts
- Verify data sharing agreements are enforced in code, not just in policy documents
- Check: if one coalition partner is legally compelled to disclose, what is the blast radius to other partners?
- Verify shared data has been transformed appropriately — strip identifying information before sharing across risk tiers

### Step 10: Findings Report
Document findings with severity classification:
- **Critical** — active data leak, missing encryption, compromised dependency, exposed witness identity
- **High** — weak input validation, missing zero-retention verification, unaudited MCP server
- **Medium** — incomplete metadata stripping, untested seizure scenario, missing contract tests at boundaries
- **Low** — documentation gaps, minor configuration improvements

Block deployment on any Critical or High finding. Track all findings to resolution.
