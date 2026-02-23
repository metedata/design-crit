---
name: crit-screen-inventory
description: Use when identifying and evaluating the complete set of screens, states, and user flows for an app or service. Usually the first facet in a design crit.
---

# Screen Inventory & Flows

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer inventorying the complete set of screens, states, and user flows for this
product. This is almost always the first facet -- everything downstream depends on knowing
what surfaces exist. No wireframe fidelity here. The artifact is structural: flow diagrams,
screen maps, and state inventories rendered as annotated HTML.

Your job: derive the screen set from the brief, map the flows between them, enumerate
states per screen, and present 2-4 options that make different bets about how much surface
area the product needs.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current project state, round number, prior feedback
2. `.design-crit/brief.md` -- the confirmed design brief
3. `reference/crit-loop.md` -- shared crit loop mechanics (compare view, feedback, convergence)
4. `reference/design-principles.md` -- option generation principles, persona guidance

If this is round 2+, also read:
5. `.design-crit/facets/screen-inventory/feedback-round-{N-1}.json` -- prior round feedback
6. Surviving option files in `.design-crit/facets/screen-inventory/`

---

## 2. Derive Screens from the Brief

### Extract the Core Loop

Read the brief's "Core Interaction Loop" field. The core loop is the spine of the screen
inventory. Every screen either supports the core loop directly or supports a secondary task
that enables the core loop.

Example: If the core loop is "capture a thought, tag it, find it later," the primary screens
are: capture surface, browse/search surface, detail view. Everything else is secondary.

### Screen Derivation Checklist

Walk through the brief systematically and extract screens from these sources:

| Source | What to Extract |
|---|---|
| Core loop | Primary task screens (one per verb in the loop) |
| Scope (in v1) | Feature screens (each scoped feature implies 1+ screens) |
| Target users | Role-specific views if multiple user types exist |
| Platform | Platform-specific surfaces (mobile tab views, desktop sidebars) |
| Differentiator | Any surfaces unique to this product's value proposition |
| Existing codebase | Routes, pages, or screens already in the code (from brief harvesting) |

### Screen Classification

Classify each derived screen into one of these categories:

- **Primary** -- directly supports the core loop. Users visit every session.
- **Secondary** -- supports a non-core task that most users need (settings, profile, onboarding).
- **Tertiary** -- edge-case or admin surfaces (error pages, empty states, modals).

Do not include tertiary screens in the inventory yet. Edge states are a separate facet.
Focus on primary and secondary screens only.

---

## 3. Enumerate States per Screen

For every primary and secondary screen, enumerate the states it can be in. This is a
structural checklist, not a visual design exercise. Each state is a row in a table, not a
wireframe.

### State Enumeration Checklist

Apply this checklist to EVERY screen:

| State | Description | Include? |
|---|---|---|
| **Default** | Screen with typical content loaded, normal usage | Always |
| **Empty (first use)** | User has never added content to this screen | If user-generated content |
| **Empty (cleared)** | Content existed but was removed/filtered to zero | If deletable/filterable |
| **Empty (no results)** | Search or filter returned nothing | If searchable/filterable |
| **Loading** | Content is being fetched or computed | If async data |
| **Error** | Something failed (network, permission, validation) | Always |
| **Partial** | Some content loaded, some pending or failed | If multiple data sources |
| **Overflow** | More content than expected (long lists, deep nesting) | If variable-length content |
| **Permission-restricted** | User lacks access to this screen or its content | If role-based access |

Note: Do NOT wireframe these states in this facet. List them. The `crit-edge-states` facet
handles the visual design of non-happy-path states.

---

## 4. Map User Flows

### Happy Path Flow

Map the primary user flow from entry to core loop completion. This is the "golden path" --
the fewest screens a user touches to accomplish the core task.

Format: a linear or branching sequence of screens with labeled transitions.

```
[Entry] -> [Screen A] --(action)--> [Screen B] --(action)--> [Screen C] -> [Success]
```

Count the steps. Fewer is better for the core loop. If the happy path exceeds 5 screens
for a simple task, flag it: "Core loop requires N screens -- consider whether steps M and
N can be combined."

### Error Paths

For each screen in the happy path, identify what can go wrong and where the user ends up:

- Validation failure -> stays on same screen with error state
- Network error -> error state or retry screen
- Permission denied -> redirect or blocked state

### Edge Paths

Map secondary flows that diverge from the happy path:

- First-time user flow (onboarding -> core loop)
- Settings or configuration flow
- Search/browse flow (alternative to the core loop's default path)
- Destructive action flow (delete, cancel, deactivate -> confirmation -> result)

---

## 5. Generate Option Artifacts

### Artifact Type: Screen Map HTML

Each option is a self-contained HTML file rendering a screen map -- NOT wireframes. The
screen map is a flow diagram showing all screens as labeled boxes with arrows indicating
navigation paths.

### What to Render

Each option file must contain:

1. **Screen map diagram** -- boxes for each screen, grouped by classification (primary,
   secondary). Use CSS grid or flexbox for layout. Draw connections with SVG lines or
   CSS borders to indicate flow.
2. **Screen list table** -- every screen listed with: name, classification (primary/
   secondary), states (from the enumeration checklist), and a one-line purpose.
3. **Flow paths** -- the happy path highlighted (bold or color), error and edge paths
   shown in secondary styling.
4. **Screen count summary** -- "N primary screens, M secondary screens, P total states."

### Visual Style for Screen Maps

- Use simple rectangles with labels for screens. No wireframe-level detail.
- Color-code by classification: primary screens get a solid border, secondary get a
  dashed border.
- Flow arrows labeled with the user action that triggers the transition.
- Group screens into zones if the product has distinct areas (e.g., "content zone,"
  "admin zone," "onboarding zone").

### What NOT to Include

- No wireframe layouts, no content placeholders, no UI components.
- No visual design (color, typography, icons). This is structural only.
- No interaction details beyond "which screen do you land on."

---

## 6. Option Generation Strategy

### Round 1: Diverge on Scope and Structure

Generate 2-4 options that make different bets about the product's surface area. Common
axes of variation for screen inventory:

**Minimal MVP vs Full Suite**
- **Minimal:** Fewest screens possible. Combine related tasks onto single screens. Favor
  modals and panels over dedicated pages. Trade discoverability for simplicity.
- **Full suite:** Dedicated screen for each task. Clear separation of concerns. More
  screens but each is simpler. Trade simplicity for discoverability.

**Flat vs Deep**
- **Flat:** All primary screens accessible from one level. Wide navigation. Works for
  products with 3-7 primary screens.
- **Deep:** Hierarchical screen structure with drill-down. Parent screens lead to detail
  screens. Works for products with many entities or content types.

**Task-Centric vs Entity-Centric**
- **Task-centric:** Screens organized around verbs (Create, Browse, Analyze). User picks
  a task first, then sees relevant content.
- **Entity-centric:** Screens organized around nouns (Projects, Documents, People). User
  picks an entity first, then acts on it.

**Single-Window vs Multi-Surface**
- **Single-window:** All interaction within one window/tab. Panels, drawers, modals for
  secondary tasks. Compact.
- **Multi-surface:** Separate pages/views for distinct tasks. More navigation but less
  cognitive load per screen.

### Naming Options

Name each option by its defining characteristic. Examples:
- "Lean Three-Screen" (minimal MVP)
- "Full Dashboard Suite" (comprehensive)
- "Entity Hub" (entity-centric)
- "Task Launcher" (task-centric)
- "Single Canvas" (single-window with panels)

### Round 2+: Converge

Refine surviving options based on user feedback. Common refinement moves:

- Merge two options: take the screen set from one and the flow structure from another.
- Split or combine screens based on user feedback about scope.
- Add or remove secondary screens based on scope clarification.
- Adjust flow paths based on happy-path feedback.

---

## 7. Evaluation Criteria

When writing the critique (critique.md) and the comparative take, evaluate each option
against these criteria:

### Completeness
- Does the screen set cover every task in the brief's v1 scope?
- Are there tasks mentioned in the brief with no corresponding screen?
- Are all user roles from the brief served?

### Necessity
- Does every screen earn its place? Can any two screens be merged without losing clarity?
- Are there screens that serve edge cases better handled as states of existing screens?
- Apply the "remove one screen" test: if you removed this screen, would users notice?

### Flow Efficiency
- How many screens does the core loop require? (Fewer = better for primary tasks.)
- Are there dead ends -- screens with no clear next step?
- Can the user always get back to the core loop from any screen?

### Scalability
- If v2 adds 3 more features, does this screen structure accommodate them?
- Does the screen set avoid painting the navigation model into a corner?

### Brief Alignment
- Does the option reflect the brief's differentiator?
- Does the option match the platform constraints (mobile vs desktop vs responsive)?
- Does the option respect scope boundaries (no screens for out-of-scope features)?

---

## 8. Common Patterns and Trade-offs

Reference these patterns when explaining options to the user:

| Pattern | When It Works | Watch Out For |
|---|---|---|
| **List-Detail** | Entity-heavy apps (email, CRM, docs) | Feels generic; detail view carries too much weight |
| **Dashboard + Drill-down** | Data products, admin panels | Dashboard becomes a dumping ground; drill-down depth unclear |
| **Wizard / Stepped** | Complex creation flows (onboarding, multi-step forms) | Feels slow for repeat users; hard to resume mid-flow |
| **Canvas + Panels** | Creative tools, editors, maps | Panel management becomes its own UX problem |
| **Hub-and-Spoke** | Utility apps with independent features | Hub screen becomes shallow; spokes feel disconnected |

---

## 9. Crit Loop Execution

Follow the crit loop mechanics from `reference/crit-loop.md` exactly:

1. Generate option HTML files in `.design-crit/facets/screen-inventory/`.
2. Write `critique.md` with per-option rationale and a comparative take.
3. Generate `compare.html` with side-by-side option cards.
4. Open `compare.html` in the user's browser.
5. Wait for feedback. Process it per the round-trip mechanics.

### State Updates

After generating options for round 1:
- Set the facet status to `"in-progress"` in `state.json`.
- Set `current_round` to the current round number.

After locking:
- Record `locked_option`, `locked_summary`, `decided_by`, `decision_rationale`,
  `rounds_completed` in the facet entry in `state.json`.
- Set facet status to `"locked"`.
- The locked screen inventory becomes the canonical surface list for ALL downstream facets.

### Constraint Propagation Note

Screen inventory is typically the first facet, so there are no prior locked decisions to
inherit. However, if the facet order was customized and prior facets exist, read all locked
decisions before generating options.

After locking, announce to the user: "Screen inventory locked as [option name]. This defines
the surface set for all remaining facets. [N] primary screens, [M] secondary screens."

---

## 10. Special Considerations

### Dark Mode Toggle

Omit the dark mode toggle for this facet. Screen maps are structural diagrams, not visual
designs. Dark mode is irrelevant.

### Scope Creep Detection

If the user's feedback asks for screens that serve features listed as "out of v1" in the
brief, flag it: "That screen serves [feature], which the brief lists as out of v1 scope.
Want to add it to scope, or keep the inventory focused on v1?"

### Codebase Cross-Reference

If the brief mentions existing routes or screens from the codebase, cross-reference the
proposed inventory against them. Note any screens in the code that are absent from the
inventory, and any inventory screens that do not yet exist in the code.
