---
name: crit-elevation-shape
description: Use when evaluating depth, layering, shadow systems, surface treatments, and shape language (border radius, corner treatment) in a design.
---

# Elevation & Shape Crit

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are critting elevation and shape -- the Z-axis strategy, shadow systems, surface treatments, and shape language that give a design its sense of depth and materiality. Elevation tells users what sits on top of what. Shape language (border radius, corner treatments) communicates brand tone and functional grouping. Together, they turn flat rectangles into a layered, tangible interface.

Elevation and shape sit in the compositional lens. This facet depends on `content-layout` (surfaces must exist before they receive depth treatment) and `component-design` (components define what needs elevation). It feeds into sensory facets like `color-system` (surface colors by elevation), `density-spacing` (spacing interacts with perceived depth), and `accessibility` (contrast requirements change with layered surfaces).

---

## 1. Load Context

Before generating any options, read:

1. `.design-crit/state.json` -- current facet state, round number, locked decisions
2. `.design-crit/brief.md` -- project purpose, brand tone, platform
3. `.design-crit/facets/elevation-shape/feedback-round-{N-1}.json` -- prior feedback (skip for round 1)
4. All locked option files from prior facets -- especially `content-layout` (surfaces to elevate), `component-design` (components that need depth), and `content-hierarchy` (emphasis that elevation reinforces)

State what you are building on: "Building on the locked layout with [N surfaces]. Component design defines [key components needing elevation]. Hierarchy uses [N] emphasis levels. Elevation and shape decisions layer onto this structure."

---

## 2. Artifact Type

Generate **elevation diagrams, surface layer mockups, and shape language specimens**.

Each option file (`option-{x}.html`) must contain:

### Elevation Scale Diagram

A vertical diagram showing all elevation levels in the system, from the lowest (background) to the highest (critical overlays). For each level, show:

- **Level number** (0 through N, where 0 is the base surface)
- **Shadow specification** -- offset-x, offset-y, blur radius, spread radius, color/opacity
- **What lives at this level** -- list the UI elements assigned to each level
- **Visual example** -- a rendered rectangle at that elevation casting its shadow onto the level below

Render the levels as stacked cards with visible shadows so the user can see the depth progression.

### Surface Treatment Specimens

Show how surfaces look at each elevation level:

- **Background surface** (level 0) -- the canvas, the lowest layer
- **Content surface** (level 1) -- cards, content areas, the primary working surface
- **Raised surface** (level 2) -- navigation bars, toolbars, elevated headers
- **Overlay surface** (level 3+) -- modals, drawers, popovers, dropdown menus
- **Critical overlay** (highest level) -- toast notifications, system alerts, tooltips

For each surface, show the treatment: shadow properties, background appearance, border treatment (if any), and the visual relationship to the surface below it.

Use grayscale or very subtle neutral tones. This facet is about depth and form, not color. The color system facet will assign surface colors later.

### Shape Language Specimens

Define and display the shape vocabulary:

**Border Radius Scale:**
Show a row of specimen shapes at each radius value in the system. Example scale:

| Token | Value | Used For |
|---|---|---|
| `radius-none` | 0px | Tables, code blocks, full-bleed elements |
| `radius-sm` | 4px | Buttons, inputs, chips |
| `radius-md` | 8px | Cards, panels, modals |
| `radius-lg` | 12-16px | Large cards, hero sections, containers |
| `radius-full` | 9999px | Avatars, pills, circular buttons |

**Corner Treatment Rules:**
- Are all corners of an element the same radius, or do some elements use mixed corners (e.g., attached elements with flat joining edges)?
- How do nested elements handle radius? Inner element radius = outer radius minus padding.
- How do full-bleed child elements interact with parent corners (overflow hidden vs matching radius)?

**Shape Specimens:**
Render the key components from the locked component design with the shape language applied: buttons, cards, inputs, modals, tooltips. Show how the radius scale gives each component its appropriate softness or sharpness.

### Overlay Stacking Diagram

Show how overlapping surfaces layer in a realistic scenario:

1. Base surface with content
2. A card elevated above the base
3. A modal overlay above the card
4. A tooltip or popover above the modal (if the system supports this depth)

Render this as a 3D isometric or offset-stack view so the user can see the physical layering. Include shadow interaction between layers.

---

## 3. Evaluation Criteria

Score each option against these questions. State your assessment in the critique.

### Elevation Communicates Hierarchy

Does the elevation system reinforce the content hierarchy? Higher-elevation elements should demand more attention. Check:

- Are interactive overlays (modals, menus) clearly above static content?
- Do cards and panels feel appropriately separated from the background?
- Is there a clear distinction between the base surface and elevated surfaces?
- Does elevation change meaningfully between levels, or do adjacent levels look identical?

### Shadow System Consistency

Is the shadow system systematic and predictable?

- Do shadows scale proportionally with elevation? (Higher = larger blur, more offset, lighter opacity)
- Are shadow values tokenized (defined as a scale) or ad hoc (different values for each component)?
- Is the shadow color consistent? (Usually a single dark color at varying opacities, not multiple hue-shifted shadows.)
- Do shadows look natural? (Light source should be consistent -- typically top-center or top-left.)

### Shape Language Matches Brand Tone

Does the shape vocabulary communicate the right personality?

- **Sharp corners (0-2px):** Technical, precise, editorial, enterprise. Signals seriousness and efficiency.
- **Moderate rounding (4-8px):** Balanced, approachable but professional. The safe middle ground for most products.
- **Heavy rounding (12-24px):** Friendly, playful, consumer-oriented. Signals warmth and approachability.
- **Full rounding (pill shapes):** Casual, modern, app-like. Common in mobile-first and social products.

Does the chosen shape language match the brand tone described in the brief? A children's education app and a financial compliance tool should not share the same radius scale.

### Level Count Appropriateness

Is the number of elevation levels appropriate for the project?

- **2-3 levels:** Sufficient for simple apps with minimal overlays. Base, content, overlay.
- **4-5 levels:** Standard for most web applications. Base, content, raised, overlay, critical.
- **6+ levels:** Complex applications with nested overlays, multi-panel layouts. Risk: too many levels are indistinguishable.

More levels is not better. Each level must be visually distinct and serve a clear purpose.

---

## 4. Surface Treatment Approaches

Reference these approaches when generating options.

### Flat with Borders

No shadows. Surfaces distinguished by borders, background color differences, and spacing. Clean, modern, high-performance (no shadow rendering).

- **Best for:** Minimal designs, text-heavy interfaces, developer tools, design systems that prioritize simplicity.
- **Watch out for:** Harder to show overlay hierarchy, can feel sterile, borders can create visual noise with many surfaces.

### Subtle Shadows

Small, tight shadows (1-4px blur) with low opacity. Surfaces feel slightly lifted but grounded. Most common approach in modern web design.

- **Best for:** Professional applications, SaaS products, content platforms, dashboards.
- **Watch out for:** Can feel generic if not calibrated carefully. Shadow values need to scale clearly between levels.

### Dramatic Shadows

Large blur (8-24px), noticeable offset, visible depth. Surfaces feel genuinely floating. Creates strong visual hierarchy through depth.

- **Best for:** Marketing sites, product showcases, hero-focused designs, creative tools.
- **Watch out for:** Can feel heavy, reduces information density, performance cost with many shadowed elements.

### Glass / Frosted

Background blur effect behind semi-transparent surfaces. Creates depth through transparency rather than shadow.

- **Best for:** Media-rich applications, mobile overlays, creative tools, designs where background content should remain partially visible.
- **Watch out for:** Performance-intensive, readability concerns with busy backgrounds, inconsistent browser support, accessibility challenges.

### Material / Layered

Opaque surfaces with clear physical metaphor. Each layer is a distinct "sheet of paper." Material Design's approach.

- **Best for:** Complex applications with many surface levels, designs that need clear spatial logic.
- **Watch out for:** Can feel heavy-handed, the paper metaphor does not suit every brand.

---

## 5. Common Elevation and Shape Trade-offs

Present these trade-offs when they apply.

### Depth vs Performance

- **Rich depth (shadows, blur, transitions):** More visual hierarchy, tangible feel, engaging. Cost: render performance, especially with many layers or glass effects.
- **Flat depth (borders, color, spacing):** Fast rendering, clean, works everywhere. Cost: less visual hierarchy, overlays need other affordances.
- **Question to ask:** "How many elevated surfaces appear on screen simultaneously? Three cards with shadows are fine; fifty will lag."

### Uniform vs Contextual Shape

- **Uniform radius:** Every component uses the same radius (or from the same scale). Consistent, systematic, easy to maintain.
- **Contextual radius:** Radius varies by component role. Buttons are pill-shaped, cards are moderately rounded, tables are sharp. More expressive but harder to maintain.
- **Question to ask:** "Does every element need to feel part of the same family, or do different component types have different personalities?"

### Tight vs Loose Elevation Scale

- **Tight scale (1px, 2px, 4px, 8px blur):** Subtle distinctions between levels. Sophisticated, understated. Risk: levels become hard to distinguish.
- **Loose scale (2px, 8px, 16px, 32px blur):** Clear jumps between levels. Bold, obvious hierarchy. Risk: lower levels may look identical next to the dramatic upper levels.
- **Question to ask:** "How many distinct layers will the user need to perceive simultaneously?"

### Nested Radius Handling

- **Consistent inner radius:** Inner element radius = outer radius - padding. Mathematically correct, looks smooth.
- **Same radius everywhere:** Inner and outer elements use the same radius value. Simpler to implement, slightly off visually at tight paddings.
- **Question to ask:** "Is this a design system that will be implemented precisely, or a rapid prototype where simplicity matters more?"

---

## 6. Option Generation Guidance

### Round 1: Diverge

Generate 2-4 options exploring different approaches to depth and shape. Each option should present a complete elevation + shape system, not just different shadow values.

Useful axes of variation:

- **Depth philosophy:** One option uses flat-with-borders (no shadows). Another uses dramatic shadows. Another uses glass/frosted effects.
- **Shape personality:** One option is sharp and geometric (0-2px radius). Another is soft and approachable (8-16px). Another uses mixed radius per component role.
- **Level count:** One option uses 3 levels (minimal). Another uses 5 levels (standard). Another uses contextual elevation (components change elevation on interaction).
- **Shadow character:** One option uses tight, crisp shadows. Another uses soft, diffused shadows. Another uses colored/tinted shadows for warmth.

Name each option by its character: "Flat & Sharp," "Soft Depth," "Glass Layers," "Bold Material."

### Round 2: Converge

Refine based on feedback. Typical refinements:

- Adjust shadow intensity (too heavy, too subtle)
- Merge the radius scale from one option with the shadow approach of another
- Resolve inconsistencies in the elevation scale
- Adjust level count based on actual overlay needs

### Round 3+: Polish

Focus on: shadow token precision (exact values for implementation), nested radius math, overlay stacking order for all identified overlay combinations, and ensuring the elevation system works with the locked layout across all screens.

---

## 7. Per-Option Critique Checklist

When writing the critique for each option in `critique.md`, address every item:

1. How many elevation levels does the system define? Is each level visually distinct?
2. Does the elevation system reinforce the content hierarchy?
3. Are shadow values systematic (tokenized scale) or ad hoc?
4. Is the light source consistent across all shadows?
5. Does the shape language (radius scale) match the brand tone from the brief?
6. How do overlays (modals, popovers, tooltips) stack visually?
7. Are there performance concerns with the chosen depth approach?
8. How does the elevation interact with the locked layout and components?

---

## 8. Constraint Propagation

Elevation and shape decisions constrain downstream facets:

- **Color system** must define surface colors per elevation level. Elevated surfaces are often slightly lighter in light mode (or lighter/darker in dark mode) to reinforce depth.
- **Density-spacing** interacts with elevation -- elevated elements often need more internal padding to feel balanced at their depth level.
- **Typography** may need adjusted weights on different surface levels if contrast changes with elevation.
- **Accessibility** will audit contrast ratios on every surface level, focus indicators against elevated backgrounds, and whether elevation changes are perceivable to users who cannot see shadows.

State these downstream implications in the critique: "This elevation system uses [N] levels. The color system will need [N] surface color variants. Accessibility will need to verify contrast at each level."

---

## 9. Locking

When the user locks an elevation and shape option:

1. Record the locked option in `state.json` with `locked_summary` describing the system (e.g., "5-level elevation with subtle shadows, 4-step radius scale from 0 to 12px, moderate rounding for approachable professional tone").
2. The locked elevation and shape specimens become the depth and form reference for all downstream facets.
3. Announce the lock and its downstream implications: "Elevation and shape locked as [name]. Color system will define surface colors per elevation level. Accessibility will audit contrast across all surface levels."
