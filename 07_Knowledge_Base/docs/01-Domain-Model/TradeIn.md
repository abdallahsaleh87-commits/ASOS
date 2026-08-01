# Trade-In

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Trade-In Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Trade-In Object represents one governed evaluation and acquisition workflow for a Customer-controlled Vehicle offered to a dealership as part of an automotive commercial journey.

A Trade-In may support:

- Vehicle replacement.
- New Vehicle purchase.
- Used Vehicle purchase.
- Cash transaction.
- Finance-supported transaction.
- Lease replacement.
- Fleet replacement.
- Direct dealership acquisition.
- Another approved automotive transaction.

The Trade-In provides a controlled process for:

- Capturing the Customer-provided Vehicle information.
- Resolving or creating the canonical Vehicle identity.
- Verifying Vehicle identity.
- Verifying Customer authority to offer the Vehicle.
- Recording ownership and registration evidence.
- Detecting lien, finance, title, theft, legal, or compliance restrictions.
- Scheduling and completing inspection.
- Recording physical, mechanical, cosmetic, and documentary condition.
- Obtaining market and wholesale valuation evidence.
- Recording Customer value expectations.
- Producing appraisal Recommendations.
- Approving actual cash value.
- Approving Customer-facing Trade-In allowance.
- Verifying lender payoff.
- Calculating positive or negative equity.
- Supporting Quotation and Deal calculations.
- Obtaining Customer acceptance.
- Completing legal acquisition.
- Transferring physical possession.
- Requesting and tracking dealership Inventory intake after valid acquisition.
- Receiving and reconciling the authoritative Inventory Record reference.
- Reconciling accounting, title, payoff, and Inventory intake.

### Trade-In, Vehicle, and Inventory Separation

The following boundaries must remain explicit:

```text
Vehicle
  = canonical physical Vehicle identity and technical specification

Trade-In
  = appraisal, ownership-verification, payoff, offer, and acquisition workflow

Inventory Record
  = dealership-specific stock and commercial Inventory context
```

The Trade-In may initially contain submitted Vehicle information before identity resolution.

After identity resolution:

- `vehicle_id` must reference the canonical Vehicle.
- Vehicle identity and technical specification remain governed by Vehicle.
- Inspection and appraisal findings remain governed by Trade-In.
- Dealership stock status remains governed by Inventory Record.

A Customer-controlled Vehicle does not become dealership Inventory merely because:

- A Trade-In record exists.
- An inspection occurred.
- An appraisal was approved.
- A Customer accepted an allowance.
- A Quotation includes the Trade-In.
- A Deal was created.
- A payoff Command was sent.

The Vehicle becomes active dealership Inventory only after:

- Required legal acquisition conditions are completed.
- Required ownership-transfer evidence exists.
- Required lien or payoff conditions are satisfied.
- Physical possession is accepted.
- Inventory intake is approved.
- A valid Inventory Record is created or activated.
- Required External Confirmations are received.

### Trade-In and Quotation Separation

The Trade-In owns:

- Vehicle appraisal.
- Actual cash value.
- Customer allowance authority.
- Payoff evidence.
- Equity calculations.
- Ownership and acquisition readiness.

The Quotation owns the Customer-facing commercial offer that includes the approved Trade-In projection.

A Trade-In appraisal does not create a Quotation.

A Quotation must preserve the exact Trade-In appraisal version used.

A change to appraisal, payoff, allowance, or equity after Quotation issue may require a new Quotation version.

### Trade-In and Deal Separation

The Trade-In manages appraisal and acquisition readiness.

The Deal manages the governed Customer transaction.

Customer acceptance of a Trade-In offer does not independently prove:

- Deal completion.
- Contract signature.
- Payment.
- Payoff settlement.
- Ownership transfer.
- Vehicle acquisition.
- Inventory intake.
- Accounting completion.

The Trade-In may enter acquisition processing only through an approved Deal or another explicitly governed acquisition workflow.

### Appraisal and Customer Allowance Separation

The Trade-In must preserve separate values for:

```text
customer_expected_value_amount
market_reference_value_amount
wholesale_reference_value_amount
auction_reference_value_amount
estimated_reconditioning_amount
actual_cash_value_amount
trade_in_allowance_amount
over_allowance_amount
verified_payoff_amount
positive_equity_amount
negative_equity_amount
final_acquisition_cost_amount
```

`actual_cash_value_amount` represents the approved dealership acquisition valuation before Customer-facing allowance adjustments.

`trade_in_allowance_amount` represents the amount credited to the Customer in the commercial transaction.

`over_allowance_amount` represents any approved allowance above actual cash value.

These values must not be treated as interchangeable.

### Appraisal Versioning

A Trade-In may contain multiple appraisal versions over time.

An appraisal version may change because of:

- New inspection findings.
- Mileage change.
- Condition change.
- Market movement.
- New history evidence.
- Payoff change.
- Ownership evidence change.
- Title or lien change.
- Reinspection.
- Approval revision.
- Expiration.
- Dispute resolution.

Each appraisal version must preserve:

- `appraisal_id`.
- `appraisal_version`.
- Input evidence.
- Inspection version.
- Vehicle snapshot.
- Valuation sources.
- Calculation snapshot.
- Approval evidence.
- Validity period.
- Customer-facing allowance.
- Status.
- Supersession relationship.

An approved, presented, accepted, expired, rejected, or superseded appraisal version must remain historically immutable.

### System Purpose

The Trade-In Object provides canonical Trade-In workflow context to:

- Customer workflows.
- Opportunity workflows.
- Appointment workflows.
- Quotation workflows.
- Finance Application workflows.
- Deal workflows.
- Payment and payoff workflows.
- Vehicle workflows.
- Inventory intake workflows.
- Accounting integrations.
- Title and registration integrations.
- Market Intelligence.
- AI Agents.
- Analytics.
- Audit and compliance services.

The Trade-In Object may contain:

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
| Vehicle identity and specifications | Vehicle Domain Service |
| Submitted Vehicle information | Customer or intake source |
| Ownership and registration evidence | Government, legal, Customer, or approved verification source |
| Lien and payoff information | Lender or approved finance authority |
| Inspection findings | Authorized Inspector or approved inspection provider |
| Market reference values | Market Intelligence or approved valuation provider |
| Actual cash value | Authorized Trade-In appraisal authority |
| Customer-facing allowance | Authorized commercial authority |
| Equity calculations | Approved deterministic calculation service |
| Canonical Trade-In workflow | Trade-In Domain Service |
| Customer acceptance | Customer evidence or approved provider |
| Deal transaction | Deal Domain Service |
| Legal acquisition completion | Approved legal, title, Deal, and external authorities |
| Inventory stock context | Inventory Domain Service |
| Accounting outcome | Configured accounting or DMS authority |
| Predictions and Recommendations | Derived Intelligence |
| External operation completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `trade_in_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `department_id`.
- `responsible_team_id`.
- `responsible_user_id`.
- `assigned_appraiser_id`.
- `assigned_inspector_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `customer_id`.
- `lead_id`.
- `qualified_lead_id`.
- `opportunity_id`.
- `appointment_id`.
- `vehicle_id`.
- `quotation_ids`.
- `active_quotation_id`.
- `finance_application_id`.
- `deal_id`.
- `financial_contract_id`.
- `payment_id`.
- `inventory_record_id`.
- `primary_interaction_id`.
- `follow_up_task_id`.

### Trade-In Identity

- `trade_in_number`.
- `trade_in_type`.
- `status`.
- `priority`.
- `workflow_authority_mode`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Submitted Vehicle Information

- `submitted_vin`.
- `submitted_registration_number`.
- `submitted_make`.
- `submitted_model`.
- `submitted_trim`.
- `submitted_model_year`.
- `submitted_body_type`.
- `submitted_exterior_color`.
- `submitted_fuel_type`.
- `submitted_transmission`.
- `submitted_drivetrain`.
- `submitted_odometer_value`.
- `submitted_odometer_unit`.
- `submitted_country_of_registration`.
- `submitted_vehicle_notes`.

Submitted Vehicle information remains unverified until accepted through Vehicle identity resolution and evidence workflows.

### Vehicle Identity Resolution

- `vehicle_id`.
- `vehicle_identity_resolution_status`.
- `vin_verification_status`.
- `vin_evidence_references`.
- `registration_verification_status`.
- `registration_evidence_references`.
- `vehicle_history_status`.
- `vehicle_history_provider_references`.
- `vehicle_snapshot`.
- `identity_conflict_references`.
- `identity_verified_at`.
- `identity_verified_by_actor_id`.

### Customer Authority and Ownership

- `registered_owner_name_projection`.
- `registered_owner_identifier_reference`.
- `customer_authority_type`.
- `customer_authority_status`.
- `ownership_verification_status`.
- `ownership_document_status`.
- `registration_document_status`.
- `title_status`.
- `title_number_reference`.
- `title_evidence_references`.
- `ownership_evidence_references`.
- `ownership_verified_at`.
- `ownership_verified_by_actor_id`.
- `co_owner_status`.
- `co_owner_approval_status`.
- `power_of_attorney_status`.
- `estate_or_company_authority_status`.

### Lien, Finance, and Payoff

- `lien_status`.
- `lienholder_references`.
- `multiple_lien_status`.
- `payoff_required`.
- `payoff_verification_status`.
- `payoff_verification_id`.
- `verified_payoff_amount`.
- `payoff_currency_code`.
- `payoff_statement_date`.
- `payoff_valid_until`.
- `payoff_daily_interest_amount`.
- `payoff_provider_reference`.
- `payoff_evidence_references`.
- `payoff_verified_at`.
- `payoff_settlement_status`.
- `payoff_settlement_reference`.
- `lien_release_status`.
- `lien_release_evidence_references`.

Sensitive lender account information must remain tokenized, masked, or referenced rather than copied unnecessarily.

### Legal and Compliance Context

- `theft_check_status`.
- `fraud_review_status`.
- `sanctions_review_status`.
- `legal_hold_status`.
- `export_restriction_status`.
- `import_document_status`.
- `salvage_status`.
- `rebuilt_status`.
- `flood_damage_status`.
- `structural_damage_status`.
- `recall_status`.
- `compliance_block_status`.
- `compliance_block_reasons`.
- `legal_evidence_references`.

### Inspection Scheduling

- `inspection_required`.
- `inspection_status`.
- `inspection_type`.
- `inspection_appointment_id`.
- `inspection_location_id`.
- `inspection_scheduled_at`.
- `inspection_started_at`.
- `inspection_completed_at`.
- `inspection_version`.
- `remote_inspection_policy_id`.
- `inspection_exception_decision_id`.

### Inspection Findings

- `overall_condition`.
- `mechanical_condition`.
- `engine_condition`.
- `transmission_condition`.
- `drivetrain_condition`.
- `body_condition`.
- `paint_condition`.
- `interior_condition`.
- `tire_condition`.
- `wheel_condition`.
- `glass_condition`.
- `electrical_condition`.
- `air_conditioning_status`.
- `warning_lights_status`.
- `fluid_leak_status`.
- `keys_available_count`.
- `service_history_status`.
- `accident_history_status`.
- `odometer_verification_status`.
- `diagnostic_scan_status`.
- `road_test_status`.
- `inspection_notes`.
- `damage_items`.
- `missing_items`.
- `required_repairs`.
- `recommended_repairs`.
- `inspection_media_references`.
- `inspection_report_reference`.
- `inspection_snapshot_hash`.

### Odometer Context

- `reported_odometer_value`.
- `reported_odometer_unit`.
- `inspected_odometer_value`.
- `inspected_odometer_unit`.
- `normalized_odometer_kilometers`.
- `odometer_verification_status`.
- `odometer_evidence_references`.
- `odometer_discrepancy_reason`.
- `odometer_verified_at`.

### Appraisal Management

- `current_appraisal_id`.
- `current_appraisal_version`.
- `appraisal_status`.
- `appraisal_method`.
- `appraisal_started_at`.
- `appraised_at`.
- `appraisal_expires_at`.
- `appraisal_validity_status`.
- `appraisal_policy_id`.
- `appraisal_policy_version`.
- `superseded_appraisal_id`.
- `appraisal_revalidation_required`.

### Valuation Inputs

- `customer_expected_value_amount`.
- `market_reference_value_amount`.
- `wholesale_reference_value_amount`.
- `auction_reference_value_amount`.
- `retail_reference_value_amount`.
- `estimated_reconditioning_amount`.
- `estimated_transport_amount`.
- `estimated_title_and_registration_amount`.
- `estimated_holding_cost_amount`.
- `estimated_disposal_cost_amount`.
- `market_adjustment_amount`.
- `condition_adjustment_amount`.
- `mileage_adjustment_amount`.
- `history_adjustment_amount`.
- `regional_adjustment_amount`.
- `risk_adjustment_amount`.
- `currency_code`.

### Approved Valuation

- `proposed_actual_cash_value_amount`.
- `actual_cash_value_amount`.
- `proposed_trade_in_allowance_amount`.
- `trade_in_allowance_amount`.
- `over_allowance_amount`.
- `verified_payoff_amount`.
- `positive_equity_amount`.
- `negative_equity_amount`.
- `final_acquisition_cost_amount`.
- `valuation_calculation_reference`.
- `valuation_snapshot`.
- `valuation_snapshot_hash`.

### Valuation Evidence

- `valuation_source_references`.
- `market_comparable_references`.
- `auction_comparable_references`.
- `wholesale_guide_references`.
- `external_appraisal_references`.
- `valuation_evidence_timestamp`.
- `valuation_data_freshness_status`.
- `comparable_selection_criteria`.
- `valuation_assumptions`.
- `valuation_limitations`.

### Approval Context

- `approval_required`.
- `approval_status`.
- `approval_reason_codes`.
- `approval_policy_id`.
- `approval_policy_version`.
- `approval_request_id`.
- `approval_decision_ids`.
- `approval_requested_at`.
- `approved_at`.
- `approved_by_actor_id`.
- `approval_expires_at`.
- `approval_evidence_references`.
- `approval_rejection_reason`.
- `approval_revocation_reason`.

Approval may be separately required for:

- Actual cash value.
- Customer allowance.
- Over-allowance.
- Negative-equity treatment.
- Payoff override.
- Remote inspection.
- Title exception.
- Structural or flood damage.
- Odometer discrepancy.
- Acquisition exception.

### Customer Offer

- `offer_status`.
- `offer_version`.
- `offer_presented_at`.
- `offer_expires_at`.
- `offer_document_reference`.
- `offer_document_hash`.
- `offer_interaction_id`.
- `offer_delivery_status`.
- `offer_delivery_confirmation_reference`.
- `customer_acceptance_status`.
- `customer_accepted_at`.
- `customer_acceptance_evidence_references`.
- `customer_rejected_at`.
- `customer_rejection_reason`.
- `customer_rejection_details`.
- `offer_withdrawal_status`.
- `offer_withdrawal_reason`.

The Customer offer must not expose restricted internal valuation fields unless specifically authorized.

### Acquisition Readiness

- `acquisition_readiness_status`.
- `deal_readiness_status`.
- `ownership_transfer_readiness_status`.
- `payoff_readiness_status`.
- `physical_possession_readiness_status`.
- `inventory_intake_readiness_status`.
- `accounting_readiness_status`.
- `acquisition_block_reasons`.
- `acquisition_review_required`.

### Acquisition

- `acquisition_status`.
- `acquisition_requested_at`.
- `acquisition_approved_at`.
- `acquisition_decision_id`.
- `acquisition_command_id`.
- `acquisition_idempotency_key`.
- `acquisition_confirmation_status`.
- `ownership_transferred_at`.
- `physical_possession_accepted_at`.
- `acquired_at`.
- `acquisition_reference`.
- `acquisition_evidence_references`.

### Inventory Intake

- `inventory_intake_status`.
- `inventory_intake_requested_at`.
- `inventory_intake_command_id`.
- `inventory_intake_idempotency_key`.
- `inventory_record_id`.
- `inventory_intake_confirmation_status`.
- `inventory_intake_completed_at`.
- `inventory_intake_failure_reason`.
- `inventory_reconciliation_status`.

The Inventory intake workflow must reference the existing canonical Vehicle where possible.

It must not create a duplicate Vehicle identity for the same physical Vehicle.

### Accounting and Settlement

- `accounting_handoff_status`.
- `accounting_reference`.
- `accounting_confirmation_status`.
- `payoff_payment_status`.
- `payoff_payment_reference`.
- `customer_equity_settlement_status`.
- `customer_equity_settlement_reference`.
- `settlement_completed_at`.
- `settlement_reconciliation_status`.

### Derived Intelligence

- `estimated_market_value_range`.
- `estimated_wholesale_value_range`.
- `estimated_reconditioning_range`.
- `valuation_recommendation_amount`.
- `allowance_recommendation_amount`.
- `damage_detection_results`.
- `document_extraction_results`.
- `ownership_conflict_risk_score`.
- `fraud_risk_score`.
- `appraisal_variance_score`.
- `inventory_profitability_projection`.
- `recommended_acquisition_channel`.
- `recommended_next_action`.
- `recommended_reinspection`.
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

- `vehicle_age_years`.
- `mileage_per_year`.
- `days_since_inspection`.
- `days_since_appraisal`.
- `days_until_appraisal_expiry`.
- `appraisal_expired`.
- `payoff_expired`.
- `document_completion_percentage`.
- `inspection_completion_percentage`.
- `over_allowance_percentage`.
- `estimated_trade_gross_impact_amount`.
- `positive_equity_amount`.
- `negative_equity_amount`.

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
- `completed_at`.
- `cancelled_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `trade_in_id` | UUID | Yes | ASOS | Immutable Canonical Trade-In identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `trade_in_number` | String | Yes | ASOS or external authority | Human-readable Trade-In reference. |
| `customer_id` | UUID | Yes | Canonical relationship | Customer controlling or offering the Vehicle. |
| `opportunity_id` | UUID | Yes | Canonical relationship | Opportunity associated with the Trade-In. |
| `vehicle_id` | UUID | Conditional | Vehicle relationship | Canonical Vehicle after identity resolution. |
| `status` | Enum | Yes | Trade-In workflow | Current Trade-In lifecycle state. |
| `priority` | Enum | Yes | Workflow State | Operational review priority. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Defines whether ASOS or an external system controls material workflow state. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine status. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Material data-conflict status. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Vehicle Identity Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `submitted_vin` | String | Conditional | Customer or source | VIN submitted before verification. |
| `vehicle_identity_resolution_status` | Enum | Yes | Workflow State | Current Vehicle-resolution state. |
| `vin_verification_status` | Enum | Yes | Verification workflow | Verification result for VIN identity. |
| `registration_verification_status` | Enum | Yes | Verification workflow | Verification result for registration identity. |
| `vehicle_history_status` | Enum | Yes | Approved provider projection | Current Vehicle-history evidence status. |
| `vehicle_snapshot` | JSON Object | Conditional | Canonical snapshot | Versioned Vehicle identity and specification snapshot used by Trade-In. |
| `identity_verified_at` | Timestamp | No | Verification workflow | Time accepted Vehicle identity verification completed. |

### Ownership Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_authority_type` | Enum | Yes | Customer or legal evidence | Basis under which the Customer may offer the Vehicle. |
| `customer_authority_status` | Enum | Yes | Verification workflow | Current authority-validation state. |
| `ownership_verification_status` | Enum | Yes | Verification workflow | Current ownership-verification state. |
| `ownership_document_status` | Enum | Yes | Document workflow | Status of required ownership documents. |
| `registration_document_status` | Enum | Yes | Document workflow | Status of required registration evidence. |
| `title_status` | Enum | Yes | Legal or title authority | Current legal title classification. |
| `co_owner_status` | Enum | Yes | Legal evidence projection | Whether another owner must participate. |
| `power_of_attorney_status` | Enum | Yes | Legal evidence projection | Validity of any representative authority. |
| `ownership_verified_at` | Timestamp | No | Verification workflow | Time ownership or authority was accepted. |

### Payoff Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lien_status` | Enum | Yes | Approved external evidence | Current known lien state. |
| `payoff_required` | Boolean | Yes | Deterministic workflow | Indicates whether lender payoff is required. |
| `payoff_verification_status` | Enum | Yes | Verification workflow | Current payoff-verification state. |
| `verified_payoff_amount` | Decimal | Conditional | Lender or approved authority | Current verified settlement amount. |
| `payoff_currency_code` | String | Conditional | Lender or approved authority | ISO 4217 currency code. |
| `payoff_valid_until` | Timestamp | Conditional | Lender authority | Expiration of the verified payoff. |
| `payoff_daily_interest_amount` | Decimal | No | Lender authority | Daily accrual where supplied. |
| `payoff_settlement_status` | Enum | Yes | Settlement workflow | Current payoff settlement state. |
| `lien_release_status` | Enum | Yes | Lender or legal authority | Current lien-release state. |

### Inspection Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inspection_required` | Boolean | Yes | Deterministic policy | Whether a physical or approved remote inspection is required. |
| `inspection_status` | Enum | Yes | Inspection workflow | Current inspection state. |
| `inspection_type` | Enum | No | Inspection workflow | Inspection method. |
| `inspection_version` | Integer | No | ASOS | Current accepted inspection version. |
| `overall_condition` | Enum | Conditional | Authorized inspection | Overall condition classification. |
| `inspected_odometer_value` | Decimal | Conditional | Authorized inspection | Accepted odometer reading. |
| `odometer_verification_status` | Enum | Yes | Verification workflow | Current odometer-verification state. |
| `accident_history_status` | Enum | Yes | Approved evidence | Current known accident-history classification. |
| `structural_damage_status` | Enum | Yes | Inspection or history evidence | Current structural-damage state. |
| `flood_damage_status` | Enum | Yes | Inspection or history evidence | Current flood-damage state. |
| `inspection_report_reference` | String | No | Controlled evidence storage | Reference to the accepted inspection report. |
| `inspection_completed_at` | Timestamp | No | Inspection workflow | Time the accepted inspection completed. |

### Appraisal Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `current_appraisal_id` | UUID | No | Appraisal workflow | Current appraisal-version identifier. |
| `current_appraisal_version` | Integer | No | ASOS | Current appraisal version. |
| `appraisal_status` | Enum | Yes | Appraisal workflow | Current appraisal state. |
| `appraisal_method` | Enum | No | Appraisal workflow | Method used for appraisal. |
| `actual_cash_value_amount` | Decimal | Conditional | Authorized appraisal Decision | Approved dealership acquisition value. |
| `trade_in_allowance_amount` | Decimal | Conditional | Authorized commercial Decision | Customer-facing Trade-In allowance. |
| `over_allowance_amount` | Decimal | Yes | Deterministic calculation | Allowance above actual cash value. |
| `positive_equity_amount` | Decimal | Yes | Deterministic calculation | Positive Customer equity. |
| `negative_equity_amount` | Decimal | Yes | Deterministic calculation | Absolute value of negative Customer equity. |
| `estimated_reconditioning_amount` | Decimal | Yes | Inspection or approved estimate | Expected preparation cost. |
| `final_acquisition_cost_amount` | Decimal | No | Deterministic accounting projection | Approved total dealership acquisition cost. |
| `appraised_at` | Timestamp | No | Appraisal workflow | Time the appraisal became effective. |
| `appraisal_expires_at` | Timestamp | No | Approved validity policy | Time revalidation becomes required. |
| `valuation_snapshot_hash` | String | No | ASOS | Integrity hash of the approved appraisal snapshot. |

### Approval Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `approval_required` | Boolean | Yes | Deterministic policy | Indicates whether one or more approvals are required. |
| `approval_status` | Enum | Yes | Approval workflow | Current approval-chain status. |
| `approval_reason_codes` | Array | No | Policy Engine | Reasons requiring approval. |
| `approval_policy_id` | String | No | Policy Engine | Applied approval policy. |
| `approval_policy_version` | String | No | Policy Engine | Applied approval-policy version. |
| `approval_decision_ids` | Array | No | Human Decision | Accepted approval Decisions. |
| `approved_at` | Timestamp | No | ASOS | Time all required approvals completed. |
| `approval_expires_at` | Timestamp | No | Policy | Time reapproval or revalidation becomes required. |

### Offer Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `offer_status` | Enum | Yes | Trade-In workflow | Current Customer offer state. |
| `offer_version` | Integer | No | ASOS | Version of the Customer-facing Trade-In offer. |
| `offer_presented_at` | Timestamp | No | Interaction or document evidence | Time the offer was presented. |
| `offer_expires_at` | Timestamp | No | Approved validity policy | Time the Customer offer expires. |
| `offer_document_hash` | String | No | ASOS | Hash of the Customer-facing offer. |
| `customer_acceptance_status` | Enum | Yes | Acceptance workflow | Current Customer acceptance state. |
| `customer_accepted_at` | Timestamp | No | Customer evidence | Time accepted Customer evidence was received. |
| `customer_rejected_at` | Timestamp | No | Customer evidence | Time rejection was received. |
| `customer_rejection_reason` | Enum | No | Customer or Human evidence | Standard rejection reason where known. |

### Acquisition Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `acquisition_readiness_status` | Enum | Yes | Deterministic workflow | Current readiness for legal acquisition. |
| `acquisition_status` | Enum | Yes | Acquisition workflow | Current acquisition state. |
| `acquisition_decision_id` | UUID | No | Authoritative Human Decision | Decision authorizing acquisition. |
| `acquisition_confirmation_status` | Enum | Yes | Workflow Projection | External Confirmation state of acquisition. |
| `ownership_transferred_at` | Timestamp | No | Legal or external authority | Time legal ownership transfer completed. |
| `physical_possession_accepted_at` | Timestamp | No | Authorized operational evidence | Time dealership accepted physical possession. |
| `acquired_at` | Timestamp | No | Acquisition workflow | Time all required acquisition conditions were accepted. |
| `inventory_record_id` | UUID | No | Inventory relationship | Inventory Record created after successful intake. |
| `inventory_intake_status` | Enum | Yes | Inventory workflow | Current Inventory intake state. |
| `inventory_intake_completed_at` | Timestamp | No | Inventory workflow | Time Inventory intake completed. |

---

## 4. Enumerations

### TradeInStatus

- `CREATED`
- `IDENTITY_RESOLUTION_PENDING`
- `INSPECTION_PENDING`
- `INSPECTION_SCHEDULED`
- `INSPECTION_IN_PROGRESS`
- `INSPECTED`
- `VALUATION_IN_PROGRESS`
- `DOCUMENTS_PENDING`
- `PAYOFF_PENDING`
- `APPROVAL_PENDING`
- `APPRAISED`
- `OFFER_PRESENTED`
- `CUSTOMER_ACCEPTED`
- `CUSTOMER_REJECTED`
- `EXPIRED`
- `ACQUISITION_PENDING`
- `ACQUIRED`
- `INVENTORY_INTAKE_PENDING`
- `COMPLETED`
- `BLOCKED`
- `DISPUTED`
- `CANCELLED`
- `ARCHIVED`

### TradeInType

- `PURCHASE_TRANSACTION`
- `LEASE_REPLACEMENT`
- `DIRECT_ACQUISITION`
- `FLEET_REPLACEMENT`
- `CUSTOMER_BUYBACK`
- `ESTATE_OR_COMPANY_DISPOSAL`
- `OTHER`

### TradeInPriority

- `STANDARD`
- `HIGH`
- `URGENT`
- `STRATEGIC`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_APPRAISAL_AUTHORITATIVE`
- `EXTERNAL_DMS_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### VehicleIdentityResolutionStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `MATCHED_EXISTING_VEHICLE`
- `NEW_VEHICLE_CREATED`
- `AMBIGUOUS`
- `CONFLICTED`
- `FAILED`
- `REVIEW_REQUIRED`

### VerificationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `VERIFIED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### CustomerAuthorityType

- `REGISTERED_OWNER`
- `CO_OWNER`
- `AUTHORIZED_REPRESENTATIVE`
- `POWER_OF_ATTORNEY`
- `COMPANY_REPRESENTATIVE`
- `ESTATE_REPRESENTATIVE`
- `LEASE_OR_FINANCE_HOLDER`
- `OTHER`
- `UNKNOWN`

### CustomerAuthorityStatus

- `NOT_VERIFIED`
- `PENDING`
- `VERIFIED`
- `PARTIALLY_VERIFIED`
- `REJECTED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### OwnershipDocumentStatus

- `NOT_REQUIRED`
- `NOT_PROVIDED`
- `PARTIAL`
- `PROVIDED`
- `VERIFICATION_PENDING`
- `VERIFIED`
- `REJECTED`
- `EXPIRED`
- `DISPUTED`

### TradeInTitleStatus

- `UNKNOWN`
- `CLEAR`
- `FINANCED`
- `LIEN_RECORDED`
- `MULTIPLE_LIENS`
- `DUPLICATE_TITLE_REQUIRED`
- `LOST_TITLE`
- `SALVAGE`
- `REBUILT`
- `EXPORT_RESTRICTED`
- `IMPORT_RESTRICTED`
- `LEGAL_HOLD`
- `DISPUTED`
- `BLOCKED`

### TradeInLienStatus

- `UNKNOWN`
- `NO_LIEN`
- `ACTIVE_LIEN`
- `MULTIPLE_LIENS`
- `LIEN_RELEASE_PENDING`
- `LIEN_RELEASED`
- `DISPUTED`
- `BLOCKED`

### PayoffVerificationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUESTED`
- `PENDING`
- `VERIFIED`
- `EXPIRED`
- `REJECTED`
- `FAILED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### PayoffSettlementStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PAYMENT_PENDING`
- `PAYMENT_SENT`
- `PENDING_CONFIRMATION`
- `SETTLED`
- `REJECTED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### LienReleaseStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `REQUESTED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### InspectionStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `SCHEDULING`
- `SCHEDULED`
- `IN_PROGRESS`
- `COMPLETED`
- `INCOMPLETE`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `REINSPECTION_REQUIRED`

### InspectionType

- `PHYSICAL_COMPREHENSIVE`
- `PHYSICAL_EXPRESS`
- `REMOTE_GUIDED`
- `CUSTOMER_SELF_CAPTURE`
- `THIRD_PARTY_INSPECTION`
- `AUCTION_INSPECTION`
- `OTHER`

### TradeInCondition

- `NOT_INSPECTED`
- `EXCELLENT`
- `GOOD`
- `FAIR`
- `POOR`
- `NON_RUNNING`
- `DAMAGED`
- `SALVAGE`
- `UNKNOWN`

### ComponentCondition

- `NOT_INSPECTED`
- `EXCELLENT`
- `GOOD`
- `FAIR`
- `POOR`
- `FAILED`
- `NOT_APPLICABLE`
- `UNKNOWN`

### AccidentHistoryStatus

- `UNKNOWN`
- `NONE_REPORTED`
- `REPORTED_MINOR`
- `REPORTED_MAJOR`
- `STRUCTURAL_DAMAGE`
- `TOTAL_LOSS`
- `FLOOD_DAMAGE`
- `CONFLICTED`

### OdometerVerificationStatus

- `NOT_VERIFIED`
- `VERIFICATION_PENDING`
- `VERIFIED`
- `DISCREPANCY`
- `ROLLBACK_SUSPECTED`
- `NOT_READABLE`
- `EXEMPT`
- `DISPUTED`

### TradeInAppraisalStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `EVIDENCE_INCOMPLETE`
- `CALCULATION_PENDING`
- `APPROVAL_PENDING`
- `APPROVED`
- `REJECTED`
- `EXPIRED`
- `SUPERSEDED`
- `DISPUTED`
- `REVALIDATION_REQUIRED`

### TradeInAppraisalMethod

- `MANUAL_APPRAISAL`
- `MARKET_COMPARABLES`
- `AUCTION_DATA`
- `WHOLESALE_GUIDE`
- `EXTERNAL_PROVIDER`
- `AI_ASSISTED`
- `COMBINED`

`AI_ASSISTED` does not create valuation authority.

### AppraisalValidityStatus

- `NOT_STARTED`
- `ACTIVE`
- `APPROACHING_EXPIRY`
- `EXPIRED`
- `SUSPENDED`
- `REVALIDATION_REQUIRED`

### TradeInApprovalStatus

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

### TradeInOfferStatus

- `NOT_PREPARED`
- `PREPARATION_PENDING`
- `READY`
- `PRESENTED`
- `EXPIRED`
- `REJECTED`
- `ACCEPTED`
- `WITHDRAWAL_PENDING`
- `WITHDRAWN`
- `SUPERSEDED`
- `DISPUTED`

### TradeInCustomerAcceptanceStatus

- `NOT_REQUESTED`
- `PENDING`
- `ACCEPTED`
- `REJECTED`
- `EXPIRED`
- `WITHDRAWN`
- `DISPUTED`
- `RECONFIRMATION_REQUIRED`

### TradeInRejectionReason

- `VALUE_TOO_LOW`
- `ALLOWANCE_NOT_ACCEPTED`
- `PAYOFF_TOO_HIGH`
- `NEGATIVE_EQUITY_NOT_ACCEPTED`
- `VEHICLE_CONDITION_DISPUTED`
- `CUSTOMER_CHANGED_MIND`
- `CUSTOMER_SOLD_ELSEWHERE`
- `DOCUMENTS_UNAVAILABLE`
- `OWNERSHIP_NOT_VERIFIED`
- `OPPORTUNITY_CANCELLED`
- `OTHER`
- `UNKNOWN`

### AcquisitionReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### TradeInAcquisitionStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `AWAITING_APPROVAL`
- `READY`
- `ACQUISITION_IN_PROGRESS`
- `PENDING_EXTERNAL_CONFIRMATION`
- `ACQUIRED`
- `FAILED`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### InventoryIntakeStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `CREATION_PENDING`
- `PENDING_EXTERNAL_CONFIRMATION`
- `INVENTORY_RECORD_CREATED`
- `COMPLETED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### AccountingHandoffStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### TradeInCancellationReason

- `CUSTOMER_WITHDREW`
- `OPPORTUNITY_LOST`
- `DEAL_CANCELLED`
- `OWNERSHIP_FAILED`
- `TITLE_BLOCKED`
- `LIEN_UNRESOLVED`
- `FRAUD_SUSPECTED`
- `VEHICLE_NOT_PRESENTED`
- `VEHICLE_IDENTITY_CONFLICT`
- `DUPLICATE_TRADE_IN`
- `DEALERSHIP_DECLINED`
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
- Every related Object must belong to the authorized Tenant.
- Dealership, branch, team, User, Appraiser, Inspector, and approval authority must belong to the permitted organizational scope.
- Cross-Tenant Trade-In search, matching, valuation, acquisition, AI retrieval, and export are prohibited unless an approved and auditable mechanism exists.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Customer and Opportunity Rules

- Every Trade-In must reference one resolved Customer.
- Every Trade-In must reference one valid Opportunity unless an approved direct-acquisition workflow applies.
- Opportunity and Customer must be consistent.
- Customer contact restrictions must be respected.
- Trade-In processing must not create general marketing Consent.
- A closed or cancelled Opportunity requires a governed exception before Trade-In progression continues.
- A Trade-In may remain historically valid after Opportunity closure, but acquisition activity must follow approved policy.

### Vehicle Identity Rules

- Submitted Vehicle information remains unverified input.
- Vehicle identity resolution must occur before final appraisal approval unless an approved exception applies.
- VIN must be normalized before verification.
- VIN structural validity does not prove the Vehicle’s legal identity.
- A physical Vehicle must not be represented by multiple active canonical Vehicle records.
- Identity conflicts must create Human Review.
- Registration number alone must not be used as the sole permanent Vehicle identifier.
- The canonical Vehicle record must not be silently overwritten by Trade-In data.
- Accepted identity corrections must use Vehicle governance.
- Inventory intake must reuse the resolved canonical Vehicle where possible.

### Duplicate Trade-In Rules

One physical Vehicle should normally have only one active Trade-In workflow within the same Tenant and materially overlapping commercial journey.

Duplicate detection should consider:

- `vehicle_id`.
- Verified VIN.
- Registration identifier.
- Customer.
- Opportunity.
- Dealership.
- Inspection evidence.
- Timeframe.
- Existing active Deal.
- Existing acquisition workflow.

An ambiguous match must not be merged automatically solely from an AI score.

A duplicate Trade-In must preserve:

- Original source.
- Submitted evidence.
- Customer relationship.
- Duplicate Decision.
- Surviving Trade-In reference.
- Audit history.

### Ownership and Authority Rules

- Customer identity does not by itself prove Vehicle ownership.
- Ownership must be supported by approved evidence.
- A Customer may offer the Vehicle as:
  - Registered owner.
  - Co-owner.
  - Authorized representative.
  - Company representative.
  - Estate representative.
  - Another legally permitted authority.
- Required co-owner approvals must be obtained.
- Power-of-attorney evidence must be verified where used.
- Company-owned Vehicles require authorized-signatory evidence.
- Ownership evidence must be current and applicable to the Vehicle.
- Ownership conflict must block acquisition.
- Customer acceptance must not override ownership failure.
- AI must not verify legal ownership independently.

### Title, Legal, and Compliance Rules

- Title status must be evaluated before final acquisition.
- Salvage, rebuilt, total-loss, flood-damaged, structurally damaged, imported, export-restricted, disputed, or legally held Vehicles require enhanced review.
- Theft checks must be completed where required.
- Legal or compliance blocks override commercial approval.
- A blocked title must prevent acquisition.
- Compliance evidence must preserve source and freshness.
- AI Recommendations must not clear legal or compliance blocks.
- Legal exception Decisions must be made by authorized Humans.

### Inspection Rules

A final appraisal normally requires an accepted inspection.

An approved remote-appraisal policy may permit a preliminary or conditional appraisal.

Inspection must preserve:

- Inspector.
- Inspection method.
- Location.
- Date and time.
- Vehicle identity.
- Odometer.
- Condition findings.
- Damage.
- Missing items.
- Media.
- Diagnostic evidence where applicable.
- Inspection version.
- Integrity hash.

Inspection must not be marked complete when required sections remain unresolved.

A material Vehicle condition change requires reinspection or appraisal revalidation.

AI image analysis may support damage detection.

It must not independently establish authoritative condition.

### Odometer Rules

- Odometer values must be zero or greater.
- Original reported unit must be preserved.
- A normalized kilometers projection may be calculated deterministically.
- Odometer evidence must preserve source and timestamp.
- Reported and inspected values must remain distinguishable.
- Odometer discrepancy, rollback suspicion, unreadable status, or conflicting history requires Human Review.
- Appraisal and acquisition may be blocked by material odometer conflict.
- AI must not resolve an odometer dispute independently.

### Valuation Rules

- Customer expectation must remain separate from dealership valuation.
- Market, wholesale, auction, and retail references must remain separate.
- Actual cash value must remain separate from Customer allowance.
- Over-allowance must be calculated deterministically.
- Payoff must not be treated as Vehicle value.
- Equity must be calculated deterministically.
- AI valuation output is a Recommendation until approved.
- Valuation sources must preserve:
  - Provider.
  - Timestamp.
  - Geographic context.
  - Vehicle match criteria.
  - Condition assumptions.
  - Mileage assumptions.
  - Comparable-selection criteria.
- Stale valuation evidence must not support final approval.
- A valuation must use one approved currency or a governed foreign-exchange process.
- An appraisal must expire according to configurable policy.
- No fixed appraisal-validity duration may be embedded in this Canonical Domain Model.

### Calculation Rules

All authoritative Trade-In monetary calculations must be deterministic.

Approved formulas must calculate at least:

```text
over_allowance_amount =
  max(trade_in_allowance_amount - actual_cash_value_amount, 0)

raw_equity_amount =
  trade_in_allowance_amount - verified_payoff_amount

positive_equity_amount =
  max(raw_equity_amount, 0)

negative_equity_amount =
  abs(min(raw_equity_amount, 0))
```

`final_acquisition_cost_amount` must use an approved formula and may include:

- Actual cash value.
- Approved acquisition fees.
- Transport.
- Title and registration.
- Reconditioning.
- Payoff-related cost.
- Holding cost.
- Approved adjustments.

AI must not calculate authoritative totals.

The calculation service must preserve:

- Formula version.
- Input values.
- Currency.
- Rounding.
- Rule versions.
- Output values.
- Timestamp.
- Integrity hash.

### Monetary Constraint Rules

- Monetary values must use fixed decimal precision.
- Amounts must be zero or greater unless an approved signed adjustment permits otherwise.
- `trade_in_allowance_amount` may exceed `actual_cash_value_amount` only through approved authority.
- `over_allowance_amount` must equal the approved deterministic result.
- `positive_equity_amount` and `negative_equity_amount` must not both be positive.
- Payoff currency must match the appraisal currency or use an approved conversion.
- Expired payoff evidence must not be used for final Deal or acquisition completion.
- Calculation mismatch must block approval and acquisition.

### Payoff Rules

An active lien normally requires:

- Verified lender identity.
- Current payoff statement.
- Payoff amount.
- Currency.
- Verification timestamp.
- Valid-until timestamp.
- Payment instructions through controlled storage.
- Required Customer authority.
- Settlement workflow.
- Lien-release workflow.

A lender payoff estimate must not be represented as verified payoff.

A sent payoff payment Command does not prove settlement.

Payoff remains pending until authoritative Confirmation.

Manual payoff override requires:

- Authorized Human Decision.
- Reason.
- Supporting evidence.
- Expiration.
- Audit history.

### Approval Rules

Approval may be required for:

- Actual cash value.
- Customer allowance.
- Over-allowance.
- Negative-equity treatment.
- Missing inspection.
- Remote inspection.
- Odometer discrepancy.
- Structural damage.
- Flood damage.
- Salvage or rebuilt title.
- Ownership exception.
- Payoff exception.
- Manual valuation override.
- Acquisition exception.
- Negative or restricted profitability.

Approval must preserve:

- Requested scope.
- Values.
- Evidence.
- Applied policy.
- Required approver roles.
- Decisions.
- Reasons.
- Effective period.
- Revocation.

AI Agents must not approve valuation, allowance, payoff, ownership, acquisition, or Inventory intake.

### Appraisal Version Rules

- Every appraisal version must have a unique identifier and version.
- Only one current appraisal version may be active.
- An approved or presented appraisal version must not be materially edited.
- Material changes require a new version.
- Supersession must preserve the prior appraisal.
- Circular supersession is prohibited.
- Customer acceptance must reference the exact offer and appraisal version.
- Quotation and Deal must preserve the exact appraisal version used.
- Retryable version creation must not create duplicate versions.

### Customer Offer Rules

A Trade-In offer may be presented only when:

- Current approved appraisal exists.
- Customer allowance is approved.
- Appraisal remains valid.
- Payoff information is sufficiently current where required.
- Material ownership and title risks are disclosed or resolved.
- Customer-facing document excludes restricted internal fields.
- Offer version and document hash are preserved.
- Required Quotation or Opportunity context exists.
- No blocking conflict exists.

The Customer-facing offer may show:

- Trade-In allowance.
- Verified or estimated payoff where permitted.
- Positive or negative equity.
- Validity.
- Required conditions.
- Customer disclosures.

It must not expose unauthorized:

- Actual cash value.
- Internal wholesale target.
- Reconditioning estimate.
- Internal margin.
- Approval thresholds.
- Fraud indicators.
- Internal valuation strategy.

### Customer Acceptance Rules

Customer acceptance requires:

- Valid current offer.
- Exact offer version.
- Exact document hash where applicable.
- Current appraisal.
- Current payoff information where required.
- Verified Customer identity.
- Accepted evidence.
- Acceptance timestamp.
- No blocking conflict.

Acceptance must not be inferred solely from:

- Positive sentiment.
- Appointment attendance.
- Quotation view.
- Deposit initiation.
- AI prediction.
- Salesperson note without accepted evidence.

Customer acceptance does not transfer ownership.

### Expiration and Revalidation Rules

An appraisal or offer must expire when:

- Configured validity period ends.
- Material condition changes.
- Odometer materially changes.
- Vehicle identity changes.
- Market evidence becomes stale.
- Payoff expires.
- Ownership evidence expires.
- Title or lien state changes.
- Damage is discovered.
- Vehicle is unavailable for reinspection.
- Another configured material dependency changes.

An expired appraisal must not be used for:

- New Quotation issue.
- Customer acceptance.
- Deal conversion.
- Acquisition.

Revalidation may produce a new appraisal version.

### Acquisition Readiness Rules

Acquisition readiness requires:

- Customer accepted applicable terms.
- Valid Deal or approved direct-acquisition workflow.
- Valid Customer authority.
- Verified Vehicle identity.
- Current approved appraisal.
- Current payoff.
- Valid title and ownership evidence.
- Required co-owner or representative approvals.
- Cleared legal and compliance blocks.
- Approved acquisition Decision.
- Defined physical-possession process.
- Defined Inventory intake process.
- Defined accounting and settlement process.

Readiness does not prove acquisition completion.

### Acquisition Rules

A Trade-In may become `ACQUIRED` only when:

- Required acquisition authority exists.
- Customer terms remain valid.
- Deal conditions are satisfied.
- Ownership transfer is legally valid.
- Required documents are executed.
- Required payoff settlement is completed or governed arrangements are accepted.
- Physical possession is accepted.
- Required External Confirmations are received.
- Acquisition evidence is preserved.
- `acquired_at` is populated.

A sent acquisition Command does not prove acquisition.

### Inventory Intake Rules

Inventory intake may begin only after valid acquisition.

Trade-In Domain Service may initiate and track an Inventory intake request.

Inventory Domain Service owns authoritative Inventory Record creation, activation, and stock-cycle state.

The Inventory intake request must:

- Reference the canonical Vehicle.
- Reference the originating Trade-In.
- Validate VIN consistency.
- Validate physical possession.
- Identify the receiving dealership, branch, and location.
- Identify the acquisition source as Trade-In.
- Include the approved acquisition-cost projection.
- Prevent duplicate active Inventory Records.
- Use an idempotency key.
- Remain pending until Inventory Domain Service accepts or rejects the request.
- Await External Confirmation where another Inventory or DMS system is authoritative.

Trade-In must store only:

- Inventory intake request state.
- Inventory Record reference after Confirmation.
- Confirmation evidence.
- Failure evidence.
- Reconciliation state.

Trade-In must not create, activate, or directly update the authoritative Inventory Record.

Trade-In `COMPLETED` requires authoritative Inventory intake Confirmation and accounting reconciliation according to policy.

### External Authority Rules

When an external appraisal, DMS, title, lender, accounting, or Inventory system is authoritative:

- ASOS must issue approved Commands through Command Orchestration.
- Retryable Commands must use `idempotency_key`.
- Local workflow must remain pending until External Confirmation.
- Transport success does not equal business completion.
- Conflicting data must create reconciliation.
- Higher-authority evidence must not be silently overwritten.
- Missing Confirmation must trigger retry, polling, timeout, reconciliation, or Human escalation.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Trade-In creation must support idempotency.
- Appraisal-version creation must support idempotency.
- Customer acceptance processing must support idempotency.
- Acquisition must support idempotency.
- Inventory intake must support idempotency.
- Payoff settlement Commands must support idempotency.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Trade-In records.
  - Inspections.
  - Appraisal versions.
  - Approval requests.
  - Customer offers.
  - Acceptance records.
  - Acquisitions.
  - Vehicles.
  - Inventory Records.
  - Payoff payments.

### Human Review Requirements

Human Review is required according to policy for:

- Vehicle identity conflict.
- Duplicate Trade-In.
- Ownership conflict.
- Co-owner issue.
- Power-of-attorney issue.
- Title dispute.
- Multiple liens.
- Payoff conflict.
- Odometer discrepancy.
- Structural damage.
- Flood damage.
- Salvage or rebuilt title.
- Fraud concern.
- Remote-inspection exception.
- Material appraisal override.
- Over-allowance.
- Negative-equity exception.
- Customer dispute.
- Acquisition exception.
- Inventory reconciliation failure.
- Another material legal, financial, or commercial risk.

---

## 6. State Machine

### Allowed States

```text
CREATED
IDENTITY_RESOLUTION_PENDING
INSPECTION_PENDING
INSPECTION_SCHEDULED
INSPECTION_IN_PROGRESS
INSPECTED
VALUATION_IN_PROGRESS
DOCUMENTS_PENDING
PAYOFF_PENDING
APPROVAL_PENDING
APPRAISED
OFFER_PRESENTED
CUSTOMER_ACCEPTED
CUSTOMER_REJECTED
EXPIRED
ACQUISITION_PENDING
ACQUIRED
INVENTORY_INTAKE_PENDING
COMPLETED
BLOCKED
DISPUTED
CANCELLED
ARCHIVED
```

### Principal Allowed Transitions

```text
CREATED → IDENTITY_RESOLUTION_PENDING
CREATED → INSPECTION_PENDING
CREATED → CANCELLED
CREATED → BLOCKED

IDENTITY_RESOLUTION_PENDING → INSPECTION_PENDING
IDENTITY_RESOLUTION_PENDING → BLOCKED
IDENTITY_RESOLUTION_PENDING → DISPUTED
IDENTITY_RESOLUTION_PENDING → CANCELLED

INSPECTION_PENDING → INSPECTION_SCHEDULED
INSPECTION_PENDING → INSPECTION_IN_PROGRESS
INSPECTION_PENDING → BLOCKED
INSPECTION_PENDING → CANCELLED

INSPECTION_SCHEDULED → INSPECTION_IN_PROGRESS
INSPECTION_SCHEDULED → INSPECTION_PENDING
INSPECTION_SCHEDULED → CANCELLED
INSPECTION_SCHEDULED → BLOCKED

INSPECTION_IN_PROGRESS → INSPECTED
INSPECTION_IN_PROGRESS → INSPECTION_PENDING
INSPECTION_IN_PROGRESS → DISPUTED
INSPECTION_IN_PROGRESS → BLOCKED
INSPECTION_IN_PROGRESS → CANCELLED

INSPECTED → VALUATION_IN_PROGRESS
INSPECTED → DOCUMENTS_PENDING
INSPECTED → PAYOFF_PENDING
INSPECTED → BLOCKED
INSPECTED → CANCELLED

VALUATION_IN_PROGRESS → DOCUMENTS_PENDING
VALUATION_IN_PROGRESS → PAYOFF_PENDING
VALUATION_IN_PROGRESS → APPROVAL_PENDING
VALUATION_IN_PROGRESS → APPRAISED
VALUATION_IN_PROGRESS → DISPUTED
VALUATION_IN_PROGRESS → BLOCKED
VALUATION_IN_PROGRESS → CANCELLED

DOCUMENTS_PENDING → VALUATION_IN_PROGRESS
DOCUMENTS_PENDING → PAYOFF_PENDING
DOCUMENTS_PENDING → APPROVAL_PENDING
DOCUMENTS_PENDING → BLOCKED
DOCUMENTS_PENDING → CANCELLED

PAYOFF_PENDING → VALUATION_IN_PROGRESS
PAYOFF_PENDING → APPROVAL_PENDING
PAYOFF_PENDING → BLOCKED
PAYOFF_PENDING → CANCELLED

APPROVAL_PENDING → APPRAISED
APPROVAL_PENDING → VALUATION_IN_PROGRESS
APPROVAL_PENDING → BLOCKED
APPROVAL_PENDING → CANCELLED

APPRAISED → OFFER_PRESENTED
APPRAISED → EXPIRED
APPRAISED → VALUATION_IN_PROGRESS
APPRAISED → BLOCKED
APPRAISED → CANCELLED

OFFER_PRESENTED → CUSTOMER_ACCEPTED
OFFER_PRESENTED → CUSTOMER_REJECTED
OFFER_PRESENTED → EXPIRED
OFFER_PRESENTED → VALUATION_IN_PROGRESS
OFFER_PRESENTED → DISPUTED
OFFER_PRESENTED → CANCELLED

CUSTOMER_ACCEPTED → ACQUISITION_PENDING
CUSTOMER_ACCEPTED → EXPIRED
CUSTOMER_ACCEPTED → DISPUTED
CUSTOMER_ACCEPTED → BLOCKED
CUSTOMER_ACCEPTED → CANCELLED

CUSTOMER_REJECTED → VALUATION_IN_PROGRESS
CUSTOMER_REJECTED → CANCELLED
CUSTOMER_REJECTED → ARCHIVED

EXPIRED → INSPECTION_PENDING
EXPIRED → VALUATION_IN_PROGRESS
EXPIRED → CANCELLED
EXPIRED → ARCHIVED

ACQUISITION_PENDING → ACQUIRED
ACQUISITION_PENDING → BLOCKED
ACQUISITION_PENDING → DISPUTED
ACQUISITION_PENDING → CANCELLED

ACQUIRED → INVENTORY_INTAKE_PENDING
ACQUIRED → DISPUTED
ACQUIRED → BLOCKED

INVENTORY_INTAKE_PENDING → COMPLETED
INVENTORY_INTAKE_PENDING → BLOCKED
INVENTORY_INTAKE_PENDING → DISPUTED

BLOCKED → previous permitted non-terminal state
BLOCKED → CANCELLED

DISPUTED → previous permitted non-terminal state
DISPUTED → BLOCKED
DISPUTED → CANCELLED

COMPLETED → ARCHIVED
CANCELLED → ARCHIVED
```

Returning from `BLOCKED` or `DISPUTED` requires an accepted resolution and supporting evidence.

### Forbidden Ordinary Transitions

```text
CREATED → APPRAISED
CREATED → OFFER_PRESENTED
CREATED → CUSTOMER_ACCEPTED
CREATED → ACQUIRED
CREATED → COMPLETED

INSPECTION_PENDING → APPRAISED
INSPECTED → OFFER_PRESENTED
VALUATION_IN_PROGRESS → CUSTOMER_ACCEPTED
APPROVAL_PENDING → CUSTOMER_ACCEPTED

APPRAISED → ACQUIRED
OFFER_PRESENTED → ACQUIRED
CUSTOMER_REJECTED → ACQUIRED
EXPIRED → ACQUIRED

CUSTOMER_ACCEPTED → COMPLETED
ACQUISITION_PENDING → COMPLETED
ACQUIRED → APPRAISED

COMPLETED → APPRAISED
COMPLETED → OFFER_PRESENTED
COMPLETED → CANCELLED

CANCELLED → APPRAISED
CANCELLED → ACQUIRED
CANCELLED → COMPLETED

ARCHIVED → VALUATION_IN_PROGRESS
ARCHIVED → ACQUIRED
```

Corrections to terminal or legally significant outcomes require a separate governed correction, unwind, or dispute workflow.

### Entering CREATED

Requires:

- Valid Tenant context.
- Customer.
- Opportunity or approved direct-acquisition context.
- Minimum submitted Vehicle information.
- Source.
- Creation authority.
- Idempotency protection.
- Initial audit evidence.

### Entering IDENTITY_RESOLUTION_PENDING

Requires:

- Sufficient Vehicle identifiers.
- Identity-resolution request.
- Evidence references.
- No confirmed duplicate Vehicle conflict.

### Entering INSPECTION_PENDING

Requires:

- Resolved or sufficiently reviewable Vehicle identity.
- Defined inspection requirement.
- Responsible team or queue.
- No blocking legal restriction preventing inspection.

### Entering INSPECTION_SCHEDULED

Requires:

- Valid Appointment or inspection schedule.
- Inspector or resource.
- Time.
- Location.
- Customer coordination where required.

### Entering INSPECTION_IN_PROGRESS

Requires:

- Vehicle presented or approved remote-inspection process.
- Inspector identity.
- Vehicle identity verification.
- Inspection version.
- Start timestamp.

### Entering INSPECTED

Requires:

- Required inspection sections complete.
- Odometer recorded.
- Condition findings recorded.
- Required media and evidence.
- Accepted inspector authority.
- Completion timestamp.
- No unresolved critical inspection failure.

### Entering VALUATION_IN_PROGRESS

Requires:

- Accepted inspection or approved exception.
- Vehicle identity.
- Current market or valuation evidence.
- Condition inputs.
- Mileage inputs.
- Calculation workflow.

### Entering DOCUMENTS_PENDING

Requires:

- Identified missing, expired, disputed, or rejected ownership, registration, title, identity, or representative evidence.

### Entering PAYOFF_PENDING

Requires:

- Active or possible lien.
- Missing, stale, disputed, or incomplete payoff evidence.

### Entering APPROVAL_PENDING

Requires:

- Provisional valuation.
- Completed calculation.
- Defined approval reasons.
- Frozen approval-request snapshot.
- Assigned approval authority.

### Entering APPRAISED

Requires:

- Accepted inspection.
- Accepted Vehicle identity.
- Completed deterministic valuation.
- Approved actual cash value.
- Current payoff evidence where required.
- Documented ownership and title risk.
- Required approvals.
- Effective and expiry timestamps.
- Immutable appraisal snapshot.
- No blocking conflict.

### Entering OFFER_PRESENTED

Requires:

- Current approved appraisal.
- Approved Customer allowance.
- Valid offer version.
- Customer-facing document or accepted Interaction evidence.
- Offer expiration.
- No unauthorized internal information.
- Presentation timestamp.

### Entering CUSTOMER_ACCEPTED

Requires:

- Exact offer version.
- Exact document hash where applicable.
- Current appraisal.
- Current payoff.
- Valid Customer identity.
- Accepted evidence.
- Acceptance timestamp.
- No blocking conflict.

### Entering CUSTOMER_REJECTED

Requires:

- Customer rejection evidence.
- Rejection timestamp.
- Rejection reason where known.

### Entering EXPIRED

Requires:

- Expired appraisal or offer; or
- Material change invalidating the current appraisal.

### Entering ACQUISITION_PENDING

Requires:

- Customer acceptance.
- Eligible Deal or direct-acquisition workflow.
- Acquisition readiness evaluated.
- Required ownership, title, payoff, and legal evidence.
- Required Human Decision.
- Acquisition request.
- Idempotency key.

### Entering ACQUIRED

Requires:

- Valid acquisition authority.
- Legally valid ownership transfer.
- Accepted physical possession.
- Required payoff conditions.
- Required documents.
- External Confirmation where applicable.
- Acquisition evidence.
- `acquired_at`.

### Entering INVENTORY_INTAKE_PENDING

Requires:

- Acquired Vehicle.
- Canonical Vehicle reference.
- Receiving dealership and branch.
- Approved acquisition-cost projection.
- Inventory intake request.
- Idempotency key.
- No duplicate active Inventory Record.

### Entering COMPLETED

Requires:

- Inventory Record created or confirmed.
- Inventory intake completed.
- Accounting handoff completed or accepted under policy.
- Payoff and lien workflows reconciled.
- Ownership and title workflows reconciled.
- Required audit evidence complete.
- Completion timestamp.

### Entering CANCELLED

Requires:

- Valid cancellation reason.
- Authorized actor or policy.
- Closure of active inspection, appraisal, offer, acquisition, and Inventory actions.
- Required Customer communication evaluation.
- Audit evidence.

### Terminal States

For ordinary processing:

- `COMPLETED`
- `CANCELLED`
- `ARCHIVED`

`CUSTOMER_REJECTED` and `EXPIRED` may be reopened through an approved reappraisal workflow.

### Correction and Unwind

Correcting or unwinding an acquired or completed Trade-In requires:

- Authorized Human Decision.
- Legal and accounting review.
- Deal review.
- Inventory review.
- Payoff review.
- Ownership and title review.
- Supporting evidence.
- Correction or reversal records.
- New Events.
- Preserved original history.

AI Agents must not independently correct or unwind acquisition.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
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

- Every Trade-In belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant acquisition requires an approved and auditable mechanism.

### Customer

- Every Trade-In references one Customer.
- Customer identity remains governed by Customer.
- Customer authority to offer the Vehicle must be independently verified.
- One Customer may have multiple Trade-Ins over time.

### Lead and Qualified Lead

- A Trade-In may originate from Lead or Qualified Lead interest.
- Lead Trade-In interest does not create an appraisal.
- Qualification does not prove ownership or acquisition eligibility.

### Opportunity

- Every sales-related Trade-In references one Opportunity.
- Opportunity owns the active sales pursuit.
- Trade-In supplies appraisal and acquisition projections to Opportunity.
- Opportunity closure may affect Trade-In progression according to policy.

### Appointment

- Inspection may be scheduled through Appointment.
- Appointment completion does not prove inspection completion unless accepted inspection evidence exists.
- Appointment outcome must not replace the inspection report.

### Vehicle

- Trade-In should reference one canonical physical Vehicle after identity resolution.
- Vehicle owns identity and technical specifications.
- Trade-In owns appraisal, inspection, ownership, payoff, and acquisition workflow.
- Trade-In must not create a duplicate Vehicle during Inventory intake.

### Inventory Record

- Inventory Record must not exist as active dealership stock before valid acquisition unless an approved expected-stock projection is used.
- Trade-In may request creation or activation of one Inventory Record after acquisition.
- Inventory Domain Service owns authoritative Inventory Record creation, activation, and stock-cycle state.
- Trade-In stores only the intake request, Confirmation, relationship, and reconciliation projections.
- Inventory Record owns dealership stock context.
- Inventory availability must not be stored as Trade-In status.

### Quotation

- A Quotation may reference one current Trade-In appraisal version.
- Quotation owns the Customer-facing transaction offer.
- Trade-In values changing after Quotation issue may require a new Quotation version.
- Quotation acceptance does not independently acquire the Trade-In Vehicle.

### Finance Application

- Finance Application may consume Trade-In equity projections.
- Lender Decision remains governed by the lender or F&I authority.
- Trade-In payoff and new Vehicle finance must remain distinguishable.
- Sensitive lender information must not be duplicated unnecessarily.

### Financial Contract

- A Financial Contract may include negative-equity or payoff treatment.
- Contract signature does not independently confirm lien settlement or ownership transfer.
- Contract terms remain governed by Financial Contract.

### Deal

- Deal may reference one or more permitted Trade-In records according to policy.
- Deal must preserve the exact accepted appraisal and offer version.
- Trade-In acquisition normally requires an eligible Deal.
- Deal completion does not replace Trade-In acquisition evidence.

### Payment and Payoff

- Customer Payments and lender payoff Payments remain separate transactions.
- Payment initiation does not prove settlement.
- Trade-In stores only required projections and references.
- Payment authority remains with the configured Payment or financial system.

### Interaction

Interactions may provide:

- Customer Vehicle information.
- Inspection coordination.
- Appraisal presentation.
- Customer questions.
- Customer acceptance.
- Customer rejection.
- Document requests.
- Acquisition coordination.

Original communication evidence remains governed by Interaction and its provider.

### Market Intelligence

Market Intelligence may supply:

- Comparable Vehicles.
- Wholesale data.
- Auction data.
- Retail data.
- Regional demand.
- Expected days to sell.
- Reconditioning benchmarks.
- Valuation Recommendations.

Market evidence must not silently change approved appraisal values.

### Human Decisions and Approvals

Trade-In may reference Decisions for:

- Remote inspection.
- Actual cash value.
- Allowance.
- Over-allowance.
- Negative equity.
- Payoff exception.
- Title exception.
- Ownership exception.
- Acquisition.
- Inventory intake exception.

Decision history must remain auditable.

### Supporting Child Records

Trade-In may own or govern:

- Submitted Vehicle records.
- Identity-resolution records.
- Ownership verification.
- Title verification.
- Lien and payoff records.
- Inspection versions.
- Damage records.
- Appraisal versions.
- Valuation sources.
- Approval requests.
- Customer offers.
- Acceptance records.
- Acquisition records.
- Inventory intake request, Confirmation, and reconciliation projections.
- Settlement records.
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

The following are required Trade-In Event concepts and do not replace the Event Catalog.

### Creation and Identity Event Concepts

- Trade-In created.
- Vehicle identity resolution requested.
- Existing Vehicle matched.
- New Vehicle identity created.
- Vehicle identity conflict detected.
- Duplicate Trade-In candidate detected.
- Duplicate Trade-In confirmed.

### Ownership and Document Event Concepts

- Ownership evidence requested.
- Ownership evidence received.
- Ownership verified.
- Ownership rejected.
- Co-owner approval required.
- Title conflict detected.
- Legal or compliance block applied.
- Legal or compliance block cleared.

### Inspection Event Concepts

- Trade-In inspection requested.
- Inspection scheduled.
- Inspection started.
- Inspection completed.
- Inspection failed.
- Reinspection required.
- Odometer discrepancy detected.
- Damage finding recorded.
- Inspection disputed.

### Payoff Event Concepts

- Payoff verification requested.
- Payoff statement received.
- Payoff verified.
- Payoff expired.
- Payoff conflict detected.
- Payoff settlement requested.
- Payoff settlement Command sent.
- Payoff settlement confirmed.
- Lien release received.
- Payoff reconciliation required.

### Appraisal Event Concepts

- Trade-In appraisal started.
- Valuation evidence collected.
- Trade-In appraisal calculated.
- Trade-In approval requested.
- Trade-In appraisal approved.
- Trade-In appraisal rejected.
- Trade-In appraisal expired.
- Trade-In appraisal superseded.
- Trade-In appraisal disputed.
- Trade-In revaluation required.

### Customer Offer Event Concepts

- Trade-In offer prepared.
- Trade-In offer presented.
- Trade-In offer delivered.
- Customer accepted Trade-In offer.
- Customer rejected Trade-In offer.
- Trade-In offer expired.
- Trade-In offer withdrawn.

### Acquisition Event Concepts

- Trade-In acquisition validation requested.
- Trade-In acquisition approved.
- Trade-In acquisition Command sent.
- Ownership transfer confirmed.
- Physical possession accepted.
- Trade-In acquired.
- Trade-In acquisition failed.
- Trade-In acquisition disputed.
- Trade-In acquisition reconciliation required.

### Inventory Intake Event Concepts

- Trade-In Inventory intake requested.
- Inventory Record creation requested.
- Inventory Record creation or activation pending.
- Inventory Record creation or activation observed.
- Inventory intake Confirmation received.
- Inventory intake failed.
- Inventory intake reconciliation required.
- Trade-In completed.

Trade-In Domain Service must not publish the authoritative Inventory Record-created or Inventory Record-activated Event.

Inventory Domain Service publishes accepted Inventory Record creation, activation, and lifecycle facts.

### Derived Intelligence Event Concepts

- Trade-In valuation Recommendation generated.
- Trade-In damage analysis generated.
- Trade-In fraud risk detected.
- Trade-In ownership conflict risk detected.
- Trade-In profitability projection updated.
- Trade-In Human Review recommended.

Derived Intelligence Events must not imply:

- Verified ownership.
- Verified title.
- Verified payoff.
- Approved appraisal.
- Customer acceptance.
- Acquisition.
- Inventory intake.
- Payment.
- Human Approval.
- External completion.

### Producer Rules

- Trade-In Domain Service publishes accepted canonical and workflow-state changes.
- Vehicle Domain Service publishes accepted Vehicle identity changes.
- Appointment Domain Service publishes accepted scheduling facts.
- Market Intelligence publishes governed market evidence.
- Payment, lender, title, DMS, and Inventory integrations publish normalized external observations.
- Inventory Domain Service publishes accepted Inventory Record facts.
- AI Agents may publish Agent-run, extraction, detection, valuation, or Recommendation Events.
- AI Agents must not publish authoritative ownership, appraisal, payoff, acquisition, or Inventory Confirmation Events merely because they recommended or predicted the outcome.

### Event Requirements

Every material Trade-In Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `trade_in_id`.
- `customer_id`.
- `opportunity_id`.
- `vehicle_id`.
- Appraisal identifier and version.
- Dealership and branch context.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Evidence references.
- Calculation reference.
- Applied policy.
- Human Decision.
- Automation-policy reference where applicable.
- Command.
- External Confirmation.
- Security classification.

Events are immutable.

Corrections, revaluation, supersession, dispute, cancellation, acquisition reversal, and reconciliation must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Vehicle-information extraction.
- VIN extraction.
- Registration-document extraction.
- Ownership-document classification.
- Inspection-image analysis.
- Damage detection.
- Damage-area classification.
- Repair-cost Recommendation.
- Odometer extraction.
- Vehicle-history summarization.
- Market-comparable retrieval.
- Market-comparable ranking.
- Valuation-range Recommendation.
- Reconditioning-cost Recommendation.
- Fraud-risk detection.
- Duplicate Trade-In detection.
- Document-completeness detection.
- Conflict detection.
- Appraisal-summary drafting.
- Customer-facing explanation drafting.
- Follow-up Recommendation.
- Acquisition-readiness analysis.
- Inventory-profitability projection.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create authoritative Vehicle identity.
- Verify VIN ownership.
- Verify legal ownership.
- Verify title.
- Verify lien.
- Verify lender payoff.
- Verify Customer authority.
- Approve condition.
- Approve actual cash value.
- Approve Customer allowance.
- Approve over-allowance.
- Approve negative-equity treatment.
- Clear legal or compliance blocks.
- Confirm Customer acceptance.
- Authorize acquisition.
- Confirm ownership transfer.
- Confirm payoff settlement.
- Confirm lien release.
- Create authoritative Inventory state.
- Mark the Trade-In acquired or completed.
- Execute external Commands directly.
- Access Trade-In data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Trade-In identifier and record version.
- Appraisal or inspection version where applicable.
- Supporting evidence.
- Source authority.
- Input freshness.
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

### Document Extraction

AI-extracted document fields must remain Derived Intelligence until accepted through the appropriate verification workflow.

AI must distinguish:

- Extracted text.
- Source document.
- Human-confirmed value.
- External authoritative value.
- Unreadable or missing field.
- Conflicting field.

AI must not transform a document extraction into verified legal ownership.

### Damage Detection

AI image analysis may identify potential:

- Dents.
- Scratches.
- Paint mismatch.
- Broken glass.
- Tire wear.
- Interior damage.
- Warning indicators.
- Missing parts.

AI results must preserve:

- Image reference.
- Image timestamp.
- Capture source.
- Detected region.
- Confidence where meaningful.
- Model version.
- Review status.

AI damage detection does not replace an authorized inspection.

### Valuation Recommendations

AI may produce valuation ranges and Recommendations.

The output must distinguish:

- Market reference.
- Wholesale reference.
- Auction reference.
- Retail reference.
- Condition adjustment.
- Mileage adjustment.
- Reconditioning assumption.
- Risk adjustment.
- Recommended actual cash value.
- Recommended Customer allowance.

The final approved valuation must be produced through deterministic calculations and Authoritative Human Decision where required.

A high-confidence model output does not create valuation authority.

### Fairness and Prohibited Inputs

AI valuation must not use protected Customer attributes or inappropriate proxies.

Valuation should be based on legitimate Vehicle and market factors such as:

- Vehicle identity.
- Model and specification.
- Condition.
- Mileage.
- History.
- Region.
- Market demand.
- Comparable evidence.
- Reconditioning cost.
- Commercial disposition strategy.

Customer identity or demographic characteristics must not change Vehicle valuation.

### Customer-Facing Drafting

AI may draft an appraisal explanation only when:

- Approved values are supplied.
- Customer-facing fields are identified.
- Restricted internal data is excluded.
- Required disclosures are supplied.
- Human Approval or applicable automation policy is satisfied.

AI must not invent:

- Value.
- Payoff.
- Ownership result.
- Title result.
- Tax treatment.
- Acquisition date.
- Inventory outcome.

### Action Class 2

Controlled document requests, inspection reminders, and Customer follow-up may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Customer permission.
- Purpose.
- Channel.
- Template.
- Frequency.
- Quiet hours.
- Trade-In status.
- Customer restrictions.
- Document sensitivity.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision.

Examples include:

- Actual cash value approval.
- Customer allowance approval.
- Over-allowance.
- Negative-equity exception.
- Payoff override.
- Ownership exception.
- Title exception.
- Structural or flood-damage acceptance.
- Acquisition approval.
- Acquisition reversal.
- Inventory intake exception.

### AI Context and Embeddings

Direct identifiers, legal documents, financial data, and sensitive Vehicle evidence must not enter unrestricted embeddings.

Normally excluded fields include:

- Customer name.
- Phone.
- Email.
- Address.
- National identifier.
- VIN where unrestricted retrieval is unnecessary.
- Registration number.
- Title number.
- Lender account information.
- Payoff instructions.
- Ownership documents.
- Identity documents.
- Power-of-attorney documents.
- Inspection media containing personal information.
- Exact Vehicle storage location.
- Fraud indicators.
- Approval thresholds.
- Internal actual cash value where not required.
- Internal margin and acquisition strategy.

Approved redacted context may include:

- Vehicle make, model, trim, and year.
- General mileage band.
- General condition.
- Damage categories.
- Market-comparable summary.
- Non-sensitive appraisal summary.
- Customer objection category.
- Reinspection requirement.

Every vector entry must enforce:

- `tenant_id`.
- Trade-In access scope.
- Source reference.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Explainability

Material Trade-In Recommendations must explain:

- Evidence used.
- Source authority.
- Data freshness.
- Inspection status.
- Vehicle identity status.
- Ownership and title state.
- Payoff state.
- Comparable-selection method.
- Adjustments.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Required Human authority.
- External Confirmation requirements.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Trade-In API behaviour.

### REST Resources

```text
GET    /api/v1/trade-ins
POST   /api/v1/trade-ins
GET    /api/v1/trade-ins/{trade_in_id}
PATCH  /api/v1/trade-ins/{trade_in_id}

POST   /api/v1/trade-ins/{trade_in_id}/identity-resolution-requests
POST   /api/v1/trade-ins/{trade_in_id}/inspection-requests
POST   /api/v1/trade-ins/{trade_in_id}/inspection-results
POST   /api/v1/trade-ins/{trade_in_id}/ownership-verification-requests
POST   /api/v1/trade-ins/{trade_in_id}/payoff-verification-requests
POST   /api/v1/trade-ins/{trade_in_id}/appraisal-requests
POST   /api/v1/trade-ins/{trade_in_id}/appraisal-version-requests
POST   /api/v1/trade-ins/{trade_in_id}/approval-requests
POST   /api/v1/trade-ins/{trade_in_id}/approval-decisions
POST   /api/v1/trade-ins/{trade_in_id}/offer-presentation-requests
POST   /api/v1/trade-ins/{trade_in_id}/acceptance-submissions
POST   /api/v1/trade-ins/{trade_in_id}/rejection-submissions
POST   /api/v1/trade-ins/{trade_in_id}/revalidation-requests
POST   /api/v1/trade-ins/{trade_in_id}/acquisition-requests
POST   /api/v1/trade-ins/{trade_in_id}/inventory-intake-requests
POST   /api/v1/trade-ins/{trade_in_id}/cancellation-requests
POST   /api/v1/trade-ins/{trade_in_id}/dispute-requests
POST   /api/v1/trade-ins/{trade_in_id}/correction-requests

GET    /api/v1/trade-ins/{trade_in_id}/history
GET    /api/v1/trade-ins/{trade_in_id}/inspection-history
GET    /api/v1/trade-ins/{trade_in_id}/appraisals
GET    /api/v1/trade-ins/{trade_in_id}/ownership-evidence
GET    /api/v1/trade-ins/{trade_in_id}/payoff-history
GET    /api/v1/trade-ins/{trade_in_id}/acquisition-history
GET    /api/v1/trade-ins/{trade_in_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, User, Inspector, Appraiser, and approval scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "trade_in_type": "PURCHASE_TRANSACTION",
  "workflow_authority_mode": "ASOS_AUTHORITATIVE",
  "submitted_vehicle": {
    "vin": "KMHJN81BP7U123456",
    "registration_number": "ABC-1234",
    "make": "Hyundai",
    "model": "Tucson",
    "model_year": 2020,
    "odometer_value": 85000,
    "odometer_unit": "KILOMETERS",
    "country_of_registration": "EG"
  },
  "customer_expected_value": {
    "amount": 700000,
    "currency_code": "EGP"
  }
}
```

The request must include:

```text
Idempotency-Key: 6fb46c66-d77f-44b8-9705-c54bbbde7b7c
```

### Example Create Response

```json
{
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "trade_in_number": "TI-2026-001245",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "status": "IDENTITY_RESOLUTION_PENDING",
  "vehicle_identity_resolution_status": "IN_PROGRESS",
  "inspection_status": "NOT_STARTED",
  "appraisal_status": "NOT_STARTED",
  "approval_status": "NOT_EVALUATED",
  "offer_status": "NOT_PREPARED",
  "acquisition_status": "NOT_STARTED",
  "inventory_intake_status": "NOT_STARTED",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T19:00:00Z"
}
```

### Example Inspection Result

```json
{
  "inspection_version": 1,
  "inspection_type": "PHYSICAL_COMPREHENSIVE",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "inspected_odometer_value": 85240,
  "inspected_odometer_unit": "KILOMETERS",
  "overall_condition": "GOOD",
  "mechanical_condition": "GOOD",
  "body_condition": "FAIR",
  "interior_condition": "GOOD",
  "accident_history_status": "REPORTED_MINOR",
  "odometer_verification_status": "VERIFIED",
  "damage_items": [
    {
      "area": "FRONT_BUMPER",
      "finding": "SCRATCH",
      "severity": "MINOR"
    }
  ],
  "inspection_report_reference": "evidence://trade-ins/a6cd98db/inspection/v1",
  "expected_record_version": 3
}
```

### Example Appraisal Request

```json
{
  "appraisal_method": "COMBINED",
  "currency_code": "EGP",
  "market_reference_value_amount": 720000,
  "wholesale_reference_value_amount": 610000,
  "auction_reference_value_amount": 600000,
  "estimated_reconditioning_amount": 35000,
  "requested_actual_cash_value_amount": 600000,
  "requested_trade_in_allowance_amount": 650000,
  "expected_inspection_version": 1,
  "expected_record_version": 5
}
```

The server must independently recalculate all governed values.

### Example Approval Response

```json
{
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "current_appraisal_id": "f31955ac-c4d2-4123-b8be-28fd1ec8767c",
  "current_appraisal_version": 1,
  "appraisal_status": "APPROVED",
  "approval_status": "APPROVED",
  "actual_cash_value_amount": 600000,
  "trade_in_allowance_amount": 650000,
  "over_allowance_amount": 50000,
  "verified_payoff_amount": 200000,
  "positive_equity_amount": 450000,
  "negative_equity_amount": 0,
  "currency_code": "EGP",
  "appraised_at": "2026-08-02T10:00:00Z",
  "appraisal_expires_at": "2026-08-09T10:00:00Z",
  "valuation_snapshot_hash": "sha256:84e7cd1f...",
  "record_version": 8
}
```

### Example Customer Acceptance Submission

```json
{
  "appraisal_id": "f31955ac-c4d2-4123-b8be-28fd1ec8767c",
  "appraisal_version": 1,
  "offer_version": 1,
  "offer_document_hash": "sha256:142f8bd9...",
  "acceptance_channel": "CUSTOMER_PORTAL",
  "acceptance_evidence_reference": "evidence://trade-ins/a6cd98db/acceptance/1",
  "expected_record_version": 10
}
```

### Example Acquisition Request

```json
{
  "deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "acquisition_decision_id": "d4a71438-dba3-4036-8725-95a9535878a1",
  "expected_appraisal_version": 1,
  "expected_offer_version": 1,
  "expected_record_version": 12
}
```

The request must use an idempotency key.

A response may remain pending:

```json
{
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "status": "ACQUISITION_PENDING",
  "acquisition_status": "PENDING_EXTERNAL_CONFIRMATION",
  "command_id": "c73bd7f6-931f-4a7e-87b8-47c29fb1af14",
  "record_version": 13
}
```

The API must not describe the Vehicle as acquired until authoritative conditions and Confirmations are satisfied.

### Example Inventory Intake Response

```json
{
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "inventory_intake_status": "INVENTORY_RECORD_CREATED",
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "inventory_intake_confirmation_status": "PENDING",
  "record_version": 16
}
```

The Trade-In remains incomplete until required Inventory and accounting Confirmations are reconciled.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Vehicle identity controls.
- Ownership and legal controls.
- Inspection controls.
- Deterministic calculation.
- Valuation and approval policy.
- Payoff freshness.
- Customer acceptance evidence.
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

- Trade-In records.
- Inspections.
- Appraisal versions.
- Approval requests.
- Customer offers.
- Acceptance records.
- Payoff payments.
- Acquisition attempts.
- Vehicle records.
- Inventory Records.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_TRADE_IN_REVIEW_REQUIRED`
- `VEHICLE_IDENTITY_AMBIGUOUS`
- `VEHICLE_IDENTITY_CONFLICT`
- `OWNERSHIP_EVIDENCE_REQUIRED`
- `OWNERSHIP_CONFLICT`
- `TITLE_BLOCKED`
- `LIEN_UNRESOLVED`
- `PAYOFF_REQUIRED`
- `PAYOFF_EXPIRED`
- `INSPECTION_REQUIRED`
- `INSPECTION_INCOMPLETE`
- `ODOMETER_CONFLICT`
- `VALUATION_EVIDENCE_STALE`
- `CALCULATION_FAILED`
- `CALCULATION_MISMATCH`
- `APPROVAL_REQUIRED`
- `APPROVAL_EXPIRED`
- `APPRAISAL_EXPIRED`
- `APPRAISAL_IMMUTABLE`
- `OFFER_EXPIRED`
- `ACCEPTANCE_EVIDENCE_INVALID`
- `ACQUISITION_NOT_READY`
- `HUMAN_APPROVAL_REQUIRED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `INVENTORY_RECORD_CONFLICT`
- `RECONCILIATION_REQUIRED`
- `TRADE_IN_COMPLETED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Vehicle identity governance.
- Ownership and payoff protection.
- Appraisal immutability.
- Deterministic calculations.
- Approval.
- Concurrency.
- Idempotency.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Trade-In Domain Service, deterministic calculation services, or Policy Engine controls.

---

## 11. Database Design

### Recommended Tables

```text
trade_ins
trade_in_submitted_vehicles
trade_in_identity_resolution
trade_in_ownership_verifications
trade_in_documents
trade_in_title_checks
trade_in_lien_records
trade_in_payoff_verifications
trade_in_payoff_settlements
trade_in_inspections
trade_in_inspection_findings
trade_in_inspection_media
trade_in_appraisals
trade_in_valuation_sources
trade_in_market_comparables
trade_in_calculations
trade_in_approval_requests
trade_in_approval_decisions
trade_in_offers
trade_in_acceptance_records
trade_in_rejection_records
trade_in_acquisitions
trade_in_inventory_intake
trade_in_settlements
trade_in_external_confirmations
trade_in_derived_intelligence
trade_in_reconciliation_cases
trade_in_data_quality_issues
trade_in_status_history
trade_in_record_versions
trade_in_audit_log
```

### Trade-Ins Table

The `trade_ins` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Customer and Opportunity relationships.
- Current Vehicle relationship.
- Current workflow status.
- Current identity-resolution projection.
- Current ownership, title, and lien projection.
- Current inspection projection.
- Current appraisal projection.
- Current offer and acceptance projection.
- Current acquisition projection.
- Current Inventory intake projection.
- Current accounting and settlement projection.
- Data-quality and conflict state.
- Source and synchronization status.
- Record version.
- Audit timestamps.

Historical and repeating information must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (trade_in_id)
```

### Tenant Protection

Every Trade-In-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_trade_ins_tenant_status
  (tenant_id, status)

idx_trade_ins_tenant_customer
  (tenant_id, customer_id)

idx_trade_ins_tenant_opportunity
  (tenant_id, opportunity_id)

idx_trade_ins_tenant_vehicle
  (tenant_id, vehicle_id)

idx_trade_ins_tenant_deal
  (tenant_id, deal_id)

idx_trade_ins_identity_review
  (tenant_id, vehicle_identity_resolution_status, review_status)

idx_trade_ins_inspection
  (tenant_id, inspection_status, inspection_scheduled_at)

idx_trade_ins_appraisal_expiry
  (tenant_id, appraisal_status, appraisal_expires_at)

idx_trade_ins_approval
  (tenant_id, approval_status, approval_requested_at)

idx_trade_ins_acquisition
  (tenant_id, acquisition_status)

idx_trade_ins_inventory_intake
  (tenant_id, inventory_intake_status)

idx_trade_ins_reconciliation
  (tenant_id, reconciliation_status)

idx_trade_ins_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, trade_in_number)
```

External reference uniqueness may use:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the external source guarantees uniqueness.

An active physical Vehicle should normally have only one materially overlapping Trade-In workflow.

This should be enforced through a partial uniqueness rule or controlled service logic using:

- `tenant_id`.
- `vehicle_id`.
- Active status set.
- Commercial journey context.

A Customer alone must not be used as a uniqueness constraint because one Customer may offer multiple Vehicles.

### Appraisal Storage

`trade_in_appraisals` should preserve:

- `appraisal_id`.
- `tenant_id`.
- `trade_in_id`.
- Appraisal version.
- Inspection version.
- Vehicle snapshot.
- Valuation inputs.
- Source references.
- Calculation reference.
- Proposed actual cash value.
- Approved actual cash value.
- Proposed Customer allowance.
- Approved Customer allowance.
- Payoff projection.
- Equity results.
- Approval status.
- Effective time.
- Expiry.
- Supersession.
- Snapshot hash.
- Record version.
- Related Events.

Approved and presented appraisal versions must be immutable.

### Inspection Storage

`trade_in_inspections` should preserve:

- Inspection identifier.
- Trade-In.
- Vehicle.
- Version.
- Type.
- Inspector.
- Location.
- Start and completion times.
- Odometer.
- Component condition.
- Damage.
- Missing items.
- Diagnostic results.
- Report.
- Media references.
- Integrity hash.
- Dispute state.
- Related Events.

### Ownership and Document Storage

Document and legal evidence tables should preserve:

- Document identifier.
- Trade-In.
- Vehicle.
- Document type.
- Source.
- Issuer.
- Effective date.
- Expiration.
- Verification state.
- Verification authority.
- Integrity hash.
- Controlled-storage reference.
- Security classification.
- Legal-hold state.
- Related Events.

Raw documents should not be stored directly in unrestricted relational fields.

### Payoff Storage

`trade_in_payoff_verifications` should preserve:

- Verification identifier.
- Trade-In.
- Lienholder.
- Masked account reference.
- Currency.
- Payoff amount.
- Daily interest.
- Statement date.
- Valid-until date.
- Source.
- Evidence.
- Verification status.
- External Confirmation.
- Related Events.

`trade_in_payoff_settlements` should preserve:

- Settlement identifier.
- Trade-In.
- Payment reference.
- Command.
- Idempotency key.
- Requested amount.
- Confirmed amount.
- Status.
- Confirmation.
- Lien release.
- Reconciliation state.
- Related Events.

### Approval Storage

`trade_in_approval_requests` and `trade_in_approval_decisions` should preserve:

- Request identifier.
- Trade-In.
- Appraisal version.
- Requested scope.
- Proposed values.
- Triggered policy.
- Required roles.
- Assigned approvers.
- Decisions.
- Reasons.
- Evidence.
- Effective period.
- Revocation.
- Related Events.

### Customer Offer Storage

`trade_in_offers` should preserve:

- Offer identifier.
- Trade-In.
- Appraisal version.
- Offer version.
- Customer allowance.
- Payoff projection.
- Equity projection.
- Validity.
- Document reference.
- Document hash.
- Presentation evidence.
- Delivery status.
- Customer response.
- Supersession.
- Withdrawal.
- Related Events.

### Acquisition Storage

`trade_in_acquisitions` should preserve:

- Acquisition identifier.
- Trade-In.
- Customer.
- Vehicle.
- Deal.
- Accepted appraisal and offer versions.
- Human Decision.
- Ownership-transfer evidence.
- Physical-possession evidence.
- Payoff state.
- Command.
- Idempotency key.
- External Confirmation.
- Acquisition status.
- Acquisition timestamp.
- Failure or dispute reason.
- Related Events.

### Inventory Intake Storage

`trade_in_inventory_intake` should preserve:

- Intake identifier.
- Trade-In.
- Vehicle.
- Receiving dealership.
- Branch.
- Location.
- Acquisition-cost projection.
- Inventory Record.
- Command.
- Idempotency key.
- External Confirmation.
- Status.
- Failure reason.
- Reconciliation state.
- Related Events.

### Derived Intelligence

Derived Trade-In records must remain separate from authoritative inspection, ownership, payoff, and valuation fields.

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

Trade-In audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw personal, legal, or financial values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Creation date.
- Acquisition date.
- Retention class.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Vehicle duplicate detection.
- Appraisal versioning.
- Referential integrity.
- Legal evidence.
- Audit integrity.

### Hard Deletion

A Trade-In must not be hard-deleted when referenced by:

- Customer journey.
- Lead.
- Qualified Lead.
- Opportunity.
- Appointment.
- Vehicle.
- Quotation.
- Finance Application.
- Financial Contract.
- Deal.
- Payment.
- Inventory Record.
- Interaction.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Legal evidence.
- Audit evidence.

Cancellation, archival, anonymization, governed redaction, or legal retention controls must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `CUSTOMER_IDENTIFIER_REFERENCE` | Customer and representative references |
| `VEHICLE_IDENTIFIER` | VIN, registration number, title reference |
| `OWNERSHIP_AND_LEGAL` | Ownership, title, co-owner, legal authority |
| `FINANCIAL_RESTRICTED` | Payoff, lender, equity, settlement |
| `COMMERCIAL_CONFIDENTIAL` | Actual cash value, allowance, acquisition cost |
| `INSPECTION_EVIDENCE` | Condition findings, damage, media |
| `FRAUD_AND_COMPLIANCE` | Theft, fraud, legal, and compliance reviews |
| `CUSTOMER_DOCUMENT` | Customer-facing Trade-In offer |
| `ACCEPTANCE_EVIDENCE` | Customer acceptance identity and document hash |
| `DERIVED_INTELLIGENCE` | AI valuation, damage, and risk outputs |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, history |

### Authentication

Every internal Trade-In operation requires an authenticated Human or service identity.

Customer document submission or offer acceptance must use an approved secure verification mechanism.

Anonymous unrestricted access to Trade-In records or documents is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Team.
- Responsible User.
- Inspector.
- Appraiser.
- Role.
- Requested field.
- Requested action.
- Trade-In state.
- Vehicle relationship.
- Customer relationship.
- Monetary threshold.
- Data classification.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access permitted:

- Customer Trade-In intake.
- Submitted Vehicle information.
- Appointment coordination.
- Customer-facing allowance.
- Offer presentation.
- Customer response.
- Follow-up.

Must not independently:

- Verify ownership.
- Verify title.
- Verify payoff.
- Approve actual cash value.
- Approve over-allowance.
- Clear legal blocks.
- Authorize acquisition.
- Mark Inventory intake complete.

#### Inspector

May access:

- Vehicle identity required for inspection.
- Inspection schedule.
- Condition fields.
- Odometer.
- Inspection media.
- Inspection report.

Inspector access does not automatically authorize:

- Ownership verification.
- Appraisal approval.
- Customer allowance.
- Acquisition.

#### Appraiser

May access permitted:

- Inspection evidence.
- Market data.
- Valuation inputs.
- Appraisal preparation.
- Actual cash value Recommendation or approval according to delegated authority.

#### Sales Manager

May access permitted:

- Customer allowance.
- Over-allowance.
- Commercial exceptions.
- Customer disputes.
- Acquisition review.

Manager access does not automatically authorize:

- Legal title exception.
- Payoff verification.
- Lien release.
- Payment Confirmation.
- Cross-Tenant access.

#### Finance Specialist

May access only the payoff, equity, settlement, and Deal information required for assigned responsibilities.

#### Inventory User

May access acquired Vehicle and intake context required to create the Inventory Record.

Inventory access does not permit appraisal or ownership changes.

#### Compliance or Legal Reviewer

May access:

- Ownership.
- Title.
- Co-owner evidence.
- Power of attorney.
- Legal restrictions.
- Theft or fraud review.
- Acquisition exception evidence.

#### Data Steward

May review:

- Duplicate Vehicle matches.
- Duplicate Trade-Ins.
- Source conflicts.
- Relationship inconsistencies.
- Data-quality issues.
- Reconciliation cases.

#### AI Agent

May access only the minimum Trade-In context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to ownership documents, lender details, payoff instructions, fraud evidence, approval thresholds, and legal documents.

### Document Security

Ownership, registration, title, lender, identity, and representative documents must:

- Use controlled storage.
- Use encryption.
- Use authenticated access.
- Preserve integrity hashes.
- Preserve provenance.
- Prevent predictable identifiers.
- Prevent unrestricted download.
- Be excluded from public search.
- Be excluded from ordinary Logs.
- Follow retention and legal-hold policies.

### VIN and Registration Protection

VIN and registration identifiers must:

- Be treated as controlled Vehicle identifiers.
- Be masked where full display is unnecessary.
- Be excluded from unrestricted analytics.
- Be excluded from unrestricted embeddings.
- Be protected against bulk export.
- Be disclosed only for approved business purposes.

### Payoff and Lender Protection

Payoff and lender information must use:

- Field-level authorization.
- Encryption or tokenization.
- Masked account references.
- Controlled Payment instructions.
- Export restrictions.
- Audit logging.
- AI-context restrictions.

Payoff instructions must never be copied into Prompts, ordinary Logs, or unrestricted documents.

### Valuation and Margin Protection

Restricted commercial values must not appear in:

- Customer-facing documents without authorization.
- Public APIs.
- Public search indexes.
- Unrestricted Logs.
- General-purpose embeddings.
- Unauthorized exports.

Restricted examples include:

- Actual cash value.
- Wholesale target.
- Reconditioning estimate.
- Acquisition cost.
- Internal margin.
- Approval thresholds.
- Fraud adjustments.
- Internal disposition strategy.

### Inspection Media Protection

Inspection media must:

- Use controlled storage.
- Remove or restrict unnecessary personal information.
- Preserve capture source and timestamp.
- Preserve integrity metadata.
- Restrict location metadata where sensitive.
- Follow retention requirements.
- Prevent unapproved AI training use.
- Prevent public indexing.

### Customer Communication Security

Before document requests, inspection reminders, offer delivery, or follow-up, deterministic controls must validate:

- Customer permission.
- Purpose.
- Channel.
- Template.
- Trade-In state.
- Document sensitivity.
- Frequency.
- Quiet hours.
- Customer restrictions.
- Human Approval or approved automation policy.

Prompt text, User interface state, Trade-In priority, or AI Recommendation must not bypass these controls.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Duplicate detection.
- Vehicle matching.
- Search.
- Valuation.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Documents.
- Inspection media.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Trade-In Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Requested action.
- Trade-In identifier.
- Appraisal or offer version where applicable.
- Current record version.
- Field-level write authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Trade-In activity must record:

- `tenant_id`.
- `trade_in_id`.
- Customer, Opportunity, Vehicle, Deal, and Inventory references.
- Appraisal and inspection versions.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
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

- Cross-Tenant Trade-In access attempts.
- Unauthorized VIN or title access.
- Ownership-document access.
- Payoff instruction access.
- Unauthorized appraisal changes.
- Allowance manipulation.
- Approval bypass.
- Odometer manipulation.
- Inspection evidence alteration.
- False Customer acceptance.
- False acquisition recording.
- Duplicate Inventory creation.
- Command replay.
- External Confirmation mismatch.
- AI access outside approved scope.
- Audit-log tampering.
- Suspicious bulk Trade-In export.

### Commercial and Legal Integrity

The platform must detect or prevent:

- Duplicate active Trade-Ins.
- Duplicate Vehicle identities.
- Appraisal modification after approval.
- Customer allowance without authority.
- Undisclosed over-allowance.
- Stale payoff use.
- Expired appraisal use.
- Ownership transfer without evidence.
- Acquisition without Customer authority.
- Inventory intake before acquisition.
- False lien release.
- False accounting completion.
- Trade-In status manipulation.

### Privacy and Retention

Trade-In retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Vehicle title and registration obligations.
- Financial and lender requirements.
- Tax and accounting obligations.
- Related Deal and contract obligations.
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
- Media stores.
- Backups according to policy.

Required non-personal legal, financial, security, accounting, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Trade-In appraisal.
- Customer offer delivery.
- Automated follow-up.
- Payoff Commands.
- Acquisition.
- Inventory intake.
- External write-back.
- AI valuation.
- AI document extraction.
- Trade-In export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Deal](./Deal.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Trade-In baseline.

Trade-In remains separate from Vehicle identity, Quotation, Deal, Payment, and Inventory Record.

Trade-In requests and tracks Inventory intake; Inventory Domain Service owns authoritative Inventory Record creation, activation, and lifecycle.

Customer acceptance of a Trade-In offer does not transfer legal ownership.

A Vehicle becomes dealership Inventory only after governed acquisition and Inventory intake.

Approved or presented appraisal versions are immutable.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
