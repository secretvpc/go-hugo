# go-hugo Project Setup — Part 3


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

---

## 19. Granting Write Permissions to GitHub Actions

To enable `github-actions[bot]` to successfully push to the `gh-pages` branch, write permissions must be granted explicitly in the repository settings.

### Required Change:

1. Navigate to the repository on GitHub.
2. Go to: `Settings` → `Actions` → `General`
3. Scroll to **Workflow permissions**
4. Select: `Read and write permissions`
5. Save the change.

This allows GitHub Actions to commit and push the contents of `public/` to the `gh-pages` branch.

### Result:

Subsequent workflow runs complete successfully, and the site is automatically published at:

🔗 https://secretvpc.github.io/go-hugo/

---

## 20. Optimizing Workflow Trigger Conditions

To prevent unnecessary deployments, the GitHub Actions workflow was updated to run **only when site-related files change**.

### deploy.yml modification:

The `on.push.paths` section was added to the workflow:

```yaml
on:
  push:
    branches: [main]
    paths:
      - 'content/**'
      - 'static/**'
      - 'layouts/**'
      - 'config.*'
      - 'archetypes/**'
      - 'data/**'
      - 'i18n/**'
      - 'assets/**'
      - 'themes/**'
      - 'package.json'
      - 'postcss.config.js'
      - 'hugo.toml'
      - '.github/workflows/deploy.yml'
```
---

### Result:

Workflow now triggers **only** if changes occur in:
- Hugo configuration or theme files
- Content, layout, assets, static directories
- Site build config files (e.g., PostCSS)

Changes to unrelated files like `README.md` or internal docs no longer cause deployments.

---
  
**Last updated:** 2025-05-05