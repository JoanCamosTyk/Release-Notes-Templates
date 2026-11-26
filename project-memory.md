# Release Notes project memory

Everything Claude had saved in its memory for the "RNs" project, captured on 2026-11-03 so it lives with the rest of the release-notes material instead of only in Claude's memory.

How this file relates to `CLAUDE.md`: the rules in `CLAUDE.md` are the authoritative version. Part 1 below lists the feedback rules as they were saved in memory, with the matching `CLAUDE.md` rule, so nothing is lost. Parts 2 to 5 are background knowledge that isn't a rule: Jira quirks, codebase access, lessons learned and open initiatives.

## Contents

1. [Feedback rules saved in memory](#1-feedback-rules-saved-in-memory)
2. [Jira and CVE board: technical details](#2-jira-and-cve-board-technical-details)
3. [Codebase and tooling access](#3-codebase-and-tooling-access)
4. [Lessons learned while drafting](#4-lessons-learned-while-drafting)
5. [Open initiatives](#5-open-initiatives)
6. [Rule history](#6-rule-history)

---

## 1. Feedback rules saved in memory

| Rule (as saved in memory) | In `CLAUDE.md` |
|---|---|
| Capitalize asset names such as "Fragment" / "Fragments" (2026-10-27). | A7 |
| Always prefix component names with "Tyk": Tyk Gateway, Tyk Dashboard, Tyk MDCB, Tyk Pump, never the bare name (2026-10-27). | A8 (and A5 for Tyk Operator) |
| Tyk Developer Portal entity names are capitalized when they mean the Portal concept: Product, Plan, Catalogue. "Catalogue" keeps its British spelling as a proper noun. Portal notes only (2026-08-19). | A4 |
| Tyk Operator naming: "Tyk OAuth 2.0 client management" and "TLS Certificates" (capital C, even in sentence-case titles) (2026-09-23). | A6 |
| Never use the em dash anywhere: release notes, titles, review notes, Jira text, chat. Use a colon, comma, parentheses or separate sentences (2026-10-13). | B10 |
| Patch releases: Release Highlights use one generic sentence ("This patch implements several changes and fixes to improve the stability and correct the behavior of the Dashboard."), never a list of fixes (2026-08-20). | B5 |
| Release Highlights with titled entries (latest minor releases such as 5.16.0) start directly with the first bold title. No summary paragraph above the entries (2026-10-27). | B14 |
| Split entry descriptions with more than one idea into several short paragraphs instead of one dense one (2026-08-20). | B7 |
| Dashboard release notes never mention Go plugins (for example "Go plugins must be built using Go 1.26"). Go plugins are Gateway artifacts. This reversed the 2026-08-20 rule (2026-10-14). | B6 |
| Tyk Charts Highlights always say: "This version installs the most recent Tyk LTS release [X] but includes enhancements to support features available up to Tyk Y. You must modify the charts to install the desired version if not using LTS." Link the LTS Dashboard anchor and place it before "For a comprehensive list of changes..." (2026-09-15, wording refined 2026-09-23). | B8 |
| Security-related Fixed entries must not give exploit-enabling detail (triggering config, exact mechanism, every affected mode). Example: the Gateway 5.15.0 GraphQL isolation fix (2026-09-18). | B9 |
| Gateway release notes never mention TIB or EDP, including the Compatibility Matrix and any [TO CONFIRM] text (2026-10-13). | B11 |
| Release readiness / release prep tickets never become Changelog or Highlights content, for example the "Updated embedded Tyk Identity Broker to 1.8.1" accordion built from TT-18592 in Portal 1.20.0 (2026-10-14). | B12 |
| Each component's entries describe only that component (the Dashboard CSRF fix TT-18522 must not mention Developer Portal). Preferred CSRF wording: "If you serve the Dashboard over plain HTTP, you can set the new configuration option `security.csrf_allow_missing_origin` to `true` (default `false`). This lets browser requests without origin information through the origin check, while the CSRF token is still validated. CSRF failure log entries now include more detail to help with troubleshooting." (2026-10-14) | B13 |
| Don't end a Changelog entry with a generic "This is a breaking change. See Breaking Changes." Say what may be affected, for example "This may affect existing API definitions and webhooks. See [Breaking Changes](#breaking-changes-5.13.4) for details." (2026-10-14) | B13 |
| Anything named in Release Highlights (a feature or a deprecation) must also have its own Changelog or Deprecations entry. Found on Pump 1.17.0 (`kv://` / `$kv{}` support and the `PMP_MONGO*` deprecation) (2026-08-19). | C3 |
| The Confluence page [Distro OS, KV Store, and Dependency Versions Being Tested In Tyk Pipeline](https://tyktech.atlassian.net/wiki/spaces/CVBGF/pages/4499079175/Distro+OS+KV+Store+and+Dependency+Versions+Being+Tested+In+Tyk+Pipeline) is the source of truth for every Dependencies table. Whenever a release or ticket says VALIDATED, check whether this page needs updating or referencing (2026-10-22). | C6 |
| CVEs always come from the Jira `CVE` project, filtered by Affected Component, Fix Version and CVE Severity, never from a component's own tickets. Standing default excludes only Low (`cf[11053] != "Low"`). Tyk Cloud never gets a CVE list, only "Addressed several CVEs within dependencies." in Highlights (2026-07-28). | D2, D3 |
| Critical/High-only CVE lists: Joan supplied explicit Critical/High JQL (`"cve severity [dropdown]" IN ("High ", Critical)`) for 5.13.2 (2026-08-13) and 5.13.3 + 5.15.1 Gateway/Dashboard (2026-09-28); 5.15.0 used it too. Apply it whenever Joan provides that JQL and ask if unsure. It is not yet declared the permanent rule. | D5 |

CVE list details from 2026-09-28 (5.13.3 / 5.15.1): Gateway had 18 CVEs, Dashboard 16 (Dashboard lacks CVE-2026-76905 and CVE-2026-77354, which are Gateway and Portal only), identical between 5.13.3 and 5.15.1. Intro sentence used: "We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:". Critical first, then High, NVD links. The 2026-09-11 drafts (11 CVEs, Low excluded, via TT-18285 links) were outdated by this.

---

## 2. Jira and CVE board: technical details

Site `tyktech.atlassian.net`, cloud ID `c25a3295-62f6-4d5a-8ddd-58122b144a37`.

**Projects and ticket types**
- Product tickets live in project **TT** ("Tyk Product Development"). CVEs live in project **CVE** ("CVE Repository"), issue type "CVE", linked to TT tickets with the "Fixed" / "Fixes" link type.
- Per-release CVE roundups are TT tickets of type **"Activity (Release Activity)"** named "<Component> CVEs" (for example TT-17795 Gateway and TT-17796 Dashboard for 5.13.2/5.15.0; TT-18285 / TT-18286 for 5.13.3/5.15.1). They list every linked CVE regardless of severity, so use them only as a completeness cross-check. The Security Fixes list still comes from the severity-filtered CVE-board JQL.
- Roundup / Activity tickets and "{Release Prep} Tyk X.Y.Z (<Component>)" checklist tickets are internal. Exclude them from the changelog even though they appear in a fixVersion pull (seen on Charts 5.4.0 TT-17864, Operator 1.5.0 TT-17788 and TT-17865).

**Fix versions**
- Names carry a prefix: `"Tyk 5.13.2"`, `"Tyk Charts 5.4.0"`, `"Tyk Operator 1.5.0"`. A bare `"5.13.2"` returns nothing.
- Jira can have two version records with the same name (two "Tyk 5.13.3": ids `14784` and `14860`; two "Tyk 5.15.1": `14785` and `14859`). A name-based pull then mixes in tickets from another release wave. Re-run with the numeric id, taken from the `fixVersions` array or the release-report URL Joan shares (`.../versions/<id>/tab/...`), and cross-check against the release's own roundup ticket.
- Versions are per project: the TT "Tyk 5.16.0" is id `14454`, the CVE project's "Tyk 5.16.0" is id `14786`. Use the TT id for TT tickets and the name-based JQL Joan supplies for the CVE board.
- One CVE ticket can carry several fix versions at once, so close-together releases often share CVE lists. Still query each release.

**Custom fields**
- `customfield_10335`: Include in changelog (Yes/No). `null` means unset, which is not the same as an explicit No; unset tickets are often superseded by another ticket. Always check the field on every ticket in the pull, including ones Joan didn't name (TT-18002 on Operator 1.5.0 was explicitly No and nobody had mentioned it).
- `customfield_10684`: "Breaking Change " (option labels have trailing spaces, so compare trimmed values).
- `customfield_11122`: CVE Affected Component (multi-select).
- `customfield_11053`: CVE Severity. Option labels have inconsistent trailing spaces (`"High "`, `"Medium "`, `"Critical"`), which is why the default filter uses `!= "Low"`. The structured field can disagree with the free-text "Severity:" line in the description (CVE-310 / CVE-2026-14456: field High, text Medium). Always use the structured field.

**Search tool behavior**
- `searchJiraIssuesUsingJql` sometimes caps at 5 issues per call regardless of `maxResults`, and sometimes returns everything (all 78 TT tickets for 5.16.0 in one call; all 18 for Operator 1.5.0). The cap seems tied to response size. Try a fields-scoped query first and check `pageInfo.hasNextPage` and the count. If it caps, page with `AND key > "<exact last key returned>"` (key order is numeric on the suffix).
- The `fields` parameter doesn't reliably suppress `description`, so long tickets can overflow and be saved to a file. Parse that file with `python3 -c "import json..."` instead of the Read tool.
- The Atlassian Rovo connector had an extended outage on 2026-08-13 (timeouts for several minutes) and recovered by itself. Keep retrying periodically and do codebase work meanwhile.

**Epics, superseded tickets and grouping**
- Feature grouping is often signalled by issue links rather than an epic field: "Polaris work item link" with "is implemented by" / "implements" between an old story and its replacement (TT-9550 arm64 build was superseded by TT-17641 -ng plugin compiler).
- When Joan gives an explicit bundling or status override ("treat as closed", "TREAT ALL THE TICKETS AS CLOSED", force-include a Blocked ticket), apply it at face value and say plainly in the review notes which tickets' real Jira status it covers. Don't do this on your own initiative.
- An excluded epic is still useful context. Operator 1.5.0 epic TT-17504 (declarative management of Dashboard resources) had 7 child stories, each a distinct CRD (TykKey, TykCertificate, TykIdentityProfile, TykOAuthClient, TykWebhook, TykUserGroup, plus TT-17503): kept as separate accordions (matching past Operator releases) and tied together under one Highlights subheading. Pattern: distinct sub-features get separate accordions; phases of one indivisible feature get one accordion (as with TT-17172/73/74 OAuth 2.0 in `Gateway.md`).
- Feature-cluster entries (5.16.0): Joan asked for one accordion per multi-ticket feature (API Fragments epic TT-17450; Dashboard OTel metrics epic TT-18193) even when most child tickets were Include = No. Fold their customer-facing details into the umbrella entry and list every ticket in its `{/* */}` comment.
- When Joan says to fold a specific-version follow-up into a general one (Operator 1.5.0: "just mention Go 1.26", not Go 1.26.7 from TT-18096), keep both ticket numbers in the traceability comment but write the prose at the level she asked for.

**Missing components**
- "Raise any tickets missing components" means adding a Jira comment on each ticket with an empty Components field for its owner to fix, not only noting it in the review notes. Done on 5.13.2 (TT-17862, TT-17863). On 5.15.0 they were only noted: TT-16268, TT-17574 and TT-17806 still need the Jira comment.

---

## 3. Codebase and tooling access

**Local "Tyk Code Claude" folder (primary source since 2026-08-05)**
- Extracted source snapshots, each nested one level deeper than its folder name (for example `tyk-master/tyk-master/`): `tyk-master` (Gateway), `tyk-analytics-ui-master` (Dashboard frontend only, no backend Go), `tyk-pump-master`, `tyk-sink-master` (Sync), `tyk-operator-internal-master`, `portal-master`, `tyk-identity-broker-master`, `tyk-charts-main`.
- Not available anywhere: the Dashboard backend (`tyk-analytics`, private) and Tyk Cloud (Ara). Those claims stay ticket-text-only.
- All snapshots share one extraction date (zip mtime 2026-08-05). Staleness is folder-wide, so ask Joan to refresh the whole folder at once.
- A "Closed" ticket can still be missing from the snapshot: compare the latest QA or merge comment date with the zip mtime before treating a code-search miss as a problem (TT-15287 and TT-16572 on Charts 5.4.0 were QA'd 2026-08-11 to 08-14, after the snapshot). When a whole release postdates the snapshot (Operator 1.5.0's 7 new CRD type files), say so once as a blanket statement in the review notes.
- CVE-only patch releases (5.13.3 / 5.15.1) need no source check: the fixes are third-party dependency and OS-level updates.

**GitHub fallback**
- `tyk` (Gateway), `tyk-pump` and `tyk-sync-internal` are public and clonable with `git clone`. `raw.githubusercontent.com` is blocked by the sandbox allowlist, but `github.com` smart-HTTP works.

**Device-folder tool quirks**
- If `device_stage_files` fails with "HTTP 400 adding session file", read the file with `device_bash cat "<path>"` instead of retrying.
- Large recursive `grep` / `find` over a whole repo through `device_bash` can time out. List directories first and scope commands to known subdirectories.
- To deliver a file built in the cloud workspace: `SendUserFile` to get a `file_uuid`, then `device_commit_files` with the target path (pass `expectedMtimeMs` when overwriting an existing file).
- When editing large Markdown files on the device, line-number-based Python edits are more reliable than string-anchor replacement (an anchor that looked identical failed silently on 2026-09-11).

**Confluence**
- `getConfluencePageDescendants` can 404 on a folder-shaped URL id. Use `searchConfluenceUsingCql` with `title ~ "..."` instead.

---

## 4. Lessons learned while drafting

**LTS and latest releases drafted together (2026-08-13, 5.13.2 with 5.15.0)**
1. Draft the LTS release first.
2. Tag every ticket in the latest release against the LTS ticket list.
3. Reuse the LTS prose verbatim for shared tickets. A second synthesis pass drifts in wording.
4. If research shows the LTS text is wrong or incomplete, flag it to Joan so both drafts can be updated together.
5. Say explicitly whether any divergence was found ("no divergence found" is a normal outcome).
6. CVE lists may also be identical; a single roundup ticket can carry both fix versions (TT-18285 for Gateway 5.13.3 / 5.15.1).

**Named exclusion lists**
- A named list can be only part of a larger feature. On 5.15.0, Joan named 6 MCP wizard tickets to exclude; 5 more Gateway-side tickets of the same feature were in the pull. Search by theme and ask before drafting. Joan confirmed excluding the whole cluster.
- Small releases tend to get per-ticket instructions instead ("add TT-17018 even though Blocked", "don't worry about TT-17864").
- A named list is not a substitute for checking the Include field on every ticket.

**Tickets that disappear between drafts**
- TT-431 and TT-1860 were in the 2026-08-05 Gateway 5.13.2 draft but not in the 2026-08-13 pull; direct fetches showed `fixVersions: []`. Verify with a per-ticket fetch and flag the change rather than re-adding or silently dropping content.

**Shipped behavior vs ticket text**
- QA comments can contradict acceptance criteria. TT-16572 (Charts 5.4.0): AC said `terminationGracePeriodSeconds: 0` is a valid override; shipped code treats `0` as unset. Write what shipped.

**Large pulls**
- For 80+ ticket releases, delegate in parallel: one subagent pulls and condenses all tickets to a JSON file (key, summary, status, components, flags, short synthesis, theme tags) and returns only a summary; separate subagents for each CVE pull and for Confluence research. Keep editorial judgment (especially LTS text reuse) in the main session.
- Releases with roughly 20 tickets or fewer (Charts 5.4.0 had 5, Operator 1.5.0 had 18) and CVE-only patches are fine inline.

---

## 5. Open initiatives

**Automating the release-notes workflow (raised 2026-07-28)**

Goal: move away from browsing Jira ticket by ticket, pasting into chat, and opening tyk-docs PRs by hand. Proposed phases:
1. Jira integration through the Atlassian Rovo connector to pull a fix version with its Include in Changelog, Components, Breaking Change and Ticket Type fields. Done: this is now the standard process.
2. Codebase access to verify component tagging and exact config / env var names against shipped code (rule C2). Done through the local "Tyk Code Claude" folder; the Dashboard backend is still missing.
3. AI-assisted grouping of tickets into accordion entries using the grouping rules. Needs a human review checkpoint because grouping is judgment-heavy.
4. Automated draft generation of all RN sections using the rules in `CLAUDE.md`.
5. Automated PR creation in tyk-docs (branch from main, title convention, labels: Release Notes, Review Required, component label). Not started.

The external template repo `JoanCamosTyk/Release-Notes-Templates` is where this folder's content came from; treat this folder as the working copy of that knowledge.

**One release-notes page per Major.Minor line (raised 2026-07-29)**

Exploratory proposal, not adopted: one page per minor line (for example "Release 5.8") updated in place for each patch until the next minor opens a new page. Proposed per-section rules: Release Highlights replaced each patch; Breaking Changes and Deprecations accumulated and tagged with the patch; Compatibility / Dependencies form still to be decided (current matrix only, or per-patch history); How to Upgrade replaced; Changelog accumulated with one dated sub-section per patch, newest first. A worked example with real 5.8.13 / 5.8.14 / 5.8.15 Dashboard content was made for the Commercial and Support teams; see `per-minor-structure-proposal.md`. Open questions: Dependencies table form, Downloads placement, page lifecycle and redirects at 5.9.0.

---

## 6. Rule history

Rule file first generated from session memory on 2026-05-26 and restructured on 2026-07-22. Jira / CVE-board sourcing added 2026-07-28. CVE severity flag, scope verification and Jira pagination notes added 2026-08-13, followed the same day by the LTS + latest consistency rule, feature-cluster exclusion scoping, subagent delegation and device-folder fallbacks (drafting 5.15.0). C3 (Highlights vs Changelog / Deprecations) and A4 (Portal capitalization) added 2026-08-19. B5 (patch Highlights), B6 (Go plugin scoping, later reversed on 2026-10-14) and B7 (multiple paragraphs) added 2026-08-20. C4 (legacy env-var table parity) added 2026-08-31. C5 (QA comments vs ticket description) added 2026-09-07. D11 (duplicate fix-version names) added 2026-09-11. B8 (Charts Highlights versions) added 2026-09-15 and reworded 2026-09-23. A5 ("Tyk Operator") added 2026-09-16. B9 (security redaction) added 2026-09-18. A6 (Operator feature names) added 2026-09-23. B10 (no em dash) and B11 (Gateway: no TIB / EDP) added 2026-10-13, D12 the same day. B12 (release readiness tickets) and B13 (component scope, breaking-change pointers) added 2026-10-14. C6 (Confluence dependency page) added 2026-10-22. A7 (Fragment), A8 ("Tyk" prefix) and B14 (no summary above titled Highlights) added 2026-10-27. On 2026-11-03 the folder was restructured: one instruction file (`CLAUDE.md`), one file per component, and this memory capture.
