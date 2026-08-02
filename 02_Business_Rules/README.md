# ASOS Business Rules

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Product, Operations, and Governance  
**Applies To:** Deterministic business logic, policy evaluation, eligibility, validation, prioritization constraints, approval requirements, exception handling, and deployment-specific rule configuration  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory is the authoritative location for approved ASOS Business Rules.

Business Rules translate approved governance, architecture, ownership boundaries, canonical contracts, and operational policy into explicit, testable, and auditable decision logic.

Business Rules define:

- Conditions that must be evaluated.
- Facts and evidence required for evaluation.
- Deterministic outcomes.
- Approval requirements.
- Prohibited outcomes.
- Exceptions and escalation conditions.
- Effective scope.
- Configuration boundaries.
- Versioning and change control.
- Required audit evidence.

A Business Rule must be understandable without relying on hidden model reasoning, undocumented tribal knowledge, or dealership-specific hard-coded assumptions.

---

## 2. Current Baseline

The current approved baseline establishes Business Rule governance, structure, authority, testing, and release requirements.

No individual Business Rule is currently designated as an approved Production baseline in this directory.

A future Business Rule becomes authoritative only after its:

- Purpose is defined.
- Scope is defined.
- Owner is assigned.
- Inputs and authoritative sources are defined.
- Conditions and outcomes are explicit.
- Action Class impact is classified.
- Human Approval requirements are defined.
- Exceptions are defined.
- Security and privacy impact is reviewed.
- Tests are completed.
- Governance approval is recorded.
- Version and effective status are assigned.

A file existing in this directory does not automatically make it approved or effective.

---

## 3. Authority Hierarchy

Business Rules operate under the following authority order:

```text
Applicable legal and regulatory requirements
        ↓
Binding contractual, OEM, lender, and governmental requirements
        ↓
ASOS Constitution
        ↓
Product, architecture, security, privacy, and data-governance policies
        ↓
Canonical Domain Models, Events, APIs, Schemas, and Integration Contracts
        ↓
Approved Business Rules
        ↓
Approved operational Playbooks
        ↓
Approved Prompts and AI Agent instructions
        ↓
Deployment configuration
        ↓
Individual recommendations and generated content
```

A Business Rule must not override a higher-level authority.

Where a conflict exists, the rule evaluation must:

- Apply the higher-authority requirement.
- Block unsafe or unauthorized outcomes.
- Record the conflict.
- Preserve the evaluated evidence.
- Escalate to an authorized Human role.
- Avoid inventing an undocumented exception.

---

## 4. What a Business Rule Is

A Business Rule is an approved, explicit statement that determines or constrains business behavior using defined inputs and deterministic evaluation.

A Business Rule may define:

- Eligibility.
- Required evidence.
- Required fields.
- Validation.
- Approval thresholds.
- Delegation limits.
- Consent enforcement.
- Communication restrictions.
- Data-freshness requirements.
- Workflow entry or exit conditions.
- Escalation triggers.
- Risk limits.
- Customer-protection constraints.
- Security and privacy constraints.
- Completion gates.
- Reconciliation requirements.

A Business Rule should produce the same result when evaluated against the same approved inputs, rule version, configuration version, and effective context.

---

## 5. What a Business Rule Is Not

A Business Rule is not:

- A constitutional principle.
- A replacement for the System Architecture.
- A Canonical Domain Object definition.
- A Canonical Event definition.
- An API or Integration Contract.
- A machine-readable Data Schema.
- An operational Playbook.
- A Prompt.
- An AI Agent Contract.
- A model prediction.
- A Human Decision.
- A Command.
- An External Confirmation.
- Proof that a business outcome completed.

Business Rules may reference these sources.

They must not create competing authoritative definitions.

---

## 6. Rule Classification

Each Business Rule must be classified by purpose.

Recommended classifications include:

| Classification | Purpose |
| :--- | :--- |
| Validation Rule | Verifies required structure, values, relationships, or state. |
| Eligibility Rule | Determines whether a subject qualifies for a defined process or option. |
| Approval Rule | Determines whether Human Approval is required and which authority is valid. |
| Prohibition Rule | Prevents an unlawful, unsafe, unauthorized, or disallowed outcome. |
| Calculation Rule | Produces a deterministic calculated value. |
| Routing Rule | Selects an approved workflow, queue, role, or service. |
| Prioritization Constraint | Applies deterministic boundaries to ranking or scheduling. |
| Freshness Rule | Defines maximum acceptable age or synchronization state for evidence. |
| Consent Rule | Enforces Customer Consent and communication preferences. |
| Completion Rule | Defines evidence required before a workflow or business outcome may be accepted as complete. |
| Escalation Rule | Defines when work must stop, be suspended, or be escalated. |
| Reconciliation Rule | Defines how conflicting facts or External Confirmations are resolved. |
| Retention Rule | Defines approved retention, deletion, archival, or legal-hold behavior. |

A single file may contain a related rule set only when ownership, inputs, scope, versioning, and tests remain clear.

---

## 7. Human Authority and Action Classes

Business Rules may determine whether an action is permitted, prohibited, or requires approval.

They must not silently create Human authority.

The approved Action Classes are:

```text
Action Class 0
  = Analysis only

Action Class 1
  = Internal and reversible operation

Action Class 2
  = Controlled Customer-facing or external operation

Action Class 3
  = Binding or high-impact Decision
```

Every Action Class 2 execution requires exactly one valid authority path:

- Explicit Human Approval for the exact action instance; or
- An active pre-approved automation policy covering the exact action instance.

No third path exists.

Action Class 3 requires an Authoritative Human Decision.

A Business Rule must not treat any of the following as execution authority:

- AI confidence.
- Model score.
- Opportunity stage.
- Priority.
- Task assignment.
- Draft content.
- User-interface state.
- A prior similar approval.
- Provider acknowledgement without authoritative Confirmation.

A rule may identify required authority.

It does not replace the required approval evidence or authoritative Decision.

---

## 8. Required State Separation

Business Rules must preserve the distinction between:

```text
Fact available
  ≠ Fact authoritative

Rule evaluated
  ≠ Recommendation generated

Recommendation generated
  ≠ Action authorized

Action authorized
  ≠ Command created

Command created
  ≠ Command sent

Command sent
  ≠ External system accepted

External system accepted
  ≠ Business outcome completed

External Confirmation received
  ≠ Canonical outcome accepted until validation and reconciliation
```

A rule result does not prove execution.

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

---

## 9. Required Rule Structure

Each Business Rule or rule set must contain the following sections.

### 9.1 Metadata

- Rule ID.
- Title.
- Version.
- Status.
- Rule owner.
- Governance owner.
- Applies-to scope.
- Tenant or deployment scope where applicable.
- Effective date where approved.
- Last updated date.

### 9.2 Purpose

- Business objective.
- Customer-protection objective.
- Problem addressed.
- Explicit non-goals.

### 9.3 Authority and Source

- Governing Constitution section.
- Governing legal, contractual, OEM, lender, or regulatory source where applicable.
- Governing architecture or data-governance policy.
- Related Canonical Domain Objects.
- Related Events, APIs, Schemas, and Integrations.
- Related Playbooks.

### 9.4 Scope

- Included scenarios.
- Excluded scenarios.
- Applicable roles.
- Applicable channels.
- Applicable systems.
- Applicable jurisdictions or markets where relevant.
- Deployment-specific configuration boundaries.

### 9.5 Inputs

Each input must define:

- Canonical name.
- Data type.
- Required or optional status.
- Authoritative source.
- Freshness requirement.
- Null or unknown handling.
- Security classification.
- Evidence reference requirement.

### 9.6 Conditions

Conditions must be:

- Explicit.
- Ordered where order matters.
- Deterministic.
- Testable.
- Free from hidden assumptions.
- Clear about inclusive and exclusive boundaries.
- Clear about time zones, currencies, units, and precision.

### 9.7 Outcomes

Each outcome must define:

- Outcome code.
- Meaning.
- Resulting workflow implication.
- Approval requirement.
- Action Class impact.
- Required explanation.
- Required evidence.
- Required Event where applicable.
- Prohibited downstream behavior.

### 9.8 Exceptions and Escalation

- Missing evidence.
- Stale evidence.
- Conflicting evidence.
- Unknown or null input.
- Unsupported jurisdiction.
- Rule-engine failure.
- Configuration failure.
- Security or privacy concern.
- Human override request.
- Emergency suspension.

### 9.9 Testing

- Positive cases.
- Negative cases.
- Boundary cases.
- Null and unknown cases.
- Stale-data cases.
- Conflicting-evidence cases.
- Unauthorized-use cases.
- Regression cases.
- Backward-compatibility cases.

### 9.10 Governance

- Approval record.
- Effective date.
- Change history.
- Migration requirements.
- Monitoring requirements.
- Rollback requirements.
- Supersession or retirement rules.

---

## 10. Rule Expression Requirements

Business Rules must use explicit operators and defined terms.

Preferred expression style:

```text
WHEN
  all required evidence is authoritative and current
AND
  defined conditions are satisfied
THEN
  produce an explicit outcome code
AND
  record the rule version, inputs, evidence, and explanation
ELSE
  produce a defined alternative, abstention, block, or escalation outcome
```

Rules must avoid ambiguous language such as:

- “Usually.”
- “As appropriate.”
- “Use best judgment.”
- “High value.”
- “Recent.”
- “Quickly.”
- “Normal conditions.”
- “Sufficient information.”

These phrases are acceptable only when their meaning is explicitly defined by approved configuration or authoritative policy.

---

## 11. Deterministic Enforcement

Deterministic or formally governed mechanisms must enforce controls where exact behavior is required.

This includes, where applicable:

- Authentication.
- Authorization.
- Role and permission checks.
- Tenant isolation.
- Required-field validation.
- Financial calculations.
- Approval thresholds.
- Legal and regulatory restrictions.
- Consent enforcement.
- Security policies.
- Data-retention controls.
- Command idempotency.
- External-confirmation validation.
- Prohibited-action enforcement.

AI Agents must not override these controls.

An AI-generated suggestion may inform rule design or review.

It must not silently become a Production rule.

---

## 12. AI and Derived Intelligence

A Business Rule may consume Derived Intelligence only when:

- The input is explicitly identified as Derived Intelligence.
- The model or algorithm version is recorded.
- Confidence and uncertainty are available where meaningful.
- The rule defines how low confidence is handled.
- The rule defines abstention behavior.
- Authoritative evidence requirements remain satisfied.
- Human Approval requirements remain unchanged.

A model score must not be represented as an authoritative fact.

AI output must not directly create:

- Execution authority.
- A binding Decision.
- An externally executed action.
- An authoritative external outcome.

Where deterministic enforcement is required, model output may not replace the deterministic control.

---

## 13. Data Authority and Evidence

Each rule input must identify its authoritative source.

A rule must distinguish between:

- Source Evidence.
- Observation.
- Canonical Projection.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Recommendation.
- Authoritative Human Decision.
- External Confirmation.

A Canonical Projection must not be treated as externally authoritative when another configured System of Record owns the fact.

When evidence is missing, stale, conflicting, incomplete, or unverified, the rule must produce a defined result such as:

- `INSUFFICIENT_EVIDENCE`.
- `STALE_EVIDENCE`.
- `CONFLICTING_EVIDENCE`.
- `REVIEW_REQUIRED`.
- `ACTION_BLOCKED`.
- `UNSUPPORTED_SCOPE`.

The rule must not fabricate a passing result.

---

## 14. Configuration Boundaries

Dealership-specific and deployment-specific values must remain configurable where they are not universal platform requirements.

Examples include:

- Thresholds.
- Approval limits.
- Working hours.
- Escalation timing.
- Branch scope.
- Role assignment.
- Provider selection.
- Communication channels.
- Message limits.
- Automation-policy limits.
- Retention periods where configurable.
- Jurisdiction-specific mappings.

Configuration values must be:

- Typed.
- Validated.
- Versioned.
- Effective-dated where required.
- Tenant-scoped.
- Access-controlled.
- Audited.
- Tested.

A deployment-specific value must not be promoted into a universal platform rule without governance review.

---

## 15. Rule Evaluation Record

Every material rule evaluation should record, where applicable:

- `tenant_id`.
- Rule ID.
- Rule version.
- Configuration version.
- Evaluation timestamp.
- Effective context.
- Input names and approved values.
- Source and evidence references.
- Data-freshness status.
- Derived Intelligence version and confidence where used.
- Outcome code.
- Explanation.
- Required approval.
- Action Class impact.
- User or service identity.
- Correlation and causation references.
- Related Domain Object IDs.
- Related Event IDs.
- Override or exception evidence.

Sensitive data must be minimized or redacted according to approved policy.

---

## 16. Human Override and Exception Handling

A Business Rule must state whether override is:

- Prohibited.
- Allowed only for an authorized Human role.
- Allowed only within an approved threshold.
- Allowed only with dual approval.
- Allowed only through a defined exception workflow.

An override must not be implemented as an undocumented data edit.

Where override is permitted, the system must record:

- Original rule result.
- Rule version.
- Overriding Human identity.
- Role and permission scope.
- Reason code.
- Written justification where required.
- Supporting evidence.
- Approval evidence.
- Effective scope.
- Timestamp.
- Related workflow or Command.

A Human override does not automatically prove that an external action completed.

---

## 17. Events, Commands, and Confirmations

When a rule evaluation produces or contributes to an Event, it must reference the approved Event Catalog.

The approved Event-producing boundary owns:

- Event creation.
- Canonical `event_id` assignment.
- Event version.
- Tenant context.
- Occurrence time.
- Correlation and causation.
- Authority classification.
- Evidence references.

The Event Backbone must preserve the Producer-assigned `event_id` unchanged.

Event delivery is at-least-once.

Consumers must prevent duplicate business effects using `event_id`.

Retryable Commands must use stable approved idempotency.

A rule outcome may authorize Command preparation only when all required authority controls are satisfied.

The final business outcome must not be accepted until the required authoritative Confirmation is validated and reconciled.

---

## 18. Security, Privacy, and Customer Protection

Every Business Rule must identify:

- Sensitive data involved.
- Minimum required access.
- Tenant-isolation requirements.
- Consent requirements.
- Purpose limitation.
- Retention requirements.
- Redaction requirements.
- Audit requirements.
- Customer-impact risks.
- Legal or compliance review requirements.

A Business Rule must not:

- Mislead a Customer.
- Conceal material information.
- Fabricate availability, pricing, approval, urgency, or eligibility.
- Represent a draft as an approved offer.
- Represent a Recommendation as a confirmed Decision.
- Apply prohibited discrimination.
- Bypass Consent.
- Bypass approval.
- Circumvent security controls.
- Share secrets or unrestricted Production data.
- Optimize revenue through unlawful or harmful behavior.

Legal, regulatory, safety, privacy, contractual, fairness, and Customer-protection requirements take priority over revenue.

---

## 19. Testing Requirements

Before approval, each rule must be tested for:

- Expected passing cases.
- Expected failing cases.
- Exact boundary values.
- Inclusive and exclusive comparisons.
- Null and unknown inputs.
- Missing evidence.
- Stale evidence.
- Conflicting evidence.
- Invalid configuration.
- Unauthorized actor.
- Tenant-boundary violations.
- Currency, unit, precision, and time-zone handling.
- Duplicate evaluation.
- Rule-engine timeout or failure.
- Override attempts.
- Backward compatibility.
- Migration behavior.
- Rollback behavior.

Tests must identify the rule and configuration versions being evaluated.

Production monitoring must detect unexpected rule-result distributions, failure rates, stale configuration, and policy violations.

---

## 20. Versioning and Status

Business Rules use semantic versioning:

- Major version for incompatible authority, condition, outcome, or scope changes.
- Minor version for backward-compatible additions or substantial clarifications.
- Patch version for non-semantic corrections.

Approved status values include:

- `Draft`.
- `Under Review`.
- `Approved Baseline`.
- `Pilot Approved`.
- `Production Approved`.
- `Suspended`.
- `Superseded`.
- `Retired`.

A rule status must not imply broader deployment approval than was actually granted.

A repository commit does not automatically make a Business Rule effective in Production.

Production effectiveness also requires applicable:

- Formal approval.
- Effective-date control.
- Configuration release.
- Rule-engine deployment.
- Testing.
- Monitoring.
- Migration.
- Rollback readiness.

---

## 21. Naming and File Organization

Recommended file naming:

```text
<Domain>_<Rule-Outcome>_Rules.md
```

Examples:

```text
Lead_Qualification_Rules.md
Customer_Communication_Consent_Rules.md
Appointment_Eligibility_Rules.md
Quotation_Approval_Rules.md
TradeIn_Appraisal_Approval_Rules.md
Finance_Offer_Selection_Rules.md
Deal_Completion_Gate_Rules.md
Inventory_Reservation_Rules.md
```

Rule IDs should remain stable across compatible revisions.

Recommended rule ID pattern:

```text
BR-<DOMAIN>-<NUMBER>
```

Examples:

```text
BR-LEAD-001
BR-CONSENT-001
BR-DEAL-003
```

Names and IDs should describe stable business meaning, not a vendor, temporary team, or dealership-specific label.

---

## 22. Content Boundaries

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and reusable templates | [`../06_Assets`](../06_Assets/) |
| Canonical Domain, Event, API, Schema, Agent, and Integration contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

Business Rules may reference these locations.

They must not duplicate or redefine their authoritative content inconsistently.

---

## 23. Related Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS Playbooks](../01_Playbooks/README.md)
- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md)

---

## 24. Current Status

**Phase:** Approved Business Rule governance baseline with controlled rule development pending  
**Version:** `1.0.0`  
**Status:** Approved Baseline  

Established by this baseline:

- Business Rule authority and boundaries.
- Required rule structure.
- Deterministic evaluation expectations.
- Human authority requirements.
- AI and Derived Intelligence boundaries.
- Evidence and data-authority requirements.
- Configuration controls.
- Testing requirements.
- Versioning and release governance.

No individual Business Rule is currently designated as an approved Production baseline in this directory.

Future Business Rules must preserve:

- Constitutional authority.
- Human accountability.
- Customer protection.
- Deterministic enforcement.
- Field-level data authority.
- Tenant isolation.
- Evidence and provenance.
- Controlled automation.
- Security and privacy.
- Auditability.
- Compatibility.
- Reconciliation.
- Controlled change.
