# Design-system HTML — anatomy & scaffold

## The 14-section anatomy (sticky TOC links to each)
1. **Hero** — dark brand-bg with a motif (blueprint grid for engineering, dot-grid + `</>`
   for dev, etc.), mono kicker, big display H1 with one accent word, 1-line value prop,
   mono meta row, accent bottom-border.
2. **Logo & wordmark** — the mark concept + wordmark on light AND dark box; clear-space note.
3. **Color** — grouped swatch rows (ink / accent / neutrals+functional) each with name, hex,
   usage note; close with the 60/30/10 rule line.
4. **Typography** — 3 family cards (display / body / character) + a specimen ladder
   (display 52 → heading 34 → subhead 22 → body 16 → mono label).
5. **Spacing & grid** — 8pt bar scale + radius/container/gutter note.
6. **Buttons** — primary / dark / ghost + size row. One primary per view.
7. **Tags & badges** — stack pills + trust pills + status dots.
8. **Stats / trust bar** — 4 real numbers from the site (proof that closes).
9. **Cards** — 2 service cards + 2 case-study cards (gradient cover + result metrics).
10. **Signature component(s)** — the client's OWN methodology rendered reusable (e.g. an
    N-step delivery framework; or a process pipeline + a pricing/estimate card). This is the
    spine of future proposals — make it strong.
11. **Pricing / scope table** — dark header, mono prices, zebra rows, `£X,XXX` placeholders.
12. **Callout / quote** — one real positioning line or testimonial, accent left-bar.
13. **Voice & tone** — voice chips + Do / Don't columns using the client's real copy.
14. **Next-phase bridge** — dark section: 2 proposal templates (A call-deck / B send-proposal)
    + the ICP-swappable-blocks note (swap 3 blocks: pain / case / scope-price). This is
    Viktor's standard proposal architecture — keep it consistent across clients.

## CSS-variable scaffold (adapt hexes per brand)
```css
:root{
  /* INK — dark brand base, 4-5 steps */
  --ink-900:#…; --ink-800:#…(core); --ink-700:#…; --ink-600:#…; --ink-500:#…(muted text);
  /* ACCENT — the one hero color, 5-6 steps */
  --acc-700:#…(deep/hover); --acc-600:#…; --acc-500:#…(primary); --acc-300:#…; --acc-100:#…(tint); --acc-50:#…;
  /* NEUTRALS + FUNCTIONAL (tint functional toward brand hue) */
  --paper:#…; --n-200:#…; --n-300:#…; --n-400:#…; --white:#fff;
  --ok:#…; --warn:#…; --err:#…;
  /* TYPE */
  --font-display:'…'; --font-body:'Inter'; --font-mono:'…';
  --maxw:1080px;
}
```
Shared conventions: `section{padding:84px 0;border-bottom:1px solid var(--n-200)}`,
`.wrap{max-width:1080px;margin:0 auto;padding:0 32px}`, sticky blurred TOC, mono section
labels, hover lift on cards. Mobile: hide TOC, shrink hero H1, collapse grids to 1-2 col.

## Font pairing guidance (pick a CHARACTER font that fits the vertical)
- Display = geometric/grotesque (Space Grotesk, Plus Jakarta Sans, Archivo, Clash) for warmth/structure.
- Body = **Inter** (default workhorse) or the site's real body font if clean.
- Character = the differentiator:
  - Engineering / construction / data → **IBM Plex Mono** (specs, ISO codes).
  - Software / dev / "code" brands → **JetBrains Mono** / Geist Mono.
  - Finance / legal → a serif accent (e.g. Fraunces) instead of mono.
  Use the character font for mono labels, stat captions, tags, prices.

## Distinctiveness rule
Every client must look different: vary the hero motif, corner radius (corporate = 6-10px
sharper; friendly = 12-18px rounder), accent discipline, and the signature component(s).
Never ship two clients with the same layout — use the engineering vs dev archetypes as two poles.

## Port & deploy bookkeeping
Take the next free 460X/461X port (check existing `.claude/launch.json` entries).
Deploy: `npx netlify-cli deploy --dir . --prod` then rename via `api updateSite` with the
UUID from `.netlify/state.json`. Final URL: `https://<client>-design-system.netlify.app`.
