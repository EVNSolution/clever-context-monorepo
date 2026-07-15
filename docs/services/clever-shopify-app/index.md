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
| related_mobile_runtime | <https://github.com/EVNSolution/clever-driver-app> |
| deploy_lineage | React Router Shopify embedded app, Prisma session storage, Shopify CLI config split for production/public and dev/custom-store runtimes |

## Interpretation

- The merchant-facing product name is `CLEVER`; Shopify handle, Admin path, and
  hosted app URL are separate identity fields and must be checked in the naming
  authority before release or dashboard changes.
- The Shopify app owns merchant/admin UX, Shopify authentication/session storage,
  order synchronization entry points, route planning UI, driver invite/admin
  actions, and App Store review-facing behavior.
- `Driver.displayName` is a merchant/store-scoped operational alias. It is not
  the driver's phone-owned account name and must not update or backfill it.
- The Shopify app must not become the long-term source of driver mobile runtime
  state. Driver identity, route assignment authority, driver bearer auth,
  proof-media metadata, and mobile driver APIs belong to the paired delivery API.
- Public production and dev/custom-store runtimes are intentionally split in the
  target repository so routine development does not mutate the production app
  configuration by accident.

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
- Naming guide: <https://github.com/EVNSolution/shopify-clever/blob/main/NAMING.md>
- Production Shopify config: <https://github.com/EVNSolution/shopify-clever/blob/main/apps/shopify-app/shopify.app.toml>
- Dev/custom Shopify config: <https://github.com/EVNSolution/shopify-clever/blob/main/apps/shopify-app/shopify.app.dev.toml>
- App Store asset packet entry: <https://github.com/EVNSolution/shopify-clever/blob/main/docs/shopify-app-store-assets/README.md>
