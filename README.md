# cdxvy30.github.io

Personal blog built with Hugo + [PaperMod](https://github.com/adityatelange/hugo-PaperMod), deployed to GitHub Pages (custom domain `cdxvy30.tw`).

## Setup

```bash
git clone --recurse-submodules https://github.com/cdxvy30/cdxvy30.github.io.git
# If already cloned without the theme:
git submodule update --init --recursive

brew install hugo   # extended edition required; CI uses 0.152.2
```

## Write a post

```bash
hugo new content post/<slug>.md   # generated from archetypes/default.md, draft: true by default
```

Front matter example:

```yaml
---
title: "Title"
date: 2026-11-07T22:38:00+08:00
draft: true        # set to false before publishing
tags:
  - Kubernetes
---
```

## Preview locally

```bash
hugo server -D     # -D includes drafts, http://localhost:1313
```

## Publish

Pushing to `main` triggers the GitHub Actions workflow in `.github/workflows/`, which builds and deploys to GitHub Pages.

```bash
# make sure draft: false
git add content/post/<slug>.md
git commit -m "post: <slug>"
git push origin main
```

Check deployment status on the repo's Actions page.

## Notes

- `public/` is local build output; CI rebuilds it, no need to update it manually.
- Domain is set in `static/CNAME`; `baseURL` in `hugo.yaml` currently uses HTTP temporarily.
- Update theme: `git submodule update --remote themes/PaperMod`
