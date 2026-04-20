# CLEVER MSA Platform Workspace Ownership

## 목적

이 문서는 `clever-msa-platform`이 CLEVER MSA 전환 작업의 umbrella workspace이자 linked child repo 관리 주체라는 점을 먼저 고정하기 위한 root 문서다.

## 기준 source

- 플랫폼 루트: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform`
- 우선 확인 문서
  - `WORKSPACE.md`
  - `repo-map.md`
  - `AGENTS.md`
- MSA SaaS 복제형 분기 기준
  - `docs/root/msa-saas-replication-governance.md`

## 이 workspace가 소유하는 것

- `docs/` 아래의 아키텍처, boundary, mapping, contract, decision, rollout 문서 정본
- `development/*` linked child repo visibility와 포인터 관리
- 어떤 repo를 수정해야 하는지에 대한 선택 기준
- 플랫폼 수준의 경계와 migration 상태

## 이 workspace가 직접 소유하지 않는 것

- 개별 서비스 런타임 구현 코드
- 서비스 간 공유 base package
- 서비스 repo 내부 로직
- 플랫폼 전체 runtime artifact

실제 구현 변경은 각 `development/*` child repo에서 수행한다.

## linked child repo 기준

- `development/*` 아래 각 디렉터리는 독립 구현 repo다.
- 루트는 이 repo들을 linked child repo로 노출하는 umbrella workspace다.
- 새 clone 또는 child repo 포인터 이동 후에는 `git submodule update --init --recursive`를 다시 실행한다.
- active `development/*` repo에 대해 root-tracked implementation snapshot을 다시 두면 안 된다.
- child repo 구현 ownership은 child repo에 남고, root는 umbrella visibility와 platform docs를 관리한다.

## repo 선택 기준

- 아키텍처, migration, mapping, contract, rollout 작업은 `clever-msa-platform/docs/`를 먼저 본다.
- local compose, env template, seed orchestration, smoke script는 `integration-local-stack`(out-of-band, root whitelist 바깥)를 본다.
- gateway routing과 edge entry 동작은 `development/edge-api-gateway/`를 본다.
- operator/admin UI는 해당 `front-*` repo를 본다.
- backend 동작은 해당 `service-*` repo를 본다.

## boundary 기준

- 서비스 repo끼리 직접 import하지 않는다.
- shared base package는 승인된 설계 변경 없이는 만들지 않는다.
- compose, env, seed-runner, cross-repo glue는 service repo 안에 두지 않는다.
- `*-operations-view`는 read service이며 source of truth가 아니다.
- archive는 문서 전용이다.

## active inventory 입구

- target repo와 migration 상태는 `clever-msa-platform/repo-map.md`를 기준으로 확인한다.
- 현재 runtime naming, compose service, gateway prefix는 `clever-msa-platform/docs/mappings/current-runtime-inventory.md`를 기준으로 확인한다.
- `service-vehicle-registry`, `service-settlement-registry`, `service-delivery-record`, `service-terminal-registry`, `service-telemetry-hub` 등 현재 active repo 경계는 platform docs 정본을 따른다.

## 최근 플랫폼 상태 동기화

- 동기화 시점: 2026-04-20
- 최신 런타임 상태 위키: [`clever-msa-platform-current-runtime-status.md`](../wiki/clever-msa-platform-current-runtime-status.md)
- 기준 문서: `WORKSPACE.md`, `repo-map.md`, `docs/mappings/current-runtime-inventory.md`

## LLM 위키 반영 기준

- MSA/SaaS 복제형 작업의 전역 분기 기준은 먼저 `docs/root/msa-saas-replication-governance.md`에서 확인한다.
- target service가 명시적으로 확정되지 않았으면 개별 서비스 leaf 문서를 임의로 만들지 않는다.
- 이런 경우에는 먼저 이 workspace owner 문서처럼 root 문맥을 올리고, 이후 명시적으로 지정된 child repo 단위로 서비스 문서를 작성한다.
- `docs/wiki`는 탐색용 허브이고 최종 판단은 root 문서와 service 문서로 되돌아간다.

## 확인 필요 항목

- 현재 사용 중인 editor/IDE 기준으로 “열려 있는 서비스 폴더”를 기계적으로 식별할 수 있는 표준 절차
- `clever-context-monorepo` 안에서 platform workspace용 별도 `docs/workspaces/` 계층을 둘지 여부

## Batch scanned services

- `development/service-*` 전체를 batch 스캔해 `docs/services/<service-name>/index.md`를 생성했다.
- 개별 child repo leaf 문서는 해당 repo의 README, 설정, URLConf, entrypoint, workflow 기준으로 작성했다.
