# 서비스 문서 시작점

## 목적

이 문서는 `docs/services` 아래 문서를 시작하는 기준점이다.

## 초기 서비스 목록

- `clever-msa-platform/development/service-*` child repo를 기준으로 batch 반영했다.
- target service가 추가되면 같은 규칙으로 문서를 확장한다.

## 등록된 서비스

### registry 계열
- [`service-announcement-registry`](./service-announcement-registry/index.md)
- [`service-attendance-registry`](./service-attendance-registry/index.md)
- [`service-dispatch-registry`](./service-dispatch-registry/index.md)
- [`service-organization-registry`](./service-organization-registry/index.md)
- [`service-personnel-document-registry`](./service-personnel-document-registry/index.md)
- [`service-region-registry`](./service-region-registry/index.md)
- [`service-settlement-registry`](./service-settlement-registry/index.md)
- [`service-support-registry`](./service-support-registry/index.md)
- [`service-terminal-registry`](./service-terminal-registry/index.md)
- [`service-vehicle-registry`](./service-vehicle-registry/index.md)

### operations-view 계열
- [`service-dispatch-operations-view`](./service-dispatch-operations-view/index.md)
- [`service-driver-operations-view`](./service-driver-operations-view/index.md)
- [`service-settlement-operations-view`](./service-settlement-operations-view/index.md)
- [`service-vehicle-operations-view`](./service-vehicle-operations-view/index.md)

### hub / listener / dead-letter
- [`service-telemetry-dead-letter`](./service-telemetry-dead-letter/index.md)
- [`service-telemetry-hub`](./service-telemetry-hub/index.md)
- [`service-telemetry-listener`](./service-telemetry-listener/index.md)

### 기타 service runtime
- [`service-account-access`](./service-account-access/index.md)
- [`service-delivery-record`](./service-delivery-record/index.md)
- [`service-driver-profile`](./service-driver-profile/index.md)
- [`service-notification-hub`](./service-notification-hub/index.md)
- [`service-region-analytics`](./service-region-analytics/index.md)
- [`service-settlement-payroll`](./service-settlement-payroll/index.md)
- [`service-vehicle-assignment`](./service-vehicle-assignment/index.md)

## 서비스명 규칙

- 서비스 디렉터리명은 소문자 kebab-case를 사용한다.
- 문서와 이슈, PR에서는 같은 서비스 키를 그대로 재사용한다.
- 예) `billing-api`, `admin-web`, `settlement-worker`

## 서비스 문서 최소 구조

- 각 서비스는 `docs/services/<service-name>/index.md`를 시작 문서로 둔다.
- 필요하면 같은 디렉터리에 API, 의존성, 운영 메모를 추가한다.
- 공통 규칙은 root와 contracts를 참조하고, 서비스 고유 규칙만 서비스 문서에 둔다.
- template lineage와 deploy profile은 service 문서에 남기되, 전역 원칙은 `docs/root/template-harness-governance.md`와 `docs/root/deploy-template-governance.md`를 참조한다.

## 시작 템플릿

- 새 서비스 문서는 [`docs/services/service-template.md`](./service-template.md)를 복사해서 시작한다.
- 템플릿 후보 자체는 [`docs/templates/index.md`](../templates/index.md)에서 고른다.
