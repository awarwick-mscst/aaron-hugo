---
title: "Deploying a Local Hugo Site to Azure Static Web Apps"
date: 2026-02-03T14:00:00-06:00
description: "A practical guide for deploying a locally developed Hugo site to Azure Static Web Apps using GitHub Actions."
draft: false
---
# Deploy Hugo to Azure for Free!!!

## Overview

This guide walks through moving a **locally developed Hugo site** to **Azure Static Web Apps (SWA)** using **GitHub Actions**.

The goal is a workflow where:
- You develop and preview locally
- GitHub Actions builds the site
- Azure Static Web Apps deploys it globally
- No servers are managed manually

This assumes:
- You already have a working Hugo site locally
- You are using GitHub
- You want reproducible, CI-based deployments

---

## Prerequisites

- A local Hugo site (`hugo server` works)
- Hugo **extended** edition (recommended)
- A GitHub repository
- An Azure subscription
- Git installed locally

---

## 1. Verify Local Hugo Build

From your site root:

```bash
hugo server -D
````

Visit:

```
http://localhost:1313
```

Confirm:

* The site renders correctly
* Theme assets load
* No build errors appear in the terminal

For a production-equivalent build test:

```bash
hugo --minify
```

This generates output in the `public/` directory.

---

## 2. Prepare the Repository

### Ignore Build Output

Hugo’s `public/` directory should **not** be committed.

Create or update `.gitignore`:

```gitignore
public/
resources/
.hugo_build.lock
```

If `public/` was already committed:

```bash
git rm -r --cached public
git commit -m "Stop tracking Hugo build output"
```

---

## 3. Push the Site to GitHub

From your project root:

```bash
git init
git add .
git commit -m "Initial Hugo site"
git branch -M main
git remote add origin https://github.com/<your-org>/<your-repo>.git
git push -u origin main
```

Your GitHub repo should now contain:

* Hugo source files
* Theme (as a submodule if applicable)
* NO `public/` directory

---

## 4. Create the Azure Static Web App

In the Azure Portal:

1. Create **Static Web App**
2. Choose:

   * Source: GitHub
   * Repository & branch: `main`
3. Let Azure create the GitHub Action (we’ll modify it)

Once created, Azure adds a workflow file under:

```
.github/workflows/
```

---

## 5. Build Hugo in GitHub Actions (Important)

Azure’s default build environment often lags behind modern Hugo binaries.
To avoid runtime/library issues, build Hugo **inside a Docker container** and deploy the output.

### Working GitHub Actions Workflow

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches: [main]

jobs:
  build_and_deploy_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true

      - name: Build site (Hugo in Docker)
        run: |
          docker run --rm \
            -v "${{ github.workspace }}:/src" \
            -w /src \
            ghcr.io/gohugoio/hugo:v0.155.2 \
            --minify

      - name: Deploy to Azure Static Web Apps
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "public"
          output_location: ""
          skip_app_build: true
```

### Key Points

* Hugo runs in Docker → consistent builds
* Azure SWA only uploads prebuilt files
* `skip_app_build: true` disables Oryx

---

## 6. Verify Deployment

After pushing to `main`:

1. Open **GitHub → Actions**
2. Confirm the workflow is **green**
3. Visit the Azure-provided URL
4. Hard refresh the browser (`Ctrl + F5`)

Your site should now reflect your latest commit.

---

## 7. Local Development Workflow (Recommended)

Daily workflow:

```bash
hugo server -D
```

When ready:

```bash
git add .
git commit -m "Update content"
git push
```

Azure handles the rest.

---

## Common Pitfalls

### Site deploys but content is old

* `public/` is still tracked
* Build step missing in GitHub Actions
* Azure deploying stale artifacts

### Hugo build fails in CI

* Deprecated config keys (`paginate`)
* Wrong Hugo version
* Missing extended edition

### Theme assets missing

* Theme submodule not checked out
* `submodules: true` missing in checkout step

---

## Final Notes

This setup gives you:

* Local-first development
* Deterministic builds
* Zero server maintenance
* Global CDN-backed hosting

Once configured, deploying is as simple as pushing to GitHub.

---

## References

* Hugo Documentation
* Azure Static Web Apps Documentation
* GitHub Actions Documentation

