## Pump 1.15.1 Release Notes (DRAFT)

## Breaking Changes

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

## Release Highlights

### 1.15.1
Tyk Pump 1.15.1 standardises application logs to an RFC3339-compliant format, aligning with the rest of the Tyk stack, with a `legacy` fallback for tooling that depends on the previous output. This release also fixes pre-aggregated analytics not being written to sharded PostgreSQL tables when a data-plane Pump uses the `hybrid` backend.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.15.1) below.

## Change Log

### 1.15.1
#### Changelog
<a id="Changelog-v1.15.1" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fix inconsistent application log format'>
We fixed an inconsistency where Tyk Pump application logs did not match the standardized format used across other Tyk components.

Tyk Pump now outputs a consistent, RFC3339-compliant timestamp across all application log entries.

The `log_format` configuration option (and the `TYK_PMP_LOGFORMAT` environment variable) now accepts three values:
- `text` (the new default, RFC3339 plain text)
- `json` (RFC3339 JSON)
- `legacy` (the previous behaviour, including the historical timestamp format and the `msg` field name).

Under `text` and `json`, the log message field is also renamed from `msg` to `message` for consistency with other Tyk components.

This is a **breaking change**: customers whose monitoring or log-parsing tools rely on the previous timestamp format or the `msg` field name should set `log_format` to `legacy`.
See the Breaking Changes section for details.
</Accordion>

<Accordion title='Fix pre-aggregated analytics not being written to sharded PostgreSQL tables'>
Resolved an issue where Tyk Pump's SQL aggregate storage did not write already-aggregated analytics to the correct date-specific (sharded) PostgreSQL table when table sharding was enabled. This affected deployments using the Pump `hybrid` backend with aggregated analytics: the date shard stayed empty, errors were logged about the missing aggregate table, and Dashboard aggregate views such as Activity by API showed no traffic for the affected period.

Tyk Pump now writes aggregated records to the relevant date shard, whether or not the base aggregate table exists, so the previous workaround of disabling analytics table sharding is no longer required.
</Accordion>

</AccordionGroup>
