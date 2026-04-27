# 파이프라인 문맥 기준

## 범위

이 문서는 기능 구현 흐름 기준의 파이프라인 문맥만 다룬다.

## 기준

- root 시작 라인은 `project-start issue #`로 추적한다.
- scoped change, rollout, rollback은 `change id` 기준으로 추적한다.
- 일반 intake에서는 candidate repo/service로 시작할 수 있다.
- 구현이나 rollout을 시작하기 전에는 target repo와 target service를 확정한다.
- spec, ui-spec, contracts, service docs가 맞지 않으면 구현을 진행하지 않는다.
- 파이프라인별 세부 설정값은 서비스 문서에서 관리한다.

## main 브랜치 운영 규칙

- 이 repo는 public으로 운영한다.
- `main`은 GitHub ruleset `CLEVER protect main`으로 보호한다.
- `main` direct push, branch deletion, force push는 금지한다.
- `main` 변경은 PR로만 올린다.
- `main` PR은 최소 1명의 승인이 필요하다.
- admin bypass는 `pull_request` 모드만 허용한다.
- 긴급 변경도 PR 안에 change id와 사유를 남긴다.

## 제외

- 서버 배포 절차 상세
- 인프라 운영 매뉴얼
