# Example Bundles — End-to-End Document Sets

This directory contains **complete end-to-end example bundles** demonstrating how OpenPEEP schemas work together in realistic scenarios.

Each bundle represents a **complete lifecycle** from initial assessment through to evacuation planning and review.

---

## Purpose

These bundles demonstrate:

- **Schema relationships** (how documents reference each other)
- **Real-world workflows** (PCFRA → Consent → PEEP → EES → Review)
- **Cross-referencing** (UUIDs and references between documents)
- **Compliance patterns** (UK Residential Regs 2025, FSO 2005, Equality Act 2010)
- **Privacy-by-design** (data minimisation, consent management)

---

## Bundle Index

### Bundle 1: Residential RPEEP — Wheelchair User (Consent Given)

**Scenario:**
- **Person:** Alex Thompson (fictitious), manual wheelchair user
- **Location:** Flat 12A, Churchill House, Manchester (high-rise residential)
- **Context:** Residential RPEEP under Fire Safety (Residential Evacuation Plans) (England) Regulations 2025
- **Consent:** Given for PCFRA, PEEP, EES, and sharing with FRS
- **Outcome:** PEEP created with evacuation lift route; EES Personal delivered
- **Review:** Annual review confirmed with no changes

**Documents in bundle:**
1. `bundle_01_person_profile.json` — Person Profile for Alex Thompson
2. `bundle_01_address.json` — Address for Flat 12A, Churchill House
3. `bundle_01_consent_given.json` — Consent record (given)
4. `bundle_01_pcfra_person.json` — Personal PCFRA (functional capability)
5. `bundle_01_pcfra_environment.json` — Environmental PCFRA (dwelling assessment)
6. `bundle_01_peep.json` — PEEP (evacuation plan with evacuation lift route)
7. `bundle_01_ees_personal.json` — EES Personal (plain-language instructions)
8. `bundle_01_review.json` — Annual review (confirmed, no change)
9. `bundle_01_manifest.json` — Manifest showing all document relationships

---

### Bundle 2: Residential RPEEP — Consent Refused

**Scenario:**
- **Person:** Jordan Lee (fictitious), older adult with mobility limitations
- **Location:** Flat 5, Oak Tower, Leeds
- **Context:** Residential RPEEP; resident refuses consent for PEEP creation
- **Consent:** Refused (offer recorded)
- **Outcome:** No PEEP created; refusal documented; duty to re-offer consent periodically

**Documents in bundle:**
1. `bundle_02_person_profile.json` — Person Profile for Jordan Lee
2. `bundle_02_address.json` — Address for Flat 5, Oak Tower
3. `bundle_02_consent_refused.json` — Consent record (refused)
4. `bundle_02_pcfra_person.json` — Personal PCFRA (limited, based on self-report)
5. `bundle_02_manifest.json` — Manifest (note: no PEEP or EES created)

---

### Bundle 3: Workplace PEEP — Employee with Sensory Impairments

**Scenario:**
- **Person:** Sam Patel (fictitious), employee with hearing and vision impairments
- **Location:** Office Building, Central London (workplace)
- **Context:** Workplace PEEP under FSO 2005 and Equality Act 2010
- **Consent:** Given for workplace PEEP (employer duty)
- **Outcome:** PEEP created with communication adjustments (visual alarms, tactile guidance)

**Documents in bundle:**
1. `bundle_03_person_profile.json` — Person Profile for Sam Patel
2. `bundle_03_address.json` — Address for workplace
3. `bundle_03_consent_given.json` — Consent record (given)
4. `bundle_03_pcfra_person.json` — Personal PCFRA (sensory impairments)
5. `bundle_03_peep.json` — PEEP (workplace evacuation plan)
6. `bundle_03_review.json` — 6-month review (amended due to change in role/location)
7. `bundle_03_manifest.json` — Manifest

---

## How to Use These Bundles

### Validation

Validate individual documents against their schemas:

```bash
# Validate Person Profile
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d examples/bundles/bundle_01_person_profile.json

# Validate PCFRA Person
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.person.schema.json \
  -d examples/bundles/bundle_01_pcfra_person.json

# Validate PEEP
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/peep.schema.json \
  -d examples/bundles/bundle_01_peep.json
```

### Understanding References

Each document uses **UUIDs** to reference related documents:

- **Person Profile** → `personRef` (e.g., `"person-alex-thompson-uuid"`)
- **PCFRA** → `personRef` (links to Person Profile)
- **PCFRA** → `consentRef` (links to Consent record)
- **PEEP** → `personProfile` (embeds or references Person Profile)
- **PEEP** → `consent` (embeds or references Consent record)
- **PEEP** → `references.pcfraId` (links to source PCFRA)
- **EES Personal** → `person_profile_ref` (links to Person Profile)
- **EES Personal** → `peep_document_id` (links to source PEEP)
- **Review** → `new_document_ref.document_id` (links to superseding document)

### Manifest Files

Each bundle includes a **manifest JSON** showing:

- All documents in the bundle
- Cross-references (UUIDs linking documents)
- Workflow sequence (assessment → consent → planning → delivery → review)

---

## Data Minimisation Notice

All examples use **fictitious, pseudonymised data**:

- ✅ **Fictitious names** (Alex Thompson, Jordan Lee, Sam Patel)
- ✅ **Fictitious addresses** (Churchill House, Oak Tower)
- ✅ **Functional capabilities only** (no medical diagnoses)
- ✅ **Minimal identifiers** (UUIDs, not real personal data)

**Do not use real personal data in examples or testing.**

---

## Scenarios Explained

### Bundle 1: Full RPEEP Lifecycle

This bundle demonstrates the **complete residential RPEEP workflow**:

1. **Assessment phase:**
   - Housing officer conducts in-person PCFRA (personal + environmental)
   - Functional capability recorded (manual wheelchair user, cannot use stairs)
   - Environmental risks assessed (no significant concerns)

2. **Consent phase:**
   - Resident offered consent for PCFRA, PEEP, EES, and FRS sharing
   - Consent given via written method

3. **Planning phase:**
   - PEEP created with primary route via evacuation lift (BS EN 81-76 compliant)
   - Communications adjustments specified (hearing ability: hears audible)
   - Assistance recorded (concierge team, 2 persons if lift unavailable)

4. **Delivery phase:**
   - EES Personal created in plain language
   - Delivered to resident via door drop
   - Acknowledgement recorded

5. **Review phase:**
   - Annual review conducted
   - Outcome: confirmed, no change
   - Next review due in 12 months

### Bundle 2: Consent Refused

This bundle demonstrates **refusal handling**:

1. **Assessment phase:**
   - Housing officer offers PCFRA and PEEP creation
   - Limited self-report PCFRA captured (phone interview)

2. **Consent phase:**
   - Resident declines consent for detailed PCFRA and PEEP
   - Refusal recorded with reason ("Prefers to manage own evacuation")
   - Duty holder records offer; must re-offer periodically

3. **Outcome:**
   - No PEEP created
   - Resident marked as "consent refused" in housing system
   - Duty holder retains record of offer and refusal (compliance evidence)

### Bundle 3: Workplace PEEP

This bundle demonstrates **workplace context** (different from residential):

1. **Assessment phase:**
   - Employer conducts PCFRA for employee with sensory impairments
   - Functional capability: reduced hearing and low vision

2. **Consent phase:**
   - Employee consents to workplace PEEP (employer duty under FSO 2005)

3. **Planning phase:**
   - PEEP created with communication adjustments:
     - Visual alarm beacons in workstation area
     - Tactile guidance along evacuation route
     - High-contrast signage
   - Buddy system assigned (colleague on same floor)

4. **Review phase:**
   - 6-month review conducted
   - Employee moved to different floor (role change)
   - PEEP amended with new route and new buddy
   - New document supersedes old

---

## Compliance Mapping

| Bundle | UK Regulation | Demonstrates |
|--------|---------------|--------------|
| Bundle 1 | Residential Regs 2025 (Regs 3-5, 10, 12) | Full RPEEP lifecycle; consent given; EES delivery; annual review; FRS sharing |
| Bundle 1 | Building Safety Act 2022 | Golden thread linkage via UUIDs and document references |
| Bundle 1 | Equality Act 2010 | Reasonable adjustments (evacuation lift, assistance) |
| Bundle 2 | Residential Regs 2025 (Reg 4) | Consent refusal handling; offer recorded |
| Bundle 3 | FSO 2005 (Arts 8, 9, 15) | Workplace PEEP; employer duty; communication adjustments |
| Bundle 3 | Equality Act 2010 (Sec 20) | Reasonable adjustments for sensory impairments |

---

## Next Steps

- **Validate all bundles** using AJV scripts
- **Adapt bundles** for your use case (residential, workplace, care home, public building)
- **Use bundles** as test data for system integration
- **Contribute** additional scenarios via pull request

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

