# Quotation

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Quotation Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Quotation Object represents one governed, versioned, time-bound commercial offer prepared for a specific Customer within an automotive sales journey.

A Quotation may present:

- A specific Vehicle.
- A specific Inventory Record.
- A factory-order configuration.
- A permitted alternative Vehicle scenario.
- Vehicle price.
- Discounts.
- Rebates.
- Incentives.
- Taxes.
- Registration charges.
- Dealership fees.
- Delivery charges.
- Optional products.
- Accessories.
- Service plans.
- Warranty products.
- Insurance-related products where permitted.
- Trade-In allowance.
- Trade-In payoff.
- Customer cash requirement.
- Finance assumptions.
- Lease assumptions.
- Payment schedule assumptions.
- Validity period.
- Customer-facing terms and disclosures.

The Quotation provides a controlled commercial basis for:

- Customer presentation.
- Scenario comparison.
- Negotiation.
- Approval workflows.
- Discount governance.
- Margin protection.
- Customer acceptance or rejection.
- Opportunity progression.
- Deal-conversion validation.
- Commercial analytics.
- Audit and dispute resolution.

### Quotation Domain Boundary

A Quotation is a formal commercial offer.

It is not independently:

- A Vehicle Reservation.
- A Vehicle Allocation.
- An Inventory ownership record.
- A lender Decision.
- A finance approval.
- A signed finance agreement.
- A sales contract.
- A Payment receipt.
- Cleared funds.
- A completed Deal.
- A confirmed Vehicle sale.
- A confirmed delivery.
- An accounting entry.

Those facts belong to their appropriate Canonical Domain Objects and configured authoritative systems.

### Quotation and Opportunity Separation

`Opportunity` represents the active commercial pursuit.

`Quotation` represents one specific versioned commercial offer produced during that pursuit.

An Opportunity may have:

- No formal Quotation.
- One Quotation series.
- Multiple competing Quotation series.
- Multiple versions within one series.
- Alternative Vehicle or payment scenarios.

The Opportunity may project the current Quotation state.

It must not own the authoritative issued commercial snapshot.

### Quotation Series and Version Separation

A Quotation series represents the continuing commercial offer context.

A Quotation version represents one immutable commercial snapshot inside that series.

The model must distinguish:

```text
quotation_series_id
  = stable identifier for the commercial offer series

quotation_id
  = identifier for one specific Quotation version

quotation_version
  = sequential version number inside the series
```

A Draft version may be edited under concurrency controls.

Once a Quotation is issued, its commercial and Customer-facing content becomes immutable.

Any material change after issue requires a new version.

Material changes include:

- Vehicle.
- Inventory Record.
- Vehicle price.
- Discount.
- Rebate.
- Incentive.
- Fee.
- Tax.
- Optional product.
- Trade-In figure.
- Finance assumption.
- Lease assumption.
- Customer-facing term.
- Expiration.
- Currency.
- Payment structure.
- Required disclosure.

The new version must supersede the previous version without deleting or rewriting it.

### Quotation and Inventory Separation

A Quotation may reference:

- A Vehicle configuration.
- A physical Inventory Record.
- A future factory order.
- A permitted non-stock commercial product.

Quotation does not make the Vehicle available.

Quotation does not reserve or allocate the Vehicle.

Vehicle availability, location, readiness, Reservation, and Allocation remain governed by Inventory Record or its configured authoritative system.

Availability must be revalidated before:

- Issue.
- Customer acceptance.
- Deal conversion.
- Another configured critical point.

### Quotation and Finance Separation

A Quotation may include finance or lease illustrations.

Unless supported by authoritative lender evidence, these values are assumptions or estimates.

A Quotation must not present estimated:

- Interest rate.
- Installment.
- Term.
- Down payment.
- Balloon payment.
- Finance principal.
- Approval probability.

as a confirmed lender Decision.

Authoritative finance status belongs to Finance Application and the lender or F&I platform.

Signed finance terms belong to Financial Contract.

### Quotation and Trade-In Separation

A Quotation may include Trade-In projections such as:

- Approved appraisal value.
- Customer allowance.
- Verified payoff.
- Net equity.
- Negative equity.
- Trade-In tax treatment.

Trade-In identity, condition, ownership, lien, payoff, appraisal, and acquisition approval remain governed by Trade-In and its configured authoritative sources.

### Quotation and Deal Separation

Customer acceptance of a Quotation means the Customer accepted that specific offer version under the accepted evidence policy.

Acceptance does not independently prove:

- Deal creation.
- Contract signature.
- Payment.
- Finance approval.
- Vehicle Reservation.
- Vehicle Allocation.
- Sale.
- Delivery.

An accepted Quotation may support a controlled and idempotent Deal-conversion workflow.

The Deal must preserve the exact Quotation version and commercial snapshot used.

### System Purpose

The Quotation Object provides the canonical commercial-offer context used by:

- Opportunity workflows.
- Pricing workflows.
- Approval workflows.
- Inventory validation.
- Trade-In workflows.
- Finance Application workflows.
- Customer communications.
- Document generation.
- Deal creation.
- AI Agents.
- Analytics.
- Audit and compliance services.

The Quotation Object may contain:

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
| Opportunity context | Opportunity Domain Service |
| Vehicle identity and specifications | Vehicle Domain Service |
| Inventory availability and Reservation | Inventory Domain Service or configured external authority |
| Approved list or advertised price | Configured pricing authority |
| Discount authority | Approved Business Rules and Authorized Human |
| Tax calculation | Approved deterministic tax service |
| Statutory fees | Approved authority or deterministic rules |
| Trade-In appraisal and payoff | Trade-In and approved external sources |
| Finance Decision | Lender or F&I platform |
| Canonical Quotation workflow | Quotation Domain Service |
| Issued commercial snapshot | Quotation Domain Service |
| Customer acceptance evidence | Customer action, Interaction, signature, or approved provider evidence |
| Communication delivery | Communication provider |
| Deal transaction state | Deal Domain Service and configured external authority |
| Predictions and Recommendations | Derived Intelligence |
| External action completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `quotation_id` — UUIDv4, required and immutable.
- `quotation_series_id` — UUIDv4, required and immutable within the series.
- `tenant_id` — UUIDv4, required and immutable.
- `quotation_version` — Integer, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `department_id`.
- `sales_team_id`.
- `responsible_user_id`.
- `commercial_owner_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `opportunity_id`.
- `customer_id`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `finance_application_id`.
- `financial_contract_id`.
- `accepted_deal_id`.
- `source_quotation_id`.
- `supersedes_quotation_id`.
- `superseded_by_quotation_id`.
- `primary_interaction_id`.

### Quotation Identity

- `quotation_number`.
- `quotation_series_reference`.
- `quotation_version`.
- `scenario_name`.
- `scenario_type`.
- `status`.
- `approval_status`.
- `issue_status`.
- `delivery_status`.
- `view_status`.
- `acceptance_status`.
- `deal_conversion_status`.
- `data_quality_status`.
- `conflict_status`.
- `is_current_version`.

### Commercial Context

- `commercial_purpose`.
- `payment_method`.
- `currency_code`.
- `pricing_date`.
- `pricing_authority_reference`.
- `pricing_rule_id`.
- `pricing_rule_version`.
- `tax_rule_id`.
- `tax_rule_version`.
- `fee_rule_id`.
- `fee_rule_version`.
- `incentive_program_references`.
- `commercial_terms_reference`.

### Vehicle Context

- `vehicle_id`.
- `inventory_record_id`.
- `vehicle_snapshot`.
- `inventory_snapshot`.
- `vehicle_condition`.
- `vehicle_list_price_amount`.
- `vehicle_base_price_amount`.
- `vehicle_selling_price_amount`.
- `vehicle_availability_status`.
- `vehicle_availability_confirmed_at`.
- `vehicle_availability_expires_at`.
- `vehicle_pricing_confirmed_at`.

The Vehicle snapshot must preserve the presented identity and specification context.

The Inventory snapshot must preserve the presented stock context without becoming the authoritative Inventory state.

### Commercial Line Items

- `line_items`.
- `vehicle_line_items`.
- `accessory_line_items`.
- `optional_product_line_items`.
- `service_line_items`.
- `warranty_line_items`.
- `insurance_line_items`.
- `fee_line_items`.
- `tax_line_items`.
- `discount_line_items`.
- `rebate_line_items`.
- `incentive_line_items`.
- `trade_in_line_items`.
- `finance_line_items`.
- `adjustment_line_items`.

Every line item must identify:

- `line_item_id`.
- `line_item_type`.
- `description`.
- `quantity`.
- `unit_amount`.
- `gross_amount`.
- `discount_amount`.
- `taxable_amount`.
- `tax_amount`.
- `net_amount`.
- `currency_code`.
- `authority_reference`.
- `rule_reference`.
- `customer_visible`.
- `internal_only`.
- `effective_from`.
- `effective_until`.

### Discounts, Rebates, and Incentives

- `vehicle_discount_amount`.
- `dealer_discount_amount`.
- `manufacturer_rebate_amount`.
- `dealer_incentive_amount`.
- `campaign_incentive_amount`.
- `loyalty_incentive_amount`.
- `fleet_incentive_amount`.
- `finance_incentive_amount`.
- `other_incentive_amount`.
- `total_discount_amount`.
- `discount_authority_reference`.
- `incentive_eligibility_status`.
- `incentive_evidence_references`.

Discounts and external incentives must remain distinguishable.

### Trade-In Context

- `trade_in_id`.
- `trade_in_status`.
- `trade_in_actual_cash_value_amount`.
- `trade_in_allowance_amount`.
- `trade_in_payoff_amount`.
- `trade_in_net_equity_amount`.
- `trade_in_negative_equity_amount`.
- `trade_in_tax_credit_amount`.
- `trade_in_currency_code`.
- `trade_in_value_confirmed_at`.
- `trade_in_payoff_confirmed_at`.
- `trade_in_evidence_references`.

### Finance and Lease Assumptions

- `finance_application_id`.
- `finance_assumption_status`.
- `finance_program_reference`.
- `lender_reference`.
- `down_payment_amount`.
- `deposit_amount`.
- `finance_principal_amount`.
- `estimated_interest_rate`.
- `interest_rate_type`.
- `finance_term_months`.
- `estimated_periodic_payment_amount`.
- `payment_frequency`.
- `balloon_payment_amount`.
- `residual_value_amount`.
- `lease_mileage_allowance`.
- `lease_excess_mileage_amount`.
- `finance_fee_amount`.
- `finance_assumption_generated_at`.
- `finance_assumption_expires_at`.
- `finance_disclosure_reference`.

These fields must be clearly marked as estimated unless supported by authoritative lender evidence.

### Taxes and Fees

- `registration_fee_amount`.
- `documentation_fee_amount`.
- `delivery_fee_amount`.
- `licensing_fee_amount`.
- `inspection_fee_amount`.
- `insurance_amount`.
- `administrative_fee_amount`.
- `statutory_fee_amount`.
- `other_fee_amount`.
- `total_fee_amount`.
- `total_tax_amount`.
- `tax_jurisdiction_reference`.
- `tax_calculation_reference`.
- `fee_calculation_reference`.

Fees must be itemized and supported by approved authority.

### Totals

- `vehicle_subtotal_amount`.
- `optional_products_total_amount`.
- `service_total_amount`.
- `warranty_total_amount`.
- `insurance_total_amount`.
- `trade_in_credit_total_amount`.
- `total_discount_amount`.
- `total_rebate_amount`.
- `total_incentive_amount`.
- `total_fee_amount`.
- `total_tax_amount`.
- `subtotal_amount`.
- `total_due_amount`.
- `customer_cash_due_amount`.
- `finance_principal_amount`.
- `estimated_total_cost_amount`.
- `estimated_gross_profit_amount`.
- `estimated_gross_margin_percentage`.

All totals must be calculated deterministically.

### Foreign Exchange Context

- `source_currency_code`.
- `settlement_currency_code`.
- `exchange_rate`.
- `exchange_rate_source`.
- `exchange_rate_timestamp`.
- `exchange_rate_expires_at`.
- `foreign_exchange_disclosure_reference`.

A Quotation should normally use one settlement currency.

Where conversion is permitted, the rate, source, timestamp, and expiry must be preserved.

### Approval Context

- `approval_required`.
- `approval_status`.
- `approval_reason_codes`.
- `approval_policy_id`.
- `approval_policy_version`.
- `approval_request_id`.
- `approval_requested_at`.
- `approval_decision_id`.
- `approved_at`.
- `approved_by_actor_type`.
- `approved_by_actor_id`.
- `rejected_at`.
- `rejected_by_actor_id`.
- `approval_rejection_reason`.
- `approval_evidence_references`.
- `approval_expires_at`.

Approval may involve multiple governed approval steps.

The Quotation must preserve the completed approval chain.

### Customer-Facing Presentation

- `customer_message`.
- `customer_language`.
- `terms_and_conditions`.
- `commercial_disclosures`.
- `finance_disclosures`.
- `tax_disclosures`.
- `trade_in_disclosures`.
- `optional_product_disclosures`.
- `document_template_id`.
- `document_template_version`.
- `presentation_snapshot`.
- `rendered_document_reference`.
- `document_hash`.
- `document_signature_reference`.
- `document_generated_at`.

### Validity

- `valid_from`.
- `expires_at`.
- `validity_policy_id`.
- `validity_policy_version`.
- `validity_status`.
- `expiry_reason`.
- `expired_at`.
- `revalidation_required`.

Quotation validity periods must remain configurable.

A fixed default validity duration must not be embedded in the Canonical Domain Model.

### Issue Context

- `issue_status`.
- `issued_at`.
- `issued_by_actor_type`.
- `issued_by_actor_id`.
- `issued_document_hash`.
- `issued_record_version`.
- `issue_authority_reference`.
- `issue_evidence_references`.

`ISSUED` means the immutable Customer-facing offer version was created.

It does not prove Customer delivery.

### Customer Delivery Context

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
- `delivery_attempt_count`.

A sent Command or provider acknowledgment must not be represented as confirmed delivery unless the configured evidence policy supports it.

### Customer View Context

- `view_status`.
- `first_viewed_at`.
- `last_viewed_at`.
- `view_count`.
- `view_evidence_references`.

Viewing evidence does not prove acceptance.

### Customer Acceptance and Rejection

- `acceptance_status`.
- `acceptance_requested_at`.
- `accepted_at`.
- `accepted_by_customer_id`.
- `acceptance_channel`.
- `accepted_document_hash`.
- `accepted_quotation_version`.
- `acceptance_interaction_id`.
- `acceptance_evidence_references`.
- `acceptance_confirmation_status`.
- `rejected_at`.
- `rejection_reason`.
- `rejection_details`.
- `rejection_evidence_references`.

Customer acceptance must reference the exact issued document hash and version.

### Supersession

- `supersedes_quotation_id`.
- `superseded_by_quotation_id`.
- `superseded_at`.
- `supersession_reason`.
- `supersession_evidence_references`.

Supersession must preserve the complete prior Quotation.

### Withdrawal

- `withdrawal_status`.
- `withdrawal_requested_at`.
- `withdrawn_at`.
- `withdrawal_reason`.
- `withdrawal_details`.
- `withdrawn_by_actor_type`.
- `withdrawn_by_actor_id`.
- `withdrawal_notification_status`.
- `withdrawal_evidence_references`.

Withdrawal does not delete or rewrite an issued Quotation.

### Deal Conversion

- `deal_conversion_status`.
- `accepted_deal_id`.
- `deal_conversion_requested_at`.
- `deal_conversion_requested_by_actor_id`.
- `deal_conversion_command_id`.
- `deal_conversion_idempotency_key`.
- `deal_conversion_confirmation_status`.
- `deal_conversion_completed_at`.
- `deal_conversion_failure_reason`.
- `deal_conversion_evidence_references`.

### Commercial Snapshot and Provenance

- `commercial_snapshot`.
- `calculation_snapshot`.
- `authority_snapshot`.
- `approval_snapshot`.
- `customer_presentation_snapshot`.
- `source_snapshot`.
- `input_record_versions`.
- `snapshot_hash`.
- `field_authority_map`.

### Derived Intelligence

- `recommended_discount_amount`.
- `recommended_optional_products`.
- `recommended_payment_scenario`.
- `recommended_validity_period`.
- `acceptance_probability`.
- `price_sensitivity_score`.
- `margin_risk_score`.
- `quotation_competitiveness_score`.
- `recommended_next_action`.
- `recommended_follow_up_at`.
- `requires_human_review`.
- `derived_intelligence_expires_at`.

Every material derived output must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Freshness.
- Confidence where meaningful.
- Assumptions.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required authority.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `external_quotation_id`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `reconciliation_status`.
- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `quotation_id` | UUID | Yes | ASOS | Immutable identifier for one Quotation version. |
| `quotation_series_id` | UUID | Yes | ASOS | Stable identifier shared by versions in one series. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `quotation_version` | Integer | Yes | ASOS | Sequential version inside the Quotation series. |
| `quotation_number` | String | Yes | ASOS or configured external authority | Human-readable commercial reference. |
| `opportunity_id` | UUID | Yes | Canonical relationship | Opportunity for which the offer was prepared. |
| `customer_id` | UUID | Yes | Canonical relationship | Customer receiving the offer. |
| `vehicle_id` | UUID | Conditional | Canonical relationship | Vehicle configuration or physical Vehicle included in the offer. |
| `inventory_record_id` | UUID | Conditional | Inventory relationship | Specific physical stock record where applicable. |
| `status` | Enum | Yes | Quotation workflow | Current Quotation-version lifecycle state. |
| `approval_status` | Enum | Yes | Approval workflow | Current commercial approval state. |
| `is_current_version` | Boolean | Yes | ASOS | Identifies the active version in the series. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Commercial Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `currency_code` | String | Yes | Commercial authority | ISO 4217 settlement currency. |
| `vehicle_list_price_amount` | Decimal | Conditional | Pricing authority | Approved base or list price. |
| `vehicle_selling_price_amount` | Decimal | Yes | Approved commercial calculation | Proposed Vehicle price before permitted additional items. |
| `vehicle_discount_amount` | Decimal | Yes | Approved commercial authority | Approved Vehicle-level discount. |
| `manufacturer_rebate_amount` | Decimal | Yes | OEM or approved program | Manufacturer-funded Customer rebate. |
| `dealer_incentive_amount` | Decimal | Yes | Dealership authority | Dealership-funded approved incentive. |
| `total_discount_amount` | Decimal | Yes | Deterministic calculation | Sum of approved discount components. |
| `total_rebate_amount` | Decimal | Yes | Deterministic calculation | Sum of approved rebate components. |
| `total_incentive_amount` | Decimal | Yes | Deterministic calculation | Sum of approved incentive components. |
| `subtotal_amount` | Decimal | Yes | Deterministic calculation | Commercial subtotal before final taxes and fees. |
| `total_fee_amount` | Decimal | Yes | Deterministic calculation | Sum of permitted fee lines. |
| `total_tax_amount` | Decimal | Yes | Deterministic tax service | Total applicable tax. |
| `total_due_amount` | Decimal | Yes | Deterministic calculation | Total amount proposed under the Quotation. |
| `customer_cash_due_amount` | Decimal | Yes | Deterministic calculation | Expected Customer cash contribution. |
| `estimated_gross_profit_amount` | Decimal | No | Restricted deterministic or derived calculation | Internal estimated gross profit. |
| `estimated_gross_margin_percentage` | Decimal | No | Restricted deterministic calculation | Internal estimated margin percentage. |

### Trade-In Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `trade_in_id` | UUID | No | Canonical relationship | Trade-In workflow supporting the offer. |
| `trade_in_actual_cash_value_amount` | Decimal | No | Trade-In authority | Approved wholesale or acquisition valuation. |
| `trade_in_allowance_amount` | Decimal | No | Approved commercial authority | Customer-facing Trade-In allowance. |
| `trade_in_payoff_amount` | Decimal | No | Verified external evidence | Outstanding payoff amount. |
| `trade_in_net_equity_amount` | Decimal | No | Deterministic calculation | Positive equity included in the offer. |
| `trade_in_negative_equity_amount` | Decimal | No | Deterministic calculation | Negative equity included in the offer. |
| `trade_in_value_confirmed_at` | Timestamp | No | Trade-In authority | Time the appraisal value was confirmed. |
| `trade_in_payoff_confirmed_at` | Timestamp | No | External Confirmation | Time the payoff was authoritatively confirmed. |

### Finance Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `payment_method` | Enum | Yes | Customer evidence or approved workflow | Proposed payment route. |
| `down_payment_amount` | Decimal | No | Customer input or approved scenario | Proposed Customer down payment. |
| `deposit_amount` | Decimal | No | Payment projection | Proposed or externally confirmed deposit projection. |
| `finance_principal_amount` | Decimal | No | Deterministic calculation | Proposed financed principal. |
| `estimated_interest_rate` | Decimal | No | Lender program or assumption | Non-binding rate unless supported by an authoritative Decision. |
| `finance_term_months` | Integer | No | Lender program or assumption | Proposed finance term. |
| `estimated_periodic_payment_amount` | Decimal | No | Deterministic calculation | Estimated periodic payment. |
| `balloon_payment_amount` | Decimal | No | Lender program or assumption | Proposed balloon amount. |
| `finance_assumption_status` | Enum | Yes | Finance projection | Authority and validity of finance assumptions. |
| `finance_assumption_expires_at` | Timestamp | No | Finance source or policy | Expiration time of the assumptions. |

### Approval Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `approval_required` | Boolean | Yes | Deterministic policy | Indicates whether approval is required. |
| `approval_status` | Enum | Yes | Approval workflow | Current approval state. |
| `approval_reason_codes` | Array | No | Deterministic policy | Reasons requiring approval. |
| `approval_policy_id` | String | No | Policy Engine | Applied approval policy. |
| `approval_policy_version` | String | No | Policy Engine | Applied approval-policy version. |
| `approval_decision_id` | UUID | No | Human Decision | Accepted approval Decision. |
| `approved_at` | Timestamp | No | ASOS | Time all required approvals were completed. |
| `approved_by_actor_id` | UUID | No | Authorized Human | Final or primary approving actor. |
| `approval_expires_at` | Timestamp | No | Policy | Time approval must be revalidated. |

### Document and Validity Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `valid_from` | Timestamp | Conditional | Quotation workflow | Earliest time the issued offer may be accepted. |
| `expires_at` | Timestamp | Conditional | Approved validity policy | Acceptance expiration time. |
| `document_template_id` | String | Conditional | Document service | Approved Customer-facing template. |
| `document_template_version` | String | Conditional | Document service | Template version used. |
| `rendered_document_reference` | String | Conditional | Controlled document storage | Reference to the rendered issued document. |
| `document_hash` | String | Conditional | ASOS | Cryptographic hash of the issued document and snapshot. |
| `issued_at` | Timestamp | No | Quotation workflow | Time the immutable Quotation was issued. |
| `issued_record_version` | Integer | No | ASOS | Record version frozen at issue. |

### Customer Response Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `delivery_status` | Enum | Yes | Delivery projection | Current delivery state of the issued offer. |
| `delivered_at` | Timestamp | No | Provider evidence | Time Customer delivery was confirmed. |
| `view_status` | Enum | Yes | Customer-access projection | Current document-view state. |
| `first_viewed_at` | Timestamp | No | Customer-access evidence | First accepted Customer view time. |
| `acceptance_status` | Enum | Yes | Acceptance workflow | Current Customer acceptance state. |
| `accepted_at` | Timestamp | No | Customer evidence | Time valid Customer acceptance occurred. |
| `accepted_document_hash` | String | No | Acceptance evidence | Exact issued document hash accepted. |
| `accepted_quotation_version` | Integer | No | Acceptance evidence | Exact Quotation version accepted. |
| `rejected_at` | Timestamp | No | Customer evidence | Time Customer rejected the version. |
| `rejection_reason` | Enum | No | Customer or Human evidence | Standard rejection reason where known. |

### Conversion Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `deal_conversion_status` | Enum | Yes | Conversion workflow | Current Quotation-to-Deal conversion state. |
| `accepted_deal_id` | UUID | No | Deal relationship | Deal created from this accepted Quotation version. |
| `deal_conversion_idempotency_key` | String | No | Command workflow | Retry-protection key. |
| `deal_conversion_confirmation_status` | Enum | Yes | Workflow projection | External Confirmation state where applicable. |
| `deal_conversion_completed_at` | Timestamp | No | ASOS or external authority | Time conversion was completed. |

---

## 4. Enumerations

### QuotationStatus

- `DRAFT`
- `APPROVAL_PENDING`
- `APPROVED`
- `ISSUED`
- `ACCEPTED`
- `REJECTED`
- `EXPIRED`
- `SUPERSEDED`
- `WITHDRAWAL_PENDING`
- `WITHDRAWN`
- `ARCHIVED`

### QuotationScenarioType

- `PRIMARY_OFFER`
- `ALTERNATIVE_VEHICLE`
- `ALTERNATIVE_INVENTORY`
- `CASH_SCENARIO`
- `FINANCE_SCENARIO`
- `LEASE_SCENARIO`
- `TRADE_IN_SCENARIO`
- `FLEET_SCENARIO`
- `FACTORY_ORDER_SCENARIO`
- `OPTIONAL_PRODUCT_SCENARIO`
- `OTHER`

### CommercialPurpose

- `VEHICLE_PURCHASE`
- `VEHICLE_AND_TRADE_IN`
- `VEHICLE_FINANCE`
- `VEHICLE_LEASE`
- `FLEET_PURCHASE`
- `FACTORY_ORDER`
- `WHOLESALE`
- `OTHER`

### PaymentMethod

- `CASH`
- `BANK_TRANSFER`
- `FINANCE`
- `LEASE`
- `MIXED`
- `UNDECIDED`

### ApprovalStatus

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

### IssueStatus

- `NOT_READY`
- `READY`
- `ISSUING`
- `ISSUED`
- `FAILED`
- `RECONCILIATION_REQUIRED`

### QuotationDeliveryStatus

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

### QuotationViewStatus

- `NOT_VIEWED`
- `VIEWED`
- `VIEW_EVIDENCE_CONFLICTED`
- `UNKNOWN`

### QuotationAcceptanceStatus

- `NOT_REQUESTED`
- `PENDING`
- `ACCEPTED`
- `REJECTED`
- `EXPIRED`
- `WITHDRAWN`
- `DISPUTED`
- `RECONFIRMATION_REQUIRED`

### QuotationDealConversionStatus

- `NOT_STARTED`
- `VALIDATION_PENDING`
- `AWAITING_APPROVAL`
- `READY`
- `CREATION_IN_PROGRESS`
- `PENDING_EXTERNAL_CONFIRMATION`
- `DEAL_CREATED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### QuotationValidityStatus

- `NOT_STARTED`
- `ACTIVE`
- `APPROACHING_EXPIRY`
- `EXPIRED`
- `SUSPENDED`
- `REVALIDATION_REQUIRED`

### FinanceAssumptionStatus

- `NOT_APPLICABLE`
- `ESTIMATED`
- `PROGRAM_BASED`
- `LENDER_PREQUALIFICATION_BASED`
- `LENDER_DECISION_BASED`
- `EXPIRED`
- `CONFLICTED`
- `REVALIDATION_REQUIRED`

### InterestRateType

- `FIXED`
- `VARIABLE`
- `FLAT`
- `EFFECTIVE_ANNUAL`
- `OTHER`
- `UNKNOWN`

### PaymentFrequency

- `WEEKLY`
- `BIWEEKLY`
- `MONTHLY`
- `QUARTERLY`
- `ANNUALLY`
- `OTHER`

### LineItemType

- `VEHICLE_BASE_PRICE`
- `VEHICLE_OPTION`
- `ACCESSORY`
- `SERVICE`
- `WARRANTY`
- `INSURANCE`
- `REGISTRATION_FEE`
- `DOCUMENTATION_FEE`
- `DELIVERY_FEE`
- `LICENSING_FEE`
- `INSPECTION_FEE`
- `ADMINISTRATIVE_FEE`
- `STATUTORY_FEE`
- `TAX`
- `DEALER_DISCOUNT`
- `MANUFACTURER_REBATE`
- `DEALER_INCENTIVE`
- `CAMPAIGN_INCENTIVE`
- `LOYALTY_INCENTIVE`
- `FLEET_INCENTIVE`
- `FINANCE_INCENTIVE`
- `TRADE_IN_ALLOWANCE`
- `TRADE_IN_PAYOFF`
- `TRADE_IN_EQUITY`
- `FINANCE_FEE`
- `ADJUSTMENT`
- `OTHER`

### IncentiveEligibilityStatus

- `NOT_APPLICABLE`
- `NOT_EVALUATED`
- `ELIGIBLE`
- `CONDITIONALLY_ELIGIBLE`
- `INELIGIBLE`
- `EXPIRED`
- `EVIDENCE_REQUIRED`
- `CONFLICTED`

### VehicleAvailabilityStatus

- `NOT_REQUIRED`
- `NOT_CHECKED`
- `AVAILABLE`
- `RESERVED`
- `ALLOCATED`
- `NOT_AVAILABLE`
- `PENDING_CONFIRMATION`
- `STALE`
- `CONFLICTED`
- `BLOCKED`

### QuotationRejectionReason

- `PRICE_NOT_ACCEPTED`
- `PAYMENT_TERMS_NOT_ACCEPTED`
- `VEHICLE_NOT_ACCEPTED`
- `VEHICLE_UNAVAILABLE`
- `FINANCE_TERMS_NOT_ACCEPTED`
- `TRADE_IN_VALUE_NOT_ACCEPTED`
- `OPTIONAL_PRODUCTS_NOT_ACCEPTED`
- `COMPETITOR_OFFER_SELECTED`
- `CUSTOMER_POSTPONED`
- `CUSTOMER_NO_LONGER_INTERESTED`
- `CUSTOMER_REQUESTED_REVISION`
- `OTHER`
- `UNKNOWN`

### QuotationWithdrawalReason

- `PRICING_ERROR`
- `TAX_OR_FEE_ERROR`
- `INCORRECT_CUSTOMER`
- `INCORRECT_VEHICLE`
- `VEHICLE_UNAVAILABLE`
- `INCENTIVE_EXPIRED`
- `FINANCE_ASSUMPTION_EXPIRED`
- `TRADE_IN_VALUE_CHANGED`
- `COMPLIANCE_FAILURE`
- `DUPLICATE_QUOTATION`
- `CUSTOMER_REQUEST`
- `REPLACED_BY_NEW_VERSION`
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
- Dealership, branch, team, User, pricing authority, and approval authority must belong to the authorized scope.
- Cross-Tenant Quotation access, comparison, approval, AI retrieval, export, and conversion are prohibited unless governed by an approved auditable mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Opportunity and Customer Rules

- Every Quotation must reference one valid Opportunity.
- Every Quotation must reference the same Customer as the Opportunity unless a governed Customer correction has completed.
- The Opportunity must be eligible for commercial-offer preparation.
- Closed, cancelled, or archived Opportunities require a governed exception or reopening before a new Quotation.
- Quotation creation must not rewrite Opportunity requirements or qualification evidence.
- Customer identity conflicts must block issue and acceptance where material.

### Series and Version Rules

- Every Quotation belongs to one `quotation_series_id`.
- `quotation_version` must begin at an approved initial value and increase sequentially.
- Only one version in a series may have `is_current_version = true`.
- `supersedes_quotation_id` must reference the immediately preceding or approved prior version in the same series.
- Circular supersession is prohibited.
- A superseded version must remain immutable and historically accessible.
- A new version must preserve the reason for supersession.
- Retryable version creation must not create duplicate versions.

### Draft Rules

A Draft may be edited only while:

- `status = DRAFT`.
- The User has appropriate authority.
- `record_version` is current.
- The Quotation has not been issued.
- The Quotation is not part of an active immutable approval snapshot that prohibits editing.

A material change after approval may require approval revalidation.

### Vehicle and Inventory Rules

A Quotation that references a specific physical Vehicle must include:

- Valid `vehicle_id`.
- Valid `inventory_record_id`.
- Matching Vehicle and Inventory relationship.
- Sufficiently current availability.
- Current pricing context.
- No incompatible sale, delivery, legal, quality, safety, compliance, Reservation, or Allocation conflict.

A factory-order or configuration Quotation may omit `inventory_record_id` only under an approved commercial model.

Quotation issue does not:

- Reserve Inventory.
- Allocate Inventory.
- Prevent another Customer from purchasing.
- Confirm delivery.

Availability must be revalidated before acceptance and Deal conversion.

### Commercial Line-Item Rules

- Every Customer-visible charge must be itemized.
- Hidden charges are prohibited.
- Internal-only line items must not appear in Customer-facing documents.
- Quantity must be greater than zero unless an approved adjustment type permits another value.
- Amounts must use fixed decimal precision.
- Every line item must use the approved Quotation currency unless a governed conversion applies.
- Discounts, rebates, incentives, taxes, and fees must remain distinguishable.
- Optional products must not be silently bundled.
- Customer-selected optional products must preserve selection evidence where required.
- Statutory and dealership fees must remain distinguishable.
- A zero-value line item may be permitted only when commercially meaningful and properly classified.

### Calculation Rules

All monetary calculations must be deterministic.

AI must not perform authoritative totals.

The server-side calculation service must recalculate before:

- Approval request.
- Approval Decision.
- Issue.
- Acceptance.
- Deal conversion.

Calculations must preserve:

- Formula.
- Rule versions.
- Input values.
- Rounding method.
- Currency.
- Tax jurisdiction.
- Timestamp.
- Calculation hash.

The approved formula must determine:

- Gross line amounts.
- Discounts.
- Taxable amounts.
- Taxes.
- Fees.
- Trade-In equity.
- Finance principal.
- Customer cash due.
- Total due.
- Internal gross profit.
- Margin.

### Monetary Constraint Rules

- Customer-facing monetary amounts must not be negative unless the line-item type explicitly permits a credit.
- `vehicle_discount_amount` must not exceed the applicable approved price boundary.
- `down_payment_amount` must not exceed the applicable total unless an approved refund or credit workflow applies.
- `trade_in_net_equity_amount` and `trade_in_negative_equity_amount` must not both be positive for the same calculation.
- Trade-In equity must use the approved formula.
- `total_fee_amount` must equal the sum of included fee lines.
- `total_tax_amount` must equal the sum of included tax lines.
- `total_discount_amount` must equal the sum of included discount lines.
- `total_due_amount` must equal the approved deterministic total.
- `estimated_gross_margin_percentage` must correspond to the approved margin formula.
- Calculation mismatches must block approval, issue, acceptance, and conversion.

### Currency Rules

- Every Quotation must have one settlement currency.
- Currency codes must use ISO 4217 values.
- Foreign exchange must use an approved rate source.
- Exchange rate, source, timestamp, and expiration must be preserved.
- An expired exchange rate requires recalculation and potentially a new version.
- Customer-facing documents must disclose conversion assumptions where required.

### Pricing and Discount Rules

- List price, selling price, discount, rebate, and incentive must remain distinct.
- AI Recommendations do not create pricing authority.
- Discount authority must be determined by deterministic policy.
- Restricted prices and discounts require an Authoritative Human Decision.
- Below-floor or negative-margin scenarios require the configured escalation.
- Approval must preserve:
  - Requested commercial values.
  - Authority thresholds.
  - Approvers.
  - Decision.
  - Reason.
  - Evidence.
  - Expiration.
- Pricing changes after issue require a new version.
- Customer-facing price must not expose internal floor price, acquisition cost, or margin.

### Incentive Rules

- Incentive eligibility must be checked against current program rules.
- Program effective dates must cover the applicable Quotation date.
- Vehicle, Customer, region, payment route, and campaign eligibility must be validated.
- Required incentive evidence must exist.
- Expired incentives must not remain in an active issued offer.
- An incentive change after issue requires a new version.
- An expected incentive must not be represented as confirmed unless the applicable authority confirms eligibility.

### Tax and Fee Rules

- Taxes must be calculated using approved deterministic rules.
- Tax jurisdiction must be identified.
- Manual tax overrides require the configured Authoritative Human Decision.
- Statutory fees must use current approved values.
- Dealership fees must comply with applicable law and disclosure policy.
- Fees must be itemized.
- Tax or fee changes after issue require a new version.
- AI must not invent a tax, fee, or exemption.

### Trade-In Rules

- Trade-In values must come from the governed Trade-In workflow.
- Actual cash value and Customer allowance must remain distinct.
- Payoff must use sufficiently current verified evidence.
- Trade-In equity must be recalculated when payoff or allowance changes.
- Ownership or lien conflict may block issue or acceptance.
- Trade-In figures changing after issue require a new Quotation version.
- Trade-In allowance above configured authority requires Human Approval.

### Finance Rules

- Estimated finance values must be labelled non-binding.
- Finance illustration must preserve the source and expiration of assumptions.
- Lender prequalification must not be represented as final approval.
- Final lender Decision must remain in Finance Application.
- Finance-term changes after issue require a new version.
- Expired finance assumptions require revalidation.
- Customer-facing payment examples must include required disclosures.
- AI must not claim that finance will be approved.
- A finance estimate must not discriminate using prohibited attributes or inappropriate proxies.

### Approval Rules

Approval is required whenever configured policy identifies:

- Discount threshold.
- Margin threshold.
- Price override.
- Manual tax override.
- Manual fee override.
- Trade-In allowance exception.
- Incentive exception.
- Finance exception.
- Expired authority.
- Restricted optional product.
- Compliance exception.
- Another material commercial risk.

Approval status must not become `APPROVED` until all required approval steps complete.

A rejected approval must not be bypassed by editing the status directly.

Material Draft changes after approval may:

- Revoke approval.
- Require revalidation.
- Require a new approval chain.

AI Agents must not approve:

- Prices.
- Discounts.
- Fees.
- Taxes.
- Trade-In values.
- Finance terms.
- Margin exceptions.

### Issue Rules

A Quotation may become `ISSUED` only when:

- Required commercial fields are complete.
- All deterministic calculations pass.
- Required approvals are valid.
- Required Inventory and pricing evidence is current.
- Required Trade-In and finance evidence is current.
- Validity period is defined.
- Required disclosures are included.
- Customer-facing document is rendered.
- Commercial snapshot is frozen.
- Customer presentation snapshot is frozen.
- Document hash is generated.
- No blocking conflict exists.
- `issued_record_version` is recorded.

An issued Quotation is immutable.

### Delivery Rules

- Issue and delivery must remain separate.
- Delivery requires a permitted Customer communication purpose and channel.
- Customer Consent and restrictions must be evaluated deterministically.
- Action Class 2 delivery may proceed through:
  - Explicit Human Approval; or
  - An applicable pre-approved automation policy.
- Retryable delivery Commands must use `idempotency_key`.
- Provider transport success does not automatically prove Customer delivery.
- Delivery failure must not alter the issued commercial snapshot.
- Delivery attempts must remain auditable.

### View Rules

- `VIEWED` evidence must reference the issued document or approved access mechanism.
- Provider tracking must comply with privacy and Consent requirements.
- View evidence does not prove Customer understanding or acceptance.
- A Quotation must not be marked viewed solely from AI inference.

### Acceptance Rules

A Quotation may become `ACCEPTED` only when:

- It is the applicable current issued version.
- It is within its valid period.
- It is not withdrawn, superseded, rejected, or expired.
- Customer identity is sufficiently verified.
- Acceptance references the exact `quotation_id`.
- Acceptance references the exact `quotation_version`.
- Acceptance references the exact `document_hash`.
- Acceptance channel and timestamp are preserved.
- Acceptance evidence satisfies the configured policy.
- Vehicle and Inventory dependencies are revalidated.
- Required finance, Trade-In, incentive, and approval evidence remains valid.
- No blocking conflict exists.

Acceptance must not be inferred solely from:

- Document view.
- Positive sentiment.
- Verbal interest without accepted evidence.
- Deposit initiation.
- Appointment completion.
- AI prediction.
- Opportunity stage.

### Rejection Rules

- Rejection requires Customer or authorized evidence.
- The reason may remain `UNKNOWN`.
- ASOS must not invent the rejection reason.
- Rejection applies to one specific Quotation version.
- A revised offer requires a new version.
- A rejected version must remain immutable.

### Expiry Rules

- `expires_at` must be later than `valid_from`.
- Expiry must be enforced by an authoritative server-side process.
- User-interface display alone is insufficient.
- An expired Quotation must not be accepted.
- A late Customer response requires revalidation or a new version.
- Expiration must not delete the Quotation.
- Validity duration must remain configurable.

### Supersession Rules

A Quotation may become `SUPERSEDED` only when:

- A valid replacement version exists.
- The replacement belongs to the same series.
- The replacement references the prior version.
- The current-version indicator is updated atomically.
- Supersession reason is preserved.
- The prior version remains immutable.

An accepted Quotation must not be superseded through an ordinary revision workflow.

It requires controlled cancellation, amendment, or Deal handling according to policy.

### Withdrawal Rules

- An issued Quotation may be withdrawn only through a controlled workflow.
- Withdrawal requires an authorized reason.
- Customer notification must be evaluated.
- External provider updates may remain pending until Confirmation.
- Withdrawal does not erase Customer access or prior evidence.
- An accepted Quotation requires higher-authority handling than an unaccepted version.
- AI Agents must not independently withdraw an issued Quotation.

### Deal Conversion Rules

An accepted Quotation may support Deal creation only when:

- Customer acceptance remains valid.
- The exact accepted Quotation version is used.
- Opportunity remains eligible.
- Customer identity remains valid.
- Vehicle and Inventory remain eligible.
- Required Reservation or Allocation workflow is satisfied where applicable.
- Required finance and Trade-In dependencies are current.
- Required Human Decisions are complete.
- Conversion is idempotent.
- No Deal was previously created from the same accepted Quotation.
- The resulting Deal preserves the immutable commercial snapshot.

Deal conversion does not by itself confirm external sale posting, Payment, contract signature, or delivery.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Version creation must be idempotent.
- Issue must be idempotent.
- Delivery must be idempotent.
- Acceptance processing must be idempotent.
- Rejection processing must be idempotent.
- Withdrawal must be idempotent.
- Deal conversion must be idempotent.
- Retryable Commands must use `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Quotation versions.
  - Approval requests.
  - Issued documents.
  - Delivery messages.
  - Acceptance records.
  - Withdrawal records.
  - Deal-conversion attempts.
  - Deals.

### Human Review Requirements

Human Review is required according to configured policy for:

- Customer identity conflict.
- Vehicle or Inventory conflict.
- Pricing conflict.
- Calculation mismatch.
- Below-authority pricing.
- Negative or restricted margin.
- Manual tax or fee override.
- Trade-In exception.
- Finance conflict.
- Incentive conflict.
- Disputed acceptance.
- Late acceptance.
- Withdrawal after acceptance.
- Deal-conversion exception.
- External reconciliation failure.
- Another high-risk commercial exception.

---

## 6. State Machine

### Allowed States

```text
DRAFT
APPROVAL_PENDING
APPROVED
ISSUED
ACCEPTED
REJECTED
EXPIRED
SUPERSEDED
WITHDRAWAL_PENDING
WITHDRAWN
ARCHIVED
```

Delivery and view state are managed separately from the primary Quotation lifecycle.

### Principal Allowed Transitions

```text
DRAFT → APPROVAL_PENDING
DRAFT → APPROVED
DRAFT → WITHDRAWN

APPROVAL_PENDING → DRAFT
APPROVAL_PENDING → APPROVED
APPROVAL_PENDING → WITHDRAWN

APPROVED → DRAFT
APPROVED → ISSUED
APPROVED → WITHDRAWN

ISSUED → ACCEPTED
ISSUED → REJECTED
ISSUED → EXPIRED
ISSUED → SUPERSEDED
ISSUED → WITHDRAWAL_PENDING

WITHDRAWAL_PENDING → WITHDRAWN
WITHDRAWAL_PENDING → ISSUED

ACCEPTED → ARCHIVED
REJECTED → SUPERSEDED
REJECTED → ARCHIVED
EXPIRED → SUPERSEDED
EXPIRED → ARCHIVED
SUPERSEDED → ARCHIVED
WITHDRAWN → ARCHIVED
```

Returning `APPROVED → DRAFT` requires approval invalidation or revalidation evidence.

Returning `WITHDRAWAL_PENDING → ISSUED` requires rejection or cancellation of the withdrawal request.

### Forbidden Ordinary Transitions

```text
DRAFT → ISSUED
DRAFT → ACCEPTED
DRAFT → REJECTED
DRAFT → EXPIRED

APPROVAL_PENDING → ISSUED
APPROVAL_PENDING → ACCEPTED

APPROVED → ACCEPTED

ISSUED → DRAFT
ISSUED → APPROVED

ACCEPTED → DRAFT
ACCEPTED → ISSUED
ACCEPTED → REJECTED
ACCEPTED → EXPIRED
ACCEPTED → SUPERSEDED

REJECTED → ACCEPTED
EXPIRED → ACCEPTED
SUPERSEDED → ACCEPTED
WITHDRAWN → ACCEPTED

ARCHIVED → DRAFT
ARCHIVED → ISSUED
ARCHIVED → ACCEPTED
```

Corrections to terminal or immutable outcomes require a governed correction, dispute, amendment, or replacement workflow.

### Entering DRAFT

Requires:

- Valid Tenant context.
- Opportunity.
- Customer.
- Authorized creator.
- Quotation series.
- Initial scenario context.
- Initial audit evidence.

### Entering APPROVAL_PENDING

Requires:

- Complete required commercial inputs.
- Successful deterministic calculation.
- Identified approval reasons.
- Current Vehicle, pricing, Trade-In, finance, tax, and incentive inputs.
- Created approval request.
- Frozen approval-request snapshot.

### Entering APPROVED

Requires:

- Successful deterministic calculation.
- All required approval steps completed.
- Approval evidence.
- Current pricing and dependencies.
- No blocking conflict.
- Approval expiration established where applicable.

### Entering ISSUED

Requires:

- Approval or valid `NOT_REQUIRED` status.
- Current dependencies.
- Validity period.
- Required disclosures.
- Frozen commercial snapshot.
- Frozen Customer presentation snapshot.
- Rendered document.
- Document hash.
- Issued record version.
- Issue actor.
- Issue timestamp.

### Entering ACCEPTED

Requires:

- Valid current issued version.
- Current validity.
- Exact document hash.
- Exact Quotation version.
- Accepted Customer evidence.
- Valid Customer identity.
- Current critical dependencies.
- No blocking conflict.

### Entering REJECTED

Requires:

- Customer rejection evidence.
- Rejection timestamp.
- Rejection reason where known.
- Exact Quotation version.

### Entering EXPIRED

Requires:

- Authoritative current time later than `expires_at`.
- No valid acceptance recorded before expiration.
- Applicable expiration policy.
- Expiration timestamp.

### Entering SUPERSEDED

Requires:

- Valid replacement version.
- Same Quotation series.
- Atomic current-version update.
- Supersession reason.
- Replacement reference.

### Entering WITHDRAWAL_PENDING

Requires:

- Issued Quotation.
- Authorized withdrawal request.
- Withdrawal reason.
- Required Human Decision.
- Customer-notification and external-provider evaluation.

### Entering WITHDRAWN

Requires:

- Accepted withdrawal Decision.
- Withdrawal evidence.
- Completed or appropriately pending Customer-notification workflow.
- External Confirmation where applicable.
- No unresolved acceptance conflict.

### Terminal States

For ordinary processing:

- `ACCEPTED`
- `REJECTED`
- `EXPIRED`
- `SUPERSEDED`
- `WITHDRAWN`
- `ARCHIVED`

`ARCHIVED` is terminal.

### Correction and Dispute Handling

Correcting an immutable or terminal Quotation requires:

- Authorized Human Decision.
- Correction or dispute reason.
- Supporting evidence.
- A new Quotation version, correction record, or governed commercial amendment.
- Related Opportunity and Deal reconciliation.
- New Event.
- Preserved original version.
- Audit history.

AI Agents must not independently correct:

- Issued terms.
- Acceptance.
- Rejection.
- Expiry.
- Withdrawal.
- Deal conversion.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied Business Rules.
- Approval or Human Decision.
- Automation-policy reference.
- Calculation snapshot.
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

- Every Quotation belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant commercial offers require an approved and auditable mechanism.

### Opportunity

- Every Quotation references one Opportunity.
- One Opportunity may have multiple Quotation series and versions.
- Opportunity stage may project Quotation progress.
- Quotation acceptance must not automatically make the Opportunity `WON` without governed Deal creation.

### Customer

- Every Quotation references one Customer.
- Customer identity remains governed by Customer.
- Customer Consent and contact restrictions remain governed separately.
- Customer snapshots inside the Quotation preserve presentation context and do not replace current Customer authority.

### Vehicle

- Quotation may reference one Vehicle identity or configuration.
- Vehicle specifications remain governed by Vehicle.
- The issued snapshot preserves what was presented to the Customer.

### Inventory Record

- A physical-stock Quotation may reference one Inventory Record.
- Inventory Record owns availability, location, Reservation, Allocation, sale, and delivery context.
- Quotation does not reserve or allocate Inventory.

### Trade-In

- Quotation may reference one Trade-In.
- Trade-In owns appraisal, ownership, lien, payoff, condition, and acquisition approval.
- Quotation preserves the commercial projection used in the issued version.

### Finance Application

- Quotation may reference one or more permitted finance scenarios or a Finance Application.
- Finance Application owns submission and lender Decision.
- Quotation must distinguish estimate, prequalification, and authoritative Decision.

### Financial Contract

- A Quotation may contribute commercial input to a later Financial Contract.
- Contract terms and signature remain governed by Financial Contract and the signature authority.
- Quotation acceptance does not sign the Financial Contract.

### Deal

- An accepted Quotation may create one primary Deal through a controlled workflow.
- Deal must reference the exact accepted Quotation version.
- The Quotation must preserve the resulting Deal reference.
- Deal state must not be stored as Quotation state beyond required projections.

### Interaction

Interactions may provide:

- Quotation delivery.
- Customer view.
- Customer questions.
- Negotiation discussion.
- Acceptance.
- Rejection.
- Withdrawal notification.
- Follow-up.

Original communication evidence remains governed by Interaction and the provider.

### Appointment

- A Quotation may be reviewed during an Appointment.
- Appointment completion does not prove acceptance.
- Quotation-review outcome must remain distinct from Quotation lifecycle state.

### Market Intelligence

Market Intelligence may support:

- Price context.
- Competitor context.
- Demand context.
- Incentive Recommendation.
- Scenario Recommendation.
- Acceptance probability.

Market Intelligence must not silently alter approved Quotation terms.

### Approval and Human Decision

A Quotation may reference multiple approval Decisions.

Approval history must preserve:

- Authority.
- Scope.
- Value thresholds.
- Decision.
- Reason.
- Evidence.
- Effective period.
- Revocation.

### Document Storage

Issued documents must be stored in controlled document storage.

The Quotation must preserve:

- Document reference.
- Template.
- Template version.
- Hash.
- Generation time.
- Security classification.
- Retention class.

### Supersession Relationship

A replacement version must reference:

```text
supersedes_quotation_id
```

The previous version may reference:

```text
superseded_by_quotation_id
```

Circular supersession is prohibited.

### Supporting Child Records

Quotation may own or govern:

- Line items.
- Calculation records.
- Approval requests.
- Approval Decisions.
- Document versions.
- Delivery attempts.
- Customer views.
- Acceptance records.
- Rejection records.
- Withdrawal records.
- Conversion attempts.
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

The following are required Quotation Event concepts and do not replace the Event Catalog.

### Draft and Version Event Concepts

- Quotation series created.
- Quotation Draft created.
- Quotation Draft updated.
- Quotation calculation completed.
- Quotation calculation failed.
- New Quotation version created.
- Quotation version superseded.
- Quotation data conflict detected.
- Quotation data conflict resolved.

### Approval Event Concepts

- Quotation approval evaluated.
- Quotation approval requested.
- Quotation approval partially completed.
- Quotation approved.
- Quotation approval rejected.
- Quotation approval expired.
- Quotation approval revoked.
- Quotation revalidation requested.

### Issue Event Concepts

- Quotation issue requested.
- Quotation issue validation failed.
- Quotation document generated.
- Quotation issued.
- Quotation issue failed.

### Delivery Event Concepts

- Quotation delivery requested.
- Quotation delivery approved.
- Quotation delivery Command sent.
- Quotation delivery confirmed.
- Quotation delivery failed.
- Quotation delivery reconciliation required.

### Customer Response Event Concepts

- Quotation viewed.
- Quotation acceptance received.
- Quotation acceptance validated.
- Quotation acceptance rejected by validation.
- Quotation accepted.
- Quotation rejected.
- Quotation acceptance disputed.
- Quotation expired.

### Withdrawal Event Concepts

- Quotation withdrawal requested.
- Quotation withdrawal approved.
- Quotation withdrawal rejected.
- Quotation withdrawal notification requested.
- Quotation withdrawn.
- Quotation withdrawal reconciliation required.

### Deal Conversion Event Concepts

- Quotation Deal conversion requested.
- Quotation Deal conversion validation failed.
- Quotation Deal conversion started.
- Deal created from Quotation.
- Quotation Deal conversion failed.
- Quotation Deal conversion reconciliation required.

### Derived Intelligence Event Concepts

- Quotation acceptance probability updated.
- Quotation margin risk detected.
- Quotation competitiveness score updated.
- Quotation scenario recommended.
- Quotation follow-up recommended.
- Quotation Human Review recommended.

Derived Intelligence Events must not imply:

- Pricing approval.
- Customer delivery.
- Customer acceptance.
- Vehicle Reservation.
- Vehicle Allocation.
- Finance approval.
- Deal creation.
- Sale.
- Delivery.
- External completion.

### Producer Rules

- Quotation Domain Service publishes accepted canonical and workflow-state changes.
- Pricing, Tax, Inventory, Trade-In, and Finance services publish accepted authoritative facts or projections.
- Interaction Domain Service publishes accepted communication facts.
- Deal Domain Service publishes accepted Deal facts.
- Integration services publish normalized external-source observations.
- AI Agents may publish Agent-run, analysis, prediction, or Recommendation Events.
- AI Agents must not publish authoritative pricing, approval, acceptance, Deal, Payment, sale, or delivery Events merely because they recommended or predicted the outcome.

### Event Requirements

Every material Quotation Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `quotation_id`.
- `quotation_series_id`.
- `quotation_version`.
- `opportunity_id`.
- `customer_id`.
- Vehicle and Inventory references.
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
- Approval or Human Decision.
- Automation-policy reference.
- Command.
- External Confirmation.
- Security classification.

Events are immutable.

Corrections, supersession, withdrawal, rejection, and reversal must use new Events linked to the original Event.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Quotation scenario generation.
- Vehicle-option comparison.
- Optional-product Recommendation.
- Payment-scenario Recommendation.
- Customer-message drafting.
- Customer-language adaptation.
- Terms summarization.
- Commercial explanation.
- Negotiation summarization.
- Objection classification.
- Acceptance-probability estimation.
- Price-sensitivity analysis.
- Margin-risk detection.
- Competitor-context analysis.
- Follow-up Recommendation.
- Missing-data detection.
- Approval-preparation summary.
- Data-quality issue detection.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Set authoritative price.
- Approve a discount.
- Approve a margin exception.
- Create a tax or fee.
- Approve Trade-In value.
- Approve finance.
- Confirm lender terms.
- Create general marketing Consent.
- Reverse contact restrictions.
- Issue an unapproved Quotation.
- Confirm Customer delivery.
- Mark a Quotation viewed.
- Mark a Quotation accepted.
- Withdraw an issued Quotation.
- Reserve or allocate a Vehicle.
- Create a Deal outside approved authority.
- Confirm Payment.
- Confirm sale.
- Confirm delivery.
- Execute external Commands directly.
- Access Quotation data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Quotation identifier and record version.
- Quotation series and version.
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
- Required Human authority or automation policy.

### Scenario Generation

AI may recommend alternative:

- Vehicles.
- Inventory Records.
- Payment structures.
- Optional products.
- Finance illustrations.
- Trade-In scenarios.
- Validity periods.

Every recommended scenario must be recalculated by deterministic services.

A scenario must not become Customer-facing until:

- Authoritative inputs are obtained.
- Required calculations pass.
- Required approval is completed.
- Applicable disclosures are included.
- The Quotation is issued.

### Pricing Recommendations

AI may recommend:

- Discount review.
- Product bundle.
- Price explanation.
- Alternative Vehicle.
- Alternative payment scenario.
- Management escalation.

AI must not:

- Access restricted price floors without approved purpose.
- Present internal margin to the Customer.
- Represent a Recommendation as approved pricing.
- Change an issued Quotation.
- Invent competitor prices or market facts.

### Finance Illustrations

AI may explain approved finance assumptions.

AI must distinguish:

- Estimate.
- Program information.
- Prequalification.
- Lender Decision.
- Signed contract.

AI must not state or imply finance approval without authoritative lender evidence.

### Customer-Facing Drafting

AI may draft Customer-facing content only when:

- The commercial snapshot is authoritative.
- Customer language is known or safely selected.
- Required disclosures are supplied from approved sources.
- Restricted internal information is excluded.
- Claims about Vehicle availability are current.
- Human Approval or applicable automation policy is satisfied.

AI must not invent:

- Prices.
- Tax.
- Fees.
- Incentives.
- Availability.
- Finance approval.
- Delivery date.
- Legal terms.

### Acceptance Reasoning

AI may identify potential acceptance language.

AI-derived intent must not become authoritative acceptance by itself.

Authoritative acceptance requires:

- Approved evidence.
- Customer identity.
- Exact Quotation version.
- Exact document hash.
- Accepted channel.
- Current validity.
- Applicable workflow validation.

### Action Class 2

Controlled delivery or follow-up communication may proceed through:

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
- Quotation state.
- Document version.
- Validity.
- Customer restrictions.
- Pricing authority.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision.

Examples include:

- Restricted price.
- Discount override.
- Negative-margin approval.
- Manual tax override.
- Manual fee override.
- Trade-In allowance exception.
- Finance exception.
- Quotation withdrawal after issue.
- Disputed acceptance.
- Deal conversion.
- Commercial amendment.

### AI Context and Embeddings

Direct identifiers, restricted pricing, and sensitive financial information must not enter unrestricted embeddings.

Normally excluded fields include:

- Customer name.
- Phone.
- Email.
- Address.
- Customer identifiers.
- Internal price floor.
- Acquisition cost.
- Gross profit.
- Gross margin.
- Discount authority.
- Approval thresholds.
- Trade-In payoff evidence.
- Finance Application details.
- Lender Decision details.
- Acceptance identity evidence.
- Payment information.
- Contract documents.
- Raw issued documents containing direct identifiers.

Approved redacted context may include:

- Vehicle description.
- Customer-visible line-item categories.
- Non-sensitive payment-scenario summary.
- Optional-product categories.
- Customer objection categories.
- Rejection reason.
- Non-sensitive negotiation summary.
- Approved Customer-facing terms summary.

Every vector entry must enforce:

- `tenant_id`.
- Quotation access scope.
- Source reference.
- Document version.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Explainability

Material Quotation Recommendations must explain:

- Evidence used.
- Source authority.
- Input freshness.
- Calculation method.
- Material assumptions.
- Inventory dependency.
- Trade-In dependency.
- Finance dependency.
- Approval requirement.
- Customer-facing impact.
- Margin or compliance risk.
- Confidence where meaningful.
- Required Human authority.
- External Confirmation requirement.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Quotation API behaviour.

### REST Resources

```text
GET    /api/v1/quotations
POST   /api/v1/quotations
GET    /api/v1/quotations/{quotation_id}
PATCH  /api/v1/quotations/{quotation_id}

POST   /api/v1/opportunities/{opportunity_id}/quotation-series
POST   /api/v1/quotations/{quotation_id}/version-requests
POST   /api/v1/quotations/{quotation_id}/calculation-requests
POST   /api/v1/quotations/{quotation_id}/approval-requests
POST   /api/v1/quotations/{quotation_id}/approval-decisions
POST   /api/v1/quotations/{quotation_id}/issue-requests
POST   /api/v1/quotations/{quotation_id}/delivery-requests
POST   /api/v1/quotations/{quotation_id}/acceptance-submissions
POST   /api/v1/quotations/{quotation_id}/rejection-submissions
POST   /api/v1/quotations/{quotation_id}/withdrawal-requests
POST   /api/v1/quotations/{quotation_id}/deal-conversion-requests
POST   /api/v1/quotations/{quotation_id}/correction-requests

GET    /api/v1/quotation-series/{quotation_series_id}
GET    /api/v1/quotation-series/{quotation_series_id}/versions
GET    /api/v1/quotations/{quotation_id}/calculation
GET    /api/v1/quotations/{quotation_id}/approval-history
GET    /api/v1/quotations/{quotation_id}/delivery-history
GET    /api/v1/quotations/{quotation_id}/acceptance-evidence
GET    /api/v1/quotations/{quotation_id}/conversion-attempts
GET    /api/v1/quotations/{quotation_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, User, pricing, and approval scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "scenario_name": "Primary cash and trade-in offer",
  "scenario_type": "TRADE_IN_SCENARIO",
  "commercial_purpose": "VEHICLE_AND_TRADE_IN",
  "payment_method": "CASH",
  "currency_code": "EGP",
  "vehicle_context": {
    "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
    "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000"
  },
  "trade_in_id": "a6cd98db-b21d-43a0-ad40-e027a62994da",
  "requested_line_items": [
    {
      "line_item_type": "ACCESSORY",
      "description": "Approved accessory package",
      "quantity": 1
    }
  ]
}
```

The request must include an HTTP header such as:

```text
Idempotency-Key: e18f987b-e2bc-4c48-8ae8-69594d895319
```

### Example Draft Response

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "quotation_series_id": "105e7cba-f3e8-4e90-9517-86af42793b9f",
  "quotation_number": "QT-2026-001245",
  "quotation_version": 1,
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "status": "DRAFT",
  "approval_status": "NOT_EVALUATED",
  "issue_status": "NOT_READY",
  "delivery_status": "NOT_REQUESTED",
  "acceptance_status": "NOT_REQUESTED",
  "deal_conversion_status": "NOT_STARTED",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T18:30:00Z"
}
```

### Example Approval Request

```json
{
  "expected_record_version": 4,
  "requested_approval_scope": [
    "DEALER_DISCOUNT",
    "TRADE_IN_ALLOWANCE"
  ],
  "business_reason": "Customer-specific negotiated scenario."
}
```

The approval response may return:

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "status": "APPROVAL_PENDING",
  "approval_status": "PENDING",
  "approval_request_id": "e7dd01f6-814d-477f-a87d-9b4029b7fbbe",
  "record_version": 5
}
```

### Example Issue Response

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "quotation_version": 1,
  "status": "ISSUED",
  "issue_status": "ISSUED",
  "valid_from": "2026-08-01T19:00:00Z",
  "expires_at": "2026-08-04T19:00:00Z",
  "document_hash": "sha256:8ac44d5d...",
  "rendered_document_reference": "documents://quotations/70b12969-bf19-4264-bf9f-30bd736c1262/v1",
  "record_version": 7
}
```

### Example Delivery Request

```json
{
  "channel": "EMAIL",
  "template_id": "quotation_delivery_en_v3",
  "expected_document_hash": "sha256:8ac44d5d...",
  "expected_record_version": 7
}
```

The response may remain pending:

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "delivery_status": "PENDING_CONFIRMATION",
  "command_id": "114a5c0f-b353-45ec-90b9-552a119307e0",
  "record_version": 8
}
```

The API must not describe the document as delivered until the accepted evidence policy is satisfied.

### Example Acceptance Submission

```json
{
  "quotation_version": 1,
  "document_hash": "sha256:8ac44d5d...",
  "acceptance_channel": "CUSTOMER_PORTAL",
  "acceptance_evidence_reference": "evidence://quotation-acceptances/8b839a54",
  "expected_record_version": 9
}
```

A successful validated response may return:

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "status": "ACCEPTED",
  "acceptance_status": "ACCEPTED",
  "accepted_at": "2026-08-02T10:14:00Z",
  "accepted_document_hash": "sha256:8ac44d5d...",
  "record_version": 10
}
```

### Example Deal Conversion Response

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "deal_conversion_status": "DEAL_CREATED",
  "accepted_deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "record_version": 12
}
```

An external workflow may instead return:

```json
{
  "quotation_id": "70b12969-bf19-4264-bf9f-30bd736c1262",
  "deal_conversion_status": "PENDING_EXTERNAL_CONFIRMATION",
  "accepted_deal_id": "9ea6b018-8ad8-40fb-9148-48cd8a17f2b3",
  "command_id": "7c0928cb-88fa-4787-b8db-f792793bd472",
  "record_version": 12
}
```

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Deterministic calculation.
- Pricing and approval policy.
- Inventory freshness.
- Trade-In and finance validity.
- Customer communication restrictions.
- Required Human Decision or applicable automation policy.
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

- Quotation series.
- Quotation versions.
- Approval requests.
- Issued documents.
- Delivery Commands.
- Acceptance records.
- Withdrawal requests.
- Deal-conversion attempts.
- Deals.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `OPPORTUNITY_NOT_ELIGIBLE`
- `CUSTOMER_MISMATCH`
- `VEHICLE_MISMATCH`
- `INVENTORY_AVAILABILITY_STALE`
- `VEHICLE_UNAVAILABLE`
- `CALCULATION_FAILED`
- `CALCULATION_MISMATCH`
- `PRICING_AUTHORITY_REQUIRED`
- `APPROVAL_REQUIRED`
- `APPROVAL_EXPIRED`
- `TRADE_IN_DATA_STALE`
- `FINANCE_ASSUMPTION_EXPIRED`
- `INCENTIVE_NOT_ELIGIBLE`
- `CONTACT_PERMISSION_RESTRICTED`
- `QUOTATION_IMMUTABLE`
- `QUOTATION_EXPIRED`
- `QUOTATION_SUPERSEDED`
- `QUOTATION_WITHDRAWN`
- `DOCUMENT_HASH_MISMATCH`
- `ACCEPTANCE_EVIDENCE_INVALID`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `DEAL_ALREADY_CREATED`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Field authority.
- Commercial calculation.
- Version immutability.
- Approval.
- Customer communication restrictions.
- Concurrency.
- Idempotency.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Quotation Domain Service, deterministic calculation services, or Policy Engine controls.

---

## 11. Database Design

### Recommended Tables

```text
quotation_series
quotations
quotation_line_items
quotation_calculations
quotation_calculation_inputs
quotation_approval_requests
quotation_approval_decisions
quotation_documents
quotation_delivery_attempts
quotation_view_records
quotation_acceptance_records
quotation_rejection_records
quotation_withdrawal_records
quotation_supersession_history
quotation_deal_conversion_attempts
quotation_external_references
quotation_external_confirmations
quotation_derived_intelligence
quotation_reconciliation_cases
quotation_data_quality_issues
quotation_status_history
quotation_record_versions
quotation_audit_log
```

### Quotation Series Table

`quotation_series` should contain:

- `quotation_series_id`.
- `tenant_id`.
- Opportunity.
- Customer.
- Scenario type.
- Series reference.
- Current Quotation identifier.
- Latest version.
- Series status.
- Created time.
- Updated time.

### Quotations Table

The `quotations` table should contain:

- Quotation-version identifier.
- Series identifier.
- Tenant and organizational scope.
- Opportunity and Customer relationships.
- Vehicle, Inventory, Trade-In, and finance relationships.
- Version number.
- Current-version indicator.
- Current lifecycle state.
- Current approval state.
- Current validity state.
- Current issue, delivery, view, and acceptance projections.
- Current Deal-conversion projection.
- Frozen snapshot references.
- Document hash.
- Source and synchronization status.
- Record version.
- Audit timestamps.

Historical and repeating detail must remain in child or history tables.

### Primary Keys

```text
PRIMARY KEY (quotation_series_id)
```

for `quotation_series`.

```text
PRIMARY KEY (quotation_id)
```

for `quotations`.

### Tenant Protection

Every Quotation-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_quotations_tenant_status
  (tenant_id, status)

idx_quotations_tenant_opportunity
  (tenant_id, opportunity_id)

idx_quotations_tenant_customer
  (tenant_id, customer_id)

idx_quotations_tenant_series_version
  (tenant_id, quotation_series_id, quotation_version)

idx_quotations_tenant_current
  (tenant_id, quotation_series_id, is_current_version)

idx_quotations_tenant_inventory
  (tenant_id, inventory_record_id)

idx_quotations_approval
  (tenant_id, approval_status, approval_requested_at)

idx_quotations_expiry
  (tenant_id, status, expires_at)

idx_quotations_delivery
  (tenant_id, delivery_status, delivery_requested_at)

idx_quotations_conversion
  (tenant_id, deal_conversion_status)

idx_quotations_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, quotation_number, quotation_version)
```

```text
UNIQUE (tenant_id, quotation_series_id, quotation_version)
```

A partial unique constraint or equivalent transactional control should enforce one current version:

```text
UNIQUE (tenant_id, quotation_series_id)
WHERE is_current_version = true
```

External reference uniqueness may use:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the external source guarantees uniqueness.

### Immutable Issued Versions

The persistence layer must prevent material commercial updates when:

```text
status IN (
  'ISSUED',
  'ACCEPTED',
  'REJECTED',
  'EXPIRED',
  'SUPERSEDED',
  'WITHDRAWAL_PENDING',
  'WITHDRAWN',
  'ARCHIVED'
)
```

Permitted post-issue updates must be limited to governed projections such as:

- Delivery status.
- View status.
- Acceptance status.
- Rejection status.
- Withdrawal workflow.
- Deal-conversion workflow.
- Reconciliation.
- Audit.

The frozen commercial snapshot and issued document must remain immutable.

### Line Items

`quotation_line_items` should preserve:

- Line-item identifier.
- Quotation identifier.
- Tenant.
- Type.
- Description.
- Quantity.
- Unit amount.
- Gross amount.
- Discount amount.
- Taxable amount.
- Tax amount.
- Net amount.
- Currency.
- Authority.
- Rule.
- Effective period.
- Customer-visible flag.
- Internal-only flag.
- Sort order.
- Snapshot hash.

### Calculation Storage

`quotation_calculations` should preserve:

- Calculation identifier.
- Quotation.
- Record version.
- Calculation-service version.
- Formula version.
- Rule versions.
- Input hash.
- Output hash.
- Currency.
- Rounding policy.
- Tax jurisdiction.
- Calculated totals.
- Validation result.
- Generated time.
- Expiration.
- Related Event.

### Approval Storage

`quotation_approval_requests` and `quotation_approval_decisions` should preserve:

- Request identifier.
- Quotation.
- Quotation record version.
- Requested scope.
- Threshold evidence.
- Policy and version.
- Required approver role.
- Assigned approver.
- Decision.
- Decision reason.
- Evidence.
- Effective period.
- Revocation.
- Timestamp.
- Related Event.

### Document Storage

`quotation_documents` should preserve:

- Document identifier.
- Quotation.
- Quotation version.
- Template.
- Template version.
- Language.
- Rendered document reference.
- Document hash.
- Commercial snapshot hash.
- Generated time.
- Issued time.
- Security classification.
- Retention class.
- Related Event.

### Delivery History

`quotation_delivery_attempts` should preserve:

- Delivery-attempt identifier.
- Quotation.
- Quotation version.
- Document hash.
- Customer.
- Channel.
- Purpose.
- Template.
- Approval or automation-policy reference.
- Command.
- Idempotency key.
- Provider reference.
- Requested time.
- Sent time.
- Delivered time.
- Failure reason.
- Interaction reference.
- External Confirmation.
- Related Event.

### Acceptance History

`quotation_acceptance_records` should preserve:

- Acceptance identifier.
- Quotation.
- Quotation version.
- Document hash.
- Customer.
- Channel.
- Evidence.
- Identity-verification context.
- Received time.
- Validated time.
- Validation result.
- Dispute state.
- Actor.
- Related Event.

Acceptance records must be append-only or versioned.

### Supersession History

`quotation_supersession_history` should preserve:

- Prior Quotation.
- Replacement Quotation.
- Series.
- Reason.
- Actor.
- Timestamp.
- Approval requirements.
- Related Event.

### Deal Conversion Attempts

`quotation_deal_conversion_attempts` should preserve:

- Conversion-attempt identifier.
- Quotation.
- Accepted document hash.
- Quotation record version.
- Opportunity.
- Customer.
- Vehicle and Inventory references.
- Requested by.
- Idempotency key.
- Validation result.
- Human Decision.
- Created Deal.
- Command.
- External Confirmation.
- Failure reason.
- Reconciliation state.
- Started and completed times.
- Related Events.

### Derived Intelligence

Derived Quotation records must remain separate from authoritative pricing and workflow fields.

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

Quotation audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw sensitive values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Issue date.
- Expiration date.
- Retention class.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Series versioning.
- Current-version uniqueness.
- Immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Quotation must not be hard-deleted when referenced by:

- Opportunity.
- Customer journey.
- Vehicle or Inventory workflow.
- Trade-In.
- Finance Application.
- Financial Contract.
- Deal.
- Interaction.
- Approval.
- Customer acceptance.
- Document.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Audit evidence.

Withdrawal, supersession, expiry, archival, anonymization, or governed redaction must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `CUSTOMER_IDENTIFIER_REFERENCE` | Customer and Opportunity references |
| `CUSTOMER_COMMERCIAL` | Customer-facing price, payment scenario, Trade-In allowance |
| `INTERNAL_PRICING_RESTRICTED` | Price floor, discount limits, internal adjustments |
| `COMMERCIAL_CONFIDENTIAL` | Margin, profit, negotiation strategy |
| `FINANCIAL_RESTRICTED` | Finance assumptions, lender context, payoff information |
| `TRADE_IN_RESTRICTED` | Appraisal, lien, payoff, ownership evidence |
| `CUSTOMER_DOCUMENT` | Issued Quotation document |
| `ACCEPTANCE_EVIDENCE` | Acceptance identity, channel, document hash |
| `DERIVED_INTELLIGENCE` | Acceptance probability and Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, version history |

### Authentication

Every internal Quotation operation requires an authenticated Human or service identity.

Customer access to issued Quotations must use an approved secure access mechanism.

Anonymous unrestricted access to issued Quotations is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Team.
- Opportunity ownership.
- Customer relationship.
- Role.
- Requested field.
- Requested action.
- Quotation state.
- Approval threshold.
- Monetary value.
- Data classification.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May perform permitted:

- Draft creation.
- Customer-requirement selection.
- Vehicle and scenario selection.
- Quotation preparation.
- Approval request.
- Customer presentation.
- Negotiation documentation.
- Revision request.

Must not independently:

- Approve restricted discounts.
- Approve below-floor pricing.
- Override tax.
- Override restricted fees.
- Approve Trade-In exceptions.
- Approve finance.
- Mark Customer acceptance without evidence.
- Modify issued commercial content.
- Create a Deal outside authority.
- Access unrelated Tenant Quotations.

#### Sales Manager

May perform configured:

- Discount approval.
- Margin review.
- Quotation withdrawal approval.
- Commercial exception review.
- Disputed acceptance review.
- Deal-conversion review.

Manager access does not automatically authorize:

- Tax-law override.
- Finance approval.
- Legal override.
- Payment Confirmation.
- Contract signature.
- Cross-Tenant access.
- Vehicle delivery Confirmation.

#### Pricing Manager

May access and approve permitted:

- Price boundaries.
- Discount programs.
- Incentive programs.
- Margin exceptions.
- Commercial pricing corrections.

#### Finance Specialist

May access finance assumptions and approved finance context required for the Quotation.

Finance access must not expose unrelated Customer or internal pricing information beyond approved purpose.

#### Trade-In Specialist

May access Trade-In values and evidence required for the Quotation.

#### Compliance or Legal Reviewer

May access restricted terms, disclosures, tax exceptions, and dispute evidence required for an assigned case.

#### Data Steward

May review:

- Version conflicts.
- Source mappings.
- Calculation conflicts.
- Relationship inconsistencies.
- Reconciliation cases.
- Data-quality issues.

#### AI Agent

May access only the minimum Quotation context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unauthorized access to internal prices, margins, approval thresholds, finance data, Trade-In evidence, and acceptance identity evidence.

### Field-Level Protection

Restricted fields should use:

- Field-level authorization.
- Masking.
- Encryption where appropriate.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Restricted examples include:

- Acquisition cost.
- Internal price floor.
- Maximum discount.
- Estimated gross profit.
- Estimated gross margin.
- Approval thresholds.
- Finance assumptions.
- Trade-In payoff.
- Acceptance identity evidence.

### Customer-Facing Document Security

Issued Quotation documents must:

- Use controlled storage.
- Use authenticated or tokenized access.
- Prevent predictable identifiers.
- Support access expiration where appropriate.
- Preserve document hash.
- Prevent unauthorized modification.
- Avoid exposing internal-only fields.
- Avoid unrestricted search indexing.
- Use approved download and sharing controls.
- Preserve access audit evidence.

### Acceptance Security

Customer acceptance workflows must protect against:

- Wrong-Customer acceptance.
- Wrong-document acceptance.
- Wrong-version acceptance.
- Expired-link acceptance.
- Replayed acceptance.
- Tampered document hash.
- Unauthorized representative.
- Duplicate processing.
- Disputed identity.

Acceptance must preserve sufficient evidence according to risk and applicable law.

### Communication Security

Before Quotation delivery or follow-up, deterministic controls must validate:

- Customer permission.
- Purpose.
- Channel.
- Approved template.
- Correct document.
- Current Quotation version.
- Validity.
- Frequency.
- Quiet hours.
- Customer restrictions.
- Human Approval or approved automation policy.

Prompt text, User interface state, Opportunity priority, or AI Recommendation must not bypass these controls.

### Pricing and Margin Protection

Internal pricing and margin data must not appear in:

- Customer-facing documents.
- Public APIs.
- Public search indexes.
- Unrestricted Logs.
- General-purpose embeddings.
- Unauthorized exports.
- Customer communications.

The platform must detect or prevent:

- Unauthorized price-floor access.
- Discount-limit exposure.
- Margin manipulation.
- Hidden-fee insertion.
- Calculation tampering.
- Approval bypass.
- Issued-document replacement.
- Commercial snapshot alteration.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Search.
- Quotation comparison.
- Pricing analysis.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Document storage.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Quotation Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Requested action.
- Quotation identifier and version.
- Document hash where applicable.
- Current record version.
- Field-level write authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Quotation activity must record:

- `tenant_id`.
- `quotation_id`.
- `quotation_series_id`.
- Quotation version.
- Opportunity and Customer references.
- Vehicle and Inventory references.
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
- Pricing authority.
- Approval or Human Decision.
- Automation-policy reference.
- AI involvement.
- Recommendation.
- Command.
- Idempotency key.
- External Confirmation.
- Document hash.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Quotation access attempts.
- Unauthorized pricing access.
- Unauthorized discount changes.
- Margin export attempts.
- Approval bypass.
- Tax or fee manipulation.
- Calculation tampering.
- Issued-document modification attempts.
- Document-hash mismatch.
- Wrong-Customer document access.
- Acceptance replay.
- False acceptance recording.
- Quotation withdrawal manipulation.
- Duplicate Deal conversion.
- Command replay.
- External Confirmation mismatch.
- AI access outside approved scope.
- Audit-log tampering.
- Suspicious bulk Quotation export.

### Commercial Integrity

The platform must detect or prevent:

- Hidden fees.
- Undisclosed optional products.
- Unauthorized discounts.
- Invalid rebates.
- Expired incentives.
- Stale finance assumptions.
- Stale Trade-In payoff.
- Stale Inventory availability.
- Inconsistent totals.
- Multiple current versions.
- Acceptance of the wrong version.
- Deal creation from an unaccepted version.
- Post-issue commercial modification.
- Quotation status manipulation.

### Privacy and Retention

Quotation retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Commercial-dispute requirements.
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
- Customer-access systems.
- Backups according to policy.

Required non-personal commercial, legal, tax, security, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Quotation issue.
- Customer delivery.
- Automated follow-up.
- Pricing updates.
- Discount approvals.
- Finance illustrations.
- Deal conversion.
- External write-back.
- AI Quotation Recommendations.
- Quotation export.
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
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Deal](./Deal.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Quotation baseline.

Issued Quotation versions are immutable.

A material commercial change requires a new version.

Quotation issue remains separate from Customer delivery.

Customer acceptance must reference the exact issued Quotation version and document hash.

Quotation acceptance does not independently prove Vehicle Reservation, Vehicle Allocation, finance approval, contract signature, Payment, Deal completion, sale, or delivery.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
