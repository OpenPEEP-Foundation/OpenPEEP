# EES (Personal) Schema Guide — Complete Reference

**Schema:** `ees_personal.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Content Fields Explained](#content-fields-explained)
5. [Writing for Residents](#writing-for-residents)
6. [Accessibility & Multilingual](#accessibility--multilingual)
7. [Delivery Tracking](#delivery-tracking)
8. [Complete Examples](#complete-examples)
9. [Validation](#validation)
10. [FAQs](#faqs)

---

## Overview

### What is EES (Personal)?

**EES (Personal)** = **Emergency Evacuation Statement (Personal)**

Also called: **Personal Evacuation Statement**, **Resident-Facing PEEP Summary**

It's **plain-language evacuation instructions** derived from the PEEP for **this specific person**.

**What it contains:**
- ✅ Plain-language summary ("What should I do?")
- ✅ Step-by-step instructions (numbered, simple)
- ✅ Scenario-based guidance (fire in your home, fire elsewhere)
- ✅ Who will help you and how to contact them
- ✅ Language and format preferences
- ✅ Delivery tracking (was it given to resident? did they acknowledge?)

**What it does NOT contain:**
- ❌ Technical fire safety details
- ❌ Medical information
- ❌ Assessment data (capability, risks)

**Think of it as:** "Here's what **YOU** need to do" letter given to the resident.

### Difference: PEEP vs EES (Personal)

**PEEP:**
- **Audience:** Fire safety professionals, concierge, FRS
- **Purpose:** Comprehensive evacuation plan
- **Detail level:** High (technical, references, provenance)
- **Format:** JSON schema (machine-readable)

**EES (Personal):**
- **Audience:** The resident themselves
- **Purpose:** Simple instructions they can follow
- **Detail level:** Essential only (plain language)
- **Format:** Human-readable (large print PDF, easy read, audio)

**Relationship:**  
EES (Personal) is **derived from** PEEP (simplified, resident-friendly version).

### Why Does EES (Personal) Exist?

**UK Residential Regs 2025 Reg 10:**  
Duty to provide **plain-language information** to residents.

**Practical reasons:**
- Residents don't need technical PEEP (too complex)
- Plain language = accessible (Equality Act 2010)
- Something they can keep by their door/phone
- Evidence of information duty (compliance)

**Deliverable:**  
Often a **printed leaflet** or **audio file** given to resident.

---

## Quick Start

### Minimal EES (Personal)

```json
{
  "schema_version": "1.0.0",
  "document_id": "ees-personal-uuid-001",
  "created_at": "2025-10-14T10:00:00Z",
  "person_profile_ref": "person-uuid-001",
  "unit_address": {
    "countryCode": "GB"
  },
  "peep_document_id": "peep-uuid-001",
  "content": {
    "steps_primary": [
      "When alarm sounds, leave your flat.",
      "Use the stairs (do not use lift).",
      "Go to Assembly Point in car park."
    ]
  }
}
```

**Bare minimum for validation.**

### Standard EES (Personal)

```json
{
  "schema_version": "1.0.0",
  "document_id": "ees-personal-emma-wilson-uuid-001",
  "created_at": "2025-10-01T11:00:00Z",
  "person_profile_ref": "person-emma-wilson-uuid-001",
  "unit_address": {
    "buildingName": "Meridian Court",
    "subBuilding": "Flat 12",
    "postcode": "EX1 2AB",
    "countryCode": "GB"
  },
  "peep_document_id": "peep-emma-wilson-uuid-001",
  "building_ees_document_id": "ees-building-meridian-court-uuid-001",
  "language": "en-GB",
  "format": ["standard_print"],
  "content": {
    "summary": "Your building uses a stay-put strategy. If the alarm sounds but fire is NOT in your flat, stay in your flat and await instructions.",
    "steps_primary": [
      "If you discover fire in your flat: leave immediately, close doors, take phone.",
      "Call 999 once outside.",
      "Go to Assembly Point A (by the library).",
      "If alarm sounds but NO fire in your flat: stay put, close door, wait for Fire Service."
    ],
    "scenario_instructions": [
      {
        "scenario": "fire_in_your_home",
        "detail": "Leave immediately. Use Stairwell A or B. Do not use lift. Go to Assembly Point A."
      },
      {
        "scenario": "fire_elsewhere",
        "detail": "Stay put. Keep door closed. Only leave if told to by Fire Service."
      }
    ],
    "muster_point_label": "Assembly Point A (by library)"
  },
  "assistance": {
    "needs_assistance": true,
    "assisters": ["Neighbour (Flat 11)", "Concierge (24/7)"],
    "contact_on_alarm": "Concierge: 020 7000 0001"
  },
  "delivery": {
    "delivered_by": "Housing Officer",
    "delivered_at": "2025-10-01T15:00:00Z",
    "acknowledged_by_resident": true
  }
}
```

---

## Complete Field Reference

### Required Fields

#### `schema_version`, `document_id`, `created_at`

Same as EES (Building). Use current version, UUID, and UTC timestamp.

---

#### `person_profile_ref`

**Type:** String  
**Required:** ✅ Yes

**What it is:**  
Link to the Person Profile this EES is for.

**Example:**
```json
"person_profile_ref": "person-emma-wilson-uuid-001"
```

**Must match:**  
The `personRef` from Person Profile and PEEP.

---

#### `unit_address`

**Type:** Object (Address schema)  
**Required:** ✅ Yes

**What it is:**  
Address of the person's flat/unit.

**Example:**
```json
"unit_address": {
  "buildingName": "Meridian Court",
  "subBuilding": "Flat 12",
  "postcode": "EX1 2AB",
  "countryCode": "GB",
  "unitRef": "unit-meridian-court-12-001"
}
```

**Why separate from building address:**  
EES (Personal) is **unit-specific**, not building-wide.

---

#### `peep_document_id`

**Type:** String  
**Required:** ✅ Yes

**What it is:**  
Link to the source PEEP this EES is derived from.

**Example:**
```json
"peep_document_id": "peep-emma-wilson-uuid-001"
```

**Traceability:**  
EES Personal → PEEP → PCFRA → Consent (golden thread).

---

#### `content`

**Type:** Object  
**Required:** ✅ Yes

**What it is:**  
The actual evacuation instructions for the resident.

**Required field within content:**
- `steps_primary` ✅ (array of strings)

**Optional fields:**
- `summary` - High-level overview
- `scenario_instructions` - Fire in your home, fire elsewhere, etc.
- `waiting_place` - Where to wait if not evacuating
- `muster_point_label` - Where to go after evacuating
- `notes_for_resident` - Additional info

**See:** [Content Fields Explained](#content-fields-explained) below.

---

## Content Fields Explained

### `summary`

**Type:** String  
**Required:** ❌ No (but highly recommended)

**What it is:**  
One-paragraph overview of the evacuation plan.

**How to write it:**

**Good example:**
```json
"summary": "Your building uses a stay-put strategy. If you hear the alarm and there is NO fire in your flat, stay in your flat and await instructions. If fire is in your flat, leave immediately."
```

**2-4 sentences covering:**
1. Building strategy (stay put / evacuate)
2. What to do on alarm
3. Who will help (if applicable)

**Plain language:**
- ❌ Technical terms
- ✅ Simple, clear instructions

---

### `steps_primary`

**Type:** Array of strings  
**Required:** ✅ Yes (minimum 1)

**What it is:**  
Numbered step-by-step instructions.

**How to write them:**

**One action per step:**
```json
"steps_primary": [
  "When you hear the fire alarm, stay calm.",
  "Get into your wheelchair if you are not already in it.",
  "Take your mobile phone and keys.",
  "Leave your flat and close the door behind you.",
  "Go to Lift 2 (the evacuation lift) in the west wing.",
  "Press the call button and wait for the lift.",
  "Get in the lift and press the Ground Floor button.",
  "When you reach the ground floor, leave the building.",
  "Go to Assembly Point A in the car park.",
  "Wait there until the Fire Service says you can go back inside."
]
```

**Best practices:**
- ✅ Numbered/ordered (array order = step order)
- ✅ Imperative mood ("Do this" not "You should do this")
- ✅ One action per step
- ✅ Specific locations ("Lift 2, west wing" not just "the lift")
- ✅ Include what to take (phone, keys)
- ✅ Include what NOT to do (don't collect belongings, don't use lift)

**Common mistakes:**
- ❌ Too vague: "Evacuate safely"
- ❌ Too long per step: "When you hear the alarm, stay calm, get in wheelchair, take phone and keys, check door for heat..."
- ✅ Break into small, clear steps

---

### `scenario_instructions`

**Type:** Array of scenario objects  
**Required:** ❌ No (but recommended)

**What it is:**  
Instructions for different scenarios.

**How to use:**

```json
"scenario_instructions": [
  {
    "scenario": "fire_in_your_home",
    "detail": "If fire is in your flat: leave immediately. Close door behind you. Go to Lift 2 (evacuation lift) or refuge on Floor 12. Call 999 if you cannot leave."
  },
  {
    "scenario": "fire_elsewhere",
    "detail": "If fire is NOT in your flat but alarm is sounding: prepare to leave. Call concierge. They will tell you when to evacuate. If smoke enters your flat, leave immediately."
  },
  {
    "scenario": "if_in_lift",
    "detail": "If you are in the evacuation lift when alarm sounds: lift will go to ground floor automatically. Leave building when doors open."
  },
  {
    "scenario": "other",
    "detail": "If evacuation lift not working: go to refuge on Floor 12. Press red intercom button. Tell them you need help. Stay at refuge. Concierge or Fire Service will help you."
  }
]
```

**Allowed scenarios:**
- `"fire_in_your_home"`
- `"fire_elsewhere"`
- `"alarm_without_smoke"`
- `"if_in_lift"`
- `"other"`

**When to include:**  
Always! Different scenarios need different actions (especially stay-put buildings).

---

### `waiting_place`

**Type:** String

**What it is:**  
If not evacuating immediately, where should they wait?

**Example (stay-put building):**
```json
"waiting_place": "Behind your flat's front door (fire-rated door). Only leave if told to by Fire Service or if smoke enters your flat."
```

**Example (refuge):**
```json
"waiting_place": "Refuge on Floor 15 (north stairwell landing, behind fire door). Press red intercom button if you need help."
```

**When to include:**
- ✅ Stay-put buildings (where to wait)
- ✅ Refuge-based plans (where refuge is)
- ❌ Simultaneous evacuation (everyone leaves immediately)

---

### `muster_point_label`

**Type:** String

**What it is:**  
Where to go after evacuating.

**Examples:**
```json
"muster_point_label": "Assembly Point A (car park, north side)"
```

```json
"muster_point_label": "Front courtyard (opposite main entrance)"
```

**Be specific:**  
Include compass direction or landmark.

---

### `notes_for_resident`

**Type:** String

**What it is:**  
Additional important information for the resident.

**Examples:**

**Rights and contact:**
```json
"notes_for_resident": "You have the right to ask questions about this plan at any time. If anything changes (for example, if you move to a different flat or if your mobility changes), please tell the housing office so we can update your plan. The concierge team are here to help you 24 hours a day. Their emergency number is on the back of this sheet."
```

**Equipment notes:**
```json
"notes_for_resident": "An evacuation chair is stored in the refuge cupboard on Floor 12. You do not need to know how to use it – trained staff will operate it. Your job is just to get to the refuge and call for help."
```

**When to include:**  
Always! Empowers resident, provides contacts, explains rights.

---

## Writing for Residents

### Plain Language Checklist

**✅ Do:**
- Use short sentences (10-15 words)
- Use simple words (leave, go, call)
- Use active voice (Do this, not "This should be done")
- Use "you" (You should leave, not "Residents should leave")
- Give specific locations (Stairwell A, north side)
- Use present tense for instructions

**❌ Don't:**
- Use jargon (egress, SOLAER, FD30S)
- Use technical terms (compartmentation, means of escape)
- Use passive voice (The building should be evacuated)
- Use complex sentences (If X occurs and Y is true, then...)

### Examples: Technical vs Plain Language

**Technical:**
```
"Upon activation of the SOLAER system, egress via the nearest protected means of escape to the designated assembly area."
```

**Plain language:**
```
"When the fire alarm sounds, leave the building by the nearest exit. Go to Assembly Point A in the car park."
```

---

**Technical:**
```
"If compartmentation is breached and smoke ingress occurs, immediate evacuation via alternative egress route is mandated."
```

**Plain language:**
```
"If smoke comes into your flat, leave immediately. Use the other stairwell if your normal route is blocked."
```

---

### Scenario-Based Instructions

**Why scenarios?**  
"Stay put" is confusing. When stay, when go?

**Solution:** Spell out scenarios:

**Scenario 1: Fire in your flat**
```
"Leave immediately. Close door. Go to Assembly Point."
```

**Scenario 2: Fire elsewhere**
```
"Stay in your flat. Close door. Wait for Fire Service to tell you what to do."
```

**Scenario 3: Smoke entering flat**
```
"Leave immediately. Call 999. Tell them smoke is in your flat."
```

**Reduces confusion.**  
Resident knows exactly what to do in each situation.

---

## Accessibility & Multilingual

### Format Field

**Type:** Array of strings (enum)  
**Allowed values:**
- `"standard_print"` - Normal print (12pt)
- `"large_print"` - 16pt+ font
- `"easy_read"` - Pictures + simple text
- `"audio"` - MP3/audio file
- `"braille"` - Tactile braille
- `"sign_video"` - BSL video
- `"high_contrast"` - Black on yellow

**Example:**
```json
"format": ["large_print", "high_contrast"]
```

**When to include:**  
Match resident's communication preferences from Person Profile.

**Equality Act 2010:**  
Must provide in accessible format if requested.

---

### Language Field

**Type:** String (IETF language tag)  
**Example:** `"en-GB"`, `"cy-GB"`, `"pl-PL"`, `"ur-PK"`

**What it is:**  
Language this EES is written in.

**How to use:**

**English:**
```json
"language": "en-GB"
```

**Welsh:**
```json
"language": "cy-GB",
"content": {
  "summary": "Mae eich adeilad yn defnyddio strategaeth aros yn eich lle...",
  "steps_primary": [
    "Pan glywch y larwm tân, arhoswch yn dawel.",
    "..."
  ]
}
```

**Polish:**
```json
"language": "pl-PL",
"content": {
  "summary": "Twój budynek stosuje strategię pozostania na miejscu...",
  "steps_primary": [
    "Kiedy usłyszysz alarm pożarowy, zachowaj spokój.",
    "..."
  ]
}
```

**When to use:**  
Match resident's `preferredLanguage` from Person Profile.

---

## Delivery Tracking

### Why Track Delivery?

**UK Residential Regs 2025 Reg 10:**  
Must provide information to residents. Need to **prove you did**.

**Evidence:**
- When delivered?
- Who delivered it?
- Did resident acknowledge?

### `delivery` Object

**Type:** Object  
**Fields:**
- `delivered_by` - Who delivered it
- `delivered_at` - When delivered
- `acknowledged_by_resident` - Did they confirm receipt?

**Example:**
```json
"delivery": {
  "delivered_by": "Housing Officer, Manchester Housing Ltd",
  "delivered_at": "2025-10-01T15:00:00Z",
  "acknowledged_by_resident": true
}
```

**How delivery works:**

**Door drop:**
```json
"delivery": {
  "delivered_by": "Concierge Team",
  "delivered_at": "2025-10-01T14:00:00Z",
  "acknowledged_by_resident": false
}
```
Left at door. No acknowledgement possible.

**Hand-delivered:**
```json
"delivery": {
  "delivered_by": "Housing Officer Jordan Smith",
  "delivered_at": "2025-10-01T15:00:00Z",
  "acknowledged_by_resident": true
}
```
Given in person. Resident signed receipt.

**Email:**
```json
"delivery": {
  "delivered_by": "Automated system",
  "delivered_at": "2025-10-01T12:00:00Z",
  "acknowledged_by_resident": false
}
```
Email sent. No read receipt.

**When to include:**  
Always for UK RPEEP (evidence of Reg 10 compliance).

---

## Complete Examples

### Example 1: Standard (Stay-Put Building)

See `/examples/ees/personal/ees_personal_standard.json`

**Key features:**
- Stay-put strategy
- Scenario-based instructions
- Neighbour buddy system
- Standard print format
- Delivery tracked

---

### Example 2: High Support Needs (Evacuation Lift)

See `/examples/ees/personal/ees_personal_high_needs.json`

**Key features:**
- Powered wheelchair user
- Evacuation lift primary route
- Refuge + evac chair secondary route
- Large print, high contrast format
- Oxygen dependency noted
- Detailed assistance info

---

### Example 3: Multilingual (Polish)

See `/examples/ees/personal/ees_personal_multilingual.json`

**Key features:**
- Polish language (pl-PL)
- Bilingual (Polish primary, English backup)
- Audio format
- Cultural sensitivity
- Simultaneous evacuation strategy

---

## Validation

```bash
# Validate EES (Personal)
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/ees_personal.schema.json \
  -d your-ees-personal.json
```

### Common Errors

**Error:** `should have required property 'steps_primary'`  
**Fix:** Must include at least one step in `content.steps_primary` array.

**Error:** `should NOT have fewer than 1 items`  
**Fix:** `steps_primary` must have at least 1 item.

---

## FAQs

### Q: Is EES (Personal) required for every PEEP?

**A:** UK Residential Regs 2025: Yes (Reg 10 information duty).

For workplace/other contexts: Not legally required, but **good practice**.

### Q: Can I just give resident the PEEP?

**A:** ❌ No!

PEEP is technical (JSON, references, metadata).  
Resident needs **plain-language version** (EES Personal).

### Q: How do I deliver EES (Personal)?

**A:** Options:
- **Door drop** (printed leaflet through letterbox)
- **Hand delivery** (during visit; get signature)
- **Email** (PDF attachment)
- **Tenant portal** (download)
- **Post** (if resident not at home)

**UK Reg 10 compliance:**  
Must use method that ensures resident receives it.

**Best practice:**  
Hand deliver during PCFRA visit; get acknowledgement.

### Q: What if resident doesn't speak English?

**A:** Provide in their language:

```json
"language": "pl-PL",
"content": {
  "summary": "Twój budynek...",
  "steps_primary": ["...", "..."]
}
```

**Equality Act 2010 / UK Reg 10:**  
Must provide accessible information (includes language access).

### Q: Do I update EES (Personal) when I update PEEP?

**A:** Yes!

If PEEP changes → create new EES (Personal) → deliver to resident.

**Workflow:**
1. PEEP reviewed → outcome: amended
2. Create new EES (Personal) with updated instructions
3. Deliver new EES to resident
4. Record delivery

### Q: What if resident loses their EES?

**A:** Reissue!

Keep digital copy; print and deliver again.

---

## Next Steps

- **Read:** [EES Building Schema Guide](ees_building_schema_guide.md)
- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how EES derives from PEEP
- **Implement:** EES generation and delivery workflow

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

