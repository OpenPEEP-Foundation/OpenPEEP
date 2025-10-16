# OpenPEEP Schema Validator & Viewer

A browser-based tool for validating and viewing OpenPEEP JSON documents in human-readable format.

---

## Features

✅ **Validates** JSON against OpenPEEP schemas (using AJV)  
✅ **Auto-detects** schema type from JSON structure  
✅ **Displays** human-readable formatted output  
✅ **Highlights** core fields vs jurisdiction extensions (gbr, eu, usa)  
✅ **Works offline** (client-side only, no server required)  
✅ **Privacy-preserving** (JSON never leaves your browser)  
✅ **Supports all 9 schema types:**
- PEEP (Personal Emergency Evacuation Plan)
- PCFRA Person (Personal capability assessment)
- PCFRA Environment (Environmental risk assessment)
- Consent (Consent records)
- Review (Lifecycle reviews)
- Person Profile (Identity anchor)
- Address (Location schema)
- EES Building (Building evacuation statement)
- EES Personal (Personal evacuation statement)

---

## How to Use

### Option 1: Open Locally

1. Open `index.html` in your web browser (Chrome, Firefox, Safari, Edge)
2. Drag & drop a JSON file **OR** paste JSON into the text area
3. Click **"Validate & View"**
4. View validation results and formatted output

### Option 2: GitHub Pages (Online)

Visit: `https://yourusername.github.io/openpeep-standard/tools/viewer/` *(update with actual URL)*

---

## Validation Workflow

### If JSON is Valid ✅

```
✅ VALIDATION PASSED

Schema: PEEP v1.0.0
✅ All required fields present
✅ All field types correct
✅ No validation errors

[Formatted output displayed below]
```

### If JSON is Invalid ❌

```
❌ VALIDATION FAILED

Schema: PEEP v1.0.0

3 errors found:

  ❌ Missing required field: "peepId"
     Path: /
     Required by schema
     Fix: Add "peepId" field with UUID v4 value

  ❌ Missing required field: "routes"
     Path: /
     Required by schema
     Fix: Add "routes" array with at least one route

  ❌ Routes must contain one named "Primary"
     Path: /routes
     Current routes: ["Secondary"]
     Fix: Add or rename a route to "Primary"
```

**Clear, actionable errors** - tells you exactly what's wrong and how to fix it.

---

## Schema Auto-Detection

Tool automatically detects schema type by looking for signature fields:

- `peepId` + `evacuationStrategy` → **PEEP**
- `state` + `scope` (array) → **Consent**
- `capability` object → **PCFRA Person**
- `environment` object → **PCFRA Environment**
- `reviewed_at` + `outcome` → **Review**
- `personRef` + `name` → **Person Profile**
- `evacuation_strategy` + `who_should_move_when` → **EES Building**
- `person_profile_ref` + `peep_document_id` → **EES Personal**
- Only address fields → **Address**

**Fallback:** Manual schema selection if auto-detection fails.

---

## Human-Readable Output

### Core Fields Section
Displays all core schema fields organized by logical sections.

### Extensions Section (Highlighted)
Displays jurisdiction extensions separately:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UK EXTENSIONS (gbr)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Relevant Resident:         ✅ Yes
Information Duty Evidence: [link]

EU EXTENSIONS (eu)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GDPR Lawful Basis:        Consent

VENDOR EXTENSIONS (acmeHousing)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Internal Tenant ID:       TENANT-12345
```

**This visualizes the core vs extensions pattern!**

---

## Privacy & Security

✅ **Client-side only** - all processing happens in your browser  
✅ **No data sent to server** - JSON never leaves your machine  
✅ **No tracking** - no analytics, no cookies  
✅ **Works offline** - after first page load (schemas cached)  
✅ **Safe for sensitive data** - validate real PEEPs without privacy concerns

---

## Technical Details

### Browser Compatibility

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

### Dependencies

- **AJV** (Another JSON Schema Validator) - loaded from CDN
- **AJV Formats** - for date/time/email/UUID validation

Both loaded from CDN (jsDelivr) - no npm install required.

### File Structure

```
tools/viewer/
├── README.md          ← This file
└── index.html         ← The viewer (self-contained)
```

**Single file!** Everything (HTML, CSS, JavaScript) in one file for portability.

---

## Use Cases

### For Developers
- **Validate** JSON during development
- **Debug** schema compliance issues
- **Visualize** structure (what fields map where)
- **Test** examples before committing

### For Stakeholders
- **Demonstrate** the standard (show working examples)
- **Review** sample PEEPs (human-readable format)
- **Understand** schema structure (see sections and fields)

### For Public Consultation
- **Share** examples with consultees
- **Explain** how JSON maps to readable documents
- **Show** core vs extensions pattern

### For Auditors
- **Verify** compliance (check all required fields present)
- **Review** PEEPs (readable format)
- **Trace** golden thread (see references)

---

## Limitations

❌ **Does not generate content** - only displays what's in JSON  
❌ **Does not edit JSON** - read-only viewer (not an editor)  
❌ **Does not create PEEPs** - validation and viewing only  
❌ **Does not simplify language** - shows fields as-is

These are **intentional** - tool is display-only, no hallucination.

---

## Future Enhancements

Potential additions:
- PDF export (using jsPDF)
- Side-by-side view (JSON + formatted)
- Diff viewer (compare two versions)
- Schema field explorer (click field to see documentation)
- Batch validation (validate multiple files)

---

## Questions or Issues?

- **GitHub Issues:** [Report a bug or request a feature](https://github.com/OpenPEEP-Foundation/OpenPEEP/issues) *(update URL)*
- **Email:** standards@openpeep.org *(update as needed)*

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

