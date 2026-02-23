---
name: crit-navigation
description: Use when evaluating navigation patterns, wayfinding, menu structures, and how users move between screens in an app.
---

# Navigation Model

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer deciding how users move between screens. Navigation is the skeleton of
the product -- it determines what users can reach, how quickly they can reach it, and
whether they know where they are at any moment. A good navigation model makes a complex
product feel simple. A bad one makes a simple product feel confusing.

Your job: take the locked screen inventory, evaluate how many destinations need to be
reachable, and present 2-4 navigation patterns that make different bets about discoverability,
efficiency, and scalability.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current project state, round number, prior feedback
2. `.design-crit/brief.md` -- the confirmed design brief
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles
5. `.design-crit/facets/screen-inventory/{locked_option}.html` -- the locked screen inventory

Extract from the screen inventory:
- Total primary screen count (determines nav complexity)
- Total secondary screen count (determines what goes in secondary nav)
- Flow structure (flat vs hierarchical, hub-and-spoke vs sequential)
- Screen groups (if screens cluster into categories)

If prior facets are locked (e.g., edge-states), read those too. State this in the critique:
"Building on the locked [facet]: [summary]."

If this is round 2+, also read:
6. `.design-crit/facets/navigation-model/feedback-round-{N-1}.json`
7. Surviving option files in `.design-crit/facets/navigation-model/`

---

## 2. Navigation Analysis

### Destination Count

Count the primary destinations -- screens that must be reachable from the top-level
navigation. This number drives the pattern choice:

| Primary Destinations | Guidance |
|---|---|
| 2-3 | Simple nav. Tab bar, segmented control, or minimal sidebar. |
| 4-5 | Standard nav. Tab bar (mobile), sidebar (desktop), or top nav (web). |
| 6-8 | Complex nav. Sidebar with groups, or rail + flyout. Tab bar hits its limit at 5. |
| 9+ | Deep nav. Collapsible sidebar, command palette, or multi-level hierarchy required. |

### Secondary Destinations

Identify screens that do NOT belong in primary nav but must be reachable:
- Settings, preferences, account management
- Help, documentation, support
- Admin or configuration screens
- Contextual screens (detail views reached from list screens)

These go in secondary nav surfaces: user menu, settings gear, contextual menus, breadcrumbs.

### Platform Conventions

Read the platform from the brief. Respect platform conventions unless there is a strong
reason to deviate:

**iOS:**
- Bottom tab bar (up to 5 items). Standard pattern for top-level nav.
- Navigation stack with back button for drill-down.
- Tab bar persists across the app. Do not hide it on drill-down screens.

**Android:**
- Bottom navigation bar (3-5 items) or navigation drawer (hamburger menu).
- Material 3 favors bottom nav for primary, drawer for extended.
- Navigation rail for tablets (vertical icon strip on the left).

**Desktop Web:**
- Sidebar navigation (persistent, collapsible). Standard for tools and dashboards.
- Top navigation bar for marketing sites and content platforms.
- Hybrid: top bar for global nav + sidebar for section-level nav.

**Cross-Platform:**
- Responsive navigation that adapts: sidebar on desktop, bottom tabs on mobile.
- Define breakpoint behavior explicitly. At what width does the sidebar collapse?

---

## 3. Navigation Vocabulary

Use these terms consistently in options and critique:

| Pattern | Description | Best For |
|---|---|---|
| **Sidebar** | Persistent vertical panel on the left. Shows labels + optional icons. Collapsible to icon-only rail. | Desktop tools, dashboards, 6+ destinations |
| **Top bar** | Horizontal bar at the top. Logo + nav items + actions. | Marketing sites, content platforms, 3-5 destinations |
| **Tab bar** | Fixed bar at the bottom (mobile) or top (web). Icon + label per tab. | Mobile apps, 3-5 primary destinations |
| **Rail** | Narrow vertical strip with icons only. Expands to sidebar on hover or click. | Desktop tools, space-constrained layouts |
| **Hamburger menu** | Hidden behind a menu icon. Reveals full nav on tap/click. | Mobile with 6+ destinations, or secondary nav |
| **Breadcrumbs** | Horizontal path trail showing hierarchy. Clickable segments. | Deep hierarchies, content management, e-commerce |
| **Command palette** | Keyboard-triggered search overlay (Cmd+K). Type to navigate anywhere. | Developer tools, power-user products |
| **Segmented control** | Inline switcher for 2-3 related views on the same screen. | View mode switching (list/grid, day/week/month) |
| **Contextual menu** | Appears on right-click or long-press. Screen-specific actions. | Secondary actions, power-user shortcuts |

---

## 4. Generate Option Artifacts

### Artifact Type: Navigation Wireframes

Each option is a self-contained HTML file showing the navigation pattern applied to the
product's screens. Wireframe fidelity -- enough to see the nav structure and how it works
across different screens.

### What to Render

Each option file must contain:

1. **Navigation wireframe** -- the full-page layout showing the nav element and a
   representative content area. Show the nav in its default state (expanded) and, if
   applicable, its collapsed state.
2. **Screen-by-screen nav state** -- for each primary screen, show how the nav looks
   when that screen is active. Highlight the current item. Show breadcrumbs or back
   buttons if applicable.
3. **Secondary nav** -- where do settings, help, user menu, and other secondary
   destinations live? Show their access points.
4. **Mobile adaptation** (if cross-platform) -- how the nav transforms on small screens.
   Show the mobile variant alongside the desktop variant.
5. **Reachability summary** -- a table showing how many taps/clicks it takes to reach
   each primary and secondary screen from any other primary screen.

### Wireframe Fidelity

Low-to-medium. Gray boxes for content areas, labeled rectangles for nav items, simple
icons (or icon placeholders) for nav elements. Enough to communicate the pattern, not
the visual polish. No color beyond gray/black/white. No real icons -- describe them with
text labels inside placeholder squares.

---

## 5. Option Generation Strategy

### Round 1: Diverge on Pattern and Philosophy

Generate 2-4 options that make different bets about navigation. Common axes of variation:

**Depth vs Breadth**
- **Broad/Flat:** All primary destinations visible at once. Tab bar or expanded sidebar.
  Instant access. Screen count limited by visible space.
- **Deep/Hierarchical:** Nested navigation. Top-level categories expand to reveal
  sub-items. Scales to many screens. Requires more clicks to reach deep items.

**Persistent vs Contextual**
- **Persistent:** Nav always visible. Sidebar or tab bar that never hides. User always
  knows where they are and what is available. Costs screen real estate.
- **Contextual:** Nav appears when needed (hamburger, command palette, swipe gesture).
  Maximizes content space. User must recall how to access nav.

**Labeled vs Iconic**
- **Labeled:** Every nav item has a text label. Self-documenting. Takes more space.
- **Iconic:** Icons only (with tooltips on hover). Compact. Requires learning period.
  Only works if icons are universally recognizable.
- **Hybrid:** Icons with labels on primary items, icons-only on secondary. Balances
  space and clarity.

**Global vs Sectioned**
- **Global:** One nav serves the entire app. Simple mental model. Can get long for
  complex products.
- **Sectioned:** Different areas of the app have their own nav (e.g., global top bar +
  section-specific sidebar). Scales well. Risk: user loses track of which level they are in.

### Naming Options

Name each option by its primary pattern. Examples:
- "Deep Sidebar" (persistent sidebar with collapsible groups)
- "Lean Tab Bar" (5-item bottom tab bar, mobile-first)
- "Cmd+K Navigator" (minimal chrome, command palette for power users)
- "Rails & Flyouts" (icon rail that expands to flyout panels)
- "Top Bar + Breadcrumbs" (horizontal nav with hierarchical breadcrumbs)

### Round 2+: Converge

Refine survivors based on user feedback. Common refinement moves:
- Combine primary nav from one option with secondary nav from another.
- Adjust item grouping or ordering based on user priority feedback.
- Add keyboard shortcuts or command palette as supplementary nav.
- Refine mobile adaptation based on platform feedback.

---

## 6. Evaluation Criteria

When writing the critique and comparative take, evaluate each option against:

### Reachability
- Can the user reach any primary screen in N taps or fewer? (Target: 1-2 for primary, 2-3
  for secondary.)
- Are there screens that require 4+ taps to reach? Flag them.
- Does the command palette or search provide a shortcut for power users?

### Wayfinding
- Is the user's current location always clear? (Active state, breadcrumbs, highlighted item.)
- Can the user answer "where am I?" at any point without reading content?
- Does the nav communicate the app's structure at a glance?

### Scalability
- If v2 adds 3 more primary screens, does the nav accommodate them without redesign?
- Does the nav degrade gracefully as item count grows? (Sidebar scrolls, tab bar does not.)
- Can new sections or categories be added without restructuring?

### Screen Real Estate
- How much space does the nav consume? (Measure in approximate percentage of viewport.)
- Is the trade-off between nav visibility and content space appropriate for this product?
- On mobile, does the nav leave enough room for the primary task?

### Learnability
- Can a first-time user find the screen they need without instruction?
- Are nav labels clear and distinct? (Avoid ambiguous labels like "More" or "Tools".)
- Does the nav pattern match what users expect from this platform?

### Brief Alignment
- Does the nav pattern match the platform specified in the brief?
- Does it accommodate all user roles mentioned in the brief?
- Does it support the core interaction loop with minimal friction?

---

## 7. Common Trade-offs to Surface

Reference these when explaining options to the user. Name the trade-off explicitly.

| Trade-off | Lean Toward A When... | Lean Toward B When... |
|---|---|---|
| **Visibility vs Space** (persistent nav vs hidden nav) | Users are explorers who browse. Product has 6+ destinations. | Core task dominates. Mobile-first. Content needs full width. |
| **Flat vs Deep** (all items visible vs nested groups) | Fewer than 7 primary destinations. Users need fast switching. | 8+ destinations. Items naturally cluster into categories. |
| **Labels vs Icons** (text vs symbols) | New users, unfamiliar concepts, accessibility priority. | Repeat users, universally known icons, space constrained. |
| **Single nav vs Dual nav** (one nav level vs global+local) | Simple product, uniform screen types. | Complex product, distinct sections with different sub-navigation. |
| **Fixed vs Adaptive** (one nav for all sizes vs responsive breakpoints) | Single-platform product (desktop only or mobile only). | Cross-platform product. Brief specifies responsive. |

---

## 8. Crit Loop Execution

Follow `reference/crit-loop.md` exactly:

1. Generate option HTML files in `.design-crit/facets/navigation-model/`.
2. Write `critique.md` with per-option rationale and a comparative take.
3. Generate `compare.html` with side-by-side option cards.
4. Open `compare.html` in the user's browser.
5. Wait for feedback. Process per round-trip mechanics.

### State Updates

After locking, record the decision in `state.json`. Announce: "Navigation locked as
[option name]. [pattern summary]. This constrains all layout decisions downstream."

### Dark Mode Toggle

Omit the dark mode toggle for this facet. Navigation wireframes are structural.

### Constraint Propagation

The locked navigation model is a hard constraint for the layout facet. When generating
layout options later, the nav structure is fixed -- layout fills the remaining space.
State this clearly when handing off: "The layout facet will work within the [nav pattern]
structure. The sidebar is [width], leaving [remaining]px for content."

If the user's feedback on a later facet implies changing the navigation (e.g., "I want the
sidebar wider"), flag it: "That changes the navigation model. Want to revisit navigation,
or adjust the layout within the current nav?"
