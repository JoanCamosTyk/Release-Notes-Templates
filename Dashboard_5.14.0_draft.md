## Dashboard 5.14.0 Release Notes (DRAFT)

> **Note:** This is an initial draft. The ticket list for 5.14.0 is **not final**, so Release Highlights have not been written yet. No breaking changes have been identified in the Dashboard tickets so far; this section should be revisited once the remaining tickets are confirmed (the Gateway log-format standardization is breaking and is tracked in the Gateway release notes).

## Breaking Changes

### 5.14.0

There are no breaking changes in the Dashboard for this release based on the tickets confirmed so far.

## Changelog

### 5.14.0
#### Changelog
<a id="Changelog-v5.14.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Add Dashboard-specific application log level control'>
The Dashboard now has its own application log verbosity setting, configurable with the `log_level` option in `tyk_analytics.conf` or the `TYK_DB_LOGLEVEL` environment variable. It accepts the same values as other Tyk components — `error`, `warn`, `info` (the default), and `debug` — and applies to all Dashboard application logs, including the internal database logger.

Previously, adjusting the Dashboard's verbosity required the global `TYK_LOGLEVEL`, which also changed the verbosity of every other component that reads that setting. The global `TYK_LOGLEVEL` still takes precedence over `TYK_DB_LOGLEVEL` when it is set, an invalid value falls back to `info` with a warning, and the `--debug` flag continues to force `debug` verbosity.
</Accordion>

<Accordion title='Add API-level timeout configuration'>
You can now configure an upstream request timeout at the API level, applying a single timeout across all endpoints of an API without having to set it on each endpoint individually. The API-level timeout sits between the Gateway default and any endpoint-level timeout: an endpoint timeout overrides the API-level value, which in turn overrides the Gateway default.

It can be configured from the API Designer and the Dashboard API, for both Tyk OAS and Tyk Classic APIs, and accepts human-readable durations in milliseconds, seconds, or minutes (for example `500ms`, `1.5s`, or `2m`), from a minimum of `1ms` up to a maximum of `300s`.
</Accordion>

<Accordion title='Capture the original request path in analytics'>
Analytics records now include the original client request path, making it possible to see which client-facing endpoint was called even when path stripping or URL rewriting transforms the request before it reaches the upstream. Two new fields are added: `original_path` (the full path requested by the client, before any transformation) and `listen_path` (the API's configured listen path). The existing `path` field is unchanged and continues to show the path sent to the upstream.

The original request path is also added to Gateway access logs as `original_path` and, when OpenTelemetry is enabled, as the `tyk.original_path` span attribute. Older Dashboard and Tyk Pump versions are unaffected by the additional fields, and no configuration or migration is required.
</Accordion>

<Accordion title='Add client IdP registry management to the Dashboard'>
The Dashboard now provides a dedicated registry for client identity providers (IdPs), storing each IdP's JWKS URI and scope-to-policy mappings separately from the API definition. New Dashboard API endpoints under `/api/clientidps` let you create, read, update, and delete registry entries and their per-API mappings.

This is the foundation for managing Dynamic Client Registration (DCR) identity providers as a first-class resource, removing the previous limit of one identity provider per API and the write conflicts caused by storing identity provider configuration inside the shared API definition. The capabilities that build on this registry are delivered across the Developer Portal, Gateway, and MDCB.
</Accordion>

</AccordionGroup>

##### Changed

<AccordionGroup>

<Accordion title='Log user and group identifiers when an inactive user is rejected'>
The Dashboard now emits a warning log when an authenticated request is rejected because the user is inactive, including the user's ID and group ID. Previously this rejection returned a 401 with no corresponding server-side log line, which made it difficult to diagnose — most often when a user's assigned group has been marked inactive, which can surface as a repeated SSO login loop. To avoid logging personally identifiable information, the entry uses the user's database ID rather than their email address. The HTTP response is unchanged.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix unintended changes to API definitions in the API Designer'>
We resolved two issues where opening or saving a Tyk OAS API in the API Designer could alter the stored API definition:

- **JWT scope-to-policy mappings preserved across upgrades.** After upgrading the Dashboard from 5.9.x or earlier to 5.10.x or later, retrieving and saving a JWT-authenticated API in the API Designer dropped its scope-to-policy ("Advanced policy-claim mapping") configuration, which could silently remove the policies applied to incoming tokens. These mappings are now preserved when an API is loaded and saved, so upgrading (or downgrading) the Dashboard no longer affects the security of existing APIs.
- **No phantom upstream proxy block.** Opening an API's settings could cause an empty upstream proxy block (proxy disabled, empty URL) to be written into the OpenAPI document on save, even when no proxy had been configured. The Dashboard no longer adds upstream proxy configuration unless it is deliberately set in the API Designer.
</Accordion>

<Accordion title='Support sub-second enforced timeouts'>
Enforced upstream request timeouts are no longer limited to whole-second granularity. You can now configure sub-second timeouts using a new `duration` field that accepts human-readable values such as `500ms`, `1.5s`, or `2m`. For Tyk OAS APIs this is set in the endpoint `enforceTimeout` configuration, and for Tyk Classic APIs in the endpoint `hard_timeouts` configuration. The previous integer-seconds fields (`value` for Tyk OAS and `timeout` for Tyk Classic) are now deprecated, but remain fully backward compatible so existing API definitions continue to work unchanged.

For Tyk OAS APIs, the Dashboard automatically rounds the new duration up to the nearest second and populates the deprecated seconds field, so the API still works correctly on older Gateways. Tyk Classic APIs are not synchronised automatically: if you set the new `duration` field, update the deprecated `timeout` field manually to keep both in sync. Note that an enforced timeout cannot exceed the Gateway's `http_server_options.write_timeout`, which remains the ultimate maximum.
</Accordion>

<Accordion title='Fix custom keys at the minimum token length being rejected and unusable short keys silently accepted'>
We resolved two related issues in how the minimum token length (`min_token_length`, or the `TYK_GW_MINTOKENLENGTH` environment variable) is enforced. Previously, a key whose length was exactly equal to `min_token_length` was rejected at authentication time, and creating a custom-ID key shorter than `min_token_length` returned `200 OK` but produced a key that always failed with `403` on live traffic — with no indication at creation time that the key was unusable.

A key whose length equals `min_token_length` now authenticates correctly, and creating a custom-ID key shorter than `min_token_length` fails immediately with a `400 Bad Request` that states both the submitted length and the configured minimum. Autogenerated keys, the key update path, and the default configuration are unaffected.
</Accordion>

<Accordion title='Fix orphaned Tyk OAS API versions not appearing in the Dashboard UI on PostgreSQL'>
Resolved a regression where child API versions that had been orphaned on Dashboard versions prior to 5.8.6 did not appear in the main API list when using PostgreSQL, and could be found only through search. A previous fix restored visibility for versions orphaned after upgrading, but versions orphaned beforehand remained hidden. The Dashboard now runs a one-time migration at startup that detects these legacy orphaned Tyk OAS APIs and restores them to the main API list, with no search filter required and without re-onboarding the APIs.
</Accordion>

<Accordion title='Fix Dashboard UI crash when an OAS parameter uses content instead of schema'>
Resolved an issue where opening a Tyk OAS API whose parameters use the OpenAPI `content` keyword (rather than `schema`) caused the API Designer and API Debugger to crash, and the Validate Request middleware form to reject the definition as invalid. The Dashboard now fully supports `content`-form parameters as defined by OpenAPI Specification 3.0: these APIs load without errors, and saving them preserves the `content` wrapper — including the media type and inner schema — without writing a conflicting top-level `schema`.
</Accordion>

<Accordion title='Fix policies disappearing for non-admin users when their APIs are deleted'>
Resolved an issue where a policy became invisible to non-admin users once all of the APIs it granted access to had been deleted, even though the policy still existed. Users with explicit Read or Write policy permissions lost the ability to see or manage these policies, while admin users were unaffected. Non-admin users can now see and manage such policies again; when ownership cannot be determined from the referenced APIs, visibility falls back to the user's policy group permissions (Read or Write grants access, Deny does not).
</Accordion>

<Accordion title='Fix inconsistent error codes for non-existent API resources'>
Resolved an issue where, with ownership enabled, a non-admin user requesting a Dashboard API resource that did not exist (for example an unknown API ID) received a 401 Unauthorized, while an admin user making the same request correctly received a 400 Bad Request. Non-admin users now receive the same error code as admin users when the requested resource does not exist. Ownership protection is unchanged: a 401 is still returned when the resource exists but the user does not own it.
</Accordion>

<Accordion title='Fix temporary files left behind after importing a multipart OpenAPI document'>
Resolved an issue where importing a multipart OpenAPI document (a ZIP archive) left its unpacked temporary files on disk after the import completed. Over time this could exhaust disk space, and because the temporary location was derived from the archive filename, a later import using the same filename could reuse stale contents and create an API from the wrong definition. Imports now use a unique temporary location per upload that is always cleaned up, even if the import fails, and concurrent uploads no longer interfere with one another. This applies to both creating and updating APIs, whether imported through the UI or the API.
</Accordion>

</AccordionGroup>
