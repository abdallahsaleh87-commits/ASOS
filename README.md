# ASOS

## AI Sales Operating System

**Repository Baseline:** `1.2.0`  
**Baseline Status:** Active Governed Foundation Baseline  
**Delivery Status:** Implementation and controlled expansion in progress  
**Primary Market Context:** Automotive retail and dealership operations  
**Last Updated:** 2026-08-02  

---

## 1. Overview

ASOS is a dealership-independent decision-support and workflow-orchestration platform for automotive sales operations.

It operates above and between approved dealership systems such as:

- Customer Relationship Management systems.
- Dealer Management Systems.
- Inventory Management Systems.
- Finance and Insurance platforms.
- Lender systems.
- Communication providers.
- Appointment platforms.
- Quotation and pricing systems.
- Contract and document platforms.
- Payment systems.
- Delivery systems.
- OEM systems.
- Approved spreadsheets and legacy data sources.

ASOS does not assume that one platform is the universal System of Record for every field or operation.

It combines:

- Governed Canonical Domain Models.
- Field-level and operation-level authority.
- Event-driven coordination.
- Deterministic controls.
- AI-assisted analysis.
- Explainable Recommendations.
- Human Review.
- Controlled Commands.
- External Confirmation.
- Reconciliation.
- Audit and observability.

ASOS is intended to improve Customer experience, sales effectiveness, Inventory utilization, operational consistency, and accountability without reducing Human authority.

---

## 2. Product Mission

ASOS helps automotive sales organizations make better, faster, safer, and more accountable decisions.

The platform should:

- Improve the Customer experience.
- Improve Lead-to-Deal conversion.
- Reduce lost or delayed sales opportunities.
- Improve Customer follow-up.
- Improve Vehicle and Inventory matching.
- Improve pipeline discipline.
- Improve Inventory utilization.
- Coordinate Appointment, Quotation, Trade-In, Finance, Contract, Deal, and delivery dependencies.
- Reduce duplicate entry and avoidable administration.
- Detect risk, missing evidence, stale data, and conflicts.
- Provide explainable and auditable AI assistance.
- Preserve field-level authority and external Systems of Record.
- Support multiple dealerships, branches, brands, markets, and technology stacks.

The approved Product Vision is:

[ASOS Product Vision](./05_Documentation/Product_Vision.md)

---

## 3. Governance and Authority

ASOS follows an explicit authority hierarchy.

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

A lower-level document must not override a higher-level governance, ownership, security, or authority boundary.

### Core Governing Documents

| Document | Current baseline | Responsibility |
| :--- | :---: | :--- |
| [ASOS Constitution](./00_Constitution/Constitution.md) | `v1.1.0` Approved Baseline | Highest internal governance, Human authority, AI limits, security, and non-negotiable controls. |
| [ASOS Product Vision](./05_Documentation/Product_Vision.md) | `v1.1.0` Approved Baseline | Product mission, outcomes, Users, capability direction, and product boundaries. |
| [ASOS System Architecture](./05_Documentation/System_Architecture.md) | `v1.1.0` Approved Baseline | Logical architecture, execution boundaries, Event architecture, AI architecture, security, and reliability. |
| [ASOS Data Ownership and Systems of Record](./05_Documentation/Data_Ownership_and_Systems_of_Record.md) | `v1.1.0` Approved Baseline | Field-level and operation-level authority, synchronization, conflict handling, evidence, and write permissions. |
| [ASOS MVP Pilot Framework](./05_Documentation/MVP_Pilot_Framework.md) | `v1.0.0` Approved Baseline | Reusable framework for controlled dealership Pilot planning and evaluation. |
| [ASOS Canonical Domain Model](./07_Knowledge_Base/docs/01-Domain-Model/README.md) | `v1.1.0` Approved Baseline | Canonical Object meaning, ownership, lifecycle, validation, relationships, security, and authority boundaries. |
| [ASOS Canonical Event Catalog Governance](./07_Knowledge_Base/docs/02-Event-Catalog/README.md) | `v1.0.0` Approved Baseline | Event names and meanings, envelopes, Producers, Consumers, delivery, replay, correction, compatibility, and governance. |

---

## 4. Human Authority, AI, and Automation

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

AI output must remain distinguishable from:

- Authoritative fact.
- Authoritative Human Decision.
- Command.
- External Confirmation.
- Reconciled business outcome.

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

### Recommendation, Execution, and Outcome Separation

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

---

## 5. Core Ownership Boundaries

ASOS uses one approved canonical owner for each material business concept or mutation workflow.

Examples include:

| Capability or fact | Canonical owner |
| :--- | :--- |
| Customer identity | Customer Domain Service or configured identity authority |
| Vehicle identity and specification | Vehicle Domain Service |
| Inventory intake acceptance | Inventory Domain Service |
| Inventory Record creation and activation | Inventory Domain Service |
| Trade-In appraisal and acquisition workflow | Trade-In Domain Service and configured external authorities |
| Lead qualification outcome | Qualified Lead Domain Service |
| Active sales pursuit and Opportunity lifecycle | Opportunity Domain Service or configured CRM authority |
| Customer communication execution | Interaction or Communication Service |
| Appointment lifecycle | Appointment Domain Service |
| Customer-specific commercial offer | Quotation Domain Service |
| Finance application and offer selection | Finance Application Domain Service and lender authority |
| Funding-request mutation and funding workflow | Financial Contract Domain Service |
| Commercial transaction and completion gate | Deal Domain Service |
| Inventory Reservation and Allocation | Inventory Domain Service or approved dedicated service |
| Binding or high-impact Decision | Authorized Human or configured external authority |
| Event creation and canonical `event_id` assignment | Approved Event-producing boundary |
| Event transport, retry, dead-letter handling, and replay | Event and Messaging Backbone |

A non-owning Domain may retain only necessary:

- References.
- Versioned snapshots.
- Read-only projections.
- Freshness indicators.
- Evidence references.
- Reconciliation state.

A request does not prove acceptance.

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

---

## 6. Repository Structure

| Directory | Responsibility |
| :--- | :--- |
| [`00_Constitution`](./00_Constitution/) | Constitutional principles, authority boundaries, and non-negotiable governance rules. |
| [`01_Playbooks`](./01_Playbooks/) | Governance baseline and future operational Playbooks for Human and approved AI-assisted workflows. |
| [`02_Business_Rules`](./02_Business_Rules/) | Governance baseline and future deterministic Business Rules, eligibility logic, thresholds, and approval constraints. |
| [`03_Prompts`](./03_Prompts/) | Governance baseline and future versioned Prompts, Agent instructions, and structured-output templates. |
| [`04_Data`](./04_Data/) | Governance baseline for reference data, mappings, synthetic examples, test fixtures, evaluation datasets, and other non-Production Data assets. |
| [`05_Documentation`](./05_Documentation/) | Product Vision, System Architecture, governance policies, Pilot framework, decisions, and roadmap. |
| [`06_Assets`](./06_Assets/) | Governance baseline and future diagrams, images, templates, and supporting visual Assets. |
| [`07_Knowledge_Base`](./07_Knowledge_Base/) | Approved Domain and Event contracts plus governed locations for future API, Schema, Agent, and Integration Contracts. |

Folder numbering represents repository organization.

It does not automatically establish authority or implementation order.

---

## 7. Content Ownership

Each type of knowledge must have one authoritative location.

- Constitutional rules belong in `00_Constitution`.
- Operational Playbooks belong in `01_Playbooks`.
- Business Rules belong in `02_Business_Rules`.
- Governed Prompt assets belong in `03_Prompts`.
- Reference data, mappings, synthetic examples, test fixtures, and evaluation datasets belong in `04_Data`.
- Product, architecture, governance, and Pilot documentation belong in `05_Documentation`.
- Visual and document-support Assets belong in `06_Assets`.
- Canonical implementation contracts belong in `07_Knowledge_Base`.

Documents may link to each other.

They must not create conflicting:

- Object meanings.
- Canonical ownership.
- Lifecycle states.
- Approval rules.
- Action Classes.
- Event definitions.
- API operations.
- Data Schemas.
- Agent authority.
- Integration behavior.

---

## 8. Canonical Documentation

### Canonical Domain Model

The approved Domain Model contains 14 Canonical Domain Objects:

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

The authoritative index is:

[ASOS Canonical Domain Model](./07_Knowledge_Base/docs/01-Domain-Model/README.md)

### Canonical Event Catalog

The Canonical Event Catalog governance baseline is established and approved.

It is authoritative for:

- Event names and meanings.
- Event taxonomy.
- Event versions.
- Event envelopes.
- Producer and Consumer responsibilities.
- Delivery semantics.
- Consumer deduplication.
- Replay.
- Corrections and reversals.
- Compatibility.
- Security and governance.

The authoritative catalog governance document is:

[ASOS Canonical Event Catalog Governance](./07_Knowledge_Base/docs/02-Event-Catalog/README.md)

### Remaining Canonical Catalogs

The following detailed catalogs are not yet established as approved baselines:

- API Contracts.
- Machine-readable Data Schemas.
- Agent Contracts.
- Integration Contracts.

Their future locations are:

```text
07_Knowledge_Base/docs/03-API-Contracts/
07_Knowledge_Base/docs/04-Data-Schemas/
07_Knowledge_Base/docs/05-Agent-Contracts/
07_Knowledge_Base/docs/06-Integration-Contracts/
```

They must be established through controlled governance and must not contradict the Constitution, Architecture, Data Ownership policy, Domain Model, or Event Catalog.

---

## 9. Event and Command Reliability

Material accepted state changes should publish Canonical Events.

The approved Event-producing boundary must:

- Create the Event.
- Assign the immutable canonical `event_id`.
- Preserve Tenant context.
- Preserve Event version.
- Preserve occurrence time.
- Preserve authority and evidence references.
- Publish with the accepted state or through an approved transactional outbox.

The Event Backbone must preserve the Producer-assigned `event_id` unchanged during:

- Publication retry.
- Delivery retry.
- Redelivery.
- Dead-letter handling.
- Authorized replay.

The Backbone must not silently generate, replace, or rewrite the canonical Event identifier.

Event delivery is at-least-once.

Consumers must detect previously processed `event_id` values before creating duplicate business effects.

Retryable Commands must use stable approved idempotency.

---

## 10. Security Notice

This repository must not contain:

- Real Customer personal data.
- Production credentials.
- API keys or access tokens.
- Passwords or private keys.
- Real payment information.
- Unredacted legal documents.
- Real unrestricted VIN datasets.
- Confidential internal pricing or margin data.
- Provider credentials.
- Unrestricted lender or finance data.
- Live Consent evidence.
- Live approval evidence.

Examples must use fictional, synthetic, anonymized, or safely redacted information.

Every implementation must preserve:

- Tenant isolation.
- Least privilege.
- Purpose limitation.
- Consent.
- Data minimization.
- Encryption.
- Sensitive-data protection.
- Retention and deletion requirements.
- Audit.
- AI-context restrictions.
- Emergency suspension.
- Incident detection and response.

---

## 11. Current Repository Status

### Approved Foundation Baselines

- ASOS Constitution `v1.1.0`.
- Product Vision `v1.1.0`.
- System Architecture `v1.1.0`.
- Data Ownership and Systems of Record `v1.1.0`.
- MVP Pilot Framework `v1.0.0`.
- Canonical Domain Model `v1.1.0`.
- Canonical Event Catalog Governance `v1.0.0`.

### Approved Repository Governance and Index Baselines

- Constitution index and governance guidance `v1.1.0`.
- Playbooks governance `v1.0.0`.
- Business Rules governance `v1.0.0`.
- Prompt governance `v1.0.0`.
- Data governance `v1.0.0`.
- Documentation index `v1.1.0`.
- Assets governance `v1.0.0`.
- Knowledge Base index `v1.1.0`.
- Canonical Documentation index `v1.2.0`.

These baselines govern their directories and content types.

They do not by themselves approve individual Production Playbooks, Business Rules, Prompts, Data assets, visual Assets, automation policies, integrations, or deployments.

### Current Delivery Work

- Establish detailed API Contracts.
- Establish machine-readable Data Schemas.
- Establish Agent Contracts.
- Establish Integration Contracts.
- Create, test, and approve Domain-specific Business Rules.
- Create, test, and approve operational Playbooks.
- Develop, evaluate, and approve individual Prompts.
- Introduce governed Data and visual Assets only when actual controlled content is required.
- Define deployment profiles.
- Configure controlled Pilot environments.
- Implement, test, evaluate, observe, and reconcile workflows.
- Expand automation only after evidence and control maturity are demonstrated.

### Status Interpretation

`Approved Baseline` means a document is the current approved design or governance reference.

It does not mean:

- Production implementation is complete.
- Every integration is available.
- Every workflow is enabled.
- Every dealership deployment uses the same configuration.
- Every proposed Agent or automation policy is approved.
- Migration, testing, security review, and release controls are complete.

A repository commit does not automatically make a document, contract, Business Rule, Playbook, Prompt, Data asset, visual Asset, automation policy, integration, or deployment effective in Production.

Applicable approval, configuration, migration, testing, release, monitoring, reconciliation, and rollback controls remain required.

The repository remains under active implementation and controlled expansion.
