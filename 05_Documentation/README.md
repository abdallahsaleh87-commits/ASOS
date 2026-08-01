# ASOS Documentation

## Purpose

This directory contains the authoritative product, architecture, governance, implementation-planning, and Pilot documentation for the ASOS AI Sales Operating System.

These documents explain why ASOS exists, how the platform is designed, how authority is distributed, and how controlled deployments should be planned and evaluated.

---

## Authoritative Documents

| Document | Responsibility |
| :--- | :--- |
| [System Architecture](./System_Architecture.md) | Defines the overall ASOS architecture, platform layers, security principles, and technical direction. |
| [Data Ownership and Systems of Record](./Data_Ownership_and_Systems_of_Record.md) | Defines source authority, field ownership, synchronization, conflict handling, and audit evidence. |
| [MVP Pilot Framework](./MVP_Pilot_Framework.md) | Defines the reusable framework for configuring, executing, and evaluating an ASOS Pilot for any dealership. |

---

## Content That Belongs Here

This directory is the authoritative location for:

- Product Vision and strategy.
- System Architecture.
- Governance policies.
- Architectural Decision Records.
- Security and privacy guidance.
- Data ownership policies.
- Implementation guidance.
- Roadmaps and delivery plans.
- Reusable Pilot frameworks.
- Deployment and evaluation guidance.

---

## Content That Does Not Belong Here

The following content must remain in its authoritative repository location:

| Content Type | Authoritative Location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Images, diagrams, and reusable assets | [`../06_Assets`](../06_Assets/) |
| Canonical Domain Models, Events, APIs, and Schemas | [`../07_Knowledge_Base`](../07_Knowledge_Base/) |

Documentation may reference these locations but must not create competing authoritative definitions.

---

## Documentation Hierarchy

ASOS documentation follows this authority order:

1. Constitution and non-negotiable governance.
2. Product and System Architecture.
3. Data ownership and authority policies.
4. Canonical implementation contracts.
5. Business Rules, Playbooks, and Prompts.
6. Pilot and implementation configuration.

A lower-level document must not contradict a higher-level governing document.

---

## Documentation Rules

- Every document must have a clear purpose.
- Each governed concept must have one authoritative location.
- Links should be used instead of copying complete definitions.
- Architectural changes should be versioned and documented.
- Breaking changes require impact assessment and migration planning.
- Dealership-specific values must remain configurable.
- Real credentials, secrets, and unrestricted Production data must not be stored here.
- Outdated documents must be marked clearly or replaced with an authoritative reference.

---

## Current Baseline

The current documentation baseline includes:

- System Architecture.
- Data Ownership and Systems-of-Record policy.
- Repository content ownership.
- Reusable MVP Pilot Framework.
- Links to the Canonical Domain Model.

---

## Status

**Phase:** Foundation and Canonical Architecture  
**Version:** `1.0.0`  
**Status:** Approved Baseline
