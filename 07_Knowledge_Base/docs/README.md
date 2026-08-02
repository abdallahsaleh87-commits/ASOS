# ASOS Canonical Documentation

**Version:** 1.2.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Architecture and Canonical Contract Governance  
**Applies To:** Canonical Domain Models, Event governance, API Contracts, Data Schemas, Agent Contracts, Integration Contracts, validation, security, compatibility, and implementation alignment  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory contains the canonical implementation contracts for the ASOS AI Sales Operating System.

These contracts establish the shared meanings, ownership boundaries, lifecycle requirements, Event semantics, validation controls, security requirements, compatibility rules, and integration expectations used by:

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

Canonical documentation translates approved governance and architecture into implementation-ready contracts.

It must remain:

- Dealership-independent.
- Vendor-neutral.
- Multi-tenant.
- Versioned.
- Evidence-backed.
- Secure.
- Auditable.
- Compatible with the ASOS Constitution.
- Compatible with the Product Vision.
- Compatible with the System Architecture.
- Compatible with Data Ownership and Systems-of-Record policy.

Dealership-specific targets, thresholds, roles, providers, workflows, approval limits, and automation policies must remain configurable.

---

## 2. Authority Hierarchy

Canonical documentation operates under the following authority order:

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

## 3. Current Canonical Catalogs

| Directory | Current baseline | Authoritative responsibility |
| :--- | :---: | :--- |
| [`01-Domain-Model`](./01-Domain-Model/) | `v1.1.0` Approved Baseline | Canonical Object meaning, ownership, field meaning, lifecycle, relationships, validation, security, AI boundaries, and authority boundaries. |
| [`02-Event-Catalog`](./02-Event-Catalog/) | `v1.0.0` Approved Baseline | Event names and meanings, taxonomy, envelopes, versions, Producers, Consumers, delivery, replay, corrections, reversals, compatibility, security, and governance. |

The Canonical Domain Model and Canonical Event Catalog governance baseline are established and approved.

The following detailed catalogs remain to be established:

```text
03-API-Contracts/
04-Data-Schemas/
05-Agent-Contracts/
06-Integration-Contracts/
```

These future catalogs do not become approved baselines merely because a directory or draft exists.

They require:

- Defined scope.
- Defined authority.
- Versioning rules.
- Security requirements.
- Compatibility rules.
- Review.
- Approval.
- Migration planning where applicable.

---

## 4. Canonical Domain Model

The approved Canonical Domain Model contains 14 Canonical Domain Objects:

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

The Domain Model is authoritative for:

- Object meaning.
- Canonical ownership.
- Field meaning.
- Relationships.
- Lifecycle intent.
- Validation requirements.
- Security requirements.
- Human-authority boundaries.
- AI boundaries.
- Required separation between Recommendation, Decision, Command, Confirmation, and outcome.

A Domain Model may reference conceptual Events, API behavior, and storage expectations.

It must not become a competing Event Catalog, API Contract, or machine-readable Schema.

---

## 5. Canonical Event Catalog

The Canonical Event Catalog governance baseline is established and approved.

The authoritative document is:

[ASOS Canonical Event Catalog Governance](./02-Event-Catalog/README.md)

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
- Ordering expectations.
- Delivery semantics.
- Consumer deduplication.
- Replay.
- Corrections.
- Reversals.
- Cancellation and revocation semantics.
- Compatibility.
- Deprecation and retirement.
- Event security and governance.

An Event is an immutable statement that a material occurrence happened.

An Event is not:

- A request for future action.
- A Recommendation.
- A Human Decision.
- A Command.
- A provider acknowledgement.
- Proof of an external business outcome unless the Event represents an accepted authoritative Confirmation.

---

## 6. Event Identity and Delivery

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

Replay must reuse the original canonical Event identity.

Transport metadata may change during retry, redelivery, dead-letter processing, or replay.

Canonical Event identity must not.

---

## 7. Human Authority, AI, and Automation

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

Where applicable, authoritative External Confirmation is also required before the final business outcome is accepted.

---

## 8. Recommendation, Command, and Outcome Separation

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

## 9. Core Ownership Boundaries

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

## 10. Catalog Authority Boundaries

| Catalog | Authoritative responsibility |
| :--- | :--- |
| `01-Domain-Model` | Object meaning, ownership, field meaning, relationships, lifecycle intent, validation, authority, security, and AI boundaries. |
| `02-Event-Catalog` | Event names, meanings, versions, envelopes, payload contracts, Producers, Consumers, delivery, replay, corrections, reversals, compatibility, and governance. |
| `03-API-Contracts` | API operations, request and response Schemas, authorization, errors, concurrency, and idempotency. |
| `04-Data-Schemas` | Machine-readable persistence, validation, exchange, and serialization Schemas. |
| `05-Agent-Contracts` | Agent purpose, inputs, outputs, permitted tools, Action Class, approval, evaluation, failure behavior, and escalation. |
| `06-Integration-Contracts` | External mappings, Commands, Confirmations, retries, reconciliation, failure handling, and provider-specific behavior. |

A detailed catalog becomes authoritative for its responsibility only after approval.

A lower-numbered directory is not automatically more authoritative than a higher-numbered directory.

Authority comes from approved scope, not folder order.

---

## 11. Core Contract Rules

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

## 12. Evidence and Authority Model

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
Reconciled business outcome
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

## 13. Versioning and Compatibility

Canonical contracts use semantic versioning.

- Major version: incompatible contract or governance change.
- Minor version: backward-compatible addition or substantial clarification.
- Patch version: non-semantic correction.

A canonical change must identify:

- The affected catalog.
- The affected contract.
- The current version.
- The proposed version.
- Compatibility impact.
- Migration impact.
- Security and privacy impact.
- AI and Human Approval impact.
- Event, API, Schema, and Integration impact.
- Required rollout, rollback, and reconciliation controls.

Breaking changes must not be introduced silently.

---

## 14. Content Boundaries

This directory must not duplicate the complete authoritative content of:

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../../00_Constitution`](../../00_Constitution/) |
| Operational Playbooks | [`../../01_Playbooks`](../../01_Playbooks/) |
| Business Rules | [`../../02_Business_Rules`](../../02_Business_Rules/) |
| Production and evaluation Prompts | [`../../03_Prompts`](../../03_Prompts/) |
| Reference data and safe examples | [`../../04_Data`](../../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../../05_Documentation`](../../05_Documentation/) |
| Images, diagrams, and templates | [`../../06_Assets`](../../06_Assets/) |

Canonical contracts may reference these sources.

They must not redefine their authoritative content inconsistently.

---

## 15. Change Governance

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

A repository commit does not automatically make a canonical contract effective in Production.

Controlled approval, migration, testing, release, observability, reconciliation, and rollback remain required.

---

## 16. Governing Documents

- [ASOS Constitution](../../00_Constitution/Constitution.md)
- [ASOS Product Vision](../../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Knowledge Base](../README.md)
- [ASOS Canonical Domain Model](./01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](./02-Event-Catalog/README.md)

---

## 17. Current Status

**Phase:** Approved Domain and Event governance baseline with active canonical-contract expansion  
**Version:** `1.2.0`  
**Status:** Approved Baseline  

Established and approved:

- Canonical Domain Model `v1.1.0`.
- Canonical Event Catalog Governance `v1.0.0`.

Remaining controlled expansion:

- API Contracts.
- Machine-readable Data Schemas.
- Agent Contracts.
- Integration Contracts.

The canonical documentation baseline remains under active implementation and governance.

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
