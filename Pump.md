## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

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
