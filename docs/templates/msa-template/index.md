# msa-template

## 목적

이 문서는 `msa-template` family entry다. 하나의 제품 설명이 아니라, MSA 계열 repo를 패키지처럼 선택하고 조합할 수 있게 하는 template registry README 역할을 한다.

## registry 요약

- `template_id`: `msa-template`
- `current_version`: `v1`
- `status`: `recommended`
- `use_case`: multi-repo MSA bootstrap with archetype-specific repo packages
- `deploy_profile`: `image-build-once-and-promote`
- `summary`: 운영, 인프라, 프론트, 게이트웨이, 서비스 repo archetype을 하나의 family로 설명하는 package-style template entry
- `bootstrap_rules`: family 문서는 archetype과 metadata만 제공하고, 실제 business content나 runtime truth는 싣지 않는다.
- `allowed_override_boundary`: org naming, repository naming, CI vendor, deploy environment naming, package manager 선택은 조정 가능하다.
- `known_constraints`: family entry만으로는 scaffold 파일을 직접 제공하지 않으며, leaf archetype마다 후속 concrete asset이 필요할 수 있다.

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

## future public template readiness

이 family는 나중에 GitHub public template 계열로 옮길 수 있어야 한다. 그래서 문서 단계부터 아래를 지킨다.

- neutral naming 우선
- placeholder-safe metadata 사용
- private repo나 org 내부 운영 세부에 묶이지 않는 설명 사용
- scaffold package로 확장해도 그대로 재사용 가능한 archetype naming 유지

## 버전 문서

- [`v1`](./versions/v1.md)
