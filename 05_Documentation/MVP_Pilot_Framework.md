# ASOS MVP Pilot Framework

**Version:** 1.0.0  
**Status:** Approved Baseline  
**Document Owner:** ASOS Product and Pilot Governance  
**Last Updated:** 2026-08-01  

---

## 1. Purpose

This document defines a reusable framework for planning, configuring, executing, and evaluating an ASOS MVP Pilot for any automotive dealership or dealer group.

The framework is dealership-independent.

Sales targets, Inventory scope, Pilot dates, branches, Users, approval limits, and success thresholds must be configured separately for each Pilot.

---

## 2. Pilot Objectives

An ASOS Pilot should validate whether the platform can:

- Improve Lead response and follow-up.
- Increase Lead-to-Appointment conversion.
- Increase Appointment attendance.
- Improve Vehicle matching.
- Reduce lost sales opportunities.
- Improve Inventory utilization.
- Support Sales Managers with actionable intelligence.
- Coordinate Quotation, Trade-In, finance, and Deal workflows.
- Preserve Human authority over binding decisions.
- Produce explainable and auditable recommendations.
- Demonstrate measurable commercial and operational value.

---

## 3. Reusable Pilot Configuration

Every Pilot must define its own configuration without changing the ASOS Canonical Domain Model.

A Pilot configuration may include:

```yaml
pilot_id: configurable
dealership_id: configurable
pilot_name: configurable
start_date: configurable
end_date: configurable
participating_branches: configurable
participating_users: configurable
inventory_scope: configurable
lead_sources: configurable
sales_target_units: configurable
revenue_target: configurable
gross_profit_target: configurable
approval_limits: configurable
success_thresholds: configurable
```

Pilot-specific values must not be hard-coded into Domain Models, Events, APIs, Prompts, or AI Agent contracts.

---

## 4. MVP Scope

The recommended MVP scope includes:

- Customer and Lead intake.
- Lead qualification.
- Opportunity creation and prioritization.
- Vehicle and Inventory matching.
- Follow-up recommendations.
- Appointment scheduling support.
- Quotation workflow support.
- Trade-In workflow coordination.
- Finance workflow coordination.
- Deal progression monitoring.
- Lost-opportunity risk detection.
- Sales Manager dashboards and alerts.
- Human Review and approval.
- Audit logging.
- Pilot KPI measurement.

Capabilities may be enabled or disabled according to the participating dealership’s systems and operational readiness.

---

## 5. Out of Scope

Unless explicitly approved for a specific Pilot, the following are outside the default MVP scope:

- Replacing the dealership CRM or DMS.
- Autonomous contract approval.
- Autonomous pricing approval.
- Autonomous credit decisions.
- Autonomous Trade-In valuation approval.
- Autonomous legal commitments.
- Autonomous Customer communication without approved controls.
- Full historical data migration.
- Unrestricted access to Production systems.
- Multi-country regulatory automation.
- Enterprise-wide deployment before Pilot evaluation.

---

## 6. Roles and Human Authority

A Pilot should define at least the following roles:

| Role | Primary Responsibility |
| :--- | :--- |
| Executive Sponsor | Approves the Pilot and evaluates strategic value. |
| Pilot Owner | Owns Pilot delivery, scope, and coordination. |
| Sales Manager | Reviews recommendations, priorities, exceptions, and team performance. |
| Sales Consultant | Executes approved Customer and sales activities. |
| Data Owner | Approves data access, quality, and usage. |
| System Administrator | Manages Users, permissions, and integrations. |
| ASOS Product Team | Configures, supports, observes, and evaluates the platform. |
| Compliance or Legal Reviewer | Reviews regulated or sensitive workflows when required. |

ASOS may recommend, prioritize, summarize, draft, and coordinate.

Authorized Humans remain responsible for binding commercial, financial, legal, and Customer-impacting decisions.

---

## 7. Pilot Entry Criteria

A Pilot should not begin until the following conditions are satisfied:

- Executive Sponsor identified.
- Pilot Owner identified.
- Participating branches and Users confirmed.
- Pilot dates approved.
- Inventory scope defined.
- Lead sources defined.
- Required data access approved.
- Systems of Record identified.
- Data ownership documented.
- Human approval boundaries defined.
- Success KPIs agreed.
- Security and privacy review completed.
- Training plan prepared.
- Support and escalation process defined.

---

## 8. Pilot Execution Phases

### Phase 1 — Discovery

- Confirm dealership objectives.
- Map the current sales process.
- Identify Systems of Record.
- Identify operational pain points.
- Define Pilot scope and constraints.
- Establish baseline performance.

### Phase 2 — Configuration

- Configure dealership and branch settings.
- Configure Users and permissions.
- Configure Pilot targets.
- Configure approval limits.
- Configure integrations and mappings.
- Configure KPI definitions.

### Phase 3 — Validation

- Validate data mappings.
- Validate Domain Object creation.
- Validate Human Review workflows.
- Validate recommendations and explanations.
- Validate security and tenant isolation.
- Validate audit evidence.

### Phase 4 — Controlled Launch

- Start with approved Users or branches.
- Monitor data quality and operational adoption.
- Review high-risk recommendations.
- Record incidents, overrides, and feedback.
- Adjust configuration through governed changes.

### Phase 5 — Evaluation

- Compare Pilot results with the baseline.
- Measure commercial and operational outcomes.
- Review User adoption.
- Review recommendation quality.
- Review security, compliance, and incidents.
- Produce a scale, revise, extend, or stop decision.

---

## 9. KPI Framework

Each Pilot must select measurable KPIs appropriate to its objectives.

### Commercial KPIs

- Units sold.
- Revenue.
- Gross profit.
- Lead-to-Deal conversion rate.
- Opportunity-to-Deal conversion rate.
- Average Deal cycle time.
- Inventory aging reduction.
- Target achievement rate.

### Funnel KPIs

- Lead response time.
- Lead qualification rate.
- Lead-to-Appointment conversion rate.
- Appointment attendance rate.
- Appointment-to-Quotation conversion rate.
- Quotation-to-Deal conversion rate.
- Lost-opportunity rate.

### Operational KPIs

- Follow-up completion rate.
- Overdue activity rate.
- Sales Manager review time.
- Recommendation acceptance rate.
- Human override rate.
- Workflow completion time.
- Data-quality exception rate.

### AI Quality KPIs

- Recommendation precision.
- Recommendation usefulness.
- Explanation completeness.
- Unsupported-claim rate.
- Human escalation accuracy.
- Policy violation rate.
- Hallucination or fabrication rate.

### Adoption KPIs

- Active Users.
- Daily and weekly usage.
- Workflow completion through ASOS.
- User feedback score.
- Training completion.
- Feature adoption by role.

---

## 10. Success Criteria

Pilot success must be evaluated against agreed thresholds established before launch.

A Pilot should not be considered successful based only on total sales.

The evaluation should consider:

- Commercial impact.
- Funnel improvement.
- Operational efficiency.
- User adoption.
- Recommendation quality.
- Data quality.
- Security and compliance.
- Human trust.
- Auditability.
- Technical reliability.

Targets and thresholds must be stored as Pilot configuration and may differ between dealerships.

---

## 11. Data, Security, and Audit Requirements

Every Pilot must follow the ASOS Data Ownership and Systems-of-Record policy.

The Pilot must:

- Identify the authority for each critical field.
- Separate External System data from ASOS workflow state.
- Preserve source and synchronization evidence.
- Enforce tenant isolation.
- Apply least-privilege access.
- Protect Customer and commercially sensitive data.
- Record Human approvals and overrides.
- Record AI recommendations and explanations.
- Record outbound commands and authoritative confirmations.
- Avoid storing secrets or unrestricted Production data in the repository.

---

## 12. Pilot Exit and Scale Decision

At the end of the Pilot, authorized stakeholders must select one of the following decisions:

- **Scale:** Expand to additional Users, branches, workflows, or dealerships.
- **Extend:** Continue the Pilot to collect more evidence.
- **Revise:** Change scope, configuration, integrations, or workflow design.
- **Pause:** Suspend execution until blocking issues are resolved.
- **Stop:** End the Pilot because value, safety, adoption, or feasibility requirements were not met.

The final decision must include:

- Pilot configuration.
- Baseline and final KPI results.
- Achieved and missed success criteria.
- User feedback.
- Human overrides.
- Security or compliance findings.
- Technical incidents.
- Lessons learned.
- Recommended next actions.

---

## Governing Documents

- [ASOS Constitution](../00_Constitution/Constitution.md)
- [ASOS System Architecture](./System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](./Data_Ownership_and_Systems_of_Record.md)
- [ASOS Canonical Domain Model](../07_Knowledge_Base/docs/01-Domain-Model/)

---

## Current Status

This framework defines the reusable structure for ASOS MVP Pilots.

Dealership-specific targets, dates, Inventory quantities, branches, Users, and success thresholds must be maintained as separate Pilot configuration.
