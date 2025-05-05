# go-hugo Project Setup — Part 2

**Covers steps 12–16**

---

## 12. Installing Node.js and PostCSS

Docsy requires PostCSS to compile SCSS into CSS. The default Node.js (v12) was outdated and incompatible.

### Node.js upgrade:

```bash
sudo apt remove nodejs npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

```bash
node -v   # v18.x
npm -v    # >=10.x
```

### Install PostCSS CLI and Autoprefixer:

```bash
sudo npm install -g postcss-cli autoprefixer
```

---

## 13. Local Project Configuration for PostCSS

To resolve runtime PostCSS errors, the following local setup was added:

```bash
npm init -y
npm install --save-dev postcss postcss-cli autoprefixer
```

Also created a file `postcss.config.js`:

```js
module.exports = {
  plugins: {
    autoprefixer: {}
  }
}
```

---

## 14. Building Site with Minification

```bash
hugo --minify
```

This generated a complete production-ready static site under the `public/` directory without SCSS or PostCSS errors.

---

## 15. Updating .gitignore

The following was added to `.gitignore` to exclude Node.js dependencies:

```gitignore
# Node.js dependencies
node_modules/
```

---

## Next Step

- Prepare the `gh-pages` branch for GitHub Pages deployment.

---

## 16. Manual Deployment to GitHub Pages

The Hugo site's production build was deployed manually to GitHub Pages using the `gh-pages` branch and Git worktree.

### Purpose:

To publish the contents of `public/` to GitHub Pages via a dedicated branch.

### Steps:

1. Initialize the `gh-pages` branch (once):
   ```bash
   git checkout --orphan gh-pages
   git rm -rf .
   echo "Initial commit" > index.html
   git add index.html
   git commit -m "init gh-pages"
   git push origin gh-pages
   ```

2. Switch back to `main` and create a worktree:
   ```bash
   git checkout main
   git worktree add -B gh-pages ../gh-pages origin/gh-pages
   ```

3. Copy Hugo output to the worktree:
   ```bash
   rm -rf ../gh-pages/*
   cp -r public/* ../gh-pages/
   ```

4. Commit and push:
   ```bash
   cd ../gh-pages
   git add .
   git commit -m "Deploy Hugo site"
   git push origin gh-pages
   ```

### GitHub Pages Settings:

- Source: `gh-pages`
- Folder: `/ (root)`

### Result:

The site is now published at:  
[https://secretvpc.github.io/go-hugo/](https://secretvpc.github.io/go-hugo/)

---

**Last updated:** 2025-05-05

---