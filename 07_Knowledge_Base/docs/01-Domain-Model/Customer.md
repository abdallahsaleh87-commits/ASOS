# Customer

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Customer Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Customer Object represents the canonical identity and dealership relationship of an individual or organization within an ASOS Tenant.

It provides a consistent Customer identity across:

- Leads.
- Opportunities.
- Appointments.
- Quotations.
- Trade-In workflows.
- Finance Applications.
- Financial Contracts.
- Deals.
- Interactions.
- Customer-service and retention activities.

A Customer may represent:

- An individual retail Customer.
- A corporate or fleet Customer.
- An organization.
- A prospect whose identity has been sufficiently resolved.
- A previous Customer returning for another sales journey.

The Customer Object must remain independent of:

- A single Lead.
- A single Opportunity.
- A single Deal.
- A single dealership branch.
- A single external CRM or DMS vendor.

A Customer may participate in multiple sales journeys over time.

### System Purpose

The Customer Object provides the canonical party identity used to connect Customer-related records across approved systems.

It supports:

- Identity resolution.
- External-reference mapping.
- Contact-point management.
- Communication preferences.
- Consent evidence.
- Assignment and organizational context.
- Duplicate detection.
- Controlled Customer merging.
- Data-quality management.
- Customer relationship summaries.
- Derived Customer intelligence.
- Tenant-safe AI context retrieval.

The Customer Object is not automatically the legal System of Record for every identity, consent, tax, or contact field.

External authority must be determined at field level according to the approved Field Authority Registry.

---

## 2. Canonical Schema

### Primary Identifiers

- `customer_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `originating_dealership_id` — UUIDv4, optional.
- `originating_branch_id` — UUIDv4, optional.
- `assigned_owner_user_id` — UUIDv4, optional.
- `assigned_team_id` — UUIDv4, optional.

The Customer belongs to the Tenant.

Dealership and branch identifiers provide organizational context inside the Tenant and do not replace `tenant_id` as the isolation boundary.

### Customer Classification

- `customer_type` — Individual or organization.
- `status` — Current canonical Customer lifecycle state.
- `identity_verification_status` — Current identity-verification state.
- `contactability_status` — Derived summary of permitted Customer contact.
- `data_quality_status` — Current Customer-data quality state.

### Identity Fields

For an individual:

- `display_name`.
- `given_name`.
- `middle_name`.
- `family_name`.
- `preferred_name`.
- `date_of_birth` where lawfully required.
- `nationality_code` where lawfully required.

For an organization:

- `display_name`.
- `legal_name`.
- `trade_name`.
- `registration_number`.
- `tax_identifier` where lawfully required.
- `organization_type`.

### Contact and Address Structures

Customer contact information must be represented through governed child records:

- `contact_points`.
- `addresses`.
- `communication_consents`.
- `external_references`.
- `identity_evidence`.

A Customer may have multiple:

- Phone numbers.
- Email addresses.
- Messaging identifiers.
- Physical addresses.
- Consent records.
- External-system identifiers.

### Relationship and Assignment Fields

- `preferred_language`.
- `preferred_contact_channel`.
- `relationship_started_at`.
- `last_verified_at`.
- `last_interaction_at`.
- `merged_into_customer_id`.
- `restriction_reason_code`.

### Derived Intelligence

Examples may include:

- `engagement_score`.
- `lifetime_value_amount`.
- `active_opportunity_count`.
- `completed_deal_count`.
- `days_since_last_interaction`.
- `predicted_conversion_probability`.
- `customer_summary`.
- `vehicle_preferences`.
- `next_best_action`.

Derived fields must preserve their model, formula, evidence, input-version, and freshness metadata.

### Provenance and Audit Fields

- `created_at`.
- `created_by_actor_type`.
- `created_by_actor_id`.
- `updated_at`.
- `updated_by_actor_type`.
- `updated_by_actor_id`.
- `source_system`.
- `source_record_id`.
- `authority_category`.
- `last_synced_at`.
- `last_sync_status`.
- `conflict_status`.
- `anonymized_at`.
- `merged_at`.

---

## 3. Field Definitions

### Core Customer Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `customer_id` | UUID | Yes | ASOS | Immutable Canonical Customer identifier. |
| `tenant_id` | UUID | Yes | ASOS Security Context | Primary Tenant-isolation identifier. |
| `customer_type` | Enum | Yes | Canonical Projection | Identifies an individual or organization. |
| `display_name` | String | Yes | Canonical Projection | Human-readable Customer name used in interfaces. |
| `status` | Enum | Yes | ASOS Workflow State | Canonical Customer lifecycle state. |
| `identity_verification_status` | Enum | Yes | ASOS Workflow State | Current verification state based on available evidence. |
| `contactability_status` | Enum | Yes | Derived | Summary generated from consent, preferences, restrictions, and valid contact points. |
| `data_quality_status` | Enum | Yes | ASOS Workflow State | Completeness, conflict, freshness, and quarantine status. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |
| `originating_dealership_id` | UUID | No | Canonical Projection | Dealership where the relationship was first observed. |
| `originating_branch_id` | UUID | No | Canonical Projection | Branch where the relationship was first observed. |
| `assigned_owner_user_id` | UUID | No | ASOS Workflow State | Current assigned User, where applicable. |
| `assigned_team_id` | UUID | No | ASOS Workflow State | Current assigned team, where applicable. |
| `preferred_language` | String | No | Customer or approved source | Preferred communication language. |
| `preferred_contact_channel` | Enum | No | Customer preference | Preferred permitted communication channel. |
| `relationship_started_at` | Timestamp | No | Canonical Projection | Earliest verified relationship timestamp. |
| `last_verified_at` | Timestamp | No | Evidence-driven | Most recent identity or contact verification timestamp. |
| `last_interaction_at` | Timestamp | No | Derived Projection | Most recent accepted related Interaction timestamp. |
| `merged_into_customer_id` | UUID | No | ASOS Workflow State | Surviving Customer after an approved merge. |
| `restriction_reason_code` | String | No | Authorized Human or policy | Reason the Customer is restricted. |
| `created_at` | Timestamp | Yes | ASOS | Record-creation timestamp. |
| `updated_at` | Timestamp | Yes | ASOS | Most recent accepted update timestamp. |

### Individual Identity Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `given_name` | String | Conditional | Approved source | Given name for an individual Customer. |
| `middle_name` | String | No | Approved source | Middle name where available. |
| `family_name` | String | Conditional | Approved source | Family name for an individual Customer. |
| `preferred_name` | String | No | Customer preference | Preferred non-legal name. |
| `date_of_birth` | Date | No | Verified external evidence | Date of birth only where required and lawfully processed. |
| `nationality_code` | String | No | Verified external evidence | ISO country code where required and lawful. |

### Organization Identity Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `legal_name` | String | Conditional | Verified external evidence | Registered legal organization name. |
| `trade_name` | String | No | Approved source | Public or commercial trading name. |
| `registration_number` | String | No | Verified external evidence | Organization registration identifier. |
| `tax_identifier` | String | No | Verified external evidence | Tax identifier where lawful and required. |
| `organization_type` | String | No | Approved source | Corporate, fleet, government, rental, or other organization category. |

### Contact Point Fields

Each contact point should include:

| Field | Description |
| :--- | :--- |
| `contact_point_id` | Canonical identifier for the contact point. |
| `tenant_id` | Mandatory Tenant boundary. |
| `customer_id` | Parent Customer identifier. |
| `contact_type` | Mobile phone, landline, email, messaging identifier, or another approved type. |
| `value` | Contact value stored under appropriate security controls. |
| `normalized_value` | Normalized value used for validation and matching. |
| `is_primary` | Indicates the preferred contact point of that type. |
| `verification_status` | Verification status of the contact point. |
| `verified_at` | Verification timestamp. |
| `source_system` | Source that provided the value. |
| `authority_category` | Authority classification. |
| `effective_from` | Start of validity. |
| `effective_until` | End of validity. |
| `is_active` | Whether the contact point remains active. |

### Communication Consent Fields

Each consent record should include:

| Field | Description |
| :--- | :--- |
| `consent_id` | Canonical Consent identifier. |
| `tenant_id` | Mandatory Tenant boundary. |
| `customer_id` | Parent Customer identifier. |
| `channel` | Email, SMS, phone, WhatsApp, or another approved channel. |
| `purpose` | Transactional, marketing, service, survey, or another approved purpose. |
| `status` | Granted, denied, revoked, expired, pending, unknown, or not required. |
| `legal_basis` | Applicable legal or contractual basis. |
| `jurisdiction` | Applicable jurisdiction where required. |
| `evidence_reference` | Reference to authoritative Consent evidence. |
| `captured_at` | Time Consent evidence was captured. |
| `effective_from` | Start of Consent validity. |
| `effective_until` | End of Consent validity. |
| `source_system` | Authoritative source. |
| `record_version` | Consent-record version. |

A single Customer-level Consent Boolean must not replace channel-, purpose-, and jurisdiction-specific Consent records.

---

## 4. Enumerations

### CustomerType

- `INDIVIDUAL`
- `ORGANIZATION`

### CustomerStatus

- `PENDING_VERIFICATION` — Identity or required evidence remains incomplete.
- `ACTIVE` — Customer record is available for permitted relationship activities.
- `INACTIVE` — No current active relationship workflow, but the record remains valid.
- `RESTRICTED` — Some or all Customer processing is blocked or limited.
- `MERGED` — Record has been merged into another surviving Customer.
- `ANONYMIZED` — Personal identifiers were removed or irreversibly transformed under an approved process.

### IdentityVerificationStatus

- `UNVERIFIED`
- `PARTIALLY_VERIFIED`
- `VERIFIED`
- `DISPUTED`
- `EXPIRED`

### ContactPointType

- `MOBILE_PHONE`
- `LANDLINE_PHONE`
- `EMAIL`
- `WHATSAPP`
- `OTHER_MESSAGING_IDENTIFIER`

### ContactPointVerificationStatus

- `UNVERIFIED`
- `PENDING`
- `VERIFIED`
- `FAILED`
- `EXPIRED`
- `DISPUTED`

### ConsentStatus

- `UNKNOWN`
- `PENDING`
- `GRANTED`
- `DENIED`
- `REVOKED`
- `EXPIRED`
- `NOT_REQUIRED`

### ContactabilityStatus

- `UNKNOWN`
- `CONTACTABLE`
- `LIMITED`
- `DO_NOT_CONTACT`

`contactability_status` is a derived summary.

It must not replace the underlying Consent, preference, legal-basis, suppression, and contact-point records.

### DataQualityStatus

- `COMPLETE`
- `INCOMPLETE`
- `STALE`
- `CONFLICTED`
- `QUARANTINED`

### PreferredContactChannel

- `PHONE`
- `EMAIL`
- `SMS`
- `WHATSAPP`
- `IN_PERSON`
- `NO_PREFERENCE`

### ActorType

- `HUMAN_USER`
- `ASOS_SERVICE`
- `AI_AGENT`
- `EXTERNAL_SYSTEM`
- `SYSTEM_PROCESS`

---

## 5. Validation Rules

### Tenant and Identity Rules

- `tenant_id` is required and immutable.
- Every child record must use the same `tenant_id` as its parent Customer.
- Cross-tenant Customer linking, matching, merging, or retrieval is prohibited unless governed by an approved and auditable data-sharing mechanism.
- `customer_id` must be unique inside ASOS.
- External identifiers must remain separate from `customer_id`.
- The same external reference must not map to multiple active Customers inside the same Tenant without a recorded conflict.

### Individual Customer Rules

When `customer_type = INDIVIDUAL`:

- `display_name` is required.
- At least one approved identity path must exist.
- The identity path may include a name plus a verified contact point, external reference, or identity-evidence record.
- Date of birth and nationality must not be collected unless required for an approved lawful purpose.

### Organization Customer Rules

When `customer_type = ORGANIZATION`:

- `display_name` is required.
- `legal_name` is required when the organization enters a regulated, contractual, tax, finance, or binding Deal workflow.
- Registration and tax identifiers must be verified through approved evidence where required.

### Contact Validation

- Phone numbers should be normalized to E.164 where supported.
- Email addresses must pass approved syntactic validation.
- Validation does not prove ownership or control of a contact point.
- Verified status requires approved verification evidence.
- A shared phone number or email address must not automatically cause Customer records to be merged.
- Contact values must not be globally unique because families, organizations, assistants, and shared devices may legitimately share contact points.

### Consent Validation

- Consent must not be inferred merely from Customer engagement or a successful message delivery.
- AI confidence must not create or change authoritative Consent.
- Every material Consent change must preserve source, purpose, channel, evidence, effective date, and version.
- `REVOKED` or `DENIED` Consent must block prohibited communication through deterministic controls.
- A preferred contact channel must not override a channel-specific Consent restriction.
- Consent changes requiring external write-back must remain pending until authoritative External Confirmation is received.

### Duplicate and Merge Validation

- Duplicate detection may create a suggested match or review task.
- Similarity alone must not merge Customers.
- A Customer merge requires an Authoritative Human Decision.
- The merge must identify one surviving Customer and one or more merged Customers.
- Merged records must preserve historical references and source identifiers.
- A merged Customer must not be reactivated as a separate Customer without a governed reversal or correction workflow.
- Related records must not be silently reassigned without audit evidence.

### Update and Concurrency Rules

- Every mutating operation must validate `record_version`.
- Conflicting concurrent updates must be rejected or routed to conflict resolution.
- Authoritative external fields must not be silently overwritten.
- Material changes must preserve previous value or secure hash, new value or secure hash, actor, source, reason, and timestamp.
- AI-extracted updates must remain suggestions until accepted under the applicable Action Class and authority policy.

### Deletion, Anonymization, and Retention

- Customer records with related Deals, Financial Contracts, Payments, legal evidence, or active obligations must not be hard-deleted.
- Approved anonymization may remove or transform personal identifiers while preserving required commercial, legal, financial, and audit evidence.
- Legal holds must block deletion or anonymization where required.
- `ANONYMIZED` records must not remain available for ordinary Customer targeting.
- Retention and deletion must follow Tenant, jurisdiction, contractual, and regulatory requirements.

---

## 6. State Machine

### Allowed States

```text
PENDING_VERIFICATION
ACTIVE
INACTIVE
RESTRICTED
MERGED
ANONYMIZED
```

### Allowed Transitions

```text
PENDING_VERIFICATION → ACTIVE
PENDING_VERIFICATION → RESTRICTED
PENDING_VERIFICATION → MERGED
PENDING_VERIFICATION → ANONYMIZED

ACTIVE → INACTIVE
ACTIVE → RESTRICTED
ACTIVE → MERGED
ACTIVE → ANONYMIZED

INACTIVE → ACTIVE
INACTIVE → RESTRICTED
INACTIVE → MERGED
INACTIVE → ANONYMIZED

RESTRICTED → ACTIVE
RESTRICTED → INACTIVE
RESTRICTED → MERGED
RESTRICTED → ANONYMIZED
```

### Terminal States

- `MERGED`
- `ANONYMIZED`

A terminal state may only be reversed through an approved correction, legal restoration, or controlled recovery process.

### Entry Conditions

#### Entering ACTIVE

Requires:

- Minimum valid identity data.
- No unresolved restriction blocking activation.
- Required Tenant and authority metadata.
- Valid record version.
- Permitted processing purpose.

An active Opportunity or Deal is not required for the Customer to remain `ACTIVE`.

#### Entering INACTIVE

May occur when:

- No current active Customer relationship workflow exists.
- The Customer remains legally retainable.
- No restriction requires the `RESTRICTED` state.

#### Entering RESTRICTED

May occur because of:

- Legal restriction.
- Compliance review.
- Fraud concern.
- Identity dispute.
- Data-quality quarantine.
- Customer complaint.
- Consent or communication restriction.
- Security incident.
- Another approved policy.

The restricted actions and reason must be recorded explicitly.

#### Entering MERGED

Requires:

- Authorized Human Approval.
- Surviving Customer identifier.
- Duplicate or identity-resolution evidence.
- Conflict review.
- Related-record migration plan.
- Immutable audit evidence.

#### Entering ANONYMIZED

Requires:

- Approved privacy or retention workflow.
- Legal-hold validation.
- Required authorization.
- Confirmation that required audit and transactional evidence will remain protected.

### Forbidden Transitions

- `MERGED → ACTIVE`
- `MERGED → INACTIVE`
- `ANONYMIZED → ACTIVE`
- `ANONYMIZED → INACTIVE`
- `ANONYMIZED → MERGED`

These changes require a separate governed correction or restoration process rather than an ordinary lifecycle transition.

### Transition Governance

Every state transition must preserve:

- Previous state.
- New state.
- Transition reason.
- Actor.
- Authority.
- Applicable policy.
- Evidence.
- Timestamp.
- Record version.
- Related Event.
- External Confirmation where required.

---

## 7. Relationships

### Tenant and Organization

- Every Customer belongs to exactly one `tenant_id`.
- A Customer may be associated with multiple dealerships or branches inside the same Tenant.
- Organizational associations must preserve role, effective dates, and access scope.

### Lead

- A Lead may exist before a Customer is created.
- Identity resolution may link one or more Leads to an existing Customer.
- Lead conversion must not automatically create a duplicate Customer.
- Original Lead evidence must remain preserved after linking.

### Qualified Lead

- A Qualified Lead may reference a Customer after identity resolution.
- Qualification evidence belongs to Qualified Lead, not Customer.

### Opportunity

- A Customer may have multiple Opportunities.
- Opportunity pipeline stage must not be stored as Customer lifecycle status.

### Appointment

- A Customer may have multiple Appointments.
- An externally confirmed Appointment remains authoritative in the configured Appointment System of Record.

### Quotation

- A Customer may receive multiple Quotations.
- Customer identity does not imply acceptance of a Quotation.

### Trade-In

- A Customer may participate in multiple Trade-In workflows.
- Legal Vehicle ownership evidence belongs to the Trade-In or supporting ownership evidence, not to an unsupported Customer assertion.

### Finance Application

- A Customer may have multiple Finance Applications.
- Sensitive finance information must remain in the Finance Application or approved external finance system.
- Customer must not duplicate detailed lender Decisions or restricted financial data.

### Financial Contract

- A Customer may be a party to multiple Financial Contracts.
- Contractual authority remains with the signed or externally confirmed contract evidence.

### Deal

- A Customer may participate in multiple Deals.
- Final Deal completion must be confirmed by the configured authoritative external system where applicable.

### Interaction

- A Customer may have multiple Interactions across approved channels.
- Original provider evidence remains authoritative for transport and delivery status.

### Customer Child Records

A Customer may own or govern:

- Contact points.
- Addresses.
- Communication Consent records.
- External references.
- Identity evidence references.
- Organizational associations.
- Merge records.
- Data-quality issues.
- Assignment history.

### Customer Merge Relationship

A merged Customer must reference:

```text
merged_into_customer_id
```

The surviving Customer must preserve a merge-history record referencing all merged Customer identifiers.

---

## 8. Domain Events

The Canonical Event Catalog is the authoritative source for final Event names, versions, envelopes, payloads, producers, Consumers, and compatibility rules.

The following are required Customer Event concepts and do not replace the Event Catalog.

### Customer Lifecycle Event Concepts

- Customer created.
- Customer activated.
- Customer made inactive.
- Customer restricted.
- Customer restriction removed.
- Customer merged.
- Customer anonymized.
- Customer status corrected.

### Identity and Contact Event Concepts

- Customer identity updated.
- Identity verification changed.
- External Customer reference linked.
- Contact point added.
- Contact point updated.
- Contact point verified.
- Contact point deactivated.
- Customer address updated.
- Customer identity conflict detected.
- Customer identity conflict resolved.

### Consent Event Concepts

- Customer Consent recorded.
- Customer Consent granted.
- Customer Consent denied.
- Customer Consent revoked.
- Customer Consent expired.
- Communication restriction applied.
- Communication restriction removed.

### Assignment Event Concepts

- Customer assigned.
- Customer reassigned.
- Customer unassigned.

### Derived Intelligence Event Concepts

- Customer summary generated.
- Customer engagement score updated.
- Customer preference profile updated.
- Customer next-best action generated.

Derived Intelligence Events must not imply Human Approval or external completion.

### Customer Event Producer Rules

- The Customer Domain Service publishes accepted Customer canonical and workflow-state changes.
- A Consent Service or Customer Domain Service may publish accepted Consent state changes according to the approved service boundary.
- An AI Agent may publish an Agent-run or Recommendation Event but must not publish an authoritative Customer identity, Consent, merge, or external-confirmation Event merely because it suggested the change.
- External source observations must remain distinguishable from accepted canonical state changes.

### Event Requirements

Every material Customer Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `customer_id`.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Correlation identifier.
- Causation identifier.
- Record version.
- Evidence references.
- Security classification.

Events are immutable.

Corrections and reversals must use new Events linked to the original Event.

Event Consumers must safely handle duplicate delivery using `event_id`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Suggested identity matching.
- Duplicate-candidate detection.
- Contact-detail extraction.
- Language detection.
- Customer-summary generation.
- Preference extraction.
- Objection summarization.
- Engagement analysis.
- Next-best-action generation.
- Data-quality issue detection.
- Missing-information detection.
- Interaction summarization.
- Customer-context preparation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Create authoritative Consent.
- Revoke or restore Consent.
- Confirm legal identity.
- Modify verified identity evidence.
- Merge Customer records.
- Anonymize a Customer.
- Remove legal or compliance restrictions.
- Change authoritative tax or registration identifiers.
- Confirm control of a phone number or email.
- Send Customer communication outside Human Approval or an approved automation policy.
- Represent a Recommendation as an Authoritative Human Decision.
- Represent a sent Command as externally confirmed.
- Retrieve Customer data across Tenants.

### AI Extraction Rules

AI-extracted information must include:

- Extracted value.
- Source reference.
- Source timestamp.
- Model and Prompt version.
- Confidence where meaningful.
- Evidence excerpt or structured reference.
- Authority classification.
- Required review.
- Expiration or freshness metadata.

A configured confidence threshold may determine whether an extraction:

- Is discarded.
- Creates a review task.
- Is accepted as a low-risk Canonical Projection.
- Requires Human Approval.

Confidence alone must not create authoritative identity, Consent, financial, legal, or contractual facts.

### Human Approval Requirements

Authorized Human Approval is required for:

- Customer merge.
- Material identity-conflict resolution.
- Legal-name correction without authoritative automated evidence.
- Restriction override.
- Customer anonymization approval where Human authorization is required.
- High-risk cross-system identity linkage.
- Another Action Class 3 Customer decision.

### Approved Automation Policy

Action Class 2 Customer-facing communication may use an approved automation policy only when the deterministic Policy Engine validates:

- Consent.
- Channel.
- Purpose.
- Template.
- Frequency.
- Time restrictions.
- Customer status.
- Contact-point validity.
- Tenant scope.
- Revocation state.
- Risk limits.
- Audit requirements.

### AI Context and Embeddings

Direct identifiers and highly sensitive fields must not be placed into vector embeddings unless an approved architecture explicitly permits and protects that use.

Fields normally excluded from embeddings include:

- Legal name where direct identification is unnecessary.
- Phone number.
- Email address.
- Physical address.
- Date of birth.
- National identifier.
- Tax identifier.
- Registration identifier.
- Consent evidence.
- Identity documents.
- Financial information.
- Contract documents.

Approved redacted or abstracted Customer context may include:

- Vehicle preferences.
- Non-sensitive objection categories.
- Relationship summaries.
- Communication-style preference.
- Sales-journey stage.
- Approved interaction summaries.

Every vector entry must enforce:

- `tenant_id`.
- Customer access scope.
- Source references.
- Retention policy.
- Deletion or anonymization propagation.
- Security classification.

### Explainability

Material Customer Recommendations must explain:

- Relevant evidence.
- Data freshness.
- Material conflicts.
- Derived assumptions.
- Confidence where meaningful.
- Recommended action.
- Required approval.
- Applicable communication restrictions.
- Important risks.

---

## 10. API Contract

Detailed API operations, request Schemas, response Schemas, and error definitions will become authoritative in the API Contracts Catalog.

This section defines the required Customer API behavior.

### REST Resources

```text
GET    /api/v1/customers
POST   /api/v1/customers
GET    /api/v1/customers/{customer_id}
PATCH  /api/v1/customers/{customer_id}
POST   /api/v1/customers/{customer_id}/contact-points
PATCH  /api/v1/customers/{customer_id}/contact-points/{contact_point_id}
POST   /api/v1/customers/{customer_id}/consents
POST   /api/v1/customers/{customer_id}/restrictions
POST   /api/v1/customers/{customer_id}/merge
POST   /api/v1/customers/{customer_id}/anonymization-requests
GET    /api/v1/customers/{customer_id}/history
```

### Tenant Context

- `tenant_id` must come from the authenticated security context.
- A client must not be allowed to override `tenant_id` in a request body.
- Dealership and branch scope must be validated against the authenticated User or service permissions.

### Example Create Request

```json
{
  "customer_type": "INDIVIDUAL",
  "display_name": "Sarah Connor",
  "person_name": {
    "given_name": "Sarah",
    "family_name": "Connor"
  },
  "preferred_language": "en",
  "contact_points": [
    {
      "contact_type": "MOBILE_PHONE",
      "value": "+201012345678",
      "is_primary": true,
      "verification_status": "UNVERIFIED"
    }
  ],
  "origin": {
    "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
    "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
    "source_system": "CRM",
    "source_record_id": "CRM-CUSTOMER-45872"
  }
}
```

### Example Response

```json
{
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "customer_type": "INDIVIDUAL",
  "display_name": "Sarah Connor",
  "status": "PENDING_VERIFICATION",
  "identity_verification_status": "UNVERIFIED",
  "contactability_status": "UNKNOWN",
  "data_quality_status": "INCOMPLETE",
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
- Required evidence.
- Consent and purpose checks.
- Human Approval or applicable automation-policy validation.
- Audit recording.
- Event publication after accepted state change.
- External Confirmation tracking where required.

### Optimistic Concurrency

Updates must use an approved concurrency mechanism such as:

```text
If-Match: <record_version>
```

A stale version must return a conflict response rather than silently overwriting newer data.

### Idempotency

Retryable create or command operations must support:

```text
Idempotency-Key
```

The same idempotency key and request intent must not create duplicate Customer records, Consent records, communications, or external updates.

### Merge Operation

A merge request must include:

- Surviving Customer.
- Customer to be merged.
- Duplicate evidence.
- Conflict summary.
- Authorized Human Decision.
- Expected related-record impact.
- Record versions.
- Audit reason.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `DUPLICATE_CANDIDATE_DETECTED`
- `IDENTITY_CONFLICT`
- `CONSENT_RESTRICTION`
- `HUMAN_APPROVAL_REQUIRED`
- `AUTOMATION_POLICY_NOT_APPLICABLE`
- `EXTERNAL_CONFIRMATION_PENDING`
- `LEGAL_HOLD`
- `RECORD_MERGED`
- `RECORD_ANONYMIZED`

### GraphQL Requirements

A GraphQL implementation should expose the same authority, security, concurrency, Consent, and audit boundaries as REST.

GraphQL resolvers must not bypass Domain Services or deterministic policy controls.

---

## 11. Database Design

### Recommended Core Tables

```text
customers
customer_contact_points
customer_addresses
customer_consents
customer_external_references
customer_identity_evidence
customer_organizational_links
customer_assignments
customer_merge_history
customer_restrictions
customer_derived_metrics
customer_data_quality_issues
customer_audit_log
```

### Customers Table

The `customers` table should include:

- `customer_id`.
- `tenant_id`.
- `customer_type`.
- `display_name`.
- Individual or organization identity fields.
- `status`.
- `identity_verification_status`.
- `contactability_status`.
- `data_quality_status`.
- Organizational context.
- Assignment context.
- `record_version`.
- Provenance.
- Lifecycle timestamps.

### Primary Key

```text
PRIMARY KEY (customer_id)
```

### Tenant Protection

Every Customer-related table must include `tenant_id`.

Foreign keys should preserve Tenant consistency using:

- Composite Tenant-aware foreign keys; or
- Equivalent service-level and database-level controls.

Database Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_customers_tenant_status
  (tenant_id, status)

idx_customers_tenant_display_name
  (tenant_id, display_name)

idx_customers_tenant_owner
  (tenant_id, assigned_owner_user_id)

idx_customer_contacts_tenant_normalized_value
  (tenant_id, contact_type, normalized_value)

idx_customer_external_refs
  (tenant_id, source_system, source_record_id)

idx_customer_consents_active
  (tenant_id, customer_id, channel, purpose, status)

idx_customer_merge_survivor
  (tenant_id, surviving_customer_id)

idx_customer_updated_at
  (tenant_id, updated_at)
```

Contact-point indexes support matching and duplicate detection.

They must not automatically impose global uniqueness.

### Unique Constraints

Recommended unique constraints include:

```text
UNIQUE (tenant_id, source_system, source_record_id)
```

for an active external reference where the external source guarantees uniqueness.

A phone number or email address must not be the sole universal unique key for a Customer.

### Merge History

`customer_merge_history` should preserve:

- `merge_id`.
- `tenant_id`.
- `surviving_customer_id`.
- `merged_customer_id`.
- Merge reason.
- Evidence.
- Authorized Human Decision.
- Record versions.
- Timestamp.
- Related Event identifiers.
- Reversal status where applicable.

### Consent History

Consent records must be historically versioned.

A later Consent record should supersede or revoke an earlier record without silently deleting the original evidence.

### Derived Metrics

Derived Customer metrics should be separated from authoritative identity fields.

Each derived metric should preserve:

- Metric type.
- Metric value.
- Model or formula version.
- Prompt version where applicable.
- Input version references.
- Evidence references.
- Confidence.
- Generated timestamp.
- Expiration timestamp.

### Audit Storage

Customer audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Audit records must preserve secure hashes rather than raw sensitive values where full value retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Region.
- Retention class.
- Time for append-only audit data.

Partitioning must not weaken Tenant isolation.

### Deletion and Anonymization

Anonymization must propagate to:

- Search indexes.
- Vector stores.
- Caches.
- Analytics projections.
- Export datasets.
- Non-authoritative replicas.
- Approved backups according to policy.

Required financial, contractual, and audit evidence must remain protected under the applicable retention rules.

---

## 12. Security

### Security Classification

The Customer Object contains sensitive personal and commercial information.

Recommended classifications include:

| Classification | Example Fields |
| :--- | :--- |
| `DIRECT_IDENTIFIER` | Name, phone, email, address |
| `SENSITIVE_IDENTITY` | Date of birth, national identifier, identity evidence |
| `COMMUNICATION_PREFERENCE` | Preferred channel and language |
| `CONSENT_EVIDENCE` | Consent status, legal basis, evidence reference |
| `COMMERCIAL_CONFIDENTIAL` | Customer relationship summary and lifetime value |
| `DERIVED_INTELLIGENCE` | Scores, predictions, preferences, Recommendations |
| `AUDIT_EVIDENCE` | Actor, previous state, new state, approval and Command references |

### Authentication

Every Customer operation requires authenticated User or service identity.

Anonymous access to Customer records is prohibited.

### Authorization

Authorization must consider:

- `tenant_id`.
- Dealership.
- Branch.
- Role.
- Assigned Customer or team.
- Related Opportunity or Deal.
- Requested field.
- Requested action.
- Data classification.
- Workflow state.
- Business purpose.
- Delegated authority.

### Example Role Boundaries

#### Sales Consultant

May access Customers:

- Assigned to the User.
- Assigned to the User’s team where permitted.
- Related to Opportunities the User is authorized to manage.

Access to legal identity, finance information, consent evidence, and unrestricted Customer history may be restricted.

#### Sales Manager

May access Customers inside approved dealership and branch scope.

Manager access does not automatically authorize:

- Consent override.
- Legal-identity override.
- Customer merge.
- Anonymization.
- Cross-Tenant access.

#### Finance Specialist

May access Customer identity required for an approved Finance Application.

Finance access must not expose unrelated Customer data.

#### Compliance or Legal Reviewer

May access restricted evidence required for an assigned review.

#### Data Steward

May review:

- Duplicate candidates.
- Data conflicts.
- Source provenance.
- Identity-quality issues.

Merge and identity corrections still require the configured authority.

#### AI Agent

May access only the minimum authorized context required for its approved task.

AI Agent access must be:

- Tenant-scoped.
- Purpose-limited.
- Time-limited where appropriate.
- Logged.
- Restricted by field classification.
- Prevented from retrieving cross-Tenant data.

### Encryption

- Data in transit must use approved transport encryption.
- Data at rest must use approved storage encryption.
- Highly sensitive identifiers should use field-level encryption, tokenization, or equivalent protection.
- Searchable sensitive fields should use approved protected-search techniques.
- Encryption keys must be managed outside application source code.

### Masking

Interfaces, Logs, analytics, and AI context must mask sensitive fields according to role and purpose.

Examples:

```text
Phone: +20******5678
Email: s*****@example.com
Tax identifier: ********789
```

### Consent Enforcement

Consent restrictions must be enforced deterministically before Customer communication.

A User interface, Prompt, Agent output, or Recommendation must not bypass:

- Revocation.
- Denial.
- Channel restrictions.
- Purpose restrictions.
- Frequency limits.
- Time restrictions.
- Legal-basis requirements.

### Tenant Isolation

Tenant isolation must apply to:

- Database queries.
- Search.
- Vector retrieval.
- Caches.
- Events.
- APIs.
- Exports.
- Analytics.
- Logs.
- Backups.
- AI context.
- Support access.

Every query and Event Consumer must validate `tenant_id`.

### Audit Requirements

Material Customer actions must record:

- `tenant_id`.
- `customer_id`.
- Actor.
- Role and permission.
- Action.
- Business purpose.
- Previous value or secure hash.
- New value or secure hash.
- Record version.
- Source.
- Authority category.
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

The platform must detect and record:

- Cross-Tenant Customer access attempts.
- Unauthorized identity changes.
- Unauthorized Consent changes.
- Bulk Customer export attempts.
- Suspicious duplicate merges.
- AI retrieval outside approved scope.
- Excessive Customer lookup.
- Restricted-field access.
- Policy-bypass attempts.
- Anonymization failures.
- Audit-log tampering attempts.

### Privacy and Data Rights

ASOS must support governed workflows for:

- Access requests.
- Correction requests.
- Consent management.
- Restriction of processing.
- Export.
- Deletion or anonymization.
- Legal holds.
- Retention expiration.

Privacy actions must not destroy legally required financial, contractual, fraud, security, or audit evidence.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)

---

## Current Status

This document is the approved Canonical Customer baseline.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
