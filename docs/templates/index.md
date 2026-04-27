# 템플릿 registry 시작점

## 목적

이 문서는 CLEVER가 채택 가능한 템플릿의 registry 시작점이다.

정본 판단은 각 템플릿 문서와 root 거버넌스 문서로 되돌아간다.

## registry 규칙

각 템플릿 entry는 최소 아래 값을 가진다.

- `template_id`
- `version`
- `status`
- `use_case`
- `deploy_profile`
- `summary`
- `bootstrap_rules`
- `allowed_override_boundary`
- `known_constraints`

추가 세부값은 템플릿별 version 문서에서 관리한다.

## 등록된 템플릿

### `Clever-OIDC-deploy`

- status: `recommended`
- latest version: `v1`
- use_case: current CLEVER deploy baseline for monorepo and MSA
- deploy_profile: `image-build-once-central-release`
- summary: current CLEVER deploy truth elevated as a reusable template family
- note: current deploy baseline을 template lineage로 기록하기 위한 family entry다. monorepo는 single workload, MSA는 multiple workloads로 읽되, 둘 다 같은 release/inventory/contract probe 모델을 따른다.
- 문서:
  - [`docs/templates/Clever-OIDC-deploy/index.md`](./Clever-OIDC-deploy/index.md)
  - [`docs/templates/Clever-OIDC-deploy/versions/v1.md`](./Clever-OIDC-deploy/versions/v1.md)

### `msa-template`

- status: `candidate`
- latest version: `v1`
- use_case: package-style MSA template family registry
- deploy_profile: `image-build-once-and-promote`
- summary: 운영, 인프라, 프론트, 게이트웨이, 서비스 archetype을 묶어 설명하는 generic MSA template family 문서
- note: family 구조와 archetype 조합을 설명하는 entry다. current deploy baseline 자체를 설명하는 역할은 `Clever-OIDC-deploy` family가 우선 가진다.
- 문서:
  - [`docs/templates/msa-template/index.md`](./msa-template/index.md)
  - [`docs/templates/msa-template/versions/v1.md`](./msa-template/versions/v1.md)

### `test-erik-project-template`

- status: `recommended`
- latest version: `v1`
- use_case: AI-assisted full-stack monorepo bootstrap
- deploy_profile: `preview-dev-prod-monorepo`
- summary: Next.js, Fastify, Prisma, AWS CDK/ECS Fargate 기반의 opinionated monorepo 템플릿
- note: monorepo scaffold와 infra posture를 함께 설명하는 별도 템플릿이다. current CLEVER deploy baseline family와는 목적이 다르다.
- 문서:
  - [`docs/templates/test-erik-project-template/index.md`](./test-erik-project-template/index.md)
  - [`docs/templates/test-erik-project-template/versions/v1.md`](./test-erik-project-template/versions/v1.md)

## 관련 문서

- `docs/root/template-harness-governance.md`
- `docs/root/deploy-template-governance.md`
- `docs/services/service-template.md`
