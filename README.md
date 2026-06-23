# Synathra Lab Static Site Skeleton — SITE-0.2

This package is a mobile-first static skeleton for the first public Synathra Lab website.

## Purpose

The site is not a file showcase. It is a trust, explanation and feedback layer for the Synathra method.

The first public proof case is:

**Synathra.Sport v0.5 — domain-transfer validated football event-chain risk twin**

## Included pages

```text
/
/en/
/en/sport/v0-5/
/en/sport/v0-5/validation/
/en/sport/v0-5/demo/
/en/sport/v0-5/docs/
/en/about/
/en/contact/
/ru/                  # placeholder for later Russian layer
```

## Included assets

```text
assets/css/styles.css
assets/data/validation_summary.csv
assets/data/demo_events.csv
assets/data/feedback_log_template.csv
assets/docs/*.placeholder.md
robots.txt
sitemap.xml
```

## How to preview locally

Open `index.html` directly in a browser, or run a local static server:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

## What must be replaced before public launch

1. Replace all `TBD` validation metrics with final v0.5 release values.
2. Replace placeholder documents in `assets/docs/` with final public docs and PDFs.
3. Replace `https://synathra-lab.example` in HTML canonical URLs and `sitemap.xml` with the real domain.
4. Replace the contact form `action="#"` with the chosen form handler.
5. Add an Open Graph image in `assets/img/` and reference it in page metadata.
6. Run Lighthouse and fix any performance, accessibility or SEO issues.

## Deployment options

Recommended first deployment:

- Cloudflare Pages
- Netlify
- GitHub Pages
- Vercel static deployment

No backend is required for SITE-0.2.
