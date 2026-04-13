# 공식 시작점

이 문서는 `clever-context-monorepo`의 공식 시작점이다.

## 읽기 순서

1. [`docs/root/index.md`](./index.md)에서 전체 구조를 확인한다.
2. [`docs/root/doc-governance.md`](./doc-governance.md)와 [`docs/root/agent-runtime-governance.md`](./agent-runtime-governance.md)를 읽고 문서 해석 기준과 실행 기준을 맞춘다.
3. MSA 기반 SaaS 복제, 고객사별 변형, 컨테이너별 이미지 운영을 다루는 작업이면 [`docs/root/msa-saas-replication-governance.md`](./msa-saas-replication-governance.md)를 먼저 읽는다.
4. 플랫폼 umbrella workspace나 linked child repo ownership을 다루는 작업이면 [`docs/root/clever-msa-platform-workspace.md`](./clever-msa-platform-workspace.md)를 먼저 읽는다.
5. [`docs/root/pipeline-governance.md`](./pipeline-governance.md), [`docs/root/central-deploy-runtime-current-truth.md`](./central-deploy-runtime-current-truth.md), [`docs/root/architecture-principles.md`](./architecture-principles.md), [`docs/root/security-reliability.md`](./security-reliability.md), [`docs/root/design-system-ux-rules.md`](./design-system-ux-rules.md), [`docs/root/local-verification-modes.md`](./local-verification-modes.md), [`docs/root/ui-implementation-lessons.md`](./ui-implementation-lessons.md)를 읽고 공통 규칙을 확인한다.
6. 필요한 경우 [`contracts/`](../../contracts/)를 확인한다.
7. 대상 서비스가 정해지면 [`docs/services/index.md`](../services/index.md)를 읽고 서비스 문서 구조를 맞춘다.
8. 빠른 탐색이 필요하면 [`docs/wiki/`](../wiki/)를 사용하되 정본 판단은 root와 contracts로 되돌아간다.

## 위치 안내

- [`docs/root`](./): 전역 규칙과 공식 시작점
- [`docs/root/msa-saas-replication-governance.md`](./msa-saas-replication-governance.md): MSA/SaaS 복제형 작업 분기와 문서화 기준
- [`docs/root/clever-msa-platform-workspace.md`](./clever-msa-platform-workspace.md): CLEVER MSA platform umbrella workspace와 linked child repo ownership 기준
- [`docs/services`](../services/): 서비스별 상세 문서
- [`docs/wiki`](../wiki/): 빠른 탐색 입구
- [`contracts`](../../contracts/): 전역 계약과 공통 규칙 기준점
