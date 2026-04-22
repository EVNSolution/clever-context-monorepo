# Clever-ODIC-deploy Design

## 목적

이 문서는 `Clever-ODIC-deploy`를 `clever-context-monorepo` 안의 정식 deploy template family로 추가하는 설계를 고정한다.

이 설계는 새 배포 저장소를 만드는 것이 아니라, 현재 최신 배포 정본을 template registry, root governance, reusable asset 구조로 승격하는 것을 목표로 한다.

## 배경

현재 CLEVER의 deploy 관련 문서는 두 갈래가 섞여 있다.

- template registry와 root governance는 `clever-context-monorepo`에 있다.
- 실제 최신 runtime truth는 `image build once + central release + runtime inventory` 모델에 수렴해 있다.

문제는 이 최신 모델이 template family로 명시돼 있지 않아, monorepo와 MSA를 가로지르는 공통 deploy 채택 기준으로 재사용하기 어렵다는 점이다.

또한 legacy CDK/ECS 계열 문서와 현재 release/inventory 중심 모델이 같은 층위에서 읽힐 수 있어, 신규 채택 기준이 흐려질 수 있다.

## 설계 목표

- `Clever-ODIC-deploy`를 정식 template family로 registry에 등록한다.
- 최신 배포 정본을 아래 모델로 고정한다.
  - app repo는 image build만 담당
  - central release lane이 실제 deploy를 담당
  - runtime inventory가 deploy 대상과 workload 단위를 고정한다
- monorepo와 MSA를 서로 다른 deploy 방식으로 다루지 않는다.
  - `monorepo`는 single workload
  - `MSA`는 multiple workloads
  - 둘 다 같은 release/inventory/contract probe 모델을 따른다
- reusable deploy asset을 위 모델 기준으로 다시 설명한다.
- legacy CDK/ECS 계열은 current authority가 아님을 명시한다.

## 비목표

- 실제 release control repo 또는 runtime inventory repo를 새로 만들지 않는다.
- 배포 스크립트나 GitHub Actions workflow를 이 문서에서 직접 제공하지 않는다.
- service-specific rollout runbook을 template family 안으로 흡수하지 않는다.
- legacy ECS/CDK 기반 profile을 공존 가능한 최신 profile로 유지하지 않는다.

## 채택 모델

`Clever-ODIC-deploy`의 canonical deploy model은 아래 한 줄이다.

`repo main build -> immutable image artifact -> central release resolve -> runtime inventory scoped deploy -> public contract probe -> release evidence`

이 모델의 의미는 아래와 같다.

- 코드 저장소는 deploy entrypoint가 아니다.
- 코드 저장소는 immutable image artifact 생성 책임만 가진다.
- 실제 production or shared runtime rollout은 central release lane이 수행한다.
- 어떤 workload를 어떤 단위로 배포할지는 runtime inventory가 정한다.
- 배포 완료 판단은 public contract probe까지 통과해야 닫힌다.

## monorepo / MSA 해석

### monorepo

- repo 수: 1
- image 수: 1
- workload 수: 1
- 기본 해석: single workload deploy

monorepo는 애플리케이션 구조가 단일 저장소라는 뜻일 뿐, deploy baseline 자체가 달라지지는 않는다.

즉 monorepo도 아래를 그대로 따른다.

- repo에서 image를 한 번 빌드한다
- central release가 그 image를 선택한다
- runtime inventory에는 workload 하나가 등록된다
- public contract probe로 완료를 닫는다

### MSA

- repo 수: 여러 개일 수 있다
- image 수: 여러 개일 수 있다
- workload 수: 여러 개
- 기본 해석: multiple workload deploy

MSA도 deploy model은 바뀌지 않는다.

- 각 workload repo는 자기 image를 만든다
- central release가 release 대상 bundle을 고른다
- runtime inventory가 workload class와 deploy scope를 고정한다
- workload별 또는 bundle 기준으로 public contract probe를 남긴다

### 공통 규칙

monorepo와 MSA의 차이는 artifact cardinality와 workload cardinality에만 있다.

아래는 동일하다.

- immutable artifact 기준
- central release ownership
- runtime inventory authority
- env/secret separation
- rollout / rollback proof
- public contract probe 필수

## 문서 구조

이번 표준 범위에서 바뀌는 문서 구조는 아래다.

### 새로 추가

- `docs/templates/Clever-ODIC-deploy/index.md`
- `docs/templates/Clever-ODIC-deploy/versions/v1.md`

### 갱신

- `docs/templates/index.md`
- `docs/root/deploy-template-governance.md`
- `templates/deploy/README.md`
- `templates/deploy/checklist.md`
- `templates/deploy/env-template.example`
- `templates/deploy/override-guide.md`

## 문서별 책임

### `docs/templates/Clever-ODIC-deploy/index.md`

template family entry 역할을 가진다.

최소 아래를 설명한다.

- 템플릿 목적
- 사용 대상
- monorepo / MSA 해석
- deploy profile 요약
- current authority 링크
- version 문서 링크

### `docs/templates/Clever-ODIC-deploy/versions/v1.md`

`v1`의 concrete baseline을 설명한다.

최소 아래를 포함한다.

- `template_id`, `version`, `status`, `project_type`, `deploy_profile`
- canonical deploy model
- monorepo single workload 규칙
- MSA multiple workload 규칙
- build / release / inventory 책임 분리
- app-only change vs deploy-affecting change 기준
- known constraints
- exclusion 범위

### `docs/templates/index.md`

registry entry로 `Clever-ODIC-deploy`를 추가한다.

registry 표시는 아래처럼 읽히게 한다.

- status: `recommended`
- use_case: current CLEVER deploy baseline for monorepo and MSA
- deploy_profile: `image-build-once-central-release`
- summary: current CLEVER deploy truth elevated as a reusable template family

기존 템플릿과의 관계도 짧게 적는다.

- `Clever-ODIC-deploy`: 현재 deploy 정본
- `msa-template`: family-level 설명용 entry
- `test-erik-project-template`: 특정 monorepo scaffold와 infra posture를 가진 별도 템플릿

### `docs/root/deploy-template-governance.md`

root 기준 문서로서 아래를 분명히 한다.

- current deploy baseline은 `Clever-ODIC-deploy` family로 읽는다
- reusable asset은 `templates/deploy/`에 두되 current truth 자체를 대체하지는 않는다
- legacy CDK/ECS 계열은 current authority가 아니다
- monorepo와 MSA는 같은 deploy model의 다른 workload cardinality일 뿐이다

### `templates/deploy/*`

reusable boilerplate 자산이다.

이 자산은 아래를 기준으로 다시 정리한다.

- build repo / release repo / runtime inventory의 역할 분리
- monorepo single workload 예시
- MSA multiple workload 예시
- env/secret category 예시
- override boundary
- public contract probe checklist

## reusable asset 설계

### `templates/deploy/README.md`

아래 질문에 답하는 entrypoint 문서로 바꾼다.

- 이 자산은 어떤 deploy model을 전제로 하는가
- 언제 monorepo single workload로 읽는가
- 언제 MSA multiple workload로 읽는가
- current truth는 어디서 보는가
- override는 어디까지 허용되는가

### `templates/deploy/checklist.md`

기존 check list를 아래 기준으로 확장한다.

- image artifact immutable tag 확인
- central release 대상 확인
- runtime inventory 등록 상태 확인
- rollback target 확인
- public contract probe 항목 확인

monorepo와 MSA 공통 항목을 유지하되, MSA는 workload bundle 확인이 추가된다고 설명한다.

### `templates/deploy/env-template.example`

실제 값 예시가 아니라 category 예시로 유지한다.

최소 아래 범주를 보여준다.

- image reference category
- runtime env category
- secret reference category
- inventory identifier category
- probe / health path category

monorepo는 single workload 예시, MSA는 workload별 변수 naming 예시를 같이 둔다.

### `templates/deploy/override-guide.md`

아래 override만 허용 범위로 본다.

- image repository or tag policy 차이
- env category 차이
- secret source 차이
- external integration 차이
- rollout bundle 차이

반대로 아래는 baseline 변경 또는 migration으로 기록한다.

- deploy ownership 변경
- central release bypass
- runtime inventory 없이 direct deploy
- immutable artifact 규칙 파기
- public contract probe 생략

## 오류 모델

`Clever-ODIC-deploy`는 모든 배포 오류를 막지 않는다.

대신 아래 class 오류를 줄이는 데 책임이 있다.

- build와 deploy ownership 혼동
- monorepo와 MSA를 전혀 다른 deploy 방식으로 오해하는 것
- runtime inventory 없이 임의 deploy scope를 잡는 것
- release success만으로 완료를 닫는 것
- env/secret/health path 같은 deploy contract 변화를 app-only change로 오해하는 것

반대로 아래는 template만으로 해결되지 않는다.

- business logic bug
- service-specific startup bug
- 데이터 정합성 문제
- 운영자 승인 누락
- runtime inventory 자체가 잘못 기록된 경우

## 마이그레이션 해석

기존 서비스가 아래 조건이면 `Clever-ODIC-deploy` 채택 또는 migration 검토 대상으로 본다.

- 현재 image artifact + central release + runtime inventory 모델을 실제로 따르고 있다
- 하지만 서비스 문서에 그 lineage가 명시돼 있지 않다

이 경우 신규 서비스는 `Clever-ODIC-deploy@v1`을 기본 추천으로 두고, 기존 서비스는 service metadata에 lineage를 보강한다.

반대로 legacy CDK/ECS 또는 direct host deploy 모델은 current recommended가 아니다.

## 테스트와 검증

이번 범위는 문서형 템플릿이므로 검증은 아래로 한다.

- registry entry가 `docs/templates/index.md`에서 일관되게 보이는지
- root governance와 template family 설명이 모순되지 않는지
- `templates/deploy/*` 자산이 canonical model과 같은 어휘를 쓰는지
- monorepo / MSA 차이가 workload cardinality로만 설명되는지
- legacy ECS/CDK를 current truth처럼 읽히게 두지 않는지

## 구현 순서

1. `Clever-ODIC-deploy` spec 승인
2. template family 문서 추가
3. registry 갱신
4. root deploy governance 갱신
5. `templates/deploy/*` 자산 갱신
6. 문서 간 용어 정합성 점검

## 오픈 포인트

이번 설계에서 의도적으로 남겨 둔 것은 아래다.

- service metadata example을 같은 턴에 같이 갱신할지 여부
- change-control template에 deploy lineage 확인 항목을 추가할지 여부
- legacy deploy family를 별도 `legacy` template entry로 등록할지 여부

표준 범위에서는 위 셋을 당장 다루지 않는다.
