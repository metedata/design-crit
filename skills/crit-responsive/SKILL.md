---
name: crit-responsive
description: Use when evaluating how a design adapts across screen sizes and devices — breakpoint strategy, layout reflow, and touch vs pointer adaptation.
---

# Responsive Design Crit

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing how this product adapts across screen sizes and input modes.
Responsive design is a contextual facet -- it applies when the product must serve multiple
viewport widths, devices, or input methods. Your job: evaluate breakpoint strategy, layout
reflow patterns, content adaptation, and touch vs pointer accommodation, then guide the user
to a locked responsive direction.

Responsive design is distinct from the layout facet. Layout defines spatial composition at
a single canonical viewport. Responsive defines how that composition transforms across the
full viewport spectrum. If layout is the blueprint, responsive is the set of transformation
rules.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (platforms, devices, audience, primary use context)
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Responsive decisions transform all prior
structure. Pay special attention to:

- **Layout** -- the canonical grid, column count, sidebar width, content regions to reflow
- **Navigation** -- nav pattern at desktop that must adapt (sidebar -> hamburger? top bar -> bottom tabs?)
- **Component design** -- components that change form factor (table -> card list, multi-column form -> stacked)
- **Typography** -- type scale that may need fluid sizing or scale adjustment per breakpoint
- **Density/spacing** -- spacing scale that shifts with viewport (tighter on mobile, looser on desktop)
- **Content hierarchy** -- priority order that determines what stays visible vs what gets hidden or collapsed

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/responsive/`. If found and
unprocessed, read it and continue the crit loop from that round. Do not regenerate round 1.

---

## 2. Breakpoint Strategy

Every option must define a breakpoint strategy. This is the core structural decision of
responsive design.

### Common Breakpoint Sets

| Strategy | Breakpoints | Philosophy |
|---|---|---|
| **Mobile-first (3 tier)** | 0-767px, 768-1199px, 1200px+ | Start with mobile, enhance upward. Industry standard. |
| **Mobile-first (4 tier)** | 0-599px, 600-899px, 900-1199px, 1200px+ | Finer control for tablets. Better for complex layouts. |
| **Desktop-first (3 tier)** | 1200px+, 768-1199px, 0-767px | Start with desktop, simplify downward. Common in enterprise. |
| **Content-driven** | Breakpoints set where the layout breaks, not at device widths | Most responsive, but harder to communicate. |
| **Component-level** | No global breakpoints; each component defines its own via container queries | Modern approach. Components adapt to their container, not the viewport. |

### Choosing a Strategy

Read the brief to determine the right approach:

- **Audience device split** -- if 70%+ mobile traffic, mobile-first is essential. If desktop-dominant (enterprise SaaS), desktop-first may be pragmatic.
- **Core task by device** -- identify the primary task per device. If the mobile task differs from desktop (consumption vs creation), the responsive strategy must support both paths.
- **Tech stack** -- container queries require modern browser support. Check the brief's platform constraints.
- **Design system maturity** -- content-driven breakpoints require more design work per component. Fixed breakpoints are simpler to systematize.

### What Each Breakpoint Must Define

For every breakpoint in the option, specify:

1. **Viewport range** -- min and max width
2. **Grid adjustment** -- column count, gutter width, margin width at this range
3. **Layout reflow** -- which content areas change position, size, or visibility
4. **Navigation adaptation** -- how the locked nav pattern transforms
5. **Typography adjustment** -- any type scale changes (fluid type or step changes)
6. **Spacing adjustment** -- density/spacing scale multiplier change

---

## 3. Layout Reflow Patterns

Evaluate how content areas transform across breakpoints. Each pattern has trade-offs.

### Reflow Strategies

| Strategy | Behavior | Use When |
|---|---|---|
| **Stack** | Side-by-side columns collapse to a vertical stack | Default for most multi-column content. Simple, predictable. |
| **Reorder** | Content areas change visual order at smaller viewports | When mobile priority differs from desktop reading order. Use CSS `order` property. |
| **Hide** | Non-essential content is hidden at smaller viewports | When some content is desktop-only (e.g., decorative sidebar, preview pane). |
| **Simplify** | Complex component swaps to a simpler form | Tables -> card lists, multi-step forms -> wizard, data grid -> summary cards. |
| **Collapse** | Content is available but tucked behind a toggle or accordion | When everything is needed but not everything fits. Sidebars -> drawers, filters -> expandable panel. |
| **Promote** | Mobile elevates something that was secondary on desktop | When mobile users need quick access to a feature buried in the desktop layout. |

### Evaluating Reflow

For each content area in the locked layout, trace its reflow path across all breakpoints:

```
Desktop (1200px+)     -> Tablet (768-1199px)   -> Mobile (0-767px)
Sidebar: visible 260px   Sidebar: rail 64px       Sidebar: hidden, hamburger
Content: 3-column grid   Content: 2-column grid    Content: 1-column stack
Detail panel: right 30%  Detail panel: full below   Detail panel: full-screen overlay
Filter bar: horizontal   Filter bar: horizontal     Filter bar: collapsed into drawer
```

This trace is mandatory in each option HTML. Show it as an annotated diagram.

---

## 4. Navigation Adaptation

The locked navigation must adapt across breakpoints. Evaluate the adaptation pattern.

### Common Navigation Adaptations

| Desktop Pattern | Tablet Adaptation | Mobile Adaptation |
|---|---|---|
| **Full sidebar** | Collapsed rail (icons only) | Hamburger menu or bottom sheet |
| **Top navigation bar** | Same, possibly condensed | Hamburger or bottom tab bar |
| **Tab bar (horizontal)** | Same, possibly scrollable | Bottom tab bar (limit to 5 tabs) |
| **Breadcrumb trail** | Truncated with ellipsis | Back arrow + current page title |
| **Mega menu** | Simplified dropdown | Full-screen overlay menu |

### Adaptation Criteria

- The user must be able to reach every nav destination at every breakpoint.
- The mobile nav should require no more than 2 taps to reach any primary destination.
- Collapsed or hidden nav must have a clear trigger affordance (hamburger icon, tab bar).
- Current location indicator must remain visible at all breakpoints.

---

## 5. Touch vs Pointer Accommodation

Evaluate how the design adapts to different input methods.

### Touch Considerations

| Element | Mouse/Pointer | Touch |
|---|---|---|
| **Target size** | 24px minimum, 32px+ recommended | 44px minimum (iOS), 48dp minimum (Android) |
| **Hover states** | Primary discovery mechanism | Unavailable. Replace with tap, long-press, or visible affordance. |
| **Drag and drop** | Fine-grained cursor control | Coarse, requires large grip handles and clear drop zones |
| **Right-click** | Context menus, secondary actions | Long-press (unreliable discovery). Prefer visible action menus. |
| **Text selection** | Click-and-drag | Long-press + handles. Harder in dense text. |
| **Scroll** | Scroll wheel, precise | Momentum scroll, imprecise. Avoid scroll hijacking. |

### Hover State Replacement

Every hover-dependent interaction must have a touch alternative. Audit the locked
component designs for hover-only patterns:

- **Hover-to-reveal actions** (e.g., row actions on hover) -> Always-visible actions or swipe-to-reveal
- **Hover tooltips** -> Tap-to-show tooltip or inline text
- **Hover previews** -> Tap preview or detail navigation
- **Hover color change** (decorative) -> Remove or use active/pressed state instead

### Pointer Precision Zones

On touch devices, the touch imprecision zone is approximately 7mm around the point of
contact. Ensure no two interactive elements fall within 8px of each other. Dense desktop
UIs that work with a mouse cursor may become unusable with fingers.

---

## 6. Content Adaptation

Evaluate how content itself transforms across breakpoints, beyond layout reflow.

### Image and Media Scaling

- **Art direction** -- different image crops for different breakpoints, not just resized.
  A wide hero image may need a square crop on mobile to maintain subject visibility.
- **Responsive images** -- use `srcset` and `sizes` attributes for performance. Serve
  smaller images to smaller viewports.
- **Video** -- auto-play video may be inappropriate on mobile (data cost, battery). Consider
  static thumbnail with play button on mobile.

### Typography Adaptation

| Approach | How It Works | Trade-off |
|---|---|---|
| **Fluid type** | `clamp()` function scales font size between min and max across viewport range | Smooth, no jumps. Harder to control precisely. |
| **Stepped type** | Type scale changes at breakpoints (e.g., H1 is 36px desktop, 28px mobile) | Predictable, easy to control. Visible jump at breakpoint. |
| **Scale ratio shift** | Desktop uses 1.25 ratio, mobile uses 1.2 ratio | Naturally compresses the hierarchy for small screens. |

### Content Priority

Not all content deserves equal treatment on mobile. Define a content priority map:

- **Always visible** -- core task content, primary actions. Visible at all breakpoints.
- **Available on demand** -- secondary content, filters, metadata. Collapsed or behind a toggle on mobile.
- **Desktop only** -- decorative elements, preview panes, secondary visualizations. Hidden on mobile.

State the content priority map in each option.

---

## 7. Generating Options

### Round 1: Diverge

Generate 2-4 responsive options. Each must make a different bet about breakpoint strategy,
reflow approach, or adaptation philosophy. Maximize variety.

Example option set for a project management SaaS:

- **Mobile-First Stacker** -- 3-tier mobile-first breakpoints. Everything stacks vertically
  on mobile. Navigation becomes bottom tabs. Tables convert to card lists. Optimized for
  on-the-go task checking.
- **Desktop-Anchored Simplifier** -- desktop-first, 3-tier. Full-featured desktop layout
  preserved. Tablet collapses sidebar to rail. Mobile hides non-essential panels, simplifies
  views. Optimized for power users who occasionally check on mobile.
- **Container-Adaptive** -- component-level breakpoints using container queries. Each
  component adapts to its available space, not the viewport. Dashboard widgets reflow
  independently. Optimized for flexible layouts and future-proofing.

Each option HTML must include:

1. Breakpoint strategy diagram: viewport ranges with grid/column specs per range
2. Reflow trace: the locked layout shown at each breakpoint with annotations
3. Navigation adaptation: locked nav pattern at each breakpoint
4. At least one component transformation (table -> cards, form reflow, nav collapse)
5. Touch target annotations on the mobile viewport
6. Content priority map

Render all breakpoint variants within a single option file. Stack the viewport
representations vertically: desktop at top, tablet in middle, mobile at bottom. Label
each viewport width clearly.

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Adjust breakpoint values (e.g., tablet breakpoint too early or too late)
- Change reflow strategy for a specific content area (stack vs collapse vs hide)
- Improve navigation adaptation based on mobile usability feedback
- Refine touch target sizing for specific components
- Adjust content priority (show more or less on mobile)

When merging two options, create a new option file (e.g., `option-ab.html`) and state
what was taken from each in the rationale.

---

## 8. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite which
breakpoints, which reflow patterns, which components.

### Core Task Completability

- Can the user complete every core task at every breakpoint?
- Is any essential feature hidden or inaccessible on mobile?
- Does the mobile experience feel like a complete product, not a degraded desktop?

### Adaptation Intentionality

- Does each breakpoint feel designed, or does the layout just collapse?
- Are reflow decisions (stack, hide, simplify) deliberate and documented?
- Is there a clear content priority map driving what stays and what goes?

### Navigation Consistency

- Can the user navigate the full information architecture at every viewport?
- Does the mobile nav pattern match platform conventions (bottom tabs for iOS, hamburger for Android)?
- Is the current location always visible?

### Touch Readiness

- Do all interactive elements meet 44px minimum touch target on mobile?
- Are hover-dependent interactions replaced with touch alternatives?
- Is there adequate spacing between adjacent touch targets?

### Performance

- Does the mobile version avoid loading desktop-only assets?
- Are images art-directed or just shrunk?
- Is the breakpoint strategy compatible with the tech stack's responsive tooling?

### Consistency Across Breakpoints

- Does the design feel like the same product at every breakpoint?
- Are colors, typography, and spacing consistent (adjusted, not redesigned)?
- Can a user switching between devices (laptop at desk, phone on couch) recognize the product?

---

## 9. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** if the locked color system has dark mode. Responsive
  behavior must work in both themes.
- Each option card's iframe must show the multi-viewport representation (desktop + tablet +
  mobile stacked vertically). The user needs to see all breakpoints simultaneously.
- Below each iframe, include a "Breakpoint Map" expandable section listing every breakpoint
  with its viewport range, column count, and key reflow changes.
- The comparative take ("My Take") must address: which option provides the strongest mobile
  experience, which is most intentional in its adaptation strategy, and whether the
  breakpoint count is right for the project. State your recommendation.

---

## 10. Locking and Handoff

When the user locks a responsive option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: breakpoint strategy (mobile-first/desktop-first),
   breakpoint count and values, key reflow patterns, and navigation adaptation approach.
3. The locked option file becomes the responsive reference for downstream facets
   (accessibility will audit mobile touch targets, keyboard nav at all breakpoints).
4. Responsive specifications from the locked file inform implementation at the direction
   stage. Extract breakpoint values, reflow rules, and adaptation patterns for the
   engineering handoff.

---

## 11. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| Mobile-first 3 breakpoints | Standard web products, broad audience | Desktop may feel like stretched mobile |
| Desktop-first simplification | Enterprise SaaS, desktop-dominant usage | Mobile feels like an afterthought |
| Container queries | Modern stacks, flexible layouts, widget-based | Browser support limits; harder to reason about globally |
| Fluid everything (no breakpoints) | Simple content sites, blogs | Complex layouts need discrete breakpoints |
| Separate mobile layout | When mobile task differs fundamentally | Two designs to maintain; divergence over time |
| Responsive + adaptive hybrid | Performance-critical mobile + rich desktop | Implementation complexity |

---

## 12. Relationship to Adjacent Facets

### Layout (locked before)

Layout defines the canonical viewport composition. Responsive transforms it. Do not
redesign the layout -- define how it adapts. If responsive reveals that the locked layout
does not adapt well, flag it: "The locked 3-column layout may need a responsive revision.
Want to revisit layout, or adapt within the current structure?"

### Density/Spacing (locked before or after)

Spacing may shift per breakpoint: desktop compact, mobile comfortable. Define the
multiplier per breakpoint if the density facet is already locked. If density is not yet
locked, note the responsive spacing needs for the density facet.

### Navigation (locked before)

Navigation adaptation is constrained by the locked nav pattern. Propose how the locked
pattern transforms; do not propose a different pattern.

### Typography (locked before or after)

Type scale adaptation (fluid type, stepped type, scale ratio shift) is a responsive
decision but uses the locked type scale as the source. Define how the scale adapts, not
what the scale is.

---

## 13. Constraints

- Every breakpoint must show the complete locked layout adapted, not just one section.
- The mobile viewport must be a complete, usable product experience, not a degraded desktop.
- All locked navigation destinations must be reachable at every breakpoint.
- Touch targets on mobile must meet 44px minimum. No exceptions.
- Hover-only interactions must have touch alternatives documented in the option.
- Content that is hidden on mobile must be explicitly listed in the content priority map.
- Responsive options must use the locked color palette, type scale, and spacing system.
  Adjust scale application per breakpoint, but do not introduce new values.
- Each option must show at least 3 viewport representations (desktop, tablet, mobile)
  unless the brief specifies a single platform.
