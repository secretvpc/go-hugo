# go-hugo Project Setup — Part 4

**Covers steps 21–23 + troubleshooting**

---

## 21. Custom Domain Configuration with Cloudflare

This step connects your GitHub Pages deployment to a custom domain via Cloudflare.

### 🏡 Objective

Connect `go-hugo.secretvpc.dev` to `secretvpc.github.io` with proper DNS and SSL settings.

---

### ✅ Step-by-Step: DNS Setup in Cloudflare

1. **Log into Cloudflare Dashboard:**
   [https://dash.cloudflare.com](https://dash.cloudflare.com)

2. **Select your domain:**
   Click on `secretvpc.dev` from your domain list.

3. **Navigate to DNS > Records:**
   Menu: `DNS` → `Records`

4. **Add a CNAME Record:**

   | Field        | Value                       |
   | ------------ | --------------------------- |
   | Type         | `CNAME`                     |
   | Name         | `go-hugo`                   |
   | Target       | `secretvpc.github.io`       |
   | TTL          | `Auto`                      |
   | Proxy status | `DNS only` (**gray cloud**) |

5. **Save the record.**

---

### ⚡ Step-by-Step: SSL/TLS Configuration in Cloudflare

1. Go to: `SSL/TLS` → `Overview`

   * **Encryption Mode:** `Full` (not Flexible, not Full (strict))

2. Go to: `SSL/TLS` → `Edge Certificates`

   * **Always Use HTTPS:** `On`
   * **Automatic HTTPS Rewrites:** `On`
   * \*\*Certificate for `*.secretvpc.dev` must be active\`

3. Skip: `Custom Hostnames`, `Custom Certificates`

---

## 22. Configuring Hugo for Custom Domain Deployment

### Update `config.toml`:

```toml
baseURL = "https://go-hugo.secretvpc.dev/"
```

### Add a `CNAME` file:

```bash
echo "go-hugo.secretvpc.dev" > static/CNAME
```

> Hugo will copy this file into the `public/` directory on build.

### Rebuild the site:

```bash
hugo --minify
```

### Deploy the site (Manual or via GitHub Actions):

If manual:

```bash
rm -rf ../gh-pages/*
cp -r public/* ../gh-pages/
cd ../gh-pages
git add .
git commit -m "Deploy with custom domain"
git push
```

---

## 23. GitHub Pages Settings for Custom Domain

1. Go to: GitHub repo → `Settings` → `Pages`

2. Under "Custom domain":

   * Enter: `go-hugo.secretvpc.dev`
   * Enable: `Enforce HTTPS`

3. Wait for certificate issuance (2–5 mins).

   * You should see: `Certificate: Active`

---

## ⚠️ Troubleshooting: Custom Domain 404 + SSL

### Problem 1: 404 Error at root domain

**Symptom:** You open `https://go-hugo.secretvpc.dev` and see a 404 error.

**Cause:** Hugo was built with `baseURL = "https://secretvpc.github.io/go-hugo/"`, so all links point to `/go-hugo/`.

**Fix:**

* Set `baseURL = "https://go-hugo.secretvpc.dev/"`
* Rebuild and redeploy the site

### Problem 2: SSL Certificate Invalid (ERR\_CERT\_COMMON\_NAME\_INVALID)

**Symptom:** HTTPS fails with a certificate error.

**Causes:**

* Missing `CNAME` file
* GitHub not configured with your domain

**Fixes:**

* Ensure `static/CNAME` exists and is included in every build
* Go to GitHub Pages settings → enter `go-hugo.secretvpc.dev` as Custom domain
* Enable `Enforce HTTPS`

### Problem 3: GitHub Pages not deploying to root

**Symptom:** Only `https://go-hugo.secretvpc.dev/go-hugo/` works

**Cause:** You are still deploying the site to a subdirectory

**Fix:**

* Clean `gh-pages` directory
* Deploy content of `public/` directly to root

---

**End of Part 4**
**Next steps:** Optional enhancements like redirect rules, robots.txt, custom error pages, or analytics integration.
