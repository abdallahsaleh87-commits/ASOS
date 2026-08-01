# Lead

## 1. Object Purpose

### Business Purpose

The Lead object represents an unverified initial contact that may have automotive purchase intent. It captures every inbound inquiry before the dealership confirms the person's identity, contact validity, requirements, or commercial intent.

The Lead object preserves top-of-funnel marketing attribution, enables accurate response-time measurement, prevents duplicate follow-up, and ensures that no potential customer inquiry is lost before qualification.

### System Purpose

The Lead object is the canonical ingestion entity for raw inquiries entering ASOS from Layer 1 sources such as websites, WhatsApp, phone calls, email, marketplaces, OEM platforms, social media, referrals, and showroom walk-ins.

It provides an immutable record of the original inquiry, supports deterministic deduplication, and supplies structured and unstructured data to the Lead Qualification Agent.

A Lead remains separate from a Customer and Qualified Lead until identity resolution, contact validation, and automotive intent verification are completed.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `lead_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:**
  - `customer_id` (UUIDv4 — optional; populated after identity resolution)
  - `owner_id` (UUIDv4 — optional; assigned Sales Consultant or BDC Agent)
  - `duplicate_of_lead_id` (UUIDv4 — optional; populated when the Lead is classified as a duplicate)

### Data Payload

- **Contact Fields:** `first_name`, `last_name`, `primary_phone`, `email`
- **Inquiry Fields:** `inquiry_text`, `vehicle_interest_text`, `preferred_language`
- **Source Fields:** `source_channel`, `source_system`, `source_record_id`
- **Marketing Attribution:** `campaign_id`, `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`
- **Qualification Fields:** `contact_validity_status`, `intent_status`, `qualification_score`, `disqualification_reason`
- **Permission Fields:** `contact_permission_status`
- **Assignment Fields:** `owner_id`, `assigned_at`
- **Duplicate Fields:** `duplicate_score`, `duplicate_of_lead_id`
- **Computed Fields:** `response_time_seconds`, `assignment_delay_seconds`, `lead_age_minutes`

### Governance & Lifecycle

- **Raw Source Payload:** `raw_payload` (JSONB)
- **Payload Integrity:** `raw_payload_hash`
- **Metadata:** `ip_address`, `user_agent`, `landing_page_url`, `referrer_url`
- **Audit Fields:** `created_by`, `updated_by`, `last_processed_by_agent`
- **Version:** `record_version` (Integer — optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** `owner_id` (UUIDv4 — optional until assignment)
- **Timestamps:**
  - `received_at`
  - `created_at`
  - `updated_at`
  - `assigned_at`
  - `first_response_at`
  - `last_contact_attempt_at`
  - `qualified_at`
  - `disqualified_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| lead_id | UUID | Unique canonical identifier for the Lead. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Identifies the dealership tenant receiving the Lead. | Yes | Active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| customer_id | UUID | Customer identity resolved from the Lead. | No | Null | Must reference a Customer in the same dealership | 456e7890-e12b-34d5-a678-426614174000 | System-controlled |
| source_channel | Enum | Communication channel through which the Lead entered ASOS. | Yes | IMPORT | Must match LeadSourceChannel ENUM | WHATSAPP | At least 0.99 |
| source_system | String | External platform that supplied the Lead. | No | Null | Maximum 100 characters | Meta Lead Ads | Source-provided |
| source_record_id | String | Identifier assigned by the external source. | No | Null | Unique with dealership and source system when supplied | META-985467 | Source-provided |
| first_name | String | Unverified given name supplied by the Lead. | No | Null | Maximum 50 characters | Ahmed | At least 0.85 if AI-extracted |
| last_name | String | Unverified family name supplied by the Lead. | No | Null | Maximum 50 characters | Hassan | At least 0.85 if AI-extracted |
| primary_phone | String | Primary telephone number supplied with the inquiry. | Conditional | Null | Must use normalized E.164 format when present | +201012345678 | At least 0.95 if AI-extracted |
| email | String | Email address supplied with the inquiry. | Conditional | Null | Must use a valid normalized email format | ahmed@example.com | At least 0.90 if AI-extracted |
| inquiry_text | Text | Original customer message or transcription. | No | Null | Maximum 10,000 characters | I am interested in a new SUV. | Source-provided |
| vehicle_interest_text | Text | Unstructured description of the requested vehicle. | No | Null | Maximum 2,000 characters | White family SUV under 2 million EGP | At least 0.80 |
| preferred_language | String | Preferred communication language inferred or supplied. | No | Null | Must use an approved language code | ar-EG | At least 0.90 |
| status | Enum | Current Lead validation and qualification state. | Yes | RECEIVED | Must match LeadStatus ENUM | VALIDATING | At least 0.99 |
| contact_validity_status | Enum | Result of contact-information validation. | Yes | UNKNOWN | Must match ContactValidityStatus ENUM | VALID | At least 0.95 |
| intent_status | Enum | Classification of the inquiry's automotive intent. | Yes | UNKNOWN | Must match IntentStatus ENUM | AUTOMOTIVE_INTENT | At least 0.85 |
| contact_permission_status | Enum | Whether ASOS may initiate or continue outbound communication. | Yes | UNKNOWN | Must match ContactPermissionStatus ENUM | PERMITTED | At least 0.99 |
| qualification_score | Decimal | Combined confidence that the Lead is contactable and has valid automotive intent. | No | 0.00 | Must be between 0.00 and 1.00 | 0.93 | System-computed |
| duplicate_score | Decimal | Probability that the Lead duplicates an existing Lead or Customer inquiry. | No | 0.00 | Must be between 0.00 and 1.00 | 0.97 | System-computed |
| duplicate_of_lead_id | UUID | Canonical Lead record that this Lead duplicates. | No | Null | Required when status is DUPLICATE | 789e0123-e45b-67d8-a901-426614174000 | System-controlled |
| owner_id | UUID | User responsible for the Lead. | No | Null | Must reference an active User in the same dealership | 321e6547-e89b-12d3-a456-426614174000 | System or human assignment |
| disqualification_reason | Enum | Reason the Lead failed qualification. | No | Null | Required when status is DISQUALIFIED or INVALID | NO_AUTOMOTIVE_INTENT | At least 0.90 |
| raw_payload | JSONB | Immutable original payload received from the source system. | Yes | Empty object | Must remain unchanged after ingestion | {"message":"Interested in Civic"} | Source-provided |
| raw_payload_hash | String | Hash used for payload integrity and idempotency checks. | Yes | System-generated | Must use an approved cryptographic hash | sha256:a8f5... | System-generated |
| received_at | Timestamp | Time the inquiry was received by the dealership or source system. | Yes | Current timestamp | Cannot be materially later than created_at | 2026-08-01T10:30:00Z | Source or system |
| first_response_at | Timestamp | Time of the first valid dealership response. | No | Null | Must be equal to or later than received_at | 2026-08-01T10:32:00Z | System-recorded |
| response_time_seconds | Integer | Time between receipt and first valid response. | No | Null | Must be zero or greater | 120 | System-computed |

> At least one of `primary_phone`, `email`, or a documented walk-in identity reference must be present.

## 4. Enumerations

### LeadStatus

- **RECEIVED:** The inquiry has entered ASOS but has not yet been evaluated.
- **VALIDATING:** Contact information, identity, intent, and duplication are being checked.
- **QUALIFIED:** The Lead passed validation and produced a Qualified Lead.
- **DISQUALIFIED:** The inquiry is valid but does not currently meet qualification requirements.
- **DUPLICATE:** The inquiry duplicates an existing canonical Lead.
- **INVALID:** The record is spam, corrupted, fraudulent, or contains no usable contact path.

### LeadSourceChannel

- WEBSITE
- WHATSAPP
- PHONE
- EMAIL
- WALK_IN
- SOCIAL_MEDIA
- MARKETPLACE
- OEM_PLATFORM
- REFERRAL
- IMPORT

### ContactValidityStatus

- **UNKNOWN:** Contact information has not been validated.
- **VALID:** At least one verified and usable contact method exists.
- **INVALID:** Supplied contact information is structurally invalid.
- **UNREACHABLE:** Contact information appears valid but repeated contact attempts failed.

### IntentStatus

- **UNKNOWN:** Automotive intent has not yet been determined.
- **AUTOMOTIVE_INTENT:** The inquiry relates to purchasing, financing, trading, reserving, or testing a vehicle.
- **NON_AUTOMOTIVE:** The inquiry does not represent an automotive sales opportunity.
- **SPAM:** The content is unsolicited, malicious, automated, or irrelevant.

### ContactPermissionStatus

- **UNKNOWN:** Permission or lawful contact basis has not been established.
- **PERMITTED:** Communication is allowed under the applicable consent or transactional basis.
- **RESTRICTED:** Only limited transactional communication is permitted.
- **OPT_OUT:** Outbound communication is prohibited.

### DisqualificationReason

- NO_VALID_CONTACT
- NO_AUTOMOTIVE_INTENT
- OUT_OF_MARKET
- UNREACHABLE
- OPT_OUT
- DUPLICATE
- SPAM
- TEST_RECORD
- FRAUDULENT
- INCOMPLETE_DATA
- OTHER

## 5. Validation Rules

### Business Rules

- A Lead represents an unverified inquiry and must not contain Opportunity, Quotation, Deal, or negotiation-stage data.
- At least one usable contact route or documented walk-in identity reference must be provided.
- A Lead cannot enter `QUALIFIED` unless valid automotive intent and a usable contact method are confirmed.
- Qualification must resolve or create a Customer and create a separate Qualified Lead record in one controlled transaction.
- A Lead marked `OPT_OUT` cannot trigger outbound AI or human follow-up.
- A Lead cannot be assigned to more than one active owner at the same time.
- A duplicate Lead must retain its original payload for attribution and audit purposes.

### Technical Rules

- Phone numbers must be normalized to E.164 before qualification.
- Email addresses must be trimmed, lowercased, and structurally validated.
- `raw_payload` becomes immutable immediately after successful ingestion.
- `raw_payload_hash` must be calculated before the ingestion transaction is committed.
- Source ingestion must support idempotent retries without creating multiple canonical Lead records.
- `record_version` must increment after every successful update.

### Data Constraints

- `qualification_score` and `duplicate_score` must remain between `0.00` and `1.00`.
- `first_response_at` cannot be earlier than `received_at`.
- `response_time_seconds` and `assignment_delay_seconds` cannot be negative.
- When supplied, the combination of `dealership_id`, `source_system`, and `source_record_id` must be unique.
- `duplicate_of_lead_id` cannot equal the current `lead_id`.

### Referential Integrity

- `customer_id`, `owner_id`, and `duplicate_of_lead_id` must belong to the same `dealership_id`.
- `duplicate_of_lead_id` must reference an existing non-deleted Lead.
- A qualified Lead cannot be hard-deleted if a Qualified Lead, Customer Journey, or Interaction Log references it.
- Lead deletion must use soft deletion unless a legally approved data-purge request applies.

### Human Approval Requirements

- If AI qualification confidence is below the approved threshold, the Lead must be routed for human review.
- Merging Lead identities requires human approval when the duplicate score is below `0.95`.
- Reopening a `QUALIFIED` Lead requires Sales Manager approval.
- Changing `contact_permission_status` from `OPT_OUT` requires verified human evidence and an immutable audit record.
- AI Agents cannot overwrite the original source attribution or raw payload.

## 6. State Machine

### Allowed States

- RECEIVED
- VALIDATING
- QUALIFIED
- DISQUALIFIED
- DUPLICATE
- INVALID

### Allowed Transitions

- RECEIVED → VALIDATING
- VALIDATING → QUALIFIED
- VALIDATING → DISQUALIFIED
- VALIDATING → DUPLICATE
- VALIDATING → INVALID
- DISQUALIFIED → VALIDATING
- INVALID → VALIDATING

### Forbidden Transitions

- QUALIFIED → RECEIVED
- QUALIFIED → VALIDATING
- DUPLICATE → QUALIFIED
- DUPLICATE → RECEIVED
- RECEIVED → QUALIFIED
- INVALID → QUALIFIED

### Entry Conditions

- To enter `VALIDATING`, the raw payload and source attribution must be stored successfully.
- To enter `QUALIFIED`:
  - `contact_validity_status` must equal `VALID`.
  - `intent_status` must equal `AUTOMOTIVE_INTENT`.
  - `contact_permission_status` cannot equal `OPT_OUT`.
  - `qualification_score` must meet the approved threshold.
  - `customer_id` must be resolved or created.
  - A Qualified Lead record must be created successfully.
- To enter `DUPLICATE`, `duplicate_of_lead_id` and supporting match evidence must exist.
- To enter `DISQUALIFIED`, a valid `disqualification_reason` must be recorded.
- To enter `INVALID`, evidence of unusable, fraudulent, spam, or corrupted data must be recorded.

### Exit Conditions

- A Lead cannot exit `RECEIVED` until payload-integrity validation completes.
- A Lead cannot exit `VALIDATING` without a documented qualification, disqualification, duplicate, or invalid decision.
- A Lead may exit `DISQUALIFIED` or `INVALID` only when new evidence or corrected contact data is supplied.

### Terminal States

- **QUALIFIED:** The Lead journey continues through the separate Qualified Lead object.
- **DUPLICATE:** The original payload remains stored, but all active processing continues against the surviving Lead.

## 7. Relationships

- **Depends On:** The Dealership tenant identified by `dealership_id`.
- **Consumes:** Raw inquiries received from websites, WhatsApp, phone calls, email, social media, marketplaces, OEM platforms, referrals, imports, and showroom walk-ins.
- **Produces:** A Qualified Lead after successful identity, contact, intent, and permission validation.
- **Creates:** A Customer record when no matching canonical Customer exists.
- **Triggers:** Lead assignment, qualification, response-time tracking, duplicate detection, and follow-up workflows.
- **Owned By:** A Sales Consultant, BDC Agent, or dealership assignment queue.
- **Referenced By:** Qualified Lead, Customer, Interaction Log, Task, Campaign Attribution, and AI Agent Run records.

## 8. Domain Events

### Emitted Events

- **LeadReceived**  
  Payload: `lead_id`, `dealership_id`, `source_channel`, `received_at`

- **LeadValidationStarted**  
  Payload: `lead_id`, `started_at`, `processing_agent_id`

- **LeadAssigned**  
  Payload: `lead_id`, `owner_id`, `assigned_at`

- **LeadFirstResponseRecorded**  
  Payload: `lead_id`, `first_response_at`, `response_time_seconds`

- **LeadQualified**  
  Payload: `lead_id`, `customer_id`, `qualified_lead_id`, `qualification_score`

- **LeadDisqualified**  
  Payload: `lead_id`, `disqualification_reason`, `qualification_score`

- **LeadDuplicateDetected**  
  Payload: `lead_id`, `duplicate_of_lead_id`, `duplicate_score`

- **LeadInvalidated**  
  Payload: `lead_id`, `disqualification_reason`, `invalidated_at`

- **LeadContactPermissionChanged**  
  Payload: `lead_id`, `old_status`, `new_status`, `changed_at`

### Consumed Events

- **InboundInquiryReceived**  
  Creates the canonical Lead and stores the immutable raw source payload.

- **ContactValidationCompleted**  
  Updates `contact_validity_status` and advances the qualification workflow.

- **CustomerIdentityResolved**  
  Populates `customer_id` when the Lead matches an existing Customer.

- **AgentResponseSent**  
  Populates `first_response_at` when the first valid response is delivered.

- **ContactOptOutReceived**  
  Changes `contact_permission_status` to `OPT_OUT` and suspends outbound activity.

## 9. AI Considerations

### Fields Used for Vector Embeddings

- `inquiry_text`
- `vehicle_interest_text`
- Extracted automotive requirements
- Extracted purchase intent
- Objections and customer concerns
- Conversation summaries
- Preferred vehicle characteristics
- Timing and urgency indicators

### Fields Excluded from Embeddings — PII Protection

- `first_name`
- `last_name`
- `primary_phone`
- `email`
- `ip_address`
- `raw_payload`
- `source_record_id`

> Personally identifiable information must be supplied to AI Agents through controlled structured context, not retrieved through unrestricted semantic search.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every retrieval query.
- `lead_id`
- `status`
- `source_channel`
- `preferred_language`
- `contact_permission_status`

### Confidence Thresholds

- Contact-information extraction requires confidence of at least `0.95`.
- Automotive-intent classification requires confidence of at least `0.85`.
- Vehicle-interest extraction requires confidence of at least `0.80`.
- Automatic duplicate classification requires a duplicate score of at least `0.98`.
- Qualification requires the configured dealership threshold and must never rely on one signal alone.

### Human Approval Thresholds

- Duplicate matches below `0.98` must be routed for human review.
- AI Agents cannot reverse an `OPT_OUT` status.
- AI Agents cannot overwrite original source attribution or `raw_payload`.
- Low-confidence qualification decisions must create a human-review Task.
- Identity merges involving conflicting phone numbers or email addresses require human approval.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/leads`
- **Methods:**
  - `GET` — list or search Leads.
  - `POST` — ingest a new Lead.
  - `GET /{id}` — retrieve one Lead.
  - `PATCH /{id}` — update permitted Lead fields.
  - `POST /{id}/assign` — assign the Lead to a User or queue.
  - `POST /{id}/validate` — start or repeat validation.
  - `POST /{id}/qualify` — execute the controlled qualification transaction.
  - `POST /{id}/disqualify` — record a disqualification decision.
  - `POST /{id}/mark-duplicate` — link the Lead to a surviving Lead.
  - `DELETE /{id}` — perform a soft delete.

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateLeadRequest",
  "type": "object",
  "properties": {
    "source_channel": {
      "type": "string",
      "enum": [
        "WEBSITE",
        "WHATSAPP",
        "PHONE",
        "EMAIL",
        "WALK_IN",
        "SOCIAL_MEDIA",
        "MARKETPLACE",
        "OEM_PLATFORM",
        "REFERRAL",
        "IMPORT"
      ]
    },
    "source_system": {
      "type": ["string", "null"],
      "maxLength": 100
    },
    "source_record_id": {
      "type": ["string", "null"],
      "maxLength": 255
    },
    "first_name": {
      "type": ["string", "null"],
      "maxLength": 50
    },
    "last_name": {
      "type": ["string", "null"],
      "maxLength": 50
    },
    "primary_phone": {
      "type": ["string", "null"],
      "pattern": "^\\+?[1-9]\\d{1,14}$"
    },
    "email": {
      "type": ["string", "null"],
      "format": "email"
    },
    "inquiry_text": {
      "type": ["string", "null"],
      "maxLength": 10000
    },
    "vehicle_interest_text": {
      "type": ["string", "null"],
      "maxLength": 2000
    },
    "preferred_language": {
      "type": ["string", "null"],
      "maxLength": 20
    },
    "contact_permission_status": {
      "type": "string",
      "enum": ["UNKNOWN", "PERMITTED", "RESTRICTED", "OPT_OUT"]
    },
    "received_at": {
      "type": "string",
      "format": "date-time"
    },
    "raw_payload": {
      "type": "object"
    }
  },
  "required": [
    "source_channel",
    "contact_permission_status",
    "received_at",
    "raw_payload"
  ],
  "anyOf": [
    {
      "required": ["primary_phone"]
    },
    {
      "required": ["email"]
    },
    {
      "required": ["inquiry_text"]
    }
  ],
  "additionalProperties": false
}
```

### GraphQL Type

```graphql
type Lead {
  id: ID!
  dealershipId: ID!
  customerId: ID
  ownerId: ID
  sourceChannel: LeadSourceChannel!
  sourceSystem: String
  sourceRecordId: String
  firstName: String
  lastName: String
  primaryPhone: String
  email: String
  inquiryText: String
  vehicleInterestText: String
  preferredLanguage: String
  status: LeadStatus!
  contactValidityStatus: ContactValidityStatus!
  intentStatus: IntentStatus!
  contactPermissionStatus: ContactPermissionStatus!
  qualificationScore: Float!
  duplicateScore: Float!
  duplicateOfLeadId: ID
  disqualificationReason: DisqualificationReason
  receivedAt: DateTime!
  assignedAt: DateTime
  firstResponseAt: DateTime
  responseTimeSeconds: Int
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Primary Table:** `leads`
- **Immutable Source Table:** `lead_raw_payloads`
- **Audit Table:** `lead_audit_log`
- **Qualification Evidence Table:** `lead_qualification_evidence`
- **Duplicate Evidence Table:** `lead_duplicate_matches`

### Indexes

- `idx_leads_tenant_status (dealership_id, status)`  
  Used for qualification queues and operational dashboards.

- `idx_leads_tenant_phone (dealership_id, primary_phone)`  
  Used for Customer and Lead identity resolution.

- `idx_leads_tenant_email (dealership_id, email)`  
  Used for duplicate detection and Customer matching.

- `idx_leads_owner_status (dealership_id, owner_id, status)`  
  Used for Sales Consultant and BDC work queues.

- `idx_leads_received_at (dealership_id, received_at DESC)`  
  Used for response-time monitoring.

- `idx_leads_source (dealership_id, source_channel, source_system)`  
  Used for source-performance and attribution analysis.

- `idx_leads_duplicate_target (duplicate_of_lead_id)`  
  Used to retrieve all duplicate records linked to a surviving Lead.

### Unique Constraints

- `UQ_lead_source_record (dealership_id, source_system, source_record_id)`  
  Applies when both source fields are present.

- `UQ_lead_payload_hash (dealership_id, raw_payload_hash)`  
  Prevents repeated ingestion of the same source payload when appropriate.

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `customer_id` → `customers(id)` — nullable
- `owner_id` → `users(id)` — nullable
- `duplicate_of_lead_id` → `leads(id)` — nullable
- `campaign_id` → `campaigns(id)` — nullable

### Partition Keys

- Partition the Lead dataset by `dealership_id`.
- High-volume deployments may sub-partition historical Lead records by `received_at`.
- Tenant and time partitions must preserve source attribution, audit history, and referential integrity.

## 12. Security

### RBAC — Role-Based Access Control

- **BDC Agent:** Read/Write access to assigned Leads and unassigned Leads available within the dealership queue.
- **Sales Consultant:** Read/Write access only to Leads assigned to them, unless dealership policy permits shared-pool access.
- **Sales Manager:** Read/Write access to all Leads within the matching `dealership_id`.
- **Marketing User:** Read access to source attribution and aggregated performance data, with restricted access to direct contact information.
- **AI Qualification Agent:** Service Account access limited to validation, classification, scoring, and approved state transitions.
- **Integration Service:** Create-only access for source ingestion, with no permission to modify an accepted `raw_payload`.

### PII Classification

- **Level:** `CRITICAL_PII`
- **Fields:**
  - `first_name`
  - `last_name`
  - `primary_phone`
  - `email`
  - `ip_address`
  - `user_agent`
  - `raw_payload`

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for database volumes and backups.
- **Column-Level Protection:** Phone numbers, email addresses, and source IP addresses require encryption, tokenization, or an equivalent approved protection method.
- Encryption keys must be isolated by environment and rotated according to the security policy.

### Audit Requirements

- Every status transition must generate an immutable audit entry.
- Assignment and reassignment must record the previous owner, new owner, actor, timestamp, and reason.
- Qualification and disqualification must preserve the evidence, thresholds, Agent version, and decision path.
- Duplicate classification must preserve every compared identifier and score.
- Changes to `contact_permission_status` must record the evidence and responsible Human or system actor.
- Access to `raw_payload` must be logged because it may contain unstructured PII.
- Human overrides of AI qualification decisions must retain both the original AI decision and the final human decision.

### Tenant Isolation

- Every query must include and enforce the authenticated `dealership_id`.
- Cross-tenant Lead searches, duplicate checks, vector retrievals, and exports are prohibited.
- Background Jobs and AI Agents must receive the tenant scope through signed execution context.

### Retention and Deletion

- Soft deletion is the default for operational Lead records.
- Legally approved deletion requests must purge or anonymize PII across the Lead, raw payload, vector store, audit references, backups, and downstream analytics according to the applicable retention policy.
