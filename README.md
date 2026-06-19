# Design System Generator — B2B Global

A Claude Code plugin that turns **a website URL into a complete, client-ready brand/design
system** — a single self-contained HTML page you can show the client and grow proposals from.

It is the repeatable B2B Global flow, codified. Two archetypes it has produced, as reference poles:

- **Engineering / construction brand** — charcoal `#1C1C1C` base + a single corporate-blue accent, blueprint character.
- **Software / dev brand** — a green hero + dev-mono, friendly-SaaS character.

---

## What it does

1. **Grounds the brand** — fetches the site for what they do, ICP, tone, real copy and proof.
2. **Extracts the REAL palette & fonts** — ranks colours by frequency straight from the live
   CSS and **filters out WordPress / Gutenberg default-palette noise** (the `#0693e3`,
   `#00d084`, `#9b51e0`… that appear on every WP site regardless of brand). Fonts come from
   Google Fonts links, `@font-face` and theme tokens.
3. **Designs a token system** — disciplined ink / accent / neutral / functional scales on a
   60/30/10 rule, plus a 3-family type system with a *character* font that fits the vertical
   (mono for engineering/dev, serif for finance, etc.).
4. **Builds a single-file HTML design system** — 14-section anatomy: hero (with a brand
   motif), logo, colour, type, spacing, buttons, tags, stats, cards, **1–2 signature
   components** (the client's own methodology — the spine of future proposals), pricing
   table, callout, voice & do/don't, and a **proposal bridge** (call-deck vs send-proposal +
   ICP-swappable blocks).
5. **Previews & deploys** — local preview, then Netlify deploy under
   `https://<client>-design-system.netlify.app`.

Each client is made visually distinct — different motif, radius and signature components.
Never a 1:1 clone of the previous one.

---

## Install

This plugin follows the standard Claude Code plugin layout. Either copy the skill into your
skills directory, or add the repo as a plugin marketplace.

```bash
# Option A — copy the skill into your user skills dir
cp -R skills/design-system-generator ~/.claude/skills/

# Option B — add as a plugin marketplace (then enable the plugin)
/plugin marketplace add victor-shulga/<repo-name>
/plugin install design-system-generator
```

## Use

Just ask, with a URL:

> зроби дизайн систему для https://example.com
> design system for example.com

The skill confirms two forks (format = single-file HTML; direction = faithful / **elevate** /
restyle), then runs the full flow end-to-end.

---

## Structure

```
.claude-plugin/
  plugin.json          # plugin manifest
  marketplace.json     # marketplace entry
skills/
  design-system-generator/
    SKILL.md           # the 6-step flow
    reference/
      brand-extraction.md   # WP-noise list + curl/grep recipe + accent/ink/neutral HSL heuristic
      structure.md          # 14-section anatomy, CSS-variable scaffold, font-pairing guidance
scripts/validate.sh    # CI structure check
```

## Related

A **deterministic public twin** of step 2 runs as a free lead-magnet tool —
[brandscan.netlify.app](https://brandscan.netlify.app) — which extracts any site's colours
and fonts in the browser with no LLM. This plugin is the full, human-in-the-loop system.

## License

MIT © 2026 Victor Shulga / B2B Global
