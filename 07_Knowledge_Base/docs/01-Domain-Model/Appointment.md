# Appointment

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Appointment Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Appointment Object represents one governed, time-bound interaction planned between a Customer and authorized dealership participants for a defined automotive business purpose.

An Appointment may support:

- Sales consultation.
- Showroom visit.
- Remote consultation.
- Vehicle demonstration.
- Test drive.
- Trade-In inspection.
- Finance consultation.
- Quotation review.
- Document collection.
- Contract-signing session.
- Vehicle-delivery handoff.
- Fleet consultation.
- Follow-up meeting.
- Another approved Customer-facing activity.

The Appointment coordinates:

- Date and time.
- Timezone.
- Customer participation.
- Dealership participants.
- Channel.
- Location.
- Meeting provider.
- Operational resources.
- Vehicle or Inventory dependencies.
- Customer Confirmation.
- Reminders.
- Check-in.
- Attendance.
- Rescheduling.
- Cancellation.
- No-show processing.
- Interaction outcome.
- Required follow-up.

### Appointment Domain Boundary

The Appointment represents the scheduling, attendance, and meeting-outcome workflow.

It does not independently prove:

- Customer identity.
- General marketing Consent.
- Vehicle availability.
- Vehicle Reservation.
- Vehicle Allocation.
- Trade-In approval.
- Customer-specific price approval.
- Finance approval.
- Contract signature.
- Payment.
- Deal finalization.
- Vehicle sale.
- Vehicle delivery.
- Accounting completion.

Those facts belong to their appropriate Canonical Domain Objects and configured authoritative systems.

### Appointment and Interaction Separation

`Appointment` represents the planned and attended engagement.

`Interaction` represents an actual communication or meaningful Customer engagement.

Examples:

- Appointment reminder message belongs to Interaction.
- Customer Confirmation response belongs to Interaction or provider evidence.
- The scheduled meeting belongs to Appointment.
- The conversation during the meeting may create one or more Interactions or structured outcome records.
- A completed Appointment does not automatically create every related business outcome.

Appointment projections may reference:

- Confirmation Interaction.
- Reminder Interactions.
- Check-in Interaction.
- Outcome Interaction.
- Follow-up Interaction.

The original communication evidence remains governed by Interaction and the communication provider.

### Appointment and Calendar Event Separation

An external calendar Event is not the Canonical Appointment.

The external Event is a provider-specific representation used for scheduling synchronization.

Appointment owns the canonical scheduling workflow where ASOS is authoritative.

An external scheduling provider may remain authoritative where configured.

ASOS must distinguish:

- Appointment requested.
- Appointment validated.
- External calendar Command sent.
- External calendar Event accepted.
- Customer Confirmation received.
- Appointment attended.
- Appointment completed.

A successful API request or calendar transport acknowledgment does not prove that the external scheduling action was authoritatively completed.

### Appointment and Inventory Separation

An Appointment may reference:

- `vehicle_id`.
- `inventory_record_id`.
- A test-drive resource.
- An operational Vehicle hold.

The Appointment itself must not create or imply a commercial Inventory Reservation or Allocation.

A test-drive resource hold is operational scheduling state.

It is distinct from:

- Customer sales Reservation.
- Deal Allocation.
- Inventory ownership.
- Commercial availability.

Current Vehicle availability and readiness must come from Inventory Record or its configured authoritative source.

### Appointment and Business Outcome Separation

Appointment outcome describes what occurred during the interaction.

It must not falsely claim completion of another governed workflow.

Examples:

```text
SIGNING_SESSION_COMPLETED
```

does not automatically mean:

```text
FinancialContractSigned
```

And:

```text
DELIVERY_HANDOFF_SESSION_COMPLETED
```

does not automatically mean:

```text
VehicleDeliveryConfirmed
```

Authoritative contract, Payment, Deal, sale, and delivery outcomes must be confirmed by their responsible Domain Services and external authorities.

### System Purpose

The Appointment Object provides canonical scheduling and attendance context to:

- Customer workflows.
- Lead and Qualified Lead workflows.
- Opportunity workflows.
- Vehicle and Inventory workflows.
- Trade-In workflows.
- Finance Application workflows.
- Quotation workflows.
- Deal workflows.
- Financial Contract workflows.
- Interaction workflows.
- Task and follow-up workflows.
- Calendar integrations.
- Reminder orchestration.
- Resource scheduling.
- AI Agents.
- Analytics.
- Audit and compliance services.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Customer identity | Customer Domain Service |
| Customer Consent and contact restrictions | Customer Consent authority |
| Canonical Appointment workflow | Appointment Domain Service |
| External provider Event | Configured scheduling or calendar provider |
| Customer Confirmation evidence | Customer response or verified provider evidence |
| Dealership participant availability | Workforce or scheduling authority |
| Vehicle identity | Vehicle Domain Service |
| Vehicle availability and readiness | Inventory Domain Service or configured external authority |
| Appointment-resource availability | Resource Scheduling Service |
| Reminder delivery | Communication provider |
| Attendance evidence | Approved check-in, Human, or provider evidence |
| Appointment outcome | Authorized Human or trusted workflow evidence |
| Quotation approval | Quotation Domain Service |
| Finance Decision | Lender or F&I authority |
| Contract signature | Contract or document-signature authority |
| Vehicle delivery | Delivery workflow and configured external authority |
| Predictions and Recommendations | Derived Intelligence |
| External scheduling completion | Configured external scheduling authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `appointment_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `department_id`.
- `sales_team_id`.
- `responsible_user_id`.
- `responsible_role_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `customer_id`.
- `lead_id`.
- `qualified_lead_id`.
- `opportunity_id`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `finance_application_id`.
- `quotation_id`.
- `financial_contract_id`.
- `deal_id`.
- `primary_interaction_id`.
- `follow_up_task_id`.

### Appointment Identity and Classification

- `appointment_number`.
- `appointment_type`.
- `status`.
- `priority`.
- `channel`.
- `source`.
- `workflow_authority_mode`.
- `data_quality_status`.
- `conflict_status`.

### Scheduling

- `scheduled_start_at`.
- `scheduled_end_at`.
- `timezone`.
- `duration_minutes`.
- `arrival_window_start_at`.
- `arrival_window_end_at`.
- `confirmation_deadline_at`.
- `cancellation_deadline_at`.
- `rescheduling_deadline_at`.
- `scheduling_freshness_status`.
- `schedule_last_verified_at`.

Timestamps should be stored in UTC while preserving the original IANA timezone.

### Customer Context

- `customer_snapshot`.
- `customer_display_name_projection`.
- `customer_preferred_language_projection`.
- `customer_preferred_channel_projection`.
- `contact_permission_status`.
- `contact_permission_checked_at`.
- `special_assistance_required`.
- `special_assistance_notes`.
- `guest_count`.

Customer projections must not replace the Customer Object.

### Participant Context

- `responsible_user_id`.
- `participant_user_ids`.
- `participant_role_requirements`.
- `external_participants`.
- `customer_participant_count`.
- `participant_confirmation_status`.
- `responsible_user_availability_status`.
- `participant_conflict_status`.

### Location and Channel

- `location_type`.
- `dealership_location_id`.
- `resource_location_id`.
- `public_location_name`.
- `address_reference`.
- `meeting_provider`.
- `meeting_url_reference`.
- `phone_contact_reference`.
- `arrival_instructions`.
- `remote_access_instructions`.
- `location_confirmation_status`.

Sensitive access instructions must not be exposed through public calendar descriptions.

### Appointment Resources

- `appointment_resource_ids`.
- `resource_hold_ids`.
- `room_resource_id`.
- `inspection_bay_resource_id`.
- `test_drive_resource_id`.
- `resource_hold_status`.
- `resource_confirmation_status`.
- `resource_hold_expires_at`.
- `resource_conflict_status`.

Resource holds must remain distinct from commercial Inventory Reservations.

### Vehicle and Test-Drive Context

- `vehicle_required`.
- `vehicle_id`.
- `inventory_record_id`.
- `vehicle_snapshot`.
- `vehicle_availability_status`.
- `vehicle_availability_confirmed_at`.
- `vehicle_availability_expires_at`.
- `test_drive_required`.
- `customer_driving_required`.
- `driver_license_required`.
- `driver_license_verification_status`.
- `driver_license_evidence_reference`.
- `insurance_verification_status`.
- `test_drive_readiness_status`.
- `test_drive_route_id`.
- `test_drive_hold_id`.
- `test_drive_hold_status`.

The Appointment must not treat `test_drive_hold_id` as a Customer sales Reservation or Deal Allocation.

### Customer Confirmation

- `customer_confirmation_status`.
- `confirmation_requested_at`.
- `confirmation_deadline_at`.
- `confirmed_at`.
- `declined_at`.
- `confirmation_channel`.
- `confirmation_interaction_id`.
- `confirmation_evidence_reference`.
- `confirmed_appointment_version`.
- `confirmation_expires_at`.

Customer Confirmation applies only to the confirmed Appointment version.

A material schedule change may invalidate the previous Confirmation.

### External Scheduling and Calendar Integration

- `external_scheduling_provider`.
- `external_calendar_id`.
- `external_event_id`.
- `external_event_version`.
- `scheduling_confirmation_status`.
- `calendar_sync_status`.
- `calendar_sync_error_code`.
- `last_calendar_sync_at`.
- `calendar_command_id`.
- `calendar_command_idempotency_key`.
- `external_confirmation_reference`.
- `reconciliation_status`.

### Reminder and Notification Context

- `reminder_policy_id`.
- `reminder_schedule`.
- `reminder_status`.
- `reminder_count`.
- `last_reminder_requested_at`.
- `last_reminder_sent_at`.
- `last_reminder_delivered_at`.
- `last_reminder_interaction_id`.
- `reminder_failure_reason`.
- `next_reminder_at`.

Reminder scheduling does not prove reminder delivery.

Delivery must be supported by communication-provider evidence.

### Check-In and Attendance

- `check_in_status`.
- `check_in_at`.
- `check_in_method`.
- `check_in_evidence_reference`.
- `actual_start_at`.
- `actual_end_at`.
- `check_out_at`.
- `attendance_status`.
- `attendance_recorded_at`.
- `attendance_recorded_by`.
- `attendance_evidence_references`.
- `late_arrival_minutes`.
- `actual_duration_minutes`.

### Appointment Outcome

- `outcome`.
- `outcome_summary`.
- `outcome_evidence_references`.
- `outcome_recorded_at`.
- `outcome_recorded_by`.
- `customer_feedback_reference`.
- `requirements_updated`.
- `follow_up_required`.
- `follow_up_reason`.
- `next_action_type`.
- `next_action_at`.
- `follow_up_task_id`.

### Rescheduling

- `reschedule_status`.
- `reschedule_count`.
- `reschedule_requested_at`.
- `reschedule_requested_by`.
- `reschedule_reason`.
- `supersedes_appointment_id`.
- `rescheduled_to_appointment_id`.
- `replacement_confirmation_status`.
- `reschedule_command_id`.
- `reschedule_idempotency_key`.

The original Appointment must remain traceable after rescheduling.

### Cancellation

- `cancellation_status`.
- `cancelled_at`.
- `cancelled_by_actor_type`.
- `cancelled_by_actor_id`.
- `cancellation_reason`.
- `cancellation_details`.
- `cancellation_evidence_references`.
- `cancellation_notification_status`.
- `resource_release_status`.
- `external_cancellation_confirmation_status`.

### No-Show

- `no_show_status`.
- `no_show_eligible_at`.
- `no_show_recorded_at`.
- `no_show_recorded_by`.
- `no_show_reason`.
- `no_show_evidence_references`.
- `no_show_follow_up_status`.

### Derived Intelligence

- `no_show_risk_score`.
- `confirmation_probability`.
- `late_arrival_risk_score`.
- `resource_conflict_risk_score`.
- `recommended_duration_minutes`.
- `recommended_time_slots`.
- `recommended_channel`.
- `recommended_reminder_schedule`.
- `recommended_follow_up_action`.
- `appointment_readiness_score`.
- `requires_human_review`.
- `derived_intelligence_expires_at`.

Every material derived output must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Supporting evidence.
- Input freshness.
- Confidence where meaningful.
- Assumptions.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required authority.

### Computed Projections

- `is_upcoming`.
- `is_overdue`.
- `is_within_check_in_window`.
- `is_late_arrival`.
- `minutes_until_appointment`.
- `minutes_since_scheduled_start`.
- `actual_duration_minutes`.
- `confirmation_overdue`.
- `reminder_due`.
- `resource_conflict_detected`.
- `participant_conflict_detected`.
- `customer_conflict_detected`.
- `appointment_age_days`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_updated_at`.
- `last_synced_at`.
- `last_sync_status`.
- `field_authority_map`.
- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `completed_at`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `appointment_id` | UUID | Yes | ASOS | Immutable Canonical Appointment identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `appointment_number` | String | Yes | ASOS or external source | Human-readable Appointment reference. |
| `customer_id` | UUID | Yes | Canonical relationship | Customer participating in the Appointment. |
| `appointment_type` | Enum | Yes | Workflow State | Defined business purpose of the Appointment. |
| `status` | Enum | Yes | Configured workflow authority | Current Appointment lifecycle state. |
| `priority` | Enum | Yes | Workflow State | Operational scheduling priority. |
| `channel` | Enum | Yes | Workflow State | Channel through which the Appointment occurs. |
| `source` | Enum | Yes | Provenance | Source through which the Appointment was initiated. |
| `workflow_authority_mode` | Enum | Yes | Configuration | Defines whether ASOS or an external provider controls scheduling state. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine status. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Current material scheduling-conflict state. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Relationship Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_id` | UUID | No | Canonical relationship | Related Lead where applicable. |
| `qualified_lead_id` | UUID | No | Canonical relationship | Related Qualified Lead where applicable. |
| `opportunity_id` | UUID | Conditional | Canonical relationship | Related active commercial pursuit. |
| `vehicle_id` | UUID | Conditional | Canonical relationship | Vehicle discussed or used in the Appointment. |
| `inventory_record_id` | UUID | Conditional | Inventory relationship | Physical stock context used for Vehicle-dependent Appointments. |
| `trade_in_id` | UUID | Conditional | Canonical relationship | Trade-In inspected or discussed. |
| `finance_application_id` | UUID | Conditional | Canonical relationship | Finance Application discussed. |
| `quotation_id` | UUID | Conditional | Canonical relationship | Quotation reviewed or presented. |
| `financial_contract_id` | UUID | Conditional | Canonical relationship | Financial Contract associated with a signing session. |
| `deal_id` | UUID | Conditional | Canonical relationship | Deal associated with signing or delivery handoff. |

### Scheduling Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `scheduled_start_at` | Timestamp | Conditional | Scheduling authority | Planned Appointment start in UTC. |
| `scheduled_end_at` | Timestamp | Conditional | Scheduling authority | Planned Appointment end in UTC. |
| `timezone` | String | Conditional | Scheduling authority | Valid IANA timezone used for display and scheduling. |
| `duration_minutes` | Integer | Conditional | Deterministic or Human Decision | Planned duration. |
| `arrival_window_start_at` | Timestamp | No | Scheduling policy | Earliest permitted check-in time. |
| `arrival_window_end_at` | Timestamp | No | Scheduling policy | Latest expected arrival time before late handling. |
| `confirmation_deadline_at` | Timestamp | No | Scheduling policy | Deadline for required Customer Confirmation. |
| `schedule_last_verified_at` | Timestamp | No | Scheduling authority | Last time the schedule was authoritatively verified. |
| `scheduling_freshness_status` | Enum | Yes | Deterministic calculation | Whether scheduling information remains current. |

### Organizational and Participant Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `dealership_id` | UUID | Yes | Canonical Projection | Responsible dealership inside the Tenant. |
| `branch_id` | UUID | Conditional | Canonical Projection | Responsible branch where applicable. |
| `responsible_user_id` | UUID | Conditional | Workflow authority | Primary User responsible for the Appointment. |
| `sales_team_id` | UUID | No | Workflow authority | Responsible team. |
| `participant_user_ids` | Array | No | Workflow authority | Additional authorized dealership participants. |
| `participant_confirmation_status` | Enum | Yes | Workflow Projection | Internal participant-confirmation state. |
| `responsible_user_availability_status` | Enum | Yes | Scheduling Projection | Availability of the responsible User. |
| `guest_count` | Integer | Yes | Customer or Human input | Number of additional Customer guests. |

### Location and Channel Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `location_type` | Enum | Yes | Workflow State | Type of physical or remote meeting location. |
| `dealership_location_id` | UUID | Conditional | Canonical relationship | Dealership location used for an in-person Appointment. |
| `meeting_provider` | Enum | Conditional | Integration configuration | Provider used for a remote meeting. |
| `meeting_url_reference` | String | Conditional | Trusted provider | Secure reference to the meeting link. |
| `phone_contact_reference` | String | Conditional | Approved contact source | Authorized phone contact used for a phone Appointment. |
| `arrival_instructions` | Text | No | Authorized Human | Customer-facing arrival instructions. |
| `location_confirmation_status` | Enum | Yes | Workflow Projection | Confirmation state of the selected location. |

### Vehicle and Resource Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `vehicle_required` | Boolean | Yes | Deterministic workflow | Whether a specific Vehicle is required. |
| `vehicle_availability_status` | Enum | Yes | Inventory Projection | Current authoritative availability projection. |
| `vehicle_availability_confirmed_at` | Timestamp | No | External Confirmation | Time availability was last confirmed. |
| `test_drive_required` | Boolean | Yes | Workflow State | Whether the Appointment includes a test drive. |
| `customer_driving_required` | Boolean | Yes | Workflow State | Whether the Customer will operate the Vehicle. |
| `driver_license_required` | Boolean | Yes | Deterministic policy | Whether licence evidence is required. |
| `driver_license_verification_status` | Enum | Yes | Verification workflow | Current licence-verification state. |
| `test_drive_readiness_status` | Enum | Yes | Inventory or readiness projection | Whether test-drive requirements are satisfied. |
| `resource_hold_status` | Enum | Yes | Resource Scheduling Service | Current operational resource-hold state. |
| `resource_confirmation_status` | Enum | Yes | Workflow Projection | External or internal resource Confirmation state. |

### Customer Confirmation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_confirmation_status` | Enum | Yes | Customer evidence projection | Customer Confirmation state for the current Appointment version. |
| `confirmation_requested_at` | Timestamp | No | ASOS | Time Confirmation was requested. |
| `confirmed_at` | Timestamp | No | Customer or provider evidence | Time valid Confirmation was received. |
| `declined_at` | Timestamp | No | Customer or provider evidence | Time Customer declined. |
| `confirmation_channel` | Enum | No | Interaction evidence | Channel used for Confirmation. |
| `confirmation_interaction_id` | UUID | No | Interaction relationship | Interaction containing Confirmation evidence. |
| `confirmation_evidence_reference` | String | No | Evidence repository | Evidence supporting the Confirmation. |
| `confirmed_appointment_version` | Integer | No | ASOS | Appointment version confirmed by the Customer. |

### Attendance Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `check_in_status` | Enum | Yes | Attendance workflow | Current check-in state. |
| `check_in_at` | Timestamp | No | Trusted evidence | Time the Customer checked in. |
| `actual_start_at` | Timestamp | No | Trusted evidence | Actual meeting start. |
| `actual_end_at` | Timestamp | No | Trusted evidence | Actual meeting end. |
| `check_out_at` | Timestamp | No | Trusted evidence | Time the Customer checked out. |
| `attendance_status` | Enum | Yes | Trusted workflow | Accepted attendance outcome. |
| `attendance_recorded_at` | Timestamp | No | ASOS | Time attendance was accepted. |
| `attendance_evidence_references` | Array | No | Evidence repository | Evidence supporting attendance. |
| `late_arrival_minutes` | Integer | No | Deterministic calculation | Accepted delay after scheduled start. |

### Outcome Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `outcome` | Enum | Conditional | Authorized Human or trusted workflow | Appointment interaction outcome. |
| `outcome_summary` | Text | No | Human or Derived Intelligence | Summary of what occurred. |
| `outcome_evidence_references` | Array | No | Evidence repository | Supporting outcome evidence. |
| `follow_up_required` | Boolean | Yes | Human or deterministic workflow | Indicates whether continued action is needed. |
| `follow_up_reason` | String | No | Human or workflow | Reason supporting follow-up. |
| `next_action_type` | Enum | No | Workflow or Recommendation | Next required or recommended action. |
| `next_action_at` | Timestamp | No | Workflow State | Due time for the next action. |
| `follow_up_task_id` | UUID | No | Task relationship | Created follow-up Task. |

### Integration Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `external_scheduling_provider` | Enum | No | Configuration | External scheduling authority or calendar provider. |
| `external_calendar_id` | String | No | External provider | External calendar identifier. |
| `external_event_id` | String | No | External provider | External Event identifier. |
| `external_event_version` | String | No | External provider | Current provider version or ETag. |
| `scheduling_confirmation_status` | Enum | Yes | Workflow Projection | External schedule-confirmation state. |
| `calendar_sync_status` | Enum | Yes | Integration workflow | Current calendar synchronization state. |
| `calendar_command_id` | UUID | No | Command relationship | Command used for the current provider operation. |
| `external_confirmation_reference` | String | No | External Confirmation | Evidence of authoritative provider outcome. |
| `reconciliation_status` | Enum | Yes | Reconciliation workflow | Current external reconciliation state. |

---

## 4. Enumerations

### AppointmentStatus

- `DRAFT`
- `SCHEDULING`
- `SCHEDULED`
- `CONFIRMATION_PENDING`
- `CONFIRMED`
- `CHECKED_IN`
- `IN_PROGRESS`
- `COMPLETED`
- `NO_SHOW`
- `CANCELLED`
- `RESCHEDULED`
- `EXPIRED`
- `ARCHIVED`

### AppointmentType

- `SALES_CONSULTATION`
- `SHOWROOM_VISIT`
- `REMOTE_CONSULTATION`
- `VEHICLE_DEMONSTRATION`
- `TEST_DRIVE`
- `TRADE_IN_INSPECTION`
- `FINANCE_CONSULTATION`
- `QUOTATION_REVIEW`
- `DOCUMENT_COLLECTION`
- `SIGNING_SESSION`
- `DELIVERY_HANDOFF`
- `FLEET_CONSULTATION`
- `FOLLOW_UP`
- `OTHER`

### AppointmentPriority

- `LOW`
- `STANDARD`
- `HIGH`
- `URGENT`
- `STRATEGIC`

Priority does not override safety, Consent, resource, Inventory, or authorization controls.

### AppointmentChannel

- `IN_PERSON`
- `PHONE`
- `VIDEO`
- `WHATSAPP_CALL`
- `OTHER_REMOTE`

### AppointmentSource

- `CUSTOMER_SELF_SERVICE`
- `SALES_CONSULTANT`
- `BDC`
- `SALES_MANAGER`
- `WEBSITE`
- `PHONE`
- `WHATSAPP`
- `EMAIL`
- `OEM_PLATFORM`
- `MARKETPLACE`
- `EXTERNAL_SCHEDULING_PROVIDER`
- `AI_ASSISTED_WORKFLOW`
- `MANUAL_ENTRY`
- `OTHER`

An AI-assisted workflow may recommend or prepare an Appointment.

It must not bypass applicable scheduling authority.

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_SCHEDULING_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### AppointmentLocationType

- `DEALERSHIP`
- `CUSTOMER_LOCATION`
- `THIRD_PARTY_LOCATION`
- `REMOTE`
- `MOBILE_SHOWROOM`
- `EXTERNAL_SERVICE_LOCATION`
- `OTHER`

### CustomerConfirmationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `CONFIRMED`
- `DECLINED`
- `EXPIRED`
- `FAILED`
- `CONFLICTED`
- `RECONFIRMATION_REQUIRED`

### SchedulingConfirmationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `CONFIRMED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### ParticipantConfirmationStatus

- `NOT_REQUIRED`
- `PENDING`
- `PARTIALLY_CONFIRMED`
- `CONFIRMED`
- `DECLINED`
- `CONFLICTED`

### AvailabilityStatus

- `UNKNOWN`
- `NOT_CHECKED`
- `AVAILABLE`
- `UNAVAILABLE`
- `PENDING_CONFIRMATION`
- `STALE`
- `CONFLICTED`
- `BLOCKED`

### ResourceHoldStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `HELD`
- `CONFIRMED`
- `RELEASE_PENDING`
- `RELEASED`
- `EXPIRED`
- `REJECTED`
- `FAILED`
- `CONFLICTED`
- `RECONCILIATION_REQUIRED`

### ReadinessStatus

- `NOT_REQUIRED`
- `NOT_READY`
- `PARTIALLY_READY`
- `READY`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### VerificationStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `PENDING`
- `VERIFIED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### CheckInStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `AVAILABLE`
- `CHECKED_IN`
- `FAILED`
- `CANCELLED`

### AppointmentAttendanceStatus

- `NOT_RECORDED`
- `ATTENDED`
- `LATE`
- `PARTIALLY_ATTENDED`
- `NO_SHOW`
- `CANCELLED_BY_CUSTOMER`
- `CANCELLED_BY_DEALERSHIP`
- `ATTENDANCE_DISPUTED`

### AppointmentOutcome

- `CONSULTATION_COMPLETED`
- `REQUIREMENTS_UPDATED`
- `VEHICLE_OPTIONS_REVIEWED`
- `VEHICLE_SELECTED_FOR_FURTHER_REVIEW`
- `TEST_DRIVE_COMPLETED`
- `TEST_DRIVE_NOT_COMPLETED`
- `TRADE_IN_INSPECTION_SESSION_COMPLETED`
- `QUOTATION_DISCUSSION_COMPLETED`
- `FINANCE_CONSULTATION_COMPLETED`
- `DOCUMENT_COLLECTION_SESSION_COMPLETED`
- `SIGNING_SESSION_COMPLETED`
- `DELIVERY_HANDOFF_SESSION_COMPLETED`
- `FOLLOW_UP_REQUIRED`
- `CUSTOMER_UNDECIDED`
- `CUSTOMER_NOT_INTERESTED`
- `CUSTOMER_REQUESTED_RESCHEDULE`
- `NO_ACTION_COMPLETED`
- `OTHER`

Appointment outcome must not replace authoritative outcomes from Quotation, Trade-In, Finance Application, Financial Contract, Deal, Payment, or Delivery workflows.

### AppointmentCancellationReason

- `CUSTOMER_REQUEST`
- `CUSTOMER_UNAVAILABLE`
- `DEALERSHIP_UNAVAILABLE`
- `RESPONSIBLE_USER_UNAVAILABLE`
- `VEHICLE_UNAVAILABLE`
- `RESOURCE_UNAVAILABLE`
- `LOCATION_UNAVAILABLE`
- `WEATHER_OR_EMERGENCY`
- `DUPLICATE_APPOINTMENT`
- `OPPORTUNITY_CLOSED`
- `DEAL_CANCELLED`
- `CONTACT_RESTRICTED`
- `COMPLIANCE_BLOCK`
- `SAFETY_BLOCK`
- `DATA_CONFLICT`
- `EXTERNAL_PROVIDER_REJECTED`
- `OTHER`

### AppointmentNoShowReason

- `UNKNOWN`
- `CUSTOMER_FORGOT`
- `CUSTOMER_UNREACHABLE`
- `CUSTOMER_DELAYED`
- `TRANSPORTATION_ISSUE`
- `SCHEDULING_MISUNDERSTANDING`
- `CUSTOMER_CHANGED_MIND`
- `PROVIDER_ACCESS_FAILURE`
- `LOCATION_CONFUSION`
- `OTHER`

A no-show reason may remain `UNKNOWN`.

ASOS must not invent a reason.

### RescheduleStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `VALIDATING`
- `REPLACEMENT_PENDING`
- `REPLACEMENT_CONFIRMED`
- `COMPLETED`
- `REJECTED`
- `FAILED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### CancellationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `PENDING_CONFIRMATION`
- `CONFIRMED`
- `REJECTED`
- `FAILED`
- `RECONCILIATION_REQUIRED`

### ReminderStatus

- `NOT_REQUIRED`
- `NOT_SCHEDULED`
- `SCHEDULED`
- `PENDING_APPROVAL`
- `ACTIVE`
- `PARTIALLY_SENT`
- `COMPLETED`
- `FAILED`
- `SUSPENDED`
- `CANCELLED`

### CalendarProvider

- `NONE`
- `GOOGLE_CALENDAR`
- `MICROSOFT_OUTLOOK`
- `APPLE_CALENDAR`
- `CALENDLY`
- `INTERNAL_SCHEDULER`
- `CRM_SCHEDULER`
- `OEM_SCHEDULER`
- `OTHER`

### CalendarSyncStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `CREATE_PENDING`
- `UPDATE_PENDING`
- `DELETE_PENDING`
- `PENDING_CONFIRMATION`
- `SYNCED`
- `FAILED`
- `CONFLICTED`
- `DELETED_EXTERNALLY`
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

### NextActionType

- `CONTACT_CUSTOMER`
- `SEND_CONFIRMATION_REQUEST`
- `SEND_REMINDER`
- `REQUEST_RESCHEDULE`
- `CREATE_REPLACEMENT_APPOINTMENT`
- `UPDATE_REQUIREMENTS`
- `PREPARE_VEHICLE_OPTIONS`
- `REQUEST_TEST_DRIVE_HOLD`
- `PREPARE_QUOTATION`
- `FOLLOW_UP_QUOTATION`
- `REQUEST_TRADE_IN_WORKFLOW`
- `REQUEST_FINANCE_WORKFLOW`
- `REQUEST_DOCUMENTS`
- `REQUEST_APPROVAL`
- `CREATE_DEAL_REVIEW`
- `CREATE_DELIVERY_REVIEW`
- `ESCALATE`
- `CLOSE_FOLLOW_UP`
- `OTHER`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, department, team, User, location, and resource must belong to the authorized Tenant.
- All linked Domain Objects must belong to the same authorized Tenant scope.
- Cross-Tenant Appointment scheduling, matching, search, AI retrieval, export, and synchronization are prohibited unless governed through an approved and auditable mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Appointment Creation Rules

Appointment creation requires:

- Valid Customer.
- Valid Appointment type.
- Valid Tenant and organizational context.
- Valid source.
- Valid workflow-authority mode.
- Valid scheduling intent.
- Record-version controls where applicable.
- Idempotency for retryable creation.
- Conflict checks.
- Required contact-permission assessment.
- Required resource and participant checks.

A new Appointment may remain `DRAFT` before complete scheduling information exists.

It must not become `SCHEDULED` until all required conditions pass.

### Customer Rules

- Every Appointment must reference one resolved Customer.
- Submitted or projected Customer details must not replace canonical Customer identity.
- Customer restrictions and permitted contact basis must be evaluated before outbound Confirmation or reminder activity.
- General marketing Consent must not be inferred from Appointment creation.
- A Customer-requested Appointment may permit necessary transactional communication only where law and policy allow it.
- Customer preference does not override legal, safety, resource, or availability restrictions.
- Customer Confirmation applies only to the current Appointment version.

### Scheduling Rules

- `scheduled_end_at` must be later than `scheduled_start_at`.
- `duration_minutes` must equal the approved planned duration.
- `timezone` must be a valid IANA timezone.
- Daylight-saving and offset transitions must be handled by the Scheduling Service.
- The original timezone must remain preserved.
- Appointment time must comply with configured:
  - Business hours.
  - Resource hours.
  - Employee availability.
  - Customer restrictions.
  - Legal restrictions.
  - Location restrictions.
- Scheduling outside configured boundaries requires the applicable authorized exception.
- A past Appointment may be imported only through an approved historical-import or correction workflow.

### Workflow Authority Rules

When `workflow_authority_mode = ASOS_AUTHORITATIVE`:

- Appointment Domain Service may accept the canonical schedule after deterministic validation.
- External calendar synchronization may remain a secondary projection.

When `workflow_authority_mode = EXTERNAL_SCHEDULING_AUTHORITATIVE`:

- ASOS must create an approved Command.
- The Appointment remains `SCHEDULING` until authoritative external Confirmation is received.
- Transport success does not mean the Appointment is externally scheduled.
- Missing Confirmation must trigger timeout, polling, reconciliation, or escalation.

When `workflow_authority_mode = GOVERNED_BIDIRECTIONAL`:

- Field-level authority must be configured.
- Version conflicts must be detected.
- External and ASOS changes must not silently overwrite each other.
- Reconciliation is required for conflicting schedules.

### Participant Rules

- Every active Appointment must have an authorized responsible User, team, or queue.
- Required participant roles must be satisfied.
- Participants must be available for the full required scheduling window unless approved capacity rules permit overlap.
- A User must not be double-booked without an authorized and recorded exception.
- Assignment does not grant unrelated commercial or legal approval authority.
- Participant changes must preserve history.

### Location Rules

- In-person Appointments require an approved location.
- Dealership-based Appointments require a valid dealership location.
- Customer-location or off-site Appointments may require:
  - Address verification.
  - Travel-time validation.
  - Safety review.
  - Additional approval.
- Remote Appointments require an approved provider or channel.
- Meeting links must be generated or stored securely.
- Public descriptions must not expose sensitive access information.
- Location changes may require Customer reconfirmation.

### Resource Rules

- Required rooms, bays, equipment, and operational resources must be available for the full scheduled period.
- Resource holds must use concurrency-safe operations.
- Resource holds must expire or be released when appropriate.
- A resource hold does not create a commercial Inventory Reservation.
- Appointment cancellation, rescheduling, or expiration must trigger resource-release processing.
- A failed release must create reconciliation.
- Resource conflicts must not be silently ignored.

### Vehicle and Inventory Rules

A Vehicle-dependent Appointment requires:

- Valid `vehicle_id`.
- Valid physical `inventory_record_id` where a specific unit is required.
- Current Inventory availability.
- Current physical location.
- Current readiness.
- No incompatible safety, quality, legal, compliance, Reservation, Allocation, sale, or delivery block.
- Sufficient freshness.

A Vehicle selected for discussion does not require physical Inventory.

A Vehicle selected for a test drive normally requires a current physical Inventory Record.

Appointment scheduling must not:

- Mark the Vehicle commercially reserved.
- Allocate the Vehicle to a Deal.
- Change authoritative Inventory availability.
- Confirm a sale.
- Confirm delivery.

### Test-Drive Rules

A Customer-driven test drive requires:

- Approved test-drive Appointment type.
- Eligible Vehicle and Inventory Record.
- Current test-drive readiness.
- Valid operational resource hold.
- Required insurance evidence.
- Required Customer driving-licence verification.
- Applicable age and jurisdiction checks.
- Approved route or operational boundary where required.
- No active safety, compliance, legal, or quality block.
- Authorized participant supervision where required.

AI Agents must not independently verify:

- Driving licence.
- Insurance.
- Vehicle roadworthiness.
- Customer legal eligibility.

A completed test-drive Appointment confirms only that the governed interaction occurred.

It does not prove purchase intent, Reservation, sale, or delivery.

### Customer Confirmation Rules

- Customer Confirmation requires verifiable Customer or authorized participant evidence.
- Confirmation must identify the Appointment version confirmed.
- A material change to time, location, channel, Vehicle dependency, or purpose may require reconfirmation.
- Provider delivery alone does not prove Customer Confirmation.
- An automated calendar acceptance may be accepted only under the configured evidence policy.
- `CONFIRMED` must not be assigned solely from AI inference.
- Declined or expired Confirmation must trigger applicable rescheduling, cancellation, or Human Review.

### Reminder Rules

Reminder activity must comply with:

- Customer contact permission.
- Channel restrictions.
- Purpose.
- Frequency limits.
- Quiet hours.
- Customer language.
- Appointment state.
- Confirmation state.
- Cancellation state.
- Applicable automation policy.

Action Class 2 reminder activity may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate the policy before sending.

AI Agents may draft reminder content.

They must not transmit it directly outside the approved Command workflow.

Reminder count must reflect accepted outbound attempts according to the configured metric policy.

Reminder delivery must preserve provider evidence.

### Check-In Rules

- Check-in must occur within the configured check-in window unless an authorized exception exists.
- Check-in evidence may include:
  - Authorized Human entry.
  - Secure kiosk.
  - Approved Customer self-check-in.
  - Verified remote-provider join Event.
  - Approved location or access evidence.
- Check-in does not prove the interaction started.
- Check-in does not prove completion.
- Check-in correction requires audit evidence.

### Attendance Rules

- `ATTENDED` requires accepted participation evidence.
- `LATE` requires accepted attendance after the configured threshold.
- `PARTIALLY_ATTENDED` requires evidence that the interaction started but ended before required completion.
- Attendance must not be inferred solely from calendar status.
- Provider join data may support attendance but must be evaluated under approved evidence policy.
- Conflicting attendance evidence must create Human Review.
- Attendance status must remain historically traceable.

### Completion Rules

An Appointment may become `COMPLETED` only when:

- The interaction actually occurred.
- `actual_start_at` is populated.
- `actual_end_at` is populated.
- An authorized actor or trusted workflow recorded the outcome.
- Required evidence exists.
- Required follow-up was evaluated.
- The record version is current.
- No blocking attendance conflict exists.

Appointment completion must not automatically mark:

- Opportunity won.
- Quotation accepted.
- Trade-In approved.
- Finance approved.
- Contract signed.
- Payment completed.
- Vehicle sold.
- Vehicle delivered.

Those changes require separate governed workflows.

### Outcome Rules

- `outcome` is required when `status = COMPLETED`.
- Outcome must describe the interaction, not falsely claim another Domain outcome.
- Outcome summaries generated by AI remain Derived Intelligence until accepted where required.
- A follow-up Task must be created when the accepted outcome requires continued action.
- A required follow-up must include owner or queue, due time, and business purpose.
- Sensitive notes must not be stored in unrestricted fields.

### No-Show Rules

A no-show may be recorded only when:

- Scheduled start and configured grace period have passed.
- No accepted check-in or attendance evidence exists.
- The Appointment was not validly cancelled.
- Provider and communication evidence were considered where required.
- The no-show Decision authority is valid.
- Follow-up or closure policy was applied.

AI no-show prediction must not create authoritative `NO_SHOW` state.

The no-show reason may remain unknown.

A no-show Appointment must not return to an active state through an ordinary update.

A new Appointment must be created for rescheduling.

### Cancellation Rules

Cancellation requires:

- Valid cancellation reason.
- Authorized actor or applicable source evidence.
- Resource-release handling.
- External calendar cancellation handling where applicable.
- Customer notification where permitted and required.
- Follow-up handling.
- Audit evidence.

When an external scheduling provider is authoritative:

- Cancellation remains pending until authoritative Confirmation.
- A sent cancellation Command does not prove cancellation.
- Missing Confirmation must trigger reconciliation.

### Rescheduling Rules

Rescheduling must:

- Preserve the original Appointment.
- Create a new replacement Appointment.
- Reference the original through `supersedes_appointment_id`.
- Reference the replacement through `rescheduled_to_appointment_id`.
- Use an idempotent controlled workflow.
- Validate the new schedule and resources.
- Preserve the rescheduling reason.
- Release or safely transfer old resource holds.
- Reconfirm the Customer where required.
- Update external providers through approved Commands.
- Mark the original `RESCHEDULED` only after the replacement is accepted under the applicable authority model.

A material schedule change must not silently modify the historical original Appointment.

### External Calendar Rules

- External Event identifiers and versions must be preserved.
- Provider webhook deliveries must use source deduplication.
- Event Consumers must prevent duplicate effects using `event_id`.
- Retryable provider Commands must use `idempotency_key`.
- External updates must validate current provider version where available.
- External deletion must not silently delete the Canonical Appointment.
- External conflicts must create reconciliation.
- Provider data must not override higher-authority Customer, safety, legal, Inventory, or attendance evidence.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Appointment creation must support idempotency.
- Rescheduling must support idempotency.
- Cancellation Commands must support idempotency.
- Resource-hold Commands must support idempotency.
- Calendar synchronization Commands must support idempotency.
- Reminder Commands must support idempotency.
- Duplicate retries must not create duplicate:
  - Appointments.
  - Replacement Appointments.
  - Resource holds.
  - Calendar Events.
  - Reminders.
  - Follow-up Tasks.
- Event Consumers must prevent duplicate effects using `event_id`.

### Human Review Requirements

Human Review is required according to policy for:

- Scheduling conflict.
- Customer identity conflict.
- Participant conflict.
- Resource conflict.
- Vehicle-availability conflict.
- Driving-licence dispute.
- Attendance dispute.
- No-show dispute.
- Off-site or restricted test drive.
- High-risk Vehicle use.
- Reopening or correcting a terminal Appointment.
- External scheduling conflict.
- Repeated synchronization failure.
- Material outcome correction.
- Another high-risk exception.

---

## 6. State Machine

### Allowed States

```text
DRAFT
SCHEDULING
SCHEDULED
CONFIRMATION_PENDING
CONFIRMED
CHECKED_IN
IN_PROGRESS
COMPLETED
NO_SHOW
CANCELLED
RESCHEDULED
EXPIRED
ARCHIVED
```

### Principal Allowed Transitions

```text
DRAFT → SCHEDULING
DRAFT → SCHEDULED
DRAFT → CANCELLED
DRAFT → EXPIRED

SCHEDULING → SCHEDULED
SCHEDULING → CANCELLED
SCHEDULING → EXPIRED

SCHEDULED → CONFIRMATION_PENDING
SCHEDULED → CONFIRMED
SCHEDULED → CHECKED_IN
SCHEDULED → NO_SHOW
SCHEDULED → CANCELLED
SCHEDULED → RESCHEDULED
SCHEDULED → EXPIRED

CONFIRMATION_PENDING → CONFIRMED
CONFIRMATION_PENDING → SCHEDULED
CONFIRMATION_PENDING → CHECKED_IN
CONFIRMATION_PENDING → NO_SHOW
CONFIRMATION_PENDING → CANCELLED
CONFIRMATION_PENDING → RESCHEDULED
CONFIRMATION_PENDING → EXPIRED

CONFIRMED → CHECKED_IN
CONFIRMED → IN_PROGRESS
CONFIRMED → NO_SHOW
CONFIRMED → CANCELLED
CONFIRMED → RESCHEDULED

CHECKED_IN → IN_PROGRESS
CHECKED_IN → CANCELLED

IN_PROGRESS → COMPLETED
IN_PROGRESS → CANCELLED

COMPLETED → ARCHIVED
NO_SHOW → ARCHIVED
CANCELLED → ARCHIVED
RESCHEDULED → ARCHIVED
EXPIRED → ARCHIVED
```

### Forbidden Ordinary Transitions

```text
DRAFT → CHECKED_IN
DRAFT → IN_PROGRESS
DRAFT → COMPLETED
DRAFT → NO_SHOW

SCHEDULING → CHECKED_IN
SCHEDULING → IN_PROGRESS
SCHEDULING → COMPLETED

SCHEDULED → COMPLETED

CONFIRMATION_PENDING → COMPLETED

CONFIRMED → COMPLETED

NO_SHOW → CHECKED_IN
NO_SHOW → IN_PROGRESS
NO_SHOW → COMPLETED

CANCELLED → SCHEDULED
CANCELLED → CONFIRMED
CANCELLED → COMPLETED

RESCHEDULED → SCHEDULED
RESCHEDULED → CONFIRMED
RESCHEDULED → IN_PROGRESS

COMPLETED → SCHEDULED
COMPLETED → CANCELLED
COMPLETED → RESCHEDULED

ARCHIVED → SCHEDULED
ARCHIVED → IN_PROGRESS
ARCHIVED → COMPLETED
```

Corrections to terminal outcomes require a separate governed correction or reopening workflow.

### Entering DRAFT

Requires:

- Valid Tenant context.
- Customer.
- Appointment purpose.
- Source.
- Creation authority.
- Initial audit evidence.

A schedule may remain incomplete.

### Entering SCHEDULING

Requires:

- Proposed start and end time.
- Timezone.
- Responsible organizational context.
- Required participant and resource checks initiated.
- External scheduling Command where applicable.
- Idempotency protection.

### Entering SCHEDULED

Requires:

- Valid accepted schedule.
- Valid responsible User, team, or queue.
- Required location.
- Required participant availability.
- Required resource availability.
- Required Vehicle and Inventory checks.
- Required contact-permission assessment.
- External scheduling Confirmation when the external provider is authoritative.

### Entering CONFIRMATION_PENDING

Requires:

- Appointment is scheduled.
- Customer Confirmation is required.
- Valid permitted Confirmation request.
- Request timestamp.
- Channel.
- Appointment version.
- Confirmation deadline.

### Entering CONFIRMED

Requires:

- Valid Customer or authorized participant Confirmation.
- Confirmation evidence.
- Confirmed Appointment version.
- Current schedule.
- Required resources remain available.
- No blocking conflict.

### Entering CHECKED_IN

Requires:

- Appointment is within the permitted check-in window or authorized exception.
- Accepted check-in evidence.
- Required identity or attendance controls.
- `check_in_at`.

### Entering IN_PROGRESS

Requires:

- Accepted Customer participation evidence.
- Responsible User or authorized replacement.
- Required location and resources.
- Required Vehicle readiness.
- Required test-drive eligibility.
- `actual_start_at`.
- No blocking safety, legal, compliance, or resource condition.

### Entering COMPLETED

Requires:

- Interaction ended.
- `actual_end_at`.
- `completed_at`.
- Accepted outcome.
- Outcome authority.
- Required evidence.
- Follow-up evaluation.
- No unresolved attendance dispute.

### Entering NO_SHOW

Requires:

- Scheduled time and grace period passed.
- No accepted Customer attendance.
- Appointment was not validly cancelled.
- No conflicting provider evidence.
- Accepted no-show Decision.
- No-show timestamp.
- Follow-up evaluation.

### Entering CANCELLED

Requires:

- Valid cancellation reason.
- Cancellation authority.
- Resource-release workflow.
- Provider cancellation workflow where applicable.
- Customer notification evaluation.
- Audit evidence.

### Entering RESCHEDULED

Requires:

- Replacement Appointment created.
- Replacement Appointment accepted under its scheduling-authority model.
- Rescheduling references linked.
- Resource handling completed or reconciled.
- Old Appointment no longer active.

### Entering EXPIRED

May occur when:

- Draft scheduling window expired.
- External scheduling request expired.
- Required Customer Confirmation expired under policy.
- Appointment became unusable before activation.
- Applicable expiration policy was satisfied.

An Appointment that passed its scheduled time must normally be evaluated for attendance, no-show, cancellation, or correction rather than automatically expired.

### Terminal States

For ordinary processing:

- `COMPLETED`
- `NO_SHOW`
- `CANCELLED`
- `RESCHEDULED`
- `EXPIRED`
- `ARCHIVED`

`ARCHIVED` is terminal.

### Correction and Reopening

Correcting a terminal Appointment requires:

- Authorized Human Decision.
- Correction reason.
- Supporting evidence.
- Previous state.
- Corrected state or replacement record.
- Related Domain reconciliation.
- New record version.
- New Event.
- Audit history.

AI Agents must not independently reopen or correct terminal Appointment outcomes.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied policy.
- Record version.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Human Decision.
- Related automation policy.
- Related Command.
- Related External Confirmation.

---

## 7. Relationships

### Tenant

- Every Appointment belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant scheduling requires an approved and auditable mechanism.

### Customer

- Every Appointment references one Customer.
- One Customer may have multiple Appointments.
- Customer identity and Consent remain governed by Customer.
- Appointment snapshots must not replace current Customer authority.

### Lead

- An Appointment may reference the Lead that initiated the scheduling request.
- Lead inquiry does not prove Appointment Confirmation.
- Appointment state must not rewrite original Lead evidence.

### Qualified Lead

- A Qualified Lead may create or support an Appointment.
- Appointment scheduling does not alter qualification evidence.
- Qualification expiration may require Appointment review but does not automatically cancel the Appointment.

### Opportunity

- An Appointment may support one Opportunity.
- Opportunity stage may be informed by Appointment evidence.
- Appointment status must not automatically change Opportunity stage without the governed Opportunity workflow.
- Appointment completion does not make the Opportunity `WON`.

### Vehicle

- Appointment may reference one Vehicle identity or configuration.
- Vehicle identity remains governed by Vehicle.
- Vehicle reference does not prove physical availability.

### Inventory Record

- Vehicle-dependent Appointments may reference one physical Inventory Record.
- Inventory Record owns availability, location, readiness, Reservation, and Allocation.
- Appointment may request a test-drive operational hold.
- Appointment must not create a commercial Reservation or Allocation implicitly.

### Trade-In

- A Trade-In inspection Appointment may reference one Trade-In workflow.
- Appointment outcome may state that the inspection session occurred.
- Appraisal, ownership, lien, payoff, and acquisition approval remain governed by Trade-In.

### Finance Application

- A finance-consultation Appointment may reference a Finance Application.
- Consultation completion does not prove submission, lender Decision, or approval.
- Restricted finance data must not be duplicated unnecessarily.

### Quotation

- A Quotation-review Appointment may reference one or more Quotations.
- Appointment outcome may state that a Quotation discussion occurred.
- Quotation approval, expiry, acceptance, and commercial terms remain governed by Quotation.

### Financial Contract

- A signing-session Appointment may reference a Financial Contract.
- Appointment completion does not prove authoritative signature or contract activation.
- Signature provider or contract authority must confirm the contract outcome.

### Deal

- Signing and delivery-handoff Appointments may reference a Deal.
- Appointment completion does not finalize the Deal.
- Deal state remains governed by Deal and configured external authority.

### Interaction

Interactions may provide:

- Appointment request evidence.
- Confirmation request.
- Customer Confirmation.
- Reminder delivery.
- Customer cancellation.
- Customer rescheduling request.
- Appointment outcome discussion.
- Follow-up communication.

Appointment may reference relevant Interactions without duplicating original content.

### Task

Appointment may create:

- Preparation Task.
- Confirmation Task.
- Reminder Task.
- Resource Task.
- Follow-up Task.
- Review Task.
- Reconciliation Task.

Task completion does not automatically prove the related Appointment or business outcome.

### Calendar Provider

An Appointment may be projected to one or more external calendar systems.

The provider Event must remain linked through:

- Provider.
- Calendar identifier.
- External Event identifier.
- External version.
- Command.
- Confirmation.
- Reconciliation.

### Appointment Resources

Appointment may reference:

- Room.
- Desk.
- Inspection bay.
- Test-drive resource.
- Demonstration area.
- Delivery bay.
- Remote-meeting resource.
- Another approved operational resource.

Resource Scheduling Service remains responsible for resource availability and holds.

### Rescheduling Relationship

A rescheduled Appointment must reference:

```text
rescheduled_to_appointment_id
```

The replacement must reference:

```text
supersedes_appointment_id
```

Circular rescheduling chains are prohibited.

### Supporting Child Records

Appointment may own or govern:

- Schedule versions.
- Participant records.
- Resource holds.
- Customer Confirmation records.
- Reminder schedules.
- Attendance records.
- Outcome records.
- Cancellation records.
- Rescheduling records.
- External calendar mappings.
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

The following are required Appointment Event concepts and do not replace the Event Catalog.

### Appointment Creation and Scheduling Event Concepts

- Appointment draft created.
- Appointment scheduling requested.
- Appointment scheduling validated.
- Appointment scheduling rejected.
- Appointment scheduled.
- External scheduling Command created.
- External scheduling Command sent.
- External schedule Confirmation received.
- External schedule rejected.
- Scheduling reconciliation required.

### Customer Confirmation Event Concepts

- Customer Confirmation requested.
- Customer confirmed Appointment.
- Customer declined Appointment.
- Customer Confirmation expired.
- Appointment reconfirmation required.
- Customer Confirmation conflict detected.

### Participant and Resource Event Concepts

- Appointment participant assigned.
- Appointment participant changed.
- Resource hold requested.
- Resource hold confirmed.
- Resource hold rejected.
- Resource hold released.
- Resource conflict detected.
- Participant conflict detected.

### Vehicle and Test-Drive Event Concepts

- Vehicle availability check requested.
- Vehicle availability confirmed.
- Vehicle became unavailable.
- Test-drive readiness confirmed.
- Test-drive eligibility blocked.
- Test-drive resource hold requested.
- Test-drive resource hold confirmed.

These Events must not imply a commercial Inventory Reservation or Allocation.

### Reminder Event Concepts

- Appointment reminder scheduled.
- Appointment reminder approval requested.
- Appointment reminder Command sent.
- Appointment reminder delivered.
- Appointment reminder failed.
- Appointment reminders suspended.

### Attendance Event Concepts

- Appointment check-in recorded.
- Appointment started.
- Appointment ended.
- Appointment attendance recorded.
- Appointment attendance disputed.
- Appointment no-show recorded.
- Appointment no-show corrected.

### Outcome Event Concepts

- Appointment outcome recorded.
- Appointment follow-up required.
- Appointment follow-up Task created.
- Appointment outcome corrected.

Outcome Events must not imply authoritative completion of another Domain workflow.

### Rescheduling and Cancellation Event Concepts

- Appointment rescheduling requested.
- Replacement Appointment created.
- Appointment rescheduled.
- Appointment cancellation requested.
- Appointment cancellation Command sent.
- Appointment cancellation confirmed.
- Appointment cancellation failed.
- Appointment expired.
- Appointment archived.

### Derived Intelligence Event Concepts

- Appointment no-show risk updated.
- Appointment Confirmation probability updated.
- Appointment time-slot Recommendation generated.
- Appointment reminder Recommendation generated.
- Appointment readiness score updated.
- Appointment Human Review recommended.

Derived Intelligence Events must not imply:

- Customer Confirmation.
- Attendance.
- No-show.
- Vehicle availability.
- Test-drive eligibility.
- Contract signature.
- Vehicle delivery.
- Human Approval.
- External completion.

### Producer Rules

- Appointment Domain Service publishes accepted Appointment canonical and workflow-state changes.
- Interaction Domain Service publishes accepted communication facts.
- Customer Domain Service publishes accepted Customer identity and Consent facts.
- Inventory Domain Service publishes accepted Vehicle availability and readiness facts.
- Resource Scheduling Service publishes accepted resource-hold facts.
- Integration services publish external source observations.
- AI Agents may publish Agent-run, analysis, prediction, or Recommendation Events.
- AI Agents must not publish authoritative Customer Confirmation, attendance, no-show, external scheduling, contract, sale, or delivery Events merely because they predicted or recommended the result.

### Event Requirements

Every material Appointment Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `appointment_id`.
- `customer_id`.
- Opportunity identifier where applicable.
- Dealership and branch context.
- Scheduled time and timezone where applicable.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Evidence references.
- Applied policy.
- Related Human Decision.
- Related automation policy.
- Related Command.
- Related External Confirmation.
- Security classification.

Events are immutable.

Corrections, cancellation, rescheduling, release, and reversal must use new Events linked to the original Event.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Appointment-intent classification.
- Appointment-type Recommendation.
- Time-slot Recommendation.
- Duration Recommendation.
- Participant Recommendation.
- Location Recommendation.
- Channel Recommendation.
- Customer-language adaptation.
- Reminder-content drafting.
- Reminder-timing Recommendation.
- No-show-risk prediction.
- Confirmation-probability prediction.
- Resource-conflict detection.
- Vehicle-readiness issue detection.
- Appointment-summary generation.
- Outcome-summary drafting.
- Follow-up Recommendation.
- Data-quality issue detection.
- Human Review preparation.
- Calendar-conflict explanation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create or change Customer Consent.
- Reverse Customer contact restrictions.
- Confirm Customer identity.
- Confirm Customer attendance.
- Mark an Appointment `NO_SHOW`.
- Mark an Appointment `COMPLETED`.
- Confirm driving-licence validity.
- Confirm insurance validity.
- Confirm Vehicle availability.
- Confirm Vehicle readiness.
- Create a commercial Vehicle Reservation.
- Allocate a Vehicle.
- Remove safety or compliance blocks.
- Confirm contract signature.
- Confirm Payment.
- Confirm sale.
- Confirm Vehicle delivery.
- Reopen a terminal Appointment.
- Override resource conflicts.
- Execute external calendar or communication Commands directly.
- Access Appointment data outside authorized Tenant scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Appointment identifier and record version.
- Supporting evidence.
- Source authority.
- Input freshness.
- Applied Business Rules.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Generated timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority or automation policy.

### Time-Slot Recommendations

AI may recommend available time slots.

Final validation must be deterministic and must consider:

- Customer constraints.
- User availability.
- Resource availability.
- Location hours.
- Travel time.
- Vehicle availability.
- Test-drive readiness.
- Timezone.
- External provider state.
- Existing holds.
- Required preparation time.

An AI Recommendation must not be represented as a confirmed Appointment.

### No-Show Prediction

No-show risk is Derived Intelligence.

It may support:

- Reminder timing.
- Human outreach.
- Confirmation review.
- Scheduling Recommendation.
- Operational planning.

It must not:

- Cancel the Appointment automatically without applicable policy.
- Mark the Customer as unreliable.
- Create adverse Customer treatment based on protected attributes.
- Create authoritative no-show state.

### Reminder Generation

AI may draft Customer-facing reminder content only when:

- The communication purpose is permitted.
- The channel is permitted.
- The Appointment is active.
- The schedule is current.
- Customer language is known or safely selected.
- The template is approved.
- Sensitive information is minimized.
- Human Approval or an approved automation policy applies.

The deterministic Policy Engine must validate the outbound Command.

### Outcome Summarization

AI may summarize accepted Human or trusted workflow notes.

AI must distinguish:

- Direct observation.
- Customer statement.
- Authorized Human Decision.
- Derived inference.
- Missing information.
- Related external outcome.

AI must not transform:

```text
Signing session completed
```

into:

```text
Contract signed
```

without authoritative signature evidence.

AI must not transform:

```text
Delivery handoff session completed
```

into:

```text
Vehicle delivered
```

without authoritative delivery Confirmation.

### Action Class 2

Controlled Reminder, Confirmation-request, and follow-up activity may proceed through:

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
- Appointment status.
- Schedule freshness.
- Customer restrictions.
- Revocation.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

High-impact Appointment-related Decisions may require an Authoritative Human Decision.

Examples include:

- High-risk off-site test drive.
- Restricted Vehicle use.
- Safety-block override.
- Participant or resource double-booking override.
- Delivery-handoff authorization.
- Terminal outcome correction.
- Cross-Tenant or exceptional resource handling.
- Legal or compliance exception.

### AI Context and Embeddings

Direct identifiers, access instructions, and sensitive evidence must not enter unrestricted embeddings.

Normally excluded fields include:

- Customer name.
- Phone.
- Email.
- Physical address.
- Exact secure location.
- Meeting access token.
- Driving-licence evidence.
- Insurance evidence.
- Identity evidence.
- Contract documents.
- Finance information.
- Payment information.
- Consent evidence.
- Raw attendance evidence.

Approved redacted context may include:

- Appointment type.
- Non-sensitive purpose.
- General location category.
- Time-window category.
- Customer-language preference.
- Non-sensitive outcome summary.
- Follow-up category.
- Vehicle category.
- General readiness status.

Every vector entry must enforce:

- `tenant_id`.
- Appointment access scope.
- Source references.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Explainability

Material Appointment Recommendations must explain:

- Evidence used.
- Data freshness.
- Customer restrictions.
- Participant availability.
- Resource availability.
- Vehicle and Inventory dependencies.
- Timezone.
- Material conflicts.
- Assumptions.
- Confidence where meaningful.
- Required authority.
- External Confirmation requirement.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Appointment API behaviour.

### REST Resources

```text
GET    /api/v1/appointments
POST   /api/v1/appointments
GET    /api/v1/appointments/{appointment_id}
PATCH  /api/v1/appointments/{appointment_id}

POST   /api/v1/appointments/{appointment_id}/scheduling-requests
POST   /api/v1/appointments/{appointment_id}/confirmation-requests
POST   /api/v1/appointments/{appointment_id}/participant-requests
POST   /api/v1/appointments/{appointment_id}/resource-hold-requests
POST   /api/v1/appointments/{appointment_id}/resource-release-requests
POST   /api/v1/appointments/{appointment_id}/vehicle-readiness-checks
POST   /api/v1/appointments/{appointment_id}/reminder-requests
POST   /api/v1/appointments/{appointment_id}/check-in
POST   /api/v1/appointments/{appointment_id}/start
POST   /api/v1/appointments/{appointment_id}/complete
POST   /api/v1/appointments/{appointment_id}/attendance-decisions
POST   /api/v1/appointments/{appointment_id}/no-show-decisions
POST   /api/v1/appointments/{appointment_id}/rescheduling-requests
POST   /api/v1/appointments/{appointment_id}/cancellation-requests
POST   /api/v1/appointments/{appointment_id}/correction-requests

GET    /api/v1/appointments/{appointment_id}/history
GET    /api/v1/appointments/{appointment_id}/attendance-evidence
GET    /api/v1/appointments/{appointment_id}/external-synchronization
GET    /api/v1/appointments/{appointment_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, User, team, location, and resource scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Create Request

```json
{
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "appointment_type": "TEST_DRIVE",
  "priority": "STANDARD",
  "channel": "IN_PERSON",
  "source": "SALES_CONSULTANT",
  "workflow_authority_mode": "ASOS_AUTHORITATIVE",
  "responsible_user_id": "0f63b9de-97fd-42f6-a53d-531155afdf56",
  "schedule": {
    "scheduled_start_at": "2026-08-05T07:00:00Z",
    "scheduled_end_at": "2026-08-05T07:45:00Z",
    "timezone": "Africa/Cairo"
  },
  "location": {
    "location_type": "DEALERSHIP",
    "dealership_location_id": "b8c98d43-566e-4a55-b058-b1e07610287a"
  },
  "vehicle_context": {
    "vehicle_required": true,
    "vehicle_id": "550e8400-e29b-41d4-a716-446655440000",
    "inventory_record_id": "123e4567-e89b-12d3-a456-426614174000",
    "test_drive_required": true,
    "customer_driving_required": true
  }
}
```

The request must include an HTTP header such as:

```text
Idempotency-Key: 08a8ec06-b3d3-4ddb-a661-94932ecb70aa
```

### Example Scheduled Response

```json
{
  "appointment_id": "6efa31fd-27a3-4de9-8105-f6c920fa09c7",
  "appointment_number": "APT-2026-001842",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "appointment_type": "TEST_DRIVE",
  "status": "SCHEDULED",
  "scheduled_start_at": "2026-08-05T07:00:00Z",
  "scheduled_end_at": "2026-08-05T07:45:00Z",
  "timezone": "Africa/Cairo",
  "customer_confirmation_status": "NOT_REQUESTED",
  "scheduling_confirmation_status": "NOT_REQUIRED",
  "vehicle_availability_status": "AVAILABLE",
  "test_drive_readiness_status": "READY",
  "resource_hold_status": "CONFIRMED",
  "data_quality_status": "COMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T18:30:00Z"
}
```

### External Scheduling Response

When an external provider is authoritative, the initial response may be:

```json
{
  "appointment_id": "6efa31fd-27a3-4de9-8105-f6c920fa09c7",
  "status": "SCHEDULING",
  "scheduling_confirmation_status": "PENDING",
  "calendar_sync_status": "CREATE_PENDING",
  "command_id": "20ac7475-b9c7-4db6-a707-044206525167",
  "record_version": 1
}
```

The API must not return `SCHEDULED` as an externally confirmed outcome until authoritative provider Confirmation is received.

### Customer Confirmation Request

```json
{
  "channel": "WHATSAPP",
  "template_id": "appointment_confirmation_ar_v3",
  "appointment_record_version": 2
}
```

The request must pass:

- Customer contact-permission checks.
- Template checks.
- Channel checks.
- Human Approval or automation-policy validation.
- Frequency and time restrictions.

### Rescheduling Request

```json
{
  "requested_start_at": "2026-08-06T08:00:00Z",
  "requested_end_at": "2026-08-06T08:45:00Z",
  "timezone": "Africa/Cairo",
  "reason": "CUSTOMER_REQUEST",
  "expected_record_version": 4
}
```

A rescheduling request must use an idempotency key.

The response may remain pending until the replacement Appointment and external provider update are confirmed.

### Completion Request

```json
{
  "actual_start_at": "2026-08-05T07:04:00Z",
  "actual_end_at": "2026-08-05T07:43:00Z",
  "outcome": "TEST_DRIVE_COMPLETED",
  "outcome_summary": "Customer completed the scheduled test drive and requested a quotation review.",
  "follow_up_required": true,
  "next_action_type": "PREPARE_QUOTATION",
  "next_action_at": "2026-08-05T10:00:00Z",
  "expected_record_version": 7
}
```

The Appointment response must not claim that a Quotation was approved or a Deal was created.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Field-authority validation.
- Lifecycle validation.
- Scheduling conflict checks.
- Customer contact restrictions.
- Resource and Inventory checks.
- Freshness requirements.
- Human Approval or applicable automation policy.
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

- Appointments.
- Replacement Appointments.
- Calendar Events.
- Resource holds.
- Reminder messages.
- Attendance records.
- Follow-up Tasks.
- Cancellation Commands.

### Pending Confirmation

Operations requiring external authority may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "appointment_id": "6efa31fd-27a3-4de9-8105-f6c920fa09c7",
  "command_id": "20ac7475-b9c7-4db6-a707-044206525167",
  "record_version": 5
}
```

The API must not describe the operation as confirmed until authoritative evidence is received.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `CUSTOMER_RESTRICTED`
- `CONTACT_PERMISSION_RESTRICTED`
- `SCHEDULING_CONFLICT`
- `PARTICIPANT_UNAVAILABLE`
- `RESOURCE_UNAVAILABLE`
- `LOCATION_UNAVAILABLE`
- `VEHICLE_UNAVAILABLE`
- `INVENTORY_AVAILABILITY_STALE`
- `TEST_DRIVE_NOT_READY`
- `DRIVER_LICENSE_EVIDENCE_REQUIRED`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `CUSTOMER_RECONFIRMATION_REQUIRED`
- `ATTENDANCE_EVIDENCE_REQUIRED`
- `NO_SHOW_NOT_YET_ELIGIBLE`
- `APPOINTMENT_TERMINAL`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Scheduling validation.
- Resource checks.
- Customer contact restrictions.
- Concurrency.
- Idempotency.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Appointment Domain Service or deterministic policy controls.

---

## 11. Database Design

### Recommended Tables

```text
appointments
appointment_schedule_versions
appointment_participants
appointment_participant_history
appointment_locations
appointment_resource_holds
appointment_vehicle_context
appointment_customer_confirmations
appointment_reminders
appointment_attendance
appointment_outcomes
appointment_rescheduling
appointment_cancellations
appointment_no_show_decisions
appointment_external_calendar_mappings
appointment_external_confirmations
appointment_reconciliation_cases
appointment_derived_intelligence
appointment_data_quality_issues
appointment_status_history
appointment_record_versions
appointment_audit_log
```

### Appointments Table

The `appointments` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Customer and related Domain references.
- Current Appointment classification.
- Current lifecycle state.
- Current scheduling projection.
- Current Customer Confirmation projection.
- Current participant projection.
- Current location and channel projection.
- Current resource projection.
- Current Vehicle and test-drive projection.
- Current reminder projection.
- Current attendance projection.
- Current outcome projection.
- Current rescheduling and cancellation projection.
- External scheduling projection.
- Data-quality and conflict state.
- Record version.
- Audit timestamps.

Historical detail must remain in child or history tables.

### Primary Key

```text
PRIMARY KEY (appointment_id)
```

### Tenant Protection

Every Appointment-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_appointments_tenant_status
  (tenant_id, status)

idx_appointments_tenant_customer
  (tenant_id, customer_id, scheduled_start_at)

idx_appointments_tenant_opportunity
  (tenant_id, opportunity_id)

idx_appointments_tenant_responsible_user
  (tenant_id, responsible_user_id, scheduled_start_at)

idx_appointments_tenant_dealership_branch
  (tenant_id, dealership_id, branch_id, scheduled_start_at)

idx_appointments_tenant_location
  (tenant_id, dealership_location_id, scheduled_start_at)

idx_appointments_tenant_vehicle
  (tenant_id, inventory_record_id, scheduled_start_at)

idx_appointments_confirmation_pending
  (tenant_id, customer_confirmation_status, confirmation_deadline_at)

idx_appointments_upcoming
  (tenant_id, scheduled_start_at, status)

idx_appointments_calendar_sync
  (tenant_id, calendar_sync_status, reconciliation_status)

idx_appointments_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, appointment_number)
```

```text
UNIQUE (
  tenant_id,
  external_scheduling_provider,
  external_calendar_id,
  external_event_id
)
```

where external identifiers are populated.

Idempotency records should enforce:

```text
UNIQUE (tenant_id, operation_type, idempotency_key)
```

for protected retryable operations.

### Scheduling Conflict Controls

Database or Scheduling Service controls should prevent incompatible overlapping active Appointments for:

- Responsible User.
- Exclusive resource.
- Exclusive room.
- Inspection bay.
- Test-drive resource.
- Specific Inventory Record where exclusive use is required.

Conflict detection must consider active states such as:

```text
SCHEDULING
SCHEDULED
CONFIRMATION_PENDING
CONFIRMED
CHECKED_IN
IN_PROGRESS
```

The exact conflict policy must remain configurable.

### Schedule Version History

`appointment_schedule_versions` should preserve:

- Schedule-version identifier.
- `tenant_id`.
- `appointment_id`.
- Record version.
- Start.
- End.
- Timezone.
- Location.
- Channel.
- Vehicle and resource dependencies.
- Change reason.
- Actor.
- Authority.
- Customer reconfirmation requirement.
- External provider version.
- Timestamp.
- Related Event.

Schedule history must not be silently overwritten.

### Participant History

`appointment_participant_history` should preserve:

- Participant identifier.
- Appointment.
- User or external participant.
- Role.
- Confirmation state.
- Effective period.
- Assignment authority.
- Change reason.
- Related Event.

### Resource Holds

`appointment_resource_holds` should preserve:

- Resource-hold identifier.
- `tenant_id`.
- Appointment.
- Resource.
- Hold type.
- Status.
- Requested time.
- Confirmed time.
- Expiration.
- Release time.
- Command.
- Idempotency key.
- External Confirmation.
- Failure reason.
- Related Events.

Resource holds must not be stored as Inventory sales Reservations.

### Customer Confirmation History

`appointment_customer_confirmations` should preserve:

- Confirmation identifier.
- Appointment.
- Appointment record version.
- Customer or participant.
- Channel.
- Status.
- Requested time.
- Response time.
- Interaction reference.
- Evidence.
- Expiration.
- Source.
- Related Event.

A later schedule version must not silently rewrite an earlier Confirmation.

### Reminder History

`appointment_reminders` should preserve:

- Reminder identifier.
- Appointment.
- Appointment record version.
- Channel.
- Purpose.
- Template.
- Scheduled time.
- Approval or automation-policy reference.
- Command.
- Idempotency key.
- Provider message identifier.
- Sent status.
- Delivery status.
- Failure reason.
- Interaction reference.
- Related Event.

### Attendance History

`appointment_attendance` should preserve:

- Attendance identifier.
- Appointment.
- Check-in evidence.
- Actual start.
- Actual end.
- Check-out.
- Attendance status.
- Actor.
- Evidence.
- Correction reference.
- Related Event.

Attendance history must remain append-only or versioned.

### Rescheduling Records

`appointment_rescheduling` should preserve:

- Rescheduling identifier.
- Original Appointment.
- Replacement Appointment.
- Requested schedule.
- Reason.
- Requesting actor.
- Idempotency key.
- Resource handling.
- Customer Confirmation handling.
- External Command.
- External Confirmation.
- Final status.
- Related Events.

### Cancellation Records

`appointment_cancellations` should preserve:

- Cancellation identifier.
- Appointment.
- Requested by.
- Reason.
- Evidence.
- Request time.
- Command.
- Idempotency key.
- Provider Confirmation.
- Resource-release status.
- Customer-notification status.
- Final status.
- Related Events.

### External Calendar Mapping

`appointment_external_calendar_mappings` should preserve:

- Provider.
- External calendar.
- External Event identifier.
- External version.
- Appointment record version.
- Last accepted source update.
- Last Command.
- Confirmation status.
- Conflict status.
- Reconciliation status.
- Related Events.

### Derived Intelligence

Derived Appointment records must remain separate from authoritative scheduling and attendance fields.

Each derived record should preserve:

- Output type.
- Output value.
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

Appointment audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw sensitive values where full retention is unnecessary.

### Partitioning

High-volume deployments may partition by:

- `tenant_id`.
- Region.
- Scheduled date.
- Dealership.
- Retention class.
- Audit time.

Partitioning must not weaken:

- Tenant isolation.
- Conflict detection.
- Resource availability.
- Referential integrity.
- History.
- Audit integrity.

### Hard Deletion

An Appointment must not be hard-deleted when referenced by:

- Customer journey.
- Lead.
- Qualified Lead.
- Opportunity.
- Vehicle or Inventory workflow.
- Trade-In.
- Finance Application.
- Quotation.
- Financial Contract.
- Deal.
- Interaction.
- Task.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Audit evidence.

Cancellation, expiration, archival, anonymization, or governed redaction must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER_REFERENCE` | Customer and participant references |
| `SCHEDULING_INFORMATION` | Date, time, timezone, duration |
| `SENSITIVE_LOCATION` | Customer location, secure dealership area |
| `SECURE_ACCESS_INFORMATION` | Meeting links, access codes, arrival instructions |
| `VEHICLE_OPERATIONAL` | Inventory Record, readiness, test-drive resource |
| `IDENTITY_EVIDENCE` | Driving-licence verification reference |
| `COMMUNICATION_PERMISSION` | Contact basis and restriction projection |
| `CUSTOMER_RESTRICTED` | Assistance notes, outcome notes, feedback |
| `DERIVED_INTELLIGENCE` | No-show risk and Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, attendance history |

### Authentication

Every internal Appointment operation requires an authenticated Human or service identity.

Customer self-service Appointment actions may use an approved secure Customer authentication or verification mechanism.

Anonymous management of existing Appointments is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Department.
- Team.
- Responsible User.
- Participant.
- Appointment type.
- Location.
- Resource.
- Vehicle and Inventory dependency.
- Requested field.
- Requested action.
- Appointment state.
- Data classification.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access and manage permitted:

- Assigned Appointments.
- Customer consultations.
- Showroom visits.
- Test-drive coordination.
- Appointment outcomes.
- Follow-up Tasks.

Must not independently:

- Override Customer contact restrictions.
- Verify legal driving eligibility without evidence.
- Remove safety blocks.
- Confirm Vehicle delivery.
- Confirm contract signature.
- Override restricted resource conflicts.
- Reopen terminal Appointments.

#### BDC User

May access permitted:

- Scheduling.
- Customer Confirmation.
- Reminder coordination.
- Rescheduling.
- Assigned Appointment queues.

Access to restricted Deal, finance, identity, and delivery evidence may be limited.

#### Sales Manager

May access Appointments inside authorized organizational scope.

May perform configured:

- Reassignment.
- Conflict override.
- High-priority scheduling approval.
- Off-site Appointment approval.
- Terminal correction review.

Manager access does not automatically authorize:

- Finance approval.
- Legal override.
- Driving-licence verification.
- Payment Confirmation.
- Contract signature.
- Vehicle delivery Confirmation.
- Cross-Tenant access.

#### Inventory User

May access Vehicle and resource context needed for:

- Test-drive preparation.
- Vehicle readiness.
- Resource holding.
- Location coordination.

Inventory User must not modify Customer Consent or Appointment outcome outside authority.

#### Trade-In Specialist

May access Appointments required for approved Trade-In inspections.

#### Finance Specialist

May access finance-consultation Appointments required for assigned Finance Applications.

#### Delivery Coordinator

May access delivery-handoff scheduling and readiness projections.

Delivery Coordinator access does not by itself prove or authorize final delivery Confirmation.

#### Compliance or Legal Reviewer

May access restricted evidence required for an assigned review.

#### Data Steward

May review:

- Scheduling conflicts.
- External mapping conflicts.
- Duplicate Appointments.
- Relationship inconsistencies.
- Reconciliation cases.
- Data-quality issues.

#### AI Agent

May access only the minimum Appointment context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unauthorized access to meeting links, exact locations, identity evidence, Consent evidence, finance data, and contract information.

### Secure Meeting Access

Meeting URLs, access tokens, PINs, and dial-in credentials must:

- Use approved providers.
- Be stored securely.
- Be displayed only to authorized participants.
- Be excluded from ordinary Logs.
- Be excluded from unrestricted analytics.
- Expire or be rotated where supported.
- Not be embedded in public calendar descriptions.

### Location Protection

Exact secure locations, parking slots, restricted areas, access instructions, and Customer home addresses must be limited to authorized roles.

Customer-facing descriptions must use approved public location information.

### Driving-Licence and Insurance Evidence

Driving-licence and insurance evidence must:

- Use controlled storage.
- Be purpose-limited.
- Be access-restricted.
- Preserve verification status and provenance.
- Avoid unnecessary duplication.
- Follow applicable retention requirements.
- Be excluded from general-purpose AI context.

### Customer Communication Security

Before outbound communication, deterministic controls must validate:

- Customer permission.
- Purpose.
- Channel.
- Template.
- Appointment status.
- Schedule freshness.
- Frequency.
- Quiet hours.
- Customer restrictions.
- Human Approval or approved automation policy.

Prompt text, User interface state, Appointment priority, or AI Recommendation must not bypass these controls.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Scheduling conflict checks.
- Search.
- Vector retrieval.
- Events.
- Queues.
- Caches.
- Calendar synchronization.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound Appointment Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Requested action.
- Current record version.
- Field-level write authority.
- Human Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Appointment activity must record:

- `tenant_id`.
- `appointment_id`.
- Customer reference.
- Related Opportunity or Deal where applicable.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Record version.
- Source.
- Authority category.
- Applied policy.
- AI involvement.
- Recommendation.
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

- Cross-Tenant Appointment access attempts.
- Unauthorized schedule changes.
- Unauthorized Customer contact.
- Meeting-link exposure.
- Restricted-location access.
- Resource-hold manipulation.
- Vehicle-readiness override attempts.
- Driving-licence evidence access.
- False attendance recording.
- False no-show recording.
- False completion recording.
- Duplicate reminder execution.
- Calendar Command replay.
- External Confirmation mismatch.
- AI access outside approved scope.
- Audit-log tampering.
- Suspicious bulk Appointment export.

### Scheduling Integrity

The platform must detect or prevent:

- Unauthorized double-booking.
- Duplicate Appointments from retries.
- Resource over-allocation.
- Appointment status manipulation.
- False Customer Confirmation.
- False attendance.
- False no-show.
- False completion.
- Appointment outcome claiming unrelated business completion.
- External provider conflict being silently ignored.
- Rescheduling chains that lose historical evidence.

### Privacy and Retention

Appointment retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Calendar-provider agreements.
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
- External providers where lawfully required.
- Backups according to policy.

Required non-personal scheduling, commercial, security, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Customer reminders.
- Appointment Confirmation messages.
- External calendar write-back.
- Automated scheduling.
- Test-drive scheduling.
- Off-site scheduling.
- Resource holds.
- AI Appointment Recommendations.
- Appointment export.
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
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Appointment baseline.

Appointment represents scheduling, attendance, and meeting outcome.

Appointment does not independently prove Vehicle Reservation, Vehicle Allocation, finance approval, contract signature, Payment, sale, or delivery.

Resource holds remain separate from commercial Inventory Reservations.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
