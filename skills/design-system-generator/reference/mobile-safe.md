# Mobile-safe layout — mandatory rules

A design system is opened on a phone more often than on a laptop (the client forwards the
link in a messenger). On 2026-10-07 a mobile audit found 8 deployed design systems that
scrolled sideways at 360/390px (+27 … +296px). Almost every case was the same mechanism,
repeated because the template only said "collapse grids on mobile". These rules close it.

## The one mechanism behind most breaks

`1fr` is `minmax(auto, 1fr)`. The `auto` minimum = the min-content width of the widest child.
One unbreakable child — a 52px specimen word, a pricing table, a `<pre>` with tokens, a
logo lockup with 48px padding, a stat row inside a mockup — makes the track wider than the
screen, and the whole page scrolls sideways. Collapsing to `1fr` does NOT help: one column of
`1fr` still grows to that child. The same trap exists for flex items (`min-width:auto`).

## Rules

1. **Grid tracks: never bare `1fr`.** Write `minmax(0,1fr)` / `repeat(n,minmax(0,1fr))` —
   including the page shell (`sidebar + main`) and every mobile override.
2. **Every multi-column grid is listed in the mobile `@media`.** A grid you forget
   (`.grid-note`, `.logo-grid`, the proposal-bridge grid) stays 2-3 columns on a phone.
3. **No layout grids in inline `style=""`.** A media query cannot reach them. Give the
   element a class.
4. **Tables always scroll inside their own box:** `<div class="tbl-wrap"><table>…</table></div>`
   with `overflow-x:auto`. Pricing / spec / matrix tables are always wider than 360px.
5. **Type specimen rows** (fixed-width label + big sample): `flex-wrap:wrap`, label
   `width:100%` on mobile, sample size via `clamp(32px,10vw,52px)`. Never
   `white-space:nowrap; overflow:hidden` on a specimen — it silently cuts the text.
6. **Flex children that hold wide content get `min-width:0`**, and stat rows inside
   mockups (LinkedIn banner, cover) get `flex-wrap:wrap`.
7. **Logo boxes:** padding drops to ~`32px 18px` on mobile, wordmark size via `clamp()`,
   SVG logos `width:100%; max-width:Npx` (never a fixed `width:280px`), horizontal
   mark+wordmark lockups `flex-wrap:wrap`.
8. **Never zero the `.wrap` gutters.** `.hero-inner{padding:104px 0 88px}` on an element
   that is also `.wrap` wipes the 32px side padding → hero text touches the screen edge.
   Write `padding-top` / `padding-bottom` only. Same for `footer`.
9. **Never "fix" with `overflow-x:hidden` on `html`/`body`.** It hides the scroll and clips
   the content; the bug stays.

## Base block — paste LAST in `<style>` (after all other rules, so it wins)

Rename the class lists to the page's real classes; keep every multi-column grid in them.

```css
/* MOBILE-SAFE — keep this block last in <style> */
img,svg,video{max-width:100%}
.tbl-wrap{overflow-x:auto;-webkit-overflow-scrolling:touch}
@media(max-width:860px){
  .wrap{padding-left:20px;padding-right:20px}
  .shell{grid-template-columns:minmax(0,1fr)}            /* sidebar layouts */
  .shell>*{min-width:0}
  /* every multi-column grid of the page → one column (list them ALL) */
  .logo-grid,.card-grid,.dodont,.tpl-grid,.font-cards,.grid-note,
  .pipeline,.scope,.li-row2{grid-template-columns:minmax(0,1fr)}
  .stats,.swatch-row,.gfx-grid{grid-template-columns:repeat(2,minmax(0,1fr))}
  .ico-grid{grid-template-columns:repeat(4,minmax(0,1fr))}
  .specimen>div{flex-wrap:wrap;gap:4px 20px}
  .specimen .tag{width:100%}
  .sp-display,.d1{font-size:clamp(32px,10vw,52px)}
  .logo-box{padding:32px 18px}
  .wordmark{font-size:clamp(26px,9vw,40px)}
  .li-stats{flex-wrap:wrap;gap:12px 18px}
  table{display:block;overflow-x:auto}                   /* a table that missed .tbl-wrap */
}
```

## Check before deploy (gate)

Render at **360 and 390px**. Pass = `document.documentElement.scrollWidth === innerWidth` and
no text touching or cut at the screen edge. Quick probe in the preview console:

```js
[innerWidth, document.documentElement.scrollWidth,
 [...document.querySelectorAll('body *')]
   .filter(e => e.getBoundingClientRect().right > innerWidth + 1)
   .slice(0, 10).map(e => e.tagName + '.' + e.className)]
```

If you have a headless mobile-QA script, run it on the local preview and again on the live
URL after deploy — zero failures is required before the page is called done.
