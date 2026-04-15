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

### `msa-template`

- status: `candidate`
- latest version: `v1`
- use_case: package-style MSA template family registry
- deploy_profile: `image-build-once-and-promote`
- summary: 운영, 인프라, 프론트, 게이트웨이, 서비스 archetype을 묶어 설명하는 generic MSA template family 문서
- note: 현재는 family 설명용 registry entry이며, GitHub template repo 또는 concrete scaffold가 준비되기 전까지는 에이전트 직접 선택 대상이 아니다.
- 문서:
  - [`docs/templates/msa-template/index.md`](./msa-template/index.md)
  - [`docs/templates/msa-template/versions/v1.md`](./msa-template/versions/v1.md)

### `test-erik-project-template`

- status: `recommended`
- latest version: `v1`
- use_case: AI-assisted full-stack monorepo bootstrap
- deploy_profile: `preview-dev-prod-monorepo`
- summary: Next.js, Fastify, Prisma, AWS CDK/ECS Fargate 기반의 opinionated monorepo 템플릿
- 문서:
  - [`docs/templates/test-erik-project-template/index.md`](./test-erik-project-template/index.md)
  - [`docs/templates/test-erik-project-template/versions/v1.md`](./test-erik-project-template/versions/v1.md)

## 관련 문서

- `docs/root/template-harness-governance.md`
- `docs/root/deploy-template-governance.md`
- `docs/services/service-template.md`
