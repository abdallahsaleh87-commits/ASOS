# Inventory Record

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Inventory Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Inventory Record Object represents one governed dealership Inventory holding or stock cycle for one specific physical Vehicle inside an ASOS Tenant.

It describes how a Vehicle is:

- Acquired or expected.
- Held by a dealership.
- Located.
- Inspected and prepared.
- Priced.
- Published.
- Made commercially available.
- Reserved.
- Allocated.
- Transferred.
- Sold.
- Delivered.
- Returned.
- Retired from Inventory.

The Inventory Record answers operational questions such as:

- Is the Vehicle physically present?
- Which dealership and branch currently control it?
- Is it commercially available?
- Is it ready for sale or test drive?
- Is it reserved or allocated?
- What pricing authority applies?
- Is a transfer in progress?
- Is a sale or delivery externally confirmed?
- Is the Inventory information current and verified?
- Is a legal, safety, financial, quality, or compliance block active?

### Vehicle and Inventory Separation

`Vehicle` defines the stable identity and specifications of the Vehicle.

`Inventory Record` defines the commercial and operational dealership context of that Vehicle.

The Vehicle Object owns:

- VIN and chassis identity.
- Make.
- Model.
- Trim.
- Variant.
- Model year.
- Technical specifications.
- Factory configuration.
- Identity-verification evidence.
- Odometer evidence.

The Inventory Record owns or projects:

- Inventory number.
- Stock number.
- Dealership and branch context.
- Location.
- Physical-presence state.
- Acquisition context.
- Ownership or holding arrangement.
- Inventory lifecycle.
- Commercial availability.
- Pricing context.
- Preparation state.
- Publication state.
- Reservation state.
- Allocation state.
- Inventory aging.
- Transfer state.
- Exit state.

The Inventory Record must not redefine authoritative Vehicle identity or technical specifications.

### Historical Stock Cycles

A physical Vehicle may have multiple historical Inventory Records because of:

- Stock transfer.
- Return and reacquisition.
- Trade-In acquisition.
- Repurchase.
- Consignment renewal.
- Ownership change.
- Dealer-group transfer.
- Data correction.
- Separate Inventory holding periods.

Only one Inventory Record may claim the current physical possession and active commercial availability of the same physical Vehicle inside the same Tenant.

A transfer workflow may temporarily create a planned destination record, but only one record may claim authoritative current physical possession at a time.

### System Purpose

The Inventory Record provides canonical Inventory context to:

- Vehicle search and matching.
- Lead and Opportunity workflows.
- Appointment and test-drive scheduling.
- Quotation preparation.
- Reservation workflows.
- Allocation workflows.
- Deal progression.
- Trade-In acquisition.
- Finance coordination.
- Publication channels.
- Inventory aging analysis.
- Pricing Recommendations.
- Transfer workflows.
- Management reporting.
- AI Agents.
- Audit and reconciliation services.

The Inventory Record may contain:

- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

The configured DMS, Inventory Management System, accounting platform, or another approved external system may remain authoritative for:

- Physical stock status.
- Legal ownership.
- Acquisition cost.
- Final posted pricing.
- Reservation completion.
- Allocation completion.
- Sale posting.
- Delivery posting.
- Transfer completion.
- Accounting entries.

ASOS must not represent an internal projection, Recommendation, Human Decision, or sent Command as an externally completed Inventory action without authoritative External Confirmation.

### Canonical Ownership Matrix

| Information | Canonical Owner |
| :--- | :--- |
| Vehicle identity and technical specifications | Vehicle |
| Dealership stock context | Inventory Record |
| Current Inventory location | Inventory Record projection or configured external authority |
| Commercial availability | Inventory Record |
| Reservation workflow | Inventory Record or dedicated Reservation service |
| Allocation workflow | Inventory Record or dedicated Allocation service |
| Inventory pricing context | Inventory Record |
| Customer-specific commercial offer | Quotation |
| Final commercial Deal terms | Deal |
| Customer Payment evidence | Payment or configured external authority |
| Finance approval | Finance Application or lender |
| Signed finance agreement | Financial Contract |
| Vehicle delivery Confirmation | Delivery workflow and configured external authority |
| Inventory demand and aging intelligence | Derived Intelligence associated with Inventory Record |

---

## 2. Canonical Schema

### Primary Identifiers

- `inventory_record_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `vehicle_id` — UUIDv4, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `location_id`.
- `responsible_team_id`.
- `assigned_inventory_user_id`.

`dealership_id` is required because an Inventory Record represents dealership stock context.

`branch_id` and `location_id` may remain null while the Vehicle is planned, ordered, or in transit.

They become required before confirmed physical receipt or commercial availability.

### Inventory Identity

- `inventory_number`.
- `stock_number`.
- `inventory_type`.
- `ownership_type`.
- `vehicle_condition`.
- `status`.
- `availability_status`.
- `physical_presence_status`.
- `data_quality_status`.
- `conflict_status`.
- `is_current_record`.
- `supersedes_inventory_record_id`.

### Vehicle Reference Projection

- `vehicle_snapshot`.
- `vin_projection`.
- `make_projection`.
- `model_projection`.
- `trim_projection`.
- `model_year_projection`.
- `odometer_projection`.

Vehicle projection fields are convenience snapshots.

The authoritative identity remains with the Vehicle Object and its configured sources.

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
- `acquisition_evidence_references`.

### Location Context

- `branch_id`.
- `location_id`.
- `location_type`.
- `parking_slot`.
- `display_zone`.
- `storage_zone`.
- `physical_presence_status`.
- `last_location_verified_at`.
- `last_location_verified_by`.
- `current_transfer_id`.

### Availability and Blocking

- `availability_status`.
- `available_from`.
- `available_until`.
- `availability_block_reason`.
- `availability_blocked_at`.
- `availability_blocked_by`.
- `availability_block_evidence`.
- `availability_freshness_status`.
- `last_availability_confirmed_at`.

### Reservation Projection

- `current_reservation_id`.
- `reservation_status`.
- `reserved_for_customer_id`.
- `reserved_for_opportunity_id`.
- `reserved_at`.
- `reservation_expires_at`.
- `reservation_authority_reference`.
- `reservation_confirmation_status`.

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
- `quality_hold_reason`.
- `sale_readiness_status`.
- `test_drive_readiness_status`.
- `delivery_readiness_status`.

Readiness status is not proof of sale or delivery.

### Publication

- `publication_status`.
- `published_channel_ids`.
- `primary_sales_channel`.
- `published_at`.
- `last_publication_sync_at`.
- `publication_error_code`.
- `publication_confirmation_status`.

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

These fields are restricted projections.

The external financial or accounting platform may remain authoritative.

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

Derived Intelligence must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Freshness.
- Confidence where meaningful.
- Generation timestamp.
- Expiration timestamp.
- Required approval.

### Sale, Delivery, Transfer, and Exit Projection

- `sold_deal_id`.
- `sale_confirmation_status`.
- `sold_at`.
- `sold_price_amount`.
- `delivery_confirmation_status`.
- `delivered_at`.
- `transfer_confirmation_status`.
- `transfer_out_at`.
- `returned_at`.
- `retired_at`.
- `archived_at`.
- `exit_reason`.
- `exit_reference`.
- `external_confirmation_reference`.

Sale price is a projection from the approved Deal or configured external authority.

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

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inventory_record_id` | UUID | Yes | ASOS | Immutable Canonical Inventory Record identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation boundary. |
| `vehicle_id` | UUID | Yes | Canonical relationship | Physical Vehicle represented by the Inventory Record. |
| `dealership_id` | UUID | Yes | Canonical Projection | Dealership holding or controlling the Inventory context. |
| `branch_id` | UUID | Conditional | Canonical Projection | Responsible branch; required before confirmed receipt or availability. |
| `location_id` | UUID | Conditional | Canonical Projection | Current controlled location; required when physical presence is confirmed. |
| `inventory_number` | String | Yes | ASOS or external source | Stable Inventory Record reference. |
| `stock_number` | String | Conditional | External source or ASOS | Operational dealership stock number. |
| `inventory_type` | Enum | Yes | Canonical Projection | Commercial or operational Inventory type. |
| `ownership_type` | Enum | Yes | External evidence | Legal or financial holding arrangement. |
| `vehicle_condition` | Enum | Yes | Approved source | New, used, demonstrator, or another approved condition. |
| `status` | Enum | Yes | Canonical lifecycle | Current Inventory lifecycle state. |
| `availability_status` | Enum | Yes | Canonical Projection | Current permitted commercial availability. |
| `physical_presence_status` | Enum | Yes | Verification workflow | Current physical-presence state. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine status. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Material data-conflict status. |
| `is_current_record` | Boolean | Yes | ASOS | Identifies the current stock-cycle record. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Acquisition Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `acquisition_source` | Enum | Yes | External evidence | Source through which the Vehicle entered Inventory. |
| `acquisition_status` | Enum | Yes | Canonical Projection | Current acquisition progress. |
| `acquisition_date` | Date | Conditional | External evidence | Date responsibility or ownership was accepted. |
| `acquisition_reference` | String | No | External source | External order, transfer, invoice, or acquisition reference. |
| `purchase_price_amount` | Decimal | Conditional | Restricted external authority | Direct approved acquisition cost. |
| `landed_cost_amount` | Decimal | Conditional | Deterministic calculation | Approved total acquisition cost. |
| `acquisition_currency_code` | String | Conditional | External authority | ISO 4217 currency code. |
| `supplier_id` | UUID | No | Canonical relationship | Supplier or source party. |
| `trade_in_id` | UUID | No | Canonical relationship | Trade-In workflow that created the stock unit. |
| `acquisition_evidence_references` | Array | Conditional | Evidence repository | Supporting acquisition, ownership, or consignment evidence. |

### Location and Presence Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `location_type` | Enum | Conditional | Approved source | Type of physical or controlled location. |
| `parking_slot` | String | No | Operational source | Physical parking or storage reference. |
| `display_zone` | String | No | Operational source | Showroom or display-zone reference. |
| `physical_presence_status` | Enum | Yes | Verification workflow | Whether the Vehicle is verified present, absent, or in transit. |
| `last_location_verified_at` | Timestamp | No | Verification evidence | Time of the latest accepted location verification. |
| `last_location_verified_by` | UUID | No | Human or service identity | Actor responsible for the verification. |
| `current_transfer_id` | UUID | No | Transfer workflow | Current transfer workflow identifier. |

### Availability Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `availability_status` | Enum | Yes | Canonical Projection | Permitted commercial availability state. |
| `available_from` | Timestamp | No | Canonical Projection | Earliest approved commercial availability time. |
| `available_until` | Timestamp | No | Canonical Projection | Optional availability end time. |
| `availability_block_reason` | Enum | No | Human Decision or policy | Reason commercial use is blocked. |
| `last_availability_confirmed_at` | Timestamp | No | External Confirmation | Latest authoritative availability verification. |
| `availability_freshness_status` | Enum | Yes | Deterministic calculation | Whether availability remains within its Freshness SLA. |

### Reservation and Allocation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `current_reservation_id` | UUID | No | Reservation workflow | Current active Reservation. |
| `reservation_status` | Enum | Yes | Workflow projection | Current Reservation state. |
| `reserved_for_customer_id` | UUID | No | Workflow projection | Customer associated with the Reservation. |
| `reserved_for_opportunity_id` | UUID | No | Workflow projection | Opportunity associated with the Reservation. |
| `reservation_expires_at` | Timestamp | No | Approved policy | Expiration time for a time-limited Reservation. |
| `reservation_confirmation_status` | Enum | Yes | Workflow projection | External Confirmation status of the Reservation. |
| `current_allocation_id` | UUID | No | Allocation workflow | Current active Allocation. |
| `allocation_status` | Enum | Yes | Workflow projection | Current Allocation state. |
| `allocated_deal_id` | UUID | No | Workflow projection | Deal receiving the Allocation. |
| `allocation_expires_at` | Timestamp | No | Approved policy | Allocation expiration time where applicable. |
| `allocation_confirmation_status` | Enum | Yes | Workflow projection | External Confirmation status of the Allocation. |

### Pricing Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `currency_code` | String | Conditional | Pricing authority | ISO 4217 currency code. |
| `list_price_amount` | Decimal | Conditional | Approved pricing authority | Standard dealership list price. |
| `advertised_price_amount` | Decimal | Conditional | Approved pricing authority | Current permitted advertised price. |
| `target_sale_price_amount` | Decimal | No | Approved pricing authority | Internal commercial target. |
| `minimum_authorized_price_amount` | Decimal | No | Restricted pricing authority | Lowest price permitted without additional approval. |
| `maximum_discount_amount` | Decimal | No | Restricted pricing authority | Maximum discount within delegated authority. |
| `current_discount_amount` | Decimal | No | Deterministic calculation | Current approved or advertised discount. |
| `market_reference_price_amount` | Decimal | No | Market Intelligence | Evidence-backed market reference. |
| `estimated_gross_profit_amount` | Decimal | No | Derived Intelligence | Estimated profit using approved inputs. |
| `pricing_status` | Enum | Yes | Pricing workflow | Current pricing validity and approval state. |
| `price_effective_from` | Timestamp | No | Pricing authority | Start of price validity. |
| `price_effective_until` | Timestamp | No | Pricing authority | End of price validity. |
| `price_authority_reference` | String | No | Evidence | Rule, Decision, or external-source reference supporting the price. |

### Readiness Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inspection_status` | Enum | Yes | Inspection workflow | Current inspection status. |
| `reconditioning_status` | Enum | Yes | Preparation workflow | Current reconditioning status. |
| `documentation_status` | Enum | Yes | Document workflow | Required-document readiness. |
| `quality_hold_status` | Enum | Yes | Policy or Human Decision | Whether a quality hold exists. |
| `sale_readiness_status` | Enum | Yes | Deterministic projection | Whether sale-readiness requirements are satisfied. |
| `test_drive_readiness_status` | Enum | Yes | Deterministic projection | Whether test-drive requirements are satisfied. |
| `delivery_readiness_status` | Enum | Yes | Deterministic projection | Whether delivery-readiness requirements are satisfied. |

### Exit Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `sold_deal_id` | UUID | No | Deal relationship | Deal through which the Vehicle was sold. |
| `sale_confirmation_status` | Enum | Yes | External Confirmation projection | Whether the sale is authoritatively confirmed. |
| `sold_at` | Timestamp | No | External authority | Authoritative sale time. |
| `sold_price_amount` | Decimal | No | Deal or external authority | Final sale-price projection. |
| `delivery_confirmation_status` | Enum | Yes | External Confirmation projection | Current delivery Confirmation status. |
| `delivered_at` | Timestamp | No | External authority | Authoritative delivery time. |
| `exit_reason` | Enum | No | Approved workflow | Reason the Inventory Record left active stock. |
| `exit_reference` | String | No | External or internal evidence | Supporting sale, transfer, return, or retirement reference. |

### Derived Intelligence Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `days_in_inventory` | Integer | Yes | Deterministic calculation | Calendar days in the current stock cycle. |
| `aging_status` | Enum | Yes | Deterministic policy | Configured Inventory-aging classification. |
| `demand_score` | Decimal | No | Derived Intelligence | Normalized observed or predicted demand. |
| `stock_pressure_score` | Decimal | No | Derived Intelligence | Normalized urgency for review or commercial action. |
| `recommended_markdown_amount` | Decimal | No | Derived Intelligence | Suggested markdown requiring applicable approval. |
| `recommended_action` | String | No | Derived Intelligence | Suggested Inventory action. |
| `requires_management_review` | Boolean | Yes | Deterministic or derived | Indicates whether configured review criteria are met. |

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
- `CUSTOMER_OWNED`
- `LEASED`
- `DEMONSTRATION_LOAN`
- `OTHER`

### InventoryCondition

- `NEW`
- `USED`
- `CERTIFIED_PRE_OWNED`
- `DEMONSTRATOR`
- `DAMAGED`
- `RECONDITIONED`
- `SALVAGE`
- `UNKNOWN`

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

### InventoryAcquisitionSource

- `OEM_ALLOCATION`
- `OEM_ORDER`
- `DEALER_TRANSFER`
- `SUPPLIER_PURCHASE`
- `AUCTION`
- `TRADE_IN`
- `CUSTOMER_BUYBACK`
- `FLEET_RETURN`
- `LEASE_RETURN`
- `CONSIGNMENT`
- `IMPORT`
- `MANUAL_ENTRY`
- `OTHER`

### InventoryAcquisitionStatus

- `NOT_STARTED`
- `PENDING`
- `ORDERED`
- `IN_TRANSIT`
- `RECEIVED`
- `CONFIRMED`
- `REJECTED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### InventoryLocationType

- `SHOWROOM`
- `WAREHOUSE`
- `PARKING_YARD`
- `TRANSIT_HUB`
- `SERVICE_CENTER`
- `BODY_SHOP`
- `DETAILING_CENTER`
- `TEST_DRIVE_ZONE`
- `CUSTOMER_DELIVERY_AREA`
- `EXTERNAL_STORAGE`
- `SUPPLIER_LOCATION`
- `IN_TRANSIT`
- `OTHER`

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

### InventoryReadinessStatus

- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REQUIRES_REVIEW`

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

### InventoryAgingStatus

- `FRESH`
- `NORMAL`
- `MATURING`
- `AGED`
- `CRITICAL`
- `EXEMPT`

The thresholds for these values must remain dealership-configurable.

### FloorPlanStatus

- `NOT_APPLICABLE`
- `PENDING`
- `ACTIVE`
- `MATURING`
- `MATURED`
- `PAYMENT_DUE`
- `SETTLED`
- `DEFAULTED`
- `DISPUTED`
- `CANCELLED`

### InventoryBlockReason

- `QUALITY_HOLD`
- `SAFETY_RECALL`
- `COMPLIANCE_HOLD`
- `DOCUMENTS_INCOMPLETE`
- `LOCATION_UNVERIFIED`
- `DAMAGE_DETECTED`
- `PRICE_REVIEW_REQUIRED`
- `OWNERSHIP_DISPUTE`
- `PAYMENT_OR_LIEN_ISSUE`
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
- A client must not override `tenant_id` through a request body.
- `dealership_id` must belong to the authenticated Tenant.
- `branch_id` must belong to the selected dealership and Tenant.
- `location_id` must belong to the authorized organizational scope.
- All child records must use the same `tenant_id` as the parent Inventory Record.
- Cross-Tenant Inventory access, matching, reservation, allocation, or transfer is prohibited unless governed by an approved and auditable mechanism.

### Vehicle Relationship Rules

- Every Inventory Record must reference one physical Vehicle.
- A catalog-only Vehicle configuration must not be used as a physical Inventory Record.
- The referenced Vehicle must belong to the permitted Tenant scope.
- Vehicle identity fields inside Inventory Record are projections only.
- A Vehicle identity conflict may block commercial use.
- A Vehicle merge must trigger Inventory relationship reconciliation.

### Current Record Rules

- Only one Inventory Record may claim current physical possession of the same Vehicle inside the Tenant.
- Only one current Inventory Record may claim active commercial availability for the same Vehicle.
- `is_current_record = true` must be protected by concurrency and uniqueness controls.
- A superseded, transferred-out, returned, retired, or archived record must not remain commercially available.
- A destination transfer record may be planned before transfer completion but must not claim verified physical possession.

### Acquisition Rules

- Acquisition source is required.
- Acquisition evidence is required before ownership-dependent commercial use.
- Trade-In stock must not become available before required:
  - Ownership evidence.
  - Lien or payoff evidence.
  - Inspection.
  - Acquisition approval.
- Cost fields must be zero or greater.
- `landed_cost_amount` must use an approved deterministic formula.
- AI must not create authoritative cost values.
- Restricted financial values must not be exposed to unauthorized Users.

### Location and Presence Rules

- A Vehicle must not be marked physically present without accepted evidence.
- `branch_id` and `location_id` are required before confirmed receipt.
- Physical location conflicts must block dependent high-risk activity where required.
- A Vehicle in transit must not be represented as physically present at the destination.
- Customer-facing availability must not rely on stale or unverified location data.
- Location changes requiring external write-back remain pending until authoritative Confirmation.

### Availability Rules

A Vehicle may become `AVAILABLE_FOR_SALE` only when:

- Physical presence is verified or an approved future-stock policy applies.
- Required ownership or holding evidence exists.
- Required inspection and preparation checks pass.
- Valid approved pricing exists.
- Availability data is within its Freshness SLA.
- No active safety, legal, compliance, ownership, payment, quality, fraud, or data-conflict block exists.
- No incompatible active Reservation or Allocation exists.

A Vehicle may become `AVAILABLE_FOR_TEST_DRIVE` only when:

- Physical presence is verified.
- Test-drive readiness is `READY`.
- Applicable insurance, registration, safety, and operational requirements pass.
- No incompatible active Reservation, Allocation, or block exists.

### Reservation Rules

- Reservation creation must use an atomic concurrency-safe operation.
- A Vehicle must not have more than one incompatible active Reservation.
- A Reservation must identify its Customer or permitted business purpose.
- A time-limited Reservation must include an expiration time.
- Expired Reservations must not continue blocking stock.
- Reservation approval does not prove that the external Inventory system accepted the Reservation.
- When the external system is authoritative, Reservation remains `PENDING_CONFIRMATION` until Confirmation arrives.
- Reservation release must also use a controlled and idempotent operation.
- AI Agents must not independently reserve stock outside an applicable approved automation policy.
- Action Class 2 Reservation activity requires:
  - Explicit Human Approval; or
  - An applicable pre-approved automation policy.

### Allocation Rules

- Allocation must use an atomic concurrency-safe operation.
- A Vehicle must not be allocated to more than one active Deal.
- Allocation must reference an authorized Opportunity or Deal.
- Allocation does not prove:
  - Payment.
  - Finance approval.
  - Contract completion.
  - Sale.
  - Delivery.
- When an external system is authoritative, Allocation remains pending until External Confirmation.
- Releasing an Allocation must preserve history and reason.
- Action Class 3 Allocation or override decisions require the configured Authoritative Human Decision.

### Pricing Rules

- `currency_code` is required when a price is present.
- Price and cost amounts must be zero or greater.
- `minimum_authorized_price_amount` must not exceed the approved list-price boundary without explicit policy.
- Advertised price must comply with pricing, advertising, tax, and consumer-protection rules.
- Final Customer-specific price belongs to Quotation or Deal.
- Advertised price must not be represented as a final accepted Deal price.
- Restricted price floors, margins, costs, and discount limits must remain access-controlled.
- AI pricing outputs are Recommendations until approved.
- A high-confidence Recommendation does not create pricing authority.
- Price changes must record:
  - Previous value.
  - New value.
  - Authority.
  - Reason.
  - Effective period.
  - Approval.
  - Evidence.
- Restricted pricing changes are Action Class 3 decisions.

### Preparation and Readiness Rules

- Readiness statuses must be derived from approved deterministic requirements.
- A single AI output must not set sale, test-drive, or delivery readiness.
- Quality, safety, compliance, and legal blocks must override commercial readiness.
- Delivery readiness does not confirm delivery.
- Sale readiness does not confirm sale.
- Readiness must expire when dependent evidence becomes stale.

### Publication Rules

- Only approved Customer-visible fields may be published.
- Publication must not expose:
  - Acquisition cost.
  - Landed cost.
  - Internal margin.
  - Minimum authorized price.
  - Floor-plan details.
  - Customer-specific Reservation or Allocation information.
- Published availability and price must match sufficiently current approved sources.
- Publication success must remain pending until provider Confirmation where required.
- Failed or stale publication must create reconciliation or review.

### Sale and Delivery Rules

- `SOLD` requires an approved Deal and authoritative sale Confirmation where applicable.
- A sent Deal-posting Command does not make the Inventory Record `SOLD`.
- `DELIVERED` requires authoritative delivery Confirmation.
- Payment, contract, registration, preparation, and release conditions must be validated before delivery authorization.
- A sold Vehicle must not return to `AVAILABLE` through an ordinary state update.
- Sale cancellation, return, repurchase, or reacquisition requires a controlled workflow.
- A delivered Vehicle requires a new Inventory Record before re-entering active stock.

### Transfer Rules

- Transfer must identify:
  - Source location.
  - Destination location.
  - Responsible authority.
  - Vehicle.
  - Transfer evidence.
  - Expected timing.
- Transfer initiation does not prove destination receipt.
- Source physical possession remains authoritative until accepted transfer evidence changes it.
- Destination receipt requires External Confirmation or accepted physical-verification evidence.
- Transfer retries must not create duplicate destination Inventory Records.

### Conflict and Authority Rules

- Lower-authority values must not silently overwrite higher-authority values.
- Material conflicts must create a controlled review or reconciliation workflow.
- Inventory conflicts involving availability, ownership, location, price, Reservation, Allocation, sale, or delivery may block commercial action.
- Human Approval does not prove external execution.
- External Confirmation must remain distinct from Human Decision.
- Stale authoritative data must not support high-risk Customer-facing claims.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Reservation and Allocation must use atomic locking, compare-and-swap, or an equivalent controlled mechanism.
- Retryable Commands must use an approved `idempotency_key`.
- Duplicate Command retries must not create duplicate:
  - Reservations.
  - Allocations.
  - Transfers.
  - Publications.
  - External updates.
- Event Consumers must prevent duplicate effects using `event_id`.

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

### Principal Allowed Transitions

```text
PLANNED → ORDERED
PLANNED → RETIRED

ORDERED → IN_TRANSIT
ORDERED → ACQUIRED
ORDERED → RETURN_PENDING
ORDERED → RETIRED

IN_TRANSIT → RECEIVED
IN_TRANSIT → BLOCKED
IN_TRANSIT → RETURN_PENDING

ACQUIRED → RECEIVED
ACQUIRED → INSPECTION_PENDING
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

Returning from `BLOCKED` requires evidence that the blocking reason was formally cleared.

### Forbidden Ordinary Transitions

```text
PLANNED → AVAILABLE
ORDERED → SOLD
IN_TRANSIT → AVAILABLE without receipt or approved future-stock policy
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

A new Inventory Record is normally required when a delivered, transferred-out, returned, retired, or archived Vehicle re-enters Inventory.

### Entering AVAILABLE

Requires:

- Valid current Inventory Record.
- Approved acquisition or holding evidence.
- Required physical-presence or future-stock evidence.
- Completed required preparation.
- Approved current pricing.
- Current availability evidence.
- No incompatible Reservation or Allocation.
- No active blocking condition.

### Entering RESERVED

Requires:

- Current commercial availability.
- Atomic Reservation creation.
- Valid Reservation purpose.
- Required approval or automation policy.
- Expiration where required.
- External Confirmation where the external system is authoritative.

### Entering ALLOCATED

Requires:

- Current Inventory eligibility.
- Atomic Allocation creation.
- Valid Opportunity or Deal.
- Required Human authority.
- External Confirmation where the external system is authoritative.

### Entering SALE_PENDING

Requires:

- Valid Deal workflow.
- Vehicle and Inventory relationship validation.
- Required Quotation and approval evidence.
- No incompatible Reservation or Allocation.
- Required pricing authorization.

### Entering SOLD

Requires:

- Authoritative approved Deal.
- Required Human Decision.
- External sale Confirmation where applicable.
- Recorded final sale reference.
- Reconciliation of Reservation and Allocation.

### Entering DELIVERY_PENDING

Requires:

- Confirmed sale.
- Delivery workflow created.
- Required Payment, contract, registration, preparation, and release checks.

### Entering DELIVERED

Requires:

- Authoritative delivery evidence.
- Authorized release.
- Delivery timestamp.
- External Confirmation where applicable.
- Inventory exit reconciliation.

### Entering TRANSFERRED_OUT

Requires:

- Approved transfer.
- Confirmed departure.
- Destination or receiving authority evidence where applicable.
- Removal of active commercial availability.

### Terminal States

- `DELIVERED`
- `TRANSFERRED_OUT`
- `RETURNED`
- `ARCHIVED`

`RETIRED` is inactive and normally progresses to `ARCHIVED`.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Human Decision or automation-policy reference.
- Evidence.
- Related Command.
- External Confirmation.
- Record version.
- Timestamp.
- Correlation identifier.
- Related Event.

---

## 7. Relationships

### Tenant

- Every Inventory Record belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant transfer requires an approved and auditable transfer or data-sharing mechanism.

### Dealership and Branch

- Every Inventory Record belongs to one responsible dealership.
- A branch may become responsible for the Inventory Record.
- Branch changes must preserve history.
- Physical location and organizational responsibility are separate concepts.

### Vehicle

- Every Inventory Record references exactly one physical Vehicle.
- Vehicle identity and specification fields remain authoritative in Vehicle.
- Inventory Record may preserve historical Vehicle snapshots for audit.
- Inventory status must not update Vehicle identity status to `SOLD` or `DELIVERED`.

### Customer

- A Customer may be associated through Reservation, Allocation, Quotation, Opportunity, or Deal.
- Customer identity does not belong directly to the Inventory Record outside governed relationship references.
- Restricted Customer information must not enter public Inventory projections.

### Lead and Qualified Lead

- Leads may indicate interest in the Inventory Record.
- Lead interest does not reserve or allocate the Vehicle.
- Aggregate Lead counts may be derived.

### Opportunity

- An Opportunity may reference one or more matching Inventory Records.
- Opportunity priority does not create Inventory authority.
- Allocation requires a separate controlled workflow.

### Appointment

- Appointments and test drives may reference an Inventory Record.
- Appointment scheduling does not prove availability unless the required Inventory checks pass.
- External Appointment Confirmation does not reserve the Vehicle unless a Reservation workflow exists.

### Quotation

- A Quotation may reference an Inventory Record.
- Customer-specific pricing belongs to Quotation.
- Quotation approval does not automatically allocate or sell the Vehicle.

### Reservation

- Reservation history must remain separate and auditable.
- Only the current active Reservation may be projected into the Inventory Record.
- Reservation expiration or release must not erase history.

### Allocation

- Allocation history must remain separate and auditable.
- Only the current active Allocation may be projected into the Inventory Record.
- Allocation must reference an authorized Opportunity or Deal.

### Trade-In

- A Trade-In workflow may create an Inventory Record after approved acquisition conditions pass.
- Trade-In appraisal values do not automatically become Inventory acquisition costs.
- Vehicle ownership and lien evidence must remain traceable.

### Deal

- A Deal may reference one Inventory Record and Vehicle.
- Final sale terms belong to Deal.
- Sale Confirmation may update the Inventory Record projection.
- A cancelled Deal must use a controlled Reservation, Allocation, and Inventory release workflow.

### Financial Contract and Finance Application

- Finance eligibility and lender Decisions do not belong to Inventory Record.
- Finance or contract status may contribute to delivery readiness.
- Restricted finance information must not be copied unnecessarily into Inventory.

### Market Intelligence

- Market Intelligence may support:
  - Market-reference pricing.
  - Demand scoring.
  - Aging analysis.
  - Markdown Recommendations.
  - Transfer Recommendations.
- Market evidence must not silently change authoritative pricing or availability.

### Interaction

- Customer Interactions may contribute to demand metrics.
- Interaction content is not authoritative Reservation or Allocation evidence.

### Supporting Child Records

Inventory Record may own or govern:

- Acquisition records.
- Location history.
- Physical-verification records.
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
- Derived metrics.
- Audit records.

### Supersession

A replacement Inventory Record should reference:

```text
supersedes_inventory_record_id
```

Historical records must remain traceable and must not be silently overwritten.

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

The following are required Inventory Event concepts and do not replace the Event Catalog.

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

### Location and Presence Event Concepts

- Inventory location updated.
- Physical presence verified.
- Physical absence verified.
- Location mismatch detected.
- Location conflict resolved.

### Availability Event Concepts

- Availability changed.
- Availability became stale.
- Availability Confirmation received.
- Availability conflict detected.
- Availability blocked.

### Reservation Event Concepts

- Reservation requested.
- Reservation approved.
- Reservation Command sent.
- Reservation Confirmation received.
- Reservation rejected.
- Reservation expired.
- Reservation release requested.
- Reservation released.
- Reservation reconciliation required.

### Allocation Event Concepts

- Allocation requested.
- Allocation approved.
- Allocation Command sent.
- Allocation Confirmation received.
- Allocation rejected.
- Allocation released.
- Allocation converted to sale.
- Allocation reconciliation required.

### Pricing Event Concepts

- Inventory price proposed.
- Inventory price approved.
- Inventory price activated.
- Inventory price expired.
- Inventory pricing conflict detected.
- Markdown Recommendation generated.

### Preparation and Publication Event Concepts

- Inspection completed.
- Quality hold applied.
- Quality hold cleared.
- Sale readiness changed.
- Test-drive readiness changed.
- Delivery readiness changed.
- Publication requested.
- Publication confirmed.
- Publication failed.
- Publication reconciliation required.

### Transfer and Exit Event Concepts

- Transfer requested.
- Transfer approved.
- Transfer Command sent.
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
- Management review requested.

Derived Intelligence Events must not imply:

- Pricing approval.
- Reservation.
- Allocation.
- Sale.
- Delivery.
- Transfer completion.

### Event Producer Rules

- Inventory Domain Service publishes accepted Inventory canonical and workflow-state changes.
- Integration services may publish normalized external-source observations.
- Reservation, Allocation, Transfer, or Publication services may publish their workflow Events according to approved service boundaries.
- AI Agents may publish Agent-run, analysis, or Recommendation Events.
- AI Agents must not publish authoritative availability, Reservation, Allocation, sale, delivery, or transfer Confirmation Events merely because they suggested an action.

### Event Requirements

Every material Inventory Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `inventory_record_id`.
- `vehicle_id`.
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
- Related Decision.
- Related Command.
- Related External Confirmation.
- Security classification.

Events are immutable.

Correction, reversal, release, cancellation, and revocation must use new Events linked to the original Event.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Inventory matching.
- Demand analysis.
- Aging analysis.
- Stock-pressure analysis.
- Markdown Recommendations.
- Transfer Recommendations.
- Publication-content drafting.
- Preparation-summary generation.
- Data-quality issue detection.
- Location-conflict detection.
- Availability-risk detection.
- Reservation-expiry risk.
- Allocation-stall detection.
- Deal-risk detection.
- Inventory-performance summarization.
- Management-priority generation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create authoritative acquisition cost.
- Change restricted pricing.
- Approve a discount.
- Confirm physical presence.
- Confirm commercial availability.
- Reserve a Vehicle outside an approved policy.
- Allocate a Vehicle to a Deal.
- Remove a legal, safety, compliance, ownership, financial, or quality block.
- Confirm a sale.
- Confirm delivery.
- Confirm transfer completion.
- Represent stale data as current.
- Represent a sent Command as completed.
- Access restricted Inventory data outside authorized Tenant and role scope.

### Recommendation Requirements

Every material Inventory Recommendation must preserve:

- Recommendation type.
- Inventory Record identifier and version.
- Vehicle identifier.
- Supporting evidence.
- Source authority.
- Input freshness.
- Applied Business Rules.
- Model, algorithm, or formula version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Expected commercial impact.
- Important risks.
- Action Class.
- Required Human authority or automation policy.
- Expiration time.

### Availability Reasoning

AI must distinguish between:

- Vehicle identity.
- Physical presence.
- Commercial availability.
- Reservation.
- Allocation.
- Sale.
- Delivery.
- Transfer.
- External Confirmation.

AI must not describe a Vehicle as available merely because:

- A Vehicle record exists.
- An old Inventory Record exists.
- A previous price exists.
- A source update was sent.
- No active Deal was found.
- The AI predicted likely availability.

### Pricing Reasoning

AI may recommend:

- Price review.
- Markdown amount.
- Campaign inclusion.
- Transfer.
- Wholesale review.
- Management escalation.

AI must not:

- Activate a restricted price.
- Expose internal pricing floors to unauthorized Users.
- Represent a market-reference price as an approved Customer price.
- Represent predicted demand as guaranteed demand.

### Action Class 2

Controlled Customer-facing or external actions may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Customer eligibility.
- Consent.
- Template.
- Channel.
- Frequency.
- Inventory freshness.
- Availability.
- Pricing authority.
- Revocation.
- Risk limits.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision.

Examples include:

- Restricted price approval.
- Discount override.
- Allocation override.
- Inventory release for sale.
- Transfer approval.
- Block removal.
- Delivery authorization.
- Financial or ownership override.

### AI Context and Embeddings

Restricted Inventory information must not enter general-purpose embeddings.

Normally excluded fields include:

- Acquisition cost.
- Landed cost.
- Internal price floor.
- Maximum discount.
- Gross-profit values.
- Floor-plan details.
- Supplier financial terms.
- Customer-specific Reservation information.
- Customer-specific Allocation information.
- Secure location information.
- Ownership evidence.
- Legal or compliance documents.

Approved non-sensitive context may include:

- Customer-visible Vehicle description.
- Approved advertised price.
- Non-sensitive availability summary.
- Preparation summary.
- Public location description.
- Inventory-age band.
- Demand category.
- Customer-visible features.

Every vector entry must enforce:

- `tenant_id`.
- Organizational access scope.
- Source references.
- Freshness.
- Security classification.
- Retention policy.
- Deletion and supersession propagation.

### Explainability

Inventory Recommendations must explain:

- Data used.
- Authority of the data.
- Current Inventory version.
- Freshness.
- Material conflicts.
- Calculation or model.
- Expected impact.
- Required approval.
- Why execution may remain pending.
- Whether External Confirmation is required.

---

## 10. API Contract

Detailed API request, response, error, and Schema definitions will become authoritative in the API Contracts Catalog.

This section defines required Inventory API behaviour.

### REST Resources

```text
GET    /api/v1/inventory-records
POST   /api/v1/inventory-records
GET    /api/v1/inventory-records/{inventory_record_id}
PATCH  /api/v1/inventory-records/{inventory_record_id}

POST   /api/v1/inventory-records/{inventory_record_id}/receive
POST   /api/v1/inventory-records/{inventory_record_id}/location-verifications
POST   /api/v1/inventory-records/{inventory_record_id}/inspection-requests
POST   /api/v1/inventory-records/{inventory_record_id}/preparation-actions
POST   /api/v1/inventory-records/{inventory_record_id}/make-available
POST   /api/v1/inventory-records/{inventory_record_id}/blocks
POST   /api/v1/inventory-records/{inventory_record_id}/block-releases

POST   /api/v1/inventory-records/{inventory_record_id}/reservation-requests
POST   /api/v1/inventory-records/{inventory_record_id}/reservation-releases
POST   /api/v1/inventory-records/{inventory_record_id}/allocation-requests
POST   /api/v1/inventory-records/{inventory_record_id}/allocation-releases

POST   /api/v1/inventory-records/{inventory_record_id}/price-proposals
POST   /api/v1/inventory-records/{inventory_record_id}/price-approvals
POST   /api/v1/inventory-records/{inventory_record_id}/publication-requests

POST   /api/v1/inventory-records/{inventory_record_id}/transfer-requests
POST   /api/v1/inventory-records/{inventory_record_id}/sale-confirmations
POST   /api/v1/inventory-records/{inventory_record_id}/delivery-confirmations
POST   /api/v1/inventory-records/{inventory_record_id}/retirement

GET    /api/v1/inventory-records/{inventory_record_id}/history
GET    /api/v1/inventory-records/{inventory_record_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, and location scope must be validated against authenticated permissions.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "inventory_type": "RETAIL_STOCK",
  "ownership_type": "DEALERSHIP_OWNED",
  "vehicle_condition": "NEW",
  "acquisition_source": "OEM_ALLOCATION",
  "acquisition_status": "CONFIRMED",
  "acquisition_date": "2026-07-15",
  "currency_code": "EGP",
  "source": {
    "source_system": "DMS",
    "source_record_id": "DMS-STOCK-4521",
    "source_authority": "EXTERNAL_SYSTEM_OF_RECORD"
  }
}
```

### Example Response

```json
{
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "inventory_number": "INV-2026-004521",
  "status": "ACQUIRED",
  "availability_status": "NOT_AVAILABLE",
  "physical_presence_status": "NOT_VERIFIED",
  "sale_readiness_status": "NOT_READY",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T17:00:00Z"
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
- Concurrency control.
- Freshness requirements.
- Conflict checks.
- Required Human Decision or automation policy.
- Audit evidence.
- Event publication after an accepted state change.
- External Confirmation tracking where applicable.

### Optimistic Concurrency

Updates must use an approved mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response.

### Reservation and Allocation Concurrency

Reservation and Allocation APIs must use:

- Atomic conditional update.
- Database lock.
- Compare-and-swap.
- Distributed lock.
- Another approved concurrency mechanism.

A successful HTTP response must not be returned as final business completion when authoritative external Confirmation remains pending.

### Idempotency

Retryable create, Reservation, Allocation, transfer, publication, and external-write operations must support:

```text
Idempotency-Key
```

The same idempotency key and request intent must not create duplicate effects.

### Pending External Confirmation

Operations requiring an external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "command_id": "44d94aa1-c0dd-497f-920e-c9bd93daebc0",
  "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
  "record_version": 8
}
```

The API must not describe the action as confirmed until authoritative evidence is received.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `INVENTORY_CONFLICT`
- `VEHICLE_ALREADY_HAS_ACTIVE_INVENTORY`
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
- Concurrency.
- Idempotency.
- Lifecycle.
- Approval.
- External Confirmation.
- Audit controls.

Resolvers must not bypass Inventory Domain Service or deterministic policy controls.

---

## 11. Database Design

### Recommended Tables

```text
inventory_records
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
inventory_floor_plan
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

### Inventory Records Table

The `inventory_records` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Vehicle relationship.
- Current stock-cycle identity.
- Current lifecycle status.
- Current availability status.
- Current location projection.
- Current Reservation projection.
- Current Allocation projection.
- Current pricing projection.
- Current preparation and readiness projection.
- Current publication state.
- Current sale, delivery, transfer, and exit projection.
- Source and synchronization status.
- Record version.
- Audit timestamps.

Historical detail must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (inventory_record_id)
```

### Tenant Protection

Every Inventory-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_inventory_tenant_status
  (tenant_id, status)

idx_inventory_tenant_availability
  (tenant_id, availability_status)

idx_inventory_tenant_vehicle_current
  (tenant_id, vehicle_id, is_current_record)

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

idx_inventory_sync_status
  (tenant_id, last_sync_status, reconciliation_status)

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

where the external source guarantees uniqueness.

A partial unique constraint or equivalent service control should prevent more than one current active physical-possession record for the same Vehicle:

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

The exact implementation may differ according to the database and transfer model.

### Reservation Storage

`inventory_reservations` should preserve:

- `reservation_id`.
- `tenant_id`.
- `inventory_record_id`.
- Customer and Opportunity references.
- Status.
- Requested time.
- Approved time.
- Expiration.
- Human Decision or automation-policy reference.
- Command.
- Idempotency key.
- External Confirmation.
- Release or expiration reason.
- Record version.
- Related Events.

Reservation history must remain append-only or versioned.

### Allocation Storage

`inventory_allocations` should preserve:

- `allocation_id`.
- `tenant_id`.
- `inventory_record_id`.
- Customer, Opportunity, and Deal references.
- Status.
- Requested time.
- Approved time.
- Expiration.
- Human Decision.
- Command.
- Idempotency key.
- External Confirmation.
- Release or conversion reason.
- Record version.
- Related Events.

### Pricing History

`inventory_price_history` must preserve:

- Previous price.
- New price.
- Currency.
- Price type.
- Effective period.
- Pricing authority.
- Human Decision.
- Business Rule.
- Recommendation reference.
- Reason.
- Actor.
- Timestamp.
- Record version.

Restricted pricing values must remain separately access-controlled.

### Location History

`inventory_location_history` must preserve:

- Previous location.
- New location.
- Physical-presence state.
- Transfer reference.
- Verification evidence.
- Actor.
- Source.
- Timestamp.
- External Confirmation.
- Record version.

### Derived Intelligence Storage

Derived metrics should remain separate from authoritative operational fields.

Each derived record should preserve:

- Metric type.
- Metric value.
- Algorithm, model, or formula version.
- Prompt version where applicable.
- Input-record versions.
- Evidence.
- Confidence.
- Generated time.
- Expiration time.
- Recommendation reference.

### Audit Storage

Inventory audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Audit storage should use secure hashes instead of raw sensitive values where full-value retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Retention class.
- Time for Event, history, and audit data.

Partitioning must not weaken Tenant isolation.

### Hard Deletion

An Inventory Record must not be hard-deleted when referenced by:

- Reservation.
- Allocation.
- Appointment.
- Quotation.
- Trade-In.
- Finance Application.
- Financial Contract.
- Deal.
- Payment.
- Delivery.
- Transfer.
- Publication.
- External Confirmation.
- Audit evidence.

Supersession, retirement, archival, or governed redaction must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `PUBLIC_VEHICLE_INFORMATION` | Approved make, model, trim, year, public features |
| `OPERATIONAL_INVENTORY` | Availability, branch, preparation, publication |
| `SENSITIVE_ASSET_LOCATION` | Parking slot, secure storage location |
| `COMMERCIAL_CONFIDENTIAL` | Cost, internal price floor, discount limit, margin |
| `FINANCIAL_CONFIDENTIAL` | Floor-plan and holding-cost information |
| `CUSTOMER_RESTRICTED` | Reservation and Allocation Customer references |
| `LEGAL_OR_COMPLIANCE` | Ownership evidence, legal holds, safety blocks |
| `DERIVED_INTELLIGENCE` | Scores, forecasts, Recommendations |
| `AUDIT_EVIDENCE` | Actor, Decisions, Commands, Confirmations, history |

### Authentication

Every Inventory operation requires an authenticated Human or service identity.

Anonymous access to internal Inventory records is prohibited.

Approved public inventory feeds must expose only permitted Customer-visible projections.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Location.
- Role.
- Team.
- Resource.
- Requested field.
- Requested action.
- Value threshold.
- Workflow state.
- Data classification.
- Delegated authority.
- Business purpose.

### Example Role Boundaries

#### Sales Consultant

May access approved Customer-visible:

- Vehicle specifications.
- Availability.
- Advertised pricing.
- Appointment and test-drive readiness.

A Sales Consultant must not independently access or change:

- Acquisition cost.
- Internal margin.
- Minimum authorized price.
- Floor-plan information.
- Restricted location.
- Reservation override.
- Allocation override.
- Sale Confirmation.
- Delivery Confirmation.

#### Sales Manager

May access permitted:

- Reservations.
- Allocations.
- Availability.
- Pricing approvals.
- Aging and performance.
- Assigned dealership Inventory.

Manager access does not automatically authorize:

- Legal-hold removal.
- Safety-block removal.
- Financial override.
- Cross-Tenant transfer.
- Final delivery Confirmation.

#### Inventory User

May perform permitted:

- Receipt.
- Location update.
- Inspection coordination.
- Preparation.
- Physical verification.
- Publication.
- Transfer preparation.

#### Inventory Manager

May perform permitted:

- Inventory oversight.
- Transfer approval.
- Aging review.
- Operational block management.
- Inventory reconciliation.
- Authorized pricing review.

#### Finance Specialist

May access restricted cost or floor-plan information only where required by approved responsibilities.

#### Compliance or Legal Reviewer

May access legal, ownership, safety, fraud, and compliance evidence required for an assigned review.

#### Data Steward

May review:

- Data conflicts.
- Duplicate records.
- Source provenance.
- Synchronization failures.
- Reconciliation cases.

#### AI Agent

May access only the minimum Inventory context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from retrieving cross-Tenant information.
- Prevented from accessing restricted cost or margin data unless specifically approved.

### Pricing and Cost Protection

Restricted commercial values must use:

- Field-level authorization.
- Masking.
- Encryption where appropriate.
- Audit logging.
- Export restrictions.
- AI-context restrictions.

Minimum authorized price, maximum discount, acquisition cost, landed cost, and internal margin must not appear in:

- Public APIs.
- Public search indexes.
- Public Inventory feeds.
- Customer communications.
- Unrestricted Logs.
- General-purpose embeddings.

### Location Protection

Exact secure storage locations, parking slots, access instructions, and movement schedules must be limited to authorized operational roles.

Customer-facing location descriptions must use approved public values.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- APIs.
- Search.
- Vector retrieval.
- Events.
- Queues.
- Caches.
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
- Tenant scope.
- Organizational scope.
- Field-level write authority.
- Applicable Human Decision or automation-policy reference.
- Idempotency key.
- Current record version.
- Audit evidence.
- Confirmation requirements.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Inventory activity must record:

- `tenant_id`.
- `inventory_record_id`.
- `vehicle_id`.
- Dealership and branch.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Record version.
- Source.
- Authority category.
- Human Decision.
- Automation-policy reference.
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

- Cross-Tenant Inventory access attempts.
- Unauthorized Reservation or Allocation.
- Duplicate Reservation or Allocation attempts.
- Restricted pricing access.
- Unauthorized cost export.
- Availability manipulation.
- Location tampering.
- Block-removal attempts.
- AI access outside approved scope.
- Command replay.
- External Confirmation mismatch.
- Audit-log tampering.
- Repeated reconciliation failure.
- Suspicious Inventory transfer.

### Evidence Protection

Ownership, acquisition, inspection, safety, transfer, sale, and delivery evidence must:

- Use controlled storage.
- Preserve integrity hashes where required.
- Restrict access.
- Preserve provenance.
- Prevent unauthorized deletion.
- Follow retention and legal-hold requirements.

### Emergency Controls

The platform must support immediate suspension of:

- Reservation.
- Allocation.
- Publication.
- Pricing updates.
- Transfer.
- Sale release.
- Delivery release.
- External Inventory write-back.
- AI Agent access.

Emergency suspension must be deterministic, Tenant-scoped, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Vehicle](./Vehicle.md)

---

## Current Status

This document is the approved Canonical Inventory Record baseline.

Inventory context remains separate from Vehicle identity.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
