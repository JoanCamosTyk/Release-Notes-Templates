# Tyk Release Notes Knowledge Repository

A comprehensive collection of historical release notes and templates for all Tyk components, designed to maintain consistency, quality, and brand voice across all product releases.

## Contents

1. [Purpose](#purpose)
2. [Tyk Components](#tyk-components)
3. [Working with Claude](#working-with-claude)
   - [Changelog Entries](#changelog-entries)
   - [Release Highlights](#release-highlights)
4. [Editorial Rules](#editorial-rules)
5. [AI-Assisted Drafting Process (current workflow)](#ai-assisted-drafting-process-current-workflow)
   - [Sourcing tickets from Jira](#sourcing-tickets-from-jira)
   - [Sourcing CVEs from the CVE board](#sourcing-cves-from-the-cve-board)
   - [Codebase cross-check](#codebase-cross-check)
   - [Output](#output)
6. [Drafting Workflow (per-entry)](#drafting-workflow-per-entry)
7. [Useful Prompts](#useful-prompts)
8. [Before You Start](#before-you-start)

---

## Purpose

This repository serves as:

- **Style guide** for release communications
- **Historical reference** for past releases
- **AI knowledge base** for Claude to learn Tyk's release patterns
- **Template library** for consistent formatting

---

## Tyk Components

| Component | Description | Latest Examples |
|---|---|---|
| **[Gateway](gateway/)** | Core API Gateway functionality | Traffic management, auth, rate limiting |
| **[Dashboard](dashboard/)** | Management UI and API | Admin interface, analytics, configuration |
| **[Pump](pump/)** | Analytics data processor | Data export, monitoring integrations |
| **[Sync](sync/)** | Configuration synchronization | Multi-gateway coordination |
| **[Multi Data Center Bridge](multi-data-center-bridge/)** | Multi-region connectivity | Global deployments, data replication |
| **[Tyk Operator](tyk-operator/)** | Kubernetes operator | K8s native management |
| **[Tyk Chart](tyk-chart/)** | Helm charts | Kubernetes deployment templates |
| **[Tyk Identity Broker](tyk-identity-broker/)** | Identity management | SSO, identity federation |
| **[Tyk Cloud](tyk-cloud/)** | SaaS platform | Cloud-hosted service updates |

---

## Working with Claude

### Changelog Entries

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

## Editorial Rules

These rules apply to every changelog entry and Release Highlights paragraph. They reflect feedback gathered while drafting real Tyk release notes and should be applied without needing to be re-stated. (Additional, more granular rules learned session-to-session live in `CLAUDE_MEMORY.md` — see [Before You Start](#before-you-start).)

### Terminology

- **Always write "Tyk OAS API"** (or "Tyk OAS APIs"). Never abbreviate to "OAS API" — the "Tyk" prefix disambiguates Tyk's product from generic OpenAPI/OAS terminology.
- **"OpenAPI document" vs. "OpenAPI Specification":**
  - Use **"OpenAPI document"** when referring to a YAML/JSON file the user provides, imports, or validates.
  - Use **"OpenAPI Specification"** (capitalized) only when naming the standard itself (e.g., "OpenAPI Specification 3.1").
  - Do not write "OpenAPI specification" or "OAS specification" to mean a file — that wording is incorrect.
- **Use customer-facing license names**: "unlimited node license" and "limited node license". Do not surface internal names such as "Infinite Nodes" in release notes.

### Tone and Length (especially for Fixed entries)

- **Fixed entries should be short.** Aim for 2–4 sentences. Calibrate against existing Fixed entries in `Gateway.md`, `Dashboard.md`, `Sync.md`, etc. — they are the target.
- **Lead with user-visible impact, not the cause.** Describe what was broken from the customer's experience and what now works correctly. Skip the chain of reasoning and root-cause analysis.
- **Cut RCA narrative.** Phrases like "the root cause was X", "previously the code did Y, now it does Z", or "this is caused by a combination of A and B" do not belong in a Fixed entry. The customer needs to know it's fixed and what improves — not the postmortem.
- **Remember tickets are written for engineers, not for release notes.** Most of the ticket body (root-cause analysis, file paths, function names, before/after snippets, PR numbers) should not appear in the RN. Translate the ticket into customer impact.
- **Less verbosity, more impact.** If a sentence does not change what the customer learns, remove it.

### What Not to Expose

The following must never appear in a release-note entry:

- **Internal field names and JSON paths** (e.g., `version_data.versions.<name>.expires`, `x-tyk-api-gateway.info.expiration`)
- **Internal Redis keys** (e.g., `tyk:node:registration:lock:<sessionID>`)
- **Function names, file paths, source files, PR numbers, ticket IDs**
- **Internal subsystem names** that aren't customer-facing
- **Step-by-step "what we changed" / before-after code descriptions**
- **Internal license-type names** (use the customer-facing term)

Replace internal references with the customer-facing concept (e.g., "the version expiry" instead of `version_data.versions.<name>.expires`).

### What to Always Call Out

- **New configuration options.** If a ticket adds a config key, env var, API/policy/session field, CLI flag, or changes a default, name it in the entry. State the default value and whether the change is backward-compatible or opt-in.
- **New customer-facing fields.** New log fields, response headers, request attributes, and API fields are configuration surface for downstream consumers — they must be named.
- **License-scope caveats.** If a fix only fully applies to certain license types (e.g., unlimited node licenses), state the scope in the title or the opening sentence so customers can immediately tell whether it affects them. Set expectations for any planned follow-up.

### Ticket Traceability

- **Every Changelog accordion must carry its source ticket number(s) as an MDX comment**, placed on the line immediately after the `<Accordion title='...'>` opening tag: `{/* TT-431 */}`. For entries built from multiple tickets, list them comma-separated on the same line: `{/* TT-9550, TT-17641 */}`.
- This comment is invisible in the rendered docs (MDX strips `{/* ... */}`), so it has no effect on what customers see — it exists purely so we can trace any Changelog entry straight back to its Jira ticket(s) later, without re-reading the prose to guess which ticket it came from.
- Apply this to every Changelog accordion (Changed/Added/Fixed) without exception. The bulk "Resolved CVEs" Security Fixes accordion is the one exception — CVEs are sourced from the CVE board rather than a single TT ticket, so no ticket comment is needed there (a dedicated named CVE fix tied to one ticket, like a library migration, should still carry its ticket number).
- Breaking Changes and Release Highlights entries are prose, not accordions, so this doesn't apply to them.

### Component Grouping

- **Do not split shared fixes by component.** When a single fix spans Gateway and Dashboard (or any pair of components that ship together), describe it as one unified change from the customer's point of view. Do not write "the Dashboard now does X and the Gateway now does Y" — describe the resulting behavior. The same entry simply appears in both component release notes with the same text.
- **Do not add "requires upgrading both X and Y" warnings.** Tyk customers upgrade Gateway and Dashboard at the same time; mixed-version caveats clutter the entry and are not actionable.
- **Bundle related small UI/UX items into one accordion** when the individual entries don't warrant standalone changelog items. Use a section title like "Dashboard UI enhancements" with one bold sub-heading per change. Keep substantive features (anything with API or behavioral surface beyond the UI) as their own dedicated entry.
- **Consolidate critical-incident fixes into one entry.** If a single customer incident produced multiple tickets across components (e.g., a registration storm split across lock contention, retry handling, and Redis load), prefer one consolidated changelog entry that states the high-level problem, briefly notes the cause, and describes the improved behavior — not multiple accordions that read like a postmortem.

---

## AI-Assisted Drafting Process (current workflow)

As of 2026-07-28, release notes for a given version are no longer drafted purely from tickets pasted into chat. Claude pulls the tickets directly from Jira, pulls CVEs directly from the CVE board, cross-checks against the public Tyk codebase where possible, and produces ready-to-paste MDX files for review. This section documents that process so it can be repeated consistently for every release. The [Drafting Workflow](#drafting-workflow-per-entry) section below still applies to how each individual entry is written once the source tickets are identified.

### Sourcing tickets from Jira

1. Connect to Jira via the Atlassian Rovo MCP connector (cloud site `tyktech.atlassian.net`).
2. Pull every ticket tagged with the release's Fix Version. **Jira fix versions use a `"Tyk X.Y.Z"` prefix** (e.g. `"Tyk 5.13.2"`), not the bare version number — `fixVersion = "5.13.2"` will return nothing.
3. For each ticket, check:
   - **Include in Changelog** (custom field `customfield_10335`) — a `Yes` value with the exact label `"Include in changelog (Yes – Included in external release notes No – Not included in external release notes internal information only)"`. If this is explicitly `No`, exclude the ticket. If it's unset (`null`), don't assume `No` — check whether the ticket is superseded by another ticket instead (see below) before excluding it.
   - **Breaking Change** (custom field `customfield_10684`, labelled `"Breaking Change "` with a trailing space) — a `Yes` value means the change must appear in the Breaking Changes section as well as its own Changelog entry.
   - **Components** — determines which component release notes page(s) the entry belongs to. A ticket tagged with more than one component (e.g. Gateway + Pump) gets the same entry mirrored in both component files.
   - **Ticket Type** (Bug vs. Story) — Bug → candidate for Fixed, Story → candidate for Added, unless it's actually a CVE-driven Story, in which case it belongs in Security Fixes instead (see below).
4. Watch for **superseded/merged tickets**: Jira sometimes links an older story as "is implemented by" another ticket (a "Polaris work item link"). If a ticket has no Include-in-Changelog value set and is linked this way, it's very likely folded into the implementing ticket's entry rather than needing (or getting) its own.
5. Watch for **epic/parent groupings** the same way — via issue links, not always a formal epic field.

### Sourcing CVEs from the CVE board

CVEs are **not** pulled from a component's tickets directly — they live in a separate Jira project, key `CVE` ("CVE Repository"). Use this exact filter every time a Security Fixes section is needed for any release and component:

- **Project** = `CVE`
- **Affected Component** (custom field `customfield_11122`, multi-select) = the target component, e.g. `"Tyk Dashboard"` or `"Tyk Gateway"`
- **Fix Version** (standard `fixVersion` field) = the target release, e.g. `"Tyk 5.13.2"`
- **CVE Severity** (custom field `customfield_11053`) — **exclude `"Low"`**; only `Critical`, `High`, and `Medium` are shown in release notes.

As JQL, this looks like:

```
project = CVE AND cf[11122] = "Tyk Dashboard" AND fixVersion = "Tyk 5.13.2" AND cf[11053] != "Low" ORDER BY key ASC
```

**Important implementation note:** the CVE Severity option values have inconsistent trailing whitespace in Jira (e.g. `"High "`, `"Medium "` vs. `"Critical"` with no space), which breaks an inclusive `IN ("Critical","High","Medium")` clause — some valid entries silently fail to match. Always use the **exclusion** form (`cf[11053] != "Low"`) instead of an inclusive list, since `"Low"` itself doesn't have this whitespace problem.

Each CVE ticket's description contains the CVE/GHSA ID, an NVD or OSV link, CVSS score, and severity — use the ID and link directly rather than re-deriving them. Link CVE-prefixed IDs to `https://nvd.nist.gov/vuln/detail/<CVE-ID>` and GHSA-prefixed IDs to `https://osv.dev/vulnerability/<GHSA-ID>`, matching the existing convention in the component files.

If a specific CVE fix is significant enough to warrant its own named accordion (for example, a library migration tied to one CVE, like moving off an unmaintained `nosurf` fork), give it its own entry above the generic "Resolved CVEs" bulk-list accordion — don't bury a notable fix inside the anonymous batch.

**Exception — Tyk Cloud:** Tyk Cloud release notes do **not** list individual CVEs in a Security Fixes section. Instead, mention it briefly in the Release Highlights paragraph, e.g. *"Addressed several CVEs within dependencies."* This exception applies to Tyk Cloud only — every other component (Gateway, Dashboard, Pump, Sync, MDCB, Operator, Portal, TIB, Charts) continues to show the filtered CVE list in a dedicated Security Fixes section as normal.

### Codebase cross-check

**Primary source: the locally mounted "Tyk Code Claude" folder.** Joan has connected a folder containing extracted snapshots of the Tyk repos, giving direct file access every session with no cloning, no network restrictions, and — critically — no public/private repo problem. Always check this folder first before falling back to cloning from GitHub.

Current contents and their component mapping (each is a zip extracted into a folder of the same name, so the real source sits one level deeper, e.g. `tyk-master/tyk-master/`):

| Folder | Component | Notes |
|---|---|---|
| `tyk-master` | Tyk Gateway | Full Go source, `go.mod` present |
| `tyk-analytics-ui-master` | Tyk Dashboard — **frontend/UI only** | No `go.mod` — this is the `webclient` layer only (JS, AngularJS, `app/pages/...`). The Dashboard **backend** (Go handlers under `dashboard/*.go`, referenced by tickets like the ones touching `dashboard/api_users.go` or `internal/unzip/unzip.go`) is still not available anywhere. Only UI/webclient-side ticket claims can be verified against this folder — backend-side Dashboard claims still can't be verified and should stay flagged as ticket-text-only. |
| `tyk-operator-internal-master` | Tyk Operator | This is the internal/private development repo, not the public OSS mirror |
| `tyk-sink-master` | Tyk Sync | Repo's actual name is `tyk-sink` |
| `tyk-pump-master` | Tyk Pump | |
| `portal-master` | Tyk Developer Portal | |
| `tyk-identity-broker-master` | Tyk Identity Broker (TIB) | |
| `tyk-charts-main` | Tyk Charts | Helm charts, no `go.mod` (expected — not a Go project) |

**Still not available:** the Tyk Dashboard backend (Go) and Tyk Cloud (Ara) have no source in this folder. Backend Dashboard tickets and any Cloud ticket stay ticket-text-only until/unless those are added too.

For each ticket, grep the relevant folder for the function names, config keys, or log strings mentioned, to confirm a fix has actually merged/matches the described behavior before treating it as final — especially anything touching a config name, default value, or exact behavior description. A ticket still "In Code Review" / "In Dev" / "In Refinement" in Jira may simply not be reflected in this snapshot yet (it's a point-in-time export, not a live clone); that's expected, not an error, but should be flagged back for a final check closer to release, and re-confirmed once Joan refreshes the folder with newer snapshots.

**Fallback: cloning from GitHub.** If a repo isn't in the local folder, the public ones (`tyk`, `tyk-pump`, `tyk-sync-internal` confirmed clonable) can still be cloned directly (`git clone https://github.com/TykTechnologies/<repo>.git`) from the sandbox. Note that `raw.githubusercontent.com` is blocked by the sandbox's network allowlist, but `github.com` itself is allowed — use `git clone`/`git ls-remote` over the smart HTTP protocol rather than raw file fetches. Private repos (like the Dashboard backend, `tyk-analytics`) still can't be cloned this way — that's exactly the gap the local folder is meant to close, so ask Joan to add a repo to the folder rather than assuming it's unreachable forever.

### Output

Produce one Markdown file per component per release (e.g. `Gateway_5.13.2_Release_Notes.md`), formatted as ready-to-paste MDX matching the existing `<AccordionGroup>`/`<Accordion>` structure used in the component files, covering all seven sections from [Purpose](#purpose): Release Highlights, Breaking Changes, Dependencies, Deprecations, Upgrade Instructions, Downloads, and Changelog. Include a "Notes for Joan" block at the end of each file (removed before publishing) covering: excluded tickets and why, merged/superseded tickets, tickets still in progress in Jira as of drafting, and anything that could or couldn't be verified against source. Mark anything genuinely unconfirmable (compatibility matrix specifics, published artifact links, 3rd-party dependency versions with no corresponding ticket) with **[TO CONFIRM]** rather than inventing values.

---

## Drafting Workflow (per-entry)

When drafting a release note from a ticket, follow this sequence:

1. **Classify the ticket** into one of: `Changed` (updates/upgrades), `Added` (new features), `Fixed` (bug fixes), or `Security Fixes` (CVE batches).
2. **Confirm the component(s).** If the fix applies to more than one component that ships together (e.g., Gateway + Dashboard), the same entry is mirrored in both component files.
3. **Scan for new configuration surface** — config options, env vars, API/policy/session fields, log fields, response headers — and ensure each is named in the draft.
4. **Scan for license-scope caveats** — if the fix only fully resolves the issue for a subset of license types, the scope must be in the title or opening sentence.
5. **Draft the title** — short, action-oriented, customer-visible language. Existing entries in this repo are the calibration target.
6. **Draft the summary** — 2–4 sentences for Fixed entries; longer is acceptable for Added entries if the feature genuinely requires it. Apply all editorial rules above.
7. **Self-check before delivering:**
   - Could a customer who has never read the ticket understand what improved for them, in 2–4 sentences?
   - Is any internal field name, file path, function name, or PR number still present?
   - Is "Tyk OAS API" used (never "OAS API")?
   - Is "OpenAPI document" used where appropriate?
   - Are new config options and new customer-facing fields named?
   - Is the license scope clear if it matters?
   - Are related fixes consolidated rather than split?
   - Does the accordion have its source ticket number(s) as an `{/* TT-XXX */}` comment on the line after the title? (see [Ticket Traceability](#ticket-traceability))

---

## Useful Prompts

*(No prompts recorded yet — add ready-to-use prompt templates here as they come up.)*

---

## Before You Start

> Also read **`CLAUDE_MEMORY.md`** in this folder before drafting anything. It contains accumulated feedback and rules learned from past sessions that apply on top of everything in this document, and it must be kept up to date as new feedback is given.
