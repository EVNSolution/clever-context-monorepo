# Clever-ODIC-deploy

## 목적

이 문서는 `Clever-ODIC-deploy` family entry다.

이 family는 CLEVER의 현재 deploy 정본을 template registry에서 재사용 가능한 형태로 올린 entry다.

핵심은 아래 네 가지다.

- app repo는 image build만 담당한다.
- central release가 실제 deploy를 수행한다.
- runtime inventory가 workload scope를 고정한다.
- public contract probe까지 통과해야 release를 닫는다.

## registry 요약

- `template_id`: `Clever-ODIC-deploy`
- `current_version`: `v1`
- `status`: `recommended`
- `use_case`: current CLEVER deploy baseline for monorepo and MSA
- `deploy_profile`: `image-build-once-central-release`
- `summary`: current CLEVER deploy truth elevated as a reusable template family
- `bootstrap_rules`: repo는 image artifact를 만들고, central release와 runtime inventory가 실제 deploy를 닫는다.
- `allowed_override_boundary`: image repository/tag policy, env category, secret source, external integration, rollout bundle 차이는 조정할 수 있다.
- `known_constraints`: 이 family는 current deploy model을 설명하는 문서형 baseline이며, release workflow 자체나 runtime inventory 자체를 직접 제공하지는 않는다.

## 적용 대상

이 family는 아래 두 경우를 같은 deploy model로 읽는다.

- single workload monorepo
- multiple workload MSA

차이는 workload cardinality뿐이다.

- monorepo: image 1개, workload 1개
- MSA: image 여러 개, workload 여러 개

둘 다 아래는 같다.

- immutable image artifact 기준
- central release ownership
- runtime inventory authority
- env/secret separation
- rollout / rollback proof
- public contract probe 필수

## current deploy baseline

이 family가 설명하는 canonical deploy model은 아래 한 줄이다.

`repo main build -> immutable image artifact -> central release resolve -> runtime inventory scoped deploy -> public contract probe -> release evidence`

이 문장은 아래 뜻으로 읽는다.

- 코드 저장소는 deploy entrypoint가 아니다.
- 코드 저장소는 build success까지만 닫는다.
- 실제 runtime change는 central release lane에서 닫는다.
- deploy scope는 runtime inventory에서 읽는다.
- release 완료 판단은 public contract probe까지 포함한다.

## 문서 역할

이 family entry의 역할은 아래다.

- current deploy baseline을 template lineage로 기록할 수 있게 한다.
- monorepo와 MSA를 서로 다른 deploy 방식으로 오해하지 않게 한다.
- reusable deploy assets가 어떤 모델을 전제로 하는지 고정한다.
- service metadata와 change-control 기록에서 같은 deploy profile을 재사용할 수 있게 한다.

## current authority 읽기 순서

이 family를 사용할 때는 아래 순서로 읽는다.

1. `docs/root/deploy-template-governance.md`
2. `docs/root/central-deploy-runtime-current-truth.md`
3. `docs/templates/Clever-ODIC-deploy/versions/v1.md`
4. `templates/deploy/README.md`
5. 해당 서비스 문서

## 채택 적합 조건

아래에 가까우면 이 family를 먼저 쓴다.

- repo는 image build를 소유하고 실제 release는 분리돼 있는 경우
- deploy scope를 runtime inventory로 고정하는 경우
- monorepo와 MSA를 같은 release model에서 운영하는 경우
- service 문서에 deploy lineage를 현재 정본 기준으로 남겨야 하는 경우

## template-only 원칙

이 family는 deploy model을 설명하는 template entry다.

아래는 의도적으로 넣지 않는다.

- business rule 예제
- 운영자용 상세 runbook
- release workflow 구현 파일
- 실제 secret, ARN, URL 값
- 서비스별 고유 rollout caveat

대신 아래는 분명히 넣는다.

- build / release / inventory 책임 분리
- workload cardinality 해석
- app-only change와 deploy-affecting change의 경계
- 공통 override boundary

## 버전 문서

- [`v1`](./versions/v1.md)
