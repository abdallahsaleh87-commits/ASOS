# Opportunity

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Opportunity Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

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
- Vehicle reservation preparation.
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
- Next-action management.
- Follow-up management.
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
  = active commercial pursuit

Quotation
  = governed Customer-specific commercial offer

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

### Opportunity and Deal Separation

The Opportunity manages pursuit, discovery, solution fit, proposal, negotiation, commitment, and closure.

The Deal manages the governed transaction.

An Opportunity may become `WON` only when a valid Deal has been created through the approved conversion workflow.

`WON` means that the sales pursuit successfully converted into a governed Deal.

It does not prove:

- Contract signature.
- Payment.
- Finance approval.
- Vehicle sale posting.
- Vehicle registration.
- Vehicle delivery.
- Revenue recognition.

### System Purpose

The Opportunity Object provides the canonical sales-pipeline context used by:

- Sales work queues.
- Sales ownership and reassignment.
- Management dashboards.
- Pipeline forecasting.
- Customer follow-up.
- Vehicle Recommendation workflows.
- Appointment workflows.
- Quotation workflows.
- Trade-In workflows.
- Finance Application workflows.
- Deal-conversion workflows.
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
| Opportunity canonical meaning | Opportunity Domain Service |
| ASOS-native workflow state | ASOS |
| External CRM stage where configured | External CRM |
| Vehicle identity | Vehicle |
| Vehicle availability and Reservation | Inventory Record or configured Inventory authority |
| Customer-specific pricing | Quotation |
| Final Deal terms | Deal |
| Finance Decision | Lender or F&I platform |
| Trade-In appraisal and approval | Trade-In and configured appraisal authority |
| Interaction-delivery evidence | Communication provider |
| Forecasts, scores, and Recommendations | Derived Intelligence |
| Sales Decision or override | Authorized Human |
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

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `sales_team_id`.
- `owner_user_id`.
- `secondary_owner_user_ids`.
- `assignment_queue_id`.
- `territory_id`.

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
- `appointment_count`.
- `completed_appointment_count`.
- `no_show_count`.
- `last_appointment_outcome`.
- `appointment_readiness_status`.

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

Trade-In values are projections from the governed Trade-In workflow.

### Finance Context

- `finance_application_id`.
- `finance_interest`.
- `payment_preference`.
- `estimated_down_payment_amount`.
- `finance_application_status`.
- `finance_decision_projection`.
- `finance_last_updated_at`.

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
| `opportunity_id` | UUID | Yes | ASOS | Immutable Canonical Opportunity identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `qualified_lead_id` | UUID | Yes | Canonical relationship | Qualified Lead from which the Opportunity originated. |
| `originating_lead_id` | UUID | Yes | Canonical relationship | Original Lead associated with the qualification. |
| `customer_id` | UUID | Yes | Canonical relationship | Resolved Customer participating in the pursuit. |
| `opportunity_number` | String | Yes | ASOS or external CRM | Human-readable Opportunity reference. |
| `name` | String | Yes | Canonical Projection | Human-readable Opportunity title. |
| `primary_intent` | Enum | Yes | Qualified Lead or Human Decision | Primary commercial objective. |
| `stage` | Enum | Yes | Configured workflow authority | Current pipeline stage. |
| `stage_confirmation_status` | Enum | Yes | Workflow Projection | Confirmation state when an external CRM controls stage. |
| `priority` | Enum | Yes | Workflow State | Current operational priority. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Identifies whether ASOS or an external CRM controls workflow state. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine state. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Material conflict state. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Assignment Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `owner_user_id` | UUID | Conditional | Workflow authority | Primary User responsible for progression. |
| `sales_team_id` | UUID | No | Workflow authority | Responsible sales team. |
| `assignment_queue_id` | UUID | No | Workflow authority | Queue responsible while no individual owner exists. |
| `assignment_status` | Enum | Yes | Workflow State | Current assignment state. |
| `assigned_at` | Timestamp | No | ASOS or external CRM | Time the current assignment became effective. |
| `assignment_rule_id` | String | No | Deterministic policy | Rule used for automated assignment. |
| `assignment_reason` | String | No | Human or policy | Reason supporting assignment or reassignment. |
| `ownership_confirmation_status` | Enum | Yes | Workflow Projection | External Confirmation status where assignment is externally authoritative. |

Every active Opportunity must have either:

- An authorized owner; or
- An approved queue or team responsible for the Opportunity.

### Requirements Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `requirements_version` | Integer | Yes | ASOS | Current version of Customer requirements. |
| `vehicle_interest_text` | Text | Yes | Customer evidence or Canonical Projection | Current normalized Vehicle requirement. |
| `vehicle_preferences` | JSON Object | No | Canonical Projection | Structured Customer preferences. |
| `required_features` | Array | No | Customer-confirmed or Human-reviewed | Features required for acceptable solution fit. |
| `preferred_features` | Array | No | Customer-confirmed or Derived | Features preferred but not mandatory. |
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
| `vehicle_selection_status` | Enum | Yes | Workflow State | Current selection maturity. |
| `vehicle_match_score` | Decimal | No | Derived Intelligence | Estimated match between requirements and selected Vehicle. |
| `inventory_availability_status` | Enum | Yes | Inventory projection | Current availability projection. |
| `inventory_availability_confirmed_at` | Timestamp | No | External Confirmation | Time availability was last authoritatively confirmed. |
| `inventory_availability_expires_at` | Timestamp | No | Deterministic policy | Time the availability projection becomes stale. |

Vehicle selection must not be represented as Reservation or Allocation.

### Commercial Estimate Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `estimated_transaction_value_amount` | Decimal | No | Estimate | Expected non-binding transaction value. |
| `estimated_discount_amount` | Decimal | No | Estimate | Expected discount assumption, not approval. |
| `estimated_trade_in_credit_amount` | Decimal | No | Trade-In projection | Estimated Trade-In contribution. |
| `estimated_down_payment_amount` | Decimal | No | Customer-provided or estimate | Expected down-payment amount. |
| `estimated_gross_profit_amount` | Decimal | No | Derived Intelligence | Estimated internal gross profit. |
| `estimated_gross_margin_percentage` | Decimal | No | Derived Intelligence | Estimated gross-margin percentage. |
| `commercial_estimate_source` | Enum | No | Provenance | Source of the current estimate. |
| `commercial_estimate_updated_at` | Timestamp | No | ASOS | Time the estimate was generated or accepted. |

Commercial estimates must not be presented as approved Customer terms.

### Engagement and Next-Action Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_temperature` | Enum | Yes | Derived or Human Estimate | Current engagement classification. |
| `engagement_status` | Enum | Yes | Workflow Projection | Current engagement condition. |
| `last_meaningful_contact_at` | Timestamp | No | Interaction Projection | Latest accepted meaningful Customer contact. |
| `last_customer_response_at` | Timestamp | No | Interaction Projection | Latest Customer response. |
| `contact_attempt_count` | Integer | Yes | Interaction Projection | Accepted outbound contact-attempt count. |
| `next_action_type` | Enum | No | Workflow or Recommendation | Current next action. |
| `next_action_at` | Timestamp | No | Workflow State | Due time for the next action. |
| `next_action_status` | Enum | Yes | Workflow State | Status of the next action. |
| `next_follow_up_at` | Timestamp | No | Workflow State | Next permitted follow-up time. |
| `activity_freshness_status` | Enum | Yes | Deterministic calculation | Whether engagement context remains current. |

A recommended next action must remain distinguishable from an approved Task or executed action.

### Forecast Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `close_probability` | Decimal | Yes | Derived or Human Estimate | Estimated probability of Opportunity-to-Deal conversion. |
| `close_probability_source` | Enum | Yes | Provenance | Source of the probability. |
| `forecast_category` | Enum | Yes | Derived or Human Estimate | Pipeline forecast classification. |
| `expected_close_date` | Date | No | Estimate | Expected Deal-conversion date. |
| `forecast_value_amount` | Decimal | No | Deterministic projection | Opportunity value used for forecast. |
| `weighted_pipeline_value_amount` | Decimal | No | Deterministic calculation | Forecast value multiplied by close probability. |
| `forecast_override_reason` | String | No | Authorized Human | Reason for overriding a calculated forecast. |

### Commitment and Closure Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `commitment_status` | Enum | Yes | Workflow Projection | Current commitment-signal state. |
| `commitment_type` | Enum | No | Customer evidence or Human Decision | Type of documented commitment signal. |
| `commitment_evidence_references` | Array | No | Evidence repository | Evidence supporting the commitment signal. |
| `deal_conversion_readiness_status` | Enum | Yes | Deterministic workflow | Readiness for Deal conversion. |
| `closure_type` | Enum | No | Human Decision or approved policy | Won, lost, cancelled, or another approved closure. |
| `loss_reason` | Enum | No | Human Decision or approved policy | Standard reason for losing the pursuit. |
| `loss_reason_details` | Text | No | Human input | Additional loss evidence. |
| `competitor_name` | String | No | Human or extracted evidence | Competitor associated with the loss where known. |
| `closed_at` | Timestamp | No | Workflow authority | Accepted closure timestamp. |
| `converted_deal_id` | UUID | No | Deal relationship | Primary Deal created from the Opportunity. |
| `deal_conversion_status` | Enum | Yes | Workflow State | Current conversion state. |

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

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_CRM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

Bidirectional authority requires field-level ownership and conflict policies.

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

It is not an authoritative Customer attribute.

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

### VehicleUsagePurpose

- `PERSONAL`
- `FAMILY`
- `BUSINESS`
- `FLEET`
- `COMMERCIAL_TRANSPORT`
- `RIDE_HAILING`
- `LEISURE`
- `OTHER`
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

A commitment signal does not prove Payment, contract, sale, or delivery.

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

### CommercialEstimateSource

- `QUOTATION_PROJECTION`
- `INVENTORY_PRICING_PROJECTION`
- `HUMAN_ESTIMATE`
- `DETERMINISTIC_CALCULATION`
- `AI_DERIVED`
- `EXTERNAL_CRM_PROJECTION`

### NextActionType

- `CALL_CUSTOMER`
- `SEND_MESSAGE`
- `SEND_EMAIL`
- `SEND_VEHICLE_OPTIONS`
- `REQUEST_MORE_INFORMATION`
- `SCHEDULE_APPOINTMENT`
- `SCHEDULE_TEST_DRIVE`
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
- `PLANNED`
- `DUE`
- `IN_PROGRESS`
- `COMPLETED`
- `OVERDUE`
- `CANCELLED`
- `BLOCKED`
- `EXPIRED`

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

### OpportunityCancelReason

- `DUPLICATE_CREATED_IN_ERROR`
- `INVALID_CONVERSION`
- `CUSTOMER_IDENTITY_CORRECTION`
- `ADMINISTRATIVE_CORRECTION`
- `PROCESSING_RESTRICTED`
- `TRANSFERRED_TO_ANOTHER_WORKFLOW`
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

### SynchronizationStatus

- `NOT_SYNCED`
- `PENDING`
- `SYNCED`
- `FAILED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- All related Objects must belong to the authorized Tenant scope.
- Dealership, branch, team, queue, owner, and territory must belong to the Tenant.
- Cross-Tenant Opportunity access, conversion, reassignment, AI retrieval, and reporting are prohibited unless an approved and auditable mechanism exists.
- Background Jobs, Event Consumers, and AI Agents must receive trusted Tenant execution context.

### Opportunity Creation Rules

An Opportunity may be created only when:

- The Qualified Lead is eligible for conversion.
- The Qualified Lead is not expired, revoked, invalid, or already converted.
- The Qualified Lead references one resolved Customer.
- The Customer belongs to the same Tenant.
- Contact restrictions are understood.
- Required qualification evidence exists.
- Required organizational routing is valid.
- The conversion request is idempotent.
- No blocking conflict exists.

Opportunity creation must:

- Preserve the qualification snapshot.
- Preserve the originating Lead.
- Preserve the Qualified Lead record version.
- Record the conversion authority.
- Record the conversion timestamp.
- Prevent duplicate creation from retry.
- Publish an accepted state change only after the transaction succeeds.

### Origin Integrity Rules

- `qualified_lead_id` is required and immutable.
- `originating_lead_id` is required and immutable.
- `customer_id` must initially match the Customer linked to the Qualified Lead.
- Changing the linked Customer requires a governed identity-correction workflow.
- Qualification evidence must not be silently rewritten.
- Later requirement changes must increment `requirements_version`.

### Assignment Rules

- Every active Opportunity must have an authorized owner, team, or queue.
- Assignment must remain within authorized Tenant and organizational scope.
- Assignment and reassignment must preserve:
  - Previous owner.
  - New owner.
  - Reason.
  - Authority.
  - Effective timestamp.
  - Record version.
- AI may recommend assignment but must not bypass deterministic routing and authorization.
- When an external CRM is authoritative, the assignment remains pending until External Confirmation.
- Assignment does not grant authority to approve pricing, finance, Trade-In, contract, or delivery.

### Duplicate Opportunity Rules

Only one primary active Opportunity should normally exist for the same:

- Customer.
- Materially identical purchase intent.
- Overlapping timeframe.
- Same organizational scope.

A possible duplicate must not be merged automatically based only on an AI score.

Creating a justified parallel Opportunity requires:

- Valid business reason.
- Distinct commercial intent or buying process.
- Authorized Human Decision where required.
- Preserved relationship between the Opportunities.
- Audit evidence.

A duplicate Opportunity closure must not delete its history.

### Customer Requirement Rules

- Customer requirements must preserve evidence and version history.
- AI-extracted requirements remain Derived Intelligence until accepted by policy or Human Review.
- Customer requirements must not be inferred from protected attributes.
- Budget values must be zero or greater.
- `budget_min_amount` must not exceed `budget_max_amount`.
- Customer uncertainty must be represented as unknown rather than invented.
- Material requirement changes may require:
  - Vehicle rematching.
  - Quotation review.
  - Forecast review.
  - Stage review.
  - Requalification.
- Stale requirements must not support binding Customer claims.

### Vehicle and Inventory Rules

- `primary_vehicle_id` must reference a valid Vehicle.
- `primary_inventory_record_id` must reference a physical Inventory Record associated with the selected Vehicle.
- Vehicle identity and Inventory context must remain separate.
- Vehicle selection does not reserve or allocate the Vehicle.
- Vehicle match score does not prove availability.
- Customer-visible availability must come from a sufficiently current authoritative Inventory source.
- Stale availability must be labelled and revalidated.
- A Vehicle that becomes unavailable must trigger:
  - Revalidation.
  - Alternative matching.
  - Quotation review.
  - Customer communication review.
- AI must not represent predicted availability as confirmed stock.

### Appointment Rules

- An Appointment request does not prove Appointment Confirmation.
- Opportunity stage must not advance solely because an Appointment was requested.
- Appointment outcomes must be supported by accepted Appointment or Interaction evidence.
- A completed Appointment may update engagement and solution-fit context.
- Appointment cancellation or no-show must not automatically close the Opportunity without applicable policy.

### Quotation and Pricing Rules

- An Opportunity may request or reference Quotations.
- Binding Customer-specific terms belong to Quotation and Deal.
- `PROPOSAL` normally requires an active approved or governed Quotation workflow.
- `NEGOTIATION` requires:
  - Presented commercial terms; or
  - Documented negotiation context.
- Expired, withdrawn, or superseded Quotations must not be presented as current.
- AI Recommendations must not be represented as approved pricing.
- Restricted discounts and pricing overrides require the configured Authoritative Human Decision.
- Estimated commercial value must remain distinguishable from approved terms.

### Finance Rules

- Finance interest does not create a Finance Application.
- Finance Application submission does not prove approval.
- Lender Decision remains authoritative in the lender or F&I platform.
- Opportunity must not store unnecessary sensitive finance data.
- Finance projections must preserve source and freshness.
- A declined finance outcome must not automatically close the Opportunity when another lawful commercial path remains possible.
- AI must not predict or represent finance approval as fact.

### Trade-In Rules

- Trade-In interest does not create an approved appraisal.
- Trade-In valuation, ownership, lien, payoff, inspection, and acquisition approval belong to Trade-In.
- Opportunity may store only necessary projections.
- Trade-In Recommendation must not be represented as an approved acquisition value.
- A material Trade-In change may require Quotation and forecast revalidation.

### Engagement and Contact Rules

- Opportunity communication must comply with Customer Consent and contact restrictions.
- `DO_NOT_CONTACT` must block prohibited outbound activity through deterministic controls.
- Customer-requested response and marketing permission must remain distinguishable.
- Automated acknowledgment and meaningful two-way response must remain distinguishable.
- Provider delivery does not prove Customer engagement.
- `last_meaningful_contact_at` must reference accepted Interaction evidence where possible.
- Contact-attempt counts must not be manipulated through retry duplication.
- Follow-up frequency and timing must comply with approved policy.

### Next-Action Rules

- A Recommendation is not an approved or executed action.
- A next action must identify:
  - Type.
  - Owner.
  - Due time where applicable.
  - Status.
  - Evidence or reason.
- Completed actions should reference supporting Interaction, Appointment, Quotation, Task, or workflow evidence.
- Closed Opportunities must not generate ordinary sales follow-up.
- Approved post-close activities must remain explicitly classified.

### Forecast Rules

- `close_probability` must remain between `0.00` and `1.00`.
- Forecast thresholds must remain configurable.
- Fixed AI-confidence thresholds must not be embedded in this Canonical Domain Model.
- `weighted_pipeline_value_amount` must be calculated deterministically from the approved forecast value and probability.
- Forecast values must identify their source.
- A Human override must preserve:
  - Previous value.
  - New value.
  - Reason.
  - Actor.
  - Timestamp.
- Forecast values must not be represented as guaranteed revenue.
- Closed Opportunities must map to the appropriate closed forecast category.

### Commitment Rules

An Opportunity may enter `COMMITMENT` only when:

- A documented Customer commitment signal exists.
- The commitment evidence is current.
- Material Vehicle and Inventory dependencies are understood.
- Required approval dependencies are known.
- Required commercial information is sufficiently complete.
- No blocking compliance or identity conflict exists.
- Deal-conversion readiness is evaluated.

Commitment does not prove:

- Payment.
- Contract signature.
- Finance approval.
- Vehicle Reservation.
- Vehicle Allocation.
- Sale.
- Delivery.

### Won Rules

An Opportunity may enter `WON` only when:

- A valid primary Deal has been created.
- The Deal references the Opportunity and Customer.
- The controlled conversion transaction succeeded.
- `converted_deal_id` is populated.
- The conversion is not a duplicate retry.
- Required Human authority was satisfied.
- Required external workflow Confirmation was received where applicable.
- `closed_at` and closure authority are recorded.

`WON` does not confirm completion of the resulting Deal.

### Lost Rules

An Opportunity may enter `LOST` only when:

- A valid loss reason exists.
- Supporting evidence is recorded.
- The configured Human Decision or approved closure policy applies.
- Open Customer commitments and pending transactions are reviewed.
- Required Tasks, Quotations, Appointments, Reservations, or Allocations are handled appropriately.
- `closed_at` is recorded.

`CUSTOMER_UNREACHABLE` must use the approved contact-attempt and timing policy.

A low AI score alone must not close an Opportunity as lost.

### Cancelled Rules

`CANCELLED` is used for controlled administrative or processing closure rather than commercial loss.

It requires:

- Valid cancellation reason.
- Appropriate authority.
- Related-record reconciliation.
- Preserved audit history.

### Hold Rules

An Opportunity placed `ON_HOLD` must include:

- Hold reason.
- Previous active stage.
- Review date or end condition.
- Permitted follow-up behaviour.
- Authority.
- Customer communication requirements where applicable.

An Opportunity must not remain indefinitely on hold without review.

### External Workflow Authority Rules

When an external CRM is authoritative:

- ASOS stage or assignment changes must use an approved Command.
- Retryable Commands must use `idempotency_key`.
- The local projection remains pending until Confirmation.
- Transport success does not equal business completion.
- A missing Confirmation must trigger:
  - Timeout handling.
  - Polling.
  - Reconciliation.
  - Human escalation where required.
- Conflicting CRM and ASOS values must not be silently overwritten.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Qualified Lead conversion must be idempotent.
- Deal conversion must be idempotent.
- Retryable Commands must use an approved `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Opportunities.
  - Assignments.
  - Stage changes.
  - Tasks.
  - Appointments.
  - Quotations.
  - Deal-conversion attempts.
  - Deals.

### Human Review Requirements

Human Review is required according to configured policy for:

- Customer identity conflict.
- Duplicate active Opportunity.
- Reopening a closed Opportunity.
- Restricted pricing or discount approval.
- Material forecast override.
- Material requirement conflict.
- Trade-In value override.
- Allocation override.
- Commitment dispute.
- Opportunity-to-Deal exception.
- Cross-Tenant or cross-authority conflict.
- Another material high-risk exception.

---

## 6. State Machine

### Allowed States

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

Reopening a closed Opportunity uses a separate governed reopening workflow.

It is not an ordinary direct state transition.

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

- Valid Tenant context.
- Eligible Qualified Lead.
- Resolved Customer.
- Qualification snapshot.
- Valid organizational context.
- Idempotent conversion.
- Initial audit evidence.

### Entering DISCOVERY

Requires:

- Responsible owner, team, or queue.
- Available qualification context.
- Permitted Customer engagement.
- Initial next action.
- No blocking identity or permission conflict.

### Entering SOLUTION_FIT

Requires:

- Current Customer requirements.
- Purchase timeframe.
- Payment preference or explicit unknown state.
- Budget context or explicit unknown state.
- Vehicle or solution-matching workflow.
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
- Deal-conversion readiness assessment.
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
- Completed idempotent conversion transaction.
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

### Entering CANCELLED

Requires:

- Valid administrative or processing reason.
- Authorized Decision or policy.
- Related-record reconciliation.
- Closure timestamp.

### Reopening

Reopening `WON`, `LOST`, or `CANCELLED` requires:

- Authorized Human Decision.
- Reopening reason.
- Supporting evidence.
- Reconciliation of the linked Deal or closure outcome.
- Selected restored stage.
- New record version.
- New Event.
- Audit history.

AI Agents must not independently reopen an Opportunity.

### Terminal States

For ordinary sales progression:

- `WON`
- `LOST`
- `CANCELLED`
- `ARCHIVED`

`ARCHIVED` is terminal.

### Transition Evidence

Every stage transition must preserve:

- Previous stage.
- New stage.
- Transition reason.
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

- Every Opportunity belongs to exactly one `tenant_id`.
- All related Objects must remain inside authorized Tenant scope.
- Cross-Tenant processing requires an explicit and auditable mechanism.

### Qualified Lead

- Every Opportunity originates from one Qualified Lead.
- One Qualified Lead may create no more than one primary Opportunity.
- Reopening must use the existing Opportunity rather than creating a duplicate from the same qualification.
- Original qualification evidence must remain traceable.

### Lead

- The originating Lead remains immutable historical intake evidence.
- Opportunity updates must not rewrite original Lead content.
- Lead attribution may contribute to Opportunity and Deal analytics.

### Customer

- Every Opportunity references one resolved Customer.
- One Customer may have multiple legitimate Opportunities over time.
- Parallel active Opportunities require distinguishable commercial intent or approved exception.
- Customer identity remains governed by Customer Domain Service.

### Owner, Team, and Queue

- An Opportunity may have one primary owner.
- It may also reference a team or queue.
- Assignment history must remain auditable.
- Ownership does not grant unrestricted approval authority.

### Vehicle

- Opportunity may reference one primary Vehicle and multiple alternatives.
- Vehicle identity remains governed by Vehicle.
- A selected Vehicle does not prove physical availability.

### Inventory Record

- Opportunity may reference one primary Inventory Record and multiple alternatives.
- Current availability, Reservation, Allocation, and location belong to Inventory Record.
- Opportunity must revalidate stale Inventory context before Customer-facing claims.

### Appointment

- Opportunity may have multiple Appointments.
- Appointment Confirmation and outcome remain governed by Appointment.
- Appointment completion may influence stage and engagement but does not automatically change them.

### Quotation

- Opportunity may have multiple Quotations.
- One current Quotation may be projected.
- Final Customer-specific terms remain governed by Quotation or Deal.
- Quotation acceptance may support commitment but does not create a Deal automatically.

### Trade-In

- Opportunity may reference one active Trade-In workflow.
- Trade-In owns appraisal, ownership, lien, payoff, and acquisition approval.
- Opportunity stores only required projections.

### Finance Application

- Opportunity may reference one or more Finance Applications according to policy.
- Finance Decision remains governed by lender or F&I authority.
- Opportunity must not duplicate unnecessary sensitive finance data.

### Financial Contract

- Opportunity may be indirectly related through the resulting Deal.
- Contract signature and activation do not belong to Opportunity.

### Interaction

- Interactions provide:
  - Customer responses.
  - Meaningful-contact evidence.
  - Objections.
  - Requirements updates.
  - Negotiation evidence.
- Provider delivery evidence remains authoritative for transport status.

### Deal

- One Opportunity may create one primary Deal.
- Failed Deal-conversion attempts must remain historically traceable.
- Deal state must not be stored as Opportunity stage beyond necessary projections.
- Deal cancellation may trigger a governed Opportunity reopening review.

### Market Intelligence

Market Intelligence may support:

- Vehicle alternatives.
- Demand context.
- Competitor context.
- Price context.
- Forecast risk.
- Commercial Recommendations.

Market evidence must not silently alter authoritative Opportunity state or Customer terms.

### Tasks and Human Review

Opportunity may create or reference:

- Follow-up Tasks.
- Approval Tasks.
- Human Review Tasks.
- Data-quality Tasks.
- Reconciliation Tasks.
- Escalations.

Task completion must not automatically imply completion of the related business outcome.

### Supporting Child Records

Opportunity may own or govern:

- Requirement versions.
- Assignment history.
- Stage history.
- Forecast history.
- Vehicle-match records.
- Commercial-estimate history.
- Commitment evidence references.
- Hold history.
- Closure records.
- Deal-conversion attempts.
- Derived Intelligence.
- Data-quality issues.
- Reconciliation cases.
- Audit records.

---

## 8. Domain Events

The Canonical Event Catalog is the authoritative source for final:

- Event names.
- Event versions.
- Event envelopes.
- Payload Schemas.
- Producers.
- Consumers.
- Compatibility rules.
- Correction and reversal behaviour.

The following are required Opportunity Event concepts and do not replace the Event Catalog.

### Creation and Assignment Event Concepts

- Qualified Lead conversion requested.
- Opportunity created.
- Opportunity creation rejected.
- Opportunity queued.
- Opportunity assigned.
- Opportunity assignment accepted.
- Opportunity reassigned.
- Opportunity assignment reconciliation required.

### Stage Event Concepts

- Opportunity stage-change requested.
- Opportunity stage changed.
- Opportunity stage Confirmation received.
- Opportunity stage change rejected.
- Opportunity placed on hold.
- Opportunity released from hold.
- Opportunity reopened.
- Opportunity archived.

### Requirements Event Concepts

- Opportunity requirements captured.
- Opportunity requirements updated.
- Material requirement change detected.
- Opportunity requalification requested.
- Opportunity data conflict detected.
- Opportunity data conflict resolved.

### Vehicle and Inventory Event Concepts

- Vehicle matching requested.
- Vehicle options generated.
- Primary Vehicle selected.
- Primary Inventory Record selected.
- Inventory availability revalidation requested.
- Inventory became unavailable.
- Alternative Vehicle review requested.

### Appointment Event Concepts

- Appointment requested from Opportunity.
- Appointment confirmed.
- Appointment completed.
- Appointment cancelled.
- Appointment no-show recorded.

### Quotation and Negotiation Event Concepts

- Quotation preparation requested.
- Quotation presented.
- Quotation expired.
- Negotiation started.
- Negotiation updated.
- Approval requested.
- Commitment evidence recorded.
- Commitment expired.
- Commitment withdrawn.

### Finance and Trade-In Event Concepts

- Finance Application requested.
- Finance status projection updated.
- Trade-In workflow requested.
- Trade-In projection updated.
- Finance or Trade-In dependency blocked progression.

### Forecast and Derived Intelligence Event Concepts

- Opportunity forecast updated.
- Opportunity health score updated.
- Opportunity stagnation risk detected.
- Opportunity next action recommended.
- Opportunity escalation recommended.
- Opportunity management review requested.

Derived Intelligence Events must not imply:

- Human approval.
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

### Producer Rules

- Opportunity Domain Service publishes accepted Opportunity canonical and workflow-state changes.
- Qualified Lead Domain Service publishes accepted qualification and conversion-eligibility facts.
- Customer Domain Service publishes accepted Customer identity changes.
- Inventory Domain Service publishes accepted availability, Reservation, and Allocation facts.
- Quotation Domain Service publishes accepted Quotation facts.
- Deal Domain Service publishes accepted Deal facts.
- Integration services may publish source-observation Events.
- AI Agents may publish Agent-run, analysis, forecast, or Recommendation Events.
- AI Agents must not publish authoritative stage, closure, pricing, Deal, or external-completion Events merely because they recommended the action.

### Event Requirements

Every material Opportunity Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `opportunity_id`.
- `qualified_lead_id`.
- `customer_id`.
- Dealership and branch context.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Evidence references.
- Applied policy.
- Related Recommendation.
- Related Human Decision.
- Related Command.
- Related External Confirmation.
- Security classification.

Events are immutable.

Corrections, reopening, cancellation, and reversal must use new Events linked to the original Event.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

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
- Management-summary generation.
- Data-quality issue detection.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Change authoritative Customer identity.
- Create general marketing Consent.
- Reverse contact restrictions.
- Confirm Vehicle availability.
- Reserve or allocate a Vehicle outside approved authority.
- Approve Customer-visible pricing.
- Approve discounts.
- Approve a Trade-In.
- Approve finance.
- Confirm lender Decision.
- Sign contracts.
- Confirm Payment.
- Finalize a Deal.
- Confirm sale.
- Confirm delivery.
- Mark an Opportunity `WON`.
- Close a material Opportunity as `LOST` solely from AI scoring.
- Reopen a closed Opportunity.
- Change restricted forecast or commercial values without authority.
- Execute external Commands directly.
- Access Opportunity data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Opportunity identifier and record version.
- Supporting evidence.
- Source authority.
- Input freshness.
- Applied Business Rules.
- Model or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Known limitations.
- Generated timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority or automation policy.

### Requirement Extraction

AI-extracted Customer requirements must distinguish:

- Direct Customer statement.
- Human-confirmed information.
- External authoritative fact.
- AI inference.
- Missing information.
- Conflict.

AI confidence alone must not create authoritative:

- Budget.
- Payment ability.
- Customer identity.
- Consent.
- Vehicle availability.
- Commercial commitment.

### Vehicle Matching

AI may rank Vehicle and Inventory options.

Matching must distinguish:

- Vehicle technical fit.
- Inventory availability.
- Commercial affordability.
- Customer preference.
- Data freshness.
- Mandatory requirement violations.

A high Vehicle-match score must not override:

- Required features.
- Budget boundary.
- Safety issue.
- Inventory block.
- Availability.
- Customer exclusion.
- Legal or compliance restriction.

### Forecasting

AI forecasts must not be represented as guaranteed outcomes.

Forecasts must explain:

- Primary evidence.
- Data freshness.
- Stage.
- Engagement.
- Inventory dependency.
- Quotation status.
- Finance dependency.
- Trade-In dependency.
- Missing information.
- Important risks.

Forecasts must not use protected attributes as inappropriate commercial proxies.

### Next-Best Action

AI may recommend a next action.

The Recommendation must remain separate from:

- Approved Task.
- Human Decision.
- Command.
- Executed Interaction.
- External Confirmation.

A recommended Customer communication may proceed only through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

### Action Class 2

Controlled outbound communication may use an approved automation policy only when the deterministic Policy Engine validates:

- Tenant scope.
- Customer Consent.
- Purpose.
- Channel.
- Template.
- Frequency.
- Time restrictions.
- Opportunity stage.
- Contact-path validity.
- Inventory freshness.
- Pricing authority.
- Revocation state.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision.

Examples include:

- Restricted price approval.
- Discount override.
- Vehicle Allocation.
- Trade-In approval.
- Finance Decision.
- Contract commitment.
- Deal finalization.
- Opportunity closure override.
- Reopening a won Opportunity.
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
- Another high-risk action.

### AI Context and Embeddings

Direct identifiers and restricted commercial information must not enter unrestricted embeddings.

Normally excluded fields include:

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
- Identity documents.
- Contract documents.
- Payment information.

Approved redacted or abstracted context may include:

- Vehicle requirements.
- Non-sensitive preferences.
- Objection categories.
- Engagement summary.
- Negotiation summary.
- Sales-stage summary.
- Non-sensitive appointment feedback.
- Approved Customer-facing Vehicle information.

Every vector entry must enforce:

- `tenant_id`.
- Opportunity access scope.
- Source references.
- Security classification.
- Retention.
- Expiration.
- Deletion propagation.
- Customer anonymization propagation.

### Explainability

Material Opportunity Recommendations must explain:

- Evidence used.
- Source authority.
- Data freshness.
- Current stage.
- Material conflicts.
- Missing information.
- Assumptions.
- Confidence where meaningful.
- Expected commercial impact.
- Important risks.
- Required Human authority.
- External Confirmation requirement.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Opportunity API behaviour.

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
POST   /api/v1/opportunities/{opportunity_id}/appointment-requests
POST   /api/v1/opportunities/{opportunity_id}/quotation-requests
POST   /api/v1/opportunities/{opportunity_id}/trade-in-requests
POST   /api/v1/opportunities/{opportunity_id}/finance-application-requests
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

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, queue, and owner scope must be validated.
- Cross-Tenant searches must be blocked by default.

### Example Qualified Lead Conversion Request

```json
{
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "sales_team_id": "f4e7be20-26d1-4df1-a17a-04c6d4a58419",
  "requested_owner_user_id": "0f63b9de-97fd-42f6-a53d-531155afdf56",
  "expected_qualified_lead_record_version": 6,
  "initial_next_action": {
    "type": "CALL_CUSTOMER",
    "due_at": "2026-08-02T09:00:00Z"
  }
}
```

The request must include an HTTP header such as:

```text
Idempotency-Key: 81e7b9ad-0c08-45a2-82ab-2aa86450e740
```

### Example Opportunity Response

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "opportunity_number": "OPP-2026-000842",
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "originating_lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "stage": "CREATED",
  "assignment_status": "ASSIGNED",
  "owner_user_id": "0f63b9de-97fd-42f6-a53d-531155afdf56",
  "primary_intent": "NEW_VEHICLE_PURCHASE",
  "vehicle_selection_status": "NOT_STARTED",
  "deal_conversion_status": "NOT_STARTED",
  "data_quality_status": "COMPLETE",
  "record_version": 1,
  "opened_at": "2026-08-01T18:30:00Z"
}
```

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Source and evidence requirements.
- Customer contact restrictions.
- Freshness requirements.
- Conflict checks.
- Required Human Decision or applicable automation policy.
- Audit recording.
- Event publication after accepted state change.
- External Confirmation tracking where applicable.

### Optimistic Concurrency

Updates must use an approved mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response.

### Idempotency

Retryable creation, conversion, and external-write operations must support:

```text
Idempotency-Key
```

The same idempotency key and request intent must not create duplicate:

- Opportunities.
- Assignment changes.
- Stage transitions.
- Appointment requests.
- Quotation requests.
- Finance Application requests.
- Trade-In requests.
- Deal-conversion attempts.
- Deals.
- Customer communications.

### Stage Transition Response

When ASOS is authoritative, an accepted transition may return:

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "previous_stage": "DISCOVERY",
  "stage": "SOLUTION_FIT",
  "stage_confirmation_status": "NOT_REQUIRED",
  "record_version": 4
}
```

When an external CRM is authoritative, the response may return:

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "requested_stage": "SOLUTION_FIT",
  "stage_confirmation_status": "PENDING",
  "command_id": "f9d71acd-690f-4d74-9046-a11f9291ffbb",
  "record_version": 4
}
```

The API must not describe the external stage change as confirmed until authoritative Confirmation is received.

### Deal Conversion Response

A successful canonical conversion may return:

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "deal_conversion_status": "DEAL_CREATED",
  "converted_deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "stage": "WON",
  "record_version": 15
}
```

A pending external workflow may return:

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "deal_conversion_status": "PENDING_EXTERNAL_CONFIRMATION",
  "converted_deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "command_id": "82b98d89-dd4b-41cf-8479-e671178b3d8d",
  "record_version": 15
}
```

### Error Categories

The API must distinguish at least:

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
- `REQUIREMENTS_STALE`
- `INVENTORY_AVAILABILITY_STALE`
- `VEHICLE_UNAVAILABLE`
- `QUOTATION_REQUIRED`
- `COMMITMENT_EVIDENCE_REQUIRED`
- `DEAL_CONVERSION_NOT_READY`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `OPPORTUNITY_CLOSED`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Field authority.
- Lifecycle validation.
- Concurrency.
- Idempotency.
- Customer contact restrictions.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Opportunity Domain Service or deterministic policy controls.

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

### Opportunities Table

The `opportunities` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Qualified Lead, Lead, and Customer relationships.
- Current assignment projection.
- Current stage.
- Current requirement version.
- Current Vehicle and Inventory selection.
- Current Appointment and Quotation projections.
- Current Trade-In and finance projections.
- Current commercial estimate.
- Current engagement and next action.
- Current forecast.
- Current commitment projection.
- Current hold state.
- Current closure and Deal-conversion state.
- Source and synchronization status.
- Record version.
- Audit timestamps.

Historical details must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (opportunity_id)
```

### Tenant Protection

Every Opportunity-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

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

idx_opportunities_tenant_team
  (tenant_id, sales_team_id, stage)

idx_opportunities_tenant_dealership_branch
  (tenant_id, dealership_id, branch_id)

idx_opportunities_next_action
  (tenant_id, next_action_status, next_action_at)

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

One Qualified Lead must not create multiple primary Opportunities:

```text
UNIQUE (tenant_id, qualified_lead_id)
```

Recommended external-reference uniqueness:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the external source guarantees uniqueness.

One primary Deal must not be linked to multiple Opportunities unless an explicitly approved model supports that case:

```text
UNIQUE (tenant_id, converted_deal_id)
```

when `converted_deal_id` is populated.

### Duplicate Detection

Potential duplicate active Opportunities should be detected using policy and evidence including:

- Customer.
- Intent.
- Vehicle requirements.
- Timeframe.
- Dealership.
- Branch.
- Qualified Lead.
- Current stage.
- Existing Deal.

A hard database uniqueness constraint on Customer alone must not be used because a Customer may have multiple legitimate commercial pursuits.

### Requirement History

`opportunity_requirement_history` should preserve:

- Requirement-version identifier.
- `tenant_id`.
- `opportunity_id`.
- Version.
- Full structured requirement snapshot.
- Changed fields.
- Change reason.
- Evidence.
- Actor.
- Source.
- Timestamp.
- Related Event.

Requirement history must not be silently overwritten.

### Stage History

`opportunity_stage_history` should preserve:

- Stage-history identifier.
- `tenant_id`.
- `opportunity_id`.
- Previous stage.
- Requested stage.
- Confirmed stage.
- Authority mode.
- Decision authority.
- Command.
- External Confirmation.
- Reason.
- Timestamp.
- Record version.
- Related Event.

### Assignment History

`opportunity_assignment_history` should preserve:

- Assignment identifier.
- Previous owner.
- New owner.
- Team.
- Queue.
- Rule.
- Reason.
- Authority.
- Effective time.
- Confirmation status.
- Record version.
- Related Event.

### Forecast History

`opportunity_forecasts` should preserve:

- Forecast identifier.
- Opportunity record version.
- Forecast category.
- Close probability.
- Expected close date.
- Forecast value.
- Weighted value.
- Source.
- Model or rule version.
- Prompt version where applicable.
- Evidence.
- Confidence.
- Human override.
- Generation time.
- Expiration time.

### Commitment Records

`opportunity_commitments` should preserve:

- Commitment identifier.
- Commitment type.
- Status.
- Evidence.
- Recorded time.
- Verified time.
- Actor.
- Authority.
- Expiration.
- Withdrawal or dispute reason.
- Related Events.

### Deal Conversion Attempts

`opportunity_deal_conversion_attempts` should preserve:

- Conversion-attempt identifier.
- `tenant_id`.
- `opportunity_id`.
- Opportunity record version.
- Idempotency key.
- Requested by.
- Validation result.
- Human Decision.
- Created Deal identifier.
- Command identifier.
- External Confirmation.
- Failure reason.
- Reconciliation state.
- Started time.
- Completed time.
- Related Events.

The same idempotency key and intent must not create multiple Deals.

### Derived Intelligence

Derived Opportunity records must remain separate from authoritative workflow fields.

Each derived record should preserve:

- Output type.
- Value.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Confidence.
- Assumptions.
- Generated time.
- Expiration time.
- Review status.

### Audit Storage

Opportunity audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw sensitive values where full retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Creation time.
- Closure time.
- Retention class.

Partitioning must not weaken:

- Tenant isolation.
- Referential integrity.
- Stage history.
- Forecast history.
- Audit integrity.

### Hard Deletion

An Opportunity must not be hard-deleted when referenced by:

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
- AI Agent Run.
- Command.
- External Confirmation.
- Audit evidence.

Cancellation, closure, archival, anonymization, or governed redaction must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER_REFERENCE` | Customer and Lead references |
| `COMMERCIAL_REQUIREMENTS` | Vehicle needs, budget, purchase timeframe |
| `COMMERCIAL_CONFIDENTIAL` | Forecast value, margin estimate, negotiation context |
| `CUSTOMER_RESTRICTED` | Objections, contact restrictions, personal preferences |
| `FINANCIAL_RESTRICTED` | Finance projections and down-payment estimate |
| `INTERNAL_PRICING_RESTRICTED` | Estimated discount, profit, internal boundaries |
| `DERIVED_INTELLIGENCE` | Scores, forecasts, Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Events, Confirmations, stage history |

### Authentication

Every Opportunity operation requires an authenticated Human or service identity.

Anonymous access to internal Opportunity records is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Team.
- Queue.
- Owner.
- Role.
- Requested field.
- Requested action.
- Opportunity stage.
- Data classification.
- Value threshold.
- Related Customer.
- Related Inventory Record.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access Opportunities:

- Assigned to the User.
- Assigned to an approved team or queue.
- Explicitly shared under policy.

May perform permitted:

- Discovery.
- Requirement updates.
- Vehicle matching.
- Appointment coordination.
- Quotation requests.
- Follow-up.
- Negotiation documentation.

Must not independently:

- Override Consent.
- Approve restricted discounts.
- Approve finance.
- Approve Trade-In acquisition.
- Allocate Inventory outside authority.
- Finalize a Deal.
- Mark external actions confirmed.
- Reopen a closed Opportunity.

#### Sales Manager

May access Opportunities within approved organizational scope.

May perform configured:

- Reassignment.
- Escalation.
- Forecast override.
- Duplicate Opportunity approval.
- Closure review.
- Reopening review.
- Commercial approval.

Manager access does not automatically authorize:

- Finance approval.
- Legal override.
- Compliance override.
- Payment Confirmation.
- Contract signature.
- Cross-Tenant access.
- Delivery Confirmation.

#### BDC User

May access permitted:

- Assignment queues.
- Contact and qualification context.
- Appointment coordination.
- Follow-up workflows.

Restricted commercial and financial fields may remain unavailable.

#### Finance Specialist

May access finance-related Opportunity context required for an approved Finance Application.

Finance access must not expose unrelated Customer or negotiation data.

#### Trade-In Specialist

May access Vehicle and Customer context required for an approved Trade-In workflow.

#### Inventory User

May access Vehicle-selection and Inventory dependencies required for availability, Reservation, Allocation, and transfer workflows.

#### Data Steward

May review:

- Data conflicts.
- Duplicate Opportunities.
- Relationship inconsistencies.
- Source provenance.
- Reconciliation cases.

#### Compliance or Legal Reviewer

May access restricted evidence required for an assigned case.

#### AI Agent

May access only the minimum Opportunity context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unauthorized pricing, finance, identity, and Consent access.

### Field-Level Protection

Restricted fields should use:

- Field-level authorization.
- Masking.
- Encryption where appropriate.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Examples include:

- Budget.
- Down-payment estimate.
- Internal margin.
- Estimated gross profit.
- Discount assumptions.
- Finance projections.
- Customer objections.
- Competitor details.
- Commitment evidence.

### Consent and Communication Enforcement

Before any outbound Customer communication, deterministic controls must validate:

- Contact purpose.
- Contact channel.
- Customer Consent or permitted basis.
- Customer restrictions.
- Opportunity stage.
- Contact frequency.
- Permitted time.
- Approved template.
- Human Approval or applicable automation policy.

Prompt text, AI Recommendation, User interface state, or Opportunity priority must not override these controls.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Search.
- Duplicate detection.
- Forecasting.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Opportunity Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Requested action.
- Current record version.
- Field-level write authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Opportunity activity must record:

- `tenant_id`.
- `opportunity_id`.
- Qualified Lead and Customer references.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
- Applied Business Rules.
- AI involvement.
- Recommendation.
- Human Decision.
- Automation-policy reference.
- Command.
- Idempotency key.
- External Confirmation.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Opportunity access attempts.
- Unauthorized reassignment.
- Unauthorized stage changes.
- Unauthorized pricing or forecast access.
- Restricted margin export.
- Consent-bypass attempts.
- Duplicate Deal-conversion requests.
- Artificial pipeline inflation.
- Unauthorized reopening.
- Closure manipulation.
- AI retrieval outside approved scope.
- Command replay.
- External Confirmation mismatch.
- Audit-log tampering.
- Suspicious bulk Opportunity export.

### Pipeline Integrity

The platform must detect or prevent:

- Duplicate active Opportunities used to inflate pipeline.
- Unsupported forecast overrides.
- False stage progression.
- Opportunities marked `WON` without valid Deal conversion.
- Artificial expected-close-date manipulation.
- Unauthorized loss-reason modification.
- Removal of unfavorable Opportunities from reporting.
- Conversion attribution manipulation.

### Privacy and Retention

Opportunity retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Contractual requirements.
- Related Deal and finance obligations.
- Legal holds.
- Audit requirements.

Privacy actions must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Backups according to policy.

Required non-personal commercial and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Automated follow-up.
- Assignment automation.
- Stage write-back.
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

---

## Current Status

This document is the approved Canonical Opportunity baseline.

Opportunity remains the governed active commercial pursuit between Qualified Lead and Deal.

Vehicle selection does not create Reservation or Allocation.

Commitment does not prove Payment, contract, sale, or delivery.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
