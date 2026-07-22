# Tyk Release Notes Knowledge Repository

A comprehensive collection of historical release notes and templates for all Tyk components, designed to maintain consistency, quality, and brand voice across all product releases.

## Contents

1. [Purpose](#purpose)
2. [Tyk Components](#tyk-components)
3. [Working with Claude](#working-with-claude)
   - [Changelog Entries](#changelog-entries)
   - [Release Highlights](#release-highlights)
4. [Editorial Rules](#editorial-rules)
5. [Drafting Workflow](#drafting-workflow)
6. [Useful Prompts](#useful-prompts)
7. [Before You Start](#before-you-start)

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

### Component Grouping

- **Do not split shared fixes by component.** When a single fix spans Gateway and Dashboard (or any pair of components that ship together), describe it as one unified change from the customer's point of view. Do not write "the Dashboard now does X and the Gateway now does Y" — describe the resulting behavior. The same entry simply appears in both component release notes with the same text.
- **Do not add "requires upgrading both X and Y" warnings.** Tyk customers upgrade Gateway and Dashboard at the same time; mixed-version caveats clutter the entry and are not actionable.
- **Bundle related small UI/UX items into one accordion** when the individual entries don't warrant standalone changelog items. Use a section title like "Dashboard UI enhancements" with one bold sub-heading per change. Keep substantive features (anything with API or behavioral surface beyond the UI) as their own dedicated entry.
- **Consolidate critical-incident fixes into one entry.** If a single customer incident produced multiple tickets across components (e.g., a registration storm split across lock contention, retry handling, and Redis load), prefer one consolidated changelog entry that states the high-level problem, briefly notes the cause, and describes the improved behavior — not multiple accordions that read like a postmortem.

---

## Drafting Workflow

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

---

## Useful Prompts

*(No prompts recorded yet — add ready-to-use prompt templates here as they come up.)*

---
