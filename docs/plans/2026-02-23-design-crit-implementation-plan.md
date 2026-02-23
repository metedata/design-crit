# Design Crit Plugin — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Code plugin with 24 skills that guides users through a structured design critique process, generating HTML wireframe artifacts for iterative review.

**Architecture:** Multi-skill plugin with orchestrator. 4 pipeline skills + 19 pre-built facet skills + 1 generic facet skill + 3 shared reference files. State flows through `.design-crit/` directory with JSON and HTML files.

**Tech Stack:** Claude Code plugin (YAML frontmatter + Markdown SKILL.md files), HTML/CSS for artifacts, JSON for state

---

## Phase 1: Plugin Foundation

### Task 1: Plugin Scaffolding

**Files:**
- Create: `plugin.json`
- Create: `README.md`

**Step 1: Create plugin.json**

```json
{
  "name": "design-crit",
  "description": "Structured design critique process that generates HTML wireframe artifacts for iterative, side-by-side review. Guides users from idea through brief, screen inventory, and visual design facets to a complete design direction.",
  "author": {
    "name": "Mete Polat"
  }
}
```

**Step 2: Create README.md**

Write a concise README covering:
- What the plugin does (1 paragraph)
- Installation: `claude plugin add github:metepolat/design-crit`
- Quick start: invoke `/design-crit` and follow the prompts
- List of included skills (orchestrator, pipeline stages, facet library)
- Link to the design document

**Step 3: Create directory structure**

```bash
mkdir -p skills/{design-crit,design-brief,facet-planning,design-direction}
mkdir -p skills/{crit-screen-inventory,crit-edge-states,crit-onboarding-flows}
mkdir -p skills/{crit-navigation,crit-layout,crit-content-hierarchy,crit-component-design}
mkdir -p skills/{crit-elevation-shape,crit-data-visualization,crit-responsive}
mkdir -p skills/{crit-typography,crit-color-system,crit-density-spacing,crit-iconography}
mkdir -p skills/{crit-motion,crit-content-voice,crit-imagery-illustration,crit-loading-performance}
mkdir -p skills/{crit-accessibility,crit-custom}
mkdir -p reference
```

**Step 4: Commit**

```bash
git add plugin.json README.md skills/ reference/
git commit -m "feat: scaffold design-crit plugin structure"
```

---

### Task 2: Shared Reference — `crit-loop.md`

**Files:**
- Create: `reference/crit-loop.md`

This is the most critical reference file. Every `crit-*` facet skill loads this for the shared crit loop mechanics.

**Step 1: Write `reference/crit-loop.md`**

Content must cover (target: 200-300 lines):

1. **Compare view HTML generation**
   - Side-by-side layout rules (2 options → 50/50, 3 → thirds, 4 → 2x2 grid)
   - Option card structure: iframe + name + rationale (WHY / WORKS WELL FOR / WATCH OUT FOR) + Keep/Cut + comment
   - Comparative take section below all cards
   - Global feedback area: direction notes, Save Feedback, Skip, Decide For Me
   - Dark mode toggle, filter controls (All/Surviving/Eliminated)
   - Context banner: project name, facet, round number

2. **Individual option file requirements**
   - Self-contained HTML with inline CSS, no external dependencies
   - Must render correctly both iframed and standalone
   - Files persist across rounds, get refined in place

3. **Feedback round-trip mechanics**
   - Read state.json → read previous feedback → read surviving options → generate/refine → write compare.html → open browser
   - Feedback JSON schema (facet, round, per-option action/comment, overall, decision)
   - Save Feedback implementation: File System Access API with copy-paste fallback

4. **Convergence guidance**
   - Default rhythm: Round 1 diverge, Round 2 converge, Round 3 polish (optional)
   - Escalating lock prompts per round
   - Convergence tone principles: curiosity before correction, reframe don't confront, name the trade-off not the answer, be opinionated but hold it loosely
   - Convergence signals table

5. **"Decide For Me" mechanics**
   - When to offer proactively (technical facets, low-confidence feedback, non-differentiating decisions)
   - Must explain choice with full rationale
   - Record as `decided_by: llm` in state
   - Distinct indicator in overview

6. **Locking mechanics**
   - Winner becomes canonical, decision + rationale in state.json
   - Locked decisions carry forward as constraints for later facets
   - Overview page update with Review button for decision history

**Step 2: Commit**

```bash
git add reference/crit-loop.md
git commit -m "feat: add shared crit loop mechanics reference"
```

---

### Task 3: Shared Reference — `brief-template.md`

**Files:**
- Create: `reference/brief-template.md`

**Step 1: Write `reference/brief-template.md`**

Content (target: 80-120 lines):

1. **The brief schema** — what fields an optimal brief contains:
   - Project name and one-line description
   - Target users and context of use (who, where, when)
   - Core interaction loop (the one thing users do repeatedly)
   - Differentiator (what makes THIS one different)
   - Platform and constraints (web/native/both, tech stack, device targets)
   - Scope (in v1 / explicitly out of v1)
   - Existing design language or references (if any)
   - Accessibility requirements (if known)

2. **Evaluation rubric** — how to score context sufficiency:
   - Each field: filled / partially filled / empty
   - Sufficient = core loop + platform + scope are filled
   - Partial = some fields filled, targeted questions can close gaps
   - Minimal = most fields empty, guided brief creation needed

3. **Example briefs** — two contrasting examples:
   - Rich brief (from a project with existing codebase)
   - Minimal brief (from a vague idea)

**Step 2: Commit**

```bash
git add reference/brief-template.md
git commit -m "feat: add brief template reference"
```

---

### Task 4: Shared Reference — `design-principles.md`

**Files:**
- Create: `reference/design-principles.md`

**Step 1: Write `reference/design-principles.md`**

Content (target: 100-150 lines):

1. **First principles for facet selection**
   - The three lenses: what exists → how it's arranged → how it feels
   - Dependency ordering: structural before visual
   - The full facet library listing with universal/contextual flags
   - Default pipeline order template

2. **First principles for generating options**
   - LLM decides option count based on divergence needed
   - Each option must have a clear reason to exist
   - Named characterization, not letters
   - Honest trade-offs, not just pros

3. **First principles for the designer-at-crit persona**
   - Present each option with conviction and rationale
   - Be opinionated but in service of the user's decision
   - Surface the trade-off clearly, let the user decide
   - Curiosity before correction when feedback seems contradictory

**Step 2: Commit**

```bash
git add reference/design-principles.md
git commit -m "feat: add shared design principles reference"
```

---

## Phase 2: Pipeline Skills

### Task 5: Orchestrator — `design-crit/SKILL.md`

**Files:**
- Create: `skills/design-crit/SKILL.md`

**Step 1: Write the orchestrator skill**

Target: 150-200 lines. This is the primary entry point users invoke.

```yaml
---
name: design-crit
description: Use when starting a design critique, visual design review, or wireframing process for any app or service. Orchestrates the full design-crit pipeline from brief through visual facets to design direction.
---
```

Content must cover:
1. **Overview** — what the plugin does, the pipeline stages
2. **Routing logic** — read `.design-crit/state.json`, determine current stage:
   - No state.json → invoke `design-crit:design-brief`
   - Brief confirmed, no facet plan → invoke `design-crit:facet-planning`
   - Facet plan confirmed, facets in progress → invoke the current facet's `crit-*` skill
   - All facets locked → invoke `design-crit:design-direction`
   - Already complete → show summary, offer to revisit
3. **State initialization** — create `.design-crit/` directory and initial `state.json` if it doesn't exist
4. **Overview page** — generate/regenerate `overview.html` showing current progress, facet checklist, decision history with Review buttons
5. **Re-entry handling** — detect when user comes back mid-crit, pick up where they left off

**Step 2: Commit**

```bash
git add skills/design-crit/SKILL.md
git commit -m "feat: add design-crit orchestrator skill"
```

---

### Task 6: Stage 1 — `design-brief/SKILL.md`

**Files:**
- Create: `skills/design-brief/SKILL.md`

**Step 1: Write the design-brief skill**

Target: 200-300 lines.

```yaml
---
name: design-brief
description: Use when starting a new design project, when context seems insufficient for design decisions, or when explicitly gathering requirements before visual design work.
---
```

Content must cover:
1. **Overview** — the brief is a scoping tool, not brainstorming. Purposeful friction.
2. **Context harvesting** — what to read from the codebase:
   - Package.json / config files → tech stack
   - Route files / page directories → existing screens
   - Component directories → existing UI patterns
   - README, docs → project description
   - User's verbal description
3. **Evaluation against brief template** — reference `brief-template.md` for the schema and rubric
4. **Three branches:**
   - Sufficient → draft brief, generate overview.html showing it, ask for confirmation
   - Partial → draft what's available, ask 2-3 targeted questions (use AskUserQuestion)
   - Minimal → guided brief creation, one question at a time
5. **Output** — write `.design-crit/brief.md`, update `state.json` with `brief_status: confirmed`
6. **Handoff** — announce completion, invoke `design-crit:facet-planning`

**Step 2: Commit**

```bash
git add skills/design-brief/SKILL.md
git commit -m "feat: add design-brief skill"
```

---

### Task 7: Stage 2 — `facet-planning/SKILL.md`

**Files:**
- Create: `skills/facet-planning/SKILL.md`

**Step 1: Write the facet-planning skill**

Target: 200-300 lines.

```yaml
---
name: facet-planning
description: Use when a design brief is confirmed and the project needs a plan for which design facets to evaluate and in what order.
---
```

Content must cover:
1. **Overview** — derive facets from the brief using first principles, not a fixed list
2. **Reference** — load `design-principles.md` for the facet library and selection framework
3. **Analysis process:**
   - Read brief.md → identify project type, platform, complexity, scope
   - Select relevant facets from the pre-built library (list all 19 with descriptions)
   - Determine dependency ordering (reference the three lenses + dependency rules)
   - Add custom facets if needed (route to `crit-custom`)
4. **Overview HTML generation** — generate `overview.html` showing:
   - Brief summary at top
   - Proposed facets with rationale, dependency indicators, skill mapping
   - Checkboxes to enable/disable, drag to reorder, add custom
   - Confirm button
5. **User confirmation** — present in terminal via AskUserQuestion with the proposed plan, or let user interact with overview.html
6. **Output** — update `state.json` with confirmed facet plan including skill mappings per facet
7. **Handoff** — announce plan confirmed, start first facet via orchestrator

**Step 2: Commit**

```bash
git add skills/facet-planning/SKILL.md
git commit -m "feat: add facet-planning skill"
```

---

### Task 8: Stage 4 — `design-direction/SKILL.md`

**Files:**
- Create: `skills/design-direction/SKILL.md`

**Step 1: Write the design-direction skill**

Target: 150-200 lines.

```yaml
---
name: design-direction
description: Use when all design facets are locked and the crit is ready to be synthesized into a final design direction document with deliverables.
---
```

Content must cover:
1. **Overview** — synthesize all locked facet decisions into a coherent direction
2. **Prerequisite check** — verify all enabled facets are locked in state.json
3. **Deliverable generation:**
   - `direction.html` — polished HTML page with all screens, design decisions + rationale, links to final wireframes, expandable decision log
   - `design-tokens.json` — extract tokens from relevant locked facets (color, typography, spacing, layout) if those facets were covered
   - `decisions.md` — full markdown decision log (every facet, round, option, rationale)
4. **Final overview update** — regenerate overview.html showing completed state with all Review buttons
5. **Handoff suggestion** — "Ready to implement? Your project uses [tech stack]. The `frontend-design` skill can use these wireframes as reference."

**Step 2: Commit**

```bash
git add skills/design-direction/SKILL.md
git commit -m "feat: add design-direction skill"
```

---

## Phase 3: Core Facet Skills (Universal)

Each facet skill follows the same structure. They all:
- Reference `crit-loop.md` for shared mechanics
- Have domain-specific evaluation criteria, artifact types, and trade-off knowledge
- Stay under 500 lines (target 200-350 per skill)

Tasks 9-20 can be **parallelized** — they are independent of each other.

### Task 9: `crit-screen-inventory/SKILL.md`

**Files:**
- Create: `skills/crit-screen-inventory/SKILL.md`

```yaml
---
name: crit-screen-inventory
description: Use when identifying and evaluating the complete set of screens, states, and user flows for an app or service. Usually the first facet in a design crit.
---
```

**Domain-specific content:**
- Artifact type: flow diagrams, screen maps, state inventories (NOT wireframes yet)
- How to derive screens from the brief's core loop
- State enumeration checklist: default, empty, error, loading, edge cases per screen
- User flow mapping: happy path + error paths + edge paths
- Common patterns: minimal MVP vs full suite trade-offs
- Evaluation criteria: completeness (are all tasks covered?), necessity (does each screen earn its place?), flow efficiency (minimum steps to complete core tasks)

**Step 1: Write the skill. Step 2: Commit.**

---

### Task 10: `crit-edge-states/SKILL.md`

```yaml
---
name: crit-edge-states
description: Use when designing empty states, error handling, offline behavior, loading content, and boundary conditions for an app's screens.
---
```

**Domain-specific content:**
- Artifact type: state-specific wireframes showing what users see in non-happy-path scenarios
- Empty state patterns: first-use empty, cleared empty, no-results empty (different approaches for each)
- Error state hierarchy: inline validation, toast/banner, full-page error, recovery actions
- Offline behavior: cached content, sync indicators, degraded functionality
- Loading states: placeholder content, skeleton screens, progress indicators (coordinate with `crit-loading-performance` if both are in the plan)
- Edge cases: permission denied, expired session, rate limiting, browser incompatibility
- Evaluation criteria: does every screen have its edge states defined? Are error messages actionable? Do empty states guide the user toward the happy path?

---

### Task 11: `crit-navigation/SKILL.md`

```yaml
---
name: crit-navigation
description: Use when evaluating navigation patterns, wayfinding, menu structures, and how users move between screens in an app.
---
```

**Domain-specific content:**
- Artifact type: navigation wireframes, sitemap diagrams, menu structure mockups
- Navigation pattern vocabulary: sidebar, top bar, tab bar, hamburger, breadcrumbs, command palette, rail
- Platform conventions: mobile tab bar (iOS/Android), desktop sidebar, web top nav
- Depth vs breadth trade-off: flat navigation (many items visible) vs deep hierarchy (fewer items, nested)
- Evaluation criteria: can the user reach any screen in N taps? Is the current location always clear? Does the nav scale with content growth?

---

### Task 12: `crit-layout/SKILL.md`

```yaml
---
name: crit-layout
description: Use when evaluating page layout, grid systems, spatial composition, and how content areas relate to each other within screens.
---
```

**Domain-specific content:**
- Artifact type: layout wireframes with grid overlays, spatial arrangement mockups
- Grid systems: 12-column, flexible, content-driven
- Common layout patterns: holy grail, sidebar+content, split-pane, card grid, single-column
- Spatial relationships: fixed vs fluid, proportional splits (70/30, 60/40)
- Content area hierarchy: primary, secondary, auxiliary, chrome
- Evaluation criteria: does the layout support the content hierarchy? Does it degrade gracefully? Is the grid consistent across screens?

---

### Task 13: `crit-content-hierarchy/SKILL.md`

```yaml
---
name: crit-content-hierarchy
description: Use when evaluating information hierarchy within screens — what's prominent, what's secondary, and how the user's eye is guided through content.
---
```

**Domain-specific content:**
- Artifact type: wireframes with emphasis annotations, reading order diagrams
- Visual hierarchy tools: size, weight, color, position, spacing, contrast
- F-pattern and Z-pattern reading flows
- Progressive disclosure: what's visible immediately vs on demand
- CTA prominence and placement
- Evaluation criteria: can a user understand the page purpose in 5 seconds? Is the primary action obvious? Is the reading order logical?

---

### Task 14: `crit-component-design/SKILL.md`

```yaml
---
name: crit-component-design
description: Use when evaluating interactive component patterns — buttons, forms, cards, modals, tables, and other UI building blocks.
---
```

**Domain-specific content:**
- Artifact type: component wireframes, interaction state diagrams
- Component categories: input (forms, selectors, toggles), display (cards, lists, tables), feedback (toasts, alerts, progress), overlay (modals, drawers, popovers)
- State coverage per component: default, hover, active, disabled, error, loading
- Platform conventions: native vs web component expectations
- Form design patterns: inline validation, grouped fields, progressive forms
- Evaluation criteria: are interactive states defined? Do components follow platform conventions? Is the component inventory consistent?

---

### Task 15: `crit-elevation-shape/SKILL.md`

```yaml
---
name: crit-elevation-shape
description: Use when evaluating depth, layering, shadow systems, surface treatments, and shape language (border radius, corner treatment) in a design.
---
```

**Domain-specific content:**
- Artifact type: elevation diagrams, surface layer mockups, shape language specimens
- Z-axis strategy: how many elevation levels, what lives at each
- Shadow token systems: size/blur/spread/color by elevation
- Surface treatments: flat, elevated, glass/blur, frosted, material
- Shape language: sharp vs rounded, radius scale, consistency rules
- Overlay hierarchy: how modals, popovers, tooltips layer
- Evaluation criteria: does elevation communicate hierarchy? Is the shadow system consistent? Does the shape language match the brand tone?

---

### Task 16: `crit-typography/SKILL.md`

```yaml
---
name: crit-typography
description: Use when evaluating font selection, type scale, typographic hierarchy, and text readability in a design.
---
```

**Domain-specific content:**
- Artifact type: type specimens, hierarchy scales, paragraph samples
- Font selection criteria: readability, brand alignment, variable font support, license
- Type scale: modular scale ratios (1.25, 1.333, 1.5), number of levels needed
- Hierarchy through weight, size, color, spacing — not just size alone
- Line height, letter spacing, paragraph width (45-75 characters ideal)
- System font stacks vs custom fonts (performance trade-off)
- Evaluation criteria: is the hierarchy clear at a glance? Is body text comfortable to read? Does the type scale have enough (but not too many) levels?

---

### Task 17: `crit-color-system/SKILL.md`

```yaml
---
name: crit-color-system
description: Use when evaluating color palettes, theming, dark mode, contrast, and color tokens for a design system.
---
```

**Domain-specific content:**
- Artifact type: palette swatches, theme previews (light/dark/compare), applied color mockups
- Color system structure: brand colors, surface colors, text colors, status colors (success/warning/error/info)
- Contrast requirements: WCAG AA minimum (4.5:1 text, 3:1 large text/UI)
- Dark mode: not just inverted — separate surface scale, adjusted saturation, contrast preservation
- Color token naming: semantic (surface-primary, text-secondary) vs literal (blue-500)
- Emotional tone: warm vs cool, saturated vs muted, monochromatic vs complementary
- Evaluation criteria: do all text/background combinations pass contrast? Does the palette support the brand tone? Does dark mode feel intentional, not inverted?

---

### Task 18: `crit-density-spacing/SKILL.md`

```yaml
---
name: crit-density-spacing
description: Use when evaluating information density, whitespace, spacing systems, and compact vs comfortable modes in a design.
---
```

**Domain-specific content:**
- Artifact type: density comparison wireframes (comfortable vs compact side by side)
- Spacing scale: base unit (4px or 8px), multiplier scale
- Touch target minimums: 44x44pt (Apple), 48x48dp (Material)
- Information density spectrum: spacious (marketing) → comfortable (consumer app) → compact (power tool) → dense (data dashboard)
- Breathing room: when whitespace serves a purpose vs when it wastes space
- Compact mode patterns: toggle, progressive density, responsive density
- Evaluation criteria: is the density appropriate for the user context? Are touch targets accessible? Is the spacing scale consistent?

---

### Task 19: `crit-content-voice/SKILL.md`

```yaml
---
name: crit-content-voice
description: Use when evaluating microcopy, error messages, CTA language, terminology consistency, and the overall voice and tone of UI text.
---
```

**Domain-specific content:**
- Artifact type: microcopy specimens, tone comparison boards, terminology glossaries
- Voice dimensions: formal ↔ casual, serious ↔ playful, technical ↔ simple, authoritative ↔ friendly
- Microcopy areas: button labels, placeholder text, tooltips, helper text, error messages, empty states, confirmation dialogs
- Error message formula: what happened + why + what to do next
- CTA language: action verbs, specificity ("Save changes" not "Submit"), urgency calibration
- Terminology consistency: pick one term and use it everywhere
- Evaluation criteria: is the tone consistent across screens? Are error messages actionable? Do CTAs clearly communicate what happens next?

---

### Task 20: `crit-accessibility/SKILL.md`

```yaml
---
name: crit-accessibility
description: Use when auditing a design for accessibility — keyboard navigation, screen reader support, touch targets, focus management, color contrast, and cognitive load. Typically runs as the last facet to audit accumulated decisions.
---
```

**Domain-specific content:**
- Artifact type: accessibility audit reports, remediation wireframes, annotated screenshots
- Keyboard navigation: tab order, focus indicators, skip links, keyboard shortcuts
- Screen reader support: semantic HTML structure, ARIA landmarks, live regions, alt text
- Touch targets: minimum sizes per platform
- Color: contrast ratios (reference locked color system), color-blindness simulation
- Motion: reduced motion alternatives, vestibular sensitivity
- Cognitive accessibility: reading level, cognitive load, clear wayfinding
- This facet runs LAST to audit all prior locked decisions
- Evaluation criteria: WCAG AA compliance, keyboard-only usability, screen reader navigability

---

**Commit all universal facet skills:**

```bash
git add skills/crit-*/SKILL.md
git commit -m "feat: add 12 universal facet skills"
```

---

## Phase 4: Contextual Facet Skills

Tasks 21-27 can be **parallelized**.

### Task 21: `crit-data-visualization/SKILL.md`

```yaml
---
name: crit-data-visualization
description: Use when evaluating chart types, data display patterns, dashboard layouts, and how data is visually communicated to users.
---
```

Domain focus: chart selection (bar/line/pie/scatter/heatmap), data encoding (color/size/position), dashboard composition, annotation patterns, interactive exploration patterns, data table design.

---

### Task 22: `crit-responsive/SKILL.md`

```yaml
---
name: crit-responsive
description: Use when evaluating how a design adapts across screen sizes and devices — breakpoint strategy, layout reflow, and touch vs pointer adaptation.
---
```

Domain focus: breakpoint strategy (mobile-first vs desktop-first), layout reflow patterns (stack/reorder/hide/simplify), navigation adaptation, touch vs pointer accommodation, image/media scaling.

---

### Task 23: `crit-iconography/SKILL.md`

```yaml
---
name: crit-iconography
description: Use when evaluating icon systems, visual metaphors, icon style consistency, and symbolic language in a design.
---
```

Domain focus: icon style (outlined/filled/duotone), optical sizing, metaphor clarity, consistency across the set, icon+label pairing, custom vs library icons.

---

### Task 24: `crit-motion/SKILL.md`

```yaml
---
name: crit-motion
description: Use when evaluating animation, transitions, micro-interactions, and motion design in a user interface.
---
```

Domain focus: easing curves, duration scales, enter/exit/transition patterns, scroll-triggered animation, loading animations, hover/focus micro-interactions, reduced motion alternatives.

---

### Task 25: `crit-imagery-illustration/SKILL.md`

```yaml
---
name: crit-imagery-illustration
description: Use when evaluating photography style, illustration systems, hero visuals, and decorative imagery for a product or marketing site.
---
```

Domain focus: photography style (mood, color grading, subject matter), illustration style (line weight, color, character design), hero imagery patterns, avatar display, empty state illustrations, stock vs custom.

---

### Task 26: `crit-loading-performance/SKILL.md`

```yaml
---
name: crit-loading-performance
description: Use when evaluating loading states, perceived performance patterns, skeleton screens, optimistic UI, and progress indicators.
---
```

Domain focus: skeleton screens vs spinners, optimistic UI patterns, progressive loading, progress indicators for long operations, transition states between views, offline/sync indicators.

---

### Task 27: `crit-onboarding-flows/SKILL.md`

```yaml
---
name: crit-onboarding-flows
description: Use when evaluating first-run experience, progressive disclosure, onboarding tutorials, and how new users learn to use the product.
---
```

Domain focus: first-use experience, progressive disclosure, tooltip tours, feature discovery, empty state onboarding, permission requests, sample/demo content, account setup flows.

---

### Task 28: `crit-custom/SKILL.md`

```yaml
---
name: crit-custom
description: Use when a project needs a design facet not covered by the pre-built facet library. Provides crit loop mechanics with design principles but no domain-specific guidance.
---
```

Content: reference `crit-loop.md` and `design-principles.md`. Instruct the LLM to derive domain knowledge from its training for the specific facet topic. Same compare view, same feedback loop, same convergence mechanics — just without pre-built evaluation criteria.

---

**Commit all contextual facet skills:**

```bash
git add skills/crit-*/SKILL.md
git commit -m "feat: add 8 contextual and generic facet skills"
```

---

## Phase 5: End-to-End Testing

### Task 29: Test the full pipeline with a sample project

**Step 1:** Create a test project directory with a minimal codebase (e.g., a simple React app with a few routes)

**Step 2:** Invoke `/design-crit` and walk through the full pipeline:
- Brief creation → facet planning → 2-3 facets of actual crit → design direction
- Verify state.json updates correctly at each stage
- Verify overview.html renders and is interactive
- Verify compare.html shows options side by side with working feedback forms
- Verify feedback.json round-trip works
- Test "Decide For Me" on one facet
- Test "Skip" on one facet
- Verify design-direction produces all deliverables

**Step 3:** Document any issues and iterate on skill content

**Step 4: Commit any fixes**

```bash
git add -A
git commit -m "fix: address issues from end-to-end testing"
```

---

### Task 30: Test standalone facet invocation

**Step 1:** Invoke `/crit-color-system` directly (without full pipeline) on a project
- Verify it works independently
- Verify it generates proper compare.html and feedback mechanics

**Step 2:** Invoke `/crit-screen-inventory` directly
- Verify it works for structural (non-visual) facets

**Step 3: Commit any fixes**

---

### Task 31: Final polish and publish

**Step 1:** Review all SKILL.md files for:
- Under 500 lines each
- Consistent terminology
- No stale references
- Descriptions start with "Use when..." and don't summarize workflow

**Step 2:** Update README with final skill list and any usage notes from testing

**Step 3:** Initialize git repo and push to GitHub

```bash
git init
git add -A
git commit -m "feat: design-crit plugin v1.0"
git remote add origin git@github.com:metepolat/design-crit.git
git push -u origin main
```

**Step 4:** Test installation

```bash
claude plugin add github:metepolat/design-crit
```

---

## Execution Notes

**Parallelization opportunities:**
- Tasks 9-20 (universal facet skills) are fully independent — can run in parallel
- Tasks 21-28 (contextual facet skills) are fully independent — can run in parallel
- Phase 1 and 2 are sequential (reference files before skills that reference them)

**Estimated task count:** 31 tasks across 5 phases
- Phase 1: 4 tasks (foundation)
- Phase 2: 4 tasks (pipeline skills)
- Phase 3: 12 tasks (universal facets)
- Phase 4: 8 tasks (contextual facets + generic)
- Phase 5: 3 tasks (testing + publish)

**Each facet skill task:** Write the SKILL.md following the structure and domain content outlined above. Reference `crit-loop.md` for shared mechanics. Target 200-350 lines. Stay under 500 lines.
