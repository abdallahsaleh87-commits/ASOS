# Vehicle

## 1. Object Purpose

### Business Purpose

The Vehicle object represents the physical automotive asset and primary capital investment of the dealership. It acts as the core product entity being tracked, marketed, and sold. This object provides the foundation for inventory management, capital efficiency metrics (turnover, aging), and revenue generation.

### System Purpose

The Vehicle object is a master inventory record in Layer 1. It structurally decouples physical assets from transactions, allowing a single VIN to exist independently until mathematically bound to a Customer via a Deal. It serves as the primary query target for the Inventory Agent, providing semantic and structured data for exact customer-to-vehicle matching, margin calculations, and API syndication.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `vehicle_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:** `current_deal_id` (UUIDv4 — optional; populated when the vehicle is reserved or sold)

### Data Payload

- **Required Fields:** `vin`, `stock_number`, `condition`, `make`, `model`, `year`, `mileage`, `status`
- **Optional Fields:** `trim`, `exterior_color`, `interior_color`, `transmission`, `drivetrain`, `fuel_type`, `engine_specs`, `factory_options`
- **Financial Fields:** `base_cost`, `retail_price`, `minimum_allowed_price`
- **Computed Fields:** `days_in_stock`, `current_gross_margin_potential`, `age_penalty_score`

### Governance & Lifecycle

- **Metadata:** `dms_reference_id`, `oem_build_sheet_url`, `arrival_date`
- **Audit Fields:** `created_by`, `updated_by`, `last_appraised_at`
- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** Dealership entity; no individual user ownership
- **Timestamps:** `created_at`, `updated_at`, `status_changed_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| vehicle_id | UUID | Unique canonical identifier for the vehicle. | Yes | Auto-generated | Must use a valid UUIDv4 format | 550e8400-e29b-41d4-a716-446655440000 | N/A |
| dealership_id | UUID | Identifies the dealership tenant that owns the vehicle. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| vin | String | Vehicle Identification Number. | Yes | N/A | Exactly 17 characters; must exclude I, O, and Q and pass VIN validation | 1HGCM82664A123456 | At least 0.99 |
| stock_number | String | Internal dealership or DMS inventory number. | Yes | N/A | Must follow dealership-specific format | N24015A | At least 0.95 |
| condition | Enum | Classifies the vehicle's commercial condition. | Yes | NEW | Must match VehicleCondition ENUM | USED | At least 0.99 |
| make | String | Vehicle manufacturer or brand. | Yes | N/A | Must match the approved OEM dictionary | Honda | At least 0.90 |
| model | String | Manufacturer's vehicle model. | Yes | N/A | Must belong to the selected make | Civic | At least 0.90 |
| year | Integer | Vehicle model year. | Yes | N/A | Between 1980 and the current year plus one | 2026 | At least 0.99 |
| mileage | Integer | Current odometer reading. | Yes | 0 | Must be zero or greater | 12500 | At least 0.95 |
| base_cost | Decimal | Dealership acquisition or invoice cost. | Yes | 0.00 | Must be zero or greater with two decimal places | 28500.00 | DMS sync only |
| retail_price | Decimal | Current advertised or asking price. | Yes | 0.00 | Must be zero or greater and normally not below base cost | 32000.00 | DMS or pricing-rule sync |
| minimum_allowed_price | Decimal | Lowest price permitted without additional approval. | No | 0.00 | Must be zero or greater and not exceed retail price | 30000.00 | Human or pricing-engine input |
| status | Enum | Current inventory and sales state. | Yes | IN_TRANSIT | Must match VehicleStatus ENUM | AVAILABLE | At least 0.99 |
| current_deal_id | UUID | Deal currently reserving or purchasing the vehicle. | No | Null | Required when status is RESERVED | 123e4567-e89b-12d3-a456-426614174000 | System-controlled |
| days_in_stock | Integer | Number of days the vehicle has been held as active stock. | No | 0 | Must be system-computed and zero or greater | 42 | System-computed |

## 4. Enumerations

### VehicleCondition

- **NEW:** Never titled and supplied directly by the manufacturer.
- **USED:** Previously titled or owned.
- **CPO:** Certified Pre-Owned and approved under a defined inspection program.
- **DEMO:** Dealership-owned demonstration or test-drive vehicle.

### VehicleStatus

- **ORDERED:** Allocated or ordered from the manufacturer.
- **IN_TRANSIT:** Built and being transported to the dealership.
- **IN_RECON:** Undergoing inspection, repair, detailing, or preparation; not currently sellable.
- **AVAILABLE:** Ready for sale and available for customer matching.
- **RESERVED:** Linked to an active Deal or secured by a deposit.
- **SOLD:** Delivered to the customer.
- **WHOLESALE:** Removed from retail inventory and sent to auction or wholesale.

### FuelType

- GASOLINE
- DIESEL
- HYBRID
- PHEV
- BEV
- HYDROGEN

### Transmission

- AUTOMATIC
- MANUAL
- CVT
- DCT

## 5. Validation Rules

### Business Rules

- A Vehicle with `status = IN_RECON` cannot be linked to a delivery operation.
- `retail_price` cannot fall below `minimum_allowed_price` without explicit Sales Manager or GSM approval.
- `days_in_stock` begins when the Vehicle first enters `IN_RECON` or `AVAILABLE`.
- A Vehicle cannot be assigned to more than one active Deal simultaneously.

### Technical Rules

- `vin` must pass a valid VIN structure and checksum verification before database commit.
- `record_version` must increase after every successful update.
- `base_cost` and `minimum_allowed_price` must be hidden from customer-facing AI Agents.

### Data Constraints

- `vin` must be globally unique across the complete system.
- `stock_number + dealership_id` must be unique.
- `mileage`, `base_cost`, `retail_price`, and `minimum_allowed_price` cannot be negative.

### Referential Integrity

- The Vehicle cannot transition to `RESERVED` unless a valid `current_deal_id` is supplied.
- `current_deal_id` must reference a Deal belonging to the same `dealership_id`.
- A Vehicle linked to a non-terminal Deal cannot be hard-deleted.

### Human Approval Requirements

- Moving a Vehicle manually from `SOLD` back to `AVAILABLE` requires Finance Manager or Sales Manager approval.
- Reducing `retail_price` below `minimum_allowed_price` requires documented human approval.

## 6. State Machine

### Allowed States

- ORDERED
- IN_TRANSIT
- IN_RECON
- AVAILABLE
- RESERVED
- SOLD
- WHOLESALE

### Allowed Transitions

- ORDERED → IN_TRANSIT
- IN_TRANSIT → IN_RECON
- IN_RECON → AVAILABLE
- AVAILABLE → RESERVED
- RESERVED → SOLD
- RESERVED → AVAILABLE
- AVAILABLE → WHOLESALE

### Forbidden Transitions

- SOLD → RESERVED
- ORDERED → SOLD
- IN_RECON → SOLD
- WHOLESALE → RESERVED

### Entry Conditions

- To enter `AVAILABLE`, the inspection and preparation checklist must be completed.
- To enter `RESERVED`, the Vehicle must have a valid active Deal or confirmed deposit.
- To enter `SOLD`, the linked Deal must have completed all required approvals.

### Exit Conditions

- To move from `AVAILABLE` to `RESERVED`, a valid `current_deal_id` must exist.
- To move from `RESERVED` back to `AVAILABLE`, the linked Deal must be cancelled, rejected, or expired.
- To move from `RESERVED` to `SOLD`, the linked Deal must reach its successful terminal state.

### Terminal States

- **SOLD:** The Vehicle has left the dealership's active inventory.
- **WHOLESALE:** The Vehicle has been liquidated outside the retail sales process.

## 7. Relationships

- **Depends On:** Tenant identified by `dealership_id` and the Layer 1 DMS synchronization.
- **Consumes:** OEM manifests for vehicle build sheets and appraisal data when `condition = USED`.
- **Produces:** Inventory aging scores and capital-efficiency metrics.
- **Creates:** Vehicle Memory containing service history and days-in-stock logs.
- **Triggers:** `VehicleStatusChanged` and `PriceDropAlert` events.
- **Owned By:** The Dealership entity.
- **Referenced By:** Deal, Quotation, Test-Drive Appointment, and Trade-in records.

## 8. Domain Events

### Emitted Events

- **VehicleAddedToInventory**  
  Payload: `vehicle_id`, `vin`, `retail_price`, `condition`

- **VehicleStatusChanged**  
  Payload: `vehicle_id`, `old_status`, `new_status`, `timestamp`

- **VehiclePriceUpdated**  
  Payload: `vehicle_id`, `old_price`, `new_price`  
  Triggers the AI Brain to notify customers watching the relevant vehicle segment.

- **VehicleAgedOut**  
  Payload: `vehicle_id`, `days_in_stock`  
  Triggers the inventory-liquidation playbook.

### Consumed Events

- **DMSInventorySynced**  
  Triggers batch updates of vehicle statuses and mileage.

- **DealWon**  
  Triggers the Vehicle transition to `SOLD`.

- **DealUnwound**  
  Triggers the Vehicle rollback to `AVAILABLE`.

## 9. AI Considerations

### Fields Used for Vector Embeddings — Semantic Search

- `make`
- `model`
- `trim`
- `exterior_color`
- `interior_color`
- `factory_options`
- `vehicle_type`

> These fields allow the Context Layer to match natural-language requirements such as “I need a family car with leather seats” to a specific available VIN.

### Fields Excluded from Embeddings — Protected Financial Data

- `base_cost`
- `minimum_allowed_price`
- `dms_reference_id`

> Excluding these fields prevents customer-facing AI Agents from exposing dealership margins or internal inventory references.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every query.
- `status` — defaults to `AVAILABLE` or `IN_TRANSIT`.
- `condition`

### Confidence Thresholds

- Matching a Customer requirement to a specific VIN requires a semantic-similarity score of at least `0.85`.

### Human Approval Thresholds

- AI Agents cannot directly update `retail_price`.
- The Agent may only create a price-change recommendation task for a Sales Manager to approve.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/vehicles`
- **Methods:**
  - `GET` — list or search vehicles using filters.
  - `POST` — ingest a Vehicle record.
  - `GET /{id}` — retrieve one Vehicle.
  - `PATCH /{id}` — update status, mileage, or permitted pricing fields.
  - `DELETE /{id}` — perform a soft delete.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "IngestVehicleRequest",
  "type": "object",
  "properties": {
    "vin": {
      "type": "string",
      "minLength": 17,
      "maxLength": 17
    },
    "stock_number": {
      "type": "string",
      "maxLength": 50
    },
    "condition": {
      "type": "string",
      "enum": ["NEW", "USED", "CPO", "DEMO"]
    },
    "make": {
      "type": "string"
    },
    "model": {
      "type": "string"
    },
    "year": {
      "type": "integer"
    },
    "mileage": {
      "type": "integer",
      "minimum": 0
    },
    "status": {
      "type": "string",
      "enum": ["ORDERED", "IN_TRANSIT", "IN_RECON", "AVAILABLE"]
    },
    "base_cost": {
      "type": "number",
      "minimum": 0
    },
    "retail_price": {
      "type": "number",
      "minimum": 0
    }
  },
  "required": [
    "vin",
    "stock_number",
    "condition",
    "make",
    "model",
    "year",
    "mileage",
    "status"
  ]
}
```

### GraphQL Type

```graphql
type Vehicle {
  id: ID!
  dealershipId: ID!
  vin: String!
  stockNumber: String!
  condition: VehicleCondition!
  make: String!
  model: String!
  year: Int!
  mileage: Int!
  status: VehicleStatus!
  retailPrice: Float!
  daysInStock: Int!
  factoryOptions: [String!]
  currentDeal: Deal
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Table Name:** `vehicles`

### Indexes

- `idx_vehicles_tenant_status (dealership_id, status)`  
  Used heavily for available-inventory searches.

- `idx_vehicles_vin (vin)`  
  Global unique VIN lookup.

- `idx_vehicles_make_model (dealership_id, make, model)`  
  Used by customer-to-vehicle matching queries.

### Unique Constraints

- `UQ_vin (vin)`
- `UQ_dealership_stock (dealership_id, stock_number)`

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `current_deal_id` → `deals(id)` — nullable

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning for tenant isolation and improved query performance.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read-only access to `retail_price`, `status`, and vehicle specifications. Explicitly denied access to `base_cost` and `minimum_allowed_price`.
- **Sales Manager / GSM:** Read/Write access to all pricing fields and vehicle statuses.
- **Inventory Agent:** Service Account access with Read/Write permission for `status` and `mileage`, and Read-only permission for `base_cost`.
- **Customer-Facing Agents — Lead / Follow-up:** Restricted to Vehicles with `status = AVAILABLE` or `IN_TRANSIT`. They cannot query internal financial or margin data.

### PII Classification

- **Level:** `NONE`
- Vehicle assets do not contain personally identifiable information unless connected to a Trade-in Vehicle, which is handled in a separate specification.

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for the entire database volume.

### Audit Requirements

- Every price change or status transition must generate an immutable entry in `vehicle_audit_log` containing:
  - `timestamp`
  - `actor_id`
  - `previous_value`
  - `new_value`
  - `triggering_event` — for example, DMS Sync or Manual Override

