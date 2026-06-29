# Claude Memory & Context — Tyk Release Notes Project

> **How to use this file:** When starting a new Claude session, paste the following instruction into the chat before giving any task:
>
> *"Please read `CLAUDE_MEMORY.md` and `README_and_Instructions.md` in this folder before we start. These contain the accumulated style rules and feedback from previous sessions that you must follow."*

---

## About This Project

This folder is a knowledge repository for writing **Tyk Release Notes** (changelogs and release highlights) across all Tyk components: Gateway, Dashboard, Pump, Sync, Multi Data Center Bridge (MDCB), Tyk Operator, Tyk Charts, Tyk Identity Broker, and Tyk Cloud.

The full workflow, ticket types, formatting templates, and editorial rules are documented in `README_and_Instructions.md`. This file records **accumulated feedback** from real drafting sessions — things Claude got wrong or right that should carry forward permanently.

---

## Accumulated Feedback Rules

These rules were learned from feedback given during previous sessions. Apply all of them without needing to be reminded.

---

### 1. Always call out new config options and configuration changes

When writing any changelog entry or release highlight, explicitly surface any new configuration option, changed config behavior, or new field that a ticket introduces.

- Name the exact option (config file key, env var, API/policy/session field, CLI flag, log field, response header, etc.)
- State its default value
- Describe what it controls and whether it is backward-compatible or opt-in
- Treat new log fields, response headers, and request attributes the same way — they are configuration surface for downstream consumers

**Why:** Customers rely on release notes to know what knobs are now available. Missing a new config option means they don't know the feature exists or how to opt in/out.

---

### 2. Tyk OAS terminology — mandatory naming conventions

Apply these substitutions to every draft before delivering:

| ❌ Never write | ✅ Always write |
|---|---|
| bare `OAS` | `Tyk OAS` |
| `OAS API` / `OAS APIs` | `Tyk OAS API` / `Tyk OAS APIs` |
| `OAS file` / `OAS files` | `OpenAPI document` / `OpenAPI documents` |
| `OpenAPI specification` (meaning a file) | `OpenAPI document` |

**Exception:** Keep `OpenAPI Specification 3.1` (or 3.0) intact when naming the *standard itself*, not a user's document.

**Why:** Brand voice and disambiguation. Bare "OAS" is ambiguous. "OAS file" and "OpenAPI specification" are technically incorrect when referring to a document instance.

---

### 3. Release note style — concise, user-impact focused, no internal detail

**Fixed entries must be short:** aim for 2–4 sentences. Calibrate against existing Fixed entries in `Gateway.md`, `Dashboard.md`, `Sync.md`, etc.

**Lead with user-visible impact, not the cause.** Describe what was broken from the customer's experience and what now works. Skip root-cause analysis.

**Never expose internal implementation details:**
- Internal field names / JSON paths (e.g., `version_data.versions.<name>.expires`)
- Redis key names (e.g., `tyk:node:registration:lock:<sessionID>`)
- Function names, file paths, source files, PR numbers, ticket IDs
- Internal subsystem names that aren't customer-facing
- Step-by-step "what we changed" / before–after code descriptions
- Internal license-type names (use the customer-facing term instead)

**Cut RCA narrative.** Phrases like "the root cause was X", "previously the code did Y, now it does Z" do not belong in a Fixed entry.

**Accordion titles use sentence case, not Title Case.** Only capitalize the first word and proper nouns/product names/acronyms.

- ✅ `<Accordion title='Fix malformed responses from Go plugins returning error status codes'>`
- ❌ `<Accordion title='Fix Malformed Responses from Go Plugins Returning Error Status Codes'>`

Proper nouns keep their casing: Tyk, Tyk Gateway, Tyk Dashboard, Go, Redis, mTLS, TLS, CORS, OAuth, JWT, etc.

**Self-check after drafting:** "Could a customer who has never read the ticket understand what improved for them, in 2–4 sentences?"

---

### 4. Release Highlights open with "This release", not the component name

In **Release Highlights** (not Changelog), the opening subject of a highlight body must be `"This release"`, not `"Tyk Dashboard"`, `"Tyk Gateway"`, etc.

- ✅ *"This release resolves a set of related issues affecting Gateway registration..."*
- ❌ *"Tyk Dashboard resolves a set of related issues affecting Gateway registration..."*

**Why:** The component name is already implied by context. "This release" keeps voice consistent across all components and avoids odd anthropomorphisation.

This rule applies to the body's opening subject only. Titles of highlights can still mention the component name if helpful. Keep component names elsewhere in the body when needed for clarity.

---

### 5. Cross-product examples are valid references

When drafting release notes for any single Tyk component, use examples from **all** other component files as calibration sources — `Gateway.md`, `Dashboard.md`, `Pump.md`, `Sync.md`, MDCB, Operator, Charts, Cloud, etc.

All Tyk products share one consistent voice and structure. Do not limit yourself to same-component examples.

**Why:** Looking only at the named component can miss patterns that are well established elsewhere in the same product family.

---

### 6. Always capitalise Tyk component names

Every Tyk component name must be capitalised whenever it appears in a release note — in titles, body text, accordion labels, everywhere.

| ✅ Always write |
|---|
| Gateway |
| Dashboard |
| Cloud |
| MDCB |
| Operator |
| Pump |
| Sync |
| TIB (Tyk Identity Broker) |

**Examples:**

- ✅ *"...even when the Gateway was registered and running."*
- ❌ *"...even when the gateway was registered and running."*

This applies to all occurrences — singular references ("the Gateway"), plural ("multiple Gateways"), and adjectival uses ("Gateway registration", "Dashboard configuration").

**Why:** These are proper product names, not generic nouns. Lowercase diminishes brand voice and creates inconsistency with how Tyk documents its own products.

---

### 7. Use American English in all release note content

Write all release notes, changelog entries, and Release Highlights in **American English** spelling and conventions.

| ❌ British | ✅ American |
|---|---|
| capitalise / capitalised | capitalize / capitalized |
| anthropomorphisation | anthropomorphization |
| behaviour | behavior |
| centre | center |
| colour | color |
| licence (noun) | license |
| optimise / optimisation | optimize / optimization |
| catalogue | catalog |

**Why:** Tyk's release notes follow American English as the house standard. Mixed spelling conventions read as inconsistent.

---

## Component Files in This Folder

| File | Component |
|---|---|
| `Gateway.md` | Tyk Gateway |
| `Dashboard.md` | Tyk Dashboard |
| `Pump.md` | Tyk Pump |
| `Sync.md` | Tyk Sync |
| `Multi-data-center-bridge.md` | Tyk MDCB |
| `Operator.md` | Tyk Operator |
| `Charts.md` | Tyk Charts |
| `Tyk Identity Broker.md` | Tyk Identity Broker |
| `Portal.md` | Tyk Developer Portal |
| `Cloud.md` | Tyk Cloud |
| `README_and_Instructions.md` | Full workflow, templates, editorial rules |

---

*This file was generated from accumulated session memory on 2026-05-26.*
