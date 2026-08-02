# ASOS Data

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Data Governance, Architecture, Security, and Product  
**Applies To:** Reference data, mappings, synthetic examples, test fixtures, evaluation datasets, non-Production datasets, data-quality samples, lookup tables, and controlled data documentation stored in this directory  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory is the governed location for approved ASOS reference data, mappings, synthetic examples, test fixtures, evaluation datasets, and other non-Production data assets.

Data assets may support product design, Domain and Event contract validation, integration development, Business Rule testing, Playbook testing, Prompt and Agent evaluation, demonstrations, controlled Pilot preparation, regression testing, reconciliation testing, and security or privacy testing.

This directory must not become an uncontrolled copy of dealership Production systems.

A data file must not be treated as authoritative merely because it exists in the repository.

---

## 2. Current Baseline

The current approved baseline establishes governance, classification, security, provenance, quality, release, and change-control requirements for ASOS Data assets.

No individual Data asset is designated by this README as an approved Production dataset.

An individual asset becomes eligible for approved use only after its purpose, owner, classification, source, provenance, Tenant scope, environment scope, permitted uses, prohibited uses, structure, sensitive-data treatment, rights, quality checks, retention, version, status, and approval evidence are defined.

A file existing in this directory does not automatically make it approved, safe, representative, complete, authoritative, or Production-ready.

---

## 3. Authority Hierarchy

```text
Applicable legal and regulatory requirements
        ↓
Binding contractual, OEM, lender, and governmental requirements
        ↓
ASOS Constitution
        ↓
Product, architecture, security, privacy, and data-governance policies
        ↓
Canonical Domain Models, Events, APIs, Schemas, and Integration Contracts
        ↓
Approved Business Rules
        ↓
Approved operational Playbooks
        ↓
Approved Prompt and Agent governance
        ↓
Approved Data asset documentation
        ↓
Deployment configuration and individual examples
```

A Data asset must not override a higher-level authority.

A conflicting asset must be blocked from unsafe use, reviewed, corrected, superseded, or retired.

---

## 4. Permitted Data Asset Types

This directory may contain governed versions of:

- Reference data and lookup tables.
- Canonical-to-external mappings.
- Provider, market, or jurisdiction mappings where permitted.
- Synthetic Customer, Lead, Vehicle, Inventory, Trade-In, Appointment, Quotation, Finance, and Deal records.
- Test fixtures and golden cases.
- Prompt, Agent, Rule, and workflow evaluation datasets.
- Event and integration payload examples.
- Reconciliation and data-quality examples.
- Redacted demonstration data.
- Non-sensitive sample exports.
- Dataset manifests and checksums.

Every asset must have a defined purpose and owner.

---

## 5. Prohibited Content

This directory must not contain uncontrolled or unnecessary copies of:

- Live Production databases or raw dealership exports.
- Customer lists or unredacted Customer communications.
- Government identifiers, payment-card data, bank-account data, or credit reports.
- Passwords, private keys, access tokens, API secrets, session cookies, or Production connection strings.
- Unrestricted logs, signed documents, or legal records containing unnecessary personal data.
- Unlicensed third-party datasets.
- Scraped data without approved rights and purpose.
- Cross-Tenant combined datasets without explicit authorization and controls.
- Model outputs represented as authoritative facts.
- Generated examples that could be mistaken for real Customer records.

Production secrets must not be stored in Data files.

Sensitive Production data must not be committed for convenience, debugging, demonstration, or model evaluation.

---

## 6. Data Asset Categories

| Category | Purpose |
| :--- | :--- |
| Reference Data | Stable approved values used for lookup, validation, display, or mapping. |
| Mapping Data | Controlled relationships between canonical and external values. |
| Synthetic Dataset | Artificial records that do not represent real individuals or transactions. |
| Test Fixture | Minimal deterministic input used to test a defined behavior. |
| Golden Dataset | Versioned expected inputs and outputs used for regression evaluation. |
| Evaluation Dataset | Cases used to assess a model, Prompt, Agent, Rule, or workflow. |
| Demonstration Dataset | Safe examples used for demos, training, or documentation. |
| Redacted Sample | Source-derived content processed under an approved redaction method. |
| Data-Quality Dataset | Missing, stale, duplicate, conflicting, malformed, or invalid cases. |
| Reconciliation Dataset | Cases used to test authority, synchronization, conflict resolution, and confirmation. |
| Manifest | Metadata describing source, classification, version, quality, rights, and integrity. |

---

## 7. Data Classification

Every asset must have an explicit classification.

| Classification | Meaning |
| :--- | :--- |
| Public | Approved for public disclosure. |
| Internal | Non-public ASOS information with limited sensitivity. |
| Confidential | Sensitive business, Customer, dealership, provider, or operational information. |
| Restricted | Highly sensitive information requiring strict access and handling controls. |
| Synthetic | Artificial information that does not represent a real person or transaction. |
| Redacted | Source-derived information processed to remove or mask approved sensitive elements. |

`Synthetic` and `Redacted` describe origin or treatment; they do not automatically define access level.

---

## 8. Production and Non-Production Separation

This directory is intended for governed non-Production assets.

Production data must remain in approved Production systems, stores, backups, and controlled export locations.

Moving source-derived data into this repository requires an approved purpose, owner, legal and contractual basis, Tenant scope, minimum field set, redaction or transformation method, access control, retention period, deletion method, and audit evidence.

A lower environment must not receive Production data by default.

Production-derived data must not be copied into Development, Test, Demonstration, or Evaluation environments unless an approved process confirms that the copy is necessary, minimized, protected, and permitted.

---

## 9. Synthetic Data Requirements

Synthetic data must be generated so that it does not represent or allow reconstruction of a real individual, dealership transaction, credential, account, or confidential record.

Synthetic assets must:

- Use fictional identities.
- Use non-routable or reserved contact values where possible.
- Avoid real government identifiers, payment information, and account numbers.
- Avoid live credentials.
- Avoid rare combinations that identify a real person.
- Mark records clearly as synthetic.
- Preserve only the structural and behavioral properties required for the test.
- Document the generation method and known limitations.

Synthetic data must not be described as anonymized Production data unless an approved anonymization assessment supports that claim.

---

## 10. Redaction, Pseudonymization, and Anonymization

These terms must not be used interchangeably.

**Redaction** removes or masks selected values from source-derived content. Redacted data may remain identifiable through other fields or external linkage.

**Pseudonymization** replaces direct identifiers with controlled substitutes. Pseudonymized data remains personal or sensitive where re-identification remains reasonably possible.

**Anonymization** is an approved transformation intended to make identification not reasonably possible under the applicable standard and context.

Removing names alone is not sufficient to establish anonymization.

---

## 11. Tenant Isolation

Every Data asset must define its Tenant scope.

Allowed scopes include global synthetic reference, platform-wide public reference, single Tenant, single dealership, single branch, approved dealer group, or explicitly authorized cross-Tenant aggregate.

Tenant data must not be combined merely because schemas are compatible.

Cross-Tenant aggregation requires an approved purpose, legal and contractual authority, minimum necessary fields, aggregation or privacy controls, access controls, re-identification risk review, and audit evidence.

A filename, folder, Prompt, query, or User request must not expand Tenant scope.

---

## 12. Data Ownership and Systems of Record

Every field in a source-derived asset must retain a documented relationship to its authority.

A Data asset may contain:

- Source Evidence.
- Observation.
- Canonical Projection.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Recommendation.
- Authoritative Human Decision.
- Command state.
- External Confirmation.

These states must remain distinguishable.

A Canonical Projection does not automatically replace the configured External System of Record.

A fixture or example does not become an authoritative business fact.

Derived Intelligence must not be labeled as source evidence.

---

## 13. Required Asset Manifest

Each material Data asset must have a manifest in the file itself, an adjacent metadata file, or a governed catalog.

The manifest must define:

- Asset ID, title, version, status, owner, and steward.
- Category and classification.
- Purpose, permitted uses, and prohibited uses.
- Source type, source owner, and provenance.
- Tenant, environment, and jurisdiction scope.
- Schema or structure reference.
- Generation or extraction date.
- Freshness expectations.
- Sensitive fields and their treatment.
- Licensing and usage rights.
- Retention and deletion requirements.
- Quality checks and known limitations.
- Integrity checksum where required.
- Approval evidence and change history.

An undocumented asset must not be promoted to governed use.

---

## 14. Asset IDs and Naming

Recommended Asset ID format:

```text
DAT-<DOMAIN>-<PURPOSE>-<NUMBER>
```

Examples:

```text
DAT-LEAD-QUALIFICATION-001
DAT-VEHICLE-REFERENCE-001
DAT-INVENTORY-RECONCILIATION-001
DAT-TRADEIN-VALUATION-001
DAT-FINANCE-OFFER-EVALUATION-001
DAT-DEAL-READINESS-001
```

Recommended file naming:

```text
<Asset-ID>_<Short-Name>_<Version>.<extension>
```

IDs must remain stable across compatible revisions.

A materially different purpose or population should receive a new Asset ID.

---

## 15. File Formats

| Format | Typical use |
| :--- | :--- |
| `.csv` | Simple tabular reference data with documented encoding and delimiters. |
| `.json` | Structured examples, mappings, and bounded fixtures. |
| `.jsonl` | Record-oriented evaluation and test datasets. |
| `.yaml` | Human-maintained mappings or configuration examples where ambiguity is controlled. |
| `.parquet` | Larger analytical non-Production datasets where approved tooling exists. |
| `.md` | Manifests, data dictionaries, examples, and human-readable guidance. |
| `.txt` | Plain-text evaluation input with explicit encoding and structure. |

Text assets should use UTF-8 unless another encoding is explicitly required.

Dates, times, currencies, units, precision, and null behavior must be explicit.

---

## 16. Canonical Contract Alignment

Data assets must align with approved canonical contracts.

A Domain fixture must reference the applicable Canonical Domain Object and version.

An Event fixture must reference the applicable Event definition and version.

An API, Schema, or Integration fixture must reference its applicable approved contract and version.

Examples must not silently introduce undocumented fields, competing enumerations, new ownership rules, new authority paths, new completion semantics, or new Event identity behavior.

Intentionally invalid examples must identify the invalid condition and expected handling.

---

## 17. Event Data Requirements

Event examples must follow the approved Event Catalog governance.

The approved Event-producing boundary owns:

- Event creation.
- Canonical `event_id` assignment.
- Event version.
- Tenant context.
- Occurrence time.
- Correlation and causation.
- Authority classification.
- Evidence references.

The Event Backbone must preserve the Producer-assigned `event_id` unchanged.

Event delivery is at-least-once.

Consumer fixtures must test duplicate delivery and prevent duplicate business effects using `event_id`.

Replay examples must preserve the original canonical Event identity and must not imply that replay creates a new business occurrence.

---

## 18. Recommendation, Command, and Confirmation Data

Data assets must preserve the distinction between:

```text
Recommendation generated
  ≠ Action authorized

Action authorized
  ≠ Command created

Command created
  ≠ Command sent

Command sent
  ≠ External system accepted

External system accepted
  ≠ Business outcome completed

External Confirmation received
  ≠ Canonical outcome accepted until validation and reconciliation
```

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

Fixtures must not label an unconfirmed request as a completed external action.

---

## 19. Human Authority and Action Classes

Data examples must preserve the approved Action Classes.

```text
Action Class 0
  = Analysis only

Action Class 1
  = Internal and reversible operation

Action Class 2
  = Controlled Customer-facing or external operation

Action Class 3
  = Binding or high-impact Decision
```

Every Action Class 2 execution requires exactly one valid authority path:

- Explicit Human Approval for the exact action instance; or
- An active pre-approved automation policy covering the exact action instance.

No third path exists.

Action Class 3 requires an Authoritative Human Decision.

A fixture must not represent AI confidence, stage, priority, Task assignment, draft content, or User-interface state as execution authority.

---

## 20. Reference Data Governance

Reference data must define:

- Business meaning and owner.
- Allowed codes and display labels.
- Effective and retirement dates where required.
- Compatibility behavior.
- Unknown or unmapped behavior.
- Tenant or market scope.
- Source authority.

Reference values must not be changed in place when the change would alter historical meaning.

Retired values should remain interpretable for historical records where required.

Unknown external values must not be silently coerced into an unrelated canonical value.

---

## 21. Mapping Governance

Mappings between canonical and external values must be explicit, versioned, and directional where direction matters.

Each mapping must define the canonical value, external system, external value, direction, Tenant or deployment scope, effective date, fallback behavior, unmapped behavior, and owner.

A mapping must not imply that ASOS owns the external value.

Ambiguous many-to-one or one-to-many mappings must define reconciliation and Human Review behavior.

---

## 22. Data Quality Requirements

Applicable checks may include:

- Schema conformance.
- Required-field completeness.
- Type and enumeration validation.
- Referential integrity.
- Uniqueness and duplicate detection.
- Range, currency, unit, timestamp, and time-zone validation.
- Freshness and provenance completeness.
- Tenant-scope validation.
- Sensitive-data and secret scanning.
- Expected record count and checksum validation.

Quality failures must produce explicit results.

A failing asset must not be silently promoted.

---

## 23. Missing, Stale, and Conflicting Data

Fixtures should include defined cases for missing evidence, null or unknown values, stale data, conflicting sources, duplicates, out-of-order Events, late Confirmations, partial responses, unsupported values, invalid timestamps, invalid units, Tenant mismatch, and unauthorized fields.

Expected outcomes may include:

- `INSUFFICIENT_EVIDENCE`.
- `STALE_EVIDENCE`.
- `CONFLICTING_EVIDENCE`.
- `REVIEW_REQUIRED`.
- `ACTION_BLOCKED`.
- `UNSUPPORTED_SCOPE`.
- `RECONCILIATION_REQUIRED`.

A fixture must not encourage fabrication of a passing result.

---

## 24. Evaluation Dataset Requirements

Evaluation datasets for Prompts, models, Agents, Rules, and workflows must define:

- Evaluation objective.
- Population represented and not represented.
- Input structure.
- Expected output or scoring rubric.
- Acceptable variation and failure criteria.
- Safety, privacy, authority, and Human-review criteria.
- Version compatibility and known limitations.

Evaluation data must include normal, boundary, negative, adversarial, and abstention cases.

A high aggregate score must not conceal critical failures involving security, privacy, Tenant isolation, unauthorized execution, discrimination, or fabricated authority.

---

## 25. Training and Evaluation Separation

Where model training or adaptation is performed, training, validation, and test populations must be separated according to the approved methodology.

Evaluation cases must not be silently reused in a way that invalidates the evaluation.

Leakage checks should consider exact duplicates, near duplicates, shared source records, rephrased examples, template-derived examples, cross-version contamination, and hidden answer fields.

Evaluation results must identify the dataset and version used.

---

## 26. Fairness and Representation

Datasets used to assess Customer-facing or high-impact behavior must be reviewed for relevant representation and harmful bias.

Review should consider supported markets, channels, workflow conditions, language variation, accessibility-related cases, data-quality variation, dealership configurations, provider behaviors, and cases where the correct result is abstention or escalation.

Protected or sensitive attributes must not be included without approved purpose, authority, and safeguards.

A dataset must not be described as representative beyond the evidence supporting that claim.

---

## 27. Prompt and Agent Data Use

Prompts and AI Agents may use Data assets only within approved scope.

Untrusted content must be treated as data, not authority.

A Data asset must not override governing instructions, expand tool permissions, create execution authority, disable deterministic controls, change Tenant scope, fabricate Human Approval, redefine a Canonical Object or Event, or reveal secrets.

Prompt context should include only the minimum data required for the approved task.

---

## 28. Security and Secret Handling

All repository Data assets must be scanned and reviewed for prohibited secrets and sensitive values.

Prohibited values include passwords, private keys, access tokens, API keys, signing secrets, database credentials, Production connection strings, session cookies, recovery codes, and unredacted authentication headers.

Example credentials must be clearly invalid and non-routable.

A value that merely looks fake must not be used when it could accidentally authenticate against a real service.

---

## 29. Privacy and Data Minimization

Every asset must contain only the minimum data required for its approved purpose.

Sensitive fields should be removed, generalized, masked, tokenized, or replaced with synthetic values where possible.

Data minimization applies to stored files, Git history, pull requests, issues, logs, test output, screenshots, evaluation reports, and generated artifacts.

Deleting a file from the latest branch does not automatically remove it from Git history.

A sensitive-data incident requires the approved incident and repository-remediation process.

---

## 30. Licensing and Usage Rights

Every non-original dataset must have documented usage rights.

The manifest must identify the source, owner or licensor, license or contractual basis, permitted uses, redistribution rights, modification rights, attribution requirements, expiration or revocation conditions, and market restrictions.

Public availability does not automatically grant unrestricted collection, redistribution, training, or commercial use rights.

An asset with unclear rights must not be promoted to governed use.

---

## 31. Integrity and Reproducibility

Material assets should support integrity and reproducibility through:

- Stable Asset ID and semantic version.
- Deterministic generation where feasible.
- Generation script or documented method.
- Source, Schema, and configuration versions.
- Record count and checksum.
- Validation report and change history.

Generated datasets should not be manually edited without either regenerating them or documenting the controlled exception.

---

## 32. Versioning and Status

Data assets use semantic versioning where practical:

- Major for incompatible meaning, structure, population, authority, or evaluation changes.
- Minor for backward-compatible additions or material coverage improvements.
- Patch for non-semantic corrections that do not alter intended interpretation.

A dataset version must be immutable once used for a recorded evaluation, release decision, or audit record.

Approved status values include:

- `Draft`.
- `Under Review`.
- `Approved Baseline`.
- `Evaluation Approved`.
- `Pilot Approved`.
- `Production Reference Approved`.
- `Suspended`.
- `Superseded`.
- `Retired`.

A Data asset status must not imply broader model, Prompt, Rule, Playbook, Pilot, or Production approval than was actually granted.

---

## 33. Release and Promotion

Before promotion, an asset must complete applicable owner, Data Governance, Security, Privacy, legal, contractual, licensing, quality, Schema, sensitive-data, secret, Tenant-scope, testing, and documentation review.

A repository commit does not automatically make a Data asset effective in Production.

Production effectiveness also requires applicable deployment, access, configuration, and effective-date controls.

---

## 34. Change Control

Every material change must identify:

- Reason for change.
- Affected Asset ID.
- Prior and new versions.
- Changed fields, population, meaning, or expected outputs.
- Compatibility, privacy, security, licensing, and evaluation impact.
- Migration requirements.
- Rollback or supersession plan.
- Approval evidence.

Silent replacement of a dataset used for recorded evaluation is prohibited.

---

## 35. Retention, Deletion, and Access

Each asset must define a retention period or retention rule considering legal, contractual, audit, reproducibility, security, privacy, and source-owner requirements.

Expired or unauthorized assets must be deleted or archived through an approved process.

Deletion must consider repository history, forks, artifacts, caches, backups, and local copies.

Repository access does not automatically authorize every use of every Data asset.

Access must follow least privilege, purpose limitation, Tenant scope, environment scope, classification, role, permission, contractual restrictions, and export restrictions.

---

## 36. Auditability

Material Data activity should preserve, where applicable:

- Asset ID and version.
- User or service identity.
- Purpose and source.
- Tenant scope.
- Access decision.
- Transformation, redaction, or generation method.
- Validation result.
- Approval and release status.
- Evaluation use.
- Export or sharing record.
- Retention or deletion action.
- Timestamp.

Audit records must minimize unnecessary sensitive content.

---

## 37. Recommended Directory Organization

```text
04_Data/
├── README.md
├── reference/
├── mappings/
├── synthetic/
├── fixtures/
├── evaluations/
├── reconciliation/
├── data-quality/
├── demonstrations/
└── manifests/
```

Subdirectories should be created only when governed assets exist.

An empty directory structure is not required.

Each material subdirectory should contain an index.

---

## 38. Content Boundaries

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Prompt and Agent instructions | [`../03_Prompts`](../03_Prompts/) |
| Product, architecture, data-governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and reusable visual templates | [`../06_Assets`](../06_Assets/) |
| Canonical Domain, Event, API, Schema, Agent, and Integration contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

This directory may store examples conforming to those contracts.

It must not redefine the contracts themselves.

---

## 39. Related Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md)

---

## 40. Current Status

**Phase:** Governed Data foundation with controlled asset introduction  
**Version:** `1.0.0`  
**Status:** Approved Baseline  

The current baseline defines how ASOS Data assets must be classified, documented, secured, validated, versioned, approved, and retired.

It does not approve an individual dataset for Production use.

Future work must preserve:

- Data authority and source provenance.
- Tenant isolation and environment separation.
- Data minimization, security, and privacy.
- Licensing and contractual rights.
- Canonical contract alignment.
- Deterministic validation.
- Evaluation integrity.
- Auditability and reproducibility.
- Controlled change.
