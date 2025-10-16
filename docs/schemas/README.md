# Schema Guides — Complete Field-Level Reference

**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Purpose

This directory contains **comprehensive, field-by-field reference guides** for every OpenPEEP schema.

Each guide provides:
- 📖 **Complete field reference** (every field explained in detail)
- 🎯 **Quick start** (minimal viable examples)
- 🌍 **Core vs Extensions pattern** (international portability explained)
- 🏛️ **Jurisdiction profiles** (UK, EU, USA examples)
- ✅ **Validation guidance** (common errors and fixes)
- ❓ **FAQs** (common questions answered)

**Target audience:**
- **Developers** building OpenPEEP systems
- **Implementers** creating PEEPs and PCFRAs
- **Standards editors** understanding design decisions
- **International adopters** adapting to local jurisdictions

---

## Schema Guide Index

### Core Planning Schemas

| Schema | Guide | Description | Lines |
|--------|-------|-------------|-------|
| **PEEP** | [peep_schema_guide.md](peep_schema_guide.md) | Personal Emergency Evacuation Plan - complete evacuation plan for an individual | 1,538 |
| **PCFRA (Person)** | [pcfra_person_schema_guide.md](pcfra_person_schema_guide.md) | Personal functional capability assessment (mobility, sensory, cognitive) | 1,164 |
| **PCFRA (Environment)** | [pcfra_environment_schema_guide.md](pcfra_environment_schema_guide.md) | Environmental/dwelling risk assessment (hoarding, electrical, safeguarding) | 816 |

### Supporting Schemas

| Schema | Guide | Description | Lines |
|--------|-------|-------------|-------|
| **Consent** | [consent_schema_guide.md](consent_schema_guide.md) | Consent records (given/refused/withdrawn) | 1,083 |
| **Review** | [review_schema_guide.md](review_schema_guide.md) | Lifecycle review tracking (annual reviews, amendments) | 865 |
| **Person Profile** | [person_profile_schema_guide.md](person_profile_schema_guide.md) | Minimal person identity anchor (no medical data) | 726 |
| **Address** | [address_schema_guide.md](address_schema_guide.md) | Portable international addressing schema | 739 |

### Resident-Facing Schemas

| Schema | Guide | Description | Lines |
|--------|-------|-------------|-------|
| **EES (Building)** | [ees_building_schema_guide.md](ees_building_schema_guide.md) | Building-wide plain-language evacuation statement | 684 |
| **EES (Personal)** | [ees_personal_schema_guide.md](ees_personal_schema_guide.md) | Individual plain-language evacuation instructions | 778 |

**Total:** 9 comprehensive guides, **8,801 lines** of documentation

---

## How to Use These Guides

### If You're New to OpenPEEP

**Start here:**
1. Read [PEEP Schema Guide](peep_schema_guide.md) - the main document type
2. Read [Consent Schema Guide](consent_schema_guide.md) - critical for compliance
3. Read [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - how to assess capabilities

**Then:**
- Explore the other guides as needed
- Refer to field reference sections when implementing

### If You're Implementing UK RPEEP

**Required reading:**
1. [Consent Schema Guide](consent_schema_guide.md) - UK Reg 4 compliance
2. [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - assessment workflow
3. [PEEP Schema Guide](peep_schema_guide.md) - especially UK extensions section
4. [Review Schema Guide](review_schema_guide.md) - UK Reg 5 annual reviews
5. [EES Personal Schema Guide](ees_personal_schema_guide.md) - UK Reg 10 information duty

### If You're Implementing Internationally

**Focus on:**
- **Core vs Extensions Pattern** sections in each guide
- **International Portability** examples
- **Jurisdiction Profiles** (EU, USA examples)

### If You're Building Software

**Reference material:**
- **Quick Start** sections - minimal valid examples
- **Complete Field Reference** - every field explained
- **Validation** sections - AJV usage and common errors
- **FAQs** - edge cases and tricky scenarios

---

## What Each Guide Contains

Every guide follows the same structure:

### 1. Overview
- What is this schema?
- Why does it exist?
- When do you use it?
- Who uses it?

### 2. Quick Start
- Minimal valid example (required fields only)
- Standard example (common usage)
- Advanced example (comprehensive)

### 3. Complete Field Reference
- **Every field explained:**
  - Type and constraints
  - Required or optional
  - What it is (definition)
  - Why it exists (purpose)
  - How to use it (usage guidance)
  - Examples (field in isolation)
  - Common mistakes
  - Legal significance (where applicable)

### 4. Core vs Extensions Pattern
- What belongs in core (portable)
- What belongs in extensions (jurisdiction-specific)
- **Concrete examples:**
  - UK implementation (`extensions.gbr`)
  - EU implementation (`extensions.eu`)
  - USA implementation (`extensions.usa`)
  - Vendor extensions
- How to create your own jurisdiction profile

### 5. Complete Examples
- Real-world scenarios
- Links to example files in `/examples/`

### 6. Validation
- AJV command-line usage
- Common validation errors and fixes

### 7. FAQs
- Common questions
- Edge cases
- Best practices
- Compliance guidance

---

## Schema Relationships

```
Person Profile ─────┐
                    │
                    ├──> PCFRA (Person) ─────┐
                    │                        │
Consent ────────────┴──> PCFRA (Environment) ┴──> PEEP ──> EES (Personal)
                                                    │            │
Address ─────────────────────────────────────────────┘            │
                                                                  │
Review ───────────────────────────────────────────────────────────┘
                                                                  │
EES (Building) ───────────────────────────────────────────────────┘
```

**Workflow:**
1. **Person Profile** + **Consent** → enables assessment
2. **PCFRA (Person)** + **PCFRA (Environment)** → assess capability and risks
3. **PEEP** → create evacuation plan (informed by PCFRAs)
4. **EES (Personal)** → plain-language version for resident (derived from PEEP)
5. **EES (Building)** → building-wide context
6. **Review** → lifecycle tracking (embedded in all documents)
7. **Address** → location context (embedded in PEEP, EES)

---

## Key Concepts Explained Across Guides

### No Medical Diagnoses

**Covered in:**
- [PCFRA Person Guide](pcfra_person_schema_guide.md) - detailed explanation
- [PEEP Guide](peep_schema_guide.md) - mentioned in FAQs

**Principle:**  
Record **functional capability**, not diagnoses.

- ❌ "Has Parkinson's disease"
- ✅ "Mobility transfer: stand_pivot_with_support"

---

### Core vs Extensions Pattern

**Covered in every guide:**
- What core is (portable, international)
- What extensions are (jurisdiction-specific)
- How to use extensions
- Concrete examples (UK, EU, USA)

**Critical for:**
- International adoption
- Avoiding jurisdiction lock-in
- Local customization without breaking core

**Best explained in:**
- [PEEP Guide](peep_schema_guide.md) - most comprehensive
- [Address Guide](address_schema_guide.md) - international addressing

---

### Consent Lifecycle

**Covered in:**
- [Consent Guide](consent_schema_guide.md) - comprehensive lifecycle flows
- [PEEP Guide](peep_schema_guide.md) - how consent links to PEEPs

**Flows explained:**
- Given → Still active
- Refused → Re-offered → Given
- Given → Withdrawn (partial)

---

### Data Minimisation

**Covered in:**
- [Person Profile Guide](person_profile_schema_guide.md) - detailed decision tree
- [PCFRA Person Guide](pcfra_person_schema_guide.md) - functional vs diagnostic
- [Consent Guide](consent_schema_guide.md) - minimal data capture

**Principle:**  
Collect **only what's necessary** for evacuation planning.

---

### Plain Language

**Covered in:**
- [EES Building Guide](ees_building_schema_guide.md) - plain language checklist
- [EES Personal Guide](ees_personal_schema_guide.md) - writing for residents

**Principle:**  
Write for **11-year-old reading age** (UK government standard).

---

## Documentation Hierarchy

```
README.md (root)
    │
    ├── docs/overview.md ──────────────── Concept model & relationships
    │
    ├── docs/data_dictionary.md ───────── Field catalogue (tabular)
    │
    ├── docs/mapping_to_guidance.md ───── UK regulations mapping
    │
    ├── docs/privacy_gdpr.md ──────────── Privacy & data protection
    │
    └── docs/schemas/ ──────────────────── THIS DIRECTORY
            │
            ├── README.md ──────────────── This file (index)
            │
            ├── peep_schema_guide.md ──── Tutorial-level, field-by-field
            ├── consent_schema_guide.md
            ├── pcfra_person_schema_guide.md
            ├── pcfra_environment_schema_guide.md
            ├── person_profile_schema_guide.md
            ├── address_schema_guide.md
            ├── review_schema_guide.md
            ├── ees_building_schema_guide.md
            └── ees_personal_schema_guide.md
```

**When to use each:**

**Overview** → Big picture, how schemas relate  
**Data Dictionary** → Quick field lookup (table format)  
**Mapping to Guidance** → UK regulation compliance  
**Privacy & GDPR** → Legal basis, retention, rights  
**Schema Guides** → Deep dive, field-by-field, with examples (YOU ARE HERE)

---

## Completeness Checklist

### Schema Coverage

- ✅ **9 schemas** in repository
- ✅ **9 comprehensive guides** created
- ✅ **100% coverage**

### Content Quality

- ✅ **Every field explained** (what, why, how, examples)
- ✅ **Core vs Extensions pattern** (in every guide)
- ✅ **Jurisdiction profiles** (UK, EU, USA examples)
- ✅ **Validation guidance** (AJV usage, common errors)
- ✅ **FAQs** (common questions answered)
- ✅ **Cross-references** (links to related guides)

### Tutorial Level

- ✅ **Assumes zero prior knowledge**
- ✅ **Step-by-step examples**
- ✅ **Common mistakes highlighted**
- ✅ **Plain language explanations**
- ✅ **Visual examples** (JSON snippets showing before/after)

---

## Next Steps

### For Implementers

1. **Start:** [PEEP Schema Guide](peep_schema_guide.md)
2. **Understand compliance:** [Consent Schema Guide](consent_schema_guide.md)
3. **Build assessment workflow:** [PCFRA Person](pcfra_person_schema_guide.md) + [PCFRA Environment](pcfra_environment_schema_guide.md)
4. **Implement reviews:** [Review Schema Guide](review_schema_guide.md)
5. **Create resident materials:** [EES Personal Schema Guide](ees_personal_schema_guide.md)

### For International Adopters

1. **Read Core vs Extensions sections** in each guide
2. **Study jurisdiction profile examples** (UK, EU, USA)
3. **Create your own jurisdiction profile** (follow the pattern)
4. **Document your extensions** (contribute back to standard!)

### For Developers

1. **Use Quick Start** sections for rapid prototyping
2. **Reference Field Reference** sections during development
3. **Use Validation** sections for testing
4. **Check FAQs** for edge cases

---

## Contributing

Found an error? Have a suggestion?

- **Open an issue:** [GitHub Issues](https://github.com/OpenPEEP-Foundation/OpenPEEP/issues) *(update with actual URL)*
- **Submit a PR:** Corrections and improvements welcome
- **Ask questions:** standards@openpeep.org *(update as needed)*

---

## Change Log

| Date | Version | Change | Guides Updated |
|------|---------|--------|----------------|
| 2025-10-14 | 1.0.0-beta | Initial schema guides created | All 9 guides |

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

---

## Quick Reference Card

### Schemas by Use Case

**Creating a PEEP:**
1. Get consent → [Consent Guide](consent_schema_guide.md)
2. Assess person → [PCFRA Person Guide](pcfra_person_schema_guide.md)
3. Assess environment → [PCFRA Environment Guide](pcfra_environment_schema_guide.md)
4. Create PEEP → [PEEP Guide](peep_schema_guide.md)
5. Create resident instructions → [EES Personal Guide](ees_personal_schema_guide.md)

**Annual review:**
→ [Review Guide](review_schema_guide.md)

**Building-wide information:**
→ [EES Building Guide](ees_building_schema_guide.md)

**Identity management:**
→ [Person Profile Guide](person_profile_schema_guide.md)

**Location management:**
→ [Address Guide](address_schema_guide.md)

