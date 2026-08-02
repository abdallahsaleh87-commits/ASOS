# ASOS Prompts

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS AI Engineering, Product, Security, and Governance  
**Applies To:** System Prompts, Developer Prompts, Task Prompts, Prompt templates, evaluation Prompts, extraction Prompts, classification Prompts, drafting Prompts, Agent instructions, tool-use instructions, structured-output instructions, and deployment-specific Prompt configuration  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory is the authoritative location for governed ASOS Prompt assets.

Prompts translate approved governance, architecture, canonical contracts, Business Rules, and operational procedures into bounded instructions for AI-assisted behavior.

Prompt governance exists to ensure that AI behavior remains:

- Lawful.
- Customer-protective.
- Evidence-based.
- Human-governed where required.
- Consistent with approved authority boundaries.
- Secure.
- Privacy-preserving.
- Tenant-isolated.
- Testable.
- Versioned.
- Auditable.
- Reversible where applicable.
- Compatible with approved ASOS contracts.

A Prompt must not be used as a substitute for deterministic controls, canonical definitions, Human authority, or approved operational policy.

---

## 2. Current Baseline

The current approved baseline establishes Prompt governance, structure, security, evaluation, release, and change-control requirements.

No individual Prompt is designated by this README as an approved Production Prompt.

An individual Prompt becomes eligible for approved use only after its:

- Purpose is defined.
- Scope is defined.
- Owner is assigned.
- Inputs and outputs are defined.
- Authority boundary is defined.
- Action Class impact is classified.
- Tool permissions are defined.
- Prohibited behavior is defined.
- Data and privacy impact is reviewed.
- Prompt-injection risks are evaluated.
- Structured-output requirements are defined where applicable.
- Tests and evaluations are completed.
- Human Review requirements are defined.
- Failure and abstention behavior are defined.
- Governance approval is recorded.
- Version and effective status are assigned.

A file existing in this directory does not automatically make it approved, effective, safe, or Production-ready.

---

## 3. Authority Hierarchy

Prompts operate under the following authority order:

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
Approved Agent Contracts and Prompt governance
        ↓
Deployment configuration
        ↓
Individual Task instructions and User requests
        ↓
Model-generated content
```

A Prompt must not override a higher-level authority.

A lower-priority instruction must not override a higher-priority instruction.

Where a conflict exists, the AI-assisted workflow must:

- Follow the higher-authority requirement.
- Reject or ignore the conflicting lower-priority instruction.
- Prevent unsafe or unauthorized execution.
- Preserve evidence of the conflict where material.
- Escalate when required.
- Avoid inventing an undocumented exception.

---

## 4. What a Prompt Is

A Prompt is a versioned instruction asset that defines how an AI model should perform a bounded task within approved ASOS constraints.

A Prompt may define:

- Task objective.
- Allowed context.
- Required evidence.
- Expected reasoning behavior.
- Required output structure.
- Tone and communication constraints.
- Tool-use rules.
- Abstention behavior.
- Escalation behavior.
- Security boundaries.
- Privacy boundaries.
- Evaluation criteria.
- Error handling.
- Human Review requirements.

A Prompt should be specific enough to produce consistent behavior while preserving approved configuration and deployment boundaries.

---

## 5. What a Prompt Is Not

A Prompt is not:

- A constitutional principle.
- A legal authority.
- A Business Rule.
- A System-of-Record definition.
- A Canonical Domain Object definition.
- A Canonical Event definition.
- An API Contract.
- An Integration Contract.
- A machine-readable Data Schema.
- An operational Playbook.
- An Agent permission grant.
- A Human Decision.
- Execution authority.
- A Command.
- An External Confirmation.
- Proof that a business outcome completed.
- A substitute for authentication or authorization.
- A substitute for deterministic validation.

Prompts may reference approved sources.

They must not create competing authoritative definitions.

---

## 6. Prompt Classification

Each Prompt must be classified by purpose.

Recommended classifications include:

| Classification | Purpose |
| :--- | :--- |
| System Prompt | Defines stable model behavior, boundaries, authority, and safety constraints. |
| Developer Prompt | Defines application-specific behavior and implementation constraints. |
| Task Prompt | Defines a bounded request for a specific workflow step. |
| Extraction Prompt | Extracts structured fields from approved source content. |
| Classification Prompt | Assigns an approved label or category with evidence and confidence. |
| Summarization Prompt | Produces a bounded summary without changing source meaning. |
| Recommendation Prompt | Produces a non-binding Recommendation supported by evidence. |
| Drafting Prompt | Prepares content for Human Review or policy-controlled use. |
| Explanation Prompt | Explains a result, Decision, Rule evaluation, or workflow state. |
| Evaluation Prompt | Assesses model output against approved criteria. |
| Red-Team Prompt | Tests unsafe, conflicting, deceptive, or adversarial behavior. |
| Tool-Use Prompt | Defines when and how approved tools may be requested. |
| Structured-Output Prompt | Requires output conforming to an approved Schema. |
| Retrieval Prompt | Guides selection and use of authorized context. |
| Reconciliation Prompt | Assists comparison of conflicting evidence without asserting authority. |

A Prompt may serve multiple related purposes only when its scope, outputs, authority, and evaluation remain clear.

---

## 7. Human Authority and Action Classes

Prompts must preserve the approved Action Classes.

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

A Prompt must not treat any of the following as execution authority:

- AI confidence.
- Model score.
- Opportunity stage.
- Priority.
- Task assignment.
- Draft content.
- User-interface state.
- A previous similar approval.
- The presence of a tool.
- A tool result.
- A provider acknowledgement without authoritative Confirmation.

A Prompt may identify required authority.

It must not create or fabricate that authority.

---

## 8. AI Execution Boundary

AI Agents must not directly execute external actions.

A Prompt may request analysis, drafting, classification, extraction, recommendation, explanation, or a proposed tool call.

Execution must occur only through approved orchestration services that enforce:

- Authentication.
- Authorization.
- Tenant scope.
- Role and permission scope.
- Human Approval where required.
- Automation-policy scope where permitted.
- Business Rules.
- Security controls.
- Consent controls.
- Idempotency.
- Audit logging.
- External Confirmation handling.
- Suspension and revocation.

A Prompt must not instruct the model to bypass orchestration or call an external provider outside approved controls.

---

## 9. Required State Separation

Prompts must preserve the distinction between:

```text
Source content available
  ≠ Source content authoritative

Model inference generated
  ≠ Authoritative fact established

Recommendation generated
  ≠ Action authorized

Draft prepared
  ≠ Message approved

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

A Prompt result does not prove execution.

A Command does not prove completion.

A provider acknowledgement does not prove the authoritative business outcome unless it is the configured authoritative Confirmation.

---

## 10. Required Prompt Structure

Each Prompt asset must contain or reference the following sections.

### 10.1 Metadata

- Prompt ID.
- Title.
- Version.
- Status.
- Prompt owner.
- Governance owner.
- Model or model-class assumptions.
- Applies-to scope.
- Tenant or deployment scope where applicable.
- Effective date where approved.
- Last updated date.

### 10.2 Purpose

- Task objective.
- Customer or business value.
- Explicit non-goals.
- Expected output.
- Completion criteria.

### 10.3 Authority and Sources

- Governing Constitution sections.
- Governing Business Rules.
- Related Playbooks.
- Related Canonical Domain Objects.
- Related Events, APIs, Schemas, Agent Contracts, and Integrations.
- Approved source types.
- Prohibited source types.

### 10.4 Inputs

Each input must define:

- Name.
- Meaning.
- Data type or content type.
- Required or optional status.
- Authoritative source where applicable.
- Freshness requirement.
- Security classification.
- Privacy classification.
- Null, unknown, and missing handling.
- Maximum allowed size where required.
- Injection-risk treatment.

### 10.5 Instructions

Instructions must define:

- Required task behavior.
- Evidence requirements.
- Output constraints.
- Tone constraints where applicable.
- Tool-use constraints.
- Authority limits.
- Prohibited behavior.
- Abstention conditions.
- Escalation conditions.

### 10.6 Outputs

Each output must define:

- Output name.
- Meaning.
- Expected structure.
- Required fields.
- Optional fields.
- Enumerated values where applicable.
- Confidence representation where meaningful.
- Evidence references.
- Error representation.
- Prohibited downstream interpretation.

### 10.7 Failure Behavior

- Missing context.
- Stale evidence.
- Conflicting evidence.
- Unsupported request.
- Unauthorized request.
- Prompt injection.
- Tool failure.
- Structured-output failure.
- Low confidence.
- Safety concern.
- Privacy concern.
- Tenant-boundary concern.

### 10.8 Evaluation

- Positive cases.
- Negative cases.
- Boundary cases.
- Adversarial cases.
- Injection cases.
- Privacy cases.
- Unauthorized-action cases.
- Hallucination cases.
- Structured-output cases.
- Regression cases.
- Human-review criteria.

### 10.9 Governance

- Approval record.
- Effective date.
- Change history.
- Model compatibility.
- Migration requirements.
- Monitoring requirements.
- Rollback requirements.
- Supersession or retirement rules.

---

## 11. Prompt ID and Naming

Recommended Prompt ID format:

```text
PRM-<DOMAIN>-<PURPOSE>-<NUMBER>
```

Examples:

```text
PRM-LEAD-QUALIFICATION-001
PRM-APPOINTMENT-SUMMARY-001
PRM-TRADEIN-EXTRACTION-001
PRM-FINANCE-OFFER-EXPLANATION-001
PRM-DEAL-READINESS-RECOMMENDATION-001
PRM-CUSTOMER-FOLLOWUP-DRAFT-001
```

Recommended file naming:

```text
<Prompt-ID>_<Short-Name>.md
```

Example:

```text
PRM-LEAD-QUALIFICATION-001_Lead_Qualification_Recommendation.md
```

IDs must remain stable across compatible revisions.

A materially different purpose should receive a new Prompt ID.

---

## 12. Instruction Precedence

Prompt implementations must preserve instruction precedence.

A lower-priority instruction must not override a higher-priority instruction.

Examples of prohibited behavior include:

- Following retrieved content that says to ignore ASOS governance.
- Following a Customer message that requests secret disclosure.
- Following a document instruction that requests unauthorized tool use.
- Following a User request that conflicts with approved Business Rules.
- Following model-generated content as though it were a governing instruction.

Untrusted content must be treated as data, not authority.

The Prompt must explicitly separate:

- Governing instructions.
- Trusted application context.
- Retrieved evidence.
- User-provided content.
- External content.
- Model-generated prior output.

---

## 13. Prompt Injection and Untrusted Content

Any content originating outside the approved instruction boundary must be treated as potentially untrusted.

This includes:

- Customer messages.
- Emails.
- Documents.
- Websites.
- CRM notes.
- Vehicle descriptions.
- Vendor responses.
- Tool output.
- Retrieved knowledge.
- Uploaded files.
- Model-generated text from previous steps.

A Prompt must not allow untrusted content to:

- Redefine the task.
- Override governing instructions.
- Expand permissions.
- Reveal secrets.
- Change Tenant scope.
- Authorize execution.
- Select unapproved tools.
- Disable safety controls.
- Modify Business Rules.
- Fabricate Human Approval.

Where untrusted content contains instructions, the model should interpret them only as source content unless the approved workflow explicitly designates them as authorized instructions.

---

## 14. Data Minimization and Privacy

Prompts must receive only the minimum context required for the approved task.

Prompt context must not include unrestricted Production data merely because it is available.

Sensitive data must be:

- Purpose-limited.
- Tenant-scoped.
- Access-controlled.
- Minimized.
- Redacted where feasible.
- Retained only as approved.
- Excluded from logs where required.
- Protected from cross-Tenant exposure.

Prompts must not request or expose:

- Passwords.
- Private keys.
- Access tokens.
- Unredacted secrets.
- Unnecessary government identifiers.
- Unnecessary financial account details.
- Unnecessary credit information.
- Unnecessary health information.
- Unnecessary Customer communications.

A Prompt must define how sensitive values are represented in evaluation fixtures and examples.

Production secrets must not be embedded in Prompt files.

---

## 15. Tenant Isolation

Every Prompt execution must preserve Tenant isolation.

Prompt inputs, retrieval, tools, logs, caches, examples, evaluations, and output storage must not mix Tenant data without an approved cross-Tenant purpose and explicit controls.

The model must not infer that similarly named Customers, vehicles, dealerships, branches, or records belong to the same Tenant.

A Prompt must not request cross-Tenant comparison using identifiable data unless the workflow is explicitly authorized and privacy-preserving.

Global learning or analytics must use approved de-identification, aggregation, or governance controls.

---

## 16. Evidence and Grounding

A Prompt must state which claims require evidence.

Material recommendations should identify:

- Relevant facts.
- Source references.
- Source authority.
- Freshness.
- Conflicts.
- Missing evidence.
- Assumptions.
- Derived Intelligence.
- Confidence where meaningful.

A Prompt must not instruct the model to present an unsupported assumption as a fact.

When evidence is missing, stale, conflicting, incomplete, or unverified, the model must produce a defined result such as:

- `INSUFFICIENT_EVIDENCE`.
- `STALE_EVIDENCE`.
- `CONFLICTING_EVIDENCE`.
- `REVIEW_REQUIRED`.
- `ABSTAINED`.
- `UNSUPPORTED_SCOPE`.

The model must not fabricate evidence, citations, approvals, prices, inventory, eligibility, or external outcomes.

---

## 17. Confidence and Uncertainty

Confidence must not replace evidence.

A Prompt requesting confidence must define:

- What confidence represents.
- The permitted scale.
- Calibration expectations.
- Required evidence.
- Low-confidence behavior.
- Abstention threshold where applicable.
- Prohibited downstream use.

A confidence value must not be interpreted as:

- A Human Decision.
- A Business Rule result.
- Execution authority.
- External Confirmation.
- A guarantee of correctness.

The model must explicitly identify material uncertainty.

---

## 18. Structured Outputs

Structured outputs should be used where downstream systems require machine-readable results.

A Structured-Output Prompt must define:

- Schema name and version.
- Required fields.
- Optional fields.
- Data types.
- Enumerations.
- Null handling.
- Unknown handling.
- Evidence-reference fields.
- Error fields.
- Confidence fields where applicable.
- Validation behavior.

Free-form text must not be parsed as a binding Decision or Command when an approved structured contract is required.

A schema-valid output is not automatically semantically correct.

Downstream services must still apply:

- Authorization.
- Business Rules.
- Canonical validation.
- Tenant checks.
- Security checks.
- Approval checks.
- Reconciliation.

---

## 19. Tool Use

A Prompt that allows tool use must define:

- Permitted tools.
- Permitted operations.
- Required inputs.
- Prohibited inputs.
- Tenant scope.
- Authorization requirements.
- Approval requirements.
- Expected tool outputs.
- Failure behavior.
- Retry behavior.
- Audit requirements.

The presence of a tool does not grant permission to use it.

A model request for tool use is not execution authority.

Tool output must be treated according to its source authority and freshness.

A tool acknowledgement does not prove a final business outcome unless it is the configured authoritative Confirmation.

---

## 20. Drafting and Customer-Facing Content

Prompts used to draft Customer-facing content must define:

- Approved channel.
- Approved purpose.
- Required Customer context.
- Consent requirements.
- Communication preferences.
- Tone requirements.
- Prohibited claims.
- Required disclaimers where applicable.
- Approval requirements.
- Automation-policy scope where applicable.
- Expiration or freshness requirements.

A draft is not an approved message.

A Customer-facing draft must not:

- Fabricate urgency.
- Fabricate availability.
- Fabricate price.
- Fabricate approval.
- Fabricate eligibility.
- Conceal material information.
- Misrepresent a Recommendation as a Decision.
- Misrepresent a draft as a final offer.
- Bypass Consent.
- Apply prohibited discrimination.
- Use manipulative or coercive language.

---

## 21. Deterministic Controls

Prompts must not replace deterministic or formally governed enforcement for:

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

A Prompt may explain a deterministic result.

It must not silently change that result.

---

## 22. Events, Commands, and Confirmations

When a Prompt references Events, it must align with the approved Event Catalog.

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

Prompts must not generate canonical Event IDs for authoritative Events unless the Prompt is part of the approved Event-producing boundary and the surrounding service enforces the contract.

Retryable Commands must use stable approved idempotency.

A Prompt must not represent a proposed Command as an executed Command.

---

## 23. Model Compatibility

Each approved Prompt must identify its model assumptions.

Relevant assumptions may include:

- Required context length.
- Structured-output support.
- Tool-use support.
- Language support.
- Determinism expectations.
- Temperature or sampling constraints.
- Safety behavior.
- Retrieval behavior.
- Multimodal support.

A model change requires compatibility evaluation.

A Prompt approved for one model or model class is not automatically approved for another.

Model-version changes must be assessed for:

- Instruction following.
- Hallucination rate.
- Refusal behavior.
- Tool-use behavior.
- Structured-output reliability.
- Privacy behavior.
- Injection resistance.
- Bias and fairness.
- Latency.
- Cost.
- Regression risk.

---

## 24. Prompt Composition and Chains

A Prompt chain must define:

- Step order.
- Input and output contracts.
- State ownership.
- Evidence propagation.
- Authority boundaries.
- Failure propagation.
- Retry behavior.
- Human Review points.
- Audit correlation.

Model-generated output from one step must not become trusted authority in a later step merely because it is reused.

Each step must preserve the distinction between:

- Source evidence.
- Extracted data.
- Derived Intelligence.
- Recommendation.
- Human Decision.
- Command.
- Confirmation.

A later Prompt must not silently upgrade the authority of an earlier output.

---

## 25. Evaluation Requirements

Every material Prompt must have an evaluation set appropriate to its risk.

Evaluation should include:

- Representative normal cases.
- Missing-context cases.
- Stale-data cases.
- Conflicting-evidence cases.
- Ambiguous-request cases.
- Unsupported-scope cases.
- Hallucination cases.
- Prompt-injection cases.
- Secret-extraction cases.
- Cross-Tenant cases.
- Unauthorized-action cases.
- Action Class 2 approval cases.
- Action Class 3 Decision cases.
- Tool-failure cases.
- Structured-output cases.
- Customer-protection cases.
- Bias and fairness cases.
- Regression cases.

Evaluation criteria must be explicit.

A Prompt must not be approved solely because a small number of examples appear correct.

---

## 26. Evaluation Metrics

Metrics must reflect the Prompt purpose and risk.

Possible metrics include:

- Factual accuracy.
- Evidence coverage.
- Unsupported-claim rate.
- Abstention quality.
- Classification precision and recall.
- Extraction accuracy.
- Schema-validity rate.
- Tool-selection accuracy.
- Unauthorized-action rate.
- Injection-resistance rate.
- Secret-disclosure rate.
- Cross-Tenant leakage rate.
- Human-approval compliance.
- Customer-protection compliance.
- Explanation quality.
- Consistency.
- Latency.
- Cost.

Success thresholds are deployment-specific and must remain configurable unless an approved universal minimum exists.

A single aggregate score must not hide a critical safety failure.

---

## 27. Human Review

A Prompt must define when Human Review is:

- Optional.
- Required before use.
- Required before Customer communication.
- Required before tool execution.
- Required before accepting a high-impact conclusion.
- Required after an exception or override.

Human Review must provide the reviewer with:

- Source evidence.
- Material assumptions.
- Uncertainty.
- Prompt version.
- Model version.
- Relevant Business Rules.
- Required approval scope.
- Proposed output or action.

Human Review must not be reduced to a meaningless confirmation click without sufficient context.

---

## 28. Logging and Audit

Material Prompt execution should record, where applicable:

- `tenant_id`.
- Prompt ID.
- Prompt version.
- Model identifier and version.
- Deployment configuration version.
- User or service identity.
- Role and permission scope.
- Input references.
- Evidence references.
- Retrieval references.
- Tool requests and responses.
- Output.
- Confidence and uncertainty.
- Abstention or refusal reason.
- Human Review.
- Human approval, rejection, or modification.
- Correlation and causation references.
- Related Domain Object IDs.
- Related Event IDs.
- Timestamp.
- Errors and retries.

Sensitive content must be minimized, redacted, hashed, or referenced according to approved policy.

Auditability must not be implemented by logging unrestricted secrets or unnecessary personal data.

---

## 29. Security Review

Prompt security review must assess:

- Instruction precedence.
- Prompt injection.
- Indirect prompt injection.
- Secret disclosure.
- Data exfiltration.
- Cross-Tenant leakage.
- Unauthorized tool use.
- Tool-argument manipulation.
- Over-broad retrieval.
- Unsafe content transformation.
- Log leakage.
- Evaluation-data leakage.
- Model-output trust escalation.
- Denial-of-service inputs.
- Excessive context or token usage.

High-risk Prompts require adversarial testing before approval.

---

## 30. Customer Protection and Fairness

Prompts must preserve Customer dignity, lawful treatment, fairness, and transparency.

A Prompt must not instruct the model to:

- Mislead a Customer.
- Manipulate a Customer.
- Conceal material information.
- Fabricate scarcity or urgency.
- Fabricate price or availability.
- Fabricate finance approval or eligibility.
- Apply prohibited discrimination.
- Infer protected characteristics without lawful need.
- Use sensitive attributes for prohibited purposes.
- Optimize revenue through unlawful or harmful behavior.

Where a recommendation may materially affect a Customer, the Prompt must require sufficient evidence, explanation, and Human authority.

---

## 31. Language and Localization

Prompts supporting multiple languages must define:

- Supported languages.
- Source language behavior.
- Output language behavior.
- Translation requirements.
- Terminology requirements.
- Jurisdiction-specific wording.
- Cultural and communication constraints.
- Unsupported-language behavior.

Translation must preserve meaning and uncertainty.

A translation must not introduce authority, approval, guarantees, or legal claims absent from the source.

---

## 32. Versioning and Status

Prompts use semantic versioning:

- Major version for incompatible purpose, authority, output-contract, tool-permission, or behavior changes.
- Minor version for backward-compatible capabilities or substantial instruction changes.
- Patch version for non-semantic corrections, formatting, or examples that do not change behavior.

Approved status values include:

- `Draft`.
- `Under Review`.
- `Evaluation Approved`.
- `Pilot Approved`.
- `Production Approved`.
- `Suspended`.
- `Superseded`.
- `Retired`.

A Prompt status must not imply broader deployment approval than was actually granted.

A repository commit does not automatically make a Prompt effective in Production.

Production effectiveness also requires applicable:

- Formal approval.
- Effective-date control.
- Model compatibility.
- Configuration readiness.
- Tool and integration readiness.
- Security review.
- Privacy review.
- Evaluation approval.
- Monitoring.
- Rollback readiness.

---

## 33. Change Control

A Prompt change must identify:

- Problem addressed.
- Proposed change.
- Reason for change.
- Behavioral impact.
- Authority impact.
- Input-contract impact.
- Output-contract impact.
- Tool-use impact.
- Security impact.
- Privacy impact.
- Customer impact.
- Evaluation impact.
- Model compatibility impact.
- Migration requirements.
- Rollback plan.

Material Prompt changes require re-evaluation.

A Prompt must not be edited directly in Production without versioning, review, and audit.

Emergency suspension must be supported when unsafe behavior is detected.

---

## 34. Rollback and Suspension

Each Production-approved Prompt must support:

- Rapid suspension.
- Version rollback.
- Tool revocation.
- Automation-policy revocation.
- Model rollback where applicable.
- Traffic reduction or isolation.
- Human-only fallback.
- Incident logging.
- Post-incident review.

A suspended Prompt must not continue to receive Production traffic through an undocumented path.

---

## 35. Monitoring

Production Prompt monitoring should include:

- Unsupported-claim rate.
- Abstention rate.
- Human-edit rate.
- Human-rejection rate.
- Unauthorized-action attempts.
- Tool-use failures.
- Injection detections.
- Sensitive-data exposure incidents.
- Cross-Tenant concerns.
- Schema-validation failures.
- Customer complaints.
- Safety incidents.
- Latency.
- Cost.
- Model drift.
- Outcome quality where measurable.

Monitoring must not treat high usage or low latency as proof of safe or correct behavior.

---

## 36. Examples and Test Fixtures

Examples must be:

- Clearly labeled.
- Synthetic or appropriately de-identified where possible.
- Tenant-safe.
- Free of Production secrets.
- Free of unnecessary personal data.
- Representative of normal and failure cases.
- Versioned with the Prompt where behavior depends on them.

Examples must not silently become Business Rules.

Few-shot examples must not override explicit governing instructions.

---

## 37. Content Boundaries

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and reusable templates | [`../06_Assets`](../06_Assets/) |
| Canonical Domain, Event, API, Schema, Agent, and Integration contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

Prompts may reference these locations.

They must not duplicate their authoritative content in a way that creates conflicting definitions.

---

## 38. Related Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS Constitution Index](../00_Constitution/README.md)
- [ASOS Playbooks Governance](../01_Playbooks/README.md)
- [ASOS Business Rules Governance](../02_Business_Rules/README.md)
- [ASOS Product Vision](../05_Documentation/Product_Vision.md)
- [ASOS System Architecture](../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/README.md)
- [ASOS Canonical Event Catalog Governance](../07_Knowledge_Base/docs/02-Event-Catalog/README.md)

---

## 39. Prompt Review Checklist

Before approval, reviewers must confirm:

### Purpose and Scope

- The Prompt has one clear approved purpose.
- Non-goals are explicit.
- Inputs and outputs are defined.
- Deployment scope is defined.

### Authority

- Higher-level authority is referenced.
- Action Class impact is correct.
- Human Approval requirements are explicit.
- The Prompt does not create execution authority.

### Evidence

- Required evidence is defined.
- Missing and conflicting evidence behavior is defined.
- Unsupported claims are prohibited.
- Confidence is not treated as authority.

### Security and Privacy

- Prompt-injection risks are evaluated.
- Secrets are excluded.
- Tenant isolation is preserved.
- Sensitive data is minimized.
- Tool permissions are bounded.

### Behavior

- Prohibited behavior is explicit.
- Abstention behavior is defined.
- Failure behavior is defined.
- Structured output is validated where required.

### Evaluation

- Normal cases pass.
- Boundary cases pass.
- Adversarial cases pass.
- Unauthorized-action cases pass.
- Regression criteria are defined.

### Release

- Version is assigned.
- Status is accurate.
- Approval evidence exists.
- Monitoring is defined.
- Rollback is defined.

---

## 40. Current Status

**Phase:** Prompt governance baseline established; individual Prompt development and controlled evaluation may proceed  
**Version:** `1.0.0`  
**Status:** Approved Baseline  

This README establishes the approved governance baseline for ASOS Prompt assets.

It does not by itself approve any individual Prompt for Production use.

Future Prompt work must preserve:

- Constitutional compliance.
- Human authority.
- Action Class boundaries.
- Evidence before assertion.
- Deterministic controls.
- AI execution separation.
- Prompt-injection resistance.
- Data minimization.
- Tenant isolation.
- Customer protection.
- Structured-output validation.
- Tool-use governance.
- Evaluation and monitoring.
- Auditability.
- Controlled change.
