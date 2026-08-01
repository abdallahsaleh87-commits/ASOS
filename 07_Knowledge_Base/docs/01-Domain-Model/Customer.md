# Customer

## 1. Object Purpose

### Business Purpose

The Customer object serves as the persistent master identity record for an individual or business entity engaging with the dealership. It represents the central focus of all revenue operations, spanning multiple vehicle purchases, service interactions, and long-term retention efforts. It ensures a unified customer journey across all dealership departments.

### System Purpose

The Customer object is the root node of the ASOS data model. It acts as the anchor for referential integrity across all transactional entities (Leads, Opportunities, Deals, Appointments). It establishes the multi-tenant boundary for data access and provides the foundational context payload for all AI Agent reasoning and Memory retrieval.

## 2. Canonical Schema

### Identifiers

- **Primary Key:** `customer_id` (UUIDv4)
- **Tenant ID:** `dealership_id` (UUIDv4)
- **Foreign Keys:** `owner_id` (UUIDv4 - mapping to Layer 6 User), `household_id` (UUIDv4 - optional)

### Data Payload

- **Required Fields:** `first_name`, `last_name`, `primary_phone`, `customer_type`, `consent_status`
- **Optional Fields:** `email`, `secondary_phone`, `company_name`, `tax_id`, `address_line_1`, `city`, `postal_code`, `preferred_contact_method`
- **Computed Fields:** `lifetime_value_amount`, `days_since_last_interaction`, `active_pipeline_count`

### Governance & Lifecycle

- **Metadata:** `lead_source`, `external_crm_id`, `external_dms_id`
- **Audit Fields:** `created_by`, `updated_by`, `last_ai_interaction_agent`
- **Version:** `record_version` (Integer, optimistic locking)
- **Soft Delete:** `is_deleted` (Boolean), `deleted_at` (Timestamp)
- **Ownership:** `owner_id` (UUIDv4)
- **Timestamps:** `created_at`, `updated_at`, `last_interaction_at`

## 3. Field Definitions

| Name | Type | Description | Required | Default | Validation Rule | Example | Confidence Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| customer_id | UUID | Unique identifier for the customer record. | Yes | Auto-generated | Must use a valid UUIDv4 format | 123e4567-e89b-12d3-a456-426614174000 | N/A |
| dealership_id | UUID | Identifies the dealership tenant that owns the record. | Yes | From active context | Must match the authenticated tenant | 987f6543-a21b-43d2-b123-426614174000 | N/A |
| first_name | String | Customer's given name. | Yes | N/A | Maximum 50 characters | Sarah | At least 0.85 if AI-extracted |
| last_name | String | Customer's family name. | Yes | N/A | Maximum 50 characters | Connor | At least 0.85 if AI-extracted |
| primary_phone | String | Customer's main contact number. | Yes | N/A | Must follow E.164 international format | +201012345678 | At least 0.95 |
| customer_type | Enum | Classifies the customer as retail or corporate. | Yes | RETAIL | Must match CustomerType ENUM | RETAIL | At least 0.90 |
| consent_status | Enum | Indicates whether communication consent was granted. | Yes | PENDING | Must match ConsentStatus ENUM | OPT_IN | At least 0.99 |
| email | String | Customer's email address. | No | Null | Must be a valid email format | sarah.c@email.com | At least 0.90 |
| company_name | String | Registered business or company name. | No | Null | Maximum 100 characters; required for corporate customers | Nile Logistics | At least 0.85 |
| tax_id | String | Corporate tax registration identifier. | No | Null | Must follow the applicable regional tax format | 123-456-789 | Human input only |
| lifetime_value_amount | Decimal | Total realized customer value calculated by the system. | No | 0.00 | Must be zero or greater with two decimal places | 4500.00 | System-computed |

## 4. Enumerations

### CustomerType

- **RETAIL:** Individual B2C buyer.
- **CORPORATE:** Business/Fleet B2B buyer.

### CustomerStatus

- **ACTIVE:** Currently engaged in an active Opportunity or Deal.
- **NURTURE:** No active deal, but enrolled in long-term AI follow-up.
- **INACTIVE:** No engagement within 12 months.
- **CHURNED:** Explicitly stated they purchased elsewhere or requested closure.

### ConsentStatus

- **PENDING:** Awaiting formal opt-in (limits outbound AI Agent outreach).
- **OPT_IN:** Fully consented to marketing and transactional AI outreach.
- **OPT_OUT:** Do not contact (hard block for AI Agents).

### PreferredContactMethod

- WHATSAPP
- PHONE
- EMAIL
- SMS

## 5. Validation Rules

### Business Rules

- If `customer_type = CORPORATE`, then `company_name` is mandatory.
- A Customer cannot be moved to `ACTIVE` status without a linked Opportunity or Deal.

### Technical Rules

- `primary_phone` must strictly adhere to E.164 format prior to database commit.
- `record_version` must be incremented on every UPDATE payload to prevent lost updates (Optimistic Concurrency Control).

### Data Constraints

- `primary_phone + dealership_id` must be unique to prevent duplicate records within the same rooftop.

### Referential Integrity

- Cannot hard-delete a Customer if a linked Deal exists in a non-terminal state.
- Must use `is_deleted = true` (Soft Delete) to preserve historical Deal ledgers.

### Human Approval Requirements

- AI Agents cannot update `tax_id` or `consent_status` without Human Approval (Layer 6).

  ## 6. State Machine

### Allowed States

- NEW
- ACTIVE
- NURTURE
- INACTIVE
- CHURNED

### Allowed Transitions

- NEW → ACTIVE
- ACTIVE → NURTURE
- ACTIVE → CHURNED
- NURTURE → ACTIVE
- NURTURE → INACTIVE
- INACTIVE → ACTIVE

### Forbidden Transitions

- CHURNED → NEW  
  Must transition directly back to ACTIVE if the customer returns.

### Entry Conditions

- To enter ACTIVE: The customer must have more than 0 records in the Opportunities table with a status other than CLOSED.

### Exit Conditions

- To exit ACTIVE: All linked Opportunities and Deals must reach a terminal state, either Won Deal or Lost Deal.

### Terminal States

- None. A Customer record is perpetual unless legally required to be purged under GDPR or applicable regional deletion mandates.

## 7. Relationships

- **Depends On:** The dealership tenant identified by `dealership_id`.
- **Consumes:** Lead data when a Lead is converted into a Customer.
- **Produces:** Aggregated Customer Memory and behavioral context.
- **Creates:** None; Customer is a root entity.
- **Triggers:** `CustomerCreated` and `CustomerUpdated` events.
- **Owned By:** A Sales Consultant or BDC Agent.
- **Referenced By:** Opportunity, Deal, Quotation, Appointment, Task, and MemoryLog.

## 8. Domain Events

### Emitted Events

- **CustomerCreated**  
  Payload: `customer_id`, `dealership_id`, `customer_type`

- **CustomerContactUpdated**  
  Payload: `customer_id`, `new_phone`, `new_email`

- **CustomerConsentRevoked**  
  Payload: `customer_id`, `timestamp`  
  Critical priority for AI Agent suspension.

- **CustomerMerged**  
  Payload: `surviving_customer_id`, `merged_customer_id`

### Consumed Events

- **LeadQualified**  
  Triggers creation or update of the Customer.

- **DealWon**  
  Triggers recalculation of `lifetime_value_amount` and Customer status updates.

## 9. AI Considerations

### Fields Used for Vector Embeddings — Customer Memory

- Behavioral notes
- Objections
- Vehicle preferences
- Extracted conversational summaries

### Fields Excluded from Embeddings — PII Protection

- `first_name`
- `last_name`
- `primary_phone`
- `email`
- `tax_id`
- `address_line_1`

> The AI Brain accesses PII strictly through structured JSON injection at prompt time, never through semantic search retrieval.

### Metadata Filters for Context Retrieval

- `dealership_id` — mandatory for every vector query to prevent cross-tenant data leakage.
- `customer_status`

### Confidence Thresholds

- AI extraction of new contact information from raw chat requires a confidence score of at least `0.95` before executing an automated update.

### Human Approval Thresholds

- Any AI operation attempting to merge two Customer records requires strict Layer 6 Human Approval.

## 10. API Contract

### REST Resource

- **Base Path:** `/api/v1/dealerships/{dealership_id}/customers`
- **Methods:** `GET` (List/Search), `POST` (Create), `GET /{id}`, `PATCH /{id}` (Update), `DELETE /{id}` (Soft Delete)

### Suggested JSON Schema — Create Payload

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CreateCustomerRequest",
  "type": "object",
  "properties": {
    "first_name": {
      "type": "string",
      "maxLength": 50
    },
    "last_name": {
      "type": "string",
      "maxLength": 50
    },
    "primary_phone": {
      "type": "string",
      "pattern": "^\\+?[1-9]\\d{1,14}$"
    },
    "customer_type": {
      "type": "string",
      "enum": ["RETAIL", "CORPORATE"]
    },
    "consent_status": {
      "type": "string",
      "enum": ["PENDING", "OPT_IN", "OPT_OUT"]
    }
  },
  "required": [
    "first_name",
    "last_name",
    "primary_phone",
    "customer_type",
    "consent_status"
  ]
}

```
### GraphQL Type

```graphql

  type Customer {
  id: ID!
  dealershipId: ID!
  firstName: String!
  lastName: String!
  primaryPhone: String!
  email: String
  customerType: CustomerType!
  status: CustomerStatus!
  lifetimeValue: Float!
  activeDeals: [Deal!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}
```

## 11. Database Design

### Recommended SQL Tables

- **Table Name:** `customers`

### Indexes

- `idx_customers_dealership_phone (dealership_id, primary_phone)`  
  High read volume for inbound lead matching.

- `idx_customers_dealership_email (dealership_id, email)`

- `idx_customers_status (dealership_id, status)`  
  Used for batch agent operations.

### Unique Constraints

- `UQ_dealership_phone (dealership_id, primary_phone) WHERE is_deleted = false`

### Foreign Keys

- `dealership_id` → `dealerships(id)`
- `owner_id` → `users(id)`

### Partition Keys

- Partition by `dealership_id` using Hash or List partitioning to guarantee tenant isolation at the disk and query-execution level.

## 12. Security

### RBAC — Role-Based Access Control

- **Sales Consultant:** Read/Write access only if `owner_id` matches their User ID, or if the Customer is unassigned within their dealership.
- **Sales Manager:** Read/Write access for all Customers where `dealership_id` matches their assigned tenant.
- **AI Agents:** Service Account access constrained implicitly by the Context Layer's injection of the `dealership_id` JWT scope.

### PII Classification

- **Level:** `CRITICAL_PII`
- **Fields:** `first_name`, `last_name`, `primary_phone`, `email`, `tax_id`

### Encryption Requirements

- **In Transit:** TLS 1.3 minimum.
- **At Rest:** AES-256 encryption for the entire database volume.
- **Column-Level:** `tax_id` requires explicit column-level encryption — CLE — or tokenization.

### Audit Requirements

- Every `PATCH` operation must generate an immutable log entry in a `customer_audit_log` table containing:
  - `timestamp`
  - `actor_id` — Human or AI Agent
  - `previous_state`
  - `new_state`
  - `reason`
