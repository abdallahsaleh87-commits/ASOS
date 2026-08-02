# ASOS Playbooks

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Operations and Governance  
**Applies To:** Operational workflows, Human procedures, controlled execution, exception handling, escalation, evidence capture, and deployment-specific operating guidance  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory is the authoritative location for approved ASOS operational Playbooks.

A Playbook translates approved governance, architecture, canonical contracts, and Business Rules into a repeatable Human and system operating procedure.

Playbooks explain:

- When an operational procedure begins.
- Which roles participate.
- Which evidence is required.
- Which systems and Domain Objects are involved.
- Which Decisions require Human authority.
- Which Commands may be executed.
- Which Confirmations prove completion.
- How exceptions are handled.
- How work is escalated, suspended, revoked, retried, reconciled, or closed.
- Which records must be retained for audit.

A Playbook must be usable by an authorized operator without inventing missing authority, policy, or execution rules.

---

## 2. Current Baseline

The current approved baseline establishes Playbook governance, structure, and content requirements.

No operational Playbook is currently designated as an approved Production baseline in this directory.

Future Playbooks become authoritative only after their:

- Purpose is defined.
- Scope is defined.
- Owner is assigned.
- Preconditions are defined.
- Roles and permissions are defined.
- Action Classes are classified.
- Human Approval requirements are defined.
- Business Rules are linked.
- Canonical Objects and Events are linked.
- Commands and Confirmations are defined.
- Security and privacy impact is reviewed.
- Exception and rollback behavior is defined.
- Testing is completed.
- Governance approval is recorded.
- Version and effective status are assigned.

A file existing in this directory does not automatically make it approved or effective.

---

## 3. Authority Hierarchy

Playbooks operate under the following authority order:

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

A Playbook must not override a higher-level authority.

Where a conflict exists, the Playbook must:

- Stop unsafe execution.
- Apply the higher-authority requirement.
- Preserve evidence of the conflict.
- Escalate to an authorized Human role.
- Avoid creating an undocumented exception.

---

## 4. What a Playbook Is

A Playbook is an approved operational procedure describing how authorized Humans and ASOS services coordinate to achieve a defined outcome.

A Playbook may define:

- Trigger conditions.
- Entry criteria.
- Required evidence.
- Assigned roles.
- Review steps.
- Decision points.
- Approval gates.
- Command preparation.
- Controlled execution.
- External Confirmation handling.
- Reconciliation.
- Exception handling.
- Escalation.
- Closure criteria.
- Required audit evidence.

A Playbook should be specific enough to operate consistently while keeping dealership-specific values configurable.

---

## 5. What a Playbook Is Not

A Playbook is not:

- A constitutional rule.
- A replacement for the System Architecture.
- A Canonical Domain Object definition.
- A Canonical Event definition.
- An API or Integration Contract.
- A machine-readable Data Schema.
- A Business Rule engine specification.
- A Prompt.
- An AI Agent Contract.
- A model output.
- A substitute for Human authority.
- Proof that an external action completed.

A Playbook may reference these sources.

It must not create competing authoritative definitions.

---

## 6. Human Authority and Action Classes

Playbooks must classify every material activity using the approved Action Classes.

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

Where applicable, authoritative External Confirmation is also required before the final business outcome is accepted.

A Playbook must not treat any of the following as execution authority:

- AI confidence.
- Opportunity stage.
- Priority.
- Task assignment.
- Draft content.
- User-interface state.
- A prior similar approval.
- Provider acknowledgement without authoritative Confirmation.

---

## 7. Required State Separation

Every Playbook must preserve the distinction between:

```text
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

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

A Playbook must define which evidence moves the workflow from one state to the next.

---

## 8. Required Playbook Structure

Each Playbook must contain the following sections.

### 8.1 Metadata

- Title.
- Version.
- Status.
- Document owner.
- Operational owner.
- Applies-to scope.
- Effective date where approved.
- Last updated date.
- Related deployment or Tenant scope where applicable.

### 8.2 Purpose and Outcome

- Operational objective.
- Expected Customer and business outcome.
- Explicit non-goals.
- Completion definition.

### 8.3 Scope

- Included scenarios.
- Excluded scenarios.
- Applicable channels.
- Applicable roles.
- Applicable systems.
- Deployment-specific constraints.

### 8.4 Trigger and Preconditions

- Triggering Event or Human request.
- Required Domain Object state.
- Required evidence.
- Data freshness requirements.
- Consent requirements.
- Permission requirements.
- Blocking conditions.

### 8.5 Roles and Authority

- Responsible role.
- Reviewing role.
- Approving role.
- Executing service.
- Escalation role.
- Separation-of-duties requirements.
- Delegation limits.

### 8.6 Procedure

Each step must identify:

- Step number.
- Responsible actor.
- Required input.
- Action Class.
- Decision or action.
- Required approval.
- Output.
- Evidence recorded.
- Next state.

### 8.7 Commands and Confirmations

- Command owner.
- Authorization evidence.
- Idempotency requirement.
- Target integration.
- Expected acknowledgement.
- Authoritative Confirmation.
- Timeout behavior.
- Retry behavior.
- Reconciliation behavior.

### 8.8 Exceptions and Escalation

- Missing evidence.
- Stale data.
- Conflicting authority.
- Integration failure.
- Duplicate request.
- Policy violation.
- Customer objection.
- Security or privacy concern.
- Emergency stop.
- Manual recovery.

### 8.9 Closure

- Completion criteria.
- Cancellation criteria.
- Expiration criteria.
- Required final evidence.
- Required notifications.
- Required reconciliation.
- Required audit record.

### 8.10 Governance

- Related Constitution sections.
- Related Business Rules.
- Related Domain Objects.
- Related Events.
- Related APIs and Integrations.
- Security review.
- Privacy review.
- Testing evidence.
- Approval record.
- Change history.

---

## 9. Playbook Step Format

The recommended step format is:

| Field | Requirement |
| :--- | :--- |
| Step | Stable ordered identifier. |
| Actor | Authorized Human role, ASOS service, or external authority. |
| Input | Required facts, evidence, state, and versions. |
| Action Class | `0`, `1`, `2`, or `3`. |
| Procedure | Exact operational activity. |
| Authority | Human Approval, automation policy, or authoritative external Decision. |
| Output | Resulting state, draft, Decision, Command, or evidence. |
| Confirmation | Evidence required to accept completion. |
| Failure path | Retry, stop, escalate, reverse, or reconcile behavior. |
| Audit evidence | Required record of what occurred. |

Ambiguous phrases such as “process normally,” “use judgment,” or “complete the action” must be replaced with defined steps, authority, evidence, and exception behavior.

---

## 10. AI Use in Playbooks

AI Agents may assist with:

- Classification.
- Extraction.
- Summarization.
- Matching.
- Forecasting.
- Prioritization.
- Risk detection.
- Opportunity detection.
- Drafting.
- Recommendation.
- Explanation.
- Context retrieval.
- Data-quality detection.

AI Agents must not directly execute external actions.

A Playbook using AI must define:

- Agent purpose.
- Permitted inputs.
- Permitted outputs.
- Required evidence.
- Confidence handling.
- Abstention conditions.
- Human Review requirements.
- Action Class.
- Permitted tools.
- Prohibited actions.
- Failure behavior.
- Escalation behavior.
- Evaluation requirements.

AI output must remain distinguishable from authoritative facts and Human Decisions.

---

## 11. Business Rules and Configuration

Playbooks may reference approved Business Rules.

They must not embed a competing rule definition when the rule belongs in `02_Business_Rules`.

Deployment-specific values must remain configurable, including:

- Thresholds.
- Approval limits.
- Working hours.
- Escalation timing.
- Communication channels.
- Message templates.
- Provider choices.
- Branch scope.
- Role assignments.
- Automation-policy limits.
- Retention periods where configurable.

A deployment-specific value must not be promoted into a universal platform rule without governance review.

---

## 12. Event, Command, and Integration Requirements

When a Playbook uses Events, it must reference the approved Event Catalog.

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

A Playbook must define how duplicate requests, retries, late Confirmations, and reconciliation conflicts are handled.

---

## 13. Security, Privacy, and Customer Protection

Every Playbook must identify:

- Sensitive data involved.
- Minimum required access.
- Tenant-isolation requirements.
- Consent requirements.
- Communication preferences.
- Purpose limitation.
- Retention requirements.
- Redaction requirements.
- Audit requirements.
- Customer-impact risks.
- Legal or compliance review requirements.

A Playbook must not instruct Users or systems to:

- Mislead a Customer.
- Conceal material information.
- Fabricate availability, pricing, approval, urgency, or eligibility.
- Represent a draft as an approved offer.
- Represent a Recommendation as a confirmed Decision.
- Bypass Consent.
- Bypass approval.
- Circumvent security controls.
- Share secrets or unrestricted Production data.
- Optimize revenue through unlawful or harmful behavior.

---

## 14. Testing and Release

Before a Playbook becomes effective, it must be tested for:

- Normal flow.
- Missing evidence.
- Stale evidence.
- Conflicting evidence.
- Unauthorized actor.
- Approval rejection.
- Approval expiration.
- Automation-policy revocation.
- Duplicate request.
- Integration timeout.
- Retry.
- Partial failure.
- Late Confirmation.
- Reconciliation conflict.
- Customer objection.
- Security incident.
- Emergency stop.
- Rollback or manual recovery.

Approval of the document does not automatically activate the Playbook in Production.

Activation also requires applicable:

- Configuration.
- Permissions.
- Integration readiness.
- Training.
- Testing.
- Monitoring.
- Rollback readiness.
- Effective-date control.

---

## 15. Versioning and Status

Playbooks use semantic versioning:

- Major version for incompatible procedure, authority, or outcome changes.
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

A Playbook status must not imply broader deployment approval than was actually granted.

A repository commit does not automatically make a Playbook effective in Production.

---

## 16. Naming and File Organization

Recommended file naming:

```text
<Domain>_<Operational-Outcome>_Playbook.md
```

Examples:

```text
Lead_Qualification_Review_Playbook.md
Appointment_Confirmation_Playbook.md
TradeIn_Appraisal_Review_Playbook.md
Finance_Offer_Selection_Playbook.md
Deal_Completion_Readiness_Playbook.md
Customer_FollowUp_Playbook.md
```

Names should describe the operational outcome, not a temporary team name, vendor, or dealership-specific label.

Deployment-specific supplements should be clearly marked and must not redefine the platform baseline.

---

## 17. Content Boundaries

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and reusable templates | [`../06_Assets`](../06_Assets/) |
| Canonical Domain, Event, API, Schema, Agent, and Integration contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

Playbooks may reference these locations.

They must not duplicate their complete authoritative content or create conflicting definitions.

---

## 18. Related Governing Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../05_Documentation/MVP_Pilot_Framework.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md)

---

## 19. Current Status

**Phase:** Playbook governance baseline established; operational Playbooks pending controlled creation and approval  
**Version:** `1.0.0`  
**Status:** Approved Baseline  

The directory currently establishes how operational Playbooks must be structured, governed, tested, approved, and activated.

Operational Playbooks remain to be created through controlled, Domain-specific work.

Future Playbooks must preserve:

- Constitutional authority.
- Human accountability.
- Action Class boundaries.
- Evidence before assertion.
- Recommendation, Decision, Command, Confirmation, and outcome separation.
- Field-level data authority.
- Tenant isolation.
- Security and privacy.
- Customer protection.
- Idempotency.
- Auditability.
- Reconciliation.
- Controlled change.
