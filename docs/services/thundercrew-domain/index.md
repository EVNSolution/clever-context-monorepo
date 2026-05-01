# thundercrew-domain Context Pointer

## Purpose

This file records the canonical context pointer for the `thundercrew-domain`
product service. It does not copy runtime proof, secrets, environment values, or
release evidence from the target repository.

## Registry

| Field | Value |
| --- | --- |
| service_id | `thundercrew-domain` |
| owner_repo | `EVNSolution/thundercrew-domain` |
| product_scope | electric two-wheeler control and operations management web service |
| current_mvp_anchor | `EVNSolution/thundercrew-domain#106`, `EVNSolution/clever-change-control#98` |
| context_issue | `EVNSolution/clever-context-monorepo#21` |
| target_runtime_docs | `EVNSolution/thundercrew-domain:README.md`, `docs/deployment/` |
| deploy_lineage | `Clever-OIDC-deploy` with existing-EC2 host-update override |
| workspace_lineage | `msa-template` boundary principles, adapted to the target repo's current `development/front-admin-web` and `development/service-ops-api` slices |

## Interpretation

- The service follows the CLEVER convention that target repositories own runtime
  source, runtime evidence, and operational proof.
- This context repository owns only the interpretation pointer: which target repo
  is canonical, which template family is relevant, and which issue anchors the
  current MVP completion state.
- The current deployment path uses GitHub OIDC for AWS authority, but the
  runtime update is an existing EC2 host update rather than the
  `Clever-OIDC-deploy` image-build-once central release profile. That difference
  is an explicit service-level override, not a template registry mutation.
- The MSA template is used as a boundary guideline. The target repo keeps
  frontend and backend runtime slices under `development/`; detailed API, DB,
  environment, and deployment proof remain in the target repository.

## Do not store here

- DB passwords, JWT secrets, service-role keys, SSH keys, or connection strings
- EC2 host private access material
- Full API contracts or runtime inventories copied from the target repository
- Per-run deployment evidence

## Source-of-truth pointers

- Target repository README: `EVNSolution/thundercrew-domain:README.md`
- AWS EC2/EBS baseline: `EVNSolution/thundercrew-domain:docs/deployment/aws-ec2-ebs-deployment.md`
- Main-merge deploy workflow: `EVNSolution/thundercrew-domain:.github/workflows/aws-ec2-deploy.yml`
- Main-merge deploy docs: `EVNSolution/thundercrew-domain:docs/deployment/aws-ec2-main-merge-deploy.md`
