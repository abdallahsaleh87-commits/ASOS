# ASOS Data Ownership and Systems of Record

**Document Status:** Approved Baseline  
**Version:** 1.0.0  
**Document Owner:** ASOS Architecture  
**Applies To:** Domain Models, Integrations, APIs, Events, AI Agents, Analytics, and Operational Workflows

---

## 1. Purpose

This document defines how data ownership, source authority, synchronization, conflict resolution, and write permissions operate across the ASOS AI Sales Operating System.

ASOS integrates with dealership systems such as:

- Customer Relationship Management systems.
- Dealer Management Systems.
- Inventory Management Systems.
- Finance and Insurance platforms.
- OEM systems.
- Website forms and lead providers.
- Communication channels.
- Calendar and appointment systems.
- Document and contract platforms.
- Spreadsheet and legacy operational sources.
- Market and pricing providers.

These systems may contain overlapping, delayed, incomplete, or conflicting data.

Without a formal ownership model, different services, AI Agents, databases, and Users may interpret the same information differently.

This document ensures that every governed field has a defined:

- External System of Record.
- ASOS Canonical Owner.
- Write Authority.
- Synchronization Direction.
- Conflict-Resolution Rule.
- Freshness Requirement.
- Evidence Requirement.
- Security Classification.
- Retention Policy.

The objectives are to:

- Prevent duplicate sources of truth.
- Prevent silent data overwrites.
- Preserve legal and operational evidence.
- Maintain traceability to the original source.
- Allow ASOS to normalize data without falsely claiming legal ownership.
- Define which workflow states ASOS may authoritatively own.
- Ensure AI recommendations remain distinguishable from authoritative facts.
- Support reliable Event-Driven Architecture.
- Enable safe integration with multiple dealership technology stacks.

---

## 2. Scope

This policy applies to all ASOS data, including:

- The 14 Canonical Domain Objects.
- External source records.
- ASOS canonical projections.
- ASOS-native workflow states.
- Domain Events.
- API commands.
- Integration payloads.
- AI-generated outputs.
- Human decisions.
- Documents and evidence.
- Analytics and KPI calculations.
- Search indexes.
- Vector embeddings.
- Audit records.
- Cached data.
- Data exports.
- Backups.

The policy applies across:

- Single-dealership deployments.
- Multi-branch dealerships.
- Dealer groups.
- Multi-tenant SaaS deployments.
- New Vehicle operations.
- Used Vehicle operations.
- Trade-In operations.
- Finance workflows.
- Customer follow-up.
- Market Intelligence.
- Management reporting.

This document does not replace:

- Legal requirements.
- OEM agreements.
- Lender agreements.
- Government registration rules.
- Accounting policies.
- Data-protection regulations.
- Dealership-specific authorization policies.

Where an external legal or contractual requirement is stricter, the stricter requirement applies.

---

## 3. Core Definitions

### External System of Record

An External System of Record is the system recognized as the primary operational, contractual, financial, regulatory, or legal authority for a specific field or process.

Examples may include:

- CRM for the original Lead record.
- DMS for finalized Deal records.
- Inventory system for physical stock status.
- Lender platform for finance approval.
- Payment provider for transaction confirmation.
- Government system for registration.
- Communication provider for original message delivery.
- Document platform for signed contract evidence.

An External System of Record may vary by dealership deployment.

ASOS must not assume that one vendor is authoritative for every dealership.

### ASOS Canonical Projection

An ASOS Canonical Projection is the normalized ASOS representation of information received from one or more sources.

It exists to provide:

- Consistent field names.
- Consistent enumerations.
- Unified relationships.
- Normalized timestamps.
- Source provenance.
- AI-ready context.
- Cross-system analysis.
- Event-driven processing.

A Canonical Projection does not automatically replace the external System of Record.

### ASOS Authoritative Workflow State

ASOS Authoritative Workflow State is a state that ASOS is permitted to create and govern because the workflow is natively owned by ASOS.

Examples may include:

- AI qualification assessment.
- Human Review Task.
- Recommended next action.
- Lead-priority score.
- Opportunity-risk assessment.
- Follow-up recommendation.
- AI Agent Run.
- Manager decision recorded inside ASOS.
- Internal exception workflow.
- ASOS notification.
- ASOS orchestration status.

ASOS Authoritative Workflow State must not be confused with an external legal or financial transaction.

### Authoritative Evidence

Authoritative Evidence is immutable or controlled evidence supporting a fact or decision.

Examples include:

- Signed contract.
- Lender approval response.
- Payment-provider confirmation.
- Vehicle handover document.
- Registration document.
- Customer consent record.
- Inspection report.
- Supplier invoice.
- DMS transaction identifier.
- Provider webhook with verified signature.

### Derived Intelligence

Derived Intelligence is information calculated or generated by ASOS.

Examples include:

- Lead score.
- Demand score.
- Conversion probability.
- Lost-opportunity risk.
- Recommended discount.
- Forecast.
- Inventory-pressure score.
- AI summary.
- Suggested Customer response.
- Manager action priority.

Derived Intelligence is not an authoritative business fact unless an authorized Human or governed workflow adopts it.

### Human Decision Record

A Human Decision Record captures an authorized Human decision made using ASOS.

It must preserve:

- Decision maker.
- Role.
- Decision.
- Reason.
- Supporting evidence.
- Related AI recommendation.
- Timestamp.
- Domain Object version.
- Approval scope.

### Field Authority

Field Authority defines which source is allowed to provide or change a specific field.

Field Authority may differ between fields inside the same Domain Object.

### Canonical Owner

The Canonical Owner is the ASOS Domain Object responsible for the normalized meaning, validation, relationships, and lifecycle of a field.

Canonical ownership does not necessarily mean external legal ownership.

---

## 4. Data Authority Model

ASOS uses five data-authority categories.

### Category A — External Authoritative Data

This data is controlled by an external System of Record.

Examples:

- Lender finance decision.
- Payment confirmation.
- Signed Financial Contract.
- Government registration status.
- Original WhatsApp or email delivery result.
- DMS Deal posting.
- Inventory-system physical stock movement.

ASOS may:

- Read it.
- Normalize it.
- Validate it.
- Cache it.
- Reference it.
- Detect conflicts.
- Request an external update through an approved command.

ASOS must not silently overwrite it.

### Category B — ASOS Canonical Projection

This data is a normalized representation of external data.

Examples:

- Normalized Customer profile.
- Canonical Vehicle identity.
- Canonical Inventory Record.
- Unified Interaction timeline.
- Normalized Lead.
- Normalized Deal summary.

ASOS may maintain the projection, but every authoritative field must preserve its provenance.

### Category C — ASOS-Native Operational Data

This data is created and governed directly by ASOS.

Examples:

- AI Agent Run.
- Human Review Task.
- Recommended action.
- Decision explanation.
- Internal workflow status.
- Exception state.
- Manager-priority queue.
- Notification status.
- Data-quality issue.

ASOS is the System of Record for this category.

### Category D — Derived Intelligence

This data is calculated by ASOS and must remain distinguishable from facts.

Examples:

- Scores.
- Predictions.
- Recommendations.
- Forecasts.
- Summaries.
- Risk levels.
- Suggested markdown.
- Suggested follow-up time.

Derived Intelligence must preserve:

- Model or formula version.
- Input versions.
- Confidence.
- Calculation timestamp.
- Expiry or freshness period.
- Human-approval status.

### Category E — Authoritative Human Decision

This data is created when an authorized Human accepts, rejects, modifies, or overrides a recommendation or workflow decision.

ASOS may be the System of Record for the decision itself.

The underlying external transaction may still require confirmation from another System of Record.

Example:

A Sales Manager may approve a discount in ASOS, but the finalized Deal price may remain authoritative only after the DMS accepts and records the Deal.

---

## 5. Canonical Domain Ownership Matrix

The following matrix defines the default ownership model.

Dealership-specific integration configuration may override the external System of Record, but it must not change the Canonical Domain Owner without an approved architectural decision.

| Domain Object | ASOS Canonical Owner | Default External Authority | ASOS-Native Authority |
| :--- | :--- | :--- | :--- |
| Customer | Customer | CRM, DMS, identity or consent systems | Customer intelligence, normalized profile, internal segmentation |
| Vehicle | Vehicle | DMS, OEM, registration, inspection provider | Canonical identity matching and normalized specifications |
| Inventory Record | Inventory Record | DMS or Inventory Management System | Inventory analytics, pressure scores, recommendations |
| Lead | Lead | CRM, website, lead provider, OEM platform | Lead normalization, enrichment, internal scoring |
| Qualified Lead | Qualified Lead | May not exist externally | Qualification assessment, review state, qualification evidence |
| Opportunity | Opportunity | CRM or DMS where supported | Risk analysis, prioritization, internal opportunity intelligence |
| Appointment | Appointment | CRM, calendar, booking provider | Scheduling orchestration and internal readiness state |
| Quotation | Quotation | DMS, ERP, approved quoting platform | Draft recommendation and approval workflow |
| Trade-In | Trade-In | DMS, appraisal provider, finance or ownership systems | Appraisal workflow, comparison, recommendation |
| Finance Application | Finance Application | F&I or lender platform | Application completeness, routing, internal workflow |
| Financial Contract | Financial Contract | Lender, DMS, contract or document platform | Contract projection and compliance monitoring |
| Deal | Deal | DMS, ERP, accounting system | Deal intelligence, risk assessment, internal orchestration |
| Interaction | Interaction | Communication provider for original communication | Unified timeline, classification, summary, follow-up intelligence |
| Market Intelligence | Market Intelligence | Market providers and evidence sources | Normalized observations, analysis, insights, recommendations |

### Customer

The Customer object owns the normalized Customer identity and relationship context.

External authority may own:

- Original Customer record.
- Legal identity.
- Contact details.
- Consent.
- Communication preferences.
- Address.
- Tax information.

ASOS may own:

- Customer summary.
- Engagement score.
- Propensity score.
- Journey stage.
- Internal recommendations.
- Data-quality status.

### Vehicle

Vehicle owns:

- VIN and chassis identity.
- Make.
- Model.
- Trim.
- Model year.
- Technical specifications.
- Odometer evidence.
- Source provenance.

Vehicle does not own:

- Stock number.
- Price.
- Availability.
- Reservation.
- Deal allocation.
- Sale status.
- Delivery status.

These belong to Inventory Record, Quotation, Deal, or the relevant workflow.

### Inventory Record

Inventory Record owns the ASOS canonical representation of:

- Dealership stock context.
- Physical or controlled location.
- Availability.
- Preparation.
- Reservation.
- Allocation.
- Pricing context.
- Inventory aging.
- Stock exit.

The external Inventory Management System or DMS may remain the operational authority for physical stock.

### Lead

Lead owns the normalized initial sales-interest record.

The original source may remain authoritative for:

- Lead receipt time.
- Source campaign.
- Original form data.
- Provider reference.

ASOS may own:

- Enrichment.
- Classification.
- Priority.
- Follow-up recommendation.
- Duplicate detection.

### Qualified Lead

Qualified Lead is normally an ASOS Authoritative Workflow State.

It must preserve:

- Qualification criteria.
- Evidence.
- Score.
- Human-review state.
- Qualification timestamp.
- Source Lead version.

It must not silently overwrite the original Lead status in an external CRM without an approved write-back workflow.

### Opportunity

Opportunity owns the canonical representation of a commercially meaningful sales pursuit.

The CRM may remain authoritative for its operational stage.

ASOS may own:

- Opportunity health.
- Risk.
- Forecast.
- Next-best action.
- Internal management priority.

### Appointment

The booking provider, CRM, or calendar may remain authoritative for confirmed attendance time.

ASOS may own:

- Appointment recommendation.
- Scheduling orchestration.
- Readiness checklist.
- No-show risk.
- Follow-up workflow.

### Quotation

Final Customer-visible pricing must come from an approved quoting authority.

ASOS may:

- Recommend a Quotation.
- Check completeness.
- Validate price boundaries.
- Route approval.
- Generate an approved draft.

ASOS must not create a binding Quotation without authorized pricing approval.

### Trade-In

External authorities may own:

- Legal ownership evidence.
- Lien or payoff.
- Inspection result.
- External valuation.
- Acquisition posting.

ASOS may own:

- Trade-In workflow.
- Valuation comparison.
- Recommendation.
- Risk assessment.
- Appraisal completeness.

### Finance Application

The lender or F&I platform owns:

- Finance decision.
- Approved amount.
- Interest rate.
- Conditions.
- Rejection reason where permitted.
- Funding status.

ASOS may own:

- Completeness status.
- Routing.
- Document checklist.
- Internal progress state.
- Customer communication recommendation.

### Financial Contract

The signed or provider-confirmed contract is authoritative.

ASOS may maintain:

- Canonical contract projection.
- Compliance checklist.
- Expiry monitoring.
- Missing-document detection.

ASOS must not represent an unsigned draft as an active Financial Contract.

### Deal

The DMS, ERP, or approved transaction platform normally owns the finalized Deal.

ASOS may own:

- Internal Deal health.
- Exception workflow.
- Approval history.
- Risk assessment.
- Forecast contribution.
- Recommended action.

### Interaction

The communication provider owns original message transport evidence.

ASOS may own:

- Unified Interaction Record.
- Classification.
- Sentiment.
- Summary.
- Intent.
- Follow-up recommendation.
- Relationship to Lead, Opportunity, or Deal.

Original communication content must remain traceable to the source.

### Market Intelligence

Evidence providers own the raw source information.

ASOS owns:

- Normalized observation.
- Evidence links.
- Analysis.
- Confidence.
- Commercial risk.
- Commercial opportunity.
- Recommendation.

Market Intelligence must never present an unsupported AI inference as verified market fact.

---

## 6. Field-Level Ownership Requirements

Every governed field must have metadata defining:

| Metadata Field | Purpose |
| :--- | :--- |
| `system_of_record` | External or internal system recognized as authoritative |
| `canonical_owner` | ASOS Domain Object responsible for normalized meaning |
| `source_system` | Actual system from which the current value originated |
| `source_record_id` | Record identifier in the source system |
| `source_field_name` | Original source field |
| `source_authority` | Authority level of the source |
| `write_authority` | Roles or systems permitted to change the value |
| `sync_direction` | Inbound, outbound, bidirectional, or internal-only |
| `conflict_policy` | Rule used when values disagree |
| `freshness_sla` | Maximum acceptable data age |
| `verified_at` | Time of authoritative verification |
| `effective_from` | Time from which the value is valid |
| `effective_until` | Time until which the value is valid |
| `sensitivity_classification` | Security and privacy classification |
| `retention_policy` | Required retention period |
| `evidence_reference` | Supporting authoritative evidence |
| `record_version` | Version used for concurrency and history |
| `last_synced_at` | Last successful synchronization |
| `last_sync_status` | Success, failure, partial, or pending |
| `conflict_status` | No conflict, unresolved, under review, or resolved |

### Required Field Authority Map

Every canonical record must support a field-level authority map.

Example:

```json
{
  "email": {
    "system_of_record": "CRM",
    "source_system": "SALESFORCE",
    "source_record_id": "CUST-98217",
    "source_authority": "CUSTOMER_CONFIRMED",
    "write_authority": ["CUSTOMER", "AUTHORIZED_CRM_USER"],
    "sync_direction": "BIDIRECTIONAL",
    "conflict_policy": "MOST_RECENT_VERIFIED",
    "verified_at": "2026-08-01T10:30:00Z"
  },
  "lead_score": {
    "system_of_record": "ASOS",
    "source_system": "ASOS_SCORING_SERVICE",
    "source_authority": "DERIVED_INTELLIGENCE",
    "write_authority": ["ASOS_SCORING_SERVICE"],
    "sync_direction": "INTERNAL_ONLY",
    "conflict_policy": "LATEST_VALID_MODEL_RUN",
    "verified_at": null
  }
}
```

### No Object-Level Assumption

The system must not assume that one complete Domain Object has only one System of Record.

Example:

A Customer may have:

- Name from CRM.
- National identity from an identity provider.
- Consent from a consent platform.
- Finance information from a lender.
- Communication preferences from a messaging platform.
- Lead score from ASOS.

Authority must therefore be defined at field or controlled field-group level.

---

## 7. Synchronization Rules

### Supported Synchronization Directions

#### Inbound

External system to ASOS.

Used when ASOS observes and normalizes an external source.

#### Outbound

ASOS to external system.

Used only where ASOS has authorized write permission.

#### Bidirectional

Both ASOS and the external system may submit changes.

Bidirectional synchronization requires explicit conflict-resolution rules.

#### Internal Only

Data remains authoritative inside ASOS.

Examples include AI recommendations, Agent Runs, and internal workflow status.

### Supported Synchronization Mechanisms

- Signed webhooks.
- Change Data Capture.
- Approved API polling.
- Scheduled batch import.
- Secure file import.
- Manual governed import.
- Outbox pattern.
- Event streaming.
- Approved API command.
- Reconciliation Job.

### Synchronization Requirements

Every synchronization operation must preserve:

- Tenant.
- Source system.
- Source record.
- Source event or batch ID.
- Source timestamp.
- Retrieval timestamp.
- Payload or document hash.
- Processing result.
- Idempotency key.
- Correlation ID.
- Record version.
- Retry count.
- Error reason.
- Reconciliation state.

### Idempotency

Repeated processing of the same source change must not create:

- Duplicate Customers.
- Duplicate Leads.
- Duplicate Vehicles.
- Duplicate Inventory Records.
- Duplicate Payments.
- Duplicate reservations.
- Duplicate Deals.
- Duplicate Interactions.

### Freshness

Every integration must define a Freshness SLA.

Examples:

| Data Type | Suggested Freshness Requirement |
| :--- | :--- |
| Inventory availability | Near real time |
| Reservation and allocation | Real time or transactionally synchronized |
| Customer contact changes | Minutes |
| Lead receipt | Near real time |
| Appointment confirmation | Near real time |
| Finance decision | Near real time |
| Payment confirmation | Real time |
| Signed contract | Near real time |
| Market Intelligence | Provider-dependent |
| Historical reporting | Daily or scheduled |

The exact SLA must be configured per deployment.

### Stale Data Protection

When authoritative data exceeds its Freshness SLA:

- The record must be marked stale.
- High-risk actions may be blocked.
- AI outputs must disclose the stale-data condition.
- A synchronization or Human Review Task must be created.
- Customer-facing availability, pricing, finance, and delivery claims must not rely on stale data.

### Reconciliation

Every integration must support periodic reconciliation between:

- External source count.
- ASOS projection count.
- Changed records.
- Failed records.
- Missing records.
- Duplicate records.
- Version mismatches.
- Unresolved conflicts.

---

## 8. Conflict Resolution

### General Principle

Conflicts must not be silently resolved by overwriting one value with another.

Every material conflict must preserve:

- Competing values.
- Competing sources.
- Source authority.
- Timestamps.
- Evidence.
- Current operational use.
- Resolution policy.
- Final decision.
- Decision maker.
- Resolution timestamp.

### Default Authority Precedence

Unless a field-specific policy overrides it, authority should normally follow this order:

1. Legal or regulatory evidence.
2. Signed contractual evidence.
3. Verified financial-provider response.
4. Verified OEM or government source.
5. Verified DMS or operational System of Record.
6. Authorized Human decision with evidence.
7. Verified Customer confirmation.
8. External operational source.
9. ASOS Canonical Projection.
10. AI-extracted data.
11. AI inference.
12. Unverified manual entry.

This precedence is not universal.

Each governed field must define its own approved policy.

### Conflict-Resolution Strategies

Permitted strategies include:

- Highest-authority source.
- Most recent verified value.
- Legal evidence wins.
- Customer-confirmed value.
- External System of Record wins.
- Human Review required.
- Merge values.
- Reject update.
- Preserve both values with effective dates.
- Controlled supersession.

### Material Conflicts

Human Review is mandatory for conflicts involving:

- Customer legal identity.
- Consent.
- VIN or chassis number.
- Vehicle ownership.
- Inventory availability.
- Customer-visible price.
- Reservation.
- Deal allocation.
- Deal status.
- Payment.
- Finance approval.
- Signed contract.
- Vehicle delivery.
- Compliance.
- Fraud.
- Legal hold.

### Non-Material Conflicts

Low-risk formatting differences may be normalized automatically.

Examples:

- Telephone formatting.
- Letter case.
- Whitespace.
- Approved date formatting.
- Known manufacturer-name aliases.

Normalization must preserve the original source value where required.

### Conflict State

A record with unresolved material conflict must include:

- `conflict_status`
- `conflicting_fields`
- `conflicting_sources`
- `conflict_detected_at`
- `workflow_restrictions`
- `review_task_id`

---

## 9. Write Authority and Command Governance

### Read Does Not Imply Write

The ability to read an external field does not authorize ASOS to change it.

Every outbound update must have:

- Approved business purpose.
- Authorized User or service.
- Tenant scope.
- Current record version.
- Idempotency key.
- Validation.
- Required Human Approval.
- External provider permission.
- Audit record.

### Command Ownership

Commands must be handled by the service that owns the applicable workflow.

Examples:

| Command | Responsible Owner |
| :--- | :--- |
| Update Vehicle identity | Vehicle domain service |
| Change inventory availability | Inventory Record domain service |
| Reserve Vehicle | Inventory reservation workflow |
| Approve discount | Authorized pricing workflow |
| Submit finance application | Finance Application workflow |
| Approve finance | External lender only |
| Mark Deal finalized | DMS or approved Deal authority |
| Confirm Payment | Payment provider or finance authority |
| Confirm delivery | Vehicle Delivery workflow with authoritative evidence |

### Write-Back Status

An ASOS outbound command must track:

- `PENDING`
- `SENT`
- `ACCEPTED`
- `REJECTED`
- `FAILED`
- `TIMED_OUT`
- `RECONCILIATION_REQUIRED`

ASOS must not update the canonical projection as externally accepted until authoritative confirmation is received.

### Optimistic Updates

Optimistic user-interface updates may be used only when:

- Clearly marked as pending.
- Reversible.
- Not legally or financially binding.
- Reconciled with the authoritative source.
- Restricted from triggering dependent irreversible actions.

### Human Approval

Human Approval is required before ASOS initiates binding actions involving:

- Customer consent.
- Discount exceptions.
- Price commitments.
- Trade-In acquisition.
- Finance submission where required.
- Contract acceptance.
- Deal finalization.
- Payment or refund.
- Vehicle release.
- Vehicle delivery.
- Legal or compliance override.

### AI Write Restrictions

AI Agents must not independently:

- Change authoritative identity.
- Approve finance.
- Confirm Payment.
- Sign contracts.
- Finalize Deals.
- Remove legal holds.
- Confirm Vehicle delivery.
- Override active reservations.
- Change restricted pricing.
- Resolve fraud or compliance conflicts.

---

## 10. Event Ownership and Provenance

### Event Categories

#### Source Events

Source Events report a change observed in an external System of Record.

Examples:

- `CRMLeadReceived`
- `DMSDealUpdated`
- `LenderDecisionReceived`
- `PaymentProviderTransactionConfirmed`
- `InventorySystemVehicleReserved`

The integration adapter is the producer of the normalized Source Event.

The event must identify the original source.

#### Canonical Domain Events

Canonical Domain Events report an accepted change in the ASOS canonical projection.

Examples:

- `CustomerCanonicalProfileUpdated`
- `VehicleVerified`
- `InventoryRecordSynchronized`
- `LeadCreated`
- `DealProjectionUpdated`

The applicable ASOS Domain Service is the producer.

#### ASOS Workflow Events

These report ASOS-native workflow changes.

Examples:

- `LeadQualified`
- `HumanReviewRequested`
- `RecommendationAccepted`
- `FollowUpTaskCreated`
- `OpportunityRiskEscalated`

ASOS is authoritative for these events.

#### Derived Intelligence Events

These report new AI or analytical outputs.

Examples:

- `LeadScoreCalculated`
- `DealRiskDetected`
- `InventoryMarkdownRecommended`
- `SalesForecastUpdated`

These events must never be interpreted as external transaction confirmation.

### Event Producer Rule

The service that authoritatively accepts the state change is the Event producer.

An AI Agent that recommends a state change is not the producer of the resulting authoritative business Event.

Example:

```text
AI Agent recommends Vehicle reservation
        ↓
Authorized workflow validates and approves
        ↓
Inventory system confirms reservation
        ↓
Inventory domain service emits authoritative reservation Event
```

### Event Provenance

Every material Event must include:

```json
{
  "event_id": "uuid",
  "event_type": "LeadQualified",
  "event_version": 1,
  "dealership_id": "uuid",
  "aggregate_id": "uuid",
  "aggregate_type": "QualifiedLead",
  "occurred_at": "2026-08-01T12:00:00Z",
  "recorded_at": "2026-08-01T12:00:01Z",
  "producer": "qualified-lead-service",
  "source_system": "ASOS",
  "source_record_id": "uuid",
  "source_event_id": "external-event-id-or-null",
  "correlation_id": "uuid",
  "causation_id": "uuid",
  "idempotency_key": "string",
  "actor_type": "USER",
  "actor_id": "uuid",
  "authority_category": "ASOS_WORKFLOW_STATE",
  "evidence_references": [],
  "payload": {}
}
```

### Event Truthfulness

Event names must describe what actually occurred.

Permitted:

- `FinanceApplicationSubmitted`
- `FinanceDecisionReceived`
- `FinanceOfferRecommended`

Prohibited misleading Event:

- `FinanceApproved` when only an AI recommendation exists.

---

## 11. AI, Security, and Audit Governance

### AI Context Requirements

AI Agents must receive data with authority metadata.

The context should distinguish:

- Verified fact.
- External authoritative fact.
- ASOS canonical projection.
- Human decision.
- Derived metric.
- AI inference.
- Unverified source.
- Stale data.
- Conflicting data.

### AI Output Labelling

Every AI output must specify:

- Output type.
- Supporting data.
- Source authority.
- Input freshness.
- Model.
- Prompt version.
- Confidence.
- Assumptions.
- Required Human Approval.
- Expiration time where applicable.

### AI Conflict Behaviour

When material sources conflict, AI must:

- Identify the conflict.
- Avoid choosing an authoritative value without permission.
- Explain the operational risk.
- Recommend Human Review.
- Restrict high-risk downstream action.

### Tenant Isolation

All ownership, synchronization, and conflict operations must enforce `dealership_id`.

Cross-tenant access is prohibited unless governed by:

- Explicit data-sharing agreement.
- Authorized stock-transfer workflow.
- Approved group-level analytics.
- Anonymization or aggregation.
- Auditable privileged access.

### Least Privilege

Services and Users must receive only the permissions needed for:

- Required fields.
- Required Objects.
- Required branches.
- Required workflows.
- Required time period.

### Sensitive Data

Sensitive fields must use:

- Encryption in transit.
- Encryption at rest.
- Field-level access controls.
- Masking.
- Audit logging.
- Retention limits.
- Purpose limitation.

### Audit Requirements

Every authoritative or material operation must record:

- Tenant.
- Domain Object.
- Record ID.
- Field or state changed.
- Previous value or hash.
- New value or hash.
- Source.
- Source authority.
- Actor.
- Role.
- Business purpose.
- Evidence.
- Timestamp.
- Correlation ID.
- Record version.
- AI involvement.
- Human approval.
- External confirmation status.

### Immutable History

The following must not be silently overwritten:

- Customer consent.
- Verified Vehicle identity.
- Inventory reservation and allocation history.
- Approved Quotation.
- Trade-In appraisal approval.
- Finance decision.
- Signed Financial Contract.
- Payment confirmation.
- Deal finalization.
- Vehicle delivery evidence.
- Human override.
- AI recommendation used in a material decision.

Corrections must use:

- Versioning.
- Supersession.
- Amendment.
- Compensating transaction.
- Controlled redaction.
- Controlled reversal.

---

## 12. Implementation and Acceptance Criteria

### Required Architecture Components

ASOS implementation must provide:

- Source Registry.
- Field Authority Registry.
- Integration Configuration.
- Canonical Mapping Registry.
- Conflict-Resolution Engine.
- Synchronization Service.
- Idempotency Store.
- Event Provenance Store.
- Human Review Workflow.
- Audit Log.
- Data Freshness Monitor.
- Reconciliation Jobs.
- Write-Back Status Tracking.
- Evidence Vault.
- Tenant-aware access controls.

### Source Registry

The Source Registry must define:

- Source-system ID.
- Source type.
- Tenant.
- Integration owner.
- Authentication method.
- Authority level.
- Supported Objects.
- Supported fields.
- Read permission.
- Write permission.
- Freshness SLA.
- Retry policy.
- Reconciliation schedule.
- Security classification.

### Field Authority Registry

The Field Authority Registry must define for every governed field:

- Canonical Object.
- Field name.
- Default System of Record.
- Permitted sources.
- Source precedence.
- Write authority.
- Conflict policy.
- Verification requirement.
- Freshness SLA.
- Sensitivity.
- Retention.

### Domain Model Requirements

Every Canonical Domain Model must identify:

- Which fields are externally authoritative.
- Which fields are ASOS projections.
- Which fields are ASOS-native.
- Which fields are derived.
- Which fields require Human Approval.
- Which fields may be written back.
- Which fields must remain immutable.
- Which Events represent source change.
- Which Events represent accepted canonical change.

### API Requirements

Every mutating API must enforce:

- Authentication.
- Tenant.
- Authorization.
- Record version.
- Idempotency.
- Business validation.
- Authority validation.
- Conflict checks.
- Required approvals.
- Audit.
- External confirmation where applicable.

### Event Requirements

Every material Event must include:

- Provenance.
- Authority category.
- Source reference.
- Event version.
- Tenant.
- Actor.
- Correlation.
- Causation.
- Idempotency.
- Authoritative timestamp.

### Acceptance Criteria

This architecture decision is considered implemented when:

- Every production source is registered.
- Every critical field has a defined authority.
- No material field is silently overwritten.
- External and ASOS-native states are distinguishable.
- AI inference is never presented as authoritative fact.
- Stale data is detected and labelled.
- Reservation, allocation, pricing, finance, Payment, Deal, and delivery workflows identify their authoritative system.
- Every outbound command tracks external acceptance.
- Every material conflict creates a controlled resolution path.
- Every authoritative change is auditable.
- Every Event identifies its producer and authority category.
- Tenant isolation is enforced across integrations and projections.
- Reconciliation reports identify missing, duplicate, failed, and conflicting records.

### Architectural Decision

ASOS is the authoritative owner of:

- Its Canonical Domain definitions.
- Its normalized projections.
- Its native workflow states.
- Its AI recommendations.
- Its Human Review records.
- Its decision explanations.
- Its Agent Runs.
- Its internal audit and orchestration records.

ASOS is not automatically the legal or operational System of Record for external dealership transactions.

External authority remains field-specific and deployment-specific.

No service, Agent, API, or User may treat an ASOS projection, recommendation, or pending command as externally confirmed without authoritative evidence.
