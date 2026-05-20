# 문서 관리 기준

## 원칙

- 이 repo에는 완료된 규칙과 정본 위치 포인터만 둔다.
- 작업 중 문서, 구현 계획, runtime proof, release evidence, batch scan 결과는 두지 않는다.
- target repo의 API, env, secret category, ECR, commit, rollout caveat를 복제하지 않는다.
- 같은 사실은 소유 repo 한 곳에서만 관리하고, 이 repo는 필요하면 pointer만 둔다.
- generated summary는 이 repo의 정본 입력으로 쓰지 않는다.

## 변경 기준

- 전역 규칙 변경은 `docs/root` 또는 `contracts`에서 먼저 반영한다.
- product service 또는 runtime slice 범위 변경은 소유 repo의 정본 문서에서 반영한다.
- 이 repo에는 그 정본 위치가 바뀌었을 때 pointer만 갱신한다.
- change package는 승인/추적 repo의 기록이지 이 repo에 복제할 문서가 아니다.

## 사용자 승인 pointer 예외

사용자가 명시적으로 요청하면 issue-backed 외부 monorepo 작성 목록을 짧은 pointer로
남길 수 있다.

- 위치: `docs/root/monorepo-write-pointers.md`
- 허용: target repo, issue URL, 작성 대상 파일 경로, 작성 범주
- 금지: target repo 문서 본문 복제, runtime proof, credential, provider payload

## 금지 문서

아래는 이 repo에 만들지 않는다.

- target repo 상세를 복제하는 `docs/services/<slice>/index.md`
- 작업 계획이나 implementation plan
- 진행 중 design draft
- runtime current status
- runtime proof 또는 smoke evidence
- 서비스별 README 요약
- 서비스별 API/env/ECR/commit 목록
- local absolute path가 들어간 문서
- `확인 필요` 상태의 정보

## PR 완료 시 context 판단

target repo PR을 닫기 전에 이 repo 반영 필요 여부를 확인한다.

- 전역 규칙, 공통 용어, template registry, authority pointer가 바뀌면 이 repo를 갱신한다.
- 제품/플랫폼/런타임 사실만 바뀌었으면 소유 repo 문서만 갱신하고 이 repo는 `not-needed`로 남긴다.
- 검토 완료 결과에는 `clever-context-monorepo update: <commit-or-PR>` 또는 `not-needed`만 남긴다.
- PR 정보와 운영 증거를 wiki에 올리지 않는다.

## 검토 기준

- 문서 간 용어가 일치해야 한다.
- 외부 정본의 사실을 이 repo가 다시 설명하면 실패다.
- issue-backed pointer는 상세 본문이 아니라 target repo로 가는 작업 위치만 남겨야 한다.
- 실행자가 빠르게 판단할 수 있게 짧고 명확하게 작성한다.
