# clever-driver-app Context Pointer

## Purpose

This file records the canonical context pointer for the `clever-driver-app`
mobile runtime. It does not copy runtime proof, store evidence, secrets,
environment values, or release artifacts from the target repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `clever-driver-app` |
| owner_repo | <https://github.com/EVNSolution/clever-driver-app> |
| runtime_source | <https://github.com/EVNSolution/clever-driver-app/tree/dev> |
| product_scope | native iPhone/Android delivery driver app for invite-based access, driver identity, consent, assigned-route view, active-delivery location tracking, proof capture, offline retry, and route completion cleanup |
| current_mvp_anchor | <https://github.com/EVNSolution/clever-change-control/issues/145>, <https://github.com/EVNSolution/clever-driver-app/issues/72>, <https://github.com/EVNSolution/clever-driver-app/issues/73> |
| context_issue | <https://github.com/EVNSolution/clever-context-monorepo/issues/23> |
| target_runtime_docs | <https://github.com/EVNSolution/clever-driver-app/blob/dev/README.md> and <https://github.com/EVNSolution/clever-driver-app/tree/dev/docs> |
| related_backend | <https://github.com/EVNSolution/clever-route-server/tree/main/apps/delivery-api> |
| related_shopify_admin | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app> |
| platform_lineage | Expo / React Native native iOS and Android app bootstrap under CLEVER target-repo workflow |

## Interpretation

- The app is a native mobile runtime, not a PWA/web-first driver surface,
  because active delivery needs OS location permissions, foreground/background
  tracking behavior, secure token storage, camera/photo/barcode proof capture,
  and controlled distribution evidence.
- Invite-code verification and phone identity are app entry mechanisms, not a
  replacement for server-issued driver access and tenant/shop scoping.
- The phone-owned driver account name is global across shops. Each Shopify
  store's `Driver.displayName` is a merchant-scoped alias, and neither value is
  synchronized or backfilled into the other.
- The app owns driver-facing UX, native permission prompts, SecureStore token
  handling, app-side offline retry/discard behavior, and mobile release evidence
  runbooks. It does not own Shopify data, route assignment authority,
  proof-media metadata persistence, or object-storage credentials.
- Stop payment collection context is read-only in the app. The paired delivery
  API supplies the payment method, exact order total and currency, and
  normalized payment status; the app presents those values in stop details and
  active-route notifications without calculating totals or changing payment
  state.
- Route lists use the server lifecycle labels `Ready`, `In progress`, and
  `Completed`. Assignment does not advance status; the driver's explicit start
  and completion actions record the corresponding lifecycle events.
- The paired delivery API owns companies/shops, drivers, route assignments,
  consent records, driver events, proof-media metadata/storage contracts, and
  cleanup evidence. The driver app must use the server driver APIs rather than
  Shopify APIs directly.
- Account profile name updates and account deletion requests use the global
  `DriverAccount` bearer through the paired delivery API, never a Store route
  token. A deletion request is an account-scoped audit request, not immediate
  deletion of route, proof, or consent history.

## Do not store here

- App Store / Google Play screenshots, review answers, signing files, or private
  distribution evidence
- Driver phone numbers, route data, customer addresses, proof images, or logs
- Expo/EAS project IDs, Apple/Google credentials, API origins, or runtime env
  values copied from the target repository
- Per-PR status, build artifacts, or smoke evidence manifests
- Local filesystem paths or machine-specific URLs

## Source-of-truth pointers

- Target repository: <https://github.com/EVNSolution/clever-driver-app>
- Runtime source branch: <https://github.com/EVNSolution/clever-driver-app/tree/dev>
- Target repository README: <https://github.com/EVNSolution/clever-driver-app/blob/dev/README.md>
- Product brief and scenario plan: <https://github.com/EVNSolution/clever-driver-app/blob/dev/docs/project-brief.md>
- App-side API/runtime flow: <https://github.com/EVNSolution/clever-driver-app/blob/dev/docs/route-access-flow.md>
- Driver account authentication and profile contract: <https://github.com/EVNSolution/clever-route-server/blob/main/apps/delivery-api/docs/api/driver-auth.md>
- Release readiness and evidence gates: <https://github.com/EVNSolution/clever-driver-app/blob/dev/docs/release-readiness.md>
- Physical-device smoke runbook: <https://github.com/EVNSolution/clever-driver-app/blob/dev/docs/physical-device-smoke-runbook.md>
- Expo app identity config: <https://github.com/EVNSolution/clever-driver-app/blob/dev/app.json>
