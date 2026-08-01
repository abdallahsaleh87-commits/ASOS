# Interaction

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Interaction Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Interaction Object represents one governed communication, contact, communication attempt, communication session, internal note, or meaningful engagement record involving the dealership and one or more participants.

Interactions may include:

- Incoming and outgoing messages.
- Phone calls.
- Emails.
- SMS messages.
- WhatsApp messages.
- Website chat.
- Mobile application chat.
- Marketplace communication.
- Social-media communication.
- Video calls.
- In-person conversations.
- Voice notes.
- Document exchanges.
- Appointment discussions.
- Test-drive feedback.
- Quotation presentation.
- Trade-In discussion.
- Finance and contract communication.
- Payment reminders.
- Delivery coordination.
- Customer complaints.
- Customer feedback.
- Customer opt-out requests.
- Authorized internal notes.
- Authorized automated communication.
- AI-assisted communication.

The Interaction provides a governed chronological record that supports:

- Communication continuity.
- Omnichannel inboxes.
- Contact-center workflows.
- Customer Journey timelines.
- Response-SLA monitoring.
- Follow-up orchestration.
- Identity-resolution workflows.
- Consent and communication-policy enforcement.
- Complaint and dispute investigation.
- Evidence preservation.
- Customer intent and sentiment analysis.
- Human and AI collaboration.
- Sales, service, and operational analytics.

### Interaction as Evidence

An Interaction records what was:

- Received.
- Sent.
- Said.
- Written.
- Presented.
- Requested.
- Attempted.
- Delivered.
- Read where supported.
- Recorded.
- Observed.
- Confirmed by a provider.
- Entered as an internal note.

An Interaction may contain evidence that supports another Domain Object.

It does not independently make the resulting business outcome authoritative.

Examples:

```text
Customer asks for an Appointment
  ≠ Appointment created

Customer asks for a Quotation
  ≠ Quotation issued

Customer says an offer looks acceptable
  ≠ Quotation authoritatively accepted

Customer discusses finance
  ≠ Finance Application submitted

Customer says payment was sent
  ≠ Payment cleared

Customer asks when the Vehicle will arrive
  ≠ Vehicle delivered

Salesperson records a commitment
  ≠ Binding contractual commitment
```

A controlled downstream workflow must validate the evidence and create or update the responsible Domain Object.

### Observation, Evidence, and Derived Intelligence Separation

Interaction data must preserve explicit separation between:

```text
Original Communication Evidence
  = provider payload, recording, document, message, or Human-entered note

Normalized Interaction Content
  = governed representation of the original evidence

Derived Intelligence
  = AI-generated intent, sentiment, summary, entity extraction,
    risk detection, or Recommendation

Authoritative Human Decision
  = approved interpretation or operational Decision where required

External Confirmation
  = authoritative provider or external-system completion evidence
```

Derived Intelligence must not overwrite original evidence.

### Interaction and Conversation Separation

An Interaction represents one communication unit or governed session.

A Conversation Thread represents the ordered relationship among multiple Interactions.

Examples:

```text
One inbound WhatsApp message
  = one Interaction

One outbound reply
  = another Interaction

The related message history
  = one Conversation Thread

One completed phone call
  = one Interaction session

Call recording and transcript
  = evidence and derived records associated with that Interaction
```

The Interaction Object may reference a Conversation Thread.

Thread identity, sequence, and participant continuity must remain governed and auditable.

### Interaction and Communication Command Separation

The following concepts must remain separate:

```text
Outbound Draft
  = prepared but not yet authorized or sent

Recommendation
  = proposed content or proposed next action

Human Decision or Approved Automation Policy
  = authority to continue

Command
  = request sent to a communication provider

Provider Acknowledgement
  = provider accepted or received the technical request

Delivery Confirmation
  = provider reports delivery where supported

Read Confirmation
  = provider reports opening or acknowledgement where supported

Customer Understanding or Acceptance
  = separate evidence and business interpretation
```

A successful communication API request does not prove delivery.

Delivery does not prove that the Customer read, understood, agreed with, or accepted the content.

### Inbound and Outbound Separation

Inbound Interactions represent communication received from an external or internal participant.

Outbound Interactions represent communication initiated by an authorized dealership User, approved system workflow, or authorized AI-assisted process.

Inbound receipt does not automatically establish:

- Customer identity.
- Customer ownership of a phone number or email address.
- Marketing Consent.
- Authority to disclose sensitive data.
- Authority to create a binding commercial outcome.
- Authenticity of an attached document.

Outbound communication requires applicable:

- Identity and recipient checks.
- Purpose.
- Communication permission.
- Channel permission.
- Quiet-hours checks.
- Frequency controls.
- Content controls.
- Human Approval or approved automation policy.
- Provider Command.
- Audit evidence.

### Internal Note Separation

An internal note is an Interaction with restricted visibility.

Internal notes must remain separate from Customer-visible communication.

An internal note:

- Must identify its author.
- Must preserve the business purpose.
- Must not be delivered to the Customer.
- Must not impersonate Customer communication.
- Must not be presented as external evidence.
- Must not silently change another Domain Object.
- May require enhanced access restrictions.
- May be corrected only through governed correction or supersession.

### Interaction and Customer Separation

The Customer Domain Service owns canonical Customer identity and contact points.

The Interaction may initially contain unresolved participant information such as:

- Phone number.
- Email address.
- Provider user identifier.
- Social-media account.
- Marketplace account.
- Display name.
- Submitted name.

These are communication observations.

They are not automatically verified Customer identity.

Identity resolution must remain a separate governed process.

### Interaction and Consent Separation

The Customer Domain Service and applicable Consent authority own canonical Consent and communication permissions.

The Interaction preserves the exact permission and policy snapshot applied to an outbound communication.

An inbound message does not automatically create:

- Marketing Consent.
- Cross-channel Consent.
- Consent for unrelated follow-up.
- Consent to record a call.
- Consent to share data with a third party.

An opt-out request is Customer evidence that must trigger deterministic permission processing.

AI may detect a possible opt-out.

AI detection alone must not be the final Consent record without the governed opt-out workflow.

### Interaction and Appointment Separation

An Interaction may contain evidence that a Customer:

- Requested an Appointment.
- Confirmed an Appointment.
- Requested rescheduling.
- Requested cancellation.
- Reported late arrival.
- Reported non-attendance.

The Appointment Domain Service owns authoritative scheduling and attendance state.

An Interaction outcome such as `APPOINTMENT_REQUESTED` does not itself create or confirm an Appointment.

### Interaction and Quotation Separation

An Interaction may record:

- Quotation request.
- Quotation presentation.
- Customer questions.
- Negotiation.
- Customer response.
- Possible acceptance signal.
- Possible rejection signal.

The Quotation Domain Service owns:

- Quotation version.
- Issuance.
- Presentation evidence.
- Customer acceptance.
- Decline.
- Expiration.
- Supersession.
- Conversion.

Free text alone must not silently set an authoritative Quotation outcome.

### Interaction and Finance Separation

Finance-related Interactions may contain highly sensitive information.

The Interaction must not become the unrestricted storage location for:

- Identity documents.
- National identifiers.
- Credit reports.
- Bank statements.
- Lender Decisions.
- Funding instructions.
- Signed Financial Contracts.

Controlled document or finance services must store the authoritative records.

The Interaction may preserve secure references and permitted summaries.

### Interaction and Deal Separation

Interactions may support:

- Customer commitment.
- Deal updates.
- Document coordination.
- Payment follow-up.
- Funding updates.
- Delivery coordination.
- Cancellation requests.
- Dispute evidence.

The Deal Domain Service owns authoritative transaction state.

An Interaction must not independently mark a Deal:

- Approved.
- Funded.
- Delivered.
- Completed.
- Cancelled.
- Unwound.

### Interaction Immutability

Original inbound evidence and finalized outbound content must be immutable.

Material corrections require:

- A correction record.
- A superseding Interaction.
- A governed redaction.
- An annotation.
- A provider correction where supported.
- Preserved original hashes and lineage.

Redaction must not rewrite history without preserving the required audit evidence.

### System Purpose

The Interaction Object provides governed communication context to:

- Customer workflows.
- Lead workflows.
- Qualified Lead workflows.
- Opportunity workflows.
- Appointment workflows.
- Quotation workflows.
- Vehicle and Inventory workflows.
- Trade-In workflows.
- Finance Application workflows.
- Financial Contract workflows.
- Deal workflows.
- Complaint workflows.
- Task and follow-up workflows.
- Communication providers.
- AI Agents.
- Analytics.
- Audit and compliance services.

The Interaction may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Original inbound provider message | Communication Provider |
| Original call or meeting recording | Approved recording source |
| Human-entered internal note | Authorized Human author |
| Canonical Interaction record | Interaction Domain Service |
| Customer identity | Customer Domain Service |
| Communication Consent | Customer Domain Service or configured Consent authority |
| Provider delivery and read status | Communication Provider |
| Appointment state | Appointment Domain Service |
| Quotation state and acceptance | Quotation Domain Service |
| Finance Application state | Finance Application Domain Service |
| Financial Contract and signatures | Financial Contract Domain Service |
| Deal state | Deal Domain Service |
| Payment outcome | Payment authority |
| Delivery outcome | Delivery authority |
| Transcript and translation | Approved provider or Derived Intelligence |
| Intent, sentiment, summary, and Recommendation | Derived Intelligence |
| Operational interpretation | Authorized Human or approved deterministic workflow |
| External communication completion | Configured External Confirmation authority |

---

## 2. Canonical Schema

### Primary Identifiers

- `interaction_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `interaction_version` — Integer for governed content versions where applicable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `department_id`.
- `team_id`.
- `responsible_user_id`.
- `assigned_user_id`.
- `supervisor_user_id`.
- `queue_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Related Domain Objects

- `customer_id`.
- `lead_id`.
- `qualified_lead_id`.
- `opportunity_id`.
- `appointment_id`.
- `quotation_id`.
- `quotation_version`.
- `vehicle_id`.
- `inventory_record_id`.
- `trade_in_id`.
- `finance_application_id`.
- `financial_contract_id`.
- `deal_id`.
- `primary_task_reference`.
- `campaign_reference`.
- `complaint_reference`.
- `dispute_reference`.

### Interaction Identity and Classification

- `interaction_number`.
- `interaction_type`.
- `direction`.
- `channel`.
- `status`.
- `purpose`.
- `communication_category`.
- `priority`.
- `visibility`.
- `source_system`.
- `source_authority`.
- `workflow_authority_mode`.
- `customer_journey_stage_projection`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Conversation and Threading

- `conversation_thread_id`.
- `external_conversation_id`.
- `external_thread_id`.
- `root_interaction_id`.
- `parent_interaction_id`.
- `reply_to_interaction_id`.
- `supersedes_interaction_id`.
- `corrects_interaction_id`.
- `sequence_number`.
- `thread_sequence_reference`.
- `subject`.
- `conversation_topic`.
- `thread_status_projection`.
- `thread_participant_snapshot`.
- `thread_last_interaction_at`.

### Participant Records

- `participant_record_ids`.
- `sender_participant_id`.
- `recipient_participant_ids`.
- `cc_participant_ids`.
- `bcc_participant_ids`.
- `external_participant_count`.
- `participant_snapshot`.
- `participant_snapshot_hash`.

Each participant record may contain:

- `interaction_participant_id`.
- `participant_role`.
- `participant_type`.
- `customer_id`.
- `user_id`.
- `agent_id`.
- `external_party_reference`.
- `display_name_projection`.
- `contact_point_reference`.
- `submitted_contact_value`.
- `normalized_contact_value_token`.
- `provider_participant_id`.
- `identity_resolution_status`.
- `identity_resolution_method`.
- `identity_resolution_evidence_references`.
- `identity_resolution_decision_id`.
- `authentication_status`.
- `signing_or_commitment_authority_status`.
- `participant_visibility`.
- `participant_status`.

### Customer Identity Resolution

- `customer_identity_resolution_status`.
- `candidate_customer_ids`.
- `resolved_customer_id`.
- `identity_resolution_rule_id`.
- `identity_resolution_rule_version`.
- `identity_resolution_evidence_references`.
- `identity_resolution_decision_id`.
- `identity_resolved_at`.
- `identity_resolved_by_actor_id`.
- `identity_conflict_references`.
- `identity_revalidation_required`.

### Content Evidence

- `original_content_reference`.
- `original_content_type`.
- `original_content_hash`.
- `original_provider_payload_reference`.
- `original_provider_payload_hash`.
- `normalized_text`.
- `normalized_html_reference`.
- `structured_content`.
- `content_language_code`.
- `content_encoding`.
- `content_hash`.
- `content_mime_type`.
- `subject`.
- `content_character_count`.
- `content_word_count`.
- `content_finalized_at`.
- `content_integrity_status`.

Original provider content should normally remain in controlled evidence storage.

### Customer-Visible Content

- `customer_visible_text`.
- `customer_visible_html_reference`.
- `customer_visible_document_references`.
- `customer_visible_content_hash`.
- `customer_visible_language_code`.
- `customer_visible_template_id`.
- `customer_visible_template_version`.
- `customer_visible_content_status`.

### Internal Content

- `internal_note_text`.
- `internal_note_category`.
- `internal_note_reason`.
- `internal_note_author_id`.
- `internal_note_visibility`.
- `internal_note_finalized_at`.
- `internal_note_hash`.

Internal content must never be included in Customer-visible rendering.

### Attachments and Documents

- `attachment_ids`.
- `attachment_count`.
- `attachment_processing_status`.
- `attachment_security_status`.
- `attachment_malware_scan_status`.
- `attachment_data_loss_prevention_status`.
- `attachment_content_moderation_status`.
- `attachment_document_references`.
- `attachment_media_references`.
- `attachment_snapshot`.
- `attachment_snapshot_hash`.
- `restricted_attachment_detected`.
- `untrusted_document_detected`.

Each attachment record may contain:

- `interaction_attachment_id`.
- `document_reference`.
- `media_reference`.
- `file_name_projection`.
- `mime_type`.
- `size_bytes`.
- `content_hash`.
- `source`.
- `security_classification`.
- `malware_scan_status`.
- `data_loss_prevention_status`.
- `content_moderation_status`.
- `processing_status`.
- `retention_class`.
- `legal_hold_status`.

### Call and Synchronous Session

- `session_id`.
- `session_type`.
- `session_status`.
- `provider_call_id`.
- `provider_meeting_id`.
- `started_at`.
- `answered_at`.
- `ended_at`.
- `duration_seconds`.
- `ring_duration_seconds`.
- `hold_duration_seconds`.
- `participant_join_events`.
- `participant_leave_events`.
- `disconnection_reason`.
- `call_disposition`.
- `session_quality_status`.
- `session_quality_metrics`.

### Recording

- `recording_required`.
- `recording_status`.
- `recording_consent_required`.
- `recording_consent_status`.
- `recording_consent_reference`.
- `recording_started_at`.
- `recording_stopped_at`.
- `recording_reference`.
- `recording_hash`.
- `recording_duration_seconds`.
- `recording_provider_reference`.
- `recording_retention_class`.
- `recording_legal_hold_status`.
- `recording_access_status`.

### Transcript

- `transcript_status`.
- `transcript_reference`.
- `transcript_text`.
- `transcript_language_code`.
- `transcript_provider`.
- `transcript_model_reference`.
- `transcript_version`.
- `transcript_generated_at`.
- `transcript_hash`.
- `transcript_speaker_segments`.
- `transcript_quality_status`.
- `transcript_review_status`.
- `transcript_correction_references`.

A transcript is not the original recording.

### Translation

- `translation_status`.
- `source_language_code`.
- `target_language_code`.
- `translated_text`.
- `translation_reference`.
- `translation_provider`.
- `translation_model_reference`.
- `translation_version`.
- `translation_generated_at`.
- `translation_hash`.
- `translation_review_status`.

A translation is not the original communication evidence.

### Communication Purpose and Policy

- `communication_purpose`.
- `communication_subpurpose`.
- `transactional_or_marketing_classification`.
- `lawful_basis`.
- `consent_requirement_status`.
- `consent_record_ids`.
- `consent_snapshot`.
- `consent_snapshot_hash`.
- `contact_permission_status`.
- `channel_permission_status`.
- `recipient_permission_status`.
- `quiet_hours_status`.
- `frequency_limit_status`.
- `communication_policy_id`.
- `communication_policy_version`.
- `policy_evaluated_at`.
- `policy_expires_at`.
- `policy_revalidation_required`.
- `policy_block_reasons`.

### Opt-Out Context

- `opt_out_signal_status`.
- `opt_out_signal_type`.
- `opt_out_detected_at`.
- `opt_out_evidence_references`.
- `opt_out_processing_status`.
- `opt_out_command_id`.
- `opt_out_idempotency_key`.
- `opt_out_processed_at`.
- `opt_out_confirmation_status`.
- `opt_out_reconciliation_status`.
- `opt_out_human_review_required`.

### Outbound Draft

- `draft_status`.
- `draft_content_reference`.
- `draft_content_hash`.
- `draft_created_at`.
- `draft_created_by_actor_type`.
- `draft_created_by_actor_id`.
- `draft_source_type`.
- `draft_template_id`.
- `draft_template_version`.
- `draft_expiration_at`.
- `draft_review_required`.
- `draft_review_status`.
- `draft_review_decision_id`.
- `draft_approved_at`.
- `draft_approved_by_actor_id`.

### Outbound Command

- `send_authorization_status`.
- `send_authorization_decision_id`.
- `automation_policy_id`.
- `automation_policy_version`.
- `send_request_id`.
- `send_command_id`.
- `send_idempotency_key`.
- `send_requested_at`.
- `send_command_created_at`.
- `send_command_status`.
- `send_attempt_count`.
- `last_send_attempt_at`.
- `next_retry_at`.
- `send_failure_reason`.

### Provider Context

- `provider_id`.
- `provider_name`.
- `provider_account_id`.
- `provider_channel_id`.
- `provider_message_id`.
- `provider_conversation_id`.
- `provider_thread_id`.
- `provider_event_ids`.
- `provider_status`.
- `provider_status_code`.
- `provider_error_code`.
- `provider_error_message_reference`.
- `provider_payload_references`.
- `provider_received_at`.
- `provider_accepted_at`.
- `provider_updated_at`.
- `provider_data_freshness_status`.

### Provider Deduplication

- `provider_deduplication_key`.
- `provider_message_deduplication_status`.
- `provider_event_deduplication_status`.
- `duplicate_provider_record_ids`.
- `provider_duplicate_detected_at`.
- `provider_reconciliation_status`.

Provider deduplication keys remain separate from:

- ASOS `event_id`.
- Command `idempotency_key`.
- Canonical `interaction_id`.

### Delivery and Read Projection

- `delivery_status`.
- `delivery_confirmation_status`.
- `sent_at`.
- `provider_accepted_at`.
- `delivered_at`.
- `delivery_failed_at`.
- `read_status`.
- `read_at`.
- `read_confirmation_status`.
- `recipient_response_status`.
- `recipient_responded_at`.
- `bounce_status`.
- `bounce_type`.
- `unsubscribe_status`.
- `delivery_evidence_references`.
- `read_evidence_references`.
- `delivery_reconciliation_status`.

### Interaction Timing

- `occurred_at`.
- `recorded_at`.
- `received_at`.
- `created_at`.
- `queued_at`.
- `sent_at`.
- `delivered_at`.
- `read_at`.
- `started_at`.
- `answered_at`.
- `ended_at`.
- `completed_at`.
- `failed_at`.
- `cancelled_at`.
- `redacted_at`.
- `archived_at`.

`occurred_at` represents business occurrence time.

`recorded_at` represents when ASOS recorded the evidence.

### Outcome

- `outcome_status`.
- `outcome_type`.
- `outcome_details`.
- `outcome_evidence_references`.
- `outcome_recorded_at`.
- `outcome_recorded_by_actor_id`.
- `outcome_authority`.
- `downstream_action_required`.
- `downstream_action_type`.
- `downstream_action_reference`.
- `outcome_review_status`.
- `outcome_decision_id`.

### Response and SLA

- `response_required`.
- `response_requirement_reason`.
- `response_policy_id`.
- `response_policy_version`.
- `response_due_at`.
- `first_response_interaction_id`.
- `first_responded_at`.
- `response_time_seconds`.
- `response_sla_status`.
- `response_sla_breached_at`.
- `response_sla_waiver_decision_id`.
- `response_sla_completed_at`.

### Follow-Up and Escalation

- `follow_up_required`.
- `follow_up_reason`.
- `recommended_follow_up_at`.
- `authoritative_follow_up_at`.
- `follow_up_task_reference`.
- `follow_up_status`.
- `escalation_required`.
- `escalation_reason`.
- `escalation_policy_id`.
- `escalation_reference`.
- `escalated_at`.
- `escalation_resolved_at`.

### Complaint and Dispute Context

- `complaint_signal_status`.
- `complaint_type_projection`.
- `complaint_reference`.
- `complaint_created_at`.
- `dispute_signal_status`.
- `dispute_reference`.
- `legal_hold_status`.
- `legal_hold_reference`.
- `investigation_status`.
- `investigation_evidence_references`.

An Interaction complaint signal does not itself resolve or close a complaint.

### Content Safety and Compliance

- `content_moderation_status`.
- `restricted_data_status`.
- `restricted_data_types`.
- `sensitive_data_detected`.
- `personal_data_detected`.
- `financial_data_detected`.
- `identity_document_detected`.
- `payment_instruction_detected`.
- `prompt_injection_status`.
- `malicious_content_status`.
- `compliance_review_status`.
- `compliance_block_reasons`.
- `compliance_evidence_references`.

### Redaction and Correction

- `redaction_status`.
- `redaction_type`.
- `redaction_reason`.
- `redaction_policy_id`.
- `redaction_decision_id`.
- `redacted_content_reference`.
- `redacted_content_hash`.
- `redaction_applied_at`.
- `redaction_applied_by_actor_id`.
- `correction_status`.
- `correction_reason`.
- `correction_interaction_id`.
- `supersession_status`.
- `superseded_at`.

### Derived Intelligence

- `intent_classification`.
- `intent_scores`.
- `sentiment_classification`.
- `sentiment_score`.
- `urgency_classification`.
- `urgency_score`.
- `topic_labels`.
- `entity_extraction_results`.
- `vehicle_interest_extraction`.
- `appointment_signal`.
- `quotation_signal`.
- `finance_signal`.
- `trade_in_signal`.
- `payment_signal`.
- `delivery_signal`.
- `commitment_signal`.
- `acceptance_signal`.
- `rejection_signal`.
- `opt_out_signal`.
- `complaint_signal`.
- `risk_signal`.
- `ai_summary`.
- `recommended_next_action`.
- `recommended_response`.
- `recommended_priority`.
- `recommended_human_review`.
- `requires_human_review`.
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

### AI Processing

- `ai_processing_status`.
- `ai_processing_request_ids`.
- `ai_agent_run_ids`.
- `ai_last_processed_at`.
- `ai_processing_failure_reasons`.
- `ai_processing_block_reasons`.
- `ai_review_status`.
- `ai_review_decision_id`.

Canonical ingestion must not depend on successful AI processing.

### Computed Projections

- `interaction_age_seconds`.
- `interaction_age_days`.
- `conversation_duration_seconds`.
- `time_to_first_response_seconds`.
- `time_to_delivery_seconds`.
- `is_unanswered`.
- `is_overdue`.
- `is_customer_visible`.
- `is_internal_only`.
- `is_recorded`.
- `has_attachments`.
- `has_restricted_data`.
- `requires_follow_up`.
- `sla_breached`.
- `thread_interaction_count`.
- `days_since_last_customer_interaction`.
- `days_since_last_outbound_interaction`.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_created_at`.
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
- `finalized_at`.
- `finalized_by_actor_id`.
- `archived_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `interaction_id` | UUID | Yes | ASOS | Immutable Canonical Interaction identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `interaction_number` | String | Yes | ASOS or configured authority | Human-readable Interaction reference. |
| `interaction_type` | Enum | Yes | Source or workflow | Form of communication or engagement. |
| `direction` | Enum | Yes | Source or workflow | Inbound, outbound, internal, or system direction. |
| `channel` | Enum | Yes | Source or workflow | Communication channel. |
| `status` | Enum | Yes | Interaction workflow | Current Interaction lifecycle state. |
| `purpose` | Enum | Yes | Human, deterministic workflow, or Derived Intelligence | Governed communication purpose. |
| `visibility` | Enum | Yes | Authorization policy | Permitted visibility of the Interaction. |
| `occurred_at` | Timestamp | Yes | Source or authorized Human | Business occurrence time. |
| `recorded_at` | Timestamp | Yes | ASOS | Time the Interaction was recorded. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Relationship Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | UUID | Conditional | Customer relationship | Resolved Customer where identity is sufficiently established. |
| `lead_id` | UUID | No | Canonical relationship | Lead associated with the Interaction. |
| `qualified_lead_id` | UUID | No | Canonical relationship | Qualified Lead supported by the Interaction. |
| `opportunity_id` | UUID | No | Canonical relationship | Opportunity supported by the Interaction. |
| `appointment_id` | UUID | No | Canonical relationship | Appointment discussed by the Interaction. |
| `quotation_id` | UUID | No | Canonical relationship | Quotation discussed or presented. |
| `vehicle_id` | UUID | No | Canonical relationship | Vehicle referenced by the Interaction. |
| `trade_in_id` | UUID | No | Canonical relationship | Trade-In discussed. |
| `finance_application_id` | UUID | No | Canonical relationship | Finance Application discussed. |
| `financial_contract_id` | UUID | No | Canonical relationship | Financial Contract discussed. |
| `deal_id` | UUID | No | Canonical relationship | Deal supported by the Interaction. |

### Threading Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `conversation_thread_id` | UUID | Conditional | Thread workflow | Canonical Conversation Thread. |
| `root_interaction_id` | UUID | No | Thread workflow | First Interaction in the thread. |
| `parent_interaction_id` | UUID | No | Thread workflow | Parent Interaction where applicable. |
| `reply_to_interaction_id` | UUID | No | Thread workflow | Interaction directly answered. |
| `sequence_number` | Integer | No | Thread workflow | Canonical ordered sequence. |
| `external_conversation_id` | String | No | Provider | External conversation reference. |
| `external_thread_id` | String | No | Provider | External thread reference. |

### Participant Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `interaction_participant_id` | UUID | Yes | ASOS | Identifier of one Interaction participant. |
| `participant_role` | Enum | Yes | Source or workflow | Sender, recipient, copied participant, observer, or internal participant. |
| `participant_type` | Enum | Yes | Source or workflow | Customer, User, AI Agent, external party, or system. |
| `customer_id` | UUID | No | Customer relationship | Resolved Customer represented by the participant. |
| `user_id` | UUID | No | Identity service | Internal authorized User. |
| `agent_id` | UUID | No | Agent registry | Authorized AI Agent. |
| `submitted_contact_value` | String | No | External source | Contact value received before verification. |
| `normalized_contact_value_token` | String | No | Secure contact service | Protected normalized contact token. |
| `identity_resolution_status` | Enum | Yes | Identity-resolution workflow | Current participant resolution state. |
| `authentication_status` | Enum | Yes | Authentication authority | Whether the participant was authenticated for the interaction purpose. |

### Content Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `original_content_reference` | String | Conditional | Evidence repository | Controlled reference to original source content. |
| `original_content_hash` | String | Conditional | ASOS | Integrity hash of original evidence. |
| `normalized_text` | Text | Conditional | Normalization workflow | Governed normalized representation. |
| `structured_content` | JSON Object | No | Source or approved workflow | Structured message or system content. |
| `content_language_code` | String | Yes | Source, Human, or Derived Intelligence | BCP 47 language tag. |
| `content_hash` | String | Yes | ASOS | Hash of finalized normalized content. |
| `customer_visible_text` | Text | Conditional | Authorized outbound workflow | Exact Customer-visible text. |
| `customer_visible_content_hash` | String | Conditional | ASOS | Hash of finalized Customer-visible content. |
| `internal_note_text` | Text | Conditional | Authorized Human | Restricted internal note content. |
| `internal_note_hash` | String | Conditional | ASOS | Integrity hash of finalized internal note. |

### Provider Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `provider_id` | UUID | Conditional | Integration registry | Configured communication provider. |
| `provider_account_id` | String | Conditional | Provider configuration | Provider account used. |
| `provider_message_id` | String | No | Provider | External provider message identifier. |
| `provider_event_ids` | Array | No | Provider | Provider events associated with the Interaction. |
| `provider_status` | String | No | Provider | Latest normalized provider status. |
| `provider_deduplication_key` | String | Conditional | Integration workflow | Key used to prevent duplicate provider ingestion effects. |
| `provider_reconciliation_status` | Enum | Yes | Reconciliation workflow | Current provider reconciliation state. |

### Permission Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `communication_purpose` | Enum | Yes | Authorized workflow | Purpose for which communication occurs. |
| `transactional_or_marketing_classification` | Enum | Yes | Deterministic policy | Communication classification. |
| `lawful_basis` | Enum | Conditional | Consent or legal authority | Applicable processing basis. |
| `consent_requirement_status` | Enum | Yes | Deterministic policy | Whether Consent is required. |
| `consent_record_ids` | Array | No | Consent authority | Applicable Consent records. |
| `contact_permission_status` | Enum | Yes | Policy Engine | Contact permission result. |
| `channel_permission_status` | Enum | Yes | Policy Engine | Channel permission result. |
| `quiet_hours_status` | Enum | Yes | Policy Engine | Quiet-hours evaluation result. |
| `frequency_limit_status` | Enum | Yes | Policy Engine | Frequency-control result. |
| `communication_policy_version` | String | Yes | Policy Engine | Applied policy version. |

### Command and Delivery Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `send_authorization_status` | Enum | Conditional | Approval workflow | Authority to execute outbound delivery. |
| `send_command_id` | UUID | No | Command Orchestration | Outbound communication Command. |
| `send_idempotency_key` | String | Conditional | Command workflow | Retry-protection key. |
| `send_command_status` | Enum | Yes | Command workflow | Current outbound Command state. |
| `delivery_status` | Enum | Yes | Provider Projection | Current delivery state. |
| `delivery_confirmation_status` | Enum | Yes | Workflow Projection | Provider delivery Confirmation state. |
| `sent_at` | Timestamp | No | Provider or workflow | Time provider accepted the send where applicable. |
| `delivered_at` | Timestamp | No | Provider | Time delivery was reported. |
| `read_at` | Timestamp | No | Provider | Time read or opened status was reported. |

### Outcome and Follow-Up Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `outcome_status` | Enum | Yes | Outcome workflow | Current outcome-recording state. |
| `outcome_type` | Enum | No | Human or trusted workflow | Governed Interaction outcome. |
| `outcome_evidence_references` | Array | No | Evidence repository | Supporting evidence. |
| `response_required` | Boolean | Yes | Deterministic workflow or Human | Whether a response is required. |
| `response_due_at` | Timestamp | Conditional | SLA policy | Response deadline. |
| `response_sla_status` | Enum | Yes | SLA workflow | Current SLA state. |
| `follow_up_required` | Boolean | Yes | Deterministic workflow or Human | Whether follow-up is required. |
| `follow_up_task_reference` | String | No | Task workflow | Follow-up task reference. |
| `escalation_required` | Boolean | Yes | Policy or Human | Whether escalation is required. |

### Derived Intelligence Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `intent_classification` | Enum | No | Derived Intelligence | Predicted Interaction intent. |
| `sentiment_classification` | Enum | No | Derived Intelligence | Predicted sentiment. |
| `urgency_classification` | Enum | No | Derived Intelligence | Predicted urgency. |
| `commitment_signal` | Enum | No | Derived Intelligence | Possible commitment indicator. |
| `acceptance_signal` | Enum | No | Derived Intelligence | Possible acceptance indicator. |
| `opt_out_signal` | Enum | No | Derived Intelligence | Possible opt-out indicator. |
| `ai_summary` | Text | No | Derived Intelligence | Evidence-grounded summary. |
| `recommended_next_action` | Enum | No | Derived Intelligence | Proposed next action. |
| `requires_human_review` | Boolean | Yes | Policy or Derived Intelligence | Indicates required Human Review. |

---

## 4. Enumerations

### InteractionStatus

- `CREATED`
- `DRAFT`
- `APPROVAL_PENDING`
- `READY_TO_SEND`
- `SEND_PENDING`
- `QUEUED`
- `SENT`
- `DELIVERED`
- `READ`
- `RECEIVED`
- `IN_PROGRESS`
- `COMPLETED`
- `FAILED`
- `CANCELLED`
- `REDACTED`
- `SUPERSEDED`
- `ARCHIVED`

### InteractionType

- `TEXT_MESSAGE`
- `EMAIL`
- `PHONE_CALL`
- `VIDEO_CALL`
- `IN_PERSON_CONVERSATION`
- `VOICE_NOTE`
- `WEB_CHAT`
- `SOCIAL_MESSAGE`
- `MARKETPLACE_MESSAGE`
- `WEBSITE_FORM_SUBMISSION`
- `MOBILE_APP_MESSAGE`
- `DOCUMENT_EXCHANGE`
- `SYSTEM_NOTIFICATION`
- `INTERNAL_NOTE`
- `CUSTOMER_FEEDBACK`
- `COMPLAINT`
- `SURVEY_RESPONSE`
- `OTHER`

### InteractionDirection

- `INBOUND`
- `OUTBOUND`
- `INTERNAL`
- `SYSTEM_GENERATED`

### InteractionChannel

- `PHONE`
- `SMS`
- `WHATSAPP`
- `EMAIL`
- `WEB_CHAT`
- `WEBSITE_FORM`
- `MOBILE_APP`
- `VIDEO`
- `IN_PERSON`
- `FACEBOOK`
- `INSTAGRAM`
- `OTHER_SOCIAL_PLATFORM`
- `MARKETPLACE`
- `OEM_PLATFORM`
- `PARTNER_PLATFORM`
- `INTERNAL_SYSTEM`
- `POSTAL_MAIL`
- `OTHER`

### InteractionPurpose

- `GENERAL_INQUIRY`
- `LEAD_RESPONSE`
- `LEAD_FOLLOW_UP`
- `QUALIFICATION`
- `REQUIREMENT_DISCOVERY`
- `VEHICLE_INFORMATION`
- `VEHICLE_AVAILABILITY`
- `APPOINTMENT_SCHEDULING`
- `APPOINTMENT_CONFIRMATION`
- `APPOINTMENT_RESCHEDULING`
- `APPOINTMENT_CANCELLATION`
- `TEST_DRIVE_COORDINATION`
- `TEST_DRIVE_FOLLOW_UP`
- `TRADE_IN_DISCUSSION`
- `QUOTATION_REQUEST`
- `QUOTATION_PRESENTATION`
- `QUOTATION_FOLLOW_UP`
- `NEGOTIATION`
- `FINANCE_DISCUSSION`
- `FINANCE_DOCUMENT_REQUEST`
- `CONTRACT_DISCUSSION`
- `SIGNATURE_COORDINATION`
- `PAYMENT_FOLLOW_UP`
- `FUNDING_UPDATE`
- `DELIVERY_COORDINATION`
- `POST_SALE_FOLLOW_UP`
- `CUSTOMER_SUPPORT`
- `COMPLAINT_HANDLING`
- `DISPUTE_HANDLING`
- `OPT_OUT_PROCESSING`
- `INTERNAL_COORDINATION`
- `SYSTEM_ALERT`
- `OTHER`

### CommunicationCategory

- `TRANSACTIONAL`
- `SERVICE`
- `MARKETING`
- `SALES_FOLLOW_UP`
- `LEGAL_OR_REGULATORY`
- `SECURITY`
- `INTERNAL`
- `OTHER`

### InteractionPriority

- `LOW`
- `STANDARD`
- `HIGH`
- `URGENT`
- `CRITICAL`

Priority must not override Consent, authorization, privacy, safety, legal, or communication-policy controls.

### InteractionVisibility

- `CUSTOMER_VISIBLE`
- `INTERNAL`
- `RESTRICTED`
- `FINANCE_RESTRICTED`
- `LEGAL_RESTRICTED`
- `COMPLIANCE_RESTRICTED`
- `MANAGEMENT_RESTRICTED`
- `SECURITY_RESTRICTED`

### InteractionSource

- `MANUAL`
- `CRM`
- `DMS`
- `WEBSITE`
- `MOBILE_APP`
- `PHONE_PROVIDER`
- `MESSAGING_PROVIDER`
- `EMAIL_PROVIDER`
- `SOCIAL_PLATFORM`
- `MARKETPLACE`
- `OEM_PLATFORM`
- `PARTNER_PLATFORM`
- `CALENDAR`
- `AI_AGENT`
- `AUTOMATION`
- `IMPORT`
- `API_INTEGRATION`
- `OTHER`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `COMMUNICATION_PROVIDER_AUTHORITATIVE`
- `EXTERNAL_CRM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### InteractionParticipantRole

- `SENDER`
- `PRIMARY_RECIPIENT`
- `CC_RECIPIENT`
- `BCC_RECIPIENT`
- `CALLER`
- `CALLEE`
- `ATTENDEE`
- `AUTHOR`
- `OBSERVER`
- `INTERNAL_PARTICIPANT`
- `OTHER`

### InteractionParticipantType

- `CUSTOMER`
- `PROSPECT`
- `USER`
- `AI_AGENT`
- `SYSTEM`
- `LENDER`
- `COMMUNICATION_PROVIDER`
- `VENDOR`
- `OEM`
- `PARTNER`
- `LEGAL_REPRESENTATIVE`
- `AUTHORIZED_REPRESENTATIVE`
- `UNKNOWN_EXTERNAL_PARTY`

### ParticipantStatus

- `UNRESOLVED`
- `RESOLVED`
- `AUTHENTICATED`
- `UNAUTHENTICATED`
- `CONFLICTED`
- `RESTRICTED`
- `REMOVED`
- `DISPUTED`

### IdentityResolutionStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `CANDIDATES_FOUND`
- `PARTIALLY_RESOLVED`
- `RESOLVED`
- `AMBIGUOUS`
- `CONFLICTED`
- `REJECTED`
- `MANUAL_REVIEW_REQUIRED`

### AuthenticationStatus

- `NOT_REQUIRED`
- `NOT_AUTHENTICATED`
- `AUTHENTICATION_PENDING`
- `AUTHENTICATED`
- `AUTHENTICATION_FAILED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### ContentIntegrityStatus

- `NOT_EVALUATED`
- `VALID`
- `HASH_MISMATCH`
- `INCOMPLETE`
- `ALTERED`
- `CORRUPTED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### AttachmentProcessingStatus

- `NOT_REQUIRED`
- `PENDING`
- `PROCESSING`
- `COMPLETED`
- `PARTIALLY_COMPLETED`
- `FAILED`
- `BLOCKED`
- `QUARANTINED`
- `REVIEW_REQUIRED`

### MalwareScanStatus

- `NOT_REQUIRED`
- `NOT_SCANNED`
- `PENDING`
- `CLEAN`
- `SUSPICIOUS`
- `MALICIOUS`
- `SCAN_FAILED`
- `QUARANTINED`

### DataLossPreventionStatus

- `NOT_REQUIRED`
- `NOT_EVALUATED`
- `PENDING`
- `CLEARED`
- `RESTRICTED_DATA_DETECTED`
- `BLOCKED`
- `REVIEW_REQUIRED`

### SessionStatus

- `NOT_STARTED`
- `RINGING`
- `CONNECTING`
- `IN_PROGRESS`
- `ON_HOLD`
- `COMPLETED`
- `MISSED`
- `DECLINED`
- `FAILED`
- `CANCELLED`
- `DISCONNECTED`
- `DISPUTED`

### RecordingStatus

- `NOT_REQUIRED`
- `NOT_STARTED`
- `CONSENT_PENDING`
- `RECORDING`
- `STOPPED`
- `COMPLETED`
- `FAILED`
- `BLOCKED`
- `REDACTED`
- `DELETED_UNDER_POLICY`
- `LEGAL_HOLD`

### RecordingConsentStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `PENDING`
- `GRANTED`
- `DECLINED`
- `WITHDRAWN`
- `EXPIRED`
- `DISPUTED`
- `REVALIDATION_REQUIRED`

### TranscriptStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `QUEUED`
- `PROCESSING`
- `COMPLETED`
- `FAILED`
- `BLOCKED`
- `REVIEW_REQUIRED`
- `CORRECTED`
- `REDACTED`

### TranslationStatus

- `NOT_REQUIRED`
- `NOT_REQUESTED`
- `QUEUED`
- `PROCESSING`
- `COMPLETED`
- `FAILED`
- `REVIEW_REQUIRED`
- `CORRECTED`
- `REDACTED`

### CommunicationClassification

- `TRANSACTIONAL`
- `MARKETING`
- `SERVICE`
- `LEGAL`
- `SECURITY`
- `INTERNAL`
- `MIXED`
- `UNKNOWN`

### LawfulBasis

- `CONSENT`
- `CONTRACTUAL_NECESSITY`
- `LEGAL_OBLIGATION`
- `LEGITIMATE_INTEREST_WHERE_PERMITTED`
- `VITAL_INTEREST_WHERE_APPLICABLE`
- `OTHER_APPROVED_BASIS`
- `NOT_APPLICABLE`
- `UNKNOWN`

### ConsentRequirementStatus

- `NOT_EVALUATED`
- `NOT_REQUIRED`
- `REQUIRED`
- `SATISFIED`
- `NOT_SATISFIED`
- `WITHDRAWN`
- `EXPIRED`
- `DISPUTED`
- `REVALIDATION_REQUIRED`

### PermissionStatus

- `NOT_EVALUATED`
- `PERMITTED`
- `PERMITTED_WITH_RESTRICTIONS`
- `NOT_PERMITTED`
- `OPTED_OUT`
- `BLOCKED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### QuietHoursStatus

- `NOT_EVALUATED`
- `NOT_APPLICABLE`
- `PERMITTED_NOW`
- `DEFERRED`
- `BLOCKED`
- `OVERRIDE_APPROVAL_REQUIRED`

### FrequencyLimitStatus

- `NOT_EVALUATED`
- `NOT_APPLICABLE`
- `WITHIN_LIMIT`
- `LIMIT_APPROACHING`
- `LIMIT_REACHED`
- `BLOCKED`
- `OVERRIDE_APPROVAL_REQUIRED`

### OptOutSignalStatus

- `NOT_DETECTED`
- `POTENTIAL`
- `DETECTED`
- `CONFIRMED`
- `DISPUTED`
- `FALSE_POSITIVE`

### OptOutProcessingStatus

- `NOT_REQUIRED`
- `PENDING`
- `PROCESSING`
- `PENDING_CONFIRMATION`
- `COMPLETED`
- `FAILED`
- `DISPUTED`
- `RECONCILIATION_REQUIRED`

### DraftStatus

- `NOT_APPLICABLE`
- `CREATED`
- `AI_GENERATED`
- `HUMAN_AUTHORED`
- `REVIEW_PENDING`
- `APPROVED`
- `REJECTED`
- `EXPIRED`
- `SUPERSEDED`
- `CANCELLED`

### SendAuthorizationStatus

- `NOT_REQUIRED`
- `NOT_EVALUATED`
- `PENDING`
- `AUTHORIZED_BY_HUMAN`
- `AUTHORIZED_BY_AUTOMATION_POLICY`
- `REJECTED`
- `REVOKED`
- `EXPIRED`

### SendCommandStatus

- `NOT_REQUIRED`
- `NOT_CREATED`
- `CREATION_PENDING`
- `CREATED`
- `DISPATCH_PENDING`
- `DISPATCHED`
- `PENDING_PROVIDER_CONFIRMATION`
- `ACKNOWLEDGED_BY_PROVIDER`
- `FAILED`
- `CANCELLED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### InteractionDeliveryStatus

- `NOT_APPLICABLE`
- `NOT_SENT`
- `QUEUED`
- `PROVIDER_ACCEPTED`
- `PENDING_CONFIRMATION`
- `DELIVERED`
- `PARTIALLY_DELIVERED`
- `BOUNCED`
- `UNDELIVERABLE`
- `FAILED`
- `EXPIRED`
- `UNKNOWN`
- `RECONCILIATION_REQUIRED`

### ReadStatus

- `NOT_SUPPORTED`
- `NOT_READ`
- `PENDING_CONFIRMATION`
- `READ`
- `ACKNOWLEDGED`
- `UNKNOWN`
- `RECONCILIATION_REQUIRED`

### InteractionOutcomeStatus

- `NOT_EVALUATED`
- `PENDING`
- `RECORDED`
- `REVIEW_REQUIRED`
- `CONFIRMED`
- `DISPUTED`
- `SUPERSEDED`

### InteractionOutcome

- `INFORMATION_PROVIDED`
- `CUSTOMER_RESPONDED`
- `CUSTOMER_UNREACHABLE`
- `CALLBACK_REQUESTED`
- `APPOINTMENT_REQUESTED`
- `APPOINTMENT_CHANGE_REQUESTED`
- `APPOINTMENT_CANCELLATION_REQUESTED`
- `REQUIREMENTS_DISCOVERED`
- `VEHICLE_INTEREST_EXPRESSED`
- `QUOTATION_REQUESTED`
- `QUOTATION_PRESENTED`
- `POSSIBLE_OFFER_ACCEPTANCE`
- `POSSIBLE_OFFER_REJECTION`
- `TRADE_IN_INTEREST_EXPRESSED`
- `FINANCE_INTEREST_EXPRESSED`
- `DOCUMENTS_RECEIVED`
- `CONTRACT_QUESTION_RECEIVED`
- `PAYMENT_STATUS_DISCUSSION`
- `DELIVERY_UPDATE_PROVIDED`
- `COMPLAINT_SIGNAL_DETECTED`
- `COMPLAINT_OPENED`
- `OPT_OUT_REQUESTED`
- `FOLLOW_UP_REQUIRED`
- `ESCALATION_REQUIRED`
- `NO_FURTHER_ACTION`
- `OTHER`

Outcome values that contain `POSSIBLE` remain evidence signals and not authoritative downstream outcomes.

### ResponseSLAStatus

- `NOT_APPLICABLE`
- `PENDING`
- `WITHIN_SLA`
- `DUE_SOON`
- `BREACHED`
- `WAIVER_PENDING`
- `WAIVED`
- `COMPLETED`
- `DISPUTED`

### FollowUpStatus

- `NOT_REQUIRED`
- `PENDING`
- `SCHEDULED`
- `IN_PROGRESS`
- `COMPLETED`
- `CANCELLED`
- `OVERDUE`
- `ESCALATED`

### ComplaintSignalStatus

- `NOT_DETECTED`
- `POTENTIAL`
- `DETECTED`
- `CONFIRMED`
- `FALSE_POSITIVE`
- `DISPUTED`

### ContentModerationStatus

- `NOT_EVALUATED`
- `PENDING`
- `CLEARED`
- `RESTRICTED`
- `BLOCKED`
- `ESCALATED`
- `REVIEW_REQUIRED`

### PromptInjectionStatus

- `NOT_EVALUATED`
- `NOT_DETECTED`
- `POTENTIAL`
- `DETECTED`
- `BLOCKED`
- `REVIEW_REQUIRED`

### ComplianceReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `CLEARED`
- `CLEARED_WITH_RESTRICTIONS`
- `BLOCKED`
- `ESCALATED`
- `DISPUTED`

### RedactionStatus

- `NOT_REQUIRED`
- `REQUESTED`
- `REVIEW_PENDING`
- `APPROVED`
- `APPLIED`
- `REJECTED`
- `FAILED`
- `LEGAL_HOLD_BLOCKED`
- `RECONCILIATION_REQUIRED`

### CorrectionStatus

- `NOT_REQUIRED`
- `REQUESTED`
- `REVIEW_PENDING`
- `APPROVED`
- `CORRECTED_BY_SUPERSESSION`
- `REJECTED`
- `DISPUTED`

### AIProcessingStatus

- `NOT_REQUESTED`
- `QUEUED`
- `PROCESSING`
- `COMPLETED`
- `PARTIALLY_COMPLETED`
- `FAILED`
- `BLOCKED`
- `HUMAN_REVIEW_REQUIRED`
- `EXPIRED`

### InteractionIntent

- `UNKNOWN`
- `ASK_VEHICLE_PRICE`
- `ASK_VEHICLE_AVAILABILITY`
- `REQUEST_VEHICLE_DETAILS`
- `REQUEST_QUOTATION`
- `SCHEDULE_APPOINTMENT`
- `SCHEDULE_TEST_DRIVE`
- `CHANGE_APPOINTMENT`
- `CANCEL_APPOINTMENT`
- `DISCUSS_TRADE_IN`
- `APPLY_FOR_FINANCE`
- `PROVIDE_DOCUMENTS`
- `NEGOTIATE_PRICE`
- `POSSIBLE_ACCEPTANCE`
- `POSSIBLE_REJECTION`
- `REQUEST_CALLBACK`
- `REQUEST_PAYMENT_UPDATE`
- `REQUEST_DELIVERY_UPDATE`
- `REPORT_COMPLAINT`
- `REQUEST_SUPPORT`
- `OPT_OUT`
- `OTHER`

### InteractionSentiment

- `UNKNOWN`
- `VERY_NEGATIVE`
- `NEGATIVE`
- `NEUTRAL`
- `POSITIVE`
- `VERY_POSITIVE`
- `MIXED`

### InteractionUrgency

- `UNKNOWN`
- `LOW`
- `NORMAL`
- `HIGH`
- `URGENT`
- `CRITICAL`

### SignalStatus

- `NOT_DETECTED`
- `POTENTIAL`
- `DETECTED`
- `CONFIRMED_BY_HUMAN`
- `CONFIRMED_BY_WORKFLOW`
- `FALSE_POSITIVE`
- `DISPUTED`
- `EXPIRED`

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

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

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Every related Domain Object must belong to the authorized Tenant.
- Dealership, branch, department, queue, team, User, and Agent must belong to the permitted organizational scope.
- Cross-Tenant Interaction access, search, threading, identity matching, AI retrieval, export, and communication execution are prohibited unless governed by an approved mechanism.
- Background Jobs, Event Consumers, connectors, and AI Agents must receive trusted Tenant execution context.

### Interaction Creation Rules

Every Interaction must contain:

- Tenant context.
- Interaction type.
- Direction.
- Channel.
- Source.
- Occurrence time.
- Participant context.
- Content, session evidence, attachment, structured system content, or internal-note content.
- Visibility.
- Security classification.
- Audit actor.
- Idempotency or provider deduplication controls where applicable.

An Interaction must not be created without meaningful evidence or a valid outbound Draft purpose.

### Inbound Ingestion Rules

Inbound provider ingestion requires:

- Authenticated or otherwise trusted provider source.
- Provider account.
- Provider event or message reference where supported.
- Provider deduplication key.
- Source timestamp.
- Original payload reference.
- Original payload hash.
- Normalization outcome.
- Tenant routing.
- Security scanning.
- Audit evidence.

An inbound provider event may be delivered more than once.

The integration must prevent duplicate business effects using provider-specific deduplication identifiers.

Provider deduplication must not be confused with ASOS Event Consumer deduplication using `event_id`.

### Outbound Creation Rules

An outbound Interaction must not be sent unless:

- Authorized sender exists.
- Intended recipients are identified.
- Recipient contact points are valid for the purpose.
- Communication purpose is known.
- Transactional or marketing classification is known.
- Required Consent or lawful basis exists.
- Channel permission is valid.
- Quiet-hours policy passes.
- Frequency-limit policy passes.
- Customer restrictions are respected.
- Content is finalized.
- Customer-visible content hash exists.
- Required Human Approval or automation policy exists.
- Communication provider is approved.
- Send Command uses an `idempotency_key`.
- No security or compliance block exists.

### Outbound Content Freeze Rules

Once a send Command is created:

- Customer-visible content must be immutable.
- Recipient set must be immutable for that Command.
- Template version must be preserved.
- Content hash must be preserved.
- Permission snapshot must be preserved.
- Approval or automation-policy reference must be preserved.

A material change requires:

- Cancellation where possible.
- A new outbound Interaction or version.
- A new authorization evaluation.
- A new idempotency key.
- Preserved history.

### Provider Authority Rules

Provider authority may establish:

- Technical acceptance.
- Queueing.
- Delivery.
- Bounce.
- Read status where supported.
- Call connection.
- Call duration.
- Provider failure.

Provider authority does not establish:

- Customer identity.
- Customer understanding.
- Customer legal acceptance.
- Payment.
- Appointment completion.
- Contract signature unless the provider is the configured signature authority.
- Deal completion.

### Delivery Rules

- `SENT` requires provider acceptance or equivalent authoritative send evidence.
- `DELIVERED` requires provider delivery evidence where supported.
- `READ` requires provider read or acknowledgement evidence where supported.
- A channel that does not support read Confirmation must remain `NOT_SUPPORTED` or `UNKNOWN`.
- The platform must not infer read status solely from lack of response.
- Delivery timestamps must follow valid chronological order.
- A bounce or delivery failure must preserve provider evidence.
- Retry policy must remain configurable.
- Retry must reuse the same Command idempotency intent or create a governed new attempt according to provider requirements.

### Identity-Resolution Rules

- External contact values are unverified observations until governed resolution.
- A phone number, email, social account, or display name must not be used as the sole permanent Customer identifier.
- Identity candidates must belong to the same Tenant.
- Sensitive data must not be disclosed before sufficient authentication and identity verification.
- Ambiguous identity matches require Human Review or deterministic evidence.
- AI confidence alone must not merge a participant into a Customer.
- Customer resolution must preserve evidence and Decision.
- Identity corrections must not rewrite original sender information.

### Participant Rules

- Every Interaction must have at least one participant.
- Outbound Customer communication requires at least one recipient.
- Inbound communication requires a sender or trusted external source.
- Internal notes require one authenticated Human author.
- AI-generated messages must identify the responsible Agent and authorizing workflow.
- Participant roles must remain compatible with direction.
- A participant must not be added to finalized original evidence without a correction annotation.
- BCC recipients must receive restricted visibility.
- External participant details must remain protected.

### Threading Rules

- All Interactions in a Conversation Thread must belong to the same Tenant.
- Thread participants must have a valid continuity relationship.
- `parent_interaction_id`, `reply_to_interaction_id`, and `root_interaction_id` must belong to the same thread.
- An Interaction cannot reply to itself.
- Circular reply relationships are prohibited.
- Circular supersession relationships are prohibited.
- Sequence numbers must be unique within the canonical thread where used.
- Provider sequence and canonical sequence must remain distinguishable.
- Thread merging requires governed identity and participant review.
- Thread splitting must preserve original history.

### Content Rules

- At least one applicable content or session evidence field must exist.
- Normalized content must not replace original evidence.
- Customer-visible content must exclude internal notes.
- Restricted financial, identity, legal, compliance, margin, funding, or security information must not appear in unrestricted outbound content.
- HTML and rich content must be sanitized.
- Links must be validated according to security policy.
- Content hashes must use an approved cryptographic algorithm.
- Finalized content must be immutable.
- Unsupported characters or encoding failures must not silently alter meaning.
- The original language must be preserved.
- A summary must not introduce unsupported facts.

### Internal Note Rules

- Internal notes require an authenticated authorized author.
- Internal notes require a business purpose.
- Internal notes must use `INTERNAL` or a more restrictive visibility.
- Internal notes must not be copied into Customer-visible messages automatically.
- Internal notes must not impersonate the Customer.
- Internal notes must distinguish observation from interpretation.
- AI-generated internal summaries must be labelled as Derived Intelligence.
- Finalized internal notes must be corrected through supersession or annotation.

### Attachment Rules

Every attachment must:

- Use controlled storage.
- Preserve a content hash.
- Preserve MIME type and size.
- Be scanned for malware.
- Be evaluated for restricted data.
- Be classified.
- Be blocked or quarantined when required.
- Be treated as untrusted input.
- Be excluded from public indexing.
- Be excluded from unrestricted AI context.
- Preserve retention and legal-hold requirements.

An attachment received through an Interaction does not make the document authoritative.

The responsible document or Domain workflow must verify and accept it.

### Recording Rules

- Recording requirements must be determined before recording begins.
- Valid recording Consent must exist where required.
- Consent status must be participant-specific where applicable.
- Recording must stop when required by withdrawal or policy.
- Recording source, time, participants, and hash must be preserved.
- Public or predictable recording URLs are prohibited.
- Recording access must be logged.
- Recording retention must follow law and policy.
- AI must not create recording Consent.
- A transcript must not replace the original recording.

### Transcript Rules

- Transcript output is Derived Intelligence or approved provider output.
- Transcript source recording must be preserved.
- Transcript model or provider must be recorded.
- Transcript version must be recorded.
- Speaker attribution uncertainty must remain explicit.
- Corrections must preserve prior versions.
- Material contractual or acceptance interpretation must not rely solely on an unreviewed transcript.
- Failed transcription must not block canonical Interaction storage.
- Restricted content in transcripts must receive the same or stronger classification as the recording.

### Translation Rules

- Original content and source language must remain available.
- Translation must identify provider or model.
- Translation must identify target language.
- Translation must not replace original legal or contractual content.
- Customer-facing translated legal, finance, or contractual material requires approved translation controls.
- Material uncertainty must be escalated.
- Translation corrections must preserve history.

### Consent and Permission Rules

Before outbound communication, deterministic policy must evaluate:

- Customer or recipient identity.
- Contact point.
- Purpose.
- Communication category.
- Channel.
- Jurisdiction.
- Consent.
- Lawful basis.
- Opt-out state.
- Quiet hours.
- Frequency.
- Customer restrictions.
- Template.
- Required disclosure.
- Age or representation restrictions where applicable.
- Human Approval or approved automation policy.

Prompt content, Agent Recommendation, sales priority, or User interface state must not override these controls.

### Marketing and Transactional Separation

- Marketing and transactional communication must remain distinguishable.
- Transactional purpose must not be used as a pretext for marketing.
- Mixed-purpose content must follow the stricter applicable policy.
- Marketing Consent must not be inferred from a purchase inquiry.
- Finance Consent must not be reused as marketing Consent.
- An inbound conversation does not create indefinite outbound permission.

### Opt-Out Rules

A potential opt-out signal must:

- Be recorded.
- Preserve original evidence.
- Trigger deterministic processing.
- Prevent prohibited outbound communication while pending where required.
- Update the appropriate Consent or permission authority.
- Preserve channel and purpose scope.
- Use idempotency.
- Produce External Confirmation where applicable.
- Create Human Review when ambiguous.

AI may detect a possible opt-out.

AI must not suppress or dismiss an opt-out request independently.

### Acceptance and Commitment Rules

AI or Human interpretation of communication must distinguish:

- Interest.
- Question.
- Negotiation.
- Conditional language.
- Possible acceptance.
- Confirmed acceptance.
- Binding acceptance.

An Interaction must not independently confirm:

- Quotation acceptance.
- Trade-In acceptance.
- Finance offer selection.
- Financial Contract signature.
- Deal commitment.
- Payment settlement.
- Delivery acceptance.

Authoritative acceptance requires the responsible Domain workflow and accepted evidence.

### Outcome Rules

- Outcomes must describe what the Interaction established, not unsupported downstream completion.
- `POSSIBLE_OFFER_ACCEPTANCE` must not update Quotation to accepted.
- `APPOINTMENT_REQUESTED` must not create an Appointment without the Appointment workflow.
- `DOCUMENTS_RECEIVED` must not mark documents verified.
- `PAYMENT_STATUS_DISCUSSION` must not mark Payment received.
- `DELIVERY_UPDATE_PROVIDED` must not mark delivery completed.
- Outcome authority and evidence must be recorded.
- Disputed outcomes must remain explicit.

### SLA Rules

- SLA calculations must use authoritative server time.
- SLA policy and version must be preserved.
- `response_due_at` is required when a response deadline applies.
- Inbound and outbound response relationships must be explicit.
- A provider auto-acknowledgement must not satisfy a Human-response SLA unless policy permits it.
- Waivers require an authorized Decision.
- SLA breach must not rewrite the original deadline.
- SLA changes require policy versioning.

### Follow-Up Rules

- Follow-up Recommendations remain separate from authoritative tasks.
- A required follow-up must identify reason, due time, owner or queue, and status.
- Duplicate follow-up tasks must be prevented.
- Completion must reference the satisfying Interaction or action.
- AI may recommend follow-up but must not claim completion.
- Customer opt-out and communication restrictions override ordinary sales follow-up.

### Complaint and Dispute Rules

- Complaint language must not be dismissed solely by sentiment classification.
- A confirmed complaint signal must create or update the responsible complaint workflow.
- Original evidence must be preserved.
- Complaint closure must not be represented by the Interaction alone.
- Legal holds must override deletion and ordinary retention.
- Dispute evidence access must remain restricted.
- AI may summarize but must not resolve a complaint or legal dispute.

### Correction Rules

Original evidence must not be materially edited in place.

A correction must preserve:

- Original Interaction.
- Corrected or superseding Interaction.
- Reason.
- Actor.
- Authority.
- Timestamp.
- Evidence.
- Original and revised hashes.
- Impacted downstream references.
- Audit record.

Provider-originated corrections must preserve both provider records where possible.

### Redaction Rules

Redaction may occur only through approved policy.

Redaction must preserve:

- Redaction request.
- Scope.
- Legal basis.
- Decision.
- Original secure evidence where lawful and required.
- Redacted representation.
- Hashes.
- Actor.
- Timestamp.
- Downstream propagation status.
- Legal-hold evaluation.

Redaction must not be used to conceal unauthorized communication or alter commercial history.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Manual Interaction creation must support idempotency where retry is possible.
- Outbound send Commands must use `idempotency_key`.
- Provider ingestion must use provider-specific deduplication keys.
- Opt-out processing must support idempotency.
- Redaction and correction requests must support idempotency.
- Event Consumers must prevent duplicate ASOS Event effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Interactions.
  - Provider messages.
  - Send Commands.
  - Delivery records.
  - Conversation Thread entries.
  - Opt-out records.
  - Follow-up tasks.
  - Complaint cases.
  - Redactions.
  - Corrections.

### AI Failure Rules

- AI processing failure must not block canonical inbound ingestion.
- AI processing failure must not delete content.
- AI processing failure must remain explicit.
- Outbound AI-generated content must not be sent when required review or policy checks fail.
- Missing AI output must not be replaced with fabricated values.
- Human workflows must remain available.

### Human Review Requirements

Human Review is required according to policy for:

- Ambiguous Customer identity.
- Conflicting participants.
- Possible sensitive-data disclosure.
- Possible opt-out.
- Complaint or legal-threat signal.
- Possible contractual acceptance.
- Finance or Payment instruction.
- Restricted document.
- Malware or prompt injection.
- Recording-Consent dispute.
- Transcript dispute.
- Translation uncertainty affecting a material outcome.
- AI-generated regulated content.
- Unauthorized commitment.
- Delivery or provider conflict.
- Correction or redaction.
- Legal hold.
- Another material privacy, legal, financial, or commercial risk.

---

## 6. State Machine

### Allowed States

```text
CREATED
DRAFT
APPROVAL_PENDING
READY_TO_SEND
SEND_PENDING
QUEUED
SENT
DELIVERED
READ
RECEIVED
IN_PROGRESS
COMPLETED
FAILED
CANCELLED
REDACTED
SUPERSEDED
ARCHIVED
```

Not every state applies to every direction or Interaction type.

### Principal Outbound Transitions

```text
CREATED → DRAFT
CREATED → CANCELLED

DRAFT → APPROVAL_PENDING
DRAFT → READY_TO_SEND
DRAFT → CANCELLED
DRAFT → SUPERSEDED

APPROVAL_PENDING → READY_TO_SEND
APPROVAL_PENDING → DRAFT
APPROVAL_PENDING → CANCELLED
APPROVAL_PENDING → SUPERSEDED

READY_TO_SEND → SEND_PENDING
READY_TO_SEND → DRAFT
READY_TO_SEND → CANCELLED
READY_TO_SEND → SUPERSEDED

SEND_PENDING → QUEUED
SEND_PENDING → SENT
SEND_PENDING → FAILED
SEND_PENDING → CANCELLED

QUEUED → SENT
QUEUED → FAILED
QUEUED → CANCELLED

SENT → DELIVERED
SENT → READ
SENT → COMPLETED
SENT → FAILED

DELIVERED → READ
DELIVERED → COMPLETED
DELIVERED → FAILED

READ → COMPLETED

FAILED → READY_TO_SEND
FAILED → CANCELLED
FAILED → SUPERSEDED

COMPLETED → REDACTED
COMPLETED → SUPERSEDED
COMPLETED → ARCHIVED

CANCELLED → ARCHIVED
REDACTED → ARCHIVED
SUPERSEDED → ARCHIVED
```

A retry from `FAILED` must follow provider and idempotency rules.

### Principal Inbound Transitions

```text
CREATED → RECEIVED

RECEIVED → IN_PROGRESS
RECEIVED → COMPLETED
RECEIVED → FAILED
RECEIVED → REDACTED

IN_PROGRESS → COMPLETED
IN_PROGRESS → FAILED
IN_PROGRESS → CANCELLED

FAILED → RECEIVED
FAILED → COMPLETED

COMPLETED → REDACTED
COMPLETED → SUPERSEDED
COMPLETED → ARCHIVED

REDACTED → ARCHIVED
SUPERSEDED → ARCHIVED
```

`FAILED → RECEIVED` is permitted only for successful reconciliation of previously incomplete provider ingestion.

### Principal Synchronous Session Transitions

```text
CREATED → IN_PROGRESS
IN_PROGRESS → COMPLETED
IN_PROGRESS → FAILED
IN_PROGRESS → CANCELLED
COMPLETED → REDACTED
COMPLETED → ARCHIVED
```

### Internal Note Transitions

```text
CREATED → DRAFT
DRAFT → COMPLETED
DRAFT → CANCELLED
COMPLETED → SUPERSEDED
COMPLETED → REDACTED
COMPLETED → ARCHIVED
```

### Forbidden Ordinary Transitions

```text
DRAFT → DELIVERED
DRAFT → READ
DRAFT → COMPLETED_AS_OUTBOUND_DELIVERY

APPROVAL_PENDING → SENT
APPROVAL_PENDING → DELIVERED

READY_TO_SEND → DELIVERED
READY_TO_SEND → READ

SEND_PENDING → DELIVERED
QUEUED → READ

SENT → RECEIVED
DELIVERED → RECEIVED
READ → RECEIVED

RECEIVED → DRAFT
RECEIVED → SENT

COMPLETED → DRAFT
COMPLETED → SEND_PENDING
COMPLETED → RECEIVED

CANCELLED → SENT
CANCELLED → DELIVERED

REDACTED → DRAFT
REDACTED → SENT

SUPERSEDED → SENT
ARCHIVED → DRAFT
ARCHIVED → SENT
```

Corrections require a governed correction, superseding Interaction, or redaction workflow.

### Entering CREATED

Requires:

- Valid Tenant context.
- Interaction classification.
- Direction.
- Source.
- Occurrence or intended communication context.
- Initial participants.
- Creation authority.
- Audit evidence.
- Idempotency or deduplication control where applicable.

### Entering DRAFT

Requires:

- Outbound or internal Interaction.
- Author or Agent.
- Draft content.
- Purpose.
- Intended recipients for outbound communication.
- Visibility.
- Draft hash.
- No claim of delivery.

### Entering APPROVAL_PENDING

Requires:

- Final proposed content.
- Recipient set.
- Purpose.
- Communication-policy evaluation.
- Reason approval is required.
- Assigned authorized approver.
- Frozen approval snapshot.

### Entering READY_TO_SEND

Requires:

- Final Customer-visible content.
- Final content hash.
- Valid recipients.
- Valid permission and policy checks.
- Required Human Approval or automation policy.
- Approved provider.
- No blocking security or compliance issue.

### Entering SEND_PENDING

Requires:

- Valid send authorization.
- Outbound Command.
- `idempotency_key`.
- Current content hash.
- Current recipient snapshot.
- Audit evidence.

### Entering QUEUED

Requires:

- Provider or internal queue evidence.
- Command reference.
- No delivery claim.

### Entering SENT

Requires:

- Provider acceptance or equivalent authoritative send evidence.
- Provider message reference where supported.
- `sent_at`.
- No claim of Customer receipt.

### Entering DELIVERED

Requires:

- Authoritative provider delivery evidence.
- Recipient scope.
- Delivery timestamp.
- Delivery Confirmation reference.

### Entering READ

Requires:

- Provider-supported authoritative read or acknowledgement evidence.
- Read timestamp.
- No claim of understanding or acceptance.

### Entering RECEIVED

Requires:

- Trusted inbound source.
- Original evidence.
- Provider or source deduplication.
- Occurrence and receipt timestamps.
- Participant information.
- Security processing state.

### Entering IN_PROGRESS

Requires:

- Active call, meeting, live chat session, or synchronous conversation.
- Session reference.
- Start timestamp.
- Active participants.
- Recording controls where applicable.

### Entering COMPLETED

Requires applicable:

- Final content or session evidence.
- Finalized content hash.
- End or completion timestamp.
- Provider or Human evidence.
- Outcome evaluation.
- Required follow-up evaluation.
- Required SLA update.
- No unresolved ingestion failure.

Completion does not prove a downstream Domain outcome.

### Entering FAILED

Requires:

- Failure reason.
- Failure stage.
- Provider or system evidence.
- Retry eligibility.
- Security and reconciliation evaluation.
- Failure timestamp.

### Entering CANCELLED

Requires:

- Valid cancellation reason.
- Authorized actor or policy.
- Confirmation that prohibited or pending send activity is stopped where possible.
- Audit evidence.

Cancellation after provider acceptance may not be possible and must not falsify external status.

### Entering REDACTED

Requires:

- Approved redaction Decision.
- Redaction scope.
- Legal-hold evaluation.
- Preserved original evidence where lawful.
- Redacted representation.
- Redaction timestamp.
- Audit evidence.

### Entering SUPERSEDED

Requires:

- Valid replacement or correction Interaction.
- Supersession reason.
- Replacement reference.
- Preserved original evidence.
- No circular lineage.

### Terminal States

For ordinary processing:

- `COMPLETED`
- `CANCELLED`
- `REDACTED`
- `SUPERSEDED`
- `ARCHIVED`

Provider status reconciliation may append new evidence without altering finalized original content.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Interaction version.
- Content hash.
- Participant snapshot.
- Applied Business Rules.
- Consent and permission snapshot.
- Human Decision.
- Automation-policy reference where applicable.
- Command.
- Provider reference.
- External Confirmation.
- Record version.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.

---

## 7. Relationships

### Tenant

- Every Interaction belongs to exactly one `tenant_id`.
- All relationships must remain inside authorized Tenant scope.
- Cross-Tenant communication processing requires an approved and auditable legal and technical mechanism.

### Customer

- An Interaction may exist before Customer resolution.
- After accepted resolution, `customer_id` references the canonical Customer.
- Original participant observations must remain preserved.
- Interaction must not rewrite Customer identity or Consent.

### Lead

- An Interaction may create evidence for Lead creation.
- Lead intake may reference the originating Interaction.
- Interaction must not silently create or qualify a Lead without the governed workflow.

### Qualified Lead

- Qualification evidence may reference one or more Interactions.
- Interaction intent or sentiment alone must not create authoritative qualification.

### Opportunity

- Interactions may support requirement discovery, negotiation, follow-up, and commitment evidence.
- Opportunity owns commercial pursuit state.
- Interaction must not independently close the Opportunity as won or lost.

### Appointment

- Interactions may request, discuss, confirm, reschedule, or cancel Appointment activity.
- Appointment owns scheduling and attendance.
- Interaction delivery does not prove Appointment confirmation.

### Quotation

- Interactions may request, present, discuss, or collect responses to a Quotation.
- Exact Quotation and version must be referenced.
- Quotation owns authoritative acceptance and decline.

### Vehicle

- Interactions may reference Vehicle interest or questions.
- Vehicle owns canonical identity and specification.
- Submitted vehicle text remains unverified until resolved.

### Inventory Record

- Interactions may discuss availability.
- Inventory Record owns authoritative availability, Reservation, Allocation, sale, and delivery projections.
- Customer-facing availability statements must use current authoritative information.

### Trade-In

- Interactions may collect submitted Trade-In information and Customer expectations.
- Trade-In owns appraisal, ownership, payoff, offer, and acquisition.
- Interaction must not present AI-extracted value as approved appraisal.

### Finance Application

- Interactions may support document requests, status communication, and offer presentation.
- Finance Application owns Applicant, Consent, Lender submission, and Decision workflows.
- Sensitive finance content must be minimized.

### Financial Contract

- Interactions may deliver Contract documents or coordinate signatures.
- Financial Contract owns Contract version, disclosure, signature, effectiveness, and activation.
- Message delivery does not prove Contract signature.

### Deal

- Interactions may support Deal communication, Payment follow-up, delivery coordination, cancellation request, or dispute evidence.
- Deal owns authoritative transaction state.

### Conversation Thread

A Conversation Thread groups related Interactions.

It must preserve:

- Tenant.
- Participants.
- External thread references.
- Canonical sequence.
- Root Interaction.
- Current status.
- Security classification.
- Last activity.
- Merge and split history.

### Communication Provider

Provider relationships may establish:

- Provider account.
- Message identifier.
- Conversation identifier.
- Call identifier.
- Delivery status.
- Read status.
- Provider errors.
- Provider timestamps.

Provider data must remain normalized and source-traceable.

### Task and Follow-Up

An Interaction may create or satisfy a follow-up Task.

Task state must remain separate from Interaction state.

### Campaign

Campaign communication may reference campaign and audience context.

Campaign association does not bypass individual Consent and permission checks.

### Complaint and Dispute

Complaint and dispute workflows may reference multiple Interactions as evidence.

Interaction must not independently close the complaint or dispute.

### Human Decision

Interactions may reference Decisions for:

- Outbound approval.
- Identity resolution.
- Acceptance interpretation.
- Permission exception.
- Recording exception.
- Redaction.
- Correction.
- Complaint escalation.
- Legal hold.

### AI Agent Run

AI Agent Runs may reference Interactions as input.

Each run must preserve:

- Approved purpose.
- Tenant scope.
- Input fields.
- Model and Prompt versions.
- Output.
- Decision authority.
- Tool use.
- Audit evidence.

### Supporting Child Records

Interaction may own or govern:

- Participant records.
- Thread relationships.
- Provider records.
- Provider events.
- Delivery records.
- Attachments.
- Recordings.
- Transcripts.
- Translations.
- Permission snapshots.
- Draft versions.
- Send Commands.
- SLA records.
- Outcomes.
- Follow-up references.
- Redaction records.
- Correction records.
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

The following are required Interaction Event concepts and do not replace the Event Catalog.

### Inbound Event Concepts

- Inbound Interaction observed.
- Inbound Interaction recorded.
- Inbound provider duplicate detected.
- Inbound Interaction normalization completed.
- Inbound Interaction normalization failed.
- Inbound attachment quarantined.
- Inbound Interaction identity resolution requested.
- Inbound Interaction Customer resolved.
- Inbound Interaction identity conflict detected.

### Draft and Approval Event Concepts

- Outbound Interaction Draft created.
- Outbound Interaction Draft updated.
- Outbound Interaction Draft reviewed.
- Outbound Interaction approval requested.
- Outbound Interaction authorized.
- Outbound Interaction rejected.
- Outbound Interaction authorization expired.
- Outbound Interaction superseded.

### Policy Event Concepts

- Communication permission evaluated.
- Communication permission denied.
- Quiet-hours deferral applied.
- Frequency limit reached.
- Communication policy revalidation required.
- Customer restriction applied.

### Send and Provider Event Concepts

- Outbound Interaction send requested.
- Outbound Interaction Command created.
- Outbound Interaction Command dispatched.
- Communication provider accepted message.
- Communication provider rejected message.
- Communication provider reported delivery.
- Communication provider reported read status.
- Communication provider reported bounce.
- Communication provider reported failure.
- Communication provider reconciliation required.

### Session Event Concepts

- Interaction session started.
- Interaction session answered.
- Interaction session placed on hold.
- Interaction session resumed.
- Interaction session ended.
- Interaction session missed.
- Interaction session failed.

### Recording and Transcript Event Concepts

- Recording Consent requested.
- Recording Consent granted.
- Recording Consent declined.
- Recording started.
- Recording stopped.
- Recording completed.
- Recording failed.
- Transcript requested.
- Transcript generated.
- Transcript failed.
- Transcript corrected.
- Translation generated.
- Translation corrected.

### Content and Attachment Event Concepts

- Interaction content finalized.
- Interaction attachment received.
- Interaction attachment scan completed.
- Restricted data detected.
- Malicious attachment detected.
- Prompt injection detected.
- Interaction content restricted.
- Interaction security review requested.

### Outcome and Follow-Up Event Concepts

- Interaction outcome recorded.
- Interaction response required.
- Interaction response SLA started.
- Interaction response SLA breached.
- Interaction response completed.
- Interaction follow-up required.
- Interaction follow-up completed.
- Interaction escalation requested.
- Interaction escalation resolved.

### Opt-Out Event Concepts

- Possible opt-out detected.
- Opt-out processing requested.
- Opt-out Command created.
- Opt-out confirmed.
- Opt-out processing failed.
- Opt-out reconciliation required.

### Complaint and Dispute Event Concepts

- Complaint signal detected.
- Complaint workflow requested.
- Interaction dispute opened.
- Interaction legal hold applied.
- Interaction legal hold released.

### Correction and Redaction Event Concepts

- Interaction correction requested.
- Interaction corrected by supersession.
- Interaction redaction requested.
- Interaction redaction approved.
- Interaction redacted.
- Interaction redaction failed.

### Derived Intelligence Event Concepts

- Interaction intent analysis generated.
- Interaction sentiment analysis generated.
- Interaction urgency analysis generated.
- Interaction acceptance signal detected.
- Interaction commitment signal detected.
- Interaction complaint signal detected.
- Interaction summary generated.
- Interaction next action recommended.
- Interaction Human Review recommended.

Derived Intelligence Events must not imply:

- Customer identity resolution.
- Consent.
- Appointment confirmation.
- Quotation acceptance.
- Finance approval.
- Contract signature.
- Payment.
- Delivery.
- Deal completion.
- Complaint resolution.
- Human Approval.
- External completion.

### Producer Rules

- Interaction Domain Service publishes accepted Interaction canonical and workflow-state changes.
- Customer Domain Service publishes accepted Customer identity and permission facts.
- Communication integrations publish normalized provider observations.
- Appointment, Quotation, Trade-In, Finance Application, Financial Contract, and Deal Domain Services publish their authoritative outcomes.
- Document and media services publish accepted processing facts.
- AI Agents may publish Agent-run, extraction, classification, summarization, or Recommendation Events.
- AI Agents must not publish authoritative identity, Consent, acceptance, Payment, delivery, or business-completion Events merely because they predicted or detected the outcome.

### Event Requirements

Every material Interaction Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `interaction_id`.
- `conversation_thread_id`.
- Customer and related Domain references.
- Interaction direction.
- Channel.
- Purpose.
- Content hash.
- Participant snapshot reference.
- Provider references.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Consent and permission snapshot.
- Human Decision.
- Automation-policy reference where applicable.
- Command.
- External Confirmation.
- Evidence references.
- Security classification.

Events are immutable.

Corrections, redactions, provider reconciliation, opt-out correction, and supersession must use new Events linked to prior Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate ASOS Event effects using `event_id`.

Provider-event deduplication must use the appropriate provider identifiers and remains a separate concern.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Language detection.
- Transcription.
- Translation.
- Content summarization.
- Intent classification.
- Sentiment analysis.
- Urgency analysis.
- Topic classification.
- Entity extraction.
- Vehicle-interest extraction.
- Appointment-signal detection.
- Quotation-signal detection.
- Trade-In-signal detection.
- Finance-signal detection.
- Commitment-signal detection.
- Possible acceptance detection.
- Possible rejection detection.
- Opt-out detection.
- Complaint detection.
- Sensitive-data detection.
- Prompt-injection detection.
- Response drafting.
- Follow-up Recommendation.
- SLA prioritization.
- Thread summarization.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Resolve Customer identity from ambiguous evidence.
- Merge Customers.
- Create Consent.
- Ignore or reverse an opt-out.
- Decide lawful basis.
- Bypass quiet hours or frequency limits.
- Send outbound content without required authority.
- Create authoritative Appointment confirmation.
- Confirm Quotation acceptance.
- Confirm Trade-In acceptance.
- Submit a Finance Application.
- Select a Lender offer for the Customer.
- Sign or confirm a Financial Contract.
- Confirm Payment.
- Confirm funding.
- Confirm delivery.
- Complete a Deal.
- Resolve a complaint or legal dispute.
- Delete or redact evidence.
- Remove a legal hold.
- Execute communication-provider Commands directly.
- Access Interactions outside authorized Tenant and purpose scope.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Interaction identifier.
- Interaction record version.
- Content or evidence hash.
- Input fields.
- Supporting evidence.
- Source authority.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Alternative interpretations where material.
- Assumptions.
- Limitations.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority.

### Evidence Grounding

AI outputs must reference the evidence supporting them.

AI must distinguish:

- Direct quotation or observed content.
- Normalized content.
- Extracted entity.
- Interpretation.
- Prediction.
- Recommendation.
- Authoritative fact.

AI summaries must not introduce unsupported:

- Prices.
- Dates.
- Customer commitments.
- Finance results.
- Payment results.
- Delivery results.
- Legal conclusions.

### Intent and Sentiment

Intent and sentiment are Derived Intelligence.

They must not be used as sole authority for:

- Customer identity.
- Qualification.
- Commercial acceptance.
- Adverse treatment.
- Complaint closure.
- Contact permission.
- Finance Decision.
- Delivery outcome.

Sentiment must not be treated as a reliable measure of Customer truthfulness or creditworthiness.

### Acceptance and Commitment Detection

AI may identify possible acceptance or commitment language.

The output must preserve:

- Exact evidence span.
- Communication language.
- Context.
- Conditions stated by the Customer.
- Ambiguity.
- Model version.
- Confidence where meaningful.
- Required downstream workflow.

AI must distinguish:

```text
“I like the offer”
  = positive reaction

“I may proceed if finance is approved”
  = conditional interest

“I accept quotation version 4”
  = possible explicit acceptance evidence

Authoritative acceptance
  = governed Quotation workflow outcome
```

### Opt-Out Detection

AI may detect phrases indicating:

- Stop contacting me.
- Do not send promotions.
- Remove me from the list.
- Unsubscribe.
- Do not call this number.

Potential opt-outs must be processed conservatively.

AI must not:

- Downgrade a possible opt-out to ordinary negative sentiment.
- Delay required policy enforcement.
- Limit the scope without evidence.
- Re-enable communication.

### Complaint Detection

AI may identify complaint, threat, escalation, or regulatory language.

Complaint detection must:

- Preserve evidence.
- Avoid dismissing indirect or polite complaints.
- Support Human Review.
- Respect legal-hold rules.
- Avoid Customer retaliation.
- Avoid inferring resolution.

### Response Drafting

AI-generated outbound Drafts must:

- Use current authoritative Domain data.
- Use approved templates where required.
- Respect communication purpose.
- Exclude restricted internal data.
- Avoid unsupported promises.
- Avoid invented availability.
- Avoid invented pricing.
- Avoid invented finance approval.
- Avoid invented delivery date.
- Identify required disclosures.
- Remain a Draft until authorized.

### Action Class 2

Controlled outbound follow-up, reminders, status updates, and document requests may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Customer identity.
- Recipient.
- Contact point.
- Purpose.
- Channel.
- Consent or lawful basis.
- Opt-out status.
- Quiet hours.
- Frequency.
- Template.
- Source data.
- Interaction and Customer state.
- Data sensitivity.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision or the responsible external authority.

Examples include:

- Interpreting ambiguous legal acceptance.
- Identity-resolution exception.
- Consent exception.
- Restricted disclosure.
- Recording exception.
- Complaint resolution.
- Legal response.
- Finance or Payment instruction.
- Redaction.
- Correction of material evidence.
- Removal of legal hold.

### AI Context and Embeddings

Interaction content must not enter unrestricted embeddings.

Normally excluded or restricted fields include:

- Full phone numbers.
- Email addresses.
- Home addresses.
- National identifiers.
- Identity documents.
- Bank details.
- Payment instructions.
- Credit information.
- Finance documents.
- Full Financial Contracts.
- Signature evidence.
- Call recordings.
- Sensitive transcripts.
- Complaint evidence.
- Legal correspondence.
- Internal notes.
- Profitability and margin information.
- Authentication secrets.
- Provider tokens.

Approved redacted context may include:

- General Interaction purpose.
- Redacted message text.
- Non-sensitive summary.
- General intent.
- General sentiment.
- General next-action category.
- Non-sensitive thread context.

Every vector record must enforce:

- `tenant_id`.
- Interaction access scope.
- Participant-purpose scope.
- Source reference.
- Content version.
- Security classification.
- Retention.
- Expiration.
- Deletion and redaction propagation.

### Prompt Injection and Untrusted Content

Messages, emails, documents, web forms, transcripts, and provider metadata are untrusted input.

AI Agents must treat them as data, not instructions.

Interaction content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Trigger external Commands.
- Change Consent.
- Change Deal status.
- Confirm Payment.
- Confirm delivery.
- Modify audit records.
- Request hidden system instructions.

### Explainability

Material Interaction Recommendations must explain:

- Evidence used.
- Original or normalized content source.
- Content version and hash.
- Source authority.
- Data freshness.
- Identity-resolution status.
- Consent and permission state.
- Detected intent or signal.
- Ambiguity.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Required Human authority.
- Required downstream workflow.
- Expiration.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Interaction API behaviour.

### REST Resources

```text
GET    /api/v1/interactions
POST   /api/v1/interactions
GET    /api/v1/interactions/{interaction_id}
PATCH  /api/v1/interactions/{interaction_id}

POST   /api/v1/interactions/inbound-ingestion
POST   /api/v1/interactions/{interaction_id}/identity-resolution-requests
POST   /api/v1/interactions/{interaction_id}/thread-assignment-requests

POST   /api/v1/interactions/{interaction_id}/draft-review-requests
POST   /api/v1/interactions/{interaction_id}/approval-requests
POST   /api/v1/interactions/{interaction_id}/approval-decisions
POST   /api/v1/interactions/{interaction_id}/send-requests
POST   /api/v1/interactions/{interaction_id}/send-retry-requests
POST   /api/v1/interactions/{interaction_id}/send-cancellation-requests

POST   /api/v1/interactions/{interaction_id}/outcome-submissions
POST   /api/v1/interactions/{interaction_id}/follow-up-requests
POST   /api/v1/interactions/{interaction_id}/escalation-requests
POST   /api/v1/interactions/{interaction_id}/opt-out-processing-requests

POST   /api/v1/interactions/{interaction_id}/transcription-requests
POST   /api/v1/interactions/{interaction_id}/translation-requests
POST   /api/v1/interactions/{interaction_id}/ai-analysis-requests

POST   /api/v1/interactions/{interaction_id}/correction-requests
POST   /api/v1/interactions/{interaction_id}/redaction-requests
POST   /api/v1/interactions/{interaction_id}/archive-requests

GET    /api/v1/interactions/{interaction_id}/participants
GET    /api/v1/interactions/{interaction_id}/attachments
GET    /api/v1/interactions/{interaction_id}/provider-history
GET    /api/v1/interactions/{interaction_id}/delivery-status
GET    /api/v1/interactions/{interaction_id}/ai-analysis
GET    /api/v1/interactions/{interaction_id}/history
GET    /api/v1/interactions/{interaction_id}/reconciliation

GET    /api/v1/conversation-threads
GET    /api/v1/conversation-threads/{conversation_thread_id}
GET    /api/v1/conversation-threads/{conversation_thread_id}/interactions
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, queue, User, and Agent scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Manual Inbound Interaction

```json
{
  "interaction_type": "IN_PERSON_CONVERSATION",
  "direction": "INBOUND",
  "channel": "IN_PERSON",
  "purpose": "VEHICLE_INFORMATION",
  "visibility": "INTERNAL",
  "occurred_at": "2026-08-01T19:15:00+03:00",
  "participants": [
    {
      "participant_role": "SENDER",
      "participant_type": "CUSTOMER",
      "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b"
    },
    {
      "participant_role": "PRIMARY_RECIPIENT",
      "participant_type": "USER",
      "user_id": "d64ed10d-ded1-454e-b57e-cfa92ce0ee0c"
    }
  ],
  "internal_note_text": "Customer requested specifications for the selected vehicle.",
  "related_objects": {
    "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
    "vehicle_id": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

A retryable request must include:

```text
Idempotency-Key: 819fc894-39ec-4ee3-bdb7-31f6b91fb6cf
```

### Example Inbound Provider Ingestion

```json
{
  "provider_id": "f4629990-aeaa-4b34-8bf1-17cb85addb42",
  "provider_account_id": "whatsapp-account-01",
  "provider_message_id": "wamid.HBgMNT...",
  "provider_event_id": "provider-event-912887",
  "provider_conversation_id": "provider-conversation-771",
  "channel": "WHATSAPP",
  "occurred_at": "2026-08-01T19:20:00+03:00",
  "sender": {
    "provider_participant_id": "201001234567",
    "submitted_contact_value": "+201001234567",
    "display_name": "Submitted Customer Name"
  },
  "content": {
    "content_type": "TEXT",
    "text": "هل العربية متاحة للتجربة غدًا؟",
    "language_code": "ar-EG"
  },
  "original_payload_reference": "evidence://provider-events/provider-event-912887",
  "original_payload_hash": "sha256:5d4d371f..."
}
```

The provider event must be authenticated and deduplicated using the provider-specific identifiers.

### Example Inbound Response

```json
{
  "interaction_id": "47af5c31-2443-4cba-b107-bdc7aeb1558e",
  "interaction_number": "INT-2026-004829",
  "direction": "INBOUND",
  "channel": "WHATSAPP",
  "status": "RECEIVED",
  "customer_identity_resolution_status": "CANDIDATES_FOUND",
  "provider_message_id": "wamid.HBgMNT...",
  "provider_message_deduplication_status": "CURRENT",
  "content_language_code": "ar-EG",
  "content_hash": "sha256:f6b672a1...",
  "ai_processing_status": "QUEUED",
  "record_version": 1,
  "recorded_at": "2026-08-01T19:20:02+03:00"
}
```

### Example Outbound Draft Request

```json
{
  "interaction_type": "TEXT_MESSAGE",
  "direction": "OUTBOUND",
  "channel": "WHATSAPP",
  "purpose": "APPOINTMENT_SCHEDULING",
  "communication_category": "TRANSACTIONAL",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "recipient_contact_point_reference": "contact-point://customers/a2d85b86/whatsapp/primary",
  "customer_visible_text": "يمكننا ترتيب موعد لتجربة السيارة غدًا. يرجى اختيار الوقت المناسب.",
  "content_language_code": "ar-EG",
  "related_objects": {
    "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
    "vehicle_id": "550e8400-e29b-41d4-a716-446655440000"
  }
}
```

### Example Draft Response

```json
{
  "interaction_id": "c5c8aec3-4ab5-47e8-9d58-d6126f20432c",
  "status": "DRAFT",
  "draft_status": "HUMAN_AUTHORED",
  "customer_visible_content_hash": "sha256:6f005408...",
  "contact_permission_status": "NOT_EVALUATED",
  "send_authorization_status": "NOT_EVALUATED",
  "send_command_status": "NOT_CREATED",
  "record_version": 1
}
```

### Example Send Request

```json
{
  "expected_content_hash": "sha256:6f005408...",
  "expected_record_version": 4,
  "send_authorization_decision_id": "a4a74a4d-f14d-43ec-a89b-14d9bb557aea"
}
```

The request must include an idempotency key.

A pending response may be:

```json
{
  "interaction_id": "c5c8aec3-4ab5-47e8-9d58-d6126f20432c",
  "status": "SEND_PENDING",
  "send_command_status": "PENDING_PROVIDER_CONFIRMATION",
  "command_id": "77e9aeae-b646-4786-bf18-d060f3bb1bc7",
  "delivery_status": "NOT_SENT",
  "record_version": 5
}
```

The response must not describe the message as sent or delivered.

### Example Provider-Accepted Projection

```json
{
  "interaction_id": "c5c8aec3-4ab5-47e8-9d58-d6126f20432c",
  "status": "SENT",
  "send_command_status": "ACKNOWLEDGED_BY_PROVIDER",
  "provider_message_id": "wamid.HBgMNj...",
  "delivery_status": "PROVIDER_ACCEPTED",
  "sent_at": "2026-08-01T19:25:10+03:00",
  "record_version": 6
}
```

### Example Delivery Projection

```json
{
  "interaction_id": "c5c8aec3-4ab5-47e8-9d58-d6126f20432c",
  "status": "DELIVERED",
  "delivery_status": "DELIVERED",
  "delivery_confirmation_status": "RECEIVED",
  "delivered_at": "2026-08-01T19:25:16+03:00",
  "read_status": "NOT_READ",
  "record_version": 7
}
```

Delivery does not prove Customer acceptance.

### Example AI Analysis Response

```json
{
  "interaction_id": "47af5c31-2443-4cba-b107-bdc7aeb1558e",
  "ai_processing_status": "COMPLETED",
  "intent_classification": "SCHEDULE_TEST_DRIVE",
  "intent_scores": {
    "SCHEDULE_TEST_DRIVE": 0.94
  },
  "sentiment_classification": "NEUTRAL",
  "urgency_classification": "NORMAL",
  "appointment_signal": "DETECTED",
  "recommended_next_action": "CREATE_APPOINTMENT_PROPOSAL",
  "requires_human_review": false,
  "model_reference": "interaction-intelligence-model",
  "model_version": "2026-08",
  "prompt_version": "interaction-analysis-1.1",
  "generated_at": "2026-08-01T19:20:08+03:00"
}
```

This response is Derived Intelligence.

It does not create an Appointment.

### Example Opt-Out Processing Request

```json
{
  "opt_out_signal_type": "EXPLICIT_TEXT_REQUEST",
  "evidence_reference": "evidence://interactions/47af5c31/content",
  "requested_scope": {
    "channel": "WHATSAPP",
    "communication_category": "MARKETING"
  },
  "expected_record_version": 3
}
```

The operation must use an idempotency key.

### Example Correction Request

```json
{
  "correction_reason": "The internal note attributed the statement to the wrong participant.",
  "corrected_internal_note_text": "The accompanying representative, not the Customer, requested the callback.",
  "expected_record_version": 6
}
```

A correction must create a governed replacement or correction record.

It must not overwrite the finalized original evidence.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Direction and channel validation.
- Participant validation.
- Identity-resolution controls.
- Content integrity.
- Visibility.
- Communication permission.
- Consent and lawful-basis controls.
- Quiet-hours and frequency controls.
- Content safety.
- Human Approval or approved automation policy.
- Lifecycle validation.
- Idempotency or provider deduplication where applicable.
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

Retryable ASOS operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate:

- Interactions.
- Outbound Commands.
- Provider send attempts.
- Opt-out requests.
- Follow-up tasks.
- Redaction requests.
- Correction requests.

Provider ingestion deduplication must use provider-specific identifiers.

### Pending External Confirmation

Operations requiring provider Confirmation may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "interaction_id": "c5c8aec3-4ab5-47e8-9d58-d6126f20432c",
  "command_id": "77e9aeae-b646-4786-bf18-d060f3bb1bc7",
  "record_version": 5
}
```

The API must not describe the external operation as complete until authoritative evidence exists.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_PROVIDER_EVENT`
- `DUPLICATE_INTERACTION`
- `PROVIDER_AUTHENTICATION_FAILED`
- `PROVIDER_CONFIGURATION_INVALID`
- `PARTICIPANT_REQUIRED`
- `PARTICIPANT_IDENTITY_AMBIGUOUS`
- `CUSTOMER_IDENTITY_REQUIRED`
- `RECIPIENT_REQUIRED`
- `CONTACT_POINT_INVALID`
- `CONTACT_PERMISSION_NOT_GRANTED`
- `CONSENT_REQUIRED`
- `CONSENT_WITHDRAWN`
- `OPT_OUT_ACTIVE`
- `QUIET_HOURS_BLOCK`
- `FREQUENCY_LIMIT_REACHED`
- `COMMUNICATION_PURPOSE_INVALID`
- `CONTENT_REQUIRED`
- `CONTENT_IMMUTABLE`
- `CONTENT_HASH_MISMATCH`
- `INTERNAL_CONTENT_EXPOSURE_BLOCKED`
- `RESTRICTED_DATA_DETECTED`
- `ATTACHMENT_QUARANTINED`
- `MALWARE_DETECTED`
- `PROMPT_INJECTION_DETECTED`
- `RECORDING_CONSENT_REQUIRED`
- `SEND_AUTHORIZATION_REQUIRED`
- `SEND_COMMAND_DUPLICATE`
- `PROVIDER_CONFIRMATION_PENDING`
- `DELIVERY_NOT_CONFIRMED`
- `INVALID_LIFECYCLE_TRANSITION`
- `HUMAN_REVIEW_REQUIRED`
- `CORRECTION_REQUIRED`
- `REDACTION_NOT_PERMITTED`
- `LEGAL_HOLD_ACTIVE`
- `RECONCILIATION_REQUIRED`
- `INTERACTION_TERMINAL`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Participant-purpose scope.
- Visibility.
- Content integrity.
- Consent and permission.
- Communication policy.
- Provider authority.
- Concurrency.
- Idempotency.
- Provider deduplication.
- Human Approval.
- External Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Interaction Domain Service, Consent controls, Policy Engine, Command Orchestration, or provider integration controls.

---

## 11. Database Design

### Recommended Tables

```text
interactions
interaction_versions
interaction_participants
interaction_thread_links
conversation_threads
conversation_thread_participants
interaction_content_evidence
interaction_customer_visible_content
interaction_internal_notes
interaction_attachments
interaction_media
interaction_sessions
interaction_recordings
interaction_recording_consents
interaction_transcripts
interaction_transcript_versions
interaction_translations
interaction_permission_snapshots
interaction_opt_out_signals
interaction_outbound_drafts
interaction_approval_requests
interaction_approval_decisions
interaction_send_commands
interaction_send_attempts
interaction_provider_messages
interaction_provider_events
interaction_delivery_events
interaction_outcomes
interaction_response_sla
interaction_follow_up_references
interaction_escalations
interaction_complaint_signals
interaction_redactions
interaction_corrections
interaction_external_confirmations
interaction_derived_intelligence
interaction_ai_runs
interaction_reconciliation_cases
interaction_data_quality_issues
interaction_status_history
interaction_record_versions
interaction_audit_log
```

### Interactions Table

The `interactions` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Related Domain references.
- Direction.
- Channel.
- Interaction type.
- Purpose.
- Visibility.
- Current lifecycle state.
- Conversation Thread reference.
- Current participant projection.
- Current content references and hashes.
- Current permission projection.
- Current provider projection.
- Current delivery and read projection.
- Current SLA and follow-up projection.
- Current outcome projection.
- Current security, redaction, correction, and legal-hold projection.
- Data-quality and conflict state.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Repeating, historical, sensitive, and binary information must remain in child or controlled-storage records.

### Primary Key

```text
PRIMARY KEY (interaction_id)
```

### Tenant Protection

Every Interaction-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_interactions_tenant_status
  (tenant_id, status)

idx_interactions_tenant_customer
  (tenant_id, customer_id, occurred_at)

idx_interactions_tenant_lead
  (tenant_id, lead_id, occurred_at)

idx_interactions_tenant_opportunity
  (tenant_id, opportunity_id, occurred_at)

idx_interactions_tenant_deal
  (tenant_id, deal_id, occurred_at)

idx_interactions_tenant_thread
  (tenant_id, conversation_thread_id, sequence_number)

idx_interactions_tenant_channel
  (tenant_id, channel, occurred_at)

idx_interactions_tenant_direction
  (tenant_id, direction, occurred_at)

idx_interactions_tenant_assigned_user
  (tenant_id, assigned_user_id, response_sla_status)

idx_interactions_response_due
  (tenant_id, response_sla_status, response_due_at)

idx_interactions_follow_up
  (tenant_id, follow_up_status, authoritative_follow_up_at)

idx_interactions_provider_message
  (tenant_id, provider_id, provider_account_id, provider_message_id)

idx_interactions_delivery
  (tenant_id, delivery_status, sent_at)

idx_interactions_opt_out
  (tenant_id, opt_out_processing_status)

idx_interactions_review
  (tenant_id, review_status)

idx_interactions_legal_hold
  (tenant_id, legal_hold_status)

idx_interactions_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, interaction_number)
```

Provider message uniqueness should normally use:

```text
UNIQUE (
  tenant_id,
  provider_id,
  provider_account_id,
  provider_message_id
)
```

where the provider guarantees message-identifier uniqueness.

Provider event uniqueness should use:

```text
UNIQUE (
  tenant_id,
  provider_id,
  provider_account_id,
  provider_event_id
)
```

Send-Command idempotency should use:

```text
UNIQUE (
  tenant_id,
  send_idempotency_key
)
```

where appropriate to the Command scope.

### Interaction Versions

`interaction_versions` should preserve:

- Interaction.
- Version.
- Content references.
- Content hashes.
- Participant snapshot.
- Visibility.
- Reason.
- Creation authority.
- Creation timestamp.
- Superseded version.
- Related Events.

Original inbound evidence and finalized outbound content must remain immutable.

### Participant Storage

`interaction_participants` should preserve:

- Participant identifier.
- Interaction.
- Role.
- Type.
- Customer, User, or Agent reference.
- External participant reference.
- Contact-point token.
- Submitted contact observation.
- Identity-resolution state.
- Authentication state.
- Authority state.
- Visibility.
- Evidence.
- Related Events.

### Conversation Thread Storage

`conversation_threads` should preserve:

- Thread identifier.
- Tenant.
- Thread status.
- Primary Customer projection.
- Participant snapshot.
- Channel set.
- External references.
- Root Interaction.
- Last Interaction.
- Security classification.
- Merge and split history.
- Created and updated timestamps.

Thread merging must remain auditable.

### Content Evidence Storage

`interaction_content_evidence` should preserve:

- Original evidence reference.
- Original hash.
- Source.
- MIME type.
- Encoding.
- Normalized representation.
- Normalized hash.
- Language.
- Integrity state.
- Security classification.
- Retention class.
- Legal hold.
- Related Events.

Binary and large source content should remain in controlled evidence storage.

### Internal Note Storage

Internal notes should be stored separately from Customer-visible content.

Storage must preserve:

- Author.
- Business purpose.
- Visibility.
- Text.
- Hash.
- Finalization.
- Correction lineage.
- Access history.
- Related Events.

### Attachment Storage

Attachment records should preserve:

- Interaction.
- Document or media reference.
- File metadata.
- Hash.
- Scan state.
- DLP state.
- Content moderation.
- Prompt-injection state.
- Security classification.
- Retention.
- Legal hold.
- Related Events.

### Provider Storage

Provider-message and provider-event tables should preserve:

- Provider.
- Account.
- Channel.
- Message identifier.
- Event identifier.
- Conversation identifier.
- Provider status.
- Provider timestamps.
- Original payload reference.
- Payload hash.
- Deduplication state.
- Reconciliation state.
- Related Events.

Provider payloads must not be stored in unrestricted Logs.

### Send Command Storage

`interaction_send_commands` should preserve:

- Interaction.
- Content hash.
- Recipient snapshot.
- Permission snapshot.
- Human Decision or automation policy.
- Command.
- Idempotency key.
- Provider.
- Requested time.
- Dispatch time.
- Status.
- Failure.
- Retry state.
- External Confirmation.
- Related Events.

### Delivery Storage

Delivery-event storage should preserve:

- Interaction.
- Provider message.
- Provider event.
- Recipient.
- Status.
- Timestamp.
- Source.
- Evidence.
- Deduplication status.
- Reconciliation status.
- Related Events.

Delivery history must be append-only.

### Recording Storage

Recording records should preserve:

- Interaction.
- Provider.
- Session.
- Participant Consent.
- Start and end times.
- Controlled media reference.
- Hash.
- Duration.
- Security classification.
- Retention.
- Legal hold.
- Access state.
- Related Events.

### Transcript and Translation Storage

Transcript and translation versions must preserve:

- Source.
- Model or provider.
- Version.
- Language.
- Text reference.
- Hash.
- Generated time.
- Review.
- Correction lineage.
- Security classification.
- Related Events.

### Permission Snapshot Storage

`interaction_permission_snapshots` should preserve:

- Customer or recipient.
- Contact point.
- Purpose.
- Communication category.
- Channel.
- Consent records.
- Lawful basis.
- Opt-out state.
- Quiet-hours result.
- Frequency result.
- Policy and version.
- Evaluation time.
- Expiration.
- Result.
- Related Events.

### SLA Storage

SLA records should preserve:

- Interaction.
- Policy and version.
- Start time.
- Due time.
- Satisfying response.
- Completion time.
- Breach time.
- Waiver Decision.
- Status.
- Related Events.

### Redaction and Correction Storage

Redaction and correction records should preserve:

- Request.
- Interaction.
- Scope.
- Reason.
- Legal basis.
- Human Decision.
- Original hash.
- Revised or redacted hash.
- Secure evidence.
- Actor.
- Timestamp.
- Propagation state.
- Related Events.

### Derived Intelligence

Derived Interaction records must remain separate from original evidence and authoritative workflow fields.

Each derived record should preserve:

- Output type.
- Value.
- Evidence spans.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Confidence.
- Assumptions.
- Generated time.
- Expiration time.
- Review status.

### Audit Storage

Interaction audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw communication, identity, financial, and legal values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Dealership.
- Channel.
- Occurrence date.
- Retention class.
- Security classification.
- Legal-hold status.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Provider deduplication.
- Thread ordering.
- Content immutability.
- Legal holds.
- Referential integrity.
- Audit integrity.

### Hard Deletion

An Interaction must not be hard-deleted when referenced by:

- Customer Journey.
- Lead.
- Qualified Lead.
- Opportunity.
- Appointment.
- Quotation.
- Trade-In.
- Finance Application.
- Financial Contract.
- Deal.
- Complaint.
- Dispute.
- Follow-up Task.
- Human Decision.
- AI Agent Run.
- Command.
- External Confirmation.
- Legal hold.
- Audit evidence.

Redaction, anonymization, governed deletion, archival, or legally required retention workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Name, phone, email, social account |
| `CUSTOMER_COMMUNICATION` | Customer-visible message content |
| `INTERNAL_RESTRICTED` | Internal notes and internal coordination |
| `FINANCIAL_RESTRICTED` | Finance, Payment, bank, and funding content |
| `IDENTITY_RESTRICTED` | Identity documents and national identifiers |
| `CONTRACTUAL_RESTRICTED` | Contract and signature discussion |
| `LEGAL_AND_COMPLIANCE` | Complaints, disputes, legal holds |
| `RECORDING_RESTRICTED` | Call and meeting recordings |
| `PROVIDER_CONFIDENTIAL` | Provider identifiers and payloads |
| `DERIVED_INTELLIGENCE` | Intent, sentiment, summaries, Recommendations |
| `AUDIT_EVIDENCE` | Decisions, Commands, Confirmations, and history |

### Authentication

Every internal Interaction operation requires an authenticated Human or service identity.

Customer self-service access must use an approved authentication or verification mechanism.

Anonymous inbound communication may be accepted only into a restricted unresolved-identity workflow.

Anonymous communication must not receive sensitive Customer information.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Department.
- Team.
- Queue.
- Assigned User.
- Customer relationship.
- Related Domain Object.
- Participant relationship.
- Direction.
- Channel.
- Purpose.
- Visibility.
- Requested field.
- Requested action.
- Security classification.
- Legal hold.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access permitted:

- Assigned Customer communication.
- Vehicle and Appointment discussion.
- Customer-facing Quotation communication.
- Approved follow-up.
- General Deal coordination.

Must not access without explicit authority:

- Restricted finance documents.
- Credit information.
- Bank details.
- Internal legal notes.
- Compliance investigations.
- Full recordings unrelated to assigned work.
- Other Users’ restricted notes.

#### Sales Manager

May access permitted team Interactions and perform configured:

- Escalation review.
- Outbound approval.
- Quality review.
- Commitment review.
- Complaint escalation.
- Follow-up reassignment.

Manager access does not automatically grant access to legal, finance, identity, or compliance-restricted content.

#### Finance Specialist

May access only finance-related Interactions required for assigned Finance Applications, Financial Contracts, or funding workflows.

#### Compliance or Legal Reviewer

May access restricted Interactions required for an assigned review, complaint, dispute, or legal hold.

Access must remain purpose-limited and audited.

#### Contact-Center User

May access assigned queues and permitted Customer contact data.

Contact-center access does not grant access to unrestricted Deal margin, finance documents, or legal evidence.

#### Quality Reviewer

May access applicable Interactions, recordings, transcripts, and quality metadata according to assigned purpose and policy.

#### Data Steward

May review:

- Identity-resolution conflicts.
- Duplicate provider records.
- Thread conflicts.
- Source mappings.
- Data-quality issues.
- Reconciliation cases.

Raw sensitive content access should remain minimized.

#### AI Agent

May access only the minimum Interaction context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to identity, finance, Payment, Contract, legal, compliance, recording, and internal-note data.

#### Integration Service

May access only fields required for an approved communication, recording, transcription, translation, document, CRM, or workflow integration.

### Field-Level Protection

Restricted fields must use:

- Field-level authorization.
- Encryption.
- Tokenization where appropriate.
- Masking.
- Controlled evidence references.
- Export restrictions.
- AI-context restrictions.
- Purpose limitation.
- Audit logging.

Restricted examples include:

- Phone numbers.
- Email addresses.
- Social account identifiers.
- Recording references.
- Transcript content.
- Internal notes.
- Identity documents.
- Payment instructions.
- Finance documents.
- Contract content.
- Complaint evidence.
- Provider credentials.

### Contact-Point Protection

Contact points must:

- Use tokenization or equivalent protection where possible.
- Be masked when full display is unnecessary.
- Be restricted by purpose.
- Be excluded from general Logs.
- Be excluded from unrestricted embeddings.
- Be protected against bulk export.
- Preserve verification state.
- Preserve source.
- Not be treated as permanent Customer identity.

### Communication Content Protection

Communication content must:

- Use encryption in transit and at rest.
- Preserve content hashes.
- Use controlled access.
- Prevent public indexing.
- Prevent unrestricted sharing.
- Follow retention.
- Support legal hold.
- Support governed redaction.
- Prevent Customer-visible exposure of internal notes.
- Prevent cross-Customer content leakage.

### Provider Credential Protection

Provider credentials, API keys, tokens, signing secrets, and webhook secrets must:

- Remain in approved secret-management systems.
- Never appear in Interaction records.
- Never appear in Prompts.
- Never appear in ordinary Logs.
- Use rotation.
- Use least privilege.
- Use audited service identities.

### Webhook and Provider Security

Provider webhooks must use applicable:

- Signature verification.
- Timestamp validation.
- Replay protection.
- Source validation.
- Tenant routing.
- Provider-account validation.
- Payload-size limits.
- Schema validation.
- Deduplication.
- Rate limiting.
- Security logging.

A provider event must not choose its own unrestricted `tenant_id`.

Tenant routing must derive from trusted provider-account configuration.

### Attachment Security

Attachments must:

- Be treated as untrusted.
- Use controlled storage.
- Be scanned for malware.
- Be inspected for restricted data.
- Be protected from public access.
- Use non-predictable references.
- Preserve hashes.
- Prevent active-content execution.
- Prevent unapproved AI training.
- Follow retention and legal-hold policy.

### Recording Security

Recordings must:

- Use controlled encrypted storage.
- Preserve recording Consent.
- Restrict playback and download.
- Preserve access logs.
- Protect Customer and employee privacy.
- Prevent public links.
- Follow jurisdictional retention.
- Support legal hold.
- Support governed deletion or redaction where lawful.
- Be excluded from unrestricted AI context.

### Transcript and Translation Security

Transcripts and translations inherit the security classification of the source content unless a stricter classification applies.

They must not be placed in lower-security storage solely because they are text.

### Consent Enforcement

Before outbound communication, deterministic controls must validate:

- Recipient identity.
- Contact point.
- Purpose.
- Communication classification.
- Channel.
- Consent.
- Lawful basis.
- Opt-out state.
- Quiet hours.
- Frequency.
- Template.
- Customer restrictions.
- Human Approval or automation policy.

Prompt text, sales urgency, or AI Recommendation must not bypass these controls.

### Internal Note Security

Internal notes must:

- Use restricted visibility.
- Remain excluded from Customer delivery.
- Remain excluded from Customer exports unless legally required.
- Prevent copy into outbound messages without explicit authorized review.
- Preserve author and purpose.
- Be subject to professional conduct and anti-discrimination rules.
- Remain discoverable under lawful audit or legal requirements.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Contact-point matching.
- Identity resolution.
- Conversation threading.
- Search.
- Vector retrieval.
- Provider events.
- Queues.
- Caches.
- Attachments.
- Recordings.
- Transcripts.
- Documents.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Command Security

Outbound communication Commands must include:

- Authenticated service identity.
- `tenant_id`.
- Organizational scope.
- Interaction identifier.
- Current record version.
- Content hash.
- Recipient snapshot.
- Purpose.
- Permission snapshot.
- Human Decision or automation-policy reference.
- Provider.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit provider Commands directly.

### Audit Requirements

Material Interaction activity must record:

- `tenant_id`.
- `interaction_id`.
- Conversation Thread.
- Customer and related Domain references.
- Actor.
- Role and permission.
- Direction.
- Channel.
- Purpose.
- Business purpose.
- Content hash.
- Participant snapshot reference.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Record version.
- Consent and permission snapshot.
- Human Decision.
- Automation-policy reference.
- AI involvement.
- Recommendation.
- Command.
- Idempotency key.
- Provider reference.
- External Confirmation.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Events.

### Security Events

ASOS must detect and record:

- Cross-Tenant Interaction access attempts.
- Provider webhook authentication failure.
- Provider event replay.
- Provider-account Tenant-routing mismatch.
- Duplicate provider ingestion anomalies.
- Unauthorized Customer disclosure.
- Internal-note exposure attempt.
- Consent bypass.
- Opt-out bypass.
- Quiet-hours override attempt.
- Frequency-limit bypass.
- Unauthorized outbound communication.
- Message-content substitution.
- Content-hash mismatch.
- Attachment malware.
- Restricted-data exposure.
- Recording without required Consent.
- Unauthorized recording access.
- Prompt injection.
- AI access outside approved scope.
- Bulk communication export.
- Command replay.
- External Confirmation mismatch.
- Audit-log tampering.

### Communication Integrity

The platform must detect or prevent:

- Sending to the wrong Customer.
- Sending through the wrong Tenant account.
- Sending the wrong content version.
- Sending after approval expiration.
- Sending after opt-out.
- Sending internal notes to Customers.
- Recording without required Consent.
- Treating delivery as acceptance.
- Treating AI interpretation as authoritative outcome.
- Altering finalized inbound evidence.
- Altering finalized outbound content.
- Concealing corrections or redactions.
- Duplicate provider-message creation.
- Interaction status manipulation.

### Privacy and Retention

Interaction retention must follow:

- Applicable law.
- Tenant policy.
- Communication-provider requirements.
- Customer privacy rights.
- Consent state.
- Contractual requirements.
- Complaint and dispute requirements.
- Financial and regulatory obligations.
- Legal holds.
- Audit requirements.

Privacy workflows must support applicable:

- Access.
- Correction.
- Restriction.
- Export.
- Consent withdrawal.
- Redaction.
- Anonymization.
- Deletion where lawful.
- Dispute handling.

Privacy actions must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Document stores.
- Media stores.
- Communication providers where lawfully required.
- Backups according to policy.

Required contractual, security, complaint, legal, financial, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Outbound messaging.
- Automated follow-up.
- Marketing communication.
- Provider webhooks.
- Recording.
- Transcription.
- Translation.
- AI Interaction analysis.
- Attachment processing.
- External write-back.
- Interaction export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Lead](./Lead.md)
- [ASOS Qualified Lead](./QualifiedLead.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Financial Contract](./FinancialContract.md)
- [ASOS Deal](./Deal.md)

---

## Current Status

This document is the approved Canonical Interaction baseline.

Interaction preserves communication evidence and does not independently create authoritative downstream business outcomes.

Original inbound evidence and finalized outbound content are immutable.

Internal notes remain separate from Customer-visible communication.

Inbound communication does not automatically establish Customer identity or marketing Consent.

Outbound communication requires deterministic permission controls and Human Approval or an applicable pre-approved automation policy.

Provider acceptance does not prove delivery.

Delivery does not prove Customer understanding or acceptance.

AI intent, sentiment, summaries, and acceptance signals remain Derived Intelligence.

Provider-event deduplication, ASOS Event Consumer deduplication using `event_id`, and Command retry protection using `idempotency_key` remain separate controls.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
