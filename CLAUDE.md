# Writing Tyk Release Notes: instructions for Claude

This is the single instruction file for drafting and reviewing Tyk release notes. It replaces the old `README_and_Instructions.md` and `CLAUDE_MEMORY.md` files. Read it in full before drafting anything, and apply every rule without needing to be reminded.

## Contents

1. [How to work in this folder](#how-to-work-in-this-folder)
2. [Release note anatomy and templates](#release-note-anatomy-and-templates)
3. [Drafting process: Jira, CVE board and codebase](#drafting-process-jira-cve-board-and-codebase)
4. [Drafting workflow (per entry)](#drafting-workflow-per-entry)
5. [Editorial rules](#editorial-rules)
6. [Useful prompts](#useful-prompts)

---

## How to work in this folder

- **Component files** (`Gateway.md`, `Dashboard.md`, `Pump.md`, `Sync.md`, `Multi-data-center-bridge.md`, `Operator.md`, `Charts.md`, `Tyk Identity Broker.md`, `Portal.md`, `Cloud.md`) hold every past release note for that component, grouped by section with versions newest first. They are the calibration target for tone, length and detail (see rule B3: use examples from every component, not only the one you are drafting).
- **New drafts** go straight into the component file. See [Output](#output) for the exact layout.
- **`project-memory.md`** holds background knowledge captured from Claude's memory: Jira quirks, codebase snapshot details, lessons learned, and open initiatives. Read it when a task touches Jira sourcing, codebase checks, or the automation and per-minor-page initiatives.
- **When Joan gives feedback that should apply to future release notes**, add it to [Editorial rules](#editorial-rules) in this file (next free number in the right group, with the date) instead of a separate memory file.
- **Never use the em dash** in anything you write here or in chat (rule B10).

| Component | Description | File |
|---|---|---|
| Tyk Gateway | Core API Gateway functionality | `Gateway.md` |
| Tyk Dashboard | Management UI and API | `Dashboard.md` |
| Tyk Pump | Analytics data processor | `Pump.md` |
| Tyk Sync | Configuration synchronization | `Sync.md` |
| Tyk MDCB | Multi Data Center Bridge, multi-region deployments | `Multi-data-center-bridge.md` |
| Tyk Operator | Kubernetes operator | `Operator.md` |
| Tyk Charts | Helm charts | `Charts.md` |
| Tyk Identity Broker | SSO and identity federation | `Tyk Identity Broker.md` |
| Tyk Developer Portal | Enterprise Developer Portal | `Portal.md` |
| Tyk Cloud (Ara) | SaaS platform | `Cloud.md` |

---

## Release note anatomy and templates

### Changelog entries

Changelog entries are created from individual tickets (or a combination of tickets). Each ticket will be given with its component, and Claude needs to produce a title and a summary for it.

There are four ticket types:

#### Changed
Updates and upgrades.

> **Title:** Updated Golang version to 1.25
> **Summary:** The Tyk Dashboard has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.

#### Added
New features (stories).

> **Title:** OpenTelemetry and Observability Enhancements
> **Summary:** This release brings significant improvements to OpenTelemetry tracing and log correlation capabilities within the Gateway. These enhancements ensure better observability and easier debugging across distributed systems by unifying trace context across all log types:
> - Added the `trace_id` field to Gateway access logs when OpenTelemetry is enabled, matching the `X-Tyk-Trace-Id` response header.
> - Added `trace_id` and `span_id` fields to all request-scoped Gateway application logs (middleware execution, errors, and debugging).
> - Introduced custom trace header configuration (e.g., `X-Correlation-ID`) to support non-standard header names as trace context sources, with three trace propagation modes:
>   - Custom-only (read and write custom headers exclusively)
>   - Hybrid (read custom headers, write standard `traceparent`)
>   - Composite (read custom headers, write both custom and standard formats)
> - Implemented automatic fallback to standard W3C propagators when custom trace headers are missing or invalid.

#### Fixed
Bug fixes.

> **Title:** Fixed Intermittent NewRelic Tracing
> **Summary:** Resolved an issue where NewRelic OpenTracing integration worked inconsistently in Tyk Gateway. The Gateway now properly mounts NewRelic middleware on all routers, including reused ones, with thread-safe duplicate prevention and improved memory management during router swaps. This fix ensures consistent NewRelic APM visibility across all API calls and gateway versions, supporting both legacy NewRelic configurations and newer OpenTelemetry collector setups.

#### Security Fixes
Typically CVE fixes.

> **Title:** CVE fixed
> **Summary:** Addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
> - CVE-2025-15467
> - CVE-2025-69419
> - CVE-2025-61726
> - CVE-2025-61728
> - CVE-2025-68121

---

### Release Highlights

Release Highlights are a separate part of the release notes from the changelog. The format varies by component: for most components it's a single summary paragraph; for others (especially the latest releases) it goes into more depth.

#### Short format example

> Tyk MDCB has been updated to Go 1.25 for enhanced security and performance. This release also addresses some CVEs and exposes more controls to configure the transfer of aggregated traffic logs to the Control Plane data store. For a comprehensive list of changes, please refer to the detailed changelog.

#### Long format example

> **OpenAPI Specification 3.1 is now supported**
> In this release, we are delighted to bring initial support for OAS 3.1, covering:
> - Import and validation of OpenAPI 3.1 descriptions using Tyk Dashboard to create Tyk OAS APIs
> - OAS 3.1 features:
>   - Full JSON Schema Support and `$schema` keyword
>   - The single `example` keyword is deprecated in OAS 3.1
>   - `type` can be an array
>   - `exclusiveMinimum` and `exclusiveMaximum` keywords
>
> We do not yet have support for all new features. For more details, see the documentation.
>
> **Simplified Management of Versioned Tyk OAS APIs**
>
> Managing versioned API hierarchies is now much easier and safer. We've introduced the ability to promote any child API version to become the new base (routing) API directly from the Dashboard UI, seamlessly transferring routing configurations without service disruption.
>
> Additionally, we've improved the experience of deleting a base API. Instead of manually cleaning up orphaned child APIs, you are now presented with clear, intelligent options: promote a child to the new base, delete all associated child APIs together, or leave them as independent APIs. These enhancements eliminate manual cleanup work and give administrators full control over their API lifecycles.
>
> **Enhanced Security with Client Certificate-Token Binding**
> To provide an additional layer of security for your APIs, we've introduced Dashboard support for Client Certificate-Token Binding. This feature allows you to form a strict binding association between an Auth Token issued to a client and their specific client certificate.
> By ensuring that a token can be used only with its bound certificate, you can significantly reduce the risk of token theft or misuse. You can now easily manage these bindings directly from the Dashboard when creating or modifying keys, with full support for certificate rotation scenarios by allowing multiple certificates to be bound to a single key.
>
> For a comprehensive list of changes, please refer to the detailed changelog.

#### Choosing a format

When asked for Release Highlights, Claude should ask whether the user wants the **short format** or the **long format** shown above.


---

## Drafting process: Jira, CVE board and codebase

As of 2026-07-28, release notes for a given version are no longer drafted purely from tickets pasted into chat. Claude pulls the tickets directly from Jira, pulls CVEs directly from the CVE board, cross-checks against the public Tyk codebase where possible, and adds the draft to the component file for review. This section documents that process so it can be repeated consistently for every release. The [Drafting workflow](#drafting-workflow-per-entry) section below still applies to how each individual entry is written once the source tickets are identified.

### Sourcing tickets from Jira
1. Connect to Jira via the Atlassian Rovo MCP connector (cloud site `tyktech.atlassian.net`).
2. Pull every ticket tagged with the release's Fix Version. **Jira fix versions use a `"Tyk X.Y.Z"` prefix** (e.g. `"Tyk 5.13.2"`), not the bare version number - `fixVersion = "5.13.2"` will return nothing.
3. For each ticket, check:
   - **Include in Changelog** (custom field `customfield_10335`) - a `Yes` value with the exact label `"Include in changelog (Yes – Included in external release notes No – Not included in external release notes internal information only)"`. If this is explicitly `No`, exclude the ticket. If it's unset (`null`), don't assume `No` - check whether the ticket is superseded by another ticket instead (see below) before excluding it.
   - **Breaking Change** (custom field `customfield_10684`, labelled `"Breaking Change "` with a trailing space) - a `Yes` value means the change must appear in the Breaking Changes section as well as its own Changelog entry.
   - **Components** - determines which component release notes page(s) the entry belongs to. A ticket tagged with more than one component (e.g. Gateway + Pump) gets the same entry mirrored in both component files.
   - **Ticket Type** (Bug vs. Story) - Bug → candidate for Fixed, Story → candidate for Added, unless it's actually a CVE-driven Story, in which case it belongs in Security Fixes instead (see below).
4. Watch for **superseded/merged tickets**: Jira sometimes links an older story as "is implemented by" another ticket (a "Polaris work item link"). If a ticket has no Include-in-Changelog value set and is linked this way, it's very likely folded into the implementing ticket's entry rather than needing (or getting) its own.
5. Watch for **epic/parent groupings** the same way - via issue links, not always a formal epic field.

### Sourcing CVEs from the CVE board
CVEs are **not** pulled from a component's tickets directly - they live in a separate Jira project, key `CVE` ("CVE Repository"). Use this exact filter every time a Security Fixes section is needed for any release and component:

- **Project** = `CVE`
- **Affected Component** (custom field `customfield_11122`, multi-select) = the target component, e.g. `"Tyk Dashboard"` or `"Tyk Gateway"`
- **Fix Version** (standard `fixVersion` field) = the target release, e.g. `"Tyk 5.13.2"`
- **CVE Severity** (custom field `customfield_11053`) - **exclude `"Low"`**; only `Critical`, `High`, and `Medium` are shown in release notes.

As JQL, this looks like:

```
project = CVE AND cf[11122] = "Tyk Dashboard" AND fixVersion = "Tyk 5.13.2" AND cf[11053] != "Low" ORDER BY key ASC
```

**Important implementation note:** the CVE Severity option values have inconsistent trailing whitespace in Jira (e.g. `"High "`, `"Medium "` vs. `"Critical"` with no space), which breaks an inclusive `IN ("Critical","High","Medium")` clause - some valid entries silently fail to match. Always use the **exclusion** form (`cf[11053] != "Low"`) instead of an inclusive list, since `"Low"` itself doesn't have this whitespace problem.

Each CVE ticket's description contains the CVE/GHSA ID, an NVD or OSV link, CVSS score, and severity - use the ID and link directly rather than re-deriving them. Link CVE-prefixed IDs to `https://nvd.nist.gov/vuln/detail/<CVE-ID>` and GHSA-prefixed IDs to `https://osv.dev/vulnerability/<GHSA-ID>`, matching the existing convention in the component files.

If a specific CVE fix is significant enough to warrant its own named accordion (for example, a library migration tied to one CVE, like moving off an unmaintained `nosurf` fork), give it its own entry above the generic "Resolved CVEs" bulk-list accordion - don't bury a notable fix inside the anonymous batch.

**Exception - Tyk Cloud:** Tyk Cloud release notes do **not** list individual CVEs in a Security Fixes section. Instead, mention it briefly in the Release Highlights paragraph, e.g. *"Addressed several CVEs within dependencies."* This exception applies to Tyk Cloud only - every other component (Gateway, Dashboard, Pump, Sync, MDCB, Operator, Portal, TIB, Charts) continues to show the filtered CVE list in a dedicated Security Fixes section as normal.

### Codebase cross-check
**Primary source: the locally mounted "Tyk Code Claude" folder.** Joan has connected a folder containing extracted snapshots of the Tyk repos, giving direct file access every session with no cloning, no network restrictions, and - critically - no public/private repo problem. Always check this folder first before falling back to cloning from GitHub.

Current contents and their component mapping (each is a zip extracted into a folder of the same name, so the real source sits one level deeper, e.g. `tyk-master/tyk-master/`):

| Folder | Component | Notes |
|---|---|---|
| `tyk-master` | Tyk Gateway | Full Go source, `go.mod` present |
| `tyk-analytics-ui-master` | Tyk Dashboard - **frontend/UI only** | No `go.mod` - this is the `webclient` layer only (JS, AngularJS, `app/pages/...`). The Dashboard **backend** (Go handlers under `dashboard/*.go`, referenced by tickets like the ones touching `dashboard/api_users.go` or `internal/unzip/unzip.go`) is still not available anywhere. Only UI/webclient-side ticket claims can be verified against this folder - backend-side Dashboard claims still can't be verified and should stay flagged as ticket-text-only. |
| `tyk-operator-internal-master` | Tyk Operator | This is the internal/private development repo, not the public OSS mirror |
| `tyk-sink-master` | Tyk Sync | Repo's actual name is `tyk-sink` |
| `tyk-pump-master` | Tyk Pump | |
| `portal-master` | Tyk Developer Portal | |
| `tyk-identity-broker-master` | Tyk Identity Broker (TIB) | |
| `tyk-charts-main` | Tyk Charts | Helm charts, no `go.mod` (expected - not a Go project) |

**Still not available:** the Tyk Dashboard backend (Go) and Tyk Cloud (Ara) have no source in this folder. Backend Dashboard tickets and any Cloud ticket stay ticket-text-only until/unless those are added too.

For each ticket, grep the relevant folder for the function names, config keys, or log strings mentioned, to confirm a fix has actually merged/matches the described behavior before treating it as final - especially anything touching a config name, default value, or exact behavior description. A ticket still "In Code Review" / "In Dev" / "In Refinement" in Jira may simply not be reflected in this snapshot yet (it's a point-in-time export, not a live clone); that's expected, not an error, but should be flagged back for a final check closer to release, and re-confirmed once Joan refreshes the folder with newer snapshots.

**Fallback: cloning from GitHub.** If a repo isn't in the local folder, the public ones (`tyk`, `tyk-pump`, `tyk-sync-internal` confirmed clonable) can still be cloned directly (`git clone https://github.com/TykTechnologies/<repo>.git`) from the sandbox. Note that `raw.githubusercontent.com` is blocked by the sandbox's network allowlist, but `github.com` itself is allowed - use `git clone`/`git ls-remote` over the smart HTTP protocol rather than raw file fetches. Private repos (like the Dashboard backend, `tyk-analytics`) still can't be cloned this way - that's exactly the gap the local folder is meant to close, so ask Joan to add a repo to the folder rather than assuming it's unreachable forever.

### Output

Draft each release directly into its component file (for example `Dashboard.md`). Do not create separate per-release files.

- Add a `### X.Y.Z` block under each section that has content for that release: Breaking Changes, Release Highlights, Dependencies, Deprecations, Upgrade Instructions, Downloads and Change Log. Keep versions newest first within each section.
- Inside a version block, sub-headings move one level down from the published page: `#### Compatibility Matrix for Tyk Components`, `#### 3rd Party Dependencies & Tools`, and so on.
- The Change Log block starts with `#### Changelog` and the `<a id="Changelog-vX.Y.Z" data-scroll-offset></a>` anchor, followed by `##### Added`, `##### Changed`, `##### Fixed` and `##### Security Fixes`, each holding the usual `<AccordionGroup>`/`<Accordion>` MDX.
- Skip a section for that version when it would only say "There are no breaking changes in this release." or "There are no deprecations in this release." When Joan needs the paste-ready page for the tyk-docs PR, assemble it from the version blocks in the published order (Release Highlights, Breaking Changes, Dependencies, Deprecations, Upgrade Instructions, Downloads, Changelog) and add those standard lines for the empty sections.
- Mark anything that can't be confirmed (compatibility matrix specifics, published artifact links, 3rd-party dependency versions with no corresponding ticket) with **[TO CONFIRM]** rather than inventing values.
- The review notes that used to be the "Notes for Joan" block (excluded tickets and why, merged or superseded tickets, tickets still in progress, what could or couldn't be verified against source) go in the chat reply, not in the component file. Wherever a rule below mentions "Notes for Joan", it means these review notes.

**After a release is published**, the published page is the source of truth. Replace that version's draft blocks in the component file with the published text word for word, changing only heading levels to fit the file. Add a `**Release date:** ...` line at the top of its Release Highlights block, and update the file's header lines (which versions are synced, which are still unpublished drafts). The exact MDX is in the `TykTechnologies/tyk-docs` repo under `developer-support/release-notes/<page>.mdx`, where the pages are `gateway`, `dashboard`, `pump`, `sync`, `mdcb`, `operator`, `helm-chart`, `tib`, `portal` and `cloud`. The repo is public, so a sparse `git clone` works; `tyk.io/docs` pages fetched through the web only come back as summaries.

For the Dependencies section, always check 3rd-party dependency versions against the source of truth: [Distro OS, KV Store, and Dependency Versions Being Tested In Tyk Pipeline](https://tyktech.atlassian.net/wiki/spaces/CVBGF/pages/4499079175/Distro+OS+KV+Store+and+Dependency+Versions+Being+Tested+In+Tyk+Pipeline) (Confluence page ID `4499079175`). See rule C6.


---

## Drafting workflow (per entry)

When drafting a release note from a ticket, follow this sequence:

1. **Classify the ticket** into one of: `Changed` (updates/upgrades), `Added` (new features), `Fixed` (bug fixes), or `Security Fixes` (CVE batches).
2. **Confirm the component(s).** If the fix applies to more than one component that ships together (e.g., Gateway + Dashboard), the same entry is mirrored in both component files.
3. **Scan for new configuration surface** - config options, env vars, API/policy/session fields, log fields, response headers - and ensure each is named in the draft.
4. **Scan for license-scope caveats** - if the fix only fully resolves the issue for a subset of license types, the scope must be in the title or opening sentence.
5. **Draft the title** - short, action-oriented, customer-visible language. Existing entries in this repo are the calibration target.
6. **Draft the summary** - 2–4 sentences for Fixed entries; longer is acceptable for Added entries if the feature genuinely requires it. Apply all editorial rules above.
7. **Self-check before delivering:**
   - Could a customer who has never read the ticket understand what improved for them, in 2–4 sentences?
   - Is any internal field name, file path, function name, or PR number still present?
   - Is "Tyk OAS API" used (never "OAS API")?
   - Is "OpenAPI document" used where appropriate?
   - Are new config options and new customer-facing fields named?
   - Is the license scope clear if it matters?
   - Are related fixes consolidated rather than split?
   - Does the accordion have its source ticket number(s) as an `{/* TT-XXX */}` comment on the line after the title? (see rule B4)


---

## Editorial rules

These rules come from real feedback on Tyk release notes. The core rules apply to every entry; groups A to D hold the more specific rules, each with the date it was added and why.

### Core rules: terminology
- **Always write "Tyk OAS API"** (or "Tyk OAS APIs"). Never abbreviate to "OAS API" - the "Tyk" prefix disambiguates Tyk's product from generic OpenAPI/OAS terminology.
- **"OpenAPI document" vs. "OpenAPI Specification":**
  - Use **"OpenAPI document"** when referring to a YAML/JSON file the user provides, imports, or validates.
  - Use **"OpenAPI Specification"** (capitalized) only when naming the standard itself (e.g., "OpenAPI Specification 3.1").
  - Do not write "OpenAPI specification" or "OAS specification" to mean a file - that wording is incorrect.
- **Use customer-facing license names**: "unlimited node license" and "limited node license". Do not surface internal names such as "Infinite Nodes" in release notes.

### Core rules: license scope
- **License-scope caveats.** If a fix only fully applies to certain license types (e.g., unlimited node licenses), state the scope in the title or the opening sentence so customers can immediately tell whether it affects them. Set expectations for any planned follow-up.

The other "what to call out" and "what not to expose" rules are in C1 and B1. Ticket traceability is B4.

### Core rules: component grouping
- **Do not split shared fixes by component.** When a single fix spans Gateway and Dashboard (or any pair of components that ship together), describe it as one unified change from the customer's point of view. Do not write "the Dashboard now does X and the Gateway now does Y" - describe the resulting behavior. The same entry simply appears in both component release notes with the same text.
- **Do not add "requires upgrading both X and Y" warnings.** Tyk customers upgrade Gateway and Dashboard at the same time; mixed-version caveats clutter the entry and are not actionable.
- **Bundle related small UI/UX items into one accordion** when the individual entries don't warrant standalone changelog items. Use a section title like "Dashboard UI enhancements" with one bold sub-heading per change. Keep substantive features (anything with API or behavioral surface beyond the UI) as their own dedicated entry.
- **Consolidate critical-incident fixes into one entry.** If a single customer incident produced multiple tickets across components (e.g., a registration storm split across lock contention, retry handling, and Redis load), prefer one consolidated changelog entry that states the high-level problem, briefly notes the cause, and describes the improved behavior - not multiple accordions that read like a postmortem.

### A. Terminology & Language

#### A1. Tyk OAS terminology - mandatory naming conventions

Apply these substitutions to every draft before delivering:

| ❌ Never write | ✅ Always write |
|---|---|
| bare `OAS` | `Tyk OAS` |
| `OAS API` / `OAS APIs` | `Tyk OAS API` / `Tyk OAS APIs` |
| `OAS file` / `OAS files` | `OpenAPI document` / `OpenAPI documents` |
| `OpenAPI specification` (meaning a file) | `OpenAPI document` |

**Exception:** Keep `OpenAPI Specification 3.1` (or 3.0) intact when naming the *standard itself*, not a user's document.

**Why:** Brand voice and disambiguation. Bare "OAS" is ambiguous. "OAS file" and "OpenAPI specification" are technically incorrect when referring to a document instance.

#### A2. Use American English in all release note content

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

#### A3. Always capitalize Tyk component names

Every Tyk component name must be capitalized whenever it appears in a release note - in titles, body text, accordion labels, everywhere.

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

This applies to all occurrences - singular references ("the Gateway"), plural ("multiple Gateways"), and adjectival uses ("Gateway registration", "Dashboard configuration").

**Why:** These are proper product names, not generic nouns. Lowercase diminishes brand voice and creates inconsistency with how Tyk documents its own products.

#### A4. Capitalize Tyk Developer Portal resource/entity names - Product, Plan, Catalogue, etc. (added 2026-08-19)

When writing Portal release notes, capitalize the names of Portal-specific resource types whenever they refer to that Portal concept/entity, not the generic English word. Confirmed so far: **Product**, **Plan**, **Catalogue**.

| ❌ Never write | ✅ Always write |
|---|---|
| a product | a Product |
| products and plans | Products and Plans |
| the catalogue | the Catalogue |

**Examples:**
- ✅ *"MCP Proxy APIs and their Products now sync into the Portal Catalogue."*
- ❌ *"MCP Proxy APIs and their products now sync into the Portal catalogue."*

**Note on spelling:** Joan's instruction used "Catalogue" (not "Catalog"). Treat **Catalogue** as the correct spelling for this Portal entity name specifically - a proper-noun exception to the American-English rule (A2), the same way A1 carves out an exception for "OpenAPI Specification" as a standard name. Don't silently "correct" it to "Catalog" under A2.

**Applies to:** Portal release notes only (`Portal.md`), for now. Flag to Joan if this list of Portal entity names should extend to any not yet confirmed (e.g. "Application", "Documentation") rather than guessing.

**Why:** Explicit instruction from Joan (2026-08-19) - the pre-existing Portal 1.19.0 draft used lowercase "product"/"catalogue" throughout when referring to these named Portal resources/UI concepts, which reads as generic nouns rather than the product's own proper terms.

#### A5. Say "Tyk Operator" -- never bare "Operator" -- when referring to the Tyk Kubernetes Operator component (added 2026-09-16)

Explicit instruction from Joan (2026-09-16): always write **"Tyk Operator"** (not bare "Operator") when referring to the Tyk Operator component -- in release notes, changelog entries, accordion titles, and any other content. This tightens A3's existing "Operator" example: the bare word is now ambiguous, because "operator" has started being used elsewhere (e.g. in Portal/API contexts) to mean "a user" / "an end user", not the Kubernetes Operator component.

| ❌ Never write | ✅ Always write |
|---|---|
| The Operator now applies ownership from `OperatorContext`... | The Tyk Operator now applies ownership from `OperatorContext`... |
| Operator support for Tyk OAS APIs | Tyk Operator support for Tyk OAS APIs |

This applies everywhere the component is named -- titles, body text, accordion titles, changelog descriptions -- the same scope as A3. Note: this only governs the *component name*; other identifiers that happen to contain the word (e.g. `OperatorContext`, a CRD/field name) are left as-is, since those are literal API/code identifiers, not prose references to the component.

**Why:** Explicit instruction from Joan (2026-09-16) -- "operator" now means "user" in some contexts at Tyk, so dropping the "Tyk" prefix creates ambiguity about whether a release note is talking about the Kubernetes Operator component or an end user.

#### A6. Tyk Operator feature names: "Tyk OAuth 2.0 client management" and "TLS Certificates" (added 2026-09-23)

Explicit corrections from Joan on the Tyk Operator 1.5.0 release notes (2026-09-23):

| ❌ Never write | ✅ Always write |
|---|---|
| OAuth 2.0 client management | Tyk OAuth 2.0 client management |
| TLS certificates | TLS Certificates |

- **"Tyk OAuth 2.0 client management"** -- always prefix with "Tyk" when naming this Tyk Operator capability, so it's clear it manages Tyk's own OAuth 2.0 clients (not a generic OAuth feature).
- **"TLS Certificates"** -- capitalize "Certificates" when referring to this Tyk Operator resource/feature. Treat it as a proper feature name, so it keeps this casing even in sentence-case accordion titles (B-section sentence-case rule doesn't lowercase it).

**Applies to:** Tyk Operator release notes (`Operator.md`) -- titles, Highlights, body text, accordion titles, changelog entries. Generic prose references to TLS certificates in other components (e.g. Gateway "an expired TLS certificate") are unchanged unless Joan extends this.

**Why:** Explicit instruction from Joan (2026-09-23) while reviewing Tyk Operator 1.5.0.

#### A7. Capitalize asset names such as "Fragment" (added 2026-10-27)

Assets like Fragments are proper Tyk entity names, so capitalize them: "Fragment", "Fragments", never "fragment".

- ✅ *"If an API references a Fragment that is missing, or a Fragment would make the API invalid..."*
- ❌ *"If an API references a fragment that is missing..."*

**Why:** Explicit instruction from Joan (2026-10-27).

#### A8. Always prefix component names with "Tyk" (added 2026-10-27)

Write the full component name with the Tyk prefix, never the bare name: "Tyk Gateway", "Tyk Dashboard", "Tyk MDCB", "Tyk Pump" (same pattern as A5 for Tyk Operator).

- ✅ *"Tyk Gateway, Tyk Dashboard, Tyk MDCB and Tyk Pump can now all be autoscaled..."*
- ❌ *"Gateway, Dashboard, MDCB, and Pump can now all be autoscaled..."*
- ✅ *"...the Tyk Pump chart...", "...the existing Tyk MDCB autoscaling support..."*

**Why:** Explicit instruction from Joan (2026-10-27), reviewing Tyk Charts / MDCB release notes.

---

### B. Style, Tone & Structure

#### B1. Release note style - concise, user-impact focused, no internal detail

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

#### B4. Every Changelog accordion carries its ticket number as an invisible comment

Confirmed 2026-07-28 (Joan: "that's great, keep doing this") - every Changelog `<Accordion>` (Changed/Added/Fixed) gets its source ticket number(s) as an MDX comment `{/* TT-431 */}` on the line right after the opening `<Accordion title='...'>` tag. Multiple source tickets are comma-separated on one line: `{/* TT-9550, TT-17641 */}`.

This is invisible to customers (MDX strips the comment on render) - it exists so any entry can be traced straight back to its Jira ticket(s) without re-reading the prose. Applies to every Changelog accordion without exception, except the bulk "Resolved CVEs" Security Fixes accordion (CVEs come from the CVE board, not one ticket) - a named CVE fix tied to a specific ticket (e.g. a library migration) still gets its ticket comment. Breaking Changes and Release Highlights are prose, not accordions, so this doesn't apply there.

**Why:** Makes it much faster to navigate a changelog and identify/update a specific entry later, without hunting through Jira to match prose back to a ticket.

#### B2. Release Highlights open with "This release", not the component name

In **Release Highlights** (not Changelog), the opening subject of a highlight body must be `"This release"`, not `"Tyk Dashboard"`, `"Tyk Gateway"`, etc.

- ✅ *"This release resolves a set of related issues affecting Gateway registration..."*
- ❌ *"Tyk Dashboard resolves a set of related issues affecting Gateway registration..."*

**Why:** The component name is already implied by context. "This release" keeps voice consistent across all components and avoids odd anthropomorphization.

This rule applies to the body's opening subject only. Titles of highlights can still mention the component name if helpful. Keep component names elsewhere in the body when needed for clarity.

#### B3. Cross-product examples are valid references

When drafting release notes for any single Tyk component, use examples from **all** other component files as calibration sources - `Gateway.md`, `Dashboard.md`, `Pump.md`, `Sync.md`, MDCB, Operator, Charts, Cloud, etc.

All Tyk products share one consistent voice and structure. Do not limit yourself to same-component examples.

**Why:** Looking only at the named component can miss patterns that are well established elsewhere in the same product family.

#### B5. Patch releases - Release Highlights must NOT list individual bug fixes (added 2026-08-20)

For **patch releases** (e.g., an LTS point release like 5.13.2), the Release Highlights section should not enumerate or summarize the individual bug fixes shipped in that patch. Instead, use one short, generic sentence, e.g.:

> "This patch implements several changes and fixes to improve the stability and correct the behavior of the Dashboard."

The full list of individual fixes still belongs in the **Changelog** as normal (Fixed accordions, per B1/B4) - this rule only changes what goes in Highlights, not what goes in the Changelog.

**Major/minor releases are different:** Release Highlights are most valuable when a release introduces new features - that's when a quick overview of the different features before the Changelog helps the reader. Patch releases are typically fixes-only, so a detailed Highlights section there would just duplicate the Changelog.

**Why:** Explicit instruction from Joan (2026-08-20) - patch-release Release Highlights should not list the bug fixes; keep Highlights to one generic sentence for patches and let the Changelog carry the fix detail.

#### B6. Dashboard release notes never mention Go plugins (reversed 2026-10-14)

Explicit instruction from Joan (2026-10-14), replacing the 2026-08-20 version of this rule: Dashboard release notes must not mention Go plugins, for example the 3rd Party Dependencies comment "[Go plugins](/api-management/plugins/golang) must be built using Go 1.26". Go plugins are Gateway artifacts and must match the Gateway's Go version. In the Dashboard dependencies table, use a plain comment such as "Tyk Dashboard is built with Go 1.26".

#### B7. Prefer multiple short paragraphs over one dense paragraph in entry/ticket descriptions (added 2026-08-20)

Joan's feedback: Claude tends to default to writing an entry's description (a Changelog accordion body, a Release Highlight body, a Jira ticket/comment description, etc.) as a single dense paragraph. Where the content has more than one distinct idea, split it into multiple shorter paragraphs instead - it reads much more easily than one block of text.

**How to apply:** When an entry naturally covers more than one idea - e.g. what changed, why it matters, how to enable/configure it, or a follow-up caveat - put each idea in its own paragraph rather than running them together. This applies generally across the kinds of descriptions Claude writes in this project: Changelog accordions, Release Highlights bodies, and any Jira ticket/comment text Claude drafts. A short Fixed entry that's genuinely just one idea in 2-4 sentences (per B1) doesn't need to be forced into multiple paragraphs - use judgment based on how much the entry actually has to say, not a fixed rule to always split.

**Why:** Explicit instruction from Joan (2026-08-20) - a single dense paragraph is harder for the reader to scan than clearly separated paragraphs.

#### B8. Tyk Charts - Release Highlights must state the default component versions it installs (added 2026-09-15)

For **every** Tyk Charts release (major, minor, or patch), the Release Highlights section must mention which Tyk component release the chart installs by default - i.e., tie the Charts version to the Tyk LTS (or latest) release it bundles. Example phrasing:

> "This version installs the most recent Tyk LTS release [5.13.2](/developer-support/release-notes/dashboard#5-13-2-release-notes) but includes enhancements to support features available up to Tyk 5.15.0. You must modify the charts to install the desired version if not using LTS."

Link to the relevant Dashboard (or Gateway) release notes anchor for the LTS version, and name the latest Tyk version whose features the chart supports. **Do not** use the older, shorter "This version aligns with the most recent Tyk LTS release X" phrasing - it doesn't tell readers the chart supports newer (non-LTS) features or that they must override the version to get them. Place it as its own paragraph in Release Highlights, before the "For a comprehensive list of changes..." line.

**Why:** Explicit instruction from Joan (2026-09-15) - standing rule for every Tyk Charts release note. Wording refined 2026-09-23: must say the chart *installs* the LTS by default, *supports* features up to the latest release, and that users must modify the charts to install a non-LTS version.

#### B9. Security-related Fixed entries - redact exploit-enabling detail (added 2026-09-18)

For any Fixed entry describing a security-related bug (a vulnerability found and fixed internally, not a CVE pulled from the CVE board - this is broader than, and separate from, D2/D3's CVE-list sourcing/exclusion rules), do not describe the underlying flaw in enough detail to make it easier for an attacker to identify or target installations still running a vulnerable version.

This is distinct from B1's general "no internal implementation detail" rule - B1 is about not leaking internal field/Redis-key/function names; this rule is specifically about not overexposing exploit mechanics, even when the wording itself is otherwise customer-facing and readable.

Avoid, in a security-related Fixed entry:
- Naming the exact configuration condition that triggers the vulnerability (e.g. a specific config flag/value combination like `strip_auth_data: false`)
- Spelling out precisely how the flaw could be exploited or what an attacker could obtain, step by step
- Naming every specific execution mode/code path affected, when that level of specificity would help an attacker scope which installations are vulnerable

Prefer a shorter, higher-level description that still tells customers a real issue was fixed and gives them a clear reason to upgrade - without handing over a roadmap for exploiting unpatched instances.

**Confirmed example (Gateway 5.15.0, flagged by Joan 2026-09-18):**

❌ Too detailed: the first draft named the triggering configuration flag, listed every affected GraphQL execution mode, and explained how one caller's credentials and responses could reach another caller. The full draft text is intentionally not kept here, because this file is shared publicly.

✅ Preferred instead - still conveys real impact and urgency, without the exploit roadmap:

> "Improved request context isolation in federated GraphQL supergraph execution. Fixed an issue where concurrent requests with the same query shape could incorrectly share cached responses and upstream headers, instead of being handled independently per caller."

**Why:** Explicit instruction from Joan (2026-09-18) - the detailed version names the exact config flag, every affected execution mode, and the underlying mechanism, which makes it materially easier for an attacker to identify and target Gateways still running a vulnerable version. Security-related Fixed entries should still tell customers something real was fixed and give them a reason to upgrade, but must not double as a blueprint for exploiting unpatched instances.

#### B10. Never use the em dash (added 2026-10-13)

Explicit instruction from Joan (2026-10-13): never write the em dash character (the long dash) in any text: release notes, Changelog accordions, Release Highlights, accordion titles, Notes for Joan, Jira text, or chat replies. Rephrase with a colon, comma, parentheses, or separate sentences instead.

| Avoid | Write instead |
|---|---|
| API Fragments (em dash) one accordion | API Fragments: one accordion |
| ...invalid in YAML view (em dash) for example, a path parameter with `required: true` (em dash) preventing... | ...invalid in YAML view (for example, a path parameter with `required: true`), preventing... |

**How to apply:** before delivering any draft, search it for the em dash character and rewrite every occurrence.

#### B11. Gateway release notes never mention TIB or EDP (added 2026-10-13)

Explicit instruction from Joan (2026-10-13): Gateway release notes must not mention TIB (Tyk Identity Broker) or EDP (Enterprise Developer Portal) anywhere. This includes the Compatibility Matrix (no "TIB" or "EDP" rows) and any [TO CONFIRM] or Notes text in the Gateway file. Dashboard release notes can still list them.

#### B12. Release readiness tickets never become release-note content (added 2026-10-14)

Explicit instruction from Joan (2026-10-14): release readiness / release prep tickets (for example "[R5 Prep] Tyk Portal 1.20.0 Release readiness") are internal. Never create a Changelog accordion or a Highlights mention from them, even when the ticket lists a version bump such as the embedded Tyk Identity Broker version.

Example to avoid (Portal 1.20.0): an "Updated embedded Tyk Identity Broker to version 1.8.1" accordion with `{/* TT-18592 */}`, where TT-18592 is the release readiness ticket. Version facts from these tickets can still inform the Dependencies section or Notes for Joan.

#### B13. Keep entries about their own component, and make breaking-change pointers specific (added 2026-10-14)

Explicit feedback from Joan (2026-10-14):

- A component's entry describes only that component. Example: the Dashboard CSRF fix (TT-18522) must not say it also applies to Developer Portal sign-up and login. Preferred wording: "If you serve the Dashboard over plain HTTP, you can set the new configuration option `security.csrf_allow_missing_origin` to `true` (default `false`). This lets browser requests without origin information through the origin check, while the CSRF token is still validated. CSRF failure log entries now include more detail to help with troubleshooting."
- Don't end a Changelog entry with a generic "This is a breaking change. See Breaking Changes." Say what may be affected, for example: "This may affect existing API definitions and webhooks. See [Breaking Changes](#breaking-changes-5.13.4) for details."

#### B14. No summary paragraph above titled Release Highlights entries (added 2026-10-27)

Explicit feedback from Joan (2026-10-27): when Release Highlights uses titled entries (the long format, typical for latest minor releases such as Tyk 5.16.0), do not open the section with a summary line such as "This release introduces API Fragments, OpenTelemetry metrics for Tyk Dashboard, and support for the MCP 2026-07-28 protocol." The section starts directly with the first bold title, for example `**Introducing API Fragments: reusable building blocks for your Tyk OAS APIs**`.

The closing "This release also..." paragraph (other additions, fixes, CVEs, Breaking Changes pointer, changelog link) stays at the end as normal. A single summary paragraph is only used when Highlights has no titled entries (the short format, e.g. patch releases or small minor releases like Pump 1.18.0 or Sync 2.3.0).

---

### C. Accuracy & Verification

#### C1. Always call out new config options and configuration changes

When writing any changelog entry or release highlight, explicitly surface any new configuration option, changed config behavior, or new field that a ticket introduces.

- Name the exact option (config file key, env var, API/policy/session field, CLI flag, log field, response header, etc.)
- State its default value
- Describe what it controls and whether it is backward-compatible or opt-in
- Treat new log fields, response headers, and request attributes the same way - they are configuration surface for downstream consumers

**Why:** Customers rely on release notes to know what knobs are now available. Missing a new config option means they don't know the feature exists or how to opt in/out.

#### C2. Verify env var/config names against engineering source, not just the ticket text

Ticket descriptions sometimes use placeholder or outdated env var notation that doesn't match what engineering actually shipped. Before publishing, check for a reviewer/engineering comment confirming the real name(s) - tickets can introduce several related env vars under one naming convention even when the ticket body only calls out one.

**Example:** a ticket mentioned a single `TYK_OPERATOR_HTTP_MAX_IDLE_CONNS_PER_HOST` var, but engineering actually shipped six vars under the `TYK_OPERATOR_HTTPCLIENTCONFIG_*` prefix (`MAXIDLECONNS`, `MAXIDLECONNSPERHOST`, `MAXCONNSPERHOST`, `IDLECONNTIMEOUT`, `TLSHANDSHAKETIMEOUT`, `TIMEOUT`).

**When exact default values for each new var aren't confirmed, don't invent them.** Name the vars, state that each ships with a sensible built-in default, and - if the ticket/reviewer indicates the knob is for edge-case tuning (e.g., infrastructure constraints like file descriptor exhaustion) rather than routine use - say so explicitly and recommend leaving defaults unless that constraint applies.

#### C3. Every item named in Release Highlights must also appear in the Changelog (and Deprecations) - highlights and changelog must never diverge

If Release Highlights mentions a change, that exact change must also have its own entry in the corresponding Changelog section (Added/Changed/Fixed) - not just prose in Highlights. Likewise, if Release Highlights says something is deprecated, that deprecation must also have its own entry in the `#### Deprecations` section. Never let something named in Highlights exist only in Highlights.

**Example (Pump 1.17.0, flagged by Joan 2026-08-19):** an earlier draft's Release Highlights paragraph mentioned both the new secret-store `kv://` / `$kv{}` support and the resulting deprecation of the legacy `PMP_MONGO*` environment variable format - but the deprecation wasn't carried into the Deprecations section, and the KV secret-store change wasn't given its own Added accordion in the Changelog.

**How to apply:** After drafting Release Highlights, walk back through every claim made there one by one and confirm: (1) anything added/changed/fixed and named in Highlights has a matching Added/Changed/Fixed accordion in the Changelog, and (2) anything called out as deprecated in Highlights has a matching entry in the Deprecations section. Do this as a mandatory self-check before delivering any draft, the same way B1's "could a customer understand this" self-check works.

**Why:** A customer who reads Release Highlights and then checks the Changelog/Deprecations sections for the full detail should always find a matching entry - a highlight with no matching section entry reads as a gap or an inconsistency in the release notes.

#### C4. Legacy env-var deprecation tables: verify per-pump-variant field parity against the actual config structs (added 2026-08-31)

When a deprecation table maps a legacy env-var prefix (e.g. `PMP_MONGO_*`) to new-style equivalents (`TYK_PMP_*`), and the ticket/PR also deprecates sibling prefixes for related pump variants (e.g. `PMP_MONGOAGG_*` for the Mongo Aggregate pump, `PMP_MONGOSEL_*` for the Mongo Selective pump), do **not** assume every variant carries the exact same set of suffixes as the original table. Check the actual Go config structs in the mounted `tyk-pump-master` source (`pumps/mongo.go`, `mongo_aggregate.go`, `mongo_selective.go`) - pump variants share a common embedded base config (e.g. `BaseMongoConf`) but each variant's own struct only adds the fields that variant actually supports.

**Confirmed example (Pump 1.17.0, tyk-pump#1060):** `BaseMongoConf` (10 shared fields: MongoURL, MongoUseSSL, MongoSSLInsecureSkipVerify, MongoSSLAllowInvalidHostnames, MongoSSLCAFile, MongoSSLPEMKeyfile, OmitIndexCreation, MongoSessionConsistency, MongoDriverType, MongoDirectConnection) is embedded in all of `MongoConf` (standard pump), `MongoAggregateConf`, and `MongoSelectiveConf` - so `PMP_MONGOAGG_*` and `PMP_MONGOSEL_*` both get these 10 rows. But `CollectionName`, `CollectionCapMaxSizeBytes`, and `CollectionCapEnable` exist **only** on `MongoConf` (standard pump) - neither Aggregate nor Selective have them. `MaxInsertBatchSizeBytes` and `MaxDocumentSizeBytes` exist on `MongoConf` **and** `MongoSelectiveConf`, but not on `MongoAggregateConf`. Net result: standard pump gets all 15 legacy vars, Selective gets 12 (10 base + the two Max*Bytes), Aggregate gets only 10 (base only) - not a uniform 15/15/15 mirror across all three prefixes.

**How to apply:** grep the relevant pump/component's config struct definitions for the base config type and each variant's own added fields before writing or extending a migration table with a new prefix family. Also worth checking `EnvPrefix` default-value doc comments on the base struct (e.g. `BaseMongoConf.EnvPrefix` in `mongo.go` lists all four `TYK_PMP_PUMPS_*_META` defaults directly) - they're a fast way to confirm the exact new-style prefix name for each variant. See `project-memory.md`.

**Why:** Joan's PR feedback flagged that the original deprecation table only covered `PMP_MONGO_*` when the underlying ticket also deprecated `PMP_MONGOAGG_*`/`PMP_MONGOSEL_*`. A naive fix would have duplicated all 15 rows for each new prefix, which would have introduced fabricated env vars (`PMP_MONGOAGG_COLLECTIONNAME` etc.) that don't exist in the actual config struct.

#### C5. Read a ticket's QA/review comments, not just its description - shipped behavior can diverge from the original acceptance criteria (added 2026-09-07)

A ticket's description/acceptance-criteria section is the *proposed* behavior; QA and engineering-review comments posted later are where you find out what was *actually* shipped, and the two can disagree. Always read the full comment thread before writing a changelog entry, especially for tickets that look complex or went through more than one review round.

**Confirmed example (Tyk Charts 5.4.0, TT-16572):** the ticket's acceptance criteria explicitly called for `terminationGracePeriodSeconds: 0` to be honored the same way `maxSurge: 0` / `maxUnavailable: 0` are (falsy-zero handling). A QA comment posted after implementation found the shipped code deliberately treats `0` as "unset" instead (documented in the shipped `values.yaml` comment, with a passing test asserting exactly that) - a considered decision, not a bug, but one that contradicts the ticket's own stated AC. Write the changelog entry to describe the real shipped behavior (here: "set a positive value; `0` is treated as unset") rather than the ticket's original claim.

**Related - stale codebase snapshot can look like a red flag when it isn't:** on the same release, two other "Closed" tickets (TT-15287, TT-16461) had zero trace in the local `tyk-charts-main` snapshot even though their Jira status was Closed. Checking each ticket's QA comment *dates* (2026-08-11 through 2026-08-14) against the snapshot's extraction date (zip mtime: 2026-08-05) explained it - both were verified on their feature branches only after the snapshot was taken. **When a supposedly-shipped feature is missing from the local snapshot, compare the ticket's latest QA/merge comment timestamp against the snapshot folder's zip mtime before concluding anything is wrong** - a mismatch where the comment postdates the snapshot means "snapshot is stale," not "ticket isn't really done." See D4 above for the broader snapshot-staleness pattern this reinforces.

**Why:** Two real discrepancies in one release (a shipped-vs-claimed behavior mismatch, and a stale-snapshot false alarm) both surfaced only by reading comments in full rather than trusting the ticket description or ticket status alone.

#### C6. Dependency tables: the Confluence dependency page is the source of truth (added 2026-10-22)

**Source of truth:** [Distro OS, KV Store, and Dependency Versions Being Tested In Tyk Pipeline](https://tyktech.atlassian.net/wiki/spaces/CVBGF/pages/4499079175/Distro+OS+KV+Store+and+Dependency+Versions+Being+Tested+In+Tyk+Pipeline) (Confluence space CVBGF, page ID `4499079175`).

This page lists the 3rd-party dependencies (distro OS, KV stores such as Redis, databases, and other dependency versions) that Tyk actually validates and tests in its pipeline.

**How to apply, for every release note of every component:**
- Before writing the **Dependencies** section ("3rd Party Dependencies & Tools" table), fetch this page (Atlassian connector, page ID `4499079175`) and check every row against it.
- Only mark a version as tested/validated if the page shows it is. If the page doesn't cover a dependency or version, mark it **[TO CONFIRM]** rather than guessing.
- Flag any mismatch between the draft (or the previous release's table) and the page in the "Notes for Joan" block.
- Applies to new drafts and to reviews of existing release notes.

**Why:** Joan confirmed this page is where the team records what is actually validated and tested, so it is the reference for checking dependency tables across all RNs.

---

---

### D. Process - Jira & CVE Sourcing (in effect since 2026-07-28)

#### D1. This is now the standard drafting process, not ad-hoc

Starting with the Tyk 5.13.2 release, release notes are drafted by pulling tickets directly from Jira (via the Atlassian Rovo connector) and CVEs directly from the CVE board, rather than from tickets pasted into chat. Full procedure lives in `CLAUDE.md` under "AI-Assisted Drafting Process" - treat that as the authoritative, up-to-date version of this workflow and keep it in sync if the process changes further.

**Why:** Manual ticket-by-ticket Jira browsing plus copy-paste was the main source of toil in the old process. This also gets Include-in-Changelog and Breaking-Change filtering right by construction, rather than relying on manual judgement per ticket.

#### D2. CVE board filter - always apply this exact combination

When sourcing CVEs for any release and component's Security Fixes section, filter the `CVE` project by: **Affected Component** = target component, **Fix Version** = target release, **CVE Severity** not equal to `"Low"` (Critical/High/Medium only - Low is always excluded).

Field IDs: Affected Component = `customfield_11122`, CVE Severity = `customfield_11053`. Use `cf[11053] != "Low"` rather than `cf[11053] IN ("Critical","High","Medium")` - the severity option labels have inconsistent trailing whitespace (`"High "`, `"Medium "` vs. `"Critical"`) that silently breaks an inclusive IN-list match.

**Why:** Explicit instruction from Joan (2026-07-28) - CVE severity was previously being over-included (e.g. the first 5.13.2 draft included three Low-severity CVEs that should have been dropped).

#### D3. Tyk Cloud never gets a CVE list

For Tyk Cloud release notes specifically, do not list individual CVEs in a Security Fixes section. Say only, in the Release Highlights paragraph: *"Addressed several CVEs within dependencies."* Every other component (Gateway, Dashboard, Pump, Sync, MDCB, Operator, Portal, TIB, Charts) keeps the full filtered CVE list in its own Security Fixes section as normal - this exception is Cloud-only.

**Why:** Explicit instruction from Joan (2026-07-28) - Tyk Cloud's release notes intentionally don't expose the underlying CVE detail to that audience.

#### D4. Codebase access - mounted "Tyk Code Claude" folder (added 2026-08-05)

Joan has connected a second folder, `Tyk Code Claude`, containing extracted source snapshots of the Tyk repos. **Always check this folder first** for codebase cross-checks, before falling back to cloning from GitHub - full mapping and caveats are documented in `CLAUDE.md` under "Codebase cross-check". Quick summary:

- Covers: Gateway (`tyk-master`), Operator (`tyk-operator-internal-master`), Sync (`tyk-sink-master`), Pump (`tyk-pump-master`), Portal (`portal-master`), TIB (`tyk-identity-broker-master`), Charts (`tyk-charts-main`) - all with full source, one directory level deeper than the top-level folder name (e.g. `tyk-master/tyk-master/`).
- `tyk-analytics-ui-master` is Dashboard's **frontend/webclient only** (no `go.mod`, no backend Go code) - it does not close the Dashboard backend verification gap. Backend Dashboard tickets (anything under a `dashboard/*.go` path) are still ticket-text-only.
- Tyk Cloud (Ara) still has no source available anywhere.
- This is a point-in-time snapshot, not a live clone - a ticket still in progress in Jira may not show up here yet even once merged, if the folder hasn't been refreshed since. If verification against this folder is inconclusive for something you'd expect to be merged, ask Joan whether the snapshot needs refreshing before concluding the fix isn't real.

**Why:** Closes most of the private-repo gap discovered while drafting 5.13.2 (only `tyk-analytics`/Dashboard-backend remains unverifiable), and is much faster than cloning fresh each session.

#### D4b. Device-folder tool fallbacks (added 2026-08-13)

If `device_stage_files` fails on a specific file with `"HTTP 400 adding session file"` (seen on 6 of 9 governance-UI JS files in one batch, both individually and grouped - root cause not diagnosed), fall back immediately to reading it via `device_bash cat "<full path>"` instead of retrying the staging call. Also, `device_bash` against a large repo tree (e.g. a recursive grep/find over an entire app directory) can time out at 45s - prefer `device_list_dir` (non-recursive first, then a targeted recursive call on a narrower subdirectory) over a full-tree shell search, and scope any `device_bash` command to a specific known subdirectory rather than a repo root. Separately, if Confluence's `getConfluencePageDescendants` 404s on a folder-shaped URL ID (a `/wiki/spaces/.../folder/<id>` link isn't always a real Confluence page/content entity), fall back to `searchConfluenceUsingCql` with a `title ~ "..."` query to find the named pages directly.

#### D5. CVE severity filter for 5.13.2 - Critical/High only, Medium excluded (2026-08-13)

For the 5.13.2 Gateway and Dashboard drafts specifically, Joan gave an explicit JQL filter that only includes `Critical` and `"High "` severities - **Medium is excluded**, unlike the standing D2 rule below (which keeps Medium and only excludes Low). This dropped two CVEs that would otherwise have appeared (CVE-2026-41178, OpenTelemetry-Go baggage-header DoS; GHSA-jpcw-4wr7-c3vq/CVE-2026-73502, kin-openapi nil-pointer DoS - both Medium).

**Status: not yet confirmed as the new standing default.** I flagged this explicitly in both drafts' "Notes for Joan" and applied Critical/High-only for this round, but D2 below still says Critical/High/Medium. **Do not silently apply the tighter Critical/High-only filter to other releases or other components until Joan confirms whether this replaces D2 permanently or was specific to 5.13.2.** If she confirms it's permanent, update D2 itself rather than leaving both rules side by side.

**2026-09-28 update:** Joan again supplied explicit Critical/High-only JQL (`"cve severity [dropdown]" IN ("High ", Critical)`) for the final 5.13.3 and 5.15.1 Gateway/Dashboard CVE lists - third release using it. Still apply it whenever Joan provides that JQL; ask before assuming it for releases where she doesn't. Result that day: Gateway 18 CVEs, Dashboard 16 (Dashboard lacks CVE-2026-76905 and CVE-2026-77354, which are Gateway+Portal only); identical across 5.13.3 and 5.15.1. The CVE tickets by then carried version ids 14784/14785, so name-based fixVersion JQL returned the right set. Intro sentence used: "We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:" (matches 5.13.2), ordered Critical first then High, NVD links.

#### D6. When a ticket that appeared in a prior draft is missing from a fresh fixVersion pull, verify directly - don't assume it's still in scope

Discovered 2026-08-13 re-drafting 5.13.2: TT-431 and TT-1860 were headline entries (a Breaking Change + a Security Fixed entry) in the 2026-08-05 Gateway draft, but neither appeared in a fresh `fixVersion = "Tyk 5.13.2"` pull. Fetching each directly via `getJiraIssue` with the `fixVersions` field confirmed both now carry `fixVersions: []` - no fix version at all, not even a different one. **The ticket's disappearance from the list was correct, not a pagination/query bug** - always verify with a direct per-ticket fetch before assuming a previously-included ticket "must still belong" and re-adding it from memory of the last draft. Flag the removal to Joan rather than silently dropping or silently keeping the old content.

#### D7. Jira search tool paginates at 5 nodes per call regardless of `maxResults`

`searchJiraIssuesUsingJql` returns at most 5 issues in `nodes` per call no matter what `maxResults` is set to (seen up to 100). It does report a real `remainingCount`/`totalCount` (use `searchResultMode: "all"` or `"count"` to get an accurate total), but `pageInfo.hasNextPage`/`endCursor` are unreliable - they showed `false`/`null` even when more results existed. To page through a full result set: note the **last key actually returned** in each batch (not an assumed/rounded key), then re-run with `AND key > "<that exact last key>"` appended to the JQL, repeating until an empty page comes back. Also: the `fields` parameter is not fully respected for `description` - even a narrow field list can still return full ticket descriptions, which is what causes the "output too large, saved to file" fallback on tickets with long descriptions (e.g. CVE tickets, or any ticket with an AI-analysis comment). Read the saved file with `python3 -c "import json..."` rather than the raw `Read` tool when that happens.

#### D8. Drafting an LTS release and a "latest" release together - keep shared tickets textually identical (added 2026-08-13)

When Joan asks for both the LTS release (e.g. 5.13.2) and the current latest/non-LTS release (e.g. 5.15.0) in the same session, many tickets ship in both. Rule: reuse the already-drafted LTS prose **verbatim** for any shared ticket - don't re-synthesize from the ticket text a second time, since independent synthesis passes drift in wording even when the meaning matches. Tag every new-release ticket against the LTS draft's ticket list before drafting, so shared ones are identified up front rather than discovered by accident. If researching the new release surfaces a better or more accurate way to write a shared entry, do NOT apply it unilaterally to only the new draft - flag it to Joan so she can decide whether to update the LTS draft too, and say explicitly in the new draft's Notes-for-Joan whether any divergence was found ("no divergence found" is a normal, good outcome worth stating, not leaving implicit). CVE lists can also turn out identical between two close-together releases - always verify by querying the new release rather than assuming reuse is safe (see the CVE-sourcing notes in `project-memory.md`).

#### D9. A named ticket-exclusion list may only be part of a larger feature - check before excluding just those tickets (added 2026-08-13)

Joan named 6 specific tickets to exclude from the 5.15.0 draft (an "API-to-MCP" feature's Dashboard UI wizard) with the phrase "don't worry about any tickets regarding MCP stuff." A full fixVersion pull turned up 5 more tickets - not in her named list - that were clearly the Gateway-side backend half of the exact same feature. When a named exclusion (or inclusion) list looks like it could be only part of a coherent feature, actively scan the rest of the release's ticket pull by theme/keyword for the missing half, and surface a scoping question before drafting rather than silently assuming the named list is exhaustive or silently expanding it yourself. Joan confirmed excluding the whole cluster once asked. General pattern: treat named-ticket instructions as likely meaning "this whole feature," and verify that reading before committing to a scope.

#### D10. Large ticket/CVE/documentation pulls should be delegated to parallel subagents (added 2026-08-13)

A single fixVersion pull can be 80-90+ tickets, each returned with a full (sometimes very long) description regardless of the `fields` filter (see D7) - reasoning over all of them inline in the main session risks blowing the context budget before drafting even starts. Pattern that worked well for the 5.15.0 draft: spin up one subagent per independent research stream (full ticket-list pull + condense-to-JSON, one CVE pull per component, Confluence documentation research) and run them concurrently. Have the ticket-pulling subagent write the full condensed dataset to a file and return only a compact summary (counts, and the full key list for anything flagged for follow-up, like missing-components or feature-cluster tags) - then read that file directly in the main session instead of re-deriving it. Keep the actual editorial/consistency judgment calls (especially LTS-vs-latest text reuse) in the main session, since that needs the full cross-release picture at once; don't delegate that part.

---

#### D11. Jira can have two different version records sharing the exact same name - disambiguate by numeric ID (added 2026-09-11)

Discovered drafting Tyk 5.13.3/5.15.1: there were **two separate Jira version objects both literally named "Tyk 5.13.3"** (id `14784` and id `14860`), and likewise two named "Tyk 5.15.1" (`14785` and `14859`). A plain JQL search by name (`fixVersion = "Tyk 5.13.3"`) silently matches issues tagged with *either* version object, which can make a release look like it includes unrelated tickets from a different release wave (in this case, CVE-board tickets actually belonging to the "2026 Release 4" wave alongside Ara 1.33.1 / Tyk Portal 1.19.0 got pulled in alongside the real 5.13.3/5.15.1 CVE-roundup tickets).

**How to catch this:** if a fixVersion-name JQL pull returns tickets that look tangential or from an unrelated wave, re-run the query using the exact numeric version ID instead of the name (`fixVersion = 14860`, taken from the version's `id` field in the `fixVersions` array, or from the Jira release-report URL Joan links - `.../versions/<id>/tab/...`). Cross-check by fetching the release's own roundup/Activity tickets (e.g. "<Component> X.Y.Z CVEs") directly and reading their `fixVersions` array to see which numeric ID is actually correct, then use that ID for the real pull.

**Why it matters:** name-based JQL is what's used everywhere else in this workflow (D1/D2/D7), so this is an easy silent-scope-inflation bug to miss - always sanity-check that a small patch/security release actually returns a small, coherent ticket set before drafting from it.

#### D12. CVE-project fix versions have their own IDs - TT release-report IDs don't apply to CVE tickets (added 2026-10-13)

Drafting Tyk 5.16.0: the TT release-report URL Joan linked uses version id `14454`, but the CVE tickets carry the **CVE project's own** "Tyk 5.16.0" version (id `14786`). Jira versions are per project, so this is expected, not a D11-style duplicate. Use the TT numeric id for TT tickets (`fixVersion = 14454`), and the name-based JQL Joan supplies for the CVE board. Also seen this round: the Jira search returned all 78 TT tickets in one call, so the D7 5-per-page limit doesn't always apply - still verify the count.

**Feature-cluster entries (5.16.0):** Joan asked for one Changelog accordion per multi-ticket feature (API Fragments epic TT-17450; Dashboard OTel metrics "D1, D1.1, D2…" epic TT-18193), even when most child tickets are Include = No - fold their customer-facing details into the one umbrella entry and list every ticket in its `{/* */}` comment.


---

## Useful prompts

**Draft a release.** Typical request: "Draft the release notes for Tyk Gateway and Tyk Dashboard 5.13.6. Jira release: https://tyktech.atlassian.net/projects/TT/versions/15161/tab/release-report-all-issues". How to handle it:

1. Take the numeric version id from the link (`15161`) and pull TT tickets with `fixVersion = 15161` (rule D11). Check the version name it resolves to against the version Joan typed; if they differ, ask before drafting.
2. If no link is given, ask for it. Release names have a prefix: "Tyk 5.13.6" for Gateway and Dashboard, "Tyk Pump 1.18.0", "Tyk Charts 5.5.0", "Tyk Operator 1.6.0", and so on.
3. Pull CVEs from the CVE board per component and version (D2, D5, D12), unless it's Tyk Cloud (D3).
4. Follow the drafting process, the per-entry workflow and every editorial rule above, then add the version to each component file (see [Output](#output)).
5. Reply with the review notes and a list of every [TO CONFIRM].

**Other common requests:** a single entry from one ticket ("Write a changelog entry for TT-12345"); a paste-ready page for the tyk-docs PR (assemble the version blocks in published order); a review of existing notes against these rules; adding a new rule to this file.
