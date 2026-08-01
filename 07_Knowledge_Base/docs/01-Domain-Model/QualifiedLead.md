# Qualified Lead

**Version:** 1.1.0  
**Status:** Approved Baseline  
**Canonical Owner:** Qualified Lead Domain Service  
**Primary Isolation Boundary:** `tenant_id`  
**Last Updated:** 2026-08-01  

---

## 1. Object Purpose

### Business Purpose

The Qualified Lead Object represents the governed result of determining that an originating Lead has sufficient current evidence to justify active automotive sales engagement.

A Qualified Lead is not a person.

The person or organization is represented by `Customer`.

A Qualified Lead is not the original inquiry.

The original inquiry is represented by `Lead`.

A Qualified Lead is not an active commercial negotiation.

An active commercial pursuit is represented by `Opportunity`.

The Qualified Lead establishes a controlled handoff between Lead intake and Opportunity management.

It confirms that an approved qualification process evaluated the originating Lead against configured criteria and produced a usable commercial-intent package.

The qualification package may include:

- Resolved Customer identity.
- Permitted contact path.
- Automotive commercial intent.
- Purchase purpose.
- Purchase timeframe.
- Vehicle requirements.
- Budget information.
- Payment preference.
- Finance interest.
- Trade-In interest.
- Appointment or test-drive interest.
- Geographic or dealership preference.
- Qualification evidence.
- Missing-information state.
- Priority and routing context.
- Human Review outcome where required.
- Expiration and revalidation requirements.

### Lead, Customer, Qualified Lead, and Opportunity Separation

The principal separation is:

```text
Lead
  = original inquiry or expression of interest

Customer
  = canonical party identity and dealership relationship

Qualified Lead
  = governed qualification outcome and commercial-intent snapshot

Opportunity
  = active commercial pursuit and sales pipeline workflow
```

A Qualified Lead must reference:

- Exactly one originating Lead.
- Exactly one resolved Customer.

A Lead may produce one current Qualified Lead for one active qualification cycle.

A Qualified Lead may later create one primary Opportunity through a controlled and idempotent conversion workflow.

The original Lead must remain historically traceable after qualification and conversion.

### Qualification Is a Time-Bounded Decision

Qualification is not a permanent Customer attribute.

A Customer may qualify for one commercial journey and not qualify for another.

A qualification outcome may become stale when:

- Purchase timeframe changes.
- Contact permission changes.
- Customer identity becomes disputed.
- Vehicle requirements materially change.
- Budget information changes.
- Fraud or compliance risk changes.
- Required evidence expires.
- The configured qualification-validity period ends.

A Qualified Lead therefore requires:

- Qualification timestamp.
- Qualification policy and version.
- Evidence snapshot.
-
