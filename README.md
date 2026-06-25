# Emotion Wheel Interactive Tool — Soluzioni PDF

A standalone, embeddable web widget for `soluzionipdf.it`, designed following Roberto Serra's GEO framework: useful to the host site's audience, lightweight, non self-promotional, brand-present but not aggressive.

The widget content (UI strings, emotion names, definitions) is in **Italian**. The host site is `soluzionipdf.it` and the target audience is Italian-speaking users. Do not translate the widget content during integration.

## Files in this folder

| File | Description |
|---|---|
| `index.html` | Standalone widget. Single file (HTML + inline CSS + inline JS), no JS dependencies, ~45 KB + ~40 KB Google Fonts (preconnected, `display:swap`). Premium magazine-style design aligned with the Soluzioni PDF brand (ivory/warm gold palette, Playfair Display + Inter typography). |
| `embed-snippet.html` | The exact code third-party sites need to copy in order to embed the tool on their pages. Includes SEO-friendly context paragraph above the iframe (required by the Roberto Serra framework). |
| `README.md` | This document — deployment guide for the freelancer. |

## Demand validation (DataForSEO, Italy)

| Keyword | Monthly searches | Competition |
|---|---|---|
| ruota delle emozioni | 5,400 | LOW |
| ruota emozioni | 880 | LOW |

Aggregate Italian demand ≈ 6,300 / month, low competition. Multiple Italian articles exist on the topic, but no professional-quality interactive Italian tool — clear opportunity.

## Brand context

Soluzioni PDF (`soluzionipdf.it`) is an Italian wellness publishing project by Sara Borghi. It sells practical PDF guides on mental health, sleep, women's wellness, and personal growth, all priced at €9.99 and grounded in peer-reviewed science.

The widget follows the same editorial register: scientific but accessible, warm and personal ("an informed friend"), no clinical jargon. The visual language is taken directly from the Soluzioni PDF site CSS palette:

- **Background**: `floralwhite #FBFBF3` (brand)
- **Ink**: `darkslategray #2E2F2B` (brand)
- **Accent (gold/tan)**: `#A8865E` (brand `tan`)
- **Borders**: `#D5D0C4` (brand `lightgray` warm)
- **Typography**: Playfair Display (serif, italic accents) for headings and prompts; Inter (sans-serif) for UI and meta text
- **Cluster emotion colors** (derived from the brand palette):
  - Gioia (Joy) → `#A8865E` (tan)
  - Tristezza (Sadness) → `#528596` (slategray)
  - Paura (Fear) → `#8B6E94` (mauve, tonally aligned)
  - Rabbia (Anger) → `#9B5343` (sienna)
  - Sorpresa (Surprise) → `#66A38D` (cadetblue)
  - Disgusto (Disgust) → `#4A6850` (dark sage, brand-aligned)

The widget links back to the brand only in two non-intrusive places: the header (logo + "Esplora le guide →" CTA) and a single promo card before the footer (CTA to `/products/intelligenza-emotiva`). Per Roberto Serra's framework, the tool itself must be useful regardless of who made it.

## Deploy to Shopify

The host site is on Shopify. The widget must live at the URL `https://soluzionipdf.it/pages/ruota-emozioni` so the embed iframe `src` (already hardcoded in `embed-snippet.html`) resolves correctly.

### Step 1 — Create a dedicated Shopify Page Template

The widget needs a stripped-down page template that does **not** render the site header, footer, or sidebar. The iframe (when used by third-party sites) should display only the widget, not the full Soluzioni PDF site chrome.

1. In the Shopify Admin, go to `Online Store → Themes → [Live theme] → Edit code`.
2. In the **Templates** folder, click **Add a new template**.
3. Choose:
   - **For**: `page`
   - **File name**: `ruota-emozioni`
   - **File type**: `liquid` (or `JSON` if the theme uses JSON templates — Dawn 7.0+ uses JSON; in that case create `page.ruota-emozioni.json` referencing a stripped section)
4. If using `.liquid`: replace the file contents with the minimal template below. If using `.json`: create a corresponding section file `sections/page-bare.liquid` and reference it from the JSON template.

Minimal `page.ruota-emozioni.liquid` (Liquid version):

```liquid
<!doctype html>
<html lang="{{ request.locale.iso_code }}">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{ page.title | escape }} — {{ shop.name }}</title>
    {%- if page.meta_description != blank -%}
      <meta name="description" content="{{ page.meta_description | escape }}">
    {%- endif -%}
    {{ content_for_header }}
  </head>
  <body style="margin:0;padding:0;background:#F5F1EA;">
    {{ page.content }}
  </body>
</html>
```

This template skips the global `theme.liquid` wrapping (no header, no nav, no footer, no cart drawer). It's required so the iframe renders cleanly on third-party sites.

### Step 2 — Create the Shopify Page

1. In the Shopify Admin, go to `Online Store → Pages → Add page`.
2. Fields:
   - **Title**: `Ruota delle Emozioni Interattiva`
   - **Online store → Theme template**: select `ruota-emozioni`
   - **Search engine listing → URL handle**: `ruota-emozioni` (final URL: `https://soluzionipdf.it/pages/ruota-emozioni`)
   - **Meta description**: `Strumento interattivo gratuito per nominare con precisione le emozioni. Sei primarie, trenta secondarie, oltre cento sfumature. Basato sui modelli di Plutchik, Wilcox e Junto.`
3. In the **Content** field, switch to the HTML view (`</>` icon) and paste the contents of `index.html` — but **only** the contents between `<body>` and `</body>` (exclusive). Specifically: keep the entire `<style>...</style>` block, the `<div id="ruota-app">...</div>` block, and the `<script>...</script>` block. Do NOT paste `<!DOCTYPE html>`, `<html>`, `<head>`, `<meta>`, the Google Fonts `<link>` tags, `<body>`, or the closing tags.
4. The Google Fonts `<link>` tags must be moved into the page template's `<head>` (or added globally). Add this immediately before `{{ content_for_header }}` in `page.ruota-emozioni.liquid`:

```liquid
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;0,700;0,800;1,500;1,700&family=Inter:wght@400;500;600;700&display=swap">
```

5. Save the page.

### Step 3 — Test

1. Visit `https://soluzionipdf.it/pages/ruota-emozioni` directly.
2. Confirm:
   - The page loads with only the widget visible (no Soluzioni PDF site header/footer around it).
   - The SVG wheel renders with 6 colored sectors and labels.
   - Clicking a primary emotion opens the secondary level grid.
   - Clicking a secondary opens the tertiary chips.
   - Clicking a tertiary populates the detail panel on the right (or below on mobile).
   - The "Copia la frase" (Copy the sentence) button works and shows the green check feedback.
   - The page is responsive down to 320 px width.
   - Lighthouse performance score: target ≥ 90 (mobile + desktop).

### Step 4 — Companion blog article (SEO entry point)

Create a new blog post in `Online Store → Blog posts → Intelligenza Emotiva`:

- **Title**: `Ruota delle Emozioni: lo Strumento Interattivo Gratuito`
- **Excerpt**: 1–2 paragraphs introducing the tool and what it does
- **Content**: a 600–800 word article explaining the three scientific models (Plutchik, Geneva, Wilcox), the Lieberman 2007 study on affect labeling, and how to use the tool. Embed the tool inside the post via:

```html
<iframe src="https://soluzionipdf.it/pages/ruota-emozioni" 
        style="width:100%;height:900px;border:1px solid #1A18131A;border-radius:8px;" 
        loading="lazy" 
        title="Ruota delle Emozioni Interattiva"></iframe>
```

This article is the main SEO entry point for the keyword "ruota delle emozioni" (5,400 monthly searches in Italy).

### Step 5 — Internal linking

Update the existing article `/blogs/intelligenza-emotiva/ruota-delle-emozioni` to include a prominent callout box at the top linking to the interactive tool at `/pages/ruota-emozioni`. Add similar callouts in the pillar article `/blogs/intelligenza-emotiva/intelligenza-emotiva` and in the product page `/products/intelligenza-emotiva`.

## Distribution to third-party sites (the GEO play)

Roberto Serra's argument: **one well-built tool on 20 industry sites is worth more than many ad campaigns**. The mechanism is that every embed becomes an "earned media" contextual brand mention in the corpus used by AI models like ChatGPT, Claude, and Gemini.

### Outreach strategy (3 tiers)

**Tier 1 — Sites already covering "ruota delle emozioni"**
Search `"ruota delle emozioni" site:.it` to find Italian psychology, parenting, mindfulness, and HR blogs that have already written on the topic. These are the warmest leads: they have the audience, they have the article, they only need the embed code. Send them `embed-snippet.html` with a short personal email (Italian, template available on request).

**Tier 2 — Pages with online psychological self-tests**
Search `"test psicologico" OR "psicotest" OR "self test" site:.it`. Pitch the tool as complementary to their tests (the wheel is NOT a diagnostic instrument, so it doesn't compete with them).

**Tier 3 — Women's magazines, parenting blogs, HR / burnout blogs**
Magazines like Vanity Fair, Donna Moderna, parenting blogs, HR portals (Welfarexme, Glickon). Same outreach with tone tweaks per audience.

**Realistic expectation**: 20–50 outreach emails → 5–10 embeds. Each embed is a contextual "earned" mention in the AI corpus that persists for months or years (unlike a guest post link that ages out).

## What the tool does

1. **Step 1 — Primary**: 6 primary emotions arranged as an SVG donut wheel (Gioia, Tristezza, Paura, Rabbia, Sorpresa, Disgusto). Each has a signature color from the editorial palette.
2. **Click a primary**: the secondary level opens — 5 secondary emotions in a grid (e.g., inside Tristezza: Solitudine, Vulnerabilità, Disperazione, Senso di colpa, Delusione).
3. **Click a secondary**: 5 tertiary chips appear (e.g., inside Solitudine: Trascurata, Isolata, Esclusa, Abbandonata, Ignorata).
4. **Click a tertiary**: the detail panel populates with a 1-line definition + a diary-prompt sentence ("Mi sento trascurata quando ___") + a "Copy the sentence" button that writes to the clipboard.
5. **Science callout**: every detail card displays the mini-box "La scienza dietro" (Lieberman, UCLA 2007) referencing the fMRI affect labeling study.

Total dataset: 6 primary × 5 secondary × 5 tertiary = **150 emotion nodes**.

## Technical specifications

- Single HTML file, ~45 KB (plus ~40 KB Google Fonts, preconnected and lazy-loaded via `display:swap`).
- Premium typography stack: Playfair Display (700/800 + italic) for headings, Inter (400/500/600/700) for UI.
- Brand-aligned palette matching `soluzionipdf.it`.
- Zero external JS dependencies (no jQuery, no React, no analytics scripts).
- Zero cookies, zero tracking.
- Mobile-first responsive, tested down to 320 px width.
- Accessibility: aria-label on all interactive elements, visible focus states, keyboard navigation (Enter/Space), AA contrast ratios.
- Sticky detail panel on desktop (`position: sticky`); stacked layout on mobile.
- Subtle entrance animations using `cubic-bezier(.16,1,.3,1)` easing; hover states with translate + box-shadow + scale.

## What the tool does NOT do (by design)

- It is **not** a diagnostic instrument — no scoring, no profile, no clinical classification. This is intentional to maximize adoption by serious third-party sites (psychology blogs, healthcare portals) that would be cautious about embedding a third-party diagnostic test for ethical/legal reasons.
- It does **not** collect user data — no analytics, no email opt-in, no localStorage.
- It does **not** have aggressive CTAs — only one discreet promo card linking to the Intelligenza Emotiva guide, before the footer.

## Optional enhancements (post-launch)

If post-launch metrics justify additional investment, here are 4 ideas in increasing order of effort:

1. **Privacy-friendly analytics**: add Plausible or Fathom (no cookies, GDPR-friendly) to track how often the tool is opened and which emotion paths are most clicked. Useful for editorial insight.
2. **Shareable card**: add a "share on WhatsApp/Instagram" button that generates a PNG card with the selected emotion + the diary prompt. Drives viral distribution.
3. **Quick-start quiz**: a 3-question mini quiz that suggests a primary emotion to start from. Lowers the cold-start friction.
4. **English version**: same tool, translated dataset and copy. The English-language demand for "emotion wheel" is much higher (~40k monthly searches on .com) and the embeddable angle would open distribution to international wellness blogs.

## Open questions for the freelancer

When picking up this work, please confirm with Sara:

1. **Final URL slug**: confirmed as `soluzionipdf.it/pages/ruota-emozioni`? If different, update the `src` attribute in `embed-snippet.html`.
2. **Theme**: which Shopify theme is in use (Dawn / Sense / Refresh / custom)? This affects whether the page template should be `.liquid` or `.json` and how `theme.liquid` is structured.
3. **Cookie banner**: does the current site display a cookie/consent banner on every page? If yes, confirm whether the banner should appear on the `/pages/ruota-emozioni` page (for direct visitors) or be suppressed for the iframe-embed scenario.
4. **Google Fonts**: is the brand already loading Playfair Display + Inter elsewhere? If yes, we can reuse the existing font loading instead of adding new `<link>` tags. If the brand uses different font choices on the rest of the site (e.g., a custom font hosted on the theme), let us know and we can adjust the widget's font stack to match for full visual consistency.

---

**Status**: ready to ship. Estimated freelancer effort: 1–2 hours for the Shopify integration, plus QA across mobile/desktop.