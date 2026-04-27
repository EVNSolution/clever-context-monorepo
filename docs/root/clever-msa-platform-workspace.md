# CLEVER MSA Platform Pointer

## 목적

이 문서는 CLEVER MSA platform 사실을 복제하지 않고, 정본 위치만 고정한다.

CLEVER MSA platform은 product service 단위에서 하나로 본다. 그 안의
`service-*`, `front-*`, `edge-*`, `runtime-*`는 runtime slice다.

## Current Authority

CLEVER MSA platform의 구조, runtime slice, deploy, boundary, contract 사실은
`clever-msa-platform` repo가 소유한다.

우선순위는 아래 repo 문서를 따른다.

1. `clever-msa-platform:WORKSPACE.md`
2. `clever-msa-platform:repo-map.md`
3. `clever-msa-platform:docs/mappings/current-runtime-inventory.md`
4. `clever-msa-platform:docs/mappings/repo-responsibility-matrix.md`
5. `clever-msa-platform:docs/boundaries/`
6. `clever-msa-platform:docs/contracts/`
7. `clever-msa-platform:docs/runbooks/`

## 이 Repo에 두지 않는 것

- platform 문서 요약
- runtime slice별 API/env/ECR/commit 정보
- rollout proof와 smoke evidence
- local absolute path
- batch scan 결과
- `확인 필요` 상태의 정보

정본 위치가 바뀐 경우에만 이 pointer 문서를 갱신한다.
