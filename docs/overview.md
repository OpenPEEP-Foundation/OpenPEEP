# Overview — OpenPEEP Concept Model

**Version:** 1.0.0-beta  
**Last updated:** 14 October 2025

---

## Purpose

This document provides a high-level overview of the OpenPEEP standard's **concept model** and explains how the schemas relate to one another. It is intended for:

- Standards editors and schema authors
- Implementers (housing providers, care organisations, fire services)
- Regulators and governance bodies
- Developers integrating OpenPEEP into systems

---

## Mission

OpenPEEP is an open, government-grade standard for creating, storing, and sharing:

- **Person-Centred Fire Risk Assessments (PCFRA)** — personal and environmental
- **Personal Emergency Evacuation Plans (PEEP)**
- **Consent records**
- **Emergency Evacuation Statements (EES)** — building and personal
- **Review blocks** — lifecycle tracking

The standard originates from UK fire safety regulation but is designed for **international portability**.

---

## Core Concepts

### 1. Person-Centred Fire Risk Assessment (PCFRA)

A PCFRA captures **functional evacuation capability** and **environmental risks** without storing medical diagnoses. It is designed to inform a PEEP.

OpenPEEP splits PCFRA into **two schema variants**:

#### PCFRA (Personal)

**Schema:** `pcfra.person.schema.json`

- Focuses on the **individual's functional capability** for evacuation:
  - Mobility, sensory, cognitive, communication, awareness
  - Assistance needs, equipment dependencies
  - Behavioural risks (observable, non-diagnostic)
- Portable: can follow the person across different environments
- Assessment method may be: self-assessment, phone interview, or in-person
- Includes decision outputs: `relevantResident`, `peepRequired`, `rationale`

**Example use cases:**
- A resident self-assesses their evacuation capability via a digital form
- A housing officer conducts a phone interview to record functional needs
- A fire safety advisor observes and records mobility and sensory capabilities

#### Cross-cutting annotations

The personal PCFRA employs two schema annotations to support consistent implementation:
- `x-openpeep-kind` — classifies a field’s role (e.g., functional capability vs planning consideration)
- `x-openpeep-domain` — groups a field by domain (mobility_physical, sensory, cognition_communication, alertness_response, behavioural, planning_considerations)

Annotations are schema metadata; they do not alter validation or add personal data. They enable UI grouping, analytics, and transformation of PCFRA outputs into PEEPs and EES content.

#### PCFRA (Environmental)

**Schema:** `pcfra.environment.schema.json`

- Focuses on the **dwelling or setting**:
  - Observed risks: smoke alarms, electrical hazards, hoarding, cooking practices
  - Access/egress issues
  - Safeguarding context (ASB, agency involvement, immediate danger)
- Typically in-person assessment
- Anchored to the location (building or unit) rather than the person
- Includes decision outputs: `relevantResident`, `peepRequired`, `rationale`

**Example use cases:**
- A housing officer visits a flat and observes clutter blocking exits
- A fire safety advisor notes electrical risks and refers to adult social care
- An assessor records immediate mitigation actions taken (e.g., isolated circuit)

**Joining Personal and Environmental PCFRAs:**

These may be captured separately but are **referenced together** when producing a PEEP. A PEEP typically draws on:
- Personal PCFRA → informs assistance, communications, equipment needs
- Environmental PCFRA → informs risk context and safeguarding escalation
- Building context (EES Building) → informs evacuation strategy and routes

---

### 2. Personal Emergency Evacuation Plan (PEEP)

**Schema:** `peep.schema.json`

A PEEP is the **final evacuation plan** for an individual. It:

- References source PCFRAs (via `references.pcfraId`)
- Embeds or references a **Person Profile** and **Address**
- Contains the **step-by-step evacuation procedure** (`evacuationProcedure`)
- Specifies **routes** (at least one named "Primary"), **assistance**, **equipment**, and **communications adjustments**
- Includes **metadata** (created date, author, status, provenance)
- Links to a **Consent** record (given, refused, or withdrawn)
- Supports **lifecycle reviews** (via embedded `reviews` array)

**Example use cases:**
- A housing provider creates a PEEP for a wheelchair user in a high-rise flat
- A workplace creates a PEEP for an employee with sensory impairments
- A care home documents a PEEP for a resident with cognitive support needs

---

### 3. Consent

**Schema:** `common/consent.schema.json`

A **minimal, legally auditable consent record** for evacuation planning. It records:

- **State**: `given`, `refused`, or `withdrawn`
- **Scope**: what the consent applies to (e.g., `pcfra`, `peep`, `rpeep`, `ees_person_statement`, `share_with_frs`)
- **Timestamp** and **captured by** (provenance)
- **Method**: `written`, `verbal`, or `assisted`
- Optional **reason** for refusal/withdrawal
- Optional **validity period** (`validFrom`, `validUntil`)

**Key principle:** Consent is **standalone and reusable**. It is **not** an access-control model; it simply records the consent decision.

**Example use cases:**
- A resident gives written consent for a PCFRA and PEEP to be created
- A resident refuses consent for sharing their PEEP with the Fire and Rescue Service
- A resident withdraws consent previously given; this is recorded with a timestamp

---

### 4. Review

**Schema:** `common/review.schema.json`

A **reusable lifecycle block** for tracking reviews of OpenPEEP documents (PCFRA, PEEP, EES). It records:

- **Reviewed at** (timestamp)
- **Reviewed by** (name, role, organisation, contact)
- **Outcome**: `confirmed_no_change`, `amended`, `superseded`, or `withdrawn`
- **Summary** of the review decision
- **Next due** date for review
- **New document reference** (if superseded or amended)

**Example use cases:**
- A PEEP is reviewed annually and confirmed with no changes
- A PCFRA is amended following a change in the resident's mobility; a new PCFRA is created
- A PEEP is superseded when a resident moves to a new address

---

### 5. Emergency Evacuation Statement (EES)

EES provides **plain-language evacuation guidance** for residents and buildings.

#### EES (Building)

**Schema:** `ees_building.schema.json`

- **Building-wide evacuation strategy** (e.g., stay put, simultaneous, phased)
- High-level summary of routes, refuges, lifts, muster points
- Plain-language guidance for residents: "Who should move when?"
- Accessibility formats and languages available
- Distribution methods (noticeboard, door drop, email, QR codes)

**Example use cases:**
- A housing provider publishes a building-wide EES for a high-rise residential block
- A care home displays the EES in common areas and distributes it in large print and easy-read formats

#### EES (Personal)

**Schema:** `ees_personal.schema.json`

- **Resident-facing instructions** derived from the PEEP and building strategy
- Plain-language, step-by-step guidance for **this person**
- Scenario-based instructions (e.g., "If fire is in your home", "If fire is elsewhere")
- Assistance arrangements (who, what equipment, how to contact)
- Delivery and acknowledgement tracking

**Example use cases:**
- A resident receives a personal EES in large print via door drop
- A resident with cognitive support needs receives an easy-read EES with step-by-step instructions
- A resident's EES is updated following a review of their PEEP

---

### 6. Person Profile

**Schema:** `common/person_profile.schema.json`

A **minimal identity anchor** for a person associated with a PCFRA, PEEP, or EES. It includes:

- **Person reference** (unique identifier)
- **Name** and optional **alias**
- Optional **date of birth** or **age band** (data minimisation)
- **Preferred language** and **communication preferences** (channels, formats)
- Optional **contact details** (phone, email)

**Key principle:** No impairments, evacuation needs, or medical data. Identity only.

---

### 7. Address

**Schema:** `common/address.schema.json`

A **portable, international addressing schema** aligned with ISO 19160 concepts. It includes:

- Building name, number, sub-building (e.g., flat, unit, room)
- Address lines, locality, town/city, county, postcode
- **Country code** (ISO 3166-1 alpha-2)
- Optional UK-specific identifiers: **UPRN**, **UDPRN**
- Generic **national address ID** slot for international use
- **Building reference** and **unit reference** (OpenPEEP stable identifiers)

**Key principle:** No routing, wayfinding, or proprietary constructs. Location identity only.

---

## Concept Model Diagram

```
Person Profile ─┐
                │
                ├─> Personal PCFRA ─┐
                │                   │
Consent ────────┘                   │
                                    ├─> PEEP ──> Evacuation Procedure
                ┌─> Environmental PCFRA ┘              │
                │                                       │
Address ────────┘                                       │
                                                        │
Review (lifecycle) ─────────────────────────────────────┘
                                                        │
EES (Building) ─────────────────────────────────────────┘
                                                        │
EES (Personal) ─────────────────────────────────────────┘
```

**Flow:**

1. **Person Profile** + **Consent** → enables PCFRA
2. **Personal PCFRA** + **Environmental PCFRA** → inform PEEP
3. **PEEP** + **Building context (EES Building)** → produce **EES Personal**
4. **Review** blocks track lifecycle changes to PCFRAs, PEEPs, and EES documents
5. **Address** provides location context for PEEP and EES

---

## RPEEP (Residential Personal Emergency Evacuation Plan)

**RPEEP** is **not a separate schema**. It is a **process profile** or **methodology** applied to the core PEEP schema in residential contexts.

Key RPEEP characteristics (implemented via PEEP extensions and metadata):

- **Context:** Residential settings (flats, sheltered housing, care homes)
- **Regulation:** Fire Safety (Residential Evacuation Plans) (England) Regulations 2025 (in force April 2026)
- **Duty holders:** Responsible persons (landlords, housing providers, care home operators)
- **Process differences:**
  - Consent must be offered and recorded (`consent.state`)
  - Information duty to residents (plain-language EES Personal)
  - Information duty to Fire and Rescue Service (sharing consent)
  - Review cycles (typically annual or on change of circumstance)
  - "Relevant resident" determination (captured in PCFRA `outcome.relevantResident`)

**Implementation approach:**

- Use the core **PEEP schema** (`peep.schema.json`)
- Record RPEEP-specific metadata in **extensions.gbr** (UK-specific profile):
  - `relevantResident` (boolean)
  - `residentFacingStatementRef` (link to EES Personal)
  - `informationDutyEvidenceRef` (evidence of information provided to residents/FRS)
  - `stayingPutBoundary` (limits/conditions of staying put)

- Link to **Consent** record with scope `["rpeep", "ees_person_statement", "share_with_frs"]`
- Embed or reference **Review** blocks for lifecycle tracking

**Example use case:**
- A housing provider creates a PEEP for a "relevant resident" in a high-rise block, records consent, delivers a plain-language EES Personal, and shares the PEEP with the Fire and Rescue Service (with consent).

---

## Application Contexts

PEEPs (including RPEEPs) must work across different environments:

### Residential settings
- Flats, sheltered housing, care homes
- Required under Residential Regs 2025 (from April 2026)
- Anchored to both individual and property
- Often long-term

### Workplaces and industrial sites
- Employers have duty under Fire Safety Order and H&S legislation
- May be short-term (contractors) or permanent (employees)
- Typically part of fire risk management

### Public buildings
- Universities, hospitals, transport hubs
- Equality Act requires reasonable adjustments
- PEEPs may be needed for staff, students, or visitors
- Often integrated with wider accessibility planning

### Transient or temporary settings
- Hotels, hostels, student halls
- Short-term PEEPs needed
- Portability and ease of consent are critical
- Minimise burden on individuals while enabling safe evacuation

OpenPEEP supports **portability and consistency** across all these contexts.

---

## Privacy & Data Minimisation

### Core principles (embedded in schemas):

1. **No medical diagnoses**: PCFRA captures functional capability only
2. **Purpose limitation**: collect only what is necessary for evacuation planning
3. **Provenance**: record how information was obtained (`method`, `source`, `evidence`)
4. **Consent as first-class object**: standalone, auditable, reusable
5. **Pseudonymisation in examples**: all example data uses fictitious identities

### Retention guidance:

- PCFRA: review annually or on change of circumstance; archive when superseded
- PEEP: retain for duration of tenancy/employment/stay + regulatory retention period (typically 6 years post-occupancy)
- Consent: retain for life of the associated document + regulatory retention period
- EES: update when building strategy or individual circumstances change

For detailed privacy guidance, see **[Privacy & GDPR](privacy_gdpr.md)**.

---

## Schema Versioning

OpenPEEP uses **Semantic Versioning (SEMVER)**:

- **Major version** (1.x.x): breaking changes (e.g., removing required fields, changing types)
- **Minor version** (x.1.x): non-breaking additions (e.g., new optional fields, new enums)
- **Patch version** (x.x.1): documentation fixes, clarifications, typos

Each schema includes a `schemaVersion` field (e.g., `"1.0.0"`).

**Backwards compatibility:**
- **Additive changes** (new optional fields, new enum values) are non-breaking
- **Removing required fields** or **changing types** are breaking
- **Deprecation policy**: deprecated fields marked in schema description; retained for at least one major version

For full versioning policy, see **[GOVERNANCE.md](../GOVERNANCE.md)**.

---

## Interoperability & Profiles

### Core vs Profiles

- **Core schemas**: portable, internationally usable, no jurisdiction lock-in
- **Profiles**: optional extensions for jurisdiction-specific requirements (e.g., `extensions.gbr`, `extensions.eu`, `extensions.usa`)

**Example (UK profile for Address):**

```json
{
  "extensions": {
    "gbr": {
      "postcode": "SW1A 1AA"
    }
  }
}
```

**Example (UK profile for PEEP/RPEEP):**

```json
{
  "extensions": {
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-uuid-here",
      "informationDutyEvidenceRef": "https://example.org/evidence/resident-info-2025"
    }
  }
}
```

### Global Standards Alignment

- **ISO 8601 / RFC 3339**: timestamps (UTC recommended)
- **ISO 3166-1 alpha-2**: country codes
- **ISO 3166-2**: subdivision codes (optional)
- **ISO 639-1/2**: language codes
- **ISO 19160**: addressing concepts (portable addressing)
- **OGC GeoJSON**: optional geospatial fields (not included in core; vendor extensions only)

---

## Quality Gates

Every schema and example must pass:

1. **Schema validation**: AJV 2020-12 (`npm run lint:schemas`)
2. **Example validation**: At least 3 examples per schema (nominal, edge, refusal/withdrawal)
3. **Data dictionary**: field catalogue updated
4. **Mapping to guidance**: regulatory anchors documented
5. **Backwards compatibility**: assessment recorded; deprecation notes if applicable
6. **Changelog**: SEMVER bump rationale
7. **Prose check**: UK English; no jurisdiction-locked assumptions
8. **Accessibility check**: terminology clarity; plain-language summaries

For full quality gates, see **[CONTRIBUTING.md](../CONTRIBUTING.md)**.

---

## Next Steps

- **Read the [Data Dictionary](data_dictionary.md)** for a complete field catalogue
- **Review [Mapping to Guidance](mapping_to_guidance.md)** for UK regulation anchors
- **Consult [Privacy & GDPR](privacy_gdpr.md)** for data minimisation and retention guidance
- **Explore the schemas** in `/schemas/`
- **Validate examples** using `npm run test:all`

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

