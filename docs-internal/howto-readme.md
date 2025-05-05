# Project Setup Documentation: go-hugo with Hugo + Docsy + Go Modules

**Last updated:** 2025-05-05

This document describes the complete setup and configuration process for the `go-hugo` project, which integrates the Hugo static site generator with the Docsy theme and Go module system, targeting deployment on GitHub Pages.

---

## 1. Repository Initialization

- A GitHub repository `go-hugo` was created: https://github.com/secretvpc/go-hugo
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

The system originally had Go 1.18.1 installed via `apt`. It was removed and replaced with the latest stable version.

### Install Go 1.22.2 manually:

```bash
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz
echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
go version  # should show go1.22.2
```

---

## 4. Installing Hugo Extended

The default `apt` package was outdated and non-extended. Instead, the Extended version was installed manually:

```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.126.1/hugo_extended_0.126.1_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.126.1_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version  # should show +extended
```

---

## 5. Removing Old Go

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

This created:
- `config.toml`
- `content/`
- `layouts/`
- `static/`
- `archetypes/`

---

## 7. Removing Redundant hugo.toml

```bash
rm hugo.toml
```

---

## 8. Preparing for Docsy Theme (Switch from Submodule to Module)

Originally, Docsy was added as a Git submodule:

```bash
git submodule add https://github.com/google/docsy.git themes/docsy
```

This was later removed to switch to Hugo Modules:

```bash
git submodule deinit themes/docsy
git rm themes/docsy
rm -rf .git/modules/themes/docsy
rm -rf themes/docsy
```

---

## 9. Configuring Hugo Modules

### `config.toml` was updated to include:

```toml
[module]
  [[module.imports]]
    path = "github.com/google/docsy"
```

The `theme = "docsy"` line was removed to comply with Hugo Module usage.

---

## 10. Initializing Hugo Module System

```bash
hugo mod init github.com/secretvpc/go-hugo
```

This created a `go.mod` file to manage Hugo module dependencies.

---

## Next Steps

- Run `hugo mod tidy` to fetch dependencies.
- Create a sample content file:
  ```bash
  hugo new content/docs/overview.md
  ```
- Add site navigation in `config.toml`.
- Preview the site locally:
  ```bash
  hugo server --buildDrafts
  ```

