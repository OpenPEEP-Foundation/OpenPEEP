# Consent Schema Guide — Complete Reference

**Schema:** `common/consent.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Core vs Extensions Pattern](#core-vs-extensions-pattern)
5. [Jurisdiction Profiles](#jurisdiction-profiles)
6. [Consent Lifecycle](#consent-lifecycle)
7. [Complete Examples](#complete-examples)
8. [Validation](#validation)
9. [FAQs](#faqs)

---

## Overview

### What is Consent?

A **Consent** record is a **standalone, legally auditable record** of whether a person has:
- ✅ **Given** consent for evacuation planning
- ❌ **Refused** consent
- 🔄 **Withdrawn** previously given consent

Think of it as a **legal receipt** that proves:
- You **offered** consent (with date/time)
- The person **decided** (given/refused/withdrawn)
- You **recorded** it properly (who, when, how)
- What the consent **covers** (PCFRA, PEEP, sharing with FRS, etc.)

### Why Does Consent Matter?

**Legal requirements:**
- 🇬🇧 **UK Residential Regs 2025 (Reg 4):** Must offer and record consent for RPEEPs
- 🇪🇺 **UK GDPR Article 6:** Consent is a lawful basis for processing personal data
- 🌍 **Equality Act 2010:** Can't force someone to disclose disability

**Practical reasons:**
- **Respect autonomy** - people have the right to refuse
- **Audit trail** - prove you offered consent
- **Legal protection** - evidence of compliance
- **Data rights** - basis for data processing

### Key Principle

Consent is **NOT an access control model**.  
It's a **record of decision**, not a permission system.

What it does:
- ✅ Records: "Did person agree to PEEP creation?"
- ✅ Records: "Can we share PEEP with FRS?"

What it doesn't do:
- ❌ Control who can view the PEEP
- ❌ Define user permissions
- ❌ Manage system access

---

## Quick Start

### Minimal Consent (Given)

```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep"]
}
```

**This records:**
- ✅ Consent was **given**
- ⏰ On **14 Oct 2025 at 10:00 UTC**
- 👤 By **housing.officer@example.org** (who recorded it)
- 📋 For **PCFRA and PEEP** creation

### Minimal Consent (Refused)

```json
{
  "state": "refused",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep"]
}
```

**This records:**
- ❌ Consent was **refused**
- ⏰ On **14 Oct 2025 at 10:00 UTC**
- 👤 By **housing.officer@example.org**
- 📋 For **PCFRA, PEEP, and RPEEP**

### Minimal Consent (Withdrawn)

```json
{
  "state": "withdrawn",
  "capturedAt": "2025-11-01T14:30:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["share_with_frs"]
}
```

**This records:**
- 🔄 Previously given consent was **withdrawn**
- ⏰ On **1 Nov 2025 at 14:30 UTC**
- 👤 By **housing.officer@example.org**
- 📋 For **sharing with Fire and Rescue Service**

---

## Complete Field Reference

### Required Fields

#### `state`

**Type:** String (enum)  
**Required:** ✅ Yes  
**Allowed values:** `"given"`, `"refused"`, `"withdrawn"`

**What it is:**  
The outcome of the consent decision.

**How to use it:**

**`"given"`** - Person consented:
```json
"state": "given"
```
Use when: Person says yes, signs form, ticks box.

**`"refused"`** - Person declined:
```json
"state": "refused"
```
Use when: Person says no, declines to participate.

**`"withdrawn"`** - Person revoked previous consent:
```json
"state": "withdrawn"
```
Use when: Person previously gave consent, now wants it cancelled.

**Common mistakes:**
- ❌ Using `"accepted"`, `"denied"`, `"revoked"` (not in enum)
- ❌ Using `null` or empty string
- ✅ Use exact strings: `"given"`, `"refused"`, `"withdrawn"`

**Legal significance:**
- **Given:** Proceed with PEEP/PCFRA
- **Refused:** Cannot proceed; must record offer and refusal
- **Withdrawn:** Must stop processing (unless other lawful basis exists)

---

#### `capturedAt`

**Type:** String (ISO 8601 date-time)  
**Required:** ✅ Yes  
**Example:** `"2025-10-14T10:30:00Z"`

**What it is:**  
The exact date and time the consent decision was recorded.

**Why it exists:**  
Legal auditability. Proves when consent was given/refused/withdrawn.

**How to use it:**  
Use ISO 8601 format with timezone:

**UTC (recommended):**
```json
"capturedAt": "2025-10-14T10:30:00Z"
```
The `Z` means UTC (Coordinated Universal Time).

**With timezone offset:**
```json
"capturedAt": "2025-10-14T10:30:00+01:00"
```
UK summer time (BST = UTC+1).

```json
"capturedAt": "2025-10-14T10:30:00-05:00"
```
US Eastern Standard Time (EST = UTC-5).

**How to generate:**
```javascript
// JavaScript
new Date().toISOString()
// "2025-10-14T10:30:00.123Z"

// Python
from datetime import datetime, timezone
datetime.now(timezone.utc).isoformat()
# "2025-10-14T10:30:00.123456+00:00"

// Command line (Mac/Linux)
date -u +"%Y-%m-%dT%H:%M:%SZ"
# "2025-10-14T10:30:00Z"
```

**Common mistakes:**
- ❌ Local time without timezone: `"2025-10-14T10:30:00"` (ambiguous!)
- ❌ Date only: `"2025-10-14"` (missing time)
- ❌ Non-ISO format: `"14/10/2025 10:30"` (wrong format)
- ✅ Always use ISO 8601 with timezone (prefer UTC)

**Legal significance:**  
If consent is disputed, this timestamp proves when decision was made.

---

#### `capturedBy`

**Type:** String  
**Required:** ✅ Yes  
**Example:** `"housing.officer@example.org"`

**What it is:**  
Identifier of the person or system that recorded the consent decision.

**Why it exists:**  
Accountability. Who recorded this? Who can verify it?

**How to use it:**  
Can be:
- **Email address:** `"housing.officer@example.org"`
- **User ID:** `"user-12345"`
- **Name + role:** `"Jordan Smith, Housing Officer"`
- **System:** `"automated-system@example.org"`

**Best practice:**  
Use a format you can trace back:
- ✅ Email address (can contact them)
- ✅ User ID (can look up in your system)
- ❌ Just a name (which "John Smith"?)

**Common mistakes:**
- ❌ Empty string or generic: `"admin"`, `"system"`
- ❌ Just first name: `"Jordan"`
- ✅ Specific, traceable identifier

**Legal significance:**  
If consent is audited, you need to know who recorded it.

---

#### `scope`

**Type:** Array of strings (enum)  
**Required:** ✅ Yes (minimum 1 item)  
**Allowed values:**
- `"pcfra"` - Person-Centred Fire Risk Assessment
- `"peep"` - Personal Emergency Evacuation Plan
- `"rpeep"` - Residential PEEP (UK Residential Regs 2025)
- `"ees_person_statement"` - EES Personal (plain-language instructions)
- `"share_with_frs"` - Share PEEP with Fire and Rescue Service

**What it is:**  
What does this consent cover? What are they agreeing/refusing?

**Why it exists:**  
Purpose limitation (UK GDPR Article 5(1)(b)). Consent must be **specific**.

Can't say: "Consent for everything"  
Must say: "Consent for PCFRA and PEEP"

**How to use it:**

**Single purpose:**
```json
"scope": ["pcfra"]
```
Consent only for PCFRA (assessment).

**Multiple purposes:**
```json
"scope": ["pcfra", "peep", "rpeep"]
```
Consent for assessment and plan.

**Full residential scope (UK RPEEP):**
```json
"scope": ["pcfra", "peep", "rpeep", "ees_person_statement", "share_with_frs"]
```
Consent for everything needed for RPEEP compliance.

**Partial consent (given for PEEP, refused for FRS sharing):**

First record (given):
```json
{
  "state": "given",
  "scope": ["pcfra", "peep", "rpeep"]
}
```

Second record (refused):
```json
{
  "state": "refused",
  "scope": ["share_with_frs"]
}
```

**Scope meanings:**

**`"pcfra"`:**  
Consent for Person-Centred Fire Risk Assessment.  
Needed to: assess functional capability, environmental risks.

**`"peep"`:**  
Consent for Personal Emergency Evacuation Plan.  
Needed to: create evacuation plan based on PCFRA.

**`"rpeep"`:**  
Consent specifically for Residential PEEP (UK Residential Regs 2025).  
Needed in: UK residential buildings from April 2026.

**`"ees_person_statement"`:**  
Consent for EES Personal (plain-language evacuation instructions).  
Needed to: provide resident-facing instructions (UK Reg 10).

**`"share_with_frs"`:**  
Consent to share PEEP with Fire and Rescue Service.  
Needed for: FRS access to plans (UK Reg 12).

**Common mistakes:**
- ❌ Empty array: `"scope": []` (must have at least 1)
- ❌ Misspelled: `"scope": ["peep_plan"]` (not in enum)
- ❌ Too broad: Can't consent to "everything" generically
- ✅ Specific, enumerated purposes

**Legal significance:**  
If you process data beyond the consented scope, you're in breach.

---

### Optional Fields

#### `method`

**Type:** String (enum)  
**Required:** ❌ No (optional)  
**Allowed values:** `"written"`, `"verbal"`, `"assisted"`

**What it is:**  
How was consent captured?

**How to use it:**

**`"written"`:**  
Person signed a form, ticked a box, submitted online form.

```json
"method": "written"
```

Best for: Audit trail, legal strength.

**`"verbal"`:**  
Person gave consent verbally (in person or phone).

```json
"method": "verbal"
```

Best for: Quick consent, phone assessments.  
Recommendation: Record who witnessed it in `notes`.

**`"assisted"`:**  
Person needed help to give consent (reasonable adjustments).

```json
"method": "assisted"
```

Best for: People needing communication support, interpreter, advocate.  
Recommendation: Document how assistance was provided in `notes`.

**When to include:**  
Always! Helps prove consent was properly obtained.

**Legal significance:**  
Written > Verbal in terms of evidence strength.  
Assisted shows you made reasonable adjustments (Equality Act 2010).

---

#### `reason`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Why did the person refuse or withdraw consent?

**When to use it:**  
Only for `state: "refused"` or `state: "withdrawn"`.

**How to use it:**

**Refused:**
```json
{
  "state": "refused",
  "reason": "Prefers to manage own evacuation; does not wish to participate in formal assessment."
}
```

**Withdrawn:**
```json
{
  "state": "withdrawn",
  "reason": "No longer wishes to share PEEP with Fire Service due to privacy concerns."
}
```

**Best practices:**
- ✅ Record what they said (if they gave a reason)
- ✅ Keep it brief and factual
- ❌ Don't interpret or judge
- ❌ Don't include sensitive personal data

**Examples:**

Good reasons:
- "Prefers to manage own evacuation"
- "Does not want information shared with FRS"
- "Changed mind after discussion with family"

Bad reasons:
- "Being difficult" (judgmental)
- "Has anxiety disorder" (medical diagnosis - wrong place)
- "Doesn't trust us" (editorializing)

**When to include:**  
If the person volunteers a reason. Don't press if they don't want to explain.

**Legal significance:**  
Shows you respected their decision and recorded it accurately.

---

#### `notes`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Additional context about the consent decision.

**When to use it:**  
For clarifications, circumstances, special considerations.

**Examples:**

**Verbal consent witness:**
```json
{
  "method": "verbal",
  "notes": "Consent given verbally during home visit. Housing officer and colleague both present as witnesses."
}
```

**Assisted consent:**
```json
{
  "method": "assisted",
  "notes": "BSL interpreter present. Consent form explained in British Sign Language. Resident signed form."
}
```

**Refusal context:**
```json
{
  "state": "refused",
  "notes": "Resident was polite but declined. Officer explained benefits of RPEEP; resident confirmed understanding but preferred not to participate. Offered to reconsider at any time. Will re-offer at annual tenancy review."
}
```

**Best practices:**
- ✅ Factual, objective
- ✅ Data minimisation (no unnecessary details)
- ❌ No medical/health data
- ❌ No opinions or judgments

**Legal significance:**  
Provides context if consent is challenged.

---

#### `evidenceRef`

**Type:** String (URI or reference)  
**Required:** ❌ No (optional)

**What it is:**  
Link to evidence of consent (signed form, recording, screenshot).

**How to use it:**

**PDF signature file:**
```json
"evidenceRef": "https://example.org/consent-forms/consent-alex-thompson-2025-10-14.pdf"
```

**Internal reference:**
```json
"evidenceRef": "consent-form-12345"
```

**Scanned document:**
```json
"evidenceRef": "urn:internal:scans:consent-2025-10-14-001"
```

**When to include:**  
If you have physical/digital evidence.

**What NOT to include:**  
Don't embed the actual file (too large, privacy risk). Link to it instead.

**Legal significance:**  
Strong evidence if consent is disputed.

---

#### `offerMadeAt`

**Type:** String (ISO 8601 date-time)  
**Required:** ❌ No (optional)

**What it is:**  
When was consent offered (before decision was made)?

**Why it exists:**  
UK Residential Regs 2025 require you to **offer** consent.  
Even if refused, you must prove you offered.

**How to use it:**

**Consent offered, then given:**
```json
{
  "offerMadeAt": "2025-10-14T09:00:00Z",
  "capturedAt": "2025-10-14T10:30:00Z",
  "state": "given"
}
```
Offered at 09:00, decided at 10:30.

**Consent offered, then refused:**
```json
{
  "offerMadeAt": "2025-10-14T09:00:00Z",
  "capturedAt": "2025-10-14T10:00:00Z",
  "state": "refused"
}
```
Offered at 09:00, refused at 10:00.

**When to include:**  
Always for UK RPEEP (proves you offered). Optional for other contexts.

**Legal significance:**  
Proves compliance with duty to offer consent (UK Reg 4).

---

#### `validFrom` / `validUntil`

**Type:** String (ISO 8601 date-time)  
**Required:** ❌ No (optional)

**What they are:**  
Time-bound consent. When does consent start/end?

**How to use them:**

**Consent valid for 1 year:**
```json
{
  "state": "given",
  "validFrom": "2025-10-14T10:00:00Z",
  "validUntil": "2026-10-14T10:00:00Z"
}
```

**Consent withdrawn (set end date):**
```json
{
  "state": "withdrawn",
  "validUntil": "2025-11-01T14:30:00Z"
}
```
Consent ended on 1 Nov 2025.

**When to use:**
- ✅ Temporary consent (e.g., short-term accommodation)
- ✅ When consent is withdrawn (set `validUntil` to withdrawal date)
- ❌ Not needed for most permanent consents

**Legal significance:**  
Shows consent expired or was withdrawn on a specific date.

---

#### `personRef`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Link to the Person Profile this consent relates to.

**How to use it:**
```json
"personRef": "person-alex-thompson-uuid-001"
```

**When to include:**  
If you want to link consent directly to a person record.

**Alternative:**  
PCFRA and PEEP can embed/reference consent. Not always needed in consent itself.

---

#### `documentRef`

**Type:** String  
**Required:** ❌ No (optional)

**What it is:**  
Link to the specific PCFRA/PEEP this consent is for.

**How to use it:**
```json
"documentRef": "peep-alex-thompson-uuid-001"
```

**When to include:**  
If consent is tied to a specific document (not just general consent).

---

## Core vs Extensions Pattern

### Consent Schema is Fully Portable

The Consent schema **doesn't need extensions** for most jurisdictions.

Why? Because consent concepts are universal:
- ✅ Given/refused/withdrawn - same everywhere
- ✅ Timestamp, method, scope - same everywhere
- ✅ Lawful basis for data processing - global concept

### When You Might Use Extensions

**Jurisdiction-specific consent requirements:**

**UK (gbr):**
```json
"extensions": {
  "gbr": {
    "residentialRegs2025Compliant": true,
    "informationDutyFulfilled": true
  }
}
```

**EU (eu):**
```json
"extensions": {
  "eu": {
    "gdprArticle6Basis": "consent",
    "gdprArticle9Basis": "explicit_consent"
  }
}
```

**USA (usa):**
```json
"extensions": {
  "usa": {
    "hipaaApplicable": false,
    "ccpaOptOutOffered": true
  }
}
```

### But Usually Not Needed

For 95% of use cases, core Consent schema is sufficient.

---

## Jurisdiction Profiles

### UK Profile

**UK Residential Regs 2025 Requirements:**

**Reg 4:** Must offer and record consent for RPEEP.

**Compliant consent record:**
```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep", "ees_person_statement", "share_with_frs"],
  "method": "written",
  "evidenceRef": "https://example.org/consent-form.pdf",
  "offerMadeAt": "2025-10-14T09:30:00Z",
  "personRef": "person-uuid-001"
}
```

**Key points:**
- ✅ Must offer (record `offerMadeAt`)
- ✅ Must record decision (even if refused)
- ✅ Scope includes `"rpeep"` and `"share_with_frs"` (Reg 12)
- ✅ Method documented (evidence trail)

**Refusal handling:**
```json
{
  "state": "refused",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep"],
  "method": "verbal",
  "reason": "Prefers to manage own evacuation",
  "offerMadeAt": "2025-10-14T09:30:00Z",
  "notes": "Resident politely declined. Explained benefits; confirmed understanding. Offered to reconsider any time. Will re-offer at annual review."
}
```

**Compliance:** Duty holder has offered consent and recorded refusal. ✅

---

### EU Profile

**GDPR Requirements:**

**Article 7:** Consent must be:
- Freely given
- Specific
- Informed
- Unambiguous

**Compliant consent:**
```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "data.processor@example.eu",
  "scope": ["pcfra", "peep"],
  "method": "written",
  "notes": "Consent form provided in resident's language (German). GDPR information notice provided. Resident confirmed understanding before signing.",
  "validFrom": "2025-10-14T10:00:00Z"
}
```

**Key GDPR points:**
- ✅ Specific scope (not blanket consent)
- ✅ Informed (information notice provided)
- ✅ Unambiguous (written, signed)
- ✅ Can be withdrawn (right under Article 7(3))

**Withdrawal:**
```json
{
  "state": "withdrawn",
  "capturedAt": "2025-11-01T14:00:00Z",
  "capturedBy": "data.processor@example.eu",
  "scope": ["share_with_frs"],
  "method": "written",
  "validUntil": "2025-11-01T14:00:00Z",
  "notes": "Resident exercised right to withdraw consent under GDPR Article 7(3). Processing ceased immediately."
}
```

---

### USA Profile

**Generally:** Consent less emphasized (no equivalent to GDPR consent requirements).

**ADA/OSHA:** Employer has legal duty (not consent-based).

**CCPA (California):** Right to opt out of data sharing.

**Example:**
```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "safety.officer@example.com",
  "scope": ["pcfra", "peep"],
  "method": "written",
  "notes": "Employee provided consent as part of workplace accommodation process (ADA). CCPA privacy notice provided; opt-out option offered."
}
```

---

## Consent Lifecycle

### Flow 1: Given → Still Active

```
1. Offer made (offerMadeAt)
2. Consent given (state: "given", capturedAt)
3. PCFRA created
4. PEEP created
5. EES delivered
6. Annual review → consent still valid
```

**Consent record:**
```json
{
  "state": "given",
  "offerMadeAt": "2025-01-15T09:00:00Z",
  "capturedAt": "2025-01-15T10:00:00Z",
  "scope": ["pcfra", "peep", "rpeep", "share_with_frs"],
  "validFrom": "2025-01-15T10:00:00Z"
}
```

### Flow 2: Refused → Re-offered → Given

```
1. Offer made (2025-01-15)
2. Consent refused (state: "refused")
3. [Time passes]
4. Re-offer made (2026-01-20, annual review)
5. Consent given (state: "given")
```

**First record (refused):**
```json
{
  "state": "refused",
  "offerMadeAt": "2025-01-15T09:00:00Z",
  "capturedAt": "2025-01-15T10:00:00Z",
  "scope": ["pcfra", "peep"],
  "reason": "Not ready to participate"
}
```

**Second record (given, later):**
```json
{
  "state": "given",
  "offerMadeAt": "2026-01-20T09:00:00Z",
  "capturedAt": "2026-01-20T10:30:00Z",
  "scope": ["pcfra", "peep", "rpeep"],
  "notes": "Resident reconsidered at annual review. Confirmed willing to participate."
}
```

### Flow 3: Given → Withdrawn (Partial)

```
1. Consent given for: PCFRA, PEEP, share with FRS
2. [Time passes]
3. Resident withdraws consent for: share with FRS (but keeps PEEP consent)
```

**Original consent:**
```json
{
  "state": "given",
  "capturedAt": "2025-01-15T10:00:00Z",
  "scope": ["pcfra", "peep", "share_with_frs"]
}
```

**Withdrawal (partial):**
```json
{
  "state": "withdrawn",
  "capturedAt": "2025-06-20T14:00:00Z",
  "scope": ["share_with_frs"],
  "validUntil": "2025-06-20T14:00:00Z",
  "reason": "Privacy concerns; no longer wishes to share with FRS"
}
```

**Result:** PCFRA and PEEP consent still valid. FRS sharing consent withdrawn.

---

## Complete Examples

### Example 1: UK Residential - Consent Given (Full Scope)

```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:30:00Z",
  "capturedBy": "housing.officer@manchesterhousing.example.org",
  "scope": [
    "pcfra",
    "peep",
    "rpeep",
    "ees_person_statement",
    "share_with_frs"
  ],
  "method": "written",
  "notes": "Resident signed consent form during home visit. RPEEP process explained in plain language. Resident confirmed understanding. Copy of consent form provided to resident.",
  "evidenceRef": "https://manchesterhousing.example.org/evidence/consent-alex-thompson-2025-10-14.pdf",
  "offerMadeAt": "2025-10-14T10:00:00Z",
  "validFrom": "2025-10-14T10:30:00Z",
  "personRef": "person-alex-thompson-uuid-001",
  "documentRef": "peep-alex-thompson-uuid-001"
}
```

---

### Example 2: UK Residential - Consent Refused

```json
{
  "state": "refused",
  "capturedAt": "2025-08-20T14:00:00Z",
  "capturedBy": "housing.officer@leedshousing.example.org",
  "scope": [
    "pcfra",
    "peep",
    "rpeep",
    "ees_person_statement"
  ],
  "method": "verbal",
  "reason": "Prefers to manage own evacuation arrangements; does not wish to participate in detailed assessment.",
  "notes": "Resident was polite but declined consent. Officer explained the purpose and benefits of RPEEP; resident confirmed they understood but preferred not to proceed. Officer advised that offer remains open and can be reconsidered at any time. Will re-offer consent at next annual tenancy review (August 2026).",
  "offerMadeAt": "2025-08-20T13:45:00Z",
  "personRef": "person-jordan-lee-uuid-002"
}
```

---

### Example 3: Consent Withdrawn (FRS Sharing Only)

```json
{
  "state": "withdrawn",
  "capturedAt": "2025-11-01T14:30:00Z",
  "capturedBy": "housing.officer@manchesterhousing.example.org",
  "scope": [
    "share_with_frs"
  ],
  "method": "verbal",
  "reason": "No longer wishes to share PEEP with Fire and Rescue Service due to privacy concerns.",
  "notes": "Resident called housing office to withdraw consent for FRS sharing. Consent for PCFRA and PEEP remains in place. PEEP will be retained but not shared with FRS. Resident confirmed understanding that FRS may request PEEP during emergency incident under vital interests lawful basis (GDPR Article 6(1)(d)). Withdrawal recorded; FRS notified of change.",
  "validUntil": "2025-11-01T14:30:00Z",
  "personRef": "person-alex-thompson-uuid-001",
  "documentRef": "peep-alex-thompson-uuid-001"
}
```

---

## Validation

```bash
# Validate consent record
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d your-consent.json
```

### Common Errors

**Error:** `should have required property 'state'`  
**Fix:** Add `"state": "given"` (or "refused" / "withdrawn").

**Error:** `should be equal to one of the allowed values`  
**Fix:** Use exact enum values: `"given"`, `"refused"`, `"withdrawn"`.

**Error:** `should NOT have fewer than 1 items`  
**Fix:** `scope` array must have at least one item.

---

## FAQs

### Q: Do I need separate consent for PCFRA and PEEP?

**A:** No, one consent can cover both. Use `scope`:
```json
"scope": ["pcfra", "peep"]
```

### Q: What if person consents to PCFRA but refuses PEEP?

**A:** Two separate records:

Consent for PCFRA:
```json
{
  "state": "given",
  "scope": ["pcfra"]
}
```

Refusal for PEEP:
```json
{
  "state": "refused",
  "scope": ["peep"]
}
```

### Q: How long do I keep consent records?

**A:** UK guidance:
- **Active consent:** For life of PCFRA/PEEP
- **Refused consent:** 6 years (evidence you offered)
- **Withdrawn consent:** 6 years after withdrawal

### Q: Can I assume consent if they don't respond?

**A:** ❌ **No!** Silence is not consent.

Must have explicit action:
- ✅ Signed form
- ✅ Verbal "yes" (recorded)
- ✅ Ticked box
- ❌ Didn't respond
- ❌ Didn't say no

### Q: Is workplace PEEP consent-based?

**A:** Usually no. Employer has **legal duty** (FSO 2005, not consent).

But good practice: offer consent anyway (respectful, good evidence).

### Q: What if family member gives consent on their behalf?

**A:** Depends on capacity:

- **Person has capacity:** Only they can consent
- **Person lacks capacity:** Authorized representative can consent (under relevant law - Mental Capacity Act 2005 in UK)

Record in `notes`:
```json
"notes": "Consent given by authorized representative (Power of Attorney) due to resident's lack of capacity. Legal authorization verified."
```

---

## Next Steps

- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how consent links to PEEPs
- **Read:** [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - how consent links to assessments
- **Read:** [Privacy & GDPR Guide](../privacy_gdpr.md) - legal basis for processing

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

