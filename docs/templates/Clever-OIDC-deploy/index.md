# Clever-OIDC-deploy

## 목적

이 문서는 `Clever-OIDC-deploy` template family entry다.

이 family는 build, release, runtime inventory 책임을 분리하는 deploy contract를
template lineage로 기록한다. 실제 release workflow, runtime inventory, proof는
소유 repo 문서를 따른다.

## Registry 요약

- `template_id`: `Clever-OIDC-deploy`
- `current_version`: `v1`
- `status`: `recommended`
- `use_case`: deploy lineage for image artifact and central release separation
- `deploy_profile`: `image-build-once-central-release`
- `summary`: reusable deploy template contract for build/release/inventory separation
- `allowed_override_boundary`: image policy, env category, secret source, external integration, rollout scope
- `known_constraints`: runtime facts and release evidence are out of scope for this registry entry

## Contract

- code owner와 release owner를 구분한다.
- mutable working tree가 아니라 immutable artifact 기준으로 release를 닫는다.
- workload scope는 runtime inventory 소유 문서에서 판단한다.
- release 완료 판단은 소유 repo의 verification/runbook 기준을 따른다.

## 제외 범위

- release workflow 구현 파일
- runtime inventory 데이터
- runtime slice별 rollout caveat
- secret, ARN, URL, account id
- runtime proof 또는 release evidence

## 버전 문서

- [`v1`](./versions/v1.md)
