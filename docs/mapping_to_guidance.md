# Mapping to Guidance — UK Regulations & International Portability

**Version:** 1.0.0-beta  
**Last updated:** 14 October 2025

---

## Purpose

This document provides **explicit mapping** between OpenPEEP schema fields and:

- **UK fire safety regulations** (primary alignment)
- **International standards** (portability)
- **Accessibility and safeguarding duties**

It is intended for:
- **Duty holders** (landlords, housing providers, care organisations, employers)
- **Regulators** (local authorities, Fire and Rescue Services, HSE)
- **Implementers** ensuring compliance
- **International adopters** seeking to adapt OpenPEEP to their jurisdictions

---

## UK Regulatory Framework

OpenPEEP is designed to support compliance with:

### 1. Fire Safety (Residential Evacuation Plans) (England) Regulations 2025

**Status:** In force from **April 2026**

**Key requirements:**

- **Regulation 3**: Duty to prepare and maintain evacuation plans for "relevant residents" in residential buildings
- **Regulation 4**: Requirement to offer consent; refusal must be recorded
- **Regulation 5**: Plans must be reviewed at least annually or on change of circumstance
- **Regulation 10**: Duty to provide plain-language information to residents
- **Regulation 12**: Duty to share evacuation plans with Fire and Rescue Service (with consent)

**OpenPEEP support:**

| Regulation | Schema Field(s) | Notes |
|------------|----------------|-------|
| Reg 3 (duty to prepare) | `pcfra.person.outcome.relevantResident`, `pcfra.person.outcome.peepRequired` | Boolean flags trigger RPEEP creation |
| Reg 4 (consent) | `consent.state`, `consent.scope`, `consent.capturedAt`, `consent.method` | Standalone consent record; supports `given`, `refused`, `withdrawn` |
| Reg 5 (review) | `peep.reviews[]`, `review.reviewed_at`, `review.outcome`, `review.next_due` | Lifecycle tracking; supports annual review cycle |
| Reg 10 (information to residents) | `ees_personal.content`, `ees_personal.language`, `ees_personal.format`, `ees_personal.delivery` | Plain-language EES Personal; accessibility formats; delivery tracking |
| Reg 12 (share with FRS) | `consent.scope` (`share_with_frs`), `peep.metadata.documentRef` | Consent scope includes FRS sharing; document location for golden thread |

**Extensions for RPEEP (UK-specific):**

```json
{
  "extensions": {
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-uuid",
      "informationDutyEvidenceRef": "https://example.org/evidence/info-duty-2025",
      "stayingPutBoundary": "Stay put unless fire is in your flat or you are told to leave by FRS"
    }
  }
}
```

---

### 2. Regulatory Reform (Fire Safety) Order 2005 (FSO 2005)

**Key requirements:**

- **Article 8**: Duty to identify relevant persons and assess risks
- **Article 9**: Duty to make and implement fire safety arrangements
- **Article 15**: Duty to provide fire safety information to employees and relevant persons
- **Article 21**: Duty to provide training
- **Article 22**: Duty to maintain premises and equipment

**OpenPEEP support:**

| Article | Schema Field(s) | Notes |
|---------|----------------|-------|
| Art 8 (identify persons) | `person_profile.personRef`, `pcfra.person.personRef`, `peep.personProfile` | Person identity anchor |
| Art 9 (fire safety arrangements) | `peep.evacuationStrategy`, `peep.routes`, `peep.evacuationProcedure` | Evacuation planning |
| Art 15 (information) | `ees_building.who_should_move_when`, `ees_personal.content`, `peep.planSummary` | Plain-language guidance |
| Art 21 (training) | `peep.trainingDrills` | Training and drill arrangements |
| Art 22 (maintain) | `peep.reviews[]`, `pcfra.person.reviews[]` | Review and maintenance cycle |

---

### 3. Building Safety Act 2022 ("Golden Thread")

**Key requirements:**

- **Golden thread**: digital record of building information, accessible throughout building lifecycle
- **Duty to manage building safety risks** (Accountable Person)
- **Duty to cooperate and share information**

**OpenPEEP support:**

| Concept | Schema Field(s) | Notes |
|---------|----------------|-------|
| Golden thread linkage | `peep.metadata.goldenThreadUrn`, `address.uprn`, `address.buildingRef`, `peep.references.buildingStatementEesId` | Stable identifiers for cross-referencing |
| Digital record | All schemas use JSON Schema 2020-12; machine-readable; version-controlled | Structured data for golden thread systems |
| Information sharing | `consent.scope`, `peep.metadata.documentRef`, `ees_building.related_document_ids` | Controlled sharing with consent |

---

### 4. Equality Act 2010

**Key requirements:**

- **Section 20**: Duty to make reasonable adjustments for disabled persons
- **Section 29**: Duty to provide accessible services
- **Public Sector Equality Duty (PSED)**: advance equality of opportunity

**OpenPEEP support:**

| Duty | Schema Field(s) | Notes |
|------|----------------|-------|
| Reasonable adjustments | `pcfra.person.capability.*`, `peep.communications`, `peep.routes.assistance`, `peep.routes.equipment` | Functional needs captured; adjustments specified |
| Accessible information | `person_profile.communicationPreferences`, `ees_personal.language`, `ees_personal.format`, `ees_building.accessibility_formats` | Multiple formats supported (large print, easy read, BSL, audio, braille) |
| Advance equality | `pcfra.person.capability` (functional, non-diagnostic) | Avoid medical model; focus on functional capability |

---

### 5. Approved Document B (Fire Safety)

**Key guidance:**

- **Section 5**: Access and facilities for Fire and Rescue Service
- **Section 6**: Means of escape
- **Section 7**: Fire warning and alarm systems

**OpenPEEP support:**

| Section | Schema Field(s) | Notes |
|---------|----------------|-------|
| Sec 5 (FRS access) | `peep.references.buildingStatementEesId`, `ees_building.resident_help_and_contacts.frs_information_point` | FRS information point |
| Sec 6 (means of escape) | `peep.routes`, `peep.evacuationStrategy`, `ees_building.routes_summary`, `peep.routes.refuge` | Routes, refuges, muster points |
| Sec 7 (alarm systems) | `peep.communications.alarmHearingAbility`, `peep.communications.alertingMethods`, `ees_building.alarm_and_alerting_summary` | Alarm accessibility |

---

### 6. NFCC Person-Centred Fire Risk Framework

**Key concepts:**

- **Functional assessment** (mobility, sensory, cognitive, communication)
- **Environmental risks** (hoarding, electrical, cooking, smoking)
- **Multi-agency approach** (safeguarding, escalation)
- **Review cycles** (annual or on change)

**OpenPEEP support:**

| Concept | Schema Field(s) | Notes |
|---------|----------------|-------|
| Functional assessment | `pcfra.person.capability.*` | Comprehensive functional capability capture |
| Environmental risks | `pcfra.environment.[fireLoad, ignitionSources, detectionAlarm, escapeRoute, heatingRisk, oxygenRisk, petsRisk]` | Observable risks; no diagnoses |
| Multi-agency | `pcfra.environment.[agencyInvolved, referralsMade]` | Safeguarding context |
| Review cycles | `pcfra.person.reviews[]`, `peep.reviews[]`, `review.next_due` | Annual review tracking |

#### Classification Annotations

To support consistent implementation and analytics, OpenPEEP uses schema annotations:

- `x-openpeep-kind`: classifies fields (e.g., `functional_capability`, `support_in_place`, `behavioural_risk`, `planning_consideration`, `outcome_field`)
- `x-openpeep-domain`: groups fields into domains (`mobility_physical`, `sensory`, `cognition_communication`, `alertness_response`, `behavioural`, `planning_considerations`)

These labels enable:
- Clear mapping from PCFRA capability items to PEEP sections (routes, assistance, communications) and EES content
- Domain-level reporting aligned with the NFCC framework
- UI grouping and accessibility-friendly presentation

Note: These annotations are schema metadata; they do not change validation or add personal data.

---

## International Portability

OpenPEEP is designed to be **internationally portable** by:

1. **Using global standards** for core data types
2. **Avoiding UK-specific hard-coding** in core schemas
3. **Providing extension points** for jurisdiction-specific requirements

### Global Standards Alignment

| Concept | Standard | OpenPEEP Implementation |
|---------|----------|-------------------------|
| Timestamps | ISO 8601 / RFC 3339 | All `date-time` fields (e.g., `createdAt`, `assessedAt`, `capturedAt`) |
| Dates | ISO 8601 | All `date` fields (e.g., `dob`, `effectiveFrom`, `next_due`) |
| Countries | ISO 3166-1 alpha-2 | `address.countryCode` (default: `GB`; override for other countries) |
| Subdivisions | ISO 3166-2 | Not enforced in core; may be used in `address.county` or extensions |
| Languages | ISO 639-1 | `person_profile.preferredLanguage`, `peep.language`, `ees_personal.language` |
| Addressing | ISO 19160 (conceptual) | `address.schema.json` uses portable, flexible addressing fields |
| UUIDs | RFC 4122 | `peepId`, `document_id`, `pcfraId` (UUID v4) |

### Jurisdiction Profiles

OpenPEEP supports **jurisdiction-specific extensions** via the `extensions` object. Examples:

#### UK (`gbr`) Profile

Used for UK-specific requirements (e.g., RPEEP, UPRN, postcode validation):

```json
{
  "extensions": {
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-uuid",
      "informationDutyEvidenceRef": "https://example.org/evidence/info-duty-2025"
    }
  }
}
```

#### EU (`eu`) Profile

Placeholder for EU-specific requirements (e.g., accessibility directives, GDPR-specific fields):

```json
{
  "extensions": {
    "eu": {
      "gdprLawfulBasis": "consent",
      "dataProtectionOfficerContact": "dpo@example.eu"
    }
  }
}
```

#### USA (`usa`) Profile

Placeholder for US-specific requirements (e.g., ADA compliance, state-level regulations):

```json
{
  "extensions": {
    "usa": {
      "adaCompliant": true,
      "stateRegulation": "CA_Title24"
    }
  }
}
```

### Portable Addressing

The **Address schema** is designed to support international addressing patterns:

- **No postcode format enforcement** in core (UK postcode validation in `extensions.gbr` only)
- **Flexible address lines** (1-3 lines + locality + town/city + county)
- **Generic `nationalAddressId`** slot for non-UK identifiers (e.g., cadastre, municipal IDs)
- **Country code** (ISO 3166-1 alpha-2) for international use

**Example (UK address):**

```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "addressLine1": "45 High Street",
  "townCity": "Manchester",
  "county": "Greater Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678"
}
```

**Example (USA address):**

```json
{
  "buildingNumber": "123",
  "addressLine1": "Main Street",
  "addressLine2": "Apartment 4B",
  "townCity": "New York",
  "county": "New York",
  "postcode": "10001",
  "countryCode": "US",
  "nationalAddressId": "usps-123456789"
}
```

**Example (EU address, France):**

```json
{
  "buildingNumber": "12",
  "addressLine1": "Rue de la République",
  "addressLine2": "Appartement 3",
  "townCity": "Lyon",
  "postcode": "69001",
  "countryCode": "FR"
}
```

---

## Mapping Table: UK Regulations → OpenPEEP Schemas

| UK Regulation/Guidance | OpenPEEP Schema(s) | Key Fields | Notes |
|------------------------|-------------------|------------|-------|
| **Residential Regs 2025, Reg 3** (duty to prepare RPEEP) | `pcfra.person`, `peep` | `outcome.relevantResident`, `outcome.peepRequired`, `peepId` | Triggers RPEEP creation |
| **Residential Regs 2025, Reg 4** (consent) | `consent` | `state`, `scope`, `capturedAt`, `method` | Standalone consent record |
| **Residential Regs 2025, Reg 5** (review) | `review`, `peep.reviews[]` | `reviewed_at`, `outcome`, `next_due` | Annual review cycle |
| **Residential Regs 2025, Reg 10** (information to residents) | `ees_personal` | `content`, `language`, `format`, `delivery` | Plain-language EES |
| **Residential Regs 2025, Reg 12** (share with FRS) | `consent`, `peep` | `consent.scope` (`share_with_frs`), `peep.metadata.documentRef` | Controlled sharing |
| **FSO 2005, Art 8** (identify persons) | `person_profile` | `personRef`, `name`, `contact` | Person identity |
| **FSO 2005, Art 9** (fire safety arrangements) | `peep` | `evacuationStrategy`, `routes`, `evacuationProcedure` | Evacuation plan |
| **FSO 2005, Art 15** (information) | `ees_building`, `ees_personal` | `who_should_move_when`, `content` | Resident guidance |
| **FSO 2005, Art 21** (training) | `peep` | `trainingDrills` | Training arrangements |
| **FSO 2005, Art 22** (maintain) | `review` | `reviewed_at`, `outcome`, `next_due` | Maintenance cycle |
| **Building Safety Act 2022** (golden thread) | `peep`, `address`, `ees_building` | `goldenThreadUrn`, `uprn`, `buildingRef`, `documentRef`, `related_document_ids` | Digital linkage |
| **Equality Act 2010, Sec 20** (reasonable adjustments) | `pcfra.person`, `peep` | `capability.*`, `communications`, `routes.assistance`, `routes.equipment` | Functional needs |
| **Equality Act 2010, Sec 29** (accessible services) | `person_profile`, `ees_personal`, `ees_building` | `communicationPreferences`, `language`, `format`, `accessibility_formats` | Accessible formats |
| **Approved Document B, Sec 5** (FRS access) | `ees_building` | `resident_help_and_contacts.frs_information_point` | FRS liaison |
| **Approved Document B, Sec 6** (means of escape) | `peep`, `ees_building` | `routes`, `evacuationStrategy`, `routes_summary`, `routes.refuge` | Escape routes |
| **Approved Document B, Sec 7** (alarm systems) | `peep`, `ees_building` | `communications.alarmHearingAbility`, `alarm_and_alerting_summary` | Alarm accessibility |
| **NFCC PCF Framework** (functional assessment) | `pcfra.person` | `capability.*`, `assessmentMethod`, `confidenceLevel` | Person-centred approach |
| **NFCC PCF Framework** (environmental risks) | `pcfra.environment` | `environment.*`, `safeguarding.*` | Observable risks |

---

## Compliance Checklist: RPEEP (UK Residential)

For duty holders preparing RPEEPs under the **Fire Safety (Residential Evacuation Plans) (England) Regulations 2025**, ensure the following:

### Pre-requisites

- [ ] **Determine "relevant resident" status** (`pcfra.person.outcome.relevantResident`)
- [ ] **Offer consent** (`consent.offerMadeAt`, `consent.state`)
- [ ] **Record consent decision** (given, refused, or withdrawn) (`consent.capturedAt`, `consent.method`)

### PCFRA (Personal)

- [ ] **Assess functional capability** (`pcfra.person.capability.*`)
- [ ] **Record assessment method** (`pcfra.person.assessmentMethod`: phone, digital_self, in_person)
- [ ] **Record confidence level** (`pcfra.person.confidenceLevel`)
- [ ] **Determine PEEP requirement** (`pcfra.person.outcome.peepRequired`, `pcfra.person.outcome.rationale`)
- [ ] **Link consent record** (`pcfra.person.consentRef`)

### PCFRA (Environmental) (if applicable)

- [ ] **Assess dwelling/setting risks** (`pcfra.environment.[fireLoad, ignitionSources, detectionAlarm, escapeRoute, heatingRisk, oxygenRisk, petsRisk]`)
- [ ] **Record safeguarding context** (`pcfra.environment.[asbViolenceRisk, environmentalSafeguarding, agencyInvolved, immediateDanger, immediateActionTaken, referralsMade]` if applicable)
- [ ] **Determine PEEP requirement** (`pcfra.environment.outcome.peepRequired`)

### PEEP

- [ ] **Create PEEP** (`peep.peepId`, `peep.schemaVersion`)
- [ ] **Link Person Profile** (`peep.personProfile`)
- [ ] **Specify address** (`peep.address`)
- [ ] **Define evacuation strategy** (`peep.evacuationStrategy`)
- [ ] **Define routes** (minimum 1; must include "Primary") (`peep.routes[]`)
- [ ] **Define step-by-step procedure** (`peep.evacuationProcedure[]`)
- [ ] **Define communications adjustments** (`peep.communications` if applicable)
- [ ] **Link consent** (`peep.consent`)
- [ ] **Record metadata** (`peep.metadata.createdAt`, `peep.metadata.createdBy`, `peep.metadata.status`)
- [ ] **Set validity period** (`peep.validity.effectiveFrom`, optionally `effectiveTo`)
- [ ] **Add UK-specific extensions** (`peep.extensions.gbr.relevantResident`, `residentFacingStatementRef`)

### EES (Personal)

- [ ] **Create plain-language EES** (`ees_personal.document_id`)
- [ ] **Link to PEEP** (`ees_personal.peep_document_id`)
- [ ] **Provide step-by-step instructions** (`ees_personal.content.steps_primary[]`)
- [ ] **Provide scenario instructions** (`ees_personal.content.scenario_instructions[]`)
- [ ] **Specify language and format** (`ees_personal.language`, `ees_personal.format[]`)
- [ ] **Deliver to resident** (`ees_personal.delivery.delivered_by`, `ees_personal.delivery.delivered_at`)
- [ ] **Track acknowledgement** (`ees_personal.delivery.acknowledged_by_resident`)

### EES (Building)

- [ ] **Create building-wide EES** (`ees_building.document_id`)
- [ ] **Define evacuation strategy** (`ees_building.evacuation_strategy`)
- [ ] **Provide plain-language guidance** (`ees_building.who_should_move_when`)
- [ ] **Specify accessibility formats** (`ees_building.accessibility_formats[]`)
- [ ] **Specify distribution methods** (`ees_building.distribution_methods[]`)

### Information Duty (Reg 10)

- [ ] **Provide EES Personal to resident** (delivered via accessible format)
- [ ] **Record delivery** (`ees_personal.delivery`)
- [ ] **Provide evidence** (`peep.extensions.gbr.informationDutyEvidenceRef`)

### Sharing with FRS (Reg 12)

- [ ] **Obtain consent for sharing** (`consent.scope` includes `share_with_frs`)
- [ ] **Share PEEP with FRS** (via agreed mechanism; record in `peep.metadata.documentRef`)

### Review Cycle (Reg 5)

- [ ] **Review annually or on change** (`review.reviewed_at`, `review.outcome`)
- [ ] **Set next review due date** (`review.next_due`)
- [ ] **Record review outcome** (`confirmed_no_change`, `amended`, `superseded`, `withdrawn`)
- [ ] **Link new document if superseded** (`review.new_document_ref`)

---

## Adaptation for Other Jurisdictions

### Steps for International Adoption

1. **Identify local regulations** (e.g., EU directives, ADA in USA, Australian Standards)
2. **Map local requirements to OpenPEEP core schemas**
3. **Define jurisdiction profile** (e.g., `extensions.eu`, `extensions.usa`, `extensions.au`)
4. **Add jurisdiction-specific fields** in extensions (avoid modifying core)
5. **Provide examples** demonstrating compliance
6. **Document mapping** (similar to this document)

### Example: EU Adaptation

**Relevant EU directives:**

- **EU Directive 2012/18/EU (Seveso III)**: Major accident hazards
- **EU Accessibility Act (2019)**: Accessibility of products and services
- **GDPR**: Data protection

**Proposed EU profile:**

```json
{
  "extensions": {
    "eu": {
      "gdprLawfulBasis": "consent",
      "dataProtectionOfficerContact": "dpo@example.eu",
      "accessibilityActCompliant": true,
      "sevesoDirectiveApplicable": false
    }
  }
}
```

### Example: USA Adaptation

**Relevant US regulations:**

- **ADA (Americans with Disabilities Act)**: Accessibility requirements
- **OSHA (Occupational Safety and Health Administration)**: Workplace safety
- **NFPA (National Fire Protection Association) codes**: Fire safety standards

**Proposed USA profile:**

```json
{
  "extensions": {
    "usa": {
      "adaCompliant": true,
      "oshaApplicable": true,
      "nfpaCodeVersion": "NFPA101-2021",
      "stateRegulation": "CA_Title24"
    }
  }
}
```

---

## References

### UK Legislation & Guidance

- **Fire Safety (Residential Evacuation Plans) (England) Regulations 2025** (SI 2025/XXX) — in force April 2026
- **Regulatory Reform (Fire Safety) Order 2005** (SI 2005/1541)
- **Building Safety Act 2022** (c. 30)
- **Equality Act 2010** (c. 15)
- **Approved Document B: Fire Safety** (2019 edition with 2020/2022 amendments)
- **NFCC Person-Centred Fire Risk Framework** (2021)
- **UK GDPR** (retained EU law)

### International Standards

- **ISO 8601:2004** — Date and time format
- **RFC 3339** — Date and Time on the Internet: Timestamps
- **ISO 3166-1:2020** — Country codes (alpha-2)
- **ISO 3166-2:2020** — Subdivision codes
- **ISO 639-1:2002** — Language codes (two-letter)
- **ISO 19160** — Addressing (conceptual framework)
- **RFC 4122** — UUID specification
- **JSON Schema Draft 2020-12** — Schema specification

### Fire Safety Standards (Optional Reference)

- **BS 9991:2015** — Fire safety in the design, management and use of residential buildings
- **BS 9999:2017** — Fire safety in the design, management and use of buildings
- **BS 7974** — Application of fire safety engineering principles
- **EN 81-76:2011** — Evacuation lifts

---

## Change Log

| Date | Version | Change | Rationale |
|------|---------|--------|-----------|
| 2025-10-14 | 1.0.0-beta | Initial mapping to guidance | Baseline documentation |

---

## Next Steps

- **Review [Data Dictionary](data_dictionary.md)** for field definitions
- **Consult [Privacy & GDPR](privacy_gdpr.md)** for data minimisation and retention guidance
- **Explore the schemas** in `/schemas/`

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

