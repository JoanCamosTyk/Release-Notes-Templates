## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 5.3.0
Tyk Charts 5.3.0 completes enterprise support for private container registries by adding `imagePullSecrets` configuration to the tyk-bootstrap chart's bootstrap jobs. The Developer Portal bootstrap job's image is now also configurable and respects the `global.imageRegistry` prefix.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.3.0) below.

### 5.2.0
In this release, we have enhanced Helm chart customization capabilities by adding pod labeling support for the Tyk Operator.
This version aligns with Tyk LTS release 5.8.14.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 5.1.1
Tyk Charts v5.1.1 resolves a blocking installation issue introduced in v5.1.0. This release fixes the Helm chart template error that prevented users from successfully installing the operator via Helm charts.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.1.1) below.

### 5.1.0
Tyk Charts 5.1.0 enhances Kubernetes deployment flexibility with support for priority classes across all components, ensuring API Gateway services maintain scheduling priority during resource pressure. This release also renews the default certificate in Gateway Docker images to prevent March 2026 expiration and completes the migration to Bitnami Legacy registry for continued chart compatibility. Additionally, the release fixes secret reference handling in bootstrap jobs for improved custom secret management.

This version aligns with Tyk LTS release 5.8.12.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.1.0) below.

### 5.0.0
Tyk Charts 5.0.0 introduces a more secure default for client IP detection by setting the XFF depth to 1 for all new deployments. This release also adds the ability to configure Redis max idle connections directly through Helm values. The release aligns with the most recent Tyk LTS release [5.8.9](/developer-support/release-notes/dashboard#5-8-9-release-notes).

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v5.0.0) below.


### 4.1.1
This release addresses an issue from Tyk Charts 4.1.0 where the new Kubernetes tolerations features was not functioning correctly for Tyk Operator deployments. No further changes have been implemented in this release.

### 4.0.1
Tyk Charts 4.0.1 is a version alignment release that ensures compatibility with the most recent Tyk LTS release [5.8.6](/developer-support/release-notes/dashboard#5-8-6-release-notes) and Developer Portal [1.14.1](/developer-support/release-notes/portal#1-14-1-release-notes). No functional changes have been implemented in this release.

### 4.0.0
This release includes improvements to support for Redis Sentinel deployments and updates the default charts to install the most recent Tyk LTS release [5.8.5](/developer-support/release-notes/dashboard#5-8-5-release-notes) and Developer Portal [1.14.0](/developer-support/release-notes/portal#1-14-0-release-notes).

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v4.0.0) below.

## Change Logs

### 5.3.0
#### Changelog
<a id="Changelog-v5.3.0" data-scroll-offset></a>

##### Added

<Accordion title='Add `imagePullSecrets` support for tyk-bootstrap jobs'>
You can now configure `imagePullSecrets` for the tyk-bootstrap chart's bootstrap jobs via the new `bootstrap.imagePullSecrets` value (default: `[]`, opt-in, backward-compatible). Setting this value propagates the secret to all bootstrap hook jobs (pre-install, post-install, and pre-delete) automatically through the job's ServiceAccount. The Developer Portal bootstrap job's image is also now configurable through the new `bootstrapJob.image.repository` and `bootstrapJob.image.tag` values, and respects the `global.imageRegistry` prefix, replacing the previously hardcoded `curlimages/curl` image. Together, these changes let Tyk Charts be deployed fully in air-gapped or private-registry environments.
</Accordion>

### 5.2.0

#### Changelog
Title: Add `podLabels` support for Tyk Operator

Summary: You can now attach custom labels to Tyk Operator pods using the podLabels map. This functionality is already available for Tyk Dashboard, Tyk Gateway, the Developer Portal, and Tyk Pump.

### 5.1.1

#### Changelog
<a id="Changelog-v5.1.1" data-scroll-offset></a>

##### Fixed

<Accordion title='Fixed Tyk Operator Helm chart installation failure'>
Resolved an issue introduced in v5.1.0 where installing the Tyk Operator via Helm failed due to an incorrect webhook annotation reference in the Helm template. Users can now successfully install the Tyk Operator using Helm without encountering template rendering errors.
</Accordion>

### 5.1.0
#### Changelog
<a id="Changelog-v5.1.0" data-scroll-offset></a>

##### Changed
<AccordionGroup>

<Accordion title='Renewed Default Certificate in Gateway Docker Images'>
Updated the default self-signed certificate shipped with Gateway Docker images to prevent expiration in March 2026. This certificate is only intended for initial bootstrapping and testing purposes when `global.tls.gateway=true` is enabled in Helm deployments and must not be used in production environments
</Accordion>

<Accordion title='Completed Migration to Bitnami Legacy Registry'>
Updated all remaining references to use the Bitnami Legacy registry following Bitnami's distribution model changes. This completes the migration from the standard Bitnami registry to the `bitnamilegacy` registry for PostgreSQL and Redis chart dependencies used in Helm deployment examples and testing infrastructure.
</Accordion>

</AccordionGroup>
##### Added

<AccordionGroup>

<Accordion title='Enhanced Pod Scheduling Control with Kubernetes Priority Classes Support'>
Added support for configuring Kubernetes `priorityClassName` across all Tyk components in Helm charts to prevent API Gateway services from being preempted during cluster resource pressure. Users can now assign appropriate priority levels to Tyk Gateway, Dashboard, Pump, Bootstrap jobs, Dev Portal, Operator, and MDCB components through their respective chart values. 

This enhancement ensures that essential Tyk infrastructure maintains a higher scheduling priority than default workloads, improving service availability and preventing unexpected downtime when Kubernetes nodes experience resource constraints.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Secret Reference Handling for Admin Credentials in Portal Bootstrap Job'>
Fixed an issue where the Developer Portal bootstrap job incorrectly handled secret references for admin email and password configuration. The bootstrap job now properly respects custom secret names for both admin email and password, allowing users to successfully deploy with their own secret management solutions.
</Accordion> 

</AccordionGroup>

### 5.0.0
#### Changelog
<a id="Changelog-v5.0.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Default XFF Depth Now Set to Most Secure Value in Tyk Charts'>
We have updated the default configuration in the Tyk Helm Charts to set the X-Forwarded-For (XFF) depth to 1, ensuring that new deployments use the most secure and accurate client IP selection.
</Accordion>

<Accordion title='Redis Max Idle Connections Now Configurable in Tyk Charts'>
You can now configure the maximum number of idle Redis connections (`TYK_GW_STORAGE_MAXIDLE`) directly through the Helm Charts using the new `global.redis.maxIdle` parameter. This eliminates the previous limitation where setting this value would cause deployment errors, requiring manual post-deployment configuration changes.
</Accordion>

</AccordionGroup>

### 4.1.0
#### Changelog
<a id="Changelog-v4.1.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Support for Kubernetes Tolerations in Tyk Helm Charts'>
Added full support for configuring Kubernetes tolerations across all Tyk components (Gateway, Dashboard, MDCB, Pump, etc.) through Helm values.yaml. This enhancement allows teams to deploy Tyk workloads on their controlled node pools for improved compliance and performance.
</Accordion>

<Accordion title='Support for Pod Topology Spread Constraints in Tyk Gateway Helm Chart'>
You can now configure Kubernetes Pod Topology Spread Constraints directly through the Tyk Gateway Helm Chart, allowing you to control how Gateway pods are distributed across availability zones or topology domains.

This enables more resilient, highly available deployments and helps prevent all Gateway replicas from being scheduled in the same zone or node group. Existing users are unaffected, as the feature is disabled by default unless explicitly configured.
</Accordion>

</AccordionGroup>

### 4.0.0
#### Changelog
<a id="Changelog-v4.0.0" data-scroll-offset></a>

##### Added



<Expandable title='Add Redis Sentinel Global Config Handler for Dashboard and MDCB Charts'>
Added Helm Chart support for Redis Sentinel configurations using global values for both the Tyk Dashboard and MDCB charts to match the existing support in Tyk Gateway and Pump charts.
</Expandable>



##### Changed



<Expandable title='Updated default versions of Tyk components'>
Tyk Charts 4.0 will install the following Tyk components:

- Tyk Gateway v5.8.5
- Tyk Dashboard v5.8.5
- Tyk Pump v1.12.0
- Tyk MDCB v2.8.4
- Tyk Developer Portal v1.14.0
- Tyk Operator v1.2.0
</Expandable>


<Expandable title='Tyk Pump Helm Chart: GraphQL Pump disabled by default'>
The Tyk Pump Helm Chart has been updated to disable the GraphQL Pump configuration by default for both MongoDB and PostgreSQL backends. This change provides users with explicit control to enable the pump via Helm values (e.g., `pump.mongoGraph.enabled`), addressing concerns about rapid storage increase. Users currently relying on the GraphQL Pump will need to enable it explicitly after this update.
</Expandable>



##### Fixed



<AccordionGroup>
<Accordion title='Fixed Helm Charts bootstrap failure'>
Resolved an issue where the Helm Charts bootstrap post-install job for tyk-stack failed due to an unnecessary requirement for an operator license, even when the operator was disabled by default. The bootstrap process now completes successfully without requiring the operator license.
</Accordion>

<Accordion title='Incorrect fsGroup Placement in Tyk Helm Chart'>
Corrected the placement of the `fsGroup` field in the `tyk-stack` chart's `values.yaml` from `containerSecurityContext` to `podSecurityContext` to resolve pre-install job failures.
</Accordion>

<Accordion title='Fixed Redis Sentinel Password Configuration in Helm Charts'>
Resolved an issue where the Redis Sentinel password ([TYK_GW_STORAGE_SENTINELPASSWORD](/tyk-oss-gateway/configuration#storage-sentinel_password)) was not correctly picked up by Helm charts. Previously, both Redis and Sentinel passwords referenced the same secret key, leading to the Sentinel password defaulting to the regular Redis password. This fix ensures correct handling of distinct Sentinel passwords.
</Accordion>
</AccordionGroup>



