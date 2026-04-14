# 템플릿 하네스 거버넌스

## 목적

이 문서는 CLEVER가 외부 또는 내부 프로젝트 템플릿을 채택할 때 따르는 공통 하네스 규칙의 정본이다.

이 문서는 아래를 고정한다.

- 에이전트가 템플릿 선택지를 항상 사용자에게 보여주는 규칙
- 템플릿 registry 구조와 상태 기준
- 서비스 유지보수 시 기존 template lineage를 기본값으로 쓰는 규칙
- 템플릿 전환을 일반 수정이 아니라 `migration` 성격으로 다루는 규칙

## 기본 원칙

- 템플릿 선택은 에이전트가 추론으로 고정하지 않는다.
- 신규 개발과 유지보수 모두에서 템플릿 후보를 항상 사용자에게 제시한다.
- 유지보수에서는 기존 서비스 문서에 기록된 template lineage를 기본 추천으로 사용한다.
- 선택 결과는 대화에만 남기지 않고 service 문서와 change-control 기록에도 남긴다.
- 외부 템플릿은 바로 내부 복사하지 않고, 우선 registry entry와 채택 규칙으로 편입한다.

## 템플릿 상태

registry의 템플릿은 아래 상태 중 하나를 가진다.

- `recommended`: 기본 추천 후보
- `legacy`: 기존 서비스 유지 목적의 지원 대상
- `deprecated`: 신규 선택 기본안으로는 권장하지 않지만, 필요 시 명시적 선택은 허용

## registry 최소 구조

각 템플릿은 최소 아래 정보를 가진다.

- `template_id`
- `version`
- `status`
- `use_case`
- `summary`
- `bootstrap_rules`
- `deploy_profile`
- `allowed_override_boundary`
- `known_constraints`

registry entry는 `docs/templates/<template-id>/` 아래에 둔다.

## 사용자 선택 규칙

에이전트는 템플릿 선택 질문에서 최소 아래를 보여준다.

- `template_id@version`
- `status`
- 한 줄 목적
- 배포 방식 요약
- 현재 작업에 적합한 이유

기본 추천은 아래 기준을 따른다.

- 신규 개발: `recommended` 템플릿 중 가장 적합한 후보
- 유지보수: 서비스 문서에 기록된 기존 `template_id`와 `template_version`

다만 최종 선택은 항상 사용자가 한다.

## 유지보수와 lineage 기준

유지보수의 정본은 대화 기록이 아니라 서비스 문서 메타다.

서비스 문서는 최소 아래 메타를 가져야 한다.

- `template_id`
- `template_version`
- `deploy_profile`
- `override_scope`
- `lifecycle_state`

여기서 `lifecycle_state`는 서비스나 lineage의 현재 상태를 뜻한다.

- 예: `active`, `legacy`, `deprecated`, `retiring`

반면 bootstrap packet과 change-control 기록에 남기는 `lifecycle_action`은 이번 작업에서 수행하려는 변화 종류를 뜻한다.

- 예: `adopt`, `modify`, `migrate`, `retire`

이 메타는 유지보수 에이전트가 아래를 판단하는 기준이다.

- 기존 템플릿 유지
- 같은 템플릿 안의 수정/변경
- 템플릿 migration
- retire 대상 여부

## 예외 처리

### lineage unknown

기존 서비스 문서에 template lineage가 없으면 `lineage unknown`으로 본다.

이 경우에도 템플릿 선택지는 다시 제시하고, 이번 작업부터 메타를 기록한다.

### unregistered template candidate

사용자가 registry에 없는 템플릿을 원하면 차단하지 않는다.

대신 아래처럼 기록한다.

- `unregistered template candidate`
- 추후 registry 편입 검토 필요

### deprecated 템플릿

`deprecated` 템플릿은 경고를 준 뒤 계속 진행할 수 있다.

단 신규 기본안으로는 추천하지 않는다.

## migration 규칙

아래 경우는 일반 수정으로 흡수하지 않는다.

- 기존 템플릿과 다른 템플릿으로 전환
- 같은 템플릿이지만 다른 major version으로 전환
- 기존 deploy profile이 크게 달라지는 전환

이 경우는 `lifecycle_action=migrate`로 기록하고, change-control에도 별도 변화로 남긴다.

## 문서 배치 규칙

- 전역 원칙은 `docs/root`에 둔다.
- 템플릿 registry는 `docs/templates`에 둔다.
- 서비스별 lineage와 override는 `docs/services/<service-name>/index.md`에 둔다.
- 빠른 탐색 링크는 `docs/wiki`에만 둔다.

## 관련 문서

- `docs/root/deploy-template-governance.md`
- `docs/root/msa-saas-replication-governance.md`
- `docs/services/service-template.md`
- `docs/templates/index.md`
