# Appointment

## 1. Object Purpose

### Business Purpose

The Appointment object represents a scheduled interaction between a Customer and the dealership for a defined sales, service-to-sales, finance, inspection, test-drive, documentation, or delivery purpose.

It provides the dealership with a controlled process for:

- Scheduling Customer visits and remote consultations.
- Coordinating Sales Consultants, Appraisers, Finance Users, and Delivery Coordinators.
- Reserving Vehicles, locations, and operational resources.
- Confirming Customer attendance.
- Sending authorized reminders.
- Recording check-in and check-out.
- Tracking rescheduling, cancellation, and no-show outcomes.
- Capturing the commercial result of the interaction.
- Triggering the next required sales action.

An Appointment does not itself confirm a Vehicle sale, Trade-In valuation, finance approval, signed contract, or completed delivery. It coordinates the interaction through which those outcomes may occur.

### System Purpose

The Appointment object is the canonical scheduling and attendance aggregate for Customer-facing dealership interactions.

It connects:

- Customer
- Lead
- Qualified Lead
- Opportunity
- Vehicle
- Trade-In
- Finance Application
- Deal
- Delivery
- Assigned User
- Dealership Location
- Communication Channel
- Calendar Provider
- Appointment Resource

The Appointment provides the authoritative scheduling state used by:

- Sales-consultation workflows.
- Test-drive scheduling.
- Trade-In inspection scheduling.
- Finance consultations.
- Contract-signing sessions.
- Vehicle-delivery coordination.
- Customer reminder workflows.
- Calendar synchronization.
- Resource-conflict detection.
- Attendance and no-show analytics.
- Opportunity and Customer Journey updates.

Appointment creation, rescheduling, cancellation, and external calendar synchronization must be idempotent.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `appointment_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — required)
  - `lead_id` (UUIDv4 — optional)
  - `qualified_lead_id` (UUIDv4 — optional)
  - `opportunity_id` (UUIDv4 — optional)
  - `vehicle_id` (UUIDv4 — optional)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — optional)
  - `deal_id` (UUIDv4 — optional)
  - `assigned_user_id` (UUIDv4 — required)
  - `created_by` (UUIDv4 — required)
  - `dealership_location_id` (UUIDv4 — optional)
  - `appointment_resource_id` (UUIDv4 — optional)
  - `supersedes_appointment_id` (UUIDv4 — optional)
  - `rescheduled_to_appointment_id` (UUIDv4 — optional)

### Appointment Classification

- `appointment_type`
- `status`
- `priority`
- `channel`
- `confirmation_status`
- `attendance_status`
- `outcome`
- `source`

### Scheduling Fields

- `scheduled_start_at`
- `scheduled_end_at`
- `timezone`
- `duration_minutes`
- `arrival_window_start_at`
- `arrival_window_end_at`
- `check_in_at`
- `actual_start_at`
- `actual_end_at`
- `check_out_at`

### Location and Channel Fields

- `location_type`
- `location_name`
- `address_line_1`
- `address_line_2`
- `city`
- `region`
- `postal_code`
- `country_code`
- `meeting_url`
- `phone_number`
- `arrival_instructions`

### Customer and Participant Fields

- `customer_name_snapshot`
- `customer_preferred_language`
- `customer_contact_channel`
- `customer_notes`
- `participant_user_ids`
- `external_participants`
- `guest_count`
- `special_assistance_required`
- `special_assistance_notes`

### Vehicle and Resource Fields

- `vehicle_required`
- `vehicle_id`
- `vehicle_snapshot`
- `test_drive_required`
- `test_drive_route_id`
- `driver_license_required`
- `driver_license_verified`
- `appointment_resource_id`
- `resource_snapshot`

### Confirmation and Reminder Fields

- `confirmation_status`
- `confirmation_requested_at`
- `confirmed_at`
- `confirmation_channel`
- `reminder_policy`
- `reminder_schedule`
- `last_reminder_sent_at`
- `reminder_count`
- `customer_response`

### Rescheduling and Cancellation Fields

- `reschedule_count`
- `reschedule_reason`
- `supersedes_appointment_id`
- `rescheduled_to_appointment_id`
- `cancelled_at`
- `cancelled_by`
- `cancellation_reason`
- `cancellation_details`

### Attendance and Outcome Fields

- `attendance_status`
- `no_show_recorded_at`
- `no_show_reason`
- `outcome`
- `outcome_notes`
- `customer_feedback`
- `follow_up_required`
- `next_action_type`
- `next_action_at`
- `completed_by`

### External Calendar Integration

- `external_calendar_provider`
- `external_calendar_id`
- `external_event_id`
- `external_event_version`
- `calendar_sync_status`
- `calendar_sync_error`
- `last_calendar_sync_at`

### Computed Fields

- `is_upcoming`
- `is_overdue`
- `is_late_arrival`
- `actual_duration_minutes`
- `days_until_appointment`
- `minutes_until_appointment`
- `reminder_due`
- `confirmation_overdue`
- `resource_conflict_detected`
- `customer_conflict_detected`
- `user_conflict_detected`
- `appointment_age_days`

### Governance and Lifecycle

- **Customer Snapshot:** `customer_snapshot` (JSONB)
- **Vehicle Snapshot:** `vehicle_snapshot` (JSONB)
- **Resource Snapshot:** `resource_snapshot` (JSONB)
- **Scheduling Snapshot:** `scheduling_snapshot` (JSONB)
- **Confirmation Evidence:** `confirmation_evidence` (JSONB)
- **Attendance Evidence:** `attendance_evidence` (JSONB)
- **Outcome Snapshot:** `outcome_snapshot` (JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `confirmed_by`
  - `cancelled_by`
  - `completed_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `confirmation_requested_at`
  - `confirmed_at`
  - `last_reminder_sent_at`
  - `check_in_at`
  - `actual_start_at`
  - `actual_end_at`
  - `check_out_at`
  - `no_show_recorded_at`
  - `cancelled_at`
  - `completed_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| appointment_id | UUID | Unique canonical identifier for the Appointment. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Appointment. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Customer participating in the Appointment. | Yes | N/A | Must reference an active Customer in the same dealership | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| lead_id | UUID | Lead associated with the Appointment. | No | Null | Must belong to the same Customer and dealership | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| qualified_lead_id | UUID | Qualified Lead associated with the Appointment. | No | Null | Must belong to the same Customer and dealership | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| opportunity_id | UUID | Opportunity supported by the Appointment. | No | Null | Must belong to the same Customer and dealership | 333e4444-e55b-66d7-a888-426614174000 | System-controlled |
| vehicle_id | UUID | Vehicle reserved or discussed during the Appointment. | No | Null | Required for Vehicle-specific test drives | 555e6666-e77b-88d9-a000-426614174000 | System-controlled |
| trade_in_id | UUID | Trade-In evaluated during the Appointment. | No | Null | Required for a linked Trade-In inspection when one exists | 666e7777-e88b-99d0-a111-426614174000 | System-controlled |
| deal_id | UUID | Deal associated with signing or delivery Appointments. | No | Null | Required for Deal-specific contracting or delivery | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| assigned_user_id | UUID | Primary dealership User responsible for the Appointment. | Yes | Assignment rules | Must reference an active authorized User | 321e6547-e89b-12d3-a456-426614174000 | System-controlled |
| appointment_type | Enum | Business purpose of the Appointment. | Yes | SALES_CONSULTATION | Must match AppointmentType ENUM | TEST_DRIVE | At least 0.95 |
| status | Enum | Current lifecycle state of the Appointment. | Yes | DRAFT | Must match AppointmentStatus ENUM | CONFIRMED | At least 0.99 |
| priority | Enum | Operational urgency of the Appointment. | Yes | STANDARD | Must match AppointmentPriority ENUM | HIGH | At least 0.90 |
| channel | Enum | Communication or meeting channel. | Yes | IN_PERSON | Must match AppointmentChannel ENUM | VIDEO | At least 0.95 |
| scheduled_start_at | Timestamp | Planned Appointment start time. | Yes | N/A | Must be a valid future or authorized historical timestamp | 2026-08-05T10:00:00+03:00 | At least 0.99 |
| scheduled_end_at | Timestamp | Planned Appointment end time. | Yes | Calculated | Must be later than scheduled_start_at | 2026-08-05T10:45:00+03:00 | System-calculated |
| timezone | String | IANA timezone used for scheduling. | Yes | Dealership timezone | Must use a valid IANA timezone identifier | Africa/Cairo | At least 0.99 |
| duration_minutes | Integer | Planned Appointment duration. | Yes | Type-based default | Must be within the allowed range for the Appointment type | 45 | System or authorized human |
| dealership_location_id | UUID | Dealership location hosting the Appointment. | Conditional | Null | Required for dealership-based in-person Appointments | 456e7890-e12b-34d5-a678-426614174000 | System-controlled |
| meeting_url | String | Secure link for a remote Appointment. | Conditional | Null | Required for VIDEO channel and must use an approved provider | https://meet.example.com/secure-event | Trusted integration |
| phone_number | String | Authorized number used for a phone Appointment. | Conditional | Null | Required for PHONE channel and must be normalized | +201234567890 | Verified source |
| confirmation_status | Enum | Current Customer-confirmation state. | Yes | NOT_REQUESTED | Must match AppointmentConfirmationStatus ENUM | CONFIRMED | System-controlled |
| attendance_status | Enum | Recorded attendance result. | Yes | NOT_RECORDED | Must match AppointmentAttendanceStatus ENUM | ATTENDED | System-controlled |
| vehicle_required | Boolean | Indicates whether a specific Vehicle must be available. | Yes | false | Must be true for Vehicle-specific test drives | true | System or human |
| test_drive_required | Boolean | Indicates whether the Appointment includes a test drive. | Yes | false | Must be true when appointment_type is TEST_DRIVE | true | System-controlled |
| driver_license_required | Boolean | Indicates whether a valid driving license is required. | Yes | false | Must be true for Customer-driven test drives | true | System-controlled |
| driver_license_verified | Boolean | Indicates whether required license evidence passed verification. | Yes | false | Must be true before a Customer-driven test drive begins | true | Authoritative evidence |
| appointment_resource_id | UUID | Vehicle bay, test-drive resource, room, or other reserved resource. | No | Null | Must be available for the full scheduled period | 888e9999-e00b-11d2-a222-426614174000 | System-controlled |
| reminder_count | Integer | Number of reminders sent successfully. | Yes | 0 | Must be zero or greater | 2 | System-computed |
| reschedule_count | Integer | Number of completed rescheduling operations. | Yes | 0 | Must be zero or greater | 1 | System-computed |
| check_in_at | Timestamp | Time the Customer or participant checked in. | No | Null | Cannot materially precede the permitted arrival window | 2026-08-05T09:54:00+03:00 | Trusted system or human |
| actual_start_at | Timestamp | Actual interaction start time. | No | Null | Cannot precede check-in without an authorized exception | 2026-08-05T10:03:00+03:00 | Trusted system or human |
| actual_end_at | Timestamp | Actual interaction end time. | No | Null | Must be later than actual_start_at | 2026-08-05T10:42:00+03:00 | Trusted system or human |
| outcome | Enum | Commercial or operational result of the Appointment. | No | Null | Required when status becomes COMPLETED | QUOTATION_REQUESTED | Authorized human or trusted workflow |
| follow_up_required | Boolean | Indicates whether another action must be scheduled. | Yes | false | Must be true when the selected outcome requires continuation | true | System or human |
| next_action_at | Timestamp | Due time for the next required action. | No | Null | Required when follow_up_required is true | 2026-08-05T14:00:00+03:00 | System or human |
| external_calendar_provider | Enum | External provider used for synchronization. | No | NONE | Must match CalendarProvider ENUM | GOOGLE_CALENDAR | Trusted integration |
| external_event_id | String | Provider-specific event identifier. | No | Null | Must be unique per provider and dealership | evt_01HXYZ123 | Trusted integration |
| calendar_sync_status | Enum | Current external-calendar synchronization status. | Yes | NOT_REQUIRED | Must match CalendarSyncStatus ENUM | SYNCED | System-controlled |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 6 | System-controlled |

## 4. Enumerations

### AppointmentStatus

- **DRAFT:** Appointment information is being prepared and no schedule is committed.
- **SCHEDULED:** A valid time and responsible User were assigned.
- **CONFIRMATION_PENDING:** Customer confirmation was requested but not completed.
- **CONFIRMED:** The Customer or authorized participant confirmed attendance.
- **CHECKED_IN:** The Customer arrived or joined the remote session.
- **IN_PROGRESS:** The scheduled interaction is actively taking place.
- **COMPLETED:** The interaction ended and an outcome was recorded.
- **NO_SHOW:** The Customer did not attend within the permitted attendance window.
- **CANCELLED:** The Appointment ended before completion through cancellation.
- **RESCHEDULED:** The Appointment was replaced by a new Appointment record.

### AppointmentType

- SALES_CONSULTATION
- SHOWROOM_VISIT
- TEST_DRIVE
- TRADE_IN_INSPECTION
- FINANCE_CONSULTATION
- DOCUMENT_COLLECTION
- DOCUMENT_SIGNING
- VEHICLE_DELIVERY
- REMOTE_CONSULTATION
- FOLLOW_UP
- CUSTOMER_SUPPORT_HANDOFF
- OTHER

### AppointmentPriority

- LOW
- STANDARD
- HIGH
- URGENT
- VIP

### AppointmentChannel

- IN_PERSON
- PHONE
- VIDEO
- WHATSAPP
- OTHER_REMOTE

### AppointmentLocationType

- DEALERSHIP
- CUSTOMER_LOCATION
- THIRD_PARTY_LOCATION
- REMOTE
- MOBILE_SHOWROOM
- OTHER

### AppointmentConfirmationStatus

- NOT_REQUESTED
- PENDING
- CONFIRMED
- DECLINED
- EXPIRED
- FAILED

### AppointmentAttendanceStatus

- NOT_RECORDED
- ATTENDED
- LATE
- PARTIALLY_ATTENDED
- NO_SHOW
- CANCELLED_BY_CUSTOMER
- CANCELLED_BY_DEALERSHIP

### AppointmentOutcome

- CONSULTATION_COMPLETED
- FOLLOW_UP_REQUIRED
- OPPORTUNITY_CREATED
- REQUIREMENTS_UPDATED
- VEHICLE_SELECTED
- TEST_DRIVE_COMPLETED
- TEST_DRIVE_DECLINED
- TRADE_IN_INSPECTED
- QUOTATION_REQUESTED
- QUOTATION_PRESENTED
- FINANCE_APPLICATION_STARTED
- DOCUMENTS_COLLECTED
- CONTRACT_SIGNED
- VEHICLE_DELIVERED
- CUSTOMER_NOT_INTERESTED
- CUSTOMER_UNDECIDED
- RESCHEDULE_REQUIRED
- OTHER

### AppointmentSource

- SALES_CONSULTANT
- CUSTOMER_SELF_SERVICE
- AI_AGENT
- BDC
- WEBSITE
- PHONE
- WHATSAPP
- MARKETPLACE
- OEM_PLATFORM
- WALK_IN
- IMPORTED_CALENDAR
- OTHER

### AppointmentCancellationReason

- CUSTOMER_REQUEST
- CUSTOMER_UNAVAILABLE
- DEALERSHIP_UNAVAILABLE
- ASSIGNED_USER_UNAVAILABLE
- VEHICLE_UNAVAILABLE
- RESOURCE_UNAVAILABLE
- WEATHER_OR_EMERGENCY
- DUPLICATE_APPOINTMENT
- OPPORTUNITY_CLOSED
- DEAL_CANCELLED
- COMPLIANCE_BLOCK
- OTHER

### AppointmentNoShowReason

- UNKNOWN
- CUSTOMER_FORGOT
- CUSTOMER_UNREACHABLE
- CUSTOMER_DELAYED
- TRANSPORTATION_ISSUE
- SCHEDULING_MISUNDERSTANDING
- CUSTOMER_CHANGED_MIND
- OTHER

### CalendarProvider

- NONE
- GOOGLE_CALENDAR
- MICROSOFT_OUTLOOK
- APPLE_CALENDAR
- INTERNAL_CALENDAR
- OTHER

### CalendarSyncStatus

- NOT_REQUIRED
- PENDING
- SYNCED
- UPDATE_PENDING
- FAILED
- CONFLICT
- DELETED_EXTERNALLY

## 5. Validation Rules

### Business Rules

- Every Appointment must belong to one resolved Customer.
- Every Appointment must have one assigned responsible User.
- The scheduled start and end times must use an explicit timezone.
- The assigned User must be available for the entire scheduled period.
- Any required Vehicle, room, inspection bay, or operational resource must be available for the entire scheduled period.
- A test-drive Appointment requires an eligible Vehicle.
- A Customer-driven test drive requires valid driving-license verification before the test drive begins.
- A Trade-In inspection Appointment must reference the corresponding Trade-In when the Trade-In record already exists.
- A finance-consultation Appointment should reference the Opportunity or Finance Application.
- A document-signing or delivery Appointment must reference the applicable Deal.
- A Customer may not have conflicting active Appointments unless an authorized override is recorded.
- A User may not have conflicting active Appointments unless capacity or group scheduling is explicitly supported.
- Customer reminders must follow communication consent, channel, quiet-hours, and frequency policies.
- A cancelled Appointment cannot be completed.
- A completed Appointment cannot be directly rescheduled; a new Appointment must be created.
- Rescheduling must preserve the original Appointment and create a replacement record.
- A no-show decision must not be recorded before the allowed attendance grace period ends.
- Appointment completion requires a documented outcome.
- Outcomes that require continued engagement must create a next action or follow-up Task.

### Technical Rules

- Appointment creation and rescheduling must require an idempotency key.
- Time conflicts must be checked server-side.
- Time calculations must preserve the original timezone and normalized UTC values.
- Daylight-saving transitions must be handled by the scheduling service.
- `record_version` must increment after every successful update.
- External-calendar synchronization must preserve provider IDs and versions.
- Calendar webhook events must be deduplicated.
- Failed calendar synchronization must not silently change the canonical Appointment state.
- Reminder delivery must record channel, timestamp, provider response, and delivery status.
- Appointment status transitions must create immutable history and audit records.
- Resource reservations and Appointment creation must use a controlled transaction or compensating workflow.
- Personally identifiable data must not appear in public calendar descriptions or insecure meeting links.

### Data Constraints

- `scheduled_end_at` must be later than `scheduled_start_at`.
- `duration_minutes` must equal the planned difference between start and end.
- `duration_minutes` must remain within the configured limit for `appointment_type`.
- `arrival_window_end_at` must not precede `arrival_window_start_at`.
- `actual_end_at` must be later than `actual_start_at`.
- `check_out_at` must not precede `check_in_at`.
- `reminder_count` and `reschedule_count` cannot be negative.
- `guest_count` cannot be negative.
- `meeting_url` is required when `channel = VIDEO`.
- `phone_number` is required when `channel = PHONE`.
- `dealership_location_id` is required for dealership-based in-person Appointments.
- `vehicle_id` is required when `vehicle_required = true`.
- `driver_license_verified` must be true before starting a Customer-driven test drive.
- `outcome` is required when `status = COMPLETED`.
- `cancellation_reason` is required when `status = CANCELLED`.
- `rescheduled_to_appointment_id` is required when `status = RESCHEDULED`.
- `no_show_recorded_at` is required when `status = NO_SHOW`.
- `next_action_at` is required when `follow_up_required = true`.

### Referential Integrity

- All linked entities must belong to the same `dealership_id`.
- `lead_id`, `qualified_lead_id`, and `opportunity_id` must belong to `customer_id`.
- `vehicle_id` must be eligible for the Appointment purpose.
- `trade_in_id` must belong to the same Customer and Opportunity context.
- `finance_application_id` must belong to the same Customer and Opportunity context.
- `deal_id` must belong to the same Customer.
- `assigned_user_id` must reference an active User authorized for the Appointment type.
- `appointment_resource_id` must belong to the same dealership or approved shared location.
- `supersedes_appointment_id` and `rescheduled_to_appointment_id` must reference Appointments in the same dealership.
- An Appointment cannot supersede itself.
- Circular rescheduling chains are prohibited.
- An Appointment cannot be hard-deleted while referenced by attendance, Customer communication, Deal, Delivery, Trade-In, or audit records.

### Human Approval Requirements

- Scheduling outside permitted business hours may require manager approval.
- Double-booking a User, Vehicle, or resource requires authorized override.
- Test drives involving restricted, high-value, or special-category Vehicles require additional approval.
- Remote or off-site test drives may require Sales Manager approval.
- Delivery Appointments cannot be confirmed until delivery-readiness requirements pass.
- AI Agents cannot confirm driving-license validity without authoritative evidence.
- AI Agents cannot override resource, Vehicle, compliance, or delivery blocks.
- AI Agents cannot mark an Appointment `COMPLETED`, `NO_SHOW`, or `VEHICLE_DELIVERED` without trusted evidence or authorized human action.
- Conflicting Customer, time, resource, or attendance evidence must create a Human Review Task.

## 6. State Machine

### Allowed States

- DRAFT
- SCHEDULED
- CONFIRMATION_PENDING
- CONFIRMED
- CHECKED_IN
- IN_PROGRESS
- COMPLETED
- NO_SHOW
- CANCELLED
- RESCHEDULED

### Allowed Transitions

- DRAFT → SCHEDULED
- DRAFT → CANCELLED
- SCHEDULED → CONFIRMATION_PENDING
- SCHEDULED → CONFIRMED
- SCHEDULED → CHECKED_IN
- SCHEDULED → CANCELLED
- SCHEDULED → RESCHEDULED
- CONFIRMATION_PENDING → CONFIRMED
- CONFIRMATION_PENDING → SCHEDULED
- CONFIRMATION_PENDING → CANCELLED
- CONFIRMATION_PENDING → RESCHEDULED
- CONFIRMED → CHECKED_IN
- CONFIRMED → IN_PROGRESS
- CONFIRMED → NO_SHOW
- CONFIRMED → CANCELLED
- CONFIRMED → RESCHEDULED
- CHECKED_IN → IN_PROGRESS
- CHECKED_IN → CANCELLED
- IN_PROGRESS → COMPLETED
- IN_PROGRESS → CANCELLED
- SCHEDULED → NO_SHOW
- CONFIRMATION_PENDING → NO_SHOW

### Forbidden Transitions

- DRAFT → CHECKED_IN
- DRAFT → IN_PROGRESS
- DRAFT → COMPLETED
- SCHEDULED → COMPLETED
- CONFIRMATION_PENDING → COMPLETED
- CONFIRMED → COMPLETED
- NO_SHOW → IN_PROGRESS
- NO_SHOW → COMPLETED
- CANCELLED → SCHEDULED
- CANCELLED → CONFIRMED
- CANCELLED → COMPLETED
- RESCHEDULED → CONFIRMED
- RESCHEDULED → IN_PROGRESS
- COMPLETED → SCHEDULED
- COMPLETED → CANCELLED
- COMPLETED → RESCHEDULED

### Entry Conditions

- To enter `SCHEDULED`:
  - Customer, Appointment type, responsible User, start time, end time, and timezone must be valid.
  - Required User, Vehicle, location, and resource availability checks must pass.
  - The applicable Customer communication permission must be evaluated.

- To enter `CONFIRMATION_PENDING`:
  - A valid confirmation request must be sent through an authorized channel.
  - The request timestamp and channel must be recorded.

- To enter `CONFIRMED`:
  - Verifiable Customer or authorized participant confirmation must exist.
  - The confirmed schedule must match the current Appointment version.
  - Required resources must remain available.

- To enter `CHECKED_IN`:
  - The Appointment must be within the permitted arrival window or have an authorized exception.
  - Customer or participant presence must be verified.
  - `check_in_at` must be populated.

- To enter `IN_PROGRESS`:
  - The Customer or authorized participant must be present.
  - The assigned User or authorized replacement must be present.
  - Required Vehicle and resources must be available.
  - For a Customer-driven test drive, driving-license verification must pass.
  - `actual_start_at` must be populated.

- To enter `COMPLETED`:
  - The interaction must have ended.
  - `actual_end_at`, `completed_at`, `completed_by`, and `outcome` must be populated.
  - Required outcome notes and evidence must exist.
  - A follow-up action must be created when the outcome requires one.

- To enter `NO_SHOW`:
  - The scheduled start time and permitted grace period must have passed.
  - No verified Customer attendance may exist.
  - `no_show_recorded_at` must be populated.
  - A follow-up or closure action must be selected.

- To enter `CANCELLED`:
  - A valid cancellation reason and responsible actor must be recorded.
  - Active resource reservations must be released.
  - Customer notification must be attempted when legally permitted.

- To enter `RESCHEDULED`:
  - A replacement Appointment must be created successfully.
  - `rescheduled_to_appointment_id` and `reschedule_reason` must be populated.
  - The replacement Appointment must reference the original through `supersedes_appointment_id`.
  - Original resource reservations must be released or transferred safely.

### Exit Conditions

- An Appointment cannot exit `DRAFT` without complete minimum scheduling information.
- An Appointment cannot exit `SCHEDULED` toward attendance states before the permitted check-in window.
- An Appointment cannot exit `CONFIRMATION_PENDING` toward `CONFIRMED` without valid confirmation evidence.
- An Appointment cannot exit `CONFIRMED` toward `IN_PROGRESS` while mandatory Vehicle, license, compliance, or resource blocks exist.
- An Appointment cannot exit `CHECKED_IN` toward `COMPLETED` without first entering `IN_PROGRESS`, except through an authorized correction workflow.
- An Appointment cannot exit `IN_PROGRESS` toward `COMPLETED` without a documented outcome.
- A `NO_SHOW` Appointment cannot return to an active state; a new Appointment must be created.
- A `CANCELLED` Appointment cannot return to an active state; a new Appointment must be created.
- A `RESCHEDULED` Appointment remains historical and cannot be reactivated.
- A `COMPLETED` Appointment cannot be edited except through controlled correction and audit workflows.

### Terminal States

- **COMPLETED:** The interaction occurred and its outcome was recorded.
- **NO_SHOW:** The Customer did not attend within the permitted window.
- **CANCELLED:** The Appointment ended before it occurred or completed.
- **RESCHEDULED:** A replacement Appointment superseded the original record.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Customer identified by `customer_id`.
  - Assigned User identified by `assigned_user_id`.
  - Valid timezone, business-hours, capacity, and scheduling policies.
  - Required Vehicle, location, room, inspection bay, or delivery resource availability.

- **Consumes:**
  - Customer identity, language, communication consent, and preferred channel.
  - Lead, Qualified Lead, Opportunity, or Deal context.
  - Vehicle availability and test-drive eligibility.
  - Trade-In inspection requirements.
  - Finance-consultation requirements.
  - Contract-signing prerequisites.
  - Delivery-readiness status.
  - User and resource calendar availability.
  - External calendar events and synchronization metadata.

- **Produces:**
  - Canonical scheduling record.
  - Customer confirmation and reminder instructions.
  - User, Vehicle, room, and resource reservations.
  - Attendance and no-show evidence.
  - Appointment outcome.
  - Opportunity and Customer Journey updates.
  - Follow-up Task or next-action requirements.
  - Scheduling, attendance, and conversion analytics.

- **Creates:**
  - Calendar event.
  - Confirmation request.
  - Reminder Jobs.
  - Resource reservation.
  - Follow-up Task.
  - Replacement Appointment after rescheduling.
  - Interaction Log entries.

- **Triggers:**
  - Customer confirmation workflow.
  - Reminder and notification workflow.
  - Vehicle or resource hold.
  - Check-in workflow.
  - Test-drive authorization checks.
  - No-show detection.
  - Outcome processing.
  - Lead, Opportunity, Trade-In, Deal, or Delivery updates.
  - External calendar synchronization.

- **Owned By:**
  - The dealership User identified by `assigned_user_id`.
  - Operational ownership may be transferred through an authorized reassignment process.

- **Referenced By:**
  - Customer
  - Lead
  - Qualified Lead
  - Opportunity
  - Vehicle
  - Trade-In
  - Finance Application
  - Deal
  - Delivery
  - Task
  - Interaction Log
  - Calendar Event
  - Appointment Resource
  - Customer Journey
  - AI Agent Run

- **Supersedes / Replaced By:**
  - A rescheduled Appointment remains immutable as historical scheduling evidence.
  - The replacement Appointment references the original through `supersedes_appointment_id`.
  - The original references the replacement through `rescheduled_to_appointment_id`.

## 8. Domain Events

### Emitted Events

- **AppointmentDraftCreated**  
  Payload: `appointment_id`, `customer_id`, `appointment_type`, `created_by`, `created_at`

- **AppointmentScheduled**  
  Payload: `appointment_id`, `assigned_user_id`, `scheduled_start_at`, `scheduled_end_at`, `timezone`

- **AppointmentConfirmationRequested**  
  Payload: `appointment_id`, `customer_id`, `confirmation_channel`, `confirmation_requested_at`

- **AppointmentConfirmed**  
  Payload: `appointment_id`, `customer_id`, `confirmed_at`, `confirmation_evidence_reference`

- **AppointmentConfirmationDeclined**  
  Payload: `appointment_id`, `customer_id`, `declined_at`, `customer_response`

- **AppointmentReminderScheduled**  
  Payload: `appointment_id`, `reminder_schedule`, `communication_channel`

- **AppointmentReminderSent**  
  Payload: `appointment_id`, `reminder_type`, `channel`, `sent_at`, `delivery_status`

- **AppointmentCheckedIn**  
  Payload: `appointment_id`, `customer_id`, `check_in_at`, `check_in_method`

- **AppointmentStarted**  
  Payload: `appointment_id`, `assigned_user_id`, `actual_start_at`

- **AppointmentCompleted**  
  Payload: `appointment_id`, `outcome`, `completed_by`, `completed_at`

- **AppointmentNoShowRecorded**  
  Payload: `appointment_id`, `no_show_reason`, `no_show_recorded_at`, `recorded_by`

- **AppointmentCancelled**  
  Payload: `appointment_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

- **AppointmentRescheduled**  
  Payload: `appointment_id`, `replacement_appointment_id`, `reschedule_reason`, `rescheduled_at`

- **AppointmentResourceConflictDetected**  
  Payload: `appointment_id`, `resource_type`, `resource_id`, `conflicting_appointment_id`

- **AppointmentCalendarSyncFailed**  
  Payload: `appointment_id`, `external_calendar_provider`, `calendar_sync_error`, `failed_at`

- **AppointmentFollowUpRequired**  
  Payload: `appointment_id`, `next_action_type`, `next_action_at`, `owner_id`

### Consumed Events

- **LeadAppointmentRequested**  
  Creates an Appointment associated with a Lead.

- **QualifiedLeadAppointmentRequested**  
  Creates an Appointment using qualified Customer and intent context.

- **OpportunityAppointmentRequested**  
  Creates an Appointment for consultation, test drive, Quotation presentation, or follow-up.

- **TradeInInspectionRequested**  
  Creates a Trade-In inspection Appointment.

- **FinanceConsultationRequested**  
  Creates a finance-related Appointment.

- **ContractSigningRequested**  
  Creates a document-signing Appointment linked to a Deal.

- **DealReadyForDelivery**  
  Allows creation or confirmation of a Vehicle-delivery Appointment.

- **VehicleStatusChanged**  
  Revalidates Vehicle availability for a Vehicle-specific Appointment.

- **UserAvailabilityChanged**  
  Detects assignment conflicts and may trigger rescheduling.

- **AppointmentResourceAvailabilityChanged**  
  Revalidates room, bay, Vehicle, route, or other required resources.

- **CustomerContactPermissionChanged**  
  Suspends unauthorized confirmation and reminder messages.

- **ExternalCalendarEventUpdated**  
  Reconciles permitted external schedule changes.

- **ExternalCalendarEventDeleted**  
  Creates a synchronization exception without silently deleting the canonical Appointment.

- **CustomerCheckedIn**  
  Moves an eligible Appointment to `CHECKED_IN`.

- **DeliveryCompleted**  
  Updates a delivery Appointment outcome when the handover is verified.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `customer_notes`
- `outcome_notes`
- `customer_feedback`
- `no_show_reason`
- `reschedule_reason`
- `cancellation_details`
- `arrival_instructions`
- Appointment-purpose summaries
- Follow-up summaries
- Test-drive feedback
- Consultation summaries
- Non-sensitive interaction summaries

### Fields Excluded from Embeddings

- `appointment_id`
- `customer_id`
- `lead_id`
- `qualified_lead_id`
- `opportunity_id`
- `deal_id`
- `finance_application_id`
- `assigned_user_id`
- `external_event_id`
- Direct phone numbers
- Customer addresses
- Secure meeting URLs
- Driving-license documents
- Identity documents
- Confirmation evidence
- Attendance evidence
- Exact location coordinates
- External participant contact details

> Personally identifiable information, secure access links, driving-license data, and exact location information must be supplied only through authorized structured context.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `appointment_type`
- `status`
- `priority`
- `channel`
- `scheduled_start_at`
- `scheduled_end_at`
- `timezone`
- `confirmation_status`
- `attendance_status`
- `vehicle_required`
- `test_drive_required`
- `driver_license_required`
- `driver_license_verified`
- `follow_up_required`
- `next_action_type`
- `next_action_at`
- Non-sensitive location label
- Permitted Customer language and communication channel

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `appointment_id`
- `customer_id`
- `lead_id`
- `qualified_lead_id`
- `opportunity_id`
- `deal_id`
- `assigned_user_id`
- `appointment_type`
- `status`
- `scheduled_start_at`
- `channel`

### Confidence Thresholds

- Date and time extraction requires confidence of at least `0.95`.
- Timezone extraction requires confidence of at least `0.99`.
- Appointment-type classification requires confidence of at least `0.90`.
- Customer confirmation or cancellation intent requires confidence of at least `0.95`.
- Rescheduling-request extraction requires confidence of at least `0.95`.
- Appointment outcome extraction requires confidence of at least `0.90`.
- Driving-license extraction or verification requires confidence of at least `0.99` and authoritative evidence.
- AI-generated reminder content requires confidence of at least `0.95` before automated delivery.
- No AI confidence score may override availability, consent, compliance, Vehicle, or resource validation.

### Human Approval Thresholds

- AI Agents cannot mark an Appointment `COMPLETED`, `NO_SHOW`, or `VEHICLE_DELIVERED` without trusted evidence.
- AI Agents cannot override User, Customer, Vehicle, or resource conflicts.
- AI Agents cannot approve an off-site or restricted Vehicle test drive.
- AI Agents cannot verify driving-license validity without an authoritative verification result.
- AI Agents cannot expose secure meeting links or Customer addresses outside authorized channels.
- Scheduling outside policy-defined hours requires authorized approval.
- Conflicting time, identity, attendance, or location evidence must create a Human Review Task.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/appointments`

### Methods

- `GET` — list or search Appointments.
- `POST` — create a Draft or Scheduled Appointment.
- `GET /{id}` — retrieve one Appointment.
- `PATCH /{id}` — update permitted fields before terminal completion.
- `POST /availability` — check User, Customer, Vehicle, location, and resource availability.
- `POST /{id}/schedule` — commit the Appointment schedule.
- `POST /{id}/request-confirmation` — send a Customer confirmation request.
- `POST /{id}/confirm` — record verified confirmation.
- `POST /{id}/decline` — record Customer decline.
- `POST /{id}/check-in` — record Customer arrival or remote attendance.
- `POST /{id}/start` — begin the Appointment.
- `POST /{id}/complete` — complete the Appointment with a documented outcome.
- `POST /{id}/record-no-show` — record an eligible no-show.
- `POST /{id}/reschedule` — create a replacement Appointment.
- `POST /{id}/cancel` — cancel an eligible Appointment.
- `POST /{id}/sync-calendar` — request external calendar synchronization.
- `POST /{id}/retry-calendar-sync` — retry a failed synchronization.
- `DELETE /{id}` — perform a soft delete when permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateAppointmentRequest",
  "type": "object",
  "properties": {
    "customer_id": {
      "type": "string",
      "format": "uuid"
    },
    "lead_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "qualified_lead_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "opportunity_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "vehicle_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "trade_in_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "finance_application_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "deal_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "assigned_user_id": {
      "type": "string",
      "format": "uuid"
    },
    "appointment_type": {
      "type": "string",
      "enum": [
        "SALES_CONSULTATION",
        "SHOWROOM_VISIT",
        "TEST_DRIVE",
        "TRADE_IN_INSPECTION",
        "FINANCE_CONSULTATION",
        "DOCUMENT_COLLECTION",
        "DOCUMENT_SIGNING",
        "VEHICLE_DELIVERY",
        "REMOTE_CONSULTATION",
        "FOLLOW_UP",
        "CUSTOMER_SUPPORT_HANDOFF",
        "OTHER"
      ]
    },
    "priority": {
      "type": "string",
      "enum": ["LOW", "STANDARD", "HIGH", "URGENT", "VIP"]
    },
    "channel": {
      "type": "string",
      "enum": [
        "IN_PERSON",
        "PHONE",
        "VIDEO",
        "WHATSAPP",
        "OTHER_REMOTE"
      ]
    },
    "scheduled_start_at": {
      "type": "string",
      "format": "date-time"
    },
    "scheduled_end_at": {
      "type": "string",
      "format": "date-time"
    },
    "timezone": {
      "type": "string",
      "minLength": 1,
      "maxLength": 100
    },
    "dealership_location_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "appointment_resource_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "meeting_url": {
      "type": ["string", "null"],
      "format": "uri"
    },
    "phone_number": {
      "type": ["string", "null"],
      "maxLength": 30
    },
    "vehicle_required": {
      "type": "boolean"
    },
    "test_drive_required": {
      "type": "boolean"
    },
    "driver_license_required": {
      "type": "boolean"
    },
    "customer_preferred_language": {
      "type": ["string", "null"],
      "maxLength": 20
    },
    "customer_notes": {
      "type": ["string", "null"],
      "maxLength": 5000
    }
  },
  "required": [
    "customer_id",
    "assigned_user_id",
    "appointment_type",
    "priority",
    "channel",
    "scheduled_start_at",
    "scheduled_end_at",
    "timezone",
    "vehicle_required",
    "test_drive_required",
    "driver_license_required"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Appointment {
  id: ID!
  dealershipId: ID!
  customerId: ID!
  leadId: ID
  qualifiedLeadId: ID
  opportunityId: ID
  vehicleId: ID
  tradeInId: ID
  financeApplicationId: ID
  dealId: ID
  assignedUserId: ID!
  dealershipLocationId: ID
  appointmentResourceId: ID
  supersedesAppointmentId: ID
  rescheduledToAppointmentId: ID
  appointmentType: AppointmentType!
  status: AppointmentStatus!
  priority: AppointmentPriority!
  channel: AppointmentChannel!
  source: AppointmentSource!
  scheduledStartAt: DateTime!
  scheduledEndAt: DateTime!
  timezone: String!
  durationMinutes: Int!
  confirmationStatus: AppointmentConfirmationStatus!
  attendanceStatus: AppointmentAttendanceStatus!
  locationType: AppointmentLocationType
  locationName: String
  meetingUrl: String
  vehicleRequired: Boolean!
  testDriveRequired: Boolean!
  driverLicenseRequired: Boolean!
  driverLicenseVerified: Boolean!
  reminderCount: Int!
  rescheduleCount: Int!
  checkInAt: DateTime
  actualStartAt: DateTime
  actualEndAt: DateTime
  checkOutAt: DateTime
  outcome: AppointmentOutcome
  followUpRequired: Boolean!
  nextActionType: NextActionType
  nextActionAt: DateTime
  externalCalendarProvider: CalendarProvider!
  externalEventId: String
  calendarSyncStatus: CalendarSyncStatus!
  confirmedAt: DateTime
  completedAt: DateTime
  cancelledAt: DateTime
  noShowRecordedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `appointments`
- **Participant Table:** `appointment_participants`
- **Resource-Reservation Table:** `appointment_resource_reservations`
- **Reminder Table:** `appointment_reminders`
- **Confirmation Table:** `appointment_confirmations`
- **Attendance Table:** `appointment_attendance`
- **Outcome Table:** `appointment_outcomes`
- **Calendar-Sync Table:** `appointment_calendar_sync`
- **Status-History Table:** `appointment_status_history`
- **Audit Table:** `appointment_audit_log`

### Indexes

- `idx_appointments_tenant_status (dealership_id, status)`  
  Used for operational scheduling queues.

- `idx_appointments_customer_time (dealership_id, customer_id, scheduled_start_at)`  
  Used to detect Customer conflicts and retrieve appointment history.

- `idx_appointments_user_time (dealership_id, assigned_user_id, scheduled_start_at, scheduled_end_at)`  
  Used to detect User scheduling conflicts.

- `idx_appointments_vehicle_time (dealership_id, vehicle_id, scheduled_start_at, scheduled_end_at)`  
  Used to detect Vehicle conflicts.

- `idx_appointments_resource_time (dealership_id, appointment_resource_id, scheduled_start_at, scheduled_end_at)`  
  Used to detect room, bay, and resource conflicts.

- `idx_appointments_opportunity (dealership_id, opportunity_id, scheduled_start_at DESC)`  
  Used for Opportunity appointment history.

- `idx_appointments_deal (dealership_id, deal_id, scheduled_start_at DESC)`  
  Used for contracting and delivery Appointments.

- `idx_appointments_confirmation (dealership_id, confirmation_status, scheduled_start_at)`  
  Used for pending-confirmation queues.

- `idx_appointments_reminders (dealership_id, last_reminder_sent_at, scheduled_start_at, status)`  
  Used by reminder Jobs.

- `idx_appointments_calendar_event (external_calendar_provider, external_event_id)`  
  Used for external-calendar reconciliation.

- `idx_appointments_follow_up (dealership_id, follow_up_required, next_action_at)`  
  Used for follow-up monitoring.

### Unique Constraints

- `UQ_appointment_external_event (dealership_id, external_calendar_provider, external_event_id)`  
  Applies when `external_event_id` is not null.

- `UQ_appointment_rescheduled_to (rescheduled_to_appointment_id)`  
  Applies when `rescheduled_to_appointment_id` is not null.

- `UQ_appointment_supersedes (supersedes_appointment_id)`  
  Applies when the business process allows only one direct replacement.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)`
- `lead_id` → `leads(id)` — nullable
- `qualified_lead_id` → `qualified_leads(id)` — nullable
- `opportunity_id` → `opportunities(id)` — nullable
- `vehicle_id` → `vehicles(id)` — nullable
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)` — nullable
- `deal_id` → `deals(id)` — nullable
- `assigned_user_id` → `users(id)`
- `created_by` → `users(id)`
- `dealership_location_id` → `dealership_locations(id)` — nullable
- `appointment_resource_id` → `appointment_resources(id)` — nullable
- `supersedes_appointment_id` → `appointments(id)` — nullable
- `rescheduled_to_appointment_id` → `appointments(id)` — nullable
- `completed_by` → `users(id)` — nullable
- `cancelled_by` → `users(id)` — nullable

### Database Constraints

- `scheduled_end_at > scheduled_start_at`
- `duration_minutes > 0`
- `actual_end_at > actual_start_at`
- `check_out_at >= check_in_at`
- `reminder_count >= 0`
- `reschedule_count >= 0`
- `guest_count >= 0`
- `vehicle_id IS NOT NULL` when `vehicle_required = true`
- `meeting_url IS NOT NULL` when `channel = VIDEO`
- `phone_number IS NOT NULL` when `channel = PHONE`
- `outcome IS NOT NULL` when `status = COMPLETED`
- `cancellation_reason IS NOT NULL` when `status = CANCELLED`
- `no_show_recorded_at IS NOT NULL` when `status = NO_SHOW`
- `rescheduled_to_appointment_id IS NOT NULL` when `status = RESCHEDULED`
- `next_action_at IS NOT NULL` when `follow_up_required = true`
- `driver_license_verified = true` before a Customer-driven test drive enters `IN_PROGRESS`
- Circular Appointment rescheduling chains are prohibited.

### Conflict Protection

- Use exclusion constraints or equivalent transaction-level conflict checks for:
  - Assigned User availability.
  - Vehicle availability.
  - Appointment Resource availability.
  - Customer scheduling conflicts when simultaneous Appointments are prohibited.

- Conflict validation must exclude terminal states:
  - `COMPLETED`
  - `NO_SHOW`
  - `CANCELLED`
  - `RESCHEDULED`

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition historical Appointments by `scheduled_start_at`.
- Participant, reminder, confirmation, attendance, outcome, calendar-sync, history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Create, view, update, complete, cancel, and reschedule Appointments assigned to them within permitted policy.
- **Sales Manager:** View and manage dealership Appointments, reassign Users, and approve authorized scheduling exceptions.
- **BDC Agent:** Create, confirm, reschedule, and cancel early-stage Customer Appointments according to assignment scope.
- **Appraiser:** Access Trade-In inspection Appointments assigned to them.
- **Finance User:** Access finance-consultation Appointments and related permitted context.
- **Delivery Coordinator:** Access Vehicle-delivery Appointments and delivery-readiness context.
- **Reception User:** Limited check-in, arrival, and location-assistance access.
- **Inventory User:** Read access to Vehicle and resource scheduling without unrestricted Customer financial information.
- **AI Scheduling Agent:** Service Account access limited to availability checks, drafting, confirmation requests, reminders, and approved rescheduling workflows.
- **Calendar Integration Service:** Restricted synchronization access using tenant-scoped credentials.
- **Notification Service:** Restricted reminder and confirmation-delivery access.

### PII Classification

- **Level:** `CRITICAL_PII`

The Appointment may contain or reference:

- Customer name
- Phone number
- Email address
- Customer address
- Appointment location
- Preferred language
- Driving-license verification
- Accessibility requirements
- Attendance history
- Meeting links
- Customer feedback
- External participant information

### Sensitive Operational Fields

- `meeting_url`
- `phone_number`
- Customer-location address
- `arrival_instructions`
- `driver_license_verified`
- `special_assistance_notes`
- `customer_notes`
- `attendance_evidence`
- `confirmation_evidence`
- External calendar identifiers

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, snapshots, documents, calendar tokens, and backups.
- **Column-Level Protection:** Customer contact information, meeting URLs, addresses, driving-license evidence, assistance notes, and attendance evidence require encryption, tokenization, or equivalent approved protection.
- External calendar access tokens must be stored in a secrets-management system.
- Meeting and check-in links must be time-limited and cryptographically protected.
- Encryption keys must be separated by environment and rotated according to security policy.

### Audit Requirements

- Every scheduling change must record:
  - Previous start and end times
  - New start and end times
  - Previous and new assigned User
  - Actor
  - Reason
  - Timestamp

- Every confirmation operation must preserve:
  - Customer or participant identity
  - Confirmation channel
  - Appointment version
  - Evidence reference
  - Timestamp

- Every reminder must preserve:
  - Reminder type
  - Delivery channel
  - Provider response
  - Delivery status
  - Timestamp

- Every attendance change must record:
  - Previous attendance state
  - New attendance state
  - Check-in method
  - Actor
  - Evidence
  - Timestamp

- Every completion must preserve:
  - Outcome
  - Outcome notes
  - Completed User
  - Actual start and end times
  - Follow-up requirements
  - Timestamp

- Every cancellation or rescheduling operation must preserve:
  - Reason
  - Actor
  - Original schedule
  - Replacement Appointment ID when applicable
  - Customer-notification result
  - Timestamp

- Human overrides of AI scheduling recommendations must retain both the original recommendation and the final human decision.
- Access to Customer location, contact, attendance, driving-license, and meeting-link information must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, User, Vehicle, Opportunity, Trade-In, Finance Application, Deal, Delivery, resource, or Appointment linking is prohibited.
- AI Agents, Jobs, calendar integrations, notification services, exports, and semantic retrieval must receive tenant scope through signed execution context.
- Calendar and meeting links must never expose records from another tenant.
- External provider events must be mapped to exactly one tenant before processing.

### Retention and Deletion

- Completed, cancelled, no-show, and rescheduled Appointments must follow dealership, legal, audit, and Customer-consent retention policies.
- Appointment records linked to Deals, Deliveries, Trade-Ins, finance processes, or compliance evidence must not be hard-deleted while dependencies remain.
- Soft deletion is the operational default for eligible records.
- External calendar deletion must not automatically delete the canonical Appointment.
- Legally approved deletion requests must purge or anonymize permitted PII across:
  - Appointment records
  - Customer and scheduling snapshots
  - Confirmation and attendance evidence
  - Reminder history
  - Calendar synchronization records
  - Meeting links
  - Interaction summaries
  - Vector stores
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
