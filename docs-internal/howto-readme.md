# Project Setup Documentation: go-hugo with Hugo + Docsy + Go Modules

**Last updated:** 2025-05-05

This document describes the step-by-step setup and configuration process for the `go-hugo` project, which integrates the Hugo static site generator with the Docsy theme and Go module system.

---

## 1. Repository Initialization

- A private GitHub repository `go-hugo` was created at:  
  https://github.com/secretvpc/go-hugo

- The repository contains the following initial files:
  - `.gitignore`
  - `README.md`
  - `LICENSE`
  - Default branch: `main` (automatically set by GitHub)

---

## 2. Cloning the Repository

```bash
git clone https://github.com/secretvpc/go-hugo.git
cd go-hugo
```

---

## 3. Installing Go

- The default Ubuntu package provided an outdated version: Go 1.18.1
- To install the latest stable version (Go 1.22.2):

```bash
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz
```

- The following line was added at the top of `~/.bashrc`:

```bash
export PATH=/usr/local/go/bin:$PATH
```

- Environment reloaded:

```bash
source ~/.bashrc
go version
# Output: go version go1.22.2 linux/amd64
```

- The legacy Go 1.18 packages were removed:

```bash
sudo apt purge golang-go
sudo apt autoremove
```

---

## 4. Installing Hugo Extended

- The default `apt` version was 0.92.2 (non-extended), which is incompatible with Docsy.
- Installed manually from official release:

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.126.1/hugo_extended_0.126.1_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.126.1_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
# Output: hugo v0.126.1+extended linux/amd64
```

---

## 5. Adding Docsy Theme as Git Submodule

```bash
git submodule add https://github.com/google/docsy.git themes/docsy
```

---

## 6. `.gitignore` Configuration

The following rules were added to `.gitignore` to exclude build artifacts and dependencies:

```gitignore
# Go
/bin/
/pkg/
/vendor/

# Hugo
/public/
/resources/_gen/
/hugo_stats.json

# Node.js (for Docsy theme management)
/node_modules/
/package-lock.json

# OS/editor artifacts
.DS_Store
.idea/
.vscode/
*.swp
```

---

## 7. Hugo Site Initialization

The Hugo site structure was created directly in the existing repository using:

```bash
hugo new site . --force
```

This ensured that no existing files (e.g., `.gitignore`, `README.md`) were overwritten, while adding the standard Hugo directories:
- `config.toml`
- `content/`
- `layouts/`
- `archetypes/`
- `static/`

The default file `hugo.toml` was removed, as it is superseded by the new `config.toml`.

---

## 8. Hugo Configuration for Docsy (config.toml)

A full `config.toml` file was generated, including:
- `baseURL` for GitHub Pages: `https://secretvpc.github.io/go-hugo/`
- Theme declaration: `docsy`
- GitHub integration (`params.github_repo`)
- Markup configuration for Goldmark and SCSS rendering
- Hugo module import for Docsy

This file is now used as the main site configuration. The obsolete `hugo.toml` was deleted.

---


## 9. Removing Default hugo.toml

The Hugo command `hugo new site` generated a default file `hugo.toml`, which is not used when a `config.toml` file is present. This file was deleted to avoid confusion:

```bash
rm hugo.toml
```

---

## 10. Verifying Hugo Setup

The installed Hugo version was verified to be the Extended variant:

```bash
hugo version
# Output: hugo v0.126.1+extended linux/amd64
```

This confirms that the system is ready to build and serve Docsy-based Hugo sites.

---

## Next Steps (Planned)

- Add a sample page using:
  ```bash
  hugo new content/docs/overview.md
  ```
- Create navigation menus in `config.toml`
- Serve the site locally for verification:
  ```bash
  hugo server --buildDrafts
  ```

**Last updated:** 2025-05-05
