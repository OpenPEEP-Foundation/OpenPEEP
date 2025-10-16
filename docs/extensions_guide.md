# Extensions Guide — Making OpenPEEP Work for YOUR Organization

**Version:** 1.0.0  
**Last Updated:** 14 October 2025

---

## 🎯 The Critical Concept That Makes OpenPEEP Universal

This document explains **THE KEY PATTERN** that makes OpenPEEP:
- ✅ **Internationally portable** (works in UK, EU, USA, anywhere)
- ✅ **Organizationally customizable** (housing providers, care homes, workplaces can add their own fields)
- ✅ **Future-proof** (new jurisdictions can adopt without changing core)
- ✅ **Interoperable** (core data travels between systems; custom data stays with you)

**If you understand one thing about OpenPEEP, understand this:**

> **Core Schema = Universal**  
> **Extensions = Yours**

---

## Table of Contents

1. [The Problem Extensions Solve](#the-problem-extensions-solve)
2. [Core vs Extensions: The Pattern](#core-vs-extensions-the-pattern)
3. [Jurisdiction Extensions (UK, EU, USA)](#jurisdiction-extensions)
4. [Vendor Extensions (Your Organization)](#vendor-extensions)
5. [Step-by-Step: Creating Your Extensions](#step-by-step-creating-your-extensions)
6. [Real-World Examples](#real-world-examples)
7. [Governance: What Goes in Core?](#governance-what-goes-in-core)
8. [Best Practices](#best-practices)
9. [How Extensions Appear in Tools](#how-extensions-appear-in-tools)
10. [FAQs](#faqs)

---

## The Problem Extensions Solve

### The Challenge

**Scenario:** You're building a global fire safety standard.

**Problem 1: Different jurisdictions have different requirements**

**UK requires:**
- "Relevant resident" determination (Residential Regs 2025)
- Information duty evidence (Reg 10)
- FRS sharing consent (Reg 12)

**EU requires:**
- GDPR lawful basis documentation
- Data Protection Officer contact
- Accessibility Act compliance flags

**USA requires:**
- ADA compliance documentation
- NFPA code version
- OSHA applicability flags
- State-specific regulations

**If you put UK fields in core schema → EU/USA can't use it**  
**If you put EU fields in core schema → UK/USA can't use it**  
**If you put everything → schema becomes bloated and confusing**

---

**Problem 2: Organizations need internal operational data**

**Manchester Housing needs:**
- Tenant ID (their system reference)
- Property code (internal building codes)
- Assigned officer (who's responsible)
- Evacuation time estimates (their operational planning)

**TechCorp (workplace) needs:**
- Employee ID
- Department, floor, desk
- Line manager contact
- Building security protocols

**Sunnymeadows Care Home needs:**
- Resident care level
- Equipment serial numbers
- Night staff assignments
- Medication grab bag location

**If organizations can't add their fields → they won't adopt OpenPEEP**  
**If everyone adds fields differently → no interoperability**

---

### The Solution: Extensions

**Core schema** contains **only universal, portable fields**:
- Person identity (name, ref)
- Location (address)
- Evacuation capability (mobility, sensory, cognitive)
- Evacuation plan (routes, procedures)
- Assistance needs (who helps, what equipment)

**Extensions** contain **jurisdiction or organization-specific fields**:
- UK legal compliance → `extensions.gbr`
- EU legal compliance → `extensions.eu`
- Your internal data → `extensions.yourOrgName`

**Result:**
- ✅ Core schema works everywhere (portable)
- ✅ Each jurisdiction adds what it needs (compliant)
- ✅ Each organization adds what it needs (adoptable)
- ✅ Systems that don't understand extensions ignore them (interoperable)

**This is the "secret sauce" of OpenPEEP.** 🎯

---

## Core vs Extensions: The Pattern

### Anatomy of an OpenPEEP Document

```json
{
  // ═══════════════════════════════════════════════════════════
  // CORE SCHEMA (Portable, Universal)
  // Everyone uses these fields
  // ═══════════════════════════════════════════════════════════
  
  "schemaVersion": "1.0.0",
  "peepId": "peep-uuid-001",
  "personProfile": { ... },
  "address": { ... },
  "evacuationStrategy": "Simultaneous Evacuation",
  "routes": [ ... ],
  "evacuationProcedure": [ ... ],
  "planSummary": "...",
  "metadata": { ... },
  
  // ═══════════════════════════════════════════════════════════
  // EXTENSIONS (Optional, Jurisdiction/Organization-Specific)
  // Only relevant parties use these
  // ═══════════════════════════════════════════════════════════
  
  "extensions": {
    
    // UK-specific fields (for UK Residential Regs 2025 compliance)
    "gbr": {
      "relevantResident": true,
      "residentFacingStatementRef": "ees-personal-uuid-001",
      "informationDutyEvidenceRef": "https://example.org/evidence.pdf"
    },
    
    // Your organization's fields (your internal operational data)
    "yourOrganizationName": {
      "tenantId": "TENANT-12345",
      "propertyCode": "CH-12A",
      "assignedOfficer": "jordan.smith@example.org",
      "evacuationTimeEstimate": "6 minutes"
    }
  }
}
```

---

### Decision Tree: Core or Extension?

```
Is this field needed for evacuation planning EVERYWHERE?
    │
    ├─ YES → Core schema
    │   Examples:
    │   • evacuationStrategy (universal concept)
    │   • routes (everyone needs evacuation routes)
    │   • personProfile (everyone needs to identify who it's for)
    │
    └─ NO → Extension
        │
        ├─ Is it jurisdiction-specific?
        │   YES → Jurisdiction extension (gbr, eu, usa)
        │   Examples:
        │   • relevantResident (UK legal term)
        │   • gdprLawfulBasis (EU requirement)
        │   • nfpaCodeVersion (USA requirement)
        │
        └─ Is it organization-specific?
            YES → Vendor extension (yourOrgName)
            Examples:
            • tenantId (your internal ID)
            • assignedOfficer (your staff assignment)
            • evacuationTimeEstimate (your planning data)
```

---

## Jurisdiction Extensions

### Purpose

**Jurisdiction extensions** enable **legal compliance** in different countries/regions without modifying core schema.

### Namespace Convention

Use **ISO 3166-1 alpha-3 country codes:**
- `gbr` - United Kingdom (Great Britain)
- `usa` - United States of America
- `fra` - France
- `deu` - Germany
- `can` - Canada
- `aus` - Australia

Or regional groupings:
- `eu` - European Union
- `asia` - Asia (if creating pan-Asian profile)

---

### UK Extensions (`gbr`)

**Legal basis:** UK Residential Regs 2025, Building Safety Act 2022

**Example:**
```json
"extensions": {
  "gbr": {
    "relevantResident": true,
    "residentFacingStatementRef": "ees-personal-uuid-001",
    "informationDutyEvidenceRef": "https://evidence.example.org/info-duty.pdf",
    "stayingPutBoundary": "Stay put unless fire is in your flat or FRS instructs",
    "goldenThreadCompliant": true,
    "frsSharingConsent": "given",
    "annualReviewDue": "2026-09-15"
  }
}
```

**Field meanings:**

**`relevantResident`** (boolean):  
Is this person a "relevant resident" under Residential Regs 2025?  
Triggers RPEEP duty.

**`residentFacingStatementRef`** (string):  
Link to EES Personal (plain-language evacuation statement).  
Required under Reg 10 (information duty).

**`informationDutyEvidenceRef`** (string):  
Evidence you provided information to resident/FRS.  
Compliance proof for Regs 10 & 12.

**`stayingPutBoundary`** (string):  
Plain description of when "stay put" applies.  
Clarity for residents and FRS.

**When to use:**  
Implementing RPEEPs in England (Residential Regs 2025).

---

### EU Extensions (`eu`)

**Legal basis:** EU GDPR, EU Accessibility Act, member state regulations

**Example:**
```json
"extensions": {
  "eu": {
    "gdprLawfulBasis": "consent",
    "gdprArticle9Basis": "explicit_consent",
    "dataProtectionOfficerContact": "dpo@example.eu",
    "dataProtectionImpactAssessment": "DPIA-2025-001",
    "accessibilityActCompliant": true,
    "accessibilityDirective2019": true,
    "sevesoDirectiveApplicable": false,
    "memberState": "FR",
    "nationalRegulation": "French Fire Safety Code Article R.123-4"
  }
}
```

**Field meanings:**

**`gdprLawfulBasis`** (string):  
GDPR Article 6 basis: `"consent"`, `"legal_obligation"`, `"vital_interests"`, `"public_task"`.

**`dataProtectionOfficerContact`** (string):  
DPO contact email/phone (GDPR Articles 37-39).

**`accessibilityActCompliant`** (boolean):  
Complies with EU Accessibility Act (2019).

**`memberState`** (ISO 3166-1 alpha-2):  
Which EU country: `"FR"`, `"DE"`, `"ES"`, etc.

**`nationalRegulation`** (string):  
Country-specific fire safety regulation reference.

**When to use:**  
Implementing in EU member states.

---

### USA Extensions (`usa`)

**Legal basis:** ADA, OSHA, NFPA codes, state regulations

**Example:**
```json
"extensions": {
  "usa": {
    "adaCompliant": true,
    "adaAccommodationRef": "ADA-2025-456",
    "oshaApplicable": true,
    "oshaReportingRequired": false,
    "nfpaCodeVersion": "NFPA101-2021",
    "nfpaCodeSection": "Section 7.2 (Means of Egress)",
    "stateRegulation": "CA_Title24",
    "localJurisdiction": "San Francisco Building Code",
    "eeocNotificationRequired": false,
    "hipaaApplicable": false
  }
}
```

**Field meanings:**

**`adaCompliant`** (boolean):  
Complies with Americans with Disabilities Act.

**`nfpaCodeVersion`** (string):  
NFPA code edition: `"NFPA101-2021"`, `"NFPA1-2024"`, etc.

**`stateRegulation`** (string):  
State-specific regulation (e.g., California Title 24).

**`localJurisdiction`** (string):  
City/county building code.

**When to use:**  
Implementing in USA (federal, state, and local compliance).

---

### Other Jurisdictions

**Australia (`aus`):**
```json
"extensions": {
  "aus": {
    "disabilityDiscriminationActCompliant": true,
    "buildingCodeOfAustralia": "BCA-2022",
    "state": "NSW",
    "accessibleExitCompliant": true
  }
}
```

**Canada (`can`):**
```json
"extensions": {
  "can": {
    "pipedaCompliant": true,
    "ontarioFireCode": "OFC-2024",
    "province": "ON",
    "accessibilityOntarioCompliant": true
  }
}
```

**Create your own** following the pattern!

---

## Vendor Extensions (Your Organization)

### Purpose

**Vendor extensions** enable **operational customization** for your organization's internal processes.

### Namespace Convention

Use **your organization name** (camelCase):
- `manchesterHousing` - Manchester Housing Ltd
- `techCorp` - TechCorp UK Ltd
- `sunnymeadowsCare` - Sunnymeadows Care Home
- `nhsTrust` - NHS Trust name
- `universityHousing` - University student accommodation

**Make it unique and recognizable.**

---

### Example 1: Housing Provider

**Manchester Housing Ltd** operates 50 residential buildings. They need:
- Internal tenant IDs
- Property coding system
- Assigned officer tracking
- Evacuation timing estimates
- Equipment serial numbers
- Inspection dates
- Risk scoring

**Their extension:**

```json
"extensions": {
  "manchesterHousing": {
    // ─────────────────────────────────────────────────────────
    // Internal References
    // ─────────────────────────────────────────────────────────
    "tenantId": "TENANT-12345",
    "propertyCode": "CH-12A",
    "buildingCode": "CHURCHILL-001",
    "tenancyStartDate": "2022-03-15",
    "tenancyType": "social_rent",
    
    // ─────────────────────────────────────────────────────────
    // Staff & Responsibilities
    // ─────────────────────────────────────────────────────────
    "assignedHousingOfficer": {
      "name": "Jordan Smith",
      "email": "jordan.smith@manchesterhousing.example.org",
      "phone": "+44 161 234 5680",
      "staffId": "STAFF-789"
    },
    "fireOfficerAssigned": "fire.safety@manchesterhousing.example.org",
    
    // ─────────────────────────────────────────────────────────
    // Operational Planning
    // ─────────────────────────────────────────────────────────
    "evacuationTiming": {
      "flatToLift": "90 seconds",
      "liftToGroundFloor": "120 seconds",
      "totalPrimaryRoute": "4 minutes",
      "totalSecondaryRoute": "8 minutes"
    },
    "conciergeDetails": {
      "conciergeId": "CONC-456",
      "dayShift": "Pat Rodriguez",
      "nightShift": "Sam Williams",
      "conciergePhone": "+44 161 234 5678",
      "conciergeTrainingExpiry": "2026-03-01"
    },
    
    // ─────────────────────────────────────────────────────────
    // Equipment Tracking
    // ─────────────────────────────────────────────────────────
    "equipmentInventory": {
      "evacChair": {
        "serial": "EC-2024-789",
        "location": "Refuge cupboard, Floor 12 north stairwell",
        "lastMaintenance": "2025-08-01",
        "nextMaintenance": "2026-08-01",
        "trainingRequired": true
      },
      "evacuationLift": {
        "liftId": "LIFT-2-WEST",
        "bsEn81_76Compliant": true,
        "lastInspection": "2025-09-01",
        "monthlyTestDate": "First Monday of month"
      }
    },
    
    // ─────────────────────────────────────────────────────────
    // Risk & Compliance
    // ─────────────────────────────────────────────────────────
    "internalRiskScore": {
      "overall": 3,
      "personal": 3,
      "environmental": 1,
      "scale": "1-5 (1=low, 5=critical)",
      "calculatedDate": "2025-09-15"
    },
    "complianceChecks": {
      "reg3Compliant": true,
      "reg4ConsentRecorded": true,
      "reg5ReviewScheduled": true,
      "reg10InformationProvided": true,
      "reg12FrsShared": true
    },
    
    // ─────────────────────────────────────────────────────────
    // Process Management
    // ─────────────────────────────────────────────────────────
    "processFlags": {
      "peepApproved": true,
      "approvedBy": "senior.officer@manchesterhousing.example.org",
      "approvalDate": "2025-09-20",
      "residentBriefed": true,
      "conciergeBriefed": true,
      "frsNotified": true,
      "monthlyDrillRequired": true,
      "nextDrillDate": "2025-11-15"
    },
    
    // ─────────────────────────────────────────────────────────
    // Integration & Sync
    // ─────────────────────────────────────────────────────────
    "systemIntegration": {
      "crmReference": "CRM-12345",
      "facilityManagementRef": "FM-CH-12A",
      "tenancySystemRef": "TENANCY-67890",
      "lastSyncToFrs": "2025-09-21T10:00:00Z",
      "syncStatus": "synced"
    },
    
    // ─────────────────────────────────────────────────────────
    // Audit Trail
    // ─────────────────────────────────────────────────────────
    "auditTrail": {
      "createdInSystem": "DigiPEEP v2.4",
      "lastModifiedBy": "jordan.smith@manchesterhousing.example.org",
      "lastModifiedAt": "2025-09-15T14:30:00Z",
      "versionHistory": "v1.0 (2025-09-15)"
    }
  }
}
```

**Manchester Housing can now:**
- ✅ Use OpenPEEP (core schema)
- ✅ Add all their internal data (extensions)
- ✅ Track their processes (compliance flags, timing, staff)
- ✅ Still share with FRS (core data is portable)

---

### Example 2: Care Home

**Sunnymeadows Care Home** needs detailed care coordination:

```json
"extensions": {
  "sunnymeadowsCare": {
    // ─────────────────────────────────────────────────────────
    // Care Resident Information
    // ─────────────────────────────────────────────────────────
    "residentId": "RES-789",
    "roomNumber": "Room 12, West Wing",
    "admissionDate": "2023-05-10",
    "careLevel": "Level 3 (24-hour supervision)",
    "careTeamLead": "care.manager@sunnymeadows.example.org",
    
    // ─────────────────────────────────────────────────────────
    // Medical Equipment (Operational, Not Diagnostic)
    // ─────────────────────────────────────────────────────────
    "mobilityEquipment": {
      "wheelchairSerial": "WC-2023-456",
      "hoistSerial": "HT-2024-123",
      "lastServiceDate": "2025-09-01",
      "maintenanceProvider": "MedEquip Ltd"
    },
    "oxygenTherapy": {
      "concentratorSerial": "OC-2024-789",
      "portableCylinderLocation": "Bedside cupboard",
      "flowRate": "2L/min",
      "deliveryCompany": "BOC Healthcare"
    },
    
    // ─────────────────────────────────────────────────────────
    // Evacuation Protocol (Care-Specific)
    // ─────────────────────────────────────────────────────────
    "careEvacuationProtocol": {
      "assignedCarers": ["Carer-Smith-A", "Carer-Jones-B"],
      "backupCarers": ["Carer-Williams-C"],
      "evacuationMethod": "horizontal_evacuation_to_compartment_b",
      "mustEvacuateInWheelchair": true,
      "cannotTransferDuringEmergency": true,
      "medicationGrabBag": "Bedside drawer (insulin, rescue inhaler)",
      "specialInstructions": "Must bring oxygen cylinder. Cannot be left alone. Requires calm, reassuring approach."
    },
    
    // ─────────────────────────────────────────────────────────
    // Night-Time Arrangements
    // ─────────────────────────────────────────────────────────
    "nightArrangements": {
      "nightStaffOnFloor": 2,
      "nightStaffNames": ["Night-Carer-A", "Night-Carer-B"],
      "sleepLocation": "Bedroom (West Wing, Room 12)",
      "wakingProtocol": "Staff physically wake resident; loud verbal + tactile",
      "nightEvacuationChairLocation": "Floor 1 corridor cupboard"
    },
    
    // ─────────────────────────────────────────────────────────
    // Care Plan Integration
    // ─────────────────────────────────────────────────────────
    "carePlanRef": "CP-RES-789-2025",
    "riskAssessmentRef": "RA-EVAC-789-2025",
    "lastCareReview": "2025-09-01",
    "nextCareReview": "2026-03-01"
  }
}
```

**Care home can now:**
- ✅ Link PEEP to care plan
- ✅ Track equipment maintenance
- ✅ Assign specific carers
- ✅ Document grab bag contents
- ✅ Coordinate with care processes

**Still OpenPEEP compliant!**

---

### Example 3: Workplace (Tech Company)

**TechCorp UK Ltd** needs site-specific operational data:

```json
"extensions": {
  "techCorp": {
    // ─────────────────────────────────────────────────────────
    // Employee Information
    // ─────────────────────────────────────────────────────────
    "employeeId": "EMP-8901",
    "department": "Engineering",
    "team": "Platform Team",
    "lineManager": "manager@techcorp.example.com",
    "hrContact": "hr@techcorp.example.com",
    "startDate": "2024-07-01",
    "employmentType": "permanent_full_time",
    
    // ─────────────────────────────────────────────────────────
    // Location Within Building
    // ─────────────────────────────────────────────────────────
    "workLocation": {
      "building": "Innovation Centre",
      "floor": 8,
      "zone": "South Wing",
      "desk": "8-24",
      "nearestExit": "Stairwell B (15m south)",
      "nearestRefuge": "Floor 8 lift lobby"
    },
    
    // ─────────────────────────────────────────────────────────
    // Buddy System
    // ─────────────────────────────────────────────────────────
    "buddySystem": {
      "primaryBuddy": {
        "name": "Alex Chen",
        "desk": "8-25",
        "email": "alex.chen@techcorp.example.com",
        "trainingCompleted": "2025-07-12",
        "trainingExpiry": "2026-07-12"
      },
      "backupBuddy": {
        "name": "Jordan Smith",
        "desk": "8-30",
        "email": "jordan.smith@techcorp.example.com",
        "trainingCompleted": "2025-07-15"
      }
    },
    
    // ─────────────────────────────────────────────────────────
    // Reasonable Adjustments Implemented
    // ─────────────────────────────────────────────────────────
    "adjustmentsImplemented": {
      "visualAlarmBeacons": {
        "installed": true,
        "location": "Desk 8-24, Meeting Room 8-A, Meeting Room 8-B",
        "installationDate": "2025-07-18"
      },
      "tactileWayfinding": {
        "installed": true,
        "route": "Desk 8-24 to Stairwell B",
        "installationDate": "2025-07-18"
      },
      "bslInterpreter": {
        "available": true,
        "bookingContact": "accessibility@techcorp.example.com",
        "costCode": "ACC-BSL-2025"
      }
    },
    
    // ─────────────────────────────────────────────────────────
    // Site-Specific Protocols
    // ─────────────────────────────────────────────────────────
    "siteProtocols": {
      "buildingSecurityNotified": true,
      "canaryWharfControlRoom": "+44 20 7123 4567",
      "outOfHoursProtocol": "Security sweeps all floors; checks refuges",
      "frsPrePlan": "FRS pre-plan reference: CANARY-INN-CENTRE-2024"
    }
  }
}
```

---

### Example 4: Hospital/NHS

**Royal Victoria Hospital** needs clinical coordination:

```json
"extensions": {
  "royalVictoriaHospital": {
    "nhsNumber": "1234567890",  // Only if essential
    "patientId": "PAT-456789",
    "ward": "Ward 7B",
    "bedNumber": "Bed 12",
    "consultant": "Dr. Patel",
    "wardSister": "sister.ward7b@rvh.nhs.uk",
    
    "clinicalEvacuationCategory": "Category 2 (Requires assistance)",
    "evacuationPriority": "Medium",
    
    "medicalEquipment": {
      "oxygenTherapy": true,
      "ivPump": false,
      "ventilator": false,
      "portableEquipment": ["Oxygen cylinder", "Wheelchair"]
    },
    
    "staffingRequirement": {
      "nursesRequired": 2,
      "porterRequired": false,
      "paramedicEscort": false
    },
    
    "evacuationDestination": {
      "primaryDestination": "Ward 3A (adjacent compartment)",
      "secondaryDestination": "Ground floor day room",
      "externalEvacuation": "Ambulance bay, east side"
    },
    
    "clinicalNotes": "Requires oxygen during evacuation. Calm, reassuring approach. Family contact: next-of-kin on file."
  }
}
```

---

## Step-by-Step: Creating Your Extensions

### Step 1: Identify What You Need

**Ask yourself:**

**Q1:** What internal IDs/references do we use?
- Tenant IDs, employee IDs, patient IDs
- Property codes, building references
- Care plan references, case numbers

**Q2:** What operational data do we need?
- Staff assignments (who's responsible?)
- Equipment tracking (serial numbers, maintenance)
- Timing estimates (evacuation duration)
- Process flags (approval status, delivery confirmed)

**Q3:** What integrations do we have?
- CRM system
- Facility management system
- HR system
- Other fire safety systems

**Q4:** What compliance tracking do we do?
- Risk scores
- Inspection dates
- Training records
- Audit trails

**Write these down.**

---

### Step 2: Design Your Extension Structure

**Organize logically:**

```json
"extensions": {
  "yourOrgName": {
    // Group related fields
    "internalReferences": { ... },
    "staffAssignments": { ... },
    "equipmentTracking": { ... },
    "processManagement": { ... },
    "systemIntegration": { ... }
  }
}
```

**Or flat structure if simpler:**

```json
"extensions": {
  "yourOrgName": {
    "tenantId": "...",
    "assignedOfficer": "...",
    "riskScore": 3,
    "lastInspection": "..."
  }
}
```

**Your choice!** Structure it however makes sense for you.

---

### Step 3: Document Your Extensions

**Create a guide** for your organization:

```markdown
# [Your Organization] PEEP Extensions

## Purpose
We add these fields to OpenPEEP documents for internal operational management.

## Namespace
`yourOrgName`

## Fields

### `tenantId`
- **Type:** String
- **Format:** TENANT-XXXXX
- **Required:** Yes (internal)
- **Purpose:** Links PEEP to our tenancy management system

### `propertyCode`
- **Type:** String
- **Format:** BUILDING-UNIT
- **Example:** CH-12A
- **Purpose:** Internal property coding for maintenance system

[etc...]

## Integration
These fields sync with:
- CRM system (via API)
- Facility management database
- Fire safety tracking system
```

**Share with:**
- Your developers
- Your housing officers / assessors
- Your IT team

---

### Step 4: Add to Your PEEPs

**In your PEEP creation workflow:**

```javascript
// Create core PEEP (OpenPEEP standard)
const peep = {
  schemaVersion: "1.0.0",
  peepId: generateUUID(),
  personProfile: { ... },
  routes: [ ... ],
  // ... all core fields
};

// Add your extensions
peep.extensions = {
  yourOrgName: {
    tenantId: getTenantId(person),
    propertyCode: getPropertyCode(address),
    assignedOfficer: getCurrentOfficer(),
    riskScore: calculateRiskScore(pcfra),
    // ... all your custom fields
  }
};

// Optionally add UK extensions if in UK
if (jurisdiction === 'UK') {
  peep.extensions.gbr = {
    relevantResident: pcfra.outcome.relevantResident,
    residentFacingStatementRef: eesPersonalId
  };
}

// Save PEEP
savePeep(peep);
```

---

### Step 5: Test & Validate

**Your PEEP with extensions will:**
- ✅ Validate against OpenPEEP core schema (using npm run test:all)
- ✅ Display in OpenPEEP validator/viewer
- ✅ Show extensions in separate yellow section
- ✅ Still be compliant with OpenPEEP standard

**Extensions don't break validation!**  
Core schema allows `additionalProperties: true` in extensions object.

---

## Real-World Examples

### Case Study 1: Manchester Housing (50 Buildings, 2,000 Residents)

**Challenge:**  
Need to track PEEPs across 50 buildings, coordinate with 20 housing officers, integrate with existing tenancy system.

**Solution:**  
Use OpenPEEP core + vendor extensions:

```json
"extensions": {
  "manchesterHousing": {
    "tenantId": "TENANT-12345",
    "buildingCode": "CHURCHILL-001",
    "assignedOfficer": "jordan.smith@manchesterhousing.example.org",
    "crmReference": "CRM-12345",
    "evacuationTimeEstimate": "4 minutes",
    "conciergeTeam": "Team A (Churchill House)",
    "internalRiskScore": 3
  },
  "gbr": {
    "relevantResident": true,
    "residentFacingStatementRef": "ees-personal-uuid-001"
  }
}
```

**Result:**
- ✅ OpenPEEP compliant (can share with FRS, other housing providers)
- ✅ Has all internal data needed (tenant ID, officer assignment, CRM link)
- ✅ UK legally compliant (gbr extensions)

---

### Case Study 2: TechCorp (Multi-Site Employer, 5,000 Employees)

**Challenge:**  
Track PEEPs for employees across 3 office buildings, integrate with HR system, manage buddy assignments.

**Solution:**

```json
"extensions": {
  "techCorp": {
    "employeeId": "EMP-8901",
    "department": "Engineering",
    "building": "Innovation Centre",
    "floor": 8,
    "desk": "8-24",
    "lineManager": "manager@techcorp.example.com",
    "hrSystemRef": "HR-EMP-8901",
    "buddyAssignment": {
      "primaryBuddy": "alex.chen@techcorp.example.com",
      "backupBuddy": "jordan.smith@techcorp.example.com",
      "trainingCompleted": true
    },
    "adjustmentsCostCode": "ACC-2025-456",
    "nextReviewDue": "2026-01-15"
  }
}
```

**Result:**
- ✅ OpenPEEP compliant (portable if employee moves to different employer)
- ✅ Integrated with HR system
- ✅ Tracks adjustments and costs
- ✅ Manages buddy assignments

---

### Case Study 3: University (Student Accommodation, 3,000 Students)

**Challenge:**  
High turnover (students arrive/leave annually), term-time only occupancy, international students (language needs).

**Solution:**

```json
"extensions": {
  "universityOfManchester": {
    "studentId": "STUD-2024-5678",
    "academicYear": "2024-2025",
    "course": "Computer Science",
    "hallOfResidence": "Owens Park",
    "block": "Block B",
    "roomNumber": "B-12-305",
    "hallWarden": "warden.block-b@manchester.ac.uk",
    "buddySystem": {
      "buddyRoom": "B-12-306",
      "buddyName": "Student Jones"
    },
    "termDates": {
      "inResidenceFrom": "2024-09-15",
      "inResidenceTo": "2025-06-30",
      "christmasVacation": "2024-12-15 to 2025-01-10"
    },
    "internationalStudent": true,
    "homeCountry": "CN",
    "emergencyContact": "parent@example.com",
    "accessibilityServices": {
      "disabilitySupportOfficer": "disability@manchester.ac.uk",
      "adjustmentsApproved": true,
      "adjustmentsRef": "ADJ-2024-456"
    }
  }
}
```

---

## Governance: What Goes in Core?

### Decision Process

**Question:** Should this field be in **core** or **extension**?

**Core criteria (ALL must be true):**
1. ✅ **Universally applicable** (needed everywhere, not just one country/org)
2. ✅ **Essential for evacuation** (directly affects evacuation planning)
3. ✅ **Portable concept** (other jurisdictions understand it)
4. ✅ **Standardizable** (clear definition, enumerable values)

**If any criterion fails → Extension.**

---

### Examples: Core or Extension?

| Field | Core or Extension? | Why? |
|-------|-------------------|------|
| **routes** | ✅ Core | Universal concept; essential for evacuation; portable |
| **evacuationStrategy** | ✅ Core | Universal concept; every building has one |
| **mobilityAid** | ✅ Core | Universal concept; essential for planning; portable |
| **relevantResident** | ❌ Extension (gbr) | UK-specific legal term (Residential Regs 2025) |
| **gdprLawfulBasis** | ❌ Extension (eu) | EU-specific requirement (GDPR) |
| **nfpaCodeVersion** | ❌ Extension (usa) | USA-specific standard |
| **tenantId** | ❌ Extension (vendor) | Organization-specific ID |
| **assignedOfficer** | ❌ Extension (vendor) | Organization-specific process |
| **evacuationTimeEstimate** | ❌ Extension (vendor) | Organization-specific planning data |

---

### Proposing Core Additions

**What if many organizations add the same extension?**

**Example:** 10 housing providers all add `evacuationTimeEstimate` in their vendor extensions.

**Process:**
1. **Propose** adding to core schema (open GitHub issue with RFC label)
2. **Discuss** whether it's universally applicable
3. **14-day public comment** period
4. **Two maintainer approval** (one domain, one technical)
5. **If approved** → add to core schema in next minor version
6. **Migration note** → vendors can remove from extensions

**This allows the standard to evolve** based on real-world usage!

---

## Best Practices

### ✅ Do:

**1. Use descriptive namespace:**
```json
"extensions": {
  "manchesterHousing": { ... }  // ✅ Clear who owns this
}
```

**2. Structure logically:**
```json
"extensions": {
  "yourOrg": {
    "internalReferences": { ... },
    "operationalData": { ... },
    "integrationHooks": { ... }
  }
}
```

**3. Document your extensions:**
Create a guide explaining what each field means.

**4. Version your extensions:**
If you change structure, version it:
```json
"extensions": {
  "yourOrg": {
    "extensionVersion": "2.1.0",
    // ... your fields
  }
}
```

**5. Respect privacy:**
Only add fields you need; don't add sensitive data unnecessarily.

---

### ❌ Don't:

**1. Don't use reserved namespaces:**
```json
"extensions": {
  "gbr": { ... }  // ❌ Reserved for UK jurisdiction!
}
```
Unless you're defining UK jurisdiction profile.

**2. Don't duplicate core fields:**
```json
{
  "routes": [ ... ],  // Core
  "extensions": {
    "yourOrg": {
      "routes": [ ... ]  // ❌ Confusing! Use different name
    }
  }
}
```

**3. Don't put essential evacuation info in extensions:**
```json
"extensions": {
  "yourOrg": {
    "evacuationRoute": "Use stairs"  // ❌ This belongs in core routes!
  }
}
```

**4. Don't make extensions required:**
Extensions are always optional. Core schema must work without them.

---

## How Extensions Appear in Tools

### In the OpenPEEP Validator & Viewer

**Core fields** display in main sections (white background).

**Extensions** display separately (yellow background):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🌍 JURISDICTION & VENDOR EXTENSIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🇬🇧 UK EXTENSIONS (gbr)
──────────────────────────────────────────────────────────
Relevant Resident:        ✅ Yes
Information Duty Evidence: [link]

🏢 VENDOR EXTENSION (manchesterHousing)
──────────────────────────────────────────────────────────
Tenant ID:                TENANT-12345
Property Code:            CH-12A
Assigned Officer:         jordan.smith@example.org
Evacuation Time Estimate: 4 minutes
Risk Score:               3
```

**Visual separation** makes it clear:
- **White sections** = Core (portable, standard)
- **Yellow sections** = Extensions (jurisdiction/org-specific)

---

## FAQs

### Q: Can I add fields to core schema directly?

**A:** No, don't modify core schema files!

**Instead:** Add to extensions.

**If many orgs need it:** Propose adding to core via RFC process.

---

### Q: What if I need a field that's not in core?

**A:** Add it to your vendor extension! Example:

```json
"extensions": {
  "yourOrg": {
    "yourCustomField": "your value"
  }
}
```

---

### Q: Will my extensions break OpenPEEP compliance?

**A:** No! Extensions are **additive**.

Core schema + extensions = **still valid OpenPEEP document**.

---

### Q: What if another organization doesn't understand my extensions?

**A:** They ignore them! Example:

**You send:**
```json
{
  "peepId": "peep-001",
  "routes": [ ... ],
  "extensions": {
    "yourOrg": {
      "internalStuff": "..."
    }
  }
}
```

**They receive:**
```json
{
  "peepId": "peep-001",
  "routes": [ ... ]
  // extensions ignored (they don't know yourOrg namespace)
}
```

**Core data survives. Extensions are optional.**

---

### Q: Can I share PEEPs with extensions to FRS?

**A:** Yes! FRS will:
- ✅ Read core fields (what they need for rescue)
- ⚠️ Ignore your extensions (don't need your tenant ID)

**Perfect!** You keep operational data; FRS gets evacuation info.

---

### Q: Should I put equipment serial numbers in extensions?

**A:** Yes! Example:

```json
"extensions": {
  "yourOrg": {
    "equipmentInventory": {
      "evacChairSerial": "EC-2024-789",
      "lastMaintenance": "2025-08-01",
      "maintenanceProvider": "SafeEvac Ltd"
    }
  }
}
```

**Core schema just says:** "Evacuation chair required"  
**Your extensions add:** Which specific chair, maintenance tracking

---

### Q: Can I use extensions in PCFRA, Consent, etc.?

**A:** Yes! Extensions work in **all OpenPEEP schemas**:
- PEEP
- PCFRA (Person & Environment)
- Consent
- Review
- EES (Building & Personal)

**Address** has extensions field too (for postcode validation, etc.).

---

### Q: How do I version my extensions?

**A:** Add version field:

```json
"extensions": {
  "yourOrg": {
    "extensionVersion": "2.1.0",
    // ... your fields
  }
}
```

**If you change extension structure:**
1. Bump version
2. Document changes
3. Provide migration notes

---

## Summary

### **The Game-Changing Concept**

**Extensions make OpenPEEP universally adoptable because:**

1. **Core schema** stays portable (works everywhere)
2. **Jurisdiction extensions** enable legal compliance (UK, EU, USA, etc.)
3. **Vendor extensions** enable operational customization (your internal data)
4. **Systems interoperate** (core data travels; extensions are optional)

**This means:**
- ✅ Housing providers can add tenant IDs, property codes, risk scores
- ✅ Care homes can add resident IDs, care levels, equipment tracking
- ✅ Workplaces can add employee IDs, department, buddy assignments
- ✅ Hospitals can add patient IDs, ward, clinical protocols
- ✅ Universities can add student IDs, term dates, hall wardens

**Everyone uses the same core. Everyone customizes for their needs.**

**This is why OpenPEEP will succeed.** 🚀

---

## Next Steps

- **Read:** [PEEP Schema Guide](schemas/peep_schema_guide.md) - see extensions in context
- **Read:** [Data Dictionary](data_dictionary.md) - see which fields are core
- **Design:** Create your organization's extension structure
- **Document:** Write your internal extensions guide
- **Implement:** Add extensions to your PEEP creation workflow
- **Test:** Validate with OpenPEEP viewer (extensions show in yellow!)

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

