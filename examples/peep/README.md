# PEEP Examples

Examples demonstrating the **PEEP (Personal Emergency Evacuation Plan)** schema (`peep.schema.json`).

---

## Purpose

PEEP is the **final evacuation plan** for an individual. It contains:
- Person Profile and Address
- Evacuation strategy and routes (minimum 1; must include "Primary")
- Step-by-step evacuation procedure
- Communications adjustments
- Assistance and equipment needs
- References to source PCFRA and building context
- Lifecycle management (consent, reviews)

**Key principle:** Comprehensive, actionable evacuation plan with full traceability.

---

## Examples

### 1. `peep_nominal.json` — Nominal Example

**Use case:** Standard residential PEEP with single primary route.

**Features:**
- Single primary route (stairwell)
- Moderate assistance (buddy system)
- Standard communications (hears alarm)
- Links to PCFRA and consent
- UK RPEEP extensions

---

### 2. `peep_minimal.json` — Minimal Example

**Use case:** Minimal viable PEEP (required fields only).

**Features:**
- Only required fields populated
- Single primary route (minimal description)
- Minimal procedure (3 steps)
- No optional fields (communications, reviews, etc.)

**Demonstrates:** Data minimisation; minimum compliance

---

### 3. `peep_complex.json` — Complex Multi-Route Example

**Use case:** Complex PEEP with multiple routes, refuge, evacuation lift.

**Features:**
- Primary route (evacuation lift)
- Secondary route (refuge + evacuation chair)
- Communications adjustments (visual alarms)
- Multiple assistance types
- Critical dependencies
- Detailed procedure (8 steps)
- Reviews and attachments

**Demonstrates:** Comprehensive PEEP for high support needs

---

## Validation

```bash
# Validate all PEEP examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/peep.schema.json \
  -d "examples/peep/*.json"
```

---

## Schema Requirements

### Required Fields
- `schemaVersion`, `peepId`
- `personProfile`, `address`
- `evacuationStrategy`
- `routes[]` (min 1; must include "Primary")
- `evacuationProcedure[]` (min 1)
- `planSummary`
- `metadata` (createdAt, createdBy, status)

### Conditional Requirements
- At least one route must be named "Primary" (enforced via `allOf`)

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

