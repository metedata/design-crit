# Crit UI Design System

> Shared reference for `overview.html`, `compare.html`, and `direction.html`. Follow these
> specs exactly. Two sessions must produce visually identical chrome. The wireframes are
> the hero -- the crit UI recedes. Professional instrument aesthetic, not marketing page.

---

## 1. Color Palette (locked)

```
--surface-base:    #0D0D0F       --text-primary:    #F0F0F2
--surface-raised:  #141416       --text-secondary:  #8B8B92
--surface-overlay: #1A1A1E       --text-muted:      #5C5C63
--surface-input:   #111113       --accent:          #6366F1
--border-subtle:   #1E1E22       --accent-hover:    #818CF8
--border-focus:    #6366F1       --status-success:  #4ADE80
                                 --status-warning:  #FBBF24
                                 --status-error:    #F87171
```

These values are fixed across all sessions and pages. Never use `#FFF` as a background.
No opacity variants -- use the named muted colors. Crit chrome is always dark.

---

## 2. Typography (locked)

```css
--font-ui:   -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
--font-mono: "SF Mono", "Fira Code", "Cascadia Code", "Consolas", monospace;
```

No external fonts. No CDN links. No `@import` or `@font-face`.

| Role       | Size | Weight | Line-height | Where                                    |
|------------|------|--------|-------------|------------------------------------------|
| Page title | 24px | 600    | 1.3         | Page headers                             |
| Section    | 18px | 600    | 1.3         | "My Take", facet group headings          |
| Subheading | 15px | 600    | 1.3         | Option names, banner text                |
| Body       | 13px | 400    | 1.5         | Rationale, descriptions, buttons         |
| Label      | 11px | 500    | 1.3         | Uppercase tags, badges, filters          |
| Mono       | 12px | 400    | 1.5         | Filenames, JSON, technical values        |

Uppercase labels: `letter-spacing: 0.02em; text-transform: uppercase`.

---

## 3. Spacing & Layout

Base unit: 4px. Scale: `4 / 8 / 12 / 16 / 24 / 32 / 48`.

- Card padding: `16px`. Section spacing: `24px`. Page section gaps: `48px`.
- Card border: `1px solid var(--border-subtle)`, `border-radius: 8px`, no box-shadow.
- Max-width for text: `680px`. Page content: `1440px`, centered.
- Buttons: `border-radius: 6px`, `padding: 10px 20px`. Pill buttons: `border-radius: 9999px`, `padding: 6px 16px`.

---

## 4. Context Banner (compare.html)

Fixed top, full width, `48px` tall, `--surface-base`, bottom `1px solid var(--border-subtle)`,
`z-index: 100`, padded `0 24px`.

**Left:** Project name (15px/500/`--text-primary`) | Facet name | Round N -- pipe-separated
in `--text-muted`. **Right:** Segmented filter (All/Surviving/Eliminated) as 11px uppercase
buttons, `--accent` fill on active. Dark mode toggle (visual facets only): small switch,
36x20px, "Light"/"Dark" label in 11px `--text-muted`.

---

## 5. Option Cards (compare.html)

Card: `--surface-raised`, subtle border, `8px` radius, no shadow. `border-radius: 8px 8px 0 0`
on the iframe, inset shadow `inset 0 0 0 1px rgba(255,255,255,0.04)`.

**Iframe sizing is adaptive, not fixed.** The LLM chooses the iframe dimensions based on
the content shape and the layout chosen for this compare view (see `crit-loop.md`):
- Desktop wireframes: landscape aspect, prioritize width (`min-height: 400px`)
- Mobile wireframes: portrait aspect, prioritize height (`min-height: 600px`, narrower width)
- Component-level facets (buttons, spacing, icons): can be compact (`min-height: 300px`)
- Full-page layouts: maximize available space

The goal: the wireframe is visible at a glance without scrolling inside the iframe.
When in doubt, make the iframe bigger.

Below iframe: "Open in new tab" link (11px, `--text-muted`, right-aligned). Option name
(15px/600/`--text-primary`). Three rationale sections with 11px uppercase labels
(`WHY THIS OPTION` / `WORKS WELL FOR` / `WATCH OUT FOR`) in `--text-muted` and 13px body
in `--text-secondary`. Spacing: `8px` label-to-body, `16px` between sections.

**Option label:** A prominent letter label ("A", "B", "C") displayed at the top-left of
each card, 18px/600/`--accent`, so users can reference options easily in the chat as
an alternative to using the Keep/Cut buttons.

**Keep/Cut pills:** Outline by default (`--border-subtle`). Active Keep: filled
`--status-success`, dark text. Active Cut: filled `--status-error`, dark text. Mutually
exclusive toggle.

**Comment textarea:** `--surface-input`, `--border-subtle` border, `6px` radius,
`min-height: 60px`, `resize: vertical`, 13px `--font-ui`, placeholder in `--text-muted`.

**Eliminated cards (round 2+):** `opacity: 0.5`, "CUT" badge (11px uppercase,
`--status-error` border, `4px 8px` padding, top-right of iframe). Hidden when
filter = Surviving.

---

## 6. My Take Section (compare.html)

**Full-width below all option cards.** This is NOT a card in the grid — it spans the
entire content column width, visually separated from the options above.

Container: `--surface-overlay` background, `8px` radius, `3px solid var(--accent)` left
border, `24px 32px` padding, `48px` top margin. `max-width: 100%` of the content area
(not `680px` — it should be as wide as the option grid above it).

Title: 18px/600/`--text-primary`, with a subtle accent-colored label "MY TAKE" in 11px
uppercase above it. Body: 14px (slightly larger than card body text)/`--text-secondary`,
`line-height: 1.6`, `max-width: 720px` for readable line length within the full-width
container.

This section should feel like the designer stepping back and giving their overall read —
prominent, opinionated, and easy to find. It should never be confused with an option card.

---

## 7. Feedback Area (compare.html)

Direction notes textarea: same input styling, `min-height: 100px`, max-width `680px`.

**Button row** (flex, `12px` gap, `16px` top margin):
- **Save Feedback** -- `--accent` fill, white text, no border (primary).
- **Skip This Area** / **Decide For Me** -- transparent, `--text-secondary`,
  `--border-subtle` border (ghost).

Delegate textarea: hidden, slides open (`200ms ease-out`) on "Decide For Me" click.

**Instruction text** above the button row (13px, `--text-muted`): "Click Save Feedback
and Claude will pick it up automatically. Or just tell Claude your thoughts in the chat."

---

## 8. Overview Page (overview.html)

Max-width `960px`, centered, `--surface-base`.

**Header:** Project name (24px/600), brief summary (13px/`--text-secondary`, max 3 lines),
pipeline steps (Brief > Facets > Direction) colored by state: `--status-success` done,
`--accent` active, `--text-muted` pending. Divider below.

**Facet rows:** Clean rows (`12px 0` padding, bottom border), not cards. Status indicator
(left), name + status text (center), action button (right).

| Status         | Indicator                                          |
|----------------|----------------------------------------------------|
| Locked (user)  | Filled circle `--status-success`                   |
| Locked (auto)  | Filled circle `--status-success` + "auto" tag      |
| In-progress    | Pulsing circle `--status-warning`                  |
| Pending        | Empty circle `--border-subtle`                     |
| Skipped        | Dash `--text-muted`                                |

No colored row backgrounds. Review expansion: `<details>/<summary>`, `200ms ease-out`
slide-down, `24px` left indent, shows round history and decision rationale.

---

## 9. Direction Page (direction.html)

Max-width `1080px`, centered. More polished -- this is the deliverable.

**Screen gallery:** CSS grid `repeat(auto-fill, minmax(280px, 1fr))`, `16px` gap. Each
thumbnail: `--surface-raised` card with small iframe preview, option name below (13px/500),
click opens full wireframe.

**Decision sections:** One per facet in pipeline order. Facet name (18px/600), locked
summary (13px/`--text-secondary`), user signal in muted italic.

**Decision log:** `<details>/<summary>` per facet, visually quiet when collapsed. Expanded:
rounds, options, feedback excerpts, elimination rationale.

---

## 10. Interaction & Motion

- Color/opacity transitions: `150ms ease-out`.
- Expand/collapse: `200ms ease-out`.
- Scroll: `html { scroll-behavior: smooth; }`.
- Focus: `outline: 2px solid var(--accent); outline-offset: 2px` on all interactive elements.
- No decorative animation. No hover scale. No bouncing loaders.

---

## 11. Responsive

Desktop-first. The compare view layout is chosen per-facet by the LLM (see
`crit-loop.md` Layout Goals) — there is no single fixed grid for all compare views.

General responsive behavior regardless of chosen layout:
- Below 768px: stack all cards vertically, simplified banner, single column.
- Min touch target: `36px`.
- Iframe min-height should adapt to content shape (portrait mobile frames need
  more height, landscape desktop frames need more width).

---

## 12. Dark Mode Toggle

Affects ONLY wireframe iframes via `postMessage({ type: 'theme', value: 'light' })`.
Crit chrome stays dark always. Present only for visual facets (color, typography, density).

---

## 13. Anti-Patterns

Never: bright white backgrounds, heavy shadows or 3D effects, custom/web fonts, colored
card backgrounds for status, multiple accent colors, palette variation between sessions,
hero sections or decorative illustrations, external CSS frameworks, horizontal scrolling,
attention-grabbing loaders, emoji in chrome.
