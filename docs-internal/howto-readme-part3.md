# go-hugo Project Setup — Part 3

**Covers step 18: Automatic GitHub Pages Deployment via GitHub Actions**  
**Last updated:** 2025-05-05

---

## 18. Automatic Deployment to GitHub Pages

To replace the manual deployment workflow, GitHub Actions is used to automatically build and deploy the Hugo site on every push to the `main` branch.

### Workflow file:

A new workflow file was created at:

```
.github/workflows/deploy.yml
```

### deploy.yml contents:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.126.1'
          extended: true

      - name: Setup Node.js (for PostCSS)
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install PostCSS dependencies
        run: |
          npm install --save-dev postcss postcss-cli autoprefixer
          echo "module.exports = { plugins: { autoprefixer: {} } };" > postcss.config.js

      - name: Build site
        run: hugo --minify

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

### Result:

- Any push to the `main` branch now triggers a full rebuild and deployment to `gh-pages`.
- The site remains published at:  
  [https://secretvpc.github.io/go-hugo/](https://secretvpc.github.io/go-hugo/)

---
