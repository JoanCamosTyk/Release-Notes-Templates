## Intructions
I am going to copy paste all the Highlights and Change Logs & Breaking Changes from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Breaking Changes

### 5.13.0
MCP Proxy policies incorrectly synchronize to Developer Portal versions prior to 1.17.2
Dashboard 5.13.0 introduces policies that grant access to MCP Proxies. Tyk Developer Portal versions prior to 1.17.2 may automatically retrieve these policies and present them as API Products when synchronizing with the Dashboard.
There is no facility at this time for Developer Portal to manage API Products that grant access to MCP Proxies. An administrator could inadvertently publish an MCP server to API Consumers leading to a confusing experience.
Upgrade to Developer Portal 1.17.2 or later before or immediately after upgrading the Dashboard. Developer Portal 1.17.2 recognises MCP Proxy policies and does not make them available as API Products.
Strict Validation of Characters Allowed in Policy IDs
To avoid an issue where Policy IDs containing special characters could cause problems when parsing API endpoint requests, we have introduced strict validation of Policy IDs during Policy creation and update.
The allowed characters are:
alphanumeric characters
_
-
.
~
Strict validation can be disabled, if required for existing Policies with incompatible Policy IDs, using the new Dashboard configuration allow_unsafe_policy_ids. If using this mode, care must be taken not to use characters that could affect URL parsing.

## Release Highlights

### 5.8.14
This patch release contains various bug fixes and addresses some vulnerabilities in 3rd party libraries, providing improved performance and security enhancements.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.13.0
MCP Gateway Management Tyk Dashboard 5.13.0 adds comprehensive support for the MCP (Model Context Protocol) Gateway. A dedicated /api/mcps endpoint set provides full CRUD operations for MCP Proxy definitions.
A new mcp RBAC permission group (deny/read/write) controls access to MCP management independently of the existing apis permission. Visibility is controlled solely by the mcp RBAC permission.
Sessions (Keys) and Policies can now carry four new MCP-specific access-right fields for configuring tool-based access control and per-primitive rate limiting.
This release also delivers a range of Dashboard UI enhancements, bug fixes, and CVE fixes to provide additional protection against security vulnerabilities.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.12.1
Tyk Dashboard has been updated to Golang 1.25 and Debian 13 (Trixie) for enhanced security and performance. This release also addresses multiple CVEs in dependent libraries.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.12.1).

### 5.12.0
**OpenAPI Specification 3.1 is now supported**

In this release, we are delighted to bring initial support for OAS 3.1, covering:

- Import and validation of OpenAPI 3.1 descriptions using Tyk Dashboard to create Tyk OAS APIs
- OAS 3.1 features:
    - Full JSON Schema Support and $schema keyword
    - The single `example` keyword is deprecated in OAS 3.1
    - `type` can be an array
    - exclusiveMinimum and exclusiveMaximum keywords

We do not yet have support for all new features. For more details, see the [documentation](/api-management/gateway-config-tyk-oas#openapi-specification-3-1).

**Simplified Management of Versioned Tyk OAS APIs**

Managing versioned API hierarchies is now much easier and safer. We've introduced the ability to promote any child API version to become the new base (routing) API directly from the Dashboard UI, seamlessly transferring routing configurations without service disruption.

Additionally, we've improved the experience of deleting a base API. Instead of manually cleaning up orphaned child APIs, you are now presented with clear, intelligent options: promote a child to the new base, delete all associated child APIs together, or leave them as independent APIs. These enhancements eliminate manual cleanup work and give administrators full control over their API lifecycles.

**Enhanced Security with Client Certificate-Token Binding**

To provide an additional layer of security for your APIs, we've introduced Dashboard support for [Client Certificate-Token Binding](/api-management/authentication/bearer-token#client-certificate-token-binding). This feature allows you to form a strict binding association between an Auth Token issued to a client and their specific client certificate.

By ensuring that a token can be used only with its bound certificate, you can significantly reduce the risk of token theft or misuse. You can now easily manage these bindings directly from the Dashboard when creating or modifying keys, with full support for certificate rotation scenarios by allowing multiple certificates to be bound to a single key.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.12.0).

### 5.11.1
In this release, we have fixed some priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.11.1).

### 5.11.0
Tyk 5.11 delivers security enhancements, streamlined developer workflows, and deeper operational visibility - empowering teams to scale their API programs with confidence and efficiency.

**Strengthened API Security & Authentication**
This release advances our security foundation with enhanced JWT authentication capabilities. Teams can now leverage scope-to-policy mapping without requiring default policies, while new support for nested claims enables more granular policy and subject identification. We've also added IP spoofing protection through configurable depth selection in X-Forwarded-For headers, and introduced a new Dashboard API endpoint that allows admins to clear JWKS cache across all connected Gateways.

**Improved Certificate Management & Visibility**
Certificate management becomes more proactive with enhanced visibility showing which APIs are impacted by each certificate, plus highlighting certificates approaching expiry to prevent service disruptions before they occur.

**Enhanced Developer Experience**
API integration gets simpler with automatically populated OpenAPI descriptions that include all accessible URLs based on deployment targets and version identifiers. This reduces integration errors and accelerates time-to-value for API consumers.

**Advanced Operational Control**
Operations teams gain greater flexibility with the ability to temporarily remove targets from upstream load balancers during maintenance windows. Enhanced observability comes through OTel trace propagation to custom gRPC plugins, trace ID inclusion in API traffic logs, and dedicated Gateway latency metrics alongside upstream measurements.

**Stability & User Experience Improvements**
This release also includes important fixes for memory exhaustion issues when Redis logging is enabled, improved consistency between Dashboard and Gateway validation for JWT configurations, and streamlined policy management with standardized Policy ID usage across the platform. Multiple UI improvements enhance the user experience including fixes for API filtering, certificate search functionality, Quick Start onboarding flow, and various dashboard display issues - ensuring a smoother workflow for API management and configuration tasks.

These enhancements collectively strengthen Tyk's position as the platform of choice for organizations requiring enterprise-scale API management with robust security, operational excellence, and developer productivity.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.11.0).

### 5.10.2
This patch release fixes an issue in the API Designer where JWKS cache timeout values could not be saved properly. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.10.2).

### 5.10.1
This patch release upgrades the Go build environment and delivers UI, analytics, and security fixes. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.10.1).

### 5.10.0
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.10.0).

##### Streamlined API Versioning Experience

The Tyk Dashboard now provides a completely redesigned versioning experience for Tyk OAS APIs, making API version management intuitive and efficient through guided workflows and centralized controls.

**Intuitive version creation**

- **Step-by-step wizard**: Guided process for creating new API versions with clear configuration options at each step
- **Smart configuration cloning**: Choose to inherit settings from existing versions or start fresh
- **Flexible publishing**: Control version activation and Gateway deployment during creation
- **Pre-configuration support**: Set up versioning parameters before creating any versions, preparing APIs for future versioning needs

**Centralized version management**

- **Unified "Versions" tab**: Single location to view and manage all aspects of API versioning
- **Clear configuration visibility**: Version identifier settings, proxy options, and version lists displayed in one organized interface
- **Inline editing**: Modify version names and configuration directly without navigating between screens
- **Consistent experience**: Same interface and capabilities whether working with base or child APIs

**Key benefits**

- Eliminate confusion around version setup and management
- Reduce time spent navigating between different configuration screens
- Enable proactive versioning preparation for future API evolution
- Provide clear visibility into version configuration and relationships

Perfect for teams managing multiple API versions or planning version rollout strategies, this enhancement makes API versioning accessible to users of all experience levels while maintaining the power and flexibility that advanced users require.


##### Certificate Expiry Monitoring and Notifications

The Tyk Dashboard now provides proactive certificate lifecycle management to help prevent service outages caused by expired mTLS certificates.

Proactive monitoring capabilities:
- **Event-driven alerts**: Certificate expiry events are now available in the Tyk OAS API Designer for webhook and event handler configuration
- **Dashboard API notifications**: New endpoint provides programmatic access to certificate status information
  - **Smart monitoring**: Automatic detection of certificates approaching expiry or already expired with configurable warning thresholds
  - **Duplicate prevention**: Intelligent notification system prevents alert flooding while ensuring visibility

**Key benefits**

- Prevent unexpected API outages due to expired certificates
- Enable automated certificate renewal workflows through event handlers
- Provide clear visibility into certificate health across your API infrastructure
- Support integration with existing monitoring and alerting systems

Perfect for organizations managing multiple certificates across complex API infrastructures where manual certificate tracking becomes impractical.

For more details, please see the dedicated [Gateway events](/api-management/gateway-events) section.


### 5.9.2
This release fixes a compatibility issue between MDCB and Dashboard where APIs containing dots (.) in their paths were not handled correctly in MDCB. API definitions are now processed consistently with the Dashboard, ensuring middleware works as expected across all gateways.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.9.2).

## Change Log

### 5.8.14
#### Changelog
<a id="Changelog-v5.8.14" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed JavaScript regex unicode escape sequence handling during OpenAPI import'>
We have resolved an issue where JavaScript-style Unicode regex patterns in OpenAPI documents failed validation during API import.

Previously, when regex patterns containing Unicode escape sequences (for example `\u0000-\u017f`) were defined with single quotes in YAML, the YAML-to-JSON conversion process would double-escape the backslashes, transforming valid `\u` sequences into invalid `\\u` sequences that were rejected by Tyk's regex validator.

The system now automatically translates these escape sequences during API ingestion, ensuring that OpenAPI documents with JavaScript regex patterns import successfully regardless of whether single or double quotes are used in the YAML definition.
</Accordion>

<Accordion title='Fixed behavior of Filter by APIs option in certificate list'>
We have resolved an issue where the "Filter by API" feature on the TLS/SSL Certificates page did not return all certificates linked to an API.

Previously, the filter only displayed certificates used for client-to-gateway mTLS, ignoring certificates used for gateway-to-upstream mTLS, custom domain certificates, and certificate pinning. This resulted in incomplete and misleading results for users trying to view all certificates actively used by a given API.

The Dashboard now correctly returns all certificates when filtering by API.
</Accordion>

<Accordion title='Fix API expiry date drift'>
We have resolved an issue where an API's expiry date could drift back by one day each time the API was saved in the API Designer. This was reported by users in positive UTC timezones (such as UTC+8). Saving an API without changing the expiry is now fully idempotent, and the expiry date remains stable across repeated saves regardless of the user's timezone.
</Accordion>

<Accordion title='Resolved Gateway registration failures at scale with Unlimited Node licenses'>
We have resolved a set of related issues affecting Gateway registration with the Dashboard at scale for deployments using an **unlimited node license**. During mass registrations or rolling upgrades, a combination of lock contention, excessive Redis load, and incorrect handling of `409 Conflict` responses could leave Gateways stuck in registration loops without the credentials needed to serve traffic.

Gateway registration is now significantly more robust at scale: registration requests are no longer serialized across the fleet, Gateways recover cleanly from transient `409 Conflict` responses instead of looping, and the Redis load generated during registration storms is substantially reduced.

A dedicated fix for **limited node license** deployments will be provided in an upcoming release.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
Addressed CVEs reported in dependent libraries, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33762" target="_blank">CVE-2026-33762</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-1229" target="_blank">CVE-2026-1229</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33532" target="_blank">CVE-2026-33532</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-2950" target="_blank">CVE-2026-2950</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-13465" target="_blank">CVE-2025-13465</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15599" target="_blank">CVE-2025-15599</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39882" target="_blank">CVE-2026-39882</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34165" target="_blank">CVE-2026-34165</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33671" target="_blank">CVE-2026-33671</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-4800" target="_blank">CVE-2026-4800</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39883" target="_blank">CVE-2026-39883</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33487" target="_blank">CVE-2026-33487</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-62718" target="_blank">CVE-2025-62718</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-40175" target="_blank">CVE-2026-40175</a>

</Accordion>

</AccordionGroup>


### 5.13.0
#### Changelog
<a id="Changelog-v5.13.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Add MCP Proxy management API'>
We have introduced the `/api/mcps` endpoint set, providing full CRUD operations for MCP Proxy definitions:

- `GET /api/mcps`: list MCP Proxies
- `POST /api/mcps`: create an MCP Proxy
- `GET /api/mcps/{id}`: retrieve an MCP Proxy
- `PUT /api/mcps/{id}`: update an MCP Proxy
- `DELETE /api/mcps/{id}`: delete an MCP Proxy

A schema endpoint at `/api/schemas/apidefs/mcp` returns the MCP Proxy definition JSON schema. MCP Proxy definitions are not returned by the standard API listing endpoints.

For details, see the [Managing MCP Proxies documentation](/ai-management/mcp-gateway/managing-proxies).
</Accordion>

<Accordion title='Add dedicated MCP RBAC permission group'>
We have introduced a dedicated `mcp` permission group (deny/read/write) for controlling access to MCP management endpoints in the Dashboard. Previously, `/api/mcps` endpoints fell under the `apis` permission.

Users who need to manage MCP Proxies must be granted `mcp: write`. Read-only access to MCP listings requires `mcp: read`. This change allows organizations to grant access to MCP management without also granting broader API management permissions.

For details, see the [Managing MCP proxies documentation](/ai-management/mcp-gateway/managing-proxies).
</Accordion>

<Accordion title='Add MCP access rights to Sessions and Policies'>
Sessions and Policies now support four new fields for configuring Tool-Based Access Control (TBAC) and per-primitive rate limiting on MCP Proxies:

- `json_rpc_methods`: per-method rate limits (e.g., `tools/call`, `resources/read`)
- `json_rpc_methods_access_rights`: allow or deny rules by JSON-RPC method name
- `mcp_primitives`: per-primitive rate limits keyed by type and name
- `mcp_access_rights`: allow or deny rules by primitive type and name (tool, resource, or prompt)

Validation rejects these fields on non-MCP APIs with an `HTTP 400 Bad Request` response. Unknown API IDs are skipped during validation to avoid false negatives at Gateway startup.

For details, see the [MCP Proxy policies documentation](/ai-management/mcp-gateway/policies).
</Accordion>

<Accordion title='Simplified management of Session lifetime'>
We have added a new, simplified, approach to configuring [Session lifetime](/api-management/access-control/sessions-and-keys/session-lifecycle) within Redis.

Two new fields have been added to the Session object, which can be directly configured when creating Sessions using the [Keys API](https://tyk.io/docs/api-reference/keys/create-custom-key) or [Policy API](https://tyk.io/docs/api-reference/policies/create-policy-definition), or from the key/policy management screens in the Dashboard UI:

- [post_expiry_action](/tyk-oss-gateway/configuration#post_expiry_action) - determines what happens to the data in Redis after the `expires` timestamp is reached.
- [post_expiry_grace_period](/tyk-oss-gateway/configuration#post_expiry_grace_period) - defines how long (in seconds) the Session is kept in Redis after expiration (if the `post_expiry_actions` is to retain the Session)


The existing Gateway-wide [global session lifetime](/api-management/access-control/sessions-and-keys/session-lifecycle#gateway-level-settings) override is still respected.

The [legacy API level controls](/api-management/access-control/sessions-and-keys/session-lifecycle#legacy-controls) can still be used if both new fields are set to `0` (or unset) so **there is no change in behavior for existing Sessions**.
</Accordion>


<Accordion title='Dashboard UI enhancements'>
We have introduced several improvements to the Dashboard UI to enhance security, usability, and developer productivity.

- **Masked sensitive configuration values**: Sensitive fields such as HMAC shared secrets, JWT public keys, OAuth client secrets, upstream basic auth passwords, Dashboard API access tokens, and Kafka SASL passwords are now masked by default across the API Designer, Tyk Classic and Tyk OAS API configuration forms, and Universal Data Graph data source configurations. A reveal/hide toggle allows authorised users to view values when needed, and values are automatically re-masked on page refresh.

- **Improved certificate ID visibility**: Certificate IDs are now displayed in full when space allows, and truncated from the left (preserving the distinctive fingerprint) when space is limited. The behavior is consistent across the Certificate Store, API Designers, Keys, Policies, and all certificate selection components, with the full ID always available on hover.

- **Configurable toast notification duration**: The display duration of toast notifications can now be configured separately for success, warning, info, and error messages via the new [ui.notifications.duration](/tyk-dashboard/configuration#ui-notifications-duration) configuration option, replacing the previous fixed three-second duration. This helps reduce visual noise from frequent notifications while keeping critical messages visible for longer.

- **Skip Universal Data Graph schema update confirmations**: Added a checkbox to suppress the confirmation modal when updating Universal Data Graph schemas. When selected, confirmations are skipped for the remainder of the browser session and reset on a new session, supporting faster iterative development while preserving safe defaults.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix JavaScript regex unicode escape sequence handling during OpenAPI import'>
We have resolved an issue where JavaScript-style Unicode regex patterns in OpenAPI documents failed validation during API import.

Previously, when regex patterns containing Unicode escape sequences (for example `\u0000-\u017f`) were defined with single quotes in YAML, the YAML-to-JSON conversion process would double-escape the backslashes, transforming valid `\u` sequences into invalid `\\u` sequences that were rejected by Tyk's regex validator.

The system now automatically translates these escape sequences during API ingestion, ensuring that OpenAPI documents with JavaScript regex patterns import successfully regardless of whether single or double quotes are used in the YAML definition.
</Accordion>

<Accordion title='Fix Dashboard certificate filter for APIs'>
We have resolved an issue where the Dashboard's "Filter by API" feature on the TLS/SSL Certificates page did not return all certificates linked to an API.

Previously, the filter only displayed certificates used for client-to-gateway mTLS, ignoring certificates used for gateway-to-upstream mTLS, custom domain certificates, and certificate pinning. This resulted in incomplete and misleading results for users trying to view all certificates actively used by a given API.

The Dashboard now correctly returns all certificates when filtering by API.
</Accordion>

<Accordion title='Fix Tyk OAS update failures during import'>
We have resolved an issue when importing an updated OpenAPI document to update an existing Tyk OAS APIs if the OpenAPI document contained external parameter references.

Previously, when importing OpenAPI documents that referenced multiple distinct parameters from external files (e.g., `$ref: './parameters.yaml#/ID'`, `$ref: './parameters.yaml#/Name'`), the Dashboard's import process incorrectly collapsed all external references into a single generic parameter component and duplicated that reference multiple times, creating invalid OpenAPI specifications with multiple parameters having the same name and location.

The Dashboard now properly resolves external parameter references to distinct parameter definitions during import, ensuring that each `$ref` becomes a separate, uniquely-named parameter component and maintains OAS compliance for successful updates.
</Accordion>

<Accordion title='Add strict validation for Policy ID characters'>
We have resolved an issue where policy IDs containing special characters could cause problems when parsing API endpoint requests. Previously, policy IDs with characters such as `#`, `?`, `%`, and `/` would interfere with URL parsing in Dashboard and Gateway API endpoints that use the policy ID as a path parameter, potentially causing request failures or unexpected behavior.

We have introduced strict validation across both the Dashboard and Gateway APIs to restrict policy identifiers to a safe character set (alphanumeric characters plus `_`, `-`, `.`, `~`). The validation occurs during policy creation and updates via the following endpoints:
- `POST /api/portal/policies` and `PUT /api/portal/policies/{id}` (Dashboard API)
- `POST /tyk/policies` and `PUT /tyk/policies/{polID}` (Gateway API)

The API now returns a clear error message if an unacceptable character is used in the policy ID field, ensuring reliable policy management across all deployment types.
</Accordion>

<Accordion title='JWT Subject and Policy Claim fields now migrated correctly on load'>
This release fixes a migration issue affecting Tyk OAS APIs with JWT authentication that were created on versions prior to v5.10.0 - the release in which the multiple IdP enhancement introduced the new `subjectClaims`, `basePolicyClaims`, and `jwksURIs` fields, replacing the legacy `identityBaseField` and `policyFieldName` in the Dashboard UI.
When a Tyk OAS JWT API definition created on an earlier version is now loaded in the Dashboard, the legacy field values are automatically copied into the new fields:
| Legacy Field          | New Field            |
|-----------------------|----------------------|
| `identityBaseField`   | `subjectClaims`      |
| `policyFieldName`     | `basePolicyClaims`   |

The `jwksURIs` field is also now populated correctly on initial load rather than only after saving.
Backward compatibility is preserved: the legacy fields are left untouched in the API definition, so the API will continue to work correctly with older Gateways during a phased upgrade.
</Accordion>

<Accordion title='Fix API expiry date drift'>
We have resolved an issue where an API's expiry date could drift back by one day each time the API was saved in the API Designer. This was reported by users in positive UTC timezones (such as UTC+8). Saving an API without changing the expiry is now fully idempotent, and the expiry date remains stable across repeated saves regardless of the user's timezone.
</Accordion>

<Accordion title='Resolve Gateway registration failures at scale on unlimited node licenses'>
We have resolved a set of related issues affecting Gateway registration with the Dashboard at scale for deployments using an **unlimited node license**. During mass registrations or rolling upgrades, a combination of lock contention, excessive Redis load, and incorrect handling of `409 Conflict` responses could leave Gateways stuck in registration loops without the credentials needed to serve traffic.

Gateway registration is now significantly more robust at scale: registration requests are no longer serialized across the fleet, Gateways recover cleanly from transient `409 Conflict` responses instead of looping, and the Redis load generated during registration storms is substantially reduced.

A dedicated fix for **limited node license** deployments will be provided in an upcoming release. 

</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Fix CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33762" target="_blank">CVE-2026-33762</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-1229" target="_blank">CVE-2026-1229</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33812" target="_blank">CVE-2026-33812</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33532" target="_blank">CVE-2026-33532</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-2950" target="_blank">CVE-2026-2950</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-13465" target="_blank">CVE-2025-13465</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15599" target="_blank">CVE-2025-15599</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39882" target="_blank">CVE-2026-39882</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34165" target="_blank">CVE-2026-34165</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33671" target="_blank">CVE-2026-33671</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-4800" target="_blank">CVE-2026-4800</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39883" target="_blank">CVE-2026-39883</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-15558" target="_blank">CVE-2025-15558</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33487" target="_blank">CVE-2026-33487</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-62718" target="_blank">CVE-2025-62718</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-40175" target="_blank">CVE-2026-40175</a>

</Accordion>

</AccordionGroup>

### 5.12.1
#### Changelog
<a id="Changelog-v5.12.1" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Updated Golang version to 1.25'>
The Tyk Dashboard has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

<Accordion title='Update Docker images to Debian 13 (Trixie)'>
Updated the Docker images to Debian 13 (Trixie) to address multiple vulnerabilities in the underlying operating system.
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
- Updated `exclusiveMinimum` and `exclusiveMaximum` to support numeric values instead of booleans, aligning with OAS 3.1 standards.
- Deprecated the single `example` keyword in accordance with the OAS 3.1 specification.

Please note the following limitations in this initial release:
- Conversion from OAS 3.0 to OAS 3.1 is not yet supported.
- Reusable Path Item Objects and the new `mutualTLS` security scheme are not currently supported.
- Tyk Dashboard's API Editor does not yet validate the schema in real-time; validation is performed only when saving the API.
</Accordion>

<Accordion title='Added base API reassignment and intelligent deletion for versioned Tyk OAS APIs'>
This release introduces comprehensive UI and API support for restructuring versioned Tyk OAS API hierarchies, eliminating the manual work previously required to manage orphaned APIs:

- Added the ability to promote a child API to become the new base API directly from the versions table, automatically reassigning the existing base API as a child version.
- Enhanced the API Designer deletion workflow for base APIs to present three options: promote a child to base, delete all child versions (bulk deletion), or leave child APIs orphaned (previous behavior).
- Extended the Dashboard API with a new `PATCH /api/apis/oas/{current-base-api-id}/{next-base-api-id}` endpoint to programmatically convert a child API into the base API.
- Added an optional `purge` parameter to the `DELETE /api/apis/oas/{apiId}` endpoint to enable bulk deletion of all child APIs when a base API is deleted.
</Accordion>

<Accordion title='JSON Log Format Support for Dashboard'>
The Tyk Dashboard now supports [application logging](/api-management/logs#application-logs) in structured JSON format. To enable this feature, set the environment variable `TYK_DB_LOGFORMAT` to `json`.

This feature provides consistency with the Gateway's JSON log format option (introduced in Tyk 5.6.0) and enables better log ingestion by centralized logging systems, SIEM tools, and observability platforms. The configuration is case-insensitive and falls back to the standard text format for any invalid values, ensuring backward compatibility and reliable operation.
</Accordion>

<Accordion title='Full Support for Custom Policy IDs in Dashboard API'>
The Dashboard API's policy management endpoints now support the recommended Policy ID (`id`) for all operations. Previously the API only supported the Policy Database ID (`_id`) for some operations, which could cause problems when  migrating policies between environments. For reasons of backward compatibility, the Policy Database ID is still supported.

Users are advised to avoid duplicate Policy IDs to minimise the risk of ambiguous interpretation.

This enhancement streamlines policy management for users who prefer meaningful, custom identifiers over auto-generated database IDs, while ensuring existing workflows remain unaffected.
</Accordion>

<Accordion title='Added Dashboard support for Client Certificate-Token Binding'>
This release introduces Dashboard UI and API support for managing Client Certificate-Token bindings for APIs secured with a static mTLS allow list. This allows administrators to easily configure enhanced token security:

- Added the ability to bind one or more client certificates to a key (token) when creating or modifying it via the Dashboard.
- Supports binding multiple client certificates to a single key to facilitate certificate rotation scenarios.
- Maintains full backward compatibility with existing keys that do not specify certificate bindings.

Please note that bound certificates must also be present in the client certificate allow list within the API definition to ensure successful validation after the mTLS handshake.
</Accordion>

<Accordion title='Replaced Dynamic mTLS with Certificate Authentication'>
Renamed "Dynamic mTLS" to "Certificate Authentication" across the Tyk Classic and Tyk OAS API Designers and Key Management screens. It is now presented as a standalone authentication method rather than a sub-option of Auth Token, making it more intuitive to configure and aligning the UI with the Gateway's updated configuration structure.
</Accordion>

<Accordion title='Improved middleware visualization in the API Debugger'>
Enhanced the visual timeline in the API Designer's debug panel to make request tracing easier. The "Inspect" tab now clearly differentiates between Request and Response middleware chains with distinct labels and colors, and visually isolates the Reverse Proxy step so you can easily see exactly where the request was sent upstream.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed GeoIP Location Data Display in SQL Analytics Storage'>
Resolved issues preventing GeoIP location data from displaying correctly when using SQL storage for analytics. Previously, the Log Browser showed "GeoIP not recognized" for country codes, and geographic traffic filtering returned empty results, even though location data was properly captured and stored in the database. 

The Dashboard can now correctly read and display all geographic information, including country codes, city names, coordinates, and timezone data from SQL storage. Geographic traffic filtering by country code now works as expected, enabling users to analyze traffic patterns by location when using SQL-based analytics storage.
</Accordion>

<Accordion title='Fixed API-Specific Key Retrieval in Dashboard'>
Resolved an issue where the Dashboard's `GET /api/apis/{api_id}/keys` endpoint incorrectly returned all keys in the environment instead of only keys associated with the specified API. Users can now accurately retrieve and manage keys for individual APIs, eliminating confusion in multi-API environments. Note that adding this filtering can cause minor delays in responses to this endpoint when there are many APIs and keys.
</Accordion>

<Accordion title='Issues Associating Client Certificates with API Keys and Policy ID Display'>
Resolved multiple Dashboard UI issues affecting mTLS configuration and policy management:
- **Certificate Authentication Configuration**: Fixed an issue where the Create Key and Edit Key screens would incorrectly require the user to register a client certificate with the key when granting access to an API secured with static mTLS. Additionally, fixed a separate issue where the option to assign the client certificate was not possible when creating a key for an API which has the authentication method changed to use dynamic mTLS. 
- **Policy ID Display Consistency**: Updated the Dashboard UI to consistently display the policy `id` field instead of the MongoDB database `_id` field across all policy listings, detail views, and selection components. This ensures that policy IDs shown in the UI work reliably with JWT `pol` claims regardless of how policies were created (Dashboard UI vs. Operator) or Gateway configuration settings. Copy-to-clipboard functionality now copies the correct `id` field, and policy selection dropdowns use consistent identifiers.

These fixes eliminate confusing certificate linking requirements, ensure consistent dynamic mTLS key creation regardless of when certificate authentication is enabled, provide clearer separation between different mTLS authentication modes, and resolve JWT authentication failures caused by users copying database IDs from the UI instead of the proper policy identifiers.
</Accordion>

<Accordion title='Fixed Certificate Re-use After Swapping in Multi-Auth Keys'>
Resolved an issue where swapping certificates in multi-auth (mTLS + Basic auth) keys prevented the original certificate from being reused. Previously, when updating a key's certificate, the original certificate remained incorrectly associated with the key internally, causing "key with given certificate already found" errors when attempting to reuse that certificate. 

Tyk now properly detaches certificates during key updates, allowing certificates to be freely reused across different keys after being removed from their original association.
</Accordion>

<Accordion title='Improved Experience when License Missing or Expired'>
Fixed inaccurate wording and links on the screen presented on startup when the Dashboard license is missing or has expired.
</Accordion>


</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Security Vulnerabilities Fixed'> 
Multiple high-severity CVEs have been fixed. 
</Accordion>

</AccordionGroup>

### 5.11.1
#### Changelog
<a id="Changelog-v5.11.1" data-scroll-offset></a>

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

<Accordion title='Improved Flexibility for Key Expiration Settings'>
We've improved key expiration flexibility in the Tyk Dashboard by adding support for custom expiration periods using human-readable time formats (e.g., "5h,34m,30s"). Users can now input custom timeframes supporting months, weeks, days, hours, and seconds in addition to the existing predefined options, eliminating the previous restriction and removing the need for API calls or manual key revocation for custom expiration times.
</Accordion>

<Accordion title='Open Policy Agent Support for Distinguishing Tyk OAS APIs'>
Added three new OPA helper functions (`isTykOas`, `isTykClassic`, and `isTykStreams`) that can be used in OPA rules to distinguish between different API types, ensuring that logic is only applied to the appropriate API types.
</Accordion>

<Accordion title='Added API Endpoint for Certificate Dependencies'>
Introduced a new Dashboard API endpoint `GET /certs/{certificateId}/apis` that retrieves all APIs linked to a specific certificate. The endpoint returns API names, IDs, and how they are used by each API (client, upstream, server) with proper pagination support, enabling the frontend to display certificate impact information without expensive client-side filtering across all APIs.
</Accordion>

<Accordion title='Improvements in the Dashboard UI'>
- **Proactive Certificate Expiry Notifications in Tyk Dashboard**: Added banner notification to inform the user if the Dashboard has received a certificate expiring soon/expired event from the Gateway. Such certificates are now highlighted on the certificates screen.
- **Improved Automatic Configuration Options for API Creation**: Enhanced the user experience when importing OpenAPI descriptions by replacing checkboxes with toggle switches and improving the wording of configuration options. The new interface provides clearer descriptions for Request Validation, Mock Responses, Authentication Detection, and Path Restrictions.
- **Enhanced Certificate Management with Impact Visibility**: Introduced a new Certificate Details page that displays user-friendly certificate metadata with a toggle between formatted and raw JSON views. The page now shows certificate status indicators (valid, expiring soon, expired) and lists all APIs using the certificate with clickable navigation to their API Designer pages.
</Accordion>

<Accordion title='Improved visibility of API base paths'>
Tyk has always added the base path to the `servers` section of an API's OpenAPI description. Until now, this has been constructed from the address of the local Gateway with the unique listen path for the API. This is not always accessible, for example for child API versions configured for internal access.

Now Tyk will calculate all the URLs where the API can be accessed, adding these to the `servers` section of the OpenAPI description. This takes into account the versioning identifier if URL location is in use (e.g. `<gateway_url>/<listen_path>/v1`) and, where `edge_endpoints` are defined in the Dashboard config, the data plane Gateways where the API is deployed.

We have introduced a new Dashboard API endpoint (`GET /api/apis/oas/{apiID}/urls`) that returns a list of all of these base paths, presented in a structured format including detailed components (protocol, domain, port, listen-path, versioning identifier).

The API Designer uses the data from this endpoint to display all known API URLs and also indicates how to select the specific API version (if applicable) using URL, query parameter or header.
</Accordion>

<Accordion title='Removed Default Policy Requirement for JWT Scope-to-Policy Mapping'>
We’ve removed the need to supply a default policy when using scope-to-policy mapping with JWT Authentication. Now, if you enable scope-to-policy mapping by configuring `scopes.claimName`, you do not need to provide a policy ID in `defaultPolicies`. If a request does not contain any valid scopes, it will be rejected with `HTTP 403 Forbidden` (default deny). You can still provide a default policy if you require a different behaviour.
</Accordion>

<Accordion title='Added JWKS cache flush to the Dashboard API and MDCB'>
Added JWKS cache invalidation endpoint `DELETE /api/cache/jwks/{apiID}` to the Dashboard API to flush JWKS cache for specific APIs across all connected Gateways. This extends the existing Gateway-level JWKS cache flush functionality to work through the Dashboard API with proper MDCB propagation to data plane Gateways. Access is restricted to admin users with API ownership validation, ensuring secure cache management across distributed Gateway deployments.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Dashboard Memory Exhaustion When `use_redis_log` Is Enabled'>
Fixed an issue where enabling `use_redis_log` on a Tyk Gateway could cause the Dashboard to exhaust memory due to unbounded log accumulation from the UI notification endpoint.
</Accordion>

<Accordion title='Dashboard Now Corrects Padding for Base64-Encoded JWKS URLs'>
Fixed an issue where the Dashboard and Gateway had inconsistent validation for base64-encoded JWKS URIs when using JWT authentication. Previously, the Dashboard would accept a URI in the `source` (or `jwt_source` for Tyk Classic) field that was not correctly padded, while the Gateway required correct padding. This could cause an API to be saved successfully in the Dashboard but then fail when processed by the Gateway. The fix has been applied to the Dashboard, which now automatically adds the required padding to the base64-encoded string when the API definition is saved. This resolves the inconsistency and ensures the configuration is valid for the Gateway.
</Accordion>

<Accordion title='Fixed Certificate Filtering Not Working in Non-Detailed Mode'>
Fixed an issue where certificate filtering (`filter=with_pk`, `filter=without_pk`) was only applied in detailed mode, causing inconsistent behavior. Tyk now properly applies filtering in both detailed and non-detailed modes, ensuring consistent results regardless of the mode parameter used.
</Accordion>

<Accordion title='Fixed Policy Deletion Failure When Referenced API No Longer Exists'>
Fixed an issue where the user was unable to delete the policy if the referenced API didn't exist, even though the user had access to the policy before API deletion.
</Accordion>

<Accordion title='Renamed `XTykApiGateway` Schema to Avoid Case-Sensitive Conflicts'>
Renamed `XTykApiGateway` schema to `TykVendorExtension` to eliminate case-only naming conflicts with the existing `XTykAPIGateway` schema. Updated all corresponding $ref references throughout swagger.yml to prevent code generation ambiguity and ensure compatibility across case-insensitive file systems.
</Accordion>

<Accordion title='Standardised Use of Policy IDs Across Tyk Gateway and Dashboard'>
The Tyk Policy object has two identifiers `id` (Policy ID) and `_id` (Policy Database ID). All policy definitions will have a value in `_id` when loaded to the Dashboard, used to identify the object in the persistent storage. Historically, the Policy ID (`id`) has been optional (via the `allow_explicit_policy_id` Gateway configuration), and different Tyk components have used one or the other, leading to inconsistency and scenarios where a policy might be rejected due to the configuration of IDs.
We have simplified the Tyk Gateway to use the Policy ID exclusively, instead of the Policy Database ID, improving consistency across the platform. This means that the `allow_explicit_policy_id` Gateway configuration is now redundant and marked deprecated, as the Policy ID field is always used.
If a policy definition is loaded onto Tyk Dashboard that does not have a Policy ID set, Tyk will automatically set the `id` field to the value in `_id`. There are no changes to the Gateway or Dashboard APIs from this enhancement, ensuring backward compatibility.
</Accordion>

<Accordion title='Dashboard UI Fixes'>
- **Incorrect Data Range Display in Total API Traffic Dashboard Chart**: Fixed an issue where the **Total API Traffic** chart on the **Nodes & Licenses** screen displays data from the wrong time period.
- **TIB User Role Mappings Disappeared on Update**: Fixed a bug that caused TIB profiles to vanish on updates made to `Provider Configuration` advanced settings.
- **Access Tab Selection from Filtered Dropdown**: Fixed an issue where users and categories could not be selected from filtered dropdown lists in the Access tab of the Tyk OAS API designer.
- **Unable to Apply Multiple Filters to API List**: Resolved filtering issues where applying multiple filters (name + category) would drop previously applied filters instead of combining them. Users can now apply multiple filters that work together as expected, with all active filters preserved during pagination and sorting.
- **Data Graph Wizard Incorrectly Displayed**: Fixed an issue where the Data Graph onboarding wizard incorrectly appeared when filters returned zero results - it now only shows when no APIs actually exist in the system, keeping filters visible when results are empty.
- **Incomplete Certificate Search in API Designer**: Fixed an issue when selecting a TLS certificate to attach to a Tyk OAS API (for example, a client certificate for client-gateway mTLS). To improve performance when scrolling through the list of existing certificates, the UI uses pagination to load a subset of certificates at a time. The certificate ID search would only match against the currently loaded page of certificates, so a certificate might not appear to exist when it does, but just hasn't been loaded yet. Now the search functionality has been improved to look through the full list of certificates in the Tyk Certificate Store, rather than relying on the paginated data.
</Accordion>

<Accordion title='Fixed Quick Start API Testing Issues'>
Resolved multiple issues preventing successful API testing in the Quick Start onboarding flow. Users can now test APIs with any response type (JSON, HTML, XML) without encountering HTTP 500 errors or blank response displays.
Additionally, fixed URL handling issues that occurred when gateway endpoints were configured without URL schemas, ensuring the "Test your API" step works reliably regardless of configuration format. 
</Accordion>

<Accordion title='Removed Redundant Boolean Enums from OpenAPI Specification'>
Fixed redundant boolean enum definitions in OpenAPI specification by removing unnecessary enum: [true, false] declarations from boolean type fields in swagger.yml files. Boolean parameters now use only type: boolean, following OpenAPI best practices.
</Accordion>

</AccordionGroup>

### 5.10.2
#### Changelog
<a id="Changelog-v5.10.2" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fix JWKS Cache Timeout Setting in API Designer'>
Fixed an issue where JWKS cache timeout values when configuring JWT authentication could not be saved in the API Designer. The timeout field would appear to accept values, but would disappear after saving the API configuration. Users can now successfully configure and persist JWKS cache timeout settings as expected
</Accordion>

</AccordionGroup>

### 5.10.1
#### Changelog
<a id="Changelog-v5.10.1" data-scroll-offset></a>

##### Changed

<AccordionGroup>
<Accordion title='Upgraded Go build environment to Debian 12 ("Bookworm")'>
We have updated the Go build environment from Debian 11 ("Bullseye") to Debian 12 ("Bookworm") across all pipelines. This change ensures that all builds use the latest Go 1.24 patch version, addressing recent CVEs and improving overall security and stability.
</Accordion>

</AccordionGroup>

##### Added

<AccordionGroup>

<Accordion title='Enabled Gzip Compression for Static Assets to Improve Dashboard Load Performance'>
Implemented gzip compression for static assets (JavaScript, CSS, images, etc.) when the browser client requests for gzip with the `Accepted-Encoding` header. This significantly reduces the file size transferred when loading the Dashboard, reducing bandwidth usage and improving page load times for users.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='API Editor UI Glitch When Scrolling in Import Mode'>
Fixed an issue introduced in 5.10.0 where there was a graphical glitch with the code editor in the API Designer.
</Accordion>

<Accordion title='Dashboard Analytics and Monitoring Fixes'>
- **Fixed non-clickable endpoint rows in the Activity page**: Fixed an issue where selecting an endpoint in the "Most Popular Endpoints" list on the "Activity Overview" screen did not direct the user to the "Activity by Endpoint" screen.
- **Fixed incorrect error code descriptions in API activity dashboard**: Error codes now display correct descriptions (409 shows "Conflict" instead of "Rate limit or quota exceeded", and missing descriptions for 502, 504, 499, and 422 have been added).
- **Fixed unicode character display in Activity Logs view**: Non-ASCII characters (Cyrillic, Arabic, Hindi, Telugu, Yoruba, etc.) now display correctly instead of showing garbled text when viewing request/response logs.
- **Fixed date range filtering showing extra day in analytics charts**: Date range selectors now accurately reflect the selected end date instead of automatically including the following day's data in charts and legends.
- **Fixed Log Browser querying wrong tables when SQL table sharding is enabled**: Dashboard now correctly queries sharded tables (tyk_analytics_YYYYMMDD) instead of the main tyk_analytics table when `TYK_DB_STORAGE_LOGS_TABLESHARDING=true` is configured, ensuring analytics data displays properly with SQL database sharding.
- **Fixed incorrect date labels and data aggregation in analytics charts**: Fixed multiple issues in the analytics aggregation layer when using PostgreSQL backend that caused incorrect chart rendering and service problems. Resolved problems, including hourly charts showing nonsensical dates like "30 Nov 1899", monthly charts displaying incorrect months, incomplete time-series data due to improper date padding, and API activity being incorrectly split across multiple rows.
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
<Accordion title='Enhanced versioning experience for Tyk OAS APIs'>
Completely redesigned the versioning experience for Tyk OAS APIs with an intuitive wizard-driven workflow and centralized version management interface.

**New version creation wizard**

- **Guided configuration process**: Step-by-step wizard for creating new API versions with clear decision points
- **Configuration cloning options**: Choose whether to clone settings from an existing version, with selection from available versions when multiple exist
- **Version identifier setup**: Configure version location (header, URL path, query parameter) and key name if not already set
- **Publishing controls**: Decide whether to immediately activate the new version and select target Gateways using segment tags

**Centralized version management**

- **New "Versions" tab**: Unified interface displaying version identifier configuration and complete version list for both base and child APIs
- **Pre-configuration support**: Set up version identifier location, key name, and proxy options before creating any child versions, preparing non-versioned APIs to become base APIs
- **Clear configuration visibility**: Version identifier and proxy settings prominently displayed above the version list
- **Inline editing capabilities**: Edit version names directly for any API version, and modify versioning configuration from the base API
- **Streamlined access**: Create new versions from any API (base or child) with direct access to the creation wizard

**Improved user experience**

- Removed legacy version management screens that were difficult to locate
- Consistent versioning interface across all Tyk OAS APIs
- Reduced complexity in version setup and management workflows

This enhancement significantly simplifies API versioning workflows and provides better visibility into version configuration and management.
</Accordion>

<Accordion title='Certificate expiry notifications and event handling'>
Added certificate expiry monitoring capabilities to help administrators proactively manage certificate lifecycles and prevent service outages.

**Event handling integration**

- Certificate expiry events (`CertificateExpiringSoon` and `CertificateExpired`) are now available as selectable options in the event handling section, enabling users to assign webhooks or other event handlers directly through the UI

**Dashboard notifications system**

- **Proactive notification endpoint**: New `GET /api/org/notifications` Dashboard API endpoint provides organization-specific notifications for expiring and expired certificates
- **Smart monitoring**: In-memory notification repository automatically checks certificate metadata storage and creates notifications for certificates approaching expiry or already expired
- **Configurable thresholds**: Dashboard configuration options for refresh intervals and warning thresholds:

**Notification details**

- **Severity classification**: Notifications marked as "warning" for soon-to-expire certificates or "critical" for expired certificates
- **Rich metadata**: Each notification includes certificate ID, expiry date, days remaining, and other relevant details
- **Duplicate prevention**: Hash-based system prevents duplicate notifications for the same certificate status

**Note:** This release provides the foundational API and event integration for certificate monitoring. Enhanced UI functionality for certificate management will be available in a future release.

This enhancement provides multiple layers of certificate expiry visibility through Gateway events and API-based notifications, ensuring administrators can maintain certificate health across their API infrastructure.
</Accordion>

<Accordion title='Enhanced JWT claims configuration for Tyk OAS APIs'>
Updated the Tyk OAS API Designer to support multiple claim sources for JWT authentication, enabling multi-Identity Provider scenarios where different providers use different claim names.

**UI enhancements**

- **Multiple subject claims**: Replace the single "Subject identity claim" field with support for multiple claim sources
- **Multiple policy claims**: Replace the single "Policy claim" field with support for multiple claim mapping sources
- **Multiple scope claims**: Replace the single "Scope claim" field with support for multiple scope claim sources

**Current implementation**

- Updated API editor schema to accept the new multi-value claim fields
- Multi-value claim configuration available through the API Designer interface
- Advanced JWT validation features (custom claims framework, issuer/audience/subject validation, JWT ID enforcement) must be configured directly in the API definition via the API editor or external API calls
- Existing single-value configurations remain functional for backward compatibility

This enhancement supports scenarios where different Identity Providers use different claim names (e.g., Keycloak's `scope` vs Okta's `scp`) within the same API configuration, laying the foundation for comprehensive JWT claim validation workflows.

**Note:** Full API Designer integration for these fields will be available in a future release.
</Accordion>

<Accordion title='OpenAPI compliant multi-authentication configuration for Tyk OAS APIs'>
Added initial support for OpenAPI Specification compliant multi-authentication configuration in Tyk OAS APIs, enabling flexible authentication workflows that follow standard OpenAPI security patterns.

**UI enhancements**

- We have added a new toggle in the Tyk OAS API Designer's *Server > Authentication* section to choose between "legacy" and "compliant" authentication processing modes when Multiple Authentication Methods is selected:
  - **Legacy mode**: Existing configuration interface remains available for legacy mode behavior (AND logic for all authentication methods)
  - **Compliant mode**: Users selecting compliant mode are directed to configure authentication directly in the API editor for full OpenAPI security specification support

**Current implementation**

- Manual configuration of compliant mode security settings available through the API definition editor
- OpenAPI import with automatic authentication configuration continues to configure legacy mode by default (no change to existing behavior)
- Advanced authentication combinations (OR logic between security entries) must be configured directly in the API definition

This enhancement provides the foundation for OpenAPI compliant authentication workflows while maintaining complete backward compatibility with existing authentication configurations.

**Note:** Full integration for compliant mode authentication configuration will be available in a future release.
</Accordion>
</AccordionGroup>




##### Changed



<Expandable title='Upgrade Tyk Dashboard to Golang 1.24'>
The Tyk Dashboard has been updated to [Golang 1.24](https://tip.golang.org/doc/go1.24), improving security by staying current with the latest Go versions.
</Expandable>





##### Fixed




<AccordionGroup>
<Accordion title='Fixed Policy and Key Management UI for versioned APIs'>
Fixed UI issues in policy and key management that caused confusion and unnecessary validation errors. The API Versions field in the Dashboard UI now appears only when relevant - specifically for versioned Tyk Classic APIs. 

The field is no longer displayed for Tyk OAS APIs or non-versioned Tyk Classic APIs, eliminating confusion about when version selection is required and preventing policies and keys from failing to save due to irrelevant validation requirements.
</Accordion>

<Accordion title='Fixed issues with Tyk OAS API Debugger'>
Fixed some issues in the Tyk OAS API Debugger (Test Your API panel) when inspecting API tests:

- The debugger only displayed request middleware execution, omitting response middleware from the debug output
- The debugger did not show the details of the transformations applied by Request Body Transform and Request Header Transform middleware
- The debugger incorrectly reported errors for endpoints using Response Body Transform middleware, even when API calls completed successfully

The test debugger now correctly shows both request and response middleware execution, accurately displays the execution status, and eliminates false error messages that could mislead developers during API testing and troubleshooting.
</Accordion>

<Accordion title='Fixed Dashboard default page_size behavior'>
Fixed an issue where the Tyk Dashboard did not correctly apply a default `page_size` value when none was specified in the Dashboard configuration, potentially causing unexpected pagination behavior. 

The Dashboard now properly defaults to a page size of 10 items as documented, ensuring consistent and predictable pagination across all Dashboard views.
</Accordion>

<Accordion title='Fixed multiple issues with the creation of child versions of Tyk OAS APIs'>
Fixed several issues that affected the creation of new child versions of Tyk OAS APIs to ensure reliable version creation and proper validation:

UI and API Creation:
- Resolved an issue that prevented users from creating new versions via the API Designer's Manage Versions screen
- Added validation for the `base_api_id` parameter - providing a non-existent ID would previously create the API successfully, but leave it invisible in the Dashboard UI
- Added stricter validation for version names - users can no longer create API versions without specifying a valid `new_version_name`, preventing unusable or empty version entries
- Improved error messaging when the `base_api_version_name` parameter is missing or incorrectly specified

Version Management:
- Fixed an issue where creating new child versions would incorrectly reset the default version back to the base API, overriding previously configured settings

The system now provides comprehensive validation with clear error responses (`HTTP 400 Bad Request` and `HTTP 422 Unprocessable Entity`), ensures that all API versions have meaningful identifiers, and maintains proper default version settings during the creation of child versions.
</Accordion>

<Accordion title='Fixed `/versions` endpoint to only accept valid Tyk OAS base APIs'>
Fixed an issue where the `/api/apis/oas/{apiId}/versions` endpoint incorrectly returned version data for Tyk Classic APIs and non-versioned Tyk OAS APIs. The endpoint now properly validates requests and returns `HTTP 422 Unprocessable Entity` when the target API is not a valid Tyk OAS base API, ensuring the endpoint only returns meaningful version information.
</Accordion>

<Accordion title='Fixed OpenAPI `servers` section handling for regex-based custom domains'>
Fixed an issue where custom domains containing regular expressions were not correctly parsed and stored in the `servers` section of OpenAPI descriptions for Tyk OAS APIs. The Dashboard now properly converts regex-based domains into valid OpenAPI `servers` entries with appropriate variables, ensuring accurate API documentation and preventing validation errors during API editing. 

This fix includes enhanced syntax validation for regular expression (regex) patterns and improved capture group handling, which previously could cause Gateway crashes.
</Accordion>

<Accordion title='Fixed delayed application of global webhook changes for Tyk OAS APIs'>
Fixed an issue where updates to [global webhooks](/api-management/gateway-events#local-and-global-webhooks) were not immediately applied to Tyk OAS APIs using those webhooks. When global webhook configurations were modified, the Gateway would continue using the previous settings for affected Tyk OAS APIs until a manual reload occurred. 

The system now automatically triggers a Gateway reload for all impacted Tyk OAS APIs when global webhook configurations are updated, ensuring that the new webhook settings take effect immediately.
</Accordion>

<Accordion title='Fixed cross-interface compatibility for keys and policies with Tyk OAS and non-versioned Tyk Classic APIs'>
Fixed an issue where keys and policies created or updated via the Dashboard API were sometimes rejected by the Dashboard UI, and vice versa, due to inconsistent handling of the `versions` field for non-versioned Tyk Classic APIs. The issue occurred because the API and UI used different formats when populating the versions list in access rights. 

Both interfaces now consistently accept either `null` or `[]` (empty array) values in the `versions` field of the access control list, ensuring seamless interoperability between API and UI workflows for policy and key management. Tyk OAS APIs use a [different approach](/api-management/api-versioning#how-api-versioning-works-with-tyk) to versioning, with each (base or child) version having a unique API ID that is added to the access list.
</Accordion>

<Accordion title='Fixed visibility of orphaned Tyk OAS API versions when using PostgreSQL'>
Fixed an issue where orphaned child versions of a Tyk OAS API would disappear from the Dashboard UI after their base API was deleted, specifically when using PostgreSQL as the datastore. 

Orphaned Tyk OAS API versions now remain visible in the Dashboard, ensuring consistent behavior across all supported datastores and preventing loss of access to existing API versions.
</Accordion>

<Accordion title='Fixed inconsistent ordering of Tyk OAS API versions in Dashboard UI'>
Fixed an issue where the child versions of a Tyk OAS API were sorted by creation date in the **Created APIs** and alphabetically by version name (e.g., v1, v2) in the **Versions** list. 

Now versions are always sorted alphabetically by version name, providing predictable and controllable ordering.
</Accordion>

<Accordion title='Fixed Dashboard API panic when accessing logs without timestamp parameters in PostgreSQL'>
Fixed an issue where the Tyk Dashboard API would panic and return `HTTP 500 Internal Server Error` when accessing the `/api/logs` endpoint without the required `start` and `end` timestamp parameters in PostgreSQL environments using table sharding. 

The API now properly handles missing parameters by returning `HTTP 400 Bad Request` with a descriptive error message, improving error handling and API reliability.
</Accordion>

<Accordion title='Fixed PATCH endpoint validation to reject Tyk OAS API definitions when expecting OpenAPI description'>
Fixed an inconsistency where the Dashboard API's `PATCH /api/apis/oas/{apiId}` endpoint incorrectly accepted full Tyk OAS API definitions containing Tyk Vendor Extensions, when it should only accept standard OpenAPI descriptions. 

The endpoint now properly validates incoming requests and returns `HTTP 400 Bad Request` if the Tyk Vendor Extension is present, ensuring consistent behavior with the Dashboard UI and maintaining the intended separation between OpenAPI description updates and full API configuration changes.
</Accordion>

<Accordion title='Fixed incorrect creation of duplicate or blank API categories'>
Fixed an issue where duplicate or blank API categories could be created for Tyk OAS APIs when using the Dashboard API's `PUT /api/apis/oas/{API_ID}/categories` endpoint. Now, if blank or duplicate category labels are provided in the body of the `PUT` request, these will be ignored. 

This matches the validation in the API Designer which does not allow blank or duplicated categories to be assigned to APIs.
</Accordion>

<Accordion title='Fixed GraphQL API creation via upstream introspection when OPA rules modify requests'>
Fixed an issue where creating GraphQL APIs using upstream introspection in the Dashboard could fail with `HTTP 502 Bad Gateway` errors when OPA rules (typically using `patch_request`) modified the introspection request body. 

The problem occurred because the Dashboard did not recalculate the `Content-Length` header after OPA modifications, causing length mismatches that resulted in proxy errors. The Dashboard now properly recalculates the content length for modified introspection requests, ensuring reliable GraphQL API creation regardless of OPA rule configurations.
</Accordion>
</AccordionGroup>





##### Security Fixes


<Expandable title='High priority CVEs fixed'>
Fixed the following high-priority CVEs identified in the Tyk Dashboard, providing increased protection against security
vulnerabilities:<br />
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2024-47875" target="_blank">CVE-2024-47875</a><br />
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2024-45801" target="_blank">CVE-2024-45801</a>
</Expandable>

### 5.9.2
#### Changelog
<a id="Changelog-v5.9.2" data-scroll-offset></a>

##### Fixed



<Expandable title='Consistent Handling of Escaped Dots in OpenAPI Endpoint Paths'>
Resolved a compatibility issue introduced via a fix in Dashboard v5.9.0 (LTS v5.8.3) to account for DocumentDB's handling of endpoint paths containing dots (`.`). The Dashboard now escapes dot characters (e.g., `\u002e`) when storing Tyk OAS API definitions in the database, but MDCB failed to unescape them when reading from the database. This caused the [request validation](/api-management/traffic-transformation/request-validation) and [mock response](/api-management/traffic-transformation/mock-response) middleware configured on affected endpoints not to be applied. 

To align MDCB's dot handling mechanism with Tyk Dashboard, a new configuration option, `escape_dots_in_oas_paths`, has been introduced in both [Dashboard](/tyk-dashboard/configuration#escape_dots_in_oas_paths) and [MDCB](/tyk-multi-data-centre/mdcb-configuration-options#escape_dots_in_oas_paths):

* By Default, `escape_dots_in_oas_paths` is set to `false` in both MDCB and Dashboard, restoring the Dashboard behaviour before v5.9.0 (LTS v5.8.3), where dots are unescaped.
* When `escape_dots_in_oas_paths` is set to `true`, Dots are escaped for compatibility with specific databases. With this config set to `true`, MDCB and Dashboard encode/decode these paths consistently.

Check the [Upgrade and Compatibility section](#upgrade-5.9.2) for details on the recommended upgrade path.
</Expandable>


### 5.9.1
#### Changelog
<a id="Changelog-v5.9.1" data-scroll-offset></a>

##### Fixed



<Expandable title='URL Rewrite Middleware UI Compatibility Fix'>
Resolved an issue in the URL Rewrite middleware UI where the absence of the `negate` field in OAS API definition configurations caused unexpected behavior. The UI now correctly interprets a missing `negate` field in URL rewrite rules as `false` by default, ensuring compatibility with APIs created in earlier versions.
</Expandable>


### 5.9.0
#### Changelog
<a id="Changelog-v5.9.0" data-scroll-offset></a>

##### Added



<AccordionGroup>
<Accordion title='Authenticate with Multiple JWKS Providers'>
Added support for configuration of multiple JWKS (JSON Web Key Set) endpoints for Tyk OAS APIs. This enables the Gateway to authenticate JSON Web Tokens (JWTs) in multi-identity provider environments. The JWKS endpoints are configured in the new `jwksURIs` array in the JWT Auth `securityScheme`. This will take precedence over the existing `source` field, and existing API definitions will be automatically migrated to use the new field, while maintaining backward compatibility in case of rollback. Full support has been added to the Tyk OAS API Designer.
</Accordion>

<Accordion title='Valkey Database Compatibility'>
Added compatibility with Valkey database as an alternative to Redis. This is for fresh environments, with no migration support from Redis.
</Accordion>

<Accordion title='Experimental Access to Additional Input and Output Options for Tyk Streams APIs'>
We have introduced a new Dashboard configuration option, `TYK_DB_STREAMING_ENABLEALLEXPERIMENTAL`, to enable all experimental input and output options for Tyk Streams APIs. This is strictly provided for demos and MVPs and should not be enabled in production use.
</Accordion>
</AccordionGroup>



##### Changed



<Expandable title='Updated to use latest kin-openapi'>
Upgraded to use the latest upstream version of kin-openapi (v0.132.0). This ensures improved compatibility, full stack interoperability, and continued support for existing OpenAPI 3.0.x specifications.
</Expandable>



##### Fixed
<a id="Fixed-v5.9.0"></a>



<Expandable title='Tyk Streams Endpoint Incorrectly Expected `Content-Type` Header'>
Fixed an issue where the `/apis/streams/{apiID}` endpoint was expecting a `Content-Type` header instead of an `Accept` header for `GET` requests.
</Expandable>

