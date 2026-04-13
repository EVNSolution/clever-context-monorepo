# service-account-access

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-account-access`
- 확인 기준 브랜치: `codex/manager-role-scope-assignment`
- 확인한 최근 커밋: `d4c700a6e3ccd6731b1e6b4a2569b6058b3246a7` (`2026-04-13`)

## 서비스 개요

`service-account-access`는 Django/DRF 기반의 계정 출입구와 접근 제어 정본이다. 이 서비스는 `Identity`, 로그인 수단, 비밀번호, JWT 세션, 관리자/기사 계정, 계정-실체 링크, 동의 상태, 가입 요청, 회사별 관리자 역할과 메뉴 접근 정책을 소유한다.

README 기준 포함 범위는 계정/인증/토큰/접근 제어, 로그인/refresh/logout, lockout, 비밀번호 변경, account-driver link helper까지다. 조직 정본, 배송원 profile 정본, 차량/배정 로직, 플랫폼 전체 compose/gateway 설정은 이 repo 범위에 포함되지 않는다.

## 주요 기능

- 신규 `Identity` 생성과 가입 요청 intake
- 이메일/비밀번호 로그인, 소셜 로그인(Kakao), refresh, logout, self recovery
- 필수 동의(개인정보/위치) 저장과 consent recovery 세션 전환
- 이메일/전화/소셜 로그인 수단 추가 및 마지막 로그인 수단 삭제 시 계정/실체 archive
- `SystemAdminAccount`, `ManagerAccount`, `DriverAccount` 생성 및 수명주기 관리
- 관리자 역할(`CompanyManagerRole`) 기본값 보장, 커스텀 역할 생성/수정/삭제, 메뉴 접근 키 정책 관리
- 기사 계정-기사 실체(`DriverAccountLink`) 연결/해제
- Redis 기반 refresh token registry와 로그인 lockout
- 초기 시스템 관리자와 기본 관리자 역할 seed

## 아키텍처와 실행 흐름

1. 공개 엔드포인트가 가입 요청, 로그인, refresh, logout, recovery를 받는다.
2. 인증된 요청은 `accounts.authentication.JWTAuthentication`으로 access token을 검증하고, 기본 권한은 `IsAuthenticated`다.
3. `accounts.views`가 요청을 받고 `accounts.serializers` 검증 후 도메인 서비스로 위임한다.
4. 인증/세션 흐름은 `IdentityAuthService`, `jwt_service`, `RefreshRegistry`, `LockoutService`가 담당한다.
5. 관리자 역할과 범위 흐름은 `CompanyManagerRoleService`, `NavigationPolicyService`, `ManagerAccountScopeService`가 담당한다.
6. 기사 계정 연결은 `DriverAccountLinkService`가 담당하고, 외부 `driver-profile` 조회로 `driver_id -> company_id`를 검증한다.
7. 플릿 범위 검증은 `OrganizationFleetLookupClient`가 외부 조직 서비스의 `/fleets/{fleet_id}/`를 조회해 `company_id` 일치를 검증한다.
8. 런타임 시작 시 컨테이너 엔트리포인트가 `python manage.py migrate --noinput`을 먼저 실행한 뒤 Gunicorn을 기동한다.

## 디렉터리 구조

```text
service-account-access/
├── accounts/
│   ├── management/commands/seed_accounts.py
│   ├── migrations/
│   ├── services/
│   ├── tests/
│   ├── models.py
│   ├── serializers.py
│   ├── urls.py
│   └── views.py
├── config/
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── .github/workflows/build-image.yml
├── Dockerfile
├── entrypoint.sh
├── manage.py
└── requirements.txt
```

## 핵심 엔트리포인트/모듈

- `manage.py`
  - Django 관리 명령 진입점
- `config/settings.py`
  - PostgreSQL, Redis, JWT, CORS/CSRF, 외부 연동 URL, Kakao 설정
- `config/urls.py`
  - 루트 URLConf, 모든 라우트를 `accounts.urls`로 연결
- `accounts/models.py`
  - Identity, 로그인 수단, 계정 타입, 가입 요청, 역할, 링크, 동의/이력 모델
- `accounts/views.py`
  - 인증, 동의, 프로필, 가입 요청, 관리자 역할, 기사 계정 링크 API
- `accounts/services/identity_auth_service.py`
  - 이메일/비밀번호 및 소셜 로그인 인증
- `accounts/services/refresh_registry.py`
  - Redis 기반 refresh token 등록/회전/폐기
- `accounts/services/lockout_service.py`
  - Redis 기반 로그인 실패 누적/잠금
- `accounts/services/company_manager_role_service.py`
  - 회사별 관리자 역할 생성/수정/삭제와 기본 역할 보장
- `accounts/services/manager_account_scope_service.py`
  - 플릿 범위 역할의 fleet assignment 검증/동기화
- `accounts/services/driver_account_link_service.py`
  - 기사 계정과 기사 실체 연결/해제
- `accounts/management/commands/seed_accounts.py`
  - 초기 시스템 관리자 계정과 기본 관리자 역할 bootstrap

## API / worker / batch / 외부 연동

### API

- 공개 또는 비인증 허용
  - `POST /identity-signup-requests/`
  - `POST /identity-login/`
  - `POST /identity-refresh/`
  - `POST /identity-logout/`
  - `POST /identity-recovery/`
  - `GET /health/`
- 인증된 identity/session
  - `GET /identity-me/`
  - `GET|PATCH /identity-profile/`
  - `GET /identity-consent/`
  - `POST /identity-consent/withdraw/`
  - `POST /identity-consent/recover/`
  - `GET|POST /identity-login-methods/`
  - `POST /identity-login-methods/<method_id>/delete/`
  - `PUT /identity-password/`
  - `GET|POST /identity-signup-requests/me/`
  - `POST /identity-signup-requests/<request_id>/cancel/`
  - `GET /identity-navigation-policy/`
- 관리자/운영 계정 관리
  - `GET /identity-signup-requests/manage/`
  - `POST /identity-signup-requests/<request_id>/approve/`
  - `POST /identity-signup-requests/<request_id>/reject/`
  - `POST /identity-signup-requests/<request_id>/complete-setup/`
  - `GET /manager-accounts/manage/`
  - `POST /manager-accounts/<manager_account_id>/change-role/`
  - `POST /manager-accounts/<manager_account_id>/archive/`
  - `GET|POST /company-manager-roles/`
  - `PATCH|DELETE /company-manager-roles/<role_id>/`
  - `GET /driver-accounts/manage/`
  - `GET|POST /driver-account-links/`
  - `POST /driver-account-links/<link_id>/unlink/`

### Worker / batch

- 별도 상시 worker 구현은 확인되지 않았다.
- 배치/운영 명령으로 `python manage.py seed_accounts`가 존재한다.

### 외부 연동

- PostgreSQL
  - 기본 DB 엔진은 `django.db.backends.postgresql`
- Redis
  - refresh token registry와 로그인 lockout 저장소
- `service-driver-profile`
  - `DRIVER_PROFILE_BASE_URL`을 통해 기사의 `company_id` 조회
- 조직 서비스
  - `ORGANIZATION_MASTER_BASE_URL`을 통해 플릿의 `company_id` 조회
- Kakao
  - `KAKAO_USER_INFO_URL`과 API key/secret 기반 소셜 로그인 subject 조회

## 로컬 실행 방법

- README에는 로컬 실행 절차가 상세히 적혀 있지 않다.
- 코드에서 확인되는 최소 실행 경로
  - Python 3.12 환경 준비
  - `pip install -r requirements.txt`
  - PostgreSQL과 Redis 준비
  - 필수 환경변수 설정
  - `python manage.py migrate`
  - 필요 시 `python manage.py seed_accounts`
  - Django 관리 명령 기반으로 실행 가능 (`manage.py` 존재)
- 컨테이너 기본 실행 경로
  - 이미지 시작 시 `entrypoint.sh`가 `python manage.py migrate --noinput` 후 Gunicorn(`config.wsgi:application`)을 `0.0.0.0:8000`에 바인드한다.

## 필수 환경변수

- Django 기본
  - `DJANGO_SECRET_KEY`
  - `DJANGO_DEBUG`
  - `DJANGO_ALLOWED_HOSTS`
  - `CSRF_TRUSTED_ORIGINS`
- PostgreSQL
  - `POSTGRES_DB`
  - `POSTGRES_USER`
  - `POSTGRES_PASSWORD`
  - `POSTGRES_HOST`
  - `POSTGRES_PORT`
- JWT / 세션
  - `JWT_SECRET_KEY`
  - `JWT_ISSUER`
  - `JWT_AUDIENCE`
  - `JWT_ALGORITHM`
  - `ACCESS_TOKEN_LIFETIME_MINUTES`
  - `REFRESH_TOKEN_LIFETIME_DAYS`
  - `REFRESH_COOKIE_NAME`
- Redis / lockout
  - `REDIS_URL`
  - `LOGIN_LOCKOUT_THRESHOLD`
  - `LOGIN_LOCKOUT_TTL_SECONDS`
- 외부 서비스
  - `DRIVER_PROFILE_BASE_URL`
  - `ORGANIZATION_MASTER_BASE_URL`
  - `KAKAO_REST_API_KEY`
  - `KAKAO_CLIENT_SECRET`
  - `KAKAO_USER_INFO_URL`
- seed/bootstrap
  - `SEED_ADMIN_EMAIL`
  - `SEED_ADMIN_PASSWORD`
  - `SEED_MANAGER_COMPANY_ID`
- Gunicorn
  - `GUNICORN_WORKERS`
  - `GUNICORN_TIMEOUT`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push 시 Docker 이미지를 빌드하고 AWS ECR에 push한다.
- 확인된 ECR 설정
  - region: `ap-northeast-2`
  - repository: `service-account-access`
  - account id: `902837199612`
- 컨테이너 런타임은 이미지 시작 시 DB migration을 자동 실행한 뒤 Gunicorn으로 서비스를 띄운다.
- 실제 배포 orchestration, 서비스 디스커버리, runtime manifests는 이 repo에 없다. 중앙 배포 repo 또는 운영 문서 기준 확인이 필요하다.

## 운영 시 주의사항

- `DJANGO_SECRET_KEY`, `JWT_SECRET_KEY` 기본값이 모두 `change-me`이므로 운영 환경에서는 반드시 override해야 한다.
- `CORS_ALLOW_ALL_ORIGINS = True`가 코드에 고정되어 있어 운영 환경 검토가 필요하다.
- startup 시 migration을 자동 실행하므로 다중 인스턴스 롤아웃 시 순서/락 전략을 확인해야 한다.
- Redis가 없으면 refresh token registry와 login lockout 기능이 동작하지 않는다.
- `DriverProfileCompanyLookupClient`, `OrganizationFleetLookupClient`는 외부 서비스 응답의 `company_id`에 의존하므로 연동 장애 시 관리자/기사 계정 작업이 실패할 수 있다.
- 마지막 로그인 수단 삭제 시 identity와 연결된 계정이 archive되고 관련 세션이 revoke된다.

## 확인 필요 항목

- 현재 대상 서비스가 실제로 `service-account-access`가 맞는지 사용자 확인 필요
- 로컬 개발 표준 실행 명령과 `.env` 템플릿
- 실제 배포 대상 환경, 중앙 배포 repo, rollout 절차
- OpenAPI schema endpoint 노출 여부
- Redis/DB 운영 정책(백업, TLS, 장애 조치)
