# clever-delivery-server Context Pointer

## Purpose

This file records the canonical context pointer for the CLEVER delivery API
runtime. It does not copy API payloads, runtime proof, secrets, environment
values, deployment evidence, or object-storage credentials from the target
repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `clever-delivery-server` |
| owner_repo | <https://github.com/EVNSolution/clever-route-server> |
| runtime_source | <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api> |
| product_scope | delivery data server and driver API for shops, orders, routes, drivers, invite/auth flows, account profiles, consent records, ordered driver-event evidence, synchronization health, operational alerts, customer-notification evidence, the durable Shopify webhook inbox, proof-media metadata/storage contracts, and retention cleanup |
| target_runtime_docs | <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/README.md> and <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api/docs> |
| deploy_lineage | Node/TypeScript Fastify service with Prisma/PostgreSQL, deployed as the delivery API runtime paired with the Shopify embedded app |
| related_shopify_admin | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app> |
| related_mobile_runtime | <https://github.com/EVNSolution/clever-routes-app> |
| context_issue | <https://github.com/EVNSolution/clever-context-monorepo/issues/48> |
| change_control | <https://github.com/EVNSolution/clever-change-control/issues/265> |

## Interpretation

- The server is the data and authorization source for the Shopify admin app and
  the driver app. The driver app must not call Shopify directly for route/order
  data.
- Admin-facing APIs cover delivery orders, route planning, route assignment,
  driver invite/access operations, and synchronization boundaries used by the
  Shopify embedded app.
- Shopify order and customer records are immutable upstream source data for
  K-food and development stores. Synchronization is one-way from Shopify into
  CLEVER, and operator corrections are stored only in the CLEVER database.
- Route execution starts at child creation: every created child is `Ready`
  without driver, stop, or publish prerequisites. Driver assignment is
  lifecycle-neutral; `ROUTE_STARTED` moves it to `In progress`, and
  `ROUTE_COMPLETED` moves it to `Completed`.
- Driver-facing APIs cover invite verification, bearer access, consent records,
  assigned-route reads, driver events, proof-media upload metadata/storage,
  scoped proof-media read access, scan rejection hooks, monitoring hooks, and
  retention cleanup evidence.
- An ordered Driver v2 attempt is durable evidence before business validation.
  Device-local queueing or progress is not server completion. Only a committed
  event or an `APPLIED` event receipt confirms the transition; unknown receipts
  remain reconciliation work, and assignment/version conflicts stop blind replay.
- Device heartbeat and queue counters are synchronization evidence, not route
  lifecycle authority. The server owns synchronization-health projection,
  mismatch detection, and operational alerts while keeping GPS, device progress,
  server-confirmed progress, and route lifecycle as independent facts.
- The Delivery API is the only durable Shopify webhook inbox. The Shopify edge
  authenticates session-free webhook requests and forwards the authenticated
  webhook body. The Delivery API alone minimizes the persisted replay envelope;
  terminal tombstones preserve duplicate suppression, while retryable, leased,
  failed, and dead-letter work is not removed by terminal retention.
- The Delivery API owns customer-notification facts, render/idempotency state,
  delivery attempts, retry leases, and provider-send evidence. Automatic sending
  remains disabled unless the separately governed consent, readiness,
  idempotency, retry, and kill-switch gates authorize it. Queued or overdue facts
  are evidence to reconcile, not permission to send.
- Operational logs and monitors use allowlisted identifiers, stable outcome codes,
  and aggregate counts. They exclude customer payloads, tokens, coordinates,
  free-form provider errors, and client network identity. CloudWatch retention and
  database evidence cleanup are separate controls; unresolved evidence remains.
- Ordered Driver v2 is additive: the compatible server contract and durable
  evidence schema roll out before Driver and admin consumers. A consumer rollback
  comes first; the server keeps legacy admission and additive evidence readable
  until adoption and the full observation-window gate authorize later removal.
- The global `DriverAccount` owns native Push installations. The server accepts
  installation registration and revocation only through account bearer access,
  while Store-specific driver references remain route-assignment metadata.
- Route Push is a non-authoritative notification channel. Successful route
  mutations may emit PII-free assignment, change, cancellation, or release
  signals, but delivery failure does not roll back the mutation and clients
  must refresh the server route contract before acting.
- The server owns the public CLEVER Routes direct-distribution boundary:
  `/routes-app` and `/routes-app/release/android` for current builds, with
  `/driver-app` compatibility for the legacy Android identity. The release
  manifest declares the canonical and replaced package IDs, while the backing
  package URL remains hidden behind the server-owned install/download flow.
- Package-identity migrations deploy the compatible server routes before
  publishing the replacement package as the latest required release. This
  preserves update discovery for installed legacy builds that cannot upgrade
  in place.
- Assigned-route reads provide the server-authoritative payment method, exact
  order total and currency, and normalized payment status required for cash or
  transfer collection decisions. The driver app consumes this context as
  read-only data and does not derive totals or mutate payment state.
- A driver's phone-owned account name is global across shops. A Shopify store's
  `Driver.displayName` is a separate merchant-scoped alias; the server does not
  synchronize or backfill either value into the other.
- CLEVER Routes Android release metadata uses a product-specific PostgreSQL
  artifact history and current pointer. Environment values are an empty-registry
  bootstrap fallback, not the steady-state release control plane.
- The server keeps stable `/routes-app` and legacy `/driver-app` public
  contracts while hiding the backing artifact URL. SSM invokes the publisher in
  the running container; it does not store release state and does not require a
  server restart for later APK promotions.
- Production rollout is a manual, exact-CI-gated SSM operation with
  digest-addressed current and rollback image pointers. Runtime rollback restores
  the previous image selection; it is not a database rollback, so schema-affecting
  releases require an explicit compatibility or restore/forward-fix decision.
- Physical Driver validation, client adoption, the seven-day full-invariant
  observation window, email reconciliation authorization, and alert-notification
  SNS owner/subscription approval remain release/change-control gates. Their
  current evidence belongs in the linked change-control issue rather than this
  context pointer.
- Detailed API contracts, database schema, exact runtime environment variables,
  secret categories, object-storage provider settings, deployment commands, and
  proof/evidence records remain in the target repository and change-control.

## Do not store here

- Shopify Admin tokens, JWT secrets, database credentials, object-storage access
  keys, session tokens, bucket names used in production, or connection strings
- Full API schemas, Prisma schema copies, route inventories, or env matrices
- Proof images, proof-media storage keys, scanner logs, cleanup job logs, or
  production evidence manifests
- Per-run deploy, backup, scheduler, scanner, or signed URL smoke evidence
- Local filesystem paths or machine-specific URLs

## Source-of-truth pointers

- Target repository: <https://github.com/EVNSolution/clever-route-server>
- Delivery API runtime source: <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api>
- Delivery API README: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/README.md>
- Service project brief: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/project-brief.md>
- API documentation strategy: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/README.md>
- OpenAPI contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/openapi.yaml>
- Driver authentication and account profile contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-auth.md>
- Admin route plans contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/admin-route-plans.md>
- Driver route access contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-route-access.md>
- Driver assigned route contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-assigned-route.md>
- Driver proof-media API and storage contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-proof-media.md>
- Ordered driver-event admission, evidence, and receipt contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-event-contract-v2.md>
- Shopify webhook replay, tombstone, and retention contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/shopify-webhook-retention.md>
- Customer-notification outbox contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/admin-route-plans.md>
- Customer-notification ownership and activation boundary: <https://github.com/EVNSolution/clever-route-server/blob/main/docs/adr/2026-08-05-customer-notification-settings-ownership.md>
- Synchronization evidence, CloudWatch, and retention runbook: <https://github.com/EVNSolution/clever-route-server/blob/main/docs/observability/driver-event-contract-cloudwatch.md>
- Production health, invariant, alert, and email-outbox monitor runbook: <https://github.com/EVNSolution/clever-route-server/blob/main/docs/observability/dsv-g007-operations.md>
- CI/deploy validation boundary: <https://github.com/EVNSolution/clever-route-server/blob/main/docs/deployment/route-ops-ci-deploy-validation.md>
- Manual SSM deploy and rollback runbook: <https://github.com/EVNSolution/clever-route-server/blob/main/docs/deployment/route-ops-simple-ssm-deploy.md>
- Location/proof data handling: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/compliance/location-data-handling.md>
- EC2/EBS deployment notes: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/deployment/ec2-ebs.md>
