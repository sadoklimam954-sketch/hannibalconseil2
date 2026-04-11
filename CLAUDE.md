# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static website for **Hannibal Conseil**, a French railway signalling engineering firm. No build step, no package manager — pure HTML/CSS/JS, deployed on GitHub Pages at `hannibalconseil.fr`.

## Deployment

```bash
git add .
git commit -m "description"
git push   # triggers GitHub Pages auto-deploy to hannibalconseil.fr
```

**After every code change: always run the three commands above automatically.**

## Architecture

All pages share a single stylesheet (`style.css`) and embed page-specific styles in a `<style>` block inside `<head>`. There is no component system — nav, footer, and WhatsApp button are copy-pasted across pages.

### Files
| File | Role |
|---|---|
| `style.css` | Global: CSS variables, nav, footer, animations, WhatsApp button, newsletter, rail dividers, responsive breakpoints |
| `index.html` | Homepage: full-screen hero, stats, split presentation, 3 service cards, "Notre approche" dark section, CTA band |
| `references.html` | Project references: 3-column table, photo gallery, client logos |
| `contact.html` | Contact form (Netlify Forms) + info panel |
| `mentions-legales.html` / `privacy-policy.html` | Legal pages |
| `merci.html` | Post-form submission confirmation page |
| `CNAME` | GitHub Pages custom domain — do not delete |

### CSS variables (defined in `style.css :root`)
```
--green: #1a6b3a        main brand green
--green-acc: #6dd99a    accent/highlight green
--dark: #0c1410         near-black backgrounds
--bg: #f5f5f3           off-white section backgrounds
--border: #e2e2dc       dividers
--nav-h: 70px           nav height (used in hero padding calc)
```

### Footer structure (all pages)
The footer always has two parts nested as follows:
```html
<footer>
  <div class="footer-newsletter">…</div>   <!-- dark bg, newsletter form -->
  <div class="footer-main">                <!-- padded wrapper -->
    <div class="footer-inner">…</div>      <!-- 3-column grid -->
    <div class="footer-bottom">…</div>     <!-- legal links row -->
  </div>
</footer>
```
`footer` itself has no padding — padding lives in `.footer-main`. Secondary pages (`mentions-legales.html`, `privacy-policy.html`) omit the newsletter block but must keep the `.footer-main` wrapper.

### Scroll reveal
Elements with classes `reveal`, `reveal-r`, `reveal-l` start invisible and animate in via `IntersectionObserver`. Delay classes `d1`/`d2`/`d3` stagger them. The JS is inlined at the bottom of each page.

### Nav behaviour
- Transparent on the homepage hero, `opaque` class hard-set on internal pages.
- On scroll past 80 px: adds `sticky` class → white bg + `backdrop-filter: blur(20px)`.
- Hides (translateY -100%) when scrolling down past 200 px, reappears on scroll up.

### Scroll progress bar
`<div class="scroll-progress" id="scroll-progress">` must be the **first child of `<body>`** on every page. Width is updated via inline JS at the bottom of each page.

### Contact form
Uses **Netlify Forms** (`data-netlify="true"`, `data-netlify-honeypot="bot-field"`, `action="/merci.html"`). No backend required; Netlify handles submission on the hosted version.

### Images
Local images: `ferro.jpeg`, `ferro1.jpg`, `fff.jpg`. All other images are remote URLs (Unsplash, Pexels, Zyrosite CDN). The Zyrosite CDN URLs use `cdn-cgi/image` transform params — keep them as-is.
