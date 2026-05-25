## Intructions
I am going to copy paste all the Highlights and Change Logs and Breaking Changes from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Breaking Changes

### 1.15.0
**Stdout Pump JSON log formatting has changed by default**

The **Stdout Pump** now outputs cleaner JSON logs. Newline (`\n`), tab (`\t`), and carriage return (`\r`) characters are no longer escaped in the `raw_request` and `raw_response` fields.

Users whose log-processing pipelines, dashboards, or alerting rules rely on the previous escaped-character format will need to either update their tooling to handle the new format or set the new [`pumps.stdout.use_legacy_payload_format`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-stdout-use_legacy_payload_format) option to `true` in their Pump configuration.


When `pumps.stdout.use_legacy_payload_format: true`, the Stdout Pump output is identical to previous releases. When unset or set to `false`, the new clean formatting is applied.


## Release Highlights

### 1.15.0
Tyk Pump 1.15.0 introduces **MCP analytics support**, bringing full observability to [MCP Proxy](/ai-management/mcp-gateway/managing-proxies) traffic. MCP analytics records are captured and processed across MongoDB, PostgreSQL, Elasticsearch, and Prometheus backends, with no changes required to existing Pump configurations.

This release also improves **Stdout Pump** output with cleaner, more readable JSON logs by properly formatting escaped characters in request and response fields.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.15.0).

### 1.14.1
In this release, we have updated Tyk Pump to Golang 1.25 and addressed CVEs for enhanced security and performance.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.14.1). 

### 1.14.0
In this release, we've enhanced Tyk Pump's security capabilities by introducing support for custom CA certificates for Kafka, Elasticsearch, and Splunk pumps. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.14.0). 

### 1.13.2
In this patch release, we've resolved SQL Pump schema migration issues for sharded tables. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.13.2).

### 1.13.1
In this patch release, we fixed high-priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.13.1).

### 1.13.0
This release has no breaking changes, but does include the deprecation of two global configuration options ([DecodeRawRequest](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_request_decoded) and [DecodeRawResponse](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_response_decoded)) that did not previously work. There is no change to functionality from these deprecations.


## Change Log

### 1.15.0
#### Changelog
<a id="Changelog-v1.15.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Cleaner JSON formatting for Stdout Pump logs'>
The **Stdout Pump** now outputs cleaner JSON logs, making the logs significantly easier to read and analyze. Newline (`\n`), tab (`\t`), and carriage return (`\r`) characters are no longer escaped in the `raw_request` and `raw_response` fields.

This is a breaking change for any user whose downstream tooling depends on the original format. A new `pumps.stdout.use_legacy_payload_format` configuration option (default `false`) is available if you need to continue to use the previous escaped-character output..
</Accordion>

</AccordionGroup>

##### Added

<AccordionGroup>

<Accordion title='Add MCP analytics support'>
This release adds support for the storing and processing of MCP (Model Context Protocol) Gateway analytics records:

- **Storage backends**: MCP analytics records are supported across MongoDB, PostgreSQL, Elasticsearch, and Prometheus backends. Each record includes MCP-specific fields: `mcp_method`, `mcp_primitive_type`, `mcp_primitive_name`, and `mcp_error_code`.
- **Hybrid Pump aggregation**: A new [`pumps.hybrid.meta.enable_mcp_aggregation`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-hybrid-meta-enable_mcp_aggregation) configuration option on the Hybrid Pump enables pre-aggregation of MCP records before they are forwarded to the Control Plane, reducing data volume for high-traffic distributed deployments.
- **Prometheus labels**: Three new Prometheus labels are emitted for MCP traffic: `mcp_method`, `mcp_primitive_type`, and `mcp_primitive_name`, aligned with the OpenTelemetry `mcp.*` semantic conventions. A new `mcp_only` filter restricts Prometheus output to MCP records exclusively.


For details, see the [MCP analytics documentation](/ai-management/mcp-gateway/mcp-observability).
</Accordion>

</AccordionGroup>

### 1.14.1
<AccordionGroup>

<Accordion title='Updated Go version to 1.25'>
Pump has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25679" target="_blank">CVE-2026-25679</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-32286" target="_blank">CVE-2025-32286</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27139" target="_blank">CVE-2026-27139</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32280" target="_blank">CVE-2026-32280</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32281" target="_blank">CVE-2026-32281</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32288" target="_blank">CVE-2026-32288</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32289" target="_blank">CVE-2026-32289</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32282" target="_blank">CVE-2026-32282</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32283" target="_blank">CVE-2026-32283</a>

</Accordion>

</AccordionGroup>

### 1.14.0
##### Added

<AccordionGroup>

<Accordion title='Enhance Pump Security with Custom CA Certificates'>
You can now establish fully trusted and secure TLS connections from Tyk Pump to Kafka, Elasticsearch, and Splunk services that use certificates signed by specific Certificate Authorities.

To configure this, set the path to the PEM file containing the CA certificate using the `ssl_ca_file` option or the `TYK_PMP_PUMPS_<PUMP_NAME>_META_SSLCAFILE` environment variable.
</Accordion>

</AccordionGroup>


### 1.13.3
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

### 1.13.2
##### Fixed

<AccordionGroup>

<Accordion title='Fixed SQL Pump table sharding schema migration issues'>
Fixed an issue when using table sharding with the SQL pumps (creating new tables for each day), where the Pump would generate error messages in the application logs due to missing columns in the tables. This would only occur when upgrading to a newer version of Pump that added columns (e.g., to Pump 1.11.0, which added some GraphQL-related columns). Now, when Pump is upgraded, the current day's table will automatically be updated to include the new columns, so the error messages will not be generated.

We have added a new configuration option `migrate_sharded_tables`, which will update all pre-existing tables in the SQL database to match the latest schema. This will be a one-time operation when the Pump starts up, but could take some time to complete if there is a large history in the database, so it has been made an optional activity via this configuration option.
</Accordion>

</AccordionGroup>
