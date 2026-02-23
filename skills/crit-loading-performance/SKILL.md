---
name: crit-loading-performance
description: Use when evaluating loading states, perceived performance patterns, skeleton screens, optimistic UI, and progress indicators.
---

# Loading & Perceived Performance Crit

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing how this product handles loading, transitions, and perceived
performance. Loading is when the user's trust is most fragile -- the product promised
something and has not yet delivered. Every second of uncertainty erodes confidence. The goal
is not to make the product faster (that is engineering), but to make it FEEL faster and keep
the user informed at every moment.

Loading performance is a sensory/experiential facet. It depends on `screen-inventory` (which
screens load data), `edge-states` (which screens have loading states defined), `layout`
(content areas that need placeholders), and `component-design` (components that show loading
indicators). It feeds into `accessibility` (screen reader announcements for loading states,
ARIA live regions).

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (platform, tech stack, network conditions, user context)
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Loading patterns apply to every screen with
async content. Pay special attention to:

- **Screen inventory** -- which screens exist and which load async data
- **Edge states** -- which screens have loading states defined, what approach was chosen
  (the edge states facet establishes WHICH screens need loading; this facet designs HOW
  loading works across the product)
- **Layout** -- content zones that need skeleton or placeholder treatment
- **Component design** -- buttons, cards, lists, forms that transition between states
- **Navigation** -- view transitions, route changes, navigation loading patterns
- **Color system** -- skeleton placeholder colors, progress indicator colors, status colors
- **Typography** -- text skeleton line heights, placeholder text sizing

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/loading-performance/`. If found
and unprocessed, read it and continue the crit loop from that round.

---

## 2. Loading Audit

Before generating options, catalog every loading moment in the product.

### Loading Moment Inventory

Walk every screen in the locked inventory and identify async operations:

| Screen | Operation | Expected Duration | Data Type | Criticality |
|---|---|---|---|---|
| Dashboard | Initial data fetch | 1-3s | Mixed (stats, lists, charts) | High -- first thing user sees |
| Search results | Query execution | 0.5-2s | List of items | High -- user is waiting for answer |
| Detail view | Record fetch | 0.3-1s | Single record | Medium -- user knows what to expect |
| Settings | Config load | 0.2-0.5s | Key-value pairs | Low -- infrequent screen |
| Image gallery | Media loading | 2-10s | Binary (images) | High -- visual content is the point |
| Form submit | Server write | 0.5-3s | Confirmation | High -- user awaits feedback on action |

### Duration Tiers

Classify each loading moment by expected duration. The design pattern changes with duration:

| Duration | Tier | Appropriate Pattern |
|---|---|---|
| < 300ms | Instant | No indicator needed. Show result immediately. |
| 300ms - 1s | Brief | Subtle indicator: button spinner, inline skeleton. No full-page treatment. |
| 1s - 5s | Noticeable | Skeleton screen, shimmer placeholder, or content streaming. |
| 5s - 15s | Long | Progress bar with percentage or step indicator. Explain what is happening. |
| 15s+ | Extended | Progress with estimated time, cancel option, and "work in background" choice. |

### Transition Inventory

Catalog screen-to-screen transitions:

- Route change (clicking a nav item): what happens during the load between views?
- Tab switching within a screen: does content re-fetch or is it cached?
- Modal/drawer opening: does the overlay content load independently?
- Pagination / infinite scroll: how does the next page of content appear?
- Pull-to-refresh (mobile): what visual feedback signals the refresh?

---

## 3. Loading Pattern Vocabulary

### Skeleton Screens

Gray placeholder shapes that match the layout of the loaded content. The user sees the
structure before the data.

**When to use:**
- Content-heavy screens where the layout is predictable
- Lists and feeds where each item has a consistent structure
- Dashboards where card positions are fixed

**Design rules:**
- Skeleton shapes must match the loaded content dimensions. A skeleton card that is a different
  height than the loaded card creates a layout shift.
- Use the locked layout grid. Skeleton elements align to the same columns and rows as real
  content.
- Animate with a subtle left-to-right shimmer (wave effect). Static skeletons look broken.
  Shimmer speed: 1.5-2s per cycle.
- Skeleton colors: use a neutral gray (or locked surface color lightened/darkened). Two shades:
  one for background, one slightly lighter for the shimmer highlight.
- Replace skeleton with real content progressively as data arrives. Do not wait for all data.

**Anti-patterns:**
- Skeleton that does not match loaded layout -- jarring snap when content loads.
- Too many skeleton levels (skeleton inside skeleton inside skeleton) -- visual noise.
- Skeleton for content that loads in under 300ms -- unnecessary visual complexity.

### Spinners

Rotating indicators for indeterminate waits.

**When to use:**
- Single component loading (button, small section)
- Actions where the layout is unknown until data arrives
- Short waits (300ms - 3s) where building a skeleton is overkill

**Design rules:**
- Size spinner proportionally to the container. A 16px spinner in a button. A 32px spinner in
  a card. A 48px spinner for a section.
- Use the locked color system: primary color for the active segment, neutral for the track.
- Add a brief text label for waits over 2s: "Loading..." or a specific message like
  "Fetching results..."
- Hide spinner if the content loads in under 300ms (use a display delay to avoid flash).

### Optimistic UI

Show the result of an action immediately, before the server confirms it.

**When to use:**
- Low-risk, reversible actions: toggle, like, bookmark, tag, mark as read
- Actions where server rejection is rare (< 5% failure rate)
- Interactive moments where latency breaks the flow (drag-and-drop reorder, inline edit)

**When NOT to use:**
- Destructive actions (delete, cancel subscription)
- Financial actions (purchase, transfer, payment)
- Actions with complex validation the client cannot replicate
- Actions where failure has consequences the user cannot see (sending a message to the wrong recipient)

**Design rules:**
- Apply the visual change immediately. No spinner, no delay.
- If the server rejects the action, revert the visual change AND show an error toast explaining
  what happened: "Could not save. Your change has been reverted."
- For actions with a delay before server response, show a subtle pending indicator (a small
  sync icon or color hint) that resolves when confirmed.

### Progressive Loading

Content streams in as it becomes available instead of all at once.

**When to use:**
- Pages with multiple independent data sources (dashboard with several API calls)
- Long lists where above-the-fold items can load first
- Media-heavy pages where text loads before images

**Design rules:**
- Load above-the-fold content first. The user's visible viewport populates before off-screen
  content.
- Each content block transitions independently: its own skeleton resolves to its own content.
- Maintain layout stability. Reserve space for content that has not loaded yet. No cumulative
  layout shift.
- Order the loading sequence by user priority: most important content first.

### Progress Indicators

For operations with measurable progress.

**When to use:**
- File uploads, data imports, exports
- Multi-step processes (processing, then saving, then sending)
- Any operation over 5 seconds where progress is computable

**Design rules:**
- Use a determinate progress bar when you know the total (e.g., bytes uploaded / total bytes).
- Show percentage AND a human description: "Uploading... 45% (23 of 50 files)"
- For multi-step processes, show the current step: "Step 2 of 4: Processing data..."
- Include a cancel button for operations over 10 seconds.
- For operations over 30 seconds, offer "Run in background" to free the UI.
- Never let a progress bar jump backward. If progress is non-linear, smooth it.

### View Transitions

How the interface moves from one state to another.

**When to use:**
- Every route change and view switch

**Design rules:**
- Transition duration: 150-300ms for most view changes. 300-500ms for significant context
  changes (opening a modal, expanding a panel).
- Prefer content dissolve or slide over instant swap. But prioritize speed over polish --
  a fast cut is better than a slow animation that delays content.
- Do not animate both the outgoing and incoming content simultaneously (causes visual chaos).
  Fade out the old, then fade in the new. Or slide the new over the old.
- Navigation loading: show a thin progress bar at the top of the viewport (NProgress-style)
  during route changes. This is the minimum viable loading indicator for route transitions.

---

## 4. Artifact Type

Generate **loading pattern specimens and transition storyboards**.

Each option file (`option-{x}.html`) must contain:

### Loading Pattern Catalog

For each loading moment in the audit, show:

1. **Before state** -- the screen/component before the loading operation starts.
2. **Loading state** -- the screen/component during the wait, with the chosen pattern applied.
3. **After state** -- the screen/component with content loaded.

Render these as a horizontal strip: BEFORE -> LOADING -> AFTER for each loading moment.
Use the locked layout, color system, and typography to render realistic specimens.

### Transition Storyboard

Show screen-to-screen transitions as a sequence:

1. **Trigger** -- what the user did (clicked nav, submitted form, opened modal)
2. **Transition** -- how the view changes (route loading bar, content fade, slide)
3. **Loading** -- how new content appears (skeleton, spinner, progressive)
4. **Settled** -- final state with all content loaded

### Timing Specification Table

Document the timing for every loading pattern:

| Pattern | Delay Before Show | Animation Duration | Shimmer/Spin Speed | Threshold |
|---|---|---|---|---|
| Skeleton | 0ms | N/A | 1.8s cycle | Show for loads > 300ms |
| Button spinner | 200ms | N/A | 1s cycle | Show for actions > 200ms |
| Route progress bar | 100ms | Matches load | N/A | Always for route change |
| Optimistic update | 0ms (instant) | 200ms visual | N/A | Always for qualifying actions |
| Progress bar | 0ms | Matches progress | N/A | Operations > 5s |

### Optimistic UI Inventory

List every action classified as optimistic or confirmed:

| Action | Approach | Rollback Behavior |
|---|---|---|
| Toggle favorite | Optimistic | Revert toggle + error toast |
| Delete item | Confirmed (with undo) | N/A, waits for confirmation |
| Save form | Confirmed (spinner) | Show inline error |
| Send message | Optimistic with pending indicator | Show failed badge + retry |

---

## 5. Evaluation Criteria

Score each option against these questions in your critique.

### User Always Knows What Is Happening

- Is there a visual indicator for every wait over 300ms?
- Can the user distinguish between "loading" and "broken"? (A screen with no content and no
  indicator looks broken.)
- Do progress indicators communicate real progress, not just activity?
- Are view transitions smooth enough that the user maintains spatial orientation?

### Loading Feels Fast

- Are skeleton screens used where layout is predictable?
- Does progressive loading show above-the-fold content first?
- Are optimistic patterns used for appropriate low-risk actions?
- Is there a display delay on short-lived indicators to avoid flash? (A spinner that appears
  for 50ms is worse than no spinner at all.)

### Errors Surface Gracefully

- When an optimistic update fails, is the rollback clear and the error explained?
- When a long operation fails mid-progress, does the user know what was saved and what was not?
- When a route change fails, does the user stay on the previous screen with an error message?
- Are timeout thresholds reasonable? (Do not show "loading" for 60 seconds before showing an error.)

### Layout Stability

- Do loading states prevent cumulative layout shift? (Content should not push other content
  around when it appears.)
- Do skeleton dimensions match loaded content dimensions?
- Are images and media reserving space before they load?

### Pattern Consistency

- Is the same loading pattern used for similar operations across the product?
- Are skeleton styles consistent (same gray, same shimmer, same animation)?
- Are progress indicators consistent (same bar style, same position, same color)?
- Is there a clear hierarchy: which patterns are used when?

---

## 6. Option Generation Strategy

### Round 1: Diverge

Generate 2-3 options that make different bets about perceived performance strategy.

**Axes of variation:**

- **Skeleton-First vs Spinner-First** -- one option uses skeleton screens as the primary
  loading pattern for all content areas. Another uses targeted spinners and keeps the
  chrome stable while only the data areas indicate loading.
- **Optimistic vs Confirmed** -- one option aggressively uses optimistic UI for most user
  actions, making the product feel instant. Another confirms every action with the server
  first, prioritizing data integrity over perceived speed.
- **Progressive vs Atomic** -- one option streams content progressively (each section loads
  independently). Another loads entire views atomically (the whole screen transitions at once
  with a single loading state).
- **Minimal vs Informative** -- one option uses the minimum viable loading indicators (thin
  progress bar, brief skeleton). Another provides rich feedback (progress percentages, step
  descriptions, estimated time remaining).

### Naming Options

Name each option by its performance personality:

- "Instant Optimist" (aggressive optimistic UI, minimal indicators)
- "Skeleton Everything" (skeleton screens everywhere, progressive loading)
- "Honest Progress" (explicit indicators, confirmed actions, detailed progress)
- "Minimal Fast" (thin progress bar, no skeletons, spinner only when needed)
- "Stream & Settle" (progressive loading, independent section resolution)

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Mix patterns: skeleton for initial loads, optimistic for user actions
- Adjust timing thresholds based on user preference (show indicators sooner or later)
- Reclassify specific actions from optimistic to confirmed (or vice versa) based on feedback
- Refine skeleton fidelity to better match loaded content
- Adjust view transition timing and style

---

## 7. Common Patterns and Trade-offs

Reference these patterns when explaining options.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| **Skeleton screens everywhere** | Content-heavy apps, predictable layouts, feeds | Expensive to maintain skeletons for every layout variation; skeletons that do not match loaded content are worse than a spinner |
| **Optimistic-first** | Social apps, high-interaction tools, real-time UIs | Rollback handling is complex; silent failures erode trust; not suitable for critical actions |
| **NProgress-style top bar** | Route changes, SPAs, minimal overhead | Too subtle for long waits; users may not notice it; not enough feedback for 5s+ loads |
| **Progressive/streaming** | Dashboards with multiple data sources, media-heavy pages | Complex to implement; partial states can confuse users; requires careful layout reservation |
| **Full-page loading screen** | Initial app boot, auth check | Feels slow; blocks all interaction; acceptable only once per session if necessary |
| **Stale-while-revalidate** | Data that changes frequently but stale data is acceptable | User may act on stale data; needs "last updated" indicator; confusing if stale data is too old |
| **Background sync indicators** | Offline-capable apps, collaborative tools | Must be subtle enough not to distract but visible enough to notice; sync conflicts are complex |

---

## 8. Compare View Specifics

When generating `compare.html`, follow `crit-loop.md` with these additions:

- **Include the dark mode toggle** if the locked color system has dark mode. Loading indicators
  must work on both light and dark surfaces.
- Each option card must show the BEFORE -> LOADING -> AFTER strips for at least 3 key loading
  moments (initial page load, user action, route change). These are the minimum specimens for
  meaningful comparison.
- Include CSS animations in option files. Skeleton shimmer, spinner rotation, and progress bar
  fill should be animated so the user can see the actual loading experience, not a static
  representation of it.
- The comparative take must address: which option feels fastest, which best handles failure
  states, and whether the loading patterns are consistent and maintainable.

---

## 9. Relationship to Adjacent Facets

### Edge States (locked before)

Edge states defined WHICH screens have loading states. This facet designs HOW loading works
product-wide. Do not add loading states to screens the edge states facet did not identify.
If a screen is missing loading coverage, flag it: "The edge states facet did not define a
loading state for [screen]. Should we add one?"

### Component Design (locked before)

Buttons, cards, and lists from component design define the containers that show loading
indicators. A button spinner must fit inside the locked button size. A card skeleton must
match the locked card dimensions.

### Motion (if locked before)

If motion is already locked, loading animations must use the locked easing curves and timing.
Skeleton shimmer, spinner rotation, and view transitions should feel like part of the same
motion system.

### Accessibility (runs after)

The accessibility audit will evaluate: ARIA live regions for loading announcements, screen
reader behavior during skeleton display, focus management during view transitions, and whether
progress indicators are accessible (role="progressbar", aria-valuenow, etc.).

---

## 10. Locking and Handoff

When the user locks a loading performance option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: primary loading pattern (skeleton/spinner/progressive),
   optimistic UI strategy (which actions are optimistic), timing thresholds, and view
   transition approach.
3. The locked loading patterns inform `design-tokens.json` at the direction stage for
   timing values (shimmer speed, transition duration, display delay thresholds).
4. Announce: "Loading patterns locked as [name]. [summary]. The accessibility audit will
   check loading state announcements and focus management."

---

## 11. Constraints

- Loading specimens must include CSS animations. A static screenshot of a skeleton screen
  does not convey the experience. Shimmer, spin, and progress fill must be animated in the
  option HTML.
- Every loading moment from the audit must have a specimen in the option. No loading moments
  left unaddressed.
- Optimistic UI classifications must be justified. For each action classified as optimistic,
  state why the failure rate is low enough and the consequence of rollback is acceptable.
- Skeleton screens must visually match the loaded content layout. Render both the skeleton
  and the loaded state in the specimen so the user can verify the match.
- Timing specifications must be concrete numbers, not ranges. "Show skeleton after 300ms" not
  "show skeleton after a short delay."
- View transitions must not block content display. A 500ms transition on every route change
  adds 500ms to every navigation. Justify any transition over 300ms.
