# Crit Loop Mechanics

> Shared reference for all `crit-*` facet skills. Defines the compare view, feedback
> round-trip, convergence guidance, and locking mechanics that every facet follows.

---

## 1. Compare View HTML Generation

Generate `.design-crit/facets/{facet-id}/compare.html`. Desktop-first, side-by-side layout.
Options must always be visible simultaneously.

### Layout Rules

| Option Count | Layout |
|---|---|
| 2 | 50/50 horizontal split, full viewport height |
| 3 | 33/33/33 row |
| 4 | 2x2 grid |
| 5+ | 2-column scrollable grid (rare, early divergent rounds only) |

Use CSS grid. Each cell must scale proportionally. No horizontal scrolling.

### Context Banner

Fixed at top. Shows: **project name** | **facet name** | **Round N** | dark mode toggle.
Include filter controls for rounds 2+: **All** / **Surviving** / **Eliminated**.
Eliminated cards render at 50% opacity with a "CUT" badge and are hidden by default.

### Option Card Structure

Each option is a card containing, in order:

1. **Iframe** -- `src` points to the individual option file (e.g., `option-a.html`). Set a
   fixed aspect ratio container. Add an "Open in new tab" link below the iframe.
2. **Name** -- Characterizing label, not a letter. "Deep Sidebar", not "Option A".
3. **Rationale block** -- Three sections, each with a heading:
   - **WHY THIS OPTION** -- The design reasoning. Why this direction was worth presenting.
     Tie back to the brief.
   - **WORKS WELL FOR** -- Scenarios, user types, or contexts where this approach shines.
   - **WATCH OUT FOR** -- Honest trade-offs. What could go wrong, what this sacrifices.
4. **Per-option controls:**
   - **Keep** / **Cut** toggle buttons (mutually exclusive, default unset)
   - **Comment** textarea (placeholder: "What works? What doesn't?")

### Comparative Take

Below all option cards, render a section titled **"My Take"**. This is the LLM's overall
read across all options -- like a designer saying "here's my recommendation and why." Write
this in `critique.md` and embed it in the compare view.

### Global Feedback Area

Below the comparative take, render:

- **Direction notes** textarea (placeholder: "Overall thoughts or direction preferences...")
- Three action buttons in a row:
  - **Save Feedback** (primary) -- writes `feedback-round-N.json`
  - **Skip This Facet** (secondary) -- writes feedback with `"decision": "skip"`
  - **Decide For Me** (secondary) -- writes feedback with `"decision": "delegate"`
- Optional: **Delegate context** textarea, shown when "Decide For Me" is clicked
  (placeholder: "Any guidance? e.g., 'prioritize accessibility' or 'keep it simple'")

### Dark Mode Toggle

When the facet involves visual styling (color, typography, density), include a toggle that
switches the compare view chrome AND sends a message to each iframe to toggle. For
structural facets (screen inventory, navigation), omit this.

---

## 2. Individual Option Files

Each option lives at `.design-crit/facets/{facet-id}/option-{x}.html`.

### Requirements

- **Self-contained HTML** with all CSS inlined. No external stylesheets, fonts, images, or scripts.
  Everything renders from a single file.
- **Must render correctly both iframed** (inside `compare.html`) **and standalone** (opened
  directly in a browser tab).
- Use a `<meta name="viewport" content="width=device-width, initial-scale=1">` tag.
- Use a descriptive `<title>` matching the option name.

### Persistence Across Rounds

Option files persist across rounds and get **refined in place**. When the user keeps an
option and provides feedback, update the existing file -- do not create a new one. Files
accumulate fidelity as later facets layer decisions onto earlier ones. For example, the
layout wireframe from round 1 becomes the canvas that color gets applied to in a later facet.

### Naming

Use lowercase kebab-case: `option-a.html`, `option-b.html`, etc. When merging two surviving
options, create a new file (e.g., `option-ab.html`) and remove the originals from the
surviving list in state.json.

---

## 3. Feedback Round-Trip

### Round N Flow

```
Claude reads:
  1. state.json           -> current facet, round number, prior locked decisions
  2. feedback-round-(N-1).json -> what to keep, cut, refine (skip for round 1)
  3. surviving option files    -> current state of each survivor

Claude writes:
  4. option-{x}.html files    -> generate new or refine existing
  5. critique.md              -> per-option rationale + comparative take
  6. compare.html             -> regenerated, iframes current options
  7. Open compare.html in the user's browser

User acts:
  8. Reviews compare.html side by side
  9. Marks Keep/Cut per option, writes comments
  10. Clicks Save Feedback (or Skip / Decide For Me)
  11. Browser writes feedback-round-N.json to disk

Next turn:
  12. User returns to Claude Code (or Claude detects the feedback file)
  13. Claude reads feedback -> refine survivors / generate new options / suggest locking
```

### Feedback JSON Schema

File: `.design-crit/facets/{facet-id}/feedback-round-{N}.json`

```json
{
  "facet": "navigation-model",
  "round": 2,
  "timestamp": "2026-02-23T14:30:00Z",
  "options": {
    "option-a": { "action": "keep", "comment": "Like the sidebar but needs keyboard shortcuts" },
    "option-b": { "action": "cut", "comment": "Too simple for deep hierarchies" },
    "option-c": { "action": "keep", "comment": "" }
  },
  "overall": "Leaning toward A but want C's quick-switch as a secondary mode",
  "decision": "refine"
}
```

Valid `decision` values:
- `"refine"` -- continue iterating with surviving options
- `"lock"` -- lock the winner (requires exactly one option with `"action": "keep"`)
- `"skip"` -- skip this facet entirely (unresolved, can revisit later)
- `"delegate"` -- hand the decision to the LLM (see Decide For Me below)

Valid `action` values per option: `"keep"`, `"cut"`, or `null` (no action taken).

### Save Feedback Implementation

The compare.html inline JS must handle saving without a server:

1. **Primary: File System Access API.** Use `window.showSaveFilePicker` with suggested
   filename `feedback-round-{N}.json`. Serialize form state to JSON, write via
   `FileSystemWritableFileStream`. Detect with `if ('showSaveFilePicker' in window)`.

2. **Fallback: Copy-paste.** Show a modal with JSON in a `<textarea>`, a "Copy to Clipboard"
   button, and instructions: "Paste this into Claude Code, or save as
   `feedback-round-{N}.json` in your facet folder."

---

## 4. Convergence Guidance

### Default Rhythm

This is soft guidance, not enforced. Most facets resolve in 2 rounds.

| Round | Mode | LLM Behavior |
|---|---|---|
| 1 | **Diverge** | Present 2-4 options exploring different directions. Maximize variety. |
| 2 | **Converge** | Refine survivors, possibly merge best elements. Reduce to 1-2 options. |
| 3 | **Polish** (optional) | One final refinement pass on the chosen direction. High-stakes facets only. |
| 4+ | **Nudge to lock** | Actively guide toward a decision. The remaining differences are implementation details. |

### Escalating Lock Prompts

After each round, ask the user with increasing directness:

- **Round 1:** "Want to refine the survivors, or do you already see the direction?"
- **Round 2:** "Ready to lock this, or one more refinement pass?"
- **Round 3:** "I'd recommend locking. The remaining differences are details for implementation."
- **Round 4+:** "We've been on this one for a while. What's the ONE thing still unresolved?
  Let's nail that and lock it."

### Convergence Tone

You are a designer in service of the decision, not managing the user into convergence.

1. **Curiosity before correction.** Assume you are missing context, not that the user is
   confused. Ask: "Help me understand what changed."
2. **Reframe, don't confront.** Not "you said X before" but "it sounds like the priority
   might be shifting toward Y -- is that right?"
3. **Make the decision easy, not forced.** Reduce to the clearest question: "This comes
   down to whether you value navigation depth or content space more."
4. **Be opinionated but hold it loosely.** "I'd lean toward A, but I can see why B appeals."
5. **Name the trade-off, not the answer.** The user decides. You make the trade-off visible.

### Convergence Signals

Watch for these patterns and respond accordingly:

| Signal | Response |
|---|---|
| User keeps very similar options | "These are converging. Want me to merge the best of both?" |
| Comments getting narrower / more specific | "Sounds like the direction is clear. Ready to lock?" |
| Round 3+ with no eliminations | "We're exploring broadly -- what's the ONE thing you're uncertain about?" |
| Repeated requests for new options | "What would a new option solve that these don't? That'll help me generate something genuinely different." |
| Feedback contradicts prior round | "It sounds like the priority might be shifting. Can I ask what changed?" |
| Low-effort feedback ("looks fine", "sure") | "I can decide this one for you if you'd like -- or is there a specific thing holding you back?" |

---

## 5. Decide For Me

"Decide For Me" is **NOT skip**. Skip means the facet is unresolved. Decide For Me means it
IS resolved -- by the LLM, with full rationale, reviewable and reversible.

### When to Offer Proactively

Suggest "I can decide this one for you if you'd like" when:

- The facet is highly technical (accessibility, elevation, loading patterns)
- The user's feedback signals low confidence ("I don't really know", "whatever you think")
- The user is moving fast and this facet is not core to their product's differentiation
- The brief suggests this is a standard decision, not a differentiating one

### How It Works

1. User clicks "Decide For Me" (optionally adding context in the delegate textarea)
2. Compare.html writes feedback with `"decision": "delegate"` and any `"delegate_context"`
3. Claude picks the best option based on: the brief, locked decisions from prior facets,
   design best practices, and any delegate context provided
4. Claude writes a full rationale -- same quality as a regular critique, not a silent auto-pick
5. Claude locks the decision with `"decided_by": "llm"` in state.json

### Delegation Feedback JSON

```json
{
  "facet": "accessibility",
  "round": 1,
  "decision": "delegate",
  "delegate_context": "Focus on keyboard nav, I trust best practices for the rest",
  "options": {},
  "overall": ""
}
```

### State Recording

Record in state.json with `"decided_by": "llm"` and include `decision_rationale`:

```json
{
  "status": "locked",
  "locked_option": "option-a",
  "locked_summary": "WCAG AA compliant with keyboard-first interaction model",
  "decided_by": "llm",
  "decision_rationale": "Selected for strongest keyboard nav support...",
  "rounds_completed": 1
}
```

The overview page must show delegated decisions distinctly: "locked (auto)" vs "locked".

---

## 6. Locking Mechanics

### What Locking Means

1. **Winner becomes canonical.** The locked option file is the source of truth for that facet.
2. **Decision + rationale recorded in state.json** with `status`, `locked_option`,
   `locked_summary`, `decided_by` ("user" or "llm"), `decision_rationale`, `rounds_completed`.
3. **Constraints carry forward.** All subsequent facets inherit locked decisions. When critting
   color, the locked layout wireframe is the canvas. Read all prior locked option files before
   generating new options.
4. **Overview page updates.** Show locked facets with checkmark, winner name, and a **Review**
   button that expands the full decision history inline.

### Constraint Propagation

Before generating options for any facet, read ALL prior locked decisions. Load each locked
facet's winning option file and `locked_summary` from state.json. Apply as constraints:
locked layout = wireframe structure is fixed; locked color = palette is set; locked density =
spacing scale is defined. State this in the critique: "Building on the locked [facet]: [summary]."

### Overview Page Updates

After locking, regenerate `overview.html`:

```
[checkmark] Navigation -- locked (Deep Sidebar)                    [Review]
[checkmark] Color System -- locked, auto (Soft Indigo)             [Review]
[spinner]   Typography -- round 2, 2 options surviving             [Continue]
[ ]         Density -- pending
```

**Review** expands inline: options per round, user feedback excerpts, eliminations with
reasons, final decision + rationale, and whether user-decided or LLM-delegated.
