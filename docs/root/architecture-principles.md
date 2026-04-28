# 아키텍처 원칙

## 기본 원칙

- product service와 runtime slice 경계는 명확하게 유지한다.
- MSA는 새 service를 붙이는 방식이 아니라 책임, 데이터 소유권, API edge를 분리하는 방식이다.
- 공통 규칙은 `contracts` 또는 `docs/root`에 둔다.
- runtime slice 내부 결정은 소유 repo 문서에 둔다.
- root 작업 라인은 `project-start issue #`로 연결한다.
- scoped execution 단위는 `change id`로 연결한다.

## 문맥 해석

- 전역 구조 판단은 root 문서를 따른다.
- runtime slice 구현 판단은 소유 repo 문서를 따른다.
- 충돌 시 상위 우선순위 문서를 기준으로 정리한다.

## MSA 경계 판단

runtime slice를 새로 만들거나 나눌 때는
[`msa-boundary-governance.md`](./msa-boundary-governance.md)를 먼저 본다.
새 slice는 독립 책임과 authority statement가 먼저 있어야 하며, 데이터/table
ownership, API edge authority, release/workload 경계 중 무엇이 기존 slice
확장만으로 부족한지 설명될 때만 정당화된다. API edge 추가만으로는 새 slice가
아니라 contract/API edge 변경으로 먼저 본다.
