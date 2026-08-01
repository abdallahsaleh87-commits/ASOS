# Lead

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Lead Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Lead Object represents one captured expression of potential automotive interest, inquiry, referral, response, or requested dealership engagement.

A Lead may originate from:

- Website forms.
- Landing pages.
- OEM platforms.
- Marketplaces.
- Social media.
- Paid advertising.
- Messaging channels.
- Email.
- Phone calls.
- Showroom walk-ins.
- Events.
- Referrals.
- Campaign imports.
- Approved partner systems.
- Manual governed entry.
- Existing Customer re-engagement.

A Lead preserves the original commercial inquiry before qualification determines whether it should become a Qualified Lead or continue into another governed workflow.

A Lead may belong to:

- An unidentified person.
- A partially identified person.
- An existing Customer.
- A new prospective Customer.
- An organization.
- A fleet buyer.
- A representative acting for another party.

The Lead Object supports:

- Inquiry capture.
- Source attribution.
- Campaign attribution.
- Response-time measurement.
- Identity-resolution initiation.
- Contact-path validation.
- Automotive-intent assessment.
- Duplicate detection.
- Assignment.
- Qualification.
- Disqualification.
- Human Review.
- Consent and communication-basis checks.
- AI-assisted extraction and classification.
- Audit and evidence preservation.

### Lead, Customer, and Qualified Lead Separation

A Lead is not automatically a Customer.

A Lead records an inquiry or expression of interest.

A Customer represents a resolved canonical party identity and dealership relationship.

A Qualified Lead represents the result of an approved qualification process.

The relationships are:

```text
Lead
   ├── may resolve to an existing Customer
   ├── may create a new Customer after identity resolution
   ├── may remain unresolved
   ├── may be classified as duplicate, invalid, or disqualified
   └── may produce one Qualified Lead through a controlled qualification workflow
```

Lead qualification must not silently convert the Lead record into a Qualified Lead.

The original Lead must remain historically traceable after qualification.

### Lead and Interaction Separation

The Lead stores the original inquiry and its normalized intake context.

Subsequent communications belong to `Interaction`.

The Lead may contain projections such as:

- First-response time.
- Last-contact-attempt time.
- Contact-attempt count.
- Latest Interaction reference.

The original communication evidence and subsequent communication history must remain in governed Interaction or provider records.

### Lead and Opportunity Separation

A Lead must not contain authoritative:

- Opportunity pipeline stage.
- Negotiation state.
- Quotation terms.
- Discount approval.
- Reservation.
- Allocation.
- Finance Decision.
- Contract status.
- Deal status.
- Payment status.
- Delivery status.

Those responsibilities belong to their appropriate Canonical Domain Objects.

### System Purpose

The Lead Object is the canonical intake representation for potential automotive sales interest entering ASOS.

It provides:

- A normalized inquiry envelope.
- Immutable original-source evidence.
- Source and campaign attribution.
- Source deduplication.
- Identity-resolution inputs.
- Qualification inputs.
- Assignment and response projections.
- Data-quality state.
- AI-derived classifications.
- Human Review state.
- Tenant-safe context for approved workflows.

The Lead Object may include:

- External Authoritative Data.
- ASOS Canonical Projection.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

The original source remains authoritative for the original submitted payload and source-delivery evidence.

ASOS is authoritative for its internal:

- Lead workflow state.
- Qualification workflow.
- Human Review tasks.
- Assignment workflow.
- Data-quality state.
- Recommendation records.
- Derived scores and classifications.

### Core Authority Boundaries

| Information | Authority |
| :--- | :--- |
| Original inquiry payload | Original source or provider |
| Original source receipt identifier | Original source or provider |
| ASOS normalized Lead | Lead Domain Service |
| Customer identity | Customer Domain Service and approved identity sources |
| Qualification workflow state | Lead or Qualified Lead Domain Service |
| Qualified Lead | Qualified Lead Domain Service |
| Original communication delivery | Communication provider |
| Consent evidence | Customer Consent authority or approved source |
| Marketing attribution | Approved campaign or source evidence |
| Lead score and classifications | Derived Intelligence |
| Assignment workflow | ASOS workflow state or configured external CRM |
| Opportunity stage | Opportunity Domain Service or configured CRM |
| Final commercial outcome | Deal and configured external authority |

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
- `receiving_team_id`.
- `assigned_owner_user_id`.
- `assignment_queue_id`.

`tenant_id` is the primary isolation boundary.

Dealership, branch, team, and queue identifiers represent organizational scope inside the Tenant.

### Customer and Workflow Relationships

- `customer_id`.
- `qualified_lead_id`.
- `duplicate_of_lead_id`.
- `campaign_id`.
- `latest_interaction_id`.
- `source_interaction_id`.
- `active_review_task_id`.

### Source Identity

- `source_channel`.
- `source_system`.
- `source_record_id`.
- `source_event_id`.
- `source_provider_account_id`.
- `source_received_at`.
- `source_updated_at`.
- `source_authority`.
- `source_deduplication_key`.
- `raw_payload_reference`.
- `raw_payload_hash`.
- `original_content_reference`.

### Submitted Party Information

- `submitted_display_name`.
- `submitted_given_name`.
- `submitted_family_name`.
- `submitted_organization_name`.
- `submitted_phone`.
- `submitted_email`.
- `submitted_messaging_identifier`.
- `submitted_preferred_language`.
- `submitted_location_text`.

Submitted party information remains unverified until accepted through the appropriate identity or contact-verification workflow.

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
- `trade_in_interest`.
- `finance_interest`.
- `test_drive_interest`.
- `preferred_contact_time_text`.

### Marketing Attribution

- `campaign_id`.
- `campaign_name_projection`.
- `utm_source`.
- `utm_medium`.
- `utm_campaign`.
- `utm_content`.
- `utm_term`.
- `referrer_url`.
- `landing_page_url`.
- `advertisement_id`.
- `advertisement_set_id`.
- `creative_id`.
- `affiliate_reference`.
- `referral_reference`.

### Technical Intake Metadata

- `source_ip_address`.
- `user_agent`.
- `device_type`.
- `session_id`.
- `form_id`.
- `form_version`.
- `provider_delivery_metadata`.
- `ingestion_connector_id`.
- `ingestion_batch_id`.

Technical metadata must be retained only where lawful, necessary, and permitted by retention policy.

### Lead Workflow State

- `status`.
- `identity_resolution_status`.
- `contact_validation_status`.
- `intent_classification_status`.
- `permission_assessment_status`.
- `duplicate_assessment_status`.
- `qualification_status`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.

### Assignment

- `assignment_status`.
- `assigned_owner_user_id`.
- `assigned_team_id`.
- `assignment_queue_id`.
- `assigned_at`.
- `assignment_rule_id`.
- `assignment_reason`.
- `assignment_expires_at`.
- `previous_owner_user_id`.

### Response and Activity Projections

- `first_response_due_at`.
- `first_response_at`.
- `first_response_interaction_id`.
- `response_time_seconds`.
- `last_contact_attempt_at`.
- `last_contact_attempt_interaction_id`.
- `contact_attempt_count`.
- `last_activity_at`.
- `lead_age_seconds`.
- `time_to_assignment_seconds`.
- `time_to_qualification_seconds`.

These fields are projections calculated from accepted Events and Interactions.

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
- `fraud_risk_score`.
- `spam_risk_score`.
- `requires_human_review`.

Derived Intelligence must preserve:

- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Confidence where meaningful.
- Assumptions.
- Generation timestamp.
- Expiration timestamp.
- Applicable Action Class.
- Required Human authority or automation policy.

### Qualification Outcome

- `qualification_decision`.
- `qualification_reason_codes`.
- `qualification_evidence_references`.
- `qualification_decided_at`.
- `qualification_decided_by_actor_type`.
- `qualification_decided_by_actor_id`.
- `qualification_policy_id`.
- `qualification_policy_version`.

### Disqualification, Invalidity, and Duplicate Outcome

- `disqualification_reason`.
- `disqualified_at`.
- `invalid_reason`.
- `invalidated_at`.
- `duplicate_of_lead_id`.
- `duplicate_reason`.
- `duplicate_evidence_references`.
- `duplicate_decided_at`.
- `withdrawal_reason`.
- `withdrawn_at`.
- `expiration_reason`.
- `expired_at`.

### Consent and Communication Basis Projection

- `contact_permission_status`.
- `contact_permission_basis`.
- `contact_permission_source`.
- `contact_permission_evidence_reference`.
- `contact_permission_checked_at`.
- `contact_permission_expires_at`.

These fields are projections.

They must not replace Customer-level, channel-specific, purpose-specific, and jurisdiction-specific Consent records.

### Source, Synchronization, and Audit

- `source_system`.
- `source_record_id`.
- `source_authority`.
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
| `dealership_id` | UUID | Conditional | Canonical Projection | Dealership receiving or handling the Lead. |
| `branch_id` | UUID | No | Canonical Projection | Branch context inside the Tenant. |
| `customer_id` | UUID | No | Customer relationship | Resolved Canonical Customer. |
| `qualified_lead_id` | UUID | No | Qualified Lead relationship | Qualified Lead created from this Lead. |
| `status` | Enum | Yes | ASOS Workflow State | Current Lead lifecycle state. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, freshness, conflict, or quarantine state. |
| `conflict_status` | Enum | Yes | ASOS Workflow State | Material Lead-data conflict status. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Source Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `source_channel` | Enum | Yes | Source evidence | Channel through which the inquiry entered. |
| `source_system` | String | Yes | Integration Context | System or provider supplying the inquiry. |
| `source_record_id` | String | No | External source | Record identifier assigned by the source. |
| `source_event_id` | String | No | External source | Source Event or delivery identifier. |
| `source_received_at` | Timestamp | Yes | Source or ASOS receipt | Time the source received or emitted the inquiry. |
| `source_authority` | Enum | Yes | Governance | Authority classification of the source. |
| `source_deduplication_key` | String | No | Integration Context | Approved source-level duplicate-prevention key. |
| `raw_payload_reference` | String | Yes | Evidence repository | Reference to the immutable original payload. |
| `raw_payload_hash` | String | Yes | ASOS | Integrity hash for the original payload. |
| `original_content_reference` | String | No | Evidence repository | Reference to original message, call recording, transcript, or document. |

### Submitted Party Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `submitted_display_name` | String | No | Source-provided | Unverified name submitted with the inquiry. |
| `submitted_given_name` | String | No | Source-provided or extracted | Unverified given name. |
| `submitted_family_name` | String | No | Source-provided or extracted | Unverified family name. |
| `submitted_organization_name` | String | No | Source-provided or extracted | Unverified organization name. |
| `submitted_phone` | String | Conditional | Source-provided | Contact number supplied with the inquiry. |
| `submitted_email` | String | Conditional | Source-provided | Email address supplied with the inquiry. |
| `submitted_messaging_identifier` | String | Conditional | Source-provided | Provider-specific messaging identifier. |
| `submitted_preferred_language` | String | No | Source-provided | Preferred language supplied by the Lead. |

At least one of the following must exist unless the source represents a governed anonymous inquiry:

- Contact path.
- Source-account identifier.
- Walk-in reference.
- Original Interaction reference.
- Sufficient inquiry content for manual review.

### Inquiry Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `inquiry_subject` | String | No | Source-provided or extracted | Short inquiry subject. |
| `inquiry_text` | Text | No | Source evidence | Original or normalized inquiry text. |
| `inquiry_language` | String | No | Source or Derived Intelligence | Language code of the inquiry. |
| `inquiry_category` | Enum | Yes | Canonical Projection | High-level inquiry category. |
| `requested_action` | Enum | No | Source or Derived Intelligence | Action requested by the Lead. |
| `vehicle_interest_text` | Text | No | Source-provided | Unstructured Vehicle interest. |
| `vehicle_interest_snapshot` | JSON Object | No | Derived Intelligence | Extracted Vehicle preferences. |
| `trade_in_interest` | Boolean | No | Source or Derived Intelligence | Indicates possible Trade-In interest. |
| `finance_interest` | Boolean | No | Source or Derived Intelligence | Indicates possible finance interest. |
| `test_drive_interest` | Boolean | No | Source or Derived Intelligence | Indicates possible test-drive interest. |

### Workflow Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `identity_resolution_status` | Enum | Yes | Workflow State | Customer-identity resolution progress. |
| `contact_validation_status` | Enum | Yes | Workflow State | Validation state for submitted contact paths. |
| `intent_classification_status` | Enum | Yes | Workflow State | State of automotive-intent assessment. |
| `permission_assessment_status` | Enum | Yes | Workflow State | Assessment state for permitted communication. |
| `duplicate_assessment_status` | Enum | Yes | Workflow State | Duplicate-detection state. |
| `qualification_status` | Enum | Yes | Workflow State | Qualification process state. |
| `review_status` | Enum | Yes | Workflow State | Human Review state. |
| `assignment_status` | Enum | Yes | Workflow State | Lead-assignment state. |
| `assigned_owner_user_id` | UUID | No | Workflow State | Current responsible User. |
| `assigned_team_id` | UUID | No | Workflow State | Current responsible team. |
| `assigned_at` | Timestamp | No | ASOS | Time the current assignment became effective. |

### Permission Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contact_permission_status` | Enum | Yes | Canonical Projection | Current summary of permitted outbound contact. |
| `contact_permission_basis` | Enum | No | Approved evidence | Consent, requested response, contractual basis, or another approved basis. |
| `contact_permission_source` | String | No | Provenance | Source used to assess the permission. |
| `contact_permission_evidence_reference` | String | No | Evidence | Supporting Consent or contact-basis evidence. |
| `contact_permission_checked_at` | Timestamp | No | ASOS | Time permission was last assessed. |
| `contact_permission_expires_at` | Timestamp | No | Policy or source | Time the current basis expires where applicable. |

### Derived Intelligence Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_score` | Decimal | No | Derived Intelligence | Overall configured Lead-priority score. |
| `qualification_score` | Decimal | No | Derived Intelligence | Evidence-based qualification score. |
| `duplicate_score` | Decimal | No | Derived Intelligence | Likelihood of duplication. |
| `automotive_intent_score` | Decimal | No | Derived Intelligence | Likelihood of automotive commercial intent. |
| `contactability_score` | Decimal | No | Derived Intelligence | Estimated likelihood of successful permitted contact. |
| `urgency_score` | Decimal | No | Derived Intelligence | Estimated urgency of the inquiry. |
| `spam_risk_score` | Decimal | No | Derived Intelligence | Estimated likelihood of spam. |
| `fraud_risk_score` | Decimal | No | Derived Intelligence | Estimated risk requiring governed review. |
| `inquiry_summary` | Text | No | Derived Intelligence | AI-generated or deterministic summary. |
| `recommended_next_action` | String | No | Derived Intelligence | Suggested next action. |
| `requires_human_review` | Boolean | Yes | Deterministic or Derived | Indicates whether review criteria are met. |

Scores must use an approved normalized scale defined by the applicable Schema or model contract.

### Outcome Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `qualification_decision` | Enum | No | Human Decision or approved policy | Final qualification workflow Decision. |
| `qualification_reason_codes` | Array | No | Decision evidence | Reasons supporting the Decision. |
| `qualification_decided_at` | Timestamp | No | ASOS | Time the Decision was accepted. |
| `disqualification_reason` | Enum | No | Human Decision or policy | Reason the Lead was disqualified. |
| `invalid_reason` | Enum | No | Human Decision or policy | Reason the Lead was invalidated. |
| `duplicate_of_lead_id` | UUID | No | Workflow State | Surviving Lead for a duplicate record. |
| `duplicate_evidence_references` | Array | No | Evidence | Evidence supporting duplicate classification. |
| `withdrawal_reason` | Enum | No | Source, Customer, or Human Decision | Reason processing was withdrawn. |
| `expiration_reason` | Enum | No | Approved policy | Reason the Lead expired. |

### Response Metrics

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `first_response_due_at` | Timestamp | No | Deterministic policy | Configured response deadline. |
| `first_response_at` | Timestamp | No | Interaction projection | Time of the first accepted valid response. |
| `first_response_interaction_id` | UUID | No | Interaction relationship | Interaction proving the first valid response. |
| `response_time_seconds` | Integer | No | Deterministic calculation | Time between accepted Lead receipt and first valid response. |
| `contact_attempt_count` | Integer | Yes | Interaction projection | Accepted contact-attempt count. |
| `last_contact_attempt_at` | Timestamp | No | Interaction projection | Latest accepted contact-attempt time. |
| `lead_age_seconds` | Integer | Yes | Deterministic calculation | Current or final Lead age. |
| `time_to_assignment_seconds` | Integer | No | Deterministic calculation | Time from receipt to accepted assignment. |
| `time_to_qualification_seconds` | Integer | No | Deterministic calculation | Time from receipt to accepted qualification outcome. |

---

## 4. Enumerations

### LeadStatus

- `RECEIVED`
- `NORMALIZING`
- `VALIDATING`
- `REVIEW_REQUIRED`
- `QUALIFIED`
- `DISQUALIFIED`
- `DUPLICATE`
- `INVALID`
- `WITHDRAWN`
- `EXPIRED`
- `ARCHIVED`

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
- `OTHER`

### LeadSourceAuthority

- `PROVIDER_VERIFIED`
- `CRM_VERIFIED`
- `OEM_VERIFIED`
- `PARTNER_VERIFIED`
- `DEALERSHIP_REPORTED`
- `CUSTOMER_SUBMITTED`
- `AI_EXTRACTED`
- `MANUAL_ENTRY`
- `UNKNOWN`

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

A requested action does not prove that the action is approved, scheduled, reserved, or completed.

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

- `UNKNOWN`
- `RESPONSE_PERMITTED`
- `TRANSACTIONAL_ONLY`
- `MARKETING_PERMITTED`
- `RESTRICTED`
- `DO_NOT_CONTACT`
- `EXPIRED`

### ContactPermissionBasis

- `CUSTOMER_REQUESTED_RESPONSE`
- `EXPLICIT_CONSENT`
- `EXISTING_CUSTOMER_RELATIONSHIP`
- `CONTRACTUAL_NECESSITY`
- `LEGITIMATE_INTEREST`
- `LEGAL_OBLIGATION`
- `NOT_REQUIRED`
- `UNKNOWN`

Applicable law and Tenant policy determine which bases are permitted.

### DuplicateAssessmentStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `NO_MATCH`
- `POTENTIAL_MATCH`
- `CONFIRMED_DUPLICATE`
- `CONFLICTED`
- `REVIEW_REQUIRED`

### QualificationStatus

- `NOT_STARTED`
- `IN_PROGRESS`
- `EVIDENCE_INCOMPLETE`
- `REVIEW_REQUIRED`
- `QUALIFIED`
- `DISQUALIFIED`
- `CANCELLED`
- `FAILED`

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `REJECTED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

### AssignmentStatus

- `UNASSIGNED`
- `QUEUED`
- `ASSIGNMENT_PENDING`
- `ASSIGNED`
- `REASSIGNMENT_PENDING`
- `REASSIGNED`
- `RETURNED_TO_QUEUE`
- `SUSPENDED`

### QualificationDecision

- `QUALIFY`
- `DISQUALIFY`
- `REQUEST_MORE_INFORMATION`
- `ROUTE_TO_HUMAN_REVIEW`
- `MARK_DUPLICATE`
- `MARK_INVALID`
- `WITHDRAW`
- `EXPIRE`

### DisqualificationReason

- `NO_VALID_CONTACT_PATH`
- `NO_AUTOMOTIVE_INTENT`
- `OUTSIDE_CONFIGURED_MARKET`
- `UNREACHABLE_AFTER_APPROVED_ATTEMPTS`
- `CUSTOMER_NOT_READY`
- `DUPLICATE`
- `DO_NOT_CONTACT`
- `BUDGET_OR_REQUIREMENT_MISMATCH`
- `UNSUPPORTED_REQUEST`
- `TRANSFERRED_TO_ANOTHER_WORKFLOW`
- `OTHER`

A disqualified Lead may still be valid business data.

Disqualification must not be used as a substitute for `INVALID`.

### InvalidReason

- `SPAM`
- `MALICIOUS_CONTENT`
- `FRAUDULENT`
- `CORRUPTED_PAYLOAD`
- `TEST_RECORD`
- `NO_USABLE_INFORMATION`
- `UNAUTHORIZED_SOURCE`
- `POLICY_VIOLATION`
- `OTHER`

### WithdrawalReason

- `CUSTOMER_WITHDREW`
- `SOURCE_CANCELLED`
- `REQUEST_RESOLVED_EXTERNALLY`
- `PROCESSING_RESTRICTED`
- `LEGAL_OR_PRIVACY_REQUEST`
- `OTHER`

### ExpirationReason

- `NO_RESPONSE_WITHIN_POLICY`
- `STALE_INQUIRY`
- `CAMPAIGN_EXPIRED`
- `QUALIFICATION_WINDOW_EXPIRED`
- `PERMISSION_EXPIRED`
- `OTHER`

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
- Request bodies must not override `tenant_id`.
- Every related record must belong to the authorized Tenant scope.
- `dealership_id`, `branch_id`, team, owner, campaign, and queue must belong to the Tenant.
- Cross-Tenant Lead search, matching, assignment, qualification, export, and AI retrieval are prohibited unless an approved and auditable sharing mechanism exists.
- Background processes and AI Agents must receive signed Tenant execution context.

### Source Intake Rules

- Every Lead must identify its source channel and source system.
- The original source payload or equivalent original evidence must be preserved.
- `raw_payload_hash` must be calculated using an approved integrity algorithm.
- The original payload must not be silently modified after accepted ingestion.
- A normalized correction must not rewrite the original evidence.
- Duplicate source delivery must not create duplicate canonical Lead effects.
- Source ingestion must support replay-safe processing.
- An unauthorized or unverifiable source may be quarantined.

### Source Deduplication Rules

Source deduplication should use the strongest available source evidence, such as:

- Source Event identifier.
- Provider delivery identifier.
- Source record identifier.
- Source account and record combination.
- Source deduplication key.
- Payload hash.
- Approved composite key.

`event_id` is used by Event Consumers to detect repeated delivery of the same ASOS Event.

Command `idempotency_key` is used for retryable Commands.

These identifiers must not be confused with source-level deduplication identifiers.

### Submitted Identity Rules

- Submitted names, phones, emails, and organization names are unverified input.
- Lead submission must not automatically create verified Customer identity.
- A shared phone number or email must not automatically merge Leads or Customers.
- Phone numbers should be normalized to E.164 where supported.
- Email addresses should be normalized and syntactically validated.
- Contact-format validity does not prove contact ownership.
- AI-extracted identity fields must preserve source evidence and confidence.
- Sensitive identity information must not be collected unless required for the Lead purpose.

### Customer Resolution Rules

- `customer_id` may remain null while identity resolution is incomplete.
- A Lead may be linked to an existing Customer only through an approved identity-resolution process.
- Creation of a new Customer must preserve the Lead as the originating evidence.
- Ambiguous identity matches must create Human Review.
- A Lead must not be linked to multiple active Customers.
- Customer merges and corrections must be handled through Customer governance, not Lead updates.

### Inquiry Rules

- A Lead must contain sufficient evidence of an inquiry, source delivery, or governed walk-in.
- A requested Reservation does not reserve a Vehicle.
- A requested Quotation does not create an approved Quotation.
- A requested Appointment does not confirm an Appointment.
- A request for finance does not create a Finance Application or lender Decision.
- A Vehicle-interest extraction does not prove Vehicle availability.
- Customer-facing action claims must use the applicable authoritative Domain Object.

### Contact Permission Rules

- Receipt of an inquiry may support a response to that inquiry only where applicable law and policy permit it.
- Inquiry receipt must not automatically create general marketing Consent.
- Consent must not be inferred solely from engagement, message delivery, or AI confidence.
- A preferred channel does not override channel-specific restrictions.
- `DO_NOT_CONTACT` must block prohibited outbound communication through deterministic controls.
- `TRANSACTIONAL_ONLY` must not permit unrelated marketing.
- Permission evidence must include channel, purpose, source, effective period, and legal basis where required.
- Changes requiring external write-back must remain pending until authoritative External Confirmation.
- AI Agents must not reverse a contact restriction.

### Duplicate Rules

- Duplicate detection may generate a candidate match.
- Similarity score alone must not mark a Lead as a confirmed duplicate unless an approved deterministic policy permits the case.
- Ambiguous duplicate matches require Human Review.
- A duplicate Lead must preserve:
  - Original payload.
  - Source attribution.
  - Receipt time.
  - Campaign attribution.
  - Duplicate evidence.
  - Surviving Lead reference.
- `duplicate_of_lead_id` must not equal `lead_id`.
- Circular duplicate chains are prohibited.
- Active processing should continue against the surviving Lead or Customer according to policy.
- Marking a Lead duplicate must not delete its original evidence.

### Assignment Rules

- A Lead may have only one current primary owner.
- Assignment may be to:
  - A User.
  - A team.
  - A queue.
- Assignment must preserve the responsible actor or rule.
- Assignment and reassignment must be auditable.
- Owner availability, branch scope, workload, language, skill, and conflict rules may be configurable.
- AI may recommend an owner but must not bypass deterministic assignment policy.
- Assignment does not imply qualification.

### Response Metric Rules

- `first_response_at` must reference a valid accepted Interaction where possible.
- Automated acknowledgment and meaningful response must remain distinguishable.
- Tenant policy must define what qualifies as a valid first response.
- `first_response_at` must not precede `source_received_at`.
- Response metrics must not be negative.
- Reprocessing must not reset the original response-time measurement.
- Provider delivery failure must not be counted as successful Customer contact.

### Qualification Rules

A Lead may become `QUALIFIED` only when the approved qualification policy determines that sufficient evidence exists.

The policy may consider:

- Valid or usable contact path.
- Automotive sales intent.
- Vehicle or commercial requirement.
- Customer identity resolution.
- Contact permission.
- Geographic or operational scope.
- Timing.
- Duplicate status.
- Fraud and spam risk.
- Required information completeness.
- Human Review where required.

Qualification must:

- Preserve applied policy and version.
- Preserve supporting evidence.
- Record the Decision authority.
- Resolve or create a Customer where required.
- Create one separate Qualified Lead.
- Link the Qualified Lead to the original Lead.
- Publish the accepted state change only after the controlled transaction succeeds.

Qualification must not:

- Create an Opportunity automatically unless a separate governed workflow permits it.
- Confirm Vehicle availability.
- Commit pricing.
- Reserve stock.
- Approve finance.
- Create a Deal.

### Disqualification Rules

- Disqualification requires a valid reason.
- Disqualification does not mean the source record is invalid.
- A disqualified Lead may be reconsidered when new evidence arrives.
- Reconsideration must use a governed reopening or correction workflow.
- Customer opt-out must not be misrepresented as lack of automotive intent.
- Out-of-market handling may route the Lead instead of discarding it.

### Invalidity Rules

A Lead may become `INVALID` only with evidence of:

- Spam.
- Malicious content.
- Fraudulent submission.
- Corrupted or unusable payload.
- Test data.
- Unauthorized source.
- No usable inquiry evidence.
- Another approved invalidity reason.

Invalidity must not be used merely because:

- The Customer is not ready.
- Contact attempts failed.
- Budget does not match.
- The Lead is low priority.
- The Lead is outside a campaign target.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Retryable creation and external-write operations must support an approved `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Leads.
  - Assignments.
  - Qualification outcomes.
  - Qualified Leads.
  - Customer records.
  - Messages.
  - Review tasks.

### Human Review Requirements

Human Review is required when configured policy identifies:

- Ambiguous Customer identity.
- Conflicting contact information.
- Uncertain duplicate match.
- Low-confidence qualification.
- Fraud risk.
- Permission conflict.
- Conflicting source attribution.
- Requested correction to original-source evidence.
- Reopening of a terminal Lead outcome.
- Another material exception.

---

## 6. State Machine

### Allowed States

```text
RECEIVED
NORMALIZING
VALIDATING
REVIEW_REQUIRED
QUALIFIED
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
RECEIVED → INVALID
RECEIVED → WITHDRAWN

NORMALIZING → VALIDATING
NORMALIZING → REVIEW_REQUIRED
NORMALIZING → INVALID

VALIDATING → REVIEW_REQUIRED
VALIDATING → QUALIFIED
VALIDATING → DISQUALIFIED
VALIDATING → DUPLICATE
VALIDATING → INVALID
VALIDATING → WITHDRAWN
VALIDATING → EXPIRED

REVIEW_REQUIRED → VALIDATING
REVIEW_REQUIRED → QUALIFIED
REVIEW_REQUIRED → DISQUALIFIED
REVIEW_REQUIRED → DUPLICATE
REVIEW_REQUIRED → INVALID
REVIEW_REQUIRED → WITHDRAWN
REVIEW_REQUIRED → EXPIRED

DISQUALIFIED → VALIDATING
EXPIRED → VALIDATING

QUALIFIED → ARCHIVED
DUPLICATE → ARCHIVED
INVALID → ARCHIVED
WITHDRAWN → ARCHIVED
EXPIRED → ARCHIVED
DISQUALIFIED → ARCHIVED
```

Reopening `DISQUALIFIED` or `EXPIRED` requires new evidence and the configured authority.

### Forbidden Ordinary Transitions

```text
RECEIVED → QUALIFIED
RECEIVED → DUPLICATE without evidence
NORMALIZING → QUALIFIED
QUALIFIED → RECEIVED
QUALIFIED → VALIDATING
QUALIFIED → DISQUALIFIED
DUPLICATE → QUALIFIED
INVALID → QUALIFIED
WITHDRAWN → QUALIFIED
ARCHIVED → VALIDATING
ARCHIVED → QUALIFIED
```

Corrections to terminal outcomes must use a governed correction or reopening workflow.

### Entering RECEIVED

Requires:

- Valid Tenant context.
- Source channel.
- Source system.
- Source receipt time.
- Original payload or equivalent evidence.
- Integrity hash.
- Source deduplication checks.

### Entering NORMALIZING

Requires:

- Successfully persisted original evidence.
- Accepted source context.
- No blocking ingestion failure.

### Entering VALIDATING

Requires:

- Normalized Lead record.
- Minimum inquiry evidence.
- Source attribution.
- Initial data-quality assessment.

### Entering REVIEW_REQUIRED

Requires:

- Recorded review reason.
- Supporting evidence.
- Workflow restrictions.
- Human Review Task.
- Applicable priority and SLA.

### Entering QUALIFIED

Requires:

- Approved qualification policy.
- Sufficient qualification evidence.
- Accepted qualification Decision.
- Customer resolution where required.
- Successfully created Qualified Lead.
- No unresolved blocking conflict.
- Valid record version.

### Entering DISQUALIFIED

Requires:

- Accepted disqualification Decision.
- Valid disqualification reason.
- Supporting evidence.
- Follow-up or retention policy.

### Entering DUPLICATE

Requires:

- Surviving Lead reference.
- Duplicate evidence.
- Accepted duplicate Decision.
- No circular duplicate chain.
- Preserved original source evidence.

### Entering INVALID

Requires:

- Valid invalidity reason.
- Supporting evidence.
- Applied policy.
- Required Human Review where applicable.

### Entering WITHDRAWN

Requires:

- Customer, source, or authorized Human withdrawal evidence.
- Recorded reason.
- Suspension of prohibited follow-up.

### Entering EXPIRED

Requires:

- Applicable expiration policy.
- Expiration reason.
- Expiration timestamp.
- No active blocking workflow requiring another state.

### Terminal Outcomes

The following are terminal for ordinary processing:

- `QUALIFIED`
- `DUPLICATE`
- `INVALID`
- `WITHDRAWN`
- `ARCHIVED`

`DISQUALIFIED` and `EXPIRED` may be reopened through a governed workflow.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Applied policy.
- Evidence.
- Record version.
- Timestamp.
- Correlation identifier.
- Related Event.
- Human Decision where applicable.
- Automation-policy reference where applicable.

---

## 7. Relationships

### Tenant

- Every Lead belongs to exactly one `tenant_id`.
- All related Objects must remain inside authorized Tenant scope.
- Cross-Tenant Lead routing requires an approved and auditable mechanism.

### Dealership and Branch

- A Lead may be associated with one receiving dealership.
- A branch may remain unknown until routing or assignment.
- Routing to another branch must preserve history.
- Organizational routing must not alter original source evidence.

### Customer

- A Lead may resolve to one Customer.
- A Customer may have multiple Leads over time.
- Lead identity fields remain submitted or extracted evidence.
- Customer identity remains governed by Customer Domain Service.
- Lead qualification must not silently overwrite Customer identity.

### Qualified Lead

- One Lead may produce no more than one active Qualified Lead for the same qualification outcome.
- The Qualified Lead must reference its originating Lead.
- Requalification must follow the approved versioning or reopening policy.

### Opportunity

- A Lead does not directly represent an Opportunity.
- A Qualified Lead may later create or influence an Opportunity through a separate governed workflow.
- Pipeline stage belongs to Opportunity.

### Interaction

- Original inquiry evidence may be represented as an Interaction.
- Subsequent communication attempts and responses belong to Interaction.
- Lead response metrics may project accepted Interaction facts.
- Provider delivery evidence remains traceable.

### Vehicle and Inventory Record

- Lead may contain Vehicle-interest text or extracted preferences.
- Lead interest does not prove a Vehicle exists in Inventory.
- Vehicle matching does not reserve or allocate Inventory.
- Current availability must come from Inventory Record and its authoritative source.

### Appointment

- A Lead may request an Appointment.
- Appointment request does not create confirmed Appointment status.
- Confirmed scheduling belongs to Appointment and its configured authority.

### Quotation

- A Lead may request pricing.
- A Lead must not contain binding pricing or Quotation state.
- Approved Customer-visible terms belong to Quotation.

### Trade-In

- A Lead may express Trade-In interest.
- Appraisal, ownership evidence, lien, and acquisition state belong to Trade-In.

### Finance Application

- A Lead may express finance interest.
- Sensitive finance data, application state, and lender Decisions belong to Finance Application.

### Deal

- A Lead may contribute attribution to a future Deal.
- Lead status must not represent Deal completion.
- Deal attribution rules must preserve the original Lead and campaign evidence.

### Campaign and Attribution

- A Lead may reference one primary originating campaign and multiple attribution observations.
- Attribution model calculations must remain distinguishable from original source evidence.
- Marketing attribution changes must preserve history and model version.

### Human Review Task

- A Lead may have one active review task for a specific review purpose.
- Review outcomes must remain linked to the Lead and evidence.

### Duplicate Relationship

A duplicate Lead must reference:

```text
duplicate_of_lead_id
```

The surviving Lead must preserve references to all duplicate Leads.

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

The following are required Lead Event concepts and do not replace the Event Catalog.

### Intake Event Concepts

- Lead source observation received.
- Lead ingestion accepted.
- Lead ingestion rejected.
- Lead created.
- Lead normalization started.
- Lead normalization completed.
- Lead payload quarantined.

### Validation Event Concepts

- Lead validation started.
- Contact validation updated.
- Automotive intent classified.
- Customer identity resolution requested.
- Customer identity resolved.
- Lead data conflict detected.
- Lead data conflict resolved.
- Human Review requested.

### Assignment Event Concepts

- Lead queued.
- Lead assigned.
- Lead reassigned.
- Lead returned to queue.
- Lead assignment suspended.

### Response Event Concepts

- First valid response recorded.
- Response deadline approaching.
- Response deadline missed.
- Lead contact attempt recorded.
- Lead became unreachable under policy.

### Qualification Event Concepts

- Lead qualification started.
- Lead qualification evidence completed.
- Lead qualified.
- Lead disqualified.
- Lead qualification review requested.
- Lead qualification corrected.

### Duplicate and Invalidity Event Concepts

- Lead duplicate candidate detected.
- Lead confirmed duplicate.
- Lead invalidated.
- Lead withdrawn.
- Lead expired.
- Lead reopened.
- Lead archived.

### Permission Event Concepts

- Lead contact permission assessed.
- Lead contact restriction detected.
- Lead contact permission expired.
- Lead outbound activity suspended.

### Derived Intelligence Event Concepts

- Lead score updated.
- Lead inquiry summarized.
- Lead Vehicle preferences extracted.
- Lead next action recommended.
- Lead fraud risk detected.
- Lead spam risk detected.

Derived Intelligence Events must not imply:

- Human approval.
- Customer identity verification.
- Qualification completion.
- Communication permission.
- Opportunity creation.
- Appointment Confirmation.
- Vehicle reservation.
- Quotation approval.
- Deal completion.

### Event Producer Rules

- Lead Domain Service publishes accepted Lead canonical and workflow-state changes.
- Integration services publish normalized source-observation Events.
- Interaction Domain Service publishes accepted communication facts.
- Customer Domain Service publishes accepted Customer identity changes.
- Qualified Lead Domain Service publishes Qualified Lead lifecycle Events.
- AI Agents may publish Agent-run, extraction, classification, or Recommendation Events.
- AI Agents must not publish an authoritative qualification, Consent, identity, or external-completion Event merely because they proposed the result.

### Event Requirements

Every material Lead Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `lead_id`.
- Customer identifier where applicable.
- Dealership and branch context.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Correlation identifier.
- Causation identifier.
- Source reference.
- Evidence references.
- Applied policy.
- Related Human Decision.
- Security classification.

Events are immutable.

Corrections, reopening, withdrawal, and reversal must use new Events linked to the original Event.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate business effects using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Inquiry classification.
- Language detection.
- Contact-detail extraction.
- Vehicle-interest extraction.
- Budget and timeline extraction.
- Intent classification.
- Spam detection.
- Fraud-risk detection.
- Duplicate-candidate detection.
- Customer-match suggestion.
- Lead scoring.
- Qualification Recommendation.
- Response drafting.
- Summary generation.
- Assignment Recommendation.
- Next-best-action Recommendation.
- Missing-information detection.
- Human Review preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create authoritative Customer identity.
- Verify ownership of a contact point.
- Create or revoke Consent.
- Reverse `DO_NOT_CONTACT`.
- Confirm qualification where policy requires Human Review.
- Mark an ambiguous Lead as duplicate.
- Delete original source evidence.
- Change original campaign attribution without governed evidence.
- Create a binding Quotation.
- Reserve or allocate a Vehicle outside approved authority.
- Approve finance.
- Create a contract.
- Finalize a Deal.
- Send communication outside Human Approval or an approved automation policy.
- Access Lead data outside authorized Tenant scope.
- Represent a Recommendation as a Human Decision.
- Represent a sent Command as externally confirmed.

### AI Extraction Requirements

Every material AI extraction must preserve:

- Output type.
- Suggested value.
- Source reference.
- Evidence location.
- Source timestamp.
- Model version.
- Prompt version.
- Confidence where meaningful.
- Authority category.
- Review requirement.
- Generation timestamp.
- Expiration timestamp.

### Configurable Thresholds

Confidence thresholds must remain configurable by:

- Tenant.
- Use case.
- Risk level.
- Model version.
- Language.
- Channel.
- Field type.

A fixed confidence threshold must not be embedded in this Canonical Domain Model.

Policy may determine whether an AI output:

- Is discarded.
- Remains Derived Intelligence.
- Creates Human Review.
- Is accepted as a low-risk Canonical Projection.
- Requires authoritative evidence.

### Qualification Reasoning

AI qualification must distinguish:

- Source fact.
- Submitted information.
- Verified information.
- Derived inference.
- Missing evidence.
- Contact permission.
- Human Decision.
- Final qualification state.

AI must not infer qualification solely from:

- One keyword.
- One score.
- Message sentiment.
- High-value Vehicle interest.
- Marketing source.
- Customer demographics.
- Ability to pay inferred from protected or unrelated data.

### Duplicate Reasoning

Duplicate Recommendations should use multiple evidence types where available, such as:

- Normalized contact points.
- Source identifiers.
- Customer references.
- Inquiry similarity.
- Temporal proximity.
- Vehicle interest.
- Campaign context.
- Provider account identifiers.

Similarity must not automatically establish shared identity.

### Response Generation

AI may draft a response only when:

- Contact purpose is permitted.
- Channel is permitted.
- Customer or Lead context is current.
- Vehicle and pricing claims use authoritative sources.
- Required templates and disclosures apply.
- Human Approval or approved automation policy is satisfied.

A generated response must not claim:

- Vehicle availability without current Inventory evidence.
- Approved price without pricing authority.
- Confirmed Appointment without Confirmation.
- Approved finance without lender Decision.
- Reservation without authoritative Reservation state.

### Action Class 2

Controlled outbound Lead communication may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Contact permission.
- Purpose.
- Channel.
- Template.
- Frequency.
- Time restrictions.
- Lead state.
- Customer restrictions.
- Source freshness.
- Revocation.
- Risk limits.
- Audit requirements.

### Human Review

Human Review is required according to configured policy for:

- Ambiguous identity.
- Ambiguous duplicate match.
- Low-confidence qualification.
- Permission conflict.
- Fraud concern.
- Sensitive correction.
- Reopening terminal outcomes.
- Material source conflict.
- Another high-risk exception.

### AI Context and Embeddings

Direct identifiers and raw source payloads must not be placed into unrestricted vector embeddings.

Normally excluded fields include:

- Submitted name.
- Phone number.
- Email address.
- Messaging identifier.
- IP address.
- User agent.
- Raw payload.
- Source account identifier.
- External Customer identifier.
- Consent evidence.
- Identity evidence.
- Financial information.

Approved redacted or abstracted context may include:

- Inquiry summary.
- Vehicle preferences.
- Budget category.
- Timeline category.
- Objections.
- Non-sensitive intent classification.
- Approved Interaction summary.

Every vector record must enforce:

- `tenant_id`.
- Lead access scope.
- Source reference.
- Security classification.
- Retention.
- Expiration.
- Deletion and anonymization propagation.

### Explainability

Material Lead Recommendations must explain:

- Evidence used.
- Source authority.
- Data freshness.
- Material conflicts.
- Missing information.
- Applied policy.
- Score or model interpretation.
- Confidence where meaningful.
- Recommended action.
- Required Human authority.
- Communication restrictions.
- Expiration.

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

POST   /api/v1/leads/{lead_id}/normalization-requests
POST   /api/v1/leads/{lead_id}/validation-requests
POST   /api/v1/leads/{lead_id}/identity-resolution-requests
POST   /api/v1/leads/{lead_id}/assignment-requests
POST   /api/v1/leads/{lead_id}/reassignment-requests
POST   /api/v1/leads/{lead_id}/qualification-requests
POST   /api/v1/leads/{lead_id}/qualification-decisions
POST   /api/v1/leads/{lead_id}/disqualification-decisions
POST   /api/v1/leads/{lead_id}/duplicate-decisions
POST   /api/v1/leads/{lead_id}/invalidation-decisions
POST   /api/v1/leads/{lead_id}/withdrawal
POST   /api/v1/leads/{lead_id}/reopen
POST   /api/v1/leads/{lead_id}/review-requests

GET    /api/v1/leads/{lead_id}/history
GET    /api/v1/leads/{lead_id}/source-evidence
GET    /api/v1/leads/{lead_id}/qualification-evidence
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, queue, and owner scope must be validated.
- Cross-Tenant searches must be blocked by default.

### Example Create Request

```json
{
  "source_channel": "WEBSITE",
  "source": {
    "source_system": "DEALERSHIP_WEBSITE",
    "source_record_id": "WEB-LEAD-78452",
    "source_event_id": "FORM-SUBMISSION-88412",
    "source_received_at": "2026-08-01T17:00:00Z",
    "source_authority": "CUSTOMER_SUBMITTED"
  },
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "submitted_party": {
    "display_name": "Ahmed Hassan",
    "phone": "+201012345678",
    "email": "ahmed@example.com",
    "preferred_language": "ar-EG"
  },
  "inquiry": {
    "inquiry_text": "I would like to know the price and availability of a family SUV.",
    "vehicle_interest_text": "New family SUV",
    "requested_action": "REQUEST_INFORMATION",
    "finance_interest": true
  },
  "marketing_attribution": {
    "utm_source": "google",
    "utm_medium": "cpc",
    "utm_campaign": "summer_suv"
  },
  "raw_payload_reference": "evidence://lead-payloads/WEB-LEAD-78452",
  "raw_payload_hash": "sha256:6f1e8a7d..."
}
```

### Example Response

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "RECEIVED",
  "identity_resolution_status": "NOT_STARTED",
  "contact_validation_status": "NOT_STARTED",
  "intent_classification_status": "NOT_STARTED",
  "permission_assessment_status": "NOT_STARTED",
  "duplicate_assessment_status": "NOT_STARTED",
  "qualification_status": "NOT_STARTED",
  "assignment_status": "UNASSIGNED",
  "data_quality_status": "INCOMPLETE",
  "record_version": 1,
  "created_at": "2026-08-01T17:00:01Z"
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
- Source-evidence protection.
- Lifecycle validation.
- Permission checks.
- Duplicate checks.
- Required Human Decision or applicable automation policy.
- Audit recording.
- Event publication after accepted state change.
- External Confirmation tracking where applicable.

### Optimistic Concurrency

Updates must use an approved concurrency mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response.

### Idempotency

Retryable Lead creation and Command operations must support:

```text
Idempotency-Key
```

The same key and request intent must not create duplicate:

- Leads.
- Assignment requests.
- Qualification Decisions.
- Qualified Leads.
- Customer records.
- Review tasks.
- Outbound responses.

### Qualification Response

A successful qualification transaction may return:

```json
{
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "lead_status": "QUALIFIED",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "record_version": 8,
  "qualification_decision_id": "b5ab02ac-a6c9-4c0b-9dcc-49ed4641f5fe"
}
```

The API must not return `QUALIFIED` until the controlled Customer and Qualified Lead transaction succeeds.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `SOURCE_DUPLICATE`
- `SOURCE_EVIDENCE_MISSING`
- `IDENTITY_AMBIGUOUS`
- `IDENTITY_CONFLICT`
- `CONTACT_PERMISSION_RESTRICTED`
- `DUPLICATE_REVIEW_REQUIRED`
- `QUALIFICATION_EVIDENCE_INCOMPLETE`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `INVALID_LIFECYCLE_TRANSITION`
- `RECORD_DUPLICATE`
- `RECORD_INVALID`
- `RECORD_ARCHIVED`
- `RECONCILIATION_REQUIRED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Source-evidence protection.
- Field authority.
- Permission checks.
- Lifecycle validation.
- Concurrency.
- Idempotency.
- Human Review.
- Audit requirements.

GraphQL resolvers must not bypass Lead Domain Service or deterministic policy controls.

---

## 11. Database Design

### Recommended Tables

```text
leads
lead_source_evidence
lead_submitted_contact_points
lead_marketing_attribution
lead_assignments
lead_assignment_history
lead_identity_matches
lead_duplicate_candidates
lead_duplicate_decisions
lead_qualification_runs
lead_qualification_evidence
lead_qualification_decisions
lead_permission_assessments
lead_review_tasks
lead_derived_intelligence
lead_status_history
lead_reconciliation_cases
lead_data_quality_issues
lead_record_versions
lead_audit_log
```

### Leads Table

The `leads` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Current source projection.
- Current submitted-party projection.
- Current inquiry projection.
- Current lifecycle state.
- Current Customer and Qualified Lead relationships.
- Current assignment projection.
- Current qualification projection.
- Current permission projection.
- Current response metrics.
- Current data-quality and conflict state.
- Record version.
- Audit timestamps.

Original payload and historical evidence should remain in protected evidence or history tables.

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

idx_leads_tenant_received_at
  (tenant_id, source_received_at)

idx_leads_tenant_assignment
  (tenant_id, assignment_status, assigned_owner_user_id)

idx_leads_tenant_dealership_branch
  (tenant_id, dealership_id, branch_id)

idx_leads_tenant_customer
  (tenant_id, customer_id)

idx_leads_tenant_phone
  (tenant_id, submitted_phone)

idx_leads_tenant_email
  (tenant_id, submitted_email)

idx_leads_tenant_source
  (tenant_id, source_system, source_record_id)

idx_leads_tenant_campaign
  (tenant_id, campaign_id, source_received_at)

idx_leads_duplicate_target
  (tenant_id, duplicate_of_lead_id)

idx_leads_review_queue
  (tenant_id, review_status, requires_human_review)

idx_leads_updated_at
  (tenant_id, updated_at)
```

Sensitive contact indexes should use approved protected-indexing or tokenization methods where required.

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

when the external source guarantees record uniqueness.

```text
UNIQUE (tenant_id, source_system, source_event_id)
```

when the source Event identifier is stable and unique.

A payload hash must not always be globally unique because two legitimate submissions may contain identical content.

Payload-hash deduplication must use source, time, and policy context.

### Source Evidence

`lead_source_evidence` should preserve:

- Source-evidence identifier.
- `tenant_id`.
- `lead_id`.
- Source system.
- Source record.
- Source Event.
- Original receipt time.
- Original content reference.
- Raw payload reference.
- Integrity hash.
- Content type.
- Security classification.
- Retention class.
- Legal-hold state.
- Ingestion connector.
- Ingestion batch.
- Processing status.

Original evidence must be immutable except for lawful governed redaction or deletion.

### Assignment History

`lead_assignment_history` should preserve:

- Assignment identifier.
- `tenant_id`.
- `lead_id`.
- Previous owner.
- New owner.
- Team.
- Queue.
- Assignment rule.
- Reason.
- Actor.
- Effective time.
- Expiration.
- Record version.
- Related Event.

### Duplicate Evidence

`lead_duplicate_candidates` should preserve:

- Candidate identifier.
- Lead.
- Candidate Lead or Customer.
- Compared evidence types.
- Scores.
- Model or rule version.
- Detected time.
- Review status.
- Final Decision.
- Decision maker.
- Evidence references.

### Qualification Storage

`lead_qualification_runs` should preserve:

- Qualification-run identifier.
- `tenant_id`.
- `lead_id`.
- Lead record version.
- Policy identifier and version.
- Evidence references.
- Model or rules version.
- Prompt version where applicable.
- Scores.
- Missing evidence.
- Recommendation.
- Final Decision.
- Decision authority.
- Created Customer.
- Created Qualified Lead.
- Timestamp.
- Related Events.

### Permission Assessments

`lead_permission_assessments` should preserve:

- Assessment identifier.
- `tenant_id`.
- `lead_id`.
- Channel.
- Purpose.
- Permission status.
- Legal basis.
- Source.
- Evidence.
- Effective period.
- Actor.
- Policy version.
- Timestamp.

### Derived Intelligence

Derived Lead data must remain separate from authoritative source evidence.

Each derived record should preserve:

- Output type.
- Value.
- Model, formula, or algorithm version.
- Prompt version where applicable.
- Input-record versions.
- Evidence references.
- Confidence.
- Generated time.
- Expiration time.
- Review status.

### Audit Storage

Lead audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Audit storage should use secure hashes rather than raw sensitive values where full retention is unnecessary.

### Partitioning

High-volume deployments may partition by:

- `tenant_id`.
- Region.
- Source-receipt time.
- Retention class.
- Security class.

Partitioning must not weaken Tenant isolation, attribution, audit, or referential integrity.

### Hard Deletion

A Lead must not be hard-deleted when referenced by:

- Customer identity-resolution evidence.
- Qualified Lead.
- Opportunity attribution.
- Interaction.
- Appointment.
- Quotation.
- Campaign attribution.
- Human Decision.
- AI Agent Run.
- Audit evidence.
- Legal hold.

Lawful deletion or anonymization must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Exports.
- Non-authoritative replicas.
- Backups according to policy.

Required non-personal attribution and audit evidence may be retained only where lawful.

---

## 12. Security

### Security Classification

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Submitted name, phone, email, messaging identifier |
| `TECHNICAL_IDENTIFIER` | IP address, user agent, session identifier |
| `RAW_SOURCE_EVIDENCE` | Raw payload, recording, transcript, original message |
| `COMMUNICATION_PERMISSION` | Permission status, basis, evidence |
| `MARKETING_ATTRIBUTION` | Campaign and source metadata |
| `COMMERCIAL_CONFIDENTIAL` | Lead score, budget range, priority |
| `DERIVED_INTELLIGENCE` | Intent, sentiment, duplicate score, Recommendations |
| `AUDIT_EVIDENCE` | Decisions, policies, actor, source, record history |

### Authentication

Every internal Lead operation requires an authenticated Human or service identity.

Public Lead-submission endpoints may accept unauthenticated Customer submissions only through approved controls.

Public intake must use:

- Rate limiting.
- Bot and abuse protection.
- Payload validation.
- Source verification where supported.
- Tenant-safe routing.
- Content-size limits.
- Malware and malicious-content controls.
- Secure evidence storage.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealership.
- Branch.
- Team.
- Queue.
- Assigned owner.
- Role.
- Requested field.
- Requested action.
- Lead state.
- Data classification.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### BDC Agent

May access:

- Assigned Leads.
- Permitted shared queues.
- Approved contact information.
- Qualification workflow required for the role.

Must not independently:

- Override `DO_NOT_CONTACT`.
- Merge Customer identity.
- Change original source evidence.
- Approve restricted actions.

#### Sales Consultant

May access:

- Leads assigned to the User.
- Leads assigned to approved shared teams.
- Customer-visible inquiry and Vehicle-interest context.

Access to raw payload, IP information, fraud evidence, and unrestricted marketing data may be limited.

#### Sales Manager

May access Leads inside approved organizational scope.

Manager access does not automatically authorize:

- Consent override.
- Cross-Tenant search.
- Original evidence modification.
- Privacy deletion.
- Fraud clearance.
- Customer merge.

#### Marketing User

May access approved:

- Source attribution.
- Campaign attribution.
- Aggregated Lead performance.
- Redacted Lead-level details where permitted.

Direct contact and raw payload access must be restricted by purpose.

#### Data Steward

May review:

- Duplicate candidates.
- Identity conflicts.
- Source mappings.
- Data-quality issues.
- Reconciliation cases.

#### Compliance or Legal Reviewer

May access restricted Lead evidence required for an assigned case.

#### AI Agent

May access only the minimum Lead context required for its approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from unrestricted PII retrieval.
- Prevented from cross-Tenant matching.

#### Integration Service

May create source observations and approved projections.

It must not:

- Modify accepted original evidence.
- Qualify Leads without applicable policy.
- Bypass permission controls.
- Access unrelated Tenant data.

### Encryption

- Lead data in transit must use approved encryption.
- Lead data at rest must use approved encryption.
- Contact points, technical identifiers, and raw evidence may require field-level encryption, tokenization, or equivalent protection.
- Encryption keys must be managed outside application source code and Prompts.
- Evidence repositories must use controlled access and integrity protection.

### Masking

Interfaces, Logs, analytics, and AI context must mask sensitive fields according to role and purpose.

Examples:

```text
Phone: +20******5678
Email: a*****@example.com
IP address: 197.***.***.24
```

### Raw Evidence Protection

Access to raw payload, recordings, transcripts, and source metadata must:

- Require explicit authorization.
- Be logged.
- Be purpose-limited.
- Use secure retrieval.
- Prevent uncontrolled export.
- Respect retention and legal holds.
- Avoid inclusion in ordinary application Logs.

### Contact Permission Enforcement

Outbound communication must be blocked unless deterministic controls confirm:

- Permitted purpose.
- Permitted channel.
- Valid contact path.
- Applicable Consent or contact basis.
- Frequency limits.
- Time restrictions.
- Lead state.
- Customer restrictions.
- Tenant policy.
- Human Approval or approved automation policy.

Prompt text or AI Recommendation must not override these controls.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Duplicate detection.
- Customer matching.
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

### Audit Requirements

Material Lead activity must record:

- `tenant_id`.
- `lead_id`.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Source.
- Source authority.
- Record version.
- Applied policy.
- AI involvement.
- Human Decision.
- Automation-policy reference.
- Evidence.
- Timestamp.
- Correlation identifier.
- Causation identifier.
- Related Event.
- Related Command.
- External Confirmation where applicable.

### Security Events

ASOS must detect and record:

- Cross-Tenant Lead access attempts.
- Unauthorized contact-data access.
- Bulk Lead export attempts.
- Raw-payload access.
- Original-source evidence modification attempts.
- Unauthorized permission override.
- Assignment abuse.
- Duplicate-classification manipulation.
- AI access outside approved scope.
- Prompt-injection attempts inside inquiry content.
- Malicious payloads.
- Automated submission abuse.
- Command replay.
- Audit-log tampering.

### Prompt-Injection and Untrusted Content

Lead inquiry content is untrusted input.

AI Agents must treat:

- Inquiry text.
- Emails.
- Documents.
- Transcripts.
- Website submissions.
- Provider payloads.

as data, not trusted instructions.

Untrusted content must not:

- Change system policy.
- Grant tool access.
- Override Tenant scope.
- Reveal secrets.
- Bypass authorization.
- Cause external execution without policy validation.

### Retention and Privacy

Lead retention must follow:

- Applicable law.
- Tenant policy.
- Consent status.
- Source contract.
- Campaign requirements.
- Legal hold.
- Related Customer and Deal obligations.

Privacy workflows must support:

- Access.
- Correction.
- Restriction.
- Export.
- Deletion or anonymization.
- Consent management.
- Legal hold.

Privacy actions must not destroy required legal, security, contractual, attribution, or audit evidence unlawfully.

### Emergency Controls

The platform must support immediate Tenant-scoped suspension of:

- Lead ingestion.
- Automated response.
- Lead assignment.
- External write-back.
- AI qualification.
- Campaign processing.
- Lead export.
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
- [ASOS Interaction](./Interaction.md)

---

## Current Status

This document is the approved Canonical Lead baseline.

The original Lead inquiry remains historically traceable after Customer resolution, qualification, duplicate classification, disqualification, or invalidation.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
