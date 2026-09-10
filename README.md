# Puzzle Peace ADHD Coaching and Psychotherapy — Website

Static site for Maggie Hall's practice. Plain HTML, CSS and one small JS file —
no build step, no framework, no dependencies. Edit a file, commit, push.

**Live:** https://puzzlepeaceadhd.co.uk
**Repo:** `prismatic-minds/puzzle-peace-website`
**Host:** Netlify, auto-deploying from `main`

---

## Files

```
404.html              Served for any unmatched URL
index.html            Home
about.html            About Maggie
services.html         Overview of both services
adhd-coaching.html    ADHD coaching, how it works, pricing
psychotherapy.html    Integrative psychotherapy, how it works, pricing
contact.html          Phone and email, what happens next
privacy-policy.html   UK GDPR privacy policy
css/style.css         All styles
js/main.js            Mobile nav toggle and active-link highlighting
images/               See "Images" below
netlify.toml          Publish directory, security headers, cache rules
robots.txt            Allows everything, points at the sitemap
sitemap.xml           All seven public pages
```

## Working on it locally

```bash
python -m http.server 8777
```

Then open http://localhost:8777. Use a server rather than opening the files
directly — `404.html` and the root-absolute paths need one to behave correctly.

## Deploying

Push to `main`. Netlify builds and publishes automatically; there is no build
command, the repo root *is* the site.

---

## Domains

| Domain | Behaviour |
|---|---|
| `puzzlepeaceadhd.co.uk` | Canonical. Every `<link rel="canonical">` and `og:url` points here. |
| `www.puzzlepeaceadhd.co.uk` | 301 to the apex |
| `puzzlepeaceaudhd.co.uk` | Typo-guard alias, 301 to the apex |

Redirects and the alias are configured as **domains in the Netlify UI**, not as
redirects in `netlify.toml`. Registrar is Porkbun.

If the domain ever changes, the absolute URLs are hardcoded in all seven pages
(canonical, `og:url`, `og:image`), plus `robots.txt`, `sitemap.xml` and the
JSON-LD blocks in `index.html` and `contact.html`. Grep for the old domain.

---

## Things that will bite you

**`img { max-width: 100%; height: auto; }` in the reset must stay.** Images
carry `width`/`height` attributes so they reserve space while loading, but those
attributes also set a *presentational height* that beats `aspect-ratio`. Without
`height: auto`, any image using `aspect-ratio` renders at full intrinsic height.
This shipped as a bug once already.

**`404.html` uses root-absolute paths throughout** (`/css/style.css`, not
`css/style.css`). Netlify serves it from any unmatched URL, so relative paths
would resolve against whatever directory the visitor asked for and the page
would come out unstyled with dead links. Keep every path in that file starting
with `/`.

**The CSP in `netlify.toml` needs `'unsafe-inline'`** for scripts and styles,
because the pages use inline `style` attributes throughout and an inline
`onerror` on the header logo. Moving those into the stylesheet and `main.js`
would let it be tightened. The CSP otherwise allows only Google Fonts
(`fonts.googleapis.com` for the stylesheet, `fonts.gstatic.com` for the files).

**Images cache for a week and are not content-hashed.** Replacing one in place
can serve stale for up to seven days. Rename the file instead.

**Adding a page?** Add it to `sitemap.xml`, the footer sitemap column, the nav if
it belongs there, and give it a canonical tag and OG block matching the others.

**There is deliberately no contact form.** Maggie asked for phone and email only.

---

## Images

| File | Used for | Notes |
|---|---|---|
| `logo.png` (480×480) | Homepage hero, JSON-LD `logo` | Don't shrink below 480 — the hero renders it at up to 240px |
| `logo-mark.png` (235×180) | Header | The brain-dove alone; the full logo's wordmark is illegible at header size and duplicates the text beside it |
| `favicon.png` (32×32) | Browser tab | |
| `apple-touch-icon.png` (180×180) | iOS home screen | |
| `og-image.png` (1200×630) | Social sharing card | Logo on the brand cream |
| `maggie.webp` (860×1146) | About and home | WebP, no JPEG fallback |

Total ~200KB across the site. Keep it that way: resize to roughly twice the
displayed size and compress, rather than dropping in camera originals.

---

## Design notes

- **Font:** Nunito, from Google Fonts — dyslexia-friendly rounded letterforms
- **Background:** `#fff7d0` warm cream, never pure white
- **Text:** `#284859` dark teal, never pure black
- **Accents:** `#91c0bc` teal, `#bbd1b0` sage
- **Line length** capped at 65 characters, **line height** 1.85
- **Left-aligned throughout** — no justified text, for dyslexia accessibility
- Hamburger nav below 768px

These aren't arbitrary: the whole palette and typography are chosen for
readability by neurodivergent visitors. Keep that in mind before changing them.

---

## Registrations shown in the footer

ICO `ZC033803` · UKCP membership `10161098`
