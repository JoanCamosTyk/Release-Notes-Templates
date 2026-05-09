## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

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