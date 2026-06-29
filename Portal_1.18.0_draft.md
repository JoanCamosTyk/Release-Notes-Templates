## Portal 1.18.0 Release Notes (DRAFT)

> **Note:** This is an initial draft. The ticket list for 1.18.0 is **not final**, so Release Highlights have not been written yet. The three Fixed entries below were drafted earlier and are provisionally placed in 1.18.0 — move them if they belong to a different release.

## Release Highlights

### 1.18.0

_To be written once the ticket list is confirmed._

## Change log

### 1.18.0
#### Changelog
<a id="Changelog-v1.18.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Add support for the client IdP registry in Dynamic Client Registration flows'>
The Developer Portal can now store the identity provider configuration it generates during Dynamic Client Registration (DCR) — the JWKS location and the scope-to-policy mappings — in the new centralised client IdP registry, instead of writing it directly into the shared API definition. This removes write conflicts between the Portal and the Dashboard, allows a single API to be associated with more than one identity provider, and ensures the configuration is cleaned up automatically when a plan or product is deleted (previously it could be left orphaned). A new optional `scope_claim_name` field in the DCR configuration lets you specify which token claim carries the scopes; when it is omitted, the default claim `scope` is used.

This behaviour is disabled by default and controlled by the new `enable_idp_registry` configuration option. Enable it only once you are running versions of Tyk Gateway and Dashboard that support the client IdP registry. Migration is incremental and requires no backfill: existing registrations continue to work unchanged, and only new approvals are written to the registry.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix error when searching Teams by name or organisation in Portal Admin'>
Resolved an issue where searching on the Teams page (API Consumers > Teams) in Portal Admin returned a database error whenever the search term was not numeric, such as a team or organisation name. Team search now works for both numeric IDs and text: a numeric value matches teams by ID, text matches team names with a case-insensitive substring search, and non-numeric input is handled safely instead of producing an error. The organisation filter is unaffected.
</Accordion>

<Accordion title='Fix Catalogues field missing from the Portal API get-plan response'>
Resolved an inconsistency in the Developer Portal API where retrieving a single plan (`GET /portal-api/plans/{plan_id}`) omitted the `Catalogues` field, even though that field can be set when a plan is created. The response now includes a `Catalogues` field listing the IDs of the catalogues associated with the plan, so plan-to-catalogue associations can be verified directly without extra API calls.
</Accordion>

<Accordion title='Fix NameWithSlug field missing from the Portal API get-catalogue response'>
Resolved an inconsistency in the Developer Portal API where retrieving a single catalogue (`GET /portal-api/catalogues/{id}`) did not return the `NameWithSlug` field, even though the list-catalogues endpoint did. The get-catalogue-by-ID response now includes `NameWithSlug`, matching the list endpoint and removing the need for workarounds when associating Portal assets.
</Accordion>

</AccordionGroup>
