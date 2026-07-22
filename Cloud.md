## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 1.32.0
This release completes the Tyk Cloud observability triangle by adding gateway metrics export, joining the distributed traces and application logs already supported through the OpenTelemetry pipeline. Customers with the Telemetry entitlement can now stream RED (Rate, Error, Duration) metrics from their Cloud Data Plane gateways into their existing observability backend and correlate them with the traces and logs they already receive.
This release also fixes a bug where Hybrid Data Plane Groups could incorrectly appear as NOT CONNECTED on the Cloud Deployments list page even when their gateways were running.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 1.31.1
This release includes infrastructure improvements and security enhancements to ensure continued platform stability and compliance.
No functional changes have been implemented in this release. 

### 1.31.0
This release includes infrastructure improvements and security enhancements to ensure continued platform stability and compliance.
No functional changes have been implemented in this release. 

### 1.30.2
This release upgrades Tyk Cloud to Golang 1.25 for enhanced security and transitions to Enterprise Edition Gateway images by default, ensuring full feature support and consistency between Dashboard and Gateway components. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.30.3). 

### 1.30.3
This release improves the database connection management in customer Control Plane deployments. This change intends to make database-related operations more stable and capacity management more predictable. No customer action required.

## Change Log

### 1.32.0
#### Changelog 
<a id="Changelog-v1.32.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Export Cloud Gateway metrics to observability providers'>
Cloud customers with the Telemetry entitlement can now export gateway metrics to their observability backend through the same OpenTelemetry pipeline already used for distributed traces and application logs. A new **Enable Metrics** toggle has been added to the Telemetry Export section of each Cloud Data Plane deployment, to the provider configured at the organisation level (New Relic, Elastic, Dynatrace, Datadog, or any OpenTelemetry-native backend via the Custom option). Gateway metrics export requires the Cloud Data Plane to run Gateway v5.13.0 or later. See [Configure Telemetry in Tyk Cloud](https://tyk.io/docs/tyk-cloud/telemetry) for setup instructions.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed Hybrid Data Plane connection status on the Deployments list'>
Resolved an issue where a Hybrid Data Plane Group could appear as **NOT CONNECTED** with "0 connected" Gateways on the Cloud Deployments list page, even when the Gateway was registered and running. Opening the same Data Plane Group showed the correct **CONNECTED** status, Gateway count, and version in the detail view. The list and detail views now report the same connection state, so the Deployments page reflects reality on page load and refresh.
</Accordion>

</AccordionGroup>

### 1.30.3
#### Changelog 
<a id="Changelog-v1.30.3" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Upgraded Tyk Cloud to Golang 1.25'>
Tyk Cloud has been updated to [Golang 1.25](https://tip.golang.org/doc/go1.25), improving security by staying current with the latest Go versions.
</Accordion>

<Accordion title='Upgraded Tyk Cloud to use Enterprise Edition (EE) Gateway images'>
Tyk Cloud deployments now use Enterprise Edition (EE) Gateway images by default, replacing the previously used OSS images. This ensures that all EE features are fully supported and consistent between the Dashboard and the Gateway. This change is completely transparent and requires no action from users, and existing deployments maintain full compatibility.
</Accordion>

</AccordionGroup>

### 1.30.0
##### Added

<Expandable title='Export Application Logs to Observability Providers'>
Cloud users can now export Tyk application logs to observability providers using OpenTelemetry (such as Datadog, New Relic, Elastic, and Dynatrace). This feature can be enabled or disabled per deployment, and logs are streamed in real time to the chosen provider, enabling better monitoring and faster troubleshooting.
</Expandable>

<Expandable title='Email Notifications for Auto-Upgrades'>
Introduced automated email notifications to inform organisations and team admins when a Control Plane [auto-upgrade](/tyk-cloud/environments-deployments/managing-control-planes#auto-upgrade) begins. Notifications include key details such as deployment name and version changes, helping teams track upgrade activity more effectively.
</Expandable>