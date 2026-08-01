# ASOS Canonical Documentation

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Architecture and Domain Governance  
**Last Updated:** 2026-08-01  

---

## Purpose

This directory contains the canonical implementation contracts for the ASOS AI Sales Operating System.

These contracts establish the shared structures, meanings, authority boundaries, lifecycle requirements, validation controls, security requirements, and integration expectations used by:

- Domain Services.
- Databases.
- APIs.
- Events.
- Schemas.
- Integrations.
- Workflow Engines.
- Policy Engines.
- AI Agents.
- Analytics.
- Security and audit services.

Canonical documentation must remain:

- Vendor-neutral.
- Dealership-independent.
- Multi-tenant.
- Versioned.
- Evidence-backed.
- Human-governed where required.
- Compatible with the ASOS Constitution and System Architecture.

Dealership targets, thresholds, dates, approval limits, branches, Systems of Record, providers, and integrations must remain configurable.

---

## Current Documentation

| Directory | Responsibility | Status |
| :--- | :--- | :--- |
| [`01-Domain-Model`](./01-Domain-Model/) | Defines the canonical ASOS Domain Objects, ownership boundaries, relationships, lifecycles, validation, security, authority, AI boundaries, and audit requirements. | Approved Baseline `v1.1.0` |

The Canonical Domain Model currently contains 14 approved core Domain Objects.

---

## Canonical Domain Objects

1. [Customer](./01-Domain-Model/Customer.md)
2. [Vehicle](./01-Domain-Model/Vehicle.md)
3. [Inventory Record](./01-Domain-Model/InventoryRecord.md)
4. [Lead](./01-Domain-Model/Lead.md)
5. [Qualified Lead](./01-Domain-Model/QualifiedLead.md)
6. [Opportunity](./01-Domain-Model/Opportunity.md)
7. [Appointment](./01-Domain-Model/Appointment.md)
8. [Quotation](./01-Domain-Model/Quotation.md)
9. [Trade-In](./01-Domain-Model/TradeIn.md)
10. [Finance Application](./01-Domain-Model/FinanceApplication.md)
11. [Financial Contract](./01-Domain-Model/FinancialContract.md)
12. [Deal](./01-Domain-Model/Deal.md)
13. [Interaction](./01-Domain-Model/Interaction.md)
14. [Market Intelligence](./01-Domain-Model/MarketIntelligence.md)

The authoritative Domain Model index is:

[ASOS Canonical Domain Model](./01-Domain-Model/README.md)

---

## Canonical Documentation Roadmap

```text
docs/
├── 01-Domain-Model/
├── 02-Event-Catalog/
├── 03-API-Contracts/
├── 04-Data-Schemas/
├── 05-Agent-Contracts/
└── 06-Integration-Contracts/
```

### Current Phase

```text
01-Domain-Model
Status: Approved Baseline
Version: 1.1.0
```

### Next Controlled Phase

```text
02-Event-Catalog
Status: Governance Definition Required
```

The Event Catalog directory must not be created until its:

- Purpose.
- Scope.
- Event taxonomy.
- Naming rules.
- Versioning rules.
- Envelope standard.
- Producer and consumer responsibilities.
- Compatibility policy.
- Correction and reversal policy.
- Idempotency and deduplication rules.
- Security requirements.
- Governance and approval workflow.

have been formally defined and approved.

The remaining catalogs must follow the same controlled establishment process.

---

## Catalog Authority Boundaries

Each canonical catalog has a distinct authority.

| Catalog | Authoritative Responsibility |
| :--- | :--- |
| `01-Domain-Model` | Object meaning, ownership, field meaning, relationships, lifecycle intent, validation, authority, security, and AI boundaries |
| `02-Event-Catalog` | Event names, versions, envelopes, payload contracts, producers, consumers, compatibility, corrections, and reversals |
| `03-API-Contracts` | API operations, request and response Schemas, authorization requirements, errors, concurrency, and idempotency |
| `04-Data-Schemas` | Machine-readable persistence, validation, exchange, and serialization Schemas |
| `05-Agent-Contracts` | Agent purpose, inputs, outputs, permitted tools, authority boundaries, evaluations, and escalation requirements |
| `06-Integration-Contracts` | External-system mappings, Commands, Confirmations, retries, reconciliation, and failure handling |

A Domain Model may describe conceptual Events, API behaviour, and storage expectations.

The applicable detailed catalog becomes authoritative once approved.

---

## Core Contract Rules

Canonical documentation must:

- Use consistent Domain Object and field names.
- Use `tenant_id` as the primary Tenant-isolation boundary.
- Define ownership and authority boundaries.
- Separate authoritative facts from projections and Derived Intelligence.
- Preserve evidence and provenance.
- Preserve Human Approval requirements.
- Separate Recommendations from Human Decisions.
- Separate Commands from External Confirmations.
- Treat Events as immutable facts about completed occurrences.
- Treat Event delivery as at-least-once.
- Require Event Consumers to prevent duplicate business effects using `event_id`.
- Require retryable Commands to use `idempotency_key`.
- Define concurrency and versioning requirements.
- Define security, privacy, retention, and audit requirements.
- Avoid dealership-specific hard-coded values.
- Avoid conflicting definitions across catalogs.
- Preserve backward compatibility according to the applicable catalog policy.

---

## Evidence and Authority Model

Canonical contracts must distinguish:

```text
Source Evidence
Observation
Canonical Projection
ASOS Authoritative Workflow State
Derived Intelligence
Recommendation
Authoritative Human Decision
Command
External Confirmation
```

These concepts must not be silently merged.

Examples:

```text
Recommendation
  ≠ Human Decision

Human Decision
  ≠ Command execution

Command accepted
  ≠ External completion

Provider acknowledgement
  ≠ Business outcome

AI prediction
  ≠ Authoritative fact
```

---

## Change Governance

A canonical documentation change must:

- Identify the affected contract.
- Identify the current and proposed version.
- Explain the reason for change.
- Identify affected Domain Objects and catalogs.
- Identify backward-compatibility impact.
- Identify migration impact.
- Identify security and privacy impact.
- Identify AI and Human Approval impact.
- Preserve prior approved versions.
- Receive the required governance approval.

Material incompatible changes require a major version.

Backward-compatible additions require a minor version.

Non-semantic corrections may use a patch version.

---

## Content Boundaries

This directory must not duplicate the complete authoritative content of:

| Content Type | Authoritative Location |
| :--- | :--- |
| Constitutional principles | [`../../00_Constitution`](../../00_Constitution/) |
| Operational Playbooks | [`../../01_Playbooks`](../../01_Playbooks/) |
| Business Rules | [`../../02_Business_Rules`](../../02_Business_Rules/) |
| Production and evaluation Prompts | [`../../03_Prompts`](../../03_Prompts/) |
| Reference data and safe examples | [`../../04_Data`](../../04_Data/) |
| Architecture and governance documentation | [`../../05_Documentation`](../../05_Documentation/) |
| Images, diagrams, and templates | [`../../06_Assets`](../../06_Assets/) |

Canonical contracts may reference these sources but must not redefine their authoritative content.

---

## Governing Documents

- [ASOS Constitution](../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Domain Model](./01-Domain-Model/README.md)

---

## Status

**Phase:** Canonical Domain Baseline Complete  
**Version:** `1.1.0`  
**Status:** Approved Baseline  
**Next Controlled Phase:** Event Catalog Governance  
