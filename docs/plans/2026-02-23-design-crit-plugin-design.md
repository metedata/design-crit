# Design Crit Plugin — Design Document

## Overview

A Claude Code plugin that guides users through a structured design critique process for any app or service. It generates HTML wireframe artifacts for iterative, side-by-side review — like a designer presenting options at crit, helping the user make informed decisions about every visual and structural aspect of their product.

**The core loop:** Present options with rationale → user reviews and provides feedback → refine survivors → converge on a direction. Repeat per design facet until a complete design direction emerges.

**What this is:** A design crit partner that helps you decide HOW your product should look and work.

**What this is not:** A brainstorming tool for figuring out WHAT to build. The plugin assumes you have (or will develop through a guided brief) enough context about the product to start making design decisions.

**Nothing like this exists.** Competitive research across GitHub, plugin registries, community directories, and the broader AI design tool space found no multi-stage design critique workflow that generates HTML wireframe artifacts for iterative review. Existing design skills (frontend-design, Impeccable, interface-design, etc.) provide aesthetic guidance but no critique process, no wireframing, and no iterative review loop.

## Architecture

### Plugin Structure (Multi-Skill with Orchestrator)

Follows the proven superpowers pattern (`brainstorming` → `writing-plans` → `executing-plans`). Each stage is its own skill, and each design facet has its own domain-specific skill. Token-efficient: only the current stage + current facet skill loads into context.

```
design-crit/
  plugin.json
  skills/
    # Pipeline skills
    design-crit/SKILL.md             # Orchestrator — knows the full pipeline, routes to stages
    design-brief/SKILL.md            # Stage 1: Context assessment + brief creation
    facet-planning/SKILL.md          # Stage 2: Derive facets + ordering from pre-built library
    design-direction/SKILL.md        # Stage 3: Synthesis + deliverables

    # Pre-built facet skills — WHAT EXISTS (structural)
    crit-screen-inventory/SKILL.md   # Flow diagrams, state maps, screen enumeration
    crit-edge-states/SKILL.md        # Empty, error, offline, loading content, boundary conditions
    crit-onboarding-flows/SKILL.md   # First-run experience, progressive disclosure, tutorials

    # Pre-built facet skills — HOW IT'S ARRANGED (compositional)
    crit-navigation/SKILL.md         # Nav patterns, wayfinding, menu structures
    crit-layout/SKILL.md             # Grid, composition, spatial arrangement
    crit-content-hierarchy/SKILL.md  # Information architecture within screens, what's prominent
    crit-component-design/SKILL.md   # Buttons, forms, cards, modals — interaction patterns
    crit-elevation-shape/SKILL.md    # Z-axis layering, shadows, surface treatments, border radius
    crit-data-visualization/SKILL.md # Charts, graphs, dashboards, data display patterns
    crit-responsive/SKILL.md         # Cross-device adaptation, breakpoint strategy

    # Pre-built facet skills — HOW IT FEELS (sensory/experiential)
    crit-typography/SKILL.md         # Font systems, type scale, hierarchy, readability
    crit-color-system/SKILL.md       # Palettes, themes, contrast, dark mode, tokens
    crit-density-spacing/SKILL.md    # Information density, whitespace, compact modes
    crit-iconography/SKILL.md        # Icon systems, visual metaphors, symbolic language
    crit-motion/SKILL.md             # Animation, transitions, micro-interactions
    crit-content-voice/SKILL.md      # Microcopy, error messaging, CTA language, tone consistency
    crit-imagery-illustration/SKILL.md # Photography style, illustration system, hero visuals
    crit-loading-performance/SKILL.md  # Skeleton screens, optimistic UI, progress indicators

    # Pre-built facet skills — CROSS-CUTTING AUDIT
    crit-accessibility/SKILL.md      # Keyboard nav, screen readers, touch targets, focus, ARIA

    # Generic facet skill (for project-specific facets not in the library)
    crit-custom/SKILL.md              # Crit loop mechanics + design principles, no domain specifics

  reference/
    design-principles.md             # Shared first principles for all facet skills
    brief-template.md                # What an optimal brief contains
    crit-loop.md                     # Shared crit loop mechanics (compare, feedback, convergence)
```

**Why this architecture:**
- **Token efficiency** — only the current facet's skill loads. A color crit doesn't carry typography knowledge.
- **Domain depth** — each facet skill has specific evaluation criteria, artifact types, and trade-off knowledge that a generic skill can't match.
- **Independent entry** — user can invoke `/crit-color-system` directly without the full pipeline.
- **Curated, not fixed** — facet-planning picks from the pre-built library per project. A note-taking app might use 6 of these; a data dashboard might use 8; a marketing site might use 5 different ones. Not all are invoked for every project.
- **Extensible** — project-specific facets (e.g., "map interaction patterns" for a geo app) use `crit-custom`, the generic skill with crit loop mechanics but no domain specifics.
- Each SKILL.md stays under 500 lines (Anthropic best practice)
- State flows through files on disk, same as superpowers flows through `docs/plans/`

**Shared mechanics across all facet skills:**
All `crit-*` skills share the same crit loop (generate options, compare view, per-option rationale, feedback round-trip, convergence). This shared behavior lives in `reference/crit-loop.md`, loaded by each facet skill. What differs per skill:
- What kind of artifacts to generate (flow diagrams vs wireframes vs palettes vs type specimens)
- What evaluation criteria matter for this dimension
- What "good" looks like in this domain
- Common trade-offs and patterns to present
- How many options typically make sense

**The orchestrator** (`design-crit/SKILL.md`) is the primary entry point. It reads `.design-crit/state.json`, determines what stage the project is in, and routes to the appropriate stage or facet skill. Users can also invoke any facet skill directly for standalone use.

### File System State

All state lives in a `.design-crit/` directory at the project root:

```
.design-crit/
  state.json                        # Master state: facets, progress, decisions, current round
  brief.md                          # The confirmed creative brief
  overview.html                     # Facet navigation + progress dashboard

  facets/
    01-screen-inventory/
      compare.html                  # Side-by-side comparison view (iframes options)
      option-a.html                 # Individual option: "Minimal (4 screens)"
      option-b.html                 # Individual option: "Full Suite (7 screens)"
      feedback-round-1.json         # User selections + comments from round 1
      critique.md                   # LLM's written rationale + comparative take

    02-navigation-model/
      compare.html
      option-a.html                 # "Deep Sidebar"
      option-b.html                 # "Tab Rail"
      option-c.html                 # "Quick-Switch Panel"
      feedback-round-1.json
      feedback-round-2.json
      critique.md

    03-content-layout/
      ...
```

**Key principles:**
- `state.json` is the single source of truth the LLM reads each turn
- Individual option files persist across rounds — they get refined in place, gaining fidelity
- `compare.html` is regenerated each round to compose current options
- Feedback JSON files capture the round-trip from HTML back to Claude
- Facet folders are numbered for ordering but the overview allows reordering
- Locked facet decisions carry forward as constraints for later facets

## The Pipeline

### Stage 1: Design Brief (`design-brief`)

**Purpose:** Ensure the LLM has enough context to make good design decisions. Not brainstorming — scoping.

**Flow:**
1. Harvest available context: codebase (routes, components, tech stack), docs, READMEs, specs, user's description
2. Evaluate against brief template — score which sections can be filled from existing context
3. Branch based on context sufficiency:
   - **Sufficient:** Draft brief from available context, user confirms/adjusts in `overview.html`
   - **Partial:** Draft what's available, ask 2-3 targeted questions for gaps
   - **Minimal:** Guide user through brief creation, question by question

**What the brief captures (the minimum for good design work):**
- **Who's using this and where?** "Commuters on their phone" vs "analysts at their desk" — changes density, touch targets, flow
- **What's the core loop?** The one thing the user does repeatedly. Anchors every screen.
- **What makes THIS one different?** If nothing, fine. But "it's for couples" changes the entire screen inventory.
- **Platform and constraints.** Web, native, both. Existing tech stack.
- **Scope.** What's in v1. What's explicitly out.

**The brief is the first deliverable, not a gate.** It's presented in `overview.html` as a draft the LLM wrote, with gaps highlighted. For rich-context projects, it's mostly filled in and confirmed in 30 seconds. For vague ideas, the highlighted gaps are honest: "I don't know what makes your habit tracker different from the 500 that exist. That matters for design."

**Output:** `.design-crit/brief.md`

### Stage 2: Facet Planning (`facet-planning`)

**Purpose:** Derive which design dimensions need to be resolved for THIS specific project, in what order.

**The plugin ships with a rich library of pre-built facet skills** (14 domain-specific `crit-*` skills + 1 generic). Facet planning selects the relevant subset per project and proposes an ordering. Not all facets are used for every project.

**Framework encoded in the skill:**

1. **Dependency ordering** — Structural decisions before visual ones. Decisions that constrain other decisions come first. Screen inventory is always first because everything else depends on knowing what surfaces exist.

2. **The three lenses** — Every design project moves through:
   - **What exists** (screens, states, flows, information architecture)
   - **How it's arranged** (layout, hierarchy, spatial relationships, components)
   - **How it feels** (tone, motion, density, typography, color)

3. **The LLM's job:** Analyze the brief → select facets from the pre-built library → add project-specific custom facets if needed → propose ordering with dependency reasoning → generate `overview.html` showing the plan as an interactive checklist.

**The pre-built facet library (19 + 1 generic, cross-referenced against Material Design 3, Apple HIG, Atlassian, Polaris, Carbon, NN/g):**

**WHAT EXISTS (structural):**

| Facet Skill | Universal? | Artifacts | Typical Projects |
|---|---|---|---|
| `crit-screen-inventory` | Universal | Flow diagrams, state maps, screen sets | All projects (always first) |
| `crit-edge-states` | Universal | Empty/error/offline/loading state designs | All apps |
| `crit-onboarding-flows` | Contextual | First-run experience, tutorials | Consumer apps, complex tools |

**HOW IT'S ARRANGED (compositional):**

| Facet Skill | Universal? | Artifacts | Typical Projects |
|---|---|---|---|
| `crit-navigation` | Universal | Nav wireframes, menu structures | Multi-screen apps |
| `crit-layout` | Universal | Grid compositions, spatial arrangements | All visual projects |
| `crit-content-hierarchy` | Universal | Priority maps, emphasis wireframes | Content-heavy apps |
| `crit-component-design` | Universal | Button/form/card/modal/table patterns | Interactive apps |
| `crit-elevation-shape` | Universal | Shadow systems, surface layers, radius scales | All visual projects |
| `crit-data-visualization` | Contextual | Chart types, data display patterns | Dashboards, analytics |
| `crit-responsive` | Contextual | Breakpoint strategies, device adaptation | Cross-device apps |

**HOW IT FEELS (sensory/experiential):**

| Facet Skill | Universal? | Artifacts | Typical Projects |
|---|---|---|---|
| `crit-typography` | Universal | Type specimens, hierarchy scales | All visual projects |
| `crit-color-system` | Universal | Palettes, theme previews, contrast checks | All visual projects |
| `crit-density-spacing` | Universal | Compact vs comfortable comparisons | Data-heavy apps, power tools |
| `crit-iconography` | Contextual | Icon systems, visual metaphors | Apps with rich UI |
| `crit-motion` | Contextual | Transition specs, animation concepts | Polish-focused projects |
| `crit-content-voice` | Universal | Microcopy specimens, tone comparisons | All apps (every UI has text) |
| `crit-imagery-illustration` | Contextual | Photo/illustration style guides | Marketing sites, consumer apps |
| `crit-loading-performance` | Contextual | Skeleton screens, progress patterns | Async/data-heavy apps |

**CROSS-CUTTING AUDIT:**

| Facet Skill | Universal? | Artifacts | Typical Projects |
|---|---|---|---|
| `crit-accessibility` | Universal | Audit reports, remediation wireframes | All projects (runs last) |

**GENERIC:**

| Facet Skill | Universal? | Artifacts | Typical Projects |
|---|---|---|---|
| `crit-custom` | N/A | Depends on context | Project-specific needs |

**Total: 19 pre-built + 1 generic = 20 facet skills. 12 universal, 7 contextual. Typical project uses 8-12.**

**Example — facet planning picks different subsets per project:**

A note-taking app (10 facets):
```
 1. crit-screen-inventory    → Screen inventory & flows
 2. crit-edge-states         → Empty notebooks, sync errors, offline mode
 3. crit-navigation          → Navigation model
 4. crit-layout              → Content layout (editor, list, detail)
 5. crit-elevation-shape     → Card surfaces, modal layers, editor depth
 6. crit-typography          → Typography system (monospace for code?)
 7. crit-color-system        → Color & theming, dark mode
 8. crit-density-spacing     → Information density
 9. crit-content-voice       → Microcopy, empty state messaging
10. crit-accessibility       → Keyboard nav (power users), focus management
```

A data dashboard (12 facets):
```
 1. crit-screen-inventory    → Data views & drill-down flows
 2. crit-edge-states         → No data, query errors, stale data indicators
 3. crit-navigation          → Dashboard switching, drill-down patterns
 4. crit-layout              → Dashboard grid & composition
 5. crit-data-visualization  → Chart types, data display patterns
 6. crit-component-design    → Filter controls, date pickers, data tables
 7. crit-elevation-shape     → Card hierarchy, popover layers
 8. crit-typography          → Data label legibility, number formatting
 9. crit-color-system        → Status colors, data encoding, themes
10. crit-density-spacing     → Data density & progressive disclosure
11. crit-loading-performance → Skeleton screens, streaming data indicators
12. crit-accessibility       → Screen reader for data, color blindness for charts
```

A marketing site (9 facets):
```
 1. crit-screen-inventory    → Page inventory & conversion flows
 2. crit-layout              → Hero/section composition
 3. crit-content-hierarchy   → Messaging priority & CTA placement
 4. crit-typography          → Brand typography
 5. crit-color-system        → Brand palette & mood
 6. crit-imagery-illustration → Photography style, hero visuals
 7. crit-motion              → Scroll animations & micro-interactions
 8. crit-content-voice       → CTA copy, persuasive tone, headline style
 9. crit-responsive          → Mobile-first conversion optimization
```

**The overview page (`overview.html`) for this stage:**
- Shows all proposed facets with rationale, dependency arrows, and ordering
- Maps each facet to its pre-built skill (or generic)
- Checkboxes to enable/disable facets
- Drag to reorder
- Button to add custom facets (routes to `crit-custom`)
- User confirms before any crit work begins

**Output:** `state.json` updated with confirmed facet plan (including skill mappings)

### Stage 3: The Crit Loop (per-facet skills)

**Purpose:** For each enabled facet, the orchestrator invokes the corresponding `crit-*` skill, which generates design options and runs the iterative crit loop until the user locks a direction.

Each facet skill is domain-specific but follows the same shared crit loop mechanics (from `reference/crit-loop.md`). This is the core of the plugin experience.

#### The Compare View (`compare.html`)

Desktop-first, side-by-side layout. Options are always visible simultaneously for comparison:

- **2 options:** 50/50 split, full height
- **3 options:** 33/33/33 row
- **4 options:** 2x2 grid
- **5+ options:** 2-column scrollable grid (rare, early divergent rounds only)

Each option card contains:

**The wireframe** — iframed from the individual option HTML file. Scales proportionally. Can also be opened directly in a new tab for full-size review.

**Named characterization** — Not "Option A" but "Deep Sidebar" or "Content-First Rail." A short label that captures the essence.

**Per-option rationale (the designer presenting at crit):**
- **WHY THIS OPTION:** The design reasoning. Why the LLM thought this was worth presenting. Ties back to the brief.
- **WORKS WELL FOR:** Scenarios where this approach shines
- **WATCH OUT FOR:** Honest trade-offs and considerations

**Per-option controls:**
- Keep / Cut buttons
- Comment text field

Below all option cards, a **comparative take** — the LLM's overall read across all options, like a designer saying "here's my recommendation and why."

At the bottom, **global feedback:**
- Overall direction / notes text field
- Save Feedback button (writes to `feedback-round-N.json`)
- Skip This Facet button
- **Decide For Me** button (see below)

Additional controls:
- Dark mode toggle (where relevant)
- Filter: All / Surviving / Eliminated (for rounds 2+)
- Context banner: project name, current facet, round number

#### Individual Option Files (`option-[x].html`)

Each option is a self-contained HTML file with inline CSS. No external dependencies. Renders correctly both iframed in `compare.html` and opened directly in a browser tab.

In early structural facets (screen inventory), these are flow diagrams and screen maps. In layout facets, they're low-fi wireframes with boxes and placeholder text. As facets progress (typography, color, density), the same files get refined in place — they accumulate fidelity. Option A from the layout round becomes the canvas that color gets applied to.

#### The Feedback Round-Trip

```
Round N starts:
  1. Claude reads state.json → current facet, round, prior decisions
  2. Claude reads feedback-round-(N-1).json → what to refine
  3. Claude reads surviving option files → current state of each
  4. Claude generates/refines option HTML files
  5. Claude generates compare.html (iframes the options)
  6. Claude writes critique.md with per-option rationale + comparative take
  7. Claude opens compare.html in browser

User reviews:
  8. Views compare.html, reviews options side by side
  9. Marks Keep/Cut per option, writes comments
  10. Clicks Save Feedback
  11. Form writes to .design-crit/facets/[current]/feedback-round-N.json

Round N+1:
  12. User returns to Claude Code
  13. Claude reads feedback → refine survivors, generate new options, or suggest locking
  14. Loop or advance to next facet
```

**Feedback JSON format:**
```json
{
  "facet": "navigation-model",
  "round": 2,
  "options": {
    "option-a": { "action": "keep", "comment": "Like the sidebar but needs keyboard shortcuts" },
    "option-b": { "action": "cut", "comment": "Too simple for deep hierarchies" },
    "option-c": { "action": "keep", "comment": "" }
  },
  "overall": "Leaning toward A but want C's quick-switch as a secondary mode",
  "decision": "refine"
}
```

**How Save Feedback works without a server:**
Inline JS serializes form data to JSON and uses the File System Access API (if supported) to write directly. Falls back to a copy-pasteable JSON text block the user pastes into Claude Code.

#### Convergence Mechanics

**Default rhythm (soft guidance for the LLM, not enforced):**
- **Round 1:** Diverge. Present options exploring different directions. User eliminates, comments.
- **Round 2:** Converge. Refine survivors, possibly merge. User picks direction.
- **Round 3 (optional):** Polish. One more pass on the chosen direction for high-stakes facets.
- **Most facets resolve in 2 rounds.** Round 4+ triggers active convergence guidance.

**After each round, the LLM asks:**
- Round 1: "Want to refine the survivors, or do you already see the direction?"
- Round 2: "Ready to lock this, or one more refinement pass?"
- Round 3+: "I'd recommend locking. The remaining differences are details for implementation."

**Convergence tone — designer in service of the decision:**

The LLM acts as a designer whose job is to help the stakeholder decide, not to manage them into convergence.

Core principles:
1. **Curiosity before correction.** If feedback seems inconsistent, assume the LLM is missing context, not that the user is confused.
2. **Reframe, don't confront.** Instead of "you said X before," try "it sounds like the priority might be shifting toward Y — is that right?"
3. **Make the decision easy, not forced.** Reduce to the clearest possible question: "This comes down to whether you value navigation depth or content space more."
4. **Be opinionated but hold it loosely.** "I'd lean toward A, but I can see why B appeals."
5. **Name the trade-off, not the answer.** The user decides. The LLM makes the trade-off crystal clear.

**Convergence signals the LLM watches for:**

| Signal | Response |
|--------|----------|
| User keeps very similar options | "These are converging. Want me to merge the best of both?" |
| Comments are getting narrower | "Sounds like the direction is clear. Ready to lock?" |
| Round 3+ with no eliminations | "We're exploring broadly — what's the ONE thing you're uncertain about?" |
| Repeated requests for new options | "What would a new option solve that these don't? That'll help me generate something genuinely different." |
| Feedback contradicts prior round | "It sounds like the priority might be shifting. Can I ask what changed?" |

**What "locking" means:**
1. Winning option file becomes canonical for that screen
2. Decision + rationale recorded in `state.json`
3. All subsequent facets inherit this as a constraint (locked layout is baked into wireframes when critting color)
4. Overview page updates with locked status
5. User can unlock later, but LLM notes downstream impact

#### "Decide For Me" — Delegated Decisions

Not every user has an opinion on every facet. Accessibility best practices, elevation systems, loading patterns — these can be deep technical design questions where the user trusts the LLM's judgment. The compare view includes a **"Decide For Me"** button alongside Save Feedback and Skip.

**How it works:**
- Available at any point during any facet's crit round
- The user can optionally add context: "Decide for me — I care most about accessibility" or "Decide for me — keep it simple"
- The LLM picks the best option based on: the brief, locked decisions from prior facets, design best practices, and any user context provided
- The LLM explains its choice with full rationale — same quality as a regular critique, not a silent auto-pick
- The decision is recorded in `state.json` with `"decided_by": "llm"` so the user can revisit it later
- The overview page shows these with a distinct indicator (e.g., "locked (auto)" vs "locked")

**When the LLM should suggest it proactively:**
The skill teaches the LLM to offer "I can decide this one for you if you'd like" when:
- The facet is highly technical (accessibility, elevation, loading patterns)
- The user's feedback suggests low confidence ("I don't really know" / "whatever you think")
- The user is moving fast and this facet isn't core to their product's differentiation
- Prior context in the brief suggests this is a standard, not differentiating, decision

**Important: "Decide for me" is NOT "skip."** Skip means the facet isn't resolved. Decide for me means it IS resolved — by the LLM, with full rationale, reviewable and reversible. The user delegates the decision, they don't skip it.

**In feedback.json:**
```json
{
  "facet": "accessibility",
  "round": 1,
  "decision": "delegate",
  "delegate_context": "Focus on keyboard nav, I trust best practices for the rest",
  "options": {}
}
```

#### Decision History & Review

The overview page is a persistent artifact that evolves throughout the crit. Once facets start getting locked, it becomes a **decision history** — not just a progress tracker.

**Each locked facet shows:**
```
✅ Navigation — locked (Deep Sidebar)                    [Review]
✅ Color System — locked, delegated (Soft Indigo)         [Review]
🔄 Typography — round 2, 2 options surviving              [Continue]
☐  Density — pending
```

**"Review" expands the full crit history for that facet inline:**
- Which options were presented each round (with their names and rationale summaries)
- What the user said each round (feedback excerpts)
- What got eliminated and why
- The final decision and rationale
- Whether it was user-decided or LLM-delegated

This serves two purposes:
1. **During the crit:** remind yourself why you made an earlier choice when a later facet raises questions about it. "Wait, why did I go with the deep sidebar?" — click Review, see the full reasoning.
2. **After the crit:** institutional memory. When onboarding a team member or revisiting the design months later, the history explains every decision.

The decision history is also captured in `decisions.md` as a persistent markdown artifact (see Stage 4 outputs).

**Future enhancement (not v1):** Ability to unlock a facet and re-crit it, with cascade awareness for downstream dependencies. For now, if the user wants to change a locked decision, they can start a new crit targeting just that facet using the standalone facet skill.

### Stage 4: Design Direction (`design-direction`)

**Purpose:** Synthesize all locked facet decisions into a coherent design direction document.

**Outputs:**

**1. `direction.html` — The Design Direction Document**

A polished HTML page presenting the complete design direction:
- All screens with links to their final wireframe files
- Each design decision with rationale (navigation, layout, density, typography, color, etc.)
- How decisions connect to each other and to the brief
- Expandable decision log (every round, every choice, every rationale)

**2. `design-tokens.json` — Extracted Design Tokens** (optional, if relevant facets were covered)

```json
{
  "colors": { "primary": "#6366F1", "surface": "#FAFAFB" },
  "spacing": { "unit": "4px", "comfortable": "16px", "compact": "8px" },
  "typography": { "primary": "JetBrains Mono", "scale": [14, 16, 20, 24, 32] },
  "layout": { "sidebar_width": "280px", "content_max": "960px" }
}
```

**3. `decisions.md` — Full Decision Log**

Markdown file documenting every facet, round, options considered, and rationale. Institutional memory for onboarding team members or revisiting decisions.

**4. Final wireframe files**

Each screen's winning option at whatever fidelity the crit reached. The real deliverable — HTML files ready to reference during implementation.

**Handoff to implementation:**

The design direction becomes the source of truth for building. When the user starts implementing (with `frontend-design` skill or manually), they have visual direction per screen, design tokens, and rationale for every decision.

## State Management

### `state.json` Schema

```json
{
  "project": {
    "name": "Remarc",
    "description": "Personal knowledge manager for developers",
    "platform": "web",
    "tech_stack": ["React", "Tailwind"]
  },
  "brief_status": "confirmed",
  "facets": [
    {
      "id": "screen-inventory",
      "skill": "crit-screen-inventory",
      "name": "Screen Inventory & Flows",
      "rationale": "Everything depends on knowing what surfaces exist",
      "depends_on": [],
      "status": "locked",
      "locked_option": "option-b",
      "locked_summary": "7 screens: Dashboard, Capture (modal + full), Search, Collections, Tags, Settings, Onboarding",
      "rounds_completed": 2,
      "enabled": true
    },
    {
      "id": "navigation-model",
      "skill": "crit-navigation",
      "name": "Navigation Model",
      "rationale": "Constrains all layout decisions downstream",
      "depends_on": ["screen-inventory"],
      "status": "in-progress",
      "current_round": 2,
      "surviving_options": ["option-a", "option-c"],
      "enabled": true
    },
    {
      "id": "content-layout",
      "skill": "crit-layout",
      "name": "Content Layout",
      "rationale": "Core editor/list/detail arrangement",
      "depends_on": ["screen-inventory", "navigation-model"],
      "status": "pending",
      "enabled": true
    }
  ],
  "current_facet": "navigation-model",
  "current_round": 2
}
```

## Distribution

**Claude Code plugin on GitHub.** Users install via:

```
claude plugin add github:username/design-crit
```

Public repo, auto-updates, standard distribution path. Free and open source.

## Design Principles (Encoded in the Skill)

These are the meta-principles the skill teaches the LLM to apply. They are not shown to the user — they guide the LLM's behavior throughout.

### First Principles for Facet Selection
- Select from the pre-built facet skill library based on project type and brief
- Dependency ordering: structural before visual, decisions that constrain come first
- Three lenses: what exists → how it's arranged → how it feels
- Add project-specific custom facets via `crit-custom` when the library doesn't cover a need
- Not every facet applies to every project — a marketing site doesn't need `crit-data-visualization`

### First Principles for Generating Options
- The LLM decides how many options per round based on the divergence needed
- Each option must have a clear reason to exist — not variations for variation's sake
- Name options with characterizing labels, not letters
- Present honest trade-offs, not just pros

### First Principles for Convergence
- Curiosity before correction
- Reframe, don't confront
- Make the decision easy, not forced
- Be opinionated but hold it loosely
- Name the trade-off, not the answer
- Bias toward understanding WHY the user is giving this feedback

### First Principles for the Brief
- The brief is a scoping tool, not a brainstorming tool
- Show what you know and highlight what you don't — be honest about gaps
- Purposeful friction: a few good questions produce dramatically better wireframes
- The brief is the first deliverable, not a gate to clear

## Research Foundation

### Design Process Research
- Google Design's three-stage review model (Early Conceptual → Mid-Stage Mocks → Late-Stage Build)
- Design Council's Double Diamond (Discover → Define → Develop → Deliver)
- IDEO/Stanford d.school Design Thinking (Empathize → Define → Ideate → Prototype → Test)
- Brad Frost's Atomic Design (Atoms → Molecules → Organisms → Templates → Pages)
- Nielsen Norman Group's design critique best practices (clear scope, agreed objectives, conversation over commands)
- Figma's internal critique culture (safe space, multiple critique formats, focused sessions)
- Nielsen's 10 Usability Heuristics as universal evaluation lens

### Fidelity Progression
- Paper sketches → Low-fi wireframes → Mid-fi wireframes → High-fi mockups → Interactive prototypes
- Each level has distinct decisions and review focus
- The plugin operates primarily in the low-fi to mid-fi range, gaining fidelity as facets progress

### Architecture Research
- Superpowers plugin pipeline pattern (separate skills, filesystem state, explicit handoffs)
- Token efficiency: only load current stage's SKILL.md (~150-300 lines each)
- Progressive disclosure: reference files loaded on demand
- State through filesystem artifacts, not built-in mechanisms
