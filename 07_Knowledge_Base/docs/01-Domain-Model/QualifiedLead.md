# Qualified Lead

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Qualified Lead Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Qualified Lead Object represents the governed and time-bounded result of determining that one originating Lead has sufficient current evidence to justify active automotive sales engagement.

A Qualified Lead is not a person or organization.

The person or organization is represented by `Customer`.

A Qualified Lead is not the original inquiry.

The original inquiry is represented by `Lead`.

A Qualified Lead is not an active commercial negotiation or sales pipeline record.

The active commercial pursuit is represented by `Opportunity`.

The Qualified Lead establishes the controlled handoff between Lead intake and Opportunity management.

It preserves the exact qualification outcome, evidence, policy, review, commercial intent, routing context, and validity conditions that existed when the positive qualification Decision was accepted.

### Core Domain Separation

```text
Lead
  = original inquiry, intake, source evidence,
    qualification request, evidence collection,
    pending qualification workflow, and negative intake outcomes

Customer
  = canonical individual or organization identity
    and dealership relationship

Qualified Lead
  = accepted positive qualification Decision,
    qualification evidence snapshot,
    commercial-intent snapshot,
    validity and revalidation lifecycle,
    and Opportunity-conversion eligibility

Opportunity
  = active commercial pursuit and sales-pipeline workflow
```

A Lead must not be silently changed into a Qualified Lead.

A new Qualified Lead must be created through a governed and idempotent qualification-acceptance workflow.

The original Lead remains historically traceable.

### Positive Qualification Boundary

The Qualified Lead Object represents an accepted positive qualification outcome.

It must not be created for:

- A disqualified Lead.
- An invalid Lead.
- A duplicate Lead.
- A spam Lead.
- A fraudulent or prohibited inquiry.
- A withdrawn Lead.
- An expired Lead that was never positively qualified.
- An unresolved qualification request.
- A preliminary AI score.
- A Recommendation awaiting Decision.
- An incomplete qualification review.

Negative or non-positive outcomes remain governed by:

- The Lead lifecycle.
- The qualification Decision record.
- Human Review.
- Data-quality workflows.
- Fraud, security, or compliance workflows.

The Qualified Lead may preserve references to the originating qualification request and Decision, but it must not duplicate ownership of the Lead’s intake lifecycle.

### Lead Qualification Responsibility

The Lead Domain Service owns or coordinates:

- Original inquiry capture.
- Source normalization.
- Source deduplication.
- Initial identity-resolution inputs.
- Initial contact-path validation.
- Qualification evidence collection.
- Missing-information collection.
- Qualification request initiation.
- Qualification pending state.
- Lead-level Human Review.
- Lead invalidation.
- Lead duplicate classification.
- Lead withdrawal.
- Lead disqualification.
- Negative qualification outcome.
- Lead expiration before positive qualification.
- Handoff of an accepted positive qualification Decision.

The Qualified Lead Domain Service owns:

- Accepted positive qualification outcome.
- Qualified Lead identity.
- Qualification-policy snapshot.
- Qualification-criteria snapshot.
- Qualification-evidence snapshot.
- Commercial-intent snapshot.
- Customer and Lead relationship integrity.
- Time-bounded qualification validity.
- Freshness.
- Revalidation requirements.
- Revocation and invalidation after qualification.
- Conversion eligibility.
- Opportunity-conversion request state.
- Confirmed conversion linkage.
- Qualification-version history.
- Qualified Lead audit history.

### Customer Requirement

A Qualified Lead must reference exactly one resolved Customer.

A Qualified Lead must not be created from:

- An unidentified party.
- An ambiguous Customer match.
- A disputed Customer identity.
- A Customer belonging to another Tenant.
- An unverified representative without sufficient authority where representation matters.

The original submitted identity information remains preserved in the Lead and source evidence.

The Qualified Lead stores the accepted Customer relationship and the identity-resolution evidence applicable at qualification time.

### One Originating Lead

A Qualified Lead must reference exactly one originating Lead.

The originating Lead must:

- Belong to the same Tenant.
- Contain or reference the qualification evidence.
- Contain a completed positive qualification Decision.
- Reference the same resolved Customer.
- Not be invalid, duplicate, withdrawn, or prohibited at creation time.
- Not already have another current Qualified Lead for the same qualification cycle.

A Lead may support more than one legitimate qualification cycle over time when:

- A previous qualification expired.
- Customer requirements materially changed.
- A previous qualification was revoked.
- A new independent commercial intent exists.
- Policy explicitly allows a new cycle.

Each cycle must remain separately identifiable and historically traceable.

### Qualification Is a Time-Bounded Decision

Qualification is not a permanent Customer attribute.

A Customer may qualify:

- For one Vehicle category but not another.
- For one purchase timeframe but not another.
- For one dealership or region but not another.
- For one sales journey but not another.
- At one point in time but not later.

A qualification may become stale or invalid when:

- Purchase timeframe changes.
- Vehicle requirements materially change.
- Budget context materially changes.
- Payment preference changes.
- Finance interest or prerequisites change.
- Trade-In context materially changes.
- Customer identity becomes disputed.
- Customer representation authority changes.
- Contact permission changes.
- Communication restrictions change.
- Required evidence expires.
- Fraud or compliance risk changes.
- Required review expires.
- The qualification policy changes materially.
- The configured qualification-validity period ends.
- A material source correction is received.
- The Customer withdraws the commercial intent.

The Qualified Lead must therefore preserve:

- Qualification timestamp.
- Qualification policy and version.
- Qualification Decision.
- Qualification criteria and outcomes.
- Evidence snapshot.
- Evidence hash.
- Lead record version.
- Customer identity version or reference.
- Commercial-intent snapshot.
- Validity period.
- Expiration time.
- Revalidation triggers.
- Revalidation history.
- Human Review evidence where required.

### Qualification Request and Decision Separation

The following concepts must remain separate:

```text
Qualification Request
  = request to evaluate whether a Lead satisfies configured criteria

Qualification Evidence
  = facts and records considered during the evaluation

Qualification Score
  = deterministic or AI-derived analytical input

Qualification Recommendation
  = proposed outcome requiring applicable authority

Qualification Decision
  = accepted authoritative determination

Qualified Lead
  = canonical positive qualification result created from that Decision
```

A score is not a Decision.

A Recommendation is not a Decision.

A qualification request is not a Qualified Lead.

A Human Review request is not a Qualified Lead.

### Evidence, Observation, and Interpretation Separation

Qualification records must distinguish:

```text
Original Source Evidence
  = original Lead payload, Interaction, document,
    provider data, or approved source record

Normalized Observation
  = governed representation of source evidence

Deterministic Calculation
  = result produced by approved rules or formulas

Derived Intelligence
  = AI-generated classification, score, extraction,
    prediction, or Recommendation

Human Decision
  = approved qualification determination where required

Qualified Lead Snapshot
  = accepted canonical positive outcome and supporting context
```

Derived Intelligence must not overwrite source evidence.

Human interpretation must not be represented as original Customer evidence.

### Qualification Criteria

Qualification criteria are configuration and policy.

Criteria may consider:

- Sufficient automotive commercial intent.
- Resolved Customer identity.
- Valid or usable contact path.
- Permitted communication basis.
- Purchase purpose.
- Purchase timeframe.
- Vehicle need or category.
- Budget information where required.
- Payment preference.
- Finance interest.
- Trade-In interest.
- Geographic fit.
- Dealership or branch eligibility.
- Fleet or retail context.
- Representation authority.
- Fraud risk.
- Compliance risk.
- Required missing information.
- Required Human Review.
- Another configured criterion.

Criteria, thresholds, weights, mandatory fields, exceptions, and validity periods must not be hard-coded into the Canonical Object.

They must be governed through versioned Business Rules and configuration.

### Qualification Score Boundary

A qualification score may support a Decision.

It must not independently:

- Create a Qualified Lead.
- Reject a Lead.
- Override Human Review.
- Bypass identity resolution.
- Bypass communication permission.
- Bypass fraud or compliance controls.
- Create an Opportunity.
- Commit dealership resources.
- Create a Customer-facing promise.

Every score must preserve:

- Scale.
- Model, formula, or algorithm.
- Version.
- Input records.
- Input versions.
- Evidence.
- Generation time.
- Expiration.
- Limitations.
- Confidence where meaningful.
- Applicable review requirements.

### Commercial Intent Snapshot

The Qualified Lead preserves the accepted commercial-intent context at qualification time.

The snapshot may include:

- Primary commercial intent.
- Vehicle requirements.
- New or used Vehicle preference.
- Purchase purpose.
- Purchase timeframe.
- Budget range.
- Payment preference.
- Finance interest.
- Trade-In interest.
- Appointment interest.
- Test-drive interest.
- Preferred dealership or branch.
- Geographic constraints.
- Language preference.
- Preferred permitted contact channel.
- Routing priority.
- Missing non-blocking information.
- Material assumptions.

The snapshot is historical evidence.

Later changes belong to:

- Revalidation.
- A new qualification version.
- Opportunity requirement versions after conversion.

Later Opportunity changes must not silently rewrite the original Qualified Lead snapshot.

### Contact Permission Projection

The Qualified Lead may contain a projection of current permitted contact status.

The projection must not replace the Customer Domain Service or configured Consent authority.

A Qualified Lead does not create:

- Marketing Consent.
- Cross-channel Consent.
- Recording Consent.
- Finance Consent.
- Permission to disclose sensitive data.
- Unlimited future contact permission.

Before every outbound communication, the applicable deterministic policy must re-evaluate current permission.

### Routing and Priority

The Qualified Lead may preserve routing Recommendations or accepted routing context such as:

- Dealership.
- Branch.
- Team.
- Queue.
- Sales profile.
- Language capability.
- Product specialization.
- Geographic responsibility.
- Fleet specialization.
- Priority.
- Response target.
- Escalation requirement.

AI may recommend routing.

The authoritative assignment and active sales ownership normally begin in Opportunity or the configured CRM workflow.

The Qualified Lead must not silently assign an Opportunity owner before the Opportunity exists.

### Qualified Lead and Opportunity Separation

A Qualified Lead confirms that the commercial intent is currently eligible for active pursuit.

An Opportunity represents the active pursuit after conversion is accepted.

The Qualified Lead owns:

- Conversion eligibility.
- Conversion prerequisites.
- Conversion request.
- Conversion idempotency context.
- Qualification snapshot supplied to Opportunity.
- Conversion linkage after Opportunity creation.

The Opportunity Domain Service owns:

- Opportunity identity.
- Opportunity creation.
- Sales assignment.
- Pipeline stage.
- Requirement evolution after conversion.
- Vehicle matching workflow.
- Appointment coordination.
- Quotation coordination.
- Negotiation.
- Deal conversion.

The Qualified Lead must not describe Opportunity creation as completed until an accepted Opportunity record exists.

### Conversion Does Not Prove Sale

Qualified Lead conversion proves only that an active commercial pursuit was created.

It does not prove:

- Customer commitment.
- Quotation acceptance.
- Vehicle Reservation.
- Vehicle Allocation.
- Trade-In acceptance.
- Finance approval.
- Contract signature.
- Payment.
- Funding.
- Sale posting.
- Delivery.
- Deal completion.

### Revalidation

Revalidation determines whether an existing positive qualification remains current.

Revalidation may result in:

- Qualification remaining active.
- Updated non-material projection.
- New qualification version.
- Revalidation requirement remaining unresolved.
- Qualification expiration.
- Qualification revocation.
- Qualification invalidation.
- Qualification supersession.
- Downstream Human Review.

Revalidation must preserve the prior accepted snapshot.

Material changes must not be applied by overwriting historical qualification evidence.

### Revocation and Invalidation

Revocation means a previously valid qualification is no longer permitted to support further progression because of a later governed occurrence.

Invalidation means the qualification was materially defective, unsupported, or incorrect.

Examples include:

- Identity resolution was wrong.
- Evidence was fraudulent.
- A required criterion was not actually satisfied.
- The qualification Decision lacked authority.
- The policy was applied incorrectly.
- A source correction changed the outcome.
- A compliance restriction was discovered.

Revocation or invalidation after Opportunity creation must:

- Preserve the Qualified Lead.
- Notify the Opportunity workflow.
- Create review or hold requirements.
- Avoid silently deleting or cancelling the Opportunity.
- Preserve Human Decisions and evidence.
- Trigger downstream reconciliation where required.

### External CRM Boundary

A configured CRM may remain authoritative for:

- External qualification status.
- External ownership.
- External conversion status.
- External sales assignment.
- External pipeline creation.

Where an external CRM is authoritative:

- ASOS preserves a Canonical Projection.
- Commands remain pending until External Confirmation.
- External status does not overwrite ASOS evidence without reconciliation.
- ASOS internal qualification meaning remains governed by this Canonical Object.
- Conflicts must remain explicit.

### System Purpose

The Qualified Lead Object provides governed qualification context to:

- Opportunity creation.
- Sales routing.
- Sales work queues.
- Customer engagement planning.
- Vehicle-matching workflows.
- Appointment planning.
- Trade-In discovery.
- Finance-interest discovery.
- Management dashboards.
- Conversion analytics.
- Revalidation workflows.
- AI Agents.
- Audit and compliance services.

The Qualified Lead Object may contain:

- External Authoritative Data.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow State.
- Derived Intelligence.
- Authoritative Human Decisions.
- External Confirmations.

### Authority Boundaries

| Information | Default Authority |
| :--- | :--- |
| Original inquiry and submitted payload | Lead source or provider |
| Canonical Lead | Lead Domain Service |
| Qualification request and pending Lead workflow | Lead Domain Service |
| Negative qualification outcome | Lead Domain Service and applicable Decision authority |
| Resolved Customer identity | Customer Domain Service |
| Contact permission and Consent | Customer Domain Service or configured Consent authority |
| Qualification policy | Policy and Business Rules authority |
| Qualification criteria calculations | Approved deterministic service |
| Qualification score | Derived Intelligence |
| Qualification Recommendation | Recommendation record |
| Positive qualification Decision | Authorized Human or approved deterministic policy |
| Canonical Qualified Lead | Qualified Lead Domain Service |
| Qualification snapshot and validity | Qualified Lead Domain Service |
| Opportunity-conversion eligibility | Qualified Lead Domain Service |
| Opportunity creation | Opportunity Domain Service or configured CRM |
| Opportunity stage and sales pursuit | Opportunity Domain Service or configured CRM |
| Vehicle identity | Vehicle Domain Service |
| Vehicle availability | Inventory Record or configured Inventory authority |
| Final commercial outcome | Deal Domain Service and configured external authorities |

---

## 2. Canonical Schema

### Primary Identifiers

- `qualified_lead_id` — UUIDv4, required and immutable.
- `qualification_cycle_id` — UUIDv4, required and immutable.
- `tenant_id` — UUIDv4, required and immutable.
- `qualification_version` — Integer, required.
- `record_version` — Integer used for optimistic concurrency.

### Organizational Context

- `dealer_group_id`.
- `dealership_id`.
- `branch_id`.
- `legal_entity_id`.
- `receiving_team_id`.
- `routing_queue_id`.
- `recommended_owner_profile_id`.
- `responsible_reviewer_user_id`.
- `responsible_data_steward_user_id`.

`tenant_id` is the primary isolation boundary.

All organizational identifiers must belong to the authenticated Tenant.

### Origin Relationships

- `lead_id`.
- `lead_record_version`.
- `customer_id`.
- `customer_record_version`.
- `qualification_request_id`.
- `qualification_decision_id`.
- `source_interaction_ids`.
- `primary_source_interaction_id`.
- `previous_qualified_lead_version_id`.
- `supersedes_qualified_lead_version_id`.
- `opportunity_id`.
- `opportunity_record_version`.

### Qualified Lead Identity

- `qualified_lead_number`.
- `status`.
- `qualification_result`.
- `qualification_basis_type`.
- `workflow_authority_mode`.
- `is_current_qualification_version`.
- `qualification_accepted_at`.
- `effective_from`.
- `effective_until`.
- `expires_at`.
- `freshness_status`.
- `data_quality_status`.
- `conflict_status`.
- `review_status`.
- `reconciliation_status`.

### Originating Lead Snapshot

- `lead_snapshot`.
- `lead_snapshot_hash`.
- `lead_source_channel`.
- `lead_source_system`.
- `lead_source_record_id`.
- `lead_received_at`.
- `lead_original_payload_reference`.
- `lead_original_payload_hash`.
- `lead_inquiry_summary`.
- `lead_status_at_qualification`.
- `lead_qualification_request_status`.
- `lead_evidence_collection_status`.

### Customer Snapshot

- `customer_snapshot`.
- `customer_snapshot_hash`.
- `customer_identity_status`.
- `customer_identity_resolution_reference`.
- `customer_identity_evidence_references`.
- `customer_contact_point_references`.
- `customer_preferred_language_projection`.
- `customer_geographic_projection`.
- `customer_restriction_projection`.
- `customer_relationship_type_projection`.
- `customer_representation_status`.
- `customer_representative_reference`.

The Customer snapshot must contain only the minimum information required for qualification and conversion.

### Qualification Policy Snapshot

- `qualification_policy_id`.
- `qualification_policy_version`.
- `qualification_policy_name`.
- `qualification_policy_effective_from`.
- `qualification_policy_effective_until`.
- `qualification_policy_snapshot`.
- `qualification_policy_snapshot_hash`.
- `qualification_validity_period_seconds`.
- `required_criteria_ids`.
- `optional_criteria_ids`.
- `blocking_criteria_ids`.
- `required_review_types`.
- `policy_jurisdiction`.
- `policy_dealership_scope`.
- `policy_vehicle_scope`.
- `policy_customer_scope`.

### Qualification Decision

- `qualification_decision_id`.
- `qualification_decision_status`.
- `qualification_result`.
- `qualification_reason_codes`.
- `qualification_decision_summary`.
- `qualification_decided_at`.
- `qualification_decided_by_actor_type`.
- `qualification_decided_by_actor_id`.
- `qualification_decision_authority`.
- `qualification_decision_snapshot`.
- `qualification_decision_snapshot_hash`.
- `qualification_decision_evidence_references`.
- `qualification_decision_expiration_at`.
- `qualification_decision_review_status`.

For a Qualified Lead, `qualification_result` must be `QUALIFIED`.

### Qualification Criteria Results

- `qualification_criterion_result_ids`.
- `qualification_criteria_count`.
- `qualification_criteria_satisfied_count`.
- `qualification_criteria_not_satisfied_count`.
- `qualification_criteria_unknown_count`.
- `blocking_criteria_satisfied`.
- `criteria_snapshot`.
- `criteria_snapshot_hash`.

Each criterion result may contain:

- `criterion_result_id`.
- `criterion_id`.
- `criterion_version`.
- `criterion_name`.
- `criterion_type`.
- `criterion_outcome`.
- `criterion_required`.
- `criterion_blocking`.
- `observed_value`.
- `normalized_value`.
- `calculated_value`.
- `threshold_reference`.
- `evaluation_method`.
- `evaluation_actor_type`.
- `evaluation_actor_id`.
- `evaluated_at`.
- `evidence_references`.
- `assumptions`.
- `limitations`.
- `review_status`.

### Qualification Evidence Package

- `qualification_evidence_package_id`.
- `qualification_evidence_references`.
- `qualification_evidence_count`.
- `qualification_evidence_snapshot`.
- `qualification_evidence_snapshot_hash`.
- `qualification_evidence_integrity_status`.
- `qualification_evidence_completeness_status`.
- `qualification_evidence_freshness_status`.
- `qualification_evidence_conflict_status`.
- `supporting_evidence_count`.
- `contradicting_evidence_count`.
- `excluded_evidence_count`.
- `evidence_exclusion_reasons`.
- `evidence_security_classification`.

Each evidence record may contain:

- `qualification_evidence_id`.
- `evidence_type`.
- `evidence_role`.
- `source_system`.
- `source_record_id`.
- `source_authority`.
- `source_record_version`.
- `evidence_reference`.
- `evidence_hash`.
- `observed_at`.
- `recorded_at`.
- `effective_from`.
- `effective_until`.
- `verification_status`.
- `freshness_status`.
- `exclusion_status`.
- `exclusion_reason`.
- `security_classification`.

### Commercial Intent

- `primary_commercial_intent`.
- `secondary_commercial_intents`.
- `purchase_purpose`.
- `purchase_timeline_category`.
- `target_purchase_date_from`.
- `target_purchase_date_to`.
- `urgency_category`.
- `decision_maker_status`.
- `representation_context`.
- `fleet_or_retail_context`.
- `intended_usage`.
- `commercial_intent_snapshot`.
- `commercial_intent_snapshot_hash`.
- `commercial_intent_evidence_references`.

### Vehicle Requirements

- `vehicle_requirement_status`.
- `vehicle_id`.
- `vehicle_model_reference`.
- `vehicle_catalog_configuration_reference`.
- `vehicle_condition_preference`.
- `make_preferences`.
- `model_preferences`.
- `body_type_preferences`.
- `fuel_type_preferences`.
- `transmission_preferences`.
- `drivetrain_preferences`.
- `color_preferences`.
- `feature_requirements`.
- `required_specifications`.
- `optional_specifications`.
- `excluded_specifications`.
- `quantity_required`.
- `vehicle_requirement_text`.
- `vehicle_requirement_snapshot`.
- `vehicle_requirement_snapshot_hash`.
- `vehicle_requirement_evidence_references`.

A Vehicle requirement does not prove Vehicle availability.

### Budget Context

- `budget_status`.
- `budget_min_amount`.
- `budget_max_amount`.
- `budget_currency_code`.
- `budget_period`.
- `budget_flexibility`.
- `budget_source_type`.
- `budget_verification_status`.
- `budget_snapshot`.
- `budget_snapshot_hash`.
- `budget_evidence_references`.
- `budget_sensitive_data_present`.

Budget information may be optional according to policy.

A Customer-submitted budget is not verified financial capacity.

### Payment Preference

- `payment_preference`.
- `cash_interest_status`.
- `finance_interest_status`.
- `lease_interest_status`.
- `mixed_payment_interest_status`.
- `payment_preference_snapshot`.
- `payment_preference_snapshot_hash`.
- `payment_preference_evidence_references`.

Payment preference is not a Payment outcome.

### Finance Interest

- `finance_interest_status`.
- `finance_information_permission_status`.
- `finance_prequalification_interest`.
- `preferred_finance_term_months`.
- `preferred_periodic_payment_range`.
- `down_payment_range`.
- `finance_constraint_summary`.
- `finance_interest_snapshot`.
- `finance_interest_snapshot_hash`.
- `finance_interest_evidence_references`.

The Qualified Lead must not contain authoritative:

- Credit Decision.
- Lender Decision.
- Approved rate.
- Approved term.
- Credit report.
- Bank statement.
- National identifier.
- Funding status.

### Trade-In Interest

- `trade_in_interest_status`.
- `trade_in_vehicle_count_projection`.
- `trade_in_vehicle_summary`.
- `trade_in_ownership_projection`.
- `trade_in_payoff_interest`.
- `trade_in_snapshot`.
- `trade_in_snapshot_hash`.
- `trade_in_evidence_references`.

Trade-In interest is not an appraisal or acquisition.

### Appointment and Test-Drive Interest

- `appointment_interest_status`.
- `test_drive_interest_status`.
- `preferred_appointment_date_from`.
- `preferred_appointment_date_to`.
- `preferred_time_windows`.
- `preferred_location_type`.
- `preferred_dealership_id`.
- `preferred_branch_id`.
- `appointment_interest_snapshot`.
- `appointment_interest_snapshot_hash`.
- `appointment_interest_evidence_references`.

Interest does not create or confirm an Appointment.

### Geographic and Dealership Context

- `country_code`.
- `region_code`.
- `city_code`.
- `postal_area_reference`.
- `preferred_dealership_ids`.
- `preferred_branch_ids`.
- `geographic_eligibility_status`.
- `dealership_eligibility_status`.
- `routing_geographic_rule_id`.
- `routing_geographic_rule_version`.
- `geographic_snapshot`.
- `geographic_snapshot_hash`.

### Contact Permission Projection

- `contact_permission_status`.
- `contact_permission_basis`.
- `contact_permission_source`.
- `contact_permission_evidence_reference`.
- `contact_permission_checked_at`.
- `contact_permission_expires_at`.
- `permitted_channel_projections`.
- `restricted_channel_projections`.
- `marketing_permission_projection`.
- `transactional_contact_projection`.
- `opt_out_projection`.
- `quiet_hours_projection`.
- `contact_permission_revalidation_required`.

These fields are projections and must be re-evaluated before communication.

### Qualification Scoring

- `qualification_score`.
- `qualification_score_scale`.
- `qualification_score_method`.
- `qualification_score_model_reference`.
- `qualification_score_model_version`.
- `qualification_score_formula_reference`.
- `qualification_score_formula_version`.
- `qualification_score_inputs`.
- `qualification_score_input_hash`.
- `qualification_score_generated_at`.
- `qualification_score_expires_at`.
- `qualification_score_confidence`.
- `qualification_score_limitations`.
- `qualification_score_review_status`.

### Priority and Routing

- `priority_classification`.
- `priority_score`.
- `priority_reason_codes`.
- `routing_status`.
- `recommended_dealership_id`.
- `recommended_branch_id`.
- `recommended_team_id`.
- `recommended_queue_id`.
- `recommended_owner_profile_id`.
- `routing_rule_id`.
- `routing_rule_version`.
- `routing_evidence_references`.
- `routing_decision_id`.
- `routing_snapshot`.
- `routing_snapshot_hash`.
- `routing_expires_at`.

Routing Recommendation does not create Opportunity assignment.

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
- `review_snapshot`.
- `review_snapshot_hash`.

### Derived Intelligence

- `automotive_intent_score`.
- `contactability_score`.
- `urgency_score`.
- `purchase_readiness_score`.
- `vehicle_match_readiness_score`.
- `conversion_readiness_score`.
- `fraud_risk_score`.
- `spam_risk_score`.
- `compliance_risk_score`.
- `missing_information_predictions`.
- `extracted_vehicle_preferences`.
- `extracted_budget_range`.
- `extracted_purchase_timeline`.
- `qualification_summary`.
- `recommended_qualification_outcome`.
- `recommended_next_action`.
- `recommended_routing`.
- `recommended_revalidation`.
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

### Freshness and Validity

- `freshness_status`.
- `freshness_evaluated_at`.
- `freshness_policy_id`.
- `freshness_policy_version`.
- `effective_from`.
- `effective_until`.
- `expires_at`.
- `days_until_expiry`.
- `expiration_reason`.
- `expired_at`.
- `expired_by_policy_id`.
- `validity_extension_decision_id`.
- `validity_extension_reason`.
- `validity_extension_expires_at`.

### Revalidation

- `revalidation_status`.
- `revalidation_required`.
- `revalidation_trigger_codes`.
- `revalidation_requested_at`.
- `revalidation_due_at`.
- `revalidation_request_id`.
- `revalidation_decision_id`.
- `revalidation_policy_id`.
- `revalidation_policy_version`.
- `revalidation_evidence_references`.
- `revalidation_outcome`.
- `revalidated_at`.
- `revalidation_expires_at`.
- `revalidation_snapshot`.
- `revalidation_snapshot_hash`.
- `revalidation_failure_reason`.

### Revocation

- `revocation_status`.
- `revocation_reason`.
- `revocation_request_id`.
- `revocation_decision_id`.
- `revocation_evidence_references`.
- `revoked_at`.
- `revoked_by_actor_id`.
- `downstream_review_required`.
- `downstream_review_references`.

### Invalidation

- `invalidation_status`.
- `invalidation_reason`.
- `invalidation_request_id`.
- `invalidation_decision_id`.
- `invalidation_evidence_references`.
- `invalidated_at`.
- `invalidated_by_actor_id`.
- `affected_opportunity_id`.
- `downstream_reconciliation_status`.

### Opportunity-Conversion Eligibility

- `conversion_eligibility_status`.
- `conversion_eligibility_checked_at`.
- `conversion_eligibility_policy_id`.
- `conversion_eligibility_policy_version`.
- `conversion_eligibility_reason_codes`.
- `conversion_block_reasons`.
- `conversion_eligibility_expires_at`.
- `conversion_readiness_snapshot`.
- `conversion_readiness_snapshot_hash`.

### Opportunity-Conversion Request

- `opportunity_conversion_request_id`.
- `opportunity_conversion_idempotency_key`.
- `opportunity_conversion_requested_at`.
- `opportunity_conversion_requested_by_actor_type`.
- `opportunity_conversion_requested_by_actor_id`.
- `opportunity_conversion_request_status`.
- `opportunity_conversion_expected_record_version`.
- `requested_dealership_id`.
- `requested_branch_id`.
- `requested_sales_team_id`.
- `requested_owner_user_id`.
- `initial_next_action_snapshot`.
- `conversion_request_failure_reason`.
- `conversion_reconciliation_status`.

### Confirmed Opportunity Linkage

- `opportunity_id`.
- `opportunity_record_version`.
- `opportunity_created_at`.
- `opportunity_creation_event_id`.
- `opportunity_conversion_confirmed_at`.
- `opportunity_conversion_confirmation_status`.
- `converted_qualification_version`.
- `converted_qualification_snapshot_hash`.
- `conversion_authority`.
- `conversion_evidence_references`.

The Opportunity Domain Service is authoritative for Opportunity creation.

### Supersession and Correction

- `supersession_status`.
- `supersedes_qualification_version`.
- `superseded_by_qualification_version`.
- `supersession_reason`.
- `supersession_decision_id`.
- `superseded_at`.
- `correction_status`.
- `correction_reason`.
- `correction_decision_id`.
- `corrected_fields`.
- `correction_evidence_references`.
- `corrected_at`.

### Data Quality and Conflict

- `data_quality_status`.
- `missing_required_fields`.
- `missing_optional_fields`.
- `stale_field_names`.
- `conflict_status`.
- `conflict_types`.
- `conflicting_source_references`.
- `conflict_summary`.
- `conflict_resolution_status`.
- `conflict_resolution_decision_id`.
- `conflict_resolved_at`.
- `quarantine_status`.
- `quarantine_reason`.

### Computed Projections

- `qualification_age_seconds`.
- `qualification_age_days`.
- `days_until_expiry`.
- `time_from_lead_receipt_to_qualification_seconds`.
- `time_from_qualification_to_conversion_seconds`.
- `is_current`.
- `is_convertible`.
- `is_expired`.
- `is_revalidation_required`.
- `has_blocking_conflict`.
- `has_resolved_customer`.
- `has_permitted_contact_projection`.
- `has_vehicle_requirement`.
- `has_budget_context`.
- `has_finance_interest`.
- `has_trade_in_interest`.
- `has_appointment_interest`.

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
- `activated_at`.
- `converted_at`.
- `archived_at`.
- `anonymized_at`.

---

## 3. Field Definitions

### Core Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `qualified_lead_id` | UUID | Yes | ASOS | Immutable Canonical Qualified Lead identifier. |
| `qualification_cycle_id` | UUID | Yes | ASOS | Identifier grouping the qualification lifecycle for one commercial-intent cycle. |
| `tenant_id` | UUID | Yes | Security Context | Primary Tenant-isolation identifier. |
| `qualification_version` | Integer | Yes | ASOS | Sequential accepted qualification version. |
| `qualified_lead_number` | String | Yes | ASOS or configured authority | Human-readable Qualified Lead reference. |
| `status` | Enum | Yes | Qualified Lead workflow | Current Qualified Lead lifecycle state. |
| `qualification_result` | Enum | Yes | Qualification Decision | Must be `QUALIFIED` for this Object. |
| `qualification_basis_type` | Enum | Yes | Decision workflow | Basis through which the positive outcome was accepted. |
| `effective_from` | Timestamp | Yes | Qualified Lead workflow | Start of qualification validity. |
| `expires_at` | Timestamp | Yes | Policy | Time after which the qualification cannot be treated as current without revalidation. |
| `record_version` | Integer | Yes | ASOS | Optimistic-concurrency version. |

### Origin Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_id` | UUID | Yes | Canonical relationship | Exactly one originating Lead. |
| `lead_record_version` | Integer | Yes | Lead relationship | Lead version used for qualification. |
| `customer_id` | UUID | Yes | Customer relationship | Exactly one resolved Customer. |
| `customer_record_version` | Integer | Conditional | Customer relationship | Customer version used for the qualification snapshot. |
| `qualification_request_id` | UUID | Yes | Lead workflow | Qualification request from which the Decision originated. |
| `qualification_decision_id` | UUID | Yes | Decision authority | Accepted positive qualification Decision. |
| `primary_source_interaction_id` | UUID | No | Interaction relationship | Primary Interaction supporting qualification. |
| `opportunity_id` | UUID | No | Opportunity relationship | Confirmed Opportunity created from the Qualified Lead. |

### Snapshot Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `lead_snapshot_hash` | String | Yes | ASOS | Integrity hash of the originating Lead snapshot. |
| `customer_snapshot_hash` | String | Yes | ASOS | Integrity hash of the Customer relationship snapshot. |
| `qualification_policy_snapshot_hash` | String | Yes | ASOS | Integrity hash of the applied policy snapshot. |
| `qualification_decision_snapshot_hash` | String | Yes | ASOS | Integrity hash of the positive Decision snapshot. |
| `criteria_snapshot_hash` | String | Yes | ASOS | Integrity hash of the criteria outcomes. |
| `qualification_evidence_snapshot_hash` | String | Yes | ASOS | Integrity hash of the evidence package. |
| `commercial_intent_snapshot_hash` | String | Yes | ASOS | Integrity hash of the commercial-intent snapshot. |

### Policy and Decision Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `qualification_policy_id` | UUID | Yes | Policy authority | Applied qualification policy. |
| `qualification_policy_version` | String | Yes | Policy authority | Exact policy version. |
| `qualification_decision_status` | Enum | Yes | Decision workflow | Status of the accepted qualification Decision. |
| `qualification_reason_codes` | Array | Yes | Decision evidence | Reasons supporting the positive outcome. |
| `qualification_decided_at` | Timestamp | Yes | Decision workflow | Time the positive Decision was accepted. |
| `qualification_decided_by_actor_type` | Enum | Yes | Decision workflow | Human, approved policy, or configured external authority. |
| `qualification_decided_by_actor_id` | UUID or String | Conditional | Decision workflow | Actor responsible for the Decision. |
| `qualification_decision_authority` | Enum | Yes | Governance | Authority category supporting the Decision. |

### Criteria Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `criterion_id` | UUID or String | Yes | Policy authority | Governed qualification criterion. |
| `criterion_version` | String | Yes | Policy authority | Applied criterion version. |
| `criterion_outcome` | Enum | Yes | Deterministic or Human evaluation | Accepted criterion outcome. |
| `criterion_required` | Boolean | Yes | Policy | Whether the criterion was mandatory. |
| `criterion_blocking` | Boolean | Yes | Policy | Whether failure blocks qualification. |
| `evidence_references` | Array | Conditional | Evidence | Supporting or contradicting evidence. |
| `evaluated_at` | Timestamp | Yes | Evaluation workflow | Time the criterion was evaluated. |
| `review_status` | Enum | Yes | Review workflow | Human Review state where applicable. |

### Commercial Intent Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `primary_commercial_intent` | Enum | Yes | Decision snapshot | Primary automotive commercial objective. |
| `purchase_purpose` | Enum | No | Customer evidence or approved interpretation | Purpose of the prospective purchase. |
| `purchase_timeline_category` | Enum | Yes | Customer evidence or approved policy | Accepted purchase-timeframe category. |
| `target_purchase_date_from` | Date | No | Customer evidence | Earliest indicated target date. |
| `target_purchase_date_to` | Date | No | Customer evidence | Latest indicated target date. |
| `urgency_category` | Enum | Yes | Evidence-backed projection | Accepted urgency context. |
| `decision_maker_status` | Enum | Yes | Evidence-backed projection | Customer or representative Decision authority context. |
| `fleet_or_retail_context` | Enum | Yes | Qualification snapshot | Retail, fleet, business, or another approved context. |

### Vehicle Requirement Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `vehicle_requirement_status` | Enum | Yes | Qualification snapshot | Completeness of Vehicle requirements. |
| `vehicle_id` | UUID | No | Vehicle relationship | Specific canonical Vehicle where known. |
| `vehicle_model_reference` | String | No | Vehicle catalogue | Canonical Vehicle Model reference. |
| `vehicle_condition_preference` | Enum | No | Customer evidence | New, used, demo, or other permitted condition. |
| `feature_requirements` | Array | No | Customer evidence | Requested Vehicle features. |
| `quantity_required` | Integer | Yes | Customer evidence or policy default | Number of Vehicles required. |
| `vehicle_requirement_snapshot_hash` | String | Yes | ASOS | Integrity hash of accepted Vehicle requirements. |

### Budget and Payment Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `budget_status` | Enum | Yes | Qualification snapshot | Availability and verification state of budget context. |
| `budget_min_amount` | Decimal | No | Customer evidence | Customer-submitted or accepted minimum budget context. |
| `budget_max_amount` | Decimal | No | Customer evidence | Customer-submitted or accepted maximum budget context. |
| `budget_currency_code` | String | Conditional | Customer evidence | ISO 4217 currency code. |
| `budget_verification_status` | Enum | Yes | Qualification workflow | Whether budget information has been verified, where applicable. |
| `payment_preference` | Enum | Yes | Customer evidence | Indicated payment preference. |
| `finance_interest_status` | Enum | Yes | Customer evidence | General finance interest. |
| `trade_in_interest_status` | Enum | Yes | Customer evidence | General Trade-In interest. |

### Permission Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `contact_permission_status` | Enum | Yes | Customer projection | Current summary of permitted contact. |
| `contact_permission_basis` | Enum | No | Consent authority | Applicable communication basis. |
| `contact_permission_evidence_reference` | String | No | Evidence | Supporting permission evidence. |
| `contact_permission_checked_at` | Timestamp | Yes | Policy workflow | Time permission was last checked. |
| `contact_permission_expires_at` | Timestamp | No | Policy or source | Time the current permission projection expires. |
| `opt_out_projection` | Enum | Yes | Customer projection | Current opt-out projection. |
| `contact_permission_revalidation_required` | Boolean | Yes | Policy | Indicates whether permission must be revalidated. |

### Scoring and Derived Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `qualification_score` | Decimal | No | Derived Intelligence | Analytical qualification score. |
| `qualification_score_scale` | String | Conditional | Model or Schema contract | Scale used by the score. |
| `qualification_score_model_version` | String | Conditional | Model governance | Model version used. |
| `qualification_score_generated_at` | Timestamp | Conditional | Derived workflow | Generation time. |
| `qualification_score_expires_at` | Timestamp | Conditional | Model policy | Score expiration time. |
| `recommended_qualification_outcome` | Enum | No | Derived Intelligence | Non-binding proposed outcome. |
| `conversion_readiness_score` | Decimal | No | Derived Intelligence | Estimated conversion readiness. |
| `requires_human_review` | Boolean | Yes | Deterministic policy or Derived Intelligence | Whether Human Review is required. |

### Freshness and Revalidation Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `freshness_status` | Enum | Yes | Deterministic policy | Current qualification freshness. |
| `freshness_evaluated_at` | Timestamp | Yes | Deterministic policy | Last freshness evaluation time. |
| `revalidation_required` | Boolean | Yes | Policy | Whether qualification requires revalidation. |
| `revalidation_status` | Enum | Yes | Qualified Lead workflow | Current revalidation state. |
| `revalidation_trigger_codes` | Array | No | Evidence or policy | Reasons revalidation is required. |
| `revalidation_outcome` | Enum | No | Decision workflow | Accepted revalidation outcome. |
| `revalidated_at` | Timestamp | No | Qualified Lead workflow | Time revalidation completed. |

### Conversion Fields

| Field | Type | Required | Authority | Description |
| :--- | :--- | :---: | :--- | :--- |
| `conversion_eligibility_status` | Enum | Yes | Qualified Lead workflow | Current eligibility for Opportunity conversion. |
| `opportunity_conversion_request_id` | UUID | No | Conversion workflow | Opportunity-conversion request. |
| `opportunity_conversion_idempotency_key` | String | Conditional | Conversion workflow | Duplicate-prevention key for conversion retry. |
| `opportunity_conversion_request_status` | Enum | Yes | Conversion workflow | Current request state. |
| `opportunity_id` | UUID | No | Opportunity Domain Service | Confirmed Opportunity created from the Qualified Lead. |
| `opportunity_creation_event_id` | UUID | No | Event evidence | Event proving accepted Opportunity creation. |
| `opportunity_conversion_confirmed_at` | Timestamp | No | Conversion workflow | Time Opportunity creation was reconciled. |
| `converted_qualification_version` | Integer | No | Qualified Lead workflow | Qualification version used for conversion. |

---

## 4. Enumerations

### QualifiedLeadStatus

- `ACTIVE`
- `REVALIDATION_REQUIRED`
- `REVALIDATION_PENDING`
- `CONVERSION_PENDING`
- `CONVERTED`
- `REVOKED`
- `EXPIRED`
- `INVALIDATED`
- `SUPERSEDED`
- `ARCHIVED`

### QualificationResult

- `QUALIFIED`

Negative outcomes belong to the Lead and qualification Decision workflows.

### QualificationBasisType

- `AUTHORIZED_HUMAN_DECISION`
- `APPROVED_DETERMINISTIC_POLICY`
- `CONFIGURED_EXTERNAL_AUTHORITY`
- `HYBRID_HUMAN_AND_POLICY`

AI-only qualification is prohibited.

### QualificationDecisionStatus

- `PENDING`
- `APPROVED`
- `APPROVED_WITH_CONDITIONS`
- `REJECTED`
- `EXPIRED`
- `REVOKED`
- `SUPERSEDED`
- `DISPUTED`

A Qualified Lead may be created only from `APPROVED` or policy-permitted `APPROVED_WITH_CONDITIONS`.

### QualificationDecisionAuthority

- `AUTHORITATIVE_HUMAN_DECISION`
- `ASOS_AUTHORITATIVE_WORKFLOW_STATE`
- `EXTERNAL_AUTHORITATIVE_WORKFLOW`
- `APPROVED_AUTOMATION_POLICY`

### QualificationCriterionType

- `IDENTITY`
- `CONTACTABILITY`
- `COMMUNICATION_PERMISSION`
- `AUTOMOTIVE_INTENT`
- `PURCHASE_PURPOSE`
- `PURCHASE_TIMEFRAME`
- `VEHICLE_REQUIREMENT`
- `BUDGET_CONTEXT`
- `PAYMENT_PREFERENCE`
- `FINANCE_INTEREST`
- `TRADE_IN_INTEREST`
- `GEOGRAPHIC_ELIGIBILITY`
- `DEALERSHIP_ELIGIBILITY`
- `REPRESENTATION_AUTHORITY`
- `FRAUD_RISK`
- `COMPLIANCE_RISK`
- `DATA_COMPLETENESS`
- `HUMAN_REVIEW`
- `CUSTOM`

### QualificationCriterionOutcome

- `SATISFIED`
- `SATISFIED_WITH_CONDITIONS`
- `NOT_SATISFIED`
- `UNKNOWN`
- `NOT_APPLICABLE`
- `WAIVED_BY_AUTHORIZED_DECISION`
- `DISPUTED`
- `EXPIRED`

### QualificationEvidenceType

- `LEAD_SOURCE_PAYLOAD`
- `CUSTOMER_IDENTITY`
- `CONTACT_VERIFICATION`
- `CONSENT_OR_PERMISSION`
- `INTERACTION`
- `CUSTOMER_STATEMENT`
- `VEHICLE_REQUIREMENT`
- `BUDGET_STATEMENT`
- `FINANCE_INTEREST`
- `TRADE_IN_INTEREST`
- `APPOINTMENT_INTEREST`
- `DEALERSHIP_ELIGIBILITY`
- `GEOGRAPHIC_EVIDENCE`
- `POLICY_CALCULATION`
- `HUMAN_REVIEW`
- `EXTERNAL_CRM`
- `OTHER`

### EvidenceRole

- `SUPPORTING`
- `CONTRADICTING`
- `CONTEXTUAL`
- `EXCLUDED`
- `SUPERSEDED`

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

### CommercialIntentType

- `NEW_VEHICLE_PURCHASE`
- `USED_VEHICLE_PURCHASE`
- `DEMO_VEHICLE_PURCHASE`
- `FLEET_PURCHASE`
- `REPLACEMENT_PURCHASE`
- `ADDITIONAL_VEHICLE_PURCHASE`
- `LEASE_OR_SUBSCRIPTION`
- `TRADE_IN_WITH_PURCHASE`
- `FINANCE_EXPLORATION_WITH_PURCHASE`
- `OTHER_AUTOMOTIVE_PURCHASE`

### PurchasePurpose

- `PERSONAL_USE`
- `FAMILY_USE`
- `BUSINESS_USE`
- `FLEET_USE`
- `RIDE_HAILING`
- `REPLACEMENT`
- `UPGRADE`
- `GIFT`
- `OTHER`
- `UNKNOWN`

### PurchaseTimelineCategory

- `IMMEDIATE`
- `WITHIN_7_DAYS`
- `WITHIN_30_DAYS`
- `WITHIN_90_DAYS`
- `WITHIN_180_DAYS`
- `MORE_THAN_180_DAYS`
- `DATE_RANGE_PROVIDED`
- `FLEXIBLE`
- `UNKNOWN`

### UrgencyCategory

- `LOW`
- `STANDARD`
- `HIGH`
- `URGENT`
- `UNKNOWN`

Urgency does not override policy, Consent, security, or approval controls.

### DecisionMakerStatus

- `CUSTOMER_IS_DECISION_MAKER`
- `CUSTOMER_IS_JOINT_DECISION_MAKER`
- `AUTHORIZED_REPRESENTATIVE`
- `INFLUENCER_ONLY`
- `DECISION_MAKER_UNKNOWN`
- `AUTHORITY_REVIEW_REQUIRED`

### FleetOrRetailContext

- `RETAIL_INDIVIDUAL`
- `RETAIL_ORGANIZATION`
- `SMALL_FLEET`
- `LARGE_FLEET`
- `GOVERNMENT_OR_PUBLIC_ENTITY`
- `PARTNER_OR_RESELLER`
- `UNKNOWN`

### VehicleRequirementStatus

- `NOT_PROVIDED`
- `PARTIAL`
- `SUFFICIENT_FOR_DISCOVERY`
- `SPECIFIC_MODEL_IDENTIFIED`
- `SPECIFIC_CONFIGURATION_IDENTIFIED`
- `MULTIPLE_OPTIONS_ACCEPTABLE`
- `REVALIDATION_REQUIRED`
- `CONFLICTED`

### VehicleConditionPreference

- `NEW`
- `USED`
- `DEMO`
- `CERTIFIED_USED`
- `ANY_ACCEPTABLE`
- `UNKNOWN`

### BudgetStatus

- `NOT_REQUIRED`
- `NOT_PROVIDED`
- `PARTIAL`
- `CUSTOMER_SUBMITTED`
- `RANGE_AVAILABLE`
- `VERIFICATION_PENDING`
- `VERIFIED_WHERE_REQUIRED`
- `CONFLICTED`
- `RESTRICTED`
- `EXPIRED`

### BudgetVerificationStatus

- `NOT_REQUIRED`
- `NOT_VERIFIED`
- `PENDING`
- `PARTIALLY_VERIFIED`
- `VERIFIED`
- `FAILED`
- `DISPUTED`

### PaymentPreference

- `CASH`
- `FINANCE`
- `LEASE`
- `MIXED`
- `UNDECIDED`
- `OTHER`
- `UNKNOWN`

### FinanceInterestStatus

- `NOT_EXPRESSED`
- `POSSIBLE`
- `EXPRESSED`
- `INFORMATION_REQUESTED`
- `PREQUALIFICATION_INTEREST`
- `DECLINED`
- `UNKNOWN`

### TradeInInterestStatus

- `NOT_EXPRESSED`
- `POSSIBLE`
- `EXPRESSED`
- `VEHICLE_DETAILS_PENDING`
- `APPRAISAL_REQUESTED`
- `DECLINED`
- `UNKNOWN`

`APPRAISAL_REQUESTED` is a Customer-interest projection and not a Trade-In appraisal outcome.

### AppointmentInterestStatus

- `NOT_EXPRESSED`
- `POSSIBLE`
- `EXPRESSED`
- `TIME_PREFERENCE_PROVIDED`
- `REQUEST_READY`
- `DECLINED`
- `UNKNOWN`

### TestDriveInterestStatus

- `NOT_EXPRESSED`
- `POSSIBLE`
- `EXPRESSED`
- `VEHICLE_PENDING`
- `REQUEST_READY`
- `DECLINED`
- `UNKNOWN`

### ContactPermissionStatus

- `NOT_EVALUATED`
- `PERMITTED`
- `PERMITTED_WITH_RESTRICTIONS`
- `TRANSACTIONAL_RESPONSE_ONLY`
- `NOT_PERMITTED`
- `OPTED_OUT`
- `EXPIRED`
- `REVALIDATION_REQUIRED`
- `DISPUTED`

### OptOutProjection

- `NO_ACTIVE_OPT_OUT_OBSERVED`
- `POTENTIAL_OPT_OUT`
- `ACTIVE_OPT_OUT`
- `PARTIAL_CHANNEL_OPT_OUT`
- `PARTIAL_PURPOSE_OPT_OUT`
- `DISPUTED`
- `UNKNOWN`

### PriorityClassification

- `LOW`
- `STANDARD`
- `HIGH`
- `URGENT`
- `CRITICAL_REVIEW`

### RoutingStatus

- `NOT_EVALUATED`
- `RECOMMENDATION_PENDING`
- `RECOMMENDED`
- `DECISION_PENDING`
- `ROUTING_CONTEXT_ACCEPTED`
- `CONFLICTED`
- `EXPIRED`

### QualificationFreshnessStatus

- `CURRENT`
- `APPROACHING_EXPIRY`
- `REVALIDATION_REQUIRED`
- `STALE`
- `EXPIRED`
- `DISPUTED`
- `UNKNOWN`

### RevalidationStatus

- `NOT_REQUIRED`
- `REQUIRED`
- `REQUESTED`
- `IN_PROGRESS`
- `DECISION_PENDING`
- `COMPLETED_CURRENT`
- `COMPLETED_NEW_VERSION_REQUIRED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`

### RevalidationTrigger

- `VALIDITY_PERIOD_ENDING`
- `VALIDITY_PERIOD_ENDED`
- `CUSTOMER_IDENTITY_CHANGED`
- `CUSTOMER_IDENTITY_DISPUTED`
- `CONTACT_PERMISSION_CHANGED`
- `CUSTOMER_OPTED_OUT`
- `PURCHASE_TIMEFRAME_CHANGED`
- `VEHICLE_REQUIREMENT_CHANGED`
- `BUDGET_CHANGED`
- `PAYMENT_PREFERENCE_CHANGED`
- `FINANCE_CONTEXT_CHANGED`
- `TRADE_IN_CONTEXT_CHANGED`
- `GEOGRAPHIC_CONTEXT_CHANGED`
- `POLICY_CHANGED`
- `EVIDENCE_EXPIRED`
- `EVIDENCE_CORRECTED`
- `FRAUD_RISK_CHANGED`
- `COMPLIANCE_RISK_CHANGED`
- `CUSTOMER_WITHDRAWAL`
- `MANUAL_REVIEW_REQUIRED`
- `OTHER`

### RevalidationOutcome

- `REMAINS_CURRENT`
- `NEW_QUALIFICATION_VERSION_CREATED`
- `REVOKED`
- `EXPIRED`
- `INVALIDATED`
- `SUPERSEDED`
- `INSUFFICIENT_EVIDENCE`
- `HUMAN_REVIEW_REQUIRED`

### RevocationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `REVIEW_PENDING`
- `APPROVED`
- `REJECTED`
- `APPLIED`
- `FAILED`
- `DISPUTED`

### InvalidationStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `REVIEW_PENDING`
- `APPROVED`
- `REJECTED`
- `APPLIED`
- `FAILED`
- `RECONCILIATION_REQUIRED`
- `DISPUTED`

### ConversionEligibilityStatus

- `NOT_EVALUATED`
- `ELIGIBLE`
- `ELIGIBLE_WITH_CONDITIONS`
- `TEMPORARILY_BLOCKED`
- `NOT_ELIGIBLE`
- `REVALIDATION_REQUIRED`
- `ALREADY_CONVERTED`
- `EXPIRED`
- `REVOKED`
- `INVALIDATED`
- `CONFLICTED`

### OpportunityConversionRequestStatus

- `NOT_REQUESTED`
- `REQUESTED`
- `VALIDATION_PENDING`
- `ACCEPTED`
- `PENDING_OPPORTUNITY_CONFIRMATION`
- `CONFIRMED`
- `REJECTED`
- `FAILED`
- `EXPIRED`
- `CANCELLED`
- `RECONCILIATION_REQUIRED`

### ConversionConfirmationStatus

- `NOT_REQUIRED`
- `NOT_RECEIVED`
- `PENDING`
- `RECEIVED`
- `REJECTED`
- `FAILED`
- `RECONCILIATION_REQUIRED`

### ReviewStatus

- `NOT_REQUIRED`
- `PENDING`
- `IN_REVIEW`
- `APPROVED`
- `APPROVED_WITH_CONDITIONS`
- `REJECTED`
- `REVISION_REQUIRED`
- `ESCALATED`
- `CANCELLED`
- `EXPIRED`

### ReviewType

- `IDENTITY`
- `QUALIFICATION_EXCEPTION`
- `EVIDENCE_CONFLICT`
- `CONTACT_PERMISSION`
- `REPRESENTATION_AUTHORITY`
- `FRAUD`
- `COMPLIANCE`
- `DATA_QUALITY`
- `CONVERSION_EXCEPTION`
- `REVOCATION`
- `INVALIDATION`
- `CORRECTION`
- `OTHER`

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
- Lead, Customer, Opportunity, Interaction, dealership, branch, team, queue, User, and policy references must belong to the authorized Tenant.
- Cross-Tenant qualification, Customer matching, evidence access, conversion, analytics, search, AI retrieval, and export are prohibited unless governed by an approved mechanism.
- Background Jobs, Event Consumers, integrations, and AI Agents must receive trusted Tenant execution context.
- A source or external CRM must not choose an unrestricted `tenant_id`.

### Positive Qualification Creation Rules

A Qualified Lead may be created only when:

- One originating Lead exists.
- The Lead belongs to the same Tenant.
- The Lead references one resolved Customer.
- The Customer belongs to the same Tenant.
- A positive qualification request was completed.
- An accepted positive qualification Decision exists.
- The Decision result is `QUALIFIED`.
- The Decision has applicable authority.
- The qualification policy and version are known.
- Required criteria have accepted outcomes.
- Blocking criteria are satisfied or validly waived.
- Required evidence exists.
- Evidence integrity requirements pass.
- Required Human Review is completed.
- No blocking fraud, compliance, identity, or permission issue exists.
- Qualification validity can be determined.
- Snapshot hashes can be generated.
- The creation request is idempotent.
- No duplicate current Qualified Lead exists for the same cycle.

### No Negative Qualified Lead Rule

A Qualified Lead must not be created when the accepted outcome is:

- `DISQUALIFIED`.
- `INVALID`.
- `DUPLICATE`.
- `SPAM`.
- `FRAUDULENT`.
- `WITHDRAWN`.
- `EXPIRED_WITHOUT_POSITIVE_QUALIFICATION`.
- `INSUFFICIENT_EVIDENCE`.
- `REVIEW_PENDING`.
- `REJECTED`.

These outcomes remain in the Lead or qualification Decision workflow.

### Lead Ownership Rules

The Qualified Lead must not take ownership of:

- Lead source ingestion.
- Lead source deduplication.
- Original submitted party information.
- Negative qualification outcomes.
- Lead invalidation.
- Lead duplicate status.
- Lead withdrawal before qualification.
- Lead pending qualification state.
- Lead response-SLA workflow.

The Qualified Lead may preserve immutable or versioned references to these facts.

### Origin Integrity Rules

- `lead_id` is required and immutable.
- `customer_id` is required at creation.
- `lead_record_version` is required.
- `qualification_request_id` is required.
- `qualification_decision_id` is required.
- The Lead and Qualified Lead must reference the same Customer at creation.
- The Lead must not be a duplicate, invalid, prohibited, or withdrawn record at creation.
- The original Lead evidence must remain unchanged.
- Customer identity corrections require a governed workflow.
- A Customer correction must not silently rewrite historical qualification snapshots.
- Circular origin or supersession relationships are prohibited.

### Qualification Cycle Rules

- `qualification_cycle_id` is required.
- Only one current Qualified Lead aggregate may exist for the same Lead and active qualification cycle.
- Material revalidation may create a new `qualification_version`.
- Historical versions must remain immutable.
- A new commercial intent may require a new qualification cycle.
- A new cycle must not be used to conceal revocation, invalidation, or a prior Opportunity.
- Duplicate cycles must be detected.
- Opportunity creation must reference the exact qualification version used.

### Qualification Decision Rules

The positive Decision must preserve:

- Decision identifier.
- Decision authority.
- Decision status.
- Decision timestamp.
- Decision reasons.
- Evidence.
- Policy and version.
- Required review.
- Conditions.
- Expiration.
- Actor.
- Audit history.

An AI Recommendation alone cannot satisfy the Decision requirement.

An AI Agent must not approve its own Recommendation.

### Deterministic Policy Decision Rules

An approved deterministic policy may establish a positive qualification Decision only when:

- The Constitution permits the applicable Action Class.
- The policy is approved and active.
- All required inputs are authoritative or appropriately governed.
- Criteria are deterministic.
- No criterion requires Human judgment.
- No blocking conflict exists.
- No identity ambiguity exists.
- No fraud or compliance escalation exists.
- No exception is applied.
- The result is auditable.
- The policy and version are preserved.
- Human Review remains available.

### Human Decision Rules

Human qualification approval requires:

- Authenticated Human identity.
- Authorized role.
- Tenant and organizational scope.
- Evidence access.
- Decision reason.
- Policy context.
- Review of material conflicts.
- Decision timestamp.
- Decision record.
- Expiration where applicable.

A Human Decision must not override legal, security, privacy, or Tenant-isolation controls.

### Evidence Rules

Qualification evidence must:

- Be traceable.
- Preserve source authority.
- Preserve source record version.
- Preserve occurrence or observation time.
- Preserve evidence hash where applicable.
- Preserve security classification.
- Distinguish supporting and contradicting evidence.
- Distinguish original evidence and normalized values.
- Preserve excluded evidence and exclusion reasons.
- Preserve freshness.
- Remain accessible according to retention and legal-hold policy.

Evidence with failed integrity must not support qualification.

### Criteria Rules

- Every required criterion must have one accepted result.
- Each result must reference the applicable criterion and version.
- Blocking criteria must be satisfied or validly waived.
- Waiver requires authorized Decision.
- Unknown criteria must remain unknown.
- Missing values must not be replaced with invented defaults.
- Criteria thresholds must remain configurable.
- Criteria must not rely on prohibited personal attributes.
- Criteria changes must not silently alter an existing snapshot.
- Material policy changes require revalidation.

### Customer Identity Rules

- Qualified Lead creation requires a resolved Customer.
- Identity resolution must be governed by Customer Domain Service.
- AI confidence alone must not resolve Customer identity.
- Submitted contact values must not be treated as permanent Customer identity.
- Representative authority must be validated where material.
- Identity conflict blocks conversion according to policy.
- Identity correction after conversion requires downstream review.

### Contact Permission Rules

At qualification time:

- Current contact-permission projection must be captured.
- Active opt-out must be preserved.
- Channel restrictions must be preserved.
- Purpose restrictions must be preserved.
- Permission evidence must be referenced where required.

Before outbound communication:

- Current permission must be re-evaluated.
- Quiet hours must be checked.
- Frequency restrictions must be checked.
- Current opt-out must be checked.
- Channel validity must be checked.

Qualification does not create permission.

### Commercial Intent Rules

- Primary commercial intent is required.
- Intent must concern an approved automotive sales journey.
- General information without sufficient commercial intent must not be forced into positive qualification.
- Intent evidence must be preserved.
- Conditional Customer language must remain conditional.
- AI classification must not be represented as Customer commitment.
- Intent changes after conversion belong to Opportunity requirement management.
- Material intent changes before conversion require revalidation.

### Vehicle Requirement Rules

- Vehicle requirement may be broad if policy allows discovery during Opportunity.
- Specific Vehicle identity is not required unless policy requires it.
- Vehicle interest must not be represented as availability.
- Vehicle Model normalization must preserve the submitted text.
- AI Vehicle matching remains a Recommendation until governed acceptance.
- Material changes require revalidation or a new qualification version.

### Budget Rules

- Budget requirements remain policy-configurable.
- Monetary amounts must use fixed decimal precision.
- Currency must use ISO 4217.
- Customer-submitted budget must remain labelled as submitted.
- Budget must not be represented as verified financial capacity without evidence.
- Sensitive finance documents must not be stored in unrestricted Qualified Lead fields.
- Missing optional budget data must not automatically disqualify a Lead unless policy requires it.
- Budget changes may require revalidation.

### Finance Rules

The Qualified Lead may preserve finance interest only.

It must not:

- Create a Finance Application.
- Submit data to a Lender.
- Record a Lender Decision.
- Store unrestricted credit data.
- Approve finance.
- Promise a rate.
- Promise a periodic payment.
- Request funding.
- Confirm funding.

Finance progression begins through the appropriate Opportunity and Finance Application workflows.

### Trade-In Rules

The Qualified Lead may preserve Trade-In interest only.

It must not:

- Create an appraisal.
- Approve a value.
- Confirm ownership.
- Confirm payoff.
- Acquire the Vehicle.
- Create an Inventory Record.

Trade-In progression begins through the appropriate Opportunity and Trade-In workflows.

### Appointment Rules

Appointment and test-drive interest do not create an Appointment.

A separate Appointment workflow must:

- Validate Customer.
- Validate dealership and branch.
- Validate resource availability.
- Validate Vehicle availability where required.
- Create the Appointment.
- Confirm external scheduling where applicable.

### Routing Rules

- Routing criteria must be versioned.
- Routing Recommendations must not create sales ownership.
- Recommended dealership, branch, team, or owner must belong to the same Tenant.
- Geographic routing must use approved data.
- Restricted Customer information must not be unnecessarily exposed.
- AI may recommend routing.
- Authoritative Opportunity assignment begins after conversion.
- External CRM routing Commands remain pending until Confirmation.

### Scoring Rules

- Scores must use an approved scale.
- Model or formula version is required.
- Input records and versions are required.
- Score timestamp is required.
- Expiration is required where applicable.
- Missing scores must not be replaced with invented values.
- Score thresholds must remain configurable.
- A score must not override evidence.
- A score must not override Human Review.
- A score must not create a Qualified Lead independently.

### Freshness Rules

- `expires_at` is required.
- Freshness must be evaluated deterministically.
- Expired qualification must not be converted.
- Stale qualification must not be treated as current.
- Approaching expiry may trigger revalidation.
- Freshness policy and version must be preserved.
- Manual extension requires authorized Decision.
- Validity extension must not rewrite the original qualification timestamp.

### Revalidation Rules

Revalidation is required when configured triggers occur.

Revalidation must:

- Reference the current qualification version.
- Preserve previous evidence.
- Collect new evidence where needed.
- Re-evaluate applicable criteria.
- Preserve policy and version.
- Preserve Decision and authority.
- Produce an explicit outcome.
- Update freshness.
- Create a new qualification version when material.
- Prevent conversion while blocking revalidation is unresolved.

### Conversion Eligibility Rules

A Qualified Lead is eligible for conversion only when:

- Status is `ACTIVE`.
- Qualification is current.
- Qualification is not expired.
- Qualification is not revoked.
- Qualification is not invalidated.
- Required Customer identity remains valid.
- Contact restrictions are understood.
- Required evidence remains current.
- No blocking conflict exists.
- Required organizational routing is valid.
- Required Human Review is complete.
- No primary Opportunity already exists.
- Conversion request uses idempotency.
- Exact qualification version is identified.

### Opportunity-Conversion Rules

The Qualified Lead Domain Service may accept and track an Opportunity-conversion request.

The Opportunity Domain Service owns Opportunity creation.

The conversion workflow must:

- Validate Qualified Lead eligibility.
- Preserve `qualified_lead_id`.
- Preserve `lead_id`.
- Preserve `customer_id`.
- Preserve qualification version.
- Preserve snapshot hashes.
- Preserve conversion authority.
- Preserve requested organizational context.
- Use an idempotency key.
- Prevent duplicate Opportunity creation.
- Remain pending until Opportunity creation is accepted.
- Store the resulting Opportunity reference.
- Record failure and reconciliation.

The Qualified Lead must not enter `CONVERTED` only because a request was sent.

### One Primary Opportunity Rule

One Qualified Lead must not create more than one primary Opportunity.

A retry must return or reconcile the existing Opportunity.

Reopening a sales pursuit should normally use the existing Opportunity according to Opportunity rules.

A materially different new commercial intent requires a new qualification cycle.

### Post-Conversion Invalidity Rules

If a Qualified Lead is revoked or invalidated after conversion:

- The Qualified Lead history remains.
- The Opportunity must be notified.
- Opportunity review or hold may be required.
- The Opportunity must not be silently deleted.
- Existing Customer communication must be evaluated.
- Existing Quotations, Appointments, Trade-In, Finance, and Deal workflows may require review.
- Automatic cancellation is prohibited unless an approved deterministic rule explicitly permits it.
- High-impact decisions require Human authority.

### Correction Rules

A material correction must preserve:

- Original version.
- Corrected version.
- Reason.
- Actor.
- Authority.
- Evidence.
- Previous and new hashes.
- Affected downstream records.
- Timestamp.
- Related Events.

Published or accepted historical snapshots must not be edited in place.

### Supersession Rules

- A superseding version must belong to the same qualification cycle.
- It must reference the superseded version.
- Only one current version may exist.
- Supersession must be atomic.
- The old version remains readable.
- Opportunity conversion must reference the exact version used.
- Circular supersession is prohibited.

### Concurrency and Idempotency Rules

- Every mutation must validate `record_version`.
- Stale updates must return a version conflict.
- Qualified Lead creation must support idempotency.
- Qualification-version creation must support idempotency.
- Revalidation requests must support idempotency.
- Opportunity-conversion requests must support idempotency.
- Revocation and invalidation requests must support idempotency.
- Retryable Commands must use `idempotency_key`.
- Event Consumers must prevent duplicate effects using `event_id`.
- Duplicate retries must not create duplicate:
  - Qualified Leads.
  - Qualification versions.
  - Revalidation requests.
  - Human Review requests.
  - Opportunity-conversion requests.
  - Opportunities.
  - Revocation records.
  - Invalidation records.
  - Reconciliation cases.

### AI Failure Rules

- AI processing failure must not delete qualification evidence.
- AI processing failure must remain explicit.
- Missing AI output must not be replaced with fabricated values.
- Deterministic and Human workflows must remain available.
- Qualified Lead creation must not depend solely on successful AI processing.
- AI failure must not silently produce a negative qualification outcome.
- AI failure may trigger Human Review.

### Human Review Requirements

Human Review is required according to policy for:

- Ambiguous Customer identity.
- Conflicting evidence.
- Representative authority uncertainty.
- Contact-permission conflict.
- High fraud risk.
- Compliance risk.
- Material policy exception.
- Waiver of a blocking criterion.
- Low evidence quality.
- Sensitive-data issue.
- AI and deterministic-rule disagreement.
- Conversion exception.
- Revocation.
- Invalidation.
- Post-conversion qualification defect.
- Another material legal, commercial, privacy, or security risk.

---

## 6. State Machine

### Allowed States

```text
ACTIVE
REVALIDATION_REQUIRED
REVALIDATION_PENDING
CONVERSION_PENDING
CONVERTED
REVOKED
EXPIRED
INVALIDATED
SUPERSEDED
ARCHIVED
```

A Qualified Lead begins in `ACTIVE` only after a positive qualification Decision and creation transaction are accepted.

### Principal Allowed Transitions

```text
ACTIVE → REVALIDATION_REQUIRED
ACTIVE → CONVERSION_PENDING
ACTIVE → REVOKED
ACTIVE → EXPIRED
ACTIVE → INVALIDATED
ACTIVE → SUPERSEDED
ACTIVE → ARCHIVED

REVALIDATION_REQUIRED → REVALIDATION_PENDING
REVALIDATION_REQUIRED → REVOKED
REVALIDATION_REQUIRED → EXPIRED
REVALIDATION_REQUIRED → INVALIDATED
REVALIDATION_REQUIRED → SUPERSEDED

REVALIDATION_PENDING → ACTIVE
REVALIDATION_PENDING → REVALIDATION_REQUIRED
REVALIDATION_PENDING → REVOKED
REVALIDATION_PENDING → EXPIRED
REVALIDATION_PENDING → INVALIDATED
REVALIDATION_PENDING → SUPERSEDED

CONVERSION_PENDING → CONVERTED
CONVERSION_PENDING → ACTIVE
CONVERSION_PENDING → REVALIDATION_REQUIRED
CONVERSION_PENDING → REVOKED
CONVERSION_PENDING → EXPIRED
CONVERSION_PENDING → INVALIDATED

CONVERTED → INVALIDATED
CONVERTED → SUPERSEDED
CONVERTED → ARCHIVED

REVOKED → ARCHIVED
EXPIRED → ARCHIVED
INVALIDATED → ARCHIVED
SUPERSEDED → ARCHIVED
```

### Forbidden Ordinary Transitions

```text
ACTIVE → ACTIVE_BY_UNCONTROLLED_UPDATE

REVALIDATION_REQUIRED → CONVERTED
REVALIDATION_PENDING → CONVERTED

CONVERSION_PENDING → CONVERTED_WITHOUT_OPPORTUNITY

EXPIRED → ACTIVE
REVOKED → ACTIVE
INVALIDATED → ACTIVE
SUPERSEDED → ACTIVE

CONVERTED → ACTIVE
CONVERTED → CONVERSION_PENDING

ARCHIVED → ACTIVE
ARCHIVED → CONVERSION_PENDING
ARCHIVED → CONVERTED
```

A new valid qualification after expiration, revocation, invalidation, or material change requires an approved revalidation version or a new qualification cycle.

### Entering ACTIVE

Initial entry requires:

- Valid Tenant context.
- One valid Lead.
- One resolved Customer.
- Accepted positive qualification Decision.
- Approved policy and version.
- Required criteria satisfied.
- Required evidence package.
- Required Human Review completed.
- No blocking conflict.
- Valid effective and expiration times.
- Snapshot hashes.
- Idempotent creation.
- Initial audit evidence.

Re-entry from revalidation requires:

- Accepted revalidation outcome.
- Current evidence.
- Current policy.
- Updated validity.
- New version where material.
- Preserved prior snapshot.

### Entering REVALIDATION_REQUIRED

Requires:

- Valid trigger.
- Trigger evidence.
- Trigger timestamp.
- Current qualification version.
- Revalidation policy.
- Conversion eligibility update.
- Blocking or non-blocking classification.
- Audit evidence.

When blocking, Opportunity conversion must be prevented.

### Entering REVALIDATION_PENDING

Requires:

- Revalidation request.
- Assigned workflow or reviewer.
- Evidence requirements.
- Due time where applicable.
- Idempotency.
- Current record version.
- Audit evidence.

### Entering CONVERSION_PENDING

Requires:

- Status `ACTIVE`.
- Current qualification.
- Valid Customer.
- Eligible conversion status.
- No primary Opportunity.
- Exact qualification version.
- Frozen conversion snapshot.
- Valid organizational context.
- Opportunity-conversion request.
- Idempotency key.
- Audit evidence.

### Entering CONVERTED

Requires:

- Accepted Opportunity creation.
- Valid `opportunity_id`.
- Opportunity record version.
- Opportunity creation Event reference.
- Confirmed matching Tenant.
- Confirmed matching Qualified Lead.
- Confirmed matching Lead and Customer.
- Qualification version used.
- Conversion timestamp.
- Reconciliation completed.

Conversion does not prove a Deal or sale.

### Entering REVOKED

Requires:

- Revocation request or deterministic trigger.
- Authorized Decision where required.
- Revocation reason.
- Evidence.
- Current version.
- Conversion and downstream impact evaluation.
- Timestamp.
- Audit evidence.

### Entering EXPIRED

Requires:

- Deterministic expiration.
- Applicable policy and version.
- Expiration reason.
- Expiration timestamp.
- Conversion eligibility removal.
- Audit evidence.

### Entering INVALIDATED

Requires:

- Material defect.
- Invalidation request.
- Authorized Decision.
- Evidence.
- Affected versions.
- Affected Opportunity where applicable.
- Downstream reconciliation.
- Audit evidence.

### Entering SUPERSEDED

Requires:

- Valid replacement qualification version.
- Same qualification cycle.
- Supersession reason.
- Replacement reference.
- Atomic current-version update.
- Preserved historical evidence.
- Audit evidence.

### Entering ARCHIVED

Requires:

- Valid retention or closure condition.
- No active pending workflow requiring the record.
- Preservation of required audit and legal evidence.
- Access and retention classification.
- Archive timestamp.

### Terminal States

For ordinary progression:

- `REVOKED`
- `EXPIRED`
- `INVALIDATED`
- `SUPERSEDED`
- `ARCHIVED`

`CONVERTED` is terminal for ordinary conversion activity, but may receive governed invalidation, supersession, or archival.

### Transition Evidence

Every material transition must preserve:

- Previous state.
- New state.
- Reason.
- Actor.
- Authority.
- Qualified Lead identifier.
- Qualification cycle.
- Qualification version.
- Lead and Customer references.
- Record version.
- Evidence.
- Policy and version.
- Decision.
- Human Review.
- Recommendation where applicable.
- Idempotency key where applicable.
- Opportunity reference where applicable.
- Correlation identifier.
- Causation identifier.
- Timestamp.
- Related Event.

---

## 7. Relationships

### Tenant

- Every Qualified Lead belongs to exactly one `tenant_id`.
- Every Tenant-owned relationship must match that Tenant.
- Cross-Tenant qualification or conversion is prohibited by default.

### Lead

- Every Qualified Lead originates from exactly one Lead.
- Lead remains authoritative for original inquiry and intake history.
- Lead owns the qualification request and pending workflow.
- Lead owns negative qualification outcomes.
- Qualified Lead owns the accepted positive result.
- The Lead must retain the `qualified_lead_id` relationship after successful creation.
- Lead data must not be deleted because qualification succeeded.

### Customer

- Every Qualified Lead references exactly one resolved Customer.
- Customer Domain Service owns identity and Consent.
- Qualified Lead stores a time-bounded Customer snapshot.
- Customer corrections must preserve historical qualification context.
- Customer opt-out may trigger revalidation or conversion blocking.

### Opportunity

- A Qualified Lead may create no more than one primary Opportunity.
- Opportunity Domain Service owns Opportunity creation.
- Conversion must be idempotent.
- Opportunity preserves the qualification snapshot and version.
- Qualified Lead invalidation after conversion triggers review, not silent deletion.
- Opportunity requirement changes must not rewrite the Qualified Lead.

### Interaction

Interactions may provide evidence for:

- Commercial intent.
- Purchase timeframe.
- Vehicle requirements.
- Budget statement.
- Finance interest.
- Trade-In interest.
- Appointment interest.
- Customer withdrawal.
- Contact permission.
- Revalidation trigger.

Interaction owns communication evidence.

Qualified Lead stores references and accepted interpretations.

### Vehicle

Qualified Lead may reference:

- Specific Vehicle.
- Vehicle Model.
- Vehicle configuration.
- Broad Vehicle requirements.

Vehicle Domain Service owns canonical Vehicle identity.

Qualified Lead does not own Vehicle availability.

### Inventory Record

Qualified Lead may use an authorized availability projection for routing or readiness.

Inventory Record owns:

- Availability.
- Reservation.
- Allocation.
- Location.
- Sale state.
- Delivery projection.

Qualified Lead must not reserve or allocate Inventory.

### Appointment

Qualified Lead may preserve Appointment or test-drive interest.

Appointment Domain Service owns scheduling and confirmation.

### Quotation

A Qualified Lead does not own Customer-specific commercial terms.

Quotation is normally created during Opportunity progression.

### Trade-In

Qualified Lead may preserve Trade-In interest.

Trade-In Domain Service owns:

- Vehicle identification.
- Inspection.
- Appraisal.
- Offer.
- Payoff.
- Ownership transfer.
- Acquisition.

### Finance Application

Qualified Lead may preserve finance interest.

Finance Application Domain Service owns:

- Applicant workflow.
- Finance Consent.
- Lender submissions.
- Lender Decisions.
- Selected offer.
- Contracting handoff.

### Deal

Qualified Lead does not create or own a Deal.

Deal creation follows Opportunity and applicable commercial commitment workflows.

### Human Decision

Qualified Lead may reference Human Decisions for:

- Positive qualification.
- Criterion waiver.
- Policy exception.
- Revalidation.
- Revocation.
- Invalidation.
- Conversion exception.
- Correction.
- Supersession.

### Recommendation

Recommendations may propose:

- Qualification outcome.
- Missing information.
- Priority.
- Routing.
- Revalidation.
- Conversion readiness.
- Human Review.

Recommendations remain non-binding.

### AI Agent Run

AI Agent Runs may reference:

- Lead.
- Customer.
- Qualification evidence.
- Qualified Lead version.
- Model.
- Prompt.
- Output.
- Recommendation.
- Review.

AI Agent output remains Derived Intelligence.

### External CRM

An external CRM may be authoritative for configured operational status.

ASOS must preserve:

- External reference.
- External record version.
- Command.
- Idempotency.
- External Confirmation.
- Conflict.
- Reconciliation.

### Supporting Child Records

Qualified Lead may own or govern:

- Qualification versions.
- Policy snapshots.
- Criteria results.
- Evidence packages.
- Commercial-intent snapshots.
- Vehicle-requirement snapshots.
- Budget snapshots.
- Contact-permission projections.
- Review records.
- Revalidation records.
- Conversion requests.
- Revocation records.
- Invalidation records.
- Supersession records.
- Derived Intelligence.
- Data-quality issues.
- Conflict records.
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

The following are required Qualified Lead Event concepts and do not replace the Event Catalog.

### Qualification Acceptance Event Concepts

- Positive qualification Decision accepted.
- Qualified Lead creation requested.
- Qualified Lead created.
- Qualified Lead creation rejected.
- Qualified Lead activated.
- Qualification version created.
- Qualification version superseded.

### Freshness Event Concepts

- Qualified Lead approaching expiry.
- Qualified Lead freshness evaluated.
- Qualified Lead became stale.
- Qualified Lead expired.
- Qualification validity extended.

### Revalidation Event Concepts

- Qualified Lead revalidation required.
- Qualified Lead revalidation requested.
- Qualified Lead revalidation started.
- Qualified Lead revalidation Decision recorded.
- Qualified Lead revalidation completed.
- New qualification version created.
- Qualified Lead revalidation failed.
- Qualified Lead Human Review requested.

### Conversion Event Concepts

- Qualified Lead conversion eligibility evaluated.
- Qualified Lead became eligible for conversion.
- Qualified Lead conversion blocked.
- Opportunity conversion requested.
- Opportunity conversion request accepted.
- Opportunity conversion request rejected.
- Opportunity conversion pending Confirmation.
- Opportunity conversion confirmed.
- Opportunity conversion reconciliation required.
- Qualified Lead converted.

Qualified Lead Domain Service must not publish the authoritative Opportunity-created Event.

Opportunity Domain Service publishes accepted Opportunity creation.

### Revocation and Invalidation Event Concepts

- Qualified Lead revocation requested.
- Qualified Lead revoked.
- Qualified Lead invalidation requested.
- Qualified Lead invalidated.
- Post-conversion qualification review requested.
- Downstream reconciliation required.

### Evidence and Data-Quality Event Concepts

- Qualification evidence conflict detected.
- Qualification evidence became stale.
- Qualification evidence corrected.
- Qualification data-quality issue opened.
- Qualification data-quality issue resolved.
- Qualified Lead quarantined.

### AI and Recommendation Event Concepts

- Qualification score generated.
- Qualification Recommendation generated.
- Missing qualification information detected.
- Routing Recommendation generated.
- Revalidation Recommendation generated.
- Conversion-readiness assessment generated.
- Qualified Lead Human Review recommended.

Derived Intelligence Events must not imply:

- Positive qualification Decision.
- Customer identity resolution.
- Communication permission.
- Opportunity creation.
- Appointment confirmation.
- Quotation issuance.
- Finance approval.
- Trade-In appraisal.
- Deal creation.
- Human Approval.

### Producer Rules

- Lead Domain Service publishes accepted Lead intake, qualification-request, evidence-readiness, and negative-outcome facts.
- Qualified Lead Domain Service publishes accepted positive qualification, validity, revalidation, revocation, invalidation, and conversion-eligibility facts.
- Customer Domain Service publishes accepted Customer identity and permission facts.
- Opportunity Domain Service publishes accepted Opportunity creation and lifecycle facts.
- Interaction Domain Service publishes accepted communication facts.
- Policy and Decision services publish applicable policy and Decision facts.
- AI Agents may publish scoring, extraction, classification, Recommendation, and Agent-run Events.
- AI Agents must not publish an authoritative positive qualification Event merely because they predicted qualification.

### Event Requirements

Every material Qualified Lead Event must preserve, where applicable:

- `event_id`.
- `event_type`.
- `event_version`.
- `tenant_id`.
- `qualified_lead_id`.
- `qualification_cycle_id`.
- Qualification version.
- Lead identifier and version.
- Customer identifier.
- Opportunity identifier where applicable.
- Occurrence timestamp.
- Recording timestamp.
- Producer.
- Actor.
- Authority category.
- Record version.
- Policy and version.
- Decision.
- Human Review.
- Snapshot hashes.
- Evidence references.
- Recommendation where applicable.
- Idempotency key where applicable.
- Correlation identifier.
- Causation identifier.
- Security classification.

Events are immutable.

Corrections, revocations, invalidations, expirations, supersessions, and reconciliation outcomes require new linked Events.

The Event Backbone may deliver the same Event more than once.

Consumers must prevent duplicate effects using `event_id`.

Command and request retries must use the applicable `idempotency_key`.

---

## 9. AI Considerations

### Permitted AI Assistance

AI Agents may assist with:

- Automotive-intent classification.
- Vehicle-requirement extraction.
- Purchase-timeframe extraction.
- Budget-range extraction.
- Payment-preference extraction.
- Finance-interest detection.
- Trade-In-interest detection.
- Appointment-interest detection.
- Missing-information detection.
- Evidence summarization.
- Qualification scoring.
- Fraud-risk detection.
- Spam-risk detection.
- Compliance-risk detection.
- Customer-contactability assessment.
- Priority Recommendation.
- Routing Recommendation.
- Revalidation-trigger detection.
- Conversion-readiness assessment.
- Human Review preparation.
- Management-summary generation.

### Prohibited Independent AI Actions

AI Agents must not independently:

- Resolve ambiguous Customer identity.
- Create Customer Consent.
- Ignore an opt-out.
- Create an authoritative positive qualification Decision.
- Create a Qualified Lead from their own score.
- Waive a blocking criterion.
- Override Human Review.
- Extend qualification validity.
- Revoke or invalidate a Qualified Lead.
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
- Lead identifier and version.
- Qualified Lead identifier and version where applicable.
- Customer reference.
- Input fields.
- Supporting evidence.
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
- Canonical Qualified Lead fact.

AI must not invent:

- Customer budget.
- Purchase date.
- Vehicle requirement.
- Customer authority.
- Consent.
- Finance eligibility.
- Trade-In ownership.
- Appointment availability.
- Customer commitment.

### Qualification Scoring

AI qualification scores must:

- Use approved inputs.
- Use approved models.
- Preserve version.
- Preserve scale.
- Preserve expiration.
- Identify missing data.
- Identify uncertainty.
- Identify material conflicts.
- Remain separate from the qualification Decision.

High model confidence must not override:

- Weak evidence.
- Identity conflict.
- Active opt-out.
- Compliance block.
- Human Review.
- Policy restrictions.

### Missing Information

AI may identify likely missing information.

It must not:

- Fill missing values with invented values.
- Treat absence as rejection unless policy explicitly defines it.
- Contact the Customer directly.
- Request restricted data without approved purpose.

### Routing

AI may recommend:

- Dealership.
- Branch.
- Team.
- Language skill.
- Product specialization.
- Fleet specialization.
- Priority.

Deterministic authorization and routing policy must validate the Recommendation.

### Revalidation

AI may detect possible revalidation triggers.

An AI signal must not itself:

- Expire the Qualified Lead.
- Revoke the Qualified Lead.
- Invalidate the Qualified Lead.
- Cancel Opportunity conversion.
- Cancel an existing Opportunity.

The signal must enter the governed revalidation workflow.

### Action Class 2

Controlled internal actions or Customer communication may proceed through:

- Explicit Human Approval; or
- An applicable pre-approved automation policy.

The deterministic Policy Engine must validate:

- Tenant scope.
- Qualified Lead status.
- Qualification freshness.
- Customer identity.
- Contact permission.
- Purpose.
- Channel.
- Template.
- Frequency.
- Quiet hours.
- Current opt-out.
- Data sensitivity.
- Routing scope.
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
- Qualification revocation.
- Qualification invalidation.
- Post-conversion correction.
- Conversion exception.
- Disclosure of restricted information.
- Another material legal, privacy, financial, or commercial exception.

### AI Context and Embeddings

Qualified Lead data must not enter unrestricted embeddings.

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
- Internal legal notes.
- Fraud-investigation detail.
- Restricted Decision notes.
- Provider credentials.
- Authentication secrets.

Approved redacted context may include:

- General commercial intent.
- General Vehicle preferences.
- General purchase-timeframe category.
- Non-sensitive budget band.
- General finance-interest status.
- General Trade-In-interest status.
- Qualification freshness.
- Non-sensitive qualification summary.
- Conversion-readiness category.

Every vector record must enforce:

- `tenant_id`.
- Purpose.
- Source.
- Qualified Lead version.
- Security classification.
- Retention.
- Expiration.
- Deletion and correction propagation.

### Prompt Injection and Untrusted Content

Lead messages, Interactions, documents, forms, provider payloads, and Customer text are untrusted input.

AI Agents must treat them as data, not instructions.

Untrusted content must not:

- Change system policy.
- Grant permissions.
- Override Tenant scope.
- Reveal secrets.
- Approve qualification.
- Create Opportunities.
- Trigger external Commands.
- Change Consent.
- Modify audit evidence.
- Request hidden system instructions.

### Explainability

Material AI qualification outputs must explain:

- Evidence used.
- Evidence source.
- Input version.
- Model or rule.
- Score scale.
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

This section defines required Qualified Lead API behaviour.

### REST Resources

```text
GET    /api/v1/qualified-leads
GET    /api/v1/qualified-leads/{qualified_lead_id}
PATCH  /api/v1/qualified-leads/{qualified_lead_id}

POST   /api/v1/leads/{lead_id}/qualified-lead-creation-requests

POST   /api/v1/qualified-leads/{qualified_lead_id}/freshness-evaluation-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/revalidation-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/revalidation-decisions

POST   /api/v1/qualified-leads/{qualified_lead_id}/opportunity-conversion-requests

POST   /api/v1/qualified-leads/{qualified_lead_id}/revocation-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/invalidation-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/correction-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/supersession-requests
POST   /api/v1/qualified-leads/{qualified_lead_id}/archive-requests

GET    /api/v1/qualified-leads/{qualified_lead_id}/versions
GET    /api/v1/qualified-leads/{qualified_lead_id}/criteria
GET    /api/v1/qualified-leads/{qualified_lead_id}/evidence
GET    /api/v1/qualified-leads/{qualified_lead_id}/reviews
GET    /api/v1/qualified-leads/{qualified_lead_id}/revalidation-history
GET    /api/v1/qualified-leads/{qualified_lead_id}/conversion
GET    /api/v1/qualified-leads/{qualified_lead_id}/history
GET    /api/v1/qualified-leads/{qualified_lead_id}/reconciliation
```

### Tenant Context

- `tenant_id` must come from authenticated security context.
- Request bodies must not override `tenant_id`.
- Dealership, branch, team, queue, User, Lead, Customer, and Opportunity scope must be validated.
- Cross-Tenant queries must be blocked by default.

### Creation Request Boundary

The creation endpoint does not perform the full Lead qualification workflow.

It accepts a completed and authoritative positive qualification Decision and creates the Canonical Qualified Lead.

The Lead qualification request and negative outcomes remain governed by Lead.

### Example Creation Request

```json
{
  "qualification_request_id": "3c9355aa-71a8-48ce-8909-8958446e5b52",
  "qualification_decision_id": "dd64f44f-e317-49d3-8f66-62fa8d2a9d42",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "expected_lead_record_version": 9,
  "expected_customer_record_version": 14,
  "qualification_policy": {
    "qualification_policy_id": "dcbfa69f-3bdd-43f6-8923-7d258a9615ab",
    "qualification_policy_version": "2.3.0",
    "qualification_policy_snapshot_hash": "sha256:281dbab6..."
  },
  "expected_evidence_snapshot_hash": "sha256:6e5b28d2...",
  "commercial_intent": {
    "primary_commercial_intent": "NEW_VEHICLE_PURCHASE",
    "purchase_timeline_category": "WITHIN_30_DAYS",
    "vehicle_condition_preference": "NEW",
    "quantity_required": 1,
    "payment_preference": "UNDECIDED",
    "finance_interest_status": "POSSIBLE",
    "trade_in_interest_status": "NOT_EXPRESSED",
    "appointment_interest_status": "EXPRESSED"
  },
  "effective_from": "2026-08-01T20:00:00Z",
  "expires_at": "2026-08-31T20:00:00Z"
}
```

The request must include:

```text
Idempotency-Key: 945270b5-e58d-45e8-b500-8c79c799ba3a
```

### Example Creation Response

```json
{
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "qualification_cycle_id": "4a4593f3-69c0-4589-887a-fced0d77c781",
  "qualified_lead_number": "QL-2026-000842",
  "qualification_version": 1,
  "lead_id": "123e4567-e89b-12d3-a456-426614174000",
  "customer_id": "a2d85b86-7079-4aaf-964a-580cc040046b",
  "status": "ACTIVE",
  "qualification_result": "QUALIFIED",
  "freshness_status": "CURRENT",
  "conversion_eligibility_status": "ELIGIBLE",
  "effective_from": "2026-08-01T20:00:00Z",
  "expires_at": "2026-08-31T20:00:00Z",
  "record_version": 1,
  "created_at": "2026-08-01T20:00:01Z"
}
```

### Example Revalidation Request

```json
{
  "trigger_codes": [
    "PURCHASE_TIMEFRAME_CHANGED",
    "VEHICLE_REQUIREMENT_CHANGED"
  ],
  "trigger_evidence_references": [
    "evidence://interactions/71df1d4e/content"
  ],
  "expected_qualification_version": 1,
  "expected_record_version": 4
}
```

The request must include an idempotency key.

### Example Revalidation-Pending Response

```json
{
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "status": "REVALIDATION_PENDING",
  "revalidation_status": "IN_PROGRESS",
  "conversion_eligibility_status": "REVALIDATION_REQUIRED",
  "revalidation_request_id": "1fc5bdd0-dd5f-4cf1-8bd0-c9c4aa08d82f",
  "record_version": 5
}
```

### Example Revalidation Decision

```json
{
  "revalidation_decision_id": "90710d4a-49ad-4c1b-8eec-2c5402b6eb3b",
  "revalidation_outcome": "NEW_QUALIFICATION_VERSION_CREATED",
  "qualification_policy_id": "dcbfa69f-3bdd-43f6-8923-7d258a9615ab",
  "qualification_policy_version": "2.3.0",
  "evidence_snapshot_hash": "sha256:98db376f...",
  "commercial_intent_snapshot_hash": "sha256:beaa3091...",
  "new_effective_from": "2026-08-03T08:00:00Z",
  "new_expires_at": "2026-09-02T08:00:00Z",
  "expected_record_version": 5
}
```

### Example Opportunity-Conversion Request

```json
{
  "dealership_id": "2dc50e3c-392a-44d7-9dc4-8fd7e586ff03",
  "branch_id": "6835ea02-a8df-4d3d-a1ec-4e309ea9ac38",
  "sales_team_id": "f4e7be20-26d1-4df1-a17a-04c6d4a58419",
  "requested_owner_user_id": "0f63b9de-97fd-42f6-a53d-531155afdf56",
  "expected_qualification_version": 2,
  "expected_record_version": 8,
  "initial_next_action": {
    "type": "CALL_CUSTOMER",
    "due_at": "2026-08-04T09:00:00Z"
  }
}
```

The request must include:

```text
Idempotency-Key: 77acb283-ce30-4413-80ba-d6304fc438ec
```

### Example Conversion-Pending Response

```json
{
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "status": "CONVERSION_PENDING",
  "opportunity_conversion_request_status": "PENDING_OPPORTUNITY_CONFIRMATION",
  "opportunity_conversion_request_id": "4276b3d4-1688-4f45-81f5-2930477148dc",
  "qualification_version": 2,
  "record_version": 9
}
```

The response must not claim that an Opportunity exists.

### Example Confirmed Conversion Response

```json
{
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "status": "CONVERTED",
  "qualification_version": 2,
  "opportunity_id": "a524e9f1-6d7e-4820-a0c3-5039f5089346",
  "opportunity_record_version": 1,
  "opportunity_conversion_request_status": "CONFIRMED",
  "opportunity_conversion_confirmation_status": "RECEIVED",
  "opportunity_conversion_confirmed_at": "2026-08-04T08:31:15Z",
  "record_version": 10
}
```

### Example Revocation Request

```json
{
  "revocation_reason": "CUSTOMER_WITHDREW_COMMERCIAL_INTENT",
  "revocation_evidence_references": [
    "evidence://interactions/a7e7412d/content"
  ],
  "expected_qualification_version": 2,
  "expected_record_version": 10
}
```

### Example Invalidation Request

```json
{
  "invalidation_reason": "CUSTOMER_IDENTITY_RESOLUTION_WAS_INCORRECT",
  "invalidation_decision_id": "5e6e244a-eb6b-43a1-8358-d1d772484142",
  "invalidation_evidence_references": [
    "evidence://customer-identity-corrections/20941"
  ],
  "expected_qualification_version": 2,
  "expected_record_version": 10
}
```

Post-conversion invalidation must return or create downstream review and reconciliation references.

### Mutation Requirements

Every mutation must enforce:

- Authentication.
- Tenant scope.
- Organizational scope.
- Authorization.
- Record-version validation.
- Qualification-version validation.
- Lead and Customer relationship validation.
- Policy and Decision authority.
- Evidence integrity.
- Lifecycle validation.
- Freshness.
- Human Review.
- Conversion eligibility.
- Idempotency.
- Audit recording.
- Event publication after accepted state change.
- Opportunity Confirmation tracking where applicable.

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

- Qualified Leads.
- Qualification versions.
- Revalidation requests.
- Reviews.
- Opportunity-conversion requests.
- Opportunities.
- Revocations.
- Invalidations.
- Corrections.
- Supersessions.

### Pending Confirmation

Opportunity conversion may return:

```json
{
  "operation_status": "PENDING_CONFIRMATION",
  "qualified_lead_id": "e8a95ca8-e829-48f2-b635-4cc585dce131",
  "opportunity_conversion_request_id": "4276b3d4-1688-4f45-81f5-2930477148dc",
  "record_version": 9
}
```

The API must not describe the conversion as complete until Opportunity Domain Service confirms creation.

### Error Categories

The API must distinguish at least:

- `UNAUTHENTICATED`
- `UNAUTHORIZED`
- `TENANT_SCOPE_VIOLATION`
- `ORGANIZATIONAL_SCOPE_VIOLATION`
- `VALIDATION_FAILED`
- `VERSION_CONFLICT`
- `QUALIFICATION_VERSION_CONFLICT`
- `LEAD_NOT_FOUND`
- `LEAD_NOT_ELIGIBLE`
- `LEAD_TENANT_MISMATCH`
- `LEAD_ALREADY_HAS_CURRENT_QUALIFIED_LEAD`
- `CUSTOMER_REQUIRED`
- `CUSTOMER_IDENTITY_UNRESOLVED`
- `CUSTOMER_IDENTITY_CONFLICT`
- `CUSTOMER_TENANT_MISMATCH`
- `QUALIFICATION_REQUEST_REQUIRED`
- `QUALIFICATION_DECISION_REQUIRED`
- `QUALIFICATION_DECISION_NOT_APPROVED`
- `QUALIFICATION_RESULT_NOT_POSITIVE`
- `QUALIFICATION_POLICY_REQUIRED`
- `QUALIFICATION_POLICY_EXPIRED`
- `QUALIFICATION_EVIDENCE_REQUIRED`
- `EVIDENCE_INTEGRITY_FAILED`
- `BLOCKING_CRITERION_NOT_SATISFIED`
- `HUMAN_REVIEW_REQUIRED`
- `CONTACT_PERMISSION_CONFLICT`
- `QUALIFICATION_STALE`
- `QUALIFICATION_EXPIRED`
- `QUALIFICATION_REVOKED`
- `QUALIFICATION_INVALIDATED`
- `REVALIDATION_REQUIRED`
- `CONVERSION_NOT_ELIGIBLE`
- `CONVERSION_ALREADY_CONFIRMED`
- `OPPORTUNITY_ALREADY_EXISTS`
- `OPPORTUNITY_CONFIRMATION_PENDING`
- `INVALID_LIFECYCLE_TRANSITION`
- `PUBLISHED_SNAPSHOT_IMMUTABLE`
- `RECONCILIATION_REQUIRED`
- `RECORD_ARCHIVED`

### GraphQL Requirements

A GraphQL implementation must enforce the same:

- Tenant isolation.
- Organizational scope.
- Lead and Customer integrity.
- Decision authority.
- Evidence integrity.
- Freshness.
- Qualification-version immutability.
- Concurrency.
- Idempotency.
- Human Review.
- Conversion ownership.
- Opportunity Confirmation.
- Audit requirements.

GraphQL resolvers must not bypass Qualified Lead Domain Service, Lead Domain Service, Customer Domain Service, Policy Engine, Decision controls, or Opportunity Domain Service.

---

## 11. Database Design

### Recommended Tables

```text
qualified_leads
qualified_lead_versions
qualified_lead_origin_snapshots
qualified_lead_customer_snapshots
qualified_lead_policy_snapshots
qualified_lead_decision_snapshots
qualified_lead_criteria_results
qualified_lead_evidence_packages
qualified_lead_evidence_items
qualified_lead_commercial_intent
qualified_lead_vehicle_requirements
qualified_lead_budget_context
qualified_lead_payment_preferences
qualified_lead_finance_interest
qualified_lead_trade_in_interest
qualified_lead_appointment_interest
qualified_lead_geographic_context
qualified_lead_permission_projections
qualified_lead_scores
qualified_lead_routing_recommendations
qualified_lead_review_requests
qualified_lead_review_decisions
qualified_lead_revalidation_requests
qualified_lead_revalidation_decisions
qualified_lead_conversion_requests
qualified_lead_conversion_confirmations
qualified_lead_revocations
qualified_lead_invalidations
qualified_lead_corrections
qualified_lead_supersessions
qualified_lead_derived_intelligence
qualified_lead_data_quality_issues
qualified_lead_conflicts
qualified_lead_reconciliation_cases
qualified_lead_status_history
qualified_lead_record_versions
qualified_lead_audit_log
```

### Qualified Leads Table

The `qualified_leads` table should contain:

- Canonical identifiers.
- Tenant and organizational scope.
- Lead, Customer, and Opportunity relationships.
- Qualification cycle.
- Current qualification version.
- Current lifecycle state.
- Current freshness.
- Current validity.
- Current conversion eligibility.
- Current Opportunity-conversion state.
- Current review state.
- Current data-quality and conflict state.
- Current source and synchronization state.
- Record version.
- Audit timestamps.

Immutable snapshots and repeating data must remain in child tables.

### Primary Key

```text
PRIMARY KEY (qualified_lead_id)
```

### Tenant Protection

Every Qualified Lead-related table must include:

```text
tenant_id
```

Tenant consistency must be enforced using:

- Composite Tenant-aware foreign keys; or
- Equivalent database and service controls.

Row-Level Security should be used where supported.

### Recommended Indexes

```text
idx_qualified_leads_tenant_status
  (tenant_id, status)

idx_qualified_leads_tenant_lead
  (tenant_id, lead_id)

idx_qualified_leads_tenant_customer
  (tenant_id, customer_id)

idx_qualified_leads_tenant_cycle
  (tenant_id, qualification_cycle_id)

idx_qualified_leads_tenant_opportunity
  (tenant_id, opportunity_id)

idx_qualified_leads_freshness
  (tenant_id, freshness_status, expires_at)

idx_qualified_leads_revalidation
  (tenant_id, revalidation_status, revalidation_due_at)

idx_qualified_leads_conversion
  (tenant_id, conversion_eligibility_status, status)

idx_qualified_leads_review
  (tenant_id, review_status)

idx_qualified_leads_data_quality
  (tenant_id, data_quality_status, conflict_status)

idx_qualified_leads_routing
  (tenant_id, recommended_dealership_id, recommended_branch_id)

idx_qualified_leads_updated_at
  (tenant_id, updated_at)
```

### Unique Constraints

Recommended constraints include:

```text
UNIQUE (tenant_id, qualified_lead_number)
```

```text
UNIQUE (
  tenant_id,
  qualification_cycle_id,
  qualification_version
)
```

One accepted qualification Decision must not create multiple Qualified Leads:

```text
UNIQUE (
  tenant_id,
  qualification_decision_id
)
```

One Qualified Lead must not create multiple primary Opportunities:

```text
UNIQUE (
  tenant_id,
  qualified_lead_id,
  opportunity_id
)
```

A partial or transactional constraint should enforce one linked primary Opportunity per Qualified Lead.

### Current-Version Constraint

Only one current qualification version may exist:

```text
UNIQUE (
  tenant_id,
  qualified_lead_id
)
WHERE is_current_qualification_version = true
```

or an equivalent transactional control.

### Active Cycle Constraint

A Lead should not have multiple current active Qualified Leads for the same qualification cycle.

An equivalent controlled uniqueness rule should apply to:

```text
tenant_id
lead_id
qualification_cycle_id
current-state classification
```

Legitimate different cycles must remain supported.

### Version Storage

`qualified_lead_versions` must preserve:

- Qualified Lead identifier.
- Qualification cycle.
- Qualification version.
- Lead version.
- Customer version.
- Policy snapshot.
- Decision snapshot.
- Criteria snapshot.
- Evidence snapshot.
- Commercial-intent snapshot.
- Freshness.
- Effective period.
- Snapshot hashes.
- Creation authority.
- Supersession relationship.
- Related Events.

Accepted versions must be immutable.

### Evidence Storage

Evidence-package and evidence-item tables should preserve:

- Source.
- Authority.
- Source record.
- Source version.
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

Large documents and media must remain in controlled evidence storage.

### Criteria Storage

Criteria-result records should preserve:

- Policy.
- Criterion.
- Criterion version.
- Required and blocking flags.
- Observed values.
- Normalized values.
- Calculation.
- Outcome.
- Evidence.
- Actor.
- Review.
- Timestamp.
- Related Events.

### Decision Storage

Qualification Decision snapshots should preserve:

- Decision.
- Authority.
- Actor.
- Status.
- Result.
- Reasons.
- Conditions.
- Policy.
- Evidence.
- Review.
- Effective period.
- Expiration.
- Hash.
- Related Events.

### Commercial-Intent Storage

Commercial-intent child records should preserve:

- Qualification version.
- Original evidence.
- Normalized values.
- Accepted values.
- Assumptions.
- Limitations.
- Hash.
- Related Events.

### Permission Projection Storage

Permission-projection records should preserve:

- Customer.
- Purpose.
- Channel.
- Consent or lawful-basis reference.
- Opt-out state.
- Restriction.
- Checked time.
- Expiration.
- Policy.
- Source.
- Related Events.

Permission projections must not replace canonical Consent records.

### Derived Intelligence Storage

Derived records must remain separate from authoritative snapshots.

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

### Revalidation Storage

Revalidation records should preserve:

- Trigger.
- Trigger evidence.
- Current qualification version.
- Requested time.
- Policy.
- Reviewer.
- New evidence.
- Criteria.
- Decision.
- Outcome.
- New qualification version where applicable.
- Failure.
- Related Events.

### Conversion Storage

Conversion request records should preserve:

- Qualified Lead.
- Qualification version.
- Conversion request.
- Idempotency key.
- Requested organizational context.
- Request actor.
- Request time.
- Validation.
- Opportunity reference.
- Opportunity Event reference.
- Confirmation.
- Failure.
- Reconciliation.
- Related Events.

### Revocation and Invalidation Storage

Revocation and invalidation records should preserve:

- Qualified Lead.
- Qualification version.
- Request.
- Reason.
- Decision.
- Evidence.
- Actor.
- Timestamp.
- Affected Opportunity.
- Downstream review.
- Reconciliation.
- Related Events.

### Audit Storage

Qualified Lead audit records must be append-only or protected through an equivalent immutable-audit mechanism.

Secure hashes should replace raw sensitive values where full-value audit retention is unnecessary.

### Partitioning

Large deployments may partition by:

- `tenant_id`.
- Qualification date.
- Expiration date.
- Status.
- Dealership.
- Region.
- Retention class.
- Security classification.
- Audit date.

Partitioning must not weaken:

- Tenant isolation.
- Decision uniqueness.
- Version uniqueness.
- One-Opportunity rule.
- Snapshot immutability.
- Referential integrity.
- Audit integrity.

### Hard Deletion

A Qualified Lead must not be hard-deleted when referenced by:

- Lead.
- Customer Journey.
- Opportunity.
- Appointment.
- Quotation.
- Trade-In.
- Finance Application.
- Deal.
- Interaction.
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
| `DIRECT_IDENTIFIER` | Customer and contact-point references |
| `CUSTOMER_COMMERCIAL_CONTEXT` | Vehicle need, timeline, purchase purpose |
| `CUSTOMER_FINANCIAL_CONTEXT` | Budget and finance-interest context |
| `IDENTITY_RESTRICTED` | Identity-resolution evidence |
| `CONSENT_AND_PERMISSION` | Contact permission and opt-out evidence |
| `FRAUD_AND_COMPLIANCE_RESTRICTED` | Risk and investigation data |
| `INTERNAL_ROUTING` | Priority and routing Recommendations |
| `DERIVED_INTELLIGENCE` | Scores, classifications, summaries |
| `AUTHORITATIVE_DECISION` | Qualification and review Decisions |
| `AUDIT_EVIDENCE` | Events, versions, Commands, and history |

### Authentication

Every internal Qualified Lead operation requires an authenticated Human or service identity.

Anonymous creation, approval, revalidation, conversion, revocation, or invalidation is prohibited.

Customer-originated evidence may enter through approved Lead or Interaction workflows.

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
- Lead relationship.
- Qualified Lead status.
- Qualification version.
- Requested field.
- Requested action.
- Security classification.
- Business purpose.
- Delegated authority.
- Legal hold.
- External CRM authority.

### Example Role Boundaries

#### Lead Intake User

May access permitted:

- Originating Lead.
- Qualification evidence collection.
- Missing-information state.
- Qualification-request status.

Must not independently:

- Approve restricted qualification exceptions.
- Revoke a Qualified Lead.
- Invalidate a converted Qualified Lead.
- View restricted fraud or finance details without authority.

#### Sales Consultant

May access assigned or permitted:

- Active Qualified Lead summary.
- Commercial intent.
- Vehicle requirements.
- General budget band where permitted.
- Finance and Trade-In interest.
- Contact-permission projection.
- Conversion readiness.

Must not access without authority:

- Identity documents.
- Detailed fraud investigation.
- Restricted compliance notes.
- Full finance documents.
- Unrestricted Decision notes.
- Another team’s restricted Qualified Leads.

#### Sales Manager

May access permitted team Qualified Leads and perform configured:

- Qualification review.
- Routing Decision.
- Conversion exception review.
- Priority override.
- Revalidation review.

Manager access does not automatically bypass security, Consent, fraud, compliance, or Tenant controls.

#### Data Steward

May review:

- Duplicate cycles.
- Identity relationships.
- Snapshot integrity.
- Source conflicts.
- Data-quality issues.
- Reconciliation.
- Version lineage.

Raw sensitive evidence access should remain minimized.

#### Compliance or Fraud Reviewer

May access restricted evidence required for assigned review.

Access must remain purpose-limited and audited.

#### AI Agent

May access only the minimum Qualified Lead context required for an approved task.

AI access must be:

- Tenant-scoped.
- Purpose-limited.
- Field-restricted.
- Logged.
- Time-limited where appropriate.
- Prevented from cross-Tenant retrieval.
- Prevented from unrestricted access to identity documents, finance records, fraud investigations, Decision notes, and full Interaction content.

#### Integration Service

May access only fields required for an approved CRM, workflow, analytics, or Domain integration.

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

- Customer contact points.
- Identity evidence.
- Budget details.
- Finance context.
- Fraud-risk evidence.
- Compliance-review notes.
- Qualification Decision notes.
- Reviewer identity where restricted.

### Customer Data Minimization

The Qualified Lead must contain only data required for:

- Qualification evidence.
- Validity.
- Routing.
- Conversion.
- Audit.

It must not become a copy of the full Customer record.

Customer identifiers and sensitive attributes should be referenced rather than duplicated.

### Financial Context Protection

Budget and finance-interest fields must:

- Be minimized.
- Use appropriate classification.
- Avoid storing full bank or credit information.
- Avoid storing Lender records.
- Avoid unrestricted export.
- Avoid unrestricted AI processing.
- Be masked where full values are unnecessary.
- Follow applicable retention.

### Contact-Permission Protection

Permission evidence must:

- Use controlled references.
- Preserve purpose and channel.
- Preserve source.
- Preserve checked time.
- Preserve expiration.
- Prevent outdated permission from supporting communication.
- Prevent opt-out bypass.

### Evidence Security

Qualification evidence must:

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
- Qualification cycles.
- Customer matching.
- Lead matching.
- Search.
- Vector retrieval.
- Evidence.
- Queues.
- Caches.
- Events.
- Revalidation.
- Conversion.
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

External credentials must never appear in Qualified Lead records, Events, Prompts, or ordinary Logs.

### Conversion Security

An Opportunity-conversion request must include:

- Authenticated actor or service.
- `tenant_id`.
- Qualified Lead identifier.
- Qualification version.
- Record version.
- Lead and Customer references.
- Conversion eligibility.
- Organizational scope.
- Idempotency key.
- Snapshot hashes.
- Audit evidence.

Qualified Lead Domain Service must not directly manipulate Opportunity persistence.

### Command Security

Where an external CRM Command is required, it must include:

- Authenticated service identity.
- Tenant.
- Qualified Lead.
- Qualification version.
- Requested action.
- Current record version.
- Decision or automation-policy reference.
- Idempotency key.
- Audit evidence.
- External Confirmation requirement.

The AI Intelligence Layer must not transmit external Commands directly.

### Audit Requirements

Material Qualified Lead activity must record:

- `tenant_id`.
- `qualified_lead_id`.
- `qualification_cycle_id`.
- Qualification version.
- Lead.
- Customer.
- Opportunity where applicable.
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
- Criteria.
- Evidence snapshot hash.
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

- Cross-Tenant Qualified Lead access attempt.
- Unauthorized positive qualification creation.
- Qualification Decision without authority.
- Duplicate Qualified Lead creation attempt.
- Snapshot-hash mismatch.
- Evidence tampering.
- Policy-version manipulation.
- Contact-permission bypass.
- Opt-out bypass.
- Unauthorized conversion.
- Duplicate Opportunity-conversion attempt.
- Conversion with expired qualification.
- Conversion with invalid Customer identity.
- Unauthorized revocation.
- Unauthorized invalidation.
- Restricted evidence export.
- AI access outside approved scope.
- Prompt injection in qualification evidence.
- Audit-log tampering.

### Qualification Integrity

The platform must detect or prevent:

- AI score represented as Decision.
- Recommendation represented as approval.
- Negative Lead outcome represented as Qualified Lead.
- Multiple current versions.
- Multiple primary Opportunities.
- Expired qualification used as current.
- Evidence substitution.
- Policy substitution.
- Customer identity substitution.
- Contact-permission substitution.
- Snapshot modification after acceptance.
- Conversion status changed without Opportunity evidence.
- Historical qualification deletion.
- Cross-Tenant Customer or Lead linkage.

### Privacy and Retention

Qualified Lead retention must follow:

- Applicable law.
- Tenant policy.
- Customer privacy rights.
- Consent and permission state.
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

- Qualified Lead creation.
- Automated qualification.
- Opportunity conversion.
- CRM synchronization.
- Revalidation automation.
- AI qualification analysis.
- Qualified Lead export.
- Connector access.

Emergency suspension must be deterministic, auditable, and reversible only by authorized roles.

---

## Governing Documents

- [ASOS Constitution](../../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](./README.md)
- [ASOS Customer](./Customer.md)
- [ASOS Lead](./Lead.md)
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

This document is the approved Canonical Qualified Lead baseline.

Lead owns original inquiry intake, qualification requests, evidence collection, pending qualification workflow, and negative outcomes.

Qualified Lead owns the accepted positive qualification Decision, qualification snapshot, validity, freshness, revalidation, revocation, invalidation, and Opportunity-conversion eligibility.

Opportunity owns the active commercial pursuit after confirmed conversion.

A Qualified Lead is created only from an accepted positive qualification Decision.

A score or Recommendation is not a qualification Decision.

Qualification is time-bounded and must not be treated as a permanent Customer attribute.

Expired, revoked, invalidated, stale, or conflicted qualification must not be converted.

Opportunity conversion is idempotent and is not complete until Opportunity Domain Service confirms Opportunity creation.

The originating Lead and historical qualification versions remain traceable and immutable.

Detailed Event names and Schemas will be governed by the Canonical Event Catalog.

Detailed API Schemas will be governed by the API Contracts Catalog.

Machine-readable storage and validation Schemas will be governed by the Data Schemas Catalog.
