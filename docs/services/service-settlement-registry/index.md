# service-settlement-registry

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-settlement-registry`
- 확인 기준 브랜치: `main`
- 확인한 최근 커밋: `f9155f6e70c5aa3f2cd6cee88193ca7f91ba5aaa 2026-04-09 feat: add company fleet settlement pricing table`

## 서비스 개요

이 repo는 정산 기준표와 정책 `registry` 정본 runtime이다.

## 주요 기능

- `SettlementPolicy`, `SettlementPolicyVersion`, `SettlementPolicyAssignment` registry CRUD
- 회사/플릿 scope 기준 assignment 관리
- admin-only management API와 `health` endpoint
- deterministic bootstrap seed command

## 아키텍처와 실행 흐름

- `config/urls.py`가 서비스 앱 URLConf로 요청을 연결한다.
- 핵심 앱 코드는 `settlementregistry/` 아래에 있고 모델, API, 테스트를 함께 둔다.
- `settlementregistry/services/`가 도메인 조합과 외부 연동 helper를 담당한다.
- DRF 기본 인증/권한 설정이 `config/settings.py`에 정의돼 있다.
- 컨테이너 시작 시 migration 후 Gunicorn WSGI 앱을 기동한다.
- health endpoint가 존재한다.

## 디렉터리 구조

```text
service-settlement-registry/
├── config/
├── settlementregistry/
├── README.md
├── Dockerfile
├── entrypoint.sh
├── manage.py
└── requirements.txt
```

## 핵심 엔트리포인트/모듈

- `manage.py`: Django 관리 명령 진입점
- `config/settings.py`: 런타임 설정과 환경변수 로딩
- `config/urls.py`: 루트 URLConf
- `settlementregistry/`: 서비스 핵심 앱 디렉터리
- `settlementregistry/urls.py`: 내부 API route 정의
- `settlementregistry/services/`: 도메인 조합 및 외부 연동 helper
- `settlementregistry/tests/`: 서비스 테스트

## API / worker / batch / 외부 연동

### API
- `/`
- `settlement-config/metadata/`
- `settlement-config/`
- `health/`

### Worker / batch
- management command: `seed_settlement_registry`

### 외부 연동
- 외부/연동 설정: `ORGANIZATION_MASTER_BASE_URL`

## 로컬 실행 방법

- README 기준 개발용 runserver 명령은 명시돼 있지 않다.
- 확인된 컨테이너 진입점은 `entrypoint.sh`이며 `python manage.py migrate --noinput` 후 Gunicorn으로 `0.0.0.0:8000`에 바인드한다.

## 필수 환경변수

- `CSRF_TRUSTED_ORIGINS`
- `DJANGO_ALLOWED_HOSTS`
- `DJANGO_DEBUG`
- `DJANGO_SECRET_KEY`
- `JWT_ALGORITHM`
- `JWT_AUDIENCE`
- `JWT_ISSUER`
- `JWT_SECRET_KEY`
- `ORGANIZATION_MASTER_BASE_URL`
- `POSTGRES_DB`
- `POSTGRES_HOST`
- `POSTGRES_PASSWORD`
- `POSTGRES_PORT`
- `POSTGRES_USER`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push와 수동 실행을 지원한다.
- AWS region: `ap-northeast-2`
- ECR repository: `service-settlement-registry`
- allowed account id: `902837199612`
- 컨테이너 엔트리포인트는 migration 후 Gunicorn 기동 방식이다.

## 운영 시 주의사항

- startup 시 migration을 자동 수행하므로 다중 인스턴스 배포 순서를 점검해야 한다.
- 외부 service base URL 의존성이 있어 upstream 장애 시 일부 API가 실패할 수 있다.

## 확인 필요 항목

- 실제 운영 환경의 배포 manifest와 rollout 절차
- 로컬 개발용 `.env` 템플릿과 표준 실행 절차
