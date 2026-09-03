# Skinly marketing site

A single static, self-contained landing page for the Skinly / Derma Intelligence startup. No build step, no framework, no backend calls, no third-party requests at runtime.

```
website/
  index.html        # everything: markup, CSS, i18n (DE/EN), mock demo logic
  impressum.html     # legal notice (§5 DDG) for the Einzelunternehmen — see below
  datenschutz.html   # privacy notice (DSGVO) for this static site + the planned app
  fonts/              # self-hosted Inter + Playfair Display (no Google Fonts CDN)
```

## Preview locally

Just open `index.html` in a browser — no server required:

```
website/index.html
```

If you'd rather serve it (e.g. to test relative paths exactly as a host would):

```bash
cd website && python -m http.server 8080
```

## Deploy

Any static host works — the whole thing is 4 files plus `fonts/`. Drag-and-drop the `website/` folder onto Netlify or Vercel, or push it to a GitHub Pages branch. No environment variables, no build command, no `package.json` needed.

## Renaming the brand

The name is currently a placeholder (**Skinly** / product name **Derma Intelligence**) and deliberately kept in as few places as possible:

1. `index.html` → the `BRAND` object at the top of the `<script>` block (company, product, **email**, domain). The `mailto:` contact link is generated from `BRAND.email` — change it there, nowhere else.
2. `index.html` → `<title>` and the two `og:title` / `og:description` meta tags near the top of `<head>`.
3. `impressum.html` and `datenschutz.html` → `<title>` tags and the footer line.
4. Anywhere the wordmark "Skinly" appears in markup (header, mobile nav, footer) — these are plain text, not templated, so a find-and-replace across the three HTML files covers it.

## Before this goes public

- **`impressum.html` and `datenschutz.html` reflect the current status**: David Jahn operating as an Einzelunternehmer (no UG/GmbH yet), no Handelsregister entry, no Umsatzsteuer-ID — all correctly omitted rather than left blank, since none apply to that legal form. Both are marked `noindex`. If/when a UG or GmbH is founded, `impressum.html` must be updated the same day (Rechtsform + Handelsregister/HRB number become mandatory the moment the entity is registered).
- `BRAND.email` is set to a real inbox (`davidajahn1104@gmail.com`) — update it if a dedicated business address is set up later.
- Not yet covered in `datenschutz.html`: a line disclosing that the static host's access logs (IP, user-agent, timestamp) are processed under Art. 6(1)(f) DSGVO (legitimate interest) — every HTTP host does this even with zero analytics/cookies. Add once a hosting provider is chosen.
- The **demo section is a client-side mock**: ~11 hardcoded example products and simplified scoring rules, explicitly labeled "Illustrative Simulation" in both languages. It does not call the FastAPI backend and never renders a raw numeric score — only the same four ordinal match bands (`strong` / `good` / `some benefit`) the real API returns, on purpose (see `docs/API_CONTRACT.md` in the main repo for why raw scores are never shown to users).
- No analytics, no cookies, no tracking are wired up. If you add any, you'll also need a cookie notice and to update `datenschutz.html` beyond the placeholder.

## Why fonts are self-hosted

The site does not load fonts from `fonts.googleapis.com` at runtime — the woff2 files live in `fonts/` and are declared via local `@font-face`. This is deliberate: German courts have held that embedding the Google Fonts CDN transfers a visitor's IP to Google without consent, which would be a strange thing to ship on a site whose whole pitch is EU-first data handling. If you add any other embed (video, maps, forms), re-check this constraint before adding it.
