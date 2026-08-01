# ASOS

## AI Sales Operating System

ASOS is an AI-powered Sales Operating System designed for automotive dealerships.

It operates as an intelligent decision and workflow layer above existing dealership systems such as CRM, DMS, inventory, finance, communication, and booking platforms.

ASOS does not automatically replace these external systems. It normalizes their data, applies governed business knowledge, identifies risks and opportunities, recommends actions, and keeps authorized Humans responsible for binding decisions.

## Project Goals

- Increase Vehicle sales.
- Improve Lead-to-Deal conversion.
- Reduce lost sales opportunities.
- Standardize dealership sales operations.
- Improve Customer follow-up.
- Improve Inventory utilization.
- Support Vehicle matching and pricing decisions.
- Coordinate Trade-In and finance workflows.
- Provide explainable and auditable AI assistance.
- Support multiple dealerships and technology stacks.

## Core Principles

- Knowledge Before Code.
- Human-in-the-Loop.
- Explainable AI.
- Canonical Domain Ownership.
- External System-of-Record Awareness.
- Event-Driven Architecture.
- Tenant Isolation.
- Security by Design.
- Immutable Auditability.
- Single Source of Truth.

## Repository Structure

| Directory | Responsibility |
| :--- | :--- |
| [`00_Constitution`](./00_Constitution/) | Constitutional principles, authority boundaries, and non-negotiable governance rules. |
| [`01_Playbooks`](./01_Playbooks/) | Operational Playbooks for dealership Users and AI Agents. |
| [`02_Business_Rules`](./02_Business_Rules/) | Governed business logic, thresholds, eligibility rules, and approval policies. |
| [`03_Prompts`](./03_Prompts/) | Versioned AI prompts, Agent instructions, and structured-output templates. |
| [`04_Data`](./04_Data/) | Approved reference data, mappings, synthetic examples, and non-production datasets. |
| [`05_Documentation`](./05_Documentation/) | Product Vision, System Architecture, governance policies, decisions, and roadmap. |
| [`06_Assets`](./06_Assets/) | Diagrams, images, templates, and supporting visual resources. |
| [`07_Knowledge_Base`](./07_Knowledge_Base/) | Canonical implementation contracts, including Domain Models, Events, APIs, and Schemas. |

## Content Ownership

Each type of knowledge must have one authoritative location:

- Constitutional rules belong in `00_Constitution`.
- Operational Playbooks belong in `01_Playbooks`.
- Business Rules belong in `02_Business_Rules`.
- Production and evaluation prompts belong in `03_Prompts`.
- Reference data and safe examples belong in `04_Data`.
- Architecture and project documentation belong in `05_Documentation`.
- Visual assets belong in `06_Assets`.
- Canonical implementation contracts belong in `07_Knowledge_Base`.

Documents may link to each other, but must not create conflicting definitions, lifecycle states, approval rules, Events, or API contracts.

## Canonical Domain Model

The current Canonical Domain Model contains 14 core objects:

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

The Domain Model is located at:

[ASOS Canonical Domain Model](./07_Knowledge_Base/docs/01-Domain-Model/)

## Data Ownership

ASOS distinguishes between:

- External Systems of Record.
- ASOS Canonical Projections.
- ASOS Authoritative Workflow States.
- Derived Intelligence.
- Authoritative Human Decisions.

The governing policy is:

[ASOS Data Ownership and Systems of Record](./05_Documentation/Data_Ownership_and_Systems_of_Record.md)

The overall architecture is:

[ASOS System Architecture](./05_Documentation/System_Architecture.md)

## Build Sequence

1. Establish governance and architecture.
2. Define the Canonical Domain Model.
3. Define data ownership and Systems of Record.
4. Establish repository content ownership.
5. Define the Canonical Event Catalog.
6. Define API and integration contracts.
7. Define canonical schemas.
8. Define AI Agent contracts.
9. Align Business Rules and Playbooks.
10. Implement and evaluate the controlled MVP.

Folder numbering represents repository organization and does not necessarily represent implementation order.

## Security Notice

This repository must not contain:

- Real Customer personal data.
- Production credentials.
- API keys or access tokens.
- Passwords or private keys.
- Real payment information.
- Unredacted legal documents.
- Real unrestricted VIN datasets.
- Confidential internal pricing or margin data.

Examples must use fictional, synthetic, anonymized, or safely redacted information.

## Current Status

**Project Phase:** Foundation and Canonical Architecture

Completed:

- Product Vision.
- System Architecture.
- Fourteen Canonical Domain Objects.
- Vehicle and Inventory Record responsibility separation.
- Data Ownership and Systems-of-Record policy.
- Repository content ownership definition.

**Version:** `0.1.0`

**Status:** In Progress
