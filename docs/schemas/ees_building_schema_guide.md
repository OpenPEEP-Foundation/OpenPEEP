# EES (Building) Schema Guide — Complete Reference

**Schema:** `ees_building.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Evacuation Strategies Explained](#evacuation-strategies-explained)
5. [Plain Language Principles](#plain-language-principles)
6. [Accessibility Requirements](#accessibility-requirements)
7. [Complete Examples](#complete-examples)
8. [Validation](#validation)
9. [FAQs](#faqs)

---

## Overview

### What is EES (Building)?

**EES (Building)** = **Emergency Evacuation Statement (Building)**

Also called: **Building Evacuation Statement**, **Resident Information Statement**

It's a **plain-language summary** of the building's evacuation strategy for residents.

**What it contains:**
- ✅ Building evacuation strategy (stay put, simultaneous, phased)
- ✅ Plain-language guidance: "Who should move when?"
- ✅ Routes, refuges, lifts summary (high-level, no routing detail)
- ✅ Muster points
- ✅ Contacts (concierge, emergency, FRS info point)
- ✅ Accessibility formats available (large print, easy read, BSL, audio)

**What it does NOT contain:**
- ❌ Individual PEEPs (that's EES Personal)
- ❌ Technical fire engineering details
- ❌ Routing/wayfinding (just high-level description)
- ❌ Personal evacuation needs

**Think of it as:** Building-wide evacuation poster/leaflet for residents.

### Why Does EES (Building) Exist?

**UK Residential Regs 2025 Reg 10:**  
Duty to provide **plain-language information** to residents about evacuation strategy.

**Practical reasons:**
- Residents need to know: "What do I do if alarm sounds?"
- Different from Fire Risk Assessment (FRA) - that's for duty holders
- Different from individual PEEPs - this is building-wide guidance

**Audience:**
- Residents (primary)
- Fire and Rescue Service (secondary - building context)
- Visitors, contractors

---

## Quick Start

### Minimal EES (Building)

```json
{
  "schema_version": "1.0.0",
  "document_id": "ees-building-uuid-001",
  "created_at": "2025-10-14T10:00:00Z",
  "building": {
    "address": {
      "countryCode": "GB"
    }
  },
  "evacuation_strategy": "simultaneous_evacuate",
  "who_should_move_when": "When alarm sounds, everyone must leave immediately via nearest stairwell."
}
```

**This is bare minimum.**

### Standard EES (Building)

```json
{
  "schema_version": "1.0.0",
  "document_id": "ees-building-meridian-court-uuid-001",
  "created_at": "2025-10-01T10:00:00Z",
  "building": {
    "name": "Meridian Court",
    "address": {
      "buildingName": "Meridian Court",
      "townCity": "Exampolis",
      "postcode": "EX1 2AB",
      "countryCode": "GB",
      "buildingRef": "building-meridian-court-001"
    }
  },
  "evacuation_strategy": "stay_put",
  "who_should_move_when": "If fire is in your flat: leave immediately. If fire is elsewhere: stay put unless told to leave by Fire and Rescue Service.",
  "routes_summary": "Two protected stairwells (A and B). Do not use lifts.",
  "muster_points": [
    {
      "label": "Assembly Point A",
      "description": "Car park, north side of building"
    }
  ],
  "accessibility_formats": ["plain_english", "large_print"],
  "distribution_methods": ["noticeboard", "door_drop"]
}
```

---

## Complete Field Reference

### Required Fields

#### `schema_version`

**Type:** String  
**Required:** ✅ Yes  
**Default:** `"1.0.0"`

**What it is:**  
Version of the EES Building schema.

**How to use:**
```json
"schema_version": "1.0.0"
```

**Just use current version.** Don't change unless schema updates.

---

#### `document_id`

**Type:** String (UUID format)  
**Required:** ✅ Yes

**What it is:**  
Unique identifier for this EES Building document.

**Example:**
```json
"document_id": "ees-building-meridian-court-uuid-001"
```

**Use UUID v4 format** (same as PEEP `peepId`).

---

#### `created_at`

**Type:** String (ISO 8601 date-time)  
**Required:** ✅ Yes

**What it is:**  
When was this EES created?

**Example:**
```json
"created_at": "2025-10-01T10:00:00Z"
```

**Use UTC timestamp.**

---

#### `building`

**Type:** Object  
**Required:** ✅ Yes

**What it is:**  
Building identification.

**Required fields within building:**
- `address` ✅ (Address object)

**Optional fields:**
- `name` - Building name
- `identifiers` - UPRN, internal IDs

**Minimal:**
```json
"building": {
  "address": {
    "countryCode": "GB"
  }
}
```

**Standard:**
```json
"building": {
  "name": "Meridian Court",
  "address": {
    "buildingName": "Meridian Court",
    "addressLine1": "1 Example Street",
    "townCity": "Exampolis",
    "postcode": "EX1 2AB",
    "countryCode": "GB",
    "buildingRef": "building-meridian-court-001"
  },
  "identifiers": {
    "uprn": "100012345678",
    "internal_building_id": "MC-001"
  }
}
```

---

#### `evacuation_strategy`

**Type:** String (enum)  
**Required:** ✅ Yes  
**Allowed values:** `"stay_put"`, `"simultaneous_evacuate"`, `"phased"`, `"progressive_horizontal"`, `"other"`

**What it is:**  
The building's overall evacuation strategy.

**Strategy meanings:**

### `"stay_put"`

**What it means:**  
Residents stay in their flats unless:
- Fire is in their flat
- Fire and Rescue Service tells them to leave
- Smoke enters their flat

**When used:**  
High-rise buildings with:
- Compartmentation (fire-resistant walls/doors between flats)
- Protected escape routes
- Usually designed to contain fire to flat of origin

**Example:**
```json
{
  "evacuation_strategy": "stay_put",
  "who_should_move_when": "If fire is in your flat: leave immediately and close doors. If fire is elsewhere: stay in your flat unless told to leave by Fire Service."
}
```

**Key message:**  
"Don't evacuate unless fire is in your flat or you're told to."

---

### `"simultaneous_evacuate"`

**What it means:**  
Everyone evacuates at once when alarm sounds.

**When used:**  
Low-rise buildings, care homes, buildings without compartmentation.

**Example:**
```json
{
  "evacuation_strategy": "simultaneous_evacuate",
  "who_should_move_when": "When fire alarm sounds, everyone must leave immediately via nearest safe exit."
}
```

**Key message:**  
"Alarm = everyone out now."

---

### `"phased"`

**What it means:**  
Floors evacuate in sequence:
1. Alarm floor + adjacent floors evacuate first
2. Other floors prepare but wait
3. FRS directs further evacuations if needed

**When used:**  
Very tall buildings, complex buildings.

**Example:**
```json
{
  "evacuation_strategy": "phased",
  "who_should_move_when": "If fire alarm sounds AND fire is on your floor: evacuate immediately. If alarm sounds but fire is NOT on your floor: prepare to evacuate but wait for instructions from Fire Service or building staff."
}
```

**Key message:**  
"Your floor evacuates first. Other floors: wait for instructions."

---

### `"progressive_horizontal"`

**What it means:**  
Move horizontally to adjacent fire compartment, then down if needed.

**When used:**  
Hospitals, care homes with horizontal compartments.

**Example:**
```json
{
  "evacuation_strategy": "progressive_horizontal",
  "who_should_move_when": "If fire alarm sounds: move to adjacent compartment (through fire doors). Wait there. Only move to lower floors if instructed by staff or Fire Service."
}
```

---

### `"other"`

**When used:**  
Custom or hybrid strategy.

**If using "other", also include:**
```json
"strategy_other_detail": "Hybrid strategy: Stay put for flats; simultaneous for common areas."
```

---

#### `who_should_move_when`

**Type:** String  
**Required:** ✅ Yes

**What it is:**  
**THE MOST IMPORTANT FIELD.**

Plain-language summary of the evacuation strategy.

**Why it's required:**  
UK Residential Regs 2025 Reg 10 - duty to inform residents.

**How to write it:**

**Use plain language:**
- ❌ "Egress via protected means of escape upon activation of SOLAER"
- ✅ "When alarm sounds, leave via Stairwell A or B"

**Be specific:**
- ❌ "Evacuate if necessary"
- ✅ "If fire is in your flat: leave immediately. If fire is elsewhere: stay put."

**Cover scenarios:**
- Fire in your flat
- Fire elsewhere
- When to stay, when to go

**Examples:**

**Stay put:**
```json
"who_should_move_when": "If fire is in your flat: leave immediately and close doors behind you. If the fire alarm sounds but fire is NOT in your flat: stay in your flat and await instructions from the Fire and Rescue Service. Only leave if you are told to by the Fire Service or if you see smoke entering your flat."
```

**Simultaneous:**
```json
"who_should_move_when": "When you hear the fire alarm, everyone must leave the building immediately. Do not stop to collect belongings. Leave by the nearest safe exit and go to the Assembly Point in the car park."
```

**Phased:**
```json
"who_should_move_when": "Phase 1: If the fire alarm sounds AND fire is on your floor, evacuate immediately. Phase 2: If the alarm sounds but fire is NOT on your floor, prepare to evacuate but wait for instructions from the Fire and Rescue Service or building staff. Phase 3: If instructed, evacuate using your designated route."
```

**Good practices:**
- ✅ Short sentences
- ✅ Active voice ("Leave immediately", not "You should leave")
- ✅ Clear conditions ("If X, then Y")
- ✅ Avoid jargon

---

### Optional But Important Fields

#### `alarm_and_alerting_summary`

**Type:** String

**What it is:**  
Plain-language description of the alarm system.

**Example:**
```json
"alarm_and_alerting_summary": "A continuous alarm throughout the building means fire has been detected. If the fire is not in your flat, stay put unless instructed otherwise by the Fire and Rescue Service."
```

**When to include:**  
Always (helps residents understand what alarm means).

---

#### `routes_summary`

**Type:** String

**What it is:**  
High-level description of escape routes.

**Example:**
```json
"routes_summary": "Two protected stairwells (Stairwell A – north side, Stairwell B – south side). Do not use passenger lifts unless told to by the Fire and Rescue Service. Use stairwells only."
```

**Not routing detail!**  
Just: how many stairwells, where they are (compass directions), what not to use (lifts).

---

#### `refuges_and_lifts_summary`

**Type:** String

**What it is:**  
High-level info about refuges and evacuation lifts.

**Examples:**

**No lift:**
```json
"refuges_and_lifts_summary": "No evacuation lift available. Refuge spaces provided at stair lobbies on Levels 5–10 (behind fire doors). If you cannot use stairs, go to nearest refuge and use red intercom button to call for help."
```

**With evacuation lift:**
```json
"refuges_and_lifts_summary": "Evacuation lift (Lift 3, west wing) available for wheelchair users and those who cannot use stairs. Lift is BS EN 81-76 compliant with backup power. Refuges available on all floors (stairwell lobbies)."
```

---

#### `muster_points`

**Type:** Array of objects

**What it is:**  
Where should people go after evacuating?

**Example:**
```json
"muster_points": [
  {
    "label": "Assembly Point A",
    "description": "Car park, east side of building (primary muster point)"
  },
  {
    "label": "Assembly Point B",
    "description": "Garden square, south side (overflow if Assembly Point A full)"
  }
]
```

**Best practice:**  
Include compass direction or landmark ("north side", "by the library").

---

#### `resident_help_and_contacts`

**Type:** Object

**What it is:**  
Who can residents call for help?

**Example:**
```json
"resident_help_and_contacts": {
  "building_contact": "Concierge desk: 020 7000 0001 (24/7)",
  "out_of_hours": "Emergency line: 020 7000 0002",
  "frs_information_point": "Fire information box in main reception"
}
```

**When to include:**  
Always (UK Reg 10 - information duty).

---

## Evacuation Strategies Explained

### How to Choose Strategy

**Based on:**
- Building height
- Compartmentation (fire-resistant construction)
- Occupant type (residential, care home, hotel)
- Fire Risk Assessment (FRA) recommendation

**Get strategy from:**
- Building's Fire Risk Assessment
- Fire engineer's report
- Fire and Rescue Service consultation

**Don't make it up!** Use what FRA recommends.

---

## Plain Language Principles

### Write for 11-Year-Old Reading Age

**UK government guidance:**  
Public-facing documents should be readable by 11-year-olds.

**How to achieve:**

**Short sentences:**
- ❌ "In the event of audible fire alarm activation, building occupants should proceed expeditiously via designated means of egress."
- ✅ "When you hear the fire alarm, leave the building by the nearest exit."

**Simple words:**
- ❌ "Egress", "proceed", "vicinity"
- ✅ "Exit", "go", "near"

**Active voice:**
- ❌ "The building should be evacuated"
- ✅ "Leave the building"

**Avoid jargon:**
- ❌ "SOLAER system", "compartmentation", "FD30S door"
- ✅ "Fire alarm", "fire-resistant walls", "fire door"

**Use examples:**
- ❌ "Proceed to designated assembly area"
- ✅ "Go to Assembly Point A in the car park (north side of building)"

---

## Accessibility Requirements

### Multiple Formats (Equality Act 2010)

**UK law requires reasonable adjustments.**

**Formats to consider:**

```json
"accessibility_formats": [
  "plain_english",    // Simple language, short sentences
  "large_print",      // Minimum 16pt font
  "easy_read",        // Pictures + simple text (learning disabilities)
  "audio",            // MP3/audio file
  "braille",          // Tactile braille
  "sign_video",       // BSL video
  "high_contrast"     // Black text on yellow (visual impairment)
]
```

**Minimum:**  
Offer at least `plain_english` and `large_print`.

**Best practice:**  
Offer all formats; provide on request.

---

### Multiple Languages

**If community has non-English speakers:**

```json
"languages_available": [
  "en-GB",  // British English
  "pl-PL",  // Polish
  "ur-PK"   // Urdu
]
```

**Provide EES in community languages** (Equality Act 2010).

---

### Distribution Methods

**How will residents get this info?**

```json
"distribution_methods": [
  "noticeboard",      // Posted in common areas
  "door_drop",        // Delivered to each flat
  "email",            // Sent via email
  "tenant_portal",    // Online portal
  "welcome_pack",     // Given at move-in
  "qr_in_common_areas" // QR code for download
]
```

**UK Reg 10 compliance:**  
Must provide to all residents. Pick methods that reach everyone.

---

## Complete Examples

### Example 1: Stay Put Strategy (High-Rise)

See `/examples/ees/building/ees_building_stay_put.json`

**Key features:**
- 18-storey high-rise
- Stay put strategy
- Two stairwells, no evacuation lift
- Refuges on floors 5-10
- Multiple accessibility formats

---

### Example 2: Simultaneous Evacuation (Low-Rise)

See `/examples/ees/building/ees_building_simultaneous.json`

**Key features:**
- 4-storey low-rise
- Everyone evacuates on alarm
- Two stairwells
- Multiple languages (English, Polish, Urdu)

---

### Example 3: Phased Evacuation (Mixed-Use Tower)

See `/examples/ees/building/ees_building_phased.json`

**Key features:**
- 20-storey mixed-use
- Phased evacuation with PA system
- BS EN 81-76 evacuation lift
- Comprehensive accessibility (sign video, Scottish Gaelic)

---

## Validation

```bash
# Validate EES (Building)
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/ees_building.schema.json \
  -d your-ees-building.json
```

---

## FAQs

### Q: Is EES (Building) the same as Fire Risk Assessment?

**A:** No!

- **FRA:** Technical document for duty holders (compliance)
- **EES (Building):** Plain-language document for residents (information)

FRA informs EES, but they're different audiences.

### Q: How often should I update EES (Building)?

**A:** Update when:
- Building strategy changes
- Major refurbishment (new lifts, stairwells)
- FRA identifies new risks
- Annually (good practice)

### Q: Do I need EES (Building) for workplace?

**A:** Not legally required (FSO 2005 doesn't mandate it).

But **good practice** for:
- Public-facing buildings
- Visitors/contractors
- Accessibility (Equality Act 2010)

### Q: What's the difference between EES (Building) and EES (Personal)?

**A:**
- **EES (Building):** Building-wide (same for all residents)
- **EES (Personal):** Individual-specific (customized per person)

See: [EES Personal Schema Guide](ees_personal_schema_guide.md)

---

## Next Steps

- **Read:** [EES Personal Schema Guide](ees_personal_schema_guide.md)
- **Read:** [PEEP Schema Guide](peep_schema_guide.md)
- **Implement:** Create building-wide EES for your buildings

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

