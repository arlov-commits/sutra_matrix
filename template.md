# Sutra Matrix — Master Template Principles

> Last updated: 2026-04-27
> Apply these to every sutra page. The framework is **identical**; only the per-sutra theming and content differ.

---

## File layout per sutra

```
sutras/<sutra-id>/
  inputs/                  # raw sources (pdf scans, txt, theme images, chapter title lists)
  outline.json             # parsed nested outline (single source of truth)
  <Name> Sutra Matrix.html # the rendered page
```

Where `<sutra-id>` is kebab-case: `lotus`, `earth-store`, `ten-grounds`.

The hub at project root is **`Sutra Matrix.html`** — a tile grid linking to each sutra's matrix HTML.

---

## Outline JSON shape

Each node:
```js
{
  d: <integer depth, 1 = top>,
  kind: 'item' | 'chapter' | 'other',
  letter: 'A' | 'B' | ... | null,        // outline letter prefix
  label: 'A1' | 'C3' | 'CHAPTER ONE',    // display label
  body: '<text content>',                // the title/content
  page: <int> | null,                    // page reference if known
  chapter: <int> | null,                 // chapter # if this node IS a chapter marker
  children: [ ...nodes ],
}
```

Different sutras may use different nesting conventions (Lotus = letter-based; Ten Grounds = roman-numeral / letter / decimal-numbered). The parser per sutra is custom; the **output JSON is uniform**.

---

## Page anatomy (top → bottom)

1. **Breadcrumb nav** — `◂ Sutra Matrix / <Sutra Name>`. Click left link to return to hub. No second crest. No "Mahayana · Outline System" tagline.
2. **Compact title block** — sutra name in English (large serif), with Sanskrit/Chinese stylized variant inline or beneath. Stats (chapters · sections · max depth) inline-right or below. **No multi-line subtitle. No exegesis blurb.**
3. **One-line control bar** — depth dial + `Pages` / `Sigils` toggles + `Collapse all` / `Expose all` quick actions.
4. **Tree** — the matrix.
5. **Footer** — build stamp + source hash. Hover for source filename.

---

## Floating UI (always-on, fixed-position)

- **Bottom-LEFT:** ↑ "Jump to top" — appears after scrolling >300px.
- **Bottom-RIGHT:** `A−` / `A+` font-size stepper. Modifies a CSS variable `--text-scale` on `:root`. Persisted in `localStorage` per page.

Both are unobtrusive: small pill buttons in the page's accent color.

---

## Removed (do NOT include)

- ❌ Secondary crest header (`Sutra Matrix · Mahā­yāna · Outline System`)
- ❌ Multi-line subtitle/blurb
- ❌ Sigil legend (the `A L2 / B L2 / C L3 ...` row beneath the control panel)
- ❌ Translator/dynasty subtitle in nav
- ❌ Redundant "Compiled · N chapters · M sections" footer line (stats are already in hero)
- ❌ **Page numbers** — no page-ref UI, no `Pages` toggle, no `.page-ref` cells. Page data may live in `outline.json` for archival reasons but is **never rendered**. The control bar has only `Sigils` toggle + `Collapse` / `Expose` quick actions.

---

## Per-sutra theming (color tokens)

Each sutra defines its own palette via `:root` CSS vars but uses the **same token names**:

```css
--bg-deep, --bg-mid, --bg-elev, --bg-card, --bg-card-hover
--line, --line-strong, --line-accent
--accent, --accent-bright, --accent-deep, --accent-glow
--secondary, --secondary-bright, --secondary-deep
--ink, --ink-soft, --ink-dim, --ink-faint
```

Per-sutra colors so far:

| Sutra | bg-deep | accent (gold-equivalent) | secondary |
|---|---|---|---|
| Lotus | `#0b0d14` (psionic night) | `#d4af6e` (gold) | `#7fdbde` (psionic teal) |
| Earth Store | `#1a0f12` (cocoa) | `#e8b860` (amber) | `#5ea88a` (jade) |
| Ten Grounds | TBD — proposing celadon/sapphire ground-tones |

---

## Build/sync protocol

- User edits files in `sutras/<id>/inputs/` (and may upload new files via composer)
- User says **"sync"**
- Claude:
  1. Re-parses each sutra's inputs → updates its `outline.json`
  2. Injects fresh JSON into the matrix HTML's `<script id="sutra-data">`
  3. Updates footer build stamp + 4-byte SHA-256 prefix of source files
  4. Updates hub stats
  5. Reports diff

The footer stamp is the user's at-a-glance staleness indicator: hash matches current source = fresh.
