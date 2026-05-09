## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 2.10.0
Tyk MDCB has been updated to Go 1.25 for enhanced security and performance. This release also addresses some CVEs and exposes more controls to configure the transfer of aggregated traffic logs to the Control Plane data store.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.10.0).

### 2.9.0
In this release, we've added visibility of the lists of APIs and Policies loaded by each Data Plane Gateway connected to MDCB.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.9.0).

### 2.8.8
In this release, we have fixed some priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.8.8). 

### 2.8.7
In this patch release, we've resolved MongoDB connection leaks, and faster gateway recovery after Redis outages. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.8.7).

### 2.8.6
In this patch release, we fixed high-priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.8.6).


## Change Logs

### 2.10.0
#### Changelog
<a id="Changelog-v2.10.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Updated Go version to 1.25'>
The Tyk MDCB has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

</AccordionGroup>

##### Added

<AccordionGroup>

<Accordion title='Extended existing MongoDB option to avoid database contention when using PostgreSQL data store in the Control Plane'>
The [`omit_analytics_index_creation`](/tyk-multi-data-centre/mdcb-configuration-options#omit_analytics_index_creation) configuration option is used to prevent deadlocks in horizontally scaled deployments that could occur if multiple MDCB instances created the same index concurrently.
When this option is enabled, MDCB does not [automatically create indexes](/api-management/tyk-pump#omitting-indexes), eliminating database contention issues, while maintaining full analytics functionality.
This control was previously only applied when targeting a MongoDB data store in the Control Plane. Now it can also be used with a PostgreSQL data store.
</Accordion>

<Accordion title='Added options to allow better control of high cardinality data when using MongoDB store in the Control Plane'>
We have added additional configuration options when using a MongoDB data store in the Control Plane. These allow better control of high cardinality data, such as per-key analytics, which helps prevent performance degradation and reduces database storage costs.

- `ignore_aggregations` accepts a list of aggregations that should not be passed to the Control Plane
- `enable_aggregate_self_healing` automatically creates a new document in MongoDB when the maximum document size is reached.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33186" target="_blank">CVE-2026-33186</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25679" target="_blank">CVE-2026-25679</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32285" target="_blank">CVE-2026-32285</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32286" target="_blank">CVE-2026-32286</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32280" target="_blank">CVE-2026-32280</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39883" target="_blank">CVE-2026-39883</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32282" target="_blank">CVE-2026-32282</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32283" target="_blank">CVE-2026-32283</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27139" target="_blank">CVE-2026-27139</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27142" target="_blank">CVE-2026-27142</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32281" target="_blank">CVE-2026-32281</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32288" target="_blank">CVE-2026-32288</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32289" target="_blank">CVE-2026-32289</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39882" target="_blank">CVE-2026-39882</a>





</Accordion>

</AccordionGroup>


### 2.9.0
#### Changelog
<a id="Changelog-v2.9.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Visibility of the APIs and Policies loaded by each Data Plane Gateway'>
The `GET /dataplanes` endpoint has been extended to provide enhanced visibility of your Tyk deployment. It now returns a list of the APIs and policies loaded on each connected Data Plane Gateway. You can retrieve the results for a specific Gateway using the `node_id` query parameter.

This update provides a clear picture of what is running on each Gateway, simplifying the monitoring and troubleshooting of your Tyk deployment.
</Accordion>

</AccordionGroup>

### 2.8.8
#### Changelog
<a id="Changelog-v2.8.8" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed CVEs reported in dependent libraries, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61726" target="_blank">CVE-2025-61726</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-61728" target="_blank">CVE-2025-61728</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-68121" target="_blank">CVE-2025-68121</a>

</Accordion>

</AccordionGroup>

### 2.8.7
#### Changelog
<a id="Changelog-v2.8.7" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed MDCB MongoDB Connection Leak During Error-Triggered Reconnects'>
Fixed an issue where MDCB could perform unnecessary re-connections in response to server errors reported by MongoDB, failing to disconnect the client, leaving the old client’s connection pool orphaned and connected to the server forever until the application is terminated (manifesting as a connection leak). We have fixed the connection leak, preventing unnecessarily high connection counts and the associated costs.
</Accordion>

<Accordion title='MDCB Gateway Recovery After Control Plane Redis Outage'>
Fixed an issue where Data Plane Gateways remained disconnected for 3-5 minutes after Control Plane Redis recovered from an outage. Previously, when Redis went down and came back up, gateways couldn't receive API updates, policy changes, or other configuration changes until a fixed cache timeout expired.
With this fix, gateways now reconnect and sync updated assets (APIs, policies) within 30 seconds of Redis recovery instead of waiting several minutes.
</Accordion>

</AccordionGroup>

### 2.8.6
#### Changelog
<a id="Changelog-v2.8.6" data-scroll-offset></a>

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

</Accordion>

</AccordionGroup>

### 2.8.5
#### Changelog
<a id="Changelog-v2.8.5" data-scroll-offset></a>

##### Changed



<Expandable title='Upgrade Tyk MDCB to Golang 1.24'>
Tyk MDCB has been upgraded to [Golang 1.24](https://tip.golang.org/doc/go1.24), improving security by staying current with the latest Go versions.
</Expandable>



##### Fixed



<Expandable title='Enhanced Redis Connection Resilience with Intelligent Retry Logic'>
We've resolved a synchronization issue where MDCB would permanently stop syncing with Gateways after Redis connection failures. The system now implements robust exponential backoff retry logic that continues indefinitely until a successful reconnection is achieved, ensuring your API infrastructure maintains continuous operation during Redis outages, network disruptions, or server restarts. 

Previously, MDCB would attempt only a single reconnection before silently abandoning the sync process while appearing healthy, leaving Gateways without updates. 

With this enhancement, both pub/sub and keyspace listeners automatically recover from transient Redis issues, provide clear logging of retry attempts for improved observability, and eliminate the need for manual MDCB restarts to restore synchronization.
</Expandable>


