# Tyk Release Notes

This folder lives in `product-brain/skills/release-notes/` in the shared-claude repo.

Everything we use to write Tyk release notes (RNs): the house rules, the drafting process, and the full history of release notes for every Tyk component. Claude reads this folder before drafting or reviewing any release note, and people can use it as the style guide.

**Owner:** Joan Camos. Changes to the rules or the process should be agreed with Joan.

## What's in this folder

| File | What it is |
|---|---|
| `README.md` | This file: what the folder contains and who owns it. |
| `CLAUDE.md` | The single instruction file for writing release notes: templates, the Jira and CVE drafting process, the per-entry workflow, and every editorial rule learned so far. Claude follows it whenever it works in this folder. |
| `project-memory.md` | Everything Claude had saved in memory for this project (feedback rules, Jira and tooling knowledge, lessons learned, open initiatives), captured as one readable file. |
| `skills/tyk-release-notes/SKILL.md` | Copy of the Claude skill that runs the drafting procedure. The live version is saved in Claude; update both together. |
| `per-minor-structure-proposal.md` | Exploratory proposal (July 2026) for one release-notes page per Major.Minor line instead of one page per patch. Not adopted yet. |
| `Gateway.md` | Tyk Gateway release notes |
| `Dashboard.md` | Tyk Dashboard release notes |
| `Pump.md` | Tyk Pump release notes |
| `Sync.md` | Tyk Sync release notes |
| `Multi-data-center-bridge.md` | Tyk MDCB release notes |
| `Operator.md` | Tyk Operator release notes |
| `Charts.md` | Tyk Charts (Helm) release notes |
| `Tyk Identity Broker.md` | Tyk Identity Broker (TIB) release notes |
| `Portal.md` | Tyk Developer Portal release notes |
| `Cloud.md` | Tyk Cloud (Ara) release notes |

## How the component files are organized

There is one file per component. Each file is grouped by section, and each section lists versions newest first:

1. Breaking Changes
2. Release Highlights
3. Dependencies
4. Deprecations
5. Upgrade Instructions
6. Downloads
7. Change Log

A version only appears in a section when it has something to say there (for example, releases with no breaking changes are not listed under Breaking Changes).

On 2026-11-26 every version on the published tyk.io pages was synced into the component files word for word, back to the oldest release each page lists (only heading levels changed to fit the files). Published sections that only say there are no breaking changes or no deprecations are left out. Versions that are not on the published pages are Claude drafts: they are listed in the first lines of each file (Tyk Pump 1.15.1, Tyk Identity Broker 1.8.2, Tyk Cloud 1.33.2) and may still contain **[TO CONFIRM]** items. Pre-2022 combined release notes on the tyk-docs `archived` page are not included.

## How to use it

### Asking Claude to draft a release

Give Claude the component(s), the version and the Jira release link. The link matters: Claude takes the numeric version id from it (for example `15161` in `.../versions/15161/tab/release-report-all-issues`) and pulls the tickets by that id, which avoids the duplicate-version-name problem in Jira.

> Draft the release notes for Tyk Gateway and Tyk Dashboard 5.13.6. Jira release: https://tyktech.atlassian.net/projects/TT/versions/15161/tab/release-report-all-issues

Claude will then:

1. Read `CLAUDE.md` and the component files.
2. Pull every ticket in that Jira release, check Include in Changelog, Breaking Change, Components and ticket type, and group the tickets into entries.
3. Pull the CVEs from the CVE board for each component and version.
4. Check tickets against the "Tyk Code Claude" folder where the source is available.
5. Add the new version to each component file (`Gateway.md`, `Dashboard.md`, ...).
6. Reply with its review notes: excluded tickets and why, merged tickets, tickets still in progress, anything it couldn't verify, and every **[TO CONFIRM]**.

Useful things to add to the request when they apply:

- **Exclusions or overrides:** "don't include the MCP tickets", "treat all tickets as closed", "include TT-12345 even though it's Blocked".
- **CVE filter:** your JQL if it should be Critical/High only.
- **LTS and latest together:** "Also draft 5.16.1 from <link>, and reuse the 5.13.6 text for shared tickets."
- **Highlights format:** short (one paragraph) or long (titled entries). Claude asks if you don't say.
- **Paste-ready page:** "Give me the full 5.13.6 Dashboard page for the tyk-docs PR" to get all sections in published order.

Other requests that work well:

- "Write a changelog entry for TT-12345" (a single entry, no file change).
- "Review the Gateway 5.13.6 notes against the rules" (a check against `CLAUDE.md`).
- "Add this as a rule: ..." (new feedback goes into `CLAUDE.md`).

### Checking style

Look up a similar past entry in the component files. They are the calibration target for tone, length and level of detail.

## Content and security

- No secrets, tokens or credentials belong here.
- Draft content can describe security fixes before they are published. Follow rule B9 in `CLAUDE.md` (no exploit-enabling detail) and keep internal review notes out of these files.
