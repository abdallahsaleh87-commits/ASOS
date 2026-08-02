# Deal

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Deal Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-02  

---

## 1. Object Purpose

### Business Purpose

The Deal Object represents one governed automotive commercial transaction accepted for controlled execution between a Customer and an authorized dealership legal entity.

A Deal normally begins after:

- A valid Customer and Opportunity exist.
- The Customer has made a verifiable commercial commitment.
- An accepted Quotation or another approved commercial basis exists.
- The dealership has accepted the transaction for controlled execution.
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
- Read-only Lender-funding projections and completion blockers.
- Compliance and document requirements.
- Vehicle registration and title requirements.
- Insurance requirements.
- Delivery preparation.
- Vehicle handover.
- Accounting and DMS posting.
- Sale Confirmation.
- Completion evaluation.
- Cancellation.
- Unwind.
- Replacement Deal.
- Commercial and operational reconciliation.

### Deal Domain Boundary

The Deal is the central automotive transaction aggregate.

It owns:

- Canonical transaction identity.
- Exact accepted commercial basis.
- Transaction participants.
- Transaction execution workflow.
- Completion-condition orchestration.
- Deal-level approval state.
- Deal-level readiness state.
- Deal-level funding requirement and completion blocker.
- Read-only funding projection required for execution and completion.
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
- Inventory Record creation or activation.
- Quotation terms.
- Trade-In appraisal.
- Trade-In acquisition.
- Lender underwriting Decision.
- Finance Application lifecycle.
- Financial Contract terms.
- Contract signature.
- Contract effectiveness or activation.
- Funding Command creation.
- Funding-request idempotency.
- Funding-request transmission.
- Lender or bank funding outcome.
- Customer Payment settlement.
- Registration.
- Insurance.
- Vehicle delivery.
- Accounting posting.
- Revenue recognition.

Those facts belong to their responsible Domain Services or configured authoritative external systems.

### Canonical Ownership Separation

```text
Finance Application
  = Applicant information, Consent, verification, Lender submissions,
    underwriting Decisions, selected offer, funding readiness,
    and governed Financial Contract handoff

Financial Contract
  = authoritative contractual terms, signature workflow,
    effectiveness, funding-request orchestration,
    External Confirmation tracking, reconciliation, and activation

Deal
  = governed automotive commercial transaction,
    transaction execution, completion gates,
    and non-owning funding projection

Payment
  = Customer or third-party payment transaction and settlement evidence

External Lender or Funding Authority
  = authoritative credit Decision and funding outcome
```

The Deal must not recreate a workflow owned by Finance Application, Financial Contract, Payment, Inventory, Trade-In, Delivery, Registration, or Accounting.

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

- New Quotation version.
- Customer reconfirmation.
- New approval.
- Finance re-underwriting.
- New Finance Application version.
- Financial Contract regeneration or amendment.
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

A Deal must not directly overwrite authoritative Inventory availability or stock-cycle state.

### Deal and Trade-In Separation

The Trade-In Domain Service owns:

- Trade-In Vehicle identity resolution.
- Ownership verification.
- Inspection.
- Appraisal.
- Actual cash value.
- Customer allowance support.
- Lien and payoff workflow.
- Equity calculation context.
- Acquisition workflow.
- Inventory-intake request and Confirmation tracking.

The Inventory Domain Service owns authoritative Inventory Record creation, activation, and stock-cycle state after valid acquisition.

The Deal preserves the exact accepted Trade-In and appraisal versions used in the transaction.

A Trade-In offer accepted by the Customer does not independently complete:

- Legal ownership transfer.
- Payoff settlement.
- Trade-In acquisition.
- Inventory intake.
- Inventory Record creation or activation.

### Deal and Finance Application Separation

The Finance Application Domain Service owns:

- Applicants.
- Finance Consent.
- Verification.
- Credit-bureau activity.
- Lender submissions.
- Lender Decisions.
- Customer-selected finance offer.
- Contract-readiness assessment.
- Funding-readiness assessment.
- Governed Financial Contract handoff.
- Read-only contract and funding projections after handoff.

The Deal may preserve:

- Finance Application reference.
- Exact application version.
- Selected Lender Decision reference and version.
- Funding-readiness projection.
- Finance-related completion blockers.
- Staleness and reconciliation references.

The Deal must not:

- Alter a Lender Decision.
- Represent a pending application as approved.
- Treat prequalification as approval.
- Create a Funding Command.
- Own funding-request idempotency.
- Publish authoritative funding outcomes.

### Deal and Financial Contract Separation

The Financial Contract Domain Service owns:

- Contract terms.
- Contract versions.
- Disclosures.
- Signatories.
- Signature evidence.
- Contract effectiveness.
- Funding-request workflow.
- Funding Command creation and transmission.
- Funding idempotency.
- External funding Confirmation tracking.
- Partial-funding and shortfall workflow.
- Funding failure, reversal, and reconciliation.
- Contract activation.
- Amendments.
- Termination.
- Settlement projections.

The Deal consumes authoritative Financial Contract facts and stores only the projections required for transaction execution and completion.

The Deal must distinguish:

```text
Contract handoff accepted
  ≠ Contract created

Contract created
  ≠ Contract signed

Contract fully signed
  ≠ Contract effective

Contract effective
  ≠ Funding requested

Funding requested
  ≠ Lender funded

Lender funded
  ≠ Contract activated

Contract activated
  ≠ Deal completed
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

The Deal owns Payment requirements and stores read-only Payment projections and references.

The Deal must distinguish:

```text
Payment required
  ≠ Payment requested

Payment requested
  ≠ Payment authorized

Payment authorized
  ≠ Payment cleared

Payment cleared
  ≠ Payment reconciled
```

Only cleared, applicable, and reconciled funds may satisfy a configured Deal Payment requirement.

### Deal and Funding Separation

Finance approval and funding readiness do not prove funding.

The Financial Contract Domain Service is the sole canonical owner of the funding-request mutation after a valid and effective Financial Contract exists.

The configured Lender, bank, or funding authority owns the authoritative funding outcome.

The Deal Domain Service owns only:

- Whether external funding is required for this transaction.
- The funding amount required by the accepted transaction.
- Read-only funding-status projection.
- Confirmed amount projection.
- Shortfall calculation.
- Funding-related delivery and completion blockers.
- Funding Confirmation references.
- Reconciliation and freshness indicators.
- Unwind triggers when a previously confirmed outcome is reversed.

The Deal must not:

- Create or transmit a Funding Command.
- Own a funding idempotency key.
- Expose a funding-request mutation.
- Publish authoritative funding-requested, funding-confirmed, funding-failed, or funding-reversed Events.
- Treat an API response, transport acknowledgement, provider receipt, or Payment instruction as proof of funding.
- Alter or reinterpret the authoritative external outcome.

The Deal may become transaction-ready for funding before funding is requested.

The Deal may become delivery-ready only when its configured funding requirement is satisfied by current authoritative evidence.

### Deal and Delivery Separation

The Deal coordinates delivery readiness.

The authoritative Delivery workflow owns:

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
- Delivery Confirmation.

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
- Finance Application.
- Financial Contract.
- Prices.
- Discounts.
- Incentives.
- Taxes.
- Fees.
- Optional products.
- Customer contribution.
- Funding requirement and projection.
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
| Inventory availability, Reservation, Allocation, and stock-cycle state | Inventory Domain Service or configured external authority |
| Trade-In appraisal and acquisition workflow | Trade-In Domain Service |
| Inventory Record creation after Trade-In acquisition | Inventory Domain Service |
| Lender Decision | Lender through Finance Application |
| Finance Application workflow and Financial Contract handoff | Finance Application Domain Service |
| Financial Contract, signature, funding workflow, and activation | Financial Contract Domain Service |
| Funding Command creation and idempotency | Financial Contract Domain Service |
| Authoritative Lender funding outcome | Lender, bank, or configured funding authority |
| Deal funding requirement, blocker, and non-owning projection | Deal Domain Service |
| Customer Payment settlement | Payment or banking authority |
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
- `funding_confirmation_reference`.
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

### Reservation Projection

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

Reservation Commands may be orchestrated by the Deal only when the configured Inventory contract delegates that request to Deal.

The Inventory Domain Service remains authoritative for the outcome.

### Allocation Projection

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
- `trade_in_inventory_intake_status_projection`.
- `trade_in_inventory_record_id_projection`.

### Finance Snapshot

- `finance_required`.
- `finance_application_id`.
- `finance_application_version`.
- `finance_application_status_projection`.
- `contract_handoff_status_projection`.
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
- `finance_funding_readiness_projection`.
- `finance_snapshot`.
- `finance_snapshot_hash`.

### Financial Contract Projection

- `financial_contract_required`.
- `financial_contract_id`.
- `financial_contract_version`.
- `financial_contract_status_projection`.
- `contract_signature_status_projection`.
- `contract_effectiveness_status_projection`.
- `contract_activation_status_projection`.
- `contract_signed_at_projection`.
- `contract_effective_at_projection`.
- `contract_activated_at_projection`.
- `signed_contract_document_hash`.
- `contract_projection_source_record_version`.
- `contract_projection_observed_at`.
- `contract_projection_freshness_status`.
- `contract_reconciliation_status_projection`.

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

- `payment_status_projection`.
- `deposit_status_projection`.
- `down_payment_status_projection`.
- `customer_payment_status_projection`.
- `total_payment_authorized_amount_projection`.
- `total_payment_captured_amount_projection`.
- `total_payment_cleared_amount_projection`.
- `total_payment_refunded_amount_projection`.
- `total_payment_reversed_amount_projection`.
- `total_payment_chargeback_amount_projection`.
- `net_customer_payment_amount_projection`.
- `payment_shortfall_amount`.
- `last_payment_confirmed_at_projection`.
- `payment_reconciliation_status_projection`.
- `payment_projection_source_record_version`.
- `payment_projection_observed_at`.
- `payment_projection_freshness_status`.
- `payment_references`.

### Funding Requirement and Read-Only Projection

- `funding_required`.
- `funding_amount_required`.
- `funding_currency_code`.
- `funding_requirement_source`.
- `funding_requirement_source_record_version`.
- `funding_requirement_evaluated_at`.
- `funding_completion_block_status`.
- `funding_completion_block_reasons`.
- `funding_status_projection`.
- `confirmed_funding_amount_projection`.
- `funding_shortfall_amount`.
- `funding_authority_reference`.
- `funding_confirmation_status_projection`.
- `funding_confirmation_reference`.
- `funding_confirmed_at_projection`.
- `funding_reversal_status_projection`.
- `funding_reversal_reference`.
- `funding_reconciliation_status_projection`.
- `funding_projection_source`.
- `funding_projection_source_record_id`.
- `funding_projection_source_record_version`.
- `funding_projection_observed_at`.
- `funding_projection_last_synced_at`.
- `funding_projection_freshness_status`.

The Deal must not store or own:

- Funding Command identifiers.
- Funding-request idempotency keys.
- Funding Command execution state.
- Authoritative funding transaction state.
- A locally decided funding outcome.

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

- `delivery_status_projection`.
- `delivery_reference`.
- `delivery_appointment_id`.
- `delivery_scheduled_at_projection`.
- `delivery_started_at_projection`.
- `vehicle_handover_at_projection`.
- `delivery_confirmed_at_projection`.
- `delivery_confirmation_status_projection`.
- `delivery_evidence_references`.
- `delivery_reconciliation_status_projection`.
- `delivery_projection_source_record_version`.
- `delivery_projection_freshness_status`.

### Sale and Accounting Projection

- `sale_posting_status_projection`.
- `sale_posting_reference`.
- `sale_posting_requested_at`.
- `sale_posting_confirmed_at_projection`.
- `sale_confirmation_status_projection`.
- `accounting_handoff_status_projection`.
- `accounting_reference`.
- `accounting_confirmation_status_projection`.
- `accounting_period_reference`.
- `revenue_recognition_status_projection`.
- `cost_of_sale_status_projection`.
- `tax_posting_status_projection`.
- `accounting_reconciliation_status_projection`.

### Profitability Projection

- `vehicle_cost_amount`.
- `reconditioning_cost_amount`.
- `accessory_cost_amount`.
- `optional_product_cost_amount`.
- `funding_cost_amount_projection`.
- `trade_in_cost_impact_amount`.
- `front_end_gross_amount`.
- `back_end_gross_amount`.
- `estimated_gross_profit_amount`.
- `estimated_gross_margin_percentage`.
- `confirmed_gross_profit_amount_projection`.
- `confirmed_gross_margin_percentage_projection`.
- `commissionable_gross_amount_projection`.
- `profitability_status`.
- `profitability_source`.
- `profitability_confirmed_at_projection`.

Estimated and confirmed profitability must remain distinguishable.

### Commission Projection

- `commission_calculation_status`.
- `commission_plan_id`.
- `commission_plan_version`.
- `commissionable_amount_projection`.
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
- `days_to_funding_confirmation`.
- `days_to_delivery`.
- `days_to_completion`.
- `document_completion_percentage`.
- `delivery_readiness_percentage`.
- `completion_requirement_percentage`.
- `payment_shortfall_amount`.
- `funding_shortfall_amount`.
- `estimated_total_receipts_amount`.
- `confirmed_total_receipts_amount_projection`.

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
| `status` | Enum | Yes | Deal workflow | Current aggregate transaction lifecycle state. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Defines ASOS or external authority over Deal workflow state. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Finance and Contract Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `finance_required` | Boolean | Yes | Deal structure | Whether Lender finance is required. |
| `finance_application_id` | UUID | Conditional | Canonical relationship | Finance Application supporting the Deal. |
| `finance_application_version` | Integer | Conditional | Finance Application | Exact immutable application version used. |
| `selected_lender_decision_id` | UUID | Conditional | Finance Application relationship | Exact selected Lender Decision. |
| `selected_lender_decision_version` | Integer | Conditional | Lender Decision projection | Exact selected Decision version. |
| `contract_handoff_status_projection` | Enum | No | Finance Application projection | Observed contracting-handoff status. |
| `financial_contract_id` | UUID | Conditional | Canonical relationship | Financial Contract supporting the transaction. |
| `financial_contract_version` | Integer | Conditional | Financial Contract projection | Observed Contract version. |
| `contract_signature_status_projection` | Enum | Yes | Financial Contract projection | Current signature state. |
| `contract_effectiveness_status_projection` | Enum | Yes | Financial Contract projection | Current legal effectiveness state. |
| `contract_activation_status_projection` | Enum | Yes | Financial Contract projection | Current activation state. |
| `contract_projection_freshness_status` | Enum | Yes | Projection metadata | Whether Contract data is current enough for dependent decisions. |

### Payment Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `total_customer_payment_required_amount` | Decimal | Yes | Deal requirement | Total required Customer funds. |
| `total_payment_cleared_amount_projection` | Decimal | Yes | Payment authority projection | Total cleared amount applicable to the Deal. |
| `net_customer_payment_amount_projection` | Decimal | Yes | Deterministic projection | Cleared amount less applicable refunds and reversals. |
| `payment_shortfall_amount` | Decimal | Yes | Deterministic calculation | Remaining Customer Payment requirement. |
| `payment_status_projection` | Enum | Yes | Payment projection | Aggregate observed Payment state. |
| `payment_reconciliation_status_projection` | Enum | Yes | Payment projection | Observed reconciliation state. |
| `payment_projection_freshness_status` | Enum | Yes | Projection metadata | Whether Payment data is current enough for dependent decisions. |

### Funding Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `funding_required` | Boolean | Yes | Deal structure | Whether external funding is required. |
| `funding_amount_required` | Decimal | Yes | Accepted transaction and Contract terms | Required external funding amount. |
| `funding_currency_code` | String | Conditional | Accepted transaction and Contract terms | Currency used by the requirement and projection. |
| `funding_completion_block_status` | Enum | Yes | Deal workflow | Whether funding currently blocks delivery or completion. |
| `funding_status_projection` | Enum | No | Financial Contract or external projection | Observed funding workflow and outcome status. |
| `confirmed_funding_amount_projection` | Decimal | No | Funding authority projection | Authoritatively confirmed amount observed by the Deal. |
| `funding_shortfall_amount` | Decimal | Yes | Deterministic calculation | Difference between required and confirmed funding. |
| `funding_confirmation_status_projection` | Enum | No | Financial Contract projection | Observed External Confirmation status. |
| `funding_confirmation_reference` | String | No | Funding authority or Confirmation adapter | Evidence reference supporting the observed outcome. |
| `funding_confirmed_at_projection` | Timestamp | No | Funding authority projection | Observed authoritative funding timestamp. |
| `funding_reversal_status_projection` | Enum | No | Financial Contract projection | Observed reversal state. |
| `funding_reconciliation_status_projection` | Enum | No | Financial Contract projection | Observed reconciliation state. |
| `funding_projection_source_record_version` | String | No | Projection metadata | Source version used for freshness and reconciliation. |
| `funding_projection_freshness_status` | Enum | Yes | Projection metadata | Whether funding data is current enough for dependent decisions. |

The Deal must not contain Funding Command identifiers or funding-request idempotency keys.

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

### DealPaymentProjectionStatus

- `UNKNOWN`
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
- `STALE`

### DealFundingProjectionStatus

- `UNKNOWN`
- `NOT_REQUIRED`
- `NOT_STARTED`
- `REQUIREMENTS_PENDING`
- `READY`
- `REQUEST_PENDING`
- `PENDING_CONFIRMATION`
- `PARTIALLY_CONFIRMED`
- `CONFIRMED`
- `SHORTFALL`
- `FAILED`
- `REVERSED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`
- `STALE`

This is a non-owning projection received from Financial Contract or the configured external funding authority.

### FundingCompletionBlockStatus

- `NOT_EVALUATED`
- `NOT_REQUIRED`
- `BLOCKING`
- `PARTIALLY_SATISFIED`
- `SATISFIED`
- `STALE`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### ContractProjectionStatus

- `UNKNOWN`
- `NOT_REQUIRED`
- `NOT_STARTED`
- `HANDOFF_PENDING`
- `CREATED`
- `SIGNATURE_PENDING`
- `PARTIALLY_SIGNED`
- `FULLY_SIGNED`
- `EFFECTIVENESS_PENDING`
- `EFFECTIVE`
- `FUNDING_PENDING`
- `ACTIVE`
- `VOIDED`
- `TERMINATED`
- `EXPIRED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`
- `STALE`

### DeliveryReadinessStatus

- `NOT_ASSESSED`
- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### DealDeliveryProjectionStatus

- `UNKNOWN`
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
- `STALE`

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

### ProjectionFreshnessStatus

- `CURRENT`
- `AGING`
- `STALE`
- `UNKNOWN`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

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
- Dealership, branch, legal entity, team, User, approver, and operational authority must belong to permitted scope.
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

- Deal must reference the exact accepted Quotation version.
- Accepted Quotation must be issued, current at acceptance, and valid under the accepted policy.
- Accepted document hash must match.
- Customer acceptance evidence must satisfy configured policy.
- Commercial values must match the accepted Quotation.
- Quotation acceptance must not be inferred from view, sentiment, or AI prediction.
- An expired, withdrawn, rejected, or superseded Quotation must not create a new Deal.
- A material commercial change requires a new Quotation version and Deal revalidation.

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

### Reservation and Allocation Rules

The Deal may initiate Reservation or Allocation requests only through the approved Inventory contract.

Each request requires:

- Eligible Deal.
- Eligible Inventory Record.
- Applicable Customer commitment.
- Current Inventory version.
- Required deposit or approval where applicable.
- Stable idempotency key.
- No incompatible active Reservation or Allocation.
- External Confirmation where Inventory authority is external.

The Deal must remain pending until authoritative Inventory Confirmation.

A request does not prove Reservation, Allocation, sale, transfer, or delivery.

### Trade-In Rules

When a Trade-In is included:

- Exact Trade-In record must be referenced.
- Exact appraisal and offer versions must be preserved.
- Trade-In acceptance must be valid.
- Ownership and payoff evidence must be sufficiently current.
- Equity must be calculated deterministically.
- Trade-In acquisition readiness must be evaluated.
- Inventory intake must remain distinct from authoritative Inventory Record creation and activation.
- Trade-In changes may require new Quotation and Deal revalidation.
- Deal completion must not falsely imply Trade-In acquisition or Inventory intake if either remains pending.

### Finance Rules

A financed Deal requires:

- Valid Finance Application.
- Exact immutable Finance Application version.
- Valid selected Lender Decision.
- Valid Customer selection.
- Current Lender Decision.
- Current approved finance terms.
- Satisfied or permitted outstanding conditions.
- Accepted Financial Contract handoff where required.
- Applicable Financial Contract.
- Current funding-readiness projection.

The Deal must not:

- Alter Lender terms.
- Mark finance approved from an AI score.
- Treat prequalification as approval.
- Treat Contract handoff as signature.
- Treat approval or readiness as funding.
- Create a Funding Command.
- Own a funding-request idempotency key.

### Financial Contract Rules

When a Financial Contract is required:

- Deal must reference the correct Contract.
- Contract Customer, Deal, Vehicle, Quotation, Finance Application, and finance terms must match.
- Required signatures must be authoritative.
- Contract effectiveness must be evaluated separately.
- Funding workflow must remain owned by Financial Contract.
- Contract activation must remain separately projected.
- Contract mismatch must block delivery and completion.
- A fully signed Contract must not be treated as effective unless effectiveness is confirmed.
- A confirmed funding outcome must not be treated as Contract activation unless all activation conditions are satisfied.

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

Customer Payment readiness requires the configured cleared and reconciled amount.

Duplicate Payment Events must not create duplicate financial effects.

### Funding Projection Rules

The Deal is not the funding-request owner.

The Deal may evaluate funding completion only from:

- Current Financial Contract projection.
- Current external funding authority projection.
- Accepted External Confirmation.
- Matching Contract and Deal references.
- Confirmed amount.
- Currency.
- Authoritative timestamp.
- Source authority.
- Source record version where available.
- Reconciliation result.
- Freshness status.

The Deal must calculate:

```text
funding_shortfall_amount
  = max(funding_amount_required - confirmed_funding_amount_projection, 0)
```

A financed Deal must not satisfy its funding completion requirement until:

- Authoritative funding Confirmation exists.
- Confirmed amount is known.
- Currency matches.
- Funding reference is known.
- Funding timestamp is known.
- Projection matches the Financial Contract and Deal.
- Material shortfall is resolved.
- Reconciliation is current.
- Projection freshness is acceptable.
- No reversal or dispute is active.

Partial funding must remain explicit.

Funding failure must remain explicit.

Funding reversal must trigger:

- Deal block.
- Delivery review.
- Accounting review.
- Contract review.
- Human escalation.
- Unwind assessment.
- Reconciliation.

The Deal must not transmit or retry the underlying funding request.

### Approval Rules

Deal approval may be required for:

- Discount exception.
- Margin exception.
- Pricing override.
- Trade-In over-allowance.
- Negative-equity treatment.
- Payment exception.
- Funding completion exception.
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

A Deal approval must not override an external authoritative funding result.

### Delivery Readiness Rules

Delivery readiness must be calculated deterministically from configured requirements.

Possible requirements include:

- Vehicle allocated.
- Vehicle physically ready.
- Required Payment cleared.
- Required funding confirmed.
- Financial Contract effective or active as policy requires.
- Required documents complete.
- Required compliance cleared.
- Registration ready.
- Insurance verified.
- Trade-In conditions satisfied.
- Customer identity valid.
- Delivery Appointment available.
- No legal or operational hold.

`READY` must not be set solely from a User-interface action or AI Recommendation.

### Completion Rules

A Deal may become `COMPLETED` only when all configured completion requirements are satisfied.

These may include:

- Valid Customer commitment.
- Current accepted commercial snapshot.
- Required Vehicle Reservation and Allocation.
- Valid and effective Financial Contract where required.
- Required Contract activation where required.
- Required Customer Payment cleared.
- Required Lender funding confirmed through current authoritative projection.
- Required Trade-In conditions completed or governed.
- Required compliance cleared.
- Required documents completed.
- Required registration and title conditions.
- Required insurance.
- Authoritative delivery Confirmation where required.
- Authoritative sale posting where required.
- Accounting reconciliation at the required completion level.
- No material dispute, reversal, stale projection, or reconciliation block.
- Completion Decision or External Confirmation where required.
- `completed_at`.

The Deal completion workflow may evaluate funding evidence but must not create or alter the funding transaction.

### Cancellation and Unwind Rules

Cancellation normally applies before irreversible or effective transaction outcomes.

An effective, delivered, funded, posted, or completed Deal may require unwind rather than ordinary cancellation.

Unwind assessment must include:

- Payment reversals and refunds.
- Financial Contract status.
- Funding reversal or recovery.
- Inventory Reservation and Allocation.
- Trade-In acquisition and payoff.
- Registration and title.
- Delivery.
- Sale posting.
- Accounting and tax.
- Customer communication.
- Legal obligations.

AI must not independently authorize or execute cancellation or unwind.

### Concurrency and Idempotency Rules

- Every Deal mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Deal creation must support idempotency.
- Replacement Deal creation must support idempotency.
- Reservation requests must support idempotency.
- Allocation requests must support idempotency.
- Registration requests must support idempotency.
- Sale-posting requests must support idempotency.
- Completion processing must support idempotency.
- Cancellation and unwind requests must support idempotency.
- Event Consumers must prevent duplicate effects using `event_id`.
- The Deal must not create or own funding-request idempotency.

Duplicate retries must not create duplicate:

- Deals.
- Reservations.
- Allocations.
- Payment requirements.
- Registration requests.
- Delivery outcomes.
- Sale postings.
- Completion outcomes.
- Cancellation records.
- Unwind records.
- Replacement Deals.

### External Authority Rules

When an external CRM, DMS, Inventory, Payment, funding, registration, delivery, or accounting system is authoritative:

- ASOS must issue approved Commands only from the owning workflow.
- Retryable Commands must use the owning workflow's `idempotency_key`.
- Local state must remain pending until External Confirmation.
- Transport success does not equal business completion.
- Conflicting data must create reconciliation.
- Higher-authority evidence must not be silently overwritten.
- Missing Confirmation must trigger retry, polling, timeout, reconciliation, or Human escalation by the owning workflow.

For funding, the owning workflow is Financial Contract, not Deal.

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
- Funding shortfall, failure, reversal, or stale projection.
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

Reservation, Allocation, Payment, Finance Application, Financial Contract, funding, registration, delivery, sale-posting, and accounting states are governed as separate projections.

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

- Delivery-readiness evaluation.
- All configured readiness requirements satisfied.
- Required Payment condition.
- Required funding condition based on current authoritative projection.
- Required Contract condition.
- Required Vehicle and Inventory conditions.
- Required documents and compliance.
- Required registration and insurance.
- No blocking conflict, reversal, stale projection, or reconciliation requirement.

### Entering COMPLETION_PENDING

Requires:

- Authoritative delivery or applicable fulfillment outcome.
- Required sale-posting workflow.
- Accounting handoff.
- Completion requirements evaluated.
- Outstanding External Confirmations identified.
- Funding projection current and reconciled where funding is required.

### Entering COMPLETED

Requires:

- All configured completion requirements satisfied.
- Authoritative outcome evidence.
- Required external systems reconciled.
- No unresolved material shortfall, reversal, stale projection, dispute, or reconciliation block.
- Completion Decision where required.
- `completed_at`.

`COMPLETED` is a Deal outcome.

It does not replace Financial Contract completion, Payment settlement, funding authority, Delivery authority, or Accounting authority.

### Entering UNWIND_PENDING

Requires:

- Materially executed, effective, funded, delivered, posted, or completed transaction requiring reversal.
- Authorized unwind request.
- Impact assessment.
- Legal, financial, accounting, Inventory, and Customer plan.
- Required Human Decision.

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
- New immutable Event.
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
- Deal preserves issued and accepted document hashes.
- Deal must not alter Quotation terms.
- Material changes require Quotation and Deal governance.

### Vehicle and Inventory Record

- Every Vehicle transaction references one Vehicle.
- A physical Vehicle Deal references one Inventory Record.
- Vehicle owns identity and specifications.
- Inventory owns availability, Reservation, Allocation, sale, transfer, Inventory Record activation, and stock-cycle state.
- Deal requests and consumes authoritative Inventory outcomes.

### Trade-In

- Deal may reference one or more permitted Trade-In records.
- Trade-In owns appraisal, payoff, ownership transfer, acquisition, and Inventory-intake request tracking.
- Inventory owns authoritative Inventory Record creation and activation.
- Deal preserves exact appraisal and offer versions.

### Finance Application

- A financed Deal references one Finance Application.
- It preserves the exact application version and selected Lender Decision.
- Finance Application remains authoritative for underwriting, selected offer, readiness, and Contract handoff.
- Deal consumes non-owning projections.

### Financial Contract

- Deal may reference one current applicable Financial Contract.
- Financial Contract owns contractual terms, signatures, effectiveness, funding-request workflow, reconciliation, activation, amendments, and legal lifecycle.
- Deal consumes authoritative Contract and funding-workflow facts.

### Payment

- Deal may reference multiple Payment transactions.
- Payment authority owns authorization, clearing, refund, reversal, chargeback, and settlement.
- Deal owns transaction-level Payment requirements and projections.

### Funding Authority

- Financial Contract owns the funding-request workflow.
- External Lender, bank, or configured authority owns the funding outcome.
- Deal stores only required transaction projections, blockers, and evidence references.
- Deal must not create a funding transaction.

### Delivery Workflow

- Delivery workflow owns delivery schedule, release authorization, handover checklist, Customer identity at handover, Vehicle condition at handover, delivery evidence, and Delivery Confirmation.
- Deal owns delivery requirements and completion gating.

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

### Supporting Child Records

Deal may own or govern:

- Deal snapshots.
- Approval requests.
- Approval Decisions.
- Completion requirements.
- Reservation requests where delegated.
- Allocation requests where delegated.
- Payment requirements.
- Funding completion requirements.
- Funding projections.
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

Deal must not own Funding Commands or funding-request execution records.

---

## 8. Domain Events

The Canonical Event Catalog is authoritative for final:

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
- Deal Reservation Confirmation observed.
- Deal Reservation rejected.
- Deal Reservation expired.
- Deal Reservation released.
- Deal Allocation requested.
- Deal Allocation Confirmation observed.
- Deal Allocation rejected.
- Deal Allocation released.
- Deal Inventory conflict detected.
- Deal Inventory reconciliation required.

Inventory Domain Service publishes authoritative Reservation, Allocation, activation, and stock-cycle facts.

### Contract Event Concepts

- Deal Contract handoff status observed.
- Deal Financial Contract created projection updated.
- Deal Contract signature projection updated.
- Deal Contract effectiveness projection updated.
- Deal Contract activation projection updated.
- Deal Contract mismatch detected.
- Deal Contract reconciliation required.

Financial Contract Domain Service publishes authoritative Contract lifecycle facts.

### Payment Event Concepts

- Deal Payment requirement created.
- Deal Payment projection updated.
- Deal Payment shortfall detected.
- Deal Payment reversal observed.
- Deal Payment chargeback observed.
- Deal Payment reconciliation required.

Payment or banking authority publishes authoritative Payment facts.

### Funding Projection Event Concepts

- Deal funding requirement evaluated.
- Deal funding projection updated.
- Deal funding Confirmation observed.
- Deal funding shortfall detected.
- Deal funding failure observed.
- Deal funding reversal observed.
- Deal funding projection became stale.
- Deal funding completion blocker added.
- Deal funding completion blocker removed.
- Deal funding reconciliation required.
- Deal unwind assessment required after funding reversal.

The Deal Domain Service must not publish authoritative:

- Funding request created.
- Funding Command sent.
- Funding confirmed.
- Funding failed.
- Funding reversed.

The Financial Contract Domain Service publishes accepted funding-workflow facts.

The configured Lender, bank, or funding authority remains authoritative for the external outcome.

### Delivery Event Concepts

- Deal delivery readiness evaluated.
- Deal became ready for delivery.
- Deal delivery scheduling requested.
- Deal delivery projection updated.
- Deal Vehicle handover projection observed.
- Deal delivery Confirmation observed.
- Deal delivery failed.
- Deal delivery disputed.
- Deal delivery reconciliation required.

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
- Finance Application publishes accepted underwriting, selected-offer, readiness, and handoff facts.
- Financial Contract publishes accepted Contract and funding-workflow facts.
- Payment, registration, delivery, DMS, accounting, and external funding integrations publish normalized observations through their owning services.
- AI Agents may publish Agent-run, analysis, prediction, anomaly, or Recommendation Events.
- AI Agents must not publish authoritative Reservation, Allocation, Payment, funding, signature, delivery, accounting, completion, cancellation, or unwind Events merely because they predicted or recommended an outcome.

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

Corrections, cancellation, unwind, replacement, Payment reversal, funding reversal observation, delivery reversal, and accounting reversal must use new Events linked to prior Events.

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
- Funding-projection staleness detection.
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
- Create or transmit a Funding Command.
- Confirm Lender funding.
- Alter a funding projection without authoritative evidence.
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
- Funding-readiness review.
- Delivery preparation.
- Completion review.

The deterministic Policy Engine and authoritative source data must validate all requirements.

AI readiness must not become authoritative Deal state by itself.

AI must not recommend or invoke a Deal-owned funding request because Deal does not own that mutation.

### Payment and Funding Analysis

AI may identify possible:

- Payment shortfall.
- Funding delay.
- Funding shortfall.
- Funding-projection staleness.
- Reconciliation gap.
- Missing Confirmation.
- Inconsistent amount.
- Reversal impact.

AI must not treat:

- Provider acknowledgement.
- Bank instruction.
- Pending transfer.
- Customer statement.
- Predicted funding.

as authoritative cleared funds.

### Customer-Facing Drafting

AI may draft Deal-related communication only when:

- Purpose is permitted.
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

Controlled Deal status updates, document requests, Payment reminders, and delivery-coordination messages may proceed through:

- Explicit Human Approval; or
- Applicable pre-approved automation policy.

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
- Funding completion exception.
- Delivery authorization.
- Completion.
- Cancellation after material execution.
- Unwind.
- Replacement Deal.
- Accounting correction.
- Dispute resolution.

The funding request itself remains owned by Financial Contract.

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
POST   /api/v1/deals/{deal_id}/payment-reconciliation-review-requests
POST   /api/v1/deals/{deal_id}/funding-requirement-evaluations
POST   /api/v1/deals/{deal_id}/funding-projection-refresh-requests
POST   /api/v1/deals/{deal_id}/funding-reconciliation-review-requests

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

The Deal API must not expose a funding-request mutation.

Funding-request mutations belong only to the Financial Contract API.

### Example Funding Projection

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "financial_contract_id": "fb48670c-7650-4a7e-bace-69f8db34b9e0",
  "funding_required": true,
  "funding_amount_required": 1650000,
  "funding_currency_code": "EGP",
  "funding_status_projection": "PENDING_CONFIRMATION",
  "confirmed_funding_amount_projection": 0,
  "funding_shortfall_amount": 1650000,
  "funding_confirmation_status_projection": "PENDING",
  "funding_reconciliation_status_projection": "PENDING",
  "funding_projection_source": "FINANCIAL_CONTRACT",
  "funding_projection_source_record_version": "18",
  "funding_projection_freshness_status": "CURRENT",
  "funding_completion_block_status": "BLOCKING",
  "record_version": 14
}
```

This response is a projection.

It does not create, retry, confirm, fail, or reverse a funding request.

### Mutation Requirements

Every Deal mutation must enforce:

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
- Contract, Payment, funding projection, compliance, and delivery checks.
- Required Human Decision.
- Idempotency where required.
- Audit recording.
- Event publication after accepted state change.
- External Confirmation tracking where applicable.

A Deal mutation must reject any attempt to set:

- Funding Command identifiers.
- Funding-request idempotency keys.
- Authoritative funding status.
- Authoritative funded amount.
- Authoritative funding timestamp.
- Authoritative reversal outcome.

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
- `CONTRACT_NOT_ACTIVE`
- `PAYMENT_SHORTFALL`
- `PAYMENT_NOT_CLEARED`
- `FUNDING_PROJECTION_REQUIRED`
- `FUNDING_PROJECTION_STALE`
- `FUNDING_CONFIRMATION_PENDING`
- `FUNDING_SHORTFALL`
- `FUNDING_REVERSED`
- `FUNDING_REQUEST_NOT_OWNED_BY_DEAL`
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
- Payment authority.
- Financial Contract funding-workflow ownership.
- Funding projection-only behaviour in Deal.
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
deal_funding_completion_requirements
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

The Deal database must not contain a Deal-owned funding-command or funding-request execution table.

The canonical funding workflow belongs to:

```text
financial_contract_funding_workflows
```

### Deals Table

The `deals` table should contain:

- Canonical identifiers.
- Deal series.
- Tenant and organizational scope.
- Opportunity and Customer.
- Current Quotation, Vehicle, Inventory, Trade-In, Finance Application, Financial Contract, and Delivery references.
- Current aggregate lifecycle state.
- Current approval state.
- Current Reservation and Allocation projections.
- Current Payment projection.
- Current funding requirement and projection.
- Current compliance and document projections.
- Current registration, insurance, delivery, sale-posting, accounting, and completion projections.
- Cancellation, unwind, replacement, and dispute projections.
- Data-quality and conflict state.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Historical and repeating data must remain in child or history tables.

### Funding Completion Requirement Storage

`deal_funding_completion_requirements` should preserve:

- Requirement identifier.
- `tenant_id`.
- Deal.
- Financial Contract.
- Finance Application.
- Selected Lender Decision.
- Required amount.
- Currency.
- Requirement source.
- Source record version.
- Completion-block status.
- Block reasons.
- Evaluation timestamp.
- Applied policy.
- Related Events.

It must not store:

- Funding Command.
- Funding idempotency key.
- Request-transmission state.
- Authoritative outcome.

### Funding Projection Storage

`deal_funding_projections` should preserve:

- Projection identifier.
- `tenant_id`.
- Deal.
- Financial Contract.
- External funding authority reference.
- Observed funding status.
- Confirmed amount.
- Currency.
- Confirmation reference.
- Confirmation timestamp.
- Shortfall.
- Failure projection.
- Reversal projection.
- Reconciliation status.
- Source system.
- Source record identifier.
- Source record version.
- Observation timestamp.
- Last synchronization timestamp.
- Freshness status.
- Related Events.

Projection rows must be traceable to Financial Contract or the configured external funding authority.

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

idx_deals_payment_projection
  (tenant_id, payment_status_projection)

idx_deals_funding_projection
  (tenant_id, funding_status_projection)

idx_deals_funding_block
  (tenant_id, funding_completion_block_status)

idx_deals_delivery
  (tenant_id, delivery_status_projection)

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

A partial unique constraint or equivalent service control should normally enforce one primary active Deal per Opportunity.

### Tenant Protection

Every Deal-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Audit Storage

Deal audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw financial, identity, Contract, Payment, funding, and dispute values where full retention is unnecessary.

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
- Funding projection.
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

### Payment and Funding Protection

Payment and funding data must use:

- Encryption.
- Tokenized account references where applicable.
- Field-level authorization.
- Controlled instructions.
- Idempotency protection in the owning workflow.
- External Confirmation.
- Reconciliation.
- Audit logging.

Deal must not store funding-request credentials, raw banking instructions, or a funding idempotency key.

Payment or funding instructions must never be copied into:

- Prompts.
- Ordinary Logs.
- General-purpose embeddings.
- Public documents.
- Unapproved Customer messages.

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
- Funding projections.
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
- Idempotency key where Deal owns the request.
- Audit evidence.
- External Confirmation requirement.

The Deal Domain Service must reject a funding Command request.

Funding Commands must originate only from Financial Contract Domain Service.

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
- Command where applicable.
- Idempotency key where applicable.
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
- Attempted Deal-owned Funding Command.
- False funding Confirmation.
- Funding projection tampering.
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
- Funding counted before accepted Confirmation.
- Deal-created Funding Commands.
- Contract signature treated as effectiveness.
- Funding Confirmation treated as Contract activation without policy.
- Delivery before readiness.
- Completion with outstanding requirements.
- Unwind without consequence handling.
- Replacement Deal without lineage.
- Deal status manipulation.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Deal creation.
- Deal approval.
- Reservation.
- Allocation.
- Payment-related Deal requests.
- Funding-projection ingestion.
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

Deal owns transaction execution, completion gates, cancellation, unwind, and non-owning projections required to complete the transaction.

Deal remains separate from Opportunity, Quotation, Inventory Record, Trade-In, Finance Application, Financial Contract, Payment, external funding authority, Delivery, and Accounting authority.

Finance Application owns underwriting, selected-offer, readiness, and Financial Contract handoff.

Financial Contract owns the funding-request workflow, Funding Commands, idempotency, External Confirmation tracking, partial-funding, failure, reversal, reconciliation, and Contract activation.

The configured Lender, bank, or funding authority owns the authoritative funding outcome.

Deal stores only the funding requirement, read-only funding projection, completion blocker, Confirmation references, freshness, and reconciliation state.

Deal APIs must not expose a funding-request mutation.

Vehicle Reservation and Allocation require authoritative Inventory Confirmation.

Trade-In requests and tracks Inventory intake; Inventory owns authoritative Inventory Record creation, activation, and stock-cycle state.

Payment authorization does not prove cleared funds.

Finance approval does not prove funding.

A fully signed Financial Contract is not automatically effective.

Funding Confirmation does not automatically prove Contract activation or Deal completion.

Delivery readiness does not prove Vehicle delivery.

Deal completion requires all configured authoritative completion conditions.

Completed or materially executed Deals require a governed unwind rather than ordinary cancellation.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
