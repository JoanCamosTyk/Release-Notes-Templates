## Sync 2.2.0 Release Notes (DRAFT)

## Release Highlights

### 2.2.0
This release adds first-class support for Client IdP registry entries to Tyk Sync, so the JWKS URIs and scope-to-policy mappings used for Dynamic Client Registration can be exported, version-controlled, and promoted across environments alongside your APIs and policies.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.2.0) below.

## Change log

### 2.2.0
#### Changelog
<a id="Changelog-v2.2.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Add Client IdP support to dump, publish, and sync'>
Tyk Sync can now manage Client IdP registry entries as a first-class resource, alongside APIs, policies, and other assets. Previously these entries — which hold the JWKS URI and scope-to-policy mappings used for Dynamic Client Registration — existed only in the Dashboard and were invisible to GitOps workflows, so they could not be version-controlled or promoted between environments.

`tyk-sync dump` now exports each Client IdP to its own `clientidp-{id}.json` file and records them under a new `client_idps` section in `.tyk.json`, with a `--idps` flag to export a specific subset. `tyk-sync publish` (and `update`) recreate them on the Dashboard, and `tyk-sync sync` reconciles the full create, update, and delete lifecycle. Required fields (`org_id`, `name`, and `jwks_uri`) are validated before any request is sent, and Dashboard versions that do not support the Client IdP registry are handled gracefully so existing workflows continue to run unchanged.
</Accordion>

</AccordionGroup>
