# Address Schema Guide — Complete Reference

**Schema:** `common/address.schema.json`  
**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Complete Field Reference](#complete-field-reference)
4. [Core vs Extensions Pattern](#core-vs-extensions-pattern)
5. [International Portability](#international-portability)
6. [Complete Examples](#complete-examples)
7. [Validation](#validation)
8. [FAQs](#faqs)

---

## Overview

### What is Address?

**Address** is a **portable, international addressing schema** for OpenPEEP documents.

**What it provides:**
- ✅ Building and unit identification
- ✅ Postal address components
- ✅ Country-specific identifiers (optional)
- ✅ OpenPEEP stable references

**What it does NOT include:**
- ❌ Routing information (wayfinding, GPS coordinates)
- ❌ Spatial geometry (floor plans, layouts)
- ❌ Proprietary building models

**Think of it as:** Postal address + building identifiers, nothing more.

### Why Does Address Matter?

PEEPs are **location-specific**:
- Same person needs **different PEEPs** for home vs workplace
- Address identifies **which building** the plan is for
- Enables **golden thread linkage** (Building Safety Act 2022)

### Design Principle: International Portability

Address schema is designed to work **globally**, not just UK:
- ✅ No mandatory postcode format (flexible for all countries)
- ✅ Flexible address lines (works for any addressing system)
- ✅ Country code (ISO 3166-1 alpha-2)
- ✅ Generic `nationalAddressId` slot (any country's ID system)

**UK-specific fields** (UPRN, UDPRN) are **optional**. Other countries use `nationalAddressId`.

---

## Quick Start

### Minimal Address

```json
{
  "countryCode": "GB"
}
```

**That's it!** Everything else is optional. But in practice, you need more to identify the location.

### Standard UK Address

```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "townCity": "Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB"
}
```

### UK Address with UPRN (Golden Thread)

```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "addressLine1": "45 High Street",
  "townCity": "Manchester",
  "county": "Greater Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001",
  "unitRef": "unit-churchill-12a-001"
}
```

### International Address (USA)

```json
{
  "buildingNumber": "350",
  "addressLine1": "5th Avenue",
  "addressLine2": "Suite 1200",
  "townCity": "New York",
  "county": "New York",
  "postcode": "10118",
  "countryCode": "US",
  "nationalAddressId": "usps-ny-10118-350-5th"
}
```

---

## Complete Field Reference

### All Fields Are Optional!

**Why?** International portability. Different countries have different addressing systems.

**But in practice:** Include enough to uniquely identify the location.

**UK minimum (residential):**
```json
{
  "subBuilding": "Flat 12A",
  "buildingName": "Churchill House",
  "postcode": "M1 1AA",
  "countryCode": "GB"
}
```

**UK best practice (with golden thread):**
```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "addressLine1": "45 High Street",
  "townCity": "Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001",
  "unitRef": "unit-churchill-12a-001"
}
```

---

### Building Identification Fields

#### `organisationName`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Organisation or landlord name.

**When to use:**
```json
"organisationName": "TechCorp UK Ltd"
```

Useful for: workplaces, care homes, hotels.

---

#### `buildingName`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Named building (e.g., "Churchill House").

**Examples:**
```json
"buildingName": "Churchill House"
```
```json
"buildingName": "Innovation Centre"
```
```json
"buildingName": "Royal Victoria Hospital"
```

**When to use:**  
When building has a name (most UK residential blocks do).

---

#### `buildingNumber`

**Type:** String (not integer!)  
**Required:** ❌ No

**What it is:**  
Street number or range.

**Why string, not number?**  
Addresses can be:
- `"25"` - simple number
- `"25-27"` - range
- `"25A"` - with letter suffix

**Examples:**
```json
"buildingNumber": "45"
```
```json
"buildingNumber": "12-14"
```
```json
"buildingNumber": "25A"
```

---

#### `subBuilding`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Flat, unit, room number, or apartment within a building.

**Why it exists:**  
UK Residential Regs 2025 require **unit-level precision** for RPEEPs.

**Examples:**
```json
"subBuilding": "Flat 12A"
```
```json
"subBuilding": "Apartment 4B"
```
```json
"subBuilding": "Room 305"
```
```json
"subBuilding": "Unit 7"
```

**When to use:**  
Always for flats/apartments. Critical for multi-unit buildings.

**Legal significance:**  
UK Residential Regs 2025 - RPEEP must be specific to each flat/unit.

---

### Postal Address Fields

#### `addressLine1`, `addressLine2`, `addressLine3`

**Type:** String  
**Required:** ❌ No

**What they are:**  
Flexible address lines for any addressing system.

**How to use them:**

**UK pattern:**
```json
"addressLine1": "45 High Street",
"addressLine2": "Didsbury",
"townCity": "Manchester"
```

**USA pattern:**
```json
"addressLine1": "350 5th Avenue",
"addressLine2": "Suite 1200",
"townCity": "New York"
```

**EU pattern (France):**
```json
"addressLine1": "12 Rue de la République",
"addressLine2": "Appartement 3",
"townCity": "Lyon"
```

**Best practice:**  
Use as many lines as needed for your country's addressing system.

---

#### `locality`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Locality, village, or district (smaller than town/city).

**Example:**
```json
"locality": "Didsbury"
```

**When to use:**  
UK addressing often has locality between address line and town/city.

---

#### `townCity`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Town or city name.

**Examples:**
```json
"townCity": "Manchester"
```
```json
"townCity": "London"
```
```json
"townCity": "New York"
```

**When to use:**  
Almost always (most addresses have a town/city).

---

#### `county`

**Type:** String  
**Required:** ❌ No

**What it is:**  
County or region.

**Examples:**

**UK:**
```json
"county": "Greater Manchester"
```

**USA:**
```json
"county": "New York"
```

**When to use:**  
UK often includes county. USA counties are important. EU less so.

---

#### `postcode`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Postal code (any format - not enforced in core).

**Examples:**

**UK:**
```json
"postcode": "M1 1AA"
```

**USA (ZIP code):**
```json
"postcode": "10118"
```

**USA (ZIP+4):**
```json
"postcode": "10118-0001"
```

**France:**
```json
"postcode": "69001"
```

**Germany:**
```json
"postcode": "10117"
```

**Important:** Core schema does **NOT enforce format**. Any string allowed.

**UK-specific validation:**  
If you want to enforce UK postcode format, use `extensions.gbr`:
```json
"postcode": "M1 1AA",
"extensions": {
  "gbr": {
    "postcode": "M1 1AA"
  }
}
```
The `extensions.gbr.postcode` field has pattern validation.

---

#### `countryCode`

**Type:** String (ISO 3166-1 alpha-2)  
**Required:** ❌ No  
**Default:** `"GB"`

**What it is:**  
Two-letter country code.

**Examples:**
```json
"countryCode": "GB"  // United Kingdom
```
```json
"countryCode": "US"  // United States
```
```json
"countryCode": "FR"  // France
```
```json
"countryCode": "DE"  // Germany
```
```json
"countryCode": "CA"  // Canada
```

**Full list:** [ISO 3166-1 alpha-2 codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

**When to use:**  
Always (or accept default `GB`).

**Why it matters:**  
Tells systems which addressing rules to apply.

---

### UK-Specific Fields

#### `uprn`

**Type:** String  
**Required:** ❌ No (but recommended for UK)

**What it is:**  
**UK Unique Property Reference Number** - a unique identifier for every addressable location in the UK.

**Example:**
```json
"uprn": "100012345678"
```

**Format:** 12 digits (string, not integer - may have leading zeros).

**Where to get it:**  
- Ordnance Survey database
- Local authority planning portal
- Royal Mail PAF (Postcode Address File)

**Why use it:**
- ✅ **Unique** (no two properties have same UPRN)
- ✅ **Stable** (doesn't change if address changes)
- ✅ **Golden thread** (Building Safety Act 2022 traceability)
- ✅ **Deduplication** (links records across systems)

**When to include:**  
Always for UK buildings, if you have it.

**Legal significance:**  
Building Safety Act 2022 - golden thread requires stable property identifiers.

---

#### `udprn`

**Type:** String  
**Required:** ❌ No

**What it is:**  
**Royal Mail Unique Delivery Point Reference Number**.

**Example:**
```json
"udprn": "12345678"
```

**Format:** 8 digits.

**Difference from UPRN:**
- **UPRN:** Every addressable location (OS/GeoPlace)
- **UDPRN:** Every postal delivery point (Royal Mail)

**When to use:**  
If you have it. Less commonly used than UPRN.

---

### International Fields

#### `nationalAddressId`

**Type:** String  
**Required:** ❌ No

**What it is:**  
Generic slot for **any country's** national address identifier.

**Why it exists:**  
International portability. Not every country has UPRN.

**Examples:**

**USA (USPS):**
```json
"countryCode": "US",
"nationalAddressId": "usps-ny-10118-350-5th"
```

**France (cadastre):**
```json
"countryCode": "FR",
"nationalAddressId": "cadastre-69001-0012"
```

**Germany (municipal ID):**
```json
"countryCode": "DE",
"nationalAddressId": "berlin-mitte-12345"
```

**When to use:**  
For non-UK addresses where a national ID exists.

---

### OpenPEEP Stable References

#### `buildingRef`

**Type:** String  
**Required:** ❌ No (but recommended)

**What it is:**  
**OpenPEEP-specific** stable reference for the building.

**Why it exists:**  
Creates a **stable identifier** that doesn't change even if:
- Building name changes
- Ownership changes
- UPRN is unavailable (international)

**How to use it:**
```json
"buildingRef": "building-churchill-house-001"
```

**Format:** Any unique string. Recommended pattern:
- Prefix: `building-`
- Building identifier: descriptive + unique suffix
- Example: `building-churchill-house-001`

**When to use:**  
Always, for cross-referencing within OpenPEEP documents.

**Links to:**
- PCFRA Environment (`buildingRef` field)
- EES Building (`building.identifiers`)

---

#### `unitRef`

**Type:** String  
**Required:** ❌ No (but recommended for multi-unit)

**What it is:**  
**OpenPEEP-specific** stable reference for a unit/flat/room.

**How to use it:**
```json
"unitRef": "unit-churchill-12a-001"
```

**Format:** Any unique string. Recommended pattern:
- Prefix: `unit-`
- Building + unit identifier
- Example: `unit-churchill-12a-001`

**When to use:**  
For flats, apartments, rooms in multi-unit buildings.

**Links to:**
- PCFRA Environment (`unitRef` field)
- PEEP (`address.unitRef`)

**Legal significance:**  
UK Residential Regs 2025 - RPEEPs must be unit-specific.

---

## Core vs Extensions Pattern

### Address Schema is Already Portable

Core Address fields work for **any country**:
- ✅ Flexible address lines (1-3 lines + locality + town + county)
- ✅ No format enforcement on postcode (any format accepted)
- ✅ Country code (ISO 3166-1)
- ✅ Generic `nationalAddressId` (any country)

### When to Use Extensions

**UK-specific postcode validation:**

If you want to **enforce UK postcode format**, use extensions:

```json
{
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "extensions": {
    "gbr": {
      "postcode": "M1 1AA"
    }
  }
}
```

The `extensions.gbr.postcode` field has pattern validation: `^[A-Za-z]{1,2}[0-9][0-9A-Za-z]? ?[0-9][A-Za-z]{2}$`

**But usually not needed!** Core schema is sufficient.

---

## International Portability

### UK Addressing

```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "addressLine1": "45 High Street",
  "locality": "Didsbury",
  "townCity": "Manchester",
  "county": "Greater Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001",
  "unitRef": "unit-churchill-12a-001"
}
```

---

### USA Addressing

```json
{
  "buildingNumber": "350",
  "addressLine1": "5th Avenue",
  "addressLine2": "Suite 1200",
  "townCity": "New York",
  "county": "New York",
  "postcode": "10118",
  "countryCode": "US",
  "nationalAddressId": "usps-ny-10118-350-5th",
  "buildingRef": "building-empire-state-001"
}
```

**USA notes:**
- Use `buildingNumber` for street number
- Use `addressLine2` for suite/apartment
- Use `county` (important in USA)
- Use `postcode` for ZIP code
- Use `nationalAddressId` for USPS or municipal ID

---

### EU Addressing (France)

```json
{
  "buildingNumber": "12",
  "addressLine1": "Rue de la République",
  "addressLine2": "Appartement 3",
  "townCity": "Lyon",
  "postcode": "69001",
  "countryCode": "FR",
  "nationalAddressId": "cadastre-69001-0012"
}
```

**France notes:**
- Street name in `addressLine1`
- Apartment in `addressLine2`
- 5-digit postcode
- Cadastre ID in `nationalAddressId`

---

### EU Addressing (Germany)

```json
{
  "addressLine1": "Unter den Linden 77",
  "townCity": "Berlin",
  "postcode": "10117",
  "countryCode": "DE",
  "nationalAddressId": "berlin-mitte-10117-77"
}
```

**Germany notes:**
- Street + number often in one line
- 5-digit postcode
- Municipality ID in `nationalAddressId`

---

### Asia Addressing (Japan)

```json
{
  "addressLine1": "1-2-3 Shibuya",
  "addressLine2": "Shibuya-ku",
  "townCity": "Tokyo",
  "postcode": "150-0002",
  "countryCode": "JP",
  "nationalAddressId": "jp-tokyo-shibuya-1-2-3"
}
```

**Japan notes:**
- Japanese addressing is block-based, not street-based
- Use flexible address lines to accommodate

---

## Complete Examples

### Example 1: UK Residential High-Rise

```json
{
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "addressLine1": "45 High Street",
  "townCity": "Manchester",
  "county": "Greater Manchester",
  "postcode": "M1 1AA",
  "countryCode": "GB",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001",
  "unitRef": "unit-churchill-12a-001"
}
```

---

### Example 2: UK Workplace

```json
{
  "organisationName": "TechCorp UK Ltd",
  "buildingName": "Innovation Centre",
  "buildingNumber": "25",
  "addressLine1": "Canary Wharf",
  "townCity": "London",
  "postcode": "E14 5AB",
  "countryCode": "GB",
  "uprn": "100034567890",
  "buildingRef": "building-innovation-centre-001"
}
```

**Note:** No `unitRef` (whole building, not individual office).

---

### Example 3: International (USA)

```json
{
  "buildingNumber": "350",
  "addressLine1": "5th Avenue",
  "addressLine2": "Suite 1200",
  "townCity": "New York",
  "county": "New York",
  "postcode": "10118",
  "countryCode": "US",
  "nationalAddressId": "usps-ny-10118-350-5th",
  "buildingRef": "building-empire-state-001"
}
```

---

## Validation

```bash
# Validate address
ajv validate --spec=draft2020 -c ajv-formats \
  -s schemas/common/address.schema.json \
  -d your-address.json
```

### Common Errors

**Error:** `should match format "email"` (if you put email in wrong place)  
**Fix:** Email goes in Person Profile, not Address!

**Error:** `should NOT have additional properties`  
**Fix:** Only use fields defined in schema. No custom fields (use extensions).

**No errors if all fields omitted:**  
All fields are optional! But provide enough to identify location.

---

## FAQs

### Q: Do I need to fill in every address field?

**A:** No! All fields are optional. Include enough to:
1. **Uniquely identify** the location
2. **Meet local requirements** (e.g., postcode for postal delivery)
3. **Enable golden thread** (e.g., UPRN for UK)

### Q: What's the difference between `buildingRef` and `uprn`?

**A:**
- **UPRN:** Official UK government ID (from Ordnance Survey)
- **buildingRef:** Your internal OpenPEEP stable ID

**Use both:**
- `uprn` for golden thread compliance
- `buildingRef` for internal cross-referencing

### Q: How do I handle addresses in countries without postcodes?

**A:** Omit `postcode` field:

```json
{
  "addressLine1": "Main Street, Village Center",
  "townCity": "Small Village",
  "countryCode": "XX"
}
```

### Q: Can I use GPS coordinates?

**A:** Not in core schema (out of scope - no routing/geometry).

If needed, use `extensions`:
```json
"extensions": {
  "geoLocation": {
    "latitude": 51.5074,
    "longitude": -0.1278
  }
}
```

**But not recommended** - OpenPEEP is for postal identity, not navigation.

### Q: What if building changes name?

**A:** Update `buildingName`, but keep `buildingRef` and `uprn` stable:

**Before:**
```json
{
  "buildingName": "Churchill House",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001"
}
```

**After (building renamed):**
```json
{
  "buildingName": "Victoria Apartments",
  "uprn": "100012345678",
  "buildingRef": "building-churchill-house-001"
}
```

UPRN and buildingRef stay the same (stable identifiers).

### Q: Do I need separate addresses for PCFRA and PEEP?

**A:** No, use the same Address object. Can embed or reference.

**Embedded in PEEP:**
```json
"address": {
  "buildingName": "Churchill House",
  "subBuilding": "Flat 12A",
  "postcode": "M1 1AA",
  "countryCode": "GB"
}
```

**Referenced in PCFRA:**
```json
"buildingRef": "building-churchill-house-001",
"unitRef": "unit-churchill-12a-001"
```

---

## Next Steps

- **Read:** [PEEP Schema Guide](peep_schema_guide.md) - how Address embeds in PEEPs
- **Read:** [PCFRA Environment Schema Guide](pcfra_environment_schema_guide.md) - how Address links to environment assessments
- **Implement:** Build your addressing system with international portability

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

