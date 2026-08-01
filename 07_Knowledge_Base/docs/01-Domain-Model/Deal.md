# Deal

## 1. Object Purpose

### Business Purpose

The Deal object represents the dealership's formally committed vehicle-sales transaction involving a specific Customer, Vehicle, and Opportunity.

It begins when the Customer has provided a verifiable commercial commitment and the dealership has accepted the transaction for contracting, payment, finance, compliance, and delivery processing.

The Deal provides the dealership with a controlled transaction record for:

- Final commercial-term preservation.
- Vehicle allocation and reservation.
- Contract preparation and execution.
- Payment and deposit tracking.
- Finance approval coordination.
- Trade-in settlement.
- Regulatory and compliance checks.
- Delivery preparation.
- Revenue and gross-profit recognition.
- Deal cancellation, unwind, and exception management.

A Deal is more mature than an Opportunity or Quotation. It represents the authoritative transaction aggregate, but it does not replace specialized child records such as Contracts, Payments, Finance Applications, Trade-Ins, Compliance Checks, or Deliveries.

### System Purpose

The Deal object is the central transactional aggregate of the ASOS sales domain.

It connects:

- Opportunity
- Customer
- Vehicle
- Accepted Quotation
- Trade-In
- Finance Application
- Contract
- Payment
- Vehicle Reservation
- Compliance Case
- Delivery
- Sales Consultant
- Dealership

The Deal preserves the approved commercial snapshot used to complete the transaction.

It provides the canonical state used by:

- Contract-generation workflows.
- Vehicle-reservation workflows.
- Finance and payment workflows.
- Compliance and document-verification workflows.
- Delivery coordination.
- Accounting and DMS synchronization.
- Revenue reporting.
- Gross-profit reporting.
- Deal exception and unwind workflows.
- Post-sale Customer Journey workflows.

Deal creation must be idempotent. One Opportunity may create only one primary active Deal unless an authorized unwind or restructuring process creates a replacement transaction.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `deal_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `opportunity_id` (UUIDv4 — required)
  - `customer_id` (UUIDv4 — required)
  - `vehicle_id` (UUIDv4 — required)
  - `owner_id` (UUIDv4 — required)
  - `accepted_quotation_id` (UUIDv4 — conditional)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — optional)
  - `reservation_id` (UUIDv4 — optional)
  - `contract_id` (UUIDv4 — optional)
  - `delivery_id` (UUIDv4 — optional)
  - `compliance_case_id` (UUIDv4 — optional)
  - `replacement_deal_id` (UUIDv4 — optional)
  - `original_deal_id` (UUIDv4 — optional)
  - `sales_manager_id` (UUIDv4 — optional)
  - `finance_manager_id` (UUIDv4 — optional)

### Transaction Identity

- `deal_number`
- `deal_type`
- `sales_channel`
- `status`
- `approval_status`
- `funding_status`
- `payment_status`
- `delivery_readiness_status`

### Vehicle Snapshot

- `vehicle_vin`
- `vehicle_stock_number`
- `vehicle_make`
- `vehicle_model`
- `vehicle_year`
- `vehicle_condition`
- `vehicle_mileage`
- `vehicle_snapshot`

### Commercial Snapshot

- `vehicle_list_price`
- `vehicle_selling_price`
- `vehicle_discount_amount`
- `manufacturer_rebate_amount`
- `dealer_incentive_amount`
- `optional_products_total_amount`
- `subtotal_amount`
- `total_discount_amount`
- `total_fee_amount`
- `total_tax_amount`
- `total_due_amount`
- `currency_code`

### Trade-In Snapshot

- `trade_in_actual_cash_value`
- `trade_in_allowance_amount`
- `trade_in_payoff_amount`
- `trade_in_net_equity_amount`

### Funding and Payment Fields

- `payment_method`
- `deposit_required_amount`
- `deposit_received_amount`
- `down_payment_amount`
- `finance_principal_amount`
- `approved_interest_rate`
- `finance_term_months`
- `monthly_payment_amount`
- `balloon_payment_amount`
- `customer_cash_due_amount`
- `amount_paid`
- `balance_due_amount`

### Profitability Fields

- `vehicle_cost_amount`
- `gross_profit_amount`
- `gross_margin_percentage`
- `front_end_gross_amount`
- `back_end_gross_amount`
- `commissionable_gross_amount`

### Approval Fields

- `approval_required`
- `approval_reason`
- `approved_by`
- `approved_at`
- `approval_evidence`
- `manager_override_reason`

### Compliance and Execution Fields

- `contract_status`
- `document_status`
- `identity_verification_status`
- `compliance_status`
- `vehicle_title_status`
- `insurance_status`
- `registration_status`
- `delivery_readiness_status`

### Closing and Exception Fields

- `cancellation_reason`
- `cancellation_details`
- `unwind_reason`
- `unwind_details`
- `replacement_deal_id`
- `closed_reason`
- `closed_at`

### Computed Fields

- `total_paid_amount`
- `balance_due_amount`
- `funding_shortfall_amount`
- `deal_age_days`
- `days_to_contract`
- `days_to_funding`
- `days_to_delivery`
- `approval_delay_hours`
- `document_completion_percentage`
- `delivery_readiness_percentage`

### Governance and Lifecycle

- **Commercial Snapshot:** `commercial_snapshot` (JSONB)
- **Accepted Offer Snapshot:** `accepted_offer_snapshot` (JSONB)
- **Vehicle Snapshot:** `vehicle_snapshot` (JSONB)
- **Customer Snapshot:** `customer_snapshot` (JSONB)
- **Approval Evidence:** `approval_evidence` (JSONB)
- **Compliance Snapshot:** `compliance_snapshot` (JSONB)
- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `approved_by`
  - `closed_by`
  - `cancelled_by`
  - `unwound_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** `owner_id`
- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `submitted_for_approval_at`
  - `approved_at`
  - `reserved_at`
  - `contract_started_at`
  - `contract_signed_at`
  - `deposit_received_at`
  - `funded_at`
  - `delivery_ready_at`
  - `delivered_at`
  - `cancelled_at`
  - `unwound_at`
  - `closed_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| deal_id | UUID | Unique canonical identifier for the Deal. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Deal. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| opportunity_id | UUID | Opportunity converted into the Deal. | Yes | N/A | Must reference an eligible Opportunity in the same dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| customer_id | UUID | Customer purchasing or leasing the Vehicle. | Yes | From Opportunity | Must match the Opportunity Customer | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| vehicle_id | UUID | Vehicle allocated to the Deal. | Yes | From Opportunity | Must reference an eligible Vehicle in the same dealership | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| owner_id | UUID | Sales Consultant responsible for the Deal. | Yes | From Opportunity | Must reference an active authorized User | 321e6547-e89b-12d3-a456-426614174000 | System-controlled |
| accepted_quotation_id | UUID | Accepted Quotation used as the commercial basis. | Conditional | Null | Required when the Sales Playbook requires a formal Quotation | 444e5555-e66b-77d8-a999-426614174000 | System-controlled |
| deal_number | String | Human-readable dealership transaction number. | Yes | System-generated | Must be unique within the dealership | DL-2026-000248 | System-generated |
| deal_type | Enum | Commercial structure of the transaction. | Yes | RETAIL_CASH | Must match DealType ENUM | RETAIL_FINANCE | At least 0.99 |
| sales_channel | Enum | Channel through which the Deal was completed. | Yes | SHOWROOM | Must match DealSalesChannel ENUM | DIGITAL | At least 0.95 |
| status | Enum | Current lifecycle state of the Deal. | Yes | CREATED | Must match DealStatus ENUM | CONTRACTING | At least 0.99 |
| approval_status | Enum | Current commercial-approval state. | Yes | NOT_REQUIRED | Must match DealApprovalStatus ENUM | APPROVED | System-controlled |
| funding_status | Enum | Current funding or finance state. | Yes | NOT_REQUIRED | Must match DealFundingStatus ENUM | APPROVED | System-controlled |
| payment_status | Enum | Current payment-completion state. | Yes | UNPAID | Must match DealPaymentStatus ENUM | PARTIALLY_PAID | System-controlled |
| vehicle_list_price | Decimal | Published or approved list price of the Vehicle. | Yes | 0.00 | Must be zero or greater | 2100000.00 | Pricing system |
| vehicle_selling_price | Decimal | Final approved Vehicle selling price. | Yes | 0.00 | Must be zero or greater | 2000000.00 | Approved commercial source |
| vehicle_discount_amount | Decimal | Approved dealer-funded discount. | Yes | 0.00 | Must be zero or greater and not exceed list price | 100000.00 | Approved commercial source |
| manufacturer_rebate_amount | Decimal | Approved manufacturer-funded Customer rebate. | Yes | 0.00 | Must reference an eligible active incentive | 50000.00 | OEM or approved source |
| trade_in_actual_cash_value | Decimal | Approved internal value of the trade-in Vehicle. | No | 0.00 | Must come from an approved appraisal | 600000.00 | Appraisal system |
| trade_in_allowance_amount | Decimal | Trade-in credit applied to the transaction. | No | 0.00 | Must preserve any over-allowance separately | 650000.00 | Approved human |
| trade_in_payoff_amount | Decimal | Verified lender payoff for the trade-in Vehicle. | No | 0.00 | Requires current verified payoff evidence | 200000.00 | Verified external evidence |
| trade_in_net_equity_amount | Decimal | Net value applied after deducting payoff. | No | 0.00 | Must be system-computed | 450000.00 | System-computed |
| total_discount_amount | Decimal | Total approved discounts, rebates, and incentives. | Yes | 0.00 | Must equal the sum of approved components | 150000.00 | System-computed |
| total_fee_amount | Decimal | Total statutory and dealership fees. | Yes | 0.00 | Must equal the sum of active fee items | 35000.00 | System-computed |
| total_tax_amount | Decimal | Total transaction tax. | Yes | 0.00 | Must use approved jurisdiction rules | 280000.00 | Tax engine |
| total_due_amount | Decimal | Complete approved transaction value. | Yes | 0.00 | Must equal the server-authoritative commercial formula | 2210000.00 | System-computed |
| currency_code | String | ISO 4217 currency code for all monetary fields. | Yes | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| deposit_required_amount | Decimal | Deposit required to secure the transaction or Vehicle. | No | 0.00 | Must be zero or greater | 100000.00 | Approved policy |
| deposit_received_amount | Decimal | Cleared deposit amount received from the Customer. | Yes | 0.00 | Cannot exceed total due without approved refund handling | 100000.00 | Payment system |
| down_payment_amount | Decimal | Customer down payment included in the transaction. | No | 0.00 | Must be zero or greater and not exceed total due | 500000.00 | Approved financial source |
| finance_principal_amount | Decimal | Amount approved or expected to be financed. | No | 0.00 | Required for financed Deals | 1710000.00 | Finance system |
| approved_interest_rate | Decimal | Final lender-approved annual interest rate. | No | Null | Must come from an approved Finance Application | 18.50 | Finance system |
| finance_term_months | Integer | Approved finance duration. | No | Null | Required for financed Deals | 60 | Finance system |
| monthly_payment_amount | Decimal | Approved periodic payment. | No | Null | Must match the approved finance schedule | 43800.00 | Finance system |
| amount_paid | Decimal | Total cleared Customer funds applied to the Deal. | Yes | 0.00 | Must be system-computed from cleared Payments | 600000.00 | Payment system |
| balance_due_amount | Decimal | Remaining amount required before completion. | Yes | Total due | Must be system-computed | 1610000.00 | System-computed |
| vehicle_cost_amount | Decimal | Dealership cost basis for the Vehicle. | Yes | 0.00 | Restricted internal field | 1825000.00 | DMS or accounting system |
| gross_profit_amount | Decimal | Approved estimated gross profit. | Yes | 0.00 | Must be system-computed | 175000.00 | System-computed |
| gross_margin_percentage | Decimal | Gross profit expressed as a percentage. | Yes | 0.00 | Must be system-computed | 8.75 | System-computed |
| contract_status | Enum | Current execution state of the contract package. | Yes | NOT_STARTED | Must match ContractStatus ENUM | SIGNED | System-controlled |
| compliance_status | Enum | Overall compliance-review status. | Yes | NOT_STARTED | Must match DealComplianceStatus ENUM | CLEARED | System-controlled |
| delivery_readiness_status | Enum | Current readiness for Vehicle handover. | Yes | NOT_READY | Must match DeliveryReadinessStatus ENUM | READY | System-controlled |
| replacement_deal_id | UUID | Replacement Deal created after an approved unwind. | No | Null | Must reference a Deal in the same dealership | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 8 | System-controlled |

## 4. Enumerations

### DealStatus

- **CREATED:** The Deal was created from an approved Opportunity or accepted Quotation.
- **PENDING_APPROVAL:** Required commercial or managerial approval is outstanding.
- **APPROVED:** Required approvals were completed.
- **VEHICLE_RESERVED:** The Vehicle was allocated successfully to this Deal.
- **CONTRACTING:** Contract and supporting documents are being prepared or executed.
- **AWAITING_PAYMENT:** Required Customer funds remain outstanding.
- **AWAITING_FUNDING:** Lender or external funding remains outstanding.
- **COMPLIANCE_REVIEW:** Identity, regulatory, title, insurance, or document checks are incomplete.
- **DELIVERY_PREPARATION:** Commercial and compliance conditions passed, and delivery preparation is underway.
- **READY_FOR_DELIVERY:** All mandatory delivery conditions are satisfied.
- **DELIVERED:** The Vehicle was handed over and delivery evidence was recorded.
- **CLOSED:** The completed Deal was posted to authoritative accounting or DMS systems.
- **CANCELLED:** The transaction ended before completion.
- **UNWOUND:** A previously executed or delivered transaction was formally reversed.

### DealType

- RETAIL_CASH
- RETAIL_FINANCE
- RETAIL_LEASE
- CORPORATE_CASH
- CORPORATE_FINANCE
- FLEET
- TRADE_IN_CASH
- TRADE_IN_FINANCE
- DEMO_SALE
- WHOLESALE

### DealSalesChannel

- SHOWROOM
- DIGITAL
- PHONE
- WHATSAPP
- MARKETPLACE
- OEM_PLATFORM
- FLEET_DESK
- REFERRAL
- OTHER

### DealApprovalStatus

- NOT_REQUIRED
- REQUIRED
- PENDING
- APPROVED
- REJECTED
- EXPIRED

### DealFundingStatus

- NOT_REQUIRED
- NOT_STARTED
- APPLICATION_PENDING
- DOCUMENTS_REQUIRED
- UNDER_REVIEW
- CONDITIONALLY_APPROVED
- APPROVED
- FUNDED
- DECLINED
- CANCELLED

### DealPaymentStatus

- UNPAID
- DEPOSIT_PAID
- PARTIALLY_PAID
- PAID
- OVERPAID
- REFUND_PENDING
- REFUNDED
- CHARGEBACK

### ContractStatus

- NOT_STARTED
- DRAFT
- READY_FOR_SIGNATURE
- PARTIALLY_SIGNED
- SIGNED
- VOIDED
- REPLACED

### DealComplianceStatus

- NOT_STARTED
- PENDING
- DOCUMENTS_REQUIRED
- UNDER_REVIEW
- CLEARED
- BLOCKED
- REJECTED

### DeliveryReadinessStatus

- NOT_READY
- BLOCKED
- IN_PROGRESS
- READY
- DELIVERED

### DealCancellationReason

- CUSTOMER_WITHDREW
- VEHICLE_UNAVAILABLE
- FINANCE_DECLINED
- PAYMENT_FAILED
- COMPLIANCE_FAILED
- PRICING_ERROR
- CONTRACT_NOT_SIGNED
- TRADE_IN_FAILED
- DEALERSHIP_DECLINED
- DUPLICATE_DEAL
- OTHER

### DealUnwindReason

- CUSTOMER_RETURN
- CONTRACT_ERROR
- FUNDING_REVERSED
- PAYMENT_REVERSAL
- TITLE_FAILURE
- COMPLIANCE_FAILURE
- VEHICLE_DEFECT
- FRAUD_DETECTED
- DEALERSHIP_ERROR
- LEGAL_REQUIREMENT
- OTHER

## 5. Validation Rules

### Business Rules

- A Deal may be created only from an eligible Opportunity in `COMMITMENT`.
- The Deal Customer must match the Opportunity Customer.
- The Deal Vehicle must match the selected Opportunity Vehicle or an explicitly approved replacement.
- A formal accepted Quotation is required when the applicable Sales Playbook requires one.
- Cash Deals may bypass the Quotation object only when dealership policy explicitly permits direct Deal creation.
- One Vehicle cannot belong to more than one active Deal simultaneously.
- One Opportunity cannot create more than one primary active Deal.
- Commercial terms must be frozen when the Deal is approved.
- Changes to approved commercial terms require a controlled amendment or replacement process.
- A Deal cannot proceed to contracting until required commercial approvals are complete.
- A financed Deal cannot proceed to delivery without approved and available funding.
- A cash Deal cannot proceed to delivery until required cleared funds are received.
- A Vehicle cannot be delivered until contracts, identity checks, compliance checks, insurance, title, registration, and payment conditions pass.
- A delivered Deal cannot be cancelled through the ordinary cancellation process; it requires an authorized unwind.
- Deal cancellation must release the Vehicle reservation when legally and operationally permitted.
- Customer-facing documents must not expose Vehicle cost, gross profit, margin, or internal approval thresholds.
- Revenue recognition must follow the authoritative accounting policy and must not rely only on Deal status.

### Technical Rules

- Deal creation must use an idempotency key.
- Deal creation, Opportunity conversion, Vehicle reservation, and Quotation linkage must occur in a controlled transaction or compensating workflow.
- Every financial calculation must use fixed decimal precision.
- Server-side calculations are authoritative.
- `record_version` must increase after every permitted update.
- Approved commercial and Customer snapshots must be immutable.
- Every lifecycle transition must create an immutable audit entry.
- Every external DMS or accounting sync must store its request, response, external identifier, and reconciliation state.
- Payment totals must be derived from cleared Payment records.
- Funding totals must be derived from authorized Finance and lender events.
- Delivery readiness must be recalculated whenever a blocking dependency changes.
- Failed multi-service operations must create compensating actions and Human Review Tasks.

### Data Constraints

- Monetary values cannot be negative unless an approved accounting adjustment applies.
- `vehicle_discount_amount` cannot exceed `vehicle_list_price`.
- `trade_in_net_equity_amount` must equal `trade_in_allowance_amount - trade_in_payoff_amount`.
- `amount_paid` must equal the total of cleared and non-reversed Payments.
- `balance_due_amount` must equal `total_due_amount - amount_paid - confirmed_external_funding`.
- `deposit_received_amount` cannot exceed `amount_paid`.
- `down_payment_amount` cannot exceed `total_due_amount`.
- `gross_margin_percentage` must correspond to the approved profitability formula.
- `contract_signed_at` cannot precede `contract_started_at`.
- `delivered_at` cannot precede `contract_signed_at`.
- `closed_at` cannot precede `created_at`.
- `replacement_deal_id` must be null unless an unwind or replacement process exists.
- `cancelled_at` and `delivered_at` cannot both be populated unless the Deal later enters `UNWOUND`.
- `accepted_quotation_id` must reference the exact accepted immutable version.

### Referential Integrity

- Every linked entity must belong to the same `dealership_id`.
- `customer_id` must match the Customer linked to the Opportunity and accepted Quotation.
- `vehicle_id` must match the Vehicle contained in the accepted commercial snapshot.
- `accepted_quotation_id` must belong to the same Opportunity.
- `trade_in_id` must belong to the same Customer and Opportunity.
- `finance_application_id` must belong to the same Customer, Opportunity, and Deal context.
- `contract_id`, `delivery_id`, `reservation_id`, and `compliance_case_id` must reference this exact Deal.
- `replacement_deal_id` cannot reference the current Deal.
- Circular original and replacement Deal relationships are prohibited.
- A Deal cannot be hard-deleted while referenced by Contracts, Payments, Finance Applications, Deliveries, accounting entries, DMS records, or audit logs.

### Human Approval Requirements

- Discounts or margins outside delegated authority require authorized approval.
- Manual tax, fee, or pricing overrides require documented approval.
- Changing the Customer or Vehicle after Deal approval requires Sales Manager approval and controlled revalidation.
- Changing approved commercial terms requires a new approval cycle.
- Cancelling an approved Deal requires an authorized User and documented reason.
- Unwinding a delivered or funded Deal requires Sales Manager, Finance Manager, and any required legal or accounting approval.
- AI Agents cannot approve, cancel, unwind, fund, sign, deliver, or close a Deal.
- AI Agents cannot record Payments or Customer signatures without verifiable external evidence.
- AI Agents cannot override compliance, finance, title, insurance, or registration blocks.
- Conflicting financial or identity evidence must create a Human Review Task.

## 6. State Machine

### Allowed States

- CREATED
- PENDING_APPROVAL
- APPROVED
- VEHICLE_RESERVED
- CONTRACTING
- AWAITING_PAYMENT
- AWAITING_FUNDING
- COMPLIANCE_REVIEW
- DELIVERY_PREPARATION
- READY_FOR_DELIVERY
- DELIVERED
- CLOSED
- CANCELLED
- UNWOUND

### Allowed Transitions

- CREATED → PENDING_APPROVAL
- CREATED → APPROVED
- CREATED → CANCELLED
- PENDING_APPROVAL → APPROVED
- PENDING_APPROVAL → CREATED
- PENDING_APPROVAL → CANCELLED
- APPROVED → VEHICLE_RESERVED
- APPROVED → CANCELLED
- VEHICLE_RESERVED → CONTRACTING
- VEHICLE_RESERVED → CANCELLED
- CONTRACTING → AWAITING_PAYMENT
- CONTRACTING → AWAITING_FUNDING
- CONTRACTING → COMPLIANCE_REVIEW
- CONTRACTING → DELIVERY_PREPARATION
- CONTRACTING → CANCELLED
- AWAITING_PAYMENT → AWAITING_FUNDING
- AWAITING_PAYMENT → COMPLIANCE_REVIEW
- AWAITING_PAYMENT → DELIVERY_PREPARATION
- AWAITING_PAYMENT → CANCELLED
- AWAITING_FUNDING → COMPLIANCE_REVIEW
- AWAITING_FUNDING → DELIVERY_PREPARATION
- AWAITING_FUNDING → CANCELLED
- COMPLIANCE_REVIEW → AWAITING_PAYMENT
- COMPLIANCE_REVIEW → AWAITING_FUNDING
- COMPLIANCE_REVIEW → DELIVERY_PREPARATION
- COMPLIANCE_REVIEW → CANCELLED
- DELIVERY_PREPARATION → READY_FOR_DELIVERY
- DELIVERY_PREPARATION → COMPLIANCE_REVIEW
- DELIVERY_PREPARATION → AWAITING_PAYMENT
- DELIVERY_PREPARATION → AWAITING_FUNDING
- DELIVERY_PREPARATION → CANCELLED
- READY_FOR_DELIVERY → DELIVERED
- READY_FOR_DELIVERY → DELIVERY_PREPARATION
- DELIVERED → CLOSED
- DELIVERED → UNWOUND
- CLOSED → UNWOUND

### Forbidden Transitions

- CREATED → CONTRACTING
- CREATED → DELIVERED
- PENDING_APPROVAL → VEHICLE_RESERVED
- PENDING_APPROVAL → CONTRACTING
- APPROVED → DELIVERED
- CONTRACTING → DELIVERED
- AWAITING_PAYMENT → DELIVERED
- AWAITING_FUNDING → DELIVERED
- COMPLIANCE_REVIEW → DELIVERED
- DELIVERY_PREPARATION → DELIVERED
- CANCELLED → APPROVED
- CANCELLED → CONTRACTING
- CANCELLED → DELIVERED
- UNWOUND → DELIVERED
- CLOSED → CANCELLED

### Entry Conditions

- To enter `PENDING_APPROVAL`:
  - Required Deal fields and commercial terms must be complete.
  - Every required approval reason must be identified.
  - The Vehicle must remain eligible.

- To enter `APPROVED`:
  - Required commercial approvals must be completed.
  - The accepted commercial snapshot must be validated.
  - Pricing, taxes, fees, discounts, and incentives must be current.
  - No unresolved approval rejection may exist.

- To enter `VEHICLE_RESERVED`:
  - The Vehicle must be available.
  - The reservation operation must succeed.
  - No competing active Deal may control the Vehicle.
  - The Vehicle status must change to `RESERVED`.

- To enter `CONTRACTING`:
  - Deal approval must remain valid.
  - The Vehicle reservation must remain active.
  - Required Customer and transaction data must be complete.
  - Contract-generation prerequisites must pass.

- To enter `AWAITING_PAYMENT`:
  - The required Customer-payment amount must be known.
  - Payment instructions or authorized collection workflow must exist.

- To enter `AWAITING_FUNDING`:
  - The Deal must require external finance or lender funding.
  - A valid Finance Application must exist.
  - Funding requirements and outstanding conditions must be documented.

- To enter `COMPLIANCE_REVIEW`:
  - Required Customer, Vehicle, identity, title, insurance, registration, and regulatory checks must be identified.
  - A Compliance Case or equivalent workflow must exist.

- To enter `DELIVERY_PREPARATION`:
  - Contracts must be signed or satisfy the dealership's approved execution standard.
  - Mandatory payment and funding thresholds must be satisfied.
  - No critical compliance block may exist.
  - Vehicle preparation may begin safely.

- To enter `READY_FOR_DELIVERY`:
  - Contract status must be `SIGNED`.
  - Compliance status must be `CLEARED`.
  - Required funds must be cleared or irrevocably approved.
  - Insurance and registration requirements must pass.
  - Vehicle inspection and preparation must be complete.
  - Delivery Appointment and handover documents must be ready.

- To enter `DELIVERED`:
  - Delivery readiness must equal `READY`.
  - Authorized handover must occur.
  - Customer identity must be verified.
  - Delivery evidence and Vehicle condition confirmation must be recorded.
  - `delivered_at` must be populated.

- To enter `CLOSED`:
  - The Deal must be delivered.
  - Final DMS, accounting, payment, funding, commission, and inventory postings must reconcile.
  - No unresolved blocking exception may remain.

- To enter `CANCELLED`:
  - The Vehicle must not have been delivered.
  - An authorized cancellation reason must be recorded.
  - Payment-refund and reservation-release consequences must be identified.

- To enter `UNWOUND`:
  - The Deal must previously have been delivered or closed.
  - An authorized unwind reason and approval package must exist.
  - Accounting, payment, Vehicle, title, registration, finance, and Customer consequences must be documented.
  - A replacement Deal must be linked when applicable.

### Exit Conditions

- A Deal cannot exit `CREATED` until required validation completes.
- A Deal cannot exit `PENDING_APPROVAL` without approval, revision, or cancellation.
- A Deal cannot exit `APPROVED` toward contracting until Vehicle reservation succeeds.
- A Deal cannot exit `VEHICLE_RESERVED` while a reservation conflict exists.
- A Deal cannot exit `CONTRACTING` toward delivery preparation until contract requirements are satisfied.
- A Deal cannot exit `AWAITING_PAYMENT` until the required payment threshold is met or an authorized exception exists.
- A Deal cannot exit `AWAITING_FUNDING` until funding is approved, received, or formally replaced.
- A Deal cannot exit `COMPLIANCE_REVIEW` while a critical compliance block exists.
- A Deal cannot exit `READY_FOR_DELIVERY` without verified handover evidence.
- A Deal cannot exit `DELIVERED` toward `CLOSED` until external systems reconcile.
- A `CANCELLED` Deal cannot return to an active state; a new Deal must be created.
- An `UNWOUND` Deal cannot return to an active state; a replacement Deal must be created when required.

### Terminal States

- **CLOSED:** The completed transaction was delivered and reconciled.
- **CANCELLED:** The transaction ended before delivery.
- **UNWOUND:** A delivered or closed transaction was formally reversed.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Opportunity identified by `opportunity_id`.
  - Customer identified by `customer_id`.
  - Vehicle identified by `vehicle_id`.
  - Authorized Sales Consultant identified by `owner_id`.
  - Applicable pricing, approval, compliance, payment, and delivery rules.

- **Consumes:**
  - Opportunity commitment and requirements context.
  - Accepted Quotation or approved direct-sale commercial terms.
  - Customer identity and contact information.
  - Vehicle availability, cost, specifications, and title status.
  - Trade-In appraisal, payoff, and ownership evidence.
  - Finance Application decisions and lender conditions.
  - Cleared Payment records.
  - Contract signatures and document-verification results.
  - Compliance, insurance, registration, and delivery-readiness results.

- **Produces:**
  - Authoritative transaction state.
  - Approved commercial and profitability snapshots.
  - Vehicle reservation and inventory instructions.
  - Contract-generation payloads.
  - Payment and funding requirements.
  - Delivery-readiness context.
  - Accounting and DMS posting instructions.
  - Commission and revenue-reporting context.
  - Post-sale Customer Journey context.

- **Creates:**
  - Vehicle Reservation.
  - Contract package.
  - Payment requests.
  - Compliance Case.
  - Delivery record.
  - Approval requests.
  - Refund or unwind workflows when required.

- **Triggers:**
  - Deal-approval workflows.
  - Vehicle reservation.
  - Contract preparation.
  - Payment collection.
  - Finance and funding monitoring.
  - Compliance verification.
  - Delivery preparation.
  - DMS and accounting synchronization.
  - Customer post-sale follow-up.
  - Cancellation or unwind processing.

- **Owned By:**
  - The Sales Consultant identified by `owner_id`.
  - Commercial and financial approvals remain owned by authorized management and finance roles.

- **Referenced By:**
  - Contract
  - Payment
  - Finance Application
  - Trade-In
  - Vehicle Reservation
  - Compliance Case
  - Delivery
  - Appointment
  - Commission Record
  - Accounting Entry
  - Customer Journey
  - Interaction Log
  - Document Vault
  - AI Agent Run

- **Derived From:**
  - The approved commercial terms of the accepted Quotation or authorized direct-sale transaction.

- **Replaces / Replaced By:**
  - An unwound Deal may reference a replacement Deal using `replacement_deal_id`.
  - The replacement Deal must preserve a reference to the original Deal using `original_deal_id`.

## 8. Domain Events

### Emitted Events

- **DealCreated**  
  Payload: `deal_id`, `opportunity_id`, `customer_id`, `vehicle_id`, `deal_number`, `created_at`

- **DealApprovalRequested**  
  Payload: `deal_id`, `approval_reason`, `requested_by`, `submitted_for_approval_at`

- **DealApproved**  
  Payload: `deal_id`, `approved_by`, `approved_at`, `approval_evidence_reference`

- **DealApprovalRejected**  
  Payload: `deal_id`, `rejected_by`, `rejection_reason`, `rejected_at`

- **DealVehicleReserved**  
  Payload: `deal_id`, `vehicle_id`, `reservation_id`, `reserved_at`

- **DealContractingStarted**  
  Payload: `deal_id`, `contract_id`, `contract_started_at`

- **DealContractSigned**  
  Payload: `deal_id`, `contract_id`, `contract_signed_at`, `document_hash`

- **DealDepositReceived**  
  Payload: `deal_id`, `payment_id`, `deposit_received_amount`, `deposit_received_at`

- **DealPaymentStatusChanged**  
  Payload: `deal_id`, `old_status`, `new_status`, `amount_paid`, `balance_due_amount`

- **DealFundingStatusChanged**  
  Payload: `deal_id`, `old_status`, `new_status`, `finance_application_id`, `changed_at`

- **DealComplianceStatusChanged**  
  Payload: `deal_id`, `old_status`, `new_status`, `compliance_case_id`, `changed_at`

- **DealDeliveryPreparationStarted**  
  Payload: `deal_id`, `vehicle_id`, `delivery_id`, `started_at`

- **DealReadyForDelivery**  
  Payload: `deal_id`, `delivery_id`, `delivery_ready_at`

- **DealDelivered**  
  Payload: `deal_id`, `vehicle_id`, `customer_id`, `delivery_id`, `delivered_at`

- **DealClosed**  
  Payload: `deal_id`, `closed_at`, `dms_reference_id`, `accounting_reference_id`

- **DealCancelled**  
  Payload: `deal_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

- **DealUnwindRequested**  
  Payload: `deal_id`, `unwind_reason`, `requested_by`, `requested_at`

- **DealUnwound**  
  Payload: `deal_id`, `replacement_deal_id`, `unwind_reason`, `unwound_by`, `unwound_at`

- **DealExceptionDetected**  
  Payload: `deal_id`, `exception_type`, `severity`, `detected_at`

### Consumed Events

- **OpportunityCommitmentRecorded**  
  Makes the Opportunity eligible for controlled Deal creation.

- **QuotationAccepted**  
  Supplies the accepted immutable commercial snapshot.

- **VehicleStatusChanged**  
  Revalidates Vehicle availability and reservation eligibility.

- **VehicleReservationCreated**  
  Populates `reservation_id` and moves the Deal to `VEHICLE_RESERVED`.

- **VehicleReservationFailed**  
  Blocks contracting and creates an exception or Human Review Task.

- **ContractGenerated**  
  Populates `contract_id` and updates contract status.

- **ContractSigned**  
  Updates `contract_status` and records signature evidence.

- **PaymentCleared**  
  Updates `amount_paid`, deposit totals, payment status, and balance due.

- **PaymentReversed**  
  Recalculates payment status and may block delivery or trigger unwind review.

- **FinanceApplicationApproved**  
  Updates funding terms and permits progression toward delivery.

- **FinanceApplicationDeclined**  
  Blocks funded progression and creates a restructuring or cancellation workflow.

- **BankFundingReceived**  
  Updates `funding_status` to `FUNDED`.

- **TradeInAppraisalCompleted**  
  Supplies approved Trade-In values.

- **TradeInPayoffVerified**  
  Supplies verified payoff and net-equity information.

- **ComplianceCaseCleared**  
  Updates `compliance_status` to `CLEARED`.

- **ComplianceCaseBlocked**  
  Prevents delivery progression.

- **DeliveryCompleted**  
  Transitions the Deal to `DELIVERED`.

- **DMSSyncCompleted**  
  Allows final reconciliation and Deal closure.

- **DMSSyncFailed**  
  Blocks closure and creates an operational exception.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- Customer-visible Deal summaries
- Contract-preparation summaries
- Delivery-preparation notes
- Customer objections and concerns
- Exception-resolution summaries
- Cancellation explanations
- Unwind explanations
- Non-sensitive interaction summaries
- Post-sale handoff summaries

### Fields Excluded from Embeddings

- `deal_id`
- `customer_id`
- `vehicle_id`
- `opportunity_id`
- `accepted_quotation_id`
- `finance_application_id`
- `contract_id`
- `payment_id`
- `vehicle_vin`
- Direct Customer contact information
- Identity documents
- Contract documents
- Payment references
- Bank-account information
- Finance documents
- `vehicle_cost_amount`
- `gross_profit_amount`
- `gross_margin_percentage`
- `commissionable_gross_amount`
- Internal approval thresholds
- `commercial_snapshot`
- `approval_evidence`
- `compliance_snapshot`

> Personally identifiable information, payment information, signed documents, exact internal profitability, and restricted commercial terms must be provided only through authorized structured context.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `status`
- `deal_type`
- `payment_method`
- `payment_status`
- `funding_status`
- `contract_status`
- `compliance_status`
- `document_status`
- `delivery_readiness_status`
- `balance_due_amount`
- `deposit_required_amount`
- `deposit_received_amount`
- Blocking requirements
- Next required operational action
- Customer-visible delivery information

Only authorized internal Agents may receive:

- `gross_profit_amount`
- `gross_margin_percentage`
- `vehicle_cost_amount`
- Approval requirements
- Internal exception details

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `deal_id`
- `customer_id`
- `vehicle_id`
- `opportunity_id`
- `owner_id`
- `status`
- `deal_type`
- `payment_status`
- `funding_status`
- `compliance_status`
- `delivery_readiness_status`

### Confidence Thresholds

- Contract-document classification requires confidence of at least `0.95`.
- Payment-evidence extraction requires confidence of at least `0.99`.
- Customer identity-document extraction requires confidence of at least `0.99`.
- Delivery-readiness recommendations require confidence of at least `0.95`.
- Cancellation-intent classification requires confidence of at least `0.95`.
- Operational next-action recommendations require confidence of at least `0.90`.
- No AI confidence score may replace authoritative Payment, lender, compliance, signature, or delivery evidence.

### Human Approval Thresholds

- AI Agents cannot approve, fund, sign, deliver, cancel, unwind, or close a Deal.
- AI Agents cannot record cleared funds without authoritative Payment evidence.
- AI Agents cannot validate Customer signatures without a trusted signature provider or authorized human verification.
- AI Agents cannot override lender, payment, compliance, title, registration, insurance, or delivery blocks.
- AI Agents cannot modify approved commercial snapshots directly.
- Conflicting identity, financial, contract, or Vehicle evidence must create a Human Review Task.
- Unwind recommendations must be reviewed by authorized management, finance, accounting, and legal roles when applicable.
- AI-generated operational actions remain recommendations until accepted by an authorized User or trusted system.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/deals`

### Methods

- `GET` — list or search Deals.
- `POST` — create a Deal through controlled Opportunity conversion.
- `GET /{id}` — retrieve one Deal.
- `PATCH /{id}` — update permitted non-frozen Deal fields.
- `POST /{id}/submit-for-approval` — request required approval.
- `POST /{id}/approve` — approve the Deal.
- `POST /{id}/reject-approval` — reject the Deal approval request.
- `POST /{id}/reserve-vehicle` — create the Vehicle reservation.
- `POST /{id}/start-contracting` — begin Contract preparation.
- `POST /{id}/recalculate` — recalculate authoritative financial totals.
- `POST /{id}/request-payment` — create an approved Payment request.
- `POST /{id}/start-compliance` — create or start the Compliance Case.
- `POST /{id}/prepare-delivery` — begin delivery preparation.
- `POST /{id}/mark-ready-for-delivery` — validate all delivery prerequisites.
- `POST /{id}/record-delivery` — record verified Vehicle handover.
- `POST /{id}/close` — close the Deal after DMS and accounting reconciliation.
- `POST /{id}/cancel` — cancel an eligible undelivered Deal.
- `POST /{id}/request-unwind` — request an authorized unwind.
- `POST /{id}/unwind` — complete an approved unwind process.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateDealRequest",
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
    "owner_id": {
      "type": "string",
      "format": "uuid"
    },
    "accepted_quotation_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "deal_type": {
      "type": "string",
      "enum": [
        "RETAIL_CASH",
        "RETAIL_FINANCE",
        "RETAIL_LEASE",
        "CORPORATE_CASH",
        "CORPORATE_FINANCE",
        "FLEET",
        "TRADE_IN_CASH",
        "TRADE_IN_FINANCE",
        "DEMO_SALE",
        "WHOLESALE"
      ]
    },
    "sales_channel": {
      "type": "string",
      "enum": [
        "SHOWROOM",
        "DIGITAL",
        "PHONE",
        "WHATSAPP",
        "MARKETPLACE",
        "OEM_PLATFORM",
        "FLEET_DESK",
        "REFERRAL",
        "OTHER"
      ]
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
    "total_fee_amount": {
      "type": "number",
      "minimum": 0
    },
    "total_tax_amount": {
      "type": "number",
      "minimum": 0
    },
    "total_due_amount": {
      "type": "number",
      "minimum": 0
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "deposit_required_amount": {
      "type": "number",
      "minimum": 0
    },
    "down_payment_amount": {
      "type": ["number", "null"],
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
    "commercial_snapshot": {
      "type": "object"
    }
  },
  "required": [
    "opportunity_id",
    "customer_id",
    "vehicle_id",
    "owner_id",
    "deal_type",
    "sales_channel",
    "payment_method",
    "vehicle_list_price",
    "vehicle_selling_price",
    "vehicle_discount_amount",
    "manufacturer_rebate_amount",
    "total_fee_amount",
    "total_tax_amount",
    "total_due_amount",
    "currency_code",
    "deposit_required_amount",
    "commercial_snapshot"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Deal {
  id: ID!
  dealershipId: ID!
  opportunityId: ID!
  customerId: ID!
  vehicleId: ID!
  ownerId: ID!
  acceptedQuotationId: ID
  tradeInId: ID
  financeApplicationId: ID
  reservationId: ID
  contractId: ID
  deliveryId: ID
  complianceCaseId: ID
  originalDealId: ID
  replacementDealId: ID
  dealNumber: String!
  dealType: DealType!
  salesChannel: DealSalesChannel!
  status: DealStatus!
  approvalStatus: DealApprovalStatus!
  fundingStatus: DealFundingStatus!
  paymentStatus: DealPaymentStatus!
  contractStatus: ContractStatus!
  complianceStatus: DealComplianceStatus!
  deliveryReadinessStatus: DeliveryReadinessStatus!
  vehicleListPrice: Float!
  vehicleSellingPrice: Float!
  vehicleDiscountAmount: Float!
  manufacturerRebateAmount: Float!
  totalDiscountAmount: Float!
  totalFeeAmount: Float!
  totalTaxAmount: Float!
  totalDueAmount: Float!
  currencyCode: String!
  depositRequiredAmount: Float!
  depositReceivedAmount: Float!
  downPaymentAmount: Float
  financePrincipalAmount: Float
  approvedInterestRate: Float
  financeTermMonths: Int
  monthlyPaymentAmount: Float
  amountPaid: Float!
  balanceDueAmount: Float!
  vehicleCostAmount: Float!
  grossProfitAmount: Float!
  grossMarginPercentage: Float!
  approvalRequired: Boolean!
  createdAt: DateTime!
  approvedAt: DateTime
  reservedAt: DateTime
  contractSignedAt: DateTime
  fundedAt: DateTime
  deliveryReadyAt: DateTime
  deliveredAt: DateTime
  cancelledAt: DateTime
  unwoundAt: DateTime
  closedAt: DateTime
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `deals`
- **Commercial Snapshot Table:** `deal_commercial_snapshots`
- **Vehicle Snapshot Table:** `deal_vehicle_snapshots`
- **Customer Snapshot Table:** `deal_customer_snapshots`
- **Status-History Table:** `deal_status_history`
- **Approval Table:** `deal_approvals`
- **Exception Table:** `deal_exceptions`
- **External-Sync Table:** `deal_external_sync`
- **Audit Table:** `deal_audit_log`

### Related Transaction Tables

- `contracts`
- `payments`
- `finance_applications`
- `trade_ins`
- `vehicle_reservations`
- `compliance_cases`
- `deliveries`
- `refunds`
- `commission_records`
- `accounting_entries`

### Indexes

- `idx_deals_tenant_status (dealership_id, status)`  
  Used for operational Deal queues.

- `idx_deals_customer (dealership_id, customer_id, created_at DESC)`  
  Used for Customer transaction history.

- `idx_deals_vehicle (dealership_id, vehicle_id, status)`  
  Used to prevent competing active Deals for one Vehicle.

- `idx_deals_opportunity (opportunity_id)`  
  Used to validate Opportunity conversion.

- `idx_deals_owner_status (dealership_id, owner_id, status)`  
  Used for Sales Consultant work queues.

- `idx_deals_approval (dealership_id, approval_status, submitted_for_approval_at)`  
  Used for Deal-approval queues.

- `idx_deals_payment_status (dealership_id, payment_status, status)`  
  Used for outstanding-payment monitoring.

- `idx_deals_funding_status (dealership_id, funding_status, status)`  
  Used for finance and funding queues.

- `idx_deals_delivery_status (dealership_id, delivery_readiness_status, status)`  
  Used for delivery-preparation dashboards.

- `idx_deals_number (dealership_id, deal_number)`  
  Used for human-readable Deal lookup.

- `idx_deals_replacement (original_deal_id, replacement_deal_id)`  
  Used for unwind and replacement tracing.

### Unique Constraints

- `UQ_deal_number (dealership_id, deal_number)`
- `UQ_deal_primary_opportunity (opportunity_id)`  
  Applies to the primary non-cancelled Deal.

- `UQ_active_vehicle_deal (dealership_id, vehicle_id)`  
  Applies while the Deal remains in a non-terminal active state.

- `UQ_deal_reservation (reservation_id)`  
  Applies when `reservation_id` is not null.

- `UQ_deal_contract (contract_id)`  
  Applies when `contract_id` is not null.

- `UQ_deal_delivery (delivery_id)`  
  Applies when `delivery_id` is not null.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `opportunity_id` → `opportunities(id)`
- `customer_id` → `customers(id)`
- `vehicle_id` → `vehicles(id)`
- `owner_id` → `users(id)`
- `accepted_quotation_id` → `quotations(id)` — nullable
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)` — nullable
- `reservation_id` → `vehicle_reservations(id)` — nullable
- `contract_id` → `contracts(id)` — nullable
- `delivery_id` → `deliveries(id)` — nullable
- `compliance_case_id` → `compliance_cases(id)` — nullable
- `replacement_deal_id` → `deals(id)` — nullable
- `original_deal_id` → `deals(id)` — nullable
- `approved_by` → `users(id)` — nullable
- `sales_manager_id` → `users(id)` — nullable
- `finance_manager_id` → `users(id)` — nullable

### Database Constraints

- `vehicle_discount_amount <= vehicle_list_price`
- `trade_in_net_equity_amount = trade_in_allowance_amount - trade_in_payoff_amount`
- `deposit_received_amount <= amount_paid`
- `down_payment_amount <= total_due_amount`
- `amount_paid` must equal the total of cleared, non-reversed Payments.
- `balance_due_amount` must equal the authoritative outstanding-balance formula.
- `contract_signed_at >= contract_started_at`
- `delivered_at >= contract_signed_at`
- `closed_at >= delivered_at` when status is `CLOSED`.
- `replacement_deal_id != deal_id`
- `original_deal_id != deal_id`
- Circular Deal-replacement relationships are prohibited.
- Approved commercial snapshots are immutable.
- `status = DELIVERED` requires `delivery_id` and `delivered_at`.
- `status = CLOSED` requires successful DMS and accounting reconciliation.
- `status = CANCELLED` requires `cancellation_reason` and `cancelled_at`.
- `status = UNWOUND` requires `unwind_reason` and `unwound_at`.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical Deals by `created_at`.
- Transaction, status-history, approval, snapshot, exception, sync, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read access and limited permitted updates for Deals assigned to them.
- **Sales Manager:** Approval, reassignment, cancellation-request, and operational oversight within the matching dealership.
- **GSM / Executive Approver:** Approval authority for exceptional margin, pricing, and unwind scenarios.
- **Finance Manager:** Access to funding, payment, finance, profitability, and reconciliation fields.
- **Accounting User:** Access to cleared Payments, postings, refunds, revenue, and reconciliation records.
- **Compliance User:** Access to identity, title, regulatory, insurance, registration, and compliance evidence.
- **Delivery Coordinator:** Access to delivery-readiness and Vehicle-handover fields without unrestricted profitability access.
- **Inventory User:** Access to Vehicle allocation and reservation details without unrestricted Customer financial data.
- **AI Deal Agent:** Service Account access limited to summaries, recommendations, permitted data extraction, exception detection, and approved workflow requests.
- **Deal Service:** Controlled transaction access using tenant-scoped service credentials.
- **Payment Service:** Restricted access to Payment and balance fields.
- **Contract Service:** Restricted access to Contract preparation and signature status.
- **DMS Integration Service:** Restricted access to approved synchronization and reconciliation operations.

### PII Classification

- **Level:** `CRITICAL_PII`

The Deal may contain or reference:

- Customer identity information
- Contact information
- Residential or business address
- Identity documents
- Tax identifiers
- Insurance information
- Registration information
- Contract signatures
- Trade-In ownership information
- Finance and payment information

### Financially Sensitive Fields

- `total_due_amount`
- `deposit_received_amount`
- `down_payment_amount`
- `finance_principal_amount`
- `approved_interest_rate`
- `monthly_payment_amount`
- `amount_paid`
- `balance_due_amount`
- Bank or Payment references
- Lender information
- Refund information

### Commercially Sensitive Fields

- `vehicle_cost_amount`
- `vehicle_discount_amount`
- `dealer_incentive_amount`
- `gross_profit_amount`
- `gross_margin_percentage`
- `front_end_gross_amount`
- `back_end_gross_amount`
- `commissionable_gross_amount`
- Internal approval thresholds
- Manager-override reasons

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, documents, snapshots, backups, and event stores.
- **Column-Level Protection:** Customer identity, payment references, finance details, tax identifiers, insurance information, signatures, compliance evidence, and profitability fields require encryption, tokenization, or equivalent approved protection.
- Contracts and supporting documents must be stored in an encrypted Document Vault.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every Deal-state transition must create an immutable audit record containing:
  - `deal_id`
  - `previous_status`
  - `new_status`
  - `actor_id`
  - `timestamp`
  - `reason`
  - `supporting_evidence`

- Every commercial-field change must record:
  - Previous value
  - New value
  - Actor
  - Timestamp
  - Approval reference
  - Business-rule version

- Every approval decision must record:
  - Approver
  - Authority level
  - Decision
  - Reason
  - Supporting evidence
  - Timestamp

- Every Payment or funding change must record:
  - External transaction reference
  - Amount
  - Currency
  - Previous status
  - New status
  - Source system
  - Timestamp

- Contract execution must preserve:
  - Document hash
  - Signer identity
  - Signature provider reference
  - Signature timestamp
  - Contract version

- Delivery must preserve:
  - Authorized handover User
  - Customer identity-verification evidence
  - Vehicle condition evidence
  - Odometer reading
  - Delivery timestamp
  - Customer acknowledgement

- Cancellation and unwind operations must preserve:
  - Reason
  - Approvers
  - Payment consequences
  - Vehicle consequences
  - Accounting consequences
  - Replacement Deal reference
  - Timestamp

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to Customer, finance, payment, contract, profitability, and compliance data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, Opportunity, Vehicle, Quotation, Trade-In, Finance Application, Contract, Payment, Delivery, or Deal linking is prohibited.
- AI Agents, Jobs, integrations, exports, and semantic retrieval must receive tenant scope through signed execution context.
- External document and payment links must be tenant-scoped, time-limited, and cryptographically protected.

### Retention and Deletion

- Deal records are financial and transactional records and must follow the applicable legal, tax, accounting, and dealership retention policy.
- Closed, Cancelled, and Unwound Deals must remain immutable.
- Hard deletion is prohibited while Contracts, Payments, finance records, delivery records, accounting entries, DMS records, or audit dependencies exist.
- Soft deletion may hide eligible operational records without destroying their audit history.
- Legally approved deletion requests must purge or anonymize permitted PII while preserving financial records that must legally remain.
- Deletion and anonymization must address:
  - Deal records
  - Customer and Vehicle snapshots
  - Contracts and documents
  - Payment and finance references
  - Compliance evidence
  - Delivery evidence
  - Vector stores
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
