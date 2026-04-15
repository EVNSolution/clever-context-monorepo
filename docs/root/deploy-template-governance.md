# 배포 템플릿 거버넌스

## 목적

이 문서는 서비스 문서마다 반복되는 배포 baseline을 root 기준으로 끌어올린 정본이다.

이 문서는 아래를 공통 기준으로 삼는다.

- image build와 central deploy의 역할 분리
- env/secret 분리
- rollout/rollback 기본 체크
- post-deploy public contract probe
- customer-specific override 경계

## SSOT 경계

- 현재 중앙 배포 런타임 truth는 `docs/root/central-deploy-runtime-current-truth.md`를 우선 기준으로 본다.
- 서비스 문서는 공통 배포 절차를 다시 쓰지 않고, 서비스 고유 차이만 적는다.
- `templates/deploy/`는 root 규칙을 재사용하기 위한 boilerplate와 예시 자산을 둔다.

## 공통 deploy baseline

### build와 deploy의 역할

- image-backed 서비스는 service repo에서 이미지를 만들고, 실제 배포는 중앙 배포 기준 repo에서 수행한다.
- 배포 진입점과 fresh host 가정은 `docs/root/central-deploy-runtime-current-truth.md`를 따른다.
- 서비스 문서는 build 자체를 설명하기보다, 해당 서비스의 deploy profile과 예외만 적는다.

### env와 secret

- 공통 baseline은 env 구조와 secret 분리를 template 자산에서 제공한다.
- 실제 값은 service 문서나 root 문서에 직접 박지 않는다.
- customer-specific divergence는 `override_scope`와 override guide 기준으로만 설명한다.

### rollout과 rollback

- root 문서는 공통 rollout/rollback 체크를 정의한다.
- 서비스 문서는 해당 서비스만의 startup caveat, migration 주의점, gateway prefix, unique rollout risk만 적는다.

### post-deploy verification

- 배포 완료 판단은 build success와 deploy success만으로 끝내지 않는다.
- public URL에서 contract probe를 다시 확인해야 한다.
- 공통 기준은 root 문서가 가진다. 서비스 문서는 probe가 필요한 구체 endpoint나 caveat만 추가한다.

## 템플릿이 막아야 하는 오류와 못 막는 오류

배포 템플릿은 모든 오류를 없애는 장치가 아니다. 대신 반복되는 배포 class 오류를 줄이는 정본이다.

핵심 기준은 아래다.

- 앱 내부 변화가 deploy contract를 바꾸지 않으면, 배포 템플릿은 그 변화를 그대로 수용해야 한다.
- 앱 내부 변화가 deploy contract를 바꾸면, 그 순간 그건 더 이상 pure app-only change가 아니다.

여기서 deploy contract는 아래를 뜻한다.

- image build와 deploy의 역할 분리
- immutable artifact 사용
- env/secret category
- health check path
- port와 upstream name
- startup ordering과 bootstrap prerequisite
- resource sizing 전제

즉 아래 같은 변화는 app-only change로 본다.

- UI 텍스트, 레이아웃, 화면 내부 로직
- API handler 내부 계산식
- 기존 env와 port 안에서의 business rule 변경
- 기존 DB schema와 runtime contract를 안 건드리는 내부 refactor

반대로 아래는 deploy-affecting change로 본다.

- 새 env 또는 secret 추가
- health path 변경
- port 또는 upstream name 변경
- image architecture 변경
- startup order 의존성 추가
- DB bootstrap, migration, seed prerequisite 추가
- host size 또는 runtime lane 크기 전제 변경

첫 번째 범주의 변화는 템플릿이 반복 오류 없이 흡수해야 한다. 두 번째 범주의 변화는 템플릿/거버넌스/서비스 문서를 같이 바꾸지 않으면 다시 배포 오류가 생길 수 있다.

따라서 정직한 약속은 아래다.

- 배포 템플릿은 반복적인 deploy wiring 실수를 줄이는 데 책임이 있다.
- 템플릿이 있다고 해서 app contract 변화까지 자동으로 안전해지는 것은 아니다.
- contract 변화가 생기면 template update 또는 migration 기록이 같이 따라와야 한다.

## root 문서와 service 문서의 경계

### root에 둘 것

- 중앙 배포의 공통 흐름
- remote git auth, image-state bootstrap, build context cleanup 같은 공통 runtime 기준
- deploy success 후 contract probe 필수 규칙
- template asset 사용법과 override 경계

### service 문서에 둘 것

- 서비스 고유 deploy profile
- 서비스 고유 env/secret 범주
- startup 시 migration이나 batch worker 같은 서비스 고유 caveat
- customer-specific override 중 이 서비스에서만 의미 있는 차이

## template asset 사용 규칙

`templates/deploy/`는 아래 자산으로 구성한다.

- `README.md`: baseline과 override의 경계 설명
- `checklist.md`: 배포 전/후 최소 확인 체크
- `env-template.example`: 구조 예시
- `override-guide.md`: 허용되는 customer-specific divergence 범위

이 자산은 예시와 반복 가능한 구조를 제공하지만, 운영 truth 자체를 대체하지 않는다.

## customer-specific override 경계

customer-specific override는 아래처럼 나눠 적는다.

- image/tag 차이
- env 차이
- secret 차이
- 외부 연동 차이
- rollout 단위 차이

공통 baseline을 바꾸는 변경이면 service 문서만 수정하지 않고 root 문서까지 같이 본다.

## 관련 문서

- `docs/root/central-deploy-runtime-current-truth.md`
- `docs/root/msa-saas-replication-governance.md`
- `docs/services/service-template.md`
- `templates/deploy/README.md`
