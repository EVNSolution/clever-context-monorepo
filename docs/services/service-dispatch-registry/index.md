# service-dispatch-registry

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-dispatch-registry`
- 확인 기준 브랜치: `main`
- 확인한 최근 커밋: `5344a8b389822b4669a3d5edfb71f40fdfb00bf0 2026-04-12 fix: send household count to attendance sync`

## 서비스 개요

배차 계획 정본을 소유하는 Django/DRF runtime repo다.

## 주요 기능

- `dispatch_plan`, `vehicle_schedule`, `dispatch_assignment` 1차 계획 truth
- `fleet + dispatch_date` 물량 계획
- `vehicle + dispatch_date + shift_slot` 차량 슬롯 계획
- `vehicle + driver + dispatch_date + shift_slot` 계획 배정

## 아키텍처와 실행 흐름

- `config/urls.py`가 서비스 앱 URLConf로 요청을 연결한다.
- 핵심 앱 코드는 `dispatch/` 아래에 있고 모델, API, 테스트를 함께 둔다.
- `dispatch/services/`가 도메인 조합과 외부 연동 helper를 담당한다.
- DRF 기본 인증/권한 설정이 `config/settings.py`에 정의돼 있다.
- 컨테이너 시작 시 migration 후 Gunicorn WSGI 앱을 기동한다.
- health endpoint가 존재한다.

## 디렉터리 구조

```text
service-dispatch-registry/
├── config/
├── dispatch/
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
- `dispatch/`: 서비스 핵심 앱 디렉터리
- `dispatch/urls.py`: 내부 API route 정의
- `dispatch/services/`: 도메인 조합 및 외부 연동 helper
- `dispatch/tests/`: 서비스 테스트

## API / worker / batch / 외부 연동

### API
- `/`
- `health/`
- `plans/`
- `plans/<uuid:dispatch_plan_id>/`
- `upload-batches/`
- `upload-batches/preview/`
- `upload-batches/<uuid:upload_batch_id>/confirm/`
- `outsourced-drivers/`
- `outsourced-drivers/<uuid:outsourced_driver_id>/`
- `work-rules/`
- `work-rules/<uuid:work_rule_id>/`
- `driver-day-exceptions/`
- `vehicle-schedules/`
- `assignments/`

### Worker / batch
- management command: `import_ops_fixture`
- management command: `seed_dispatch`

### 외부 연동
- 외부/연동 설정: `ATTENDANCE_REGISTRY_BASE_URL`
- 외부/연동 설정: `DELIVERY_RECORD_BASE_URL`
- 외부/연동 설정: `DRIVER_PROFILE_BASE_URL`
- 외부/연동 설정: `VEHICLE_REGISTRY_BASE_URL`

## 로컬 실행 방법

- `../../development/integration-local-stack/docker-compose.account-driver-settlement.yml`
- gateway route: `/api/dispatch/`

## 필수 환경변수

- `ATTENDANCE_REGISTRY_BASE_URL`
- `CSRF_TRUSTED_ORIGINS`
- `DELIVERY_RECORD_BASE_URL`
- `DJANGO_ALLOWED_HOSTS`
- `DJANGO_DEBUG`
- `DJANGO_SECRET_KEY`
- `DRIVER_PROFILE_BASE_URL`
- `JWT_ALGORITHM`
- `JWT_AUDIENCE`
- `JWT_ISSUER`
- `JWT_SECRET_KEY`
- `POSTGRES_DB`
- `POSTGRES_HOST`
- `POSTGRES_PASSWORD`
- `POSTGRES_PORT`
- `POSTGRES_USER`
- `VEHICLE_REGISTRY_BASE_URL`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push와 수동 실행을 지원한다.
- AWS region: `ap-northeast-2`
- ECR repository: `service-dispatch-registry`
- allowed account id: `902837199612`
- 컨테이너 엔트리포인트는 migration 후 Gunicorn 기동 방식이다.

## 운영 시 주의사항

- startup 시 migration을 자동 수행하므로 다중 인스턴스 배포 순서를 점검해야 한다.
- 외부 service base URL 의존성이 있어 upstream 장애 시 일부 API가 실패할 수 있다.

## 확인 필요 항목

- 실제 운영 환경의 배포 manifest와 rollout 절차
- 로컬 개발용 `.env` 템플릿과 표준 실행 절차
