# PCFRA (Environment) Schema Guide — Complete Reference

**Schema:** `pcfra.environment.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Environment Fields Explained](#environment-fields-explained)
5. [Safeguarding Fields Explained](#safeguarding-fields-explained)
6. [When to Escalate](#when-to-escalate)
7. [Complete Examples](#complete-examples)
8. [Validation](#validation)
9. [FAQs](#faqs)

---

## Overview

### What is PCFRA (Environment)?

**PCFRA (Environment)** = **Person-Centred Fire Risk Assessment (Environmental)**

It's an assessment of **dwelling/setting risks** that affect fire safety.

**What it captures:**
- ✅ Environmental/home risks (hoarding, electrical hazards, cooking risks)
- ✅ Access/egress issues (blocked exits, key management)
- ✅ Observable ignition sources
- ✅ Safeguarding context (ASB, violence, multi-agency)
- ✅ Immediate danger and action taken

**What it does NOT capture:**
- ❌ Personal capabilities (that's PCFRA Person)
- ❌ Medical diagnoses
- ❌ Detailed case management

**Think of it as:** Home safety inspection for fire risk, not full housing inspection.

### Why Separate from PCFRA (Person)?

**Different focus:**
- **PCFRA (Person):** Portable (follows person to different buildings)
- **PCFRA (Environment):** Location-specific (tied to building/unit)

**Example:**
- Same person (wheelchair user) → **one PCFRA (Person)**
- Lives in Flat A (tidy) → **one PCFRA (Environment) for Flat A** (low risk)
- Moves to Flat B (hoarding) → **new PCFRA (Environment) for Flat B** (high risk)

**Person's capability doesn't change. Environment does.**

### When Do You Use PCFRA (Environment)?

**Use when:**
- ✅ Conducting home visit (residential RPEEP)
- ✅ Assessing workplace/building safety
- ✅ Environmental risks present (hoarding, electrical, smoking)
- ✅ Safeguarding concerns (ASB, multi-agency)

**Not needed when:**
- ❌ Remote assessment (phone/digital - can't observe environment)
- ❌ Temporary location (hotel guest - not assessing the room)
- ❌ No environmental concerns

**Typical workflow:**
1. Conduct **PCFRA (Person)** - assess person's capabilities
2. Conduct **PCFRA (Environment)** - assess dwelling/setting risks (if in-person)
3. **Both inform PEEP** creation

---

## Quick Start

### Minimal PCFRA (Environment)

```json
{
  "buildingRef": "building-uuid-001",
  "assessedAt": "2025-10-14T11:00:00Z",
  "assessmentMethod": "in_person",
  "outcome": {
    "relevantResident": true,
    "peepRequired": false,
    "rationale": "No environmental risks identified. Flat tidy; clear egress."
  }
}
```

**Note:** Must have **either** `buildingRef` **or** `unitRef` (schema constraint).

### Standard PCFRA (Environment)

```json
{
  "pcfraId": "pcfra-environment-uuid-001",
  "version": "1.0.0",
  "buildingRef": "building-churchill-house-001",
  "unitRef": "unit-churchill-12a-001",
  "assessedAt": "2025-09-15T11:15:00Z",
  "assessorRole": "housing_officer",
  "assessmentMethod": "in_person",
  "confidenceLevel": "high",
  "consentRef": "consent-alex-thompson-uuid-001",
  "fireLoad": ["pathways_narrowed"],
  "ignitionSources": [],
  "detectionAlarm": [],
  "escapeRoute": ["exit_partially_blocked"],
  "heatingRisk": [],
  "oxygenRisk": [],
  "petsRisk": [],
  "asbViolenceRisk": "None reported.",
  "environmentalSafeguarding": [],
  "agencyInvolved": [],
  "immediateDanger": false,
  "immediateActionTaken": "None required.",
  "outcome": {
    "relevantResident": true,
    "peepRequired": true,
    "rationale": "No environmental risks. PEEP required due to personal capability (wheelchair user)."
  }
}
```

---

## Complete Field Reference

### Required Fields

#### `buildingRef` OR `unitRef`

**Type:** String  
**Required:** ⚠️ **Must have one** (schema constraint: `oneOf`)

**What they are:**  
Identifiers for the building or unit being assessed.

**How to use:**

**Building-level assessment (whole building):**
```json
"buildingRef": "building-churchill-house-001"
```

**Unit-level assessment (specific flat/room):**
```json
"unitRef": "unit-churchill-12a-001"
```

**Both (recommended for multi-unit buildings):**
```json
"buildingRef": "building-churchill-house-001",
"unitRef": "unit-churchill-12a-001"
```

**Common mistakes:**
- ❌ Omitting both (validation fails!)
- ❌ Using address instead of ref (use stable IDs, not postal address)
- ✅ Include both for precision

---

#### `assessedAt`

**Type:** String (ISO 8601 date-time)  
**Required:** ✅ Yes

**What it is:**  
When was the environmental assessment conducted?

**Example:**
```json
"assessedAt": "2025-10-14T11:00:00Z"
```

**Best practice:**  
Same day or close to PCFRA (Person) if conducted together.

---

#### `assessmentMethod`

**Type:** String (enum)  
**Required:** ✅ Yes  
**Allowed values:** `"phone"`, `"digital_self"`, `"in_person"`

**What it is:**  
How was the environment assessed?

**Realistically, must be `"in_person"`!**

You cannot assess:
- Hoarding (need to see it)
- Electrical risks (need to see sockets)
- Egress blockages (need to see paths)

...over the phone or digitally.

**Exceptions:**
- `"phone"` - if person describes environment (low confidence)
- `"digital_self"` - if person uploads photos (medium confidence)

**Best practice:**  
Always conduct in-person for environmental PCFRA.

```json
"assessmentMethod": "in_person",
"confidenceLevel": "high"
```

---

#### `outcome`

**Type:** Object  
**Required:** ✅ Yes

**Same as PCFRA (Person) outcome.**

**Required fields:**
- `relevantResident` ✅ (boolean)
- `peepRequired` ✅ (boolean)
- `rationale` ✅ (string)

**Example:**
```json
"outcome": {
  "relevantResident": true,
  "peepRequired": true,
  "rationale": "High environmental fire risk: multiple ignition sources, hoarding, electrical hazards, egress partially blocked. Immediate action taken. PEEP required urgently. Follow-up visit scheduled."
}
```

**Decision logic:**

**Environmental risks alone don't always trigger PEEP:**
- If person can evacuate independently **AND** low environmental risk → `peepRequired: false`
- If high environmental risk **OR** person needs assistance → `peepRequired: true`

**Combine with PCFRA (Person) for full picture.**

---

### Optional Fields

#### `environment`

**Type:** Object  
**Required:** ❌ No (but this is the main content!)

**What it is:**  
Observed environmental/home risks.

**Environment fields (top-level, arrays of enums):**
- `fireLoad`
- `ignitionSources`
- `detectionAlarm`
- `escapeRoute`
- `heatingRisk`
- `oxygenRisk`
- `petsRisk`

All fields are **optional** and accept enumerated values; multiple selections are allowed (arrays with unique items).

**See:** [Environment Fields Explained](#environment-fields-explained) below.

---

#### Safeguarding fields (top-level)

**Type:** Mixed (string, boolean, arrays)  
**Required:** ❌ No

**What they are:**  
Top-level fields for safeguarding and multi-agency context:
- `asbViolenceRisk` (string)
- `environmentalSafeguarding` (array of enums)
- `agencyInvolved` (array of enums)
- `immediateDanger` (boolean)
- `immediateActionTaken` (string)
- `referralsMade` (array of strings)

**See:** [Safeguarding Fields Explained](#safeguarding-fields-explained) below.

---

## Environment Fields Explained

### `cooking_risk`

**Type:** String  
**What it measures:** Fire risks related to cooking.

**Examples:**

**Low risk:**
```json
"cooking_risk": "None observed. Kitchen tidy; smoke alarm functional and tested."
```

**Medium risk:**
```json
"cooking_risk": "Clutter near hob observed. Resident advised to keep area clear."
```

**High risk:**
```json
"cooking_risk": "Chip pan present (high fire risk). Unattended cooking reported by neighbours. Oxygen concentrator located 2m from hob (extreme risk). Resident advised; offered chip pan replacement scheme."
```

**What to look for:**
- Clutter near hob/cooker
- Chip pan (high fire risk - use of oil)
- Unattended cooking habits
- Oxygen near cooking area (extreme danger!)
- Broken/missing smoke alarm

**Immediate action:**  
If oxygen near hob → **immediate danger**. Relocate oxygen or disable hob.

---

### `electrical_risk`

**Type:** String  
**What it measures:** Electrical fire hazards.

**Examples:**

**Low risk:**
```json
"electrical_risk": "None observed. No overloaded sockets; PAT testing up to date."
```

**High risk:**
```json
"electrical_risk": "Multiple overloaded extension sockets observed (6+ plugs in 4-way extension). Damaged cable on portable heater. Circuit isolated immediately (immediate action). Portable heater removed. Resident advised on electrical safety."
```

**What to look for:**
- Overloaded sockets/extensions
- Damaged cables (frayed, exposed wires)
- DIY wiring (unprofessional, dangerous)
- Heaters on extension leads
- Multiple adaptors stacked ("tower of doom")

**Immediate action:**  
Damaged cables, overloaded sockets → isolate circuit, remove appliance.

---

### `smoking_risk`

**Type:** String  
**What it measures:** Fire risks related to smoking.

**Examples:**

**Low risk:**
```json
"smoking_risk": "None observed. Non-smoker."
```

**Medium risk:**
```json
"smoking_risk": "Indoor smoking observed. Ashtrays present. Advised on safe disposal."
```

**High risk:**
```json
"smoking_risk": "Smoking in bed observed (cigarette burns on bedding). Careless disposal in bedroom bin (combustible materials). Resident advised on fire risks; offered smoking cessation support."
```

**What to look for:**
- Smoking in bed (extreme risk)
- Careless disposal (cigarette butts in bins)
- Smoking near oxygen (extreme danger!)
- Heavy indoor smoking (reduces alarm responsiveness due to smoke)

---

### `hoarding_risk`

**Type:** String  
**What it measures:** Hoarding affecting egress or fire load.

**Examples:**

**Low risk:**
```json
"hoarding_risk": "None observed. Flat tidy; clear pathways."
```

**Medium risk:**
```json
"hoarding_risk": "Moderate clutter; pathways mostly clear but narrowed in hallway."
```

**High risk:**
```json
"hoarding_risk": "Severe hoarding. Pathways narrowed to <60cm. Combustible load high (newspapers, cardboard stacked floor-to-ceiling). Exit partially blocked. Doors cannot close fully. Fire risk extreme. Resident offered support; multi-agency referral made."
```

**What to look for:**
- Blocked pathways
- High combustible load (paper, cardboard, textiles)
- Doors blocked (can't close = fire spread)
- Exit obstructed

**Immediate action:**  
Exit blocked → **immediate danger**. Clear pathway or arrange temporary accommodation.

---

### `accessEgress`

**Type:** String  
**What it measures:** Can they get out? Are exits blocked?

**Examples:**

**Clear:**
```json
"accessEgress": "Clear. Front door accessible; no obstructions."
```

**Partially blocked:**
```json
"accessEgress": "Exit partially blocked by items in hallway. Resident advised to clear. Will re-check at follow-up visit."
```

**Blocked:**
```json
"accessEgress": "Exit blocked. Cannot open front door due to items stacked behind it. Immediate danger identified. Items removed during visit to clear egress route. Resident advised; safeguarding referral made."
```

**Key management issues:**
```json
"accessEgress": "Key management issue: spare key locked in bedroom safe. If resident incapacitated, FRS cannot access spare key quickly. Advised to provide spare key to concierge desk."
```

---

### `ignition_sources`

**Type:** Array of strings  
**What it measures:** Observable ignition sources of concern.

**Examples:**

**Multiple sources:**
```json
"ignition_sources": [
  "candles_unattended",
  "multiple_space_heaters",
  "chip_pan"
]
```

**Single source:**
```json
"ignition_sources": ["open_flame"]
```

**None:**
```json
"ignition_sources": []
```

**Common ignition sources:**
- `candles_unattended`
- `multiple_space_heaters`
- `chip_pan`
- `open_flame` (cooking, fireplace)
- `smoking_materials`
- `overloaded_sockets`

**When to record:**  
Any ignition source you're concerned about.

---

### `pets_risk`

**Type:** String  
**What it measures:** Do pets create evacuation risks?

**Examples:**

**No risk:**
```json
"pets_risk": "None. No pets present."
```

**Potential risk:**
```json
"pets_risk": "Large dog (German Shepherd) present. Dog may impede evacuation if panicked. Resident advised on pet evacuation planning (lead kept by door)."
```

**High risk:**
```json
"pets_risk": "Multiple large dogs (3 dogs, aggressive when stressed). Cages blocking hallway route to door. Resident advised; cages repositioned during visit to clear egress."
```

**What to look for:**
- Large/aggressive animals
- Cages blocking routes
- Animals that panic in emergencies

**Note:** Small pets (cat, small dog) rarely create significant risk.

---

## Safeguarding Fields Explained

### What is Safeguarding Context?

**Safeguarding** = protecting vulnerable adults from abuse, neglect, harm.

**Why in PCFRA?**  
Fire safety officers often encounter safeguarding issues during home visits:
- Anti-social behaviour (ASB)
- Violence/threats to staff
- Self-neglect (hoarding, unsafe living conditions)
- Mental health crises
- Adult social care needs

**PCFRA provides** a mechanism to **record and escalate** without detailed case management.

---

### `asbViolenceRisk`

**Type:** String

**What it measures:**  
Anti-social behaviour or violence risks.

**Examples:**

**None:**
```json
"asbViolenceRisk": "None reported."
```

**ASB reported:**
```json
"asbViolenceRisk": "ASB reported by neighbours (loud music, shouting at night). No direct threats to staff."
```

**Violence to staff:**
```json
"asbViolenceRisk": "Threats to staff recorded (2025-06). Police attendance on 3 occasions (past 6 months). Lone working protocol applies for all future visits. Visit conducted with two officers present."
```

**What to record:**
- ASB (but no detailed incidents - just flag)
- Threats to staff
- Police attendance (number, dates - but no case details)
- Lone working protocol activated

**When to record:**  
If staff safety is a concern during visits.

**Immediate action:**  
If immediate threat → leave premises; call police; record incident.

---

### `agencyInvolved`

**Type:** Array of strings (enum)  
**Allowed values:** `"police"`, `"adult_social_care"`, `"mental_health_team"`, `"landlord_housing"`, `"fire_and_rescue"`, `"other_agency"`

**What it measures:**  
Which agencies are involved with this person/property?

**Example:**
```json
"agencyInvolved": [
  "police",
  "adult_social_care",
  "mental_health_team"
]
```

**What this means:**  
Multi-agency case. Coordination required.

**When to record:**  
If you know other agencies are involved (case conferences, shared records).

**Don't record:**  
Detailed case information (wrong place - sensitive).

**Just record:**  
Which agencies (roles only).

---

### `immediateDanger`

**Type:** Boolean  
**What it measures:** Was immediate life safety hazard present at assessment?

**Examples:**

**False (most cases):**
```json
"immediateDanger": false
```

**True (urgent action required):**
```json
"immediateDanger": true
```

**When to set true:**
- Blocked exit (cannot escape)
- Severe electrical hazard (exposed wires, overheating)
- Oxygen near ignition source
- Person incapacitated and alone (medical emergency)

**If true, you MUST also set:**
```json
"immediateActionTaken": "Isolated electrical circuit; removed portable heater; called 999."
```

---

### `immediateActionTaken`

**Type:** String

**What it is:**  
What did you do **immediately** to mitigate the danger?

**Examples:**

**No action needed:**
```json
"immediateActionTaken": "None required. Low environmental risk."
```

**Action taken:**
```json
"immediateActionTaken": "Isolated electrical circuit feeding damaged portable heater; removed portable heater from flat. Advised resident on safe electrical use. Will arrange follow-up visit in 7 days to check compliance."
```

**What to record:**
- Immediate actions you took
- Who you called (999, adult social care)
- What you removed/isolated
- Follow-up arranged

**When to include:**  
If `immediateDanger: true` or if you took any mitigation action.

---

### `referralsMade`

**Type:** Array of strings

**What it is:**  
References to referrals made to other services.

**Example:**
```json
"referralsMade": [
  "referral-adult-social-care-2025-10-12",
  "referral-mental-health-team-2025-10-12"
]
```

**What to record:**
- Referral IDs (opaque identifiers in your system)
- **Not** detailed case information

**When to include:**  
If you made referrals during/after assessment.

---

## When to Escalate

### Immediate Danger → Call 999

**Trigger scenarios:**
- Person collapsed/incapacitated
- Severe electrical hazard (fire/shock risk)
- Exit completely blocked (cannot escape)
- Oxygen near ignition source
- Domestic abuse (immediate risk)

**Action:**
1. Call 999 (police/ambulance/fire as appropriate)
2. Record: `"immediateDanger": true`
3. Record: `"immediateActionTaken": "Called 999..."`

---

### High Risk → Multi-Agency Referral

**Trigger scenarios:**
- Severe hoarding
- Self-neglect
- Mental health crisis
- Safeguarding concerns (abuse, exploitation)

**Action:**
1. Refer to adult social care
2. Refer to mental health team (if appropriate)
3. Record: `"agencyInvolved": ["adult_social_care", "mental_health_team"]`
4. Record: `"referralsMade": ["referral-id-001"]`

---

### Medium Risk → Follow-Up

**Trigger scenarios:**
- Moderate clutter
- Minor electrical issues
- Resident willing to address

**Action:**
1. Advise resident
2. Offer support
3. Schedule follow-up visit (7-14 days)
4. Record in `notes`

---

## Complete Examples

### Example 1: Low Environmental Risk

See `/examples/pcfra/environment/pcfra_environment_low_risk.json`

**Key features:**
- No hazards identified
- Clear egress
- Functional smoke alarm
- No safeguarding concerns

---

### Example 2: High Environmental Risk

See `/examples/pcfra/environment/pcfra_environment_high_risk.json`

**Key features:**
- Multiple hazards (hoarding, electrical, smoking)
- Immediate danger (damaged cable)
- Immediate action taken (isolated circuit)
- Follow-up scheduled

---

### Example 3: Safeguarding Context

See `/examples/pcfra/environment/pcfra_environment_safeguarding.json`

**Key features:**
- ASB/violence risk
- Multi-agency involvement
- Lone working protocol
- Referrals made

---

## Validation

```bash
# Validate PCFRA (Environment)
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.environment.schema.json \
  -d your-pcfra-environment.json
```

### Provenance Pointer Format

When recording provenance for specific fields, use JSON Pointer paths with the correct casing:

- Personal capability example: `/capability/mobilityAid`
- Environmental example: `/escapeRoute`

These paths correspond to field locations in their respective schemas.

### Common Errors

**Error:** `should match exactly one schema`  
**Fix:** Must have **either** `buildingRef` **or** `unitRef` (or both).

---

## FAQs

### Q: Do I always need PCFRA (Environment)?

**A:** No. Only when:
- ✅ In-person visit conducted
- ✅ Environmental risks present or possible
- ❌ Not for phone/digital assessments

### Q: Can I combine PCFRA (Person) and (Environment) in one document?

**A:** No. They're separate schemas.

**Why?**  
- Person is portable (follows person)
- Environment is location-specific

### Q: What if I find immediate danger?

**A:** Act immediately:
1. Mitigate (isolate, remove, call 999)
2. Record: `"immediateDanger": true`
3. Record: `"immediateActionTaken": "..."`

### Q: How do I record hoarding without being judgmental?

**A:** Be factual and objective:

❌ "Resident is a hoarder"  
✅ "Pathways narrowed by accumulated items"

❌ "Flat is disgusting"  
✅ "High combustible load; egress partially blocked"

**Describe observable conditions**, not judgments.

---

## Next Steps

- **Read:** [PCFRA Person Schema Guide](pcfra_person_schema_guide.md)
- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how both PCFRAs inform PEEPs
- **Implement:** Environmental assessment checklists

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

