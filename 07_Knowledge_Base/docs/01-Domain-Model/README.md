# ASOS Canonical Domain Model

## Purpose

This directory contains the canonical business objects for the ASOS AI Sales Operating System.

These documents define the shared domain language used across:

- Business workflows.
- Backend services.
- APIs and integrations.
- Databases and event streams.
- AI Agents and automation.
- Analytics and reporting.
- Security, compliance, and audit controls.

Each Domain Model file is the authoritative specification for its object.

Implementations must not introduce conflicting meanings, lifecycle states, relationships, or field definitions without updating the applicable canonical document.

## Canonical Domain Objects

| Order | Domain Object | Purpose |
| :---: | :--- | :--- |
| 1 | [Customer](./Customer.md) | Represents the canonical individual or organization interacting with the dealership. |
| 2 | [Vehicle](./Vehicle.md) | Represents the canonical Vehicle, its identity, specifications, ownership context, and operational attributes. |
| 3 | [Inventory Record](./InventoryRecord.md) | Represents the dealership stock, availability, location, pricing, reservation, allocation, and inventory lifecycle of a Vehicle. |
| 4 | [Lead](./Lead.md) | Represents an initial Customer inquiry or potential sales interest. |
| 5 | [Qualified Lead](./QualifiedLead.md) | Represents a Lead that has passed the required qualification criteria. |
| 6 | [Opportunity](./Opportunity.md) | Represents an active and commercially meaningful sales opportunity. |
| 7 | [Appointment](./Appointment.md) | Represents a scheduled Customer engagement, consultation, showroom visit, or test drive. |
| 8 | [Quotation](./Quotation.md) | Represents the governed commercial offer presented to a Customer. |
| 9 | [Trade-In](./TradeIn.md) | Represents the appraisal, valuation, payoff, and acquisition workflow for a Customer Vehicle. |
| 10 | [Finance Application](./FinanceApplication.md) | Represents a Customer request for Vehicle financing and its lender-underwriting lifecycle. |
| 11 | [Financial Contract](./FinancialContract.md) | Represents the legally governed financial agreement created from an approved finance offer. |
| 12 | [Deal](./Deal.md) | Represents the governed commercial transaction between the Customer and dealership. |
| 13 | [Interaction](./Interaction.md) | Represents an omnichannel communication or meaningful Customer engagement. |
| 14 | [Market Intelligence](./MarketIntelligence.md) | Represents verified market observations, evidence-backed analysis, commercial risks, and opportunities. |

## Core Sales Lifecycle

```text
Customer
   ↓
Lead
   ↓
Qualified Lead
   ↓
Opportunity
   ├── Appointment
   ├── Interaction
   ├── Vehicle
   │      ↓
   │  Inventory Record
   ├── Trade-In
   └── Quotation
          ↓
        Deal
          ├── Finance Application
          │        ↓
          │  Financial Contract
          └── Transaction Completion
```

Market Intelligence supports the lifecycle by providing governed pricing, demand, supply, inventory, competitor, and commercial context.

## Standard Document Structure

Every canonical Domain Model document contains the following 12 sections:

1. Object Purpose
2. Canonical Schema
3. Field Definitions
4. Enumerations
5. Validation Rules
6. State Machine
7. Relationships
8. Domain Events
9. AI Considerations
10. API Contract
11. Database Design
12. Security

This structure must remain consistent for all future canonical Domain Objects.

## Domain Modeling Principles

### Canonical Ownership

Each business concept must have one authoritative canonical object.

Duplicate objects with overlapping meanings must not be created without an approved architectural decision.

### Tenant Isolation

Every dealership-owned object must be scoped by `dealership_id`.

Cross-tenant access, linking, retrieval, event processing, AI context retrieval, and analytics are prohibited unless governed by an explicit authorized sharing model.

### Lifecycle Governance

Every stateful object must define:

- Allowed states.
- Allowed transitions.
- Forbidden transitions.
- Entry conditions.
- Exit conditions.
- Terminal states.

Lifecycle changes must be validated server-side and recorded in immutable history.

### Referential Integrity

Relationships between Domain Objects must:

- Remain within the permitted tenant scope.
- Reference existing canonical records.
- Match the same Customer and transaction context.
- Prevent circular or invalid dependency chains.
- Preserve historical references after supersession.

### Immutable Evidence

Commercial, financial, legal, consent, signature, funding, inventory, provider, and compliance evidence must remain immutable after becoming authoritative.

Corrections must use:

- Versioning.
- Supersession.
- Amendment.
- Controlled replacement.
- Governed redaction.

Authoritative historical records must not be silently overwritten.

### Inventory Consistency

Vehicle identity and dealership inventory context must remain separate.

- `Vehicle` defines the canonical physical or catalogued asset.
- `Inventory Record` defines how that Vehicle is held, located, priced, prepared, reserved, allocated, sold, transferred, or retired by a dealership.
- Vehicle availability must come from the authoritative Inventory Record.
- Reservations and allocations must use concurrency-safe operations.
- Sold, delivered, transferred, returned, and retired inventory records must remain historically traceable.

### Event-Driven Architecture

Material lifecycle changes must emit canonical Domain Events.

Events must be:

- Tenant-scoped.
- Idempotent.
- Traceable.
- Versioned.
- Replay-safe where required.
- Supported by authoritative timestamps and correlation identifiers.

### AI Governance

AI Agents may assist with:

- Classification.
- Extraction.
- Summarization.
- Recommendation.
- Completeness checking.
- Workflow coordination.
- Context retrieval.

AI Agents must not independently create binding:

- Customer consent.
- Pricing commitments.
- Discounts.
- Vehicle reservations outside approved authority.
- Deal allocations.
- Finance approvals.
- Legal signatures.
- Contracts.
- Payments.
- Funding confirmation.
- Compliance clearance.
- Vehicle sale confirmation.
- Vehicle-delivery confirmation.

High-risk, conflicting, low-confidence, legal, financial, identity, inventory, fraud, or compliance decisions require authorized Human Review.

### Security by Design

Every domain implementation must enforce:

- Role-Based Access Control.
- Least-privilege access.
- Tenant isolation.
- Encryption in transit and at rest.
- Field-level protection for sensitive data.
- Audit logging.
- Consent and purpose limitation.
- Retention and deletion policies.
- Legal-hold controls where applicable.

## Naming Conventions

- Markdown filenames use PascalCase.
- Canonical object names use singular form.
- Database tables use plural `snake_case`.
- Fields use `snake_case`.
- REST resources use plural `kebab-case`.
- GraphQL types use PascalCase.
- Domain Events use past-tense PascalCase.
- Enumeration values use uppercase `SNAKE_CASE`.
- Primary identifiers use UUIDv4 unless explicitly specified otherwise.

## Change Management

Changes to a canonical Domain Model must include:

- Business justification.
- Impacted fields or relationships.
- Lifecycle impact.
- API and database impact.
- Domain Event impact.
- AI-context impact.
- Security and compliance impact.
- Migration or backward-compatibility plan.
- Updated document version and audit history.

Breaking changes must not be introduced silently.

## Completion Status

The initial ASOS Canonical Domain Model contains 14 completed objects.

All listed objects include the standard 12-section specification and are ready for architectural review, implementation planning, and controlled future expansion.
