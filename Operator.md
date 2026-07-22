## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 1.4.2
In this release, we have fixed issues with the reliability of SecurityPolicy reconciliation under high load that could lead to infinite reconciliation loops, orphaned Dashboard policies, and inconsistent state between Kubernetes and Dashboard. The fix also introduces new configuration options that enable tuning of Tyk Operator’s HTTP connection pool for different deployments.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 1.4.1
In this release, we have addressed CVEs for enhanced security and performance.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.4.1) below.

### 1.4.0
Tyk Operator 1.4.0 introduces support for managing [MCP (Model Context Protocol) Proxy](/ai-management/mcp-gateway/managing-proxies) definitions through Kubernetes custom resources. The new `TykMcpProxyDefinition` CRD allows you to manage MCP Proxies declaratively, and the `SecurityPolicy` CRD has been extended with MCP access rights for tool-based access control and per-primitive rate limiting.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.4.0).

<Note>
The initial Helm chart for Operator 1.4.0 contained an installation bug. Please ensure you use [Tyk Charts 5.2.0](/developer-support/release-notes/helm-chart#5-2-0-release-notes) or later to install Operator 1.4.0.
</Note>

### 1.3.0
Tyk Operator v1.3.0 delivers improvements for API monitoring and ingress management in Kubernetes environments. This release adds native uptime test configuration support for both Tyk OAS and Tyk Classic API definitions, eliminating the need for manual Dashboard configuration.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.3.0) below.

<Note>
The initial `Helm chart` for Operator 1.3.0 contained an installation bug. Please ensure you use tyk-charts version [5.1.1](/developer-support/release-notes/helm-chart#5-1-1-release-notes) or later to install Operator 1.3.0.
</Note>


### 1.2.0
##### Support for Tyk 5.8

Tyk Operator v1.2 introduces key enhancements and critical fixes to improve API management in Kubernetes environments. This release adds support for HMAC request signing and YAML-based OAS definitions, aligning with Tyk Gateway 5.8.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.2.0) below.

### 1.1.0
{/* Required. Use similar ToV to previous release notes. For example for a patch release:
This release primarily focuses on bug fixes.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-vX.Y.0) below. */}
###### Support for Tyk Streams API
Tyk Operator v1.1 supports management of Tyk Streams APIs through the new **`TykStreamsApiDefinition`** custom resource. This allows you to have declarative, versioned, and fully automated control to your streaming APIs.

## Change log

### 1.4.2
#### Changelog
<a id="Changelog-v1.4.2" data-scroll-offset></a>

##### Fixed

<AccordionGroup>
<Accordion title='Fix SecurityPolicy reconciliation race conditions and connection leaks under load'>
Resolved a set of related issues that prevented `SecurityPolicy` resources from reconciling reliably, particularly under high load or when a policy referenced a large number of APIs.

Affected Operators could enter continuous reconciliation loops (most commonly with policies referencing nine or more APIs), leave a policy in the Dashboard after its Kubernetes resource had been deleted, repeatedly try to recreate a policy and fail with a "policy name already used" error, and steadily leak HTTP connections and file descriptors until the host exhausted its available ports.

SecurityPolicy reconciliation is now idempotent and converges reliably: Kubernetes and Dashboard state stay consistent across create, update, and delete, the Operator recovers cleanly from partial failures, and HTTP connections are reused correctly so file descriptors no longer accumulate under sustained load. The same reconciliation improvements have been applied to Tyk OAS API definition resources.
 
This release also introduces a set of environment variables for tuning the Operator's HTTP client to the Dashboard: `TYK_OPERATOR_HTTPCLIENTCONFIG_MAXIDLECONNS`, `TYK_OPERATOR_HTTPCLIENTCONFIG_MAXIDLECONNSPERHOST`, `TYK_OPERATOR_HTTPCLIENTCONFIG_MAXCONNSPERHOST`, `TYK_OPERATOR_HTTPCLIENTCONFIG_IDLECONNTIMEOUT`, `TYK_OPERATOR_HTTPCLIENTCONFIG_TLSHANDSHAKETIMEOUT`, and `TYK_OPERATOR_HTTPCLIENTCONFIG_TIMEOUT`. Each ships with a sensible built-in default, and Tyk recommends leaving them unchanged unless your infrastructure has specific constraints, such as file descriptor exhaustion, that require tuning.
</Accordion>
 
</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33814" target="_blank">CVE-2026-33814</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39830" target="_blank">CVE-2026-39830</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39831" target="_blank">CVE-2026-39831</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39833" target="_blank">CVE-2026-39833</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42508" target="_blank">CVE-2026-42508</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46595" target="_blank">CVE-2026-46595</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39821" target="_blank">CVE-2026-39821</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39829" target="_blank">CVE-2026-39829</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42504" target="_blank">CVE-2026-42504</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39832" target="_blank">CVE-2026-39832</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39834" target="_blank">CVE-2026-39834</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46597" target="_blank">CVE-2026-46597</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25680" target="_blank">CVE-2026-25680</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25681" target="_blank">CVE-2026-25681</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27136" target="_blank">CVE-2026-27136</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39827" target="_blank">CVE-2026-39827</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39828" target="_blank">CVE-2026-39828</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39835" target="_blank">CVE-2026-39835</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42502" target="_blank">CVE-2026-42502</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42506" target="_blank">CVE-2026-42506</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46598" target="_blank">CVE-2026-46598</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42507" target="_blank">CVE-2026-42507</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27145" target="_blank">CVE-2026-27145</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39824" target="_blank">CVE-2026-39824</a>

</Accordion>

</AccordionGroup>

### 1.4.1
#### Changelog
<a id="Changelog-v1.4.1" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>

Addressed the following CVEs, providing increased protection against security 
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33811" target="_blank">CVE-2026-33811</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33814" target="_blank">CVE-2026-33814</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39820" target="_blank">CVE-2026-39820</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39836" target="_blank">CVE-2026-39836</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42499" target="_blank">CVE-2026-42499</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39823" target="_blank">CVE-2026-39823</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39826" target="_blank">CVE-2026-39826</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39825" target="_blank">CVE-2026-39825</a>

</Accordion>

</AccordionGroup>

### 1.4.0
#### Changelog
<a id="Changelog-v1.4.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Add MCP Proxy support'>
This release adds support for MCP (Model Context Protocol) Proxies with a new TykMcpProxyDefinition CRD and an extension to the SecurityPolicy CRD.

**TykMcpProxyDefinition CRD**

A new `TykMcpProxyDefinition` CRD allows you to manage MCP Proxy definitions declaratively in Kubernetes. 

The CRD status surface exposes three hashes — `CRDSpecHash`, `ConfigMapHash`, and `TykSpecHash` — to track synchronisation state between the Kubernetes resource, the ConfigMap, and the Tyk Gateway definition.

**SecurityPolicy CRD — MCP access rights**

The `SecurityPolicy` CRD has been extended to support MCP-specific access rights:
- Per-tool, per-resource, and per-prompt allow/deny lists (`mcp_access_rights`)
- Per-JSON-RPC-method allow/deny lists (`json_rpc_methods_access_rights`)
- Per-primitive rate limits (`mcp_primitives`) and per-method rate limits (`json_rpc_methods`)

For details, see the [MCP proxy policies documentation](/ai-management/mcp-gateway/policies).
</Accordion>

</AccordionGroup>

### 1.2.0
##### Added



<AccordionGroup>
<Accordion title='HMAC request signing support'>
Tyk Operator now supports HMAC request signing, enabling enhanced security and integrity for API requests. This feature aligns with Tyk 5.8 capabilities.

[Learn More](/api-management/upstream-authentication/request-signing)
</Accordion>

<Accordion title='YAML-based OAS API definitions'>
Tyk Operator now allows OAS API definitions in YAML format, increasing flexibility in API configurations.
</Accordion>

<Accordion title='JWT OAS API policy linking'>
Tyk Operator now supports linking of policies for JWT default policies and JWT scope-to-policy mappings using kubernetes names. They can set these fields in TykOASApiDefinition CRD.
</Accordion>
</AccordionGroup>



##### Updated



<Expandable title='Removed dependency on kube-rbac-proxy'>
Tyk Operator removed dependency on kubebuilder's `rbac-proxy` for authentication/authorization of metrics server.
`WithAuthenticationAndAuthorization` feature provided by Controller-Runtime will be used instead.

Users are encouraged to update to Tyk Operator v1.2 to benefit from this change.

For users who cannot immediately update, there is an alternative option: modifying the Operator's Helm chart configuration to replace the image `gcr.io/kubebuilder/kube-rbac-proxy` with another trusted source. For details, please see [Issue 365](https://github.com/TykTechnologies/tyk-charts/issues/365).
</Expandable>



##### Fixed



<AccordionGroup>
<Accordion title='Operator reconciliation error handling'>
Fixed an issue where reconciliation conflicts appeared as errors in logs, which occurred because an outdated copy of the Kubernetes resource was being processed. This has been resolved by fetching the latest copy of the object from the cluster and retrying the operation.
</Accordion>

<Accordion title='Cert-manager dependency'>
Users can now disable cert-manager, making it optional rather than mandatory for onboarding. This enhances flexibility in deployment configurations.
</Accordion>

<Accordion title='Circuit breaker schema validation'>
Fixed an issue where user was getting validation error while setting `threshold_precent` field of classic API Definition CRD starting from Operator v1.0.0, which blocked users from upgrading.
</Accordion>

<Accordion title='Portal API Catalog infinite loop'>
Resolved an issue where the Operator could enter an infinite loop when a PortalAPICatalogue CR was created.
</Accordion>

<Accordion title='Leader election flag'>
Because of some issue in Operator helm chart, configurations options were not getting read correctly.
Helm chart has been fixed and leader election works by default again.
</Accordion>
</AccordionGroup>
