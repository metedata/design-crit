---
name: facet-planning
description: Use when a design brief is confirmed and the project needs a plan for which design facets to evaluate and in what order.
---

# Facet Planning

You are a design lead planning a structured critique. The brief is confirmed. Your job: derive WHICH design facets this project needs and in WHAT order, then get the user's sign-off before any crit work begins.

Derive facets from first principles -- analyze the brief, identify what decisions matter for THIS project, and select from the pre-built library. Do not apply a fixed checklist. A note-taking app and a data dashboard need different facets in different orders.

---

## 1. Load References

Read these files before doing anything else:

1. `.design-crit/brief.md` -- the confirmed design brief
2. `.design-crit/state.json` -- current project state
3. `reference/design-principles.md` -- the facet library, three lenses framework, dependency ordering rules, and default pipeline order

The design-principles reference contains the full facet library with universal/contextual flags, the three lenses framework, and dependency ordering rules. Do not duplicate that content here. Read it every time.

---

## 2. Analyze the Brief

Read `.design-crit/brief.md` and extract these signals:

**Project type.** Identify the category: productivity app, data dashboard, marketing site, e-commerce, social platform, content platform, developer tool, mobile utility, or other. Project type determines which contextual facets apply.

**Platform.** Web, iOS, Android, desktop, cross-platform. Platform determines whether responsive design matters and which navigation conventions apply.

**Complexity indicators.** Count:
- Number of screens or pages mentioned (or implied by scope)
- Number of user roles or personas
- Data display needs (tables, charts, dashboards)
- Content types (text, media, user-generated, data)
- Interaction density (forms, editors, drag-and-drop, real-time)

**Scope signals.** What is in v1? What is out? A tightly scoped MVP needs fewer facets than a full product.

**Existing constraints.** Does the brief mention an existing design system, component library, brand guidelines, or tech stack that pre-decides certain facets? If shadcn/ui is specified, component-design may be lower priority. If a brand guide exists, color and typography may be partially pre-decided.

**Differentiator.** What makes this product distinct? The differentiator often reveals which facets deserve extra attention. A "beautiful reading experience" product promotes typography. A "data-first" tool promotes data-visualization.

---

## 3. Select Facets

Use the three lenses framework from `design-principles.md` to select facets.

### Universal Facets (consider for every project)

These 12 facets apply to nearly every project. Include them by default, then remove only with explicit reasoning:

| Facet ID | Skill | What It Decides |
|---|---|---|
| `screen-inventory` | `crit-screen-inventory` | What screens exist, user flows, state maps |
| `edge-states` | `crit-edge-states` | Empty, error, offline, loading states |
| `navigation-model` | `crit-navigation` | Nav patterns, wayfinding, menu structures |
| `content-layout` | `crit-layout` | Grid, spatial composition, content areas |
| `content-hierarchy` | `crit-content-hierarchy` | Information priority, emphasis, reading order |
| `component-design` | `crit-component-design` | Buttons, forms, cards, modals, tables |
| `elevation-shape` | `crit-elevation-shape` | Z-axis, shadows, surface treatments, radius |
| `typography` | `crit-typography` | Font systems, type scale, readability |
| `color-system` | `crit-color-system` | Palettes, themes, contrast, dark mode |
| `density-spacing` | `crit-density-spacing` | Information density, whitespace, spacing scale |
| `content-voice` | `crit-content-voice` | Microcopy, error messages, tone, terminology |
| `accessibility` | `crit-accessibility` | Keyboard, screen readers, contrast, ARIA |

### Contextual Facets (include based on project type)

These 7 facets apply only when the project warrants them:

| Facet ID | Skill | Include When |
|---|---|---|
| `onboarding-flows` | `crit-onboarding-flows` | Consumer apps, complex tools, products with learning curve |
| `data-visualization` | `crit-data-visualization` | Dashboards, analytics, data-heavy products |
| `responsive` | `crit-responsive` | Cross-device products, mobile+desktop |
| `iconography` | `crit-iconography` | Apps with rich icon-driven UI, custom icon needs |
| `motion` | `crit-motion` | Polish-focused products, marketing sites, transitions-heavy UI |
| `imagery-illustration` | `crit-imagery-illustration` | Marketing sites, consumer apps, brand-heavy products |
| `loading-performance` | `crit-loading-performance` | Async-heavy apps, data dashboards, slow-network contexts |

### Custom Facets

If the brief reveals a design dimension not covered by the 19 pre-built facets, add a custom facet. Examples: "map interaction patterns" for a geo app, "audio player controls" for a music app, "real-time collaboration indicators" for a multiplayer tool.

Custom facets use the `crit-custom` skill. Give each a clear id, name, and rationale explaining why it is needed and where it fits in the dependency order.

### Selection Reasoning

For each facet you include, write a one-sentence rationale tied to the brief. Not "this is a universal facet" but "the brief describes 7 screens with deep hierarchies -- navigation decisions constrain every layout downstream."

For each universal facet you EXCLUDE, state why. Exclusion requires a reason: "component-design excluded because the brief specifies shadcn/ui with no custom components in v1 scope."

**Typical project uses 8-12 facets.** If you have fewer than 6, you are likely under-scoping. If you have more than 15, you are likely over-scoping. Question both extremes.

---

## 4. Determine Dependency Order

Apply the three lenses and dependency rules from `design-principles.md`:

### Ordering Rules

1. **Screen inventory is always first.** Everything depends on knowing what surfaces exist.
2. **Structural before compositional.** Define what exists before arranging it.
3. **Compositional before sensory.** Arrange content before styling it.
4. **Accessibility is always last.** It audits accumulated decisions from all prior facets.
5. **Within a lens, order by downstream impact.** Facets that constrain more downstream decisions come earlier. Navigation before layout. Layout before typography.
6. **When two facets have no dependency, order by project priority.** If the brief emphasizes data display, promote data-visualization earlier within its lens.

### Dependency Declaration

For each facet, declare which other facets it depends on. A facet cannot start until all its dependencies are locked. Use the `depends_on` array in the facet entry.

Common dependency chains:
- `screen-inventory` -> `edge-states`, `navigation-model`
- `navigation-model` -> `content-layout`
- `content-layout` -> `content-hierarchy`, `component-design`, `typography`, `color-system`, `density-spacing`
- `typography`, `color-system`, `density-spacing` -> `accessibility`

### Deviating from Default Order

The default pipeline order in `design-principles.md` is a starting point, not a mandate. Deviate when the brief provides a clear reason:
- A brand-heavy marketing site might promote `color-system` and `typography` earlier.
- A data dashboard might promote `data-visualization` right after `content-layout`.
- A mobile-first product might move `responsive` earlier to resolve breakpoint strategy before layout details.

State the deviation and why: "Promoting color-system before density-spacing because the brief specifies a strong brand identity that should guide spacing decisions."

---

## 5. Generate Overview HTML

Generate `.design-crit/overview.html` with the proposed facet plan. This is the user's primary interface for reviewing and confirming the plan.

### Layout

Build a single-page HTML file with all CSS and JS inlined. No external dependencies.

**Header section:**
- Project name and one-line description from the brief
- Brief summary: platform, core loop, scope (3-4 lines max, extracted from brief.md)

**Facet plan section:**
- Ordered list of proposed facets, each showing:
  - Sequence number
  - Facet name (human-readable, e.g., "Navigation Model")
  - Mapped skill name (e.g., `crit-navigation`)
  - Lens tag: "structural", "compositional", "sensory", or "cross-cutting"
  - One-sentence rationale tied to the brief
  - Dependency indicators: which prior facets this depends on (show as connected labels or arrows)
  - Checkbox to enable/disable (checked by default for included facets)

**Interaction controls:**
- Drag handles on each facet row for reordering (use native HTML5 drag-and-drop)
- "Add Custom Facet" button at the bottom that reveals a form: name, description, lens category, position in order
- Visual grouping by lens (structural / compositional / sensory / cross-cutting) with subtle section dividers

**Confirmation area:**
- Facet count summary: "12 facets selected (9 universal, 3 contextual)"
- Estimated effort note: "Most facets resolve in 2 rounds. Estimated: 12-24 review rounds total."
- **Confirm Plan** button (primary)
- **Reset to Default** button (secondary) -- restores the LLM's original proposal

**Confirm Plan behavior:**
- Serialize the current plan state (enabled facets, order, custom additions) to JSON
- Use the File System Access API to write `facet-plan-confirmation.json` to the `.design-crit/` directory
- Fallback: display the JSON in a copyable textarea with instructions to paste into Claude Code

### Styling

Clean, professional. Light background, clear typography. Use the same design language as the compare view (consistent with `crit-loop.md` styling guidance). Include a dark mode toggle.

---

## 6. User Confirmation

After generating `overview.html`, open it in the user's browser.

Present the proposed plan in the terminal as well, using this format:

```
Facet Plan for [Project Name]
=============================

Based on the brief, I'm proposing [N] facets:

WHAT EXISTS (structural):
  1. Screen Inventory & Flows -- everything depends on knowing what surfaces exist
  2. Edge States -- [rationale]
  ...

HOW IT'S ARRANGED (compositional):
  3. Navigation Model -- [rationale]
  4. Content Layout -- [rationale]
  ...

HOW IT FEELS (sensory):
  7. Typography -- [rationale]
  8. Color System -- [rationale]
  ...

CROSS-CUTTING:
  [N]. Accessibility -- audits all accumulated decisions

Excluded: [list excluded universal facets with reasons]

Review the full plan in your browser (overview.html) where you can
reorder, enable/disable, or add custom facets. Or confirm here.
```

Ask the user to confirm. Accept confirmation via:
1. The user clicks "Confirm Plan" in `overview.html` (writes `facet-plan-confirmation.json`)
2. The user says "confirm", "looks good", "go ahead", or similar in the terminal

If the user requests changes in the terminal, apply them and regenerate `overview.html`.

---

## 7. Write State and Hand Off

Once the user confirms the plan:

### Update state.json

Write the confirmed facet plan to `.design-crit/state.json`. Each facet entry follows this schema:

```json
{
  "id": "navigation-model",
  "skill": "crit-navigation",
  "name": "Navigation Model",
  "lens": "compositional",
  "rationale": "Constrains all layout decisions downstream",
  "depends_on": ["screen-inventory"],
  "status": "pending",
  "enabled": true
}
```

Set `current_facet` to the first facet's id (always `screen-inventory`).
Set `current_round` to 1.

Set `facet_plan_status` to `"confirmed"` on the root object. Include the `project` block (name, description, platform, tech_stack) and `brief_status: "confirmed"` from the prior stage.

### Regenerate overview.html

Update `overview.html` to reflect the confirmed plan. Replace "Confirm Plan" with a "Plan Confirmed" indicator. Each facet now shows status (pending/in-progress/locked) and updates as crits progress.

### Hand Off

Announce the plan and start the first crit:

```
Plan confirmed: [N] facets, starting with Screen Inventory & Flows.
Invoking design-crit:crit-screen-inventory to begin the first crit.
```

The orchestrator reads `state.json`, sees `facet_plan_status: confirmed` and `current_facet: screen-inventory`, and routes to `crit-screen-inventory`.

---

## Example: Facet Plans by Project Type

These are illustrative, not prescriptive. Derive from the brief each time.

**Note-taking app (10 facets):**
screen-inventory -> edge-states -> navigation -> layout -> elevation-shape -> typography -> color-system -> density-spacing -> content-voice -> accessibility

**Data dashboard (12 facets):**
screen-inventory -> edge-states -> navigation -> layout -> data-visualization -> component-design -> elevation-shape -> typography -> color-system -> density-spacing -> loading-performance -> accessibility

**Marketing site (9 facets):**
screen-inventory -> layout -> content-hierarchy -> typography -> color-system -> imagery-illustration -> motion -> content-voice -> responsive

**Mobile utility app (8 facets):**
screen-inventory -> edge-states -> navigation -> layout -> component-design -> typography -> color-system -> accessibility
