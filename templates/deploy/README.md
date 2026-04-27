# deploy template baseline

## 목적

이 디렉터리는 공통 deploy baseline을 반복 가능하게 쓰기 위한 자산 모음이다.

이 자산은 `Clever-OIDC-deploy` family가 설명하는 current deploy baseline을 반복 가능하게 쓰기 위한 boilerplate다.

정본 규칙은 아래 순서로 읽는다.

1. `docs/root/deploy-template-governance.md`
2. `docs/root/central-deploy-runtime-current-truth.md`
3. `docs/templates/Clever-OIDC-deploy/versions/v1.md`

## 포함 자산

- `checklist.md`: 배포 전/후 공통 확인 항목
- `env-template.example`: env 구조 예시
- `override-guide.md`: customer-specific override 허용 범위

## 이 자산이 전제하는 deploy model

이 자산은 아래 모델을 전제로 한다.

- app repo는 image build만 담당
- central release가 실제 deploy를 수행
- runtime inventory가 workload scope를 고정
- public contract probe까지 완료돼야 release를 닫는다

## monorepo와 MSA 해석

이 자산은 monorepo와 MSA를 같은 deploy model로 읽는다.

- monorepo: single workload
- MSA: multiple workloads

차이는 workload cardinality뿐이다.

즉 monorepo와 MSA 모두 아래는 같다.

- immutable image artifact 기준
- release ownership 분리
- runtime inventory authority
- env/secret category 관리
- contract probe 필수

## baseline과 override 경계

- baseline은 모든 서비스가 공유하는 deploy 규칙과 최소 구조다.
- override는 특정 서비스나 고객사에서만 달라지는 값과 rollout 차이다.
- override는 baseline을 대체하지 않고, baseline 위에 덧붙는 것으로 기록한다.

## current truth와 자산의 관계

이 디렉터리는 운영 truth 자체를 대체하지 않는다.

- current runtime truth는 root 문서에서 읽는다.
- 이 디렉터리는 그 truth를 서비스 문서와 template lineage에서 반복 가능하게 재사용하는 예시 자산이다.
- 서비스별 caveat와 concrete value는 서비스 문서에서 보강한다.
