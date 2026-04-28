# msa-template

## 목적

이 문서는 `msa-template` family entry다.

MSA product service를 개발하기 위한 **repo-template lineage**를 설명한다.
여기서 repo-template은 실행형 scaffold나 배포 절차가 아니라, 새 MSA 개발
저장소가 따라야 하는 루트 구조, 문서 정본, source slice 경계, 개발 운영
규칙의 기준이다.

실제 product/runtime 사실은 소유 repo 문서를 따른다.

## Registry 요약

- `template_id`: `msa-template`
- `current_version`: `v1`
- `status`: `candidate`
- `use_case`: MSA umbrella development repository template lineage
- `deploy_profile`: `not-applicable-development-template`
- `summary`: development repo structure, docs authority, and runtime slice boundary template for MSA product services
- `template_repository`: `EVNSolution/clever-msa-repo-template`
- `allowed_override_boundary`: org naming, repository naming, source-slice naming, docs taxonomy, package manager, local tooling
- `known_constraints`: executable scaffold assets, deploy process, and product-specific runtime facts are out of scope

## Template Source Anchors

이 template family는 특정 product 사실을 복제하지 않는다. 현재 GitHub template
repo artifact와 근거 reference는 아래 pointer를 따른다.

- template repo artifact:
  `https://github.com/EVNSolution/clever-msa-repo-template`
- `clever-msa-platform:WORKSPACE.md`
- `clever-msa-platform:repo-map.md`
- `clever-msa-platform:docs/mappings/repo-responsibility-matrix.md`
- `clever-msa-platform:docs/root` equivalent pointer:
  [`docs/root/clever-msa-platform-workspace.md`](../../root/clever-msa-platform-workspace.md)
- MSA slice 판단 기준:
  [`docs/root/msa-boundary-governance.md`](../../root/msa-boundary-governance.md)

## 포함되는 Development Archetype

1. `msa-root-docs-template`
2. `msa-source-slice-map-template`
3. `msa-front-slice-template`
4. `msa-edge-slice-template`
5. `msa-write-service-slice-template`
6. `msa-read-model-slice-template`
7. `msa-worker-or-special-runtime-slice-template`

위 이름은 개발 repo 구조와 책임 분리 기준을 설명하는 archetype label이다.
특정 framework, cloud runtime, CI/CD vendor, deploy target을 강제하지 않는다.

## Repo-template 기준

- root 문서는 제품의 목표, 경계, mapping, contract, decision의 정본이다.
- source slice는 product service 안의 책임 단위이며, 물리 repo 수와 동일하지 않을 수 있다.
- slice 이름은 역할을 드러내야 한다. 예: `front-*`, `edge-*`, `service-*`, 필요 시 `runtime-*`.
- service slice는 다른 service slice의 내부 구현을 import하지 않는다.
- repo-local README는 사용법과 운영 메모만 담고, 아키텍처 정본은 root docs를 가리킨다.
- 새 slice는 책임, owned data boundary, API edge, consumer, 대안 검토가 먼저 설명되어야 한다.

## Template-only 원칙

이 family entry에는 아래를 넣지 않는다.

- business rule 예제
- 고객/조직 전용 내용
- deploy workflow, release 절차, CI/CD 구현
- secret, ARN, internal URL
- rollout history나 운영 incident 기록
- 특정 product service의 runtime truth
- runtime proof 또는 host sizing evidence

## 채택 제외 또는 주의 조건

아래에 가까우면 소유 repo 문서를 먼저 본다.

- 현재 운영 중인 runtime topology를 판단해야 한다.
- 배포 또는 release runbook detail이 필요하다.
- 지금 바로 실행형 scaffold가 필요하다.
- 특정 framework별 service starter code가 필요하다.

## 버전 문서

- [`v1`](./versions/v1.md)
