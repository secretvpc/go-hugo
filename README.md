[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

# go-hugo Documentation Site

This project hosts the **go-hugo** documentation site — an academic, modular, and extensible static website built with [Hugo](https://gohugo.io) and the [Docsy](https://www.docsy.dev) theme. It serves as a reference implementation for creating structured, production-ready documentation using open tools and GitHub Pages.

---

## Project Objectives

* Provide a clean, minimal starter template with Hugo and Docsy
* Document each step of setup, configuration, and deployment
* Support both manual and automated GitHub Pages deployments
* Enable modular structure, multi-language support, and CI/CD workflows
* Serve educational and production use cases

---

## Repository Structure

```text
go-hugo/
├── archetypes/             # Content archetype templates
├── assets/                 # SCSS, JS and processing assets
├── content/                # Markdown documentation content
├── data/                   # Site data (YAML/JSON/TOML)
├── docs-internal/          # Internal project documentation (step-by-step)
├── layouts/                # Custom templates and overrides
├── static/                 # Static site assets (images, fonts, etc.)
├── themes/                 # Hugo modules and themes (e.g., Docsy)
├── config.toml             # Hugo site configuration
├── go.mod / go.sum         # Hugo module dependencies
├── package.json            # Node.js dependencies for PostCSS
├── postcss.config.js       # PostCSS + Autoprefixer configuration
└── .github/workflows/      # GitHub Actions workflow definitions
```

---

## Live Site

The site is automatically deployed via GitHub Actions at:
[https://go-hugo.secretvpc.dev](https://go-hugo.secretvpc.dev)

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/secretvpc/go-hugo.git
cd go-hugo
```

### 2. Install prerequisites

* Go 1.22 or higher
* Hugo Extended 0.126 or higher
* Node.js 18+ (for SCSS/PostCSS)

### 3. Install Node.js dependencies

```bash
npm install
```

### 4. Run the development server

```bash
hugo server --buildDrafts
```

Visit [http://localhost:1313/go-hugo/](http://localhost:1313/go-hugo/)

---

## Build and Deployment

### Local production build

```bash
hugo --minify
```

### GitHub Pages deployment

The deployment workflow is defined in `.github/workflows/deploy.yml`.
It builds the site and deploys it to the `gh-pages` branch only if site-related files are changed.

---

## Project Documentation

Project setup and configuration are documented in:

* [`docs-internal/howto-readme-part1.md`](docs-internal/howto-readme-part1.md) — Steps 1–11
* [`docs-internal/howto-readme-part2.md`](docs-internal/howto-readme-part2.md) — Steps 12–16
* [`docs-internal/howto-readme-part3.md`](docs-internal/howto-readme-part3.md) — Steps 17–20
* [`docs-internal/howto-readme-part4.md`](docs-internal/howto-readme-part4.md) — Steps 21–23, Cloudflare integration, SSL, custom domain, and troubleshooting

---

## Credits and References

This project is based on:

* [Hugo](https://gohugo.io/)
* [Docsy theme](https://www.docsy.dev/)
* [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)
* [GitHub Actions](https://docs.github.com/en/actions)
* [Cloudflare](https://www.cloudflare.com/) — for DNS and SSL with custom domains

---

## License

This project is licensed under the [MIT License](./LICENSE).
