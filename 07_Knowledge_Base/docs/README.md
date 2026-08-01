# ASOS Canonical Documentation

## Purpose

This directory contains the canonical implementation contracts for the ASOS AI Sales Operating System.

These contracts define the shared structures and behaviors used by services, databases, integrations, Events, APIs, Schemas, and AI Agents.

---

## Current Documentation

| Directory | Responsibility | Status |
| :--- | :--- | :--- |
| [`01-Domain-Model`](./01-Domain-Model/) | Defines the canonical ASOS Domain Objects, relationships, ownership boundaries, lifecycles, validation, security, and audit requirements. | Established |

---

## Planned Documentation

The planned expansion of this directory includes:

```text
docs/
├── 01-Domain-Model/
├── 02-Event-Catalog/
├── 03-API-Contracts/
├── 04-Data-Schemas/
├── 05-Agent-Contracts/
└── 06-Integration-Contracts/
```

Planned directories must not be created until their scope and governing standards are approved.

---

## Canonical Domain Model

The current Domain Model contains 14 core objects:

1. Customer
2. Vehicle
3. Inventory Record
4. Lead
5. Qualified Lead
6. Opportunity
7. Appointment
8. Quotation
9. Trade-In
10. Finance Application
11. Financial Contract
12. Deal
13. Interaction
14. Market Intelligence

The authoritative Domain Model index is:

[ASOS Canonical Domain Model](./01-Domain-Model/)

---

## Contract Rules

Canonical documentation must:

- Use consistent Object and field names.
- Define ownership and authority boundaries.
- Reference canonical lifecycle states.
- Define validation requirements.
- Define security and tenant-isolation requirements.
- Define audit and provenance requirements.
- Preserve Human Approval boundaries.
- Support versioned changes.
- Avoid dealership-specific hard-coded values.
- Avoid conflicting definitions.

---

## Content Boundaries

This directory must not duplicate the complete content of:

| Content Type | Authoritative Location |
| :--- | :--- |
| Constitutional principles | [`../../00_Constitution`](../../00_Constitution/) |
| Operational Playbooks | [`../../01_Playbooks`](../../01_Playbooks/) |
| Business Rules | [`../../02_Business_Rules`](../../02_Business_Rules/) |
| Production and evaluation Prompts | [`../../03_Prompts`](../../03_Prompts/) |
| Reference data and safe examples | [`../../04_Data`](../../04_Data/) |
| Architecture and governance documentation | [`../../05_Documentation`](../../05_Documentation/) |
| Images, diagrams, and templates | [`../../06_Assets`](../../06_Assets/) |

Canonical contracts may reference these sources but must not redefine their authoritative content.

---

## Governing Documents

- [ASOS Constitution](../../00_Constitution/Constitution.md)
- [ASOS System Architecture](../../05_Documentation/System_Architecture.md)
- [ASOS Data Ownership and Systems of Record](../../05_Documentation/Data_Ownership_and_Systems_of_Record.md)
- [ASOS MVP Pilot Framework](../../05_Documentation/MVP_Pilot_Framework.md)

---

## Status

**Phase:** Canonical Architecture  
**Version:** `1.0.0`  
**Status:** Approved Baseline
