## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 1.18.0
In this release, we have implemented support for the Client IdP registry introduced in Tyk 5.14.0. This feature provides centralized management of IdPs and scope-to-policy mappings used with API Products secured with JWT Authentication, resolving a historical issue with Dynamic Client Registration configuration not being properly cleaned up when associated API Products or Plans are deleted.
This feature is available only from Tyk Gateway 5.14.0 and Tyk Dashboard 5.14.0 and must be enabled in the Developer Portal by setting the new configuration option TYK_PORTAL_ENABLEIDPREGISTRY to true.
This release also fixes some issues in the Developer Portal API and when searching in the Admin Portal. We have also resolved several recent CVEs to strengthen security.
For a comprehensive list of changes, please refer to the detailed changelog below.

### 1.17.3
This release restores the environment variable behaviour that changed in 1.17.1 and 1.17.2, where some `PORTAL_*` variables were no longer recognised. Most notably, `PORTAL_DISABLE_CSRF_CHECK` is honoured again, resolving the "bad request" error customers hit at login. Every legacy `PORTAL_*` variable now works exactly as it did in 1.17.0, and Portal also gains a new `TYK_PORTAL_` prefix with a consistent naming style that matches the convention used across other Tyk components.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.17.3) below.

## Change log

### 1.18.0
#### Changelog
<a id="Changelog-v1.18.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Updated embedded Tyk Identity Broker to version 1.7.3'>
{/* TT-17453 */}
Tyk Developer Portal embeds Tyk Identity Broker. This has been updated to [version 1.7.3](/developer-support/release-notes/tib#1-7-3-release-notes).
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Deleting DCR Products or Plans did not remove IdP details from API definition'>
With the introduction of the new Client IdP registry in Tyk Dashboard (from Tyk 5.14.0) we have resolved an issue seen when deleting an API Product that uses Dynamic Client Registration (DCR). When deleting the API Product, the associated policy is correctly deleted from the Dashboard, but the JWKS endpoint and scope-to-policy mappings are not deleted from the associated API definitions. This limitation is caused by uncertainty over the provenance of that configuration in the API definition.

The Client IdP registry removes this uncertainty through central management of Identity Provider configuration.

The Developer Portal will use the Client IdP Registry to manage associations between OAuth 2.0 Providers (Identity Providers) and the APIs included in an API Product under the following circumstances:

- Tyk Gateway version 5.14.0 or later
- Tyk Dashboard version 5.14.0 or later
- Tyk Developer Portal version 1.18.0 or later with [`TYK_PORTAL_ENABLEIDPREGISTRY`](/product-stack/tyk-enterprise-developer-portal/deploy/configuration#tyk_portal_enableidpregistry) set to `true`

If [`TYK_PORTAL_ENABLEIDPREGISTRY`](/product-stack/tyk-enterprise-developer-portal/deploy/configuration#tyk_portal_enableidpregistry) is not set, then the Portal will continue to manage the Identity Provider details in the API definitions and is unable to remove these if the API Product is deleted.
</Accordion>

<Accordion title='Fix error when searching Teams by name or Organisation in Admin Portal'>
Resolved an issue where searching on the Teams page (**API Consumers > Teams**) in the Admin Portal returned a database error whenever the search term was not numeric, such as a Team or Organisation name.

Team search now works for both numeric IDs and text: a numeric value matches teams by ID, text matches team names with a case-insensitive substring search, and non-numeric input is handled safely instead of producing an error. The Organisation filter is unaffected.
</Accordion>

<Accordion title='Fields missing from Portal API response'>
Resolved an issue seen when retrieving an API Plan using the `GET /portal-api/plans/{plan_id}` endpoint. The data returned by this call omitted the `Catalogues` field but now correctly returns the list of IDs for the catalogues associated with the API Plan, so that plan-to-catalogue associations can be verified directly without extra API calls.

Resolved an issue seen when retrieving a Catalogue using the `GET /portal-api/catalogues/{id}` endpoint. The data returned by this call omitted the `NameWithSlug` field but now correctly returns this in the response payload.
</Accordion>


</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33813" target="_blank">CVE-2026-33813</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39830" target="_blank">CVE-2026-39830</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39831" target="_blank">CVE-2026-39831</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39833" target="_blank">CVE-2026-39833</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42508" target="_blank">CVE-2026-42508</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46595" target="_blank">CVE-2026-46595</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39821" target="_blank">CVE-2026-39821</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39829" target="_blank">CVE-2026-39829</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46599" target="_blank">CVE-2026-46599</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42504" target="_blank">CVE-2026-42504</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-45447" target="_blank">CVE-2026-45447</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34180" target="_blank">CVE-2026-34180</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-45445" target="_blank">CVE-2026-45445</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-7383" target="_blank">CVE-2026-7383</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-9076" target="_blank">CVE-2026-9076</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46602" target="_blank">CVE-2026-46602</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46604" target="_blank">CVE-2026-46604</a>
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
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42500" target="_blank">CVE-2026-42500</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42507" target="_blank">CVE-2026-42507</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27145" target="_blank">CVE-2026-27145</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42766" target="_blank">CVE-2026-42766</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42767" target="_blank">CVE-2026-42767</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-45446" target="_blank">CVE-2026-45446</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34182" target="_blank">CVE-2026-34182</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-2303" target="_blank">CVE-2026-2303</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46601" target="_blank">CVE-2026-46601</a>
- <a href="https://osv.dev/vulnerability/GHSA-xmrv-pmrh-hhx2" target="_blank">GHSA-xmrv-pmrh-hhx2</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39824" target="_blank">CVE-2026-39824</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34181" target="_blank">CVE-2026-34181</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34183" target="_blank">CVE-2026-34183</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42764" target="_blank">CVE-2026-42764</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42768" target="_blank">CVE-2026-42768</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42769" target="_blank">CVE-2026-42769</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42770" target="_blank">CVE-2026-42770</a>


</Accordion>

</AccordionGroup>


### 1.17.3
#### Changelog
<a id="Changelog-v1.17.3" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fix Portal environment variables no longer being recognised'>
Resolved a regression introduced in 1.17.1 where some configuration environment variables stopped taking effect. The most visible symptom was that `PORTAL_DISABLE_CSRF_CHECK` was silently ignored, causing a "bad request" error when users tried to log in. All legacy `PORTAL_*` variable names now work exactly as they did in 1.17.0, so the documented underscore-separated names are honoured again (with the exception of `FULLSTORY_ENABLED` and `PORTAL_ALPHA_POLICY_SERVICE_ENABLED`, which are intentionally deprecated).

This release also introduces a new `TYK_PORTAL_` prefix for configuration environment variables, using a single consistent naming style that matches the convention used by other Tyk components such as the Dashboard (`TYK_DB_*`). When the same setting is provided under both prefixes, the `TYK_PORTAL_` value takes precedence over the `PORTAL_` value.
</Accordion>

</AccordionGroup>
