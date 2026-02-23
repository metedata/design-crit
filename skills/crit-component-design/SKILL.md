---
name: crit-component-design
description: Use when evaluating interactive component patterns — buttons, forms, cards, modals, tables, and other UI building blocks.
---

# Component Design Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are critting component design -- the interactive building blocks that users touch, click, and read. Buttons, forms, cards, modals, tables, toggles, selectors, alerts. This facet defines the component vocabulary: what patterns exist, how they behave across states, and whether they form a consistent, learnable system.

Component design sits in the compositional lens. It depends on `content-layout` (components live within the grid) and `content-hierarchy` (components inherit emphasis levels). It feeds into sensory facets like `typography`, `color-system`, and `elevation-shape` which style the components.

---

## 1. Load Context

Before generating any options, read:

1. `.design-crit/state.json` -- current facet state, round number, locked decisions
2. `.design-crit/brief.md` -- project purpose, platform, tech stack, target users
3. `.design-crit/facets/component-design/feedback-round-{N-1}.json` -- prior feedback (skip for round 1)
4. All locked option files from prior facets -- especially `content-layout` (the grid), `content-hierarchy` (emphasis levels), and `navigation-model` (nav components already decided)

Check the brief for an existing component library (shadcn, MUI, Ant Design, Radix, etc.). If one is specified, treat it as a constraint: design within its patterns, not against them. Note which components it already provides and which you need to design custom.

State what you are building on: "Building on the locked layout and hierarchy. Platform is [platform]. Component library: [library or none]. Designing components that fit within this structure."

---

## 2. Artifact Type

Generate **component wireframes with interaction state diagrams**.

Each option file (`option-{x}.html`) must contain:

### Component Inventory

A comprehensive inventory of all components needed across the screens identified in the screen inventory. Organize by category:

**Input Components** -- elements users interact with to provide data:
- Buttons (primary, secondary, tertiary, icon-only, grouped)
- Text inputs (single-line, multi-line, search)
- Selectors (dropdown, radio, checkbox, toggle, segmented control)
- Date/time pickers
- File uploads
- Sliders and range inputs

**Display Components** -- elements that present information:
- Cards (content cards, list items, stat cards)
- Tables (data tables, list views, comparison tables)
- Lists (simple, complex with actions, nested)
- Badges, tags, and chips
- Avatars and thumbnails
- Empty states and placeholders

**Feedback Components** -- elements that communicate system status:
- Toast notifications
- Inline alerts (info, success, warning, error)
- Progress indicators (bar, spinner, skeleton)
- Confirmation dialogs
- Validation messages

**Overlay Components** -- elements that layer above the main content:
- Modals (small, medium, full-screen)
- Drawers (side panels)
- Popovers and tooltips
- Dropdown menus
- Command palettes

Not every project needs every component. Include only what the screens require. Annotate each component with which screens use it.

### State Diagrams

For each component in the inventory, show its interactive states. Render states side by side in a horizontal strip:

| State | When | Visual Treatment |
|---|---|---|
| Default | Resting, no interaction | Base appearance |
| Hover | Cursor over (desktop) | Subtle elevation or color shift |
| Active/Pressed | Mouse down or touch | Compressed or darkened |
| Focused | Keyboard focus | Visible focus ring |
| Disabled | Interaction not available | Reduced opacity, no pointer |
| Loading | Async operation in progress | Spinner or skeleton replacement |
| Error | Validation or operation failed | Error color, message visible |
| Success | Operation completed | Confirmation indicator |

Not every component needs every state. Buttons need default/hover/active/disabled/loading. Inputs need default/focused/filled/disabled/error. Cards may only need default/hover/selected. Show the states that apply.

### Component Specimens

Render each component at actual wireframe size within a specimen block. Include:

- The component in its default state, large enough to read
- Dimension annotations (height, padding, minimum width)
- Touch target indicators for mobile (minimum 44x44px)
- Spacing between related components (button groups, form field stacks)

Use grayscale. This facet is about structure and interaction, not color.

---

## 3. Evaluation Criteria

Score each option against these questions. State your assessment in the critique.

### State Coverage

Are all necessary interaction states defined for each component? Check for gaps:

- Does every clickable element have a hover state (desktop)?
- Does every focusable element have a visible focus indicator?
- Do form inputs show error, disabled, and filled states?
- Do async actions show loading states?
- Are empty states defined for lists, tables, and data displays?

Missing states are not minor polish -- they are unfinished interaction design.

### Platform Conventions

Do the components follow the conventions of the target platform?

- **Web:** Standard form controls, expected keyboard shortcuts, hover states for desktop, focus management for accessibility.
- **iOS:** Native-feeling controls (toggle switches not checkboxes for on/off, action sheets not dropdowns for mobile actions, swipe gestures for list items).
- **Android:** Material-influenced patterns (FABs, bottom sheets, snackbars), back button behavior, system navigation bar respect.
- **Cross-platform:** Identify which components should feel native per platform vs which should be consistent across platforms.

### Consistency

Does the component inventory form a coherent system?

- Are similar components treated similarly? (All buttons share the same height, all cards share the same radius, all inputs share the same padding.)
- Is the component hierarchy clear? (Primary button is obviously more important than secondary. Card types are distinguishable.)
- Are spacing values consistent? (Same padding inside all components of the same type.)
- Does the system use a small, reusable set of patterns rather than one-off solutions?

### Affordance

Do components look interactive when they should and static when they should?

- Can users tell what is clickable without instructions?
- Do buttons look like buttons? Do links look like links?
- Are non-interactive elements clearly static?
- Do disabled states clearly communicate "not available" without looking broken?

---

## 4. Component Design Patterns

Reference these patterns when generating and evaluating options.

### Button Hierarchy

Every project needs a clear button system. Define at minimum:

- **Primary:** The main action. One per screen section. High contrast, filled.
- **Secondary:** Supporting actions. Can coexist with primary. Lower contrast, outlined or lighter fill.
- **Tertiary/Ghost:** Low-priority actions (cancel, dismiss, "learn more"). Text-only or minimal border.
- **Destructive:** Delete, remove, revoke. Shares primary prominence but uses a warning treatment.
- **Icon-only:** Compact actions (close, menu, settings). Must have accessible labels.

Common mistake: too many button styles. Three to four levels cover most projects.

### Form Design Patterns

Forms are where most users struggle. Define the approach:

- **Label placement:** Top-aligned (fastest scan, best for mobile), left-aligned (compact, good for desktop forms with short labels), or floating/inline.
- **Validation timing:** On blur (validates when leaving a field), on submit (all at once), or real-time (as typing). On blur is the best default.
- **Error display:** Inline below the field (best for specific errors), top-of-form summary (best for form-level errors), or both.
- **Field grouping:** Related fields grouped with a section heading. Logical tab order. Progressive disclosure for optional fields.
- **Required indicators:** Asterisk on required fields (convention) or "(optional)" on optional fields (better when most fields are required).

### Card Patterns

Cards are the most versatile display component. Distinguish:

- **Content cards:** Preview of content with a link to detail view. Typically image + title + summary + metadata.
- **Action cards:** Represent a task or choice. Typically icon + label + description + action button.
- **Stat cards:** Single key metric with label and optional trend indicator.
- **List-item cards:** Row in a collection. Consistent height, scannable structure.

Define whether cards are clickable (the entire card is a link) or contain discrete clickable elements. Not both -- mixed click targets confuse users.

### Table Patterns

Tables are critical for data-heavy products. Define:

- **Column behavior:** Fixed vs auto-width, sortable columns, resizable columns
- **Row interactions:** Hover highlight, row selection (single or multi), expandable rows, inline editing
- **Density modes:** Compact (for power users scanning large datasets), comfortable (default), spacious (for touch or accessibility)
- **Responsive behavior:** Horizontal scroll, column priority hiding, card-view transformation on mobile
- **Empty and loading states:** Skeleton rows during load, meaningful empty state with action prompt

### Overlay Patterns

Define how overlays work as a system:

- **Modals:** When to use (confirmations, focused tasks, creation flows). Size variants. Dismiss behavior (click outside, ESC key, explicit close). Scroll behavior for long content.
- **Drawers:** When to use (detail panels, filters, settings). Direction (right is most common). Width. Push vs overlay behavior.
- **Popovers:** When to use (contextual actions, quick info). Trigger (click vs hover). Positioning (auto-flip to stay in viewport). Dismiss behavior.
- **Tooltips:** When to use (icon labels, truncated text, supplementary info). Delay. Max width. Never for essential information.

---

## 5. Common Component Trade-offs

Present these trade-offs when they apply.

### Custom vs Platform-Native Controls

- **Custom:** Consistent across platforms, full design control, brand-aligned. Risk: breaks user expectations, accessibility gaps, higher implementation cost.
- **Platform-native:** Familiar, accessible by default, lower implementation cost. Risk: inconsistent cross-platform, limited design control.
- **Question to ask:** "Is this component's behavior standard enough that native controls cover it, or does your product need something custom?"

### Component Density

- **Dense components:** Compact padding, smaller text, tighter spacing. More on screen, efficient for experts. Risk: hard to tap on mobile, cluttered for new users.
- **Spacious components:** Generous padding, larger targets, more whitespace. Comfortable, accessible, approachable. Risk: less information on screen, more scrolling.
- **Question to ask:** "Are your users power users who want to see everything, or occasional users who need comfort?"

### Inline vs Overlay Interactions

- **Inline:** Edit-in-place, expandable rows, inline forms. Keeps context visible, reduces disorientation. Risk: layout shifts, complex state management.
- **Overlay:** Modals, drawers, popovers. Clean main view, focused sub-tasks. Risk: context loss, modal fatigue, mobile awkwardness.
- **Question to ask:** "Does the user need to reference the main content while performing this action?"

### Prescriptive vs Flexible Components

- **Prescriptive:** One button style, one card style, one table style. Simple to learn, consistent, fast to build. Risk: does not cover edge cases.
- **Flexible:** Multiple variants per component (sizes, densities, styles). Covers more use cases. Risk: inconsistent usage, decision fatigue for developers.
- **Question to ask:** "How many different contexts will this component appear in?"

---

## 6. Option Generation Guidance

### Round 1: Diverge

Generate 2-4 options exploring different component philosophies. Each option should represent a distinct approach to the component system as a whole, not just different button styles.

Useful axes of variation:

- **Minimal vs comprehensive:** One option defines only the 8-10 essential components with tight constraints. Another defines 20+ components covering every identified need.
- **Dense vs spacious:** One option optimizes for information density (compact inputs, tight tables). Another optimizes for comfort (generous padding, large targets).
- **Platform-native vs custom:** One option leans heavily on platform conventions (native selects, system dialogs). Another defines a custom design language for all interactive elements.
- **Flat vs layered:** One option uses flat, border-based component styling. Another uses elevation and depth to distinguish interactive elements.

Name each option by its philosophy: "Lean Toolkit," "Full Component Library," "Touch-Friendly Spacious," "Dense Pro Dashboard."

### Round 2: Converge

Refine based on feedback. Typical refinements:

- Adjust state coverage for components the user flagged
- Merge the button system from one option with the form approach of another
- Add missing components identified during review
- Resolve consistency issues between component types

### Round 3+: Polish

Focus on: state coverage completeness, spacing consistency across all components, responsive behavior definitions, and ensuring the component system works across all primary screens.

---

## 7. Per-Option Critique Checklist

When writing the critique for each option in `critique.md`, address every item:

1. How many components are defined? Is that sufficient for the identified screens?
2. Are all necessary states covered for each interactive component?
3. Do the components follow platform conventions for the target platform?
4. Is the component system internally consistent (shared dimensions, spacing, hierarchy)?
5. Do interactive elements have clear affordance?
6. How does this component vocabulary interact with the locked layout and hierarchy?
7. What is the implementation cost? (Minimal set = fast. Comprehensive set = slower but fewer ad-hoc decisions.)

---

## 8. Constraint Propagation

Component design decisions constrain downstream facets:

- **Typography** must work at the sizes defined in components. If buttons are 36px tall with 14px text, the type scale must include that level.
- **Color system** must provide states for all interactive components (hover, active, disabled, error colors).
- **Elevation-shape** must define the surface treatment for cards, modals, drawers, and other layered components.
- **Density-spacing** must align with the spacing values established inside components (padding, margins, gaps).
- **Accessibility** will audit keyboard navigation order, focus indicators, ARIA roles, and screen reader labels for every component defined here.

State these downstream implications in the critique: "This component system defines [N] interactive components. The color system will need [specific states]. Elevation will need to handle [specific layers]."

---

## 9. Locking

When the user locks a component design option:

1. Record the locked option in `state.json` with `locked_summary` describing the component philosophy (e.g., "Lean toolkit of 12 components with spacious density, platform-native forms, overlay pattern for detail views").
2. The locked component wireframes become the interactive pattern reference for all downstream facets.
3. Announce the lock and its downstream implications: "Component design locked as [name]. Typography, color, elevation, and density facets will style these components. Accessibility will audit their interactive behavior."
