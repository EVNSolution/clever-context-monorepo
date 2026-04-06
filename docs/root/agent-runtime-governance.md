# 에이전트 실행 기준

## 전제

- 에이전트는 공유 에이전트 디바이스에서 실행된다고 가정한다.
- 같은 장비를 여러 사용자 세션이 순차적으로 사용할 수 있다.

## 작업 시작 기준

작업을 시작하기 전에 아래 네 가지를 먼저 확정한다.

1. user session
2. target repo
3. target service
4. change id

이 값이 확정되지 않으면 이전 문맥을 임의로 이어서 사용하지 않는다.

## 세션 규칙

- 이전 세션 문맥을 다음 세션으로 넘기지 않는다.
- 현재 세션에 필요한 문맥만 다시 읽어온다.
- 사용자 세션이 바뀌면 대상 저장소와 대상 서비스도 다시 확인한다.

## 문맥 범위

- agent는 하나의 실행자다.
- global과 local은 agent 종류가 아니라 context 범위다.
- global context는 전역 규칙, contracts, root 문서를 뜻한다.
- local context는 현재 target repo, target service, change id 범위를 뜻한다.
