# Tyk Pump Release Notes

Release notes for Tyk Pump, grouped by section (Breaking Changes, Release Highlights, Dependencies, Deprecations, Upgrade Instructions, Downloads, Change Log). Within each section, versions are listed newest first.

Use this file as the reference for how much detail we give and how we phrase things for this component. Follow the rules in `CLAUDE.md` when drafting anything new.

Published source: [https://tyk.io/docs/developer-support/release-notes/pump](https://tyk.io/docs/developer-support/release-notes/pump). Every version on the published page (1.8.0 to 1.18.0) was synced word for word on 2026-11-26 (only heading levels changed to fit this file).

Not on the published page (Claude drafts, may still change or may never have been published): 1.15.1. Anything marked **[TO CONFIRM]** in them still needs checking.

## Breaking Changes

### 1.16.0
**Changes to Tyk Pump application logs**

This release makes [changes](#Changelog-v1.16.0) to the format of Tyk Pump application logs so that output is consistent with other Tyk components and aligned with industry observability standards.

<Note>
This is only a breaking change if you specifically rely on the current Tyk Pump log format: for example, if your monitoring, parsing, or alerting tools are built around the existing timestamp format or the `msg` log field.
</Note>

**Mitigation:** A new `legacy` option has been added to the [`log_format`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#log_format) configuration. This setting preserves the format used prior to Pump 1.16.0, with the historical timestamp format and the `msg` field name.

### 1.15.1
**Changes to Tyk Pump application logs**

**Note: This change does not affect you unless you specifically rely on the current Tyk Pump log format**: for example, if your monitoring, parsing, or alerting tools are built around the existing timestamp format or the `msg` log field.

This release makes several changes to the format of Tyk Pump application logs so that output is consistent with other Tyk components and aligned with industry observability standards. All of the changes below are controlled by a single setting, and every one of them can be reverted by selecting the `legacy` log format.

The `log_format` configuration option (and the `TYK_PMP_LOGFORMAT` environment variable) now accepts three explicit values:

- `text` (**new default**): plain-text logs with the new, consistent formatting described below.
- `json`: JSON logs with the same consistent formatting.
- `legacy`: preserves the previous behaviour exactly, including the historical timestamp format and the `msg` field name.

**What changes under the new `text` and `json` formats**

- **Consistent RFC3339 timestamps.** All log entries now use the industry-standard RFC3339 timestamp format (e.g. `2024-12-12T13:59:08Z`), replacing the previous default `logrus` timestamp format.
- **Log message field renamed.** The log message field is renamed from `msg` to `message`, for consistency with other Tyk components.

**Migration**

- To keep the previous behaviour exactly, the historical timestamps and the `msg` field name, set `log_format` to `legacy` (or `TYK_PMP_LOGFORMAT=legacy`). The `legacy` option is the supported fallback for customers whose observability tooling depends on the previous Tyk Pump log output.

### 1.15.0
**Stdout Pump JSON log formatting has changed by default**

The **Stdout Pump** now outputs cleaner JSON logs. Newline (`\n`), tab (`\t`), and carriage return (`\r`) characters are no longer escaped in the `raw_request` and `raw_response` fields.

Users whose log-processing pipelines, dashboards, or alerting rules rely on the previous escaped-character format will need to either update their tooling to handle the new format or set the new [`pumps.stdout.meta.use_legacy_payload_format`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-stdout-meta-use_legacy_payload_format) option to `true` in their Pump configuration.


When `pumps.stdout.use_legacy_payload_format: true`, the Stdout Pump output is identical to previous releases. When unset or set to `false`, the new clean formatting is applied.

### 1.14.1
This release has no breaking changes, however a misconfiguration that previously caused a silent failure is now surfaced and causes the Pump to crash on startup, see [known issues](#known-issues).

#### Known Issues

**Pump can crash on startup when using environment variables without setting `TYK_PMP_OMITCONFIGFILE=true`**

Tyk Pump can be configured using either a configuration file or environment variables.

When using environment variables it is important to set [`TYK_PMP_OMITCONFIGFILE=true`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#omit_config_file). If this is not set, Pump will attempt to load the default configuration file (`tyk_pump.conf`).

Prior to v1.14.1 if this file was missing, there was a silent failure from which Pump automatically recovered. With the update to Go 1.25, however, changes in the underlying libraries mean that the missing file will cause Pump to fail to start.

Failing to set `TYK_PMP_OMITCONFIGFILE=true` when using environment variables is a misconfiguration and must be corrected.

### 1.13.2
No breaking changes. However, please note that first startup may be slower due to database schema migration if the new `migrate_sharded_tables` configuration is set. See changelog for details.

### 1.13.0
This release has no breaking changes, but does include the deprecation of two global configuration options ([DecodeRawRequest](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_request_decoded) and [DecodeRawResponse](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_response_decoded)) that did not previously work. There is no change to functionality from these deprecations.

### 1.12.0
This release removes support for SQLite in Pump, aligning with the broader removal of SQLite from the Tyk Dashboard. This change improves compatibility with enterprise environments and addresses implementation issues related to SQLite.

## Release Highlights

### 1.18.0
**Release date:** 27 October 2026

This release adds controls for the PostgreSQL connection pools that Tyk Pump uses, and an option to include analytics record tags in the Syslog Pump output.

**PostgreSQL Connection Pool Controls**

Four new options let you limit the size and lifetime of each PostgreSQL connection pool, so Tyk Pump can release idle connections instead of holding them indefinitely. This helps in blue/green deployments, where an idle Pump can otherwise hold connections that the active Pump needs.

**Tags in Syslog Pump Output**

The new `include_tags` option adds the `tags` field of each analytics record to the Syslog Pump output. The option is disabled by default.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.18.0).

### 1.17.0
**Release date:** 1 September 2026

This release adds [Secrets Management](/tyk-configuration-reference/kv-store) to the Tyk Pump configuration file for the first time, letting you reference values from external secrets stores in place of plain-text values.

Tyk Pump now supports [IAM-based authentication](/tyk-configuration-reference/redis-cluster-sentinel#iam-authentication) for Google Cloud Memorystore for Redis Cluster and Valkey, as an alternative to a static password.

We have also fixed two issues affecting the [Kinesis pump](/api-management/logs/external-data-sinks#kinesis) and deprecated the legacy `PMP_MONGO*` environment variable format in lieu of the standard `TYK_PMP_*` format (see [Deprecations](#deprecations) for the full list).

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.17.0).

### 1.16.0
**Release date:** 7 July 2026

In this patch we fix inconsistencies in the application log format, introducing a `legacy` option for any users whose observability tooling relies on the specific format of those logs. We have also fixed a bug when using the Hybrid Pump and PostgreSQL.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.16.0).

### 1.15.0
Tyk Pump 1.15.0 introduces **MCP analytics support**, bringing full observability to [MCP Proxy](/ai-management/mcp-gateway/managing-proxies) traffic. MCP analytics records are captured and processed across MongoDB, PostgreSQL, Elasticsearch, and Prometheus backends, with no changes required to existing Pump configurations.

This release also improves **Stdout Pump** output with cleaner, more readable JSON logs by properly formatting escaped characters in request and response fields.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.15.0).

### 1.14.1
In this release, we have updated Tyk Pump to Golang 1.25 and addressed CVEs for enhanced security and performance.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.14.1). 

### 1.14.0
In this release, we've enhanced Tyk Pump's security capabilities by introducing support for custom CA certificates for Kafka, Elasticsearch, and Splunk pumps. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.14.0). 

### 1.13.3
**Release date:** 12 February 2026

In this release, we have fixed some priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.13.3). 

### 1.13.2
In this patch release, we've resolved SQL Pump schema migration issues for sharded tables. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.13.2).

### 1.13.1
In this patch release, we fixed high-priority CVEs. For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.13.1).

### 1.13.0
**Release date:** 29 October 2025

### 1.12.2
**Release date:** 15th October 2025

This release strengthens Tyk Pump's security foundation with updates, including an upgrade to Golang 1.24 for enhanced runtime security and the resolution of two CVEs (CVE-2025-22871 and CVE-2025-22869), ensuring robust protection against identified security vulnerabilities.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.12.2) below.

### 1.12.1
**Release date:** 18 August 2025

This release improves log reliability in the Syslog Pump by ensuring that raw request and response data are no longer fragmented into multiple entries. Logs are now consolidated into a single entry per API request, making analysis easier while preserving compatibility with existing log parsers.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.12.1) below.

### 1.12.0
**Release date:** 28 March 2025

This release enhances Pump's database compatibility and security by adding support for PostgreSQL 17, removing SQLite support, and addressing critical security vulnerabilities (CVEs). These improvements ensure better performance, long-term stability, and overall enhanced security.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.12.0) below.

### 1.11.2
**Release date:** 10 January 2025

This patch release focuses on improving the robustness of SQL-Pump in scenarios involving table sharding and analytics data, ensuring smoother operations and better compatibility with PostgreSQL-based environments.

### 1.11.1
**Release date:** 04 December 2024

This patch release focuses on critical dependency updates to address security vulnerabilities and maintain compatibility with the latest tools. Users are encouraged to upgrade to benefit from enhanced security and improved stability.

### 1.11.0
**Release date:** 13 August 2024

#### Security fixes
This release focuses on improving security and compliance, enhancing integration capabilities, and ensuring robust performance in secure environments.

### 1.8.1
<Note>
#### Notes on MongoDB v5 and v6 compatibility

For MongoDB v5 and v6 users, please [set mongo driver type](https://github.com/TykTechnologies/tyk-pump#driver-type) to `mongo-go`.

From pump v1.8.1, the default MongoDB driver it uses is [mgo](https://github.com/go-mgo/mgo). This is to align with the default MongoDB driver of other Tyk components. This driver supports MongoDB versions up to v4. If you are using a later version of MongoDB v5 or MongoDB v6, please [follow this guide to update the driver type](https://github.com/TykTechnologies/tyk-pump#driver-type) to [mongo-go](https://github.com/mongodb/mongo-go-driver).
</Note>

### 1.8.0
**Release date:** 2023-05-04

#### Major features
Pump 1.8 introduces two new pumps: The GraphQL SQL Aggregate Pump - which allows you to transfer GraphQL transaction logs to SQL; and Resurface Pump - which allows you to transfer data to [Resurface.io](http://resurface.io/) for context based security analysis. 

We have changed the default MongoDB driver from [mgo](https://github.com/go-mgo/mgo) to [mongo-go](https://github.com/mongodb/mongo-go-driver). The new driver supports MongoDB versions greater or equal to v4. If you are using older version of MongoDB v3.x, please [follow this guide to update the driver type](https://github.com/TykTechnologies/tyk-pump#driver-type).

We have also added a config option that allow you to decode the raw requests and responses for all pumps so you don't need to worry about processing them in your data pipeline. For demo mode, there is now an option to generate future data for your convenience.

In this release, we are using a new Tyk storage library to connect to Mongo DB. This would allow us to switch to use the official Mongo Driver very easily in the future.


<Note>
#### Notes on MongoDB v3.x compatibility

In 1.8.0, the default MongoDB driver it use is [mongo-go](https://github.com/mongodb/mongo-go-driver). This driver supports MongoDB versions greater or equal to v4. If you are using older version of MongoDB v3.x, please [follow this guide to update the driver type](https://github.com/TykTechnologies/tyk-pump#driver-type).
</Note>

## Dependencies

### 1.18.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions            | Compatible Versions        |
| :-------------------------------------------------------- | :------------------------- | :------------------------- | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 7.0.x, 8.0.x, 9.0.x        | 7.0.x, 8.0.x, 9.0.x        |
| [PostgreSQL](https://www.postgresql.org/download/)        | 15.x, 16.x, 17.x, 18.x     | 15.x, 16.x, 17.x, 18.x     | 
| [Redis](https://redis.io/download/)                       | 7.x, 8.x (up to 8.4)       | 7.x, 8.x (up to 8.4)       | 
| [Valkey](https://valkey.io/download/)                     | 7.2.x, 8.0.x, 8.1.x, 9.1.x | 7.2.x, 8.0.x, 8.1.x, 9.1.x |                            

Given the time difference between your upgrade and the release of this version, we recommend that customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.17.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :-------------------------------------------------------- | :---------------- | :----------------------- | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.4) | 6.x - 7.x, 8.x (up to 8.4) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.16.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :-------------------------------------------------------- | :---------------- | :----------------------- | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.4) | 6.x - 7.x, 8.x (up to 8.4) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.15.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.4) | 6.x - 7.x, 8.x (up to 8.4) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.14.1
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.4) | 6.x - 7.x, 8.x (up to 8.4) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.14.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.4) | 6.x - 7.x, 8.x (up to 8.4) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.13.3
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.13.2
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.13.1
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.13.0
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      |
| :--------------------------------------------------------- | :----------------- | :------------------------ | 
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x       | 13.x - 17.x              | 
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | 
| [Valkey](https://valkey.io/download/)                     | 8.0.x, 8.1.x      | 7.2.x, 8.0.x, 8.1.x      |                            

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.12.2
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Pump and Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x    | 13.x - 17.x              | Used by Tyk Pump and Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.12.1
#### 3rd Party Dependencies & Tools


| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Pump and Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x    | 13.x - 17.x              | Used by Tyk Pump and Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0, 8.x (up to 8.2) | 6.x - 7.x, 8.x (up to 8.2) | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.12.0
#### 3rd Party Dependencies & Tools

With PostgreSQL v11 reaching [EOL](https://www.postgresql.org/support/versioning/) in November 2023, we can no longer guarantee full compatibility with this version of the database. If you are [using PostgreSQL](/tyk-configuration-reference/sql) we recommend that you upgrade to a version that we have tested with, as indicated below.

| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Pump and Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 13.x - 17.x    | 13.x - 17.x              | Used by Tyk Pump and Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0         | 6.x - 7.x                | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.11.2
#### 3rd Party Dependencies & Tools

With PostgreSQL v11 reaching [EOL](https://www.postgresql.org/support/versioning/) in November 2023, we can no longer guarantee full compatibility with this version of the database. If you are [using PostgreSQL](/tyk-configuration-reference/sql) we recommend that you upgrade to a version that we have tested with, as indicated below.

| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Pump and Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 12.x - 16.x    | 12.x - 16.x              | Used by Tyk Pump and Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0         | 6.x - 7.x                | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.11.1
#### 3rd Party Dependencies & Tools

With PostgreSQL v11 reaching [EOL](https://www.postgresql.org/support/versioning/) in November 2023, we can no longer guarantee full compatibility with this version of the database. If you are [using PostgreSQL](/tyk-configuration-reference/sql) we recommend that you upgrade to a version that we have tested with, as indicated below.

| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 12.x - 16.x    | 12.x - 16.x              | Used by Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0         | 6.x - 7.x                | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.11.0
#### 3rd Party Dependencies & Tools

With PostgreSQL v11 reaching [EOL](https://www.postgresql.org/support/versioning/) in November 2023, we can no longer guarantee full compatibility with this version of the database. If you are [using PostgreSQL](/tyk-configuration-reference/sql) we recommend that you upgrade to a version that we have tested with, as indicated below.

| Third Party Dependency                                    | Tested Versions   | Compatible Versions      | Comments                   |
| :--------------------------------------------------------- | :----------------- | :------------------------ | :-------------------------- |
| [GoLang](https://go.dev/dl/)                              | 1.19, 1.20, 1.21  | 1.19, 1.20, 1.21         | All our binaries           |
| [MongoDB](https://www.mongodb.com/try/download/community) | 5.x, 6.x, and 7.0 | 4.4.x, 5.x, 6.x, and 7.0 | Used by Tyk Dashboard      |
| [PostgreSQL](https://www.postgresql.org/download/)        | 12.x - 16.x    | 12.x - 16.x              | Used by Tyk Dashboard      |
| [Redis](https://redis.io/download/)                       | 6.x - 7.0         | 6.x - 7.x                | Used by all Tyk components |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

## Deprecations

### 1.17.0
Legacy `PMP_MONGO_*` environment variables are now deprecated and do not support Secrets Management references; migrate to the `TYK_PMP_*` equivalents below.

| Legacy (deprecated) | Use for [standard Mongo pump](/api-management/dashboard-analytics/control-plane-pumps#standard-mongo-pump) | Use for [Mongo uptime pump](/planning-for-production/ensure-high-availability/uptime-tests#mongodb) |
|---|---|---|
| `PMP_MONGO_MONGOURL` | `TYK_PMP_PUMPS_MONGO_META_MONGOURL` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOURL` |
| `PMP_MONGO_MONGOUSESSL` | `TYK_PMP_PUMPS_MONGO_META_MONGOUSESSL` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOUSESSL` |
| `PMP_MONGO_MONGOSSLINSECURESKIPVERIFY` | `TYK_PMP_PUMPS_MONGO_META_MONGOSSLINSECURESKIPVERIFY` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOSSLINSECURESKIPVERIFY` |
| `PMP_MONGO_MONGOSSLALLOWINVALIDHOSTNAMES` | `TYK_PMP_PUMPS_MONGO_META_MONGOSSLALLOWINVALIDHOSTNAMES` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOSSLALLOWINVALIDHOSTNAMES` |
| `PMP_MONGO_MONGOSSLCAFILE` | `TYK_PMP_PUMPS_MONGO_META_MONGOSSLCAFILE` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOSSLCAFILE` |
| `PMP_MONGO_MONGOSSLPEMKEYFILE` | `TYK_PMP_PUMPS_MONGO_META_MONGOSSLPEMKEYFILE` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOSSLPEMKEYFILE` |
| `PMP_MONGO_OMITINDEXCREATION` | `TYK_PMP_PUMPS_MONGO_META_OMITINDEXCREATION` | `TYK_PMP_UPTIMEPUMPCONFIG_OMITINDEXCREATION` |
| `PMP_MONGO_MONGOSESSIONCONSISTENCY` | `TYK_PMP_PUMPS_MONGO_META_MONGOSESSIONCONSISTENCY` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGOSESSIONCONSISTENCY` |
| `PMP_MONGO_MONGODRIVERTYPE` | `TYK_PMP_PUMPS_MONGO_META_MONGODRIVERTYPE` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGODRIVERTYPE` |
| `PMP_MONGO_MONGODIRECTCONNECTION` | `TYK_PMP_PUMPS_MONGO_META_MONGODIRECTCONNECTION` | `TYK_PMP_UPTIMEPUMPCONFIG_MONGODIRECTCONNECTION` |
| `PMP_MONGO_COLLECTIONNAME` | `TYK_PMP_PUMPS_MONGO_META_COLLECTIONNAME` | `TYK_PMP_UPTIMEPUMPCONFIG_COLLECTIONNAME` |
| `PMP_MONGO_MAXINSERTBATCHSIZEBYTES` | `TYK_PMP_PUMPS_MONGO_META_MAXINSERTBATCHSIZEBYTES` | `TYK_PMP_UPTIMEPUMPCONFIG_MAXINSERTBATCHSIZEBYTES` |
| `PMP_MONGO_MAXDOCUMENTSIZEBYTES` | `TYK_PMP_PUMPS_MONGO_META_MAXDOCUMENTSIZEBYTES` | `TYK_PMP_UPTIMEPUMPCONFIG_MAXDOCUMENTSIZEBYTES` |
| `PMP_MONGO_COLLECTIONCAPMAXSIZEBYTES` | `TYK_PMP_PUMPS_MONGO_META_COLLECTIONCAPMAXSIZEBYTES` | `TYK_PMP_UPTIMEPUMPCONFIG_COLLECTIONCAPMAXSIZEBYTES` |
| `PMP_MONGO_COLLECTIONCAPENABLE` | `TYK_PMP_PUMPS_MONGO_META_COLLECTIONCAPENABLE` | `TYK_PMP_UPTIMEPUMPCONFIG_COLLECTIONCAPENABLE` |

**Note:** This deprecation also applies to the Mongo Aggregate and Mongo Selective pumps. 
- For the **Mongo Aggregate Pump**, migrate `PMP_MONGOAGG_*` to `TYK_PMP_PUMPS_MONGOAGGREGATE_META_*`
- For the **Mongo Selective Pump**, migrate `PMP_MONGOSEL_*` to `TYK_PMP_PUMPS_MONGOSELECTIVE_META_*`

### 1.13.0
Deprecated the global configuration options [DecodeRawRequest](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_request_decoded) and [DecodeRawResponse](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#raw_response_decoded).
For further information, please check the [changelog](#Changelog-v1.13.0) below.

## Upgrade Instructions

### 1.18.0
For users currently on v1.17.0, we strongly recommend promptly upgrading to the latest release.

Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade instructions.

### 1.17.0
For users currently on v1.16.0, we strongly recommend promptly upgrading to the latest release.

Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.16.0
For users currently on v1.15.0, we strongly recommend promptly upgrading to the latest release.

Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.15.0
For users currently on v1.14.1, we strongly recommend promptly upgrading to the latest release.

In distributed deployments, MDCB 2.11.0 must be deployed before upgrading Pump to 1.15.0. Keep `enable_mcp_aggregation` set to `false` during the upgrade — only set it to `true` once MDCB, Pump, Gateway, and Dashboard are all on their new versions. Setting it before MDCB is upgraded will cause RPC failures and silent loss of MCP analytics data. See [MCP Gateway upgrade considerations](/developer-support/upgrading#mcp-upgrade-considerations) for the full enablement sequence.

Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.14.1
For users currently on v1.14.0, we strongly recommend promptly upgrading to the latest release. 
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.14.0
For users currently on v1.13.3, we strongly recommend promptly upgrading to the latest release. 
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.13.3
For users currently on v1.13.2, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.13.2 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.13.2
For users currently on v1.13.1, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.13.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.13.1
For users currently on v1.13.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.13.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.13.0
For users currently on v1.12.2, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.12.2 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.12.2
For users currently on v1.12.1, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.12.1 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.12.1
For users currently on v1.12.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.12.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.12.0
For users currently on v1.11.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.11.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.11.2
For users currently on v1.11.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.11.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.11.1
For users currently on v1.11.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.11.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.11.0
For users currently on v1.10.X, we strongly recommend promptly upgrading to the latest release. If you are working with an older version, it is advisable to bypass version 1.10 and proceed directly to this latest release.

## Downloads

### 1.18.0
- [Docker Image v1.18.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.18.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.18.0@sha256:bf263c8ba195a09bec6e039bf89994ad9607f0d608fe534699c8fc125ac896ed
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.18.0)

### 1.17.0
- [Docker Image v1.17.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.17.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.17.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.17.0)

### 1.16.0
- [Docker Image v1.16.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.16.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.16.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.16.0)

### 1.15.0
- [Docker Image v1.15.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.15.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.15.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.15.0)

### 1.14.1
- [Docker Image v1.14.1](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.14.1)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.14.1
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.14.1)

### 1.14.0
- [Docker Image v1.14.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.14.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.14.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.14.0)

### 1.13.3
- [Docker Image v1.13.3](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.13.3)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.13.3
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.13.3)

### 1.13.2
- [Docker Image v1.13.2](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.13.2)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.13.2
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.13.2)

### 1.13.1
- [Docker Image v1.13.1](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.13.1)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.13.1
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.13.1)

### 1.13.0
- [Docker Image v1.13.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.13.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.13.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.13.0)

### 1.12.2
- [Docker Image v1.12.2](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.12.2)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.12.2
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.12.2)

### 1.12.1
- [Docker Image v1.12.1](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.12.1)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.12.1
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.12.1)

### 1.12.0
- [Docker Image v1.12.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.12.0)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.12.0
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.12.0)

### 1.11.2
- [Docker Image v1.11.2](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.11.2)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.11.2
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.11.2)

### 1.11.1
- [Docker Image v1.11.1](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.11.1)
  - ```bash
    docker pull tykio/tyk-pump-docker-pub:v1.11.1
    ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.11.1)

### 1.11.0
- [Docker Image v1.11.0](https://hub.docker.com/r/tykio/tyk-pump-docker-pub/tags?page=&page_size=&ordering=&name=v1.11)
- ```bash
  docker pull tykio/tyk-pump-docker-pub:v1.11.0
  ```
- Source code tarball for OSS - [GH Tyk Pump Repo](https://github.com/TykTechnologies/tyk-pump/releases/tag/v1.11.0)

## Change Log

### 1.18.0
#### Changelog
<a id="Changelog-v1.18.0" data-scroll-offset></a>


##### Added

<AccordionGroup>

<Accordion title='Added PostgreSQL connection pool controls'>
{/* TT-17916 */}
Tyk Pump now exposes configuration options to manage its pools of PostgreSQL database connections.

The following options are available in the `postgres` section of each SQL Pump's `meta` configuration (for example, `pumps.sql.meta.postgres.max_open_connections`) and of `uptime_pump_config`:

- `max_open_connections`: sets the maximum number of open connections in the pool (default: unlimited)
- `max_idle_connections`: sets the maximum number of idle connections kept in the pool (default: 2)
- `connection_max_lifetime`: sets how long a connection can be reused before being closed (default: unlimited)
- `connection_max_idle_time`: sets how long a connection can sit idle before being closed (default: unlimited)

If a value is negative or is not a valid duration, the affected pump type fails to initialize and Pump logs an error.

<Note>
Tyk Pump maintains a separate connection pool for each configured SQL Pump and another for the Uptime Pump. The limits apply to each pool. 
</Note>

For more details see [SQL Connection Pool Management](/tyk-configuration-reference/sql#connection-pool-management).
</Accordion>

<Accordion title='Added the option to include tags in Syslog Pump output'>
{/* TT-17479 */}
The Syslog Pump can now include the `tags` field of each analytics record, in the same way as the Elasticsearch, Kafka, Kinesis and other pumps. Enable it with the new `include_tags` option in the Syslog Pump's `meta` configuration, or with the `TYK_PMP_PUMPS_SYSLOG_META_INCLUDETAGS` environment variable.

The option is disabled by default, so existing Syslog output is unchanged unless you turn it on. Tags are escaped so each record stays on a single line.

Records with many tags can exceed the size limit of the default UDP transport, which can cause some syslog servers to truncate them. Pump logs a warning when `include_tags` is enabled with UDP. If you use many tags, we recommend the `tcp` transport.
</Accordion>

<Accordion title='Added MCP protocol version fields to MCP analytics'>
{/* TT-18010 */}
Tyk Pump now stores the MCP protocol version information that [Tyk Gateway 5.16.0](/developer-support/release-notes/gateway#5-16-0-release-notes) records for each MCP request, so you can track adoption of the MCP 2026-07-28 protocol across your MCP traffic.

Three new fields are added to MCP analytics records:

- `effective_protocol_version`: the protocol version Tyk used to handle the request.
- `declared_protocol_version`: the protocol version the client declared.
- `protocol_version_source`: how the effective version was determined (`header_body`, `header`, `body`, `session` or `legacy_fallback`).

The fields are written by the MCP MongoDB and MCP SQL pumps, and by the Elasticsearch pump when an MCP index is configured. With SQL, the new columns are added to the MCP table automatically. Records from older Gateways leave these fields empty.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed lost aggregated analytics from PostgreSQL deadlocks with multiple Pump instances'>
{/* TT-9424 */}
Running two or more Pump instances that write analytics to PostgreSQL could cause database deadlocks on the aggregated analytics tables. Each deadlock caused that cycle's aggregated analytics to be lost, with a `deadlock detected (SQLSTATE 40P01)` error in the logs.

Pump instances now write aggregated analytics in a consistent order, so they no longer deadlock and can keep writing in parallel. The fix applies to the SQL Aggregate, GraphQL SQL Aggregate, MCP SQL Aggregate and Uptime Pumps, including date-sharded tables. If you reduced Pump to a single instance, or adjusted `batch_size` or `purge_delay`, to work around this issue, you can now revert those changes.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56855" target="_blank">CVE-2026-56855</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78662" target="_blank">CVE-2026-78662</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-84445" target="_blank">CVE-2026-84445</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56866" target="_blank">CVE-2026-56866</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78659" target="_blank">CVE-2026-78659</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78660" target="_blank">CVE-2026-78660</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78667" target="_blank">CVE-2026-78667</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78669" target="_blank">CVE-2026-78669</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-94439" target="_blank">CVE-2026-94439</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-94440" target="_blank">CVE-2026-94440</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-97031" target="_blank">CVE-2026-97031</a>
- <a href="https://security-tracker.debian.org/tracker/DSA-6572-1" target="_blank">DSA-6572-1</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-111943" target="_blank">CVE-2026-111943</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-63209" target="_blank">CVE-2026-63209</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-111497" target="_blank">CVE-2026-111497</a>

</Accordion>

</AccordionGroup>

### 1.17.0
#### Changelog
<a id="Changelog-v1.17.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>


<Accordion title='Updated Golang version to 1.26'>
{/* TT-17868 */}
Tyk Pump has been updated to build with Go 1.26, keeping the toolchain current with upstream Go security and performance updates.
</Accordion>

</AccordionGroup>

##### Added

<AccordionGroup>
<Accordion title="Secrets Management">
Tyk Pump now supports Secrets Management, letting you reference values from external secrets stores: the Tyk config file, environment variables, local files (including Kubernetes-mounted secrets), Consul, Vault, AWS Secrets Manager, Azure Key Vault, and GCP Secret Manager, in place of plain-text values in Pump's configuration, using a consistent reference syntax:

- `kv://<store>/<path>[#fragment]` for whole-value references.
- `$kv{<store>:<path>[#fragment]}` for embedding a reference inside a larger string.

The secrets management configuration block (`kv`) can itself contain references, for example pulling a Vault token from an environment variable.

References are resolved once, at startup. Individual secrets stores can be marked as `required` and, if they cannot be reached or initialized, Pump will fail to start with a clear error rather than starting with that store silently unavailable.

See [Secrets Management](/tyk-configuration-reference/kv-store) for full details.
</Accordion>

<Accordion title='Added GCP IAM authentication for Redis/Valkey'>
Tyk Pump can now authenticate to Google Cloud Memorystore for Redis/Valkey using GCP IAM-based authentication, as an alternative to a static Redis password.

Pump obtains a Google OAuth2 access token from the workload's identity, using Application Default Credentials (ADC) and authenticates with the Memorystore using this token.

This is supported for use with Memorystore for Valkey and Memorystore for Redis Cluster. The legacy non-cluster Memorystore for Redis has no IAM support from Google and continues to use a static password.

The IAM configuration is set in the new `iam_auth` section of Pump's storage config.

For more details see [IAM Authentication](/tyk-configuration-reference/redis-cluster-sentinel#iam-authentication).
</Accordion>
</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed the Kinesis pump failing to authenticate using EC2 instance or IRSA credentials'>
{/* TT-15741 */}
The Kinesis pump could fail to refresh AWS credentials obtained via the EC2 Instance Metadata Service (IMDS) or IAM Roles for Service Accounts (IRSA), due to incompatible versions of AWS SDK dependencies, causing analytics records to be silently dropped. Pump's AWS SDK dependencies have been updated to mutually compatible versions, restoring credential refresh for IMDS- and IRSA-based authentication. Static AWS credentials configured via environment variables continue to work as before.
</Accordion>

<Accordion title='Fixed the Kinesis pump logging errors for batches emptied by an API ID filter'>
{/* TT-15903 */}
When an API ID filter excluded every record in a batch, the Kinesis pump still attempted to send the resulting empty batch to AWS, which was rejected with a validation error and logged repeatedly. No analytics data was lost, but the errors created log noise. The Kinesis pump now skips the request entirely when a batch contains no records after filtering.
</Accordion>

<Accordion title='Fixed the hybrid Pump leaking RPC connections to MDCB after a reconnect'>
{/* TT-14423 */}
In a hybrid deployment, the hybrid Pump did not close its previous RPC connection to MDCB when recovering from a Pump-to-MDCB outage that outlasted `call_timeout` (10s by default) — for example, during an MDCB restart. Each such recovery permanently added another `rpc_pool_size` (default 5) connections to MDCB without releasing the earlier ones, so repeated outages caused MDCB's connection count, goroutines and memory usage to keep climbing, in some cases leading to MDCB running out of memory.

The hybrid Pump now stops the RPC client it is replacing before establishing a new one, so its connection count to MDCB stays constant no matter how many times it reconnects.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39822" target="_blank">CVE-2026-39822</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42505" target="_blank">CVE-2026-42505</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46600" target="_blank">CVE-2026-46600</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56852" target="_blank">CVE-2026-56852</a>
</Accordion>

</AccordionGroup>

### 1.16.0
#### Changelog
<a id="Changelog-v1.16.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>
<Accordion title='Standardize application log format'>
We have made changes to the format of Tyk Pump application logs to make them consistent with logs generated by other Tyk components and aligned with industry observability standards.

- **RFC3339 timestamps:** Log entries now use the industry-standard RFC3339 timestamp format (e.g. `2024-12-12T13:59:08Z`), replacing the previous default `logrus` timestamp format.
- **Log message field renamed:** The log message field is renamed from `msg` to `message`, for consistency with other Tyk components.

**Configuration**

The [`log_format`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#log_format) configuration option now accepts three explicit values:
 
- `text`: plain-text logs in the new format introduced in this release (**new default**).
- `json`: JSON logs in the new format.
- `legacy`: preserves the format used prior to Pump 1.16.0, with the historical timestamp format and the `msg` field name.
 
This is a **breaking change** for any user whose monitoring or log-parsing tools rely on the specific format of the Tyk Pump application logs. If you require the previous timestamp format or the `msg` field name, you should set `log_format` to `legacy`.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>
 
<Accordion title='Fix pre-aggregated analytics not being written to sharded PostgreSQL tables'>
Resolved an issue when using the Hybrid Pump to store aggregated analytics in a sharded PostgreSQL database.

Tyk Pump did not correctly write already-aggregated analytics to the correct date-specific (sharded) PostgreSQL table when table sharding was enabled. The date shard remained empty, errors were logged for the missing aggregate table, and Dashboard aggregate views such as *Activity by API* showed no traffic for the affected period.

Tyk Pump now writes aggregated records to the relevant date shard, whether or not the base aggregate table exists. The previously recommended workaround of disabling analytics table sharding is no longer required.
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
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-41889" target="_blank">CVE-2026-41889</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-2303" target="_blank">CVE-2026-2303</a>
- <a href="https://osv.dev/vulnerability/GHSA-xmrv-pmrh-hhx2" target="_blank">GHSA-xmrv-pmrh-hhx2</a>


</Accordion>

</AccordionGroup>

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
#### Changelog
<a id="Changelog-v1.14.1" data-scroll-offset></a>

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
#### Changelog
<a id="Changelog-v1.14.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Enhance Pump Security with Custom CA Certificates'>
You can now establish fully trusted and secure TLS connections from Tyk Pump to Kafka, Elasticsearch, and Splunk services that use certificates signed by specific Certificate Authorities.

To configure this, set the path to the PEM file containing the CA certificate using the `ssl_ca_file` option or the `TYK_PMP_PUMPS_<PUMP_NAME>_META_SSLCAFILE` environment variable.
</Accordion>

</AccordionGroup>

### 1.13.3
#### Changelog
<a id="Changelog-v1.13.3" data-scroll-offset></a>

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
#### Changelog
<a id="Changelog-v1.13.2" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fixed SQL Pump table sharding schema migration issues'>
Fixed an issue when using table sharding with the SQL pumps (creating new tables for each day), where the Pump would generate error messages in the application logs due to missing columns in the tables. This would only occur when upgrading to a newer version of Pump that added columns (e.g., to Pump 1.11.0, which added some GraphQL-related columns). Now, when Pump is upgraded, the current day's table will automatically be updated to include the new columns, so the error messages will not be generated.

We have added a new configuration option `migrate_sharded_tables`, which will update all pre-existing tables in the SQL database to match the latest schema. This will be a one-time operation when the Pump starts up, but could take some time to complete if there is a large history in the database, so it has been made an optional activity via this configuration option.
</Accordion>

</AccordionGroup>

### 1.13.1
#### Changelog
<a id="Changelog-v1.13.1" data-scroll-offset></a>

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

### 1.13.0
#### Changelog
<a id="Changelog-v1.13.0" data-scroll-offset></a>

##### Changed



<Expandable title='Deprecated payload decoding global configuration options'>
We have deprecated the global `DecodeRawRequest` and `DecodeRawResponse` configuration options, which were never correctly implemented and led to confusion when combined with the Pump-specific options. Now, if you want a Pump to decode the base64 encoded request and/or response payloads when transferring traffic logs to your data sink, you should set the options in the configuration for that Pump.
</Expandable>



##### Added



<AccordionGroup>
<Accordion title='Added support for encrypted Kinesis streams in Pump'>
Added support for server-side encryption in the Kinesis Pump by introducing a new configuration option [TYK_PMP_PUMPS_KINESIS_META_KMSKEYID](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-kinesis-meta-kmskeyid) that allows users to specify an AWS KMS key for encrypting data at rest in Kinesis Data Streams.

This enhancement enables compliance with strict regulatory requirements and security needs by automatically encrypting data before it's written to Kinesis storage and decrypting it when retrieved. The feature is backward compatible, with server-side encryption disabled by default for existing deployments.
</Accordion>

<Accordion title='Added "Latency.Total" and "Latency.Upstream" fields to InfluxDB2 Pump'>
Added two new fields to the traffic logs transferred using the InfluxDB2 pump. 

1. `Latency.Total`: Represents the total time taken to process a request.
2. `Latency.Upstream`: Represents the time taken to communicate with the upstream.

These metrics can be used to calculate the Gateway processing time (`Latency.Total` - `Latency.Upstream`), which is essential for monitoring API performance and diagnosing bottlenecks. This enhancement enables customers to perform comprehensive latency analysis and identify performance bottlenecks in their API infrastructure.
</Accordion>

<Accordion title='Added "batchbytes" configuration option for Kafka pump'>
Added a new configuration option [TYK_PMP_PUMPS_KAFKA_META_BATCHBYTES](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-kafka-meta-batch_bytes) to the Kafka pump that allows users to configure the maximum size (in bytes) of a batch before it is sent to a Kafka partition. 

This enhancement resolves issues where batched analytics data exceeded Kafka's default 1MB message size limit, causing "Message Size Too Large" errors and resulting in missing analytics data. Users can now optimize the batch size to match their Kafka topic configurations, with backward compatibility maintained through the default 1MB limit.
</Accordion>
</AccordionGroup>

### 1.12.2
#### Changelog
<a id="Changelog-v1.12.2" data-scroll-offset></a>

##### Changed



<Expandable title='Upgrade Tyk Pump to Golang 1.24'>
Tyk Pump has been upgraded to [Golang 1.24](https://tip.golang.org/doc/go1.24), improving security by staying current with the latest Go versions.
</Expandable>



##### Security Fixes



<Expandable title='High priority CVEs fixed'>
Fixed the following high-priority CVEs identified in Tyk Pump, providing increased protection against security vulnerabilities:<br />
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-22871" target="_blank">CVE-2025-22871</a><br />
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-22869" target="_blank">CVE-2025-22869</a>
</Expandable>

### 1.12.1
#### Changelog
<a id="Changelog-v1.12.1" data-scroll-offset></a>

##### Fixed



<Expandable title='Consolidated Syslog Log Entries'>
Resolved an issue in the Syslog Pump where enabling `Decode Raw Request` and `Decode Raw Response` caused log entries to fragment into multiple messages due to unescaped newline characters. The fix ensures a single consolidated log entry by escaping newlines (\n) in raw request and response fields, maintaining backward compatibility with existing log parsers.
</Expandable>

### 1.12.0
#### Changelog
<a id="Changelog-v1.12.0" data-scroll-offset></a>

##### Changed




<Expandable title='Support for PostgreSQL 17'>
Pump now supports PostgreSQL 17, ensuring compatibility with the latest database version.
</Expandable>



<Expandable title='Removal of SQLite Support in Pump'>
SQLite support has been fully removed from Pump.
</Expandable>





##### Security Fixes


<Expandable title='Fixed the following CVE'>
- [GHSA-v778-237x](https://github.com/advisories/GHSA-v778-237x-gjrc)
</Expandable>

### 1.11.2
#### Changelog
##### Fixed



<Expandable title='SQL-Pump Crashes When `tablesharding=true` and Analytics Are Skipped for Certain APIs'>
The issue causing a runtime error when writing analytics data for skipped APIs has been addressed. SQL-Pump now processes these scenarios gracefully, ensuring stability in table-sharded configurations.
</Expandable>



<Expandable title='Indices Not Created in Sharded `tyk_analytics` Tables if Indices with Same Names Exist in Earlier Tables'>
Fixed the index creation logic to ensure indices are generated correctly for all sharded tables, regardless of whether indices with the same names exist in earlier tables. This ensures proper query performance and data integrity in environments with table sharding.
</Expandable>

### 1.11.1
#### Changelog
##### Changed



<Expandable title='Upgraded Golang to v1.22.7'>
Updated to the [Go v1.22.7](https://go.dev/doc/devel/release#go1.22) to leverage its performance improvements, bug fixes, and security patches.
</Expandable>



##### Security Fixes
- Fixed the following CVEs:
    - [GHSA-7jwh-3vrq-q3m8](https://github.com/jackc/pgproto3/security/advisories/GHSA-7jwh-3vrq-q3m8)
    - [GHSA-mrww-27vc-gghv](https://github.com/advisories/GHSA-mrww-27vc-gghv)
    - [GO-2024-2611](https://pkg.go.dev/vuln/GO-2024-2611)

### 1.11.0
#### Changelog
##### Added



<Expandable title='Add Kinesis backend support'>
Tyk Pump now supports Kinesis as a backend to push analytics to a data lake efficiently.
</Expandable>

### 1.8.3
#### Changelog
##### Fixed
- Corrected configuration for _pumps.kafka.meta.timeout_ to be interpreted as the number of seconds (_Type: int_) instead of a duration requiring a unit (_Type: Duration_).
- Fixed an issue where _Graph SQL Pump_ couldn't restart correctly when analytics storage table name was changed in Pump config. Some relations were not torn down and migrated correctly.

### 1.8.2
#### Changelog
##### Fixed
- Resolved performance issue where _SQL Aggregate_ analytics failed to load on the _Dashboard_ during heavy traffic by introducing a new index on the _sql_aggregate_ Pump called _idx_dimension_.
- Fixed _Prometheus Pump_ crashes on non UTF-8 URLs by updating to _prometheus-client_ v1.16.
- Fixed _MongoDB_ connection string issues related to certain characters ("?" and "@"), recommending URL-encoded values in usernames and passwords. This ensures compatibility with both _mgo_ and _mongo-go_ drivers.
- Fixed security vulnerabilities: _CVE-2022-36640_, _CVE-2022-21698_, _GO-2022-0322_ and _GHSA-cg3q-j54f-5p7p_.

##### Added
- Add `track_all_paths` configuration for _Prometheus Pump_. If enabled, all APIs will have path in the `tyk_http_status_per_path` metric. Otherwise, only endpoint that have "track" plugin set with have path shown in the metric. Endpoints without “track” plugin set will have “unknown” path shown in the metric.

##### Updated
- Improved security by obfuscating _Mongo Pump_ credentials in log outputs.

### 1.8.1
#### Changelog
##### Fixed
- GraphQL analytics records were being excluded from the _tyk_analytics_ collection for Mongo Pump. This has been fixed so that GraphQL analytic records are now included as expected.
- Fixed MongoDB connection issue when using a password with URL escape characters (with mongo-go driver)
- Fixed an issue in Prometheus pump when filtering fields , e.g. _API Name_, that contain `--` in their value. For example, `test--name`. Prometheus Pump filtered the field as two separate instances, e.g. `test` & `name`, instead of the expected `test--name`.
- When [`omit_configfile`](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#omit_config_file) is set to `true`, Pump will not try to load the config file and spit out error logs

##### Updated
- Updated the default Hybrid Pump RPC pool size from 20 to 5 connections in order to reduce default CPU and memory footprint. See [Pump configurations](/tyk-pump/tyk-pump-configuration/tyk-pump-environment-variables#pumps-hybrid-meta-rpcpoolsize)
- Import and use latest [storage library v1.0.5](https://github.com/TykTechnologies/storage/releases/tag/v1.0.5)
- Updated default MongoDB driver to `mgo`. [Follow this guide to update the driver type](https://github.com/TykTechnologies/tyk-pump#driver-type)
- Pump name is now case-insensitive. It will override two or more pumps with the same name but in different cases (e.g. _Mongo_ / _mongo_)

### 1.8.0
#### Changelog
##### Added
- Added GraphQL SQL Aggregate Pump.
- Added Resurface Pump - Resurface can provide context-based security analysis for attack and failure triage, root cause, threat and risk identification based on detailed API logs sent from Tyk Pump.
- Add config option raw_request_decoded and raw_response_decoded for decoding from base64 the raw requests/responses fields before writing to Pump. This is useful if you want to search for specific values in the raw request/response. Both are disabled by default. This setting is not available for Mongo and SQL pumps, since the dashboard will decode the raw request/response.
- Add the ability to generate future data in demo mode using --demo-future-data flag. 
- Remove critical CVE go.uuid vulnerability 
- Use the latest Tyk storage library to connect to Mongo 
- Hybrid Pump refactoring - we now have better RPC connection control, testability, and documentation 

##### Fixed
- Std pump does not log accurate time when set to json format 
- GraphPump doesn’t include names of queries/mutation and subscriptions called 
- Mongo Pump’s connection hangs forever if misconfigured 
