# clever-context-monorepo

## 목적

이 저장소는 팀 에이전트가 따라야 하는 완료된 해석 규칙과 정본 위치 포인터만 관리한다.

이 저장소는 작업 중 문서, 서비스별 상세 정보, 런타임 상태, 운영 증거, target repo 문서의 복사본을 보관하지 않는다.

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

- `docs/root`: 완료된 전역 규칙과 authority boundary
- `docs/services`: 서비스 상세가 아니라 외부 정본 위치만 가리키는 compatibility entry
- `docs/wiki`: 사실 요약이 아니라 포인터만 담는 얇은 탐색 입구
- `contracts`: 이 repo가 직접 소유하는 전역 계약이 있을 때만 사용

## wiki의 위치

`docs/wiki`는 정본이 아니다. 런타임 proof, 진행 상태, 서비스별 요약을 올리지 않는다.

## 문서 우선순위

문서를 해석할 때 우선순위는 다음과 같다.

1. `docs/root`
2. `contracts`
3. 외부 정본 repo의 문서
4. change-control 기록

하위 우선순위 문서는 상위 우선순위 문서와 충돌하면 정본으로 보지 않는다.

## agent와 context 범위

agent는 하나의 실행자다. global과 local은 agent 종류가 아니라 문맥 범위를 뜻한다. global context는 전역 규칙과 공통 문서를, local context는 대상 저장소와 대상 서비스의 현재 변경 범위를 뜻한다.

`project-start` root와 scoped execution의 authority 경계는 `docs/root/authority-boundaries.md`를 우선 기준으로 본다.

## 서비스 상세 문서 규칙

MSA 전체는 product service로 보고, `service-*`, `front-*`, `edge-*`, `runtime-*`는 runtime slice로 부른다.

runtime slice별 API, env, ECR, commit, rollout caveat는 이 repo에 복제하지 않는다. 해당 정보는 소유 repo의 정본 문서를 직접 본다.

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

### PR 완료 후 branch 정리

PR이 merge됐거나 source branch를 버리기로 하고 closed 처리된 뒤에는 task
branch를 정리한다. 단, 해당 branch가 아직 open PR, 후속 issue, child branch,
active release/hotfix에 쓰이면 삭제하지 않는다.

기본 명령은 아래 순서다.

```bash
git switch main
git pull --ff-only origin main
git branch -d <source-branch>
git push origin --delete <source-branch>
git fetch --prune origin
```

- `main`과 `dev`는 삭제 대상이 아니다.
- 기본은 `git branch -d <source-branch>`를 쓴다.
- merge 없이 닫은 branch를 폐기해야 할 때만 사용자 확인 후 `git branch -D <source-branch>`를 쓴다.
- remote branch가 GitHub에서 이미 삭제됐더라도 `git fetch --prune origin`으로 로컬 추적 branch를 정리한다.

## clever-change-control과의 관계

`clever-change-control`은 변경 요청, 승인, rollout/rollback 흐름을 관리한다. 이 저장소는 그 흐름에서 참조하는 정본 문맥, 규칙, 용어, 계약을 관리한다.
