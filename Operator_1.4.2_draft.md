## Operator 1.4.2 Release Notes (DRAFT)

## Release Highlights

### 1.4.2
This release makes SecurityPolicy reconciliation reliable under load, keeping Kubernetes and Dashboard state consistent and preventing the reconciliation loops, orphaned Dashboard policies, and connection leaks that could occur when policies referenced many APIs.

For a comprehensive list of changes, please refer to the detailed [changelog](#Changelog-v1.4.2) below.

## Change log

### 1.4.2
#### Changelog
<a id="Changelog-v1.4.2" data-scroll-offset></a>

##### Fixed

<AccordionGroup>

<Accordion title='Fix SecurityPolicy reconciliation race conditions and connection leaks under load'>
Resolved a set of related issues that prevented `SecurityPolicy` resources from reconciling reliably, particularly under high load or when a policy referenced a large number of APIs. Affected Operators could enter continuous reconciliation loops (most commonly with policies referencing nine or more APIs), leave a policy in the Dashboard after its Kubernetes resource had been deleted, repeatedly try to recreate a policy and fail with a "policy name already used" error, and steadily leak HTTP connections and file descriptors until the host exhausted its available ports.

SecurityPolicy reconciliation is now idempotent and converges reliably: Kubernetes and Dashboard state stay consistent across create, update, and delete, the Operator recovers cleanly from partial failures, and HTTP connections are reused correctly so file descriptors no longer accumulate under sustained load. The same reconciliation improvements have been applied to Tyk OAS API definition resources.

This release also adds a new `TYK_OPERATOR_HTTP_MAX_IDLE_CONNS_PER_HOST` environment variable to tune the size of the Operator's HTTP connection pool to the Dashboard. It defaults to `100`, and can be increased for high-throughput clusters or reduced for resource-constrained environments.
</Accordion>

</AccordionGroup>
