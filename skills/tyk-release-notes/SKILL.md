---
name: "tyk-release-notes"
description: "Draft, review or update Tyk release notes (changelog, Highlights, Breaking Changes, CVEs) for any Tyk component from a Jira release link, following the team's rules."
---

# Tyk release notes

Use this skill whenever someone asks to draft, review, fix or extend Tyk release notes (RNs) for any component: Tyk Gateway, Tyk Dashboard, Tyk Pump, Tyk Sync, Tyk MDCB, Tyk Operator, Tyk Charts, Tyk Identity Broker, Tyk Developer Portal or Tyk Cloud (Ara). Also use it for a single changelog entry from one ticket, or for adding a new release-notes rule.

## 1. Find the release-notes folder (the source of truth)

The rules, the process and every past release note live in the release-notes knowledge folder. Look for it in this order:

1. A connected folder on the user's computer named `Release_Notes` (Joan's working copy).
2. The `product-brain/skills/release-notes/` folder in the `TykTechnologies/shared-claude` GitHub repo.
3. Files synced into the current Claude Project.

It contains:

| File | Use |
|---|---|
| `CLAUDE.md` | Authoritative instructions: templates, the Jira and CVE process, the per-entry workflow and all editorial rules (A1 to D12 plus core rules). Read it in full before drafting. |
| `project-memory.md` | Background: Jira field IDs and quirks, codebase snapshot details, lessons learned, open initiatives. Read when a task touches Jira sourcing or codebase checks. |
| `Gateway.md`, `Dashboard.md`, `Pump.md`, `Sync.md`, `Multi-data-center-bridge.md`, `Operator.md`, `Charts.md`, `Tyk Identity Broker.md`, `Portal.md`, `Cloud.md` | One file per component with every past release, grouped by section, newest version first. These are the calibration target for tone, length and detail, and where new drafts go. |
| `README.md` | Human overview and owner (Joan Camos). |

If you can't find the folder, say so and ask the user to connect it or attach `CLAUDE.md`. If you still have to work without it, follow the quick rules in section 5, and say in your reply that you didn't check against the full rules.

If `CLAUDE.md` and this skill disagree, `CLAUDE.md` wins.

## 2. What the user should give you

The typical request looks like:

> Draft the release notes for Tyk Gateway and Tyk Dashboard 5.13.6. Jira release: https://tyktech.atlassian.net/projects/TT/versions/15161/tab/release-report-all-issues

- **Component(s) and version.**
- **Jira release link.** Take the numeric version id from it (`15161`) and pull by id, because Jira can have two versions with the same name. If there is no link, ask for it. Release names have a prefix: "Tyk 5.13.6" for Gateway and Dashboard, "Tyk Pump 1.18.0", "Tyk Charts 5.5.0", "Tyk Operator 1.6.0", and so on.
- **Check that they match.** If the release the id resolves to is not the version the user typed (for example they wrote 5.15.0 but the link is 5.13.6), ask before drafting.
- **Optional:** exclusions or overrides ("don't include the MCP tickets", "treat all tickets as closed"), a CVE JQL (for example Critical/High only), a second release to draft alongside (LTS plus latest), and the Highlights format (short or long; ask if not given).

## 3. Procedure for a full release

Use a task list; the user may step away.

1. **Read** `CLAUDE.md` in full, then the component file(s) you'll draft into.
2. **Pull tickets** from Jira (Atlassian Rovo connector, site `tyktech.atlassian.net`, project TT) with `fixVersion = <id>`. Check each ticket for:
   - Include in changelog (`customfield_10335`). Explicit No means exclude; unset is not No, so check whether it's superseded.
   - Breaking Change (`customfield_10684`, trimmed value). Yes means it goes in Breaking Changes too.
   - Components. These decide which file(s) the entry goes into; mirror shared entries with identical text.
   - Type. Bug means Fixed, Story means Added, version bumps mean Changed. CVE-driven work goes in Security Fixes.
   - Superseded or grouped tickets (issue links, epics), and the full QA comments (shipped behavior can differ from the description).
   - Exclude release readiness / release prep tickets and CVE roundup Activity tickets.
   - For 80 or more tickets, delegate the pull to parallel subagents that write condensed JSON; keep the editorial judgment in the main session.
3. **Pull CVEs** from the Jira `CVE` project per component and version: `project = CVE AND cf[11122] = "<Tyk Component>" AND fixVersion = "<Tyk X.Y.Z>" AND cf[11053] != "Low"`, unless the user gives their own JQL. Tyk Cloud never gets a CVE list; its Highlights say "Addressed several CVEs within dependencies."
4. **Verify against code** in the "Tyk Code Claude" folder where the source exists (no Dashboard backend, no Cloud). Check config names, defaults and behavior. Compare QA comment dates with the snapshot date before calling a miss a problem.
5. **Check dependencies** against the Confluence page "Distro OS, KV Store, and Dependency Versions Being Tested In Tyk Pipeline" (page id `4499079175`).
6. **Draft** each entry with the per-entry workflow and every rule in `CLAUDE.md`. If an LTS and a latest release are drafted together, reuse the LTS text verbatim for shared tickets.
7. **Write into the component file:** add a `### X.Y.Z` block, newest first, under each section that has content: Breaking Changes, Release Highlights, Dependencies, Deprecations, Upgrade Instructions, Downloads, Change Log. Sub-headings go one level down (`####`). The Change Log block starts with `#### Changelog` and `<a id="Changelog-vX.Y.Z" data-scroll-offset></a>`, then `##### Added / Changed / Fixed / Security Fixes` with `<AccordionGroup>` / `<Accordion>` MDX. Skip sections that would only say "There are no breaking changes / deprecations in this release." Never create separate per-release files.
8. **Self-check:** run the per-entry checklist in `CLAUDE.md`. Everything named in Highlights must have a Changelog or Deprecations entry. Search for em dashes and remove every one.
9. **Reply** with review notes in the chat, not in the file: excluded tickets and why, merged or superseded tickets, tickets still in progress, what was or wasn't verified, missing components, and a list of every **[TO CONFIRM]**.

When the user asks for the paste-ready page for the tyk-docs PR, assemble the version blocks in published order (Release Highlights, Breaking Changes, Dependencies, Deprecations, Upgrade Instructions, Downloads, Changelog), adding the standard "There are no ... in this release." lines for empty sections.

## 4. Other requests

- **Single entry** ("Write a changelog entry for TT-12345"): fetch the ticket and its comments, then return a title, the `{/* TT-12345 */}` comment and the body in chat. Don't edit files unless asked.
- **Review** ("Review the Gateway 5.13.6 notes"): check the version blocks against every rule and list the issues with suggested rewrites.
- **New rule** ("Add this as a rule: ..."): add it to `CLAUDE.md` under the right group (A terminology, B style and structure, C accuracy, D process) with the next number, the date and a one-line why.

## 5. Quick rules (the ones most often missed)

These are a summary. The full wording and examples are in `CLAUDE.md`.

- **Names:** always "Tyk Gateway", "Tyk Dashboard", "Tyk MDCB", "Tyk Pump", "Tyk Operator" with the Tyk prefix. Use "Tyk OAS API" (never "OAS API"), "OpenAPI document" for a file, and "OpenAPI Specification" only for the standard. Capitalize "Fragment", and the Portal entities Product, Plan and Catalogue (keep that spelling). In Tyk Operator notes write "Tyk OAuth 2.0 client management" and "TLS Certificates". Use American English and customer-facing license names (unlimited / limited node license).
- **Style:** sentence-case accordion titles. Fixed entries are 2 to 4 sentences on user impact, with no root cause narrative, internal field names, Redis keys, function names, file paths or PR numbers. Split entries with several ideas into short paragraphs. Never use the em dash.
- **Traceability:** every Changelog accordion has `{/* TT-XXXX */}` on the line after the title (comma-separated for several tickets), except the bulk "Resolved CVEs" accordion.
- **Highlights:** open with "This release". Patch releases get one generic sentence and no list of fixes. When Highlights has titled entries, there's no summary paragraph above them. Tyk Charts Highlights say which LTS it installs and the latest version it supports.
- **Always call out** new config options, env vars, fields, log fields and headers, with their default and whether they're opt-in. Also call out license-scope caveats.
- **Security fixes:** no exploit-enabling detail (triggering config, exact mechanism, every affected mode).
- **Scope:** each component's entry talks only about that component. Gateway notes never mention TIB or EDP. Dashboard notes never mention Go plugins. Breaking-change pointers say what may be affected. Don't split shared fixes by component, and don't add "upgrade both" warnings.
- **Accuracy:** don't invent values; mark them **[TO CONFIRM]**.