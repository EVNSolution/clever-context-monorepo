# 배포 템플릿 거버넌스

## 목적

이 문서는 배포 template lineage의 완료된 해석 규칙만 고정한다.

runtime inventory, rollout proof, host state, runtime slice별 caveat는 이 repo에
두지 않는다. 그런 사실은 deploy/runtime 소유 repo의 정본 문서를 직접 따른다.

## 공통 해석

- app repo 또는 runtime slice는 artifact build 책임을 가질 수 있다.
- release lane과 runtime inventory 책임은 소유 repo 문서에서 확인한다.
- product service가 MSA여도 이 repo에서는 runtime slice별 세부값을 복제하지 않는다.
- deploy-affecting change는 template/governance 변경 필요 여부를 확인한다.

## app-only change

아래는 기존 deploy contract를 유지하면 app-only change로 본다.

- UI 텍스트와 화면 내부 로직
- API handler 내부 계산
- 기존 env, port, health path 안에서의 business rule 변경
- runtime contract를 유지하는 내부 refactor

## deploy-affecting change

아래는 deploy-affecting change로 본다.

- 새 env 또는 secret category
- health path 변경
- port 또는 upstream name 변경
- image architecture 변경
- startup order dependency 추가
- migration/bootstrap prerequisite 추가
- workload scope 또는 runtime sizing 전제 변경

## 문서 위치 규칙

- 이 repo: deploy template lineage와 change classification rule
- 소유 repo: runtime inventory, rollout/runbook, concrete values, proof
- change-control: 승인, rollout/rollback trace, release evidence linkage

## 관련 pointer

- [`docs/templates/Clever-OIDC-deploy/index.md`](../templates/Clever-OIDC-deploy/index.md)
- [`docs/templates/index.md`](../templates/index.md)
- [`docs/root/clever-msa-platform-workspace.md`](./clever-msa-platform-workspace.md)
