---
paths:
  - "**/data/**"
  - "**/user/**"
  - "**/profile/**"
  - "**/*pii*"
---
# Privacy Rules for Animal Advocacy Projects

Privacy in advocacy software is not a compliance checkbox — it is the difference between operational security and activist prosecution. Data that seems harmless in isolation becomes evidence under ag-gag statutes: participation timestamps, IP addresses, device fingerprints, and access patterns can identify investigators, witnesses, and rescue coordinators. Every privacy decision must be made against the worst-case legal scenario, not the average one.

## Data Minimization as Default Architecture

Collect the absolute minimum data required for each function. Do not collect data "in case we need it later." Every data point stored is a data point that can be subpoenaed, seized, or leaked. Before adding any field to any data model, ask: **if this data appeared in a court filing, who would it endanger?** If the answer is anyone, justify its existence or eliminate it.

## Activist Identity Protection

Activist identities are the highest-sensitivity data category. Use pseudonymous identifiers internally. Never store legal names alongside action records. Separate authentication identity from operational identity — the system that verifies login credentials must not be the system that records who participated in which investigation. Compartmentalization is the structural principle: compromise of one system must not cascade into identification across systems.

## GDPR/CCPA Compliance as Floor, Not Ceiling

Regulatory compliance is the minimum standard. Advocacy software should exceed it. Implement the full data subject rights: access, rectification, erasure, portability, and objection. Right to deletion MUST be real deletion — not soft delete with a `deleted_at` flag. When an activist requests erasure, their data must be irrecoverable from all storage layers including backups, replicas, search indices, analytics pipelines, and log aggregation systems. Soft delete in advocacy software is a liability: "deleted" records surfacing in legal discovery destroy trust and endanger people.

## Consent as Ongoing Process

Consent is not a one-time checkbox at registration. Implement re-consent workflows for scope changes (new coalition partner gets data access, new feature collects additional data, investigation footage is shared with a new organization). Provide granular consent controls: participation in a public campaign does not imply consent to be recorded as an investigation participant. Withdrawal of consent must be as easy as granting it, with immediate effect.

## Coalition Data Sharing Across Risk Profiles

Different advocacy organizations operate at different risk levels. A grassroots direct action group, a legal defense fund, and a public education nonprofit have fundamentally different threat models. When sharing data across coalition boundaries: (1) classify each partner's risk level, (2) apply the strictest data handling rules of any partner in the exchange, (3) implement data transformation at boundaries — strip identifying information before sharing across risk tiers, (4) maintain audit trails that themselves do not create new identification vectors, (5) design data sharing agreements that specify what happens to shared data when a partner is compromised or legally compelled to disclose.

## Whistleblower and Witness Protection

Whistleblower identities require the strongest protections in the system. Implement: end-to-end encryption for all whistleblower communications, no server-side access to decrypted content, anonymous submission channels that do not require account creation, zero-knowledge architectures where even system administrators cannot identify whistleblowers. Witness testimony records must have consent verification before any display, anonymization by default, and explicit opt-in for any identifiable presentation.

## Investigation Participant Records

Records of who participated in undercover investigations are the most legally dangerous data in the system. Store these records in maximally encrypted, compartmentalized storage with access limited to the minimum number of people. Consider whether these records need to exist at all — if the operational need can be met without a persistent record, do not create one. When records must exist, design them with plausible deniability: the storage system should not reveal whether it contains investigation records without the correct credentials.

## Anonymization Requirements

Anonymization must be irreversible. AI-generated anonymization is often superficial — replacing names while leaving uniquely identifying combinations of attributes (location + date + role + demographic data). True anonymization requires k-anonymity at minimum: no individual should be distinguishable from at least k-1 others in any released dataset. Test anonymization by attempting re-identification with publicly available information.
