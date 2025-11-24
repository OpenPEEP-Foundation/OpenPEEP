---
layout: default
title: Get Started with OpenPEEP
description: Choose your path to implementing OpenPEEP - from technical integration to understanding the complete framework. Step-by-step guide for developers and decision-makers.
keywords: PEEP implementation guide, fire safety compliance setup, digital evacuation planning, PEEP technical integration, fire regulations compliance guide
---

# Get Started with OpenPEEP

> **Choose your path based on your role and technical expertise.**

---

## Choose Your Starting Point

**I'm Technical - Take Me to the Code**  
→ [Jump to Technical Implementation](#technical-implementation)  
→ [View Schemas on GitHub](https://github.com/OpenPEEP-Foundation/OpenPEEP)  
→ [Read API Documentation](/documentation/)

**I Need to Understand the Process**  
→ [Continue Reading Below](#understanding-openpeep)  
→ [Learn the Complete Workflow](#the-complete-workflow)  
→ [Understand What Schemas Are](#what-are-schemas)

---

## Understanding OpenPEEP

OpenPEEP is **not software** — it is a **shared system of language and logic** that enables consistent, compliant evacuation planning across organisations and platforms.

Think of it as the **digital blueprint** that ensures everyone speaks the same language when creating, sharing, and managing Personal Emergency Evacuation Plans.

### Why OpenPEEP Exists

From April 2026, the Fire Safety (Residential Evacuation Plans) (England) Regulations 2025 mandate that responsible persons must create and digitally share PEEPs. 

The legislation tells us **what** must be done, but not **how** to do it consistently across thousands of organisations.

OpenPEEP fills that gap by providing:
- **Standardised structure** for evacuation plans
- **Clear data formats** for digital sharing
- **Compliance framework** aligned with UK regulations
- **Interoperability** between different systems and agencies

---

## The Complete Workflow

OpenPEEP defines the **entire lifecycle** of evacuation planning — from initial assessment through to digital sharing and ongoing review.

### 1. Person-Centred Fire Risk Assessment (PCFRA)

**What it is:** A comprehensive evaluation of both the person and their environment to identify evacuation needs and risks.

**What it covers:**
- Individual capabilities and support requirements
- Environmental factors (building layout, access routes, equipment)
- Risk identification and mitigation strategies
- Equality Act compliance for reasonable adjustments

**Why it matters:** The PCFRA forms the foundation for everything that follows. It is the "assessment" that feeds into the "plan."

#### PCFRA structure (how to read the data)
- Personal PCFRA: one `capability` object with functional fields (mobility, sensory, cognition, communication, awareness) and optional `capability.domainNotes`.
- Environmental PCFRA: flattened top‑level fields for environmental indicators:
  - `fireLoad`, `ignitionSources`, `detectionAlarm`, `escapeRoute`, `heatingRisk`, `oxygenRisk`, `petsRisk`
  - Safeguarding at top level: `asbViolenceRisk`, `environmentalSafeguarding`, `agencyInvolved`, `immediateDanger`, `immediateActionTaken`, `referralsMade`
  - Optional `domainNotes` for brief clarifiers (non‑sensitive)
- Both use annotations for grouping and analytics:
  - `x-openpeep-kind` (role) and `x-openpeep-domain` (domain) — these are metadata that do not change validation.

### 2. PEEP Creation and Management

**What it is:** The actual evacuation plan created from PCFRA data, structured in a standardised digital format.

**What it contains:**
- Individual evacuation procedures
- Support requirements and equipment needs
- Emergency contacts and coordination protocols
- Validation and compliance flags

**Why it matters:** This is the document that fire services, care providers, and emergency responders will use during an actual emergency.

### 3. Distribution and Sharing

**What it is:** The secure digital exchange of evacuation information between relevant parties.

**What it involves:**
- **Emergency Evacuation Statements (EES)** — resident-facing summaries
- **Digital sharing** with Fire & Rescue Services
- **Consent management** and data protection compliance
- **Multi-agency coordination** protocols

**Why it matters:** Information must reach the right people at the right time, in a format they can immediately understand and act upon.

### 4. Review and Maintenance

**What it is:** Ongoing management to ensure plans remain accurate, valid, and compliant.

**What it includes:**
- Regular plan updates and validation
- Compliance monitoring and reporting
- Continuous improvement through standardised review cycles
- Version control and audit trails

**Why it matters:** Evacuation plans are living documents that must evolve with changing circumstances, regulations, and best practices.

---

## What Are Schemas?

A **schema** is essentially a **digital template** that defines how information should be structured and organised.

### Think of it Like a Form

Imagine you're filling out a job application. The application form has:
- **Specific fields** (name, address, qualifications)
- **Required information** (some fields must be completed)
- **Format requirements** (dates in DD/MM/YYYY format)
- **Validation rules** (email addresses must be valid)

A schema works the same way, but for digital evacuation plans.

### Why Schemas Matter

**Without schemas:** Every organisation creates evacuation plans differently. Fire services receive inconsistent data. Information gets lost or misinterpreted.

**With OpenPEEP schemas:** Everyone uses the same structure. Information flows seamlessly between systems. Compliance is built-in and verifiable.

### OpenPEEP Schemas Cover:

- **PCFRA Schema** — How to structure fire risk assessments
- **PEEP Schema** — How to format evacuation plans
- **Emergency Evacuation Statement Schema** — How to create resident-facing statements
- **Consent Schema** — How to manage consent and data protection
- **Review Schema** — How to track updates and compliance

---

## The Consent and Review Process

### Why Consent Matters

Creating evacuation plans involves personal information — health conditions, mobility needs, support requirements. This data is sensitive and protected under GDPR and the Data Protection Act 2018.

**Consent is not optional** — it is a legal requirement that protects both the individual and the organisation.

### The OpenPEEP Consent Framework

**What it includes:**
- Clear consent capture and management
- Data minimisation (collecting only what's necessary)
- Purpose limitation (using data only for evacuation planning)
- Retention policies (how long data is kept)
- Individual rights (access, correction, deletion)

**Why it's built-in:** OpenPEEP schemas include consent fields and validation rules, ensuring compliance is designed-in rather than bolted-on.

### The Review Process

Evacuation plans must be reviewed regularly because:
- **Individual needs change** (health, mobility, support requirements)
- **Building conditions change** (refurbishments, new equipment, access modifications)
- **Regulations evolve** (new requirements, updated guidance)
- **Best practices improve** (lessons learned, new technologies)

**OpenPEEP provides:** Standardised review cycles, version control, and compliance tracking to ensure plans remain current and valid.

---

## Technical Implementation

### For Developers and Integrators

**Repository:** [github.com/OpenPEEP-Foundation/OpenPEEP](https://github.com/OpenPEEP-Foundation/OpenPEEP)

**What's Available:**
- **JSON Schemas** — Machine-readable validation rules
- **Example Data** — Sample PEEPs, PCFRAs, and Emergency Evacuation Statement documents
- **API Specifications** — How to exchange data between systems
- **Implementation Guides** — Step-by-step integration instructions

**Key Files:**
- `/schemas/` — JSON Schema definitions
- `/examples/` — Sample documents and data
- `/docs/` — Technical documentation
- `/tests/` — Validation and compliance testing

### Integration Options

**Option 1: Direct Integration**
- Implement OpenPEEP schemas in your existing system
- Validate data against OpenPEEP rules
- Export/import using OpenPEEP formats

**Option 2: API Integration**
- Connect to OpenPEEP-compliant services
- Exchange data using standardised APIs
- Leverage existing OpenPEEP implementations

**Option 3: Hybrid Approach**
- Use OpenPEEP for data validation
- Maintain your existing workflows
- Export compliant data when required

### Getting Technical Support

- **GitHub Issues** — Technical questions and bug reports
- **Documentation** — Complete technical reference
- **Community Forum** — Implementation discussions
- **Professional Support** — Commercial implementation assistance

---

## Next Steps

### For Non-Technical Users

1. **Understand the Framework** — Review the complete workflow above
2. **Identify Your Role** — Which part of the process are you responsible for?
3. **Plan Implementation** — How will you integrate OpenPEEP into your existing processes?
4. **Seek Support** — Contact the community or professional services

### For Technical Users

1. **Review Schemas** — Examine the JSON Schema definitions
2. **Study Examples** — Understand the data structures
3. **Build Prototypes** — Create test implementations
4. **Validate Compliance** — Ensure your implementation meets requirements

### For Everyone

- **Join the Community** — Stay informed about updates and best practices
- **Share Feedback** — Help improve OpenPEEP for everyone
- **Contribute** — Whether through code, documentation, or real-world testing

---

## Get Involved

- [**View Examples**](/examples/) — See schemas and templates in action
- [**Read Documentation**](/documentation/) — Complete technical reference
- [**Join the Community**](/support/) — Collaborate, contribute, and stay informed
- [**GitHub Repository**](https://github.com/OpenPEEP-Foundation/OpenPEEP) — Access schemas, examples, and code

---

## Understanding the Business Value

**Before diving into implementation, understand the compelling business case for OpenPEEP.**

Early adoption delivers significant ROI through compliance assurance, risk reduction, and operational efficiency. Most organizations see full payback within 12 months.

<div class="next-steps">
    <a href="/valuation/" class="btn btn-primary">See Business Value & ROI →</a>
    <a href="/examples/" class="btn btn-secondary">View Real Examples →</a>
</div>


