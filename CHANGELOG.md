# Changelog — OpenPEEP

All notable changes to the OpenPEEP standard schemas and documentation will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- Initial schema suite (beta):
  - `peep.schema.json` — Personal Emergency Evacuation Plan
  - `pcfra.person.schema.json` — Person-Centred Fire Risk Assessment (Personal)
  - `pcfra.environment.schema.json` — Person-Centred Fire Risk Assessment (Environmental)
  - `common/consent.schema.json` — Consent record (given/refused/withdrawn)
  - `common/review.schema.json` — Review lifecycle block
  - `common/person_profile.schema.json` — Person identity anchor
  - `common/address.schema.json` — Portable addressing schema
  - `ees_building.schema.json` — Emergency Evacuation Statement (Building)
  - `ees_personal.schema.json` — Emergency Evacuation Statement (Personal)

- Initial documentation suite:
  - `README.md` — Mission, scope, status, validation instructions
  - `docs/overview.md` — Concept model and schema relationships
  - `docs/data_dictionary.md` — Authoritative field catalogue
  - `docs/mapping_to_guidance.md` — UK regulation anchors and international portability
  - `docs/privacy_gdpr.md` — Data minimisation, retention, roles, rights
  - `CONTRIBUTING.md` — Contribution guidelines
  - `GOVERNANCE.md` — Stewardship and release process
  - `SECURITY.md` — Vulnerability reporting
  - `CODE_OF_CONDUCT.md` — Community expectations
  - `CHANGELOG.md` — This file

- Example validation suite:
  - Example PCFRA (Personal)
  - Example PCFRA (Environmental)
  - Example PEEP
  - Example Consent records
  - Example EES (Building)
  - Example EES (Personal)

- Validation tooling:
  - AJV validation scripts (JSON Schema Draft 2020-12)
  - `package.json` with test scripts for all schemas

### Changed
- N/A (initial release)

### Deprecated
- N/A (initial release)

### Removed
- N/A (initial release)

### Fixed
- N/A (initial release)

### Security
- N/A (initial release)

---

## Versioning Policy

OpenPEEP follows **Semantic Versioning (SEMVER)**:

- **Major version (X.0.0)**: Breaking changes (e.g., removing required fields, changing types)
- **Minor version (0.X.0)**: Non-breaking additions (e.g., new optional fields, new enum values)
- **Patch version (0.0.X)**: Documentation fixes, clarifications, typos (no schema changes)

### Breaking Changes

A change is **breaking** if it:

- Removes a required field
- Changes a field type (e.g., `string` → `integer`)
- Renames a field
- Changes an enum to exclude previously valid values
- Adds a new required field without a default

### Non-Breaking Changes

A change is **non-breaking** if it:

- Adds a new optional field
- Adds a new enum value
- Relaxes a constraint (e.g., increases `maxLength`)
- Adds a new schema (does not affect existing schemas)
- Updates documentation (no schema change)

### Deprecation Policy

- Deprecated fields are **marked in schema descriptions** with `DEPRECATED: use X instead`
- Deprecated fields are **retained for at least one major version**
- Migration notes are provided in the **CHANGELOG** and **documentation**

---

## Release Process

1. **Proposal**: Open an issue with the `RFC` label for schema changes
2. **Public review**: 14-day public comment period for breaking changes
3. **Approval**: Two maintainers (one domain, one technical) must approve
4. **Implementation**: Update schemas, examples, documentation
5. **Validation**: All schemas and examples must pass `npm run test:all`
6. **Changelog**: Update this file with SEMVER bump and rationale
7. **Tag**: Create a Git tag (e.g., `v1.0.0`)
8. **Release notes**: Publish release notes on GitHub (or equivalent)

---

## Migration Guides

### Migrating from [Future Version X] to [Future Version Y]

*(Reserved for future breaking changes)*

---

## Questions or Feedback?

- Open an issue: [GitHub Issues](https://github.com/OpenPEEP-Foundation/OpenPEEP/issues) *(replace with actual repo URL)*
- Email: standards@openpeep.org *(update as needed)*

---

**OpenPEEP** — *Safe evacuation planning, open by design.*

