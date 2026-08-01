# Quotation

## 1. Object Purpose

### Business Purpose

The Quotation object represents a formal, time-bound commercial offer prepared by the dealership for a specific Customer and Opportunity.

It communicates the proposed vehicle, price, discounts, fees, taxes, finance assumptions, trade-in allowances, optional products, and total amount payable under clearly defined terms.

The Quotation provides a controlled commercial document that allows the dealership to:

- Present approved pricing to the Customer.
- Compare alternative vehicle and payment scenarios.
- Track Customer acceptance, rejection, and expiry.
- Prevent unauthorized discounts or margin leakage.
- Preserve every commercial version presented to the Customer.
- Support negotiation without modifying previously issued offers.
- Create the approved commercial basis for Deal creation.

A Quotation is not an executed contract, payment receipt, finance approval, Vehicle reservation, or final Deal.

### System Purpose

The Quotation object is the canonical, versioned commercial-offer aggregate generated from an Opportunity.

It connects:

- Opportunity
- Customer
- Vehicle
- Trade-In
- Finance Application
- Pricing Rules
- Discount Approvals
- Optional Products
- Taxes and Fees
- The eventual Deal

Every issued Quotation must be immutable. Any commercial change after issuance creates a new Quotation version that supersedes the previous version.

The object provides the authoritative commercial snapshot used by:

- Quotation-generation workflows.
- Sales Manager approval workflows.
- Customer-facing presentation.
- Negotiation tracking.
- Quotation expiry Jobs.
- Opportunity-stage progression.
- Deal-creation validation.
- Revenue and discount analytics.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `quotation_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `opportunity_id` (UUIDv4 — required)
  - `customer_id` (UUIDv4 — required)
  - `vehicle_id` (UUIDv4 — required)
  - `created_by` (UUIDv4 — required)
  - `approved_by` (UUIDv4 — optional)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — optional)
  - `supersedes_quotation_id` (UUIDv4 — optional)
  - `accepted_deal_id` (UUIDv4 — optional)

### Versioning Fields

- `quotation_number`
- `quotation_version`
- `supersedes_quotation_id`
- `is_current_version`
- `document_hash`
- `pricing_snapshot_version`
- `business_rule_version`

### Commercial Payload

- **Vehicle Pricing:**
  - `vehicle_list_price`
  - `vehicle_selling_price`
  - `vehicle_discount_amount`
  - `manufacturer_rebate_amount`
  - `dealer_incentive_amount`

- **Trade-In Pricing:**
  - `trade_in_actual_cash_value`
  - `trade_in_allowance_amount`
  - `trade_in_payoff_amount`
  - `trade_in_net_equity_amount`

- **Fees and Taxes:**
  - `tax_amount`
  - `registration_fee_amount`
  - `documentation_fee_amount`
  - `delivery_fee_amount`
  - `insurance_amount`
  - `other_fee_amount`

- **Optional Products:**
  - `optional_products`
  - `optional_products_total_amount`

- **Finance Assumptions:**
  - `payment_method`
  - `down_payment_amount`
  - `finance_principal_amount`
  - `estimated_interest_rate`
  - `finance_term_months`
  - `estimated_monthly_payment`
  - `balloon_payment_amount`

### Totals

- `subtotal_amount`
- `total_discount_amount`
- `total_fee_amount`
- `total_tax_amount`
- `total_due_amount`
- `customer_cash_due_amount`
- `currency_code`
- `gross_profit_amount`
- `gross_margin_percentage`

### Customer Presentation

- `customer_message`
- `terms_and_conditions`
- `valid_from`
- `expires_at`
- `sent_at`
- `viewed_at`
- `accepted_at`
- `rejected_at`
- `rejection_reason`

### Approval Fields

- `approval_status`
- `approval_required`
- `approval_reason`
- `approved_by`
- `approved_at`
- `approval_notes`

### Governance and Lifecycle

- **Commercial Snapshot:** `commercial_snapshot` (JSONB)
- **Customer Presentation Snapshot:** `presentation_snapshot` (JSONB)
- **Approval Evidence:** `approval_evidence` (JSONB)
- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `approved_by`
  - `sent_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `submitted_for_approval_at`
  - `approved_at`
  - `sent_at`
  - `viewed_at`
  - `accepted_at`
  - `rejected_at`
  - `expires_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| quotation_id | UUID | Unique canonical identifier for the Quotation version. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Quotation. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| opportunity_id | UUID | Opportunity for which the offer was prepared. | Yes | N/A | Must reference an active Opportunity in the same dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| customer_id | UUID | Customer receiving the offer. | Yes | From Opportunity | Must match the Opportunity Customer | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| vehicle_id | UUID | Vehicle included in the offer. | Yes | N/A | Must reference an eligible Vehicle in the same dealership | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| quotation_number | String | Human-readable dealership quotation reference. | Yes | System-generated | Must be unique within the dealership | QT-2026-000145 | System-generated |
| quotation_version | Integer | Sequential version number for the commercial offer. | Yes | 1 | Must be one or greater and increase sequentially | 3 | System-controlled |
| supersedes_quotation_id | UUID | Previous Quotation replaced by this version. | No | Null | Must reference an earlier version for the same Opportunity | 888e9999-e00b-11d2-a222-426614174000 | System-controlled |
| is_current_version | Boolean | Indicates whether this is the active Quotation version. | Yes | true | Only one current version may exist per quotation series | true | System-controlled |
| status | Enum | Current lifecycle state of the Quotation. | Yes | DRAFT | Must match QuotationStatus ENUM | SENT | At least 0.99 |
| approval_status | Enum | Current commercial-approval state. | Yes | NOT_REQUIRED | Must match QuotationApprovalStatus ENUM | APPROVED | System-controlled |
| vehicle_list_price | Decimal | Published or approved list price of the Vehicle. | Yes | 0.00 | Must be zero or greater | 2100000.00 | Pricing system |
| vehicle_selling_price | Decimal | Proposed Vehicle price before taxes and additional fees. | Yes | 0.00 | Must be zero or greater | 2000000.00 | Human or pricing engine |
| vehicle_discount_amount | Decimal | Dealer-funded reduction from the list price. | Yes | 0.00 | Must be zero or greater and cannot exceed list price | 100000.00 | Human or pricing engine |
| manufacturer_rebate_amount | Decimal | Manufacturer-funded incentive applied to the Customer. | Yes | 0.00 | Must be supported by an active rebate program | 50000.00 | OEM or approved human |
| trade_in_actual_cash_value | Decimal | Dealership's assessed wholesale value of the trade-in Vehicle. | No | 0.00 | Must come from an approved appraisal | 600000.00 | Appraisal system |
| trade_in_allowance_amount | Decimal | Amount credited to the Customer for the trade-in. | No | 0.00 | Must not be confused with actual cash value | 650000.00 | Approved human |
| trade_in_payoff_amount | Decimal | Outstanding lender payoff attached to the trade-in. | No | 0.00 | Requires verified payoff evidence | 200000.00 | Verified external evidence |
| trade_in_net_equity_amount | Decimal | Net trade-in value after subtracting the payoff. | No | 0.00 | Must be system-computed | 450000.00 | System-computed |
| tax_amount | Decimal | Total applicable tax for the proposed transaction. | Yes | 0.00 | Must be calculated using active jurisdiction rules | 280000.00 | Tax engine |
| total_fee_amount | Decimal | Sum of all approved dealership and statutory fees. | Yes | 0.00 | Must equal the sum of individual fee fields | 35000.00 | System-computed |
| optional_products_total_amount | Decimal | Total amount for selected optional products. | Yes | 0.00 | Must equal the sum of active optional-product items | 45000.00 | System-computed |
| total_discount_amount | Decimal | Sum of all approved discounts and rebates. | Yes | 0.00 | Must be system-computed | 150000.00 | System-computed |
| subtotal_amount | Decimal | Commercial subtotal before tax and final fees. | Yes | 0.00 | Must be system-computed | 1895000.00 | System-computed |
| total_due_amount | Decimal | Complete transaction amount proposed to the Customer. | Yes | 0.00 | Must equal the approved pricing formula | 2210000.00 | System-computed |
| down_payment_amount | Decimal | Customer down payment assumed in the offer. | No | 0.00 | Must be zero or greater and cannot exceed total due | 500000.00 | Human or finance input |
| finance_principal_amount | Decimal | Amount expected to be financed. | No | 0.00 | Must equal financed transaction components under the approved formula | 1710000.00 | System-computed |
| estimated_interest_rate | Decimal | Non-binding annual finance-rate assumption. | No | Null | Must be zero or greater and clearly marked as estimated | 18.50 | Finance source |
| finance_term_months | Integer | Proposed finance duration. | No | Null | Must match an approved lender term | 60 | Finance source |
| estimated_monthly_payment | Decimal | Estimated periodic Customer payment. | No | Null | Must be system-calculated and marked non-final until lender approval | 43800.00 | System-computed |
| currency_code | String | ISO 4217 currency code for all monetary values. | Yes | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| gross_profit_amount | Decimal | Estimated dealership gross profit for the Quotation. | Yes | 0.00 | Restricted internal field; must be system-computed | 175000.00 | System-computed |
| gross_margin_percentage | Decimal | Gross profit expressed as a percentage. | Yes | 0.00 | Must be system-computed | 8.75 | System-computed |
| valid_from | Timestamp | Time from which the offer becomes valid. | Yes | Approval or creation time | Cannot precede final approval when approval is required | 2026-08-01T12:00:00Z | System-recorded |
| expires_at | Timestamp | Time after which the Quotation can no longer be accepted. | Yes | Policy-defined | Must be later than valid_from; default policy is 48 hours | 2026-08-03T12:00:00Z | System-calculated |
| document_hash | String | Cryptographic hash of the issued Quotation document and snapshot. | Conditional | Generated on issue | Required before status becomes SENT | sha256:8ac4... | System-generated |
| accepted_deal_id | UUID | Deal created from the accepted Quotation. | No | Null | Required only after successful Deal creation | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| record_version | Integer | Optimistic concurrency version. | Yes | 1 | Must increase after every permitted update | 4 | System-controlled |

## 4. Enumerations

### QuotationStatus

- **DRAFT:** The Quotation is being prepared and may still be edited.
- **PENDING_APPROVAL:** The Quotation requires authorized commercial review.
- **APPROVED:** The Quotation passed all required approval checks but has not yet been sent.
- **SENT:** The immutable Quotation was delivered to the Customer.
- **VIEWED:** The Customer accessed or opened the Quotation.
- **ACCEPTED:** The Customer provided a valid acceptance signal.
- **REJECTED:** The Customer explicitly rejected the Quotation.
- **EXPIRED:** The acceptance deadline passed without valid acceptance.
- **SUPERSEDED:** A newer Quotation version replaced this version.
- **CANCELLED:** The dealership withdrew the Quotation before acceptance.

### QuotationApprovalStatus

- NOT_REQUIRED
- REQUIRED
- PENDING
- APPROVED
- REJECTED

### PaymentMethod

- CASH
- BANK_TRANSFER
- FINANCE
- LEASE
- MIXED

### QuotationRejectionReason

- PRICE_TOO_HIGH
- PAYMENT_TERMS_UNACCEPTABLE
- VEHICLE_CHANGED
- VEHICLE_UNAVAILABLE
- FINANCE_TERMS_UNACCEPTABLE
- TRADE_IN_VALUE_REJECTED
- COMPETITOR_OFFER
- CUSTOMER_POSTPONED
- CUSTOMER_NO_LONGER_INTERESTED
- OTHER

### QuotationCancellationReason

- PRICING_ERROR
- VEHICLE_UNAVAILABLE
- INCORRECT_CUSTOMER
- INCORRECT_VEHICLE
- EXPIRED_REBATE
- DUPLICATE_QUOTATION
- COMPLIANCE_FAILURE
- CUSTOMER_REQUEST
- OTHER

### OptionalProductType

- WARRANTY
- SERVICE_PLAN
- INSURANCE
- ACCESSORY
- PROTECTION_PACKAGE
- REGISTRATION_SERVICE
- OTHER

## 5. Validation Rules

### Business Rules

- A Quotation can be created only for an active, non-terminal Opportunity.
- The Customer and Vehicle must match the linked Opportunity context.
- A Quotation cannot be generated for a Vehicle reserved by another active Opportunity or Deal.
- Every Quotation must contain one specific Vehicle or an explicitly approved commercial product.
- A Quotation must not represent finance approval; finance figures remain estimates until lender approval.
- A Quotation must not represent confirmed payment or cleared bank funds.
- A Quotation must not create or guarantee Vehicle reservation unless a separate authorized reservation operation succeeds.
- A sent Quotation is immutable.
- Any change to price, Vehicle, fee, tax, product, finance assumption, or trade-in amount after sending requires a new version.
- A new version must reference the previous version through `supersedes_quotation_id`.
- Only one Quotation version in a series may have `is_current_version = true`.
- An accepted Quotation cannot create a Deal after expiration unless it is formally revalidated or replaced.
- The default validity period is 48 hours unless a dealership rule specifies another approved period.
- Discounts below approved pricing boundaries require human approval.
- Optional products must be explicitly itemized and cannot be hidden inside unrelated fees.
- Trade-in actual cash value, allowance, payoff, and net equity must remain separate.
- Customer-facing documents must not expose cost, margin, internal discount limits, or approval thresholds.

### Technical Rules

- All monetary calculations must use fixed decimal precision.
- Every monetary field must use the same `currency_code`.
- Totals must be recalculated server-side before approval and issue.
- The issued document and `commercial_snapshot` must produce a cryptographic `document_hash`.
- Idempotency keys must be required for issue, accept, reject, and Deal-conversion operations.
- `record_version` must increase after every permitted update.
- Issued Quotation snapshots must be immutable.
- Quotation versions must increase sequentially without gaps inside the same series.
- Tax and fee calculations must record the rule version used.
- OEM incentives must be validated against active effective dates and Vehicle eligibility.
- Expiry must be enforced by an authoritative server-side Job, not only by the user interface.
- Customer acceptance evidence must include the channel, timestamp, actor, and accepted document hash.

### Data Constraints

- Financial amounts cannot be negative unless an explicitly permitted accounting adjustment applies.
- `vehicle_discount_amount` cannot exceed `vehicle_list_price`.
- `budget`, pricing, fee, tax, and payment fields must use approved precision.
- `valid_from` must precede `expires_at`.
- `sent_at` cannot precede approval when approval is required.
- `viewed_at` cannot precede `sent_at`.
- `accepted_at` and `rejected_at` cannot both be populated.
- `accepted_deal_id` must be null unless the Quotation was accepted and converted.
- `gross_margin_percentage` must correspond to the approved gross-profit formula.
- `trade_in_net_equity_amount` must equal `trade_in_allowance_amount - trade_in_payoff_amount`.
- `finance_principal_amount` must match the approved finance calculation.
- `total_fee_amount` must equal the sum of its component fees.
- `total_discount_amount` must equal the sum of approved discount and rebate components.
- `total_due_amount` must equal the server-calculated commercial formula.

### Referential Integrity

- All linked entities must belong to the same `dealership_id`.
- `customer_id` must match the Customer linked to `opportunity_id`.
- `vehicle_id` must match the selected Vehicle for the Opportunity or an approved replacement.
- `trade_in_id` must belong to the same Customer and Opportunity.
- `finance_application_id` must belong to the same Customer and Opportunity.
- `supersedes_quotation_id` must reference an earlier Quotation in the same series.
- Circular supersession is prohibited.
- `accepted_deal_id` must reference a Deal created from this exact Quotation version.
- A Quotation cannot be hard-deleted while referenced by a Deal, approval, Customer communication, or audit entry.

### Human Approval Requirements

- Discounts exceeding the Sales Consultant's authority require Sales Manager approval.
- Negative or below-threshold gross margin requires GSM or authorized executive approval.
- Manual tax overrides require Finance or compliance approval.
- Manual fee overrides require an authorized User and documented reason.
- Changes to trade-in allowance above actual cash value require explicit approval.
- An expired Quotation cannot be reactivated by an AI Agent.
- AI Agents cannot approve prices, discounts, fees, finance terms, or trade-in values.
- AI Agents cannot mark a Quotation accepted without verifiable Customer evidence.
- AI Agents cannot convert a Quotation into a Deal without all mandatory validations and approvals.

## 6. State Machine

### Allowed States

- DRAFT
- PENDING_APPROVAL
- APPROVED
- SENT
- VIEWED
- ACCEPTED
- REJECTED
- EXPIRED
- SUPERSEDED
- CANCELLED

### Allowed Transitions

- DRAFT → PENDING_APPROVAL
- DRAFT → APPROVED
- DRAFT → CANCELLED
- PENDING_APPROVAL → APPROVED
- PENDING_APPROVAL → DRAFT
- PENDING_APPROVAL → CANCELLED
- APPROVED → SENT
- APPROVED → CANCELLED
- SENT → VIEWED
- SENT → ACCEPTED
- SENT → REJECTED
- SENT → EXPIRED
- SENT → SUPERSEDED
- VIEWED → ACCEPTED
- VIEWED → REJECTED
- VIEWED → EXPIRED
- VIEWED → SUPERSEDED
- REJECTED → SUPERSEDED
- EXPIRED → SUPERSEDED

### Forbidden Transitions

- SENT → DRAFT
- VIEWED → DRAFT
- ACCEPTED → DRAFT
- ACCEPTED → SENT
- ACCEPTED → EXPIRED
- REJECTED → ACCEPTED
- EXPIRED → ACCEPTED
- SUPERSEDED → ACCEPTED
- CANCELLED → SENT
- CANCELLED → ACCEPTED
- DRAFT → ACCEPTED
- PENDING_APPROVAL → SENT
- PENDING_APPROVAL → ACCEPTED

### Entry Conditions

- To enter `PENDING_APPROVAL`:
  - All required commercial fields must be populated.
  - Server-side totals must be valid.
  - The Vehicle must remain eligible.
  - Every required approval reason must be identified.

- To enter `APPROVED`:
  - All required human approvals must be completed.
  - Pricing, tax, fees, rebates, and Vehicle availability must be revalidated.
  - No unresolved compliance block may exist.

- To enter `SENT`:
  - The Quotation must be approved or require no approval.
  - `commercial_snapshot` must be frozen.
  - The customer-facing document must be generated.
  - `document_hash`, `valid_from`, and `expires_at` must be populated.
  - A permitted Customer communication channel must exist.

- To enter `VIEWED`:
  - A valid Customer-access event must reference the issued document.

- To enter `ACCEPTED`:
  - The Quotation must not be expired, cancelled, rejected, or superseded.
  - Acceptance must reference the exact current document hash.
  - Customer identity and acceptance evidence must be verified.
  - Vehicle availability must be revalidated.

- To enter `REJECTED`:
  - A Customer rejection signal must be documented.
  - A standardized rejection reason should be recorded when known.

- To enter `EXPIRED`:
  - The authoritative current time must be later than `expires_at`.
  - No valid acceptance may have been recorded before expiry.

- To enter `SUPERSEDED`:
  - A valid replacement Quotation version must exist.
  - The new version must reference the previous version.
  - The previous version must have `is_current_version = false`.

- To enter `CANCELLED`:
  - An authorized cancellation reason must be recorded.
  - The Quotation must not already be accepted.

### Exit Conditions

- A Quotation cannot exit `DRAFT` until all mandatory pricing inputs are available.
- A Quotation cannot exit `PENDING_APPROVAL` without an approval, rejection, revision, or cancellation decision.
- A Quotation cannot exit `APPROVED` toward `SENT` until the immutable document snapshot is generated.
- A Quotation cannot exit `SENT` or `VIEWED` through direct editing.
- A rejected or expired Quotation may continue only through creation of a superseding version.
- An accepted Quotation may continue only through controlled Deal creation; its commercial content remains immutable.

### Terminal States

- **ACCEPTED:** The accepted commercial snapshot may be used for controlled Deal creation.
- **REJECTED:** The Customer declined this specific version.
- **EXPIRED:** The offer can no longer be accepted.
- **SUPERSEDED:** A newer version replaced this Quotation.
- **CANCELLED:** The dealership withdrew the Quotation.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Active Opportunity identified by `opportunity_id`.
  - Customer identified by `customer_id`.
  - Eligible Vehicle identified by `vehicle_id`.
  - Current dealership pricing, tax, fee, incentive, and approval rules.

- **Consumes:**
  - Opportunity commercial context.
  - Customer payment preference.
  - Vehicle price and availability.
  - Approved manufacturer rebates and dealership incentives.
  - Trade-In appraisal and payoff information.
  - Finance assumptions or approved lender programs.
  - Optional-product selections.
  - Tax and registration rules.
  - Discount-authority limits.

- **Produces:**
  - Immutable commercial-offer versions.
  - Customer-facing Quotation documents.
  - Approval requests.
  - Discount and margin analytics.
  - Negotiation context.
  - Deal-creation commercial snapshots.

- **Creates:**
  - Pricing-approval requests.
  - Customer communication records.
  - New Quotation versions when commercial terms change.
  - Deal-conversion requests after valid Customer acceptance.

- **Triggers:**
  - Quotation-approval workflows.
  - Customer-delivery workflows.
  - Expiry monitoring.
  - Opportunity-stage updates.
  - Vehicle-availability revalidation.
  - Customer acceptance or rejection workflows.
  - Controlled Deal creation.

- **Owned By:**
  - The Sales Consultant or authorized User who created the Quotation.
  - Commercial approval remains owned by the authorized Sales Manager, GSM, Finance User, or other approving role.

- **Referenced By:**
  - Opportunity
  - Deal
  - Vehicle Reservation
  - Finance Application
  - Trade-In
  - Approval Request
  - Interaction Log
  - Customer Journey
  - Document Vault
  - AI Agent Run

- **Supersedes:**
  - A newer Quotation version supersedes the previous version without deleting or modifying it.

> Quotation generation is conditional. Cash Opportunities may bypass this object when the dealership process does not require a formal Quotation. Finance, lease, trade-in, or structured-offer scenarios may require it according to the applicable Sales Playbook.

## 8. Domain Events

### Emitted Events

- **QuotationDraftCreated**  
  Payload: `quotation_id`, `opportunity_id`, `customer_id`, `vehicle_id`, `quotation_version`

- **QuotationApprovalRequested**  
  Payload: `quotation_id`, `approval_reason`, `requested_by`, `submitted_for_approval_at`

- **QuotationApproved**  
  Payload: `quotation_id`, `approved_by`, `approved_at`, `approval_evidence_reference`

- **QuotationApprovalRejected**  
  Payload: `quotation_id`, `rejected_by`, `rejection_reason`, `rejected_at`

- **QuotationIssued**  
  Payload: `quotation_id`, `quotation_number`, `document_hash`, `valid_from`, `expires_at`

- **QuotationSent**  
  Payload: `quotation_id`, `customer_id`, `communication_channel`, `sent_at`

- **QuotationViewed**  
  Payload: `quotation_id`, `viewed_at`, `access_reference`

- **QuotationAccepted**  
  Payload: `quotation_id`, `customer_id`, `document_hash`, `accepted_at`, `acceptance_channel`

- **QuotationRejected**  
  Payload: `quotation_id`, `rejection_reason`, `rejected_at`

- **QuotationExpired**  
  Payload: `quotation_id`, `expires_at`, `expired_at`

- **QuotationSuperseded**  
  Payload: `quotation_id`, `replacement_quotation_id`, `superseded_at`

- **QuotationCancelled**  
  Payload: `quotation_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

- **QuotationDealConversionRequested**  
  Payload: `quotation_id`, `opportunity_id`, `customer_id`, `vehicle_id`, `idempotency_key`

- **QuotationConvertedToDeal**  
  Payload: `quotation_id`, `accepted_deal_id`, `converted_at`

### Consumed Events

- **OpportunityQuotationRequested**  
  Creates a Draft Quotation when the Opportunity and Sales Playbook require a formal offer.

- **VehiclePriceUpdated**  
  Marks an unsent Draft for recalculation and may invalidate an approved but unsent version.

- **VehicleStatusChanged**  
  Blocks issue or acceptance when the Vehicle becomes unavailable or reserved elsewhere.

- **TradeInAssessmentCompleted**  
  Updates trade-in figures in a Draft or creates a new Quotation version after issue.

- **TradeInPayoffVerified**  
  Updates verified payoff and net-equity calculations.

- **FinanceProgramSelected**  
  Supplies permitted term, rate, and payment assumptions.

- **FinanceApplicationApproved**  
  Allows approved finance information to replace estimated assumptions through a new version when necessary.

- **OEMRebateProgramUpdated**  
  Revalidates applicable manufacturer incentives.

- **CustomerContactPermissionChanged**  
  Prevents unauthorized delivery of a Customer-facing Quotation.

- **DealCreated**  
  Populates `accepted_deal_id` after successful conversion.

- **DealCreationFailed**  
  Preserves the accepted Quotation and creates a retry or Human Review Task.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `customer_message`
- Non-sensitive `terms_and_conditions` summaries
- Customer objections
- Quotation rejection explanations
- Negotiation summaries
- Optional-product preferences
- Vehicle and payment-scenario descriptions

### Fields Excluded from Embeddings

- `quotation_id`
- `customer_id`
- `opportunity_id`
- `vehicle_id`
- `accepted_deal_id`
- `document_hash`
- `commercial_snapshot`
- `approval_evidence`
- `vehicle_list_price`
- `vehicle_selling_price`
- `gross_profit_amount`
- `gross_margin_percentage`
- `trade_in_actual_cash_value`
- `trade_in_payoff_amount`
- Internal discounts and approval thresholds
- Direct Customer contact information
- Finance or identity documents

> Exact prices, internal margins, Customer identity information, and approval boundaries must be supplied only through authorized structured context.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `status`
- `approval_status`
- `payment_method`
- `vehicle_selling_price`
- `total_due_amount`
- `down_payment_amount`
- `estimated_monthly_payment`
- `finance_term_months`
- `valid_from`
- `expires_at`
- Customer-visible optional products
- Customer-visible fees and taxes

Internal Agents with the correct role scope may additionally receive:

- `gross_profit_amount`
- `gross_margin_percentage`
- Approved discount boundaries
- Approval requirements

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `quotation_id`
- `opportunity_id`
- `customer_id`
- `vehicle_id`
- `status`
- `quotation_version`
- `payment_method`
- `is_current_version`

### Confidence Thresholds

- Extraction of Customer acceptance or rejection requires confidence of at least `0.95`.
- Extraction of requested commercial changes requires confidence of at least `0.90`.
- Payment-method classification requires confidence of at least `0.90`.
- Optional-product extraction requires confidence of at least `0.85`.
- AI-generated Customer messages require confidence of at least `0.95` before automated delivery.
- No AI confidence score may replace server-side financial validation.

### Human Approval Thresholds

- AI Agents cannot approve discounts, prices, fees, tax overrides, trade-in allowances, interest rates, or finance terms.
- AI Agents cannot mark a Quotation `ACCEPTED` without verifiable Customer evidence.
- AI Agents cannot reactivate an expired, rejected, cancelled, or superseded Quotation.
- Negative gross-profit or below-threshold margin scenarios require authorized human approval.
- Any discrepancy between the generated document and the commercial snapshot must block delivery and create a Human Review Task.
- AI-generated recommendations are advisory and cannot become binding commercial commitments without approval.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/quotations`

### Methods

- `GET` — list or search Quotations.
- `POST` — create a Draft Quotation.
- `GET /{id}` — retrieve one Quotation version.
- `PATCH /{id}` — update a Draft Quotation only.
- `POST /{id}/recalculate` — recalculate all server-authoritative totals.
- `POST /{id}/submit-for-approval` — request commercial approval.
- `POST /{id}/approve` — approve the Quotation.
- `POST /{id}/reject-approval` — reject the approval request.
- `POST /{id}/issue` — freeze the snapshots and generate the immutable document.
- `POST /{id}/send` — deliver the issued Quotation through an authorized channel.
- `POST /{id}/record-view` — record verified Customer access.
- `POST /{id}/accept` — record verified Customer acceptance.
- `POST /{id}/reject` — record Customer rejection.
- `POST /{id}/supersede` — create a replacement Quotation version.
- `POST /{id}/cancel` — withdraw the Quotation when permitted.
- `POST /{id}/convert` — create a Deal from an accepted Quotation.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateQuotationRequest",
  "type": "object",
  "properties": {
    "opportunity_id": {
      "type": "string",
      "format": "uuid"
    },
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "vehicle_id": {
      "type": "string",
      "format": "uuid"
    },
    "payment_method": {
      "type": "string",
      "enum": [
        "CASH",
        "BANK_TRANSFER",
        "FINANCE",
        "LEASE",
        "MIXED"
      ]
    },
    "vehicle_list_price": {
      "type": "number",
      "minimum": 0
    },
    "vehicle_selling_price": {
      "type": "number",
      "minimum": 0
    },
    "vehicle_discount_amount": {
      "type": "number",
      "minimum": 0
    },
    "manufacturer_rebate_amount": {
      "type": "number",
      "minimum": 0
    },
    "trade_in_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "finance_application_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "down_payment_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "estimated_interest_rate": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "finance_term_months": {
      "type": ["integer", "null"],
      "minimum": 1
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "optional_products": {
      "type": "array",
      "items": {
        "type": "object"
      }
    },
    "customer_message": {
      "type": ["string", "null"],
      "maxLength": 5000
    },
    "terms_and_conditions": {
      "type": "string",
      "minLength": 1
    },
    "expires_at": {
      "type": "string",
      "format": "date-time"
    }
  },
  "required": [
    "opportunity_id",
    "customer_id",
    "vehicle_id",
    "payment_method",
    "vehicle_list_price",
    "vehicle_selling_price",
    "vehicle_discount_amount",
    "manufacturer_rebate_amount",
    "currency_code",
    "optional_products",
    "terms_and_conditions",
    "expires_at"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Quotation {
  id: ID!
  dealershipId: ID!
  opportunityId: ID!
  customerId: ID!
  vehicleId: ID!
  tradeInId: ID
  financeApplicationId: ID
  supersedesQuotationId: ID
  acceptedDealId: ID
  quotationNumber: String!
  quotationVersion: Int!
  isCurrentVersion: Boolean!
  status: QuotationStatus!
  approvalStatus: QuotationApprovalStatus!
  paymentMethod: PaymentMethod!
  vehicleListPrice: Float!
  vehicleSellingPrice: Float!
  vehicleDiscountAmount: Float!
  manufacturerRebateAmount: Float!
  dealerIncentiveAmount: Float!
  tradeInActualCashValue: Float
  tradeInAllowanceAmount: Float
  tradeInPayoffAmount: Float
  tradeInNetEquityAmount: Float
  subtotalAmount: Float!
  totalDiscountAmount: Float!
  totalFeeAmount: Float!
  totalTaxAmount: Float!
  optionalProductsTotalAmount: Float!
  totalDueAmount: Float!
  customerCashDueAmount: Float!
  downPaymentAmount: Float
  financePrincipalAmount: Float
  estimatedInterestRate: Float
  financeTermMonths: Int
  estimatedMonthlyPayment: Float
  currencyCode: String!
  grossProfitAmount: Float!
  grossMarginPercentage: Float!
  documentHash: String
  validFrom: DateTime!
  expiresAt: DateTime!
  sentAt: DateTime
  viewedAt: DateTime
  acceptedAt: DateTime
  rejectedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `quotations`
- **Line-Item Table:** `quotation_line_items`
- **Optional-Product Table:** `quotation_optional_products`
- **Approval Table:** `quotation_approvals`
- **Customer-Delivery Table:** `quotation_deliveries`
- **Acceptance-Evidence Table:** `quotation_acceptance_evidence`
- **Version-History Table:** `quotation_versions`
- **Audit Table:** `quotation_audit_log`

### Indexes

- `idx_quotations_tenant_status (dealership_id, status)`  
  Used for Draft, approval, sent, accepted, and expiry work queues.

- `idx_quotations_opportunity (dealership_id, opportunity_id, quotation_version DESC)`  
  Used to retrieve every commercial version for an Opportunity.

- `idx_quotations_customer (dealership_id, customer_id, created_at DESC)`  
  Used to retrieve Customer Quotation history.

- `idx_quotations_vehicle (dealership_id, vehicle_id, status)`  
  Used for Vehicle availability and competing-offer checks.

- `idx_quotations_expiry (dealership_id, expires_at, status)`  
  Used by the expiry Job.

- `idx_quotations_approval (dealership_id, approval_status, submitted_for_approval_at)`  
  Used for manager-approval queues.

- `idx_quotations_current_version (dealership_id, opportunity_id, is_current_version)`  
  Used to locate the active Quotation version.

- `idx_quotations_accepted_deal (accepted_deal_id)`  
  Used to validate Deal conversion.

- `idx_quotations_number (dealership_id, quotation_number)`  
  Used for human-readable Quotation lookup.

### Unique Constraints

- `UQ_quotation_number (dealership_id, quotation_number)`
- `UQ_quotation_series_version (dealership_id, opportunity_id, quotation_version)`
- `UQ_current_quotation_version (dealership_id, opportunity_id) WHERE is_current_version = true`
- `UQ_quotation_document_hash (document_hash) WHERE document_hash IS NOT NULL`
- `UQ_quotation_accepted_deal (accepted_deal_id) WHERE accepted_deal_id IS NOT NULL`

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `opportunity_id` → `opportunities(id)`
- `customer_id` → `customers(id)`
- `vehicle_id` → `vehicles(id)`
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)` — nullable
- `supersedes_quotation_id` → `quotations(id)` — nullable
- `accepted_deal_id` → `deals(id)` — nullable
- `created_by` → `users(id)`
- `approved_by` → `users(id)` — nullable

### Database Constraints

- `quotation_version >= 1`
- `valid_from < expires_at`
- `vehicle_discount_amount <= vehicle_list_price`
- `trade_in_net_equity_amount = trade_in_allowance_amount - trade_in_payoff_amount`
- `total_fee_amount` must equal the sum of all active fee components.
- `total_discount_amount` must equal the sum of approved discounts and rebates.
- `accepted_at` and `rejected_at` cannot both be populated.
- `accepted_deal_id IS NOT NULL` only after valid acceptance and Deal creation.
- `document_hash IS NOT NULL` when status is `SENT`, `VIEWED`, `ACCEPTED`, `REJECTED`, `EXPIRED`, or `SUPERSEDED`.
- Issued financial and presentation snapshots must be immutable.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical Quotations by `created_at`.
- Supporting line-item, version, approval, delivery, evidence, and audit tables must use the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Create and edit Draft Quotations within their assigned Opportunities and approved pricing authority.
- **Sales Manager:** Review, approve, reject, and supersede Quotations within the matching dealership.
- **GSM / Executive Approver:** Approve exceptional margin, discount, or commercial-policy deviations.
- **Finance User:** Access finance assumptions, approved lender terms, and related Finance Application information.
- **Trade-In Appraiser:** Access trade-in valuation fields without unrestricted access to unrelated Customer financial data.
- **Marketing User:** No direct access to individual Quotation financial details; aggregated analytics only.
- **AI Quotation Agent:** Service Account access limited to calculation support, document drafting, summaries, and approval-request preparation.
- **Pricing Service:** Access to server-side pricing, tax, fee, incentive, and margin calculations.
- **Deal Service:** Read accepted immutable commercial snapshots during controlled Deal creation.

### PII Classification

- **Level:** `CRITICAL_PII — INHERITED`

The Quotation may reference or present:

- Customer name
- Customer contact information
- Customer address
- Finance intentions
- Trade-in information
- Payment assumptions
- Customer communication and acceptance evidence

### Commercially Sensitive Fields

- `vehicle_list_price`
- `vehicle_selling_price`
- `vehicle_discount_amount`
- `dealer_incentive_amount`
- `trade_in_actual_cash_value`
- `trade_in_allowance_amount`
- `gross_profit_amount`
- `gross_margin_percentage`
- Internal approval reasons
- Discount-authority limits
- Cost and internal pricing references

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for database volumes, documents, snapshots, and backups.
- **Column-Level Protection:** Customer financial assumptions, trade-in payoff details, commercial snapshots, approval evidence, and acceptance evidence require encryption, tokenization, or an equivalent approved protection method.
- Issued documents must be stored in an encrypted Document Vault.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every pricing or commercial-field change must record:
  - Previous value
  - New value
  - Actor
  - Timestamp
  - Reason
  - Pricing-rule version

- Every approval decision must record:
  - Approver
  - Authority level
  - Approval reason
  - Supporting evidence
  - Timestamp

- Every issue or send operation must preserve:
  - Document hash
  - Commercial snapshot
  - Presentation snapshot
  - Delivery channel
  - Actor
  - Timestamp

- Customer acceptance must preserve:
  - Customer identity evidence
  - Accepted document hash
  - Acceptance channel
  - Timestamp
  - IP address or equivalent technical evidence when legally permitted

- Deal conversion must preserve:
  - Conversion request ID
  - Idempotency key
  - Accepted Quotation ID
  - Created Deal ID
  - Actor
  - Timestamp

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to margin, trade-in, approval, and Customer financial data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, Opportunity, Vehicle, Trade-In, Finance Application, Quotation, and Deal linking is prohibited.
- AI Agents, pricing services, Jobs, exports, and semantic retrieval must receive tenant scope through signed execution context.
- Customer-facing document links must be tenant-scoped, time-limited, and cryptographically protected.

### Retention and Deletion

- Issued, accepted, superseded, rejected, expired, and cancelled Quotations must remain immutable.
- Soft deletion applies only when the Quotation is eligible and no compliance, Deal, approval, or audit dependency exists.
- Accepted Quotations must remain available while their linked Deals, finance records, payment records, or legal documents exist.
- Legally approved deletion requests must purge or anonymize applicable PII across:
  - Quotation records
  - Issued documents
  - Commercial and presentation snapshots
  - Approval evidence
  - Acceptance evidence
  - Customer communications
  - Vector stores
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
