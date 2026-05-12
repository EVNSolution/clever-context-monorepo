# clever-delivery-server Context Pointer

## Purpose

This file records the canonical context pointer for the `clever-delivery-server`
backend runtime. It does not copy API payloads, runtime proof, secrets,
environment values, deployment evidence, or object-storage credentials from the
target repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `clever-delivery-server` |
| owner_repo | `EVNSolution/clever-delivery-server` |
| product_scope | Shopify companion delivery data server and driver API for shops, orders, routes, drivers, consent records, driver events, proof-media metadata/storage contracts, and retention cleanup |
| current_mvp_anchor | `EVNSolution/clever-change-control#99`, `EVNSolution/clever-delivery-server#71` |
| context_issue | `EVNSolution/clever-context-monorepo#23` for driver-app release boundary linkage |
| target_runtime_docs | `EVNSolution/clever-delivery-server:README.md`, `docs/project-brief.md`, `docs/api/driver-proof-media.md`, `docs/compliance/location-data-handling.md`, `docs/deployment/ec2-ebs.md` |
| deploy_lineage | Node/TypeScript Fastify service; EC2 + EBS PostgreSQL first, RDS/S3 path as scale or proof-media requirements demand |
| related_mobile_runtime | `EVNSolution/clever-driver-app` |

## Interpretation

- The server is the data and authorization source for the driver app. The driver
  app must not call Shopify directly for route/order data.
- Driver-facing APIs currently cover route+phone access, consent records,
  assigned-route reads, driver events, proof-media upload metadata/storage, scoped
  short-lived proof-media read access, scanner rejection hooks, scan monitoring
  hooks, and retention cleanup run evidence.
- Detailed API contracts, database schema, exact runtime environment variables,
  secret categories, object-storage provider settings, deployment commands, and
  proof/evidence records remain in the target repository and change-control.
- Target-repo docs now include an opt-in S3-compatible proof-media storage backend
  with SigV4 PUT/DELETE signing and presigned read access. Production bucket/IAM
  approval, credential custody, live signed URL smoke, scanner deployment,
  scanner alerting, cleanup scheduler deployment, and private evidence storage
  remain target-repo/change-control gates.

## Do not store here

- Shopify Admin tokens, JWT secrets, database credentials, S3 access keys,
  session tokens, bucket names used in production, or connection strings
- Full API schemas, Prisma schema copies, route inventories, or env matrices
- Proof images, proof-media storage keys, scanner logs, cleanup job logs, or
  production evidence manifests
- Per-run deploy, backup, scheduler, scanner, or signed URL smoke evidence

## Source-of-truth pointers

- Target repository README: `EVNSolution/clever-delivery-server:README.md`
- Service project brief: `EVNSolution/clever-delivery-server:docs/project-brief.md`
- Driver proof-media API and storage contract: `EVNSolution/clever-delivery-server:docs/api/driver-proof-media.md`
- Location/proof data handling: `EVNSolution/clever-delivery-server:docs/compliance/location-data-handling.md`
- EC2/EBS deployment and proof-media scheduler notes: `EVNSolution/clever-delivery-server:docs/deployment/ec2-ebs.md`
