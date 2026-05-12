# clever-driver-app Context Pointer

## Purpose

This file records the canonical context pointer for the `clever-driver-app`
mobile runtime. It does not copy runtime proof, store evidence, secrets,
environment values, or release artifacts from the target repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `clever-driver-app` |
| owner_repo | `EVNSolution/clever-driver-app` |
| product_scope | native iPhone/Android delivery driver app for route access, consent, assigned-route view, active-delivery location tracking, proof capture, offline retry, and route completion cleanup |
| current_mvp_anchor | `EVNSolution/clever-change-control#145`, `EVNSolution/clever-driver-app#72`, `EVNSolution/clever-driver-app#73` |
| context_issue | `EVNSolution/clever-context-monorepo#23` |
| target_runtime_docs | `EVNSolution/clever-driver-app:README.md`, `docs/project-brief.md`, `docs/route-access-flow.md`, `docs/release-readiness.md`, `docs/physical-device-smoke-runbook.md` |
| related_backend | `EVNSolution/clever-delivery-server`, especially driver route/proof-media API docs |
| platform_lineage | Expo / React Native native iOS and Android app bootstrap under CLEVER target-repo workflow |

## Interpretation

- The app is a native mobile runtime, not a PWA/web-first driver surface,
  because active delivery needs OS location permissions, foreground/background
  tracking behavior, secure token storage, camera/photo/barcode proof capture,
  and controlled distribution evidence.
- Route+phone lookup is only an access starting point. Phone number alone is not
  a global driver identity; company/route context and server-issued short-lived
  driver access are part of the boundary.
- The app owns driver-facing UX, native permission prompts, SecureStore token
  handling, app-side offline retry/discard behavior, and mobile release evidence
  runbooks. It does not own Shopify data, route assignment authority, proof-media
  metadata persistence, or object-storage credentials.
- `clever-delivery-server` owns companies/shops, drivers, route assignments,
  consent records, driver events, proof-media metadata/storage contracts, and
  cleanup evidence. The driver app must use the server driver APIs rather than
  Shopify APIs directly.
- Production release still requires target-repo evidence for physical iOS/Android
  device smoke, owner-controlled Expo/EAS/Apple/Google signing and distribution,
  owner/legal privacy disclosure approval, public license decision, and related
  delivery-server proof-media hardening evidence.

## Do not store here

- App Store / Google Play screenshots, review answers, signing files, or private
  distribution evidence
- Driver phone numbers, route data, customer addresses, proof images, or logs
- Expo/EAS project IDs, Apple/Google credentials, API origins, or runtime env
  values copied from the target repository
- Per-PR status, build artifacts, or smoke evidence manifests

## Source-of-truth pointers

- Target repository README: `EVNSolution/clever-driver-app:README.md`
- Product brief and scenario plan: `EVNSolution/clever-driver-app:docs/project-brief.md`
- App-side API/runtime flow: `EVNSolution/clever-driver-app:docs/route-access-flow.md`
- Release readiness and evidence gates: `EVNSolution/clever-driver-app:docs/release-readiness.md`
- Physical-device smoke runbook: `EVNSolution/clever-driver-app:docs/physical-device-smoke-runbook.md`
