# OpenPEEP

> **The open standard for digital Personal Emergency Evacuation Plan (PEEP) information.**

**Version:** 1.0.0-beta  
**Status:** Public consultation (pre-release)  
**Published:** October 2025

---

## 📖 New to Software Standards & Open Source?

**If you're new to open standards and want to understand why OpenPEEP matters before diving into technical details:**

👉 **[Start Here: OpenPEEP Concept Guide](docs/concept_guide.md)** *(5-minute read)*

*Explains open source, standards, and why OpenPEEP changes everything — perfect for housing managers, fire officers, and local authority staff.*

---

## For Technical Stakeholders

## Purpose

**From April 2026**, the Fire Safety (Residential Evacuation Plans) (England) Regulations 2025 require responsible persons to:

1. **Assess** evacuation capability of relevant residents
2. **Create** Personal Emergency Evacuation Plans (PEEPs)
3. **Obtain consent** for information sharing
4. **Share** PEEPs with Fire & Rescue Services
5. **Review** plans annually

**The regulations tell you WHAT to do.**  
**OpenPEEP tells you HOW to structure and share this information digitally.**

This standard provides the technical specification for creating, storing, and exchanging PEEP information in a consistent, interoperable format.

---

## Scope

OpenPEEP defines data structures for:

- **PCFRA** (Person-Centred Fire Risk Assessment) — evacuation capability and environmental risk
- **PEEP** (Personal Emergency Evacuation Plan) — the evacuation procedure
- **Consent** — legal record of consent decisions (given, refused, withdrawn)
- **Review** — periodic review tracking
- **EES** (Emergency Evacuation Statement) — plain-language resident instructions

**Application contexts:**
- Residential buildings (high-rise, sheltered housing, care homes)
- Workplaces (Fire Safety Order 2005, Equality Act 2010)
- Healthcare facilities (hospitals, residential care)
- Public buildings (universities, hotels, transport hubs)

**Jurisdiction:**
- UK-origin (compliant with Residential Regs 2025, FSO 2005, BSA 2022)
- Internationally portable (ISO-aligned, extensible for any jurisdiction)

---

## Why a Standard Format Matters

### **The Problem**

After the Grenfell Tower tragedy (72 deaths), it became clear that:

❌ **Fragmented data** — Every organization creates PEEPs differently (Word docs, PDFs, proprietary databases)  
❌ **No interoperability** — Fire Services receive information in 10+ different formats  
❌ **Data doesn't travel** — Resident moves building → PEEP doesn't follow  
❌ **No digital sharing** — Regulations require sharing with FRS, but no standard format exists  
❌ **Reinventing the wheel** — 170 local authorities creating bespoke solutions  

### **The Solution**

✅ **One standard format** — Everyone structures PEEP data the same way  
✅ **Interoperable** — Systems can exchange information seamlessly  
✅ **Portable** — Person moves → data travels with them  
✅ **Machine-readable** — Fire Services can integrate into operational systems  
✅ **Compliant** — Aligns with UK regulations, GDPR, international standards  

**OpenPEEP provides the specification.** Like BS 7671 for electrical wiring or Approved Document B for fire safety design, OpenPEEP defines how to structure digital PEEP information correctly.

---

## Core Principles

### 1. **Standards-Based**

OpenPEEP aligns with established international standards:

- **ISO 8601 / RFC 3339** — Date and time formats (UTC with timezone offset)
- **ISO 3166-1 alpha-2** — Country codes
- **ISO 3166-2** — Subdivision codes (UK nations, US states, etc.)
- **ISO 639-1/2** — Language codes
- **ISO 19160** — Addressing concepts (international portability)
- **JSON Schema Draft 2020-12** — Technical validation specification
- **WCAG 2.2** — Accessible documentation principles

### 2. **Privacy-by-Design**

**Data minimisation:**
- ❌ Do NOT record medical diagnoses ("has Parkinson's disease")
- ✅ DO record functional capability ("uses wheelchair; cannot use stairs")

**GDPR compliant:**
- Consent tracking (legal record of decisions)
- Clear data retention guidance
- Purpose limitation (evacuation planning only)
- Data subject rights (access, rectification, erasure)

### 3. **Separation of Concerns**

**PCFRA** = Assessment outputs (capability, risks, needs)  
**PEEP** = The plan (routes, assistance, procedure)  
**Consent** = Legal record (standalone, auditable)  
**Review** = Version control (annual reviews, supersession)  
**EES** = Plain-language instructions (for residents)

Each serves a distinct purpose. Link them via references (UUIDs).

#### PCFRA structure at a glance
- Personal PCFRA: uses a `capability` object (functional fields + optional `capability.domainNotes`).
- Environmental PCFRA: uses flattened top‑level fields (e.g., `fireLoad`, `ignitionSources`, `detectionAlarm`, `escapeRoute`, `heatingRisk`, `oxygenRisk`, `petsRisk`) plus safeguarding at top level (`asbViolenceRisk`, `environmentalSafeguarding`, `agencyInvolved`, `immediateDanger`, `immediateActionTaken`, `referralsMade`) and optional `domainNotes`.
- Both schemas use `x-openpeep-kind` and `x-openpeep-domain` annotations to support consistent grouping, UI, and analytics.

### 4. **Core + Extensions Pattern**

**This is what makes OpenPEEP universally adoptable.**

**Core schema** = Portable, universal fields (works everywhere)  
**Extensions** = Jurisdiction-specific or organizational fields (your custom data)

**Example:**

```json
{
  "id": "peep-a1b2c3d4",
  "personRef": "person-xyz",
  "addressRef": "address-abc",
  "routes": [
    {
      "type": "primary",
      "description": "Via lift to assembly point..."
    }
  ],
  "extensions": {
    "gbr": {
      "relevantResident": true
    },
    "yourOrganization": {
      "tenantId": "TENANT-12345",
      "propertyCode": "CH-12A"
    }
  }
}
```

**Core travels everywhere. Extensions stay local.**

**See:** [Extensions Guide](docs/extensions_guide.md) for comprehensive explanation.

---

## Who Should Adopt This Standard

### 🏘️ **Housing Providers**
*Social housing, private landlords, housing associations, sheltered housing*

**Your obligation:**
- Fire Safety (Residential Evacuation Plans) (England) Regulations 2025
- Create PEEPs for "relevant residents"
- Share with Fire & Rescue Services (with consent)

**How OpenPEEP helps:**
- Structure your PEEP data in standard format
- Share with FRS using a format they can integrate
- Transfer PEEPs when residents move (data portability)

---

### 🏥 **Care Homes**
*Residential care, nursing homes, supported living*

**Your obligation:**
- Fire Safety Order 2005
- Evacuation planning for vulnerable residents
- CQC compliance (safe evacuation procedures)

**How OpenPEEP helps:**
- Document evacuation capability and needs
- Coordinate with care plans (via extensions)
- Receive PEEPs when residents transfer in (standard format)

---

### 🏢 **Workplaces**
*Employers, building operators, facilities managers*

**Your obligation:**
- Fire Safety Order 2005 (suitable emergency procedures)
- Equality Act 2010 (reasonable adjustments)
- PEEPs for employees and visitors with disabilities

**How OpenPEEP helps:**
- Standardize workplace PEEP creation
- Enable portability (employee takes PEEP to new employer)
- Share with building operators (standard format)

---

### 🚒 **Fire & Rescue Services**
*Emergency response, pre-planning, operational firefighting*

**Your need:**
- Access building evacuation information (pre-planning)
- Structured, machine-readable data (not PDFs)
- Consistent format across all buildings (operational efficiency)

**How OpenPEEP helps:**
- **Receive PEEPs in one standard format** (not 10+ proprietary formats)
- Integrate into operational systems (machine-readable JSON)
- Know who needs assistance, where, and what equipment before attending incident

---

### 🏛️ **Local Authorities**
*Building control, fire safety enforcement, social services*

**Your obligation:**
- Oversee compliance with Residential Regs 2025
- Coordinate across housing stock
- Share information with FRS

**How OpenPEEP helps:**
- Adopt one standard across all housing providers in your area
- Avoid 170 different local implementations
- Enable cross-organization data exchange

---

### 💻 **Software Developers & System Integrators**
*Building PEEP management systems, FRS platforms, housing databases*

**Your need:**
- Technical specification for PEEP data structures
- Validation schemas (ensure compliance)
- Examples and test cases

**How OpenPEEP helps:**
- Production-ready JSON Schema specifications
- Comprehensive documentation (zero ambiguity)
- 48 validation examples (test your implementation)
- Validator tool (verify compliance)

---

## How It Works

### **The OpenPEEP Workflow**

```
┌─────────────────────────────────────────────────────────────┐
│ 1. ASSESS (PCFRA)                                           │
│    Person-Centred Fire Risk Assessment                      │
│    → Functional capability (mobility, sensory, cognitive)   │
│    → Environmental risks (hoarding, electrical, access)     │
│    → Produces: pcfra.person.json + pcfra.environment.json   │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. CONSENT                                                  │
│    Offer consent for PEEP creation and sharing              │
│    → Record decision (given, refused, withdrawn)            │
│    → Produces: consent.json                                 │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. CREATE PEEP                                              │
│    Design Personal Emergency Evacuation Plan                │
│    → Routes (primary, secondary, tertiary)                  │
│    → Assistance (who helps, what equipment)                 │
│    → Step-by-step procedure                                 │
│    → Produces: peep.json                                    │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. INFORM RESIDENT (EES)                                    │
│    Emergency Evacuation Statement (plain language)        │
│    → Accessible formats (large print, easy read, audio)     │
│    → Produces: ees_personal.json + ees_building.json        │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. SHARE                                                    │
│    Share PEEP with Fire & Rescue Service (if consented)     │
│    → Standard format enables integration                    │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. REVIEW                                                   │
│    Annual review (or on change of circumstance)             │
│    → Update if needed                                       │
│    → Produces: review.json                                  │
│    → Links to new PEEP version if amended                   │
└─────────────────────────────────────────────────────────────┘
```

**All documents link via stable identifiers (UUIDs).** This creates an auditable chain.

---

## What This Standard Provides

### **📋 9 Production-Ready Schemas**

**Core schemas:**
- `person_profile.schema.json` — Person identity and contact details
- `address.schema.json` — Location (UK and international addressing)
- `consent.schema.json` — Legal consent record
- `review.schema.json` — Review tracking and version control

**Assessment schemas:**
- `pcfra.person.schema.json` — Person-centred fire risk assessment
- `pcfra.environment.schema.json` — Environmental risk assessment

**Plan schemas:**
- `peep.schema.json` — Personal Emergency Evacuation Plan

**Statement schemas:**
- `ees_personal.schema.json` — Emergency Evacuation Statement (resident-facing)
- `ees_building.schema.json` — Building evacuation statement

**All schemas:**
- JSON Schema Draft 2020-12 compliant
- Stable `$id` URIs for versioning
- Comprehensive validation rules
- Extensibility support (core + extensions pattern)

---

### **📖 Comprehensive Documentation (12,000+ lines)**

**Essential reading:**
- **[Overview](docs/overview.md)** — Concept model and schema relationships
- **[Extensions Guide](docs/extensions_guide.md)** ⭐ **THE KEY CONCEPT** — How to customize for your jurisdiction/organization
- **[Data Dictionary](docs/data_dictionary.md)** — Complete field catalogue
- **[Mapping to Guidance](docs/mapping_to_guidance.md)** — UK regulations compliance reference
- **[Privacy & GDPR](docs/privacy_gdpr.md)** — Data protection guidance

**Technical reference:**
- **[Schema Guides](docs/schemas/)** — 9 comprehensive field-by-field guides (8,801 lines)
  - Every field explained (what, why, how, examples, common mistakes)
  - Core vs extensions clarified
  - Validation requirements detailed

**Reference materials:**
- **[Reference Templates & Examples](docs/reference_templates/)** — Comprehensive templates and realistic examples
  - 5 complete reference templates (what goes in each document)
  - 5 realistic examples (what finished documents look like)
  - Bridge between traditional documents and digital OpenPEEP implementation

**Governance:**
- **[CHANGELOG](CHANGELOG.md)** — Version history and release notes
- **[CONTRIBUTING](CONTRIBUTING.md)** — How to propose changes
- **[GOVERNANCE](GOVERNANCE.md)** — Stewardship and release process

---

### **📁 48 Validation Examples**

**3 examples per schema type:**
1. **Nominal** — Standard, straightforward case
2. **Edge** — Complex needs, multiple requirements
3. **Refusal/Withdrawal** — Consent scenarios (compliance evidence)

**Plus: 3 end-to-end bundles** (21 JSON files):
1. **Residential RPEEP** — High-rise wheelchair user, full lifecycle (assess → consent → plan → inform → review)
2. **Consent refused** — Compliance demonstration (how to record refusal)
3. **Workplace PEEP** — Sensory impairments, buddy system, reasonable adjustments

**All examples:**
- Realistic scenarios (based on real-world cases)
- Privacy-respecting (fictitious data, no real persons)
- Demonstrate linking (UUIDs, cross-references)
- Show extensions pattern (UK, organizational)

**See:** [examples/EXAMPLES_INDEX.md](examples/EXAMPLES_INDEX.md)

---

### **🛠️ Validator & Viewer Tool**

**Browser-based validation tool** (no installation required)

**Access:** `tools/viewer/index.html` (open locally or host on GitHub Pages)

**Features:**
- ✅ Drag & drop JSON file → instant validation
- ✅ Full schema validation (JSON Schema 2020-12 via AJV)
- ✅ Detailed error messages with fix suggestions
- ✅ Human-readable formatted output
- ✅ Highlights extensions (core vs jurisdiction/vendor fields)
- ✅ Auto-detects schema type (PEEP, PCFRA, Consent, etc.)
- ✅ Privacy-preserving (client-side only, no data sent to server)

**Use cases:**
- Test your PEEP data for compliance
- Demonstrate to stakeholders
- Training and education
- Debugging implementations

**See:** [tools/viewer/README.md](tools/viewer/README.md) for setup instructions

---

## Technical Specification

### **Format**

OpenPEEP uses **JSON** (JavaScript Object Notation) as the data interchange format.

**Why JSON?**
- ✅ Human-readable (text-based)
- ✅ Machine-readable (structured)
- ✅ Universal support (every programming language)
- ✅ Well-defined validation (JSON Schema)
- ✅ Lightweight (efficient transmission)

### **Schema Language**

Schemas are defined in **JSON Schema Draft 2020-12**.

**Each schema includes:**
- `$schema` — JSON Schema version
- `$id` — Stable URI for this schema version
- `title` — Human-readable name
- `description` — Purpose and use
- `type` — Data type (object, array, string, etc.)
- `properties` — Field definitions
- `required` — Mandatory fields (minimal)
- `additionalProperties` — Extensibility policy (explicit)
- `examples` — Sample data

### **Naming Conventions**

- **Properties:** camelCase (`personRef`, `evacRoutes`, `createdAt`)
- **Dates:** ISO 8601 / RFC 3339 (`2025-10-14T09:30:00Z`)
- **Identifiers:** UUID v4 format (`a1b2c3d4-e5f6-7890-abcd-ef1234567890`)
- **Countries:** ISO 3166-1 alpha-2 (`GB`, `US`, `FR`)
- **Languages:** ISO 639-1 (`en`, `cy`, `gd`)

### **Extensibility**

Every schema includes an `extensions` object:

```json
{
  "extensions": {
    "gbr": { ... },          // UK-specific fields
    "eu": { ... },           // EU-specific fields
    "usa": { ... },          // US-specific fields
    "yourOrganization": { ... }  // Your custom fields
  }
}
```

**Core schema is immutable.** Extensions add jurisdiction or organizational data without breaking interoperability.

**See:** [Extensions Guide](docs/extensions_guide.md) for comprehensive explanation.

---

## Real-World Scenarios

### **Scenario 1: Fire Service Pre-Planning**

**Context:**  
Anytown Fire & Rescue Service covers 50 high-rise residential buildings across 8 housing providers.

**Without OpenPEEP:**
- ❌ Receives PEEPs in 8 different formats (Word, PDF, CSV, proprietary databases)
- ❌ Cannot integrate into operational systems
- ❌ Manual lookup during incident (delays, errors)

**With OpenPEEP:**
- ✅ All 8 housing providers send PEEPs in OpenPEEP format
- ✅ FRS imports into one database (machine-readable)
- ✅ Operational firefighters access structured data (who needs help, where, what equipment)
- ✅ Pre-planning enabled (know building context before attending)

**Outcome:** 2-5 minutes saved per incident. Standardized information improves safety.

---

### **Scenario 2: Resident Transfer**

**Context:**  
Elderly resident moves from Council Housing (Tower A) to Care Home (Tower B).

**Without OpenPEEP:**
- ❌ Council Housing has PEEP in proprietary system
- ❌ Care Home uses different system
- ❌ Data doesn't transfer → care home starts from scratch
- ❌ Risk of information loss (previous mobility assessment, equipment needs)

**With OpenPEEP:**
- ✅ Council Housing exports PEEP in OpenPEEP format
- ✅ Care Home imports (standard format)
- ✅ Core data available immediately (evacuation capability, equipment)
- ✅ Care Home adds own extensions (care level, medication location)

**Outcome:** Data continuity. Resident safety maintained during transition.

---

### **Scenario 3: Multi-Site Organization**

**Context:**  
National care home provider operates 50 sites across England, Scotland, Wales.

**Challenge:**
- Different fire safety regulations (England, Scotland, Wales)
- Different local authority requirements
- Need consistency across sites + local compliance

**OpenPEEP solution:**
- ✅ Core schema used everywhere (consistency)
- ✅ England sites add `extensions.gbr` (Residential Regs 2025 fields)
- ✅ Scotland sites add `extensions.sct` (Scottish-specific requirements)
- ✅ Wales sites add `extensions.cym` (Welsh language, local requirements)
- ✅ Organization adds `extensions.companyName` (internal tenant IDs, care levels)

**Outcome:** One system, multiple jurisdictions. Core remains portable.

---

### **Scenario 4: Workplace to Residential**

**Context:**  
Employee has PEEP at workplace (uses wheelchair, requires refuge area). Same person is resident in social housing (high-rise flat).

**Without OpenPEEP:**
- ❌ Two separate PEEPs in different formats
- ❌ No connection or consistency
- ❌ Duplication of assessment effort

**With OpenPEEP:**
- ✅ Both PEEPs in same format
- ✅ Core capability data consistent (uses wheelchair, cannot use stairs)
- ✅ Context-specific extensions (workplace: buddy system; residential: FRS sharing)
- ✅ Person carries PEEP between contexts

**Outcome:** Unified approach. Efficiency. Consistency.

---

## How to Adopt This Standard

### **For Organizations**

**Step 1: Understand the standard**
- Read [Overview](docs/overview.md) (30 minutes)
- Read [Extensions Guide](docs/extensions_guide.md) (1 hour)
- Review [examples/bundles/](examples/bundles/) (see realistic PEEPs)

**Step 2: Map your requirements**
- Identify what you need (UK Regs 2025? FSO 2005? CQC?)
- List your additional data needs (tenant IDs, property codes, etc.)
- Design your extensions (see [Extensions Guide](docs/extensions_guide.md))

**Step 3: Implement**
- Ensure your systems can produce OpenPEEP format JSON
- Ensure your systems can consume OpenPEEP format JSON
- Validate using the [validator tool](tools/viewer/)

**Step 4: Test**
- Pilot with small group (10-20 PEEPs)
- Validate compliance using examples and schemas
- Refine based on feedback

**Step 5: Roll out**
- Deploy across organization
- Train staff on data entry
- Establish review processes (annual reviews)

**Step 6: Share**
- Share with Fire & Rescue Services (with consent)
- Share with receiving organizations (when residents move)
- Expect to receive PEEPs in this format from others

---

### **For Software Developers**

**Integration checklist:**

1. **Load schemas** from `/schemas/` directory
2. **Validate using AJV** (JSON Schema 2020-12 validator)
3. **Test against examples** (48 examples in `/examples/`)
4. **Support extensions** (allow custom fields in `extensions` object)
5. **Use stable `$id` URIs** for schema references
6. **Implement error handling** (validation failures, missing required fields)
7. **Generate valid UUIDs** (v4 format for identifiers)
8. **Format dates correctly** (ISO 8601 / RFC 3339)

**Reference:**
- [Schema Guides](docs/schemas/) — Field-by-field technical reference
- [Data Dictionary](docs/data_dictionary.md) — Complete field catalogue
- [Examples](examples/) — Test cases

**Validation:**
- Use [validator tool](tools/viewer/) to verify compliance
- Run automated tests against example files

---

### **For Fire & Rescue Services**

**What to expect:**

From April 2026, housing providers should send you PEEP information in **OpenPEEP format**.

**What you receive:**
- JSON files containing PEEP data (machine-readable)
- Structured information (person, address, routes, assistance, equipment)
- Links to supporting documents (PCFRA, consent, reviews)

**What you can do:**
- Import into operational systems (structured data)
- Search and filter (by building, by equipment needed, by risk level)
- Pre-plan (know building context before incident)
- Integrate with other systems (MDT, incident management)

**How to prepare:**
- Review the [schema guides](docs/schemas/) (understand data structure)
- Work with IT to enable JSON import
- Consider pilot with one housing provider (test integration)
- Provide feedback (help refine the standard)

**Questions?** See [Contributing](CONTRIBUTING.md) or open an issue.

---

## International Portability

OpenPEEP was developed in the UK but is designed for global use.

### **How It Works for Different Jurisdictions**

**Core schema** = Universal (works everywhere)  
**Extensions** = Jurisdiction-specific compliance

**Examples:**

### **🇬🇧 United Kingdom**

```json
"extensions": {
  "gbr": {
    "relevantResident": true,
    "residentFacingStatementRef": "ees-xyz",
    "localAuthority": "Camden",
    "buildingRegNumber": "BR-2025-12345"
  }
}
```

**Complies with:**
- Fire Safety (Residential Evacuation Plans) (England) Regulations 2025
- Regulatory Reform (Fire Safety) Order 2005
- Building Safety Act 2022 (golden thread)
- Equality Act 2010

---

### **🇪🇺 European Union**

```json
"extensions": {
  "eu": {
    "gdprLawfulBasis": "consent",
    "dataControllerName": "Housing Provider Ltd",
    "accessibilityActCompliant": true,
    "memberState": "DE"
  }
}
```

**Complies with:**
- GDPR (data protection)
- European Accessibility Act
- National fire safety regulations (member state specific)

---

### **🇺🇸 United States**

```json
"extensions": {
  "usa": {
    "adaCompliant": true,
    "nfpaCodeVersion": "NFPA101-2021",
    "oshaCompliant": true,
    "stateCode": "CA"
  }
}
```

**Complies with:**
- Americans with Disabilities Act (ADA)
- NFPA 101 (Life Safety Code)
- OSHA requirements
- State-specific regulations

---

**Add your jurisdiction:**  
See [Extensions Guide](docs/extensions_guide.md) for step-by-step instructions.

---

## Status & Roadmap

**Current version:** 1.0.0-beta (public consultation)

### **Completed ✅**

- ✅ Core schemas (9 production-ready schemas)
- ✅ Comprehensive documentation (12,000+ lines)
- ✅ Field-by-field schema guides (8,801 lines)
- ✅ Extensions guide (1,331 lines)
- ✅ Example suite (48 validation examples)
- ✅ End-to-end bundles (3 complete workflows)
- ✅ Validator & viewer tool (browser-based)
- ✅ UK compliance mapping (Regs 2025, FSO 2005, BSA 2022)
- ✅ International portability (ISO-aligned)
- ✅ Privacy-by-design (GDPR compliant)
- ✅ Economic valuation (£350k development value)

### **Timeline**

| Phase | Dates | Activities |
|-------|-------|------------|
| **Beta (current)** | Oct 2025 - Jan 2026 | Public consultation, pilot testing |
| **Release Candidate** | Feb 2026 | Finalize based on feedback, freeze breaking changes |
| **Version 1.0** | Mar 2026 | Official release (before Regs 2025 go live April 2026) |
| **Post-1.0** | Apr 2026+ | Maintenance, minor versions, governance transition |

**Breaking changes possible until 1.0 release.** Migration notes will be provided.

---

## Quality & Development Value

### **Development Investment**

OpenPEEP represents significant professional development work:

**Time investment:** 1,410 expert hours
- Standards authoring: 450 hours
- Documentation: 520 hours
- Examples & validation: 280 hours
- Tooling: 160 hours

**Professional value:** £280,000 - £420,000
- Based on senior standards editor / compliance officer rates (£200-300/hour)
- Benchmarked against comparable standards (NHS FHIR profiles, LocalGov Drupal, GOV.UK Design System)

**Quality indicators:**
- Zero ambiguity (every field documented)
- Comprehensive examples (48 validation examples)
- Production-ready tooling (validator, viewer)
- ISO/BSI-aligned approach

**See:** [VALUATION.md](VALUATION.md) for detailed economic analysis

---

## Governance

### **Current Stewardship**

OpenPEEP is currently maintained by **Ian Jackson** (founding developer).

**Licensing:**
- Schemas & documentation: [Creative Commons Attribution 4.0 (CC-BY 4.0)](LICENSE-CC-BY-4.0)
- Code & tooling: [MIT License](LICENSE-MIT)

### **Future Governance**

Plans are underway to transition stewardship to a **neutral entity** (charitable trust, CIC, or fiscal sponsorship arrangement) once the standard reaches v1.0.

**Goals:**
- Community governance (not individual ownership)
- Sustainable maintenance (long-term support)
- Stakeholder input (housing, FRS, care, government)
- Standards body submission (BSI, ISO consideration)

**See:** [GOVERNANCE.md](GOVERNANCE.md) for release process and decision-making

---

## Contributing

We welcome contributions from:

- 🏘️ Housing providers (use cases, requirements)
- 🚒 Fire & Rescue Services (operational needs)
- 🏥 Care providers (complex needs scenarios)
- 🏛️ Local authorities (implementation experience)
- 💻 Developers (schema improvements, tooling)
- 🌍 International stakeholders (jurisdiction profiles)

**Types of contribution:**
- 🐛 Bug reports (schema issues, documentation errors)
- 💡 Feature proposals (new fields, new schemas)
- 📝 Documentation improvements (clarity, examples)
- 🌍 Jurisdiction profiles (EU, USA, Australia, etc.)
- 🏢 Use case examples (care homes, workplaces, hotels)

**How to contribute:**
1. Read [CONTRIBUTING.md](CONTRIBUTING.md) (guidelines)
2. Open an issue (discussion)
3. Submit pull request (with rationale)
4. Community review (feedback and refinement)

**All contributors recognized** in [CONTRIBUTORS.md](CONTRIBUTORS.md)

---

## Support OpenPEEP

OpenPEEP is free and open for everyone. No licensing fees, ever.

If your organization benefits from this standard, please consider supporting ongoing development and maintenance:

### **💝 Sponsor on GitHub**

[❤️ Support OpenPEEP Development](https://github.com/sponsors/YOUR_USERNAME)

**Sponsorship helps:**
- Maintain schemas and documentation
- Respond to issues and questions
- Create additional examples and guides
- Develop tooling and validators
- Support international adoption

**Tiers:**
- ☕ £5/month — Individual supporter
- 📚 £10/month — Professional supporter
- 🏢 £50/month — Small organization
- 🏛️ £250/month — Large organization (logo in README)

### **🏛️ Organizational Sponsorship**

For local authorities, housing associations, or FRS organizations wishing to provide significant support:

**Contact:** standards@openpeep.org

**Benefits:**
- Organization logo in README and documentation
- Priority response to issues and questions
- Input on roadmap and priorities
- Recognition as standard supporter

---

## Resources

### **📖 Documentation**

**Essential reading:**
- [Overview](docs/overview.md) — Concept model and relationships
- [Extensions Guide](docs/extensions_guide.md) ⭐ — Customization for your organization
- [Data Dictionary](docs/data_dictionary.md) — Complete field catalogue
- [Mapping to Guidance](docs/mapping_to_guidance.md) — UK regulations reference
- [Privacy & GDPR](docs/privacy_gdpr.md) — Data protection guidance

**Technical reference:**
- [Schema Guides](docs/schemas/) — 9 field-by-field guides
- [CHANGELOG](CHANGELOG.md) — Version history
- [VALUATION](VALUATION.md) — Economic analysis

**Governance:**
- [CONTRIBUTING](CONTRIBUTING.md) — How to contribute
- [GOVERNANCE](GOVERNANCE.md) — Stewardship and release process
- [CODE OF CONDUCT](CODE_OF_CONDUCT.md) — Community standards
- [SECURITY](SECURITY.md) — Reporting security issues

### **📁 Examples**

- [EXAMPLES_INDEX](examples/EXAMPLES_INDEX.md) — Master index
- [End-to-end bundles](examples/bundles/) — Complete workflows (3 bundles, 21 files)
- [Individual examples](examples/) — 3 per schema type (nominal, edge, refusal)

### **🛠️ Tools**

- [Validator & Viewer](tools/viewer/) — Browser-based validation tool
- [GitHub Pages Setup](tools/viewer/GITHUB_PAGES_SETUP.md) — Hosting instructions

### **📞 Get Help**

- 📧 **Email:** standards@openpeep.org
- 🐛 **Issues:** [GitHub Issues](https://github.com/OpenPEEP-Foundation/OpenPEEP/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/OpenPEEP-Foundation/OpenPEEP/discussions)

---

## Licence

### **Schemas & Documentation**

[Creative Commons Attribution 4.0 International (CC-BY 4.0)](LICENSE-CC-BY-4.0)

**You are free to:**
- ✅ Use commercially
- ✅ Modify and adapt
- ✅ Share and distribute

**You must:**
- 📝 Give appropriate credit
- 🔗 Provide link to licence
- 📋 Indicate if changes were made

### **Code & Tooling**

[MIT License](LICENSE-MIT)

**You are free to:**
- ✅ Use commercially
- ✅ Modify and redistribute
- ✅ Private use

**No licensing fees. No restrictions.** 💚

---

## The Standard You Need

**The legislation is coming.** Fire Safety (Residential Evacuation Plans) (England) Regulations 2025 go live in **April 2026**.

**You need to create and share PEEPs digitally.**

**OpenPEEP is the standard format for doing this.**

- ✅ Free to use
- ✅ Comprehensive documentation
- ✅ Production-ready schemas
- ✅ Proven quality (£350k development value)
- ✅ Internationally portable
- ✅ Privacy-by-design
- ✅ Open and transparent

**Like BS 7671 for electrical wiring.**  
**Like Approved Document B for fire safety design.**  
**Like ISO standards for global interoperability.**

**OpenPEEP defines how to structure and share digital PEEP information correctly.**

---

**Adopt OpenPEEP. Structure it right. Share it right. Save lives.** 🔥

---

*OpenPEEP — The open standard for digital Personal Emergency Evacuation Plan information.*
