# Lead

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Lead Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Lead Object represents one captured expression of potential automotive commercial interest, inquiry, referral, response, or requested dealership engagement.

A Lead may originate from:

- Website forms.
- Landing pages.
- OEM platforms.
- Automotive marketplaces.
- Social-media platforms.
- Paid advertising.
- Messaging channels.
- Email.
- Phone calls.
- Showroom walk-ins.
- Events.
- Referrals.
- Campaign imports.
- Approved partner systems.
- Existing Customer re-engagement.
- Manual governed entry.
- Another approved source.

The Lead is the canonical intake record for the original inquiry.

It preserves:

- Original source evidence.
- Submitted party information.
- Submitted contact information.
- Inquiry content.
- Source and campaign attribution.
- Intake timestamps.
- Source deduplication.
- Initial Customer-resolution context.
- Initial contact-path validation.
- Initial communication-permission projection.
- Qualification evidence collection.
- Qualification-request workflow.
- Pending qualification state.
- Negative qualification outcomes.
- Response and assignment projections.
- Data-quality state.
- Human Review state.
- Derived Intelligence.
- Audit evidence.

### Lead Is Not a Customer

A Lead is an inquiry or expression of interest.

A Customer represents a resolved canonical individual or organization identity and dealership relationship.

A Lead may initially belong to:

- An unidentified person.
- A partially identified person.
- An existing Customer.
- A new prospective Customer.
- An organization.
- A fleet buyer.
- A representative acting for another party.
- An unknown external participant.

Customer identity must be governed by Customer Domain Service.

A Lead must not create, merge, or correct Customer identity merely because:

- A name matches.
- A phone number matches.
- An email address matches.
- An AI model predicts a match.
- A source claims the party is an existing Customer.

### Lead, Qualified Lead, and Opportunity Separation

The commercial journey must preserve the following ownership boundary:

```text
Lead
  = original inquiry, intake evidence,
    qualification request, evidence collection,
    pending qualification workflow,
    and negative intake outcomes

Qualified Lead
  = accepted positive qualification Decision,
    qualification snapshot, validity,
    revalidation, revocation, invalidation,
    and Opportunity-conversion eligibility

Opportunity
  = active commercial pursuit,
    sales ownership, pipeline state,
    requirement evolution, negotiation,
    and Deal conversion
```

A Lead must not be silently changed into a Qualified Lead.

A Qualified Lead must be created as a separate Canonical Domain Object through a governed and idempotent creation workflow.

The original Lead remains historically traceable after:

- Positive qualification.
- Qualified Lead creation.
- Opportunity creation.
- Deal creation.
- Disqualification.
- Invalidation.
- Withdrawal.
- Expiration.
- Archival.

### Qualification Ownership Boundary

The Lead Domain Service owns or coordinates:

- Qualification request initiation.
- Qualification evidence collection.
- Missing-information collection.
- Qualification-readiness evaluation.
- Qualification pending state.
- Qualification Human Review requests.
- Qualification Decision request.
- Positive-Decision handoff preparation.
- Qualified Lead creation request.
- Qualified Lead creation Confirmation projection.
- Negative qualification outcome.
- Disqualification.
- Invalidity.
- Duplicate classification.
- Withdrawal.
- Expiration before positive qualification.
- Qualification request cancellation.
- Intake-related fraud or spam handling.

The Lead Domain Service does not own:

- The canonical positive qualification result.
- Qualification validity after positive acceptance.
- Qualification freshness after Qualified Lead creation.
- Qualified Lead revalidation.
- Qualified Lead revocation.
- Qualified Lead invalidation.
- Opportunity-conversion eligibility.
- Opportunity creation.
- Opportunity pipeline state.

Those responsibilities belong to Qualified Lead Domain Service and Opportunity Domain Service.

### Qualification Request and Decision Separation

The following concepts must remain separate:

```text
Qualification Request
  = request to evaluate the Lead

Qualification Evidence
  = governed facts considered during evaluation

Qualification Score
  = deterministic or AI-derived analytical input

Qualification Recommendation
  = non-binding proposed outcome

Qualification Decision
  = accepted authoritative determination

Qualified Lead
  = canonical positive result created from
    an accepted positive qualification Decision
```

A qualification request is not a Decision.

A score is not a Decision.

A Recommendation is not a Decision.

A positive Decision is not itself a Qualified Lead.

A Qualified Lead creation request is not a confirmed Qualified Lead.

### Positive and Negative Qualification Outcomes

A positive qualification Decision must result in a controlled handoff to Qualified Lead Domain Service.

The Lead may preserve:

- `qualification_decision_id`.
- Positive-outcome projection.
- Decision authority projection.
- Decision timestamp.
- Qualified Lead creation request.
- Qualified Lead identifier after Confirmation.
- Handoff status.
- Handoff evidence.

The authoritative positive qualification snapshot belongs to Qualified Lead Domain Service.

Negative outcomes remain owned by Lead Domain Service and the applicable Decision authority.

Negative outcomes may include:

- Disqualified.
- Invalid.
- Duplicate.
- Spam.
- Fraudulent or prohibited inquiry.
- Withdrawn.
- Expired before positive qualification.
- Insufficient automotive intent.
- Unsupported request.
- No valid contact path.
- Outside configured market.
- Another governed negative outcome.

A negative Lead outcome must not create a Qualified Lead.

### Lead and Interaction Separation

The Lead stores the original inquiry and normalized intake context.

Subsequent communications belong to Interaction Domain Service.

The Lead may contain projections such as:

- Source Interaction.
- First valid response Interaction.
- Latest Interaction.
- Last contact attempt.
- Contact-attempt count.
- Response time.
- Latest qualification evidence Interaction.

The Lead must not become the authoritative communication history.

Original messages, recordings, transcripts, attachments, and later communications remain governed by Interaction or provider evidence records.

### Lead and Assignment Separation

Lead assignment represents intake ownership or response responsibility.

It does not represent active Opportunity ownership.

Lead assignment may identify:

- Intake User.
- Response User.
- Intake team.
- Qualification team.
- Queue.
- Human reviewer.

Opportunity assignment begins after confirmed Opportunity creation.

Lead assignment must not silently become Opportunity assignment.

### Lead and Communication Permission Separation

A Lead may contain a current projection of:

- Contact permission.
- Communication purpose.
- Permitted channel.
- Opt-out state.
- Quiet-hours status.
- Frequency restrictions.

The Customer Domain Service or configured Consent authority owns canonical Consent and communication permissions.

A Lead does not create:

- Marketing Consent.
- Cross-channel Consent.
- Recording Consent.
- Finance Consent.
- Permission to disclose sensitive data.
- Unlimited follow-up permission.

Before every outbound communication, current permission must be re-evaluated through deterministic policy.

### Lead and Vehicle Separation

A Lead may contain:

- Submitted Vehicle-interest text.
- Extracted Vehicle preferences.
- Requested make.
- Requested model.
- Requested body type.
- Requested condition.
- Requested features.
- Specific Vehicle reference where resolved.

Vehicle Domain Service owns canonical Vehicle identity and specifications.

Inventory Record owns authoritative availability.

Lead interest does not prove:

- Vehicle identity.
- Vehicle availability.
- Vehicle Reservation.
- Vehicle Allocation.
- Price.
- Delivery date.

### Lead and Appointment Separation

A Lead may contain a request for:

- Appointment.
- Callback.
- Showroom visit.
- Test drive.

A requested Appointment does not create or confirm an Appointment.

Appointment Domain Service owns:

- Appointment identity.
- Scheduling.
- Resource availability.
- Confirmation.
- Rescheduling.
- Cancellation.
- Attendance.

### Lead and Quotation Separation

A Lead may request pricing or a Quotation.

Lead Domain Service does not own:

- Quotation version.
- Commercial terms.
- Discount approval.
- Tax calculations.
- Customer acceptance.
- Quotation expiration.
- Quotation supersession.

Those responsibilities belong to Quotation Domain Service and the applicable pricing authorities.

### Lead and Trade-In Separation

A Lead may contain possible Trade-In interest.

It does not own:

- Trade-In Vehicle identity.
- Inspection.
- Appraisal.
- Actual cash value.
- Customer allowance.
- Payoff.
- Ownership transfer.
- Acquisition.
- Inventory intake.

Trade-In interest is evidence used by later Opportunity and Trade-In workflows.

### Lead and Finance Separation

A Lead may contain possible finance interest.

It does not own:

- Applicant identity.
- Finance Consent.
- Finance Application.
- Lender submission.
- Lender Decision.
- Approved rate.
- Approved term.
- Financial Contract.
- Funding.
- Payment.

Sensitive finance information must not be collected or stored merely because a Lead expressed finance interest.

### Lead and Deal Separation

A Lead does not own:

- Customer commitment.
- Deal creation.
- Vehicle Reservation.
- Vehicle Allocation.
- Contract execution.
- Payment.
- Funding.
- Delivery.
- Deal completion.

Those responsibilities belong to the applicable downstream Domain Services and external authorities.

### Original Evidence and Immutability

The original Lead source evidence must remain immutable.

The Lead must preserve:

- Source record identifier.
- Source delivery identifier.
- Original payload reference.
- Original payload hash.
- Original content reference.
- Source timestamp.
- ASOS receipt timestamp.
- Source authority.
- Source deduplication key.
- Connector and batch context.

Corrections must use:

- A correction record.
- A superseding normalized projection.
- A governed annotation.
- A source correction Event.
- A reconciliation workflow.

The original source evidence must not be silently overwritten.

### System Purpose

The Lead Object provides governed intake context to:

- Customer identity resolution.
- Response workflows.
- Communication workflows.
- Qualification workflows.
- Qualified Lead creation.
- Opportunity creation.
- Sales routing.
- Appointment discovery.
- Vehicle discovery.
- Trade-In discovery.
- Finance-interest discovery.
- Campaign attribution.
- Source analytics.
- Human Review.
- AI Agents.
- Audit and compliance services.

The Lead Object may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Original inquiry payload | Original source or provider |
| Original source delivery evidence | Original source or provider |
| Canonical Lead | Lead Domain Service |
| Submitted party information | Source evidence |
| Customer identity | Customer Domain Service |
| Original communication evidence | Interaction Domain Service or provider |
| Lead intake workflow | Lead Domain Service |
| Qualification request and pending state | Lead Domain Service |
| Qualification evidence package preparation | Lead Domain Service |
| Qualification score | Derived Intelligence |
| Qualification Recommendation | Recommendation record |
| Positive qualification Decision | Authorized Human, approved deterministic policy, or configured authority |
| Negative qualification outcome | Lead Domain Service and applicable Decision authority |
| Canonical Qualified Lead | Qualified Lead Domain Service |
| Qualification validity and revalidation | Qualified Lead Domain Service |
| Opportunity creation and pipeline | Opportunity Domain Service or configured CRM |
| Communication Consent | Customer Domain Service or configured Consent authority |
| Vehicle identity | Vehicle Domain Service |
| Vehicle availability | Inventory Record or configured Inventory authority |
| Appointment state | Appointment Domain Service |
| Quotation state | Quotation Domain Service |
| Trade-In state | Trade-In Domain Service |
| Finance state | Finance Application, Financial Contract, and configured external authorities |
| Final commercial transaction | Deal Domain Service and configured external authorities |

---

## 2. Canonical Schema

### Primary Identifiers

- `lead_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `receiving_team_id`.
- `assigned_owner_user_id`.
- `assigned_team_id`.
- `assignment_queue_id`.
- `qualification_team_id`.
- `responsible_reviewer_user_id`.
- `responsible_data_steward_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Customer and Journey Relationships

- `customer_id`.
- `customer_record_version`.
- `qualified_lead_id`.
- `qualification_cycle_id`.
- `opportunity_id_projection`.
- `duplicate_of_lead_id`.
- `campaign_id`.
- `source_interaction_id`.
- `latest_interaction_id`.
- `first_response_interaction_id`.
- `active_review_task_id`.
- `qualification_request_id`.
- `qualification_decision_id`.
- `qualified_lead_creation_request_id`.

### Lead Identity

- `lead_number`.
- `status`.
- `workflow_authority_mode`.
- `source_channel`.
- `source_system`.
- `source_authority`.
- `received_at`.
- `recorded_at`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.
- `reconciliation_status`.

### Source Identity

- `source_record_id`.
- `source_record_version`.
- `source_event_id`.
- `source_provider_account_id`.
- `source_received_at`.
- `source_occurred_at`.
- `source_updated_at`.
- `source_deduplication_key`.
- `source_correlation_id`.
- `source_campaign_reference`.
- `source_connector_id`.
- `source_connector_version`.
- `ingestion_batch_id`.
- `ingestion_job_id`.

### Original Evidence

- `raw_payload_reference`.
- `raw_payload_hash`.
- `original_content_reference`.
- `original_content_hash`.
- `original_content_type`.
- `original_content_language`.
- `original_attachment_references`.
- `original_evidence_snapshot`.
- `original_evidence_snapshot_hash`.
- `evidence_integrity_status`.
- `evidence_security_classification`.

### Submitted Party Information

- `submitted_display_name`.
- `submitted_given_name`.
- `submitted_family_name`.
- `submitted_organization_name`.
- `submitted_role`.
- `submitted_phone`.
- `submitted_email`.
- `submitted_messaging_identifier`.
- `submitted_social_identifier`.
- `submitted_preferred_language`.
- `submitted_country_code`.
- `submitted_location_text`.
- `submitted_representative_name`.
- `submitted_represented_party_name`.

Submitted party information remains unverified until accepted through Customer identity-resolution and contact-verification workflows.

### Inquiry Content

- `inquiry_subject`.
- `inquiry_text`.
- `inquiry_language`.
- `inquiry_category`.
- `requested_action`.
- `vehicle_interest_text`.
- `vehicle_interest_snapshot`.
- `budget_text`.
- `purchase_timeline_text`.
- `trade_in_interest_projection`.
- `finance_interest_projection`.
- `test_drive_interest_projection`.
- `appointment_interest_projection`.
- `preferred_contact_time_text`.
- `inquiry_summary_projection`.
- `inquiry_snapshot`.
- `inquiry_snapshot_hash`.

### Marketing Attribution

- `campaign_id`.
- `campaign_name_projection`.
- `marketing_source_reference`.
- `utm_source`.
- `utm_medium`.
- `utm_campaign`.
- `utm_content`.
- `utm_term`.
- `referrer_url_reference`.
- `landing_page_url_reference`.
- `advertisement_id`.
- `advertisement_set_id`.
- `creative_id`.
- `affiliate_reference`.
- `referral_reference`.
- `marketing_attribution_snapshot`.
- `marketing_attribution_snapshot_hash`.

### Technical Intake Metadata

- `source_ip_address_reference`.
- `user_agent_reference`.
- `device_type`.
- `session_id`.
- `form_id`.
- `form_version`.
- `provider_delivery_metadata_reference`.
- `webhook_delivery_id`.
- `webhook_signature_validation_status`.
- `replay_protection_status`.
- `payload_schema_version`.

Technical metadata must be retained only where lawful, necessary, and permitted by policy.

### Customer Resolution

- `identity_resolution_status`.
- `candidate_customer_ids`.
- `resolved_customer_id`.
- `identity_resolution_request_id`.
- `identity_resolution_decision_id`.
- `identity_resolution_method`.
- `identity_resolution_evidence_references`.
- `identity_resolution_conflict_references`.
- `identity_resolved_at`.
- `identity_revalidation_required`.

### Contact-Path Validation

- `contact_validation_status`.
- `phone_validation_status`.
- `email_validation_status`.
- `messaging_validation_status`.
- `preferred_contact_path_projection`.
- `validated_contact_point_references`.
- `invalid_contact_values`.
- `contact_validation_evidence_references`.
- `contact_validation_checked_at`.
- `contact_validation_expires_at`.

### Communication-Permission Projection

- `permission_assessment_status`.
- `contact_permission_status`.
- `contact_permission_basis`.
- `contact_permission_source`.
- `contact_permission_evidence_reference`.
- `contact_permission_checked_at`.
- `contact_permission_expires_at`.
- `permitted_channel_projections`.
- `restricted_channel_projections`.
- `marketing_permission_projection`.
- `transactional_response_projection`.
- `opt_out_projection`.
- `quiet_hours_projection`.
- `frequency_limit_projection`.
- `permission_revalidation_required`.

### Duplicate Assessment

- `duplicate_assessment_status`.
- `duplicate_candidate_lead_ids`.
- `duplicate_of_lead_id`.
- `duplicate_score`.
- `duplicate_reason`.
- `duplicate_evidence_references`.
- `duplicate_decision_id`.
- `duplicate_decided_at`.
- `duplicate_resolution_status`.
- `duplicate_merge_prohibited`.

Lead duplicate handling must not merge Customer identity automatically.

### Intent Assessment

- `intent_classification_status`.
- `automotive_intent_status`.
- `automotive_intent_score`.
- `inquiry_category_projection`.
- `commercial_intent_projection`.
- `non_automotive_routing_reference`.
- `spam_risk_score`.
- `fraud_risk_score`.
- `compliance_risk_score`.
- `intent_evidence_references`.
- `intent_review_status`.

### Qualification Workflow

- `qualification_status`.
- `qualification_request_id`.
- `qualification_request_status`.
- `qualification_request_type`.
- `qualification_requested_at`.
- `qualification_requested_by_actor_type`.
- `qualification_requested_by_actor_id`.
- `qualification_policy_id`.
- `qualification_policy_version`.
- `qualification_policy_snapshot_hash`.
- `qualification_readiness_status`.
- `qualification_missing_information`.
- `qualification_evidence_package_id`.
- `qualification_evidence_snapshot_hash`.
- `qualification_decision_request_id`.
- `qualification_decision_id`.
- `qualification_outcome_projection`.
- `qualification_decision_status_projection`.
- `qualification_decision_authority_projection`.
- `qualification_decided_at_projection`.
- `qualification_handoff_status`.

### Qualification Evidence

- `qualification_evidence_references`.
- `qualification_evidence_count`.
- `qualification_evidence_completeness_status`.
- `qualification_evidence_integrity_status`.
- `qualification_evidence_freshness_status`.
- `qualification_evidence_conflict_status`.
- `qualification_evidence_last_updated_at`.
- `qualification_evidence_required_fields`.
- `qualification_evidence_missing_fields`.
- `qualification_evidence_excluded_references`.
- `qualification_evidence_exclusion_reasons`.

Each qualification evidence record may contain:

- `qualification_evidence_id`.
- `evidence_type`.
- `evidence_role`.
- `source_system`.
- `source_record_id`.
- `source_authority`.
- `evidence_reference`.
- `evidence_hash`.
- `observed_at`.
- `recorded_at`.
- `freshness_status`.
- `verification_status`.
- `security_classification`.
- `exclusion_status`.
- `exclusion_reason`.

### Positive Qualification Handoff

- `positive_qualification_decision_received`.
- `positive_qualification_decision_id`.
- `positive_qualification_decision_snapshot_hash`.
- `qualified_lead_creation_request_id`.
- `qualified_lead_creation_idempotency_key`.
- `qualified_lead_creation_requested_at`.
- `qualified_lead_creation_request_status`.
- `qualified_lead_creation_failure_reason`.
- `qualified_lead_creation_confirmation_status`.
- `qualified_lead_id`.
- `qualification_cycle_id`.
- `qualified_lead_created_at`.
- `qualified_lead_creation_event_id`.
- `qualified_lead_creation_reconciliation_status`.

The Lead must not treat the positive qualification handoff as complete until Qualified Lead Domain Service confirms creation.

### Negative Outcome

- `negative_outcome_type`.
- `negative_outcome_reason_codes`.
- `negative_outcome_summary`.
- `negative_outcome_decision_id`.
- `negative_outcome_authority`.
- `negative_outcome_evidence_references`.
- `negative_outcome_at`.
- `negative_outcome_review_status`.
- `negative_outcome_expiration_or_reentry_policy`.

### Disqualification

- `disqualification_reason`.
- `disqualification_decision_id`.
- `disqualification_evidence_references`.
- `disqualified_at`.
- `disqualified_by_actor_type`.
- `disqualified_by_actor_id`.

### Invalidity

- `invalid_reason`.
- `invalidation_decision_id`.
- `invalidation_evidence_references`.
- `invalidated_at`.
- `invalidated_by_actor_type`.
- `invalidated_by_actor_id`.

### Withdrawal

- `withdrawal_reason`.
- `withdrawal_source`.
- `withdrawal_evidence_references`.
- `withdrawn_at`.
- `withdrawn_by_actor_type`.
- `withdrawn_by_actor_id`.

### Expiration

- `expiration_reason`.
- `expiration_policy_id`.
- `expiration_policy_version`.
- `expired_at`.
- `expiration_evidence_references`.

### Assignment

- `assignment_status`.
- `assigned_owner_user_id`.
- `assigned_team_id`.
- `assignment_queue_id`.
- `assigned_at`.
- `assignment_rule_id`.
- `assignment_rule_version`.
- `assignment_reason`.
- `assignment_expires_at`.
- `previous_owner_user_id`.
- `assignment_decision_id`.
- `assignment_snapshot`.
- `assignment_snapshot_hash`.

### Response and Activity Projections

- `first_response_due_at`.
- `first_response_at`.
- `first_response_interaction_id`.
- `response_time_seconds`.
- `response_sla_status`.
- `response_sla_policy_id`.
- `response_sla_policy_version`.
- `last_contact_attempt_at`.
- `last_contact_attempt_interaction_id`.
- `contact_attempt_count`.
- `last_activity_at`.
- `lead_age_seconds`.
- `time_to_assignment_seconds`.
- `time_to_qualification_request_seconds`.
- `time_to_positive_decision_seconds`.
- `time_to_qualified_lead_creation_seconds`.
- `time_to_negative_outcome_seconds`.

These fields are projections calculated from accepted Events and Interactions.

### Human Review

- `review_required`.
- `review_status`.
- `review_type`.
- `review_reason_codes`.
- `review_request_ids`.
- `assigned_reviewer_ids`.
- `review_decision_ids`.
- `review_outcome`.
- `review_notes`.
- `review_evidence_references`.
- `review_requested_at`.
- `reviewed_at`.
- `review_expires_at`.

### Derived Intelligence

- `lead_score`.
- `qualification_score`.
- `duplicate_score`.
- `automotive_intent_score`.
- `contactability_score`.
- `urgency_score`.
- `purchase_timeline_category`.
- `vehicle_preferences`.
- `budget_range`.
- `sentiment`.
- `inquiry_summary`.
- `recommended_next_action`.
- `recommended_owner_profile`.
- `recommended_response_channel`.
- `recommended_response_template_id`.
- `recommended_qualification_outcome`.
- `recommended_missing_information`.
- `recommended_human_review`.
- `requires_human_review`.
- `derived_intelligence_expires_at`.

Every material derived output must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority or automation policy.

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
- `archived_at`.
- `anonymized_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_id` | UUID | Yes | ASOS | Immutable Canonical Lead identifier. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `lead_number` | String | Yes | ASOS or configured authority | Human-readable Lead reference. |
| `status` | Enum | Yes | Lead workflow | Current Lead lifecycle state. |
| `source_channel` | Enum | Yes | Source evidence | Channel through which the inquiry entered. |
| `source_system` | String | Yes | Integration Context | Source system or provider. |
| `source_authority` | Enum | Yes | Governance | Authority classification of the original source. |
| `received_at` | Timestamp | Yes | Source or ASOS | Time the inquiry was accepted for intake. |
| `recorded_at` | Timestamp | Yes | ASOS | Time the Canonical Lead was recorded. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Relationship Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | UUID | No | Customer relationship | Resolved Canonical Customer. |
| `qualified_lead_id` | UUID | No | Qualified Lead relationship | Confirmed Qualified Lead created from the positive Decision. |
| `qualification_cycle_id` | UUID | No | Qualified Lead relationship | Qualification cycle established by Qualified Lead Domain Service. |
| `opportunity_id_projection` | UUID | No | Opportunity projection | Opportunity related through Qualified Lead conversion. |
| `duplicate_of_lead_id` | UUID | No | Lead workflow | Surviving Lead where this Lead is a duplicate. |
| `source_interaction_id` | UUID | No | Interaction relationship | Interaction representing the original communication where applicable. |
| `campaign_id` | UUID | No | Campaign relationship | Campaign associated with the inquiry. |

### Source Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `source_record_id` | String | No | External source | Source-system record identifier. |
| `source_event_id` | String | No | External source | External Event or delivery identifier. |
| `source_received_at` | Timestamp | Yes | Source evidence | Source receipt or emission time. |
| `source_deduplication_key` | String | Conditional | Integration Context | Source-specific duplicate-prevention key. |
| `raw_payload_reference` | String | Yes | Evidence repository | Controlled reference to the original payload. |
| `raw_payload_hash` | String | Yes | ASOS | Integrity hash of the original payload. |
| `original_content_reference` | String | Conditional | Evidence repository | Original message, recording, document, or submitted content. |
| `original_content_hash` | String | Conditional | ASOS | Integrity hash of the original content. |

### Submitted Party Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `submitted_display_name` | String | No | Source-provided | Unverified submitted name. |
| `submitted_organization_name` | String | No | Source-provided | Unverified organization name. |
| `submitted_phone` | String | Conditional | Source-provided | Unverified submitted phone number. |
| `submitted_email` | String | Conditional | Source-provided | Unverified submitted email address. |
| `submitted_messaging_identifier` | String | Conditional | Source-provided | Provider-specific messaging identifier. |
| `submitted_preferred_language` | String | No | Source-provided | Submitted language preference. |
| `submitted_location_text` | String | No | Source-provided | Unverified geographic description. |

At least one of the following must exist unless the source represents an approved anonymous inquiry:

- Contact path.
- Source-account identifier.
- Walk-in reference.
- Original Interaction reference.
- Sufficient inquiry content for Human Review.

### Inquiry Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inquiry_subject` | String | No | Source or normalization | Short inquiry subject. |
| `inquiry_text` | Text | Conditional | Source evidence | Submitted or normalized inquiry content. |
| `inquiry_language` | String | No | Source or Derived Intelligence | BCP 47 language tag. |
| `inquiry_category` | Enum | Yes | Canonical Projection | High-level category of the inquiry. |
| `requested_action` | Enum | No | Source or Derived Intelligence | Requested action without completion implication. |
| `vehicle_interest_text` | Text | No | Source evidence | Unstructured Vehicle interest. |
| `vehicle_interest_snapshot` | JSON Object | No | Derived Intelligence or accepted normalization | Extracted Vehicle preferences. |
| `trade_in_interest_projection` | Enum | No | Source or Derived Intelligence | Possible Trade-In interest. |
| `finance_interest_projection` | Enum | No | Source or Derived Intelligence | Possible finance interest. |
| `test_drive_interest_projection` | Enum | No | Source or Derived Intelligence | Possible test-drive interest. |

### Workflow Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `identity_resolution_status` | Enum | Yes | Workflow State | Customer-resolution progress. |
| `contact_validation_status` | Enum | Yes | Workflow State | Submitted contact-path validation state. |
| `intent_classification_status` | Enum | Yes | Workflow State | Automotive-intent assessment state. |
| `permission_assessment_status` | Enum | Yes | Workflow State | Communication-permission assessment state. |
| `duplicate_assessment_status` | Enum | Yes | Workflow State | Duplicate-assessment state. |
| `qualification_status` | Enum | Yes | Lead workflow | Qualification-request and handoff state. |
| `review_status` | Enum | Yes | Workflow State | Human Review state. |
| `assignment_status` | Enum | Yes | Workflow State | Intake-assignment state. |

### Qualification Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `qualification_request_id` | UUID | No | Lead workflow | Qualification request owned by Lead. |
| `qualification_request_status` | Enum | Yes | Lead workflow | Current qualification-request state. |
| `qualification_policy_id` | UUID | Conditional | Policy authority | Policy applied to the request. |
| `qualification_policy_version` | String | Conditional | Policy authority | Exact applied policy version. |
| `qualification_readiness_status` | Enum | Yes | Lead workflow | Readiness of evidence for Decision. |
| `qualification_evidence_package_id` | UUID | No | Lead workflow | Evidence package assembled by Lead. |
| `qualification_evidence_snapshot_hash` | String | Conditional | ASOS | Integrity hash of qualification evidence. |
| `qualification_decision_id` | UUID | No | Decision relationship | Authoritative qualification Decision reference. |
| `qualification_outcome_projection` | Enum | Yes | Decision projection | Projected accepted outcome. |
| `qualification_handoff_status` | Enum | Yes | Lead workflow | State of positive Decision handoff. |
| `qualified_lead_creation_request_id` | UUID | No | Lead workflow | Request sent to Qualified Lead Domain Service. |
| `qualified_lead_creation_idempotency_key` | String | Conditional | Lead workflow | Duplicate-prevention key for Qualified Lead creation. |
| `qualified_lead_creation_confirmation_status` | Enum | Yes | Lead workflow | Qualified Lead creation Confirmation state. |

### Negative Outcome Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `negative_outcome_type` | Enum | No | Lead workflow and Decision authority | Accepted negative Lead outcome. |
| `negative_outcome_reason_codes` | Array | No | Decision evidence | Reasons supporting the outcome. |
| `negative_outcome_decision_id` | UUID | Conditional | Decision authority | Decision authorizing the outcome. |
| `negative_outcome_evidence_references` | Array | Conditional | Evidence | Evidence supporting the outcome. |
| `negative_outcome_at` | Timestamp | Conditional | Lead workflow | Time the outcome became effective. |
| `disqualification_reason` | Enum | No | Decision authority | Reason for disqualification. |
| `invalid_reason` | Enum | No | Decision authority | Reason the Lead is invalid. |
| `withdrawal_reason` | Enum | No | Customer, source, or Decision authority | Reason processing was withdrawn. |
| `expiration_reason` | Enum | No | Policy | Reason the Lead expired. |

### Permission Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contact_permission_status` | Enum | Yes | Customer projection | Summary of current permitted contact. |
| `contact_permission_basis` | Enum | No | Consent authority | Applicable basis. |
| `contact_permission_evidence_reference` | String | No | Evidence | Supporting permission evidence. |
| `contact_permission_checked_at` | Timestamp | No | Policy workflow | Last evaluation time. |
| `contact_permission_expires_at` | Timestamp | No | Policy or source | Projection expiration time. |
| `opt_out_projection` | Enum | Yes | Customer projection | Current opt-out projection. |
| `permission_revalidation_required` | Boolean | Yes | Policy | Whether permission must be re-evaluated. |

### Derived Intelligence Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_score` | Decimal | No | Derived Intelligence | Lead-priority score. |
| `qualification_score` | Decimal | No | Derived Intelligence | Analytical qualification score. |
| `duplicate_score` | Decimal | No | Derived Intelligence | Estimated duplicate likelihood. |
| `automotive_intent_score` | Decimal | No | Derived Intelligence | Estimated automotive-commercial intent. |
| `contactability_score` | Decimal | No | Derived Intelligence | Estimated permitted contactability. |
| `urgency_score` | Decimal | No | Derived Intelligence | Estimated urgency. |
| `spam_risk_score` | Decimal | No | Derived Intelligence | Estimated spam risk. |
| `fraud_risk_score` | Decimal | No | Derived Intelligence | Estimated fraud risk. |
| `recommended_qualification_outcome` | Enum | No | Recommendation | Non-binding proposed qualification outcome. |
| `recommended_next_action` | String | No | Recommendation | Proposed next action. |
| `requires_human_review` | Boolean | Yes | Deterministic policy or Derived Intelligence | Whether Human Review is required. |

### Response Metrics

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `first_response_due_at` | Timestamp | No | Deterministic policy | Response deadline. |
| `first_response_at` | Timestamp | No | Interaction projection | Time of first accepted response. |
| `first_response_interaction_id` | UUID | No | Interaction relationship | Interaction proving the response. |
| `response_time_seconds` | Integer | No | Deterministic calculation | Time from Lead receipt to response. |
| `contact_attempt_count` | Integer | Yes | Interaction projection | Accepted contact-attempt count. |
| `last_contact_attempt_at` | Timestamp | No | Interaction projection | Latest contact-attempt time. |
| `time_to_qualification_request_seconds` | Integer | No | Deterministic calculation | Time to qualification request. |
| `time_to_qualified_lead_creation_seconds` | Integer | No | Deterministic calculation | Time to confirmed Qualified Lead creation. |

---

## 4. Enumerations

### LeadStatus

- `RECEIVED`
- `NORMALIZING`
- `VALIDATING`
- `REVIEW_REQUIRED`
- `QUALIFICATION_PENDING`
- `POSITIVE_HANDOFF_PENDING`
- `QUALIFIED_HANDOFF_COMPLETED`
- `DISQUALIFIED`
- `DUPLICATE`
- `INVALID`
- `WITHDRAWN`
- `EXPIRED`
- `ARCHIVED`

`QUALIFIED_HANDOFF_COMPLETED` means a separate Qualified Lead was confirmed.

It does not mean the Lead itself became the Qualified Lead.

### LeadSourceChannel

- `WEBSITE`
- `LANDING_PAGE`
- `WHATSAPP`
- `SMS`
- `PHONE`
- `EMAIL`
- `WALK_IN`
- `SOCIAL_MEDIA`
- `MARKETPLACE`
- `OEM_PLATFORM`
- `REFERRAL`
- `EVENT`
- `PARTNER`
- `CAMPAIGN_IMPORT`
- `CRM_IMPORT`
- `MANUAL_ENTRY`
- `EXISTING_CUSTOMER_REENGAGEMENT`
- `OTHER`

### LeadSourceAuthority

- `PROVIDER_VERIFIED`
- `CRM_VERIFIED`
- `OEM_VERIFIED`
- `PARTNER_VERIFIED`
- `DEALERSHIP_REPORTED`
- `CUSTOMER_SUBMITTED`
- `AUTHORIZED_HUMAN_ENTRY`
- `UNKNOWN`
- `DISPUTED`

`AI_EXTRACTED` is not a source authority.

AI-extracted data is Derived Intelligence.

### InquiryCategory

- `VEHICLE_PURCHASE`
- `VEHICLE_AVAILABILITY`
- `VEHICLE_PRICING`
- `TEST_DRIVE`
- `TRADE_IN`
- `FINANCE`
- `FLEET`
- `AFTERSALES_REDIRECT`
- `GENERAL_ENQUIRY`
- `COMPLAINT_REDIRECT`
- `NON_AUTOMOTIVE`
- `UNKNOWN`

### RequestedAction

- `REQUEST_INFORMATION`
- `REQUEST_CALLBACK`
- `REQUEST_QUOTATION`
- `REQUEST_TEST_DRIVE`
- `REQUEST_APPOINTMENT`
- `REQUEST_TRADE_IN_VALUATION`
- `REQUEST_FINANCE_INFORMATION`
- `REQUEST_VEHICLE_RESERVATION`
- `REQUEST_FLEET_CONTACT`
- `OTHER`
- `UNKNOWN`

A requested action does not prove approval, creation, Reservation, confirmation, or completion.

### IdentityResolutionStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `MATCHED_EXISTING_CUSTOMER`
- `NEW_CUSTOMER_CREATED`
- `AMBIGUOUS`
- `CONFLICTED`
- `NOT_REQUIRED`
- `FAILED`
- `REVIEW_REQUIRED`

### ContactValidationStatus

- `NOT_STARTED`
- `UNKNOWN`
- `VALIDATION_PENDING`
- `PARTIALLY_VALID`
- `VALID`
- `INVALID`
- `UNREACHABLE`
- `CONFLICTED`
- `REVIEW_REQUIRED`

### IntentClassificationStatus

- `NOT_STARTED`
- `UNKNOWN`
- `AUTOMOTIVE_INTENT`
- `NON_AUTOMOTIVE`
- `AMBIGUOUS`
- `SPAM`
- `FRAUD_RISK`
- `REVIEW_REQUIRED`

### PermissionAssessmentStatus

- `NOT_STARTED`
- `PENDING`
- `ASSESSED`
- `EVIDENCE_MISSING`
- `CONFLICTED`
- `EXPIRED`
- `REVIEW_REQUIRED`

### ContactPermissionStatus

- `NOT_EVALUATED`
- `RESPONSE_PERMITTED`
- `TRANSACTIONAL_ONLY`
- `MARKETING_PERMITTED`
- `PERMITTED_WITH_RESTRICTIONS`
- `NOT_PERMITTED`
- `OPTED_OUT`
- `EXPIRED`
- `REVALIDATION_REQUIRED`
- `DISPUTED`

### ContactPermissionBasis

- `CUSTOMER_REQUESTED_RESPONSE`
- `EXPLICIT_CONSENT`
- `EXISTING_CUSTOMER_RELATIONSHIP`
- `CONTRACTUAL_NECESSITY`
- `LEGITIMATE_INTEREST_WHERE_PERMITTED`
- `LEGAL_OBLIGATION`
- `NOT_REQUIRED`
- `UNKNOWN`

Applicable law and Tenant policy determine which bases are permitted.

### OptOutProjection

- `NO_ACTIVE_OPT_OUT_OBSERVED`
- `POTENTIAL_OPT_OUT`
- `ACTIVE_OPT_OUT`
- `PARTIAL_CHANNEL_OPT_OUT`
- `PARTIAL_PURPOSE_OPT_OUT`
- `DISPUTED`
- `UNKNOWN`

### DuplicateAssessmentStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `NO_MATCH`
- `POTENTIAL_MATCH`
- `CONFIRMED_DUPLICATE`
- `CONFLICTED`
- `REVIEW_REQUIRED`

### QualificationRequestStatus

- `NOT_REQUESTED`
- `DRAFT`
- `EVIDENCE_COLLECTION`
- `EVIDENCE_INCOMPLETE`
- `READY_FOR_DECISION`
- `DECISION_PENDING`
- `POSITIVE_DECISION_ACCEPTED`
- `NEGATIVE_DECISION_ACCEPTED`
- `MORE_INFORMATION_REQUIRED`
- `HUMAN_REVIEW_REQUIRED`
- `CANCELLED`
- `FAILED`
- `EXPIRED`

### QualificationReadinessStatus

- `NOT_EVALUATED`
- `NOT_READY`
- `MISSING_INFORMATION`
- `EVIDENCE_CONFLICT`
- `READY`
- `READY_WITH_LIMITATIONS`
- `HUMAN_REVIEW_REQUIRED`
- `BLOCKED`

### QualificationOutcomeProjection

- `NOT_DECIDED`
- `POSITIVE_DECISION_ACCEPTED`
- `DISQUALIFIED`
- `INVALID`
- `DUPLICATE`
- `WITHDRAWN`
- `EXPIRED`
- `MORE_INFORMATION_REQUIRED`
- `HUMAN_REVIEW_REQUIRED`
- `DECISION_REVOKED`
- `DISPUTED`

### QualificationHandoffStatus

- `NOT_APPLICABLE`
- `NOT_STARTED`
- `READY`
- `REQUESTED`
- `PENDING_QUALIFIED_LEAD_CONFIRMATION`
- `CONFIRMED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ConfirmationStatus

- `NOT_REQUIRED`
- `NOT_RECEIVED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `RECONCILIATION_REQUIRED`

### NegativeOutcomeType

- `DISQUALIFIED`
- `INVALID`
- `DUPLICATE`
- `SPAM`
- `FRAUDULENT_OR_PROHIBITED`
- `WITHDRAWN`
- `EXPIRED`
- `TRANSFERRED_TO_NON_SALES_WORKFLOW`

### DisqualificationReason

- `NO_VALID_CONTACT_PATH`
- `NO_AUTOMOTIVE_INTENT`
- `OUTSIDE_CONFIGURED_MARKET`
- `UNREACHABLE_AFTER_APPROVED_ATTEMPTS`
- `CUSTOMER_NOT_READY`
- `DO_NOT_CONTACT`
- `BUDGET_OR_REQUIREMENT_MISMATCH`
- `UNSUPPORTED_REQUEST`
- `TRANSFERRED_TO_ANOTHER_WORKFLOW`
- `INSUFFICIENT_EVIDENCE`
- `OTHER`

A disqualified Lead may still be valid business data.

Disqualification must not be used as a substitute for invalidity.

### InvalidReason

- `SPAM`
- `TEST_RECORD`
- `MALFORMED_SOURCE_DATA`
- `NONEXISTENT_CONTACT`
- `PROHIBITED_CONTENT`
- `FRAUDULENT_SUBMISSION`
- `SOURCE_AUTHENTICATION_FAILED`
- `SOURCE_INTEGRITY_FAILED`
- `NO_ACTIONABLE_INQUIRY`
- `OTHER`

### WithdrawalReason

- `CUSTOMER_WITHDREW`
- `AUTHORIZED_REPRESENTATIVE_WITHDREW`
- `SOURCE_WITHDREW`
- `DUPLICATE_RESOLUTION`
- `TRANSFERRED_TO_OTHER_WORKFLOW`
- `LEGAL_OR_COMPLIANCE_RESTRICTION`
- `OTHER`

### ExpirationReason

- `NO_RESPONSE_WITHIN_POLICY`
- `QUALIFICATION_NOT_COMPLETED_WITHIN_POLICY`
- `SOURCE_RECORD_EXPIRED`
- `CAMPAIGN_EXPIRED`
- `CUSTOMER_INTENT_EXPIRED`
- `EVIDENCE_EXPIRED`
- `OTHER`

### AssignmentStatus

- `UNASSIGNED`
- `QUEUED`
- `ASSIGNMENT_PENDING`
- `ASSIGNED`
- `REASSIGNMENT_PENDING`
- `REASSIGNED`
- `RETURNED_TO_QUEUE`
- `SUSPENDED`
- `CLOSED`

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `APPROVED_WITH_CONDITIONS`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

### ReviewType

- `IDENTITY`
- `CONTACT_VALIDATION`
- `CONTACT_PERMISSION`
- `DUPLICATE`
- `AUTOMOTIVE_INTENT`
- `QUALIFICATION_EVIDENCE`
- `QUALIFICATION_EXCEPTION`
- `FRAUD`
- `COMPLIANCE`
- `DATA_QUALITY`
- `POSITIVE_HANDOFF`
- `CORRECTION`
- `OTHER`

### EvidenceIntegrityStatus

- `NOT_EVALUATED`
- `PENDING`
- `VALID`
- `HASH_MISMATCH`
- `INCOMPLETE`
- `CORRUPTED`
- `ALTERED`
- `DISPUTED`
- `REVIEW_REQUIRED`

### EvidenceCompletenessStatus

- `NOT_EVALUATED`
- `INSUFFICIENT`
- `PARTIALLY_COMPLETE`
- `COMPLETE`
- `COMPLETE_WITH_LIMITATIONS`
- `REVIEW_REQUIRED`

### EvidenceFreshnessStatus

- `NOT_EVALUATED`
- `CURRENT`
- `APPROACHING_EXPIRY`
- `STALE`
- `EXPIRED`
- `DISPUTED`

### DataQualityStatus

- `COMPLETE`
- `COMPLETE_WITH_LIMITATIONS`
- `INCOMPLETE_NON_BLOCKING`
- `INCOMPLETE_BLOCKING`
- `STALE`
- `CONFLICTED`
- `QUARANTINED`

### ConflictStatus

- `NONE`
- `POTENTIAL`
- `CONFIRMED`
- `UNDER_REVIEW`
- `RESOLVED`
- `ACCEPTED_WITH_LIMITATIONS`

### ReconciliationStatus

- `NOT_REQUIRED`
- `CURRENT`
- `PENDING`
- `IN_PROGRESS`
- `RESOLVED`
- `FAILED`
- `MANUAL_REVIEW_REQUIRED`

### WorkflowAuthorityMode

- `ASOS_AUTHORITATIVE`
- `EXTERNAL_CRM_AUTHORITATIVE`
- `GOVERNED_BIDIRECTIONAL`

### ActorType

- `USER`
- `SERVICE`
- `AI_AGENT`
- `AUTOMATION`
- `EXTERNAL_SYSTEM`
- `CUSTOMER`
- `AUTHORIZED_REPRESENTATIVE`

---

## 5. Validation Rules

### Tenant and Organizational Rules

- `tenant_id` is required and immutable.
- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Customer, Qualified Lead, Opportunity, Interaction, campaign, dealership, branch, team, queue, User, and policy references must belong to the authorized Tenant.
- Cross-Tenant intake, Customer matching, duplicate matching, qualification, evidence access, analytics, search, AI retrieval, export, or conversion is prohibited unless governed by an approved mechanism.
- Background Jobs, Event Consumers, integrations, and AI Agents must receive trusted Tenant execution context.
- An external source must not choose an unrestricted `tenant_id`.

### Lead Creation Rules

Every Lead must contain:

- Valid Tenant context.
- Source channel.
- Source system.
- Source authority.
- Receipt timestamp.
- Original evidence or a valid manual-entry record.
- Source deduplication context where applicable.
- At least one contact, account, walk-in, Interaction, or inquiry path.
- Data classification.
- Creation actor.
- Audit evidence.

A Lead must not be created without a meaningful inquiry or approved anonymous-review purpose.

### Source Authentication Rules

External Lead ingestion must validate applicable:

- Provider authentication.
- Webhook signature.
- Timestamp.
- Replay protection.
- Provider account.
- Tenant routing.
- Payload Schema.
- Payload size.
- Content type.
- Source permission.
- Deduplication key.
- Original payload hash.

Failed source authentication must not create a normal active Lead.

The payload may be retained in a restricted security workflow where permitted.

### Source Deduplication Rules

- Source deduplication must use source-specific identifiers.
- Repeated delivery of the same source occurrence must not create duplicate Leads.
- Source deduplication remains separate from ASOS Event Consumer deduplication using `event_id`.
- API and Command retries must use `idempotency_key`.
- Duplicate source delivery must preserve operational evidence.
- A source duplicate does not automatically prove that two different Leads represent the same Customer.

### Evidence Integrity Rules

- Original payload and content hashes must use an approved cryptographic algorithm.
- Original evidence must be immutable.
- Failed integrity checks block normal progression.
- Evidence correction must preserve prior values and hashes.
- Evidence references must use controlled storage.
- Restricted evidence must not be copied into unrestricted fields.
- Missing evidence must remain explicitly missing.

### Submitted Party Rules

- Submitted party information is unverified.
- Submitted names must not automatically create a Customer.
- Contact values must not become permanent Customer identity without governed resolution.
- An organization name must not prove representative authority.
- Ambiguous party information may require Human Review.
- Original submitted values must be preserved.
- Normalized values must remain separate.

### Customer Resolution Rules

- Customer identity resolution is governed by Customer Domain Service.
- A Lead may remain unresolved during intake where policy permits.
- Positive qualification creation normally requires a resolved Customer.
- AI confidence alone must not resolve identity.
- Candidate Customers must belong to the same Tenant.
- Cross-Tenant identity matching is prohibited.
- Customer merge must use the Customer merge workflow.
- Customer correction must not rewrite original Lead evidence.
- Identity conflict may block qualification or handoff.

### Contact Validation Rules

- Contact-path validation must preserve method and evidence.
- A syntactically valid email or phone number is not automatically verified ownership.
- Provider-delivery success does not prove Customer identity.
- Invalid contact values must not be used for outbound communication.
- Sensitive contact information must be protected.
- Contact validation expiration must be configurable.
- Missing valid contact may produce a negative outcome only through approved policy or Decision.

### Communication-Permission Rules

Before outbound communication, deterministic policy must evaluate:

- Customer or recipient.
- Contact point.
- Purpose.
- Channel.
- Consent or lawful basis.
- Opt-out.
- Quiet hours.
- Frequency.
- Jurisdiction.
- Customer restrictions.
- Message classification.
- Human Approval or approved automation policy where required.

Lead priority, qualification score, or AI Recommendation must not bypass communication controls.

### Duplicate-Assessment Rules

Duplicate assessment must distinguish:

```text
Duplicate source delivery
  = repeated delivery of the same occurrence

Duplicate Lead
  = two Lead records representing the same inquiry or commercial-intent record

Duplicate Customer candidate
  = possible identity overlap requiring Customer governance
```

- Duplicate score is not a duplicate Decision.
- Confirmed duplicate status requires evidence.
- `duplicate_of_lead_id` must belong to the same Tenant.
- Circular duplicate relationships are prohibited.
- Duplicate handling must preserve the original Lead.
- Duplicate handling must not merge Customers automatically.
- A Lead with a distinct commercial intent must not be discarded merely because the Customer already exists.

### Intent Rules

- Automotive intent must be evidence-backed.
- General information may remain unqualified.
- Non-automotive inquiries may be redirected.
- Complaint or aftersales inquiries must not be forced into a sales qualification workflow.
- AI classification must remain Derived Intelligence.
- Spam or fraud signals require governed handling.
- Missing intent must not be replaced with invented intent.

### Qualification Request Rules

Lead Domain Service owns the qualification request.

A qualification request requires applicable:

- Lead identifier.
- Current record version.
- Customer-resolution status.
- Contact-path status.
- Communication-permission context.
- Evidence package.
- Policy and version.
- Missing-information status.
- Human Review requirements.
- Request actor.
- Idempotency key.
- Audit evidence.

A request must not be described as a positive Decision.

### Qualification Evidence Rules

Qualification evidence must:

- Be traceable.
- Preserve source authority.
- Preserve source record version.
- Preserve timestamps.
- Preserve hashes where applicable.
- Preserve security classification.
- Distinguish original evidence from Derived Intelligence.
- Distinguish supporting, contradicting, and excluded evidence.
- Preserve exclusion reasons.
- Preserve freshness.
- Preserve missing information.

Evidence with failed integrity must not support a positive handoff.

### Qualification Score Rules

- Scores must use approved scales.
- Model or formula version is required.
- Input records and versions are required.
- Evidence is required.
- Generation timestamp is required.
- Expiration is required where applicable.
- Missing scores must not be replaced with invented values.
- Score thresholds must remain configurable.
- A score must not create a positive qualification Decision.
- A score must not create a Qualified Lead.
- A score must not override Human Review.
- A score must not create an Opportunity.

### Qualification Recommendation Rules

A Recommendation must preserve:

- Proposed outcome.
- Evidence.
- Reasons.
- Missing information.
- Assumptions.
- Limitations.
- Model or rule.
- Version.
- Expiration.
- Required authority.

A Recommendation must not be stored as the qualification Decision.

An AI Agent must not approve its own Recommendation.

### Qualification Decision Rules

The authoritative Decision record may be created by:

- An authorized Human.
- An approved deterministic policy where permitted.
- A configured external authoritative workflow.
- A governed hybrid workflow.

The Decision must preserve:

- Decision identifier.
- Result.
- Authority.
- Actor.
- Policy and version.
- Evidence.
- Reasons.
- Conditions.
- Human Review.
- Timestamp.
- Expiration.
- Audit history.

Lead Domain Service may receive and project the Decision.

Lead Domain Service must not duplicate the canonical positive qualification snapshot.

### Positive Handoff Rules

A positive qualification Decision requires:

- Accepted positive Decision.
- Resolved Customer.
- Same-Tenant relationships.
- Valid qualification policy.
- Required evidence.
- Evidence integrity.
- Required Human Review.
- No blocking conflict.
- Handoff snapshot.
- Idempotent Qualified Lead creation request.

Lead enters `POSITIVE_HANDOFF_PENDING` after the creation request is accepted.

Lead enters `QUALIFIED_HANDOFF_COMPLETED` only after:

- Qualified Lead Domain Service confirms creation.
- `qualified_lead_id` exists.
- `qualification_cycle_id` exists.
- Qualified Lead Event or equivalent evidence exists.
- Tenant and relationship validation passes.
- Reconciliation completes.

### Negative Outcome Rules

Negative outcomes remain governed by Lead.

A negative outcome must preserve:

- Outcome type.
- Reason.
- Decision or policy.
- Authority.
- Evidence.
- Actor.
- Timestamp.
- Review.
- Re-entry policy where applicable.

A negative outcome must not create a Qualified Lead.

### Disqualification Rules

Disqualification means the Lead is valid business data but does not currently proceed through the configured sales qualification workflow.

Disqualification must not be used to hide:

- Invalid source data.
- Duplicate Lead.
- Spam.
- Fraud.
- Opt-out.
- Customer complaint.
- Security incident.

Disqualification may permit a later new Lead or re-engagement only according to policy.

### Invalidity Rules

Invalidity means the Lead record cannot represent a valid normal sales inquiry.

Invalidity requires:

- Reason.
- Evidence.
- Decision or deterministic rule.
- Actor.
- Timestamp.
- Audit record.

Invalidity must not delete original evidence.

### Withdrawal Rules

Withdrawal must preserve:

- Withdrawal source.
- Customer or representative evidence where applicable.
- Scope.
- Timestamp.
- Actor.
- Communication-permission impact.
- Downstream impact.

Customer withdrawal must be evaluated for possible opt-out.

### Expiration Rules

- Expiration policy must be versioned.
- Expiration must use authoritative server time.
- Expired Lead must not enter positive handoff.
- Expiration must not erase Customer or Interaction evidence.
- A later independent inquiry should normally create a new Lead.
- Manual expiration override requires authorized Decision.

### Assignment Rules

- Assignment must remain Tenant-scoped.
- User, team, and queue must be authorized.
- Assignment policy must be versioned.
- Assignment must preserve reason.
- AI may recommend assignment.
- Assignment Recommendation does not change ownership.
- Lead assignment does not create Opportunity ownership.
- Assignment must respect restricted Customer and evidence access.

### Response-SLA Rules

- SLA policy must be versioned.
- Response deadline must use authoritative server time.
- Provider auto-acknowledgement must not satisfy a Human-response SLA unless policy permits it.
- First valid response must reference an Interaction.
- Duplicate Lead resolution must define SLA treatment.
- Waiver requires authorized Decision.
- SLA changes must not rewrite the original deadline.

### Appointment Request Rules

A Lead Appointment request must not directly:

- Reserve a calendar slot.
- Confirm an Appointment.
- Confirm Vehicle availability.
- Confirm test-drive availability.

The Appointment workflow owns the result.

### Quotation Request Rules

A Lead Quotation request must not directly:

- Create authoritative pricing.
- Approve a discount.
- Issue a Quotation.
- Confirm Customer acceptance.

The Quotation workflow owns those results.

### Finance-Interest Rules

Lead may preserve general finance interest.

It must not:

- Collect unnecessary credit information.
- Create Finance Consent.
- Create a Finance Application.
- Submit to a Lender.
- Record a Lender Decision.
- Promise finance terms.
- Request funding.

### Trade-In-Interest Rules

Lead may preserve general Trade-In interest.

It must not:

- Create an appraisal.
- Approve value.
- Verify ownership.
- Verify payoff.
- Acquire the Vehicle.
- Request Inventory intake.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Lead creation must support idempotency.
- Qualification requests must support idempotency.
- Human Review requests must support idempotency.
- Qualified Lead creation requests must support idempotency.
- Negative outcome requests must support idempotency.
- Assignment requests must support idempotency.
- Event Consumers must prevent duplicate business effects using `event_id`.
- Source ingestion must use source-specific deduplication.
- Duplicate retries must not create duplicate:
  - Leads.
  - Qualification requests.
  - Evidence packages.
  - Human Reviews.
  - Qualified Lead creation requests.
  - Qualified Leads.
  - Assignments.
  - Negative outcome records.
  - Notifications.
  - Tasks.

### AI Failure Rules

- AI failure must not block durable Lead intake.
- AI failure must not delete evidence.
- AI failure must remain explicit.
- Missing AI output must not be replaced with fabricated values.
- Deterministic and Human workflows must remain available.
- AI failure must not silently create a negative outcome.
- AI failure may trigger Human Review.

### Human Review Requirements

Human Review is required according to policy for:

- Ambiguous Customer identity.
- Conflicting contact information.
- Duplicate uncertainty.
- Representative-authority uncertainty.
- Contact-permission conflict.
- High fraud risk.
- Compliance risk.
- Evidence conflict.
- Low evidence quality.
- Qualification exception.
- Positive Decision exception.
- Source authentication concern.
- Restricted-data exposure.
- AI and deterministic-rule disagreement.
- Another material legal, privacy, security, or commercial risk.

---

## 6. State Machine

### Allowed States

```text
RECEIVED
NORMALIZING
VALIDATING
REVIEW_REQUIRED
QUALIFICATION_PENDING
POSITIVE_HANDOFF_PENDING
QUALIFIED_HANDOFF_COMPLETED
DISQUALIFIED
DUPLICATE
INVALID
WITHDRAWN
EXPIRED
ARCHIVED
```

### Principal Allowed Transitions

```text
RECEIVED → NORMALIZING
RECEIVED → VALIDATING
RECEIVED → REVIEW_REQUIRED
RECEIVED → DUPLICATE
RECEIVED → INVALID
RECEIVED → WITHDRAWN
RECEIVED → EXPIRED

NORMALIZING → VALIDATING
NORMALIZING → REVIEW_REQUIRED
NORMALIZING → DUPLICATE
NORMALIZING → INVALID

VALIDATING → REVIEW_REQUIRED
VALIDATING → QUALIFICATION_PENDING
VALIDATING → DISQUALIFIED
VALIDATING → DUPLICATE
VALIDATING → INVALID
VALIDATING → WITHDRAWN
VALIDATING → EXPIRED

REVIEW_REQUIRED → VALIDATING
REVIEW_REQUIRED → QUALIFICATION_PENDING
REVIEW_REQUIRED → DISQUALIFIED
REVIEW_REQUIRED → DUPLICATE
REVIEW_REQUIRED → INVALID
REVIEW_REQUIRED → WITHDRAWN
REVIEW_REQUIRED → EXPIRED

QUALIFICATION_PENDING → REVIEW_REQUIRED
QUALIFICATION_PENDING → POSITIVE_HANDOFF_PENDING
QUALIFICATION_PENDING → DISQUALIFIED
QUALIFICATION_PENDING → DUPLICATE
QUALIFICATION_PENDING → INVALID
QUALIFICATION_PENDING → WITHDRAWN
QUALIFICATION_PENDING → EXPIRED

POSITIVE_HANDOFF_PENDING → QUALIFIED_HANDOFF_COMPLETED
POSITIVE_HANDOFF_PENDING → QUALIFICATION_PENDING
POSITIVE_HANDOFF_PENDING → REVIEW_REQUIRED
POSITIVE_HANDOFF_PENDING → WITHDRAWN
POSITIVE_HANDOFF_PENDING → EXPIRED

QUALIFIED_HANDOFF_COMPLETED → ARCHIVED

DISQUALIFIED → ARCHIVED
DUPLICATE → ARCHIVED
INVALID → ARCHIVED
WITHDRAWN → ARCHIVED
EXPIRED → ARCHIVED
```

### Forbidden Ordinary Transitions

```text
RECEIVED → QUALIFIED_HANDOFF_COMPLETED

NORMALIZING → QUALIFIED_HANDOFF_COMPLETED

VALIDATING → QUALIFIED_HANDOFF_COMPLETED

REVIEW_REQUIRED → QUALIFIED_HANDOFF_COMPLETED

QUALIFICATION_PENDING → QUALIFIED_HANDOFF_COMPLETED

POSITIVE_HANDOFF_PENDING → QUALIFIED_HANDOFF_COMPLETED
  without Qualified Lead Confirmation

DISQUALIFIED → POSITIVE_HANDOFF_PENDING
DUPLICATE → POSITIVE_HANDOFF_PENDING
INVALID → POSITIVE_HANDOFF_PENDING
WITHDRAWN → POSITIVE_HANDOFF_PENDING
EXPIRED → POSITIVE_HANDOFF_PENDING

QUALIFIED_HANDOFF_COMPLETED → QUALIFICATION_PENDING

ARCHIVED → RECEIVED
ARCHIVED → QUALIFICATION_PENDING
ARCHIVED → POSITIVE_HANDOFF_PENDING
```

A later independent inquiry must normally create a new Lead.

### Entering RECEIVED

Requires:

- Trusted Tenant context.
- Source.
- Receipt time.
- Original evidence or approved manual-entry context.
- Deduplication evaluation.
- Creation actor.
- Data classification.
- Audit evidence.

### Entering NORMALIZING

Requires:

- Original evidence.
- Approved normalization process.
- Preserved original values.
- Normalization actor or service.
- No mutation of original evidence.

### Entering VALIDATING

Requires:

- Normalized intake projection.
- Source validation.
- Contact-path evaluation.
- Customer-resolution initiation.
- Duplicate assessment.
- Intent assessment.
- Permission assessment where applicable.

### Entering REVIEW_REQUIRED

Requires:

- Review reason.
- Review type.
- Frozen review context.
- Assigned authorized role or queue.
- Evidence.
- Due time where applicable.
- Audit evidence.

### Entering QUALIFICATION_PENDING

Requires:

- Sufficient automotive-intent basis.
- Qualification request.
- Applied qualification policy and version.
- Evidence collection.
- Missing-information state.
- Customer-resolution state.
- Contact-path state.
- Permission context.
- Human Review state.
- Idempotency.
- Audit evidence.

### Entering POSITIVE_HANDOFF_PENDING

Requires:

- Accepted positive qualification Decision.
- Decision identifier.
- Decision authority.
- Resolved Customer.
- Current Lead version.
- Evidence snapshot hash.
- Policy and version.
- Required Review completed.
- Qualified Lead creation request.
- Idempotency key.
- Audit evidence.

This state does not prove that a Qualified Lead exists.

### Entering QUALIFIED_HANDOFF_COMPLETED

Requires:

- Confirmed `qualified_lead_id`.
- Qualification cycle.
- Qualified Lead creation evidence.
- Qualified Lead creation Event or equivalent Confirmation.
- Matching Tenant.
- Matching Lead.
- Matching Customer.
- Reconciliation completed.
- Completion timestamp.

This state does not prove Opportunity or Deal creation.

### Entering DISQUALIFIED

Requires:

- Accepted negative Decision.
- Disqualification reason.
- Evidence.
- Authority.
- Actor.
- Timestamp.
- Review where required.

### Entering DUPLICATE

Requires:

- Confirmed duplicate Decision.
- Surviving Lead reference.
- Evidence.
- Same-Tenant validation.
- No circular relationship.
- Audit evidence.

### Entering INVALID

Requires:

- Invalidity reason.
- Evidence.
- Authority.
- Actor.
- Timestamp.
- Security workflow where applicable.

### Entering WITHDRAWN

Requires:

- Withdrawal evidence or authorized Decision.
- Scope.
- Permission-impact evaluation.
- Downstream-impact evaluation.
- Timestamp.

### Entering EXPIRED

Requires:

- Applicable expiration policy.
- Expiration reason.
- Authoritative time.
- No completed positive handoff.
- Audit evidence.

### Entering ARCHIVED

Requires:

- Valid retention or closure condition.
- No unresolved active workflow.
- Preservation of required evidence.
- Archive timestamp.
- Access classification.

### Terminal States

For ordinary Lead processing:

- `QUALIFIED_HANDOFF_COMPLETED`
- `DISQUALIFIED`
- `DUPLICATE`
- `INVALID`
- `WITHDRAWN`
- `EXPIRED`
- `ARCHIVED`

Terminal records may receive governed:

- Correction.
- Reconciliation.
- Privacy processing.
- Legal hold.
- Archival updates.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Lead identifier.
- Customer reference where applicable.
- Qualified Lead reference where applicable.
- Record version.
- Policy and version.
- Decision.
- Evidence.
- Human Review.
- Recommendation where applicable.
- Idempotency key where applicable.
- Correlation identifier.
- Causation identifier.
- Timestamp.
- Related Event.

---

## 7. Relationships

### Tenant

- Every Lead belongs to exactly one `tenant_id`.
- Every Tenant-owned relationship must match that Tenant.
- Cross-Tenant Lead processing is prohibited by default.

### Customer

- A Lead may initially have no resolved Customer.
- Customer Domain Service owns canonical identity.
- Lead stores submitted identity observations.
- Lead may reference one resolved Customer.
- Customer correction must not rewrite original Lead evidence.
- Customer merge must preserve Lead relationships.
- Positive qualification normally requires a resolved Customer.

### Qualified Lead

- A Lead may produce one current Qualified Lead per accepted qualification cycle.
- Lead owns the creation request.
- Qualified Lead Domain Service owns Qualified Lead creation.
- Lead stores the confirmed `qualified_lead_id`.
- Qualified Lead owns positive qualification validity and revalidation.
- Lead must not duplicate the Qualified Lead snapshot.
- Failed handoff must remain explicit.
- Handoff must be idempotent.

### Opportunity

- Lead does not create Opportunity directly.
- Qualified Lead conversion creates Opportunity.
- Opportunity Domain Service owns Opportunity creation and pipeline.
- Lead may store an informational Opportunity projection after confirmed linkage.
- Opportunity changes must not rewrite Lead intake evidence.

### Interaction

Interaction may provide:

- Original communication evidence.
- Response evidence.
- Contact-attempt evidence.
- Qualification evidence.
- Customer withdrawal evidence.
- Opt-out evidence.
- Appointment interest.
- Finance interest.
- Trade-In interest.

Interaction owns communication evidence.

Lead stores references and projections.

### Vehicle

Lead may reference submitted or resolved Vehicle interest.

Vehicle Domain Service owns canonical identity.

Inventory Record owns availability.

### Inventory Record

Lead must not:

- Reserve Inventory.
- Allocate Inventory.
- Transfer Inventory.
- Mark Inventory sold.
- Mark Inventory delivered.

### Appointment

Lead may request an Appointment.

Appointment Domain Service owns creation, scheduling, Confirmation, and attendance.

### Quotation

Lead may request a Quotation.

Quotation Domain Service owns Quotation identity, commercial terms, issuance, acceptance, and expiration.

### Trade-In

Lead may contain Trade-In interest.

Trade-In Domain Service owns Trade-In workflow.

### Finance Application

Lead may contain finance interest.

Finance Application Domain Service owns Applicant and Lender workflows.

### Financial Contract

Lead does not own Financial Contract terms, signatures, effectiveness, activation, or funding.

### Deal

Lead does not own Deal state.

The final commercial result may be traced through:

```text
Lead
  → Qualified Lead
  → Opportunity
  → Deal
```

### Campaign

Lead may reference campaign attribution.

Campaign relationship must preserve source evidence.

Campaign attribution must not override original source evidence.

### Human Decision

Lead may reference Decisions concerning:

- Duplicate status.
- Invalidity.
- Disqualification.
- Withdrawal.
- Qualification Recommendation review.
- Positive qualification.
- Source exception.
- Contact-permission exception.
- Data-quality exception.

### Recommendation

Recommendations may propose:

- Qualification outcome.
- Missing information.
- Priority.
- Assignment.
- Response.
- Human Review.

Recommendations remain non-binding.

### AI Agent Run

AI Agent Runs may reference:

- Lead.
- Customer candidate.
- Evidence.
- Input versions.
- Model.
- Prompt.
- Output.
- Recommendation.
- Review.

AI output remains Derived Intelligence.

### External CRM

A configured CRM may be authoritative for configured:

- External Lead status.
- External assignment.
- External qualification workflow.
- External Qualified Lead creation.
- External pipeline handoff.

ASOS must preserve:

- External reference.
- External version.
- Command.
- Idempotency.
- External Confirmation.
- Conflict.
- Reconciliation.

### Supporting Child Records

Lead may own or govern:

- Source records.
- Source-delivery records.
- Original evidence references.
- Submitted party observations.
- Inquiry snapshots.
- Attribution records.
- Customer-resolution requests.
- Contact-validation records.
- Permission projections.
- Duplicate assessments.
- Intent assessments.
- Qualification requests.
- Qualification evidence packages.
- Positive handoff requests.
- Negative outcome records.
- Assignment history.
- Response projections.
- Human Reviews.
- Derived Intelligence.
- Data-quality issues.
- Conflicts.
- Reconciliation cases.
- Status history.
- Audit history.

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
- Correction and reversal behaviour.

The following are required Lead Event concepts and do not replace the Event Catalog.

### Intake Event Concepts

- Lead intake observed.
- Lead creation requested.
- Lead created.
- Lead source duplicate detected.
- Lead normalization completed.
- Lead validation completed.
- Lead source authentication failed.
- Lead evidence integrity failed.

### Identity and Contact Event Concepts

- Lead Customer resolution requested.
- Lead Customer resolved.
- Lead identity conflict detected.
- Lead contact validation completed.
- Lead contact became invalid.
- Lead communication permission evaluated.
- Lead communication permission conflict detected.

### Duplicate Event Concepts

- Lead duplicate assessment requested.
- Potential duplicate detected.
- Lead confirmed as duplicate.
- Lead duplicate Decision rejected.
- Lead duplicate relationship corrected.

### Intent Event Concepts

- Lead automotive intent assessed.
- Lead non-automotive routing requested.
- Lead spam risk detected.
- Lead fraud risk detected.
- Lead compliance review requested.

### Qualification Request Event Concepts

- Lead qualification requested.
- Lead qualification evidence requested.
- Lead qualification evidence updated.
- Lead qualification evidence became ready.
- Lead qualification evidence conflict detected.
- Lead qualification Decision requested.
- Lead qualification Human Review requested.
- Additional qualification information requested.

### Positive Handoff Event Concepts

- Positive qualification Decision observed.
- Qualified Lead creation requested.
- Qualified Lead creation pending Confirmation.
- Qualified Lead creation rejected.
- Qualified Lead creation failed.
- Qualified Lead creation reconciliation required.
- Lead positive handoff completed.

Lead Domain Service must not publish the authoritative Qualified Lead-created Event.

Qualified Lead Domain Service publishes accepted Qualified Lead creation.

### Negative Outcome Event Concepts

- Lead disqualified.
- Lead marked invalid.
- Lead confirmed as duplicate.
- Lead withdrawn.
- Lead expired.
- Lead transferred to another workflow.

### Assignment and Response Event Concepts

- Lead assignment requested.
- Lead assigned.
- Lead reassigned.
- Lead returned to queue.
- Lead response required.
- Lead response SLA breached.
- Lead first response recorded.

### Data-Quality Event Concepts

- Lead data-quality issue opened.
- Lead data-quality issue resolved.
- Lead conflict detected.
- Lead reconciliation requested.
- Lead reconciliation completed.
- Lead correction recorded.

### AI and Recommendation Event Concepts

- Lead score generated.
- Qualification score generated.
- Automotive intent classified.
- Duplicate Recommendation generated.
- Qualification Recommendation generated.
- Assignment Recommendation generated.
- Response Recommendation generated.
- Lead Human Review recommended.

Derived Intelligence Events must not imply:

- Customer identity resolution.
- Contact permission.
- Positive qualification Decision.
- Qualified Lead creation.
- Opportunity creation.
- Appointment confirmation.
- Quotation issuance.
- Finance approval.
- Trade-In appraisal.
- Deal creation.
- Human Approval.

### Producer Rules

- Lead Domain Service publishes accepted Lead intake, evidence, request, handoff, negative outcome, assignment, and data-quality facts.
- Customer Domain Service publishes accepted Customer identity and permission facts.
- Qualified Lead Domain Service publishes accepted Qualified Lead creation, validity, and revalidation facts.
- Opportunity Domain Service publishes accepted Opportunity facts.
- Interaction Domain Service publishes accepted communication facts.
- Policy and Decision services publish applicable Decision facts.
- AI Agents may publish Agent-run, score, classification, extraction, and Recommendation Events.
- AI Agents must not publish authoritative positive qualification or Qualified Lead Events merely because they predicted the outcome.

### Event Requirements

Every material Lead Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `lead_id`.
- Customer identifier where applicable.
- Qualified Lead identifier where applicable.
- Lead record version.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Source references.
- Original evidence hash.
- Policy and version.
- Decision.
- Human Review.
- Evidence references.
- Recommendation where applicable.
- Idempotency key where applicable.
- Correlation identifier.
- Causation identifier.
- Security classification.

Events are immutable.

Corrections, duplicate decisions, negative outcomes, handoff failures, and reconciliation outcomes require new linked Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate effects using `event_id`.

Command and request retries must use the applicable `idempotency_key`.

Source-delivery deduplication remains a separate concern.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Language detection.
- Inquiry classification.
- Automotive-intent classification.
- Vehicle-interest extraction.
- Budget-text extraction.
- Purchase-timeframe extraction.
- Finance-interest detection.
- Trade-In-interest detection.
- Test-drive-interest detection.
- Appointment-interest detection.
- Lead scoring.
- Qualification scoring.
- Duplicate detection.
- Spam detection.
- Fraud-risk detection.
- Contactability estimation.
- Urgency estimation.
- Missing-information detection.
- Evidence summarization.
- Qualification Recommendation.
- Assignment Recommendation.
- Response drafting.
- Human Review preparation.
- Management-summary generation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Resolve ambiguous Customer identity.
- Merge Customers.
- Confirm a duplicate Lead without governed authority.
- Create Consent.
- Ignore an opt-out.
- Create a positive qualification Decision.
- Create a Qualified Lead from their own score.
- Disqualify a Lead as the sole authority where Human authority is required.
- Mark a Lead invalid without applicable authority.
- Waive qualification criteria.
- Create an Opportunity.
- Assign an Opportunity owner.
- Contact a Customer without applicable authority.
- Reserve or allocate a Vehicle.
- Create a Trade-In appraisal.
- Submit a Finance Application.
- Promise pricing or finance terms.
- Create a Deal.
- Execute external Commands directly.
- Access another Tenant’s data.

### AI Output Requirements

Every material AI output must preserve:

- Output type.
- Lead identifier.
- Lead record version.
- Customer reference where applicable.
- Input fields.
- Evidence.
- Evidence spans where meaningful.
- Source authority.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Confidence where meaningful.
- Assumptions.
- Limitations.
- Generation timestamp.
- Expiration timestamp.
- Action Class.
- Required Human authority or automation policy.

### Evidence Grounding

AI must distinguish:

- Direct Customer statement.
- Original Lead evidence.
- Normalized observation.
- Extracted value.
- Deterministic calculation.
- Prediction.
- Recommendation.
- Human Decision.
- Canonical Lead fact.

AI must not invent:

- Customer identity.
- Contact ownership.
- Customer budget.
- Purchase date.
- Vehicle requirement.
- Consent.
- Representative authority.
- Finance eligibility.
- Trade-In ownership.
- Customer commitment.

### Qualification Analysis

AI may recommend:

- More information.
- Human Review.
- Positive qualification.
- Disqualification.
- Invalidity.
- Duplicate review.
- Non-sales routing.

The output remains a Recommendation.

High confidence must not override:

- Weak evidence.
- Identity conflict.
- Active opt-out.
- Compliance block.
- Human Review.
- Policy restrictions.
- Evidence-integrity failure.

### Duplicate Analysis

AI may identify possible duplicate Leads.

It must preserve:

- Candidate Leads.
- Evidence.
- Similarity method.
- Model version.
- Differences.
- Confidence where meaningful.
- Required review.

AI must not merge Leads or Customers.

### Response Drafting

AI-generated Drafts must:

- Use current authoritative data.
- Respect communication purpose.
- Respect contact permission.
- Avoid unsupported availability.
- Avoid invented pricing.
- Avoid invented finance approval.
- Avoid invented Appointment confirmation.
- Avoid invented Customer identity.
- Exclude internal restricted data.
- Remain Drafts until authorized.

### Action Class 2

Controlled internal actions or Customer communication may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Lead status.
- Customer-resolution status.
- Contact point.
- Purpose.
- Channel.
- Consent or lawful basis.
- Opt-out.
- Quiet hours.
- Frequency.
- Template.
- Data sensitivity.
- Assignment scope.
- Risk limits.
- Audit requirements.

The AI Agent must not approve its own automation authority.

### Action Class 3

Binding or high-impact actions require an Authoritative Human Decision.

Examples include:

- Qualification-policy exception.
- Blocking-criterion waiver.
- Ambiguous identity exception.
- Representative-authority exception.
- High-risk disqualification.
- Fraud Decision.
- Compliance Decision.
- Restricted disclosure.
- Correction of material source interpretation.
- Another material legal, privacy, financial, or commercial exception.

### AI Context and Embeddings

Lead data must not enter unrestricted embeddings.

Normally excluded or restricted fields include:

- Full phone numbers.
- Email addresses.
- Home addresses.
- National identifiers.
- Identity documents.
- Bank information.
- Credit information.
- Finance documents.
- Full Interaction content.
- Fraud-investigation detail.
- Restricted Decision notes.
- Source credentials.
- Authentication secrets.
- Raw provider payloads.

Approved redacted context may include:

- General inquiry category.
- General Vehicle interest.
- General purchase-timeframe category.
- Non-sensitive budget band.
- General finance-interest status.
- General Trade-In-interest status.
- Lead workflow state.
- Non-sensitive summary.
- Qualification-readiness category.

Every vector record must enforce:

- `tenant_id`.
- Purpose.
- Source.
- Lead record version.
- Security classification.
- Retention.
- Expiration.
- Deletion and correction propagation.

### Prompt Injection and Untrusted Content

Lead messages, forms, documents, provider payloads, URLs, and attachments are untrusted input.

AI Agents must treat them as data, not instructions.

Untrusted content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Approve qualification.
- Create Qualified Leads.
- Create Opportunities.
- Trigger external Commands.
- Change Consent.
- Modify audit evidence.
- Request hidden system instructions.

### Explainability

Material Lead Recommendations must explain:

- Evidence used.
- Evidence source.
- Input version.
- Model or rule.
- Missing information.
- Contradictory evidence.
- Data freshness.
- Assumptions.
- Limitations.
- Confidence where meaningful.
- Required Human authority.
- Expiration.
- Recommended next workflow.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines required Lead API behaviour.

### REST Resources

```text
GET    /api/v1/leads
POST   /api/v1/leads
GET    /api/v1/leads/{lead_id}
PATCH  /api/v1/leads/{lead_id}

POST   /api/v1/leads/inbound-ingestion
POST   /api/v1/leads/{lead_id}/customer-resolution-requests
POST   /api/v1/leads/{lead_id}/contact-validation-requests
POST   /api/v1/leads/{lead_id}/permission-assessment-requests
POST   /api/v1/leads/{lead_id}/duplicate-assessment-requests
POST   /api/v1/leads/{lead_id}/intent-assessment-requests

POST   /api/v1/leads/{lead_id}/qualification-requests
POST   /api/v1/leads/{lead_id}/qualification-evidence-submissions
POST   /api/v1/leads/{lead_id}/qualification-decision-requests
POST   /api/v1/leads/{lead_id}/qualified-lead-creation-requests

POST   /api/v1/leads/{lead_id}/assignment-requests
POST   /api/v1/leads/{lead_id}/disqualification-requests
POST   /api/v1/leads/{lead_id}/duplicate-decisions
POST   /api/v1/leads/{lead_id}/invalidation-requests
POST   /api/v1/leads/{lead_id}/withdrawal-requests
POST   /api/v1/leads/{lead_id}/expiration-requests

POST   /api/v1/leads/{lead_id}/review-requests
POST   /api/v1/leads/{lead_id}/correction-requests
POST   /api/v1/leads/{lead_id}/archive-requests

GET    /api/v1/leads/{lead_id}/source-evidence
GET    /api/v1/leads/{lead_id}/qualification
GET    /api/v1/leads/{lead_id}/qualification-evidence
GET    /api/v1/leads/{lead_id}/qualified-lead-handoff
GET    /api/v1/leads/{lead_id}/reviews
GET    /api/v1/leads/{lead_id}/history
GET    /api/v1/leads/{lead_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, queue, User, Customer, Qualified Lead, and Interaction scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Example Manual Lead Creation

```json
{
  "source_channel": "WALK_IN",
  "source_system": "DEALERSHIP_MANUAL_ENTRY",
  "source_authority": "AUTHORIZED_HUMAN_ENTRY",
  "received_at": "2026-08-01T18:45:00Z",
  "submitted_party": {
    "submitted_display_name": "Submitted Customer Name",
    "submitted_phone": "+201000000000",
    "submitted_preferred_language": "ar-EG"
  },
  "inquiry": {
    "inquiry_category": "VEHICLE_PURCHASE",
    "requested_action": "REQUEST_INFORMATION",
    "vehicle_interest_text": "Customer is interested in a new compact SUV."
  },
  "original_evidence": {
    "original_content_reference": "evidence://manual-intake/2026/000918",
    "original_content_hash": "sha256:d9ba41b2..."
  }
}
```

The request must include:

```text
Idempotency-Key: 680b8847-6eaf-4870-bdce-7192337af9e7
```

### Example Creation Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "lead_number": "LEAD-2026-009184",
  "status": "RECEIVED",
  "identity_resolution_status": "NOT_STARTED",
  "contact_validation_status": "NOT_STARTED",
  "qualification_status": "NOT_REQUESTED",
  "qualification_handoff_status": "NOT_APPLICABLE",
  "record_version": 1,
  "created_at": "2026-08-01T18:45:01Z"
}
```

### Example Provider Ingestion

```json
{
  "provider_id": "lead-provider-01",
  "provider_account_id": "account-eg-01",
  "provider_record_id": "provider-lead-7771",
  "provider_event_id": "provider-event-99818",
  "source_channel": "MARKETPLACE",
  "source_occurred_at": "2026-08-01T18:52:00Z",
  "source_received_at": "2026-08-01T18:52:02Z",
  "payload_reference": "evidence://provider-leads/provider-event-99818",
  "payload_hash": "sha256:1163ef77..."
}
```

Provider ingestion must use provider-specific authentication and deduplication controls.

### Example Qualification Request

```json
{
  "qualification_policy_id": "dcbfa69f-3bdd-43f6-8923-7d258a9615ab",
  "qualification_policy_version": "2.3.0",
  "expected_record_version": 7,
  "evidence_requirements": [
    "RESOLVED_CUSTOMER",
    "VALID_CONTACT_PATH",
    "AUTOMOTIVE_INTENT",
    "PURCHASE_TIMEFRAME",
    "VEHICLE_REQUIREMENT"
  ]
}
```

The request must include an idempotency key.

### Example Qualification-Pending Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "QUALIFICATION_PENDING",
  "qualification_request_id": "3c9355aa-71a8-48ce-8909-8958446e5b52",
  "qualification_request_status": "EVIDENCE_COLLECTION",
  "qualification_readiness_status": "MISSING_INFORMATION",
  "qualification_missing_information": [
    "PURCHASE_TIMEFRAME"
  ],
  "record_version": 8
}
```

### Example Decision-Ready Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "QUALIFICATION_PENDING",
  "qualification_request_status": "READY_FOR_DECISION",
  "qualification_readiness_status": "READY",
  "qualification_evidence_package_id": "5f3028ad-1555-420c-9ab1-25aef174d4c3",
  "qualification_evidence_snapshot_hash": "sha256:6e5b28d2...",
  "record_version": 11
}
```

### Positive Qualification Decision Boundary

The authoritative positive Decision is accepted through the applicable Decision workflow.

Lead API stores the Decision reference and requests Qualified Lead creation.

The Lead API does not create the canonical positive qualification snapshot itself.

### Example Qualified Lead Creation Request

```json
{
  "qualification_request_id": "3c9355aa-71a8-48ce-8909-8958446e5b52",
  "qualification_decision_id": "dd64f44f-e317-49d3-8f66-62fa8d2a9d42",
  "expected_qualification_evidence_snapshot_hash": "sha256:6e5b28d2...",
  "expected_record_version": 12
}
```

The request must include:

```text
Idempotency-Key: 945270b5-e58d-45e8-b500-8c79c799ba3a
```

### Example Positive Handoff-Pending Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "POSITIVE_HANDOFF_PENDING",
  "qualification_outcome_projection": "POSITIVE_DECISION_ACCEPTED",
  "qualification_handoff_status": "PENDING_QUALIFIED_LEAD_CONFIRMATION",
  "qualified_lead_creation_request_id": "915d9490-862a-439c-b73f-c753b35c6018",
  "record_version": 13
}
```

The response must not claim that the Qualified Lead exists.

### Example Confirmed Handoff Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "QUALIFIED_HANDOFF_COMPLETED",
  "qualification_outcome_projection": "POSITIVE_DECISION_ACCEPTED",
  "qualification_handoff_status": "CONFIRMED",
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "qualification_cycle_id": "4a4593f3-69c0-4589-887a-fced0d77c781",
  "qualified_lead_creation_confirmation_status": "RECEIVED",
  "qualified_lead_creation_event_id": "018f9d93-209d-7000-a4ea-09018b1a1da2",
  "record_version": 14
}
```

### Example Disqualification Request

```json
{
  "disqualification_reason": "CUSTOMER_NOT_READY",
  "negative_outcome_decision_id": "26f925cb-2385-4e06-99ce-092705087e06",
  "negative_outcome_evidence_references": [
    "evidence://interactions/afc34b9e/content"
  ],
  "expected_record_version": 11
}
```

### Example Duplicate Decision

```json
{
  "duplicate_of_lead_id": "9dbdd463-6436-49d7-b88c-8914dbb2dc97",
  "duplicate_decision_id": "f9fc8958-778f-453a-9e55-0935e015eaed",
  "duplicate_evidence_references": [
    "evidence://lead-duplicate-assessments/87421"
  ],
  "expected_record_version": 6
}
```

Duplicate handling must not merge Customers.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Source authority.
- Evidence integrity.
- Customer-resolution rules.
- Contact-validation rules.
- Communication-permission rules.
- Duplicate rules.
- Qualification ownership boundary.
- Qualification Decision authority.
- Lifecycle validation.
- Human Review.
- Idempotency.
- Audit recording.
- Event publication after accepted state change.
- Qualified Lead Confirmation tracking where applicable.

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

- Leads.
- Qualification requests.
- Evidence packages.
- Human Reviews.
- Qualified Lead creation requests.
- Qualified Leads.
- Assignments.
- Negative outcomes.
- Corrections.

### Pending Confirmation

Qualified Lead creation may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "qualified_lead_creation_request_id": "915d9490-862a-439c-b73f-c753b35c6018",
  "record_version": 13
}
```

The API must not describe the handoff as completed until Qualified Lead Domain Service confirms creation.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_SOURCE_DELIVERY`
- `DUPLICATE_LEAD`
- `SOURCE_AUTHENTICATION_FAILED`
- `SOURCE_INTEGRITY_FAILED`
- `SOURCE_SCHEMA_INVALID`
- `SOURCE_NOT_PERMITTED`
- `ORIGINAL_EVIDENCE_REQUIRED`
- `EVIDENCE_INTEGRITY_FAILED`
- `CONTACT_PATH_REQUIRED`
- `CUSTOMER_IDENTITY_CONFLICT`
- `CONTACT_VALIDATION_FAILED`
- `CONTACT_PERMISSION_NOT_GRANTED`
- `OPT_OUT_ACTIVE`
- `QUALIFICATION_REQUEST_REQUIRED`
- `QUALIFICATION_EVIDENCE_REQUIRED`
- `QUALIFICATION_EVIDENCE_INCOMPLETE`
- `QUALIFICATION_DECISION_REQUIRED`
- `QUALIFICATION_DECISION_NOT_POSITIVE`
- `QUALIFICATION_POLICY_EXPIRED`
- `HUMAN_REVIEW_REQUIRED`
- `QUALIFIED_LEAD_CREATION_PENDING`
- `QUALIFIED_LEAD_CREATION_REJECTED`
- `QUALIFIED_LEAD_ALREADY_EXISTS`
- `INVALID_LIFECYCLE_TRANSITION`
- `LEAD_ALREADY_TERMINAL`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Source authority.
- Evidence integrity.
- Customer identity boundary.
- Communication permission.
- Qualification ownership.
- Decision authority.
- Concurrency.
- Idempotency.
- Qualified Lead Confirmation.
- Human Review.
- Audit requirements.

GraphQL resolvers must not bypass Lead Domain Service, Customer Domain Service, Policy Engine, Decision controls, Qualified Lead Domain Service, or Interaction Domain Service.

---

## 11. Database Design

### Recommended Tables

```text
leads
lead_source_records
lead_source_deliveries
lead_original_evidence
lead_submitted_party_observations
lead_inquiry_snapshots
lead_marketing_attribution
lead_customer_resolution_requests
lead_contact_validation_records
lead_permission_projections
lead_duplicate_assessments
lead_intent_assessments
lead_qualification_requests
lead_qualification_evidence_packages
lead_qualification_evidence_items
lead_qualification_decision_references
lead_qualified_lead_creation_requests
lead_qualified_lead_confirmations
lead_negative_outcomes
lead_assignments
lead_response_sla
lead_review_requests
lead_review_decisions
lead_derived_intelligence
lead_data_quality_issues
lead_conflicts
lead_corrections
lead_reconciliation_cases
lead_status_history
lead_record_versions
lead_audit_log
```

### Leads Table

The `leads` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Customer and journey relationships.
- Current intake lifecycle state.
- Current source projection.
- Current Customer-resolution projection.
- Current contact-validation projection.
- Current permission projection.
- Current duplicate projection.
- Current intent projection.
- Current qualification-request projection.
- Current positive-handoff projection.
- Current negative-outcome projection.
- Current assignment and response projection.
- Current data-quality, conflict, and review state.
- Source and synchronization state.
- Record version.
- Audit timestamps.

Immutable evidence, repeating records, and historical decisions must remain in child tables.

### Primary Key

```text
PRIMARY KEY (lead_id)
```

### Tenant Protection

Every Lead-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_leads_tenant_status
  (tenant_id, status)

idx_leads_tenant_customer
  (tenant_id, customer_id, received_at)

idx_leads_tenant_qualified_lead
  (tenant_id, qualified_lead_id)

idx_leads_tenant_source
  (tenant_id, source_system, source_record_id)

idx_leads_tenant_channel
  (tenant_id, source_channel, received_at)

idx_leads_tenant_assignment
  (tenant_id, assignment_status, assigned_owner_user_id)

idx_leads_tenant_qualification
  (tenant_id, qualification_status, status)

idx_leads_tenant_handoff
  (tenant_id, qualification_handoff_status)

idx_leads_tenant_review
  (tenant_id, review_status)

idx_leads_tenant_data_quality
  (tenant_id, data_quality_status, conflict_status)

idx_leads_first_response_due
  (tenant_id, response_sla_status, first_response_due_at)

idx_leads_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, lead_number)
```

Source-delivery uniqueness may use:

```text
UNIQUE (
  tenant_id,
  source_system,
  source_provider_account_id,
  source_event_id
)
```

or the appropriate source-specific deduplication key.

One confirmed Qualified Lead relationship should not be attached to multiple unrelated Lead handoffs.

The applicable relationship constraint must preserve legitimate qualification-cycle rules.

### Source Storage

Source records should preserve:

- Source system.
- Provider account.
- Source record.
- Source Event.
- Original payload.
- Hash.
- Occurrence time.
- Receipt time.
- Connector.
- Connector version.
- Authentication result.
- Deduplication result.
- Security classification.
- Related Events.

### Original Evidence Storage

Original evidence records should preserve:

- Lead.
- Evidence type.
- Controlled reference.
- Hash.
- MIME type.
- Language.
- Source authority.
- Occurrence time.
- Security classification.
- Retention.
- Legal hold.
- Related Events.

Original evidence must be immutable.

### Submitted Party Storage

Submitted party observations should remain separate from canonical Customer records.

They should preserve:

- Original value.
- Normalized value.
- Source.
- Timestamp.
- Verification state.
- Customer-resolution relationship.
- Correction lineage.
- Security classification.

### Qualification Request Storage

Qualification-request records should preserve:

- Lead.
- Policy and version.
- Request actor.
- Requested time.
- Evidence requirements.
- Readiness.
- Missing information.
- Decision request.
- Status.
- Idempotency key.
- Failure.
- Related Events.

### Qualification Evidence Storage

Evidence-package and evidence-item tables should preserve:

- Lead.
- Qualification request.
- Source.
- Source authority.
- Evidence type.
- Evidence role.
- Reference.
- Hash.
- Observation time.
- Verification.
- Freshness.
- Inclusion or exclusion.
- Security classification.
- Retention.
- Legal hold.
- Related Events.

### Decision Reference Storage

Lead stores references and projections for qualification Decisions.

The canonical Decision record may live in the responsible Decision service.

Lead decision-reference records should preserve:

- Decision identifier.
- Decision type.
- Result projection.
- Authority.
- Actor.
- Policy.
- Evidence references.
- Timestamp.
- Expiration.
- Snapshot hash.
- Related Events.

### Qualified Lead Handoff Storage

Qualified Lead creation-request records should preserve:

- Lead.
- Customer.
- Qualification request.
- Positive Decision.
- Evidence snapshot hash.
- Policy and version.
- Request actor.
- Request time.
- Idempotency key.
- Status.
- Qualified Lead reference.
- Qualification cycle.
- Confirmation.
- Event reference.
- Failure.
- Reconciliation.
- Related Events.

### Negative Outcome Storage

Negative-outcome records should preserve:

- Lead.
- Outcome.
- Reason.
- Decision.
- Authority.
- Actor.
- Evidence.
- Effective time.
- Review.
- Re-entry policy.
- Related Events.

### Derived Intelligence Storage

Derived records must remain separate from authoritative workflow fields.

Each derived record should preserve:

- Output type.
- Value.
- Model, formula, or algorithm.
- Version.
- Prompt version.
- Input records and versions.
- Evidence.
- Confidence.
- Assumptions.
- Limitations.
- Generation time.
- Expiration.
- Review status.
- Related Events.

### Audit Storage

Lead audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw sensitive values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Receipt date.
- Source.
- Channel.
- Status.
- Dealership.
- Region.
- Retention class.
- Security classification.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Source deduplication.
- Qualified Lead handoff idempotency.
- Evidence immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Lead must not be hard-deleted when referenced by:

- Customer Journey.
- Qualified Lead.
- Opportunity.
- Appointment.
- Quotation.
- Trade-In.
- Finance Application.
- Deal.
- Interaction.
- Campaign.
- Human Decision.
- Recommendation.
- AI Agent Run.
- Command.
- Event.
- External Confirmation.
- Reconciliation.
- Audit evidence.
- Legal hold.

Archival, anonymization, governed redaction, or legally required deletion workflows must be used instead.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Submitted name, phone, email, account identifiers |
| `CUSTOMER_COMMERCIAL_CONTEXT` | Inquiry, Vehicle interest, timeframe |
| `CUSTOMER_FINANCIAL_CONTEXT` | Budget and finance-interest text |
| `IDENTITY_RESTRICTED` | Customer-resolution evidence |
| `CONSENT_AND_PERMISSION` | Permission and opt-out evidence |
| `FRAUD_AND_COMPLIANCE_RESTRICTED` | Fraud and compliance review data |
| `SOURCE_CONFIDENTIAL` | Provider payloads and source metadata |
| `INTERNAL_ROUTING` | Assignment and routing context |
| `DERIVED_INTELLIGENCE` | Scores, classifications, summaries |
| `AUTHORITATIVE_DECISION` | Negative outcomes and Decision references |
| `AUDIT_EVIDENCE` | Events, versions, Commands, and history |

### Authentication

Every internal Lead operation requires an authenticated Human or service identity.

Anonymous creation is permitted only through an approved inbound source and restricted unresolved-identity workflow.

Anonymous internal modification, qualification approval, negative outcome, assignment, or handoff is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealer group.
- Dealership.
- Branch.
- Team.
- Queue.
- User role.
- Customer relationship.
- Lead status.
- Requested field.
- Requested action.
- Source.
- Security classification.
- Business purpose.
- Delegated authority.
- Legal hold.
- External CRM authority.

### Example Role Boundaries

#### Lead Intake User

May access permitted:

- Incoming Lead.
- Submitted party information.
- Inquiry content.
- Contact validation.
- Customer-resolution request.
- Missing qualification information.

Must not independently:

- Approve restricted qualification exceptions.
- Merge Customers.
- Override opt-out.
- Create a Qualified Lead without an accepted Decision.
- View restricted fraud or finance information without authority.

#### Sales Consultant

May access assigned or permitted:

- Lead summary.
- Customer relationship.
- Vehicle interest.
- General timeframe.
- General finance or Trade-In interest.
- Contact-permission projection.
- Qualification-request state.

Must not access without authority:

- Identity documents.
- Detailed fraud investigation.
- Restricted compliance notes.
- Full provider payloads.
- Another team’s restricted Leads.

#### Sales Manager

May access permitted team Leads and perform configured:

- Intake review.
- Assignment Decision.
- Qualification review.
- Negative-outcome review.
- Handoff-exception review.

Manager access does not bypass Tenant, security, Consent, fraud, compliance, or evidence-integrity controls.

#### Data Steward

May review:

- Source mappings.
- Duplicate assessments.
- Customer relationships.
- Evidence integrity.
- Data-quality issues.
- Conflicts.
- Reconciliation.
- Lineage.

Raw sensitive evidence access should remain minimized.

#### Compliance or Fraud Reviewer

May access restricted evidence required for an assigned review.

Access must remain purpose-limited and audited.

#### AI Agent

May access only the minimum Lead context required for an approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to identity documents, finance records, fraud investigations, Decision notes, provider payloads, and full Interaction content.

#### Integration Service

May access only fields required for an approved source, CRM, analytics, or Domain integration.

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
- Messaging identifiers.
- IP address references.
- Identity evidence.
- Budget details.
- Finance-interest details.
- Fraud evidence.
- Compliance notes.
- Provider payloads.
- Decision notes.

### Contact-Point Protection

Contact points must:

- Use tokenization or equivalent protection where possible.
- Be masked when full display is unnecessary.
- Be restricted by purpose.
- Be excluded from ordinary Logs.
- Be excluded from unrestricted embeddings.
- Be protected against bulk export.
- Preserve verification state.
- Preserve source.
- Not be treated as permanent Customer identity.

### Source Credential Protection

Provider credentials, API keys, tokens, signing secrets, and webhook secrets must:

- Remain in approved secret-management systems.
- Never appear in Lead records.
- Never appear in Events.
- Never appear in Prompts.
- Never appear in ordinary Logs.
- Use rotation.
- Use least privilege.
- Use audited service identities.

### Webhook and Source Security

External Lead ingestion must use applicable:

- Signature verification.
- Timestamp validation.
- Replay protection.
- Provider-account validation.
- Trusted Tenant routing.
- Schema validation.
- Payload-size limits.
- Rate limiting.
- Deduplication.
- Malware scanning.
- Content validation.
- Security logging.

An external payload must not choose its own unrestricted `tenant_id`.

### Evidence Security

Lead evidence must:

- Use controlled storage.
- Preserve hashes.
- Prevent public indexing.
- Prevent unauthorized download.
- Follow retention.
- Support legal hold.
- Support governed correction.
- Prevent cross-Tenant access.
- Prevent unapproved AI-training use.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Customer matching.
- Duplicate matching.
- Search.
- Vector retrieval.
- Evidence.
- Queues.
- Caches.
- Events.
- Qualification.
- Handoff.
- Analytics.
- Exports.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### External CRM Security

External CRM integration must use:

- Approved service identity.
- Least-privilege credentials.
- Tenant-specific configuration.
- Field mapping.
- Version control.
- Command idempotency.
- External Confirmation.
- Reconciliation.
- Audit.
- Secret management.

External credentials must never appear in Lead records, Events, Prompts, or ordinary Logs.

### Handoff Security

A Qualified Lead creation request must include:

- Authenticated actor or service.
- `tenant_id`.
- Lead identifier.
- Lead record version.
- Customer reference.
- Qualification request.
- Positive Decision reference.
- Policy and version.
- Evidence snapshot hash.
- Idempotency key.
- Audit evidence.

Lead Domain Service must not directly manipulate Qualified Lead persistence.

### Command Security

Where an external CRM Command is required, it must include:

- Authenticated service identity.
- Tenant.
- Lead.
- Requested action.
- Current record version.
- Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Lead activity must record:

- `tenant_id`.
- `lead_id`.
- Customer reference where applicable.
- Qualified Lead reference where applicable.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous state.
- New state.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Authority category.
- Policy and version.
- Evidence hash.
- Decision.
- Human Review.
- AI involvement.
- Model and Prompt versions.
- Recommendation.
- Idempotency key.
- Command where applicable.
- Confirmation.
- Record version.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Events.

### Security Events

ASOS must detect and record applicable:

- Cross-Tenant Lead access attempt.
- Unauthorized Lead creation.
- Provider authentication failure.
- Webhook replay.
- Tenant-routing mismatch.
- Duplicate-source anomaly.
- Evidence-hash mismatch.
- Customer identity substitution.
- Contact-permission bypass.
- Opt-out bypass.
- Unauthorized positive handoff.
- Qualified Lead creation without Decision.
- Duplicate Qualified Lead creation attempt.
- Restricted evidence export.
- AI access outside approved scope.
- Prompt injection in Lead content.
- Audit-log tampering.

### Lead Integrity

The platform must detect or prevent:

- Source payload substitution.
- Original evidence modification.
- AI score represented as Decision.
- Recommendation represented as approval.
- Positive Decision represented as confirmed Qualified Lead.
- Lead represented as Qualified Lead.
- Multiple duplicate handoff effects.
- Customer identity substitution.
- Contact-permission substitution.
- Cross-Tenant Customer linkage.
- Lead status manipulation.
- Negative outcome without authority.
- Qualification handoff without evidence.

### Privacy and Retention

Lead retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Consent and permission state.
- Source requirements.
- Commercial-record requirements.
- Fraud and compliance requirements.
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
- AI context.
- Backups according to policy.

Required commercial, security, fraud, compliance, Decision, and audit evidence may remain only where lawful.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Lead ingestion.
- Specific source connectors.
- Automated Customer matching.
- Automated qualification analysis.
- Qualified Lead handoff.
- CRM synchronization.
- Customer communication.
- Lead export.
- AI Lead analysis.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Qualified Lead](./QualifiedLead.md)
- [ASOS Opportunity](./Opportunity.md)
- [ASOS Interaction](./Interaction.md)
- [ASOS Vehicle](./Vehicle.md)
- [ASOS Inventory Record](./InventoryRecord.md)
- [ASOS Appointment](./Appointment.md)
- [ASOS Quotation](./Quotation.md)
- [ASOS Trade-In](./TradeIn.md)
- [ASOS Finance Application](./FinanceApplication.md)
- [ASOS Deal](./Deal.md)

---

## Current Status

This document is the approved Canonical Lead baseline.

Lead owns original inquiry intake, source evidence, source deduplication, Customer-resolution initiation, contact validation, qualification requests, qualification evidence collection, pending qualification workflow, and negative outcomes.

Lead does not own the canonical positive qualification result.

Qualified Lead owns the accepted positive qualification Decision snapshot, qualification validity, freshness, revalidation, revocation, invalidation, and Opportunity-conversion eligibility.

A positive qualification Decision causes an idempotent Qualified Lead creation request.

Lead qualification handoff is not complete until Qualified Lead Domain Service confirms Qualified Lead creation.

The original Lead remains historically traceable after positive qualification and conversion.

A score or Recommendation is not a qualification Decision.

A requested Appointment, Quotation, Trade-In, finance workflow, Vehicle Reservation, or Deal is not an authoritative completed outcome.

Source-delivery deduplication, ASOS Event Consumer deduplication using `event_id`, and retryable request or Command protection using `idempotency_key` remain separate controls.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
