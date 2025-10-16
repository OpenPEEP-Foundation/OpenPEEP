# PCFRA Examples

Examples demonstrating **Person-Centred Fire Risk Assessment** schemas:
- **PCFRA (Personal)** — `pcfra.person.schema.json`
- **PCFRA (Environmental)** — `pcfra.environment.schema.json`

---

## Purpose

PCFRA captures **functional evacuation capability** and **environmental risks** without storing medical diagnoses.

### PCFRA (Personal)
- Functional capabilities (mobility, sensory, cognitive, communication)
- Assistance needs and equipment dependencies
- Observable behavioural risks (non-diagnostic)
- Decision outputs (relevantResident, peepRequired)

### PCFRA (Environmental)
- Environmental/setting risks (hoarding, electrical, cooking, smoking)
- Access/egress issues
- Safeguarding context (ASB, agency involvement)
- Decision outputs

---

## Directory Structure

```
pcfra/
├── README.md              ← You are here
├── person/                ← PCFRA (Personal) examples
│   ├── pcfra_person_nominal.json
│   ├── pcfra_person_high_needs.json
│   └── pcfra_person_low_confidence.json
└── environment/           ← PCFRA (Environmental) examples
    ├── pcfra_environment_low_risk.json
    ├── pcfra_environment_high_risk.json
    └── pcfra_environment_safeguarding.json
```

---

## PCFRA (Personal) Examples

### 1. `person/pcfra_person_nominal.json` — Nominal Example

**Use case:** Standard in-person assessment with moderate mobility limitations.

**Features:**
- In-person assessment (high confidence)
- Moderate capabilities (uses walking stick, stairs with assist)
- Provenance tracking (self-report + observation)
- Outcome: PEEP required

---

### 2. `person/pcfra_person_high_needs.json` — High Support Needs

**Use case:** Comprehensive assessment for individual with multiple support needs.

**Features:**
- Multiple capability limitations (wheelchair, sensory, cognitive)
- Equipment dependencies (oxygen therapy)
- Assistance required (2 persons)
- Detailed provenance with verification

---

### 3. `person/pcfra_person_low_confidence.json` — Low Confidence Assessment

**Use case:** Phone assessment with limited information.

**Features:**
- Phone assessment method (low confidence)
- Minimal capability data (self-report only)
- No verification
- Outcome: PEEP required but confidence low

---

## PCFRA (Environmental) Examples

### 1. `environment/pcfra_environment_low_risk.json` — Low Environmental Risk

**Use case:** Tidy flat with no significant fire risks.

**Features:**
- No environmental hazards
- Clear egress
- No safeguarding concerns
- Outcome: PEEP required (due to personal factors, not environmental)

---

### 2. `environment/pcfra_environment_high_risk.json` — High Environmental Risk

**Use case:** Dwelling with multiple fire risks (hoarding, electrical hazards).

**Features:**
- Multiple environmental risks (hoarding, overloaded sockets, smoking)
- Egress partially blocked
- Immediate action taken (isolated circuit)
- Outcome: PEEP required + urgent risk mitigation

---

### 3. `environment/pcfra_environment_safeguarding.json` — Safeguarding Context

**Use case:** Complex case with safeguarding and multi-agency involvement.

**Features:**
- ASB/violence risk
- Multiple agencies involved (police, adult social care)
- Immediate danger recorded
- Referrals made
- Outcome: PEEP required + safeguarding escalation

---

## Validation

```bash
# Validate PCFRA (Person) examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.person.schema.json \
  -d "examples/pcfra/person/*.json"

# Validate PCFRA (Environment) examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/pcfra.environment.schema.json \
  -d "examples/pcfra/environment/*.json"
```

---

## Key Principles

### No Medical Diagnoses
- ❌ "Has Parkinson's disease"
- ✅ "Mobility transfer: stand_pivot_with_support"

### Functional Focus
- ❌ "Diabetic, on insulin"
- ✅ "Equipment dependency: none" (unless insulin pump affects evacuation)

### Observable Behaviours
- ❌ "Has dementia"
- ✅ "Cognitive ability: significant_support_needed; behaviour: confusion_in_emergency"

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

