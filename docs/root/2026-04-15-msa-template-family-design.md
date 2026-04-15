# MSA Template Family Design

## 목적

이 문서는 CLEVER의 MSA 계열 템플릿을 `docs/templates` 아래에서 어떻게 패키지처럼 보이게 정리할지에 대한 설계 문서다.

핵심 목표는 아래다.

1. 사용자는 `msa-template`라는 하나의 상위 템플릿 패밀리를 먼저 이해할 수 있어야 한다.
2. 실제 채택과 lineage 추적은 서비스 성격별 leaf template 단위로 남겨야 한다.
3. `docs/templates`는 registry 정본으로 유지하고, runtime/operator truth와 섞지 않는다.

## 현재 상태

현재 `clever-context-monorepo`는 아래 구조를 이미 가진다.

- `docs/templates/index.md`
- `docs/templates/test-erik-project-template/`
- `docs/root/template-harness-governance.md`
- `docs/root/deploy-template-governance.md`

즉 템플릿 registry 형식은 이미 존재한다. 다만 CLEVER MSA 내부 archetype을 한 묶음으로 보여 주는 family template entry는 아직 없다.

## 문제 정의

지금 `clever-msa-platform` 쪽에서는 아래 archetype baseline이 정리됐다.

- orchestration
- infra-ecs
- frontend runtime
- gateway runtime
- django write/registry service
- read-model service
- special runtime service

하지만 이 상태만으로는 아래 문제가 남는다.

1. 사용자는 "MSA 템플릿 한 벌"을 고르기보다 조각난 규칙을 읽게 된다.
2. 유지보수 시 어느 archetype lineage를 타는지 서비스 문서에 일관되게 남기기 어렵다.
3. `clever-agent-project`에서 템플릿을 보여 줄 때도 하나의 패밀리와 세부 leaf를 동시에 설명할 구조가 부족하다.

## 선택지 비교

### 1. leaf template만 직접 등록

예:

- `msa-service-django-template`
- `msa-front-web-template`
- `msa-edge-gateway-template`

장점:

- 단순하다.
- 추적 단위가 명확하다.

단점:

- 사람이 보기엔 CLEVER MSA가 하나의 계열이라는 점이 잘 드러나지 않는다.
- intake 단계에서 템플릿 선택이 과도하게 조각난다.

### 2. `msa-template` 하나만 등록

장점:

- 설명이 쉽다.
- 패키지처럼 보인다.

단점:

- 실제 lineage 추적이 너무 뭉개진다.
- 서비스별 deploy profile과 archetype 차이를 반영하기 어렵다.

### 3. `msa-template` family + leaf template 조합

장점:

- 사람에게는 하나의 패키지처럼 설명할 수 있다.
- 시스템에는 leaf template 단위 추적을 남길 수 있다.
- 향후 versioning과 migration도 family/leaf 두 층으로 정리할 수 있다.

단점:

- 문서 구조를 한 단계 더 설계해야 한다.

## 선택된 접근

이번 설계는 3번을 채택한다.

한 줄로 정리하면 이렇다.

`docs/templates`에는 `msa-template` family entry를 두고, 실제 채택과 lineage 추적은 그 아래 leaf template로 분리한다.

## template-only 원칙

이 family entry는 템플릿 registry 문서다. 따라서 아래 원칙을 강하게 지킨다.

1. business content를 넣지 않는다.
2. 특정 고객/조직 전용 naming, URL, ARN, account 값을 넣지 않는다.
3. 현재 운영 truth, runbook, rollout caveat를 템플릿 문서에 섞지 않는다.
4. 서비스 예시는 archetype 설명용으로만 취급하고, template payload로 고정하지 않는다.

즉 이 문서와 이후 registry entry는 "무엇을 복제 가능한 템플릿 단위로 볼 것인가"만 설명해야 한다.

## 목표 구조

### registry entry

아래 구조를 새 기준으로 둔다.

```text
docs/templates/
  index.md
  msa-template/
    index.md
    versions/
      v1.md
```

`msa-template/index.md`는 사실상 family README 역할을 한다.

여기에는 아래를 적는다.

- `template_id`
- `current_version`
- `status`
- `use_case`
- `deploy_profile`
- `summary`
- `bootstrap_rules`
- `allowed_override_boundary`
- `known_constraints`
- 포함되는 leaf template 목록

### leaf template set

family 문서 안에서 아래 leaf template를 정의한다.

1. `msa-ops-orchestration-template`
2. `msa-infra-ecs-template`
3. `msa-front-web-template`
4. `msa-edge-gateway-template`
5. `msa-service-django-template`
6. `msa-service-read-model-template`
7. `msa-service-special-runtime-template`

## leaf template 의미

### `msa-ops-orchestration-template`

- 대상: orchestration repo archetype
- 기준 자산:
  - `compose/`
  - `infra/`
  - `scripts/`
  - `tests/`
  - `README.md`
  - `lesson.md`

### `msa-infra-ecs-template`

- 대상: ECS/CDK infra repo archetype
- 기준 자산:
  - `bin/`
  - `lib/`
  - `test/`
  - `README.md`
  - `lesson.md`
  - `.github/workflows/deploy-ecs.yml`

### `msa-front-web-template`

- 대상: frontend runtime repo archetype
- 기준 자산:
  - `.env.local.example`
  - `.env.local-test.example`
  - `Dockerfile`
  - `README.md`
  - `lesson.md`
  - build workflow

### `msa-edge-gateway-template`

- 대상: edge gateway repo archetype
- 기준 자산:
  - `nginx.conf`
  - `tests/`
  - `Dockerfile`
  - `README.md`
  - `lesson.md`
  - build workflow

### `msa-service-django-template`

- 대상: write/registry 계열 service repo archetype
- 기준 자산:
  - `manage.py`
  - `config/`
  - primary app package
  - `entrypoint.sh`
  - `requirements.txt`
  - `Dockerfile`
  - `README.md`
  - `lesson.md`
  - build workflow

### `msa-service-read-model-template`

- 대상: read-model service repo archetype
- 기준 자산은 django service와 유사하되, README에서 read-only boundary와 upstream dependency를 더 강하게 명시한다.

### `msa-service-special-runtime-template`

- 대상: special runtime service repo archetype
- 기준 자산은 django service와 유사하되, worker-only / dead-letter / ingest hub 같은 runtime role을 별도 규칙으로 가진다.

## future public template readiness

이 family는 이후 GitHub public template 계열로 확장될 수 있어야 한다. 그래서 registry 단계부터 아래를 보장한다.

1. 문서는 vendor-neutral 또는 domain-neutral wording을 우선 사용한다.
2. private repo 이름, internal-only URL, secret naming, private org workflow를 template contract로 넣지 않는다.
3. placeholder는 public-safe 값으로 유지한다.
4. template 문서는 archetype, metadata, allowed override boundary만 다루고, internal migration history는 제외한다.
5. 실제 public template asset으로 옮길 때도 문서 제목과 leaf naming을 그대로 재사용할 수 있어야 한다.

## 문서 배치 규칙

### `docs/templates`에 둘 것

- family entry
- version 문서
- leaf template 설명
- 채택 적합/제외 조건
- deploy profile 요약

### `docs/root`에 둘 것

- 템플릿 선택 규칙
- template harness governance
- deploy template governance

### `docs/services`에 둘 것

- 실제 서비스별 `template_id`
- `template_version`
- `deploy_profile`
- `override_scope`
- `lifecycle_state`

즉 `docs/templates`는 registry이고, 실제 서비스 채택 결과는 `docs/services`가 정본이다.

## versioning 규칙

`msa-template` family는 family version을 가진다.

예:

- `msa-template@v1`

leaf template는 우선 family 문서 안의 섹션으로 관리한다. 이후 독립적인 변화량이 커지면 아래처럼 분리할 수 있다.

```text
docs/templates/msa-service-django-template/
docs/templates/msa-front-web-template/
...
```

초기에는 과도한 분산보다 family 중심 문서가 더 낫다.

## 비목표

이번 설계는 아래를 하지 않는다.

1. 실제 scaffold 파일을 바로 `templates/` 루트에 복사
2. 모든 repo를 완전히 동일 구조로 강제
3. 서비스 내부 business code 구조를 공통화
4. lineage 추적을 family 하나로만 뭉개기

## 구현 순서

1. `docs/root`에 implementation plan 문서 추가
2. `docs/templates/msa-template/index.md` 추가
3. `docs/templates/msa-template/versions/v1.md` 추가
4. `docs/templates/index.md`에 `msa-template` registry entry 추가
5. 필요하면 `clever-agent-project`와 `clever-change-control` 문서에서 새 family entry를 참조하도록 연결

## 완료 기준

아래가 성립하면 이번 설계가 유효하다.

1. `docs/templates`에서 `msa-template` family를 registry entry로 읽을 수 있다.
2. 사용자는 CLEVER MSA 템플릿을 하나의 패키지처럼 이해할 수 있다.
3. 실제 서비스 문서에는 leaf template lineage를 남길 수 있다.
4. root governance, template registry, service lineage의 경계가 서로 섞이지 않는다.
