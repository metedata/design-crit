---
name: crit-custom
description: Use when a project needs a design facet not covered by the pre-built facet library. Provides crit loop mechanics with design principles but no domain-specific guidance.
---

# Custom Facet Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing a custom design facet -- one not covered by the pre-built facet
library. The facet planning stage identified a project-specific design dimension that needs
its own crit loop. This skill provides the framework. You supply the domain knowledge.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Read the Custom Facet Definition

In `state.json`, find the current facet entry. Read its `id`, `name`, `rationale`, and
`depends_on` fields. These were set during facet planning and describe:

- **What** this facet covers (the design dimension)
- **Why** it was added (the rationale tying it to the brief)
- **Where** it sits in the dependency chain (what must be locked before this runs)

### Prior Locked Decisions

Read every locked facet's winning option file. The custom facet inherits all prior locked
decisions as constraints. State in your critique: "Building on the locked [facet]: [summary]"
for each dependency listed in `depends_on`.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/{facet-id}/`. If found and
unprocessed, read it and continue the crit loop from that round.

---

## 2. Derive Domain Knowledge

You have no pre-built domain content for this facet. Derive it from first principles.

### Step A: Define the Design Space

From the facet name, rationale, and brief, identify:

1. **What decisions does this facet make?** List the specific design choices this facet
   resolves. Example: for "map interaction patterns," the decisions might be: zoom controls,
   pin clustering, area selection, map vs list toggle, location search behavior.
2. **What are the axes of variation?** Identify 2-4 dimensions along which options can
   meaningfully differ. Example: "information density on map" (sparse vs dense), "interaction
   model" (click-to-inspect vs hover-to-preview), "map prominence" (map-primary vs
   split-view).
3. **What are the established patterns?** Draw on your knowledge of design systems, platform
   conventions, and industry patterns for this domain. Cite specific references where
   possible (e.g., "Google Maps uses X, Mapbox defaults to Y").

### Step B: Derive Evaluation Criteria

Generate 4-6 evaluation criteria specific to this facet. Each criterion must be:
- Phrased as a question (e.g., "Can the user accomplish the core task without the map?")
- Tied to the brief (e.g., "Does this support the [target user] workflow described in the brief?")
- Assessable from the artifact (e.g., "Is the interaction model learnable in under 30 seconds?")

### Step C: Identify Common Trade-offs

Generate a table of 4-6 common trade-offs for this design dimension, following the format
used by pre-built facets:

| Pattern | When It Works | Watch Out For |
|---|---|---|
| [Pattern name] | [Scenarios where this is strong] | [Risks and costs] |

---

## 3. Generate Options

Follow `../../reference/design-principles.md` for option count, naming, and rationale structure.

### Round 1: Diverge

Generate 2-4 options that explore different approaches to this design dimension. Each option
must:

1. Make a distinct bet about what matters most (from the axes of variation in Step A).
2. Have a descriptive name capturing its core idea (not "Option A").
3. Include a complete rationale: WHY THIS OPTION, WORKS WELL FOR, WATCH OUT FOR.
4. Be rendered as a self-contained HTML artifact demonstrating the approach at wireframe to
   medium fidelity, depending on the facet's nature.

### Round 2+: Converge

Refine survivors based on feedback. Apply the convergence guidance from `crit-loop.md`.

---

## 4. Artifact Requirements

Each option file (`option-{x}.html`) must be self-contained HTML per `crit-loop.md` rules.

Determine the appropriate artifact type based on the facet's nature:

- **Interaction facets** (map controls, drag-and-drop, gesture patterns): render interactive
  specimens showing the key interactions. Include state transitions (idle, hover, active,
  completed).
- **Visual facets** (brand elements, chart styles, data display): render visual specimens
  in context using the locked layout, colors, and typography.
- **Structural facets** (information architecture, content models, workflow patterns): render
  diagrams and flow maps similar to the screen inventory approach.
- **Behavioral facets** (notification rules, sync strategy, collaboration model): render
  scenario storyboards showing the system's behavior across key moments.

Apply locked visual styles (colors, typography, density, layout) when they are available.
The custom facet should feel like part of the same design system as the locked facets.

---

## 5. Compare View and Crit Loop

Follow `../../reference/crit-loop.md` exactly for:

- Compare view HTML generation in `.design-crit/facets/{facet-id}/compare.html`
- Feedback round-trip mechanics
- Convergence guidance and escalating lock prompts
- Decide For Me handling

Include the dark mode toggle only if the facet involves visual styling. Omit it for
structural or behavioral facets.

Write `critique.md` with per-option rationale and a comparative take. Apply the derived
evaluation criteria from Step B. State your recommendation and the key trade-off.

---

## 6. Locking and Handoff

When the user locks a custom facet option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must be specific enough for downstream facets to use as a constraint.
   Not "custom facet resolved" but "map interaction uses click-to-inspect with clustered
   pins and a persistent search bar."
3. Announce the lock and note any downstream implications: "Map interactions locked as
   [name]. [summary]. This constrains component design (map controls) and responsive
   (map behavior at mobile breakpoints)."

---

## 7. Constraints

- Do not invent domain knowledge you do not have. If a custom facet covers a domain where
  your training data is thin, state what you know and what you are uncertain about. Ask the
  user to fill gaps rather than guessing.
- Apply the same structural rigor as pre-built facets: concrete artifacts, honest trade-offs,
  named options, evaluation criteria. The custom facet is not a lesser crit.
- The facet definition from state.json is your brief within the brief. If the name and
  rationale are too vague to generate meaningful options, ask the user to clarify before
  proceeding: "The custom facet [name] needs more definition. What specific design decisions
  should it resolve?"
