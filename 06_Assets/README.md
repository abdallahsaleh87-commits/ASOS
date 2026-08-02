# ASOS Assets

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Product Design, Architecture, Documentation, Brand, Security, and Governance  
**Applies To:** Diagrams, images, icons, reusable visual templates, document-layout templates, presentation graphics, source artwork, exported renditions, wireframes, mockups, and other governed supporting assets stored in this directory  
**Last Updated:** 2026-08-02  

---

## 1. Purpose

This directory is the governed location for reusable ASOS visual, design, diagram, and document-support assets.

Assets may support:

- Product and architecture communication.
- Canonical Domain and Event documentation.
- Operational Playbooks.
- Business Rule explanation.
- Prompt and Agent evaluation guidance.
- Pilot training.
- User-interface design.
- Presentations and workshops.
- Customer-safe demonstrations.
- Internal documentation.
- Release communication.

An Asset must improve communication without creating a competing source of authority.

A diagram, screenshot, icon, template, or exported image must not be treated as authoritative merely because it exists in the repository.

---

## 2. Current Baseline

The current approved baseline establishes governance, classification, ownership, accessibility, security, licensing, versioning, release, and change-control requirements for ASOS Assets.

No individual Asset is designated by this README as an approved Production Asset.

An individual Asset becomes eligible for approved use only after its:

- Purpose is defined.
- Owner is assigned.
- Category is defined.
- Intended audience is defined.
- Source file is identified where applicable.
- Authoritative references are linked.
- Sensitive-data impact is reviewed.
- Copyright, license, and usage rights are confirmed.
- Accessibility requirements are satisfied.
- Brand requirements are satisfied where applicable.
- Accuracy is reviewed.
- Version and status are assigned.
- Approval evidence is recorded.

A file existing in this directory does not automatically make it approved, accurate, current, licensed, accessible, safe, or Production-ready.

---

## 3. Authority Hierarchy

Assets operate under the following authority order:

```text
Applicable legal and regulatory requirements
        ↓
Binding contractual, OEM, lender, and governmental requirements
        ↓
ASOS Constitution
        ↓
Product, architecture, security, privacy, brand, and data-governance policies
        ↓
Canonical Domain Models, Events, APIs, Schemas, Agent Contracts, and Integration Contracts
        ↓
Approved Business Rules
        ↓
Approved operational Playbooks
        ↓
Approved Prompts and AI Agent instructions
        ↓
Approved Asset source and usage documentation
        ↓
Exported renditions, screenshots, presentations, and individual visual examples
```

An Asset must not override a higher-level authority.

Where an Asset conflicts with an authoritative source, the authoritative source applies and the Asset must be corrected, suspended, superseded, or retired.

---

## 4. What an Asset Is

An Asset is a governed reusable file that supports visual communication, documentation, design, training, demonstration, or implementation alignment.

An Asset may include:

- Architecture diagrams.
- Workflow diagrams.
- Sequence diagrams.
- Domain relationship diagrams.
- Event-flow diagrams.
- Data-ownership diagrams.
- Integration maps.
- User-interface wireframes.
- User-interface mockups.
- Icons and symbols.
- Logos and approved brand elements.
- Presentation graphics.
- Document-layout templates.
- Diagram source files.
- Exported image renditions.
- Safe screenshots.
- Training visuals.
- Approved reusable visual components.

An Asset should communicate a defined purpose to a defined audience using accurate, accessible, and governed content.

---

## 5. What an Asset Is Not

An Asset is not:

- A constitutional rule.
- A legal authority.
- A Business Rule.
- An operational Playbook.
- A Prompt.
- An AI Agent Contract.
- A Canonical Domain Object definition.
- A Canonical Event definition.
- An API, Schema, or Integration Contract.
- A System-of-Record definition.
- A Production dataset.
- A Human Approval.
- Execution authority.
- A Command.
- An External Confirmation.
- Proof that a business outcome completed.
- Proof that a feature exists or is deployed.
- Proof that a screen is implemented.
- Proof that a process is legally or operationally approved.

Assets may reference authoritative sources.

They must not create competing definitions.

---

## 6. Permitted Asset Categories

| Category | Purpose |
| :--- | :--- |
| Architecture Diagram | Communicates approved components, boundaries, dependencies, and data flows. |
| Domain Diagram | Visualizes approved Domain Objects, ownership, and relationships. |
| Event Diagram | Visualizes approved Event production, transport, consumption, and confirmation behavior. |
| Workflow Diagram | Communicates approved workflow states, Decisions, approvals, Commands, Confirmations, and exceptions. |
| Data-Ownership Diagram | Visualizes Systems of Record, canonical ownership, write authority, and synchronization direction. |
| Integration Map | Visualizes approved system boundaries, interfaces, and provider relationships. |
| Wireframe | Explores page structure and interaction without implying final design or Production availability. |
| Mockup | Represents a proposed or approved visual design with explicit status. |
| Icon or Symbol | Provides reusable visual meaning under approved brand and accessibility rules. |
| Brand Asset | Provides approved logos, marks, typography guidance, or visual identity elements. |
| Presentation Asset | Supports approved internal or external communication. |
| Document Template | Provides reusable visual structure without replacing governed document content. |
| Screenshot | Captures a controlled interface state for documentation or evidence under strict privacy controls. |
| Training Visual | Supports authorized training using current approved information. |
| Exported Rendition | Provides a generated format derived from an approved source asset. |

Every Asset must have a defined category, purpose, owner, and status.

---

## 7. Prohibited Content

This directory must not contain uncontrolled or unnecessary copies of:

- Production secrets.
- Passwords.
- Private keys.
- Access tokens.
- API credentials.
- Session cookies.
- Production connection strings.
- Unredacted Customer information.
- Unnecessary government identifiers.
- Payment-card or bank-account information.
- Credit reports or sensitive finance records.
- Signed contracts or legal documents containing unnecessary personal data.
- Live operational screenshots containing unrestricted Production data.
- Unlicensed third-party artwork, photographs, fonts, icons, or templates.
- Misleading diagrams that contradict approved architecture or ownership.
- Images that imply approval, completion, availability, pricing, eligibility, or authority that does not exist.
- Executable code or macros disguised as a visual Asset.
- Files whose origin, rights, or ownership cannot be established.

Production secrets must not be embedded in Asset files.

Sensitive Production data must not appear in screenshots, diagrams, image metadata, document properties, hidden layers, comments, or exported files.

---

## 8. Asset Classification

Every Asset must have an explicit classification.

| Classification | Meaning |
| :--- | :--- |
| Public | Approved for public disclosure. |
| Internal | Non-public ASOS information intended for authorized internal use. |
| Confidential | Sensitive product, architecture, dealership, provider, Customer, or operational information. |
| Restricted | Highly sensitive information requiring strict access and handling controls. |
| Draft | Incomplete or unapproved content not suitable for reliance. |
| Synthetic | Uses artificial data and fictional examples. |
| Redacted | Derived from source material after approved sensitive elements were removed or masked. |

`Synthetic` and `Redacted` describe origin or treatment and do not automatically define access level.

A public-looking image is not Public unless its classification and approval explicitly say so.

---

## 9. Required Asset Manifest

Each material Asset must contain, reference, or be accompanied by a governed manifest.

The manifest must define:

- Asset ID.
- Title.
- Version.
- Status.
- Category.
- Owner.
- Reviewer or approving authority.
- Purpose.
- Intended audience.
- Permitted uses.
- Prohibited uses.
- Classification.
- Tenant or deployment scope where applicable.
- Environment scope.
- Source file.
- Exported renditions.
- Authoritative references.
- Accessibility requirements.
- Brand requirements.
- Copyright and license information.
- Sensitive-data treatment.
- Creation method.
- AI-generation disclosure where applicable.
- Last review date.
- Known limitations.
- Approval evidence.
- Change history.

An undocumented Asset must not be promoted to governed use.

---

## 10. Asset IDs and Naming

Recommended Asset ID format:

```text
AST-<CATEGORY>-<PURPOSE>-<NUMBER>
```

Examples:

```text
AST-ARCH-SYSTEM-CONTEXT-001
AST-DOMAIN-SALES-LIFECYCLE-001
AST-EVENT-FUNDING-FLOW-001
AST-WORKFLOW-ACTION-APPROVAL-001
AST-UI-OPPORTUNITY-DETAIL-001
AST-BRAND-PRIMARY-LOGO-001
```

Recommended source-file naming:

```text
<Asset-ID>_<Short-Name>_<Version>.<extension>
```

Recommended exported-rendition naming:

```text
<Asset-ID>_<Short-Name>_<Version>_<Purpose-or-Size>.<extension>
```

IDs must remain stable across compatible revisions.

A materially different purpose, audience, or visual meaning should receive a new Asset ID.

---

## 11. Source Files and Exported Renditions

Where an Asset has an editable source, the source file must be identified as the maintained source.

Examples include:

- Diagram source.
- Vector artwork.
- Design-tool source.
- Presentation source.
- Document-template source.

An exported `.png`, `.jpg`, `.webp`, `.pdf`, or similar rendition must not silently become the maintained source when an editable source exists.

Source and exported renditions must remain version-aligned.

A change to the source that affects meaning requires review of every published rendition.

Generated renditions should record:

- Source Asset ID and version.
- Export format.
- Export date where required.
- Intended use.
- Resolution or dimensions where relevant.
- Accessibility alternative.

---

## 12. Supported Formats

Typical approved formats may include:

| Format | Typical use |
| :--- | :--- |
| `.svg` | Scalable diagrams, icons, and vector artwork. |
| `.png` | Lossless screenshots, diagrams, and transparent graphics. |
| `.jpg` or `.jpeg` | Photographic content where compression is acceptable. |
| `.webp` | Optimized web imagery where supported. |
| `.pdf` | Controlled distribution or print-ready rendition. |
| `.drawio` | Editable diagram source where approved tooling is used. |
| `.fig` or governed design export | Approved design-tool source where access and licensing are controlled. |
| `.pptx` | Presentation source under approved document controls. |
| `.docx` | Document-layout template under approved document controls. |
| `.md` | Asset manifest, usage guidance, and text-based diagrams. |
| `.mmd` or Mermaid source | Text-based diagrams where approved and reviewable. |

File type alone does not establish safety or approval.

Files must be scanned and handled according to their actual content and capabilities.

---

## 13. Architecture and Domain Accuracy

Architecture, Domain, Event, data-ownership, workflow, and integration diagrams must reference approved source documents and versions.

A diagram must not:

- Invent a new Domain owner.
- Invent a new System of Record.
- Invent a new write path.
- Invent a new Action Class.
- Invent a new approval path.
- Collapse Recommendation, authorization, Command, and Confirmation states.
- Represent a projection as an externally authoritative fact.
- Represent a provider acknowledgement as completion unless it is the configured authoritative Confirmation.
- Show a proposed component as deployed without a clear status.
- Omit a material security or Tenant boundary when the diagram claims to represent the operational design.

Where a diagram simplifies detail, the simplification and excluded scope must be explicit.

---

## 14. Event and Messaging Diagrams

Event-related Assets must follow the approved Event Catalog and System Architecture.

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

Consumer diagrams must not imply exactly-once delivery when duplicate delivery remains possible.

Replay diagrams must preserve the original canonical Event identity and must not imply that replay creates a new business occurrence.

An Event-flow diagram must distinguish:

- Producer.
- Event Backbone.
- Consumer.
- Retry.
- Dead-letter handling.
- Replay.
- Idempotency behavior.
- External Confirmation where applicable.

---

## 15. Recommendation, Command, and Confirmation Separation

Assets must preserve the distinction between:

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

A screenshot, badge, icon, checkmark, color, or visual state must not falsely imply completion.

A provider acknowledgement must not be displayed as the authoritative business outcome unless it is the configured authoritative Confirmation.

---

## 16. Human Authority and Action Classes

Assets that describe actions, approvals, automation, or workflow must preserve the approved Action Classes.

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

An Asset must not represent any of the following as execution authority:

- AI confidence.
- Model score.
- Opportunity stage.
- Priority.
- Task assignment.
- Draft content.
- User-interface state.
- A checkmark or visual badge.
- A previous similar approval.
- A provider acknowledgement without authoritative Confirmation.

A visual design may display required authority.

It must not fabricate that authority.

---

## 17. User-Interface Mockups and Wireframes

A wireframe or mockup must clearly state its status, such as:

- Concept.
- Draft.
- Under Review.
- Approved Design Baseline.
- Implemented.
- Production Verified.
- Superseded.

A mockup must not imply that a feature is implemented merely because the screen has been designed.

A user-interface Asset must identify, where applicable:

- Intended User role.
- Tenant scope.
- Data classification.
- Displayed source authority.
- Editable and read-only fields.
- Required approval controls.
- Disabled or blocked states.
- Error states.
- Loading states.
- Empty states.
- Expired states.
- Stale-data indicators.
- Conflict indicators.
- Confirmation status.
- Audit or evidence access.

High-impact actions must not be visually presented as casual or reversible when they are not.

---

## 18. Screenshots

Screenshots require special handling because they may capture sensitive data, hidden identifiers, system names, browser details, notifications, or credentials.

Before a screenshot is committed, it must be reviewed for:

- Customer information.
- Dealer or Tenant information.
- Email addresses and phone numbers.
- Government identifiers.
- Financial information.
- Access tokens and secrets.
- URLs containing sensitive parameters.
- Browser tabs and notifications.
- File paths and User names.
- Internal hostnames.
- Debug information.
- Hidden metadata.
- Third-party copyrighted content.

Screenshots should use synthetic or approved redacted data.

Cropping alone is not a sufficient redaction method when sensitive content remains recoverable or visible elsewhere.

---

## 19. Accessibility

Every user-facing or documentation-critical Asset must support accessibility.

Applicable requirements include:

- Meaningful alternative text.
- Text equivalents for complex diagrams.
- Sufficient contrast.
- Legible font size.
- Clear reading order.
- Labels that do not depend only on color.
- Shapes, patterns, text, or symbols in addition to color.
- Keyboard-accessible equivalent behavior for interactive implementations.
- Captions or transcripts for time-based media where applicable.
- Language identification where required.

A diagram containing material governance or operational information must have a text explanation that communicates the same essential meaning.

Decorative Assets should be marked as decorative where supported.

---

## 20. Brand Governance

Brand Assets must use approved names, marks, typography, spacing, colors, and usage rules.

A Brand Asset must not:

- Misrepresent ASOS legal identity.
- Imply partnership, certification, sponsorship, lender approval, OEM approval, or governmental endorsement without authority.
- Modify a third-party mark outside permitted usage.
- Use an obsolete or unapproved logo as a current Production mark.
- Combine marks in a way that creates confusion about ownership or endorsement.

Brand changes require approval from the designated brand owner and any applicable legal review.

---

## 21. Copyright, Licensing, and Usage Rights

Every non-original Asset must have documented usage rights.

The manifest should record:

- Creator or rights holder.
- License type.
- Source.
- Permitted uses.
- Required attribution.
- Modification rights.
- Distribution restrictions.
- Expiration or renewal conditions.
- Geographic or channel restrictions where applicable.

An Asset must not be committed merely because it was available online.

Unclear rights require the Asset to remain blocked from governed use.

Font files must not be committed or redistributed unless the license explicitly permits repository storage and intended distribution.

---

## 22. AI-Generated and AI-Assisted Assets

AI-generated or AI-assisted Assets must be disclosed in their manifest where material.

The Asset owner must review:

- Accuracy.
- Copyright and similarity risk.
- Brand compliance.
- Bias and harmful representation.
- Sensitive-data leakage.
- Fabricated text, logos, interfaces, or approvals.
- Misleading realism.
- Accessibility.
- Intended audience and use.

An AI-generated Asset must not be treated as verified evidence.

AI-generated diagrams must be checked against approved architecture, Domain, Event, and ownership sources.

AI output must not directly create Production approval, execution authority, or an authoritative business outcome.

---

## 23. Templates

A visual or document-layout template may define:

- Page dimensions.
- Spacing.
- Typography roles.
- Section placement.
- Diagram styles.
- Header and footer structure.
- Placeholder locations.
- Accessibility guidance.
- Brand treatment.

A template must not silently embed:

- A competing Business Rule.
- A hidden Prompt.
- An unauthorized workflow.
- A Production secret.
- Real Customer data.
- An unapproved contractual statement.
- Macros or executable behavior without explicit governance.

A template provides structure.

It does not make filled content approved.

---

## 24. Security Requirements

Assets must be processed under approved security controls.

Applicable controls include:

- Malware scanning.
- Secret scanning.
- Metadata inspection.
- Embedded-file inspection.
- Macro inspection.
- Link inspection.
- Access control.
- Tenant-scope review.
- Content classification.
- Copyright and license review.
- Safe export configuration.

Active content, embedded scripts, macros, external references, and linked objects must be disabled, removed, or explicitly approved.

A visual file must not be assumed safe solely because its extension appears non-executable.

---

## 25. Privacy and Customer Protection

Assets must use the minimum information required for their purpose.

An Asset must not:

- Expose unnecessary Customer information.
- Reveal cross-Tenant information.
- Use real Customer stories without approval and lawful basis.
- Fabricate Customer approval or consent.
- Mislead a Customer about pricing, availability, eligibility, finance approval, urgency, or completion.
- Present a draft as an approved offer.
- Present a Recommendation as a confirmed Decision.
- Present a simulated interface as a live Customer record without clear labeling.

Synthetic examples must be visibly distinguishable from real records where confusion is possible.

---

## 26. Tenant and Environment Scope

Every Asset that contains deployment-specific, dealership-specific, or Tenant-specific information must define its scope.

Possible scopes include:

- Platform-wide.
- Public.
- Internal global.
- Single Tenant.
- Single dealership.
- Single branch.
- Approved dealer group.
- Development.
- Test.
- Pilot.
- Demonstration.
- Production documentation.

An Asset approved for one Tenant or environment must not be reused for another scope without review.

A filename, folder, presentation, or User request must not expand Tenant scope.

---

## 27. Data and Asset Boundary

Structured reference data, mappings, synthetic datasets, test fixtures, and evaluation datasets belong in [`../04_Data`](../04_Data/).

Assets may contain small synthetic examples needed to communicate visual meaning.

They must not become the authoritative location for structured Data assets.

A screenshot of data is not a governed replacement for the underlying Data definition.

A diagram label is not a canonical field definition.

---

## 28. Documentation and Contract Boundary

Assets may support documentation and canonical contracts.

Authoritative locations remain:

| Content type | Authoritative location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Prompts and AI instructions | [`../03_Prompts`](../03_Prompts/) |
| Reference data and non-Production datasets | [`../04_Data`](../04_Data/) |
| Product, architecture, governance, and Pilot documentation | [`../05_Documentation`](../05_Documentation/) |
| Canonical Domain, Event, API, Schema, Agent, and Integration contracts | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

An Asset must link to the authoritative source instead of copying and redefining it.

---

## 29. Review Requirements

Review depth must match Asset impact.

Possible reviewers include:

- Product Design.
- Architecture.
- Domain owner.
- Data Governance.
- Security.
- Privacy.
- Legal.
- Brand owner.
- Accessibility reviewer.
- Operations.
- Documentation owner.
- Tenant representative where applicable.

A high-impact architecture, workflow, Customer-facing, legal, financial, security, or brand Asset requires review by the relevant authority.

Self-review alone is insufficient where separation of duties is required.

---

## 30. Testing and Validation

Applicable validation may include:

- Link validation.
- Reference-version validation.
- Visual-regression comparison.
- Resolution and export checks.
- Accessibility review.
- Contrast checks.
- Text-equivalent review.
- Secret scanning.
- Metadata inspection.
- Malware scanning.
- Copyright and license review.
- Cross-format consistency.
- Print validation.
- Mobile and desktop rendering review.
- Localization expansion review.
- Brand review.
- Domain and architecture accuracy review.

An exported Asset must be checked in the format in which it will be consumed.

---

## 31. Versioning

Assets use semantic versioning where practical:

- Major version for incompatible meaning, structure, audience, authority, or brand changes.
- Minor version for backward-compatible content additions or substantial visual clarification.
- Patch version for non-semantic corrections, export fixes, or minor accessibility improvements.

A meaning-changing visual correction must not be treated as a cosmetic patch merely because the file remains visually similar.

The manifest and source file must identify the current version.

Exported renditions must remain traceable to that version.

---

## 32. Status Values

Approved status values include:

- `Draft`.
- `Under Review`.
- `Approved Baseline`.
- `Approved for Internal Use`.
- `Approved for Public Use`.
- `Pilot Approved`.
- `Production Approved`.
- `Suspended`.
- `Superseded`.
- `Retired`.

Status must describe the Asset itself and its permitted use.

It must not imply that the represented feature, workflow, integration, or deployment is approved or implemented unless that has been separately established.

A repository commit does not automatically make an Asset effective or approved for Production use.

---

## 33. Release and Publication

Before an Asset is published, distributed, embedded, or used in Production documentation, confirm:

- Correct Asset ID and version.
- Approved source file.
- Correct exported rendition.
- Current authoritative references.
- Accuracy review.
- Classification.
- Tenant and environment scope.
- Accessibility.
- Brand approval.
- License and attribution.
- Security and privacy review.
- No secrets or sensitive metadata.
- Required approval evidence.
- Publication channel.
- Expiration or review date where required.

Publication approval must match the intended audience and channel.

Internal approval does not automatically permit public distribution.

---

## 34. Change Control

A material Asset change must record:

- Reason for change.
- Changed meaning or visual behavior.
- Source references affected.
- Source file version.
- Exported renditions affected.
- Accessibility impact.
- Brand impact.
- Security or privacy impact.
- Required reviewers.
- Approval evidence.
- Superseded versions.
- Migration or replacement actions.

A change to a referenced authoritative contract may require Asset review even when the Asset file itself has not yet changed.

---

## 35. Supersession and Retirement

A superseded or retired Asset must not remain presented as current.

Where historical retention is required, the Asset should retain:

- Final status.
- Replacement Asset reference.
- Retirement reason.
- Effective date.
- Historical-use restrictions.

Published copies, embedded documentation, presentations, and cached renditions should be replaced or clearly marked where practical.

---

## 36. Repository Hygiene

Asset contributions should avoid:

- Duplicate exports without purpose.
- Uncompressed files where optimization is appropriate.
- Untracked source files.
- Ambiguous names such as `final`, `final2`, or `latest` without a stable Asset ID and version.
- Temporary files.
- Editor caches.
- Hidden backup files.
- Unnecessary embedded fonts.
- Unused layers containing sensitive information.
- Broken external links.
- Uncontrolled binary history growth.

Large or frequently changing binaries may require an approved storage strategy outside the normal repository path.

---

## 37. Recommended Directory Organization

Future Assets may be organized by governed category, for example:

```text
06_Assets/
├── README.md
├── Architecture/
├── Domain/
├── Events/
├── Workflows/
├── Integrations/
├── UI/
├── Brand/
├── Presentations/
├── Templates/
└── Training/
```

Directories should be created only when actual governed content requires them.

An empty planned directory does not establish an approved catalog or Production capability.

---

## 38. Contribution Checklist

Before adding or updating an Asset, confirm:

- The Asset belongs in `06_Assets`.
- The purpose and audience are defined.
- The Asset ID is stable.
- The owner is identified.
- The status is explicit.
- The classification is explicit.
- The source file is identified.
- Exported renditions are traceable.
- Authoritative references are linked.
- No competing definitions are introduced.
- No secrets are present.
- No unnecessary Production data is present.
- Screenshot data is synthetic or properly redacted.
- Tenant and environment scope are clear.
- Copyright and licensing are documented.
- Accessibility requirements are met.
- Brand requirements are met.
- AI assistance is disclosed where material.
- Required reviews are complete.
- Change history is recorded.

---

## 39. Consumer Checklist

Before using an Asset, confirm:

- The Asset is current.
- The status permits the intended use.
- The version matches referenced documentation or contracts.
- The classification permits access and distribution.
- The Tenant and environment scope match.
- The Asset is not superseded or retired.
- The source authority is clear.
- The visual does not replace the underlying authoritative definition.
- Accessibility alternatives are available where needed.
- Required attribution is included.
- The Asset does not expose sensitive information.

---

## 40. Governance Principles

All Assets in this directory must preserve:

- Constitutional compliance.
- One authoritative source for each governed definition.
- Accurate ownership boundaries.
- Human authority.
- Deterministic enforcement boundaries.
- Tenant isolation.
- Data minimization.
- Security and privacy.
- Accessibility.
- Copyright and licensing compliance.
- Brand integrity.
- Recommendation, Command, and Confirmation separation.
- Event identity and replay semantics.
- Auditability.
- Version traceability.
- Controlled change.

An Asset exists to communicate approved meaning.

It must not create authority, hide uncertainty, or imply a completed outcome that has not been authoritatively confirmed.
