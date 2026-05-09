## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

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
