# Contributing

## How to contribute
1. Open an issue for discussion (use the RFC template for schema changes).
2. For small fixes, submit a PR referencing the issue.
3. For schema changes, include:
   - Updated schema file(s)
   - Updated examples
   - Docs (data dictionary and mapping)
   - Passing `npm run validate`

## Style & Conventions
- JSON Schema draft 2020-12
- `additionalProperties: false`
- Use UK English in descriptions
- Prefer enums with clear definitions
- Dates use `format: date` and datetimes `format: date-time`
- IDs are UUID v4 (`format: uuid`)
- Keep fields minimal; add via extensions if needed

## Commit messages
- Conventional commits recommended: `feat:`, `fix:`, `docs:`, `chore:`

## Code of Conduct
See CODE_OF_CONDUCT.md
