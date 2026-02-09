# Aaron Hugo Site

This repository contains the source for **Aaron Warwick's Hugo website**.

It is a personal site focused on:
- networking and infrastructure
- cloud and identity (Azure / Microsoft 365)
- projects, writeups, and technical notes

## Tech Stack

- [Hugo](https://gohugo.io/)
- `terminal` Hugo theme (included under `themes/terminal`)

## Repository Structure

- `content/` — pages and posts (`about`, `projects`, `writeups`, `contact`, `posts`)
- `archetypes/` — default content templates
- `themes/terminal/` — site theme
- `hugo.toml` — site configuration

## Run Locally

1. Install Hugo (extended version recommended).
2. From the repository root, start the dev server:

```bash
hugo server -D
```

3. Open the local URL Hugo prints (typically `http://localhost:1313`).

## Build for Production

```bash
hugo
```

Generated static files will be written to `public/`.

## Deployment

`baseURL` is configured in `hugo.toml` for deployment to Azure Static Web Apps.

---

If you're here from GitHub: this repo is the content and configuration source for the live site.
