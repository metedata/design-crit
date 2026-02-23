---
name: crit-typography
description: Use when evaluating font selection, type scale, typographic hierarchy, and text readability in a design.
---

# Typography Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are critting typography -- font selection, type scale, hierarchy expression through type, and text readability. Typography is how the design speaks. It carries the content hierarchy into visible form, sets the product's tone, and determines whether users can comfortably read and scan every screen. Good typography is invisible; bad typography is the first thing users feel (even if they cannot name it).

Typography sits in the sensory lens. It depends on `content-hierarchy` (emphasis levels that the type scale must express), `content-layout` (column widths that constrain line length), and `component-design` (components that contain text at specific sizes). It feeds into `color-system` (text colors must work with font weights) and `accessibility` (contrast ratios, font size minimums, line spacing requirements).

---

## 1. Load Context

Before generating any options, read:

1. `.design-crit/state.json` -- current facet state, round number, locked decisions
2. `.design-crit/brief.md` -- project purpose, brand tone, platform, tech stack
3. `.design-crit/facets/typography/feedback-round-{N-1}.json` -- prior feedback (skip for round 1)
4. All locked option files from prior facets -- especially `content-hierarchy` (emphasis levels to express), `content-layout` (column widths for line length), `component-design` (component text sizes), and `elevation-shape` (shape language tone that type should complement)

Check the brief for:
- **Existing font choices** -- brand guidelines may specify fonts. Treat as a hard constraint.
- **Tech stack** -- affects font loading strategy (Next.js has built-in font optimization; static sites may prefer system fonts).
- **Platform** -- iOS defaults to SF Pro, Android to Roboto. Web has full freedom but loading cost.
- **Content type** -- long-form reading needs different type treatment than dashboards or marketing pages.

State what you are building on: "Building on the locked hierarchy with [N] emphasis levels. Layout has [column width]. Components define [key text contexts]. Shape language is [sharp/moderate/soft], so typography should complement that tone."

---

## 2. Artifact Type

Generate **type specimens, hierarchy scales, and paragraph samples**.

Each option file (`option-{x}.html`) must contain:

### Type Scale Specimen

Display the complete type scale as a vertical specimen. For each level, show:

- **Scale level name** -- semantic, not numeric. Use: Display, Heading 1, Heading 2, Heading 3, Body Large, Body, Body Small, Caption, Overline (or a subset appropriate to the project).
- **Rendered sample text** -- a short, meaningful phrase (not "Lorem ipsum"). Use content relevant to the project: "Dashboard Overview" for a heading, "Last updated 3 minutes ago" for a caption.
- **Specifications** -- font family, font weight, font size (px and rem), line height (unitless ratio), letter spacing (if non-default).
- **Scale ratio** -- the multiplier between each level.

Render the specimen using the actual fonts specified. For web-safe or system fonts, use a font stack that renders correctly in the browser. For custom fonts, use Google Fonts or note that the font must be loaded.

### Hierarchy in Context

Show the type scale applied to an actual screen wireframe from the locked layout. Map each text element to its type scale level:

- Page title -> Heading 1
- Section heading -> Heading 2
- Card title -> Heading 3
- Body text -> Body
- Metadata -> Caption

This demonstrates that the type scale actually works within the layout constraints. Annotate each element with its scale level and specs.

### Paragraph Sample

Render a block of body text at the specified size, weight, line height, and column width. Show at least 4-5 lines of realistic content. This tests readability -- the most critical job of typography.

Include:

- **Line length indicator** -- show the character count per line. Ideal range: 45-75 characters. Flag if outside this range.
- **Line height visualization** -- subtle guides showing the spacing between baselines.
- **Paragraph spacing** -- the gap between paragraphs.

### Weight and Style Inventory

Show all the weights and styles used in the type system:

- Regular (400) -- body text, descriptions
- Medium (500) -- emphasis within body, secondary headings (if used)
- Semibold (600) -- subheadings, labels (if used)
- Bold (700) -- primary headings, key data

Not every project needs every weight. Show only the weights the type scale actually uses. Fewer weights = faster load, simpler system.

### Font Pairing (if applicable)

If the type system uses more than one font family (e.g., a sans-serif for UI and a serif for long-form content), show them together:

- A heading in the heading font above body text in the body font
- A UI context (button, label, input) in the UI font alongside content
- An explanation of when each font is used and why

---

## 3. Evaluation Criteria

Score each option against these questions. State your assessment in the critique.

### Hierarchy Clarity at a Glance

Can you instantly distinguish each level of the type scale? Check:

- Is there a clear visual jump between each heading level?
- Can you tell a Heading 2 from a Heading 3 without reading the label?
- Does the hierarchy rely on more than just size? (Weight, spacing, and color also create distinction.)
- Are there "dead zones" where two adjacent levels look too similar?

If the hierarchy is unclear at a glance, it will fail in practice. Users do not read labels -- they read visual weight.

### Body Text Readability

Is the body text comfortable to read for extended periods?

- **Font size:** 16px minimum for body text on web. 14px minimum for secondary body text. Below 14px only for captions and metadata.
- **Line height:** 1.4-1.6 for body text. Tighter (1.2-1.3) for headings. Looser (1.6-1.8) for large text or text-heavy interfaces.
- **Line length:** 45-75 characters per line is the comfortable range. Below 45 feels choppy. Above 75 causes eye-tracking fatigue.
- **Letter spacing:** Default for body text. Slightly increased (0.01-0.02em) for all-caps text, small text, and wide headings. Slightly decreased for large display text.
- **Font choice:** Does the body font have good x-height, open apertures, and clear letterforms at the target size?

### Type Scale Economy

Does the scale have enough levels for the hierarchy without excess?

- **Too few levels (3-4):** Cannot express the locked content hierarchy. Headings and body lack distinction. Metadata and captions merge.
- **Right-sized (5-7 levels):** Covers most projects. Display/H1/H2/H3/Body/Small/Caption.
- **Too many levels (8+):** Adjacent levels become indistinguishable. Developers cannot remember which to use. Fragmentation across the product.

Match the type scale level count to the locked content hierarchy level count. If hierarchy defined 4 emphasis levels, the type scale needs at least 4 clearly distinct levels.

### Scale Ratio Appropriateness

Is the ratio between levels well-chosen?

| Ratio | Name | Character |
|---|---|---|
| 1.125 | Major Second | Tight. For dense UIs with many levels in a small range. |
| 1.200 | Minor Third | Moderate. Good general-purpose ratio for applications. |
| 1.250 | Major Third | Common default. Clear but not dramatic jumps. |
| 1.333 | Perfect Fourth | Generous. Good for content-heavy sites, editorial design. |
| 1.500 | Perfect Fifth | Dramatic. Good for marketing sites, hero-heavy designs. |
| 1.618 | Golden Ratio | Very dramatic. Only for designs with few levels and strong contrast. |

Dense UIs (dashboards, admin tools) benefit from tighter ratios (1.125-1.200). Content-rich UIs (blogs, documentation) benefit from wider ratios (1.250-1.333). Marketing sites benefit from dramatic ratios (1.333-1.618).

---

## 4. Font Selection Criteria

Reference these criteria when generating and evaluating font options.

### Readability at Target Sizes

The font must be legible at the smallest size it will be used. Test:

- Does the font have a tall x-height? (Taller x-height = more legible at small sizes.)
- Are the apertures open? (Open counters in a, e, c, s improve legibility.)
- Are similar letterforms distinguishable? (I/l/1, O/0, rn/m.)
- Does the font render well on screen at the target sizes? (Some fonts designed for print break down at 14-16px on screen.)

### Brand Alignment

The font communicates tone before the user reads a single word:

- **Geometric sans-serif** (Inter, Circular, Futura): Modern, clean, tech-forward.
- **Humanist sans-serif** (Source Sans, Fira Sans, Open Sans): Approachable, readable, warm.
- **Neo-grotesque** (Helvetica, Arial, Roboto): Neutral, professional, ubiquitous.
- **Serif** (Georgia, Merriweather, Playfair): Editorial, authoritative, traditional.
- **Monospace** (JetBrains Mono, Fira Code, SF Mono): Technical, precise, developer-oriented.
- **Slab serif** (Roboto Slab, Zilla Slab): Bold, industrial, statement-making.

Match the font personality to the brief's brand tone and audience.

### Variable Font Support

Variable fonts allow smooth interpolation between weights (and sometimes width, slant, optical size). Benefits:

- Single file covers all weights -- faster loading
- Precise weight tuning for emphasis (e.g., 450 instead of jumping from 400 to 500)
- Animation and responsive weight adjustment
- Growing browser support

Prefer variable fonts when the tech stack supports them (modern web, iOS 15+, Android).

### License and Loading

- **System font stacks:** Zero loading cost, renders instantly. Limited design control. Best for performance-critical applications and MVPs.
- **Google Fonts:** Free, easy to load, CDN-cached. Good for most web projects.
- **Commercial fonts:** Require license. Premium quality and uniqueness. Check license terms for web embedding.
- **Self-hosted:** Full control over loading. Best performance if combined with `font-display: swap` and preloading.

For each option, note the loading strategy and its performance implication.

---

## 5. Common Typography Trade-offs

Present these trade-offs when they apply.

### System Fonts vs Custom Fonts

- **System fonts:** Instant rendering, no FOUT (flash of unstyled text), zero load cost, familiar to platform users. Risk: limited design expression, different appearance across platforms.
- **Custom fonts:** Brand-aligned, distinctive, consistent cross-platform. Risk: loading delay, FOUT/FOIT, additional HTTP requests, license cost.
- **Question to ask:** "Is typographic personality important to your brand, or is instant rendering more important?"

### Tight Scale vs Wide Scale

- **Tight scale (1.125-1.200):** Many levels fit in a small range. Dense interfaces, dashboards, data-heavy tools. Risk: adjacent levels blend together.
- **Wide scale (1.333-1.618):** Dramatic hierarchy, editorial feel, strong visual rhythm. Risk: large headings consume space, fewer practical levels.
- **Question to ask:** "How many distinct text levels do your screens need, and how much vertical space can headings claim?"

### Single Family vs Font Pairing

- **Single family:** One font for everything. Consistent, simple, fewer loading resources. Most applications work well with one good sans-serif.
- **Font pairing:** Heading font + body font (or UI font + content font). More expressive, editorial feel, can optimize readability for different contexts. Risk: pairs can clash, doubles the loading cost.
- **Question to ask:** "Does your product have distinct content modes (reading vs doing) that benefit from different type treatments?"

### Fewer Weights vs More Weights

- **2-3 weights (Regular, Semibold, Bold):** Simpler system, faster loading, fewer decisions. Sufficient for most applications.
- **4-5 weights (Regular, Medium, Semibold, Bold, Heavy):** More nuanced hierarchy expression. Useful when hierarchy has many levels and size alone is not enough.
- **Question to ask:** "Can you express your hierarchy through size and spacing alone, or do you need weight to create additional distinction?"

---

## 6. Option Generation Guidance

### Round 1: Diverge

Generate 2-4 options exploring different typographic directions. Each option must present a complete type system (font selection, scale, hierarchy mapping), not just different fonts.

Useful axes of variation:

- **Font personality:** One option uses a geometric sans-serif (modern, clean). Another uses a humanist sans-serif (warm, readable). Another uses a serif for headings with sans body (editorial). Another uses system fonts (performance-first).
- **Scale character:** One option uses a tight ratio (1.125) for a dense dashboard feel. Another uses a generous ratio (1.333) for editorial readability.
- **Weight strategy:** One option relies on 2 weights with size doing the heavy lifting. Another uses 4 weights for nuanced hierarchy expression.
- **Pairing vs single:** One option uses one font family throughout. Another pairs a display font for headings with a reading font for body.

Name each option by its typographic identity: "Inter Clean," "Source Editorial," "System Speed," "Playfair & Fira."

### Round 2: Converge

Refine based on feedback. Typical refinements:

- Adjust the scale ratio up or down based on whether levels felt too similar or too dramatic
- Swap one font for a similar alternative that addresses a specific concern (readability, weight availability, loading speed)
- Merge the font selection from one option with the scale structure of another
- Add or remove a scale level to match hierarchy needs

### Round 3+: Polish

Focus on: precise token values for implementation (exact px/rem, line-height, letter-spacing per level), paragraph sample tuning (line length verification against locked layout columns), weight inventory minimization, and ensuring the type system works across all primary screens.

---

## 7. Per-Option Critique Checklist

When writing the critique for each option in `critique.md`, address every item:

1. How many type scale levels are defined? Does this match the locked content hierarchy?
2. Can you distinguish every level at a glance without reading the label?
3. Is the body text readable at the target column width? (Check line length: 45-75 characters.)
4. What is the scale ratio and is it appropriate for the project type?
5. Does the font selection match the brand tone from the brief?
6. How many fonts and weights are loaded? What is the performance cost?
7. Does the type system complement the locked shape language (sharp type with sharp corners, soft type with soft corners, or intentional contrast)?
8. Are there any readability concerns at the smallest text size used?

---

## 8. Constraint Propagation

Typography decisions constrain downstream facets:

- **Color system** must provide text colors that meet contrast requirements at every font size and weight. Lighter text colors only work at larger/bolder sizes.
- **Density-spacing** inherits vertical rhythm from line height and paragraph spacing. The spacing scale should relate to the type scale's line height.
- **Accessibility** will audit font size minimums (16px body minimum), contrast ratios for text at every size/weight, line height adequacy, and whether the type system supports user font scaling.
- **Content voice** will write microcopy within the constraints of the type system -- character counts, line breaks, and how text fits in components.

State these downstream implications in the critique: "This type system uses [font] at [body size] with [N] scale levels. The color system will need text colors with [contrast ratio] contrast at [smallest size]. Spacing should align with the [line-height] baseline."

---

## 9. Dark Mode Considerations

Typography behaves differently in dark mode. Include these notes in the compare view when the design will support dark mode:

- **Weight perception:** Fonts appear heavier on dark backgrounds. Consider reducing font weight by one step in dark mode (e.g., Regular 400 -> 350 if the font supports it, or use a lighter optical variant).
- **Contrast calibration:** Pure white (#FFFFFF) on dark backgrounds creates glare. Reduce body text to off-white (e.g., #E0E0E0 or similar) for comfortable reading. Headings can be brighter.
- **Letter spacing:** Text on dark backgrounds can benefit from slightly increased letter spacing (0.01-0.02em) for readability.

These are notes for the color system facet. Do not define colors here -- flag the typographic implications.

---

## 10. Locking

When the user locks a typography option:

1. Record the locked option in `state.json` with `locked_summary` describing the type system (e.g., "Inter variable font, 6-level scale at 1.25 ratio from 12px caption to 36px display, 3 weights, system font fallback stack").
2. The locked type specimen becomes the typographic reference for all downstream facets. Extract token values for `design-tokens.json` when the design direction is synthesized.
3. Announce the lock and its downstream implications: "Typography locked as [name]. Color system will define text colors for these sizes and weights. Density will build on this vertical rhythm. Accessibility will audit contrast and size minimums."
