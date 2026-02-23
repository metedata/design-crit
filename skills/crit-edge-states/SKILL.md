---
name: crit-edge-states
description: Use when designing empty states, error handling, offline behavior, loading content, and boundary conditions for an app's screens.
---

# Edge States

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer working through the non-happy-path states of every screen in the product.
Empty states, error handling, loading, offline behavior, and boundary conditions. These are
the screens users see when something is missing, broken, slow, or unexpected. Most products
fail here -- the happy path gets all the attention and the edges feel like afterthoughts.

Your job: take the locked screen inventory and design how every screen behaves when things
are not ideal. Present 2-3 options that make different bets about the product's personality
and helpfulness in degraded states.

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current project state, round number, prior feedback
2. `.design-crit/brief.md` -- the confirmed design brief
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles
5. `.design-crit/facets/screen-inventory/{locked_option}.html` -- the locked screen inventory

The locked screen inventory is the foundation. Every screen listed there needs edge state
coverage. Read its screen list table and state enumeration.

If this is round 2+, also read:
6. `.design-crit/facets/edge-states/feedback-round-{N-1}.json`
7. Surviving option files in `.design-crit/facets/edge-states/`

---

## 2. Edge State Audit

### Build the State Matrix

For every primary and secondary screen in the locked inventory, build a matrix of edge
states. The screen inventory already enumerated states per screen. Now design how each
state looks and behaves.

| Screen | Empty (First Use) | Empty (Cleared) | Empty (No Results) | Loading | Error | Partial | Overflow | Permission |
|---|---|---|---|---|---|---|---|---|
| Dashboard | X | -- | -- | X | X | X | -- | -- |
| Search | -- | -- | X | X | X | -- | X | -- |
| Settings | -- | -- | -- | X | X | -- | -- | X |

Mark each cell: **X** (this state applies), **--** (does not apply to this screen).
Present this matrix in the critique so the user sees the full coverage map.

---

## 3. Edge State Design Vocabulary

Use this vocabulary when designing and presenting edge states. Each category has distinct
design patterns.

### Empty States

Empty states are the product's first impression for new screens. They answer: "Why is this
empty, and what should I do?"

**First-use empty** -- The user has never added content. The screen has no data at all.
- Pattern A: **Instructional.** Show a brief explanation + primary action button.
  "No projects yet. Create your first project."
- Pattern B: **Illustrated.** Custom illustration or icon + explanation + action.
  Warmer, more branded, takes more vertical space.
- Pattern C: **Starter content.** Pre-populate with sample data the user can edit or delete.
  Reduces blank-canvas anxiety but risks confusion ("is this my data?").
- Pattern D: **Progressive disclosure.** Show the UI chrome (sidebar, headers) with a
  single callout in the content area. The screen looks "real" but empty.

**Cleared empty** -- Content existed but was removed (all items deleted, all filters cleared).
- Usually simpler than first-use. No need for onboarding language.
- Pattern: "No items match" or "All items removed" + action to add/reset.
- Avoid re-showing the first-use onboarding -- the user already knows what this screen does.

**No-results empty** -- A search or filter returned zero matches.
- Always include the query that failed: "No results for 'xyzzy'."
- Pattern A: **Suggestions.** Offer alternative searches, related items, or filter resets.
- Pattern B: **Create from here.** "No matching [thing]? Create one." Turn the dead end
  into an on-ramp.
- Pattern C: **Minimal.** Just the message. No suggestions. Clean but potentially frustrating.

### Error States

Errors have a hierarchy. Match the error severity to the display pattern.

**Inline validation** -- Field-level errors on forms and inputs.
- Show immediately on blur or submit. Red text below the field.
- Message must be specific: "Email must include @" not "Invalid input."
- Do not clear valid fields when one field fails.

**Toast / Banner** -- Non-blocking errors that do not prevent the page from functioning.
- Use for transient failures: "Could not save. Retrying..." or "Connection lost."
- Auto-dismiss after 5-8 seconds OR persist until resolved (for connection errors).
- Position: top-right toast or top-of-page banner. Be consistent across screens.

**Full-page error** -- The entire screen cannot load or function.
- Use sparingly. Only when the page has no usable content at all.
- Always include: what went wrong (in human language), a recovery action (retry, go back,
  contact support), and what was NOT lost (unsaved data status).
- Never show stack traces, error codes, or technical details to end users.

**Recovery actions** -- Every error must suggest a next step.
- Retry: "Try again" button that re-attempts the failed operation.
- Fallback: "Go to [safe screen]" that takes the user somewhere functional.
- Report: "Contact support" or "Report this issue" for unrecoverable errors.
- Undo: If the error resulted from a user action, offer to undo it.

### Loading States

Loading states manage perceived performance. The goal: the screen feels faster than it is.

**Skeleton screens** -- Gray placeholder shapes matching the layout of the loaded content.
- Use for content-heavy screens where the layout is predictable.
- Skeletons should match the actual content layout closely. A skeleton that looks nothing
  like the loaded state creates a jarring transition.
- Animate with a subtle shimmer or pulse. Static skeletons look broken.

**Placeholder content** -- Blurred or low-fidelity versions of the actual content.
- Use when you have partial data (e.g., image thumbnails while full images load).
- Progressive enhancement: show low-res first, swap to high-res.

**Progress indicators** -- Spinners, progress bars, percentage counters.
- Spinner: use for indeterminate waits under 5 seconds. No percentage.
- Progress bar: use when the operation has measurable progress (file upload, data import).
- Percentage: use for long operations (>10 seconds) where users need reassurance.

**Optimistic UI** -- Show the result before the server confirms it.
- Use for low-risk actions: toggling a setting, adding a tag, marking as read.
- Do NOT use for destructive or financial actions (delete, purchase).
- If the server rejects the optimistic update, revert visually and show an error toast.

### Offline Behavior

For apps that may be used without connectivity (mobile apps, PWAs, desktop apps with sync).

**Cached content** -- Previously loaded data displayed from local storage.
- Indicator: "Last updated [time]" or a subtle "offline" badge.
- Allow reading cached content. Disable or queue write actions.

**Sync indicators** -- Visual signals that data is not yet synced.
- Unsynced items marked with a small icon (cloud with line, pending dot).
- Sync queue count: "3 changes waiting to sync."
- On reconnect: sync automatically, show "Syncing..." then "All changes saved."

**Degraded functionality** -- Some features work offline, others do not.
- Clearly indicate which actions are unavailable: gray out buttons, show tooltip on hover.
- Never silently fail. If the user taps a disabled action, explain: "This requires a
  connection. Your change will sync when you're back online."

### Boundary Conditions

Edge cases that are not errors but are unusual situations.

**Permission denied** -- User lacks access to a screen or action.
- Never show a blank page. Explain what they cannot access and why.
- If appropriate, show a path to requesting access: "Ask [admin] for access."

**Expired session** -- Authentication has timed out.
- Preserve the user's current state (unsaved form data, scroll position).
- Show a re-authentication prompt that returns the user to where they were.
- Never silently redirect to a login page and lose in-progress work.

**Rate limiting** -- User has exceeded usage limits.
- Explain what happened, when they can retry, and whether upgrading helps.
- Show a countdown if the limit resets on a timer.

**Browser incompatibility** -- Feature not supported in this browser.
- Graceful degradation: show a simpler version if possible.
- If impossible, explain which browsers are supported. Link to download.

---

## 4. Generate Option Artifacts

### Artifact Type: Edge State Wireframes

Each option is a self-contained HTML file showing wireframe-level designs for edge states
across the product's screens. Not pixel-perfect mockups -- wireframes with enough detail
to communicate the approach.

### What to Render

Each option file must contain:

1. **Coverage summary** -- table showing which screens have which edge states designed.
2. **Edge state wireframes** -- for each screen that has edge states, show the wireframe
   for each applicable state. Group by screen, then by state type.
3. **Pattern legend** -- explain the approach this option takes (e.g., "all empty states
   use illustrated approach with action button").
4. **Copy samples** -- actual text for key states. Error messages, empty state headlines,
   loading text. Not lorem ipsum.

### Wireframe Fidelity

Low-to-medium. Use simple boxes, text, and basic shapes. The wireframes communicate the
structure and content of each state, not the visual design. Gray backgrounds, black text,
simple borders. No color, no icons (describe icon placement with a placeholder square).

---

## 5. Option Generation Strategy

### Round 1: Diverge on Personality and Helpfulness

Generate 2-3 options that make different bets about how the product handles non-ideal
states. Common axes of variation:

**Minimal vs Helpful**
- **Minimal:** Short messages, no illustrations, functional recovery actions. Clean and
  efficient. Treats the user as competent. Risk: feels cold or unhelpful.
- **Helpful:** Detailed explanations, illustrations, contextual suggestions. Guides the
  user through every edge case. Risk: feels patronizing or cluttered.

**Uniform vs Contextual**
- **Uniform:** Same pattern across all screens (every empty state looks the same, every
  error uses the same template). Consistent and predictable. Risk: misses opportunities
  to be more helpful in specific contexts.
- **Contextual:** Each screen's edge states are tailored to its content and purpose.
  Richer but more design work. Risk: inconsistency.

**Optimistic vs Cautious**
- **Optimistic:** Skeleton screens, optimistic UI, progressive loading. Assumes things
  will work and optimizes for perceived speed. Risk: jarring reversals when things fail.
- **Cautious:** Explicit loading indicators, confirmation before destructive actions,
  clear sync status. Assumes things might fail and communicates state honestly.
  Risk: feels slower, more friction.

### Naming Options

Name each option by its defining characteristic. Examples:
- "Clean & Minimal" (minimal, uniform)
- "Guided Recovery" (helpful, contextual)
- "Optimistic Fast" (optimistic, skeleton-heavy)
- "Honest & Clear" (cautious, explicit status)

### Round 2+: Converge

Refine survivors based on user feedback. Common refinement moves:
- Mix approaches: use one option's empty states with another's error handling.
- Adjust copy tone based on feedback about personality.
- Add or remove edge states based on coverage feedback.

---

## 6. Evaluation Criteria

When writing the critique and comparative take, evaluate each option against:

### Coverage
- Does every screen in the locked inventory have edge states designed?
- Are there screens missing empty, error, or loading states?
- Does the coverage matrix have any gaps?

### Actionability
- Does every error message suggest a recovery action?
- Do empty states guide the user toward the happy path?
- Can the user always get unstuck without external help?

### Consistency
- Do similar states look and behave similarly across screens?
- Is the copy tone consistent? (Same product voice in errors and empty states.)
- Are loading patterns uniform? (All screens use skeletons, or all use spinners -- not a mix without reason.)

### Content Quality
- Are error messages specific and human-readable? ("Could not save your note" not "Error 500".)
- Do empty states explain WHY it is empty and WHAT to do next?
- Is loading text present where waits exceed 2 seconds?

### Brief Alignment
- Do edge state patterns match the product's personality from the brief?
- Are offline states included if the brief mentions mobile or offline use?
- Do permission patterns match the user roles in the brief?

---

## 7. Common Mistakes to Flag

Call these out in the critique when you see them in options or user feedback:

| Mistake | Why It Matters |
|---|---|
| Empty state that just says "No data" | User does not know what to do next |
| Error message showing technical details | Users cannot act on "Error: ECONNREFUSED" |
| Loading spinner with no text | User does not know what is loading or how long it will take |
| Offline mode that silently drops writes | Data loss is the worst edge case |
| Permission denial with no explanation | User feels locked out with no recourse |
| Re-showing onboarding on cleared empty | User already knows the product; treat them as returning |
| Skeleton that does not match loaded layout | Skeleton-to-content transition feels broken |

---

## 8. Crit Loop Execution

Follow `reference/crit-loop.md` exactly:

1. Generate option HTML files in `.design-crit/facets/edge-states/`.
2. Write `critique.md` with per-option rationale and a comparative take.
3. Generate `compare.html` with side-by-side option cards.
4. Open `compare.html` in the user's browser.
5. Wait for feedback. Process per round-trip mechanics.

### State Updates

After locking, record the decision in `state.json`. The locked edge state approach
becomes a constraint for downstream facets. Announce: "Edge states locked as [option name].
[N] screens covered with [approach summary]."

### Dark Mode Toggle

Omit the dark mode toggle for this facet. Edge state wireframes are structural, not visual.

### Constraint Propagation

Read the locked screen inventory before generating options. The screen list is fixed --
do not add or remove screens. If the user's feedback implies a missing screen, flag it:
"That sounds like a new screen. Want to revisit the screen inventory?"

After locking, the edge state patterns carry forward as constraints for visual facets.
When the color system is designed later, error states need red/warning colors. When
typography is set, empty state headlines need a type treatment.
