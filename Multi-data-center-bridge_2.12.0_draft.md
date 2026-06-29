## MDCB 2.12.0 Release Notes (DRAFT)

## Release Highlights

### 2.12.0
This release extends the Client IdP registry to multi-data-centre deployments, so Data Plane Gateways receive registry-managed identity providers over RPC — segment-filtered in the same way as API definitions. It also fixes aggregated analytics not being written to sharded PostgreSQL tables when a data-plane Tyk Pump uses the `hybrid` backend.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.12.0) below.

## Change Logs

### 2.12.0
#### Changelog
<a id="Changelog-v2.12.0" data-scroll-offset></a>

##### Added

<AccordionGroup>

<Accordion title='Distribute the Client IdP registry to Data Plane Gateways'>
Client IdP registry entries are now distributed to Data Plane Gateways in MDCB deployments, the same way API definitions already are. Previously the registry was available only in single-data-centre deployments, so JWT credentials relying on a registry-managed identity provider were rejected by edge Gateways connected through MDCB.

Each Data Plane Gateway now receives only the identity providers bound to the APIs it serves — filtered by its segment tags, with each provider's API mappings pruned to that Gateway's APIs. As a result, no Gateway sees identity provider details (JWKS URIs, issuers, or scope-to-policy mappings) for APIs it does not serve. Changes made on the Control Plane propagate to connected Gateways automatically.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix aggregated analytics missing from sharded PostgreSQL tables when using the Pump hybrid backend'>
Resolved an issue where, with MDCB analytics table sharding enabled, pre-aggregated analytics forwarded by a data-plane Tyk Pump using the `hybrid` backend were not written to the date-specific PostgreSQL aggregate table. The date shard stayed empty, MDCB logged errors about the missing aggregate table, and Dashboard aggregate views such as Activity by API showed no traffic for the affected period.

Aggregated analytics are now written correctly to the relevant date shard, whether or not the base aggregate table exists, so the previous workaround of disabling MDCB analytics table sharding is no longer required.
</Accordion>

</AccordionGroup>
