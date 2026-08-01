# ASOS Canonical Domain Model

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Domain Governance  
**Applies To:** Domain services, APIs, Schemas, Events, databases, integrations, AI Agents, analytics, security, and operational workflows  
**Last Updated:** 2026-08-01  

---

## Purpose

This directory contains the canonical Domain Objects of the ASOS AI Sales Operating System.

These documents establish the shared business language used across:

- Business workflows.
- Domain services.
- APIs and integrations.
- Databases and data pipelines.
- Event processing.
- AI Agents.
- Analytics and reporting.
- Security, privacy, compliance, and audit controls.

Each Domain Model document is the authoritative specification for the meaning and ownership boundaries of its Object.

Implementations must not introduce conflicting:

- Object meanings.
- Field definitions.
- Relationships.
- Lifecycle states.
- Validation rules.
- Security classifications.
- Authority boundaries.

The Canonical Domain Model must remain dealership-independent.

Dealership-specific targets, Systems of Record, workflows, thresholds, approval limits, and integrations must remain configurable.

---

## Canonical Domain Objects

| Order | Domain Object | Purpose |
| :---: | :--- | :--- |
| 1 | [Customer](./Customer.md) | Represents the canonical individual or organization participating in a dealership relationship or sales journey. |
| 2 | [Vehicle](./Vehicle.md) | Represents the canonical identity and specifications of a physical or catalogued Vehicle, independent of dealership Inventory context. |
| 3 | [Inventory Record](./InventoryRecord.md) | Represents how a Vehicle is stocked, located, priced, prepared, reserved, allocated, transferred, sold, delivered, or retired by a dealership. |
| 4 | [Lead](./Lead.md) | Represents an initial Customer inquiry, response, referral, or potential sales interest. |
| 5 | [Qualified Lead](./QualifiedLead.md) | Represents a Lead that has been evaluated against approved qualification criteria. |
| 6 | [Opportunity](./Opportunity.md) | Represents an active and commercially meaningful sales pursuit. |
| 7 | [Appointment](./Appointment.md) | Represents a scheduled Customer engagement, consultation, showroom visit, remote meeting, or test drive. |
| 8 | [Quotation](./Quotation.md) | Represents a governed commercial offer prepared or presented to a Customer. |
| 9 | [Trade-In](./TradeIn.md) | Represents the appraisal, valuation, ownership, payoff, approval, and acquisition workflow for a Customer Vehicle. |
| 10 | [Finance Application](./FinanceApplication.md) | Represents a Customer request for Vehicle financing and its application-processing lifecycle. |
| 11 | [Financial Contract](./FinancialContract.md) | Represents a governed financial agreement created from an approved finance outcome. |
| 12 | [Deal](./Deal.md) | Represents the governed commercial transaction between a Customer and a dealership. |
| 13 | [Interaction](./Interaction.md) | Represents an omnichannel communication or meaningful Customer engagement. |
| 14 | [Market Intelligence](./MarketIntelligence.md) | Represents sourced market observations, evidence-backed analysis, commercial risks, and opportunities. |

---

## Core Sales Lifecycle

```text
Customer
   ↓
Lead
   ↓
Qualified Lead
   ↓
Opportunity
   ├── Interaction
   ├── Appointment
   ├── Vehicle Match
   │       ↓
   │  Inventory Record
   ├── Trade-In
   ├── Finance Application
   └── Quotation
           ↓
         Deal
           ├── Financial Contract
           ├── Payment and External Processing
           ├── Vehicle Allocation
           └── Delivery and Completion
```

Market Intelligence supports the lifecycle by providing governed:

- Pricing context.
- Demand observations.
- Supply observations.
- Inventory context.
- Competitor evidence.
- Commercial risks.
- Commercial opportunities.

The lifecycle shown here is conceptual.

It does not imply that every deployment uses the same operational sequence or external systems.

---

## Standard Domain Document Structure

Every Canonical Domain Model document contains the following 12 sections:

1. Object Purpose.
2. Canonical Schema.
3. Field Definitions.
4. Enumerations.
5. Validation Rules.
6. State Machine.
7. Relationships.
8. Domain Events.
9. AI Considerations.
10. API Contract.
11. Database Design.
12. Security.

This structure must remain consistent for future Canonical Domain Objects.

### Section Ownership Boundaries

The Domain Model remains authoritative for:

- Object meaning.
- Field meaning.
- Object ownership.
- Relationships.
- Lifecycle intent.
- Validation requirements.
- Authority classification.
- Security requirements.

The following future catalogs remain authoritative for their detailed contracts:

| Contract Type | Authoritative Location |
| :--- | :--- |
| Event names, envelopes, payloads, producers, and compatibility | `../02-Event-Catalog/` |
| API operations, requests, responses, and errors | `../03-API-Contracts/` |
| Machine-readable data Schemas | `../04-Data-Schemas/` |
| AI Agent inputs, outputs, tools, and authority | `../05-Agent-Contracts/` |
| Connector and external-system contracts | `../06-Integration-Contracts/` |

Until these catalogs are established, the relevant Domain Model sections describe required behavior and implementation direction.

They must not become competing authoritative catalogs.

---

## Domain Modeling Principles

### Canonical Ownership

Each business concept must have one authoritative Canonical Domain Object.

Duplicate Objects with overlapping responsibilities must not be created without an approved architectural decision.

Canonical ownership defines normalized meaning and responsibility.

It does not automatically mean that ASOS is the external legal, financial, contractual, or operational System of Record.

---

### Tenant Isolation

Every tenant-owned Object must be scoped by:

```text
tenant_id
```

Additional organizational context may include:

```text
dealer_group_id
dealership_id
branch_id
```

`tenant_id` is the primary isolation boundary.

`dealership_id` and `branch_id` provide additional scope inside the Tenant where applicable.

Cross-tenant access, linking, retrieval, processing, Event consumption, AI context retrieval, search, and analytics are prohibited unless governed by an explicit and auditable sharing mechanism.

---

### Data Authority

Every Domain Model must distinguish between:

- External System-of-Record Data.
- ASOS Canonical Projection.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decision.
- External Confirmation.

A Canonical Projection must not be represented as an authoritative external fact without supporting evidence.

A Recommendation must not be represented as a Human Decision.

A Human Decision must not be represented as external completion.

A sent Command must not be represented as an externally confirmed business outcome.

Detailed ownership and authority requirements are governed by:

[ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)

---

### Lifecycle Governance

Every stateful Object must define:

- Allowed states.
- Allowed transitions.
- Forbidden transitions.
- Entry conditions.
- Exit conditions.
- Required evidence.
- Required authority.
- Required Human Approval or automation policy.
- External Confirmation requirements.
- Terminal states.
- Correction or reversal behaviour.

Lifecycle transitions must be validated through deterministic controls.

Lifecycle history must remain auditable.

Externally controlled lifecycle states must remain pending until authoritative External Confirmation is received.

---

### Referential Integrity

Relationships between Domain Objects must:

- Remain within the permitted Tenant scope.
- Reference existing Canonical records.
- Preserve correct Customer, Vehicle, Inventory, Opportunity, and Deal context.
- Prevent invalid dependency chains.
- Prevent unauthorized cross-branch or cross-dealership linkage.
- Preserve historical references after supersession.
- Identify external references separately from Canonical identifiers.

---

### Immutable and Versioned Evidence

Commercial, financial, legal, consent, identity, signature, funding, Inventory, provider, and compliance evidence must not be silently overwritten after becoming authoritative.

Corrections may use:

- New corrective Events.
- Versioning.
- Supersession.
- Amendment.
- Controlled reversal.
- Compensating transactions.
- Governed redaction where legally required.

The original historical record must remain traceable where legally permitted.

---

### Vehicle and Inventory Separation

Vehicle identity and dealership Inventory context must remain separate.

`Vehicle` defines:

- Vehicle identity.
- VIN or chassis identity.
- Make.
- Model.
- Trim.
- Model year.
- Technical specifications.
- Identity and verification evidence.

`Inventory Record` defines:

- Dealership stock number.
- Tenant and dealership context.
- Branch and location.
- Availability.
- Preparation state.
- Reservation.
- Allocation.
- Pricing context.
- Inventory aging.
- Sale context.
- Transfer.
- Delivery context.
- Stock exit.

Vehicle availability must come from the configured authoritative Inventory source through the Inventory Record.

Reservations and allocations must use concurrency-safe operations.

Sold, delivered, transferred, returned, and retired Inventory Records must remain historically traceable.

---

### Event-Driven Architecture

Material accepted state changes should publish Canonical Domain Events.

Events must be:

- Immutable.
- Tenant-scoped.
- Uniquely identified by `event_id`.
- Traceable.
- Versioned.
- Supported by authoritative timestamps.
- Linked through correlation and causation identifiers.
- Replay-safe where required.
- Truthful about what occurred.

The Event Backbone may deliver an Event more than once.

Event Consumers must process duplicate delivery safely by detecting previously processed `event_id` values.

Events themselves are not described as “idempotent.”

Idempotency is a responsibility of:

- Event Consumers.
- State transitions.
- Retryable Commands.
- External write operations.

Retryable Commands must use an approved:

```text
idempotency_key
```

The Canonical Event Catalog is the authoritative source for:

- Event names.
- Event versions.
- Event envelopes.
- Payload Schemas.
- Producers.
- Consumers.
- Compatibility rules.
- Correction and reversal behaviour.

The Domain Events section inside each Object must reference related Event concepts without creating a competing Event specification.

---

### Event Truthfulness

Events describe past facts.

They must not request future actions.

ASOS must preserve the distinction between:

```text
RecommendationGenerated
RecommendationApproved
CommandCreated
CommandSent
ExternalActionConfirmed
```

A Recommendation Event must not imply Human Approval.

A Human Decision Event must not imply external completion.

A Command Event must not imply authoritative External Confirmation.

Corrections and reversals must be represented using new Events linked to the original Event.

---

### AI Governance

AI Agents may assist with:

- Classification.
- Extraction.
- Summarization.
- Matching.
- Forecasting.
- Recommendation.
- Risk detection.
- Opportunity detection.
- Completeness checking.
- Draft generation.
- Workflow coordination.
- Context retrieval.
- Explanation generation.

AI output must distinguish between:

- Fact.
- Observation.
- Derived Intelligence.
- Assumption or hypothesis.
- Recommendation.
- Human Decision.
- External Confirmation.

AI Agents must not independently create or confirm binding:

- Customer consent.
- Customer-visible pricing commitments.
- Restricted discounts.
- Vehicle reservations outside approved authority.
- Deal allocations.
- Credit or finance Decisions.
- Trade-In approvals.
- Legal signatures.
- Contracts.
- Payments.
- Funding Confirmation.
- Compliance clearance.
- Deal finalization.
- Vehicle sale Confirmation.
- Vehicle delivery Confirmation.

High-risk or binding actions require the authority defined by the ASOS Constitution and System Architecture.

Action Class 2 may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

Action Class 3 requires an Authoritative Human Decision and, where applicable, External Confirmation.

The AI Intelligence Layer must not execute external actions directly.

---

### Deterministic Controls

The following controls must not depend solely on probabilistic AI reasoning:

- Authentication.
- Authorization.
- Tenant isolation.
- Required-field validation.
- Financial calculations.
- Approval thresholds.
- Consent enforcement.
- Lifecycle transition validation.
- Data-retention controls.
- Prohibited-action enforcement.
- Command idempotency.
- External-confirmation validation.
- Security-policy enforcement.

AI Agents must not override deterministic controls.

---

### Security by Design

Every Domain implementation must enforce:

- Authentication.
- Role-Based Access Control.
- Attribute-based controls where required.
- Least-privilege access.
- Tenant isolation.
- Branch and dealership scope.
- Encryption in transit.
- Encryption at rest.
- Field-level protection.
- Sensitive-data masking.
- Audit logging.
- Consent and purpose limitation.
- Retention and deletion policies.
- Legal-hold controls where applicable.
- Secret-management requirements.
- AI-context restrictions.
- Emergency suspension where applicable.

Access to an Object does not automatically grant authority to modify, approve, or execute actions against it.

---

## Naming Conventions

- Markdown filenames use PascalCase.
- Canonical Object names use singular form.
- Database tables use plural `snake_case`.
- Fields use `snake_case`.
- REST resources use plural `kebab-case`.
- GraphQL types use PascalCase.
- Domain Events use past-tense PascalCase.
- Commands use imperative PascalCase.
- Enumeration values use uppercase `SNAKE_CASE`.
- Primary Canonical identifiers use UUIDv4 unless an approved standard specifies otherwise.
- Every tenant-owned Object includes `tenant_id`.
- External identifiers must remain separate from Canonical identifiers.

Examples:

```text
Event: LeadCreated
Command: CreateExternalAppointment
Field: tenant_id
Table: inventory_records
REST Resource: finance-applications
GraphQL Type: FinanceApplication
Enum: PENDING_CONFIRMATION
```

---

## Change Management

Changes to a Canonical Domain Model must include:

- Business justification.
- Constitutional impact.
- Data-ownership impact.
- Impacted fields.
- Impacted relationships.
- Lifecycle impact.
- Authority impact.
- Event impact.
- API impact.
- Schema and database impact.
- AI-context impact.
- Security and privacy impact.
- Integration impact.
- Migration or backward-compatibility plan.
- Updated document version.
- Review and approval evidence.

Breaking changes must not be introduced silently.

A change committed to the repository is not automatically an approved Production change.

Required governance, testing, migration, and release controls must be completed.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../../../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Documentation](../README.md)

---

## Completion Status

The initial ASOS Canonical Domain Model contains 14 established Domain Objects.

All listed Objects include the standard 12-section structure.

The Domain Model is approved as the current conceptual baseline.

Individual Object specifications must still be reviewed and aligned with:

- `tenant_id` as the primary isolation boundary.
- Data Ownership Policy version `1.1.0`.
- System Architecture version `1.1.0`.
- Action Classes.
- Human Approval and approved automation policies.
- External Confirmation requirements.
- Event immutability.
- At-least-once Event delivery.
- Consumer idempotency.
- The future Canonical Event Catalog.

An Object must not be considered implementation-ready until its individual alignment review is completed.
