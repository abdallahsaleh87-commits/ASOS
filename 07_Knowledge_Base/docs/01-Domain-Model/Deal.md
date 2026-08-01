# Deal

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Deal Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Deal Object represents one governed automotive commercial transaction accepted for controlled execution between a Customer and an authorized dealership legal entity.

A Deal normally begins after:

- A valid Customer and Opportunity exist.
- The Customer has made a verifiable commercial commitment.
- An accepted Quotation or another approved commercial basis exists.
- The dealership has accepted the transaction for execution.
- The Vehicle or approved Vehicle configuration is identified.
- Applicable commercial approvals are valid.
- No blocking legal, compliance, Inventory, identity, or authority conflict exists.

The Deal coordinates the governed transaction across:

- Customer commitment.
- Commercial-term preservation.
- Vehicle Reservation and Allocation requests.
- Inventory eligibility.
- Trade-In integration.
- Finance Application integration.
- Financial Contract integration.
- Customer Payment requirements.
- Lender funding requirements.
- Compliance and document requirements.
- Vehicle registration and title requirements.
- Insurance requirements.
- Delivery preparation.
- Vehicle handover.
- Accounting and DMS posting.
- Sale Confirmation.
- Cancellation.
- Unwind.
- Replacement Deal.
- Commercial and operational reconciliation.

### Deal Domain Boundary

The Deal is the central automotive transaction aggregate.

It owns:

- The canonical transaction identity.
- The exact accepted commercial basis.
- The transaction participants.
- The transaction execution workflow.
- Completion-condition orchestration.
- Transaction-level approval state.
- Transaction-level readiness state.
- Cancellation and unwind coordination.
- Final transaction outcome projection.
- References to authoritative child workflows.
- Required audit and reconciliation context.

The Deal does not independently own authoritative:

- Customer identity.
- Vehicle identity.
- Inventory availability.
- Vehicle Reservation.
- Vehicle Allocation.
- Quotation terms.
- Trade-In appraisal.
- Lender underwriting Decision.
- Financial Contract terms.
- Contract signature.
- Customer Payment settlement.
- Lender funding.
- Registration.
- Insurance.
- Vehicle delivery.
- Accounting posting.
- Revenue recognition.

Those facts belong to their responsible Domain Services or configured authoritative external systems.

### Deal and Opportunity Separation

`Opportunity` represents the active commercial pursuit.

`Deal` represents the governed transaction created after sufficient Customer and dealership commitment.

An Opportunity may progress through:

```text
Interest
Qualification
Commercial pursuit
Quotation
Negotiation
Commitment
Deal creation
```

Deal creation may support transitioning an Opportunity to `WON`.

However:

```text
Opportunity WON
  ≠ Contract signed

Opportunity WON
  ≠ Payment settled

Opportunity WON
  ≠ Lender funded

Opportunity WON
  ≠ Vehicle delivered

Opportunity WON
  ≠ Deal completed
```

The Opportunity remains the historical commercial pursuit.

The Deal becomes the governed execution record.

### Deal and Quotation Separation

`Quotation` owns the versioned Customer-facing commercial offer.

`Deal` owns the transaction created from an accepted commercial basis.

The Deal must preserve the exact:

- `quotation_id`.
- `quotation_series_id`.
- `quotation_version`.
- Issued document hash.
- Accepted document hash.
- Customer acceptance evidence.
- Accepted commercial snapshot.
- Calculation snapshot.
- Pricing and approval evidence.

The Deal must not silently modify the accepted Quotation.

A material commercial change after Deal creation may require:

- A new Quotation version.
- Customer reconfirmation.
- New approval.
- Finance re-underwriting.
- Financial Contract regeneration.
- Deal amendment.
- Deal cancellation and replacement.

### Deal and Inventory Separation

The Deal may request or reference:

- Vehicle eligibility.
- Inventory Reservation.
- Inventory Allocation.
- Inventory release.
- Inventory sale posting.
- Vehicle transfer.
- Delivery readiness.

The Inventory Domain Service or configured authoritative Inventory system owns those outcomes.

The Deal must distinguish:

```text
Reservation requested
  ≠ Vehicle reserved

Allocation requested
  ≠ Vehicle allocated

Sale posting requested
  ≠ Vehicle sold

Delivery preparation
  ≠ Vehicle delivered
```

A Deal must not directly overwrite authoritative Inventory availability.

### Deal and Trade-In Separation

The Trade-In Domain owns:

- Trade-In Vehicle identity resolution.
- Ownership verification.
- Inspection.
- Appraisal.
- Actual cash value.
- Customer allowance.
- Lien and payoff.
- Equity.
- Acquisition.
- Inventory intake.

The Deal preserves the exact accepted Trade-In and appraisal versions used in the transaction.

A Trade-In offer accepted by the Customer does not independently complete:

- Legal ownership transfer.
- Payoff settlement.
- Trade-In acquisition.
- Inventory intake.

### Deal and Finance Application Separation

The Finance Application owns:

- Applicants.
- Finance Consent.
- Verification.
- Credit-bureau activity.
- Lender submissions.
- Lender Decisions.
- Customer-selected finance offer.
- Funding readiness.

The Deal may project the finance status required for transaction execution.

The Deal must not alter a Lender Decision or represent a pending application as approved.

### Deal and Financial Contract Separation

The Financial Contract owns:

- Contract terms.
- Contract versions.
- Disclosures.
- Signatories.
- Signature evidence.
- Contract effectiveness.
- Contract activation.
- Amendments.
- Termination.
- Settlement projections.

The Deal must distinguish:

```text
Contract preparation requested
  ≠ Contract created

Contract created
  ≠ Contract signed

Contract fully signed
  ≠ Contract effective

Contract effective
  ≠ Lender funded
```

### Deal and Payment Separation

A Payment Object or configured Payment authority owns:

- Payment initiation.
- Payment authorization.
- Payment capture.
- Clearing.
- Settlement.
- Refund.
- Reversal.
- Chargeback.
- Reconciliation.

The Deal may maintain authoritative projections and references.

The Deal must distinguish:

```text
Payment required
  ≠ Payment requested

Payment requested
  ≠ Payment authorized

Payment authorized
  ≠ Payment cleared

Payment received projection
  ≠ Payment reconciled
```

Only cleared and applicable funds may satisfy a Deal Payment requirement.

### Deal and Funding Separation

Lender approval does not prove funding.

A funding request does not prove that funds were received.

Funding remains authoritative to:

- Lender.
- Bank.
- F&I platform.
- DMS.
- Accounting platform.
- Another configured funding authority.

The Deal may become funding-ready before funding is confirmed.

### Deal and Delivery Separation

The Deal coordinates delivery readiness.

The authoritative delivery workflow owns:

- Delivery Appointment.
- Vehicle release authorization.
- Customer identity check.
- Handover checklist.
- Key handover.
- Document handover.
- Odometer at handover.
- Customer acceptance.
- Delivery timestamp.
- Delivery evidence.

The Deal must distinguish:

```text
Ready for delivery
  ≠ Delivery scheduled

Delivery scheduled
  ≠ Delivery started

Delivery started
  ≠ Vehicle handed over

Vehicle handed over
  ≠ Delivery authoritatively confirmed

Delivery confirmed
  ≠ Accounting fully reconciled
```

### Deal and Accounting Separation

The configured accounting or DMS authority owns:

- Sale posting.
- Receivable.
- Cost of sale.
- Tax posting.
- Revenue recognition.
- Gross-profit recognition.
- Commission posting.
- Accounting period.
- Reversal.
- Final ledger reconciliation.

The Deal may contain estimated and confirmed projections.

It must not independently create accounting authority.

### Transaction Snapshot

The Deal preserves the exact transaction basis accepted for execution.

The snapshot may include:

- Customer.
- Vehicle.
- Inventory Record.
- Quotation.
- Trade-In.
- Finance.
- Financial Contract.
- Prices.
- Discounts.
- Incentives.
- Taxes.
- Fees.
- Optional products.
- Customer contribution.
- Funding.
- Approvals.
- Delivery conditions.
- Completion requirements.

Once the Deal is approved for execution, material snapshot changes require a controlled amendment or replacement workflow.

### System Purpose

The Deal Object provides canonical transaction context to:

- Customer workflows.
- Opportunity workflows.
- Quotation workflows.
- Vehicle and Inventory workflows.
- Trade-In workflows.
- Finance Application workflows.
- Financial Contract workflows.
- Payment workflows.
- Funding workflows.
- Compliance workflows.
- Registration workflows.
- Insurance workflows.
- Delivery workflows.
- Accounting and DMS integrations.
- Commission workflows.
- AI Agents.
- Analytics.
- Audit and regulatory reporting.

The Deal may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Customer identity | Customer Domain Service |
| Opportunity outcome | Opportunity Domain Service |
| Accepted Quotation | Quotation Domain Service |
| Vehicle identity | Vehicle Domain Service |
| Inventory availability, Reservation, and Allocation | Inventory Domain Service or configured external authority |
| Trade-In appraisal and acquisition | Trade-In Domain Service |
| Lender Decision | Lender through Finance Application |
| Financial Contract and signature | Financial Contract Domain Service and signature authority |
| Customer Payment settlement | Payment or banking authority |
| Lender funding | Lender, bank, or funding authority |
| Insurance verification | Approved insurance authority |
| Registration and title outcome | Government, DMS, or registration authority |
| Delivery outcome | Delivery workflow or configured external authority |
| Accounting and sale posting | Accounting or DMS authority |
| Canonical Deal workflow | Deal Domain Service |
| Deal-level Human Decisions | Authorized Human role |
| Predictions and Recommendations | Derived Intelligence |
| External operation completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `deal_id` — UUIDv4, required and immutable.
- `deal_series_id` — UUIDv4, required for replacement or restructuring lineage.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `department_id`.
- `sales_team_id`.
- `responsible_user_id`.
- `sales_manager_user_id`.
- `finance_manager_user_id`.
- `delivery_coordinator_user_id`.
- `accounting_owner_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `opportunity_id`.
- `customer_id`.
- `accepted_quotation_id`.
- `accepted_quotation_series_id`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_ids`.
- `primary_trade_in_id`.
- `finance_application_id`.
- `selected_lender_decision_id`.
- `financial_contract_id`.
- `appointment_ids`.
- `delivery_appointment_id`.
- `primary_interaction_id`.
- `payment_references`.
- `funding_reference`.
- `delivery_reference`.
- `accounting_reference`.
- `compliance_case_id`.
- `registration_case_id`.
- `insurance_verification_id`.
- `replacement_deal_id`.
- `original_deal_id`.

### Deal Identity and Classification

- `deal_number`.
- `deal_type`.
- `sales_channel`.
- `transaction_type`.
- `status`.
- `priority`.
- `workflow_authority_mode`.
- `approval_status`.
- `execution_status`.
- `completion_status`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.
- `is_primary_deal`.
- `is_replacement_deal`.

### Customer Commitment

- `customer_commitment_status`.
- `customer_commitment_type`.
- `customer_commitment_received_at`.
- `customer_commitment_interaction_id`.
- `customer_commitment_document_reference`.
- `customer_commitment_document_hash`.
- `customer_commitment_evidence_references`.
- `customer_commitment_expires_at`.
- `customer_commitment_revalidation_required`.

### Accepted Quotation Context

- `accepted_quotation_id`.
- `accepted_quotation_series_id`.
- `accepted_quotation_version`.
- `accepted_quotation_document_hash`.
- `accepted_quotation_snapshot`.
- `accepted_quotation_snapshot_hash`.
- `quotation_acceptance_status`.
- `quotation_accepted_at`.
- `quotation_acceptance_evidence_references`.
- `quotation_validity_confirmed_at`.

### Customer Snapshot

- `customer_snapshot`.
- `customer_snapshot_hash`.
- `customer_identity_status`.
- `customer_identity_confirmed_at`.
- `customer_contact_restriction_projection`.
- `customer_language_projection`.
- `customer_tax_profile_projection`.
- `customer_legal_name_projection`.

The snapshot preserves transaction context and does not replace Customer authority.

### Vehicle and Inventory Context

- `vehicle_id`.
- `inventory_record_id`.
- `vehicle_snapshot`.
- `vehicle_snapshot_hash`.
- `inventory_snapshot`.
- `inventory_snapshot_hash`.
- `vehicle_condition`.
- `vehicle_eligibility_status`.
- `inventory_eligibility_status`.
- `inventory_availability_status`.
- `inventory_availability_confirmed_at`.
- `inventory_availability_expires_at`.
- `vehicle_location_projection`.
- `vehicle_readiness_projection`.

### Reservation

- `reservation_required`.
- `reservation_status`.
- `reservation_request_id`.
- `reservation_command_id`.
- `reservation_idempotency_key`.
- `reservation_reference`.
- `reservation_requested_at`.
- `reservation_confirmed_at`.
- `reservation_expires_at`.
- `reservation_confirmation_status`.
- `reservation_release_status`.
- `reservation_release_reference`.
- `reservation_reconciliation_status`.

### Allocation

- `allocation_required`.
- `allocation_status`.
- `allocation_request_id`.
- `allocation_command_id`.
- `allocation_idempotency_key`.
- `allocation_reference`.
- `allocation_requested_at`.
- `allocation_confirmed_at`.
- `allocation_confirmation_status`.
- `allocation_release_status`.
- `allocation_release_reference`.
- `allocation_reconciliation_status`.

### Commercial Snapshot

- `currency_code`.
- `vehicle_list_price_amount`.
- `vehicle_selling_price_amount`.
- `vehicle_discount_amount`.
- `manufacturer_rebate_amount`.
- `dealer_incentive_amount`.
- `campaign_incentive_amount`.
- `optional_products_total_amount`.
- `service_products_total_amount`.
- `warranty_products_total_amount`.
- `insurance_products_total_amount`.
- `accessory_total_amount`.
- `subtotal_amount`.
- `total_discount_amount`.
- `total_rebate_amount`.
- `total_incentive_amount`.
- `total_fee_amount`.
- `total_tax_amount`.
- `total_transaction_amount`.
- `customer_cash_due_amount`.
- `commercial_snapshot`.
- `commercial_snapshot_hash`.
- `calculation_snapshot`.
- `calculation_snapshot_hash`.

### Trade-In Snapshot

- `trade_in_ids`.
- `primary_trade_in_id`.
- `trade_in_snapshot`.
- `trade_in_snapshot_hash`.
- `trade_in_appraisal_ids`.
- `trade_in_appraisal_versions`.
- `trade_in_offer_versions`.
- `trade_in_actual_cash_value_amount`.
- `trade_in_allowance_amount`.
- `trade_in_over_allowance_amount`.
- `trade_in_payoff_amount`.
- `trade_in_positive_equity_amount`.
- `trade_in_negative_equity_amount`.
- `trade_in_acquisition_status`.
- `trade_in_payoff_status`.
- `trade_in_ownership_transfer_status`.

### Finance Snapshot

- `finance_required`.
- `finance_application_id`.
- `finance_application_version`.
- `selected_lender_submission_id`.
- `selected_lender_decision_id`.
- `selected_lender_decision_version`.
- `selected_lender_id`.
- `selected_finance_program_id`.
- `finance_decision_status`.
- `finance_decision_valid_until`.
- `finance_principal_amount`.
- `approved_down_payment_amount`.
- `approved_interest_rate`.
- `approved_annual_percentage_rate`.
- `approved_term_months`.
- `approved_payment_frequency`.
- `approved_periodic_payment_amount`.
- `approved_balloon_payment_amount`.
- `finance_conditions_status`.
- `finance_snapshot`.
- `finance_snapshot_hash`.

### Financial Contract Projection

- `financial_contract_required`.
- `financial_contract_id`.
- `financial_contract_version`.
- `financial_contract_status`.
- `contract_signature_status`.
- `contract_effectiveness_status`.
- `contract_activation_status`.
- `contract_signed_at`.
- `contract_effective_at`.
- `contract_activated_at`.
- `signed_contract_document_hash`.
- `contract_reconciliation_status`.

### Payment Requirements

- `payment_requirement_ids`.
- `deposit_required`.
- `deposit_required_amount`.
- `down_payment_required_amount`.
- `additional_customer_payment_required_amount`.
- `total_customer_payment_required_amount`.
- `refundable_deposit_amount`.
- `non_refundable_amount`.
- `payment_currency_code`.
- `payment_deadline_at`.
- `payment_policy_id`.
- `payment_policy_version`.

### Payment Projections

- `payment_status`.
- `deposit_status`.
- `down_payment_status`.
- `customer_payment_status`.
- `total_payment_authorized_amount`.
- `total_payment_captured_amount`.
- `total_payment_cleared_amount`.
- `total_payment_refunded_amount`.
- `total_payment_reversed_amount`.
- `total_payment_chargeback_amount`.
- `net_customer_payment_amount`.
- `payment_shortfall_amount`.
- `last_payment_confirmed_at`.
- `payment_reconciliation_status`.
- `payment_references`.

### Funding Requirements and Projection

- `funding_required`.
- `funding_readiness_status`.
- `funding_status`.
- `funding_amount_required`.
- `funding_amount_requested`.
- `funded_amount`.
- `funding_currency_code`.
- `funding_shortfall_amount`.
- `funding_requested_at`.
- `funding_received_at`.
- `funding_reference`.
- `funding_command_id`.
- `funding_idempotency_key`.
- `funding_confirmation_status`.
- `funding_confirmation_reference`.
- `funding_reversal_status`.
- `funding_reconciliation_status`.

### Approval Context

- `approval_required`.
- `approval_status`.
- `approval_reason_codes`.
- `approval_policy_id`.
- `approval_policy_version`.
- `approval_request_ids`.
- `approval_decision_ids`.
- `approval_requested_at`.
- `approved_at`.
- `approved_by_actor_ids`.
- `approval_expires_at`.
- `approval_evidence_references`.
- `approval_revocation_status`.
- `approval_revalidation_required`.

### Compliance Context

- `identity_verification_status`.
- `sanctions_review_status`.
- `anti_money_laundering_review_status`.
- `source_of_funds_review_status`.
- `fraud_review_status`.
- `customer_eligibility_status`.
- `vehicle_compliance_status`.
- `deal_compliance_status`.
- `compliance_case_id`.
- `compliance_block_reasons`.
- `compliance_evidence_references`.
- `compliance_confirmed_at`.

### Document Context

- `document_requirement_set_id`.
- `required_document_types`.
- `received_document_types`.
- `verified_document_types`.
- `missing_document_types`.
- `rejected_document_types`.
- `expired_document_types`.
- `document_completion_status`.
- `document_verification_status`.
- `document_package_reference`.
- `document_package_hash`.
- `document_reconciliation_status`.

### Registration and Title Context

- `registration_required`.
- `registration_status`.
- `registration_authority_reference`.
- `registration_request_id`.
- `registration_command_id`.
- `registration_idempotency_key`.
- `registration_requested_at`.
- `registration_completed_at`.
- `registration_confirmation_status`.
- `registration_reference`.
- `title_status`.
- `title_transfer_status`.
- `title_reference`.
- `title_confirmation_status`.
- `registration_reconciliation_status`.

### Insurance Context

- `insurance_required`.
- `insurance_verification_status`.
- `insurance_policy_reference`.
- `insurance_provider_reference`.
- `insurance_effective_at`.
- `insurance_expires_at`.
- `insurance_confirmation_status`.
- `insurance_evidence_references`.

### Delivery Readiness

- `delivery_readiness_status`.
- `delivery_requirement_ids`.
- `outstanding_delivery_requirements`.
- `vehicle_readiness_status`.
- `payment_readiness_status`.
- `funding_readiness_projection`.
- `contract_readiness_status`.
- `document_readiness_status`.
- `registration_readiness_status`.
- `insurance_readiness_status`.
- `trade_in_readiness_status`.
- `customer_identity_readiness_status`.
- `delivery_readiness_evaluated_at`.
- `delivery_readiness_snapshot`.
- `delivery_readiness_snapshot_hash`.

### Delivery Projection

- `delivery_status`.
- `delivery_reference`.
- `delivery_appointment_id`.
- `delivery_scheduled_at`.
- `delivery_started_at`.
- `vehicle_handover_at`.
- `delivery_confirmed_at`.
- `delivery_confirmation_status`.
- `delivery_evidence_references`.
- `delivery_reconciliation_status`.

### Sale and Accounting Projection

- `sale_posting_status`.
- `sale_posting_reference`.
- `sale_posting_requested_at`.
- `sale_posting_confirmed_at`.
- `sale_confirmation_status`.
- `accounting_handoff_status`.
- `accounting_reference`.
- `accounting_confirmation_status`.
- `accounting_period_reference`.
- `revenue_recognition_status`.
- `cost_of_sale_status`.
- `tax_posting_status`.
- `accounting_reconciliation_status`.

### Profitability Projection

- `vehicle_cost_amount`.
- `reconditioning_cost_amount`.
- `accessory_cost_amount`.
- `optional_product_cost_amount`.
- `funding_cost_amount`.
- `trade_in_cost_impact_amount`.
- `front_end_gross_amount`.
- `back_end_gross_amount`.
- `estimated_gross_profit_amount`.
- `estimated_gross_margin_percentage`.
- `confirmed_gross_profit_amount`.
- `confirmed_gross_margin_percentage`.
- `commissionable_gross_amount`.
- `profitability_status`.
- `profitability_source`.
- `profitability_confirmed_at`.

Estimated and confirmed profitability must remain distinguishable.

### Commission Projection

- `commission_calculation_status`.
- `commission_plan_id`.
- `commission_plan_version`.
- `commissionable_amount`.
- `commission_amount_projection`.
- `commission_reference`.
- `commission_confirmation_status`.
- `commission_reconciliation_status`.

The Deal must not independently authorize payroll.

### Completion Context

- `completion_requirement_set_id`.
- `completion_requirement_ids`.
- `completion_status`.
- `outstanding_completion_requirements`.
- `completion_evaluated_at`.
- `completion_decision_id`.
- `completed_at`.
- `completion_confirmation_status`.
- `completion_evidence_references`.

### Cancellation

- `cancellation_status`.
- `cancellation_requested_at`.
- `cancellation_reason`.
- `cancellation_details`.
- `cancellation_decision_id`.
- `cancelled_at`.
- `cancelled_by_actor_type`.
- `cancelled_by_actor_id`.
- `cancellation_evidence_references`.
- `cancellation_external_confirmation_status`.
- `cancellation_reconciliation_status`.

### Unwind

- `unwind_status`.
- `unwind_requested_at`.
- `unwind_reason`.
- `unwind_details`.
- `unwind_decision_id`.
- `unwind_plan_reference`.
- `unwind_started_at`.
- `unwound_at`.
- `unwind_evidence_references`.
- `unwind_external_confirmation_status`.
- `unwind_reconciliation_status`.

### Replacement Deal

- `original_deal_id`.
- `replacement_deal_id`.
- `replacement_reason`.
- `replacement_status`.
- `replacement_created_at`.
- `replacement_confirmation_status`.
- `replacement_lineage_snapshot`.

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

- `deal_completion_probability`.
- `delivery_delay_risk_score`.
- `funding_delay_risk_score`.
- `payment_shortfall_risk_score`.
- `cancellation_risk_score`.
- `margin_risk_score`.
- `compliance_delay_risk_score`.
- `document_completion_prediction`.
- `recommended_next_action`.
- `recommended_escalation`.
- `recommended_delivery_date`.
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

- `deal_age_days`.
- `days_since_deal_creation`.
- `days_to_approval`.
- `days_to_reservation`.
- `days_to_contract`.
- `days_to_funding`.
- `days_to_delivery`.
- `days_to_completion`.
- `document_completion_percentage`.
- `delivery_readiness_percentage`.
- `completion_requirement_percentage`.
- `payment_shortfall_amount`.
- `funding_shortfall_amount`.
- `estimated_total_receipts_amount`.
- `confirmed_total_receipts_amount`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `external_deal_id`.
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
- `approved_at`.
- `execution_started_at`.
- `completed_at`.
- `cancelled_at`.
- `unwound_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `deal_id` | UUID | Yes | ASOS | Immutable Canonical Deal identifier. |
| `deal_series_id` | UUID | Yes | ASOS | Stable lineage identifier for original and replacement Deals. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `deal_number` | String | Yes | ASOS or configured external authority | Human-readable transaction reference. |
| `opportunity_id` | UUID | Yes | Canonical relationship | Opportunity that produced the Deal. |
| `customer_id` | UUID | Yes | Canonical relationship | Customer participating in the transaction. |
| `accepted_quotation_id` | UUID | Conditional | Canonical relationship | Exact accepted Quotation used as the commercial basis. |
| `vehicle_id` | UUID | Yes | Canonical relationship | Vehicle identity included in the transaction. |
| `inventory_record_id` | UUID | Conditional | Inventory relationship | Physical Inventory Record where applicable. |
| `deal_type` | Enum | Yes | Workflow State | Commercial transaction structure. |
| `sales_channel` | Enum | Yes | Workflow State | Channel through which the transaction originated. |
| `status` | Enum | Yes | Deal workflow | Current aggregate transaction lifecycle state. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Defines ASOS or external authority over Deal workflow state. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Accepted Quotation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `accepted_quotation_version` | Integer | Conditional | Quotation | Exact accepted Quotation version. |
| `accepted_quotation_document_hash` | String | Conditional | Quotation | Hash of the issued and accepted document. |
| `quotation_accepted_at` | Timestamp | Conditional | Customer evidence | Time valid acceptance occurred. |
| `accepted_quotation_snapshot_hash` | String | Conditional | ASOS | Integrity hash of the accepted commercial snapshot. |
| `quotation_acceptance_evidence_references` | Array | Conditional | Evidence repository | Evidence supporting Customer acceptance. |

### Commercial Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `currency_code` | String | Yes | Quotation | ISO 4217 transaction currency. |
| `vehicle_selling_price_amount` | Decimal | Yes | Accepted Quotation | Approved Vehicle selling price. |
| `total_discount_amount` | Decimal | Yes | Accepted Quotation | Approved discount total. |
| `total_rebate_amount` | Decimal | Yes | Accepted Quotation | Approved rebate total. |
| `total_incentive_amount` | Decimal | Yes | Accepted Quotation | Approved incentive total. |
| `total_fee_amount` | Decimal | Yes | Accepted Quotation | Approved disclosed fee total. |
| `total_tax_amount` | Decimal | Yes | Accepted Quotation | Approved tax total. |
| `total_transaction_amount` | Decimal | Yes | Deterministic calculation | Total transaction value. |
| `customer_cash_due_amount` | Decimal | Yes | Deterministic calculation | Total applicable Customer cash requirement. |
| `commercial_snapshot_hash` | String | Yes | ASOS | Integrity hash of the Deal commercial snapshot. |

### Inventory Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inventory_eligibility_status` | Enum | Yes | Inventory Projection | Whether the physical Inventory Record remains transaction-eligible. |
| `inventory_availability_status` | Enum | Yes | Inventory Projection | Current authoritative availability projection. |
| `reservation_status` | Enum | Yes | Inventory Projection | Current commercial Reservation state. |
| `allocation_status` | Enum | Yes | Inventory Projection | Current Deal Allocation state. |
| `reservation_confirmation_status` | Enum | Yes | Workflow Projection | External Reservation Confirmation state. |
| `allocation_confirmation_status` | Enum | Yes | Workflow Projection | External Allocation Confirmation state. |

### Trade-In Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `primary_trade_in_id` | UUID | No | Canonical relationship | Primary Trade-In supporting the transaction. |
| `trade_in_appraisal_version` | Integer | No | Trade-In | Exact appraisal version used. |
| `trade_in_allowance_amount` | Decimal | Yes | Quotation and Trade-In projection | Customer-facing Trade-In allowance. |
| `trade_in_payoff_amount` | Decimal | Yes | Trade-In and payoff authority | Verified payoff included in the transaction. |
| `trade_in_positive_equity_amount` | Decimal | Yes | Deterministic calculation | Positive Trade-In equity. |
| `trade_in_negative_equity_amount` | Decimal | Yes | Deterministic calculation | Negative Trade-In equity. |
| `trade_in_acquisition_status` | Enum | Yes | Trade-In Projection | Current authoritative acquisition projection. |

### Finance and Contract Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_required` | Boolean | Yes | Deal structure | Whether Lender finance is required. |
| `finance_application_id` | UUID | Conditional | Canonical relationship | Finance Application supporting the Deal. |
| `selected_lender_decision_id` | UUID | Conditional | Finance Application relationship | Exact selected Lender Decision. |
| `finance_decision_status` | Enum | Yes | Lender Projection | Current authoritative Decision projection. |
| `financial_contract_id` | UUID | Conditional | Canonical relationship | Financial Contract supporting the transaction. |
| `contract_signature_status` | Enum | Yes | Financial Contract Projection | Current signature state. |
| `contract_effectiveness_status` | Enum | Yes | Financial Contract Projection | Current legal effectiveness state. |
| `contract_activation_status` | Enum | Yes | Financial Contract Projection | Current activation state. |

### Payment Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `deposit_required_amount` | Decimal | Yes | Deal requirement | Required deposit. |
| `down_payment_required_amount` | Decimal | Yes | Deal and Lender terms | Required Customer down payment. |
| `total_customer_payment_required_amount` | Decimal | Yes | Deterministic calculation | Total required Customer funds. |
| `total_payment_authorized_amount` | Decimal | Yes | Payment Projection | Total authorized amount. |
| `total_payment_captured_amount` | Decimal | Yes | Payment Projection | Total captured amount. |
| `total_payment_cleared_amount` | Decimal | Yes | Payment authority | Total cleared amount applicable to the Deal. |
| `net_customer_payment_amount` | Decimal | Yes | Deterministic projection | Cleared amount less applicable refunds and reversals. |
| `payment_shortfall_amount` | Decimal | Yes | Deterministic calculation | Remaining Customer Payment requirement. |
| `payment_status` | Enum | Yes | Payment Projection | Aggregate Payment state. |

### Funding Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `funding_required` | Boolean | Yes | Deal structure | Whether external funding is required. |
| `funding_readiness_status` | Enum | Yes | Deterministic workflow | Readiness to request funding. |
| `funding_status` | Enum | Yes | Funding Projection | Current authoritative funding state. |
| `funding_amount_required` | Decimal | Yes | Financial Contract and Deal | Required external funding amount. |
| `funded_amount` | Decimal | Yes | Funding authority | Authoritatively confirmed funded amount. |
| `funding_shortfall_amount` | Decimal | Yes | Deterministic calculation | Difference between required and confirmed funding. |
| `funding_confirmation_status` | Enum | Yes | Workflow Projection | External funding Confirmation state. |

### Delivery Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `delivery_readiness_status` | Enum | Yes | Deterministic workflow | Aggregate readiness for Vehicle handover. |
| `delivery_status` | Enum | Yes | Delivery Projection | Current authoritative delivery state. |
| `delivery_appointment_id` | UUID | No | Appointment relationship | Appointment scheduled for handover. |
| `vehicle_handover_at` | Timestamp | No | Delivery evidence | Time physical handover was recorded. |
| `delivery_confirmed_at` | Timestamp | No | Delivery authority | Time authoritative delivery Confirmation occurred. |
| `delivery_confirmation_status` | Enum | Yes | Workflow Projection | Current External Confirmation state. |

### Accounting Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `sale_posting_status` | Enum | Yes | DMS or accounting projection | Current authoritative sale-posting state. |
| `accounting_handoff_status` | Enum | Yes | Accounting workflow | Current accounting handoff state. |
| `revenue_recognition_status` | Enum | Yes | Accounting authority | Current revenue-recognition projection. |
| `confirmed_gross_profit_amount` | Decimal | No | Accounting authority | Confirmed transaction gross profit. |
| `accounting_reconciliation_status` | Enum | Yes | Reconciliation workflow | Current accounting reconciliation state. |

---

## 4. Enumerations

### DealStatus

- `CREATED`
- `VALIDATION_PENDING`
- `APPROVAL_PENDING`
- `APPROVED`
- `EXECUTION_IN_PROGRESS`
- `READY_FOR_DELIVERY`
- `DELIVERY_PENDING`
- `COMPLETION_PENDING`
- `COMPLETED`
- `CANCELLATION_PENDING`
- `CANCELLED`
- `UNWIND_PENDING`
- `UNWOUND`
- `DISPUTED`
- `SUPERSEDED`
- `ARCHIVED`

### DealType

- `RETAIL_CASH`
- `RETAIL_FINANCE`
- `RETAIL_LEASE`
- `FLEET_CASH`
- `FLEET_FINANCE`
- `FLEET_LEASE`
- `FACTORY_ORDER`
- `WHOLESALE`
- `DIRECT_SALE`
- `REPLACEMENT_TRANSACTION`
- `OTHER`

### DealTransactionType

- `NEW_VEHICLE`
- `USED_VEHICLE`
- `DEMO_VEHICLE`
- `FACTORY_ORDER`
- `FLEET_TRANSACTION`
- `WHOLESALE_TRANSACTION`
- `OTHER`

### DealSalesChannel

- `SHOWROOM`
- `DIGITAL`
- `PHONE_ASSISTED`
- `BDC`
- `FLEET`
- `OEM_REFERRAL`
- `MARKETPLACE`
- `PARTNER_REFERRAL`
- `OTHER`

### DealPriority

- `STANDARD`
- `HIGH`
- `URGENT`
- `STRATEGIC`

Priority must not override legal, safety, Consent, approval, Payment, funding, or delivery controls.

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_DMS_AUTHORITATIVE`
- `EXTERNAL_CRM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### CustomerCommitmentStatus

- `NOT_RECEIVED`
- `PENDING_VALIDATION`
- `VALIDATED`
- `EXPIRED`
- `WITHDRAWN`
- `DISPUTED`
- `REVALIDATION_REQUIRED`

### CustomerCommitmentType

- `ACCEPTED_QUOTATION`
- `SIGNED_ORDER_FORM`
- `VALID_DEPOSIT_COMMITMENT`
- `APPROVED_DIGITAL_ACCEPTANCE`
- `APPROVED_PHYSICAL_ACCEPTANCE`
- `OTHER_GOVERNED_COMMITMENT`

### DealApprovalStatus

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

### DealExecutionStatus

- `NOT_STARTED`
- `PREPARING`
- `IN_PROGRESS`
- `BLOCKED`
- `PARTIALLY_COMPLETE`
- `COMPLETE`
- `FAILED`
- `RECONCILIATION_REQUIRED`

### InventoryEligibilityStatus

- `NOT_EVALUATED`
- `ELIGIBLE`
- `CONDITIONALLY_ELIGIBLE`
- `INELIGIBLE`
- `STALE`
- `CONFLICTED`
- `REVIEW_REQUIRED`

### DealReservationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `RESERVED`
- `EXPIRED`
- `RELEASE_PENDING`
- `RELEASED`
- `REJECTED`
- `FAILED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### DealAllocationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `ALLOCATED`
- `RELEASE_PENDING`
- `RELEASED`
- `REJECTED`
- `FAILED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### DealPaymentStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PAYMENT_REQUIRED`
- `AUTHORIZATION_PENDING`
- `PARTIALLY_AUTHORIZED`
- `AUTHORIZED`
- `CAPTURE_PENDING`
- `PARTIALLY_CAPTURED`
- `CAPTURED`
- `CLEARING_PENDING`
- `PARTIALLY_CLEARED`
- `CLEARED`
- `SHORTFALL`
- `REFUND_PENDING`
- `PARTIALLY_REFUNDED`
- `REFUNDED`
- `REVERSED`
- `CHARGEBACK`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DealFundingReadinessStatus

- `NOT_REQUIRED`
- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### DealFundingStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `REQUIREMENTS_PENDING`
- `REQUEST_READY`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `PARTIALLY_FUNDED`
- `FUNDED`
- `SHORTFALL`
- `FAILED`
- `REVERSED`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### ContractProjectionStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PREPARATION_PENDING`
- `CREATED`
- `SIGNATURE_PENDING`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `EFFECTIVENESS_PENDING`
- `EFFECTIVE`
- `ACTIVE`
- `VOIDED`
- `TERMINATED`
- `EXPIRED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DocumentCompletionStatus

- `NOT_STARTED`
- `INCOMPLETE`
- `PARTIALLY_COMPLETE`
- `COMPLETE`
- `REJECTED`
- `EXPIRED`
- `REVALIDATION_REQUIRED`

### DealComplianceStatus

- `NOT_ASSESSED`
- `PENDING`
- `PARTIALLY_COMPLETE`
- `CLEARED`
- `CLEARED_WITH_CONDITIONS`
- `BLOCKED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### RegistrationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `DOCUMENTS_PENDING`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `REGISTERED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### TitleTransferStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `PENDING_CONFIRMATION`
- `TRANSFERRED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### InsuranceStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `VERIFIED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### DeliveryReadinessStatus

- `NOT_ASSESSED`
- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### DealDeliveryStatus

- `NOT_STARTED`
- `PREPARATION_PENDING`
- `READY`
- `SCHEDULING`
- `SCHEDULED`
- `DELIVERY_PENDING`
- `HANDOVER_IN_PROGRESS`
- `HANDOVER_RECORDED`
- `PENDING_CONFIRMATION`
- `DELIVERED`
- `FAILED`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### SalePostingStatus

- `NOT_STARTED`
- `READY`
- `REQUEST_PENDING`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `POSTED`
- `REJECTED`
- `FAILED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### AccountingHandoffStatus

- `NOT_STARTED`
- `PREPARATION_PENDING`
- `READY`
- `PENDING`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `FAILED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### RevenueRecognitionStatus

- `NOT_EVALUATED`
- `NOT_ELIGIBLE`
- `ELIGIBLE`
- `PENDING`
- `RECOGNIZED`
- `PARTIALLY_RECOGNIZED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DealCompletionStatus

- `NOT_EVALUATED`
- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `BLOCKED`
- `FAILED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DealCancellationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `VALIDATION_PENDING`
- `APPROVAL_PENDING`
- `APPROVED`
- `PENDING_EXTERNAL_CONFIRMATION`
- `CANCELLED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DealCancellationReason

- `CUSTOMER_REQUEST`
- `CUSTOMER_PAYMENT_FAILED`
- `FINANCE_DECLINED`
- `FINANCE_EXPIRED`
- `CONTRACT_NOT_SIGNED`
- `VEHICLE_UNAVAILABLE`
- `VEHICLE_INELIGIBLE`
- `TRADE_IN_FAILED`
- `COMPLIANCE_BLOCK`
- `DOCUMENTS_INCOMPLETE`
- `COMMERCIAL_TERMS_CHANGED`
- `DUPLICATE_DEAL`
- `DEALERSHIP_DECLINED`
- `OTHER`

### DealUnwindStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUESTED`
- `IMPACT_ASSESSMENT_PENDING`
- `APPROVAL_PENDING`
- `APPROVED`
- `IN_PROGRESS`
- `PENDING_EXTERNAL_CONFIRMATION`
- `PARTIALLY_UNWOUND`
- `UNWOUND`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DealUnwindReason

- `POST_COMPLETION_CANCELLATION`
- `CONTRACT_VOIDED`
- `FUNDING_REVERSED`
- `PAYMENT_REVERSED`
- `VEHICLE_RETURNED`
- `DELIVERY_REVERSED`
- `LEGAL_INVALIDITY`
- `FRAUD_CONFIRMED`
- `ACCOUNTING_CORRECTION`
- `DUPLICATE_TRANSACTION`
- `CUSTOMER_RESCISSION`
- `OTHER`

### DealDisputeStatus

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

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

### ConfirmationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ReconciliationStatus

- `NOT_REQUIRED`
- `CURRENT`
- `PENDING`
- `IN_PROGRESS`
- `RESOLVED`
- `FAILED`
- `MANUAL_REVIEW_REQUIRED`

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

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- All related Domain Objects must belong to the authorized Tenant.
- Dealership, branch, legal entity, team, User, approver, and operational authority must belong to the permitted organizational scope.
- Cross-Tenant Deal access, matching, processing, AI retrieval, export, or synchronization is prohibited unless governed by an approved and auditable mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Deal Creation Rules

A Deal may be created only when:

- A valid Customer exists.
- A valid Opportunity exists.
- The Opportunity is eligible for Deal conversion.
- Customer and Opportunity are consistent.
- A valid accepted Quotation or approved alternative commercial basis exists.
- Customer commitment evidence exists.
- Vehicle identity exists.
- Physical Inventory is identified where required.
- Required commercial approvals are valid.
- Required Customer identity checks pass.
- No blocking legal, compliance, Vehicle, Inventory, Quotation, or authority conflict exists.
- Creation is idempotent.
- No duplicate primary active Deal already exists.

Deal creation must preserve:

- Opportunity version.
- Customer relationship.
- Quotation version and hash.
- Customer acceptance evidence.
- Vehicle and Inventory references.
- Commercial snapshot.
- Calculation snapshot.
- Approval evidence.
- Source-record versions.
- Creation authority.

### Primary Deal and Duplicate Rules

One Opportunity may normally have only one primary active Deal.

Additional Deals require a governed reason such as:

- Approved replacement.
- Restructuring.
- Separate Vehicle transaction.
- Split commercial transaction.
- Correction.
- Another configured legitimate scenario.

Duplicate detection must consider:

- Tenant.
- Opportunity.
- Customer.
- Accepted Quotation.
- Vehicle.
- Inventory Record.
- Active Deal status.
- External DMS reference.
- Deal series.
- Timeframe.

An ambiguous duplicate must not be merged or cancelled solely by AI.

### Accepted Quotation Rules

- The Deal must reference the exact accepted Quotation version.
- The accepted Quotation must be issued, current at acceptance, and valid under the accepted policy.
- The accepted document hash must match.
- Customer acceptance evidence must satisfy the configured policy.
- Commercial values must match the accepted Quotation.
- Quotation acceptance must not be inferred from view, sentiment, or AI prediction.
- An expired, withdrawn, rejected, or superseded Quotation must not create a new Deal.
- A material commercial change requires a new Quotation version and Deal revalidation.

### Customer Commitment Rules

Customer commitment must be supported by an approved evidence type.

Possible evidence may include:

- Valid accepted Quotation.
- Approved order form.
- Approved digital acceptance.
- Approved physical acceptance.
- Valid deposit commitment combined with applicable acceptance.
- Another governed commitment.

Customer commitment does not prove:

- Payment clearing.
- Contract signature.
- Finance approval.
- Vehicle delivery.

### Vehicle and Inventory Rules

A physical Vehicle Deal requires:

- Valid `vehicle_id`.
- Valid `inventory_record_id`.
- Matching Vehicle and Inventory relationship.
- Current Inventory eligibility.
- Current Inventory availability.
- No incompatible legal, safety, quality, Reservation, Allocation, sale, transfer, or delivery block.
- Current Vehicle location.
- Current Vehicle readiness where applicable.

A factory-order Deal may omit a physical Inventory Record only under an approved factory-order model.

Vehicle or Inventory changes after Deal approval require:

- Revalidation.
- New commercial snapshot.
- Quotation review.
- Finance review.
- Contract review.
- Customer reconfirmation where material.

### Reservation Rules

A Reservation request requires:

- Eligible Deal.
- Eligible Inventory Record.
- Applicable Customer commitment.
- Valid Reservation policy.
- Required deposit or approval where applicable.
- Idempotency key.
- Current Inventory version.
- No incompatible existing Reservation or Allocation.

The Deal must remain pending until authoritative Reservation Confirmation is received where Inventory authority is external.

A Reservation does not prove Allocation, sale, or delivery.

Reservation expiration or release must trigger Deal review.

### Allocation Rules

Allocation requires:

- Eligible Deal.
- Eligible Inventory Record.
- Valid Reservation where required.
- Applicable commercial, finance, Payment, or contract conditions.
- Allocation authority.
- Concurrency-safe operation.
- Idempotency key.
- External Confirmation where applicable.

The Deal must not represent a requested Allocation as confirmed.

Allocation does not prove sale or delivery.

### Commercial Calculation Rules

All authoritative Deal monetary calculations must be deterministic.

AI must not calculate authoritative totals.

The calculation service must validate:

- Vehicle selling price.
- Optional products.
- Taxes.
- Fees.
- Discounts.
- Rebates.
- Incentives.
- Trade-In allowance.
- Trade-In payoff.
- Equity.
- Customer contribution.
- Finance amount.
- Total transaction amount.
- Payment shortfall.
- Funding shortfall.
- Estimated profitability.

Every calculation must preserve:

- Formula.
- Rule version.
- Input-record versions.
- Currency.
- Rounding.
- Tax jurisdiction.
- Output.
- Timestamp.
- Integrity hash.

Calculation mismatch must block approval, execution, delivery, and completion.

### Commercial Snapshot Rules

- The approved Deal commercial snapshot must be immutable during execution.
- Material changes require an amendment or replacement process.
- Customer-visible values must match the accepted Quotation and applicable contracts.
- Hidden charges are prohibited.
- Internal costs and margins must not appear in Customer-facing documents.
- Expired incentives or invalid discounts require revalidation.
- Currency conversion requires approved source, timestamp, rate, and disclosure.

### Trade-In Rules

When a Trade-In is included:

- Exact Trade-In record must be referenced.
- Exact appraisal and offer versions must be preserved.
- Trade-In acceptance must be valid.
- Ownership and payoff evidence must be sufficiently current.
- Equity must be calculated deterministically.
- Trade-In acquisition readiness must be evaluated.
- Trade-In changes may require a new Quotation and Deal revalidation.
- Deal completion must not falsely imply Trade-In acquisition if acquisition remains pending.

### Finance Rules

A financed Deal requires:

- Valid Finance Application.
- Exact immutable Finance Application version.
- Valid selected Lender Decision.
- Valid Customer selection.
- Current Lender Decision.
- Current approved finance terms.
- Satisfied or permitted outstanding conditions.
- Applicable Financial Contract.
- Funding readiness.

The Deal must not:

- Alter Lender terms.
- Mark finance approved from an AI score.
- Treat prequalification as approval.
- Treat contract preparation as signature.
- Treat approval as funding.

### Financial Contract Rules

When a Financial Contract is required:

- The Deal must reference the correct Contract.
- Contract Customer, Deal, Vehicle, Quotation, and finance terms must match.
- Required signatures must be authoritative.
- Contract effectiveness must be evaluated separately.
- Contract activation must remain separately projected.
- Contract mismatch must block delivery and completion.
- A fully signed Contract must not be treated as effective unless effectiveness is confirmed.

### Payment Rules

Only authoritative Payment sources may confirm:

- Authorization.
- Capture.
- Clearing.
- Refund.
- Reversal.
- Chargeback.
- Settlement.

The Deal must not count:

- Failed Payment.
- Expired authorization.
- Uncaptured authorization.
- Pending bank transfer.
- Reversed Payment.
- Refunded Payment.
- Disputed funds.

as cleared available funds.

Customer Payment readiness requires the configured cleared amount.

Duplicate Payment events must not create duplicate financial effects.

### Funding Rules

A financed Deal must not become funded until:

- Authoritative funding Confirmation exists.
- Confirmed amount is known.
- Currency is known.
- Funding reference is known.
- Funding timestamp is known.
- Funding is reconciled to the Contract and Deal.
- Material shortfall is resolved.

A partial funding outcome must remain explicit.

A funding reversal must trigger:

- Deal block.
- Delivery review.
- Accounting review.
- Contract review.
- Human escalation.
- Reconciliation.

### Approval Rules

Deal approval may be required for:

- Discount exception.
- Margin exception.
- Pricing override.
- Trade-In over-allowance.
- Negative-equity treatment.
- Payment exception.
- Funding exception.
- Reservation exception.
- Allocation exception.
- Compliance exception.
- Document exception.
- Delivery exception.
- Another material commercial risk.

Approval must preserve:

- Requested scope.
- Snapshot.
- Values.
- Applied policy.
- Required approvers.
- Decisions.
- Reasons.
- Evidence.
- Effective period.
- Revocation.

AI must not approve a Deal or exception.

### Compliance Rules

Applicable Customer, Vehicle, transaction, fraud, sanctions, source-of-funds, and legal checks must complete before prohibited progression.

A compliance block overrides:

- Sales priority.
- Customer urgency.
- Management preference.
- AI Recommendation.
- Delivery schedule.

AI may recommend review but must not clear a compliance block.

### Document Rules

- Required documents must be determined by current policy.
- Document completion and document verification must remain separate.
- Raw documents must remain in controlled storage.
- Missing, expired, rejected, or disputed documents must remain explicit.
- Document completion does not prove authenticity.
- Document authenticity does not prove Payment, funding, registration, or delivery.

### Registration and Title Rules

- Required registration and title workflows must be identified.
- Registration requests must be idempotent.
- A sent registration Command does not prove registration.
- External Confirmation must be preserved.
- Material registration failure must block delivery where required.
- Title and registration state must not be invented from document submission.
- AI must not confirm registration or title transfer.

### Insurance Rules

Where insurance is required:

- Applicable coverage must match the Customer, Vehicle, and Deal.
- Coverage must be effective.
- Coverage must satisfy required conditions.
- Provider and policy references must be verified.
- Expired or cancelled coverage must block dependent delivery.
- AI extraction does not create authoritative insurance verification.

### Delivery Readiness Rules

Delivery readiness must be calculated deterministically from configured requirements.

Possible requirements include:

- Vehicle allocated.
- Vehicle physically ready.
- Required Payment cleared.
- Required funding confirmed.
- Financial Contract effective.
- Required documents complete.
- Required compliance cleared.
- Registration ready.
- Insurance verified.
- Trade-In conditions satisfied.
- Customer identity valid.
- Delivery Appointment available.
- No legal or operational hold.

`READY` must not be set solely from a User interface action or AI Recommendation.

### Delivery Rules

Delivery may begin only when:

- Delivery readiness is `READY`.
- Vehicle is authorized for release.
- Correct Customer or representative is present.
- Required identity checks pass.
- Required documents are available.
- Required Payment and funding conditions pass.
- Required Contract conditions pass.
- Required registration and insurance conditions pass.
- Delivery workflow is authorized.

The Deal must not become delivered solely because:

- Appointment was completed.
- Vehicle left a location.
- Salesperson marked a checklist.
- Customer sent a message.
- AI detected likely handover.

Authoritative delivery evidence is required.

### Sale and Accounting Rules

- Sale posting must use the configured DMS or accounting authority.
- A sent sale-posting Command does not prove posting.
- Revenue and cost recognition remain accounting-controlled.
- Estimated profitability must remain separate from confirmed profitability.
- Commission projections must not create payroll liability.
- Accounting failure or reversal must trigger reconciliation.
- Deal completion must not falsify accounting completion.

### Completion Rules

A Deal may become `COMPLETED` only when all configured completion requirements are satisfied.

These may include:

- Valid Customer commitment.
- Current accepted commercial snapshot.
- Required Vehicle Reservation and Allocation.
- Valid and effective Financial Contract where required.
- Required Customer Payment cleared.
- Required Lender funding confirmed.
- Required Trade-In conditions completed or governed.
- Required compliance cleared.
- Required documents completed.
- Required registration and title conditions.
- Required insurance.
- Authoritative delivery Confirmation where required.
- Authoritative sale posting where required.
- Accounting reconciliation at the required completion level.
- No material dispute, reversal, or reconciliation block.
- Completion Decision or External Confirmation where required.
- `completed_at`.

Completion requirements must remain configurable by:

- Transaction type.
- Jurisdiction.
- Lender.
- Legal entity.
- Dealership.
- Product.
- Delivery model.
- Accounting authority.

### Cancellation Rules

Cancellation normally applies before irreversible or effective transaction outcomes.

Cancellation requires:

- Valid reason.
- Authorized Human Decision or applicable policy.
- Assessment of Reservation and Allocation.
- Payment refund or release handling.
- Finance Application handling.
- Financial Contract handling.
- Trade-In handling.
- Registration handling.
- Delivery handling.
- Customer communication.
- External Confirmation where applicable.
- Reconciliation.

An effective, delivered, funded, or completed Deal may require unwind rather than ordinary cancellation.

### Unwind Rules

Unwind is required when reversing or correcting a materially executed transaction.

An unwind plan must assess:

- Customer.
- Opportunity.
- Quotation.
- Vehicle.
- Inventory.
- Reservation.
- Allocation.
- Trade-In.
- Finance Application.
- Financial Contract.
- Payment.
- Funding.
- Registration.
- Insurance.
- Delivery.
- Sale posting.
- Accounting.
- Tax.
- Commission.
- Customer communication.
- Legal obligations.

Unwind requires:

- Authoritative Human Decision.
- Legal and financial review.
- Controlled execution.
- External Confirmations.
- Reconciliation.
- Complete evidence.
- Preserved original Deal.

AI must not independently authorize or execute an unwind.

### Replacement Deal Rules

A replacement Deal may be created only through a governed process.

It must:

- Reference the original Deal.
- Preserve the replacement reason.
- Use a new `deal_id`.
- Share or reference the appropriate `deal_series_id`.
- Revalidate Customer, Quotation, Vehicle, Inventory, Trade-In, finance, Contract, Payment, and approvals.
- Avoid duplicate external posting.
- Preserve the original Deal history.

The original Deal must not be overwritten.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Deal creation must support idempotency.
- Replacement Deal creation must support idempotency.
- Reservation requests must support idempotency.
- Allocation requests must support idempotency.
- Payment Commands must support idempotency.
- Funding requests must support idempotency.
- Registration requests must support idempotency.
- Sale-posting requests must support idempotency.
- Completion processing must support idempotency.
- Cancellation and unwind requests must support idempotency.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Deals.
  - Reservations.
  - Allocations.
  - Payments.
  - Funding requests.
  - Registration requests.
  - Delivery outcomes.
  - Sale postings.
  - Cancellation records.
  - Unwind records.
  - Replacement Deals.

### External Authority Rules

When an external CRM, DMS, Inventory, Payment, funding, registration, delivery, or accounting system is authoritative:

- ASOS must issue approved Commands through Command Orchestration.
- Retryable Commands must use `idempotency_key`.
- Local state must remain pending until External Confirmation.
- Transport success does not equal business completion.
- Conflicting data must create reconciliation.
- Higher-authority evidence must not be silently overwritten.
- Missing Confirmation must trigger retry, polling, timeout, reconciliation, or Human escalation.

### Human Review Requirements

Human Review is required according to policy for:

- Duplicate Deal.
- Customer identity conflict.
- Quotation or commercial mismatch.
- Vehicle or Inventory conflict.
- Reservation or Allocation conflict.
- Pricing or margin exception.
- Trade-In conflict.
- Finance Decision conflict.
- Financial Contract mismatch.
- Payment shortfall.
- Funding shortfall or reversal.
- Compliance block.
- Registration failure.
- Delivery dispute.
- Accounting conflict.
- Deal completion exception.
- Cancellation after material execution.
- Deal unwind.
- Replacement Deal.
- Reopening a terminal Deal.
- Another material commercial, legal, financial, or operational risk.

---

## 6. State Machine

### Allowed States

```text
CREATED
VALIDATION_PENDING
APPROVAL_PENDING
APPROVED
EXECUTION_IN_PROGRESS
READY_FOR_DELIVERY
DELIVERY_PENDING
COMPLETION_PENDING
COMPLETED
CANCELLATION_PENDING
CANCELLED
UNWIND_PENDING
UNWOUND
DISPUTED
SUPERSEDED
ARCHIVED
```

Reservation, Allocation, Payment, funding, Contract, registration, delivery, sale-posting, and accounting states are governed as separate projections.

### Principal Allowed Transitions

```text
CREATED → VALIDATION_PENDING
CREATED → CANCELLATION_PENDING
CREATED → CANCELLED

VALIDATION_PENDING → CREATED
VALIDATION_PENDING → APPROVAL_PENDING
VALIDATION_PENDING → APPROVED
VALIDATION_PENDING → CANCELLATION_PENDING
VALIDATION_PENDING → DISPUTED

APPROVAL_PENDING → CREATED
APPROVAL_PENDING → APPROVED
APPROVAL_PENDING → CANCELLATION_PENDING
APPROVAL_PENDING → DISPUTED

APPROVED → EXECUTION_IN_PROGRESS
APPROVED → CANCELLATION_PENDING
APPROVED → DISPUTED

EXECUTION_IN_PROGRESS → READY_FOR_DELIVERY
EXECUTION_IN_PROGRESS → CANCELLATION_PENDING
EXECUTION_IN_PROGRESS → UNWIND_PENDING
EXECUTION_IN_PROGRESS → DISPUTED

READY_FOR_DELIVERY → DELIVERY_PENDING
READY_FOR_DELIVERY → EXECUTION_IN_PROGRESS
READY_FOR_DELIVERY → CANCELLATION_PENDING
READY_FOR_DELIVERY → UNWIND_PENDING
READY_FOR_DELIVERY → DISPUTED

DELIVERY_PENDING → COMPLETION_PENDING
DELIVERY_PENDING → READY_FOR_DELIVERY
DELIVERY_PENDING → UNWIND_PENDING
DELIVERY_PENDING → DISPUTED

COMPLETION_PENDING → COMPLETED
COMPLETION_PENDING → EXECUTION_IN_PROGRESS
COMPLETION_PENDING → UNWIND_PENDING
COMPLETION_PENDING → DISPUTED

CANCELLATION_PENDING → CANCELLED
CANCELLATION_PENDING → previous permitted non-terminal state
CANCELLATION_PENDING → UNWIND_PENDING
CANCELLATION_PENDING → DISPUTED

COMPLETED → UNWIND_PENDING
COMPLETED → DISPUTED
COMPLETED → ARCHIVED

UNWIND_PENDING → UNWOUND
UNWIND_PENDING → DISPUTED

DISPUTED → previous permitted non-terminal state
DISPUTED → CANCELLATION_PENDING
DISPUTED → UNWIND_PENDING

CANCELLED → SUPERSEDED
CANCELLED → ARCHIVED

UNWOUND → SUPERSEDED
UNWOUND → ARCHIVED

SUPERSEDED → ARCHIVED
```

Returning from `DISPUTED` or `CANCELLATION_PENDING` requires an accepted resolution and supporting evidence.

### Forbidden Ordinary Transitions

```text
CREATED → APPROVED
CREATED → EXECUTION_IN_PROGRESS
CREATED → COMPLETED

VALIDATION_PENDING → EXECUTION_IN_PROGRESS
APPROVAL_PENDING → EXECUTION_IN_PROGRESS

APPROVED → COMPLETED
EXECUTION_IN_PROGRESS → COMPLETED
READY_FOR_DELIVERY → COMPLETED

CANCELLED → EXECUTION_IN_PROGRESS
CANCELLED → READY_FOR_DELIVERY
CANCELLED → COMPLETED

UNWOUND → EXECUTION_IN_PROGRESS
UNWOUND → COMPLETED

COMPLETED → CANCELLED
COMPLETED → EXECUTION_IN_PROGRESS

SUPERSEDED → EXECUTION_IN_PROGRESS
SUPERSEDED → COMPLETED

ARCHIVED → CREATED
ARCHIVED → EXECUTION_IN_PROGRESS
ARCHIVED → COMPLETED
```

Corrections to terminal or materially executed outcomes require a governed correction, replacement, dispute, or unwind workflow.

### Entering CREATED

Requires:

- Valid Tenant context.
- Customer.
- Opportunity.
- Vehicle.
- Commercial commitment.
- Accepted Quotation or approved commercial basis.
- Responsible organizational context.
- Creation authority.
- Idempotency protection.
- Initial audit evidence.

### Entering VALIDATION_PENDING

Requires:

- Complete transaction snapshot.
- Source-record versions.
- Validation scope.
- Commercial calculation request.
- Duplicate check.
- Responsible workflow.

### Entering APPROVAL_PENDING

Requires:

- Successful core validation.
- Identified approval reasons.
- Frozen approval-request snapshot.
- Required approval roles.
- No blocking compliance conflict.

### Entering APPROVED

Requires:

- Valid Customer commitment.
- Valid accepted Quotation.
- Successful commercial calculation.
- Current Vehicle and Inventory eligibility.
- Required approvals.
- Current Trade-In and finance dependencies.
- No blocking conflict.
- Approval evidence.

### Entering EXECUTION_IN_PROGRESS

Requires:

- Approved Deal.
- Execution plan.
- Required Reservation or Allocation workflows started.
- Required Payment, finance, Contract, document, compliance, and registration workflows started.
- Responsible owners.
- No blocking execution conflict.

### Entering READY_FOR_DELIVERY

Requires:

- Delivery readiness evaluation.
- All configured readiness requirements satisfied.
- Required Payment and funding conditions.
- Required Contract conditions.
- Required Vehicle and Inventory conditions.
- Required documents and compliance.
- Required registration and insurance.
- No blocking conflict.

### Entering DELIVERY_PENDING

Requires:

- Delivery authorization.
- Valid Delivery Appointment or approved delivery workflow.
- Vehicle release authorization.
- Customer identity and handover controls.
- Delivery reference.
- No blocking condition.

### Entering COMPLETION_PENDING

Requires:

- Authoritative delivery or applicable fulfillment outcome.
- Required sale-posting workflow.
- Accounting handoff.
- Completion requirements evaluated.
- Any outstanding External Confirmations identified.

### Entering COMPLETED

Requires:

- All configured completion requirements satisfied.
- Authoritative outcome evidence.
- Required external systems reconciled.
- No unresolved material shortfall or dispute.
- Completion Decision where required.
- `completed_at`.

### Entering CANCELLATION_PENDING

Requires:

- Cancellation request.
- Valid reason.
- Impact assessment.
- Required authority.
- Review of active Commands and external workflows.

### Entering CANCELLED

Requires:

- Approved cancellation.
- Reservation and Allocation handling.
- Payment handling.
- Finance and Contract handling.
- Trade-In handling.
- Registration and delivery handling.
- Customer communication.
- External Confirmations where applicable.
- Cancellation evidence.

### Entering UNWIND_PENDING

Requires:

- Materially executed, effective, funded, delivered, posted, or completed transaction requiring reversal.
- Authorized unwind request.
- Impact assessment.
- Legal, financial, accounting, Inventory, and Customer plan.
- Required Human Decision.

### Entering UNWOUND

Requires:

- Approved unwind plan completed.
- Required refunds, reversals, releases, and external corrections.
- Vehicle and Inventory reconciled.
- Contract and funding reconciled.
- Registration and delivery reconciled.
- Accounting reconciled.
- Customer obligations resolved.
- Unwind evidence.
- `unwound_at`.

### Entering SUPERSEDED

Requires:

- Valid replacement Deal.
- Replacement reference.
- Original Deal no longer active.
- Required external reconciliation.
- Lineage evidence.

### Terminal States

For ordinary processing:

- `COMPLETED`
- `CANCELLED`
- `UNWOUND`
- `ARCHIVED`

A completed Deal may enter `UNWIND_PENDING` only through a governed high-impact workflow.

### Correction and Reopening

Correcting or reopening a material Deal outcome requires:

- Authorized Human Decision.
- Reason.
- Supporting evidence.
- Commercial, legal, finance, Payment, Inventory, delivery, and accounting review.
- New Event.
- Preserved original history.
- Replacement Deal or unwind where applicable.

AI Agents must not independently reopen, complete, cancel, unwind, or supersede a Deal.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
- Deal snapshot.
- Record version.
- Human Decision.
- Automation-policy reference where applicable.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Command.
- Related External Confirmation.

---

## 7. Relationships

### Tenant

- Every Deal belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant transaction processing requires an approved and auditable legal and technical mechanism.

### Customer

- Every Deal references one Customer.
- Customer identity remains governed by Customer.
- Customer snapshots preserve transaction context.
- Customer updates must not rewrite the approved Deal snapshot.

### Opportunity

- Every primary Deal references one Opportunity.
- An Opportunity normally creates one primary active Deal.
- Deal creation may support Opportunity `WON`.
- Opportunity remains the historical pursuit.
- Deal state must not rewrite Opportunity evidence.

### Quotation

- Every standard Deal references one exact accepted Quotation version.
- Deal preserves the issued and accepted document hashes.
- Deal must not alter Quotation terms.
- Material changes require Quotation and Deal governance.

### Vehicle

- Every Vehicle transaction references one Vehicle.
- Vehicle owns identity and specifications.
- Deal preserves the Vehicle snapshot used in the transaction.

### Inventory Record

- A physical Vehicle Deal references one Inventory Record.
- Inventory Record owns availability, Reservation, Allocation, sale, transfer, and delivery projections.
- Deal requests and consumes authoritative Inventory outcomes.

### Trade-In

- Deal may reference one or more permitted Trade-In records.
- Trade-In owns appraisal, payoff, ownership transfer, acquisition, and Inventory intake.
- Deal preserves exact appraisal and offer versions.

### Finance Application

- A financed Deal references one Finance Application.
- It preserves the exact application version and selected Lender Decision.
- Finance Application remains authoritative for the finance workflow.

### Financial Contract

- Deal may reference one current applicable Financial Contract.
- Financial Contract owns contractual terms, signatures, effectiveness, amendments, and legal lifecycle.
- Deal consumes authoritative Contract state.

### Payment

- Deal may reference multiple Payment transactions.
- Payment authority owns authorization, clearing, refund, reversal, and chargeback.
- Deal calculates transaction-level Payment requirements and projections.

### Funding Authority

- Deal may reference one or more funding transactions or Confirmations.
- Funding authority owns the confirmed funding outcome.
- Deal preserves only required transaction projections.

### Appointment

Appointments may support:

- Contract signing.
- Document collection.
- Payment coordination.
- Delivery.
- Handover.

Appointment completion does not independently prove the corresponding Deal outcome.

### Interaction

Interactions may provide:

- Customer commitment.
- Quotation acceptance.
- Payment communication.
- Contract communication.
- Delivery coordination.
- Cancellation request.
- Dispute evidence.

Original communication evidence remains governed by Interaction and its provider.

### Compliance Case

A Deal may reference:

- Identity review.
- Fraud review.
- Sanctions review.
- Source-of-funds review.
- Legal review.
- Customer dispute.
- Delivery dispute.

Restricted case details must remain purpose-limited.

### Registration and Title Authority

Registration and title systems own authoritative:

- Registration outcome.
- Ownership registration.
- Title transfer.
- Plate assignment.
- Government reference.

Deal stores only required projections and references.

### Insurance Authority

Insurance provider or approved verifier owns authoritative policy status.

### Delivery Workflow

Delivery workflow owns:

- Delivery schedule.
- Release authorization.
- Handover checklist.
- Customer identity at handover.
- Vehicle condition at handover.
- Delivery evidence.
- Delivery Confirmation.

### Accounting and DMS

Accounting or DMS systems may own:

- External Deal number.
- Sale posting.
- Receivable.
- Revenue.
- Cost of sale.
- Profit.
- Commission basis.
- Tax posting.
- Reversal.

ASOS preserves Canonical Projections and reconciliation.

### Replacement Deal

A replacement Deal must reference the original Deal.

The original Deal must reference the replacement where applicable.

Circular replacement chains are prohibited.

### Supporting Child Records

Deal may own or govern:

- Deal snapshots.
- Approval requests.
- Approval Decisions.
- Completion requirements.
- Reservation requests.
- Allocation requests.
- Payment requirements.
- Funding requirements.
- Compliance projections.
- Document requirements.
- Delivery-readiness evaluations.
- Cancellation records.
- Unwind records.
- Replacement lineage.
- Dispute records.
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

The following are required Deal Event concepts and do not replace the Event Catalog.

### Deal Creation and Validation Event Concepts

- Deal creation requested.
- Deal created.
- Deal duplicate candidate detected.
- Deal validation requested.
- Deal validation completed.
- Deal validation failed.
- Deal commercial mismatch detected.
- Deal conflict detected.
- Deal conflict resolved.

### Approval Event Concepts

- Deal approval evaluated.
- Deal approval requested.
- Deal partially approved.
- Deal approved.
- Deal approval rejected.
- Deal approval expired.
- Deal approval revoked.
- Deal revalidation required.

### Inventory Event Concepts

- Deal Reservation requested.
- Deal Reservation confirmed.
- Deal Reservation rejected.
- Deal Reservation expired.
- Deal Reservation released.
- Deal Allocation requested.
- Deal Allocation confirmed.
- Deal Allocation rejected.
- Deal Allocation released.
- Deal Inventory conflict detected.
- Deal Inventory reconciliation required.

### Contract Event Concepts

- Deal Contract preparation requested.
- Deal Financial Contract created.
- Deal Contract signature status updated.
- Deal Contract became effective.
- Deal Contract activated.
- Deal Contract mismatch detected.
- Deal Contract reconciliation required.

### Payment Event Concepts

- Deal Payment requirement created.
- Deal Payment requested.
- Deal Payment authorized.
- Deal Payment captured.
- Deal Payment cleared.
- Deal Payment shortfall detected.
- Deal Payment refunded.
- Deal Payment reversed.
- Deal Payment chargeback received.
- Deal Payment reconciliation required.

### Funding Event Concepts

- Deal funding readiness evaluated.
- Deal funding requested.
- Deal funding Command sent.
- Deal partially funded.
- Deal funding confirmed.
- Deal funding shortfall detected.
- Deal funding failed.
- Deal funding reversed.
- Deal funding reconciliation required.

### Compliance and Document Event Concepts

- Deal documents requested.
- Deal document requirement completed.
- Deal compliance review requested.
- Deal compliance cleared.
- Deal compliance blocked.
- Deal compliance dispute opened.

### Registration and Insurance Event Concepts

- Deal registration requested.
- Deal registration confirmed.
- Deal registration failed.
- Deal title transfer confirmed.
- Deal insurance verified.
- Deal insurance expired.
- Deal registration reconciliation required.

### Delivery Event Concepts

- Deal delivery readiness evaluated.
- Deal became ready for delivery.
- Deal delivery scheduling requested.
- Deal delivery started.
- Deal Vehicle handover recorded.
- Deal delivery confirmed.
- Deal delivery failed.
- Deal delivery disputed.
- Deal delivery reconciliation required.

### Sale and Accounting Event Concepts

- Deal sale posting requested.
- Deal sale posting confirmed.
- Deal sale posting failed.
- Deal accounting handoff requested.
- Deal accounting confirmed.
- Deal profitability confirmed.
- Deal accounting reversed.
- Deal accounting reconciliation required.

### Completion Event Concepts

- Deal completion evaluation requested.
- Deal completion requirements satisfied.
- Deal completion blocked.
- Deal completed.
- Deal completion disputed.
- Deal completion corrected.

### Cancellation and Unwind Event Concepts

- Deal cancellation requested.
- Deal cancellation approved.
- Deal cancellation rejected.
- Deal cancelled.
- Deal unwind requested.
- Deal unwind approved.
- Deal unwind started.
- Deal partially unwound.
- Deal unwound.
- Deal unwind failed.
- Replacement Deal created.
- Deal superseded.

### Derived Intelligence Event Concepts

- Deal completion probability updated.
- Deal delivery-delay risk detected.
- Deal funding-delay risk detected.
- Deal Payment-shortfall risk detected.
- Deal cancellation risk updated.
- Deal margin risk detected.
- Deal next action recommended.
- Deal Human Review recommended.

Derived Intelligence Events must not imply:

- Deal approval.
- Vehicle Reservation.
- Vehicle Allocation.
- Payment settlement.
- Lender funding.
- Contract signature.
- Contract effectiveness.
- Registration.
- Delivery.
- Sale posting.
- Accounting completion.
- Deal completion.
- Cancellation.
- Unwind.
- Human Approval.
- External completion.

### Producer Rules

- Deal Domain Service publishes accepted Deal canonical and workflow-state changes.
- Opportunity and Quotation Domain Services publish accepted commercial facts.
- Vehicle and Inventory Domain Services publish accepted Vehicle and Inventory facts.
- Trade-In Domain Service publishes accepted Trade-In facts.
- Finance Application and Financial Contract Domain Services publish accepted finance and contract facts.
- Payment, funding, registration, delivery, DMS, and accounting integrations publish normalized external observations.
- AI Agents may publish Agent-run, analysis, prediction, anomaly, or Recommendation Events.
- AI Agents must not publish authoritative Reservation, Allocation, Payment, funding, signature, delivery, accounting, completion, cancellation, or unwind Events merely because they predicted or recommended the outcome.

### Event Requirements

Every material Deal Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `deal_id`.
- `deal_series_id`.
- Opportunity.
- Customer.
- Quotation and version.
- Vehicle and Inventory Record.
- Trade-In references.
- Finance Application and Lender Decision.
- Financial Contract.
- Dealership and legal entity.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Commercial snapshot hash.
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

Corrections, cancellation, unwind, replacement, Payment reversal, funding reversal, delivery reversal, and accounting reversal must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Deal-completeness analysis.
- Commercial-snapshot comparison.
- Quotation-to-Deal mismatch detection.
- Contract-to-Deal mismatch detection.
- Payment-shortfall analysis.
- Funding-delay analysis.
- Delivery-readiness analysis.
- Missing-document detection.
- Compliance-delay risk detection.
- Deal cancellation-risk prediction.
- Margin-risk analysis.
- Next-action Recommendation.
- Escalation Recommendation.
- Delivery-date Recommendation.
- Deal-summary drafting.
- Customer-communication drafting.
- Reconciliation-case summarization.
- Dispute-evidence summarization.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create authoritative Customer commitment.
- Approve a Deal.
- Approve a discount or margin exception.
- Reserve or allocate a Vehicle.
- Confirm Payment.
- Confirm Lender funding.
- Confirm Contract signature.
- Confirm Contract effectiveness.
- Clear compliance.
- Confirm registration.
- Confirm insurance.
- Authorize Vehicle release.
- Confirm delivery.
- Post a sale.
- Recognize revenue.
- Confirm accounting completion.
- Mark a Deal completed.
- Cancel a materially executed Deal.
- Authorize an unwind.
- Create a replacement Deal outside approved authority.
- Execute external Commands directly.
- Access Deal data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Deal identifier and record version.
- Commercial snapshot version.
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

### Deal Readiness Recommendations

AI may recommend that a Deal appears ready for:

- Approval.
- Contracting.
- Payment follow-up.
- Funding request.
- Delivery preparation.
- Completion review.

The deterministic Policy Engine and authoritative source data must validate all requirements.

AI readiness must not become authoritative Deal state by itself.

### Commercial Analysis

AI may explain:

- Approved commercial terms.
- Customer-visible differences.
- Material mismatches.
- Potential margin risk.
- Missing approval.

AI must not:

- Alter the Quotation.
- Alter the Deal snapshot.
- Expose internal margin to the Customer.
- Invent prices, taxes, fees, incentives, or approval authority.
- Present a Recommendation as approved commercial terms.

### Payment and Funding Analysis

AI may identify possible:

- Payment shortfall.
- Funding delay.
- Reconciliation gap.
- Missing Confirmation.
- Inconsistent amount.

AI must not treat:

- Provider acknowledgment.
- Bank instruction.
- Pending transfer.
- Customer statement.
- Predicted funding.

as authoritative cleared funds.

### Delivery Analysis

AI may recommend delivery preparation steps.

It must not confirm:

- Customer identity.
- Vehicle release authorization.
- Physical handover.
- Delivery.
- Registration.
- Insurance.

without authoritative evidence.

### Customer-Facing Drafting

AI may draft Deal-related communication only when:

- The purpose is permitted.
- Customer contact is permitted.
- Current authoritative Deal facts are supplied.
- Applicable template is approved.
- Sensitive internal data is excluded.
- Human Approval or an approved automation policy applies.

AI must not claim:

- Vehicle is reserved when pending.
- Finance is funded when pending.
- Contract is effective when only signed.
- Delivery is confirmed when only scheduled.
- Deal is completed when requirements remain outstanding.

### Action Class 2

Controlled Deal status updates, document requests, Payment reminders, and delivery coordination messages may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Customer permission.
- Purpose.
- Channel.
- Template.
- Deal state.
- Current authoritative facts.
- Frequency.
- Quiet hours.
- Document sensitivity.
- Payment and funding communication restrictions.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact Deal actions require an Authoritative Human Decision or External Authoritative Decision.

Examples include:

- Deal approval.
- Commercial exception.
- Reservation or Allocation override.
- Payment exception.
- Funding request.
- Delivery authorization.
- Completion.
- Cancellation after material execution.
- Unwind.
- Replacement Deal.
- Accounting correction.
- Dispute resolution.

### AI Context and Embeddings

Deal data must not enter unrestricted embeddings.

Normally excluded fields include:

- Customer identifiers.
- Addresses.
- Payment information.
- Banking information.
- Funding references.
- Contract documents.
- Signature evidence.
- National identifiers.
- Insurance documents.
- Registration documents.
- Internal Vehicle cost.
- Internal gross profit.
- Internal margin.
- Commission information.
- Fraud and compliance details.
- Dispute evidence.
- Unwind plans.

Approved redacted context may include:

- Deal type.
- General Deal status.
- Non-sensitive blocker categories.
- General document-completion category.
- General delivery-readiness category.
- Non-sensitive next-action summary.
- Redacted commercial summary.

Every vector record must enforce:

- `tenant_id`.
- Deal access scope.
- Customer-purpose scope.
- Source references.
- Snapshot version.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Untrusted Documents and Prompt Injection

Deal documents, Customer documents, Lender documents, Payment descriptions, and external system notes are untrusted input.

AI Agents must treat them as data, not instructions.

Document content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Trigger external Commands.
- Change Deal state.
- Approve a Deal.
- Confirm Payment.
- Confirm funding.
- Confirm delivery.
- Alter accounting.
- Modify audit records.

### Explainability

Material Deal Recommendations must explain:

- Evidence used.
- Authority of each input.
- Data freshness.
- Deal record version.
- Commercial snapshot.
- Outstanding requirements.
- Material mismatches.
- Payment and funding state.
- Contract state.
- Inventory state.
- Delivery state.
- Accounting state.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Required Human authority.
- External Confirmation requirements.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Deal API behaviour.

### REST Resources

```text
GET    /api/v1/deals
POST   /api/v1/deals
GET    /api/v1/deals/{deal_id}
PATCH  /api/v1/deals/{deal_id}

POST   /api/v1/opportunities/{opportunity_id}/deal-conversion-requests
POST   /api/v1/deals/{deal_id}/validation-requests
POST   /api/v1/deals/{deal_id}/approval-requests
POST   /api/v1/deals/{deal_id}/approval-decisions

POST   /api/v1/deals/{deal_id}/reservation-requests
POST   /api/v1/deals/{deal_id}/reservation-release-requests
POST   /api/v1/deals/{deal_id}/allocation-requests
POST   /api/v1/deals/{deal_id}/allocation-release-requests

POST   /api/v1/deals/{deal_id}/payment-requirements
POST   /api/v1/deals/{deal_id}/payment-reconciliation-requests
POST   /api/v1/deals/{deal_id}/funding-readiness-checks
POST   /api/v1/deals/{deal_id}/funding-requests

POST   /api/v1/deals/{deal_id}/document-requests
POST   /api/v1/deals/{deal_id}/compliance-review-requests
POST   /api/v1/deals/{deal_id}/registration-requests
POST   /api/v1/deals/{deal_id}/delivery-readiness-checks
POST   /api/v1/deals/{deal_id}/delivery-authorization-requests

POST   /api/v1/deals/{deal_id}/sale-posting-requests
POST   /api/v1/deals/{deal_id}/completion-requests
POST   /api/v1/deals/{deal_id}/cancellation-requests
POST   /api/v1/deals/{deal_id}/unwind-requests
POST   /api/v1/deals/{deal_id}/replacement-deal-requests
POST   /api/v1/deals/{deal_id}/dispute-requests
POST   /api/v1/deals/{deal_id}/correction-requests

GET    /api/v1/deals/{deal_id}/requirements
GET    /api/v1/deals/{deal_id}/commercial-snapshot
GET    /api/v1/deals/{deal_id}/payment-status
GET    /api/v1/deals/{deal_id}/funding-status
GET    /api/v1/deals/{deal_id}/delivery-readiness
GET    /api/v1/deals/{deal_id}/completion-readiness
GET    /api/v1/deals/{deal_id}/external-synchronization
GET    /api/v1/deals/{deal_id}/reconciliation
GET    /api/v1/deals/{deal_id}/history
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Legal entity, dealership, branch, team, User, and approval scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Deal-Conversion Request

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "accepted_quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "accepted_quotation_version": 1,
  "accepted_quotation_document_hash": "sha256:8ac44d5d...",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "trade_in_ids": [
    "a6cd98db-b21d-43a0-ad40-e027a62994da"
  ],
  "finance_application_id": "9d2ad54a-d4af-45c9-a152-b4f64dcfd233",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "legal_entity_id": "aed9092a-b3db-4a28-b96e-bc4bbb4b99dc",
  "deal_type": "RETAIL_FINANCE",
  "transaction_type": "NEW_VEHICLE",
  "sales_channel": "SHOWROOM"
}
```

The request must include:

```text
Idempotency-Key: a8d065e2-98e6-4de0-9d14-a2492957bc29
```

### Example Created Response

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "deal_series_id": "c3555054-7aae-483f-bc04-69b582d78736",
  "deal_number": "DL-2026-000248",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "status": "CREATED",
  "approval_status": "NOT_EVALUATED",
  "reservation_status": "NOT_REQUESTED",
  "allocation_status": "NOT_REQUESTED",
  "payment_status": "PAYMENT_REQUIRED",
  "funding_status": "NOT_STARTED",
  "delivery_readiness_status": "NOT_ASSESSED",
  "completion_status": "NOT_EVALUATED",
  "data_quality_status": "COMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T20:30:00Z"
}
```

### Example Reservation Request

```json
{
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "expected_inventory_record_version": 12,
  "expected_deal_record_version": 4
}
```

The request must use an idempotency key.

A pending response may be:

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "reservation_status": "PENDING_CONFIRMATION",
  "reservation_confirmation_status": "PENDING",
  "command_id": "c10fd71e-9835-471d-a02c-20474a403420",
  "record_version": 5
}
```

The API must not describe the Vehicle as reserved until authoritative Confirmation exists.

### Example Approval Response

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "status": "APPROVED",
  "approval_status": "APPROVED",
  "commercial_snapshot_hash": "sha256:e51af221...",
  "approved_at": "2026-08-02T11:00:00Z",
  "record_version": 8
}
```

### Example Funding Projection

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "funding_required": true,
  "funding_readiness_status": "READY",
  "funding_status": "PENDING_CONFIRMATION",
  "funding_amount_required": 1650000,
  "funded_amount": 0,
  "funding_shortfall_amount": 1650000,
  "funding_currency_code": "EGP",
  "command_id": "4cf52c54-f0c2-494f-b87f-09314193a354",
  "record_version": 14
}
```

The Deal must not represent the funding as received.

### Example Delivery-Readiness Response

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "delivery_readiness_status": "PARTIALLY_READY",
  "outstanding_delivery_requirements": [
    "FUNDING_CONFIRMATION",
    "REGISTRATION_CONFIRMATION"
  ],
  "payment_readiness_status": "READY",
  "contract_readiness_status": "READY",
  "vehicle_readiness_status": "READY",
  "record_version": 18
}
```

### Example Completion Request

```json
{
  "expected_record_version": 26,
  "expected_delivery_confirmation_reference": "delivery://confirmations/DLV-2026-8821",
  "expected_sale_posting_reference": "dms://sales/SALE-2026-11582"
}
```

A pending response may be:

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "status": "COMPLETION_PENDING",
  "completion_status": "PENDING_CONFIRMATION",
  "outstanding_completion_requirements": [
    "ACCOUNTING_CONFIRMATION"
  ],
  "record_version": 27
}
```

A confirmed response may be:

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "status": "COMPLETED",
  "completion_status": "COMPLETED",
  "delivery_status": "DELIVERED",
  "sale_posting_status": "POSTED",
  "accounting_handoff_status": "COMPLETED",
  "completed_at": "2026-08-15T14:40:00Z",
  "record_version": 30
}
```

### Example Unwind Request

```json
{
  "unwind_reason": "FUNDING_REVERSED",
  "business_reason": "The Lender reversed the funding after authoritative post-delivery review.",
  "expected_record_version": 31
}
```

The request must not directly reverse Payment, Inventory, Contract, delivery, or accounting state.

It creates a governed unwind workflow.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Accepted-Quotation validation.
- Commercial-snapshot validation.
- Vehicle and Inventory checks.
- Trade-In and finance checks.
- Contract, Payment, funding, compliance, and delivery checks.
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

- Deals.
- Replacement Deals.
- Reservations.
- Allocations.
- Payment requests.
- Funding requests.
- Registration requests.
- Sale postings.
- Completion outcomes.
- Cancellation requests.
- Unwind requests.

### Pending External Confirmation

Operations requiring an external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "command_id": "c10fd71e-9835-471d-a02c-20474a403420",
  "record_version": 5
}
```

The API must not describe the operation as complete until authoritative evidence exists.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_DEAL_REVIEW_REQUIRED`
- `OPPORTUNITY_NOT_ELIGIBLE`
- `CUSTOMER_MISMATCH`
- `QUOTATION_REQUIRED`
- `QUOTATION_INVALID`
- `QUOTATION_VERSION_MISMATCH`
- `CUSTOMER_COMMITMENT_INVALID`
- `VEHICLE_MISMATCH`
- `INVENTORY_NOT_ELIGIBLE`
- `INVENTORY_DATA_STALE`
- `RESERVATION_CONFLICT`
- `ALLOCATION_CONFLICT`
- `TRADE_IN_DATA_STALE`
- `FINANCE_DECISION_REQUIRED`
- `FINANCE_DECISION_EXPIRED`
- `FINANCIAL_CONTRACT_REQUIRED`
- `CONTRACT_NOT_EFFECTIVE`
- `PAYMENT_SHORTFALL`
- `PAYMENT_NOT_CLEARED`
- `FUNDING_NOT_READY`
- `FUNDING_SHORTFALL`
- `COMPLIANCE_BLOCK`
- `DOCUMENTS_INCOMPLETE`
- `REGISTRATION_REQUIRED`
- `INSURANCE_REQUIRED`
- `DELIVERY_NOT_READY`
- `DELIVERY_NOT_CONFIRMED`
- `SALE_POSTING_NOT_CONFIRMED`
- `ACCOUNTING_NOT_RECONCILED`
- `COMPLETION_REQUIREMENTS_OUTSTANDING`
- `HUMAN_APPROVAL_REQUIRED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `CANCELLATION_NOT_PERMITTED`
- `UNWIND_REQUIRED`
- `DEAL_TERMINAL`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Field authority.
- Commercial-snapshot immutability.
- Inventory authority.
- Payment and funding authority.
- Contract and delivery authority.
- Concurrency.
- Idempotency.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Deal Domain Service, Policy Engine, Command Orchestration, or authoritative child services.

---

## 11. Database Design

### Recommended Tables

```text
deal_series
deals
deal_snapshots
deal_customer_commitments
deal_quotation_references
deal_vehicle_inventory_context
deal_reservation_requests
deal_allocation_requests
deal_trade_in_references
deal_finance_references
deal_contract_references
deal_payment_requirements
deal_payment_projections
deal_funding_requirements
deal_funding_projections
deal_approval_requests
deal_approval_decisions
deal_compliance_projections
deal_document_requirements
deal_registration_projections
deal_insurance_projections
deal_delivery_requirements
deal_delivery_projections
deal_completion_requirements
deal_sale_posting_projections
deal_accounting_projections
deal_profitability_projections
deal_commission_projections
deal_cancellations
deal_unwinds
deal_replacements
deal_disputes
deal_external_references
deal_external_confirmations
deal_derived_intelligence
deal_reconciliation_cases
deal_data_quality_issues
deal_status_history
deal_record_versions
deal_audit_log
```

### Deal Series Table

`deal_series` should contain:

- `deal_series_id`.
- `tenant_id`.
- Opportunity.
- Customer.
- Original Deal.
- Current Deal.
- Latest replacement.
- Series status.
- Created time.
- Updated time.

### Deals Table

The `deals` table should contain:

- Canonical identifiers.
- Deal series.
- Tenant and organizational scope.
- Opportunity and Customer.
- Current Quotation, Vehicle, Inventory, Trade-In, finance, Contract, and delivery references.
- Current aggregate lifecycle state.
- Current approval state.
- Current Reservation and Allocation projections.
- Current Payment and funding projections.
- Current compliance and document projections.
- Current registration, insurance, delivery, sale-posting, accounting, and completion projections.
- Cancellation, unwind, replacement, and dispute projections.
- Data-quality and conflict state.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Historical and repeating data must remain in child or history tables.

### Primary Keys

```text
PRIMARY KEY (deal_series_id)
```

for `deal_series`.

```text
PRIMARY KEY (deal_id)
```

for `deals`.

### Tenant Protection

Every Deal-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_deals_tenant_status
  (tenant_id, status)

idx_deals_tenant_opportunity
  (tenant_id, opportunity_id)

idx_deals_tenant_customer
  (tenant_id, customer_id)

idx_deals_tenant_series
  (tenant_id, deal_series_id)

idx_deals_tenant_quotation
  (tenant_id, accepted_quotation_id)

idx_deals_tenant_vehicle
  (tenant_id, vehicle_id)

idx_deals_tenant_inventory
  (tenant_id, inventory_record_id)

idx_deals_tenant_finance
  (tenant_id, finance_application_id)

idx_deals_tenant_contract
  (tenant_id, financial_contract_id)

idx_deals_reservation
  (tenant_id, reservation_status)

idx_deals_allocation
  (tenant_id, allocation_status)

idx_deals_payment
  (tenant_id, payment_status)

idx_deals_funding
  (tenant_id, funding_status)

idx_deals_delivery
  (tenant_id, delivery_status)

idx_deals_completion
  (tenant_id, completion_status)

idx_deals_reconciliation
  (tenant_id, reconciliation_status)

idx_deals_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, deal_number)
```

External source uniqueness may use:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the source guarantees uniqueness.

A partial unique constraint or equivalent service control should normally enforce one primary active Deal per Opportunity:

```text
UNIQUE (tenant_id, opportunity_id)
WHERE is_primary_deal = true
  AND status NOT IN (
    'CANCELLED',
    'UNWOUND',
    'SUPERSEDED',
    'ARCHIVED'
  )
```

One active physical Inventory Record should not be allocated to incompatible Deals.

That constraint must be enforced by Inventory authority through concurrency-safe controls.

### Deal Snapshots

`deal_snapshots` should preserve:

- Snapshot identifier.
- Deal.
- Record version.
- Customer snapshot.
- Quotation snapshot.
- Vehicle snapshot.
- Inventory snapshot.
- Trade-In snapshot.
- Finance snapshot.
- Commercial snapshot.
- Calculation snapshot.
- Approval snapshot.
- Completion-requirement snapshot.
- Source-record versions.
- Snapshot hashes.
- Created by.
- Created at.
- Related Events.

Approved execution snapshots must remain immutable.

### Reservation and Allocation Storage

Reservation and Allocation tables should preserve:

- Request identifier.
- Deal.
- Inventory Record.
- Expected Inventory version.
- Status.
- Command.
- Idempotency key.
- Requested time.
- External reference.
- Confirmation.
- Expiration.
- Release.
- Failure reason.
- Reconciliation state.
- Related Events.

### Payment Requirement Storage

`deal_payment_requirements` should preserve:

- Requirement identifier.
- Deal.
- Requirement type.
- Required amount.
- Currency.
- Due time.
- Refundability.
- Policy.
- Status.
- Satisfaction references.
- Related Events.

Payment projections must reference authoritative Payment transactions rather than duplicate them.

### Funding Storage

Funding tables should preserve:

- Funding requirement.
- Deal.
- Finance Application.
- Lender Decision.
- Financial Contract.
- Required amount.
- Requested amount.
- Confirmed amount.
- Currency.
- Command.
- Idempotency key.
- External Confirmation.
- Shortfall.
- Reversal.
- Reconciliation.
- Related Events.

### Approval Storage

Approval tables should preserve:

- Request identifier.
- Deal and snapshot version.
- Requested scope.
- Triggered policy.
- Values.
- Required role.
- Assigned approver.
- Decision.
- Reason.
- Evidence.
- Effective period.
- Revocation.
- Related Events.

### Delivery Requirements Storage

`deal_delivery_requirements` should preserve:

- Requirement identifier.
- Deal.
- Requirement type.
- Authority.
- Required state.
- Current projection.
- Evidence.
- Expiration.
- Satisfaction status.
- Related Event.

### Completion Requirements Storage

`deal_completion_requirements` should preserve:

- Requirement identifier.
- Deal.
- Requirement set and version.
- Requirement type.
- Authority.
- Required state.
- Current state.
- Evidence.
- External Confirmation.
- Satisfaction timestamp.
- Reversal state.
- Related Events.

### Cancellation Storage

`deal_cancellations` should preserve:

- Cancellation identifier.
- Deal.
- Deal record version.
- Reason.
- Requesting actor.
- Decision.
- Impact assessment.
- Reservation handling.
- Allocation handling.
- Payment handling.
- Finance handling.
- Contract handling.
- Trade-In handling.
- Registration handling.
- Delivery handling.
- External Confirmations.
- Reconciliation.
- Related Events.

### Unwind Storage

`deal_unwinds` should preserve:

- Unwind identifier.
- Deal.
- Reason.
- Requesting actor.
- Human Decision.
- Legal review.
- Financial review.
- Inventory plan.
- Payment plan.
- Funding plan.
- Contract plan.
- Trade-In plan.
- Delivery plan.
- Registration plan.
- Accounting plan.
- Commands.
- External Confirmations.
- Progress.
- Completion.
- Reconciliation.
- Related Events.

### Replacement Storage

`deal_replacements` should preserve:

- Original Deal.
- Replacement Deal.
- Deal series.
- Reason.
- Actor.
- Decision.
- Created time.
- Source snapshot.
- Reconciliation.
- Related Events.

### Profitability Storage

Estimated and confirmed profitability must remain separate.

Profitability records should preserve:

- Source authority.
- Cost components.
- Revenue components.
- Estimate or confirmed classification.
- Formula.
- Rule version.
- Accounting period.
- Timestamp.
- Reversal.
- Related Events.

### Derived Intelligence

Derived Deal records must remain separate from authoritative workflow, Payment, funding, delivery, and accounting fields.

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

Deal audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw financial, identity, contract, Payment, funding, and dispute values where full retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Legal entity.
- Dealership.
- Creation date.
- Completion date.
- Status.
- Retention class.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Opportunity uniqueness.
- Deal lineage.
- Inventory concurrency.
- Snapshot immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Deal must not be hard-deleted when referenced by:

- Customer journey.
- Opportunity.
- Quotation.
- Vehicle.
- Inventory Record.
- Trade-In.
- Finance Application.
- Financial Contract.
- Payment.
- Funding.
- Registration.
- Delivery.
- Accounting.
- Interaction.
- Approval.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Compliance or dispute evidence.
- Audit evidence.

Cancellation, unwind, supersession, archival, anonymization, governed redaction, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `CUSTOMER_IDENTIFIER_REFERENCE` | Customer and representative references |
| `COMMERCIAL_RESTRICTED` | Prices, discounts, Trade-In allowance |
| `INTERNAL_PRICING_RESTRICTED` | Vehicle cost, margin, profit |
| `FINANCIAL_RESTRICTED` | Payment, funding, finance, bank references |
| `CONTRACTUAL_RESTRICTED` | Contract state and document references |
| `INVENTORY_OPERATIONAL` | Reservation, Allocation, Vehicle location |
| `DELIVERY_RESTRICTED` | Handover, delivery identity, release evidence |
| `LEGAL_AND_COMPLIANCE` | Compliance, disputes, unwind evidence |
| `ACCOUNTING_CONFIDENTIAL` | Revenue, cost, commission, accounting posting |
| `DERIVED_INTELLIGENCE` | Risk scores and Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, history |

### Authentication

Every internal Deal operation requires an authenticated Human or service identity.

Customer access to Deal documents or status must use an approved secure authentication or verification mechanism.

Anonymous unrestricted Deal access is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Legal entity.
- Branch.
- Department.
- Team.
- Responsible User.
- Customer relationship.
- Opportunity relationship.
- Deal status.
- Deal type.
- Monetary value.
- Requested field.
- Requested action.
- Data classification.
- Business purpose.
- Delegated authority.
- Legal hold.

### Example Role Boundaries

#### Sales Consultant

May access permitted:

- Assigned Deal summary.
- Customer-facing commercial terms.
- Reservation and Allocation status.
- Document status.
- Customer Payment requirement summary.
- Delivery readiness.
- Approved follow-up.

Must not independently:

- Approve restricted pricing.
- Confirm Payment.
- Confirm funding.
- Alter Lender terms.
- Confirm Contract effectiveness.
- Override compliance.
- Authorize restricted Vehicle release.
- Confirm accounting.
- Complete or unwind the Deal.

#### Sales Manager

May perform configured:

- Deal approval.
- Commercial exception review.
- Assignment.
- Deal escalation.
- Cancellation review.
- Replacement Deal review.

Manager access does not automatically authorize:

- Lender underwriting.
- Contract legal validity.
- Payment settlement.
- Funding Confirmation.
- Accounting posting.
- Cross-Tenant access.

#### Finance Specialist

May access finance, Contract, funding, and Customer contribution context required for assigned Deals.

Finance access does not authorize altering Lender Decisions or confirming funding without evidence.

#### Finance Manager

May perform configured:

- Funding readiness review.
- Finance exception review.
- Funding-shortfall handling.
- Contract and funding reconciliation.

#### Inventory User

May access Vehicle, Reservation, Allocation, location, readiness, and release context required for the Deal.

Inventory access does not authorize pricing, finance, Payment, or Deal completion.

#### Delivery Coordinator

May access:

- Delivery readiness.
- Delivery Appointment.
- Vehicle release requirements.
- Customer identity checks required for handover.
- Handover workflow.

Delivery Coordinator must not bypass Payment, funding, Contract, registration, insurance, or compliance controls.

#### Accounting User

May access:

- Sale posting.
- Customer receivable projection.
- Cost and profitability.
- Tax.
- Reconciliation.
- Accounting reversal.

Accounting access does not authorize Customer contract changes or Vehicle delivery.

#### Compliance or Legal Reviewer

May access restricted Deal evidence required for the assigned review.

#### Data Steward

May review:

- Duplicate Deals.
- Relationship conflicts.
- Source mappings.
- Reconciliation.
- Data-quality issues.
- Deal lineage.

#### AI Agent

May access only the minimum Deal context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to Payment, banking, funding, Contract, identity, compliance, margin, commission, and dispute data.

#### Integration Service

May access only fields required for an approved CRM, DMS, Inventory, Payment, funding, registration, delivery, or accounting integration.

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

- Vehicle cost.
- Internal margin.
- Gross profit.
- Commissionable gross.
- Payment references.
- Funding references.
- Banking information.
- Contract documents.
- Customer identity evidence.
- Compliance findings.
- Unwind plans.

### Customer-Facing Protection

Customer-facing Deal views must not expose:

- Internal Vehicle cost.
- Internal gross profit.
- Margin.
- Commission.
- Approval thresholds.
- Fraud indicators.
- Internal compliance notes.
- Internal funding instructions.
- Internal accounting status beyond permitted summaries.
- Unrelated Customer or Deal information.

### Payment and Funding Protection

Payment and funding data must use:

- Encryption.
- Tokenized account references where applicable.
- Field-level authorization.
- Controlled instructions.
- Idempotency protection.
- External Confirmation.
- Reconciliation.
- Audit logging.

Payment or funding instructions must never be copied into:

- Prompts.
- Ordinary Logs.
- General-purpose embeddings.
- Public documents.
- Unapproved Customer messages.

### Contract and Document Protection

Contract and Deal documents must:

- Use controlled storage.
- Preserve hashes.
- Preserve versions.
- Use authenticated access.
- Prevent public indexing.
- Prevent uncontrolled sharing.
- Support legal holds.
- Preserve access logs.
- Exclude sensitive content from unrestricted AI context.

### Inventory and Delivery Protection

Exact Vehicle location, release codes, key-control information, secure storage details, and delivery instructions must be restricted.

The platform must prevent:

- Unauthorized Vehicle release.
- Unauthorized Allocation change.
- False handover.
- Delivery to the wrong Customer.
- Exposure of secure Vehicle location.
- Release before required Payment, funding, Contract, compliance, registration, or insurance conditions.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Deal matching.
- Duplicate detection.
- Inventory Reservation and Allocation.
- Search.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Documents.
- Payments.
- Funding.
- Delivery.
- Accounting.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Deal Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational and legal-entity scope.
- Deal identifier.
- Current Deal record version.
- Relevant child-record versions.
- Requested action.
- Field-level authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Deal activity must record:

- `tenant_id`.
- `deal_id`.
- `deal_series_id`.
- Opportunity.
- Customer.
- Quotation and version.
- Vehicle and Inventory Record.
- Trade-In.
- Finance Application.
- Lender Decision.
- Financial Contract.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
- Commercial snapshot hash.
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

- Cross-Tenant Deal access attempts.
- Unauthorized Deal creation.
- Duplicate Deal creation.
- Quotation or snapshot substitution.
- Unauthorized price or margin changes.
- Reservation or Allocation manipulation.
- False Payment recording.
- False funding Confirmation.
- Contract-status manipulation.
- Compliance bypass.
- Unauthorized Vehicle release.
- False delivery Confirmation.
- False sale posting.
- False accounting completion.
- Unauthorized Deal completion.
- Unauthorized cancellation.
- Unauthorized unwind.
- Command replay.
- External Confirmation mismatch.
- Bulk Deal export.
- AI access outside approved scope.
- Prompt-injection attempts inside Deal documents.
- Audit-log tampering.

### Transaction Integrity

The platform must detect or prevent:

- Multiple unauthorized primary active Deals for one Opportunity.
- Deal creation from the wrong Quotation.
- Commercial snapshot alteration.
- Vehicle or Inventory substitution without governance.
- Reservation and Allocation conflict.
- Payment counted before clearing.
- Funding counted before Confirmation.
- Contract signature treated as effectiveness.
- Delivery before readiness.
- Completion with outstanding requirements.
- Unwind without consequence handling.
- Replacement Deal without lineage.
- Deal status manipulation.

### Privacy and Retention

Deal retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Tax and accounting obligations.
- Contractual requirements.
- Payment and funding obligations.
- Registration requirements.
- Dispute requirements.
- Legal holds.
- Audit requirements.

Privacy actions must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Document stores.
- External systems where lawfully required.
- Backups according to policy.

Required commercial, contractual, financial, accounting, security, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Deal creation.
- Deal approval.
- Reservation.
- Allocation.
- Payment Commands.
- Funding requests.
- Contract handoff.
- Registration requests.
- Delivery authorization.
- Sale posting.
- Deal completion.
- Automated Customer communication.
- External write-back.
- AI Deal analysis.
- Deal export.
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
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Deal baseline.

Deal is the governed automotive transaction aggregate.

Deal remains separate from Opportunity, Quotation, Inventory Record, Trade-In, Finance Application, Financial Contract, Payment, funding, delivery, and accounting authority.

Vehicle Reservation and Allocation require authoritative Inventory Confirmation.

Payment authorization does not prove cleared funds.

Finance approval does not prove funding.

A fully signed Financial Contract is not automatically effective.

Delivery readiness does not prove Vehicle delivery.

Deal completion requires all configured authoritative completion conditions.

Completed or materially executed Deals require a governed unwind rather than ordinary cancellation.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
