# ASOS Data Ownership and Systems of Record

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Data Governance and Architecture  
**Applies To:** Domain Models, data services, integrations, APIs, Events, Commands, AI Agents, analytics, documents, and operational workflows  
**Effective Date:** 2026-08-01  
**Last Updated:** 2026-08-01  

---

## 1. Purpose and Policy Authority

This document defines how data ownership, source authority, synchronization, conflict resolution, write permissions, evidence, and provenance operate across the ASOS AI Sales Operating System.

ASOS integrates with different dealership technology environments, including:

- Customer Relationship Management systems.
- Dealer Management Systems.
- Inventory Management Systems.
- Finance and Insurance platforms.
- Lender systems.
- OEM systems.
- Website forms and Lead providers.
- Communication providers.
- Calendar and Appointment platforms.
- Trade-In and valuation providers.
- Contract and document platforms.
- Payment providers.
- Government or regulatory services.
- Market and pricing providers.
- Approved spreadsheets and legacy operational sources.

These sources may contain overlapping, delayed, incomplete, stale, duplicated, or conflicting information.

This policy ensures that every governed field, state, Decision, Command, and material Event has a defined:

- Authority classification.
- External System of Record where applicable.
- ASOS Canonical Owner.
- Source provenance.
- Read authority.
- Write authority.
- Synchronization direction.
- Conflict-resolution policy.
- Freshness requirement.
- Evidence requirement.
- Security classification.
- Retention requirement.
- Audit requirement.

The objectives are to:

- Prevent competing sources of truth.
- Prevent silent overwrites.
- Prevent AI inference from being represented as authoritative fact.
- Preserve legal, financial, contractual, and operational evidence.
- Maintain traceability to original sources.
- Allow ASOS to normalize data without falsely claiming external authority.
- Define the workflow states for which ASOS may be authoritative.
- Separate Recommendations, Human Decisions, Commands, and External Confirmations.
- Support reliable Event-Driven Architecture.
- Support multiple dealerships, branches, dealer groups, and technology stacks.
- Enforce tenant isolation.
- Enable safe reconciliation and controlled write-back.

This policy is subordinate to:

1. Applicable legal and regulatory requirements.
2. Binding contractual, OEM, lender, and governmental requirements.
3. The ASOS Constitution.

All lower-level Domain Models, Events, APIs, Schemas, Business Rules, Playbooks, Prompts, integrations, and dealership configurations must comply with this policy.

---

## 2. Scope and Governing Boundaries

This policy applies to all information processed by ASOS, including:

- The 14 Canonical Domain Objects.
- External source records.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.
- Recommendations.
- Internal Tasks.
- Human Review records.
- Commands and Command status.
- Source changes.
- Domain Events.
- API requests and responses.
- Integration payloads.
- Documents and supporting evidence.
- Customer communications.
- Analytics and KPI calculations.
- Search indexes.
- Vector embeddings.
- Caches.
- Audit records.
- Data exports.
- Backups.
- Data-quality records.
- Reconciliation records.

This policy applies across:

- Multi-tenant Software-as-a-Service deployments.
- Dedicated tenant deployments.
- Single-dealership deployments.
- Multi-branch dealerships.
- Dealer groups.
- New Vehicle operations.
- Used Vehicle operations.
- Trade-In operations.
- Finance workflows.
- Customer follow-up.
- Appointment workflows.
- Quotation workflows.
- Deal workflows.
- Market Intelligence.
- Management reporting.
- Pilot and Production environments.

This policy does not replace:

- Applicable law.
- Data-protection requirements.
- Accounting requirements.
- OEM agreements.
- Lender agreements.
- Payment-provider requirements.
- Government registration requirements.
- Dealership-specific authorization policies.
- Contractual retention or evidence requirements.

Where an external requirement is stricter, the stricter lawful requirement applies.

Dealership-specific configuration may identify different Systems of Record, Freshness SLAs, retention periods, approval limits, and integration methods.

Dealership configuration must not:

- Override tenant isolation.
- Override the Constitution.
- Convert AI inference into authoritative fact.
- Grant unauthorized write authority.
- Remove required Human Approval.
- Bypass an applicable approved automation policy.
- Treat an unconfirmed Command as a completed external action.

---

## 3. Core Definitions

### External System of Record

An External System of Record is the system recognized as the primary operational, financial, contractual, regulatory, or legal authority for a specific field, document, status, or process.

Examples may include:

- CRM for an original Lead submission.
- DMS for a finalized Deal.
- Inventory system for physical stock status.
- Lender platform for a finance decision.
- Payment provider for Payment confirmation.
- Government platform for registration status.
- Communication provider for original delivery evidence.
- Document platform for signed-contract evidence.

The System of Record may differ by:

- Tenant.
- Dealership.
- Branch.
- Market.
- Field.
- Workflow.
- Integration.
- Effective date.

ASOS must not assume that one vendor is authoritative for every deployment or every field.

### ASOS Canonical Projection

An ASOS Canonical Projection is the normalized ASOS representation of information received from one or more approved sources.

It provides:

- Consistent field names.
- Consistent enumerations.
- Normalized timestamps.
- Unified identifiers.
- Canonical relationships.
- Source provenance.
- Data-quality status.
- Conflict status.
- AI-ready context.
- Cross-system analysis.

A Canonical Projection does not automatically replace the external System of Record.

### ASOS Authoritative Workflow State

ASOS Authoritative Workflow State is state created and governed natively by ASOS.

Examples include:

- Human Review Task.
- Recommendation record and status.
- Internal Task.
- Escalation.
- Approval workflow state.
- Reconciliation state.
- Data-quality issue.
- AI Agent Run.
- Command-processing state.
- Internal priority queue.
- Notification state.
- Exception workflow.

ASOS Authoritative Workflow State must not be represented as an externally completed legal, financial, contractual, or operational transaction.

### Derived Intelligence

Derived Intelligence is information calculated, classified, predicted, extracted, summarized, or generated by ASOS.

Examples include:

- Lead score.
- Vehicle-match score.
- Deal-risk score.
- Conversion probability.
- Demand forecast.
- Inventory-pressure score.
- Suggested price markdown.
- Next-best-action Recommendation.
- Customer-response draft.
- AI summary.
- Sentiment.
- Predicted no-show risk.

Derived Intelligence is not an authoritative external business fact.

### Recommendation Content

Recommendation Content is the proposed action, explanation, evidence summary, confidence, risks, and expected impact generated by ASOS.

Recommendation Content is Derived Intelligence.

### Recommendation Record

A Recommendation Record is the ASOS-native workflow record used to track:

- Recommendation creation.
- Review status.
- Required authority.
- Expiration.
- Approval.
- Rejection.
- Modification.
- Escalation.
- Related Command or outcome.

The Recommendation Record is ASOS Authoritative Workflow State.

### Authoritative Human Decision

An Authoritative Human Decision is a Decision recorded from a Human who has the required:

- Identity.
- Role.
- Permission.
- Scope.
- Threshold authority.
- Delegation where applicable.

The Decision may:

- Approve.
- Reject.
- Modify.
- Defer.
- Escalate.
- Revoke where permitted.
- Override where permitted.

The Human Decision does not automatically prove that an external action was performed.

### Command

A Command requests that a specific action be performed.

A Command represents an intention to act.

It is not proof that the action occurred.

### External Confirmation

External Confirmation is authoritative evidence received from the configured external authority that an action was:

- Accepted.
- Rejected.
- Completed.
- Reversed.
- Cancelled.
- Failed.

Transport success or request receipt is not automatically business completion.

### Authoritative Evidence

Authoritative Evidence is controlled evidence supporting a fact, Decision, or Confirmation.

Examples include:

- Signed contract.
- Verified lender response.
- Payment-provider Confirmation.
- DMS transaction identifier.
- Vehicle handover document.
- Customer consent record.
- Inspection report.
- Verified provider webhook.
- Registration document.
- Supplier invoice.
- Approved Human Decision record.

### Field Authority

Field Authority defines which source, system, service, or role may provide, verify, change, approve, or confirm a specific field.

### Canonical Owner

The Canonical Owner is the ASOS Domain Object or Domain Service responsible for:

- Normalized meaning.
- Field definition.
- Validation.
- Relationships.
- Canonical lifecycle.
- Projection integrity.

Canonical ownership does not automatically mean external legal or operational authority.

### Source Provenance

Source Provenance records where information originated, when it was observed, how it was received, and what evidence supports it.

### Tenant

A Tenant is the primary isolation boundary within ASOS.

A Tenant may represent:

- A dealership.
- A dealer group.
- An approved isolated business entity.

A Tenant may contain dealerships, branches, departments, roles, and Users.

---

## 4. Information Authority Classification

ASOS uses six information-authority categories.

### Category A — External Authoritative Data

This information is controlled by an approved external System of Record.

Examples include:

- Lender finance Decision.
- Payment confirmation.
- Signed Financial Contract.
- Government registration status.
- DMS Deal posting.
- Physical Inventory movement.
- Original communication-delivery result.

ASOS may:

- Read it.
- Normalize it.
- Validate it.
- Cache it.
- Reference it.
- Detect conflicts.
- Request an external change through an approved Command.

ASOS must not silently overwrite it.

### Category B — ASOS Canonical Projection

This information is a normalized representation of external or combined source information.

Examples include:

- Normalized Customer profile.
- Canonical Vehicle identity.
- Canonical Inventory Record.
- Unified Interaction timeline.
- Normalized Lead.
- Normalized Deal summary.

Every externally authoritative field must preserve its provenance and authority classification.

### Category C — ASOS Authoritative Workflow State

This information is created and governed directly by ASOS.

Examples include:

- Recommendation Record.
- Human Review Task.
- Internal workflow status.
- Approval workflow status.
- Command-processing status.
- Escalation.
- Internal Task.
- Data-quality issue.
- Reconciliation case.
- AI Agent Run.

ASOS is the System of Record for this workflow state.

### Category D — Derived Intelligence

This information is calculated or generated by ASOS.

Examples include:

- Scores.
- Predictions.
- Risk levels.
- Forecasts.
- Recommendations.
- Summaries.
- Suggested actions.
- Suggested timing.
- Suggested price markdown.

Derived Intelligence must preserve:

- Output type.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input record versions.
- Evidence references.
- Input freshness.
- Confidence where meaningful.
- Assumptions.
- Generation timestamp.
- Expiration or Freshness SLA.
- Required Decision or approval.
- Applicable Action Class.

### Category E — Authoritative Human Decision

This information is created when an authorized Human approves, rejects, modifies, defers, escalates, revokes, or overrides a proposed action.

ASOS may be the System of Record for the Human Decision.

The Decision must preserve:

- Decision maker.
- Role.
- Permission scope.
- Delegation where applicable.
- Decision.
- Reason where required.
- Supporting evidence.
- Related Recommendation.
- Related Domain Object version.
- Timestamp.
- Expiration where applicable.
- Approval threshold.
- Tenant and organizational scope.

### Category F — External Confirmation and Authoritative Outcome

This information is received from an external authority and confirms the status or outcome of an external action.

Examples include:

- Vehicle reservation confirmed.
- Appointment accepted by an external provider.
- CRM update confirmed.
- DMS Deal posted.
- Finance Decision received.
- Payment confirmed.
- Contract signed.
- Vehicle delivery confirmed.

External Confirmation must preserve:

- Confirming system.
- External record identifier.
- External operation identifier.
- Confirmation status.
- Authoritative timestamp.
- Receipt timestamp.
- Evidence reference.
- Related Command.
- Correlation identifier.
- Reconciliation status.

A Human Decision or successfully sent Command must not be treated as Category F without authoritative external evidence.

---

## 5. Canonical Domain Ownership Matrix

The following matrix defines the default ownership model.

A dealership deployment may configure a different external System of Record.

It must not change the Canonical Domain Owner without an approved architectural Decision.

| Domain Object | ASOS Canonical Responsibility | Default External Authority | ASOS-Native Authority |
| :--- | :--- | :--- | :--- |
| Customer | Canonical Customer identity and relationship context | CRM, DMS, identity, consent, or communication-preference systems | Customer intelligence, normalized profile, segmentation, data-quality state |
| Vehicle | Canonical Vehicle identity and specifications | DMS, OEM, registration, inspection, or verified Vehicle-data provider | Identity matching, normalized specifications, verification workflow |
| Inventory Record | Canonical dealership stock context | DMS or Inventory Management System | Inventory analytics, pressure scores, recommendations, reconciliation state |
| Lead | Canonical initial sales-interest record | CRM, website, OEM, or Lead provider | Normalization, enrichment, scoring, duplicate detection |
| Qualified Lead | Canonical qualification assessment | May not exist externally | Qualification evidence, review state, score, internal lifecycle |
| Opportunity | Canonical commercial pursuit | CRM or DMS where supported | Risk, priority, health, forecast, next-best action |
| Appointment | Canonical Appointment representation | CRM, booking provider, or calendar | Scheduling orchestration, readiness, risk, internal follow-up |
| Quotation | Canonical Quotation representation | DMS, ERP, or approved quoting platform | Draft workflow, completeness, boundary validation, approval routing |
| Trade-In | Canonical Trade-In workflow representation | DMS, appraisal, ownership, lien, or valuation systems | Appraisal workflow, comparison, risk, Recommendation |
| Finance Application | Canonical application and workflow projection | F&I or lender platform | Completeness, routing, checklist, internal progress |
| Financial Contract | Canonical contract projection | Lender, DMS, contract, or document platform | Compliance checklist, expiry monitoring, missing-document detection |
| Deal | Canonical Deal projection and internal orchestration | DMS, ERP, accounting, or transaction platform | Deal health, risk, exception workflow, approval history |
| Interaction | Canonical unified Interaction Record | Communication provider for original transport evidence | Classification, summary, intent, sentiment, relationship mapping |
| Market Intelligence | Canonical market observation and evidence model | Market providers and evidence sources | Analysis, confidence, risks, opportunities, Recommendations |

### Customer

Customer owns the normalized Customer identity and relationship context.

Externally authoritative fields may include:

- Legal identity.
- Original Customer record.
- Contact details.
- Address.
- Consent.
- Communication preferences.
- Tax information.

ASOS-native or derived information may include:

- Customer summary.
- Engagement score.
- Propensity score.
- Journey stage.
- Internal Recommendations.
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
- Identity-verification evidence.
- Source provenance.

Vehicle does not own:

- Dealership stock number.
- Current location.
- Availability.
- Reservation.
- Allocation.
- Customer-visible pricing.
- Sale status.
- Delivery status.

Those responsibilities belong to Inventory Record, Quotation, Deal, or the relevant workflow.

### Inventory Record

Inventory Record owns the canonical representation of:

- Dealership stock context.
- Location.
- Availability projection.
- Preparation status.
- Reservation projection.
- Allocation projection.
- Pricing context.
- Inventory aging.
- Stock exit.

The external DMS or Inventory Management System may remain authoritative for physical stock status.

### Lead

Lead owns the normalized initial sales-interest record.

The original source may remain authoritative for:

- Receipt time.
- Source campaign.
- Original form data.
- Provider reference.
- Original source consent evidence.

ASOS may own or derive:

- Enrichment.
- Classification.
- Priority.
- Follow-up Recommendation.
- Duplicate-detection status.

### Qualified Lead

Qualified Lead is normally ASOS Authoritative Workflow State.

It must preserve:

- Qualification criteria.
- Evidence.
- Score.
- Human Review state.
- Qualification timestamp.
- Source Lead version.

It must not silently overwrite an external CRM status without an approved Command and Confirmation workflow.

### Opportunity

Opportunity owns the canonical representation of a commercially meaningful sales pursuit.

An external CRM may remain authoritative for operational stage.

ASOS may own or derive:

- Opportunity health.
- Risk.
- Forecast.
- Next-best action.
- Internal management priority.
- Escalation state.

### Appointment

The booking provider, CRM, or calendar may remain authoritative for externally confirmed Appointment time and status.

ASOS may own:

- Appointment Recommendation.
- Scheduling orchestration.
- Readiness checklist.
- No-show risk.
- Follow-up workflow.
- Pending-Confirmation state.

### Quotation

Final Customer-visible pricing must originate from an approved pricing authority.

ASOS may:

- Recommend a Quotation.
- Check completeness.
- Validate price boundaries.
- Route approval.
- Prepare an approved draft.
- Track external submission and Confirmation.

ASOS must not represent an unapproved draft as a binding Quotation.

### Trade-In

External authorities may own:

- Legal ownership evidence.
- Lien or payoff status.
- Inspection result.
- External valuation.
- Acquisition posting.

ASOS may own:

- Trade-In workflow.
- Valuation comparison.
- Recommendation.
- Risk assessment.
- Appraisal completeness.
- Review and approval state.

### Finance Application

The F&I or lender platform owns:

- Finance Decision.
- Approved amount.
- Interest rate.
- Conditions.
- Rejection reason where lawfully available.
- Funding status.

ASOS may own:

- Completeness status.
- Routing.
- Document checklist.
- Internal progress.
- Customer communication Recommendation.
- Pending external Confirmation.

### Financial Contract

The signed or externally confirmed contract is authoritative.

ASOS may maintain:

- Canonical contract projection.
- Compliance checklist.
- Expiry monitoring.
- Missing-document detection.
- Signature-status projection.

ASOS must not represent an unsigned or unconfirmed draft as an active Financial Contract.

### Deal

The DMS, ERP, accounting system, or approved transaction platform normally owns the finalized Deal.

ASOS may own:

- Internal Deal health.
- Exception workflow.
- Approval history.
- Risk assessment.
- Forecast contribution.
- Recommended action.
- Command and reconciliation state.

### Interaction

The communication provider owns original transport and delivery evidence.

ASOS may own:

- Unified Interaction Record.
- Classification.
- Sentiment.
- Summary.
- Intent.
- Follow-up Recommendation.
- Relationship to Lead, Opportunity, Appointment, Quotation, or Deal.

Original content and transport evidence must remain traceable to the source.

### Market Intelligence

Evidence providers own the raw source information.

ASOS owns or derives:

- Normalized observation.
- Evidence references.
- Analysis.
- Confidence.
- Commercial risk.
- Commercial opportunity.
- Recommendation.

Market Intelligence must not present unsupported inference as verified market fact.

---

## 6. Field-Level Authority Requirements

Authority must be defined at the field or approved field-group level.

ASOS must not assume that one complete Domain Object has a single System of Record.

A Customer record, for example, may contain:

- Name from CRM.
- Legal identity from an identity provider.
- Consent from a consent system.
- Communication preferences from a messaging provider.
- Finance information from a lender.
- Lead score from ASOS.

Every governed field must support the following metadata where applicable:

| Metadata Field | Purpose |
| :--- | :--- |
| `tenant_id` | Mandatory Tenant isolation identifier |
| `dealer_group_id` | Optional dealer-group context |
| `dealership_id` | Optional dealership context inside the Tenant |
| `branch_id` | Optional branch context |
| `system_of_record` | External or internal authority |
| `canonical_owner` | ASOS Domain Object responsible for normalized meaning |
| `source_system` | Actual source of the current value |
| `source_record_id` | Identifier in the source system |
| `source_field_name` | Original source field |
| `source_authority` | Authority classification |
| `authority_category` | External, projection, workflow, derived, Human Decision, or external Confirmation |
| `read_authority` | Roles or services allowed to read the value |
| `write_authority` | Roles or services allowed to propose or change the value |
| `approval_authority` | Role or policy required to approve the change |
| `sync_direction` | Inbound, outbound, bidirectional, or internal-only |
| `conflict_policy` | Approved resolution policy |
| `freshness_sla` | Maximum acceptable age |
| `verified_at` | Time of authoritative verification |
| `effective_from` | Start of validity |
| `effective_until` | End of validity |
| `sensitivity_classification` | Security and privacy classification |
| `retention_policy` | Retention requirement |
| `evidence_reference` | Supporting evidence |
| `record_version` | Version for concurrency and history |
| `last_synced_at` | Last successful synchronization |
| `last_sync_status` | Synchronization status |
| `conflict_status` | Conflict status |
| `data_quality_status` | Quality and completeness status |

### Authority Registry

Every Production deployment must maintain an approved Field Authority Registry defining:

- Canonical Object.
- Field name or governed field group.
- Default external authority.
- Permitted sources.
- Source precedence.
- Read authority.
- Write authority.
- Approval authority.
- Applicable automation policy.
- Synchronization direction.
- Conflict policy.
- Verification requirement.
- Freshness SLA.
- Sensitivity classification.
- Retention requirement.
- Evidence requirement.
- External Confirmation requirement.

### Access Does Not Equal Authority

The ability to:

- Read a field does not authorize changing it.
- Recommend a change does not authorize approving it.
- Approve a change does not prove external execution.
- Send a Command does not prove business completion.
- Receive a transport acknowledgment does not prove authoritative Confirmation.

---

## 7. Synchronization, Freshness, Idempotency, and Reconciliation

### Synchronization Directions

#### Inbound

External source to ASOS.

Used when ASOS observes, validates, normalizes, and projects external information.

#### Outbound

ASOS to an external system.

Used only where an approved connector and write authority exist.

#### Bidirectional

ASOS and an external system may submit permitted changes.

Bidirectional synchronization requires:

- Field-level write authority.
- Conflict policies.
- Version controls.
- Idempotency.
- Reconciliation.
- Audit evidence.

#### Internal Only

Information remains authoritative inside ASOS.

Examples include:

- Recommendation status.
- AI Agent Run.
- Human Review Task.
- Internal escalation.
- Command-processing state.

### Synchronization Mechanisms

Approved mechanisms may include:

- Signed webhooks.
- Change Data Capture.
- REST or GraphQL APIs.
- Event streaming.
- Approved polling.
- Scheduled batch import.
- Secure file exchange.
- Manual governed import.
- Outbox pattern.
- Approved outbound Command.
- Reconciliation job.

### Synchronization Evidence

Every material synchronization operation must preserve:

- `tenant_id`.
- Dealership and branch context where applicable.
- Source system.
- Source record.
- Source Event or batch identifier.
- Source timestamp.
- Retrieval timestamp.
- Payload or document hash where required.
- Processing result.
- Source deduplication identifier where applicable.
- Correlation identifier.
- Record version.
- Retry count.
- Error reason.
- Reconciliation state.

### Source-Change Deduplication

Repeated processing of the same external source change must not create duplicate:

- Customers.
- Leads.
- Vehicles.
- Inventory Records.
- Appointments.
- Quotations.
- Reservations.
- Payments.
- Deals.
- Interactions.
- Commands.

Source deduplication may use:

- Verified external Event identifier.
- Provider transaction identifier.
- Source record version.
- Payload hash.
- Approved source deduplication key.

### Event Idempotency

Published Events are identified by `event_id`.

An Event Consumer must prevent duplicate business effects when the same `event_id` is delivered more than once.

### Command Idempotency

Commands that may be retried must use an approved `idempotency_key`.

The same Command intent must not create duplicate external actions.

### Freshness

Every governed integration and field group must define a Freshness SLA.

Suggested examples:

| Data Type | Suggested Freshness Requirement |
| :--- | :--- |
| Inventory availability | Near real time |
| Reservation and allocation | Real time or transactionally synchronized |
| Lead receipt | Near real time |
| Customer contact change | Minutes |
| Appointment Confirmation | Near real time |
| Finance Decision | Near real time |
| Payment Confirmation | Real time |
| Signed contract | Near real time |
| Market Intelligence | Provider-dependent |
| Historical reporting | Scheduled |

The exact SLA must be configured per deployment.

### Stale Data Protection

When authoritative information exceeds its Freshness SLA:

- The field or record must be marked stale.
- Dependent high-risk actions may be blocked.
- AI outputs must disclose the stale-data condition.
- Synchronization or Human Review must be initiated.
- Customer-facing availability, pricing, finance, Payment, reservation, and delivery claims must not rely on stale information.

### Reconciliation

Every material integration must support reconciliation between external authority and ASOS.

Reconciliation should detect:

- Missing records.
- Duplicate records.
- Count mismatches.
- Version mismatches.
- Status mismatches.
- Missing Confirmations.
- Failed Commands.
- Stale projections.
- Unresolved conflicts.
- Orphaned workflow state.

Reconciliation outcomes must be recorded and auditable.

---

## 8. Conflict Resolution, Correction, and Supersession

### General Principle

Material conflicts must not be silently resolved through uncontrolled overwrite.

Every material conflict must preserve:

- Competing values.
- Competing sources.
- Authority category.
- Source timestamps.
- Evidence.
- Current operational use.
- Applicable policy.
- Workflow restrictions.
- Resolution.
- Authorized Decision maker where required.
- Resolution timestamp.

### Default Authority Considerations

Field-specific policy remains authoritative.

Where no field-specific policy exists, the following may guide escalation:

1. Applicable legal or regulatory evidence.
2. Signed contractual evidence.
3. Verified lender or financial-provider response.
4. Verified government or OEM source.
5. Verified DMS or operational System of Record.
6. Authorized Human Decision supported by evidence.
7. Verified Customer Confirmation.
8. Approved external operational source.
9. ASOS Canonical Projection.
10. AI-extracted information.
11. AI inference.
12. Unverified manual entry.

This list must not be applied blindly.

### Permitted Conflict Strategies

Approved strategies may include:

- Highest-authority source.
- Most recent verified value.
- Legal evidence wins.
- Signed evidence wins.
- External System of Record wins.
- Verified Customer Confirmation.
- Human Review required.
- Reject update.
- Merge non-conflicting components.
- Preserve both values with effective dates.
- Controlled supersession.
- Quarantine until resolved.

### Material Conflicts

Authorized Human Review is normally required for conflicts involving:

- Customer legal identity.
- Consent.
- VIN or chassis identity.
- Vehicle ownership.
- Inventory availability.
- Reservation.
- Allocation.
- Customer-visible pricing.
- Deal status.
- Finance Decision.
- Payment.
- Signed contract.
- Vehicle delivery.
- Fraud.
- Compliance.
- Legal hold.

A deterministic policy may block the workflow before Human Review is completed.

### Non-Material Conflicts

Low-risk formatting differences may be normalized automatically.

Examples include:

- Telephone formatting.
- Letter case.
- Whitespace.
- Approved date formatting.
- Known manufacturer aliases.

The original source value must be preserved where required.

### Conflict State

A record with unresolved material conflict should include:

- `conflict_status`.
- `conflicting_fields`.
- `conflicting_sources`.
- `conflict_detected_at`.
- `workflow_restrictions`.
- `review_task_id`.
- `resolution_policy`.
- `resolved_at`.
- `resolved_by`.

### Corrections and Historical Integrity

Historical authoritative records and published Events must not be silently rewritten.

Corrections may use:

- New corrective Event.
- Versioning.
- Supersession.
- Amendment.
- Controlled reversal.
- Compensating transaction.
- Controlled redaction where legally required.

A correction must reference the original record or Event where applicable.

---

## 9. Write Authority, Automation, and Command Governance

### Read Does Not Imply Write

The ability to read an external field does not authorize ASOS to change it.

Every outbound action must have:

- Approved business purpose.
- Authorized User or service.
- `tenant_id`.
- Dealership and branch scope where applicable.
- Current record version.
- Validation.
- Field-level write permission.
- External provider permission.
- Applicable Action Class.
- Required Human Approval or applicable approved automation policy.
- Command `idempotency_key`.
- Audit evidence.

### Human Approval or Approved Automation Policy

Action Class 2 operations may proceed through either:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

A pre-approved automation policy must define:

- Permitted use case.
- Eligible Customer or workflow conditions.
- Approved templates.
- Approved channels.
- Permitted data fields.
- Frequency limits.
- Time restrictions where applicable.
- Consent requirements.
- Value or risk limits.
- Monitoring.
- Revocation.
- Escalation.
- Audit requirements.
- Emergency suspension.

The AI Agent must not grant itself permission to use an automation policy.

The deterministic Policy and Authorization layer must validate the policy before Command creation or execution.

### Binding and High-Impact Actions

Action Class 3 operations require an Authoritative Human Decision.

Examples include:

- Final pricing approval.
- Restricted discount approval.
- Credit or finance Decision.
- Trade-In acquisition approval.
- Contractual commitment.
- Payment or refund authorization.
- Deal finalization.
- Vehicle release.
- Vehicle delivery.
- Legal or compliance override.

External Confirmation may also be required before the resulting business state is considered complete.

### Command Responsibilities

Command governance separates the following responsibilities:

| Responsibility | Owner |
| :--- | :--- |
| Business intention | Applicable workflow or Domain Service |
| Policy and authority validation | Policy, Authorization, and Workflow Control |
| Human Decision | Authorized Human role |
| Automation-policy Decision | Deterministic Policy Engine |
| Command creation | Applicable workflow service |
| Command orchestration | Command Orchestration service |
| External transmission | Approved connector |
| External completion | Configured external authority |
| Canonical reconciliation | Applicable Domain Service |
| Audit evidence | Audit and Observability services |

### Command Lifecycle

An outbound Command may use the following lifecycle:

```text
Created
Validated
Awaiting Approval
Approved
Queued
Sent
Pending Confirmation
Confirmed
Rejected
Failed
Expired
Reconciliation Required
Cancelled
```

Not every Command uses every state.

### Pending Confirmation

A successfully transmitted Command must remain pending until the required authoritative External Confirmation is received.

ASOS must distinguish between:

- Transport succeeded.
- Request was received.
- Request was accepted for processing.
- Business action was completed.
- Business outcome was authoritatively confirmed.

### Missing Confirmation

When Confirmation is not received within the configured period, ASOS must not mark the action complete.

It must initiate one or more of:

- Timeout handling.
- Status polling.
- Safe retry.
- Reconciliation.
- Human escalation.
- Incident creation.

### Optimistic User Interface State

Optimistic interface updates may be used only when they are:

- Clearly marked as pending.
- Reversible.
- Not legally or financially binding.
- Prevented from triggering irreversible dependent actions.
- Reconciled with the authoritative source.

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
- Represent a pending Command as confirmed.
- Bypass deterministic policy controls.

---

## 10. Event Ownership, Provenance, and Truthfulness

This policy defines Event ownership and provenance rules.

The Canonical Event Catalog is the authoritative source for:

- Canonical Event names.
- Event versions.
- Event Schemas.
- Payload definitions.
- Producer and Consumer contracts.
- Compatibility rules.

Event names in this policy are illustrative only until defined in the Event Catalog.

### Event Categories

#### Source-Observation Events

Report that ASOS observed a change from an external source.

The integration adapter produces the normalized observation.

The Event must identify the original source.

#### Canonical Domain Events

Report an accepted change to an ASOS Canonical Projection or canonical Domain state.

The applicable Domain Service produces the Event.

#### ASOS Workflow Events

Report a change in ASOS Authoritative Workflow State.

Examples may include:

- Human Review requested.
- Recommendation created.
- Recommendation approved.
- Escalation created.
- Internal Task completed.

#### Derived Intelligence Events

Report the generation or update of Derived Intelligence.

These Events must not be interpreted as authoritative external transaction Confirmation.

#### Authoritative Human Decision Events

Report an authorized Human Decision.

The Event must preserve authority evidence.

#### Command Lifecycle Events

Report a material Command lifecycle change.

Examples may include:

- Command created.
- Command sent.
- Command failed.
- Command awaiting reconciliation.

#### External Confirmation Events

Report authoritative evidence received from an external source.

These Events must clearly identify:

- Confirming authority.
- Related Command.
- External operation.
- Confirmation status.
- Authoritative timestamp.

### Event Producer Rule

The service that authoritatively accepts the represented state change is the Event producer.

An AI Agent that recommends a change is not automatically the producer of the resulting authoritative business Event.

Example:

```text
AI Agent recommends Vehicle reservation
        ↓
Policy and authority checks pass
        ↓
Authorized Human or automation policy approves
        ↓
Reservation Command is created
        ↓
External Inventory authority confirms reservation
        ↓
Inventory Domain Service reconciles state
        ↓
Authoritative reservation-confirmed Event is published
```

### Event Immutability

Published Events are immutable.

An Event must not be edited or deleted merely to change historical meaning.

Correction, cancellation, reversal, or revocation must be represented by a new Event linked to the original Event.

### Event Delivery

The Event Backbone may deliver the same Event more than once.

Consumers must use `event_id` and idempotent processing to prevent duplicate business effects.

### Event Provenance Requirements

Every material Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- Aggregate or Domain Object identifier.
- Aggregate type.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Source system.
- Source record identifier.
- Source Event identifier.
- Correlation identifier.
- Causation identifier.
- Actor type.
- Actor identifier.
- Authority category.
- Evidence references.
- Security classification.
- Payload.

The Event envelope and payload Schema will be defined by the Canonical Event Catalog.

### Event Truthfulness

Event names must describe what actually occurred.

Permitted distinctions include:

```text
FinanceOfferRecommended
FinanceApplicationSubmitted
FinanceDecisionReceived
FinanceApprovalConfirmed
```

A Recommendation must not produce an Event name implying an external approval.

A sent Command must not produce an Event name implying completion.

A Human Decision must not produce an Event name implying external Confirmation unless that Confirmation was actually received.

---

## 11. AI, Security, Privacy, and Audit Governance

### AI Context Requirements

AI Agents must receive authority-aware context.

Context should distinguish:

- External authoritative fact.
- Verified fact.
- ASOS Canonical Projection.
- ASOS Workflow State.
- Authoritative Human Decision.
- External Confirmation.
- Derived Intelligence.
- AI inference.
- Unverified source.
- Stale information.
- Conflicting information.

### AI Output Labelling

Material AI output must specify, where applicable:

- Output type.
- Supporting evidence.
- Source authority.
- Input versions.
- Input freshness.
- Model or algorithm version.
- Prompt version.
- Confidence.
- Assumptions.
- Applicable Action Class.
- Required Human Approval or applicable automation-policy requirement.
- Expiration time.

### AI Conflict Behaviour

When material sources conflict, AI must:

- Identify the conflict.
- Avoid selecting an authoritative value without permission.
- Explain operational risk.
- Reduce confidence.
- Recommend or initiate Human Review where required.
- Restrict high-risk downstream action.

### Tenant Isolation

All ownership, synchronization, Event, Command, AI-context, conflict, and audit operations must enforce `tenant_id`.

`dealership_id` and `branch_id` may further restrict access inside the Tenant.

Cross-tenant access is prohibited unless governed through an approved mechanism such as:

- Explicit data-sharing agreement.
- Authorized stock-transfer workflow.
- Approved group-level analytics.
- Lawful aggregation or anonymization.
- Auditable privileged support access.

### Least Privilege

Users and services must receive only the access required for:

- Necessary fields.
- Necessary Domain Objects.
- Necessary dealerships and branches.
- Necessary workflows.
- Necessary actions.
- Necessary time period.
- Approved business purpose.

### Sensitive Data

Sensitive information must use appropriate controls, including:

- Encryption in transit.
- Encryption at rest.
- Field-level access controls.
- Masking or redaction.
- Audit logging.
- Retention limits.
- Purpose limitation.
- Restricted AI context.
- Secret-management services.

### Audit Requirements

Every authoritative or material operation must record, where applicable:

- `tenant_id`.
- Dealership and branch context.
- Domain Object.
- Record identifier.
- Field or state changed.
- Previous value or hash.
- New value or hash.
- Source.
- Source authority.
- Actor.
- Role and permission.
- Business purpose.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Record version.
- AI involvement.
- Human Decision.
- Automation-policy reference.
- Command identifier.
- Command idempotency key.
- External Confirmation status.
- Correction or reversal reference.

### Immutable and Controlled History

The following must not be silently overwritten:

- Customer consent.
- Verified Vehicle identity.
- Inventory reservation history.
- Inventory allocation history.
- Approved Quotation.
- Trade-In approval.
- Finance Decision.
- Signed Financial Contract.
- Payment Confirmation.
- Deal finalization.
- Vehicle delivery evidence.
- Authoritative Human Decision.
- Human override.
- AI Recommendation used in a material Decision.
- Command lifecycle.
- External Confirmation.

Lawful redaction, deletion, or anonymization requirements must preserve appropriate audit evidence without retaining prohibited content.

---

## 12. Implementation Requirements and Acceptance Criteria

### Required Architecture Components

ASOS implementation must provide:

- Source Registry.
- Field Authority Registry.
- Integration Configuration Registry.
- Canonical Mapping Registry.
- Policy and Authorization controls.
- Conflict-Resolution workflow.
- Synchronization services.
- Source-deduplication controls.
- Event Consumer idempotency controls.
- Command idempotency store.
- Event provenance store.
- Human Review and approval workflow.
- Automation Policy Registry.
- Command Orchestration service.
- External Confirmation tracking.
- Audit Log.
- Data Freshness Monitor.
- Reconciliation services.
- Evidence repository.
- Tenant-aware access controls.
- Data-quality monitoring.

### Source Registry

The Source Registry must define:

- Source-system identifier.
- Source type.
- `tenant_id`.
- Dealer group, dealership, and branch scope where applicable.
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
- Confirmation behavior.
- Security classification.

### Field Authority Registry

The Field Authority Registry must define for every governed field or field group:

- Canonical Object.
- Field name.
- Default System of Record.
- Permitted sources.
- Source precedence.
- Read authority.
- Write authority.
- Approval authority.
- Applicable automation policy.
- Conflict policy.
- Verification requirement.
- Freshness SLA.
- Sensitivity.
- Retention.
- Evidence requirement.
- External Confirmation requirement.

### Automation Policy Registry

Every approved automation policy must define:

- Policy identifier.
- Version.
- Tenant and organizational scope.
- Action Class.
- Permitted use case.
- Conditions.
- Data restrictions.
- Channel and template restrictions.
- Frequency limits.
- Consent rules.
- Risk limits.
- Monitoring.
- Revocation.
- Expiration.
- Approval evidence.
- Emergency suspension.

### Domain Model Requirements

Every Canonical Domain Model must identify:

- Externally authoritative fields.
- ASOS Canonical Projection fields.
- ASOS-native workflow fields.
- Derived Intelligence fields.
- Human Decision fields.
- External Confirmation fields.
- Required provenance.
- Fields requiring Human Approval.
- Fields permitted under approved automation policy.
- Fields permitted for write-back.
- Immutable or historically versioned fields.
- Conflict behaviour.
- Freshness requirements.

Canonical Domain Models may reference Event categories but must not duplicate the full Event Catalog.

### API Requirements

Every mutating API must enforce:

- Authentication.
- `tenant_id`.
- Organizational scope.
- Authorization.
- Record version.
- Idempotency where required.
- Business validation.
- Field-authority validation.
- Conflict checks.
- Human Approval or applicable automation-policy validation.
- Audit evidence.
- External Confirmation tracking where applicable.

### Event Requirements

The Canonical Event Catalog must define:

- Event name.
- Event version.
- Producer.
- Authority category.
- Aggregate.
- Payload Schema.
- Provenance.
- Actor requirements.
- Correlation and causation.
- Correction or reversal behaviour.
- Consumer idempotency expectations.
- Security classification.
- Retention.
- Compatibility policy.

Event Consumers must process duplicate delivery safely using `event_id`.

### Command Requirements

Every retryable Command must define:

- Command identifier.
- Command type.
- `tenant_id`.
- Domain Object and record identifier.
- Requested action.
- Requesting workflow.
- Authority evidence.
- Human Decision or automation-policy reference.
- `idempotency_key`.
- Validation status.
- Connector.
- External operation identifier.
- Confirmation requirement.
- Timeout.
- Retry policy.
- Reconciliation behaviour.
- Final outcome.

### Acceptance Criteria

This policy is considered implemented when:

- Every Production source is registered.
- Every critical field has defined authority.
- Every material field preserves provenance.
- `tenant_id` is enforced throughout the data lifecycle.
- External and ASOS-native states are distinguishable.
- Derived Intelligence is not presented as authoritative fact.
- Recommendation Content and Recommendation workflow status are distinguishable.
- Authoritative Human Decisions are distinguishable from external outcomes.
- No material field is silently overwritten.
- Stale information is detected and labelled.
- Material conflicts create a controlled resolution path.
- Every outbound action has Human Approval or an applicable approved automation policy.
- Every retryable Command uses idempotency.
- A sent Command remains pending until required authoritative Confirmation.
- Missing Confirmations trigger timeout or reconciliation.
- Duplicate Event delivery does not produce duplicate business effects.
- Event history is immutable.
- Corrections use new Events or controlled supersession.
- Every material Event identifies its producer and authority category.
- The Event Catalog remains the authoritative Event contract.
- Every authoritative or material change is auditable.

---

## Governing Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS System Architecture](./System_Architecture.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/)
- [ASOS MVP Pilot Framework](./MVP_Pilot_Framework.md)
- [ASOS Repository Structure](../README.md)

---

## Current Status

This document is the approved Data Ownership and Systems-of-Record baseline for ASOS.

Dealership-specific Systems of Record, authority mappings, Freshness SLAs, retention requirements, integration methods, and approval limits must be configured separately without weakening the requirements of this policy.
