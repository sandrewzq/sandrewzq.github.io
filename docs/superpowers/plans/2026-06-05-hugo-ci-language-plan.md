# Hugo CI Language Maintenance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Align the site language configuration with the Chinese content and update the Pages artifact action version.

**Architecture:** Keep the existing Hugo + Stack theme architecture. Apply two maintenance-only edits: one Hugo language setting and one GitHub Actions version pin. Do not modify any files under `content/`.

**Tech Stack:** Hugo 0.128.0, Hugo Stack theme, GitHub Actions, GitHub Pages.

---

## Files

- Modify: `d:\sandrew\sandrewzq.github.io\config.yaml`
- Modify: `d:\sandrew\sandrewzq.github.io\.github\workflows\hugo.yml`
- Do not modify: `d:\sandrew\sandrewzq.github.io\content\**`

## Task 1: Align Hugo Language Code

**Files:**
- Modify: `d:\sandrew\sandrewzq.github.io\config.yaml`

- [ ] **Step 1: Change the language code**

Change:

```yaml
languageCode: en-us
```

To:

```yaml
languageCode: zh-cn
```

- [ ] **Step 2: Verify language settings remain consistent**

Run:

```bash
git diff -- config.yaml
```

Expected diff includes only the `languageCode` change for this task. Existing `DefaultContentLanguage: zh-cn` and `hasCJKLanguage: true` stay unchanged.

## Task 2: Update Pages Artifact Action

**Files:**
- Modify: `d:\sandrew\sandrewzq.github.io\.github\workflows\hugo.yml`

- [ ] **Step 1: Update upload-pages-artifact version**

Change:

```yaml
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
```

To:

```yaml
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v4
```

- [ ] **Step 2: Verify deployment flow remains unchanged**

Run:

```bash
git diff -- .github/workflows/hugo.yml
```

Expected diff includes only the action version change. Checkout, Hugo build, artifact path, and deploy action stay unchanged.

## Task 3: Validate, Commit, and Push

**Files:**
- Inspect: `d:\sandrew\sandrewzq.github.io\config.yaml`
- Inspect: `d:\sandrew\sandrewzq.github.io\.github\workflows\hugo.yml`
- Inspect: `d:\sandrew\sandrewzq.github.io\docs\superpowers\plans\2026-06-05-hugo-ci-language-plan.md`

- [ ] **Step 1: Verify changed files are allowed**

Run:

```bash
git diff --name-only
git status --short
```

Expected changed files:

```text
.github/workflows/hugo.yml
config.yaml
docs/superpowers/plans/2026-06-05-hugo-ci-language-plan.md
```

No `content/` path appears.

- [ ] **Step 2: Run whitespace check**

Run:

```bash
git diff --check
```

Expected: command exits with code 0.

- [ ] **Step 3: Commit changes**

Run:

```bash
git add config.yaml .github/workflows/hugo.yml docs/superpowers/plans/2026-06-05-hugo-ci-language-plan.md
git commit -m "chore: align hugo language and pages artifact action"
```

- [ ] **Step 4: Push changes**

Run:

```bash
git push
```

- [ ] **Step 5: Confirm repository sync**

Run:

```bash
git log --oneline --decorate -3
git status -sb
```

Expected: latest commit is on `master` and `origin/master`, and the working tree is clean.
