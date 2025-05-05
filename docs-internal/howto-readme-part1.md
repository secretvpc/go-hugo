# Project Setup Documentation: go-hugo with Hugo + Docsy + Go Modules

**Last updated:** 2025-05-05

This document describes the complete setup and configuration process for the `go-hugo` project, which integrates the Hugo static site generator with the Docsy theme and Go module system, targeting deployment on GitHub Pages.

---

## 1. Repository Initialization

- GitHub repository created at: https://github.com/secretvpc/go-hugo
- Initial files:
  - `.gitignore`
  - `README.md`
  - `LICENSE`
- Default branch: `main`

---

## 2. Cloning the Repository

```bash
git clone https://github.com/secretvpc/go-hugo.git
cd go-hugo
```

---

## 3. Installing Go

System had Go 1.18.1 via `apt`. Replaced with Go 1.22.2.

### Manual installation of Go 1.22.2:

```bash
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz
echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
go version
```

---

## 4. Installing Hugo Extended

Default Hugo via `apt` was outdated and non-extended. Installed manually:

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.126.1/hugo_extended_0.126.1_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.126.1_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
```

---

## 5. Removing Old Go Installation

```bash
sudo apt purge golang-go
sudo apt autoremove
```

---

## 6. Creating Hugo Site in Existing Repository

```bash
cd ~/secretvpc/go-hugo
hugo new site . --force
```

Generated:
- `config.toml`
- `content/`
- `layouts/`
- `archetypes/`
- `static/`

Removed unused:
```bash
rm hugo.toml
```

---

## 7. Switching from Git Submodule to Hugo Module

Originally added Docsy as submodule:

```bash
git submodule add https://github.com/google/docsy.git themes/docsy
```

Later removed for Hugo Modules:

```bash
git submodule deinit themes/docsy
git rm themes/docsy
rm -rf .git/modules/themes/docsy
rm -rf themes/docsy
```

---

## 8. Hugo Modules Setup

### In `config.toml`:

```toml
[module]
  [[module.imports]]
    path = "github.com/google/docsy"
```

Removed legacy `theme = "docsy"` line.

### Init and tidy:

```bash
hugo mod init github.com/secretvpc/go-hugo
hugo mod tidy
```

---

## 9. Creating Documentation Page

```bash
hugo new content/docs/overview.md
```

### `overview.md` content (in TOML front matter format):

```toml
+++
title = "Overview"
linkTitle = "Overview"
weight = 1
description = "Project overview and objectives"
draft = false
+++

Welcome to the **go-hugo** project documentation site.

This site is built using the [Hugo](https://gohugo.io) static site generator and the [Docsy](https://www.docsy.dev/) theme, and is deployed via GitHub Pages. It is intended as an educational and modular reference for building Hugo-based documentation with versioning, GitHub Actions deployment, and multi-language support.

### Goals of this project:

- Provide a minimal, functional Hugo + Docsy site as a reference template.
- Document every setup step clearly for reproducibility.
- Enable CI/CD deployment to GitHub Pages.
- Support future extensions like multi-language, diagrams, and embedded code samples.

Refer to the sidebar for detailed guides and development notes.
```

---

## 10. Adding Navigation Menu

In `config.toml`, add:

```toml
[[menu.main]]
  identifier = "docs"
  name = "Documentation"
  url = "/docs/overview/"
  weight = 10
```

---

## 11. Running the Hugo Development Server

```bash
hugo server --buildDrafts
```

This builds and serves the site locally at:  
[http://localhost:1313/go-hugo/](http://localhost:1313/go-hugo/)

---

## Next Planned Steps

- Add more content under `content/docs/`
- Customize sidebar structure and menus
- Configure deployment to GitHub Pages
