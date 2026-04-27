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
- `main` PR의 필수 승인 수는 0명이다.
- merge/write는 repo admin 권한자만 수행한다.
- admin bypass는 `pull_request` 모드만 허용한다.
- 긴급 변경도 PR 안에 change id와 사유를 남긴다.

## PR merge commit 제목 규칙

`main`에 들어가는 merge commit 제목은 PR merge임이 바로 보이게 만든다.
기본 형식은 아래다.

```text
Merge pull request #<pr-number> from <owner>/<source-branch>
```

에이전트가 squash merge를 사용할 때도 GitHub 기본형에 가까운 subject를 명시한다.

```bash
gh pr merge <pr-number> --squash \
  --subject "Merge pull request #<pr-number> from <owner>/<source-branch>" \
  --body-file <merge-body-file>
```

merge body 첫 줄에는 PR title을 남기고, 이어서 merge summary, validation evidence, wiki/service context update result를 남긴다.
일반 작업 commit 제목을 그대로 main merge commit 제목으로 쓰지 않는다.

## PR Scope Grouping Gate

PR은 파일 수가 아니라 변경 축, 검증 단위, 롤백 단위로 나눈다.

같은 issue 안에서 same document/operating-rule cleanup 축이고 same validation command로
충분하면 한 PR로 묶는다. `AGENTS.md`, PR template, startup state
template, project brief template, design source policy, merge title template
sync처럼 같은 운영 규칙을 맞추는 작은 문서 정리는 한 PR로 처리한다.

분리 PR은 아래 경우에 쓴다.

- different app/service/contract surface를 건드린다.
- 테스트 범위와 실패 지점이 다르다.
- merge order dependency가 있다.
- 실패 시 rollback unit이 다르다.

OpenAPI schema 변경, Admin Web smoke 화면, Rider App smoke 화면, Spring
service mock endpoint 구현은 보통 분리 PR로 다룬다.

## 제외

- 서버 배포 절차 상세
- 인프라 운영 매뉴얼
