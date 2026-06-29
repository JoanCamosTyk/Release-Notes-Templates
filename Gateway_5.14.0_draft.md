## Gateway 5.14.0 Release Notes (DRAFT)

> **Note:** This is an initial draft. The ticket list for 5.14.0 is **not final**, so Release Highlights have not been written yet, and the Breaking Changes section will be extended as more tickets are confirmed.
>
> **Backport scope:** The two log-related fixes below (consistent log format, and 404 logs on a separate Control API port) ship as **bug fixes** in **5.8.15**, **5.13.1**, and **5.14.0**. The Fixed entries and the Breaking Changes notice apply to all three releases. Patch/LTS releases (5.8.15, 5.13.1) do not ship new features, so these are framed strictly as fixes. The `oauth2` security scheme addition is **5.14.0 only**.

## Breaking Changes

### 5.8.15 / 5.13.1 / 5.14.0

**Changes to Gateway application logs**

**This change does not affect you unless you specifically rely on the current Gateway log format** — for example, if your monitoring, parsing, or alerting tools are built around the existing timestamp format or the existing 404 log fields. If nothing in your stack depends on the exact log format, no action is needed.

As part of fixing an inconsistency in the application logs, the Gateway now produces a single, consistent log format across all components. The `log_format` configuration option (and the `TYK_GW_LOGFORMAT` environment variable) now accepts three explicit values:

- `text` (**new default**): plain-text logs with the new, consistent formatting described below.
- `json`: JSON logs with the same consistent formatting.
- `legacy`: preserves the previous behaviour exactly, including the historical mixed timestamp formats, the previous 404 log fields, and the `msg` log message field name.

**What changes under the new `text` and `json` formats**

- **Consistent RFC3339 timestamps.** All log entries now use the industry-standard RFC3339 timestamp format (e.g. `2024-12-12T13:59:08Z`). Previously the same log file could contain two different timestamp formats, because some internal components emitted their own format. The default changes from the custom `Jan 02 15:04:05` format to RFC3339.
- **`host` field added to 404 logs.** `Not Found` (404) log entries now include a `host` field identifying the host that received the request.
- **Log message field renamed from `msg` to `message`.** The main log message field is now `message`, aligning the Gateway with Tyk Pump and common logging conventions (such as ECS, Fluentd, and Datadog) and removing the previous inconsistency between components.

**If you are affected, you have two options:**

1. **Update your log tooling** to the new RFC3339-based format (recommended — it is consistent and aligned with industry observability standards), or
2. **Keep the previous behaviour** by setting `log_format` to `legacy` (or `TYK_GW_LOGFORMAT=legacy`), which preserves the historical timestamps, the previous 404 log fields, and the `msg` field name exactly.

## Changelog

### 5.14.0
#### Changelog
<a id="Changelog-v5.14.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Introduce new oauth2 security scheme (foundation)'>
We have introduced a new `oauth2` security scheme that lays the foundation for upcoming OAuth-related capabilities, including token introspection, Protected Resource Metadata, per-operation scope checking, and RFC 8693 token exchange.

In this release the scheme is a configurable container only. It can be declared in a Tyk OAS API definition under the API authentication configuration and switched on or off with a master `enabled` flag, and it can be saved and loaded without affecting request processing. The new scheme coexists with the existing JWT and external OAuth schemes on the same Gateway, and existing API definitions are unaffected. The capabilities that build on this scheme will arrive in subsequent releases.
</Accordion>

<Accordion title='Add API-level timeout configuration'>
You can now configure an upstream request timeout at the API level, applying a single timeout across all endpoints of an API without having to set it on each endpoint individually. The API-level timeout sits between the Gateway default (`proxy_default_timeout`) and any endpoint-level timeout: an endpoint timeout overrides the API-level value, which in turn overrides the Gateway default.

It is supported for both Tyk OAS and Tyk Classic APIs and accepts human-readable durations in milliseconds, seconds, or minutes (for example `500ms`, `1.5s`, or `2m`), from a minimum of `1ms` up to a maximum of `300s`.
</Accordion>

<Accordion title='Capture the original request path in analytics'>
The Gateway now records the original client request path — captured before any path stripping or URL rewriting — so you can tell which client-facing endpoint was called even when the request is transformed before reaching the upstream. Two new fields are added to analytics records (and forwarded to Tyk Pump): `original_path` (the full path requested by the client) and `listen_path` (the API's configured listen path). The existing `path` field is unchanged and continues to show the path sent to the upstream.

The original request path is also added to Gateway access logs as `original_path` and, when OpenTelemetry is enabled, as the `tyk.original_path` span attribute. The new fields require no configuration or migration, and older Tyk Pump and Dashboard versions are unaffected.
</Accordion>

<Accordion title='Add a configurable graceful shutdown delay'>
The Gateway can now wait for a configurable period after receiving a shutdown signal before it stops accepting new connections, giving load balancers and orchestrators (such as Kubernetes Services or AWS ALB) time to detect that the node is no longer ready and remove it from the pool. This prevents the `502` errors that could otherwise occur during scale-down in high-traffic deployments.

A new `graceful_shutdown_delay_seconds` configuration option controls the wait. As soon as a `SIGTERM`, `SIGINT`, or `SIGQUIT` is received, the readiness endpoint (`/ready`) immediately returns `503 Service Unavailable`, but the Gateway keeps accepting and serving traffic for the configured delay before beginning its existing drain. The option defaults to `0` (the previous behaviour — stop accepting connections immediately), and the existing `graceful_shutdown_timeout` still governs draining of in-flight requests once the delay has elapsed.
</Accordion>

<Accordion title='Support certificates embedded directly in the API definition'>
API definitions can now carry a PEM-encoded certificate directly in any of their certificate fields, in addition to the existing options of referencing a certificate by ID from the Tyk certificate store or by file path. This applies to client certificates, client CA certificates, custom domain certificates, and upstream (mutual TLS) certificates, for both Tyk OAS and Tyk Classic APIs.

Because the certificate is now a string field in the API definition, it can also be populated from a KV/secret reference (for example a Kubernetes secret), keeping private key material out of the management plane. Existing API definitions that reference certificates by ID or file path continue to work unchanged.
</Accordion>

<Accordion title='Add file-based KV/secret references'>
The Gateway can now resolve KV/secret references from the contents of a local file, making it straightforward to consume secrets mounted as files (such as Kubernetes secrets) on the data plane without storing the secret material in the management plane. A new `file://<path>` scheme resolves at API load time, alongside the existing `env://`, `vault://`, `consul://`, and `secrets://` schemes, and a new `$secret_file.<key>` prefix resolves at request time in the header-modification, URL-rewrite, and body-transform middleware, alongside the existing `$secret_env.`, `$secret_vault.`, and `$secret_consul.` prefixes.

Trailing newlines are stripped by default, multi-line content such as PEM certificates is preserved, and Kubernetes secret rotations are picked up on API reload.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix inconsistent application log format'>
We fixed a bug where Tyk Gateway application logs were inconsistent: the same log file could contain two different timestamp formats, because some internal components emitted their own format instead of using the Tyk logger. This made logs harder to parse, alert on, and triage.

The Gateway now produces a consistent log format across all components. As part of this fix we introduced three explicit `log_format` options (also configurable via the `TYK_GW_LOGFORMAT` environment variable): `text` (the new default, using RFC3339-compliant timestamps), `json` (RFC3339 timestamps in JSON), and `legacy` (which preserves the previous behaviour). Under `text` and `json` the main log message field is also renamed from `msg` to `message`, for consistency with Tyk Pump and common logging conventions. This is a breaking change for anyone whose monitoring or log-parsing tools rely on the previous timestamp format or the `msg` field name; set `log_format` to `legacy` to keep the previous behaviour. See the [Breaking Changes](#breaking-changes) section for details.
</Accordion>

<Accordion title='Fix missing 404 logs when the Control API runs on a separate port'>
Resolved an issue where configuring the Control API on a different port from the main Gateway listening port caused 404 (Not Found) errors from API traffic to stop appearing in the listening port's logs, showing up only in the Control API port's logs. Customers who separate these ports for security hardening lost visibility of 404 patterns on their main API traffic. Each port now records its own 404 errors independently, regardless of the Control API port configuration. In addition, 404 log entries now include a `host` field when using the `text` or `json` log formats (the `legacy` format is unchanged).
</Accordion>

<Accordion title='Fix custom keys at the minimum token length being rejected and unusable short keys silently accepted'>
We resolved two related issues in how the minimum token length (`min_token_length`, or the `TYK_GW_MINTOKENLENGTH` environment variable) is enforced. Previously, a key whose length was exactly equal to `min_token_length` was rejected at authentication time, and creating a custom-ID key shorter than `min_token_length` returned `200 OK` but produced a key that always failed with `403` on live traffic — with no indication at creation time that the key was unusable.

A key whose length equals `min_token_length` now authenticates correctly, and creating a custom-ID key shorter than `min_token_length` fails immediately with a `400 Bad Request` that states both the submitted length and the configured minimum. Autogenerated keys, the key update path, and the default configuration are unaffected.
</Accordion>

<Accordion title='Support sub-second enforced timeouts'>
Enforced upstream request timeouts are no longer limited to whole-second granularity. You can now configure sub-second timeouts using a new `duration` field that accepts human-readable values such as `500ms`, `1.5s`, or `2m` — for Tyk OAS APIs in the endpoint `enforceTimeout` configuration, and for Tyk Classic APIs in the endpoint `hard_timeouts` configuration. The previous integer-seconds fields (`value` for Tyk OAS and `timeout` for Tyk Classic) are now deprecated, but remain fully backward compatible so existing API definitions continue to work unchanged.

For Tyk OAS APIs, the Gateway automatically rounds the new duration up to the nearest second and populates the deprecated seconds field, so the API still works on older Gateways. Tyk Classic APIs are not synchronised automatically: if you set the new `duration` field, the deprecated `timeout` field must be updated manually to keep both in sync. An enforced timeout cannot exceed the Gateway's `http_server_options.write_timeout`, which remains the ultimate maximum.
</Accordion>

<Accordion title='Fix unbounded memory growth with CoProcess plugins and large sessions'>
Resolved an issue where Gateways using CoProcess plugins (such as gRPC authentication plugins) with large session objects could grow in memory until the process was terminated by the operating system, often mitigated only by scheduled restarts. The growth came from two sources: an internal regular-expression cache that could grow without limit when session access rights contained per-user URL patterns, and excessive memory churn when copying large sessions on every request. The Gateway now bounds the cache and reuses session objects to eliminate the churn.

A new `regexp_cache_max_entries` configuration option (default `5000`) caps the regular-expression cache; setting `disable_regexp_cache_bound` to `true` restores the previous unbounded behaviour for deployments with a naturally limited set of patterns. The Gateway now also aligns `GOMAXPROCS` with the container's CPU quota at startup, which is enabled by default and can be turned off with the new `disable_auto_max_procs` option (or `TYK_GW_AUTOMAXPROCS`). A related crash in the CoProcess response-hook path that could drop in-flight requests under memory pressure has also been fixed.
</Accordion>

<Accordion title='Fix Gateway crash under concurrent authentication load'>
Resolved an issue where the Gateway could crash with a `fatal error: concurrent map iteration and map write` under sustained authentication load — most often with OIDC, and also with JWT and auth-token APIs — when the local session cache was disabled or when policies modified session data at request time. The crash occurred because the same session could be read and written concurrently while it was being cloned.

Session handling is now safe under high concurrency, so these authentication flows no longer panic, and the previous workarounds (enabling the local session cache or reducing concurrency on protected endpoints) are no longer required.
</Accordion>

<Accordion title='Fix authentication headers not forwarded upstream for GraphQL APIs'>
Resolved an issue where, when a GraphQL API was configured with `strip_auth_data` set to `false`, the client's authentication header (for example `Authorization`, or a custom header such as `X-API-KEY`) was not forwarded to the upstream service. This affected all GraphQL execution modes — proxy-only (including WebSocket subscriptions), subgraph, Universal Data Graph, and supergraph — leaving upstream services that require the header unable to authenticate the request.

The active authentication method's header is now propagated upstream across all execution modes, with only the active method's header forwarded. In addition, upstream WebSocket connections for subscriptions are no longer reused across clients presenting different credentials, so each subscription carries its own authentication header and one client's token can no longer leak to another. When `strip_auth_data` is `true`, no authentication headers are forwarded, as before.
</Accordion>

</AccordionGroup>
