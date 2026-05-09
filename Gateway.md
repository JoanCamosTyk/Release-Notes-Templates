## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 5.12.1
Tyk Gateway has been updated to Golang 1.25 and Debian 13 (Trixie) for enhanced security and performance, including updated FIPS-compliant images. This release addresses multiple CVEs in dependent libraries and fixes a path matching inconsistency for Tyk OAS APIs.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.12.0
**OpenAPI Specification 3.1 is now supported** 
In this release, we are delighted to bring initial support for OAS 3.1 in Tyk OAS APIs covering:
- Import and validation of OpenAPI 3.1 descriptions using Tyk Dashboard to create Tyk OAS APIs
- OAS 3.1 features:
    - Full JSON Schema Support and $schema keyword
    - The single example keyword is deprecated in OAS 3.1
    - type can be an array
    - exclusiveMinimum and exclusiveMaximum keywords
We do not yet have support for all new features. For more details, see the documentation

**Enhanced OpenTelemetry Tracing and Log Correlation**
In this release, we’ve significantly improved observability by bridging the gap between logs and distributed traces. When OpenTelemetry is enabled, Tyk Gateway now automatically injects W3C trace IDs into access logs, and both trace and span IDs into application logs.
This allows your DevOps and SRE teams to seamlessly correlate Gateway operational events with distributed traces across platforms such as Grafana Tempo, Jaeger, and OpenSearch, providing full visibility into the request journey.
Additionally, we’ve introduced flexible support for custom trace headers. If your organization uses custom correlation ID systems (like X-Correlation-ID), the Gateway can now recognize these as trace context sources. With multiple propagation modes, you can gradually migrate to standard OpenTelemetry tracing without modifying existing downstream systems.

**Enhanced Error Observability in Access Logs**
Troubleshooting API errors in production just got significantly faster. We’ve enhanced Gateway access logs to include rich, structured error context for 4XX and 5XX errors, eliminating the need to cross-reference multiple log sources during an incident.
Users can now instantly identify the root cause of failures—whether it’s an expired TLS certificate, a network connectivity issue, or a backend service problem—directly from the access logs. This comprehensive visibility drastically reduces time-to-resolution and simplifies debugging.

**Programmatic Configuration Inspection for Faster Troubleshooting**
Verifying configuration settings and debugging deployment issues can be time-consuming when multiple configuration sources (files, environment variables, defaults) are involved.
To streamline troubleshooting, we’ve introduced configuration inspection endpoints to the Tyk Gateway API. Platform engineers and support teams can now programmatically access the Gateway’s actual runtime configuration directly through the control API. This eliminates the need for manual configuration file sharing and supports automated drift detection, while built-in redaction automatically protects sensitive data like passwords and secrets.

**Enhanced Security with Client Certificate-Token Binding**
To provide an additional layer of security for your APIs, we’ve introduced Client Certificate-Token Binding. This feature allows you to form a strict binding association between an Auth Token issued to a client and their specific client certificate.
By ensuring that a token can be used only with its bound certificate, you can significantly reduce the risk of token theft or misuse. The feature fully supports certificate rotation scenarios by allowing multiple certificates to be bound to a single key, ensuring uninterrupted access during credential updates.

**Certificate Authentication as a Standalone Auth Method for Tyk OAS**
We have restructured Certificate Authentication (formerly known as Dynamic mTLS) to be a dedicated, standalone authentication method in Tyk OAS API definitions.
Previously configured as an adjunct to Auth Token authentication, this change aligns Certificate Authentication with other Tyk proprietary methods like HMAC and Custom Auth. This improves API design consistency and makes it much more intuitive to configure certificate-based access, all while maintaining full backward compatibility with your existing API definitions.

**Optimized Redis Storage for Data Planes**
We have significantly reduced Redis memory consumption for Data Plane deployments, delivering immediate storage cost savings and improved efficiency for large-scale environments.
By implementing intelligent storage optimization, the Gateway now automatically omits empty fields when storing session data, reducing memory usage for typical API keys by up to 20%. Additionally, we’ve introduced optional compression for cached API definitions, reducing storage requirements by up to 75% without impacting API response times. These enhancements are fully backward compatible and require no migration of existing keys or definitions.

For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.11.1
In this release, we have fixed a memory leak that could occur when using JWT authentication; we have resolved a performance issue with bundle verification that significantly impacted resource consumption when using plugin bundles; and we have fixed some priority CVEs. For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.11.0
Tyk 5.11 delivers security enhancements and deeper operational visibility - empowering teams to scale their API programs with confidence and efficiency.

**Strengthened API Security & Authentication** This release advances our security foundation with enhanced JWT authentication capabilities. Teams can now leverage scope-to-policy mapping without requiring default policies, while new support for nested claims enables more granular policy and subject identification. We’ve also added IP spoofing protection through configurable depth selection in X-Forwarded-For headers.

**Advanced Operational Control** Operations teams gain greater flexibility with the ability to temporarily remove targets from upstream load balancers during maintenance windows. Enhanced observability comes through OTel trace propagation to custom gRPC plugins, trace ID inclusion in API traffic logs, and dedicated Gateway latency metrics alongside upstream measurements. Data Plane Gateways now recover more quickly from interruptions to the MDCB link to the Control Plane.

**Enhanced Stability & Performance** This release includes important stability improvements, resolving crash conditions in JWT authentication and concurrent processing scenarios, eliminating blocking operations that could cause significant delays during MDCB connectivity issues, and improving performance for OAuth key retrieval in hybrid deployments. These fixes collectively deliver a more reliable and responsive API gateway experience for enterprise environments.

These enhancements collectively strengthen Tyk’s position as the platform of choice for organizations requiring enterprise-scale API management with robust security, operational excellence, and developer productivity.

For a comprehensive list of changes, please refer to the detailed changelog.

### 5.10.0
For a comprehensive list of changes, please refer to the detailed changelog.

OpenAPI Compliant Multi-Authentication for Tyk OAS APIs

Tyk Gateway now supports true OpenAPI specification compliant authentication workflows, giving developers the flexibility to implement industry-standard security patterns while maintaining backward compatibility.

OpenAPI compliant authentication brings:

- **Multiple authentication paths:** Process all entries in the OpenAPI security section, not just the first one
- **Flexible security combinations:** Enable authentication scenarios like “OAuth2 OR Auth Token” where clients can choose their preferred method
- **Proprietary method integration:** Seamlessly combine standard OpenAPI authentication with Tyk’s proprietary methods (Custom Authentication plugin, HMAC) using the same flexible logic
- **Standards compliance:** Follow OpenAPI security specification patterns that developers expect

Backward compatibility guaranteed:

- **Legacy mode preserved:** Existing APIs continue to work unchanged with the current AND-only logic
- **Opt-in enhancement:** Switch to compliant mode via the securityProcessingMode configuration when ready
- **No breaking changes:** Existing multi-security configurations remain functional

Real-world applications

- Support diverse client authentication capabilities within the same API
- Implement progressive authentication strategies (basic → advanced security)
- Align with OpenAPI tooling and documentation expectations
- Reduce integration complexity for API consumers

Perfect for organizations wanting to leverage standard OpenAPI security patterns while maintaining the flexibility of Tyk’s advanced authentication features.

For more details, please see the dedicated Multi Auth section.

Comprehensive JWT Claim Validation for Tyk OAS APIs

Tyk Gateway now provides enterprise-grade JWT validation capabilities exclusively for Tyk OAS APIs, enabling complete control over token validation beyond basic expiry and signature checks.

Complete registered claim validation
- Multi-Identity Provider support: Validate issuer, audience, and subject claims against multiple allowed values
- Flexible claim mapping: Configure different claim names for subject, policy, and scope mapping to support various Identity Providers (Keycloak, Okta, Auth0, etc.) within the same API
- JWT ID enforcement: Require unique token identifiers for enhanced security

Advanced custom claim validation

- Flexible validation rules: Define validation for any JWT claim using required, exact match, or containment rules
- Rich data type support: Handle strings, numbers, booleans, and arrays with nested claim access using dot notation
- Non-blocking validation: Monitor claim compliance without rejecting requests, perfect for gradual policy enforcement

Real-world applications
- Role-based access control with custom permission claims
- Department or organization-based API access restrictions
- Multi-tenant scenarios with flexible claim validation
- Gradual migration from legacy authentication systems

This enhancement makes Tyk’s JWT middleware the primary validation mechanism for complex enterprise authentication scenarios, providing the flexibility needed for modern Identity Provider integrations while maintaining backward compatibility.

Ideal for organizations that require sophisticated JWT validation beyond standard token checks.

For more details, please see the dedicated JWT Auth section.

Advanced JWKS Cache Management for Tyk OAS APIs

Tyk Gateway now provides comprehensive JWKS (JSON Web Key Set) cache control for Tyk OAS APIs, delivering significant performance improvements and operational flexibility for JWT validation workflows with:

- Configurable cache timeouts: Set custom cache durations per Identity Provider to match their key rotation schedules
- On-demand cache invalidation: Instantly refresh cached keys for any API (Classic or OAS) when Identity Providers rotate their signing keys
- Intelligent pre-fetching: Eliminate first-request latency by fetching JWKS data during Tyk OAS API initialization

Key benefits
- Faster JWT validation with reduced Identity Provider round-trips
- Zero cold-start delays for JWT-protected endpoints
- Immediate response to Identity Provider key rotations
- Better performance in high-traffic JWT validation scenarios

This enhancement is particularly valuable for organizations migrating to Tyk OAS APIs or those requiring consistent low-latency JWT validation performance with multiple Identity Providers that have different key rotation policies.

For more details, please see the JWT Auth section.

Centralized External Service Configuration

Tyk Gateway now provides unified configuration for all external service connections through the new external_services section. This enhancement brings together previously scattered and incomplete configuration options into a single, coherent system that supports:
- Proxy configuration: Apply proxy settings globally or per service, with automatic support for standard environment variables (HTTP_PROXY, HTTPS_PROXY, NO_PROXY)
- mTLS certificate management:Centralized certificate configuration for secure connections to external services
- Comprehensive service coverage: Covers all external integrations, including databases, OAuth providers, and webhook endpoints

This improvement simplifies deployment in enterprise environments where proxy servers and certificate management are critical, while maintaining full backward compatibility with existing configurations.

Key benefits
- Reduced configuration complexity and duplication
- Better security through centralized certificate management
- Simplified proxy configuration for containerized deployments
- Consistent external service connection handling across all Tyk components

For more details, please see the dedicated section.

Proactive Certificate Expiry Monitoring

Tyk Gateway now automatically monitors certificate health and proactively alerts administrators before certificates expire, helping prevent service outages caused by expired mTLS certificates.

The new certificate monitoring system provides:

- Early warning notifications: Configurable alerts when certificates approach expiry (default: 30 days)
- Immediate expiry detection: Real-time notifications when expired certificates are detected in use
- Comprehensive coverage: Monitors certificates used in both client-to-Gateway and Gateway-to-upstream connections
- Smart throttling: Built-in cooldown mechanisms prevent alert flooding while ensuring visibility

These events integrate seamlessly with existing monitoring and alerting systems through Tyk’s standard event framework, enabling teams to set up automated workflows for certificate renewal and replacement.

Key benefits

- Prevent unexpected API outages due to expired certificates
- Reduce manual certificate monitoring overhead
- Enable proactive certificate lifecycle management
- Improve overall API reliability and uptime

Perfect for organizations managing multiple certificates across complex API infrastructures where manual tracking becomes impractical.

For a comprehensive list of changes, please refer to the detailed changelog.

## Change Log

### 5.12.1
##### Changed
<AccordionGroup>

<Accordion title='Updated Golang version to 1.25'>
The Tyk Gateway has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

<Accordion title='Update Docker images to Debian 13 (Trixie)'>
Updated the Docker images for Tyk Gateway to Debian 13 (Trixie) to address multiple vulnerabilities in the underlying operating system.
</Accordion>


</AccordionGroup>

##### Fixed
<AccordionGroup>

<Accordion title='Fixed path matching inconsistency for Tyk OAS APIs'>
Resolved an issue where parameterized paths could incorrectly take precedence over static paths when using the Request Validation or Mock Response middleware in Tyk OAS APIs. Static paths will now correctly bypass these middleware if not explicitly configured, restoring the expected routing behavior.
</Accordion>
</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security vulnerabilities:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15281" target="_blank">CVE-2025-15281</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-0861" target="_blank">CVE-2026-0861</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-0915" target="_blank">CVE-2026-0915</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25679" target="_blank">CVE-2026-25679</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32285" target="_blank">CVE-2026-32285</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32286" target="_blank">CVE-2026-32286</a>
- <a href="https://www.cvedetails.com/cve/CVE-2026-33186/" target="_blank">CVE-2026-33186</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34986" target="_blank">CVE-2026-34986</a>

</Accordion>

</AccordionGroup>

### 5.12.0
#### Changelog
<a id="Changelog-v5.12.0" data-scroll-offset></a>

##### Added
<AccordionGroup>

<Accordion title='Added support for OpenAPI 3.1 (OAS v3.1.x)'>
This release introduces initial support for importing and validating OpenAPI 3.1 descriptions to create Tyk OAS APIs. The implementation maintains backward compatibility with OAS 3.0 while adding support for the new JSON Schema 2020-12 validator:

- Added full JSON Schema support, including the `$schema` keyword.
- Added support for defining `type` as an array (e.g., `["string", "null"]`).
- Support for `exclusiveMinimum` and `exclusiveMaximum` 

Please note the following limitations in this initial release:
- Reusable Path Item Objects and the new `mutualTLS` security scheme are not currently supported.
</Accordion>

<Accordion title='Optimized Data Plane Redis storage for sessions and API definitions'>
This release introduces significant storage optimizations for Data Plane Redis caching, reducing memory consumption while maintaining full backward compatibility:

- *Session Object Optimization*: The Gateway now automatically omits empty and zero-value fields when storing API keys and session objects. This results in up to a 20% reduction in memory usage, with minimal keys now consuming only 500-700 bytes.
- *API Definition Compression*: Added optional Zstd compression for API definitions cached in Redis, achieving up to 75% storage reduction. Compression and decompression occur during Gateway reloads, ensuring zero impact on the request hot path.
- *Configuration*: API definition compression is disabled by default. It can be enabled via the new `storage.enable_api_definition_compression` configuration option.
- *Security Limit*: To mitigate the risk of decompression bombs, the maximum combined uncompressed size for the API definitions is limited to 100MB.
</Accordion>

<Accordion title='OpenTelemetry and Observability Enhancements'>
This release brings significant improvements to OpenTelemetry tracing and log correlation capabilities within the Gateway. These enhancements ensure better observability and easier debugging across distributed systems by unifying trace context across all log types:

- Added the `trace_id` field to Gateway [access logs](/api-management/logs#access-logs) when OpenTelemetry is enabled, matching the `X-Tyk-Trace-Id` response header.
- Added `trace_id` and `span_id` fields to all request-scoped Gateway [application logs](/api-management/logs#application-logs) (middleware execution, errors, and debugging).
- Introduced custom trace header configuration (e.g., `X-Correlation-ID`) to support non-standard header names as trace context sources with three trace propagation modes:
  - **Custom-only** (read and write custom headers exclusively)
  - **Hybrid** (read custom headers, write standard traceparent)
  - **Composite** (read custom headers, write both custom and standard formats)
- Implemented automatic fallback to standard W3C propagators when custom trace headers are missing or invalid.
</Accordion>

<Accordion title='Prevent OpenTelemetry Span Loss in High-Throughput Environments'>
In high-throughput production environments, default OpenTelemetry settings can cause silent span loss and incomplete traces under heavy load. To address this, we've introduced the ability to tune OpenTelemetry `BatchSpanProcessor` settings to match your specific traffic patterns. By adjusting queue sizes and batch parameters, you can significantly reduce orphaned child spans and ensure trace completeness, giving you full visibility into your API traffic.

Added a new `span_batch_config` section to the OpenTelemetry configuration to prevent span loss in high-throughput environments. Users can now override default Go SDK settings by tuning `max_queue_size`, `max_export_batch_size`, and `batch_timeout`. This configuration is optional and backward compatible; omitted or zero values will default to standard SDK values.
</Accordion>

<Accordion title='Added structured error context to access logs'>
This release introduces detailed error context fields to access logs for 4XX and 5XX gateway and upstream errors, providing immediate technical insight into failure root causes including:

- `response_flag` for standardized error codes (e.g., `TLE` for TLS expired, `UCF` for connection refused, `RLT` for rate limiting).
- `response_code_details` for human-readable error descriptions.
- `error_source` (originating component) and `error_target` (upstream address).
- `upstream_status` to capture the HTTP status returned from the upstream service.
- context-specific fields that appear only when relevant: `tls_cert_expiry` and `tls_cert_subject` for certificate errors, and `circuit_breaker_state` for circuit breaker errors.

The full list of response flag codes is available in the [Access Logs documentation](/api-management/logs#access-logs).
</Accordion>

<Accordion title='Added configuration inspection endpoints to the Tyk Gateway API'>
Added new `/config` and `/env` endpoints to the Tyk Gateway API to provide programmatic access to runtime configuration and environment variable mappings. This enables targeted troubleshooting and configuration auditing directly through authenticated API calls:

- Supports both full configuration dumps and targeted single-field queries (e.g., `/config?field=storage.host`).
- Automatically redacts sensitive data (passwords, secrets, connection strings) to preserve configuration structure visibility securely.
- Clarifies configuration precedence and naming conventions from multiple sources.

This feature is disabled by default and is enabled using [`enable_config_inspection`](/tyk-oss-gateway/configuration#enable_config_inspection).
</Accordion>

<Accordion title='Added Client Certificate-Token Binding for Auth Token APIs'>
This release introduces the ability to bind client certificates to Auth Tokens for APIs secured with a static mTLS allow list. This provides enhanced token security by ensuring tokens are only used with their associated certificates:

- Added a new `mtls_static_certificate_bindings` field to the session object, which accepts a list of one or more certificate IDs.
- Enforces that the certificate presented in the request matches the bound certificate IDs; otherwise, the request is rejected.
- Supports binding multiple client certificates to a single key (token) to facilitate certificate rotation.

Please note that bound certificates must also be present in the client certificate allow list within the API definition for successful post-handshake validation. This feature maintains full backward compatibility with existing keys that do not specify certificate bindings.
</Accordion>

<Accordion title='Certificate Expiry Events for Upstream Connections'>
Extended [certificate expiry monitoring](/api-management/certificates#monitoring-certificate-expiry) to include TLS certificates used by the Gateway as the client in connections to upstream services. When a certificate used by the Gateway to authenticate itself with the upstream has expired or is within the configured number of days prior to expiry, an entry will be added to the application log and the appropriate Gateway event will be generated. The new `cert_role` field in the event metadata indicates whether the certificate was used in `client` or `upstream` authentication.

With this addition, the certificate expiry monitor now tracks all certificates used to represent the Gateway in TLS handshakes.
</Accordion>

<Accordion title='Restructured Certificate Authentication in Tyk OAS API definitions'>
This release introduces a new, dedicated configuration structure for Certificate Authentication (formerly Dynamic mTLS) in Tyk OAS API definitions, separating it from Auth Token authentication:

- Introduced the new `authentication.certificateAuth.enabled` field to configure Certificate Authentication as a standalone method.
- Deprecated the legacy `authentication.securitySchemes.authToken.enableClientCertificate` field (it remains fully supported for backward compatibility).
- When both the new and deprecated fields are present, the new `certificateAuth.enabled` field takes precedence.
</Accordion>

<Accordion title='Configurable Gateway-Default JWKS Cache Timeout'>
In Tyk 5.10.0, we introduced API-level configuration for the validity period of the JWKS cache for Tyk OAS APIs. Now we have made the Gateway default (which is applied if no API-level configuration is set) configurable via a new option in the Gateway config file: [`jwks.cache.timeout`](/tyk-oss-gateway/configuration#jwks-cache-timeout) or the equivalent environment variable. If this is not set, the timeout will continue to default to 240 seconds. This will be applied to both Tyk Classic and Tyk OAS APIs, simplifying JWKS cache management across large API deployments while providing flexibility for APIs that require specific caching behaviors. 
</Accordion>

<Accordion title='Improved Policy ID Handling in Multi-Organisation Environments'>
This release introduces improvements to how the Gateway handles policy IDs, particularly in multi-Organisation deployments. These changes ensure that policies are correctly applied and provide better visibility into potential configuration conflicts:

- The Gateway now correctly discriminates between policies with identical `id` fields across different Organisations (Orgs), ensuring that policies are only applied to keys within their respective `org_id`.
- Added a new warning-level log message that triggers if multiple policies are loaded with the same `id` within a single Org. The log details the shared `id` and the individual internal `_id` values of the conflicting policies to assist with troubleshooting.

These enhancements allow users to safely use custom policy IDs without risking cross-Org conflicts. The new warning logs help administrators identify and resolve legacy configuration issues in which duplicate policy IDs may exist within the same Organisation.
</Accordion>

<Accordion title='Visibility of the APIs and Policies loaded by Data Plane Gateway'>
Gateway now includes a list of the loaded APIs and policies in the information it provides to MDCB. This provides a clear picture of what is running on each Gateway in a distributed deployment, simplifying monitoring and troubleshooting of your deployed Data Planes.
</Accordion>

<Accordion title='Added Usage-Aware Certificate Synchronization for Data Planes'>
This release introduces a usage-aware certificate synchronization system for distributed deployments (MDCB). Data Planes can now be configured to only sync and store certificates that are actually required by their loaded APIs when using the [MDCB Synchroniser](/api-management/mdcb#mdcb-synchroniser), rather than pulling all certificates from the Control Plane:

- Added a new `sync_used_certs_only` boolean flag to the `slave_options` configuration.
- When enabled alongside `use_rpc: true`, the data plane tracks certificate usage by analyzing loaded API specifications and filters synchronization to only pull required certificates.
- Reduces memory usage and eliminates log noise caused by expired certificates that are not relevant to the specific Data Plane's APIs.

This feature is disabled by default (`sync_used_certs_only: false`) to ensure backward compatibility. When disabled, the Gateway will continue to synchronize all certificates from the Control Plane as before.
</Accordion>

<Accordion title='Optimized Bundle Verification to Reduce Memory Consumption'>
Fixed a performance issue introduced in v5.8.7 where bundle verification significantly increased CPU and memory consumption, particularly when using multiple APIs with plugin bundles.
We have introduced a new Gateway configuration option `skip_verify_existing_plugin_bundle` that allows you to skip cryptographic verification when loading signed plugin bundles from disk. When set to true, this option reduces performance overhead in environments with large numbers of APIs using signed bundles, while still maintaining security by validating signatures during initial bundle download.
Note: This option only affects signed bundles loaded from disk, unsigned bundles and initial downloads will continue to follow standard verification procedures.
</Accordion>


</AccordionGroup>

##### Fixed
<AccordionGroup>

<Accordion title='Fixed Path Matching Inconsistencies Between Classic and OAS API Middleware'>
Resolved path matching inconsistencies that could lead to Tyk OAS-specific middleware not being executed when expected.

These inconsistencies could cause the [Request Validation](/api-management/traffic-transformation/request-validation) and [Mock Response](/api-management/traffic-transformation/mock-response) middleware to be skipped in certain scenarios when using Tyk OAS APIs.
These scenarios included:
- Some subpaths, for example, the middleware configured for `/users` would not execute for `/users/123`
- some child API versions
- wildcard regexes in paths
- root paths

Now, Tyk will apply the same decisions to these middleware as it does to the rest of the request processing chain.
</Accordion>

<Accordion title='Fixed Certificate Re-use After Swapping in Multi-Auth Keys'>
Resolved an issue where swapping certificates in multi-auth (mTLS + Basic auth) keys prevented the original certificate from being reused. Previously, when updating a key's certificate, the original certificate remained incorrectly associated with the key internally, causing "key with given certificate already found" errors when attempting to reuse that certificate.

Tyk now properly detaches certificates during key updates, allowing certificates to be freely reused across different keys after being removed from their original association.
</Accordion>

<Accordion title='Improved JWKS Error Messaging for Faster JWT Troubleshooting'>
Enhanced Gateway error logging for JWT authentication failures related to JWKS endpoints. Previously, JWKS configuration issues generated vague error messages that didn't indicate the root cause, making troubleshooting difficult and time-consuming. 

The Gateway now provides specific, actionable error messages that clearly identify whether failures stem from Base64 decoding issues, network connectivity problems, or invalid JWKS content.
</Accordion>

<Accordion title='Fixed Gateway Panic if HashiCorp Vault Path Not Found'>
Resolved an issue where the Gateway could crash with a panic if the API definition contained an illegal reference to a secret in HashiCorp Vault. If the requested path did not exist in Vault, this could cause the Gateway process to exit, resulting in a complete service outage during API loads, hot reloads, or Dashboard saves. The Gateway now gracefully handles the missing Vault path and logs a clear error message.
</Accordion>

<Accordion title='Fixed OpenAPI multipleOf Validation for Floating-Point Numbers'>
Resolved a floating-point precision issue where mathematically valid multipleOf values were incorrectly rejected due to binary representation limitations. This could cause incorrect failures when performing Request Validation for Tyk OAS APIs.

The Gateway now properly handles floating-point precision in multipleOf validation, ensuring that all mathematically valid decimal multiples pass validation consistently while continuing to correctly reject invalid values.
</Accordion>

<Accordion title='Fixed Incomplete Validation of Multi-Value Request Headers'>
Resolved an issue where Tyk only validated the first instance of multi-value headers when processing requests to Tyk OAS APIs, allowing invalid header values to bypass schema constraints. 

The Gateway now properly normalizes and validates all header values according to HTTP standards, ensuring that all values in multi-value headers comply with the defined OpenAPI schema constraints.
</Accordion>

<Accordion title='Fixed API Routing Issues with Custom Domains and Similar Listen Paths'>
Resolved a routing issue where APIs could return `HTTP 404 Not Found` errors depending on custom domain settings, with differing behavior between Tyk OAS and Tyk Classic APIs. Previously, when APIs had similar listen path prefixes (e.g., `/caa` and `/caas2itsamu0456w2ayl9`), the Gateway's routing logic would incorrectly match requests, causing legitimate API calls to fail. The issue affected Tyk OAS APIs when custom domains were disabled, and Tyk Classic APIs when they were enabled. 

The Gateway now properly sorts and matches API specifications by listen path length, while correctly considering domain configuration options, ensuring all APIs are accessible via their configured paths regardless of custom domain settings or API type. 
</Accordion>

<Accordion title='Fixed Missing Request Duration Logging for Gateway Error Responses'>
Resolved an issue where the Gateway incorrectly logged 0ms duration for error responses, including `HTTP 504 Gateway Timeout`, `HTTP 499 Client Closed Request`, and `HTTP 500 Internal Server Error`, creating gaps in API observability and monitoring. Previously, these error responses were hardcoded with zero-latency values, making it impossible to determine the actual processing time, gateway saturation, or connection utilization for failed requests. 

The Gateway now accurately calculates and logs the actual request duration from start to error occurrence for all error responses, providing complete timing visibility across successful and failed API requests. This enhancement improves observability for performance monitoring, capacity planning, and troubleshooting workflows.
</Accordion>

<Accordion title='Fixed Missing Identity Source in OTEL Traces for JWT Protected APIs'>
Resolved an issue where OpenTelemetry traces were missing the "alias" field when using JWT-protected APIs, making it impossible to identify API consumers in tracing data. Previously, while the alias was correctly populated in Redis sessions and pump metrics, it was not included in OTEL spans for JWT-authenticated requests.

The Gateway now ensures that OTEL spans include the alias attribute for all authentication methods, enabling proper consumer identification and request attribution in distributed tracing systems.
</Accordion>

<Accordion title='Fixed Intermittent NewRelic Tracing'>
Resolved an issue where NewRelic OpenTracing integration worked inconsistently in Tyk Gateway. The Gateway now properly mounts NewRelic middleware on all routers, including reused ones, with thread-safe duplicate prevention and improved memory management during router swaps. This fix ensures consistent NewRelic APM visibility across all API calls and gateway versions, supporting both legacy NewRelic configurations and newer OpenTelemetry collector setups.
</Accordion>

<Accordion title='Fixed Custom Authentication Plugins in Compliant Mode'>
Resolved an issue where custom authentication plugins failed to execute properly when APIs were configured with Compliant Mode security processing. Previously, switching from Legacy Mode to Compliant Mode caused custom plugins to generate "JSVM isn't enabled" errors and return 500 Internal Server Error responses, even when JSVM was correctly configured. Custom authentication plugins now function identically in both Legacy and Compliant modes, allowing users to leverage flexible OR/AND authentication logic without breaking existing plugin functionality. Users can now seamlessly switch between authentication modes and use custom plugins with individual authentication methods in Compliant Mode's OR logic scenarios.
</Accordion>

<Accordion title='Fixed Incorrect X-RateLimit-Reset Timestamp on First Request After Quota Initialization'>
Resolved an issue where the `X-RateLimit-Reset` header showed an incorrect timestamp on the first API request after rate limit or quota counter initialization. Previously, when quota windows expired and were reset within the distributed lock, the Gateway failed to update its local timestamp variable, causing the first request to return stale timing information while subsequent requests showed correct values.

The Gateway now properly synchronizes its internal timer with the storage backend during quota window resets, ensuring that `X-RateLimit-Reset` headers accurately reflect the correct expiration time from the very first request.
</Accordion>

<Accordion title='Fixed Policy ID Collisions Across Organizations in Multi-Org Gateway'>
Resolved an issue where policies with identical custom IDs across different organizations could overwrite each other in the Gateway's memory storage, causing incorrect policy application. Previously, when multiple organizations used the same policy ID, the Gateway would retain only the last loaded policy, potentially applying incorrect rate limits, quotas, or access controls to API requests. The Gateway now properly isolates policies by organization, ensuring that policy lookups correctly match both the policy ID and organization ID. This fix prevents cross-organizational policy conflicts, ensures that keys and JWT tokens apply the correct policies from their respective organizations, and maintains proper tenant isolation in multi-organization deployments. Organizations can now safely use identical policy IDs without risk of policy collision or incorrect access control enforcement.
</Accordion>

<Accordion title='Fixed Missing Alias in OpenTelemetry Traces for JWT Multi-Auth APIs'>
Resolved an issue where OpenTelemetry traces were missing the `alias` attribute for JWT-authenticated requests in multi-auth APIs using compliant security processing mode. Previously, while the alias was correctly populated in analytics records and Redis session data (e.g., JWT claims or API key names), it was not included in OpenTelemetry spans for JWT authentication, making request attribution difficult in distributed tracing systems. The fix ensures that OTEL spans now include the alias attribute for all authentication methods in multi-auth configurations, providing consistent identity information across analytics records, pump output, and distributed traces. This enhancement improves observability for APIs using multiple authentication schemes, allowing operators to easily identify request sources in tracing backends like Jaeger, Tempo, or Zipkin when analyzing JWT-authenticated traffic alongside API key requests.
</Accordion>

<Accordion title='Fixed SSL Certificate Loading from MDCB During Gateway Startup'>
Resolved an issue where data plane gateways failed to load SSL certificates from MDCB during startup, preventing HTTPS listeners from functioning correctly. The fix implements exponential backoff retry logic that waits for the MDCB connection to become available during certificate loading, ensuring SSL certificates are properly retrieved, and HTTPS listeners start correctly. This resolves startup failures for new data plane deployments using HTTPS.
</Accordion>

</AccordionGroup>

##### Security Fixes
<AccordionGroup>

<Accordion title='Fixed Security Vulnerability in Dynamic mTLS Authentication'>
The Gateway now enforces certificate presence for dynamic mTLS authentication by default, rejecting requests that provide only tokens without valid client certificates. A new configuration option `allow_unsafe_dynamic_mtls_token` has been added for backward compatibility, but defaults to `false` to ensure secure behavior. When enabled, this option restores the previous (insecure) behavior of accepting token-only authentication for dynamic mTLS APIs.
</Accordion>

</AccordionGroup>

### 5.11.1
#### Changelog
<a id="Changelog-v5.11.1" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Memory Leak When Using JWKS URL Cache'>
Resolved a memory leak issue that could occur when APIs used JWT authentication with JWKS URL cache.
</Accordion>

<Accordion title='Optimized Bundle Verification to Reduce Memory Consumption'>
Fixed a performance issue introduced in v5.8.7 where bundle verification significantly increased CPU and memory consumption, particularly when using multiple APIs with plugin bundles.

We have introduced a new Gateway configuration option `skip_verify_existing_plugin_bundle` that allows you to skip cryptographic verification when loading signed plugin bundles from disk. When set to `true`, this option reduces the performance overhead for environments with large numbers of APIs using signed bundles, while still maintaining security by validating signatures during the initial bundle download.

**Note**: This option only affects signed bundles loaded from disk, unsigned bundles and initial downloads will continue to follow standard verification procedures.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed CVEs reported in dependent libraries, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15467" target="_blank">CVE-2025-15467</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-69419" target="_blank">CVE-2025-69419</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61726" target="_blank">CVE-2025-61726</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61728" target="_blank">CVE-2025-61728</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-68121" target="_blank">CVE-2025-68121</a>

</Accordion>

</AccordionGroup>

### 5.11.0
#### Changelog
<a id="Changelog-v5.11.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Support for Nested JWT Claim Paths in Subject, Policy, and Scope Mapping'>
Added support for nested JWT claims for subject and policy fields, enabling hierarchical claim structures to be used in authentication and policy application. Now you can configure the `subjectClaims`, `basePolicyClaims`, and `scopes.claims fields` to use nested claim names, such as `test.sub` or `policy.base`.
</Accordion>

<Accordion title='Enhanced Latency Metrics with Gateway-Only Processing Time'>
We have enhanced request-level timing by tracking precise timestamps when requests enter the Gateway, enabling accurate end-to-end latency calculations that extend beyond previous proxy-only measurements.
- Added `gateway` field to the `latency` struct in traffic logs to capture Gateway-specific processing time separate from upstream latency.
- Extended Prometheus and StatsD pumps to expose Gateway-only latency metrics alongside existing total and upstream measurements for improved observability.
</Accordion>

<Accordion title='Full Support for Custom GraphQL Scalar Values in Tyk GraphQL Engine'>
We have added support for custom scalar values when working with GraphQL APIs. Custom scalars can accept any valid GraphQL value literal (string, number, boolean, enum, object, list, null, variable), matching the GraphQL specification's requirements for custom scalars.
Existing standard scalar types (Int, Float, String, Boolean, ID) continue to work as before.
</Accordion>

<Accordion title='Background DNS Monitor for Faster MDCB Endpoint Failover'>
We have implemented background monitoring of MDCB endpoint DNS resolution to ensure rapid response to changes without waiting for failures, which block API consumer requests. When a DNS change is detected, Tyk will now automatically reconnect the RPC client to minimise downtime and risk of request blocking. The DNS monitor checks for changes at a configurable interval (default: 30 seconds, minimum: 10 seconds). This can be set using the `slave_options.dns_monitor` configuration.
</Accordion>

<Accordion title='Support Temporary Removal of Upstream Targets via Zero-Weight Load Balancing'>
You can now temporarily remove upstream targets (servers) from Tyk's upstream load balancing group. If a target is removed from the group, Tyk will route no traffic to it. This allows temporary target removal for maintenance, troubleshooting or environment issues.
Simply set the weight for the target to zero, and it will be removed from the round robin list. Multiple targets can be removed, but at least one must have a non-zero weight and thus will be served traffic.
</Accordion>

<Accordion title='Removed Default Policy Requirement for JWT Scope-to-Policy Mapping'>
We’ve removed the need to supply a default policy when using scope-to-policy mapping with JWT Authentication. Now, if you enable scope-to-policy mapping by configuring `scopes.claimName`, you do not need to provide a policy ID in `defaultPolicies`. If a request does not contain any valid scopes, it will be rejected with `HTTP 403 Forbidden` (default deny). You can still provide a default policy if you require a different behaviour.
</Accordion>

<Accordion title='Add OTel Trace ID to Traffic Logs for Improved Observability'>
When OpenTelemetry is enabled, the Trace ID allocated to each request is tagged in traffic logs as `trace-id-{traceID}` and also exposed in `$tyk_context.tyk_trace_id`. This makes it easier to correlate traffic logs with OTel traces in observability platforms and also allows transformation middleware and custom plugins to correlate logs with traces.
</Accordion>

<Accordion title='Added Configurable `X-Forwarded-For` Header Selection'>
Added `xff_depth` configuration parameter to the Gateway's HTTP server options for improved security. This specifies which entry in the `X-Forwarded-For` header chain should be considered to contain the real client IP. The value set in `xff_depth` is used when counting from the rightmost (most trusted) end of the IP chain, where a value of 1 is the first (counting from the right). If `xff_depth` is set to 0 or not configured, Tyk continues using the first IP address as before, maintaining backward compatibility. We have updated the default configurations across Tyk Demo, Helm Charts, and example files to use `xff_depth=1` for enhanced security in new deployments.
</Accordion>
<Accordion title='OpenTelemetry Trace Context Propagation for gRPC Plugins'>
Implemented OpenTelemetry trace context propagation to maintain request tracing visibility as requests flow through plugins, with specific support for gRPC plugins. Enhanced the Protocol Buffer definitions and Dispatcher interface to include trace context fields, updated the `CoProcessor` and `GRPCDispatcher` to preserve trace information, and added OpenTelemetry gRPC interceptors for seamless context propagation. This eliminates observability blind spots in plugin processing, allowing customers to see complete end-to-end traces of API requests, including all plugin activities.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed JWT Authentication Panic in MDCB Emergency Mode'>
Fixed a bug causing JWT authentication to panic in MDCB emergency mode. When processing tokens with new sub-claims, the gateway returned an uninitialized session missing its KeyID, leading to a crash when generating the session’s KeyHash. The fix ensures the KeyID is preserved in the emergency-mode path, allowing sessions to be created and cached correctly and preventing panics during MDCB outages.
</Accordion>

<Accordion title='Resolved Panic Triggered by DRL Updates in Mixed Rate-Limiter Environments'>
Fixed an issue where Gateways using Redis-based rate limiters would crash when sharing Redis with Gateways using Distributed Rate Limiting (DRL). Non-DRL Gateways now properly ignore DRL update messages instead of attempting to process them, enabling mixed rate-limiter deployments across shared Redis instances.
</Accordion>

<Accordion title='Fixed Data Plane Startup Failures Causing Incorrect 404 Responses'>
Fixed an issue where a Data Plane Gateway could fail to load API definitions the MDCB link failed during initialisation. This would lead to client requests returning HTTP 404 errors. The expected behaviour, if MDCB is unavailable, is for the Data Plane Gateway to retrieve policies and API definitions from the local storage (Redis), but this was not occurring in certain scenarios. We have improved the robustness of the Gateway startup so that, if MDCB goes down, it will automatically switch to the local storage (Redis) as expected.
</Accordion>

<Accordion title='Corrected mTLS Certificate Advertising for RFC-Compliant Clients'>
Fixed an issue where Tyk Gateway advertised leaf certificate Subject DNs instead of Certificate Authority DNs during mTLS handshakes, causing connection failures with RFC-compliant TLS clients. The Gateway now properly extracts and advertises CA DNs from certificate chains in the CertificateRequest message, ensuring compatibility with standards-compliant clients like `Apache mod_ssl` while maintaining backward compatibility with existing configurations.
</Accordion>

<Accordion title='Fixed JSON Formatter Failures with Large Numeric Error Values'>
We fixed a logging bug in the JSON formatter that could cause error logs to fail to serialize when an error message contained very large numeric values (for example a big integer), which sometimes resulted in missing or broken log output; the formatter now writes the error as a plain text string instead of attempting to encode the underlying error object, so logs reliably serialize to JSON.
</Accordion>

<Accordion title='Reduced RPC Retry Delays by Improving DNS Change Detection'>
Fixed an issue where the Gateway would incorrectly retry RPC calls repeatedly when MDCB is unavailable, but the DNS hasn't changed. This would cause API requests to block for over 90 seconds before returning an error. Now it takes into account the fact that DNS has not changed and so fails fast, entering Emergency Mode after one retry (30 seconds).
</Accordion>

<Accordion title='Removed Redundant Boolean Enums from OpenAPI Specification'>
Fixed redundant boolean enum definitions in OpenAPI specification by removing unnecessary enum: [true, false] declarations from boolean type fields in swagger.yml files. Boolean parameters now use only type: boolean, following OpenAPI best practices.
</Accordion>

<Accordion title='Fixed OAuth Client Key Retrieval Delays in JWT APIs on Hybrid Gateways'> 
Resolved an issue where JWT APIs using Keycloak authentication experienced significant delays on hybrid gateways due to failed local key lookups. The gateway was unable to find OAuth client keys in local Redis and had to fetch them from the control plane on every request, causing performance degradation and "key not found" errors in logs. JWT API requests now retrieve keys efficiently from local storage, eliminating unnecessary round-trip requests and providing consistent response times.
</Accordion>

<Accordion title='Request pipeline blocked by synchronous RPC calls every 10 minutes when MDCB is unavailable'>
Fixed blocking synchronous RPC calls in the request pipeline that occurred every 10 minutes during organization expiry checks when MDCB was unavailable. The organization expiry validation is now asynchronous and non-blocking, preventing API request timeouts and latency spikes (up to 90 seconds) when MDCB connectivity issues occur. This ensures consistent API response times regardless of MDCB availability status.
</Accordion>

<Accordion title='Fixed Gateway Crash During Concurrent JWT Claims Validation'>
Resolved an issue where Tyk Gateway would crash when multiple users simultaneously accessed APIs with JWT claims validation enabled. The Gateway now processes JWT validation configurations once during API startup instead of during each request, eliminating the race conditions that caused service interruptions under concurrent load.
</Accordion>

<Accordion title='Fixed: API Keys Remain Active When Set to Inactive Status'>
Resolved an issue where API keys continued to process traffic even after being marked as inactive through API updates.
</Accordion>

</AccordionGroup>

### 5.10.1
#### Changelog
<a id="Changelog-v5.10.1" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Custom Authentication fallback when custom plugin bundle is disabled'>
Fixed an issue where [Custom Authentication](/api-management/authentication/custom-auth) could fall back to a previously configured alternative authentication method if the custom plugin bundle was not loaded. Now this is treated as for any other failed plugin load, and requests to the API will be rejected with `HTTP 500 Internal Server Error` to prevent access to an improperly configured endpoint.
</Accordion>

<Accordion title='Fixed issue with invalid or missing bundle manifests'>
Fixed an issue where the Gateway would load and attempt to use plugin bundles even when the manifest file was invalid or missing. The Gateway now properly validates bundle manifests and fails safely by rejecting API requests when bundles cannot be properly loaded or verified. 
This prevents risks from corrupted or tampered bundles and ensures that APIs with invalid plugin configurations are not accessible, maintaining the integrity of authentication and authorization checks implemented by plugins.
</Accordion>

<Accordion title='Fixed JWT key activation when toggling default policy from draft to active'>
Fixed an issue where keys could remain deactivated when a policy applied to them was changed from `draft` to `active` status. When an access key/token is presented to Tyk in a request, policies linked to the key will be applied, configuring the authorization for that request. If any policy is in `draft` state, the key will be rejected. 
Toggling the policy to the `active` state should activate any keys to which the policy is applied. Previously, if the policy had never been applied when it was in `draft` state, there was an issue where keys would incorrectly be marked as `inactive`. This has now been resolved, and the policy state is correctly mapped to keys.
</Accordion>

<Accordion title='Added new configuration option for limiting response body size'>
Added a new configuration option, [HttpServerOptions.MaxResponseBodySize](/tyk-oss-gateway/configuration#http_server_options-max_response_body_size) to limit the maximum size of the response bodies processed during any response body transformations.  When the limit is exceeded, the Gateway returns `HTTP 500 Response Body Too Large` instead of attempting to process the oversized content.
</Accordion>

<Accordion title='Fixed plugin loading failure errors being ignored for gRPC, Python, and Lua plugins'>
Fixed an issue where plugin loading failure errors were ignored for gRPC, Python, and Lua plugins, allowing API requests to be processed even when plugins failed to load. The Gateway now properly validates plugin drivers during request processing and fails safely by returning `HTTP 500 Internal Server Error` when any plugin fails to load, ensuring consistent behavior across all plugin types.
</Accordion>

<Accordion title='Fixed random version selection when `not_versioned` is set to true'>
Fixed an issue where a **Tyk Classic API** with inconsistent versioning configuration would process requests using a **random version’s configuration**.

A non-versioned API should:

- Contain a single entry in `version_data.versions` with the API configuration.
- Have the `version_data.not_versioned` flag set to `true`.

Previously, if multiple entries existed in the `version_data.versions` array while `not_versioned` was set to `true`, the Gateway would **randomly select one** of those versions to process incoming requests.

**New behavior:**

When `version_data.not_versioned` is set to `true` and multiple versions are present, Tyk now deterministically selects the configuration for the **default version** instead of picking one at random.

Tyk determines the default version as follows:

- First, it looks for an entry named `"Default"`.
- If not found, it checks for `"default"`.
- If neither exists, it checks for an entry with an **empty string key** (`""`).
- If none of these are found, Tyk returns an **error**, indicating a misconfigured non-versioned API.
</Accordion>

<Accordion title='Improved path handling during bundle decompression.'>
Tyk Gateway now validates all file paths in zip bundles before extraction, rejecting bundles that contain invalid paths. Bundle extraction fails immediately upon detecting invalid paths, with detailed error logging, ensuring that only proper bundles with valid relative paths are processed.
</Accordion>

<Accordion title='Fixed Data Plane Gateway hanging when MDCB connection is lost'>
Fixed an issue where a Data Plane Gateway could hang for all client requests when the MDCB connection was lost. This was caused by the Gateway incorrectly checking the Organisation quota when `TYK_GW_ENFORCEORGQUOTAS` was not set. If the Organisation quota cache expired before the Gateway performed a health check, the Gateway could hang.

From this release, the Gateway does not check the Organisation quota cache if this is not set. For users relying on Organisation quotas (setting `TYK_GW_ENFORCEORGQUOTAS=true`), the scenario is different and the lock does not occur.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Fixed the following high-priority CVEs, providing increased protection against security
vulnerabilities:

- <a href="https://www.cve.org/CVERecord?id=CVE-2025-47912" target="_blank">CVE-2025-47912</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58183" target="_blank">CVE-2025-58183</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58185" target="_blank">CVE-2025-58185</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58186" target="_blank">CVE-2025-58186</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58187" target="_blank">CVE-2025-58187</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58188" target="_blank">CVE-2025-58188</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-58189" target="_blank">CVE-2025-58189</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-61723" target="_blank">CVE-2025-61723</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-61724" target="_blank">CVE-2025-61724</a>
- <a href="https://www.cve.org/CVERecord?id=CVE-2025-61725" target="_blank">CVE-2025-61725</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-63811" target="_blank">CVE-2025-63811</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-31133" target="_blank">CVE-2025-31133</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-52565" target="_blank">CVE-2025-52565</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-52881" target="_blank">CVE-2025-52881</a>

</Accordion>

</AccordionGroup>

### 5.10.0
#### Changelog
<a id="Changelog-v5.10.0" data-scroll-offset></a>

##### Added




<AccordionGroup>
<Accordion title='OpenAPI compliant multi-authentication mode for Tyk OAS APIs'>
Added OpenAPI Specification compliant multi-authentication support for Tyk OAS APIs, providing flexible authentication workflows that follow standard OpenAPI security patterns.

**Compliant mode (new)**
- Processes all entries in the OpenAPI `security` section sequentially, not just the first entry
- Supports a local `security` section in the Tyk vendor extension for proprietary authentication methods (Custom Authentication plugin, HMAC)
- Uses AND logic within each security entry and OR logic between entries, enabling flexible authentication combinations such as: OAuth2 OR Auth Token
- Allows clients to authenticate using any of the defined security combinations

**Legacy mode (existing behavior)**
- Continues to use only the first entry from the OpenAPI `security` section
- Combines all declared methods with proprietary vendor extension methods using AND logic
- Requires clients to satisfy ALL authentication methods

The authentication processing mode is controlled by the new `server.authentication.securityProcessingMode`
field in the Tyk Vendor Extension, with `legacy` as the default to ensure backward compatibility. In compliant mode, proprietary authentication methods are configured in the new `server.authentication.security` section within the vendor extension, following the same array structure as the OpenAPI `security` section. This prevents breaking changes for existing API definitions that contain multiple entries in the
`security` section but were designed for legacy processing behavior.
</Accordion>

<Accordion title='Enhanced JWT claim validation for Tyk OAS APIs'>
Tyk OAS APIs now support comprehensive validation of JWT registered claims, extending beyond basic token validation to provide complete access control capabilities. This enhancement includes:

**Registered claim validation**

- **Subject, issuer, and audience validation**: Validate tokens against allowed values with support for multiple entries per claim type
- **JWT ID enforcement**: Require presence of unique token identifiers (`jti`) when needed
- **Flexible claim mapping**: Configure different claim names for subject, base policy, and scope-to-policy mapping to support multiple Identity Providers within the same API setup (e.g., Keycloak's `scope` vs Okta's `scp`)

**Custom claim validation framework**

- **Flexible validation rules**: Define validation for any custom JWT claim using three rule types: `required` (claim must exist), `exact_match` (claim equals specific values), or `contains` (claim contains specific values)
- **Advanced data support**: Handle string, number, boolean, and array data types with nested claim access using dot notation (e.g., `user.department`)
- **Non-blocking validation**: Configure rules to log warnings instead of rejecting requests for monitoring and gradual enforcement scenarios

These features enable advanced use cases, such as role-based access control, department validation, and custom permission schemes, while maintaining backward compatibility with existing JWT configurations.

**Note:** Available only for Tyk OAS APIs and configured directly in the API definition via the Tyk Vendor Extension.
</Accordion>

<Accordion title='Enhanced JWKS caching with configurable timeout, invalidation, and pre-fetching'>
Enhanced the JWKS (JSON Web Key Set) caching system with three key improvements to reduce latency and provide better control over JWT validation:

Configurable cache timeout - Tyk OAS APIs can now specify custom cache timeout values for JWKS endpoints in their JWT validation configuration, allowing fine-tuned control over cache refresh intervals based on Identity Provider requirements.

- Cache invalidation API - Administrators can now manually invalidate JWKS cache entries via new Gateway API endpoints (`DELETE /tyk/cache/jwks/{apiID}` and `DELETE /tyk/cache/jwks`), either targeting specific APIs or purging all cached JWKS data. This enables immediate cache refresh when Identity Provider keys are rotated.
- Automatic pre-fetching - For Tyk OAS APIs, JWKS data is now automatically fetched and cached when API definitions are loaded, eliminating cold-start delays for JWT validation. Pre-fetching includes comprehensive logging of fetch attempts and results, and failures do not prevent API initialization.

**Note:** For Tyk Classic APIs, JWKS caching behavior remains unchanged with on-demand fetching during token validation using the default cache timeout (60 seconds). Cache invalidation via the new API endpoints works for both Classic and OAS APIs.

These enhancements improve JWT validation performance for Tyk OAS APIs and provide administrators with better tools for managing JWKS cache lifecycle when Identity Provider keys change.
</Accordion>

<Accordion title='Enhanced external service integration with proxy and mTLS support'>
Added a new `external_services` section in the [Gateway configuration](/configure/external-service) to provide centralized configuration for proxy settings and mTLS certificates when communicating with external services. This includes connections to persistent and temporal storage, OAuth 2.0 Authorization Servers, and webhook targets.

Tyk Gateway can now apply proxy settings from standard environment variables (`HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY`) or use the new granular configuration options. All existing configuration methods remain supported, including legacy options such as `jwt_ssl_insecure_skip_verify` and `http_proxy`.
</Accordion>

<Accordion title='Gateway Certificate Expiry Notification Events'>
Introduced a proactive event system to warn administrators when mTLS certificates are approaching expiry. The Gateway now emits two new [API events](/api-management/gateway-events#api-events) to provide visibility into certificate status:

- `CertificateExpiringSoon` - Generated when a certificate is used in an API request (either client-to-Gateway or Gateway-to-upstream) within a configurable time period of its expiry date
- `CertificateExpired` - Generated when an attempt is made to use an already expired certificate, in addition to the standard error response sent to the API client

A cooldown mechanism prevents event flooding by throttling the generation of these notifications. The threshold for the `CertificateExpiringSoon` event and cooldown parameters are configured in the Gateway configuration:

```
"security": {
  "certificate_expiry_monitor": {}
}
```

The default threshold is 30 days before expiry.
</Accordion>
</AccordionGroup>




##### Changed



<Expandable title='Go 1.24 Upgrade for Tyk Gateway'>
The Tyk Gateway has been updated to [Golang 1.24](https://tip.golang.org/doc/go1.24), improving security by staying up-to-date with Go versions.
</Expandable>



<Expandable title='Support for pre-configurable versioning setup for Tyk OAS APIs'>
Implemented changes to the validation of Tyk OAS API definitions to support the enhanced versioning workflow implemented in Tyk Dashboard v5.10.0. This allows the pre-configuration of versioning settings before creating any child versions. You can now define the version identifier location (header, URL path, or query parameter) and key/name/pattern, and the request proxying behavior on a non-versioned API, preparing it to become a base API.
</Expandable>



##### Fixed



<AccordionGroup>
<Accordion title='Fixed panic when an unexpected query parameter is provided to the Gateway API'>
Fixed an issue where sending certain unexpected query parameters to the `GET /tyk/apis/oas/{id}` endpoint could cause a panic.
</Accordion>

<Accordion title='Fixed duplication of version identifier configuration when importing OpenAPI description'>
Fixed an issue where importing an OpenAPI description with an `apiKey` security scheme, while using the `authentication` query parameter, resulted in the unnecessary generation of a `header` object within the Tyk Vendor Extension (`x-tyk-api-gateway`), duplicating information already present in the declared OpenAPI security scheme.
</Accordion>

<Accordion title='Fixed mock responses not working with internal API proxying'>
Fixed an issue where Tyk OAS mock response middleware failed to execute when internal API proxying was enabled. Mock responses configured in the target API are now correctly returned when a request is redirected to another API on the same Tyk Gateway instance via [internal looping](/advanced-configuration/transform-traffic/looping).
</Accordion>

<Accordion title='Base API CORS settings incorrectly applied to child API versions'>
Fixed an issue where CORS settings from the base API were incorrectly applied to all versions of a Tyk OAS API, preventing child API versions from using their own CORS configuration. This occurred because the CORS check was performed before the request was routed to the correct API version.

The processing order has been corrected so that requests are first routed to the appropriate version (base or child), then the correct CORS settings are applied, allowing each API version to have its own CORS configuration.
</Accordion>

<Accordion title='Fixed Request Body Transform middleware not being applied with regex in URL rewrite'>
Fixed an issue where Response Body Transformation middleware failed to apply to endpoints that used URL rewrite with regex patterns. When the endpoint path contained regex metacharacters (e.g., $, ^, (), []), these characters interfered with the body transformation's internal pattern-matching process, preventing the middleware from executing.
</Accordion>

<Accordion title='Fixed duration format validation errors in Tyk OAS API definitions'>
Resolved an issue where the Gateway automatically converted Readable Duration values (such as uptime test timeouts) in Tyk OAS API definitions from integer-based formats to decimal formats, which triggered schema validation warnings. The effect of this was seen in the Tyk OAS API editor in the Dashboard UI where, for example, a duration of '4s500ms' would be converted to '4.5s' when reopening an API definition. 

Duration values are now consistently serialized and maintained in their original, integer-based format, preventing these validation errors.
</Accordion>

<Accordion title='Fixed TLS configuration not being applied for Redis rate limiting'>
Fixed an issue where Tyk Gateway did not properly apply the configured TLS settings when connecting to Redis for rate limiting operations. This could result in connection failures and incorrect `HTTP 429 Too Many Requests` responses being returned to clients. The rate limiter now correctly establishes TLS connections to Redis.
</Accordion>

<Accordion title='Fixed Gateway crash when deleting APIs with Uptime Test enabled'>
Fixed a bug where deleting an API with the Uptime Test feature enabled could cause the Gateway to crash due to a nil pointer dereference during cleanup operations. The Gateway now properly handles memory cleanup when removing APIs with active uptime tests, preventing crashes and ensuring stable API lifecycle management.
</Accordion>

<Accordion title='Fixed Gateway re-registration failures after restart'>
Fixed an issue where Gateways could fail to re-register with the Dashboard after a restart, particularly during upgrades or in large-scale deployments. This resulted in `Authorization failed (Nonce empty)` errors and Gateway crash loops that prevented successful registration. 

The fix includes an updated license handler with hardened registration logic, enhanced Dashboard authentication retry mechanisms, and support for new "Unlimited Gateway" licenses, ensuring Gateways register reliably without entering failure loops even during heavy churn or rolling upgrades.
</Accordion>

<Accordion title='Fixed body decompression errors with GraphQL APIs when analytics is enabled'>
Fixed an issue that caused repeated `Body decompression error: EOF` log messages when analytics were enabled for GraphQL APIs. The problem occurred because the Gateway attempted to decompress the response body after it had already been consumed for analytics processing, resulting in End of File (EOF) errors. 

The Gateway now properly handles response body consumption for GraphQL APIs with analytics, eliminating the spurious error logs.
</Accordion>

<Accordion title='Stricter validation for version name parameter when creating a new child API version'>
Fixed an issue where users could create child Tyk OAS API versions using the `/tyk/apis/oas` endpoint without specifying a valid version name (`new_version_name`). The Gateway API now rejects such requests with an `HTTP 422 Unprocessable Entity` error, ensuring all versions have meaningful identifiers and preventing the creation of unusable or empty version entries.
</Accordion>

<Accordion title='Fixed inconsistent middleware updates for Tyk OAS API `PATCH` requests'>
Fixed an issue where updating a Tyk OAS API via `PATCH /tyk/apis/oas/{apiId}` did not properly update the Tyk Vendor Extension (`x-tyk-api-gateway`). When endpoints were removed or modified in the OpenAPI description, their corresponding middleware definitions could persist incorrectly in the vendor extension, leaving the API definition in an inconsistent state. 

The vendor extension is now correctly rebuilt to reflect all changes made to the OpenAPI description.
</Accordion>
</AccordionGroup>

### 5.8.13
##### Changed

<AccordionGroup>

<Accordion title='Updated Go version to 1.25'>
The Tyk Gateway has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

<Accordion title='Update Docker images to Debian 13 (Trixie)'>
Updated the Docker images for Tyk Gateway to Debian 13 (Trixie) to address multiple vulnerabilities in the underlying operating system.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed path matching inconsistency for Tyk OAS APIs'>
Resolved an issue where parameterized paths could incorrectly take precedence over static paths when using the Request Validation or Mock Response middleware in Tyk OAS APIs. Static paths will now correctly bypass these middleware if not explicitly configured, restoring the expected routing behavior.
</Accordion>

<Accordion title='Gateway /hello endpoint behaviour restored when Redis is unavailable'>
Reverted the change introduced in versions 5.9.0 and 5.8.3 to the /hello health check endpoint, restoring its original functionality. This fix resolves an issue where the endpoint returned a 503 error when Redis was down. The /hello endpoint now correctly returns HTTP 200 during normal operations, ensuring compatibility with Kubernetes liveness and readiness probes.

_This issue was originally fixed in Tyk 5.8.4 but then was omitted from Tyk 5.8.6 onwards_
</Accordion>

<Accordion title='Gateways in Distributed Data Planes Were Unable To Perform mTLS When MDCB Link Unavailable'>
Resolved an issue introduced in Tyk 5.7.1 where Gateways in distributed Data Planes failed to cache TLS certificates correctly in the local Redis, resulting in potential service disruptions if MDCB became unavailable. Data plane gateways now reliably serve HTTPS and mTLS traffic even if MDCB is unavailable.

_This issue was originally fixed in Tyk 5.8.2 but then was omitted from Tyk 5.8.6 onwards_
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15281" target="_blank">CVE-2025-15281</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-0861" target="_blank">CVE-2026-0861</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-0915" target="_blank">CVE-2026-0915</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25679" target="_blank">CVE-2026-25679</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32285" target="_blank">CVE-2026-32285</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32286" target="_blank">CVE-2026-32286</a>
- <a href="https://www.cvedetails.com/cve/CVE-2026-33186/" target="_blank">CVE-2026-33186</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34986" target="_blank">CVE-2026-34986</a>

</Accordion>

</AccordionGroup>

### 5.8.12
#### Changelog
<a id="Changelog-v5.8.12" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Path Matching Inconsistencies Between Classic and OAS API Middleware'>
Resolved path matching inconsistencies that could lead to Tyk OAS-specific middleware not being executed when expected.

These inconsistencies could see the [Request Validation](/api-management/traffic-transformation/request-validation) and [Mock Response](/api-management/traffic-transformation/mock-response) middleware being skipped in certain scenarios when using Tyk OAS APIs.

These scenarios included:
- some subpaths, for example the middleware configured for `/users` would not execute for `/users/123`
- some child API versions
- wildcard regexes in paths
- root paths

Now Tyk will apply the same decisions for these middleware as for the rest of the request processing chain.

</Accordion>

<Accordion title='Improved JWKS Error Messaging for Faster JWT Troubleshooting'>
Enhanced Gateway error logging for JWT authentication failures related to JWKS endpoints. Previously, JWKS configuration issues generated vague error messages that didn't indicate the root cause, making troubleshooting difficult and time-consuming. 

The Gateway now provides specific, actionable error messages that clearly identify whether failures stem from Base64 decoding issues, network connectivity problems, or invalid JWKS content.
</Accordion>

<Accordion title='Fixed Gateway Panic if HashiCorp Vault Path Not Found'>
Resolved an issue where the Gateway could crash with a panic if the API definition contained an illegal reference to a secret in HashiCorp Vault. If the requested path did not exist in Vault, the Gateway process could exit, resulting in a complete service outage during API loads, hot reloads, or Dashboard saves. The Gateway now gracefully handles the missing Vault path and logs a clear error message.
</Accordion>

<Accordion title='Fixed Incomplete Validation of Multi-Value Request Headers'>
Resolved an issue where Tyk only validated the first instance of multi-value headers when processing requests to Tyk OAS APIs, allowing invalid header values to bypass schema constraints. 

The Gateway now properly normalizes and validates all header values according to HTTP standards, ensuring that all values in multi-value headers comply with the defined OpenAPI schema constraints.
</Accordion>

<Accordion title='Fixed API Routing Issues with Custom Domains and Similar Listen Paths'>
Resolved a routing issue where APIs could return `HTTP 404 Not Found` errors depending on custom domain settings, with differing behavior between Tyk OAS and Tyk Classic APIs. Previously, when APIs had similar listen path prefixes (e.g., `/caa` and `/caas2itsamu0456w2ayl9`), the Gateway's routing logic would incorrectly match requests, causing legitimate API calls to fail. The issue affected Tyk OAS APIs when custom domains were disabled, and Tyk Classic APIs when they were enabled. 

The Gateway now properly sorts and matches API specifications by listen path length, while correctly considering domain configuration options, ensuring all APIs are accessible via their configured paths regardless of custom domain settings or API type. 
</Accordion>

<Accordion title='Fixed Missing Request Duration Logging for Gateway Error Responses'>
Resolved an issue where the Gateway incorrectly logged 0ms duration for error responses, including `HTTP 504 Gateway Timeout`, `HTTP 499 Client Closed Request`, and `HTTP 500 Internal Server Error`, creating gaps in API observability and monitoring. Previously, these error responses were hardcoded with zero-latency values, making it impossible to determine the actual processing time, gateway saturation, or connection utilization for failed requests. 

The Gateway now accurately calculates and logs the actual request duration from start to error occurrence for all error responses, providing complete timing visibility across successful and failed API requests. This enhancement improves observability for performance monitoring, capacity planning, and troubleshooting workflows.
</Accordion>

<Accordion title='Fixed Missing Identity Source in OTEL Traces for JWT Protected APIs'>
Resolved an issue where OpenTelemetry traces were missing the "alias" field when using JWT-protected APIs, making it impossible to identify API consumers in tracing data. Previously, while the alias was correctly populated in Redis sessions and pump metrics, it was not included in OTEL spans for JWT-authenticated requests.

The Gateway now ensures that OTEL spans include the alias attribute for all authentication methods, enabling proper consumer identification and request attribution in distributed tracing systems.
</Accordion>

<Accordion title='Fixed Intermittent NewRelic Tracing'>
Resolved an issue where NewRelic OpenTracing integration worked inconsistently in Tyk Gateway. The Gateway now properly mounts NewRelic middleware on all routers, including reused ones, with thread-safe duplicate prevention and improved memory management during router swaps. This fix ensures consistent NewRelic APM visibility across all API calls and gateway versions, supporting both legacy NewRelic configurations and newer OpenTelemetry collector setups.
</Accordion>

<Accordion title='Fixed Incorrect X-RateLimit-Reset Timestamp'>
Resolved an issue where the `X-RateLimit-Reset` header showed an incorrect timestamp in the response to the first API request after rate limit or quota counter initialization. Previously, when quota windows expired and were reset within the distributed lock, the Gateway would return stale timing information in the first response.

The Gateway now properly synchronizes its internal timer with the storage backend during quota window resets, ensuring that `X-RateLimit-Reset` headers accurately reflect the correct expiration time from the very first response.
</Accordion>

<Accordion title='Fixed OpenAPI multipleOf Validation for Floating-Point Numbers'>
Resolved a floating-point precision issue where mathematically valid multipleOf values were incorrectly rejected due to binary representation limitations. This could cause incorrect failures when performing Request Validation for Tyk OAS APIs.

The Gateway now properly handles floating-point precision in multipleOf validation, ensuring that all mathematically valid decimal multiples pass validation consistently while continuing to correctly reject invalid values.
</Accordion>

<Accordion title='Fixed SSL Certificate Loading from MDCB During Gateway Startup'>
Resolved an issue where data plane gateways failed to load SSL certificates from MDCB during startup, preventing HTTPS listeners from functioning correctly. The fix implements exponential backoff retry logic that waits for the MDCB connection to become available during certificate loading, ensuring SSL certificates are properly retrieved, and HTTPS listeners start correctly. This resolves startup failures for new data plane deployments using HTTPS.
</Accordion>

</AccordionGroup>

##### Security Fixes
<AccordionGroup>

<Accordion title='Fixed Security Vulnerability in Dynamic mTLS Authentication'>
The Gateway now enforces the mutual TLS handshake when an API is secured using Auth Token with Dynamic mTLS. The client must therefore present a valid client certificate in the request. Previously Dynamic mTLS would permit authentication using only the Auth Token and the mTLS handshake was not enforced.

A new configuration option `allow_unsafe_dynamic_mtls_token` has been added for any users relying on the legacy behavior. This defaults to `false`.

A new Gateway configuration option `allow_unsafe_dynamic_mtls_token` has been added for backward compatibility, but defaults to `false` to ensure secure behavior. When enabled, this option restores the previous (insecure) behavior of accepting token-only authentication for APIs secured with Auth Token + Dynamic mTLS.
</Accordion>

</AccordionGroup>

### 5.8.11
#### Changelog
<a id="Changelog-v5.8.11" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Optimized Bundle Verification to Reduce Memory Consumption'>
Fixed a performance issue introduced in v5.8.7 where bundle verification significantly increased CPU and memory consumption, particularly when using multiple APIs with plugin bundles.

We have introduced a new Gateway configuration option `skip_verify_existing_plugin_bundle` that allows you to skip cryptographic verification when loading signed plugin bundles from disk. When set to `true`, this option reduces the performance overhead for environments with large numbers of APIs using signed bundles, while still maintaining security by validating signatures during the initial bundle download.

**Note**: This option only affects signed bundles loaded from disk. Unsigned bundles and initial downloads will continue to follow standard verification procedures.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed CVEs reported in dependent libraries, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15467" target="_blank">CVE-2025-15467</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-69419" target="_blank">CVE-2025-69419</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61726" target="_blank">CVE-2025-61726</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61728" target="_blank">CVE-2025-61728</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-68121" target="_blank">CVE-2025-68121</a>


</Accordion>

</AccordionGroup>


