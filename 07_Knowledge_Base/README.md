# ASOS Knowledge Base

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Architecture and Canonical Contract Governance  
**Applies To:** Canonical Domain Models, Event governance, APIs, Data Schemas, Agent Contracts, Integration Contracts, validation, security, and implementation alignment  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

The ASOS Knowledge Base contains the canonical implementation contracts used to build, integrate, test, operate, and govern the ASOS AI Sales Operating System.

It defines the shared technical and business language used by:

- Domain Services.
- Databases.
- APIs.
- Event Producers and Consumers.
- Workflow and Policy Services.
- Command Orchestration.
- Integration Connectors.
- AI Agents.
- Analytics.
- Security and audit services.
- Engineering, Product, Architecture, and Governance teams.

The Knowledge Base translates approved governance and architecture into implementation-ready contracts.

It must remain:

- Dealership-independent.
- Vendor-neutral.
- Multi-tenant.
- Versioned.
- Evidence-backed.
- Secure.
- Auditable.
- Compatible with the ASOS Constitution.
- Compatible with the System Architecture.
- Compatible with Data Ownership and Systems-of-Record policy.

Dealership-specific targets, thresholds, roles, providers, workflows, approval limits, and automation policies must remain configurable.

---

## 2. Authority Hierarchy

The Knowledge Base operates under the following authority order:

```text
ASOS Constitution
        ↓
Product Vision and System Architecture
        ↓
Data Ownership and Systems of Record
        ↓
Canonical Domain Model and Canonical Event Catalog
        ↓
API, Data, Agent, and Integration Contracts
        ↓
Business Rules, Playbooks, Prompts, and deployment configuration
```

A canonical contract must not override a higher-level governance, ownership, security, Human-authority, or Customer-protection boundary.

Each catalog is authoritative only for its approved responsibility.

---

## 3. Authoritative Scope

This directory is the authoritative location for:

- Canonical Domain Models.
- Canonical Event governance and Event contracts.
- API Contracts.
- Machine-readable Data Schemas.
- AI Agent Contracts.
- Integration Contracts.
- Canonical validation requirements.
- Canonical concurrency and idempotency requirements.
- Canonical security and privacy requirements.
- Canonical evidence and audit requirements.
- Canonical compatibility and migration requirements.

Canonical specifications may reference higher-level documents.

They must not duplicate or redefine them inconsistently.

---

## 4. Current Structure

```text
07_Knowledge_Base/
├── README.md
└── docs/
    ├── README.md
    ├── 01-Domain-Model/
    └── 02-Event-Catalog/
```

Approved current catalogs:

| Catalog | Current baseline | Responsibility |
| :--- | :---: | :--- |
| [Canonical Domain Model](./docs/01-Domain-Model/README.md) | `v1.1.0` Approved Baseline | Canonical Object meaning, ownership, lifecycle, relationships, validation, security, AI boundaries, and authority boundaries. |
| [Canonical Event Catalog Governance](./docs/02-Event-Catalog/README.md) | `v1.0.0` Approved Baseline | Event names and meanings, taxonomy, envelopes, versions, Producers, Consumers, delivery, replay, correction, compatibility, and governance. |

Planned detailed catalogs:

```text
07_Knowledge_Base/docs/03-API-Contracts/
07_Knowledge_Base/docs/04-Data-Schemas/
07_Knowledge_Base/docs/05-Agent-Contracts/
07_Knowledge_Base/docs/06-Integration-Contracts/
```

These planned catalogs are not approved baselines until their governance, scope, contracts, review, and versioning are formally established.

---

## 5. Canonical Domain Model

The approved Canonical Domain Model contains 14 Canonical Domain Objects:

1. Customer.
2. Vehicle.
3. Inventory Record.
4. Lead.
5. Qualified Lead.
6. Opportunity.
7. Appointment.
8. Quotation.
9. Trade-In.
10. Finance Application.
11. Financial Contract.
12. Deal.
13. Interaction.
14. Market Intelligence.

The authoritative Domain Model index is:

[ASOS Canonical Domain Model](./docs/01-Domain-Model/README.md)

The Domain Model is authoritative for:

- Object meaning.
- Canonical ownership.
- Field meaning.
- Relationships.
- Lifecycle intent.
- Validation requirements.
- Authority boundaries.
- Security requirements.
- AI considerations.
- Required separation between Recommendation, Decision, Command, Confirmation, and outcome.

---

## 6. Canonical Event Catalog

The Canonical Event Catalog governance baseline is established and approved.

The authoritative document is:

[ASOS Canonical Event Catalog Governance](./docs/02-Event-Catalog/README.md)

The Event Catalog is authoritative for:

- Event names and meanings.
- Event taxonomy.
- Event versions.
- Event envelopes.
- Event payload contracts.
- Producer responsibilities.
- Consumer responsibilities.
- Authority classification.
- Correlation and causation.
- Delivery semantics.
- Consumer deduplication.
- Replay.
- Corrections.
- Reversals.
- Compatibility.
- Deprecation and retirement.
- Event security and governance.

A Domain Model may describe conceptual Events.

It must not become a competing Event Catalog.

---

## 7. Event Identity and Delivery

The approved Event-producing boundary owns:

- Event creation.
- Canonical `event_id` assignment.
- Event version.
- Tenant context.
- Occurrence time.
- Correlation and causation.
- Authority classification.
- Evidence references.

The Event Backbone owns:

- Validation.
- Routing.
- Delivery.
- Retry.
- Redelivery.
- Dead-letter handling.
- Authorized replay.

The Event Backbone must preserve the Producer-assigned `event_id` unchanged.

It must not silently generate, replace, or rewrite the canonical Event identifier.

Event delivery is at-least-once.

Consumers must detect previously processed `event_id` values before creating duplicate business effects.

---

## 8. Human Authority, AI, and Automation

AI Agents may assist with:

- Classification.
- Extraction.
- Summarization.
- Matching.
- Forecasting.
- Prioritization.
- Risk detection.
- Opportunity detection.
- Drafting.
- Recommendation.
- Explanation.
- Context retrieval.
- Data-quality detection.

AI Agents must not directly execute external actions.

### Action Classes

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

AI confidence, Opportunity stage, priority, Task assignment, draft content, or User-interface state is not execution authority.

---

## 9. Recommendation, Command, and Outcome Separation

Canonical contracts must preserve the following distinctions:

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

Retryable Commands must use stable approved idempotency.

---

## 10. Core Ownership Boundaries

Canonical contracts must preserve one approved owner for each material business concept or mutation workflow.

Examples include:

| Capability or fact | Canonical owner |
| :--- | :--- |
| Customer identity | Customer Domain Service or configured identity authority |
| Vehicle identity and specification | Vehicle Domain Service |
| Inventory intake acceptance | Inventory Domain Service |
| Inventory Record creation and activation | Inventory Domain Service |
| Trade-In appraisal and acquisition workflow | Trade-In Domain Service and configured external authorities |
| Lead qualification outcome | Qualified Lead Domain Service |
| Active Opportunity lifecycle | Opportunity Domain Service or configured CRM authority |
| Customer communication execution | Interaction or Communication Service |
| Appointment lifecycle | Appointment Domain Service |
| Customer-specific commercial offer | Quotation Domain Service |
| Finance application and offer selection | Finance Application Domain Service and lender authority |
| Funding-request mutation and funding workflow | Financial Contract Domain Service |
| Commercial transaction and completion gate | Deal Domain Service |
| Inventory Reservation and Allocation | Inventory Domain Service or approved dedicated service |
| Binding or high-impact Decision | Authorized Human or configured external authority |
| Event creation and canonical `event_id` assignment | Approved Event-producing boundary |
| Event transport and replay | Event and Messaging Backbone |

A non-owning Domain may retain only necessary:

- References.
- Versioned snapshots.
- Read-only projections.
- Freshness indicators.
- Evidence references.
- Reconciliation state.

---

## 11. Catalog Responsibilities

| Catalog | Authoritative responsibility |
| :--- | :--- |
| `01-Domain-Model` | Object meaning, ownership, field meaning, relationships, lifecycle intent, validation, authority, security, and AI boundaries. |
| `02-Event-Catalog` | Event names, meanings, versions, envelopes, payload contracts, Producers, Consumers, compatibility, replay, corrections, and reversals. |
| `03-API-Contracts` | API operations, request and response Schemas, authorization, errors, concurrency, and idempotency. |
| `04-Data-Schemas` | Machine-readable persistence, validation, exchange, and serialization Schemas. |
| `05-Agent-Contracts` | Agent purpose, inputs, outputs, permitted tools, Action Class, approval, evaluation, failure behavior, and escalation. |
| `06-Integration-Contracts` | External mappings, Commands, Confirmations, retries, reconciliation, failure handling, and provider-specific behavior. |

A detailed catalog becomes authoritative for its responsibility only after approval.

---

## 12. Canonical Contract Rules

Canonical documentation must:

- Use consistent Domain Object and field names.
- Use `tenant_id` as the primary Tenant-isolation boundary.
- Define ownership and authority boundaries.
- Separate authoritative facts from projections and Derived Intelligence.
- Preserve evidence and provenance.
- Preserve Human Approval requirements.
- Preserve Action Class boundaries.
- Separate Recommendations from Human Decisions.
- Separate Commands from External Confirmations.
- Treat Events as immutable statements about completed occurrences.
- Treat Event delivery as at-least-once.
- Require Consumer deduplication using `event_id`.
- Require retryable Commands to use `idempotency_key` or an approved equivalent.
- Define concurrency and versioning requirements.
- Define correction, reversal, cancellation, and revocation behavior.
- Define security, privacy, retention, and audit requirements.
- Avoid dealership-specific hard-coded values.
- Avoid conflicting definitions across catalogs.
- Preserve backward compatibility according to the applicable catalog policy.

---

## 13. Out of Scope

The following content must remain in its authoritative repository location:

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and templates | [`../06_Assets`](../06_Assets/) |

Canonical specifications may reference these sources.

They must not redefine their authoritative content inconsistently.

---

## 14. Change Governance

A canonical documentation change must:

- Identify the affected contract.
- Identify the current and proposed version.
- Explain the reason for change.
- Identify affected Domain Objects and catalogs.
- Identify backward-compatibility impact.
- Identify migration impact.
- Identify security and privacy impact.
- Identify AI and Human Approval impact.
- Identify Event, API, Schema, and Integration impact.
- Preserve prior approved versions where required.
- Receive the required governance approval.

Material incompatible changes require a major version.

Backward-compatible additions require a minor version.

Non-semantic corrections may use a patch version.

A repository commit does not automatically make a canonical contract effective in Production.

Controlled approval, migration, testing, release, observability, reconciliation, and rollback remain required.

---

## 15. Governing Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Documentation](./docs/README.md)
- [ASOS Canonical Domain Model](./docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](./docs/02-Event-Catalog/README.md)

---

## 16. Current Status

**Phase:** Approved Domain and Event governance baseline with active canonical-contract expansion  
**Version:** `1.1.0`  
**Status:** Approved Baseline  

Established and approved:

- Canonical Domain Model `v1.1.0`.
- Canonical Event Catalog Governance `v1.0.0`.

Remaining controlled expansion:

- API Contracts.
- Machine-readable Data Schemas.
- Agent Contracts.
- Integration Contracts.

The Knowledge Base remains under active implementation and governance.

Future catalog work must preserve:

- Constitutional authority.
- Dealership independence.
- Tenant isolation.
- Field-level data authority.
- Human accountability.
- Controlled automation.
- Event identity ownership.
- Command and Confirmation truthfulness.
- Security.
- Privacy.
- Evidence.
- Auditability.
- Compatibility.
- Reconciliation.
