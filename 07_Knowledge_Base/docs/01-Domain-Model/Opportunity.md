# Opportunity

## 1. Object Purpose

### Business Purpose

The Opportunity object represents an active and commercially viable automotive sales pursuit involving a resolved Customer and a validated Qualified Lead.

It begins when the dealership formally accepts responsibility for progressing the Customer toward a vehicle purchase, finance arrangement, lease, reservation, fleet order, or another approved automotive sales outcome.

The Opportunity object provides the dealership with a controlled pipeline record for:

- Sales-stage management.
- Vehicle matching.
- Customer-needs discovery.
- Appointment and test-drive coordination.
- Quotation preparation.
- Negotiation tracking.
- Forecasting.
- Sales ownership.
- Win and loss analysis.
- Deal creation.

An Opportunity is more commercially mature than a Qualified Lead but does not represent a finalized transaction. Contractual, payment, delivery, finance-approval, and final Deal information must remain within their dedicated objects.

### System Purpose

The Opportunity object is the central sales-pipeline aggregate connecting the Customer, Qualified Lead, Vehicle, Quotation, Appointment, Trade-In, Finance Application, and eventual Deal.

It provides the canonical state used by:

- Sales pipeline dashboards.
- Sales Consultant work queues.
- Revenue forecasting.
- Vehicle recommendation Agents.
- Follow-up Agents.
- Sales Manager escalation workflows.
- Quotation-generation workflows.
- Negotiation support.
- Opportunity-to-Deal conversion.

The Opportunity stores the current commercial context while preserving historical changes through immutable activity, stage-history, and audit records.

Only one controlled conversion transaction may create the primary Deal from an Opportunity.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `opportunity_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `qualified_lead_id` (UUIDv4 — required)
  - `customer_id` (UUIDv4 — required)
  - `owner_id` (UUIDv4 — required)
  - `primary_vehicle_id` (UUIDv4 — optional)
  - `current_quotation_id` (UUIDv4 — optional)
  - `primary_appointment_id` (UUIDv4 — optional)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — optional)
  - `converted_deal_id` (UUIDv4 — optional)
  - `sales_team_id` (UUIDv4 — optional)

### Commercial Payload

- **Opportunity Fields:**
  - `name`
  - `stage`
  - `priority`
  - `primary_intent`
  - `purchase_timeframe`
  - `expected_close_date`
  - `close_probability`
  - `close_confidence_source`

- **Vehicle Fields:**
  - `vehicle_interest_text`
  - `primary_vehicle_id`
  - `alternative_vehicle_ids`
  - `preferred_condition`
  - `required_features`
  - `vehicle_match_score`

- **Financial Fields:**
  - `estimated_value_amount`
  - `currency_code`
  - `budget_min_amount`
  - `budget_max_amount`
  - `payment_preference`
  - `finance_required`
  - `estimated_down_payment_amount`

- **Trade-In Fields:**
  - `trade_in_intent`
  - `trade_in_id`

- **Engagement Fields:**
  - `customer_temperature`
  - `next_action_type`
  - `next_action_at`
  - `last_meaningful_contact_at`
  - `next_follow_up_at`
  - `engagement_score`

- **Closing Fields:**
  - `won_reason`
  - `loss_reason`
  - `loss_reason_details`
  - `competitor_name`
  - `closed_at`

### Computed Fields

- `weighted_pipeline_value`
- `days_open`
- `stage_age_days`
- `days_since_last_contact`
- `next_action_overdue`
- `forecast_category`
- `vehicle_match_count`
- `active_quotation_count`
- `engagement_risk_score`

### Governance & Lifecycle

- **Source Snapshot:** `qualification_snapshot` (JSONB)
- **Requirements Snapshot:** `current_requirements_snapshot` (JSONB)
- **Forecast Snapshot:** `forecast_snapshot` (JSONB)
- **Metadata:**
  - `source_channel`
  - `campaign_id`
  - `originating_agent_id`
  - `external_crm_id`

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `stage_changed_by`
  - `last_processed_by_agent`
  - `closed_by`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** `owner_id`
- **Timestamps:**
  - `opened_at`
  - `assigned_at`
  - `stage_changed_at`
  - `last_meaningful_contact_at`
  - `next_follow_up_at`
  - `expected_close_date`
  - `closed_at`
  - `created_at`
  - `updated_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| opportunity_id | UUID | Unique canonical identifier for the Opportunity. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Opportunity. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| qualified_lead_id | UUID | Qualified Lead that created the Opportunity. | Yes | N/A | Must reference an eligible Qualified Lead in the same dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| customer_id | UUID | Customer associated with the sales pursuit. | Yes | N/A | Must match the Customer linked to the Qualified Lead | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| owner_id | UUID | Sales Consultant responsible for progressing the Opportunity. | Yes | Assignment result | Must reference an active authorized User in the same dealership | 321e6547-e89b-12d3-a456-426614174000 | System or human assignment |
| name | String | Human-readable Opportunity title. | Yes | System-generated | Maximum 150 characters | Ahmed Hassan - New SUV Purchase | N/A |
| stage | Enum | Current commercial pipeline stage. | Yes | CREATED | Must match OpportunityStage ENUM | NEGOTIATION | At least 0.99 |
| priority | Enum | Operational urgency of the Opportunity. | Yes | STANDARD | Must match OpportunityPriority ENUM | HIGH | At least 0.90 |
| primary_intent | Enum | Main commercial objective being pursued. | Yes | VEHICLE_PURCHASE | Must match OpportunityIntent ENUM | VEHICLE_PURCHASE | At least 0.90 |
| purchase_timeframe | Enum | Expected period in which the Customer may complete the purchase. | Yes | UNKNOWN | Must match PurchaseTimeframe ENUM | WITHIN_30_DAYS | At least 0.85 |
| vehicle_interest_text | Text | Current normalized description of the requested vehicle. | Yes | From qualification | Maximum 2,000 characters | Seven-seat family SUV with leather seats | At least 0.85 |
| primary_vehicle_id | UUID | Vehicle currently preferred by the Customer. | No | Null | Must reference a sellable Vehicle in the same dealership | 555e6666-e77b-88d9-a000-426614174000 | Human or system recommendation |
| alternative_vehicle_ids | JSONB | Alternative Vehicle identifiers being considered. | No | Empty array | Every Vehicle must belong to the same dealership | ["666e7777-e88b-99d0-a111-426614174000"] | System-assisted |
| vehicle_match_score | Decimal | Match confidence between Customer requirements and the primary Vehicle. | No | Null | Must remain between 0.00 and 1.00 | 0.92 | System-computed |
| estimated_value_amount | Decimal | Expected gross transaction value before final Deal approval. | Yes | 0.00 | Must be zero or greater | 1950000.00 | System or human estimate |
| currency_code | String | ISO 4217 currency code used by the Opportunity. | Yes | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| budget_min_amount | Decimal | Customer's lowest expected purchase budget. | No | Null | Must be zero or greater and not exceed budget_max_amount | 1600000.00 | At least 0.90 |
| budget_max_amount | Decimal | Customer's highest expected purchase budget. | No | Null | Must be zero or greater and not be below budget_min_amount | 2100000.00 | At least 0.90 |
| payment_preference | Enum | Expected payment method. | Yes | UNKNOWN | Must match PaymentPreference ENUM | FINANCE | At least 0.90 |
| finance_required | Boolean | Indicates whether finance is expected. | Yes | false | Must align with payment_preference | true | At least 0.90 |
| estimated_down_payment_amount | Decimal | Expected down-payment amount. | No | Null | Must be zero or greater and not exceed the estimated value | 500000.00 | At least 0.85 |
| trade_in_intent | Enum | Indicates whether the Customer expects to trade in a Vehicle. | Yes | UNKNOWN | Must match TradeInIntent ENUM | YES | At least 0.90 |
| trade_in_id | UUID | Trade-In assessment linked to the Opportunity. | No | Null | Must belong to the same Customer and dealership | 888e9999-e00b-11d2-a222-426614174000 | System-controlled |
| current_quotation_id | UUID | Current active Quotation presented for this Opportunity. | No | Null | Must reference a non-expired Quotation for the same Opportunity | 444e5555-e66b-77d8-a999-426614174000 | System-controlled |
| primary_appointment_id | UUID | Primary upcoming Appointment connected to the Opportunity. | No | Null | Must reference an active Appointment for the same Customer | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| close_probability | Decimal | Estimated probability that the Opportunity will be won. | Yes | 0.10 | Must remain between 0.00 and 1.00 | 0.75 | System or authorized human |
| close_confidence_source | Enum | Identifies whether close probability came from rules, AI, or a human. | Yes | RULE_BASED | Must match CloseConfidenceSource ENUM | AI_ASSISTED | N/A |
| expected_close_date | Date | Expected date for Deal conversion or closure. | No | Null | Must not precede opened_at | 2026-08-20 | Human or AI-assisted |
| next_action_type | Enum | Next required sales action. | No | Null | Must match NextActionType ENUM | SEND_QUOTATION | At least 0.90 |
| next_action_at | Timestamp | Deadline for the next required action. | No | Null | Must be later than or equal to the current time when created | 2026-08-02T09:00:00Z | Human or system |
| last_meaningful_contact_at | Timestamp | Most recent two-way or commercially meaningful Customer interaction. | No | Null | Cannot precede opened_at | 2026-08-01T11:30:00Z | System-recorded |
| engagement_score | Decimal | Current engagement strength calculated from recent Customer behavior. | Yes | 0.00 | Must remain between 0.00 and 100.00 | 82.50 | System-computed |
| loss_reason | Enum | Standard reason why the Opportunity was lost. | No | Null | Required when stage is LOST | PURCHASED_FROM_COMPETITOR | Human-verified |
| loss_reason_details | Text | Additional explanation supporting the loss classification. | No | Null | Maximum 2,000 characters | Customer selected a lower-priced competitor vehicle. | Human input |
| competitor_name | String | Competitor associated with the loss. | No | Null | Maximum 150 characters | Competitor Motors | Human or AI-extracted |
| converted_deal_id | UUID | Deal created after successful commercial commitment. | No | Null | Required when stage is WON | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| weighted_pipeline_value | Decimal | Estimated value multiplied by close probability. | No | 0.00 | Must be system-computed | 1462500.00 | System-computed |
| record_version | Integer | Version number used for optimistic concurrency control. | Yes | 1 | Must increment after every successful update | 7 | System-controlled |

## 4. Enumerations

### OpportunityStage

- **CREATED:** The Opportunity was created successfully but has not yet completed initial discovery.
- **DISCOVERY:** Customer requirements, budget, timing, and decision criteria are being confirmed.
- **SOLUTION_FIT:** Vehicles, finance structures, or commercial solutions are being matched to the Customer.
- **PROPOSAL:** A formal Quotation or structured proposal has been prepared or presented.
- **NEGOTIATION:** Price, Vehicle, finance, trade-in, or commercial terms are under active discussion.
- **COMMITMENT:** The Customer has provided a documented commitment signal and the Opportunity is ready for Deal creation.
- **ON_HOLD:** Progress is paused temporarily with a documented reason and review date.
- **WON:** A valid Deal was created successfully.
- **LOST:** The Opportunity ended without a successful Deal.

### OpportunityPriority

- STANDARD
- HIGH
- URGENT
- VIP

### OpportunityIntent

- VEHICLE_PURCHASE
- VEHICLE_REPLACEMENT
- FLEET_PURCHASE
- FINANCE_ONLY
- LEASE
- TRADE_IN_AND_PURCHASE
- VEHICLE_RESERVATION

### CustomerTemperature

- COLD
- WARM
- HOT
- IMMEDIATE

### ForecastCategory

- PIPELINE
- BEST_CASE
- COMMIT
- CLOSED_WON
- CLOSED_LOST

### CloseConfidenceSource

- RULE_BASED
- AI_AUTOMATED
- AI_ASSISTED
- HUMAN_ESTIMATE
- MANAGER_OVERRIDE

### NextActionType

- CALL_CUSTOMER
- SEND_MESSAGE
- SEND_VEHICLE_OPTIONS
- SCHEDULE_APPOINTMENT
- SCHEDULE_TEST_DRIVE
- PREPARE_QUOTATION
- SEND_QUOTATION
- FOLLOW_UP_QUOTATION
- REQUEST_DOCUMENTS
- REVIEW_FINANCE
- REVIEW_TRADE_IN
- REQUEST_MANAGER_APPROVAL
- CREATE_DEAL
- OTHER

### OpportunityLossReason

- PURCHASED_FROM_COMPETITOR
- PRICE_TOO_HIGH
- VEHICLE_UNAVAILABLE
- FINANCE_DECLINED
- TRADE_IN_VALUE_REJECTED
- CUSTOMER_UNREACHABLE
- CUSTOMER_POSTPONED
- CUSTOMER_NO_LONGER_INTERESTED
- REQUIREMENTS_NOT_MET
- DUPLICATE_OPPORTUNITY
- INVALID_QUALIFICATION
- DEALERSHIP_DECLINED
- OTHER

### OnHoldReason

- CUSTOMER_REQUEST
- WAITING_FOR_VEHICLE
- WAITING_FOR_FINANCE
- WAITING_FOR_TRADE_IN
- WAITING_FOR_DOCUMENTS
- WAITING_FOR_DECISION_MAKER
- SEASONAL_DELAY
- OTHER

## 5. Validation Rules

### Business Rules

- An Opportunity may be created only from an eligible Qualified Lead.
- `qualified_lead_id`, `customer_id`, and `dealership_id` must remain consistent.
- Every active Opportunity must have an assigned `owner_id`.
- The Opportunity must not store executed contract, payment, delivery, or final Deal data.
- An Opportunity cannot enter `PROPOSAL` unless at least one valid Vehicle or commercial solution has been identified.
- An Opportunity cannot enter `NEGOTIATION` without an active Quotation or documented negotiation context.
- An Opportunity cannot enter `COMMITMENT` unless the Customer has provided a verifiable commitment signal.
- An Opportunity cannot enter `WON` unless a Deal is created successfully.
- An Opportunity cannot enter `LOST` without a standardized `loss_reason`.
- A closed Opportunity cannot continue generating ordinary follow-up Tasks.
- Only one primary active Opportunity should exist for the same Customer and materially identical purchase intent unless a Sales Manager approves an exception.
- Vehicle availability must be revalidated before presenting or reserving a specific Vehicle.
- AI recommendations must not be presented as approved commercial terms.

### Technical Rules

- Opportunity creation and Qualified Lead conversion must use an idempotent transaction.
- `record_version` must increment after every successful update.
- Every stage transition must create a stage-history and audit record.
- Computed financial fields must use fixed decimal precision.
- `weighted_pipeline_value` must equal `estimated_value_amount × close_probability`.
- Stage age and days-open metrics must be calculated from authoritative timestamps.
- `qualification_snapshot` must preserve the original qualification context.
- `current_requirements_snapshot` must be versioned after material Customer-requirement changes.
- Closed Opportunities must be excluded from active work queues by default.
- Background Jobs must detect overdue next actions and stale Opportunities.

### Data Constraints

- `close_probability` must remain between `0.00` and `1.00`.
- `engagement_score` must remain between `0.00` and `100.00`.
- `vehicle_match_score` must remain between `0.00` and `1.00`.
- `budget_min_amount` cannot exceed `budget_max_amount`.
- Financial amounts cannot be negative.
- `estimated_down_payment_amount` cannot exceed `estimated_value_amount`.
- `expected_close_date` cannot precede `opened_at`.
- `closed_at` cannot precede `opened_at`.
- `converted_deal_id` must be null unless the Opportunity is `WON`.
- `loss_reason` must be null unless the Opportunity is `LOST`.
- `primary_vehicle_id` cannot appear again inside `alternative_vehicle_ids`.
- `next_action_at` must be null after terminal closure unless it represents an approved post-close action.

### Referential Integrity

- All linked records must belong to the same `dealership_id`.
- The linked Customer must match the Customer associated with the Qualified Lead.
- The linked Vehicle must be available to the dealership or accessible through an approved cross-location inventory policy.
- The current Quotation must reference this exact Opportunity.
- A linked Trade-In must belong to the same Customer.
- A linked Finance Application must reference this Opportunity or its Customer.
- `converted_deal_id` must reference a Deal created from this Opportunity.
- An Opportunity cannot be hard-deleted while referenced by a Quotation, Appointment, Trade-In, Finance Application, Deal, audit log, or Customer Journey.

### Human Approval Requirements

- A Sales Manager must approve reopening a `WON` or `LOST` Opportunity.
- A Sales Manager must approve duplicate active Opportunities for the same Customer and purchase intent.
- AI Agents cannot mark an Opportunity `WON` or `LOST` without an authorized transaction or verified human decision.
- AI Agents cannot override Vehicle pricing, finance approval, trade-in valuation, or Deal terms.
- Reducing the estimated commercial value below approved pricing boundaries requires human approval.
- Manager overrides of close probability or forecast category must include a reason.
- Material conflicts between Customer statements and stored commercial data must be routed for human review.
- Changing the linked Customer after identity verification requires controlled identity-resolution approval.

## 6. State Machine

### Allowed States

- CREATED
- DISCOVERY
- SOLUTION_FIT
- PROPOSAL
- NEGOTIATION
- COMMITMENT
- ON_HOLD
- WON
- LOST

### Allowed Transitions

- CREATED → DISCOVERY
- CREATED → LOST
- DISCOVERY → SOLUTION_FIT
- DISCOVERY → ON_HOLD
- DISCOVERY → LOST
- SOLUTION_FIT → DISCOVERY
- SOLUTION_FIT → PROPOSAL
- SOLUTION_FIT → ON_HOLD
- SOLUTION_FIT → LOST
- PROPOSAL → SOLUTION_FIT
- PROPOSAL → NEGOTIATION
- PROPOSAL → ON_HOLD
- PROPOSAL → LOST
- NEGOTIATION → PROPOSAL
- NEGOTIATION → COMMITMENT
- NEGOTIATION → ON_HOLD
- NEGOTIATION → LOST
- COMMITMENT → NEGOTIATION
- COMMITMENT → WON
- COMMITMENT → LOST
- ON_HOLD → DISCOVERY
- ON_HOLD → SOLUTION_FIT
- ON_HOLD → PROPOSAL
- ON_HOLD → NEGOTIATION
- ON_HOLD → LOST
- LOST → DISCOVERY
- WON → NEGOTIATION

> Transitions from `LOST` or `WON` require authorized reopening approval and must never occur automatically.

### Forbidden Transitions

- CREATED → PROPOSAL
- CREATED → NEGOTIATION
- CREATED → COMMITMENT
- CREATED → WON
- DISCOVERY → COMMITMENT
- DISCOVERY → WON
- SOLUTION_FIT → WON
- PROPOSAL → WON
- ON_HOLD → WON
- LOST → WON
- WON → CREATED
- WON → DISCOVERY
- WON → SOLUTION_FIT
- WON → PROPOSAL
- WON → ON_HOLD

### Entry Conditions

- To enter `DISCOVERY`:
  - An active owner must be assigned.
  - The Customer identity and contact permission must be valid.
  - The Qualified Lead context must be available.

- To enter `SOLUTION_FIT`:
  - Core Customer requirements must be documented.
  - Budget or affordability context must be known or explicitly marked unknown.
  - Purchase timing must be recorded.

- To enter `PROPOSAL`:
  - At least one valid Vehicle or approved commercial solution must exist.
  - Pricing inputs must be current.
  - A Quotation or proposal-preparation workflow must exist.

- To enter `NEGOTIATION`:
  - A proposal must have been presented.
  - Customer feedback or requested changes must be documented.
  - The current Quotation must remain valid or be replaced.

- To enter `COMMITMENT`:
  - The Customer must provide a verifiable commitment signal.
  - Vehicle availability must be confirmed.
  - Required approval dependencies must be identified.
  - The Opportunity must be ready for controlled Deal creation.

- To enter `ON_HOLD`:
  - An `on_hold_reason` must be recorded.
  - A review or follow-up date must be supplied.
  - The Customer must not have explicitly ended the purchase process.

- To enter `WON`:
  - A valid Deal must be created.
  - `converted_deal_id` must be populated.
  - The Deal must reference this Opportunity.
  - The conversion transaction must complete successfully.
  - `closed_at` and `closed_by` must be populated.

- To enter `LOST`:
  - `loss_reason` must be recorded.
  - `closed_at` and `closed_by` must be populated.
  - Open Quotations, Appointments, and ordinary follow-up Tasks must be closed or cancelled appropriately.

### Exit Conditions

- A record cannot exit `CREATED` until ownership and basic validation are complete.
- A record cannot exit `DISCOVERY` without documented Customer requirements.
- A record cannot exit `SOLUTION_FIT` toward `PROPOSAL` without a valid solution.
- A record cannot exit `PROPOSAL` toward `NEGOTIATION` without Customer response or documented negotiation activity.
- A record cannot exit `NEGOTIATION` toward `COMMITMENT` without a commitment signal.
- A record cannot exit `ON_HOLD` without a review decision and refreshed next action.
- A record cannot exit `LOST` or `WON` without authorized reopening approval.

### Terminal States

- **WON:** The sales journey continues through the linked Deal.
- **LOST:** The active pursuit ended without a successful Deal, while the Customer may later begin a new qualification or Opportunity cycle.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Qualified Lead identified by `qualified_lead_id`.
  - Customer identified by `customer_id`.
  - Active Sales Consultant identified by `owner_id`.

- **Consumes:**
  - Qualified Lead requirements and qualification evidence.
  - Customer preferences, consent status, and interaction history.
  - Vehicle inventory, availability, specifications, and pricing.
  - Appointment and test-drive outcomes.
  - Quotation responses and negotiation feedback.
  - Trade-In and Finance Application information.

- **Produces:**
  - Current sales-pipeline context.
  - Forecast values and close-probability estimates.
  - Vehicle recommendations.
  - Quotation and negotiation requirements.
  - Next-action and follow-up Tasks.
  - Deal-creation payloads.
  - Win and loss analytics.

- **Creates:**
  - Quotation records.
  - Appointment and test-drive requests.
  - Sales Tasks.
  - Finance Application requests.
  - Trade-In assessments.
  - Deal after successful conversion.

- **Triggers:**
  - Stage-change workflows.
  - Follow-up reminders.
  - Vehicle availability checks.
  - Quotation preparation.
  - Manager approval requests.
  - Stale-Opportunity alerts.
  - Opportunity-to-Deal conversion.

- **Owned By:**
  - A Sales Consultant identified by `owner_id`.
  - A Sales Manager may reassign ownership under dealership policy.

- **Referenced By:**
  - Deal
  - Quotation
  - Appointment
  - Task
  - Trade-In
  - Finance Application
  - Interaction Log
  - Customer Journey
  - Vehicle Match Recommendation
  - AI Agent Run
  - Forecast Snapshot

## 8. Domain Events

### Emitted Events

- **OpportunityCreated**  
  Payload: `opportunity_id`, `qualified_lead_id`, `customer_id`, `owner_id`, `opened_at`

- **OpportunityAssigned**  
  Payload: `opportunity_id`, `previous_owner_id`, `new_owner_id`, `assigned_at`

- **OpportunityStageChanged**  
  Payload: `opportunity_id`, `old_stage`, `new_stage`, `changed_by`, `stage_changed_at`

- **OpportunityRequirementsUpdated**  
  Payload: `opportunity_id`, `changed_fields`, `requirements_version`, `updated_at`

- **OpportunityVehicleSelected**  
  Payload: `opportunity_id`, `primary_vehicle_id`, `vehicle_match_score`

- **OpportunityQuotationRequested**  
  Payload: `opportunity_id`, `primary_vehicle_id`, `requested_by`, `requested_at`

- **OpportunityQuotationPresented**  
  Payload: `opportunity_id`, `quotation_id`, `presented_at`

- **OpportunityNegotiationStarted**  
  Payload: `opportunity_id`, `quotation_id`, `started_at`

- **OpportunityCommitmentRecorded**  
  Payload: `opportunity_id`, `commitment_type`, `evidence_reference`, `recorded_at`

- **OpportunityPlacedOnHold**  
  Payload: `opportunity_id`, `on_hold_reason`, `next_review_at`

- **OpportunityWon**  
  Payload: `opportunity_id`, `converted_deal_id`, `closed_at`, `closed_by`

- **OpportunityLost**  
  Payload: `opportunity_id`, `loss_reason`, `competitor_name`, `closed_at`

- **OpportunityReopened**  
  Payload: `opportunity_id`, `previous_stage`, `new_stage`, `approved_by`, `reopened_at`

- **OpportunityNextActionOverdue**  
  Payload: `opportunity_id`, `owner_id`, `next_action_type`, `next_action_at`

### Consumed Events

- **QualifiedLeadConverted**  
  Creates the Opportunity using the Qualified Lead and Customer context.

- **CustomerContactPermissionChanged**  
  Suspends unauthorized outbound actions when permission becomes restricted or `OPT_OUT`.

- **VehicleStatusChanged**  
  Revalidates the primary and alternative Vehicles linked to the Opportunity.

- **VehiclePriceUpdated**  
  Marks affected Quotations or pricing assumptions for review.

- **AppointmentCompleted**  
  Updates engagement context, Customer feedback, and the next recommended action.

- **QuotationAccepted**  
  Allows progression toward `COMMITMENT`.

- **QuotationRejected**  
  Updates negotiation context and may return the Opportunity to `SOLUTION_FIT`.

- **FinanceApplicationStatusChanged**  
  Updates the Opportunity when finance is approved, declined, or requires additional documents.

- **TradeInAssessmentCompleted**  
  Updates the commercial solution and negotiation context.

- **DealCreated**  
  Populates `converted_deal_id` and transitions the Opportunity to `WON`.

- **DealCreationFailed**  
  Preserves the Opportunity in `COMMITMENT` and creates a retry or Human Review Task.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `vehicle_interest_text`
- `required_features`
- Customer objections
- Customer preferences
- Negotiation summaries
- Appointment and test-drive feedback
- Sales-interaction summaries
- Purchase motivation
- Decision criteria
- Competitor comparisons
- Loss-reason details
- Next-action context

### Fields Excluded from Embeddings

- `opportunity_id`
- `customer_id`
- `qualified_lead_id`
- `owner_id`
- `converted_deal_id`
- Direct phone numbers and email addresses
- `budget_min_amount`
- `budget_max_amount`
- `estimated_down_payment_amount`
- `estimated_value_amount`
- Internal pricing boundaries
- Finance documents
- Identity documents
- Internal User or Agent identifiers

> Personally identifiable information, exact financial values, and restricted commercial terms must be provided through authorized structured context rather than unrestricted semantic retrieval.

### Structured AI Context Fields

Authorized AI Agents may receive the following fields as structured context:

- `stage`
- `priority`
- `primary_intent`
- `purchase_timeframe`
- `primary_vehicle_id`
- `alternative_vehicle_ids`
- `payment_preference`
- `finance_required`
- `trade_in_intent`
- `customer_temperature`
- `next_action_type`
- `next_action_at`
- `expected_close_date`
- `close_probability`
- `engagement_score`

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `opportunity_id`
- `customer_id`
- `owner_id`
- `stage`
- `priority`
- `primary_vehicle_id`
- `purchase_timeframe`
- `preferred_language`
- `forecast_category`

### Confidence Thresholds

- Vehicle-preference extraction requires confidence of at least `0.85`.
- Budget extraction requires confidence of at least `0.90`.
- Purchase-timeframe classification requires confidence of at least `0.85`.
- Customer-objection extraction requires confidence of at least `0.80`.
- Next-action recommendations require confidence of at least `0.85`.
- Vehicle recommendations require a match score of at least `0.85`.
- AI-generated close-probability changes above `0.15` require explanation and human review.
- Automatic stage recommendations require confidence of at least `0.90`.
- No AI confidence score alone may transition an Opportunity to `WON` or `LOST`.

### Human Approval Thresholds

- AI Agents cannot mark an Opportunity `WON` or `LOST`.
- AI Agents cannot approve pricing, discounts, finance terms, trade-in values, or reservations.
- AI Agents cannot reopen a closed Opportunity.
- Changes to `customer_id`, `converted_deal_id`, or ownership across dealerships require controlled human approval.
- Conflicting Customer requirements must create a Human Review Task.
- Manager approval is required before presenting terms below permitted commercial boundaries.
- AI-generated negotiation suggestions must be treated as recommendations, not approved commitments.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/opportunities`

### Methods

- `GET` — list or search Opportunities.
- `POST` — create an Opportunity through Qualified Lead conversion.
- `GET /{id}` — retrieve one Opportunity.
- `PATCH /{id}` — update permitted Opportunity fields.
- `POST /{id}/assign` — assign or reassign ownership.
- `POST /{id}/transition` — perform a validated stage transition.
- `POST /{id}/select-vehicle` — select or replace the primary Vehicle.
- `POST /{id}/request-quotation` — begin Quotation preparation.
- `POST /{id}/place-on-hold` — pause progress with a documented reason.
- `POST /{id}/record-commitment` — store a verified Customer commitment signal.
- `POST /{id}/convert` — create a Deal through an idempotent transaction.
- `POST /{id}/close-lost` — close the Opportunity with a standardized loss reason.
- `POST /{id}/reopen` — reopen a closed Opportunity after approval.
- `DELETE /{id}` — perform a soft delete when permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateOpportunityRequest",
  "type": "object",
  "properties": {
    "qualified_lead_id": {
      "type": "string",
      "format": "uuid"
    },
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "owner_id": {
      "type": "string",
      "format": "uuid"
    },
    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 150
    },
    "priority": {
      "type": "string",
      "enum": ["STANDARD", "HIGH", "URGENT", "VIP"]
    },
    "primary_intent": {
      "type": "string",
      "enum": [
        "VEHICLE_PURCHASE",
        "VEHICLE_REPLACEMENT",
        "FLEET_PURCHASE",
        "FINANCE_ONLY",
        "LEASE",
        "TRADE_IN_AND_PURCHASE",
        "VEHICLE_RESERVATION"
      ]
    },
    "purchase_timeframe": {
      "type": "string",
      "enum": [
        "IMMEDIATE",
        "WITHIN_7_DAYS",
        "WITHIN_30_DAYS",
        "WITHIN_90_DAYS",
        "OVER_90_DAYS",
        "UNKNOWN"
      ]
    },
    "vehicle_interest_text": {
      "type": "string",
      "minLength": 1,
      "maxLength": 2000
    },
    "primary_vehicle_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "estimated_value_amount": {
      "type": "number",
      "minimum": 0
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "budget_min_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "budget_max_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "payment_preference": {
      "type": "string",
      "enum": ["CASH", "FINANCE", "LEASE", "MIXED", "UNKNOWN"]
    },
    "finance_required": {
      "type": "boolean"
    },
    "trade_in_intent": {
      "type": "string",
      "enum": ["YES", "NO", "UNKNOWN"]
    },
    "expected_close_date": {
      "type": ["string", "null"],
      "format": "date"
    },
    "close_probability": {
      "type": "number",
      "minimum": 0,
      "maximum": 1
    }
  },
  "required": [
    "qualified_lead_id",
    "customer_id",
    "owner_id",
    "name",
    "priority",
    "primary_intent",
    "purchase_timeframe",
    "vehicle_interest_text",
    "estimated_value_amount",
    "currency_code",
    "payment_preference",
    "finance_required",
    "trade_in_intent",
    "close_probability"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Opportunity {
  id: ID!
  dealershipId: ID!
  qualifiedLeadId: ID!
  customerId: ID!
  ownerId: ID!
  salesTeamId: ID
  name: String!
  stage: OpportunityStage!
  priority: OpportunityPriority!
  primaryIntent: OpportunityIntent!
  purchaseTimeframe: PurchaseTimeframe!
  vehicleInterestText: String!
  primaryVehicleId: ID
  alternativeVehicleIds: [ID!]!
  vehicleMatchScore: Float
  estimatedValueAmount: Float!
  currencyCode: String!
  budgetMinAmount: Float
  budgetMaxAmount: Float
  paymentPreference: PaymentPreference!
  financeRequired: Boolean!
  estimatedDownPaymentAmount: Float
  tradeInIntent: TradeInIntent!
  tradeInId: ID
  currentQuotationId: ID
  primaryAppointmentId: ID
  financeApplicationId: ID
  closeProbability: Float!
  forecastCategory: ForecastCategory!
  expectedCloseDate: Date
  customerTemperature: CustomerTemperature
  nextActionType: NextActionType
  nextActionAt: DateTime
  lastMeaningfulContactAt: DateTime
  engagementScore: Float!
  weightedPipelineValue: Float!
  daysOpen: Int!
  stageAgeDays: Int!
  lossReason: OpportunityLossReason
  competitorName: String
  convertedDealId: ID
  openedAt: DateTime!
  stageChangedAt: DateTime!
  closedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `opportunities`
- **Stage-History Table:** `opportunity_stage_history`
- **Requirements Table:** `opportunity_requirement_versions`
- **Vehicle-Match Table:** `opportunity_vehicle_matches`
- **Forecast Table:** `opportunity_forecast_history`
- **Activity Table:** `opportunity_activities`
- **Audit Table:** `opportunity_audit_log`

### Indexes

- `idx_opportunities_tenant_stage (dealership_id, stage)`  
  Used for pipeline dashboards and active-stage queues.

- `idx_opportunities_owner_stage (dealership_id, owner_id, stage)`  
  Used for Sales Consultant work queues.

- `idx_opportunities_customer (dealership_id, customer_id, stage)`  
  Used to retrieve a Customer's active and historical Opportunities.

- `idx_opportunities_qualified_lead (qualified_lead_id)`  
  Used to validate Qualified Lead conversion.

- `idx_opportunities_primary_vehicle (dealership_id, primary_vehicle_id, stage)`  
  Used to detect competing active Opportunities against the same Vehicle.

- `idx_opportunities_expected_close (dealership_id, expected_close_date, stage)`  
  Used for forecasting and manager reviews.

- `idx_opportunities_next_action (dealership_id, next_action_at, stage)`  
  Used for overdue-action monitoring.

- `idx_opportunities_forecast (dealership_id, forecast_category, expected_close_date)`  
  Used for pipeline forecasting.

- `idx_opportunities_converted_deal (converted_deal_id)`  
  Used to validate successful Deal conversion.

### Unique Constraints

- `UQ_opportunity_qualified_lead (qualified_lead_id)`  
  Applies to the primary Opportunity created from a Qualified Lead.

- `UQ_opportunity_converted_deal (converted_deal_id)`  
  Applies when `converted_deal_id` is not null.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `qualified_lead_id` → `qualified_leads(id)`
- `customer_id` → `customers(id)`
- `owner_id` → `users(id)`
- `sales_team_id` → `sales_teams(id)` — nullable
- `primary_vehicle_id` → `vehicles(id)` — nullable
- `current_quotation_id` → `quotations(id)` — nullable
- `primary_appointment_id` → `appointments(id)` — nullable
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)` — nullable
- `converted_deal_id` → `deals(id)` — nullable

### Database Constraints

- `close_probability BETWEEN 0.00 AND 1.00`
- `engagement_score BETWEEN 0.00 AND 100.00`
- `vehicle_match_score BETWEEN 0.00 AND 1.00`
- `budget_min_amount <= budget_max_amount`
- `estimated_down_payment_amount <= estimated_value_amount`
- `weighted_pipeline_value = estimated_value_amount × close_probability`
- `closed_at >= opened_at`
- `converted_deal_id IS NOT NULL` when `stage = WON`
- `loss_reason IS NOT NULL` when `stage = LOST`
- `next_action_at IS NULL` after terminal closure unless an approved post-close action exists

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical Opportunities by `opened_at`.
- Stage-history, forecast, activity, requirements, and audit tables must use the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read/Write access to Opportunities assigned to them.
- **Sales Manager:** Read/Write, reassignment, forecasting, reopening, and approval access across the matching dealership.
- **BDC Agent:** Limited access to early-stage Opportunities when explicitly assigned.
- **Finance User:** Access only to approved finance-related fields and linked Finance Applications.
- **Inventory User:** Read access to Vehicle requirements and linked inventory, without unrestricted Customer financial data.
- **Marketing User:** Read-only access to aggregated source, stage, and conversion analytics.
- **AI Sales Agent:** Service Account access limited to recommendations, summaries, permitted updates, and approved transition requests.
- **Opportunity Service:** Controlled create, update, and conversion access using tenant-scoped service credentials.
- **Deal Service:** Create-conversion access only through validated and idempotent operations.

### PII Classification

- **Level:** `CRITICAL_PII — INHERITED`

The Opportunity references Customer and Lead information that may contain:

- Names
- Phone numbers
- Email addresses
- Communication preferences
- Interaction summaries
- Appointment information

### Commercially Sensitive Fields

- `estimated_value_amount`
- `budget_min_amount`
- `budget_max_amount`
- `estimated_down_payment_amount`
- `close_probability`
- `forecast_category`
- `payment_preference`
- `finance_required`
- `current_quotation_id`
- Negotiation summaries
- Customer objections
- Manager approvals
- Internal pricing or margin references

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for database volumes and backups.
- **Column-Level Protection:** Financial-intent fields, requirement snapshots, negotiation summaries, and inherited Customer context require encryption, tokenization, or an equivalent approved protection method.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every stage transition must create an immutable audit record containing:
  - `opportunity_id`
  - `previous_stage`
  - `new_stage`
  - `actor_id`
  - `timestamp`
  - `reason`
  - `supporting_evidence`

- Ownership changes must record:
  - Previous owner
  - New owner
  - Reassignment reason
  - Approver when required
  - Timestamp

- Forecast changes must record:
  - Previous close probability
  - New close probability
  - Previous forecast category
  - New forecast category
  - Source of the estimate
  - Manager-override reason when applicable

- Deal conversion must preserve:
  - Conversion request ID
  - Idempotency key
  - Created Deal ID
  - Vehicle and Quotation references
  - Actor
  - Timestamp
  - Commercial-context snapshot

- Human overrides of AI recommendations must retain both the original AI recommendation and the final human decision.
- Access to negotiation, budget, and finance-related data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, Qualified Lead, Vehicle, Quotation, Appointment, Finance Application, Trade-In, or Deal linking is prohibited.
- AI Agents and background Jobs must receive tenant scope through signed execution context.
- Vector retrieval, exports, analytics, and duplicate checks must remain tenant-scoped.

### Retention and Deletion

- Soft deletion is the operational default.
- Won Opportunities must remain available while their linked Deals, financial records, or audit entries exist.
- Lost Opportunities must be retained according to the dealership's sales-history and compliance policies.
- Legally approved deletion requests must purge or anonymize inherited PII across:
  - Opportunity records
  - Requirement versions
  - Stage history
  - Interaction summaries
  - Vector stores
  - Audit references
  - Analytics stores
  - Backups, according to the approved retention policy
