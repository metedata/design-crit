---
name: crit-content-hierarchy
description: Use when evaluating information hierarchy within screens — what's prominent, what's secondary, and how the user's eye is guided through content.
---

# Content Hierarchy Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are critting content hierarchy -- how information is prioritized, emphasized, and sequenced within each screen. This facet determines what the user sees first, what they see next, and what they have to hunt for. Good hierarchy makes a screen self-explanatory in seconds. Bad hierarchy makes every screen a puzzle.

Content hierarchy sits in the compositional lens. It depends on `content-layout` (the spatial grid must exist before you can assign emphasis within it) and feeds into sensory facets like `typography`, `color-system`, and `density-spacing` which implement the hierarchy through visual means.

---

## 1. Load Context

Before generating any options, read:

1. `.design-crit/state.json` -- current facet state, round number, locked decisions
2. `.design-crit/brief.md` -- project purpose, target users, core loop
3. `.design-crit/facets/content-hierarchy/feedback-round-{N-1}.json` -- prior feedback (skip for round 1)
4. All locked option files from prior facets -- especially `content-layout` (the grid you are working within) and `navigation-model` (what navigation chrome exists)

State what you are building on: "Building on the locked layout: [summary]. Navigation is [summary]. Hierarchy decisions apply within this structure."

---

## 2. Artifact Type

Generate **wireframes with emphasis annotations and reading order diagrams**.

Each option file (`option-{x}.html`) must contain:

### Screen Wireframes

Render wireframes for the primary screens identified in the screen inventory. Use the locked layout grid as the structural foundation. Do NOT reinvent the layout -- place content within it.

Within each screen wireframe, convey hierarchy through:

- **Size variation** -- larger elements signal higher importance
- **Visual weight** -- bolder strokes, filled vs outlined regions
- **Position** -- top-left (in LTR) carries natural priority; above the fold matters
- **Spacing** -- more whitespace around an element elevates it
- **Contrast** -- higher contrast elements read first
- **Grouping** -- related items cluster; separation creates distinct zones

Use grayscale only. This facet is about structural emphasis, not color. Reserve color hierarchy for the `color-system` facet.

### Reading Order Overlay

Include a numbered reading order overlay on each wireframe. Draw numbered circles (1, 2, 3...) connected by a light dashed path showing the intended eye flow. This makes the hierarchy explicit and reviewable.

### Emphasis Key

Below each wireframe, include a legend:

| Level | Role | Visual Treatment |
|---|---|---|
| Primary | Page title, hero content, main CTA | Largest, heaviest, most space |
| Secondary | Section headings, key data, supporting actions | Medium size, clear grouping |
| Tertiary | Body content, descriptions, metadata | Standard weight, comfortable reading |
| Ambient | Timestamps, fine print, system info | Smallest, lowest contrast |

Map each element in the wireframe to an emphasis level.

---

## 3. Evaluation Criteria

Score each option against these questions. State your assessment in the critique.

### The 5-Second Test

Can a new user understand the page purpose and find the primary action within 5 seconds of seeing the screen? If the answer is no, the hierarchy has failed at its most basic job.

For each screen, articulate in one sentence: "This page is for [purpose] and the first thing you do is [primary action]." If that sentence is not obvious from the wireframe alone, the hierarchy needs work.

### Primary Action Prominence

Is the primary CTA (call to action) the most visually prominent interactive element on the screen? Check:

- Is it in a high-attention zone (top-right for desktop, bottom for mobile, or inline with the content flow)?
- Is there visual competition from other elements at the same emphasis level?
- On screens with multiple CTAs, is there a clear primary/secondary/tertiary distinction?

### Reading Order Logic

Does the numbered reading path follow a logical sequence for the task?

- **Task-oriented screens** (forms, editors, flows): reading order should match task sequence. The user reads what they need to know, then acts.
- **Information screens** (dashboards, profiles, detail views): reading order should match information priority. Most important metric or content first.
- **Decision screens** (pricing, comparison, selection): reading order should frame the decision, then present options, then guide toward action.

### Competing Emphasis

Are there elements fighting for the same attention level? Common problems:

- Two CTAs with equal visual weight when one should dominate
- A sidebar that competes with main content for attention
- Decorative elements that distract from functional ones
- Navigation that overwhelms content
- Multiple "hero" zones splitting focus

### Progressive Disclosure

Is the right amount of information visible immediately vs available on demand?

- Critical information visible without interaction
- Supporting details available one click/tap away
- Edge case content (help text, advanced settings) appropriately buried
- No information overload on first view

---

## 4. Reading Flow Patterns

Use these as reference when evaluating and generating options. Not every screen fits a pattern -- but deviations should be intentional.

### F-Pattern

Users scan horizontally across the top, then down the left side with shorter horizontal scans. Best for:

- Text-heavy pages (articles, documentation)
- List-based interfaces (email, feeds, search results)
- Admin dashboards with left navigation

Place the most important content in the top-left quadrant and along the left edge. Secondary content fills the right side and lower portions.

### Z-Pattern

Users scan top-left to top-right, diagonal to bottom-left, then to bottom-right. Best for:

- Marketing pages and landing pages
- Simple task screens with minimal content
- Login/signup forms
- Call-to-action-focused pages

Place brand/context top-left, key visual top-right, supporting info bottom-left, primary CTA bottom-right.

### Hub-and-Spoke

A central focal point with satellite elements radiating outward. Best for:

- Dashboard screens with a primary metric
- Profile or entity detail pages
- Product pages with a hero image

Place the most important element (metric, image, title) at the visual center. Supporting elements orbit with decreasing emphasis.

### Gutenberg Diagram

Divides the page into four quadrants with predictable attention: primary (top-left), strong fallow (top-right), weak fallow (bottom-left), terminal (bottom-right). Best for:

- Pages with evenly distributed content
- Grid-based layouts (cards, tiles)

Place key content in the primary area and the CTA in the terminal area.

---

## 5. Common Hierarchy Trade-offs

Present these trade-offs when they apply to the user's project. Name the tension explicitly.

### Density vs Clarity

- **Dense hierarchy:** More information visible, less scrolling, efficient for expert users. Risk: cognitive overload, no clear entry point.
- **Sparse hierarchy:** Clear focus, breathable, great for onboarding. Risk: excessive scrolling, hidden important information.
- **Question to ask:** "Are your users scanning to find something specific, or being guided through a flow?"

### Content-First vs Action-First

- **Content-first:** Headline, description, and context dominate. Actions follow understanding. Works for editorial, detail views, education.
- **Action-first:** CTA is the hero. Content supports the action decision. Works for transactional screens, dashboards with clear tasks.
- **Question to ask:** "Does the user need to understand before acting, or are they coming here to act?"

### Flat vs Layered Hierarchy

- **Flat:** 2-3 emphasis levels. Everything feels roughly equal. Simple, democratic, works for homogeneous content (card grids, settings lists).
- **Layered:** 4-5 emphasis levels. Clear primary/secondary/tertiary/ambient. Guides the eye precisely. Works for complex screens with mixed content types.
- **Question to ask:** "Are the content elements roughly equal in importance, or is there a clear star of the show?"

### Fixed vs Contextual Emphasis

- **Fixed emphasis:** The same elements are always prominent regardless of state. Predictable, learnable.
- **Contextual emphasis:** Emphasis shifts based on what matters now (active task, new data, alerts). Dynamic, responsive to state.
- **Question to ask:** "Does what matters most change based on what the user is doing?"

---

## 6. Option Generation Guidance

### Round 1: Diverge

Generate 2-4 options exploring genuinely different hierarchy strategies. Each option must answer a different question about what matters most on each screen.

Useful axes of variation for round 1:

- **What is the hero?** One option leads with the primary data/metric, another leads with the primary action, another leads with context/navigation.
- **How deep is the hierarchy?** One option uses 2 levels (flat, democratic), another uses 4-5 levels (deeply layered, editorial).
- **Where does the eye start?** One option uses an F-pattern, another uses a Z-pattern, another uses a hub-and-spoke.
- **How much is above the fold?** One option fits the complete task context in the viewport, another prioritizes a single focal point with scrollable detail.

Name each option by its dominant strategy: "Data Hero," "Action-First Minimal," "Editorial Flow," "Dense Dashboard."

### Round 2: Converge

Refine surviving options based on user feedback. Typical refinements:

- Adjust emphasis levels for specific elements the user flagged
- Merge the reading order from one option with the density approach of another
- Resolve CTA prominence issues identified in feedback
- Add or remove progressive disclosure layers

### Round 3+: Polish

Focus on fine-tuning: micro-adjustments to spacing-as-emphasis, resolving any remaining competing-emphasis issues, and ensuring the hierarchy works across all primary screens (not just the one the user focused on in feedback).

---

## 7. Per-Option Critique Checklist

When writing the critique for each option in `critique.md`, address every item:

1. What does the user see first, second, third? (State the reading order.)
2. Can you identify the page purpose and primary action in 5 seconds?
3. How many emphasis levels are used? Is that enough? Too many?
4. Are there any elements competing for the same attention level?
5. What information is hidden (progressive disclosure) and is the hiding justified?
6. Does the reading order match the task flow or information priority?
7. How does this hierarchy interact with the locked navigation and layout?

---

## 8. Constraint Propagation

Content hierarchy decisions constrain downstream facets:

- **Typography** must implement the emphasis levels defined here. If hierarchy has 4 levels, the type scale needs at least 4 distinct levels.
- **Color system** inherits emphasis structure. Primary elements may get brand color; ambient elements get muted tones.
- **Density-spacing** uses the spacing-as-emphasis logic established here. Prominent elements get more breathing room.
- **Component design** must respect the hierarchy when defining component prominence (primary vs secondary buttons, card emphasis levels).

State these downstream implications in the critique: "This hierarchy uses 4 levels, which means the type scale will need at least 4 distinct steps."

---

## 9. Locking

When the user locks a content hierarchy option:

1. Record the locked option in `state.json` with `locked_summary` describing the hierarchy strategy (e.g., "4-level hierarchy with data-hero focus, F-pattern reading flow, progressive disclosure for settings and metadata").
2. The locked wireframes become the emphasis reference for all downstream sensory facets.
3. Announce the lock and its downstream implications: "Content hierarchy locked as [name]. Typography, color, and density facets will build on this emphasis structure."
