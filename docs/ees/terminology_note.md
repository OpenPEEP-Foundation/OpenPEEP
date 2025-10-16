# EES terminology note — why two types?

OpenPEEP uses the umbrella term **Emergency Evacuation Statement (EES)** in line with UK usage,
but implements **two distinct JSON schema types** for clarity and audit separation:

- **EES-Building** — the building-wide evacuation strategy statement (akin to a GEEP-style strategy).
  It summarises the global strategy (e.g., stay put / simultaneous / phased), protected means of escape,
  refuges / evacuation lifts availability, high-level routes, and muster information. It contains **no personal data**.

- **EES-Personal** — an individual’s plain-language instructions derived from their PEEP/RPEEP *and*
  the building’s strategy. It focuses on **what this person should do** in likely scenarios,
  any agreed assistance, and delivery/acknowledgement to the resident. It references, but does not embed,
  medical diagnoses.

This split avoids data confusion (global vs individual), enables clean compliance audits, and preserves
vendor-neutrality while keeping DigiPEEP-specific routing/waypoints out of the open standard.

## Position in the golden thread

- **PCFRA (personal + environmental)** → informs → **PEEP** (individual plan).
- **PEEP** → is distilled into → **EES-Personal** (resident handout).
- **Fire risk assessment & building strategy** → informs → **EES-Building** (global strategy).

