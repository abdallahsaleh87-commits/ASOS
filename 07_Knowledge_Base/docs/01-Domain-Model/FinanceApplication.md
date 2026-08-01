# Finance Application

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Finance Application Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Finance Application Object represents one governed request by one or more Applicants for an automotive finance, lease, or another approved credit product associated with a specific Customer commercial journey.

It provides a controlled process for:

- Creating the finance request.
- Identifying the primary Applicant.
- Identifying Co-Applicants, Guarantors, or authorized business representatives.
- Capturing purpose-specific Consent and authorization.
- Collecting required personal, employment, income, expense, asset, liability, and business information.
- Verifying identity and supporting documents.
- Obtaining authorized credit-bureau information.
- Calculating deterministic affordability and transaction ratios.
- Preparing immutable application versions.
- Submitting applications to approved Lenders.
- Receiving authoritative Lender Decisions.
- Tracking underwriting conditions.
- Comparing approved Lender offers.
- Presenting permitted offers to the Customer.
- Recording Customer selection or rejection.
- Supporting Financial Contract preparation.
- Monitoring funding requirements.
- Projecting authoritative funding Confirmation.
- Supporting Quotation and Deal workflows.
- Preserving regulatory, Consent, Decision, and audit evidence.

### Finance Application Domain Boundary

The Finance Application represents the credit-request, submission, underwriting, Decision-selection, and funding-readiness workflow.

It does not independently represent:

- Guaranteed finance approval.
- A dealership-created credit Decision.
- A signed Financial Contract.
- Cleared Customer Payment.
- Cleared Lender funding.
- A completed Deal.
- A confirmed Vehicle sale.
- A confirmed Vehicle delivery.
- A final accounting entry.

The following separation must remain explicit:

```text
Finance Application
  = Applicant information, Consent, verification, Lender submissions,
    underwriting Decisions, selected offer, conditions, and funding readiness

Financial Contract
  = authoritative contractual terms and signature workflow

Deal
  = governed automotive commercial transaction

Payment
  = Customer or third-party payment transaction and settlement evidence

External Lender or Funding Authority
  = authoritative credit Decision and funding outcome
```

### Finance Application and Quotation Separation

`Quotation` represents the Customer-facing automotive commercial offer.

`Finance Application` represents the formal request for finance based on an approved commercial scenario.

The Finance Application may preserve a versioned commercial snapshot containing:

- Vehicle price.
- Optional products.
- Taxes.
- Fees.
- Trade-In allowance.
- Trade-In payoff.
- Customer cash contribution.
- Requested finance amount.
- Requested term.
- Requested payment structure.

The Finance Application must not silently modify the Quotation.

Material commercial changes may require:

- A new Quotation version.
- A new Finance Application version.
- A revised Lender submission.
- Lender re-underwriting.
- Customer reconfirmation.
- Financial Contract revision.

### Finance Application and Lender Submission Separation

One Finance Application may produce multiple governed Lender submissions.

Each Lender submission must reference:

- One exact immutable Finance Application version.
- One approved Lender.
- One approved finance program where applicable.
- One exact commercial snapshot.
- One Consent and authorization snapshot.
- One document and verification snapshot.
- One submission idempotency key.
- One submission timestamp.
- One external Lender reference.
- All subsequent Lender responses and Decisions.

A retry must not create a duplicate Lender application.

A materially changed request must not overwrite a prior submitted version.

### Finance Application and Lender Decision Separation

A Lender Decision is an External Authoritative outcome.

ASOS and dealership Users may:

- Receive it.
- Validate its integrity.
- Normalize it.
- Display it to authorized Users.
- Compare eligible offers.
- Track its conditions and expiration.
- Request clarification or reconsideration where permitted.

ASOS, dealership Users, and AI Agents must not:

- Convert a decline into an approval.
- Alter approved terms.
- Remove Lender conditions.
- Extend approval validity without Lender authority.
- Represent a pending submission as approved.
- Represent prequalification as final approval.
- Represent predicted approval as an authoritative Decision.

### Finance Application and Financial Contract Separation

Customer selection of a Lender offer does not create a signed contract.

The Financial Contract workflow must preserve:

- Selected Lender Decision.
- Selected finance program.
- Approved terms.
- Required disclosures.
- Contract version.
- Signatories.
- Signature evidence.
- Contract activation state.

The Finance Application may reference the resulting Financial Contract.

It must not treat contract preparation as contract execution.

### Finance Application and Funding Separation

Finance approval does not guarantee funding.

Funding may require:

- Signed Financial Contract.
- Customer down payment.
- Vehicle and Deal eligibility.
- Registration or title evidence.
- Insurance evidence.
- Required Lender conditions.
- Original documents.
- Lender verification.
- Dealership invoice.
- Delivery controls.
- Another Lender-specific requirement.

A funding request, API acknowledgment, or payment instruction does not prove that funds were received.

`FUNDED` requires authoritative funding evidence and reconciliation.

### Application Versioning

The Finance Application is a continuing governed workflow.

Each submitted application version is an immutable snapshot.

A new version is required when material submitted information changes, including:

- Applicant identity.
- Co-Applicant or Guarantor.
- Income.
- Employment.
- Residence.
- Declared debts or commitments.
- Requested finance amount.
- Down payment.
- Vehicle.
- Inventory Record.
- Quotation.
- Trade-In equity.
- Finance product.
- Requested term.
- Material Consent scope.
- Required documents.
- Another Lender-relevant underwriting input.

Every application version must preserve:

- Version number.
- Applicant snapshot.
- Commercial snapshot.
- Consent snapshot.
- Verification snapshot.
- Document snapshot.
- Affordability snapshot.
- Submission purpose.
- Source record versions.
- Integrity hash.
- Creation authority.
- Effective timestamp.

### Applicant and Customer Separation

`Customer` represents the canonical individual or organization.

An Applicant represents a Customer or authorized party participating in one Finance Application.

A Finance Application may include:

- Primary Applicant.
- Co-Applicant.
- Guarantor.
- Corporate Applicant.
- Authorized company signatory.
- Beneficial owner where lawfully required.
- Another approved party role.

Applicant financial information must not become unrestricted Customer-profile information.

Finance-specific data must remain purpose-limited and access-controlled.

### Consent and Legal Basis

Consent and authorization must be:

- Applicant-specific.
- Purpose-specific.
- Recipient-specific where required.
- Channel-specific where applicable.
- Versioned.
- Time-stamped.
- Supported by evidence.
- Revocable where applicable.
- Evaluated under applicable law and Tenant policy.

Consent to:

- Process a Finance Application.
- Obtain a credit report.
- Share data with a Lender.
- Use electronic communication.
- Use electronic signature.

must remain distinguishable.

Consent for finance processing must not automatically create general marketing Consent.

Consent withdrawal must stop future processing that no longer has a valid lawful basis.

It does not automatically erase legally required historical processing or completed submissions.

### System Purpose

The Finance Application Object provides canonical finance-request context to:

- Customer workflows.
- Opportunity workflows.
- Quotation workflows.
- Vehicle and Inventory workflows.
- Trade-In workflows.
- Appointment workflows.
- Deal workflows.
- Financial Contract workflows.
- Payment and funding reconciliation.
- Lender integrations.
- Credit-bureau integrations.
- Identity-verification services.
- Document services.
- Compliance workflows.
- AI Agents.
- Analytics.
- Audit and regulatory reporting.

The Finance Application Object may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Customer identity | Customer Domain Service and approved identity source |
| Applicant-submitted information | Applicant or approved intake source |
| Identity verification | Approved identity-verification authority |
| Employment and income evidence | Approved documents or verification provider |
| Credit-bureau report and score | Authorized Credit Bureau |
| Consent evidence | Applicant action and approved Consent authority |
| Quotation commercial terms | Quotation Domain Service |
| Vehicle identity | Vehicle Domain Service |
| Inventory eligibility | Inventory Domain Service or configured external authority |
| Trade-In value and payoff | Trade-In Domain Service and approved external sources |
| Deterministic affordability calculations | Approved Calculation Service |
| Lender underwriting Decision | Lender |
| Approved finance terms | Lender Decision |
| Customer offer selection | Customer evidence |
| Financial Contract | Financial Contract Domain Service and signature provider |
| Funding outcome | Lender, bank, or configured funding authority |
| Deal status | Deal Domain Service and configured external authority |
| Predictions and Recommendations | Derived Intelligence |
| Canonical Finance Application workflow | Finance Application Domain Service |
| External operation completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `finance_application_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `current_application_version` — Integer, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `finance_department_id`.
- `finance_team_id`.
- `assigned_finance_user_id`.
- `responsible_manager_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `customer_id`.
- `opportunity_id`.
- `quotation_id`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `appointment_id`.
- `deal_id`.
- `financial_contract_id`.
- `primary_interaction_id`.
- `compliance_case_id`.
- `funding_transaction_reference`.

### Application Identity

- `application_number`.
- `application_type`.
- `finance_product_type`.
- `status`.
- `priority`.
- `submission_channel`.
- `workflow_authority_mode`.
- `current_application_version`.
- `current_submission_count`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Applicant Parties

- `primary_applicant_id`.
- `applicant_ids`.
- `co_applicant_ids`.
- `guarantor_ids`.
- `corporate_signatory_ids`.
- `beneficial_owner_ids`.
- `applicant_count`.
- `applicant_structure_status`.

Each Applicant must be represented through a governed child record.

### Applicant Record

Each Applicant record may contain:

- `finance_applicant_id`.
- `customer_id`.
- `applicant_role`.
- `applicant_type`.
- `legal_name_projection`.
- `date_of_birth_projection`.
- `national_identifier_token`.
- `national_identifier_type`.
- `nationality_projection`.
- `residency_status`.
- `marital_status`.
- `dependents_count`.
- `preferred_language_projection`.
- `primary_phone_reference`.
- `primary_email_reference`.
- `current_address_reference`.
- `residence_type`.
- `months_at_current_address`.
- `previous_address_reference`.
- `identity_verification_status`.
- `applicant_status`.

Raw direct identifiers must not be stored in unrestricted application fields.

### Corporate Applicant Context

- `organization_customer_id`.
- `legal_business_name_projection`.
- `registration_number_token`.
- `tax_identifier_token`.
- `business_type`.
- `incorporation_date`.
- `incorporation_country`.
- `registered_address_reference`.
- `operating_address_reference`.
- `industry_code`.
- `business_activity`.
- `annual_revenue_amount`.
- `years_in_business`.
- `authorized_signatory_status`.
- `beneficial_ownership_status`.
- `corporate_document_status`.

### Employment and Income

Per applicable Applicant:

- `employment_status`.
- `employer_name`.
- `employer_identifier_reference`.
- `job_title`.
- `employment_start_date`.
- `months_with_employer`.
- `employment_type`.
- `gross_monthly_income_amount`.
- `net_monthly_income_amount`.
- `other_monthly_income_amount`.
- `other_income_source`.
- `income_currency_code`.
- `income_frequency`.
- `declared_income_amount`.
- `verified_income_amount`.
- `income_verification_status`.
- `employment_verification_status`.
- `income_evidence_references`.
- `employment_evidence_references`.

Declared and verified income must remain separate.

### Self-Employment and Business Income

- `business_name`.
- `business_registration_reference`.
- `business_ownership_percentage`.
- `business_start_date`.
- `average_monthly_business_income_amount`.
- `verified_business_income_amount`.
- `business_expense_amount`.
- `business_bank_statement_status`.
- `business_financial_statement_status`.
- `business_income_verification_status`.
- `business_evidence_references`.

### Financial Position

- `monthly_housing_cost_amount`.
- `monthly_debt_payment_amount`.
- `monthly_other_commitment_amount`.
- `monthly_alimony_or_support_amount`.
- `monthly_insurance_commitment_amount`.
- `declared_assets_amount`.
- `verified_assets_amount`.
- `declared_liabilities_amount`.
- `verified_liabilities_amount`.
- `available_down_payment_amount`.
- `source_of_down_payment`.
- `source_of_funds_status`.
- `financial_position_currency_code`.
- `financial_position_evidence_references`.

### Commercial Transaction Snapshot

- `quotation_id`.
- `quotation_version`.
- `quotation_document_hash`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `vehicle_snapshot`.
- `inventory_snapshot`.
- `commercial_snapshot`.
- `vehicle_selling_price_amount`.
- `optional_products_amount`.
- `tax_amount`.
- `fee_amount`.
- `trade_in_allowance_amount`.
- `trade_in_payoff_amount`.
- `trade_in_positive_equity_amount`.
- `trade_in_negative_equity_amount`.
- `customer_cash_contribution_amount`.
- `requested_down_payment_amount`.
- `deposit_amount`.
- `requested_finance_amount`.
- `requested_term_months`.
- `requested_balloon_payment_amount`.
- `preferred_periodic_payment_amount`.
- `maximum_periodic_payment_amount`.
- `payment_frequency`.
- `currency_code`.

### Application Versions

- `application_version_ids`.
- `current_application_version`.
- `latest_draft_version`.
- `latest_submitted_version`.
- `version_creation_reason`.
- `version_snapshot_hash`.
- `version_created_at`.
- `version_created_by`.
- `supersedes_application_version`.
- `material_change_reasons`.

Each application version must preserve immutable snapshots of:

- Applicants.
- Employment.
- Income.
- Financial position.
- Commercial transaction.
- Consent.
- Verification.
- Documents.
- Affordability calculations.
- Source-record versions.

### Consent and Authorization

Per Applicant and purpose:

- `finance_consent_record_ids`.
- `application_processing_authorization_status`.
- `credit_bureau_consent_status`.
- `lender_sharing_consent_status`.
- `identity_verification_consent_status`.
- `document_verification_consent_status`.
- `electronic_communication_consent_status`.
- `electronic_signature_consent_status`.
- `consent_policy_id`.
- `consent_policy_version`.
- `consent_artifact_reference`.
- `consent_artifact_hash`.
- `consent_captured_at`.
- `consent_effective_from`.
- `consent_expires_at`.
- `consent_withdrawn_at`.
- `consent_withdrawal_reason`.
- `consent_revalidation_required`.

### Identity and Verification

- `identity_verification_status`.
- `address_verification_status`.
- `residency_verification_status`.
- `employment_verification_status`.
- `income_verification_status`.
- `bank_statement_verification_status`.
- `source_of_funds_verification_status`.
- `business_verification_status`.
- `beneficial_ownership_verification_status`.
- `fraud_verification_status`.
- `verification_provider_references`.
- `verification_evidence_references`.
- `verification_completed_at`.
- `verification_expires_at`.
- `verification_conflict_references`.

### Document Management

- `document_requirement_set_id`.
- `required_document_types`.
- `received_document_types`.
- `missing_document_types`.
- `rejected_document_types`.
- `expired_document_types`.
- `document_completion_status`.
- `document_verification_status`.
- `document_record_ids`.
- `document_snapshot`.
- `document_snapshot_hash`.
- `document_last_updated_at`.

Raw documents must remain in controlled document storage.

### Credit-Bureau Context

- `credit_bureau_request_ids`.
- `credit_bureau_request_status`.
- `credit_bureau_provider`.
- `credit_bureau_reference`.
- `credit_report_reference`.
- `credit_report_hash`.
- `credit_report_retrieved_at`.
- `credit_report_expires_at`.
- `credit_score_projection`.
- `credit_score_band_projection`.
- `credit_report_dispute_status`.
- `credit_bureau_consent_record_id`.

Credit-bureau information must remain purpose-limited and jurisdiction-controlled.

### Deterministic Affordability and Ratios

- `total_declared_monthly_income_amount`.
- `total_verified_monthly_income_amount`.
- `total_monthly_commitments_amount`.
- `disposable_monthly_income_amount`.
- `requested_payment_amount`.
- `debt_to_income_ratio`.
- `payment_to_income_ratio`.
- `loan_to_value_ratio`.
- `down_payment_percentage`.
- `affordability_status`.
- `affordability_rule_id`.
- `affordability_rule_version`.
- `affordability_calculation_reference`.
- `affordability_snapshot`.
- `affordability_snapshot_hash`.
- `affordability_calculated_at`.
- `affordability_expires_at`.

Thresholds must remain configurable by approved policy and Lender program.

### Internal Risk and Compliance Projection

- `internal_risk_status`.
- `fraud_risk_status`.
- `identity_risk_status`.
- `document_risk_status`.
- `affordability_risk_status`.
- `compliance_status`.
- `sanctions_review_status`.
- `politically_exposed_person_review_status`.
- `anti_money_laundering_review_status`.
- `source_of_funds_review_status`.
- `risk_review_required`.
- `risk_review_reason_codes`.
- `risk_case_id`.

These projections must not replace authoritative Lender underwriting.

### Lender Eligibility

- `eligible_lender_ids`.
- `eligible_finance_program_ids`.
- `ineligible_lender_reasons`.
- `lender_eligibility_status`.
- `lender_eligibility_rule_id`.
- `lender_eligibility_rule_version`.
- `lender_selection_strategy`.
- `lender_selection_decision_id`.

Eligibility to submit does not predict approval.

### Lender Submissions

- `lender_submission_ids`.
- `lender_submission_count`.
- `active_lender_submission_count`.
- `last_submitted_at`.
- `latest_submission_status`.
- `selected_lender_submission_id`.

Each Lender submission must contain:

- `lender_submission_id`.
- `lender_id`.
- `finance_program_id`.
- `application_version`.
- `application_version_hash`.
- `submission_status`.
- `submission_requested_at`.
- `submission_command_id`.
- `submission_idempotency_key`.
- `submitted_at`.
- `lender_application_reference`.
- `external_confirmation_status`.
- `external_confirmation_reference`.
- `response_received_at`.
- `reconciliation_status`.
- `failure_reason`.

### Lender Decision

Each Lender Decision must contain:

- `lender_decision_id`.
- `lender_submission_id`.
- `lender_id`.
- `finance_program_id`.
- `decision_version`.
- `decision_status`.
- `decision_received_at`.
- `decision_effective_at`.
- `decision_valid_until`.
- `decision_reason_codes`.
- `decision_details_reference`.
- `approved_finance_amount`.
- `approved_down_payment_amount`.
- `approved_interest_rate`.
- `approved_annual_percentage_rate`.
- `approved_term_months`.
- `approved_periodic_payment_amount`.
- `approved_payment_frequency`.
- `approved_balloon_payment_amount`.
- `approved_residual_value_amount`.
- `approved_fees_amount`.
- `approved_total_cost_amount`.
- `approved_vehicle_reference`.
- `approved_conditions`.
- `outstanding_conditions`.
- `condition_satisfaction_status`.
- `decision_artifact_reference`.
- `decision_artifact_hash`.
- `decision_source_reference`.
- `decision_confirmation_status`.

A Lender Decision must be stored as received.

Normalization must not change its legal meaning.

### Underwriting Conditions

- `underwriting_condition_ids`.
- `condition_count`.
- `outstanding_condition_count`.
- `condition_status`.
- `conditions_due_at`.
- `conditions_satisfied_at`.
- `conditions_expired_at`.

Each condition must preserve:

- `underwriting_condition_id`.
- `lender_decision_id`.
- `condition_type`.
- `condition_description`.
- `required_evidence`.
- `status`.
- `satisfaction_evidence_references`.
- `satisfied_at`.
- `confirmed_by_lender_at`.
- `external_confirmation_status`.

Dealership evidence submission does not prove that the Lender accepted the condition.

### Lender Offer Presentation

- `presentable_offer_ids`.
- `offer_comparison_status`.
- `offer_presentation_status`.
- `offer_presented_at`.
- `offer_presentation_interaction_id`.
- `offer_document_references`.
- `offer_disclosure_references`.
- `offer_expiration_status`.

Only currently valid and permitted Lender offers may be presented.

### Customer Selection

- `customer_decision_status`.
- `selected_lender_decision_id`.
- `selected_lender_id`.
- `selected_finance_program_id`.
- `selected_offer_version`.
- `selected_offer_snapshot`.
- `selected_offer_snapshot_hash`.
- `customer_selection_requested_at`.
- `customer_selected_at`.
- `customer_selection_channel`.
- `customer_selection_evidence_references`.
- `customer_rejected_at`.
- `customer_rejection_reason`.
- `customer_requested_changes`.

Customer selection must reference one exact authoritative Lender Decision version.

### Contracting Handoff

- `contract_readiness_status`.
- `contract_preparation_status`.
- `financial_contract_id`.
- `contract_creation_requested_at`.
- `contract_creation_command_id`.
- `contract_creation_idempotency_key`.
- `contract_creation_confirmation_status`.
- `contract_ready_at`.
- `contract_signed_projection_status`.
- `contract_signature_confirmation_reference`.

A prepared contract is not a signed contract.

### Funding Readiness

- `funding_readiness_status`.
- `funding_requirement_ids`.
- `outstanding_funding_requirements`.
- `customer_down_payment_status`.
- `deal_eligibility_status`.
- `vehicle_eligibility_status`.
- `insurance_requirement_status`.
- `registration_requirement_status`.
- `contract_requirement_status`.
- `lender_condition_status`.
- `funding_block_reasons`.

### Funding Projection

- `funding_status`.
- `funding_requested_at`.
- `funding_command_id`.
- `funding_idempotency_key`.
- `funding_reference`.
- `funding_amount_requested`.
- `funded_amount`.
- `funding_currency_code`.
- `funding_received_at`.
- `funding_confirmation_status`.
- `funding_confirmation_reference`.
- `funding_reconciliation_status`.
- `funding_shortfall_amount`.
- `funding_reversal_status`.
- `funding_reversal_reference`.

### Closure

- `decline_status`.
- `decline_reason_codes`.
- `declined_at`.
- `decline_source`.
- `withdrawal_status`.
- `withdrawn_at`.
- `withdrawal_reason`.
- `cancellation_status`.
- `cancelled_at`.
- `cancellation_reason`.
- `expiration_status`.
- `expired_at`.
- `expiration_reason`.
- `dispute_status`.
- `dispute_case_id`.
- `archived_at`.

### Derived Intelligence

- `application_completion_score`.
- `document_completeness_prediction`.
- `approval_probability`.
- `expected_condition_probability`.
- `funding_delay_risk_score`.
- `fraud_risk_score`.
- `offer_comparison_summary`.
- `recommended_document_request`.
- `recommended_lender_submission_order`.
- `recommended_next_action`.
- `recommended_follow_up_at`.
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
- Limitations.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority.

### Computed Projections

- `application_age_days`.
- `days_since_last_customer_action`.
- `days_since_last_lender_response`.
- `days_in_underwriting`.
- `days_until_decision_expiry`.
- `decision_expired`.
- `document_completion_percentage`.
- `condition_completion_percentage`.
- `funding_requirement_completion_percentage`.
- `funding_shortfall_amount`.
- `active_submission_count`.
- `valid_approval_count`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `reconciliation_status`.
- `field_authority_map`.
- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `submitted_at`.
- `completed_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_application_id` | UUID | Yes | ASOS | Immutable Canonical Finance Application identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `application_number` | String | Yes | ASOS or external authority | Human-readable application reference. |
| `customer_id` | UUID | Yes | Canonical relationship | Primary Customer associated with the application. |
| `opportunity_id` | UUID | Yes | Canonical relationship | Opportunity supported by the Finance Application. |
| `quotation_id` | UUID | Conditional | Canonical relationship | Quotation providing the submitted commercial terms. |
| `vehicle_id` | UUID | Yes | Canonical relationship | Vehicle included in the submitted transaction. |
| `inventory_record_id` | UUID | Conditional | Inventory relationship | Physical Inventory Record where applicable. |
| `trade_in_id` | UUID | No | Canonical relationship | Trade-In contributing equity or payoff. |
| `deal_id` | UUID | No | Canonical relationship | Deal supported by the selected finance offer. |
| `financial_contract_id` | UUID | No | Canonical relationship | Financial Contract created from the selected Decision. |
| `application_type` | Enum | Yes | Workflow State | Individual, joint, corporate, or another approved structure. |
| `finance_product_type` | Enum | Yes | Workflow State | Requested finance-product category. |
| `status` | Enum | Yes | Finance Application workflow | Current aggregate lifecycle state. |
| `current_application_version` | Integer | Yes | ASOS | Current version of the application data. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Applicant Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_applicant_id` | UUID | Yes | ASOS | Identifier for one Applicant inside the Finance Application. |
| `customer_id` | UUID | Yes | Customer relationship | Customer or organization represented by the Applicant record. |
| `applicant_role` | Enum | Yes | Workflow State | Primary Applicant, Co-Applicant, Guarantor, or authorized representative. |
| `applicant_type` | Enum | Yes | Workflow State | Individual or organization classification. |
| `legal_name_projection` | String | Yes | Verified identity projection | Legal name used for the application. |
| `date_of_birth_projection` | Date | Conditional | Verified identity projection | Applicant date of birth where required. |
| `national_identifier_token` | String | Conditional | Secure identity authority | Tokenized national identifier reference. |
| `residency_status` | Enum | Conditional | Applicant evidence or verification | Applicable residency classification. |
| `dependents_count` | Integer | No | Applicant evidence | Number of applicable dependents. |
| `identity_verification_status` | Enum | Yes | Verification workflow | Current identity-verification result. |
| `applicant_status` | Enum | Yes | Workflow State | Current participation status in the application. |

### Employment and Income Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `employment_status` | Enum | Yes | Applicant and verification evidence | Current employment classification. |
| `employer_name` | String | Conditional | Applicant evidence | Employer or business name. |
| `job_title` | String | No | Applicant evidence | Current job title. |
| `employment_start_date` | Date | No | Applicant or verification evidence | Start date of employment. |
| `declared_income_amount` | Decimal | Yes | Applicant | Income declared by the Applicant. |
| `verified_income_amount` | Decimal | No | Approved verification authority | Income accepted through verification. |
| `income_currency_code` | String | Yes | Applicant or verification source | ISO 4217 income currency. |
| `income_frequency` | Enum | Yes | Applicant or verification source | Frequency of the income amount. |
| `income_verification_status` | Enum | Yes | Verification workflow | Current income-verification state. |
| `employment_verification_status` | Enum | Yes | Verification workflow | Current employment-verification state. |
| `income_evidence_references` | Array | No | Controlled evidence storage | Supporting income evidence. |

### Financial Position Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `monthly_housing_cost_amount` | Decimal | Yes | Applicant or verified evidence | Recurring housing commitment. |
| `monthly_debt_payment_amount` | Decimal | Yes | Applicant, bureau, or verified evidence | Existing monthly debt repayments. |
| `monthly_other_commitment_amount` | Decimal | Yes | Applicant or verified evidence | Other applicable recurring commitments. |
| `declared_assets_amount` | Decimal | No | Applicant | Total assets declared for the application. |
| `verified_assets_amount` | Decimal | No | Approved verification authority | Assets accepted through verification. |
| `declared_liabilities_amount` | Decimal | No | Applicant | Total declared liabilities. |
| `verified_liabilities_amount` | Decimal | No | Approved verification authority | Liabilities accepted through verification. |
| `available_down_payment_amount` | Decimal | Yes | Applicant or verified evidence | Funds currently available for down payment. |
| `source_of_funds_status` | Enum | Yes | Verification or compliance workflow | Current source-of-funds review state. |

### Commercial Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `quotation_version` | Integer | Conditional | Quotation relationship | Exact Quotation version used. |
| `quotation_document_hash` | String | Conditional | Quotation evidence | Hash of the submitted commercial document. |
| `vehicle_selling_price_amount` | Decimal | Yes | Quotation | Approved Vehicle selling price. |
| `trade_in_allowance_amount` | Decimal | Yes | Quotation and Trade-In projection | Customer-facing Trade-In allowance. |
| `trade_in_payoff_amount` | Decimal | Yes | Trade-In and Lender evidence | Current payoff included in the transaction. |
| `customer_cash_contribution_amount` | Decimal | Yes | Deterministic calculation | Customer cash contribution. |
| `requested_down_payment_amount` | Decimal | Yes | Customer request and calculation | Proposed down payment. |
| `requested_finance_amount` | Decimal | Yes | Deterministic calculation | Finance amount requested. |
| `requested_term_months` | Integer | Yes | Customer request | Preferred finance term. |
| `preferred_periodic_payment_amount` | Decimal | No | Customer | Preferred periodic payment. |
| `maximum_periodic_payment_amount` | Decimal | No | Customer | Customer-stated maximum periodic payment. |
| `currency_code` | String | Yes | Commercial authority | ISO 4217 transaction currency. |

### Consent Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_consent_record_id` | UUID | Yes | Consent authority | Identifier of one purpose-specific Consent record. |
| `finance_applicant_id` | UUID | Yes | Applicant relationship | Applicant who granted or declined the Consent. |
| `consent_type` | Enum | Yes | Consent workflow | Purpose of the authorization. |
| `consent_status` | Enum | Yes | Applicant evidence | Current Consent status. |
| `consent_policy_version` | String | Yes | Policy authority | Version of the terms presented. |
| `consent_artifact_hash` | String | Conditional | ASOS | Hash of the accepted Consent artifact. |
| `consent_captured_at` | Timestamp | Conditional | Applicant evidence | Time valid Consent was received. |
| `consent_expires_at` | Timestamp | No | Policy or law | Expiration where applicable. |
| `consent_withdrawn_at` | Timestamp | No | Applicant evidence | Time Consent was withdrawn. |

### Verification and Document Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `identity_verification_status` | Enum | Yes | Approved verification authority | Identity-verification state. |
| `address_verification_status` | Enum | Yes | Verification workflow | Current address-verification state. |
| `income_verification_status` | Enum | Yes | Verification workflow | Current income-verification state. |
| `document_completion_status` | Enum | Yes | Document workflow | Required-document completion state. |
| `document_verification_status` | Enum | Yes | Document workflow | Document-verification state. |
| `required_document_types` | Array | Yes | Lender or policy | Required document categories. |
| `missing_document_types` | Array | Yes | Deterministic projection | Currently missing document categories. |
| `document_snapshot_hash` | String | No | ASOS | Hash of the submitted document snapshot. |

### Affordability Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `total_verified_monthly_income_amount` | Decimal | No | Deterministic calculation | Total accepted verified monthly income. |
| `total_monthly_commitments_amount` | Decimal | No | Deterministic calculation | Total applicable recurring commitments. |
| `disposable_monthly_income_amount` | Decimal | No | Deterministic calculation | Verified income less applicable commitments. |
| `debt_to_income_ratio` | Decimal | No | Deterministic calculation | Applicable debt commitments divided by verified income. |
| `payment_to_income_ratio` | Decimal | No | Deterministic calculation | Proposed payment divided by verified income. |
| `loan_to_value_ratio` | Decimal | No | Deterministic calculation | Requested or approved finance divided by applicable Vehicle value. |
| `affordability_status` | Enum | Yes | Deterministic rules or Lender | Current affordability-assessment state. |
| `affordability_rule_version` | String | No | Calculation authority | Applied rule version. |
| `affordability_snapshot_hash` | String | No | ASOS | Integrity hash of the calculation snapshot. |

### Lender Submission Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lender_submission_id` | UUID | Yes | ASOS | Identifier of one Lender submission. |
| `lender_id` | UUID | Yes | Approved Lender registry | Lender receiving the submission. |
| `finance_program_id` | UUID | Conditional | Approved Lender program | Requested finance program. |
| `application_version` | Integer | Yes | ASOS | Exact immutable application version submitted. |
| `application_version_hash` | String | Yes | ASOS | Hash of the submitted application snapshot. |
| `submission_status` | Enum | Yes | Submission workflow | Current submission state. |
| `submission_idempotency_key` | String | Yes | Command workflow | Retry-protection key. |
| `lender_application_reference` | String | No | Lender | External Lender application reference. |
| `external_confirmation_status` | Enum | Yes | Workflow Projection | Current Lender receipt Confirmation state. |

### Lender Decision Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lender_decision_id` | UUID | Yes | ASOS reference | Identifier of the normalized Lender Decision. |
| `decision_version` | String | Yes | Lender | Lender Decision version or reference. |
| `decision_status` | Enum | Yes | Lender | Authoritative underwriting outcome. |
| `decision_valid_until` | Timestamp | No | Lender | Expiration of the Decision. |
| `approved_finance_amount` | Decimal | No | Lender | Approved finance principal. |
| `approved_down_payment_amount` | Decimal | No | Lender | Required approved down payment. |
| `approved_interest_rate` | Decimal | No | Lender | Approved nominal rate. |
| `approved_annual_percentage_rate` | Decimal | No | Lender | Approved APR or equivalent measure. |
| `approved_term_months` | Integer | No | Lender | Approved finance term. |
| `approved_periodic_payment_amount` | Decimal | No | Lender | Approved payment amount. |
| `approved_balloon_payment_amount` | Decimal | No | Lender | Approved balloon amount. |
| `outstanding_conditions` | Array | No | Lender | Conditions not yet accepted as satisfied. |
| `decision_artifact_hash` | String | Yes | ASOS | Integrity hash of the authoritative Decision artifact. |

### Customer Selection Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_decision_status` | Enum | Yes | Customer evidence | Current Customer selection state. |
| `selected_lender_decision_id` | UUID | Conditional | Canonical relationship | Exact Decision selected by the Customer. |
| `selected_offer_version` | String | Conditional | Lender Decision | Version of the selected offer. |
| `selected_offer_snapshot_hash` | String | Conditional | ASOS | Hash of the selected finance offer. |
| `customer_selected_at` | Timestamp | No | Customer evidence | Time valid selection was received. |
| `customer_selection_evidence_references` | Array | No | Evidence repository | Supporting selection evidence. |
| `customer_rejection_reason` | Enum | No | Customer evidence | Standard reason where known. |

### Contract and Funding Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contract_readiness_status` | Enum | Yes | Deterministic workflow | Readiness to create the Financial Contract. |
| `financial_contract_id` | UUID | No | Canonical relationship | Financial Contract generated from the selected offer. |
| `contract_signed_projection_status` | Enum | Yes | Financial Contract projection | Current signature projection. |
| `funding_readiness_status` | Enum | Yes | Deterministic workflow | Readiness to request Lender funding. |
| `funding_status` | Enum | Yes | Funding workflow | Current Lender-funding state. |
| `funding_amount_requested` | Decimal | No | Contract or Deal | Amount requested from the Lender. |
| `funded_amount` | Decimal | No | Authoritative funding source | Confirmed funds received. |
| `funding_received_at` | Timestamp | No | Authoritative funding source | Time funding was received. |
| `funding_confirmation_status` | Enum | Yes | Workflow Projection | External Confirmation status. |
| `funding_reconciliation_status` | Enum | Yes | Reconciliation workflow | Current funding reconciliation state. |

---

## 4. Enumerations

### FinanceApplicationStatus

- `DRAFT`
- `CONSENT_PENDING`
- `DOCUMENTS_PENDING`
- `VERIFICATION_PENDING`
- `READY_FOR_SUBMISSION`
- `SUBMISSION_PENDING`
- `SUBMITTED`
- `UNDER_REVIEW`
- `CONDITIONS_PENDING`
- `CONDITIONALLY_APPROVED`
- `APPROVED`
- `OFFER_PRESENTED`
- `CUSTOMER_ACCEPTED`
- `CONTRACTING`
- `FUNDING_PENDING`
- `FUNDED`
- `DECLINED`
- `WITHDRAWN`
- `EXPIRED`
- `CANCELLED`
- `DISPUTED`
- `ARCHIVED`

### FinanceApplicationType

- `INDIVIDUAL`
- `JOINT`
- `GUARANTEED`
- `CORPORATE`
- `SOLE_PROPRIETOR`
- `FLEET`
- `OTHER`

### FinanceApplicantRole

- `PRIMARY_APPLICANT`
- `CO_APPLICANT`
- `GUARANTOR`
- `CORPORATE_APPLICANT`
- `AUTHORIZED_SIGNATORY`
- `BENEFICIAL_OWNER`
- `OTHER`

### FinanceApplicantType

- `INDIVIDUAL`
- `ORGANIZATION`

### FinanceApplicantStatus

- `INVITED`
- `INFORMATION_PENDING`
- `CONSENT_PENDING`
- `VERIFICATION_PENDING`
- `COMPLETE`
- `WITHDRAWN`
- `REJECTED`
- `EXPIRED`

### FinanceProductType

- `RETAIL_FINANCE`
- `VEHICLE_LOAN`
- `HIRE_PURCHASE`
- `LEASE`
- `BALLOON_FINANCE`
- `ISLAMIC_FINANCE`
- `CORPORATE_FINANCE`
- `FLEET_FINANCE`
- `REFINANCE`
- `OTHER`

### FinanceSubmissionChannel

- `DEALERSHIP`
- `CUSTOMER_PORTAL`
- `DIGITAL_APPLICATION`
- `PHONE_ASSISTED`
- `LENDER_PORTAL`
- `OEM_PLATFORM`
- `API_INTEGRATION`
- `MANUAL_CONTROLLED_ENTRY`
- `OTHER`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_FI_PLATFORM_AUTHORITATIVE`
- `LENDER_PLATFORM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### EmploymentStatus

- `UNKNOWN`
- `EMPLOYED`
- `SELF_EMPLOYED`
- `BUSINESS_OWNER`
- `CONTRACTOR`
- `RETIRED`
- `STUDENT`
- `UNEMPLOYED`
- `OTHER`

### EmploymentType

- `PERMANENT`
- `TEMPORARY`
- `FIXED_TERM`
- `PART_TIME`
- `FULL_TIME`
- `SEASONAL`
- `FREELANCE`
- `OTHER`
- `UNKNOWN`

### IncomeFrequency

- `WEEKLY`
- `BIWEEKLY`
- `MONTHLY`
- `QUARTERLY`
- `ANNUALLY`
- `IRREGULAR`

### ResidencyStatus

- `UNKNOWN`
- `CITIZEN`
- `PERMANENT_RESIDENT`
- `RESIDENT`
- `TEMPORARY_RESIDENT`
- `NON_RESIDENT`
- `OTHER`

### ResidenceType

- `OWNED`
- `MORTGAGED`
- `RENTED`
- `FAMILY_OWNED`
- `EMPLOYER_PROVIDED`
- `OTHER`
- `UNKNOWN`

### FinanceConsentType

- `APPLICATION_PROCESSING`
- `CREDIT_BUREAU_ACCESS`
- `LENDER_DATA_SHARING`
- `IDENTITY_VERIFICATION`
- `DOCUMENT_VERIFICATION`
- `ELECTRONIC_COMMUNICATION`
- `ELECTRONIC_SIGNATURE`
- `OTHER`

### FinanceConsentStatus

- `NOT_REQUESTED`
- `PENDING`
- `GRANTED`
- `DECLINED`
- `WITHDRAWN`
- `EXPIRED`
- `REVALIDATION_REQUIRED`
- `DISPUTED`

### VerificationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `PARTIALLY_VERIFIED`
- `VERIFIED`
- `FAILED`
- `REJECTED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### DocumentCompletionStatus

- `NOT_STARTED`
- `INCOMPLETE`
- `PARTIALLY_COMPLETE`
- `COMPLETE`
- `EXPIRED`
- `REJECTED`
- `REVALIDATION_REQUIRED`

### DocumentVerificationStatus

- `NOT_STARTED`
- `PENDING`
- `PARTIALLY_VERIFIED`
- `VERIFIED`
- `FAILED`
- `REJECTED`
- `EXPIRED`
- `DISPUTED`

### CreditBureauRequestStatus

- `NOT_REQUESTED`
- `CONSENT_PENDING`
- `REQUEST_PENDING`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `FAILED`
- `NOT_PERMITTED`
- `EXPIRED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### AffordabilityStatus

- `NOT_ASSESSED`
- `PENDING`
- `PASSED`
- `PASSED_WITH_CONDITIONS`
- `FAILED`
- `INDETERMINATE`
- `MANUAL_REVIEW_REQUIRED`
- `EXPIRED`

An ASOS affordability result is not a Lender Decision.

### InternalRiskStatus

- `NOT_ASSESSED`
- `LOW_REVIEW_REQUIREMENT`
- `STANDARD_REVIEW`
- `ENHANCED_REVIEW`
- `BLOCKED`
- `DISPUTED`

These labels represent internal workflow needs and must not be presented as Lender underwriting outcomes.

### LenderEligibilityStatus

- `NOT_EVALUATED`
- `EVALUATING`
- `ELIGIBLE`
- `PARTIALLY_ELIGIBLE`
- `NO_ELIGIBLE_LENDER`
- `REVIEW_REQUIRED`
- `EXPIRED`

### LenderSubmissionStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `CONSENT_VALIDATION_PENDING`
- `READY`
- `COMMAND_PENDING`
- `SUBMITTED_TO_PROVIDER`
- `PENDING_CONFIRMATION`
- `RECEIVED_BY_LENDER`
- `UNDER_REVIEW`
- `REJECTED_BY_PROVIDER`
- `FAILED`
- `CANCELLED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### FinanceDecisionStatus

- `NOT_DECIDED`
- `PENDING`
- `MORE_INFORMATION_REQUIRED`
- `REFERRED`
- `CONDITIONALLY_APPROVED`
- `APPROVED`
- `DECLINED`
- `WITHDRAWN_BY_LENDER`
- `EXPIRED`
- `CANCELLED`
- `DISPUTED`

### UnderwritingConditionStatus

- `OPEN`
- `EVIDENCE_REQUESTED`
- `EVIDENCE_SUBMITTED`
- `PENDING_LENDER_CONFIRMATION`
- `SATISFIED`
- `REJECTED`
- `WAIVED_BY_LENDER`
- `EXPIRED`
- `DISPUTED`

Only the Lender may authoritatively satisfy or waive a Lender condition.

### OfferPresentationStatus

- `NOT_READY`
- `READY`
- `PRESENTATION_PENDING`
- `PRESENTED`
- `FAILED`
- `EXPIRED`
- `WITHDRAWN`

### CustomerFinanceDecisionStatus

- `NOT_PRESENTED`
- `PENDING`
- `SELECTED`
- `REJECTED_ALL`
- `REQUESTED_CHANGES`
- `WITHDRAWN`
- `EXPIRED`
- `DISPUTED`

### CustomerFinanceRejectionReason

- `INTEREST_RATE_NOT_ACCEPTED`
- `PAYMENT_NOT_ACCEPTED`
- `DOWN_PAYMENT_NOT_ACCEPTED`
- `TERM_NOT_ACCEPTED`
- `FEES_NOT_ACCEPTED`
- `CONDITIONS_NOT_ACCEPTED`
- `CUSTOMER_SELECTED_CASH`
- `CUSTOMER_SELECTED_ANOTHER_LENDER`
- `CUSTOMER_POSTPONED`
- `CUSTOMER_NO_LONGER_INTERESTED`
- `OTHER`
- `UNKNOWN`

### ContractReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### ContractPreparationStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `CREATION_PENDING`
- `PENDING_EXTERNAL_CONFIRMATION`
- `CREATED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ContractSignatureProjectionStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `PARTIALLY_SIGNED`
- `SIGNED`
- `DECLINED`
- `EXPIRED`
- `VOIDED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

The authoritative signature state belongs to Financial Contract or its configured signature authority.

### FundingReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### FinanceFundingStatus

- `NOT_STARTED`
- `REQUIREMENTS_PENDING`
- `FUNDING_REQUEST_READY`
- `COMMAND_PENDING`
- `FUNDING_REQUESTED`
- `PENDING_CONFIRMATION`
- `PARTIALLY_FUNDED`
- `FUNDED`
- `FUNDING_FAILED`
- `FUNDING_REVERSED`
- `DISPUTED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### FinanceApplicationWithdrawalReason

- `CUSTOMER_REQUEST`
- `CONSENT_WITHDRAWN`
- `CUSTOMER_SELECTED_CASH`
- `CUSTOMER_SELECTED_ANOTHER_FINANCE_SOURCE`
- `COMMERCIAL_TERMS_CHANGED`
- `CUSTOMER_POSTPONED`
- `OTHER`

### FinanceApplicationCancellationReason

- `OPPORTUNITY_LOST`
- `DEAL_CANCELLED`
- `DUPLICATE_APPLICATION`
- `APPLICATION_ERROR`
- `CUSTOMER_IDENTITY_CONFLICT`
- `VEHICLE_UNAVAILABLE`
- `QUOTATION_INVALID`
- `COMPLIANCE_BLOCK`
- `FRAUD_REVIEW`
- `LENDER_SUBMISSION_PROHIBITED`
- `OTHER`

### FinanceApplicationExpirationReason

- `APPLICATION_STALE`
- `CONSENT_EXPIRED`
- `DOCUMENTS_EXPIRED`
- `VERIFICATION_EXPIRED`
- `LENDER_DECISION_EXPIRED`
- `CUSTOMER_SELECTION_EXPIRED`
- `CONTRACTING_WINDOW_EXPIRED`
- `FUNDING_WINDOW_EXPIRED`
- `OTHER`

### ConfirmationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

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
- All related Domain Objects must belong to the authorized Tenant.
- Dealership, branch, finance team, assigned User, Lender, and finance program must belong to the permitted organizational scope.
- Cross-Tenant Finance Application access, matching, submission, underwriting display, AI retrieval, export, and reporting are prohibited unless governed by an approved auditable mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Customer and Opportunity Rules

- Every Finance Application must reference one primary Customer.
- Every sales-related Finance Application must reference one valid Opportunity.
- Customer and Opportunity must be consistent.
- The Opportunity must be eligible for finance progression.
- A closed, lost, cancelled, or archived Opportunity requires a governed exception or reopening before new submission activity.
- Customer identity corrections must use Customer governance.
- A Finance Application must not silently rewrite Customer identity.

### Applicant Structure Rules

- Every Finance Application must contain one primary Applicant.
- Joint applications require at least one Co-Applicant.
- Guaranteed applications require at least one Guarantor where applicable.
- Corporate applications require one organization and the required authorized signatories.
- Every participating Applicant must have:
  - Valid relationship to the Finance Application.
  - Required Consent.
  - Required identity verification.
  - Required documents.
- An Applicant must not be added to a submitted immutable version without creating a new version.
- Removing an Applicant after submission requires a new application version and may require Lender resubmission.
- Applicant relationships must not be inferred solely by AI.

### Applicant Identity Rules

- Applicant identity must be linked to a canonical Customer or organization record.
- National identifiers must be tokenized or stored in approved secure systems.
- Raw national identifiers must not appear in:
  - General application fields.
  - Logs.
  - Prompts.
  - Analytics exports.
  - General-purpose embeddings.
- Format validation does not prove identity.
- Identity verification must preserve:
  - Provider.
  - Method.
  - Evidence.
  - Result.
  - Timestamp.
  - Expiration.
- Identity conflicts must create Human Review.
- AI must not authoritatively verify identity.

### Consent Rules

Before credit-bureau access or Lender submission:

- Required Applicant-specific Consent must be valid.
- Consent must cover the specific purpose.
- Consent must cover the relevant data categories.
- Consent must cover the permitted recipient where required.
- Consent terms and version must be preserved.
- Consent evidence and timestamp must be preserved.
- Consent must not be expired, withdrawn, or disputed.

A joint application requires required Consent from every applicable Applicant.

Consent withdrawal must:

- Stop future prohibited processing.
- Stop new unauthorized bureau requests.
- Stop new unauthorized Lender submissions.
- Trigger review of active pending Commands.
- Preserve lawful historical evidence.
- Not falsely claim that an already completed Lender submission never occurred.

Finance Consent must not create general marketing Consent.

### Data-Minimization Rules

- Only data required for the application, verification, underwriting, contract, funding, compliance, or lawful audit purpose may be collected.
- Optional information must be clearly distinguished.
- Sensitive data must not be copied between Objects without an approved purpose.
- Full credit reports should remain in controlled storage.
- Finance Application projections should store only required values and references.
- Applicant data must not be reused for unrelated marketing, profiling, or AI training without an independent lawful basis.

### Commercial Snapshot Rules

- Quotation, Vehicle, Inventory, Trade-In, and Customer references must be consistent.
- The Finance Application must preserve the exact Quotation version used.
- The commercial snapshot must be immutable after Lender submission.
- Requested finance amount must be calculated deterministically.
- Customer cash contribution, down payment, Trade-In equity, fees, and requested finance must reconcile.
- Material commercial changes require a new application version.
- An expired or withdrawn Quotation must not support a new submission.
- Vehicle and Inventory eligibility must be sufficiently current before submission and funding.

### Income and Financial Position Rules

- Declared income must remain distinguishable from verified income.
- Income and commitment values must use approved currency and frequency normalization.
- Amounts must use fixed decimal precision.
- Negative income, asset, liability, payment, or finance values are prohibited unless a specific signed adjustment model permits them.
- `dependents_count` must be zero or greater.
- Customer uncertainty must be represented as unknown rather than invented.
- Income verification must preserve source and freshness.
- Expired income evidence must not support a new submission without revalidation.
- Self-employed and corporate Applicants may require different evidence rules.
- AI extraction may assist but must not create verified income.

### Affordability Calculation Rules

Authoritative ratios and financial calculations must be performed by approved deterministic services.

AI must not calculate authoritative affordability outcomes.

Applicable calculations may include:

```text
total_verified_monthly_income_amount
total_monthly_commitments_amount
disposable_monthly_income_amount
debt_to_income_ratio
payment_to_income_ratio
loan_to_value_ratio
down_payment_percentage
funding_shortfall_amount
```

Every calculation must preserve:

- Formula version.
- Input values.
- Source-record versions.
- Currency.
- Frequency normalization.
- Rounding method.
- Rule version.
- Timestamp.
- Result.
- Integrity hash.

Thresholds must remain configurable by:

- Jurisdiction.
- Lender.
- Finance program.
- Applicant type.
- Product.
- Policy version.

An ASOS affordability result must not be represented as Lender approval.

### Credit-Bureau Rules

- Credit-bureau access requires applicable valid Consent and legal authority.
- Only approved Credit Bureaus may be used.
- Each request must preserve:
  - Applicant.
  - Purpose.
  - Consent record.
  - Provider.
  - Request time.
  - Response time.
  - Reference.
  - Integrity hash.
- Duplicate bureau requests must be prevented or explicitly authorized.
- Bureau data must remain purpose-limited.
- Bureau disputes must be recorded.
- A credit score must not be altered by ASOS.
- AI must not fabricate, estimate, or replace an authoritative bureau score.
- Expired bureau information must not be used beyond applicable policy.

### Document Rules

- Required documents must be determined by current policy and Lender requirements.
- Raw documents must remain in controlled document storage.
- Every document must preserve:
  - Document type.
  - Applicant.
  - Source.
  - Issue date.
  - Expiration.
  - Verification result.
  - Evidence reference.
  - Integrity hash.
  - Security classification.
- AI-extracted fields remain Derived Intelligence until accepted by an approved verification workflow.
- Missing, expired, rejected, or disputed documents must remain explicit.
- Document completion does not prove document authenticity.
- Document authenticity does not prove Lender approval.

### Fraud, Compliance, and Source-of-Funds Rules

- Applicable identity, fraud, sanctions, source-of-funds, beneficial-ownership, and compliance checks must complete before prohibited actions.
- A compliance block must override commercial progression.
- AI risk scoring must not independently create a final adverse Decision.
- Material risk findings require authorized Human Review.
- Sensitive risk details must be restricted.
- Lender underwriting and dealership compliance Decisions must remain distinguishable.
- Fraud suspicion must not be disclosed to unauthorized Users or Customers.

### Lender Eligibility Rules

- Only approved active Lenders may receive Applicant data.
- Only approved active finance programs may be selected.
- Lender eligibility must use current configured rules.
- Eligibility to submit does not mean likely approval.
- Lender selection must not use protected attributes unlawfully.
- Applicant information must not be sent to unnecessary Lenders.
- Recipient minimization and permitted submission count must follow policy.
- An unauthorized Lender must never receive Customer data.

### Application Version Rules

- `current_application_version` must begin at an approved initial value and increase sequentially.
- Every submitted version must be immutable.
- A material change after submission requires a new version.
- A new version must reference the prior version.
- Circular version relationships are prohibited.
- Every version must preserve its snapshot hash.
- A Lender submission must reference exactly one version.
- A Customer-selected offer must remain linked to the submitted version that produced it.
- Retryable version creation must not create duplicate versions.
- Prior submitted versions must remain historically accessible to authorized Users.

### Lender Submission Rules

A Lender submission requires:

- Valid application version.
- Valid Applicants.
- Required Consent.
- Required identity verification.
- Required documents.
- Current commercial snapshot.
- Current affordability calculation.
- Current compliance state.
- Eligible Lender and program.
- Required Human Decision where applicable.
- Submission idempotency key.
- Immutable submission snapshot.
- Audit evidence.

Submission must remain `PENDING_CONFIRMATION` until the Lender or configured platform confirms receipt where external authority applies.

A successful HTTP response does not prove the Lender received or accepted the application.

Duplicate retries must not create duplicate Lender applications.

### Lender Response and Decision Rules

- Lender responses must be authenticated where technically supported.
- External responses must preserve source and integrity evidence.
- A Lender Decision must be recorded exactly as received.
- ASOS normalization must not change legal or financial meaning.
- Dealership Users must not change:
  - Decision status.
  - Approved amount.
  - Rate.
  - APR.
  - Term.
  - Payment.
  - Conditions.
  - Expiration.
- A conditional approval must preserve every condition.
- A Decision must not be shown as current after expiration.
- A decline must remain distinguishable from:
  - Technical failure.
  - Incomplete submission.
  - Referred Decision.
  - Customer withdrawal.
- Reconsideration or resubmission requires a governed process.

### Multiple Lender Decision Rules

One Finance Application may have:

- Multiple pending submissions.
- Multiple declines.
- Multiple conditional approvals.
- Multiple approvals.
- One Customer-selected offer at a time.

Offer comparison must:

- Compare equivalent payment frequencies and currencies.
- Preserve Lender disclosures.
- Avoid hiding fees or conditions.
- Avoid ranking solely by a single payment figure.
- Preserve offer expiration.
- Identify important conditions.
- Remain explainable.

ASOS must not represent one Lender Decision as another Lender’s Decision.

### Underwriting Condition Rules

- Every Lender condition must preserve the original Lender wording or controlled reference.
- Submitted evidence does not prove condition satisfaction.
- Only authoritative Lender Confirmation may mark a Lender condition satisfied or waived.
- Expired condition evidence must be revalidated.
- Unresolved conditions must block contract or funding progression where required.
- AI may summarize conditions but must not remove or reinterpret their legal meaning.

### Customer Offer Presentation Rules

An offer may be presented only when:

- The Lender Decision is valid.
- The offer is permitted for Customer presentation.
- Required disclosures are available.
- Material conditions are shown.
- Customer-facing values match the Lender Decision.
- The offer has not expired, been withdrawn, or been superseded.
- Customer contact is permitted.
- Human Approval or applicable automation policy authorizes delivery.

Internal underwriting notes must not appear in Customer-facing content.

### Customer Selection Rules

Customer selection requires:

- Exact Lender Decision.
- Exact Decision version.
- Exact offer snapshot hash.
- Valid offer.
- Customer identity.
- Accepted selection evidence.
- Selection timestamp.
- Required disclosures.
- No blocking conflict.

Customer selection must not be inferred solely from:

- Positive sentiment.
- Offer view.
- Appointment attendance.
- Salesperson note without accepted evidence.
- AI prediction.
- Contract preparation.
- Down-payment initiation.

Customer selection does not sign the Financial Contract.

### Contracting Rules

Financial Contract creation requires:

- Valid Customer-selected offer.
- Current Lender Decision.
- All required conditions for contract preparation.
- Valid Customer and Applicant identities.
- Current commercial terms.
- Required Human Decisions.
- Idempotent creation request.
- Exact selected-offer snapshot.

The Finance Application must not become `FUNDED` because:

- A contract was generated.
- A contract was sent.
- One Applicant signed.
- A signature provider acknowledged receipt.

Authoritative contract status belongs to Financial Contract and its configured authority.

### Funding Readiness Rules

Funding readiness requires applicable:

- Signed and valid Financial Contract.
- Current Lender approval.
- Required Lender conditions.
- Eligible Deal.
- Eligible Vehicle and Inventory.
- Required Customer contribution.
- Required insurance.
- Required registration or title evidence.
- Required dealership invoice.
- Required delivery or release controls.
- No compliance or reconciliation block.

Funding readiness does not prove funding.

### Funding Rules

A funding request must:

- Reference the exact selected Lender Decision.
- Reference the exact Financial Contract.
- Reference the eligible Deal.
- Preserve the requested amount.
- Use an idempotency key.
- Preserve the Command and External Confirmation.
- Remain pending until authoritative evidence is received.

`FUNDED` requires:

- Authoritative funding Confirmation.
- Confirmed amount.
- Confirmed currency.
- Funding timestamp.
- Funding reference.
- Reconciliation with Deal and contract.
- No unresolved material shortfall.

A partial funding outcome must remain `PARTIALLY_FUNDED`.

A funding reversal must use a new authoritative event and reconciliation workflow.

### Decline Rules

A Finance Application may become aggregate `DECLINED` only when:

- All applicable active Lender paths are declined, closed, or no longer eligible; and
- No valid approved or conditionally approved offer remains; and
- The configured closure policy is satisfied.

One Lender decline must not automatically mark the entire Finance Application declined when other active submissions remain.

A Lender decline reason must be handled according to applicable disclosure rules.

AI must not invent or expand a decline reason.

### Withdrawal Rules

Customer withdrawal requires:

- Customer or authorized representative evidence.
- Withdrawal scope.
- Effective timestamp.
- Treatment of active Lender submissions.
- Treatment of Consent.
- Treatment of pending Commands.
- Required communication.
- Audit evidence.

Withdrawal does not erase completed Lender submissions or Decisions.

### Cancellation Rules

Dealership cancellation requires:

- Valid reason.
- Authorized Human Decision or policy.
- Review of active submissions.
- Review of selected offer.
- Review of Financial Contract and funding state.
- Required Customer communication.
- Audit evidence.

A funded application cannot be cancelled through an ordinary update.

It requires a governed unwind, reversal, or correction workflow.

### Expiration Rules

Application or Decision expiration must be based on authoritative time and policy.

An expired:

- Consent.
- Document.
- Verification.
- Credit report.
- Lender Decision.
- Customer offer.
- Contracting window.
- Funding window.

must block dependent future actions until revalidation.

Expiration must not delete historical records.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Application creation must support idempotency.
- Application-version creation must support idempotency.
- Bureau requests must support deduplication and idempotency.
- Lender submissions must support idempotency.
- Customer selection must support idempotency.
- Financial Contract creation must support idempotency.
- Funding requests must support idempotency.
- Retryable Commands must use `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Applications.
  - Application versions.
  - Applicant records.
  - Bureau requests.
  - Lender submissions.
  - Lender Decisions.
  - Customer selections.
  - Financial Contracts.
  - Funding requests.
  - Funding transactions.

### Fairness and Non-Discrimination Rules

- ASOS must not make unlawful credit Decisions.
- Protected attributes must not be used unlawfully in eligibility, ranking, or Recommendations.
- AI proxies for protected attributes must not be used to create adverse treatment.
- Lender Decisions must remain distinguishable from ASOS analysis.
- Any model used for finance assistance must undergo applicable:
  - Fairness review.
  - Bias assessment.
  - Explainability review.
  - Monitoring.
  - Version control.
  - Approval.
- Adverse action must not be generated solely from an unapproved AI model.
- Required notices must use authoritative Lender or legal sources.

### Human Review Requirements

Human Review is required according to policy for:

- Applicant identity conflict.
- Duplicate Finance Application.
- Consent dispute.
- Joint-Applicant inconsistency.
- Document authenticity conflict.
- Income or employment conflict.
- Credit-report dispute.
- Source-of-funds issue.
- Fraud or compliance concern.
- Manual affordability exception.
- Lender submission exception.
- Lender Decision conflict.
- Customer selection dispute.
- Contract mismatch.
- Funding shortfall.
- Funding reversal.
- Reopening a terminal application.
- Another high-risk legal, financial, or compliance exception.

---

## 6. State Machine

### Allowed States

```text
DRAFT
CONSENT_PENDING
DOCUMENTS_PENDING
VERIFICATION_PENDING
READY_FOR_SUBMISSION
SUBMISSION_PENDING
SUBMITTED
UNDER_REVIEW
CONDITIONS_PENDING
CONDITIONALLY_APPROVED
APPROVED
OFFER_PRESENTED
CUSTOMER_ACCEPTED
CONTRACTING
FUNDING_PENDING
FUNDED
DECLINED
WITHDRAWN
EXPIRED
CANCELLED
DISPUTED
ARCHIVED
```

### Principal Allowed Transitions

```text
DRAFT → CONSENT_PENDING
DRAFT → DOCUMENTS_PENDING
DRAFT → VERIFICATION_PENDING
DRAFT → CANCELLED
DRAFT → WITHDRAWN

CONSENT_PENDING → DOCUMENTS_PENDING
CONSENT_PENDING → VERIFICATION_PENDING
CONSENT_PENDING → DRAFT
CONSENT_PENDING → WITHDRAWN
CONSENT_PENDING → CANCELLED
CONSENT_PENDING → EXPIRED

DOCUMENTS_PENDING → CONSENT_PENDING
DOCUMENTS_PENDING → VERIFICATION_PENDING
DOCUMENTS_PENDING → READY_FOR_SUBMISSION
DOCUMENTS_PENDING → WITHDRAWN
DOCUMENTS_PENDING → CANCELLED
DOCUMENTS_PENDING → EXPIRED

VERIFICATION_PENDING → DOCUMENTS_PENDING
VERIFICATION_PENDING → READY_FOR_SUBMISSION
VERIFICATION_PENDING → DISPUTED
VERIFICATION_PENDING → WITHDRAWN
VERIFICATION_PENDING → CANCELLED
VERIFICATION_PENDING → EXPIRED

READY_FOR_SUBMISSION → SUBMISSION_PENDING
READY_FOR_SUBMISSION → DOCUMENTS_PENDING
READY_FOR_SUBMISSION → VERIFICATION_PENDING
READY_FOR_SUBMISSION → WITHDRAWN
READY_FOR_SUBMISSION → CANCELLED
READY_FOR_SUBMISSION → EXPIRED

SUBMISSION_PENDING → SUBMITTED
SUBMISSION_PENDING → READY_FOR_SUBMISSION
SUBMISSION_PENDING → DISPUTED
SUBMISSION_PENDING → WITHDRAWN
SUBMISSION_PENDING → CANCELLED

SUBMITTED → UNDER_REVIEW
SUBMITTED → CONDITIONS_PENDING
SUBMITTED → CONDITIONALLY_APPROVED
SUBMITTED → APPROVED
SUBMITTED → DECLINED
SUBMITTED → WITHDRAWN
SUBMITTED → EXPIRED
SUBMITTED → DISPUTED

UNDER_REVIEW → CONDITIONS_PENDING
UNDER_REVIEW → CONDITIONALLY_APPROVED
UNDER_REVIEW → APPROVED
UNDER_REVIEW → DECLINED
UNDER_REVIEW → WITHDRAWN
UNDER_REVIEW → EXPIRED
UNDER_REVIEW → DISPUTED

CONDITIONS_PENDING → UNDER_REVIEW
CONDITIONS_PENDING → CONDITIONALLY_APPROVED
CONDITIONS_PENDING → APPROVED
CONDITIONS_PENDING → DECLINED
CONDITIONS_PENDING → EXPIRED
CONDITIONS_PENDING → DISPUTED

CONDITIONALLY_APPROVED → CONDITIONS_PENDING
CONDITIONALLY_APPROVED → APPROVED
CONDITIONALLY_APPROVED → OFFER_PRESENTED
CONDITIONALLY_APPROVED → DECLINED
CONDITIONALLY_APPROVED → EXPIRED
CONDITIONALLY_APPROVED → WITHDRAWN
CONDITIONALLY_APPROVED → DISPUTED

APPROVED → OFFER_PRESENTED
APPROVED → CUSTOMER_ACCEPTED
APPROVED → EXPIRED
APPROVED → WITHDRAWN
APPROVED → DISPUTED

OFFER_PRESENTED → CUSTOMER_ACCEPTED
OFFER_PRESENTED → APPROVED
OFFER_PRESENTED → EXPIRED
OFFER_PRESENTED → WITHDRAWN
OFFER_PRESENTED → DISPUTED

CUSTOMER_ACCEPTED → CONTRACTING
CUSTOMER_ACCEPTED → EXPIRED
CUSTOMER_ACCEPTED → WITHDRAWN
CUSTOMER_ACCEPTED → DISPUTED

CONTRACTING → FUNDING_PENDING
CONTRACTING → CUSTOMER_ACCEPTED
CONTRACTING → EXPIRED
CONTRACTING → CANCELLED
CONTRACTING → DISPUTED

FUNDING_PENDING → FUNDED
FUNDING_PENDING → CONTRACTING
FUNDING_PENDING → EXPIRED
FUNDING_PENDING → CANCELLED
FUNDING_PENDING → DISPUTED

DISPUTED → previous permitted non-terminal state
DISPUTED → WITHDRAWN
DISPUTED → CANCELLED

FUNDED → ARCHIVED
DECLINED → ARCHIVED
WITHDRAWN → ARCHIVED
EXPIRED → ARCHIVED
CANCELLED → ARCHIVED
```

Returning from `DISPUTED` requires an accepted resolution and supporting evidence.

### Forbidden Ordinary Transitions

```text
DRAFT → SUBMITTED
DRAFT → APPROVED
DRAFT → CUSTOMER_ACCEPTED
DRAFT → FUNDED

CONSENT_PENDING → SUBMITTED
DOCUMENTS_PENDING → SUBMITTED
VERIFICATION_PENDING → SUBMITTED

READY_FOR_SUBMISSION → APPROVED
SUBMISSION_PENDING → APPROVED

SUBMITTED → CUSTOMER_ACCEPTED
UNDER_REVIEW → CUSTOMER_ACCEPTED

CONDITIONALLY_APPROVED → FUNDED
APPROVED → FUNDED
OFFER_PRESENTED → FUNDED
CUSTOMER_ACCEPTED → FUNDED

DECLINED → APPROVED
DECLINED → CUSTOMER_ACCEPTED
DECLINED → FUNDED

WITHDRAWN → SUBMITTED
WITHDRAWN → APPROVED
WITHDRAWN → FUNDED

EXPIRED → SUBMITTED
EXPIRED → CUSTOMER_ACCEPTED
EXPIRED → FUNDED

CANCELLED → SUBMITTED
CANCELLED → APPROVED
CANCELLED → FUNDED

FUNDED → DRAFT
FUNDED → CANCELLED
FUNDED → EXPIRED

ARCHIVED → DRAFT
ARCHIVED → SUBMITTED
ARCHIVED → FUNDED
```

Corrections to terminal or financially significant outcomes require a separate governed correction, dispute, resubmission, reversal, or unwind workflow.

### Entering DRAFT

Requires:

- Valid Tenant context.
- Customer.
- Opportunity.
- Finance-product intent.
- Creation authority.
- Idempotency protection.
- Initial audit evidence.

### Entering CONSENT_PENDING

Requires:

- Identified Applicants.
- Identified required Consent purposes.
- Applicable Consent terms.
- Consent requests or collection workflow.

### Entering DOCUMENTS_PENDING

Requires:

- Current document-requirement set.
- Identified missing, expired, rejected, or incomplete documents.
- Responsible User or queue.
- Permitted Customer communication plan.

### Entering VERIFICATION_PENDING

Requires:

- Received minimum required evidence.
- Identity, income, employment, document, or compliance verification request.
- Applicable Consent.
- Approved providers.

### Entering READY_FOR_SUBMISSION

Requires:

- Complete required Applicants.
- Valid required Consent.
- Required identity verification.
- Required documents.
- Current commercial snapshot.
- Successful deterministic calculation.
- Eligible Lender path.
- No blocking compliance issue.
- Current record version.

### Entering SUBMISSION_PENDING

Requires:

- Exact immutable application version.
- Approved Lender and program.
- Valid Consent snapshot.
- Valid document snapshot.
- Submission Command.
- Idempotency key.
- Audit evidence.

### Entering SUBMITTED

Requires:

- Authoritative Lender receipt or configured acceptance evidence.
- Lender submission reference.
- Submitted application-version hash.
- Submission timestamp.
- No duplicate submission conflict.

### Entering UNDER_REVIEW

Requires:

- Lender or configured platform evidence that underwriting review is active.

### Entering CONDITIONS_PENDING

Requires:

- Authoritative Lender request for additional information or conditions.
- Preserved condition list.
- Due dates where applicable.
- Responsible workflow.

### Entering CONDITIONALLY_APPROVED

Requires:

- Authoritative Lender conditional Decision.
- Preserved Decision version.
- Approved terms.
- Outstanding conditions.
- Decision validity period.
- Decision artifact and hash.

### Entering APPROVED

Requires:

- Authoritative Lender approval.
- Valid Decision.
- Approved terms.
- Required conditions satisfied or waived by the Lender.
- Decision artifact.
- Decision Confirmation.
- No blocking conflict.

### Entering OFFER_PRESENTED

Requires:

- Current valid presentable Lender offer.
- Required Customer disclosures.
- Permitted communication.
- Customer-facing presentation evidence.
- Offer timestamp.

### Entering CUSTOMER_ACCEPTED

Requires:

- Exact selected Lender Decision.
- Exact offer version.
- Exact selected-offer snapshot hash.
- Valid Customer identity.
- Accepted selection evidence.
- Current Decision validity.
- No blocking conflict.

### Entering CONTRACTING

Requires:

- Valid selected offer.
- Contract readiness.
- Financial Contract creation workflow.
- Required Human authority.
- Idempotency protection.
- Current selected terms.

### Entering FUNDING_PENDING

Requires:

- Applicable signed Financial Contract.
- Current Lender approval.
- Funding readiness.
- Eligible Deal and Vehicle.
- Required Customer contribution.
- Required Lender conditions.
- Funding request Command.
- Idempotency key.

### Entering FUNDED

Requires:

- Authoritative funding Confirmation.
- Confirmed amount.
- Confirmed currency.
- Funding timestamp.
- Funding reference.
- Deal and contract reconciliation.
- No unresolved material shortfall.
- No active funding reversal.

### Entering DECLINED

Requires:

- Applicable authoritative Lender declines or aggregate closure policy.
- No valid active approval.
- Preserved Decision evidence.
- Required Customer notification handling.
- Decline timestamp.

### Entering WITHDRAWN

Requires:

- Customer or authorized representative withdrawal evidence.
- Withdrawal reason.
- Treatment of active submissions and Commands.
- Audit evidence.

### Entering EXPIRED

Requires:

- Applicable application, Consent, document, verification, Decision, offer, contracting, or funding validity period ended.
- No accepted progression before expiration.
- Expiration reason and timestamp.

### Entering CANCELLED

Requires:

- Authorized cancellation Decision or policy.
- Valid reason.
- Review of active Lender submissions.
- Review of contract and funding state.
- Required Customer communication.
- Audit evidence.

### Terminal States

For ordinary processing:

- `FUNDED`
- `DECLINED`
- `WITHDRAWN`
- `CANCELLED`
- `ARCHIVED`

`EXPIRED` may be reopened only through an approved revalidation or new-version workflow.

### Correction, Resubmission, and Reversal

Correcting or reopening a material Finance Application outcome requires:

- Authorized Human Decision.
- Correction or reopening reason.
- Supporting evidence.
- New application version where applicable.
- New Lender submission where applicable.
- Contract, Deal, and funding reconciliation.
- New Events.
- Preserved original history.

A funding reversal requires:

- Authoritative reversal evidence.
- Lender or banking reference.
- Deal and accounting review.
- Customer-impact review.
- Reconciliation.
- Human escalation.
- New immutable Event.

AI Agents must not independently reopen, correct, resubmit, or reverse finance outcomes.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
- Consent references.
- Application version.
- Lender Decision.
- Human Decision.
- Automation-policy reference where applicable.
- Evidence.
- Record version.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Command.
- Related External Confirmation.

---

## 7. Relationships

### Tenant

- Every Finance Application belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant processing requires an approved and auditable legal and technical mechanism.

### Customer

- Every Finance Application references one primary Customer.
- Applicant records may reference additional Customers or organizations.
- Customer identity remains governed by Customer Domain Service.
- Finance-specific data must not become unrestricted Customer-profile data.

### Lead and Qualified Lead

- Finance interest may originate from Lead or Qualified Lead.
- Finance interest does not create a Finance Application.
- Qualification does not prove finance eligibility or approval.

### Opportunity

- Every sales-related Finance Application references one Opportunity.
- Opportunity owns the active commercial pursuit.
- Finance status may influence Opportunity progression.
- Finance Application must not directly mark the Opportunity `WON`.

### Quotation

- Finance Application references the exact Quotation version used.
- Quotation owns Customer-facing vehicle commercial terms.
- Finance Application owns the formal credit request based on those terms.
- A material Quotation change may require a new application version and Lender submission.

### Vehicle

- Finance Application references one Vehicle identity or approved configuration.
- Vehicle identity remains governed by Vehicle.
- Lender approval may be Vehicle-specific.
- Vehicle change after submission may require re-underwriting.

### Inventory Record

- Physical-stock finance applications may reference one Inventory Record.
- Inventory Record owns availability, Reservation, Allocation, sale, and delivery context.
- Finance approval does not reserve or allocate Inventory.
- Stale Inventory context must be revalidated before contracting or funding where required.

### Trade-In

- Finance Application may reference one or more permitted Trade-In projections.
- Trade-In owns appraisal, payoff, ownership, and acquisition workflow.
- Trade-In equity used for finance must preserve source and freshness.
- A payoff change may require recalculation and resubmission.

### Appointment

- Finance consultations or document collection may be coordinated through Appointment.
- Appointment completion does not prove application submission, approval, contract signature, or funding.

### Financial Contract

- One Customer-selected Lender offer may create one or more governed Financial Contract versions.
- Financial Contract owns contractual terms, signatures, activation, and voiding.
- Finance Application stores only necessary projections and references.
- Signed contract does not automatically prove funding.

### Deal

- Finance Application may support one primary Deal.
- Deal owns the governed automotive transaction.
- Deal must preserve the selected Lender Decision and Financial Contract references.
- Finance Application `FUNDED` does not independently confirm Vehicle delivery.

### Interaction

Interactions may provide:

- Applicant invitation.
- Consent request.
- Document request.
- Lender-offer presentation.
- Customer selection.
- Customer withdrawal.
- Condition follow-up.
- Funding communication.

Original communication evidence remains governed by Interaction and its provider.

### Lender

A Lender relationship must preserve:

- Approved Lender identifier.
- Regulatory or contractual eligibility.
- Supported finance programs.
- Submission endpoint.
- Data-sharing scope.
- Security requirements.
- External references.
- Decision and funding authority.

### Credit Bureau

A Credit Bureau relationship must preserve:

- Approved provider.
- Purpose.
- Applicant.
- Consent.
- Request.
- Response.
- Report reference.
- Integrity hash.
- Retention and access restrictions.

### Document Service

Finance documents should remain in controlled document storage.

Finance Application stores:

- Document identifiers.
- Types.
- Statuses.
- Verification outcomes.
- Hashes.
- References.

### Compliance Case

A Finance Application may reference a compliance or fraud-review case.

Case details must remain restricted to authorized roles.

### Funding Authority

Funding evidence may come from:

- Lender platform.
- Bank.
- Payment processor.
- DMS.
- Accounting platform.
- Another configured authoritative system.

ASOS must preserve the source authority and External Confirmation.

### Supporting Child Records

Finance Application may own or govern:

- Applicant records.
- Employment records.
- Income records.
- Financial-position records.
- Consent records.
- Verification records.
- Document requirements.
- Credit-bureau requests.
- Affordability calculations.
- Application versions.
- Lender submissions.
- Lender Decisions.
- Underwriting conditions.
- Offer presentations.
- Customer selections.
- Contracting handoffs.
- Funding requirements.
- Funding projections.
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

The following are required Finance Application Event concepts and do not replace the Event Catalog.

### Creation and Applicant Event Concepts

- Finance Application created.
- Applicant added.
- Applicant removed through governed revision.
- Applicant information updated.
- Joint application structure completed.
- Corporate Applicant structure completed.
- Applicant identity conflict detected.

### Consent Event Concepts

- Finance Consent requested.
- Finance Consent granted.
- Finance Consent declined.
- Finance Consent withdrawn.
- Finance Consent expired.
- Finance Consent revalidation required.
- Finance processing suspended.

### Document and Verification Event Concepts

- Finance documents requested.
- Finance document received.
- Finance document verified.
- Finance document rejected.
- Finance document expired.
- Identity verification requested.
- Identity verification completed.
- Income verification completed.
- Employment verification completed.
- Verification conflict detected.
- Human Review requested.

### Credit-Bureau Event Concepts

- Credit-bureau request authorized.
- Credit-bureau request Command sent.
- Credit-bureau report received.
- Credit-bureau request failed.
- Credit report expired.
- Credit report disputed.
- Credit-bureau reconciliation required.

### Application-Version Event Concepts

- Finance Application version created.
- Finance Application version validated.
- Finance Application version frozen.
- Finance Application material change detected.
- Finance Application resubmission required.

### Affordability and Eligibility Event Concepts

- Affordability calculation completed.
- Affordability calculation failed.
- Lender eligibility evaluated.
- Lender path identified.
- Finance compliance block applied.
- Finance compliance block cleared.

### Lender Submission Event Concepts

- Lender submission requested.
- Lender submission Command sent.
- Lender submission received by provider.
- Lender submission confirmed by Lender.
- Lender submission rejected.
- Lender submission failed.
- Lender submission reconciliation required.

### Lender Decision Event Concepts

- Lender requested more information.
- Lender referred application.
- Lender conditionally approved application.
- Lender approved application.
- Lender declined application.
- Lender Decision expired.
- Lender Decision withdrawn.
- Lender Decision disputed.
- Lender Decision corrected by Lender.

### Underwriting Condition Event Concepts

- Underwriting condition created.
- Underwriting evidence requested.
- Underwriting evidence submitted.
- Underwriting condition confirmed satisfied.
- Underwriting condition rejected.
- Underwriting condition waived by Lender.
- Underwriting condition expired.

### Customer Offer Event Concepts

- Finance offers ready for presentation.
- Finance offer presented.
- Customer selected finance offer.
- Customer rejected finance offers.
- Customer requested changes.
- Customer withdrew Finance Application.
- Customer selection disputed.

### Contracting Event Concepts

- Financial Contract creation requested.
- Financial Contract created.
- Financial Contract creation failed.
- Financial Contract signature status updated.
- Contract reconciliation required.

### Funding Event Concepts

- Funding readiness evaluated.
- Funding request created.
- Funding Command sent.
- Partial funding confirmed.
- Funding confirmed.
- Funding failed.
- Funding reversed.
- Funding reconciliation required.

### Closure Event Concepts

- Finance Application declined.
- Finance Application withdrawn.
- Finance Application expired.
- Finance Application cancelled.
- Finance Application disputed.
- Finance Application reopened.
- Finance Application archived.

### Derived Intelligence Event Concepts

- Finance Application completion prediction updated.
- Finance approval probability generated.
- Funding-delay risk detected.
- Finance document Recommendation generated.
- Lender submission-order Recommendation generated.
- Finance next action recommended.
- Finance Human Review recommended.

Derived Intelligence Events must not imply:

- Valid Consent.
- Verified identity.
- Verified income.
- Credit-bureau result.
- Lender submission.
- Lender approval.
- Customer selection.
- Contract signature.
- Funding.
- Human Approval.
- External completion.

### Producer Rules

- Finance Application Domain Service publishes accepted canonical and workflow-state changes.
- Customer Domain Service publishes accepted Customer identity changes.
- Quotation Domain Service publishes accepted Quotation facts.
- Vehicle and Inventory Domain Services publish accepted Vehicle and Inventory facts.
- Trade-In Domain Service publishes accepted Trade-In facts.
- Financial Contract Domain Service publishes accepted contract facts.
- Integration services publish normalized Lender, Bureau, banking, and provider observations.
- Lenders remain authoritative for underwriting Decisions.
- Funding authorities remain authoritative for funding outcomes.
- AI Agents may publish Agent-run, extraction, analysis, prediction, or Recommendation Events.
- AI Agents must not publish authoritative Consent, verification, Lender Decision, contract, funding, or external-completion Events merely because they predicted or recommended the result.

### Event Requirements

Every material Finance Application Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `finance_application_id`.
- Application version.
- Applicant identifier.
- Customer identifier.
- Opportunity, Quotation, Vehicle, Trade-In, Deal, and Contract references.
- Lender and finance-program references.
- Lender submission and Decision references.
- Dealership and branch context.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Consent references.
- Evidence references.
- Calculation reference.
- Applied policy.
- Human Decision.
- Automation-policy reference where applicable.
- Command.
- External Confirmation.
- Security classification.

Events are immutable.

Corrections, withdrawal, resubmission, expiration, Decision correction, funding reversal, and reopening must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Document classification.
- Document-field extraction.
- Missing-document detection.
- Application-completeness analysis.
- Applicant-information consistency checks.
- Income-document summarization.
- Employment-evidence summarization.
- Commercial-snapshot explanation.
- Underwriting-condition summarization.
- Lender-offer comparison.
- Customer-language adaptation.
- Customer-facing explanation drafting.
- Approval-probability estimation.
- Funding-delay risk analysis.
- Fraud-anomaly detection.
- Data-quality issue detection.
- Next-action Recommendation.
- Human Review preparation.
- Reconciliation-case summarization.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create or withdraw Consent.
- Verify identity.
- Verify income.
- Verify employment.
- Verify source of funds.
- Approve a credit-bureau request.
- Create an authoritative credit score.
- Make a Lender Decision.
- Approve or decline finance.
- Alter Lender terms.
- Remove Lender conditions.
- Extend Lender approval validity.
- Select an offer for the Customer.
- Sign a Financial Contract.
- Confirm Customer Payment.
- Confirm Lender funding.
- Approve adverse action.
- Reverse a compliance block.
- Execute external Commands directly.
- Access Finance Application data outside authorized Tenant and purpose scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Finance Application identifier.
- Application version.
- Applicant scope.
- Supporting evidence.
- Source authority.
- Input freshness.
- Applied Business Rules.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Fairness or risk review where applicable.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority.

### Document Extraction

AI-extracted values remain Derived Intelligence until accepted through an approved verification workflow.

AI must distinguish:

- Extracted value.
- Source document.
- Document page or region.
- Applicant-declared value.
- Verified value.
- Conflicting value.
- Missing value.
- Unreadable value.

AI must not transform extraction into verification.

### Approval Probability

AI may estimate approval probability for internal workflow assistance.

It must not be represented as:

- Lender prequalification.
- Lender approval.
- Guaranteed funding.
- Customer-facing commitment.

The output must preserve:

- Model version.
- Applicable population.
- Input features.
- Excluded protected attributes.
- Limitations.
- Confidence where meaningful.
- Expiration.
- Required Human interpretation.

A probability score must not determine adverse action without approved legal, policy, fairness, and Human controls.

### Lender Recommendation

AI may recommend an order for permitted Lender submissions.

The Recommendation must consider:

- Approved Lender eligibility.
- Customer permission.
- Product compatibility.
- Vehicle eligibility.
- Requested term.
- Application completeness.
- Data-sharing minimization.
- Submission limits.
- Applicable fairness controls.

AI must not send Applicant data to a Lender.

The deterministic Policy Engine and authorized workflow must approve the submission.

### Offer Comparison

AI may summarize valid Lender offers.

The comparison must include material differences such as:

- Approved amount.
- Down payment.
- Rate.
- APR or equivalent.
- Payment.
- Frequency.
- Term.
- Balloon or residual.
- Fees.
- Conditions.
- Expiration.
- Total cost where authoritative and applicable.

AI must not hide unfavorable terms or invent comparative values.

### Customer-Facing Drafting

AI may draft finance-related Customer communication only when:

- The Customer communication purpose is permitted.
- Required Consent or lawful basis exists.
- Authoritative Lender values are provided.
- Required disclosures are supplied.
- Sensitive internal information is excluded.
- Human Approval or applicable automation policy is satisfied.

AI must not claim:

- Likely approval as confirmed approval.
- Conditional approval as final approval.
- Contract preparation as contract signature.
- Funding request as funding received.
- A lower payment without authoritative calculation.
- A Lender reason not supplied by the Lender.

### Action Class 2

Controlled finance-document requests, status updates, reminders, and offer-delivery activity may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Applicant and Customer permission.
- Purpose.
- Channel.
- Template.
- Frequency.
- Quiet hours.
- Finance Application state.
- Document sensitivity.
- Lender disclosure requirements.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision or External Authoritative Decision.

Examples include:

- Lender submission authorization.
- Manual affordability exception.
- Compliance clearance.
- Customer offer presentation approval where required.
- Customer-selected offer acceptance processing.
- Financial Contract authorization.
- Funding request.
- Funding-shortfall resolution.
- Application cancellation after approval.
- Disputed Decision handling.
- Funding reversal handling.

A dealership Human Decision cannot replace the Lender’s authoritative underwriting Decision.

### Fairness and Responsible AI

AI used in finance workflows must:

- Avoid unlawful discrimination.
- Avoid inappropriate protected-attribute proxies.
- Use only approved features.
- Preserve model and feature versions.
- Preserve explanation and limitations.
- Undergo monitoring for drift and disparate impact where applicable.
- Support Human and compliance review.
- Be disabled when governance requirements are not met.

AI must not use unrelated Customer behaviour, protected health information, political views, religion, ethnicity, or another prohibited attribute to influence finance Recommendations.

### AI Context and Embeddings

Finance data must not enter unrestricted embeddings.

Normally excluded fields include:

- Legal name.
- Date of birth.
- National identifier.
- Address.
- Phone.
- Email.
- Income.
- Employment.
- Assets.
- Liabilities.
- Bank statements.
- Credit score.
- Credit report.
- Credit-bureau reference.
- Lender Decision details.
- Consent evidence.
- Identity documents.
- Source-of-funds evidence.
- Fraud indicators.
- Contract documents.
- Payment and funding details.

Approved redacted context may include:

- Application status.
- Missing-document categories.
- General condition categories.
- Non-sensitive workflow summary.
- General next-action category.
- Redacted Lender-condition summary where authorized.

Every vector record must enforce:

- `tenant_id`.
- Finance Application access scope.
- Applicant purpose scope.
- Source references.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Explainability

Material Finance Recommendations must explain:

- Evidence used.
- Authority of each input.
- Application version.
- Data freshness.
- Missing information.
- Material conflicts.
- Consent state.
- Verification state.
- Calculation method.
- Model limitations.
- Fairness controls where applicable.
- Lender authority boundary.
- Required Human authority.
- External Confirmation requirements.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Finance Application API behaviour.

### REST Resources

```text
GET    /api/v1/finance-applications
POST   /api/v1/finance-applications
GET    /api/v1/finance-applications/{finance_application_id}
PATCH  /api/v1/finance-applications/{finance_application_id}

POST   /api/v1/finance-applications/{finance_application_id}/applicants
PATCH  /api/v1/finance-applications/{finance_application_id}/applicants/{finance_applicant_id}

POST   /api/v1/finance-applications/{finance_application_id}/consent-requests
POST   /api/v1/finance-applications/{finance_application_id}/consent-submissions
POST   /api/v1/finance-applications/{finance_application_id}/document-requests
POST   /api/v1/finance-applications/{finance_application_id}/document-submissions
POST   /api/v1/finance-applications/{finance_application_id}/verification-requests
POST   /api/v1/finance-applications/{finance_application_id}/credit-bureau-requests
POST   /api/v1/finance-applications/{finance_application_id}/affordability-calculation-requests
POST   /api/v1/finance-applications/{finance_application_id}/version-requests

POST   /api/v1/finance-applications/{finance_application_id}/lender-submission-requests
POST   /api/v1/finance-applications/{finance_application_id}/underwriting-condition-submissions
POST   /api/v1/finance-applications/{finance_application_id}/offer-presentation-requests
POST   /api/v1/finance-applications/{finance_application_id}/customer-selection-submissions
POST   /api/v1/finance-applications/{finance_application_id}/withdrawal-requests
POST   /api/v1/finance-applications/{finance_application_id}/cancellation-requests

POST   /api/v1/finance-applications/{finance_application_id}/contract-creation-requests
POST   /api/v1/finance-applications/{finance_application_id}/funding-readiness-checks
POST   /api/v1/finance-applications/{finance_application_id}/funding-requests
POST   /api/v1/finance-applications/{finance_application_id}/dispute-requests
POST   /api/v1/finance-applications/{finance_application_id}/correction-requests
POST   /api/v1/finance-applications/{finance_application_id}/reopen-requests

GET    /api/v1/finance-applications/{finance_application_id}/versions
GET    /api/v1/finance-applications/{finance_application_id}/consent-history
GET    /api/v1/finance-applications/{finance_application_id}/document-status
GET    /api/v1/finance-applications/{finance_application_id}/verification-history
GET    /api/v1/finance-applications/{finance_application_id}/lender-submissions
GET    /api/v1/finance-applications/{finance_application_id}/lender-decisions
GET    /api/v1/finance-applications/{finance_application_id}/funding-history
GET    /api/v1/finance-applications/{finance_application_id}/history
GET    /api/v1/finance-applications/{finance_application_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, finance team, User, Lender, and program scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "quotation_version": 1,
  "quotation_document_hash": "sha256:8ac44d5d...",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "application_type": "JOINT",
  "finance_product_type": "RETAIL_FINANCE",
  "submission_channel": "DEALERSHIP",
  "currency_code": "EGP",
  "requested_terms": {
    "requested_down_payment_amount": 500000,
    "requested_finance_amount": 1710000,
    "requested_term_months": 60,
    "preferred_periodic_payment_amount": 45000,
    "maximum_periodic_payment_amount": 50000,
    "payment_frequency": "MONTHLY"
  }
}
```

The request must include:

```text
Idempotency-Key: 358ca7da-4ea2-4da8-9c10-1479dbe2fb1e
```

### Example Create Response

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "application_number": "FA-2026-000319",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "status": "DRAFT",
  "application_type": "JOINT",
  "finance_product_type": "RETAIL_FINANCE",
  "current_application_version": 1,
  "document_completion_status": "NOT_STARTED",
  "affordability_status": "NOT_ASSESSED",
  "funding_status": "NOT_STARTED",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T19:30:00Z"
}
```

### Example Applicant Request

```json
{
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "applicant_role": "PRIMARY_APPLICANT",
  "applicant_type": "INDIVIDUAL",
  "national_identifier_token": "tok_id_01JABCD123",
  "residency_status": "RESIDENT",
  "employment": {
    "employment_status": "EMPLOYED",
    "employer_name": "Example Automotive Group",
    "job_title": "Operations Manager",
    "declared_income_amount": 85000,
    "income_currency_code": "EGP",
    "income_frequency": "MONTHLY"
  },
  "financial_position": {
    "monthly_housing_cost_amount": 12000,
    "monthly_debt_payment_amount": 8000,
    "monthly_other_commitment_amount": 2000,
    "available_down_payment_amount": 500000
  },
  "expected_record_version": 1
}
```

### Example Version-Creation Request

```json
{
  "reason": "UPDATED_VERIFIED_INCOME_AND_DOWN_PAYMENT",
  "expected_current_application_version": 1,
  "expected_record_version": 6
}
```

A successful response may return:

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "current_application_version": 2,
  "version_snapshot_hash": "sha256:5ea92db4...",
  "record_version": 7
}
```

### Example Lender Submission Request

```json
{
  "lender_id": "fa35ccdb-dd66-49e0-8214-7a1f4b2e95fc",
  "finance_program_id": "cc33bbde-7c19-4f73-90a7-6e6234959ae7",
  "application_version": 2,
  "expected_application_version_hash": "sha256:5ea92db4...",
  "expected_record_version": 9
}
```

The request must include an idempotency key.

The initial response may remain pending:

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "lender_submission_id": "086a44b6-daa1-4d10-a478-d7e306944d1e",
  "submission_status": "PENDING_CONFIRMATION",
  "application_version": 2,
  "command_id": "33ed2d8f-af19-43f7-ba3e-4541ca27c87c",
  "record_version": 10
}
```

The API must not describe the submission as received by the Lender until authoritative Confirmation exists.

### Example Lender Decision Projection

```json
{
  "lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "lender_submission_id": "086a44b6-daa1-4d10-a478-d7e306944d1e",
  "lender_id": "fa35ccdb-dd66-49e0-8214-7a1f4b2e95fc",
  "decision_version": "LENDER-DECISION-4",
  "decision_status": "CONDITIONALLY_APPROVED",
  "approved_finance_amount": 1650000,
  "approved_down_payment_amount": 560000,
  "approved_interest_rate": 18.5,
  "approved_annual_percentage_rate": 20.1,
  "approved_term_months": 60,
  "approved_periodic_payment_amount": 43800,
  "approved_payment_frequency": "MONTHLY",
  "decision_valid_until": "2026-08-15T23:59:59Z",
  "outstanding_conditions": [
    {
      "condition_type": "UPDATED_INCOME_EVIDENCE",
      "status": "OPEN"
    }
  ],
  "decision_confirmation_status": "RECEIVED",
  "decision_artifact_hash": "sha256:7d0c86d1..."
}
```

### Example Customer Selection

```json
{
  "selected_lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "selected_offer_version": "LENDER-DECISION-4",
  "selected_offer_snapshot_hash": "sha256:60dc3c44...",
  "selection_channel": "CUSTOMER_PORTAL",
  "selection_evidence_reference": "evidence://finance-selections/7f62224a",
  "expected_record_version": 14
}
```

### Example Contract-Creation Response

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "status": "CONTRACTING",
  "contract_preparation_status": "PENDING_EXTERNAL_CONFIRMATION",
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "command_id": "cd74caa5-2c6c-4a33-90c7-fd74f20b6550",
  "record_version": 16
}
```

The API must not claim that the contract is signed.

### Example Funding Request

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "funding_amount_requested": 1650000,
  "currency_code": "EGP",
  "expected_record_version": 18
}
```

The request must use an idempotency key.

A pending response may be:

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "status": "FUNDING_PENDING",
  "funding_status": "PENDING_CONFIRMATION",
  "command_id": "506c091f-b925-4bb3-9189-d34e0937c985",
  "record_version": 19
}
```

A confirmed response may be:

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "status": "FUNDED",
  "funding_status": "FUNDED",
  "funded_amount": 1650000,
  "funding_currency_code": "EGP",
  "funding_received_at": "2026-08-12T10:30:00Z",
  "funding_confirmation_status": "RECEIVED",
  "funding_reconciliation_status": "RESOLVED",
  "record_version": 22
}
```

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Application-version validation.
- Field-authority validation.
- Consent validation.
- Identity and document controls.
- Commercial-snapshot validation.
- Deterministic calculations.
- Lender and program eligibility.
- Lifecycle validation.
- Required Human Decision.
- Idempotency where required.
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

Retryable operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate:

- Finance Applications.
- Application versions.
- Applicants.
- Bureau requests.
- Lender submissions.
- Customer selections.
- Financial Contracts.
- Funding requests.
- Funding transactions.

### Pending External Confirmation

Operations requiring an external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "command_id": "33ed2d8f-af19-43f7-ba3e-4541ca27c87c",
  "record_version": 10
}
```

The API must not describe the action as completed until authoritative evidence exists.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `APPLICATION_VERSION_CONFLICT`
- `CUSTOMER_MISMATCH`
- `OPPORTUNITY_NOT_ELIGIBLE`
- `QUOTATION_INVALID`
- `QUOTATION_VERSION_MISMATCH`
- `VEHICLE_NOT_ELIGIBLE`
- `INVENTORY_DATA_STALE`
- `APPLICANT_INCOMPLETE`
- `CO_APPLICANT_REQUIRED`
- `CONSENT_REQUIRED`
- `CONSENT_WITHDRAWN`
- `CONSENT_EXPIRED`
- `IDENTITY_VERIFICATION_REQUIRED`
- `DOCUMENTS_INCOMPLETE`
- `DOCUMENT_VERIFICATION_FAILED`
- `CREDIT_BUREAU_ACCESS_NOT_PERMITTED`
- `AFFORDABILITY_CALCULATION_FAILED`
- `COMPLIANCE_REVIEW_REQUIRED`
- `LENDER_NOT_ELIGIBLE`
- `FINANCE_PROGRAM_NOT_ELIGIBLE`
- `LENDER_SUBMISSION_DUPLICATE`
- `LENDER_DECISION_EXPIRED`
- `LENDER_CONDITIONS_OUTSTANDING`
- `CUSTOMER_SELECTION_INVALID`
- `CONTRACT_NOT_READY`
- `CONTRACT_NOT_SIGNED`
- `FUNDING_NOT_READY`
- `FUNDING_SHORTFALL`
- `HUMAN_APPROVAL_REQUIRED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `APPLICATION_IMMUTABLE`
- `APPLICATION_TERMINAL`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Applicant-purpose scope.
- Consent controls.
- Field authority.
- Application-version immutability.
- Deterministic calculation.
- Lender authority.
- Concurrency.
- Idempotency.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Finance Application Domain Service, Policy Engine, Consent controls, or approved Calculation Services.

---

## 11. Database Design

### Recommended Tables

```text
finance_applications
finance_application_versions
finance_applicants
finance_applicant_employment
finance_applicant_income
finance_applicant_financial_position
finance_corporate_applicants
finance_consent_records
finance_verification_records
finance_document_requirements
finance_document_records
finance_credit_bureau_requests
finance_affordability_calculations
finance_compliance_reviews
finance_lender_eligibility
finance_lender_submissions
finance_lender_decisions
finance_underwriting_conditions
finance_offer_presentations
finance_customer_selections
finance_contract_handoffs
finance_funding_requirements
finance_funding_requests
finance_funding_confirmations
finance_external_references
finance_external_confirmations
finance_derived_intelligence
finance_reconciliation_cases
finance_data_quality_issues
finance_status_history
finance_record_versions
finance_audit_log
```

### Finance Applications Table

The `finance_applications` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Customer and Opportunity relationships.
- Current Quotation, Vehicle, Inventory, Trade-In, Deal, and Contract references.
- Current aggregate lifecycle state.
- Current Applicant structure.
- Current application version.
- Current Consent, document, verification, and affordability projections.
- Current Lender-submission projection.
- Current selected Lender Decision.
- Current Customer selection.
- Current contract and funding projections.
- Data-quality and conflict state.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Historical and repeating information must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (finance_application_id)
```

### Tenant Protection

Every Finance-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_finance_applications_tenant_status
  (tenant_id, status)

idx_finance_applications_tenant_customer
  (tenant_id, customer_id)

idx_finance_applications_tenant_opportunity
  (tenant_id, opportunity_id)

idx_finance_applications_tenant_quotation
  (tenant_id, quotation_id)

idx_finance_applications_tenant_vehicle
  (tenant_id, vehicle_id)

idx_finance_applications_tenant_deal
  (tenant_id, deal_id)

idx_finance_applications_assigned_user
  (tenant_id, assigned_finance_user_id, status)

idx_finance_applications_document_status
  (tenant_id, document_completion_status)

idx_finance_applications_submission_status
  (tenant_id, latest_submission_status)

idx_finance_applications_decision_expiry
  (tenant_id, decision_valid_until)

idx_finance_applications_funding
  (tenant_id, funding_status)

idx_finance_applications_reconciliation
  (tenant_id, reconciliation_status)

idx_finance_applications_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, application_number)
```

External source uniqueness may use:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the external source guarantees uniqueness.

One application version number must be unique inside one Finance Application:

```text
UNIQUE (
  tenant_id,
  finance_application_id,
  application_version
)
```

One Lender submission idempotency key must be unique per protected operation:

```text
UNIQUE (
  tenant_id,
  lender_id,
  submission_idempotency_key
)
```

An external Lender application reference should be unique where the Lender guarantees uniqueness:

```text
UNIQUE (
  tenant_id,
  lender_id,
  lender_application_reference
)
```

### Duplicate Application Detection

Potential duplicate active Finance Applications should be evaluated using:

- Customer.
- Applicant set.
- Opportunity.
- Vehicle.
- Quotation.
- Lender.
- Finance product.
- Timeframe.
- Existing Deal.
- Existing active Contract.

Customer alone must not be used as a uniqueness constraint because one Customer may have multiple legitimate applications over time.

Ambiguous duplicate cases require Human Review.

### Application Versions

`finance_application_versions` should preserve:

- Version identifier.
- `tenant_id`.
- Finance Application.
- Application version.
- Applicant snapshot.
- Commercial snapshot.
- Consent snapshot.
- Verification snapshot.
- Document snapshot.
- Affordability snapshot.
- Source-record versions.
- Creation reason.
- Creator.
- Creation timestamp.
- Superseded version.
- Snapshot hash.
- Submission eligibility.
- Related Events.

Submitted versions must be immutable.

### Applicant Storage

`finance_applicants` should preserve:

- Applicant identifier.
- Finance Application.
- Customer or organization reference.
- Role.
- Type.
- Identity projection.
- Verification state.
- Participation state.
- Effective period.
- Record version.
- Related Events.

Sensitive Applicant details should be separated by security classification and purpose.

### Consent Storage

`finance_consent_records` should preserve:

- Consent identifier.
- Finance Application.
- Applicant.
- Consent type.
- Purpose.
- Permitted recipients.
- Terms version.
- Status.
- Capture channel.
- Artifact reference.
- Artifact hash.
- Effective time.
- Expiration.
- Withdrawal.
- Dispute state.
- Related Events.

Consent records must be append-only or versioned.

### Document Storage

`finance_document_records` should preserve:

- Document record identifier.
- Applicant.
- Finance Application.
- Document type.
- Controlled storage reference.
- Integrity hash.
- Issue date.
- Expiration.
- Source.
- Verification state.
- Verification authority.
- Security classification.
- Retention class.
- Legal-hold state.
- Related Events.

Raw documents must not be stored in unrestricted relational text fields.

### Credit-Bureau Storage

`finance_credit_bureau_requests` should preserve:

- Request identifier.
- Applicant.
- Finance Application.
- Bureau.
- Purpose.
- Consent record.
- Command.
- Idempotency key.
- Request time.
- Response time.
- Provider reference.
- Report reference.
- Report hash.
- Score projection where permitted.
- Expiration.
- Dispute state.
- Reconciliation state.
- Related Events.

### Affordability Storage

`finance_affordability_calculations` should preserve:

- Calculation identifier.
- Finance Application.
- Application version.
- Formula version.
- Rule versions.
- Input references.
- Input hash.
- Output values.
- Currency normalization.
- Frequency normalization.
- Rounding method.
- Result.
- Generated time.
- Expiration.
- Snapshot hash.
- Related Events.

### Lender Submission Storage

`finance_lender_submissions` should preserve:

- Submission identifier.
- Finance Application.
- Application version.
- Snapshot hash.
- Lender.
- Program.
- Status.
- Requested by.
- Human Decision where applicable.
- Command.
- Idempotency key.
- External reference.
- External Confirmation.
- Requested time.
- Submitted time.
- Response time.
- Failure reason.
- Reconciliation status.
- Related Events.

### Lender Decision Storage

`finance_lender_decisions` should preserve:

- Decision identifier.
- Lender submission.
- Lender.
- Program.
- Decision version.
- Decision status.
- Decision values.
- Conditions.
- Effective time.
- Expiration.
- Source artifact.
- Artifact hash.
- External Confirmation.
- Correction or supersession reference.
- Dispute state.
- Related Events.

Lender Decisions must be immutable.

Corrections require a new Decision record linked to the original.

### Underwriting Conditions

`finance_underwriting_conditions` should preserve:

- Condition identifier.
- Lender Decision.
- Original condition type and wording.
- Required evidence.
- Status.
- Submitted evidence.
- Submission time.
- Lender Confirmation.
- Satisfaction or waiver time.
- Expiration.
- Dispute state.
- Related Events.

### Customer Selection Storage

`finance_customer_selections` should preserve:

- Selection identifier.
- Finance Application.
- Customer.
- Selected Lender Decision.
- Decision version.
- Offer snapshot hash.
- Presentation evidence.
- Selection evidence.
- Identity-verification context.
- Selected time.
- Rejection or change request.
- Dispute state.
- Related Events.

### Contracting Handoff Storage

`finance_contract_handoffs` should preserve:

- Handoff identifier.
- Finance Application.
- Selected Lender Decision.
- Customer-selected offer.
- Financial Contract.
- Requested by.
- Human Decision.
- Command.
- Idempotency key.
- External Confirmation.
- Status.
- Failure reason.
- Related Events.

### Funding Storage

`finance_funding_requests` and `finance_funding_confirmations` should preserve:

- Funding identifier.
- Finance Application.
- Lender Decision.
- Financial Contract.
- Deal.
- Requested amount.
- Currency.
- Funding requirements.
- Command.
- Idempotency key.
- Requested time.
- Confirmed amount.
- Confirmed time.
- External authority.
- External Confirmation.
- Shortfall.
- Reversal.
- Reconciliation.
- Related Events.

### Derived Intelligence

Derived Finance records must remain separate from authoritative Applicant, Bureau, Lender Decision, contract, and funding data.

Each derived record should preserve:

- Output type.
- Value.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Confidence.
- Assumptions.
- Fairness-review reference where applicable.
- Generated time.
- Expiration time.
- Review status.

### Audit Storage

Finance audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw personal, financial, credit, or document values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Creation date.
- Submission date.
- Lender.
- Retention class.
- Security classification.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Applicant privacy.
- Application-version integrity.
- Lender-submission idempotency.
- Decision immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Finance Application must not be hard-deleted when referenced by:

- Customer journey.
- Opportunity.
- Quotation.
- Vehicle.
- Inventory Record.
- Trade-In.
- Deal.
- Financial Contract.
- Payment or funding.
- Lender submission.
- Credit-bureau request.
- Consent evidence.
- Document evidence.
- Interaction.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Compliance evidence.
- Audit evidence.

Withdrawal, cancellation, expiry, archival, anonymization, governed redaction, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Legal name, date of birth, phone, email |
| `HIGHLY_SENSITIVE_IDENTIFIER` | National identifier and identity-document references |
| `FINANCIAL_RESTRICTED` | Income, expenses, assets, liabilities, down payment |
| `CREDIT_RESTRICTED` | Credit report, score, bureau reference |
| `EMPLOYMENT_RESTRICTED` | Employer, job, employment evidence |
| `CONSENT_AND_AUTHORIZATION` | Finance Consent and recipient authorization |
| `LENDER_CONFIDENTIAL` | Underwriting Decision, reasons, conditions |
| `CONTRACT_RESTRICTED` | Financial Contract and signature projection |
| `FUNDING_RESTRICTED` | Funding amount, reference, Confirmation |
| `FRAUD_AND_COMPLIANCE` | Fraud, sanctions, source-of-funds review |
| `DERIVED_INTELLIGENCE` | Approval prediction, risk, Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, version history |

### Authentication

Every internal Finance Application operation requires an authenticated Human or service identity.

Applicant self-service access must use an approved secure authentication or verification mechanism.

Anonymous unrestricted access to Finance Applications, documents, offers, or Decisions is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Finance team.
- Assigned finance User.
- Applicant relationship.
- Customer relationship.
- Opportunity and Deal relationship.
- Lender.
- Role.
- Requested field.
- Requested action.
- Finance Application state.
- Application version.
- Data classification.
- Business purpose.
- Delegated authority.
- Consent and lawful basis.

### Example Role Boundaries

#### Sales Consultant

May access permitted:

- Finance-interest status.
- Application workflow summary.
- Missing non-sensitive action categories.
- Selected Customer-facing offer summary.
- Approved finance status required for the sales journey.

Must not access without explicit authority:

- National identifier.
- Full income details.
- Credit report.
- Credit score.
- Bank statements.
- Detailed Lender decline reasons.
- Fraud findings.
- Internal risk scores.
- Funding instructions.

#### Finance Specialist

May access Finance Applications assigned to the User or permitted team.

May perform permitted:

- Applicant coordination.
- Document collection.
- Verification workflow.
- Lender submission preparation.
- Offer presentation.
- Contracting handoff.
- Funding coordination.

Finance Specialist access does not authorize:

- Altering Lender Decisions.
- Fabricating Consent.
- Bypassing compliance.
- Confirming funding without evidence.
- Cross-Tenant access.

#### Finance Manager

May perform configured:

- Submission authorization.
- Exception review.
- Applicant-structure review.
- Lender-path review.
- Funding-shortfall review.
- Dispute escalation.
- Reconciliation approval.

Manager access does not authorize:

- Lender underwriting override.
- Consent override.
- Credit-report alteration.
- Contract signature.
- False funding Confirmation.

#### Compliance or Legal Reviewer

May access restricted:

- Consent.
- Identity verification.
- Source of funds.
- Beneficial ownership.
- Fraud.
- Sanctions.
- Disputes.
- Adverse-action evidence.

Only the minimum necessary scope may be accessed.

#### Document Verification User

May access assigned document-verification records.

Document access does not grant access to unrelated Lender Decisions or Deal pricing.

#### Data Steward

May review:

- Duplicate applications.
- Relationship inconsistencies.
- Source mappings.
- Version conflicts.
- Data-quality issues.
- Reconciliation cases.

Data Steward access to raw finance values should remain minimized.

#### AI Agent

May access only the minimum Finance Application context required for its approved task.

AI access must be:

- Tenant-scoped.
- Applicant-purpose-scoped.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to identity, bank, credit, Consent, Lender Decision, contract, funding, and fraud information.

#### Integration Service

May access only fields required for an approved Lender, Bureau, verification, document, contract, or funding integration.

Integration services must not access unrelated Applicant data.

### Field-Level Protection

Restricted fields must use:

- Field-level authorization.
- Encryption.
- Tokenization where appropriate.
- Masking.
- Controlled document references.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Restricted examples include:

- National identifier.
- Date of birth.
- Income.
- Assets.
- Liabilities.
- Bank-account references.
- Bureau scores.
- Credit reports.
- Lender Decision details.
- Fraud indicators.
- Source-of-funds evidence.
- Contract and funding references.

### Encryption and Tokenization

- Finance data in transit must use approved encryption.
- Finance data at rest must use approved encryption.
- National identifiers must use tokenization or equivalent protection.
- Banking and funding references should be tokenized where possible.
- Encryption keys must remain outside application source code and Prompts.
- Key access must be logged and controlled.
- Searchable encrypted or tokenized indexes must follow approved security design.

### Document Security

Identity, income, employment, banking, tax, corporate, and credit documents must:

- Use controlled storage.
- Use authenticated access.
- Preserve integrity hashes.
- Preserve source and provenance.
- Prevent predictable identifiers.
- Prevent public indexing.
- Prevent uncontrolled download.
- Be excluded from ordinary Logs.
- Be excluded from general-purpose embeddings.
- Follow retention and legal-hold policy.
- Prevent unapproved model-training use.

### Credit-Bureau Security

Credit-bureau information must:

- Be accessed only for an approved purpose.
- Be restricted to authorized roles.
- Preserve Consent and request evidence.
- Use controlled display and masking.
- Prevent bulk export.
- Prevent use in unrelated marketing.
- Prevent unrestricted AI access.
- Follow provider and legal retention requirements.
- Support dispute and correction workflows.

### Lender Decision Protection

Lender Decisions must:

- Preserve authoritative source.
- Preserve Decision artifact and hash.
- Be immutable.
- Be displayed only to authorized Users.
- Prevent unauthorized modification.
- Prevent unauthorized Customer disclosure.
- Preserve required adverse-action handling.
- Remain distinguishable from ASOS predictions.

### Consent Enforcement

Before bureau access, Lender sharing, document verification, electronic communication, or electronic signature, deterministic controls must validate:

- Applicant.
- Purpose.
- Data categories.
- Recipient.
- Terms version.
- Consent status.
- Effective period.
- Withdrawal state.
- Jurisdiction.
- Tenant policy.

Prompt text, User interface state, sales urgency, or AI Recommendation must not override Consent controls.

### Customer Communication Security

Before finance-related Customer communication, deterministic controls must validate:

- Applicant or Customer permission.
- Purpose.
- Channel.
- Template.
- Finance Application state.
- Lender disclosure requirements.
- Document sensitivity.
- Frequency.
- Quiet hours.
- Human Approval or approved automation policy.

Sensitive data must not be included in insecure message channels.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Applicant matching.
- Duplicate detection.
- Credit-bureau processing.
- Lender submission.
- Search.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Documents.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Finance Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Applicant and purpose scope.
- Requested action.
- Finance Application identifier.
- Application version and hash.
- Consent references.
- Current record version.
- Field-level write authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Finance Application activity must record:

- `tenant_id`.
- `finance_application_id`.
- Application version.
- Applicant identifier.
- Customer, Opportunity, Quotation, Vehicle, Trade-In, Deal, and Contract references.
- Lender and program.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
- Consent references.
- Calculation reference.
- Applied Business Rules.
- Human Decision.
- Automation-policy reference.
- AI involvement.
- Recommendation.
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

- Cross-Tenant Finance Application access attempts.
- Unauthorized identity access.
- Unauthorized income or bank-data access.
- Credit-report access.
- Consent-bypass attempts.
- Unauthorized Lender submission.
- Duplicate bureau request.
- Duplicate Lender submission.
- Lender Decision modification attempt.
- Unauthorized offer presentation.
- False Customer selection.
- Contract-status manipulation.
- False funding Confirmation.
- Funding Command replay.
- External Confirmation mismatch.
- AI access outside approved scope.
- Prompt-injection attempts inside uploaded documents.
- Bulk finance-data export.
- Audit-log tampering.

### Untrusted Documents and Prompt Injection

Finance documents and Applicant-submitted text are untrusted input.

AI Agents must treat them as data, not system instructions.

Document content must not:

- Change system policy.
- Grant tool access.
- Override Tenant scope.
- Reveal secrets.
- Trigger external Commands.
- Bypass Consent.
- Bypass authorization.
- Modify Lender Decisions.
- Alter application state.

### Fairness and Model Governance

Finance-related AI models must be registered and governed.

Governance should include:

- Approved purpose.
- Model owner.
- Training-data review.
- Feature review.
- Protected-attribute review.
- Bias testing.
- Explainability.
- Validation.
- Approval.
- Monitoring.
- Drift detection.
- Incident handling.
- Retirement.

Model output must not be treated as authoritative underwriting.

### Retention and Privacy

Finance Application retention must follow:

- Applicable law.
- Tenant policy.
- Credit-bureau terms.
- Lender agreements.
- Consent state.
- Financial and accounting obligations.
- Contractual requirements.
- Dispute requirements.
- Legal holds.
- Audit requirements.

Privacy workflows must support applicable:

- Access.
- Correction.
- Restriction.
- Export.
- Consent withdrawal.
- Deletion or anonymization.
- Dispute handling.

Privacy actions must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Document stores.
- External providers where lawfully required.
- Backups according to policy.

Required legal, lending, financial, security, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Credit-bureau requests.
- Lender submissions.
- Applicant communications.
- Document requests.
- Electronic-signature requests.
- Contract creation.
- Funding requests.
- External write-back.
- AI finance analysis.
- Finance data export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Deal](./Deal.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Finance Application baseline.

Submitted application versions are immutable.

Lender Decisions remain externally authoritative and must not be altered by ASOS, dealership Users, or AI Agents.

Customer selection of a finance offer does not create a signed Financial Contract.

Finance approval does not prove funding.

A funding request does not prove that funds were received.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
