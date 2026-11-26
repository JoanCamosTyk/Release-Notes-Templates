# Tyk Identity Broker Release Notes

Release notes for Tyk Identity Broker, grouped by section (Breaking Changes, Release Highlights, Dependencies, Deprecations, Upgrade Instructions, Downloads, Change Log). Within each section, versions are listed newest first.

Use this file as the reference for how much detail we give and how we phrase things for this component. Follow the rules in `CLAUDE.md` when drafting anything new.

Published source: [https://tyk.io/docs/developer-support/release-notes/tib](https://tyk.io/docs/developer-support/release-notes/tib). Every version on the published page (1.6.1 to 1.8.4) was synced word for word on 2026-11-26 (only heading levels changed to fit this file).

Not on the published page (Claude drafts, may still change or may never have been published): 1.8.2. Anything marked **[TO CONFIRM]** in them still needs checking.

## Breaking Changes

## Release Highlights

### 1.8.4
**Release date:** 10 November 2026

This release addresses several CVEs in dependent libraries. There are no functional changes.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.8.4) below.

### 1.8.3
**Release date:** 22 October 2026

This is a security patch release for Tyk Identity Broker. It contains no functional or behavioral changes; it addresses several CVEs identified in dependent libraries.

Tyk Identity Broker 1.8.2 contained a minor functional regression, so was withdrawn before being formally published. 

We strongly recommend that you upgrade directly to 1.8.3 as soon as possible.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.8.3) below.

### 1.8.2
This is a security patch release for Tyk Identity Broker. It addresses CVEs in dependent libraries and in the container base image, and contains no other functional changes. For the CVEs addressed, see the Security Fixes section of the detailed [changelog](#Changelog-v1.8.2) below.

### 1.8.1
**Release date:** 14 October 2026

This patch fixes the issue where users who log in to the Tyk Developer Portal through Single Sign-On have missing given and family names. This release also addresses several CVEs in dependent libraries.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.8.1) below.

### 1.8.0
**Release date:** 26 August 2026

Tyk Identity Broker supports IAM-based authentication for Google Cloud Memorystore for Redis Cluster and Valkey, as an alternative to a static password.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.8.0) below.

### 1.7.3
**Release date:** 23 June 2026

In this patch, we have fixed an issue where Tyk Identity Broker (TIB) did not honor the host application's configured log verbosity when embedded in Tyk Dashboard or Tyk Developer Portal. We have also resolved several CVEs to strengthen security.

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

### 1.6.1
**Release date:** 5 Nov 2024

{/* Required. Use similar ToV to previous release notes. For example for a patch release:
This release primarily focuses on bug fixes.
For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-vX.Y.0) below. */}
#### Enhanced Security with JWE Support for OIDC SSO
This release introduces JSON Web Encryption (JWE) support for OpenID Connect (OIDC) Single Sign-On (SSO) in the Tyk Identity Broker (TIB). With this enhancement, organizations can achieve greater security for token handling during authentication flows. JWE token validation and processing are now seamlessly integrated, offering configurable private key support for decryption.

## Dependencies

### 1.8.4
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.26       | 1.26       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 7.0, 8.0, 9.0 | 4.4.x, 5.x, 6.x, 7.0, 8.0 |
| [Redis](https://redis.io/download/)         | 7.x - 8.x        | 7.x - 8.x            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.8.3
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.26       | 1.26       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.8.2
#### Compatibility Matrix for Tyk Components

Tyk Identity Broker 1.8.2 is embedded in Tyk Dashboard 5.16.0 and Tyk Developer Portal 1.20.0.

*[TO CONFIRM: please fill in the tested compatibility matrix before publishing, and confirm whether Dashboard 5.13.5 (LTS) also embeds 1.8.2.]*

#### 3rd Party Dependencies & Tools

| Third Party Dependency | Tested Versions | Compatible Versions | Comments |
|---|---|---|---|
| [Go](https://go.dev/dl/) | 1.26 | 1.26 | Tyk Identity Broker is built with Go 1.26 |

*[TO CONFIRM: the Go fix for CVE-2026-111943 is in Go 1.26.10, so 1.8.2 should be built with Go 1.26.10 or later.]*

### 1.8.1
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.26       | 1.26       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.8.0
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.26       | 1.26       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.7.3
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.25       | 1.25       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.7.2
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.25       | 1.25       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.7.1
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- |
| [GoLang](https://go.dev/dl/)                               | 1.25       | 1.25       | 
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.7.0
#### 3rd Party Dependencies & Tools

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | Comments | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- | :-------- | 
| [GoLang](https://go.dev/dl/)                               | 1.21       | 1.21       | All our binaries |
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x | Used by Tyk Identity Broker |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            | Used by Tyk Identity Broker |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

### 1.6.1
{/* Required. Use this section to announce the following types of dependencies compatible with the release:

Version compatibility with other components in the Tyk stack. This takes the form of a compatibility matrix and is only required for Gateway and Portal.

3rd party dependencies and tools */}
      
#### 3rd Party Dependencies & Tools
{/* Required. Third-party dependencies encompass tools (GoLang, Helm etc.), databases (PostgreSQL, MongoDB etc.) and external software libraries. This section should be a table that presents the third-party dependencies and tools compatible with the release. Compatible is used in the sense of those versions tested with the releases. Such information assists customers considering upgrading to a specific release.

Additionally, a disclaimer statement was added below the table, for customers to check that the third-party dependency they decide to install remains in support.

An example is given below for illustrative purposes only. Tested Versions and Compatible Versions information will require discussion with relevant squads and QA. */}

| Third Party Dependency                                     | Tested Versions        | Compatible Versions    | Comments | 
| :---------------------------------------------------------- | :---------------------- | :---------------------- | :-------- | 
| [GoLang](https://go.dev/dl/)                               | 1.21       | 1.21       | All our binaries |
| [MongoDB](https://www.mongodb.com/try/download/community)  | 5.x, 6.x, 7.0 | 4.4.x, 5.x, 6.x and 7.0.x | Used by Tyk Identity Broker |
| [Redis](https://redis.io/download/)         | 6.x - 7.0        | 6.x - 7.0            | Used by Tyk Identity Broker |

Given the time difference between your upgrade and the release of this version, we recommend customers verify the ongoing support of third-party dependencies they install, as their status may have changed since the release.

## Deprecations

## Upgrade Instructions

### 1.8.4
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.8.3
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.8.2
Standard upgrade from Tyk Identity Broker 1.8.1. No special migration steps are required for this release.

*[TO CONFIRM, optional paragraph: This release removes the "emSign ECC Root CA - G3" and "Comodo AAA Services root" certificates from the trusted certificate store in the Tyk Identity Broker container image, and Tyk Identity Broker now rejects certificates issued by a set of compromised intermediate certificate authorities. If your identity provider uses a certificate that chains to these authorities, update it before upgrading.]*

### 1.8.1
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.8.0
For users currently on v1.7.3, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.7.3 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.7.3
For users currently on v1.7.2, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.7.2 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.7.2
For users currently on v1.7.1, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.7.1 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.7.1
For users currently on v1.7.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.7.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.7.0
For users currently on v1.6.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.6.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

### 1.6.1
{/* Required. For patches release (Z>0) use this: */}
For users currently on v1.6.0, we strongly recommend promptly upgrading to the latest release. If you are working with an older version (lower major), it is advisable to bypass version 1.6.0 and proceed directly to this latest patch release.
<br/>
Go to the [Upgrading Tyk](#upgrading-tyk) section for detailed upgrade Instructions.

## Downloads

### 1.8.4
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=v1.8.4)
  - ```bash
    docker pull tykio/tyk-identity-broker:v1.8.4@sha256:8b3c9cec4556f90154c3c228919c5be93dc3655ae59e2c16f19f56c0f7974ff1
    ```
- source code tarball for oss projects - [TIB v1.8.4](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.8.4)

### 1.8.3
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=v1.8.3)
  - ```bash
    docker pull tykio/tyk-identity-broker:v1.8.3@sha256:7d003a100b8d539dbdf9577fa5e85e731f6dfc1b80ed49cec4e772867cf712ba
    ```
- source code tarball for oss projects - [TIB v1.8.3](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.8.3)

### 1.8.2
*[TO CONFIRM: fill in once artifacts are published (standard pattern below)]*

- Docker: `docker pull tykio/tyk-identity-broker:v1.8.2`
- Helm: see the [Tyk Helm charts repository](https://github.com/TykTechnologies/tyk-charts)

### 1.8.1
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.8.1)
  - ```bash
    docker pull tykio/tyk-identity-broker:v1.8.1@sha256:9b57ea227ac792e66dde99e9cf850e4bf8c136d348199cc2a28c89a6863d8724
    ```
- source code tarball for oss projects - [TIB v1.8.1](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.8.1)

### 1.8.0
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.8.0)
  ```
  docker pull tykio/tyk-identity-broker:v1.8.0
  ```
- source code tarball for oss projects - [TIB v1.8.0](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.8.0)

### 1.7.3
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.7.3)
  ```
  docker pull tykio/tyk-identity-broker:v1.7.3
  ```
- source code tarball for oss projects - [TIB v1.7.3](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.7.3)

### 1.7.2
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.7.2)
  ```
  docker pull tykio/tyk-identity-broker:v1.7.2
  ```
- source code tarball for oss projects - [TIB v1.7.2](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.7.2)

### 1.7.1
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.7.1)
  ```
  docker pull tykio/tyk-identity-broker:v1.7.1
  ```
- source code tarball for oss projects - [TIB v1.7.1](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.7.1)

### 1.7.0
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.7.0)
  ```
  docker pull tykio/tyk-identity-broker:v1.7.0
  ```
- source code tarball for oss projects - [TIB v1.7.0](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.7.0)

### 1.6.1
- [Docker image to pull](https://hub.docker.com/r/tykio/tyk-identity-broker/tags?name=1.6.1)
  ```
  docker pull tykio/tyk-identity-broker:v1.6.1
  ```
- source code tarball for oss projects - [TIB v1.6.1](https://github.com/TykTechnologies/tyk-identity-broker/releases/tag/v1.6.1)

## Change Log

### 1.8.4
#### Changelog
<a id="Changelog-v1.8.4" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-116587" target="_blank">CVE-2026-116587</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-116585" target="_blank">CVE-2026-116585</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-116586" target="_blank">CVE-2026-116586</a>
</Accordion>

</AccordionGroup>

### 1.8.3
#### Changelog
<a id="Changelog-v1.8.3" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
- <a href="https://security-tracker.debian.org/tracker/DSA-6572-1" target="_blank">DSA-6572-1</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-111943" target="_blank">CVE-2026-111943</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-63209" target="_blank">CVE-2026-63209</a>
</Accordion>

</AccordionGroup>

### 1.8.2
#### Changelog
<a id="Changelog-v1.8.2" data-scroll-offset></a>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:

- <a href="https://security-tracker.debian.org/tracker/DSA-6572-1" target="_blank">DSA-6572-1</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-111943" target="_blank">CVE-2026-111943</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-63209" target="_blank">CVE-2026-63209</a>

</Accordion>

</AccordionGroup>

### 1.8.1
#### Changelog
<a id="Changelog-v1.8.1" data-scroll-offset></a>


##### Fixed

<AccordionGroup>

<Accordion title='Fixed the given name and family name for users logging in to the Developer Portal with SSO'>
{/* TT-14898 */}
We've resolved an issue where users logging in to the Tyk Developer Portal through OpenID Connect Single Sign-On had their full name stored in the First Name field, leaving the Last Name field empty. First and last names from the identity provider are now stored in their own fields.
The system automatically corrects existing users with incorrect names the next time they log in with SSO. If the identity provider doesn't send a first name, it uses the last name or email address instead, so the profile can still be saved.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-84304" target="_blank">CVE-2026-84304</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56855" target="_blank">CVE-2026-56855</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78662" target="_blank">CVE-2026-78662</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-84445" target="_blank">CVE-2026-84445</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56851" target="_blank">CVE-2026-56851</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56866" target="_blank">CVE-2026-56866</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78659" target="_blank">CVE-2026-78659</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78660" target="_blank">CVE-2026-78660</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78667" target="_blank">CVE-2026-78667</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-78669" target="_blank">CVE-2026-78669</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-94439" target="_blank">CVE-2026-94439</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-94440" target="_blank">CVE-2026-94440</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-97031" target="_blank">CVE-2026-97031</a>
</Accordion>

</AccordionGroup>

### 1.8.0
#### Changelog
<a id="Changelog-v1.8.0" data-scroll-offset></a>

##### Changed

<AccordionGroup>
<Accordion title='Updated Golang version to 1.26'>
{/* TT-17868 */}
Tyk Identity Broker has been updated to build with Go 1.26, keeping the toolchain current with upstream Go security and performance updates.
</Accordion>
</AccordionGroup>

##### Added

<AccordionGroup>

<Accordion title='Added GCP IAM authentication for Redis/Valkey'>
{/* TT-17611 */}
Tyk Identity Broker can authenticate to Google Cloud Memorystore for Redis/Valkey using GCP IAM-based authentication, as an alternative to a static Redis password.

TIB obtains a Google OAuth2 access token from the workload's identity, using Application Default Credentials (ADC) and authenticates with the Memorystore using this token.

This is supported for use with Memorystore for Valkey and Memorystore for Redis Cluster. The legacy non-cluster Memorystore for Redis has no IAM support from Google and continues to use a static password.

The IAM configuration is set in the new `iam_auth` section of TIB's storage config.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
We have addressed CVEs reported in dependent libraries, providing increased protection against security vulnerabilities, including, but not limited to:
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39822" target="_blank">CVE-2026-39822</a>
- <a href="https://osv.dev/vulnerability/GHSA-hrxh-6v49-42gf" target="_blank">GHSA-hrxh-6v49-42gf</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-41178" target="_blank">CVE-2026-41178</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-42505" target="_blank">CVE-2026-42505</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-46600" target="_blank">CVE-2026-46600</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-56852" target="_blank">CVE-2026-56852</a>
</Accordion>

</AccordionGroup>

### 1.7.3
#### Changelog
<a id="Changelog-v1.7.3" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Inherit Log Verbosity When Embedded'>
When Embedded in Tyk Dashboard or Tyk Developer Portal, TIB now inherits the host application's configured application log verbosity.

Previously, TIB emitted its log output at a fixed verbosity regardless of the Dashboard or Portal setting, so TIB messages could appear even when the Dashboard was configured to a less verbose level.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Resolved CVEs'>
Addressed the following CVEs, providing increased protection against security
vulnerabilities, including, but not limited to:

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

### 1.6.1
#### Changelog
{/* Required. The change log should include the following ordered set of sections below that briefly summarise the features, updates and fixed issues of the release.

Here it is important to explain the benefit of each changelog item. As mentioned by James in a previous Slack message (https://tyktech.slack.com/archives/C044R3ZTN6L/p1686812207060839?thread_ts=1686762128.651249&cid=C044R3ZTN6L):
"...it is important to document the customer impact for the work delivered, so we can share it with prospects/install base. For example:
"New Chart delivers x and y benefit to a and b customer use cases. The business impact for them will be this and that" */}

##### Added
{/* This section should be a bullet point list of new features. Explain:

- The purpose of the new feature
- How does the new feature benefit users?
- Link to documentation of the new feature
- For OSS - Link to the corresponding issue if possible on GitHub to allow the users to see further info.

Each change log item should be expandable. The first line summarises the changelog entry. It should be then possible to expand this to reveal further details about the changelog item. This is achieved using HTML as shown in the example below. */}


<Expandable title='Support for JSON Web Encryption (JWE) in OIDC SSO with TIB'>
This release adds support for JSON Web Encryption (JWE) in OIDC Single Sign-On (SSO) with TIB, providing enhanced security for token handling in authentication flows. This feature enables processing and validation of JWE tokens, with configuration options for setting the private key required for decryption.

For more details, refer to the [OIDC SSO with JWE](/api-management/single-sign-on-social-idp#json-web-encryption-jwe) documentation.
</Expandable>




---

{/* Repeat the release notes section above for every patch here */}


{/* The footer of the release notes page. It contains a further information section with details of how to upgrade Tyk,
links to API documentation and FAQs. You can copy it from the previous release. */}
