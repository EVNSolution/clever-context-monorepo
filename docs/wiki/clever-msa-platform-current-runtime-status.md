# CLEVER MSA Platform 현재 운영 상태 (2026-04-20)

이 문서는 `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform` 기준의 최신 플랫폼 운영 상태를
빠르게 전달하기 위한 문맥 정리이다.

## 기준 문서

- `clever-msa-platform/WORKSPACE.md`
- `clever-msa-platform/repo-map.md`
- `clever-msa-platform/docs/mappings/current-runtime-inventory.md`
- `clever-msa-platform/docs/boundaries/README.md`
- `clever-msa-platform/docs/contracts/README.md`
- `clever-msa-platform/docs/contracts/09-integration-rules.md`

## 핵심 원칙(요약)

- 플랫폼 루트는 구현 코드를 두지 않고 `docs/` 정본 + linked child repo visibility를 관리한다.
- 서비스 런타임 소유권은 child repo에서 유지하고, 루트는 경계/매핑/문서 진실(source-of-truth) 중심으로 관리한다.
- cross-service direct DB join/수정은 금지하고, write는 정본 서비스로만 수행한다.
- route 변경이나 naming drift는 먼저 `docs/mappings/current-runtime-inventory.md`를 갱신한다.

## 현재 linked child 상태

### 플랫폼 runtime owner

- `runtime-prod-release` (active): OIDC/SSM 기반 rollout control plane
- `runtime-prod-platform` (active): host shape + canonical inventory owner

### Edge / Front

- `edge-api-gateway` (active)
- `front-web-console` (active)

### active service 목록 (모두 `migrated-target`)

- `service-organization-registry`
- `service-account-access`
- `service-driver-profile`
- `service-personnel-document-registry`
- `service-attendance-registry`
- `service-delivery-record`
- `service-dispatch-registry`
- `service-dispatch-operations-view`
- `service-region-registry`
- `service-region-analytics`
- `service-announcement-registry`
- `service-support-registry`
- `service-notification-hub`
- `service-vehicle-registry`
- `service-vehicle-assignment`
- `service-vehicle-operations-view`
- `service-driver-operations-view`
- `service-terminal-registry`
- `service-telemetry-hub`
- `service-telemetry-listener`
- `service-telemetry-dead-letter`
- `service-settlement-registry`
- `service-settlement-payroll`
- `service-settlement-operations-view`

총 23개 service + front + 2 runtime + edge = **26개 active linked target**으로 관리됨.

## 현재 Runtime Inventory 스냅샷

| Compose service | Target repo | Gateway prefix |
|---|---|---|
| gateway | `edge-api-gateway` | `external entrypoint` |
| web-console | `front-web-console` | `/` |
| account-auth-api | `service-account-access` | `/api/auth/` |
| organization-master-api | `service-organization-registry` | `/api/org/` |
| driver-profile-api | `service-driver-profile` | `/api/drivers/` |
| personnel-document-registry-api | `service-personnel-document-registry` | `/api/personnel-documents/` |
| attendance-registry-api | `service-attendance-registry` | `/api/attendance/` |
| delivery-record-api | `service-delivery-record` | `/api/delivery-record/` |
| settlement-payroll-api | `service-settlement-payroll` | `/api/settlements/` |
| settlement-registry-api | `service-settlement-registry` | `/api/settlement-registry/` |
| settlement-ops-api | `service-settlement-operations-view` | `/api/settlement-ops/` |
| driver-ops-api | `service-driver-operations-view` | `/api/driver-ops/` |
| vehicle-asset-api | `service-vehicle-registry` | `/api/vehicles/` |
| driver-vehicle-assignment-api | `service-vehicle-assignment` | `/api/driver-vehicle-assignments/` |
| vehicle-ops-api | `service-vehicle-operations-view` | `/api/vehicle-ops/` |
| dispatch-registry-api | `service-dispatch-registry` | `/api/dispatch/` |
| dispatch-ops-api | `service-dispatch-operations-view` | `/api/dispatch-ops/` |
| region-registry-api | `service-region-registry` | `/api/regions/` |
| region-analytics-api | `service-region-analytics` | `/api/region-analytics/` |
| announcement-registry-api | `service-announcement-registry` | `/api/announcements/` |
| support-registry-api | `service-support-registry` | `/api/ticket/` |
| notification-hub-api | `service-notification-hub` | `/api/notifications/` |
| terminal-registry-api | `service-terminal-registry` | `/api/terminals/` |
| telemetry-hub-api | `service-telemetry-hub` | `/api/telemetry/` |
| telemetry-dead-letter-api | `service-telemetry-dead-letter` | `/api/telemetry-dead-letters/` |
| telemetry-listener | `service-telemetry-listener` | `internal-only` |

`service-telemetry-listener`는 `desired=0` 상태이므로 broker 준비 전 production 비활성이다.

## 참고할 운영 노트

- gateway/service 상태/권한 규칙: `9-integration-rules` (`clever-msa-platform/docs/contracts/09-integration-rules.md`)
- ID/state 사전: `06-id-and-state-dictionary.md` (`clever-msa-platform/docs/contracts/06-id-and-state-dictionary.md`)
- 경계/데이터 소유: `docs/boundaries` (`clever-msa-platform/docs/boundaries`)
