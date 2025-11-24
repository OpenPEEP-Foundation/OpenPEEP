# Privacy & GDPR — Data Minimisation, Retention, Roles, Rights

**Version:** 1.0.0-beta  
**Last updated:** 14 October 2025

---

## Purpose

This document provides **privacy-by-design guidance** for OpenPEEP implementations. It addresses:

- **Data minimisation** principles
- **Lawful basis** for processing
- **Retention** periods and archival
- **Data controller and processor roles**
- **Data subject rights** (access, rectification, erasure, portability)
- **Threat modelling** (LINDDUN-lite)
- **International portability** (GDPR vs non-EU contexts)

This guidance is intended for:
- **Data controllers** (landlords, housing providers, care organisations, employers)
- **Data processors** (SaaS providers, software vendors)
- **Data Protection Officers (DPOs)**
- **Implementers** designing systems that handle OpenPEEP documents
- **International adopters** adapting to local privacy laws

---

## Legal Framework

### UK GDPR (Retained EU Law)

OpenPEEP is designed to comply with **UK GDPR** (retained EU law post-Brexit). Key principles:

1. **Lawfulness, fairness, and transparency** (Article 5(1)(a))
2. **Purpose limitation** (Article 5(1)(b))
3. **Data minimisation** (Article 5(1)(c))
4. **Accuracy** (Article 5(1)(d))
5. **Storage limitation** (Article 5(1)(e))
6. **Integrity and confidentiality** (Article 5(1)(f))
7. **Accountability** (Article 5(2))

### Other Privacy Laws

For international implementations:

- **EU GDPR** (Regulation (EU) 2016/679) — EU member states
- **CCPA / CPRA** (California Consumer Privacy Act) — USA (California)
- **PIPEDA** (Personal Information Protection and Electronic Documents Act) — Canada
- **POPIA** (Protection of Personal Information Act) — South Africa
- **LGPD** (Lei Geral de Proteção de Dados) — Brazil

OpenPEEP's privacy-by-design approach is compatible with these frameworks.

---

## Data Minimisation

### Principle

**Collect only what is necessary for evacuation planning.**

OpenPEEP schemas enforce data minimisation by:

1. **Avoiding medical diagnoses** — PCFRA captures functional capability only
2. **Minimal person identifiers** — Person Profile excludes impairment data
3. **Purpose-specific fields** — every field has a documented evacuation-related purpose
4. **Optional fields** — most fields are optional; required fields are minimal
5. **Pseudonymisation in examples** — all example data uses fictitious identities

### Annotations and Personal Data

OpenPEEP schemas include two machine-readable annotations — `x-openpeep-kind` and `x-openpeep-domain` — to classify fields by role and domain. These are **schema metadata only** and do not add new categories of personal data. They help consumers group fields, drive accessibility-friendly UIs, and support analytics aligned to the NFCC framework without weakening data minimisation principles.

### What NOT to Collect

The following should **NOT** be stored in OpenPEEP documents:

- ❌ **Medical diagnoses** (e.g., "has Parkinson's disease", "diagnosed with COPD")
- ❌ **Clinical treatment data** (e.g., medication names, dosages, treatment plans)
- ❌ **Long-term health records** (e.g., GP records, hospital discharge summaries)
- ❌ **Genetic data** (special category under GDPR)
- ❌ **Biometric identifiers** (e.g., fingerprints, facial recognition data)
- ❌ **Unnecessary personal details** (e.g., marital status, sexual orientation, religion) unless directly relevant to evacuation (rare)

### What to Collect (Minimal, Functional)

The following are **necessary and proportionate** for evacuation planning:

- ✅ **Functional capability** (e.g., "cannot use stairs", "requires vibrating pager for alarms")
- ✅ **Observable behaviours** affecting fire risk (e.g., "smoking in bed observed", "hoarding narrowing pathways")
- ✅ **Assistance needs** (e.g., "needs 2 persons to evacuate", "requires evac chair")
- ✅ **Communication preferences** (e.g., "large print preferred", "BSL interpreter required")
- ✅ **Contact details** for emergency liaison (if consented)
- ✅ **Address** for premises-specific planning
- ✅ **Consent decision** (given, refused, withdrawn)

---

## Lawful Basis for Processing

Under UK GDPR, **at least one lawful basis** must apply. For OpenPEEP, the most common bases are:

### 1. **Consent** (Article 6(1)(a))

- Used when the individual has **freely given, specific, informed, and unambiguous consent**
- Required for **RPEEP** under Residential Regs 2025 (consent must be offered; refusal recorded)
- OpenPEEP support: `consent.schema.json` records `state` (given/refused/withdrawn), `scope`, `method`, `capturedAt`

**Example:**

```json
{
  "state": "given",
  "capturedAt": "2025-10-14T10:00:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["pcfra", "peep", "rpeep", "ees_person_statement", "share_with_frs"],
  "method": "written"
}
```

**Withdrawal:** Residents can withdraw consent at any time. Record withdrawal:

```json
{
  "state": "withdrawn",
  "capturedAt": "2025-11-01T14:30:00Z",
  "capturedBy": "housing.officer@example.org",
  "scope": ["rpeep", "share_with_frs"],
  "method": "verbal",
  "reason": "No longer wishes to share with FRS"
}
```

### 2. **Legal Obligation** (Article 6(1)(c))

- Used when processing is **necessary to comply with a legal obligation** (e.g., FSO 2005, Residential Regs 2025)
- Applies to duty holders (landlords, employers) preparing evacuation plans under statutory duty

**Example:** Employer preparing a workplace PEEP under FSO 2005 (duty to identify relevant persons and make fire safety arrangements).

### 3. **Vital Interests** (Article 6(1)(d))

- Used when processing is **necessary to protect someone's life**
- May apply in emergency situations (e.g., sharing PEEP with FRS during an active fire incident, even without prior consent)

**Example:** Fire incident; FRS requests PEEP to inform rescue operation.

### 4. **Public Task** (Article 6(1)(e))

- Used when processing is **necessary for a task carried out in the public interest** or in the exercise of official authority
- Applies to public authorities (e.g., local authority housing, NHS trusts)

**Example:** Local authority housing provider preparing RPEEPs for social housing tenants.

### Special Category Data (Article 9)

**OpenPEEP avoids special category data** (health data, racial/ethnic origin, religious beliefs, etc.) wherever possible. However, **functional capability data** may be inferred to reveal disability.

If special category data is processed, an **Article 9 condition** must also apply (in addition to Article 6):

- **Article 9(2)(b)**: Employment, social security, and social protection law
- **Article 9(2)(c)**: Vital interests (if data subject incapable of giving consent)
- **Article 9(2)(h)**: Health or social care (provision of care)

**Mitigation:** OpenPEEP captures **functional capability**, not diagnoses. This reduces (but does not eliminate) special category risk.

---

## Purpose Limitation

### Defined Purposes

OpenPEEP documents may be processed **only** for the following purposes:

1. **Fire risk assessment** (PCFRA)
2. **Evacuation planning** (PEEP, RPEEP)
3. **Provision of accessible evacuation information** (EES Personal, EES Building)
4. **Sharing with Fire and Rescue Service** (with consent, for emergency response)
5. **Compliance with fire safety regulations** (FSO 2005, Residential Regs 2025, Building Safety Act 2022)
6. **Golden thread record-keeping** (Building Safety Act 2022)
7. **Review and continuous improvement** (annual review cycles)

### Prohibited Purposes

OpenPEEP documents **must NOT** be used for:

- ❌ **Marketing or commercial profiling**
- ❌ **Insurance underwriting** (unless explicitly consented and disclosed)
- ❌ **Employment decisions** unrelated to fire safety (e.g., recruitment, promotion)
- ❌ **Research** without explicit consent and ethical approval
- ❌ **Surveillance** or monitoring unrelated to fire safety

---

## Retention Periods

### Recommended Retention

| Document Type | Retention Period | Rationale | Legal Basis |
|---------------|------------------|-----------|-------------|
| **PCFRA (Personal)** | **Duration of occupancy + 6 years** | Regulatory retention for fire safety records | FSO 2005; Limitation Act 1980 |
| **PCFRA (Environmental)** | **Duration of occupancy + 6 years** | Regulatory retention for fire safety records | FSO 2005; Limitation Act 1980 |
| **PEEP / RPEEP** | **Duration of occupancy/employment + 6 years** | Regulatory retention; potential litigation | FSO 2005; Residential Regs 2025 |
| **Consent** | **Life of associated document + 6 years** | Audit trail for lawful basis | UK GDPR (accountability) |
| **Review** | **Life of associated document + 6 years** | Audit trail for compliance | FSO 2005; Building Safety Act 2022 |
| **EES (Personal)** | **Duration of occupancy + 6 years** | Regulatory retention; information duty | Residential Regs 2025 |
| **EES (Building)** | **Life of building + golden thread retention** | Golden thread record | Building Safety Act 2022 |

### Archival and Deletion

**After retention period expires:**

1. **Archive** documents securely (encrypted, access-controlled)
2. **Pseudonymise or anonymise** personal data if retention required for statistical/research purposes
3. **Delete** securely when no longer required (GDPR Article 17: right to erasure)

**Triggering events for deletion:**

- Occupancy ends (tenancy terminated, employee leaves, care resident moves)
- Consent withdrawn (if consent is sole lawful basis)
- Data no longer accurate and cannot be rectified
- Legal retention period expires

---

## Data Controller and Processor Roles

### Data Controller

The **data controller** determines the **purposes and means** of processing. For OpenPEEP:

| Context | Data Controller | Responsibilities |
|---------|----------------|------------------|
| **Residential RPEEP** | Landlord, housing provider, or care home operator | Determine who is a "relevant resident"; prepare RPEEPs; obtain consent; review annually; share with FRS (with consent) |
| **Workplace PEEP** | Employer | Identify employees/visitors needing PEEPs; prepare PEEPs; provide training; review regularly |
| **Public building PEEP** | Building operator (e.g., university, hospital, transport authority) | Identify persons needing PEEPs; make reasonable adjustments; provide accessible information |

**Obligations:**

- Determine **lawful basis** for processing
- Ensure **data minimisation**
- Implement **technical and organisational measures** (TOMs) for security
- Respond to **data subject rights** requests
- Maintain **records of processing activities** (GDPR Article 30)
- Appoint a **Data Protection Officer (DPO)** if required

### Data Processor

The **data processor** processes personal data **on behalf of the controller**. For OpenPEEP:

| Context | Data Processor | Responsibilities |
|---------|----------------|------------------|
| **SaaS PEEP platform** | Software vendor providing PEEP management system | Process data as instructed by controller; implement security measures; notify controller of breaches; delete/return data on request |
| **Third-party assessor** | Consultant conducting PCFRAs on behalf of landlord | Conduct assessments as instructed; return data to controller; not reuse data for own purposes |

**Obligations:**

- Process data **only on documented instructions** from controller
- Implement **appropriate security measures**
- Maintain **confidentiality**
- **Notify controller** of personal data breaches (within 72 hours if feasible)
- **Assist controller** with data subject rights requests
- **Delete or return** data at end of contract

**Data Processing Agreement (DPA):**

Controllers and processors **must** have a **written contract** (GDPR Article 28) specifying:

- Subject matter and duration of processing
- Nature and purpose of processing
- Type of personal data and categories of data subjects
- Controller's obligations and rights
- Processor's obligations (security, confidentiality, sub-processing, breach notification, deletion)

---

## Data Subject Rights

### Rights Under UK GDPR

Data subjects (residents, employees, visitors) have the following rights:

| Right | Article | OpenPEEP Support |
|-------|---------|------------------|
| **Right to be informed** | Art 13-14 | Controllers must provide privacy notice; OpenPEEP documents are structured and transparent |
| **Right of access** (SAR) | Art 15 | Controllers must provide copy of PCFRA, PEEP, Consent, Review, EES upon request |
| **Right to rectification** | Art 16 | Controllers must correct inaccurate data; OpenPEEP supports versioning and review |
| **Right to erasure** ("right to be forgotten") | Art 17 | Controllers must delete data when lawful basis ceases (e.g., consent withdrawn, retention period expired) |
| **Right to restriction** | Art 18 | Controllers must restrict processing (e.g., mark as "under review") while accuracy disputed |
| **Right to data portability** | Art 20 | Controllers must provide OpenPEEP documents in **machine-readable format** (JSON) |
| **Right to object** | Art 21 | Data subjects can object to processing based on public task or legitimate interests (not consent or legal obligation) |
| **Rights related to automated decision-making** | Art 22 | OpenPEEP documents are typically **human-reviewed**; automated PEEP generation must allow for human intervention |

### Subject Access Request (SAR) Response

When a data subject requests access to their OpenPEEP documents:

1. **Verify identity** of requester
2. **Locate all relevant documents**:
   - PCFRA (Personal)
   - PCFRA (Environmental)
   - PEEP / RPEEP
   - Consent records
   - Review records
   - EES (Personal)
3. **Provide in machine-readable format** (JSON preferred; or human-readable PDF/HTML)
4. **Respond within 1 month** (GDPR Article 15(3))
5. **No fee** unless request is manifestly unfounded or excessive

**Example SAR response bundle:**

```json
{
  "subject_access_request": {
    "request_date": "2025-10-14",
    "requester": "Jane Doe",
    "documents": [
      {
        "type": "pcfra_person",
        "id": "pcfra-uuid-1234",
        "file": "pcfra_person_jane_doe.json"
      },
      {
        "type": "peep",
        "id": "peep-uuid-5678",
        "file": "peep_jane_doe.json"
      },
      {
        "type": "consent",
        "id": "consent-uuid-9abc",
        "file": "consent_jane_doe.json"
      },
      {
        "type": "ees_personal",
        "id": "ees-uuid-def0",
        "file": "ees_personal_jane_doe.json"
      }
    ]
  }
}
```

---

## Security Measures (TOMs)

### Technical Measures

Recommended technical measures for OpenPEEP systems:

1. **Encryption**:
   - **In transit**: TLS 1.2+ for all API calls
   - **At rest**: AES-256 for database storage; full-disk encryption

2. **Access control**:
   - Role-based access control (RBAC) for duty holders, assessors, residents
   - Multi-factor authentication (MFA) for privileged accounts
   - Audit logs for all access and modifications

3. **Pseudonymisation**:
   - Use UUIDs for `personRef`, `peepId`, `pcfraId`
   - Avoid embedding names in URLs or logs

4. **Data integrity**:
   - Schema validation (AJV) on ingestion
   - Digital signatures (optional) for provenance
   - Hashing (SHA-256) for attachments

5. **Backup and recovery**:
   - Encrypted backups with secure off-site storage
   - Disaster recovery plan (RTO/RPO defined)

### Organisational Measures

Recommended organisational measures:

1. **Training**:
   - Data protection training for staff handling PEEPs
   - Fire safety training for assessors

2. **Policies**:
   - Data retention policy
   - Breach notification policy
   - Privacy impact assessment (PIA) for new processing

3. **Governance**:
   - Appoint Data Protection Officer (DPO) if required (GDPR Article 37)
   - Steering group with data protection representation

4. **Contracts**:
   - Data Processing Agreements (DPAs) with processors
   - Non-disclosure agreements (NDAs) with third-party assessors

---

## Threat Modelling (LINDDUN-lite)

**LINDDUN** is a privacy threat modelling framework (Linkability, Identifiability, Non-repudiation, Detectability, Disclosure of information, Unawareness, Non-compliance).

### Privacy Threat Checklist for OpenPEEP Implementations

| Threat | Risk | Mitigation |
|--------|------|------------|
| **Linkability** (linking PEEP to other datasets) | High (especially if UUIDs are reused across systems) | Use **system-specific UUIDs**; avoid sharing raw UUIDs externally; pseudonymise in exports |
| **Identifiability** (re-identifying pseudonymised data) | Medium (name + address + capability = identifiable) | **Data minimisation**; avoid storing unnecessary identifiers; restrict access to `name` field |
| **Non-repudiation** (inability to deny an action) | Low (audit logs needed for accountability) | **Audit logs** with tamper-proof timestamps; digital signatures for consent |
| **Detectability** (inferring presence of PEEP reveals disability) | Medium (existence of PEEP implies evacuation need) | **Access control**; encrypt file names; avoid exposing PEEP existence in public APIs |
| **Disclosure of information** (unauthorised access) | High (sensitive functional capability data) | **Encryption** (at rest, in transit); RBAC; audit logs; breach detection |
| **Unawareness** (data subjects unaware of processing) | Medium (residents may not understand PEEP use) | **Privacy notice** in plain language; consent process; EES Personal as transparency tool |
| **Non-compliance** (failure to meet GDPR/fire safety regs) | High (fines, reputational damage) | **Schema validation**; compliance checklist; DPO oversight; regular audits |

### Data Breach Response

If a personal data breach occurs:

1. **Detect and contain** (immediately isolate affected systems)
2. **Assess severity** (likelihood and severity of risk to data subjects)
3. **Notify controller** (processor must notify controller **within 72 hours** if feasible)
4. **Notify ICO** (controller must notify ICO within 72 hours if breach likely to result in risk to rights and freedoms)
5. **Notify data subjects** (if high risk to rights and freedoms)
6. **Document** breach in breach register (GDPR Article 33(5))
7. **Remediate** and prevent recurrence

**Example breach scenarios:**

- **Scenario 1**: Unauthorised access to PEEP database (50 records exposed)
  - **Action**: Notify ICO (within 72 hours); notify data subjects; reset credentials; review access controls

- **Scenario 2**: Email sent to wrong recipient (1 PEEP attached)
  - **Action**: Request deletion from recipient; assess risk; notify ICO if high risk; document in breach register

---

## International Portability: GDPR vs Non-EU Contexts

### GDPR Applies When:

- **Controller or processor is established in the EU/UK**
- **Data subjects are in the EU/UK** (regardless of controller location) and processing relates to:
  - Offering goods/services to EU/UK residents
  - Monitoring behaviour of EU/UK residents

### Non-EU Contexts

For implementations outside the EU/UK (e.g., USA, Canada, Australia, Brazil):

- **Local privacy laws apply** (e.g., CCPA in California, PIPEDA in Canada, LGPD in Brazil)
- **OpenPEEP's privacy-by-design principles remain relevant**:
  - Data minimisation
  - Purpose limitation
  - Consent management
  - Retention limits
  - Data subject rights

**Recommended approach:**

1. **Map local privacy law to OpenPEEP** (similar to GDPR mapping in this document)
2. **Define jurisdiction-specific extensions** (`extensions.usa`, `extensions.au`, etc.)
3. **Document lawful basis** under local law (e.g., CCPA "business purpose", PIPEDA "consent")
4. **Provide privacy notice** in local language and format

**Example (USA / CCPA):**

```json
{
  "extensions": {
    "usa": {
      "ccpaBusinessPurpose": "fire_safety_evacuation_planning",
      "ccpaDoNotSellOptOut": true,
      "ccpaPrivacyNoticeUrl": "https://example.com/privacy-notice"
    }
  }
}
```

---

## Privacy Notice Template

Controllers must provide a **privacy notice** to data subjects. Below is a **template outline** for RPEEP context:

---

### Privacy Notice: Residential Personal Emergency Evacuation Plan (RPEEP)

**Who we are:**  
[Organisation name], [address], [contact details]

**Data Protection Officer (if applicable):**  
[DPO name/contact]

**What personal data we collect:**
- Your name, contact details, and address
- Your functional evacuation capabilities (mobility, sensory, cognitive, communication)
- Your consent decision (given, refused, or withdrawn)
- Environmental risks observed in your home (if assessed)

**Why we collect this data:**
- To prepare a Personal Emergency Evacuation Plan (PEEP) for you
- To comply with our legal duties under the Fire Safety (Residential Evacuation Plans) (England) Regulations 2025 and the Regulatory Reform (Fire Safety) Order 2005
- To provide you with accessible evacuation information
- To share your PEEP with the Fire and Rescue Service (with your consent)

**Lawful basis:**
- **Consent** (you have given us consent to prepare an RPEEP)
- **Legal obligation** (we have a statutory duty to prepare evacuation plans for relevant residents)

**Who we share your data with:**
- Fire and Rescue Service (with your consent)
- Third-party assessors conducting PCFRAs (under contract)
- SaaS providers managing our PEEP system (as data processors)

**How long we keep your data:**
- For the duration of your tenancy + 6 years after you move out

**Your rights:**
- Access your PEEP and related records (Subject Access Request)
- Correct inaccurate information
- Request deletion (if lawful basis no longer applies)
- Withdraw consent at any time (note: we may still have a legal duty to maintain some records)
- Object to processing (in limited circumstances)
- Data portability (receive your PEEP in machine-readable format)

**How to exercise your rights:**  
Contact us at [email/phone]. We will respond within 1 month.

**How to complain:**  
If you are unhappy with how we handle your data, contact the Information Commissioner's Office (ICO) at [https://ico.org.uk](https://ico.org.uk) or call 0303 123 1113.

---

---

## Compliance Checklist

### For Data Controllers

- [ ] **Determine lawful basis** for processing (consent, legal obligation, vital interests, public task)
- [ ] **Provide privacy notice** to data subjects
- [ ] **Obtain consent** where required (RPEEP, sharing with FRS)
- [ ] **Record consent decisions** (given, refused, withdrawn) in `consent.schema.json`
- [ ] **Implement data minimisation** (collect only functional capability, not diagnoses)
- [ ] **Set retention periods** (duration of occupancy + 6 years)
- [ ] **Implement technical measures** (encryption, access control, audit logs)
- [ ] **Implement organisational measures** (training, policies, DPO oversight)
- [ ] **Respond to SARs** within 1 month
- [ ] **Notify ICO of breaches** within 72 hours (if applicable)
- [ ] **Maintain records of processing activities** (GDPR Article 30)

### For Data Processors

- [ ] **Have a Data Processing Agreement (DPA)** with controller
- [ ] **Process data only on controller's instructions**
- [ ] **Implement appropriate security measures** (encryption, access control)
- [ ] **Notify controller of breaches** within 72 hours
- [ ] **Assist controller with SARs and other rights requests**
- [ ] **Delete or return data** at end of contract

---

## Change Log

| Date | Version | Change | Rationale |
|------|---------|--------|-----------|
| 2025-10-14 | 1.0.0-beta | Initial privacy & GDPR guidance | Baseline documentation |

---

## References

### UK/EU Privacy Law

- **UK GDPR** (retained EU law; Data Protection Act 2018)
- **EU GDPR** (Regulation (EU) 2016/679)
- **Data Protection Act 2018** (c. 12)
- **Information Commissioner's Office (ICO) guidance**: [https://ico.org.uk](https://ico.org.uk)

### International Privacy Laws

- **CCPA / CPRA** (California Consumer Privacy Act; California Privacy Rights Act)
- **PIPEDA** (Personal Information Protection and Electronic Documents Act, Canada)
- **POPIA** (Protection of Personal Information Act, South Africa)
- **LGPD** (Lei Geral de Proteção de Dados, Brazil)

### Privacy-by-Design Frameworks

- **LINDDUN** threat modelling framework: [https://www.linddun.org](https://www.linddun.org)
- **Privacy by Design** (Ann Cavoukian, 7 Foundational Principles)

---

## Next Steps

- **Review [Data Dictionary](data_dictionary.md)** for field definitions
- **Review [Mapping to Guidance](mapping_to_guidance.md)** for UK regulation anchors
- **Consult [Overview](overview.md)** for concept model
- **Implement technical measures** (encryption, access control, audit logs)

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

