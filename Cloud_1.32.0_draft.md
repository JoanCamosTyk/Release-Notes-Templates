## 1.32 Release Notes
### 1.32.0 Release Notes
#### Release Date xx May 2026
#### Release Highlights

**Export Cloud Gateway metrics to observability providers**

This release completes the Tyk Cloud observability triangle by adding gateway metrics export, joining the distributed traces and application logs already supported through the OpenTelemetry pipeline. Customers with the Telemetry entitlement can now stream RED (Rate, Error, Duration) metrics from their Cloud Data Plane gateways into their existing observability backend and correlate them with the traces and logs they already receive — providing the metrics signal needed for dashboards, alerting, and SLO tracking.

This release adds a new **Enable Metrics** toggle to the Telemetry Export section of every Cloud Data Plane deployment, positioned alongside the existing **Enable Traces** and **Enable Logs** toggles. When enabled, metrics flow through the OpenTelemetry Collector to the provider configured at the organisation level — New Relic, Elastic, Dynatrace, Datadog, or any OpenTelemetry-native backend via the Custom option. On the deployment API, the new feature is controlled by the `openTelemetry.metrics.enabled` field (default `false`); the existing `telemetry` field continues to work for traces, so existing API consumers are unaffected.

By default, Tyk exports RED histograms scoped to HTTP method, API ID, and HTTP response code — the dimensions needed for rate, error, and latency monitoring. Metrics use OpenTelemetry's cumulative temporality model. Additional dimensions and filtering are available on request through Tyk support to keep cardinality predictable. Gateway metrics export requires the Cloud Data Plane to run Gateway v5.13.0 or later. See [Configure Telemetry in Tyk Cloud](/tyk-cloud/telemetry) for setup instructions.

This release also fixes a bug where Hybrid Data Plane Groups could incorrectly appear as **NOT CONNECTED** on the Cloud Deployments list page even when their gateways were running.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.32.0) below.

#### Breaking Changes
There are no breaking changes in this release.
#### Downloads
- [latest version of Mserv](https://github.com/TykTechnologies/mserv/releases/latest)
#### Deprecations
There are no deprecations in this release.
#### Changelog 
<a id="Changelog-v1.32.0" data-scroll-offset></a>
##### Added
<AccordionGroup>

<Accordion title='Export Cloud Gateway metrics to observability providers'>
Tyk Cloud customers with the Telemetry entitlement can now export gateway metrics to their observability backend through the same OpenTelemetry pipeline already used for distributed traces and application logs. A new **Enable Metrics** toggle has been added to the Telemetry Export section of each Cloud Data Plane deployment, sending RED (Rate, Error, Duration) histograms — scoped to HTTP method, API ID, and HTTP response code — to the provider configured at the organisation level (New Relic, Elastic, Dynatrace, Datadog, or any OpenTelemetry-native backend via the Custom option). On the deployment API, metrics export is controlled by the new `openTelemetry.metrics.enabled` field (default `false`); the existing `telemetry` field continues to work for traces, so existing API consumers are unaffected. Metrics use OpenTelemetry's cumulative temporality model; additional dimensions and filtering are available on request through Tyk support. Gateway metrics export requires the Cloud Data Plane to run Gateway v5.13.0 or later. See [Configure Telemetry in Tyk Cloud](/tyk-cloud/telemetry) for setup instructions.
</Accordion>

</AccordionGroup>

##### Fixed
<AccordionGroup>

<Accordion title='Fixed Hybrid Data Plane connection status on the Deployments list'>
Resolved an issue where a Hybrid Data Plane Group could appear as **NOT CONNECTED** with "0 connected" gateways on the Cloud Deployments list page, even when the gateway was registered and running. Opening the same Data Plane Group showed the correct **CONNECTED** status, gateway count, and version in the detail view. The list and detail views now report the same connection state, so the Deployments page reflects reality on page load and refresh.
</Accordion>

</AccordionGroup>
