# 서비스 문서 시작점

## 목적

이 문서는 `docs/services` 아래 문서를 시작하는 기준점이다.

## 초기 서비스 목록

- 현재 초기 서비스 목록은 비워 둔다.
- target service가 확정되기 전에는 서비스 문서를 미리 만들지 않는다.
- 첫 서비스가 확정되면 아래 명명 규칙과 템플릿을 기준으로 디렉터리를 추가한다.

## 서비스명 규칙

- 서비스 디렉터리명은 소문자 kebab-case를 사용한다.
- 문서와 이슈, PR에서는 같은 서비스 키를 그대로 재사용한다.
- 예) `billing-api`, `admin-web`, `settlement-worker`

## 서비스 문서 최소 구조

- 각 서비스는 `docs/services/<service-name>/index.md`를 시작 문서로 둔다.
- 필요하면 같은 디렉터리에 API, 의존성, 운영 메모를 추가한다.
- 공통 규칙은 root와 contracts를 참조하고, 서비스 고유 규칙만 서비스 문서에 둔다.

## 시작 템플릿

- 새 서비스 문서는 [`docs/services/service-template.md`](./service-template.md)를 복사해서 시작한다.
