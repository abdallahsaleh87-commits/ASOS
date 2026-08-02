# Opportunity

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Opportunity Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-02  

---

## 1. Object Purpose

### Business Purpose

The Opportunity Object represents one active and commercially meaningful automotive sales pursuit involving a resolved Customer and an eligible Qualified Lead.

It begins when the dealership accepts responsibility for progressing a qualified commercial intent toward an approved automotive outcome.

An Opportunity may support:

- New Vehicle purchase.
- Used Vehicle purchase.
- Vehicle replacement.
- Trade-In and purchase.
- Finance-supported purchase.
- Lease.
- Fleet purchase.
- Factory order.
- Vehicle Reservation preparation.
- Another approved automotive sales journey.

The Opportunity provides a governed workspace for:

- Customer-needs discovery.
- Vehicle and solution matching.
- Sales ownership.
- Appointment and test-drive coordination.
- Quotation preparation.
- Commercial discussion.
- Negotiation tracking.
- Trade-In coordination.
- Finance Application coordination.
- Next-action planning.
- Follow-up planning.
- Pipeline forecasting.
- Risk and exception detection.
- Win and loss analysis.
- Controlled conversion to a Deal.

### Domain Separation

The following boundaries must remain explicit:

```text
Lead
  = original inquiry or expression of interest

Customer
  = canonical individual or organization

Qualified Lead
  = governed qualification outcome

Opportunity
  = active commercial pursuit,
    requirement versions, stage, ownership,
    forecast, commitment, next-action planning,
    Action Class 2 authorization context,
    and Deal-conversion coordination

Interaction and Communication
  = Customer communication content, execution,
    provider delivery, receipt, response,
    Consent enforcement, and communication evidence

Appointment
  = Appointment request, scheduling,
    external Confirmation, attendance, and outcome

Quotation
  = governed Customer-specific commercial offer

Inventory Record
  = availability, Reservation, Allocation,
    stock lifecycle, and Inventory execution

Trade-In
  = appraisal, payoff, acquisition, and intake request

Finance Application
  = finance application, lender Decision,
    offer selection, and funding readiness

Financial Contract
  = contractual lifecycle and funding workflow

Deal
  = governed commercial transaction
```

An Opportunity is more commercially mature than a Qualified Lead.

It is not:

- A signed contract.
- A completed Payment.
- A lender Decision.
- A Vehicle Reservation.
- A Vehicle Allocation.
- A sent or delivered Customer communication.
- A confirmed Appointment.
- A confirmed sale.
- A confirmed delivery.
- A finalized accounting transaction.

These outcomes belong to their appropriate Canonical Domain Objects and configured external authorities.

### Opportunity and Qualified Lead Separation

A Qualified Lead contains a time-bounded qualification outcome and commercial-intent snapshot.

An Opportunity manages the active commercial pursuit after that qualification is accepted for sales progression.

The Opportunity must preserve:

- Originating Qualified Lead.
- Originating Lead.
- Resolved Customer.
- Qualification snapshot.
- Qualification evidence references.
- Qualification policy and version.
- Conversion timestamp.
- Conversion authority.
- Conversion idempotency evidence.

Changes to Customer requirements after Opportunity creation must be stored as Opportunity requirement versions.

They must not silently rewrite the original qualification evidence.

### Opportunity and Action Execution Separation

The Opportunity Domain Service owns:

- Opportunity lifecycle.
- Sales assignment.
- Customer-requirement versions.
- Vehicle and Inventory selection projections.
- Pipeline stage.
- Forecast.
- Commitment evidence references.
- Next-action planning.
- Next-action Recommendations.
- Required Action Class.
- Action-authorization requirement.
- References to Human Approval or automation policy.
- Read-only Command, execution, and Confirmation projections.
- Deal-conversion coordination.

The Opportunity Domain Service does not own:

- Customer communication execution.
- Communication-provider delivery.
- Appointment creation or scheduling execution.
- External CRM write execution.
- Inventory Reservation or Allocation execution.
- Quotation issuance.
- Trade-In approval.
- Finance Decision.
- Financial Contract funding.
- Payment execution.
- Deal completion.
- Delivery execution.

The responsible execution service owns the Command, idempotency, provider response, External Confirmation, and reconciliation for the action it executes.

Examples:

```text
Opportunity recommends SEND_MESSAGE
  → Opportunity owns the Recommendation and authorization requirement
  → Communication or Interaction Service owns execution

Opportunity recommends SCHEDULE_APPOINTMENT
  → Opportunity owns the Recommendation and authorization requirement
  → Appointment Service owns scheduling execution

Opportunity requests CRM stage write-back
  → Opportunity owns the requested stage and local pending projection
  → Command Orchestration and CRM Connector own external execution

Opportunity selects Inventory
  → Opportunity owns the selection
  → Inventory Service owns Reservation and Allocation
```

### Action Class 2 Authority Boundary

Action Class 2 covers controlled Customer-facing or external operations such as:

- Sending an approved follow-up.
- Sending approved Vehicle options.
- Requesting or scheduling an Appointment.
- Requesting a test drive.
- Submitting an approved reversible CRM update.
- Issuing another reversible operational Command within approved limits.

Every Action Class 2 execution requires exactly one valid execution-authority path:

```text
Path A
  = Explicit Human Approval for the exact action instance

or

Path B
  = Active pre-approved automation policy
    covering the exact action instance
```

No third path exists.

The following do not create execution authority:

- AI Recommendation.
- AI confidence.
- Opportunity priority.
- Opportunity stage.
- Customer temperature.
- Forecast category.
- Task assignment.
- User-interface button visibility.
- Previous approval for a materially different action.
- Provider availability.
- A draft.
- A Command request.
- An internal workflow state.

The Opportunity Domain Service owns Recommendation and authorization context, but it does not own execution of Customer-facing or external Action Class 2 operations.

The deterministic Policy and Authorization layer must validate authority before Command creation and again before execution when material context may have changed.

### Explicit Human Approval Path

Explicit Human Approval must be bound to the exact proposed action.

The approval must identify:

- `tenant_id`.
- `opportunity_id`.
- Customer or external target.
- Action type.
- Purpose.
- Channel.
- Recipient.
- Approved content, template version, or content hash.
- Approved fields.
- Applicable limits.
- Expected execution window.
- Approving Human.
- Role and permission.
- Organizational scope.
- Approval timestamp.
- Expiration.
- Revocation state.
- Evidence.
- Opportunity record version.
- Recommendation version where applicable.

A material change requires new approval.

Material changes include:

- Different recipient.
- Different channel.
- Different purpose.
- Different template or material content.
- Different Vehicle, Inventory Record, price, or commercial claim.
- Different Appointment time or location.
- Different external fields.
- Different Opportunity stage when policy requires revalidation.
- Expired or revoked Consent.
- New complaint, restriction, legal hold, or risk.
- Expired approval.

### Pre-Approved Automation Policy Path

A pre-approved automation policy is a governed authorization instrument.

It must define at least:

- Policy identifier and version.
- Policy owner.
- Effective date.
- Expiration.
- Revocation mechanism.
- Allowed action types.
- Allowed Customer or workflow conditions.
- Allowed Opportunity stages.
- Tenant, dealership, branch, team, and role scope.
- Approved purposes.
- Approved channels.
- Approved templates and template versions.
- Permitted dynamic fields.
- Prohibited content and claims.
- Recipient-resolution rules.
- Consent or lawful-basis requirements.
- Contact restrictions.
- Frequency limits.
- Time-of-day limits.
- Quiet periods.
- Inventory-freshness requirements.
- Pricing-authority requirements.
- Appointment-capacity requirements.
- Monetary, risk, and volume limits.
- Escalation conditions.
- Monitoring requirements.
- Audit requirements.
- Emergency suspension.
- Required External Confirmation.
- Required revalidation before execution.

An automation policy must be:

- Active.
- Applicable to the exact action.
- Applicable to the exact Tenant and organizational scope.
- Current and unexpired.
- Not revoked.
- Within limits.
- Validated deterministically.

A policy identifier without a successful policy evaluation is not execution authority.

### Action Class 2 Non-Downgrade Rule

An action classified as Action Class 3 by the Constitution, architecture, Domain Model, Business Rules, or applicable law must not be downgraded to Action Class 2 by:

- AI.
- Configuration.
- Prompt.
- User request.
- Opportunity priority.
- Automation policy.
- Integration limitation.
- Operational urgency.

Examples that remain Action Class 3 include:

- Restricted price approval.
- Discount override beyond authority.
- Vehicle Allocation override.
- Trade-In approval.
- Finance Decision.
- Contractual commitment.
- Funding request.
- Deal finalization.
- Opportunity closure override.
- Reopening a won Opportunity.
- Delivery authorization.
- Material change to Customer rights or obligations.

### Authorization, Command, and Confirmation Separation

The following are distinct:

```text
Recommendation generated
  ≠ Action authorized

Action authorized
  ≠ Command created

Command created
  ≠ Command sent

Command sent
  ≠ Provider accepted

Provider accepted
  ≠ Business outcome completed

External Confirmation received
  ≠ Canonical outcome accepted until validation and reconciliation
```

Opportunity may project these states but must not collapse them into one status.

### Opportunity and Deal Separation

The Opportunity manages pursuit, discovery, solution fit, proposal, negotiation, commitment, and closure.

The Deal manages the governed transaction.

An Opportunity may become `WON` only when a valid Deal has been created through the approved conversion workflow.

`WON` means that the sales pursuit successfully converted into a governed Deal.

It does not prove:

- Contract signature.
- Payment.
- Finance approval.
- Funding.
- Vehicle sale posting.
- Vehicle registration.
- Vehicle delivery.
- Revenue recognition.

### System Purpose

The Opportunity Object provides canonical sales-pipeline context used by:

- Sales work queues.
- Sales ownership and reassignment.
- Management dashboards.
- Pipeline forecasting.
- Customer follow-up planning.
- Vehicle Recommendation workflows.
- Appointment request workflows.
- Quotation request workflows.
- Trade-In request workflows.
- Finance Application request workflows.
- Deal-conversion workflows.
- Policy and authorization services.
- Human Review services.
- Command Orchestration.
- AI Agents.
- Analytics.
- Audit and compliance controls.

The configured CRM may remain externally authoritative for some operational fields, including:

- Pipeline stage.
- External owner assignment.
- External closure status.
- External activity status.

When an external CRM is authoritative, ASOS maintains a Canonical Projection and must not represent an outbound stage or assignment Command as completed until authoritative External Confirmation is received.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Original qualification evidence | Qualified Lead |
| Customer identity | Customer |
| Opportunity lifecycle and requirement versions | Opportunity Domain Service |
| Next-action Recommendation | Opportunity or approved intelligence service |
| Action Class determination | Approved deterministic rules and architecture |
| Action Class 2 execution authorization | Authorized Human or active pre-approved automation policy |
| Communication execution and provider evidence | Interaction or Communication Service and provider |
| Appointment execution and outcome | Appointment Service and provider |
| External CRM write execution | Command Orchestration and CRM Connector |
| Vehicle identity | Vehicle |
| Inventory availability, Reservation, and Allocation | Inventory Record or configured Inventory authority |
| Customer-specific pricing | Quotation |
| Final Deal terms | Deal |
| Finance Decision | Lender or F&I platform |
| Funding workflow | Financial Contract |
| Trade-In appraisal and approval | Trade-In and configured appraisal authority |
| Forecasts, scores, and Recommendations | Derived Intelligence |
| Binding or high-impact Decision | Authorized Human |
| External action completion | Configured external authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `opportunity_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Origin Relationships

- `qualified_lead_id`.
- `originating_lead_id`.
- `customer_id`.
- `qualification_snapshot`.
- `qualification_policy_id`.
- `qualification_policy_version`.
- `qualification_evidence_references`.
- `qualified_lead_record_version`.
- `converted_from_qualified_lead_at`.
- `conversion_authority_reference`.
- `conversion_idempotency_key`.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `sales_team_id`.
- `owner_user_id`.
- `secondary_owner_user_ids`.
- `assignment_queue_id`.
- `territory_id`.
- `legal_entity_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Opportunity Identity

- `opportunity_number`.
- `name`.
- `primary_intent`.
- `stage`.
- `stage_confirmation_status`.
- `priority`.
- `status_reason`.
- `workflow_authority_mode`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Assignment

- `assignment_status`.
- `owner_user_id`.
- `sales_team_id`.
- `assignment_queue_id`.
- `assigned_at`.
- `assignment_rule_id`.
- `assignment_reason`.
- `assignment_expires_at`.
- `previous_owner_user_id`.
- `ownership_accepted_at`.
- `ownership_confirmation_status`.

### Customer Requirements

- `requirements_version`.
- `vehicle_interest_text`.
- `vehicle_preferences`.
- `required_features`.
- `preferred_features`.
- `excluded_features`.
- `preferred_vehicle_condition`.
- `preferred_make_ids`.
- `preferred_model_ids`.
- `preferred_body_types`.
- `preferred_fuel_types`.
- `preferred_transmissions`.
- `preferred_colors`.
- `required_seating_capacity`.
- `purchase_timeframe`.
- `usage_purpose`.
- `annual_usage_estimate`.
- `budget_min_amount`.
- `budget_max_amount`.
- `currency_code`.
- `payment_preference`.
- `finance_interest`.
- `trade_in_interest`.
- `appointment_interest`.
- `test_drive_interest`.
- `preferred_dealership_id`.
- `preferred_branch_id`.
- `customer_decision_criteria`.
- `known_objections`.
- `requirements_last_verified_at`.

### Vehicle and Inventory Solution Context

- `primary_vehicle_id`.
- `primary_inventory_record_id`.
- `alternative_vehicle_ids`.
- `alternative_inventory_record_ids`.
- `vehicle_selection_status`.
- `vehicle_match_score`.
- `vehicle_match_evidence`.
- `inventory_availability_status`.
- `inventory_availability_confirmed_at`.
- `inventory_availability_expires_at`.
- `inventory_projection_record_version`.
- `vehicle_selection_updated_at`.
- `vehicle_selection_updated_by`.

Vehicle selection does not create:

- Reservation.
- Allocation.
- Quotation.
- Deal.

### Appointment Context

- `primary_appointment_id`.
- `next_appointment_id`.
- `last_completed_appointment_id`.
- `appointment_request_status`.
- `appointment_confirmation_status`.
- `appointment_count`.
- `completed_appointment_count`.
- `no_show_count`.
- `last_appointment_outcome`.
- `appointment_readiness_status`.

Appointment fields are projections from the Appointment Domain Service.

### Quotation Context

- `current_quotation_id`.
- `quotation_count`.
- `active_quotation_count`.
- `last_quotation_presented_at`.
- `current_quotation_status`.
- `current_quotation_expires_at`.
- `quotation_response_status`.

Quotation projections must not replace the Quotation Object.

### Trade-In Context

- `trade_in_id`.
- `trade_in_status`.
- `trade_in_intent`.
- `trade_in_value_projection`.
- `trade_in_currency_code`.
- `trade_in_last_updated_at`.
- `trade_in_record_version`.

Trade-In values are projections from the governed Trade-In workflow.

### Finance Context

- `finance_application_id`.
- `finance_interest`.
- `payment_preference`.
- `estimated_down_payment_amount`.
- `finance_application_status`.
- `finance_decision_projection`.
- `finance_last_updated_at`.
- `finance_application_record_version`.

Sensitive lender information and authoritative finance Decisions must remain in the Finance Application or external lender system.

### Commercial Estimate

- `estimated_transaction_value_amount`.
- `currency_code`.
- `estimated_vehicle_value_amount`.
- `estimated_accessory_value_amount`.
- `estimated_service_value_amount`.
- `estimated_trade_in_credit_amount`.
- `estimated_discount_amount`.
- `estimated_finance_value_amount`.
- `estimated_gross_profit_amount`.
- `estimated_gross_margin_percentage`.
- `commercial_estimate_source`.
- `commercial_estimate_updated_at`.

These are non-binding estimates.

Final commercial terms belong to Quotation and Deal.

### Engagement and Activity

- `customer_temperature`.
- `engagement_status`.
- `last_meaningful_interaction_id`.
- `last_meaningful_contact_at`.
- `last_customer_response_at`.
- `last_outbound_attempt_at`.
- `contact_attempt_count`.
- `meaningful_interaction_count`.
- `days_since_last_meaningful_contact`.
- `next_action_type`.
- `next_action_at`.
- `next_action_status`.
- `next_action_owner_user_id`.
- `next_follow_up_at`.
- `next_review_at`.
- `activity_freshness_status`.

### Next-Action Recommendation Context

- `current_next_action_recommendation_id`.
- `next_action_recommendation_version`.
- `recommended_action_type`.
- `recommended_action_class`.
- `recommended_action_purpose`.
- `recommended_action_channel`.
- `recommended_action_template_id`.
- `recommended_action_template_version`.
- `recommended_action_content_hash`.
- `recommended_action_target_reference`.
- `recommended_action_evidence_references`.
- `recommended_action_generated_at`.
- `recommended_action_expires_at`.
- `recommended_action_status`.
- `recommended_action_requires_human_review`.

A Recommendation is not execution authority.

### Action Authorization Context

- `action_authorization_status`.
- `action_authorization_path`.
- `action_authorization_request_id`.
- `human_decision_id`.
- `human_approval_scope_hash`.
- `human_approval_expires_at`.
- `automation_policy_id`.
- `automation_policy_version`.
- `automation_policy_evaluation_id`.
- `authorization_evaluated_at`.
- `authorization_expires_at`.
- `authorization_revoked_at`.
- `authorization_failure_reasons`.
- `authorization_snapshot_hash`.

Authorization fields are governed references and projections.

The authoritative Human Decision belongs to the Human Decision service.

The authoritative automation-policy evaluation belongs to the Policy and Authorization service.

### Action Execution Projection

- `action_execution_service`.
- `action_command_id`.
- `action_idempotency_key_reference`.
- `action_execution_status`.
- `action_execution_requested_at`.
- `action_command_sent_at`.
- `action_external_confirmation_status`.
- `action_external_confirmation_reference`.
- `action_executed_at`.
- `action_reconciliation_status`.

Opportunity stores read-only projections and references.

The execution service owns the Command, raw idempotency record, provider response, Confirmation, and reconciliation workflow.

### Proposal and Negotiation

- `proposal_status`.
- `proposal_presented_at`.
- `negotiation_status`.
- `negotiation_started_at`.
- `negotiation_summary`.
- `current_customer_objections`.
- `requested_commercial_changes`.
- `approval_dependencies`.
- `pending_customer_action`.
- `pending_dealership_action`.

### Commitment

- `commitment_status`.
- `commitment_type`.
- `commitment_recorded_at`.
- `commitment_evidence_references`.
- `commitment_expiration_at`.
- `commitment_verified_by_actor_id`.
- `deal_conversion_readiness_status`.

A commitment signal does not prove:

- Payment.
- Contract.
- Reservation.
- Allocation.
- Finance approval.
- Deal completion.

### Forecast

- `close_probability`.
- `close_probability_source`.
- `forecast_category`.
- `expected_close_date`.
- `forecast_value_amount`.
- `weighted_pipeline_value_amount`.
- `forecast_period`.
- `forecast_updated_at`.
- `forecast_model_version`.
- `forecast_override_reason`.

Forecast values are Derived Intelligence or authorized Human estimates.

They are not guaranteed outcomes.

### Risk and Derived Intelligence

- `engagement_score`.
- `conversion_probability`.
- `opportunity_health_score`.
- `stagnation_risk_score`.
- `customer_response_probability`.
- `vehicle_fit_score`.
- `commercial_risk_score`.
- `finance_dependency_risk`.
- `inventory_dependency_risk`.
- `recommended_next_action`.
- `recommended_contact_channel`.
- `recommended_vehicle_ids`.
- `recommended_inventory_record_ids`.
- `recommended_escalation`.
- `recommended_forecast_category`.
- `requires_human_review`.
- `derived_intelligence_expires_at`.

Every material derived output must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Data freshness.
- Confidence where meaningful.
- Assumptions.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required authority.

### Hold Context

- `on_hold_reason`.
- `on_hold_details`.
- `on_hold_at`.
- `on_hold_until`.
- `on_hold_review_at`.
- `on_hold_authority`.
- `previous_active_stage`.

### Closure Context

- `closure_type`.
- `won_reason`.
- `loss_reason`.
- `loss_reason_details`.
- `competitor_name`.
- `cancel_reason`.
- `closed_at`.
- `closed_by_actor_type`.
- `closed_by_actor_id`.
- `closure_authority_reference`.
- `closure_evidence_references`.

### Deal Conversion

- `deal_conversion_status`.
- `converted_deal_id`.
- `deal_conversion_requested_at`.
- `deal_conversion_requested_by`.
- `deal_conversion_command_id`.
- `deal_conversion_idempotency_key`.
- `deal_conversion_completed_at`.
- `deal_conversion_failure_reason`.
- `deal_conversion_confirmation_status`.

Only one primary Deal may be created from one Opportunity.

Failed conversion attempts must remain historically traceable.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `external_crm_id`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `reconciliation_status`.
- `field_authority_map`.
- `opened_at`.
- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `stage_changed_at`.
- `stage_changed_by_actor_id`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `opportunity_id` | UUID | Yes | Opportunity Domain Service | Immutable Canonical Opportunity identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `qualified_lead_id` | UUID | Yes | Canonical relationship | Qualified Lead from which the Opportunity originated. |
| `originating_lead_id` | UUID | Yes | Canonical relationship | Original Lead associated with qualification. |
| `customer_id` | UUID | Yes | Customer relationship | Resolved Customer participating in the pursuit. |
| `opportunity_number` | String | Yes | ASOS or external CRM | Human-readable Opportunity reference. |
| `name` | String | Yes | Canonical Projection | Human-readable Opportunity title. |
| `primary_intent` | Enum | Yes | Qualified Lead or Human Decision | Primary commercial objective. |
| `stage` | Enum | Yes | Configured workflow authority | Current pipeline stage. |
| `stage_confirmation_status` | Enum | Yes | Workflow Projection | Confirmation state when an external CRM controls stage. |
| `priority` | Enum | Yes | Opportunity workflow | Current operational priority. |
| `workflow_authority_mode` | Enum | Yes | Configuration | ASOS, external CRM, or governed bidirectional authority. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine state. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Material conflict state. |
| `record_version` | Integer | Yes | Opportunity Domain Service | Optimistic-concurrency version. |

### Assignment Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `owner_user_id` | UUID | Conditional | Workflow authority | Primary User responsible for progression. |
| `sales_team_id` | UUID | No | Workflow authority | Responsible sales team. |
| `assignment_queue_id` | UUID | No | Workflow authority | Queue responsible while no individual owner exists. |
| `assignment_status` | Enum | Yes | Workflow State | Current assignment state. |
| `assigned_at` | Timestamp | No | ASOS or external CRM | Time current assignment became effective. |
| `assignment_rule_id` | String | No | Deterministic policy | Rule used for automated assignment. |
| `assignment_reason` | String | No | Human or policy | Reason supporting assignment or reassignment. |
| `ownership_confirmation_status` | Enum | Yes | Workflow Projection | External Confirmation where assignment is externally authoritative. |

Every active Opportunity must have either:

- An authorized owner; or
- An approved team or queue.

### Requirements Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `requirements_version` | Integer | Yes | Opportunity Domain Service | Current version of Customer requirements. |
| `vehicle_interest_text` | Text | Yes | Customer evidence or projection | Current normalized Vehicle requirement. |
| `vehicle_preferences` | JSON Object | No | Canonical Projection | Structured Customer preferences. |
| `required_features` | Array | No | Customer-confirmed or Human-reviewed | Mandatory solution features. |
| `preferred_features` | Array | No | Customer-confirmed or Derived | Preferred non-mandatory features. |
| `purchase_timeframe` | Enum | Yes | Customer evidence or projection | Expected purchase timeframe. |
| `usage_purpose` | Enum | No | Customer evidence or projection | Intended Vehicle use. |
| `budget_min_amount` | Decimal | No | Customer-provided or projection | Lower budget estimate. |
| `budget_max_amount` | Decimal | No | Customer-provided or projection | Upper budget estimate. |
| `currency_code` | String | Conditional | Configured authority | ISO 4217 currency code. |
| `payment_preference` | Enum | Yes | Customer evidence or projection | Expected payment route. |
| `requirements_last_verified_at` | Timestamp | No | Customer Interaction or Human review | Time requirements were last confirmed. |

### Vehicle and Inventory Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `primary_vehicle_id` | UUID | No | Opportunity selection | Preferred Vehicle identity or configuration. |
| `primary_inventory_record_id` | UUID | No | Inventory relationship | Preferred physical stock record. |
| `alternative_vehicle_ids` | Array | No | Opportunity selection | Alternative Vehicle identities or configurations. |
| `alternative_inventory_record_ids` | Array | No | Inventory relationship | Alternative physical Inventory Records. |
| `vehicle_selection_status` | Enum | Yes | Opportunity workflow | Current selection maturity. |
| `vehicle_match_score` | Decimal | No | Derived Intelligence | Estimated requirement-to-Vehicle match. |
| `inventory_availability_status` | Enum | Yes | Inventory projection | Current availability projection. |
| `inventory_availability_confirmed_at` | Timestamp | No | Inventory authority | Time availability was last confirmed. |
| `inventory_availability_expires_at` | Timestamp | No | Deterministic policy | Time availability becomes stale. |

Vehicle selection must not be represented as Reservation or Allocation.

### Engagement and Next-Action Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_temperature` | Enum | Yes | Derived or Human Estimate | Current engagement classification. |
| `engagement_status` | Enum | Yes | Workflow Projection | Current engagement condition. |
| `last_meaningful_contact_at` | Timestamp | No | Interaction Projection | Latest accepted meaningful Customer contact. |
| `last_customer_response_at` | Timestamp | No | Interaction Projection | Latest Customer response. |
| `contact_attempt_count` | Integer | Yes | Interaction Projection | Accepted outbound contact-attempt count. |
| `next_action_type` | Enum | No | Workflow or Recommendation | Current planned next action. |
| `next_action_at` | Timestamp | No | Opportunity workflow | Due time for the next action. |
| `next_action_status` | Enum | Yes | Opportunity workflow | Status of the planned next action. |
| `next_follow_up_at` | Timestamp | No | Opportunity workflow | Next permitted follow-up time. |
| `activity_freshness_status` | Enum | Yes | Deterministic calculation | Whether engagement context remains current. |

A next-action Recommendation must remain distinguishable from an approved Task, execution authorization, Command, and executed action.

### Action Recommendation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `current_next_action_recommendation_id` | UUID | No | Opportunity or Intelligence Service | Current Recommendation reference. |
| `recommended_action_type` | Enum | No | Derived Intelligence or rule | Proposed action. |
| `recommended_action_class` | Enum | Conditional | Deterministic classification | Required Action Class. |
| `recommended_action_purpose` | Enum | Conditional | Recommendation | Approved purpose candidate. |
| `recommended_action_channel` | Enum | No | Recommendation | Proposed channel. |
| `recommended_action_template_id` | String | No | Template Registry | Proposed approved template. |
| `recommended_action_template_version` | String | No | Template Registry | Exact template version. |
| `recommended_action_content_hash` | String | No | Recommendation | Hash of material proposed content. |
| `recommended_action_expires_at` | Timestamp | No | Deterministic policy | Recommendation expiration. |
| `recommended_action_status` | Enum | Yes | Opportunity workflow | Current Recommendation lifecycle projection. |

### Action Authorization Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `action_authorization_status` | Enum | Yes | Policy or Decision Projection | Current authorization result. |
| `action_authorization_path` | Enum | No | Deterministic policy | Human Approval or automation-policy path. |
| `human_decision_id` | UUID | No | Human Decision Service | Exact Human Decision reference. |
| `human_approval_scope_hash` | String | No | Human Decision Service | Integrity hash of approved action scope. |
| `automation_policy_id` | String | No | Policy Registry | Applicable automation policy. |
| `automation_policy_version` | String | No | Policy Registry | Exact policy version evaluated. |
| `automation_policy_evaluation_id` | UUID | No | Policy Engine | Exact evaluation record. |
| `authorization_expires_at` | Timestamp | No | Policy or Decision | Expiration of execution authority. |
| `authorization_revoked_at` | Timestamp | No | Policy or Decision | Time authority was revoked. |
| `authorization_snapshot_hash` | String | No | Authorization Service | Integrity hash of execution-authority context. |

### Action Execution Projection Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `action_execution_service` | String | No | Execution routing | Service responsible for execution. |
| `action_command_id` | UUID | No | Command Orchestration | Governed Command reference. |
| `action_execution_status` | Enum | Yes | Execution projection | Current execution state. |
| `action_external_confirmation_status` | Enum | Yes | External Confirmation projection | Provider or external authority outcome. |
| `action_external_confirmation_reference` | String | No | External authority | Confirmation evidence reference. |
| `action_reconciliation_status` | Enum | Yes | Execution workflow | Reconciliation state. |

Opportunity must not store provider credentials, raw provider payloads, or authoritative Command idempotency records.

### Forecast Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `close_probability` | Decimal | Yes | Derived or Human Estimate | Estimated probability of Opportunity-to-Deal conversion. |
| `close_probability_source` | Enum | Yes | Provenance | Source of probability. |
| `forecast_category` | Enum | Yes | Derived or Human Estimate | Pipeline forecast classification. |
| `expected_close_date` | Date | No | Estimate | Expected Deal-conversion date. |
| `forecast_value_amount` | Decimal | No | Deterministic projection | Opportunity value used for forecast. |
| `weighted_pipeline_value_amount` | Decimal | No | Deterministic calculation | Forecast value multiplied by close probability. |
| `forecast_override_reason` | String | No | Authorized Human | Reason for overriding forecast. |

### Commitment and Closure Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `commitment_status` | Enum | Yes | Workflow Projection | Current commitment-signal state. |
| `commitment_type` | Enum | No | Customer evidence or Human Decision | Type of commitment signal. |
| `commitment_evidence_references` | Array | No | Evidence repository | Supporting evidence. |
| `deal_conversion_readiness_status` | Enum | Yes | Deterministic workflow | Readiness for Deal conversion. |
| `closure_type` | Enum | No | Human Decision or approved policy | Won, lost, or cancelled. |
| `loss_reason` | Enum | No | Human Decision or approved policy | Standard loss reason. |
| `closed_at` | Timestamp | No | Workflow authority | Accepted closure time. |
| `converted_deal_id` | UUID | No | Deal relationship | Primary Deal created from Opportunity. |
| `deal_conversion_status` | Enum | Yes | Opportunity workflow | Current conversion state. |

---

## 4. Enumerations

### OpportunityStage

- `CREATED`
- `DISCOVERY`
- `SOLUTION_FIT`
- `PROPOSAL`
- `NEGOTIATION`
- `COMMITMENT`
- `ON_HOLD`
- `WON`
- `LOST`
- `CANCELLED`
- `ARCHIVED`

### OpportunityIntent

- `NEW_VEHICLE_PURCHASE`
- `USED_VEHICLE_PURCHASE`
- `VEHICLE_REPLACEMENT`
- `TRADE_IN_AND_PURCHASE`
- `FINANCE_SUPPORTED_PURCHASE`
- `LEASE`
- `FLEET_PURCHASE`
- `FACTORY_ORDER`
- `VEHICLE_RESERVATION_PREPARATION`
- `OTHER`

### OpportunityPriority

- `STANDARD`
- `HIGH`
- `URGENT`
- `STRATEGIC`

Priority must not override Consent, authorization, Action Class, price authority, Inventory authority, legal restrictions, or Customer protection.

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_CRM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### AssignmentStatus

- `UNASSIGNED`
- `QUEUED`
- `ASSIGNMENT_PENDING`
- `ASSIGNED`
- `ACCEPTANCE_PENDING`
- `ACCEPTED`
- `REASSIGNMENT_PENDING`
- `SUSPENDED`
- `RECONCILIATION_REQUIRED`

### OpportunityCustomerTemperature

- `UNKNOWN`
- `COLD`
- `WARM`
- `HOT`
- `IMMEDIATE`

Customer temperature is Derived Intelligence or a Human estimate.

It is not execution authority.

### EngagementStatus

- `UNKNOWN`
- `ACTIVE`
- `RESPONSIVE`
- `LOW_ENGAGEMENT`
- `UNRESPONSIVE`
- `CUSTOMER_DELAYED`
- `CONTACT_RESTRICTED`
- `STALE`
- `REVIEW_REQUIRED`

### PurchaseTimeframe

- `IMMEDIATE`
- `WITHIN_7_DAYS`
- `WITHIN_30_DAYS`
- `WITHIN_90_DAYS`
- `WITHIN_180_DAYS`
- `MORE_THAN_180_DAYS`
- `UNKNOWN`

### PaymentPreference

- `CASH`
- `FINANCE`
- `LEASE`
- `MIXED`
- `UNDECIDED`
- `UNKNOWN`

### VehicleSelectionStatus

- `NOT_STARTED`
- `REQUIREMENTS_CAPTURED`
- `MATCHING_IN_PROGRESS`
- `OPTIONS_SHORTLISTED`
- `PRIMARY_VEHICLE_SELECTED`
- `PRIMARY_INVENTORY_SELECTED`
- `AVAILABILITY_REVALIDATION_REQUIRED`
- `NO_ACCEPTABLE_MATCH`
- `CUSTOMER_RECONSIDERING`
- `BLOCKED`

### OpportunityAvailabilityStatus

- `UNKNOWN`
- `NOT_CHECKED`
- `AVAILABLE`
- `RESERVED`
- `ALLOCATED`
- `NOT_AVAILABLE`
- `STALE`
- `CONFLICTED`
- `PENDING_CONFIRMATION`

### ProposalStatus

- `NOT_STARTED`
- `PREPARATION_PENDING`
- `IN_PREPARATION`
- `APPROVAL_REQUIRED`
- `READY`
- `PRESENTED`
- `EXPIRED`
- `WITHDRAWN`
- `REJECTED`
- `REPLACED`

### NegotiationStatus

- `NOT_STARTED`
- `ACTIVE`
- `WAITING_FOR_CUSTOMER`
- `WAITING_FOR_DEALERSHIP`
- `WAITING_FOR_APPROVAL`
- `WAITING_FOR_FINANCE`
- `WAITING_FOR_TRADE_IN`
- `STALLED`
- `COMPLETED`
- `ENDED`

### CommitmentStatus

- `NOT_RECORDED`
- `POTENTIAL`
- `EVIDENCE_PENDING`
- `VERIFIED`
- `EXPIRED`
- `WITHDRAWN`
- `DISPUTED`

### CommitmentType

- `QUOTATION_ACCEPTANCE`
- `DOCUMENTED_PURCHASE_INTENT`
- `VEHICLE_SELECTION_CONFIRMED`
- `DEPOSIT_PROCESS_INITIATED`
- `FINANCE_APPLICATION_SUBMITTED`
- `DOCUMENTS_SUBMITTED`
- `OTHER`

### DealConversionReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### DealConversionStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `AWAITING_APPROVAL`
- `READY`
- `CREATION_IN_PROGRESS`
- `PENDING_EXTERNAL_CONFIRMATION`
- `DEAL_CREATED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ForecastCategory

- `EXCLUDED`
- `PIPELINE`
- `UPSIDE`
- `BEST_CASE`
- `COMMIT`
- `CLOSED_WON`
- `CLOSED_LOST`

### ForecastSource

- `DETERMINISTIC_RULE`
- `AI_DERIVED`
- `HUMAN_ESTIMATE`
- `AUTHORIZED_HUMAN_OVERRIDE`
- `EXTERNAL_CRM_PROJECTION`

### NextActionType

- `CALL_CUSTOMER`
- `SEND_MESSAGE`
- `SEND_EMAIL`
- `SEND_VEHICLE_OPTIONS`
- `REQUEST_MORE_INFORMATION`
- `REQUEST_APPOINTMENT`
- `REQUEST_TEST_DRIVE`
- `PREPARE_QUOTATION`
- `PRESENT_QUOTATION`
- `FOLLOW_UP_QUOTATION`
- `REQUEST_DOCUMENTS`
- `REVIEW_FINANCE`
- `REVIEW_TRADE_IN`
- `REVALIDATE_INVENTORY`
- `REQUEST_APPROVAL`
- `ESCALATE`
- `CREATE_DEAL`
- `PLACE_ON_HOLD`
- `CLOSE_OPPORTUNITY`
- `OTHER`

### NextActionStatus

- `NOT_SET`
- `RECOMMENDED`
- `AUTHORIZATION_REQUIRED`
- `AUTHORIZED`
- `PLANNED`
- `DUE`
- `EXECUTION_REQUESTED`
- `PENDING_EXTERNAL_CONFIRMATION`
- `COMPLETED`
- `OVERDUE`
- `CANCELLED`
- `BLOCKED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ActionClass

- `ACTION_CLASS_0_ANALYSIS_ONLY`
- `ACTION_CLASS_1_INTERNAL_REVERSIBLE`
- `ACTION_CLASS_2_CONTROLLED_EXTERNAL`
- `ACTION_CLASS_3_BINDING_HIGH_IMPACT`

### ActionAuthorizationPath

- `NONE`
- `EXPLICIT_HUMAN_APPROVAL`
- `PRE_APPROVED_AUTOMATION_POLICY`

### ActionAuthorizationStatus

- `NOT_EVALUATED`
- `NOT_REQUIRED`
- `REQUIRED`
- `EVALUATION_PENDING`
- `AUTHORIZED`
- `REJECTED`
- `EXPIRED`
- `REVOKED`
- `CONTEXT_CHANGED`
- `REVALIDATION_REQUIRED`
- `POLICY_NOT_APPLICABLE`
- `HUMAN_APPROVAL_REQUIRED`

### RecommendedActionStatus

- `NOT_AVAILABLE`
- `GENERATED`
- `PRESENTED`
- `ACCEPTED_FOR_REVIEW`
- `AUTHORIZATION_PENDING`
- `AUTHORIZED`
- `REJECTED`
- `WITHDRAWN`
- `EXPIRED`
- `SUPERSEDED`

### ActionExecutionStatus

- `NOT_REQUESTED`
- `REQUEST_READY`
- `REQUESTED`
- `COMMAND_CREATED`
- `COMMAND_VALIDATED`
- `COMMAND_QUEUED`
- `COMMAND_SENT`
- `PENDING_EXTERNAL_CONFIRMATION`
- `CONFIRMED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ActionPurpose

- `CUSTOMER_RESPONSE`
- `SALES_FOLLOW_UP`
- `VEHICLE_OPTIONS`
- `APPOINTMENT_COORDINATION`
- `TEST_DRIVE_COORDINATION`
- `QUOTATION_COORDINATION`
- `DOCUMENT_COLLECTION`
- `INVENTORY_REVALIDATION`
- `EXTERNAL_CRM_SYNCHRONIZATION`
- `OTHER_APPROVED_PURPOSE`

### ContactChannel

- `PHONE`
- `SMS`
- `MESSAGING_APP`
- `EMAIL`
- `PORTAL`
- `IN_PERSON`
- `EXTERNAL_SYSTEM`
- `OTHER`

### OnHoldReason

- `CUSTOMER_REQUEST`
- `WAITING_FOR_VEHICLE`
- `WAITING_FOR_QUOTATION`
- `WAITING_FOR_FINANCE`
- `WAITING_FOR_TRADE_IN`
- `WAITING_FOR_DOCUMENTS`
- `WAITING_FOR_DECISION_MAKER`
- `WAITING_FOR_APPROVAL`
- `SEASONAL_DELAY`
- `CONTACT_RESTRICTION`
- `DATA_CONFLICT`
- `OTHER`

### OpportunityLossReason

- `CUSTOMER_PURCHASED_FROM_COMPETITOR`
- `PRICE_NOT_ACCEPTED`
- `VEHICLE_UNAVAILABLE`
- `REQUIREMENTS_NOT_MET`
- `FINANCE_NOT_APPROVED`
- `TRADE_IN_VALUE_NOT_ACCEPTED`
- `CUSTOMER_POSTPONED`
- `CUSTOMER_NO_LONGER_INTERESTED`
- `CUSTOMER_UNREACHABLE`
- `CUSTOMER_SELECTED_DIFFERENT_CHANNEL`
- `DEALERSHIP_DECLINED`
- `QUALIFICATION_NO_LONGER_VALID`
- `DUPLICATE_OPPORTUNITY`
- `OTHER`

### ClosureType

- `WON`
- `LOST`
- `CANCELLED`

### ConfirmationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ReconciliationStatus

- `NOT_REQUIRED`
- `CURRENT`
- `PENDING`
- `IN_PROGRESS`
- `RESOLVED`
- `FAILED`
- `MANUAL_REVIEW_REQUIRED`

### FreshnessStatus

- `CURRENT`
- `APPROACHING_EXPIRY`
- `STALE`
- `UNKNOWN`

### DataQualityStatus

- `COMPLETE`
- `INCOMPLETE`
- `STALE`
- `CONFLICTED`
- `QUARANTINED`

### ConflictStatus

- `NONE`
- `POTENTIAL`
- `CONFIRMED`
- `UNDER_REVIEW`
- `RESOLVED`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from authenticated security context.
- Request bodies must not override `tenant_id`.
- All related Objects must belong to authorized Tenant scope.
- Dealership, branch, team, queue, owner, territory, and legal entity must belong to the Tenant.
- Cross-Tenant Opportunity access, conversion, reassignment, AI retrieval, action authorization, and reporting are prohibited unless an approved and auditable mechanism exists.
- Background Jobs, Event Consumers, Policy evaluations, execution services, and AI Agents must receive trusted Tenant execution context.

### Opportunity Creation Rules

An Opportunity may be created only when:

- Qualified Lead is eligible for conversion.
- Qualified Lead is not expired, revoked, invalid, or already converted.
- Qualified Lead references one resolved Customer.
- Customer belongs to the same Tenant.
- Contact restrictions are understood.
- Required qualification evidence exists.
- Organizational routing is valid.
- Conversion request is idempotent.
- No blocking conflict exists.

Opportunity creation must:

- Preserve qualification snapshot.
- Preserve originating Lead.
- Preserve Qualified Lead record version.
- Record conversion authority.
- Record conversion timestamp.
- Prevent duplicate creation from retry.
- Publish an accepted state change only after transaction success.

### Origin Integrity Rules

- `qualified_lead_id` is required and immutable.
- `originating_lead_id` is required and immutable.
- `customer_id` must initially match the Customer linked to Qualified Lead.
- Changing linked Customer requires governed identity correction.
- Qualification evidence must not be silently rewritten.
- Later requirement changes must increment `requirements_version`.

### Assignment Rules

- Every active Opportunity must have an authorized owner, team, or queue.
- Assignment must remain within authorized Tenant and organizational scope.
- Reassignment must preserve previous owner, new owner, reason, authority, effective time, and record version.
- AI may recommend assignment but must not bypass routing and authorization.
- When external CRM is authoritative, assignment remains pending until External Confirmation.
- Assignment does not grant pricing, finance, Trade-In, contract, Inventory, Deal, Payment, or delivery authority.

### Customer Requirement Rules

- Customer requirements must preserve evidence and version history.
- AI-extracted requirements remain Derived Intelligence until accepted.
- Requirements must not be inferred from protected attributes.
- Budget values must be zero or greater.
- `budget_min_amount` must not exceed `budget_max_amount`.
- Customer uncertainty must be represented as unknown.
- Material changes may require Vehicle rematching, Quotation review, forecast review, stage review, authorization invalidation, or requalification.
- Stale requirements must not support Customer-facing claims.

### Vehicle and Inventory Rules

- `primary_vehicle_id` must reference a valid Vehicle.
- `primary_inventory_record_id` must reference a physical Inventory Record associated with selected Vehicle.
- Vehicle identity and Inventory context must remain separate.
- Vehicle selection does not reserve or allocate Vehicle.
- Vehicle match score does not prove availability.
- Customer-visible availability must come from sufficiently current authoritative Inventory source.
- Stale availability must be labelled and revalidated.
- AI must not represent predicted availability as confirmed stock.

### Appointment Rules

- Opportunity may request an Appointment workflow.
- Opportunity must not create an authoritative Appointment directly.
- Appointment request does not prove Confirmation.
- Opportunity stage must not advance solely because an Appointment was requested.
- Appointment outcome must be supported by accepted Appointment evidence.
- Customer-facing Appointment request or scheduling is Action Class 2 unless stricter classification applies.
- Appointment Service owns execution, idempotency, provider Confirmation, and reconciliation.

### Quotation and Pricing Rules

- Opportunity may request or reference Quotations.
- Binding Customer-specific terms belong to Quotation and Deal.
- `PROPOSAL` normally requires an active governed Quotation workflow.
- Expired, withdrawn, or superseded Quotations must not be presented as current.
- AI Recommendations must not be represented as approved pricing.
- Restricted discounts and overrides require Authoritative Human Decision.
- Estimated commercial value must remain distinguishable from approved terms.
- Action Class 2 automation must not send unapproved or stale pricing claims.

### Finance Rules

- Finance interest does not create Finance Application.
- Finance Application submission does not prove approval.
- Lender Decision remains authoritative.
- Opportunity must not store unnecessary sensitive finance data.
- Finance projections must preserve source and freshness.
- AI must not represent finance approval as fact.
- Finance Decision and funding actions are not Action Class 2 Opportunity automation.

### Trade-In Rules

- Trade-In interest does not create approved appraisal.
- Trade-In valuation, ownership, lien, payoff, inspection, acquisition approval, and Inventory-intake request belong to Trade-In.
- Opportunity may store only necessary projections.
- Trade-In Recommendation must not be represented as approved value.
- Material Trade-In change may require Quotation, forecast, and authorization revalidation.

### Engagement and Contact Rules

- Opportunity communication must comply with Customer Consent and contact restrictions.
- `DO_NOT_CONTACT` or equivalent restriction must block prohibited outbound activity deterministically.
- Customer-requested response and marketing permission must remain distinguishable.
- Provider delivery does not prove Customer engagement.
- Contact-attempt counts must not be manipulated through retries.
- Follow-up frequency and timing must comply with approved policy.
- Contact eligibility must be checked before authorization and before execution.

### Next-Action Rules

- Recommendation is not approved or executed action.
- Next action must identify type, owner, due time, status, evidence, Action Class, and required authority.
- Completed action must reference Interaction, Appointment, Quotation, Task, Command, or workflow evidence.
- Closed Opportunities must not generate ordinary sales follow-up.
- Approved post-close activities must remain explicitly classified.
- Opportunity priority must not authorize execution.

### Action Classification Rules

- Every material proposed action must receive deterministic Action Class.
- The classification must preserve rule and version.
- AI may recommend a class but deterministic policy establishes the enforceable class.
- When classifications conflict, the stricter class applies until resolved.
- Action Class 3 must not be downgraded by configuration or automation policy.
- Unknown or ambiguous classification must block execution and require review.

### Action Class 2 Authorization Rules

Before Action Class 2 Command creation, Policy and Authorization services must validate:

- Exact action type.
- Exact action instance.
- Tenant and organizational scope.
- Target Customer or external system.
- Purpose.
- Channel.
- Recipient.
- Consent or lawful basis.
- Contact restrictions.
- Opportunity stage and state.
- Customer and requirement freshness.
- Inventory and pricing freshness where applicable.
- Template and version.
- Dynamic field values.
- Material content hash.
- Frequency and time limits.
- Risk and monetary limits.
- Human Approval or automation-policy path.
- Expiration and revocation.
- Emergency suspension.
- Required execution service.
- Required External Confirmation.
- Audit readiness.

The same controls must be revalidated immediately before execution when context may have changed.

### Human Approval Rules

For explicit Human Approval:

- Approval must be from authorized Human role.
- Approval must cover the exact action.
- Approval must be unexpired and unrevoked.
- Approval must preserve content or scope hash.
- Approval must preserve Opportunity and source record versions.
- Approval must not be reused for materially different action.
- Approval must be revalidated after material context change.
- Approval does not prove execution.

### Automation Policy Rules

For pre-approved automation:

- Policy must be active and applicable.
- Policy version must be preserved.
- Policy evaluation result must be persisted.
- Policy must cover exact action, target, purpose, channel, template, fields, stage, limits, and timing.
- Consent and restrictions must pass independently.
- Policy must be revocable and emergency-suspendable.
- Policy must not authorize Action Class 3.
- Failure or uncertainty must block execution.
- Policy does not prove execution.

### Action Execution Rules

- Opportunity Domain Service must not send external Commands directly.
- Execution request must route to approved execution service.
- Execution service must create and own Command.
- Retryable execution must use stable idempotency.
- Command must preserve authorization reference and snapshot hash.
- External state remains pending until authoritative Confirmation.
- Provider acknowledgement must not be represented as completed outcome.
- Failure, timeout, rejection, or conflict must enter reconciliation.
- Opportunity stores read-only status projection and references.

### Authorization Invalidation Rules

Authorization becomes invalid or requires revalidation when:

- Approval or policy expires.
- Approval or policy is revoked.
- Consent changes.
- Contact restriction changes.
- Customer changes.
- Recipient changes.
- Channel changes.
- Purpose changes.
- Template or material content changes.
- Vehicle or Inventory changes.
- Availability becomes stale or unavailable.
- Price or Quotation changes.
- Appointment time or location changes.
- Opportunity stage changes where relevant.
- Complaint, dispute, fraud, legal, safety, or compliance condition appears.
- Emergency suspension activates.
- Record version or material source version changes.
- Risk exceeds approved limit.

### Forecast Rules

- `close_probability` must remain between `0.00` and `1.00`.
- Forecast thresholds must remain configurable.
- Fixed AI-confidence thresholds must not be embedded in Canonical Domain Model.
- Weighted pipeline value must be deterministic.
- Forecast values must identify source.
- Human override must preserve previous value, new value, reason, actor, and timestamp.
- Forecast values must not be represented as guaranteed revenue.

### Commitment Rules

Opportunity may enter `COMMITMENT` only when:

- Documented Customer commitment signal exists.
- Evidence is current.
- Material Vehicle and Inventory dependencies are understood.
- Required approval dependencies are known.
- Commercial information is sufficiently complete.
- No blocking compliance or identity conflict exists.
- Deal-conversion readiness is evaluated.

Commitment does not prove Payment, signature, finance approval, Reservation, Allocation, sale, or delivery.

### Won Rules

Opportunity may enter `WON` only when:

- Valid primary Deal has been created.
- Deal references Opportunity and Customer.
- Controlled conversion succeeded.
- `converted_deal_id` is populated.
- Conversion is not duplicate retry.
- Required Human authority was satisfied.
- Required external workflow Confirmation was received where applicable.
- Closure timestamp and authority are recorded.

`WON` does not confirm completion of Deal.

### Lost and Cancelled Rules

`LOST` requires:

- Valid loss reason.
- Supporting evidence.
- Configured Human Decision or approved closure policy.
- Review of open commitments and pending transactions.
- Appropriate handling of related Tasks, Quotations, Appointments, Reservations, and Allocations.
- Closure timestamp.

`CANCELLED` requires:

- Valid administrative or processing reason.
- Appropriate authority.
- Related-record reconciliation.
- Preserved audit history.

Low AI score alone must not close Opportunity.

### External Workflow Authority Rules

When external CRM is authoritative:

- Stage or assignment changes must use approved Command.
- Retryable Commands must use idempotency.
- Local projection remains pending until Confirmation.
- Transport success does not equal business completion.
- Missing Confirmation triggers timeout, polling, reconciliation, and escalation.
- Conflicting values must not be silently overwritten.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return version conflict.
- Qualified Lead conversion must be idempotent.
- Deal conversion must be idempotent.
- Action execution requests must be idempotent.
- Execution services own Command idempotency.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate Opportunities, assignments, stage changes, authorization records, Commands, communications, Appointments, Quotations, conversion attempts, or Deals.

### Human Review Requirements

Human Review is required according to policy for:

- Customer identity conflict.
- Duplicate active Opportunity.
- Reopening closed Opportunity.
- Restricted pricing or discount approval.
- Material forecast override.
- Material requirement conflict.
- Trade-In value override.
- Allocation override.
- Commitment dispute.
- Opportunity-to-Deal exception.
- Action Class ambiguity.
- Action authorization conflict.
- Consent or contact restriction conflict.
- Automation-policy exception.
- Cross-Tenant or cross-authority conflict.
- Another material high-risk exception.

---

## 6. State Machine

### Opportunity Lifecycle States

```text
CREATED
DISCOVERY
SOLUTION_FIT
PROPOSAL
NEGOTIATION
COMMITMENT
ON_HOLD
WON
LOST
CANCELLED
ARCHIVED
```

### Principal Allowed Transitions

```text
CREATED → DISCOVERY
CREATED → ON_HOLD
CREATED → LOST
CREATED → CANCELLED

DISCOVERY → SOLUTION_FIT
DISCOVERY → ON_HOLD
DISCOVERY → LOST
DISCOVERY → CANCELLED

SOLUTION_FIT → DISCOVERY
SOLUTION_FIT → PROPOSAL
SOLUTION_FIT → ON_HOLD
SOLUTION_FIT → LOST
SOLUTION_FIT → CANCELLED

PROPOSAL → SOLUTION_FIT
PROPOSAL → NEGOTIATION
PROPOSAL → COMMITMENT
PROPOSAL → ON_HOLD
PROPOSAL → LOST
PROPOSAL → CANCELLED

NEGOTIATION → SOLUTION_FIT
NEGOTIATION → PROPOSAL
NEGOTIATION → COMMITMENT
NEGOTIATION → ON_HOLD
NEGOTIATION → LOST
NEGOTIATION → CANCELLED

COMMITMENT → NEGOTIATION
COMMITMENT → ON_HOLD
COMMITMENT → WON
COMMITMENT → LOST
COMMITMENT → CANCELLED

ON_HOLD → DISCOVERY
ON_HOLD → SOLUTION_FIT
ON_HOLD → PROPOSAL
ON_HOLD → NEGOTIATION
ON_HOLD → COMMITMENT
ON_HOLD → LOST
ON_HOLD → CANCELLED

WON → ARCHIVED
LOST → ARCHIVED
CANCELLED → ARCHIVED
```

Reopening a closed Opportunity uses a separate governed workflow.

### Forbidden Ordinary Transitions

```text
CREATED → PROPOSAL
CREATED → NEGOTIATION
CREATED → COMMITMENT
CREATED → WON

DISCOVERY → COMMITMENT
DISCOVERY → WON

SOLUTION_FIT → WON
PROPOSAL → WON
ON_HOLD → WON

LOST → WON
CANCELLED → WON
ARCHIVED → WON

WON → DISCOVERY
WON → NEGOTIATION

ARCHIVED → DISCOVERY
ARCHIVED → SOLUTION_FIT
```

### Entering CREATED

Requires:

- Valid Tenant.
- Eligible Qualified Lead.
- Resolved Customer.
- Qualification snapshot.
- Valid organization.
- Idempotent conversion.
- Initial audit evidence.

### Entering DISCOVERY

Requires:

- Responsible owner, team, or queue.
- Qualification context.
- Permitted Customer engagement.
- Initial next-action plan.
- No blocking identity or permission conflict.

### Entering SOLUTION_FIT

Requires:

- Current Customer requirements.
- Purchase timeframe.
- Payment preference or explicit unknown.
- Budget context or explicit unknown.
- Vehicle or solution matching.
- No blocking data-quality issue.

### Entering PROPOSAL

Requires:

- At least one commercially valid solution.
- Current Vehicle and Inventory context where applicable.
- Current pricing inputs.
- Quotation or governed proposal workflow.
- Required approval dependencies identified.

### Entering NEGOTIATION

Requires:

- Presented proposal or Quotation.
- Customer response or documented negotiation activity.
- Current commercial terms.
- Recorded objections or requested changes.
- Required approvals identified.

### Entering COMMITMENT

Requires:

- Current commitment evidence.
- Verified commitment status.
- Current Vehicle and Inventory dependencies.
- Deal-conversion readiness.
- No blocking conflict.
- Required Human authority.

### Entering ON_HOLD

Requires:

- Hold reason.
- Previous active stage.
- Review date or release condition.
- Follow-up restrictions.
- Responsible owner or team.

### Entering WON

Requires:

- Successfully created valid Deal.
- `converted_deal_id`.
- Completed idempotent conversion.
- Required authority.
- Closure evidence.
- Closure timestamp.

### Entering LOST

Requires:

- Loss reason.
- Closure evidence.
- Closure authority.
- Related-workflow review.
- Closure timestamp.

### Reopening

Reopening `WON`, `LOST`, or `CANCELLED` requires:

- Authorized Human Decision.
- Reopening reason.
- Supporting evidence.
- Reconciliation of linked Deal or closure.
- Selected restored stage.
- New record version.
- New Event.
- Audit history.

AI Agents must not independently reopen Opportunity.

### Next-Action Authorization Sub-State

The Opportunity lifecycle does not imply action authorization.

A proposed Action Class 2 operation progresses separately:

```text
RECOMMENDED
  → AUTHORIZATION_REQUIRED
  → EVALUATION_PENDING
  → AUTHORIZED
  → EXECUTION_REQUESTED
  → PENDING_EXTERNAL_CONFIRMATION
  → COMPLETED
```

Alternative outcomes:

```text
EVALUATION_PENDING → REJECTED
AUTHORIZED → EXPIRED
AUTHORIZED → REVOKED
AUTHORIZED → CONTEXT_CHANGED
EXECUTION_REQUESTED → FAILED
PENDING_EXTERNAL_CONFIRMATION → RECONCILIATION_REQUIRED
```

The Opportunity stage must not automatically advance this sub-state.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
- Evidence.
- Record version.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Human Decision where applicable.
- Automation-policy reference where applicable.
- External Confirmation where applicable.

---

## 7. Relationships

### Tenant

- Every Opportunity belongs to one `tenant_id`.
- All relationships remain in authorized Tenant scope.
- Cross-Tenant processing requires explicit auditable mechanism.

### Qualified Lead and Lead

- Every Opportunity originates from one Qualified Lead.
- One Qualified Lead creates no more than one primary Opportunity.
- Original Lead and qualification evidence remain historical.
- Opportunity updates must not rewrite intake evidence.

### Customer

- Every Opportunity references one resolved Customer.
- Customer identity and Consent remain governed by Customer and Consent authorities.
- Opportunity stores only necessary projections.
- Customer identity or Consent changes may invalidate action authorization.

### Owner, Team, and Queue

- Opportunity may have one primary owner.
- Team or queue may share responsibility.
- Assignment history remains auditable.
- Ownership does not grant unrestricted approval or execution authority.

### Vehicle and Inventory Record

- Opportunity may reference primary and alternative Vehicles and Inventory Records.
- Vehicle owns identity.
- Inventory owns availability, Reservation, Allocation, and stock execution.
- Opportunity must revalidate stale Inventory before Customer-facing claims.

### Interaction and Communication

- Opportunity may recommend or request Customer communication.
- Interaction or Communication Service owns:
  - Message or call execution.
  - Provider Command.
  - Idempotency.
  - Delivery status.
  - Customer response.
  - Interaction evidence.
  - Reconciliation.
- Opportunity stores projections and references only.
- Provider delivery does not prove Customer understanding or commitment.

### Appointment

- Opportunity may request Appointment workflow.
- Appointment Service owns scheduling, Confirmation, attendance, and outcome.
- Opportunity stores Appointment projections.
- Appointment request and Confirmation remain distinct.

### Quotation

- Opportunity may request and reference Quotations.
- Quotation owns Customer-specific terms, approval, issue, presentation, acceptance, expiration, and supersession.
- Opportunity stores projections.
- Quotation acceptance may support commitment but does not create Deal automatically.

### Trade-In

- Opportunity may reference active Trade-In.
- Trade-In owns appraisal, ownership, lien, payoff, approval, acquisition, and Inventory-intake request.
- Opportunity stores necessary projections.

### Finance Application

- Opportunity may reference Finance Applications according to policy.
- Finance Decision remains governed by lender or F&I authority.
- Opportunity must not duplicate unnecessary sensitive data.

### Financial Contract

- Opportunity may be indirectly related through Deal.
- Contract signature, effectiveness, activation, and funding do not belong to Opportunity.

### Deal

- One Opportunity may create one primary Deal.
- Failed conversion attempts remain traceable.
- Deal state must not be stored as Opportunity stage beyond necessary projection.
- Deal cancellation may trigger governed reopening review.

### Policy and Authorization Service

- Determines enforceable Action Class.
- Evaluates automation policies.
- Validates Human Approval scope.
- Produces authorization Decision or evaluation record.
- Enforces revocation and emergency suspension.
- Opportunity stores authorization projections and references.

### Command Orchestration

- Owns Commands.
- Validates execution authority.
- Enforces idempotency.
- Routes to connector.
- Tracks execution.
- Waits for External Confirmation.
- Reconciles outcomes.
- Opportunity stores read-only projections.

### Supporting Child Records

Opportunity may own or govern:

- Requirement versions.
- Assignment history.
- Stage history.
- Forecast history.
- Vehicle-match records.
- Commercial-estimate history.
- Next-action Recommendation records.
- Action-authorization request projections.
- Action-execution projections.
- Commitment evidence references.
- Hold history.
- Closure records.
- Deal-conversion attempts.
- Derived Intelligence.
- Data-quality issues.
- Reconciliation cases.
- Audit records.

Opportunity must not own authoritative communication Commands, provider delivery records, Appointment Commands, Inventory Commands, or external CRM connector credentials.

---

## 8. Domain Events

The Canonical Event Catalog is authoritative for final:

- Event names.
- Event versions.
- Event envelopes.
- Payload Schemas.
- Producers.
- Consumers.
- Compatibility.
- Correction and reversal behavior.

The following are required Event concepts and do not replace the Event Catalog.

### Creation and Assignment Event Concepts

- Qualified Lead conversion requested.
- Opportunity created.
- Opportunity creation rejected.
- Opportunity queued.
- Opportunity assigned.
- Opportunity assignment accepted.
- Opportunity reassigned.
- Assignment reconciliation required.

### Stage and Requirements Event Concepts

- Opportunity stage-change requested.
- Opportunity stage changed.
- External stage Confirmation received.
- Opportunity placed on hold.
- Opportunity released from hold.
- Opportunity reopened.
- Opportunity archived.
- Requirements captured.
- Requirements updated.
- Material requirement change detected.
- Requalification requested.

### Vehicle, Appointment, Quotation, Finance, and Trade-In Event Concepts

- Vehicle matching requested.
- Vehicle options generated.
- Primary Vehicle selected.
- Primary Inventory Record selected.
- Inventory availability revalidation requested.
- Appointment workflow requested.
- Quotation preparation requested.
- Finance Application requested.
- Trade-In workflow requested.
- Dependency blocked progression.

The responsible Domain Service publishes authoritative outcome Events.

Opportunity must not publish Appointment-confirmed, Quotation-issued, finance-approved, Trade-In-approved, Inventory-reserved, or Inventory-allocated Events unless it is the explicitly approved Producer, which is not the default boundary.

### Next-Action and Authorization Event Concepts

- Next action recommended.
- Next action Recommendation expired.
- Action classification evaluated.
- Action authorization requested.
- Human Approval referenced.
- Automation-policy evaluation requested.
- Action authorized.
- Action authorization rejected.
- Action authorization expired.
- Action authorization revoked.
- Action authorization invalidated by context change.
- Action execution requested.
- Action execution projection updated.
- Action reconciliation required.

Producer boundaries:

- Opportunity or approved Intelligence Service may publish Recommendation facts it owns.
- Policy and Authorization Service publishes authoritative policy-evaluation and authorization facts.
- Human Decision Service publishes authoritative approval or rejection facts.
- Command Orchestration publishes Command lifecycle facts.
- Communication Service publishes message sent, delivered, failed, and response facts.
- Appointment Service publishes Appointment lifecycle facts.
- Inventory Service publishes Reservation and Allocation facts.
- Opportunity must not publish authoritative execution outcomes it does not own.

### Forecast and Derived Intelligence Event Concepts

- Forecast updated.
- Opportunity health score updated.
- Stagnation risk detected.
- Escalation recommended.
- Management review requested.

Derived Intelligence Events must not imply:

- Human Approval.
- Action authorization.
- Communication execution.
- Vehicle Reservation.
- Vehicle Allocation.
- Quotation approval.
- Finance approval.
- Deal creation.
- Sale.
- Delivery.

### Closure and Deal-Conversion Event Concepts

- Deal conversion requested.
- Deal conversion validation failed.
- Deal conversion started.
- Deal created.
- Deal conversion failed.
- Deal conversion reconciliation required.
- Opportunity won.
- Opportunity lost.
- Opportunity cancelled.
- Opportunity closure corrected.

### Event Producer Rules

- Opportunity Domain Service publishes accepted Opportunity canonical and workflow-state facts.
- Qualified Lead Domain Service publishes qualification facts.
- Customer Domain Service publishes Customer identity and applicable Consent facts.
- Inventory Domain Service publishes availability, Reservation, and Allocation facts.
- Appointment Domain Service publishes Appointment facts.
- Interaction or Communication Service publishes communication execution facts.
- Quotation Domain Service publishes Quotation facts.
- Deal Domain Service publishes Deal facts.
- Policy and Authorization Service publishes authorization facts.
- Human Decision Service publishes Human Decision facts.
- Command Orchestration publishes Command lifecycle facts.
- Integration services publish normalized source observations.
- AI Agents may publish permitted Agent-run, analysis, forecast, or Recommendation Events.
- AI Agents must not publish authoritative authorization, stage, closure, pricing, Deal, or external-completion Events merely because they recommended an action.

### Event Requirements

Every material Opportunity Event must preserve where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `opportunity_id`.
- `qualified_lead_id`.
- `customer_id`.
- Dealership and branch.
- Occurrence and recording timestamps.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation and causation.
- Evidence.
- Applied policy.
- Related Recommendation.
- Related Human Decision.
- Related automation-policy evaluation.
- Related Command.
- Related External Confirmation.
- Security classification.

Events are immutable.

Corrections, reopening, cancellation, reversal, revocation, and supersession use new Events linked to affected Events.

Consumers prevent duplicate effects using preserved `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Customer-requirement extraction.
- Requirement normalization.
- Vehicle matching.
- Inventory option ranking.
- Appointment preparation.
- Quotation preparation support.
- Objection classification.
- Negotiation summarization.
- Engagement analysis.
- Forecast generation.
- Conversion-probability estimation.
- Stagnation-risk detection.
- Next-best-action Recommendation.
- Follow-up drafting.
- Owner Recommendation.
- Escalation Recommendation.
- Missing-information detection.
- Deal-conversion readiness assessment.
- Management summary.
- Data-quality detection.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Change authoritative Customer identity.
- Create general marketing Consent.
- Reverse contact restrictions.
- Confirm Vehicle availability.
- Reserve or allocate Vehicle.
- Approve Customer-visible pricing.
- Approve discounts.
- Approve Trade-In.
- Approve finance.
- Confirm lender Decision.
- Sign contracts.
- Request funding.
- Confirm Payment.
- Finalize Deal.
- Confirm sale.
- Confirm delivery.
- Mark Opportunity `WON`.
- Close material Opportunity as `LOST` solely from AI score.
- Reopen closed Opportunity.
- Authorize Action Class 2 execution.
- Create or transmit external Command.
- Access data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Opportunity identifier and version.
- Supporting evidence.
- Source authority.
- Input freshness.
- Applied Business Rules.
- Model or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Known limitations.
- Generation and expiration.
- Proposed Action Class.
- Required Human authority or automation policy.

### Next-Best Action

AI may recommend a next action.

The Recommendation must remain separate from:

- Approved Task.
- Human Decision.
- Policy evaluation.
- Execution authorization.
- Command.
- Executed Interaction.
- External Confirmation.

AI recommendation, Opportunity priority, stage, score, or User-interface state is not execution authority.

### Action Class 2

For Action Class 2, AI may:

- Draft content.
- Recommend action type.
- Recommend timing.
- Recommend channel.
- Recommend approved template.
- Explain evidence and expected impact.

AI must not:

- Select itself as approver.
- Treat its confidence as approval.
- Activate an automation policy.
- Expand policy scope.
- Bypass Consent.
- Change recipient without reauthorization.
- Modify material approved content after authorization.
- Create or send the external Command.
- Mark execution complete.

### Action Class 3

Binding or high-impact actions require Authoritative Human Decision.

Examples include:

- Restricted price approval.
- Discount override.
- Vehicle Allocation override.
- Trade-In approval.
- Finance Decision.
- Contract commitment.
- Funding request.
- Deal finalization.
- Opportunity closure override.
- Reopening won Opportunity.
- Delivery authorization.

### Human Review

Human Review is required according to policy for:

- Identity conflict.
- Duplicate Opportunity.
- Material requirement conflict.
- Restricted pricing.
- Commitment dispute.
- Forecast override.
- Customer complaint.
- Fraud or compliance risk.
- Reopening.
- Deal-conversion exception.
- Action Class ambiguity.
- Automation-policy exception.
- Material content change after approval.
- Another high-risk action.

### AI Context and Embeddings

Direct identifiers and restricted commercial information must not enter unrestricted embeddings.

Normally excluded:

- Customer name.
- Phone.
- Email.
- Address.
- National identifier.
- Raw finance information.
- Acquisition cost.
- Internal price floor.
- Maximum discount.
- Internal margin.
- Consent evidence.
- Approval evidence.
- Contract documents.
- Payment information.
- Provider credentials.
- Raw Commands.

Approved redacted context may include:

- Vehicle requirements.
- Non-sensitive preferences.
- Objection categories.
- Engagement summary.
- Negotiation summary.
- Sales-stage summary.
- Non-sensitive Appointment feedback.
- Approved Customer-facing Vehicle information.

Every vector entry must enforce:

- `tenant_id`.
- Opportunity access scope.
- Source references.
- Record versions.
- Security classification.
- Retention.
- Expiration.
- Deletion propagation.
- Customer anonymization propagation.

### Untrusted Input and Prompt Injection

Customer messages, CRM notes, uploaded documents, provider payloads, and external records are untrusted input.

Their content must not:

- Grant permission.
- Approve an action.
- Activate an automation policy.
- Override Consent or contact restrictions.
- Change Action Class.
- Trigger a Command directly.
- Modify audit evidence.
- Override system or policy instructions.

---

## 10. API Contract

Detailed API operations and Schemas will become authoritative in the API Contracts Catalog.

This section defines required Opportunity API behavior.

### REST Resources

```text
GET    /api/v1/opportunities
POST   /api/v1/opportunities
GET    /api/v1/opportunities/{opportunity_id}
PATCH  /api/v1/opportunities/{opportunity_id}

POST   /api/v1/qualified-leads/{qualified_lead_id}/opportunity-conversion-requests

POST   /api/v1/opportunities/{opportunity_id}/assignment-requests
POST   /api/v1/opportunities/{opportunity_id}/reassignment-requests
POST   /api/v1/opportunities/{opportunity_id}/stage-transition-requests
POST   /api/v1/opportunities/{opportunity_id}/requirement-versions
POST   /api/v1/opportunities/{opportunity_id}/vehicle-selection-requests
POST   /api/v1/opportunities/{opportunity_id}/inventory-revalidation-requests
POST   /api/v1/opportunities/{opportunity_id}/appointment-workflow-requests
POST   /api/v1/opportunities/{opportunity_id}/quotation-workflow-requests
POST   /api/v1/opportunities/{opportunity_id}/trade-in-workflow-requests
POST   /api/v1/opportunities/{opportunity_id}/finance-application-workflow-requests

POST   /api/v1/opportunities/{opportunity_id}/next-action-recommendations
POST   /api/v1/opportunities/{opportunity_id}/action-authorization-requests
GET    /api/v1/opportunities/{opportunity_id}/action-authorization
GET    /api/v1/opportunities/{opportunity_id}/action-execution-projection

POST   /api/v1/opportunities/{opportunity_id}/commitment-records
POST   /api/v1/opportunities/{opportunity_id}/hold-requests
POST   /api/v1/opportunities/{opportunity_id}/release-hold-requests
POST   /api/v1/opportunities/{opportunity_id}/deal-conversion-requests
POST   /api/v1/opportunities/{opportunity_id}/loss-decisions
POST   /api/v1/opportunities/{opportunity_id}/cancellation-decisions
POST   /api/v1/opportunities/{opportunity_id}/reopen-requests

GET    /api/v1/opportunities/{opportunity_id}/history
GET    /api/v1/opportunities/{opportunity_id}/forecast-history
GET    /api/v1/opportunities/{opportunity_id}/requirement-history
GET    /api/v1/opportunities/{opportunity_id}/conversion-attempts
GET    /api/v1/opportunities/{opportunity_id}/reconciliation
```

### Direct Execution Prohibition

The Opportunity API must not expose direct execution mutations such as:

```text
POST /api/v1/opportunities/{opportunity_id}/send-message
POST /api/v1/opportunities/{opportunity_id}/send-email
POST /api/v1/opportunities/{opportunity_id}/create-external-appointment
POST /api/v1/opportunities/{opportunity_id}/reserve-inventory
POST /api/v1/opportunities/{opportunity_id}/allocate-inventory
POST /api/v1/opportunities/{opportunity_id}/update-external-crm
```

Opportunity may request the relevant workflow through the responsible service boundary.

The responsible service must own execution, Command, idempotency, Confirmation, and reconciliation.

### Tenant Context

- `tenant_id` comes from authenticated security context.
- Request bodies must not override it.
- Dealership, branch, team, queue, and owner scope must be validated.
- Cross-Tenant searches are blocked by default.

### Example Next-Action Recommendation Request

```json
{
  "recommendation_type": "SEND_VEHICLE_OPTIONS",
  "proposed_action_class": "ACTION_CLASS_2_CONTROLLED_EXTERNAL",
  "purpose": "VEHICLE_OPTIONS",
  "channel": "EMAIL",
  "template_id": "vehicle-options-follow-up",
  "template_version": "3.2.0",
  "proposed_target_reference": "customer-primary-email",
  "content_hash": "sha256:32182d...",
  "expected_opportunity_record_version": 9
}
```

This creates or records a Recommendation.

It does not authorize or execute the action.

### Example Action Authorization Request

```json
{
  "recommendation_id": "9c76ea3a-3d3a-4448-aa46-9f5593b7711c",
  "action_type": "SEND_VEHICLE_OPTIONS",
  "action_class": "ACTION_CLASS_2_CONTROLLED_EXTERNAL",
  "purpose": "VEHICLE_OPTIONS",
  "channel": "EMAIL",
  "target_reference": "customer-primary-email",
  "template_id": "vehicle-options-follow-up",
  "template_version": "3.2.0",
  "content_hash": "sha256:32182d...",
  "authorization_path_requested": "PRE_APPROVED_AUTOMATION_POLICY",
  "expected_opportunity_record_version": 9
}
```

### Example Authorized Response

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "action_authorization_status": "AUTHORIZED",
  "action_authorization_path": "PRE_APPROVED_AUTOMATION_POLICY",
  "automation_policy_id": "sales-follow-up-standard",
  "automation_policy_version": "5.1.0",
  "automation_policy_evaluation_id": "384387aa-b8e9-4ea7-b2b9-476f4f596efa",
  "authorization_snapshot_hash": "sha256:6a9f62...",
  "authorization_expires_at": "2026-08-02T12:30:00Z",
  "execution_service": "communication-service",
  "record_version": 10
}
```

Authorization does not prove execution.

### Example Human Approval Response

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "action_authorization_status": "AUTHORIZED",
  "action_authorization_path": "EXPLICIT_HUMAN_APPROVAL",
  "human_decision_id": "6b39bf7d-4620-4551-a2f4-3270d968f888",
  "human_approval_scope_hash": "sha256:86c913...",
  "authorization_expires_at": "2026-08-02T13:00:00Z",
  "execution_service": "communication-service",
  "record_version": 10
}
```

### Example Execution Projection

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "action_command_id": "fc45b559-1d9c-42f4-bcde-32c4dd241cbb",
  "action_execution_service": "communication-service",
  "action_execution_status": "PENDING_EXTERNAL_CONFIRMATION",
  "action_external_confirmation_status": "PENDING",
  "record_version": 11
}
```

The authoritative Command and provider details belong to the execution service.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant and organization.
- Authorization.
- Record-version validation.
- Field authority.
- Lifecycle.
- Source and evidence.
- Customer contact restrictions.
- Freshness.
- Conflict checks.
- Required Human Decision or applicable automation policy.
- Action Class.
- Authorization-path validation.
- Audit.
- Event publication after accepted state change.
- External Confirmation tracking where applicable.

### Optimistic Concurrency

Updates must use approved mechanism such as:

```text
If-Match: <record_version>
```

Stale version returns conflict.

### Idempotency

Retryable creation, conversion, workflow requests, authorization requests, and execution requests must support:

```text
Idempotency-Key
```

Opportunity authorization-request idempotency does not replace execution-service Command idempotency.

### Error Categories

API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `QUALIFIED_LEAD_NOT_ELIGIBLE`
- `QUALIFIED_LEAD_ALREADY_CONVERTED`
- `CUSTOMER_MISMATCH`
- `DUPLICATE_OPPORTUNITY_REVIEW_REQUIRED`
- `ASSIGNMENT_REQUIRED`
- `CONTACT_RESTRICTED`
- `CONSENT_NOT_VALID`
- `ACTION_CLASS_NOT_RESOLVED`
- `ACTION_CLASS_3_HUMAN_DECISION_REQUIRED`
- `ACTION_AUTHORIZATION_REQUIRED`
- `ACTION_AUTHORIZATION_REJECTED`
- `ACTION_AUTHORIZATION_EXPIRED`
- `ACTION_AUTHORIZATION_REVOKED`
- `ACTION_CONTEXT_CHANGED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `AUTOMATION_POLICY_EXPIRED`
- `AUTOMATION_POLICY_REVOKED`
- `HUMAN_APPROVAL_REQUIRED`
- `HUMAN_APPROVAL_SCOPE_MISMATCH`
- `EXECUTION_SERVICE_REQUIRED`
- `DIRECT_EXECUTION_NOT_PERMITTED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `REQUIREMENTS_STALE`
- `INVENTORY_AVAILABILITY_STALE`
- `VEHICLE_UNAVAILABLE`
- `QUOTATION_REQUIRED`
- `COMMITMENT_EVIDENCE_REQUIRED`
- `DEAL_CONVERSION_NOT_READY`
- `INVALID_LIFECYCLE_TRANSITION`
- `OPPORTUNITY_CLOSED`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

GraphQL implementations must enforce the same:

- Tenant isolation.
- Organizational scope.
- Field authority.
- Lifecycle.
- Concurrency.
- Idempotency.
- Contact restrictions.
- Action Class.
- Human Approval.
- Automation-policy evaluation.
- Direct-execution prohibition.
- External Confirmation.
- Audit.

Resolvers must not bypass Opportunity Domain Service, Policy Engine, Human Decision Service, Command Orchestration, or responsible execution service.

---

## 11. Database Design

### Recommended Tables

```text
opportunities
opportunity_requirements
opportunity_requirement_history
opportunity_assignments
opportunity_assignment_history
opportunity_stage_history
opportunity_vehicle_matches
opportunity_vehicle_selections
opportunity_commercial_estimates
opportunity_forecasts
opportunity_engagement_metrics
opportunity_next_action_recommendations
opportunity_action_authorization_requests
opportunity_action_authorization_projections
opportunity_action_execution_projections
opportunity_commitments
opportunity_holds
opportunity_closures
opportunity_deal_conversion_attempts
opportunity_external_references
opportunity_derived_intelligence
opportunity_reconciliation_cases
opportunity_data_quality_issues
opportunity_record_versions
opportunity_audit_log
```

Opportunity tables must not replace authoritative:

```text
human_decisions
automation_policy_evaluations
commands
communication_provider_deliveries
appointment_commands
inventory_reservations
inventory_allocations
external_confirmations
```

### Opportunities Table

`opportunities` should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Qualified Lead, Lead, and Customer.
- Current assignment.
- Current stage.
- Current requirement version.
- Current Vehicle and Inventory selection.
- Current Appointment and Quotation projections.
- Current Trade-In and finance projections.
- Current commercial estimate.
- Current engagement and next action.
- Current Recommendation and authorization projection.
- Current execution projection.
- Current forecast.
- Current commitment.
- Current hold.
- Current closure and Deal conversion.
- Source and synchronization.
- Record version.
- Audit timestamps.

Historical details remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (opportunity_id)
```

### Tenant Protection

Every Opportunity-related table must include:

```text
tenant_id
```

Tenant consistency must use Tenant-aware foreign keys or equivalent controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_opportunities_tenant_stage
  (tenant_id, stage)

idx_opportunities_tenant_customer
  (tenant_id, customer_id)

idx_opportunities_tenant_qualified_lead
  (tenant_id, qualified_lead_id)

idx_opportunities_tenant_owner
  (tenant_id, owner_user_id, stage)

idx_opportunities_next_action
  (tenant_id, next_action_status, next_action_at)

idx_opportunities_action_authorization
  (tenant_id, action_authorization_status, authorization_expires_at)

idx_opportunities_action_execution
  (tenant_id, action_execution_status, action_external_confirmation_status)

idx_opportunities_expected_close
  (tenant_id, forecast_category, expected_close_date)

idx_opportunities_primary_inventory
  (tenant_id, primary_inventory_record_id)

idx_opportunities_data_quality
  (tenant_id, data_quality_status, conflict_status)

idx_opportunities_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

```text
UNIQUE (tenant_id, qualified_lead_id)
```

Recommended external-reference uniqueness:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

when source guarantees uniqueness.

```text
UNIQUE (tenant_id, converted_deal_id)
```

when `converted_deal_id` is populated and model requires one primary Opportunity per Deal.

### Recommendation Storage

`opportunity_next_action_recommendations` should preserve:

- Recommendation identifier.
- Tenant.
- Opportunity.
- Opportunity record version.
- Action type and proposed Action Class.
- Purpose, channel, target, template, and content hash.
- Evidence.
- Model, rule, or Human source.
- Generated time.
- Expiration.
- Status.
- Related Events.

### Authorization Request Storage

`opportunity_action_authorization_requests` should preserve:

- Request identifier.
- Tenant.
- Opportunity.
- Recommendation.
- Exact action scope.
- Action Class.
- Requested authorization path.
- Opportunity and source versions.
- Scope hash.
- Requesting actor.
- Status.
- Created and completed times.
- Related Events.

### Authorization Projection Storage

`opportunity_action_authorization_projections` should preserve references to:

- Human Decision or policy evaluation.
- Exact policy and version.
- Approval or authorization scope hash.
- Evaluated time.
- Expiration.
- Revocation.
- Failure reasons.
- Authorization snapshot hash.
- Authoritative source version.

It must not duplicate unrestricted Human Approval evidence or policy internals.

### Execution Projection Storage

`opportunity_action_execution_projections` should preserve:

- Opportunity.
- Authorization reference.
- Responsible execution service.
- Command reference.
- Execution status.
- Confirmation status and reference.
- Reconciliation status.
- Observed timestamps.
- Source record version.

It must not become authoritative Command or provider-delivery storage.

### Stage and Requirement History

Stage and requirement history must preserve previous and new values, authority, reason, evidence, actor, record version, and Event.

### Deal Conversion Attempts

`opportunity_deal_conversion_attempts` must preserve:

- Attempt identifier.
- Tenant.
- Opportunity and version.
- Idempotency key.
- Requesting actor.
- Validation.
- Human Decision.
- Created Deal.
- Command.
- External Confirmation.
- Failure reason.
- Reconciliation.
- Times.
- Related Events.

### Derived Intelligence

Derived records remain separate from authoritative workflow and authorization fields.

Each preserves output type, model or rule version, prompt version, input versions, evidence, confidence, assumptions, generated time, expiration, and review status.

### Audit Storage

Audit records must be append-only or equivalently protected.

Secure hashes should replace raw sensitive values when full retention is unnecessary.

### Hard Deletion

Opportunity must not be hard-deleted when referenced by:

- Qualified Lead.
- Customer journey.
- Appointment.
- Quotation.
- Trade-In.
- Finance Application.
- Deal.
- Interaction.
- Task.
- Human Decision.
- Recommendation.
- Policy evaluation.
- AI Agent Run.
- Command.
- External Confirmation.
- Audit evidence.

Closure, archival, anonymization, or governed redaction must be used.

---

## 12. Security

### Security Classification

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER_REFERENCE` | Customer and Lead references |
| `COMMERCIAL_REQUIREMENTS` | Vehicle needs, budget, timeframe |
| `COMMERCIAL_CONFIDENTIAL` | Forecast, margin estimate, negotiation |
| `CUSTOMER_RESTRICTED` | Objections, restrictions, preferences |
| `FINANCIAL_RESTRICTED` | Finance projections, down-payment estimate |
| `INTERNAL_PRICING_RESTRICTED` | Discount assumptions, profit, boundaries |
| `ACTION_AUTHORIZATION_RESTRICTED` | Human Decision and policy references |
| `COMMAND_REFERENCE_RESTRICTED` | Command and Confirmation references |
| `DERIVED_INTELLIGENCE` | Scores, forecasts, Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Events, Confirmations |

### Authentication and Authorization

Every Opportunity operation requires authenticated Human or service identity.

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Team.
- Queue.
- Owner.
- Role.
- Requested field and action.
- Opportunity stage.
- Data classification.
- Value threshold.
- Related Customer.
- Related Inventory.
- Action Class.
- Authorization path.
- Business purpose.
- Delegated authority.

### Role Boundaries

#### Sales Consultant

May perform permitted discovery, requirement updates, Vehicle matching, Appointment workflow requests, Quotation requests, follow-up planning, and negotiation documentation.

Must not independently:

- Override Consent.
- Approve restricted discounts.
- Approve finance or Trade-In.
- Allocate Inventory outside authority.
- Finalize Deal.
- Mark external actions confirmed.
- Reopen closed Opportunity.
- Authorize an action outside delegated authority.
- Execute external Commands directly.

#### Sales Manager

May perform configured reassignment, escalation, forecast override, duplicate review, closure review, reopening review, commercial approval, and exact Action Class 2 Human Approval where authorized.

Manager access does not automatically authorize finance, legal, compliance, Payment, contract, cross-Tenant access, or delivery.

#### Policy Administrator

May manage approved automation policies only within authorized governance process.

Policy administration must be separated from uncontrolled execution where required.

#### AI Agent

May access minimum Opportunity context for approved task.

AI access must be Tenant-scoped, purpose-limited, field-restricted, logged, time-limited where appropriate, and prevented from unauthorized pricing, finance, identity, Consent, approval, policy, and Command access.

### Consent and Communication Enforcement

Before outbound Customer communication, deterministic controls must validate:

- Contact purpose.
- Channel.
- Consent or permitted basis.
- Restrictions.
- Opportunity stage.
- Frequency.
- Time.
- Approved template.
- Dynamic fields.
- Content hash.
- Human Approval or applicable policy.
- Expiration and revocation.
- Emergency suspension.

Prompt text, AI Recommendation, User-interface state, stage, priority, or previous execution must not override these controls.

### Approval and Policy Protection

Human Approval and automation-policy records must use:

- Strong authorization.
- Immutable Decision or evaluation evidence.
- Versioning.
- Integrity hashes.
- Expiration.
- Revocation.
- Audit.
- Least privilege.
- Separation of duties where required.

Approval references must not be forgeable through request payloads.

Policy IDs supplied by clients must be treated as requests for evaluation, not proof of authority.

### Tenant Isolation

Tenant isolation applies to:

- Databases.
- Search.
- Duplicate detection.
- Forecasting.
- Vector retrieval.
- Authorization evaluation.
- Events.
- Queues.
- Caches.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query, Policy evaluation, Command, and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Commands must include:

- Authenticated service identity.
- Tenant and organization.
- Opportunity and record version.
- Exact action.
- Action Class.
- Authorization reference.
- Authorization snapshot hash.
- Current source versions.
- Idempotency key.
- Audit evidence.
- Confirmation requirement.

Opportunity Domain Service and AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Opportunity activity must record:

- `tenant_id`.
- `opportunity_id`.
- Qualified Lead and Customer.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous and new value or secure hash.
- Source.
- Authority category.
- Record version.
- Applied Business Rules.
- AI involvement.
- Recommendation.
- Action Class.
- Human Decision.
- Automation policy and evaluation.
- Authorization snapshot hash.
- Command.
- Idempotency reference.
- External Confirmation.
- Evidence.
- Timestamp.
- Correlation and causation.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Opportunity access.
- Unauthorized reassignment.
- Unauthorized stage change.
- Unauthorized pricing or forecast access.
- Consent-bypass attempt.
- Action Class downgrade attempt.
- Forged Human Approval reference.
- Forged policy identifier.
- Expired or revoked authorization use.
- Content changed after approval.
- Recipient changed after approval.
- Direct execution through Opportunity API.
- Duplicate execution request.
- Command replay.
- External Confirmation mismatch.
- AI execution attempt.
- Unauthorized reopening.
- Closure manipulation.
- Audit-log tampering.
- Suspicious bulk export.

### Pipeline and Transaction Integrity

The platform must detect or prevent:

- Duplicate active Opportunities used to inflate pipeline.
- Unsupported forecast overrides.
- False stage progression.
- `WON` without valid Deal.
- Artificial expected-close manipulation.
- Unauthorized loss-reason change.
- Removal of unfavorable Opportunities.
- Conversion attribution manipulation.
- Customer communication without valid authority.
- Automation policy used outside scope.
- Action Class 3 treated as Action Class 2.
- Execution marked complete without Confirmation.
- Opportunity projection treated as authoritative execution state.

### Emergency Controls

Platform must support immediate Tenant-scoped suspension of:

- Automated follow-up.
- Action Class 2 automation policies.
- Assignment automation.
- Stage write-back.
- Appointment workflow requests.
- Quotation requests.
- Deal conversion.
- External CRM Commands.
- AI forecasting.
- AI Recommendations.
- Opportunity export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Lead](./Lead.md)
- [ASOS Qualified Lead](./QualifiedLead.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Deal](./Deal.md)
- [ASOS Interaction](./Interaction.md)
- [ASOS Canonical Event Catalog Governance](../02-Event-Catalog/README.md)

---

## Current Status

This document is the approved Canonical Opportunity baseline.

Opportunity remains the governed active commercial pursuit between Qualified Lead and Deal.

The Opportunity Domain Service owns next-action Recommendation, planning, Action Class, and authorization context.

The Opportunity Domain Service does not own execution of Customer-facing or external Action Class 2 operations.

Every Action Class 2 execution requires exactly one valid authority path:

- Explicit Human Approval for the exact action instance; or
- An active pre-approved automation policy covering the exact action instance.

AI recommendation, Opportunity priority, stage, score, or User-interface state is not execution authority.

The responsible execution service owns Command creation, idempotency, external execution, Confirmation, and reconciliation.

The Opportunity API must not expose direct Customer communication, external Appointment creation, Inventory Reservation, Inventory Allocation, or external CRM execution mutations.

Action Class 3 must not be downgraded to Action Class 2.

Vehicle selection does not create Reservation or Allocation.

Commitment does not prove Payment, contract, sale, or delivery.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
