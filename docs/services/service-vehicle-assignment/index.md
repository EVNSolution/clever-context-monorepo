# service-vehicle-assignment

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-vehicle-assignment`
- 확인 기준 브랜치: `main`
- 확인한 최근 커밋: `bc65116739d3d78849c546ea185bccae5bbe7674 2026-04-08 feat: enforce navigation policy in assignment APIs`

## 서비스 개요

이 repo는 운영사의 차량 `assignment` 정본을 소유한다.

## 주요 기능

- 배송원-차량 배정과 해제
- 운영사 기준 assignment lifecycle
- 이후 handover workflow가 붙을 중심축

## 아키텍처와 실행 흐름

- `config/urls.py`가 서비스 앱 URLConf로 요청을 연결한다.
- 핵심 앱 코드는 `assignments/` 아래에 있고 모델, API, 테스트를 함께 둔다.
- `assignments/services/`가 도메인 조합과 외부 연동 helper를 담당한다.
- DRF 기본 인증/권한 설정이 `config/settings.py`에 정의돼 있다.
- 컨테이너 시작 시 migration 후 Gunicorn WSGI 앱을 기동한다.
- health endpoint가 존재한다.

## 디렉터리 구조

```text
service-vehicle-assignment/
├── config/
├── assignments/
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
- `assignments/`: 서비스 핵심 앱 디렉터리
- `assignments/urls.py`: 내부 API route 정의
- `assignments/services/`: 도메인 조합 및 외부 연동 helper
- `assignments/tests/`: 서비스 테스트

## API / worker / batch / 외부 연동

### API
- `/`
- `assignments/`
- `health/`

### Worker / batch
- management command: `import_ops_fixture`
- management command: `seed_assignments`

### 외부 연동
- 외부/연동 설정: `DRIVER_PROFILE_BASE_URL`
- 외부/연동 설정: `VEHICLE_ASSET_BASE_URL`

## 로컬 실행 방법

- README 기준 개발용 runserver 명령은 명시돼 있지 않다.
- 확인된 컨테이너 진입점은 `entrypoint.sh`이며 `python manage.py migrate --noinput` 후 Gunicorn으로 `0.0.0.0:8000`에 바인드한다.

## 필수 환경변수

- `CSRF_TRUSTED_ORIGINS`
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
- `VEHICLE_ASSET_BASE_URL`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push와 수동 실행을 지원한다.
- AWS region: `ap-northeast-2`
- ECR repository: `service-vehicle-assignment`
- allowed account id: `902837199612`
- 컨테이너 엔트리포인트는 migration 후 Gunicorn 기동 방식이다.

## 운영 시 주의사항

- startup 시 migration을 자동 수행하므로 다중 인스턴스 배포 순서를 점검해야 한다.
- 외부 service base URL 의존성이 있어 upstream 장애 시 일부 API가 실패할 수 있다.

## 확인 필요 항목

- 실제 운영 환경의 배포 manifest와 rollout 절차
- 로컬 개발용 `.env` 템플릿과 표준 실행 절차
- gateway 외부 prefix 또는 edge 라우팅 연결 여부
