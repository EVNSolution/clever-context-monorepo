# Architecture Principles

## 기본 원칙

- 서비스 경계는 명확하게 유지한다.
- 공통 규칙은 `contracts` 또는 `docs/root`에 둔다.
- 서비스 내부 결정은 `docs/services`에 둔다.
- 변경 단위는 change id로 연결한다.

## 문맥 해석

- 전역 구조 판단은 root 문서를 따른다.
- 서비스 구현 판단은 서비스 문서를 따른다.
- 충돌 시 상위 우선순위 문서를 기준으로 정리한다.
