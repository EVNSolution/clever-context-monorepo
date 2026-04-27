# 에이전트 실행 기준

## 전제

- 에이전트는 공유 에이전트 디바이스에서 실행된다고 가정한다.
- 같은 장비를 여러 사용자 세션이 순차적으로 사용할 수 있다.

## 작업 시작 모드

에이전트는 아래 두 모드를 구분한다.

## Preflight Gate

작업 시작 질문, `project-start` 초안, repo bootstrap, team-work automation으로 내려가기 전에는 `clever-agent-project`의 preflight를 먼저 통과한다.

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py --cwd "$PWD" --preflight --json
```

preflight는 `gh auth status`, GitHub login `OziinG`, `EVNSolution` org membership, 3레포 로컬 workspace, `EVNSolution/*` origin, clean worktree, remote fetch, issue/PR/ruleset read access를 확인한다.

GitHub admin 권한이 필요한 target repo ruleset/protection 작업 전에는 대상 repo를 지정해 admin preflight를 다시 통과한다.

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py \
  --cwd "$PWD" \
  --admin-preflight \
  --target-repo-full-name EVNSolution/<target-repo> \
  --json
```

새 repo 생성 권한은 destructive create 없이 완전히 증명할 수 없으므로, preflight는 membership과 API 접근을 확인하고 실제 생성 성공은 `gh repo create` 결과로 확정한다.

### root intake

새 작업 라인을 여는 intake 단계에서는 아래를 먼저 맞춘다.

1. user session 또는 request context
2. business intent
3. constraints
4. expected result
5. candidate template lineage
6. candidate target repo or target slice if known

이 단계에서는 `change id`, 확정된 `target repo`, 확정된 `target slice`를 강제하지 않는다.

### scoped execution

승인된 `project-start` 아래에서 scope가 고정된 실행 단계에서는 아래를 먼저 맞춘다.

1. user session
2. target repo
3. target slice
4. change id

이 값이 확정되지 않으면 이전 scoped execution 문맥을 임의로 이어서 사용하지 않는다.

## 세션 규칙

- 이전 세션 문맥을 다음 세션으로 넘기지 않는다.
- 현재 세션에 필요한 문맥만 다시 읽어온다.
- 사용자 세션이 바뀌면 intake context 또는 scoped execution context를 다시 확인한다.

## 문맥 범위

- agent는 하나의 실행자다.
- global과 local은 agent 종류가 아니라 context 범위다.
- global context는 전역 규칙, contracts, root 문서를 뜻한다.
- local context는 현재 단계에 따라 다르다.
- root intake에서는 현재 `project-start` draft 범위를 뜻한다.
- scoped execution에서는 target repo, target slice, change id 범위를 뜻한다.
