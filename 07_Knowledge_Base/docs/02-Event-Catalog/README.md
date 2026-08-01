# ASOS Canonical Event Catalog Governance

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Event and Architecture Governance  
**Primary Isolation Boundary:** `tenant_id`  
**Applies To:** All ASOS Domain Events, Workflow Events, integration observations, Event Producers, Event Consumers, Event Backbones, replays, corrections, and Event-driven workflows  
**Last Updated:** 2026-08-01  

---

## 1. Purpose and Scope

### Purpose

The ASOS Canonical Event Catalog defines the authoritative governance rules for Events published, stored, delivered, consumed, replayed, corrected, reversed, secured, and evolved across the ASOS AI Sales Operating System.

The Event Catalog establishes the shared Event language used by:

- Canonical Domain Services.
- Workflow Services.
- Policy and Authorization Services.
- Human Review and Approval Services.
- Command Orchestration.
- Integration Edge and Connectors.
- AI Intelligence and Agent Services.
- Audit and Observability Services.
- Analytics and reporting services.
- Dealership integrations.
- External-system synchronization workflows.

### Event Catalog Authority

The Canonical Event Catalog is authoritative for:

- Canonical Event names.
- Event meanings.
- Event taxonomy.
- Event versions.
- Event envelopes.
- Event payload contracts.
- Producer responsibilities.
- Consumer responsibilities.
- Authority classification.
- Required evidence references.
- Correlation and causation requirements.
- Ordering guarantees.
- Delivery semantics.
- Consumer deduplication requirements.
- Replay behaviour.
- Correction behaviour.
- Reversal behaviour.
- Cancellation and revocation semantics.
- Security classification.
- Compatibility requirements.
- Deprecation and retirement.
- Event governance and approval.

The Event Catalog does not replace:

- The Canonical Domain Model.
- API Contracts.
- Data Schemas.
- Agent Contracts.
- Integration Contracts.
- Business Rules.
- Playbooks.
- Production Prompts.
- Deployment-specific messaging configuration.

### Relationship to Other Canonical Catalogs

```text
Canonical Domain Model
  = defines the meaning, ownership, lifecycle, relationships,
    validation, authority, and security of Domain Objects

Canonical Event Catalog
  = defines immutable facts published when material occurrences happen

API Contracts
  = define synchronous operations, requests, responses, errors,
    authorization, concurrency, and idempotency

Data Schemas
  = define machine-readable validation and serialization structures

Agent Contracts
  = define AI Agent purpose, inputs, outputs, tools, limits, and escalation

Integration Contracts
  = define external mappings, Commands, Confirmations, retries,
    reconciliation, and vendor-specific behaviour
```

The applicable detailed catalog becomes authoritative for its own responsibility.

### Event Definition

An Event is an immutable statement that a material occurrence happened.

An Event:

- Describes a past fact.
- Has a unique identity.
- Has an occurrence time.
- Has a Producer.
- Has an authority classification.
- Has an approved Event version.
- Has a governed payload.
- May reference evidence.
- May cause authorized Consumers to evaluate or continue workflows.

An Event does not request future action.

### Event and Command Separation

```text
Event
  = something happened

Command
  = a request for something to happen
```

Examples:

```text
AppointmentConfirmed
  = Event

ConfirmAppointment
  = Command
```

```text
QuotationIssued
  = Event

IssueQuotation
  = Command
```

```text
CommandSent
  = Event describing Command lifecycle

SendApprovedFollowUp
  = Command requesting communication
```

### Event and Recommendation Separation

```text
RecommendationGenerated
  = Event confirming that a Recommendation record was created

Recommendation content
  = non-binding proposed course of action

Human Decision
  = authoritative approval, rejection, modification, deferral,
    or escalation by an authorized Human role
```

A Recommendation Event must not imply approval.

### Event and External Confirmation Separation

A Command lifecycle Event does not prove that an external business outcome completed.

```text
CommandCreated
  ≠ External operation completed

CommandSent
  ≠ External operation completed

ProviderAcknowledgedCommand
  ≠ Business outcome completed

ExternalConfirmationReceived
  = authoritative confirmation evidence was received

CanonicalOutcomeConfirmed
  = responsible Domain Service accepted and reconciled the outcome
```

### Scope

This governance applies to:

- Domain Events.
- ASOS authoritative workflow Events.
- Recommendation lifecycle Events.
- Human Decision Events.
- Command lifecycle Events.
- External observation Events.
- External Confirmation Events.
- Data-quality Events.
- Reconciliation Events.
- Correction and reversal Events.
- Security-relevant business Events.
- Controlled analytical Events.
- AI Agent execution and Derived Intelligence Events.

This governance does not treat ordinary application Logs, metrics, traces, or debugging records as Domain Events.

---

## 2. Event Semantics and Taxonomy

### Event Semantic Principle

Every Event must represent exactly one clear material occurrence.

An Event name and payload must answer:

- What happened?
- To which Domain Object or workflow?
- When did it happen?
- Which authority established it?
- Which evidence supports it?
- What version of the affected record existed?
- What caused the occurrence?
- What may Consumers safely conclude?

### Required Semantic Separations

The Event Catalog must preserve separation among:

```text
External observation
Canonical Projection update
ASOS authoritative workflow update
Derived Intelligence
Recommendation
Human Decision
Command lifecycle
External Confirmation
Canonical reconciled outcome
```

These concepts must not share an Event name when their authority or meaning differs.

### Event Categories

#### 2.1 Domain Event

A Domain Event records an accepted material change concerning a Canonical Domain Object.

Examples:

- `CustomerCreated`
- `LeadCreated`
- `LeadQualified`
- `OpportunityStageChanged`
- `AppointmentScheduled`
- `QuotationIssued`
- `TradeInAppraised`
- `FinanceApplicationSubmitted`
- `FinancialContractFullySigned`
- `DealCompleted`
- `InteractionReceived`

A Domain Event must be published by the responsible Canonical Domain Service or another explicitly approved authority.

#### 2.2 Workflow Event

A Workflow Event records ASOS Authoritative Workflow State.

Examples:

- `HumanReviewRequested`
- `ApprovalRequested`
- `ApprovalExpired`
- `EscalationCreated`
- `ReconciliationRequired`
- `DataQualityIssueOpened`

A Workflow Event does not automatically represent an externally authoritative business outcome.

#### 2.3 External Observation Event

An External Observation Event records normalized evidence received from an approved external source.

Examples:

- `ExternalInventoryAvailabilityObserved`
- `LenderDecisionObserved`
- `PaymentStatusObserved`
- `CommunicationDeliveryObserved`
- `ExternalAppointmentStatusObserved`

An external observation remains distinct from the Canonical Domain Service accepting and reconciling that observation.

#### 2.4 External Confirmation Event

An External Confirmation Event records authoritative evidence that an external system accepted, rejected, completed, reversed, or otherwise resolved an operation.

Examples:

- `ExternalAppointmentConfirmed`
- `ExternalReservationConfirmed`
- `ExternalPaymentCleared`
- `ExternalFundingConfirmed`
- `ExternalDealPostingConfirmed`
- `ExternalDeliveryConfirmed`

External Confirmation must identify the configured authority and evidence.

#### 2.5 Recommendation Event

A Recommendation Event records Recommendation lifecycle facts.

Examples:

- `RecommendationGenerated`
- `RecommendationPresented`
- `RecommendationAcceptedForReview`
- `RecommendationExpired`
- `RecommendationWithdrawn`

A Recommendation Event must not imply that an action was authorized or completed.

#### 2.6 Human Decision Event

A Human Decision Event records a Decision made by an authorized Human.

Examples:

- `RecommendationApproved`
- `RecommendationRejected`
- `QuotationExceptionApproved`
- `DealCancellationApproved`
- `ContractAmendmentRejected`

The Event must reference the authoritative Human Decision record.

A Human Decision Event does not prove external execution.

#### 2.7 Command Lifecycle Event

A Command Lifecycle Event records what happened to a governed Command.

Examples:

- `CommandCreated`
- `CommandValidated`
- `CommandApproved`
- `CommandQueued`
- `CommandSent`
- `CommandRejected`
- `CommandFailed`
- `CommandExpired`
- `CommandCancelled`
- `CommandReconciliationRequired`

The Command itself is not an Event.

#### 2.8 Derived Intelligence Event

A Derived Intelligence Event records the generation, expiration, review, or invalidation of analytical output.

Examples:

- `LeadScoreGenerated`
- `DealRiskDetected`
- `VehicleMatchRecommended`
- `InteractionIntentClassified`
- `MarketForecastGenerated`
- `DerivedIntelligenceExpired`

Derived Intelligence Events must identify the model, formula, algorithm, Prompt, evidence, inputs, limitations, and expiration where applicable.

They must not imply an authoritative business outcome.

#### 2.9 Data Quality and Reconciliation Event

These Events record conflicts, stale data, failed synchronization, or reconciliation outcomes.

Examples:

- `DataConflictDetected`
- `DataQualityIssueOpened`
- `ProjectionBecameStale`
- `ReconciliationRequested`
- `ReconciliationCompleted`
- `ReconciliationFailed`

#### 2.10 Correction, Reversal, Cancellation, and Revocation Event

These Events preserve history when a prior fact is corrected, reversed, cancelled, revoked, rescinded, voided, or superseded.

Examples:

- `LeadDataCorrected`
- `AppointmentConfirmationRevoked`
- `PaymentReversed`
- `FundingReversed`
- `FinancialContractVoided`
- `DealUnwound`
- `QuotationSuperseded`

The new Event must reference the affected prior Event where applicable.

#### 2.11 Security and Governance Event

Security and governance Events may record material governed occurrences such as:

- `TenantAccessViolationDetected`
- `UnauthorizedCommandBlocked`
- `EmergencySuspensionActivated`
- `EventReplayAuthorized`
- `EventSchemaDeprecated`

Operational security telemetry may remain in security Logs rather than the Domain Event Backbone.

The applicable Security Architecture determines which security occurrences require canonical Events.

### Facts, Observations, and Interpretations

Event names must reveal whether the occurrence is:

- Observed.
- Calculated.
- Predicted.
- Recommended.
- Approved.
- Requested.
- Sent.
- Confirmed.
- Rejected.
- Failed.
- Reversed.
- Corrected.

The following are not interchangeable:

```text
PaymentReported
PaymentObserved
PaymentAuthorized
PaymentCaptured
PaymentCleared
PaymentReconciled
PaymentReversed
```

### Materiality

An Event should be published when an occurrence:

- Changes meaningful Domain or workflow state.
- Is required by a Consumer contract.
- Is required for audit.
- Is required for legal or regulatory evidence.
- Starts, continues, blocks, or resolves a workflow.
- Creates or resolves a security or reconciliation condition.
- Changes an approved Recommendation, Decision, Command, or Confirmation state.

An Event should not be published for every internal variable assignment, database statement, cache update, User-interface refresh, or non-material implementation detail.

---

## 3. Canonical Event Envelope

### Envelope Principle

Every Canonical Event must use the approved common Event envelope.

The envelope provides stable metadata independent of the Event-specific payload.

### Required Envelope

```json
{
  "event_id": "018f9cf2-6ee4-7a62-b68e-9376a4cb338a",
  "event_type": "QuotationIssued",
  "event_version": "1.0.0",
  "tenant_id": "8f574a40-bd70-48e2-9da5-9a436edcff41",
  "occurred_at": "2026-08-01T18:31:24.512Z",
  "recorded_at": "2026-08-01T18:31:24.691Z",
  "producer": {
    "service": "quotation-domain-service",
    "service_version": "2026.08.1"
  },
  "subject": {
    "aggregate_type": "Quotation",
    "aggregate_id": "9f89d041-a8c0-42db-a57a-c13ae549e66a",
    "aggregate_version": 7
  },
  "authority": {
    "category": "ASOS_AUTHORITATIVE_WORKFLOW_STATE",
    "source_system": "ASOS"
  },
  "actor": {
    "actor_type": "USER",
    "actor_id": "f705f93f-377d-46e1-97cc-1dac4e7a5465",
    "role": "SALES_MANAGER"
  },
  "correlation_id": "448e8b97-9384-43ba-a5fb-e2aa0cd27559",
  "causation_id": "dd765b62-4c6e-4217-aa5d-1f88374b2d13",
  "payload_schema_ref": "asos://event-schemas/QuotationIssued/1.0.0",
  "data_classification": "COMMERCIAL_RESTRICTED",
  "evidence_references": [
    "evidence://quotation/9f89d041/version/7"
  ],
  "payload": {}
}
```

### Required Envelope Fields

| Field | Required | Description |
| :--- | :---: | :--- |
| `event_id` | Yes | Globally unique immutable Event identifier. |
| `event_type` | Yes | Approved canonical Event name. |
| `event_version` | Yes | Approved semantic version of the Event contract. |
| `tenant_id` | Yes | Primary Tenant-isolation boundary. |
| `occurred_at` | Yes | Time the business occurrence happened. |
| `recorded_at` | Yes | Time ASOS created the Event record. |
| `producer.service` | Yes | Approved producing service identifier. |
| `producer.service_version` | Yes | Producing implementation version. |
| `subject.aggregate_type` | Yes | Canonical aggregate or governed workflow type. |
| `subject.aggregate_id` | Yes | Identifier of the principal Event subject. |
| `subject.aggregate_version` | Conditional | Subject record version after or at the occurrence. |
| `authority.category` | Yes | Authority category supporting the Event. |
| `authority.source_system` | Conditional | External or internal source authority. |
| `actor` | Conditional | Human, service, Agent, system, or external actor. |
| `correlation_id` | Yes | Identifier linking one business process or request. |
| `causation_id` | Conditional | Identifier of the direct causing Event, Command, request, or Decision. |
| `payload_schema_ref` | Yes | Approved Event payload Schema reference. |
| `data_classification` | Yes | Security and privacy classification. |
| `evidence_references` | Conditional | Supporting evidence references. |
| `payload` | Yes | Event-specific approved payload. |

### Optional Envelope Fields

The Event envelope may include approved optional fields such as:

- `sequence_number`.
- `partition_key`.
- `region_code`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `environment`.
- `trace_id`.
- `recommendation_id`.
- `decision_id`.
- `command_id`.
- `external_confirmation_id`.
- `original_event_id`.
- `corrected_event_id`.
- `reversed_event_id`.
- `superseded_event_id`.
- `source_record_id`.
- `source_occurred_at`.
- `action_class`.
- `policy_id`.
- `policy_version`.
- `automation_policy_id`.
- `automation_policy_version`.
- `retention_class`.
- `legal_hold_status`.

Optional fields must be defined in the Event entry or shared envelope standard before use.

### Event Identifier

`event_id` must:

- Be globally unique.
- Remain unchanged during delivery retries.
- Remain unchanged during replay of the same Event.
- Never be reused for another occurrence.
- Be generated by the approved Event-producing boundary.
- Support reliable Consumer deduplication.

A new correction or reversal is a new occurrence and therefore requires a new `event_id`.

### Tenant Identifier

`tenant_id` is mandatory for Tenant-scoped Events.

It must:

- Come from trusted execution context.
- Not be accepted from an untrusted payload without verification.
- Match the principal subject and all Tenant-owned references.
- Be validated by Producers, the Event Backbone, and Consumers.
- Be included in authorization, routing, storage, replay, and audit controls.

Shared reference Events require an explicitly approved shared-reference scope and must never expose one Tenant’s internal data to another Tenant.

### Occurrence and Recording Times

`occurred_at` represents when the business occurrence happened.

`recorded_at` represents when the Canonical Event record was created.

They must not be silently treated as equivalent.

When an external source reports a historical occurrence, the Event may be recorded later.

The Event must preserve the external source timestamp where applicable.

### Producer

The Producer identifies the service that created the Canonical Event.

The Producer must not be inferred solely from the transport topic.

### Subject

The subject identifies the principal Canonical Domain Object or governed workflow.

An Event may reference additional Domain Objects inside its payload.

The Event entry must define the principal subject.

### Authority Category

Approved authority categories include:

- `EXTERNAL_AUTHORITATIVE_DATA`
- `ASOS_CANONICAL_PROJECTION`
- `ASOS_AUTHORITATIVE_WORKFLOW_STATE`
- `DERIVED_INTELLIGENCE`
- `AUTHORITATIVE_HUMAN_DECISION`
- `EXTERNAL_CONFIRMATION_AND_AUTHORITATIVE_OUTCOME`

The Event must not claim a stronger authority than its evidence supports.

### Actor

Permitted actor types may include:

- `USER`
- `SERVICE`
- `AI_AGENT`
- `AUTOMATION`
- `CUSTOMER`
- `EXTERNAL_SYSTEM`
- `UNKNOWN_EXTERNAL_ACTOR`

An AI Agent may be the actor for a Derived Intelligence Event.

An AI Agent must not be recorded as the authoritative actor for a binding Human Decision.

### Correlation and Causation

`correlation_id` links Events and operations belonging to the same business process.

`causation_id` identifies the direct cause where known.

Possible causes include:

- Prior Event.
- API request.
- Workflow transition.
- Human Decision.
- Command.
- External Confirmation.
- Scheduled Job.

Correlation does not prove causation.

### Payload Schema Reference

The Event Catalog defines the Event payload contract.

The approved machine-readable representation will be governed in the Data Schemas Catalog.

Every published Event version must reference an approved Schema.

### Evidence References

Evidence references must point to controlled evidence records.

Raw restricted evidence should not be copied into the Event payload when a secure reference is sufficient.

### Immutable Content and Delivery Metadata

The Event envelope and payload form the immutable business Event.

Transport systems may add separate delivery metadata such as:

- Delivery attempt.
- Broker partition.
- Broker offset.
- Subscription.
- Replay batch.
- Delivery timestamp.
- Dead-letter reason.

Transport metadata must not alter the Event’s business meaning or identity.

---

## 4. Naming and Versioning

### Event Naming Standard

Canonical Event names must:

- Use `PascalCase`.
- Use a clear Domain or subject noun.
- Use a past-tense occurrence.
- Describe a fact.
- Avoid implementation-specific technology names.
- Avoid ambiguous abbreviations.
- Avoid future-action language.
- Avoid Tenant or vendor names.
- Remain stable after publication.

Preferred pattern:

```text
<Subject><PastTenseOccurrence>
```

Examples:

```text
LeadCreated
LeadQualified
OpportunityStageChanged
AppointmentScheduled
AppointmentConfirmed
QuotationIssued
QuotationAccepted
TradeInAppraised
FinanceApplicationSubmitted
LenderDecisionRecorded
FinancialContractFullySigned
DealCompleted
InteractionReceived
MarketIntelligencePublished
```

### Command Names Are Not Event Names

The following are Commands and must not be registered as Events:

```text
CreateLead
ScheduleAppointment
IssueQuotation
SendFollowUp
ReserveVehicle
SubmitFinanceApplication
CompleteDeal
```

Corresponding Event names may include:

```text
LeadCreated
AppointmentScheduled
QuotationIssued
FollowUpSent
VehicleReserved
FinanceApplicationSubmitted
DealCompleted
```

### Requested Versus Completed

When the request itself is a material fact, the Event name must make that explicit.

```text
AppointmentCancellationRequested
AppointmentCancelled
```

```text
FundingRequested
FundingConfirmed
```

```text
DealCompletionRequested
DealCompleted
```

### Observed Versus Confirmed

External observations must remain distinguishable from accepted outcomes.

```text
PaymentStatusObserved
PaymentCleared
```

```text
ExternalReservationStatusObserved
VehicleReservationConfirmed
```

### Predicted Versus Actual

```text
DealCompletionProbabilityCalculated
DealCompleted
```

```text
CustomerIntentClassified
QuotationAccepted
```

### Event Version Format

`event_version` uses Semantic Versioning:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
1.0.0
1.1.0
2.0.0
```

### Major Version

Increase the major version when a change is incompatible, including:

- Removing a field.
- Renaming a field.
- Changing a field type incompatibly.
- Changing required meaning.
- Changing authority classification materially.
- Changing Event semantics.
- Changing the principal subject.
- Changing a field from optional to required where old Consumers may fail.
- Restricting an enumeration incompatibly.
- Changing units, currency semantics, time semantics, or identifier meaning.

A major version may require parallel publication and Consumer migration.

### Minor Version

Increase the minor version for backward-compatible additions such as:

- Adding an optional field.
- Adding an optional evidence reference.
- Adding a compatible enumeration value where Consumers are required to tolerate unknown values.
- Adding non-breaking metadata.
- Clarifying optional Consumer behaviour.

### Patch Version

Increase the patch version for non-breaking corrections such as:

- Documentation clarification.
- Typographical correction.
- Example correction.
- Constraint clarification that does not change the valid wire contract.

### Semantic Stability

The meaning of an existing Event name and major version must not be changed silently.

A materially different occurrence requires:

- A new major version; or
- A new Event name when the business meaning is different.

### Event Entry Version Versus Producer Version

`event_version` identifies the Event contract.

`producer.service_version` identifies the producing implementation.

They are separate.

### Compatibility Rules

Consumers must:

- Declare supported Event types and major versions.
- Validate the received Event version.
- Ignore unknown optional fields where safe.
- Reject or quarantine unsupported major versions.
- Avoid failing on compatible minor additions.
- Avoid assuming enumerations are permanently closed unless the Event contract says so.
- Preserve the original Event when transformation is required.

Producers must not publish an unapproved version.

### Dual Publication

During major-version migration, governed dual publication may be used.

Dual publication requires:

- Explicit migration approval.
- Defined start and end dates.
- Distinct Event versions.
- Stable Event identities according to the approved migration design.
- Consumer-impact assessment.
- Duplicate-effect prevention.
- Monitoring.
- Retirement plan.

Dual publication must not accidentally create two business effects.

### Deprecation

Deprecation must define:

- Deprecated Event type and version.
- Replacement.
- Reason.
- Consumer inventory.
- Migration deadline.
- Final publication date.
- Replay implications.
- Retention requirements.
- Approval.

Deprecated Events remain part of historical audit records.

### Retirement

An Event version may be retired from new publication only after:

- All required Producers have migrated.
- All required Consumers have migrated or been removed.
- Replay requirements are resolved.
- Historical records remain readable.
- Security and retention obligations are preserved.
- Governance approval is recorded.

---

## 5. Producer Contracts

### Producer Authority

Each Event entry must name its authorized Producer or Producer class.

Only the approved Producer may publish the canonical Event unless the Event entry explicitly allows multiple Producers.

Where multiple systems observe the same business condition:

- External systems may publish normalized observation Events.
- The responsible Canonical Domain Service publishes the accepted Canonical Domain Event.

### Transactional Consistency

A Domain Event must represent an accepted business-state change.

The Producer must use a reliable publication pattern such as:

- Transactional outbox.
- Change-data-capture from an approved outbox.
- Equivalent atomic state-and-Event commitment mechanism.

The Producer must prevent a state change from being permanently committed without its required Event.

The Producer must also prevent an Event from claiming a state change that was not accepted.

### Event Creation Timing

The Event should be created after:

- Required validation passes.
- Required authority is confirmed.
- The state transition is accepted.
- The applicable record version is assigned.
- Required evidence references exist.

The Event should not be published merely because a User clicked a button or a request was received, unless the request itself is the Event’s defined occurrence.

### Event Identity During Retry

When retrying publication of the same accepted occurrence:

- Reuse the same `event_id`.
- Reuse the same immutable Event content.
- Do not create a new Event merely because transport failed.

When a genuinely new occurrence happens, create a new Event.

### Aggregate Version

Where the subject is versioned, the Event should preserve the applicable aggregate version.

Consumers may use aggregate version for concurrency and ordering validation.

Aggregate version does not replace `event_id` deduplication.

### Evidence and Authority

The Producer must ensure the Event authority matches the evidence.

Examples:

- A communication provider may establish delivery.
- A Lender may establish a Lender Decision.
- A Human Decision service may establish approval.
- An AI Agent may establish that an AI Recommendation was generated.
- An AI Agent may not establish that a Vehicle was delivered.

### Payload Minimization

The Producer must publish only the minimum data needed by approved Consumers.

The Producer should prefer:

- Canonical identifiers.
- Secure evidence references.
- Status and version.
- Changed fields where governed.
- Non-sensitive summary fields.

The Producer should avoid:

- Full Customer documents.
- Raw identity documents.
- Bank information.
- Signature images.
- Access tokens.
- Secrets.
- Full unrestricted Interaction content.
- Unnecessary personal information.
- Internal model instructions.
- Hidden Prompts.

### No Secret Material

Events must never include:

- Passwords.
- API keys.
- OAuth tokens.
- Session tokens.
- Private keys.
- Webhook secrets.
- Database credentials.
- Encryption keys.
- Unredacted authentication secrets.

### Producer Validation

Before publication, the Producer must validate:

- Event type.
- Event version.
- Required envelope.
- Payload Schema.
- Tenant consistency.
- Subject existence.
- Authority.
- Actor.
- Occurrence time.
- Correlation and causation.
- Data classification.
- Evidence requirements.
- Security restrictions.
- Event-size limits.
- Catalog approval status.

### Publication Authorization

The Event Backbone must accept publication only from authorized service identities.

Authorization should consider:

- Producer service.
- Tenant scope.
- Event type.
- Event version.
- Environment.
- Data classification.
- Publication topic or stream.

### Producer Audit

The Producer must record:

- State transition.
- Event identifier.
- Event type and version.
- Subject and aggregate version.
- Actor.
- Authority.
- Evidence.
- Publication status.
- Publication attempts.
- Failure.
- Correlation and causation.
- Timestamp.

### Producer Failure Behaviour

If Event publication is temporarily unavailable:

- Preserve the committed outbox entry or equivalent durable record.
- Retry safely.
- Do not create duplicate business Events.
- Monitor backlog.
- Escalate prolonged failure.
- Preserve ordering requirements where defined.
- Do not bypass security or validation.

### AI Producer Rules

AI Agents may produce Events concerning:

- Agent Run lifecycle.
- Derived Intelligence generation.
- Classification.
- Forecast.
- Recommendation.
- Explanation.
- Evaluation.
- Failure.
- Human Review Recommendation.

AI Agents must not publish authoritative Events for:

- Customer identity.
- Consent.
- Quotation acceptance.
- Finance approval.
- Contract signature.
- Payment clearing.
- Funding.
- Vehicle Reservation.
- Vehicle Allocation.
- Delivery.
- Deal completion.
- Another binding external outcome.

---

## 6. Consumer Contracts

### Consumer Registration

Every Event Consumer must be registered with:

- Consumer name.
- Owning team or service.
- Supported Event types.
- Supported major versions.
- Tenant scope.
- Data-access purpose.
- Required fields.
- Processing guarantees.
- Retry policy.
- Dead-letter policy.
- Replay policy.
- External-action capability.
- Security classification.
- Operational owner.

### Consumer Processing Sequence

A Consumer must perform the applicable sequence:

```text
Receive Event
  ↓
Authenticate delivery source
  ↓
Validate Event envelope
  ↓
Validate tenant_id and authorization
  ↓
Validate supported Event type and version
  ↓
Check event_id processing ledger
  ↓
Validate payload Schema
  ↓
Validate subject and required authority
  ↓
Apply idempotent business logic
  ↓
Record processing result
  ↓
Acknowledge delivery
```

### Consumer Deduplication

Every Consumer must prevent duplicate business effects using `event_id`.

A Consumer should maintain an inbox or equivalent processing ledger containing:

- Consumer identifier.
- `tenant_id`.
- `event_id`.
- Event type.
- Event version.
- First received time.
- Processing status.
- Processing attempts.
- Successful completion time.
- Result reference.
- Failure.
- Replay context where applicable.

### Duplicate Effects That Must Be Prevented

Duplicate delivery must not create duplicate:

- State transitions.
- Tasks.
- Recommendations.
- Human Review requests.
- Customer communications.
- Appointments.
- Quotations.
- Reservations.
- Allocations.
- Payment requests.
- Funding requests.
- Contract requests.
- Commands.
- External updates.
- Notifications.
- Audit outcomes.

### Idempotent State Transitions

A Consumer must validate current state before applying a transition.

Receiving the same Event again must produce the same safe result without duplicating the effect.

### Event ID Versus Command Idempotency

```text
event_id
  = prevents duplicate Consumer effects from the same Event

idempotency_key
  = prevents duplicate execution of a retryable Command or operation
```

A Consumer that creates a Command should derive or preserve a stable idempotency intent according to the applicable Command contract.

### Tenant Validation

A Consumer must not trust topic routing alone.

It must validate:

- `tenant_id`.
- Subject Tenant.
- Related Tenant-owned identifiers.
- Consumer authorization.
- Data classification.
- Purpose.

A Tenant mismatch must fail closed and create a security record.

### Version Validation

A Consumer must:

- Accept only supported major versions.
- Apply compatible handling for approved minor and patch versions.
- Quarantine unsupported versions.
- Avoid guessing field meaning.
- Avoid silently discarding a material Event.
- Alert the responsible owner where required.

### Authority Validation

Consumers must not interpret an Event beyond its authority.

Examples:

- `VehicleMatchRecommended` does not authorize Allocation.
- `CommandSent` does not confirm external completion.
- `PaymentStatusObserved` does not automatically mean Payment cleared.
- `FinancialContractFullySigned` does not automatically mean effective.
- `DeliveryAppointmentCompleted` does not automatically mean Vehicle delivered.

### Consumer Side Effects

Before creating a side effect, a Consumer must validate:

- Event authority.
- Current Domain state.
- Applicable Business Rules.
- Action Class.
- Required Human Approval.
- Approved automation policy.
- Consent.
- Permissions.
- External Confirmation requirements.
- Idempotency.
- Audit requirements.

### Failure and Retry

Consumer processing failures must be classified as:

- Retryable technical failure.
- Non-retryable validation failure.
- Unsupported version.
- Authorization failure.
- Tenant mismatch.
- Data conflict.
- Missing dependency.
- Business Rule rejection.
- Reconciliation required.
- Security incident.

Retryable failures may be retried according to policy.

Non-retryable failures must not loop indefinitely.

### Dead-Letter Handling

Dead-letter handling must preserve:

- Original immutable Event.
- Consumer.
- Failure reason.
- Attempts.
- First failure time.
- Last failure time.
- Tenant.
- Security classification.
- Correlation and causation.
- Resolution status.
- Reprocessing authorization.

Dead-letter queues must not expose Events to unauthorized operators.

### Consumer Audit

Consumers must record material processing outcomes including:

- Event identifier.
- Consumer.
- Tenant.
- Processing result.
- State change.
- Created Command or Task.
- Duplicate detection.
- Failure.
- Retry.
- Dead-letter.
- Replay.
- Timestamp.

### Consumer Evolution

A Consumer change must assess:

- Supported Event versions.
- Historical replay.
- New optional fields.
- Enumeration evolution.
- Duplicate prevention.
- Ordering.
- Security.
- External side effects.
- Backward compatibility.

---

## 7. Delivery, Ordering, Replay, and Deduplication

### Delivery Guarantee

The Event Backbone provides:

```text
At-least-once delivery
```

The Event Backbone does not promise exactly-once physical delivery.

Business-level duplicate prevention must be implemented by Consumers.

### Delivery Attempts

The same immutable Event may be delivered multiple times because of:

- Producer retry.
- Broker retry.
- Consumer timeout.
- Consumer failure.
- Network interruption.
- Subscription recovery.
- Controlled replay.
- Disaster recovery.
- Backfill.

Every delivery of the same Event must preserve the same `event_id`.

### Ordering Principle

ASOS does not assume global Event ordering.

Ordering is guaranteed only where explicitly defined by the Event entry and supported by the approved Event Backbone design.

### Aggregate Ordering

Where an aggregate requires ordered consumption, the Event may include:

- `aggregate_id`.
- `aggregate_version`.
- `sequence_number`.
- Approved partition key.

Consumers must define how they handle:

- Expected sequence.
- Duplicate sequence.
- Future sequence.
- Missing sequence.
- Late Event.
- Reordered delivery.
- Corrective Event.
- Replay.

### No Cross-Aggregate Ordering Assumption

Consumers must not assume that Events concerning different aggregates arrive in business-time order.

Correlation identifiers do not create a delivery-order guarantee.

### Late Events

A late Event must not be ignored solely because a later Event was already received.

The Consumer must evaluate:

- Event semantics.
- Aggregate version.
- Occurrence time.
- Current state.
- Correction or reversal relationships.
- Authority.
- Reconciliation rules.

### Event Replay

Replay means controlled redelivery of retained immutable Events.

Replay may be used for:

- Rebuilding projections.
- Recovering a Consumer.
- Migrating a Consumer.
- Recomputing analytics.
- Testing an approved new version.
- Incident recovery.
- Reconciliation.
- Backfilling an approved derived view.

### Replay Authorization

Replay requires:

- Approved requester.
- Defined Tenant scope.
- Defined Event types and versions.
- Defined time range.
- Defined Consumer.
- Defined business purpose.
- Data-classification authorization.
- External-side-effect policy.
- Rate and capacity controls.
- Audit record.
- Start and completion evidence.

### Replay Safety

A replay-capable Consumer must declare one of the following modes:

- `STATE_REBUILD_ONLY`
- `NO_EXTERNAL_SIDE_EFFECTS`
- `IDEMPOTENT_SIDE_EFFECTS_ALLOWED`
- `MANUAL_APPROVAL_REQUIRED`
- `REPLAY_NOT_SUPPORTED`

Replay must not accidentally resend:

- Customer communication.
- Payment requests.
- Funding requests.
- Inventory Commands.
- Contract Commands.
- External appointments.
- Sale postings.
- Another non-reversible external action.

### Replay Metadata

Replay delivery context may include transport metadata such as:

- Replay request identifier.
- Replay batch identifier.
- Replay start time.
- Replay operator.
- Replay reason.

Replay metadata must remain separate from the immutable Event body.

### Retention

Event-retention requirements must consider:

- Domain requirements.
- Legal requirements.
- Regulatory requirements.
- Contractual requirements.
- Audit requirements.
- Security requirements.
- Privacy requirements.
- Replay requirements.
- Source-license restrictions.
- Tenant configuration.

Retention must not be shortened merely for technical convenience.

### Deduplication Retention

Consumer deduplication records must remain available long enough to cover:

- Event-retention window.
- Maximum replay window.
- Retry window.
- Disaster-recovery window.
- Applicable legal and operational risk.

### Topic and Stream Design

Physical topics, streams, queues, or partitions are deployment concerns.

They must not redefine Event semantics.

Deployment design must preserve:

- Tenant isolation.
- Producer authorization.
- Consumer authorization.
- Event version routing.
- Data classification.
- Ordering requirements.
- Replay.
- Dead-letter handling.
- Monitoring.

### Backpressure

The Event Backbone and Consumers must support controlled handling of backpressure.

Backpressure must not cause:

- Silent Event loss.
- Tenant leakage.
- Unbounded retry storms.
- Duplicate external execution.
- Disabled authorization.
- Disabled audit.
- Uncontrolled data retention.

### Monitoring

Required operational monitoring includes:

- Publication latency.
- Delivery latency.
- Consumer lag.
- Duplicate deliveries.
- Duplicate effects prevented.
- Processing failures.
- Dead-letter volume.
- Unsupported versions.
- Tenant violations.
- Replay activity.
- Ordering conflicts.
- Outbox backlog.
- Event size.
- Schema-validation failure.

---

## 8. Corrections, Reversals, Cancellations, and Revocations

### Immutability Principle

A published Event must never be edited to change history.

A published Event must not be silently deleted because:

- The payload was wrong.
- The business outcome changed.
- A User changed their mind.
- An external system reversed an action.
- A record was merged.
- A Deal was cancelled.
- A Consent was withdrawn.

A new governed Event must represent the later occurrence.

### Correction

A correction means the prior Event contained inaccurate or incomplete information.

Example:

```text
LeadCreated
LeadDataCorrected
```

The correction Event must preserve:

- New `event_id`.
- Original Event reference.
- Corrected Event or subject reference.
- Correction reason.
- Corrected fields or corrected snapshot.
- Authority.
- Actor.
- Evidence.
- Occurrence and recording times.
- Applicable record version.
- Correlation and causation.

A correction must not pretend the original Event never existed.

### Reversal

A reversal means the original occurrence happened but its effect was later reversed.

Examples:

```text
PaymentCleared
PaymentReversed
```

```text
FundingConfirmed
FundingReversed
```

```text
VehicleAllocated
VehicleAllocationReleased
```

The reversal Event must reference the original occurrence where applicable.

### Cancellation

Cancellation means a planned, pending, or active business workflow was cancelled according to its Domain rules.

Examples:

- `AppointmentCancelled`
- `QuotationCancelled`
- `FinanceApplicationCancelled`
- `DealCancelled`

Cancellation is a new business occurrence.

It is not Event deletion.

### Revocation

Revocation means a previously valid approval, confirmation, permission, or authority was withdrawn.

Examples:

- `AppointmentConfirmationRevoked`
- `ApprovalRevoked`
- `ConsentWithdrawn`
- `PublicationWithdrawn`

The Event entry must define the effect of revocation on Consumers.

### Rescission

Rescission is a legally or contractually governed withdrawal of an accepted or effective action.

Examples may include:

- `FinancialContractRescissionRequested`
- `FinancialContractRescinded`

Rescission must remain separate from ordinary cancellation.

### Void

A void Event records that a legally significant record was declared void through an authorized process.

Example:

- `FinancialContractVoided`

Voiding must preserve the original record and evidence.

### Unwind

An unwind Event records controlled reversal of a materially executed transaction.

Example:

- `DealUnwound`

Unwind must not be represented as ordinary Deal cancellation.

### Supersession

Supersession means a newer version replaces the prior version for current use.

Examples:

- `QuotationSuperseded`
- `FinancialContractSuperseded`
- `MarketIntelligenceSuperseded`

Historical versions and Events remain available.

### Merge and Split

Entity merges and splits require explicit Events such as:

- `CustomerMergeApproved`
- `CustomerMerged`
- `ConversationThreadSplit`

Merging must not delete original identifiers or evidence.

### Privacy Redaction

Privacy redaction must not falsify historical business Events.

Where lawful redaction is required:

- Preserve minimum required audit evidence.
- Apply governed payload minimization or protected references.
- Record `EventDataRedactionApplied` or equivalent where required.
- Preserve legal holds.
- Propagate redaction to indexes, projections, and authorized replicas.
- Avoid exposing the original restricted data to ordinary Consumers.

The Security and Privacy architecture determines whether cryptographic deletion, token revocation, anonymization, or retained legal evidence applies.

### Correction Consumer Behaviour

Consumers of corrective or reversal Events must:

- Preserve original processing evidence.
- Apply the correction idempotently.
- Avoid duplicate compensating actions.
- Evaluate downstream Commands.
- Evaluate Customer communication.
- Evaluate financial and legal consequences.
- Create reconciliation when automatic correction is unsafe.
- Preserve audit linkage.

### Event Chain

Correction and reversal relationships may form an Event chain.

The chain must prevent:

- Circular references.
- Missing original Event references where required.
- Conflicting active corrections.
- Silent replacement.
- Unauthorized history rewriting.

---

## 9. Security, Privacy, and Tenant Isolation

### Security Principle

Events are governed business records.

They must receive security protection appropriate to their:

- Tenant.
- Data classification.
- Domain.
- Participants.
- Evidence.
- Legal significance.
- Financial significance.
- Source-license restrictions.
- Retention requirements.

### Authentication

Every Producer and Consumer requires authenticated service identity.

Anonymous Event publication and unrestricted subscription are prohibited.

### Authorization

Event authorization must consider:

- `tenant_id`.
- Producer identity.
- Consumer identity.
- Event type.
- Event version.
- Domain.
- Subject.
- Dealership and branch scope.
- Requested operation.
- Data classification.
- Business purpose.
- Environment.
- Source-license restriction.
- Legal hold.
- Delegated authority.

### Tenant Isolation

Tenant isolation must apply to:

- Event creation.
- Publication.
- Routing.
- Partitioning.
- Storage.
- Subscription.
- Consumption.
- Replay.
- Dead-letter handling.
- Search.
- Monitoring.
- Export.
- Analytics.
- Backup.
- Support access.
- AI processing.

A Consumer must validate `tenant_id` even when the physical topic is Tenant-specific.

### Shared Reference Events

Shared-reference Events require explicit governance.

They must:

- Use an approved shared-reference scope.
- Contain no Tenant-private data.
- Identify the shared reference authority.
- Use separate access policy.
- Avoid allowing one Tenant to infer another Tenant’s activity.

### Data Classification

Every Event must have an approved classification.

Example classifications include:

- `PUBLIC_REFERENCE`
- `INTERNAL_OPERATIONAL`
- `CUSTOMER_RESTRICTED`
- `COMMERCIAL_RESTRICTED`
- `FINANCIAL_RESTRICTED`
- `IDENTITY_RESTRICTED`
- `CONTRACTUAL_RESTRICTED`
- `LEGAL_AND_COMPLIANCE`
- `SECURITY_RESTRICTED`
- `LICENSED_SOURCE_RESTRICTED`
- `AUDIT_EVIDENCE`

### Data Minimization

Event payloads must contain the minimum information required by registered Consumers.

A reference should be used instead of raw sensitive content where possible.

### Personal Data

Events containing personal data must define:

- Purpose.
- Lawful basis.
- Authorized Consumers.
- Retention.
- Redaction or deletion behaviour.
- Export restrictions.
- AI restrictions.
- Audit requirements.

### Restricted Data

The following should normally remain outside Event payloads:

- Raw identity documents.
- Full national identifiers.
- Bank-account details.
- Payment-card data.
- Credit reports.
- Full Financial Contracts.
- Signature images.
- Full call recordings.
- Unredacted legal documents.
- Full Customer Interaction histories.
- Secrets and credentials.

Secure references may be included when authorized.

### Encryption

Events must use approved encryption:

- In transit.
- At rest.
- In backups.
- In dead-letter storage.
- In replay storage.
- In retained archives.

### Integrity

The platform should support appropriate integrity controls such as:

- Authenticated Producers.
- Broker access controls.
- Event hashes where required.
- Signed transport or message envelopes where required.
- Immutable storage.
- Audit records.
- Schema validation.
- Tamper detection.

### Event Size

Maximum Event size must remain deployment-configurable within an approved architecture profile.

Large documents, recordings, images, reports, and payloads should use controlled storage references.

### Logs

Operational Logs must not copy complete restricted Event payloads by default.

Logs should prefer:

- Event identifier.
- Event type.
- Event version.
- Tenant-safe operational metadata.
- Processing result.
- Secure error classification.

### Dead-Letter Security

Dead-letter Events must preserve the same or stronger security controls as the original stream.

### Replay Security

Replay requires explicit authorization.

Replay must not broaden:

- Tenant scope.
- Consumer scope.
- Historical data access.
- Personal-data access.
- Licensed-source access.
- Legal-hold access.

### AI Consumption

AI Agents may consume only approved Event fields for an approved purpose.

AI Event consumption must enforce:

- Tenant scope.
- Field restrictions.
- Data minimization.
- Source and evidence references.
- Security classification.
- Model and Prompt approval.
- Retention.
- No cross-Tenant leakage.
- No unrestricted training use.
- No direct external action.

### Security Events

The platform must detect and record applicable:

- Unauthorized publication.
- Unauthorized subscription.
- Tenant mismatch.
- Invalid Producer.
- Unsupported Event version.
- Schema manipulation.
- Replay without authorization.
- Dead-letter data exposure.
- Event tampering.
- Payload containing secrets.
- Cross-Tenant processing.
- Duplicate external effect.
- Audit-record tampering.

### Emergency Suspension

The platform must support Tenant-scoped or Event-type-scoped suspension of:

- Event publication.
- Specific Producers.
- Specific Consumers.
- Replay.
- External side-effect Consumers.
- AI Event consumption.
- Event export.
- Connector-driven Events.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## 10. Catalog Structure and Event Entry Template

### Approved Directory Structure

```text
02-Event-Catalog/
├── README.md
├── 00-Shared-Event-Envelope.md
├── 01-Customer-Events.md
├── 02-Vehicle-Events.md
├── 03-InventoryRecord-Events.md
├── 04-Lead-Events.md
├── 05-QualifiedLead-Events.md
├── 06-Opportunity-Events.md
├── 07-Appointment-Events.md
├── 08-Quotation-Events.md
├── 09-TradeIn-Events.md
├── 10-FinanceApplication-Events.md
├── 11-FinancialContract-Events.md
├── 12-Deal-Events.md
├── 13-Interaction-Events.md
├── 14-MarketIntelligence-Events.md
├── 15-Shared-Workflow-Events.md
├── 16-Recommendation-and-Decision-Events.md
├── 17-Command-and-Confirmation-Events.md
└── 18-Data-Quality-and-Reconciliation-Events.md
```

Files must be created through the controlled roadmap.

The existence of a planned filename does not approve its Event definitions.

### Event Registry Requirements

Each approved Event must have one authoritative registry entry.

An Event entry must contain:

1. Event metadata.
2. Semantic statement.
3. Business trigger.
4. Explicit non-meaning.
5. Authorized Producer.
6. Intended Consumers.
7. Principal subject.
8. Authority classification.
9. Required envelope fields.
10. Payload contract.
11. Validation rules.
12. Evidence requirements.
13. Ordering requirements.
14. Consumer deduplication requirements.
15. Correction and reversal relationships.
16. Security classification.
17. Privacy and retention.
18. Version compatibility.
19. Example Event.
20. Contract tests.
21. Approval and change history.

### Required Event Entry Template

```markdown
# <EventType>

**Event Version:** 1.0.0
**Status:** Draft | Review Pending | Approved | Deprecated | Retired
**Owning Domain:** <Domain>
**Authorized Producer:** <Service>
**Primary Subject:** <Aggregate Type>
**Authority Category:** <Authority Category>
**Data Classification:** <Classification>
**Last Updated:** <Date>

---

## 1. Semantic Statement

State exactly what happened.

## 2. Business Trigger

Define the accepted condition that creates the Event.

## 3. Explicit Non-Meaning

Define what Consumers must not infer.

## 4. Producer Contract

Define Producer, validation, timing, evidence, and transaction boundary.

## 5. Consumer Contract

Define approved Consumers and permitted reactions.

## 6. Envelope Requirements

Define required shared and Event-specific envelope fields.

## 7. Payload Contract

Define fields, types, required status, authority, and meaning.

## 8. Validation and Invariants

Define required business, Tenant, security, and lifecycle validation.

## 9. Ordering and Delivery

Define ordering scope, partitioning expectation, and at-least-once handling.

## 10. Correction, Reversal, and Supersession

Define related corrective Events.

## 11. Security, Privacy, and Retention

Define classification, access, minimization, and retention.

## 12. Examples and Contract Tests

Provide valid and invalid examples and required tests.
```

### Payload Field Definition

Each payload field must define:

| Attribute | Description |
| :--- | :--- |
| `name` | Canonical field name |
| `type` | Approved logical type |
| `required` | Required or optional |
| `authority` | Authority for the value |
| `description` | Precise meaning |
| `classification` | Security classification |
| `source` | Source field or calculation |
| `compatibility` | Version-evolution restrictions |
| `example` | Safe non-production example |

### Event Example Requirements

Examples must:

- Use fictional identifiers.
- Use no real Customer information.
- Use no secrets.
- Use valid timestamps.
- Use the approved envelope.
- Match the Event version.
- Clearly distinguish observation from confirmation.
- Avoid dealership-specific hard-coded policies.

### Contract Tests

Every approved Event must have planned or implemented tests for:

- Envelope validity.
- Payload Schema validity.
- Required fields.
- Tenant consistency.
- Authority consistency.
- Valid Event version.
- Invalid unsupported version.
- Producer authorization.
- Consumer deduplication.
- Duplicate delivery.
- Ordering where applicable.
- Correction relationship.
- Security classification.
- Sensitive-data exclusion.
- Replay behaviour.
- Backward compatibility.

### Event Statuses

Approved Event-entry statuses are:

- `DRAFT`
- `REVIEW_PENDING`
- `APPROVED`
- `DEPRECATED`
- `RETIRED`

Only `APPROVED` Event types and versions may be used for new Production publication.

### Catalog Index

This README remains the governance index.

Individual Event files become authoritative only after approval and linkage from this index.

---

## 11. Governance and Change Control

### Governance Principle

The Event Catalog is a controlled architectural contract.

Event changes must not be made solely for Producer convenience.

Every change must consider:

- Domain semantics.
- Consumers.
- Integrations.
- Audit.
- Security.
- Tenant isolation.
- Replay.
- Retention.
- AI.
- Human Approval.
- Backward compatibility.
- Migration.

### Event Proposal

A new Event proposal must include:

- Business occurrence.
- Owning Domain.
- Reason the Event is required.
- Authorized Producer.
- Intended Consumers.
- Authority category.
- Principal subject.
- Payload.
- Evidence.
- Security classification.
- Ordering.
- Correction behaviour.
- Expected volume.
- Retention.
- Alternative designs considered.

### Proposal Review

Required reviewers may include:

- Domain Owner.
- Architecture Governance.
- Data Governance.
- Security.
- Privacy.
- Integration Owner.
- Consumer Owners.
- Legal or Compliance where applicable.
- AI Governance where applicable.
- Operational Owner.

### Approval Criteria

An Event may be approved only when:

- Its meaning is unambiguous.
- It represents a material past occurrence.
- It is not a disguised Command.
- Ownership is clear.
- Authority is clear.
- Producer is authorized.
- Consumers are identified.
- Payload is minimized.
- Tenant isolation is defined.
- Security classification is defined.
- Version is assigned.
- Compatibility is assessed.
- Correction and reversal behaviour is defined.
- Replay behaviour is defined.
- Tests are defined.
- No conflicting Event already exists.

### Duplicate Event Prevention

Before creating a new Event, governance must check for:

- Existing equivalent Event.
- Existing Event with different wording.
- Observation versus confirmation confusion.
- Command versus Event confusion.
- Domain ownership conflict.
- Event that can be extended compatibly.
- Event that requires a new major version instead.

### Change Request

A change request must identify:

- Existing Event type and version.
- Proposed change.
- Reason.
- Compatibility classification.
- Affected Producers.
- Affected Consumers.
- Security impact.
- Privacy impact.
- Replay impact.
- Migration plan.
- Deprecation plan where applicable.
- Rollback plan.
- Testing plan.

### Compatibility Classification

Every change must be classified as:

- `PATCH_COMPATIBLE`
- `MINOR_BACKWARD_COMPATIBLE`
- `MAJOR_BREAKING`
- `NEW_EVENT_TYPE`
- `DEPRECATION`
- `RETIREMENT`
- `SECURITY_EMERGENCY_CHANGE`

### Breaking Change Controls

A breaking change requires:

- New major version or new Event type.
- Consumer inventory.
- Migration plan.
- Dual-publication decision.
- Defined migration period.
- Monitoring.
- Approval.
- Retirement plan.
- Historical replay plan.

### Emergency Security Change

A critical security vulnerability may require emergency suspension or restriction.

Emergency changes must:

- Protect Tenant and Customer data.
- Preserve evidence.
- Be approved by authorized security governance.
- Be documented.
- Notify affected owners.
- Include compatibility and recovery assessment.
- Receive retrospective governance review.

### Event Ownership

Every Event must have:

- Domain Owner.
- Technical Producer Owner.
- Catalog Owner.
- Operational Owner.
- Consumer registry.
- Security classification owner.

### Consumer Inventory

The catalog must maintain a registry of known Consumers.

An Event must not be deprecated or retired without reviewing registered Consumers.

### Documentation Synchronization

When an Event is approved or changed, applicable updates may be required in:

- Domain Model.
- API Contracts.
- Data Schemas.
- Agent Contracts.
- Integration Contracts.
- Business Rules.
- Playbooks.
- Security documentation.
- Architecture documentation.
- Tests.
- Deployment profiles.

### Audit

Governance Decisions must preserve:

- Proposal.
- Reviewers.
- Decision.
- Reason.
- Version.
- Compatibility.
- Security impact.
- Consumer impact.
- Approval time.
- Effective time.
- Deprecation or retirement time.

### Periodic Review

The Event Catalog should receive periodic review for:

- Unused Events.
- Duplicate Events.
- Ambiguous semantics.
- Unsupported versions.
- Security classification.
- Personal-data minimization.
- Excessive payloads.
- Dead Consumers.
- Replay readiness.
- Missing correction semantics.
- Naming inconsistency.
- Producer ownership.
- Consumer ownership.

---

## 12. Initial Catalog Roadmap

### Phase 1 — Governance Baseline

This document establishes:

- Event purpose.
- Event semantics.
- Event taxonomy.
- Event envelope.
- Naming.
- Versioning.
- Producer rules.
- Consumer rules.
- At-least-once delivery.
- Deduplication.
- Replay.
- Correction and reversal rules.
- Security.
- Catalog structure.
- Change governance.

Status after approval:

```text
Event Catalog Governance
Version: 1.0.0
Status: Approved Baseline
```

### Phase 2 — Shared Event Envelope

The next controlled file will be:

```text
00-Shared-Event-Envelope.md
```

It must define the detailed shared envelope contract, including:

- Logical field types.
- Required fields.
- Optional fields.
- Authority categories.
- Actor structure.
- Subject structure.
- Correlation.
- Causation.
- Evidence.
- Data classification.
- Validation.
- Examples.
- Machine-readable Schema handoff requirements.

### Phase 3 — Core Commercial Journey Events

Recommended initial Domain Event order:

1. Lead Events.
2. Qualified Lead Events.
3. Opportunity Events.
4. Appointment Events.
5. Quotation Events.
6. Interaction Events.
7. Deal Events.

This order establishes the core sales journey before dependent execution workflows.

### Phase 4 — Vehicle and Transaction Execution Events

1. Vehicle Events.
2. Inventory Record Events.
3. Trade-In Events.
4. Finance Application Events.
5. Financial Contract Events.
6. Deal execution and completion Events.

### Phase 5 — Customer and Intelligence Events

1. Customer Events.
2. Consent and communication-permission Events where governed.
3. Market Intelligence Events.
4. Derived Intelligence and Recommendation Events.

### Phase 6 — Shared Workflow Events

Shared workflow catalogs will define:

- Human Review Events.
- Approval Events.
- Recommendation Events.
- Human Decision Events.
- Command Lifecycle Events.
- External Confirmation Events.
- Data Quality Events.
- Reconciliation Events.
- Emergency-control Events.

### Phase 7 — Data Schema Handoff

Approved Event entries will be converted into machine-readable Event Schemas under:

```text
04-Data-Schemas/
```

The Event Catalog remains authoritative for Event meaning.

The Data Schemas Catalog becomes authoritative for machine-readable validation and serialization.

### Phase 8 — Integration Mapping

Integration Contracts will map:

```text
External payload
  ↓
Normalized external observation
  ↓
Canonical Event
  ↓
Domain reconciliation
  ↓
Canonical outcome Event
```

Vendor-specific identifiers, retries, authentication, and Confirmations remain governed by Integration Contracts.

### Phase 9 — Contract Testing

Every approved Event requires:

- Producer contract tests.
- Consumer contract tests.
- Schema tests.
- Compatibility tests.
- Duplicate-delivery tests.
- Replay tests.
- Tenant-isolation tests.
- Security tests.
- Correction and reversal tests.

### Phase 10 — Production Readiness

The Event Catalog may support Production only when:

- Event governance is approved.
- Shared envelope is approved.
- Required Event entries are approved.
- Machine-readable Schemas exist.
- Producers are authorized.
- Consumers are registered.
- Consumer deduplication exists.
- Dead-letter handling exists.
- Replay controls exist.
- Monitoring exists.
- Security controls exist.
- Tenant-isolation tests pass.
- Compatibility and rollback plans exist.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Documentation](../README.md)
- [ASOS Canonical Domain Model](../01-Domain-Model/README.md)

---

## Current Status

This document is the approved governance baseline for the ASOS Canonical Event Catalog.

Events describe immutable past occurrences.

Events do not request future actions.

Recommendations, Human Decisions, Commands, External Confirmations, and Canonical outcomes remain separate concepts.

The Event Backbone uses at-least-once delivery.

Every Consumer must prevent duplicate business effects using `event_id`.

Retryable Commands must use `idempotency_key`.

Published Events must not be edited or silently deleted.

Corrections, reversals, cancellations, revocations, rescissions, voids, and unwinds require new linked Events.

`tenant_id` is the primary isolation boundary for Tenant-scoped Events.

Only approved Event types and versions may be published in Production.

The next controlled specification is the Shared Event Envelope.
