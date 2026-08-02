# ASOS Documentation

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Product, Architecture, and Governance  
**Applies To:** Product strategy, architecture, governance, Pilot design, implementation planning, deployment configuration, and controlled change  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory contains the authoritative product, architecture, governance, Pilot, and implementation-planning documentation for the ASOS AI Sales Operating System.

These documents explain:

- Why ASOS exists.
- What product outcomes ASOS is intended to create.
- How the platform is structured.
- How authority is distributed.
- How data ownership and Systems of Record are governed.
- How Human authority, AI assistance, and controlled automation interact.
- How dealership-independent foundations remain separate from deployment-specific configuration.
- How controlled Pilots should be planned and evaluated.
- How material changes must be reviewed, versioned, approved, and released.

This directory does not contain the complete authoritative contracts for:

- Canonical Domain Objects.
- Canonical Events.
- APIs.
- Machine-readable Data Schemas.
- AI Agent Contracts.
- Integration Contracts.

Those contracts belong in the ASOS Knowledge Base.

---

## 2. Documentation Authority

The documentation hierarchy is:

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

A lower-level document must not override a higher-level governance, authority, ownership, security, or Customer-protection boundary.

Where documents overlap, each document remains authoritative only for its approved scope.

---

## 3. Authoritative Documents in This Directory

| Document | Current baseline | Authoritative responsibility |
| :--- | :---: | :--- |
| [ASOS Product Vision](./Product_Vision.md) | `v1.1.0` Approved Baseline | Product mission, target organizations and Users, product outcomes, capability direction, AI and automation posture, non-goals, and long-term product direction. |
| [ASOS System Architecture](./System_Architecture.md) | `v1.1.0` Approved Baseline | Logical architecture, service boundaries, Event architecture, Command execution, AI architecture, security, reliability, observability, and deployment direction. |
| [ASOS Data Ownership and Systems of Record](./Data_Ownership_and_Systems_of_Record.md) | `v1.1.0` Approved Baseline | Field-level and operation-level authority, Systems of Record, synchronization, write permissions, conflict handling, evidence, provenance, and reconciliation. |
| [ASOS MVP Pilot Framework](./MVP_Pilot_Framework.md) | `v1.0.0` Approved Baseline | Reusable framework for planning, configuring, executing, evaluating, expanding, or stopping a controlled ASOS Pilot. |

---

## 4. Related Governing and Canonical Documents

The following documents are outside this directory but are required for correct interpretation and implementation.

| Document | Current baseline | Responsibility |
| :--- | :---: | :--- |
| [ASOS Constitution](../00_Constitution/Constitution.md) | `v1.1.0` Approved Baseline | Highest internal governance, Human authority, AI limits, security, Customer protection, and non-negotiable controls. |
| [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md) | `v1.1.0` Approved Baseline | Canonical Object meaning, ownership, lifecycle, relationships, validation, security, and authority boundaries. |
| [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md) | `v1.0.0` Approved Baseline | Event names and meanings, envelopes, versions, Producers, Consumers, delivery, replay, correction, compatibility, and governance. |
| [ASOS Knowledge Base](../07_Knowledge_Base/README.md) | Active canonical-contract index | Canonical implementation-contract structure and governance direction. |
| [ASOS Canonical Documentation](../07_Knowledge_Base/docs/README.md) | Active canonical-documentation index | Index of approved and planned canonical catalogs. |

---

## 5. Document Responsibilities

### Product Vision

The Product Vision is authoritative for:

- Product mission.
- Target organizations.
- Target Users.
- Product outcomes.
- Product capability pillars.
- Human, AI, and automation posture.
- Product principles.
- Product differentiation.
- Product non-goals.
- Long-term product direction.
- Deployment-independent success-measure categories.

The Product Vision is strategic.

It does not define:

- Field-level data ownership.
- Detailed service boundaries.
- Canonical Object Schemas.
- Event payloads.
- API operations.
- Deployment-specific thresholds.
- Production automation policies.

### System Architecture

The System Architecture is authoritative for:

- Logical platform layers.
- Service boundaries.
- AI Intelligence boundaries.
- Policy and Authorization.
- Human Review.
- Command Orchestration.
- Integration Edge.
- Event creation and delivery responsibilities.
- Canonical `event_id` ownership.
- Reliability and replay semantics.
- Security and observability.
- Deployment and infrastructure direction.

The System Architecture must preserve the distinction between:

```text
Recommendation
Human Decision
Authorization
Command
External Confirmation
Reconciled outcome
```

### Data Ownership and Systems of Record

The Data Ownership policy is authoritative for:

- External Systems of Record.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Write permissions.
- Source priority.
- Conflict handling.
- Freshness.
- Evidence.
- Provenance.
- Reconciliation.
- Deletion and correction propagation.

It must be applied at field and operation level.

No application may assume that one platform is universally authoritative.

### MVP Pilot Framework

The MVP Pilot Framework is authoritative for:

- Pilot planning.
- Pilot scope.
- Deployment configuration.
- User and role definition.
- Systems-of-Record mapping.
- Integration scope.
- Action Classes.
- Approval and automation-policy scope.
- Pilot success measures.
- Safety controls.
- Rollback.
- Expansion.
- Exit criteria.

Pilot values are deployment-specific.

They must not be promoted into universal platform rules without governance review.

---

## 6. Human Authority, AI, and Automation

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

Where applicable, authoritative External Confirmation is also required before the final business outcome is accepted.

### State Separation

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

Documentation must not collapse these states.

---

## 7. Event and Command Boundaries

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

Retryable Commands must use stable approved idempotency.

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

---

## 8. Content That Belongs Here

This directory is the authoritative location for:

- Product Vision.
- System Architecture.
- Data ownership and Systems-of-Record policy.
- Reusable Pilot frameworks.
- Architectural Decision Records.
- Product-governance policies.
- Security and privacy guidance.
- Implementation guidance.
- Roadmaps and delivery plans.
- Deployment and evaluation guidance.
- Change-control guidance.
- Cross-cutting governance that does not belong in a Canonical Domain or catalog.

---

## 9. Content That Does Not Belong Here

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Images, diagrams, and reusable assets | [`../06_Assets`](../06_Assets/) |
| Canonical Domain Models | [`../07_Knowledge_Base/docs/01-Domain-Model`](../07_Knowledge_Base/docs/01-Domain-Model/) |
| Canonical Event governance and contracts | [`../07_Knowledge_Base/docs/02-Event-Catalog`](../07_Knowledge_Base/docs/02-Event-Catalog/) |
| API Contracts | `../07_Knowledge_Base/docs/03-API-Contracts/` |
| Machine-readable Data Schemas | `../07_Knowledge_Base/docs/04-Data-Schemas/` |
| AI Agent Contracts | `../07_Knowledge_Base/docs/05-Agent-Contracts/` |
| Integration Contracts | `../07_Knowledge_Base/docs/06-Integration-Contracts/` |

Documentation may reference these locations.

It must not create competing authoritative definitions.

---

## 10. Documentation Rules

Every governed document must:

- Have a clear purpose.
- Identify its authoritative scope.
- Identify its owner.
- State its version.
- State its status.
- State its update date.
- Use approved terminology.
- Preserve dealership independence.
- Keep deployment-specific values configurable.
- Link to authoritative sources instead of duplicating complete definitions.
- Preserve Human authority.
- Preserve Action Class boundaries.
- Preserve Recommendation, Decision, Command, Confirmation, and outcome separation.
- Identify affected Domains and catalogs.
- Identify security and privacy impact.
- Identify migration and compatibility impact where applicable.
- Avoid embedding secrets or unrestricted Production data.

Breaking changes must not be introduced silently.

A repository commit does not automatically make a change effective in Production.

Approval, migration, testing, release, observability, and rollback controls remain required.

---

## 11. Versioning and Status

### Versioning

Use semantic versioning for governed documents:

- Major version for incompatible governance or contract changes.
- Minor version for backward-compatible additions or substantial clarifications.
- Patch version for non-semantic corrections.

### Status Values

Approved status values include:

- `Draft`.
- `Under Review`.
- `Approved Baseline`.
- `Superseded`.
- `Retired`.

`Approved Baseline` means the document is the current approved design or governance reference.

It does not mean:

- Production implementation is complete.
- Every integration exists.
- Every workflow is enabled.
- Every automation policy is approved.
- Every deployment uses the same configuration.
- Migration, testing, security review, and release controls are complete.

---

## 12. Current Documentation Baseline

The current approved documentation baseline includes:

- Product Vision `v1.1.0`.
- System Architecture `v1.1.0`.
- Data Ownership and Systems of Record `v1.1.0`.
- MVP Pilot Framework `v1.0.0`.

The related approved governance and canonical baseline includes:

- ASOS Constitution `v1.1.0`.
- Canonical Domain Model `v1.1.0`.
- Canonical Event Catalog Governance `v1.0.0`.

The following detailed canonical catalogs remain to be established:

- API Contracts.
- Machine-readable Data Schemas.
- Agent Contracts.
- Integration Contracts.

Implementation, integration, evaluation, and controlled deployment remain in progress.

---

## 13. Current Status

**Phase:** Approved foundation with active implementation and controlled expansion  
**Version:** `1.1.0`  
**Status:** Approved Baseline  

The Product Vision, System Architecture, Data Ownership policy, MVP Pilot Framework, Canonical Domain Model, and Canonical Event Catalog Governance are established.

The repository remains under active implementation.

Future documentation changes must preserve:

- Constitutional authority.
- Dealership independence.
- Tenant isolation.
- Field-level data authority.
- Human accountability.
- Controlled automation.
- Event and Command truthfulness.
- Security.
- Privacy.
- Evidence.
- Auditability.
- Reconciliation.
