# msa-template

## 목적

이 문서는 `msa-template` family entry다.

MSA product service를 구성하는 repo/runtime slice archetype을 template family로
묶어 설명한다. 실제 product/runtime 사실은 소유 repo 문서를 따른다.

## Registry 요약

- `template_id`: `msa-template`
- `current_version`: `v1`
- `status`: `candidate`
- `use_case`: package-style MSA template family registry
- `deploy_profile`: `image-build-once-and-promote`
- `summary`: MSA archetype family entry for repo/runtime slice templates
- `allowed_override_boundary`: org naming, repository naming, CI vendor, deploy environment naming, package manager
- `known_constraints`: scaffold asset and product-specific runtime facts are out of scope

## 포함되는 Archetype

1. `msa-ops-orchestration-template`
2. `msa-infra-ecs-template`
3. `msa-front-web-template`
4. `msa-edge-gateway-template`
5. `msa-service-django-template`
6. `msa-service-read-model-template`
7. `msa-service-special-runtime-template`

## Template-only 원칙

이 family entry에는 아래를 넣지 않는다.

- business rule 예제
- 고객/조직 전용 내용
- secret, ARN, internal URL
- rollout history나 운영 incident 기록
- 특정 product service의 runtime truth
- runtime proof 또는 host sizing evidence

## 채택 제외 또는 주의 조건

아래에 가까우면 소유 repo 문서를 먼저 본다.

- 현재 운영 중인 runtime topology를 판단해야 한다.
- runtime slice별 rollout/runbook detail이 필요하다.
- 지금 바로 실행형 scaffold가 필요하다.

## 버전 문서

- [`v1`](./versions/v1.md)
