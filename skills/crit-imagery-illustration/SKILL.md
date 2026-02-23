---
name: crit-imagery-illustration
description: Use when evaluating photography style, illustration systems, hero visuals, and decorative imagery for a product or marketing site.
---

# Imagery & Illustration Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing the imagery and illustration direction for this product. Imagery
is a sensory facet -- it layers onto surfaces, layouts, and components that are already locked.
Photography style, illustration systems, hero visuals, avatar treatments, empty state art,
and decorative elements all fall here. Imagery carries brand personality more viscerally than
any other facet. A single hero image communicates more tone in one second than a full page of
microcopy.

Your job: propose 2-3 imagery directions with concrete visual specimens, evaluate whether
imagery reinforces the brand or just decorates, and guide the user to a locked imagery strategy.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (brand tone, target audience, differentiator)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Imagery applies to many surfaces. Pay special
attention to:

- **Screen inventory** -- which screens exist and where imagery appears
- **Layout** -- content areas, hero zones, sidebar widths constrain image placement
- **Color system** -- palette must harmonize with imagery. Image color grading should not
  fight the locked palette.
- **Typography** -- type over images needs contrast handling. The locked type scale determines
  text overlay sizing.
- **Edge states** -- empty states may use illustrations. Loading states may use placeholder imagery.
- **Content voice** -- illustration tone should match the locked voice (playful illustrations
  with a serious voice creates dissonance)
- **Component design** -- cards, avatars, and hero sections define where images live

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/imagery-illustration/`. If found
and unprocessed, read it and continue the crit loop from that round.

---

## 2. Imagery Audit

Before generating options, catalog where imagery appears in the product by reading locked
facets.

### Imagery Placement Map

Build a table of every surface where imagery appears:

| Screen / Component | Imagery Type | Size / Aspect | Purpose |
|---|---|---|---|
| Hero section | Photography or illustration | Full-width, 16:9 or 3:1 | Brand statement, emotional hook |
| User avatar | Photography | Square, 32-64px | Identity, personalization |
| Card thumbnail | Photography or illustration | 4:3 or 1:1, 200-400px | Content preview |
| Empty state | Illustration | Centered, 200-300px | Guide user, reduce blankness |
| Background | Pattern or photography | Full-bleed | Atmosphere, depth |
| Feature illustration | Illustration | Inline, 100-300px | Explain concept visually |
| Onboarding | Illustration or photography | Variable | Teach and welcome |
| Error state | Illustration | Small, 100-200px | Soften negative moment |

Not every placement applies to every product. Catalog only what this product actually needs
based on locked screens, components, and edge states.

### Stock vs Custom Decision

For each placement, note whether stock imagery or custom illustration is appropriate:

- **Stock photography:** Fastest, cheapest, but generic. Works for backgrounds, hero mood
  shots, blog thumbnails. Risk: looks like every other product.
- **Custom photography:** Unique, brand-specific, but expensive to produce. Worth it for hero
  images on marketing sites, product shots, team photos.
- **Stock illustration:** Libraries like unDraw, Humaaans, Blush. Consistent style, customizable
  color. Works for empty states, feature explainers, onboarding.
- **Custom illustration:** Maximum brand expression. Distinctive character design, unique
  metaphors. Worth it for consumer brands, products with a strong personality.
- **AI-generated imagery:** Emerging option. Can produce unique visuals at scale. Risk: uncanny
  valley, inconsistency across generations, ethical considerations, brand authenticity questions.
- **No imagery:** Some products are better without decorative imagery. Data dashboards, developer
  tools, and dense productivity apps often benefit from letting the data be the visual.

---

## 3. Imagery Style Dimensions

Each option must declare its position on these style dimensions. These positions drive every
visual decision in the option.

### Photography Style (if applicable)

**Mood:** Warm and human / cool and minimal / dramatic and high-contrast / natural and
documentary / editorial and curated.

**Color grading:** Saturated and vivid / desaturated and muted / warm-shifted / cool-shifted /
high contrast black-and-white / brand-tinted overlay.

**Subject matter:** People-centric (faces, hands, activities) / object-centric (products,
tools, workspaces) / abstract (textures, patterns, close-ups) / environmental (spaces,
landscapes, architecture).

**Composition:** Lots of negative space for text overlay / subject-centered / dynamic angles /
flat lay / environmental context.

### Illustration Style (if applicable)

**Line treatment:** No outline (filled shapes only) / thin line / thick line / hand-drawn /
geometric / organic.

**Color approach:** Flat color / gradient / monochrome / duotone / full palette / limited
palette from locked color system.

**Complexity:** Minimal/abstract (simple shapes, few details) / moderate (recognizable objects,
some detail) / detailed (rich scenes, many elements) / spot illustrations (tiny inline icons).

**Character design (if applicable):** Realistic proportions / stylized proportions / no
characters (objects only) / abstract human forms.

**Consistency rules:** Stroke width, corner radius, color palette, level of detail must remain
constant across all illustrations in the system.

---

## 4. Artifact Type

Generate **imagery direction boards** with concrete visual specimens.

Each option file (`option-{x}.html`) must contain:

### Style Board

A visual overview of the imagery direction showing:

- **Mood reference strip** -- 3-5 CSS-rendered specimens that demonstrate the color grading,
  composition style, and mood using gradients, shapes, color blocks, and typographic overlays.
  Since you cannot use external images, render abstract representations using CSS that capture
  the aesthetic (e.g., warm gradients with organic shapes for "warm and human" photography,
  geometric blocks with sharp angles for "minimal and architectural").
- **Color grading specimens** -- show how the locked color palette interacts with imagery.
  Render sample cards, hero sections, and avatar containers using the locked colors with
  CSS patterns that represent the grading style.

### Application Specimens

Show imagery applied to actual product surfaces from the locked layout:

- **Hero section** -- render a hero area with the imagery approach (CSS gradient/pattern
  representing the photo or illustration style, text overlay with locked typography, CTA
  from locked components).
- **Card with thumbnail** -- a content card using the imagery treatment for its visual element.
- **Avatar display** -- user avatar with the chosen treatment (round/square, border, fallback
  for missing image).
- **Empty state** -- one empty state from the locked edge states, with the illustration
  approach applied.
- **Feature illustration** -- an inline illustration explaining a concept, rendered in the
  style direction.

### Specification Table

A reference table documenting the imagery system:

| Element | Treatment | Aspect Ratio | Sizing | Color Handling | Fallback |
|---|---|---|---|---|---|
| Hero image | [style] | 16:9 | Full-width | [grading] | Gradient fallback |
| Card thumbnail | [style] | 4:3 | 200px wide | [grading] | Placeholder icon |
| Avatar | [style] | 1:1 | 32/48/64px | None | Initials on color |
| Empty state | [style] | Free | 200px max | [palette] | Text-only |

### Imagery Guidelines

A short written guide (5-8 rules) for maintaining consistency. Examples:
- "All photography uses warm color grading with lifted shadows."
- "Illustrations use a 2px stroke weight and max 4 colors from the palette."
- "Hero images always leave 40% negative space on the left for text overlay."
- "Avatars use a 2px border in the primary color with 50% rounded corners."

---

## 5. Evaluation Criteria

Score each option against these questions in your critique.

### Brand Reinforcement

- Does the imagery make the product feel like the brand described in the brief?
- Could you swap this imagery into a competitor's product without noticing? (If yes, it is
  not distinctive enough.)
- Does the imagery direction support or undermine the locked voice and color system?

### Functional Clarity

- Do illustrations add clarity or just decoration? Every illustration should help the user
  understand something. Decorative-only imagery is a cost with no benefit.
- Do empty state illustrations guide the user toward action, or just fill space?
- Can hero imagery coexist with text overlays? Is there adequate contrast for readability?

### Style Consistency

- Could a new designer add an illustration to this system and match the existing style?
- Are the style rules specific enough to enforce consistency? "Friendly illustrations" is not
  a system. "2px stroke, rounded caps, 4-color limited palette from brand secondaries, no
  gradients, max 8 elements per scene" is a system.
- Does the illustration style scale from tiny inline spots (16px) to large hero scenes (600px)
  without breaking?

### Performance Implications

- How many images does a typical page load? What is the byte cost?
- Can illustrations be SVG (small, scalable, animatable) or must they be raster?
- Are avatar images optimized for small sizes? (A 64px avatar does not need a 2000px source.)
- Is there a progressive loading strategy? (Blur-up, LQIP, skeleton placeholders.)

### Accessibility

- Do illustrations have meaningful alt text, or are they decorative? (Decorative images get
  `alt=""`. Informational illustrations need descriptive alt text.)
- Does photography rely on color alone to communicate meaning?
- Are there sufficient contrast ratios for any text overlaid on imagery?

---

## 6. Option Generation Strategy

### Round 1: Diverge

Generate 2-3 imagery options that make different bets about how visual the product should be.

**Axes of variation:**

- **Rich vs Restrained** -- one option uses imagery extensively (hero photos, card thumbnails,
  illustrated empty states). Another uses imagery sparingly (text-driven, minimal decoration,
  imagery only where functionally necessary).
- **Photography vs Illustration** -- one option uses photography for hero and editorial
  content. Another uses a consistent illustration system throughout. A third may blend both
  with clear rules for when each is used.
- **Custom vs Stock** -- one option proposes a custom illustration system (unique, branded,
  higher production cost). Another uses a curated stock library approach (faster, cheaper,
  less distinctive).
- **Abstract vs Representational** -- one option uses abstract patterns, gradients, and
  geometric shapes. Another uses concrete images of people, places, and objects.

### Naming Options

Name each option by its visual identity:

- "Editorial Photography" (warm people photography, editorial composition)
- "Geometric Illustration" (flat, geometric illustration system)
- "Minimal Data-First" (no decorative imagery, let the content speak)
- "Warm Hand-Drawn" (organic line illustrations, personal feel)
- "Bold Duotone" (photography with brand-color duotone overlay)
- "Curated Stock" (high-quality stock with consistent grading)

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Adjust color grading to better match the locked palette
- Mix approaches: use one option's hero treatment with another's illustration system
- Increase or decrease imagery density based on performance or aesthetic feedback
- Refine illustration style rules based on consistency concerns
- Adjust avatar treatment or empty state illustrations based on specific feedback

---

## 7. Common Patterns and Trade-offs

Reference these patterns when explaining options.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| **Custom illustration system** | Consumer apps, strong brand, marketing-heavy products | Expensive to produce and maintain; needs an illustrator for each new addition |
| **Curated stock photography** | B2B products, editorial content, team/company pages | Looks generic; hard to maintain consistent grading across stock sources |
| **Duotone/brand overlay** | Making stock photography feel branded, hero sections | Can feel forced; some subjects look unnatural with color overlays |
| **Abstract patterns/gradients** | Developer tools, dashboards, products avoiding human imagery | Can feel cold or generic; low information content |
| **No imagery (content-first)** | Data-heavy products, productivity tools, developer tools | Users associate with "unfinished"; needs strong typography and color to compensate |
| **Inline spot illustrations** | Feature explanation, empty states, onboarding steps | Breaks if the style is not rigorously consistent; can feel childish in professional tools |
| **Avatar-heavy (people-first)** | Social apps, collaboration tools, community platforms | Needs fallback for missing photos; privacy considerations for user content |

---

## 8. Compare View Specifics

When generating `compare.html`, follow `crit-loop.md` with these additions:

- **Include the dark mode toggle** if the locked color system has dark mode. Imagery must
  work on both light and dark surfaces.
- Each option card must show imagery applied to real product surfaces (hero, cards, empty
  states), not as a loose mood board. The user needs to see how imagery fits the locked layout.
- The comparative take must address: which direction best fits the brand, which is most
  sustainable to maintain, and whether imagery adds functional value or just decoration.

---

## 9. Relationship to Adjacent Facets

### Color System (locked before)

The locked palette constrains imagery color. Photography color grading must harmonize with
the palette. Illustration colors must come from (or complement) the locked palette. If the
palette is cool and muted, warm saturated photography will clash.

### Typography (locked before)

Text overlaid on imagery uses the locked type scale. Verify that heading sizes read clearly
over the proposed imagery style. If the imagery is visually busy, text overlays need
background treatments (scrim, text shadow, card overlay).

### Edge States (locked before)

Empty state wireframes define where illustrations appear. The illustration style must work at
the sizes and placements defined in the locked edge states. Read the edge state wireframes
to know which states need illustrated treatment.

### Content Voice (locked before or concurrent)

Illustration tone must match the voice. A playful illustration system with a formal voice
creates dissonance. A minimal geometric illustration system with a warm friendly voice feels
disconnected. Align visual and verbal personality.

### Accessibility (runs after)

The accessibility audit will evaluate: alt text strategy for all imagery, contrast ratios for
text over images, whether informational illustrations communicate without color alone, and
whether decorative images are properly marked as decorative.

---

## 10. Locking and Handoff

When the user locks an imagery option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: imagery type (photography, illustration, or none),
   style description (e.g., "flat geometric illustrations with 2px stroke and 4-color
   palette"), placement rules (which surfaces use imagery), and production approach
   (custom, stock, or mixed).
3. The locked imagery direction feeds into `design-tokens.json` at the direction stage for
   any codified values (avatar sizes, aspect ratios, border treatments).
4. Announce: "Imagery direction locked as [name]. [summary of approach]. The accessibility
   audit will check alt text strategy and image-text contrast."

---

## 11. Dark Mode Considerations

Imagery behaves differently in dark mode. Include these notes when the design supports dark mode:

- **Photography:** May need separate dark-mode color grading. Images that look natural on white
  can feel jarring on dark surfaces. Consider slightly reducing brightness or adding a subtle
  dark overlay.
- **Illustrations:** Colors that are vibrant on white may need to be muted on dark backgrounds
  to avoid glare. Line-art illustrations may need inverted strokes (dark-on-light to light-on-dark).
- **Hero sections:** Background gradient or scrim color must adapt. A white scrim on light mode
  becomes a dark scrim on dark mode.
- **Avatars:** Border colors, background fallbacks, and shadow treatments all change in dark mode.

---

## 12. Constraints

- Every imagery specimen must be rendered in context (inside a hero section, card, empty state),
  not as standalone mood board items. The user must see imagery where it will actually appear.
- Since option HTML must be self-contained (no external images), represent imagery through CSS
  shapes, gradients, patterns, and color blocks that communicate the style direction. Use
  placeholder containers with labels like "[Hero photograph: warm, people-centric, editorial
  crop]" combined with CSS treatments that demonstrate the color grading and mood.
- Illustration style rules must be specific enough to be reproducible. "Friendly illustrations"
  is not acceptable. Specify stroke weight, corner style, color source, maximum element count,
  and character proportions.
- Every surface in the imagery placement map must have a specimen in the option. No placements
  left unaddressed.
- If proposing custom illustration, include at least 3 specimen scenes showing the style at
  different sizes and subjects to demonstrate consistency.
- If proposing photography, include color grading specs and composition rules specific enough
  for a photographer or photo editor to follow.
