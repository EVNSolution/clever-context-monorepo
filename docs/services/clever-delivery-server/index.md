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
| product_scope | delivery data server and driver API for shops, orders, routes, drivers, invite/auth flows, account profiles, consent records, driver events, proof-media metadata/storage contracts, and retention cleanup |
| target_runtime_docs | <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/README.md> and <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api/docs> |
| deploy_lineage | Node/TypeScript Fastify service with Prisma/PostgreSQL, deployed as the delivery API runtime paired with the Shopify embedded app |
| related_shopify_admin | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app> |
| related_mobile_runtime | <https://github.com/EVNSolution/clever-driver-app> |

## Interpretation

- The server is the data and authorization source for the Shopify admin app and
  the driver app. The driver app must not call Shopify directly for route/order
  data.
- Admin-facing APIs cover delivery orders, route planning, route assignment,
  driver invite/access operations, and synchronization boundaries used by the
  Shopify embedded app.
- Route execution starts at child creation: every created child is `Ready`
  without driver, stop, or publish prerequisites. Driver assignment is
  lifecycle-neutral; `ROUTE_STARTED` moves it to `In progress`, and
  `ROUTE_COMPLETED` moves it to `Completed`.
- Driver-facing APIs cover invite verification, bearer access, consent records,
  assigned-route reads, driver events, proof-media upload metadata/storage,
  scoped proof-media read access, scan rejection hooks, monitoring hooks, and
  retention cleanup evidence.
- A driver's phone-owned account name is global across shops. A Shopify store's
  `Driver.displayName` is a separate merchant-scoped alias; the server does not
  synchronize or backfill either value into the other.
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
- Location/proof data handling: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/compliance/location-data-handling.md>
- EC2/EBS deployment notes: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/deployment/ec2-ebs.md>
