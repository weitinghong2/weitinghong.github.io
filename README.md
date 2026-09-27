# weitinghong.com

A plain HTML/CSS personal website: no build step and no dependencies. It is
hosted free on GitHub Pages and served at the custom domain registered through
Squarespace.

## Layout

| Path                     | What it is                                    |
|--------------------------|-----------------------------------------------|
| `index.html`             | About page (bio, photo, contact)              |
| `research.html`          | Papers, with abstracts and links              |
| `code-data.html`         | Template for code/data; not linked in the nav yet |
| `404.html`               | Shown for missing URLs                        |
| `assets/css/style.css`   | All styling (colors and fonts are at the top) |
| `assets/img/`            | Photo and favicon                             |
| `files/Hong_CV.pdf`      | CV (every "CV" link points here)              |
| `files/papers/`          | Paper PDFs and slides                         |
| `CNAME`                  | Tells GitHub Pages to serve `weitinghong.com` |
| `.nojekyll`              | Tells GitHub Pages to serve files as-is       |

The navigation bar is copied into each page. If you add a page, update the
`<nav>` in every `.html` file.

## Preview locally

Double-click `index.html`, or run a local server from this folder:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000>.

## Publish (one-time setup)

### 1. Put the site on GitHub

1. Create a free account at <https://github.com>.
2. Create a new **public** repository named `<username>.github.io`
   (e.g. `weitinghong.github.io`). Don't add a README.
3. Upload this folder's contents, using either:
   - **Browser:** on the new repo page, click "uploading an existing file"
     and drag in everything in this folder, **including** `CNAME` and
     `.nojekyll`. You don't need `.claude/`.
   - **git:**
     ```bash
     git init -b main
     git add .
     git commit -m "Initial site"
     git remote add origin https://github.com/<username>/<username>.github.io.git
     git push -u origin main
     ```
4. In the repo, open **Settings > Pages**. Under "Build and deployment", set
   Source to **Deploy from a branch**, Branch to **main**, folder to **/ (root)**.
   Then set **Custom domain** to `weitinghong.com` and click Save.

### 2. Point the domain at GitHub (Squarespace)

In Squarespace, go to **Domains > weitinghong.com > DNS > DNS Settings**.

1. **Delete the "Squarespace Defaults" records**: four `A` records
   (198.185.159.x / 198.49.23.x) and the `www` CNAME to
   `ext-sq.squarespace.com`. They conflict with GitHub's records.
2. Add these **Custom Records**:

   | Host  | Type  | Data                     |
   |-------|-------|--------------------------|
   | `@`   | A     | `185.199.108.153`        |
   | `@`   | A     | `185.199.109.153`        |
   | `@`   | A     | `185.199.110.153`        |
   | `@`   | A     | `185.199.111.153`        |
   | `@`   | AAAA  | `2606:50c0:8000::153`    |
   | `@`   | AAAA  | `2606:50c0:8001::153`    |
   | `@`   | AAAA  | `2606:50c0:8002::153`    |
   | `@`   | AAAA  | `2606:50c0:8003::153`    |
   | `www` | CNAME | `<username>.github.io`   |

   The AAAA (IPv6) records are optional but recommended.

3. Wait for DNS to update. It usually takes minutes, but can take up to
   24–48 hours. Check progress with:
   ```bash
   nslookup weitinghong.com 8.8.8.8
   ```
   It's done when the results show the 185.199.x.153 addresses.

### 3. Turn on HTTPS

Go back to the repo's **Settings > Pages**. Once the DNS check passes, tick
**Enforce HTTPS**. The certificate can take up to an hour to be issued.

### 4. (Recommended) Verify the domain

In your GitHub **profile** settings (not the repo), open **Pages > Add a
domain** and follow the steps. You'll add one TXT record in Squarespace.
This stops anyone else's GitHub repo from claiming your domain.

## Updating the site

Edit the files, then upload the changed files (browser) or run:

```bash
git add .
git commit -m "Update CV"
git push
```

GitHub republishes in about a minute.

To update your CV, replace `files/Hong_CV.pdf` with a file of the same name.
The copy here is compiled from `Dropbox\CV\Hong, Weiting - CV.tex` with the
home address and phone number removed. Remove them again in any new version
before publishing it.

## Sharing code and data

- **Page:** fill in `code-data.html`, then add
  `<a href="code-data.html">Code &amp; Data</a>` to the `<nav>` in every page.
- **Code:** keep each project in its own GitHub repository and link it from
  `code-data.html`.
- **Data:** GitHub rejects files over 100 MB and recommends keeping
  repositories under 1 GB. For datasets, deposit them in an archive that
  issues a DOI (Zenodo, openICPSR, Harvard Dataverse) and link to them.
  Small files can go in `files/`.
