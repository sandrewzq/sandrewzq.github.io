# Hugo Maintenance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Improve Hugo blog maintenance stability without changing any content pages.

**Architecture:** Keep the existing Hugo + Stack theme architecture. Limit changes to repository hygiene, Hugo site-level configuration, and GitHub Actions maintenance. Do not edit files under `content/`.

**Tech Stack:** Hugo 0.128.0, Hugo Stack theme, GitHub Actions, GitHub Pages.

---

## Files

- Modify: `d:\sandrew\sandrewzq.github.io\.gitignore`
- Modify: `d:\sandrew\sandrewzq.github.io\config.yaml`
- Modify: `d:\sandrew\sandrewzq.github.io\.github\workflows\hugo.yml`
- Do not modify: `d:\sandrew\sandrewzq.github.io\content\**`

## Task 1: Ignore Generated Files

**Files:**
- Modify: `d:\sandrew\sandrewzq.github.io\.gitignore`

- [ ] **Step 1: Update Hugo build artifact ignores**

Set `.gitignore` to ignore generated Hugo outputs and local editor files while keeping theme submodule tracking unchanged:

```gitignore
# Except this file !.gitignore
public/
resources/
.hugo_build.lock
.obsidian
```

- [ ] **Step 2: Verify no content files changed**

Run:

```bash
git diff --name-only
```

Expected: `.gitignore` appears, no `content/` files appear.

## Task 2: Update Hugo Maintenance Config

**Files:**
- Modify: `d:\sandrew\sandrewzq.github.io\config.yaml`

- [ ] **Step 1: Enable Chinese language handling**

Change:

```yaml
hasCJKLanguage: false
```

To:

```yaml
hasCJKLanguage: true
```

- [ ] **Step 2: Disable placeholder Disqus comments**

Change:

```yaml
disqusShortname: hugo-theme-stack
```

To:

```yaml
disqusShortname:
```

Change:

```yaml
    comments:
        enabled: true
        provider: disqus
```

To:

```yaml
    comments:
        enabled: false
        provider:
```

- [ ] **Step 3: Verify no content files changed**

Run:

```bash
git diff --name-only
```

Expected: `.gitignore` and `config.yaml` appear, no `content/` files appear.

## Task 3: Simplify GitHub Actions Maintenance Steps

**Files:**
- Modify: `d:\sandrew\sandrewzq.github.io\.github\workflows\hugo.yml`

- [ ] **Step 1: Remove Node dependency no-op step**

Remove this step because the repository has no Node lockfile:

```yaml
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
```

- [ ] **Step 2: Keep the Hugo build and deployment path unchanged**

Verify these steps remain:

```yaml
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
```

- [ ] **Step 3: Verify no content files changed**

Run:

```bash
git diff --name-only
```

Expected: `.gitignore`, `config.yaml`, `.github/workflows/hugo.yml` appear, no `content/` files appear.

## Task 4: Validate and Commit

**Files:**
- Inspect: all changed files

- [ ] **Step 1: Verify changed files are maintenance-only**

Run:

```bash
git diff --name-only
```

Expected output contains only:

```text
.github/workflows/hugo.yml
.gitignore
config.yaml
docs/superpowers/plans/2026-06-05-hugo-maintenance-plan.md
```

- [ ] **Step 2: Check whether Hugo is available locally**

Run:

```bash
hugo version
```

Expected: Hugo version output, or command not found. If command not found, use GitHub Actions for build verification after push.

- [ ] **Step 3: Run local Hugo build if available**

Run only if `hugo version` succeeds:

```bash
hugo --gc --minify
```

Expected: build succeeds without fatal errors.

- [ ] **Step 4: Commit maintenance changes**

Run:

```bash
git add .gitignore config.yaml .github/workflows/hugo.yml docs/superpowers/plans/2026-06-05-hugo-maintenance-plan.md
git commit -m "chore: apply low-risk hugo maintenance updates"
```

- [ ] **Step 5: Push if requested**

Run:

```bash
git push
```
