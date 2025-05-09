# go-hugo Project Setup — Part 4

**Covers steps 21–27: Cloudflare Integration (Custom Domain + TLS)**

---

## ✅ 21. Configuring Custom Domain `go-hugo.secretvpc.dev` via Cloudflare

This step connects a Cloudflare-managed custom subdomain to the existing Hugo site deployed via GitHub Pages.

### 🧭 Goal

Make the go-hugo site accessible at:  
`https://go-hugo.secretvpc.dev`

---

### 📌 Prerequisites

- The `go-hugo` site is already deployed to:  
  `https://secretvpc.github.io/go-hugo/`
- You own and manage the domain `secretvpc.dev` via Cloudflare
- GitHub repository uses `gh-pages` branch for deployment

---

### 🛠️ Steps

#### 1. Create DNS record in Cloudflare

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Choose domain: `secretvpc.dev`
3. Navigate to **DNS** → **Records**
4. Click **Add record**
   - **Type:** `CNAME`
   - **Name:** `go-hugo`
   - **Target:** `secretvpc.github.io`
   - **Proxy status:** `DNS only` (gray cloud)
5. Save

This links `go-hugo.secretvpc.dev` to the public GitHub Pages IP via DNS.

---

#### 2. Add `CNAME` file to site output (`public/`)

In the Hugo build directory (`public/`), create a file named `CNAME` with a single line:

```
go-hugo.secretvpc.dev
```

This tells GitHub Pages to serve the site at the custom domain.

---

#### 3. Automate `CNAME` file in GitHub Actions

Add this step in `.github/workflows/deploy.yml` **before** the deploy step:

```yaml
- name: Add CNAME for custom domain
  run: echo "go-hugo.secretvpc.dev" > public/CNAME
```

This ensures the `CNAME` file is created dynamically on every deploy.

---

#### 4. Commit and push the workflow

```bash
git add .github/workflows/deploy.yml
git commit -m "Add CNAME setup for go-hugo.secretvpc.dev"
git push
```

A new deploy should run and push the updated `public/` folder with the `CNAME` to `gh-pages`.

---

#### 5. Update GitHub Pages settings (once)

In your GitHub repo:
1. Go to **Settings** → **Pages**
2. Under **Custom domain**, enter: `go-hugo.secretvpc.dev`
3. Save
4. Enable "Enforce HTTPS" if available

---

#### ✅ Result

Your site will now be live at:

🔗 [https://go-hugo.secretvpc.dev](https://go-hugo.secretvpc.dev)

---

## 22. Configuring SSL/TLS in Cloudflare for Custom Domain

To ensure secure HTTPS access to the site at `https://go-hugo.secretvpc.dev`, we configure Cloudflare's SSL/TLS settings appropriately.

---

### ✅ Goal

- Enable secure TLS encryption for GitHub Pages behind Cloudflare
- Avoid certificate mismatch issues
- Allow fast HTTPS propagation

---

### 🛠️ Steps

#### 1. Go to the Cloudflare Dashboard

- Select the domain: `secretvpc.dev`

---

#### 2. Navigate to: **SSL/TLS** → **Overview**

- Set SSL/TLS encryption mode to:  
  🔹 `Full`  
  (⚠️ Not "Full (Strict)" unless you manage your own certificate on GitHub Pages)

> Full mode encrypts between Cloudflare and GitHub, without requiring GitHub’s certificate to be signed by a recognized CA.

---

#### 3. Enable “Always Use HTTPS”

- Under **SSL/TLS** → **Edge Certificates**
- Turn on:
  - ✅ Always Use HTTPS
  - ✅ Automatic HTTPS Rewrites (optional)
  - ✅ Opportunistic Encryption (optional)

---

#### ✅ Result

Your site is now available at:

🔗 `https://go-hugo.secretvpc.dev`

With:
- End-to-end encryption (GitHub Pages → Cloudflare → Browser)
- No certificate warnings
