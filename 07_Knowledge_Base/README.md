# ASOS Knowledge Base

## Purpose

The ASOS Knowledge Base contains the canonical, implementation-ready contracts used to build and operate the ASOS AI Sales Operating System.

It defines the shared technical language used by:

- Backend services.
- APIs and integrations.
- Databases and event streams.
- AI Agents.
- Analytics.
- Security and audit services.
- Engineering and Product teams.

## Authoritative Scope

This directory is the authoritative location for:

- Canonical Domain Models.
- Canonical Event specifications.
- API Contracts.
- Data Schemas.
- AI Agent Contracts.
- Integration Contracts.
- Canonical validation requirements.
- Canonical security requirements.

## Out of Scope

The following content must not be duplicated inside this directory:

| Content Type | Authoritative Location |
| :--- | :--- |
| Constitutional principles | [`../00_Constitution`](../00_Constitution/) |
| Operational Playbooks | [`../01_Playbooks`](../01_Playbooks/) |
| Business Rules | [`../02_Business_Rules`](../02_Business_Rules/) |
| Production and evaluation Prompts | [`../03_Prompts`](../03_Prompts/) |
| Reference data and safe examples | [`../04_Data`](../04_Data/) |
| Architecture and project documentation | [`../05_Documentation`](../05_Documentation/) |
| Images, diagrams, and templates | [`../06_Assets`](../06_Assets/) |

Canonical specifications may reference these sources but must not redefine them inconsistently.

## Current Structure

```text
07_Knowledge_Base/
├── README.md
└── docs/
    ├── README.md
    └── 01-Domain-Model/
```

## Current Canonical Baseline

The current Domain Model contains 14 canonical objects and is located at:

[ASOS Canonical Domain Model](./docs/01-Domain-Model/)

## Governance Rules

- Every concept must have one authoritative definition.
- Canonical Object lifecycles must not be duplicated.
- Events must reference canonical Domain Objects.
- APIs and Schemas must use canonical field names.
- AI Agents must follow defined Human Approval boundaries.
- Every dealership-owned record must enforce tenant isolation.
- Sensitive and authoritative evidence must remain auditable.
- Breaking changes require versioning and migration planning.

## Build Direction

The planned Knowledge Base expansion is:

1. Canonical Domain Model.
2. Canonical Event Catalog.
3. API and Integration Contracts.
4. Canonical Data Schemas.
5. AI Agent Contracts.

Folder numbering represents repository organization and does not necessarily represent implementation order.

## Status

**Phase:** Canonical Architecture  
**Version:** `0.1.0`  
**Status:** In Progress
