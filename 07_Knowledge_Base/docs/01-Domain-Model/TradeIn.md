# Trade-In

## 1. Object Purpose

### Business Purpose

The Trade-In object represents a Customer-owned vehicle that may be exchanged as part of a proposed or active vehicle-sales transaction.

It provides the dealership with a controlled process for:

- Recording the Customer's current vehicle.
- Verifying ownership and vehicle identity.
- Inspecting physical and mechanical condition.
- Estimating market and wholesale value.
- Recording lender payoff obligations.
- Calculating positive or negative equity.
- Approving the final trade-in allowance.
- Supporting Quotation and Deal calculations.
- Tracking acquisition, title transfer, and inventory intake.

A Trade-In is not the same as dealership inventory. It becomes dealership-owned inventory only after the related Deal is completed, ownership transfer is legally valid, and the vehicle is accepted into inventory through an authorized process.

### System Purpose

The Trade-In object is the canonical appraisal and acquisition aggregate for Customer-owned vehicles offered to the dealership.

It connects:

- Customer
- Opportunity
- Deal
- Quotation
- Vehicle
- Finance Application
- Appraisal
- Inspection
- Payoff Verification
- Title and Ownership Documents
- Inventory Intake

The object preserves separate values for:

- Estimated market value.
- Actual cash value.
- Customer allowance.
- Outstanding finance payoff.
- Positive or negative equity.
- Reconditioning estimate.
- Final acquisition cost.

It supplies authoritative trade-in information to Quotation, Deal, finance, profitability, and inventory workflows while preventing AI Agents or unauthorized Users from presenting unapproved valuations as binding offers.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `trade_in_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — required)
  - `opportunity_id` (UUIDv4 — required)
  - `deal_id` (UUIDv4 — optional until Deal creation)
  - `appraiser_id` (UUIDv4 — optional until assignment)
  - `inspection_id` (UUIDv4 — optional)
  - `payoff_verification_id` (UUIDv4 — optional)
  - `acquired_vehicle_id` (UUIDv4 — optional; populated after inventory intake)
  - `approved_by` (UUIDv4 — optional)

### Vehicle Identity

- `vin`
- `registration_number`
- `make`
- `model`
- `trim`
- `year`
- `body_type`
- `exterior_color`
- `fuel_type`
- `transmission`
- `drivetrain`
- `mileage`
- `country_of_registration`

### Ownership and Finance

- `registered_owner_name`
- `ownership_verified`
- `ownership_document_status`
- `title_status`
- `lien_status`
- `lender_name`
- `loan_account_reference`
- `payoff_required`
- `payoff_amount`
- `payoff_valid_until`
- `payoff_verified_at`

### Condition and Inspection

- `overall_condition`
- `mechanical_condition`
- `body_condition`
- `interior_condition`
- `tire_condition`
- `service_history_status`
- `accident_history_status`
- `flood_damage_status`
- `odometer_verification_status`
- `warning_lights_present`
- `keys_available_count`
- `inspection_notes`
- `damage_items`
- `required_repairs`

### Valuation

- `customer_expected_value_amount`
- `market_value_amount`
- `wholesale_value_amount`
- `auction_value_amount`
- `estimated_reconditioning_amount`
- `actual_cash_value_amount`
- `trade_in_allowance_amount`
- `over_allowance_amount`
- `payoff_amount`
- `net_equity_amount`
- `final_acquisition_cost_amount`
- `currency_code`

### Valuation Evidence

- `valuation_method`
- `valuation_source`
- `market_comparables`
- `valuation_confidence_score`
- `appraisal_report_url`
- `inspection_media`
- `valuation_rule_version`

### Approval Fields

- `approval_required`
- `approval_status`
- `approval_reason`
- `approved_by`
- `approved_at`
- `approval_notes`

### Lifecycle Fields

- `status`
- `appointment_required`
- `inspection_scheduled_at`
- `inspection_completed_at`
- `offer_presented_at`
- `offer_accepted_at`
- `offer_rejected_at`
- `acquired_at`
- `inventory_intake_completed_at`

### Computed Fields

- `vehicle_age_years`
- `mileage_per_year`
- `condition_adjustment_amount`
- `net_equity_amount`
- `negative_equity_amount`
- `over_allowance_amount`
- `estimated_trade_gross_impact`
- `days_since_appraisal`
- `appraisal_expired`
- `document_completion_percentage`

### Governance and Lifecycle

- **Vehicle Snapshot:** `vehicle_snapshot` (JSONB)
- **Inspection Snapshot:** `inspection_snapshot` (JSONB)
- **Valuation Snapshot:** `valuation_snapshot` (JSONB)
- **Payoff Snapshot:** `payoff_snapshot` (JSONB)
- **Ownership Snapshot:** `ownership_snapshot` (JSONB)
- **Approval Evidence:** `approval_evidence` (JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `appraised_by`
  - `approved_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `inspection_scheduled_at`
  - `inspection_completed_at`
  - `appraised_at`
  - `approved_at`
  - `offer_presented_at`
  - `offer_accepted_at`
  - `offer_rejected_at`
  - `payoff_verified_at`
  - `acquired_at`
  - `inventory_intake_completed_at`
  - `expires_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| trade_in_id | UUID | Unique canonical identifier for the Trade-In. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant evaluating the Trade-In. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Customer offering the vehicle. | Yes | N/A | Must reference an active Customer in the same dealership | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| opportunity_id | UUID | Opportunity for which the Trade-In is being evaluated. | Yes | N/A | Must belong to the same Customer and dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| deal_id | UUID | Deal that ultimately includes the Trade-In. | No | Null | Must reference a Deal for the same Customer and Opportunity | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| vin | String | Vehicle Identification Number of the Customer-owned vehicle. | Yes | N/A | Exactly 17 characters and must pass VIN validation | KMHJN81BP7U123456 | At least 0.99 |
| registration_number | String | Current government registration identifier. | No | Null | Must follow the applicable regional format | ABC-1234 | At least 0.95 |
| make | String | Vehicle manufacturer. | Yes | N/A | Must match the approved OEM dictionary | Hyundai | At least 0.90 |
| model | String | Vehicle model. | Yes | N/A | Must belong to the selected make | Tucson | At least 0.90 |
| trim | String | Vehicle equipment or trim level. | No | Null | Maximum 100 characters | Premium | At least 0.85 |
| year | Integer | Vehicle model year. | Yes | N/A | Between 1980 and the current year plus one | 2020 | At least 0.99 |
| mileage | Integer | Verified or reported odometer reading. | Yes | N/A | Must be zero or greater | 85000 | At least 0.95 |
| ownership_verified | Boolean | Indicates whether the Customer's ownership was verified. | Yes | false | Must be true before acquisition | true | Authoritative evidence |
| title_status | Enum | Current legal title condition. | Yes | UNKNOWN | Must match TradeInTitleStatus ENUM | CLEAR | At least 0.99 |
| lien_status | Enum | Indicates whether a lender or other lien exists. | Yes | UNKNOWN | Must match TradeInLienStatus ENUM | ACTIVE_LIEN | At least 0.99 |
| payoff_required | Boolean | Indicates whether lender payoff is required. | Yes | false | Must be true when an active lien exists | true | System or verified human |
| payoff_amount | Decimal | Verified amount required to settle the existing lien. | No | 0.00 | Required when payoff_required is true | 200000.00 | Verified external evidence |
| payoff_valid_until | Date | Expiration date of the lender payoff statement. | No | Null | Must be later than the verification date | 2026-08-15 | Verified external evidence |
| overall_condition | Enum | Overall physical and operational classification. | Yes | NOT_INSPECTED | Must match TradeInCondition ENUM | GOOD | Human inspection |
| accident_history_status | Enum | Known accident-history classification. | Yes | UNKNOWN | Must match AccidentHistoryStatus ENUM | REPORTED_MINOR | Verified source or human |
| odometer_verification_status | Enum | Result of odometer verification. | Yes | NOT_VERIFIED | Must match OdometerVerificationStatus ENUM | VERIFIED | Authoritative evidence |
| customer_expected_value_amount | Decimal | Value expected or requested by the Customer. | No | Null | Must be zero or greater | 700000.00 | Customer-provided |
| market_value_amount | Decimal | Estimated retail-market value. | No | 0.00 | Must be zero or greater | 720000.00 | Valuation source |
| wholesale_value_amount | Decimal | Estimated wholesale or auction-equivalent value. | No | 0.00 | Must be zero or greater | 610000.00 | Valuation source |
| estimated_reconditioning_amount | Decimal | Estimated cost to prepare the Vehicle for resale. | Yes | 0.00 | Must be zero or greater | 35000.00 | Human or approved system |
| actual_cash_value_amount | Decimal | Approved dealership acquisition value before Customer allowance adjustments. | No | 0.00 | Requires completed appraisal and approval when applicable | 600000.00 | Approved appraisal |
| trade_in_allowance_amount | Decimal | Amount credited to the Customer in the transaction. | No | 0.00 | Must be separately approved from actual cash value | 650000.00 | Authorized human |
| over_allowance_amount | Decimal | Allowance exceeding the actual cash value. | No | 0.00 | Must be system-computed | 50000.00 | System-computed |
| net_equity_amount | Decimal | Trade-In allowance minus verified payoff. | No | 0.00 | Must be system-computed | 450000.00 | System-computed |
| valuation_method | Enum | Method used to determine value. | Yes | MANUAL_APPRAISAL | Must match TradeInValuationMethod ENUM | MARKET_COMPARABLES | N/A |
| valuation_confidence_score | Decimal | Confidence in the valuation result. | Yes | 0.00 | Must remain between 0.00 and 1.00 | 0.88 | System-computed |
| approval_status | Enum | Approval state of the final valuation and allowance. | Yes | NOT_REQUIRED | Must match TradeInApprovalStatus ENUM | APPROVED | System-controlled |
| status | Enum | Current lifecycle state of the Trade-In. | Yes | CREATED | Must match TradeInStatus ENUM | APPRAISED | At least 0.99 |
| acquired_vehicle_id | UUID | Inventory Vehicle created after acquisition. | No | Null | Required after inventory intake is completed | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| expires_at | Timestamp | Time after which the appraisal must be refreshed. | No | Policy-defined | Must be later than appraised_at | 2026-08-08T12:00:00Z | System-calculated |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increment after every successful update | 5 | System-controlled |

## 4. Enumerations

### TradeInStatus

- **CREATED:** Initial Trade-In record created.
- **AWAITING_INSPECTION:** Physical inspection has not yet been completed.
- **INSPECTION_SCHEDULED:** Inspection Appointment has been scheduled.
- **INSPECTED:** Physical and mechanical findings were recorded.
- **VALUATION_IN_PROGRESS:** Market, condition, payoff, and ownership data are being evaluated.
- **PENDING_PAYOFF:** A current lender payoff statement is required.
- **PENDING_DOCUMENTS:** Ownership, registration, title, or identity documents are incomplete.
- **PENDING_APPROVAL:** The valuation or allowance requires authorized approval.
- **APPRAISED:** An approved actual cash value is available.
- **OFFER_PRESENTED:** A trade-in allowance was presented to the Customer.
- **CUSTOMER_ACCEPTED:** The Customer accepted the proposed allowance.
- **CUSTOMER_REJECTED:** The Customer rejected the proposed allowance.
- **EXPIRED:** The appraisal is no longer current.
- **ACQUIRED:** Ownership transfer and Deal requirements were completed.
- **INVENTORY_INTAKE:** The acquired Vehicle is being prepared for dealership inventory.
- **COMPLETED:** Inventory intake and accounting handoff were completed.
- **CANCELLED:** The Trade-In process ended before acquisition.

### TradeInCondition

- NOT_INSPECTED
- EXCELLENT
- GOOD
- FAIR
- POOR
- NON_RUNNING
- SALVAGE

### TradeInTitleStatus

- UNKNOWN
- CLEAR
- FINANCED
- DUPLICATE_TITLE_REQUIRED
- LOST_TITLE
- SALVAGE
- REBUILT
- EXPORT_RESTRICTED
- BLOCKED

### TradeInLienStatus

- UNKNOWN
- NO_LIEN
- ACTIVE_LIEN
- MULTIPLE_LIENS
- LIEN_RELEASE_PENDING
- LIEN_RELEASED
- DISPUTED

### OwnershipDocumentStatus

- NOT_PROVIDED
- PARTIAL
- PROVIDED
- VERIFIED
- REJECTED
- EXPIRED

### AccidentHistoryStatus

- UNKNOWN
- NONE_REPORTED
- REPORTED_MINOR
- REPORTED_MAJOR
- STRUCTURAL_DAMAGE
- TOTAL_LOSS
- FLOOD_DAMAGE

### OdometerVerificationStatus

- NOT_VERIFIED
- VERIFIED
- DISCREPANCY
- ROLLBACK_SUSPECTED
- NOT_READABLE
- EXEMPT

### TradeInValuationMethod

- MANUAL_APPRAISAL
- MARKET_COMPARABLES
- AUCTION_DATA
- WHOLESALE_GUIDE
- AI_ASSISTED
- EXTERNAL_PROVIDER
- COMBINED

### TradeInApprovalStatus

- NOT_REQUIRED
- REQUIRED
- PENDING
- APPROVED
- REJECTED
- EXPIRED

### TradeInRejectionReason

- VALUE_TOO_LOW
- CUSTOMER_CHANGED_MIND
- PAYOFF_TOO_HIGH
- DOCUMENTS_UNAVAILABLE
- OWNERSHIP_NOT_VERIFIED
- VEHICLE_CONDITION_DISPUTED
- CUSTOMER_SOLD_ELSEWHERE
- OPPORTUNITY_CANCELLED
- OTHER

### TradeInCancellationReason

- CUSTOMER_WITHDREW
- OPPORTUNITY_LOST
- DEAL_CANCELLED
- OWNERSHIP_FAILED
- TITLE_BLOCKED
- FRAUD_SUSPECTED
- VEHICLE_NOT_PRESENTED
- DUPLICATE_TRADE_IN
- OTHER

## 5. Validation Rules

### Business Rules

- A Trade-In must belong to a resolved Customer and active Opportunity.
- One physical Vehicle must not have more than one active Trade-In appraisal within the same dealership unless an authorized reappraisal is created.
- The VIN must be verified before a final acquisition value is approved.
- A final actual cash value cannot be issued without a completed inspection unless an authorized remote-appraisal policy permits it.
- Customer expectations must remain separate from dealership valuation.
- Actual cash value must remain separate from the Customer-facing trade-in allowance.
- Any amount above actual cash value must be recorded as over-allowance.
- A Trade-In with an active lien requires a current verified payoff before final Deal completion.
- Ownership must be verified before acquisition.
- A salvage, rebuilt, flood-damaged, structurally damaged, or disputed-title Vehicle requires enhanced approval.
- An appraisal must expire according to dealership policy or after a material condition, mileage, payoff, or market change.
- An expired appraisal cannot be used in a new Quotation or Deal without revalidation.
- Acceptance of a trade-in offer does not transfer ownership by itself.
- The Trade-In becomes dealership inventory only after legal acquisition and inventory intake.
- AI-generated valuation recommendations are non-binding until approved by an authorized appraiser or manager.

### Technical Rules

- VIN values must be normalized and validated before database commit.
- Monetary calculations must use fixed decimal precision.
- `record_version` must increment after every successful update.
- Inspection, valuation, ownership, and payoff snapshots must be versioned.
- Approved appraisal snapshots must become immutable.
- Market-comparable records must preserve their source, timestamp, and selection criteria.
- Payoff information must preserve lender source, verification timestamp, and expiry date.
- Status changes must generate immutable history and audit records.
- Appraisal-expiry Jobs must use authoritative server time.
- Trade-In conversion into inventory must use an idempotent operation.
- External valuation-provider responses must store the request, response, provider, and correlation identifier.

### Data Constraints

- `year` must be within the permitted vehicle-year range.
- `mileage` cannot be negative.
- All valuation amounts must use the same `currency_code`.
- `valuation_confidence_score` must remain between `0.00` and `1.00`.
- `over_allowance_amount` must equal:

  `trade_in_allowance_amount - actual_cash_value_amount`

  when the allowance is greater than actual cash value; otherwise it must equal `0.00`.

- `net_equity_amount` must equal:

  `trade_in_allowance_amount - payoff_amount`

- `negative_equity_amount` must equal the absolute value of negative net equity; otherwise it must equal `0.00`.
- `payoff_valid_until` must not precede `payoff_verified_at`.
- `expires_at` must be later than `appraised_at`.
- `acquired_at` cannot precede `offer_accepted_at`.
- `inventory_intake_completed_at` cannot precede `acquired_at`.
- `acquired_vehicle_id` must be null until acquisition and intake requirements pass.

### Referential Integrity

- All linked records must belong to the same `dealership_id`.
- `opportunity_id` must belong to `customer_id`.
- `deal_id`, when populated, must reference the same Customer and Opportunity.
- `inspection_id` must reference this exact Trade-In.
- `payoff_verification_id` must reference this exact Trade-In.
- `acquired_vehicle_id` must represent the same VIN.
- A Trade-In cannot be hard-deleted while referenced by a Quotation, Deal, Finance Application, Payment, inventory record, or audit log.
- Circular links between acquired inventory Vehicles and unrelated Trade-Ins are prohibited.

### Human Approval Requirements

- Final actual cash value requires an authorized Appraiser or Manager when dealership policy requires it.
- Over-allowance requires Sales Manager or GSM approval.
- Negative-equity treatment requires Finance or Sales Manager approval.
- Structural damage, salvage title, rebuilt title, flood history, odometer discrepancy, or disputed ownership requires enhanced human review.
- Manual payoff overrides require Finance Manager approval and supporting evidence.
- AI Agents cannot approve valuations, allowances, ownership, payoff, title condition, or acquisition.
- AI Agents cannot move a Trade-In to `ACQUIRED` or `COMPLETED`.
- Conflicting VIN, mileage, ownership, payoff, or condition evidence must create a Human Review Task.

## 6. State Machine

### Allowed States

- CREATED
- AWAITING_INSPECTION
- INSPECTION_SCHEDULED
- INSPECTED
- VALUATION_IN_PROGRESS
- PENDING_PAYOFF
- PENDING_DOCUMENTS
- PENDING_APPROVAL
- APPRAISED
- OFFER_PRESENTED
- CUSTOMER_ACCEPTED
- CUSTOMER_REJECTED
- EXPIRED
- ACQUIRED
- INVENTORY_INTAKE
- COMPLETED
- CANCELLED

### Allowed Transitions

- CREATED → AWAITING_INSPECTION
- CREATED → CANCELLED
- AWAITING_INSPECTION → INSPECTION_SCHEDULED
- AWAITING_INSPECTION → INSPECTED
- AWAITING_INSPECTION → CANCELLED
- INSPECTION_SCHEDULED → INSPECTED
- INSPECTION_SCHEDULED → AWAITING_INSPECTION
- INSPECTION_SCHEDULED → CANCELLED
- INSPECTED → VALUATION_IN_PROGRESS
- INSPECTED → PENDING_DOCUMENTS
- INSPECTED → CANCELLED
- VALUATION_IN_PROGRESS → PENDING_PAYOFF
- VALUATION_IN_PROGRESS → PENDING_DOCUMENTS
- VALUATION_IN_PROGRESS → PENDING_APPROVAL
- VALUATION_IN_PROGRESS → APPRAISED
- VALUATION_IN_PROGRESS → CANCELLED
- PENDING_PAYOFF → VALUATION_IN_PROGRESS
- PENDING_PAYOFF → PENDING_APPROVAL
- PENDING_PAYOFF → CANCELLED
- PENDING_DOCUMENTS → VALUATION_IN_PROGRESS
- PENDING_DOCUMENTS → PENDING_APPROVAL
- PENDING_DOCUMENTS → CANCELLED
- PENDING_APPROVAL → APPRAISED
- PENDING_APPROVAL → VALUATION_IN_PROGRESS
- PENDING_APPROVAL → CANCELLED
- APPRAISED → OFFER_PRESENTED
- APPRAISED → EXPIRED
- APPRAISED → VALUATION_IN_PROGRESS
- APPRAISED → CANCELLED
- OFFER_PRESENTED → CUSTOMER_ACCEPTED
- OFFER_PRESENTED → CUSTOMER_REJECTED
- OFFER_PRESENTED → EXPIRED
- OFFER_PRESENTED → VALUATION_IN_PROGRESS
- CUSTOMER_REJECTED → VALUATION_IN_PROGRESS
- CUSTOMER_REJECTED → CANCELLED
- CUSTOMER_ACCEPTED → ACQUIRED
- CUSTOMER_ACCEPTED → EXPIRED
- CUSTOMER_ACCEPTED → CANCELLED
- EXPIRED → AWAITING_INSPECTION
- EXPIRED → VALUATION_IN_PROGRESS
- EXPIRED → CANCELLED
- ACQUIRED → INVENTORY_INTAKE
- INVENTORY_INTAKE → COMPLETED

### Forbidden Transitions

- CREATED → APPRAISED
- CREATED → OFFER_PRESENTED
- CREATED → CUSTOMER_ACCEPTED
- CREATED → ACQUIRED
- AWAITING_INSPECTION → APPRAISED
- INSPECTED → OFFER_PRESENTED
- PENDING_APPROVAL → CUSTOMER_ACCEPTED
- APPRAISED → ACQUIRED
- OFFER_PRESENTED → ACQUIRED
- CUSTOMER_REJECTED → ACQUIRED
- EXPIRED → ACQUIRED
- CANCELLED → APPRAISED
- CANCELLED → ACQUIRED
- COMPLETED → APPRAISED
- COMPLETED → CANCELLED

### Entry Conditions

- To enter `AWAITING_INSPECTION`:
  - Customer, Opportunity, and minimum Vehicle identity must be recorded.
  - The VIN must be structurally valid.

- To enter `INSPECTION_SCHEDULED`:
  - A valid date, location, and assigned Appraiser or inspection resource must exist.

- To enter `INSPECTED`:
  - Vehicle identity must be confirmed.
  - Mileage and condition findings must be recorded.
  - Required inspection evidence must be attached.
  - Critical inspection sections cannot remain incomplete.

- To enter `VALUATION_IN_PROGRESS`:
  - The inspection must be completed or an approved remote-appraisal exception must exist.
  - Market and valuation sources must be available.

- To enter `PENDING_PAYOFF`:
  - An active lien or finance obligation must be identified.
  - A current verified payoff amount must be missing or expired.

- To enter `PENDING_DOCUMENTS`:
  - Ownership, registration, title, identity, or supporting documents must be incomplete or invalid.

- To enter `PENDING_APPROVAL`:
  - A provisional valuation must exist.
  - One or more approval conditions must be triggered.

- To enter `APPRAISED`:
  - Inspection and valuation must be complete.
  - Ownership and title risks must be documented.
  - Required payoff information must be available.
  - Required approvals must be completed.
  - `actual_cash_value_amount`, `appraised_at`, and `expires_at` must be populated.

- To enter `OFFER_PRESENTED`:
  - An approved `trade_in_allowance_amount` must exist.
  - The appraisal must not be expired.
  - The Customer-facing offer must not expose restricted internal valuation data.

- To enter `CUSTOMER_ACCEPTED`:
  - Verifiable Customer acceptance must reference the current offer and valuation version.
  - The appraisal and payoff information must remain valid.

- To enter `CUSTOMER_REJECTED`:
  - A Customer rejection signal must be documented.
  - A standardized rejection reason should be recorded when known.

- To enter `EXPIRED`:
  - The appraisal expiry time must have passed, or a material change must invalidate the valuation.

- To enter `ACQUIRED`:
  - The Customer must have accepted the Trade-In terms.
  - The related Deal must satisfy its required acquisition conditions.
  - Ownership transfer must be legally valid.
  - Required payoff settlement arrangements must exist.
  - Title and document blocks must be cleared.
  - `acquired_at` must be populated.

- To enter `INVENTORY_INTAKE`:
  - The vehicle must be legally acquired.
  - Physical possession and intake inspection must be confirmed.
  - Inventory-creation workflow must start.

- To enter `COMPLETED`:
  - `acquired_vehicle_id` must exist.
  - Inventory, accounting, title, and DMS intake must reconcile.
  - Required audit evidence must be complete.

- To enter `CANCELLED`:
  - A valid cancellation reason and responsible actor must be recorded.
  - Any scheduled inspections or active acquisition actions must be closed.

### Exit Conditions

- A Trade-In cannot exit `CREATED` without basic Customer, Opportunity, and Vehicle identity.
- A Trade-In cannot exit `INSPECTED` toward valuation without complete inspection evidence.
- A Trade-In cannot exit `PENDING_PAYOFF` until a valid payoff is obtained or the lien is disproven.
- A Trade-In cannot exit `PENDING_DOCUMENTS` until required documents are supplied or an authorized cancellation occurs.
- A Trade-In cannot exit `PENDING_APPROVAL` without approval, revision, or cancellation.
- A Trade-In cannot exit `APPRAISED` toward offer presentation after expiry.
- A Trade-In cannot exit `CUSTOMER_ACCEPTED` toward acquisition while ownership, title, payoff, or Deal blocks remain.
- A Trade-In cannot exit `ACQUIRED` without beginning controlled inventory intake.
- A `COMPLETED` Trade-In cannot return to an appraisal state; a separate correction or unwind process is required.
- A `CANCELLED` Trade-In cannot return to an active state; a new Trade-In record must be created.

### Terminal States

- **CUSTOMER_REJECTED:** The Customer declined the current Trade-In offer, unless a revised appraisal is started.
- **COMPLETED:** Acquisition and inventory intake were completed.
- **CANCELLED:** The Trade-In process ended before acquisition.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Customer identified by `customer_id`.
  - Active Opportunity identified by `opportunity_id`.
  - Applicable inspection, valuation, title, ownership, and payoff rules.

- **Consumes:**
  - Customer-provided vehicle information.
  - Vehicle inspection findings.
  - VIN-decoding and vehicle-history data.
  - Market-comparable and auction data.
  - Wholesale valuation guides.
  - Ownership and registration documents.
  - Lender payoff statements.
  - Opportunity, Quotation, and Deal commercial context.

- **Produces:**
  - Approved appraisal value.
  - Customer-facing Trade-In allowance.
  - Positive or negative equity calculation.
  - Trade-In commercial snapshot.
  - Vehicle-acquisition requirements.
  - Inventory-intake payload after acquisition.
  - Trade-In profitability and valuation analytics.

- **Creates:**
  - Inspection Appointment.
  - Appraisal and approval requests.
  - Payoff-verification request.
  - Document-verification Task.
  - Inventory Vehicle record after completed acquisition.
  - Accounting and DMS intake instructions.

- **Triggers:**
  - Physical inspection workflow.
  - Vehicle-history verification.
  - Payoff verification.
  - Valuation and approval workflow.
  - Quotation recalculation.
  - Deal commercial recalculation.
  - Ownership-transfer workflow.
  - Inventory-intake workflow.

- **Owned By:**
  - An authorized Appraiser, Used Vehicle Manager, or assigned dealership User.

- **Referenced By:**
  - Opportunity
  - Quotation
  - Deal
  - Finance Application
  - Vehicle Inspection
  - Payoff Verification
  - Appointment
  - Document Vault
  - Inventory Vehicle
  - Accounting Entry
  - AI Agent Run

- **Transforms Into:**
  - A dealership-owned Vehicle record only after the Trade-In reaches `ACQUIRED` and controlled inventory intake succeeds.

## 8. Domain Events

### Emitted Events

- **TradeInCreated**  
  Payload: `trade_in_id`, `customer_id`, `opportunity_id`, `vin`, `created_at`

- **TradeInInspectionScheduled**  
  Payload: `trade_in_id`, `inspection_id`, `inspection_scheduled_at`, `appraiser_id`

- **TradeInInspected**  
  Payload: `trade_in_id`, `inspection_id`, `mileage`, `overall_condition`, `inspection_completed_at`

- **TradeInValuationStarted**  
  Payload: `trade_in_id`, `valuation_method`, `started_at`

- **TradeInPayoffRequested**  
  Payload: `trade_in_id`, `lender_name`, `requested_at`

- **TradeInPayoffVerified**  
  Payload: `trade_in_id`, `payoff_amount`, `payoff_valid_until`, `payoff_verified_at`

- **TradeInApprovalRequested**  
  Payload: `trade_in_id`, `approval_reason`, `requested_by`, `requested_at`

- **TradeInAppraised**  
  Payload: `trade_in_id`, `actual_cash_value_amount`, `valuation_confidence_score`, `appraised_at`, `expires_at`

- **TradeInOfferPresented**  
  Payload: `trade_in_id`, `trade_in_allowance_amount`, `net_equity_amount`, `offer_presented_at`

- **TradeInOfferAccepted**  
  Payload: `trade_in_id`, `customer_id`, `offer_version`, `offer_accepted_at`

- **TradeInOfferRejected**  
  Payload: `trade_in_id`, `rejection_reason`, `offer_rejected_at`

- **TradeInAppraisalExpired**  
  Payload: `trade_in_id`, `expires_at`, `expired_at`

- **TradeInAcquired**  
  Payload: `trade_in_id`, `deal_id`, `vin`, `acquired_at`

- **TradeInInventoryIntakeStarted**  
  Payload: `trade_in_id`, `vin`, `started_at`

- **TradeInInventoryIntakeCompleted**  
  Payload: `trade_in_id`, `acquired_vehicle_id`, `inventory_intake_completed_at`

- **TradeInCancelled**  
  Payload: `trade_in_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

### Consumed Events

- **OpportunityTradeInRequested**  
  Creates the Trade-In record for the Customer and Opportunity.

- **AppointmentScheduled**  
  Populates the Trade-In inspection schedule.

- **VehicleInspectionCompleted**  
  Supplies condition, mileage, damage, and repair findings.

- **VehicleHistoryRetrieved**  
  Supplies accident, title, odometer, and ownership-risk evidence.

- **ExternalValuationReceived**  
  Supplies market, auction, or wholesale valuation results.

- **PayoffStatementReceived**  
  Updates the verified payoff amount and validity date.

- **QuotationCreated**  
  Links the current Trade-In assumptions to a Quotation.

- **QuotationSuperseded**  
  Revalidates whether the Trade-In values remain current.

- **DealCreated**  
  Populates `deal_id` when the transaction includes this Trade-In.

- **DealCancelled**  
  Cancels or releases acquisition activities when the Trade-In was not acquired.

- **DealDelivered**  
  Permits final Trade-In acquisition when all ownership-transfer requirements pass.

- **InventoryVehicleCreated**  
  Populates `acquired_vehicle_id` and permits completion.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `inspection_notes`
- `damage_items`
- `required_repairs`
- Vehicle-condition summaries
- Customer valuation objections
- Appraiser notes
- Market-comparable summaries
- Accident-history summaries
- Trade-In negotiation summaries
- Acquisition and intake notes

### Fields Excluded from Embeddings

- `trade_in_id`
- `customer_id`
- `opportunity_id`
- `deal_id`
- `acquired_vehicle_id`
- `registered_owner_name`
- `registration_number`
- `loan_account_reference`
- `payoff_amount`
- Ownership documents
- Identity documents
- Lender statements
- `ownership_snapshot`
- `payoff_snapshot`
- Exact internal valuation limits
- Internal approval evidence

> Personally identifiable information, ownership evidence, lender information, and restricted valuation data must be supplied only through authorized structured context.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `make`
- `model`
- `year`
- `trim`
- `mileage`
- `overall_condition`
- `accident_history_status`
- `title_status`
- `lien_status`
- `valuation_method`
- `market_value_amount`
- `wholesale_value_amount`
- `estimated_reconditioning_amount`
- `status`
- `expires_at`

Restricted internal Agents may additionally receive:

- `actual_cash_value_amount`
- `trade_in_allowance_amount`
- `over_allowance_amount`
- `payoff_amount`
- `net_equity_amount`
- Approval requirements

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `trade_in_id`
- `customer_id`
- `opportunity_id`
- `deal_id`
- `vin`
- `status`
- `overall_condition`
- `title_status`
- `lien_status`
- `valuation_method`

### Confidence Thresholds

- VIN extraction requires confidence of at least `0.99`.
- Mileage extraction requires confidence of at least `0.95`.
- Make and model extraction requires confidence of at least `0.90`.
- Damage classification requires confidence of at least `0.90`.
- Ownership-document classification requires confidence of at least `0.99`.
- Payoff-document extraction requires confidence of at least `0.99`.
- AI-assisted valuation recommendations require confidence of at least `0.90`.
- No AI valuation may become an approved actual cash value without the required human approval.

### Human Approval Thresholds

- AI Agents cannot approve actual cash value or Trade-In allowance.
- AI Agents cannot verify ownership, title, lien release, or payoff conclusively without authoritative evidence.
- AI Agents cannot move a Trade-In to `ACQUIRED`, `INVENTORY_INTAKE`, or `COMPLETED`.
- Structural damage, flood history, salvage title, odometer discrepancy, or fraud indicators require human review.
- Over-allowance and negative-equity scenarios require authorized approval.
- Conflicting VIN, mileage, condition, title, or payoff information must create a Human Review Task.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/trade-ins`

### Methods

- `GET` — list or search Trade-Ins.
- `POST` — create a Trade-In record.
- `GET /{id}` — retrieve one Trade-In.
- `PATCH /{id}` — update permitted Trade-In fields.
- `POST /{id}/schedule-inspection` — schedule the physical inspection.
- `POST /{id}/record-inspection` — record inspection findings.
- `POST /{id}/start-valuation` — begin valuation.
- `POST /{id}/request-payoff` — request lender payoff verification.
- `POST /{id}/submit-for-approval` — request appraisal or allowance approval.
- `POST /{id}/approve` — approve the appraisal.
- `POST /{id}/present-offer` — present the approved allowance to the Customer.
- `POST /{id}/accept-offer` — record verified Customer acceptance.
- `POST /{id}/reject-offer` — record Customer rejection.
- `POST /{id}/refresh` — refresh an expired or materially changed appraisal.
- `POST /{id}/acquire` — record completed legal acquisition.
- `POST /{id}/start-inventory-intake` — begin inventory creation.
- `POST /{id}/complete-inventory-intake` — link the acquired Vehicle and complete processing.
- `POST /{id}/cancel` — cancel an eligible Trade-In.
- `DELETE /{id}` — perform a soft delete when permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateTradeInRequest",
  "type": "object",
  "properties": {
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "opportunity_id": {
      "type": "string",
      "format": "uuid"
    },
    "vin": {
      "type": "string",
      "minLength": 17,
      "maxLength": 17
    },
    "registration_number": {
      "type": ["string", "null"],
      "maxLength": 50
    },
    "make": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "model": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "trim": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "year": {
      "type": "integer",
      "minimum": 1980
    },
    "mileage": {
      "type": "integer",
      "minimum": 0
    },
    "exterior_color": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "fuel_type": {
      "type": ["string", "null"]
    },
    "transmission": {
      "type": ["string", "null"]
    },
    "customer_expected_value_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "appointment_required": {
      "type": "boolean"
    }
  },
  "required": [
    "customer_id",
    "opportunity_id",
    "vin",
    "make",
    "model",
    "year",
    "mileage",
    "currency_code",
    "appointment_required"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type TradeIn {
  id: ID!
  dealershipId: ID!
  customerId: ID!
  opportunityId: ID!
  dealId: ID
  appraiserId: ID
  inspectionId: ID
  payoffVerificationId: ID
  acquiredVehicleId: ID
  vin: String!
  registrationNumber: String
  make: String!
  model: String!
  trim: String
  year: Int!
  mileage: Int!
  exteriorColor: String
  fuelType: FuelType
  transmission: Transmission
  ownershipVerified: Boolean!
  titleStatus: TradeInTitleStatus!
  lienStatus: TradeInLienStatus!
  payoffRequired: Boolean!
  payoffAmount: Float
  payoffValidUntil: Date
  overallCondition: TradeInCondition!
  accidentHistoryStatus: AccidentHistoryStatus!
  odometerVerificationStatus: OdometerVerificationStatus!
  customerExpectedValueAmount: Float
  marketValueAmount: Float!
  wholesaleValueAmount: Float!
  estimatedReconditioningAmount: Float!
  actualCashValueAmount: Float!
  tradeInAllowanceAmount: Float!
  overAllowanceAmount: Float!
  netEquityAmount: Float!
  valuationMethod: TradeInValuationMethod!
  valuationConfidenceScore: Float!
  approvalStatus: TradeInApprovalStatus!
  status: TradeInStatus!
  appraisedAt: DateTime
  expiresAt: DateTime
  offerPresentedAt: DateTime
  offerAcceptedAt: DateTime
  acquiredAt: DateTime
  inventoryIntakeCompletedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `trade_ins`
- **Inspection Table:** `trade_in_inspections`
- **Damage Table:** `trade_in_damage_items`
- **Valuation Table:** `trade_in_valuations`
- **Comparable Table:** `trade_in_market_comparables`
- **Payoff Table:** `trade_in_payoff_verifications`
- **Ownership Table:** `trade_in_ownership_documents`
- **Approval Table:** `trade_in_approvals`
- **Offer-History Table:** `trade_in_offers`
- **Status-History Table:** `trade_in_status_history`
- **Audit Table:** `trade_in_audit_log`

### Indexes

- `idx_trade_ins_tenant_status (dealership_id, status)`  
  Used for inspection, appraisal, approval, and acquisition queues.

- `idx_trade_ins_customer (dealership_id, customer_id, created_at DESC)`  
  Used for Customer Trade-In history.

- `idx_trade_ins_opportunity (dealership_id, opportunity_id, status)`  
  Used to retrieve Trade-Ins linked to an Opportunity.

- `idx_trade_ins_deal (deal_id)`  
  Used to validate Deal inclusion.

- `idx_trade_ins_vin (dealership_id, vin, status)`  
  Used to prevent duplicate active appraisals.

- `idx_trade_ins_appraiser (dealership_id, appraiser_id, status)`  
  Used for Appraiser work queues.

- `idx_trade_ins_expiry (dealership_id, expires_at, status)`  
  Used by appraisal-expiry Jobs.

- `idx_trade_ins_payoff_expiry (dealership_id, payoff_valid_until)`  
  Used for payoff-refresh monitoring.

- `idx_trade_ins_acquired_vehicle (acquired_vehicle_id)`  
  Used to validate inventory intake.

### Unique Constraints

- `UQ_active_trade_in_vin (dealership_id, vin)`  
  Applies while the Trade-In remains in a non-terminal active state.

- `UQ_trade_in_inspection (inspection_id)`  
  Applies when `inspection_id` is not null.

- `UQ_trade_in_acquired_vehicle (acquired_vehicle_id)`  
  Applies when `acquired_vehicle_id` is not null.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)`
- `opportunity_id` → `opportunities(id)`
- `deal_id` → `deals(id)` — nullable
- `appraiser_id` → `users(id)` — nullable
- `inspection_id` → `vehicle_inspections(id)` — nullable
- `payoff_verification_id` → `payoff_verifications(id)` — nullable
- `acquired_vehicle_id` → `vehicles(id)` — nullable
- `approved_by` → `users(id)` — nullable

### Database Constraints

- `mileage >= 0`
- `valuation_confidence_score BETWEEN 0.00 AND 1.00`
- `payoff_valid_until >= payoff_verified_at`
- `expires_at > appraised_at`
- `over_allowance_amount = GREATEST(trade_in_allowance_amount - actual_cash_value_amount, 0)`
- `net_equity_amount = trade_in_allowance_amount - payoff_amount`
- `negative_equity_amount = GREATEST(payoff_amount - trade_in_allowance_amount, 0)`
- `inventory_intake_completed_at >= acquired_at`
- `acquired_vehicle_id IS NOT NULL` when `status = COMPLETED`
- `ownership_verified = true` before `status = ACQUIRED`
- Approved valuation snapshots must be immutable.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical Trade-Ins by `created_at`.
- Inspection, valuation, payoff, approval, offer, status-history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Create and read Trade-Ins linked to assigned Customers and Opportunities; no authority to approve final values.
- **Appraiser:** Read/Write access to inspection and valuation fields for assigned Trade-Ins.
- **Used Vehicle Manager:** Approval and operational oversight of valuation and acquisition.
- **Sales Manager / GSM:** Approval access for allowances, over-allowance, and exceptional Trade-In terms.
- **Finance Manager:** Access to payoff, lien, negative-equity, and Deal-finance implications.
- **Compliance User:** Access to ownership, title, registration, fraud, and document-verification evidence.
- **Inventory User:** Access to acquired Vehicle intake after legal ownership transfer.
- **Marketing User:** Aggregated analytics only; no ownership, payoff, or Customer-specific valuation access.
- **AI Trade-In Agent:** Service Account access limited to extraction, summaries, comparable analysis, recommendations, and approved workflow requests.
- **External Valuation Service:** Restricted create-response access using tenant-scoped credentials.

### PII Classification

- **Level:** `CRITICAL_PII`

The Trade-In may contain or reference:

- Registered owner name
- Customer identity
- Registration number
- Ownership documents
- Title documents
- Lender information
- Loan references
- Customer address
- Vehicle-location information
- Inspection media containing identifying information

### Financially Sensitive Fields

- `customer_expected_value_amount`
- `market_value_amount`
- `wholesale_value_amount`
- `actual_cash_value_amount`
- `trade_in_allowance_amount`
- `payoff_amount`
- `net_equity_amount`
- `negative_equity_amount`
- `final_acquisition_cost_amount`
- Lender and payoff references

### Commercially Sensitive Fields

- Internal appraisal methods
- Auction and wholesale data
- Reconditioning estimates
- Approval thresholds
- Over-allowance values
- Internal valuation adjustments
- Market-comparable selection logic

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for database volumes, inspection media, documents, snapshots, and backups.
- **Column-Level Protection:** Ownership data, registration identifiers, lender information, payoff references, title evidence, and financial valuation fields require encryption, tokenization, or equivalent approved protection.
- Documents and inspection media must be stored in an encrypted Document Vault.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every status transition must create an immutable audit record containing:
  - `trade_in_id`
  - `previous_status`
  - `new_status`
  - `actor_id`
  - `timestamp`
  - `reason`
  - `supporting_evidence`

- Every valuation change must record:
  - Previous value
  - New value
  - Valuation method
  - Source
  - Appraiser
  - Timestamp
  - Rule version

- Every approval decision must record:
  - Approver
  - Authority level
  - Approved value
  - Allowance value
  - Over-allowance value
  - Reason
  - Supporting evidence
  - Timestamp

- Payoff verification must preserve:
  - Lender source
  - Verified amount
  - Verification actor
  - Verification timestamp
  - Validity date
  - Source-document hash

- Customer acceptance must preserve:
  - Offer version
  - Presented allowance
  - Net-equity amount
  - Acceptance channel
  - Customer evidence
  - Timestamp

- Acquisition and inventory intake must preserve:
  - Ownership-transfer evidence
  - Vehicle possession evidence
  - Acquired Vehicle ID
  - Accounting reference
  - DMS reference
  - Responsible actor
  - Timestamp

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to ownership, title, payoff, valuation, and inspection-media data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, Opportunity, Deal, Vehicle, inspection, payoff, or Trade-In linking is prohibited.
- AI Agents, Jobs, integrations, exports, and semantic retrieval must receive tenant scope through signed execution context.
- Document and media links must be tenant-scoped, time-limited, and cryptographically protected.

### Retention and Deletion

- Trade-In records included in completed or cancelled Deals must follow applicable financial, title, tax, accounting, and legal retention requirements.
- Completed acquisition and inventory-intake records must remain immutable.
- Hard deletion is prohibited while Quotations, Deals, Finance Applications, Payments, acquired Vehicles, accounting entries, or audit dependencies exist.
- Soft deletion is the operational default for eligible records.
- Legally approved deletion requests must purge or anonymize permitted Customer PII while preserving transaction and ownership records that must legally remain.
- Deletion and anonymization must address:
  - Trade-In records
  - Ownership and title documents
  - Inspection media
  - Valuation and payoff snapshots
  - Customer communications
  - Vector stores
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
