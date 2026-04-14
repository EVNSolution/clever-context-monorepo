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
