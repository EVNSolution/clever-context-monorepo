# 템플릿 하네스 거버넌스

## 목적

이 문서는 template registry와 lineage 선택 규칙만 고정한다.

template registry는 완료된 template entry만 담는다. 작업 중 scaffold 계획,
runtime proof, runtime slice별 채택 결과, target repo 문서 요약은 이 repo에 두지 않는다.

## Template Entry 최소 필드

- `template_id`
- `version`
- `status`
- `use_case`
- `summary`
- `deploy_profile`
- `allowed_override_boundary`
- `known_constraints`

## 상태

- `recommended`: 신규 선택 기본 후보
- `candidate`: 검토 가능한 후보
- `legacy`: 기존 유지보수 목적의 지원 대상
- `deprecated`: 신규 기본 후보가 아님

## 선택 규칙

- 신규 시작에서는 registry entry를 후보로 제시한다.
- 유지보수에서는 소유 repo의 정본 문서에 남은 lineage를 먼저 확인한다.
- lineage 결과를 이 repo에 복제하지 않는다.
- template migration은 change-control에서 추적한다.

## 문서 위치 규칙

- 이 repo: 완료된 template entry와 선택 규칙
- 소유 repo: runtime slice별 채택 결과와 concrete override
- change-control: 선택 승인, migration trace, rollout evidence linkage

## 관련 문서

- [`docs/templates/index.md`](../templates/index.md)
- [`docs/root/deploy-template-governance.md`](./deploy-template-governance.md)
