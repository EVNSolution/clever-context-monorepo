# 공식 시작점

이 문서는 `clever-context-monorepo`의 공식 시작점이다.

이 repo는 완료된 해석 규칙과 정본 위치 포인터만 가진다. 작업 중 문서,
runtime proof, 서비스별 API/env/ECR/commit 요약, target repo 문서 복사본은
이 repo에 두지 않는다.

## 읽기 순서

1. [`authority-boundaries.md`](./authority-boundaries.md): 3-repo authority 경계
2. [`doc-governance.md`](./doc-governance.md): 이 repo에 둘 수 있는 문서와 둘 수 없는 문서
3. [`domain-glossary.md`](./domain-glossary.md): product service, runtime slice, workload 용어
4. [`agent-runtime-governance.md`](./agent-runtime-governance.md): agent 실행 기준
5. 필요한 template lineage만 [`docs/templates`](../templates/)에서 확인한다.
6. CLEVER MSA platform 사실은 [`clever-msa-platform-workspace.md`](./clever-msa-platform-workspace.md)의 pointer를 따라 원 repo에서 확인한다.
7. 사용자가 명시적으로 요청한 외부 monorepo 작성 pointer는 [`monorepo-write-pointers.md`](./monorepo-write-pointers.md)에서 확인한다.

## 위치 안내

- [`docs/root`](./): 완료된 전역 규칙
- [`docs/templates`](../templates/): 완료된 template registry entry
- [`docs/services`](../services/): runtime slice 상세 문서가 아니라 외부 정본 pointer
- [`docs/wiki`](../wiki/): 사실 요약이 아니라 탐색 pointer
- [`contracts`](../../contracts/): 이 repo가 직접 소유하는 완료된 계약
