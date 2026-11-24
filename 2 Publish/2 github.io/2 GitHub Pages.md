# GitHub Pages

GitHub Pages hosts a static website for free. With a GitHub Actions workflow, GitHub builds your Quartz site **every time you push**, so you never upload `public/` by hand.

Your site address will be:

```
https://<user>.github.io/<repo>/
```

Example: user `student`, repo `my-wiki` → `https://student.github.io/my-wiki/`

## Step 1. Set `baseUrl`

In `quartz.config.yaml`, set `baseUrl` to the address without `https://`:

```yaml
baseUrl: student.github.io/my-wiki
```

## Step 2. Add the workflow file

Create `.github/workflows/deploy.yml` in the repo:

```yaml
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - v5            # the branch you push to

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0          # full history, so pages show correct dates
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - run: npm ci
      - run: npx quartz build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

What it does, line by line in plain words:

1. On every push to `v5`,
2. check out the repo, install Node 22 and the packages,
3. run `npx quartz build` to make `public/`,
4. hand `public/` to GitHub Pages.

## Step 3. Turn on Pages

On GitHub: **Settings → Pages → Build and deployment → Source: GitHub Actions**.

## Step 4. Push

```bash
git add .
git commit -m "Publish my wiki"
git push origin v5
```

Open the **Actions** tab to watch the build. When it turns green, visit your site.

## Common problems

- **Page loads without styles or links are broken**: `baseUrl` does not match the real address.
- **404 on the site**: Pages source is not set to *GitHub Actions*, or the workflow has not finished.
- **Workflow never runs**: you pushed to a different branch than the one under `branches:`.
