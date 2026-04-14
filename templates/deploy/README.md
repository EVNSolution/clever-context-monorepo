# deploy template baseline

## 목적

이 디렉터리는 공통 deploy baseline을 반복 가능하게 쓰기 위한 자산 모음이다.

정본 규칙은 `docs/root/deploy-template-governance.md`와 `docs/root/central-deploy-runtime-current-truth.md`를 따른다.

## 포함 자산

- `checklist.md`: 배포 전/후 공통 확인 항목
- `env-template.example`: env 구조 예시
- `override-guide.md`: customer-specific override 허용 범위

## baseline과 override 경계

- baseline은 모든 서비스가 공유하는 deploy 규칙과 최소 구조다.
- override는 특정 서비스나 고객사에서만 달라지는 값과 rollout 차이다.
- override는 baseline을 대체하지 않고, baseline 위에 덧붙는 것으로 기록한다.
