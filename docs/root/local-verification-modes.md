# 로컬 검증 모드 기준

## 목적

`5174`, `8080`, low-CPU hybrid, full Docker 중 무엇을 언제 써야 하는지와 원격 프록시 사용 시 반드시 지켜야 할 질문 순서를 고정한다.

이 문서는 local verification의 전역 정본이다.

## 시작 전 질문 순서

로컬 검증을 시작하기 전에 아래 세 질문을 이 순서대로 먼저 보낸다.

1. `백엔드까지 같이 수정/검증할 건가요, 아니면 프론트만 빠르게 볼 건가요?`
2. `데이터는 로컬 localhost를 볼 건가요, 원격 실제 프록시를 볼 건가요, 아니면 원격 local-test(dev/staging) 타깃을 볼 건가요?`
3. `2번에서 실제 프록시를 골랐다면, 실제 DB에 영향을 주는 CRUD를 허용하나요?`

규칙:

- 사용자가 `열어줘`, `띄워줘`, `테스트해줘`만 말해도 먼저 질문한다.
- 답을 받기 전에는 `5174`, `8080`, low-CPU hybrid, full Docker를 임의로 올리지 않는다.
- 잘못된 모드로 이미 띄웠다면 바로 멈추고 현재 상태를 짧게 보고한 뒤 다시 묻는다.

## 모드 매핑

### 프론트만 + 실제 프록시 + CRUD 허용

- `front-web-console/.env.local`
- `VITE_DEV_PROXY_TARGET=https://hub.evnlogistics.com`
- `npm run dev`
- `8080`은 사용자가 full integration을 명시하지 않는 한 올리지 않는다

### 프론트만 + 실제 프록시 + CRUD 비허용

- `front-web-console/.env.local`을 쓰지 않는다
- `front-web-console/.env.local-test` 또는 다른 안전한 타깃으로 전환한다

### 프론트만 + 원격 local-test(dev/staging) 타깃

- `front-web-console/.env.local-test`
- `npm run dev:local-test`

### 백엔드 개발 + 로컬 runtime

- low-CPU hybrid 또는 full Docker
- 필요한 서비스만 localhost로 기동한다

### full integration smoke

- `development/integration-local-stack`
- `8080`

## 5174 규칙

- `5174`는 Vite dev server다
- 프론트 수정 확인용이다
- 원격 프록시를 붙여도 앱의 CRUD는 원격 DB에 직접 영향을 줄 수 있다
- full integration smoke는 `5174`가 아니라 `8080` 또는 배포 환경에서 다시 본다

## 원격 프록시 경고 문구

`front-web-console` `5174`를 원격 API에 붙일 때는 아래 경고를 그대로 보낸다.

`현재 로컬 프론트 테스트의 CRUD는 실제 DB에 영향을 줍니다. 변경을 원하면, PROXY TARGET을 변경하십시오.`

## env 파일 규칙

- `.env.local`은 실제 프록시와 실데이터 확인 모드에만 쓴다
- `.env.local-test`는 원격 local-test(dev/staging) 타깃에 쓴다
- 원격 검증 중에도 사용자가 실데이터 CRUD를 허용하지 않으면 `.env.local`로 가지 않는다

## 8080 규칙

- `8080`은 gateway 뒤 통합 진입점이다
- full Docker stack 또는 full integration smoke에서 쓴다
- 프론트만 빠르게 확인하는 작업에 `8080`을 습관적으로 올리지 않는다

## 검증 후 정리

- 사용자가 환경 정리를 요청하면 `5174`, `8080`, Playwright 세션을 모두 내린다
- 배포가 끝난 뒤에는 local verification용 임시 상태를 남기지 않는다
