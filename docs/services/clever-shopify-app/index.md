# clever-shopify-app Context Pointer

## Purpose

This file records the canonical context pointer for the CLEVER Shopify embedded
admin app runtime. It does not copy Shopify dashboard state, review evidence,
Admin API payloads, credentials, environment values, or release artifacts from
the target repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `clever-shopify-app` |
| owner_repo | <https://github.com/EVNSolution/shopify-clever> |
| runtime_source | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app> |
| product_scope | Shopify embedded admin app for CLEVER merchants: order intake, delivery-date filtering, route planning, route detail, driver invite/access, and operational navigation surfaces |
| naming_authority | <https://github.com/EVNSolution/shopify-clever/blob/main/NAMING.md> |
| paired_delivery_api | <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api> |
| related_mobile_runtime | <https://github.com/EVNSolution/clever-routes-app> |
| deploy_lineage | React Router Shopify embedded app, Prisma session storage, Shopify CLI config split for production/public and dev/custom-store runtimes |
| context_issue | <https://github.com/EVNSolution/clever-context-monorepo/issues/48> |
| change_control | <https://github.com/EVNSolution/clever-change-control/issues/265> |

## Interpretation

- The merchant-facing product name is `CLEVER`; Shopify handle, Admin path, and
  hosted app URL are separate identity fields and must be checked in the naming
  authority before release or dashboard changes.
- The Shopify app owns merchant/admin UX, Shopify authentication/session storage,
  order synchronization entry points, route planning UI, driver invite/admin
  actions, and App Store review-facing behavior.
- Shopify order and customer integrations are query-only for K-food and
  development stores. Operational corrections are written through the paired
  delivery API to the CLEVER database, not back to Shopify. App-owned
  `AppInstallation` metafields remain limited to application settings such as
  departure location and do not store order or customer corrections.
- The route admin surface shows every materialized child immediately and uses
  `Ready`, `In progress`, and `Completed` as the canonical execution labels;
  legacy pre-execution statuses are presented as `Ready`.
- Operational status uses independent Pills, one source and one claim per Pill.
  GPS, Device, Server, Sync, Gap, and Alert facts are not joined with middle dots
  or other punctuation separators. GPS proximity and device-local progress never
  imply server-confirmed completion, and unavailable evidence remains explicit.
- Current-position presentation keeps physical location, source freshness,
  device progress, and server-confirmed stop results separate. A material gap or
  completed route with unresolved results is an explicit operational warning,
  not a detail hidden behind an otherwise healthy lifecycle label.
- Shopify webhook routes own session-free HMAC admission at the public edge, but
  they do not become a second durable inbox. Durable replay, leasing, duplicate
  suppression, terminal tombstones, and reconciliation remain in the paired
  Delivery API.
- The embedded app may expose customer-notification settings and manual operator
  actions through the authenticated Delivery API boundary. It does not infer
  recipient data, bypass the canonical outbox, or treat queued facts as authority
  to activate automatic sending.
- `Driver.displayName` is a merchant/store-scoped operational alias. It is not
  the driver's phone-owned account name and must not update or backfill it.
- The Shopify app must not become the long-term source of driver mobile runtime
  state. Driver identity, route assignment authority, driver bearer auth,
  proof-media metadata, and mobile driver APIs belong to the paired delivery API.
- Public production and dev/custom-store runtimes are intentionally split in the
  target repository so routine development does not mutate the production app
  configuration by accident.
- Production, CLEVER Route, and K-food releases use the target repository's
  manual deploy workflow. Publication is transaction-like: backup and migration
  checks precede health promotion, current/previous pointers are published
  atomically, and a failed candidate restores the prior runnable release without
  claiming that application rollback reverses database changes.

## Do not store here

- Shopify Admin tokens, session tokens, API secrets, app secrets, installation
  tokens, store-specific private evidence, or dashboard payload snapshots
- Full GraphQL query inventories, API schemas, Prisma schema copies, env matrices,
  or App Store submission evidence
- Per-run deploy output, screenshots, screencasts, or review conversation logs
- Local filesystem paths or machine-specific URLs

## Source-of-truth pointers

- Target repository: <https://github.com/EVNSolution/shopify-clever>
- Monorepo README: <https://github.com/EVNSolution/shopify-clever/blob/main/README.md>
- Shopify app runtime source: <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app>
- Shopify app README: <https://github.com/EVNSolution/shopify-clever/blob/main/apps/shopify-app/README.md>
- Operational Pill grammar and alert hierarchy: <https://github.com/EVNSolution/shopify-clever/blob/main/DESIGN.md>
- Manual EC2 release and rollback runbook: <https://github.com/EVNSolution/shopify-clever/blob/main/docs/runbooks/ec2-shopify-release-rollback.md>
- One-server deployment ownership and runtime boundary: <https://github.com/EVNSolution/shopify-clever/blob/main/docs/deployment/clever-route-one-server-runtime-2026-05-16.md>
- K-food installation/deployment pointer: <https://github.com/EVNSolution/shopify-clever/blob/main/docs/deployment/kfood-cleverroute-dev-install.md>
- Naming guide: <https://github.com/EVNSolution/shopify-clever/blob/main/NAMING.md>
- Production Shopify config: <https://github.com/EVNSolution/shopify-clever/blob/main/apps/shopify-app/shopify.app.toml>
- Dev/custom Shopify config: <https://github.com/EVNSolution/shopify-clever/blob/main/apps/shopify-app/shopify.app.dev.toml>
- Protected customer data field map: <https://github.com/EVNSolution/shopify-clever/blob/main/docs/shopify-protected-customer-data-field-map.md>
