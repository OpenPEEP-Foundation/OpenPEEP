# EES (Emergency Evacuation Statement) Examples

Examples demonstrating **EES** schemas:
- **EES (Building)** — `ees_building.schema.json`
- **EES (Personal)** — `ees_personal.schema.json`

---

## Purpose

EES provides **plain-language evacuation guidance** for residents and buildings.

### EES (Building)
- Building-wide evacuation strategy (stay put, simultaneous, phased)
- High-level routes, refuges, lifts, muster points
- Plain-language guidance: "Who should move when?"
- Accessibility formats and distribution methods

### EES (Personal)
- Resident-facing instructions derived from PEEP and building strategy
- Plain-language, step-by-step guidance for **this person**
- Scenario-based instructions
- Assistance arrangements and delivery tracking

---

## Directory Structure

```
ees/
├── README.md              ← You are here
├── building/              ← EES (Building) examples
│   ├── ees_building_stay_put.json
│   ├── ees_building_simultaneous.json
│   └── ees_building_phased.json
└── personal/              ← EES (Personal) examples
    ├── ees_personal_standard.json
    ├── ees_personal_high_needs.json
    └── ees_personal_multilingual.json
```

---

## EES (Building) Examples

### 1. `building/ees_building_stay_put.json` — Stay Put Strategy

**Use case:** High-rise residential building with stay put strategy.

**Features:**
- Stay put evacuation strategy
- Plain-language guidance for residents
- Refuge and lift information
- Accessibility formats (large print, easy read, plain English)
- Distribution via noticeboard and tenant portal

**Demonstrates:** UK Residential Regs 2025 Reg 10 (information to residents)

---

### 2. `building/ees_building_simultaneous.json` — Simultaneous Evacuation

**Use case:** Low-rise residential building with simultaneous evacuation.

**Features:**
- Simultaneous evacuation strategy
- Two stairwells (no lift)
- Muster points
- Multiple languages (English, Polish, Urdu)
- Distribution via door drop and welcome pack

---

### 3. `building/ees_building_phased.json` — Phased Evacuation

**Use case:** Mixed-use building with phased evacuation strategy.

**Features:**
- Phased evacuation strategy
- Detailed "who should move when" guidance
- Evacuation lift available (BS EN 81-76)
- Multiple accessibility formats
- QR codes in common areas

---

## EES (Personal) Examples

### 1. `personal/ees_personal_standard.json` — Standard Personal EES

**Use case:** Standard resident-facing evacuation instructions.

**Features:**
- Plain-language summary
- Step-by-step primary instructions
- Scenario-based guidance (fire in your home, fire elsewhere)
- Assistance details (neighbour buddy, concierge)
- Delivery tracking

---

### 2. `personal/ees_personal_high_needs.json` — High Support Needs

**Use case:** EES Personal for resident with multiple support needs.

**Features:**
- Detailed assistance requirements (evacuation chair, 2 persons)
- Equipment list (evacuation chair, transfer board)
- Critical dependencies (oxygen therapy)
- Multiple scenarios with detailed instructions
- BSL interpreter and large print format

---

### 3. `personal/ees_personal_multilingual.json` — Multilingual Format

**Use case:** EES Personal in Welsh language with audio format.

**Features:**
- Welsh language (cy-GB)
- Audio format for accessibility
- Simplified instructions
- Cultural sensitivity notes
- Bilingual delivery (Welsh primary, English backup)

---

## Validation

```bash
# Validate EES (Building) examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/ees_building.schema.json \
  -d "examples/ees/building/*.json"

# Validate EES (Personal) examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/ees_personal.schema.json \
  -d "examples/ees/personal/*.json"
```

---

## Key Principles

### Plain Language
- ❌ "Egress via the north stairwell to the designated assembly area"
- ✅ "Leave the building using Stairwell A (north side); go to Assembly Point in the car park"

### Resident-Facing
- ❌ Technical fire safety terminology
- ✅ Simple, clear instructions anyone can follow

### Accessibility
- ✅ Multiple formats (large print, easy read, audio, BSL video, braille)
- ✅ Multiple languages (as needed by community)
- ✅ High contrast, plain English options

---

## Regulatory Mapping

| Regulation | Requirement | EES Support |
|------------|-------------|-------------|
| **Residential Regs 2025, Reg 10** | Information to residents | EES Personal delivered; delivery tracked |
| **Residential Regs 2025, Reg 12** | Information to FRS | Building EES available for FRS access |
| **Equality Act 2010** | Accessible information | Multiple formats and languages |
| **Building Safety Act 2022** | Golden thread | Related document IDs linked |

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

