# go-hugo Documentation Site

This repository hosts the **go-hugo** project — an academic, modular, and extensible documentation site built using [Hugo](https://gohugo.io) and the [Docsy](https://www.docsy.dev) theme. It is intended as a reference implementation for creating structured, production-grade documentation using open tools, GitHub Pages, and best practices for maintainable static websites.

---

## 🔍 Project Goals

- Provide a clean and minimal Hugo + Docsy starter template.
- Document every step of the installation, configuration, and deployment process.
- Enable both manual and automated deployment to GitHub Pages.
- Support modular documentation, multi-language capabilities, and CI/CD workflows.
- Serve as a foundation for educational and production use cases.

---

## 📁 Repository Structure

```text
go-hugo/
├── content/               # Markdown source files (docs, pages)
├── static/                # Static assets (images, files)
├── layouts/               # Custom layout overrides
├── themes/                # Hugo modules (e.g., Docsy)
├── config.toml           # Main Hugo configuration
├── postcss.config.js     # SCSS/Autoprefixer config
├── package.json          # Node.js deps (PostCSS)
├── go.mod / go.sum       # Hugo module definitions
├── public/               # Output directory (only locally)
├── .github/workflows/    # GitHub Actions (CI/CD)
└── docs-internal/        # Project documentation (howto-readme parts)
```

---

## 🚀 Live Site

The documentation site is published at:  
🔗 [https://secretvpc.github.io/go-hugo/](https://secretvpc.github.io/go-hugo/)

It is automatically deployed via GitHub Actions from the `main` branch to the `gh-pages` branch.

---

## ⚙️ Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/secretvpc/go-hugo.git
cd go-hugo
```

### 2. Install Requirements

- **Go** 1.22+
- **Hugo Extended** 0.126+
- **Node.js** 18+ (for PostCSS processing)

### 3. Install Node Dependencies

```bash
npm install
```

### 4. Run the Site Locally

```bash
hugo server --buildDrafts
```

Visit: [http://localhost:1313/go-hugo/](http://localhost:1313/go-hugo/)

---

## 🧪 Build & Deploy

### Local production build:

```bash
hugo --minify
```

### GitHub Pages deployment:

Deployment is handled by the GitHub Actions workflow in `.github/workflows/deploy.yml`. The site is published automatically to the `gh-pages` branch on every commit to `main` **if relevant files are changed** (e.g., `content/`, `static/`, `config.toml`).

---

## 📄 Project Documentation

Full step-by-step project history and setup is documented in:

- [`docs-internal/howto-readme-part1.md`](docs-internal/howto-readme-part1.md)
- [`docs-internal/howto-readme-part2.md`](docs-internal/howto-readme-part2.md)
- [`docs-internal/howto-readme-part3.md`](docs-internal/howto-readme-part3.md)

These documents cover everything from initial setup to advanced deployment configuration.

---

## 🧠 Credits and References

This project is based on:

- [Hugo](https://gohugo.io/)
- [Docsy theme](https://www.docsy.dev/)
- [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)
- [GitHub Actions](https://docs.github.com/en/actions)

---

## ✅ License

This repository is licensed under the MIT License. See `LICENSE` for details.
