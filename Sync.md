## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 2.1.7
Tyk Sync 2.1.7 has updated the Golang version to 1.25, fixed CVEs and a performance issue, and ensures compatibility with the most recent Tyk LTS release [5.8.13](/developer-support/release-notes/dashboard#5-8-13-release-notes).

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v2.1.7) below.

### 2.1.6
Tyk Sync 2.1.6 is a version alignment release that ensures compatibility with the most recent Tyk LTS release [5.8.12](/developer-support/release-notes/dashboard#5-8-12-release-notes).

### 2.1.5
Tyk Sync 2.1.5 is a version alignment release that ensures compatibility with the most recent Tyk LTS release [5.8.9](/developer-support/release-notes/dashboard#5-8-9-release-notes). It contains one bug fix.

### 2.1.4
Tyk Sync 2.1.4 is a version alignment release that ensures compatibility with the most recent Tyk LTS release [5.8.6](/developer-support/release-notes/dashboard#5-8-6-release-notes). No functional changes have been implemented in this release.

### 2.1.3
This patch release upgrades Tyk Sync to use Go 1.24 for enhanced security and stability.

Please refer to the [changelog](#Changelog-v2.1.3) below for detailed explanation.

## Change log

### 2.1.7
##### Changed

<AccordionGroup>

<Accordion title='Update Golang version to 1.25'>
We have updated Tyk Sync to Golang 1.25, improving security by staying up-to-date with Golang versions.
</Accordion>

</AccordionGroup>

##### Fixed

<AccordionGroup>

<Accordion title='Fix severe performance degradation in large-scale sync operations'>
Resolved an issue where syncing large numbers of policies and APIs could take hours instead of minutes. Tyk Sync will now correctly sync large deployments at the performance levels similar to those seen in v1.5.0.
</Accordion>

</AccordionGroup>

##### Security Fixes

<AccordionGroup>

<Accordion title='Fix multiple CVEs'>
Addressed the following CVEs, providing increased protection against security vulnerabilities.

- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33186" target="_blank">CVE-2026-33186</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-25679" target="_blank">CVE-2026-25679</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32285" target="_blank">CVE-2026-32285</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-1229" target="_blank">CVE-2026-1229</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32280" target="_blank">CVE-2026-32280</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39883" target="_blank">CVE-2026-39883</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32282" target="_blank">CVE-2026-32282</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32283" target="_blank">CVE-2026-32283</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32281" target="_blank">CVE-2026-32281</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32288" target="_blank">CVE-2026-32288</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-39882" target="_blank">CVE-2026-39882</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-32289" target="_blank">CVE-2026-32289</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27139" target="_blank">CVE-2026-27139</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-27142" target="_blank">CVE-2026-27142</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-33762" target="_blank">CVE-2026-33762</a>
- <a href="https://nvd.nist.gov/vuln/detail/CVE-2026-34165" target="_blank">CVE-2026-34165</a>

</Accordion>

</AccordionGroup>

### 2.1.5
##### Fixed

<Expandable title='Fixed error when deleting policies'>
Fixed an issue introduced in v2.1.0 where Tyk Sync would not successfully delete a policy that has been removed from `.tyk.json`. The Dashboard would return an error reporting `invalid doc id` when the `sync` operation was triggered. Now Tyk Sync identifies the policy to be deleted and correctly deletes it from the Dashboard.
</Expandable>

### 2.1.3
##### Updated


<Expandable title='Upgraded to Golang 1.24'>
Tyk Sync now runs on Golang 1.24, bringing:

  - Improved build performance and runtime efficiency
  - Enhanced security with latest Go security patches
  - Better dependency management and module support
</Expandable>

### 2.1.2
##### Updated


<Expandable title='Updated to use latest kin-openapi'>
Upgraded to use the latest version of kin-openapi (v0.132.0). This ensures improved compatibility, full stack interoperability, and continued support for existing OpenAPI 3.0.x specifications.
</Expandable>

### 2.1.1
##### Fixed


<Expandable title='Fixed incorrect API-level rate limits from dump command'>
Fixed an issue where the Tyk Sync dump command incorrectly set API-level rate limits in a policy that did not have such limits. This problem arose from Sync trying to add a default value when no rate limit is set in the policy, which led to unintended rate limiting. The issue has been resolved by ensuring that Sync respects the original policy when no API-level rate limit is set.
</Expandable>
