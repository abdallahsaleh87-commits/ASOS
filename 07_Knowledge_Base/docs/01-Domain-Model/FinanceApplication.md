# Finance Application

## 1. Object Purpose

### Business Purpose

The Finance Application object represents a Customer's formal request for vehicle financing, leasing, or another approved credit product through the dealership and one or more authorized lenders.

It provides a controlled process for:

- Collecting applicant and co-applicant financial information.
- Recording explicit Customer consent.
- Verifying identity, employment, income, residence, and required documents.
- Submitting finance requests to approved lenders.
- Tracking underwriting, conditions, approvals, and declines.
- Comparing permitted lender offers.
- Recording Customer acceptance of an approved finance offer.
- Supporting Quotation and Deal funding.
- Monitoring lender disbursement and funding completion.
- Preserving decision and compliance evidence.

A Finance Application does not itself represent:

- A guaranteed finance approval.
- A signed finance contract.
- A cleared lender payment.
- A completed Deal.
- A final interest rate before authoritative lender approval.

The approved lending terms become binding only through the applicable signed Financial Contract and successful lender-funding process.

### System Purpose

The Finance Application object is the canonical credit-request and underwriting aggregate within the ASOS sales domain.

It connects:

- Customer
- Co-Applicant
- Opportunity
- Quotation
- Deal
- Vehicle
- Trade-In
- Lender
- Finance Program
- Credit Bureau
- Document Vault
- Compliance Case
- Financial Contract
- Funding Transaction

The object preserves the complete lifecycle from initial application through:

- Consent collection.
- Data completion.
- Document verification.
- Lender submission.
- Underwriting.
- Conditional approval.
- Final approval.
- Customer selection.
- Contract preparation.
- Funding.
- Decline, withdrawal, cancellation, or expiry.

The object provides authoritative finance context to:

- Quotation workflows.
- Deal approval and funding workflows.
- Lender integrations.
- Compliance and identity-verification workflows.
- Document collection.
- Customer communication.
- Funding reconciliation.
- Finance-conversion analytics.
- Audit and regulatory reporting.

Every lender submission must be idempotent, tenant-scoped, consent-backed, and traceable to the exact application version submitted.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `finance_application_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — required)
  - `opportunity_id` (UUIDv4 — required)
  - `quotation_id` (UUIDv4 — optional)
  - `deal_id` (UUIDv4 — optional until Deal creation)
  - `vehicle_id` (UUIDv4 — required)
  - `trade_in_id` (UUIDv4 — optional)
  - `assigned_finance_user_id` (UUIDv4 — required)
  - `co_applicant_customer_id` (UUIDv4 — optional)
  - `selected_lender_id` (UUIDv4 — optional)
  - `selected_finance_program_id` (UUIDv4 — optional)
  - `financial_contract_id` (UUIDv4 — optional)
  - `compliance_case_id` (UUIDv4 — optional)
  - `funding_transaction_id` (UUIDv4 — optional)

### Application Identity

- `application_number`
- `application_type`
- `finance_product_type`
- `status`
- `submission_channel`
- `application_version`
- `is_current_version`
- `supersedes_application_id`

### Applicant Information

- `applicant_type`
- `legal_name`
- `date_of_birth`
- `national_identifier_token`
- `residency_status`
- `marital_status`
- `dependents_count`
- `preferred_language`
- `primary_phone`
- `primary_email`
- `current_address`
- `residence_type`
- `months_at_current_address`

### Employment and Income

- `employment_status`
- `employer_name`
- `job_title`
- `employment_start_date`
- `months_with_employer`
- `gross_monthly_income_amount`
- `net_monthly_income_amount`
- `other_monthly_income_amount`
- `other_income_source`
- `income_verification_status`

### Co-Applicant Information

- `co_applicant_required`
- `co_applicant_customer_id`
- `co_applicant_relationship`
- `co_applicant_income_amount`
- `co_applicant_consent_status`
- `co_applicant_verification_status`

### Financial Position

- `monthly_housing_cost_amount`
- `monthly_debt_payment_amount`
- `monthly_other_commitment_amount`
- `declared_assets_amount`
- `declared_liabilities_amount`
- `requested_down_payment_amount`
- `available_down_payment_amount`
- `trade_in_equity_amount`
- `requested_finance_amount`
- `requested_term_months`
- `preferred_monthly_payment_amount`
- `maximum_monthly_payment_amount`
- `currency_code`

### Vehicle and Transaction Snapshot

- `vehicle_list_price`
- `vehicle_selling_price`
- `total_transaction_amount`
- `customer_cash_contribution_amount`
- `trade_in_allowance_amount`
- `trade_in_payoff_amount`
- `trade_in_net_equity_amount`
- `finance_amount_requested`
- `vehicle_snapshot`
- `commercial_snapshot`

### Consent and Authorization

- `credit_check_consent_status`
- `data_processing_consent_status`
- `lender_sharing_consent_status`
- `electronic_communication_consent_status`
- `consent_captured_at`
- `consent_channel`
- `consent_document_hash`
- `consent_version`
- `consent_withdrawn_at`

### Identity and Document Verification

- `identity_verification_status`
- `address_verification_status`
- `employment_verification_status`
- `income_verification_status`
- `bank_statement_verification_status`
- `document_completion_status`
- `required_document_types`
- `missing_document_types`
- `document_verification_snapshot`

### Credit and Risk Assessment

- `credit_bureau_request_status`
- `credit_bureau_score`
- `credit_bureau_band`
- `credit_bureau_reference`
- `credit_report_retrieved_at`
- `debt_to_income_ratio`
- `payment_to_income_ratio`
- `loan_to_value_ratio`
- `internal_risk_band`
- `fraud_risk_score`
- `affordability_status`
- `risk_assessment_snapshot`

### Lender Submission

- `lender_submission_count`
- `last_submitted_at`
- `selected_lender_id`
- `selected_finance_program_id`
- `lender_application_reference`
- `lender_submission_status`
- `lender_submission_snapshot`
- `lender_response_received_at`

### Underwriting and Decision

- `decision_status`
- `decision_reason_code`
- `decision_details`
- `approved_finance_amount`
- `approved_down_payment_amount`
- `approved_interest_rate`
- `approved_annual_percentage_rate`
- `approved_term_months`
- `approved_monthly_payment_amount`
- `approved_balloon_payment_amount`
- `approval_valid_until`
- `underwriting_conditions`
- `outstanding_conditions`
- `conditions_satisfied_at`

### Customer Offer Selection

- `offer_presented_at`
- `offer_selected_at`
- `customer_decision`
- `customer_decline_reason`
- `selected_offer_snapshot`
- `customer_acceptance_evidence`

### Funding Fields

- `funding_status`
- `funding_requested_at`
- `funding_approved_at`
- `funding_received_at`
- `funded_amount`
- `funding_reference`
- `funding_reconciliation_status`

### Computed Fields

- `total_verified_monthly_income`
- `total_monthly_commitments`
- `disposable_monthly_income`
- `debt_to_income_ratio`
- `payment_to_income_ratio`
- `loan_to_value_ratio`
- `document_completion_percentage`
- `application_age_days`
- `days_in_underwriting`
- `days_until_approval_expiry`
- `approval_expired`
- `funding_shortfall_amount`
- `conditions_completion_percentage`

### Governance and Lifecycle

- **Applicant Snapshot:** `applicant_snapshot` (Encrypted JSONB)
- **Co-Applicant Snapshot:** `co_applicant_snapshot` (Encrypted JSONB)
- **Commercial Snapshot:** `commercial_snapshot` (JSONB)
- **Consent Snapshot:** `consent_snapshot` (Encrypted JSONB)
- **Document Snapshot:** `document_verification_snapshot` (Encrypted JSONB)
- **Risk Snapshot:** `risk_assessment_snapshot` (Encrypted JSONB)
- **Lender Submission Snapshot:** `lender_submission_snapshot` (Encrypted JSONB)
- **Decision Snapshot:** `decision_snapshot` (Encrypted JSONB)
- **Selected Offer Snapshot:** `selected_offer_snapshot` (Encrypted JSONB)
- **Funding Snapshot:** `funding_snapshot` (Encrypted JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `submitted_by`
  - `reviewed_by`
  - `approved_by`
  - `cancelled_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `consent_captured_at`
  - `submitted_at`
  - `last_submitted_at`
  - `credit_report_retrieved_at`
  - `lender_response_received_at`
  - `decision_at`
  - `offer_presented_at`
  - `offer_selected_at`
  - `conditions_satisfied_at`
  - `contract_ready_at`
  - `funding_requested_at`
  - `funding_approved_at`
  - `funding_received_at`
  - `withdrawn_at`
  - `declined_at`
  - `cancelled_at`
  - `expired_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| finance_application_id | UUID | Unique canonical identifier for the Finance Application. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the application. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Primary Customer applying for finance. | Yes | N/A | Must reference an active Customer in the same dealership | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| opportunity_id | UUID | Opportunity supported by the Finance Application. | Yes | N/A | Must belong to the same Customer and dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| quotation_id | UUID | Quotation containing the proposed commercial terms. | No | Null | Must belong to the same Customer, Opportunity, and Vehicle | 444e5555-e66b-77d8-a999-426614174000 | System-controlled |
| deal_id | UUID | Deal funded through the application. | No | Null | Must belong to the same Customer, Opportunity, and Vehicle | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| vehicle_id | UUID | Vehicle being financed or leased. | Yes | From Opportunity | Must reference an eligible Vehicle in the same dealership | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| assigned_finance_user_id | UUID | Finance User responsible for the application. | Yes | Assignment rules | Must reference an active authorized User | 321e6547-e89b-12d3-a456-426614174000 | System-controlled |
| application_number | String | Human-readable finance application reference. | Yes | System-generated | Must be unique within the dealership | FA-2026-000319 | System-generated |
| application_type | Enum | Indicates whether the request is individual, joint, or corporate. | Yes | INDIVIDUAL | Must match FinanceApplicationType ENUM | JOINT | At least 0.99 |
| finance_product_type | Enum | Type of financial product requested. | Yes | RETAIL_FINANCE | Must match FinanceProductType ENUM | RETAIL_FINANCE | At least 0.99 |
| status | Enum | Current lifecycle state of the application. | Yes | DRAFT | Must match FinanceApplicationStatus ENUM | UNDER_REVIEW | At least 0.99 |
| submission_channel | Enum | Channel through which the application was received. | Yes | DEALERSHIP | Must match FinanceSubmissionChannel ENUM | DIGITAL | At least 0.95 |
| application_version | Integer | Sequential version submitted to lenders. | Yes | 1 | Must be one or greater and increase sequentially | 2 | System-controlled |
| legal_name | String | Applicant's verified legal name. | Yes | From Customer | Must match verified identity evidence | Mahmoud Ahmed Nasser | Authoritative evidence |
| date_of_birth | Date | Applicant's date of birth. | Yes | N/A | Applicant must meet lender and legal age requirements | 1990-06-15 | Authoritative evidence |
| national_identifier_token | String | Tokenized reference to the applicant's national identifier. | Yes | N/A | Raw identifier must not be stored in unrestricted form | tok_id_01JABCD123 | Authoritative system |
| residency_status | Enum | Applicant's legal residency classification. | Yes | UNKNOWN | Must match ResidencyStatus ENUM | RESIDENT | Authoritative evidence |
| employment_status | Enum | Applicant's current employment classification. | Yes | UNKNOWN | Must match EmploymentStatus ENUM | EMPLOYED | Verified evidence |
| employer_name | String | Current employer or business name. | Conditional | Null | Required for employed or self-employed applicants | ASOS Automotive Group | Verified evidence |
| gross_monthly_income_amount | Decimal | Applicant's gross monthly income. | Yes | 0.00 | Must be zero or greater and supported by evidence when required | 85000.00 | Verified financial evidence |
| net_monthly_income_amount | Decimal | Applicant's verified net monthly income. | Yes | 0.00 | Must be zero or greater | 68000.00 | Verified financial evidence |
| other_monthly_income_amount | Decimal | Additional verified recurring monthly income. | Yes | 0.00 | Must be zero or greater | 5000.00 | Verified evidence |
| monthly_housing_cost_amount | Decimal | Applicant's recurring monthly housing cost. | Yes | 0.00 | Must be zero or greater | 12000.00 | Applicant plus verification |
| monthly_debt_payment_amount | Decimal | Existing monthly debt repayments. | Yes | 0.00 | Must be zero or greater | 8000.00 | Credit bureau or verified evidence |
| requested_down_payment_amount | Decimal | Down payment proposed by the applicant. | Yes | 0.00 | Must be zero or greater and not exceed transaction amount | 500000.00 | Customer-provided |
| finance_amount_requested | Decimal | Total finance amount requested from lenders. | Yes | Calculated | Must be zero or greater and match the commercial calculation | 1710000.00 | System-computed |
| requested_term_months | Integer | Preferred finance duration. | Yes | Policy-defined | Must match an available lender term | 60 | Customer or finance user |
| preferred_monthly_payment_amount | Decimal | Customer's preferred monthly payment. | No | Null | Must be zero or greater | 45000.00 | Customer-provided |
| maximum_monthly_payment_amount | Decimal | Customer's stated maximum acceptable payment. | No | Null | Must be zero or greater and not below the preferred amount | 50000.00 | Customer-provided |
| currency_code | String | ISO 4217 currency code for financial values. | Yes | Dealership default | Exactly three uppercase characters | EGP | At least 0.99 |
| credit_check_consent_status | Enum | Status of consent to obtain credit information. | Yes | NOT_REQUESTED | Must match FinanceConsentStatus ENUM | GRANTED | Authoritative evidence |
| data_processing_consent_status | Enum | Status of consent to process financial and personal data. | Yes | NOT_REQUESTED | Must match FinanceConsentStatus ENUM | GRANTED | Authoritative evidence |
| lender_sharing_consent_status | Enum | Status of consent to share information with lenders. | Yes | NOT_REQUESTED | Must match FinanceConsentStatus ENUM | GRANTED | Authoritative evidence |
| consent_document_hash | String | Cryptographic hash of the signed or recorded consent artifact. | Conditional | Null | Required before lender submission | sha256:9f31... | System-generated |
| identity_verification_status | Enum | Result of applicant identity verification. | Yes | NOT_STARTED | Must match VerificationStatus ENUM | VERIFIED | Authoritative evidence |
| document_completion_status | Enum | Completion state of required finance documents. | Yes | NOT_STARTED | Must match DocumentCompletionStatus ENUM | COMPLETE | System-controlled |
| credit_bureau_score | Integer | Score returned by an authorized credit bureau. | No | Null | Must be stored only when legally permitted | 720 | Authorized bureau |
| debt_to_income_ratio | Decimal | Monthly commitments divided by verified monthly income. | No | Null | Must be system-computed and remain between 0 and permitted maximum | 0.29 | System-computed |
| payment_to_income_ratio | Decimal | Proposed monthly payment divided by verified monthly income. | No | Null | Must be system-computed | 0.21 | System-computed |
| loan_to_value_ratio | Decimal | Requested finance amount divided by permitted Vehicle value. | No | Null | Must be system-computed | 0.81 | System-computed |
| affordability_status | Enum | Result of the affordability assessment. | Yes | NOT_ASSESSED | Must match AffordabilityStatus ENUM | PASSED | Authorized rules engine |
| selected_lender_id | UUID | Lender whose offer was selected. | No | Null | Must reference an authorized active lender | 888e9999-e00b-11d2-a222-426614174000 | Authorized human or workflow |
| lender_application_reference | String | External lender application identifier. | No | Null | Must be unique per lender when populated | LND-APP-784512 | Trusted integration |
| decision_status | Enum | Current underwriting decision. | Yes | NOT_DECIDED | Must match FinanceDecisionStatus ENUM | CONDITIONALLY_APPROVED | Lender-controlled |
| approved_finance_amount | Decimal | Finance amount approved by the lender. | No | Null | Must come from an authoritative lender decision | 1650000.00 | Lender-controlled |
| approved_interest_rate | Decimal | Final lender-approved nominal annual interest rate. | No | Null | Must be zero or greater | 18.50 | Lender-controlled |
| approved_annual_percentage_rate | Decimal | Lender-approved annual percentage rate including applicable costs. | No | Null | Must be zero or greater | 20.10 | Lender-controlled |
| approved_term_months | Integer | Finance term approved by the lender. | No | Null | Must match an approved lender program | 60 | Lender-controlled |
| approved_monthly_payment_amount | Decimal | Approved Customer periodic payment. | No | Null | Must match lender offer calculations | 43800.00 | Lender-controlled |
| approval_valid_until | Timestamp | Deadline for using the lender approval. | No | Null | Must be later than decision_at | 2026-08-15T23:59:59Z | Lender-controlled |
| funding_status | Enum | Current lender-funding state. | Yes | NOT_STARTED | Must match FinanceFundingStatus ENUM | FUNDING_PENDING | System or lender-controlled |
| funded_amount | Decimal | Cleared amount received from the lender. | No | 0.00 | Must be derived from authoritative funding evidence | 1650000.00 | Authoritative funding source |
| financial_contract_id | UUID | Financial Contract created from the selected approved offer. | No | Null | Required before final funding when contract execution is required | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 7 | System-controlled |

## 4. Enumerations

### FinanceApplicationStatus

- **DRAFT:** Application information is being collected or edited.
- **CONSENT_PENDING:** Mandatory Customer consent has not been completed.
- **DOCUMENTS_REQUIRED:** Required documents or verification evidence are incomplete.
- **READY_FOR_SUBMISSION:** Consent, minimum data, and required checks are complete.
- **SUBMITTED:** The application was sent to at least one authorized lender.
- **UNDER_REVIEW:** A lender or authorized Finance User is reviewing the application.
- **CONDITIONALLY_APPROVED:** Approval exists subject to documented conditions.
- **APPROVED:** Final lender approval is available.
- **OFFER_PRESENTED:** One or more approved finance offers were presented to the Customer.
- **CUSTOMER_ACCEPTED:** The Customer accepted a specific approved offer.
- **CONTRACTING:** The Financial Contract is being prepared or executed.
- **FUNDING_PENDING:** Signed-contract and lender-funding requirements are being processed.
- **FUNDED:** Authoritative lender funds were received and reconciled.
- **DECLINED:** The lender declined the application.
- **CUSTOMER_WITHDREW:** The Customer withdrew the application.
- **EXPIRED:** The application or approval is no longer valid.
- **CANCELLED:** The dealership ended the application for an authorized reason.

### FinanceApplicationType

- INDIVIDUAL
- JOINT
- CORPORATE
- SOLE_PROPRIETOR
- FLEET

### FinanceProductType

- RETAIL_FINANCE
- VEHICLE_LOAN
- HIRE_PURCHASE
- LEASE
- BALLOON_FINANCE
- ISLAMIC_FINANCE
- CORPORATE_FINANCE
- FLEET_FINANCE
- REFINANCE
- OTHER

### FinanceSubmissionChannel

- DEALERSHIP
- DIGITAL
- PHONE
- WHATSAPP
- LENDER_PORTAL
- OEM_PLATFORM
- API_INTEGRATION
- OTHER

### ApplicantType

- PRIMARY
- CO_APPLICANT
- GUARANTOR
- CORPORATE_AUTHORIZED_SIGNATORY

### EmploymentStatus

- UNKNOWN
- EMPLOYED
- SELF_EMPLOYED
- BUSINESS_OWNER
- CONTRACTOR
- RETIRED
- STUDENT
- UNEMPLOYED
- OTHER

### ResidencyStatus

- UNKNOWN
- CITIZEN
- PERMANENT_RESIDENT
- RESIDENT
- TEMPORARY_RESIDENT
- NON_RESIDENT

### ResidenceType

- OWNED
- MORTGAGED
- RENTED
- FAMILY_OWNED
- EMPLOYER_PROVIDED
- OTHER

### FinanceConsentStatus

- NOT_REQUESTED
- PENDING
- GRANTED
- DECLINED
- WITHDRAWN
- EXPIRED

### VerificationStatus

- NOT_STARTED
- PENDING
- VERIFIED
- PARTIALLY_VERIFIED
- FAILED
- EXPIRED
- NOT_REQUIRED

### DocumentCompletionStatus

- NOT_STARTED
- INCOMPLETE
- PENDING_VERIFICATION
- COMPLETE
- REJECTED
- EXPIRED

### CreditBureauRequestStatus

- NOT_REQUESTED
- PENDING
- COMPLETED
- FAILED
- NOT_PERMITTED
- EXPIRED

### AffordabilityStatus

- NOT_ASSESSED
- PENDING
- PASSED
- PASSED_WITH_CONDITIONS
- FAILED
- MANUAL_REVIEW_REQUIRED

### FinanceDecisionStatus

- NOT_DECIDED
- PENDING
- CONDITIONALLY_APPROVED
- APPROVED
- DECLINED
- REFERRED
- WITHDRAWN
- EXPIRED
- CANCELLED

### CustomerFinanceDecision

- NOT_PRESENTED
- PENDING
- ACCEPTED
- DECLINED
- REQUESTED_CHANGES
- EXPIRED

### FinanceFundingStatus

- NOT_STARTED
- CONTRACT_REQUIRED
- DOCUMENTS_REQUIRED
- FUNDING_REQUESTED
- FUNDING_PENDING
- PARTIALLY_FUNDED
- FUNDED
- FUNDING_FAILED
- FUNDING_REVERSED
- CANCELLED

### FinanceDeclineReason

- CREDIT_SCORE
- AFFORDABILITY_FAILED
- INCOME_UNVERIFIED
- EMPLOYMENT_UNVERIFIED
- EXCESSIVE_EXISTING_DEBT
- INSUFFICIENT_DOWN_PAYMENT
- LOAN_TO_VALUE_TOO_HIGH
- IDENTITY_VERIFICATION_FAILED
- DOCUMENTS_INCOMPLETE
- FRAUD_RISK
- RESIDENCY_RESTRICTION
- VEHICLE_INELIGIBLE
- LENDER_POLICY
- OTHER

### FinanceCancellationReason

- CUSTOMER_REQUEST
- OPPORTUNITY_LOST
- DEAL_CANCELLED
- DUPLICATE_APPLICATION
- CONSENT_WITHDRAWN
- VEHICLE_UNAVAILABLE
- APPLICATION_ERROR
- COMPLIANCE_BLOCK
- FRAUD_SUSPECTED
- OTHER

## 5. Validation Rules

### Business Rules

- A Finance Application must belong to one resolved Customer and active Opportunity.
- The Vehicle must be eligible for the selected finance product.
- Mandatory consent must be captured before credit-bureau access or lender submission.
- Consent must identify the purpose, data categories, permitted recipients, version, channel, and timestamp.
- Consent withdrawal must prevent future unauthorized processing and lender submissions.
- A joint application requires verified consent from every applicant.
- Raw national identifiers, bank credentials, or full credit reports must not be stored in unrestricted fields.
- The requested finance amount must match the approved commercial calculation.
- Customer-declared income must remain distinct from verified income.
- Lender decisions must be recorded exactly as received and must not be altered by dealership Users or AI Agents.
- A conditional approval must preserve every outstanding condition.
- An approved offer must not be presented after `approval_valid_until`.
- A Customer acceptance must reference one exact lender offer and decision version.
- Finance approval does not guarantee lender funding.
- The Deal cannot be marked funded until authoritative lender funds are received and reconciled.
- A declined or expired application cannot be resubmitted as the same immutable lender submission; a new version or new application is required.
- Only approved lenders and finance programs may receive Customer data.
- Finance terms presented to the Customer must include all legally required disclosures.
- AI-generated affordability or approval predictions are advisory and cannot replace lender underwriting.

### Technical Rules

- Application creation, submission, offer acceptance, and funding operations must require idempotency keys.
- Every lender submission must preserve the exact submitted application snapshot.
- External lender requests and responses must store:
  - Provider
  - Correlation ID
  - Request timestamp
  - Response timestamp
  - Application version
  - Decision version
  - Response hash
  - Reconciliation status

- `record_version` must increase after every permitted update.
- Submitted application versions must be immutable.
- Material changes after submission require a new version.
- Credit-bureau requests must be deduplicated and consent-validated.
- Financial calculations must use fixed decimal precision.
- Ratios must be calculated server-side.
- Document verification results must preserve source and evidence hashes.
- Lender webhook events must be authenticated, deduplicated, ordered, and replay-safe.
- Funding status must be derived from authoritative lender or banking events.
- Failed multi-service operations must create compensating actions and Human Review Tasks.

### Data Constraints

- Applicant age must meet legal and lender requirements.
- `dependents_count` cannot be negative.
- Income, asset, liability, payment, and finance amounts cannot be negative.
- `requested_down_payment_amount` cannot exceed `total_transaction_amount`.
- `available_down_payment_amount` cannot exceed verified available funds without an approved exception.
- `finance_amount_requested` must equal the authoritative financing formula.
- `approved_finance_amount` cannot exceed permitted lender and transaction limits.
- `approved_down_payment_amount` cannot exceed `total_transaction_amount`.
- `requested_term_months` and `approved_term_months` must match permitted lender terms.
- `maximum_monthly_payment_amount` cannot be less than `preferred_monthly_payment_amount`.
- `debt_to_income_ratio`, `payment_to_income_ratio`, and `loan_to_value_ratio` cannot be negative.
- `approval_valid_until` must be later than `decision_at`.
- `funding_received_at` cannot precede `funding_requested_at`.
- `funded_amount` cannot exceed the approved finance amount without an authorized adjustment.
- `financial_contract_id` must be populated before funding when contract execution is mandatory.
- `consent_document_hash` is required before lender submission.
- `consent_withdrawn_at` is required when mandatory consent becomes `WITHDRAWN`.

### Referential Integrity

- All linked records must belong to the same `dealership_id`.
- `opportunity_id` must belong to `customer_id`.
- `quotation_id` must belong to the same Customer, Opportunity, and Vehicle.
- `deal_id` must belong to the same Customer, Opportunity, and Vehicle.
- `trade_in_id` must belong to the same Customer and Opportunity.
- `co_applicant_customer_id` cannot equal `customer_id`.
- `selected_lender_id` must reference an active authorized lender.
- `selected_finance_program_id` must belong to `selected_lender_id`.
- `financial_contract_id` must reference the selected approved offer.
- `funding_transaction_id` must reference this exact Finance Application.
- `compliance_case_id` must reference the same Customer and transaction context.
- Circular application supersession chains are prohibited.
- A Finance Application cannot be hard-deleted while referenced by a Deal, Financial Contract, funding record, compliance record, lender response, or audit entry.

### Human Approval Requirements

- Finance Users may review completeness but cannot alter authoritative lender decisions.
- Manual income, employment, affordability, or document overrides require authorized review and supporting evidence.
- Submitting to additional lenders outside the approved Customer consent scope requires renewed consent.
- High fraud-risk results require compliance or fraud-specialist review.
- Changing the applicant, co-applicant, Vehicle, requested amount, or finance product after submission requires a new version.
- AI Agents cannot grant consent on behalf of a Customer.
- AI Agents cannot approve, decline, fund, or contract a Finance Application.
- AI Agents cannot alter lender decisions, rates, terms, conditions, or funding evidence.
- AI Agents cannot retrieve credit data without verified consent and authorized scope.
- Conflicting identity, income, employment, lender, consent, or funding evidence must create a Human Review Task.

## 6. State Machine

### Allowed States

- DRAFT
- CONSENT_PENDING
- DOCUMENTS_REQUIRED
- READY_FOR_SUBMISSION
- SUBMITTED
- UNDER_REVIEW
- CONDITIONALLY_APPROVED
- APPROVED
- OFFER_PRESENTED
- CUSTOMER_ACCEPTED
- CONTRACTING
- FUNDING_PENDING
- FUNDED
- DECLINED
- CUSTOMER_WITHDREW
- EXPIRED
- CANCELLED

### Allowed Transitions

- DRAFT → CONSENT_PENDING
- DRAFT → DOCUMENTS_REQUIRED
- DRAFT → READY_FOR_SUBMISSION
- DRAFT → CUSTOMER_WITHDREW
- DRAFT → CANCELLED
- CONSENT_PENDING → DOCUMENTS_REQUIRED
- CONSENT_PENDING → READY_FOR_SUBMISSION
- CONSENT_PENDING → CUSTOMER_WITHDREW
- CONSENT_PENDING → CANCELLED
- DOCUMENTS_REQUIRED → CONSENT_PENDING
- DOCUMENTS_REQUIRED → READY_FOR_SUBMISSION
- DOCUMENTS_REQUIRED → CUSTOMER_WITHDREW
- DOCUMENTS_REQUIRED → CANCELLED
- READY_FOR_SUBMISSION → SUBMITTED
- READY_FOR_SUBMISSION → DOCUMENTS_REQUIRED
- READY_FOR_SUBMISSION → CUSTOMER_WITHDREW
- READY_FOR_SUBMISSION → CANCELLED
- SUBMITTED → UNDER_REVIEW
- SUBMITTED → DOCUMENTS_REQUIRED
- SUBMITTED → CONDITIONALLY_APPROVED
- SUBMITTED → APPROVED
- SUBMITTED → DECLINED
- SUBMITTED → CUSTOMER_WITHDREW
- SUBMITTED → EXPIRED
- UNDER_REVIEW → DOCUMENTS_REQUIRED
- UNDER_REVIEW → CONDITIONALLY_APPROVED
- UNDER_REVIEW → APPROVED
- UNDER_REVIEW → DECLINED
- UNDER_REVIEW → CUSTOMER_WITHDREW
- UNDER_REVIEW → EXPIRED
- CONDITIONALLY_APPROVED → DOCUMENTS_REQUIRED
- CONDITIONALLY_APPROVED → APPROVED
- CONDITIONALLY_APPROVED → DECLINED
- CONDITIONALLY_APPROVED → CUSTOMER_WITHDREW
- CONDITIONALLY_APPROVED → EXPIRED
- APPROVED → OFFER_PRESENTED
- APPROVED → DOCUMENTS_REQUIRED
- APPROVED → CUSTOMER_WITHDREW
- APPROVED → EXPIRED
- OFFER_PRESENTED → CUSTOMER_ACCEPTED
- OFFER_PRESENTED → APPROVED
- OFFER_PRESENTED → CUSTOMER_WITHDREW
- OFFER_PRESENTED → EXPIRED
- CUSTOMER_ACCEPTED → CONTRACTING
- CUSTOMER_ACCEPTED → CUSTOMER_WITHDREW
- CUSTOMER_ACCEPTED → EXPIRED
- CONTRACTING → FUNDING_PENDING
- CONTRACTING → DOCUMENTS_REQUIRED
- CONTRACTING → CUSTOMER_WITHDREW
- CONTRACTING → EXPIRED
- FUNDING_PENDING → FUNDED
- FUNDING_PENDING → DOCUMENTS_REQUIRED
- FUNDING_PENDING → CUSTOMER_WITHDREW
- FUNDING_PENDING → EXPIRED
- FUNDING_PENDING → CANCELLED

### Forbidden Transitions

- DRAFT → SUBMITTED
- DRAFT → APPROVED
- CONSENT_PENDING → SUBMITTED
- DOCUMENTS_REQUIRED → APPROVED
- READY_FOR_SUBMISSION → APPROVED
- SUBMITTED → FUNDED
- UNDER_REVIEW → FUNDED
- CONDITIONALLY_APPROVED → CUSTOMER_ACCEPTED
- APPROVED → CONTRACTING
- OFFER_PRESENTED → CONTRACTING
- CUSTOMER_ACCEPTED → FUNDED
- CONTRACTING → FUNDED
- DECLINED → APPROVED
- DECLINED → SUBMITTED
- CUSTOMER_WITHDREW → SUBMITTED
- EXPIRED → CUSTOMER_ACCEPTED
- EXPIRED → FUNDED
- CANCELLED → SUBMITTED
- FUNDED → CANCELLED
- FUNDED → CUSTOMER_WITHDREW

### Entry Conditions

- To enter `CONSENT_PENDING`:
  - The applicant must be identified.
  - One or more mandatory consents must be missing, pending, expired, or invalid.

- To enter `DOCUMENTS_REQUIRED`:
  - Required identity, income, employment, residence, banking, or transaction documents must be missing, rejected, or expired.
  - `missing_document_types` must identify the outstanding items.

- To enter `READY_FOR_SUBMISSION`:
  - Applicant and transaction data must be complete.
  - Mandatory consent must be granted and current.
  - Identity verification must meet the required threshold.
  - Required documents must be complete or satisfy lender submission rules.
  - The Vehicle and commercial snapshot must be valid.
  - At least one eligible lender and finance program must exist.

- To enter `SUBMITTED`:
  - A lender submission must succeed.
  - The immutable application and consent snapshots must be stored.
  - A lender application reference or accepted submission receipt must exist.
  - `submitted_at` and `last_submitted_at` must be populated.

- To enter `UNDER_REVIEW`:
  - The lender must acknowledge review or an authorized Finance User must begin a permitted manual-review workflow.

- To enter `CONDITIONALLY_APPROVED`:
  - An authoritative lender decision must exist.
  - Approved provisional terms and all outstanding conditions must be recorded.
  - `approval_valid_until` must be populated.

- To enter `APPROVED`:
  - An authoritative final lender approval must exist.
  - Approved amount, rate, term, payment, and validity must be recorded.
  - No unresolved mandatory underwriting condition may remain.

- To enter `OFFER_PRESENTED`:
  - A valid approved lender offer must exist.
  - Required disclosures must be generated.
  - The approval must not be expired.
  - Customer communication consent must permit delivery.

- To enter `CUSTOMER_ACCEPTED`:
  - Verifiable Customer acceptance must reference one exact lender offer and decision version.
  - The offer must remain valid.
  - Acceptance evidence must be stored.

- To enter `CONTRACTING`:
  - The Customer must have accepted the selected offer.
  - Required identity, compliance, and document checks must remain valid.
  - A Financial Contract preparation workflow must begin.

- To enter `FUNDING_PENDING`:
  - Required contracts must be signed.
  - Outstanding lender funding conditions must be satisfied.
  - The Deal must exist and remain eligible.
  - A funding request must be submitted successfully.

- To enter `FUNDED`:
  - Authoritative lender funds must be received.
  - The funded amount must reconcile with the approved amount and Deal.
  - `funding_received_at`, `funded_amount`, and `funding_reference` must be populated.
  - Funding reconciliation must pass.

- To enter `DECLINED`:
  - An authoritative lender decline must exist.
  - A standardized decline reason must be stored when legally permitted.
  - Customer communication must follow applicable disclosure rules.

- To enter `CUSTOMER_WITHDREW`:
  - A verifiable Customer withdrawal request must exist.
  - Future lender submissions and unauthorized processing must stop.
  - Active external applications must be withdrawn when required.

- To enter `EXPIRED`:
  - The application, consent, lender decision, or approved offer must no longer be valid.
  - The expired component must be identified.

- To enter `CANCELLED`:
  - An authorized cancellation reason and actor must be recorded.
  - The application must not already be funded.

### Exit Conditions

- A Finance Application cannot exit `DRAFT` without minimum Customer, Vehicle, Opportunity, and requested-finance information.
- A Finance Application cannot exit `CONSENT_PENDING` toward submission without valid mandatory consent.
- A Finance Application cannot exit `DOCUMENTS_REQUIRED` until required documents are supplied, waived by an authorized lender, or the application ends.
- A Finance Application cannot exit `READY_FOR_SUBMISSION` toward `SUBMITTED` unless lender eligibility and consent checks pass.
- A Finance Application cannot exit `CONDITIONALLY_APPROVED` toward `APPROVED` until all mandatory conditions are satisfied.
- A Finance Application cannot exit `APPROVED` toward presentation after approval expiry.
- A Finance Application cannot exit `OFFER_PRESENTED` toward `CUSTOMER_ACCEPTED` without verifiable acceptance evidence.
- A Finance Application cannot exit `CONTRACTING` toward funding until required contracts are validly executed.
- A Finance Application cannot exit `FUNDING_PENDING` toward `FUNDED` without authoritative funding evidence.
- A `DECLINED`, `CUSTOMER_WITHDREW`, `EXPIRED`, or `CANCELLED` application cannot return to an active state; a new version or application must be created.
- A `FUNDED` application cannot return to underwriting; funding reversal requires a separate controlled exception workflow.

### Terminal States

- **FUNDED:** Lender funds were received and reconciled.
- **DECLINED:** The lender declined the application.
- **CUSTOMER_WITHDREW:** The Customer ended the application.
- **EXPIRED:** The application or approval became invalid.
- **CANCELLED:** The dealership ended the application before funding.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Customer identified by `customer_id`.
  - Active Opportunity identified by `opportunity_id`.
  - Eligible Vehicle identified by `vehicle_id`.
  - Authorized Finance User identified by `assigned_finance_user_id`.
  - Valid Customer consent.
  - Approved lender and finance-product configuration.
  - Applicable identity, document, affordability, compliance, and underwriting rules.

- **Consumes:**
  - Customer identity and contact information.
  - Applicant and co-applicant financial information.
  - Employment, income, residence, asset, and liability evidence.
  - Opportunity and Vehicle context.
  - Quotation or approved commercial terms.
  - Trade-In allowance, payoff, and equity information.
  - Credit-bureau responses.
  - Fraud and identity-verification results.
  - Lender underwriting responses.
  - Signed Financial Contract evidence.
  - Funding and reconciliation events.

- **Produces:**
  - Canonical finance-application state.
  - Consent-backed lender-submission snapshots.
  - Credit and affordability assessment context.
  - Lender decision records.
  - Approved finance-offer terms.
  - Customer-selected finance offer.
  - Financial Contract preparation payload.
  - Deal funding status.
  - Funding-reconciliation evidence.
  - Finance conversion and lender-performance analytics.

- **Creates:**
  - Credit-bureau request.
  - Identity-verification request.
  - Document-verification Tasks.
  - Lender submission records.
  - Underwriting-condition Tasks.
  - Customer finance-offer presentation.
  - Financial Contract request.
  - Funding request.
  - Compliance or Human Review Case when required.

- **Triggers:**
  - Consent collection.
  - Document collection.
  - Credit and affordability checks.
  - Lender routing.
  - Underwriting monitoring.
  - Approval-expiry monitoring.
  - Customer offer-selection workflow.
  - Contract-generation workflow.
  - Funding and reconciliation workflow.
  - Deal funding-status updates.
  - Customer communication under permitted consent.

- **Owned By:**
  - The authorized Finance User identified by `assigned_finance_user_id`.
  - Authoritative underwriting decisions remain owned by the applicable lender.
  - Compliance ownership remains with authorized compliance or fraud-review roles.

- **Referenced By:**
  - Customer
  - Opportunity
  - Quotation
  - Deal
  - Vehicle
  - Trade-In
  - Financial Contract
  - Funding Transaction
  - Compliance Case
  - Document Vault
  - Credit Bureau Request
  - Lender Submission
  - Interaction Log
  - Customer Journey
  - AI Agent Run

- **Supersedes / Replaced By:**
  - Material changes after lender submission require a new immutable application version.
  - A new version references the previous version through `supersedes_application_id`.
  - Previous submitted versions remain immutable for audit and lender reconciliation.

## 8. Domain Events

### Emitted Events

- **FinanceApplicationCreated**  
  Payload: `finance_application_id`, `customer_id`, `opportunity_id`, `vehicle_id`, `application_number`, `created_at`

- **FinanceConsentRequested**  
  Payload: `finance_application_id`, `consent_types`, `consent_channel`, `requested_at`

- **FinanceConsentGranted**  
  Payload: `finance_application_id`, `consent_version`, `consent_document_hash`, `consent_captured_at`

- **FinanceConsentDeclined**  
  Payload: `finance_application_id`, `consent_type`, `declined_at`

- **FinanceConsentWithdrawn**  
  Payload: `finance_application_id`, `consent_type`, `consent_withdrawn_at`

- **FinanceDocumentsRequested**  
  Payload: `finance_application_id`, `missing_document_types`, `requested_at`

- **FinanceDocumentsCompleted**  
  Payload: `finance_application_id`, `document_completion_status`, `completed_at`

- **FinanceApplicationReadyForSubmission**  
  Payload: `finance_application_id`, `application_version`, `eligible_lender_ids`, `ready_at`

- **FinanceApplicationSubmitted**  
  Payload: `finance_application_id`, `lender_id`, `application_version`, `lender_application_reference`, `submitted_at`

- **FinanceApplicationSubmissionFailed**  
  Payload: `finance_application_id`, `lender_id`, `error_code`, `failed_at`

- **FinanceCreditReportRequested**  
  Payload: `finance_application_id`, `credit_bureau_provider`, `consent_reference`, `requested_at`

- **FinanceCreditReportReceived**  
  Payload: `finance_application_id`, `credit_bureau_reference`, `credit_bureau_band`, `retrieved_at`

- **FinanceApplicationUnderReview**  
  Payload: `finance_application_id`, `lender_id`, `review_started_at`

- **FinanceApplicationConditionallyApproved**  
  Payload: `finance_application_id`, `lender_id`, `approved_finance_amount`, `outstanding_conditions`, `approval_valid_until`

- **FinanceApplicationApproved**  
  Payload: `finance_application_id`, `lender_id`, `approved_finance_amount`, `approved_interest_rate`, `approved_term_months`, `approval_valid_until`

- **FinanceApplicationDeclined**  
  Payload: `finance_application_id`, `lender_id`, `decision_reason_code`, `declined_at`

- **FinanceOfferPresented**  
  Payload: `finance_application_id`, `lender_id`, `finance_program_id`, `offer_presented_at`

- **FinanceOfferSelected**  
  Payload: `finance_application_id`, `selected_lender_id`, `selected_finance_program_id`, `offer_selected_at`

- **FinanceApplicationContractingStarted**  
  Payload: `finance_application_id`, `financial_contract_id`, `contract_ready_at`

- **FinanceFundingRequested**  
  Payload: `finance_application_id`, `deal_id`, `approved_finance_amount`, `funding_requested_at`

- **FinanceFundingApproved**  
  Payload: `finance_application_id`, `funding_reference`, `funding_approved_at`

- **FinanceApplicationFunded**  
  Payload: `finance_application_id`, `deal_id`, `funded_amount`, `funding_reference`, `funding_received_at`

- **FinanceFundingFailed**  
  Payload: `finance_application_id`, `failure_reason`, `failed_at`

- **FinanceFundingReversed**  
  Payload: `finance_application_id`, `funding_reference`, `reversed_amount`, `reversed_at`

- **FinanceApplicationExpired**  
  Payload: `finance_application_id`, `expired_component`, `expired_at`

- **FinanceApplicationWithdrawn**  
  Payload: `finance_application_id`, `withdrawal_reason`, `withdrawn_at`

- **FinanceApplicationCancelled**  
  Payload: `finance_application_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

### Consumed Events

- **OpportunityFinanceRequested**  
  Creates a Finance Application using Customer, Vehicle, and commercial context.

- **QuotationIssued**  
  Updates or validates the transaction snapshot before lender submission.

- **QuotationSuperseded**  
  Marks the application for commercial revalidation when submitted terms changed materially.

- **VehicleStatusChanged**  
  Revalidates Vehicle eligibility and availability.

- **VehiclePriceUpdated**  
  Recalculates requested finance amount and may require a new application version.

- **TradeInAppraised**  
  Supplies provisional Trade-In values.

- **TradeInPayoffVerified**  
  Supplies verified payoff and net-equity values.

- **CustomerIdentityVerified**  
  Updates identity-verification status.

- **CustomerContactPermissionChanged**  
  Restricts finance-related Customer communication when required.

- **DocumentUploaded**  
  Triggers document classification and verification.

- **DocumentVerificationCompleted**  
  Updates document and verification statuses.

- **CreditBureauResponseReceived**  
  Updates credit and affordability context.

- **LenderDecisionReceived**  
  Updates underwriting decision and approved terms.

- **FinancialContractSigned**  
  Permits progression toward lender funding.

- **DealCreated**  
  Populates `deal_id` and links the selected finance offer to the transaction.

- **DealCancelled**  
  Stops or withdraws active funding activity when required.

- **FundingTransactionCompleted**  
  Populates funding reference, amount, and reconciliation state.

- **FundingTransactionReversed**  
  Reopens funding exception handling and may block Deal delivery or closure.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- Applicant-provided finance-goal summaries
- Finance-product preferences
- Non-sensitive Customer questions
- Document-request explanations
- Underwriting-condition summaries
- Customer offer objections
- Customer decline explanations
- Finance consultation summaries
- Funding-exception summaries
- Non-sensitive operational notes

### Fields Excluded from Embeddings

- `finance_application_id`
- `customer_id`
- `co_applicant_customer_id`
- `opportunity_id`
- `deal_id`
- `vehicle_id`
- `national_identifier_token`
- `date_of_birth`
- `primary_phone`
- `primary_email`
- `current_address`
- `employer_name`
- Bank statements
- Credit reports
- Credit-bureau score
- Credit-bureau reference
- Lender application references
- Loan-account information
- Income documents
- Identity documents
- Consent documents
- Signed Financial Contracts
- Funding references
- `applicant_snapshot`
- `co_applicant_snapshot`
- `consent_snapshot`
- `risk_assessment_snapshot`
- `lender_submission_snapshot`
- `decision_snapshot`
- `funding_snapshot`

> Identity, credit, employment, income, bank, consent, underwriting, and funding data must be supplied only through authorized structured context and must never be placed in unrestricted semantic indexes.

### Structured AI Context Fields

Authorized Finance AI Agents may receive:

- `status`
- `application_type`
- `finance_product_type`
- `document_completion_status`
- `identity_verification_status`
- `employment_verification_status`
- `income_verification_status`
- `affordability_status`
- `decision_status`
- `funding_status`
- `requested_finance_amount`
- `requested_term_months`
- `requested_down_payment_amount`
- `approved_finance_amount`
- `approved_term_months`
- `approved_monthly_payment_amount`
- `approval_valid_until`
- `outstanding_conditions`
- `missing_document_types`

Restricted risk or compliance Agents may additionally receive:

- Tokenized identity references
- Verified income totals
- Monthly commitment totals
- `debt_to_income_ratio`
- `payment_to_income_ratio`
- `loan_to_value_ratio`
- `internal_risk_band`
- `fraud_risk_score`
- Verification results

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `finance_application_id`
- `customer_id`
- `opportunity_id`
- `deal_id`
- `vehicle_id`
- `assigned_finance_user_id`
- `selected_lender_id`
- `status`
- `decision_status`
- `funding_status`
- `application_version`

### Confidence Thresholds

- Legal-name extraction requires confidence of at least `0.99`.
- Date-of-birth extraction requires confidence of at least `0.99`.
- National-identifier extraction requires confidence of at least `0.995`.
- Income-document extraction requires confidence of at least `0.99`.
- Employment-document extraction requires confidence of at least `0.95`.
- Bank-statement transaction extraction requires confidence of at least `0.99`.
- Consent-intent classification requires confidence of at least `0.99`.
- Customer finance-offer acceptance requires confidence of at least `0.99`.
- Document classification requires confidence of at least `0.95`.
- Fraud-risk recommendations require human review regardless of confidence.
- No AI confidence score may replace authoritative lender, credit-bureau, consent, identity, signature, or funding evidence.

### Human Approval Thresholds

- AI Agents cannot provide, grant, withdraw, or renew Customer consent.
- AI Agents cannot approve or decline a Finance Application.
- AI Agents cannot alter lender decisions, rates, terms, fees, conditions, or validity dates.
- AI Agents cannot choose a lender offer on behalf of the Customer.
- AI Agents cannot retrieve credit data without verified consent and permitted scope.
- AI Agents cannot mark a Finance Application `FUNDED`.
- AI Agents cannot override identity, affordability, fraud, compliance, lender, contract, or funding blocks.
- Conflicting applicant, co-applicant, income, consent, credit, lender, or funding evidence must create a Human Review Task.
- AI-generated affordability, approval, and lender-routing suggestions remain advisory.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/finance-applications`

### Methods

- `GET` — list or search Finance Applications.
- `POST` — create a Draft Finance Application.
- `GET /{id}` — retrieve one Finance Application.
- `PATCH /{id}` — update permitted fields before immutable submission.
- `POST /{id}/request-consent` — request mandatory Customer consent.
- `POST /{id}/record-consent` — record verified Customer consent.
- `POST /{id}/withdraw-consent` — record consent withdrawal.
- `POST /{id}/request-documents` — create document requirements.
- `POST /{id}/verify-documents` — process authorized verification results.
- `POST /{id}/check-readiness` — validate submission readiness.
- `POST /{id}/submit` — submit one immutable application version to an authorized lender.
- `POST /{id}/submit-to-additional-lender` — submit under valid consent and lender-routing policy.
- `POST /{id}/record-decision` — process an authenticated lender decision.
- `POST /{id}/present-offer` — present an approved offer to the Customer.
- `POST /{id}/select-offer` — record verified Customer selection.
- `POST /{id}/start-contracting` — create or link the Financial Contract.
- `POST /{id}/request-funding` — submit a controlled lender-funding request.
- `POST /{id}/reconcile-funding` — reconcile authoritative funding evidence.
- `POST /{id}/withdraw` — record Customer withdrawal.
- `POST /{id}/cancel` — cancel an eligible application.
- `POST /{id}/create-version` — create a new application version after material change.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateFinanceApplicationRequest",
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
    "quotation_id": {
      "type": ["string", "null"],
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
    "assigned_finance_user_id": {
      "type": "string",
      "format": "uuid"
    },
    "co_applicant_customer_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "application_type": {
      "type": "string",
      "enum": [
        "INDIVIDUAL",
        "JOINT",
        "CORPORATE",
        "SOLE_PROPRIETOR",
        "FLEET"
      ]
    },
    "finance_product_type": {
      "type": "string",
      "enum": [
        "RETAIL_FINANCE",
        "VEHICLE_LOAN",
        "HIRE_PURCHASE",
        "LEASE",
        "BALLOON_FINANCE",
        "ISLAMIC_FINANCE",
        "CORPORATE_FINANCE",
        "FLEET_FINANCE",
        "REFINANCE",
        "OTHER"
      ]
    },
    "submission_channel": {
      "type": "string",
      "enum": [
        "DEALERSHIP",
        "DIGITAL",
        "PHONE",
        "WHATSAPP",
        "LENDER_PORTAL",
        "OEM_PLATFORM",
        "API_INTEGRATION",
        "OTHER"
      ]
    },
    "legal_name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 200
    },
    "date_of_birth": {
      "type": "string",
      "format": "date"
    },
    "residency_status": {
      "type": "string",
      "enum": [
        "UNKNOWN",
        "CITIZEN",
        "PERMANENT_RESIDENT",
        "RESIDENT",
        "TEMPORARY_RESIDENT",
        "NON_RESIDENT"
      ]
    },
    "employment_status": {
      "type": "string",
      "enum": [
        "UNKNOWN",
        "EMPLOYED",
        "SELF_EMPLOYED",
        "BUSINESS_OWNER",
        "CONTRACTOR",
        "RETIRED",
        "STUDENT",
        "UNEMPLOYED",
        "OTHER"
      ]
    },
    "gross_monthly_income_amount": {
      "type": "number",
      "minimum": 0
    },
    "net_monthly_income_amount": {
      "type": "number",
      "minimum": 0
    },
    "other_monthly_income_amount": {
      "type": "number",
      "minimum": 0
    },
    "monthly_housing_cost_amount": {
      "type": "number",
      "minimum": 0
    },
    "monthly_debt_payment_amount": {
      "type": "number",
      "minimum": 0
    },
    "requested_down_payment_amount": {
      "type": "number",
      "minimum": 0
    },
    "finance_amount_requested": {
      "type": "number",
      "minimum": 0
    },
    "requested_term_months": {
      "type": "integer",
      "minimum": 1
    },
    "preferred_monthly_payment_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "maximum_monthly_payment_amount": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    }
  },
  "required": [
    "customer_id",
    "opportunity_id",
    "vehicle_id",
    "assigned_finance_user_id",
    "application_type",
    "finance_product_type",
    "submission_channel",
    "legal_name",
    "date_of_birth",
    "residency_status",
    "employment_status",
    "gross_monthly_income_amount",
    "net_monthly_income_amount",
    "other_monthly_income_amount",
    "monthly_housing_cost_amount",
    "monthly_debt_payment_amount",
    "requested_down_payment_amount",
    "finance_amount_requested",
    "requested_term_months",
    "currency_code"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type FinanceApplication {
  id: ID!
  dealershipId: ID!
  customerId: ID!
  opportunityId: ID!
  quotationId: ID
  dealId: ID
  vehicleId: ID!
  tradeInId: ID
  assignedFinanceUserId: ID!
  coApplicantCustomerId: ID
  selectedLenderId: ID
  selectedFinanceProgramId: ID
  financialContractId: ID
  complianceCaseId: ID
  fundingTransactionId: ID
  applicationNumber: String!
  applicationType: FinanceApplicationType!
  financeProductType: FinanceProductType!
  status: FinanceApplicationStatus!
  submissionChannel: FinanceSubmissionChannel!
  applicationVersion: Int!
  isCurrentVersion: Boolean!
  residencyStatus: ResidencyStatus!
  employmentStatus: EmploymentStatus!
  grossMonthlyIncomeAmount: Float!
  netMonthlyIncomeAmount: Float!
  otherMonthlyIncomeAmount: Float!
  totalVerifiedMonthlyIncome: Float!
  monthlyHousingCostAmount: Float!
  monthlyDebtPaymentAmount: Float!
  totalMonthlyCommitments: Float!
  requestedDownPaymentAmount: Float!
  financeAmountRequested: Float!
  requestedTermMonths: Int!
  preferredMonthlyPaymentAmount: Float
  maximumMonthlyPaymentAmount: Float
  currencyCode: String!
  creditCheckConsentStatus: FinanceConsentStatus!
  dataProcessingConsentStatus: FinanceConsentStatus!
  lenderSharingConsentStatus: FinanceConsentStatus!
  identityVerificationStatus: VerificationStatus!
  employmentVerificationStatus: VerificationStatus!
  incomeVerificationStatus: VerificationStatus!
  documentCompletionStatus: DocumentCompletionStatus!
  creditBureauRequestStatus: CreditBureauRequestStatus!
  creditBureauBand: String
  debtToIncomeRatio: Float
  paymentToIncomeRatio: Float
  loanToValueRatio: Float
  affordabilityStatus: AffordabilityStatus!
  decisionStatus: FinanceDecisionStatus!
  approvedFinanceAmount: Float
  approvedDownPaymentAmount: Float
  approvedInterestRate: Float
  approvedAnnualPercentageRate: Float
  approvedTermMonths: Int
  approvedMonthlyPaymentAmount: Float
  approvalValidUntil: DateTime
  outstandingConditions: [String!]!
  customerDecision: CustomerFinanceDecision!
  fundingStatus: FinanceFundingStatus!
  fundedAmount: Float!
  consentCapturedAt: DateTime
  submittedAt: DateTime
  decisionAt: DateTime
  offerPresentedAt: DateTime
  offerSelectedAt: DateTime
  fundingRequestedAt: DateTime
  fundingReceivedAt: DateTime
  withdrawnAt: DateTime
  declinedAt: DateTime
  cancelledAt: DateTime
  expiredAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `finance_applications`
- **Applicant Table:** `finance_application_applicants`
- **Consent Table:** `finance_application_consents`
- **Document Table:** `finance_application_documents`
- **Verification Table:** `finance_application_verifications`
- **Credit-Bureau Table:** `finance_credit_bureau_requests`
- **Risk-Assessment Table:** `finance_risk_assessments`
- **Lender-Submission Table:** `finance_lender_submissions`
- **Lender-Decision Table:** `finance_lender_decisions`
- **Underwriting-Condition Table:** `finance_underwriting_conditions`
- **Offer Table:** `finance_application_offers`
- **Funding Table:** `finance_funding_transactions`
- **Version-History Table:** `finance_application_versions`
- **Status-History Table:** `finance_application_status_history`
- **Audit Table:** `finance_application_audit_log`

### Indexes

- `idx_finance_applications_tenant_status (dealership_id, status)`  
  Used for operational finance queues.

- `idx_finance_applications_customer (dealership_id, customer_id, created_at DESC)`  
  Used for Customer finance history.

- `idx_finance_applications_opportunity (dealership_id, opportunity_id, status)`  
  Used to retrieve applications linked to an Opportunity.

- `idx_finance_applications_deal (deal_id)`  
  Used for Deal-funding validation.

- `idx_finance_applications_vehicle (dealership_id, vehicle_id, status)`  
  Used to revalidate Vehicle eligibility.

- `idx_finance_applications_finance_user (dealership_id, assigned_finance_user_id, status)`  
  Used for Finance User work queues.

- `idx_finance_applications_decision (dealership_id, decision_status, decision_at)`  
  Used for underwriting and approval monitoring.

- `idx_finance_applications_approval_expiry (dealership_id, approval_valid_until, status)`  
  Used by approval-expiry Jobs.

- `idx_finance_applications_funding (dealership_id, funding_status, funding_requested_at)`  
  Used for funding and reconciliation queues.

- `idx_finance_applications_lender (dealership_id, selected_lender_id, decision_status)`  
  Used for lender-performance and decision reporting.

- `idx_finance_applications_number (dealership_id, application_number)`  
  Used for human-readable lookup.

- `idx_finance_applications_current_version (dealership_id, opportunity_id, is_current_version)`  
  Used to identify the active application version.

### Unique Constraints

- `UQ_finance_application_number (dealership_id, application_number)`

- `UQ_finance_application_series_version (dealership_id, opportunity_id, application_version)`

- `UQ_current_finance_application_version (dealership_id, opportunity_id) WHERE is_current_version = true`

- `UQ_lender_application_reference (selected_lender_id, lender_application_reference)`  
  Applies when `lender_application_reference` is not null.

- `UQ_financial_contract_application (financial_contract_id)`  
  Applies when `financial_contract_id` is not null.

- `UQ_funding_transaction_application (funding_transaction_id)`  
  Applies when `funding_transaction_id` is not null.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)`
- `opportunity_id` → `opportunities(id)`
- `quotation_id` → `quotations(id)` — nullable
- `deal_id` → `deals(id)` — nullable
- `vehicle_id` → `vehicles(id)`
- `trade_in_id` → `trade_ins(id)` — nullable
- `assigned_finance_user_id` → `users(id)`
- `co_applicant_customer_id` → `customers(id)` — nullable
- `selected_lender_id` → `lenders(id)` — nullable
- `selected_finance_program_id` → `finance_programs(id)` — nullable
- `financial_contract_id` → `financial_contracts(id)` — nullable
- `compliance_case_id` → `compliance_cases(id)` — nullable
- `funding_transaction_id` → `finance_funding_transactions(id)` — nullable
- `supersedes_application_id` → `finance_applications(id)` — nullable
- `created_by` → `users(id)`
- `submitted_by` → `users(id)` — nullable
- `reviewed_by` → `users(id)` — nullable

### Database Constraints

- `application_version >= 1`
- `dependents_count >= 0`
- `gross_monthly_income_amount >= 0`
- `net_monthly_income_amount >= 0`
- `other_monthly_income_amount >= 0`
- `monthly_housing_cost_amount >= 0`
- `monthly_debt_payment_amount >= 0`
- `requested_down_payment_amount >= 0`
- `finance_amount_requested >= 0`
- `requested_term_months > 0`
- `maximum_monthly_payment_amount >= preferred_monthly_payment_amount`
- `debt_to_income_ratio >= 0`
- `payment_to_income_ratio >= 0`
- `loan_to_value_ratio >= 0`
- `fraud_risk_score BETWEEN 0.00 AND 1.00`
- `approval_valid_until > decision_at`
- `funding_received_at >= funding_requested_at`
- `funded_amount <= approved_finance_amount` unless an approved adjustment exists.
- `co_applicant_customer_id != customer_id`
- `consent_document_hash IS NOT NULL` before lender submission.
- `financial_contract_id IS NOT NULL` before funding when a Financial Contract is required.
- `funding_reference IS NOT NULL` when `status = FUNDED`.
- Submitted application, consent, lender-submission, and decision snapshots must be immutable.
- Circular application-version relationships are prohibited.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical applications by `created_at`.
- Applicant, consent, verification, lender, decision, offer, funding, history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Limited read access to finance progress and Customer-visible approved terms for assigned Opportunities.
- **Finance User:** Create, review, prepare, and submit Finance Applications within assigned dealership scope.
- **Finance Manager:** Oversight, permitted manual-review decisions, lender routing, and funding reconciliation.
- **Compliance User:** Access to identity, consent, fraud, residency, and document-verification evidence.
- **Sales Manager:** Read access to finance status and approved commercial implications without unrestricted credit-report access.
- **Accounting User:** Access to funded amounts, reconciliation, and authorized financial postings.
- **Customer Self-Service User:** Access only to their own permitted application, consent, document, and selected-offer actions.
- **AI Finance Agent:** Service Account access limited to document classification, completeness checks, summaries, recommendations, and approved workflow requests.
- **Lender Integration Service:** Restricted submission, decision, and funding-event access using tenant-scoped credentials.
- **Credit Bureau Integration Service:** Restricted consent-validated request and response access.
- **Document Verification Service:** Restricted access to required document-processing operations.

### PII Classification

- **Level:** `RESTRICTED FINANCIAL PII`

The Finance Application may contain or reference:

- Legal name
- Date of birth
- National identifiers
- Phone number
- Email address
- Residential address
- Employment information
- Employer information
- Income information
- Bank statements
- Existing financial obligations
- Credit-bureau information
- Identity documents
- Consent evidence
- Applicant and co-applicant information
- Lender decisions
- Signed Financial Contracts

### Financially Sensitive Fields

- `gross_monthly_income_amount`
- `net_monthly_income_amount`
- `other_monthly_income_amount`
- `monthly_housing_cost_amount`
- `monthly_debt_payment_amount`
- `monthly_other_commitment_amount`
- `declared_assets_amount`
- `declared_liabilities_amount`
- `requested_down_payment_amount`
- `finance_amount_requested`
- `approved_finance_amount`
- `approved_interest_rate`
- `approved_annual_percentage_rate`
- `approved_monthly_payment_amount`
- `credit_bureau_score`
- `debt_to_income_ratio`
- `payment_to_income_ratio`
- `loan_to_value_ratio`
- `fraud_risk_score`
- `funded_amount`

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, documents, snapshots, event stores, and backups.
- **Column-Level Protection:** National identifiers, date of birth, addresses, phone numbers, email addresses, employer details, income values, credit data, lender references, consent evidence, and funding references require encryption, tokenization, or equivalent approved protection.
- Raw national identifiers must be tokenized or stored only in a dedicated regulated vault.
- Credit reports, bank statements, identity documents, consent artifacts, and signed Financial Contracts must be stored in an encrypted Document Vault.
- External integration credentials and access tokens must be stored in a secrets-management system.
- Encryption keys must be separated by environment and rotated according to security policy.

### Consent and Purpose Limitation

- Credit information may be requested only under valid, current, and verifiable consent.
- Lender sharing must remain limited to lenders and purposes covered by Customer consent.
- Consent withdrawal must prevent future unauthorized processing.
- Data collected for finance underwriting must not be reused for unrelated marketing without a separate lawful permission.
- Co-applicant data requires separate consent and authorization.
- Consent records must preserve:
  - Consent version
  - Purpose
  - Data categories
  - Permitted recipients
  - Channel
  - Customer evidence
  - Timestamp
  - Withdrawal status

### Audit Requirements

- Every applicant-data change must record:
  - Previous value or protected change reference
  - New value or protected change reference
  - Actor
  - Source
  - Timestamp
  - Reason

- Every consent operation must preserve:
  - Consent type
  - Consent version
  - Customer identity evidence
  - Channel
  - Document hash
  - Granted, declined, or withdrawn status
  - Timestamp

- Every credit-bureau request must preserve:
  - Consent reference
  - Provider
  - Purpose
  - Requesting actor or service
  - Request timestamp
  - Response reference
  - Response timestamp

- Every lender submission must preserve:
  - Exact application version
  - Immutable submission snapshot
  - Lender
  - Finance program
  - Correlation ID
  - Submission actor
  - Submission timestamp
  - Response hash

- Every lender decision must preserve:
  - Decision source
  - Decision version
  - Approved or declined terms
  - Conditions
  - Validity period
  - Response hash
  - Timestamp

- Customer offer selection must preserve:
  - Selected lender
  - Selected program
  - Exact approved offer
  - Required disclosures
  - Customer acceptance evidence
  - Timestamp

- Every funding operation must preserve:
  - Funding request
  - Lender or banking reference
  - Amount
  - Currency
  - Reconciliation result
  - Actor or service
  - Timestamp

- Human overrides of AI recommendations must retain both the original AI recommendation and the final human decision.
- Access to identity, income, credit, consent, lender, contract, and funding information must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, co-applicant, Opportunity, Quotation, Vehicle, Trade-In, Deal, lender submission, Financial Contract, funding, or Finance Application linking is prohibited.
- AI Agents, Jobs, lender integrations, credit-bureau integrations, document processors, exports, and analytics must receive tenant scope through signed execution context.
- Lender responses must be mapped to exactly one authenticated tenant before processing.
- Finance documents and Customer-facing links must be tenant-scoped, time-limited, and cryptographically protected.

### Retention and Deletion

- Finance Applications must follow applicable credit, privacy, financial, contractual, audit, and regulatory retention requirements.
- Submitted applications, lender decisions, consent records, signed contracts, and funding records must remain immutable.
- Hard deletion is prohibited while a Finance Application is linked to:
  - A Deal
  - A Financial Contract
  - A lender decision
  - A credit-bureau request
  - A funding transaction
  - A compliance case
  - An audit record

- Soft deletion is the operational default for eligible Draft or abandoned records.
- Legally approved deletion requests must purge or anonymize permitted PII while preserving records that must legally remain.
- Deletion and anonymization must address:
  - Finance Application records
  - Applicant and co-applicant snapshots
  - Consent artifacts
  - Identity and income documents
  - Credit and risk data
  - Lender submissions and responses
  - Offer-selection evidence
  - Funding records
  - Vector stores
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
