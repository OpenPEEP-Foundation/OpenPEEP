# Review Schema Guide — Complete Reference

**Schema:** `common/review.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Review Outcomes Explained](#review-outcomes-explained)
5. [Review Frequency Guidance](#review-frequency-guidance)
6. [Complete Examples](#complete-examples)
7. [Validation](#validation)
8. [FAQs](#faqs)

---

## Overview

### What is a Review?

A **Review** is a **reusable lifecycle block** for tracking reviews of OpenPEEP documents.

**What it records:**
- ✅ When was the review conducted?
- ✅ Who conducted it?
- ✅ What was the outcome? (no change, amended, superseded, withdrawn)
- ✅ When is the next review due?
- ✅ If document changed, link to new version

**What it's used for:**
- PCFRA reviews (annual or on change)
- PEEP reviews (UK Residential Regs 2025 Reg 5)
- EES reviews (when building or person circumstances change)

**Think of it as:** An audit stamp showing "this was checked on this date."

### Why Reviews Matter

**UK Residential Regs 2025 Reg 5:**  
RPEEPs must be reviewed **at least annually** or on **change of circumstance**.

**FSO 2005 Article 22:**  
Fire safety arrangements must be kept under **regular review**.

**Building Safety Act 2022:**  
Golden thread requires **lifecycle tracking** (when was document last checked?).

**Practical reasons:**
- Person's circumstances change (mobility worsens, moves flat)
- Building changes (new evacuation lift installed, stairwell closed)
- Plan becomes outdated

### Review is Reusable

**Same schema** works for:
- PCFRA reviews (`pcfra.reviews[]`)
- PEEP reviews (`peep.reviews[]`)
- EES reviews (`ees_building.reviews[]`, `ees_personal.reviews[]`)

**Embedded as array:**
```json
"reviews": [
  {
    "reviewed_at": "2026-10-14T11:00:00Z",
    "reviewed_by": { "name": "Jordan Smith", "role": "Housing Officer" },
    "outcome": "confirmed_no_change",
    "next_due": "2027-10-14"
  }
]
```

---

## Quick Start

### Minimal Review (Confirmed, No Change)

```json
{
  "reviewed_at": "2026-10-14T11:00:00Z",
  "reviewed_by": {
    "name": "Jordan Smith"
  },
  "outcome": "confirmed_no_change"
}
```

**This records:**
- ✅ Review conducted on **14 Oct 2026**
- ✅ By **Jordan Smith**
- ✅ Outcome: **No changes needed**

### Review with Next Due Date

```json
{
  "reviewed_at": "2026-10-14T11:00:00Z",
  "reviewed_by": {
    "name": "Jordan Smith",
    "role": "Housing Officer"
  },
  "outcome": "confirmed_no_change",
  "summary": "Annual review conducted. No changes to resident's mobility or building context. PEEP remains valid.",
  "next_due": "2027-10-14"
}
```

### Review: Amended (Requires New Document)

```json
{
  "reviewed_at": "2026-06-15T10:00:00Z",
  "reviewed_by": {
    "name": "Emily Watson",
    "role": "Fire Safety Advisor"
  },
  "outcome": "amended",
  "summary": "Employee moved to Floor 5. PEEP updated with new routes.",
  "new_document_ref": {
    "document_type": "peep",
    "document_id": "peep-v2-uuid-new",
    "version": "2.0"
  },
  "next_due": "2027-06-15"
}
```

---

## Complete Field Reference

### Required Fields

#### `reviewed_at`

**Type:** String (ISO 8601 date-time)  
**Required:** ✅ Yes  
**Example:** `"2026-10-14T11:00:00Z"`

**What it is:**  
Timestamp when the review was conducted.

**How to use it:**  
Use ISO 8601 format with timezone (prefer UTC):

```json
"reviewed_at": "2026-10-14T11:00:00Z"
```

**Common mistakes:**
- ❌ Local time without timezone: `"2026-10-14T11:00:00"` (ambiguous!)
- ❌ Date only: `"2026-10-14"` (missing time)
- ✅ Use UTC with `Z` suffix

**Legal significance:**  
Proves when review was conducted (compliance evidence for UK Reg 5).

---

#### `reviewed_by`

**Type:** Object  
**Required:** ✅ Yes

**What it is:**  
Who conducted the review?

**Required fields within `reviewed_by`:**
- `name` ✅ (only required field!)

**Optional fields:**
- `role` ❌
- `organisation` ❌
- `contact` ❌ (phone, email)
- `reviewer_id` ❌

**Minimal:**
```json
"reviewed_by": {
  "name": "Jordan Smith"
}
```

**Standard:**
```json
"reviewed_by": {
  "name": "Jordan Smith",
  "role": "Housing Officer",
  "organisation": "Manchester Housing Ltd"
}
```

**Full:**
```json
"reviewed_by": {
  "name": "Jordan Smith",
  "role": "Housing Officer",
  "organisation": "Manchester Housing Ltd",
  "contact": {
    "email": "jordan.smith@manchesterhousing.example.org",
    "phone": "+441612345680"
  },
  "reviewer_id": "housing-officer-jordan-smith-001"
}
```

**When to include contact:**  
If someone might need to ask questions about the review.

**Best practice:**  
Include role and organisation (accountability).

---

#### `outcome`

**Type:** String (enum)  
**Required:** ✅ Yes  
**Allowed values:** `"confirmed_no_change"`, `"amended"`, `"superseded"`, `"withdrawn"`

**What it is:**  
What happened as a result of the review?

**How to use it:**

### `"confirmed_no_change"`

**Meaning:**  
Review confirms document is still valid. No changes needed.

**When to use:**
- Person's circumstances unchanged
- Building context unchanged
- Plan still accurate and appropriate

**Example:**
```json
{
  "outcome": "confirmed_no_change",
  "summary": "Annual review conducted. Resident's mobility unchanged. Evacuation lift operational. PEEP remains current."
}
```

**What happens:**  
- ✅ Document status stays `"approved"`
- ✅ Next review date set
- ❌ No new document created

---

### `"amended"`

**Meaning:**  
Minor changes made. Document updated.

**When to use:**
- Small change (new buddy, updated phone number)
- Same document, new version
- Document ID might stay same or change (your choice)

**Example:**
```json
{
  "outcome": "amended",
  "summary": "Resident's buddy changed from Alex Chen to Priya Sharma. PEEP updated.",
  "new_document_ref": {
    "document_type": "peep",
    "document_id": "peep-v1.1-uuid",
    "version": "1.1"
  }
}
```

**Required if outcome is "amended":**  
Must include `new_document_ref` (schema constraint).

**What happens:**
- ✅ New document created (v1.1)
- ✅ Old document status changes to `"superseded"` (optional)
- ✅ Review record links to new document

---

### `"superseded"`

**Meaning:**  
Major changes. New document completely replaces old.

**When to use:**
- Major change (moved to different building, significant capability change)
- Old plan no longer applicable
- Complete replacement needed

**Example:**
```json
{
  "outcome": "superseded",
  "summary": "Resident moved from Flat 5, Oak Tower to Flat 12, Birch Court. New PEEP created for new address with different evacuation strategy. Previous PEEP archived.",
  "new_document_ref": {
    "document_type": "peep",
    "document_id": "peep-new-building-uuid",
    "version": "2.0",
    "uri": "https://example.org/peeps/peep-new-building-uuid"
  }
}
```

**Required if outcome is "superseded":**  
Must include `new_document_ref` (schema constraint).

**What happens:**
- ✅ New document created (v2.0, new ID)
- ✅ Old document status changes to `"superseded"`
- ✅ Old document archived (retained for 6 years)

---

### `"withdrawn"`

**Meaning:**  
Document no longer valid. Not replaced.

**When to use:**
- Person left (no longer occupying/employed)
- Consent withdrawn and no other lawful basis
- Plan no longer needed

**Example:**
```json
{
  "outcome": "withdrawn",
  "summary": "Resident moved out of building. Tenancy ended. PEEP no longer applicable. Archived for regulatory retention period."
}
```

**What happens:**
- ✅ Document status changes to `"withdrawn"`
- ✅ No new document created
- ✅ Archived (retained for 6 years, then deleted)

---

### Optional Fields

#### `summary`

**Type:** String (max 2000 chars)  
**Required:** ❌ No (but highly recommended)

**What it is:**  
Brief description of the review outcome.

**Examples:**

**No change:**
```json
"summary": "Annual review conducted via phone call. No changes to resident's mobility or evacuation needs. Building evacuation lift tested monthly and operational. Concierge team unchanged. PEEP remains current and valid."
```

**Amended:**
```json
"summary": "6-month review conducted. Employee moved to Floor 5 (new role: Senior Engineer). Workstation changed; primary buddy changed from Alex Chen to Priya Sharma. PEEP amended to reflect new floor and buddy."
```

**Superseded:**
```json
"summary": "Resident has moved to a different property. Previous PEEP for Oak Tower no longer applicable. New PEEP created for Birch Court with updated building context (different evacuation strategy, different concierge team)."
```

**Withdrawn:**
```json
"summary": "Resident moved out. Tenancy ended 2026-03-31. PEEP withdrawn and archived."
```

**When to include:**  
Always! Provides context for auditors.

---

#### `next_due`

**Type:** String (ISO 8601 date: `YYYY-MM-DD`)  
**Required:** ❌ No (but recommended)

**What it is:**  
When is the next review due?

**How to use it:**

**Annual review (UK Residential Regs 2025):**
```json
"reviewed_at": "2026-10-14T11:00:00Z",
"next_due": "2027-10-14"
```
12 months from review date.

**6-month review (workplace):**
```json
"reviewed_at": "2026-01-15T10:00:00Z",
"next_due": "2026-07-15"
```

**When to include:**  
Always for `confirmed_no_change` (so you know when to review again).

Optional for `amended`/`superseded` (new document will have its own review cycle).

**Best practice:**  
Set next_due = reviewed_at + 12 months (UK annual requirement).

---

#### `new_document_ref`

**Type:** Object  
**Required:** ⚠️ **YES** if outcome is `"amended"` or `"superseded"` (schema enforces this!)

**What it is:**  
Link to the new/replacement document.

**How to use it:**

**Minimal:**
```json
"new_document_ref": {
  "document_type": "peep",
  "document_id": "peep-v2-uuid-new"
}
```

**Full:**
```json
"new_document_ref": {
  "document_type": "peep",
  "document_id": "peep-v2-uuid-new",
  "version": "2.0",
  "uri": "https://example.org/peeps/peep-v2-uuid-new"
}
```

**Fields:**

**`document_type`** (required):  
Type of document: `"pcfra"`, `"peep"`, `"rpeep"`, `"ees"`, `"other"`.

**`document_id`** (optional):  
ID of new document.

**`version`** (optional):  
Version number of new document.

**`uri`** (optional):  
URL where new document is stored.

**Schema constraint:**  
If `outcome` is `"amended"` or `"superseded"`, **you MUST include `new_document_ref`**.

Otherwise validation will fail!

---

#### `attachments`

**Type:** Array of attachment objects  
**Required:** ❌ No

**What it is:**  
Supporting documents (review checklist, evidence, photos).

**Example:**
```json
"attachments": [
  {
    "label": "Annual review checklist (completed)",
    "uri": "https://example.org/reviews/checklist-2026-10-14.pdf",
    "sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
  }
]
```

**When to include:**  
If you have review forms, evidence, or supporting documents.

---

## Review Outcomes Explained

### Flowchart: Which Outcome?

```
Was the review conducted?
    ↓ Yes
Did anything change?
    ↓ No
        → outcome: "confirmed_no_change"
    ↓ Yes
Is it a minor change?
    ↓ Yes
        → outcome: "amended" (+ new_document_ref required)
    ↓ No (major change)
        → outcome: "superseded" (+ new_document_ref required)

Special case: Is document no longer needed?
    ↓ Yes
        → outcome: "withdrawn" (no new_document_ref)
```

### Decision Guide

**Use `"confirmed_no_change"` when:**
- ✅ Person's circumstances unchanged
- ✅ Building context unchanged
- ✅ Plan is still accurate
- ✅ No updates needed

**Use `"amended"` when:**
- ✅ Minor change (buddy changed, phone number updated)
- ✅ Same basic plan, small tweak
- ✅ Document updated (v1.0 → v1.1)

**Use `"superseded"` when:**
- ✅ Major change (moved to different building, significant capability change)
- ✅ Old plan no longer applicable
- ✅ Complete replacement (v1.0 → v2.0)

**Use `"withdrawn"` when:**
- ✅ Person left (tenancy/employment ended)
- ✅ Consent withdrawn (no other lawful basis)
- ✅ Plan no longer needed (not replaced)

---

## Review Frequency Guidance

### UK Residential (RPEEP)

**Regulation:** Residential Regs 2025 Reg 5

**Frequency:**
- 📅 **At least annually**
- 🔄 **On change of circumstance**

**Change of circumstance examples:**
- Person's mobility worsens
- Person gets new equipment (wheelchair, oxygen)
- Building evacuation strategy changes
- Evacuation lift installed/removed
- Concierge team changes

**Best practice:**  
Set next_due = 12 months from creation or last review.

```json
"reviewed_at": "2026-10-14T11:00:00Z",
"next_due": "2027-10-14"
```

---

### UK Workplace

**Regulation:** FSO 2005 (no specific frequency stated)

**Recommended frequency:**
- 📅 **6-12 months** (depending on risk)
- 🔄 **On change of role, location, or capability**

**Triggers:**
- Employee changes role (different floor, different department)
- Employee's capability changes (injury, acquired disability)
- Building changes (refurbishment, new fire systems)

---

### Care Homes

**Recommended frequency:**
- 📅 **Quarterly or 6-monthly** (residents' circumstances change frequently)
- 🔄 **On any change in care needs**

**Triggers:**
- Mobility changes (now using wheelchair)
- Cognitive changes (dementia progression)
- Care package changes (new carers, different support)

---

## Complete Examples

### Example 1: Annual Review - Confirmed, No Change

```json
{
  "reviewed_at": "2026-09-15T11:00:00Z",
  "reviewed_by": {
    "name": "Jordan Smith",
    "role": "Housing Officer",
    "organisation": "Manchester Housing Ltd",
    "contact": {
      "email": "jordan.smith@manchesterhousing.example.org",
      "phone": "+441612345680"
    },
    "reviewer_id": "housing-officer-jordan-smith-001"
  },
  "outcome": "confirmed_no_change",
  "summary": "Annual review conducted via phone call with resident. No changes to mobility, evacuation needs, or building context. Evacuation lift (Lift 2) remains operational and tested monthly. Concierge team unchanged. PEEP remains current and valid. Resident confirmed understanding of evacuation procedure. No amendments required.",
  "next_due": "2027-09-15",
  "attachments": [
    {
      "label": "Annual review checklist (completed)",
      "uri": "https://manchesterhousing.example.org/reviews/peep-review-2026-09-15.pdf",
      "sha256": "b2c0d44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852c456"
    }
  ]
}
```

---

### Example 2: Review - Amended

```json
{
  "reviewed_at": "2026-01-15T10:00:00Z",
  "reviewed_by": {
    "name": "Emily Watson",
    "role": "Fire Safety Advisor",
    "organisation": "TechCorp UK Ltd",
    "contact": {
      "email": "emily.watson@techcorp.example.com",
      "phone": "+442071234567"
    },
    "reviewer_id": "fire-safety-emily-watson-001"
  },
  "outcome": "amended",
  "summary": "6-month review conducted. Employee has moved to Floor 5 (new role: Senior Engineer). Workstation changed from Desk 8-24 to Desk 5-12; primary buddy changed from Alex Chen to Priya Sharma (Desk 5-13). PEEP amended to reflect new floor location and new buddy assignment. Evacuation routes updated (now uses Stairwell C instead of Stairwell B). Previous PEEP version 1.0 superseded by version 1.1.",
  "next_due": "2026-07-15",
  "new_document_ref": {
    "document_type": "peep",
    "document_id": "peep-sam-patel-v1.1-uuid-004",
    "version": "1.1",
    "uri": "https://techcorp.example.com/peeps/peep-sam-patel-v1.1-uuid-004"
  },
  "attachments": [
    {
      "label": "Amended PEEP (v1.1)",
      "uri": "https://techcorp.example.com/peeps/peep-sam-patel-v1.1-uuid-004.pdf",
      "sha256": "c5d2e66498fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852e654"
    }
  ]
}
```

---

### Example 3: Review - Superseded

```json
{
  "reviewed_at": "2026-03-20T14:00:00Z",
  "reviewed_by": {
    "name": "Maria Garcia",
    "role": "Housing Officer",
    "organisation": "Leeds Housing Trust",
    "contact": {
      "email": "maria.garcia@leedshousing.example.org"
    },
    "reviewer_id": "housing-officer-maria-garcia-001"
  },
  "outcome": "superseded",
  "summary": "Resident has moved to a different property (from Flat 5, Oak Tower to Flat 12, Birch Court). Previous PEEP for Oak Tower no longer applicable. New PEEP created for Birch Court address with updated building context (different evacuation strategy, different concierge team, different routes). Previous PEEP (Oak Tower) superseded and archived. New PEEP (Birch Court) created with new ID and effective from 2026-03-25.",
  "next_due": "2027-03-20",
  "new_document_ref": {
    "document_type": "peep",
    "document_id": "peep-jordan-lee-birch-court-uuid-005",
    "version": "2.0",
    "uri": "https://leedshousing.example.org/peeps/peep-jordan-lee-birch-court-uuid-005"
  },
  "attachments": [
    {
      "label": "New PEEP (v2.0) for Birch Court",
      "uri": "https://leedshousing.example.org/peeps/peep-jordan-lee-birch-court-uuid-005.pdf",
      "sha256": "d6e3f77598fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852f765"
    },
    {
      "label": "Change of address notification",
      "uri": "https://leedshousing.example.org/admin/change-of-address-jordan-lee-2026-03-15.pdf",
      "sha256": "e7f4g88608fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852g876"
    }
  ]
}
```

---

### Example 4: Review - Withdrawn

```json
{
  "reviewed_at": "2026-04-30T16:00:00Z",
  "reviewed_by": {
    "name": "Sarah Johnson",
    "role": "Housing Officer"
  },
  "outcome": "withdrawn",
  "summary": "Resident moved out of building. Tenancy ended 2026-04-30. PEEP no longer applicable. Document withdrawn and archived for regulatory retention period (6 years). No replacement document created."
}
```

**Note:** No `new_document_ref` (not replacing the document).

---

## Validation

```bash
# Validate review record
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/review.schema.json \
  -d your-review.json
```

### Common Errors

**Error:** `should have required property 'new_document_ref'`  
**Fix:** If outcome is `"amended"` or `"superseded"`, you MUST include `new_document_ref`.

**Error:** `should have required property 'reviewed_by'`  
**Fix:** Add `reviewed_by` object with at least `name` field.

**Error:** `should be equal to one of the allowed values`  
**Fix:** Use exact outcome values: `"confirmed_no_change"`, `"amended"`, `"superseded"`, `"withdrawn"`.

---

## FAQs

### Q: How often should I review PEEPs?

**A:**
- **UK Residential:** Annually or on change (Reg 5)
- **UK Workplace:** 6-12 months or on change
- **Care homes:** Quarterly or on change

### Q: What triggers an unscheduled review?

**A:** Change of circumstance:
- Person's capability changes
- Person moves (different flat or building)
- Building changes (new lift, stairwell closed)
- Staff changes (new concierge, new buddy)
- Equipment changes (new evacuation chair)

### Q: Do I create a new Review record for each review?

**A:** Yes! Append to `reviews` array:

```json
"reviews": [
  {
    "reviewed_at": "2025-10-14T11:00:00Z",
    "outcome": "confirmed_no_change",
    "next_due": "2026-10-14"
  },
  {
    "reviewed_at": "2026-10-14T11:00:00Z",
    "outcome": "confirmed_no_change",
    "next_due": "2027-10-14"
  },
  {
    "reviewed_at": "2027-10-14T11:00:00Z",
    "outcome": "amended",
    "new_document_ref": { ... }
  }
]
```

**History preserved** in array.

### Q: What's the difference between "amended" and "superseded"?

**A:**

**Amended:**
- **Minor change**
- **Same document conceptually**, new version
- Example: Buddy changed, phone number updated

**Superseded:**
- **Major change**
- **New document completely replaces old**
- Example: Moved to different building, major capability change

**Rule of thumb:**  
If you're unsure, use `"superseded"` (safer for compliance).

### Q: Can I skip reviews if nothing changed?

**A:** ❌ No! Still need to **conduct review** and record outcome as `"confirmed_no_change"`.

**Why?** Proves you reviewed it (compliance evidence).

### Q: What if resident refuses to participate in review?

**A:** Record the refusal:

```json
{
  "reviewed_at": "2026-10-14T11:00:00Z",
  "reviewed_by": {
    "name": "Housing Officer"
  },
  "outcome": "confirmed_no_change",
  "summary": "Annual review attempted. Resident declined to participate (verbal refusal). Officer attempted phone contact on 2026-10-14 and 2026-10-20; no response. Letter sent requesting review participation. Absent direct contact, no changes have been reported by concierge team or neighbors. PEEP assumed to remain current pending resident engagement. Next review due 2027-10-14.",
  "next_due": "2027-10-14"
}
```

**You tried.** That's compliance.

---

## Next Steps

- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how Reviews embed in PEEPs
- **Read:** [PCFRA Person Schema Guide](pcfra_person_schema_guide.md) - review cycles for assessments
- **Implement:** Set up annual review reminders in your system

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

