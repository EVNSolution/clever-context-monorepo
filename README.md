# clever-context-monorepo

## 목적

이 저장소는 팀 에이전트 규칙 통합과 기능 구현 흐름에 필요한 정본 문맥을 관리하기 위한 저장소다.

## 작업 시작 규칙

일반적인 CLEVER 작업 시작은 이 저장소가 아니라 `clever-agent-project`에서 한다.

즉 아래처럼 구분한다.

- 작업 라인과 대상 저장소가 아직 정해지지 않은 일반 시작: `clever-agent-project`
- 현재 `clever-context-monorepo` 자체를 수정하는 작업: 여기서 계속

시작 전에 먼저 아래 명령으로 로컬 3저장소, `gh auth status`, GitHub 계정, org membership, 원격 접근, issue/PR/ruleset 조회 가능 여부를 확인한다.

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py --cwd "$PWD" --preflight --json
```

현재 저장소 자체를 직접 수정하는 작업이면 아래처럼 유지보수 모드로 확인한다.

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py --cwd "$PWD" --preflight --current-repo-maintenance --json
```

`preflight_check.ready=false`이면 시작 절차로 내려가지 않고 실패한 check를 먼저 해결한다.
`true`이면 내부 `workspace_check.agent_action`을 아래처럼 해석한다.

- `proceed-with-hard-gate`: 로컬 상태가 정상이며 시작 절차를 진행할 수 있음
- `current-repo-maintenance`: 현재 저장소 자체를 수정하는 세션이므로 여기서 계속
- `switch-to-clever-agent-project`: 일반 시작이므로 `clever-agent-project`로 이동
- `stop-and-fix-workspace`: 필요한 로컬 저장소 구성이 부족하므로 먼저 보완

즉 이 저장소는 정본 해석용 저장소이지, 일반 시작의 기본 위치는 아니다.

## 디렉터리 역할

- `docs/root`: 전역 규칙, 시작 순서, 공통 원칙의 정본
- `docs/services`: 서비스별 문맥과 서비스 단위 규칙
- `docs/wiki`: 빠르게 찾아가기 위한 탐색 입구
- `contracts`: 전역 계약, 공통 인터페이스, 공통 규칙 기준점

## wiki의 위치

`docs/wiki`는 정본이 아니다. 빠르게 탐색하기 위한 입구이며, 최종 판단은 정본 문서로 되돌아가서 한다.

## 문서 우선순위

문서를 해석할 때 우선순위는 다음과 같다.

1. `docs/root`
2. `contracts`와 전역 규칙
3. `docs/services`
4. change package
5. generated summary

하위 우선순위 문서는 상위 우선순위 문서와 충돌하면 정본으로 보지 않는다.

## agent와 context 범위

agent는 하나의 실행자다. global과 local은 agent 종류가 아니라 문맥 범위를 뜻한다. global context는 전역 규칙과 공통 문서를, local context는 대상 저장소와 대상 서비스의 현재 변경 범위를 뜻한다.

`project-start` root와 scoped execution의 authority 경계는 `docs/root/authority-boundaries.md`를 우선 기준으로 본다.

## services 시작 규칙

초기 서비스 목록은 비워 둔 상태로 시작한다. target service가 확정되면 `docs/services/index.md`의 명명 규칙과 `docs/services/service-template.md`를 기준으로 서비스 문서를 추가한다.

## main 브랜치 운영 규칙

이 repo는 public으로 운영하며, `main`은 GitHub ruleset `CLEVER protect main`으로 보호한다.
기본값은 direct push 금지, PR 필수, 승인 수 0명이다.
merge/write는 repo admin 권한자만 수행한다.
admin bypass는 `pull_request` 모드만 허용한다.
상세 기준은 `docs/root/pipeline-governance.md`를 따른다.

`main` merge commit 제목은 PR merge임이 드러나게 아래 형식을 쓴다.

```text
Merge pull request #<pr-number> from <owner>/<source-branch>
```

## clever-change-control과의 관계

`clever-change-control`은 변경 요청, 승인, rollout/rollback 흐름을 관리한다. 이 저장소는 그 흐름에서 참조하는 정본 문맥, 규칙, 용어, 계약을 관리한다.
