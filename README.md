# Network Engineering Notes — site source

Static site built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/),
hosted free on GitHub Pages, with a browser admin panel ([Sveltia CMS](https://sveltiacms.app))
at `/admin/`. Every change you save → a Git commit → GitHub Actions rebuilds the site in ~1 min.

---

## 1. One-time setup (≈15 min)

1. **Create the repo.** On GitHub → *New repository* → name it exactly
   `layer3ninja.github.io` (your username), **Public**, no README.
2. **Replace placeholders.** Search-and-replace `layer3ninja` with your GitHub username in:
   `mkdocs.yml`, `docs/admin/config.yml`, `docs/blog/.authors.yml`.
   Also set your LinkedIn URL in `mkdocs.yml` (`LINKEDIN_HANDLE`).
3. **Push the code** (from this folder):
   ```bash
   git init -b main
   git add .
   git commit -m "Initial site"
   git remote add origin https://github.com/layer3ninja/layer3ninja.github.io.git
   git push -u origin main
   ```
   *(No git locally? On the empty repo page use "uploading an existing file" and drag the
   folder contents in — include the hidden `.github` folder.)*
4. **Turn on Pages.** Repo → *Settings → Pages → Build and deployment → Source:*
   **GitHub Actions**. Then *Actions* tab → wait for the green check.
5. **Visit** `https://layer3ninja.github.io` 🎉

### Admin panel login token (one time)

1. GitHub → *Settings → Developer settings → Personal access tokens → Fine-grained tokens →
   Generate new token*.
2. Repository access: **Only select repositories** → your site repo.
3. Permissions → *Repository permissions* → **Contents: Read and write**.
4. Set an expiry (e.g. 1 year), generate, copy it into your password manager.
5. Open `https://layer3ninja.github.io/admin/` → **Sign In with Token** → paste.

The token lives only in your browser. Visitors can open `/admin/` but can't do anything
without a token that has write access to your repo.

---

## 2. Posting new material

### Option A — Admin panel (no tools needed, works on phone too)
`/admin/` → pick a collection (*Blog posts*, *CCNP notes*, *Security notes*…) → **New** →
write → **Save**. Upload images with the image button. Live in about a minute.

### Option B — Markdown files (VS Code or the GitHub web editor)
Add a `.md` file to the right folder and commit:

| Content | Folder |
|---|---|
| Blog post | `docs/blog/posts/` |
| CCNA / CCNP | `docs/ccna/`, `docs/ccnp/` |
| Routing & Switching | `docs/routing-switching/` |
| Security | `docs/security/` |
| Azure / AWS / GCP | `docs/cloud/azure/`, `docs/cloud/aws/`, `docs/cloud/gcp/` |
| AI & Networking | `docs/ai/` |

New pages show up in the sidebar automatically (alphabetical). The top-menu order lives in
`docs/.nav.yml`. A new **track** = new folder with an `index.md` + one line in `docs/.nav.yml`
+ a matching collection in `docs/admin/config.yml`.

**Note template**
```markdown
---
title: BGP Path Selection
description: The BGP best-path algorithm, step by step.
tags: [BGP, CCNP]
---

Intro paragraph…

## Commands
    ```text
    show ip bgp 10.0.0.0/24
    ```
```

**Blog post template** (`docs/blog/posts/2026-10-01-my-lab.md`)
```markdown
---
title: Building a DMVPN lab in CML
date: 2026-10-01
authors: [farshad]
categories: [Labs, Security]
tags: [DMVPN, IPsec]
---

One-paragraph summary shown on the blog index.

<!-- more -->

Rest of the post…
```
Set `draft: true` to hide a post until it's ready.

### Handy formatting
- Callouts: `!!! tip "Title"`, `!!! warning`, `!!! note`, `??? question "Quiz"` (collapsible)
- Topology diagrams: a ```` ```mermaid ```` code block
- Tabs (e.g. IOS vs NX-OS): `=== "IOS-XE"` / `=== "NX-OS"`
- Keys: `++ctrl+shift+6++`
- Full reference: https://squidfunk.github.io/mkdocs-material/reference/

---

## 3. Preview locally (optional)
```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
mkdocs serve        # → http://127.0.0.1:8000, auto-reloads on save
```

## 4. Custom domain later
Buy a domain → add a `docs/CNAME` file containing e.g. `notes.example.com` → at your DNS
provider add a CNAME `notes` → `layer3ninja.github.io` → Settings → Pages → Custom domain,
tick *Enforce HTTPS*. Update `site_url` in `mkdocs.yml`.

## 5. Future-proofing
Material for MkDocs is in maintenance mode (bug/security fixes only); its authors'
successor, **Zensical**, reads `mkdocs.yml` directly and already supports the blog, tags and
search features used here. Versions are pinned in `requirements.txt`, so the site keeps
building as-is; migrating later is mostly `pip install zensical` + `zensical build`.
(The menu-ordering plugin `awesome-nav` would be swapped for a `nav:` list in `mkdocs.yml`.)
Your content is plain Markdown either way — no lock-in.
