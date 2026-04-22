# Clever-ODIC-deploy Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `Clever-ODIC-deploy`를 `clever-context-monorepo`의 정식 deploy template family로 추가하고, root deploy governance와 reusable deploy assets를 최신 배포 정본에 맞게 정렬한다.

**Architecture:** 새 template family 문서를 `docs/templates/Clever-ODIC-deploy/` 아래에 추가하고, registry와 root governance에서 이 family를 current deploy baseline으로 읽게 만든다. `templates/deploy/*` 자산은 같은 canonical model을 재사용하도록 재서술하고, monorepo와 MSA는 deploy model이 아니라 workload cardinality 차이로만 설명한다.

**Tech Stack:** Markdown docs, template registry docs, reusable deploy template assets, grep/sed/git 기반 문서 검증

---

### Task 1: Add the Clever-ODIC-deploy template family documents

**Files:**
- Create: `docs/templates/Clever-ODIC-deploy/index.md`
- Create: `docs/templates/Clever-ODIC-deploy/versions/v1.md`
- Reference: `docs/superpowers/specs/2026-04-22-clever-odic-deploy-design.md`
- Reference: `docs/templates/msa-template/index.md`
- Reference: `docs/templates/msa-template/versions/v1.md`

- [ ] **Step 1: Read the approved spec and current template family examples**

Run:
```bash
sed -n '1,260p' docs/superpowers/specs/2026-04-22-clever-odic-deploy-design.md
sed -n '1,220p' docs/templates/msa-template/index.md
sed -n '1,260p' docs/templates/msa-template/versions/v1.md
```

Expected:
- spec의 canonical deploy model과 template family 범위를 확인한다.
- existing template family 문서 구조를 따라갈 수 있다.

- [ ] **Step 2: Create the family entry document**

Create `docs/templates/Clever-ODIC-deploy/index.md` with:
```md
# Clever-ODIC-deploy

## 목적

이 템플릿은 CLEVER의 current deploy baseline을 template family로 승격한 entry다.

## 사용 대상

- single workload monorepo
- multiple workload MSA

## 핵심 모델

- app repo는 image build만 수행
- central release가 실제 deploy 수행
- runtime inventory가 workload scope를 고정
- public contract probe까지 완료돼야 release를 닫음
```

- [ ] **Step 3: Create the concrete v1 document**

Create `docs/templates/Clever-ODIC-deploy/versions/v1.md` with:
```md
# Clever-ODIC-deploy v1

## version meta

- `template_id`: `Clever-ODIC-deploy`
- `version`: `v1`
- `status`: `recommended`
- `project_type`: `monorepo single-workload / msa multi-workload`
- `deploy_profile`: `image-build-once-central-release`
```

Include:
- canonical deploy model
- monorepo single workload rules
- MSA multiple workload rules
- app-only change vs deploy-affecting change
- known constraints
- exclusion 범위

- [ ] **Step 4: Verify the new family docs exist and use the approved language**

Run:
```bash
rg -n "Clever-ODIC-deploy|image-build-once-central-release|single workload|multiple workload|public contract probe" \
  docs/templates/Clever-ODIC-deploy/index.md \
  docs/templates/Clever-ODIC-deploy/versions/v1.md
```

Expected:
- new family docs에서 canonical model과 workload cardinality wording이 모두 보인다.

- [ ] **Step 5: Commit the family docs**

Run:
```bash
git add docs/templates/Clever-ODIC-deploy/index.md docs/templates/Clever-ODIC-deploy/versions/v1.md
git commit -m "Add Clever-ODIC-deploy template family"
```

### Task 2: Register the template and align root governance

**Files:**
- Modify: `docs/templates/index.md`
- Modify: `docs/root/deploy-template-governance.md`
- Reference: `docs/root/central-deploy-runtime-current-truth.md`
- Reference: `docs/root/template-harness-governance.md`

- [ ] **Step 1: Read the registry and governance sources**

Run:
```bash
sed -n '1,240p' docs/templates/index.md
sed -n '1,260p' docs/root/deploy-template-governance.md
sed -n '1,220p' docs/root/central-deploy-runtime-current-truth.md
```

Expected:
- registry entry fields와 root governance phrasing을 확인한다.
- current runtime truth wording을 그대로 재사용할 부분을 고른다.

- [ ] **Step 2: Add Clever-ODIC-deploy to the template registry**

Update `docs/templates/index.md` to add a new entry with:
```md
### `Clever-ODIC-deploy`

- status: `recommended`
- latest version: `v1`
- use_case: current CLEVER deploy baseline for monorepo and MSA
- deploy_profile: `image-build-once-central-release`
- summary: current CLEVER deploy truth elevated as a reusable template family
```

Also note:
- `msa-template` is family-description oriented
- `test-erik-project-template` is a separate scaffold-oriented template

- [ ] **Step 3: Align deploy-template-governance to the new baseline**

Update `docs/root/deploy-template-governance.md` so it explicitly states:
- current deploy baseline is read through `Clever-ODIC-deploy`
- app repo build / central release / runtime inventory split is the reusable model
- monorepo and MSA differ only by workload cardinality

- [ ] **Step 4: Verify registry and governance consistency**

Run:
```bash
rg -n "Clever-ODIC-deploy|image-build-once-central-release|workload cardinality|current deploy baseline|runtime inventory" \
  docs/templates/index.md \
  docs/root/deploy-template-governance.md
```

Expected:
- 새 템플릿이 registry에 보인다.
- root governance가 같은 어휘로 current baseline을 설명한다.

- [ ] **Step 5: Commit the registry and governance updates**

Run:
```bash
git add docs/templates/index.md docs/root/deploy-template-governance.md
git commit -m "Register Clever-ODIC-deploy baseline"
```

### Task 3: Rework reusable deploy assets to match the canonical model

**Files:**
- Modify: `templates/deploy/README.md`
- Modify: `templates/deploy/checklist.md`
- Modify: `templates/deploy/env-template.example`
- Modify: `templates/deploy/override-guide.md`
- Reference: `docs/templates/Clever-ODIC-deploy/versions/v1.md`

- [ ] **Step 1: Read the current deploy assets**

Run:
```bash
sed -n '1,220p' templates/deploy/README.md
sed -n '1,220p' templates/deploy/checklist.md
sed -n '1,220p' templates/deploy/env-template.example
sed -n '1,260p' templates/deploy/override-guide.md
```

Expected:
- 현재 asset wording과 누락된 canonical model 요소를 파악한다.

- [ ] **Step 2: Rewrite the deploy asset README**

Update `templates/deploy/README.md` to explain:
- this asset set assumes `image build once + central release + runtime inventory`
- monorepo means single workload
- MSA means multiple workloads
- current truth comes from root governance and current runtime truth docs

- [ ] **Step 3: Expand the shared checklist**

Update `templates/deploy/checklist.md` to include:
- immutable image tag proof
- central release target confirmation
- runtime inventory presence
- rollback target confirmation
- public contract probe completion
- MSA bundle-specific check

- [ ] **Step 4: Rewrite env-template.example and override-guide.md**

Update `templates/deploy/env-template.example` with category-style examples for:
- image reference
- runtime env
- secret reference
- inventory identifier
- health/probe path

Update `templates/deploy/override-guide.md` to distinguish:
- allowed override
- migration-required baseline changes

- [ ] **Step 5: Verify the reusable assets use the canonical model consistently**

Run:
```bash
rg -n "central release|runtime inventory|single workload|multiple workloads|public contract probe|immutable" templates/deploy
```

Expected:
- all four asset files use the same canonical vocabulary

- [ ] **Step 6: Commit the asset changes**

Run:
```bash
git add templates/deploy/README.md templates/deploy/checklist.md templates/deploy/env-template.example templates/deploy/override-guide.md
git commit -m "Align deploy assets with Clever-ODIC-deploy"
```

### Task 4: Run final document consistency checks

**Files:**
- Verify: `docs/templates/Clever-ODIC-deploy/index.md`
- Verify: `docs/templates/Clever-ODIC-deploy/versions/v1.md`
- Verify: `docs/templates/index.md`
- Verify: `docs/root/deploy-template-governance.md`
- Verify: `templates/deploy/README.md`
- Verify: `templates/deploy/checklist.md`
- Verify: `templates/deploy/env-template.example`
- Verify: `templates/deploy/override-guide.md`

- [ ] **Step 1: Run targeted consistency checks**

Run:
```bash
rg -n "Clever-ODIC-deploy|image-build-once-central-release|single workload|multiple workloads|public contract probe|runtime inventory" \
  docs/templates/Clever-ODIC-deploy \
  docs/templates/index.md \
  docs/root/deploy-template-governance.md \
  templates/deploy
```

Expected:
- 모든 핵심 문서가 같은 deploy model을 가리킨다.

- [ ] **Step 2: Inspect the final diff**

Run:
```bash
git diff -- docs/templates/Clever-ODIC-deploy docs/templates/index.md docs/root/deploy-template-governance.md templates/deploy
```

Expected:
- current authority wording만 남고, monorepo/MSA 차이가 workload cardinality로만 정리돼 있다.

- [ ] **Step 3: Confirm repository status**

Run:
```bash
git status --short --branch
```

Expected:
- planned files만 변경으로 보인다.

- [ ] **Step 4: Create the integration commit**

Run:
```bash
git add docs/templates/Clever-ODIC-deploy docs/templates/index.md docs/root/deploy-template-governance.md templates/deploy
git commit -m "Document Clever-ODIC-deploy baseline"
```

- [ ] **Step 5: Push after review**

Run:
```bash
git push origin main
```

Expected:
- `main`에 `Clever-ODIC-deploy` template baseline 문서가 반영된다.
