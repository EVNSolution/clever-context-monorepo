# MSA Template Family Registry Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish a public-safe `msa-template` family entry in `docs/templates` that reads like a package-level template registry record without embedding CLEVER runtime content.

**Architecture:** Keep the implementation documentation-only. Add a family entry and version record under `docs/templates/msa-template/`, update the registry index, and keep leaf templates as archetype sections rather than concrete scaffold files. The package must stay generic enough to later become a GitHub public template family.

**Tech Stack:** Markdown docs, template registry conventions

---

### Task 1: Lock the public-safe constraints into the design doc

**Files:**
- Modify: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/root/2026-04-15-msa-template-family-design.md`

- [ ] **Step 1: Add template-only constraints**

Write explicit rules that the family entry must not include business content, private runtime truth, or internal-only identifiers.

- [ ] **Step 2: Add future public-template rules**

Document that naming, placeholders, and metadata must stay reusable for a future GitHub public template.

- [ ] **Step 3: Verify the design still points to docs-only implementation**

Run: `sed -n '1,260p' /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/root/2026-04-15-msa-template-family-design.md`
Expected: the design contains explicit `template-only` and `future public template` guidance.

- [ ] **Step 4: Commit**

```bash
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo add docs/root/2026-04-15-msa-template-family-design.md
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo commit -m "docs: tighten msa template family constraints"
```

### Task 2: Create the `msa-template` family registry entry

**Files:**
- Create: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/templates/msa-template/index.md`
- Create: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/templates/msa-template/versions/v1.md`

- [ ] **Step 1: Write the family entry**

Add a family-level registry page that behaves like a README and contains:
- family metadata
- what the family is for
- what it deliberately excludes
- the leaf archetype list
- public-template readiness note

- [ ] **Step 2: Write the version record**

Add `v1` with:
- version metadata
- included leaf archetypes
- generic constraints only
- no CLEVER runtime secrets, URLs, or environment-specific content

- [ ] **Step 3: Verify the entry reads as a package, not a product**

Run: `sed -n '1,260p' /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/templates/msa-template/index.md`
Expected: family-level README tone, no runtime-specific business content.

Run: `sed -n '1,260p' /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/templates/msa-template/versions/v1.md`
Expected: archetype-level version record, no internal-only identifiers.

- [ ] **Step 4: Commit**

```bash
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo add docs/templates/msa-template/index.md docs/templates/msa-template/versions/v1.md
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo commit -m "docs: add msa template family registry entry"
```

### Task 3: Register the family in the template index

**Files:**
- Modify: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo/docs/templates/index.md`

- [ ] **Step 1: Add the new registry entry**

Add `msa-template` to the registered template list with family-level metadata and links to `index.md` and `versions/v1.md`.

- [ ] **Step 2: Keep the index generic**

Do not describe CLEVER runtime history in the registry index. Keep the entry at template-package level.

- [ ] **Step 3: Run final doc verification**

Run: `git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo diff --check`
Expected: no whitespace or patch-format errors.

Run: `git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo status --short`
Expected: only intended template-family files are modified; pre-existing unrelated dirty files remain untouched.

- [ ] **Step 4: Commit**

```bash
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo add docs/templates/index.md
git -C /Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-context-monorepo commit -m "docs: register msa template family"
```
