# test-erik-project-template

## 목적

이 문서는 `TEST-Erik-project-template`를 CLEVER template registry에 편입한 entry다.

upstream source는 아래 repo다.

- `https://github.com/EVNSolution/TEST-Erik-project-template`

## registry 요약

- `template_id`: `test-erik-project-template`
- `current_version`: `v1`
- `status`: `recommended`
- `use_case`: AI-assisted full-stack monorepo bootstrap
- `deploy_profile`: `preview-dev-prod-monorepo`
- `summary`: Next.js, Fastify, Prisma, AWS CDK 기반의 opinionated monorepo template
- `bootstrap_rules`: monorepo 구조, preview/dev/prod workflow, shared package와 infra baseline을 시작점으로 삼는다.
- `allowed_override_boundary`: EVS 도메인 naming, placeholder env/secret, vendor-specific review automation은 CLEVER 기준으로 치환 또는 제외한다.
- `known_constraints`: AWS, ECS Fargate, OIDC, branch flow, preview subdomain 패턴 전제가 강하다.

## 채택 적합 조건

아래에 가까우면 이 템플릿을 우선 후보로 본다.

- 새 프로젝트를 monorepo로 시작하려는 경우
- frontend/BFF, auth/service, worker, shared package를 함께 가져가려는 경우
- preview/dev/prod automation을 초기에 같이 설계하려는 경우
- infra와 앱 구조를 같은 템플릿 계열 안에서 시작하려는 경우

## 채택 제외 또는 주의 조건

아래에 가까우면 다른 템플릿을 먼저 검토한다.

- 단일 service repo나 작은 runtime 하나만 필요한 경우
- AWS CDK/ECS Fargate 전제를 두기 어려운 경우
- Claude Code/Anthropic 기반 workflow를 그대로 채택하지 않을 경우
- EVS 도메인 구조와 naming을 그대로 가져오면 오히려 혼선이 생기는 경우

## CLEVER에서 채택하는 범위

- monorepo layout 개념: `services`, `packages`, `infra`, `docs`, `specs`
- shared package 중심 구조
- preview/dev/prod 배포 단계 분리
- migration, db sync, QA trigger 같은 운영 workflow 개념
- runbook/spec-first 문서 흐름

## CLEVER에서 채택하지 않는 범위

- EVS 도메인 전용 naming과 sample content
- 환경별 placeholder 값과 샘플 ARN/URL
- CLEVER가 그대로 쓰지 않는 vendor-specific automation 세부
- 예제 route나 예제 business rule

## 버전 문서

- [`v1`](./versions/v1.md)
