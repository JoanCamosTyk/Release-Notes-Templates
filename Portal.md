## Intructions
I am going to copy paste all the Highlights and Change Logs from previous releases. It is important that you understand the amount of information we usually provide from tickets and new features and also see patterns on how we like to communicate information to users

## Release Highlights

### 1.17.3
This release restores the environment variable behaviour that changed in 1.17.1 and 1.17.2, where some `PORTAL_*` variables were no longer recognised. Most notably, `PORTAL_DISABLE_CSRF_CHECK` is honoured again, resolving the "bad request" error customers hit at login. Every legacy `PORTAL_*` variable now works exactly as it did in 1.17.0, and Portal also gains a new `TYK_PORTAL_` prefix with a consistent naming style that matches the convention used across other Tyk components.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.17.3) below.

## Change log

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
