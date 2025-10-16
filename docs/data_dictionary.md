# Data Dictionary — OpenPEEP

**Version:** 1.0.0-beta  
**Last updated:** 14 October 2025

---

## Purpose

This document provides an **authoritative field catalogue** for all OpenPEEP schemas. It includes:

- Field name, type, cardinality, and description
- Purpose and use case
- Legal basis (where applicable)
- Relationship to UK regulations and international standards

This dictionary is intended for:
- **Implementers** building systems that consume or produce OpenPEEP documents
- **Data controllers** assessing lawful basis and data minimisation
- **Standards editors** ensuring schema consistency
- **Regulators** auditing compliance

---

## Schema Index

- [PEEP (Personal Emergency Evacuation Plan)](#peep-personal-emergency-evacuation-plan)
- [PCFRA (Person) — Personal PCFRA](#pcfra-person--personal-pcfra)
- [PCFRA (Environment) — Environmental PCFRA](#pcfra-environment--environmental-pcfra)
- [Consent](#consent)
- [Review](#review)
- [Person Profile](#person-profile)
- [Address](#address)
- [EES (Building) — Building Emergency Evacuation Statement](#ees-building--building-emergency-evacuation-statement)
- [EES (Personal) — Personal Emergency Evacuation Statement](#ees-personal--personal-emergency-evacuation-statement)

---

## PEEP (Personal Emergency Evacuation Plan)

**Schema:** `peep.schema.json`  
**$id:** `https://open-peep.org/schemas/peep/2025-08/peep.schema.json`

### Top-level Properties

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `schemaVersion` | string (SEMVER) | ✅ | Semantic version of the PEEP schema | Versioning, backwards compatibility | N/A (metadata) |
| `peepId` | string (UUID v4) | ✅ | Unique identifier for this PEEP | Identity, golden thread linkage | Building Safety Act 2022 (golden thread) |
| `personProfile` | object (ref) | ✅ | Reference to Person Profile schema | Identity anchor | FSO 2005 (identify relevant persons) |
| `address` | object (ref) | ✅ | Reference to Address schema | Location context | FSO 2005 (premises-specific planning) |
| `evacuationStrategy` | string (enum or custom) | ✅ | Primary evacuation strategy (e.g., Stay Put, Simultaneous, Phased) | Building context for plan | Approved Document B; NFCC framework |
| `language` | string (ISO 639-1) | ❌ | Primary language code for plan content | Accessibility | Equality Act 2010 (reasonable adjustments) |
| `planSummary` | string (max 4000) | ✅ | Plain-language summary of the evacuation plan | Clarity, accessibility | Equality Act 2010 (accessible information) |
| `translations` | array of objects | ❌ | Optional translations of plan summary | Multilingual accessibility | Equality Act 2010 |
| `metadata` | object | ✅ | Creation metadata (date, author, status, provenance) | Auditability, golden thread | Building Safety Act 2022 |
| `validity` | object | ❌ | Effective date range (`effectiveFrom`, `effectiveTo`) | Lifecycle management | N/A (process) |
| `routes` | array of objects | ✅ | Evacuation routes (minimum 1; must include "Primary") | Procedural clarity | FSO 2005; Approved Document B |
| `communications` | object | ❌ | Alarm hearing, alerting needs, raise-alarm methods | Functional adjustments | Equality Act 2010; NFCC framework |
| `nighttimeConsiderations` | string (max 4000) | ❌ | Special considerations for night-time evacuation | Risk mitigation | NFCC framework |
| `trainingDrills` | string (max 4000) | ❌ | Training and drill arrangements | Competency, preparedness | FSO 2005 (training duty) |
| `criticalDependencies` | array of objects | ❌ | Dependencies (e.g., oxygen, power, carer) | Risk awareness | NFCC framework |
| `evacuationProcedure` | array of objects | ✅ | Step-by-step evacuation instructions | Procedural clarity | FSO 2005 |
| `responsibilities` | array of objects | ❌ | Roles and contact details for responsible persons | Duty allocation | FSO 2005 (responsible person) |
| `attachments` | array of objects | ❌ | Supporting documents (e.g., floor plans, photos) | Evidence, context | Building Safety Act 2022 |
| `references` | object | ❌ | References to source PCFRA and EES | Traceability | Building Safety Act 2022 (golden thread) |
| `consent` | object (ref) | ❌ | Reference to Consent schema | Lawful basis | Residential Regs 2025; UK GDPR |
| `reviews` | array of objects (ref) | ❌ | Lifecycle reviews | Continuous improvement | FSO 2005 (review duty) |
| `extensions` | object | ❌ | Jurisdiction/vendor-specific extensions (e.g., gbr, eu, usa) | Portability | N/A (extensibility) |

### Metadata Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `createdAt` | string (date-time) | ✅ | UTC timestamp of creation | Auditability |
| `createdBy` | string (max 512) | ✅ | Identifier of creator (role or user ID) | Provenance |
| `method` | string (enum) | ❌ | How the PEEP was created: `self_assessment`, `phone_interview`, `in_person`, `other` | Quality assurance |
| `versionRef` | string (max 256) | ❌ | Version tag for document control | Versioning |
| `documentRef` | string (URI) | ❌ | Canonical location of the PEEP | Golden thread |
| `status` | string (enum) | ✅ | Document control status: `draft`, `approved`, `superseded`, `withdrawn` | Lifecycle |
| `goldenThreadUrn` | string | ❌ | Optional reference to building's Golden Thread record | Building Safety Act 2022 |
| `eesRef` | string | ❌ | Optional reference to related Building EES | Cross-reference |

### Routes Array (route object)

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `name` | string (max 64) | ✅ | Route name (e.g., "Primary", "Secondary") | Identification |
| `pathDescription` | string (max 4000) | ✅ | Plain-language description of the route | Clarity |
| `assistance` | array of objects | ❌ | Assistance required (type, persons, role, competency, availability) | Staffing |
| `equipment` | array of strings | ❌ | Equipment needed (e.g., evac chair, transfer board) | Resource planning |
| `refuge` | object | ❌ | Refuge location and wait condition | Staying put / phased evac |
| `musterPoint` | string (max 256) | ❌ | Destination for roll call | Accountability |
| `preconditions` | string (max 2000) | ❌ | Conditions that must be met for this route | Contingency |
| `contingency` | string (max 2000) | ❌ | Fallback if route is unavailable | Resilience |
| `evacuationLift` | object | ❌ | Evacuation lift details (used, standard, attendant, notes) | Lift-assisted evac |
| `notes` | string (max 2000) | ❌ | Additional route-specific notes | Flexibility |

### Communications Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `alarmHearingAbility` | string (enum) | ❌ | `hears_audible`, `needs_visual`, `needs_vibration`, `unknown` | Accessibility |
| `alertingMethods` | array (enum) | ❌ | Methods available: `visual_beacons`, `vibrating_pager`, `text_message`, `phone_call`, `door_knock`, `public_address` | Adjustments |
| `alertingNeeds` | array (enum) | ❌ | Needs: `visual_beacons`, `vibrating_pager`, `text_only`, `captioned_media`, `bsl_interpreter_required`, `high_contrast`, `large_print`, `plain_language` | Equality Act 2010 |
| `raiseAlarmMethods` | array (enum) | ❌ | How individual can raise alarm: `manual_call_point`, `pull_cord`, `intercom`, `phone_call`, `call_999`, `pendant`, `other` | Empowerment |
| `notes` | string (max 2000) | ❌ | Additional notes | Flexibility |

### Extensions (gbr profile example)

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `extensions.gbr.relevantResident` | boolean | ❌ | Whether individual is a "relevant resident" for Residential Regs 2025 | RPEEP determination |
| `extensions.gbr.residentFacingStatementRef` | string | ❌ | Link to plain-language EES Personal | Information duty (Reg 10) |
| `extensions.gbr.informationDutyEvidenceRef` | string | ❌ | Evidence of information provided to residents/FRS | Compliance (Regs 10/12) |
| `extensions.gbr.stayingPutBoundary` | string | ❌ | Limits/conditions of staying put | Clarity |

---

## PCFRA (Person) — Personal PCFRA

**Schema:** `pcfra.person.schema.json`  
**$id:** `https://open-peep.org/schemas/pcfra/pcfra.person.schema.json`

### Top-level Properties

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `pcfraId` | string | ❌ | Stable identifier for this PCFRA | Identity | Golden thread |
| `version` | string | ❌ | Schema/document version tag | Auditability | N/A |
| `personRef` | string | ✅ | Identifier linking to Person Profile | Cross-reference | FSO 2005 |
| `assessedAt` | string (date-time) | ✅ | ISO 8601 timestamp of assessment | Provenance | N/A |
| `assessorRole` | string | ❌ | Role of assessor (e.g., housing_officer, fire_safety_advisor, self) | Provenance | N/A |
| `assessmentMethod` | string (enum) | ✅ | `phone`, `digital_self`, `in_person` | Quality assurance | NFCC framework |
| `confidenceLevel` | string (enum) | ❌ | `low`, `medium`, `high` | Quality flag | NFCC framework |
| `consentRef` | string | ❌ | Link to Consent record | Lawful basis | UK GDPR; Residential Regs 2025 |
| `capability` | object | ❌ | Functional evacuation capabilities | Core assessment | Equality Act 2010; NFCC |
| `outcome` | object | ✅ | Decision outputs (`relevantResident`, `peepRequired`, `rationale`) | Triggering PEEP | Residential Regs 2025 |
| `provenance` | array of objects | ❌ | Source/verification details for specific fields | Auditability | Building Safety Act 2022 |
| `reviews` | array of objects (ref) | ❌ | Lifecycle reviews | Continuous improvement | FSO 2005 |
| `notes` | string | ❌ | Optional free text (minimal, non-sensitive) | Flexibility | N/A |
| `attachments` | array of strings | ❌ | Host-system attachment identifiers | Evidence | N/A |

### Capability Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `walkingEndurance` | enum | ❌ | `normal_pace`, `needs_frequent_breaks`, `limited_distance`, `cannot_walk` | Mobility assessment |
| `respiratoryTolerance` | enum | ❌ | `normal`, `mild_breathlessness_on_exertion`, `frequent_breaks_required`, `oxygen_support_needed` | Breathing tolerance |
| `mobilityTransfer` | enum | ❌ | `independent_transfer`, `stand_pivot_with_support`, `hoist_or_full_lift_required`, `unable_to_transfer` | Transfer capability |
| `stairTolerance` | enum | ❌ | `stairs_independent`, `stairs_with_assist`, `cannot_use_stairs` | Stair usage |
| `mobilityAid` | enum | ❌ | `none`, `stick`, `frame`, `assisted_walking`, `wheelchair_manual`, `wheelchair_powered` | Equipment |
| `hearingSupport` | enum | ❌ | `none`, `hearing_aid`, `visual_alerts_required`, `vibrating_pager` | Auditory support |
| `sensoryHearing` | enum | ❌ | `typical`, `reduced`, `profound` | Hearing baseline |
| `visionSupport` | enum | ❌ | `none`, `high_contrast_signage`, `tactile_guidance`, `additional_lighting` | Visual support |
| `sensoryVision` | enum | ❌ | `typical`, `low_vision`, `blind` | Vision baseline |
| `cognitionSupport` | enum | ❌ | `none`, `step_by_step_prompts`, `carer_guidance_required` | Cognitive support |
| `cognitionAbility` | enum | ❌ | `typical`, `mild_support_needed`, `significant_support_needed` | Cognitive baseline |
| `communicationSupport` | enum | ❌ | `none`, `plain_language`, `BSL`, `AAC_device`, `text_only` | Communication methods |
| `communicationAbility` | enum | ❌ | `typical`, `mild_difficulty`, `severe_difficulty`, `non_verbal` | Communication baseline |
| `nightResponse` | enum | ❌ | `wakes_to_alarm`, `requires_vibrating_pillow`, `deep_sleeper_support` | Night-time awareness |
| `alertnessSupport` | enum | ❌ | `none`, `prompts_needed_when_distressed`, `simplified_instructions_required` | Alertness adjustments |
| `alertnessEffects` | enum | ❌ | `typical`, `mildly_affected`, `significantly_affected` | Medication/substance effects |
| `equipmentDependency` | enum | ❌ | `none`, `oxygen_therapy`, `ventilation_device`, `powered_bed`, `other_equipment` | Critical equipment |
| `behaviouralRisks` | array (enum) | ❌ | Observable behaviours: `smoking_in_bed`, `hoarding_observed`, `aggressive_towards_staff`, `non_cooperation_with_safety_guidance`, `confusion_in_emergency`, `substances_present`, `fire_setting_behaviour`, `other_observed` | Risk factors (non-diagnostic) |
| `assistRequired` | enum | ❌ | `none`, `needs_assist_1`, `needs_assist_2` | Human assistance summary |
| `assistLevel` | enum | ❌ | `none`, `occasional_support`, `constant_support` | Assistance level |
| `equipmentNeeded` | array (enum) | ❌ | `evac_chair_required`, `transfer_board`, `evacuation_sheet`, `stair_climber`, `rescue_sledge` | Equipment planning |

### Outcome Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `relevantResident` | boolean | ✅ | True if in scope for Residential Regs 2025 | RPEEP trigger |
| `peepRequired` | boolean | ✅ | True if PEEP required | Plan trigger |
| `rationale` | string | ✅ | Short justification (no medical detail) | Transparency |

### Provenance Array (provenance object)

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `path` | string | ✅ | JSON Pointer to field (e.g., `/capability/mobilityAid`) | Field-level tracking |
| `source` | enum | ✅ | `self_report`, `assessor_observed`, `third_party_record`, `system_generated` | Origin |
| `evidence` | enum | ❌ | `visual_inspection`, `verbal_account`, `document_seen`, `device_log`, `photo_evidence`, `other_evidence` | Verification |
| `confidenceLevel` | enum | ❌ | `low`, `medium`, `high` | Field-level confidence |
| `recordedByRole` | string | ❌ | Role of recorder | Provenance |
| `verifiedByRole` | string | ❌ | Role of verifier (if any) | Verification |
| `verifiedAt` | date-time | ❌ | Verification timestamp | Auditability |
| `notes` | string | ❌ | Non-sensitive clarifier | Flexibility |

---

## PCFRA (Environment) — Environmental PCFRA

**Schema:** `pcfra.environment.schema.json`  
**$id:** `https://open-peep.org/schemas/pcfra/pcfra.environment.schema.json`

### Top-level Properties

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `pcfraId` | string | ❌ | Stable identifier | Identity | Golden thread |
| `version` | string | ❌ | Version tag | Auditability | N/A |
| `buildingRef` | string | ❌ | Building identifier (required if no unitRef) | Location | FSO 2005 |
| `unitRef` | string | ❌ | Unit/flat/room identifier (required if no buildingRef) | Location | FSO 2005 |
| `assessedAt` | string (date-time) | ✅ | Assessment timestamp | Provenance | N/A |
| `assessorRole` | string | ❌ | Role of assessor | Provenance | N/A |
| `assessmentMethod` | string (enum) | ✅ | `phone`, `digital_self`, `in_person` | Quality | NFCC |
| `confidenceLevel` | string (enum) | ❌ | `low`, `medium`, `high` | Quality flag | NFCC |
| `consentRef` | string | ❌ | Link to Consent record | Lawful basis | UK GDPR |
| `environment` | object | ❌ | Observed environmental risks | Core assessment | FSO 2005; NFCC |
| `safeguarding` | object | ❌ | Escalation and multi-agency context | Safeguarding duty | Care Act 2014 |
| `outcome` | object | ✅ | Decision outputs | Triggering PEEP | Residential Regs 2025 |
| `provenance` | array of objects | ❌ | Source/verification details | Auditability | Building Safety Act 2022 |
| `reviews` | array of objects (ref) | ❌ | Lifecycle reviews | Continuous improvement | FSO 2005 |
| `environment_other_notes` | string | ❌ | Free text for non-enumerated risks | Flexibility | N/A |
| `notes` | string | ❌ | General notes (minimal, non-sensitive) | Flexibility | N/A |
| `attachments` | array of strings | ❌ | Attachment identifiers | Evidence | N/A |

### Environment Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `cooking_risk` | string | ❌ | E.g., `unattended_cooking`, `chip_pan_use`, `clutter_near_hob`, `oxygen_near_hob` | Ignition risk |
| `electrical_risk` | string | ❌ | E.g., `socket_covers_removed`, `overloaded_extensions`, `damaged_cables`, `DIY_wiring`, `heaters_on_extension` | Electrical hazard |
| `smoking_risk` | string | ❌ | E.g., `heavy_indoor_smoking`, `careless_disposal` | Ignition risk |
| `hoarding_risk` | string | ❌ | E.g., `pathways_narrowed`, `combustible_load_high`, `doors_blocked` | Egress obstruction |
| `accessEgress` | string | ❌ | E.g., `exit_blocked`, `exit_partially_blocked`, `key_management_issue` | Evacuation barrier |
| `ignition_sources` | array of strings | ❌ | E.g., `candles_unattended`, `multiple_space_heaters`, `open_flame` | Ignition risk |
| `pets_risk` | string | ❌ | E.g., `large_dogs_impeding_exit`, `pet_cages_blocking_route` | Egress obstruction |

### Safeguarding Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `asbViolenceRisk` | string | ❌ | E.g., `ASB_reported`, `threats_to_staff`, `police_attendance_history` | Staff safety |
| `agencyInvolved` | array (enum) | ❌ | `police`, `adult_social_care`, `mental_health_team`, `landlord_housing`, `fire_and_rescue`, `other_agency` | Multi-agency context |
| `immediateDanger` | boolean | ❌ | True if immediate life safety hazard at assessment | Escalation trigger |
| `immediateActionTaken` | string | ❌ | E.g., `isolated_circuit`, `removed_portable_heater`, `called_999` | Mitigation record |
| `referralsMade` | array of strings | ❌ | Referral record IDs | Safeguarding trail |

---

## Consent

**Schema:** `common/consent.schema.json`  
**$id:** `https://open-peep.org/schemas/common/consent.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `state` | enum | ✅ | `given`, `refused`, `withdrawn` | Consent decision | UK GDPR; Residential Regs 2025 |
| `capturedAt` | date-time | ✅ | Timestamp of consent decision | Auditability | UK GDPR |
| `capturedBy` | string | ✅ | Identifier of recorder | Provenance | UK GDPR |
| `scope` | array (enum) | ✅ | `pcfra`, `peep`, `rpeep`, `ees_person_statement`, `share_with_frs` | Purpose limitation | UK GDPR |
| `method` | enum | ❌ | `written`, `verbal`, `assisted` | Verification | N/A |
| `reason` | string | ❌ | Short reason for refusal/withdrawal | Transparency | N/A |
| `notes` | string | ❌ | Contextual notes (no medical data) | Flexibility | N/A |
| `evidenceRef` | string | ❌ | Link to signature file or form | Evidence | N/A |
| `offerMadeAt` | date-time | ❌ | When offer was made (useful for refusals) | Process tracking | N/A |
| `validFrom` | date-time | ❌ | Consent effective start | Time-bound consent | N/A |
| `validUntil` | date-time | ❌ | Consent expiry | Time-bound consent | N/A |
| `personRef` | string | ❌ | Link to Person Profile | Cross-reference | N/A |
| `documentRef` | string | ❌ | Link to specific PCFRA/PEEP/EES | Cross-reference | N/A |

---

## Review

**Schema:** `common/review.schema.json`  
**$id:** `https://open-peep.org/schemas/common/review.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `reviewed_at` | date-time | ✅ | Review timestamp | Auditability | FSO 2005 (review duty) |
| `reviewed_by` | object | ✅ | Reviewer identity (name, role, org, contact, reviewer_id) | Provenance | N/A |
| `outcome` | enum | ✅ | `confirmed_no_change`, `amended`, `superseded`, `withdrawn` | Lifecycle decision | FSO 2005 |
| `summary` | string (max 2000) | ❌ | Review summary | Transparency | N/A |
| `next_due` | date | ❌ | Next review due date | Process management | FSO 2005 |
| `new_document_ref` | object | ❌ | Link to new document (required if `amended` or `superseded`) | Traceability | Building Safety Act 2022 |
| `attachments` | array of objects | ❌ | Supporting documents | Evidence | N/A |

---

## Person Profile

**Schema:** `common/person_profile.schema.json`  
**$id:** `https://open-peep.org/schemas/common/person_profile.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `personRef` | string | ✅ | Unique reference identifier | Identity anchor | Building Safety Act 2022 (golden thread) |
| `name` | string | ❌ | Legal or chosen name | Identity | Equality Act 2010 |
| `alias` | string | ❌ | Optional alias/nickname | Privacy-sensitive contexts | N/A |
| `dob` | date | ❌ | Date of birth (ISO 8601) | Identity (if required) | Data minimisation (UK GDPR) |
| `ageBand` | enum | ❌ | `child`, `adult`, `older_adult` | Approximate age group | NFCC framework |
| `preferredLanguage` | string (ISO 639-1) | ❌ | Preferred language code | Accessibility | Equality Act 2010 |
| `contact` | object | ❌ | Phone, email | Communication | FSO 2005 (consult/inform) |
| `communicationPreferences` | object | ❌ | Channels, format preferences, notes, consent ref | Accessible communication | Equality Act 2010 |

### Communication Preferences Sub-object

| Field | Type | Required | Description | Purpose |
|-------|------|----------|-------------|---------|
| `channels` | array (enum) | ❌ | `email`, `sms`, `phone_call`, `voice_message`, `postal`, `in_app`, `relay_service` | Contact preferences |
| `formatPreferences` | array (enum) | ❌ | `plain_language`, `large_print`, `high_contrast`, `alt_text_required`, `captioned_media`, `audio_format`, `written_only`, `no_voice_calls`, `bsl_interpreter_preferred` | Accessible formats |
| `notes` | string (max 500) | ❌ | Brief clarification (no health data) | Flexibility |
| `consentRef` | string | ❌ | Link to Consent record for communications | Lawful basis |

---

## Address

**Schema:** `common/address.schema.json`  
**$id:** `https://open-peep.org/schemas/common/address.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `organisationName` | string | ❌ | Organisation or landlord | Context | N/A |
| `buildingName` | string | ❌ | Named building | Identity | N/A |
| `buildingNumber` | string | ❌ | Street number or range | Identity | N/A |
| `subBuilding` | string | ❌ | Flat, unit, or room number | Precision | Residential Regs 2025 (unit-level) |
| `addressLine1` | string | ❌ | First address line | Postal address | N/A |
| `addressLine2` | string | ❌ | Second address line | Postal address | N/A |
| `addressLine3` | string | ❌ | Third address line | Postal address | N/A |
| `locality` | string | ❌ | Locality, village, district | Postal address | N/A |
| `townCity` | string | ❌ | Town or city | Postal address | N/A |
| `county` | string | ❌ | County or region | Postal address | N/A |
| `postcode` | string | ❌ | Postal code (no format enforcement in core) | Postal address | N/A |
| `countryCode` | string (ISO 3166-1 alpha-2) | ❌ | Default: `GB` | International portability | ISO 3166 |
| `uprn` | string | ❌ | UK Unique Property Reference Number | Golden thread deduplication | Building Safety Act 2022 |
| `udprn` | string | ❌ | Royal Mail Unique Delivery Point Reference Number | Precision | N/A |
| `nationalAddressId` | string | ❌ | Generic national/regional address ID | International portability | ISO 19160 |
| `buildingRef` | string | ❌ | OpenPEEP stable building reference | Cross-reference | N/A |
| `unitRef` | string | ❌ | OpenPEEP stable unit reference | Cross-reference | N/A |

---

## EES (Building) — Building Emergency Evacuation Statement

**Schema:** `ees_building.schema.json`  
**$id:** `https://open-peep.org/schemas/ees/ees_building.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `schema_version` | string | ✅ | Default: `1.0.0` | Versioning | N/A |
| `document_id` | UUID | ✅ | Unique identifier | Identity | N/A |
| `created_at` | date-time | ✅ | Creation timestamp | Auditability | N/A |
| `updated_at` | date-time | ❌ | Last update timestamp | Auditability | N/A |
| `building` | object | ✅ | Building identity (name, address, identifiers) | Location | N/A |
| `evacuation_strategy` | enum | ✅ | `stay_put`, `simultaneous_evacuate`, `phased`, `progressive_horizontal`, `other` | Strategy | Approved Document B |
| `strategy_other_detail` | string | ❌ | If strategy is `other` | Flexibility | N/A |
| `who_should_move_when` | string | ✅ | Plain-language summary of strategy | Resident clarity | Residential Regs 2025 (Reg 10) |
| `alarm_and_alerting_summary` | string | ❌ | Alarm system summary | Resident awareness | N/A |
| `refuges_and_lifts_summary` | string | ❌ | Refuges/evacuation lifts availability | Resident awareness | N/A |
| `routes_summary` | string | ❌ | High-level escape routes description | Resident awareness | N/A |
| `muster_points` | array of objects | ❌ | Muster point labels and descriptions | Accountability | N/A |
| `resident_help_and_contacts` | object | ❌ | Building contact, out-of-hours, FRS info point | Support | Residential Regs 2025 |
| `languages_available` | array of strings | ❌ | Languages available | Accessibility | Equality Act 2010 |
| `accessibility_formats` | array (enum) | ❌ | `large_print`, `easy_read`, `audio`, `braille`, `sign_video`, `high_contrast`, `plain_english` | Accessibility | Equality Act 2010 |
| `distribution_methods` | array (enum) | ❌ | `noticeboard`, `door_drop`, `email`, `tenant_portal`, `welcome_pack`, `qr_in_common_areas` | Information duty | Residential Regs 2025 |
| `related_document_ids` | array of strings | ❌ | Links to FRA, strategy reports | Golden thread | Building Safety Act 2022 |
| `provenance` | object | ❌ | Authorship, method, source refs | Auditability | N/A |
| `reviews` | array of objects (ref) | ❌ | Lifecycle reviews | Continuous improvement | FSO 2005 |
| `extensions` | object | ❌ | Vendor-specific extensions | Extensibility | N/A |

---

## EES (Personal) — Personal Emergency Evacuation Statement

**Schema:** `ees_personal.schema.json`  
**$id:** `https://open-peep.org/schemas/ees/ees_personal.schema.json`

| Field | Type | Required | Description | Purpose | Legal Basis |
|-------|------|----------|-------------|---------|-------------|
| `schema_version` | string | ✅ | Default: `1.0.0` | Versioning | N/A |
| `document_id` | UUID | ✅ | Unique identifier | Identity | N/A |
| `created_at` | date-time | ✅ | Creation timestamp | Auditability | N/A |
| `updated_at` | date-time | ❌ | Last update timestamp | Auditability | N/A |
| `person_profile_ref` | string | ✅ | Link to Person Profile | Cross-reference | N/A |
| `person_alias` | string | ❌ | Display name if profile cannot be shared | Privacy | N/A |
| `unit_address` | object (ref) | ✅ | Reference to Address schema | Location | N/A |
| `peep_document_id` | string | ✅ | Source PEEP ID | Traceability | N/A |
| `building_ees_document_id` | string | ❌ | Which building EES this aligns to | Context | N/A |
| `language` | string (IETF tag) | ❌ | E.g., `en-GB` | Accessibility | Equality Act 2010 |
| `format` | array (enum) | ❌ | `standard_print`, `large_print`, `easy_read`, `audio`, `braille`, `sign_video`, `high_contrast` | Accessibility | Equality Act 2010 |
| `content` | object | ✅ | Summary, steps, scenarios, waiting place, muster point, notes | Procedural clarity | Residential Regs 2025 (Reg 10) |
| `assistance` | object | ❌ | Needs assistance (boolean), assisters, equipment, contact on alarm | Resource planning | N/A |
| `delivery` | object | ❌ | Delivered by, delivered at, acknowledged by resident | Process tracking | Residential Regs 2025 |
| `reviews` | array of objects (ref) | ❌ | Lifecycle reviews | Continuous improvement | FSO 2005 |
| `extensions` | object | ❌ | Vendor-specific extensions | Extensibility | N/A |

---

## Notation Conventions

- **✅ Required**: field must be present
- **❌ Optional**: field may be omitted
- **enum**: field accepts only predefined values
- **date-time**: ISO 8601 format (e.g., `2025-10-14T12:00:00Z`)
- **date**: ISO 8601 date (e.g., `2025-10-14`)
- **UUID**: UUID v4 format
- **URI**: valid URI format
- **max N**: maximum character length
- **ref**: reference to another schema (via `$ref`)

---

## Change Log

| Date | Version | Change | Rationale |
|------|---------|--------|-----------|
| 2025-10-14 | 1.0.0-beta | Initial data dictionary | Baseline documentation |

---

## Next Steps

- **Review [Mapping to Guidance](mapping_to_guidance.md)** for UK regulation anchors
- **Consult [Privacy & GDPR](privacy_gdpr.md)** for data minimisation and retention guidance
- **Explore the schemas** in `/schemas/`

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

