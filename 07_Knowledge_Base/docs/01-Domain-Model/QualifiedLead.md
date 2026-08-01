# Qualified Lead

## 1. Object Purpose

### Business Purpose

The Qualified Lead object represents a validated automotive sales prospect who has passed the dealership's minimum qualification requirements.

It confirms that the prospect has:

- A resolved Customer identity.
- At least one usable and permitted contact method.
- Confirmed automotive purchase, finance, trade-in, reservation, or test-drive intent.
- Sufficient qualification evidence to justify active sales engagement.

The Qualified Lead object separates raw inquiry processing from the formal sales pipeline. It gives the dealership a reliable handoff point between Lead Qualification and Opportunity Management while preserving the original Lead for source attribution, response-time reporting, and audit history.

### System Purpose

The Qualified Lead object is the controlled bridge between the Lead and Opportunity aggregates.

It is created only through a successful Lead qualification transaction and stores the normalized commercial intent, timing, budget, vehicle requirements, finance indicators, trade-in indicators, qualification evidence, and routing information required by sales workflows.

A Qualified Lead does not represent a negotiation, quotation, reservation, or Deal. Those activities begin only after an Opportunity is created.

The object provides the canonical context used by:

- Sales routing.
- Lead prioritization.
- Customer-to-vehicle matching.
- Appointment recommendations.
- Nurture decisions.
- Opportunity creation.
- AI Agent handoff.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `qualified_lead_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `lead_id` (UUIDv4 — required)
  - `customer_id` (UUIDv4 — required)
  - `owner_id` (UUIDv4 — optional until assignment)
  - `created_opportunity_id` (UUIDv4 — optional; populated after conversion)
  - `qualifying_agent_id` (UUIDv4 — optional)
  - `qualification_reviewed_by` (UUIDv4 — optional)

### Qualification Payload

- **Intent Fields:**
  - `intent_level`
  - `purchase_timeframe`
  - `purchase_purpose`
  - `preferred_contact_method`

- **Vehicle Requirement Fields:**
  - `vehicle_interest_text`
  - `preferred_make`
  - `preferred_model`
  - `preferred_condition`
  - `preferred_body_type`
  - `preferred_fuel_type`
  - `preferred_color`
  - `required_features`
  - `acceptable_alternatives`

- **Commercial Fields:**
  - `budget_min_amount`
  - `budget_max_amount`
  - `currency_code`
  - `payment_preference`
  - `finance_required`
  - `estimated_down_payment_amount`

- **Trade-In Fields:**
  - `trade_in_intent`
  - `trade_in_vehicle_summary`

- **Engagement Fields:**
  - `test_drive_intent`
  - `appointment_intent`
  - `preferred_dealership_location`
  - `preferred_contact_time`

### Qualification Decision

- `qualification_score`
- `qualification_method`
- `qualification_reason`
- `qualification_evidence`
- `confidence_score`
- `priority_score`
- `routing_priority`
- `status`

### Governance & Lifecycle

- **Source Snapshot:** `source_context_snapshot` (JSONB)
- **Requirements Snapshot:** `requirements_snapshot` (JSONB)
- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `last_processed_by_agent`
  - `qualification_reviewed_by`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** `owner_id`
- **Timestamps:**
  - `qualified_at`
  - `routed_at`
  - `assigned_at`
  - `accepted_at`
  - `nurture_started_at`
  - `converted_at`
  - `rejected_at`
  - `expires_at`
  - `created_at`
  - `updated_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| qualified_lead_id | UUID | Unique canonical identifier for the Qualified Lead. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Qualified Lead. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| lead_id | UUID | Original Lead that produced the qualification. | Yes | N/A | Must reference a QUALIFIED Lead in the same dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| customer_id | UUID | Resolved Customer associated with the Qualified Lead. | Yes | N/A | Must reference an active Customer in the same dealership | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| owner_id | UUID | Sales Consultant or BDC Agent responsible for follow-up. | No | Null | Must reference an active User in the same dealership | 321e6547-e89b-12d3-a456-426614174000 | System or human assignment |
| created_opportunity_id | UUID | Opportunity created from the Qualified Lead. | No | Null | Required when status is CONVERTED | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| intent_level | Enum | Strength and immediacy of the customer's automotive intent. | Yes | MEDIUM | Must match IntentLevel ENUM | HIGH | At least 0.85 |
| purchase_timeframe | Enum | Expected time until the customer intends to act. | Yes | UNKNOWN | Must match PurchaseTimeframe ENUM | WITHIN_30_DAYS | At least 0.80 |
| purchase_purpose | Enum | Primary commercial purpose of the inquiry. | Yes | VEHICLE_PURCHASE | Must match PurchasePurpose ENUM | VEHICLE_PURCHASE | At least 0.85 |
| vehicle_interest_text | Text | Normalized summary of the requested vehicle. | Yes | N/A | Maximum 2,000 characters | New family SUV with seven seats | At least 0.85 |
| preferred_make | String | Preferred vehicle manufacturer. | No | Null | Must match the approved OEM dictionary when identified | Toyota | At least 0.90 |
| preferred_model | String | Preferred vehicle model. | No | Null | Must belong to preferred_make when both are provided | Fortuner | At least 0.90 |
| preferred_condition | Enum | Preferred NEW, USED, CPO, or DEMO condition. | No | Null | Must match VehicleCondition ENUM | NEW | At least 0.85 |
| preferred_body_type | String | Requested vehicle body classification. | No | Null | Must match the approved body-type dictionary | SUV | At least 0.85 |
| required_features | JSONB | Structured list of mandatory vehicle features. | No | Empty array | Each value must match an approved feature dictionary or retain its source text | ["7 seats","leather"] | At least 0.80 |
| acceptable_alternatives | JSONB | Alternative makes, models, or vehicle classes accepted by the customer. | No | Empty array | Must not duplicate the primary preference | ["Kia Sorento"] | At least 0.80 |
| budget_min_amount | Decimal | Lowest stated or inferred purchase budget. | No | Null | Must be zero or greater and not exceed budget_max_amount | 1500000.00 | At least 0.85 |
| budget_max_amount | Decimal | Highest stated or inferred purchase budget. | No | Null | Must be zero or greater and not be below budget_min_amount | 2000000.00 | At least 0.85 |
| currency_code | String | ISO 4217 currency code for financial values. | Conditional | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| payment_preference | Enum | Customer's expected payment method. | Yes | UNKNOWN | Must match PaymentPreference ENUM | FINANCE | At least 0.85 |
| finance_required | Boolean | Indicates whether vehicle finance is expected. | Yes | false | Must align with payment_preference | true | At least 0.90 |
| estimated_down_payment_amount | Decimal | Estimated customer down payment. | No | Null | Must be zero or greater | 500000.00 | At least 0.80 |
| trade_in_intent | Enum | Indicates whether a trade-in may be included. | Yes | UNKNOWN | Must match TradeInIntent ENUM | YES | At least 0.85 |
| trade_in_vehicle_summary | Text | Preliminary description of the customer's current vehicle. | No | Null | Required when trade_in_intent is YES and information is available | 2020 Hyundai Tucson | At least 0.80 |
| test_drive_intent | Boolean | Indicates interest in a vehicle test drive. | Yes | false | Cannot create an Appointment automatically without permission and availability | true | At least 0.90 |
| appointment_intent | Boolean | Indicates willingness to visit or schedule a remote consultation. | Yes | false | Must be based on explicit or high-confidence intent | true | At least 0.90 |
| qualification_score | Decimal | Overall commercial qualification score. | Yes | 0.00 | Must remain between 0.00 and 1.00 | 0.91 | System-computed |
| confidence_score | Decimal | Confidence in the extracted qualification payload. | Yes | 0.00 | Must remain between 0.00 and 1.00 | 0.94 | System-computed |
| priority_score | Decimal | Operational priority used for routing and queue ordering. | Yes | 0.00 | Must remain between 0.00 and 100.00 | 87.50 | System-computed |
| qualification_method | Enum | Method used to approve the qualification. | Yes | AI_ASSISTED | Must match QualificationMethod ENUM | AI_ASSISTED | N/A |
| qualification_reason | Text | Human-readable explanation of why the Lead qualified. | Yes | N/A | Maximum 2,000 characters | Valid contact, purchase intent and 30-day timeframe confirmed | N/A |
| qualification_evidence | JSONB | Structured evidence supporting the qualification decision. | Yes | Empty object | Must identify source interactions and extracted signals | {"contact_valid":true,"intent":"purchase"} | System-generated |
| status | Enum | Current Qualified Lead lifecycle state. | Yes | CREATED | Must match QualifiedLeadStatus ENUM | ROUTED | At least 0.99 |
| qualified_at | Timestamp | Time the Lead qualification transaction completed. | Yes | Current timestamp | Must be equal to or later than the original Lead received_at | 2026-08-01T11:00:00Z | System-recorded |
| expires_at | Timestamp | Time at which the qualification must be refreshed. | No | Policy-defined | Must be later than qualified_at | 2026-09-01T11:00:00Z | System-calculated |

## 4. Enumerations

### QualifiedLeadStatus

- **CREATED:** The qualification transaction completed and the record was created.
- **READY_FOR_ROUTING:** The Qualified Lead is ready for assignment.
- **ROUTED:** The record was routed to a User, team, or dealership queue.
- **ACCEPTED:** The assigned owner accepted responsibility for the Qualified Lead.
- **NURTURE:** The prospect is valid but not ready for immediate Opportunity creation.
- **CONVERTED:** An Opportunity was created successfully.
- **REJECTED:** A human reviewer rejected the qualification because the evidence was materially incorrect.
- **EXPIRED:** The qualification became stale and requires revalidation.

### IntentLevel

- LOW
- MEDIUM
- HIGH
- IMMEDIATE

### PurchaseTimeframe

- IMMEDIATE
- WITHIN_7_DAYS
- WITHIN_30_DAYS
- WITHIN_90_DAYS
- OVER_90_DAYS
- UNKNOWN

### PurchasePurpose

- VEHICLE_PURCHASE
- VEHICLE_REPLACEMENT
- FLEET_PURCHASE
- FINANCE_INQUIRY
- TRADE_IN
- TEST_DRIVE
- VEHICLE_RESERVATION
- GENERAL_SALES_CONSULTATION

### PaymentPreference

- CASH
- FINANCE
- LEASE
- MIXED
- UNKNOWN

### TradeInIntent

- YES
- NO
- UNKNOWN

### QualificationMethod

- RULE_BASED
- AI_AUTOMATED
- AI_ASSISTED
- HUMAN_VERIFIED
- IMPORTED

### RoutingPriority

- STANDARD
- HIGH
- URGENT
- VIP

## 5. Validation Rules

### Business Rules

- A Qualified Lead can be created only from a Lead whose status is `QUALIFIED`.
- `lead_id` and `customer_id` are mandatory and must belong to the same dealership.
- A Qualified Lead must contain confirmed automotive intent and at least one usable permitted contact method inherited from the Customer or Lead.
- The qualification transaction must preserve the original Lead and must not replace or delete it.
- Opportunity, Quotation, negotiation, reservation, and Deal information must not be stored directly in the Qualified Lead.
- Only one non-terminal Qualified Lead may exist for the same `lead_id`.
- A Qualified Lead can create no more than one primary Opportunity unless the original Opportunity is formally closed and a new qualification cycle is approved.
- A Qualified Lead with `contact_permission_status = OPT_OUT` inherited from the Customer or Lead cannot be routed for outbound engagement.
- A Qualified Lead must enter `NURTURE` when the prospect is valid but the expected purchase timeframe falls outside the dealership's active-sales window.
- Conversion to Opportunity must preserve the complete qualification snapshot.

### Technical Rules

- Creation of the Qualified Lead, update of the original Lead to `QUALIFIED`, and Customer resolution must occur in one controlled transaction.
- `qualification_score`, `confidence_score`, and `priority_score` must be generated using versioned scoring rules.
- The scoring-rule version and AI model version must be stored with the qualification evidence.
- Financial values must use fixed decimal precision and an approved `currency_code`.
- `requirements_snapshot` must become immutable after conversion to Opportunity.
- `record_version` must increment after every successful update.
- Conversion requests must support idempotency to prevent duplicate Opportunities.

### Data Constraints

- `qualification_score` and `confidence_score` must remain between `0.00` and `1.00`.
- `priority_score` must remain between `0.00` and `100.00`.
- `budget_min_amount` cannot exceed `budget_max_amount`.
- Financial amounts cannot be negative.
- `expires_at` must be later than `qualified_at`.
- `converted_at` cannot be earlier than `qualified_at`.
- `created_opportunity_id` must be null unless the status is `CONVERTED`.
- `lead_id` cannot reference a duplicate or invalid Lead.

### Referential Integrity

- `lead_id`, `customer_id`, `owner_id`, `created_opportunity_id`, and all reviewing User IDs must belong to the same `dealership_id`.
- The linked Lead must remain available for attribution and audit retrieval.
- The linked Customer cannot be soft-deleted while the Qualified Lead is non-terminal.
- `created_opportunity_id` must reference an Opportunity created from this exact Qualified Lead.
- A Converted Qualified Lead cannot be hard-deleted while its Opportunity exists.

### Human Approval Requirements

- Qualification below the configured automatic-confidence threshold requires human approval.
- A Sales Manager or authorized reviewer must approve a change from `REJECTED` back to an active state.
- AI Agents cannot change the linked `customer_id` after human identity verification.
- Conflicting budget, contact, or vehicle-preference evidence must be routed for human review.
- Conversion of an expired qualification requires revalidation or explicit human approval.
- AI Agents cannot fabricate unknown budget, finance, trade-in, or timing information; unknown values must remain explicitly unknown.

## 6. State Machine

### Allowed States

- CREATED
- READY_FOR_ROUTING
- ROUTED
- ACCEPTED
- NURTURE
- CONVERTED
- REJECTED
- EXPIRED

### Allowed Transitions

- CREATED → READY_FOR_ROUTING
- READY_FOR_ROUTING → ROUTED
- READY_FOR_ROUTING → NURTURE
- READY_FOR_ROUTING → REJECTED
- ROUTED → ACCEPTED
- ROUTED → READY_FOR_ROUTING
- ROUTED → NURTURE
- ROUTED → REJECTED
- ACCEPTED → CONVERTED
- ACCEPTED → NURTURE
- ACCEPTED → REJECTED
- NURTURE → READY_FOR_ROUTING
- NURTURE → EXPIRED
- CREATED → EXPIRED
- READY_FOR_ROUTING → EXPIRED
- ROUTED → EXPIRED
- ACCEPTED → EXPIRED
- REJECTED → READY_FOR_ROUTING

### Forbidden Transitions

- CONVERTED → CREATED
- CONVERTED → READY_FOR_ROUTING
- CONVERTED → ROUTED
- CONVERTED → ACCEPTED
- CONVERTED → NURTURE
- EXPIRED → CONVERTED
- REJECTED → CONVERTED
- CREATED → CONVERTED
- READY_FOR_ROUTING → CONVERTED
- ROUTED → CONVERTED

### Entry Conditions

- To enter `READY_FOR_ROUTING`:
  - The Customer identity must be resolved.
  - Contact permission must allow the intended communication.
  - Qualification evidence must be complete.
  - The qualification and confidence thresholds must be satisfied.

- To enter `ROUTED`:
  - A valid active owner or dealership queue must be selected.
  - The routing decision must comply with territory, language, workload, and dealership rules.

- To enter `ACCEPTED`:
  - The assigned owner must acknowledge responsibility.
  - The Qualified Lead must not be expired or rejected.

- To enter `NURTURE`:
  - A valid nurture reason and next-review date must be recorded.
  - Outbound permission must remain valid for any scheduled follow-up.

- To enter `CONVERTED`:
  - A valid Opportunity must be created.
  - The Opportunity must reference this `qualified_lead_id`.
  - `created_opportunity_id` and `converted_at` must be populated.
  - The conversion transaction must complete successfully.

- To enter `REJECTED`:
  - A documented rejection reason and supporting evidence must exist.
  - Any active assignment must be closed or released.

- To enter `EXPIRED`:
  - `expires_at` must have passed, or material qualification information must have become stale.

### Exit Conditions

- A record cannot exit `CREATED` until validation and scoring complete.
- A record cannot exit `READY_FOR_ROUTING` without a routing, nurture, rejection, or expiry decision.
- A record cannot exit `ROUTED` without assignment acceptance, rerouting, nurture, rejection, or expiry.
- A record cannot exit `NURTURE` without refreshed qualification evidence.
- A record cannot exit `REJECTED` without human review and corrected evidence.
- A record cannot exit `EXPIRED` directly; a new qualification cycle or version must be created.

### Terminal States

- **CONVERTED:** The commercial journey continues through the Opportunity.
- **EXPIRED:** The existing qualification cannot be used until a new qualification cycle is completed.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Original Lead identified by `lead_id`.
  - Resolved Customer identified by `customer_id`.
  - Active User or assignment queue used for routing.

- **Consumes:**
  - Lead qualification evidence.
  - Customer identity and contact-permission status.
  - Vehicle requirements extracted from conversations.
  - Dealership routing, territory, language, and workload rules.
  - Available inventory context used for preliminary vehicle matching.

- **Produces:**
  - Normalized commercial-intent context.
  - Vehicle-matching requirements.
  - Sales-routing recommendations.
  - Opportunity-creation payloads.
  - Nurture and follow-up recommendations.

- **Creates:**
  - Opportunity after successful conversion.
  - Assignment or follow-up Tasks when required.
  - Appointment recommendations when appointment intent exists.

- **Triggers:**
  - Qualification routing.
  - Sales-owner assignment.
  - Qualification expiry monitoring.
  - Nurture workflows.
  - Opportunity conversion workflows.

- **Owned By:**
  - A Sales Consultant, BDC Agent, or dealership assignment queue.

- **Referenced By:**
  - Opportunity
  - Appointment
  - Task
  - Interaction Log
  - Customer Journey
  - Vehicle Match Recommendation
  - AI Agent Run
  - Campaign Attribution

## 8. Domain Events

### Emitted Events

- **QualifiedLeadCreated**  
  Payload: `qualified_lead_id`, `lead_id`, `customer_id`, `qualification_score`, `qualified_at`

- **QualifiedLeadReadyForRouting**  
  Payload: `qualified_lead_id`, `priority_score`, `routing_priority`

- **QualifiedLeadRouted**  
  Payload: `qualified_lead_id`, `owner_id`, `routing_method`, `routed_at`

- **QualifiedLeadAccepted**  
  Payload: `qualified_lead_id`, `owner_id`, `accepted_at`

- **QualifiedLeadNurtureStarted**  
  Payload: `qualified_lead_id`, `nurture_reason`, `next_review_at`

- **QualifiedLeadConverted**  
  Payload: `qualified_lead_id`, `created_opportunity_id`, `converted_at`

- **QualifiedLeadRejected**  
  Payload: `qualified_lead_id`, `rejection_reason`, `reviewed_by`, `rejected_at`

- **QualifiedLeadExpired**  
  Payload: `qualified_lead_id`, `expires_at`, `expiration_reason`

- **QualifiedLeadRequirementsUpdated**  
  Payload: `qualified_lead_id`, `changed_fields`, `updated_at`

### Consumed Events

- **LeadQualified**  
  Creates the Qualified Lead using the validated Lead, resolved Customer, and qualification evidence.

- **CustomerContactPermissionChanged**  
  Suspends routing or outbound engagement when permission becomes restricted or `OPT_OUT`.

- **CustomerIdentityMerged**  
  Updates the linked Customer only through a controlled identity-resolution process.

- **AssignmentAccepted**  
  Moves a routed Qualified Lead to `ACCEPTED`.

- **QualificationRefreshCompleted**  
  Refreshes stale requirements, confidence scores, and expiry dates.

- **OpportunityCreated**  
  Populates `created_opportunity_id` and transitions the Qualified Lead to `CONVERTED`.

- **OpportunityCreationFailed**  
  Preserves the Qualified Lead in its prior active state and records the failure for retry or review.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `vehicle_interest_text`
- `required_features`
- `acceptable_alternatives`
- `trade_in_vehicle_summary`
- `qualification_reason`
- Normalized customer requirements
- Purchase-intent summaries
- Objections and preferences
- Timing and urgency indicators
- Dealership-interaction summaries

### Fields Excluded from Embeddings

- `qualified_lead_id`
- `lead_id`
- `customer_id`
- `owner_id`
- `created_opportunity_id`
- Direct phone numbers or email addresses inherited through source context
- `source_context_snapshot`
- Exact financial identifiers
- Internal Agent identifiers
- Human-reviewer identifiers

> Personally identifiable information and exact commercial values must be supplied through controlled structured context rather than unrestricted semantic retrieval.

### Structured AI Context Fields

The following fields may be provided directly to an authorized Agent as structured context:

- `budget_min_amount`
- `budget_max_amount`
- `currency_code`
- `payment_preference`
- `finance_required`
- `estimated_down_payment_amount`
- `purchase_timeframe`
- `trade_in_intent`
- `test_drive_intent`
- `appointment_intent`

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `qualified_lead_id`
- `customer_id`
- `status`
- `intent_level`
- `purchase_timeframe`
- `preferred_condition`
- `preferred_body_type`
- `routing_priority`
- `preferred_language`

### Confidence Thresholds

- Vehicle make or model extraction requires confidence of at least `0.90`.
- Required-feature extraction requires confidence of at least `0.80`.
- Budget extraction requires confidence of at least `0.90`.
- Purchase-timeframe classification requires confidence of at least `0.85`.
- Finance and trade-in intent require confidence of at least `0.90`.
- Automatic routing recommendations require confidence of at least `0.85`.
- Automatic Opportunity creation requires all mandatory validation rules to pass and confidence of at least `0.95`.

### Human Approval Thresholds

- AI Agents cannot replace a human-verified `customer_id`.
- Conflicting budget, vehicle, finance, or trade-in evidence requires human review.
- Conversion using an expired qualification requires revalidation or explicit approval.
- Qualifications below the configured threshold must create a Human Review Task.
- AI Agents cannot fabricate unknown customer requirements.
- AI Agents cannot change a record from `REJECTED` to an active state without authorized approval.
- VIP or exceptional routing decisions must comply with dealership approval rules.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/qualified-leads`

### Methods

- `GET` — list or search Qualified Leads.
- `POST` — create a Qualified Lead through an authorized qualification service.
- `GET /{id}` — retrieve one Qualified Lead.
- `PATCH /{id}` — update permitted qualification or requirement fields.
- `POST /{id}/ready-for-routing` — validate readiness for routing.
- `POST /{id}/route` — assign the record to a User, team, or queue.
- `POST /{id}/accept` — accept ownership of the record.
- `POST /{id}/nurture` — move the record into nurture.
- `POST /{id}/convert` — create an Opportunity through an idempotent transaction.
- `POST /{id}/reject` — record a reviewed rejection.
- `POST /{id}/refresh` — perform qualification revalidation.
- `DELETE /{id}` — perform a soft delete when permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateQualifiedLeadRequest",
  "type": "object",
  "properties": {
    "lead_id": {
      "type": "string",
      "format": "uuid"
    },
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "intent_level": {
      "type": "string",
      "enum": ["LOW", "MEDIUM", "HIGH", "IMMEDIATE"]
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
    "purchase_purpose": {
      "type": "string",
      "enum": [
        "VEHICLE_PURCHASE",
        "VEHICLE_REPLACEMENT",
        "FLEET_PURCHASE",
        "FINANCE_INQUIRY",
        "TRADE_IN",
        "TEST_DRIVE",
        "VEHICLE_RESERVATION",
        "GENERAL_SALES_CONSULTATION"
      ]
    },
    "vehicle_interest_text": {
      "type": "string",
      "minLength": 1,
      "maxLength": 2000
    },
    "preferred_make": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "preferred_model": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "preferred_condition": {
      "type": ["string", "null"],
      "enum": ["NEW", "USED", "CPO", "DEMO", null]
    },
    "budget_min_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "budget_max_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
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
    "test_drive_intent": {
      "type": "boolean"
    },
    "appointment_intent": {
      "type": "boolean"
    },
    "qualification_score": {
      "type": "number",
      "minimum": 0,
      "maximum": 1
    },
    "confidence_score": {
      "type": "number",
      "minimum": 0,
      "maximum": 1
    },
    "qualification_method": {
      "type": "string",
      "enum": [
        "RULE_BASED",
        "AI_AUTOMATED",
        "AI_ASSISTED",
        "HUMAN_VERIFIED",
        "IMPORTED"
      ]
    },
    "qualification_reason": {
      "type": "string",
      "minLength": 1,
      "maxLength": 2000
    },
    "qualification_evidence": {
      "type": "object"
    }
  },
  "required": [
    "lead_id",
    "customer_id",
    "intent_level",
    "purchase_timeframe",
    "purchase_purpose",
    "vehicle_interest_text",
    "currency_code",
    "payment_preference",
    "finance_required",
    "trade_in_intent",
    "test_drive_intent",
    "appointment_intent",
    "qualification_score",
    "confidence_score",
    "qualification_method",
    "qualification_reason",
    "qualification_evidence"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type QualifiedLead {
  id: ID!
  dealershipId: ID!
  leadId: ID!
  customerId: ID!
  ownerId: ID
  createdOpportunityId: ID
  intentLevel: IntentLevel!
  purchaseTimeframe: PurchaseTimeframe!
  purchasePurpose: PurchasePurpose!
  vehicleInterestText: String!
  preferredMake: String
  preferredModel: String
  preferredCondition: VehicleCondition
  preferredBodyType: String
  preferredFuelType: FuelType
  preferredColor: String
  requiredFeatures: [String!]!
  acceptableAlternatives: [String!]!
  budgetMinAmount: Float
  budgetMaxAmount: Float
  currencyCode: String!
  paymentPreference: PaymentPreference!
  financeRequired: Boolean!
  estimatedDownPaymentAmount: Float
  tradeInIntent: TradeInIntent!
  tradeInVehicleSummary: String
  testDriveIntent: Boolean!
  appointmentIntent: Boolean!
  qualificationScore: Float!
  confidenceScore: Float!
  priorityScore: Float!
  qualificationMethod: QualificationMethod!
  qualificationReason: String!
  routingPriority: RoutingPriority!
  status: QualifiedLeadStatus!
  qualifiedAt: DateTime!
  routedAt: DateTime
  acceptedAt: DateTime
  convertedAt: DateTime
  expiresAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `qualified_leads`
- **Qualification Evidence Table:** `qualified_lead_evidence`
- **Requirements Snapshot Table:** `qualified_lead_requirements`
- **Routing History Table:** `qualified_lead_routing_history`
- **Audit Table:** `qualified_lead_audit_log`

### Indexes

- `idx_qualified_leads_tenant_status (dealership_id, status)`  
  Used for routing, nurture, and conversion queues.

- `idx_qualified_leads_owner_status (dealership_id, owner_id, status)`  
  Used for Sales Consultant and BDC work queues.

- `idx_qualified_leads_customer (dealership_id, customer_id, status)`  
  Used to retrieve the Customer's active qualifications.

- `idx_qualified_leads_lead (lead_id)`  
  Used to trace the Qualified Lead back to its original Lead.

- `idx_qualified_leads_priority (dealership_id, routing_priority, priority_score DESC)`  
  Used for ordered routing and urgent-response queues.

- `idx_qualified_leads_expiry (dealership_id, expires_at)`  
  Used by qualification-expiry monitoring Jobs.

- `idx_qualified_leads_timeframe (dealership_id, purchase_timeframe, status)`  
  Used for sales and nurture segmentation.

- `idx_qualified_leads_opportunity (created_opportunity_id)`  
  Used to validate conversion integrity.

### Unique Constraints

- `UQ_qualified_lead_active_source (lead_id)`  
  Applies to non-terminal Qualified Leads.

- `UQ_qualified_lead_opportunity (created_opportunity_id)`  
  Applies when `created_opportunity_id` is not null.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `lead_id` → `leads(id)`
- `customer_id` → `customers(id)`
- `owner_id` → `users(id)` — nullable
- `created_opportunity_id` → `opportunities(id)` — nullable
- `qualifying_agent_id` → `ai_agents(id)` — nullable
- `qualification_reviewed_by` → `users(id)` — nullable

### Database Constraints

- `budget_min_amount <= budget_max_amount`
- `qualification_score BETWEEN 0.00 AND 1.00`
- `confidence_score BETWEEN 0.00 AND 1.00`
- `priority_score BETWEEN 0.00 AND 100.00`
- `expires_at > qualified_at`
- `converted_at >= qualified_at`
- `created_opportunity_id IS NOT NULL` when `status = CONVERTED`

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical records by `qualified_at`.
- Every supporting table must preserve the same tenant-partitioning strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **BDC Agent:** Read/Write access to assigned Qualified Leads and authorized dealership queues.
- **Sales Consultant:** Read/Write access to Qualified Leads assigned to them.
- **Sales Manager:** Read/Write and routing access to all Qualified Leads in the matching dealership.
- **Marketing User:** Read-only access to aggregated source and qualification metrics, without unrestricted Customer PII.
- **Finance User:** Read access only to finance-related intent fields when required for approved workflows.
- **AI Qualification Agent:** Service Account access for scoring, classification, and approved state transitions.
- **Routing Service:** Access limited to routing fields, workload data, and permitted assignment operations.
- **Opportunity Service:** Create-conversion access through an idempotent controlled transaction.

### PII Classification

- **Level:** `CRITICAL_PII — INHERITED`
- The Qualified Lead references a Customer and Lead that may contain direct personally identifiable information.

### Protected Fields

- `source_context_snapshot`
- `qualification_evidence`
- `requirements_snapshot`
- `preferred_contact_time`
- `preferred_dealership_location`
- Any name, phone number, email address, or identity data included in imported source context

### Commercially Sensitive Fields

- `budget_min_amount`
- `budget_max_amount`
- `estimated_down_payment_amount`
- `finance_required`
- `payment_preference`
- `priority_score`
- Internal routing and qualification evidence

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for database volumes and backups.
- **Column-Level Protection:** Qualification snapshots, financial-intent fields, and imported source context require encryption, tokenization, or an equivalent approved protection method.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every lifecycle-state transition must generate an immutable audit entry.
- Qualification creation must preserve:
  - Scoring-rule version
  - AI model version
  - Confidence thresholds
  - Qualification evidence
  - Human-review decision when applicable

- Routing and reassignment must record:
  - Previous owner
  - New owner
  - Routing rule
  - Actor
  - Timestamp
  - Reason

- Opportunity conversion must preserve:
  - Conversion request ID
  - Idempotency key
  - Created Opportunity ID
  - Actor
  - Timestamp
  - Complete requirements snapshot

- Human overrides must retain both the original AI decision and the final human decision.
- Access to qualification evidence and source snapshots must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant routing, Customer matching, vehicle matching, semantic retrieval, and exports are prohibited.
- AI Agents and background Jobs must receive tenant scope through signed execution context.
- Linked Lead, Customer, User, and Opportunity records must belong to the same tenant.

### Retention and Deletion

- Soft deletion is the operational default.
- A Converted Qualified Lead must remain available while its Opportunity, Deal, or audit records exist.
- Legally approved deletion requests must purge or anonymize inherited PII across:
  - Qualified Lead records
  - Qualification evidence
  - Requirements snapshots
  - Vector stores
  - Audit references
  - Analytics stores
  - Backups, according to the approved retention policy
