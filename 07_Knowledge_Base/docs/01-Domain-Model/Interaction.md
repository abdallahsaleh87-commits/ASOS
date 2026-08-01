# Interaction

## 1. Object Purpose

### Business Purpose

The Interaction object represents a single recorded communication, engagement, or meaningful contact between the dealership and a Customer, prospect, participant, external party, User, or authorized AI Agent.

It provides the dealership with a complete chronological history of activities such as:

- Incoming and outgoing phone calls.
- WhatsApp and SMS messages.
- Emails.
- Website and marketplace conversations.
- Social-media messages.
- Showroom conversations.
- Video consultations.
- Appointment discussions.
- Test-drive feedback.
- Quotation presentations.
- Finance and document discussions.
- Deal and delivery updates.
- Customer complaints and objections.
- AI-assisted or automated responses.
- Internal notes that materially affect the Customer Journey.

The Interaction object allows the dealership to:

- Preserve communication history.
- Understand Customer intent and sentiment.
- Track promises and commitments.
- Identify unanswered messages.
- Enforce consent and communication policies.
- Maintain continuity when ownership changes.
- Trigger follow-up Tasks and escalation.
- Support Lead, Opportunity, Deal, and Customer Journey analytics.
- Provide governed context to authorized AI Agents.

An Interaction is not a Customer, Lead, Opportunity, Appointment, Quotation, Finance Application, or Deal. It records the communication or activity that may create, update, support, or close those objects.

### System Purpose

The Interaction object is the canonical omnichannel communication and engagement record within the ASOS domain.

It connects:

- Customer
- Lead
- Qualified Lead
- Opportunity
- Appointment
- Quotation
- Trade-In
- Finance Application
- Deal
- Vehicle
- User
- AI Agent
- Campaign
- Communication Provider
- Conversation Thread
- Task
- Customer Journey

The Interaction provides the authoritative record used by:

- Omnichannel inboxes.
- Contact-center workflows.
- Sales Consultant work queues.
- Conversation threading.
- Customer sentiment and intent analysis.
- Follow-up generation.
- SLA monitoring.
- Communication consent enforcement.
- AI conversation memory.
- Customer Journey timelines.
- Compliance and dispute investigation.
- Engagement and conversion analytics.

Every Interaction must be:

- Tenant-scoped.
- Time-stamped.
- Traceable to its source.
- Associated with known participants when possible.
- Protected according to its data classification.
- Immutable after final completion except through controlled correction or redaction workflows.

Inbound provider events and outbound delivery events must be idempotent and deduplicated using trusted provider identifiers.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `interaction_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — optional until identity resolution)
  - `lead_id` (UUIDv4 — optional)
  - `qualified_lead_id` (UUIDv4 — optional)
  - `opportunity_id` (UUIDv4 — optional)
  - `appointment_id` (UUIDv4 — optional)
  - `quotation_id` (UUIDv4 — optional)
  - `trade_in_id` (UUIDv4 — optional)
  - `finance_application_id` (UUIDv4 — optional)
  - `deal_id` (UUIDv4 — optional)
  - `vehicle_id` (UUIDv4 — optional)
  - `owner_id` (UUIDv4 — optional)
  - `assigned_user_id` (UUIDv4 — optional)
  - `agent_id` (UUIDv4 — optional)
  - `campaign_id` (UUIDv4 — optional)
  - `conversation_thread_id` (UUIDv4 — optional)
  - `parent_interaction_id` (UUIDv4 — optional)
  - `task_id` (UUIDv4 — optional)
  - `supersedes_interaction_id` (UUIDv4 — optional)

### Interaction Classification

- `interaction_type`
- `direction`
- `channel`
- `status`
- `purpose`
- `priority`
- `source`
- `visibility`
- `communication_category`
- `customer_journey_stage`

### Participant Fields

- `sender_type`
- `sender_id`
- `sender_display_name`
- `sender_address`
- `recipient_type`
- `recipient_ids`
- `recipient_addresses`
- `participant_snapshot`
- `external_participants`
- `authenticated_customer`
- `identity_resolution_status`
- `identity_resolution_confidence`

### Conversation and Threading

- `conversation_thread_id`
- `parent_interaction_id`
- `reply_to_interaction_id`
- `root_interaction_id`
- `sequence_number`
- `subject`
- `conversation_topic`
- `thread_status`
- `external_thread_id`

### Content Fields

- `content_text`
- `content_html`
- `content_summary`
- `customer_visible_content`
- `internal_notes`
- `language_code`
- `translated_content`
- `translation_status`
- `content_hash`
- `content_redacted`
- `redaction_reason`

### Media and Attachments

- `attachment_count`
- `attachment_ids`
- `media_type`
- `recording_url`
- `recording_duration_seconds`
- `transcript_status`
- `transcript_text`
- `transcript_confidence`
- `document_references`
- `media_snapshot`

### Timing Fields

- `occurred_at`
- `received_at`
- `queued_at`
- `sent_at`
- `delivered_at`
- `read_at`
- `started_at`
- `ended_at`
- `completed_at`
- `failed_at`
- `archived_at`

### Communication Provider Fields

- `provider_name`
- `provider_account_id`
- `provider_message_id`
- `provider_conversation_id`
- `provider_event_id`
- `provider_status`
- `provider_error_code`
- `provider_error_message`
- `provider_metadata`
- `provider_received_at`

### Consent and Communication Policy

- `consent_status`
- `consent_type`
- `consent_reference`
- `contact_permission_checked`
- `contact_permission_checked_at`
- `quiet_hours_checked`
- `frequency_limit_checked`
- `communication_policy_version`
- `lawful_basis`
- `opt_out_detected`
- `opt_out_processed_at`

### AI and Intelligence Fields

- `intent`
- `intent_confidence`
- `sentiment`
- `sentiment_score`
- `urgency`
- `urgency_score`
- `topic_labels`
- `entity_extraction`
- `objection_type`
- `commitment_detected`
- `commitment_details`
- `next_best_action`
- `ai_summary`
- `ai_processing_status`
- `ai_model_reference`
- `ai_prompt_version`
- `human_review_required`
- `human_review_reason`

### Outcome and Follow-Up

- `outcome`
- `outcome_details`
- `response_required`
- `response_due_at`
- `responded_at`
- `response_sla_status`
- `follow_up_required`
- `next_action_type`
- `next_action_at`
- `task_id`
- `escalation_required`
- `escalation_reason`
- `escalated_at`

### Quality and Compliance

- `recording_consent_status`
- `compliance_review_status`
- `content_moderation_status`
- `restricted_data_detected`
- `sensitive_data_types`
- `quality_score`
- `quality_review_status`
- `dispute_reference`
- `legal_hold_status`

### Computed Fields

- `response_time_seconds`
- `conversation_duration_seconds`
- `time_to_first_response_seconds`
- `is_unanswered`
- `is_overdue`
- `customer_engagement_score`
- `interaction_recency_days`
- `sentiment_change`
- `thread_interaction_count`
- `attachment_processing_complete`
- `sla_breached`
- `requires_follow_up`
- `is_customer_visible`

### Governance and Lifecycle

- **Participant Snapshot:** `participant_snapshot` (Encrypted JSONB)
- **Content Snapshot:** `content_snapshot` (Encrypted JSONB)
- **Provider Snapshot:** `provider_snapshot` (JSONB)
- **AI Analysis Snapshot:** `ai_analysis_snapshot` (JSONB)
- **Consent Snapshot:** `consent_snapshot` (Encrypted JSONB)
- **Compliance Snapshot:** `compliance_snapshot` (Encrypted JSONB)
- **Outcome Snapshot:** `outcome_snapshot` (JSONB)

- **Audit Fields:**
  - `created_by`
  - `updated_by`
  - `sent_by`
  - `completed_by`
  - `reviewed_by`
  - `redacted_by`
  - `archived_by`
  - `last_processed_by_agent`

- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)

- **Timestamps:**
  - `created_at`
  - `updated_at`
  - `occurred_at`
  - `received_at`
  - `queued_at`
  - `sent_at`
  - `delivered_at`
  - `read_at`
  - `started_at`
  - `ended_at`
  - `completed_at`
  - `failed_at`
  - `responded_at`
  - `escalated_at`
  - `redacted_at`
  - `archived_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| interaction_id | UUID | Unique canonical identifier for the Interaction. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Dealership tenant that owns the Interaction. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Customer associated with the Interaction. | Conditional | Null | Required after successful Customer identity resolution | 777e8888-e99b-11d2-a333-426614174000 | System-controlled |
| lead_id | UUID | Lead associated with the Interaction. | No | Null | Must belong to the same Customer and dealership when populated | 111e2222-e33b-44d5-a666-426614174000 | System-controlled |
| opportunity_id | UUID | Opportunity supported by the Interaction. | No | Null | Must belong to the same Customer and dealership | 333e4444-e55b-66d7-a888-426614174000 | System-controlled |
| deal_id | UUID | Deal associated with the Interaction. | No | Null | Must belong to the same Customer and dealership | 999e0000-e11b-22d3-a444-426614174000 | System-controlled |
| conversation_thread_id | UUID | Canonical conversation thread containing the Interaction. | No | Null | Must belong to the same dealership and participants | 222e3333-e44b-55d6-a777-426614174000 | System-controlled |
| interaction_type | Enum | Specific form of communication or engagement. | Yes | MESSAGE | Must match InteractionType ENUM | PHONE_CALL | At least 0.95 |
| direction | Enum | Indicates whether the Interaction is inbound, outbound, internal, or system-generated. | Yes | INBOUND | Must match InteractionDirection ENUM | OUTBOUND | At least 0.99 |
| channel | Enum | Communication channel used. | Yes | OTHER | Must match InteractionChannel ENUM | WHATSAPP | At least 0.99 |
| status | Enum | Current lifecycle state of the Interaction. | Yes | CREATED | Must match InteractionStatus ENUM | DELIVERED | At least 0.99 |
| purpose | Enum | Primary business purpose of the Interaction. | Yes | GENERAL_INQUIRY | Must match InteractionPurpose ENUM | QUOTATION_FOLLOW_UP | At least 0.90 |
| priority | Enum | Operational urgency. | Yes | STANDARD | Must match InteractionPriority ENUM | HIGH | At least 0.90 |
| source | Enum | Originating system or operational source. | Yes | MANUAL | Must match InteractionSource ENUM | WHATSAPP_PROVIDER | At least 0.95 |
| sender_type | Enum | Type of participant that initiated the Interaction. | Yes | CUSTOMER | Must match InteractionParticipantType ENUM | AI_AGENT | At least 0.99 |
| sender_id | UUID | Internal identifier of the sender when resolved. | No | Null | Must reference an entity compatible with sender_type | 321e6547-e89b-12d3-a456-426614174000 | System-controlled |
| sender_address | String | Normalized sender address such as phone, email, or provider identifier. | Conditional | Null | Required for supported external communication channels | +201234567890 | Verified provider |
| recipient_ids | UUID Array | Internal recipients associated with the Interaction. | No | Empty array | Every identifier must belong to the same tenant | [321e6547-e89b-12d3-a456-426614174000] | System-controlled |
| subject | String | Short subject or conversation title. | No | Null | Maximum 500 characters | Test-drive confirmation | At least 0.90 |
| content_text | Text | Normalized plain-text content. | Conditional | Null | Required for text-based Interactions unless media-only | I can visit tomorrow at 10 AM. | Provider or human |
| content_summary | Text | Short governed summary of the Interaction. | No | Null | Must not introduce unsupported facts | Customer requested a test drive tomorrow morning. | AI or human |
| language_code | String | Detected or selected language. | Yes | Dealership default | Must use a supported BCP 47 language tag | ar-EG | At least 0.95 |
| occurred_at | Timestamp | Business time when the Interaction occurred. | Yes | Current server time | Must be valid and cannot be unreasonably later than ingestion time | 2026-08-01T14:00:00+03:00 | Provider or system |
| provider_message_id | String | External provider message identifier. | No | Null | Must be unique within the provider account when populated | wamid.HBgM... | Trusted provider |
| content_hash | String | Cryptographic hash of normalized Interaction content. | Conditional | Generated | Required after content normalization | sha256:7bc1... | System-generated |
| consent_status | Enum | Communication-consent status applied to the Interaction. | Yes | UNKNOWN | Must match InteractionConsentStatus ENUM | PERMITTED | System-controlled |
| contact_permission_checked | Boolean | Indicates whether permission was checked before outbound communication. | Yes | false | Must be true before regulated outbound delivery | true | System-controlled |
| intent | Enum | Detected Customer or business intent. | No | UNKNOWN | Must match InteractionIntent ENUM | SCHEDULE_TEST_DRIVE | AI or human |
| intent_confidence | Decimal | Confidence in intent classification. | No | 0.00 | Must remain between 0.00 and 1.00 | 0.94 | System-computed |
| sentiment | Enum | Detected emotional polarity. | No | UNKNOWN | Must match InteractionSentiment ENUM | POSITIVE | AI or human |
| sentiment_score | Decimal | Normalized sentiment score. | No | 0.00 | Must remain between -1.00 and 1.00 | 0.72 | System-computed |
| urgency | Enum | Detected urgency classification. | Yes | NORMAL | Must match InteractionUrgency ENUM | HIGH | AI or human |
| response_required | Boolean | Indicates whether a response is required. | Yes | false | Must be true when business policy requires continuation | true | System or human |
| response_due_at | Timestamp | Deadline for the required response. | No | Null | Required when response_required is true | 2026-08-01T14:15:00+03:00 | System-calculated |
| outcome | Enum | Result of the Interaction. | No | Null | Required when status becomes COMPLETED | APPOINTMENT_REQUESTED | Authorized human or trusted workflow |
| follow_up_required | Boolean | Indicates whether a follow-up action is required. | Yes | false | Must be consistent with outcome and policy | true | System or human |
| next_action_at | Timestamp | Due time for the follow-up action. | No | Null | Required when follow_up_required is true | 2026-08-01T15:00:00+03:00 | System or human |
| recording_consent_status | Enum | Consent state for call or meeting recording. | Yes | NOT_APPLICABLE | Must match RecordingConsentStatus ENUM | GRANTED | Authoritative evidence |
| ai_processing_status | Enum | Current AI-processing state. | Yes | NOT_REQUESTED | Must match AIProcessingStatus ENUM | COMPLETED | System-controlled |
| human_review_required | Boolean | Indicates whether AI or compliance findings require human review. | Yes | false | Must become true for configured high-risk conditions | true | System-controlled |
| record_version | Integer | Optimistic-concurrency version. | Yes | 1 | Must increase after every successful update | 4 | System-controlled |

## 4. Enumerations

### InteractionStatus

- **CREATED:** The Interaction record exists but processing has not started.
- **RECEIVED:** An inbound Interaction was received from a trusted source.
- **DRAFT:** An outbound Interaction is being prepared.
- **QUEUED:** The outbound Interaction is waiting for provider delivery.
- **SENT:** The provider accepted the outbound Interaction.
- **DELIVERED:** Delivery was confirmed by the provider.
- **READ:** The recipient opened or acknowledged the Interaction when supported.
- **IN_PROGRESS:** A synchronous call, meeting, or conversation is active.
- **COMPLETED:** The Interaction finished and its outcome was recorded.
- **FAILED:** Processing or delivery failed.
- **CANCELLED:** An outbound Interaction was cancelled before delivery.
- **ARCHIVED:** The completed Interaction was moved to historical storage.

### InteractionType

- MESSAGE
- PHONE_CALL
- EMAIL
- VIDEO_CALL
- IN_PERSON_CONVERSATION
- VOICE_NOTE
- SOCIAL_MESSAGE
- WEB_CHAT
- SYSTEM_NOTIFICATION
- INTERNAL_NOTE
- DOCUMENT_EXCHANGE
- CUSTOMER_FEEDBACK
- COMPLAINT
- OTHER

### InteractionDirection

- INBOUND
- OUTBOUND
- INTERNAL
- SYSTEM_GENERATED

### InteractionChannel

- PHONE
- SMS
- WHATSAPP
- EMAIL
- WEB_CHAT
- WEBSITE_FORM
- MOBILE_APP
- VIDEO
- IN_PERSON
- FACEBOOK
- INSTAGRAM
- MARKETPLACE
- OEM_PLATFORM
- INTERNAL_SYSTEM
- OTHER

### InteractionPurpose

- GENERAL_INQUIRY
- LEAD_FOLLOW_UP
- REQUIREMENT_DISCOVERY
- VEHICLE_INFORMATION
- VEHICLE_AVAILABILITY
- APPOINTMENT_SCHEDULING
- APPOINTMENT_CONFIRMATION
- TEST_DRIVE_FOLLOW_UP
- TRADE_IN_DISCUSSION
- QUOTATION_PRESENTATION
- QUOTATION_FOLLOW_UP
- NEGOTIATION
- FINANCE_DISCUSSION
- DOCUMENT_COLLECTION
- CONTRACT_DISCUSSION
- PAYMENT_FOLLOW_UP
- DELIVERY_COORDINATION
- POST_SALE_FOLLOW_UP
- COMPLAINT_HANDLING
- CUSTOMER_SUPPORT
- OPT_OUT_REQUEST
- INTERNAL_COORDINATION
- OTHER

### InteractionPriority

- LOW
- STANDARD
- HIGH
- URGENT
- CRITICAL
- VIP

### InteractionSource

- MANUAL
- CRM
- WEBSITE
- MOBILE_APP
- PHONE_PROVIDER
- WHATSAPP_PROVIDER
- EMAIL_PROVIDER
- SOCIAL_PLATFORM
- MARKETPLACE
- OEM_PLATFORM
- CALENDAR
- AI_AGENT
- AUTOMATION
- IMPORT
- API_INTEGRATION
- OTHER

### InteractionParticipantType

- CUSTOMER
- PROSPECT
- USER
- AI_AGENT
- SYSTEM
- LENDER
- VENDOR
- OEM
- PARTNER
- UNKNOWN

### InteractionVisibility

- CUSTOMER_VISIBLE
- INTERNAL
- RESTRICTED
- COMPLIANCE_ONLY
- MANAGEMENT_ONLY

### InteractionConsentStatus

- UNKNOWN
- NOT_REQUIRED
- PERMITTED
- RESTRICTED
- OPTED_OUT
- EXPIRED
- BLOCKED

### InteractionIntent

- UNKNOWN
- ASK_VEHICLE_PRICE
- ASK_VEHICLE_AVAILABILITY
- REQUEST_QUOTATION
- SCHEDULE_APPOINTMENT
- SCHEDULE_TEST_DRIVE
- CHANGE_APPOINTMENT
- CANCEL_APPOINTMENT
- DISCUSS_TRADE_IN
- APPLY_FOR_FINANCE
- PROVIDE_DOCUMENTS
- NEGOTIATE_PRICE
- ACCEPT_OFFER
- REJECT_OFFER
- REQUEST_CALLBACK
- REQUEST_DELIVERY_UPDATE
- REPORT_COMPLAINT
- OPT_OUT
- OTHER

### InteractionSentiment

- UNKNOWN
- VERY_NEGATIVE
- NEGATIVE
- NEUTRAL
- POSITIVE
- VERY_POSITIVE
- MIXED

### InteractionUrgency

- LOW
- NORMAL
- HIGH
- URGENT
- CRITICAL

### InteractionOutcome

- INFORMATION_PROVIDED
- CUSTOMER_RESPONDED
- CUSTOMER_UNREACHABLE
- CALLBACK_REQUESTED
- APPOINTMENT_REQUESTED
- APPOINTMENT_CONFIRMED
- APPOINTMENT_CANCELLED
- REQUIREMENTS_UPDATED
- VEHICLE_SELECTED
- QUOTATION_REQUESTED
- QUOTATION_PRESENTED
- OFFER_ACCEPTED
- OFFER_REJECTED
- FINANCE_APPLICATION_STARTED
- DOCUMENTS_RECEIVED
- DEAL_PROGRESS_UPDATED
- DELIVERY_UPDATE_PROVIDED
- COMPLAINT_OPENED
- COMPLAINT_RESOLVED
- OPT_OUT_PROCESSED
- FOLLOW_UP_REQUIRED
- NO_ACTION_REQUIRED
- ESCALATED
- OTHER

### RecordingConsentStatus

- NOT_APPLICABLE
- NOT_REQUESTED
- PENDING
- GRANTED
- DECLINED
- WITHDRAWN
- EXPIRED

### AIProcessingStatus

- NOT_REQUESTED
- QUEUED
- PROCESSING
- COMPLETED
- FAILED
- BLOCKED
- HUMAN_REVIEW_REQUIRED

### IdentityResolutionStatus

- NOT_REQUIRED
- UNRESOLVED
- PARTIALLY_RESOLVED
- RESOLVED
- CONFLICT
- MANUAL_REVIEW_REQUIRED

### ResponseSLAStatus

- NOT_APPLICABLE
- PENDING
- WITHIN_SLA
- DUE_SOON
- BREACHED
- WAIVED
- COMPLETED

### ComplianceReviewStatus

- NOT_REQUIRED
- PENDING
- CLEARED
- RESTRICTED
- BLOCKED
- MANUAL_REVIEW_REQUIRED

## 5. Validation Rules

### Business Rules

- Every Interaction must belong to exactly one dealership tenant.
- Every inbound Interaction must preserve its trusted source and provider reference when available.
- Every outbound Interaction must have an authorized sender.
- Customer identity may remain unresolved temporarily for inbound messages, but identity resolution must occur before sensitive information is disclosed.
- A Customer-facing outbound Interaction must pass consent, channel, quiet-hours, and frequency-limit checks.
- An opted-out Customer must not receive unauthorized promotional communication.
- Transactional communication may proceed only when permitted by law, policy, and documented purpose.
- A phone or video recording requires valid recording consent when legally required.
- Internal notes must never be delivered to the Customer.
- Restricted financial, identity, compliance, or profitability data must not appear in unrestricted Customer-facing content.
- A completed Interaction must preserve its outcome when an operational result occurred.
- An unanswered Customer Interaction must create a follow-up requirement according to SLA policy.
- Customer promises, dealership commitments, complaints, opt-outs, and acceptance signals must be explicitly recorded.
- AI-generated outbound content must be reviewed or policy-approved before delivery.
- An AI Agent must identify itself when required by policy.
- An Interaction linked to a Deal, Finance Application, Complaint, or legal dispute may be subject to enhanced retention or legal hold.
- Provider delivery confirmation does not prove Customer understanding or acceptance.
- Customer acceptance of an offer must reference the applicable Quotation, finance offer, contract, or Deal evidence rather than relying on free text alone.

### Technical Rules

- Provider events must be authenticated and deduplicated.
- `provider_message_id` or `provider_event_id` must be used as an idempotency key when available.
- Outbound send operations must require an idempotency key.
- Normalized content must produce a cryptographic `content_hash`.
- `record_version` must increase after every permitted update.
- Completed Customer-visible content must be immutable.
- Corrections must create a versioned replacement or governed redaction record.
- Message ordering within a thread must preserve provider sequence and event timestamps.
- Attachments must be scanned, classified, and stored in the approved Document Vault.
- Public media URLs must not be stored.
- Transcripts must preserve source recording, provider, model version, and confidence.
- AI outputs must preserve model reference, prompt version, structured input scope, and confidence.
- Failed AI processing must not block canonical Interaction ingestion.
- Failed provider delivery must create retry or Human Review logic according to policy.
- SLA calculations must use authoritative server time.

### Data Constraints

- `occurred_at` is required for every Interaction.
- `scheduled_end_at` or equivalent does not apply unless the Interaction is synchronous.
- `ended_at` must be later than `started_at`.
- `completed_at` cannot precede `occurred_at`.
- `delivered_at` cannot precede `sent_at`.
- `read_at` cannot precede `delivered_at`.
- `responded_at` cannot precede the related inbound Interaction.
- `intent_confidence` must remain between `0.00` and `1.00`.
- `identity_resolution_confidence` must remain between `0.00` and `1.00`.
- `transcript_confidence` must remain between `0.00` and `1.00`.
- `sentiment_score` must remain between `-1.00` and `1.00`.
- `urgency_score` must remain between `0.00` and `1.00`.
- `quality_score` must remain between `0.00` and `100.00`.
- `response_due_at` is required when `response_required = true`.
- `next_action_at` is required when `follow_up_required = true`.
- `escalation_reason` is required when `escalation_required = true`.
- `redaction_reason` is required when `content_redacted = true`.
- `recording_consent_status = GRANTED` is required before recording when consent is mandatory.
- `content_text`, media, attachment, recording, or structured system content must exist.
- `provider_error_code` or failure details must exist when status becomes `FAILED`.

### Referential Integrity

- All linked records must belong to the same `dealership_id`.
- `lead_id`, `qualified_lead_id`, `opportunity_id`, and `deal_id` must belong to the linked Customer when `customer_id` is known.
- `appointment_id`, `quotation_id`, `trade_in_id`, and `finance_application_id` must match the same Customer Journey context.
- `vehicle_id` must belong to the referenced Opportunity, Quotation, Appointment, or Deal context when applicable.
- `conversation_thread_id` must belong to the same dealership.
- `parent_interaction_id`, `reply_to_interaction_id`, and `root_interaction_id` must belong to the same conversation thread.
- An Interaction cannot reply to itself.
- Circular reply or supersession chains are prohibited.
- `task_id` must reference a Task created from or linked to this Interaction.
- `owner_id`, `assigned_user_id`, and `agent_id` must reference active authorized identities.
- An Interaction cannot be hard-deleted while referenced by a Customer Journey, complaint, Deal, finance record, Task, audit entry, or legal hold.

### Human Approval Requirements

- Outbound messages containing pricing commitments, discounts, finance terms, trade-in values, legal terms, or delivery commitments require the appropriate approved source.
- AI Agents cannot make binding pricing, finance, trade-in, contract, payment, reservation, or delivery commitments.
- AI Agents cannot process an opt-out without preserving the Customer evidence and updating the authoritative consent record.
- AI Agents cannot disclose sensitive Customer, Deal, finance, or compliance data to unresolved identities.
- AI Agents cannot mark complaints resolved without authorized confirmation.
- High-risk sentiment, threats, fraud indicators, legal claims, discrimination allegations, or regulatory complaints require human review.
- Conflicting identity, consent, acceptance, payment, or commitment evidence must create a Human Review Task.
- Redaction or deletion of completed Customer communications requires authorized compliance or privacy approval.
- Manual changes to provider delivery or read status require documented correction evidence.

## 6. State Machine

### Allowed States

- CREATED
- RECEIVED
- DRAFT
- QUEUED
- SENT
- DELIVERED
- READ
- IN_PROGRESS
- COMPLETED
- FAILED
- CANCELLED
- ARCHIVED

### Allowed Transitions

#### Inbound and Imported Interactions

- CREATED → RECEIVED
- RECEIVED → IN_PROGRESS
- RECEIVED → COMPLETED
- RECEIVED → FAILED
- IN_PROGRESS → COMPLETED
- IN_PROGRESS → FAILED
- COMPLETED → ARCHIVED

#### Outbound Interactions

- CREATED → DRAFT
- DRAFT → QUEUED
- DRAFT → CANCELLED
- QUEUED → SENT
- QUEUED → FAILED
- QUEUED → CANCELLED
- SENT → DELIVERED
- SENT → READ
- SENT → COMPLETED
- SENT → FAILED
- DELIVERED → READ
- DELIVERED → COMPLETED
- READ → COMPLETED
- FAILED → QUEUED
- FAILED → CANCELLED
- COMPLETED → ARCHIVED

#### Synchronous Interactions

- CREATED → IN_PROGRESS
- RECEIVED → IN_PROGRESS
- IN_PROGRESS → COMPLETED
- IN_PROGRESS → FAILED
- IN_PROGRESS → CANCELLED

#### Internal and System Interactions

- CREATED → COMPLETED
- CREATED → FAILED
- COMPLETED → ARCHIVED

### Forbidden Transitions

- RECEIVED → DRAFT
- RECEIVED → QUEUED
- DRAFT → RECEIVED
- DRAFT → DELIVERED
- DRAFT → READ
- QUEUED → RECEIVED
- QUEUED → DELIVERED
- DELIVERED → SENT
- READ → DELIVERED
- COMPLETED → IN_PROGRESS
- COMPLETED → SENT
- COMPLETED → DELIVERED
- CANCELLED → SENT
- CANCELLED → DELIVERED
- ARCHIVED → IN_PROGRESS
- ARCHIVED → SENT
- ARCHIVED → COMPLETED

### Entry Conditions

- To enter `RECEIVED`:
  - The inbound provider or source must be identified.
  - The Interaction payload must pass authentication and ingestion validation.
  - Duplicate-provider events must be rejected or linked to the existing Interaction.
  - `received_at` and `occurred_at` must be populated.

- To enter `DRAFT`:
  - An authorized User, AI Agent, or automation must be identified.
  - Intended recipients and communication purpose must be known.
  - Customer-visible content must not yet be delivered.

- To enter `QUEUED`:
  - Customer identity and recipient address must be sufficiently resolved.
  - Contact permission, quiet-hours, and frequency-limit checks must pass.
  - Content moderation and restricted-data checks must pass.
  - Required human approval must be complete.
  - A valid provider route must exist.
  - `queued_at` must be populated.

- To enter `SENT`:
  - The communication provider must accept the message or call request.
  - `provider_message_id` or equivalent provider reference must exist.
  - `sent_at` must be populated.

- To enter `DELIVERED`:
  - A trusted provider delivery confirmation must exist.
  - `delivered_at` must be populated.

- To enter `READ`:
  - A trusted provider or Customer acknowledgement event must exist.
  - `read_at` must be populated.

- To enter `IN_PROGRESS`:
  - The synchronous call, meeting, or active conversation must have started.
  - Required recording consent must be validated.
  - `started_at` must be populated.

- To enter `COMPLETED`:
  - The Interaction must have ended or required processing must be complete.
  - The final content or transcript must be preserved.
  - Required outcome, follow-up, escalation, and SLA decisions must be recorded.
  - `completed_at` must be populated.

- To enter `FAILED`:
  - A delivery, provider, ingestion, processing, or recording failure must be documented.
  - Error source, code, and retry eligibility must be recorded.
  - `failed_at` must be populated.

- To enter `CANCELLED`:
  - The outbound Interaction must not already be delivered.
  - An authorized cancellation reason and actor must be recorded.
  - Any queued provider operation must be cancelled when supported.

- To enter `ARCHIVED`:
  - The Interaction must already be completed.
  - Required retention, indexing, audit, and legal-hold checks must pass.
  - `archived_at` must be populated.

### Exit Conditions

- An Interaction cannot exit `CREATED` without source, direction, channel, and occurrence time.
- An Interaction cannot exit `DRAFT` toward `QUEUED` without completed communication-policy checks.
- An Interaction cannot exit `QUEUED` toward `SENT` without provider acceptance.
- An Interaction cannot exit `SENT` toward `DELIVERED` without trusted provider confirmation.
- An Interaction cannot exit `IN_PROGRESS` toward `COMPLETED` without an end time and outcome evaluation.
- A failed Interaction may be retried only when the failure is retryable and policy checks remain valid.
- A cancelled Interaction cannot return to an active delivery state; a new Interaction must be created.
- A completed Interaction cannot be edited directly; corrections require a governed replacement, annotation, or redaction.
- An archived Interaction remains historical and cannot return to an active state.

### Terminal States

- **COMPLETED:** The Interaction finished and its governed outcome was recorded.
- **CANCELLED:** The outbound or synchronous Interaction ended before completion.
- **ARCHIVED:** The completed Interaction moved to historical retention.
- **FAILED:** Operationally terminal when retry is not permitted; otherwise it may return to `QUEUED`.

## 7. Relationships

- **Depends On:**
  - Dealership identified by `dealership_id`.
  - Valid communication-channel configuration.
  - Customer consent and contact-permission policies.
  - Trusted communication providers and integration credentials.
  - Identity-resolution, content-security, retention, and SLA policies.

- **Consumes:**
  - Customer, Lead, Qualified Lead, Opportunity, Appointment, Quotation, Trade-In, Finance Application, Deal, and Vehicle context.
  - Customer communication preferences and consent status.
  - Provider messages, calls, emails, delivery receipts, and webhook events.
  - User and AI Agent actions.
  - Campaign and Customer Journey context.
  - Attachment, recording, transcription, translation, and document-processing results.
  - Response-time, escalation, and follow-up rules.

- **Produces:**
  - Canonical omnichannel communication history.
  - Conversation-thread state.
  - Customer intent, sentiment, urgency, and objection context.
  - Customer commitments and dealership promises.
  - Follow-up and SLA requirements.
  - Customer Journey timeline entries.
  - Engagement and response analytics.
  - Governed AI conversation memory.
  - Compliance and dispute evidence.

- **Creates:**
  - Conversation Thread.
  - Follow-up Task.
  - Appointment request.
  - Escalation request.
  - Complaint or compliance-review case.
  - Customer opt-out processing request.
  - Lead, Opportunity, Quotation, Trade-In, Finance Application, or Deal workflow request when justified by the Interaction.

- **Triggers:**
  - Identity-resolution workflow.
  - AI classification and summarization.
  - Response-SLA monitoring.
  - Customer notification workflow.
  - Follow-up Task creation.
  - Escalation and complaint processing.
  - Consent or opt-out updates.
  - Customer Journey updates.
  - Attachment and media processing.
  - Human Review when restricted or conflicting information is detected.

- **Owned By:**
  - The assigned User identified by `assigned_user_id`.
  - The relevant Customer Journey owner when no specific User is assigned.
  - Provider-originated Interactions remain operationally owned by the responsible dealership queue.

- **Referenced By:**
  - Customer
  - Lead
  - Qualified Lead
  - Opportunity
  - Appointment
  - Quotation
  - Trade-In
  - Finance Application
  - Deal
  - Vehicle
  - Task
  - Campaign
  - Complaint
  - Customer Journey
  - Conversation Thread
  - Consent Record
  - Compliance Case
  - AI Agent Run
  - Document Vault
  - Analytics Event

- **Replies To / Belongs To:**
  - An Interaction may reply to another Interaction through `reply_to_interaction_id`.
  - Related Interactions belong to one canonical Conversation Thread.
  - Thread participants, Customer identity, and tenant scope must remain consistent.

- **Supersedes / Corrects:**
  - Completed Interactions are not edited directly.
  - Governed corrections create a replacement Interaction or annotation through `supersedes_interaction_id`.
  - The original record remains immutable and auditable.

## 8. Domain Events

### Emitted Events

- **InteractionCreated**  
  Payload: `interaction_id`, `direction`, `channel`, `source`, `occurred_at`

- **InteractionReceived**  
  Payload: `interaction_id`, `provider_name`, `provider_message_id`, `received_at`

- **InteractionIdentityResolved**  
  Payload: `interaction_id`, `customer_id`, `identity_resolution_status`, `identity_resolution_confidence`

- **InteractionThreadAssigned**  
  Payload: `interaction_id`, `conversation_thread_id`, `sequence_number`

- **InteractionDraftCreated**  
  Payload: `interaction_id`, `sender_id`, `recipient_ids`, `purpose`, `created_at`

- **InteractionQueued**  
  Payload: `interaction_id`, `provider_name`, `queued_at`, `communication_policy_version`

- **InteractionSent**  
  Payload: `interaction_id`, `provider_message_id`, `sent_at`

- **InteractionDelivered**  
  Payload: `interaction_id`, `provider_message_id`, `delivered_at`

- **InteractionRead**  
  Payload: `interaction_id`, `read_at`, `provider_event_id`

- **InteractionStarted**  
  Payload: `interaction_id`, `channel`, `started_at`, `recording_consent_status`

- **InteractionCompleted**  
  Payload: `interaction_id`, `outcome`, `completed_at`, `completed_by`

- **InteractionFailed**  
  Payload: `interaction_id`, `provider_error_code`, `failure_type`, `failed_at`

- **InteractionCancelled**  
  Payload: `interaction_id`, `cancellation_reason`, `cancelled_by`, `cancelled_at`

- **InteractionArchived**  
  Payload: `interaction_id`, `archived_by`, `archived_at`

- **InteractionIntentDetected**  
  Payload: `interaction_id`, `intent`, `intent_confidence`, `model_reference`

- **InteractionSentimentDetected**  
  Payload: `interaction_id`, `sentiment`, `sentiment_score`, `model_reference`

- **InteractionUrgencyDetected**  
  Payload: `interaction_id`, `urgency`, `urgency_score`

- **InteractionCommitmentDetected**  
  Payload: `interaction_id`, `commitment_type`, `commitment_details`, `confidence`

- **InteractionOptOutDetected**  
  Payload: `interaction_id`, `customer_id`, `channel`, `detected_at`

- **InteractionResponseRequired**  
  Payload: `interaction_id`, `response_due_at`, `assigned_user_id`, `priority`

- **InteractionSLABreached**  
  Payload: `interaction_id`, `response_due_at`, `breached_at`, `assigned_user_id`

- **InteractionFollowUpRequired**  
  Payload: `interaction_id`, `task_id`, `next_action_type`, `next_action_at`

- **InteractionEscalationRequired**  
  Payload: `interaction_id`, `escalation_reason`, `priority`, `escalated_at`

- **InteractionHumanReviewRequired**  
  Payload: `interaction_id`, `human_review_reason`, `restricted_data_types`, `created_at`

- **InteractionRedacted**  
  Payload: `interaction_id`, `redaction_reason`, `redacted_by`, `redacted_at`

- **InteractionCorrectionCreated**  
  Payload: `original_interaction_id`, `replacement_interaction_id`, `correction_reason`

### Consumed Events

- **CustomerCreated**  
  Allows unresolved Interactions to be linked after identity resolution.

- **CustomerMerged**  
  Reassigns Interaction ownership to the canonical Customer while preserving historical references.

- **CustomerContactPermissionChanged**  
  Updates communication restrictions and blocks unauthorized outbound delivery.

- **CustomerOptedOut**  
  Cancels or blocks eligible pending promotional Interactions.

- **LeadCreated**  
  Links qualifying early Customer Interactions to the Lead.

- **QualifiedLeadCreated**  
  Adds qualification context to the conversation.

- **OpportunityCreated**  
  Links relevant sales Interactions to the active Opportunity.

- **AppointmentScheduled**  
  Adds scheduling context and confirmation requirements.

- **QuotationIssued**  
  Enables governed Quotation-presentation and follow-up Interactions.

- **TradeInAppraised**  
  Enables Customer-facing Trade-In discussions using approved values.

- **FinanceApplicationStatusChanged**  
  Enables permitted finance-status communication without exposing restricted information.

- **DealStatusChanged**  
  Enables transactional Deal progress communication.

- **VehicleStatusChanged**  
  Revalidates Vehicle-availability statements before outbound communication.

- **TaskCompleted**  
  Updates follow-up completion and response status.

- **ComplaintOpened**  
  Marks related Interactions for enhanced retention and review.

- **ProviderDeliveryEventReceived**  
  Updates sent, delivered, read, failed, or provider-status information.

- **RecordingTranscribed**  
  Adds the governed transcript and processing metadata.

- **AttachmentProcessed**  
  Updates attachment classification, malware status, and document references.

- **LegalHoldApplied**  
  Prevents deletion, redaction, or archival actions that conflict with the hold.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `content_text`
- `content_summary`
- Permitted `transcript_text`
- `conversation_topic`
- `customer_visible_content`
- `outcome_details`
- Customer objections
- Customer preferences
- Vehicle-interest statements
- Appointment requests
- Quotation feedback
- Trade-In feedback
- Finance questions
- Complaint summaries
- Follow-up summaries
- Non-sensitive internal notes approved for AI retrieval

### Fields Excluded from Embeddings

- `interaction_id`
- `customer_id`
- `lead_id`
- `qualified_lead_id`
- `opportunity_id`
- `appointment_id`
- `quotation_id`
- `trade_in_id`
- `finance_application_id`
- `deal_id`
- `provider_message_id`
- `provider_account_id`
- `sender_address`
- `recipient_addresses`
- Phone numbers
- Email addresses
- Customer addresses
- Secure meeting links
- National identifiers
- Bank information
- Credit information
- Payment references
- Signed documents
- Identity documents
- Driving-license documents
- Raw provider metadata
- `consent_snapshot`
- `compliance_snapshot`
- Restricted financial or profitability information

> Personally identifiable information, restricted financial data, authentication details, and secure document content must be supplied only through authorized structured context.

### Structured AI Context Fields

Authorized AI Agents may receive:

- `interaction_type`
- `direction`
- `channel`
- `purpose`
- `priority`
- `status`
- `language_code`
- `intent`
- `intent_confidence`
- `sentiment`
- `sentiment_score`
- `urgency`
- `urgency_score`
- `response_required`
- `response_due_at`
- `follow_up_required`
- `next_action_type`
- `next_action_at`
- `communication_category`
- `customer_journey_stage`
- Permitted Customer preferences
- Permitted Lead, Opportunity, Appointment, Quotation, Vehicle, and Deal context

Restricted Agents may additionally receive:

- Tokenized Customer identifiers
- Consent status
- Compliance-review status
- Restricted-data classifications
- Approved Customer-specific commercial context

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval.
- `interaction_id`
- `conversation_thread_id`
- `customer_id`
- `lead_id`
- `qualified_lead_id`
- `opportunity_id`
- `appointment_id`
- `quotation_id`
- `finance_application_id`
- `deal_id`
- `vehicle_id`
- `channel`
- `direction`
- `purpose`
- `occurred_at`
- `visibility`
- `language_code`

### Confidence Thresholds

- Customer-identity resolution requires confidence of at least `0.95`; lower-confidence matches require human review.
- Intent classification requires confidence of at least `0.85`.
- Appointment-date extraction requires confidence of at least `0.95`.
- Price, payment, finance, Trade-In, or commitment extraction requires confidence of at least `0.95`.
- Customer acceptance or rejection detection requires confidence of at least `0.99`.
- Opt-out detection requires confidence of at least `0.99`.
- Complaint detection requires confidence of at least `0.95`.
- Urgent safety, legal, fraud, or compliance classification requires human review regardless of confidence.
- AI-generated outbound content requires confidence of at least `0.95` and all policy checks.
- AI summaries must distinguish confirmed facts from inferred intent or sentiment.
- No AI confidence score may replace authoritative Customer consent, payment, contract, finance, identity, or Deal evidence.

### Human Approval Thresholds

- AI Agents cannot make binding pricing, discount, Trade-In, finance, payment, contract, reservation, delivery, or legal commitments.
- AI Agents cannot send restricted financial or identity information to unresolved participants.
- AI Agents cannot mark a Customer acceptance as legally binding without authoritative evidence.
- AI Agents cannot alter provider delivery or read evidence.
- AI Agents cannot resolve complaints, legal claims, fraud alerts, or regulatory issues without authorized review.
- AI Agents cannot redact or delete completed communications.
- AI Agents cannot override Customer opt-out or communication restrictions.
- Conflicting identity, consent, Customer commitment, payment, or contract evidence must create a Human Review Task.
- High-risk sentiment, threats, harassment, discrimination allegations, or legal notices must be escalated to authorized Users.

### AI Memory Rules

- Conversation memory must be tenant-scoped and Customer-scoped.
- Memory retrieval must exclude records outside the User or Agent's permission scope.
- Expired, redacted, deleted, or legally restricted content must not be returned.
- AI memory must preserve source Interaction IDs and timestamps.
- Summaries must not replace immutable source Interactions.
- Long-term memory must store only approved, relevant, and non-sensitive facts.
- Customer preferences inferred from one Interaction must remain provisional until confirmed.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/interactions`

### Methods

- `GET` — list or search Interactions.
- `POST` — create an inbound, internal, system, or Draft outbound Interaction.
- `GET /{id}` — retrieve one Interaction according to visibility and security scope.
- `PATCH /{id}` — update permitted fields before completion.
- `POST /{id}/resolve-identity` — associate the Interaction with a Customer or prospect.
- `POST /{id}/assign-thread` — assign or create the canonical Conversation Thread.
- `POST /{id}/queue` — validate and queue an outbound Interaction.
- `POST /{id}/send` — deliver an authorized outbound Interaction.
- `POST /{id}/record-delivery` — process trusted provider delivery evidence.
- `POST /{id}/record-read` — process trusted provider read evidence.
- `POST /{id}/start` — begin a synchronous Interaction.
- `POST /{id}/complete` — complete the Interaction with outcome and follow-up decisions.
- `POST /{id}/retry` — retry an eligible failed outbound Interaction.
- `POST /{id}/cancel` — cancel an eligible queued or in-progress Interaction.
- `POST /{id}/analyze` — request governed AI analysis.
- `POST /{id}/create-follow-up` — create a Task or next-action requirement.
- `POST /{id}/escalate` — create an escalation or complaint workflow.
- `POST /{id}/redact` — perform an authorized governed redaction.
- `POST /{id}/correct` — create a governed replacement Interaction.
- `POST /{id}/archive` — archive a completed Interaction.
- `DELETE /{id}` — perform a soft delete only when legally and operationally permitted.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateInteractionRequest",
  "type": "object",
  "properties": {
    "customer_id": {
      "type": ["string", "null"],
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
    "appointment_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "quotation_id": {
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
    "vehicle_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "conversation_thread_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "interaction_type": {
      "type": "string",
      "enum": [
        "MESSAGE",
        "PHONE_CALL",
        "EMAIL",
        "VIDEO_CALL",
        "IN_PERSON_CONVERSATION",
        "VOICE_NOTE",
        "SOCIAL_MESSAGE",
        "WEB_CHAT",
        "SYSTEM_NOTIFICATION",
        "INTERNAL_NOTE",
        "DOCUMENT_EXCHANGE",
        "CUSTOMER_FEEDBACK",
        "COMPLAINT",
        "OTHER"
      ]
    },
    "direction": {
      "type": "string",
      "enum": [
        "INBOUND",
        "OUTBOUND",
        "INTERNAL",
        "SYSTEM_GENERATED"
      ]
    },
    "channel": {
      "type": "string",
      "enum": [
        "PHONE",
        "SMS",
        "WHATSAPP",
        "EMAIL",
        "WEB_CHAT",
        "WEBSITE_FORM",
        "MOBILE_APP",
        "VIDEO",
        "IN_PERSON",
        "FACEBOOK",
        "INSTAGRAM",
        "MARKETPLACE",
        "OEM_PLATFORM",
        "INTERNAL_SYSTEM",
        "OTHER"
      ]
    },
    "purpose": {
      "type": "string",
      "enum": [
        "GENERAL_INQUIRY",
        "LEAD_FOLLOW_UP",
        "REQUIREMENT_DISCOVERY",
        "VEHICLE_INFORMATION",
        "VEHICLE_AVAILABILITY",
        "APPOINTMENT_SCHEDULING",
        "APPOINTMENT_CONFIRMATION",
        "TEST_DRIVE_FOLLOW_UP",
        "TRADE_IN_DISCUSSION",
        "QUOTATION_PRESENTATION",
        "QUOTATION_FOLLOW_UP",
        "NEGOTIATION",
        "FINANCE_DISCUSSION",
        "DOCUMENT_COLLECTION",
        "CONTRACT_DISCUSSION",
        "PAYMENT_FOLLOW_UP",
        "DELIVERY_COORDINATION",
        "POST_SALE_FOLLOW_UP",
        "COMPLAINT_HANDLING",
        "CUSTOMER_SUPPORT",
        "OPT_OUT_REQUEST",
        "INTERNAL_COORDINATION",
        "OTHER"
      ]
    },
    "priority": {
      "type": "string",
      "enum": [
        "LOW",
        "STANDARD",
        "HIGH",
        "URGENT",
        "CRITICAL",
        "VIP"
      ]
    },
    "source": {
      "type": "string"
    },
    "sender_type": {
      "type": "string",
      "enum": [
        "CUSTOMER",
        "PROSPECT",
        "USER",
        "AI_AGENT",
        "SYSTEM",
        "LENDER",
        "VENDOR",
        "OEM",
        "PARTNER",
        "UNKNOWN"
      ]
    },
    "sender_id": {
      "type": ["string", "null"],
      "format": "uuid"
    },
    "sender_address": {
      "type": ["string", "null"],
      "maxLength": 500
    },
    "recipient_ids": {
      "type": "array",
      "items": {
        "type": "string",
        "format": "uuid"
      }
    },
    "subject": {
      "type": ["string", "null"],
      "maxLength": 500
    },
    "content_text": {
      "type": ["string", "null"],
      "maxLength": 50000
    },
    "language_code": {
      "type": "string",
      "minLength": 2,
      "maxLength": 20
    },
    "occurred_at": {
      "type": "string",
      "format": "date-time"
    },
    "provider_name": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "provider_message_id": {
      "type": ["string", "null"],
      "maxLength": 500
    },
    "response_required": {
      "type": "boolean"
    },
    "response_due_at": {
      "type": ["string", "null"],
      "format": "date-time"
    },
    "visibility": {
      "type": "string",
      "enum": [
        "CUSTOMER_VISIBLE",
        "INTERNAL",
        "RESTRICTED",
        "COMPLIANCE_ONLY",
        "MANAGEMENT_ONLY"
      ]
    }
  },
  "required": [
    "interaction_type",
    "direction",
    "channel",
    "purpose",
    "priority",
    "source",
    "sender_type",
    "recipient_ids",
    "language_code",
    "occurred_at",
    "response_required",
    "visibility"
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Interaction {
  id: ID!
  dealershipId: ID!
  customerId: ID
  leadId: ID
  qualifiedLeadId: ID
  opportunityId: ID
  appointmentId: ID
  quotationId: ID
  tradeInId: ID
  financeApplicationId: ID
  dealId: ID
  vehicleId: ID
  ownerId: ID
  assignedUserId: ID
  agentId: ID
  campaignId: ID
  conversationThreadId: ID
  parentInteractionId: ID
  replyToInteractionId: ID
  rootInteractionId: ID
  taskId: ID
  supersedesInteractionId: ID
  interactionType: InteractionType!
  direction: InteractionDirection!
  channel: InteractionChannel!
  status: InteractionStatus!
  purpose: InteractionPurpose!
  priority: InteractionPriority!
  source: InteractionSource!
  visibility: InteractionVisibility!
  senderType: InteractionParticipantType!
  senderId: ID
  senderDisplayName: String
  subject: String
  contentText: String
  contentSummary: String
  languageCode: String!
  contentRedacted: Boolean!
  attachmentCount: Int!
  transcriptStatus: String
  intent: InteractionIntent
  intentConfidence: Float
  sentiment: InteractionSentiment
  sentimentScore: Float
  urgency: InteractionUrgency!
  urgencyScore: Float
  outcome: InteractionOutcome
  responseRequired: Boolean!
  responseDueAt: DateTime
  respondedAt: DateTime
  responseSlaStatus: ResponseSLAStatus!
  followUpRequired: Boolean!
  nextActionType: NextActionType
  nextActionAt: DateTime
  escalationRequired: Boolean!
  humanReviewRequired: Boolean!
  consentStatus: InteractionConsentStatus!
  identityResolutionStatus: IdentityResolutionStatus!
  providerName: String
  providerMessageId: String
  occurredAt: DateTime!
  receivedAt: DateTime
  queuedAt: DateTime
  sentAt: DateTime
  deliveredAt: DateTime
  readAt: DateTime
  startedAt: DateTime
  endedAt: DateTime
  completedAt: DateTime
  failedAt: DateTime
  archivedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `interactions`
- **Conversation-Thread Table:** `conversation_threads`
- **Participant Table:** `interaction_participants`
- **Attachment Table:** `interaction_attachments`
- **Provider-Event Table:** `interaction_provider_events`
- **Delivery Table:** `interaction_delivery_status`
- **AI-Analysis Table:** `interaction_ai_analysis`
- **Consent-Check Table:** `interaction_consent_checks`
- **Outcome Table:** `interaction_outcomes`
- **SLA Table:** `interaction_sla_tracking`
- **Redaction Table:** `interaction_redactions`
- **Correction Table:** `interaction_corrections`
- **Status-History Table:** `interaction_status_history`
- **Audit Table:** `interaction_audit_log`

### Indexes

- `idx_interactions_tenant_time (dealership_id, occurred_at DESC)`  
  Used for dealership communication timelines.

- `idx_interactions_customer_time (dealership_id, customer_id, occurred_at DESC)`  
  Used for Customer Journey history.

- `idx_interactions_thread_sequence (dealership_id, conversation_thread_id, sequence_number)`  
  Used for ordered conversation retrieval.

- `idx_interactions_lead_time (dealership_id, lead_id, occurred_at DESC)`  
  Used for Lead communication history.

- `idx_interactions_opportunity_time (dealership_id, opportunity_id, occurred_at DESC)`  
  Used for Opportunity engagement history.

- `idx_interactions_deal_time (dealership_id, deal_id, occurred_at DESC)`  
  Used for Deal communication history.

- `idx_interactions_assigned_status (dealership_id, assigned_user_id, status, priority)`  
  Used for User communication queues.

- `idx_interactions_response_due (dealership_id, response_required, response_due_at, status)`  
  Used for SLA and unanswered-message monitoring.

- `idx_interactions_follow_up (dealership_id, follow_up_required, next_action_at)`  
  Used for follow-up queues.

- `idx_interactions_provider_message (provider_name, provider_account_id, provider_message_id)`  
  Used for provider-event idempotency.

- `idx_interactions_provider_event (provider_name, provider_event_id)`  
  Used for webhook-event deduplication.

- `idx_interactions_content_hash (dealership_id, content_hash)`  
  Used for duplicate-content detection.

- `idx_interactions_intent (dealership_id, intent, occurred_at DESC)`  
  Used for intent analytics.

- `idx_interactions_sentiment (dealership_id, sentiment, urgency, occurred_at DESC)`  
  Used for escalation and Customer-experience monitoring.

- `idx_interactions_human_review (dealership_id, human_review_required, priority, created_at)`  
  Used for Human Review queues.

- `idx_interactions_legal_hold (dealership_id, legal_hold_status)`  
  Used for retention and compliance controls.

### Unique Constraints

- `UQ_provider_message (provider_name, provider_account_id, provider_message_id)`  
  Applies when `provider_message_id` is not null.

- `UQ_provider_event (provider_name, provider_event_id)`  
  Applies when `provider_event_id` is not null.

- `UQ_thread_sequence (dealership_id, conversation_thread_id, sequence_number)`  
  Applies when `conversation_thread_id` and `sequence_number` are populated.

- `UQ_interaction_correction (supersedes_interaction_id)`  
  Applies when only one direct governed replacement is permitted.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)` — nullable
- `lead_id` → `leads(id)` — nullable
- `qualified_lead_id` → `qualified_leads(id)` — nullable
- `opportunity_id` → `opportunities(id)` — nullable
- `appointment_id` → `appointments(id)` — nullable
- `quotation_id` → `quotations(id)` — nullable
- `trade_in_id` → `trade_ins(id)` — nullable
- `finance_application_id` → `finance_applications(id)` — nullable
- `deal_id` → `deals(id)` — nullable
- `vehicle_id` → `vehicles(id)` — nullable
- `owner_id` → `users(id)` — nullable
- `assigned_user_id` → `users(id)` — nullable
- `agent_id` → `agents(id)` — nullable
- `campaign_id` → `campaigns(id)` — nullable
- `conversation_thread_id` → `conversation_threads(id)` — nullable
- `parent_interaction_id` → `interactions(id)` — nullable
- `reply_to_interaction_id` → `interactions(id)` — nullable
- `root_interaction_id` → `interactions(id)` — nullable
- `task_id` → `tasks(id)` — nullable
- `supersedes_interaction_id` → `interactions(id)` — nullable
- `created_by` → `users(id)` — nullable
- `reviewed_by` → `users(id)` — nullable
- `redacted_by` → `users(id)` — nullable

### Database Constraints

- `ended_at > started_at`
- `delivered_at >= sent_at`
- `read_at >= delivered_at`
- `completed_at >= occurred_at`
- `intent_confidence BETWEEN 0.00 AND 1.00`
- `identity_resolution_confidence BETWEEN 0.00 AND 1.00`
- `transcript_confidence BETWEEN 0.00 AND 1.00`
- `sentiment_score BETWEEN -1.00 AND 1.00`
- `urgency_score BETWEEN 0.00 AND 1.00`
- `quality_score BETWEEN 0.00 AND 100.00`
- `response_due_at IS NOT NULL` when `response_required = true`.
- `next_action_at IS NOT NULL` when `follow_up_required = true`.
- `escalation_reason IS NOT NULL` when `escalation_required = true`.
- `redaction_reason IS NOT NULL` when `content_redacted = true`.
- `provider_error_code IS NOT NULL` or error details must exist when `status = FAILED`.
- `recording_consent_status = GRANTED` before recording when consent is required.
- A text, attachment, recording, media object, or structured system payload must exist.
- Completed Customer-visible content must be immutable.
- Circular reply, parent, root, and supersession relationships are prohibited.

### Storage Strategy

- Store searchable normalized text separately from encrypted original content.
- Store attachments, recordings, documents, and large transcripts in the encrypted Document Vault.
- Store only secure object references in the primary Interaction record.
- Use cryptographic hashes to detect tampering and duplicate content.
- High-volume provider events may use append-only event storage.
- Search indexes must exclude restricted and redacted content.

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning.
- High-volume deployments may sub-partition by `occurred_at`.
- Provider-event, attachment, participant, AI-analysis, SLA, history, and audit tables must preserve the same tenant-isolation strategy.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Access Customer Interactions assigned to them or linked to permitted Customers and Opportunities.
- **Sales Manager:** Access dealership sales Interactions, escalations, and quality reviews within authorized scope.
- **BDC Agent:** Access early-stage inbound and outbound communication queues.
- **Finance User:** Access finance-related Interactions without unrestricted access to unrelated Customer conversations.
- **Compliance User:** Access restricted, complaint, fraud, consent, redaction, and legal-hold Interactions.
- **Delivery Coordinator:** Access Deal and delivery-related Interactions within assigned scope.
- **Marketing User:** Access campaign Interactions only according to consent and privacy policy; restricted financial and transactional content excluded.
- **Customer Self-Service User:** Access only permitted Customer-visible Interactions belonging to their authenticated identity.
- **AI Interaction Agent:** Service Account access limited to permitted context retrieval, classification, summarization, drafting, and approved send requests.
- **Communication Provider Service:** Restricted inbound-event and outbound-delivery access.
- **Notification Service:** Restricted access to approved transactional and reminder communications.
- **Audit Service:** Read-only access to immutable audit and event metadata.

### PII Classification

- **Level:** `CRITICAL PII`

The Interaction may contain or reference:

- Customer names
- Phone numbers
- Email addresses
- Residential or business addresses
- Voice recordings
- Video recordings
- Message content
- Customer preferences
- Appointment details
- Vehicle interests
- Financial questions
- Trade-In information
- Complaint information
- Identity or supporting documents
- External participant details

### Restricted Data Categories

- National identifiers
- Bank and Payment information
- Credit and finance information
- Signed contracts
- Identity documents
- Driving-license documents
- Medical or accessibility information
- Legal claims
- Fraud indicators
- Internal profitability information
- Authentication credentials
- Secure links and tokens

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for databases, recordings, transcripts, attachments, snapshots, event stores, and backups.
- **Column-Level Protection:** Sender and recipient addresses, Customer contact data, transcripts, consent references, restricted notes, complaint content, and external identifiers require encryption, tokenization, or equivalent approved protection.
- Attachments, recordings, and documents must be stored in an encrypted Document Vault.
- Provider credentials, API keys, and access tokens must be stored in a secrets-management system.
- Secure message, meeting, and attachment links must be time-limited and cryptographically protected.
- Encryption keys must be separated by environment and rotated according to security policy.

### Communication Security

- Provider webhooks must be authenticated and protected against replay attacks.
- Outbound communication must use approved provider accounts and sender identities.
- Public URLs must never expose raw recordings, documents, or Customer content.
- Email and message content must be scanned for malware, phishing, restricted data, and unsafe attachments.
- Customer-facing AI content must pass policy, consent, and restricted-data checks before delivery.
- Internal notes must remain inaccessible through Customer-facing APIs.
- Content rendered as HTML must be sanitized against script injection.
- Message templates must be versioned and approved.

### Consent and Purpose Limitation

- Promotional communication requires valid and current permission.
- Transactional communication must remain limited to its documented operational purpose.
- Customer opt-out requests must be processed promptly and propagated to authoritative consent records.
- Recording consent must be captured when required before recording begins.
- Communication data must not be reused for unrelated marketing, model training, or profiling without lawful authority.
- Consent checks must preserve:
  - Customer identity
  - Channel
  - Purpose
  - Consent status
  - Consent version
  - Policy version
  - Timestamp
  - Result

### Audit Requirements

- Every inbound provider event must preserve:
  - Provider
  - Provider account
  - Provider message or event ID
  - Authentication result
  - Received timestamp
  - Processing result

- Every outbound operation must preserve:
  - Sender
  - Recipients
  - Purpose
  - Consent-check result
  - Policy version
  - Message-template version
  - Provider
  - Actor or Agent
  - Timestamp

- Every content change before completion must record:
  - Previous content hash
  - New content hash
  - Actor
  - Reason
  - Timestamp

- Every AI operation must preserve:
  - Model reference
  - Prompt version
  - Authorized input scope
  - Output
  - Confidence
  - Human approval status
  - Timestamp

- Every Customer opt-out must preserve:
  - Customer identity
  - Original Interaction
  - Channel
  - Evidence
  - Processing result
  - Timestamp

- Every redaction or correction must preserve:
  - Original Interaction ID
  - Reason
  - Authorized actor
  - Approval reference
  - Changed fields
  - Timestamp

- Every complaint, escalation, or legal hold must preserve:
  - Triggering Interaction
  - Classification
  - Responsible User
  - Actions taken
  - Resolution status
  - Timestamp

- Human overrides of AI recommendations must retain both the original recommendation and the final human decision.
- Access to Customer content, recordings, transcripts, attachments, complaints, financial discussions, and consent data must be logged.

### Tenant Isolation

- Every query must enforce the authenticated `dealership_id`.
- Cross-tenant Customer, Lead, Opportunity, Appointment, Quotation, Trade-In, Finance Application, Deal, Vehicle, User, Agent, Campaign, or Interaction linking is prohibited.
- Provider events must be mapped to exactly one authenticated tenant before processing.
- AI Agents, Jobs, integrations, exports, analytics, and semantic retrieval must receive tenant scope through signed execution context.
- Conversation Threads cannot contain participants or Interactions from different tenants.
- Vector retrieval must enforce tenant, Customer, visibility, legal-hold, and permission filters.

### Retention and Deletion

- Interaction retention must follow communication, privacy, contractual, financial, complaint, regulatory, and legal requirements.
- Completed Customer-visible Interactions must remain immutable.
- Records linked to Deals, Finance Applications, Payments, Contracts, complaints, disputes, or legal holds must not be hard-deleted while dependencies remain.
- Legal hold overrides ordinary deletion and archival schedules.
- Soft deletion is the operational default for eligible records.
- Redaction should be used when specific sensitive content must be removed while preserving the existence and audit history of the Interaction.
- Legally approved deletion requests must purge or anonymize permitted PII across:
  - Interaction records
  - Conversation Threads
  - Participant records
  - Attachments and documents
  - Recordings and transcripts
  - Provider metadata
  - AI analysis and embeddings
  - Search indexes
  - Customer Journey summaries
  - Analytics stores
  - Audit references
  - Backups, according to the approved retention policy
