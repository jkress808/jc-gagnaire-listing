# JC Gagnaire — Listing Site Template

Self-contained single-property listing site. One repo per listing, hosted on GitHub Pages, optionally pointed at a custom domain.

**Live demo / template:** https://jkress808.github.io/jc-gagnaire-listing/

---

## How a new listing works

1. **Create a new repo from this template**
   On GitHub, click **Use this template → Create a new repository**. Name it after the address, e.g. `3602-ne-103rd-st`.
   Or via CLI:
   ```
   gh repo create jkress808/3602-ne-103rd-st --template jkress808/jc-gagnaire-listing --public --clone
   ```

2. **Drop photos into the `photos/` folder** using these filenames:

   | File | Purpose |
   |------|---------|
   | `photos/hero.jpg` | Full-width hero banner (top of page) |
   | `photos/01.jpg` ... `photos/50.jpg` | Gallery photos, shown in numerical order. First one is featured (16:9). |
   | `photos/floorplan-1.jpg` | Main level floor plan (optional) |
   | `photos/floorplan-2.jpg` | Upper level (optional) |
   | `photos/floorplan-3.jpg` | Lower level (optional) |
   | `photos/broker.jpg` | Broker headshot (optional — falls back to initials) |

   `.jpg`, `.jpeg`, `.png`, and `.webp` all work. Photos that don't exist are simply skipped.

3. **Edit the listing data** in `index.html`. Near the top of the file there's a clearly-marked block:

   ```html
   <script id="listing-data" type="application/json">
   {
     "address": "3602 NE 103rd St",
     "city": "Seattle, WA 98125",
     "price": "$1,295,000",
     "stats": { "beds": "4", "baths": "2.5", "sqft": "2,400", "lot": "5,200" },
     ...
   }
   </script>
   ```

   Update every field. Keep the JSON valid (use double quotes, no trailing commas).

4. **Commit and push:**
   ```
   git add -A
   git commit -m "Set up listing for 3602 NE 103rd St"
   git push
   ```

5. **Enable GitHub Pages** (one click per repo):
   `Settings → Pages → Source: Deploy from branch → main / root → Save.`
   Or via CLI:
   ```
   gh api -X POST repos/jkress808/<repo-name>/pages -f "source[branch]=main" -f "source[path]=/"
   ```
   The site will be at `https://jkress808.github.io/<repo-name>/` within ~1 minute.

6. **(Optional) Point a custom domain** at the listing — see below.

---

## Custom domain per listing

Buy each listing's domain at **Cloudflare Registrar** or **Porkbun** (~$10/yr `.com`, no upsells). Pick a name tied to the address — e.g. `3602ne103rdst.com`. Drop the domain when the home sells.

**Hooking it up to GitHub Pages:**

1. In the listing repo: `Settings → Pages → Custom domain` → enter your domain → **Save**.
2. At the registrar, set DNS:

   | Type | Name | Value |
   |------|------|-------|
   | A | `@` | `185.199.108.153` |
   | A | `@` | `185.199.109.153` |
   | A | `@` | `185.199.110.153` |
   | A | `@` | `185.199.111.153` |
   | CNAME | `www` | `jkress808.github.io` |

3. Wait 5–15 minutes, then return to `Settings → Pages` and check **Enforce HTTPS**.

That's it. Repeat steps 1–6 (and optionally the domain steps) for every new listing.

---

## Inquiry form

Submissions go to the email address in `broker.email` (default `jc@jcgagnaire.com`) via [Formsubmit.co](https://formsubmit.co/) — no backend needed.

The first submission to a new email address triggers a one-time confirmation email from Formsubmit. Click the activation link in that email to enable forwarding for that inbox. After that, every inquiry from any of your listing sites lands in the inbox automatically — same activation works for every repo since the inbox is the same.

---

## Local preview

Open `index.html` directly in a browser, or for a more accurate preview that resolves the `photos/` folder paths the same way GitHub Pages will:

```
python -m http.server 8000
# then open http://localhost:8000
```

---

## Files

```
index.html       Single-file site. Has listing-data JSON near top for per-listing edits.
photos/          Drop photos here. Filenames are by convention (see table above).
README.md        This file.
```

No build step, no dependencies beyond Leaflet (loaded from CDN at runtime).
