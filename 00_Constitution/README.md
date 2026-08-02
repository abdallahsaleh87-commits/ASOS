# ASOS Constitution

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Governance  
**Applies To:** All ASOS services, AI Agents, workflows, integrations, data products, interfaces, analytics, configurations, and deployments  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory contains the highest internal governance authority for the ASOS AI Sales Operating System.

The Constitution defines the mandatory boundaries governing:

- Human authority.
- AI behavior.
- Decision support.
- Workflow automation.
- Customer protection.
- Commercial value.
- Data authority.
- Security and privacy.
- Explainability.
- Auditability.
- Controlled improvement.
- Constitutional change.

All lower-level ASOS documents, contracts, configurations, implementations, and deployments must comply with the Constitution.

---

## 2. Authoritative Document

| Document | Current baseline | Responsibility |
| :--- | :---: | :--- |
| [ASOS Constitution](./Constitution.md) | `v1.1.0` Approved Baseline | Highest internal governance principles, authority boundaries, AI and automation limits, Customer protection, security, auditability, and constitutional change control. |

The Constitution is authoritative for its approved scope.

This README is an index and interpretation guide.

It must not replace or override the complete Constitution.

---

## 3. Authority and Precedence

The following order of authority applies:

```text
Applicable legal and regulatory requirements
        ↓
Binding contractual, OEM, lender, and governmental requirements
        ↓
ASOS Constitution
        ↓
Approved architecture, security, privacy, and data-governance policies
        ↓
Canonical Domain Models, Events, APIs, and Schemas
        ↓
Approved Business Rules
        ↓
Approved operational Playbooks
        ↓
Approved Prompts and AI Agent instructions
        ↓
Dealership and Pilot configuration
        ↓
Individual recommendations and generated content
```

A lower-level document, configuration, Prompt, model output, or Human instruction must not override a higher-level requirement.

Where requirements conflict, ASOS must:

- Apply the higher-authority requirement.
- Apply the stricter lawful protection where appropriate.
- Record the conflict.
- Prevent unsafe execution.
- Escalate the issue to an authorized Human role.

---

## 4. Platform Role

ASOS is a decision-support and workflow-orchestration layer operating above approved dealership systems.

ASOS may:

- Observe and normalize authorized data.
- Detect risks, opportunities, conflicts, and missing evidence.
- Produce Derived Intelligence.
- Prioritize work.
- Recommend actions.
- Prepare drafts.
- Coordinate Human Review.
- Issue authorized Commands through controlled orchestration.
- Track External Confirmations.
- Measure outcomes.
- Support governed organizational learning.

ASOS does not automatically replace:

- Customer Relationship Management systems.
- Dealer Management Systems.
- Inventory Management Systems.
- Finance and Insurance platforms.
- Lender systems.
- Payment systems.
- Communication providers.
- Contract and document platforms.
- Government or regulatory systems.
- Authorized Human judgment.

The AI Brain must not directly perform external actions.

---

## 5. Evidence and Decision Integrity

ASOS must distinguish between:

```text
Authoritative fact
Observation
Derived Intelligence
Assumption or hypothesis
Recommendation
Authoritative Human Decision
Command
External Confirmation
Reconciled business outcome
```

These states must not be silently merged.

Confidence must not replace evidence.

A high-confidence model output is not automatically:

- An authoritative business fact.
- A Human Decision.
- Execution authority.
- A completed external outcome.

Where evidence is missing, stale, conflicting, incomplete, or unverified, ASOS must:

- State the uncertainty.
- Identify missing or conflicting evidence.
- Reduce confidence appropriately.
- Avoid unsupported certainty.
- Abstain when safe guidance cannot be produced.
- Escalate when risk exceeds permitted limits.

---

## 6. Human Authority and Automation

Authority belongs to the authorized Human role defined for the decision type and deployment configuration.

Authority must be enforced through approved:

- Role.
- Permission.
- Scope.
- Threshold.
- Delegation.
- Approval.
- Automation-policy controls.

Authority must not be inferred solely from job title, model confidence, Opportunity stage, priority, Task assignment, or User-interface state.

### Action Classes

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

Action Class 3 requires an Authoritative Human Decision and, where applicable, authoritative external confirmation.

Approved orchestration services may execute only after all required controls and approvals have been satisfied.

Every deployment must support:

- Suspension.
- Revocation.
- Escalation.
- Emergency stop.
- Audit.
- Post-action review.

---

## 7. Deterministic Controls

AI reasoning may assist where interpretation, summarization, prioritization, forecasting, matching, or contextual judgment is beneficial.

AI reasoning must not replace deterministic or formally governed controls for:

- Authentication.
- Authorization.
- Role-based access.
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

---

## 8. Data, Security, Privacy, and Tenant Isolation

ASOS must preserve field-level data authority.

Access to information does not automatically grant authority to change it.

Permission to recommend an action does not automatically grant permission to approve or execute it.

ASOS must:

- Preserve source provenance.
- Preserve synchronization status.
- Preserve authoritative timestamps.
- Prevent silent overwrites.
- Identify data conflicts.
- Apply freshness requirements.
- Enforce least privilege.
- Enforce dealership and Tenant isolation.
- Protect Customer and commercially sensitive information.
- Respect Consent and communication preferences.
- Apply purpose limitation.
- Apply retention and deletion requirements.
- Protect credentials, tokens, and cryptographic material.
- Redact sensitive data from Logs and AI context where required.

A Canonical Projection, Recommendation, internal workflow state, or outbound Command must not be treated as an externally confirmed fact without authoritative evidence.

---

## 9. Customer Protection and Commercial Value

Legal, regulatory, safety, privacy, contractual, fairness, and Customer-protection requirements take priority over revenue.

ASOS must not:

- Mislead or manipulate a Customer.
- Conceal material information.
- Fabricate availability, pricing, approval, urgency, or eligibility.
- Represent a draft as an approved offer.
- Represent a Recommendation as a confirmed Decision.
- Apply prohibited discrimination.
- Encourage actions outside approved authority.
- Optimize revenue through unlawful or harmful behavior.

Where multiple compliant options remain, ASOS should prioritize sustainable value for both the Customer and the dealership.

---

## 10. Auditability and Controlled Improvement

Material ASOS activity must remain auditable.

Audit evidence should preserve, where applicable:

- Tenant and dealership identity.
- User or service identity.
- Role and permission scope.
- Source data and versions.
- Domain Object version.
- Business Rule version.
- Prompt version.
- Model or algorithm version.
- Recommendation.
- Confidence and uncertainty.
- Explanation and evidence.
- Human approval, rejection, modification, or override.
- Command.
- External response or Confirmation.
- Timestamps.
- Errors, incidents, and retries.
- Final outcome.

Human feedback and operational outcomes may inform future improvement.

They must not automatically modify:

- The Constitution.
- Business Rules.
- Playbooks.
- Prompts.
- Models.
- Approval policies.
- Production behavior.

Material changes require review, testing, evaluation, approval, versioning, controlled release, rollback capability, and post-release monitoring.

---

## 11. Constitutional Change Control

ASOS Governance owns the Constitution.

A constitutional amendment must identify:

- The proposed change.
- The problem being addressed.
- The reason for the change.
- Affected services and documents.
- Domain Model impact.
- Event and API impact.
- AI Agent and Prompt impact.
- Security and privacy impact.
- Legal or compliance impact.
- Migration requirements.
- Testing requirements.
- Approval evidence.
- Effective date.

Versioning follows:

- Patch for a clarification that does not change authority or behavior.
- Minor for a new principle or backward-compatible governance change.
- Major for a change to authority, mandatory controls, or fundamental platform behavior.

A repository commit does not automatically make a constitutional change effective.

Effectiveness requires:

- Required review.
- Formal approval.
- Version assignment.
- Effective-date declaration.
- Dependent-document alignment.
- Required migration or rollout.

No Prompt, AI Agent, Business Rule, Playbook, configuration, integration, or individual instruction may override the Constitution.

---

## 12. Content Boundaries

This directory should contain:

- The approved ASOS Constitution.
- Constitution-specific amendment records.
- Constitution-specific governance guidance.
- Approved supersession or retirement references.

The following content belongs elsewhere:

| Content type | Authoritative location |
| :--- | :--- |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and templates | [`../06_Assets`](../06_Assets/) |
| Canonical implementation contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

Lower-level documents may reference the Constitution.

They must not duplicate it in a way that creates competing authority.

---

## 13. Related Documents

- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md)

---

## 14. Current Status

**Phase:** Approved constitutional foundation with active implementation and controlled expansion  
**Version:** `1.1.0`  
**Status:** Approved Baseline  

The ASOS Constitution `v1.1.0` is the current approved internal governance baseline.

Implementation, migration, integration, testing, and deployment remain controlled activities.

Future work must preserve:

- Applicable legal and contractual authority.
- Human accountability.
- Customer dignity and protection.
- Evidence before assertion.
- Deterministic enforcement.
- Controlled automation.
- Field-level data authority.
- Tenant isolation.
- Security and privacy.
- Explainability.
- Auditability.
- Controlled change.
