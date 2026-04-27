# Monorepo Write Pointers

## 목적

이 문서는 사용자가 명시적으로 요청한 경우에만 외부 monorepo에 작성해야 할
issue-backed 항목을 짧게 남긴다.

이 문서는 target repo 문서의 정본이 아니다. 실제 내용과 완료 판단은 target
repo issue, PR, 문서를 따른다.

## Current Open Pointer

### EVNSolution/thundercrew-platform#12

- title: `contracts: define MVP OpenAPI schemas and mock examples`
- target monorepo: `EVNSolution/thundercrew-platform`
- status: open
- source issue: `https://github.com/EVNSolution/thundercrew-platform/issues/12`

Target repo에서 작성할 파일:

- `contracts/openapi/vehicle-tms.yaml`
- `contracts/openapi/field-events.yaml`
- `contracts/openapi/bss-ops.yaml`
- `contracts/openapi/delivery-insights.yaml`
- `contracts/openapi/rider-benefits.yaml`
- `contracts/openapi/ops-view.yaml`
- 필요 시 `docs/contract-placeholders.md`

작성 범주:

- MVP 화면 흐름 기준 최소 request/response schema
- validation/auth/conflict/provider unavailable error placeholder
- command request와 rider event submission idempotency 기준
- Admin Web/Rider App smoke용 mock response example
- PR의 `clever-context-monorepo` 반영 필요 여부 기록

이 repo에 복제하지 않을 것:

- provider-specific raw payload
- credential 또는 secret
- 실제 인증/권한 정책 확정
- persistence schema
- Admin Web/Rider App 구현
- Spring service endpoint 구현

## Issue-backed 후속 후보

아래 항목은 `thundercrew-platform:docs/project-brief.md`의 다음 작업 목록에 있지만,
현재 별도 open issue로 확인되지는 않았다. 추적하려면 target repo issue를 먼저 연다.

- provider adapter interface와 mock 전환 기준 문서화
- 라이더-차량 배정 이력의 최소 필드 확정
- Admin Web과 Rider App의 첫 smoke 화면을 API mock과 연결
