# Vehicle

## 1. Object Purpose

### Business Purpose

The Vehicle object represents the canonical identity and technical characteristics of one physical or catalogued automotive asset within an authorized dealership context.

It describes what the Vehicle is, including:

- Vehicle Identification Number.
- Chassis and engine identifiers.
- Manufacturer, make, model, trim, and model year.
- Production and registration information.
- Body, powertrain, drivetrain, and transmission specifications.
- Exterior and interior configuration.
- Factory equipment and technical options.
- Odometer information.
- Authoritative identity-verification evidence.
- Source-system provenance.

The Vehicle remains identifiable independently of:

- A dealership stock cycle.
- A reservation.
- A Customer.
- An Opportunity.
- A Quotation.
- A Deal.
- A Trade-In appraisal.
- A finance workflow.
- A delivery workflow.

A Vehicle may participate in multiple historical Inventory Records, Trade-In workflows, Appointments, Quotations, Deals, and ownership periods.

The Vehicle object must not contain dealership-specific commercial stock information.

The following information belongs to the Inventory Record object and must not be owned by Vehicle:

- Stock number.
- Inventory status.
- Commercial availability.
- Branch or parking location.
- Acquisition cost.
- Landed cost.
- Advertised price.
- Minimum authorized price.
- Discount limits.
- Inventory aging.
- Reservation status.
- Deal allocation.
- Floor-plan financing.
- Publication status.
- Sale status.
- Delivery status.
- Inventory transfer or retirement.

### System Purpose

The Vehicle object is the canonical ASOS representation of Vehicle identity and technical specification within a tenant-authorized context.

It normalizes Vehicle data received from sources such as:

- Dealer Management Systems.
- OEM systems.
- Vehicle build sheets.
- Registration systems.
- Inspection providers.
- Trade-In systems.
- Stock-transfer workflows.
- Authorized manual verification.
- External Vehicle-data providers.

The Vehicle object provides stable identity and specification context to:

- Inventory Records.
- Vehicle matching.
- Lead and Opportunity analysis.
- Appointment and test-drive workflows.
- Quotations.
- Trade-In appraisals.
- Deals.
- Finance Applications.
- Financial Contracts.
- Vehicle-delivery workflows.
- Market Intelligence.
- AI Agents.
- Analytics and reporting.

The Vehicle object is an ASOS canonical projection.

It does not automatically replace the external legal or operational System of Record for:

- Vehicle registration.
- Legal ownership.
- Manufacturer certification.
- Service history.
- Insurance status.
- Vehicle inspection.
- Dealership inventory.
- Accounting records.

Each authoritative field must preserve its source, verification status, and synchronization timestamp.

### Canonical Ownership Boundary

| Information | Canonical Domain Owner |
| :--- | :--- |
| VIN and chassis identity | Vehicle |
| Make, model, trim, and model year | Vehicle |
| Engine, battery, transmission, and drivetrain | Vehicle |
| Body type, dimensions, seats, and doors | Vehicle |
| Factory colors and factory options | Vehicle |
| Odometer reading and evidence | Vehicle |
| Stock number | Inventory Record |
| Inventory availability | Inventory Record |
| Branch and physical stock location | Inventory Record |
| Acquisition and landed cost | Inventory Record |
| Retail and advertised pricing | Inventory Record |
| Reservation and allocation | Inventory Record |
| Inventory aging | Inventory Record |
| Deal commercial terms | Deal |
| Customer-specific quotation price | Quotation |
| Trade-In appraisal and valuation | Trade-In |
| Finance terms | Finance Application or Financial Contract |
| Delivery confirmation | Vehicle Delivery workflow |

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `vehicle_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `vehicle_model_id` (UUIDv4 — optional)
  - `manufacturer_id` (UUIDv4 — optional)
  - `supersedes_vehicle_id` (UUIDv4 — optional)
  - `merged_into_vehicle_id` (UUIDv4 — optional)

### Vehicle Identity

- `vin`
- `chassis_number`
- `engine_number`
- `registration_number`
- `license_plate_number`
- `country_of_registration`
- `identity_status`
- `verification_status`
- `verification_method`
- `verified_at`
- `verified_by`

### Manufacturer and Model

- `manufacturer_id`
- `make`
- `model`
- `trim`
- `variant`
- `model_year`
- `production_date`
- `vehicle_model_id`
- `manufacturer_model_code`
- `market_region`

### Physical Configuration

- `body_type`
- `vehicle_category`
- `number_of_doors`
- `seating_capacity`
- `steering_position`
- `exterior_color`
- `exterior_color_code`
- `interior_color`
- `interior_material`

### Powertrain and Performance

- `fuel_type`
- `transmission`
- `drivetrain`
- `engine_displacement_cc`
- `engine_cylinder_count`
- `engine_power_kw`
- `engine_power_hp`
- `engine_torque_nm`
- `battery_capacity_kwh`
- `electric_range_km`
- `combined_fuel_consumption`
- `emission_standard`

### Dimensions and Capacity

- `length_mm`
- `width_mm`
- `height_mm`
- `wheelbase_mm`
- `curb_weight_kg`
- `gross_vehicle_weight_kg`
- `cargo_capacity_liters`
- `fuel_tank_capacity_liters`

### Equipment and Features

- `factory_options`
- `safety_features`
- `comfort_features`
- `infotainment_features`
- `driver_assistance_features`
- `accessibility_features`
- `equipment_snapshot`

### Odometer Information

- `odometer_value`
- `odometer_unit`
- `odometer_status`
- `odometer_recorded_at`
- `odometer_source`
- `odometer_evidence_reference`

### Source and Provenance

- `source_system`
- `source_record_id`
- `source_authority`
- `source_updated_at`
- `last_synced_at`
- `field_authority_map`
- `identity_evidence_hashes`
- `vehicle_snapshot`
- `source_metadata`

### Governance and Lifecycle

- `record_version`
- `vehicle_version`
- `is_current_record`
- `supersedes_vehicle_id`
- `merged_into_vehicle_id`

### Audit Fields

- `created_by`
- `updated_by`
- `verified_by`
- `merged_by`
- `retired_by`
- `last_processed_by_agent`

### Soft Delete

- `is_deleted`
- `deleted_at`
- `deleted_by`
- `deletion_reason`

### Timestamps

- `created_at`
- `updated_at`
- `verified_at`
- `activated_at`
- `identity_conflict_detected_at`
- `merged_at`
- `retired_at`
- `archived_at`
- `last_synced_at`

### Explicitly Excluded Fields

The following fields must not be stored as authoritative Vehicle fields:

- `stock_number`
- `inventory_number`
- `inventory_status`
- `availability_status`
- `branch_id`
- `location_id`
- `arrival_date`
- `days_in_stock`
- `days_in_inventory`
- `base_cost`
- `purchase_price_amount`
- `landed_cost_amount`
- `retail_price`
- `advertised_price_amount`
- `minimum_allowed_price`
- `minimum_authorized_price_amount`
- `maximum_discount_amount`
- `current_deal_id`
- `reservation_id`
- `allocated_deal_id`
- `sold_deal_id`
- `ready_for_sale`
- `ready_for_delivery`

These fields belong to Inventory Record, Quotation, Deal, or another applicable canonical object.

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| vehicle_id | UUID | Unique canonical identifier for the Vehicle within ASOS. | Yes | Auto-generated | Must use a valid UUIDv4 format | 550e8400-e29b-41d4-a716-446655440000 | N/A |
| dealership_id | UUID | Tenant context authorized to access and manage the Vehicle projection. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| vehicle_model_id | UUID | Canonical Vehicle Model reference when available. | No | Null | Must match the make, model, trim, year, and market region | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| manufacturer_id | UUID | Canonical manufacturer or OEM reference. | No | Null | Must match the approved manufacturer dictionary | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| vin | String | Vehicle Identification Number assigned to the physical Vehicle. | Conditional | Null | Must contain exactly 17 valid characters when applicable | 1HGCM82664A123456 | At least 0.995 |
| chassis_number | String | Chassis identifier used where VIN is unavailable or supplemented. | Conditional | Null | At least one authoritative Vehicle identity must exist | CHS-2026-009821 | At least 0.995 |
| engine_number | String | Manufacturer or authority-issued engine identifier. | No | Null | Must preserve exact source formatting | ENG-K20C1-456781 | At least 0.99 |
| registration_number | String | Vehicle registration identifier issued by an authorized body. | No | Null | Must match the registration evidence | REG-2026-145892 | Authoritative source |
| license_plate_number | String | Current registered license-plate identifier. | No | Null | Must be normalized without losing the original representation | ABC-1234 | Authoritative source |
| country_of_registration | String | ISO country associated with the current registration. | No | Null | Must use an approved ISO 3166 country code | EGY | Authoritative source |
| identity_status | Enum | Current Vehicle identity lifecycle state. | Yes | DRAFT | Must match VehicleIdentityStatus ENUM | VERIFIED | System-controlled |
| verification_status | Enum | Result of the latest identity-verification process. | Yes | NOT_VERIFIED | Must match VehicleVerificationStatus ENUM | VERIFIED | System-controlled |
| verification_method | Enum | Method used to verify Vehicle identity. | No | Null | Required when verification status is VERIFIED | VIN_DOCUMENT_MATCH | Authoritative evidence |
| make | String | Vehicle manufacturer brand. | Yes | N/A | Must match the approved manufacturer dictionary | Honda | At least 0.95 |
| model | String | Manufacturer-defined Vehicle model. | Yes | N/A | Must belong to the selected make | Civic | At least 0.95 |
| trim | String | Manufacturer-defined Vehicle trim or grade. | No | Null | Must be valid for model, year, and market when known | Touring | At least 0.90 |
| variant | String | More specific manufacturer configuration or derivative. | No | Null | Must preserve the source-system value | 1.5T CVT | At least 0.90 |
| model_year | Integer | Manufacturer or registration-defined model year. | Yes | N/A | Must fall within the approved historical and future range | 2026 | At least 0.99 |
| production_date | Date | Date or approximate date on which the Vehicle was manufactured. | No | Null | Cannot be materially later than the model year without evidence | 2025-11-18 | Authoritative source |
| body_type | Enum | Physical body configuration. | No | UNKNOWN | Must match VehicleBodyType ENUM | SUV | At least 0.95 |
| fuel_type | Enum | Primary propulsion-energy type. | No | UNKNOWN | Must match VehicleFuelType ENUM | HYBRID | At least 0.99 |
| transmission | Enum | Transmission configuration. | No | UNKNOWN | Must match VehicleTransmission ENUM | CVT | At least 0.95 |
| drivetrain | Enum | Driven-wheel or propulsion configuration. | No | UNKNOWN | Must match VehicleDrivetrain ENUM | FRONT_WHEEL_DRIVE | At least 0.95 |
| exterior_color | String | Manufacturer or observed exterior color. | No | Null | Must distinguish source value from normalized value | Platinum White Pearl | At least 0.90 |
| interior_color | String | Manufacturer or observed interior color. | No | Null | Must distinguish source value from normalized value | Black | At least 0.90 |
| seating_capacity | Integer | Number of approved seating positions. | No | Null | Must be between 1 and 100 | 5 | At least 0.99 |
| odometer_value | Decimal | Latest authoritative odometer reading. | No | Null | Must be zero or greater | 12500 | At least 0.99 |
| odometer_unit | Enum | Unit used by the odometer reading. | No | KILOMETERS | Must match OdometerUnit ENUM | KILOMETERS | At least 0.99 |
| odometer_status | Enum | Trust or exception status of the odometer reading. | No | UNVERIFIED | Must match OdometerStatus ENUM | VERIFIED | Authorized evidence |
| source_system | String | System from which the current canonical record originated. | Yes | MANUAL | Must identify an approved integration or workflow | DMS | System-controlled |
| source_record_id | String | Vehicle identifier in the source system. | No | Null | Must be unique with source system and tenant when populated | DMS-VEH-987456 | System-controlled |
| source_authority | Enum | Authority level of the source for identity and specification data. | Yes | DEALERSHIP_REPORTED | Must match VehicleSourceAuthority ENUM | OEM_VERIFIED | System-controlled |
| field_authority_map | JSONB | Per-field mapping of the authoritative source and timestamp. | Yes | Empty object | Must use approved field names and source identifiers | {"vin":{"source":"OEM","verified":true}} | System-controlled |
| record_version | Integer | Optimistic-concurrency version for the current record. | Yes | 1 | Must increase after every permitted update | 8 | System-controlled |

## 4. Enumerations

### VehicleIdentityStatus

- **DRAFT:** Vehicle data exists but minimum identity requirements are incomplete.
- **IDENTIFIED:** A probable Vehicle identity exists but has not completed authoritative verification.
- **VERIFIED:** Vehicle identity was confirmed using approved evidence.
- **ACTIVE:** The verified Vehicle is available for use by authorized domain workflows.
- **IDENTITY_CONFLICT:** Two or more authoritative sources disagree about Vehicle identity.
- **MERGED:** The record was identified as a duplicate and merged into another Vehicle.
- **RETIRED:** The Vehicle projection is no longer active in the tenant context.
- **ARCHIVED:** The historical record moved to long-term retention.

### VehicleVerificationStatus

- NOT_VERIFIED
- VERIFICATION_PENDING
- VERIFIED
- FAILED
- CONFLICT_DETECTED
- MANUAL_REVIEW_REQUIRED
- EXPIRED

### VehicleVerificationMethod

- OEM_BUILD_SHEET
- VIN_DOCUMENT_MATCH
- REGISTRATION_DOCUMENT
- PHYSICAL_INSPECTION
- DMS_VERIFIED
- AUTHORIZED_PROVIDER
- STOCK_TRANSFER_EVIDENCE
- TRADE_IN_EVIDENCE
- MANUAL_SPECIALIST_REVIEW
- MULTI_SOURCE_MATCH
- OTHER

### VehicleFuelType

- GASOLINE
- DIESEL
- HYBRID
- PLUG_IN_HYBRID
- BATTERY_ELECTRIC
- HYDROGEN
- LPG
- CNG
- FLEX_FUEL
- OTHER
- UNKNOWN

### VehicleTransmission

- AUTOMATIC
- MANUAL
- CVT
- DCT
- SINGLE_SPEED
- SEMI_AUTOMATIC
- OTHER
- UNKNOWN

### VehicleDrivetrain

- FRONT_WHEEL_DRIVE
- REAR_WHEEL_DRIVE
- ALL_WHEEL_DRIVE
- FOUR_WHEEL_DRIVE
- OTHER
- UNKNOWN

### VehicleBodyType

- SEDAN
- HATCHBACK
- SUV
- CROSSOVER
- COUPE
- CONVERTIBLE
- WAGON
- PICKUP
- VAN
- MINIVAN
- BUS
- TRUCK
- MOTORCYCLE
- OTHER
- UNKNOWN

### VehicleCategory

- PASSENGER
- LIGHT_COMMERCIAL
- HEAVY_COMMERCIAL
- MOTORCYCLE
- SPECIAL_PURPOSE
- OTHER

### OdometerUnit

- KILOMETERS
- MILES

### OdometerStatus

- UNVERIFIED
- VERIFIED
- EXEMPT
- NOT_ACTUAL
- DISCREPANCY_DETECTED
- ROLLOVER_DETECTED
- REPLACED
- MANUAL_REVIEW_REQUIRED

### VehicleSourceAuthority

- OEM_VERIFIED
- GOVERNMENT_VERIFIED
- INSPECTION_VERIFIED
- DMS_VERIFIED
- AUTHORIZED_PROVIDER
- DEALERSHIP_REPORTED
- CUSTOMER_REPORTED
- AI_EXTRACTED
- MANUAL_ENTRY
- UNKNOWN

## 5. Validation Rules

### Business Rules

- Every Vehicle must belong to an authorized tenant context.
- Every Vehicle must have at least one authoritative identity:
  - VIN.
  - Chassis number.
  - Another jurisdiction-approved physical identifier.

- A VIN must not be created from AI inference alone.
- Vehicle identity must remain independent of dealership inventory status.
- A Vehicle may exist without an active Inventory Record.
- A Vehicle may reference multiple historical Inventory Records within permitted tenant and transfer rules.
- Vehicle fields must not be used as the authoritative source for:
  - Stock availability.
  - Inventory location.
  - Pricing.
  - Reservation.
  - Deal allocation.
  - Inventory aging.
  - Sale.
  - Delivery.

- Customer-visible availability and price must be obtained from an active Inventory Record and applicable Quotation or Deal.
- A Vehicle must not be marked sold, reserved, available, in transit, or delivered through its identity state.
- Vehicle identity verification does not prove:
  - Legal ownership.
  - Inventory availability.
  - Sale eligibility.
  - Roadworthiness.
  - Insurance.
  - Registration validity.
  - Delivery completion.

- A duplicate Vehicle must be merged through a governed process and not silently deleted.
- Historical source identifiers and verification evidence must remain traceable.

### Technical Rules

- Vehicle creation and source ingestion must support idempotency.
- Updates must use `record_version` for optimistic concurrency.
- Source synchronization must preserve:
  - Source system.
  - Source record ID.
  - Source timestamp.
  - Retrieval timestamp.
  - Processing result.
  - Payload hash when applicable.

- Field conflicts must be resolved using the approved source-authority policy.
- Lower-authority sources must not silently overwrite higher-authority verified values.
- Every authoritative-field change must create immutable history.
- Vehicle merge operations must be transactional.
- Search indexes must be rebuilt or updated after identity corrections or merges.
- AI-extracted fields must preserve:
  - Model reference.
  - Confidence.
  - Input evidence.
  - Extraction timestamp.
  - Human-review status.

### Data Constraints

- `vin` must contain exactly 17 characters when populated.
- VIN values must exclude the letters `I`, `O`, and `Q`.
- VIN checksum validation must be applied where required by the applicable market.
- VIN values must be normalized to uppercase.
- At least one of `vin` or `chassis_number` must be populated before entering `IDENTIFIED`.
- `model_year` must fall within the approved historical range and normally not exceed the current year plus two.
- `production_date` must not be unreasonably later than the model year.
- `odometer_value` cannot be negative.
- A later odometer reading should not be lower than a prior verified reading without an approved correction or replacement event.
- `seating_capacity` and `number_of_doors` cannot be negative.
- Technical dimensions and capacities cannot be negative.
- `merged_into_vehicle_id` is required when identity status is `MERGED`.
- `verified_at`, `verified_by`, and `verification_method` are required when verification status is `VERIFIED`.
- `identity_conflict_detected_at` is required when identity status is `IDENTITY_CONFLICT`.

### Uniqueness Rules

- An active VIN must be unique within the authorized tenant Vehicle context.
- An active chassis number must be unique within the authorized tenant Vehicle context when populated.
- `source_system + source_record_id + dealership_id` must be unique when `source_record_id` exists.
- A merged Vehicle cannot remain the current active record.
- Circular merge and supersession relationships are prohibited.

### Referential Integrity

- `vehicle_model_id` must match the Vehicle’s make, model, trim, model year, and market region.
- `manufacturer_id` must match the Vehicle make.
- `supersedes_vehicle_id` must reference an earlier Vehicle version or controlled replacement.
- `merged_into_vehicle_id` must reference a different Vehicle.
- Inventory Records referencing a merged Vehicle must be redirected through an authorized reconciliation workflow.
- Cross-tenant Vehicle relationships are prohibited unless created through an explicit governed stock-transfer or data-sharing process.
- A Vehicle referenced by an Inventory Record, Trade-In, Quotation, Deal, Financial Contract, or audit record cannot be hard-deleted.

### Human Approval Requirements

- Conflicting VIN, chassis, engine, registration, or manufacturer evidence requires Human Review.
- Changing a verified VIN requires specialist approval and immutable correction evidence.
- Merging duplicate Vehicles requires an authorized Data Steward or equivalent role.
- Correcting a verified odometer discrepancy requires supporting evidence and authorized review.
- Cross-tenant Vehicle reconciliation requires explicit security and data-governance approval.
- AI Agents cannot:
  - Verify Vehicle identity independently.
  - Change a verified VIN.
  - Merge Vehicle records.
  - Resolve an identity conflict.
  - Delete authoritative evidence.
  - Mark a Vehicle sold, reserved, available, or delivered.

## 6. State Machine

### Allowed States

- DRAFT
- IDENTIFIED
- VERIFIED
- ACTIVE
- IDENTITY_CONFLICT
- MERGED
- RETIRED
- ARCHIVED

### Allowed Transitions

- DRAFT → IDENTIFIED
- DRAFT → RETIRED
- IDENTIFIED → VERIFIED
- IDENTIFIED → IDENTITY_CONFLICT
- IDENTIFIED → RETIRED
- VERIFIED → ACTIVE
- VERIFIED → IDENTITY_CONFLICT
- VERIFIED → RETIRED
- ACTIVE → IDENTITY_CONFLICT
- ACTIVE → MERGED
- ACTIVE → RETIRED
- IDENTITY_CONFLICT → VERIFIED
- IDENTITY_CONFLICT → MERGED
- IDENTITY_CONFLICT → RETIRED
- MERGED → ARCHIVED
- RETIRED → ARCHIVED

### Forbidden Transitions

- DRAFT → ACTIVE
- DRAFT → VERIFIED
- DRAFT → MERGED
- IDENTIFIED → ACTIVE
- IDENTIFIED → MERGED without duplicate evidence
- VERIFIED → DRAFT
- ACTIVE → DRAFT
- ACTIVE → IDENTIFIED
- MERGED → ACTIVE
- MERGED → VERIFIED
- RETIRED → ACTIVE
- ARCHIVED → ACTIVE
- ARCHIVED → IDENTIFIED
- ARCHIVED → VERIFIED

### Entry Conditions

- To enter `IDENTIFIED`:
  - At least one valid identity identifier must exist.
  - Make, model, and model year must be populated.
  - The source system and source authority must be known.
  - Initial duplicate checks must complete.

- To enter `VERIFIED`:
  - Approved identity evidence must exist.
  - VIN or chassis evidence must match the Vehicle.
  - Source conflicts must be resolved.
  - `verification_method`, `verified_at`, and `verified_by` must be recorded.

- To enter `ACTIVE`:
  - Identity status must be verified.
  - No unresolved identity conflict may exist.
  - Required canonical specifications must pass validation.
  - The Vehicle must be permitted for use by downstream workflows.

- To enter `IDENTITY_CONFLICT`:
  - Two or more material identity sources must disagree.
  - The conflict type and affected fields must be recorded.
  - Downstream high-risk operations must be restricted where necessary.
  - A Human Review Task must be created.

- To enter `MERGED`:
  - The Vehicle must be confirmed as a duplicate.
  - A valid `merged_into_vehicle_id` must exist.
  - All downstream references must be reconciled or queued for reconciliation.
  - Merge evidence and approving authority must be stored.

- To enter `RETIRED`:
  - The Vehicle projection must no longer be active within the tenant context.
  - Retirement must not be used to represent a sale or delivery.
  - The retirement reason and authority must be recorded.

- To enter `ARCHIVED`:
  - The Vehicle must already be `MERGED` or `RETIRED`.
  - Retention, dependency, audit, and legal-hold checks must pass.

### Exit Conditions

- A Vehicle cannot exit `DRAFT` until minimum identity requirements are satisfied.
- A Vehicle cannot exit `IDENTIFIED` toward `VERIFIED` without authoritative evidence.
- A Vehicle cannot exit `VERIFIED` toward `ACTIVE` while a material conflict remains.
- A Vehicle cannot exit `IDENTITY_CONFLICT` without an authorized resolution.
- A merged Vehicle cannot return to active use.
- A retired Vehicle cannot return to active use through a normal transition.
- Reactivation after an incorrect retirement requires a controlled correction or replacement workflow.
- Archival must not break active Inventory Record, Trade-In, Deal, Contract, or audit references.

### Terminal States

- **MERGED:** The record was permanently consolidated into another canonical Vehicle.
- **ARCHIVED:** The inactive historical Vehicle record moved to long-term retention.

## 7. Relationships

### Depends On

- Dealership tenant identified by `dealership_id`.
- Approved Vehicle-manufacturer dictionaries.
- Approved Vehicle Model catalogues where available.
- Authorized external identity and specification sources.
- Source-authority and conflict-resolution policies.
- Identity-verification and duplicate-detection services.

### Consumes

- OEM build-sheet data.
- Dealer Management System Vehicle records.
- Registration and licensing data.
- Physical Vehicle inspection results.
- Trade-In Vehicle identity information.
- Authorized Vehicle-data-provider responses.
- Stock-transfer identity evidence.
- Odometer readings and supporting evidence.
- Manual specialist verification.
- Vehicle correction and merge decisions.

### Produces

- Stable canonical Vehicle identity.
- Normalized technical specifications.
- Verified VIN and chassis context.
- Vehicle source and provenance information.
- Odometer history and confidence status.
- Identity-conflict alerts.
- Duplicate-Vehicle candidates.
- Vehicle context for downstream inventory and sales workflows.

### One-to-Many Relationships

A Vehicle may be referenced by multiple:

- Inventory Records.
- Trade-In records.
- Appointments.
- Test-drive records.
- Quotations.
- Deals.
- Finance Applications.
- Financial Contracts.
- Vehicle-delivery records.
- Inspection records.
- Interactions.
- Market Intelligence observations.
- AI Agent Runs.
- Audit records.

### Inventory Relationship

- A Vehicle may exist without an active Inventory Record.
- A Vehicle may have multiple historical Inventory Records.
- Only an active Inventory Record may define current dealership:
  - Stock number.
  - Availability.
  - Location.
  - Pricing.
  - Reservation.
  - Deal allocation.
  - Sale state.
  - Delivery state.
  - Inventory aging.

- Vehicle identity must remain stable when an Inventory Record is:
  - Sold.
  - Delivered.
  - Transferred.
  - Returned.
  - Retired.
  - Archived.

### Trade-In Relationship

- A Trade-In references the Vehicle being evaluated or acquired.
- Trade-In valuation, appraisal, ownership, payoff, and acquisition information must remain owned by the Trade-In object.
- Completion of a Trade-In acquisition may create a new Inventory Record for the same Vehicle.
- The Vehicle record must not treat a Trade-In appraisal value as a Vehicle attribute.

### Deal and Quotation Relationships

- Quotations and Deals may reference a Vehicle directly and its active Inventory Record.
- Vehicle defines identity and specifications.
- Inventory Record defines current availability and approved commercial stock context.
- Quotation defines Customer-specific commercial terms.
- Deal defines the governed transaction.
- Vehicle must not duplicate quotation price, Deal price, discount, tax, or payment information.

### Finance Relationships

- Finance Applications and Financial Contracts may reference the Vehicle.
- Vehicle may provide:
  - VIN.
  - Make.
  - Model.
  - Model year.
  - Technical specifications.
  - Permitted valuation context.

- Vehicle must not store:
  - Finance approval.
  - Lender decision.
  - Interest rate.
  - Deposit.
  - Instalment.
  - Contract status.
  - Funding confirmation.

### Market Intelligence Relationship

Market Intelligence may reference:

- Vehicle make.
- Vehicle model.
- Trim.
- Model year.
- Fuel type.
- Body type.
- Technical configuration.

Market Intelligence does not alter authoritative Vehicle identity without approved verification evidence.

### Source-System Relationship

Every externally sourced Vehicle record must preserve:

- Source system.
- Source record identifier.
- Source authority.
- Source timestamp.
- Last synchronization timestamp.
- Field-level ownership where applicable.
- Conflict-resolution result.
- Processing and audit evidence.

### Merge and Supersession Relationships

- `supersedes_vehicle_id` represents a controlled replacement or corrected canonical projection.
- `merged_into_vehicle_id` represents duplicate consolidation.
- A merged Vehicle must reference exactly one surviving canonical Vehicle.
- Circular merges are prohibited.
- Downstream references must be reconciled before merge completion.
- Historical Vehicle records must remain discoverable for audit purposes.

### Owned By

- Tenant access and operational governance are owned by the Dealership identified by `dealership_id`.
- Identity verification is owned by authorized Data Steward, Inventory, Compliance, or specialist roles.
- Legal ownership is not inferred from tenant access or Vehicle creation.

### Supports but Does Not Replace

The Vehicle object supports but does not replace:

- Government registration records.
- OEM certification.
- Legal ownership documentation.
- Service-history systems.
- Insurance records.
- Roadworthiness inspection.
- Inventory management.
- Accounting records.
- Deal and contract evidence.
- Vehicle-delivery confirmation.

## 8. Domain Events

### Emitted Events

#### VehicleCreated

Emitted when a new Vehicle canonical projection is created.

Payload:

- `event_id`
- `event_version`
- `dealership_id`
- `vehicle_id`
- `source_system`
- `source_record_id`
- `identity_status`
- `created_at`
- `correlation_id`
- `causation_id`

#### VehicleIdentified

Emitted when minimum Vehicle identity requirements are satisfied.

Payload:

- `vehicle_id`
- `vin`
- `chassis_number`
- `make`
- `model`
- `model_year`
- `identity_status`
- `identified_at`

#### VehicleVerificationRequested

Emitted when Vehicle identity requires formal verification.

Payload:

- `vehicle_id`
- `verification_method`
- `required_evidence`
- `requested_by`
- `requested_at`
- `priority`

#### VehicleVerified

Emitted when Vehicle identity is authoritatively verified.

Payload:

- `vehicle_id`
- `verification_method`
- `source_authority`
- `verified_fields`
- `verified_by`
- `verified_at`
- `evidence_hashes`

#### VehicleActivated

Emitted when a verified Vehicle becomes available to downstream workflows.

Payload:

- `vehicle_id`
- `dealership_id`
- `vehicle_model_id`
- `activated_by`
- `activated_at`

#### VehicleIdentityConflictDetected

Emitted when material Vehicle identity sources disagree.

Payload:

- `vehicle_id`
- `conflicting_fields`
- `source_references`
- `previous_identity_status`
- `detected_at`
- `review_priority`

#### VehicleIdentityConflictResolved

Emitted after an authorized Human Review resolves the identity conflict.

Payload:

- `vehicle_id`
- `resolved_fields`
- `authoritative_sources`
- `resolution_type`
- `resolved_by`
- `resolved_at`

#### VehicleSpecificationUpdated

Emitted when an approved technical specification changes.

Payload:

- `vehicle_id`
- `changed_fields`
- `previous_values_hash`
- `new_values_hash`
- `source_authority`
- `updated_by`
- `updated_at`

#### VehicleOdometerRecorded

Emitted when a new odometer reading is accepted.

Payload:

- `vehicle_id`
- `odometer_value`
- `odometer_unit`
- `odometer_status`
- `odometer_source`
- `recorded_at`
- `evidence_reference`

#### VehicleOdometerDiscrepancyDetected

Emitted when an odometer reading conflicts with historical evidence.

Payload:

- `vehicle_id`
- `previous_verified_value`
- `submitted_value`
- `odometer_unit`
- `source_reference`
- `detected_at`
- `review_priority`

#### VehicleDuplicateDetected

Emitted when one or more probable duplicate Vehicle records are found.

Payload:

- `vehicle_id`
- `candidate_vehicle_ids`
- `matching_fields`
- `confidence`
- `detected_at`

#### VehicleMerged

Emitted after a duplicate Vehicle is merged into the surviving canonical Vehicle.

Payload:

- `source_vehicle_id`
- `surviving_vehicle_id`
- `reconciled_reference_count`
- `merged_by`
- `merged_at`

#### VehicleRetired

Emitted when a Vehicle projection is retired from active tenant use.

Payload:

- `vehicle_id`
- `retirement_reason`
- `retired_by`
- `retired_at`

#### VehicleArchived

Emitted when an inactive Vehicle record moves to long-term retention.

Payload:

- `vehicle_id`
- `previous_identity_status`
- `archived_by`
- `archived_at`

#### VehicleHumanReviewRequired

Emitted when a Vehicle decision exceeds automation authority.

Payload:

- `vehicle_id`
- `review_reason`
- `affected_fields`
- `source_references`
- `priority`
- `created_at`

### Consumed Events

#### OEMVehicleDataReceived

Used to create or enrich Vehicle identity and specifications.

#### DMSVehicleRecordReceived

Used to create or synchronize a canonical Vehicle projection.

#### RegistrationDocumentVerified

Used to verify registration-related Vehicle identity fields.

#### PhysicalVehicleInspectionCompleted

Used to validate VIN, chassis, engine, odometer, and physical specifications.

#### TradeInVehicleIdentified

Used to create or match the Customer Vehicle involved in a Trade-In workflow.

#### InventoryVehicleReceived

Used to confirm that the received inventory unit references the correct Vehicle.

#### StockTransferVehicleReceived

Used to match a transferred Vehicle with its canonical identity.

#### VehicleDataProviderResponseReceived

Used to enrich Vehicle specifications subject to source authority.

#### VehicleCorrectionApproved

Used to apply an authorized identity or specification correction.

#### LegalHoldApplied

Used to restrict merge, deletion, archival, or sensitive updates.

#### LegalHoldReleased

Used to resume permitted governance operations.

### Event Requirements

Every Vehicle event must:

- Include a unique `event_id`.
- Include `dealership_id`.
- Include `vehicle_id`.
- Include an event version.
- Include authoritative timestamps.
- Include correlation and causation identifiers.
- Be idempotent.
- Preserve source authority where relevant.
- Avoid exposing restricted evidence unnecessarily.
- Support replay without creating duplicate Vehicle records.
- Preserve ordering for identity-state transitions.
- Be recorded in immutable event or audit history.

## 9. AI Considerations

### Permitted AI Uses

AI Agents may assist with:

- Extracting Vehicle details from documents.
- Normalizing make, model, trim, and variant names.
- Matching Vehicle records across approved sources.
- Suggesting possible duplicate Vehicle records.
- Summarizing technical specifications.
- Detecting missing Vehicle information.
- Detecting inconsistent odometer data.
- Comparing source records.
- Recommending Human Review.
- Supporting Vehicle matching for Customer preferences.
- Generating Customer-friendly Vehicle descriptions from approved fields.

### Prohibited Autonomous AI Actions

AI Agents must not independently:

- Create a verified VIN.
- Confirm Vehicle legal identity.
- Resolve a VIN conflict.
- Change a verified VIN.
- Merge duplicate Vehicle records.
- Delete identity evidence.
- Confirm legal ownership.
- Confirm registration validity.
- Confirm roadworthiness.
- Confirm insurance status.
- Mark a Vehicle available.
- Set Vehicle price.
- Reserve a Vehicle.
- Allocate a Vehicle to a Deal.
- Mark a Vehicle sold.
- Confirm Vehicle delivery.

### Fields Eligible for Vector Embeddings

Permitted fields may include:

- Make.
- Model.
- Trim.
- Variant.
- Model year.
- Body type.
- Fuel type.
- Transmission.
- Drivetrain.
- Exterior color.
- Interior color.
- Factory options.
- Safety features.
- Comfort features.
- Infotainment features.
- Driver-assistance features.
- Approved technical summaries.
- Approved Customer-visible Vehicle descriptions.

### Fields Excluded from Vector Embeddings

The following must not be embedded unless an explicitly approved use case requires it:

- Full VIN.
- Chassis number.
- Engine number.
- Registration number.
- License-plate number.
- Government document identifiers.
- Identity-evidence documents.
- Evidence hashes.
- Source credentials.
- Secure provider metadata.
- Internal audit comments.
- User identifiers.
- Legal-hold information.
- Customer-linked Vehicle information.
- Exact restricted source payloads.

Where Vehicle matching requires a VIN, it must use authorized structured lookup rather than semantic vector retrieval.

### Structured AI Context

Authorized AI Agents may receive:

- `vehicle_id`
- `make`
- `model`
- `trim`
- `variant`
- `model_year`
- `body_type`
- `fuel_type`
- `transmission`
- `drivetrain`
- `exterior_color`
- `interior_color`
- `seating_capacity`
- `factory_options`
- `safety_features`
- `comfort_features`
- `driver_assistance_features`
- `verification_status`
- `source_authority`

Restricted identity fields may be provided only when:

- The Agent has an approved business purpose.
- Tenant scope is enforced.
- Field-level authorization passes.
- Access is audited.
- The output does not expose the restricted value unnecessarily.

### Metadata Filters for Retrieval

Every Vehicle retrieval must support:

- `dealership_id` — mandatory.
- `vehicle_id`
- `vehicle_model_id`
- `manufacturer_id`
- `make`
- `model`
- `trim`
- `model_year`
- `body_type`
- `fuel_type`
- `transmission`
- `drivetrain`
- `identity_status`
- `verification_status`
- `source_authority`
- `is_current_record`

### Confidence Thresholds

- VIN extraction: minimum `0.995`.
- Chassis-number extraction: minimum `0.995`.
- Registration-number extraction: minimum `0.995`.
- Make and model identification: minimum `0.95`.
- Trim and variant identification: minimum `0.90`.
- Model-year identification: minimum `0.99`.
- Odometer extraction: minimum `0.99`.
- Technical-option extraction: minimum `0.90`.
- Duplicate recommendation: minimum composite confidence `0.95`.
- Customer-facing technical summary: minimum source-grounding confidence `0.90`.

Meeting a confidence threshold does not replace authoritative verification.

### Human Review Triggers

Human Review is mandatory when:

- VIN sources disagree.
- VIN fails validation.
- Chassis and VIN refer to different Vehicles.
- Verified source data is being changed.
- Odometer decreases unexpectedly.
- Duplicate matching affects active Inventory Records or Deals.
- Vehicle identity crosses tenant boundaries.
- Registration or ownership evidence is inconsistent.
- A lower-authority source conflicts with a higher-authority source.
- AI confidence is below the required threshold for a material field.
- Legal, compliance, fraud, or safety concerns exist.

### AI Output Requirements

Every material AI output must include:

- Vehicle record version.
- Source references.
- Extracted or compared fields.
- Confidence score.
- Model reference.
- Prompt or extraction version.
- Timestamp.
- Whether Human Review is required.

AI-generated descriptions must not include:

- Unsupported performance claims.
- Unverified ownership claims.
- Unverified warranty claims.
- Unverified accident-history claims.
- Restricted identifiers.
- Inventory availability or price unless retrieved from the active Inventory Record.

## 10. API Contract

### REST Resource

**Base Path:**

```text
/api/v1/dealerships/{dealership_id}/vehicles
```

### Supported Methods

- `GET` — list and search permitted Vehicle records.
- `POST` — create a new Vehicle projection.
- `GET /{vehicle_id}` — retrieve one Vehicle.
- `PATCH /{vehicle_id}` — update permitted identity or specification fields.
- `POST /{vehicle_id}/request-verification` — start Vehicle identity verification.
- `POST /{vehicle_id}/verify` — record an authorized verification decision.
- `POST /{vehicle_id}/report-conflict` — create an identity-conflict workflow.
- `POST /{vehicle_id}/resolve-conflict` — record an authorized conflict resolution.
- `POST /{vehicle_id}/record-odometer` — add an odometer reading.
- `GET /{vehicle_id}/odometer-history` — retrieve permitted odometer history.
- `POST /{vehicle_id}/detect-duplicates` — run duplicate detection.
- `POST /{vehicle_id}/merge` — merge an approved duplicate into a surviving Vehicle.
- `GET /{vehicle_id}/source-history` — retrieve permitted source and synchronization history.
- `GET /{vehicle_id}/inventory-records` — retrieve related Inventory Records.
- `POST /{vehicle_id}/retire` — retire the Vehicle projection.
- `POST /{vehicle_id}/archive` — archive an eligible inactive Vehicle.
- `DELETE /{vehicle_id}` — soft delete only when legally and operationally permitted.

### API Restrictions

The Vehicle API must not expose commands for:

- Setting stock number.
- Changing inventory availability.
- Setting Vehicle price.
- Creating a reservation.
- Allocating a Deal.
- Marking the Vehicle sold.
- Confirming delivery.

Those operations belong to their applicable canonical objects and workflows.

### Required Headers

- `Authorization`
- `X-Dealership-Id`
- `Idempotency-Key` for mutating operations.
- `If-Match` or equivalent record-version header for updates.
- `X-Correlation-Id`
- `X-Causation-Id` where applicable.

### Suggested JSON Schema — Create Vehicle

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateVehicleRequest",
  "type": "object",
  "properties": {
    "vin": {
      "type": ["string", "null"],
      "pattern": "^[A-HJ-NPR-Z0-9]{17}$"
    },
    "chassis_number": {
      "type": ["string", "null"],
      "minLength": 3,
      "maxLength": 100
    },
    "engine_number": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "registration_number": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "license_plate_number": {
      "type": ["string", "null"],
      "maxLength": 50
    },
    "country_of_registration": {
      "type": ["string", "null"],
      "pattern": "^[A-Z]{3}$"
    },
    "manufacturer_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "vehicle_model_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "make": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "model": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "trim": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "variant": {
      "type": ["string", "null"],
      "maxLength": 150
    },
    "model_year": {
      "type": "integer",
      "minimum": 1886,
      "maximum": 2100
    },
    "production_date": {
      "type": ["string", "null"],
      "format": "date"
    },
    "body_type": {
      "type": "string",
      "enum": [
        "SEDAN",
        "HATCHBACK",
        "SUV",
        "CROSSOVER",
        "COUPE",
        "CONVERTIBLE",
        "WAGON",
        "PICKUP",
        "VAN",
        "MINIVAN",
        "BUS",
        "TRUCK",
        "MOTORCYCLE",
        "OTHER",
        "UNKNOWN"
      ]
    },
    "fuel_type": {
      "type": "string",
      "enum": [
        "GASOLINE",
        "DIESEL",
        "HYBRID",
        "PLUG_IN_HYBRID",
        "BATTERY_ELECTRIC",
        "HYDROGEN",
        "LPG",
        "CNG",
        "FLEX_FUEL",
        "OTHER",
        "UNKNOWN"
      ]
    },
    "transmission": {
      "type": "string",
      "enum": [
        "AUTOMATIC",
        "MANUAL",
        "CVT",
        "DCT",
        "SINGLE_SPEED",
        "SEMI_AUTOMATIC",
        "OTHER",
        "UNKNOWN"
      ]
    },
    "drivetrain": {
      "type": "string",
      "enum": [
        "FRONT_WHEEL_DRIVE",
        "REAR_WHEEL_DRIVE",
        "ALL_WHEEL_DRIVE",
        "FOUR_WHEEL_DRIVE",
        "OTHER",
        "UNKNOWN"
      ]
    },
    "exterior_color": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "interior_color": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "seating_capacity": {
      "type": ["integer", "null"],
      "minimum": 1,
      "maximum": 100
    },
    "odometer_value": {
      "type": ["number", "null"],
      "minimum": 0
    },
    "odometer_unit": {
      "type": ["string", "null"],
      "enum": [
        "KILOMETERS",
        "MILES"
      ]
    },
    "source_system": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "source_record_id": {
      "type": ["string", "null"],
      "maxLength": 255
    },
    "source_authority": {
      "type": "string",
      "enum": [
        "OEM_VERIFIED",
        "GOVERNMENT_VERIFIED",
        "INSPECTION_VERIFIED",
        "DMS_VERIFIED",
        "AUTHORIZED_PROVIDER",
        "DEALERSHIP_REPORTED",
        "CUSTOMER_REPORTED",
        "AI_EXTRACTED",
        "MANUAL_ENTRY",
        "UNKNOWN"
      ]
    }
  },
  "required": [
    "make",
    "model",
    "model_year",
    "source_system",
    "source_authority"
  ],
  "anyOf": [
    {
      "required": ["vin"]
    },
    {
      "required": ["chassis_number"]
    }
  ],
  "additionalProperties": false
}
```

### Suggested JSON Schema — Record Odometer

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "RecordVehicleOdometerRequest",
  "type": "object",
  "properties": {
    "odometer_value": {
      "type": "number",
      "minimum": 0
    },
    "odometer_unit": {
      "type": "string",
      "enum": [
        "KILOMETERS",
        "MILES"
      ]
    },
    "odometer_source": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "recorded_at": {
      "type": "string",
      "format": "date-time"
    },
    "evidence_reference": {
      "type": ["string", "null"],
      "maxLength": 500
    }
  },
  "required": [
    "odometer_value",
    "odometer_unit",
    "odometer_source",
    "recorded_at"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Vehicle {
  id: ID!
  dealershipId: ID!
  vehicleModelId: ID
  manufacturerId: ID
  supersedesVehicleId: ID
  mergedIntoVehicleId: ID

  vin: String
  chassisNumber: String
  engineNumber: String
  registrationNumber: String
  licensePlateNumber: String
  countryOfRegistration: String

  identityStatus: VehicleIdentityStatus!
  verificationStatus: VehicleVerificationStatus!
  verificationMethod: VehicleVerificationMethod
  verifiedAt: DateTime

  make: String!
  model: String!
  trim: String
  variant: String
  modelYear: Int!
  productionDate: Date
  manufacturerModelCode: String
  marketRegion: String

  bodyType: VehicleBodyType
  vehicleCategory: VehicleCategory
  numberOfDoors: Int
  seatingCapacity: Int
  steeringPosition: String
  exteriorColor: String
  exteriorColorCode: String
  interiorColor: String
  interiorMaterial: String

  fuelType: VehicleFuelType
  transmission: VehicleTransmission
  drivetrain: VehicleDrivetrain
  engineDisplacementCc: Int
  engineCylinderCount: Int
  enginePowerKw: Float
  enginePowerHp: Float
  engineTorqueNm: Float
  batteryCapacityKwh: Float
  electricRangeKm: Float
  combinedFuelConsumption: Float
  emissionStandard: String

  lengthMm: Float
  widthMm: Float
  heightMm: Float
  wheelbaseMm: Float
  curbWeightKg: Float
  grossVehicleWeightKg: Float
  cargoCapacityLiters: Float
  fuelTankCapacityLiters: Float

  factoryOptions: [String!]!
  safetyFeatures: [String!]!
  comfortFeatures: [String!]!
  infotainmentFeatures: [String!]!
  driverAssistanceFeatures: [String!]!
  accessibilityFeatures: [String!]!

  odometerValue: Float
  odometerUnit: OdometerUnit
  odometerStatus: OdometerStatus
  odometerRecordedAt: DateTime
  odometerSource: String

  sourceSystem: String!
  sourceRecordId: String
  sourceAuthority: VehicleSourceAuthority!
  sourceUpdatedAt: DateTime
  lastSyncedAt: DateTime

  recordVersion: Int!
  vehicleVersion: Int!
  isCurrentRecord: Boolean!
  isDeleted: Boolean!

  inventoryRecords: [InventoryRecord!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

### Error Responses

Common API error codes:

- `VEHICLE_NOT_FOUND`
- `VEHICLE_IDENTITY_INCOMPLETE`
- `VEHICLE_VIN_INVALID`
- `VEHICLE_VIN_CONFLICT`
- `VEHICLE_DUPLICATE_DETECTED`
- `VEHICLE_VERIFICATION_REQUIRED`
- `VEHICLE_IDENTITY_CONFLICT`
- `VEHICLE_MERGE_NOT_ALLOWED`
- `VEHICLE_REFERENCED_BY_ACTIVE_WORKFLOW`
- `VEHICLE_RECORD_VERSION_CONFLICT`
- `VEHICLE_SOURCE_AUTHORITY_INSUFFICIENT`
- `VEHICLE_CROSS_TENANT_ACCESS_DENIED`
- `VEHICLE_LEGAL_HOLD_ACTIVE`

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `vehicles`
- **Identity Table:** `vehicle_identifiers`
- **Specification Table:** `vehicle_specifications`
- **Equipment Table:** `vehicle_equipment`
- **Odometer Table:** `vehicle_odometer_history`
- **Source Table:** `vehicle_source_records`
- **Field Authority Table:** `vehicle_field_authority`
- **Verification Table:** `vehicle_verifications`
- **Conflict Table:** `vehicle_identity_conflicts`
- **Merge Table:** `vehicle_merges`
- **Version-History Table:** `vehicle_versions`
- **Status-History Table:** `vehicle_status_history`
- **Audit Table:** `vehicle_audit_log`

### Primary Table Responsibilities

The `vehicles` table should store:

- Canonical Vehicle identifier.
- Tenant scope.
- Current identity state.
- Current verified identity summary.
- Current normalized technical specification.
- Current source and verification status.
- Version and lifecycle fields.

It must not store authoritative:

- Inventory pricing.
- Inventory availability.
- Stock number.
- Reservation.
- Deal allocation.
- Sale state.
- Delivery state.

### Recommended Indexes

- `idx_vehicle_tenant_status (dealership_id, identity_status)`
- `idx_vehicle_tenant_vin (dealership_id, vin)`
- `idx_vehicle_tenant_chassis (dealership_id, chassis_number)`
- `idx_vehicle_make_model_year (dealership_id, make, model, model_year)`
- `idx_vehicle_model_reference (dealership_id, vehicle_model_id)`
- `idx_vehicle_verification (dealership_id, verification_status)`
- `idx_vehicle_source_record (dealership_id, source_system, source_record_id)`
- `idx_vehicle_merge_target (dealership_id, merged_into_vehicle_id)`
- `idx_vehicle_current_record (dealership_id, is_current_record)`
- `idx_vehicle_odometer_latest (dealership_id, vehicle_id, odometer_recorded_at DESC)`
- `idx_vehicle_conflict_queue (dealership_id, identity_status, identity_conflict_detected_at)`
- `idx_vehicle_updated_at (dealership_id, updated_at DESC)`

### Unique Constraints

- `UQ_vehicle_active_vin (dealership_id, vin) WHERE vin IS NOT NULL AND is_current_record = true AND is_deleted = false`
- `UQ_vehicle_active_chassis (dealership_id, chassis_number) WHERE chassis_number IS NOT NULL AND is_current_record = true AND is_deleted = false`
- `UQ_vehicle_source_record (dealership_id, source_system, source_record_id) WHERE source_record_id IS NOT NULL`
- `UQ_vehicle_version (dealership_id, vehicle_id, vehicle_version)`
- `UQ_vehicle_merge_source (dealership_id, source_vehicle_id)`

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `manufacturer_id` → `manufacturers(id)` — nullable
- `vehicle_model_id` → `vehicle_models(id)` — nullable
- `supersedes_vehicle_id` → `vehicles(id)` — nullable
- `merged_into_vehicle_id` → `vehicles(id)` — nullable
- `created_by` → `users(id)`
- `updated_by` → `users(id)`
- `verified_by` → `users(id)` — nullable
- `merged_by` → `users(id)` — nullable
- `retired_by` → `users(id)` — nullable
- `deleted_by` → `users(id)` — nullable

### Check Constraints

- `record_version >= 1`
- `vehicle_version >= 1`
- `model_year >= 1886`
- `odometer_value >= 0`
- `number_of_doors >= 0`
- `seating_capacity >= 1`
- Physical dimensions and capacities must be zero or greater.
- `merged_into_vehicle_id <> vehicle_id`
- `supersedes_vehicle_id <> vehicle_id`
- `verification_method IS NOT NULL` when verification status is `VERIFIED`.
- `verified_at IS NOT NULL` when verification status is `VERIFIED`.
- `verified_by IS NOT NULL` when verification status is `VERIFIED`.
- `merged_into_vehicle_id IS NOT NULL` when identity status is `MERGED`.
- `identity_conflict_detected_at IS NOT NULL` when identity status is `IDENTITY_CONFLICT`.

### VIN Constraints

VIN normalization should:

- Convert to uppercase.
- Remove unauthorized separators and whitespace.
- Preserve the original source value separately when required.
- Reject `I`, `O`, and `Q`.
- Require 17 characters where the jurisdiction uses standard VINs.
- Apply checksum validation where applicable.
- Avoid accepting AI-generated VINs without source evidence.

### Odometer Storage

Odometer readings should be append-only in `vehicle_odometer_history`.

Each reading should preserve:

- Vehicle.
- Value.
- Unit.
- Source.
- Evidence.
- Verification status.
- Actor.
- Timestamp.
- Previous verified reading.
- Discrepancy result.

The current Vehicle table may cache the latest accepted odometer value but must not overwrite historical evidence.

### Source Authority Storage

`vehicle_field_authority` should record for each governed field:

- Field name.
- Current authoritative source.
- Source record.
- Authority level.
- Verification state.
- Effective timestamp.
- Last synchronization timestamp.
- Previous authority.
- Conflict status.

### Merge Storage

A Vehicle merge must preserve:

- Source Vehicle.
- Surviving Vehicle.
- Duplicate evidence.
- Matching fields.
- Downstream references checked.
- References reconciled.
- Approved by.
- Merge timestamp.
- Rollback or correction reference where permitted.

Merged records must remain queryable for audit but excluded from normal active searches.

### Storage Strategy

- Use relational storage for canonical identity and normalized specifications.
- Use append-only tables for:
  - Odometer history.
  - Identity-state history.
  - Source history.
  - Verification evidence metadata.
  - Merge history.
  - Audit history.

- Store large documents and images in an encrypted Document Vault.
- Store document hashes and references in relational tables.
- Store flexible OEM or provider specifications in validated JSONB where relational modelling is impractical.
- Do not use JSONB to bypass authoritative-field validation.
- Public search indexes must exclude restricted Vehicle identifiers.
- Vector stores must contain only approved non-sensitive Vehicle descriptions.

### Partitioning

- Partition high-volume tables by `dealership_id`.
- Historical and audit tables may be sub-partitioned by creation year.
- All partitions must preserve tenant isolation.
- Cross-tenant analytics must use governed anonymized or aggregated datasets.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read approved Vehicle specifications for assigned Customer and Opportunity contexts.
- **Sales Manager:** Read Vehicle specifications and verification summary; no unrestricted identity-edit authority.
- **Inventory User:** Create and update permitted Vehicle identity and specification data during acquisition or receipt workflows.
- **Inventory Manager:** Review Vehicle identity, duplicates, and source conflicts.
- **Trade-In Appraiser:** Create or match Customer Vehicles during Trade-In workflows.
- **Data Steward:** Approve identity corrections and Vehicle merges.
- **Compliance User:** Review registration, ownership, fraud, legal, and identity conflicts.
- **Finance User:** Read permitted Vehicle identity fields required for finance processing.
- **Delivery Coordinator:** Read verified Vehicle identity required for delivery.
- **Marketing User:** Read approved Customer-visible Vehicle specifications only.
- **Customer Self-Service User:** Read Customer-visible Vehicle attributes only.
- **AI Vehicle Agent:** Limited extraction, matching, summarization, and recommendation access.
- **Integration Service:** Restricted source-specific create and synchronization access.
- **Audit Service:** Read-only access to immutable Vehicle history and evidence metadata.

### Data Classification

- **General Vehicle Specifications:** `INTERNAL` or approved `PUBLIC`.
- **Vehicle Identifiers:** `CONFIDENTIAL`.
- **Registration and Ownership Evidence:** `RESTRICTED`.
- **Odometer Evidence:** `CONFIDENTIAL`.
- **Source Credentials and Provider Payloads:** `RESTRICTED`.
- **Legal, Fraud, and Compliance Flags:** `RESTRICTED`.

### Restricted Fields

Restricted fields include:

- Full VIN.
- Chassis number.
- Engine number.
- Registration number.
- License-plate number.
- Government document references.
- Identity evidence.
- Evidence hashes.
- Source payloads.
- Legal-hold details.
- Fraud-review details.
- Internal verification notes.
- User and reviewer identifiers where protected.

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 or approved equivalent.
- Full VIN, chassis, registration, engine, evidence references, and restricted source metadata require field-level protection where risk policy requires it.
- Vehicle documents and images must be stored in an encrypted Document Vault.
- Encryption keys must be:
  - Environment-separated.
  - Access-controlled.
  - Rotated.
  - Audited.
  - Recoverable through approved key-management procedures.

### Masking Requirements

Users without full identity permission should see masked values such as:

```text
VIN: *************3456
Chassis: ********9821
Registration: ******5892
```

Customer-facing outputs must not expose full restricted identifiers unless legally and operationally required.

### Tenant Isolation

- Every Vehicle query must enforce `dealership_id`.
- Cross-tenant Vehicle lookup must be prohibited by default.
- Shared Vehicle identity resolution must use an explicitly governed service.
- Stock transfers between tenants must not grant unrestricted access to historical tenant data.
- AI retrieval, analytics, exports, Jobs, integrations, and search indexes must retain tenant scope.
- Cache keys must include tenant identity.
- Event consumers must reject mismatched tenant context.

### Write Authorization

The system must verify:

- User role.
- Tenant.
- Business purpose.
- Current Vehicle state.
- Source authority.
- Record version.
- Required Human Approval.
- Legal-hold status.

A lower-authority source must not overwrite a verified higher-authority field.

### Merge Security

Vehicle merge operations require:

- Authorized Data Steward or equivalent role.
- Duplicate evidence.
- Conflict review.
- Downstream-reference analysis.
- Immutable approval record.
- Transactional update.
- Audit event.
- Protection against cross-tenant merge.
- Protection against merging Vehicles involved in conflicting active transactions without reconciliation.

### Audit Requirements

Every Vehicle create or update operation must preserve:

- Tenant.
- Vehicle.
- Actor or service.
- Changed fields.
- Previous values or hashes.
- New values or hashes.
- Source.
- Source authority.
- Business reason.
- Timestamp.
- Correlation identifier.
- Record version.

Every verification operation must preserve:

- Verification method.
- Evidence references and hashes.
- Fields verified.
- Reviewer.
- Result.
- Timestamp.

Every AI operation must preserve:

- Model reference.
- Prompt or extraction version.
- Input evidence.
- Extracted fields.
- Confidence.
- Human-review status.
- Final accepted values.
- Timestamp.

Every merge operation must preserve:

- Source Vehicle.
- Surviving Vehicle.
- Duplicate evidence.
- Downstream references.
- Approving User.
- Timestamp.

### Legal Hold

When legal hold is active:

- Vehicle evidence must not be deleted.
- Merge may be restricted.
- Archival may be restricted.
- Source history must remain available.
- Audit history must remain immutable.
- Approved corrections must preserve previous values.

### Retention

Vehicle retention must follow:

- Automotive regulations.
- Registration requirements.
- Tax and accounting requirements.
- Contractual requirements.
- Warranty and service requirements.
- Privacy regulations.
- Fraud and compliance requirements.
- Legal-hold instructions.

### Deletion Rules

Hard deletion is prohibited while the Vehicle is referenced by:

- Inventory Record.
- Trade-In.
- Appointment or test drive.
- Quotation.
- Deal.
- Finance Application.
- Financial Contract.
- Vehicle Delivery.
- Market Intelligence evidence.
- Legal hold.
- Audit record.

Soft deletion may be permitted only when:

- The Vehicle was created in error.
- No authoritative workflow relied on it.
- No legal retention applies.
- Required approvals exist.
- Deletion evidence is retained.

### Anonymization and Redaction

Where privacy or policy permits:

- Registration and license-plate data may be redacted.
- Restricted source documents may be removed after retention expiry.
- Full VIN may remain where legally required for transaction history.
- Redaction must not destroy required commercial or legal traceability.
- Vector embeddings and search indexes must be updated after approved redaction.

### Public Repository Restrictions

A public documentation repository must never contain:

- Real Customer Vehicle records.
- Real VIN lists.
- Registration documents.
- License-plate datasets.
- Source-system credentials.
- OEM or provider secrets.
- Real access tokens.
- Unredacted internal evidence.
- Restricted production payloads.
