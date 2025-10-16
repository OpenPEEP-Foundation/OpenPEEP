# Address Examples

Examples demonstrating the **Address** schema (`common/address.schema.json`).

---

## Purpose

Address is a **portable, international addressing schema** for OpenPEEP documents. It provides:
- Building and unit identification
- Postal address components
- Country-specific identifiers (UPRN, UDPRN for UK)
- OpenPEEP stable references (buildingRef, unitRef)

**Key principle:** No routing, wayfinding, or proprietary constructs. Location identity only.

---

## Examples

### 1. `address_uk_residential.json` — UK Residential (High-Rise Flat)

**Use case:** Typical UK high-rise residential address with UPRN.

**Features:**
- Building name, sub-building (flat number)
- UK postcode
- UPRN (Unique Property Reference Number)
- Building and unit references

**Demonstrates:** UK residential addressing with golden thread linkage (Building Safety Act 2022)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/address.schema.json \
  -d examples/address/address_uk_residential.json
```

---

### 2. `address_uk_workplace.json` — UK Workplace (Office Building)

**Use case:** UK office building address with organisation name.

**Features:**
- Organisation name
- Building name and number
- Postcode
- UPRN
- Building reference only (no unit)

**Demonstrates:** Workplace addressing

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/address.schema.json \
  -d examples/address/address_uk_workplace.json
```

---

### 3. `address_international.json` — International Address (Non-UK)

**Use case:** International address (USA example) demonstrating portability.

**Features:**
- Uses `countryCode: "US"` (ISO 3166-1 alpha-2)
- No UPRN (UK-specific)
- Uses `nationalAddressId` for country-specific identifier
- International postcode format (ZIP code)

**Demonstrates:** International portability (ISO 19160 alignment)

**Validation:**
```bash
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/address.schema.json \
  -d examples/address/address_international.json
```

---

## Schema Reference

**Schema:** `schemas/common/address.schema.json`  
**$id:** `https://open-peep.org/schemas/common/address.schema.json`

### Required Fields
- None (all fields optional for maximum portability)

### UK-Specific Fields
- `uprn` — UK Unique Property Reference Number
- `udprn` — Royal Mail Unique Delivery Point Reference Number
- `postcode` — UK postcode format (validated in `extensions.gbr` only)

### International Fields
- `countryCode` — ISO 3166-1 alpha-2 (default: `GB`)
- `nationalAddressId` — Generic slot for country-specific identifiers

---

## Portability Guidance

### UK Addressing
```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "townCity": "Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678"
}
```

### USA Addressing
```json
{
  "buildingNumber": "123",
  "addressLine1": "Main Street",
  "addressLine2": "Apartment 4B",
  "townCity": "New York",
  "county": "New York",
  "postcode": "10001",
  "countryCode": "US",
  "nationalAddressId": "usps-123456789"
}
```

### EU Addressing (France)
```json
{
  "buildingNumber": "12",
  "addressLine1": "Rue de la République",
  "addressLine2": "Appartement 3",
  "townCity": "Lyon",
  "postcode": "69001",
  "countryCode": "FR"
}
```

---

## Validation

```bash
# Validate all address examples
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/address.schema.json \
  -d "examples/address/*.json"
```

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

