# Inventory Record

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Inventory Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-02  

---

## 1. Object Purpose

### Business Purpose

The Inventory Record Object represents one governed dealership stock cycle for one specific physical Vehicle inside one ASOS Tenant.

It records how the Vehicle is:

- Expected or acquired.
- Accepted into dealership stock.
- Assigned an Inventory identity.
- Located and physically verified.
- Inspected and prepared.
- Priced and published.
- Made commercially available.
- Reserved and allocated.
- Transferred.
- Sold and delivered.
- Returned, retired, or archived.

The Inventory Record answers operational questions such as:

- Does a valid stock record exist for this Vehicle?
- Which dealership, branch, and location currently control the stock cycle?
- Was the Vehicle accepted through an OEM, supplier, transfer, Trade-In, consignment, or another approved intake source?
- Is physical possession verified?
- Is legal or operational acquisition evidence complete?
- Is the Vehicle commercially available?
- Is it ready for sale, test drive, allocation, or delivery?
- Is it reserved or allocated?
- Is a transfer, sale, return, or exit workflow pending?
- Is the Inventory projection current, confirmed, and reconciled?
- Is a safety, quality, legal, financial, ownership, compliance, or data block active?

### Vehicle, Trade-In, and Inventory Separation

The following boundaries are mandatory:

```text
Vehicle
  = canonical physical Vehicle identity and technical specification

Trade-In
  = appraisal, ownership verification, lien and payoff,
    Customer offer, acquisition readiness, and legal acquisition workflow

Inventory Record
  = dealership-specific stock identity, intake acceptance,
    stock lifecycle, location, availability, preparation,
    pricing, Reservation, Allocation, transfer, sale, and exit context
```

The Vehicle Domain Service owns:

- VIN and chassis identity.
- Make, model, trim, variant, and model year.
- Technical specification.
- Factory configuration.
- Identity-resolution evidence.
- Odometer evidence and identity-level conflicts.

The Trade-In Domain Service owns:

- Trade-In appraisal and appraisal versions.
- Actual cash value.
- Customer-facing Trade-In allowance.
- Lien and payoff verification.
- Positive and negative equity.
- Customer acceptance of the Trade-In offer.
- Acquisition readiness.
- Legal acquisition workflow.
- Ownership-transfer and payoff evidence.
- The request for dealership Inventory intake after valid acquisition.
- Read-only tracking of the resulting Inventory Record reference.

The Inventory Domain Service owns:

- Inventory-intake validation.
- Intake-request acceptance or rejection.
- Inventory-intake idempotency.
- Creation of the canonical Inventory Record.
- Activation of the current stock cycle.
- Inventory number and stock-number governance.
- Dealership, branch, and location stock context.
- Physical-presence workflow.
- Inventory lifecycle.
- Commercial availability.
- Preparation and readiness.
- Inventory pricing context.
- Publication.
- Reservation and Allocation workflow unless delegated to an approved dedicated service.
- Transfer, sale, delivery, return, retirement, and archival projections.
- Inventory reconciliation.

The Trade-In Domain Service must not:

- Create an Inventory Record directly.
- Assign the authoritative `inventory_record_id`.
- Assign the authoritative stock number.
- Activate dealership stock.
- Set authoritative Inventory lifecycle or availability state.
- Publish authoritative Inventory Record-created or Inventory-activated Events.
- Treat an intake request, API acknowledgement, or sent Command as proof that Inventory intake completed.

The Inventory Domain Service must not:

- Recalculate or overwrite the Trade-In appraisal.
- Change the approved Trade-In allowance.
- Decide payoff, lien, equity, or legal acquisition outcomes.
- Represent incomplete Trade-In acquisition evidence as completed.
- Treat a Trade-In intake request as sufficient evidence for stock activation.

### Inventory Intake Ownership

A Trade-In Vehicle becomes active dealership Inventory only through a governed Inventory intake workflow.

The workflow must distinguish:

```text
Trade-In acquisition completed
  ≠ Inventory intake requested

Inventory intake requested
  ≠ Inventory intake accepted

Inventory intake accepted
  ≠ Inventory Record created

Inventory Record created
  ≠ Inventory Record activated

Inventory Record activated
  ≠ Vehicle commercially available
```

For a Trade-In source, the Inventory Domain Service must validate at least:

- Exact `trade_in_id`.
- Exact Trade-In record version.
- Exact accepted acquisition or handoff snapshot.
- Canonical `vehicle_id`.
- Tenant, dealership, branch, and legal-entity consistency.
- Legal acquisition status.
- Ownership-transfer evidence.
- Lien, payoff, and release conditions.
- Physical-possession evidence.
- Approved intake authority.
- Applicable inspection and quality requirements.
- Acquisition-cost handoff and source authority.
- Duplicate current-stock-cycle risk.
- Required Human Decision or approved automation policy.
- Idempotency key.
- External system requirements.

The Inventory Domain Service may:

- Reject an incomplete or conflicting intake request.
- Require Human Review.
- Create a planned or inactive Inventory Record while external confirmation is pending.
- Create a canonical Inventory Record after accepted internal authority.
- Submit an idempotent external stock-creation Command where a DMS or Inventory Management System is authoritative.
- Activate the stock cycle only after the configured activation evidence exists.
- Return the accepted `inventory_record_id`, record version, and reconciliation references to Trade-In.

### External Inventory Authority

The configured DMS, Inventory Management System, accounting platform, OEM system, or another approved external authority may remain authoritative for selected facts, including:

- External stock-record creation.
- External stock number.
- Physical receipt.
- Legal ownership posting.
- Acquisition cost posting.
- Final posted pricing.
- Reservation or Allocation completion.
- Sale posting.
- Delivery posting.
- Transfer completion.
- Accounting entries.

When an external system is authoritative:

- Inventory Domain Service owns the canonical Command workflow.
- The Command must be persisted before transmission.
- Safe retries must use one stable idempotency key.
- Local state must remain pending until authoritative External Confirmation.
- Transport success does not prove business completion.
- Conflicting or delayed outcomes must enter reconciliation.
- Inventory Domain Service publishes the accepted canonical projection after validation.

### Historical Stock Cycles

A physical Vehicle may have multiple historical Inventory Records because of:

- Dealer transfer.
- Return and reacquisition.
- Trade-In acquisition.
- Repurchase or buyback.
- Consignment renewal.
- Ownership change.
- Separate holding periods.
- Data correction.
- Re-entry after delivery or disposal.

Only one Inventory Record may normally claim current physical possession and active commercial control for the same physical Vehicle inside the same Tenant.

A planned destination record may exist during transfer, but it must not claim current physical possession before accepted receiving evidence.

A delivered, transferred-out, returned, retired, or archived stock cycle must not be reopened through an ordinary update. Re-entry normally requires a new Inventory Record linked through supersession or reacquisition lineage.

### System Purpose

The Inventory Record provides canonical Inventory context to:

- Vehicle search and matching.
- Lead and Opportunity workflows.
- Appointment and test-drive scheduling.
- Quotation preparation.
- Reservation and Allocation.
- Deal execution.
- Trade-In intake.
- Finance and Financial Contract readiness.
- Publication channels.
- Pricing and markdown review.
- Inventory aging analysis.
- Transfer workflows.
- Delivery workflows.
- Accounting and DMS integrations.
- Management reporting.
- AI Agents.
- Audit and reconciliation services.

The Inventory Record may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Canonical Ownership Matrix

| Information | Default Authority |
| :--- | :--- |
| Vehicle identity and technical specification | Vehicle Domain Service |
| Trade-In appraisal, payoff, equity, and acquisition workflow | Trade-In Domain Service and configured authorities |
| Inventory-intake validation and acceptance | Inventory Domain Service |
| Canonical Inventory Record creation and activation | Inventory Domain Service |
| External stock posting | Configured DMS or Inventory authority |
| Dealership stock identity and lifecycle | Inventory Domain Service |
| Physical presence and location | Inventory workflow or configured external authority |
| Commercial availability | Inventory Domain Service using accepted evidence |
| Reservation and Allocation workflow | Inventory Domain Service or approved dedicated service |
| Inventory pricing context | Inventory Domain Service and approved pricing authority |
| Customer-specific commercial offer | Quotation Domain Service |
| Final commercial transaction | Deal Domain Service |
| Customer Payment evidence | Payment authority |
| Finance approval | Lender through Finance Application |
| Financial Contract and funding workflow | Financial Contract Domain Service |
| Delivery outcome | Delivery workflow or configured external authority |
| Accounting outcome | Configured accounting or DMS authority |
| Inventory intelligence | Derived Intelligence |
| External operation completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `inventory_record_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `vehicle_id` — UUIDv4, required and immutable for the stock cycle.
- `record_version` — Integer used for optimistic concurrency.
- `inventory_cycle_number` — Integer or stable sequence for the Vehicle inside the Tenant.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `location_id`.
- `legal_entity_id`.
- `inventory_department_id`.
- `responsible_team_id`.
- `assigned_inventory_user_id`.
- `responsible_inventory_manager_user_id`.

`tenant_id` is the primary isolation boundary.

`dealership_id` is required for an active Inventory Record.

`branch_id` and `location_id` may remain absent while stock is planned, ordered, externally pending, or in transit.

They become required before confirmed physical receipt or ordinary commercial availability.

### Related Domain Objects

- `vehicle_id`.
- `trade_in_id`.
- `supplier_id`.
- `oem_id`.
- `acquisition_order_id`.
- `source_inventory_record_id`.
- `supersedes_inventory_record_id`.
- `superseded_by_inventory_record_id`.
- `current_reservation_id`.
- `current_allocation_id`.
- `allocated_customer_id`.
- `allocated_opportunity_id`.
- `allocated_deal_id`.
- `sold_deal_id`.
- `current_transfer_id`.
- `delivery_reference`.
- `accounting_reference`.
- `compliance_case_id`.
- `dispute_case_id`.

### Inventory Identity

- `inventory_number`.
- `stock_number`.
- `external_stock_reference`.
- `inventory_type`.
- `ownership_type`.
- `vehicle_condition`.
- `status`.
- `availability_status`.
- `physical_presence_status`.
- `workflow_authority_mode`.
- `is_current_record`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Inventory Intake Context

- `inventory_intake_request_id`.
- `inventory_intake_status`.
- `inventory_intake_source_type`.
- `inventory_intake_source_id`.
- `inventory_intake_source_record_version`.
- `inventory_intake_snapshot`.
- `inventory_intake_snapshot_hash`.
- `inventory_intake_requested_at`.
- `inventory_intake_requested_by_actor_type`.
- `inventory_intake_requested_by_actor_id`.
- `inventory_intake_validated_at`.
- `inventory_intake_accepted_at`.
- `inventory_intake_rejected_at`.
- `inventory_intake_rejection_reasons`.
- `inventory_intake_idempotency_key`.
- `inventory_intake_command_id`.
- `inventory_intake_confirmation_status`.
- `inventory_intake_confirmation_reference`.
- `inventory_intake_confirmation_source_record_version`.
- `inventory_intake_reconciliation_status`.
- `inventory_activated_at`.

### Trade-In Intake Projection

When the intake source is Trade-In, the Inventory Record or intake request may preserve:

- `trade_in_id`.
- `trade_in_record_version`.
- `trade_in_acquisition_status_projection`.
- `trade_in_acquisition_snapshot_hash`.
- `trade_in_ownership_transfer_status_projection`.
- `trade_in_payoff_status_projection`.
- `trade_in_lien_release_status_projection`.
- `trade_in_physical_possession_status_projection`.
- `trade_in_acquisition_completed_at_projection`.
- `trade_in_evidence_references`.
- `trade_in_projection_observed_at`.
- `trade_in_projection_freshness_status`.

These values are evidence-backed projections.

Inventory must not modify the authoritative Trade-In workflow.

### Vehicle Reference Projection

- `vehicle_snapshot`.
- `vehicle_snapshot_hash`.
- `vin_projection`.
- `make_projection`.
- `model_projection`.
- `trim_projection`.
- `variant_projection`.
- `model_year_projection`.
- `odometer_projection`.
- `odometer_unit_projection`.
- `vehicle_identity_status_projection`.
- `vehicle_source_record_version`.
- `vehicle_projection_observed_at`.

The authoritative Vehicle identity remains with the Vehicle Domain Service.

### Acquisition Context

- `acquisition_source`.
- `acquisition_status`.
- `acquisition_date`.
- `acquisition_reference`.
- `supplier_id`.
- `oem_id`.
- `trade_in_id`.
- `acquisition_order_id`.
- `purchase_price_amount`.
- `transport_cost_amount`.
- `inspection_cost_amount`.
- `reconditioning_cost_amount`.
- `registration_cost_amount`.
- `tax_cost_amount`.
- `other_acquisition_cost_amount`.
- `landed_cost_amount`.
- `acquisition_currency_code`.
- `acquisition_cost_authority`.
- `acquisition_evidence_references`.
- `acquisition_reconciliation_status`.

Trade-In appraisal values must not automatically become Inventory acquisition-cost values.

Cost handoff requires explicit authority, classification, and reconciliation.

### Location and Physical Presence

- `branch_id`.
- `location_id`.
- `location_type`.
- `parking_slot`.
- `display_zone`.
- `storage_zone`.
- `physical_presence_status`.
- `presence_verification_id`.
- `presence_verification_method`.
- `last_location_verified_at`.
- `last_location_verified_by_actor_id`.
- `location_source_authority`.
- `location_confirmation_status`.
- `location_confirmation_reference`.
- `current_transfer_id`.

### Availability and Blocking

- `availability_status`.
- `available_from`.
- `available_until`.
- `availability_block_reason_codes`.
- `availability_blocked_at`.
- `availability_blocked_by_actor_id`.
- `availability_block_evidence_references`.
- `availability_freshness_status`.
- `last_availability_evaluated_at`.
- `last_availability_confirmed_at`.
- `availability_rule_set_id`.
- `availability_rule_set_version`.

### Reservation Projection

- `current_reservation_id`.
- `reservation_status`.
- `reserved_for_customer_id`.
- `reserved_for_opportunity_id`.
- `reserved_at`.
- `reservation_expires_at`.
- `reservation_authority_reference`.
- `reservation_confirmation_status`.
- `reservation_reconciliation_status`.

Reservation history must remain in governed child records.

### Allocation Projection

- `current_allocation_id`.
- `allocation_status`.
- `allocated_customer_id`.
- `allocated_opportunity_id`.
- `allocated_deal_id`.
- `allocated_at`.
- `allocation_expires_at`.
- `allocation_authority_reference`.
- `allocation_confirmation_status`.
- `allocation_reconciliation_status`.

Allocation history must remain in governed child records.

### Pricing Context

- `currency_code`.
- `list_price_amount`.
- `advertised_price_amount`.
- `target_sale_price_amount`.
- `minimum_authorized_price_amount`.
- `maximum_discount_amount`.
- `current_discount_amount`.
- `market_reference_price_amount`.
- `estimated_gross_profit_amount`.
- `estimated_gross_margin_percentage`.
- `pricing_status`.
- `pricing_rule_id`.
- `pricing_rule_version`.
- `price_effective_from`.
- `price_effective_until`.
- `price_authority_reference`.
- `price_last_confirmed_at`.

The final Customer-specific price belongs to Quotation or Deal.

### Preparation and Readiness

- `inspection_status`.
- `reconditioning_status`.
- `cleaning_status`.
- `photography_status`.
- `documentation_status`.
- `registration_readiness_status`.
- `insurance_readiness_status`.
- `pre_delivery_inspection_status`.
- `quality_hold_status`.
- `quality_hold_reason_codes`.
- `sale_readiness_status`.
- `test_drive_readiness_status`.
- `delivery_readiness_status`.
- `readiness_evaluated_at`.
- `readiness_rule_set_id`.
- `readiness_rule_set_version`.

Readiness does not prove sale or delivery.

### Publication

- `publication_status`.
- `published_channel_ids`.
- `primary_sales_channel`.
- `published_at`.
- `last_publication_sync_at`.
- `publication_error_code`.
- `publication_confirmation_status`.
- `publication_reconciliation_status`.

### Floor-Plan and Holding-Cost Projection

- `floor_plan_provider_id`.
- `floor_plan_reference`.
- `floor_plan_status`.
- `floor_plan_principal_amount`.
- `floor_plan_interest_rate`.
- `floor_plan_start_date`.
- `floor_plan_maturity_date`.
- `floor_plan_daily_cost_amount`.
- `floor_plan_accrued_cost_amount`.
- `financial_holding_cost_amount`.
- `financial_authority_reference`.
- `floor_plan_projection_observed_at`.

These fields are restricted projections.

### Activity and Derived Intelligence

- `view_count`.
- `inquiry_count`.
- `lead_count`.
- `qualified_lead_count`.
- `opportunity_count`.
- `quotation_count`.
- `appointment_count`.
- `test_drive_count`.
- `reservation_count`.
- `deal_count`.
- `demand_score`.
- `engagement_score`.
- `market_interest_score`.
- `stock_pressure_score`.
- `inventory_age_band`.
- `aging_status`.
- `days_in_inventory`.
- `days_since_available`.
- `days_since_last_activity`.
- `recommended_markdown_amount`.
- `recommended_action`.
- `potential_gross_profit_amount`.
- `potential_gross_margin_percentage`.
- `requires_management_review`.
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

### Sale, Delivery, Transfer, and Exit Projection

- `sold_deal_id`.
- `sale_confirmation_status`.
- `sale_confirmation_reference`.
- `sold_at`.
- `sold_price_amount`.
- `delivery_confirmation_status`.
- `delivery_confirmation_reference`.
- `delivered_at`.
- `transfer_confirmation_status`.
- `transfer_confirmation_reference`.
- `transfer_out_at`.
- `returned_at`.
- `retired_at`.
- `archived_at`.
- `exit_reason`.
- `exit_reference`.
- `exit_reconciliation_status`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_record_version`.
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

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inventory_record_id` | UUID | Yes | Inventory Domain Service | Immutable canonical Inventory Record identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation boundary. |
| `vehicle_id` | UUID | Yes | Vehicle relationship | Physical Vehicle represented by this stock cycle. |
| `inventory_number` | String | Yes | Inventory Domain Service | Stable ASOS Inventory identity. |
| `stock_number` | String | Conditional | Inventory Domain Service or external authority | Operational stock number after accepted creation or confirmation. |
| `dealership_id` | UUID | Yes | Inventory context | Dealership controlling the stock cycle. |
| `branch_id` | UUID | Conditional | Inventory context | Responsible branch; required before ordinary active stock use. |
| `location_id` | UUID | Conditional | Inventory workflow | Current controlled location. |
| `status` | Enum | Yes | Inventory Domain Service | Current Inventory lifecycle state. |
| `availability_status` | Enum | Yes | Inventory Domain Service | Current permitted commercial availability. |
| `is_current_record` | Boolean | Yes | Inventory Domain Service | Whether this is the active stock cycle for the Vehicle. |
| `record_version` | Integer | Yes | Inventory Domain Service | Optimistic-concurrency version. |

### Intake Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inventory_intake_request_id` | UUID | Conditional | Inventory Domain Service | Canonical intake workflow identifier. |
| `inventory_intake_status` | Enum | Yes | Inventory Domain Service | Current intake workflow state. |
| `inventory_intake_source_type` | Enum | Yes | Inventory Domain Service | Source classification such as Trade-In, OEM, transfer, or supplier. |
| `inventory_intake_source_id` | UUID or String | Conditional | Source relationship | Source workflow or external record. |
| `inventory_intake_source_record_version` | String | Conditional | Source authority | Exact source version used for intake validation. |
| `inventory_intake_snapshot_hash` | String | Conditional | Inventory Domain Service | Integrity hash of the accepted intake snapshot. |
| `inventory_intake_idempotency_key` | String | Yes | Inventory Domain Service | Prevents duplicate intake and stock creation. |
| `inventory_intake_command_id` | UUID | No | Inventory Domain Service | Command identifier for external stock creation. |
| `inventory_intake_confirmation_status` | Enum | Yes | Confirmation workflow | External stock-creation Confirmation status. |
| `inventory_intake_confirmation_reference` | String | No | External authority | Evidence supporting external acceptance. |
| `inventory_activated_at` | Timestamp | No | Inventory Domain Service | Time the stock cycle became active. |

### Trade-In Handoff Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `trade_in_id` | UUID | Conditional | Trade-In relationship | Trade-In that requested the intake. |
| `trade_in_record_version` | Integer | Conditional | Trade-In Domain Service | Exact Trade-In version used. |
| `trade_in_acquisition_snapshot_hash` | String | Conditional | Trade-In Domain Service | Integrity hash of the acquisition handoff. |
| `trade_in_acquisition_status_projection` | Enum | Conditional | Trade-In projection | Observed legal acquisition status. |
| `trade_in_ownership_transfer_status_projection` | Enum | Conditional | Trade-In projection | Observed ownership-transfer state. |
| `trade_in_payoff_status_projection` | Enum | Conditional | Trade-In projection | Observed payoff state. |
| `trade_in_physical_possession_status_projection` | Enum | Conditional | Trade-In projection | Observed possession handoff state. |
| `trade_in_evidence_references` | Array | Conditional | Evidence repository | Evidence references required for intake validation. |

### Acquisition Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `acquisition_source` | Enum | Yes | Inventory intake | Source through which the Vehicle entered the stock cycle. |
| `acquisition_status` | Enum | Yes | Inventory projection | Inventory-side acquisition state. |
| `acquisition_date` | Date | Conditional | Accepted source evidence | Date dealership responsibility or ownership was accepted. |
| `acquisition_reference` | String | No | Source authority | Order, transfer, invoice, Trade-In, or acquisition reference. |
| `purchase_price_amount` | Decimal | Conditional | Approved cost authority | Accepted direct acquisition cost. |
| `landed_cost_amount` | Decimal | Conditional | Deterministic calculation | Approved total acquisition cost. |
| `acquisition_currency_code` | String | Conditional | Cost authority | ISO 4217 currency code. |
| `acquisition_evidence_references` | Array | Conditional | Evidence repository | Supporting ownership, possession, order, or acquisition evidence. |

### Availability Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `availability_status` | Enum | Yes | Inventory Domain Service | Permitted commercial availability state. |
| `physical_presence_status` | Enum | Yes | Inventory verification workflow | Verified physical-presence state. |
| `availability_freshness_status` | Enum | Yes | Deterministic calculation | Whether availability evidence remains current. |
| `availability_block_reason_codes` | Array | No | Policy or Human Decision | Active reasons blocking commercial use. |
| `last_availability_confirmed_at` | Timestamp | No | Accepted authority | Latest accepted availability evidence time. |

### Reservation and Allocation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `current_reservation_id` | UUID | No | Reservation workflow | Current active Reservation. |
| `reservation_status` | Enum | Yes | Inventory or delegated service | Current Reservation projection. |
| `reservation_expires_at` | Timestamp | No | Approved policy | Expiration for a time-limited Reservation. |
| `reservation_confirmation_status` | Enum | Yes | Confirmation workflow | External Reservation Confirmation state. |
| `current_allocation_id` | UUID | No | Allocation workflow | Current active Allocation. |
| `allocation_status` | Enum | Yes | Inventory or delegated service | Current Allocation projection. |
| `allocated_deal_id` | UUID | No | Deal relationship | Deal receiving the Allocation. |
| `allocation_confirmation_status` | Enum | Yes | Confirmation workflow | External Allocation Confirmation state. |

### Pricing Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `currency_code` | String | Conditional | Pricing authority | ISO 4217 currency code. |
| `list_price_amount` | Decimal | Conditional | Approved pricing authority | Standard dealership list price. |
| `advertised_price_amount` | Decimal | Conditional | Approved pricing authority | Current Customer-visible advertised price. |
| `minimum_authorized_price_amount` | Decimal | No | Restricted pricing authority | Lowest price permitted without additional approval. |
| `maximum_discount_amount` | Decimal | No | Restricted pricing authority | Delegated discount boundary. |
| `market_reference_price_amount` | Decimal | No | Market Intelligence | Evidence-backed market reference. |
| `pricing_status` | Enum | Yes | Pricing workflow | Current pricing validity and approval state. |
| `price_authority_reference` | String | No | Evidence repository | Policy, Decision, or external source supporting the price. |

### Readiness and Exit Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `sale_readiness_status` | Enum | Yes | Deterministic Inventory workflow | Whether sale-readiness requirements pass. |
| `test_drive_readiness_status` | Enum | Yes | Deterministic Inventory workflow | Whether test-drive requirements pass. |
| `delivery_readiness_status` | Enum | Yes | Deterministic Inventory workflow | Whether Inventory-side delivery requirements pass. |
| `sale_confirmation_status` | Enum | Yes | Deal or external projection | Whether the sale outcome is confirmed. |
| `delivery_confirmation_status` | Enum | Yes | Delivery projection | Whether delivery is confirmed. |
| `exit_reason` | Enum | No | Inventory workflow | Reason this stock cycle left active Inventory. |

---

## 4. Enumerations

### InventoryRecordStatus

- `PLANNED`
- `ORDERED`
- `IN_TRANSIT`
- `ACQUIRED`
- `RECEIVED`
- `INSPECTION_PENDING`
- `PREPARATION_IN_PROGRESS`
- `AVAILABLE`
- `RESERVED`
- `ALLOCATED`
- `SALE_PENDING`
- `SOLD`
- `DELIVERY_PENDING`
- `DELIVERED`
- `TRANSFER_PENDING`
- `TRANSFERRED_OUT`
- `RETURN_PENDING`
- `RETURNED`
- `BLOCKED`
- `RETIRED`
- `ARCHIVED`

### InventoryIntakeStatus

- `NOT_REQUIRED`
- `REQUESTED`
- `VALIDATION_PENDING`
- `VALIDATED`
- `COMMAND_PENDING`
- `PENDING_EXTERNAL_CONFIRMATION`
- `ACCEPTED`
- `RECORD_CREATED`
- `ACTIVATION_PENDING`
- `ACTIVATED`
- `REJECTED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### InventoryIntakeSourceType

- `OEM_ALLOCATION`
- `OEM_ORDER`
- `SUPPLIER_PURCHASE`
- `DEALER_TRANSFER`
- `TRADE_IN`
- `CUSTOMER_BUYBACK`
- `AUCTION`
- `FLEET_RETURN`
- `LEASE_RETURN`
- `CONSIGNMENT`
- `IMPORT`
- `MANUAL_GOVERNED_INTAKE`
- `OTHER`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_DMS_AUTHORITATIVE`
- `EXTERNAL_INVENTORY_SYSTEM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### InventoryType

- `RETAIL_STOCK`
- `DEMONSTRATOR`
- `TEST_DRIVE`
- `LOANER`
- `DISPLAY`
- `FLEET`
- `WHOLESALE`
- `CONSIGNMENT`
- `TRADE_IN_STOCK`
- `CERTIFIED_PRE_OWNED`
- `SERVICE_REPLACEMENT`
- `NON_RETAIL`
- `OTHER`

### InventoryOwnershipType

- `DEALERSHIP_OWNED`
- `FLOOR_PLAN_FINANCED`
- `OEM_OWNED`
- `SUPPLIER_OWNED`
- `CONSIGNMENT`
- `CUSTOMER_OWNED_PENDING_ACQUISITION`
- `LEASED`
- `DEMONSTRATION_LOAN`
- `OTHER`

### InventoryAvailabilityStatus

- `NOT_AVAILABLE`
- `AVAILABLE_FOR_SALE`
- `AVAILABLE_FOR_TEST_DRIVE`
- `AVAILABLE_FOR_TRANSFER`
- `AVAILABLE_FOR_WHOLESALE`
- `RESERVED`
- `ALLOCATED`
- `SALE_PENDING`
- `BLOCKED`
- `SOLD`
- `DELIVERED`
- `TRANSFERRED`
- `RETURNED`
- `RETIRED`

### InventoryAcquisitionStatus

- `NOT_STARTED`
- `PENDING`
- `EVIDENCE_PENDING`
- `IN_TRANSIT`
- `RECEIVED`
- `CONFIRMED`
- `REJECTED`
- `CANCELLED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### PhysicalPresenceStatus

- `NOT_VERIFIED`
- `VERIFICATION_PENDING`
- `VERIFIED_PRESENT`
- `VERIFIED_ABSENT`
- `LOCATION_MISMATCH`
- `IN_TRANSIT`
- `MANUAL_REVIEW_REQUIRED`

### InventoryReservationStatus

- `NOT_RESERVED`
- `CREATED`
- `AWAITING_APPROVAL`
- `APPROVED`
- `PENDING_CONFIRMATION`
- `ACTIVE`
- `EXPIRED`
- `RELEASE_PENDING`
- `RELEASED`
- `CONVERTED_TO_ALLOCATION`
- `REJECTED`
- `CANCELLED`
- `FAILED`
- `RECONCILIATION_REQUIRED`
- `DISPUTED`

### InventoryAllocationStatus

- `NOT_ALLOCATED`
- `CREATED`
- `AWAITING_APPROVAL`
- `APPROVED`
- `PENDING_CONFIRMATION`
- `ALLOCATED`
- `EXPIRED`
- `RELEASE_PENDING`
- `RELEASED`
- `CONVERTED_TO_SALE`
- `REJECTED`
- `CANCELLED`
- `FAILED`
- `RECONCILIATION_REQUIRED`
- `DISPUTED`

### InventoryReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REQUIRES_REVIEW`

### InventoryPreparationStatus

- `NOT_STARTED`
- `NOT_REQUIRED`
- `PENDING`
- `IN_PROGRESS`
- `COMPLETED`
- `FAILED`
- `BLOCKED`
- `EXPIRED`
- `MANUAL_REVIEW_REQUIRED`

### InventoryPublicationStatus

- `NOT_PUBLISHED`
- `READY_TO_PUBLISH`
- `AWAITING_APPROVAL`
- `PUBLISHING`
- `PENDING_CONFIRMATION`
- `PARTIALLY_PUBLISHED`
- `PUBLISHED`
- `UPDATE_PENDING`
- `UNPUBLISHING`
- `UNPUBLISHED`
- `FAILED`
- `BLOCKED`
- `RECONCILIATION_REQUIRED`

### InventoryPricingStatus

- `NOT_SET`
- `DRAFT`
- `REVIEW_REQUIRED`
- `APPROVED`
- `ACTIVE`
- `EXPIRED`
- `BLOCKED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### InventoryBlockReason

- `INTAKE_INCOMPLETE`
- `TRADE_IN_ACQUISITION_INCOMPLETE`
- `OWNERSHIP_EVIDENCE_MISSING`
- `PAYOFF_OR_LIEN_UNRESOLVED`
- `PHYSICAL_POSSESSION_UNCONFIRMED`
- `QUALITY_HOLD`
- `SAFETY_RECALL`
- `COMPLIANCE_HOLD`
- `DOCUMENTS_INCOMPLETE`
- `LOCATION_UNVERIFIED`
- `DAMAGE_DETECTED`
- `PRICE_REVIEW_REQUIRED`
- `OWNERSHIP_DISPUTE`
- `TRANSFER_IN_PROGRESS`
- `LEGAL_HOLD`
- `FRAUD_REVIEW`
- `DATA_CONFLICT`
- `STALE_AVAILABILITY`
- `OTHER`

### InventoryExitReason

- `SOLD`
- `DELIVERED`
- `TRANSFERRED`
- `RETURNED_TO_SUPPLIER`
- `RETURNED_TO_OWNER`
- `WHOLESALE_DISPOSAL`
- `AUCTION_DISPOSAL`
- `WRITTEN_OFF`
- `TOTAL_LOSS`
- `DAMAGED_BEYOND_REPAIR`
- `DUPLICATE_RECORD`
- `DATA_CORRECTION`
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

### ReconciliationStatus

- `NOT_REQUIRED`
- `CURRENT`
- `PENDING`
- `IN_PROGRESS`
- `RESOLVED`
- `FAILED`
- `MANUAL_REVIEW_REQUIRED`

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

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- All related Domain Objects must belong to the permitted Tenant.
- Dealership, branch, location, legal entity, team, User, and authority references must belong to the permitted organizational scope.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.
- Cross-Tenant Inventory matching, intake, creation, Reservation, Allocation, transfer, publication, or reconciliation is prohibited unless governed by an approved and auditable mechanism.

### Vehicle Relationship Rules

- Every Inventory Record must reference one physical Vehicle.
- A catalog-only Vehicle configuration must not create a physical Inventory Record.
- Vehicle identity fields inside Inventory remain projections.
- A material Vehicle identity conflict must block intake activation and commercial availability.
- Vehicle merge or identity correction must trigger Inventory reconciliation.
- Inventory lifecycle changes must not rewrite Vehicle identity.

### Inventory Intake Rules

An Inventory intake request must contain:

- Source type.
- Source identifier.
- Source record version.
- Vehicle identifier.
- Target dealership and legal entity.
- Requested Inventory type.
- Acquisition or holding basis.
- Required evidence references.
- Source snapshot hash.
- Stable idempotency key.
- Requesting actor and authority.
- Expected external authority mode.

Inventory Domain Service must validate:

- Tenant and organizational consistency.
- Vehicle identity and duplicate current-stock-cycle risk.
- Source workflow authority.
- Acquisition and possession evidence.
- Ownership, lien, payoff, title, legal, and compliance conditions where applicable.
- Source-data freshness.
- Cost authority and currency where cost is supplied.
- Required approval.
- External integration requirements.

An intake request must not directly choose the final `inventory_record_id`.

A duplicate retry with the same source intent and idempotency key must return the existing intake result.

A materially changed source version must use a new governed intake version or correction workflow.

### Trade-In Intake Rules

A Trade-In intake request may be accepted only when:

- Exact `trade_in_id` and record version exist.
- Canonical `vehicle_id` matches.
- Trade-In acquisition status satisfies policy.
- Required Customer and dealership transaction references match.
- Ownership-transfer evidence is accepted.
- Required payoff or lien conditions are satisfied or explicitly governed.
- Physical possession is accepted or an approved pending-possession model applies.
- Required inspection and damage information is available.
- No blocking legal, fraud, compliance, title, or identity conflict exists.
- Required Human authority or approved policy exists.
- Intake snapshot and evidence are immutable and traceable.

Trade-In appraisal, allowance, and equity values are not authoritative Inventory costs by default.

Inventory must not create active commercial stock from a Trade-In that remains only:

- Appraised.
- Offered.
- Customer accepted.
- Deal associated.
- Payoff requested.
- Acquisition pending.
- Possession unconfirmed.

### Inventory Record Creation Rules

Only Inventory Domain Service may create the canonical Inventory Record.

Creation must:

- Use a new immutable `inventory_record_id`.
- Preserve the intake request.
- Preserve source record versions.
- Preserve Vehicle and organizational references.
- Set one initial lifecycle state.
- Set `availability_status = NOT_AVAILABLE` unless a separately approved future-stock policy applies.
- Prevent duplicate current-stock cycles.
- Preserve creation authority and evidence.
- Publish an accepted Inventory-created Event.

When external stock creation is authoritative:

- Inventory must persist the request and Command before transmission.
- Inventory must remain pending.
- External stock reference and source version must be preserved.
- Rejected or conflicting confirmation must not create active stock.
- Reconciliation must resolve duplicate or mismatched external records.

### Inventory Activation Rules

Inventory activation is distinct from record creation.

Activation requires:

- Valid current Inventory Record.
- Accepted intake.
- Required external stock-creation Confirmation where applicable.
- Valid dealership and branch context.
- Accepted acquisition or holding evidence.
- No unresolved duplicate current-stock-cycle conflict.
- No blocking ownership, legal, compliance, quality, financial, or data conflict.
- Activation Decision or approved deterministic policy.
- Activation timestamp and evidence.

Activation does not automatically make the Vehicle available for sale.

### Current Record Rules

- Only one Inventory Record may normally claim current physical possession for one Vehicle inside a Tenant.
- Only one current record may claim active commercial control.
- `is_current_record = true` must be concurrency-protected.
- Planned destination transfer records must not claim current physical possession.
- Transferred-out, returned, retired, delivered, or archived records must not remain active.
- Re-entry after a completed stock cycle normally requires a new linked record.

### Acquisition and Cost Rules

- Acquisition source is required.
- Acquisition evidence is required before ownership-dependent commercial use.
- Cost values must be zero or greater.
- Currency is required when cost exists.
- `landed_cost_amount` must use an approved deterministic formula.
- Trade-In appraisal or allowance must not silently become purchase cost.
- Restricted cost values must remain access-controlled.
- AI must not create authoritative cost values.
- Cost corrections must preserve original values, authority, reason, and evidence.

### Location and Presence Rules

- A Vehicle must not be marked physically present without accepted evidence.
- Branch and location are required before confirmed receipt unless policy explicitly permits otherwise.
- A Vehicle in transit must not be represented as present at the destination.
- Stale or conflicting location data must block Customer-facing availability where required.
- External location changes remain pending until Confirmation.
- Secure location data must not be exposed to unauthorized Users or public channels.

### Availability Rules

A Vehicle may become `AVAILABLE_FOR_SALE` only when:

- Inventory Record is current and activated.
- Physical presence is verified or an approved future-stock policy applies.
- Acquisition or holding evidence is valid.
- Required inspection and preparation pass.
- Approved current pricing exists.
- Availability evidence is within its Freshness SLA.
- No incompatible Reservation or Allocation exists.
- No active safety, quality, legal, ownership, lien, financial, compliance, fraud, or data block exists.

A Vehicle may become `AVAILABLE_FOR_TEST_DRIVE` only when:

- Physical presence is verified.
- Test-drive readiness is `READY`.
- Insurance, registration, safety, preparation, and operational requirements pass.
- No incompatible Reservation, Allocation, or block exists.

### Reservation and Allocation Rules

- Reservation and Allocation must use atomic concurrency-safe operations.
- A Vehicle must not have incompatible active Reservations.
- A Vehicle must not be allocated to multiple active Deals.
- Time-limited holds must include expiration.
- External Reservation or Allocation authority requires Confirmation.
- Approval does not prove external completion.
- Release and expiration must preserve history.
- Allocation override requires an Authoritative Human Decision.
- AI Agents must not independently reserve or allocate outside approved policy.

### Pricing Rules

- Price and cost amounts must be zero or greater.
- Currency is required when a price exists.
- Final Customer-specific price belongs to Quotation or Deal.
- Advertised price must not be represented as an accepted Deal price.
- Price floors, margins, costs, and discount limits are restricted.
- AI pricing output remains a Recommendation until approved.
- Material price changes must preserve previous value, new value, authority, reason, effective period, approval, evidence, and record version.

### Preparation, Publication, Sale, and Delivery Rules

- Readiness must derive from deterministic requirements.
- Safety, quality, legal, and compliance blocks override commercial readiness.
- Publication must expose only approved Customer-visible fields.
- Publication remains pending until provider Confirmation where required.
- `SOLD` requires accepted Deal or configured external sale evidence.
- A Deal-posting Command does not prove sale.
- `DELIVERED` requires authoritative delivery evidence.
- A sold or delivered record must not return to `AVAILABLE` through an ordinary update.
- Sale cancellation, return, repurchase, or reacquisition requires a controlled workflow.

### Transfer Rules

- Transfer must identify source, destination, Vehicle, responsible authority, expected timing, and evidence.
- Transfer initiation does not prove departure or destination receipt.
- Source possession remains authoritative until accepted departure evidence.
- Destination possession requires accepted receipt evidence.
- Retry must not create duplicate destination Inventory Records.
- Completed transfer must reconcile current-record ownership.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Intake creation, Inventory creation, Reservation, Allocation, transfer, publication, and external writes must support idempotency.
- Duplicate retries must not create duplicate:
  - Intake requests.
  - Inventory Records.
  - Stock numbers.
  - Reservations.
  - Allocations.
  - Transfers.
  - Publications.
  - External updates.
- Event Consumers must prevent duplicate business effects using `event_id`.

### Human Review Requirements

Human Review is required according to policy for:

- Duplicate current-stock-cycle risk.
- Vehicle identity conflict.
- Trade-In acquisition mismatch.
- Ownership, title, lien, or payoff conflict.
- Physical-possession conflict.
- Acquisition-cost mismatch.
- External stock-record mismatch.
- Location conflict.
- Reservation or Allocation conflict.
- Pricing exception.
- Safety, quality, legal, compliance, or fraud block.
- Transfer conflict.
- Sale or delivery conflict.
- Reopening a completed stock cycle.
- Another material operational, financial, legal, or data risk.

---

## 6. State Machine

### Inventory Lifecycle States

```text
PLANNED
ORDERED
IN_TRANSIT
ACQUIRED
RECEIVED
INSPECTION_PENDING
PREPARATION_IN_PROGRESS
AVAILABLE
RESERVED
ALLOCATED
SALE_PENDING
SOLD
DELIVERY_PENDING
DELIVERED
TRANSFER_PENDING
TRANSFERRED_OUT
RETURN_PENDING
RETURNED
BLOCKED
RETIRED
ARCHIVED
```

Inventory intake is a separate workflow and must not be confused with the Inventory Record lifecycle.

### Principal Allowed Transitions

```text
PLANNED → ORDERED
PLANNED → ACQUIRED
PLANNED → BLOCKED
PLANNED → RETIRED

ORDERED → IN_TRANSIT
ORDERED → ACQUIRED
ORDERED → RETURN_PENDING
ORDERED → BLOCKED
ORDERED → RETIRED

IN_TRANSIT → RECEIVED
IN_TRANSIT → BLOCKED
IN_TRANSIT → RETURN_PENDING

ACQUIRED → RECEIVED
ACQUIRED → INSPECTION_PENDING
ACQUIRED → PREPARATION_IN_PROGRESS
ACQUIRED → BLOCKED

RECEIVED → INSPECTION_PENDING
RECEIVED → PREPARATION_IN_PROGRESS
RECEIVED → BLOCKED

INSPECTION_PENDING → PREPARATION_IN_PROGRESS
INSPECTION_PENDING → AVAILABLE
INSPECTION_PENDING → BLOCKED

PREPARATION_IN_PROGRESS → AVAILABLE
PREPARATION_IN_PROGRESS → BLOCKED

AVAILABLE → RESERVED
AVAILABLE → ALLOCATED
AVAILABLE → SALE_PENDING
AVAILABLE → TRANSFER_PENDING
AVAILABLE → RETURN_PENDING
AVAILABLE → BLOCKED
AVAILABLE → RETIRED

RESERVED → AVAILABLE
RESERVED → ALLOCATED
RESERVED → SALE_PENDING
RESERVED → BLOCKED

ALLOCATED → AVAILABLE
ALLOCATED → SALE_PENDING
ALLOCATED → BLOCKED

SALE_PENDING → SOLD
SALE_PENDING → AVAILABLE
SALE_PENDING → BLOCKED

SOLD → DELIVERY_PENDING
SOLD → BLOCKED

DELIVERY_PENDING → DELIVERED
DELIVERY_PENDING → BLOCKED

TRANSFER_PENDING → TRANSFERRED_OUT
TRANSFER_PENDING → AVAILABLE
TRANSFER_PENDING → BLOCKED

RETURN_PENDING → RETURNED
RETURN_PENDING → AVAILABLE
RETURN_PENDING → BLOCKED

BLOCKED → previous permitted non-terminal state
BLOCKED → RETURN_PENDING
BLOCKED → RETIRED

DELIVERED → ARCHIVED
TRANSFERRED_OUT → ARCHIVED
RETURNED → ARCHIVED
RETIRED → ARCHIVED
```

Returning from `BLOCKED` requires accepted evidence that the blocking condition was cleared.

### Forbidden Ordinary Transitions

```text
PLANNED → AVAILABLE without activation and readiness
ORDERED → SOLD
IN_TRANSIT → AVAILABLE without receipt or approved future-stock policy
ACQUIRED → AVAILABLE without required intake, preparation, and readiness
AVAILABLE → DELIVERED
RESERVED → SOLD without sale workflow
ALLOCATED → DELIVERED
SALE_PENDING → DELIVERED
SOLD → AVAILABLE
DELIVERED → AVAILABLE
TRANSFERRED_OUT → AVAILABLE
RETURNED → AVAILABLE
RETIRED → AVAILABLE
ARCHIVED → AVAILABLE
```

### Entering PLANNED

Requires:

- Canonical Inventory Record created by Inventory Domain Service.
- Valid Vehicle.
- Tenant and organizational scope.
- Accepted intake or approved planned-stock basis.
- No duplicate current-stock-cycle conflict.
- Creation authority.
- Audit evidence.

### Entering ACQUIRED

Requires:

- Accepted acquisition or holding basis.
- Source and evidence.
- Required Trade-In, supplier, OEM, transfer, or external references.
- Cost authority where applicable.
- No blocking ownership conflict.

### Entering RECEIVED

Requires:

- Accepted physical receipt or approved external Confirmation.
- Dealership, branch, and location.
- Physical-presence evidence.
- Receipt timestamp.
- Reconciliation with expected stock.

### Entering AVAILABLE

Requires:

- Current activated Inventory Record.
- Required physical-presence or future-stock evidence.
- Valid acquisition or holding evidence.
- Completed required inspection and preparation.
- Approved current pricing.
- Current availability evidence.
- No incompatible Reservation or Allocation.
- No active block.

### Entering RESERVED

Requires:

- Eligible current Inventory Record.
- Atomic Reservation.
- Valid Customer or business purpose.
- Required approval or automation policy.
- Expiration where applicable.
- External Confirmation where required.

### Entering ALLOCATED

Requires:

- Eligible current Inventory Record.
- Atomic Allocation.
- Valid Opportunity or Deal.
- Required Human authority.
- External Confirmation where required.

### Entering SOLD

Requires:

- Valid Deal.
- Accepted sale evidence.
- Required Human or external authority.
- Final sale reference.
- Reservation and Allocation reconciliation.

### Entering DELIVERED

Requires:

- Confirmed sale.
- Authorized Vehicle release.
- Authoritative delivery evidence.
- Delivery timestamp.
- Required external Confirmation.
- Inventory exit reconciliation.

### Entering TRANSFERRED_OUT

Requires:

- Approved transfer.
- Confirmed departure.
- Destination or receiving authority evidence where required.
- Removal of active commercial availability.
- Current-record reconciliation.

### Terminal States

For an ordinary stock cycle:

- `DELIVERED`
- `TRANSFERRED_OUT`
- `RETURNED`
- `ARCHIVED`

`RETIRED` is inactive and normally proceeds to `ARCHIVED`.

A terminal stock cycle must not be reopened through an ordinary lifecycle transition.

### Correction and Re-entry

Correcting a material Inventory outcome requires:

- Authorized Human Decision or approved correction authority.
- Reason and evidence.
- Source and external impact assessment.
- New immutable Events.
- Preserved original history.
- Reconciliation.
- New Inventory Record where a new stock cycle begins.

AI Agents must not independently reopen, activate, sell, deliver, transfer, return, or retire Inventory.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied policy.
- Record version.
- Inventory and intake snapshot.
- Human Decision or automation-policy reference.
- Evidence.
- Related Command.
- External Confirmation.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.

---

## 7. Relationships

### Tenant

- Every Inventory Record belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant stock transfer requires an approved legal and technical mechanism.

### Vehicle

- Every Inventory Record references one physical Vehicle.
- Vehicle owns identity and specifications.
- Inventory preserves historical Vehicle snapshots.
- Inventory lifecycle must not rewrite Vehicle identity.

### Trade-In

- Trade-In may request Inventory intake after its acquisition conditions pass.
- Trade-In owns appraisal, payoff, equity, acquisition readiness, and legal acquisition.
- Inventory owns intake validation, Inventory Record creation, activation, and stock lifecycle.
- Inventory returns the accepted Inventory Record reference and status to Trade-In.
- Trade-In stores only request, Confirmation, reference, and reconciliation projections.
- A Trade-In appraisal must not automatically become Inventory acquisition cost.

### Dealership, Branch, and Location

- Every active Inventory Record belongs to one responsible dealership.
- Branch responsibility and physical location are separate.
- Changes must preserve history.
- Secure location values require restricted access.

### Opportunity and Appointment

- Opportunity interest does not reserve or allocate a Vehicle.
- Appointment scheduling does not prove Inventory availability.
- A test drive requires current Inventory and readiness checks.

### Quotation and Deal

- Quotation may reference an Inventory Record and approved pricing projection.
- Customer-specific price belongs to Quotation.
- Deal owns the final commercial transaction.
- Deal requests or consumes Reservation, Allocation, sale, and delivery projections.
- Deal must not create an Inventory Record.

### Finance Application and Financial Contract

- Finance approval and contract state do not belong to Inventory.
- Finance and contract projections may contribute to allocation or delivery readiness.
- Restricted finance data must not be copied into Inventory unnecessarily.

### Reservation and Allocation

- Reservation and Allocation history remain separate and auditable.
- Only current active projections appear on the Inventory Record.
- Release, expiration, rejection, and conversion must preserve history.

### External Inventory and Accounting Systems

External systems may own selected operational or financial facts.

Inventory Domain Service must:

- Preserve source authority.
- Use Commands and idempotency.
- Wait for External Confirmation where required.
- Reconcile conflicts.
- Publish accepted canonical projections.

### Supporting Child Records

Inventory Domain Service may own or govern:

- Inventory intake requests.
- Intake validation records.
- Inventory creation Commands.
- Acquisition records.
- Location history.
- Presence verifications.
- Reservation history.
- Allocation history.
- Pricing history.
- Preparation records.
- Publication records.
- Transfer records.
- Floor-plan projections.
- Exit records.
- Data-quality issues.
- Reconciliation cases.
- Derived Intelligence.
- Audit records.

### Supersession and Stock-Cycle Lineage

A replacement or later stock cycle should preserve:

```text
supersedes_inventory_record_id
superseded_by_inventory_record_id
```

Historical Inventory Records must remain immutable and traceable.

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
- Correction and reversal behavior.

The following are required Event concepts and do not replace the Event Catalog.

### Inventory Intake Event Concepts

- Inventory intake requested.
- Inventory intake validation started.
- Inventory intake validated.
- Inventory intake rejected.
- Inventory intake Command created.
- Inventory intake Command transmitted.
- Inventory intake external Confirmation received.
- Inventory intake external Confirmation rejected.
- Inventory intake accepted.
- Inventory intake reconciliation required.
- Inventory Record created.
- Inventory Record activation requested.
- Inventory Record activated.
- Inventory Record activation failed.

Producer boundaries:

- Trade-In Domain Service may publish that it requested Inventory intake.
- Inventory Domain Service publishes intake acceptance, rejection, reconciliation, Inventory Record creation, and activation facts.
- External adapters publish normalized observations and Confirmations.
- Trade-In Domain Service must not publish authoritative Inventory Record-created or activated Events.

### Inventory Lifecycle Event Concepts

- Inventory Record planned.
- Vehicle ordered.
- Vehicle entered transit.
- Vehicle acquired.
- Vehicle received.
- Inspection requested.
- Preparation started.
- Preparation completed.
- Inventory made available.
- Inventory blocked.
- Inventory block cleared.
- Inventory retired.
- Inventory archived.

### Location, Availability, Reservation, and Allocation Event Concepts

- Inventory location updated.
- Physical presence verified.
- Location mismatch detected.
- Availability changed.
- Availability became stale.
- Reservation requested.
- Reservation confirmed.
- Reservation expired.
- Reservation released.
- Allocation requested.
- Allocation confirmed.
- Allocation released.
- Allocation converted to sale.
- Reconciliation required.

### Pricing, Publication, Transfer, and Exit Event Concepts

- Inventory price proposed.
- Inventory price approved.
- Inventory price activated.
- Markdown Recommendation generated.
- Publication requested.
- Publication confirmed.
- Publication failed.
- Transfer requested.
- Transfer departure confirmed.
- Transfer receipt confirmed.
- Sale pending.
- Sale confirmed.
- Delivery pending.
- Delivery confirmed.
- Inventory returned.
- Inventory retired.

### Derived Intelligence Event Concepts

- Demand score updated.
- Stock-pressure score updated.
- Aging classification changed.
- Inventory action Recommendation generated.
- Management review recommended.

Derived Intelligence Events must not imply:

- Intake acceptance.
- Inventory Record creation.
- Activation.
- Pricing approval.
- Reservation.
- Allocation.
- Sale.
- Delivery.
- Transfer completion.

### Event Producer Rules

- Inventory Domain Service publishes accepted Inventory canonical and workflow facts.
- Vehicle Domain Service publishes accepted Vehicle identity facts.
- Trade-In Domain Service publishes accepted Trade-In and intake-request facts it owns.
- Deal Domain Service publishes accepted transaction facts.
- Integration services publish normalized external observations.
- AI Agents may publish Agent-run, analysis, anomaly, prediction, or Recommendation Events.
- AI Agents must not publish authoritative Inventory execution Events merely because they predicted or recommended an outcome.

### Event Requirements

Every material Inventory Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `inventory_record_id`.
- `inventory_intake_request_id`.
- `vehicle_id`.
- Trade-In or source reference.
- Dealership, branch, and legal entity.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Intake or Inventory snapshot hash.
- Correlation identifier.
- Causation identifier.
- Applied policy.
- Human Decision or automation-policy reference.
- Command.
- External Confirmation.
- Evidence references.
- Security classification.

Events are immutable.

Correction, reversal, release, cancellation, rejection, and revocation must use new Events linked to prior Events.

The Event Backbone may deliver an Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Intake-completeness analysis.
- Trade-In-to-Inventory handoff comparison.
- Duplicate stock-cycle risk detection.
- Vehicle and source mismatch detection.
- Location-conflict detection.
- Availability-risk detection.
- Demand and aging analysis.
- Markdown Recommendations.
- Transfer Recommendations.
- Publication drafting.
- Preparation summaries.
- Reservation-expiry risk.
- Allocation-stall detection.
- Reconciliation summarization.
- Management-priority generation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Accept Inventory intake.
- Create or activate an Inventory Record.
- Assign a stock number.
- Confirm acquisition or physical possession.
- Change restricted cost or pricing.
- Approve a discount.
- Confirm commercial availability.
- Reserve or allocate outside approved policy.
- Remove a safety, quality, legal, ownership, financial, or compliance block.
- Confirm sale, delivery, or transfer.
- Represent a Command as completed.
- Access restricted Inventory data outside authorized scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Inventory Record or intake identifier.
- Vehicle identifier.
- Source references.
- Input-record versions.
- Evidence.
- Data freshness.
- Applied Business Rules.
- Model, algorithm, or formula version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Expected impact.
- Action Class.
- Required Human authority.
- Expiration.

### Action Class 2

Controlled low-risk operational actions may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant and organizational scope.
- Resource eligibility.
- Current source versions.
- Inventory freshness.
- Availability.
- Customer permission where relevant.
- Template and channel.
- Frequency.
- Revocation.
- Risk limits.
- Audit requirements.

### Action Class 3

Binding or high-impact Inventory actions require an Authoritative Human Decision or External Authoritative Decision.

Examples include:

- Intake exception approval.
- Inventory activation exception.
- Restricted price approval.
- Allocation override.
- Stock release for sale.
- Transfer approval.
- Legal or safety block removal.
- Delivery authorization.
- Financial or ownership override.

### AI Context and Embeddings

Restricted Inventory information must not enter unrestricted embeddings.

Normally excluded fields include:

- Acquisition cost.
- Landed cost.
- Internal price floor.
- Discount limits.
- Gross profit.
- Floor-plan information.
- Supplier financial terms.
- Trade-In ownership, payoff, and lien evidence.
- Customer-specific Reservation or Allocation data.
- Secure location data.
- Legal and compliance evidence.
- External credentials or Commands.

Approved context may include:

- Customer-visible Vehicle description.
- Approved advertised price.
- Non-sensitive availability summary.
- Preparation summary.
- Public location description.
- Inventory age band.
- Demand category.
- Redacted blocker category.

Every vector record must enforce:

- `tenant_id`.
- Organizational scope.
- Source references.
- Record version.
- Freshness.
- Security classification.
- Retention.
- Expiration.
- Deletion and supersession propagation.

### Untrusted Input and Prompt Injection

Trade-In documents, acquisition evidence, supplier files, inspection reports, DMS notes, and external messages are untrusted input.

Their content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Trigger Commands.
- Create Inventory.
- Activate stock.
- Change price or availability.
- Remove a block.
- Alter audit evidence.

---

## 10. API Contract

Detailed API operations and Schemas will become authoritative in the API Contracts Catalog.

This section defines required Inventory API behavior.

### REST Resources

```text
GET    /api/v1/inventory-records
POST   /api/v1/inventory-records
GET    /api/v1/inventory-records/{inventory_record_id}
PATCH  /api/v1/inventory-records/{inventory_record_id}

GET    /api/v1/inventory-intake-requests
POST   /api/v1/inventory-intake-requests
GET    /api/v1/inventory-intake-requests/{inventory_intake_request_id}
POST   /api/v1/inventory-intake-requests/{inventory_intake_request_id}/validation-requests
POST   /api/v1/inventory-intake-requests/{inventory_intake_request_id}/acceptance-decisions
POST   /api/v1/inventory-intake-requests/{inventory_intake_request_id}/external-confirmations
POST   /api/v1/inventory-intake-requests/{inventory_intake_request_id}/reconciliation-requests

POST   /api/v1/inventory-records/{inventory_record_id}/activation-requests
POST   /api/v1/inventory-records/{inventory_record_id}/receive
POST   /api/v1/inventory-records/{inventory_record_id}/location-verifications
POST   /api/v1/inventory-records/{inventory_record_id}/inspection-requests
POST   /api/v1/inventory-records/{inventory_record_id}/preparation-actions
POST   /api/v1/inventory-records/{inventory_record_id}/make-available
POST   /api/v1/inventory-records/{inventory_record_id}/blocks
POST   /api/v1/inventory-records/{inventory_record_id}/block-release-requests

POST   /api/v1/inventory-records/{inventory_record_id}/reservation-requests
POST   /api/v1/inventory-records/{inventory_record_id}/reservation-release-requests
POST   /api/v1/inventory-records/{inventory_record_id}/allocation-requests
POST   /api/v1/inventory-records/{inventory_record_id}/allocation-release-requests

POST   /api/v1/inventory-records/{inventory_record_id}/price-proposals
POST   /api/v1/inventory-records/{inventory_record_id}/price-approvals
POST   /api/v1/inventory-records/{inventory_record_id}/publication-requests

POST   /api/v1/inventory-records/{inventory_record_id}/transfer-requests
POST   /api/v1/inventory-records/{inventory_record_id}/retirement-requests

GET    /api/v1/inventory-records/{inventory_record_id}/history
GET    /api/v1/inventory-records/{inventory_record_id}/reconciliation
```

Sale and delivery outcomes should normally be ingested from Deal, Delivery, DMS, or another configured authority rather than accepted through unrestricted public confirmation mutations.

### Inventory Intake Mutation Ownership

Only Inventory Domain Service may expose the canonical Inventory-intake mutation.

Trade-In APIs may request the workflow through Inventory Domain Service, but they must not expose an independent mutation that creates or activates an Inventory Record.

Deal, Finance Application, Financial Contract, and AI APIs must not create Inventory Records.

### Example Trade-In Intake Request

```json
{
  "source_type": "TRADE_IN",
  "source_id": "4a97e40a-37bb-45bd-a6fa-31e84639df73",
  "source_record_version": 18,
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "inventory_type": "TRADE_IN_STOCK",
  "ownership_type": "DEALERSHIP_OWNED",
  "acquisition_snapshot_hash": "sha256:62f15a4e...",
  "requested_activation_mode": "AFTER_EXTERNAL_CONFIRMATION",
  "evidence_references": [
    "evidence://trade-ins/4a97e40a/acquisition",
    "evidence://trade-ins/4a97e40a/ownership-transfer",
    "evidence://trade-ins/4a97e40a/physical-possession"
  ]
}
```

The request must use:

```text
Idempotency-Key: 5c972c8e-06e4-4ae1-a2b8-2ac6f3216849
```

### Example Pending Intake Response

```json
{
  "inventory_intake_request_id": "9d557671-a5d1-4b33-975c-c717041c73ae",
  "source_type": "TRADE_IN",
  "source_id": "4a97e40a-37bb-45bd-a6fa-31e84639df73",
  "status": "PENDING_EXTERNAL_CONFIRMATION",
  "inventory_record_id": "30a9bbf9-b02a-4fbf-ad55-cc5ae742514f",
  "inventory_record_status": "PLANNED",
  "availability_status": "NOT_AVAILABLE",
  "command_id": "44d94aa1-c0dd-497f-920e-c9bd93daebc0",
  "record_version": 1
}
```

This response does not prove activation or commercial availability.

### Example Activated Response

```json
{
  "inventory_intake_request_id": "9d557671-a5d1-4b33-975c-c717041c73ae",
  "status": "ACTIVATED",
  "inventory_record_id": "30a9bbf9-b02a-4fbf-ad55-cc5ae742514f",
  "inventory_number": "INV-2026-004521",
  "stock_number": "STK-009174",
  "inventory_record_status": "ACQUIRED",
  "availability_status": "NOT_AVAILABLE",
  "external_stock_reference": "DMS-STOCK-9174",
  "confirmation_reference": "dms://stock-confirmations/9174",
  "record_version": 3,
  "activated_at": "2026-08-02T11:30:00Z"
}
```

Activation still does not prove that the Vehicle is available for sale.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant and organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Source-version validation.
- Concurrency.
- Idempotency where required.
- Duplicate current-stock-cycle checks.
- Required Human Decision or automation policy.
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

Retryable intake, create, activation, Reservation, Allocation, transfer, publication, and external-write operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate business effects.

### Pending External Confirmation

Operations requiring an external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "command_id": "44d94aa1-c0dd-497f-920e-c9bd93daebc0",
  "inventory_record_id": "30a9bbf9-b02a-4fbf-ad55-cc5ae742514f",
  "record_version": 2
}
```

The API must not describe the operation as complete until authoritative evidence is accepted.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `SOURCE_VERSION_MISMATCH`
- `INVENTORY_INTAKE_NOT_ELIGIBLE`
- `TRADE_IN_ACQUISITION_INCOMPLETE`
- `TRADE_IN_OWNERSHIP_EVIDENCE_MISSING`
- `TRADE_IN_PAYOFF_OR_LIEN_UNRESOLVED`
- `PHYSICAL_POSSESSION_NOT_CONFIRMED`
- `VEHICLE_ALREADY_HAS_ACTIVE_INVENTORY`
- `DUPLICATE_INTAKE_REQUEST`
- `EXTERNAL_STOCK_CONFLICT`
- `INVENTORY_ACTIVATION_NOT_PERMITTED`
- `STALE_AVAILABILITY`
- `LOCATION_NOT_VERIFIED`
- `INVENTORY_BLOCKED`
- `RESERVATION_CONFLICT`
- `ALLOCATION_CONFLICT`
- `PRICE_APPROVAL_REQUIRED`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `RECONCILIATION_REQUIRED`
- `RECORD_NOT_CURRENT`
- `RECORD_ARCHIVED`

### GraphQL Requirements

GraphQL implementations must enforce the same:

- Tenant isolation.
- Field authority.
- Source authority.
- Intake ownership.
- Concurrency.
- Idempotency.
- Lifecycle.
- Approval.
- External Confirmation.
- Audit controls.

Resolvers must not bypass Inventory Domain Service, Policy Engine, Command Orchestration, or authoritative source services.

---

## 11. Database Design

### Recommended Tables

```text
inventory_intake_requests
inventory_intake_validations
inventory_intake_commands
inventory_records
inventory_record_lineage
inventory_acquisitions
inventory_locations
inventory_location_history
inventory_presence_verifications
inventory_reservations
inventory_allocations
inventory_pricing
inventory_price_history
inventory_preparation
inventory_publications
inventory_floor_plan_projections
inventory_activity_metrics
inventory_transfers
inventory_exits
inventory_external_confirmations
inventory_reconciliation_cases
inventory_data_quality_issues
inventory_status_history
inventory_record_versions
inventory_audit_log
```

### Inventory Intake Requests

`inventory_intake_requests` should preserve:

- Intake identifier.
- Tenant and organization.
- Source type, source identifier, and source version.
- Vehicle.
- Target dealership, branch, and legal entity.
- Requested Inventory type and ownership type.
- Intake snapshot and hash.
- Status.
- Validation results.
- Human Decision or automation-policy reference.
- Idempotency key.
- Command.
- External Confirmation.
- Resulting Inventory Record.
- Rejection, failure, or cancellation reason.
- Reconciliation state.
- Created and updated times.
- Related Events.

A uniqueness control should prevent duplicate active intake for the same immutable source intent.

### Inventory Records

`inventory_records` should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Vehicle and stock-cycle lineage.
- Current intake and source references.
- Current lifecycle and availability.
- Current location and presence.
- Current Reservation and Allocation projections.
- Current pricing and readiness.
- Current publication state.
- Current sale, delivery, transfer, return, and exit projections.
- Synchronization and reconciliation.
- Record version.
- Audit timestamps.

Historical detail must remain in child or history tables.

### Primary Keys

```text
PRIMARY KEY (inventory_intake_request_id)
```

for `inventory_intake_requests`.

```text
PRIMARY KEY (inventory_record_id)
```

for `inventory_records`.

### Tenant Protection

Every Inventory-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced through:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_inventory_intake_tenant_source
  (tenant_id, inventory_intake_source_type, inventory_intake_source_id)

idx_inventory_intake_tenant_status
  (tenant_id, inventory_intake_status)

idx_inventory_tenant_status
  (tenant_id, status)

idx_inventory_tenant_availability
  (tenant_id, availability_status)

idx_inventory_tenant_vehicle_current
  (tenant_id, vehicle_id, is_current_record)

idx_inventory_tenant_trade_in
  (tenant_id, trade_in_id)

idx_inventory_tenant_dealership_branch
  (tenant_id, dealership_id, branch_id)

idx_inventory_tenant_location
  (tenant_id, location_id, physical_presence_status)

idx_inventory_tenant_stock_number
  (tenant_id, dealership_id, stock_number)

idx_inventory_active_reservation
  (tenant_id, current_reservation_id)

idx_inventory_active_allocation
  (tenant_id, current_allocation_id)

idx_inventory_aging
  (tenant_id, dealership_id, aging_status, days_in_inventory)

idx_inventory_reconciliation
  (tenant_id, reconciliation_status)

idx_inventory_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, inventory_number)
```

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

where the source guarantees uniqueness.

An intake uniqueness constraint or service control should normally enforce:

```text
UNIQUE (
  tenant_id,
  inventory_intake_source_type,
  inventory_intake_source_id,
  inventory_intake_source_record_version,
  dealership_id
)
```

for the same immutable intake intent.

A partial unique constraint or equivalent concurrency-safe service control should prevent multiple current physical-possession records:

```text
UNIQUE (tenant_id, vehicle_id)
WHERE is_current_record = true
  AND status NOT IN (
    'TRANSFERRED_OUT',
    'RETURNED',
    'RETIRED',
    'ARCHIVED'
  )
```

### Reservation, Allocation, and History Storage

Reservation, Allocation, pricing, location, transfer, and exit tables must preserve:

- Stable child identifier.
- Inventory Record.
- Tenant.
- Status.
- Requested and accepted times.
- Authority.
- Human Decision or automation-policy reference.
- Command and idempotency key where applicable.
- External Confirmation.
- Evidence.
- Reconciliation.
- Record version.
- Related Events.

### Derived Intelligence Storage

Derived records must remain separate from authoritative intake, lifecycle, cost, availability, and execution fields.

Each record should preserve:

- Output type.
- Value.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence.
- Confidence.
- Generated time.
- Expiration.
- Review status.

### Audit Storage

Inventory audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw financial, ownership, identity, or location values where full retention is unnecessary.

### Hard Deletion

An Inventory intake or Inventory Record must not be hard-deleted when referenced by:

- Trade-In.
- Vehicle.
- Reservation.
- Allocation.
- Appointment.
- Quotation.
- Finance Application.
- Financial Contract.
- Deal.
- Payment.
- Delivery.
- Transfer.
- Publication.
- External Confirmation.
- Reconciliation.
- Human Decision.
- AI Agent Run.
- Command.
- Audit evidence.

Rejection, cancellation, supersession, retirement, archival, anonymization, governed redaction, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

| Classification | Example Fields |
| :--- | :--- |
| `PUBLIC_VEHICLE_INFORMATION` | Approved make, model, trim, year, public features |
| `OPERATIONAL_INVENTORY` | Lifecycle, availability, branch, preparation, publication |
| `SENSITIVE_ASSET_LOCATION` | Parking slot, secure storage location, movement plan |
| `COMMERCIAL_CONFIDENTIAL` | Cost, internal price floor, discount limit, margin |
| `FINANCIAL_CONFIDENTIAL` | Floor-plan and holding-cost information |
| `TRADE_IN_RESTRICTED` | Ownership, lien, payoff, acquisition evidence |
| `CUSTOMER_RESTRICTED` | Reservation and Allocation Customer references |
| `LEGAL_OR_COMPLIANCE` | Legal holds, safety blocks, disputes |
| `DERIVED_INTELLIGENCE` | Scores, forecasts, Recommendations |
| `AUDIT_EVIDENCE` | Actors, Decisions, Commands, Confirmations, history |

### Authentication and Authorization

Every internal Inventory operation requires an authenticated Human or service identity.

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Location.
- Legal entity.
- Role and team.
- Resource.
- Requested field and action.
- Value threshold.
- Lifecycle state.
- Data classification.
- Delegated authority.
- Business purpose.
- Legal hold.

Approved public feeds may expose only explicitly permitted Customer-visible projections.

### Example Role Boundaries

#### Sales Consultant

May access approved:

- Customer-visible Vehicle specification.
- Availability summary.
- Advertised price.
- Appointment and test-drive readiness.

Must not independently:

- Access restricted acquisition or Trade-In evidence.
- Access internal cost or margin.
- Override Reservation or Allocation.
- Activate Inventory.
- Confirm sale or delivery.

#### Inventory User

May perform permitted:

- Intake preparation.
- Receipt.
- Location updates.
- Inspection coordination.
- Preparation.
- Physical verification.
- Publication.
- Transfer preparation.

#### Inventory Manager

May perform configured:

- Intake acceptance review.
- Activation review.
- Transfer approval.
- Aging review.
- Operational block management.
- Reconciliation.
- Pricing review.

#### Finance, Compliance, Legal, and Accounting Roles

May access only the restricted fields required by assigned responsibilities.

Their access does not automatically grant Inventory activation, availability, sale, delivery, or cross-Tenant authority.

#### AI Agent

May access only minimum approved context.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from accessing restricted cost, payoff, lien, ownership, secure location, or legal data unless specifically approved.

### Field-Level Protection

Restricted fields must use:

- Field-level authorization.
- Encryption.
- Tokenization where applicable.
- Masking.
- Controlled document references.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Restricted values must not appear in:

- Public APIs.
- Public feeds.
- Customer communications.
- Unrestricted Logs.
- General-purpose embeddings.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Intake and duplicate detection.
- APIs.
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

Outbound Inventory Commands must include:

- Authenticated service identity.
- Tenant and organizational scope.
- Inventory intake or Inventory Record identifier.
- Current source and record versions.
- Requested action.
- Field-level authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Inventory activity must record:

- `tenant_id`.
- `inventory_record_id`.
- `inventory_intake_request_id`.
- `vehicle_id`.
- Source workflow and source version.
- Trade-In reference where applicable.
- Dealership, branch, location, and legal entity.
- Actor.
- Role and permission.
- Action and business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Record version.
- Source and authority category.
- Applied Business Rules.
- Human Decision or automation-policy reference.
- AI involvement.
- Command and idempotency key.
- External Confirmation.
- Evidence.
- Timestamp.
- Correlation and causation identifiers.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Inventory access.
- Unauthorized intake acceptance.
- Unauthorized Inventory Record creation or activation.
- Duplicate stock-cycle attempts.
- Trade-In evidence substitution.
- Vehicle substitution.
- Stock-number manipulation.
- Unauthorized Reservation or Allocation.
- Restricted pricing or cost access.
- Availability manipulation.
- Location tampering.
- Unauthorized block removal.
- False sale or delivery Confirmation.
- Command replay.
- External Confirmation mismatch.
- Suspicious transfer.
- AI access outside approved scope.
- Prompt injection inside evidence or documents.
- Audit-log tampering.

### Transaction Integrity

The platform must detect or prevent:

- Multiple active current records for one Vehicle.
- Inventory creation from the wrong Trade-In or source version.
- Activation without accepted intake.
- Activation without required external Confirmation.
- Trade-In appraisal treated as authoritative Inventory cost.
- Availability before required acquisition and readiness evidence.
- Reservation and Allocation conflict.
- Sale or delivery inferred from a sent Command.
- Transfer receipt before evidence.
- Re-entry without a new governed stock cycle.
- Lifecycle or authority manipulation.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Inventory intake.
- Inventory creation.
- Inventory activation.
- Reservation.
- Allocation.
- Publication.
- Pricing updates.
- Transfer.
- Sale release.
- Delivery release.
- External Inventory write-back.
- Automated Customer communication.
- AI Inventory analysis.
- Export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Deal](./Deal.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Inventory Record baseline.

Inventory Domain Service is the sole canonical owner of Inventory-intake acceptance, Inventory Record creation, Inventory activation, stock identity, lifecycle, and commercial availability.

Trade-In Domain Service owns appraisal, payoff, equity, acquisition readiness, legal acquisition, and the request for Inventory intake.

Trade-In, Deal, Finance Application, Financial Contract, and AI services must not create or activate an Inventory Record directly.

A Trade-In intake request does not prove intake acceptance.

Intake acceptance does not prove Inventory Record creation.

Inventory Record creation does not prove activation.

Activation does not prove physical receipt or commercial availability.

External stock posting remains pending until accepted External Confirmation where the configured DMS or Inventory system is authoritative.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
