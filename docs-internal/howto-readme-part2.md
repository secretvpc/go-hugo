# go-hugo Project Setup — Part 2

**Covers steps 12–15**  
**Last updated:** 2025-05-05

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
