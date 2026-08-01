# Vehicle

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Vehicle Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Vehicle Object represents the canonical identity and technical characteristics of a physical Vehicle or an approved catalogued Vehicle configuration within an ASOS Tenant.

It defines what the Vehicle is, independently of how a dealership stocks, prices, reserves, allocates, sells, or delivers it.

The Vehicle Object may describe:

- Vehicle Identification Number.
- Chassis and engine identifiers.
- Manufacturer, make, model, trim, and variant.
- Model year and production information.
- Body and physical configuration.
- Powertrain, transmission, and drivetrain.
- Exterior and interior specifications.
- Factory equipment and technical options.
- Odometer observations and evidence.
- Registration and identity-verification evidence.
- Source-system provenance.
- Identity conflicts, corrections, and merges.

A Vehicle must remain identifiable independently of:

- A dealership stock cycle.
- A Customer.
- A Lead.
- An Opportunity.
- An Appointment.
- A Quotation.
- A Trade-In appraisal.
- A Finance Application.
- A Financial Contract.
- A Deal.
- A reservation.
- An allocation.
- A sale.
- A delivery.

A physical Vehicle may participate in multiple historical:

- Inventory Records.
- Trade-In workflows.
- Appointments and test drives.
- Quotations.
- Deals.
- Ownership periods.
- Registration periods.
- Inspection records.
- Service or odometer observations.

### Vehicle and Inventory Boundary

The Vehicle Object must not own dealership-specific commercial Inventory state.

The following information belongs to `InventoryRecord` or another appropriate workflow:

- Dealership stock number.
- Inventory status.
- Commercial availability.
- Branch stock location.
- Parking location.
- Acquisition date.
- Acquisition cost.
- Landed cost.
- Retail price.
- Advertised price.
- Minimum authorized price.
- Discount limits.
- Inventory aging.
- Preparation status.
- Reservation status.
- Allocation status.
- Current Deal assignment.
- Publication status.
- Sale status.
- Delivery status.
- Inventory transfer.
- Inventory retirement.

Vehicle identity does not prove:

- Availability for sale.
- Legal ownership.
- Dealership ownership.
- Reservation.
- Allocation.
- Roadworthiness.
- Insurance.
- Registration validity.
- Finance eligibility.
- Sale completion.
- Delivery completion.

### System Purpose

The Vehicle Object provides stable identity and specification context to:

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

The Vehicle Object is normally an ASOS Canonical Projection.

It does not automatically replace an external legal or operational System of Record for:

- Vehicle registration.
- Legal ownership.
- Manufacturer certification.
- Inspection status.
- Service history.
- Insurance.
- Physical stock.
- Accounting.
- Vehicle delivery.

Every externally authoritative field must preserve:

- Source system.
- Source record.
- Authority category.
- Source timestamp.
- Verification status.
- Evidence reference.
- Synchronization status.
- Record version.

### Canonical Ownership Matrix

| Information | Canonical Owner |
| :--- | :--- |
| VIN and chassis identity | Vehicle |
| Make, model, trim, variant, and model year | Vehicle |
| Engine, battery, transmission, and drivetrain specifications | Vehicle |
| Body type, dimensions, seats, and doors | Vehicle |
| Factory colors and factory options | Vehicle |
| Odometer observations and evidence | Vehicle |
| Registration identity projection | Vehicle |
| Stock number | Inventory Record |
| Commercial availability | Inventory Record |
| Branch and stock location | Inventory Record |
| Acquisition and landed cost | Inventory Record |
| Retail and advertised pricing | Inventory Record |
| Reservation and allocation | Inventory Record |
| Inventory aging | Inventory Record |
| Customer-specific commercial offer | Quotation |
| Trade-In appraisal and acquisition workflow | Trade-In |
| Final commercial terms | Deal |
| Finance terms and decisions | Finance Application or Financial Contract |
| Delivery confirmation | Delivery workflow and configured external authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `vehicle_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Optional Organizational Context

- `dealer_group_id`.
- `originating_dealership_id`.
- `originating_branch_id`.

These fields provide organizational context inside the Tenant.

They do not replace `tenant_id` as the primary isolation boundary.

### Record Classification

- `vehicle_record_type`.
- `identity_status`.
- `verification_status`.
- `data_quality_status`.
- `conflict_status`.

### Vehicle Identity

- `vin`.
- `chassis_number`.
- `engine_number`.
- `manufacturer_serial_number`.
- `registration_number`.
- `license_plate_number`.
- `country_of_registration`.
- `registration_effective_from`.
- `registration_effective_until`.

### Manufacturer and Model

- `manufacturer_id`.
- `vehicle_model_id`.
- `make`.
- `model`.
- `trim`.
- `variant`.
- `model_year`.
- `production_date`.
- `manufacturer_model_code`.
- `market_region`.

### Physical Configuration

- `vehicle_category`.
- `body_type`.
- `number_of_doors`.
- `seating_capacity`.
- `steering_position`.
- `exterior_color`.
- `exterior_color_code`.
- `interior_color`.
- `interior_material`.

### Powertrain and Performance

- `fuel_type`.
- `transmission`.
- `drivetrain`.
- `engine_displacement_cc`.
- `engine_cylinder_count`.
- `engine_power_kw`.
- `engine_power_hp`.
- `engine_torque_nm`.
- `battery_capacity_kwh`.
- `electric_range_km`.
- `combined_fuel_consumption`.
- `emission_standard`.

### Dimensions and Capacity

- `length_mm`.
- `width_mm`.
- `height_mm`.
- `wheelbase_mm`.
- `curb_weight_kg`.
- `gross_vehicle_weight_kg`.
- `cargo_capacity_liters`.
- `fuel_tank_capacity_liters`.

### Equipment and Features

- `factory_options`.
- `safety_features`.
- `comfort_features`.
- `infotainment_features`.
- `driver_assistance_features`.
- `accessibility_features`.
- `equipment_snapshot`.

### Odometer Projection

- `latest_odometer_value`.
- `latest_odometer_unit`.
- `latest_odometer_status`.
- `latest_odometer_recorded_at`.
- `latest_odometer_source`.
- `latest_odometer_evidence_reference`.

The latest odometer fields are a projection.

Historical odometer observations must remain in governed child records.

### Verification and Authority

- `verification_method`.
- `verified_at`.
- `verified_by_actor_type`.
- `verified_by_actor_id`.
- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `field_authority_map`.
- `identity_evidence_references`.

### Merge and Supersession

- `supersedes_vehicle_id`.
- `merged_into_vehicle_id`.
- `merged_at`.
- `merged_by_actor_id`.
- `retired_at`.
- `archived_at`.

### Audit Fields

- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.

### Explicitly Excluded Fields

The following must not be stored as authoritative Vehicle fields:

- `stock_number`.
- `inventory_number`.
- `inventory_status`.
- `availability_status`.
- `current_stock_location`.
- `parking_location`.
- `arrival_date`.
- `days_in_stock`.
- `days_in_inventory`.
- `acquisition_cost_amount`.
- `landed_cost_amount`.
- `retail_price_amount`.
- `advertised_price_amount`.
- `minimum_authorized_price_amount`.
- `maximum_discount_amount`.
- `reservation_id`.
- `allocated_deal_id`.
- `sold_deal_id`.
- `ready_for_sale`.
- `ready_for_delivery`.
- `delivery_status`.
- `sale_status`.

These fields belong to Inventory Record, Quotation, Deal, or another applicable canonical workflow.

---

## 3. Field Definitions

### Identity and Governance Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `vehicle_id` | UUID | Yes | ASOS | Immutable Canonical Vehicle identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `vehicle_record_type` | Enum | Yes | ASOS | Distinguishes a physical Vehicle from an approved catalog configuration. |
| `identity_status` | Enum | Yes | ASOS Workflow State | Canonical identity lifecycle state. |
| `verification_status` | Enum | Yes | ASOS Workflow State | Result of the latest identity-verification workflow. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine state. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Indicates whether material identity conflicts exist. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |
| `dealer_group_id` | UUID | No | Canonical Projection | Optional dealer-group context inside the Tenant. |
| `originating_dealership_id` | UUID | No | Canonical Projection | Dealership where the Vehicle was first observed. |
| `originating_branch_id` | UUID | No | Canonical Projection | Branch where the Vehicle was first observed. |

### Physical Identity Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `vin` | String | Conditional | Verified external evidence | Vehicle Identification Number for a physical Vehicle. |
| `chassis_number` | String | Conditional | Verified external evidence | Chassis identifier where VIN is unavailable or supplemented. |
| `engine_number` | String | No | Verified external evidence | Manufacturer or authority-issued engine identifier. |
| `manufacturer_serial_number` | String | No | Manufacturer source | Manufacturer-issued serial identifier where applicable. |
| `registration_number` | String | No | Registration authority | Current registration identifier. |
| `license_plate_number` | String | No | Registration authority | Current licence-plate identifier. |
| `country_of_registration` | String | No | Registration authority | ISO country code of the registration authority. |
| `registration_effective_from` | Date | No | Registration authority | Start of the registration period. |
| `registration_effective_until` | Date | No | Registration authority | End of the registration period. |

### Manufacturer and Model Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `manufacturer_id` | UUID | No | Approved dictionary | Canonical manufacturer reference. |
| `vehicle_model_id` | UUID | No | Approved catalogue | Canonical model or configuration reference. |
| `make` | String | Yes | Approved source | Manufacturer or brand name. |
| `model` | String | Yes | Approved source | Manufacturer-defined model. |
| `trim` | String | No | Approved source | Manufacturer-defined grade or trim. |
| `variant` | String | No | Approved source | More specific derivative or configuration. |
| `model_year` | Integer | Yes | Approved source | Manufacturer or market-defined model year. |
| `production_date` | Date | No | Manufacturer or evidence | Vehicle production date where known. |
| `manufacturer_model_code` | String | No | Manufacturer source | Manufacturer configuration or model code. |
| `market_region` | String | No | Approved source | Market for which the configuration was produced. |

### Physical Configuration Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `vehicle_category` | Enum | No | Approved source | Passenger, commercial, motorcycle, or special-purpose category. |
| `body_type` | Enum | No | Approved source | Physical body configuration. |
| `number_of_doors` | Integer | No | Approved source | Number of passenger or cargo doors according to approved convention. |
| `seating_capacity` | Integer | No | Approved source | Approved seating capacity. |
| `steering_position` | Enum | No | Approved source | Left-, right-, or centre-hand steering position. |
| `exterior_color` | String | No | Approved source | Normalized exterior colour. |
| `exterior_color_code` | String | No | Manufacturer source | Manufacturer exterior-colour code. |
| `interior_color` | String | No | Approved source | Normalized interior colour. |
| `interior_material` | String | No | Approved source | Interior upholstery or material description. |

### Powertrain Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `fuel_type` | Enum | No | Approved source | Primary propulsion-energy type. |
| `transmission` | Enum | No | Approved source | Transmission configuration. |
| `drivetrain` | Enum | No | Approved source | Driven-wheel or propulsion configuration. |
| `engine_displacement_cc` | Integer | No | Manufacturer or evidence | Engine displacement in cubic centimetres. |
| `engine_cylinder_count` | Integer | No | Manufacturer or evidence | Number of combustion-engine cylinders. |
| `engine_power_kw` | Decimal | No | Manufacturer or evidence | Engine or system power in kilowatts. |
| `engine_power_hp` | Decimal | No | Manufacturer or evidence | Engine or system power in horsepower. |
| `engine_torque_nm` | Decimal | No | Manufacturer or evidence | Maximum torque in Newton metres. |
| `battery_capacity_kwh` | Decimal | No | Manufacturer or evidence | Gross or approved battery capacity. |
| `electric_range_km` | Decimal | No | Approved test standard | Electric driving range with test-cycle metadata. |
| `combined_fuel_consumption` | Decimal | No | Approved test standard | Combined consumption with unit and test-cycle metadata. |
| `emission_standard` | String | No | Manufacturer or authority | Applicable emissions standard. |

### Odometer Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `latest_odometer_value` | Decimal | No | Accepted projection | Latest accepted odometer reading. |
| `latest_odometer_unit` | Enum | No | Evidence | Kilometres or miles. |
| `latest_odometer_status` | Enum | No | Verification workflow | Current trust or exception status. |
| `latest_odometer_recorded_at` | Timestamp | No | Evidence | Time the reading was observed. |
| `latest_odometer_source` | String | No | Provenance | Source of the accepted reading. |
| `latest_odometer_evidence_reference` | String | No | Evidence | Reference to inspection, document, source record, or image evidence. |

### Provenance Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `source_system` | String | Yes | Integration Context | Source from which the canonical projection originated. |
| `source_record_id` | String | No | External source | Identifier in the source system. |
| `source_authority` | Enum | Yes | Governance | Authority classification of the source. |
| `source_updated_at` | Timestamp | No | External source | Source-system update timestamp. |
| `last_synced_at` | Timestamp | No | ASOS | Latest successful synchronization time. |
| `last_sync_status` | Enum | Yes | ASOS | Current synchronization state. |
| `field_authority_map` | JSON Object | Yes | Governance | Field-level source and authority metadata. |
| `identity_evidence_references` | Array | No | Evidence repository | References supporting identity and verification. |

---

## 4. Enumerations

### VehicleRecordType

- `PHYSICAL_VEHICLE`
- `CATALOG_CONFIGURATION`

An `InventoryRecord` must reference a `PHYSICAL_VEHICLE`.

A `CATALOG_CONFIGURATION` may support:

- Factory-order exploration.
- Vehicle matching.
- Quotation preparation.
- Market Intelligence.
- Customer preference modelling.

It must not be represented as a physical available unit.

### VehicleIdentityStatus

- `DRAFT`
- `IDENTIFIED`
- `VERIFIED`
- `ACTIVE`
- `IDENTITY_CONFLICT`
- `MERGED`
- `RETIRED`
- `ARCHIVED`

`ACTIVE` means that the identity is permitted for use by approved downstream workflows.

It does not mean commercially available for sale.

### VehicleVerificationStatus

- `NOT_VERIFIED`
- `VERIFICATION_PENDING`
- `VERIFIED`
- `FAILED`
- `CONFLICT_DETECTED`
- `MANUAL_REVIEW_REQUIRED`
- `EXPIRED`

### VehicleVerificationMethod

- `OEM_BUILD_SHEET`
- `VIN_DOCUMENT_MATCH`
- `REGISTRATION_DOCUMENT`
- `PHYSICAL_INSPECTION`
- `DMS_VERIFIED`
- `AUTHORIZED_PROVIDER`
- `STOCK_TRANSFER_EVIDENCE`
- `TRADE_IN_EVIDENCE`
- `MANUAL_SPECIALIST_REVIEW`
- `MULTI_SOURCE_MATCH`
- `OTHER`

### VehicleCategory

- `PASSENGER`
- `LIGHT_COMMERCIAL`
- `HEAVY_COMMERCIAL`
- `MOTORCYCLE`
- `SPECIAL_PURPOSE`
- `OTHER`
- `UNKNOWN`

### VehicleBodyType

- `SEDAN`
- `HATCHBACK`
- `SUV`
- `CROSSOVER`
- `COUPE`
- `CONVERTIBLE`
- `WAGON`
- `PICKUP`
- `VAN`
- `MINIVAN`
- `BUS`
- `TRUCK`
- `MOTORCYCLE`
- `OTHER`
- `UNKNOWN`

### VehicleFuelType

- `GASOLINE`
- `DIESEL`
- `HYBRID`
- `PLUG_IN_HYBRID`
- `BATTERY_ELECTRIC`
- `HYDROGEN`
- `LPG`
- `CNG`
- `FLEX_FUEL`
- `OTHER`
- `UNKNOWN`

### VehicleTransmission

- `AUTOMATIC`
- `MANUAL`
- `CVT`
- `DCT`
- `SINGLE_SPEED`
- `SEMI_AUTOMATIC`
- `OTHER`
- `UNKNOWN`

### VehicleDrivetrain

- `FRONT_WHEEL_DRIVE`
- `REAR_WHEEL_DRIVE`
- `ALL_WHEEL_DRIVE`
- `FOUR_WHEEL_DRIVE`
- `OTHER`
- `UNKNOWN`

### SteeringPosition

- `LEFT_HAND_DRIVE`
- `RIGHT_HAND_DRIVE`
- `CENTRE`
- `UNKNOWN`

### OdometerUnit

- `KILOMETERS`
- `MILES`

### OdometerStatus

- `UNVERIFIED`
- `VERIFIED`
- `EXEMPT`
- `NOT_ACTUAL`
- `DISCREPANCY_DETECTED`
- `ROLLOVER_DETECTED`
- `REPLACED`
- `MANUAL_REVIEW_REQUIRED`

### VehicleSourceAuthority

- `OEM_VERIFIED`
- `GOVERNMENT_VERIFIED`
- `INSPECTION_VERIFIED`
- `DMS_VERIFIED`
- `AUTHORIZED_PROVIDER`
- `DEALERSHIP_REPORTED`
- `CUSTOMER_REPORTED`
- `AI_EXTRACTED`
- `MANUAL_ENTRY`
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

### Tenant Rules

- `tenant_id` is required and immutable.
- Every Vehicle child record must use the same `tenant_id` as its parent Vehicle.
- `dealer_group_id`, `originating_dealership_id`, and `originating_branch_id` must belong to the authenticated Tenant.
- Cross-Tenant Vehicle access is prohibited unless an approved and auditable sharing or transfer mechanism exists.
- Tenant context must come from the authenticated security context.

### Identity Rules

For `PHYSICAL_VEHICLE`:

- At least one approved physical identity must exist before entering `IDENTIFIED`.
- The preferred physical identity is VIN where the market uses a standard VIN.
- Chassis number may be used where VIN is unavailable or supplemented.
- A VIN must not be created from AI inference.
- A verified VIN must not be changed through an ordinary update.
- Registration and licence-plate identifiers must not replace VIN or chassis identity.

For `CATALOG_CONFIGURATION`:

- VIN and chassis number must be null.
- Make, model, model year, and configuration reference must be present.
- The record must not be linked to an active Inventory Record.
- The record must not be presented as a physically available unit.

### VIN Rules

When VIN is populated:

- It must contain exactly 17 characters unless an approved market-specific exception exists.
- It must be normalized to uppercase.
- It must exclude the letters `I`, `O`, and `Q`.
- Check-digit validation must be applied where required.
- Original source formatting and evidence must remain traceable.
- A verified VIN must be unique among active physical Vehicles inside the Tenant.

### Chassis and Engine Rules

- Chassis number must preserve source formatting and normalized formatting.
- An active verified chassis number should be unique inside the Tenant.
- Engine number must not be treated as the sole universal Vehicle identity.
- Engine replacement must use a governed historical update rather than silently rewriting evidence.

### Manufacturer and Specification Rules

- `make` and `model` are required.
- `model_year` is required.
- `model_year` must fall within an approved historical and future range.
- A future model year normally must not exceed the current year plus two without evidence.
- `production_date` must not be materially inconsistent with the model year.
- Trim and variant must be valid for the make, model, model year, and market where authoritative data exists.
- Technical measurements must not be negative.
- Seating capacity must be greater than zero when populated.
- Door count must not be negative.
- Battery capacity must be populated only for applicable propulsion types.
- Engine displacement must not be required for battery-electric Vehicles.

### Odometer Rules

- Odometer value must be zero or greater.
- Every odometer observation must preserve source, time, unit, evidence, and verification status.
- A later verified reading must not be lower than an earlier verified reading without:
  - Approved discrepancy classification.
  - Odometer replacement evidence.
  - Rollover evidence.
  - Authoritative correction.
- Unit conversion must preserve the original reading and unit.
- AI extraction may suggest an odometer value but cannot verify it independently.

### Authority and Conflict Rules

- Lower-authority sources must not silently overwrite higher-authority verified values.
- Material identity conflicts must create a controlled review workflow.
- Conflicting VIN, chassis, registration, manufacturer, or odometer evidence must be preserved.
- `field_authority_map` must identify authority for material fields.
- Missing or stale evidence must reduce verification status.
- A synchronization failure must not silently clear an existing authoritative value.
- Every accepted authoritative change must preserve immutable history.

### Duplicate and Merge Rules

- Duplicate detection may create a candidate match.
- Similarity alone must not merge Vehicles.
- A Vehicle merge requires an Authoritative Human Decision.
- The surviving Vehicle must be identified.
- All dependent records must be reconciled.
- Source identifiers and evidence from the merged Vehicle must remain traceable.
- Circular merge relationships are prohibited.
- A merged Vehicle must not remain active.
- A merged Vehicle must not return to active use through a normal update.

### Inventory Boundary Rules

Vehicle fields must not be used as the authoritative source for:

- Stock availability.
- Stock location.
- Pricing.
- Reservation.
- Allocation.
- Inventory aging.
- Sale.
- Delivery.

Customer-visible availability must come from an active Inventory Record and its configured authoritative source.

Customer-visible pricing must come from an approved Inventory pricing context, Quotation, or Deal workflow.

### Concurrency and Idempotency

- Every mutating update must validate `record_version`.
- Stale updates must be rejected or routed to conflict resolution.
- Retryable creation operations must support idempotency.
- External write Commands must use an approved `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate source observations must not create duplicate Vehicles.

### Human Approval Requirements

Authorized Human Review is required for:

- Changing a verified VIN.
- Resolving a material identity conflict.
- Merging duplicate Vehicles.
- Correcting a verified odometer discrepancy.
- Approving cross-Tenant Vehicle reconciliation.
- Restoring an incorrectly retired or archived identity.
- Overriding verified manufacturer or registration evidence.

---

## 6. State Machine

### Allowed States

```text
DRAFT
IDENTIFIED
VERIFIED
ACTIVE
IDENTITY_CONFLICT
MERGED
RETIRED
ARCHIVED
```

### Allowed Transitions

```text
DRAFT → IDENTIFIED
DRAFT → RETIRED

IDENTIFIED → VERIFIED
IDENTIFIED → IDENTITY_CONFLICT
IDENTIFIED → RETIRED

VERIFIED → ACTIVE
VERIFIED → IDENTITY_CONFLICT
VERIFIED → RETIRED

ACTIVE → IDENTITY_CONFLICT
ACTIVE → MERGED
ACTIVE → RETIRED

IDENTITY_CONFLICT → VERIFIED
IDENTITY_CONFLICT → MERGED
IDENTITY_CONFLICT → RETIRED

MERGED → ARCHIVED
RETIRED → ARCHIVED
```

### Forbidden Ordinary Transitions

```text
DRAFT → ACTIVE
DRAFT → VERIFIED
DRAFT → MERGED

IDENTIFIED → ACTIVE
IDENTIFIED → MERGED without approved duplicate evidence

VERIFIED → DRAFT
ACTIVE → DRAFT
ACTIVE → IDENTIFIED

MERGED → ACTIVE
MERGED → VERIFIED

RETIRED → ACTIVE

ARCHIVED → ACTIVE
ARCHIVED → IDENTIFIED
ARCHIVED → VERIFIED
```

Corrections to an incorrect terminal transition require a separate governed correction or restoration workflow.

### Entering IDENTIFIED

Requires:

- Valid Tenant context.
- Minimum identity data.
- Make, model, and model year.
- Known source and source authority.
- Initial duplicate checks.
- No blocking validation errors.

For a physical Vehicle, a VIN, chassis number, or approved jurisdictional identifier must exist.

For a catalog configuration, an approved model or configuration reference must exist.

### Entering VERIFIED

Requires:

- Approved identity evidence.
- Satisfied verification policy.
- Resolved material conflicts.
- Recorded verification method.
- Recorded verifier.
- Recorded verification timestamp.
- Verified source provenance.

### Entering ACTIVE

Requires:

- Verified identity.
- No unresolved identity conflict.
- Required specifications passing validation.
- Permitted use by downstream workflows.

`ACTIVE` does not mean:

- Available.
- Reserved.
- Allocated.
- Sold.
- Delivered.

### Entering IDENTITY_CONFLICT

Requires:

- Material disagreement between identity or specification sources.
- Recorded affected fields.
- Recorded competing evidence.
- Workflow restrictions where required.
- Human Review Task where required.

### Entering MERGED

Requires:

- Confirmed duplicate identity.
- Surviving `merged_into_vehicle_id`.
- Authorized Human Decision.
- Reconciliation of dependent references.
- Preserved evidence.
- Immutable audit record.

### Entering RETIRED

Requires:

- Approved retirement reason.
- Confirmation that retirement is not being used to represent a sale or delivery.
- Dependency and retention checks.
- Recorded authority.

### Entering ARCHIVED

Requires:

- Existing `MERGED` or `RETIRED` state.
- Retention checks.
- Dependency checks.
- Legal-hold checks.
- Audit-preservation checks.

### Terminal States

- `MERGED`
- `ARCHIVED`

A terminal Vehicle may be restored only through an approved correction process, not through an ordinary lifecycle update.

### Transition Evidence

Every transition must preserve:

- Previous state.
- New state.
- Transition reason.
- Actor.
- Authority.
- Applicable policy.
- Record version.
- Evidence references.
- Timestamp.
- Related Event.
- Related review or Decision.

---

## 7. Relationships

### Tenant Relationship

- Every Vehicle belongs to exactly one `tenant_id`.
- A Vehicle may be associated with multiple dealerships or branches inside the same Tenant.
- Organizational association does not change Vehicle identity.

### Inventory Record

- A physical Vehicle may exist without an active Inventory Record.
- A physical Vehicle may have multiple historical Inventory Records.
- Only Inventory Record may define current dealership:
  - Stock number.
  - Availability.
  - Location.
  - Pricing context.
  - Preparation status.
  - Reservation.
  - Allocation.
  - Sale context.
  - Delivery context.
  - Inventory aging.
- A catalog configuration must not have an active physical Inventory Record.

### Trade-In

- A physical Vehicle may participate in multiple historical Trade-In workflows.
- Trade-In owns:
  - Appraisal.
  - Customer ownership claim.
  - Lien and payoff workflow.
  - Acquisition Recommendation.
  - Trade-In commercial approval.
- Vehicle owns the underlying identity and specifications.

### Appointment and Test Drive

- A physical Vehicle may be associated with multiple Appointments or test-drive records.
- Appointment status does not modify Vehicle identity or Inventory availability.

### Quotation

- A Quotation may reference:
  - A physical Vehicle.
  - An Inventory Record.
  - An approved catalog configuration.
- Customer-specific price belongs to Quotation, not Vehicle.

### Opportunity

- An Opportunity may contain one or more Vehicle preferences or matches.
- Vehicle matching does not reserve or allocate a Vehicle.

### Finance Application

- A Finance Application may reference the selected Vehicle or configuration.
- Finance eligibility and lender decisions do not belong to Vehicle.

### Financial Contract

- A Financial Contract may reference the financed Vehicle.
- Contract status does not modify Vehicle identity state.

### Deal

- A Deal may reference a Vehicle and Inventory Record.
- Sale and delivery outcomes belong to Deal, Inventory Record, and the relevant external authority.
- Deal completion must not change Vehicle identity to a sale status.

### Interaction

- Interactions may reference a Vehicle discussed with a Customer.
- Interaction content is not authoritative Vehicle evidence unless accepted through a governed workflow.

### Market Intelligence

- Market Intelligence may reference:
  - Make.
  - Model.
  - Trim.
  - Configuration.
  - Physical Vehicle.
- Market evidence must not silently overwrite verified Vehicle specifications.

### Vehicle Child Records

A Vehicle may own or govern:

- External references.
- Identity evidence references.
- Registration-history projections.
- Odometer observations.
- Specification-source records.
- Feature records.
- Verification records.
- Conflict records.
- Merge history.
- Audit records.

### Merge Relationship

A merged Vehicle must contain:

```text
merged_into_vehicle_id
```

The surviving Vehicle must preserve a merge-history record for every merged Vehicle.

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

The following are required Vehicle Event concepts and do not replace the Event Catalog.

### Vehicle Identity Event Concepts

- Vehicle record created.
- Vehicle identified.
- Vehicle verification requested.
- Vehicle identity verified.
- Vehicle verification failed.
- Vehicle identity conflict detected.
- Vehicle identity conflict resolved.
- Vehicle identity corrected.
- Vehicle merged.
- Vehicle retired.
- Vehicle archived.

### Specification Event Concepts

- Vehicle specifications updated.
- Manufacturer information corrected.
- Vehicle configuration updated.
- Factory equipment updated.
- Registration projection updated.
- Registration conflict detected.

### Odometer Event Concepts

- Odometer observation recorded.
- Odometer observation verified.
- Odometer discrepancy detected.
- Odometer discrepancy resolved.
- Odometer replacement recorded.

### Source and Synchronization Event Concepts

- External Vehicle reference linked.
- Vehicle source observation received.
- Vehicle synchronization completed.
- Vehicle synchronization failed.
- Vehicle reconciliation required.
- Vehicle reconciliation completed.

### Derived Intelligence Event Concepts

- Vehicle duplicate candidate detected.
- Vehicle specification extraction completed.
- Vehicle match attributes generated.
- Vehicle data-quality issue detected.

Derived Intelligence Events must not imply authoritative verification.

### Producer Rules

- Vehicle Domain Service publishes accepted Vehicle canonical and workflow-state changes.
- Integration services may publish source-observation Events.
- AI Agents may publish Agent-run, extraction, or Recommendation Events.
- AI Agents must not publish authoritative identity-verification, merge, registration-confirmation, sale, availability, or delivery Events merely because they suggested a change.

### Event Requirements

Every material Vehicle Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `vehicle_id`.
- Aggregate type.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Evidence references.
- Security classification.

Events are immutable.

Corrections, reversals, and cancellations must use new Events linked to the original Event.

The Event Backbone may deliver an Event more than once.

Consumers must safely prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Extracting Vehicle data from approved documents.
- Normalizing make, model, trim, and variant.
- Identifying possible VIN or chassis patterns.
- Suggesting duplicate candidates.
- Detecting identity conflicts.
- Detecting specification inconsistencies.
- Classifying body type and powertrain.
- Comparing source records.
- Detecting odometer anomalies.
- Summarizing Vehicle evidence.
- Generating Vehicle-match attributes.
- Identifying missing specifications.
- Preparing Human Review context.
- Supporting Vehicle matching.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create a VIN from inference.
- Verify Vehicle identity.
- Change a verified VIN.
- Resolve a material identity conflict.
- Merge Vehicle records.
- Confirm legal ownership.
- Confirm registration validity.
- Confirm roadworthiness.
- Confirm physical Inventory availability.
- Reserve or allocate a Vehicle.
- Set Customer-visible pricing.
- Mark a Vehicle sold.
- Confirm Vehicle delivery.
- Delete authoritative evidence.
- Bypass deterministic validation.
- Access Vehicle data outside the authorized Tenant.

### AI Extraction Requirements

Every material AI extraction must preserve:

- Extracted field.
- Suggested value.
- Source reference.
- Source timestamp.
- Evidence location.
- Model version.
- Prompt version.
- Confidence where meaningful.
- Authority classification.
- Review requirement.
- Generation timestamp.
- Expiration or freshness metadata.

### Acceptance of Extracted Fields

A configured policy may determine whether an extracted field:

- Is rejected.
- Creates a Human Review Task.
- Is stored as unverified Derived Intelligence.
- Is accepted as a low-risk Canonical Projection.
- Requires authoritative external evidence.

AI confidence alone must not establish:

- VIN.
- Legal ownership.
- Registration.
- Odometer verification.
- Inspection status.
- Availability.
- Price.
- Reservation.
- Sale.
- Delivery.

### Human Approval Requirements

Authorized Human Approval is required for:

- Verified VIN correction.
- Chassis-identity conflict resolution.
- Duplicate Vehicle merge.
- Verified odometer-discrepancy resolution.
- Cross-Tenant reconciliation.
- Restoration after incorrect retirement or archival.
- Material override of verified manufacturer evidence.

### AI Context

Vehicle context supplied to AI should distinguish:

- Verified identity.
- Unverified source data.
- Conflicting data.
- Canonical Projection.
- Derived Intelligence.
- Inventory context.
- Commercial pricing context.
- External Confirmation.

Vehicle identity must not be combined with stale Inventory availability in a way that implies current stock availability.

### Explainability

Material AI Vehicle outputs must explain:

- Evidence used.
- Source authority.
- Data freshness.
- Material conflicts.
- Assumptions.
- Confidence where meaningful.
- Recommended review.
- Prohibited conclusions.

---

## 10. API Contract

Detailed API Schemas and errors will become authoritative in the API Contracts Catalog.

This section defines required Vehicle API behaviour.

### REST Resources

```text
GET    /api/v1/vehicles
POST   /api/v1/vehicles
GET    /api/v1/vehicles/{vehicle_id}
PATCH  /api/v1/vehicles/{vehicle_id}

POST   /api/v1/vehicles/{vehicle_id}/external-references
POST   /api/v1/vehicles/{vehicle_id}/identity-evidence
POST   /api/v1/vehicles/{vehicle_id}/verification-requests
POST   /api/v1/vehicles/{vehicle_id}/odometer-observations
POST   /api/v1/vehicles/{vehicle_id}/conflicts
POST   /api/v1/vehicles/{vehicle_id}/conflicts/{conflict_id}/resolution
POST   /api/v1/vehicles/{vehicle_id}/merge
POST   /api/v1/vehicles/{vehicle_id}/retirement
GET    /api/v1/vehicles/{vehicle_id}/history
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- A client must not be allowed to override `tenant_id` in a request body.
- Dealership and branch context must be validated against authorized Tenant scope.
- Cross-Tenant retrieval must be blocked by default.

### Example Create Request

```json
{
  "vehicle_record_type": "PHYSICAL_VEHICLE",
  "vin": "1HGCM82664A123456",
  "make": "Honda",
  "model": "Civic",
  "trim": "Touring",
  "variant": "1.5T CVT",
  "model_year": 2026,
  "market_region": "EGY",
  "body_type": "SEDAN",
  "fuel_type": "GASOLINE",
  "transmission": "CVT",
  "drivetrain": "FRONT_WHEEL_DRIVE",
  "exterior_color": "Platinum White Pearl",
  "interior_color": "Black",
  "source": {
    "source_system": "DMS",
    "source_record_id": "DMS-VEH-987456",
    "source_authority": "DMS_VERIFIED"
  },
  "origin": {
    "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
    "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38"
  }
}
```

### Example Response

```json
{
  "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
  "vehicle_record_type": "PHYSICAL_VEHICLE",
  "vin": "1HGCM82664A123456",
  "make": "Honda",
  "model": "Civic",
  "trim": "Touring",
  "model_year": 2026,
  "identity_status": "IDENTIFIED",
  "verification_status": "VERIFICATION_PENDING",
  "data_quality_status": "INCOMPLETE",
  "conflict_status": "NONE",
  "record_version": 1,
  "created_at": "2026-08-01T17:00:00Z"
}
```

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Evidence requirements.
- Lifecycle validation.
- Conflict checks.
- Human Approval where required.
- Audit recording.
- Event publication after accepted change.

### Optimistic Concurrency

Updates must use an approved concurrency mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response.

### Idempotency

Retryable creation and command operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate:

- Vehicles.
- Odometer observations.
- Verification requests.
- Merge operations.
- External updates.

### Merge Request Requirements

A Vehicle merge request must include:

- Surviving Vehicle.
- Vehicle to be merged.
- Duplicate evidence.
- Conflict summary.
- Authorized Human Decision.
- Record versions.
- Dependent-record impact.
- Audit reason.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_VEHICLE_CANDIDATE`
- `VIN_CONFLICT`
- `CHASSIS_CONFLICT`
- `IDENTITY_EVIDENCE_REQUIRED`
- `HUMAN_APPROVAL_REQUIRED`
- `FIELD_AUTHORITY_VIOLATION`
- `INVALID_LIFECYCLE_TRANSITION`
- `RECORD_MERGED`
- `RECORD_ARCHIVED`
- `EXTERNAL_CONFIRMATION_PENDING`
- `RECONCILIATION_REQUIRED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Field authority.
- Concurrency.
- Evidence.
- lifecycle.
- approval.
- audit.

GraphQL resolvers must not bypass Vehicle Domain Service or deterministic policy controls.

---

## 11. Database Design

### Recommended Tables

```text
vehicles
vehicle_external_references
vehicle_identity_evidence
vehicle_registrations
vehicle_odometer_observations
vehicle_specification_sources
vehicle_features
vehicle_conflicts
vehicle_verification_records
vehicle_merge_history
vehicle_derived_attributes
vehicle_data_quality_issues
vehicle_audit_log
```

### Vehicles Table

The `vehicles` table should contain:

- Canonical identifiers.
- Tenant context.
- Record classification.
- Current canonical identity.
- Current accepted specifications.
- Current lifecycle state.
- Current verification status.
- Current data-quality status.
- Current conflict status.
- Latest odometer projection.
- Current source and synchronization status.
- Record version.
- Audit timestamps.

Historical observations must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (vehicle_id)
```

### Tenant Protection

Every Vehicle-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced through:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_vehicles_tenant_identity_status
  (tenant_id, identity_status)

idx_vehicles_tenant_make_model_year
  (tenant_id, make, model, model_year)

idx_vehicles_tenant_vin
  (tenant_id, vin)

idx_vehicles_tenant_chassis
  (tenant_id, chassis_number)

idx_vehicle_external_refs
  (tenant_id, source_system, source_record_id)

idx_vehicle_odometer_history
  (tenant_id, vehicle_id, recorded_at)

idx_vehicle_conflicts_open
  (tenant_id, vehicle_id, conflict_status)

idx_vehicle_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended active-record constraints include:

```text
UNIQUE (tenant_id, vin)
```

when VIN is populated and the record is not merged or archived.

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

when the external source guarantees identifier uniqueness.

Chassis uniqueness must be configured according to the applicable jurisdiction and source quality.

### Physical and Catalog Constraints

- A physical Vehicle may have VIN or chassis identity.
- A catalog configuration must not have a VIN.
- An active Inventory Record must not reference a catalog configuration.
- Database constraints or Domain validation must enforce these rules.

### Odometer History

`vehicle_odometer_observations` should preserve:

- Observation identifier.
- `tenant_id`.
- `vehicle_id`.
- Value.
- Unit.
- Status.
- Recorded timestamp.
- Source.
- Evidence reference.
- Actor.
- Verification state.
- Supersession or correction reference.

Odometer history must be append-only except for governed redaction requirements.

### Registration History

`vehicle_registrations` should preserve:

- Registration identifier.
- Plate identifier.
- Country or authority.
- Effective period.
- Source.
- Evidence.
- Verification status.
- Supersession history.

A current registration projection must not erase historical registrations.

### Merge History

`vehicle_merge_history` should preserve:

- `merge_id`.
- `tenant_id`.
- Surviving Vehicle.
- Merged Vehicle.
- Evidence.
- Authorized Human Decision.
- Record versions.
- Dependent-record reconciliation status.
- Timestamp.
- Related Events.
- Reversal status where applicable.

### Derived Attributes

Derived Vehicle attributes should remain separate from verified identity fields.

Each derived record should preserve:

- Output type.
- Output value.
- Model or algorithm version.
- Prompt version where applicable.
- Input versions.
- Evidence references.
- Confidence.
- Generation timestamp.
- Expiration timestamp.

### Audit Storage

Vehicle audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Audit storage must preserve secure hashes instead of raw sensitive evidence where full retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Retention class.
- Event or audit time.

Partitioning must not weaken Tenant isolation.

### Hard Deletion

A Vehicle referenced by any of the following must not be hard-deleted:

- Inventory Record.
- Trade-In.
- Appointment.
- Quotation.
- Finance Application.
- Financial Contract.
- Deal.
- Delivery record.
- Audit evidence.
- External Confirmation.

Retirement, merge, archival, or lawful controlled redaction must be used instead.

---

## 12. Security

### Security Classification

Recommended Vehicle classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `VEHICLE_IDENTIFIER` | VIN, chassis number |
| `SENSITIVE_ASSET_IDENTIFIER` | Engine number, registration number, licence plate |
| `TECHNICAL_SPECIFICATION` | Make, model, dimensions, engine, battery |
| `EVIDENCE` | Registration, inspection, build sheet, odometer evidence |
| `COMMERCIAL_REFERENCE` | Relationship to Inventory Records, Quotations, and Deals |
| `DERIVED_INTELLIGENCE` | Match attributes, duplicate candidates, anomaly scores |
| `AUDIT_EVIDENCE` | Actor, authority, corrections, merges, source history |

### Authentication

Every Vehicle operation requires an authenticated User or service identity.

Anonymous access to Tenant Vehicle records is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Role.
- Resource.
- Requested action.
- Field classification.
- Workflow state.
- Related Inventory Record.
- Related Trade-In or Deal.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access Vehicle specifications required for:

- Customer consultation.
- Vehicle matching.
- Assigned Opportunities.
- Approved Quotations.
- Appointments and test drives.

A Sales Consultant must not independently:

- Change verified VIN.
- Resolve identity conflicts.
- Merge Vehicles.
- Change registration evidence.
- Confirm Inventory availability through Vehicle.

#### Sales Manager

May access Vehicles inside approved organizational scope.

Manager access does not automatically authorize:

- Verified identity override.
- Cross-Tenant reconciliation.
- Evidence deletion.
- Registration correction.
- Vehicle merge.

#### Inventory Specialist

May access Vehicle identity required for Inventory workflows.

Inventory Specialists must change stock state through Inventory Record, not Vehicle.

#### Trade-In Specialist

May access identity and technical evidence required for an approved Trade-In appraisal.

#### Data Steward

May review:

- Duplicate candidates.
- Source conflicts.
- Identity quality.
- Merge evidence.

Final merge and verified identity correction require configured authority.

#### Compliance or Legal Reviewer

May access restricted Vehicle evidence required for an assigned review.

#### AI Agent

May access only the minimum Vehicle context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Logged.
- Time-limited where appropriate.
- Restricted by field classification.
- Prevented from retrieving cross-Tenant data.

### Encryption

- Vehicle data in transit must use approved encryption.
- Vehicle data at rest must use approved encryption.
- Sensitive identifiers and evidence may require field-level encryption or tokenization.
- Encryption keys must not be stored in source code or Prompts.
- Evidence repositories must use approved access controls.

### Masking

Interfaces, Logs, analytics, and AI context should mask sensitive identifiers where full values are unnecessary.

Examples:

```text
VIN: *************3456
Chassis: ********9821
Registration: ******5892
Plate: ***-1234
```

Authorized operational interfaces may display full values only where required.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- APIs.
- Search.
- Vector retrieval.
- Events.
- Caches.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Evidence Protection

Identity, registration, inspection, and odometer evidence must:

- Use controlled storage.
- Preserve integrity hashes where required.
- Restrict access.
- Preserve provenance.
- Prevent unauthorized deletion.
- Follow retention and legal-hold requirements.

### Audit Requirements

Material Vehicle actions must record:

- `tenant_id`.
- `vehicle_id`.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Record version.
- Source.
- Authority category.
- Evidence.
- Human Decision.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Command.
- External Confirmation where applicable.

### Security Events

ASOS must detect and record:

- Cross-Tenant Vehicle access attempts.
- Unauthorized VIN changes.
- Unauthorized merge attempts.
- Evidence tampering.
- Suspicious bulk Vehicle export.
- AI retrieval outside approved scope.
- Repeated identity-conflict overrides.
- Odometer evidence manipulation.
- Tenant-scope bypass attempts.
- Audit-log tampering attempts.

### Availability and Pricing Protection

Vehicle identity APIs must not be used as the authoritative source for:

- Availability.
- Pricing.
- Reservation.
- Allocation.
- Sale.
- Delivery.

Any Customer-facing claim about these matters must use the appropriate Inventory Record, Quotation, Deal, or External Confirmation.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Inventory Record](./InventoryRecord.md)

---

## Current Status

This document is the approved Canonical Vehicle baseline.

Vehicle identity and specifications remain separate from dealership Inventory and commercial state.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
