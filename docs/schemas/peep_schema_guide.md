# PEEP Schema Guide — Complete Reference

**Schema:** `peep.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Core vs Extensions Pattern](#core-vs-extensions-pattern)
5. [Jurisdiction Profiles](#jurisdiction-profiles)
6. [Complete Examples](#complete-examples)
7. [Validation](#validation)
8. [FAQs](#faqs)

---

## Overview

### What is a PEEP?

A **Personal Emergency Evacuation Plan (PEEP)** is a document that describes **how a specific person should evacuate a specific building** in the event of a fire or emergency.

Think of it as a **personalized evacuation instruction manual** that:
- Takes into account the person's capabilities (can they use stairs? do they hear alarms?)
- Considers the building's evacuation strategy (stay put vs evacuate)
- Provides step-by-step instructions
- Identifies who will help and what equipment is needed
- Links to the assessment that informed it (PCFRA)

### When Do You Need a PEEP?

You need a PEEP when:
- ✅ A person has **difficulty evacuating independently** (mobility, sensory, cognitive limitations)
- ✅ A building has a **duty to provide evacuation plans** (UK Residential Regs 2025, FSO 2005)
- ✅ An employer must provide **reasonable adjustments** (Equality Act 2010)
- ✅ A person **requests** an evacuation plan

### Who Uses This Schema?

- **Housing providers** (social housing, private landlords, care homes)
- **Employers** (workplace fire safety)
- **Building operators** (universities, hospitals, public buildings)
- **Fire and Rescue Services** (accessing plans during incidents)
- **Software developers** (building PEEP management systems)

---

## Quick Start

### Minimal Valid PEEP

This is the **absolute minimum** you need to create a valid PEEP:

```json
{
  "schemaVersion": "1.0.0",
  "peepId": "peep-uuid-12345",
  "personProfile": {
    "personRef": "person-uuid-67890"
  },
  "address": {
    "countryCode": "GB"
  },
  "evacuationStrategy": "Simultaneous Evacuation",
  "planSummary": "Leave building via nearest stairwell on alarm.",
  "metadata": {
    "createdAt": "2025-10-14T10:00:00Z",
    "createdBy": "housing.officer@example.org",
    "status": "draft"
  },
  "routes": [
    {
      "name": "Primary",
      "pathDescription": "Exit via stairwell to ground floor."
    }
  ],
  "evacuationProcedure": [
    {
      "order": 1,
      "instruction": "Evacuate immediately on alarm."
    }
  ]
}
```

**Why this is minimal:**
- All **required fields** are present
- Contains **one route** named "Primary" (required by schema constraint)
- Has **one procedure step** (minimum 1 required)
- Links to a **person** and **location**
- Has basic **metadata** (who created it, when, status)

**What's missing:**
- Assistance details (who will help?)
- Communications adjustments (how will they be alerted?)
- Equipment needs (what tools are required?)
- Consent (did they agree to this?)
- Reviews (has it been checked?)

In practice, you'll want to add these for a useful PEEP!

---

## Complete Field Reference

### Top-Level Required Fields

#### `schemaVersion`

**Type:** String (SEMVER format: `X.Y.Z`)  
**Required:** ✅ Yes  
**Example:** `"1.0.0"`

**What it is:**  
The version of the PEEP schema this document conforms to. Uses Semantic Versioning.

**Why it exists:**  
Allows systems to know which version of the schema to validate against. Critical for backwards compatibility when schemas evolve.

**How to use it:**  
Always use the current schema version. At time of writing: `"1.0.0"`.

**Common mistakes:**
- ❌ Using `"1"` instead of `"1.0.0"` (must be full SEMVER)
- ❌ Making up your own version number
- ✅ Use the version from the `$id` in the schema file

---

#### `peepId`

**Type:** String (UUID v4 format)  
**Required:** ✅ Yes  
**Example:** `"f47ac10b-58cc-4372-a567-0e02b2c3d479"`

**What it is:**  
A globally unique identifier for this specific PEEP document.

**Why it exists:**  
Allows you to reference this PEEP from other documents (reviews, consent records, EES Personal). Essential for golden thread traceability (Building Safety Act 2022).

**How to use it:**  
Generate a UUID v4 when creating a new PEEP. Never reuse IDs. If you amend a PEEP, decide whether to:
- Keep the same ID (minor update, still the same plan)
- Generate a new ID (major change, superseding the old plan)

**How to generate:**
```bash
# Command line (Mac/Linux)
uuidgen | tr '[:upper:]' '[:lower:]'

# Python
import uuid
str(uuid.uuid4())

# JavaScript
crypto.randomUUID()
```

**Common mistakes:**
- ❌ Using sequential numbers (`"peep-001"`) - not globally unique
- ❌ Using person's name (`"peep-john-smith"`) - privacy risk, not unique
- ✅ Use proper UUID v4 format

---

#### `personProfile`

**Type:** Object (embedded or reference)  
**Required:** ✅ Yes  
**Schema:** `common/person_profile.schema.json`

**What it is:**  
Information about the person this PEEP is for.

**Why it exists:**  
Links the PEEP to a specific individual. Required for identity and cross-referencing.

**How to use it:**  
You can either:
1. **Embed minimal profile** (just `personRef`)
2. **Embed full profile** (all person details)

**Minimal (recommended for privacy):**
```json
"personProfile": {
  "personRef": "person-uuid-12345"
}
```

**Full (useful if plan needs to be standalone):**
```json
"personProfile": {
  "personRef": "person-uuid-12345",
  "name": "Emma Wilson",
  "preferredLanguage": "en-GB",
  "contact": {
    "phone": "+447700900123"
  }
}
```

**Common mistakes:**
- ❌ Forgetting `personRef` (the only required field)
- ❌ Including medical diagnoses (wrong place - goes in PCFRA if at all)
- ✅ Keep it minimal unless plan must be self-contained

**See also:** [Person Profile Schema Guide](person_profile_schema_guide.md)

---

#### `address`

**Type:** Object  
**Required:** ✅ Yes  
**Schema:** `common/address.schema.json`

**What it is:**  
The address of the building/location this PEEP applies to.

**Why it exists:**  
PEEPs are **location-specific**. The same person needs different PEEPs for home vs workplace. The address identifies which building this plan is for.

**How to use it:**  
Provide enough detail to uniquely identify the location. For UK:
```json
"address": {
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "townCity": "Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001"
}
```

For international:
```json
"address": {
  "buildingNumber": "350",
  "addressLine1": "5th Avenue",
  "townCity": "New York",
  "postcode": "10118",
  "countryCode": "US"
}
```

**Required fields within address:**  
Technically none! But in practice you need enough to identify the location. At minimum: `countryCode`.

**Common mistakes:**
- ❌ Providing home address when PEEP is for workplace (wrong building!)
- ❌ Missing `buildingRef` / `unitRef` (golden thread linkage)
- ✅ Include UPRN for UK buildings (golden thread compliance)

**See also:** [Address Schema Guide](address_schema_guide.md)

---

#### `evacuationStrategy`

**Type:** String (enum or custom)  
**Required:** ✅ Yes  
**Allowed values:** `"Stay Put"`, `"Simultaneous Evacuation"`, `"Phased Evacuation"`, `"Partial Evacuation"`, `"Progressive Horizontal Evacuation"`, or any custom string

**What it is:**  
The building's overall evacuation strategy.

**Why it exists:**  
The individual's PEEP must align with the building's strategy. You can't have a "stay put" PEEP in a building that does simultaneous evacuation.

**How to use it:**  
Use the building's evacuation strategy. Get this from:
- The building's Fire Risk Assessment (FRA)
- The building's EES (Building) document
- The fire safety manager

**Strategy meanings:**
- **Stay Put**: Remain in your flat unless fire is in your flat or FRS tells you to leave
- **Simultaneous Evacuation**: Everyone evacuates at once on alarm
- **Phased Evacuation**: Floors evacuate in sequence (alarm floor first, then adjacent, etc.)
- **Partial Evacuation**: Only affected zone evacuates
- **Progressive Horizontal Evacuation**: Move horizontally to adjacent compartment, then down if needed

**Common mistakes:**
- ❌ Making up your own strategy without checking the building's FRA
- ❌ Contradicting the building strategy (individual wants to evacuate, building says stay put)
- ✅ Align with building; explain in `planSummary` how strategy applies to this person

---

#### `routes`

**Type:** Array of route objects  
**Required:** ✅ Yes (minimum 1)  
**Constraint:** **Must include one route named "Primary"**

**What it is:**  
The evacuation route(s) this person should use.

**Why it exists:**  
Tells the person (and responders) exactly how to evacuate. Clear, actionable guidance.

**How to use it:**  
Always include:
1. **Primary route** (name must be "Primary")
2. **Secondary route** (optional, for if primary is blocked)

**Minimal route:**
```json
"routes": [
  {
    "name": "Primary",
    "pathDescription": "Exit flat; turn left to north stairwell; descend to ground floor; exit via main entrance."
  }
]
```

**Route with assistance:**
```json
"routes": [
  {
    "name": "Primary",
    "pathDescription": "Exit flat; proceed to evacuation lift (west wing); descend to ground floor.",
    "assistance": [
      {
        "type": "lift_with_attendant",
        "personsRequired": 1,
        "role": "Concierge (24/7)"
      }
    ],
    "evacuationLift": {
      "used": true,
      "standard": "BS EN 81-76:2011",
      "attendantRequired": true
    },
    "musterPoint": "Assembly Point A (car park)"
  }
]
```

**Route fields explained:**

**`name`** (required):  
Route identifier. Must include one named "Primary".

**`pathDescription`** (required):  
Plain-language description of the route. Be specific:
- ❌ "Use the stairs"
- ✅ "Exit flat; turn right; proceed 20m to Stairwell B (south side); descend 5 floors to ground level"

**`assistance`** (optional):  
Array of assistance types needed. See [Assistance section](#assistance-object).

**`equipment`** (optional):  
Array of equipment needed (e.g., `["Evacuation chair", "Transfer board"]`).

**`refuge`** (optional):  
If person cannot evacuate immediately, where should they wait?
```json
"refuge": {
  "location": "Floor 12 north stairwell landing (behind fire door)",
  "waitCondition": "Press red intercom button; call 999; wait for FRS or concierge"
}
```

**`musterPoint`** (optional):  
Where to go after evacuating (e.g., `"Assembly Point A (car park, north side)"`).

**`evacuationLift`** (optional):  
If route uses an evacuation lift. See [Evacuation Lift section](#evacuation-lift-object).

**`preconditions`** (optional):  
Conditions that must be met for this route (e.g., `"Use only if Lift 2 is operational"`).

**`contingency`** (optional):  
What to do if this route is blocked (e.g., `"If stairwell blocked, return to flat, close door, call 999"`).

**Common mistakes:**
- ❌ Forgetting to name one route "Primary" (schema validation will fail!)
- ❌ Vague descriptions ("go downstairs" - which stairs? how many floors?)
- ❌ Not providing a secondary route (what if primary is blocked?)
- ✅ Be specific, include landmarks, give fallback options

---

#### `evacuationProcedure`

**Type:** Array of procedure step objects  
**Required:** ✅ Yes (minimum 1)

**What it is:**  
Step-by-step instructions for what to do when the alarm sounds.

**Why it exists:**  
During an emergency, people need **clear, sequential instructions**. Not a description of routes, but **actions to take**.

**How to use it:**  
Write numbered steps in order of execution:

```json
"evacuationProcedure": [
  {
    "order": 1,
    "instruction": "On hearing fire alarm, remain calm."
  },
  {
    "order": 2,
    "instruction": "Transfer to wheelchair if not already in it."
  },
  {
    "order": 3,
    "instruction": "Collect mobile phone and keys. Do not collect other belongings."
  },
  {
    "order": 4,
    "instruction": "Check door for heat. If hot, do not open. Call 999 and inform FRS you are in Flat 12A."
  },
  {
    "order": 5,
    "instruction": "If door not hot, open carefully and check corridor for smoke/fire."
  },
  {
    "order": 6,
    "instruction": "If corridor clear, exit flat and close door behind you."
  },
  {
    "order": 7,
    "instruction": "Proceed to evacuation lift (Lift 2, west wing, 20m right from flat door)."
  },
  {
    "order": 8,
    "instruction": "Press lift call button. Concierge will meet you if available."
  }
]
```

**Good practices:**
- ✅ Start with `order: 1` and increment
- ✅ One action per step
- ✅ Use imperative mood ("Do this", not "You should do this")
- ✅ Include decision points ("If X, then Y; otherwise Z")
- ✅ Reference specific locations ("Lift 2, west wing" not just "the lift")

**Common mistakes:**
- ❌ Out-of-order steps (order: 1, 3, 2, 4)
- ❌ Too vague ("Evacuate safely")
- ❌ Too long per step (break into smaller steps)
- ✅ Clear, actionable, sequential

---

#### `planSummary`

**Type:** String (max 4000 characters)  
**Required:** ✅ Yes

**What it is:**  
A plain-language summary of the entire evacuation plan.

**Why it exists:**  
Provides a **high-level overview** before diving into routes and procedures. Useful for:
- Quick understanding of the plan
- Sharing with FRS
- Plain-language version for residents

**How to use it:**  
Write 2-4 sentences covering:
1. Who this is for (without identifying details if privacy-sensitive)
2. What their main evacuation method is
3. Who will assist (if anyone)
4. Any critical equipment or dependencies

**Good example:**
```json
"planSummary": "Alex Thompson is a manual wheelchair user living in Flat 12A, Churchill House (12th floor). In the event of a fire alarm, Alex should evacuate via the evacuation lift (Lift 2, west wing) with assistance from the concierge team. If the lift is unavailable, Alex should proceed to the designated refuge on Floor 12 (north stairwell) and call the concierge emergency line or 999. Two trained staff will assist with evacuation chair if required."
```

**Common mistakes:**
- ❌ Too technical ("Egress via north stairwell per BS 9999 protocol")
- ❌ Too vague ("Person needs help evacuating")
- ❌ Including medical diagnoses ("Has MS and uses wheelchair" - just say "uses wheelchair")
- ✅ Plain language, specific but privacy-conscious, actionable

---

#### `metadata`

**Type:** Object  
**Required:** ✅ Yes

**What it is:**  
Information about the PEEP document itself (who created it, when, status).

**Why it exists:**  
Auditability and golden thread compliance (Building Safety Act 2022). Allows tracking of:
- Who created/approved the plan
- When it was created
- What status it's in (draft, approved, superseded)
- How it was created (in-person, phone, self-assessment)

**Required fields within metadata:**
- `createdAt` ✅
- `createdBy` ✅
- `status` ✅

**Full metadata example:**
```json
"metadata": {
  "createdAt": "2025-10-14T10:00:00Z",
  "createdBy": "housing.officer@manchesterhousing.example.org",
  "method": "in_person",
  "versionRef": "1.0",
  "documentRef": "https://manchesterhousing.example.org/peeps/peep-alex-thompson-001",
  "status": "approved",
  "goldenThreadUrn": "urn:gb:bsa:golden-thread:building-churchill-house-001",
  "eesRef": "ees-building-churchill-house-uuid-001"
}
```

**Field explanations:**

**`createdAt`** (required):  
ISO 8601 timestamp. Use UTC if possible: `"2025-10-14T10:00:00Z"` or with timezone: `"2025-10-14T10:00:00+01:00"`.

**`createdBy`** (required):  
Identifier of who created it. Can be:
- Email address: `"housing.officer@example.org"`
- User ID: `"user-12345"`
- Name + role: `"Jordan Smith, Housing Officer"`

**`status`** (required):  
Document control status. Allowed values:
- `"draft"` - work in progress, not yet approved
- `"approved"` - approved for use
- `"superseded"` - replaced by a newer version
- `"withdrawn"` - no longer valid, not replaced

**`method`** (optional):  
How the PEEP was created:
- `"self_assessment"` - person filled it in themselves
- `"phone_interview"` - created over the phone
- `"in_person"` - face-to-face assessment
- `"other"` - something else

**`versionRef`** (optional):  
Your internal version number (e.g., `"1.0"`, `"2.1"`).

**`documentRef`** (optional):  
URL or URI where this PEEP is stored (for golden thread linkage).

**`goldenThreadUrn`** (optional):  
Reference to the building's golden thread record (UK Building Safety Act 2022).

**`eesRef`** (optional):  
Reference to the building's EES (Building) document ID.

**Common mistakes:**
- ❌ Using local time without timezone (`"2025-10-14T10:00:00"` - ambiguous!)
- ❌ Forgetting to update `status` when approved
- ✅ Use UTC; track status lifecycle

---

### Top-Level Optional Fields

#### `language`

**Type:** String (ISO 639-1 language code)  
**Required:** ❌ No (optional)  
**Example:** `"en-GB"`, `"cy-GB"`, `"pl-PL"`

**What it is:**  
The primary language this PEEP is written in.

**Why it exists:**  
Supports multilingual plans (Equality Act 2010 reasonable adjustments).

**How to use it:**  
Use ISO 639-1 two-letter codes, optionally with region:
- `"en-GB"` - British English
- `"cy-GB"` - Welsh
- `"pl-PL"` - Polish
- `"ur-PK"` - Urdu

If omitted, assume English unless `translations` array is present.

**See also:** `translations` array for multiple language versions.

---

#### `translations`

**Type:** Array of translation objects  
**Required:** ❌ No (optional)

**What it is:**  
Translations of the plan summary in other languages.

**Why it exists:**  
Accessibility for non-English speakers.

**How to use it:**
```json
"translations": [
  {
    "language": "pl-PL",
    "summary": "W przypadku alarmu pożarowego, opuść budynek przez klatkę schodową A."
  },
  {
    "language": "ur-PK",
    "summary": "آگ کا الارم سننے پر، سیڑھی A سے عمارت چھوڑ دیں۔"
  }
]
```

**When to use:**  
When you have residents who prefer languages other than the main plan language.

---

#### `validity`

**Type:** Object  
**Required:** ❌ No (optional)

**What it is:**  
Date range when this PEEP is valid.

**Why it exists:**  
PEEPs have lifespans. UK Residential Regs 2025 require annual review.

**How to use it:**
```json
"validity": {
  "effectiveFrom": "2025-10-15",
  "effectiveTo": "2026-10-15"
}
```

**Dates are ISO 8601 `YYYY-MM-DD` format.**

**Common pattern:**
- `effectiveFrom`: date PEEP was approved
- `effectiveTo`: date next review is due (typically 12 months later)

---

#### `communications`

**Type:** Object  
**Required:** ❌ No (optional, but highly recommended)

**What it is:**  
How the person perceives alarms and raises the alarm.

**Why it exists:**  
People with hearing or vision impairments need alternative alarm methods.

**How to use it:**
```json
"communications": {
  "alarmHearingAbility": "needs_visual",
  "alertingMethods": [
    "visual_beacons"
  ],
  "alertingNeeds": [
    "visual_beacons",
    "high_contrast"
  ],
  "raiseAlarmMethods": [
    "manual_call_point",
    "intercom",
    "call_999"
  ],
  "notes": "Visual alarm beacons installed at workstation. Manual call points are tactile."
}
```

**Fields:**

**`alarmHearingAbility`:**  
- `"hears_audible"` - can hear standard alarm
- `"needs_visual"` - requires visual alarm
- `"needs_vibration"` - requires vibrating alert
- `"unknown"` - not assessed

**`alertingMethods`:**  
What's available in the building:
- `"visual_beacons"`
- `"vibrating_pager"`
- `"text_message"`
- `"phone_call"`
- `"door_knock"`
- `"public_address"`

**`alertingNeeds`:**  
What this person needs:
- `"visual_beacons"`
- `"vibrating_pager"`
- `"text_only"`
- `"captioned_media"`
- `"bsl_interpreter_required"`
- `"high_contrast"`
- `"large_print"`
- `"plain_language"`

**`raiseAlarmMethods`:**  
How this person can raise the alarm:
- `"manual_call_point"`
- `"pull_cord"`
- `"intercom"`
- `"phone_call"`
- `"call_999"`
- `"pendant"`
- `"other"`

**When to include:**  
If person has any sensory impairment affecting alarm perception.

---

#### `nighttimeConsiderations`

**Type:** String (max 4000 chars)  
**Required:** ❌ No

**What it is:**  
Special considerations for evacuating at night.

**Example:**
```json
"nighttimeConsiderations": "Alex sleeps in bedroom (rear of flat); alarm sounder is audible in bedroom. If alarm sounds at night, Alex will wake, transfer to wheelchair, and proceed to evacuation lift or refuge as per primary/secondary routes. Concierge team will check on Floor 12 residents during night-time alarms."
```

**When to include:**  
If person has difficulty waking to alarms, sleeps in hearing aids, needs help at night.

---

#### `trainingDrills`

**Type:** String (max 4000 chars)  
**Required:** ❌ No

**What it is:**  
Training and fire drill arrangements.

**Example:**
```json
"trainingDrills": "Alex will participate in annual fire drill (daytime); concierge team will arrange accessible drill participation (evacuation lift test). Alex has been briefed on secondary route and refuge location. Next drill: March 2026."
```

**Why it exists:**  
FSO 2005 Article 21 requires fire safety training.

---

#### `criticalDependencies`

**Type:** Array of dependency objects  
**Required:** ❌ No

**What it is:**  
Equipment or support the person depends on (oxygen, power, carer).

**Example:**
```json
"criticalDependencies": [
  {
    "type": "oxygen_support",
    "notes": "Uses oxygen concentrator 16 hours/day. Portable oxygen cylinders available for evacuation. Concentrator creates additional fire risk if smoking present."
  },
  {
    "type": "power_for_mobility_device",
    "notes": "Powered wheelchair requires daily charging. Battery life: 8 hours. Spare battery kept in flat."
  }
]
```

**Dependency types:**
- `"oxygen_support"`
- `"power_for_mobility_device"`
- `"carer_support"`
- `"medication_reminder"`
- `"other"`

**When to include:**  
If dependency affects evacuation (e.g., can't leave without oxygen, wheelchair battery might be flat).

---

#### `responsibilities`

**Type:** Array of responsibility objects  
**Required:** ❌ No

**What it is:**  
Who is responsible for assisting with evacuation.

**Example:**
```json
"responsibilities": [
  {
    "role": "Concierge Team (24/7 on-site)",
    "organisation": "Manchester Housing Ltd",
    "contact": {
      "phone": "+441612345678",
      "email": "concierge@manchesterhousing.example.org"
    },
    "notes": "Responsible for evacuation lift operation, refuge monitoring, and evacuation chair assistance."
  },
  {
    "role": "Building Fire Warden",
    "organisation": "Manchester Housing Ltd",
    "contact": {
      "phone": "+441612345679"
    },
    "notes": "Responsible for roll call at Assembly Point A."
  }
]
```

**When to include:**  
If specific people/roles have responsibilities in this PEEP.

---

#### `attachments`

**Type:** Array of attachment objects  
**Required:** ❌ No

**What it is:**  
Supporting documents (floor plans, equipment instructions, photos).

**Example:**
```json
"attachments": [
  {
    "label": "Floor plan showing evacuation routes",
    "uri": "https://manchesterhousing.example.org/attachments/floor-12-evac-plan.pdf",
    "contentType": "application/pdf",
    "sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
  }
]
```

**When to include:**  
If you have visual aids, equipment manuals, or other supporting docs.

---

#### `references`

**Type:** Object  
**Required:** ❌ No (but highly recommended)

**What it is:**  
Links to source documents (PCFRA, EES Building).

**Example:**
```json
"references": {
  "pcfraId": "pcfra-person-alex-thompson-uuid-001",
  "buildingStatementEesId": "ees-building-churchill-house-uuid-001"
}
```

**Why it exists:**  
Golden thread traceability. Shows what assessment informed this plan.

**When to include:**  
Always! Links PEEP → PCFRA → Consent chain.

---

#### `consent`

**Type:** Object (embedded consent record)  
**Required:** ❌ No (but required for UK RPEEP compliance)  
**Schema:** `common/consent.schema.json`

**What it is:**  
Record of whether person consented to this PEEP.

**Example:**
```json
"consent": {
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep", "share_with_frs"],
  "method": "written"
}
```

**When to include:**  
- ✅ Always for UK Residential Regs 2025 (RPEEP)
- ✅ If consent was explicitly given or refused
- ❌ Optional for workplace (employer duty, not consent-based)

**See also:** [Consent Schema Guide](consent_schema_guide.md)

---

#### `reviews`

**Type:** Array of review objects  
**Required:** ❌ No  
**Schema:** `common/review.schema.json`

**What it is:**  
Record of when this PEEP was reviewed.

**Example:**
```json
"reviews": [
  {
    "reviewed_at": "2026-10-14T11:00:00Z",
    "reviewed_by": {
      "name": "Jordan Smith",
      "role": "Housing Officer"
    },
    "outcome": "confirmed_no_change",
    "next_due": "2027-10-14"
  }
]
```

**When to include:**  
After annual reviews (UK Residential Regs 2025 Reg 5).

**See also:** [Review Schema Guide](review_schema_guide.md)

---

#### `extensions`

**Type:** Object  
**Required:** ❌ No

**THIS IS THE KEY ABSTRACTION!**

**What it is:**  
Jurisdiction-specific or vendor-specific fields that don't belong in the core schema.

**Why it exists:**  
Keeps the **core schema portable** while allowing **local customisation**.

**See:** [Core vs Extensions Pattern](#core-vs-extensions-pattern) below

---

## Core vs Extensions Pattern

### The Problem This Solves

OpenPEEP is designed to be **globally adoptable**. But different countries have different regulations:

- **UK** has Residential Regs 2025, Building Safety Act 2022, "relevant resident" concept
- **EU** has different accessibility directives, GDPR-specific requirements
- **USA** has ADA, OSHA, NFPA codes, state-level regulations

If we put UK-specific fields in the core schema, it's not portable to EU/USA.  
If we put EU-specific fields in the core schema, it's not portable to UK/USA.

**Solution:** Core schema contains **only portable, universal fields**. Jurisdiction-specific fields go in **`extensions`**.

### Core Schema = Portable

The core schema fields are **internationally applicable**:
- ✅ `personProfile`, `address`, `evacuationStrategy` - universal concepts
- ✅ `routes`, `evacuationProcedure` - every country needs these
- ✅ `communications`, `assistance` - accessibility is global

### Extensions = Jurisdiction-Specific

The `extensions` object holds **local variations**:
```json
"extensions": {
  "gbr": {
    "relevantResident": true,
    "residentFacingStatementRef": "ees-personal-uuid-001"
  },
  "eu": {
    "gdprLawfulBasis": "consent"
  },
  "usa": {
    "adaCompliant": true,
    "nfpaCodeVersion": "NFPA101-2021"
  },
  "acmeHousing": {
    "internalRef": "TENANT-12345",
    "propertyCode": "CH-12A"
  }
}
```

### How Extensions Work

**Structure:**
```json
"extensions": {
  "<namespace>": {
    "<field>": <value>,
    ...
  }
}
```

**Namespace conventions:**
- `gbr` - United Kingdom (ISO 3166-1 alpha-3)
- `eu` - European Union
- `usa` - United States
- `<vendorName>` - Vendor-specific (e.g., `acmeHousing`, `techCorp`)

**Rules:**
1. **Never modify core schema** - add to extensions instead
2. **Use country codes** for jurisdiction extensions (ISO 3166-1 alpha-3)
3. **Use vendor name** for vendor extensions
4. **Document your extensions** - create a profile guide

### When to Use Core vs Extensions

| Requirement | Where it goes | Why |
|-------------|---------------|-----|
| Person needs wheelchair | ✅ **Core** (PCFRA capability) | Universal accessibility concept |
| Building has evacuation lift | ✅ **Core** (routes.evacuationLift) | Universal evacuation method |
| "Relevant resident" under UK Regs 2025 | ❌ **Extension** (gbr.relevantResident) | UK-specific legal term |
| NFPA code compliance (USA) | ❌ **Extension** (usa.nfpaCodeVersion) | USA-specific standard |
| Internal tenant ID | ❌ **Extension** (vendor.internalRef) | Vendor-specific data |

### Example: Same PEEP for UK vs EU vs USA

**UK PEEP (using gbr extensions):**
```json
{
  "schemaVersion": "1.0.0",
  "peepId": "peep-uk-001",
  "personProfile": { "personRef": "person-001" },
  "address": { "countryCode": "GB", "uprn": "100012345678" },
  "evacuationStrategy": "Stay Put",
  "routes": [...],
  "evacuationProcedure": [...],
  "planSummary": "...",
  "metadata": { ... },
  "extensions": {
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-001",
      "informationDutyEvidenceRef": "https://example.org/info-duty-evidence.pdf",
      "stayingPutBoundary": "Stay put unless fire is in your flat or FRS instructs you to leave"
    }
  }
}
```

**EU PEEP (using eu extensions):**
```json
{
  "schemaVersion": "1.0.0",
  "peepId": "peep-eu-001",
  "personProfile": { "personRef": "person-002" },
  "address": { "countryCode": "FR" },
  "evacuationStrategy": "Simultaneous Evacuation",
  "routes": [...],
  "evacuationProcedure": [...],
  "planSummary": "...",
  "metadata": { ... },
  "extensions": {
    "eu": {
      "gdprLawfulBasis": "consent",
      "dataProtectionOfficerContact": "dpo@example.fr",
      "accessibilityDirectiveCompliant": true
    }
  }
}
```

**USA PEEP (using usa extensions):**
```json
{
  "schemaVersion": "1.0.0",
  "peepId": "peep-usa-001",
  "personProfile": { "personRef": "person-003" },
  "address": { "countryCode": "US" },
  "evacuationStrategy": "Simultaneous Evacuation",
  "routes": [...],
  "evacuationProcedure": [...],
  "planSummary": "...",
  "metadata": { ... },
  "extensions": {
    "usa": {
      "adaCompliant": true,
      "nfpaCodeVersion": "NFPA101-2021",
      "oshaApplicable": true,
      "stateRegulation": "CA_Title24"
    }
  }
}
```

**Notice:** The core fields are **identical structure**. Only extensions differ!

---

## Jurisdiction Profiles

### UK Profile (gbr)

**Use when:** Implementing RPEEP under UK Residential Regs 2025

**Extension fields:**

```json
"extensions": {
  "gbr": {
    "relevantResident": true,
    "residentFacingStatementRef": "ees-personal-uuid-001",
    "informationDutyEvidenceRef": "https://example.org/evidence/info-duty.pdf",
    "stayingPutBoundary": "Stay put unless fire is in your flat or FRS tells you to leave"
  }
}
```

**Field meanings:**

**`relevantResident`** (boolean):  
Is this person a "relevant resident" under Residential Regs 2025? Triggers RPEEP duty.

**`residentFacingStatementRef`** (string):  
Link to EES Personal document (plain-language evacuation instructions for resident). Required under Reg 10.

**`informationDutyEvidenceRef`** (string):  
Evidence that information was provided to resident/FRS (Regs 10/12 compliance).

**`stayingPutBoundary`** (string):  
Plain-language description of when "stay put" applies and when it doesn't.

**Regulatory mapping:**
- Reg 3 (duty to prepare) → `relevantResident`, `peepRequired` (from PCFRA)
- Reg 4 (consent) → `consent` object
- Reg 5 (review) → `reviews` array
- Reg 10 (information) → `residentFacingStatementRef`
- Reg 12 (share with FRS) → `consent.scope` includes `"share_with_frs"`

---

### EU Profile (eu)

**Use when:** Implementing in EU member states

**Extension fields:**

```json
"extensions": {
  "eu": {
    "gdprLawfulBasis": "consent",
    "dataProtectionOfficerContact": "dpo@example.eu",
    "accessibilityDirectiveCompliant": true,
    "sevesoDirectiveApplicable": false
  }
}
```

**Field meanings:**

**`gdprLawfulBasis`** (string):  
GDPR Article 6 lawful basis: `"consent"`, `"legal_obligation"`, `"vital_interests"`, `"public_task"`.

**`dataProtectionOfficerContact`** (string):  
DPO contact (GDPR Article 37-39).

**`accessibilityDirectiveCompliant`** (boolean):  
Complies with EU Accessibility Act (2019).

**`sevesoDirectiveApplicable`** (boolean):  
Subject to Seveso III Directive (major accident hazards).

---

### USA Profile (usa)

**Use when:** Implementing in USA

**Extension fields:**

```json
"extensions": {
  "usa": {
    "adaCompliant": true,
    "nfpaCodeVersion": "NFPA101-2021",
    "oshaApplicable": true,
    "stateRegulation": "CA_Title24"
  }
}
```

**Field meanings:**

**`adaCompliant`** (boolean):  
Complies with Americans with Disabilities Act.

**`nfpaCodeVersion`** (string):  
NFPA code version (e.g., NFPA 101 Life Safety Code 2021 edition).

**`oshaApplicable`** (boolean):  
Subject to OSHA workplace safety regulations.

**`stateRegulation`** (string):  
State-specific regulation (e.g., California Title 24).

---

### Vendor Extensions

**Use when:** Adding internal fields for your organization

**Example:**

```json
"extensions": {
  "acmeHousing": {
    "tenantId": "TENANT-12345",
    "propertyCode": "CH-12A",
    "lastInspectionDate": "2025-09-15",
    "riskScore": 3
  }
}
```

**Rules:**
- Use your organization name as namespace
- Don't conflict with jurisdiction extensions
- Document what your extensions mean
- Don't rely on other vendors understanding your extensions

---

## Complete Examples

### Example 1: UK Residential RPEEP (Wheelchair User, Consent Given)

```json
{
  "schemaVersion": "1.0.0",
  "peepId": "peep-alex-thompson-uuid-001",
  "personProfile": {
    "personRef": "person-alex-thompson-uuid-001",
    "name": "Alex Thompson",
    "preferredLanguage": "en-GB"
  },
  "address": {
    "buildingName": "Churchill House",
    "subBuilding": "Flat 12A",
    "townCity": "Manchester",
    "postcode": "M1 1AA",
    "countryCode": "GB",
    "uprn": "100012345678",
    "buildingRef": "building-churchill-house-001"
  },
  "evacuationStrategy": "Simultaneous Evacuation",
  "language": "en-GB",
  "planSummary": "Alex Thompson is a manual wheelchair user living in Flat 12A, Churchill House (12th floor). In the event of a fire alarm, Alex should evacuate via the evacuation lift (Lift 2, west wing) with assistance from the concierge team. If the lift is unavailable, Alex should proceed to the designated refuge on Floor 12 and call for assistance.",
  "metadata": {
    "createdAt": "2025-09-15T14:00:00Z",
    "createdBy": "housing.officer@manchesterhousing.example.org",
    "method": "in_person",
    "status": "approved",
    "goldenThreadUrn": "urn:gb:bsa:golden-thread:building-churchill-house-001"
  },
  "validity": {
    "effectiveFrom": "2025-09-20",
    "effectiveTo": "2026-09-20"
  },
  "routes": [
    {
      "name": "Primary",
      "pathDescription": "Exit Flat 12A; turn right to west wing corridor; proceed 20 metres to Lift 2 (evacuation lift). Press call button. Enter lift; press Ground Floor button. Exit lift; proceed through main foyer to Assembly Point A (car park, north side).",
      "assistance": [
        {
          "type": "lift_with_attendant",
          "personsRequired": 1,
          "role": "Concierge team (24/7 on-site)",
          "availability": "24/7"
        }
      ],
      "evacuationLift": {
        "used": true,
        "standard": "BS EN 81-76:2011",
        "attendantRequired": true
      },
      "musterPoint": "Assembly Point A (car park, north side)"
    },
    {
      "name": "Secondary",
      "pathDescription": "If Lift 2 unavailable: exit Flat 12A; turn left to north stairwell; proceed to refuge (Floor 12 landing, behind fire door). Use refuge intercom (press red button) to call concierge or call 999. Wait for assistance with evacuation chair.",
      "assistance": [
        {
          "type": "evac_chair",
          "personsRequired": 2,
          "role": "Concierge team or Fire and Rescue Service"
        }
      ],
      "equipment": ["Evacuation chair (stored in refuge cupboard, Floor 12)"],
      "refuge": {
        "location": "Floor 12 landing, north stairwell (behind fire door)",
        "waitCondition": "Press red intercom button or call 999. Wait for trained assistance."
      },
      "musterPoint": "Assembly Point A (car park, north side)"
    }
  ],
  "communications": {
    "alarmHearingAbility": "hears_audible",
    "raiseAlarmMethods": ["manual_call_point", "intercom", "call_999"]
  },
  "evacuationProcedure": [
    {
      "order": 1,
      "instruction": "On hearing fire alarm, remain calm. Transfer to wheelchair if not already in it."
    },
    {
      "order": 2,
      "instruction": "Collect mobile phone and keys. Do not collect other belongings."
    },
    {
      "order": 3,
      "instruction": "Check door for heat. If hot, do not open. Call 999 and inform FRS you are in Flat 12A."
    },
    {
      "order": 4,
      "instruction": "If door not hot, open carefully and check corridor for smoke/fire."
    },
    {
      "order": 5,
      "instruction": "If corridor clear, exit flat and close door behind you. Turn right to Lift 2."
    },
    {
      "order": 6,
      "instruction": "Press evacuation lift call button. Concierge will meet you if available."
    },
    {
      "order": 7,
      "instruction": "Enter lift; press Ground Floor button. Exit at ground floor; proceed to Assembly Point A."
    },
    {
      "order": 8,
      "instruction": "If lift unavailable: go to refuge (Floor 12, north stairwell). Press red intercom or call 999. Wait for assistance."
    }
  ],
  "references": {
    "pcfraId": "pcfra-person-alex-thompson-uuid-001",
    "buildingStatementEesId": "ees-building-churchill-house-uuid-001"
  },
  "consent": {
    "state": "given",
    "capturedAt": "2025-09-15T10:30:00Z",
    "capturedBy": "housing.officer@manchesterhousing.example.org",
    "scope": ["pcfra", "peep", "rpeep", "ees_person_statement", "share_with_frs"],
    "method": "written"
  },
  "reviews": [],
  "extensions": {
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-alex-thompson-uuid-001",
      "informationDutyEvidenceRef": "https://manchesterhousing.example.org/evidence/info-duty-alex-thompson.pdf"
    }
  }
}
```

---

## Validation

### Schema Validation with AJV

```bash
# Install AJV
npm install ajv ajv-formats

# Validate your PEEP
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/peep.schema.json \
  -d your-peep.json
```

### Common Validation Errors

**Error:** `"should have required property 'peepId'"`  
**Fix:** Add a UUID v4 to `peepId` field.

**Error:** `"data/routes should contain at least 1 item(s)"`  
**Fix:** Add at least one route in `routes` array.

**Error:** `"data/routes should contain a route with name 'Primary'"`  
**Fix:** Ensure one route has `"name": "Primary"`.

**Error:** `"data/schemaVersion should match pattern '^\\d+\\.\\d+\\.\\d+$'"`  
**Fix:** Use full SEMVER format: `"1.0.0"` not `"1"`.

---

## FAQs

### Q: PEEP vs RPEEP - what's the difference?

**A:** RPEEP is a **process profile**, not a different schema.

- **PEEP** = the schema (this document)
- **RPEEP** = UK residential context applying the PEEP schema

RPEEP adds:
- Consent requirement (Residential Regs 2025 Reg 4)
- Annual review requirement (Reg 5)
- Information duty to residents (Reg 10)
- Sharing with FRS (Reg 12)
- "Relevant resident" determination

**Implementation:** Use PEEP schema + UK extensions (`gbr`) + consent + reviews.

---

### Q: Do I need consent for every PEEP?

**A:** Depends on context:

- ✅ **UK Residential (RPEEP):** Yes, must offer and record consent (Reg 4)
- ❌ **UK Workplace:** No, employer has legal duty (FSO 2005)
- ✅ **Good practice:** Yes, always seek consent where possible

---

### Q: What if someone refuses consent?

**A:** Record the refusal:

```json
"consent": {
  "state": "refused",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep"],
  "method": "verbal",
  "reason": "Prefers to manage own evacuation"
}
```

You still have a duty to offer. Re-offer periodically (e.g., annual tenancy review).

---

### Q: How do I link PEEP → PCFRA → Consent?

**A:** Use references:

1. **PCFRA** has `consentRef` → links to Consent
2. **PCFRA** has `pcfraId` → unique ID
3. **PEEP** has `references.pcfraId` → links to PCFRA
4. **PEEP** has `consent` (embedded) → links to Consent

**Chain:** PEEP → PCFRA → Consent

---

### Q: Can one person have multiple PEEPs?

**A:** Yes! Different PEEPs for:
- Different buildings (home vs workplace)
- Different roles (employee vs visitor)
- Different time periods (superseded versions)

Use different `peepId` for each.

---

### Q: How do I handle PEEP updates?

**Option 1: Amend (minor change)**  
- Keep same `peepId`
- Update `metadata.status` to `"approved"`
- Add review record with `outcome: "amended"`

**Option 2: Supersede (major change)**  
- Create new PEEP with new `peepId`
- Update old PEEP `metadata.status` to `"superseded"`
- Add review record with `outcome: "superseded"` and `new_document_ref`

---

### Q: What's the difference between `planSummary` and `evacuationProcedure`?

**A:**
- **`planSummary`:** High-level overview (2-4 sentences). For quick understanding.
- **`evacuationProcedure`:** Detailed step-by-step instructions. For execution.

Think: summary = elevator pitch; procedure = cookbook recipe.

---

### Q: Do I include medical diagnoses in a PEEP?

**A:** ❌ **No.** Medical diagnoses go in PCFRA (if at all - prefer functional descriptions).

PEEP should reference PCFRA but focus on **what to do**, not **why** (medical reasons).

---

### Q: How long should I keep PEEPs?

**A:** UK guidance:
- **Active PEEP:** Until superseded or person leaves
- **Superseded PEEP:** Duration of occupancy + 6 years
- **After occupancy ends:** 6 years (regulatory retention; Limitation Act 1980)

---

## Next Steps

- **Read:** [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - how to assess capabilities
- **Read:** [Consent Schema Guide](consent_schema_guide.md) - how to record consent
- **Read:** [Review Schema Guide](review_schema_guide.md) - how to track reviews
- **Validate:** Use AJV to check your PEEPs
- **Implement:** Build your PEEP creation workflow

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

