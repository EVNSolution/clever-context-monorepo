# 정본 경계

## 목적

이 문서는 CLEVER 3레포 체계에서 무엇이 시작 정본이고, 무엇이 해석 정본이며, 무엇이 승인/추적 정본인지 고정하는 root 문서다.

## 저장소별 authority

- `clever-agent-project`: 실행 시작점. 사용자 요청 intake, bootstrap packet 초안, context 문서 읽기 지시, handoff 제안을 맡는다.
- `clever-context-monorepo`: 해석 정본. 완료된 root rules, template lineage, source-of-truth pointer, contracts를 맡는다.
- `clever-change-control`: 승인·추적 정본. `project-start` root issue, scoped change request, rollout/rollback trace, release evidence linkage를 맡는다.

## 식별자 계층

- root canonical identifier: 생성된 `project-start issue #`
- scoped execution identifier: `change id`

`change id`는 root intake를 대체하지 않는다. root line은 항상 `project-start issue #`로 잡고, 실행 scope가 고정된 후에만 `change id`를 사용한다.

## intake와 scoped execution의 경계

### root intake에서 먼저 필요한 값

- request context 또는 user session
- business intent
- constraints
- expected result
- candidate template lineage
- candidate target repo or target slice if known

### root intake에서 강제하지 않는 값

- `change id`
- 확정된 `target slice`
- 확정된 implementation repo

### approval 후 scoped execution에서 필수인 값

- `change id`
- target repo
- target slice
- rollout scope if deployment-affecting

즉 시작은 느슨하게 열고, 승인 이후 실행 단위는 엄격하게 닫는다.

## 문서 해석 규칙

- 시작 절차의 실행 surface는 `clever-agent-project`를 먼저 본다.
- 규칙 해석과 template/deploy 판단은 `clever-context-monorepo`를 먼저 본다.
- 승인 상태와 change traceability는 `clever-change-control`을 먼저 본다.
- 어느 문서에서도 `change id`를 root 시작 식별자로 다시 올리지 않는다.
- 일반 intake에서는 `target slice`를 후보로만 다룰 수 있다.
- product service와 runtime slice 사실은 소유 repo 문서를 복제하지 않고 pointer로만 연결한다.

## external truth 취급

3레포 바깥 문서를 참조해야 하면 현재 상태를 아래 둘 중 하나로 명시한다.

- `current authority`: 아직 정본이 외부 repo에 있음
- `deprecated reference`: 역사적 참조만 유지

표시가 없으면 3레포 체계의 current authority로 취급하지 않는다.
