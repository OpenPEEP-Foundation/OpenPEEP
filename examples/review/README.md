# Review Examples

Examples demonstrating the **Review** schema (`common/review.schema.json`).

---

## Purpose

Review is a **reusable lifecycle block** for tracking reviews of OpenPEEP documents (PCFRA, PEEP, Emergency Evacuation Statements). It records:
- Review metadata (date, reviewer, outcome)
- Review summary and next due date
- References to superseding documents (if amended or superseded)

**Key principle:** Lifecycle tracking without duplicating consent logic.

---

## Examples

### 1. `review_confirmed.json` — Review: Confirmed, No Change

**Use case:** Annual PEEP review; no changes required.

**Features:**
- Outcome: `confirmed_no_change`
- Summary: brief explanation
- Next due date set (12 months)
- No new document created

**Demonstrates:** FSO 2005 (review duty); Residential Regs 2025 Reg 5 (annual review)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/review.schema.json \
  -d examples/review/review_confirmed.json
```

---

### 2. `review_amended.json` — Review: Amended

**Use case:** PEEP review triggers amendment (minor change).

**Features:**
- Outcome: `amended`
- New document reference (required for `amended` outcome)
- Summary explains what changed
- Next due date set

**Demonstrates:** Continuous improvement; responsive review cycles

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/review.schema.json \
  -d examples/review/review_amended.json
```

---

### 3. `review_superseded.json` — Review: Superseded

**Use case:** PEEP review triggers complete replacement (major change).

**Features:**
- Outcome: `superseded`
- New document reference (required for `superseded` outcome)
- Summary explains reason for replacement
- Attachments reference new PEEP

**Demonstrates:** Document supersession (e.g., change of address, significant capability change)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/review.schema.json \
  -d examples/review/review_superseded.json
```

---

## Schema Reference

**Schema:** `schemas/common/review.schema.json`  
**$id:** `https://open-peep.org/schemas/common/review.schema.json`

### Required Fields
- `reviewed_at` — ISO 8601 timestamp
- `reviewed_by` — Reviewer details (name, role, organisation)
- `outcome` — `confirmed_no_change`, `amended`, `superseded`, `withdrawn`

### Conditional Requirements
- If `outcome` is `amended` or `superseded`: **must** include `new_document_ref`

### Optional Fields
- `summary` — Review summary
- `next_due` — Next review due date
- `attachments` — Supporting documents

---

## Review Outcomes

| Outcome | Description | New Document Required? |
|---------|-------------|------------------------|
| `confirmed_no_change` | Review confirms document is still valid; no changes | ❌ No |
| `amended` | Minor changes made; document updated | ✅ Yes |
| `superseded` | Major changes; new document replaces old | ✅ Yes |
| `withdrawn` | Document no longer valid; not replaced | ❌ No |

---

## Review Frequency

### UK Residential Regs 2025 (Reg 5)
- **Annual review** or on **change of circumstance**
- Next due date: typically 12 months from creation or last review

### FSO 2005 (Article 22)
- **Regular review** based on risk profile
- Suggested frequency: annually for PEEPs; more frequently if high risk

### Workplace (FSO 2005)
- **Review on change of role, location, or capability**
- Suggested frequency: 6-12 months

---

## Validation

```bash
# Validate all review examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/review.schema.json \
  -d "examples/review/*.json"
```

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

