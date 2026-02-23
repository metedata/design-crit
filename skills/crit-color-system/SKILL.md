---
name: crit-color-system
description: Use when evaluating color palettes, theming, dark mode, contrast, and color tokens for a design system.
---

# Color System Crit

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing the color system for this project. Color is a sensory facet --
it applies to surfaces and content structures that are already locked. Your job: generate
palette options, evaluate them against brand intent and accessibility requirements, and guide
the user to a locked color direction.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (brand tone, platform, audience)
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Color builds on top of all prior structure.
Pay special attention to:

- **Layout** -- surface areas, content regions, sidebar/header zones
- **Typography** -- font weights and sizes inform text color contrast needs
- **Component design** -- buttons, cards, inputs need distinct color states
- **Elevation/shape** -- shadow colors and surface stacking depend on palette

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/color-system/`. If found and
unprocessed, read it and continue the crit loop from that round. Do not regenerate round 1.

---

## 2. Color System Structure

Every palette option must define these five color groups. Present them as organized swatches
in the option HTML.

### Brand Colors

The primary and secondary colors that carry the product's identity. Derived from the brief's
brand tone and differentiator. Typically 1-2 primary hues and 1-2 accent hues.

- Primary: the dominant action and emphasis color
- Secondary: complementary or supporting accent
- Tertiary (optional): used sparingly for highlights or illustrations

### Surface Colors

Background layers that establish depth and hierarchy. Define at minimum:

- `surface-base` -- the page background
- `surface-raised` -- cards, panels, dialogs (one step above base)
- `surface-sunken` -- inset areas, grouped content backgrounds
- `surface-overlay` -- modals, popovers, dropdown menus

### Text Colors

Foreground colors for content. Define at minimum:

- `text-primary` -- body text, headings
- `text-secondary` -- supporting text, labels, metadata
- `text-tertiary` -- placeholder text, disabled states
- `text-inverse` -- text on dark/colored backgrounds
- `text-link` -- hyperlinks, interactive text

### Status Colors

Semantic colors for system feedback. Each needs a background, foreground, and border variant:

- `success` -- confirmations, completed states (green family)
- `warning` -- cautions, pending states (amber/orange family)
- `error` -- failures, destructive actions (red family)
- `info` -- neutral notifications, tips (blue family)

### Interactive State Colors

Colors for interactive element states:

- `hover` -- subtle background shift or overlay
- `active` / `pressed` -- darker or more saturated shift
- `focus` -- visible focus ring color (often brand primary or high-contrast blue)
- `disabled` -- muted, reduced contrast (but still legible)
- `selected` -- active selection indicator

---

## 3. Contrast Requirements

Apply WCAG AA as the minimum standard. Check every text/background combination.

| Element | Minimum Ratio | Standard |
|---|---|---|
| Body text (< 18px) on any surface | 4.5:1 | WCAG AA |
| Large text (>= 18px or 14px bold) on any surface | 3:1 | WCAG AA |
| UI components and graphical objects | 3:1 | WCAG AA |
| Focus indicators | 3:1 against adjacent colors | WCAG AA |

### How to Verify in Option HTML

For each option file, include a contrast audit section at the bottom. Render a table showing:

- Color pair (e.g., `text-primary` on `surface-base`)
- Hex values
- Computed contrast ratio
- Pass/fail badge

Calculate contrast ratios using the WCAG relative luminance formula. Embed the calculation
in inline JavaScript so the audit is live and self-contained.

### Color-Blindness Awareness

Note in the critique when status colors rely solely on hue. Suggest pattern or icon
reinforcement for red/green distinctions. Options that depend on red vs green differentiation
alone are a trade-off worth flagging, not an automatic failure.

---

## 4. Dark Mode

If the brief specifies dark mode, or the platform convention expects it, generate both light
and dark variants for EVERY option. Dark mode is not inverted light mode. It is a separate
design decision.

### Dark Mode Principles

- **Surface scale inverts in direction, not value.** Light mode: white base, gray raised.
  Dark mode: dark gray base, slightly lighter raised. Do not use pure black (#000) as base --
  use a dark gray (#121212 to #1a1a1a range).
- **Reduce saturation on chromatic colors.** Saturated blues and reds that work on white
  become vibrating on dark backgrounds. Desaturate by 10-20%.
- **Preserve contrast ratios.** Every pair that passes in light mode must also pass in dark.
  This often means text colors shift from dark-on-light to light-on-dark, not simply invert.
- **Elevate with lightness, not shadow.** Shadows are invisible on dark backgrounds. Use
  slightly lighter surface colors to indicate elevation instead.
- **Status colors need adjustment.** Success green on dark gray has different contrast than
  on white. Recalculate.

### Option HTML for Dark Mode

Include a toggle in each option file that switches between light and dark. Use CSS custom
properties with a `[data-theme="dark"]` attribute on `<html>`. Define all colors as custom
properties so the toggle swaps the full palette.

The compare view's dark mode toggle (from crit-loop.md) sends a postMessage to each iframe.
Each option file must listen for this message and toggle accordingly.

---

## 5. Token Naming

Use semantic names, not literal color names. Tokens describe purpose, not appearance.

### Naming Convention

```
{category}-{element}-{variant}
```

Examples:
- `surface-base`, `surface-raised`, `surface-overlay`
- `text-primary`, `text-secondary`, `text-inverse`
- `status-error-bg`, `status-error-text`, `status-error-border`
- `interactive-hover`, `interactive-focus`, `interactive-disabled`
- `brand-primary`, `brand-secondary`

### Why Semantic Over Literal

- `blue-500` tells you nothing about where it goes. `brand-primary` does.
- When the brand color changes from blue to purple, semantic tokens require zero UI changes.
- Literal naming breaks down when dark mode uses a lighter blue for the same role.

In the option HTML, define all colors as CSS custom properties using semantic names. Do not
use hex values directly in component styles. This structure feeds directly into
`design-tokens.json` at the direction stage.

---

## 6. Emotional Tone and Palette Strategy

Each option should explore a different emotional direction tied to the brief.

### Tone Axes

Position each option on these spectrums and state where it sits in the rationale:

- **Warm vs Cool** -- warm (reds, oranges, yellows) for energy and friendliness; cool
  (blues, greens, purples) for professionalism and calm
- **Saturated vs Muted** -- saturated for boldness and energy; muted for sophistication
  and subtlety
- **Monochromatic vs Complementary** -- mono for cohesion and simplicity; complementary
  for contrast and dynamism
- **High contrast vs Low contrast** -- high for clarity and accessibility; low for
  softness and visual comfort

### Brief-Driven Direction

Read the differentiator and target users from the brief. A developer tool for daily use
calls for a muted, cool palette with low visual fatigue. A consumer fitness app calls for
saturated, warm energy. A financial dashboard needs authoritative neutrals with precise
status colors. Tie every option to the brief, not abstract color theory.

---

## 7. Generating Options

### Round 1: Diverge

Generate 2-4 palette options. Each must make a different bet about emotional tone, palette
strategy, or brand expression. Maximize variety across the tone axes.

Example option set for a developer productivity tool:

- **Soft Indigo** -- cool, muted, monochromatic. Low fatigue, professional. Blue-gray
  surfaces with indigo accents.
- **Warm Carbon** -- warm neutrals with amber accents. Approachable but serious. Charcoal
  surfaces, warm gray text.
- **Vivid Spectrum** -- high saturation, complementary. Bold brand presence. Deep blue
  primary, coral accent, bright status colors.

Each option HTML must include:
1. Organized color swatches (all five groups labeled)
2. Applied mockup: the locked layout wireframe with this palette applied to surfaces,
   text, buttons, and status indicators
3. Light and dark variants (if applicable) with toggle
4. Contrast audit table

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:
- Adjust contrast ratios that failed
- Shift warmth or saturation in response to tone feedback
- Merge the surface scale from one option with the accent palette of another
- Add or refine dark mode

When merging two options, create a new option file (e.g., `option-ab.html`) and state
what was taken from each in the rationale.

---

## 8. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite which
color pairs pass or fail, which tone axes the option targets.

### Accessibility
- Do ALL text/background combinations pass WCAG AA? List any failures by name.
- Do UI components (buttons, inputs, icons) meet 3:1 against adjacent colors?
- Does the palette work under simulated color blindness (protanopia, deuteranopia)?

### Brand Alignment
- Does the palette support the brief's stated or implied brand tone?
- Is there a clear primary action color that feels intentional, not default?
- Do the surface colors feel like THIS product, not a generic template?

### Systematic Completeness
- Are all five color groups defined (brand, surface, text, status, interactive)?
- Are tokens named semantically?
- Does dark mode (if present) feel intentional, not auto-generated?

### Practical Viability
- Can this palette scale to new components without needing new colors?
- Are there enough surface layers for the layout complexity?
- Do status colors remain distinguishable from brand colors?

---

## 9. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** in the context banner. This is a visual styling facet.
- Each option card's iframe must show the applied mockup (not just swatches). The user
  needs to see colors in context, not isolated squares.
- Below each iframe, include a "Swatch Reference" expandable section that shows all defined
  color tokens with hex values, organized by group.
- The comparative take ("My Take") must address: which option has the strongest brand
  presence, which is safest for accessibility, and which has the best dark mode (if
  applicable). State your recommendation.

---

## 10. Locking and Token Extraction

When the user locks a color system option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: primary brand color hex, surface strategy (warm/cool),
   dark mode status (yes/no), and contrast compliance (AA pass/fail count).
3. The locked option file becomes the color reference for all downstream facets
   (density-spacing, content-voice, accessibility).
4. Color tokens from the locked file feed into `design-tokens.json` at the direction stage.
   Ensure all CSS custom properties are parseable.

---

## 11. Common Patterns and Trade-offs

Use these to inform option generation and critique. Do not recite them to the user -- apply
them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| Monochromatic with one accent | Clean, cohesive, easy to maintain | Can feel monotonous; status colors may clash |
| Split-complementary | Strong visual interest, good contrast | Harder to balance; can feel busy |
| Neutral base + saturated accents | Professional, scalable, safe | Risk of feeling generic or corporate |
| Dark-mode-first design | Developer tools, media apps | Light mode may feel like an afterthought |
| High saturation throughout | Energetic, youthful, distinctive | Visual fatigue on daily-use tools; contrast issues |

---

## 12. Constraints

- Never use color as the SOLE means of conveying information. Always pair with text, icons,
  or patterns.
- Never use pure black (#000000) text on pure white (#FFFFFF) backgrounds in options
  presented as "comfortable" -- the contrast (21:1) causes halation. Use off-black (#1a1a1a
  to #2d2d2d) on white, or white on off-black.
- Always define CSS custom properties, never hard-coded hex in component styles.
- The compare view MUST include the dark mode toggle for this facet.
- Every option must include at minimum: brand, surface, text, and status color groups.
  Interactive states may be simplified in early rounds.
