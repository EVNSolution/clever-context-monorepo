# msa-template

## 목적

이 문서는 `msa-template` family entry다. 하나의 제품 설명이 아니라, MSA 계열 repo를 패키지처럼 선택하고 조합할 수 있게 하는 template registry README 역할을 한다.

## registry 요약

- `template_id`: `msa-template`
- `current_version`: `v1`
- `status`: `candidate`
- `use_case`: multi-repo MSA bootstrap with archetype-specific repo packages
- `deploy_profile`: `image-build-once-and-promote`
- `summary`: 운영, 인프라, 프론트, 게이트웨이, 서비스 repo archetype을 하나의 family로 설명하는 package-style template entry
- `bootstrap_rules`: family 문서는 archetype과 metadata만 제공하고, 실제 business content나 runtime truth는 싣지 않는다.
- `allowed_override_boundary`: org naming, repository naming, CI vendor, deploy environment naming, package manager 선택은 조정 가능하다.
- `known_constraints`: family entry만으로는 scaffold 파일을 직접 제공하지 않으며, leaf archetype마다 후속 concrete asset이 필요할 수 있다.

## 배포 템플릿 약속

이 family는 deploy 관점에서 아래 원칙을 패키지 단위 baseline으로 본다.

- build와 deploy를 분리한다.
- immutable artifact를 기준으로 verification lane과 release lane을 닫는다.
- env/secret, health path, port, upstream name 같은 deploy contract를 문서와 template boundary로 고정한다.
- 반복되는 deploy wiring 오류는 root deploy governance와 leaf archetype baseline이 먼저 막아야 한다.

즉 이 family는 \"앱이 바뀌면 배포도 매번 새로 해석한다\"가 아니라, \"deploy contract가 안 바뀌는 앱 변화는 같은 deploy template가 흡수한다\"를 목표로 한다.

## 현재 proof에서 끌어올린 공통 제약

이 family는 특정 서비스의 현재 runtime truth를 직접 담지 않지만, 이미 운영 proof에서 반복 확인된 제약은 family 차원의 배포 원칙으로 끌어올린다.

- stack/bootstrap lane과 warm-host partial lane을 분리해서 기록한다.
- partial deploy를 말할 때는 `같은 host에서 서비스만 in-place 반영`인지, `새 host에서 누적 상태를 다시 bootstrap`하는지 구분해서 적는다.
- burstable host는 메모리 snapshot만으로 판단하지 않는다.
  - bootstrap 구간 CPU 평균/최대
  - `CPUCreditBalance`
  - `CPUSurplusCreditBalance`
  - post-smoke steady-state CPU
  를 함께 남긴다.
- backend worker 수는 app tuning이 아니라 deploy sizing 입력값으로 본다.
- `strict full`과 현재 운영 `full`에서 아직 비활성 prerequisite가 남은 proof는 구분해서 적는다.
- partial deploy proof에는 runner 권한, host-side drift gate, rollback 가능 상태 같은 운영 조건을 같이 기록한다.

구체적인 현재 증거와 숫자는 template 정본이 아니라 내부 wiki 요약에 둔다.

- [`docs/wiki/ev-dashboard-runtime-proof.md`](../../wiki/ev-dashboard-runtime-proof.md)

## 현재 상태

현재 `msa-template`는 실행형 템플릿이 아니라 family 설명용 registry entry다.

즉 아래 상태로 본다.

- GitHub template repo는 아직 없다.
- concrete scaffold asset도 아직 없다.
- 에이전트가 바로 선택해서 repo bootstrap에 사용하는 템플릿은 아직 아니다.
- 지금 단계에서 이 문서의 역할은 archetype family와 metadata 경계를 설명하는 것이다.

하지만 deploy 관점에서의 baseline 의미는 이미 있다.

- 어떤 종류의 앱 변화는 template update 없이 흡수돼야 하는지
- 어떤 변화는 deploy contract 변경으로 취급해야 하는지
- 같은 배포 실수를 반복하지 않으려면 무엇을 root governance로 끌어올려야 하는지

## 채택 적합 조건

아래에 가까우면 이 family를 먼저 본다.

- 운영/인프라/프론트/서비스 repo를 한 계열로 설명해야 하는 경우
- repo archetype을 나눠 추적하되, 사용자에게는 하나의 package처럼 보여주고 싶은 경우
- 이후 public GitHub template family로 확장 가능한 naming과 metadata가 필요한 경우

## 채택 제외 또는 주의 조건

아래에 가까우면 다른 entry를 먼저 검토한다.

- 단일 repo scaffold 하나만 필요한 경우
- business domain 예제나 sample content가 템플릿에 같이 들어가야 하는 경우
- 현재 runtime/operator truth를 그대로 템플릿 문서에 섞으려는 경우
- 지금 바로 에이전트 bootstrap에서 selectable template로 써야 하는 경우

## family에 포함되는 leaf template

1. `msa-ops-orchestration-template`
2. `msa-infra-ecs-template`
3. `msa-front-web-template`
4. `msa-edge-gateway-template`
5. `msa-service-django-template`
6. `msa-service-read-model-template`
7. `msa-service-special-runtime-template`

## leaf template 역할

### `msa-ops-orchestration-template`

- local compose, seed, smoke, env glue 같은 orchestration repo archetype

### `msa-infra-ecs-template`

- ECS/CDK, deploy workflow, edge ingress를 소유하는 infra repo archetype

### `msa-front-web-template`

- 사용자 UI 런타임과 browser smoke 대상이 되는 frontend repo archetype

### `msa-edge-gateway-template`

- HTTP edge routing, prefix ownership, upstream mapping을 소유하는 gateway repo archetype

### `msa-service-django-template`

- write owner 또는 registry 역할의 Django service repo archetype

### `msa-service-read-model-template`

- read-only projection, operations view, analytics surface를 담당하는 service repo archetype

### `msa-service-special-runtime-template`

- worker, ingest hub, dead-letter, notification hub 같은 special runtime repo archetype

## template-only 원칙

이 family entry는 archetype registry다. 아래는 의도적으로 넣지 않는다.

- business rule 예제
- 고객/조직 전용 내용
- secret, ARN, internal URL
- rollout history나 운영 incident 기록
- 특정 서비스의 현재 canonical runtime truth

대신 deploy 안전성에 대한 추상 규칙은 넣는다.

- 어떤 변화가 app-only 인지
- 어떤 변화가 deploy-affecting 인지
- 반복되는 deploy 실수는 template/governance가 막아야 한다는 원칙

이 수준은 runtime history가 아니라 template contract 설명으로 본다.

## 보장 범위와 비보장 범위

이 family가 암묵적으로 약속하는 범위는 아래다.

- 앱 내부 변화가 기존 deploy contract를 유지하면, 같은 deploy template를 계속 재사용할 수 있어야 한다.
- build/deploy 분리, immutable artifact, preflight, post-deploy verification 같은 배포 기본선은 다시 흔들리지 않아야 한다.

이 family가 약속하지 않는 범위는 아래다.

- business logic 자체의 무오류
- migration/seed/data correctness
- 새 runtime dependency를 추가했는데 문서를 안 바꾼 경우
- port, health path, env, secret, startup order 같은 contract가 바뀌었는데 template를 안 올린 경우

즉 정직한 표현은 \"다시는 오류가 안 난다\"가 아니라 아래다.

- pure app-only change라면 deploy wiring 오류가 반복되지 않아야 한다.
- deploy-affecting change라면 template 또는 governance update가 같이 필요하다.

## future public template readiness

이 family는 나중에 GitHub public template 계열로 옮길 수 있어야 한다. 그래서 문서 단계부터 아래를 지킨다.

- neutral naming 우선
- placeholder-safe metadata 사용
- private repo나 org 내부 운영 세부에 묶이지 않는 설명 사용
- scaffold package로 확장해도 그대로 재사용 가능한 archetype naming 유지

public template로 나갈 때도 같은 원칙을 유지한다.

- zero-error promise는 쓰지 않는다.
- 대신 guarantee boundary를 쓴다.
- template가 막는 오류와 못 막는 오류를 분리해서 적는다.

## 버전 문서

- [`v1`](./versions/v1.md)
