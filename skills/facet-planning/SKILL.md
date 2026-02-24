---
name: facet-planning
description: Use when a design brief is confirmed and the project needs a plan for which design facets to evaluate and in what order.
---

# Planning the Review

You are a design lead planning a structured critique. The brief is confirmed. Your job:
figure out WHICH design areas this project needs to work through and in WHAT order, then
get the user's sign-off before any design work begins.

Derive areas from first principles -- analyze the brief, identify what decisions matter
for THIS project, and select from the pre-built library. Do not apply a fixed checklist.
A note-taking app and a data dashboard need different areas in different orders.

**User-facing language:** Always say "design area" or just "area" when talking to the
user. Never say "facet" in user-facing output. Internally, the code and file paths
still use "facet" -- that is fine. The user should never see that term.

---

## 1. Load References

Read these files before doing anything else:

1. `.design-crit/brief.md` -- the confirmed design brief
2. `.design-crit/state.json` -- current project state
3. `../../reference/design-principles.md` -- the area library, three lenses framework, dependency ordering rules, and default pipeline order

The design-principles reference contains the full library of design areas with universal/contextual flags, the three lenses framework, and dependency ordering rules. Do not duplicate that content here. Read it every time.

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

## 3. Select Design Areas

Use the three lenses framework from `design-principles.md` to select areas.

### Core Areas (consider for every project)

These 12 areas apply to nearly every project. Include them by default, then remove only with explicit reasoning:

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

### Situational Areas (include based on project type)

These 7 areas apply only when the project warrants them:

| Facet ID | Skill | Include When |
|---|---|---|
| `onboarding-flows` | `crit-onboarding-flows` | Consumer apps, complex tools, products with learning curve |
| `data-visualization` | `crit-data-visualization` | Dashboards, analytics, data-heavy products |
| `responsive` | `crit-responsive` | Cross-device products, mobile+desktop |
| `iconography` | `crit-iconography` | Apps with rich icon-driven UI, custom icon needs |
| `motion` | `crit-motion` | Polish-focused products, marketing sites, transitions-heavy UI |
| `imagery-illustration` | `crit-imagery-illustration` | Marketing sites, consumer apps, brand-heavy products |
| `loading-performance` | `crit-loading-performance` | Async-heavy apps, data dashboards, slow-network contexts |

### Custom Areas

If the brief reveals a design dimension not covered by the 19 pre-built areas, add a
custom one. Examples: "map interaction patterns" for a geo app, "audio player controls"
for a music app, "real-time collaboration indicators" for a multiplayer tool.

Custom areas use the `crit-custom` skill. Give each a clear id, name, and rationale
explaining why it is needed and where it fits in the order.

### Selection Reasoning (internal only)

For each area you include, write a one-sentence rationale tied to the brief. Not "this
is a universal area" but "the brief describes 7 screens with deep hierarchies --
navigation decisions constrain every layout downstream."

For each core area you EXCLUDE, state why. Exclusion requires a reason:
"component-design excluded because the brief specifies shadcn/ui with no custom components
in v1 scope."

**Typical project uses 8-12 areas.** Fewer than 6 likely means under-scoping. More than
15 likely means over-scoping. Question both extremes.

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

Generate `.design-crit/overview.html` with the proposed plan. This is a visual reference,
but NOT the primary way the user interacts. The terminal conversation is the primary flow.

### Layout

Build a single-page HTML file with all CSS and JS inlined. No external dependencies.
Follow `./crit-ui.md` for styling.

**Header section:**
- Project name and one-line description from the brief
- A clear explanation: "Here's the plan for your design review. We'll work through each
  area together, one at a time."

**Plan section:**
- Ordered list of proposed areas, grouped by phase with plain labels:
  - **"Defining the product"** (structural) -- what screens exist, edge cases, onboarding
  - **"Arranging the pieces"** (compositional) -- navigation, layout, component patterns
  - **"Making it feel right"** (sensory) -- typography, colors, spacing, motion
- Each area shows: sequence number, human-readable name, one-sentence rationale
- No technical skill names, no dependency arrows, no lens tags
- Checkboxes for enable/disable (checked by default)

**Interactive controls:**
- Checkboxes for enable/disable per area (checked by default for included areas)
- Drag handles on each row for reordering (use native HTML5 drag-and-drop)
- **Confirm Plan** button (primary, styled per `../../reference/crit-ui.md`)
- **Reset to Default** button (secondary) — restores the LLM's original proposal
- On Confirm: serialize the plan state (enabled areas, order) to JSON and write
  `.design-crit/facet-plan-confirmation.json` via the File System Access API.
  Show a banner: "Plan confirmed! Claude is picking it up."
  Fire an OS notification if permission is granted.
- Fallback if File System Access API is unavailable: show a copyable JSON textarea
  with instructions to paste into the terminal.

### Styling

Follow `../../reference/crit-ui.md` for all styling (dark chrome, clean typography).

---

## 6. Present the Plan Conversationally

After generating `overview.html`, open it in the user's browser.

**The terminal is the primary interface.** Present the plan conversationally. Explain
what you're proposing and why, as if you're a designer walking a client through the
process. Use this structure:

```
Here's how I'd approach the design review for [Project Name].

We'll work through [N] design areas together, one at a time. For each one, I'll
generate a few options as interactive wireframes, we'll look at them side by side,
and you'll tell me what you like and what to cut. Most areas take about 2 rounds
to nail down.

Here's the order I'm proposing:

FIRST, WE DEFINE WHAT EXISTS:
  1. Screen Inventory — figure out what screens we need and how they connect
  2. Edge States — what happens when things are empty, loading, or broken
  ...

THEN, WE ARRANGE THE PIECES:
  3. Navigation — how users move between screens
  4. Layout — how content is organized on each screen
  ...

FINALLY, WE MAKE IT FEEL RIGHT:
  7. Typography — fonts, sizes, hierarchy
  8. Colors — palette, meaning, contrast
  ...

AND A FINAL CHECK:
  [N]. Accessibility — making sure everything works for everyone

I've also opened a visual version of this plan in your browser.

Does this plan look right? You can:
- Remove areas you don't think we need
- Add something I missed
- Change the order
- Or just say "looks good" and we'll get started
```

**Key principles:**
- Explain the process, not just the list. The user may not know what a structured
  design review looks like.
- Use everyday language. "Figure out what screens we need" not "screen inventory and
  state maps."
- Tell the user both feedback paths: "You can confirm or adjust the plan right in
  the browser — I'll pick it up automatically. Or just tell me here in the chat."

**Start a Bash poll** for the confirmation file after opening the overview:
```bash
timeout 300 bash -c 'while [ ! -f ".design-crit/facet-plan-confirmation.json" ]; do sleep 2; done' && cat ".design-crit/facet-plan-confirmation.json"
```
If the file appears, read it and apply the user's changes. If the user responds in
the chat instead ("looks good", or requests changes), that interrupts the poll.

If the user requests changes (from either path), apply them and regenerate `overview.html`.

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

Announce the plan in plain language and start the first area:

```
Great, the plan is locked in. Let's start with the first area: Screen Inventory.
I'll generate a few options for what screens your app needs and how they connect,
then we'll review them side by side.
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
