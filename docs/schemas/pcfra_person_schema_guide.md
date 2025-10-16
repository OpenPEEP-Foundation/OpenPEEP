# PCFRA (Person) Schema Guide — Complete Reference

**Schema:** `pcfra.person.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Capability Fields Explained](#capability-fields-explained)
5. [No Medical Diagnoses Rule](#no-medical-diagnoses-rule)
6. [Provenance Tracking](#provenance-tracking)
7. [Core vs Extensions Pattern](#core-vs-extensions-pattern)
8. [Complete Examples](#complete-examples)
9. [Validation](#validation)
10. [FAQs](#faqs)

---

## Overview

### What is PCFRA (Person)?

**PCFRA (Person)** = **Person-Centred Fire Risk Assessment (Personal)**

It's an assessment of a **person's functional evacuation capability**.

**What it captures:**
- ✅ Functional capabilities (can they walk, hear alarms, use stairs?)
- ✅ Assistance needs (do they need help evacuating?)
- ✅ Equipment needs (evacuation chair, transfer board?)
- ✅ Observable behaviours affecting fire risk (non-diagnostic)
- ✅ Decision outputs (is PEEP required?)

**What it does NOT capture:**
- ❌ Medical diagnoses (no "has Parkinson's disease")
- ❌ Treatment data (no "takes medication X")
- ❌ Long-term health records

**Think of it as:** Functional capability profile for evacuation, not medical history.

### Why "Person-Centred"?

**Traditional approach:**  
"This person is disabled" → create PEEP

**Person-centred approach:**  
"This person can/cannot do X" → design evacuation support

**Focus on:**
- What they **can** do (capabilities)
- What they **need** (adjustments)
- **Not** what diagnoses they have

### When Do You Use PCFRA (Person)?

**Use PCFRA (Person) when:**
- ✅ Assessing evacuation capability
- ✅ Determining if PEEP is required
- ✅ Informing PEEP creation (what assistance/equipment needed?)

**Typical workflow:**
1. **Offer consent** (record in Consent schema)
2. **Conduct PCFRA (Person)** - assess functional capability
3. **Conduct PCFRA (Environment)** - assess dwelling/setting risks (optional, separate)
4. **Determine outcome** - is PEEP required?
5. **Create PEEP** - if required (informed by PCFRA)

---

## Quick Start

### Minimal PCFRA (Person)

```json
{
  "personRef": "person-uuid-001",
  "assessedAt": "2025-10-14T10:00:00Z",
  "assessmentMethod": "in_person",
  "outcome": {
    "relevantResident": true,
    "peepRequired": true,
    "rationale": "Reduced mobility; uses walking stick; stairs with assistance."
  }
}
```

**This records:**
- ✅ Link to person
- ✅ When assessed
- ✅ How assessed (in-person)
- ✅ Decision: PEEP required

**What's missing:**  
Detailed capability data. This is **bare minimum**.

### Standard PCFRA (Person)

```json
{
  "pcfraId": "pcfra-person-uuid-001",
  "version": "1.0.0",
  "personRef": "person-emma-wilson-uuid-001",
  "assessedAt": "2025-10-01T14:00:00Z",
  "assessorRole": "housing_officer",
  "assessmentMethod": "in_person",
  "confidenceLevel": "high",
  "consentRef": "consent-emma-wilson-uuid-001",
  "capability": {
    "walkingEndurance": "needs_frequent_breaks",
    "stairTolerance": "stairs_with_assist",
    "mobilityAid": "stick",
    "sensoryHearing": "typical",
    "sensoryVision": "typical",
    "cognitionAbility": "typical",
    "communicationAbility": "typical",
    "nightResponse": "wakes_to_alarm",
    "equipmentDependency": "none",
    "behaviouralRisks": [],
    "assistRequired": "needs_assist_1"
  },
  "outcome": {
    "relevantResident": true,
    "peepRequired": true,
    "rationale": "Resident uses walking stick and requires assistance on stairs. PEEP required to document evacuation assistance needs."
  }
}
```

---

## Complete Field Reference

### Required Fields

#### `personRef`

**Type:** String  
**Required:** ✅ Yes

**What it is:**  
Link to the Person Profile this assessment is for.

**Example:**
```json
"personRef": "person-emma-wilson-uuid-001"
```

**Must match:**  
The `personRef` from Person Profile schema.

---

#### `assessedAt`

**Type:** String (ISO 8601 date-time)  
**Required:** ✅ Yes

**What it is:**  
When was the assessment conducted?

**Example:**
```json
"assessedAt": "2025-10-14T14:00:00Z"
```

**Best practice:**  
Use UTC timestamp.

---

#### `assessmentMethod`

**Type:** String (enum)  
**Required:** ✅ Yes  
**Allowed values:** `"phone"`, `"digital_self"`, `"in_person"`

**What it is:**  
How was the assessment conducted?

**Method meanings:**

**`"phone"`:**  
Assessment conducted over the phone (verbal questions, self-report).

```json
"assessmentMethod": "phone"
```

**Pros:** Quick, accessible, no home visit needed  
**Cons:** No visual verification; lower confidence

**`"digital_self"`:**  
Person completed assessment themselves (online form, app).

```json
"assessmentMethod": "digital_self"
```

**Pros:** Convenient, person's own time  
**Cons:** Self-report only; no verification; may be inaccurate

**`"in_person"`:**  
Assessor visited in person (home visit, office visit).

```json
"assessmentMethod": "in_person"
```

**Pros:** Visual verification, observation, high confidence  
**Cons:** Resource-intensive, requires consent for home entry

**Impact on confidence:**
- In-person → typically `confidenceLevel: "high"`
- Phone → typically `confidenceLevel: "medium"`
- Digital self → typically `confidenceLevel: "low"` to `"medium"`

**Legal significance:**  
Method affects quality and evidential weight of assessment.

---

#### `outcome`

**Type:** Object  
**Required:** ✅ Yes

**What it is:**  
Decision outputs from the assessment.

**Required fields within outcome:**
- `relevantResident` ✅ (boolean)
- `peepRequired` ✅ (boolean)
- `rationale` ✅ (string)

**Full example:**
```json
"outcome": {
  "relevantResident": true,
  "peepRequired": true,
  "rationale": "Manual wheelchair user; cannot use stairs independently. Requires evacuation chair or evacuation lift. Relevant resident under Residential Regs 2025."
}
```

**Field explanations:**

**`relevantResident`** (boolean):  
Is this person a "relevant resident" under UK Residential Regs 2025?

```json
"relevantResident": true   // Yes, in scope for RPEEP
"relevantResident": false  // No, not in scope (e.g., workplace, not residential)
```

**UK definition:** "Relevant resident" = someone who may need assistance to evacuate due to disability or other factors.

**`peepRequired`** (boolean):  
Does this person need a PEEP?

```json
"peepRequired": true   // Yes, create PEEP
"peepRequired": false  // No, can self-evacuate
```

**Decision logic:**
- If person can evacuate independently → `false`
- If person needs assistance/adjustments → `true`

**`rationale`** (string):  
**Why** was this decision made?

**Good rationales:**
```json
"rationale": "Resident uses walking stick and requires assistance on stairs. PEEP required to document evacuation assistance needs. Relevant resident under Residential Regs 2025."
```

```json
"rationale": "Manual wheelchair user; cannot use stairs independently. Requires evacuation chair or evacuation lift. Relevant resident under Residential Regs 2025."
```

```json
"rationale": "Resident can evacuate independently with no assistance. Hears alarms; uses stairs independently. No PEEP required. Not a relevant resident."
```

**Avoid medical details:**
- ❌ "Has multiple sclerosis affecting mobility"
- ✅ "Reduced mobility; uses wheelchair"

**When to include:**  
Always! Transparency and accountability.

---

### Optional But Important Fields

#### `pcfraId`

**Type:** String  
**Required:** ❌ No (but recommended)

**What it is:**  
Unique identifier for this PCFRA.

**Example:**
```json
"pcfraId": "pcfra-person-emma-wilson-uuid-001"
```

**Why include:**  
Allows PEEP to reference this specific PCFRA (`peep.references.pcfraId`).

**Golden thread compliance** (Building Safety Act 2022).

---

#### `version`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Version tag for this PCFRA document.

**Example:**
```json
"version": "1.0.0"
```

**When to use:**  
If you track document versions internally.

---

#### `assessorRole`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Role of the person conducting the assessment.

**Examples:**
```json
"assessorRole": "housing_officer"
```
```json
"assessorRole": "fire_safety_advisor"
```
```json
"assessorRole": "self"
```

**Common roles:**
- `housing_officer`
- `fire_safety_advisor`
- `occupational_health_advisor`
- `care_manager`
- `self` (self-assessment)

**When to include:**  
Always (accountability).

---

#### `confidenceLevel`

**Type:** String (enum)  
**Required:** ❌ No (but recommended)  
**Allowed values:** `"low"`, `"medium"`, `"high"`

**What it is:**  
Overall confidence in the assessment data.

**How to determine:**

**`"high"`:**
- In-person assessment
- Visual verification
- Multiple sources corroborate
```json
"confidenceLevel": "high"
```

**`"medium"`:**
- Phone assessment
- Self-report with some verification
- Some fields unverified
```json
"confidenceLevel": "medium"
```

**`"low"`:**
- Digital self-assessment only
- No verification
- Limited information
- Consent refused (minimal data captured)
```json
"confidenceLevel": "low"
```

**Why it matters:**  
Tells consumers how much to trust this data.

**Example:**
```json
"assessmentMethod": "phone",
"confidenceLevel": "medium"
```
Phone assessment → medium confidence (self-report, no visual verification).

---

#### `consentRef`

**Type:** String  
**Required:** ❌ No (but required for UK RPEEP)

**What it is:**  
Link to the Consent record.

**Example:**
```json
"consentRef": "consent-emma-wilson-uuid-001"
```

**When to include:**  
- ✅ Always for UK RPEEP (Reg 4 consent requirement)
- ✅ Good practice for all contexts

**Links the chain:**  
Consent → PCFRA → PEEP

---

### The Core Field: `capability`

**Type:** Object  
**Required:** ❌ No (but this is the **whole point** of PCFRA!)

**What it is:**  
**Functional evacuation capabilities.**

This is where you record **what the person can and cannot do** for evacuation purposes.

**Structure:**
```json
"capability": {
  // Mobility
  "walkingEndurance": "...",
  "respiratoryTolerance": "...",
  "mobilityTransfer": "...",
  "stairTolerance": "...",
  "mobilityAid": "...",
  
  // Sensory
  "hearingSupport": "...",
  "sensoryHearing": "...",
  "visionSupport": "...",
  "sensoryVision": "...",
  
  // Cognitive
  "cognitionSupport": "...",
  "cognitionAbility": "...",
  
  // Communication
  "communicationSupport": "...",
  "communicationAbility": "...",
  
  // Awareness
  "nightResponse": "...",
  "alertnessSupport": "...",
  "alertnessEffects": "...",
  
  // Equipment & Risks
  "equipmentDependency": "...",
  "behaviouralRisks": [],
  
  // Summary
  "assistRequired": "...",
  "assistLevel": "...",
  "equipmentNeeded": []
}
```

**All fields are optional!** Include what's relevant.

---

## Capability Fields Explained

### Mobility Fields

#### `walkingEndurance`

**Enum:** `"normal_pace"`, `"needs_frequent_breaks"`, `"limited_distance"`, `"cannot_walk"`

**What it measures:**  
Can they sustain walking to an exit?

**Examples:**

**`"normal_pace"`:**  
Can walk to exit without difficulty.
```json
"walkingEndurance": "normal_pace"
```

**`"needs_frequent_breaks"`:**  
Can walk but needs to rest.
```json
"walkingEndurance": "needs_frequent_breaks"
```
**Implication:** May slow down evacuation; consider rest points.

**`"limited_distance"`:**  
Can only walk short distances.
```json
"walkingEndurance": "limited_distance"
```
**Implication:** May need wheelchair or assistance partway.

**`"cannot_walk"`:**  
Cannot walk at all.
```json
"walkingEndurance": "cannot_walk"
```
**Implication:** Wheelchair, carry, or lift required.

---

#### `respiratoryTolerance`

**Enum:** `"normal"`, `"mild_breathlessness_on_exertion"`, `"frequent_breaks_required"`, `"oxygen_support_needed"`

**What it measures:**  
Breathing tolerance during evacuation (physical exertion, potential smoke).

**Examples:**

**`"normal"`:**  
No breathing difficulty.

**`"mild_breathlessness_on_exertion"`:**  
Gets breathless when hurrying or climbing stairs.

**`"frequent_breaks_required"`:**  
Needs multiple rest breaks during evacuation.

**`"oxygen_support_needed"`:**  
Requires oxygen therapy.
```json
"respiratoryTolerance": "oxygen_support_needed"
```
**Implication:** Must bring portable oxygen; concierge team must know.

**Link to:**  
If `oxygen_support_needed`, also set:
```json
"equipmentDependency": "oxygen_therapy"
```

---

#### `mobilityTransfer`

**Enum:** `"independent_transfer"`, `"stand_pivot_with_support"`, `"hoist_or_full_lift_required"`, `"unable_to_transfer"`

**What it measures:**  
Can they stand, pivot, or transfer (e.g., wheelchair → evac chair)?

**Examples:**

**`"independent_transfer"`:**  
Can transfer independently (e.g., wheelchair → bed).

**`"stand_pivot_with_support"`:**  
Can stand briefly with someone holding them.
```json
"mobilityTransfer": "stand_pivot_with_support"
```
**Implication:** May be able to transfer to evac chair with 1 person assist.

**`"hoist_or_full_lift_required"`:**  
Needs hoist or 2+ people to lift.
```json
"mobilityTransfer": "hoist_or_full_lift_required"
```
**Implication:** Evac chair difficult; may need rescue sheet/sledge.

**`"unable_to_transfer"`:**  
Cannot be moved (medical contraindication).
```json
"mobilityTransfer": "unable_to_transfer"
```
**Implication:** Must evacuate in wheelchair/bed (evacuation lift essential).

---

#### `stairTolerance`

**Enum:** `"stairs_independent"`, `"stairs_with_assist"`, `"cannot_use_stairs"`

**What it measures:**  
Can they use stairs?

**Examples:**

**`"stairs_independent"`:**  
Can use stairs alone.
```json
"stairTolerance": "stairs_independent"
```

**`"stairs_with_assist"`:**  
Can use stairs with help (handrail, person to steady them).
```json
"stairTolerance": "stairs_with_assist"
```
**Implication:** Buddy or concierge can escort.

**`"cannot_use_stairs"`:**  
Cannot use stairs at all.
```json
"stairTolerance": "cannot_use_stairs"
```
**Implication:** Must use lift or be carried (evac chair/sledge).

**Critical for PEEP:**  
If `"cannot_use_stairs"`, PEEP must specify:
- Evacuation lift route, OR
- Refuge + evac chair route, OR
- Rescue by FRS (stay put)

---

#### `mobilityAid`

**Enum:** `"none"`, `"stick"`, `"frame"`, `"assisted_walking"`, `"wheelchair_manual"`, `"wheelchair_powered"`

**What it measures:**  
What mobility aid do they use?

**Examples:**

**`"stick"`:**
```json
"mobilityAid": "stick"
```
**Implication:** Can walk but needs support; may be slower.

**`"frame"`:**
```json
"mobilityAid": "frame"
```
**Implication:** Needs both hands on frame; cannot carry items; very slow.

**`"wheelchair_manual"`:**
```json
"mobilityAid": "wheelchair_manual"
```
**Implication:** Cannot use stairs; needs accessible route (lift or ramp).

**`"wheelchair_powered"`:**
```json
"mobilityAid": "wheelchair_powered"
```
**Implication:** Cannot use stairs; heavy chair (evacuation lift or rescue sheet).

**Link to other fields:**

If wheelchair, also expect:
- `stairTolerance: "cannot_use_stairs"`
- `walkingEndurance: "cannot_walk"`
- `assistRequired: "needs_assist_1"` or `"needs_assist_2"`
- `equipmentNeeded: ["evac_chair_required"]` or evacuation lift route

---

### Sensory Fields

#### `sensoryHearing`

**Enum:** `"typical"`, `"reduced"`, `"profound"`

**What it measures:**  
Baseline hearing ability (for alarm perception).

**Examples:**

**`"typical"`:**  
Can hear alarms.

**`"reduced"`:**  
Some hearing loss; may not hear quiet alarms.
```json
"sensoryHearing": "reduced"
```

**`"profound"`:**  
Cannot hear alarms.
```json
"sensoryHearing": "profound"
```

**If reduced or profound, also set:**
```json
"hearingSupport": "visual_alerts_required"
```

**Link to PEEP:**  
If profound, PEEP must specify visual alarm beacons.

---

#### `hearingSupport`

**Enum:** `"none"`, `"hearing_aid"`, `"visual_alerts_required"`, `"vibrating_pager"`

**What it measures:**  
What support is needed for alerts?

**Examples:**

**`"visual_alerts_required"`:**
```json
"hearingSupport": "visual_alerts_required"
```
**Implication:** Install visual alarm beacons.

**`"vibrating_pager"`:**
```json
"hearingSupport": "vibrating_pager"
```
**Implication:** Provide vibrating pager device.

---

#### `sensoryVision`

**Enum:** `"typical"`, `"low_vision"`, `"blind"`

**What it measures:**  
Baseline vision ability (for wayfinding).

**Examples:**

**`"low_vision"`:**
```json
"sensoryVision": "low_vision"
```
**Implication:** Needs high-contrast signage, additional lighting.

**`"blind"`:**
```json
"sensoryVision": "blind"
```
**Implication:** Needs tactile wayfinding, audio guidance, sighted guide.

**If low vision or blind, also set:**
```json
"visionSupport": "high_contrast_signage"
```
or
```json
"visionSupport": "tactile_guidance"
```

---

#### `visionSupport`

**Enum:** `"none"`, `"high_contrast_signage"`, `"tactile_guidance"`, `"additional_lighting"`

**What it measures:**  
What support is needed for wayfinding?

---

### Cognitive & Communication Fields

#### `cognitionAbility`

**Enum:** `"typical"`, `"mild_support_needed"`, `"significant_support_needed"`

**What it measures:**  
Ability to understand and follow emergency instructions.

**Examples:**

**`"mild_support_needed"`:**
```json
"cognitionAbility": "mild_support_needed"
```
**Implication:** Simple, clear instructions needed.

**`"significant_support_needed"`:**
```json
"cognitionAbility": "significant_support_needed"
```
**Implication:** Carer/staff must guide through evacuation.

---

#### `cognitionSupport`

**Enum:** `"none"`, `"step_by_step_prompts"`, `"carer_guidance_required"`

**What it measures:**  
What cognitive support is needed?

**Link to PEEP:**  
If carer required, include in `peep.responsibilities`.

---

#### `communicationAbility`

**Enum:** `"typical"`, `"mild_difficulty"`, `"severe_difficulty"`, `"non_verbal"`

**What it measures:**  
Ability to communicate during emergency.

**`"non_verbal"`:**
```json
"communicationAbility": "non_verbal"
```
**Implication:** Cannot call for help verbally; needs alternative (pendant, intercom).

---

#### `communicationSupport`

**Enum:** `"none"`, `"plain_language"`, `"BSL"`, `"AAC_device"`, `"text_only"`

**What it measures:**  
Preferred/required communication methods.

**`"BSL"`:**  
British Sign Language user.
```json
"communicationSupport": "BSL"
```
**Implication:** BSL interpreter for briefings/drills.

**`"AAC_device"`:**  
Uses Augmentative and Alternative Communication device.
```json
"communicationSupport": "AAC_device"
```

---

### Awareness Fields

#### `nightResponse`

**Enum:** `"wakes_to_alarm"`, `"requires_vibrating_pillow"`, `"deep_sleeper_support"`

**What it measures:**  
Will they wake to alarm at night?

**`"requires_vibrating_pillow"`:**
```json
"nightResponse": "requires_vibrating_pillow"
```
**Implication:** Install vibrating pillow/bed shaker.

**`"deep_sleeper_support"`:**
```json
"nightResponse": "deep_sleeper_support"
```
**Implication:** Neighbor or staff must physically wake them.

**Link to PEEP:**  
Include in `peep.nighttimeConsiderations`.

---

#### `alertnessEffects`

**Enum:** `"typical"`, `"mildly_affected"`, `"significantly_affected"`

**What it measures:**  
Are medications/substances likely to affect responsiveness?

**Not asking:** What medications they take (medical data).  
**Asking:** Is alertness affected? (functional impact).

**`"significantly_affected"`:**
```json
"alertnessEffects": "significantly_affected"
```
**Implication:** May not respond quickly to alarm; needs additional alerting.

---

### Equipment & Risks

#### `equipmentDependency`

**Enum:** `"none"`, `"oxygen_therapy"`, `"ventilation_device"`, `"powered_bed"`, `"other_equipment"`

**What it measures:**  
Critical equipment affecting evacuation.

**`"oxygen_therapy"`:**
```json
"equipmentDependency": "oxygen_therapy"
```
**Implication:** 
- Must bring portable oxygen when evacuating
- Oxygen concentrator creates fire risk (no smoking!)
- Record in `peep.criticalDependencies`

---

#### `behaviouralRisks`

**Type:** Array of strings (enum)  
**Required:** ❌ No

**What it is:**  
**Observable behaviours** increasing fire risk.

**Allowed values:**
- `"smoking_in_bed"`
- `"hoarding_observed"`
- `"aggressive_towards_staff"`
- `"non_cooperation_with_safety_guidance"`
- `"confusion_in_emergency"`
- `"substances_present"`
- `"fire_setting_behaviour"`
- `"other_observed"`

**Critical rule:** **Observable only, non-diagnostic!**

**Good:**
```json
"behaviouralRisks": ["smoking_in_bed"]
```
Observable: saw cigarettes, ashtray in bedroom.

**Bad:**
```json
"behaviouralRisks": ["has_dementia"]
```
❌ That's a diagnosis, not a behaviour!

**Better:**
```json
"behaviouralRisks": ["confusion_in_emergency"]
```
Observable: person becomes confused when distressed.

**When to include:**  
If observable behaviours affect fire risk or evacuation safety.

**Legal significance:**  
Avoid disability discrimination - record behaviours, not conditions.

---

#### `assistRequired`

**Enum:** `"none"`, `"needs_assist_1"`, `"needs_assist_2"`

**What it measures:**  
How many people needed to assist evacuation?

**`"none"`:**  
Can evacuate independently.

**`"needs_assist_1"`:**  
One person needed (buddy, escort, steady on stairs).

**`"needs_assist_2"`:**  
Two people needed (evac chair, carry down stairs, rescue sheet).

---

#### `assistLevel`

**Enum:** `"none"`, `"occasional_support"`, `"constant_support"`

**What it measures:**  
How much support needed?

**`"occasional_support"`:**  
Help only at key points (e.g., escorting to stairwell, then independent).

**`"constant_support"`:**  
Need help throughout evacuation (cannot be left alone).

---

#### `equipmentNeeded`

**Type:** Array of strings (enum)  
**Allowed values:** `"evac_chair_required"`, `"transfer_board"`, `"evacuation_sheet"`, `"stair_climber"`, `"rescue_sledge"`

**What it is:**  
Evacuation equipment required.

**Example:**
```json
"equipmentNeeded": ["evac_chair_required"]
```

**Implication:**  
PEEP must specify where evac chair is stored, who's trained to use it.

---

## No Medical Diagnoses Rule

### ❌ Don't Record Diagnoses

**Bad examples:**

```json
"capability": {
  "diagnosis": "Multiple Sclerosis"  // ❌ Wrong!
}
```

```json
"notes": "Has Parkinson's disease affecting mobility"  // ❌ Wrong!
```

### ✅ Record Functional Impact

**Good examples:**

```json
"capability": {
  "walkingEndurance": "limited_distance",
  "stairTolerance": "cannot_use_stairs",
  "mobilityAid": "wheelchair_manual"
}
```

```json
"capability": {
  "mobilityTransfer": "stand_pivot_with_support",
  "stairTolerance": "stairs_with_assist"
}
```

### Why?

**Legal:**
- Avoid disability discrimination
- GDPR data minimisation (don't collect unnecessary health data)

**Practical:**
- Diagnoses don't tell you evacuation capability
- "Has MS" doesn't tell you if they can use stairs
- "Uses wheelchair" **does** tell you they can't use stairs

**Functional > Diagnostic**

---

## Provenance Tracking

### What is Provenance?

**Provenance** = where did this information come from?

**Optional field:** `provenance` (array)

**Why track provenance:**
- Auditability (Building Safety Act 2022)
- Quality assurance (self-report vs observed)
- Verification (was this checked?)

**Example:**
```json
"provenance": [
  {
    "path": "/capability/mobilityAid",
    "source": "assessor_observed",
    "evidence": "visual_inspection",
    "confidenceLevel": "high",
    "recordedByRole": "housing_officer",
    "notes": "Wheelchair observed during home visit."
  },
  {
    "path": "/capability/stairTolerance",
    "source": "self_report",
    "evidence": "verbal_account",
    "confidenceLevel": "high",
    "recordedByRole": "housing_officer",
    "verifiedByRole": "housing_officer",
    "verifiedAt": "2025-10-14T14:00:00Z",
    "notes": "Resident confirmed cannot use stairs; corroborated by wheelchair observation."
  }
]
```

**Fields:**

**`path`** (required):  
JSON Pointer to the field: `"/capability/mobilityAid"`.

**`source`** (required):  
Where it came from:
- `"self_report"` - person told you
- `"assessor_observed"` - you saw it
- `"third_party_record"` - from another document (e.g., OT report)
- `"system_generated"` - calculated/inferred

**`evidence`:**  
What supports it:
- `"visual_inspection"` - you saw it
- `"verbal_account"` - they told you
- `"document_seen"` - reviewed a document
- `"device_log"` - equipment log/data
- `"photo_evidence"` - photographs
- `"other_evidence"` - something else

**When to include provenance:**  
For key fields or when verification is important.

---

## Core vs Extensions Pattern

### PCFRA (Person) is Portable

Core capability fields work **globally**:
- ✅ Mobility, sensory, cognitive - universal concepts
- ✅ Functional descriptions - no jurisdiction lock-in

### When to Use Extensions

**UK-specific assessment tools:**

```json
"extensions": {
  "gbr": {
    "nfccFrameworkUsed": true,
    "nfccRiskCategory": "medium",
    "relevantPersonDetermination": "2025-10-14"
  }
}
```

**Usually not needed!** Core schema is sufficient.

---

## Complete Examples

### Example 1: Standard Assessment (Moderate Needs)

```json
{
  "pcfraId": "pcfra-person-emma-wilson-uuid-001",
  "version": "1.0.0",
  "personRef": "person-emma-wilson-uuid-001",
  "assessedAt": "2025-10-01T14:00:00Z",
  "assessorRole": "housing_officer",
  "assessmentMethod": "in_person",
  "confidenceLevel": "high",
  "consentRef": "consent-emma-wilson-uuid-001",
  "capability": {
    "walkingEndurance": "needs_frequent_breaks",
    "respiratoryTolerance": "mild_breathlessness_on_exertion",
    "mobilityTransfer": "independent_transfer",
    "stairTolerance": "stairs_with_assist",
    "mobilityAid": "stick",
    "sensoryHearing": "typical",
    "sensoryVision": "typical",
    "cognitionAbility": "typical",
    "communicationAbility": "typical",
    "nightResponse": "wakes_to_alarm",
    "equipmentDependency": "none",
    "behaviouralRisks": [],
    "assistRequired": "needs_assist_1",
    "assistLevel": "occasional_support"
  },
  "outcome": {
    "relevantResident": true,
    "peepRequired": true,
    "rationale": "Resident uses walking stick and requires assistance on stairs. PEEP required to document evacuation assistance needs. Relevant resident under Residential Regs 2025."
  },
  "provenance": [
    {
      "path": "/capability/mobilityAid",
      "source": "assessor_observed",
      "evidence": "visual_inspection",
      "confidenceLevel": "high",
      "recordedByRole": "housing_officer",
      "notes": "Walking stick observed during home visit."
    }
  ]
}
```

---

### Example 2: High Support Needs

See full example in `/examples/pcfra/person/pcfra_person_high_needs.json`

Key features:
- Wheelchair, oxygen, sensory, cognitive needs
- Multiple equipment dependencies
- High assistance requirements
- Detailed provenance with verification

---

### Example 3: Low Confidence (Consent Refused)

See full example in `/examples/pcfra/person/pcfra_person_low_confidence.json`

Key features:
- Phone assessment (consent refused for in-person)
- Minimal capability data
- Low confidence level
- Limited provenance

---

## Validation

```bash
# Validate PCFRA (Person)
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.person.schema.json \
  -d your-pcfra-person.json
```

---

## FAQs

### Q: Do I need to fill in every capability field?

**A:** No! All capability fields are optional. Fill in **what's relevant**.

If person has no vision issues → omit `sensoryVision`, `visionSupport`.

### Q: What if person refuses detailed assessment?

**A:** Record minimal data:

```json
{
  "assessmentMethod": "phone",
  "confidenceLevel": "low",
  "capability": {
    "mobilityAid": "stick"
  },
  "outcome": {
    "relevantResident": true,
    "peepRequired": true,
    "rationale": "Limited assessment due to consent refusal."
  }
}
```

### Q: How detailed should provenance be?

**A:** Balance:
- ✅ Track key fields (mobility aid, stairs, equipment)
- ❌ Don't track every field (overhead)

---

## Next Steps

- **Read:** [PCFRA Environment Schema Guide](pcfra_environment_schema_guide.md)
- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how PCFRA informs PEEPs
- **Implement:** Build your PCFRA assessment workflow

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

