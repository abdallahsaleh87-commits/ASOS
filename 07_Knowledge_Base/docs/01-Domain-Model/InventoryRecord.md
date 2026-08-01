# Inventory Record

## 1. Object Purpose

### Business Purpose

The Inventory Record object represents the dealership’s commercial and operational stock position for one specific Vehicle.

It records whether the Vehicle is:

- Available for sale.
- Reserved.
- Allocated to a Customer or Deal.
- In transit.
- Awaiting preparation.
- Under inspection.
- On display.
- Assigned for a test drive.
- Temporarily blocked.
- Sold.
- Delivered.
- Transferred.
- Returned.
- Removed from active inventory.

The Inventory Record enables the dealership to manage:

- Vehicle availability.
- Stock location.
- Inventory ownership.
- Acquisition and landed cost.
- Retail pricing.
- Discount boundaries.
- Reservation and allocation.
- Vehicle preparation.
- Inventory aging.
- Stock transfers.
- Floor-plan or financing obligations.
- Sales-channel publication.
- Inventory reconciliation.
- Stock performance and profitability analysis.

A Vehicle describes the canonical physical or catalogued asset.

An Inventory Record describes how that Vehicle is commercially held, located, valued, prepared, offered, reserved, allocated, sold, or retired by a specific dealership.

A Vehicle may have multiple historical Inventory Records across different dealerships, ownership periods, transfers, or acquisition cycles, but only one active Inventory Record may represent the same physical Vehicle within the same dealership stock context at a time.

### System Purpose

The Inventory Record object is the canonical dealership-stock and vehicle-availability aggregate within the ASOS domain.

It connects:

- Dealership
- Vehicle
- Vehicle Model
- Supplier
- OEM
- Acquisition Order
- Stock Transfer
- Warehouse
- Branch
- Showroom
- Parking Location
- Lead
- Opportunity
- Appointment
- Quotation
- Deal
- Reservation
- Trade-In
- Market Intelligence
- Pricing Rule
- Campaign
- Finance or Floor-Plan Provider
- User
- AI Agent

The object provides authoritative inventory context to:

- Vehicle search and matching.
- Availability checks.
- Appointment and test-drive scheduling.
- Quotation generation.
- Reservation workflows.
- Deal allocation.
- Vehicle transfer workflows.
- Inventory pricing.
- Inventory aging and markdown analysis.
- Campaign selection.
- Trade-In acquisition.
- Sales-channel publication.
- Forecasting.
- Management reporting.

Every availability, reservation, allocation, transfer, sale, delivery, and retirement action must be tenant-scoped, auditable, concurrency-safe, and traceable to the responsible User, Agent, provider, or workflow.

The Inventory Record does not replace:

- Vehicle identity and specifications.
- Deal commercial terms.
- Payment evidence.
- Vehicle-delivery confirmation.
- Ownership-registration evidence.
- Supplier invoices.
- Accounting ledger entries.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `inventory_record_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `vehicle_id` (UUIDv4 — required)
  - `vehicle_model_id` (UUIDv4 — optional)
  - `branch_id` (UUIDv4 — required)
  - `location_id` (UUIDv4 — required)
  - `supplier_id` (UUIDv4 — optional)
  - `oem_id` (UUIDv4 — optional)
  - `acquisition_order_id` (UUIDv4 — optional)
  - `trade_in_id` (UUIDv4 — optional)
  - `reservation_id` (UUIDv4 — optional)
  - `allocated_customer_id` (UUIDv4 — optional)
  - `allocated_opportunity_id` (UUIDv4 — optional)
  - `allocated_deal_id` (UUIDv4 — optional)
  - `pricing_rule_id` (UUIDv4 — optional)
  - `floor_plan_provider_id` (UUIDv4 — optional)
  - `current_transfer_id` (UUIDv4 — optional)
  - `assigned_inventory_user_id` (UUIDv4 — optional)
  - `supersedes_inventory_record_id` (UUIDv4 — optional)

### Inventory Identity

- `inventory_number`
- `stock_number`
- `inventory_type`
- `ownership_type`
- `status`
- `availability_status`
- `sales_status`
- `condition_status`
- `source_type`
- `inventory_version`
- `is_current_record`

### Vehicle Snapshot

- `vin`
- `registration_number`
- `make`
- `model`
- `trim`
- `model_year`
- `vehicle_condition`
- `body_type`
- `fuel_type`
- `transmission`
- `exterior_color`
- `interior_color`
- `odometer_value`
- `odometer_unit`
- `vehicle_snapshot`

### Acquisition Information

- `acquisition_source`
- `acquisition_date`
- `acquisition_reference`
- `supplier_id`
- `oem_id`
- `trade_in_id`
- `acquisition_order_id`
- `purchase_price_amount`
- `transport_cost_amount`
- `inspection_cost_amount`
- `reconditioning_cost_amount`
- `registration_cost_amount`
- `tax_cost_amount`
- `other_acquisition_cost_amount`
- `landed_cost_amount`
- `acquisition_snapshot`

### Location Information

- `branch_id`
- `location_id`
- `location_type`
- `location_name`
- `parking_slot`
- `display_zone`
- `storage_zone`
- `current_transfer_id`
- `physical_presence_status`
- `last_location_verified_at`
- `location_snapshot`

### Availability and Reservation

- `availability_status`
- `available_from`
- `available_until`
- `reservation_id`
- `reserved_for_customer_id`
- `reserved_for_opportunity_id`
- `reservation_status`
- `reservation_expires_at`
- `reservation_priority`
- `reservation_channel`
- `availability_block_reason`
- `availability_snapshot`

### Allocation and Deal Context

- `allocated_customer_id`
- `allocated_opportunity_id`
- `allocated_deal_id`
- `allocation_status`
- `allocated_at`
- `allocation_expires_at`
- `allocation_reason`
- `allocation_snapshot`

### Pricing and Commercial Fields

- `currency_code`
- `list_price_amount`
- `advertised_price_amount`
- `minimum_authorized_price_amount`
- `target_sale_price_amount`
- `market_reference_price_amount`
- `current_discount_amount`
- `maximum_discount_amount`
- `estimated_gross_profit_amount`
- `estimated_gross_margin_percentage`
- `pricing_rule_id`
- `price_effective_from`
- `price_effective_until`
- `pricing_snapshot`

### Floor-Plan and Financial Fields

- `floor_plan_provider_id`
- `floor_plan_reference`
- `floor_plan_principal_amount`
- `floor_plan_interest_rate`
- `floor_plan_start_date`
- `floor_plan_maturity_date`
- `floor_plan_daily_cost_amount`
- `floor_plan_accrued_cost_amount`
- `floor_plan_status`
- `financial_holding_cost_amount`
- `financial_snapshot`

### Preparation and Quality

- `inspection_status`
- `pre_delivery_inspection_status`
- `reconditioning_status`
- `cleaning_status`
- `photography_status`
- `documentation_status`
- `registration_status`
- `insurance_status`
- `quality_hold_status`
- `quality_hold_reason`
- `ready_for_sale`
- `ready_for_test_drive`
- `ready_for_delivery`
- `preparation_snapshot`

### Publication and Sales Channels

- `publication_status`
- `published_channel_ids`
- `primary_sales_channel`
- `website_published`
- `marketplace_published`
- `social_media_published`
- `oem_platform_published`
- `published_at`
- `last_publication_sync_at`
- `publication_error`
- `publication_snapshot`

### Demand and Activity

- `view_count`
- `inquiry_count`
- `lead_count`
- `qualified_lead_count`
- `opportunity_count`
- `quotation_count`
- `appointment_count`
- `test_drive_count`
- `reservation_count`
- `deal_count`
- `demand_score`
- `engagement_score`
- `market_interest_score`
- `activity_snapshot`

### Aging and Performance

- `days_in_inventory`
- `days_since_available`
- `days_since_last_activity`
- `inventory_age_band`
- `aging_status`
- `turn_rate`
- `price_reduction_count`
- `last_price_reduction_at`
- `recommended_markdown_amount`
- `recommended_action`
- `stock_pressure_score`
- `performance_snapshot`

### Sale and Exit Information

- `sold_deal_id`
- `sold_at`
- `sold_price_amount`
- `final_discount_amount`
- `realized_gross_profit_amount`
- `delivered_at`
- `transfer_out_at`
- `returned_at`
- `retired_at`
- `exit_reason`
- `exit_reference`
- `exit_snapshot`

### Computed Fields

- `landed_cost_amount`
- `total_holding_cost_amount`
- `current_inventory_value_amount`
- `potential_gross_profit_amount`
- `potential_gross_margin_percentage`
- `days_in_inventory`
- `days_until_reservation_expiry`
- `days_until_allocation_expiry`
- `days_until_floor_plan_maturity`
- `publication_completion_percentage`
- `preparation_completion_percentage`
- `demand_score`
- `stock_pressure_score`
- `price_position_percentage`
- `is_available_for_sale`
- `is_available_for_test_drive`
- `is_reserved`
- `is_allocated`
- `is_stale_inventory`
- `requires_management_review`

### Governance and Lifecycle

- **Vehicle Snapshot:** `vehicle_snapshot` (JSONB)
- **Acquisition Snapshot:** `acquisition_snapshot` (Encrypted JSONB)
- **Location Snapshot:** `location_snapshot` (JSONB)
- **Availability Snapshot:** `availability_snapshot` (JSONB)
- **Allocation Snapshot:** `allocation_snapshot` (Encrypted JSONB)
- **Pricing Snapshot:** `pricing_snapshot` (Encrypted JSONB)
- **Financial Snapshot:** `financial_snapshot` (Encrypted JSONB)
- **Preparation Snapshot:** `preparation_snapshot` (JSONB)
- **Publication Snapshot:** `publication_snapshot` (JSONB)
- **Activity Snapshot:** `activity_snapshot` (JSONB)
- **Performance Snapshot:** `performance_snapshot` (JSONB)
- **Exit Snapshot:** `exit_snapshot` (Encrypted JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `acquired_by`
  - `priced_by`
  - `reserved_by`
  - `allocated_by`
  - `transferred_by`
  - `sold_by`
  - `retired_by`
  - `last_verified_by`
  - `last_processed_by_agent`

- **Version:**
  - `inventory_version`
  - `record_version`
  - `supersedes_inventory_record_id`
  - `is_current_record`

- **Soft Delete:**
  - `is_deleted`
  - `deleted_at`

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `acquisition_date`
  - `available_from`
  - `reserved_at`
  - `reservation_expires_at`
  - `allocated_at`
  - `allocation_expires_at`
  - `published_at`
  - `last_location_verified_at`
  - `last_activity_at`
  - `sold_at`
  - `delivered_at`
  - `transfer_out_at`
  - `returned_at`
  - `retired_at`
  - `archived_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| inventory_record_id | UUID | Unique canonical identifier for the Inventory Record. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Inventory Record. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| vehicle_id | UUID | Canonical Vehicle represented by the stock record. | Yes | N/A | Must reference an existing Vehicle | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| vehicle_model_id | UUID | Vehicle Model associated with the Vehicle. | No | From Vehicle | Must match the canonical Vehicle specification | 666e7777-e88b-99d0-a111-426614174000 | System-controlled |
| branch_id | UUID | Dealership branch operationally responsible for the stock unit. | Yes | Active branch | Must belong to the same dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| location_id | UUID | Current physical or controlled inventory location. | Yes | N/A | Must be active and permitted for the branch | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| inventory_number | String | Human-readable Inventory Record reference. | Yes | System-generated | Must be unique within the dealership | INV-2026-004521 | System-generated |
| stock_number | String | Dealership stock number displayed operationally. | Yes | System-generated | Must be unique among active dealership stock | STK-04521 | System-generated |
| inventory_type | Enum | Type of inventory holding. | Yes | RETAIL_STOCK | Must match InventoryType ENUM | RETAIL_STOCK | At least 0.99 |
| ownership_type | Enum | Legal or financial ownership arrangement. | Yes | DEALERSHIP_OWNED | Must match InventoryOwnershipType ENUM | FLOOR_PLAN_FINANCED | Authoritative evidence |
| status | Enum | Current Inventory Record lifecycle state. | Yes | ACQUIRED | Must match InventoryRecordStatus ENUM | AVAILABLE | At least 0.99 |
| availability_status | Enum | Current commercial availability. | Yes | NOT_AVAILABLE | Must match InventoryAvailabilityStatus ENUM | AVAILABLE_FOR_SALE | System-controlled |
| vehicle_condition | Enum | Commercial condition of the Vehicle. | Yes | NEW | Must match the Vehicle record and InventoryCondition ENUM | USED | Authoritative source |
| vin | String | Vehicle Identification Number copied from the canonical Vehicle. | Conditional | From Vehicle | Required for physical vehicles and must match the Vehicle record | WBA12345678901234 | Authoritative Vehicle source |
| acquisition_source | Enum | Source through which the dealership obtained the Vehicle. | Yes | OEM_ALLOCATION | Must match InventoryAcquisitionSource ENUM | TRADE_IN | Authoritative source |
| acquisition_date | Date | Date the dealership acquired or accepted responsibility for the Vehicle. | Yes | N/A | Cannot be later than the current date without an in-transit exception | 2026-07-15 | Authoritative source |
| purchase_price_amount | Decimal | Direct acquisition price paid or payable for the Vehicle. | Yes | 0.00 | Must be zero or greater | 1850000.00 | Authorized financial source |
| landed_cost_amount | Decimal | Total acquisition cost including approved additional costs. | Yes | Calculated | Must be system-computed using the approved cost formula | 1915000.00 | System-computed |
| location_type | Enum | Type of physical or controlled location. | Yes | SHOWROOM | Must match InventoryLocationType ENUM | WAREHOUSE | At least 0.99 |
| physical_presence_status | Enum | Verification state of the Vehicle's physical presence. | Yes | NOT_VERIFIED | Must match PhysicalPresenceStatus ENUM | VERIFIED_PRESENT | Authorized verification |
| list_price_amount | Decimal | Standard dealership retail list price. | Yes | N/A | Must be zero or greater | 2250000.00 | Authorized pricing source |
| advertised_price_amount | Decimal | Current Customer-visible advertised price. | Yes | From list price | Must comply with pricing and advertising rules | 2190000.00 | Authorized pricing source |
| minimum_authorized_price_amount | Decimal | Lowest permitted sale price without additional approval. | Yes | Policy-defined | Must not exceed list price | 2075000.00 | Restricted pricing authority |
| current_discount_amount | Decimal | Current advertised or approved discount. | Yes | 0.00 | Must be zero or greater and not exceed maximum discount | 60000.00 | System-computed |
| estimated_gross_profit_amount | Decimal | Estimated profit before final transaction adjustments. | No | Calculated | Must use approved pricing and cost sources | 275000.00 | System-computed |
| reservation_id | UUID | Active Reservation holding the Vehicle. | No | Null | Required when availability status is RESERVED | 333e4444-e55b-66d7-a888-426614174000 | System-controlled |
| reserved_for_customer_id | UUID | Customer for whom the Vehicle is temporarily reserved. | No | Null | Required for Customer-specific reservation | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| reservation_expires_at | Timestamp | Deadline after which the reservation expires. | No | Null | Required for time-limited reservations and must be later than reserved_at | 2026-08-02T18:00:00Z | System-controlled |
| allocated_deal_id | UUID | Deal to which the Vehicle is formally allocated. | No | Null | Required when allocation status is ALLOCATED | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| ready_for_sale | Boolean | Indicates whether all required sale-readiness checks passed. | Yes | false | Must be derived from preparation and compliance checks | true | System-controlled |
| ready_for_test_drive | Boolean | Indicates whether the Vehicle may be used for a test drive. | Yes | false | Must require applicable inspection, registration, and insurance checks | true | System-controlled |
| ready_for_delivery | Boolean | Indicates whether the Vehicle may be delivered to the Customer. | Yes | false | Must require Deal, Payment, preparation, registration, and contract conditions | false | System-controlled |
| inspection_status | Enum | Mechanical or inventory inspection state. | Yes | NOT_STARTED | Must match InventoryPreparationStatus ENUM | COMPLETED | Authorized workflow |
| reconditioning_status | Enum | Vehicle reconditioning state. | Yes | NOT_REQUIRED | Must match InventoryPreparationStatus ENUM | IN_PROGRESS | Authorized workflow |
| publication_status | Enum | Current publication state across sales channels. | Yes | NOT_PUBLISHED | Must match InventoryPublicationStatus ENUM | PUBLISHED | System-controlled |
| demand_score | Decimal | Normalized estimate of commercial demand for the Vehicle. | No | 0.00 | Must remain between 0.00 and 1.00 | 0.82 | System-computed |
| stock_pressure_score | Decimal | Normalized urgency to sell, transfer, or reprice the stock unit. | No | 0.00 | Must remain between 0.00 and 1.00 | 0.67 | System-computed |
| days_in_inventory | Integer | Number of calendar days since acquisition or inventory activation. | Yes | Calculated | Must be zero or greater | 17 | System-computed |
| aging_status | Enum | Inventory-aging classification. | Yes | FRESH | Must match InventoryAgingStatus ENUM | AGED | System-computed |
| floor_plan_status | Enum | Status of the stock financing arrangement. | No | NOT_APPLICABLE | Must match FloorPlanStatus ENUM | ACTIVE | Authoritative financial source |
| sold_deal_id | UUID | Deal through which the stock unit was sold. | No | Null | Required when status becomes SOLD | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| sold_price_amount | Decimal | Final authoritative Vehicle sale price. | No | Null | Must match the approved Deal | 2130000.00 | Authoritative Deal source |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 9 | System-controlled |

## 4. Enumerations

### InventoryRecordStatus

- **PLANNED:** The stock unit is expected but has not yet been acquired.
- **ORDERED:** The Vehicle was ordered from an OEM, supplier, or source.
- **IN_TRANSIT:** The Vehicle is moving toward the dealership or assigned location.
- **ACQUIRED:** The dealership accepted ownership or operational responsibility.
- **RECEIVED:** The Vehicle arrived at an authorized location.
- **INSPECTION_PENDING:** Initial inspection is required.
- **PREPARATION_IN_PROGRESS:** Inspection, reconditioning, cleaning, registration, or photography is underway.
- **AVAILABLE:** The Vehicle is ready and available for permitted commercial activity.
- **RESERVED:** A valid temporary reservation exists.
- **ALLOCATED:** The Vehicle is formally allocated to an Opportunity or Deal.
- **SALE_PENDING:** A Deal exists and sale-completion conditions are being processed.
- **SOLD:** An authoritative Deal confirms the Vehicle sale.
- **DELIVERY_PENDING:** The sold Vehicle is awaiting Customer delivery.
- **DELIVERED:** The Vehicle was delivered to the Customer.
- **TRANSFER_PENDING:** An authorized stock transfer is in progress.
- **TRANSFERRED_OUT:** The Vehicle left this dealership inventory context.
- **RETURN_PENDING:** The Vehicle is being returned to the supplier, OEM, owner, or source.
- **RETURNED:** The Vehicle was returned and is no longer active stock.
- **BLOCKED:** Commercial use is temporarily prohibited.
- **RETIRED:** The stock record is no longer active for sale or transfer.
- **ARCHIVED:** The historical record moved to long-term retention.

### InventoryType

- RETAIL_STOCK
- DEMONSTRATOR
- TEST_DRIVE
- LOANER
- DISPLAY
- FLEET
- WHOLESALE
- CONSIGNMENT
- TRADE_IN_STOCK
- CERTIFIED_PRE_OWNED
- SERVICE_REPLACEMENT
- NON_RETAIL
- OTHER

### InventoryOwnershipType

- DEALERSHIP_OWNED
- FLOOR_PLAN_FINANCED
- OEM_OWNED
- SUPPLIER_OWNED
- CONSIGNMENT
- CUSTOMER_OWNED
- LEASED
- DEMONSTRATION_LOAN
- OTHER

### InventoryAvailabilityStatus

- NOT_AVAILABLE
- AVAILABLE_FOR_SALE
- AVAILABLE_FOR_TEST_DRIVE
- AVAILABLE_FOR_TRANSFER
- AVAILABLE_FOR_WHOLESALE
- RESERVED
- ALLOCATED
- SALE_PENDING
- BLOCKED
- SOLD
- DELIVERED
- TRANSFERRED
- RETURNED
- RETIRED

### InventoryCondition

- NEW
- USED
- CERTIFIED_PRE_OWNED
- DEMONSTRATOR
- DAMAGED
- RECONDITIONED
- SALVAGE
- UNKNOWN

### InventoryAcquisitionSource

- OEM_ALLOCATION
- OEM_ORDER
- DEALER_TRANSFER
- SUPPLIER_PURCHASE
- AUCTION
- TRADE_IN
- CUSTOMER_BUYBACK
- FLEET_RETURN
- LEASE_RETURN
- CONSIGNMENT
- IMPORT
- MANUAL_ENTRY
- OTHER

### InventoryLocationType

- SHOWROOM
- WAREHOUSE
- PARKING_YARD
- TRANSIT_HUB
- SERVICE_CENTER
- BODY_SHOP
- DETAILING_CENTER
- TEST_DRIVE_ZONE
- CUSTOMER_DELIVERY_AREA
- EXTERNAL_STORAGE
- SUPPLIER_LOCATION
- IN_TRANSIT
- OTHER

### PhysicalPresenceStatus

- NOT_VERIFIED
- VERIFICATION_PENDING
- VERIFIED_PRESENT
- VERIFIED_ABSENT
- LOCATION_MISMATCH
- IN_TRANSIT
- MANUAL_REVIEW_REQUIRED

### InventoryReservationStatus

- NOT_RESERVED
- PENDING
- ACTIVE
- EXPIRED
- RELEASED
- CONVERTED_TO_ALLOCATION
- CANCELLED
- DISPUTED

### InventoryAllocationStatus

- NOT_ALLOCATED
- PENDING
- ALLOCATED
- EXPIRED
- RELEASED
- CONVERTED_TO_SALE
- CANCELLED
- DISPUTED

### InventoryPreparationStatus

- NOT_STARTED
- NOT_REQUIRED
- PENDING
- IN_PROGRESS
- COMPLETED
- FAILED
- BLOCKED
- EXPIRED
- MANUAL_REVIEW_REQUIRED

### InventoryPublicationStatus

- NOT_PUBLISHED
- READY_TO_PUBLISH
- PUBLISHING
- PARTIALLY_PUBLISHED
- PUBLISHED
- UPDATE_PENDING
- UNPUBLISHING
- UNPUBLISHED
- FAILED
- BLOCKED

### InventoryAgingStatus

- FRESH
- NORMAL
- MATURING
- AGED
- CRITICAL
- EXEMPT

### FloorPlanStatus

- NOT_APPLICABLE
- PENDING
- ACTIVE
- MATURING
- MATURED
- PAYMENT_DUE
- SETTLED
- DEFAULTED
- DISPUTED
- CANCELLED

### InventoryBlockReason

- QUALITY_HOLD
- SAFETY_RECALL
- COMPLIANCE_HOLD
- DOCUMENTS_INCOMPLETE
- LOCATION_UNVERIFIED
- DAMAGE_DETECTED
- PRICE_REVIEW_REQUIRED
- OWNERSHIP_DISPUTE
- PAYMENT_OR_LIEN_ISSUE
- TRANSFER_IN_PROGRESS
- LEGAL_HOLD
- FRAUD_REVIEW
- OTHER

### InventoryExitReason

- SOLD
- DELIVERED
- TRANSFERRED
- RETURNED_TO_SUPPLIER
- RETURNED_TO_OWNER
- WHOLESALE_DISPOSAL
- AUCTION_DISPOSAL
- WRITTEN_OFF
- TOTAL_LOSS
- DAMAGED_BEYOND_REPAIR
- DUPLICATE_RECORD
- DATA_CORRECTION
- OTHER

## 5. Validation Rules

### Business Rules

- Every Inventory Record must reference one canonical Vehicle.
- A physical Vehicle may have only one active Inventory Record within the same dealership inventory context.
- A Vehicle cannot be offered for sale unless:
  - It is physically present or an authorized future-stock sale policy applies.
  - Required ownership or consignment evidence exists.
  - Required inspection and preparation checks pass.
  - No safety, compliance, legal, quality, payment, or ownership block exists.
  - Valid pricing exists.

- A Vehicle cannot be reserved if it is:
  - Sold.
  - Delivered.
  - Transferred out.
  - Returned.
  - Retired.
  - Blocked.
  - Already allocated to another active Deal.

- A Vehicle cannot be allocated to more than one active Deal.
- A reservation does not create a completed sale.
- Allocation does not prove Payment, contract completion, funding, or delivery.
- Published availability must match the authoritative Inventory Record.
- Customer-facing price must come from an approved pricing source.
- Minimum price and margin information must remain restricted.
- Inventory aging recommendations are advisory until approved.
- A Vehicle sold through a Deal must reference the same Customer, Opportunity, Quotation, and Vehicle context.
- Trade-In stock cannot become available before ownership, payoff, inspection, and acquisition requirements pass.
- Floor-plan maturity, legal hold, safety recall, or ownership dispute may block sale or delivery.
- A sold Vehicle cannot return to `AVAILABLE` without a controlled Deal cancellation, return, or correction workflow.
- AI Agents cannot independently:
  - Change authoritative cost.
  - Approve discounts.
  - Reserve stock without policy authority.
  - Allocate a Vehicle to a Deal.
  - Mark a Vehicle sold.
  - Confirm Vehicle delivery.
  - Remove legal, safety, compliance, or financial blocks.

### Technical Rules

- Inventory creation, reservation, release, allocation, transfer, sale, and retirement must require idempotency keys.
- Availability, reservation, and allocation updates must use optimistic or pessimistic concurrency controls.
- `record_version` must increase after every permitted update.
- Reservation and allocation commands must be atomic.
- Publication updates must be idempotent and reconciliation-safe.
- Inventory status changes must create immutable history.
- Cost and pricing calculations must use fixed decimal precision.
- Computed fields must preserve their formula version.
- External inventory feeds must preserve:
  - Provider.
  - Source reference.
  - Batch or event ID.
  - Retrieval timestamp.
  - Data hash.
  - Processing result.

- Location-verification events must preserve evidence and verifier identity.
- Failed multi-service operations must create compensating actions.
- A failed Deal allocation must release any temporary hold according to policy.
- Inventory publication must not expose restricted fields.
- Historical sold, transferred, returned, or retired records must remain immutable.

### Data Constraints

- Financial amounts cannot be negative unless the field explicitly supports a signed adjustment.
- `landed_cost_amount` must equal the approved acquisition-cost formula.
- `minimum_authorized_price_amount` cannot exceed `list_price_amount`.
- `advertised_price_amount` cannot fall below the minimum authorized price without approved exception evidence.
- `current_discount_amount` cannot exceed `maximum_discount_amount`.
- `reservation_expires_at` must be later than `reserved_at`.
- `allocation_expires_at` must be later than `allocated_at`.
- `floor_plan_maturity_date` must be later than `floor_plan_start_date`.
- `sold_at` cannot precede `acquisition_date`.
- `delivered_at` cannot precede `sold_at`.
- Counts cannot be negative.
- Scores must remain between `0.00` and `1.00`.
- `days_in_inventory` cannot be negative.
- `reserved_for_customer_id` is required for Customer-specific reservations.
- `allocated_deal_id` is required when allocation status is `ALLOCATED`.
- `sold_deal_id`, `sold_at`, and `sold_price_amount` are required when status is `SOLD`.
- `availability_block_reason` is required when status is `BLOCKED`.
- `exit_reason` is required for returned, transferred, retired, or archived exits.
- `last_location_verified_at` is required when physical presence is `VERIFIED_PRESENT`.

### Referential Integrity

- All dealership-owned references must belong to the same `dealership_id`.
- `vehicle_model_id` must match the linked Vehicle.
- `trade_in_id` must reference the Vehicle acquired through the Trade-In workflow when applicable.
- `reserved_for_customer_id` and `reserved_for_opportunity_id` must represent the same Customer Journey context.
- `allocated_customer_id`, `allocated_opportunity_id`, and `allocated_deal_id` must match one transaction context.
- `sold_deal_id` must reference the same Vehicle.
- `pricing_rule_id` must be valid for the Vehicle, branch, inventory type, and effective period.
- `branch_id` and `location_id` must have an authorized relationship.
- `floor_plan_provider_id` must reference the active finance arrangement when floor-plan fields are populated.
- `supersedes_inventory_record_id` must reference an earlier Inventory Record for the same Vehicle and inventory series.
- Circular supersession relationships are prohibited.
- A sold, delivered, transferred, returned, or retired Inventory Record cannot be hard-deleted.

### Human Approval Requirements

- Sale below minimum authorized price requires approved pricing authority.
- Manual landed-cost changes require financial evidence.
- Inventory-location mismatches require Human Review.
- Conflicting VIN, ownership, Vehicle identity, or Deal evidence requires Human Review.
- Safety recall, legal hold, ownership dispute, fraud risk, or compliance block requires specialist review.
- Releasing an active Deal allocation requires authorized sales-management approval.
- Overriding an active Customer reservation requires an approved reason.
- Marking a Vehicle sold requires authoritative Deal evidence.
- Marking a Vehicle delivered requires authoritative Vehicle Delivery evidence.
- Reopening sold, delivered, transferred, returned, or retired inventory requires a controlled exception workflow.
- AI-generated repricing, markdown, transfer, or sourcing recommendations remain advisory.
- AI Agents cannot override restricted commercial, legal, compliance, safety, ownership, or financial decisions.

## 6. State Machine

### Allowed States

- PLANNED
- ORDERED
- IN_TRANSIT
- ACQUIRED
- RECEIVED
- INSPECTION_PENDING
- PREPARATION_IN_PROGRESS
- AVAILABLE
- RESERVED
- ALLOCATED
- SALE_PENDING
- SOLD
- DELIVERY_PENDING
- DELIVERED
- TRANSFER_PENDING
- TRANSFERRED_OUT
- RETURN_PENDING
- RETURNED
- BLOCKED
- RETIRED
- ARCHIVED

### Allowed Transitions

- PLANNED → ORDERED
- PLANNED → RETIRED
- ORDERED → IN_TRANSIT
- ORDERED → ACQUIRED
- ORDERED → RETURN_PENDING
- ORDERED → RETIRED
- IN_TRANSIT → RECEIVED
- IN_TRANSIT → RETURN_PENDING
- IN_TRANSIT → BLOCKED
- ACQUIRED → RECEIVED
- ACQUIRED → INSPECTION_PENDING
- ACQUIRED → BLOCKED
- RECEIVED → INSPECTION_PENDING
- RECEIVED → PREPARATION_IN_PROGRESS
- RECEIVED → BLOCKED
- INSPECTION_PENDING → PREPARATION_IN_PROGRESS
- INSPECTION_PENDING → AVAILABLE
- INSPECTION_PENDING → BLOCKED
- PREPARATION_IN_PROGRESS → AVAILABLE
- PREPARATION_IN_PROGRESS → BLOCKED
- AVAILABLE → RESERVED
- AVAILABLE → ALLOCATED
- AVAILABLE → TRANSFER_PENDING
- AVAILABLE → RETURN_PENDING
- AVAILABLE → BLOCKED
- AVAILABLE → RETIRED
- RESERVED → AVAILABLE
- RESERVED → ALLOCATED
- RESERVED → SALE_PENDING
- RESERVED → BLOCKED
- ALLOCATED → AVAILABLE
- ALLOCATED → SALE_PENDING
- ALLOCATED → BLOCKED
- SALE_PENDING → SOLD
- SALE_PENDING → AVAILABLE
- SALE_PENDING → BLOCKED
- SOLD → DELIVERY_PENDING
- SOLD → AVAILABLE
- SOLD → BLOCKED
- DELIVERY_PENDING → DELIVERED
- DELIVERY_PENDING → BLOCKED
- TRANSFER_PENDING → AVAILABLE
- TRANSFER_PENDING → TRANSFERRED_OUT
- TRANSFER_PENDING → BLOCKED
- RETURN_PENDING → AVAILABLE
- RETURN_PENDING → RETURNED
- RETURN_PENDING → BLOCKED
- BLOCKED → INSPECTION_PENDING
- BLOCKED → PREPARATION_IN_PROGRESS
- BLOCKED → AVAILABLE
- BLOCKED → RESERVED
- BLOCKED → ALLOCATED
- BLOCKED → SALE_PENDING
- BLOCKED → RETURN_PENDING
- BLOCKED → RETIRED
- DELIVERED → ARCHIVED
- TRANSFERRED_OUT → ARCHIVED
- RETURNED → ARCHIVED
- RETIRED → ARCHIVED

### Forbidden Transitions

- PLANNED → AVAILABLE
- ORDERED → SOLD
- IN_TRANSIT → AVAILABLE
- ACQUIRED → SOLD
- RECEIVED → SOLD
- INSPECTION_PENDING → RESERVED
- PREPARATION_IN_PROGRESS → SOLD
- AVAILABLE → SOLD
- RESERVED → SOLD
- ALLOCATED → SOLD
- SOLD → RESERVED
- SOLD → ALLOCATED
- DELIVERED → AVAILABLE
- DELIVERED → RESERVED
- TRANSFERRED_OUT → AVAILABLE
- RETURNED → AVAILABLE
- RETIRED → AVAILABLE
- ARCHIVED → AVAILABLE
- ARCHIVED → SOLD
- ARCHIVED → DELIVERED

### Entry Conditions

- To enter `ORDERED`:
  - An approved acquisition order or supplier commitment must exist.
  - Expected Vehicle, quantity, source, and destination must be known.

- To enter `IN_TRANSIT`:
  - Dispatch or shipping evidence must exist.
  - Expected destination and estimated arrival must be recorded.

- To enter `ACQUIRED`:
  - Ownership, consignment, transfer, or acquisition authority must be documented.
  - Acquisition source and financial basis must be recorded.

- To enter `RECEIVED`:
  - The Vehicle must arrive at an authorized location.
  - Physical receipt evidence must be recorded.
  - Vehicle identity must be checked.

- To enter `INSPECTION_PENDING`:
  - The Vehicle must require initial, quality, mechanical, ownership, or compliance inspection.

- To enter `PREPARATION_IN_PROGRESS`:
  - Preparation Tasks must exist.
  - Responsible Users or providers must be assigned.
  - Required reconditioning, cleaning, documentation, registration, or photography work must be identified.

- To enter `AVAILABLE`:
  - Vehicle identity must be verified.
  - Required inspections must pass.
  - Required preparation must be complete.
  - Valid pricing must exist.
  - Physical presence must be verified or an authorized future-stock exception must apply.
  - No active block may remain.
  - `ready_for_sale` must be true.

- To enter `RESERVED`:
  - The Vehicle must be commercially available.
  - A valid Customer, Opportunity, reservation reason, and expiry must exist.
  - No conflicting reservation or allocation may exist.
  - The reservation command must acquire the required concurrency lock.

- To enter `ALLOCATED`:
  - A valid Opportunity or Deal must exist.
  - The Vehicle must not be allocated elsewhere.
  - Required management or commercial approvals must pass.
  - Allocation evidence and expiry must be stored.

- To enter `SALE_PENDING`:
  - A Deal must exist.
  - The Vehicle must be allocated to that Deal.
  - Required Quotation, Customer, and commercial references must match.
  - Sale-completion conditions must be tracked.

- To enter `SOLD`:
  - An authoritative Deal status must confirm the sale.
  - The final sale price must be recorded.
  - The Customer, Vehicle, and Deal context must match.
  - Required approvals and contractual conditions must pass.

- To enter `DELIVERY_PENDING`:
  - The Vehicle must be sold.
  - Delivery preparation and required documents must be underway.
  - The Vehicle must not have an unresolved blocking condition.

- To enter `DELIVERED`:
  - Authoritative Vehicle Delivery evidence must exist.
  - The Customer or authorized recipient must be verified.
  - Required Payment, funding, contract, registration, insurance, and handover conditions must pass.
  - `delivered_at` must be populated.

- To enter `TRANSFER_PENDING`:
  - An approved destination branch or dealership must exist.
  - Transfer authorization and logistics must be recorded.
  - No conflicting Deal allocation may remain.

- To enter `TRANSFERRED_OUT`:
  - Physical dispatch and destination acceptance must be confirmed.
  - Transfer documents and timestamps must be stored.
  - The receiving inventory context must be created when required.

- To enter `RETURN_PENDING`:
  - An authorized return reason, destination, and responsible party must exist.
  - Sale, reservation, allocation, and transfer conflicts must be resolved.

- To enter `RETURNED`:
  - Return acceptance evidence must exist.
  - Financial, ownership, and physical transfer records must reconcile.

- To enter `BLOCKED`:
  - A standardized block reason must exist.
  - Commercial activity restrictions must be applied immediately.
  - Active publication, reservation, allocation, sale, or delivery workflows must be paused or reviewed.

- To enter `RETIRED`:
  - The Vehicle must no longer be eligible for active dealership inventory use.
  - Exit reason and authority must be recorded.

- To enter `ARCHIVED`:
  - The record must be delivered, transferred out, returned, or retired.
  - Retention, audit, legal-hold, and dependency checks must pass.

### Exit Conditions

- A record cannot exit `PLANNED` without acquisition intent or retirement justification.
- A record cannot exit `ORDERED` without dispatch, acquisition, return, or cancellation evidence.
- A record cannot exit `IN_TRANSIT` toward receipt without physical arrival evidence.
- A record cannot exit `INSPECTION_PENDING` without an inspection result.
- A record cannot exit `PREPARATION_IN_PROGRESS` toward availability until mandatory preparation Tasks are complete.
- A record cannot exit `AVAILABLE` toward reservation or allocation without atomic conflict checks.
- A reservation cannot exit toward allocation unless the Customer and transaction context remain valid.
- An allocation cannot exit toward sale unless the linked Deal remains valid.
- A record cannot exit `SALE_PENDING` toward `SOLD` without authoritative Deal evidence.
- A record cannot exit `DELIVERY_PENDING` toward `DELIVERED` without authoritative delivery evidence.
- A blocked record cannot return to commercial use until the block is formally cleared.
- A transferred, returned, retired, or archived record cannot return to active inventory; a new Inventory Record must be created when appropriate.
- A delivered Vehicle cannot return to active inventory without a controlled return, cancellation, repurchase, or reacquisition workflow.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Canonical Vehicle identified by `vehicle_id`.
  - Authorized Branch and Location.
  - Valid acquisition, ownership, transfer, or consignment evidence.
  - Applicable pricing, preparation, safety, compliance, and publication rules.
  - Concurrency-safe reservation and allocation services.

- **Consumes:**
  - Vehicle identity and specification data.
  - OEM, supplier, auction, transfer, Trade-In, or acquisition-order information.
  - Physical receipt and location-verification evidence.
  - Inspection, preparation, reconditioning, cleaning, registration, and insurance results.
  - Pricing Rules and approved price changes.
  - Customer, Opportunity, Reservation, Quotation, and Deal context.
  - Market Intelligence and demand indicators.
  - Floor-plan and holding-cost information.
  - Publication-provider events.
  - Vehicle Delivery, transfer, return, and disposal evidence.

- **Produces:**
  - Authoritative Vehicle availability.
  - Current stock location.
  - Reservation and allocation status.
  - Approved Customer-visible pricing context.
  - Inventory-aging and holding-cost context.
  - Sale and test-drive readiness.
  - Publication status across sales channels.
  - Vehicle availability for Lead and Opportunity matching.
  - Deal allocation and delivery eligibility.
  - Inventory-performance and profitability indicators.
  - Historical acquisition, transfer, sale, and exit evidence.

- **Creates:**
  - Inspection Tasks.
  - Preparation and reconditioning Tasks.
  - Location-verification Tasks.
  - Reservation records.
  - Allocation records.
  - Stock-transfer requests.
  - Pricing-review requests.
  - Publication Jobs.
  - Floor-plan monitoring Tasks.
  - Management-review Tasks.
  - Replacement Inventory Records after transfer, return, or reacquisition.

- **Triggers:**
  - Vehicle availability updates.
  - Sales-channel publication and removal.
  - Reservation-expiry monitoring.
  - Allocation-expiry monitoring.
  - Inventory-aging alerts.
  - Floor-plan maturity alerts.
  - Price and markdown review.
  - Stock transfer or sourcing review.
  - Quality, safety, compliance, or legal blocking.
  - Deal and delivery workflows.
  - Customer communication when availability changes.

- **Owned By:**
  - The Dealership identified by `dealership_id`.
  - Operational ownership may be assigned to the User identified by `assigned_inventory_user_id`.
  - Pricing authority remains with authorized Pricing or Management roles.
  - Financial cost authority remains with authorized Finance or Accounting roles.

- **Referenced By:**
  - Vehicle
  - Vehicle Model
  - Lead
  - Qualified Lead
  - Opportunity
  - Appointment
  - Quotation
  - Reservation
  - Deal
  - Trade-In
  - Market Intelligence
  - Pricing Rule
  - Campaign
  - Stock Transfer
  - Vehicle Delivery
  - Floor-Plan Facility
  - Supplier
  - OEM
  - Customer Journey
  - Interaction
  - AI Agent Run
  - Audit Record

- **Supports but Does Not Replace:**
  - Canonical Vehicle identity.
  - Supplier invoice or accounting ledger.
  - Customer Payment evidence.
  - Deal completion.
  - Financial Contract or lender funding.
  - Vehicle Delivery confirmation.
  - Registration or ownership-transfer evidence.

- **Supersedes / Replaced By:**
  - Material ownership, dealership, acquisition-cycle, or inventory-context changes may require a new Inventory Record.
  - The replacement record references the earlier record through `supersedes_inventory_record_id`.
  - Historical sold, delivered, transferred, returned, or retired records remain immutable.

## 8. Domain Events

### Emitted Events

- **InventoryRecordPlanned**  
  Payload: `inventory_record_id`, `vehicle_id`, `branch_id`, `source_type`, `created_at`

- **InventoryVehicleOrdered**  
  Payload: `inventory_record_id`, `acquisition_order_id`, `supplier_id`, `expected_location_id`, `ordered_at`

- **InventoryVehicleInTransit**  
  Payload: `inventory_record_id`, `current_transfer_id`, `origin_location`, `destination_location`, `dispatched_at`

- **InventoryVehicleAcquired**  
  Payload: `inventory_record_id`, `vehicle_id`, `acquisition_source`, `acquisition_date`, `purchase_price_amount`

- **InventoryVehicleReceived**  
  Payload: `inventory_record_id`, `location_id`, `received_by`, `received_at`

- **InventoryInspectionRequested**  
  Payload: `inventory_record_id`, `inspection_types`, `assigned_user_ids`, `requested_at`

- **InventoryInspectionCompleted**  
  Payload: `inventory_record_id`, `inspection_status`, `quality_hold_status`, `completed_at`

- **InventoryPreparationStarted**  
  Payload: `inventory_record_id`, `preparation_requirements`, `started_at`

- **InventoryPreparationCompleted**  
  Payload: `inventory_record_id`, `ready_for_sale`, `ready_for_test_drive`, `completed_at`

- **InventoryVehicleAvailable**  
  Payload: `inventory_record_id`, `vehicle_id`, `branch_id`, `location_id`, `advertised_price_amount`, `available_from`

- **InventoryVehicleReserved**  
  Payload: `inventory_record_id`, `reservation_id`, `reserved_for_customer_id`, `reserved_for_opportunity_id`, `reservation_expires_at`

- **InventoryReservationReleased**  
  Payload: `inventory_record_id`, `reservation_id`, `release_reason`, `released_at`

- **InventoryReservationExpired**  
  Payload: `inventory_record_id`, `reservation_id`, `reservation_expires_at`, `expired_at`

- **InventoryVehicleAllocated**  
  Payload: `inventory_record_id`, `allocated_customer_id`, `allocated_opportunity_id`, `allocated_deal_id`, `allocated_at`

- **InventoryAllocationReleased**  
  Payload: `inventory_record_id`, `allocated_deal_id`, `release_reason`, `released_at`

- **InventoryAllocationExpired**  
  Payload: `inventory_record_id`, `allocated_deal_id`, `allocation_expires_at`, `expired_at`

- **InventorySalePending**  
  Payload: `inventory_record_id`, `allocated_deal_id`, `customer_id`, `sale_pending_at`

- **InventoryVehicleSold**  
  Payload: `inventory_record_id`, `sold_deal_id`, `sold_price_amount`, `sold_at`

- **InventoryDeliveryPending**  
  Payload: `inventory_record_id`, `sold_deal_id`, `ready_for_delivery`, `created_at`

- **InventoryVehicleDelivered**  
  Payload: `inventory_record_id`, `sold_deal_id`, `delivered_at`, `delivery_reference`

- **InventoryTransferRequested**  
  Payload: `inventory_record_id`, `origin_branch_id`, `destination_branch_id`, `current_transfer_id`, `requested_at`

- **InventoryVehicleTransferredOut**  
  Payload: `inventory_record_id`, `current_transfer_id`, `destination_branch_id`, `transfer_out_at`

- **InventoryReturnRequested**  
  Payload: `inventory_record_id`, `return_destination`, `exit_reason`, `requested_at`

- **InventoryVehicleReturned**  
  Payload: `inventory_record_id`, `exit_reference`, `returned_at`

- **InventoryVehicleBlocked**  
  Payload: `inventory_record_id`, `availability_block_reason`, `blocked_at`, `blocked_by`

- **InventoryBlockCleared**  
  Payload: `inventory_record_id`, `previous_block_reason`, `cleared_by`, `cleared_at`

- **InventoryPriceUpdated**  
  Payload: `inventory_record_id`, `previous_price_amount`, `new_price_amount`, `pricing_rule_id`, `updated_at`

- **InventoryPricingReviewRequired**  
  Payload: `inventory_record_id`, `days_in_inventory`, `stock_pressure_score`, `recommended_markdown_amount`

- **InventoryPublicationRequested**  
  Payload: `inventory_record_id`, `channel_ids`, `requested_at`

- **InventoryPublicationUpdated**  
  Payload: `inventory_record_id`, `publication_status`, `published_channel_ids`, `updated_at`

- **InventoryLocationVerified**  
  Payload: `inventory_record_id`, `location_id`, `physical_presence_status`, `verified_by`, `verified_at`

- **InventoryLocationMismatchDetected**  
  Payload: `inventory_record_id`, `expected_location_id`, `observed_location`, `detected_at`

- **InventoryRecordRetired**  
  Payload: `inventory_record_id`, `exit_reason`, `retired_by`, `retired_at`

- **InventoryRecordArchived**  
  Payload: `inventory_record_id`, `archived_by`, `archived_at`

- **InventoryHumanReviewRequired**  
  Payload: `inventory_record_id`, `review_reason`, `priority`, `created_at`

### Consumed Events

- **VehicleCreated**  
  Creates or validates the canonical Vehicle reference.

- **VehicleIdentityUpdated**  
  Revalidates VIN, registration, specification, and Vehicle snapshot fields.

- **VehicleSafetyRecallDetected**  
  Blocks sale, test drive, or delivery when required.

- **AcquisitionOrderApproved**  
  Creates or advances a planned Inventory Record.

- **SupplierDispatchConfirmed**  
  Moves eligible stock into transit.

- **StockTransferDispatched**  
  Updates current location and transfer state.

- **StockTransferReceived**  
  Creates or updates the receiving dealership inventory context.

- **TradeInAcquisitionCompleted**  
  Creates a Trade-In Inventory Record after ownership and payoff conditions pass.

- **InspectionCompleted**  
  Updates inspection and readiness status.

- **PreparationTaskCompleted**  
  Updates preparation completion and sale readiness.

- **PricingRuleUpdated**  
  Recalculates permitted pricing and may require publication updates.

- **MarketPriceMovementDetected**  
  Recalculates market position and may create a pricing review.

- **LeadCreated**  
  Updates demand and inquiry indicators.

- **QualifiedLeadCreated**  
  Updates qualified-demand indicators.

- **OpportunityCreated**  
  Updates active Customer-demand indicators.

- **AppointmentScheduled**  
  Updates showroom or test-drive activity.

- **QuotationIssued**  
  Updates quotation activity and Vehicle demand.

- **ReservationCreated**  
  Attempts an atomic Vehicle reservation.

- **ReservationCancelled**  
  Releases the Vehicle when no other valid hold exists.

- **DealVehicleAllocated**  
  Attempts an atomic Deal allocation.

- **DealCancelled**  
  Releases the allocation or starts a controlled Vehicle-return workflow.

- **DealClosedWon**  
  Moves an eligible Inventory Record toward `SOLD`.

- **VehicleDeliveryCompleted**  
  Moves the Inventory Record to `DELIVERED`.

- **FundingTransactionReversed**  
  May block delivery or reopen the sale exception workflow.

- **FloorPlanMaturityApproaching**  
  Updates stock pressure and creates management review.

- **PublicationProviderEventReceived**  
  Updates sales-channel publication status.

- **LegalHoldApplied**  
  Blocks deletion, transfer, sale, or archival actions when required.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- Permitted Vehicle description.
- Customer-visible feature summary.
- Inventory-condition summary.
- Preparation summary.
- Sales-readiness summary.
- Non-sensitive availability summary.
- Market-position summary.
- Demand and aging summary.
- Publication description.
- Approved sales notes.
- Non-sensitive transfer or sourcing rationale.
- Non-sensitive recommended-action explanation.

### Fields Excluded from Embeddings

- `inventory_record_id`
- `dealership_id`
- `vehicle_id`
- `allocated_customer_id`
- `reserved_for_customer_id`
- `allocated_opportunity_id`
- `allocated_deal_id`
- `sold_deal_id`
- Supplier identifiers
- Floor-plan references
- Exact purchase cost
- Exact landed cost
- Minimum authorized price
- Maximum discount
- Internal gross-profit values
- Margin percentages
- Internal pricing floors
- Customer-specific reservation details
- Secure location or access information
- Ownership documents
- Supplier invoices
- Floor-plan documents
- Restricted acquisition terms
- `acquisition_snapshot`
- `allocation_snapshot`
- `pricing_snapshot`
- `financial_snapshot`
- `exit_snapshot`

> Restricted cost, margin, Customer, supplier, floor-plan, and ownership data must be supplied only through authorized structured context.

### Structured AI Context Fields

Authorized Inventory AI Agents may receive:

- `inventory_type`
- `ownership_type`
- `status`
- `availability_status`
- `vehicle_condition`
- `branch_id`
- `location_type`
- `physical_presence_status`
- `ready_for_sale`
- `ready_for_test_drive`
- `ready_for_delivery`
- `inspection_status`
- `reconditioning_status`
- `publication_status`
- `days_in_inventory`
- `aging_status`
- `demand_score`
- `stock_pressure_score`
- `market_interest_score`
- `reservation_status`
- `reservation_expires_at`
- `allocation_status`
- `allocation_expires_at`
- `floor_plan_status`
- `price_position_percentage`
- Approved Customer-visible Vehicle and pricing data

Restricted Pricing, Finance, or Management Agents may additionally receive:

- Acquisition and landed cost.
- Holding cost.
- Minimum authorized price.
- Maximum discount.
- Estimated gross profit.
- Floor-plan maturity.
- Internal transfer and markdown recommendations.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `inventory_record_id`
- `vehicle_id`
- `vehicle_model_id`
- `branch_id`
- `location_id`
- `status`
- `availability_status`
- `inventory_type`
- `vehicle_condition`
- `ready_for_sale`
- `ready_for_test_drive`
- `publication_status`
- `aging_status`
- `is_current_record`

### Confidence Thresholds

- VIN extraction or matching requires confidence of at least `0.995`.
- Vehicle make, model, trim, and model-year matching requires confidence of at least `0.95`.
- Location extraction requires confidence of at least `0.95`.
- Physical-presence recognition requires confidence of at least `0.99` plus authorized evidence.
- Price extraction requires confidence of at least `0.99`.
- Cost-document extraction requires confidence of at least `0.995`.
- Damage or quality-hold classification requires confidence of at least `0.95`.
- Reservation or allocation-intent detection requires confidence of at least `0.95`.
- Vehicle-delivery confirmation requires authoritative evidence and cannot rely on AI confidence.
- AI repricing or transfer recommendations require composite confidence of at least `0.85`.
- No AI confidence score may authorize sale, delivery, allocation, reservation override, cost change, or block removal.

### Human Approval Thresholds

- AI Agents cannot change authoritative Vehicle identity.
- AI Agents cannot create or change acquisition cost without authoritative evidence.
- AI Agents cannot approve a sale below the permitted price.
- AI Agents cannot override an active reservation or Deal allocation.
- AI Agents cannot mark a Vehicle sold or delivered.
- AI Agents cannot remove safety, legal, ownership, fraud, compliance, financial, or quality blocks.
- AI Agents cannot authorize a stock transfer, return, retirement, or write-off.
- Conflicting VIN, location, ownership, price, reservation, allocation, Deal, or delivery evidence must create a Human Review Task.
- Pricing, markdown, transfer, sourcing, and publication recommendations remain advisory until authorized.

### AI Recommendation Rules

- Recommendations must identify:
  - Supporting data.
  - Formula or model version.
  - Assumptions.
  - Confidence.
  - Expected commercial impact.
  - Required approval role.

- AI must distinguish:
  - Physical availability.
  - Commercial availability.
  - Reservation.
  - Deal allocation.
  - Sale.
  - Delivery.

- AI must not represent advertised price as final Deal price.
- AI must not represent demand score as guaranteed future demand.
- AI must not expose restricted costs or margins to unauthorized Users.
- Every AI recommendation must link to the exact Inventory Record version.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/inventory-records`

### Methods

- `GET` — list or search Inventory Records.
- `POST` — create a planned, ordered, acquired, received, or imported Inventory Record.
- `GET /{id}` — retrieve one permitted Inventory Record.
- `PATCH /{id}` — update permitted fields according to lifecycle state.
- `POST /{id}/receive` — record authoritative Vehicle receipt.
- `POST /{id}/request-inspection` — create required inspection Tasks.
- `POST /{id}/record-inspection` — process authorized inspection results.
- `POST /{id}/start-preparation` — begin preparation or reconditioning.
- `POST /{id}/complete-preparation` — complete readiness checks.
- `POST /{id}/make-available` — make an eligible Vehicle commercially available.
- `POST /{id}/reserve` — atomically reserve the Vehicle.
- `POST /{id}/release-reservation` — release an eligible reservation.
- `POST /{id}/allocate` — atomically allocate the Vehicle to an Opportunity or Deal.
- `POST /{id}/release-allocation` — release an eligible allocation.
- `POST /{id}/mark-sale-pending` — link the Vehicle to an active Deal.
- `POST /{id}/mark-sold` — process authoritative Deal sale evidence.
- `POST /{id}/mark-delivery-pending` — begin delivery preparation.
- `POST /{id}/mark-delivered` — process authoritative Vehicle Delivery evidence.
- `POST /{id}/request-transfer` — create a stock-transfer request.
- `POST /{id}/complete-transfer` — process transfer-out evidence.
- `POST /{id}/request-return` — begin an authorized return.
- `POST /{id}/complete-return` — process authoritative return evidence.
- `POST /{id}/block` — apply an authorized inventory block.
- `POST /{id}/clear-block` — clear an eligible block.
- `POST /{id}/update-price` — update pricing through an authorized pricing workflow.
- `POST /{id}/publish` — publish approved Customer-visible inventory data.
- `POST /{id}/unpublish` — remove the Vehicle from selected sales channels.
- `POST /{id}/verify-location` — record physical-location verification.
- `POST /{id}/retire` — retire an eligible Inventory Record.
- `POST /{id}/archive` — archive an eligible terminal record.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateInventoryRecordRequest",
  "type": "object",
  "properties": {
    "vehicle_id": {
      "type": "string",
      "format": "uuid"
    },
    "vehicle_model_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "branch_id": {
      "type": "string",
      "format": "uuid"
    },
    "location_id": {
      "type": "string",
      "format": "uuid"
    },
    "supplier_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "oem_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "acquisition_order_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "trade_in_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "inventory_type": {
      "type": "string",
      "enum": [
        "RETAIL_STOCK",
        "DEMONSTRATOR",
        "TEST_DRIVE",
        "LOANER",
        "DISPLAY",
        "FLEET",
        "WHOLESALE",
        "CONSIGNMENT",
        "TRADE_IN_STOCK",
        "CERTIFIED_PRE_OWNED",
        "SERVICE_REPLACEMENT",
        "NON_RETAIL",
        "OTHER"
      ]
    },
    "ownership_type": {
      "type": "string",
      "enum": [
        "DEALERSHIP_OWNED",
        "FLOOR_PLAN_FINANCED",
        "OEM_OWNED",
        "SUPPLIER_OWNED",
        "CONSIGNMENT",
        "CUSTOMER_OWNED",
        "LEASED",
        "DEMONSTRATION_LOAN",
        "OTHER"
      ]
    },
    "vehicle_condition": {
      "type": "string",
      "enum": [
        "NEW",
        "USED",
        "CERTIFIED_PRE_OWNED",
        "DEMONSTRATOR",
        "DAMAGED",
        "RECONDITIONED",
        "SALVAGE",
        "UNKNOWN"
      ]
    },
    "acquisition_source": {
      "type": "string",
      "enum": [
        "OEM_ALLOCATION",
        "OEM_ORDER",
        "DEALER_TRANSFER",
        "SUPPLIER_PURCHASE",
        "AUCTION",
        "TRADE_IN",
        "CUSTOMER_BUYBACK",
        "FLEET_RETURN",
        "LEASE_RETURN",
        "CONSIGNMENT",
        "IMPORT",
        "MANUAL_ENTRY",
        "OTHER"
      ]
    },
    "acquisition_date": {
      "type": "string",
      "format": "date"
    },
    "purchase_price_amount": {
      "type": "number",
      "minimum": 0
    },
    "transport_cost_amount": {
      "type": "number",
      "minimum": 0
    },
    "inspection_cost_amount": {
      "type": "number",
      "minimum": 0
    },
    "reconditioning_cost_amount": {
      "type": "number",
      "minimum": 0
    },
    "other_acquisition_cost_amount": {
      "type": "number",
      "minimum": 0
    },
    "location_type": {
      "type": "string",
      "enum": [
        "SHOWROOM",
        "WAREHOUSE",
        "PARKING_YARD",
        "TRANSIT_HUB",
        "SERVICE_CENTER",
        "BODY_SHOP",
        "DETAILING_CENTER",
        "TEST_DRIVE_ZONE",
        "CUSTOMER_DELIVERY_AREA",
        "EXTERNAL_STORAGE",
        "SUPPLIER_LOCATION",
        "IN_TRANSIT",
        "OTHER"
      ]
    },
    "currency_code": {
      "type": "string",
      "pattern": "^[A-Z]{3}$"
    },
    "list_price_amount": {
      "type": "number",
      "minimum": 0
    },
    "advertised_price_amount": {
      "type": "number",
      "minimum": 0
    },
    "minimum_authorized_price_amount": {
      "type": "number",
      "minimum": 0
    },
    "maximum_discount_amount": {
      "type": "number",
      "minimum": 0
    },
    "assigned_inventory_user_id": {
      "type": ["string", "null"],
      "format": "uuid"
    }
  },
  "required": [
    "vehicle_id",
    "branch_id",
    "location_id",
    "inventory_type",
    "ownership_type",
    "vehicle_condition",
    "acquisition_source",
    "acquisition_date",
    "purchase_price_amount",
    "transport_cost_amount",
    "inspection_cost_amount",
    "reconditioning_cost_amount",
    "other_acquisition_cost_amount",
    "location_type",
    "currency_code",
    "list_price_amount",
    "advertised_price_amount",
    "minimum_authorized_price_amount",
    "maximum_discount_amount"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type InventoryRecord {
  id: ID!
  dealershipId: ID!
  vehicleId: ID!
  vehicleModelId: ID
  branchId: ID!
  locationId: ID!
  supplierId: ID
  oemId: ID
  acquisitionOrderId: ID
  tradeInId: ID
  reservationId: ID
  allocatedCustomerId: ID
  allocatedOpportunityId: ID
  allocatedDealId: ID
  pricingRuleId: ID
  floorPlanProviderId: ID
  currentTransferId: ID
  assignedInventoryUserId: ID
  supersedesInventoryRecordId: ID
  inventoryNumber: String!
  stockNumber: String!
  inventoryType: InventoryType!
  ownershipType: InventoryOwnershipType!
  status: InventoryRecordStatus!
  availabilityStatus: InventoryAvailabilityStatus!
  vehicleCondition: InventoryCondition!
  vin: String
  make: String!
  model: String!
  trim: String
  modelYear: Int
  acquisitionSource: InventoryAcquisitionSource!
  acquisitionDate: Date!
  purchasePriceAmount: Float!
  landedCostAmount: Float!
  branchName: String
  locationType: InventoryLocationType!
  locationName: String
  physicalPresenceStatus: PhysicalPresenceStatus!
  currencyCode: String!
  listPriceAmount: Float!
  advertisedPriceAmount: Float!
  minimumAuthorizedPriceAmount: Float!
  currentDiscountAmount: Float!
  maximumDiscountAmount: Float!
  estimatedGrossProfitAmount: Float
  estimatedGrossMarginPercentage: Float
  reservationStatus: InventoryReservationStatus!
  reservedForCustomerId: ID
  reservationExpiresAt: DateTime
  allocationStatus: InventoryAllocationStatus!
  allocationExpiresAt: DateTime
  inspectionStatus: InventoryPreparationStatus!
  preDeliveryInspectionStatus: InventoryPreparationStatus!
  reconditioningStatus: InventoryPreparationStatus!
  cleaningStatus: InventoryPreparationStatus!
  photographyStatus: InventoryPreparationStatus!
  documentationStatus: InventoryPreparationStatus!
  registrationStatus: InventoryPreparationStatus!
  insuranceStatus: InventoryPreparationStatus!
  readyForSale: Boolean!
  readyForTestDrive: Boolean!
  readyForDelivery: Boolean!
  publicationStatus: InventoryPublicationStatus!
  publishedChannelIds: [ID!]!
  daysInInventory: Int!
  agingStatus: InventoryAgingStatus!
  demandScore: Float
  stockPressureScore: Float
  marketInterestScore: Float
  floorPlanStatus: FloorPlanStatus
  soldDealId: ID
  soldPriceAmount: Float
  soldAt: DateTime
  deliveredAt: DateTime
  transferOutAt: DateTime
  returnedAt: DateTime
  retiredAt: DateTime
  inventoryVersion: Int!
  isCurrentRecord: Boolean!
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `inventory_records`
- **Acquisition Table:** `inventory_acquisitions`
- **Location Table:** `inventory_locations`
- **Location-History Table:** `inventory_location_history`
- **Reservation Table:** `inventory_reservations`
- **Allocation Table:** `inventory_allocations`
- **Pricing Table:** `inventory_pricing`
- **Preparation Table:** `inventory_preparation`
- **Publication Table:** `inventory_publications`
- **Floor-Plan Table:** `inventory_floor_plan`
- **Activity Table:** `inventory_activity_metrics`
- **Transfer Table:** `inventory_transfers`
- **Exit Table:** `inventory_exits`
- **Version-History Table:** `inventory_record_versions`
- **Status-History Table:** `inventory_status_history`
- **Audit Table:** `inventory_audit_log`

### Indexes

- `idx_inventory_tenant_status (dealership_id, status)`  
  Used for inventory operational queues.

- `idx_inventory_vehicle_current (dealership_id, vehicle_id, is_current_record)`  
  Used to retrieve the active inventory context.

- `idx_inventory_branch_availability (dealership_id, branch_id, availability_status)`  
  Used for branch-level Vehicle searches.

- `idx_inventory_location_status (dealership_id, location_id, status)`  
  Used for physical stock management.

- `idx_inventory_vehicle_model (dealership_id, vehicle_model_id, availability_status)`  
  Used for model-level Vehicle matching.

- `idx_inventory_reservation_expiry (dealership_id, reservation_status, reservation_expires_at)`  
  Used by reservation-expiry Jobs.

- `idx_inventory_allocation_expiry (dealership_id, allocation_status, allocation_expires_at)`  
  Used by allocation-expiry Jobs.

- `idx_inventory_deal_allocation (dealership_id, allocated_deal_id, allocation_status)`  
  Used for Deal allocation checks.

- `idx_inventory_pricing (dealership_id, advertised_price_amount, availability_status)`  
  Used for Customer Vehicle searches and pricing analysis.

- `idx_inventory_aging (dealership_id, aging_status, days_in_inventory)`  
  Used for aging and markdown queues.

- `idx_inventory_stock_pressure (dealership_id, stock_pressure_score DESC)`  
  Used for management prioritization.

- `idx_inventory_floor_plan (dealership_id, floor_plan_status, floor_plan_maturity_date)`  
  Used for financing-cost monitoring.

- `idx_inventory_publication (dealership_id, publication_status, last_publication_sync_at)`  
  Used for publication reconciliation.

- `idx_inventory_stock_number (dealership_id, stock_number)`  
  Used for operational stock lookup.

- `idx_inventory_vin (vin)`  
  Used for Vehicle identity and duplicate detection.

- `idx_inventory_location_verification (dealership_id, physical_presence_status, last_location_verified_at)`  
  Used for physical inventory audits.

### Unique Constraints

- `UQ_inventory_number (dealership_id, inventory_number)`

- `UQ_active_stock_number (dealership_id, stock_number, is_current_record)`

- `UQ_current_vehicle_inventory (dealership_id, vehicle_id) WHERE is_current_record = true`

- `UQ_active_vehicle_reservation (dealership_id, vehicle_id) WHERE reservation_status = 'ACTIVE'`

- `UQ_active_vehicle_allocation (dealership_id, vehicle_id) WHERE allocation_status = 'ALLOCATED'`

- `UQ_allocated_deal_vehicle (dealership_id, allocated_deal_id)`  
  Applies when only one Vehicle may be allocated to a Deal.

- `UQ_inventory_series_version (dealership_id, vehicle_id, inventory_version)`

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `vehicle_id` → `vehicles(id)`
- `vehicle_model_id` → `vehicle_models(id)` — nullable
- `branch_id` → `branches(id)`
- `location_id` → `inventory_locations(id)`
- `supplier_id` → `suppliers(id)` — nullable
- `oem_id` → `oems(id)` — nullable
- `acquisition_order_id` → `acquisition_orders(id)` — nullable
- `trade_in_id` → `trade_ins(id)` — nullable
- `reservation_id` → `reservations(id)` — nullable
- `reserved_for_customer_id` → `customers(id)` — nullable
- `reserved_for_opportunity_id` → `opportunities(id)` — nullable
- `allocated_customer_id` → `customers(id)` — nullable
- `allocated_opportunity_id` → `opportunities(id)` — nullable
- `allocated_deal_id` → `deals(id)` — nullable
- `sold_deal_id` → `deals(id)` — nullable
- `pricing_rule_id` → `pricing_rules(id)` — nullable
- `floor_plan_provider_id` → `finance_providers(id)` — nullable
- `current_transfer_id` → `inventory_transfers(id)` — nullable
- `assigned_inventory_user_id` → `users(id)` — nullable
- `supersedes_inventory_record_id` → `inventory_records(id)` — nullable
- `created_by` → `users(id)`
- `reserved_by` → `users(id)` — nullable
- `allocated_by` → `users(id)` — nullable
- `sold_by` → `users(id)` — nullable
- `retired_by` → `users(id)` — nullable

### Database Constraints

- `inventory_version >= 1`
- Financial amounts must be zero or greater unless explicitly defined as signed adjustments.
- `minimum_authorized_price_amount <= list_price_amount`
- `current_discount_amount <= maximum_discount_amount`
- `reservation_expires_at > reserved_at`
- `allocation_expires_at > allocated_at`
- `floor_plan_maturity_date > floor_plan_start_date`
- `delivered_at >= sold_at`
- `sold_at >= acquisition_date`
- Scores must remain between `0.00` and `1.00`.
- Counts and `days_in_inventory` must be zero or greater.
- `reservation_id IS NOT NULL` when reservation status is `ACTIVE`.
- `reserved_for_customer_id IS NOT NULL` for Customer-specific reservations.
- `allocated_deal_id IS NOT NULL` when allocation status is `ALLOCATED`.
- `sold_deal_id`, `sold_at`, and `sold_price_amount` must be populated when status is `SOLD`.
- `availability_block_reason IS NOT NULL` when status is `BLOCKED`.
- `exit_reason IS NOT NULL` for transferred, returned, retired, or archived exits.
- `last_location_verified_at IS NOT NULL` when physical presence is `VERIFIED_PRESENT`.
- Historical delivered, transferred, returned, retired, and archived snapshots must be immutable.
- Circular supersession relationships are prohibited.

### Storage Strategy

- Store primary operational state in relational tables.
- Store acquisition, pricing, financial, allocation, and exit snapshots using field-level encryption.
- Store inspection images, invoices, ownership documents, location evidence, and preparation documents in the encrypted Document Vault.
- Store cryptographic hashes for authoritative documents and evidence.
- Preserve location, pricing, reservation, allocation, publication, and status history as append-only records.
- Search indexes must expose only approved Customer-visible fields.
- Restricted cost, margin, floor-plan, and ownership data must not enter public search indexes.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical records by `created_at` or `acquisition_date`.
- Reservation, allocation, location, pricing, publication, activity, transfer, history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read access to permitted Customer-visible availability and pricing for assigned Customers and Opportunities.
- **Sales Manager:** Access to reservations, allocations, stock status, and permitted pricing approvals.
- **Inventory User:** Create, receive, inspect, prepare, locate, transfer, and maintain Inventory Records within assigned scope.
- **Inventory Manager:** Full operational inventory oversight, transfer approval, aging review, and authorized block clearance.
- **Pricing Manager:** Access to restricted pricing, markdown, price-floor, and commercial-review workflows.
- **Finance or Accounting User:** Access to acquisition cost, landed cost, floor-plan, holding-cost, and reconciliation information.
- **Trade-In Appraiser:** Access to Trade-In acquisition and used-Vehicle preparation context.
- **Delivery Coordinator:** Access to sold Vehicle, delivery-readiness, location, and handover status.
- **Compliance User:** Access to legal hold, ownership, safety, fraud, quality, and compliance blocks.
- **Marketing User:** Access only to approved published Vehicle data and Campaign eligibility.
- **Customer Self-Service User:** Access only to Customer-visible availability, reservation, and approved pricing information.
- **AI Inventory Agent:** Service Account access limited to matching, summarization, monitoring, recommendations, and approved workflow requests.
- **Publication Integration Service:** Restricted access to approved Customer-visible inventory fields and provider events.
- **Audit Service:** Read-only access to immutable inventory history and evidence metadata.

### Data Classification

- **Default Level:** `CONFIDENTIAL COMMERCIAL DATA`
- **Elevated Categories:**
  - `RESTRICTED FINANCIAL DATA`
  - `CUSTOMER PII`
  - `OWNERSHIP EVIDENCE`
  - `SECURITY-SENSITIVE LOCATION DATA`
  - `LEGAL OR COMPLIANCE RESTRICTED`

### Commercially Sensitive Fields

- `purchase_price_amount`
- `landed_cost_amount`
- `minimum_authorized_price_amount`
- `maximum_discount_amount`
- `estimated_gross_profit_amount`
- `estimated_gross_margin_percentage`
- `floor_plan_principal_amount`
- `floor_plan_interest_rate`
- `floor_plan_daily_cost_amount`
- `floor_plan_accrued_cost_amount`
- `financial_holding_cost_amount`
- Internal markdown recommendations
- Supplier and acquisition terms
- Internal pricing rules

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, documents, snapshots, event stores, and backups.
- **Column-Level Protection:** Acquisition cost, landed cost, restricted pricing, margin, floor-plan data, Customer reservation data, supplier references, ownership data, and exit information require encryption, tokenization, or equivalent approved protection.
- Ownership documents, supplier invoices, inspection evidence, floor-plan documents, and transfer documents must be stored in an encrypted Document Vault.
- Provider credentials, API keys, publication tokens, and access secrets must be stored in a secrets-management system.
- Encryption keys must be separated by environment and rotated according to security policy.

### Availability and Concurrency Security

- Reservation, allocation, sale, and transfer commands must use atomic concurrency controls.
- Duplicate active reservations and allocations are prohibited.
- Commands must validate the current `record_version`.
- Expired or released holds must not continue blocking availability.
- Publication channels must receive availability only from the authoritative Inventory Record.
- Public APIs must never expose restricted price floors, cost, margin, Customer identity, or floor-plan details.
- Secure storage locations and access instructions must be restricted to authorized operational Users.

### Audit Requirements

- Every acquisition operation must preserve:
  - Acquisition source.
  - Supplier or source.
  - Vehicle.
  - Ownership basis.
  - Financial evidence.
  - Actor.
  - Timestamp.

- Every location operation must preserve:
  - Previous location.
  - New location.
  - Physical-presence evidence.
  - Verifier.
  - Timestamp.

- Every pricing operation must preserve:
  - Previous price.
  - New price.
  - Price rule.
  - Approval authority.
  - Reason.
  - Effective period.
  - Timestamp.

- Every reservation and allocation operation must preserve:
  - Customer.
  - Opportunity or Deal.
  - Previous availability.
  - New availability.
  - Expiry.
  - Actor or service.
  - Idempotency key.
  - Timestamp.

- Every publication operation must preserve:
  - Channel.
  - Customer-visible payload hash.
  - Provider reference.
  - Processing result.
  - Actor or service.
  - Timestamp.

- Every sale and delivery operation must preserve:
  - Deal.
  - Customer.
  - Vehicle.
  - Final price.
  - Required approval evidence.
  - Delivery evidence.
  - Actor.
  - Timestamp.

- Every block and block-clearance operation must preserve:
  - Reason.
  - Supporting evidence.
  - Authorized role.
  - Previous and new status.
  - Timestamp.

- Every AI operation must preserve:
  - Model reference.
  - Prompt version.
  - Authorized input scope.
  - Recommendation.
  - Confidence.
  - Human-review status.
  - Timestamp.

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to cost, margin, ownership, location, Customer reservation, floor-plan, and legal information must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Vehicle, Branch, Location, Customer, Opportunity, Deal, Trade-In, Reservation, Pricing Rule, supplier, transfer, or Inventory Record linking is prohibited.
- A stock transfer between dealerships must use an explicit controlled transfer workflow and separate sending and receiving Inventory Records.
- AI Agents, Jobs, integrations, publication services, exports, analytics, and search retrieval must receive tenant scope through signed execution context.
- Publication-provider events must map to exactly one tenant and Inventory Record.
- Customer-facing search and reservation links must be tenant-scoped, time-limited, and permission-validated.

### Retention and Deletion

- Inventory Records must follow commercial, accounting, tax, ownership, safety, contractual, privacy, audit, and legal-retention requirements.
- Sold, delivered, transferred, returned, retired, and archived Inventory Records must remain immutable.
- Hard deletion is prohibited while an Inventory Record is linked to:
  - A Vehicle
  - A Trade-In
  - A Reservation
  - An Opportunity
  - A Quotation
  - A Deal
  - A Payment
  - A Vehicle Delivery
  - A Stock Transfer
  - A Floor-Plan record
  - A legal hold
  - An audit record

- Legal hold overrides ordinary deletion and archival schedules.
- Soft deletion is permitted only for eligible duplicate, erroneous, or abandoned records that have not been commercially relied upon.
- Approved deletion or anonymization must preserve legally required accounting and transaction evidence.
- Deletion and anonymization must address:
  - Inventory Record data
  - Acquisition and ownership documents
  - Location history
  - Reservation and allocation records
  - Pricing history
  - Financial and floor-plan data
  - Inspection and preparation evidence
  - Publication records
  - Activity metrics
  - AI analysis and embeddings
  - Search indexes
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy

### Terminal States

- **DELIVERED:** The Vehicle was handed over under an authoritative delivery process.
- **TRANSFERRED_OUT:** The Vehicle left this dealership’s inventory context.
- **RETURNED:** The Vehicle was returned to its authorized source or owner.
- **RETIRED:** The stock record was permanently removed from active inventory.
- **ARCHIVED:** The historical Inventory Record moved to long-term retention.
