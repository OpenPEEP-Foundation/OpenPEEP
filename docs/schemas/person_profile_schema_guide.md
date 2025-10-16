# Person Profile Schema Guide — Complete Reference

**Schema:** `common/person_profile.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Core vs Extensions Pattern](#core-vs-extensions-pattern)
5. [Data Minimisation](#data-minimisation)
6. [Complete Examples](#complete-examples)
7. [Validation](#validation)
8. [FAQs](#faqs)

---

## Overview

### What is a Person Profile?

A **Person Profile** is a **minimal identity anchor** for an individual in OpenPEEP documents.

**What it contains:**
- ✅ Identity information (name, reference, age)
- ✅ Contact details (optional)
- ✅ Communication preferences (optional)

**What it does NOT contain:**
- ❌ Impairments or disabilities
- ❌ Evacuation needs
- ❌ Medical data
- ❌ Fire risk information

**Think of it as:** A business card, not a medical record.

### Why Separate Identity from Assessment?

**Separation of concerns:**
- **Person Profile** = Who they are
- **PCFRA** = What they can do (evacuation capability)
- **PEEP** = What they should do (evacuation plan)

**Data minimisation:**
- Person Profile can be shared widely (name, contact)
- PCFRA contains sensitive capability data (limited sharing)
- Keeps identity separate from health-related information

### When Do You Use Person Profile?

Person Profile is **embedded or referenced** in:
- PEEP (`personProfile` field)
- PCFRA (`personRef` field)
- EES Personal (`person_profile_ref` field)
- Consent (`personRef` field)

**It's the common identifier** linking all documents for one person.

---

## Quick Start

### Minimal Person Profile

```json
{
  "personRef": "person-uuid-12345"
}
```

**That's it!** Just a unique identifier. Everything else is optional.

### Standard Person Profile

```json
{
  "personRef": "person-emma-wilson-uuid-001",
  "name": "Emma Wilson",
  "preferredLanguage": "en-GB",
  "contact": {
    "phone": "+447700900123",
    "email": "emma.wilson@example.com"
  }
}
```

### Person Profile with Communication Preferences

```json
{
  "personRef": "person-rhys-davies-uuid-003",
  "name": "Rhys Davies",
  "preferredLanguage": "cy-GB",
  "contact": {
    "email": "rhys.davies@example.com"
  },
  "communicationPreferences": {
    "channels": ["email", "sms"],
    "formatPreferences": ["large_print", "high_contrast"],
    "notes": "Prefers Welsh language communications. Large print minimum 16pt."
  }
}
```

---

## Complete Field Reference

### Required Fields

#### `personRef`

**Type:** String  
**Required:** ✅ Yes (ONLY required field!)  
**Example:** `"person-emma-wilson-uuid-001"`

**What it is:**  
A unique identifier for this person within your system.

**Why it exists:**  
Links all documents (PCFRA, PEEP, Consent, EES) to the same person.

**How to use it:**

**Option 1: UUID v4 (recommended):**
```json
"personRef": "person-f47ac10b-58cc-4372-a567-0e02b2c3d479"
```

**Option 2: System ID with prefix:**
```json
"personRef": "person-TENANT-12345"
```

**Option 3: Any unique string:**
```json
"personRef": "person-emma-wilson-001"
```

**Best practices:**
- ✅ Use a format that's unique in your system
- ✅ Include a prefix (`person-`) for clarity
- ✅ Make it opaque (not revealing identity)
- ❌ Don't use name alone (`"emma-wilson"` - not unique!)
- ❌ Don't use sensitive data (`"nhs-number-1234567"` - privacy risk!)

**Common mistakes:**
- Using social security number, NHS number (privacy violation)
- Using email address (can change, not stable)
- Using name (not unique - which "John Smith"?)

**Legal significance:**  
This is the **golden thread** identifier (Building Safety Act 2022). Must be stable across document lifecycle.

---

### Optional Fields

#### `name`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Legal or chosen name of the person.

**How to use it:**

**Full name:**
```json
"name": "Emma Wilson"
```

**With title:**
```json
"name": "Dr. Aisha Patel"
```

**Chosen name (not legal name):**
```json
"name": "Alex Thompson"
```

**When to include:**
- ✅ When person has consented to sharing name
- ✅ When PEEP needs to be human-readable
- ❌ When maximum privacy required (use `alias` instead)

**Data minimisation note:**  
Name is **personal data** (UK GDPR). Only include if necessary.

**Best practices:**
- ✅ Use the name person prefers (Equality Act 2010 - respect for identity)
- ✅ Ask permission before using
- ❌ Don't assume pronouns or titles

**Legal significance:**  
If person is trans/non-binary, use their **chosen name** (Equality Act 2010).

---

#### `alias`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Optional alias or nickname. Can be used **instead of** `name` in privacy-sensitive contexts.

**When to use it:**

**Transient occupancy (hotels, hostels):**
```json
{
  "personRef": "person-guest-305-uuid",
  "alias": "Guest, Room 305"
}
```

**Maximum privacy:**
```json
{
  "personRef": "person-anon-uuid-001",
  "alias": "Resident, Flat 12A"
}
```

**Nickname preference:**
```json
{
  "personRef": "person-uuid-001",
  "name": "Alexander Thompson",
  "alias": "Alex"
}
```

**When to use:**
- ✅ Short-term accommodation (guest doesn't want name shared)
- ✅ Safeguarding cases (domestic abuse, stalking)
- ✅ Privacy-first approach (room/flat number instead of name)

**Best practice:**  
Use `alias` alone OR `name` + `alias`. Don't use `alias` instead of `name` unless necessary.

---

#### `dob`

**Type:** String (ISO 8601 date: `YYYY-MM-DD`)  
**Required:** ❌ No (optional)

**What it is:**  
Date of birth.

**How to use it:**
```json
"dob": "1992-11-08"
```

**When to include:**
- ✅ When legally required (e.g., age verification)
- ✅ When precise age affects evacuation (children vs adults vs older adults)
- ❌ When approximate age is sufficient (use `ageBand` instead)

**Data minimisation warning:**  
DOB is **sensitive personal data**. Only include if truly necessary.

**Alternative:** Use `ageBand` for approximate age (better privacy).

**Common mistakes:**
- ❌ Wrong format: `"08/11/1992"` (use ISO 8601: `YYYY-MM-DD`)
- ❌ Including when not needed (privacy violation)
- ✅ Prefer `ageBand` if exact DOB not required

---

#### `ageBand`

**Type:** String (enum)  
**Required:** ❌ No (optional)  
**Allowed values:** `"child"`, `"adult"`, `"older_adult"`

**What it is:**  
Approximate age group (better privacy than exact DOB).

**How to use it:**

```json
"ageBand": "older_adult"
```

**When to use:**
- ✅ When you need age category but not exact age
- ✅ For privacy (data minimisation)
- ✅ For risk profiling (NFCC framework considers age as risk factor)

**Age band definitions (guidance, not strict):**
- `"child"` - Under 18
- `"adult"` - 18-64
- `"older_adult"` - 65+

**Note:** These are guidance. Adjust per your jurisdiction/context.

**Best practice:**  
Use `ageBand` INSTEAD OF `dob` where possible (better privacy).

**Example (privacy-conscious):**
```json
{
  "personRef": "person-uuid-001",
  "alias": "Resident, Flat 12",
  "ageBand": "older_adult"
}
```
No name, no DOB, just age category. Minimal but useful.

---

#### `preferredLanguage`

**Type:** String (ISO 639-1 language code)  
**Required:** ❌ No (optional)

**What it is:**  
Primary language for communication.

**How to use it:**

**British English:**
```json
"preferredLanguage": "en-GB"
```

**Welsh:**
```json
"preferredLanguage": "cy-GB"
```

**Polish:**
```json
"preferredLanguage": "pl-PL"
```

**Urdu:**
```json
"preferredLanguage": "ur-PK"
```

**Format:** ISO 639-1 (two-letter code) optionally with region:
- `en` - English (generic)
- `en-GB` - British English
- `en-US` - American English
- `cy-GB` - Welsh
- `pl-PL` - Polish

**When to include:**
- ✅ When person prefers language other than default
- ✅ For accessibility (Equality Act 2010)
- ✅ To ensure communications are understood

**Legal significance:**  
Equality Act 2010 requires **reasonable adjustments** for language access.

---

#### `contact`

**Type:** Object  
**Required:** ❌ No (optional)

**What it is:**  
Contact details (phone, email).

**How to use it:**

**Phone and email:**
```json
"contact": {
  "phone": "+447700900123",
  "email": "emma.wilson@example.com"
}
```

**Phone only:**
```json
"contact": {
  "phone": "+447700900123"
}
```

**Email only:**
```json
"contact": {
  "email": "emma.wilson@example.com"
}
```

**Fields within contact:**

**`phone`** (string):  
Phone number. Prefer E.164 format: `+447700900123`

**`email`** (string):  
Email address. Must be valid email format.

**When to include:**
- ✅ When needed for emergency contact
- ✅ When needed for delivering EES Personal
- ❌ When not needed for evacuation planning (privacy!)

**Data minimisation:**  
Only include contact details if you'll **actually use them**.

**Best practice:**  
Ask permission before including contact details.

---

#### `communicationPreferences`

**Type:** Object  
**Required:** ❌ No (optional)

**What it is:**  
How this person prefers to be contacted and what formats they need.

**Why it exists:**  
Equality Act 2010 - reasonable adjustments for communication.

**How to use it:**

**Full example:**
```json
"communicationPreferences": {
  "channels": ["email", "sms", "relay_service"],
  "formatPreferences": ["large_print", "high_contrast", "bsl_interpreter_preferred"],
  "notes": "Prefers written communication. BSL interpreter required for face-to-face meetings. Large print minimum 16pt, high contrast (black on yellow).",
  "consentRef": "consent-comms-uuid-001"
}
```

**Fields within communicationPreferences:**

**`channels`** (array of enums):  
Preferred contact channels:
- `"email"`
- `"sms"`
- `"phone_call"`
- `"voice_message"`
- `"postal"`
- `"in_app"`
- `"relay_service"`

Example:
```json
"channels": ["email", "sms"]
```

**`formatPreferences`** (array of enums):  
Accessible content preferences:
- `"plain_language"`
- `"large_print"`
- `"high_contrast"`
- `"alt_text_required"`
- `"captioned_media"`
- `"audio_format"`
- `"written_only"`
- `"no_voice_calls"`
- `"bsl_interpreter_preferred"`

Example:
```json
"formatPreferences": ["large_print", "high_contrast"]
```

**`notes`** (string, max 500 chars):  
Brief clarification. Avoid health/impairment details.

Good:
```json
"notes": "Prefers email for routine matters; phone call for emergencies."
```

Bad:
```json
"notes": "Has hearing loss due to industrial injury" 
```
❌ Medical detail - wrong place!

**`consentRef`** (string):  
Link to consent record governing communications.

**When to include:**
- ✅ When person has communication access needs
- ✅ For accessible information (Equality Act 2010)
- ❌ Don't record WHY they need adjustments (medical details)

**Legal significance:**  
Equality Act 2010 - public authorities must make reasonable adjustments.

---

## Core vs Extensions Pattern

### Person Profile is Portable

Person Profile core fields work globally:
- ✅ `personRef`, `name`, `dob` - universal
- ✅ `contact` (phone, email) - universal
- ✅ `preferredLanguage` (ISO 639-1) - international standard
- ✅ `communicationPreferences` - accessibility is global

### When You Might Use Extensions

**Jurisdiction-specific identifiers:**

**UK (gbr):**
```json
"extensions": {
  "gbr": {
    "nhsNumber": "1234567890",
    "niNumber": "AB123456C"
  }
}
```

**USA (usa):**
```json
"extensions": {
  "usa": {
    "socialSecurityNumber": "123-45-6789",
    "state": "CA"
  }
}
```

**Vendor-specific fields:**
```json
"extensions": {
  "acmeHousing": {
    "tenantId": "TENANT-12345",
    "accountNumber": "ACC-67890"
  }
}
```

**But usually not needed!**  
Person Profile is already portable. Extensions mainly for internal IDs.

---

## Data Minimisation

### Principle: Collect Only What's Necessary

**Person Profile should be minimal.** Only include fields you **actually need** for evacuation planning.

### Decision Tree

**Do you need their name?**
- ✅ Yes, for human-readable PEEP → include `name`
- ❌ No, maximum privacy required → use `alias` or just `personRef`

**Do you need exact age?**
- ✅ Yes, legal requirement → include `dob`
- ❌ No, approximate is fine → use `ageBand`

**Do you need contact details?**
- ✅ Yes, for emergency contact or EES delivery → include `contact`
- ❌ No, not using them → omit `contact`

**Do they have communication needs?**
- ✅ Yes, need accessible formats → include `communicationPreferences`
- ❌ No special needs → omit `communicationPreferences`

### Privacy-First Example

**Maximum privacy (hotel guest, short-term):**
```json
{
  "personRef": "person-guest-305-uuid",
  "alias": "Guest, Room 305",
  "ageBand": "adult"
}
```

No name, no DOB, no contact. Just enough to identify and categorize.

**Standard privacy (long-term resident, consented):**
```json
{
  "personRef": "person-emma-wilson-uuid",
  "name": "Emma Wilson",
  "preferredLanguage": "en-GB",
  "contact": {
    "email": "emma.wilson@example.com"
  }
}
```

Name and email only. No DOB (use `ageBand` if needed).

**Full profile (accessibility needs, consented):**
```json
{
  "personRef": "person-rhys-davies-uuid",
  "name": "Rhys Davies",
  "dob": "1965-04-22",
  "preferredLanguage": "cy-GB",
  "contact": {
    "email": "rhys.davies@example.com"
  },
  "communicationPreferences": {
    "channels": ["email", "sms"],
    "formatPreferences": ["large_print", "high_contrast", "bsl_interpreter_preferred"],
    "notes": "Welsh language preferred. BSL interpreter for meetings. Large print 16pt+."
  }
}
```

Full detail because person has accessibility needs requiring this information.

---

## Complete Examples

### Example 1: Minimal (Privacy-First)

```json
{
  "personRef": "person-guest-room-305-uuid-002"
}
```

**Use case:** Hotel guest, temporary PEEP, maximum privacy.

---

### Example 2: Standard Residential

```json
{
  "personRef": "person-emma-wilson-uuid-001",
  "name": "Emma Wilson",
  "dob": "1992-11-08",
  "preferredLanguage": "en-GB",
  "contact": {
    "phone": "+447700900123",
    "email": "emma.wilson@example.com"
  }
}
```

**Use case:** Long-term resident, consented to sharing details.

---

### Example 3: Accessibility Focus

```json
{
  "personRef": "person-rhys-davies-uuid-003",
  "name": "Rhys Davies",
  "dob": "1965-04-22",
  "preferredLanguage": "cy-GB",
  "contact": {
    "email": "rhys.davies@example.com",
    "phone": "+447700900890"
  },
  "communicationPreferences": {
    "channels": [
      "email",
      "sms",
      "relay_service"
    ],
    "formatPreferences": [
      "large_print",
      "high_contrast",
      "bsl_interpreter_preferred",
      "captioned_media"
    ],
    "notes": "Preferred language: Welsh (Cymraeg). BSL interpreter required for face-to-face meetings. All written communications should be in large print (minimum 16pt) with high contrast (black text on yellow background). Avoid phone calls without prior arrangement via relay service.",
    "consentRef": "consent-rhys-davies-uuid-003"
  }
}
```

**Use case:** Person with sensory impairments, multiple accessibility needs.

---

## Validation

```bash
# Validate person profile
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d your-person-profile.json
```

### Common Errors

**Error:** `should have required property 'personRef'`  
**Fix:** Add `"personRef"` field (only required field!).

**Error:** `should match format "email"`  
**Fix:** Email must be valid format: `"user@example.com"`.

**Error:** `should match pattern "^[a-z]{2}(-[A-Z]{2})?$"`  
**Fix:** Language code must be ISO 639-1: `"en-GB"` not `"English"`.

---

## FAQs

### Q: What's the difference between `name` and `alias`?

**A:**
- **`name`:** Legal or chosen name (personal data)
- **`alias`:** Nickname or privacy-preserving identifier (less sensitive)

Use `alias` for maximum privacy (e.g., "Guest, Room 305").  
Use `name` when person has consented and it's needed.

### Q: Should I use `dob` or `ageBand`?

**A:** Prefer `ageBand` (better privacy).

Only use `dob` if you need **exact age** (rare).

### Q: Can I include NHS number or social security number?

**A:** Yes, but in **extensions**, not core:

```json
"extensions": {
  "gbr": {
    "nhsNumber": "1234567890"
  }
}
```

**Warning:** Very sensitive data. Only include if legally required.

### Q: What if person doesn't want to give their name?

**A:** Use `alias` or just `personRef`:

```json
{
  "personRef": "person-anon-uuid-001",
  "alias": "Resident, Flat 12A"
}
```

**Their right to privacy.** You don't need their name to create a PEEP.

### Q: How do I handle preferred pronouns?

**A:** Not in core schema (out of scope for evacuation planning).

If needed, use `extensions`:
```json
"extensions": {
  "pronouns": "they/them"
}
```

Or include in `name`:
```json
"name": "Alex Thompson (they/them)"
```

### Q: Can one person have multiple Person Profiles?

**A:** No. One person = one `personRef`.

But person can have multiple PEEPs (home, work, etc.) all linking to same `personRef`.

### Q: How long do I keep Person Profiles?

**A:** UK guidance:
- **Active:** While person is occupying/employed
- **After leaving:** 6 years (linked to PCFRA/PEEP retention)

**Delete when:** Retention period expires AND no active documents reference it.

---

## Next Steps

- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how Person Profile embeds in PEEPs
- **Read:** [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - how it links to assessments
- **Read:** [Privacy & GDPR Guide](../privacy_gdpr.md) - data minimisation principles

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

