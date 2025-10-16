# Consent Examples

Examples demonstrating the **Consent** schema (`common/consent.schema.json`).

---

## Purpose

Consent is a **minimal, legally auditable consent record** for evacuation planning. It records:
- Consent state (given, refused, withdrawn)
- Scope (what the consent applies to)
- Timestamp and method
- Provenance (who captured it, when, how)

**Key principle:** Standalone and reusable across PCFRA, PEEP, EES, and sharing with FRS.

---

## Examples

### 1. `consent_given.json` — Consent Given (Full Scope)

**Use case:** Resident gives full consent for PCFRA, PEEP, EES, and FRS sharing.

**Features:**
- State: `given`
- Scope: all available options (pcfra, peep, rpeep, ees_person_statement, share_with_frs)
- Method: `written`
- Evidence reference (PDF signature file)
- Valid from/until dates

**Demonstrates:** UK Residential Regs 2025 Reg 4 (consent given)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d examples/consent/consent_given.json
```

---

### 2. `consent_refused.json` — Consent Refused

**Use case:** Resident declines consent for PEEP creation.

**Features:**
- State: `refused`
- Scope: pcfra, peep, rpeep (refused)
- Method: `verbal`
- Reason provided by resident
- Offer date recorded

**Demonstrates:** UK Residential Regs 2025 Reg 4 (consent refusal handling)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d examples/consent/consent_refused.json
```

---

### 3. `consent_withdrawn.json` — Consent Withdrawn

**Use case:** Resident previously gave consent, now withdraws it.

**Features:**
- State: `withdrawn`
- Scope: share_with_frs (withdrawn; PEEP consent remains)
- Reason: privacy concerns
- Valid until: date consent withdrawn

**Demonstrates:** UK GDPR Article 7(3) (right to withdraw consent)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d examples/consent/consent_withdrawn.json
```

---

## Schema Reference

**Schema:** `schemas/common/consent.schema.json`  
**$id:** `https://open-peep.org/schemas/common/consent.schema.json`

### Required Fields
- `state` — `given`, `refused`, or `withdrawn`
- `capturedAt` — ISO 8601 timestamp
- `capturedBy` — Identifier of recorder
- `scope` — Array of consent purposes

### Optional Fields
- `method` — `written`, `verbal`, `assisted`
- `reason` — Reason for refusal/withdrawal
- `notes` — Contextual notes
- `evidenceRef` — Link to signature file or evidence
- `offerMadeAt` — When offer was made
- `validFrom`, `validUntil` — Time-bound consent
- `personRef`, `documentRef` — Cross-references

---

## Consent Scope Values

| Scope Value | Description |
|-------------|-------------|
| `pcfra` | Person-Centred Fire Risk Assessment |
| `peep` | Personal Emergency Evacuation Plan |
| `rpeep` | Residential PEEP (under Residential Regs 2025) |
| `ees_person_statement` | Emergency Evacuation Statement Personal (plain-language evacuation instructions) |
| `share_with_frs` | Share PEEP with Fire and Rescue Service |

---

## Consent Lifecycle

### Given → Withdrawn

```json
// Initially given
{
  "state": "given",
  "capturedAt": "2025-09-15T10:00:00Z",
  "scope": ["pcfra", "peep", "share_with_frs"]
}

// Later withdrawn (FRS sharing only)
{
  "state": "withdrawn",
  "capturedAt": "2025-11-01T14:30:00Z",
  "scope": ["share_with_frs"],
  "reason": "No longer wishes to share with FRS"
}
```

### Refused → Re-offered

```json
// Initially refused
{
  "state": "refused",
  "capturedAt": "2025-08-20T14:00:00Z",
  "scope": ["pcfra", "peep"],
  "reason": "Prefers to manage own evacuation"
}

// Later re-offered and given
{
  "state": "given",
  "capturedAt": "2026-08-25T10:00:00Z",
  "scope": ["pcfra", "peep"],
  "notes": "Resident reconsidered after annual tenancy review"
}
```

---

## Validation

```bash
# Validate all consent examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/consent.schema.json \
  -d "examples/consent/*.json"
```

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

