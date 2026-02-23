---
name: crit-layout
description: Use when evaluating page layout, grid systems, spatial composition, and how content areas relate to each other within screens.
---

# Content Layout

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer deciding how content areas are arranged within each screen. Layout is
the spatial skeleton of the product -- it determines where things go, how much space they
get, and how the page responds to different viewport sizes. Layout sits between navigation
(which defines how users move between screens) and content hierarchy (which defines what
within a screen is most important).

Your job: take the locked screen inventory and navigation model, then present 2-4 layout
options that make different bets about spatial composition, grid structure, and content
area proportions.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current project state, round number, prior feedback
2. `.design-crit/brief.md` -- the confirmed design brief
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles
5. `.design-crit/facets/screen-inventory/{locked_option}.html` -- locked screen inventory
6. `.design-crit/facets/navigation-model/{locked_option}.html` -- locked navigation model

The locked navigation model defines the chrome -- the persistent nav elements. Layout
fills the remaining space. Read the nav pattern (sidebar, top bar, tab bar) and its
dimensions. State this in the critique: "Building on the locked navigation ([pattern]):
[summary of chrome constraints]."

If edge-states were locked before layout, read those too. Edge state patterns may influence
layout (e.g., where error banners sit, how loading skeletons map to the grid).

If this is round 2+, also read:
7. `.design-crit/facets/content-layout/feedback-round-{N-1}.json`
8. Surviving option files in `.design-crit/facets/content-layout/`

---

## 2. Layout Analysis

### Content Area Inventory

For each primary screen in the locked inventory, identify the content areas -- the distinct
zones of content that need to be arranged. Content areas are NOT components; they are
regions of the page.

Example for a dashboard screen:
- **Primary content area:** The main chart or data display
- **Secondary content area:** Summary cards or KPIs
- **Auxiliary area:** Filters, date pickers, controls
- **Chrome:** Navigation sidebar (already locked)

### Content Area Classification

Classify each area by its role in the page:

| Classification | Description | Layout Implication |
|---|---|---|
| **Primary** | The main content the user came for. Gets the most space. | Largest area, above the fold, center or right of sidebar |
| **Secondary** | Supporting content that supplements the primary. | Smaller area, adjacent to primary, may scroll independently |
| **Auxiliary** | Controls, filters, tools that act on the primary content. | Fixed or sticky, often in a toolbar or panel |
| **Chrome** | Navigation, headers, footers. Already locked by nav facet. | Fixed position, subtracted from available layout space |

### Screen Layout Patterns

Map each primary screen to one of these common layout patterns:

| Pattern | Description | Best For |
|---|---|---|
| **Holy Grail** | Header + sidebar + main content + optional right sidebar + footer. Classic three-column with header/footer. | Dashboards, admin panels, tools with persistent context |
| **Sidebar + Content** | Persistent sidebar (from nav) + single content area. Simple two-column. | Most productivity apps, email clients, file managers |
| **Split Pane** | Two equal or proportional content areas side by side. Resizable divider. | Comparison views, code editors, email read view |
| **Card Grid** | Content organized as a grid of cards. Responsive reflow. | Galleries, project lists, discovery pages |
| **Single Column** | All content in one vertical flow. No columns. Full-width or max-width centered. | Mobile-first, reading-heavy, long-form content |
| **Dashboard Mosaic** | Irregular grid of widgets/panels of different sizes. | Analytics dashboards, customizable home screens |
| **Master-Detail** | List or table on one side, detail view on the other. Selection-driven. | Email, CRM, any entity-list product |
| **Canvas + Panels** | Central open canvas with docked or floating tool panels. | Design tools, map views, whiteboard apps |

Assign a pattern per screen. Multiple screens can use the same pattern. Note screens that
break the dominant pattern and explain why.

---

## 3. Grid Systems

### Grid Selection

Choose a grid system for the product. The grid determines column count, gutter width, and
margin behavior. Common systems:

**12-Column Grid**
- The standard. Divides into 1, 2, 3, 4, 6, or 12 columns.
- Use for complex layouts with multiple content areas.
- Column width: fluid (percentage-based). Gutter: fixed (16-24px typically).

**Flexible Grid (CSS Grid / Auto-layout)**
- Content-driven rather than column-driven. Areas sized by content.
- Use for dashboard mosaics or layouts where content determines size.
- Define min/max constraints per area rather than fixed column spans.

**Single Column with Max-Width**
- No grid. Content flows vertically, centered, with a max-width (600-800px for text,
  1200-1400px for mixed content).
- Use for reading-heavy or mobile-first layouts.

### Grid Tokens to Define

For each option, specify these values (they feed into the design-tokens.json later):

| Token | Description | Typical Values |
|---|---|---|
| `columns` | Number of grid columns | 12, 8, or flexible |
| `gutter` | Space between columns | 16px, 20px, 24px |
| `margin` | Page edge margin | 16px (mobile), 24px (tablet), 32px+ (desktop) |
| `max-width` | Maximum content width | 1200px, 1440px, or none |
| `sidebar-width` | Width of locked nav sidebar (if applicable) | 240px, 280px, 64px (collapsed) |

---

## 4. Spatial Relationships

### Proportional Splits

When dividing a screen into content areas, use meaningful proportions. Common splits:

| Split | Ratio | Use When |
|---|---|---|
| **70/30** | Primary / secondary | Main content with supporting sidebar or panel |
| **60/40** | Balanced emphasis | Split pane, comparison views, master-detail |
| **50/50** | Equal weight | True side-by-side comparison, dual-pane editor |
| **75/25** | Dominant + auxiliary | Content with narrow tools or filters panel |
| **Full width** | 100% | Single-column, mobile, or immersive content |

### Fixed vs Fluid

Determine which content areas are fixed-width and which are fluid:

- **Fixed:** Nav sidebar, tool panels, auxiliary areas with controls. These maintain their
  width as the viewport resizes.
- **Fluid:** Primary content area, card grids, text content. These expand or reflow to
  fill available space.
- **Hybrid:** Sidebar is fixed, content is fluid. Content has a max-width, remaining
  space is distributed as margin.

### Responsive Behavior

Define how the layout adapts across viewport sizes. Specify breakpoint behavior:

| Viewport | Behavior |
|---|---|
| **Desktop (1200px+)** | Full layout. All columns visible. Sidebar expanded. |
| **Tablet (768-1199px)** | Sidebar collapsed to rail or hidden. Content fills width. Secondary areas may stack below primary. |
| **Mobile (<768px)** | Single column. All areas stack vertically. Nav collapses to bottom tabs or hamburger. |

Note: if the brief specifies a single platform (desktop-only or mobile-only), only define
the relevant breakpoint behavior. Do not force responsive design where it is not needed.

---

## 5. Generate Option Artifacts

### Artifact Type: Layout Wireframes with Grid Overlay

Each option is a self-contained HTML file showing the layout applied to the product's
primary screens. Show the grid structure, content area positions, and proportional
relationships.

### What to Render

Each option file must contain:

1. **Grid overlay** -- show the column grid as a semi-transparent overlay on at least
   one representative screen. Columns, gutters, and margins visible.
2. **Primary screen layouts** -- for each primary screen in the inventory, show the
   layout wireframe with labeled content areas. Use the locked nav as the chrome frame.
3. **Proportional annotations** -- label the width ratios (70/30, 60/40) and key
   measurements (sidebar width, gutter size, max-width).
4. **Responsive variants** (if cross-platform) -- show how the layout adapts at tablet
   and mobile breakpoints. Stack the variants vertically for comparison.
5. **Layout consistency map** -- a summary showing which screens use which layout pattern.
   Highlight the dominant pattern and any exceptions.

### Wireframe Fidelity

Low-to-medium. Labeled rectangles for content areas. Gray backgrounds differentiate
primary, secondary, and auxiliary zones. No real content -- use placeholder text like
"[Main Content Area]", "[Sidebar Panel]", "[Filter Bar]". Show the grid lines, gutters,
and margin boundaries. No color, typography, or component detail.

### What the Layout Inherits from Navigation

The locked navigation defines fixed chrome. Do not redesign the nav. Show it as a locked
element:
- If sidebar nav: render it at its locked width on the left. Layout fills the remaining space.
- If top bar nav: render it at a fixed height at the top. Layout fills below.
- If tab bar nav: render it at the bottom (mobile). Layout fills above.

Label the nav area as "[Locked: Navigation]" to make the constraint visible.

---

## 6. Option Generation Strategy

### Round 1: Diverge on Spatial Philosophy

Generate 2-4 options that make different bets about spatial arrangement. Common axes of
variation:

**Dense vs Spacious**
- **Dense:** Tight gutters, more columns, smaller margins. Maximize information on screen.
  Power users, data-heavy products. Risk: feels cramped, hard to scan.
- **Spacious:** Wide gutters, generous margins, fewer columns per row. Focus and
  readability. Consumer products, reading-heavy apps. Risk: wastes space, requires scrolling.

**Structured vs Freeform**
- **Structured:** Strict grid. Every element aligns to columns. Predictable and uniform.
  Risk: feels rigid, hard to create visual interest.
- **Freeform:** Content-driven sizing. Areas sized by their content, not by a grid.
  Flexible and adaptive. Risk: inconsistency across screens, harder to maintain.

**Consistent vs Per-Screen**
- **Consistent:** Same layout pattern across all screens. Users learn one spatial model.
  Risk: forces different content into the same mold.
- **Per-Screen:** Each screen gets the layout best suited to its content. Richer but
  more design work. Risk: users re-learn the layout on each screen.

**Widescreen-Optimized vs Centered**
- **Widescreen:** Layout fills the full viewport width. Multiple columns at large sizes.
  Maximizes use of large monitors.
- **Centered:** Content has a max-width and centers in the viewport. Consistent experience
  across screen sizes. Wastes space on ultra-wide monitors but controls line length.

### Naming Options

Name each option by its defining spatial characteristic. Examples:
- "Tight Grid" (dense, structured, 12-column)
- "Breathing Room" (spacious, centered, generous margins)
- "Fluid Mosaic" (freeform, content-driven, dashboard-style)
- "Strict Columns" (structured, consistent pattern across all screens)
- "Adaptive Panes" (per-screen layout, split-pane dominant)

### Round 2+: Converge

Refine survivors based on user feedback. Common refinement moves:
- Adjust proportional splits based on content priority feedback.
- Tighten or loosen the grid based on density preferences.
- Standardize layout pattern across screens where feedback requested consistency.
- Adjust responsive breakpoints based on platform priority feedback.

---

## 7. Evaluation Criteria

When writing the critique and comparative take, evaluate each option against:

### Hierarchy Support
- Does the layout give the most space to the primary content area?
- Can the user identify the most important area on each screen at a glance?
- Do secondary and auxiliary areas stay subordinate without competing for attention?

### Grid Consistency
- Is the grid consistent across screens? (Same column count, gutter, margins.)
- Do exceptions to the grid have a clear rationale?
- Would a developer implementing this layout find a consistent spatial system?

### Responsive Degradation
- Does the layout degrade gracefully from desktop to tablet to mobile?
- Are content areas stacked in a logical reading order on narrow viewports?
- Does the mobile layout still feel designed, not just collapsed?

### Content Accommodation
- Can the layout handle variable content length? (Long titles, short titles, no content.)
- Does the grid work for both content-heavy screens (dashboards) and content-light
  screens (settings)?
- Are card grids responsive to card count? (1 card, 4 cards, 100 cards.)

### Navigation Integration
- Does the layout work harmoniously with the locked navigation?
- Is the transition between nav chrome and content area clean?
- Does the content area feel right-sized given the nav's footprint?

### Brief Alignment
- Does the layout match the platform's conventions (desktop = multi-column, mobile = single)?
- Does it support the content types mentioned in the brief (data, text, media)?
- Does the density match the user context (power users = denser, casual = sparser)?

---

## 8. Common Patterns to Reference

Surface these in the critique when relevant:

| Pattern | Strength | Weakness |
|---|---|---|
| **12-column grid** | Maximum flexibility, industry standard | Overkill for simple layouts, requires discipline |
| **Holy Grail** | Familiar, accommodates many content types | Right sidebar often underused, complex to implement responsively |
| **Master-Detail** | Efficient for entity-list products | Detail pane constrains content width on smaller screens |
| **Card Grid** | Flexible item count, responsive reflow | Cards compete equally for attention -- no hierarchy without sizing |
| **Single Column** | Simple, excellent readability, trivially responsive | Wastes horizontal space on desktop, requires long scroll |
| **Split Pane** | Great for comparison and side-by-side editing | Both panes fight for width, awkward on mobile |
| **Dashboard Mosaic** | Rich information display, customizable | Hard to make responsive, widget sizing is complex |

---

## 9. Crit Loop Execution

Follow `../../reference/crit-loop.md` exactly:

1. Generate option HTML files in `.design-crit/facets/content-layout/`.
2. Write `critique.md` with per-option rationale and a comparative take.
3. Generate `compare.html` with side-by-side option cards.
4. Open `compare.html` in the user's browser.
5. Wait for feedback. Process per round-trip mechanics.

### State Updates

After locking, record the decision in `state.json`. The locked layout becomes a constraint
for all visual facets downstream. Announce: "Layout locked as [option name]. [grid summary].
This defines the spatial structure for content hierarchy, typography, and all visual facets."

### Dark Mode Toggle

Omit the dark mode toggle for this facet. Layout wireframes are structural.

### Constraint Propagation

The locked layout defines these downstream constraints:
- **Content hierarchy:** knows the spatial zones it must prioritize content within.
- **Typography:** knows the column widths that determine line length and type scale.
- **Color system:** knows the surface areas that need background and border treatments.
- **Density/spacing:** knows the grid gutters and margins as a baseline.
- **Component design:** knows the available width for forms, cards, tables.

State these propagation effects when handing off. Example: "Content area is 840px at
desktop. Typography should target 60-75 characters per line at that width."

If the user's feedback on a later facet implies changing the layout (e.g., "the sidebar
content panel is too narrow for this table"), flag it: "That may require adjusting the
layout proportions. Want to revisit layout, or adapt the component to fit the current
space?"

### Layout Token Extraction

When locking, extract layout tokens for `design-tokens.json`:

```json
{
  "layout": {
    "columns": 12,
    "gutter": "20px",
    "margin-desktop": "32px",
    "margin-tablet": "24px",
    "margin-mobile": "16px",
    "max-width": "1440px",
    "sidebar-width": "260px",
    "content-max-width": "1140px"
  }
}
```

Record these in the critique so the design-direction skill can extract them later.
