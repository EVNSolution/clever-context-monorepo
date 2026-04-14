# 서비스 문서 탐색

## 목적

서비스별 문서를 빠르게 찾기 위한 안내 문서다.

## 위치

- 서비스 시작 기준은 [`docs/services/index.md`](../services/index.md)에 둔다.
- 서비스 정본 문서는 [`docs/services/<service-name>/`](../services/) 아래에 둔다.
- `clever-msa-platform` batch 스캔 기준 서비스 목록은 `development/service-*`다.

## MSA SaaS 복제형 접근

- MSA 기반 SaaS 복제나 고객사별 변형 작업이면 먼저 [`docs/root/msa-saas-replication-governance.md`](../root/msa-saas-replication-governance.md)를 읽는다.
- 템플릿 기반 신규 시작이나 lineage 확인이 필요하면 [`docs/root/template-harness-governance.md`](../root/template-harness-governance.md)와 [`docs/templates/index.md`](../templates/index.md)를 같이 읽는다.
- 공통 deploy baseline은 [`docs/root/deploy-template-governance.md`](../root/deploy-template-governance.md)를 먼저 본다.
- target service가 정해진 뒤에만 해당 service leaf 문서로 내려간다.
- service leaf 문서는 전역 규칙을 다시 쓰지 않고, 고객사별 variation과 운영 차이만 추가로 적는다.

## 분류별 링크

### registry 계열
- [`service-announcement-registry`](../services/service-announcement-registry/index.md)
- [`service-attendance-registry`](../services/service-attendance-registry/index.md)
- [`service-dispatch-registry`](../services/service-dispatch-registry/index.md)
- [`service-organization-registry`](../services/service-organization-registry/index.md)
- [`service-personnel-document-registry`](../services/service-personnel-document-registry/index.md)
- [`service-region-registry`](../services/service-region-registry/index.md)
- [`service-settlement-registry`](../services/service-settlement-registry/index.md)
- [`service-support-registry`](../services/service-support-registry/index.md)
- [`service-terminal-registry`](../services/service-terminal-registry/index.md)
- [`service-vehicle-registry`](../services/service-vehicle-registry/index.md)

### operations-view 계열
- [`service-dispatch-operations-view`](../services/service-dispatch-operations-view/index.md)
- [`service-driver-operations-view`](../services/service-driver-operations-view/index.md)
- [`service-settlement-operations-view`](../services/service-settlement-operations-view/index.md)
- [`service-vehicle-operations-view`](../services/service-vehicle-operations-view/index.md)

### hub / listener / dead-letter
- [`service-telemetry-dead-letter`](../services/service-telemetry-dead-letter/index.md)
- [`service-telemetry-hub`](../services/service-telemetry-hub/index.md)
- [`service-telemetry-listener`](../services/service-telemetry-listener/index.md)

### 기타 service runtime
- [`service-account-access`](../services/service-account-access/index.md)
- [`service-delivery-record`](../services/service-delivery-record/index.md)
- [`service-driver-profile`](../services/service-driver-profile/index.md)
- [`service-notification-hub`](../services/service-notification-hub/index.md)
- [`service-region-analytics`](../services/service-region-analytics/index.md)
- [`service-settlement-payroll`](../services/service-settlement-payroll/index.md)
- [`service-vehicle-assignment`](../services/service-vehicle-assignment/index.md)

## 사용 기준

- 이 문서는 링크 허브다.
- 서비스 규칙 판단은 실제 서비스 문서에서 한다.
- 새 서비스는 [`docs/services/service-template.md`](../services/service-template.md)를 기준으로 시작한다.
- template registry 판단은 [`docs/templates/index.md`](../templates/index.md)와 각 version 문서에서 한다.
