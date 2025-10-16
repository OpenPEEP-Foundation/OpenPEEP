# Person Profile Examples

Examples demonstrating the **Person Profile** schema (`common/person_profile.schema.json`).

---

## Purpose

Person Profile is a **minimal identity anchor** for individuals associated with OpenPEEP documents. It contains:
- Identity information (name, reference, age)
- Preferred language
- Contact details
- Communication preferences

**Key principle:** No impairments, evacuation needs, or medical data. Identity only.

---

## Examples

### 1. `person_profile_nominal.json` — Nominal Example

**Use case:** Standard person profile with full contact details and communication preferences.

**Features:**
- Full name and DOB
- Contact phone and email
- Communication preferences (channels and formats)
- Consent reference for communications

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d examples/person_profile/person_profile_nominal.json
```

---

### 2. `person_profile_minimal.json` — Minimal Example (Data Minimisation)

**Use case:** Minimal person profile using age band instead of DOB; alias instead of full name.

**Features:**
- Uses `alias` instead of `name` (privacy-sensitive context, e.g., hotel guest)
- Uses `ageBand` instead of `dob` (data minimisation)
- Minimal contact (phone only)
- No communication preferences

**Demonstrates:** Data minimisation principle (UK GDPR Article 5(1)(c))

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d examples/person_profile/person_profile_minimal.json
```

---

### 3. `person_profile_accessibility.json` — Accessibility Focus

**Use case:** Person profile emphasising communication accessibility preferences.

**Features:**
- Preferred language (Welsh)
- Communication channels (email, SMS, relay service)
- Format preferences (large print, high contrast, BSL interpreter)
- Detailed accessibility notes

**Demonstrates:** Equality Act 2010 (reasonable adjustments for communication)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d examples/person_profile/person_profile_accessibility.json
```

---

## Schema Reference

**Schema:** `schemas/common/person_profile.schema.json`  
**$id:** `https://open-peep.org/schemas/common/person_profile.schema.json`

### Required Fields
- `personRef` (unique identifier)

### Optional Fields
- `name`, `alias`, `dob`, `ageBand`, `preferredLanguage`
- `contact` (phone, email)
- `communicationPreferences` (channels, formats, notes, consentRef)

---

## Data Minimisation Guidance

- ✅ **Use `ageBand` instead of `dob`** where exact age is not required
- ✅ **Use `alias` instead of `name`** in transient contexts (hotels, short-term stays)
- ✅ **Omit contact details** if not required for evacuation planning
- ✅ **Capture communication preferences** only if accessibility adjustments needed

---

## Validation

```bash
# Validate all person profile examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/person_profile.schema.json \
  -d "examples/person_profile/*.json"
```

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

