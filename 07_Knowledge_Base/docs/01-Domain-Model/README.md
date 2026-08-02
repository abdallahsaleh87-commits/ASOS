# ASOS Canonical Domain Model

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Domain Governance  
**Primary Isolation Boundary:** `tenant_id`  
**Applies To:** Domain services, APIs, Schemas, Events, databases, integrations, AI Agents, analytics, security, and operational workflows  
**Last Updated:** 2026-08-02  

---

## 1. Purpose and Authority

This directory contains the Canonical Domain Objects of the ASOS AI Sales Operating System.

The Domain Model establishes the shared business language used across:

- Business workflows.
- Canonical Domain Services.
- APIs and integrations.
- Databases and data pipelines.
- Event processing.
- AI Agents.
- Analytics and reporting.
- Security, privacy, compliance, and audit controls.
- Human Review and approval workflows.
- Command Orchestration and external execution.

Each Domain Model document is authoritative for the meaning, ownership, lifecycle intent, relationships, validation requirements, authority boundaries, and security requirements of its Object.

Implementations must not introduce conflicting:

- Object meanings.
- Canonical ownership.
- Field definitions.
- Relationships.
- Lifecycle states.
- Validation rules.
- Authority categories.
- Security classifications.
- Command ownership.
- External Confirmation semantics.

The Canonical Domain Model must remain dealership-independent.

Dealership-specific targets, Systems of Record, workflows, thresholds, approval limits, automation policies, integrations, and deployment choices must remain configurable.

### Document Precedence

The Domain Model operates under the authority order defined by the ASOS Constitution.

For Domain implementation, the following responsibilities are distinct:

```text
Constitution
  = highest internal governance and authority boundaries

System Architecture
  = logical services, execution, Event, AI, security,
    reliability, and integration architecture

Data Ownership and Systems of Record
  = field-level and operation-level authority

Canonical Domain Model
  = Object meaning, ownership, lifecycle, relationships,
    validation, authority, and security

Canonical Event Catalog
  = Event names, meanings, envelopes, versions,
    Producers, Consumers, delivery, replay, and corrections

API Contracts
  = operations, requests, responses, errors,
    authorization, concurrency, and idempotency

Data Schemas
  = machine-readable validation and serialization

Agent Contracts
  = Agent purpose, inputs, outputs, tools,
    limits, approval, and escalation

Integration Contracts
  = external mappings, Commands, Confirmations,
    retries, and reconciliation
```

A lower-level contract must not override a higher-level governance or ownership boundary.

---

## 2. Canonical Domain Objects

The approved baseline contains 14 Canonical Domain Objects.

| Order | Domain Object | Canonical responsibility |
| :---: | :--- | :--- |
| 1 | [Customer](./Customer.md) | Canonical individual or organization identity, Customer relationships, permitted contact context, and governed Customer projections. |
| 2 | [Vehicle](./Vehicle.md) | Canonical physical or catalog Vehicle identity, VIN or chassis identity, technical specification, configuration, and identity evidence. |
| 3 | [Inventory Record](./InventoryRecord.md) | Inventory intake acceptance, canonical stock-cycle creation and activation, stock identity, location, availability, preparation, pricing context, Reservation, Allocation, transfer, sale, delivery, and exit projection. |
| 4 | [Lead](./Lead.md) | Original inquiry, response, referral, or potential commercial-interest intake and its source evidence. |
| 5 | [Qualified Lead](./QualifiedLead.md) | Governed qualification outcome, qualification evidence, validity, and conversion eligibility. |
| 6 | [Opportunity](./Opportunity.md) | Active commercial pursuit, requirements, stage, assignment, forecast, next-action planning, Action Class context, authorization projection, and Deal-conversion coordination. |
| 7 | [Appointment](./Appointment.md) | Appointment request, scheduling, external Confirmation, attendance, outcome, and Appointment reconciliation. |
| 8 | [Quotation](./Quotation.md) | Governed Customer-specific commercial offer, pricing terms, approvals, issue, presentation, acceptance, expiration, and supersession. |
| 9 | [Trade-In](./TradeIn.md) | Appraisal, ownership, lien, payoff, equity, Customer offer, acquisition readiness, legal acquisition, and request for Inventory intake. |
| 10 | [Finance Application](./FinanceApplication.md) | Finance application, underwriting workflow, lender Decision projection, offer selection, funding readiness, and Financial Contract handoff. |
| 11 | [Financial Contract](./FinancialContract.md) | Governed financial agreement, signature lifecycle, funding-request mutation, Funding Commands, external funding Confirmation, activation, reversal, and reconciliation. |
| 12 | [Deal](./Deal.md) | Governed commercial transaction, completion requirements, commercial closure, read-only funding projection, blockers, evidence references, and completion gate. |
| 13 | [Interaction](./Interaction.md) | Omnichannel communication, Customer engagement, execution evidence, provider delivery, receipt, response, and communication history. |
| 14 | [Market Intelligence](./MarketIntelligence.md) | Sourced market observations, evidence-backed analysis, commercial risks, opportunities, and Derived Intelligence. |

### Object Naming

Canonical Object names use singular form.

Examples:

```text
Customer
Vehicle
Inventory Record
Lead
Qualified Lead
Opportunity
Appointment
Quotation
Trade-In
Finance Application
Financial Contract
Deal
Interaction
Market Intelligence
```

---

## 3. Canonical Lifecycle and Coordination Map

### Core Commercial Lifecycle

```text
Customer
   ↓
Lead
   ↓
Qualified Lead
   ↓
Opportunity
   ├── Interaction and Customer communication
   ├── Appointment and test-drive coordination
   ├── Vehicle matching
   │       ↓
   │  Inventory Record selection
   ├── Trade-In workflow
   ├── Finance Application workflow
   └── Quotation workflow
           ↓
         Deal
           ├── Financial Contract
           │       └── Funding workflow
           ├── Payment and external processing
           ├── Inventory Allocation projection
           ├── Delivery and registration projections
           └── Completion gate and closure
```

This lifecycle is conceptual.

It does not imply that every deployment uses the same sequence, external systems, or workflow authority.

### Trade-In to Inventory Handoff

```text
Trade-In appraisal and acquisition workflow
        ↓
Trade-In acquisition requirements satisfied
        ↓
Trade-In requests Inventory intake
        ↓
Inventory Domain validates the request
        ↓
Inventory intake accepted or rejected
        ↓
Canonical Inventory Record created
        ↓
Inventory Record activated
        ↓
Physical receipt, preparation, pricing, and availability
```

The following are not equivalent:

```text
Trade-In acquired
  ≠ Inventory intake requested

Inventory intake requested
  ≠ Inventory intake accepted

Inventory intake accepted
  ≠ Inventory Record created

Inventory Record created
  ≠ Inventory Record activated

Inventory Record activated
  ≠ commercially available
```

Trade-In owns the request and read-only result tracking.

Inventory owns intake acceptance, Inventory Record creation, activation, and stock lifecycle.

### Finance and Funding Handoff

```text
Finance Application
  = application, underwriting, lender Decision projection,
    offer selection, funding readiness,
    and contracting handoff

Financial Contract
  = contract, signatures, funding-request mutation,
    Funding Commands, idempotency, external Confirmation,
    partial funding, shortfall, failure, reversal,
    activation, and reconciliation

Deal
  = commercial completion requirements,
    read-only funding projection, blockers,
    evidence references, and completion gate
```

The Financial Contract Domain Service is the sole canonical owner of the funding-request mutation.

Finance Application and Deal must not expose independent funding-request mutations.

The external lender, bank, or configured funding authority remains authoritative for the funding outcome.

### Opportunity Action Execution Handoff

```text
Opportunity Recommendation or next-action plan
        ↓
Deterministic Action Class evaluation
        ↓
Action Class 2 requires one valid authority path
        ├── Explicit Human Approval for the exact action
        └── Active pre-approved automation policy
        ↓
Responsible execution service creates Command
        ↓
Command Orchestration and connector execute
        ↓
External Confirmation, rejection, failure, or timeout
        ↓
Canonical projection and reconciliation
```

No third Action Class 2 authority path exists.

Opportunity stage, priority, score, AI confidence, Task assignment, draft content, or User-interface state is not execution authority.

The responsible execution service owns:

- Command creation.
- Command idempotency.
- Provider execution.
- External Confirmation.
- Failure and timeout handling.
- Reconciliation.

Opportunity owns Recommendation, planning, Action Class context, authorization references, and read-only execution projections.

### Event Publication and Delivery Handoff

```text
Accepted material occurrence
        ↓
Approved Event Producer creates Event
        ↓
Producer assigns immutable event_id
        ↓
Event is persisted with the accepted state
or through an approved transactional outbox
        ↓
Event Backbone validates and transports
        ↓
Consumers validate tenant and event version
        ↓
Consumers deduplicate by event_id before side effects
```

The Event Backbone must preserve the Producer-assigned `event_id` unchanged during:

- Publication retry.
- Delivery retry.
- Redelivery.
- Dead-letter handling.
- Authorized replay.

The Backbone must not silently generate, replace, or rewrite the canonical Event identifier.

---

## 4. Critical Ownership Boundaries

| Capability or fact | Canonical owner | Non-owning Domains may retain |
| :--- | :--- | :--- |
| Customer identity | Customer Domain Service | Customer reference and approved projection |
| Vehicle identity and specification | Vehicle Domain Service | Vehicle reference and versioned snapshot |
| Inventory intake acceptance | Inventory Domain Service | Request and result projection |
| Inventory Record creation and activation | Inventory Domain Service | `inventory_record_id`, status, evidence references |
| Trade-In appraisal, payoff, equity, and acquisition | Trade-In Domain Service and configured authorities | Trade-In projections |
| Qualification outcome | Qualified Lead Domain Service | Qualification reference and snapshot |
| Active pursuit and sales stage | Opportunity Domain Service or configured CRM authority | Stage projection where non-owning |
| Action Class 2 authorization | Authorized Human Decision or Policy and Authorization Service | Authorization reference and status projection |
| Customer communication execution | Interaction or Communication Service | Execution and outcome projection |
| Appointment scheduling and outcome | Appointment Domain Service | Appointment references and projections |
| Customer-specific offer | Quotation Domain Service | Quotation reference and projection |
| Finance application and offer selection | Finance Application Domain Service and lender authority | Finance status projection |
| Funding-request workflow | Financial Contract Domain Service | Read-only funding projection and blockers |
| External funding outcome | Lender, bank, or configured funding authority | Accepted canonical projection |
| Commercial transaction and completion gate | Deal Domain Service | Deal reference and status projection |
| Inventory Reservation and Allocation | Inventory Domain Service or approved dedicated service | Reservation or Allocation projection |
| Payment outcome | Payment authority | Payment projection and evidence references |
| Delivery outcome | Delivery workflow or configured external authority | Delivery projection and evidence references |
| Event identity creation | Approved Event-producing boundary | Preserved `event_id` |
| Event transport and replay | Event and Messaging Backbone | Delivery metadata |
| AI Recommendation | Approved AI or intelligence service | Recommendation reference and projection |
| Binding or high-impact Decision | Authorized Human or configured external authority | Decision reference and evidence |

### Ownership Rules

- One canonical capability must not have multiple competing mutation owners.
- A non-owning Domain may store only necessary references, snapshots, read-only projections, freshness, and reconciliation state.
- A request does not prove acceptance.
- A Command does not prove completion.
- Provider acknowledgement does not prove authoritative business outcome.
- External Confirmation must be validated and reconciled before canonical acceptance.
- Derived Intelligence must not overwrite authoritative state.
- AI must not impersonate a Domain Service, Human Decision authority, or external System of Record.

---

## 5. Standard Domain Document Structure

Every Canonical Domain Model document uses the following 12 numbered sections:

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

Documents may also include:

- Metadata.
- Governing Documents.
- Current Status.

### Section Authority

The Domain Model remains authoritative for:

- Object meaning.
- Object ownership.
- Field meaning.
- Relationships.
- Lifecycle intent.
- Validation requirements.
- Authority boundaries.
- Security requirements.
- Required execution and Confirmation separation.

The detailed catalogs remain authoritative for their own contracts:

| Contract type | Authoritative location |
| :--- | :--- |
| Event governance and Canonical Event Catalog | [`../02-Event-Catalog/README.md`](../02-Event-Catalog/README.md) |
| API operations and Schemas | `../03-API-Contracts/` |
| Machine-readable data Schemas | `../04-Data-Schemas/` |
| AI Agent contracts | `../05-Agent-Contracts/` |
| Connector and external-system contracts | `../06-Integration-Contracts/` |

The Canonical Event Catalog is established and approved.

A Domain Event section identifies required Event concepts and Producer boundaries but must not become a competing Event Catalog.

Where another detailed catalog has not yet been established, the Domain Model describes required behavior and implementation direction without claiming final contract authority.

---

## 6. Domain Modeling Principles

### 6.1 Canonical Ownership

Each business concept and mutation workflow must have one approved canonical owner.

Duplicate Objects or overlapping mutation boundaries require an approved architectural decision and migration plan.

Canonical ownership defines normalized meaning and responsibility.

It does not automatically mean ASOS is the external legal, financial, contractual, regulatory, or operational System of Record.

### 6.2 Tenant Isolation

Every Tenant-owned Object must be scoped by:

```text
tenant_id
```

Additional organizational context may include:

```text
dealer_group_id
dealership_id
branch_id
legal_entity_id
team_id
```

`tenant_id` is the primary isolation boundary.

Cross-Tenant access, linking, retrieval, processing, Event consumption, AI context retrieval, search, authorization evaluation, execution, and analytics are prohibited unless governed by an explicit and auditable mechanism.

### 6.3 Data Authority

Every Domain Model must distinguish among:

- External System-of-Record Data.
- ASOS Canonical Projection.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Recommendation.
- Authoritative Human Decision.
- Command.
- External Confirmation.
- Reconciled canonical outcome.

A Canonical Projection must not be represented as an authoritative external fact without evidence.

A Recommendation must not be represented as a Human Decision.

A Human Decision must not be represented as external completion.

A sent Command must not be represented as a confirmed business outcome.

A provider acknowledgement must not be represented as final completion unless that acknowledgement is the configured authoritative Confirmation.

Detailed ownership requirements are governed by:

[ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)

### 6.4 Lifecycle Governance

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
- Correction, reversal, cancellation, and revocation behavior.

Lifecycle transitions must be validated deterministically.

Lifecycle history must remain auditable.

Externally controlled states must remain pending until authoritative External Confirmation is accepted.

### 6.5 Referential Integrity

Relationships must:

- Remain within permitted Tenant scope.
- Reference existing Canonical records.
- Preserve correct Customer, Vehicle, Inventory, Lead, Opportunity, Contract, and Deal context.
- Prevent invalid dependency chains.
- Prevent unauthorized cross-branch or cross-dealership linkage.
- Preserve historical references after supersession.
- Separate external identifiers from Canonical identifiers.
- Preserve source record versions where required.

### 6.6 Immutable and Versioned Evidence

Commercial, financial, legal, Consent, identity, signature, funding, Inventory, provider, and compliance evidence must not be silently overwritten after becoming authoritative.

Corrections may use:

- New corrective Events.
- Versioning.
- Supersession.
- Amendment.
- Controlled reversal.
- Compensating transactions.
- Governed redaction where legally required.

Original history must remain traceable where legally permitted.

### 6.7 Vehicle and Inventory Separation

Vehicle identity and dealership Inventory context must remain separate.

`Vehicle` defines:

- Physical or catalog identity.
- VIN or chassis identity.
- Make, model, trim, and model year.
- Technical specification.
- Factory configuration.
- Identity-resolution evidence.

`Inventory Record` defines:

- Inventory intake and stock-cycle identity.
- Stock number.
- Tenant, dealership, branch, and location context.
- Physical presence.
- Availability.
- Preparation.
- Pricing context.
- Reservation.
- Allocation.
- Transfer.
- Sale and delivery projections.
- Stock exit.

Vehicle availability must come through the configured authoritative Inventory source and Inventory Record projection.

Reservations and Allocations must use concurrency-safe operations.

Sold, delivered, transferred, returned, retired, and archived stock cycles must remain historically traceable.

### 6.8 Trade-In and Inventory Separation

Trade-In owns appraisal and acquisition workflow.

Inventory owns stock intake and Inventory Record lifecycle.

Trade-In must not:

- Create the canonical Inventory Record directly.
- Assign the authoritative Inventory identifier or stock number.
- Activate Inventory.
- Publish authoritative Inventory-created or Inventory-activated Events.

Inventory must not:

- Recalculate Trade-In appraisal.
- Override Trade-In allowance.
- Decide payoff, lien, equity, or legal acquisition.
- Treat an intake request as completed intake.

### 6.9 Finance, Funding, and Deal Separation

Finance Application owns:

- Application and underwriting workflow.
- Lender Decision projection.
- Offer selection.
- Funding readiness.
- Contracting handoff.

Financial Contract owns:

- Contract lifecycle.
- Funding-request mutation.
- Funding Commands and idempotency.
- External funding Confirmation.
- Partial funding, shortfall, failure, reversal, and reconciliation.
- Activation based on accepted evidence.

Deal owns:

- Commercial transaction.
- Completion requirements.
- Read-only funding projection.
- Funding blockers and evidence references.
- Completion gate.

### 6.10 Action Classes and Execution

Action Classes are governed by the Constitution and System Architecture.

```text
Action Class 0
  = analysis only

Action Class 1
  = internal and reversible

Action Class 2
  = controlled Customer-facing or external operation

Action Class 3
  = binding or high-impact Decision
```

Action Class 2 requires exactly one valid authority path:

- Explicit Human Approval for the exact action instance; or
- An active pre-approved automation policy covering the exact action instance.

Action Class 3 requires an Authoritative Human Decision and, where applicable, authoritative external Confirmation.

Action Class 3 must not be downgraded by AI, Prompt, configuration, policy, urgency, or User request.

The AI Intelligence Layer must not execute external actions directly.

### 6.11 Event-Driven Architecture

Material accepted occurrences should publish Canonical Events.

Events must be:

- Immutable.
- Tenant-scoped.
- Uniquely identified by `event_id`.
- Producer-assigned before publication.
- Traceable.
- Versioned.
- Supported by authoritative timestamps.
- Linked through correlation and causation.
- Truthful about what occurred.
- Replay-safe where required.

The approved Event-producing boundary owns Event creation and `event_id` assignment.

The Event Backbone owns validation, routing, delivery, retry, dead-letter handling, and replay while preserving the Event identity.

### 6.12 Consumer Idempotency

The Event Backbone may deliver an Event more than once.

Consumers must:

- Validate `tenant_id`.
- Validate Event type and version.
- Detect previously processed `event_id` values.
- Prevent duplicate state transitions.
- Prevent duplicate Commands.
- Prevent duplicate Customer communication.
- Prevent duplicate Reservations, Allocations, Quotations, Inventory intake, funding requests, external updates, and other business effects.
- Record processing outcomes.

Events themselves are not described as idempotent.

Idempotency is a responsibility of:

- Event Producers during creation retry.
- Event Consumers.
- State transitions.
- Retryable Commands.
- External write operations.

Retryable Commands must use an approved:

```text
idempotency_key
```

### 6.13 Event Truthfulness

Events describe past facts.

They do not request future action.

ASOS must preserve distinctions such as:

```text
RecommendationGenerated
RecommendationApproved
ActionAuthorized
CommandCreated
CommandSent
ExternalConfirmationReceived
CanonicalOutcomeConfirmed
```

Corrections, reversals, cancellations, revocations, and supersessions are new occurrences.

They require new Events and new `event_id` values linked to affected Events.

Replay of the same Event preserves the original `event_id`.

### 6.14 AI Governance

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

AI output must distinguish among:

- Fact.
- Observation.
- Canonical Projection.
- Derived Intelligence.
- Assumption or hypothesis.
- Recommendation.
- Human Decision.
- Command.
- External Confirmation.

AI Agents must not independently create or confirm binding:

- Customer Consent.
- Customer-visible pricing commitments.
- Restricted discounts.
- Vehicle Reservation or Allocation outside approved authority.
- Finance Decisions.
- Trade-In approvals.
- Legal signatures.
- Contracts.
- Payments.
- Funding requests or Confirmations.
- Compliance clearance.
- Deal finalization.
- Vehicle sale Confirmation.
- Vehicle delivery Confirmation.

AI Recommendation, confidence, score, Opportunity priority, or interface state is not execution authority.

### 6.15 Deterministic Controls

The following controls must not depend solely on probabilistic AI reasoning:

- Authentication.
- Authorization.
- Tenant isolation.
- Required-field validation.
- Financial calculations.
- Approval thresholds.
- Consent enforcement.
- Action Class classification.
- Automation-policy evaluation.
- Lifecycle transition validation.
- Event-envelope validation.
- Event identity validation.
- Consumer deduplication.
- Data-retention controls.
- Prohibited-action enforcement.
- Command idempotency.
- External-confirmation validation.
- Security-policy enforcement.

AI Agents must not override deterministic controls.

### 6.16 Security by Design

Every Domain implementation must enforce:

- Authentication.
- Role-Based Access Control.
- Attribute-based controls where required.
- Least privilege.
- Tenant isolation.
- Branch, dealership, and legal-entity scope.
- Encryption in transit and at rest.
- Field-level protection.
- Sensitive-data masking.
- Audit logging.
- Consent and purpose limitation.
- Retention and deletion policies.
- Legal-hold controls.
- Secret-management requirements.
- AI-context restrictions.
- Command authorization.
- Replay authorization.
- Emergency suspension.

Access to an Object does not automatically grant authority to modify, approve, authorize, or execute actions against it.

---

## 7. Naming Conventions

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
- Every Tenant-owned Object includes `tenant_id`.
- External identifiers remain separate from Canonical identifiers.
- Event identity uses `event_id`.
- Retryable Command identity uses `idempotency_key`.

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

## 8. Change Management

A change to a Canonical Domain Model must include:

- Business justification.
- Constitutional impact.
- Architecture impact.
- Data-ownership impact.
- Impacted fields.
- Impacted relationships.
- Lifecycle impact.
- Authority impact.
- Event and Producer impact.
- API impact.
- Schema and database impact.
- AI-context impact.
- Security and privacy impact.
- Integration and external-authority impact.
- Migration or backward-compatibility plan.
- Updated document version and date.
- Review and approval evidence.

Breaking changes must not be introduced silently.

A repository commit does not automatically make a change an approved Production behavior.

Required governance, testing, migration, release, and monitoring controls must be completed.

### Changes Requiring Architecture and Event Review

The following require explicit Architecture and Event Catalog review:

- Moving mutation ownership between Domains.
- Adding a new Canonical Domain Object.
- Splitting or merging an existing Object.
- Changing `event_id` ownership or replay semantics.
- Changing Event Producer boundaries.
- Adding direct execution to a non-execution Domain.
- Downgrading an Action Class.
- Changing external authority.
- Changing Tenant-isolation behavior.
- Changing a terminal state or correction model.

---

## 9. Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS Product Vision](../../../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../../../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Event Catalog Governance](../02-Event-Catalog/README.md)
- [ASOS Canonical Documentation](../README.md)

---

## 10. Current Status

This document is the approved index and governance baseline for the ASOS Canonical Domain Model.

The baseline contains 14 Canonical Domain Objects.

The Canonical Event Catalog is established and authoritative for Event governance.

The following cross-Domain ownership boundaries are explicitly resolved:

- Lead and Qualified Lead qualification ownership.
- Trade-In and Inventory intake ownership.
- Finance Application, Financial Contract, and Deal funding ownership.
- Producer-owned `event_id` creation and Backbone preservation.
- Opportunity Action Class 2 authorization and execution separation.

Individual Object documents remain authoritative for their detailed fields, lifecycle, validation, API requirements, database design, and security requirements.

Implementation readiness also requires alignment with applicable:

- API Contracts.
- Data Schemas.
- Agent Contracts.
- Integration Contracts.
- Business Rules.
- Playbooks.
- Security and privacy controls.
- Migration and deployment profiles.

No Domain Object may bypass the Constitution, System Architecture, field-level authority, Tenant isolation, Action Classes, Command Orchestration, External Confirmation, or Event governance.
