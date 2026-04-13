# service-attendance-registry

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-attendance-registry`
- 확인 기준 브랜치: `main`
- 확인한 최근 커밋: `007287234654985df552bc53a257e71806fc3eb2 2026-04-12 fix: derive no-delivery attendance from zero counts`

## 서비스 개요

이 repo는 CLEVER MSA의 `기사 x 일자` 근태 truth를 소유하는 Django/DRF runtime repo다.

## 주요 기능

- `attendance-registry-api` runtime
- dispatch-derived attendance signal sync
- daily attendance truth 조회와 internal bulk lookup

## 아키텍처와 실행 흐름

- `config/urls.py`가 서비스 앱 URLConf로 요청을 연결한다.
- 핵심 앱 코드는 `attendanceregistry/` 아래에 있고 모델, API, 테스트를 함께 둔다.
- `attendanceregistry/services/`가 도메인 조합과 외부 연동 helper를 담당한다.
- DRF 기본 인증/권한 설정이 `config/settings.py`에 정의돼 있다.
- 컨테이너 시작 시 migration 후 Gunicorn WSGI 앱을 기동한다.
- health endpoint가 존재한다.

## 디렉터리 구조

```text
service-attendance-registry/
├── config/
├── attendanceregistry/
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
- `attendanceregistry/`: 서비스 핵심 앱 디렉터리
- `attendanceregistry/urls.py`: 내부 API route 정의
- `attendanceregistry/services/`: 도메인 조합 및 외부 연동 helper
- `attendanceregistry/tests/`: 서비스 테스트

## API / worker / batch / 외부 연동

### API
- `/`
- `days/`
- `days/<uuid:attendance_day_id>/`
- `internal/dispatch-signals:sync/`
- `internal/days:bulk-lookup/`
- `health/`

### Worker / batch
- 확인된 별도 worker/batch command 없음

### 외부 연동
- 외부 연동은 확인 필요다.

## 로컬 실행 방법

- `. .venv/bin/activate && python manage.py runserver 0.0.0.0:8000`
- `. .venv/bin/activate && python manage.py test -v 2`

## 필수 환경변수

- `CSRF_TRUSTED_ORIGINS`
- `DJANGO_ALLOWED_HOSTS`
- `DJANGO_DEBUG`
- `DJANGO_SECRET_KEY`
- `JWT_ALGORITHM`
- `JWT_AUDIENCE`
- `JWT_ISSUER`
- `JWT_SECRET_KEY`
- `POSTGRES_DB`
- `POSTGRES_HOST`
- `POSTGRES_PASSWORD`
- `POSTGRES_PORT`
- `POSTGRES_USER`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push와 수동 실행을 지원한다.
- AWS region: `ap-northeast-2`
- ECR repository: `service-attendance-registry`
- allowed account id: `902837199612`
- 컨테이너 엔트리포인트는 migration 후 Gunicorn 기동 방식이다.

## 운영 시 주의사항

- startup 시 migration을 자동 수행하므로 다중 인스턴스 배포 순서를 점검해야 한다.

## 확인 필요 항목

- 실제 운영 환경의 배포 manifest와 rollout 절차
- 로컬 개발용 `.env` 템플릿과 표준 실행 절차
- gateway 외부 prefix 또는 edge 라우팅 연결 여부
