# 중앙 배포 런타임 기준

## 목적

이 문서는 `clever-deploy-control` 기준 중앙 배포의 현재 런타임 truth만 다룬다.

2026-04-12 기준으로 fresh prod app-host에서 실제로 검증한 내용만 남긴다.

## 배포 진입점

- image-backed 서비스는 service repo에서 이미지를 만들고, 실제 배포는 `clever-deploy-control`에서 한다.
- 배포 기준 repo는 `main`이다.
- 대상 bundle은 `catalog/services.yaml` 기준으로 계산한다.

## fresh host 기준

새 prod app-host 또는 비어 있는 host에서 중앙 배포를 시작할 때는 아래를 전제로 본다.

- app-host provisioning과 SSM online 확인이 먼저 끝나야 한다.
- host에 남아 있는 과거 `.deploy-image-state.env`를 전제로 삼지 않는다.
- compose interpolation에 필요한 `*_IMAGE` 값은 host local state가 아니라 `catalog/services.yaml`과 ECR 최신 태그에서 다시 채울 수 있어야 한다.

## 원격 git 인증 기준

- host 기본 git read token이 새 private repo 접근 권한을 아직 갖지 못할 수 있다.
- 이 경우 중앙 배포 workflow secret `GH_ACTIONS_REMOTE_REPO_READ_TOKEN` 으로 remote clone 인증을 override 한다.
- 새 서비스 repo를 rollout bundle에 넣었는데 host clone 단계에서 `403`이 나오면, host bootstrap보다 먼저 이 secret과 권한 범위를 확인한다.

## image env 기준

- deploy compose가 bundle 대상 외 서비스의 `*_IMAGE` 환경변수까지 요구할 수 있다.
- fresh host에서 `.deploy-image-state.env`가 비어 있으면 대상 서비스와 무관한 image interpolation 오류로 wave 전체가 실패할 수 있다.
- 따라서 중앙 배포는 compose up 전에 `catalog/services.yaml`에 등록된 image-backed 서비스들의 `image_env`, `image_repository`, `image_registry_region`, `image_registry_account`를 읽고, 각 ECR 최신 태그로 `.deploy-image-state.env`를 먼저 채운다.
- `required variable ... is missing a value` 류 오류는 우선 대상 서비스 버그가 아니라 host image-state bootstrap 누락으로 본다.

## 검증 기준

- 중앙 배포 성공만으로 끝내지 않는다.
- 실제 원격 사이트는 public domain에서 다시 연다.
- 최소 검증은 아래 세 가지다.
  - 진입 URL이 200으로 열린다.
  - 브라우저 콘솔 error가 없다.
  - 핵심 초기 API 요청이 200으로 응답한다.

## 제외

- 서비스별 business smoke 시나리오
- local verification 모드 선택 규칙
- 서비스 내부 테스트 절차
