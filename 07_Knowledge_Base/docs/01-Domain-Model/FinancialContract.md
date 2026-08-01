# Financial Contract

## 1. Object Purpose

### Business Purpose

The Financial Contract object represents the legally governed agreement that records the approved financial obligations, rights, payment terms, security conditions, and signatures associated with financing or leasing a Vehicle.

It is created after:

- A Finance Application receives an eligible lender approval.
- The Customer selects an approved finance offer.
- The related Quotation and Deal commercial terms are confirmed.
- Mandatory identity, compliance, disclosure, and document requirements are satisfied.

The Financial Contract enables the dealership and authorized financial parties to:

- Preserve the exact finance terms accepted by the Customer.
- Record applicant, co-applicant, guarantor, dealership, and lender obligations.
- Generate legally approved contract documents.
- Present mandatory disclosures.
- Collect electronic or physical signatures.
- Validate signature completeness and authority.
- Track contract activation and lender funding.
- Maintain payment-schedule and settlement references.
- Support amendments, cancellation, termination, or completion.
- Preserve immutable legal and audit evidence.

A Financial Contract does not itself prove that:

- The Customer has received the Vehicle.
- The lender has transferred cleared funds.
- A payment has been received.
- Vehicle ownership has legally transferred.
- Every Deal obligation has been completed.

These outcomes must be confirmed through the applicable Deal, Payment, Funding Transaction, Vehicle Delivery, and ownership-registration records.

### System Purpose

The Financial Contract object is the canonical legally binding financial-agreement aggregate within the ASOS domain.

It connects:

- Dealership
- Customer
- Co-Applicant
- Guarantor
- Opportunity
- Quotation
- Deal
- Vehicle
- Trade-In
- Finance Application
- Lender
- Finance Program
- Payment Schedule
- Funding Transaction
- Document Vault
- Signature Provider
- Compliance Case
- User
- AI Agent

The object preserves the complete contract lifecycle from contract generation through:

- Legal-template selection.
- Commercial and finance-term validation.
- Disclosure generation.
- Internal review.
- Customer presentation.
- Signature collection.
- Counter-signature.
- Contract effectiveness.
- Lender-funding eligibility.
- Active servicing reference.
- Completion, cancellation, voiding, expiry, or termination.

Every Financial Contract must be:

- Tenant-scoped.
- Version-controlled.
- Linked to one exact approved finance offer.
- Traceable to authoritative commercial and lender sources.
- Immutable after complete execution.
- Protected according to legal and financial-data requirements.
- Supported by verifiable signature and document evidence.

AI Agents may assist with completeness checks, summaries, document classification, and workflow coordination, but they cannot create legal consent, sign a contract, alter authoritative terms, or determine legal enforceability.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `financial_contract_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — required)
  - `co_applicant_customer_id` (UUIDv4 — optional)
  - `guarantor_customer_id` (UUIDv4 — optional)
  - `opportunity_id` (UUIDv4 — required)
  - `quotation_id` (UUIDv4 — required)
  - `deal_id` (UUIDv4 — required)
  - `vehicle_id` (UUIDv4 — required)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — required)
  - `lender_id` (UUIDv4 — required)
  - `finance_program_id` (UUIDv4 — required)
  - `payment_schedule_id` (UUIDv4 — optional)
  - `funding_transaction_id` (UUIDv4 — optional)
  - `document_id` (UUIDv4 — optional until document generation)
  - `compliance_case_id` (UUIDv4 — optional)
  - `contract_template_id` (UUIDv4 — required)
  - `supersedes_contract_id` (UUIDv4 — optional)
  - `assigned_finance_user_id` (UUIDv4 — required)

### Contract Identity

- `contract_number`
- `contract_type`
- `status`
- `execution_method`
- `contract_version`
- `is_current_version`
- `supersedes_contract_id`
- `external_contract_reference`
- `lender_contract_reference`
- `template_version`
- `governing_law_code`
- `jurisdiction_code`

### Party Information

- `primary_customer_id`
- `co_applicant_customer_id`
- `guarantor_customer_id`
- `dealership_legal_entity_id`
- `lender_id`
- `party_snapshot`
- `customer_legal_name`
- `co_applicant_legal_name`
- `guarantor_legal_name`
- `dealership_legal_name`
- `lender_legal_name`
- `authorized_signatory_ids`
- `party_verification_status`

### Commercial Terms

- `currency_code`
- `vehicle_cash_price_amount`
- `vehicle_selling_price_amount`
- `total_transaction_amount`
- `customer_down_payment_amount`
- `trade_in_allowance_amount`
- `trade_in_payoff_amount`
- `trade_in_net_equity_amount`
- `fees_amount`
- `taxes_amount`
- `insurance_amount`
- `service_products_amount`
- `deposit_amount`
- `amount_due_at_signing`
- `commercial_terms_snapshot`

### Finance Terms

- `principal_amount`
- `financed_amount`
- `annual_interest_rate`
- `annual_percentage_rate`
- `effective_interest_rate`
- `finance_charge_amount`
- `total_repayment_amount`
- `term_months`
- `payment_frequency`
- `installment_count`
- `installment_amount`
- `first_payment_date`
- `final_payment_date`
- `balloon_payment_amount`
- `residual_value_amount`
- `late_payment_fee_amount`
- `early_settlement_policy`
- `early_settlement_fee_amount`
- `grace_period_days`
- `finance_terms_snapshot`

### Vehicle and Security Fields

- `vehicle_snapshot`
- `vin`
- `registration_number`
- `make`
- `model`
- `trim`
- `model_year`
- `vehicle_condition`
- `security_interest_type`
- `lien_registration_required`
- `lien_registration_status`
- `collateral_value_amount`
- `loan_to_value_ratio`
- `insurance_required`
- `insurance_verification_status`

### Disclosure Fields

- `disclosure_package_id`
- `required_disclosure_types`
- `presented_disclosure_types`
- `missing_disclosure_types`
- `disclosure_status`
- `disclosure_version`
- `disclosure_presented_at`
- `disclosure_acknowledged_at`
- `disclosure_evidence_hash`
- `cooling_off_period_applies`
- `cooling_off_end_at`

### Signature Fields

- `signature_provider`
- `signature_envelope_id`
- `signature_status`
- `required_signer_count`
- `completed_signer_count`
- `customer_signature_status`
- `co_applicant_signature_status`
- `guarantor_signature_status`
- `dealership_signature_status`
- `lender_signature_status`
- `signature_request_sent_at`
- `customer_signed_at`
- `co_applicant_signed_at`
- `guarantor_signed_at`
- `dealership_signed_at`
- `lender_signed_at`
- `fully_signed_at`
- `signature_evidence_hash`
- `signature_certificate_reference`

### Contract Document Fields

- `contract_template_id`
- `template_version`
- `document_id`
- `document_hash`
- `document_generation_status`
- `document_generated_at`
- `signed_document_id`
- `signed_document_hash`
- `document_language_code`
- `page_count`
- `document_storage_reference`
- `customer_copy_delivered_at`
- `document_snapshot`

### Compliance and Verification

- `identity_verification_status`
- `applicant_verification_status`
- `co_applicant_verification_status`
- `guarantor_verification_status`
- `compliance_status`
- `sanctions_screening_status`
- `fraud_review_status`
- `affordability_confirmation_status`
- `lender_approval_confirmation_status`
- `contract_terms_validation_status`
- `legal_review_status`
- `document_completeness_status`
- `compliance_snapshot`

### Effectiveness and Funding

- `effective_at`
- `funding_eligibility_status`
- `funding_request_status`
- `funding_requested_at`
- `funding_transaction_id`
- `funding_reference`
- `funded_amount`
- `funding_received_at`
- `funding_reconciliation_status`
- `activation_status`
- `activated_at`

### Payment Schedule

- `payment_schedule_id`
- `schedule_generation_status`
- `scheduled_payment_count`
- `scheduled_payment_total_amount`
- `next_payment_date`
- `next_payment_amount`
- `last_scheduled_payment_date`
- `payment_schedule_snapshot`

### Amendment and Termination

- `amendment_count`
- `last_amended_at`
- `amendment_reason`
- `amendment_snapshot`
- `cancellation_reason`
- `void_reason`
- `termination_reason`
- `termination_effective_at`
- `settlement_status`
- `settlement_amount`
- `settled_at`
- `completion_reason`

### Computed Fields

- `total_upfront_amount`
- `total_finance_cost_amount`
- `remaining_contract_amount`
- `contract_age_days`
- `days_until_first_payment`
- `days_until_final_payment`
- `days_until_cooling_off_end`
- `signature_completion_percentage`
- `disclosure_completion_percentage`
- `contract_completion_percentage`
- `funding_shortfall_amount`
- `is_fully_signed`
- `is_effective`
- `is_funding_eligible`
- `is_expired`
- `has_active_amendment`

### Governance and Lifecycle

- **Party Snapshot:** `party_snapshot` (Encrypted JSONB)
- **Commercial Terms Snapshot:** `commercial_terms_snapshot` (Encrypted JSONB)
- **Finance Terms Snapshot:** `finance_terms_snapshot` (Encrypted JSONB)
- **Vehicle Snapshot:** `vehicle_snapshot` (JSONB)
- **Disclosure Snapshot:** `disclosure_snapshot` (Encrypted JSONB)
- **Signature Snapshot:** `signature_snapshot` (Encrypted JSONB)
- **Document Snapshot:** `document_snapshot` (Encrypted JSONB)
- **Compliance Snapshot:** `compliance_snapshot` (Encrypted JSONB)
- **Funding Snapshot:** `funding_snapshot` (Encrypted JSONB)
- **Payment Schedule Snapshot:** `payment_schedule_snapshot` (Encrypted JSONB)
- **Amendment Snapshot:** `amendment_snapshot` (Encrypted JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `generated_by`
  - `reviewed_by`
  - `approved_by`
  - `sent_for_signature_by`
  - `countersigned_by`
  - `activated_by`
  - `cancelled_by`
  - `voided_by`
  - `terminated_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)

- **Soft Delete:**
  - `is_deleted`
  - `deleted_at`

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `generated_at`
  - `reviewed_at`
  - `approved_at`
  - `signature_request_sent_at`
  - `fully_signed_at`
  - `effective_at`
  - `funding_requested_at`
  - `funding_received_at`
  - `activated_at`
  - `last_amended_at`
  - `cancelled_at`
  - `voided_at`
  - `expired_at`
  - `terminated_at`
  - `completed_at`
  - `archived_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| financial_contract_id | UUID | Unique canonical identifier for the Financial Contract. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the contract. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Primary Customer entering the contract. | Yes | N/A | Must reference the Customer in the linked Deal and Finance Application | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| co_applicant_customer_id | UUID | Joint applicant entering the contract. | No | Null | Required when the approved Finance Application contains a co-applicant | 888e9999-e00b-11d2-a222-426614174000 | System-controlled |
| guarantor_customer_id | UUID | Guarantor supporting the Customer's obligations. | No | Null | Required when lender approval includes a guarantor condition | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| opportunity_id | UUID | Opportunity associated with the financed transaction. | Yes | N/A | Must belong to the same Customer and dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| quotation_id | UUID | Approved Quotation supplying commercial terms. | Yes | N/A | Must be valid, current, and linked to the Deal | 444e5555-e66b-77d8-a999-426614174000 | System-controlled |
| deal_id | UUID | Deal governed by the Financial Contract. | Yes | N/A | Must reference an active Deal in the same tenant | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| vehicle_id | UUID | Vehicle financed or leased under the contract. | Yes | N/A | Must match the Vehicle in the Deal and Finance Application | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| finance_application_id | UUID | Approved Finance Application supporting the contract. | Yes | N/A | Must contain a valid accepted lender offer | 333e4444-e55b-66d7-a888-426614174000 | System-controlled |
| lender_id | UUID | Lender entering or funding the contract. | Yes | From approved offer | Must match the selected approved lender | 222e3333-e44b-55d6-a777-426614174000 | Lender-controlled |
| finance_program_id | UUID | Approved lender finance program. | Yes | From approved offer | Must belong to the selected lender and remain valid | 666e7777-e88b-99d0-a111-426614174000 | Lender-controlled |
| contract_template_id | UUID | Approved legal template used to generate the contract. | Yes | Policy-selected | Must be active for the contract type, jurisdiction, lender, and language | 777e6666-e55b-44d3-a222-426614174000 | System-controlled |
| contract_number | String | Human-readable unique contract reference. | Yes | System-generated | Must be unique within the dealership or lender scope | FC-2026-000087 | System-generated |
| contract_type | Enum | Type of financial agreement represented. | Yes | VEHICLE_FINANCE | Must match FinancialContractType ENUM | VEHICLE_FINANCE | At least 0.99 |
| status | Enum | Current lifecycle state of the contract. | Yes | DRAFT | Must match FinancialContractStatus ENUM | READY_FOR_SIGNATURE | At least 0.99 |
| execution_method | Enum | Method used to execute the contract. | Yes | ELECTRONIC | Must match ContractExecutionMethod ENUM | ELECTRONIC | At least 0.99 |
| contract_version | Integer | Sequential legal version of the contract. | Yes | 1 | Must be one or greater and increase sequentially | 2 | System-controlled |
| customer_legal_name | String | Verified legal name of the primary Customer. | Yes | From verified Customer | Must match authoritative identity evidence | Mahmoud Ahmed Nasser | Authoritative evidence |
| party_verification_status | Enum | Overall verification state of contract parties. | Yes | PENDING | Must match PartyVerificationStatus ENUM | VERIFIED | Authoritative workflow |
| currency_code | String | ISO 4217 currency code for all contract amounts. | Yes | Deal currency | Exactly three uppercase characters | EGP | At least 0.99 |
| vehicle_selling_price_amount | Decimal | Final Vehicle selling price in the contract. | Yes | From Quotation | Must match the approved Quotation and Deal | 2250000.00 | Authoritative commercial source |
| total_transaction_amount | Decimal | Total amount of the financed transaction before Customer contributions. | Yes | Calculated | Must match the approved commercial calculation | 2320000.00 | System-computed |
| customer_down_payment_amount | Decimal | Customer cash contribution applied at signing or before funding. | Yes | From approved offer | Must be zero or greater and supported by Payment evidence | 500000.00 | Authoritative payment source |
| trade_in_net_equity_amount | Decimal | Verified Trade-In equity applied to the transaction. | Yes | 0.00 | Must match the approved Trade-In and payoff calculation | 150000.00 | System-controlled |
| amount_due_at_signing | Decimal | Total amount payable before or at contract execution. | Yes | Calculated | Must equal the configured upfront components | 520000.00 | System-computed |
| principal_amount | Decimal | Principal amount contractually advanced or financed. | Yes | From lender approval | Must match authoritative lender-approved terms | 1670000.00 | Lender-controlled |
| financed_amount | Decimal | Total amount financed under the contract. | Yes | From lender approval | Must match the accepted approved finance offer | 1670000.00 | Lender-controlled |
| annual_interest_rate | Decimal | Contractual nominal annual interest rate. | Yes | From lender approval | Must be zero or greater and match lender terms | 18.50 | Lender-controlled |
| annual_percentage_rate | Decimal | Annual percentage rate including applicable finance costs. | Yes | From lender approval | Must be zero or greater and legally calculated | 20.10 | Lender-controlled |
| finance_charge_amount | Decimal | Total finance cost over the contractual term. | Yes | Calculated | Must be derived using the approved formula and schedule | 958000.00 | Authoritative calculation |
| total_repayment_amount | Decimal | Total contractual amount payable by the Customer. | Yes | Calculated | Must reconcile with the Payment Schedule | 2628000.00 | Authoritative calculation |
| term_months | Integer | Length of the financial obligation in months. | Yes | From lender approval | Must match the selected finance program | 60 | Lender-controlled |
| payment_frequency | Enum | Frequency of contractual Customer payments. | Yes | MONTHLY | Must match PaymentFrequency ENUM | MONTHLY | Lender-controlled |
| installment_count | Integer | Number of scheduled installments. | Yes | Calculated | Must be greater than zero and reconcile with the schedule | 60 | System-computed |
| installment_amount | Decimal | Standard recurring installment amount. | Yes | Calculated | Must match the authoritative Payment Schedule | 43800.00 | Authoritative calculation |
| first_payment_date | Date | Due date of the first scheduled payment. | Yes | Lender-defined | Must be after contract effectiveness | 2026-09-15 | Lender-controlled |
| final_payment_date | Date | Due date of the final scheduled payment. | Yes | Calculated | Must be after the first payment date | 2031-08-15 | System-computed |
| balloon_payment_amount | Decimal | Final balloon payment when applicable. | Yes | 0.00 | Must be zero or greater and supported by the finance program | 0.00 | Lender-controlled |
| vin | String | Vehicle Identification Number included in the contract. | Yes | From Vehicle | Must match the canonical Vehicle and use valid VIN rules | WBA12345678901234 | Authoritative Vehicle source |
| lien_registration_required | Boolean | Indicates whether a lender security interest must be registered. | Yes | Policy-defined | Must follow contract type and lender requirements | true | System-controlled |
| disclosure_status | Enum | Completion state of mandatory disclosures. | Yes | NOT_STARTED | Must match ContractDisclosureStatus ENUM | ACKNOWLEDGED | Authoritative evidence |
| disclosure_evidence_hash | String | Cryptographic hash of the disclosure evidence package. | Conditional | Null | Required before contract signing | sha256:2fa3... | System-generated |
| signature_status | Enum | Overall signature-collection state. | Yes | NOT_STARTED | Must match ContractSignatureStatus ENUM | PARTIALLY_SIGNED | Trusted signature provider |
| signature_envelope_id | String | External signature-provider envelope reference. | No | Null | Required for electronic execution | env_01JBC8YQ4F | Trusted provider |
| required_signer_count | Integer | Number of required legal signers. | Yes | Calculated | Must be one or greater | 3 | System-controlled |
| completed_signer_count | Integer | Number of signers who completed valid signatures. | Yes | 0 | Cannot exceed required_signer_count | 2 | Trusted provider |
| fully_signed_at | Timestamp | Time when every required signature was completed. | No | Null | Required when signature status becomes COMPLETED | 2026-08-04T13:30:00Z | Trusted provider |
| signed_document_hash | String | Cryptographic hash of the fully executed contract document. | Conditional | Null | Required before the contract becomes effective | sha256:8d44... | System-generated |
| compliance_status | Enum | Overall compliance-review result. | Yes | PENDING | Must match ContractComplianceStatus ENUM | CLEARED | Authorized compliance workflow |
| lender_approval_confirmation_status | Enum | Validation state of the underlying lender approval. | Yes | PENDING | Approval must remain valid and match contract terms | CONFIRMED | Lender-controlled |
| contract_terms_validation_status | Enum | Validation result comparing contract terms with authoritative sources. | Yes | NOT_STARTED | Must match ContractTermsValidationStatus ENUM | VALIDATED | System and human-controlled |
| funding_eligibility_status | Enum | Indicates whether the signed contract is eligible for funding. | Yes | NOT_ELIGIBLE | Must match FundingEligibilityStatus ENUM | ELIGIBLE | Authorized workflow |
| funding_reconciliation_status | Enum | Reconciliation state of lender funding. | Yes | NOT_STARTED | Must match FundingReconciliationStatus ENUM | RECONCILED | Authoritative funding source |
| document_hash | String | Hash of the generated unsigned contract document. | Conditional | Null | Required after document generation | sha256:f817... | System-generated |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 8 | System-controlled |

## 4. Enumerations

### FinancialContractStatus

- **DRAFT:** Contract data and terms are being prepared.
- **GENERATED:** The contract document was generated from an approved template.
- **REVIEW_PENDING:** Internal legal, finance, compliance, or commercial review is required.
- **APPROVED_FOR_SIGNATURE:** Required reviews passed and the contract may be presented.
- **READY_FOR_SIGNATURE:** Parties, disclosures, and signature requirements are ready.
- **SENT_FOR_SIGNATURE:** The contract was delivered to one or more signers.
- **PARTIALLY_SIGNED:** At least one but not all required parties signed.
- **FULLY_SIGNED:** Every required party completed a valid signature.
- **COUNTERSIGNED:** Required dealership or lender counter-signatures were completed.
- **EFFECTIVE:** The contract is legally or operationally effective according to its terms.
- **FUNDING_PENDING:** The effective contract is awaiting lender funding.
- **ACTIVE:** The contract is funded and active.
- **COMPLETED:** All contractual financial obligations were completed or settled.
- **CANCELLED:** The contract process ended before effectiveness.
- **VOIDED:** The contract was legally invalidated or voided.
- **EXPIRED:** The unsigned or unactivated contract expired.
- **TERMINATED:** An effective or active contract ended before scheduled completion.
- **SUPERSEDED:** A newer valid contract version replaced this contract.
- **ARCHIVED:** The contract moved to historical retention.

### FinancialContractType

- VEHICLE_FINANCE
- VEHICLE_LOAN
- HIRE_PURCHASE
- FINANCE_LEASE
- OPERATING_LEASE
- BALLOON_FINANCE
- ISLAMIC_FINANCE
- CORPORATE_FINANCE
- FLEET_FINANCE
- REFINANCE
- GUARANTEE_AGREEMENT
- SECURITY_AGREEMENT
- OTHER

### ContractExecutionMethod

- ELECTRONIC
- PHYSICAL
- HYBRID
- REMOTE_NOTARIZED
- IN_PERSON_NOTARIZED
- OTHER

### PartyVerificationStatus

- NOT_STARTED
- PENDING
- PARTIALLY_VERIFIED
- VERIFIED
- FAILED
- EXPIRED
- MANUAL_REVIEW_REQUIRED

### ContractDisclosureStatus

- NOT_STARTED
- PREPARING
- PRESENTED
- PARTIALLY_ACKNOWLEDGED
- ACKNOWLEDGED
- REJECTED
- EXPIRED
- NOT_REQUIRED

### ContractSignatureStatus

- NOT_STARTED
- PREPARING
- REQUESTED
- VIEWED
- PARTIALLY_SIGNED
- COMPLETED
- DECLINED
- FAILED
- EXPIRED
- CANCELLED
- INVALIDATED

### SignerStatus

- NOT_REQUIRED
- NOT_REQUESTED
- REQUESTED
- VIEWED
- SIGNED
- DECLINED
- FAILED
- EXPIRED
- INVALIDATED

### ContractDocumentGenerationStatus

- NOT_STARTED
- QUEUED
- GENERATING
- GENERATED
- FAILED
- INVALIDATED
- SUPERSEDED

### ContractComplianceStatus

- NOT_STARTED
- PENDING
- CLEARED
- CLEARED_WITH_CONDITIONS
- BLOCKED
- FAILED
- MANUAL_REVIEW_REQUIRED
- EXPIRED

### ContractTermsValidationStatus

- NOT_STARTED
- PENDING
- VALIDATED
- MISMATCH_DETECTED
- FAILED
- MANUAL_REVIEW_REQUIRED

### FundingEligibilityStatus

- NOT_ASSESSED
- NOT_ELIGIBLE
- PENDING
- ELIGIBLE
- CONDITIONALLY_ELIGIBLE
- BLOCKED
- EXPIRED

### ContractFundingRequestStatus

- NOT_STARTED
- READY
- REQUESTED
- ACCEPTED
- PENDING
- COMPLETED
- FAILED
- CANCELLED
- REVERSED

### FundingReconciliationStatus

- NOT_STARTED
- PENDING
- PARTIALLY_RECONCILED
- RECONCILED
- MISMATCH
- FAILED
- REVERSED

### ContractActivationStatus

- NOT_STARTED
- PENDING
- ACTIVATED
- BLOCKED
- FAILED
- REVERSED

### PaymentFrequency

- WEEKLY
- BIWEEKLY
- MONTHLY
- QUARTERLY
- SEMIANNUAL
- ANNUAL
- CUSTOM

### LienRegistrationStatus

- NOT_REQUIRED
- NOT_STARTED
- PENDING
- REGISTERED
- FAILED
- RELEASED
- MANUAL_REVIEW_REQUIRED

### ContractSettlementStatus

- NOT_STARTED
- PENDING
- PARTIALLY_SETTLED
- SETTLED
- FAILED
- WAIVED
- DISPUTED

### FinancialContractCancellationReason

- CUSTOMER_REQUEST
- CUSTOMER_DECLINED_TO_SIGN
- LENDER_APPROVAL_WITHDRAWN
- LENDER_APPROVAL_EXPIRED
- FINANCE_APPLICATION_CANCELLED
- DEAL_CANCELLED
- VEHICLE_UNAVAILABLE
- COMMERCIAL_TERMS_CHANGED
- COMPLIANCE_BLOCK
- IDENTITY_VERIFICATION_FAILED
- SIGNATURE_EXPIRED
- DUPLICATE_CONTRACT
- DOCUMENT_ERROR
- OTHER

### FinancialContractVoidReason

- INVALID_SIGNATURE
- MATERIAL_TERM_ERROR
- INCORRECT_PARTY
- INCORRECT_VEHICLE
- INCORRECT_LENDER
- FRAUD_CONFIRMED
- LEGAL_INVALIDITY
- REGULATORY_REQUIREMENT
- DUPLICATE_EXECUTION
- OTHER

### FinancialContractTerminationReason

- EARLY_SETTLEMENT
- DEFAULT
- MUTUAL_AGREEMENT
- VEHICLE_TOTAL_LOSS
- CONTRACT_BREACH
- REFINANCE
- REPOSSESSION
- COURT_ORDER
- REGULATORY_ACTION
- OTHER

## 5. Validation Rules

### Business Rules

- A Financial Contract must reference one active Deal and one approved Finance Application.
- The Customer, Vehicle, Quotation, Deal, Finance Application, lender, and finance program must represent the same transaction.
- Contract terms must match one exact accepted and unexpired lender offer.
- The contract must use an approved legal template applicable to:
  - Contract type.
  - Lender.
  - Jurisdiction.
  - Language.
  - Customer type.
  - Finance program.
  - Vehicle type.

- All mandatory contract parties must be identified and verified.
- A co-applicant or guarantor included in the lender approval must also be included as a required contract party.
- Customer-declared terms cannot replace authoritative Quotation, Deal, lender, or Payment Schedule values.
- Mandatory disclosures must be presented and acknowledged before signing when required.
- The Customer must receive sufficient opportunity to review the contract and disclosures.
- Every required signature must be attributable to the correct verified signer.
- An electronic signature must preserve provider identity, signer authentication, timestamp, document hash, and certificate evidence.
- A contract cannot become effective if:
  - Required signatures are missing.
  - Mandatory disclosures are incomplete.
  - Lender approval expired.
  - Compliance is blocked.
  - Material terms do not match authoritative sources.
  - The signed document hash is missing or invalid.

- Contract effectiveness does not prove lender funding.
- A contract cannot become `ACTIVE` until authoritative lender funding is received and reconciled.
- Material changes after any party signs require a new contract version and renewed signatures.
- Signed contract documents must never be edited.
- Corrections must create a new version, amendment, or legally approved void-and-reissue workflow.
- Cancellation, voiding, termination, and settlement require authorized reasons and evidence.
- AI-generated summaries are non-binding and cannot replace the full legal contract.
- AI Agents cannot provide legal advice, sign, consent, countersign, or determine enforceability.

### Technical Rules

- Contract creation, generation, signature request, activation, funding request, amendment, cancellation, and voiding must require idempotency keys.
- Every contract version must preserve immutable snapshots of:
  - Parties.
  - Commercial terms.
  - Finance terms.
  - Vehicle.
  - Disclosures.
  - Compliance results.
  - Generated document.
  - Signature evidence.
  - Funding context.

- `record_version` must increase after every permitted update.
- `contract_version` must increase after every material legal revision.
- Generated documents must use approved deterministic templates.
- Generated and signed documents must have cryptographic hashes.
- Signature-provider events must be:
  - Authenticated.
  - Idempotent.
  - Deduplicated.
  - Ordered.
  - Replay-safe.
  - Traceable to one contract version.

- Contract terms must be validated server-side against the authoritative Quotation, Deal, Finance Application, and lender decision.
- Payment Schedule calculations must use fixed decimal precision and an approved formula version.
- Signed-document storage must use immutable encrypted object storage.
- Public document URLs are prohibited.
- Funding events must come from an authoritative lender or approved banking integration.
- Failed multi-service operations must create compensating actions and Human Review Tasks.
- Every lifecycle transition must create immutable status-history and audit records.

### Data Constraints

- `contract_version` must be one or greater.
- Financial amounts cannot be negative.
- `customer_down_payment_amount` cannot exceed `total_transaction_amount`.
- `trade_in_net_equity_amount` must match the approved Trade-In calculation.
- `financed_amount` must reconcile with:
  - Total transaction amount.
  - Customer down payment.
  - Deposit.
  - Trade-In equity.
  - Approved fees and financed products.

- `annual_interest_rate` cannot be negative.
- `annual_percentage_rate` cannot be negative.
- `term_months` must be greater than zero.
- `installment_count` must be greater than zero.
- `installment_amount` cannot be negative.
- `first_payment_date` must follow the applicable contract-effective date.
- `final_payment_date` must be later than `first_payment_date`.
- `balloon_payment_amount` cannot be negative.
- `residual_value_amount` cannot be negative.
- `completed_signer_count` cannot exceed `required_signer_count`.
- `required_signer_count` must be one or greater.
- `fully_signed_at` is required when signature status becomes `COMPLETED`.
- `signed_document_hash` is required before status becomes `EFFECTIVE`.
- `disclosure_evidence_hash` is required before signing when disclosures are mandatory.
- `funding_reference` and `funded_amount` are required before status becomes `ACTIVE`.
- `funded_amount` must reconcile with the authoritative approved amount.
- `termination_reason` is required when status becomes `TERMINATED`.
- `void_reason` is required when status becomes `VOIDED`.
- `cancellation_reason` is required when status becomes `CANCELLED`.
- `supersedes_contract_id` is required when status becomes `SUPERSEDED`.

### Referential Integrity

- All linked records must belong to the same `dealership_id`.
- `customer_id` must match the Customer in the Deal, Quotation, and Finance Application.
- `vehicle_id` must match the Vehicle in the Deal, Quotation, and Finance Application.
- `quotation_id` must be the approved commercial source for the Deal.
- `deal_id` must remain active and eligible for finance contracting.
- `finance_application_id` must contain the selected approved lender offer.
- `lender_id` and `finance_program_id` must match the accepted lender decision.
- `trade_in_id` must belong to the same Customer and Deal when populated.
- `payment_schedule_id` must reference this exact contract version.
- `funding_transaction_id` must reference this Financial Contract and Deal.
- `document_id` and `signed_document_id` must belong to this contract version.
- `supersedes_contract_id` must reference an earlier contract version in the same contract series.
- Circular contract-version and supersession relationships are prohibited.
- A signed or effective Financial Contract cannot be hard-deleted.

### Human Approval Requirements

- Legal-template changes require authorized legal or compliance approval.
- Material differences between the contract and authoritative commercial or lender terms require Human Review.
- Manual changes to finance rates, APR, term, installment amount, balloon value, fees, or repayment totals require lender-authorized evidence.
- Identity, signature, disclosure, compliance, or funding conflicts require Human Review.
- High fraud-risk or sanctions findings require compliance approval.
- A contract cannot be sent for signature while mandatory legal review remains incomplete.
- A contract cannot be voided, terminated, or materially amended solely by an AI Agent.
- AI Agents cannot:
  - Create Customer consent.
  - Apply a legal signature.
  - Act as a witness.
  - Countersign.
  - Alter authoritative terms.
  - Determine legal enforceability.
  - Approve a lender obligation.
  - Mark funding received.
  - Waive required disclosures.
  - Waive legal or compliance controls.

- Every manual override must preserve the original value, new value, actor, authority, reason, and supporting evidence.

## 6. State Machine

### Allowed States

- DRAFT
- GENERATED
- REVIEW_PENDING
- APPROVED_FOR_SIGNATURE
- READY_FOR_SIGNATURE
- SENT_FOR_SIGNATURE
- PARTIALLY_SIGNED
- FULLY_SIGNED
- COUNTERSIGNED
- EFFECTIVE
- FUNDING_PENDING
- ACTIVE
- COMPLETED
- CANCELLED
- VOIDED
- EXPIRED
- TERMINATED
- SUPERSEDED
- ARCHIVED

### Allowed Transitions

- DRAFT → GENERATED
- DRAFT → CANCELLED
- GENERATED → REVIEW_PENDING
- GENERATED → APPROVED_FOR_SIGNATURE
- GENERATED → CANCELLED
- REVIEW_PENDING → APPROVED_FOR_SIGNATURE
- REVIEW_PENDING → DRAFT
- REVIEW_PENDING → CANCELLED
- APPROVED_FOR_SIGNATURE → READY_FOR_SIGNATURE
- APPROVED_FOR_SIGNATURE → DRAFT
- APPROVED_FOR_SIGNATURE → CANCELLED
- READY_FOR_SIGNATURE → SENT_FOR_SIGNATURE
- READY_FOR_SIGNATURE → DRAFT
- READY_FOR_SIGNATURE → CANCELLED
- SENT_FOR_SIGNATURE → PARTIALLY_SIGNED
- SENT_FOR_SIGNATURE → FULLY_SIGNED
- SENT_FOR_SIGNATURE → EXPIRED
- SENT_FOR_SIGNATURE → CANCELLED
- PARTIALLY_SIGNED → FULLY_SIGNED
- PARTIALLY_SIGNED → EXPIRED
- PARTIALLY_SIGNED → CANCELLED
- FULLY_SIGNED → COUNTERSIGNED
- FULLY_SIGNED → EFFECTIVE
- FULLY_SIGNED → VOIDED
- COUNTERSIGNED → EFFECTIVE
- COUNTERSIGNED → VOIDED
- EFFECTIVE → FUNDING_PENDING
- EFFECTIVE → ACTIVE
- EFFECTIVE → TERMINATED
- EFFECTIVE → VOIDED
- FUNDING_PENDING → ACTIVE
- FUNDING_PENDING → TERMINATED
- FUNDING_PENDING → VOIDED
- ACTIVE → COMPLETED
- ACTIVE → TERMINATED
- ACTIVE → SUPERSEDED
- COMPLETED → ARCHIVED
- CANCELLED → ARCHIVED
- VOIDED → ARCHIVED
- EXPIRED → ARCHIVED
- TERMINATED → ARCHIVED
- SUPERSEDED → ARCHIVED

### Forbidden Transitions

- DRAFT → SENT_FOR_SIGNATURE
- DRAFT → FULLY_SIGNED
- GENERATED → EFFECTIVE
- REVIEW_PENDING → SENT_FOR_SIGNATURE
- READY_FOR_SIGNATURE → FULLY_SIGNED
- SENT_FOR_SIGNATURE → EFFECTIVE
- PARTIALLY_SIGNED → EFFECTIVE
- FULLY_SIGNED → ACTIVE
- COUNTERSIGNED → ACTIVE
- EFFECTIVE → COMPLETED
- FUNDING_PENDING → COMPLETED
- CANCELLED → EFFECTIVE
- CANCELLED → ACTIVE
- VOIDED → EFFECTIVE
- VOIDED → ACTIVE
- EXPIRED → SENT_FOR_SIGNATURE
- EXPIRED → EFFECTIVE
- TERMINATED → ACTIVE
- COMPLETED → ACTIVE
- SUPERSEDED → ACTIVE
- ARCHIVED → ACTIVE
- ARCHIVED → EFFECTIVE

### Entry Conditions

- To enter `GENERATED`:
  - The approved contract template must be selected.
  - Party, Vehicle, commercial, and finance data must be complete.
  - The generated document and `document_hash` must exist.
  - The template and formula versions must be recorded.

- To enter `REVIEW_PENDING`:
  - A legal, compliance, finance, commercial, or term-validation review must be required.
  - Review reasons and responsible roles must be recorded.

- To enter `APPROVED_FOR_SIGNATURE`:
  - Contract terms must match the authoritative Quotation, Deal, Finance Application, and lender decision.
  - Required legal and compliance reviews must pass.
  - The lender approval must remain valid.
  - No unresolved material mismatch may remain.

- To enter `READY_FOR_SIGNATURE`:
  - Every required signer must be identified and verified.
  - Mandatory disclosures must be presented or prepared according to policy.
  - The signature package must be complete.
  - The contract must remain within its permitted execution period.

- To enter `SENT_FOR_SIGNATURE`:
  - The signature envelope or physical execution package must be created successfully.
  - Required signers and authentication methods must be configured.
  - The exact document hash must be preserved.
  - `signature_request_sent_at` must be populated.

- To enter `PARTIALLY_SIGNED`:
  - At least one required signer must complete a valid signature.
  - The signature event must be authenticated.
  - The signed document version must match the presented document.

- To enter `FULLY_SIGNED`:
  - Every required party must complete a valid signature.
  - `completed_signer_count` must equal `required_signer_count`.
  - A fully executed document and signature certificate must exist.
  - `signed_document_hash` and `fully_signed_at` must be populated.

- To enter `COUNTERSIGNED`:
  - Required dealership or lender counter-signatures must be complete.
  - Counter-signatory authority must be verified.

- To enter `EFFECTIVE`:
  - All required signatures and counter-signatures must be complete.
  - Mandatory disclosures must be acknowledged.
  - Compliance must be cleared.
  - Contract terms validation must be `VALIDATED`.
  - The lender approval must remain valid.
  - The signed document hash must pass integrity validation.
  - Any applicable cooling-off or legal effectiveness condition must be satisfied.

- To enter `FUNDING_PENDING`:
  - The contract must be effective.
  - Funding eligibility must be `ELIGIBLE` or properly `CONDITIONALLY_ELIGIBLE`.
  - All mandatory lender-funding conditions must be complete.
  - A funding request must be accepted.
  - `funding_requested_at` must be populated.

- To enter `ACTIVE`:
  - The contract must be effective.
  - Authoritative lender funds must be received.
  - Funding must reconcile with the Deal and approved finance amount.
  - `funding_reference`, `funded_amount`, and `funding_received_at` must be populated.
  - Activation must pass all applicable controls.

- To enter `COMPLETED`:
  - All contractual obligations must be satisfied, settled, or legally discharged.
  - Settlement or completion evidence must exist.
  - No unresolved balance or mandatory action may remain.
  - `completed_at` must be populated.

- To enter `CANCELLED`:
  - The contract must not yet be effective.
  - An authorized cancellation reason and actor must be recorded.
  - Pending signature requests must be cancelled.

- To enter `VOIDED`:
  - An authorized legal or compliance decision must determine that the executed contract is invalid or must be voided.
  - The void reason, authority, and supporting evidence must be recorded.
  - Related funding and Deal workflows must be blocked or reversed as required.

- To enter `EXPIRED`:
  - The unsigned, partially signed, or unactivated contract must pass its execution or validity deadline.
  - The expired component and timestamp must be recorded.

- To enter `TERMINATED`:
  - The contract must already be effective or active.
  - An authorized termination reason and effective date must exist.
  - Outstanding settlement, Vehicle, lien, payment, and customer obligations must be evaluated.

- To enter `SUPERSEDED`:
  - A replacement Financial Contract version must exist.
  - The replacement contract must reference this contract.
  - The prior contract must no longer be the current version.
  - Legal treatment of previously collected signatures must be documented.

- To enter `ARCHIVED`:
  - The contract must be completed, cancelled, voided, expired, terminated, or superseded.
  - Retention, legal-hold, dependency, and audit requirements must pass.
  - `archived_at` must be populated.

### Exit Conditions

- A contract cannot exit `DRAFT` without complete authoritative transaction and party data.
- A contract cannot exit `GENERATED` toward signature readiness while required review remains incomplete.
- A contract cannot exit `REVIEW_PENDING` without an approval, revision, cancellation, or rejection decision.
- A contract cannot exit `READY_FOR_SIGNATURE` without a valid execution package.
- A contract cannot exit `SENT_FOR_SIGNATURE` toward completion without trusted signature-provider or physical-signature evidence.
- A contract cannot exit `PARTIALLY_SIGNED` toward `FULLY_SIGNED` until every required signer completes execution.
- A contract cannot exit `FULLY_SIGNED` toward effectiveness while mandatory counter-signatures, disclosures, or compliance checks remain incomplete.
- A contract cannot exit `EFFECTIVE` toward `ACTIVE` without authoritative reconciled funding.
- A contract cannot exit `ACTIVE` toward `COMPLETED` without settlement or obligation-completion evidence.
- A cancelled, voided, expired, terminated, superseded, or archived contract cannot return to an active lifecycle state.
- A fully signed or effective contract cannot be edited directly; changes require a controlled amendment, replacement, void-and-reissue, or termination workflow.

### Terminal States

- **COMPLETED:** Contractual obligations were completed or settled.
- **CANCELLED:** The contract process ended before effectiveness.
- **VOIDED:** The contract was invalidated through an authorized legal workflow.
- **EXPIRED:** The contract expired before valid effectiveness or activation.
- **TERMINATED:** An effective or active contract ended before scheduled completion.
- **SUPERSEDED:** A newer valid Financial Contract replaced this version.
- **ARCHIVED:** The contract moved to historical retention.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Verified Customer and all required contract parties.
  - Active Opportunity and Deal.
  - Approved Quotation.
  - Approved and Customer-accepted Finance Application offer.
  - Eligible Vehicle.
  - Authorized lender and finance program.
  - Approved contract template and applicable jurisdiction rules.
  - Valid compliance, identity, disclosure, signature, and funding controls.

- **Consumes:**
  - Customer, co-applicant, guarantor, dealership, and lender legal information.
  - Approved Quotation and Deal commercial terms.
  - Finance Application approval, rate, term, conditions, and validity.
  - Vehicle identity and security-interest information.
  - Trade-In allowance, payoff, and equity values.
  - Payment, deposit, and amount-due-at-signing evidence.
  - Legal templates and disclosure packages.
  - Identity, sanctions, fraud, affordability, and compliance results.
  - Signature-provider events and certificates.
  - Lender-funding and banking-reconciliation events.
  - Payment Schedule calculations.

- **Produces:**
  - Canonical signed financial-agreement record.
  - Immutable contract-version snapshots.
  - Legally governed contract document.
  - Disclosure and acknowledgement evidence.
  - Signature evidence and execution certificate.
  - Funding-eligibility context.
  - Payment Schedule instructions.
  - Deal activation and funding context.
  - Lien-registration requirements.
  - Contract completion, settlement, cancellation, voiding, or termination evidence.

- **Creates:**
  - Contract document.
  - Disclosure package.
  - Signature envelope.
  - Signer-verification Tasks.
  - Legal or compliance-review Tasks.
  - Payment Schedule.
  - Funding request.
  - Lien-registration request.
  - Customer contract-copy delivery record.
  - Amendment or replacement contract version.
  - Human Review Case when conflicts are detected.

- **Triggers:**
  - Contract-term validation.
  - Legal-template selection.
  - Disclosure generation and presentation.
  - Signature collection.
  - Counter-signature workflow.
  - Contract-effectiveness checks.
  - Lender-funding workflow.
  - Deal activation.
  - Payment-schedule generation.
  - Lien registration.
  - Contract-expiry monitoring.
  - Settlement, completion, amendment, termination, or archival workflows.

- **Owned By:**
  - The authorized Finance User identified by `assigned_finance_user_id`.
  - Legal-template governance remains owned by authorized Legal or Compliance roles.
  - Lender-controlled terms remain owned by the applicable lender.
  - Customer signatures remain attributable only to the verified Customer or applicable party.

- **Referenced By:**
  - Customer
  - Co-Applicant
  - Guarantor
  - Opportunity
  - Quotation
  - Deal
  - Vehicle
  - Trade-In
  - Finance Application
  - Lender
  - Finance Program
  - Payment Schedule
  - Funding Transaction
  - Payment
  - Vehicle Delivery
  - Lien Registration
  - Document Vault
  - Compliance Case
  - Customer Journey
  - Interaction
  - AI Agent Run
  - Audit Record

- **Supports but Does Not Replace:**
  - Cleared Customer Payment evidence.
  - Authoritative lender-funding evidence.
  - Vehicle-delivery confirmation.
  - Ownership-transfer or registration evidence.
  - Lien registration or release.
  - Payment servicing and settlement records.

- **Supersedes / Replaced By:**
  - Material legal, commercial, finance, party, Vehicle, or lender changes require a new contract version.
  - The replacement references the earlier contract through `supersedes_contract_id`.
  - Previously signed versions remain immutable.
  - The legal treatment of earlier signatures, disclosures, payments, and funding must be documented.

## 8. Domain Events

### Emitted Events

- **FinancialContractCreated**  
  Payload: `financial_contract_id`, `contract_number`, `deal_id`, `finance_application_id`, `created_at`

- **FinancialContractGenerated**  
  Payload: `financial_contract_id`, `contract_template_id`, `template_version`, `document_id`, `document_hash`, `generated_at`

- **FinancialContractGenerationFailed**  
  Payload: `financial_contract_id`, `error_code`, `failure_reason`, `failed_at`

- **FinancialContractReviewRequested**  
  Payload: `financial_contract_id`, `review_types`, `assigned_reviewer_ids`, `requested_at`

- **FinancialContractTermsMismatchDetected**  
  Payload: `financial_contract_id`, `mismatched_fields`, `authoritative_sources`, `detected_at`

- **FinancialContractApprovedForSignature**  
  Payload: `financial_contract_id`, `approved_by`, `approved_at`, `contract_version`

- **FinancialContractReadyForSignature**  
  Payload: `financial_contract_id`, `required_signer_count`, `disclosure_status`, `ready_at`

- **FinancialContractSentForSignature**  
  Payload: `financial_contract_id`, `signature_provider`, `signature_envelope_id`, `signature_request_sent_at`

- **FinancialContractSignerViewed**  
  Payload: `financial_contract_id`, `signer_id`, `signer_role`, `viewed_at`

- **FinancialContractPartiallySigned**  
  Payload: `financial_contract_id`, `completed_signer_count`, `required_signer_count`, `signed_at`

- **FinancialContractFullySigned**  
  Payload: `financial_contract_id`, `signed_document_id`, `signed_document_hash`, `fully_signed_at`

- **FinancialContractCountersigned**  
  Payload: `financial_contract_id`, `countersigned_by`, `countersigned_at`

- **FinancialContractSignatureDeclined**  
  Payload: `financial_contract_id`, `signer_id`, `signer_role`, `decline_reason`, `declined_at`

- **FinancialContractSignatureExpired**  
  Payload: `financial_contract_id`, `signature_envelope_id`, `expired_at`

- **FinancialContractEffective**  
  Payload: `financial_contract_id`, `deal_id`, `effective_at`, `governing_law_code`

- **FinancialContractFundingEligible**  
  Payload: `financial_contract_id`, `funding_eligibility_status`, `eligible_at`

- **FinancialContractFundingRequested**  
  Payload: `financial_contract_id`, `deal_id`, `lender_id`, `funded_amount`, `funding_requested_at`

- **FinancialContractFundingReceived**  
  Payload: `financial_contract_id`, `funding_transaction_id`, `funding_reference`, `funded_amount`, `funding_received_at`

- **FinancialContractActivated**  
  Payload: `financial_contract_id`, `deal_id`, `payment_schedule_id`, `activated_at`

- **FinancialContractPaymentScheduleCreated**  
  Payload: `financial_contract_id`, `payment_schedule_id`, `installment_count`, `total_repayment_amount`

- **FinancialContractAmendmentRequested**  
  Payload: `financial_contract_id`, `amendment_reason`, `requested_by`, `requested_at`

- **FinancialContractSuperseded**  
  Payload: `financial_contract_id`, `replacement_contract_id`, `superseded_at`

- **FinancialContractCancelled**  
  Payload: `financial_contract_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

- **FinancialContractVoided**  
  Payload: `financial_contract_id`, `void_reason`, `voided_by`, `voided_at`

- **FinancialContractExpired**  
  Payload: `financial_contract_id`, `expired_component`, `expired_at`

- **FinancialContractTerminated**  
  Payload: `financial_contract_id`, `termination_reason`, `termination_effective_at`, `terminated_at`

- **FinancialContractCompleted**  
  Payload: `financial_contract_id`, `settlement_status`, `settlement_amount`, `completed_at`

- **FinancialContractArchived**  
  Payload: `financial_contract_id`, `archived_by`, `archived_at`

- **FinancialContractHumanReviewRequired**  
  Payload: `financial_contract_id`, `human_review_reason`, `priority`, `created_at`

### Consumed Events

- **FinanceOfferSelected**  
  Supplies the exact approved lender offer used to create the contract.

- **FinanceApplicationApproved**  
  Confirms lender-authorized finance terms and approval validity.

- **FinanceApplicationExpired**  
  Blocks contract generation, signing, or effectiveness when the underlying approval is invalid.

- **QuotationApproved**  
  Supplies authoritative commercial terms.

- **QuotationSuperseded**  
  Requires contract revalidation or a new contract version.

- **DealCreated**  
  Creates the Deal context required for contracting.

- **DealCommercialTermsChanged**  
  Requires contract-term revalidation and may require void-and-reissue.

- **DealCancelled**  
  Cancels an unsigned contract or starts an authorized termination workflow for an effective contract.

- **VehicleStatusChanged**  
  Revalidates Vehicle eligibility and availability.

- **VehicleIdentityUpdated**  
  Requires revalidation when VIN or registration information changes.

- **TradeInPayoffVerified**  
  Supplies authoritative Trade-In payoff and equity values.

- **CustomerIdentityVerified**  
  Updates party-verification status.

- **ComplianceCaseCleared**  
  Removes an eligible contract compliance block.

- **ComplianceCaseBlocked**  
  Prevents signing, effectiveness, funding, or activation.

- **DisclosureAcknowledged**  
  Updates disclosure completion and evidence.

- **SignatureProviderEventReceived**  
  Updates signer, signature, decline, failure, expiry, or completion status.

- **PaymentReceived**  
  Validates the amount due at signing when applicable.

- **FundingTransactionCompleted**  
  Updates funding amount, reference, and reconciliation state.

- **FundingTransactionReversed**  
  Blocks activation or creates a funding-reversal exception workflow.

- **LienRegistered**  
  Updates security-interest registration status.

- **LienReleased**  
  Updates lien status after settlement or termination.

- **ContractSettlementCompleted**  
  Permits contract completion.

- **LegalHoldApplied**  
  Prevents deletion, alteration, or archival actions that conflict with the legal hold.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- Non-sensitive contract summary.
- Contract-type description.
- Permitted Customer questions about contract sections.
- Disclosure explanations.
- Payment-schedule explanations.
- Non-binding early-settlement explanations.
- Contract-status summaries.
- Missing-document summaries.
- Signature-workflow summaries.
- Funding-condition summaries.
- Amendment or termination summaries.
- Non-sensitive internal operational notes.

### Fields Excluded from Embeddings

- `financial_contract_id`
- `customer_id`
- `co_applicant_customer_id`
- `guarantor_customer_id`
- `deal_id`
- `finance_application_id`
- `lender_id`
- `signature_envelope_id`
- `signature_certificate_reference`
- `external_contract_reference`
- `lender_contract_reference`
- Legal names
- Dates of birth
- National identifiers
- Contact information
- Residential addresses
- Bank information
- Payment references
- Credit information
- Identity documents
- Signature images
- Signature certificates
- Signed contract documents
- Disclosure evidence
- Funding references
- Authentication information
- Secure document links
- `party_snapshot`
- `commercial_terms_snapshot`
- `finance_terms_snapshot`
- `disclosure_snapshot`
- `signature_snapshot`
- `compliance_snapshot`
- `funding_snapshot`
- `payment_schedule_snapshot`

> Signed legal documents, identity information, signature evidence, financial data, and lender data must not be placed in unrestricted semantic indexes.

### Structured AI Context Fields

Authorized Contract AI Agents may receive:

- `contract_type`
- `status`
- `execution_method`
- `contract_version`
- `currency_code`
- `term_months`
- `payment_frequency`
- `installment_count`
- `installment_amount`
- `first_payment_date`
- `final_payment_date`
- `balloon_payment_amount`
- `disclosure_status`
- `signature_status`
- `required_signer_count`
- `completed_signer_count`
- `party_verification_status`
- `compliance_status`
- `contract_terms_validation_status`
- `funding_eligibility_status`
- `funding_request_status`
- `funding_reconciliation_status`
- `activation_status`
- `lien_registration_status`
- `settlement_status`
- `missing_disclosure_types`
- Permitted operational workflow context

Restricted Finance, Legal, or Compliance Agents may additionally receive:

- Approved commercial totals.
- Authoritative finance terms.
- Verified party roles.
- Document-completeness results.
- Compliance and signature-validation results.
- Funding-reconciliation results.
- Amendment and termination context.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `financial_contract_id`
- `customer_id`
- `deal_id`
- `finance_application_id`
- `vehicle_id`
- `lender_id`
- `contract_type`
- `status`
- `contract_version`
- `is_current_version`
- `visibility`
- `compliance_status`
- `signature_status`
- `funding_eligibility_status`

### Confidence Thresholds

- Contract-party extraction requires confidence of at least `0.99`.
- Legal-name extraction requires confidence of at least `0.99`.
- Finance-term extraction requires confidence of at least `0.995`.
- Interest-rate and APR extraction requires confidence of at least `0.995`.
- Payment-date extraction requires confidence of at least `0.99`.
- Vehicle VIN extraction requires confidence of at least `0.995`.
- Disclosure classification requires confidence of at least `0.99`.
- Signature-status interpretation requires trusted provider evidence and cannot rely only on AI confidence.
- Contract-term mismatch detection requires confidence of at least `0.99`.
- Customer acceptance, signature, consent, and legal acknowledgement require authoritative evidence.
- AI summaries must never replace the full signed Financial Contract.
- No AI confidence score may determine legal enforceability.

### Human Approval Thresholds

- AI Agents cannot sign, witness, countersign, acknowledge disclosures, or provide consent.
- AI Agents cannot alter legal, commercial, finance, lender, Vehicle, or party terms.
- AI Agents cannot determine that a contract is legally enforceable.
- AI Agents cannot waive disclosures, identity checks, signatures, compliance controls, lender conditions, or cooling-off requirements.
- AI Agents cannot mark a contract `EFFECTIVE`, `ACTIVE`, `COMPLETED`, `VOIDED`, or `TERMINATED`.
- AI Agents cannot provide legal advice.
- AI Agents cannot create a binding amendment.
- Conflicting party, Vehicle, Quotation, Deal, lender, finance, signature, disclosure, Payment, or funding evidence must create a Human Review Task.
- Legal, fraud, sanctions, signature-integrity, document-integrity, or jurisdiction conflicts require specialist review.

### AI Summary Rules

- Contract summaries must be clearly labeled as non-binding.
- Summaries must link to the exact contract version.
- Every summarized amount, rate, date, and obligation must be traceable to a contract field.
- AI must not omit material fees, balloon payments, guarantees, security interests, early-settlement terms, default terms, or mandatory disclosures.
- AI must distinguish:
  - Contractual obligations.
  - Operational status.
  - Lender conditions.
  - Non-binding explanations.
- Customer-facing AI explanations must encourage review of the complete contract and applicable professional advice where appropriate.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/financial-contracts`

### Methods

- `GET` — list or search Financial Contracts.
- `POST` — create a Draft Financial Contract from an accepted finance offer.
- `GET /{id}` — retrieve one permitted contract version.
- `PATCH /{id}` — update permitted Draft fields before document generation or signing.
- `POST /{id}/validate-terms` — compare contract data with authoritative sources.
- `POST /{id}/generate` — generate the contract document.
- `POST /{id}/request-review` — create required legal, finance, or compliance review.
- `POST /{id}/approve-for-signature` — approve an eligible contract for execution.
- `POST /{id}/prepare-signature` — validate signers, disclosures, and execution configuration.
- `POST /{id}/send-for-signature` — send the exact contract version to required signers.
- `POST /{id}/record-signature-event` — process authenticated provider or physical-signature evidence.
- `POST /{id}/countersign` — record an authorized dealership or lender counter-signature.
- `POST /{id}/make-effective` — make an eligible fully executed contract effective.
- `POST /{id}/request-funding` — submit an eligible contract for lender funding.
- `POST /{id}/reconcile-funding` — reconcile authoritative funding evidence.
- `POST /{id}/activate` — activate an effective and funded contract.
- `POST /{id}/generate-payment-schedule` — create the authoritative Payment Schedule.
- `POST /{id}/create-amendment` — create a controlled amendment or replacement version.
- `POST /{id}/cancel` — cancel an eligible pre-effective contract.
- `POST /{id}/void` — perform an authorized voiding workflow.
- `POST /{id}/terminate` — terminate an eligible effective or active contract.
- `POST /{id}/complete` — complete a settled contract.
- `POST /{id}/archive` — archive an eligible terminal contract.
- `GET /{id}/document` — retrieve a time-limited authorized contract-document reference.
- `GET /{id}/audit` — retrieve permitted immutable contract audit history.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateFinancialContractRequest",
  "type": "object",
  "properties": {
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "co_applicant_customer_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "guarantor_customer_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "opportunity_id": {
      "type": "string",
      "format": "uuid"
    },
    "quotation_id": {
      "type": "string",
      "format": "uuid"
    },
    "deal_id": {
      "type": "string",
      "format": "uuid"
    },
    "vehicle_id": {
      "type": "string",
      "format": "uuid"
    },
    "trade_in_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "finance_application_id": {
      "type": "string",
      "format": "uuid"
    },
    "lender_id": {
      "type": "string",
      "format": "uuid"
    },
    "finance_program_id": {
      "type": "string",
      "format": "uuid"
    },
    "contract_template_id": {
      "type": "string",
      "format": "uuid"
    },
    "assigned_finance_user_id": {
      "type": "string",
      "format": "uuid"
    },
    "contract_type": {
      "type": "string",
      "enum": [
        "VEHICLE_FINANCE",
        "VEHICLE_LOAN",
        "HIRE_PURCHASE",
        "FINANCE_LEASE",
        "OPERATING_LEASE",
        "BALLOON_FINANCE",
        "ISLAMIC_FINANCE",
        "CORPORATE_FINANCE",
        "FLEET_FINANCE",
        "REFINANCE",
        "GUARANTEE_AGREEMENT",
        "SECURITY_AGREEMENT",
        "OTHER"
      ]
    },
    "execution_method": {
      "type": "string",
      "enum": [
        "ELECTRONIC",
        "PHYSICAL",
        "HYBRID",
        "REMOTE_NOTARIZED",
        "IN_PERSON_NOTARIZED",
        "OTHER"
      ]
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "vehicle_selling_price_amount": {
      "type": "number",
      "minimum": 0
    },
    "total_transaction_amount": {
      "type": "number",
      "minimum": 0
    },
    "customer_down_payment_amount": {
      "type": "number",
      "minimum": 0
    },
    "trade_in_net_equity_amount": {
      "type": "number"
    },
    "amount_due_at_signing": {
      "type": "number",
      "minimum": 0
    },
    "principal_amount": {
      "type": "number",
      "minimum": 0
    },
    "financed_amount": {
      "type": "number",
      "minimum": 0
    },
    "annual_interest_rate": {
      "type": "number",
      "minimum": 0
    },
    "annual_percentage_rate": {
      "type": "number",
      "minimum": 0
    },
    "finance_charge_amount": {
      "type": "number",
      "minimum": 0
    },
    "total_repayment_amount": {
      "type": "number",
      "minimum": 0
    },
    "term_months": {
      "type": "integer",
      "minimum": 1
    },
    "payment_frequency": {
      "type": "string",
      "enum": [
        "WEEKLY",
        "BIWEEKLY",
        "MONTHLY",
        "QUARTERLY",
        "SEMIANNUAL",
        "ANNUAL",
        "CUSTOM"
      ]
    },
    "installment_count": {
      "type": "integer",
      "minimum": 1
    },
    "installment_amount": {
      "type": "number",
      "minimum": 0
    },
    "first_payment_date": {
      "type": "string",
      "format": "date"
    },
    "final_payment_date": {
      "type": "string",
      "format": "date"
    },
    "balloon_payment_amount": {
      "type": "number",
      "minimum": 0
    },
    "vin": {
      "type": "string",
      "minLength": 11,
      "maxLength": 17
    },
    "document_language_code": {
      "type": "string",
      "minLength": 2,
      "maxLength": 20
    }
  },
  "required": [
    "customer_id",
    "opportunity_id",
    "quotation_id",
    "deal_id",
    "vehicle_id",
    "finance_application_id",
    "lender_id",
    "finance_program_id",
    "contract_template_id",
    "assigned_finance_user_id",
    "contract_type",
    "execution_method",
    "currency_code",
    "vehicle_selling_price_amount",
    "total_transaction_amount",
    "customer_down_payment_amount",
    "trade_in_net_equity_amount",
    "amount_due_at_signing",
    "principal_amount",
    "financed_amount",
    "annual_interest_rate",
    "annual_percentage_rate",
    "finance_charge_amount",
    "total_repayment_amount",
    "term_months",
    "payment_frequency",
    "installment_count",
    "installment_amount",
    "first_payment_date",
    "final_payment_date",
    "balloon_payment_amount",
    "vin",
    "document_language_code"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type FinancialContract {
  id: ID!
  dealershipId: ID!
  customerId: ID!
  coApplicantCustomerId: ID
  guarantorCustomerId: ID
  opportunityId: ID!
  quotationId: ID!
  dealId: ID!
  vehicleId: ID!
  tradeInId: ID
  financeApplicationId: ID!
  lenderId: ID!
  financeProgramId: ID!
  paymentScheduleId: ID
  fundingTransactionId: ID
  documentId: ID
  signedDocumentId: ID
  complianceCaseId: ID
  contractTemplateId: ID!
  supersedesContractId: ID
  assignedFinanceUserId: ID!
  contractNumber: String!
  contractType: FinancialContractType!
  status: FinancialContractStatus!
  executionMethod: ContractExecutionMethod!
  contractVersion: Int!
  isCurrentVersion: Boolean!
  externalContractReference: String
  lenderContractReference: String
  governingLawCode: String!
  jurisdictionCode: String!
  customerLegalName: String!
  coApplicantLegalName: String
  guarantorLegalName: String
  partyVerificationStatus: PartyVerificationStatus!
  currencyCode: String!
  vehicleSellingPriceAmount: Float!
  totalTransactionAmount: Float!
  customerDownPaymentAmount: Float!
  tradeInNetEquityAmount: Float!
  amountDueAtSigning: Float!
  principalAmount: Float!
  financedAmount: Float!
  annualInterestRate: Float!
  annualPercentageRate: Float!
  financeChargeAmount: Float!
  totalRepaymentAmount: Float!
  termMonths: Int!
  paymentFrequency: PaymentFrequency!
  installmentCount: Int!
  installmentAmount: Float!
  firstPaymentDate: Date!
  finalPaymentDate: Date!
  balloonPaymentAmount: Float!
  residualValueAmount: Float!
  vin: String!
  lienRegistrationRequired: Boolean!
  lienRegistrationStatus: LienRegistrationStatus!
  disclosureStatus: ContractDisclosureStatus!
  signatureStatus: ContractSignatureStatus!
  requiredSignerCount: Int!
  completedSignerCount: Int!
  customerSignatureStatus: SignerStatus!
  coApplicantSignatureStatus: SignerStatus!
  guarantorSignatureStatus: SignerStatus!
  dealershipSignatureStatus: SignerStatus!
  lenderSignatureStatus: SignerStatus!
  complianceStatus: ContractComplianceStatus!
  lenderApprovalConfirmationStatus: String!
  contractTermsValidationStatus: ContractTermsValidationStatus!
  fundingEligibilityStatus: FundingEligibilityStatus!
  fundingRequestStatus: ContractFundingRequestStatus!
  fundingReconciliationStatus: FundingReconciliationStatus!
  activationStatus: ContractActivationStatus!
  fundedAmount: Float!
  settlementStatus: ContractSettlementStatus!
  settlementAmount: Float
  documentGenerationStatus: ContractDocumentGenerationStatus!
  signatureRequestSentAt: DateTime
  fullySignedAt: DateTime
  effectiveAt: DateTime
  fundingRequestedAt: DateTime
  fundingReceivedAt: DateTime
  activatedAt: DateTime
  cancelledAt: DateTime
  voidedAt: DateTime
  expiredAt: DateTime
  terminatedAt: DateTime
  completedAt: DateTime
  archivedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `financial_contracts`
- **Party Table:** `financial_contract_parties`
- **Terms Table:** `financial_contract_terms`
- **Disclosure Table:** `financial_contract_disclosures`
- **Signer Table:** `financial_contract_signers`
- **Signature-Event Table:** `financial_contract_signature_events`
- **Document Table:** `financial_contract_documents`
- **Compliance Table:** `financial_contract_compliance`
- **Funding Table:** `financial_contract_funding`
- **Payment-Schedule Table:** `financial_contract_payment_schedules`
- **Amendment Table:** `financial_contract_amendments`
- **Version-History Table:** `financial_contract_versions`
- **Status-History Table:** `financial_contract_status_history`
- **Audit Table:** `financial_contract_audit_log`

### Indexes

- `idx_financial_contracts_tenant_status (dealership_id, status)`  
  Used for contract-generation, signature, funding, and active-contract queues.

- `idx_financial_contracts_customer (dealership_id, customer_id, created_at DESC)`  
  Used for Customer contract history.

- `idx_financial_contracts_deal (dealership_id, deal_id, status)`  
  Used for Deal-contract validation.

- `idx_financial_contracts_finance_application (finance_application_id)`  
  Used to retrieve the contract created from a Finance Application.

- `idx_financial_contracts_vehicle (dealership_id, vehicle_id, status)`  
  Used for Vehicle contract and lien checks.

- `idx_financial_contracts_lender (dealership_id, lender_id, status)`  
  Used for lender-contract operations.

- `idx_financial_contracts_assigned_user (dealership_id, assigned_finance_user_id, status)`  
  Used for Finance User work queues.

- `idx_financial_contracts_signature (dealership_id, signature_status, signature_request_sent_at)`  
  Used for pending and expired signature monitoring.

- `idx_financial_contracts_funding (dealership_id, funding_request_status, funding_requested_at)`  
  Used for funding queues.

- `idx_financial_contracts_effective (dealership_id, effective_at, status)`  
  Used for effective-contract reporting.

- `idx_financial_contracts_payment_dates (dealership_id, first_payment_date, final_payment_date)`  
  Used for payment-schedule monitoring.

- `idx_financial_contracts_lien (dealership_id, lien_registration_status, vehicle_id)`  
  Used for lien-registration operations.

- `idx_financial_contracts_number (dealership_id, contract_number)`  
  Used for human-readable lookup.

- `idx_financial_contracts_current_version (dealership_id, deal_id, is_current_version)`  
  Used to retrieve the current contract version.

- `idx_financial_contracts_external_reference (lender_id, lender_contract_reference)`  
  Used for lender integration reconciliation.

### Unique Constraints

- `UQ_financial_contract_number (dealership_id, contract_number)`

- `UQ_financial_contract_series_version (dealership_id, deal_id, contract_version)`

- `UQ_current_financial_contract (dealership_id, deal_id) WHERE is_current_version = true`

- `UQ_finance_application_contract_version (finance_application_id, contract_version)`

- `UQ_lender_contract_reference (lender_id, lender_contract_reference)`  
  Applies when `lender_contract_reference` is populated.

- `UQ_signature_envelope (signature_provider, signature_envelope_id)`  
  Applies when `signature_envelope_id` is populated.

- `UQ_signed_document_hash (signed_document_hash)`  
  Applies when the executed document must be globally unique.

- `UQ_funding_transaction_contract (funding_transaction_id)`  
  Applies when `funding_transaction_id` is populated.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)`
- `co_applicant_customer_id` → `customers(id)` — nullable
- `guarantor_customer_id` → `customers(id)` — nullable
- `opportunity_id` → `opportunities(id)`
- `quotation_id` → `quotations(id)`
- `deal_id` → `deals(id)`
- `vehicle_id` → `vehicles(id)`
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)`
- `lender_id` → `lenders(id)`
- `finance_program_id` → `finance_programs(id)`
- `payment_schedule_id` → `payment_schedules(id)` — nullable
- `funding_transaction_id` → `finance_funding_transactions(id)` — nullable
- `document_id` → `documents(id)` — nullable
- `signed_document_id` → `documents(id)` — nullable
- `compliance_case_id` → `compliance_cases(id)` — nullable
- `contract_template_id` → `contract_templates(id)`
- `supersedes_contract_id` → `financial_contracts(id)` — nullable
- `assigned_finance_user_id` → `users(id)`
- `created_by` → `users(id)`
- `reviewed_by` → `users(id)` — nullable
- `approved_by` → `users(id)` — nullable
- `cancelled_by` → `users(id)` — nullable
- `voided_by` → `users(id)` — nullable
- `terminated_by` → `users(id)` — nullable

### Database Constraints

- `contract_version >= 1`
- Financial amounts must be zero or greater unless the field explicitly supports signed equity adjustments.
- `customer_down_payment_amount <= total_transaction_amount`
- `term_months > 0`
- `installment_count > 0`
- `annual_interest_rate >= 0`
- `annual_percentage_rate >= 0`
- `first_payment_date < final_payment_date`
- `completed_signer_count BETWEEN 0 AND required_signer_count`
- `required_signer_count >= 1`
- `fully_signed_at IS NOT NULL` when `signature_status = COMPLETED`.
- `signed_document_hash IS NOT NULL` before status becomes `EFFECTIVE`.
- `disclosure_evidence_hash IS NOT NULL` before signature when disclosures are required.
- `funding_reference IS NOT NULL` and `funded_amount IS NOT NULL` before status becomes `ACTIVE`.
- `funding_received_at >= funding_requested_at`
- `settled_at IS NOT NULL` when `settlement_status = SETTLED`.
- `cancellation_reason IS NOT NULL` when status is `CANCELLED`.
- `void_reason IS NOT NULL` when status is `VOIDED`.
- `termination_reason IS NOT NULL` when status is `TERMINATED`.
- `supersedes_contract_id IS NOT NULL` when status is `SUPERSEDED`.
- `co_applicant_customer_id != customer_id`
- `guarantor_customer_id != customer_id`
- `guarantor_customer_id != co_applicant_customer_id`
- Generated, signed, effective, and active contract snapshots must be immutable.
- Circular contract-version and supersession relationships are prohibited.

### Storage Strategy

- Store generated and signed contract documents in immutable encrypted object storage.
- Store only secure document references and hashes in relational tables.
- Preserve unsigned and signed document versions separately.
- Preserve every signature-provider event in append-only storage.
- Use cryptographic hashes for documents, disclosures, signatures, and snapshots.
- Store sensitive party, finance, and compliance snapshots using field-level encryption.
- Public document URLs are prohibited.
- Search indexes must exclude signed-document content and restricted financial data.
- Legal-hold records must prevent storage deletion and lifecycle cleanup.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical records by `created_at`.
- Party, disclosure, signature, document, funding, schedule, amendment, history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Limited read access to contract status and Customer-visible milestones for assigned Deals.
- **Finance User:** Create, prepare, validate, generate, and coordinate Financial Contracts within authorized scope.
- **Finance Manager:** Approve permitted contract workflows, manage lender funding, and supervise reconciliation.
- **Legal User:** Review legal templates, jurisdiction rules, amendments, voiding, and termination.
- **Compliance User:** Review identity, disclosure, sanctions, fraud, consent, signature, and regulatory requirements.
- **Sales Manager:** Read access to contract progress and permitted commercial context without unrestricted legal or financial-document access.
- **Accounting User:** Access to amount-due-at-signing, funding, settlement, and reconciliation records.
- **Delivery Coordinator:** Read access to contract effectiveness and funding eligibility needed for Vehicle delivery.
- **Customer Self-Service User:** Access only to their own permitted contract, disclosure, signature, and document-copy actions.
- **AI Contract Agent:** Service Account access limited to completeness checks, structured comparisons, summaries, classification, and approved workflow requests.
- **Signature Provider Service:** Restricted envelope, signer, signature-event, certificate, and signed-document access.
- **Lender Integration Service:** Restricted contract-reference, funding-request, and funding-event access.
- **Document Service:** Restricted template-generation, document-storage, hashing, and retrieval operations.
- **Audit Service:** Read-only access to immutable lifecycle, signature, document, and funding evidence.

### Data Classification

- **Level:** `LEGALLY RESTRICTED FINANCIAL PII`

The Financial Contract may contain or reference:

- Legal names
- Dates of birth
- National identifiers
- Addresses
- Contact details
- Signatures
- Identity documents
- Employment and income information
- Credit and finance information
- Vehicle security interests
- Payment obligations
- Lender references
- Guarantor information
- Signed legal agreements
- Settlement and termination information

### Legally and Financially Sensitive Fields

- `principal_amount`
- `financed_amount`
- `annual_interest_rate`
- `annual_percentage_rate`
- `finance_charge_amount`
- `total_repayment_amount`
- `installment_amount`
- `balloon_payment_amount`
- `residual_value_amount`
- `customer_down_payment_amount`
- `trade_in_net_equity_amount`
- `amount_due_at_signing`
- `funded_amount`
- `settlement_amount`
- `signature_envelope_id`
- `signature_certificate_reference`
- `signed_document_hash`
- `lender_contract_reference`
- Party legal and identity information

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, documents, snapshots, signature evidence, event stores, and backups.
- **Column-Level Protection:** Party information, legal names, identifiers, addresses, contact details, finance terms, lender references, signature references, disclosure evidence, and funding information require encryption, tokenization, or equivalent protection.
- Signed contracts, identity documents, disclosures, certificates, and amendments must be stored in an encrypted immutable Document Vault.
- Signature images and biometric signature data require enhanced restricted storage when legally permitted.
- Provider credentials, API keys, signing secrets, and access tokens must be stored in a secrets-management system.
- Encryption keys must be separated by environment and rotated according to security policy.

### Document and Signature Security

- Every generated contract document must have a cryptographic hash.
- Every signed document must preserve:
  - Original unsigned document hash.
  - Final signed document hash.
  - Signature-provider certificate.
  - Signer identity and authentication evidence.
  - Signature timestamps.
  - Envelope reference.
  - Document version.

- Signature-provider webhooks must be authenticated and replay-protected.
- Signed documents must be immutable.
- Secure document links must be:
  - Time-limited.
  - Single-purpose.
  - Tenant-scoped.
  - Identity-validated.
  - Cryptographically protected.

- Internal notes and restricted compliance findings must not appear in Customer contract copies.
- Contract HTML or document previews must be protected against script injection and unauthorized field substitution.

### Consent, Disclosure, and Legal Controls

- Every signer must receive the exact document version they are expected to sign.
- Mandatory disclosures must be presented before or during execution according to applicable law.
- Disclosure acknowledgement must preserve evidence and timestamp.
- Electronic execution must comply with applicable jurisdictional and lender requirements.
- Contract language must match approved Customer and legal requirements.
- Cooling-off periods must be calculated and enforced when applicable.
- A Customer withdrawal, signature decline, or consent withdrawal must be processed according to contract status and law.
- Contract data must not be reused for unrelated marketing or model training without lawful and contractual authority.

### Audit Requirements

- Every contract creation and generation operation must preserve:
  - Source Quotation.
  - Source Deal.
  - Source Finance Application.
  - Lender decision version.
  - Template and formula versions.
  - Actor or service.
  - Timestamp.

- Every term-validation operation must preserve:
  - Contract field.
  - Authoritative source.
  - Contract value.
  - Authoritative value.
  - Match or mismatch result.
  - Reviewer decision.
  - Timestamp.

- Every disclosure operation must preserve:
  - Disclosure type.
  - Disclosure version.
  - Presented document.
  - Recipient.
  - Presentation method.
  - Acknowledgement evidence.
  - Timestamp.

- Every signature operation must preserve:
  - Signer identity.
  - Signer role.
  - Authentication method.
  - Signature provider.
  - Envelope ID.
  - Document hash.
  - Event type.
  - IP or device metadata when legally permitted.
  - Timestamp.

- Every effectiveness and activation operation must preserve:
  - Required conditions.
  - Condition results.
  - Authorizing actor or service.
  - Supporting evidence.
  - Timestamp.

- Every funding operation must preserve:
  - Funding request.
  - Lender reference.
  - Amount.
  - Currency.
  - Reconciliation result.
  - Funding transaction.
  - Actor or service.
  - Timestamp.

- Every amendment, cancellation, voiding, termination, settlement, or completion must preserve:
  - Reason.
  - Authority.
  - Supporting evidence.
  - Affected contract version.
  - Customer communication.
  - Timestamp.

- Every AI operation must preserve:
  - Model reference.
  - Prompt version.
  - Authorized input scope.
  - Output.
  - Confidence.
  - Human-review status.
  - Timestamp.

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to contract documents, signatures, identity data, finance terms, compliance findings, and funding data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, co-applicant, guarantor, Opportunity, Quotation, Deal, Vehicle, Trade-In, Finance Application, lender, document, funding, or Financial Contract linking is prohibited.
- Signature-provider events must map to exactly one authenticated tenant and contract version.
- Lender events must map to exactly one tenant, lender, Deal, and Financial Contract.
- AI Agents, Jobs, integrations, exports, analytics, and document-generation services must receive tenant scope through signed execution context.
- Contract documents and signature links must never expose another tenant's records.
- Shared lender infrastructure must enforce logical and cryptographic tenant separation.

### Retention and Deletion

- Financial Contracts must follow applicable legal, financial, credit, tax, contractual, privacy, audit, and regulatory retention requirements.
- Generated, signed, effective, active, completed, voided, terminated, and superseded contract versions must remain immutable.
- Hard deletion is prohibited while the contract is linked to:
  - A Deal
  - A Finance Application
  - A Payment
  - A Funding Transaction
  - A Payment Schedule
  - A Vehicle delivery
  - A lien record
  - A settlement
  - A compliance case
  - A dispute
  - A legal hold
  - An audit record

- Legal hold overrides ordinary deletion and archival schedules.
- Soft deletion is allowed only for eligible Draft records that have not been signed, submitted, or legally relied upon.
- Legally approved deletion or anonymization requests must preserve records required by law.
- Deletion and anonymization must address:
  - Financial Contract records
  - Party snapshots
  - Generated documents
  - Signed documents
  - Disclosure evidence
  - Signature events and certificates
  - Compliance records
  - Funding records
  - Payment Schedule records
  - Amendments
  - Search indexes
  - AI analysis and embeddings
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
