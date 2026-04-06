# clever-context-monorepo

## 목적

이 저장소는 팀 에이전트 규칙 통합과 기능 구현 흐름에 필요한 정본 문맥을 관리하기 위한 저장소다.

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

## clever-change-control과의 관계

`clever-change-control`은 변경 요청, 승인, rollout/rollback 흐름을 관리한다. 이 저장소는 그 흐름에서 참조하는 정본 문맥, 규칙, 용어, 계약을 관리한다.
