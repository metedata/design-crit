# Crit Loop Mechanics

> Shared reference for all `crit-*` facet skills. Defines the compare view, feedback
> round-trip, convergence guidance, and locking mechanics that every facet follows.
>
> **Visual design:** Read `./crit-ui.md` (in this same directory) for exact colors, typography, spacing,
> and component specs. Every generated HTML page must follow that design system.

---

## 1. Compare View HTML Generation

Generate `.design-crit/facets/{facet-id}/compare.html`. Follow `./crit-ui.md` (in this
same directory) for colors, typography, and component specs.

### Layout Goals

The compare view layout is **not fixed** — choose the layout that best serves the
content being compared. The LLM decides the layout for each compare view based on
the facet, platform, and content type. The goals are:

1. **Every option must be clearly visible.** The user should be able to see enough of
   each option to understand the design concept without excessive scrolling.
2. **Options must be comparable.** The user needs to evaluate options relative to each
   other. Side-by-side is ideal when options fit. Tabbed or toggle views work when
   they don't.
3. **The wireframe is the hero.** Maximize the space given to the iframe previews.
   Rationale text is important but secondary — it should not compete with the visual.
4. **No horizontal scrolling.** The layout must fit the browser viewport width.
5. **Adapt to the content shape.** A desktop app wireframe is landscape. A mobile
   screen is portrait. A data table is wide. A form is narrow. The layout should
   match the shape of what's being shown.

### Layout Toolkit

Use these as a starting point. Mix and adapt as needed.

**Side-by-side columns** — Best when options have a similar, compact shape (mobile
screens, component variants, icon sets). Use CSS grid with equal columns. Works well
for 2-3 options when each option fits in ~400px+ width.

**Stacked full-width** — Best when each option needs maximum horizontal space (desktop
wireframes, wide dashboards, navigation bars). Stack options vertically, each getting
the full content width. The user scrolls down to compare. Add a sticky nav or anchor
links so they can jump between options.

**Tabbed / toggle view** — Best when options are complex full-page layouts that need
the entire viewport to evaluate. Show one option at a time with tabs or a segmented
control to switch. Add a subtle transition so the user can mentally diff them.

**2x2 grid** — Best for 4 options when each is relatively compact (color palettes,
spacing scales, typography samples, icon styles). Each cell should still be large
enough that the content is legible without zooming.

**Hybrid** — Combine approaches. For example: side-by-side iframes at the top (visual
comparison) with a stacked rationale section below each (reading comparison). Or tabs
for the iframe preview with a persistent summary panel showing all options' key traits.

### How to Choose

Consider these factors when selecting a layout:

| Factor | Guidance |
|---|---|
| **Platform** | Desktop wireframes → stacked full-width or tabs. Mobile wireframes → side-by-side (portrait frames fit well in columns). |
| **Facet type** | Structural facets (navigation, layout, screen inventory) → need width, lean toward stacked or tabbed. Visual facets (color, typography, density) → can be more compact, lean toward side-by-side or grid. |
| **Option count** | 2 options → side-by-side works great. 3-4 → depends on content shape. 4+ → grid or tabbed. |
| **Content complexity** | Simple patterns (buttons, spacing) → side-by-side grid. Complex screens (full dashboards) → stacked or tabbed. |
| **Round number** | Round 1 (many options, divergent) → grid or compact side-by-side. Later rounds (2 survivors) → give each option more space. |

**When in doubt, favor giving each option more space over fitting them all on screen
at once.** A user who can clearly see one option and tab to the next makes better
decisions than a user squinting at four tiny iframes.

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
4. **Option label** -- A prominent letter label ("A", "B", "C") so the user can
   reference options easily if they prefer to give feedback in the chat instead.
5. **Per-option controls:**
   - **Keep** / **Cut** toggle buttons (mutually exclusive, default unset)
   - **Comment** textarea (placeholder: "What works? What doesn't?")

### My Take (Comparative Recommendation)

**Full-width below all option cards** — NOT inside the card grid. This section spans
the entire content column and is visually distinct from the option cards above. See
`./crit-ui.md` Section 6 for exact styling.

This is the LLM's overall read across all options — like a designer stepping back and
saying "here's my recommendation and why." Write this in `critique.md` and embed it
in the compare view. It should feel prominent and opinionated, not buried.

### Global Feedback Area

Below My Take, render:

- **Direction notes** textarea (placeholder: "Overall thoughts or direction preferences...")
- Three action buttons in a row:
  - **Save Feedback** (primary) -- writes `feedback-round-N.json`
  - **Skip This Area** (secondary) -- writes feedback with `"decision": "skip"`
  - **Decide For Me** (secondary) -- writes feedback with `"decision": "delegate"`
- Optional: **Delegate context** textarea, shown when "Decide For Me" is clicked
  (placeholder: "Any guidance? e.g., 'prioritize accessibility' or 'keep it simple'")
- **Instruction text** above the button row: "Click Save Feedback and Claude will pick
  it up automatically. Or just tell Claude your thoughts in the chat."

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

### Content Constraints for Iframed Display

Option files are primarily viewed inside iframes in the compare view. Design them to
be **scannable at a glance**, not pages that require excessive scrolling to understand.

The available iframe size depends on the compare view layout chosen for this facet
(see Section 1). A full-width stacked layout gives ~1200px width. A 3-column
side-by-side gives ~400px per option. Design the wireframe for the layout you chose.

**Rules:**
- **Show the pattern, not the content.** Use 3-5 representative items, not 20. Show one
  complete interaction, not every edge case. The wireframe demonstrates the *structure*,
  not the *content*.
- **No long text blocks.** Labels, short headlines, and placeholders only. If explaining
  the concept requires paragraphs, that belongs in the rationale text outside the iframe.
- **Single-screen focus.** Each option shows ONE screen or ONE state. Multi-screen flows
  should be broken into separate options or shown as a simplified flow diagram.
- **Scale-friendly.** Text must be legible at the iframe display size. Use 13-16px base
  font sizes for side-by-side layouts, larger if the option gets full-width.
- **Match the shape.** A mobile wireframe should be rendered in a portrait container.
  A desktop wireframe should be landscape. A component sample can be compact. Don't
  force all wireframes into the same aspect ratio.

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
Claude reads (context-efficient order):
  1. state.json                -> current facet, round number
  2. crit-session.md     -> text summaries of ALL prior locked decisions (small file)
  3. feedback-round-(N-1).json -> what to keep, cut, refine (skip for round 1)
  4. surviving option files    -> ONLY this facet's survivors (not prior facets' HTML)

Claude writes:
  5. option-{x}.html files    -> generate new or refine existing
  6. critique.md              -> per-option rationale + comparative take
  7. compare.html             -> regenerated, iframes current options
  8. Open compare.html in the user's browser

Claude waits:
  9. Immediately start a Bash poll for the feedback file (see Auto-Detect below).
  10. Tell the user: "Take your time reviewing. I'll pick up your feedback
      automatically when you click Save — or just come back and tell me
      what you think."

User acts:
  11. Reviews compare.html side by side
  12. Marks Keep/Cut per option, writes comments in the browser UI
  13. Clicks Save Feedback (browser writes feedback-round-N.json to disk)
  14. Browser shows a notification: "Feedback saved! Claude is picking it up."

Auto-detect:
  15. The Bash poll detects the new file and returns its contents
  16. Claude processes the feedback — no user typing required

  OR (alternative — chat-only feedback):
  11b. User returns to the terminal and describes preferences in their own words
       ("Keep A, cut C, tweak B's spacing"). This interrupts the Bash poll.
  12b. Claude parses the chat message and writes feedback-round-N.json itself.

Next turn:
  17. Claude refines survivors / generates new options / suggests locking
```

### Auto-Detect: Bash File Polling

**CRITICAL: You MUST start this Bash poll immediately after opening the compare view.**
This is what makes the browser-to-terminal feedback loop work automatically. Without it,
the user has to manually type feedback in the chat.

After opening the compare view, start this Bash command using the Bash tool:

```bash
# REQUIRED: Poll for the feedback file, timeout after 5 minutes
# Replace {facet-id} and {N} with actual values
FEEDBACK=".design-crit/facets/{facet-id}/feedback-round-{N}.json"
timeout 300 bash -c "while [ ! -f \"$FEEDBACK\" ]; do sleep 2; done" && cat "$FEEDBACK"
```

**How it works:**
1. Claude runs this command via the Bash tool immediately after opening compare.html.
2. The command blocks, checking every 2 seconds for the feedback file.
3. When the user clicks Save Feedback in the browser, the File System Access API writes
   the JSON file to disk.
4. The poll detects the file and returns its contents to Claude.
5. Claude processes the feedback and continues — no user typing required.

**If the user types in the chat instead**, that naturally interrupts the poll. Claude
parses their chat message and writes feedback-round-N.json itself.

**If the poll times out** (5 minutes), Claude asks: "Still reviewing? No rush. When
you're ready, just tell me your thoughts here — like 'keep A, cut C' — and I'll take
it from there."

**The sequence after generating compare.html is always:**
1. Open compare.html in the browser
2. Tell the user: "Take your time reviewing. I'll pick up your feedback automatically
   when you click Save — or just come back and tell me what you think."
3. Start the Bash poll (the command above)
4. Wait for either the poll to return or the user to type something

### Two Feedback Paths

Both paths are valid and the user can use whichever is more natural:

1. **Browser UI (automatic)** — Use Keep/Cut buttons + comments, click Save Feedback.
   Claude detects the file automatically. Best for detailed, per-option feedback.
2. **Chat (manual)** — Describe preferences in the terminal. Claude parses and records it.
   Best for quick decisions or when the File System Access API is unavailable.

Tell the user both options exist. Emphasize that the browser path is automatic — they
don't need to do anything in the terminal after clicking Save Feedback.

**IMPORTANT: Do NOT read prior facets' full HTML option files during the crit loop.** Use
`crit-session.md` for prior decisions. See Section 8 (Context Efficiency) for the
full loading strategy.

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

### Save Feedback Implementation (Browser Path)

The compare.html **MUST** include this JavaScript for the Save Feedback button. This is
not optional -- without it, the browser-to-terminal feedback loop does not work.

**Include this exact JavaScript in every compare.html** (with `FACET_ID`, `ROUND_N`, and
`FEEDBACK_FILENAME` replaced with actual values):

```javascript
// --- REQUIRED: Save Feedback to Disk ---
// Replace these values when generating compare.html:
const FACET_ID = '{facet-id}';       // e.g., 'navigation-model'
const ROUND_N = {N};                  // e.g., 2
const FEEDBACK_FILENAME = 'feedback-round-{N}.json';

// Request notification permission on page load
if ('Notification' in window && Notification.permission === 'default') {
  Notification.requestPermission();
}

function collectFeedback() {
  const options = {};
  document.querySelectorAll('[data-option-id]').forEach(card => {
    const id = card.dataset.optionId;
    const keepBtn = card.querySelector('[data-action="keep"]');
    const cutBtn = card.querySelector('[data-action="cut"]');
    const comment = card.querySelector('textarea')?.value || '';
    let action = null;
    if (keepBtn?.classList.contains('active')) action = 'keep';
    if (cutBtn?.classList.contains('active')) action = 'cut';
    options[id] = { action, comment };
  });

  const overall = document.querySelector('#direction-notes')?.value || '';

  // Determine decision type
  let decision = 'refine';
  const keptCount = Object.values(options).filter(o => o.action === 'keep').length;
  const totalOptions = Object.keys(options).length;
  if (keptCount === 1 && Object.values(options).filter(o => o.action === 'cut').length === totalOptions - 1) {
    decision = 'lock';
  }

  return {
    facet: FACET_ID,
    round: ROUND_N,
    timestamp: new Date().toISOString(),
    options,
    overall,
    decision
  };
}

async function saveFeedback() {
  const feedback = collectFeedback();
  const json = JSON.stringify(feedback, null, 2);

  // Primary: File System Access API
  if ('showSaveFilePicker' in window) {
    try {
      const handle = await window.showSaveFilePicker({
        suggestedName: FEEDBACK_FILENAME,
        types: [{ description: 'JSON', accept: { 'application/json': ['.json'] } }]
      });
      const writable = await handle.createWritable();
      await writable.write(json);
      await writable.close();

      // Show success banner
      showBanner('Feedback saved! Claude is picking it up automatically.');

      // Fire OS notification
      if (Notification.permission === 'granted') {
        new Notification('Design Crit', { body: 'Feedback saved! Claude is processing it.' });
      }
      return;
    } catch (err) {
      if (err.name === 'AbortError') return; // User cancelled
      console.error('File System Access API failed:', err);
    }
  }

  // Fallback: show copyable JSON
  showFallbackModal(json);
}

function saveFeedbackWithDecision(decision) {
  // For Skip This Area or Decide For Me buttons
  const feedback = collectFeedback();
  feedback.decision = decision;
  if (decision === 'delegate') {
    feedback.delegate_context = document.querySelector('#delegate-context')?.value || '';
  }
  const json = JSON.stringify(feedback, null, 2);
  // Same save logic as above...
  if ('showSaveFilePicker' in window) {
    window.showSaveFilePicker({
      suggestedName: FEEDBACK_FILENAME,
      types: [{ description: 'JSON', accept: { 'application/json': ['.json'] } }]
    }).then(async handle => {
      const writable = await handle.createWritable();
      await writable.write(json);
      await writable.close();
      showBanner('Feedback saved! Claude is picking it up automatically.');
      if (Notification.permission === 'granted') {
        new Notification('Design Crit', { body: 'Feedback saved! Claude is processing it.' });
      }
    }).catch(err => {
      if (err.name !== 'AbortError') showFallbackModal(json);
    });
  } else {
    showFallbackModal(json);
  }
}

function showBanner(message) {
  const banner = document.createElement('div');
  banner.textContent = message;
  banner.style.cssText = 'position:fixed;top:56px;left:50%;transform:translateX(-50%);' +
    'background:#4ADE80;color:#0D0D0F;padding:12px 24px;border-radius:8px;font-size:14px;' +
    'font-weight:600;z-index:200;animation:fadeIn 0.2s ease-out';
  document.body.appendChild(banner);
  setTimeout(() => banner.remove(), 5000);
}

function showFallbackModal(json) {
  const modal = document.createElement('div');
  modal.innerHTML = `
    <div style="position:fixed;inset:0;background:rgba(0,0,0,0.7);z-index:300;display:flex;align-items:center;justify-content:center">
      <div style="background:#1A1A1E;border:1px solid #1E1E22;border-radius:8px;padding:24px;max-width:600px;width:90%">
        <h3 style="color:#F0F0F2;margin:0 0 8px">Save not available in this browser</h3>
        <p style="color:#8B8B92;font-size:13px;margin:0 0 16px">Copy this JSON and paste it into the terminal:</p>
        <textarea style="width:100%;min-height:200px;background:#111113;color:#F0F0F2;border:1px solid #1E1E22;border-radius:6px;padding:12px;font-family:monospace;font-size:12px;resize:vertical"
          readonly onclick="this.select()">${json}</textarea>
        <button onclick="this.closest('div[style]').parentElement.remove()"
          style="margin-top:12px;padding:10px 20px;background:#6366F1;color:white;border:none;border-radius:6px;cursor:pointer">Close</button>
      </div>
    </div>`;
  document.body.appendChild(modal);
}
```

**Required HTML structure for interactive controls** (each option card must include):

```html
<!-- Option card — data-option-id is REQUIRED for feedback collection -->
<div class="option-card" data-option-id="option-a">
  <!-- ... iframe, name, rationale sections ... -->

  <!-- Keep/Cut toggle buttons — REQUIRED -->
  <div class="option-controls">
    <button data-action="keep" onclick="toggleAction(this, 'keep')">Keep</button>
    <button data-action="cut" onclick="toggleAction(this, 'cut')">Cut</button>
  </div>

  <!-- Comment textarea — REQUIRED -->
  <textarea placeholder="What works? What doesn't?"></textarea>
</div>
```

```html
<!-- Global feedback area — below My Take section -->
<div class="feedback-area">
  <p class="instruction-text">Click Save Feedback and Claude will pick it up
  automatically. Or just tell Claude your thoughts in the chat.</p>

  <textarea id="direction-notes" placeholder="Overall thoughts or direction preferences..."></textarea>

  <div class="button-row">
    <button class="primary" onclick="saveFeedback()">Save Feedback</button>
    <button class="ghost" onclick="saveFeedbackWithDecision('skip')">Skip This Area</button>
    <button class="ghost" onclick="showDelegateContext()">Decide For Me</button>
  </div>

  <!-- Hidden until "Decide For Me" clicked -->
  <textarea id="delegate-context" style="display:none"
    placeholder="Any guidance? e.g., 'prioritize accessibility' or 'keep it simple'"></textarea>
</div>
```

**The Keep/Cut toggle JS:**

```javascript
function toggleAction(btn, action) {
  const card = btn.closest('[data-option-id]');
  const keepBtn = card.querySelector('[data-action="keep"]');
  const cutBtn = card.querySelector('[data-action="cut"]');

  if (btn.classList.contains('active')) {
    btn.classList.remove('active');
    return;
  }

  keepBtn.classList.remove('active');
  cutBtn.classList.remove('active');
  btn.classList.add('active');
}

function showDelegateContext() {
  const el = document.querySelector('#delegate-context');
  el.style.display = el.style.display === 'none' ? 'block' : 'none';
}
```

**Every compare.html you generate MUST include all of the above.** Do not generate a
compare view without interactive controls. The Keep/Cut buttons, comment textareas,
Save Feedback button, and File System Access API JavaScript are mandatory.

### Feedback Persistence (Chat Path)

When the user gives feedback in the chat instead of using the browser UI, Claude parses
their message and writes `feedback-round-N.json` itself. This ensures session resilience
regardless of which feedback path the user chose.

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

After each round, ask the user with increasing directness. Use plain, conversational
language — no jargon like "lock" or "converge":

- **Round 1:** "What do you think — want to keep refining, or do you already see the direction you like?"
- **Round 2:** "I think we're getting close. Ready to go with one of these, or want one more pass?"
- **Round 3:** "I'd recommend picking one and moving on. The remaining differences are small details we can work out later."
- **Round 4+:** "We've been on this one for a while. What's the ONE thing that's still bugging you? Let's nail that and move on."

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

1. User clicks "Decide For Me" in the browser (optionally adding context in the
   delegate textarea), OR says "you decide" in the chat.
2. If triggered from browser, compare.html writes feedback with `"decision": "delegate"`
   and any `"delegate_context"`. User types "done" in terminal.
3. Claude picks the best option based on: the brief, locked decisions from prior areas,
   design best practices, and any delegate context provided.
4. Claude writes a full rationale — same quality as a regular critique, not a silent auto-pick.
5. Claude locks the decision with `"decided_by": "llm"` in state.json.

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
3. **User signal extracted.** Read all feedback files for this facet. Distill the user's
   comments into a single sentence of design intent. Not "kept option A" but what their
   feedback *reveals* about their values and priorities. Write this to `crit-session.md`.
4. **Emerging preferences updated.** After 2+ facets locked, look across all user signals
   for recurring patterns. Update the Emerging Preferences section of `crit-session.md`.
5. **Constraints carry forward.** All subsequent facets inherit locked decisions plus the
   user's design voice. Read `crit-session.md` (not full HTML files) before generating
   new options.
6. **Overview page updates.** Show locked facets with checkmark, winner name, and a **Review**
   button that expands the full decision history inline.

### Constraint Propagation

Before generating options for any facet, read `.design-crit/crit-session.md`. This
file contains a text summary of every prior locked decision — enough to inform new options
without loading full HTML files into context.

**Do NOT read prior facets' full HTML option files** unless you are directly modifying them
(e.g., applying color to a locked layout wireframe). In that case, load only the specific
file you need to edit, one at a time.

Apply locked constraints and state them in the critique: "Building on the locked
[facet]: [summary]."

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

---

## 7. Chat Hygiene

### Suppress HTML Output in the Terminal

When generating HTML files (option files, compare.html, overview.html), **do not dump
the full HTML source into the chat.** The user does not need to see raw HTML — it makes
the conversation hard to follow.

Instead:
- Write HTML files silently using the Write tool.
- After writing, tell the user what you did in plain language: "I've generated 3 options
  and a comparison view. Opening it in your browser now."
- If the user asks to see the HTML source, show it then. Otherwise, keep it behind the
  scenes.

### Guide the User After Each Step

After opening a compare view or overview page, always tell the user:
1. What you just showed them and why
2. What you want them to do next
3. That they should come back to the chat when they're ready

Example after opening a compare view:
```
I've generated 3 navigation options and opened them side by side in your browser.

Take your time — for each option, you can mark it as Keep or Cut and leave
comments right in the browser. When you're done, click Save Feedback and I'll
pick it up automatically.

Or if you prefer, just tell me your thoughts here in the chat — something like
"keep A, cut C, tweak B's spacing" works great too.
```

### Generation Speed

HTML generation for the initial round can take a while since it produces multiple full
wireframe files plus the compare view. To keep this manageable:
- For round 1, keep wireframes focused and simple. Show the design concept clearly
  without excessive detail or placeholder content.
- Increase fidelity in later rounds as options narrow.
- Tell the user upfront: "This will take a moment — I'm building out the wireframes."

---

## 8. Context Efficiency

HTML option files can be large. A full design crit with 10+ facets generates substantial
content. These rules keep context usage sustainable across long sessions.

### The `crit-session.md` File

The orchestrator maintains `.design-crit/crit-session.md` — a single, compact file
summarizing every locked decision in plain text. Updated after each facet locks.

```markdown
# Crit Session Context

## Emerging Preferences
User consistently values: keyboard-first interaction, minimal visual clutter, power-user
density over consumer spaciousness. Prefers functional over decorative. Tends to cut
options that prioritize aesthetics over efficiency.

## What Exists
- **Screen Inventory** — 7 screens: Dashboard, Capture, Search, Collections, Tags, Settings, Onboarding. Locked option: option-b. User signal: "I want fewer screens that do more, not a screen for everything."
- **Edge States** — Instructional empty states with CTAs, inline validation, skeleton loading. Locked option: option-a. User signal: "Empty states should teach, not just decorate."

## How It's Arranged
- **Navigation** — Deep sidebar with keyboard shortcuts, collapsible. 280px default width. Locked option: option-a. User signal: "Keyboard shortcuts are non-negotiable. I want power users to never touch the mouse."
- **Layout** — Sidebar + content, 70/30 split, 12-column grid in content area. Locked option: option-c. User signal: "Content area needs to breathe. The sidebar is for nav, not content."

## How It Feels
- **Typography** — Inter for UI, JetBrains Mono for code. 1.25 scale, 6 levels. Locked option: option-b. User signal: "Readability over personality. This is a tool, not a brand."
```

**This file is the primary context source for constraint propagation AND user intent.**
It serves two purposes:
1. **Locked decisions** — what was decided, with enough specifics to constrain future facets.
2. **User signals** — a compressed one-line summary of WHY the user chose this direction,
   distilled from their feedback comments across rounds. This carries the user's design
   intent forward without loading raw feedback files.

The **Emerging Preferences** section at the top captures cross-cutting patterns — design
values that show up repeatedly across multiple facets. This is the user's "design voice"
that develops as the crit progresses. Use it to inform option generation for future facets
(e.g., if the user consistently prefers minimal, don't lead with maximalist options).

### Tiered HTML Loading

Follow these rules for when to load full HTML option files:

| Situation | What to Load |
|---|---|
| **Generating new options** for the current facet | Read `crit-session.md` for context. Do NOT read prior facets' HTML files. |
| **Refining a surviving option** from the current facet | Read ONLY that option's HTML file. |
| **Layering a new facet onto prior wireframes** (e.g., applying color to a locked layout) | Read ONLY the specific locked HTML file you are modifying, one at a time. |
| **design-direction synthesis** (Stage 4) | Read locked HTML files one at a time per deliverable section, not all at once. |
| **Reviewing a prior facet** (user clicks Review) | Read that single facet's locked option file and feedback files. |

**Never** load all locked HTML files into context simultaneously during the crit loop.

### Feedback File Loading

Only read the **most recent** feedback file for the current facet:
- Round 1: no prior feedback to read.
- Round 2+: read `feedback-round-(N-1).json` only. Do NOT read all prior rounds.
- Prior round history is preserved in the individual files and reconstructed by
  `design-direction` at synthesis time.

### Session Resilience

All state lives on disk. If a session hits its context limit or the user starts a new one:
- `/design-crit` reads `state.json` and picks up exactly where it left off.
- No conversation history is needed — `crit-session.md`, feedback files, and option
  HTML files are the source of truth.
- The orchestrator's re-entry handling (Step 5) summarizes progress from files alone.

Tell the user if context is getting long: "We've been at this for a while. You can start
a fresh session anytime — just run `/design-crit` and I'll pick up where we left off.
Everything is saved."
