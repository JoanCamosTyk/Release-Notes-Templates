## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 1.7.3
This release ensures that the embedded Identity Broker honours the Tyk Dashboard's configured log verbosity, so TIB log output is consistent with the rest of the Dashboard's application logs.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.7.3) below.

### 1.7.2
In this release, we have addressed CVEs to strengthen security.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.7.2) below.

### 1.7.1
In this release, we have updated Tyk Identity Broker (TIB) to Golang 1.25 for enhanced security and performance.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.7.1) below.

### 1.7.0
This release introduces enhancements to TIB, improving group-based permission mapping, adding support for proxy settings from environment variables, and allowing dynamic state values in the OAuth2 flow. 

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.7.0) below.


## Change log

### 1.7.3
#### Changelog
<a id="Changelog-v1.7.3" data-scroll-offset></a>

##### Changed

<AccordionGroup>

<Accordion title='Embedded Identity Broker now follows the Dashboard log level'>
When running embedded in the Tyk Dashboard, the Identity Broker now honours the Dashboard's configured application log verbosity. Previously TIB emitted its log output at a fixed verbosity regardless of the Dashboard setting, so TIB messages could appear even when the Dashboard was configured to a less verbose level. TIB log output now respects the Dashboard's log level — including the new `TYK_DB_LOGLEVEL` environment variable and the global `TYK_LOGLEVEL` — keeping all Dashboard logs consistent.
</Accordion>

</AccordionGroup>

### 1.7.2
#### Changelog
<a id="Changelog-v1.7.2" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33811" target="_blank">CVE-2026-33811</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33814" target="_blank">CVE-2026-33814</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39820" target="_blank">CVE-2026-39820</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39836" target="_blank">CVE-2026-39836</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42499" target="_blank">CVE-2026-42499</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32952" target="_blank">CVE-2026-32952</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39823" target="_blank">CVE-2026-39823</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39826" target="_blank">CVE-2026-39826</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39825" target="_blank">CVE-2026-39825</a>


</Accordion>

</AccordionGroup>

### 1.7.1
#### Changelog
<a id="Changelog-v1.7.1" data-scroll-offset></a>

##### Changed
<AccordionGroup>

<Accordion title='Updated Go version to 1.25'>
The Tyk Identity Broker (TIB) has been updated to Golang 1.25, improving security by staying up-to-date with Go versions.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='CVE fixed'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33186" target="_blank">CVE-2026-33186</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33487" target="_blank">CVE-2026-33487</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32285" target="_blank">CVE-2026-32285</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-22868" target="_blank">CVE-2025-22868</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-30204" target="_blank">CVE-2025-30204</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2024-45338" target="_blank">CVE-2024-45338</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-10543" target="_blank">CVE-2025-10543</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-22870" target="_blank">CVE-2025-22870</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-22872" target="_blank">CVE-2025-22872</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-27144" target="_blank">CVE-2025-27144</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-47911" target="_blank">CVE-2025-47911</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-58181" target="_blank">CVE-2025-58181</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-58190" target="_blank">CVE-2025-58190</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34986" target="_blank">CVE-2026-34986</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39883" target="_blank">CVE-2026-39883</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39882" target="_blank">CVE-2026-39882</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2024-51744" target="_blank">CVE-2024-51744</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2025-29923" target="_blank">CVE-2025-29923</a>

</Accordion>

</AccordionGroup>


### 1.7.0
#### Changelog
<a id="Changelog-v1.7.0" data-scroll-offset></a>

##### Added


<AccordionGroup>
<Accordion title='Load Proxy Settings from Environment Variables'>
TIB now respects `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environment variables when making outbound connections. This change ensures compatibility with air-gapped Kubernetes environments where external services can only be accessed via an HTTP proxy.
</Accordion>

<Accordion title='Dynamic State Query Support in OAuth2 Flow'>
The OAuth2 "state" field can now be dynamically set via the URL or form-encoded body. This improvement allows integration with external APIs that require custom state values, ensuring compliance with various regulatory and enterprise authentication requirements.
</Accordion>

<Accordion title='Improved Multi-Group Permission Mapping for Identity Providers'>
Previously, TIB assigned a user to the last matched group when multiple groups were mapped, regardless of the identity provider (SAML, LDAP, OAuth, OIDC, etc.). The new functionality introduces support for multi-group mapping, allowing permissions to be merged. This update is backward compatible and ensures that multi-group rights (combined permissions) are only applied if the user does not have a `groupId` assigned via the Dashboard.
</Accordion>
</AccordionGroup>



##### Security Fixes


<Expandable title='Fixed the following CVE'>
- [GHSA-v778-237x](https://github.com/advisories/GHSA-v778-237x-gjrc)
</Expandable>

