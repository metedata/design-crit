---
name: design-direction
description: Use when all design facets are locked and the crit is ready to be synthesized into a final design direction document with deliverables.
---

# Design Direction

You are synthesizing a completed design crit into a final design direction. Every enabled
facet has been resolved — locked by the user or delegated to you. Your job: weave those
individual decisions into a coherent direction document, extract actionable deliverables,
and set up the handoff to implementation.

This is Stage 4 of the design crit pipeline. The crit is done. Now you are packaging it.

---

## 1. Prerequisite Check

Before generating anything, verify that the crit is actually complete.

Read `.design-crit/state.json`. For every facet where `"enabled": true`, confirm that
`"status"` is `"locked"`. If any enabled facet is not locked, stop and report:

```
These facets are still unresolved:
- [facet name] — status: [status]
```

Then ask: "Want to continue the crit for these facets, skip them, or let me decide them
for you?" Route accordingly:
- **Continue** — hand back to the orchestrator (`design-crit`) to resume the crit loop.
- **Skip** — set status to `"skipped"` in state.json and proceed with direction generation.
  Note skipped facets in the direction document.
- **Decide for me** — invoke the facet's skill with a delegate decision, lock it, then
  return here.

Only proceed with deliverable generation when ALL enabled facets are locked or skipped.

---

## 2. Gather Locked Decisions (Context-Efficient)

This is the one stage that needs the full picture, but load strategically to avoid
filling context with all HTML files simultaneously.

**Read upfront (small files):**
1. **`state.json`** — every locked facet's `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, and `rounds_completed`.
2. **`.design-crit/brief.md`** — the project brief grounds all rationale.
3. **`.design-crit/locked-constraints.md`** — compact summary of all locked decisions.

**Do NOT read all HTML option files at once.** Instead, load them one at a time as you
write each section of the deliverables (see Step 3). This keeps context manageable even
for projects with 12+ facets.

**Read per-facet (only when writing that facet's section):**
4. **The facet's winning option file** — `.design-crit/facets/{facet-id}/{locked_option}.html`
5. **The facet's `critique.md`** — final comparative take and rationale.
6. **The facet's feedback files** — `feedback-round-*.json` for decision history.

Load items 4-6 for one facet, write that facet's section in the deliverables, then move to
the next facet. Do not accumulate all facets' HTML in context.

Organize by the three lenses:
- **What exists** — screen inventory, edge states, onboarding flows
- **How it's arranged** — navigation, layout, content hierarchy, components, elevation, data viz, responsive
- **How it feels** — typography, color, density, iconography, motion, voice, imagery, loading

---

## 3. Generate Deliverables

### 3a. `direction.html` — The Design Direction Document

Write `.design-crit/direction.html`. This is the primary deliverable — a polished,
self-contained HTML page presenting the complete design direction.

**Structure the page as follows:**

**Header section:**
- Project name (from `state.json` > `project.name`)
- One-line description (from `state.json` > `project.description`)
- Platform and tech stack
- Date generated
- Summary: "[N] design facets resolved across [M] total rounds of critique"

**Screen gallery:**
- For each screen identified in the screen inventory, show:
  - Screen name and purpose (one line)
  - Thumbnail linking to the final wireframe HTML file (the locked option file)
  - "Open wireframe" link that opens the option HTML file directly

**Design decisions by lens:**
- Group decisions under the three lens headings: What Exists, How It's Arranged, How It Feels
- For each locked facet within the lens:
  - Facet name and locked option name (e.g., "Navigation — Deep Sidebar")
  - The `locked_summary` as a concise description
  - The `decision_rationale` explaining why this direction was chosen
  - Whether user-decided or LLM-delegated (show "(auto)" badge for delegated)
  - Link to the locked option HTML file

**Expandable decision log:**
- A collapsible section at the bottom titled "Full Decision History"
- For each facet (in pipeline order):
  - Facet name and final outcome
  - For each round: which options were presented (names), which were kept/cut,
    user comments, overall direction notes
  - The final lock decision with rationale
- Use `<details>` / `<summary>` elements for expand/collapse per facet

**Styling requirements:**
- Self-contained HTML with inline CSS. No external dependencies.
- Clean, professional layout. Use a readable sans-serif system font stack.
- Responsive — works on desktop and mobile.
- Light background with clear section separation.
- Delegated decisions shown with a subtle visual distinction (e.g., dotted border or "(auto)" label).

### 3b. `design-tokens.json` — Extracted Design Tokens

Write `.design-crit/design-tokens.json` ONLY if the crit covered relevant facets. Check
which token-producing facets were locked:

| Facet | Token Category | What to Extract |
|---|---|---|
| `crit-color-system` | `colors` | Primary, secondary, surface, text, status colors from the locked palette |
| `crit-typography` | `typography` | Font families, type scale values, line heights, font weights |
| `crit-density-spacing` | `spacing` | Base unit, spacing scale values, touch target sizes |
| `crit-layout` | `layout` | Grid column count, sidebar width, content max-width, gutter size |
| `crit-elevation-shape` | `elevation` | Shadow values per level, border radius scale |

If none of these facets were in the plan, skip this deliverable entirely and note in the
terminal: "No token-producing facets were covered — skipping design-tokens.json."

**Token JSON structure:** Top-level keys are `generated` (date), `project` (name), then
one object per category: `colors`, `typography`, `spacing`, `layout`, `elevation`. Only
include categories for facets that were actually locked.

Extract token values by reading the locked option HTML files. Parse inline CSS for
concrete values. Use semantic naming (`"surface-primary"`, `"text-secondary"`) not
implementation naming (`"blue-500"`).

### 3c. `decisions.md` — Full Markdown Decision Log

Write `.design-crit/decisions.md`. Version-control-friendly backup of the decision
history for team reference.

**Format:** Header with project name, date, facet/round counts, and a 2-3 sentence brief
summary. Then a `## Decisions` section grouped by the three lenses (What Exists, How It's
Arranged, How It Feels). Each facet gets an `####` heading with: locked option name +
summary, decided by (user or auto), round count, rationale, and a round-by-round history
showing options presented, kept/cut, and user comments. End with a `## Skipped Facets`
section listing any skipped facets with reason, or "None".

---

## 4. Final Overview Update

Regenerate `.design-crit/overview.html` to reflect the completed state.

Every facet row must show:
- Checkmark icon for locked facets, skip icon for skipped facets
- Facet name + locked option name (e.g., "Navigation — Deep Sidebar")
- "(auto)" indicator for LLM-delegated decisions
- **Review** button that expands inline to show the full decision history for that facet
  (same content as the expandable log in direction.html)

Add a **Completed** banner at the top:
- "Design crit complete. [N] facets resolved."
- Link to `direction.html`: "View design direction"
- Link to `decisions.md`: "View decision log"
- Link to `design-tokens.json` (if generated): "View design tokens"

---

## 5. Handoff to Implementation

After generating all deliverables, present a summary in the terminal listing each file
generated (`direction.html`, `decisions.md`, `design-tokens.json` if applicable,
`overview.html`) with a one-line description of each. Note that all files live in
`.design-crit/`.

Then read `state.json` > `project.tech_stack` and suggest the implementation handoff:

"Ready to implement? Your project uses **[tech stack]**. The `frontend-design` skill can
use these wireframes and design tokens as reference. Run `/frontend-design` to get started."

If no tech stack is recorded, suggest generically:

"Ready to implement? The wireframes in `.design-crit/facets/` and the design tokens in
`design-tokens.json` are your reference. If you use a frontend scaffolding skill, point
it at these files."

Open `direction.html` in the user's browser as the final action.
