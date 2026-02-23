---
name: crit-density-spacing
description: Use when evaluating information density, whitespace, spacing systems, and compact vs comfortable modes in a design.
---

# Density & Spacing Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing the density and spacing system for this project. Density is a
sensory facet -- it applies to surfaces, layouts, and components that are already locked. Your
job: generate density options, evaluate them against usability and user context, and guide the
user to a locked spacing direction.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (target users, device, context of use, scope)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Density decisions apply to existing structure.
Pay special attention to:

- **Layout** -- grid structure, sidebar widths, content regions define where spacing applies
- **Navigation** -- nav item density, menu spacing, wayfinding padding
- **Component design** -- buttons, form fields, cards all have internal spacing
- **Typography** -- line height, paragraph spacing interact with density
- **Color system** -- surface layers and elevation affect perceived density

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/density-spacing/`. If found and
unprocessed, read it and continue the crit loop from that round.

---

## 2. Spacing Scale Foundation

Every option must define a spacing scale. The scale is the atomic unit system that all
spacing derives from.

### Base Unit

Choose one base unit per option. The two common approaches:

- **4px base** -- finer granularity (4, 8, 12, 16, 20, 24, 32, 40, 48, 64). Good for
  dense interfaces with precise control.
- **8px base** -- coarser, simpler (8, 16, 24, 32, 48, 64, 80, 96). Good for spacious
  layouts where simplicity matters.

### Scale Definition

Define named tokens for the scale. Use T-shirt sizing or numbered steps:

```
space-xs:   4px    (or 0.5 base)
space-sm:   8px    (1 base)
space-md:   16px   (2 base)
space-lg:   24px   (3 base)
space-xl:   32px   (4 base)
space-2xl:  48px   (6 base)
space-3xl:  64px   (8 base)
space-4xl:  96px   (12 base)
```

Each option HTML must render the full scale as a visual reference: labeled bars showing each
step with its pixel value and token name.

### Applying the Scale

All spacing in the option must use scale tokens. No arbitrary pixel values. This includes:

- Padding inside components (buttons, cards, inputs)
- Gaps between components (form fields, list items, card grids)
- Margins between sections (content groups, page sections)
- Gutters in grid layouts

---

## 3. Density Spectrum

Position each option on the density spectrum. The spectrum is not good-to-bad; it is
context-dependent. Every position has valid use cases.

### The Four Density Zones

| Zone | Padding | Use Case | Example Products |
|---|---|---|---|
| **Spacious** | Generous whitespace, large margins, breathing room | Marketing sites, landing pages, luxury products | Apple.com, Stripe, Notion marketing |
| **Comfortable** | Balanced padding, readable spacing | Consumer apps, general SaaS, content platforms | Gmail, Slack, Notion app |
| **Compact** | Reduced padding, tighter gaps, more visible content | Power tools, productivity apps, data-informed workflows | Linear, VS Code, Figma |
| **Dense** | Minimal padding, maximum information per viewport | Dashboards, trading platforms, admin panels | Bloomberg Terminal, Grafana, Jira board view |

### Brief-Driven Positioning

Read the target users and context of use from the brief to determine the right zone:

- **Frequency of use** -- daily-use tools benefit from compact; occasional-use tools need
  comfortable or spacious to reduce learning curve
- **User expertise** -- power users tolerate and prefer density; casual users need breathing room
- **Device and posture** -- phone needs larger touch targets (spacious); desktop mouse allows
  compact; tablet falls between
- **Content volume** -- high-information products (dashboards) need density; low-information
  products (settings) need space
- **Session duration** -- long sessions need comfortable spacing to reduce fatigue; quick
  interactions can be denser

---

## 4. Touch Target Requirements

Regardless of density zone, interactive elements must meet minimum touch target sizes.

| Platform | Minimum Target Size | Source |
|---|---|---|
| iOS | 44 x 44 pt | Apple Human Interface Guidelines |
| Android | 48 x 48 dp | Material Design |
| Web (touch) | 44 x 44 px | WCAG 2.5.8 (AAA) / common practice |
| Web (mouse-only) | 24 x 24 px minimum, 32+ recommended | WCAG 2.5.8 |

### How This Interacts with Density

Dense layouts can have small visual elements but must maintain touch target size through
padding. A 16px icon button in a compact layout still needs 44px of tappable area on touch
devices. Methods:

- **Padding expansion** -- visual element is small, clickable area is larger
- **Spacing as target** -- gaps between targets count toward tap area
- **Progressive targets** -- compact on desktop (32px), expanded on touch (48px)

In each option HTML, annotate at least three interactive elements with visible touch target
overlays showing the actual tappable area vs the visual element.

---

## 5. Whitespace Purpose

Whitespace is not empty space -- it is a design element. Each option must demonstrate
intentional whitespace.

### Whitespace Functions

| Function | What It Does | Example |
|---|---|---|
| **Grouping** | Associates related elements (Gestalt proximity) | Tighter spacing within a card, larger gap between cards |
| **Separation** | Creates visual boundaries without lines | Section spacing in a form |
| **Emphasis** | Draws attention to isolated elements | A CTA button with generous surrounding space |
| **Rhythm** | Creates visual cadence and scanability | Consistent vertical spacing between list items |
| **Rest** | Gives the eye a place to rest, reduces cognitive load | Margins around content blocks |

### Whitespace Audit in Critique

In your critique, identify at least two places in each option where whitespace is doing
meaningful work and one place where it could be improved. Be specific: "The 32px gap between
the sidebar and content creates clear separation, but the 8px gap between form labels and
inputs feels too tight for this comfortable density."

---

## 6. Compact Mode Patterns

If the brief's user base includes both casual and power users, or if the product has both
overview and detail views, consider offering a compact mode toggle.

### Pattern 1: User-Controlled Toggle

A settings toggle or UI control that switches between comfortable and compact. Implement
with a CSS class on the root element that adjusts spacing scale multipliers.

```css
:root { --density-factor: 1; }
[data-density="compact"] { --density-factor: 0.75; }
[data-density="spacious"] { --density-factor: 1.25; }
```

### Pattern 2: Progressive Density

Different density levels for different parts of the UI within the same view. Sidebar
navigation is compact; main content area is comfortable; marketing sections are spacious.

### Pattern 3: Responsive Density

Density shifts based on viewport size. Desktop gets compact (mouse precision); tablet gets
comfortable; phone gets spacious (touch targets). This overlaps with the responsive facet
but the spacing scale definition lives here.

### When to Include in Options

Include compact mode in at least one option when:
- The brief mentions both power users and casual users
- The project type is productivity or data-heavy
- The platform includes both desktop and mobile
- Prior locked components suggest complex, information-rich screens

---

## 7. Generating Options

### Round 1: Diverge

Generate 2-4 density options. Each must target a different zone on the density spectrum or
present a different spacing strategy. Maximize variety.

Example option set for a project management SaaS:

- **Breathing Room** -- comfortable zone, 8px base, generous section spacing. Optimized for
  readability and new user onboarding. Trade-off: less content visible per screen.
- **Productive Compact** -- compact zone, 4px base, tight but organized. Optimized for power
  users scanning lots of items. Trade-off: may feel cramped on first use.
- **Adaptive Density** -- comfortable default with compact toggle. Two modes using the same
  scale with different multipliers. Trade-off: more complexity, two states to maintain.

Each option HTML must include:
1. The spacing scale rendered as a visual reference (labeled bars)
2. Applied mockup: the locked layout wireframe with this density applied to all components,
   sections, and gaps. Apply locked colors if available.
3. Touch target annotations on at least three interactive elements
4. Side-by-side comparison within the option: a list view, a form, and a card grid showing
   how the density plays out across different content types

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:
- Adjust specific spacing values (e.g., "card padding too tight, section margins fine")
- Shift density zone (e.g., "somewhere between comfortable and compact")
- Add or remove compact mode toggle based on user interest
- Tighten touch target compliance

---

## 8. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite pixel
values, screen areas, and user scenarios.

### Context Appropriateness
- Is the density zone right for the target users and their context of use?
- Does the density match the session duration and interaction frequency from the brief?
- Would a first-time user feel overwhelmed (too dense) or underwhelmed (too spacious)?

### Touch Target Compliance
- Do all interactive elements meet minimum touch target sizes for the target platform?
- Are there any elements where visual size and tappable area diverge without clear padding?
- On touch devices, can the user tap any button without accidentally hitting an adjacent one?

### Spacing Consistency
- Does all spacing derive from the defined scale? No arbitrary pixel values?
- Is the scale applied consistently across different content types (lists, forms, cards)?
- Does the vertical rhythm feel intentional? Can you identify a consistent baseline?

### Whitespace Quality
- Is whitespace performing grouping, separation, emphasis, rhythm, or rest functions?
- Are there areas where elements feel too close (cognitive overload) or too far (disconnected)?
- Does the whitespace hierarchy match the content hierarchy from the locked content-hierarchy facet?

### Scalability
- Can the spacing system accommodate new component types without new tokens?
- Does the density work at all viewport widths the product supports?
- If a compact mode toggle is included, does both modes feel intentional?

---

## 9. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** if the locked color system has dark mode. Density must
  work in both themes.
- Each option card's iframe must show the applied mockup, not just spacing bars. The user
  needs to feel the density in realistic content, not abstract geometry.
- Below each iframe, include a "Spacing Scale" expandable section showing all defined tokens
  with pixel values.
- If two options differ only in specific spacing values (not overall zone), add inline
  annotations calling out the differences: "Card padding: 16px vs 12px."
- The comparative take ("My Take") must address: which density feels right for the target
  user, which has the best touch target compliance, and whether compact mode adds value.
  State your recommendation.

---

## 10. Locking and Token Extraction

When the user locks a density option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: base unit (4px or 8px), density zone, compact mode
   (yes/no), and touch target compliance status.
3. The locked option file becomes the spacing reference for all downstream facets
   (content-voice for text spacing, accessibility for target sizes).
4. Spacing tokens from the locked file feed into `design-tokens.json` at the direction
   stage. Ensure all CSS custom properties are parseable.

---

## 11. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| 8px grid everywhere | Simple, consistent, easy to maintain | Too coarse for dense data UIs |
| 4px base with 8px multiples | Fine control where needed, round numbers | More tokens to manage |
| Content-driven density | Different density per content type | Inconsistency if not systematic |
| Viewport-responsive density | Adapts to device automatically | Unexpected layout shifts on resize |
| User-toggleable density | Serves both power and casual users | Two layouts to test and maintain |
| Fixed sidebar, fluid content | Sidebar compact, content comfortable | Density mismatch at boundary |

---

## 12. Relationship to Adjacent Facets

### Typography (locked before or after)

Line height is both a typography and spacing decision. If typography is already locked, use
its line height as a spacing input. If not yet locked, define line height ranges that the
typography facet should respect.

### Layout (locked before)

Grid gutters and column gaps are defined by the layout facet. Density refines the interior
spacing WITHIN those grid cells. Do not change the grid structure.

### Component Design (locked before)

Component internal padding (button padding, card padding, input height) is refined here.
The component facet defined structure; density defines breathing room within that structure.

---

## 13. Constraints

- Every pixel value in the option must come from the defined spacing scale. No magic numbers.
- Touch targets must meet platform minimums even in the most compact option.
- If the locked color system has dark mode, every density option must render correctly in
  both light and dark themes.
- Do not change the locked layout grid structure. Density adjusts spacing within the grid,
  not the grid itself.
- The spacing scale must use CSS custom properties (`--space-xs`, `--space-sm`, etc.) for
  clean token extraction.
- Include at least one realistic content mockup in every option. Abstract spacing diagrams
  alone do not communicate density.
