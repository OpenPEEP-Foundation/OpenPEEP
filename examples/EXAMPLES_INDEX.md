# Examples Index — OpenPEEP

This directory contains **validation examples** and **end-to-end bundles** for all OpenPEEP schemas.

---

## Directory Structure

```
examples/
├── EXAMPLES_INDEX.md          ← You are here
├── bundles/                   ← End-to-end document sets (recommended starting point)
│   ├── README.md              ← Bundle documentation
│   ├── bundle_01_*.json       ← Bundle 1: Residential RPEEP (consent given)
│   ├── bundle_02_*.json       ← Bundle 2: Residential RPEEP (consent refused)
│   └── bundle_03_*.json       ← Bundle 3: Workplace PEEP (sensory impairments)
├── ees/                       ← EES examples
│   ├── building/
│   │   └── ees_building.example.json
│   └── personal/
│       └── ees_personal.example.json
├── pcfra/                     ← PCFRA examples (to be added)
├── peep/                      ← PEEP examples (to be added)
├── consent/                   ← Consent examples (to be added)
├── person_profile/            ← Person Profile examples (to be added)
├── address/                   ← Address examples (to be added)
└── review/                    ← Review examples (to be added)
```

---

## Quick Start

### For Implementers

**Start here:** Read the [bundles/README.md](bundles/README.md) and explore the three complete end-to-end bundles:

1. **Bundle 1** — Residential RPEEP for wheelchair user (consent given, full lifecycle)
2. **Bundle 2** — Residential RPEEP with consent refused (compliance evidence)
3. **Bundle 3** — Workplace PEEP for employee with sensory impairments (reasonable adjustments)

Each bundle includes:
- Person Profile
- Address
- Consent record
- PCFRA (Personal and/or Environmental)
- PEEP (if consent given)
- EES Personal (if applicable)
- Review (lifecycle tracking)
- Manifest (cross-references and workflow sequence)

### For Validators

Validate bundles against schemas using AJV:

```bash
# Validate Bundle 1 PEEP
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/peep.schema.json \
  -d examples/bundles/bundle_01_peep.json

# Validate Bundle 1 PCFRA Person
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.person.schema.json \
  -d examples/bundles/bundle_01_pcfra_person.json

# Validate Bundle 1 Consent
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d examples/bundles/bundle_01_consent_given.json
```

---

## Bundle Summaries

### Bundle 1: Residential RPEEP — Wheelchair User (Consent Given)

**Documents:** 9 files (person_profile, address, consent_given, pcfra_person, pcfra_environment, peep, ees_personal, review, manifest)

**Scenario:**
- Alex Thompson (fictitious), manual wheelchair user
- Flat 12A, Churchill House, Manchester (high-rise)
- Consent given for PCFRA, PEEP, EES, and FRS sharing
- PEEP created with evacuation lift route (BS EN 81-76 compliant)
- EES Personal delivered to resident
- Annual review: confirmed, no change

**Demonstrates:**
- Full RPEEP lifecycle (assessment → consent → planning → delivery → review)
- UK Residential Regulations 2025 compliance (Regs 3-5, 10, 12)
- Building Safety Act 2022 (golden thread linkage)
- Equality Act 2010 (reasonable adjustments: evacuation lift, assistance)
- Cross-referencing via UUIDs

**Files:**
- `bundle_01_person_profile.json`
- `bundle_01_address.json`
- `bundle_01_consent_given.json`
- `bundle_01_pcfra_person.json`
- `bundle_01_pcfra_environment.json`
- `bundle_01_peep.json`
- `bundle_01_ees_personal.json`
- `bundle_01_review.json`
- `bundle_01_manifest.json`

---

### Bundle 2: Residential RPEEP — Consent Refused

**Documents:** 5 files (person_profile, address, consent_refused, pcfra_person, manifest)

**Scenario:**
- Jordan Lee (fictitious), older adult with mobility limitations
- Flat 5, Oak Tower, Leeds
- Consent refused for detailed PCFRA and PEEP
- Minimal assessment captured (phone interview, low confidence)
- No PEEP or EES created
- Duty holder records refusal; will re-offer consent periodically

**Demonstrates:**
- Consent refusal handling (UK Residential Regs 2025, Reg 4)
- Compliance evidence (offer recorded, refusal documented)
- Minimal data capture when consent refused
- Duty to re-offer consent at periodic intervals

**Files:**
- `bundle_02_person_profile.json`
- `bundle_02_address.json`
- `bundle_02_consent_refused.json`
- `bundle_02_pcfra_person.json`
- `bundle_02_manifest.json`

---

### Bundle 3: Workplace PEEP — Employee with Sensory Impairments

**Documents:** 7 files (person_profile, address, consent_given, pcfra_person, peep, review, manifest)

**Scenario:**
- Sam Patel (fictitious), employee with profound hearing loss and low vision
- Innovation Centre, Canary Wharf, London (workplace)
- Consent given for workplace PEEP
- PEEP created with communication adjustments (visual alarms, tactile wayfinding, buddy system)
- 6-month review: amended due to role change (new floor location, new buddy)

**Demonstrates:**
- Workplace PEEP (FSO 2005, Equality Act 2010)
- Reasonable adjustments (visual alarm beacons, high-contrast tactile wayfinding, BSL interpreter)
- Buddy system for evacuation assistance
- Responsive review cycle (triggered by change of circumstance)
- PEEP amendment (new document supersedes old)

**Files:**
- `bundle_03_person_profile.json`
- `bundle_03_address.json`
- `bundle_03_consent_given.json`
- `bundle_03_pcfra_person.json`
- `bundle_03_peep.json`
- `bundle_03_review.json`
- `bundle_03_manifest.json`

---

## Cross-References and Linkage

All bundles demonstrate **schema linkage via UUIDs**:

```
Person Profile (personRef: "person-alex-thompson-uuid-001")
    ↓
Consent (personRef: "person-alex-thompson-uuid-001")
    ↓
PCFRA Person (personRef: "person-alex-thompson-uuid-001", consentRef: "consent-alex-thompson-uuid-001")
    ↓
PEEP (personProfile.personRef: "person-alex-thompson-uuid-001", references.pcfraId: "pcfra-person-alex-thompson-uuid-001")
    ↓
EES Personal (person_profile_ref: "person-alex-thompson-uuid-001", peep_document_id: "peep-alex-thompson-uuid-001")
    ↓
Review (reviewed PEEP; new_document_ref if amended/superseded)
```

Each bundle includes a **manifest JSON** mapping all cross-references.

---

## Data Minimisation & Privacy

All examples use **fictitious, pseudonymised data**:

- ✅ **Fictitious names** (Alex Thompson, Jordan Lee, Sam Patel)
- ✅ **Fictitious addresses** (Churchill House, Oak Tower, Innovation Centre)
- ✅ **Functional capabilities only** (no medical diagnoses)
- ✅ **Minimal identifiers** (UUIDs, not real personal data)
- ✅ **Pseudonymised contact details** (example.com email addresses, placeholder phone numbers)

**Do not use real personal data in examples or testing.**

---

## Validation

### Validate All Bundles

```bash
# Run all validation tests (requires npm install)
npm run test:all

# Validate specific schemas
npm run test:peep
npm run test:pcfra
npm run test:consent
npm run test:ees-building
npm run test:ees-person
```

### Manual Validation (Individual Files)

```bash
# Install dependencies
npm install

# Validate a single document
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/peep.schema.json \
  -d examples/bundles/bundle_01_peep.json
```

---

## Contributing Examples

To contribute additional examples:

1. **Create individual schema examples** in the relevant subdirectories (`pcfra/`, `peep/`, `consent/`, etc.)
2. **Create bundles** showing end-to-end workflows
3. **Ensure all examples validate** against schemas (`npm run test:all`)
4. **Use fictitious, pseudonymised data** (no real personal data)
5. **Document cross-references** in manifest files
6. **Submit via pull request** (see [CONTRIBUTING.md](../CONTRIBUTING.md))

---

## Schema Documentation

For schema documentation and field definitions, see:

- **[Data Dictionary](../docs/data_dictionary.md)** — Authoritative field catalogue
- **[Overview](../docs/overview.md)** — Concept model and schema relationships
- **[Mapping to Guidance](../docs/mapping_to_guidance.md)** — UK regulation anchors
- **[Privacy & GDPR](../docs/privacy_gdpr.md)** — Data minimisation and retention

---

## Questions?

- **Issues:** [GitHub Issues](https://github.com/OpenPEEP-Foundation/OpenPEEP/issues) *(replace with actual repo URL)*
- **Email:** standards@openpeep.org *(update as needed)*

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

