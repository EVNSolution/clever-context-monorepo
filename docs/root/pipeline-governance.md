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

## main 브랜치 임시 운영 규칙

- 현재 private repo에서는 GitHub branch protection과 rulesets를 사용할 수 없다고 확인했다.
- 그 전까지 `main` 직접 푸시는 원칙적으로 금지하고 PR 경유를 기본값으로 운영한다.
- `main`에 머지하려면 최소 1명의 리뷰 확인을 남긴다.
- 긴급 예외 머지가 필요하면 change id와 사유를 PR 또는 issue에 같이 남긴다.
- 플랜 업그레이드나 공개 전환이 가능해지면 이 임시 규칙을 GitHub 보호 규칙으로 치환한다.

## 제외

- 서버 배포 절차 상세
- 인프라 운영 매뉴얼
