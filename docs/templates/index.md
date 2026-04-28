# 템플릿 Registry

## 목적

이 문서는 완료된 template entry의 registry다.

작업 중 scaffold 계획, runtime proof, 서비스별 채택 결과, 소유 repo 문서 요약은
이 registry에 두지 않는다.

## Registry 규칙

각 template entry는 최소 아래 값을 가진다.

- `template_id`
- `version`
- `status`
- `use_case`
- `deploy_profile`
- `summary`
- `allowed_override_boundary`
- `known_constraints`

## 등록된 템플릿

### `Clever-OIDC-deploy`

- status: `recommended`
- latest version: `v1`
- use_case: deploy lineage for image artifact and central release separation
- deploy_profile: `image-build-once-central-release`
- summary: reusable deploy template contract for build/release/inventory separation
- 문서:
  - [`docs/templates/Clever-OIDC-deploy/index.md`](./Clever-OIDC-deploy/index.md)
  - [`docs/templates/Clever-OIDC-deploy/versions/v1.md`](./Clever-OIDC-deploy/versions/v1.md)

### `msa-template`

- status: `candidate`
- latest version: `v1`
- use_case: MSA umbrella development repository template lineage
- deploy_profile: `not-applicable-development-template`
- summary: development repo structure, docs authority, and runtime slice boundary template for MSA product services
- template_repository: `https://github.com/EVNSolution/clever-msa-repo-template`
- 문서:
  - [`docs/templates/msa-template/index.md`](./msa-template/index.md)
  - [`docs/templates/msa-template/versions/v1.md`](./msa-template/versions/v1.md)

### `test-erik-project-template`

- status: `recommended`
- latest version: `v1`
- use_case: AI-assisted full-stack monorepo bootstrap
- deploy_profile: `preview-dev-prod-monorepo`
- summary: opinionated full-stack monorepo template entry
- 문서:
  - [`docs/templates/test-erik-project-template/index.md`](./test-erik-project-template/index.md)
  - [`docs/templates/test-erik-project-template/versions/v1.md`](./test-erik-project-template/versions/v1.md)

## 관련 문서

- [`docs/root/template-harness-governance.md`](../root/template-harness-governance.md)
- [`docs/root/deploy-template-governance.md`](../root/deploy-template-governance.md)
