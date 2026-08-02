# Financial Contract

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Financial Contract Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-02  

---

## 1. Object Purpose

### Business Purpose

The Financial Contract Object represents one governed and legally significant agreement that records the accepted financial obligations, rights, disclosures, security interests, payment terms, and signatures associated with an automotive finance, lease, guarantee, or another approved financial product.

A Financial Contract is created from an exact approved commercial and finance context, including:

- One eligible Finance Application.
- One immutable Finance Application version.
- One selected authoritative Lender Decision.
- One exact Lender Decision version.
- One Customer-selected finance offer.
- One eligible Deal.
- One exact Quotation version.
- One Vehicle or approved Vehicle configuration.
- One applicable Inventory Record where a physical unit is involved.
- Applicable Trade-In and equity information.
- Required Applicant and signatory identities.
- Required disclosures.
- Approved legal template and template version.
- Required Human Decisions.
- Applicable regulatory and compliance evidence.

The Financial Contract provides a governed process for:

- Validating contractual inputs.
- Selecting an approved legal template.
- Generating an immutable contract document.
- Generating required disclosures.
- Presenting contractual documents.
- Collecting acknowledgements.
- Collecting electronic or physical signatures.
- Collecting countersignatures.
- Validating signing authority.
- Determining contractual effectiveness.
- Tracking cooling-off or rescission periods.
- Registering security interests or liens where required.
- Orchestrating governed Lender funding requests after Contract eligibility is established.
- Creating and tracking Funding Commands, External Confirmations, partial funding, shortfalls, failures, reversals, and reconciliation.
- Projecting externally authoritative funding outcomes without inventing or overriding them.
- Preserving the contractual payment schedule.
- Referencing servicing and settlement outcomes.
- Managing amendments.
- Managing cancellation before effectiveness.
- Managing voiding, termination, settlement, or completion.
- Preserving immutable legal and audit evidence.

### Financial Contract Domain Boundary

The Financial Contract owns the governed contractual agreement, its execution lifecycle, and the funding-request workflow that follows valid Contract eligibility.

It does not independently represent:

- Customer identity.
- General marketing Consent.
- A Finance Application.
- A Lender underwriting Decision.
- A Quotation.
- A Vehicle Reservation.
- A Vehicle Allocation.
- A Deal payment.
- The externally authoritative Lender or bank funding outcome.
- A completed Vehicle sale.
- A completed delivery.
- Vehicle registration.
- Payment servicing transactions.
- Accounting completion.

The following boundaries must remain explicit:

```text
Finance Application
  = Applicant data, Consent, verification, Lender submissions,
    underwriting Decisions, selected offer, funding readiness,
    and governed Financial Contract handoff

Financial Contract
  = approved contractual terms, disclosures, documents,
    signatures, effectiveness, governed funding-request orchestration,
    activation, amendments, and legal lifecycle

Deal
  = governed automotive commercial transaction,
    completion gates, and non-owning funding projection

Payment
  = Customer or third-party payment and settlement evidence

Funding Authority
  = authoritative Lender or banking funding outcome

Servicing Authority
  = authoritative installment, arrears, settlement,
    balance, and contract-servicing outcomes
```

The Financial Contract Domain Service owns Funding Command creation, idempotency, pending workflow state, External Confirmation tracking, shortfall handling, reversal handling, and reconciliation.

The configured Lender, bank, or funding authority remains authoritative for whether funding actually occurred.

### Financial Contract and Finance Application Separation

The Finance Application preserves:

- Applicants.
- Applicant information.
- Consent.
- Verification.
- Documents.
- Credit-bureau activity.
- Lender submissions.
- Lender Decisions.
- Customer-selected offer.
- Contract-readiness and funding-readiness assessments.
- The immutable contracting-handoff snapshot.
- Handoff idempotency and acceptance status.
- The resulting Financial Contract reference.
- Read-only Contract and funding projections after handoff.

The Financial Contract accepts the governed handoff and preserves:

- The exact Finance Application version and handoff snapshot.
- The exact selected Lender Decision.
- The exact accepted finance terms.
- The exact contractual parties.
- Required disclosures.
- Contract document versions.
- Signatures.
- Contract effectiveness.
- The governed funding-request workflow.
- Funding Commands and idempotency.
- External funding Confirmation and reconciliation.
- Contract activation.
- Contractual payment schedule.
- Security-interest requirements.
- Amendments.
- Legal termination and completion.

A Financial Contract must not alter the authoritative Lender Decision.

A material mismatch between the Financial Contract, the accepted handoff, or the selected Lender Decision must block generation, signature, effectiveness, funding, and activation.

The Finance Application Domain Service must not create or transmit a Funding Command.

The Financial Contract Domain Service is the sole canonical owner of the funding-request mutation after a valid Financial Contract exists.

### Financial Contract and Quotation Separation

The Quotation owns the issued Customer-facing automotive commercial offer.

The Financial Contract owns the legally significant finance or lease agreement.

The Financial Contract must preserve the exact:

- `quotation_id`.
- Quotation version.
- Quotation document hash.
- Commercial snapshot.
- Vehicle price.
- Fees.
- Taxes.
- Optional products.
- Trade-In allowance.
- Trade-In payoff.
- Customer cash contribution.

A material change to the commercial terms may require:

- A new Quotation version.
- A new Finance Application version.
- New Lender underwriting.
- A new Lender Decision.
- A new Financial Contract version.
- New Customer acknowledgement and signatures.

### Financial Contract and Deal Separation

The Deal owns the overall governed commercial transaction, commercial completion gates, and the funding status needed to determine whether the transaction may complete.

The Financial Contract owns the financial agreement and the governed funding-request workflow associated with that transaction.

A signed Financial Contract does not independently prove:

- Customer down payment.
- Vehicle Reservation.
- Vehicle Allocation.
- Lender funding.
- Vehicle sale posting.
- Vehicle registration.
- Vehicle delivery.
- Deal completion.

The Deal must preserve:

- The Financial Contract reference.
- The current authoritative Financial Contract state.
- A read-only funding-status projection.
- Funding-related completion blockers.
- Funding Confirmation and reconciliation references required for Deal completion.

The Deal must not:

- Create or transmit a Funding Command.
- Own a funding idempotency key.
- Publish authoritative funding-request, funding-confirmed, funding-failed, or funding-reversed Events.
- Treat a request acknowledgement as proof of funding.

The Financial Contract must preserve the associated Deal reference and publish accepted funding-workflow facts.

The configured Lender, bank, or funding authority remains authoritative for the funding outcome.

### Contract Generation and Execution Separation

The following facts must remain distinct:

```text
Contract generated
  ≠ Contract presented

Contract presented
  ≠ Disclosures acknowledged

Disclosures acknowledged
  ≠ Contract signed

One party signed
  ≠ Contract fully signed

Contract fully signed
  ≠ Contract legally effective

Contract effective
  ≠ Lender funded

Lender funded
  ≠ Deal completed

Deal completed
  ≠ Vehicle delivered
```

Every transition requires the applicable evidence and authority.

### Signature and Effectiveness Separation

`FULLY_SIGNED` means all required signature actions were completed and accepted by the configured signature authority.

It does not automatically mean the Financial Contract is effective.

Effectiveness may additionally require:

- Required countersignatures.
- Required notarization or witnessing.
- Required disclosure acknowledgements.
- Valid Lender Decision.
- Valid Vehicle and Deal eligibility.
- Completion of a cooling-off or rescission condition.
- Customer down payment.
- Required insurance.
- Required lien or security registration.
- Required original documents.
- Required External Confirmation.
- Another jurisdiction-specific condition.

`EFFECTIVE` must therefore remain separate from `FULLY_SIGNED`.

### Effectiveness, Funding, and Activation Separation

`EFFECTIVE` means the Financial Contract became legally or operationally effective according to its approved terms and governing policy.

`FUNDING_PENDING` means the Financial Contract Domain Service created or transmitted a governed Funding Command and is awaiting an authoritative outcome or reconciliation.

`PARTIALLY_FUNDED`, `FUNDED`, `FAILED`, and `REVERSED` are funding-workflow sub-statuses. They do not independently replace the aggregate Financial Contract lifecycle state.

`ACTIVE` means the Financial Contract is effective and all configured activation conditions have been authoritatively confirmed.

For products requiring Lender funding, activation normally requires accepted authoritative funding Confirmation and resolved reconciliation.

For products not requiring an external funding event, activation requirements must remain configurable.

The Financial Contract Domain Service owns:

- Funding-readiness validation at Contract level.
- Funding-request creation.
- Funding Command creation and idempotency.
- Pending-state tracking.
- External Confirmation ingestion.
- Partial-funding and shortfall handling.
- Failure, reversal, and reconciliation workflows.
- Activation evaluation.

The configured Lender, bank, or funding authority owns the authoritative funding outcome.

A successful API request, transport acknowledgement, or Funding Command does not prove funding.

Funding Confirmation does not automatically prove Contract activation unless every configured activation condition is also satisfied.

### Contractual Payment Schedule and Payment Servicing Separation

The Financial Contract may own an immutable contractual payment-schedule snapshot containing:

- Number of installments.
- Payment frequency.
- Payment amounts.
- Due dates.
- Balloon payment.
- Residual value.
- Fees.
- Applicable grace terms.

Actual payment activity belongs to the configured Payment or servicing authority.

The Financial Contract must not independently claim:

- An installment was paid.
- A payment cleared.
- A Customer is in arrears.
- A balance is settled.
- A settlement was completed.

These facts require authoritative servicing or Payment evidence.

### Contract Versioning and Amendment Separation

A Financial Contract series represents the continuing legal agreement context.

A Financial Contract version represents one immutable contractual document version.

The model must distinguish:

```text
contract_series_id
  = stable identifier for the continuing contractual relationship

financial_contract_id
  = identifier for one specific contract version

contract_version
  = sequential version within the contract series
```

A Draft may be updated under concurrency controls.

After a signature request is issued, material contractual content must be frozen.

A material change requires:

- Cancellation or invalidation of the applicable signing workflow.
- A new Financial Contract version.
- New document generation.
- New disclosures where required.
- New signatures where required.
- Preserved supersession evidence.

An executed Financial Contract must never be overwritten.

A post-effectiveness amendment must be represented through:

- An immutable Contract Amendment record.
- A new consolidated contract version where required.
- The original executed contract.
- The amendment document.
- Amendment signatures.
- Effective date.
- Legal authority.
- Complete audit linkage.

### System Purpose

The Financial Contract Object provides canonical contractual context to:

- Customer workflows.
- Opportunity workflows.
- Quotation workflows.
- Finance Application workflows.
- Vehicle and Inventory workflows.
- Trade-In workflows.
- Deal workflows.
- Payment workflows.
- Funding workflows.
- Document-generation services.
- Electronic-signature providers.
- Lender integrations.
- Registration and lien integrations.
- Contract-servicing systems.
- Compliance and legal workflows.
- AI Agents.
- Analytics.
- Audit and regulatory reporting.

The Financial Contract may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Customer and Applicant identity | Customer Domain Service and approved identity authority |
| Selected finance offer | Finance Application and authoritative Lender Decision |
| Approved finance terms | Lender |
| Quotation commercial terms | Quotation Domain Service |
| Deal transaction context | Deal Domain Service |
| Vehicle identity | Vehicle Domain Service |
| Inventory eligibility | Inventory Domain Service or configured external authority |
| Trade-In equity and payoff | Trade-In Domain Service and approved external authorities |
| Legal template | Approved Legal Template Registry |
| Required disclosures | Approved legal, regulatory, Lender, and policy sources |
| Canonical Financial Contract and funding-request workflow | Financial Contract Domain Service |
| Funding Command creation, idempotency, and reconciliation | Financial Contract Domain Service |
| Generated contract document | Controlled Document Service |
| Signature completion | Approved Signature Provider or legally accepted physical-signature process |
| Contract effectiveness | Configured legal and contractual authority |
| Lien or security registration | Government, Lender, registration, or approved external authority |
| Funding outcome | Lender, bank, or configured funding authority |
| Deal funding projection and completion gate | Deal Domain Service |
| Payment and servicing outcomes | Payment or contract-servicing authority |
| Predictions and Recommendations | Derived Intelligence |
| Legal interpretation and high-impact Decisions | Authorized Human legal, compliance, finance, or management role |
| External operation completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `financial_contract_id` — UUIDv4, required and immutable.
- `contract_series_id` — UUIDv4, required and immutable within the series.
- `tenant_id` — UUIDv4, required and immutable.
- `contract_version` — Integer, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `finance_department_id`.
- `finance_team_id`.
- `assigned_finance_user_id`.
- `responsible_manager_user_id`.
- `responsible_legal_user_id`.
- `responsible_compliance_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `customer_id`.
- `opportunity_id`.
- `quotation_id`.
- `quotation_version`.
- `finance_application_id`.
- `finance_application_version`.
- `lender_submission_id`.
- `lender_decision_id`.
- `lender_decision_version`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `deal_id`.
- `appointment_id`.
- `primary_interaction_id`.
- `payment_schedule_reference`.
- `funding_reference`.
- `servicing_account_reference`.
- `compliance_case_id`.
- `dispute_case_id`.

### Contract Identity

- `contract_number`.
- `contract_series_reference`.
- `contract_type`.
- `contract_version`.
- `status`.
- `execution_method`.
- `workflow_authority_mode`.
- `is_current_version`.
- `supersedes_financial_contract_id`.
- `superseded_by_financial_contract_id`.
- `external_contract_reference`.
- `lender_contract_reference`.
- `jurisdiction_code`.
- `governing_law_code`.
- `contract_language_code`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Source Finance Decision

- `finance_application_id`.
- `finance_application_version`.
- `finance_application_version_hash`.
- `lender_submission_id`.
- `lender_decision_id`.
- `lender_decision_version`.
- `lender_id`.
- `finance_program_id`.
- `lender_decision_status`.
- `lender_decision_effective_at`.
- `lender_decision_valid_until`.
- `lender_decision_artifact_reference`.
- `lender_decision_artifact_hash`.
- `lender_decision_confirmation_status`.
- `selected_offer_snapshot`.
- `selected_offer_snapshot_hash`.
- `customer_selection_evidence_references`.
- `customer_selected_at`.

### Contract Parties

- `contract_party_ids`.
- `primary_customer_id`.
- `primary_applicant_id`.
- `co_applicant_ids`.
- `guarantor_ids`.
- `corporate_applicant_id`.
- `authorized_signatory_ids`.
- `beneficial_owner_ids`.
- `dealership_legal_entity_id`.
- `lender_legal_entity_id`.
- `additional_party_ids`.
- `party_structure_status`.
- `party_verification_status`.
- `party_snapshot`.
- `party_snapshot_hash`.

Each party must be represented through a governed Contract Party record.

### Contract Party Record

Each Contract Party may contain:

- `contract_party_id`.
- `customer_id`.
- `finance_applicant_id`.
- `party_role`.
- `party_type`.
- `legal_name_projection`.
- `legal_identifier_token`.
- `legal_identifier_type`.
- `registered_address_reference`.
- `contact_reference`.
- `signing_authority_type`.
- `signing_authority_status`.
- `identity_verification_status`.
- `authority_evidence_references`.
- `required_signature`.
- `required_acknowledgements`.
- `party_status`.

### Commercial Terms

- `quotation_id`.
- `quotation_version`.
- `quotation_document_hash`.
- `currency_code`.
- `vehicle_cash_price_amount`.
- `vehicle_selling_price_amount`.
- `optional_products_amount`.
- `service_products_amount`.
- `warranty_products_amount`.
- `insurance_products_amount`.
- `tax_amount`.
- `fee_amount`.
- `registration_fee_amount`.
- `documentation_fee_amount`.
- `delivery_fee_amount`.
- `total_transaction_amount`.
- `customer_down_payment_amount`.
- `deposit_amount`.
- `trade_in_allowance_amount`.
- `trade_in_payoff_amount`.
- `trade_in_positive_equity_amount`.
- `trade_in_negative_equity_amount`.
- `customer_cash_due_amount`.
- `amount_due_at_signing`.
- `commercial_terms_snapshot`.
- `commercial_terms_snapshot_hash`.

### Finance Terms

- `principal_amount`.
- `financed_amount`.
- `annual_interest_rate`.
- `annual_percentage_rate`.
- `effective_interest_rate`.
- `finance_charge_amount`.
- `total_repayment_amount`.
- `term_months`.
- `payment_frequency`.
- `installment_count`.
- `standard_installment_amount`.
- `first_payment_date`.
- `final_payment_date`.
- `balloon_payment_amount`.
- `residual_value_amount`.
- `late_payment_policy_reference`.
- `late_payment_fee_amount`.
- `grace_period_days`.
- `early_settlement_policy_reference`.
- `early_settlement_fee_amount`.
- `prepayment_policy_reference`.
- `default_policy_reference`.
- `finance_terms_snapshot`.
- `finance_terms_snapshot_hash`.

All finance terms must match the authoritative selected Lender Decision.

### Contractual Payment Schedule

- `payment_schedule_id`.
- `payment_schedule_version`.
- `payment_schedule_status`.
- `scheduled_payment_count`.
- `scheduled_payment_total_amount`.
- `payment_schedule_start_date`.
- `payment_schedule_end_date`.
- `payment_schedule_entries`.
- `payment_schedule_snapshot`.
- `payment_schedule_snapshot_hash`.
- `schedule_calculation_reference`.
- `schedule_calculation_rule_version`.
- `schedule_generated_at`.
- `schedule_validated_at`.

Each schedule entry may contain:

- `schedule_entry_id`.
- `sequence_number`.
- `due_date`.
- `principal_component_amount`.
- `interest_component_amount`.
- `fee_component_amount`.
- `tax_component_amount`.
- `installment_amount`.
- `balloon_component_amount`.
- `currency_code`.

Actual Payment status must not be stored as authoritative schedule state.

### Vehicle and Collateral Context

- `vehicle_id`.
- `inventory_record_id`.
- `vehicle_snapshot`.
- `vehicle_snapshot_hash`.
- `vin_token`.
- `vehicle_condition`.
- `vehicle_cash_value_amount`.
- `collateral_value_amount`.
- `loan_to_value_ratio`.
- `vehicle_eligibility_status`.
- `inventory_eligibility_status`.
- `vehicle_eligibility_confirmed_at`.
- `inventory_eligibility_confirmed_at`.
- `eligibility_expires_at`.

### Security Interest and Lien

- `security_interest_required`.
- `security_interest_type`.
- `security_interest_status`.
- `lien_registration_required`.
- `lien_registration_status`.
- `lien_registration_reference`.
- `lien_registration_requested_at`.
- `lien_registration_command_id`.
- `lien_registration_idempotency_key`.
- `lien_registered_at`.
- `lien_registration_confirmation_status`.
- `lien_registration_evidence_references`.
- `security_release_status`.
- `security_release_reference`.

A sent lien-registration Command does not prove that the lien was registered.

### Insurance and Protection Requirements

- `insurance_required`.
- `insurance_type_requirements`.
- `insurance_verification_status`.
- `insurance_policy_reference`.
- `insurance_provider_reference`.
- `insurance_effective_at`.
- `insurance_expires_at`.
- `insurance_evidence_references`.
- `insurance_confirmation_status`.
- `protection_product_requirements`.
- `protection_product_status`.

### Legal Template

- `contract_template_id`.
- `contract_template_version`.
- `template_jurisdiction_code`.
- `template_lender_id`.
- `template_finance_program_id`.
- `template_language_code`.
- `template_effective_from`.
- `template_effective_until`.
- `template_approval_reference`.
- `template_hash`.
- `template_selection_rule_id`.
- `template_selection_rule_version`.

### Contract Validation

- `contract_terms_validation_status`.
- `party_validation_status`.
- `commercial_validation_status`.
- `finance_terms_validation_status`.
- `vehicle_validation_status`.
- `deal_validation_status`.
- `lender_decision_validation_status`.
- `calculation_validation_status`.
- `document_validation_status`.
- `signature_requirement_validation_status`.
- `validation_failure_reasons`.
- `validation_snapshot`.
- `validation_snapshot_hash`.
- `validated_at`.
- `validated_by_actor_id`.

### Review and Approval

- `review_required`.
- `review_status`.
- `review_reason_codes`.
- `legal_review_status`.
- `compliance_review_status`.
- `finance_review_status`.
- `commercial_review_status`.
- `review_request_ids`.
- `review_decision_ids`.
- `review_requested_at`.
- `review_completed_at`.
- `approval_status`.
- `approval_policy_id`.
- `approval_policy_version`.
- `approval_decision_ids`.
- `approved_at`.
- `approved_by_actor_ids`.
- `approval_expires_at`.
- `approval_evidence_references`.

### Disclosure Package

- `disclosure_package_id`.
- `disclosure_package_version`.
- `required_disclosure_types`.
- `presented_disclosure_types`.
- `acknowledged_disclosure_types`.
- `missing_disclosure_types`.
- `rejected_disclosure_types`.
- `disclosure_status`.
- `disclosure_language_code`.
- `disclosure_presented_at`.
- `disclosure_acknowledged_at`.
- `disclosure_artifact_references`.
- `disclosure_artifact_hash`.
- `disclosure_evidence_references`.
- `disclosure_expiration_at`.
- `disclosure_revalidation_required`.

Disclosure acknowledgement must remain distinct from contract signature.

### Cooling-Off and Rescission

- `cooling_off_period_applies`.
- `cooling_off_period_basis`.
- `cooling_off_start_at`.
- `cooling_off_end_at`.
- `cooling_off_status`.
- `rescission_status`.
- `rescission_requested_at`.
- `rescission_effective_at`.
- `rescission_reason`.
- `rescission_evidence_references`.
- `rescission_confirmation_status`.

The applicable legal and contractual policy determines whether effectiveness or activation must wait for the cooling-off period.

### Document Generation

- `document_generation_status`.
- `document_generation_request_id`.
- `document_generation_idempotency_key`.
- `document_template_id`.
- `document_template_version`.
- `unsigned_document_reference`.
- `unsigned_document_hash`.
- `unsigned_document_page_count`.
- `document_generated_at`.
- `document_generation_error_code`.
- `document_snapshot`.
- `document_snapshot_hash`.

### Contract Presentation and Delivery

- `presentation_status`.
- `delivery_status`.
- `delivery_channel`.
- `delivery_requested_at`.
- `delivery_command_id`.
- `delivery_idempotency_key`.
- `delivered_at`.
- `delivery_provider_reference`.
- `delivery_interaction_id`.
- `delivery_confirmation_reference`.
- `delivery_failure_reason`.
- `customer_copy_delivery_status`.
- `customer_copy_delivered_at`.

Document generation does not prove Customer delivery.

### Signature Envelope

- `signature_provider`.
- `signature_envelope_id`.
- `signature_envelope_version`.
- `signature_status`.
- `signature_method`.
- `signature_request_status`.
- `signature_request_command_id`.
- `signature_request_idempotency_key`.
- `signature_request_sent_at`.
- `signature_request_confirmation_status`.
- `signature_expires_at`.
- `required_signer_count`.
- `completed_signer_count`.
- `signature_completion_percentage`.
- `fully_signed_at`.
- `signature_certificate_reference`.
- `signature_certificate_hash`.
- `signed_document_reference`.
- `signed_document_hash`.
- `signature_evidence_snapshot`.
- `signature_evidence_snapshot_hash`.
- `signature_reconciliation_status`.

### Signer Records

Each signer record may contain:

- `contract_signer_id`.
- `contract_party_id`.
- `signer_role`.
- `signing_order`.
- `signature_required`.
- `acknowledgement_required`.
- `identity_verification_required`.
- `identity_verification_status`.
- `signing_authority_status`.
- `signer_status`.
- `signature_requested_at`.
- `document_viewed_at`.
- `signed_at`.
- `declined_at`.
- `decline_reason`.
- `signature_evidence_reference`.
- `signature_provider_reference`.
- `signature_confirmation_status`.

### Physical Signature Context

- `physical_signature_required`.
- `physical_signature_status`.
- `original_document_count`.
- `original_document_location_reference`.
- `witness_required`.
- `witness_status`.
- `notarization_required`.
- `notarization_status`.
- `notary_reference`.
- `physical_document_verification_status`.
- `physical_document_received_at`.
- `physical_document_evidence_references`.

### Countersignature

- `countersignature_required`.
- `countersignature_status`.
- `required_countersignatory_ids`.
- `completed_countersignatory_ids`.
- `countersignature_completed_at`.
- `countersignature_evidence_references`.

### Contract Effectiveness

- `effectiveness_status`.
- `effectiveness_rule_id`.
- `effectiveness_rule_version`.
- `effectiveness_condition_ids`.
- `outstanding_effectiveness_conditions`.
- `effectiveness_evaluated_at`.
- `effective_at`.
- `effectiveness_decision_id`.
- `effectiveness_confirmation_status`.
- `effectiveness_evidence_references`.

### Activation

- `activation_status`.
- `activation_rule_id`.
- `activation_rule_version`.
- `activation_condition_ids`.
- `outstanding_activation_conditions`.
- `activation_requested_at`.
- `activation_command_id`.
- `activation_idempotency_key`.
- `activation_confirmation_status`.
- `activated_at`.
- `activation_evidence_references`.

### Funding Workflow and Outcome Projection

- `funding_required`.
- `funding_readiness_status`.
- `funding_status`.
- `funding_request_reference`.
- `funding_requested_at`.
- `funding_command_id`.
- `funding_idempotency_key`.
- `funding_amount_requested`.
- `funding_currency_code`.
- `funding_authority_id`.
- `funding_authority_reference`.
- `funded_amount`.
- `funding_received_at`.
- `funding_confirmation_status`.
- `funding_confirmation_reference`.
- `funding_confirmation_source_record_version`.
- `funding_shortfall_amount`.
- `funding_failure_reason`.
- `funding_reconciliation_status`.
- `funding_reversal_status`.
- `funding_reversal_request_reference`.
- `funding_reversal_command_id`.
- `funding_reversal_idempotency_key`.
- `funding_reversal_reference`.
- `funding_projection_observed_at`.
- `funding_projection_last_synced_at`.
- `funding_projection_freshness_status`.

The Financial Contract Domain Service owns the funding-request workflow, Funding Commands, idempotency, pending state, shortfall handling, reversal workflow, and reconciliation.

The configured Lender, bank, or funding authority owns the authoritative funding outcome.

Outcome fields must preserve source authority, source record version, observation time, Confirmation evidence, and reconciliation state.

Finance Application and Deal may consume read-only projections from this workflow but must not recreate it.

### Servicing Projection

- `servicing_provider_id`.
- `servicing_account_reference`.
- `servicing_status`.
- `servicing_activation_status`.
- `current_balance_projection`.
- `next_payment_date_projection`.
- `next_payment_amount_projection`.
- `arrears_status_projection`.
- `settlement_status_projection`.
- `servicing_last_confirmed_at`.
- `servicing_data_freshness_status`.
- `servicing_reconciliation_status`.

Servicing projections must not replace the authoritative servicing system.

### Amendment

- `amendment_ids`.
- `amendment_count`.
- `current_amendment_status`.
- `last_amended_at`.
- `current_consolidated_version`.
- `amendment_reconciliation_status`.

Each Contract Amendment must preserve:

- `contract_amendment_id`.
- `amendment_version`.
- `amendment_type`.
- `amendment_reason`.
- `affected_terms`.
- `previous_terms_hash`.
- `revised_terms_snapshot`.
- `revised_terms_hash`.
- `lender_authorization_reference`.
- `customer_acknowledgement_status`.
- `signature_status`.
- `effective_at`.
- `amendment_document_reference`.
- `amendment_document_hash`.
- `supersedes_amendment_id`.
- `status`.

### Cancellation

- `cancellation_status`.
- `cancellation_requested_at`.
- `cancelled_at`.
- `cancellation_reason`.
- `cancellation_details`.
- `cancelled_by_actor_type`.
- `cancelled_by_actor_id`.
- `cancellation_decision_id`.
- `cancellation_evidence_references`.
- `external_cancellation_confirmation_status`.

Cancellation applies before contractual effectiveness unless an applicable legal process defines otherwise.

### Voiding

- `void_status`.
- `void_requested_at`.
- `voided_at`.
- `void_reason`.
- `void_details`.
- `void_decision_id`.
- `void_authority_reference`.
- `void_evidence_references`.
- `void_external_confirmation_status`.

Voiding must not erase the original executed document.

### Termination

- `termination_status`.
- `termination_requested_at`.
- `termination_effective_at`.
- `termination_reason`.
- `termination_details`.
- `termination_decision_id`.
- `termination_authority_reference`.
- `termination_settlement_reference`.
- `termination_evidence_references`.
- `termination_confirmation_status`.

### Completion and Settlement

- `completion_status`.
- `settlement_status`.
- `settlement_amount`.
- `settlement_currency_code`.
- `settlement_reference`.
- `settlement_confirmed_at`.
- `settlement_confirmation_status`.
- `completion_reason`.
- `completed_at`.
- `completion_confirmation_status`.
- `completion_evidence_references`.

Contract completion requires authoritative servicing or settlement evidence.

### Dispute

- `dispute_status`.
- `dispute_case_id`.
- `dispute_type`.
- `dispute_opened_at`.
- `dispute_reason`.
- `disputed_fields`.
- `dispute_resolution_status`.
- `dispute_resolved_at`.
- `dispute_resolution_decision_id`.
- `dispute_evidence_references`.

### Derived Intelligence

- `contract_completeness_score`.
- `terms_mismatch_risk_score`.
- `signature_completion_risk_score`.
- `funding_delay_risk_score`.
- `document_anomaly_score`.
- `recommended_missing_action`.
- `recommended_signing_sequence`.
- `recommended_follow_up_action`.
- `recommended_human_review`.
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

- `total_upfront_amount`.
- `total_finance_cost_amount`.
- `signature_completion_percentage`.
- `disclosure_completion_percentage`.
- `effectiveness_condition_completion_percentage`.
- `activation_condition_completion_percentage`.
- `days_until_signature_expiry`.
- `days_until_cooling_off_end`.
- `days_until_first_payment`.
- `days_until_final_payment`.
- `contract_age_days`.
- `funding_shortfall_amount`.
- `is_fully_signed`.
- `is_effective`.
- `is_active`.
- `has_active_amendment`.
- `is_under_dispute`.

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
- `generated_at`.
- `approved_at`.
- `fully_signed_at`.
- `effective_at`.
- `activated_at`.
- `completed_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `financial_contract_id` | UUID | Yes | ASOS | Immutable identifier for one Financial Contract version. |
| `contract_series_id` | UUID | Yes | ASOS | Stable identifier shared by versions of the continuing agreement. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `contract_number` | String | Yes | ASOS, Lender, or configured authority | Human-readable contractual reference. |
| `contract_type` | Enum | Yes | Selected Lender Decision and legal policy | Contractual product type. |
| `contract_version` | Integer | Yes | ASOS | Sequential version in the contract series. |
| `status` | Enum | Yes | Financial Contract workflow | Current lifecycle state. |
| `execution_method` | Enum | Yes | Legal and workflow policy | Electronic, physical, hybrid, or another approved method. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Defines the system controlling the applicable contract lifecycle fields. |
| `is_current_version` | Boolean | Yes | ASOS | Identifies the current contract version in the series. |
| `jurisdiction_code` | String | Yes | Legal policy | Jurisdiction governing required contractual controls. |
| `governing_law_code` | String | Yes | Legal policy | Applicable governing-law reference. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Source Decision Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_application_id` | UUID | Yes | Canonical relationship | Finance Application supporting the contract. |
| `finance_application_version` | Integer | Yes | Finance Application | Exact immutable application version. |
| `lender_submission_id` | UUID | Yes | Canonical relationship | Lender submission that produced the selected Decision. |
| `lender_decision_id` | UUID | Yes | Canonical relationship | Selected authoritative Lender Decision. |
| `lender_decision_version` | String | Yes | Lender | Exact version of the Decision. |
| `lender_id` | UUID | Yes | Approved Lender registry | Lender party to or supporting the contract. |
| `finance_program_id` | UUID | Yes | Lender | Finance program governing the approved terms. |
| `lender_decision_valid_until` | Timestamp | Conditional | Lender | Expiration of the selected approval. |
| `lender_decision_artifact_hash` | String | Yes | ASOS | Integrity hash of the Decision artifact. |
| `selected_offer_snapshot_hash` | String | Yes | ASOS | Integrity hash of the Customer-selected finance offer. |

### Commercial Relationship Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | UUID | Yes | Canonical relationship | Primary Customer associated with the contract. |
| `opportunity_id` | UUID | Yes | Canonical relationship | Opportunity that produced the commercial journey. |
| `quotation_id` | UUID | Yes | Canonical relationship | Exact Quotation supplying commercial terms. |
| `quotation_version` | Integer | Yes | Quotation | Exact accepted Quotation version. |
| `quotation_document_hash` | String | Yes | Quotation | Hash of the accepted issued Quotation document. |
| `deal_id` | UUID | Yes | Canonical relationship | Deal governed by the contract. |
| `vehicle_id` | UUID | Yes | Canonical relationship | Vehicle financed, leased, or secured. |
| `inventory_record_id` | UUID | Conditional | Inventory relationship | Physical stock record where applicable. |
| `trade_in_id` | UUID | No | Canonical relationship | Trade-In contributing allowance, payoff, or equity. |

### Party Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contract_party_id` | UUID | Yes | ASOS | Identifier for one contractual party. |
| `party_role` | Enum | Yes | Contract structure | Role of the party. |
| `party_type` | Enum | Yes | Contract structure | Individual or organization. |
| `customer_id` | UUID | Conditional | Customer relationship | Canonical Customer or organization reference. |
| `finance_applicant_id` | UUID | Conditional | Finance Application relationship | Applicant record represented by the party. |
| `legal_name_projection` | String | Yes | Verified identity or legal registry | Legal name used in the contract. |
| `legal_identifier_token` | String | Conditional | Secure identity authority | Tokenized legal identifier. |
| `signing_authority_status` | Enum | Yes | Verification workflow | Whether the party is authorized to sign. |
| `identity_verification_status` | Enum | Yes | Identity authority | Identity-verification state. |
| `required_signature` | Boolean | Yes | Template and legal policy | Whether a signature is mandatory. |
| `party_status` | Enum | Yes | Contract workflow | Current participation state. |

### Commercial Term Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `currency_code` | String | Yes | Quotation and Lender Decision | ISO 4217 contract currency. |
| `vehicle_cash_price_amount` | Decimal | Yes | Quotation | Cash price disclosed in the contract. |
| `vehicle_selling_price_amount` | Decimal | Yes | Quotation | Final Vehicle selling price. |
| `optional_products_amount` | Decimal | Yes | Quotation | Included optional products total. |
| `tax_amount` | Decimal | Yes | Quotation and deterministic tax calculation | Applicable taxes. |
| `fee_amount` | Decimal | Yes | Quotation | Applicable disclosed fees. |
| `total_transaction_amount` | Decimal | Yes | Deterministic calculation | Total commercial transaction amount. |
| `customer_down_payment_amount` | Decimal | Yes | Lender Decision, Quotation, and Deal | Required Customer contribution. |
| `trade_in_allowance_amount` | Decimal | Yes | Quotation and Trade-In projection | Customer-facing Trade-In allowance. |
| `trade_in_payoff_amount` | Decimal | Yes | Trade-In and authoritative payoff evidence | Payoff included in the transaction. |
| `trade_in_positive_equity_amount` | Decimal | Yes | Deterministic calculation | Positive Trade-In equity. |
| `trade_in_negative_equity_amount` | Decimal | Yes | Deterministic calculation | Negative Trade-In equity. |
| `amount_due_at_signing` | Decimal | Yes | Deterministic calculation | Amount contractually due before or at signing. |
| `commercial_terms_snapshot_hash` | String | Yes | ASOS | Integrity hash of the commercial snapshot. |

### Finance Term Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `principal_amount` | Decimal | Yes | Lender Decision | Contractual principal. |
| `financed_amount` | Decimal | Yes | Lender Decision | Total amount financed. |
| `annual_interest_rate` | Decimal | Conditional | Lender Decision | Contractual nominal rate. |
| `annual_percentage_rate` | Decimal | Conditional | Lender Decision or approved legal calculation | APR or equivalent required measure. |
| `finance_charge_amount` | Decimal | Yes | Approved deterministic calculation | Total contractual finance charge. |
| `total_repayment_amount` | Decimal | Yes | Approved deterministic calculation | Total contractual repayment. |
| `term_months` | Integer | Yes | Lender Decision | Contractual finance term. |
| `payment_frequency` | Enum | Yes | Lender Decision | Contractual payment frequency. |
| `installment_count` | Integer | Yes | Approved schedule calculation | Number of scheduled payments. |
| `standard_installment_amount` | Decimal | Conditional | Lender Decision and schedule | Standard recurring payment. |
| `first_payment_date` | Date | Yes | Lender Decision or contract terms | First contractual due date. |
| `final_payment_date` | Date | Yes | Approved schedule | Final contractual due date. |
| `balloon_payment_amount` | Decimal | Yes | Lender Decision | Final balloon payment where applicable. |
| `residual_value_amount` | Decimal | Yes | Lender Decision | Contractual residual where applicable. |
| `finance_terms_snapshot_hash` | String | Yes | ASOS | Integrity hash of authoritative finance terms. |

### Document and Template Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contract_template_id` | UUID | Yes | Legal Template Registry | Approved template identifier. |
| `contract_template_version` | String | Yes | Legal Template Registry | Exact approved template version. |
| `template_hash` | String | Yes | Template authority | Integrity hash of the template. |
| `document_generation_status` | Enum | Yes | Document workflow | Current generation state. |
| `unsigned_document_reference` | String | Conditional | Controlled document storage | Generated unsigned contract. |
| `unsigned_document_hash` | String | Conditional | ASOS | Hash of the generated unsigned contract. |
| `signed_document_reference` | String | Conditional | Controlled document storage | Fully executed document. |
| `signed_document_hash` | String | Conditional | ASOS or signature authority | Hash of the executed document. |
| `document_snapshot_hash` | String | Conditional | ASOS | Integrity hash of the generation snapshot. |

### Disclosure Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `disclosure_package_id` | UUID | Yes | Disclosure workflow | Applicable disclosure package. |
| `disclosure_package_version` | String | Yes | Legal or regulatory authority | Exact disclosure version. |
| `required_disclosure_types` | Array | Yes | Legal, Lender, and policy rules | Mandatory disclosures. |
| `disclosure_status` | Enum | Yes | Disclosure workflow | Current presentation and acknowledgement state. |
| `disclosure_presented_at` | Timestamp | No | Presentation evidence | Time disclosures were presented. |
| `disclosure_acknowledged_at` | Timestamp | No | Party evidence | Time all required disclosures were acknowledged. |
| `disclosure_artifact_hash` | String | Conditional | ASOS | Integrity hash of the disclosure package. |
| `disclosure_evidence_references` | Array | No | Evidence repository | Supporting presentation and acknowledgement evidence. |

### Signature Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `signature_provider` | Enum | No | Configuration | Approved signature authority. |
| `signature_envelope_id` | String | Conditional | Signature provider | External envelope identifier. |
| `signature_status` | Enum | Yes | Signature workflow | Aggregate signature state. |
| `required_signer_count` | Integer | Yes | Template and legal policy | Number of required signers. |
| `completed_signer_count` | Integer | Yes | Signature authority | Number of accepted completed signatures. |
| `signature_request_sent_at` | Timestamp | No | Signature workflow | Time the signing request was transmitted. |
| `signature_expires_at` | Timestamp | No | Signature authority or policy | Expiration of the signing request. |
| `fully_signed_at` | Timestamp | No | Signature authority | Time all required signatures completed. |
| `signature_certificate_reference` | String | No | Signature authority | Certificate or completion evidence. |
| `signature_certificate_hash` | String | No | ASOS | Integrity hash of the certificate. |
| `signature_reconciliation_status` | Enum | Yes | Reconciliation workflow | Current signature-provider reconciliation state. |

### Effectiveness and Activation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `effectiveness_status` | Enum | Yes | Contract workflow | Current contractual effectiveness state. |
| `outstanding_effectiveness_conditions` | Array | Yes | Deterministic policy | Conditions still required for effectiveness. |
| `effective_at` | Timestamp | No | Legal or contractual authority | Time the contract became effective. |
| `effectiveness_decision_id` | UUID | No | Human Decision or governed workflow | Decision supporting effectiveness where required. |
| `activation_status` | Enum | Yes | Activation workflow | Current operational activation state. |
| `outstanding_activation_conditions` | Array | Yes | Deterministic policy | Conditions still required for activation. |
| `activated_at` | Timestamp | No | Configured activation authority | Time activation became authoritative. |

### Funding Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `funding_required` | Boolean | Yes | Finance product and Lender policy | Whether external funding is required. |
| `funding_readiness_status` | Enum | Yes | Financial Contract workflow | Contract-level readiness to create a Funding Command. |
| `funding_status` | Enum | Yes | Financial Contract workflow plus external outcome projection | Current governed request and observed outcome state. |
| `funding_request_reference` | String | No | Financial Contract workflow | Stable canonical reference for the funding request. |
| `funding_command_id` | UUID | No | Financial Contract workflow | Command identifier created by the owning Domain Service. |
| `funding_idempotency_key` | String | No | Financial Contract workflow | Key preventing duplicate funding requests. |
| `funding_amount_requested` | Decimal | No | Financial Contract workflow | Amount requested using the accepted Contract and Lender terms. |
| `funding_currency_code` | String | No | Financial Contract and Lender terms | Currency of the request and Confirmation. |
| `funding_authority_reference` | String | No | Funding authority | External funding reference. |
| `funded_amount` | Decimal | No | Funding authority | Authoritatively confirmed funded amount. |
| `funding_received_at` | Timestamp | No | Funding authority | Time funds were authoritatively confirmed. |
| `funding_confirmation_status` | Enum | Yes | Financial Contract Confirmation workflow | Current External Confirmation state. |
| `funding_confirmation_reference` | String | No | Funding authority or Confirmation adapter | Evidence reference supporting the observed outcome. |
| `funding_confirmation_source_record_version` | String | No | Funding authority projection | Source version used for freshness and reconciliation. |
| `funding_reconciliation_status` | Enum | Yes | Financial Contract workflow | Funding reconciliation state. |
| `funding_shortfall_amount` | Decimal | Yes | Deterministic calculation | Difference between required and confirmed funding. |
| `funding_failure_reason` | String | No | Funding authority projection | Normalized externally supported failure reason. |
| `funding_reversal_status` | Enum | Yes | Financial Contract workflow plus external projection | Current reversal workflow and observed result. |
| `funding_reversal_reference` | String | No | Funding authority | External reversal evidence. |
| `funding_projection_freshness_status` | Enum | Yes | Projection metadata | Whether the external outcome projection is current enough for dependent decisions. |

The Finance Application and Deal Domain Services must not own the Funding Command, funding idempotency key, or authoritative funding transaction state.

---

## 4. Enumerations

### FinancialContractStatus

- `DRAFT`
- `VALIDATION_PENDING`
- `REVIEW_PENDING`
- `APPROVED`
- `GENERATED`
- `DISCLOSURE_PENDING`
- `READY_FOR_SIGNATURE`
- `SIGNATURE_PENDING`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `EFFECTIVENESS_PENDING`
- `EFFECTIVE`
- `FUNDING_PENDING`
- `ACTIVE`
- `COMPLETED`
- `CANCELLED`
- `VOIDED`
- `EXPIRED`
- `TERMINATED`
- `SUPERSEDED`
- `DISPUTED`
- `ARCHIVED`

### FinancialContractType

- `VEHICLE_FINANCE`
- `VEHICLE_LOAN`
- `HIRE_PURCHASE`
- `FINANCE_LEASE`
- `OPERATING_LEASE`
- `BALLOON_FINANCE`
- `ISLAMIC_FINANCE`
- `CORPORATE_FINANCE`
- `FLEET_FINANCE`
- `REFINANCE`
- `GUARANTEE_AGREEMENT`
- `SECURITY_AGREEMENT`
- `OTHER`

### ContractExecutionMethod

- `ELECTRONIC`
- `PHYSICAL`
- `HYBRID`
- `REMOTE_NOTARIZED`
- `IN_PERSON_NOTARIZED`
- `OTHER`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `LENDER_PLATFORM_AUTHORITATIVE`
- `DOCUMENT_PLATFORM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### ContractPartyRole

- `PRIMARY_CUSTOMER`
- `PRIMARY_APPLICANT`
- `CO_APPLICANT`
- `GUARANTOR`
- `CORPORATE_APPLICANT`
- `AUTHORIZED_SIGNATORY`
- `BENEFICIAL_OWNER`
- `DEALERSHIP`
- `LENDER`
- `WITNESS`
- `NOTARY`
- `SECURITY_PROVIDER`
- `OTHER`

### ContractPartyType

- `INDIVIDUAL`
- `ORGANIZATION`
- `GOVERNMENT_ENTITY`
- `OTHER`

### PartyStatus

- `PENDING`
- `ACTIVE`
- `VERIFIED`
- `WITHDRAWN`
- `REJECTED`
- `REPLACED`
- `EXPIRED`
- `DISPUTED`

### SigningAuthorityStatus

- `NOT_REQUIRED`
- `NOT_VERIFIED`
- `PENDING`
- `VERIFIED`
- `REJECTED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### ContractValidationStatus

- `NOT_STARTED`
- `PENDING`
- `VALIDATED`
- `MISMATCH_DETECTED`
- `FAILED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### ContractReviewStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `APPROVED_WITH_CONDITIONS`
- `REJECTED`
- `EXPIRED`
- `REVALIDATION_REQUIRED`

### ContractApprovalStatus

- `NOT_EVALUATED`
- `NOT_REQUIRED`
- `REQUIRED`
- `PENDING`
- `PARTIALLY_APPROVED`
- `APPROVED`
- `REJECTED`
- `EXPIRED`
- `REVOKED`
- `REVALIDATION_REQUIRED`

### ContractDocumentGenerationStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `QUEUED`
- `GENERATING`
- `GENERATED`
- `FAILED`
- `INVALIDATED`
- `SUPERSEDED`
- `RECONCILIATION_REQUIRED`

### ContractPresentationStatus

- `NOT_STARTED`
- `READY`
- `DELIVERY_PENDING`
- `PRESENTED`
- `FAILED`
- `EXPIRED`
- `WITHDRAWN`
- `RECONCILIATION_REQUIRED`

### ContractDeliveryStatus

- `NOT_REQUESTED`
- `PENDING_APPROVAL`
- `COMMAND_PENDING`
- `SENT_TO_PROVIDER`
- `PENDING_CONFIRMATION`
- `DELIVERED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ContractDisclosureStatus

- `NOT_STARTED`
- `PREPARING`
- `READY`
- `PRESENTED`
- `PARTIALLY_ACKNOWLEDGED`
- `ACKNOWLEDGED`
- `REJECTED`
- `EXPIRED`
- `NOT_REQUIRED`
- `REVALIDATION_REQUIRED`
- `DISPUTED`

### ContractSignatureStatus

- `NOT_STARTED`
- `PREPARING`
- `REQUEST_PENDING`
- `REQUESTED`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `DECLINED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `INVALIDATED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### SignerStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUESTED`
- `VIEWED`
- `ACKNOWLEDGED`
- `SIGNED`
- `DECLINED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `INVALIDATED`
- `DISPUTED`

### PhysicalSignatureStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `DOCUMENTS_PREPARED`
- `SIGNING_IN_PROGRESS`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `ORIGINALS_PENDING`
- `ORIGINALS_RECEIVED`
- `VERIFIED`
- `REJECTED`
- `LOST`
- `DISPUTED`

### CountersignatureStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `PARTIALLY_COMPLETED`
- `COMPLETED`
- `DECLINED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`

### ContractEffectivenessStatus

- `NOT_ASSESSED`
- `NOT_READY`
- `CONDITIONS_PENDING`
- `READY`
- `PENDING_CONFIRMATION`
- `EFFECTIVE`
- `BLOCKED`
- `EXPIRED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### ContractActivationStatus

- `NOT_ASSESSED`
- `NOT_READY`
- `CONDITIONS_PENDING`
- `READY`
- `ACTIVATION_PENDING`
- `PENDING_EXTERNAL_CONFIRMATION`
- `ACTIVE`
- `BLOCKED`
- `FAILED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### ContractFundingReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### ContractFundingStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `REQUIREMENTS_PENDING`
- `REQUEST_READY`
- `COMMAND_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `PARTIALLY_FUNDED`
- `FUNDED`
- `FAILED`
- `REVERSED`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### PaymentScheduleStatus

- `NOT_STARTED`
- `GENERATION_PENDING`
- `GENERATED`
- `VALIDATION_PENDING`
- `VALIDATED`
- `FAILED`
- `INVALIDATED`
- `SUPERSEDED`
- `RECONCILIATION_REQUIRED`

### PaymentFrequency

- `WEEKLY`
- `BIWEEKLY`
- `MONTHLY`
- `QUARTERLY`
- `SEMIANNUAL`
- `ANNUAL`
- `CUSTOM`

### SecurityInterestType

- `NONE`
- `VEHICLE_LIEN`
- `TITLE_RETENTION`
- `CHATTEL_MORTGAGE`
- `PERSONAL_GUARANTEE`
- `CORPORATE_GUARANTEE`
- `SECURITY_DEPOSIT`
- `OTHER`

### SecurityInterestStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `REGISTERED`
- `PARTIALLY_REGISTERED`
- `FAILED`
- `EXPIRED`
- `RELEASE_PENDING`
- `RELEASED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### LienRegistrationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `REGISTERED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RELEASE_PENDING`
- `RELEASED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### InsuranceVerificationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `VERIFIED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### CoolingOffStatus

- `NOT_APPLICABLE`
- `NOT_STARTED`
- `ACTIVE`
- `COMPLETED`
- `WAIVED_WHERE_LAWFULLY_PERMITTED`
- `RESCISSION_REQUESTED`
- `RESCINDED`
- `DISPUTED`

### RescissionStatus

- `NOT_APPLICABLE`
- `NOT_REQUESTED`
- `REQUESTED`
- `VALIDATION_PENDING`
- `ACCEPTED`
- `REJECTED`
- `EFFECTIVE`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### AmendmentStatus

- `DRAFT`
- `VALIDATION_PENDING`
- `REVIEW_PENDING`
- `APPROVED`
- `GENERATED`
- `SIGNATURE_PENDING`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `EFFECTIVENESS_PENDING`
- `EFFECTIVE`
- `REJECTED`
- `CANCELLED`
- `VOIDED`
- `SUPERSEDED`
- `EXPIRED`
- `DISPUTED`

### AmendmentType

- `PAYMENT_SCHEDULE_CHANGE`
- `TERM_EXTENSION`
- `RATE_CHANGE`
- `PAYMENT_AMOUNT_CHANGE`
- `BALLOON_CHANGE`
- `PARTY_CHANGE`
- `GUARANTOR_CHANGE`
- `COLLATERAL_CHANGE`
- `ADDRESS_OR_CONTACT_CORRECTION`
- `LEGAL_CORRECTION`
- `LENDER_CORRECTION`
- `RESTRUCTURING`
- `HARDSHIP_MODIFICATION`
- `OTHER`

### ContractCancellationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `VALIDATION_PENDING`
- `APPROVED`
- `PENDING_EXTERNAL_CONFIRMATION`
- `CANCELLED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### FinancialContractCancellationReason

- `CUSTOMER_REQUEST`
- `CUSTOMER_DECLINED_TO_SIGN`
- `LENDER_APPROVAL_WITHDRAWN`
- `LENDER_APPROVAL_EXPIRED`
- `FINANCE_APPLICATION_CANCELLED`
- `DEAL_CANCELLED`
- `VEHICLE_UNAVAILABLE`
- `COMMERCIAL_TERMS_CHANGED`
- `COMPLIANCE_BLOCK`
- `IDENTITY_VERIFICATION_FAILED`
- `SIGNATURE_EXPIRED`
- `DUPLICATE_CONTRACT`
- `DOCUMENT_ERROR`
- `OTHER`

### ContractVoidStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `LEGAL_REVIEW_PENDING`
- `APPROVED`
- `PENDING_EXTERNAL_CONFIRMATION`
- `VOIDED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### FinancialContractVoidReason

- `INVALID_SIGNATURE`
- `MATERIAL_TERM_ERROR`
- `INCORRECT_PARTY`
- `INCORRECT_VEHICLE`
- `INCORRECT_LENDER`
- `FRAUD_CONFIRMED`
- `LEGAL_INVALIDITY`
- `REGULATORY_REQUIREMENT`
- `DUPLICATE_EXECUTION`
- `DOCUMENT_INTEGRITY_FAILURE`
- `OTHER`

### ContractTerminationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `REVIEW_PENDING`
- `APPROVED`
- `SETTLEMENT_PENDING`
- `PENDING_EXTERNAL_CONFIRMATION`
- `TERMINATED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### FinancialContractTerminationReason

- `EARLY_SETTLEMENT`
- `CUSTOMER_DEFAULT`
- `VOLUNTARY_TERMINATION`
- `VEHICLE_TOTAL_LOSS`
- `VEHICLE_REPOSSESSION`
- `LENDER_TERMINATION`
- `LEGAL_ORDER`
- `CONTRACT_RESTRUCTURING`
- `REFINANCE`
- `FRAUD_CONFIRMED`
- `MUTUAL_AGREEMENT`
- `OTHER`

### ContractSettlementStatus

- `NOT_STARTED`
- `CALCULATION_PENDING`
- `PENDING`
- `PARTIALLY_SETTLED`
- `SETTLED`
- `FAILED`
- `WAIVED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### ContractCompletionStatus

- `NOT_ELIGIBLE`
- `ELIGIBILITY_PENDING`
- `READY`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### ContractDisputeStatus

- `NONE`
- `OPEN`
- `UNDER_REVIEW`
- `EVIDENCE_PENDING`
- `RESOLUTION_PENDING`
- `RESOLVED`
- `REJECTED`
- `ESCALATED`
- `LEGAL_HOLD`
- `CLOSED`

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
- All related Domain Objects must belong to the authorized Tenant.
- Dealership, branch, legal entity, finance team, User, Lender, template, and approval authority must belong to the permitted scope.
- Cross-Tenant contract access, generation, signing, AI retrieval, export, amendment, activation, and termination are prohibited unless governed by an approved legal and technical mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Contract Creation Rules

A Financial Contract may be created only when:

- The Finance Application is eligible for contracting.
- A valid Customer-selected Lender offer exists.
- The selected Lender Decision is current.
- The Finance Application version is immutable and valid.
- The associated Deal exists and is eligible.
- The exact Quotation version is identified.
- Customer and Applicant identities are valid.
- Required parties are known.
- Vehicle and Inventory relationships are valid.
- Required Trade-In projections are current.
- Required compliance conditions are satisfied.
- Contract creation is idempotent.
- No blocking conflict exists.

Contract creation must preserve:

- Finance Application version.
- Lender Decision version.
- Quotation version.
- Commercial snapshot.
- Finance terms snapshot.
- Party snapshot.
- Source-record versions.
- Creation authority.
- Integrity hashes.
- Audit evidence.

### Source-Consistency Rules

The Financial Contract must match the authoritative sources for:

- Customer.
- Applicants.
- Guarantors.
- Deal.
- Quotation.
- Vehicle.
- Inventory Record.
- Trade-In.
- Lender.
- Finance program.
- Principal.
- Rate.
- APR or equivalent measure.
- Term.
- Payment frequency.
- Installment amount.
- Balloon.
- Residual.
- Fees.
- Conditions.
- Decision expiration.

A material mismatch must block:

- Approval.
- Document generation.
- Signature request.
- Effectiveness.
- Funding.
- Activation.

AI must not resolve a contractual mismatch independently.

### Party Rules

- Every Financial Contract must contain all required parties.
- Every required party must have an authorized Contract Party record.
- Every individual signer must be linked to a verified identity.
- Every organization signer must have verified signing authority.
- Co-Applicants and Guarantors required by the Lender Decision must be included.
- Party names must match accepted legal identity evidence.
- A material party change requires a new contract version.
- A signed party must not be removed through an ordinary update.
- A signer’s role and authority must be preserved historically.
- AI must not establish signing authority.

### Commercial-Term Rules

- Commercial terms must match the exact Quotation version and Deal.
- Monetary values must use fixed decimal precision.
- Currency must use ISO 4217.
- Customer-visible and contractual fees must be disclosed.
- Hidden charges are prohibited.
- Trade-In values must match the accepted Trade-In and Quotation projections.
- Customer cash due and amount due at signing must be calculated deterministically.
- A material commercial change requires revalidation and normally a new contract version.
- Internal-only price floors or margins must not appear in the Customer contract.

### Finance-Term Rules

- Finance terms must match the authoritative Lender Decision.
- Dealership Users and AI Agents must not alter Lender terms.
- Interest rate, APR, term, payment, balloon, residual, fees, and conditions must remain source-traceable.
- The Lender Decision must remain valid at contract approval and applicable contracting stages.
- Expired terms require Lender revalidation or a new Decision.
- A conditional approval must preserve all applicable conditions.
- A contract must not remove a condition merely because dealership evidence was submitted.
- Lender Confirmation is required where the Lender controls condition satisfaction.

### Calculation Rules

All authoritative contract calculations must be deterministic.

AI must not calculate authoritative contractual totals.

Calculations must preserve:

- Formula.
- Rule version.
- Inputs.
- Source-record versions.
- Currency.
- Rounding.
- Frequency normalization.
- Tax treatment.
- Output.
- Timestamp.
- Integrity hash.

The system must validate at least:

- Total transaction amount.
- Customer cash due.
- Amount due at signing.
- Financed amount.
- Finance charge.
- Total repayment.
- Installment count.
- Standard installment.
- Balloon.
- Residual.
- Payment schedule.
- Loan-to-value ratio.
- Funding shortfall.

Calculation mismatch must block progression.

### Payment-Schedule Rules

- Payment schedule must match the selected Lender Decision and contract terms.
- Payment dates must be valid and ordered.
- Sequence numbers must be unique.
- Installment count must match the schedule.
- Schedule totals must reconcile with contractual totals.
- Balloon and residual amounts must appear in the applicable schedule.
- A generated schedule must be validated before signature.
- Material schedule changes require a new contract version or governed amendment.
- The schedule must not claim actual payment status.
- Actual payment and arrears data must come from the servicing authority.

### Template Rules

- Only approved active templates may be used.
- The template must match:
  - Jurisdiction.
  - Contract type.
  - Lender.
  - Finance program.
  - Customer language.
  - Execution method.
  - Required disclosures.
  - Effective period.
- Template version and hash must be preserved.
- An expired, withdrawn, or unapproved template must not be used.
- AI must not invent or alter legal clauses.
- Free-text contractual clauses require applicable legal approval.
- A template change after generation requires document invalidation and regeneration.

### Disclosure Rules

Before signature:

- Required disclosures must be identified.
- Required disclosures must be generated from approved sources.
- Required disclosures must be presented to all applicable parties.
- Required acknowledgements must be collected.
- Disclosure language must be appropriate and approved.
- Disclosure package version and hash must be preserved.
- Missing or expired disclosure evidence must block signature progression where required.
- Disclosure acknowledgement must remain distinct from contract signature.
- AI summaries must not replace the official disclosure text.
- AI must not waive a disclosure requirement.

### Document-Generation Rules

A contract document may be generated only when:

- Authoritative source validation passes.
- Required parties are known.
- Required terms are complete.
- Approved template is selected.
- Required calculations pass.
- Required reviews and approvals are valid.
- Required disclosures are available.
- No blocking conflict exists.
- Generation is idempotent.

The generated document must preserve:

- Contract version.
- Template version.
- Source snapshot.
- Document hash.
- Page count.
- Language.
- Generation timestamp.
- Security classification.

A generated document is not yet executed.

### Version and Immutability Rules

- Every Contract version must have a unique `financial_contract_id`.
- Every Contract series must have one stable `contract_series_id`.
- Contract version numbers must increase sequentially.
- Only one current version may exist in a Contract series.
- Draft versions may be edited under concurrency controls.
- Once a signature request is sent, material terms must be frozen.
- Material changes require a new version.
- Fully signed, effective, active, terminated, voided, or completed versions must remain immutable.
- Supersession must preserve the prior version.
- Circular supersession is prohibited.
- Retryable version creation must not create duplicates.

### Signature-Request Rules

A signature request may be issued only when:

- Contract status is eligible.
- Document generation completed.
- Unsigned document hash is current.
- Required disclosures are acknowledged where required.
- All parties and signing authorities are valid.
- Required signer sequence is defined.
- Required identity-verification controls pass.
- Signature provider is approved.
- Signature request is idempotent.
- No blocking compliance issue exists.

A signature request Command does not prove that the provider created the envelope.

External Confirmation is required where the signature provider is authoritative.

### Electronic-Signature Rules

Electronic signatures must preserve:

- Signer identity.
- Signer role.
- Signing authority.
- Contract version.
- Exact document hash.
- Signature timestamp.
- Provider reference.
- Completion certificate.
- Authentication method where permitted.
- Audit trail.
- IP or device evidence only where lawful and necessary.
- Consent to electronic execution where required.

Provider delivery, document view, or click activity does not prove a valid signature.

A signature must not be accepted for the wrong document version.

### Physical-Signature Rules

Physical execution must preserve:

- Exact printed document version.
- Document hash or equivalent control.
- Required original-document count.
- Signer identity verification.
- Witness or notary evidence where required.
- Signature date.
- Document custody.
- Receipt of originals.
- Verification of complete pages.
- Alteration detection.
- Controlled storage reference.

A scanned copy must not automatically replace the legally required original where originals are required.

### Signature-Completion Rules

A Financial Contract may become `FULLY_SIGNED` only when:

- Every required signer completed a valid signature.
- Every required acknowledgement completed.
- Required signing sequence was satisfied.
- Required witnesses or notarization completed.
- Signed document hash is available.
- Signature certificate or equivalent evidence exists.
- No signer declined or expired.
- No document-integrity conflict exists.
- Signature-provider Confirmation is received where applicable.

`FULLY_SIGNED` does not automatically mean `EFFECTIVE`.

### Effectiveness Rules

A Financial Contract may become `EFFECTIVE` only when:

- Contract is fully signed.
- Required countersignatures completed.
- Required disclosures remain valid.
- Selected Lender Decision remains valid.
- All applicable effectiveness conditions are satisfied.
- Cooling-off or rescission controls are satisfied.
- Required down-payment condition is met where applicable.
- Required insurance is valid.
- Required Vehicle and Deal eligibility remains valid.
- Required legal or compliance controls pass.
- Authoritative effectiveness Decision or Confirmation exists where required.
- `effective_at` is populated.
- No blocking dispute exists.

AI must not determine legal enforceability or contract effectiveness independently.

### Cooling-Off and Rescission Rules

- Applicable cooling-off or rescission rules must be configured by jurisdiction and contract type.
- Start and end times must be calculated deterministically.
- Required Customer notices must be presented.
- A rescission request must preserve Customer evidence and timestamp.
- Rescission must remain pending until legal and contractual validation completes.
- Effectiveness, funding, activation, Deal, and delivery workflows must respond according to applicable policy.
- AI must not decide whether a legal rescission right applies.

### Lien and Security Rules

Where a security interest is required:

- Applicable security type must be identified.
- Required registration authority must be configured.
- Registration request must be idempotent.
- A sent registration Command does not prove registration.
- External Confirmation must be preserved.
- Registration failure may block funding or activation.
- Security release must use a separate governed process.
- AI must not confirm lien registration or release.

### Insurance Rules

Where insurance is required:

- Required insurance type must be identified.
- Policy and provider must be verified.
- Coverage dates must be current.
- Vehicle identity must match.
- Required beneficiary or loss-payee details must be valid.
- Expired or cancelled coverage must block dependent progression.
- AI extraction may assist but does not create authoritative insurance verification.

### Funding Rules

The Financial Contract Domain Service is the sole canonical owner of the funding-request workflow.

A funding request may occur only when:

- The Finance Application handoff was accepted.
- The exact Finance Application version and selected Lender Decision remain valid.
- The Contract is effective or otherwise eligible under the approved product.
- Funding readiness is `READY`.
- Financial Contract terms match the Lender Decision.
- Required signatures and documents exist.
- Required Deal and Vehicle conditions pass.
- Required Customer contribution is confirmed where applicable.
- Required Lender conditions are satisfied or waived by the Lender.
- Required insurance and security controls pass.
- No blocking conflict exists.
- Required Human authority or an approved automation policy exists.
- The request uses a stable idempotency key.

For every funding request, the Financial Contract Domain Service must:

- Create and persist one canonical funding-request record.
- Create and persist the Funding Command before transmission.
- Reuse the same idempotency key for safe retries.
- Preserve the exact Contract, Finance Application, Lender Decision, Deal, amount, currency, and source versions.
- Remain pending until accepted External Confirmation or a governed terminal outcome.
- Reconcile duplicate, delayed, partial, conflicting, failed, or reversed outcomes.
- Publish immutable workflow Events.

A successful API, transport, or provider acknowledgement does not prove funding.

`FUNDED` requires authoritative evidence containing:

- Funding reference.
- Amount.
- Currency.
- Timestamp.
- Source authority.
- Source record version where available.
- Contract reference.
- Deal reference.
- Confirmation evidence.
- Reconciliation result.

Partial funding must remain explicit and must calculate a shortfall.

A funding failure must preserve the external reason, evidence, and retry or escalation eligibility.

Funding reversal must use a separate governed request or observation, a new Event, preserved original Confirmation, and complete reconciliation.

Finance Application and Deal receive read-only funding projections. They must not create Funding Commands or publish authoritative funding outcomes.

The Financial Contract Domain Service may publish an accepted canonical funding-confirmed, funding-failed, or funding-reversed fact only after preserving the external authority and Confirmation evidence.

### Activation Rules

A Financial Contract may become `ACTIVE` only when:

- Contract is effective.
- Required funding is confirmed.
- Required security or lien is registered where applicable.
- Required servicing account exists.
- Required Lender activation Confirmation exists.
- No material funding shortfall exists.
- No rescission, void, cancellation, or blocking dispute exists.
- `activated_at` is populated.

The exact activation conditions must remain configurable by product and authority.

### Servicing Rules

- Servicing state remains externally authoritative where a servicing platform exists.
- Balance, payment, arrears, settlement, default, repossession, and completion projections must preserve source and freshness.
- ASOS must not invent servicing state.
- A missed projected payment date does not independently prove arrears.
- Customer servicing communication must use approved authority and accurate source data.
- Contract completion requires authoritative servicing or settlement evidence.

### Amendment Rules

A post-effectiveness amendment requires:

- Authorized amendment request.
- Applicable Lender authorization.
- Legal review where required.
- Exact affected terms.
- Previous and revised values.
- Amendment document.
- Required disclosures.
- Required Customer acknowledgements.
- Required signatures.
- Effective date.
- Audit evidence.
- Reconciliation with servicing, Deal, and funding systems.

An amendment must not overwrite:

- Original signed contract.
- Original document hash.
- Original signatures.
- Original effective terms.
- Prior amendments.

AI must not approve or execute a contractual amendment.

### Cancellation Rules

Cancellation normally applies before effectiveness.

Cancellation requires:

- Valid reason.
- Authorized Human Decision or applicable legal authority.
- Review of signature state.
- Review of Lender Decision.
- Review of Deal and Payment activity.
- Review of pending funding or lien Commands.
- Required Customer and Lender notification.
- External Confirmation where applicable.
- Preserved contract history.

A Financial Contract must not be cancelled through an ordinary update after effectiveness.

### Void Rules

Voiding requires:

- Applicable legal basis.
- Authorized legal or compliance Decision.
- Supporting evidence.
- Review of Customer impact.
- Review of Deal, funding, security, Payment, and servicing consequences.
- Required external notifications.
- External Confirmation where applicable.
- Preserved original executed document.

Voiding must not delete the contract.

### Termination Rules

Termination of an effective or active contract requires:

- Valid termination reason.
- Applicable contractual and legal authority.
- Settlement calculation where applicable.
- Customer and Lender communication.
- Security-release or repossession handling where applicable.
- Servicing reconciliation.
- Deal and accounting reconciliation.
- Effective termination timestamp.
- External Confirmation where applicable.

Termination does not automatically mean all financial obligations were settled.

### Completion Rules

A Financial Contract may become `COMPLETED` only when:

- Authoritative servicing or settlement source confirms completion.
- Contractual balance is settled or lawfully discharged.
- Applicable security interests are released or governed.
- Funding and Payment reconciliations are complete.
- Applicable termination or settlement obligations are resolved.
- Completion evidence exists.
- `completed_at` is populated.

A predicted zero balance must not create completion.

### Dispute Rules

- A material dispute may suspend applicable progression.
- Disputed terms, signatures, identity, disclosures, funding, or settlement must remain explicit.
- Original evidence must be preserved.
- Resolution requires authorized Human or external authority.
- AI may summarize dispute evidence but must not decide the legal outcome.
- Resolved disputes require new immutable Events and audit records.

### External Authority Rules

When an external Lender, signature provider, document platform, registration authority, funding authority, or servicing system is authoritative:

- ASOS must create approved Commands through Command Orchestration.
- Retryable Commands must use `idempotency_key`.
- Local state must remain pending until External Confirmation.
- Transport success does not equal business completion.
- Conflicting data must create reconciliation.
- Higher-authority evidence must not be silently overwritten.
- Missing Confirmation must trigger retry, polling, timeout, reconciliation, or Human escalation.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Contract creation must support idempotency.
- Contract-version creation must support idempotency.
- Document generation must support idempotency.
- Signature-envelope creation must support idempotency.
- Contract delivery must support idempotency.
- Effectiveness processing must support idempotency.
- Funding requests must support idempotency.
- Lien-registration requests must support idempotency.
- Activation requests must support idempotency.
- Amendment creation must support idempotency.
- Cancellation, void, and termination requests must support idempotency.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Contracts.
  - Contract versions.
  - Documents.
  - Signature envelopes.
  - Signature requests.
  - Funding requests.
  - Lien registrations.
  - Activation requests.
  - Amendments.
  - Cancellation records.
  - Termination records.

### Human Review Requirements

Human Review is required according to policy for:

- Party or identity conflict.
- Signing-authority conflict.
- Lender-term mismatch.
- Quotation or Deal mismatch.
- Calculation mismatch.
- Template exception.
- Missing or disputed disclosure.
- Physical-signature exception.
- Signature dispute.
- Contract effectiveness Decision.
- Rescission.
- Lien or security exception.
- Funding shortfall.
- Funding reversal.
- Contract amendment.
- Contract cancellation after signature.
- Contract voiding.
- Contract termination.
- Contract settlement dispute.
- Reopening a terminal Contract.
- Another material legal, financial, or compliance exception.

---

## 6. State Machine

### Allowed States

```text
DRAFT
VALIDATION_PENDING
REVIEW_PENDING
APPROVED
GENERATED
DISCLOSURE_PENDING
READY_FOR_SIGNATURE
SIGNATURE_PENDING
PARTIALLY_SIGNED
FULLY_SIGNED
EFFECTIVENESS_PENDING
EFFECTIVE
FUNDING_PENDING
ACTIVE
COMPLETED
CANCELLED
VOIDED
EXPIRED
TERMINATED
SUPERSEDED
DISPUTED
ARCHIVED
```

`FUNDING_PENDING` is the aggregate Financial Contract state while the governed funding workflow is unresolved.

`PARTIALLY_FUNDED`, `FUNDED`, `FAILED`, and `REVERSED` remain values of `ContractFundingStatus`.

### Principal Allowed Transitions

```text
DRAFT → VALIDATION_PENDING
DRAFT → CANCELLED
DRAFT → EXPIRED

VALIDATION_PENDING → DRAFT
VALIDATION_PENDING → REVIEW_PENDING
VALIDATION_PENDING → APPROVED
VALIDATION_PENDING → CANCELLED
VALIDATION_PENDING → EXPIRED
VALIDATION_PENDING → DISPUTED

REVIEW_PENDING → DRAFT
REVIEW_PENDING → APPROVED
REVIEW_PENDING → CANCELLED
REVIEW_PENDING → EXPIRED
REVIEW_PENDING → DISPUTED

APPROVED → GENERATED
APPROVED → DRAFT
APPROVED → CANCELLED
APPROVED → EXPIRED

GENERATED → DISCLOSURE_PENDING
GENERATED → READY_FOR_SIGNATURE
GENERATED → SUPERSEDED
GENERATED → CANCELLED
GENERATED → EXPIRED
GENERATED → DISPUTED

DISCLOSURE_PENDING → READY_FOR_SIGNATURE
DISCLOSURE_PENDING → GENERATED
DISCLOSURE_PENDING → CANCELLED
DISCLOSURE_PENDING → EXPIRED
DISCLOSURE_PENDING → DISPUTED

READY_FOR_SIGNATURE → SIGNATURE_PENDING
READY_FOR_SIGNATURE → SUPERSEDED
READY_FOR_SIGNATURE → CANCELLED
READY_FOR_SIGNATURE → EXPIRED
READY_FOR_SIGNATURE → DISPUTED

SIGNATURE_PENDING → PARTIALLY_SIGNED
SIGNATURE_PENDING → FULLY_SIGNED
SIGNATURE_PENDING → CANCELLED
SIGNATURE_PENDING → EXPIRED
SIGNATURE_PENDING → SUPERSEDED
SIGNATURE_PENDING → DISPUTED

PARTIALLY_SIGNED → FULLY_SIGNED
PARTIALLY_SIGNED → CANCELLED
PARTIALLY_SIGNED → EXPIRED
PARTIALLY_SIGNED → SUPERSEDED
PARTIALLY_SIGNED → DISPUTED

FULLY_SIGNED → EFFECTIVENESS_PENDING
FULLY_SIGNED → EFFECTIVE
FULLY_SIGNED → VOIDED
FULLY_SIGNED → DISPUTED

EFFECTIVENESS_PENDING → EFFECTIVE
EFFECTIVENESS_PENDING → VOIDED
EFFECTIVENESS_PENDING → EXPIRED
EFFECTIVENESS_PENDING → DISPUTED

EFFECTIVE → FUNDING_PENDING
EFFECTIVE → ACTIVE
EFFECTIVE → TERMINATED
EFFECTIVE → VOIDED
EFFECTIVE → DISPUTED

FUNDING_PENDING → ACTIVE
FUNDING_PENDING → EFFECTIVE
FUNDING_PENDING → TERMINATED
FUNDING_PENDING → VOIDED
FUNDING_PENDING → DISPUTED

ACTIVE → COMPLETED
ACTIVE → TERMINATED
ACTIVE → VOIDED
ACTIVE → DISPUTED

DISPUTED → previous permitted non-terminal state
DISPUTED → VOIDED
DISPUTED → TERMINATED

COMPLETED → ARCHIVED
CANCELLED → ARCHIVED
VOIDED → ARCHIVED
EXPIRED → ARCHIVED
TERMINATED → ARCHIVED
SUPERSEDED → ARCHIVED
```

Returning from `DISPUTED` requires an accepted resolution and supporting evidence.

`FUNDING_PENDING → ACTIVE` requires accepted authoritative funding Confirmation and every other configured activation condition.

`FUNDING_PENDING → EFFECTIVE` is permitted only through a governed failed, cancelled, reversed, short-funded, or reconciliation outcome that leaves the Contract effective but not active.

### Forbidden Ordinary Transitions

```text
DRAFT → GENERATED
DRAFT → READY_FOR_SIGNATURE
DRAFT → FULLY_SIGNED
DRAFT → EFFECTIVE
DRAFT → ACTIVE

VALIDATION_PENDING → SIGNATURE_PENDING
REVIEW_PENDING → SIGNATURE_PENDING
APPROVED → FULLY_SIGNED

GENERATED → FULLY_SIGNED
DISCLOSURE_PENDING → FULLY_SIGNED
READY_FOR_SIGNATURE → FULLY_SIGNED

SIGNATURE_PENDING → EFFECTIVE
PARTIALLY_SIGNED → EFFECTIVE

FULLY_SIGNED → ACTIVE

EFFECTIVE → CANCELLED
FUNDING_PENDING → CANCELLED
ACTIVE → CANCELLED

CANCELLED → READY_FOR_SIGNATURE
CANCELLED → EFFECTIVE
CANCELLED → ACTIVE

VOIDED → EFFECTIVE
VOIDED → ACTIVE

EXPIRED → SIGNATURE_PENDING
EXPIRED → EFFECTIVE
EXPIRED → ACTIVE

TERMINATED → ACTIVE
COMPLETED → ACTIVE

SUPERSEDED → SIGNATURE_PENDING
SUPERSEDED → EFFECTIVE

ARCHIVED → DRAFT
ARCHIVED → EFFECTIVE
ARCHIVED → ACTIVE
```

Corrections to terminal or legally significant outcomes require a separate governed correction, amendment, dispute, reversal, or legal-restoration workflow.

### Entering DRAFT

Requires:

- Valid Tenant context.
- Eligible Finance Application.
- Selected Lender Decision.
- Eligible Deal.
- Quotation.
- Customer.
- Minimum party structure.
- Contract series.
- Creation authority.
- Idempotency protection.
- Initial audit evidence.

### Entering VALIDATION_PENDING

Requires:

- Contract source snapshots.
- Contract-term validation request.
- Current source-record versions.
- Validation policy.
- Responsible workflow.

### Entering REVIEW_PENDING

Requires:

- Identified review reasons.
- Review-request snapshot.
- Assigned authorized reviewers.
- Applicable legal, compliance, finance, or commercial scope.

### Entering APPROVED

Requires:

- Source validation passed.
- Required reviews completed.
- Required approvals completed.
- Current Lender Decision.
- Current Quotation and Deal.
- Current party verification.
- No blocking conflict.
- Approval evidence.

### Entering GENERATED

Requires:

- Approved template.
- Approved Contract terms.
- Completed deterministic calculations.
- Generated unsigned document.
- Document hash.
- Contract version.
- Template version.
- Document snapshot.
- Generation timestamp.

### Entering DISCLOSURE_PENDING

Requires:

- Applicable disclosure package.
- Required disclosures identified.
- Customer and party delivery workflow.
- Applicable acknowledgement requirements.

### Entering READY_FOR_SIGNATURE

Requires:

- Generated current document.
- Required disclosures acknowledged or ready according to policy.
- Verified parties.
- Valid signing authorities.
- Defined signer order.
- Approved execution method.
- No blocking conflict.

### Entering SIGNATURE_PENDING

Requires:

- Signature request.
- Exact unsigned document hash.
- Signature provider or physical-signing workflow.
- Required signer records.
- Idempotency key.
- External Confirmation where applicable.

### Entering PARTIALLY_SIGNED

Requires:

- At least one valid required signature.
- At least one required signature still outstanding.
- Current document hash.
- Accepted signature evidence.
- No invalidated signature.

### Entering FULLY_SIGNED

Requires:

- All required signatures.
- Required acknowledgements.
- Required witnesses or notary.
- Signed document.
- Signed document hash.
- Signature certificate or equivalent evidence.
- External Confirmation where applicable.
- No unresolved signature conflict.

### Entering EFFECTIVENESS_PENDING

Requires:

- Fully signed Contract.
- Defined effectiveness conditions.
- Outstanding condition evaluation.
- Applicable cooling-off or legal controls.
- Responsible authority.

### Entering EFFECTIVE

Requires:

- Fully signed Contract.
- All required effectiveness conditions satisfied.
- Valid Lender Decision.
- Current Deal and Vehicle eligibility.
- Required Customer contribution condition where applicable.
- Required insurance and compliance state.
- Effectiveness evidence.
- `effective_at`.

### Entering FUNDING_PENDING

Requires:

- Effective or otherwise eligible Contract.
- Accepted Finance Application handoff.
- Current selected Lender Decision.
- Funding readiness `READY`.
- Valid canonical funding-request record.
- Required Lender conditions.
- Required Customer contribution.
- Funding amount and currency.
- Stable funding idempotency key.
- Persisted Funding Command.
- Required Human authority or approved automation policy.
- No blocking conflict.
- Audit evidence.

The Financial Contract Domain Service creates and owns the Funding Command.

The Contract must remain `FUNDING_PENDING` until authoritative Confirmation, failure, reversal, cancellation, or reconciliation supports the next transition.

A provider acknowledgement does not support `ACTIVE` or `FUNDED`.

### Entering ACTIVE

Requires:

- Effective Contract.
- Required funding Confirmation.
- Required security registration.
- Required servicing setup.
- Required activation Confirmation.
- No funding shortfall.
- No active rescission, void, or blocking dispute.
- `activated_at`.

### Entering COMPLETED

Requires:

- Authoritative settlement or servicing completion.
- Contractual balance settled or lawfully discharged.
- Security release handled.
- Funding and Payment reconciled.
- Completion evidence.
- Completion timestamp.

### Entering CANCELLED

Requires:

- Contract not effective unless an exceptional legal workflow applies.
- Valid cancellation reason.
- Cancellation authority.
- Pending Command handling.
- Required notifications.
- Audit evidence.

### Entering VOIDED

Requires:

- Applicable legal basis.
- Authorized legal or compliance Decision.
- Supporting evidence.
- External Confirmation where required.
- Consequence and reconciliation plan.

### Entering EXPIRED

Requires:

- Applicable Draft, approval, document, signature, or effectiveness validity period ended.
- No accepted progression before expiry.
- Expiration reason.
- Expiration timestamp.

### Entering TERMINATED

Requires:

- Effective or active Contract.
- Valid termination authority.
- Termination reason.
- Settlement or consequence handling.
- Servicing and security reconciliation.
- Effective termination timestamp.
- External Confirmation where required.

### Entering SUPERSEDED

Requires:

- Valid replacement Contract version.
- Same Contract series.
- Atomic current-version update.
- Supersession reason.
- Replacement reference.
- Required signing-workflow invalidation.

### Terminal States

For ordinary processing:

- `COMPLETED`
- `CANCELLED`
- `VOIDED`
- `TERMINATED`
- `ARCHIVED`

`EXPIRED` and `SUPERSEDED` may lead only to governed replacement or archival workflows.

### Correction, Amendment, and Reopening

Correcting or reopening a material Financial Contract outcome requires:

- Authorized Human Decision.
- Legal and compliance review.
- Supporting evidence.
- Original document preservation.
- Contract, Finance Application, Deal, funding, servicing, and security reconciliation.
- New Contract version or Amendment where applicable.
- New Events.
- Complete audit history.

AI Agents must not independently reopen, correct, amend, void, terminate, or restore a Financial Contract.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
- Contract version.
- Document hash.
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

- Every Financial Contract belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant contract processing requires an approved legal and technical mechanism.

### Customer

- Every Financial Contract references one primary Customer.
- Customer identity remains governed by Customer Domain Service.
- Contract snapshots preserve the legal context used at execution.
- Customer updates after execution must not rewrite the original party snapshot.

### Finance Application

- Every Financial Contract references one eligible Finance Application.
- It must reference one exact immutable application version.
- It must preserve the selected Lender Decision and offer.
- Finance Application data must not be copied beyond contractual necessity.

### Opportunity

- Every Financial Contract references one Opportunity.
- Opportunity owns the commercial pursuit.
- Financial Contract status may inform Opportunity and Deal workflows.
- Contract signature must not directly mark Opportunity `WON` without governed Deal state.

### Quotation

- Every Financial Contract references one exact Quotation version.
- Quotation owns the Customer-facing automotive commercial offer.
- Financial Contract preserves the commercial terms used contractually.
- A Quotation revision may require a new Contract version.

### Vehicle

- Every Vehicle-related Financial Contract references one Vehicle.
- Vehicle identity remains governed by Vehicle.
- The Contract preserves the identity and specification snapshot used at execution.

### Inventory Record

- A physical-unit Contract may reference one Inventory Record.
- Inventory Record owns availability, Reservation, Allocation, sale, and delivery context.
- Financial Contract signature does not reserve or allocate Inventory.

### Trade-In

- Financial Contract may reference one or more permitted Trade-In projections.
- Trade-In owns appraisal, payoff, ownership, and acquisition.
- Contract preserves the exact Trade-In equity used in its terms.
- Payoff or equity changes may require re-underwriting or amendment.

### Deal

- Every Financial Contract references one Deal.
- Deal owns the automotive commercial transaction.
- Deal must preserve the current applicable Financial Contract.
- Financial Contract activation does not independently complete the Deal.

### Lender Decision

- Every Financial Contract references one exact authoritative Lender Decision version.
- A Contract must not combine terms from unrelated Lender Decisions.
- Lender Decision correction requires Contract revalidation and potentially a new version.

### Lender

A Lender relationship must preserve:

- Approved Lender identifier.
- Legal entity.
- Contractual role.
- Finance program.
- External Contract reference.
- Decision authority.
- Funding authority.
- Servicing authority where applicable.
- Security requirements.

### Contract Party

One Financial Contract may contain multiple Contract Parties.

Each party must preserve:

- Identity.
- Role.
- Signing authority.
- Required disclosures.
- Required signature.
- Evidence.
- Historical status.

### Appointment

Contract-signing or document-review sessions may be scheduled through Appointment.

Appointment completion does not prove Contract signature or effectiveness.

### Interaction

Interactions may provide:

- Contract delivery.
- Disclosure delivery.
- Customer questions.
- Signature reminders.
- Customer cancellation or rescission request.
- Amendment communication.
- Termination communication.

Original communication evidence remains governed by Interaction and the provider.

### Document Service

Document Service owns the controlled rendering and storage workflow.

Financial Contract preserves:

- Template.
- Template version.
- Document reference.
- Hash.
- Page count.
- Language.
- Generation evidence.
- Retention class.

### Signature Provider

Signature Provider may remain authoritative for:

- Envelope creation.
- Signer actions.
- Signature completion.
- Certificate.
- Provider audit trail.

The Financial Contract maintains a Canonical Projection and reconciliation state.

### Payment and Funding

Customer or third-party Payments and Lender funding remain separate workflows.

Payment Domain Service or the configured payment authority owns:

- Customer payment instructions.
- Payment authorization.
- Settlement and clearing.
- Refund and chargeback outcomes.
- Authoritative Customer-payment Confirmation.

Financial Contract Domain Service owns:

- Contract-level funding readiness.
- Canonical funding-request creation.
- Funding Commands and idempotency.
- Pending workflow state.
- External Confirmation tracking.
- Partial funding and shortfall handling.
- Failure, reversal, and reconciliation workflow.
- Activation evaluation based on accepted evidence.

The configured Lender, bank, or funding authority owns whether funding actually occurred.

Financial Contract preserves:

- Request and Command references.
- Requested amount and currency.
- External funding references.
- Confirmed amount and timestamp.
- Confirmation evidence.
- Failure, shortfall, and reversal evidence.
- Reconciliation state.
- Source and freshness metadata.

Finance Application and Deal consume only the projections required by their own workflows.

### Contract Servicing

Servicing provider owns authoritative:

- Installment payment state.
- Balance.
- Arrears.
- Fees applied during servicing.
- Settlement.
- Default.
- Repossession.
- Completion.

Financial Contract preserves only governed projections and references.

### Security and Lien Authority

Lien, title, registration, or collateral-security authorities provide authoritative security outcomes.

Financial Contract must preserve:

- Request.
- Command.
- Reference.
- Confirmation.
- Evidence.
- Release state.

### Contract Amendment

One Contract series may contain multiple amendments.

Every amendment must reference:

- Original Contract.
- Applicable current Contract version.
- Previous terms.
- Revised terms.
- Authority.
- Documents.
- Signatures.
- Effective date.

### Compliance and Legal Case

Financial Contract may reference:

- Legal review.
- Compliance case.
- Fraud case.
- Signature dispute.
- Contract dispute.
- Rescission case.
- Termination case.

Restricted case details must remain access-controlled.

### Supporting Child Records

Financial Contract may own or govern:

- Contract versions.
- Party records.
- Validation records.
- Review records.
- Approval Decisions.
- Disclosure packages.
- Generated documents.
- Delivery attempts.
- Signature envelopes.
- Signer records.
- Signature evidence.
- Effectiveness conditions.
- Activation conditions.
- Funding workflow requests, Commands, Confirmations, and outcome projections.
- Security-interest records.
- Payment-schedule snapshots.
- Amendments.
- Cancellation records.
- Void records.
- Termination records.
- Dispute records.
- Derived Intelligence.
- Data-quality issues.
- Reconciliation cases.
- Audit history.

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

The following are required Financial Contract Event concepts and do not replace the Event Catalog.

### Creation and Version Event Concepts

- Financial Contract series created.
- Financial Contract Draft created.
- Financial Contract Draft updated.
- Financial Contract validation requested.
- Financial Contract validation completed.
- Financial Contract validation failed.
- Financial Contract version created.
- Financial Contract superseded.
- Financial Contract conflict detected.
- Financial Contract conflict resolved.

### Review and Approval Event Concepts

- Contract review requested.
- Legal review completed.
- Compliance review completed.
- Finance review completed.
- Contract approval requested.
- Contract approved.
- Contract approval rejected.
- Contract approval expired.
- Contract approval revoked.

### Document Event Concepts

- Contract document generation requested.
- Contract document generated.
- Contract document generation failed.
- Contract document invalidated.
- Contract document delivered.
- Contract document delivery failed.
- Contract document reconciliation required.

### Disclosure Event Concepts

- Contract disclosure package prepared.
- Contract disclosures presented.
- Contract disclosure acknowledged.
- Contract disclosure rejected.
- Contract disclosure expired.
- Contract disclosure dispute opened.

### Signature Event Concepts

- Contract signature request created.
- Contract signature envelope confirmed.
- Contract signer viewed document.
- Contract signer acknowledged disclosure.
- Contract signer signed.
- Contract signer declined.
- Contract partially signed.
- Contract fully signed.
- Contract signature expired.
- Contract signature invalidated.
- Contract signature disputed.
- Contract signature reconciliation required.

### Effectiveness Event Concepts

- Contract effectiveness evaluation requested.
- Contract effectiveness condition satisfied.
- Contract effectiveness blocked.
- Contract became effective.
- Contract effectiveness reversed.
- Cooling-off period started.
- Cooling-off period ended.
- Contract rescission requested.
- Contract rescinded.

### Security and Lien Event Concepts

- Security-interest registration requested.
- Security-interest Command sent.
- Security interest registered.
- Security-interest registration failed.
- Security-interest release requested.
- Security interest released.
- Security-interest reconciliation required.

### Funding and Activation Event Concepts

- Contract funding readiness evaluated.
- Contract funding request created.
- Contract Funding Command persisted.
- Contract Funding Command sent.
- Contract funding acknowledgement received.
- Contract funding Confirmation pending.
- Contract partially funded.
- Contract funding confirmed.
- Contract funding failed.
- Contract funding reversed.
- Contract funding reconciliation required.
- Contract funding reconciliation resolved.
- Contract activation requested.
- Contract activated.
- Contract activation failed.
- Contract activation reversed.

Financial Contract Domain Service publishes funding-request and funding-workflow facts.

Funding integrations publish normalized external observations.

Financial Contract Domain Service may publish accepted canonical outcome facts only after validating and preserving the external authority, Confirmation evidence, source version, and reconciliation result.

Finance Application and Deal Domain Services must not publish authoritative Financial Contract funding-request or funding-outcome Events.

### Amendment Event Concepts

- Contract amendment requested.
- Contract amendment validated.
- Contract amendment approved.
- Contract amendment document generated.
- Contract amendment signed.
- Contract amendment became effective.
- Contract amendment rejected.
- Contract amendment cancelled.
- Contract amendment superseded.

### Cancellation, Void, and Termination Event Concepts

- Contract cancellation requested.
- Contract cancelled.
- Contract void requested.
- Contract voided.
- Contract termination requested.
- Contract termination approved.
- Contract terminated.
- Contract settlement confirmed.
- Contract completed.
- Contract archived.

### Dispute Event Concepts

- Contract dispute opened.
- Contract evidence requested.
- Contract dispute escalated.
- Contract dispute resolved.
- Contract correction required.

### Derived Intelligence Event Concepts

- Contract completeness analysis generated.
- Contract mismatch risk detected.
- Contract signature risk detected.
- Contract funding-delay risk detected.
- Contract document anomaly detected.
- Contract next action recommended.
- Contract Human Review recommended.

Derived Intelligence Events must not imply:

- Contract approval.
- Disclosure acknowledgement.
- Signature completion.
- Contract effectiveness.
- Lien registration.
- Funding.
- Activation.
- Amendment.
- Cancellation.
- Void.
- Termination.
- Settlement.
- Human Approval.
- External completion.

### Producer Rules

- Financial Contract Domain Service publishes accepted canonical Contract changes, funding-request workflow changes, and reconciled funding outcome facts.
- Finance Application Domain Service publishes accepted application, underwriting, readiness, and contracting-handoff facts.
- Finance Application Domain Service must not publish authoritative funding-request, funding-confirmed, funding-failed, or funding-reversed Events.
- Quotation Domain Service publishes accepted commercial-offer facts.
- Deal Domain Service publishes accepted Deal facts and non-owning completion-gate changes.
- Deal Domain Service must not publish authoritative funding-request or funding-outcome Events.
- Vehicle and Inventory Domain Services publish accepted eligibility facts.
- Trade-In Domain Service publishes accepted Trade-In facts.
- Document Service publishes accepted generation and storage facts.
- Signature integrations publish normalized provider observations.
- Funding, servicing, lien, and registration integrations publish normalized external observations.
- The configured external funding authority remains authoritative for the funding outcome.
- AI Agents may publish Agent-run, extraction, analysis, anomaly, or Recommendation Events.
- AI Agents must not publish authoritative signature, effectiveness, funding, activation, amendment, termination, or completion Events merely because they predicted or recommended the outcome.

### Event Requirements

Every material Financial Contract Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `financial_contract_id`.
- `contract_series_id`.
- Contract version.
- Customer.
- Finance Application.
- Lender Decision.
- Quotation.
- Deal.
- Vehicle and Inventory Record.
- Dealership and legal entity.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Document hash.
- Signature evidence reference.
- Correlation identifier.
- Causation identifier.
- Applied policy.
- Human Decision.
- Automation-policy reference where applicable.
- Command.
- External Confirmation.
- Evidence references.
- Security classification.

Events are immutable.

Corrections, supersession, rescission, amendment, cancellation, voiding, termination, funding reversal, and dispute resolution must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Contract-data completeness analysis.
- Source-term comparison.
- Lender-term mismatch detection.
- Quotation-to-Contract comparison.
- Deal-to-Contract comparison.
- Document classification.
- Contract clause classification.
- Missing-disclosure detection.
- Missing-signature detection.
- Signer-sequence Recommendation.
- Contract-summary drafting.
- Customer-language explanation drafting.
- Signature-workflow follow-up Recommendation.
- Funding-readiness issue detection.
- Document anomaly detection.
- Amendment comparison.
- Dispute-evidence summarization.
- Data-quality issue detection.
- Human Review preparation.
- Reconciliation-case summarization.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create or alter authoritative legal terms.
- Select an unapproved legal template.
- Waive required disclosures.
- Establish legal signing authority.
- Verify a legal signature.
- Sign for a party.
- Countersign a contract.
- Determine legal enforceability.
- Mark a Contract fully signed.
- Mark a Contract effective.
- Register a lien.
- Confirm Lender funding.
- Activate a Contract.
- Approve an amendment.
- Cancel an effective Contract.
- Void a Contract.
- Terminate a Contract.
- Confirm settlement.
- Mark a Contract completed.
- Resolve a legal dispute.
- Execute external Commands directly.
- Access Financial Contract data outside authorized Tenant and purpose scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Financial Contract identifier.
- Contract series.
- Contract version.
- Document hash where applicable.
- Supporting evidence.
- Source authority.
- Input-record versions.
- Data freshness.
- Applied Business Rules.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority.

### Contract Comparison

AI may compare:

- Finance Application.
- Lender Decision.
- Quotation.
- Deal.
- Generated Contract.
- Amendment.

The comparison must distinguish:

- Exact match.
- Formatting difference.
- Non-material textual difference.
- Material financial mismatch.
- Material party mismatch.
- Material Vehicle mismatch.
- Missing disclosure.
- Missing term.
- Unverified inference.

A comparison result remains Derived Intelligence until accepted by deterministic validation or authorized Human Review.

### Legal Text

AI may summarize approved legal text for workflow assistance.

AI-generated summaries:

- Must not replace official contract language.
- Must not be presented as legal advice.
- Must identify the source document and version.
- Must not omit material obligations.
- Must not invent rights or remedies.
- Must not alter disclosure meaning.
- Must remain clearly labelled as a summary.

AI-generated legal clauses must not become contractual terms without approved legal review and controlled template governance.

### Signature Analysis

AI may detect:

- Missing signer.
- Missing signature field.
- Possible document mismatch.
- Missing page.
- Inconsistent document version.
- Possible signing-order issue.

AI must not independently determine:

- Signature authenticity.
- Legal capacity.
- Signing authority.
- Whether a signature is enforceable.
- Whether a physical signature was forged.

### Customer-Facing Drafting

AI may draft contract-delivery, signature-reminder, or document-request communication only when:

- The communication purpose is permitted.
- Customer and Applicant permissions are valid.
- Contract version is current.
- Document hash is current.
- Approved template is used.
- Sensitive information is minimized.
- Human Approval or applicable automation policy is satisfied.

AI must not claim:

- The Contract is effective when only signed.
- Funding is complete when only requested.
- Vehicle ownership transferred.
- Vehicle delivery completed.
- A legal right applies without authoritative support.

### Action Class 2

Controlled contract delivery, disclosure delivery, signature reminders, and document requests may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Customer or Applicant permission.
- Purpose.
- Channel.
- Template.
- Contract version.
- Document hash.
- Contract status.
- Frequency.
- Quiet hours.
- Document sensitivity.
- Signature state.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision or External Authoritative Decision.

Examples include:

- Contract approval.
- Legal-template exception.
- Free-text clause approval.
- Contract execution authorization.
- Effectiveness Decision.
- Lien-registration exception.
- Funding request.
- Activation.
- Contract amendment.
- Cancellation after partial signature.
- Contract voiding.
- Contract termination.
- Settlement.
- Contract dispute resolution.

### AI Context and Embeddings

Financial Contract data must not enter unrestricted embeddings.

Normally excluded fields include:

- Legal names.
- National identifiers.
- Addresses.
- Contact details.
- Signature images.
- Signature certificates.
- Biometric evidence.
- Identity documents.
- Full Contract documents.
- Lender Decision documents.
- Rates and financial terms where not required.
- Funding references.
- Payment information.
- Servicing balances.
- Dispute evidence.
- Legal case information.
- Security and lien documents.

Approved redacted context may include:

- Contract status.
- Contract type.
- Missing-document categories.
- Missing-signature roles.
- General workflow blockers.
- Non-sensitive clause categories.
- Redacted summary.
- General next-action category.

Every vector record must enforce:

- `tenant_id`.
- Financial Contract access scope.
- Party-purpose scope.
- Source references.
- Document version.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Untrusted Documents and Prompt Injection

Contract documents, Applicant documents, Lender documents, and uploaded text are untrusted input.

AI Agents must treat them as data, not system instructions.

Document content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Trigger external Commands.
- Change Contract status.
- Change approved legal terms.
- Bypass signature controls.
- Bypass disclosure requirements.
- Confirm funding.
- Alter audit records.

### Explainability

Material Financial Contract Recommendations must explain:

- Evidence used.
- Source authority.
- Contract version.
- Document hash.
- Data freshness.
- Identified mismatch.
- Materiality.
- Missing information.
- Applied rules.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Required Human authority.
- External Confirmation requirements.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Financial Contract API behaviour.

### REST Resources

```text
GET    /api/v1/financial-contracts
POST   /api/v1/financial-contracts
GET    /api/v1/financial-contracts/{financial_contract_id}
PATCH  /api/v1/financial-contracts/{financial_contract_id}

POST   /api/v1/financial-contracts/{financial_contract_id}/validation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/review-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/approval-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/approval-decisions
POST   /api/v1/financial-contracts/{financial_contract_id}/version-requests

POST   /api/v1/financial-contracts/{financial_contract_id}/document-generation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/disclosure-presentation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/delivery-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/signature-envelope-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/signature-reconciliation-requests

POST   /api/v1/financial-contracts/{financial_contract_id}/effectiveness-evaluations
POST   /api/v1/financial-contracts/{financial_contract_id}/activation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/funding-readiness-checks
POST   /api/v1/financial-contracts/{financial_contract_id}/funding-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/funding-reversal-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/funding-reconciliation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/security-registration-requests

POST   /api/v1/financial-contracts/{financial_contract_id}/amendment-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/cancellation-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/void-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/termination-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/dispute-requests
POST   /api/v1/financial-contracts/{financial_contract_id}/correction-requests

GET    /api/v1/contract-series/{contract_series_id}
GET    /api/v1/contract-series/{contract_series_id}/versions
GET    /api/v1/financial-contracts/{financial_contract_id}/parties
GET    /api/v1/financial-contracts/{financial_contract_id}/validation
GET    /api/v1/financial-contracts/{financial_contract_id}/documents
GET    /api/v1/financial-contracts/{financial_contract_id}/disclosures
GET    /api/v1/financial-contracts/{financial_contract_id}/signature-status
GET    /api/v1/financial-contracts/{financial_contract_id}/funding-status
GET    /api/v1/financial-contracts/{financial_contract_id}/amendments
GET    /api/v1/financial-contracts/{financial_contract_id}/history
GET    /api/v1/financial-contracts/{financial_contract_id}/reconciliation
```

### Funding Mutation Ownership

`POST /api/v1/financial-contracts/{financial_contract_id}/funding-requests` is the canonical funding-request mutation.

Finance Application and Deal APIs must not expose a funding-request mutation.

The mutation must:

- Validate the current Financial Contract, Finance Application handoff, Lender Decision, Deal, amount, currency, and funding-readiness snapshot.
- Require the expected Financial Contract record version.
- Require a stable idempotency key.
- Create and persist the Funding Command through the Financial Contract Domain Service.
- Return pending workflow state without claiming funding occurred.
- Await authoritative External Confirmation.
- Preserve reconciliation evidence.

Read-only Finance Application and Deal endpoints may expose funding projections received from Financial Contract.

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Legal entity, dealership, branch, finance team, User, Lender, and template scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "finance_application_version": 2,
  "finance_application_version_hash": "sha256:5ea92db4...",
  "lender_submission_id": "086a44b6-daa1-4d10-a478-d7e306944d1e",
  "lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "lender_decision_version": "LENDER-DECISION-4",
  "selected_offer_snapshot_hash": "sha256:60dc3c44...",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "quotation_version": 1,
  "quotation_document_hash": "sha256:8ac44d5d...",
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "legal_entity_id": "aed9092a-b3db-4a28-b96e-bc4bbb4b99dc",
  "contract_type": "VEHICLE_FINANCE",
  "execution_method": "ELECTRONIC",
  "jurisdiction_code": "EG",
  "contract_language_code": "ar"
}
```

The request must include:

```text
Idempotency-Key: 24bba4da-31a8-49d0-b4da-d9d265c283eb
```

### Example Create Response

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "contract_series_id": "a64c3038-c0be-4d7e-8fa0-b2f5769776bf",
  "contract_number": "FC-2026-000087",
  "contract_version": 1,
  "status": "DRAFT",
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "document_generation_status": "NOT_STARTED",
  "disclosure_status": "NOT_STARTED",
  "signature_status": "NOT_STARTED",
  "effectiveness_status": "NOT_ASSESSED",
  "funding_status": "NOT_STARTED",
  "activation_status": "NOT_ASSESSED",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T20:00:00Z"
}
```

### Example Validation Request

```json
{
  "expected_record_version": 3,
  "validation_scope": [
    "PARTIES",
    "LENDER_TERMS",
    "COMMERCIAL_TERMS",
    "DEAL",
    "VEHICLE",
    "CALCULATIONS",
    "TEMPLATE_ELIGIBILITY"
  ]
}
```

### Example Validation Response

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "VALIDATION_PENDING",
  "contract_terms_validation_status": "PENDING",
  "validation_snapshot_hash": "sha256:1137da28...",
  "record_version": 4
}
```

### Example Document-Generation Request

```json
{
  "contract_template_id": "971f1b27-bf0a-42f4-bf62-5fc3e867a68b",
  "contract_template_version": "EG-VEHICLE-FINANCE-7.2",
  "expected_template_hash": "sha256:e6af0b43...",
  "expected_record_version": 7
}
```

The request must use an idempotency key.

A successful response may be:

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "GENERATED",
  "document_generation_status": "GENERATED",
  "unsigned_document_reference": "documents://financial-contracts/31174606/v1/unsigned",
  "unsigned_document_hash": "sha256:f8172c77...",
  "contract_template_version": "EG-VEHICLE-FINANCE-7.2",
  "record_version": 8
}
```

### Example Signature-Envelope Request

```json
{
  "signature_provider": "APPROVED_E_SIGNATURE_PROVIDER",
  "unsigned_document_hash": "sha256:f8172c77...",
  "signers": [
    {
      "contract_party_id": "a7a5d5f5-8a04-4e13-b68a-638c2a86f773",
      "signer_role": "PRIMARY_CUSTOMER",
      "signing_order": 1
    },
    {
      "contract_party_id": "d7ce096a-c4f7-42e3-a30a-43d65e42938d",
      "signer_role": "DEALERSHIP",
      "signing_order": 2
    }
  ],
  "expected_record_version": 10
}
```

The request must use an idempotency key.

A pending response may be:

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "SIGNATURE_PENDING",
  "signature_status": "REQUEST_PENDING",
  "signature_request_confirmation_status": "PENDING",
  "command_id": "cf1adbf1-b087-4d4e-91d5-c2583d1ab8fb",
  "record_version": 11
}
```

The API must not claim that the signature envelope exists until authoritative provider Confirmation is received.

### Example Fully Signed Projection

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "FULLY_SIGNED",
  "signature_status": "FULLY_SIGNED",
  "required_signer_count": 2,
  "completed_signer_count": 2,
  "fully_signed_at": "2026-08-04T13:30:00Z",
  "signed_document_reference": "documents://financial-contracts/31174606/v1/signed",
  "signed_document_hash": "sha256:8d44a011...",
  "signature_certificate_reference": "evidence://signature-certificates/env_01JBC8YQ4F",
  "signature_reconciliation_status": "RESOLVED",
  "effectiveness_status": "NOT_READY",
  "record_version": 15
}
```

The response must not describe the Contract as effective solely because it is fully signed.

### Example Effectiveness Evaluation

```json
{
  "expected_signed_document_hash": "sha256:8d44a011...",
  "expected_record_version": 15
}
```

A successful response may be:

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "EFFECTIVE",
  "effectiveness_status": "EFFECTIVE",
  "effective_at": "2026-08-05T00:00:00Z",
  "outstanding_effectiveness_conditions": [],
  "funding_readiness_status": "READY",
  "record_version": 16
}
```

### Example Funding Request

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "lender_decision_id": "c82a9da1-80e7-4464-8610-c77831acb0de",
  "funding_amount_requested": 1650000,
  "currency_code": "EGP",
  "funding_readiness_snapshot_hash": "sha256:4f2e78ab...",
  "expected_record_version": 16
}
```

The request must use a stable idempotency key.

A pending response may be:

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "FUNDING_PENDING",
  "funding_status": "PENDING_CONFIRMATION",
  "funding_request_reference": "FUND-REQ-2026-000441",
  "funding_command_id": "4cf52c54-f0c2-494f-b87f-09314193a354",
  "funding_confirmation_status": "PENDING",
  "record_version": 17
}
```

The pending response proves only that the governed funding workflow accepted the request.

After authoritative funding Confirmation and reconciliation, a response may be:

```json
{
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "status": "EFFECTIVE",
  "funding_status": "FUNDED",
  "funded_amount": 1650000,
  "funding_currency_code": "EGP",
  "funding_received_at": "2026-08-06T11:30:00Z",
  "funding_confirmation_status": "RECEIVED",
  "funding_confirmation_reference": "LENDER-FUND-889144",
  "funding_reconciliation_status": "RESOLVED",
  "activation_status": "READY",
  "record_version": 20
}
```

Funding Confirmation does not automatically move the Contract to `ACTIVE`.

A separate governed activation evaluation or request must confirm every configured activation condition.

### Example Amendment Request

```json
{
  "amendment_type": "PAYMENT_SCHEDULE_CHANGE",
  "amendment_reason": "LENDER_APPROVED_RESTRUCTURING",
  "affected_terms": [
    "term_months",
    "standard_installment_amount",
    "final_payment_date"
  ],
  "lender_authorization_reference": "evidence://lender-amendments/LD-2026-8891",
  "expected_contract_version": 1,
  "expected_record_version": 20
}
```

The amendment request must not modify the original signed document.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Contract-version validation.
- Field-authority validation.
- Source-term validation.
- Legal-template validation.
- Disclosure controls.
- Signature controls.
- Lifecycle validation.
- Deterministic calculations.
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

- Financial Contracts.
- Contract versions.
- Documents.
- Signature envelopes.
- Signature requests.
- Funding requests.
- Lien-registration requests.
- Activation requests.
- Amendments.
- Cancellation requests.
- Void requests.
- Termination requests.

### Pending External Confirmation

Operations requiring an external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "financial_contract_id": "31174606-d6ec-45d4-a4a1-30213fd5daa6",
  "command_id": "cf1adbf1-b087-4d4e-91d5-c2583d1ab8fb",
  "record_version": 11
}
```

The API must not describe the external operation as complete until authoritative evidence exists.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `CONTRACT_VERSION_CONFLICT`
- `FINANCE_APPLICATION_NOT_ELIGIBLE`
- `LENDER_DECISION_NOT_FOUND`
- `LENDER_DECISION_EXPIRED`
- `LENDER_TERMS_MISMATCH`
- `CUSTOMER_SELECTION_INVALID`
- `QUOTATION_MISMATCH`
- `DEAL_MISMATCH`
- `VEHICLE_MISMATCH`
- `INVENTORY_NOT_ELIGIBLE`
- `TRADE_IN_DATA_STALE`
- `PARTY_INCOMPLETE`
- `SIGNING_AUTHORITY_REQUIRED`
- `IDENTITY_VERIFICATION_REQUIRED`
- `TEMPLATE_NOT_ELIGIBLE`
- `TEMPLATE_EXPIRED`
- `CALCULATION_FAILED`
- `CALCULATION_MISMATCH`
- `DISCLOSURE_REQUIRED`
- `DISCLOSURE_ACKNOWLEDGEMENT_REQUIRED`
- `DOCUMENT_HASH_MISMATCH`
- `CONTRACT_IMMUTABLE`
- `SIGNATURE_REQUEST_NOT_READY`
- `SIGNATURE_EVIDENCE_INVALID`
- `SIGNATURE_INCOMPLETE`
- `EFFECTIVENESS_CONDITIONS_OUTSTANDING`
- `COOLING_OFF_ACTIVE`
- `INSURANCE_REQUIRED`
- `SECURITY_REGISTRATION_REQUIRED`
- `FUNDING_NOT_READY`
- `FUNDING_SHORTFALL`
- `HUMAN_APPROVAL_REQUIRED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `AMENDMENT_REQUIRED`
- `CONTRACT_TERMINAL`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Party-purpose scope.
- Field authority.
- Contract-version immutability.
- Legal-template validation.
- Disclosure controls.
- Signature controls.
- Deterministic calculations.
- Concurrency.
- Idempotency.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Financial Contract Domain Service, Policy Engine, Document Service, Signature controls, or approved Calculation Services.

---

## 11. Database Design

### Recommended Tables

```text
contract_series
financial_contracts
financial_contract_versions
financial_contract_parties
financial_contract_party_authorities
financial_contract_commercial_terms
financial_contract_finance_terms
financial_contract_payment_schedules
financial_contract_payment_schedule_entries
financial_contract_vehicle_security
financial_contract_validations
financial_contract_review_requests
financial_contract_review_decisions
financial_contract_approval_requests
financial_contract_approval_decisions
financial_contract_disclosure_packages
financial_contract_disclosure_acknowledgements
financial_contract_documents
financial_contract_delivery_attempts
financial_contract_signature_envelopes
financial_contract_signers
financial_contract_signature_evidence
financial_contract_effectiveness_conditions
financial_contract_activation_conditions
financial_contract_funding_workflows
financial_contract_security_registrations
financial_contract_servicing_projections
financial_contract_amendments
financial_contract_cancellations
financial_contract_void_records
financial_contract_terminations
financial_contract_settlements
financial_contract_disputes
financial_contract_external_references
financial_contract_external_confirmations
financial_contract_derived_intelligence
financial_contract_reconciliation_cases
financial_contract_data_quality_issues
financial_contract_status_history
financial_contract_record_versions
financial_contract_audit_log
```

### Contract Series Table

`contract_series` should contain:

- `contract_series_id`.
- `tenant_id`.
- Customer.
- Finance Application.
- Deal.
- Lender.
- Contract type.
- Series reference.
- Current Financial Contract identifier.
- Latest version.
- Series status.
- Created time.
- Updated time.

### Financial Contracts Table

The `financial_contracts` table should contain:

- Financial Contract version identifier.
- Contract series identifier.
- Tenant and organizational scope.
- Customer, Finance Application, Lender Decision, Quotation, Deal, Vehicle, Inventory, and Trade-In relationships.
- Contract version.
- Current-version indicator.
- Current lifecycle state.
- Current document, disclosure, signature, effectiveness, funding-workflow, funding-outcome, activation, and servicing projections.
- Current amendment, cancellation, void, termination, settlement, and dispute projections.
- Source and synchronization state.
- Data-quality and conflict state.
- Record version.
- Audit timestamps.

Historical, repeating, and sensitive details must remain in child or history tables.

### Primary Keys

```text
PRIMARY KEY (contract_series_id)
```

for `contract_series`.

```text
PRIMARY KEY (financial_contract_id)
```

for `financial_contracts`.

### Tenant Protection

Every Financial Contract-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_financial_contracts_tenant_status
  (tenant_id, status)

idx_financial_contracts_tenant_series_version
  (tenant_id, contract_series_id, contract_version)

idx_financial_contracts_tenant_current
  (tenant_id, contract_series_id, is_current_version)

idx_financial_contracts_tenant_customer
  (tenant_id, customer_id)

idx_financial_contracts_tenant_finance_application
  (tenant_id, finance_application_id)

idx_financial_contracts_tenant_lender_decision
  (tenant_id, lender_decision_id)

idx_financial_contracts_tenant_deal
  (tenant_id, deal_id)

idx_financial_contracts_tenant_vehicle
  (tenant_id, vehicle_id)

idx_financial_contracts_signature
  (tenant_id, signature_status, signature_expires_at)

idx_financial_contracts_effectiveness
  (tenant_id, effectiveness_status)

idx_financial_contracts_funding
  (tenant_id, funding_status)

idx_financial_contracts_activation
  (tenant_id, activation_status)

idx_financial_contracts_dispute
  (tenant_id, dispute_status)

idx_financial_contracts_reconciliation
  (tenant_id, reconciliation_status)

idx_financial_contracts_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, contract_number, contract_version)
```

```text
UNIQUE (tenant_id, contract_series_id, contract_version)
```

A partial unique constraint or equivalent transactional control should enforce one current version:

```text
UNIQUE (tenant_id, contract_series_id)
WHERE is_current_version = true
```

External reference uniqueness may use:

```text
UNIQUE (
  tenant_id,
  source_system,
  source_record_id
)
```

where the external source guarantees uniqueness.

A Lender contract reference may use:

```text
UNIQUE (
  tenant_id,
  lender_id,
  lender_contract_reference
)
```

where the Lender guarantees uniqueness.

### Immutable Contract Versions

The persistence layer must prevent material contractual updates when:

```text
status IN (
  'SIGNATURE_PENDING',
  'PARTIALLY_SIGNED',
  'FULLY_SIGNED',
  'EFFECTIVENESS_PENDING',
  'EFFECTIVE',
  'FUNDING_PENDING',
  'ACTIVE',
  'COMPLETED',
  'VOIDED',
  'TERMINATED',
  'SUPERSEDED',
  'ARCHIVED'
)
```

Permitted updates must be limited to governed projections and child workflows such as:

- Signature status.
- External Confirmation.
- Effectiveness workflow.
- Funding workflow.
- Activation workflow.
- Servicing projection.
- Amendment.
- Dispute.
- Reconciliation.
- Audit.

The frozen contractual snapshots and documents must remain immutable.

### Contract Versions

`financial_contract_versions` should preserve:

- Contract version identifier.
- Contract series.
- Version number.
- Source Finance Application version.
- Lender Decision version.
- Quotation version.
- Party snapshot.
- Commercial snapshot.
- Finance terms snapshot.
- Vehicle snapshot.
- Disclosure snapshot.
- Template.
- Document snapshot.
- Source-record versions.
- Creation reason.
- Supersession.
- Snapshot hashes.
- Created by.
- Created at.
- Related Events.

### Party Storage

`financial_contract_parties` should preserve:

- Contract Party identifier.
- Financial Contract.
- Customer or Applicant reference.
- Party role.
- Party type.
- Legal-name projection.
- Legal-identifier token.
- Signing-authority state.
- Identity-verification state.
- Required signature.
- Required acknowledgements.
- Effective period.
- Related Events.

Sensitive identity details should remain separated by classification and purpose.

### Commercial and Finance Terms Storage

Commercial and finance terms should be immutable snapshots linked to:

- Financial Contract.
- Contract version.
- Quotation.
- Lender Decision.
- Calculation reference.
- Currency.
- Integrity hash.
- Effective time.
- Related Events.

### Payment Schedule Storage

`financial_contract_payment_schedules` should preserve:

- Schedule identifier.
- Financial Contract.
- Contract version.
- Schedule version.
- Calculation reference.
- Currency.
- Payment count.
- Total.
- Start and end dates.
- Snapshot hash.
- Validation status.
- Related Events.

`financial_contract_payment_schedule_entries` should preserve:

- Entry identifier.
- Schedule.
- Sequence.
- Due date.
- Principal component.
- Interest component.
- Fee component.
- Tax component.
- Installment amount.
- Balloon or residual component.
- Currency.

Actual servicing status must not be written into contractual schedule entries.

### Validation Storage

`financial_contract_validations` should preserve:

- Validation identifier.
- Contract.
- Contract version.
- Scope.
- Source-record versions.
- Rules.
- Results.
- Mismatches.
- Materiality.
- Actor.
- Timestamp.
- Snapshot hash.
- Related Events.

### Review and Approval Storage

Review and approval tables should preserve:

- Request identifier.
- Contract.
- Contract version.
- Review or approval type.
- Triggered policy.
- Required role.
- Assigned reviewer.
- Decision.
- Conditions.
- Reason.
- Evidence.
- Effective period.
- Revocation.
- Related Events.

### Disclosure Storage

Disclosure tables should preserve:

- Disclosure package.
- Contract version.
- Disclosure type.
- Language.
- Version.
- Artifact reference.
- Artifact hash.
- Required parties.
- Presentation evidence.
- Acknowledgement evidence.
- Timestamp.
- Expiration.
- Dispute state.
- Related Events.

### Document Storage

`financial_contract_documents` should preserve:

- Document identifier.
- Contract.
- Contract version.
- Document type.
- Template.
- Template version.
- Language.
- Storage reference.
- Document hash.
- Page count.
- Generated time.
- Signed time where applicable.
- Security classification.
- Retention class.
- Legal-hold state.
- Related Events.

Contract binary content should remain in controlled document storage.

### Signature Storage

`financial_contract_signature_envelopes` should preserve:

- Envelope identifier.
- Contract.
- Contract version.
- Document hash.
- Provider.
- External envelope identifier.
- External version.
- Status.
- Command.
- Idempotency key.
- Request time.
- Confirmation.
- Expiration.
- Completion certificate.
- Reconciliation state.
- Related Events.

`financial_contract_signers` should preserve:

- Signer identifier.
- Contract Party.
- Role.
- Sequence.
- Required action.
- Identity-verification state.
- Signing-authority state.
- Signer status.
- View time.
- Signature time.
- Decline time.
- Provider reference.
- Evidence reference.
- Related Events.

### Effectiveness and Activation Storage

Effectiveness and activation tables should preserve:

- Condition identifier.
- Contract.
- Contract version.
- Condition type.
- Required evidence.
- Status.
- Authority.
- Satisfaction time.
- Confirmation.
- Expiration.
- Related Events.

### Funding Storage

`financial_contract_funding_workflows` should preserve:

- Funding workflow identifier.
- Tenant.
- Financial Contract and Contract version.
- Finance Application and accepted handoff version.
- Lender Decision and version.
- Deal.
- Funding authority.
- Requested amount and currency.
- Funding request reference.
- Funding Command.
- Funding idempotency key.
- Request creation and transmission times.
- Transport acknowledgement.
- Pending and retry state.
- External authority reference.
- External Confirmation.
- Source record version.
- Confirmed amount and time.
- Partial-funding and shortfall state.
- Failure reason and evidence.
- Reversal request, Command, idempotency key, and outcome.
- Reconciliation state.
- Projection observation, synchronization, and freshness state.
- Related Events.

Funding Commands and idempotency belong to Financial Contract Domain Service.

Authoritative funding outcomes remain external and must be stored as source-attributed projections with Confirmation evidence.

### Security Registration Storage

Security-registration tables should preserve:

- Registration identifier.
- Contract.
- Vehicle.
- Security type.
- Registration authority.
- Command.
- Idempotency key.
- Requested time.
- External reference.
- Confirmation.
- Registered time.
- Release state.
- Evidence.
- Reconciliation state.
- Related Events.

### Amendment Storage

`financial_contract_amendments` should preserve:

- Amendment identifier.
- Contract series.
- Applicable Contract version.
- Amendment version.
- Type.
- Reason.
- Previous terms hash.
- Revised terms.
- Revised terms hash.
- Lender authorization.
- Legal approval.
- Document.
- Signatures.
- Effective date.
- Supersession.
- Status.
- Related Events.

### Cancellation, Void, and Termination Storage

Separate tables should preserve:

- Request.
- Contract and version.
- Reason.
- Actor.
- Authority.
- Human Decision.
- Evidence.
- Command.
- External Confirmation.
- Effective timestamp.
- Financial and legal consequences.
- Reconciliation state.
- Related Events.

### Derived Intelligence

Derived Financial Contract records must remain separate from authoritative legal, signature, funding, and servicing fields.

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

Financial Contract audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw personal, contractual, signature, and financial values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Legal entity.
- Dealership.
- Lender.
- Contract date.
- Effective date.
- Retention class.
- Security classification.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Contract-series integrity.
- Version uniqueness.
- Immutability.
- Signature evidence.
- Referential integrity.
- Legal hold.
- Audit integrity.

### Hard Deletion

A Financial Contract must not be hard-deleted when referenced by:

- Customer journey.
- Opportunity.
- Quotation.
- Finance Application.
- Vehicle.
- Inventory Record.
- Trade-In.
- Deal.
- Payment.
- Funding.
- Servicing.
- Lender submission.
- Lender Decision.
- Document.
- Signature evidence.
- Security registration.
- Amendment.
- Interaction.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Compliance or legal evidence.
- Audit evidence.

Cancellation, voiding, termination, supersession, archival, anonymization, governed redaction, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Legal names, contact and address references |
| `HIGHLY_SENSITIVE_IDENTIFIER` | National identifiers and identity-document references |
| `CONTRACTUAL_RESTRICTED` | Contract terms and obligations |
| `FINANCIAL_RESTRICTED` | Principal, rates, payments, funding, balances |
| `LENDER_CONFIDENTIAL` | Lender Decision, conditions, program information |
| `SIGNATURE_RESTRICTED` | Signature evidence, certificates, provider references |
| `LEGAL_AND_COMPLIANCE` | Legal reviews, disputes, void and termination evidence |
| `SECURITY_INTEREST` | Lien, collateral, and registration information |
| `CUSTOMER_DOCUMENT` | Generated and signed contract documents |
| `DERIVED_INTELLIGENCE` | Risk and completeness Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, and history |

### Authentication

Every internal Financial Contract operation requires an authenticated Human or service identity.

Customer and signer access must use an approved secure authentication or identity-verification mechanism.

Anonymous unrestricted access to Contract documents, disclosures, signatures, or financial terms is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Legal entity.
- Branch.
- Finance team.
- Assigned User.
- Contract Party.
- Customer relationship.
- Deal relationship.
- Lender.
- Role.
- Requested field.
- Requested action.
- Contract status.
- Contract version.
- Monetary value.
- Data classification.
- Business purpose.
- Delegated authority.
- Legal hold.
- Applicable Consent or lawful basis.

### Example Role Boundaries

#### Sales Consultant

May access permitted:

- Contract workflow summary.
- Customer-facing signing status.
- Approved next-action category.
- General funding-readiness projection required for the Deal.

Must not access without explicit authority:

- Full Financial Contract.
- Internal Lender information.
- Signature certificates.
- National identifiers.
- Detailed finance calculations.
- Dispute evidence.
- Funding instructions.
- Servicing balances.

#### Finance Specialist

May access assigned Financial Contracts and perform permitted:

- Contract preparation.
- Document coordination.
- Disclosure coordination.
- Signature coordination.
- Lender communication.
- Funding preparation.
- Reconciliation.

Finance Specialist access does not authorize:

- Altering Lender terms.
- Waiving disclosures.
- Establishing legal validity.
- Confirming signatures without evidence.
- Confirming funding without evidence.
- Voiding or terminating a Contract outside delegated authority.

#### Finance Manager

May perform configured:

- Contract approval.
- Commercial or finance exception review.
- Signature-workflow escalation.
- Funding-readiness approval.
- Amendment review.
- Reconciliation review.

Manager access does not automatically authorize:

- Lender underwriting override.
- Legal-clause approval.
- Signature forgery determination.
- Legal voiding.
- Cross-Tenant access.

#### Legal Reviewer

May access Contract terms, templates, disclosures, amendments, rescission, void, termination, and dispute evidence required for an assigned matter.

Legal access must remain purpose-limited and audited.

#### Compliance Reviewer

May access identity, Consent, compliance, signature, fraud, and regulatory evidence required for the assigned review.

#### Contract Administrator

May manage:

- Approved templates.
- Document generation.
- Version relationships.
- Storage.
- Signature-envelope coordination.
- Contract records.

Contract Administrator access does not authorize contractual approval or legal interpretation.

#### Funding Specialist

May access the minimum Contract, Lender Decision, Deal, and funding information required to request and reconcile funding.

#### Data Steward

May review:

- Duplicate Contracts.
- Version conflicts.
- Source mappings.
- Relationship inconsistencies.
- Data-quality issues.
- Reconciliation cases.

Access to raw Contract and signature evidence must remain minimized.

#### AI Agent

May access only the minimum Financial Contract context required for its approved task.

AI access must be:

- Tenant-scoped.
- Party-purpose-scoped.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to identity, full documents, signatures, Lender Decision details, funding data, disputes, and legal evidence.

#### Integration Service

May access only fields required for an approved Document, Signature, Lender, funding, lien, registration, or servicing integration.

Integration services must not access unrelated Customer or Contract information.

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

- National identifiers.
- Party addresses.
- Full Contract terms.
- Rates and payment obligations.
- Signature evidence.
- Lender Decision artifacts.
- Funding references.
- Security-interest records.
- Legal disputes.
- Settlement information.

### Contract Document Security

Financial Contract documents must:

- Use controlled storage.
- Use authenticated access.
- Use non-predictable references.
- Preserve document hashes.
- Preserve template and Contract versions.
- Prevent unauthorized modification.
- Prevent public indexing.
- Prevent uncontrolled sharing.
- Prevent unrestricted download.
- Support legal holds.
- Support access revocation where legally permitted.
- Preserve access logs.
- Follow retention requirements.

### Signature Security

Signature evidence must:

- Use approved providers or legally accepted processes.
- Preserve signer identity and authority.
- Preserve exact document hash.
- Preserve timestamps.
- Preserve completion certificate.
- Prevent replay.
- Prevent signature reuse across versions.
- Prevent wrong-document signing.
- Prevent unauthorized signer substitution.
- Use controlled storage.
- Exclude raw biometric or signature-image data from unrestricted systems.
- Be excluded from Prompts and general-purpose embeddings.

### Physical Document Security

Physical originals must use controlled custody processes including:

- Document identifier.
- Contract version.
- Original count.
- Storage location.
- Custodian.
- Receipt time.
- Transfer history.
- Verification status.
- Legal hold.
- Loss or damage incident handling.

### Lender Decision Protection

Lender Decision data must:

- Preserve authoritative source.
- Preserve artifact and hash.
- Be immutable.
- Be restricted to authorized roles.
- Prevent unauthorized modification.
- Remain distinguishable from ASOS predictions.
- Follow Lender contractual restrictions.

### Disclosure Security

Disclosure packages and acknowledgement evidence must:

- Preserve exact version.
- Preserve language.
- Preserve party.
- Preserve presentation time.
- Preserve acknowledgement time.
- Preserve document hash.
- Prevent unauthorized replacement.
- Remain available for legal and regulatory audit.

### Funding and Banking Protection

Financial Contract funding data must use:

- Encryption.
- Tokenized account references where applicable.
- Field-level authorization.
- Controlled Payment and banking instructions.
- Export restrictions.
- Command approval.
- Idempotency protection.
- External Confirmation.
- Source-version validation.
- Reconciliation controls.
- Audit logging.

The Financial Contract Domain Service may create governed Funding Commands but must not invent or overwrite the external funding outcome.

Funding or banking instructions must never be copied into:

- Prompts.
- Ordinary Logs.
- Public documents.
- General-purpose embeddings.
- Unapproved Customer messages.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Contract matching.
- Search.
- Document storage.
- Signature workflows.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Funding integrations.
- Servicing integrations.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Financial Contract Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational and legal-entity scope.
- Contract identifier and version.
- Document hash where applicable.
- Requested action.
- Current record version.
- Field-level authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Financial Contract activity must record:

- `tenant_id`.
- `financial_contract_id`.
- `contract_series_id`.
- Contract version.
- Customer, Applicant, Finance Application, Quotation, Deal, Vehicle, Trade-In, and Lender references.
- Lender Decision version.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
- Template and document hash.
- Disclosure evidence.
- Signature evidence.
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

- Cross-Tenant Contract access attempts.
- Unauthorized Contract generation.
- Unauthorized legal-template use.
- Contract-term alteration.
- Document-hash mismatch.
- Wrong-Customer document access.
- Disclosure bypass.
- Signature replay.
- Wrong-version signing.
- Unauthorized signer substitution.
- Signature-certificate alteration.
- False effectiveness recording.
- Unauthorized funding request.
- False funding Confirmation.
- Lien-registration manipulation.
- Unauthorized amendment.
- Unauthorized void or termination.
- Contract export.
- Command replay.
- External Confirmation mismatch.
- AI access outside approved scope.
- Prompt-injection attempts inside Contract documents.
- Audit-log tampering.

### Contract Integrity

The platform must detect or prevent:

- Multiple current Contract versions.
- Material post-signature edits.
- Document replacement after signature.
- Signature applied to the wrong document.
- Contract generated from an expired Lender Decision.
- Contract terms inconsistent with the Lender Decision.
- Contract terms inconsistent with the Quotation or Deal.
- Missing required party.
- Missing required disclosure.
- False signature completion.
- False effectiveness.
- False funding.
- False activation.
- Amendment without authority.
- Termination without settlement handling.
- Contract status manipulation.

### Privacy and Retention

Financial Contract retention must follow:

- Applicable law.
- Tenant policy.
- Lender agreements.
- Contractual requirements.
- Financial and accounting obligations.
- Signature-provider obligations.
- Dispute requirements.
- Legal holds.
- Regulatory requirements.
- Audit requirements.

Privacy workflows must support applicable:

- Access.
- Correction.
- Restriction.
- Export.
- Anonymization.
- Deletion where legally permitted.
- Dispute handling.

Privacy actions must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Document stores.
- Signature-provider systems where lawfully required.
- Backups according to policy.

Required contractual, legal, lending, financial, security, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Contract generation.
- Contract delivery.
- Disclosure delivery.
- Signature requests.
- Automated signature reminders.
- Contract effectiveness processing.
- Funding requests.
- Activation.
- Lien registration.
- Contract amendments.
- External write-back.
- AI Contract analysis.
- Contract export.
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
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Deal](./Deal.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Financial Contract baseline.

Financial Contract remains separate from Finance Application, Quotation, Deal, Customer Payment, external funding authority, and servicing authority.

Finance Application owns underwriting, readiness, and the governed contracting handoff.

Financial Contract owns the contractual lifecycle and the canonical funding-request workflow after a valid Contract exists.

Financial Contract Domain Service creates Funding Commands, owns funding idempotency, tracks pending state, processes accepted External Confirmations, handles partial funding, failure, reversal, and reconciliation, and evaluates activation.

The configured Lender, bank, or funding authority owns the authoritative funding outcome.

Deal owns commercial completion gates and stores only a non-owning funding projection and reconciliation references.

Contract generation does not prove Customer delivery.

Disclosure acknowledgement does not prove Contract signature.

A fully signed Contract is not automatically effective.

An effective Contract is not automatically funded or active.

Funding Confirmation does not automatically prove activation.

Executed Contract versions are immutable.

Material contractual changes require a governed new version or Amendment.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
