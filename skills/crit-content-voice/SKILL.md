---
name: crit-content-voice
description: Use when evaluating microcopy, error messages, CTA language, terminology consistency, and the overall voice and tone of UI text.
---

# Content & Voice Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing the content voice and microcopy for this project. Voice is a
sensory facet -- it applies to surfaces, flows, and components that are already locked. Your
job: generate voice direction options with concrete microcopy specimens, evaluate them against
clarity and brand tone, and guide the user to a locked content voice.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (target users, differentiator, brand tone)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Voice applies to every surface that has text.
Pay special attention to:

- **Screen inventory** -- which screens exist, what flows the user navigates
- **Edge states** -- empty, error, loading states all need microcopy
- **Component design** -- buttons, forms, modals, tooltips all carry text
- **Navigation** -- menu labels, breadcrumbs, section titles
- **Color system** -- status colors (error red, success green) pair with status messages

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/content-voice/`. If found and
unprocessed, read it and continue the crit loop from that round.

---

## 2. Voice Dimensions

Every voice option must declare its position on four dimensions. These positions drive every
microcopy decision in the option.

### Dimension 1: Formal <-> Casual

| Formal | Casual |
|---|---|
| "Your session has expired." | "You've been logged out." |
| "Please enter a valid email address." | "That email doesn't look right." |
| "Document saved successfully." | "Saved!" |

### Dimension 2: Serious <-> Playful

| Serious | Playful |
|---|---|
| "No results found." | "Nothing here yet -- try a different search?" |
| "Are you sure you want to delete?" | "Gone forever. You sure?" |
| "Error processing your request." | "Oops, something broke." |

### Dimension 3: Technical <-> Simple

| Technical | Simple |
|---|---|
| "API rate limit exceeded (429)." | "Too many requests. Try again in a minute." |
| "Connection timed out after 30s." | "Taking too long. Check your connection." |
| "Merge conflict on line 42." | "Two changes conflict. Pick which one to keep." |

### Dimension 4: Authoritative <-> Friendly

| Authoritative | Friendly |
|---|---|
| "You must complete all required fields." | "A few fields still need your attention." |
| "Access denied." | "You don't have access to this yet." |
| "Upgrade to continue." | "Want to unlock this? Here's how." |

### Positioning Each Option

For each option, state its position as coordinates: e.g., "Slightly formal, neutral
seriousness, simple, friendly." These coordinates should be consistent -- every microcopy
specimen in the option must reflect the declared position.

---

## 3. Microcopy Areas

Each option must provide concrete specimens for ALL of these areas. Do not describe the
voice -- demonstrate it with real text.

### Button Labels and CTAs

Write specimens for these common actions:
- Primary action (e.g., save, create, send, publish)
- Secondary action (e.g., cancel, go back, dismiss)
- Destructive action (e.g., delete, remove, revoke)
- Progressive action (e.g., next step, continue, upgrade)

**CTA principles:**
- Use specific action verbs, not vague ones. "Save changes" not "Submit." "Create project"
  not "OK."
- Front-load the verb. "Delete account" not "Account deletion."
- Match the CTA to what happens next. If clicking "Save" also closes the modal, consider
  "Save and close."
- Calibrate urgency. "Get started" for onboarding, "Upgrade now" for upsell. Do not make
  every CTA feel urgent.

### Placeholder Text

Write specimens for:
- Search input ("Search by name, tag, or keyword...")
- Text input (email, name, description)
- Empty textarea ("Write your thoughts here...")

**Placeholder principles:**
- Suggest what to type, not what the field is. "jane@example.com" not "Email address."
- Keep it short. Placeholders disappear on focus and should not carry critical instructions.
- Do not use placeholder text as a substitute for labels.

### Tooltips and Helper Text

Write specimens for:
- A form field that needs explanation (e.g., password requirements)
- A feature that is not self-explanatory (e.g., "What does this setting do?")
- A disabled element (e.g., why a button is grayed out)

**Helper text principles:**
- Answer "why" or "what happens," not "what this is." The label already says what it is.
- Keep tooltips to one sentence. If it needs more, it belongs in documentation.
- Disabled-state helper text should explain how to enable: "Complete all required fields to
  submit."

### Error Messages

Write specimens for these error types:
- Validation error (form field)
- Server error (unexpected failure)
- Permission error (access denied)
- Network error (offline or timeout)
- Conflict error (e.g., someone else edited this)

**Error message formula: WHAT + WHY + NEXT**
1. **What happened** -- state the problem clearly, no jargon
2. **Why** -- if known and helpful, explain the cause
3. **What to do next** -- give the user a concrete action

Examples:
- BAD: "Error 422." (no what, no why, no next)
- BAD: "Something went wrong." (vague what, no why, no next)
- GOOD: "That email is already registered. Sign in instead, or use a different email."
  (what: email exists; why: already registered; next: sign in or change email)

### Empty States

Write specimens for:
- First use (no content yet)
- Search with no results
- Filtered view with no matches
- Error state that emptied a view

**Empty state principles:**
- Acknowledge the emptiness without making the user feel they did something wrong.
- Include a next action when possible: "No projects yet. Create your first one."
- Adjust tone to context. A first-use empty state can be warmer ("Welcome! Let's get
  started.") than a no-results state ("No matches. Try broadening your search.").

### Confirmation Dialogs

Write specimens for:
- Destructive action confirmation (delete, remove)
- State-changing confirmation (publish, send, submit)
- Irreversible action warning

**Confirmation principles:**
- State what will happen, not what you are asking. "This will permanently delete 3 files"
  not "Are you sure?"
- Make the confirm button specific. "Delete 3 files" not "Yes."
- Make the cancel button safe and obvious. "Keep files" or "Cancel."

### Navigation Labels

Write specimens for:
- Main navigation items (based on locked screen inventory)
- Breadcrumbs
- Section headers within a page
- Tab labels

**Navigation principles:**
- Keep labels short: 1-2 words for top-level nav, 2-3 for sub-navigation.
- Use nouns for destinations ("Projects," "Settings"), verbs for actions ("Create," "Import").
- Be consistent: if one nav item is a noun, they all should be.

---

## 4. Terminology Consistency

A terminology glossary is required for every option. Inconsistent terminology is one of the
most common and damaging voice failures.

### Building the Glossary

Identify the 10-15 most important domain terms for this product. For each, define:
- The canonical term (the one you always use)
- Rejected alternatives (and why)
- Context note (when to use it)

Example for a project management tool:
```
| Canonical      | Rejected         | Context                          |
|----------------|------------------|----------------------------------|
| Project        | Workspace, Board | Top-level container              |
| Task           | Item, Ticket     | Individual work unit             |
| Assign         | Delegate, Add to | Giving someone a task            |
| Due date       | Deadline         | When a task should be completed  |
| Archive        | Hide, Remove     | Soft-delete; recoverable         |
| Delete         | Remove, Trash    | Hard-delete; not recoverable     |
```

### Consistency Rules

- Pick one term and use it everywhere. "Task" in the nav, "task" in the button, "task" in
  the tooltip, "task" in the error message.
- Do not synonym-rotate for variety. Variety in terminology causes confusion.
- If the brief mentions specific domain language (e.g., medical, legal, engineering), use
  that language. Do not simplify domain terms that the target user expects.

---

## 5. Generating Options

### Round 1: Diverge

Generate 2-3 voice options. Each must stake out a different position on the voice dimensions
and demonstrate it with complete microcopy specimens across all areas.

Example option set for a developer collaboration tool:

- **Direct Technical** -- formal, serious, technical, authoritative. Treats the user as a
  peer engineer. Minimal hand-holding. Error messages include error codes. CTAs are terse
  and precise.
- **Friendly Guide** -- casual, neutral, simple, friendly. Treats the user as someone who
  wants to get things done without friction. Error messages are plain language. CTAs are
  warm and encouraging.
- **Confident Minimal** -- slightly formal, slightly playful, simple, authoritative. Few
  words, high confidence. Every piece of text earns its place. Microcopy is short but not
  cold.

Each option HTML must include:
1. Voice dimension chart: visual positioning on all four axes
2. Complete microcopy specimens for every area in section 3 -- rendered in context (inside
   button shapes, form fields, dialog boxes, empty state cards) using the locked visual
   styles (colors, typography, density)
3. Terminology glossary table
4. A "Tone Comparison Board" showing the same three scenarios (successful action, error,
   empty state) written in this option's voice side by side for quick scanning

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:
- Adjust tone on one dimension (e.g., "a bit less playful in error messages")
- Rewrite specific specimens the user flagged
- Merge the error message style from one option with the CTA style from another
- Expand the terminology glossary based on user input

---

## 6. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- quote specimens,
name the dimension, cite the brief.

### Tone Consistency
- Does every specimen reflect the declared voice dimensions?
- Are there specimens that feel off-brand (e.g., a playful error message in an otherwise
  serious option)?
- Does the tone flex appropriately by context (errors can be slightly more serious than
  success messages) without losing the overall voice?

### Error Message Quality
- Does every error message follow the WHAT + WHY + NEXT formula?
- Can a user who reads only the error message take the correct next action?
- Do error messages avoid blame language ("you failed to..." vs "that didn't work...")?

### CTA Clarity
- Does every CTA clearly communicate what will happen when clicked?
- Are destructive actions distinguishable from safe actions in language?
- Do CTAs use specific verbs, not generic ones?

### Terminology Consistency
- Is the glossary complete for this product's core concepts?
- Are rejected alternatives genuinely excluded from all specimens?
- Would a new team member know which term to use for any concept?

### Audience Fit
- Does the voice match the target user from the brief?
- Is the technical level right? (A developer tool can say "API"; a consumer app should not.)
- Does the voice respect the user's context of use? (Urgent errors should not be playful.)

---

## 7. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Include the dark mode toggle** if the locked color system has dark mode, since microcopy
  specimens are rendered in visual context.
- Each option card's iframe must show specimens in context (inside UI components), not as a
  list of text strings. The user needs to read microcopy where it will actually appear.
- Include a "Tone Board" highlight at the top of each option card showing the same three
  scenarios across options for fast comparison.
- The comparative take ("My Take") must address: which voice best fits the target user, which
  has the strongest error messages, and whether the terminology is consistent. If one option
  has the best tone but another has better error copy, say so. State your recommendation.

---

## 8. Locking and Handoff

When the user locks a content voice option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: voice dimension positions (e.g., "casual, neutral,
   simple, friendly"), key terminology decisions, and the error message approach.
3. The locked option file becomes the voice reference for the accessibility audit (screen
   reader announcements, ARIA labels, alt text use the locked voice).
4. The terminology glossary from the locked option should inform all future content
   decisions and feeds into `decisions.md` at the direction stage.

---

## 9. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| Casual throughout | Consumer apps, young audience, friendly brand | Can feel unprofessional for serious tasks |
| Formal with casual errors | Professional tool that softens during failures | Tonal whiplash if the gap is too wide |
| Minimal wordcount | Power tools, frequent-use apps, developer audiences | Can feel cold or unhelpful for new users |
| Warm and guiding | Onboarding-heavy, complex products, broad audience | Can feel patronizing to expert users |
| Technical precision | Developer tools, engineering audiences | Alienates non-technical stakeholders |
| Context-adaptive tone | Different tone for errors vs success vs empty | Hard to keep consistent; needs clear rules |

---

## 10. Relationship to Adjacent Facets

### Edge States (locked before)

Edge states defined WHAT empty, error, and loading states exist. Content voice defines HOW
they speak. Read the locked edge state wireframes to know which states need copy.

### Component Design (locked before)

Buttons, modals, forms defined the UI containers. Content voice fills them with text.
Respect component sizing -- if the locked button is compact, the CTA must be short.

### Typography (locked before)

The locked type scale determines how much text fits where. Long microcopy on a small type
scale creates readability issues. Write to the space available.

### Accessibility (runs after)

The accessibility audit will evaluate locked microcopy for screen reader clarity, reading
level, and cognitive load. Write with screen readers in mind: be direct, avoid idioms that
do not translate, and front-load important information.

---

## 11. Constraints

- Every microcopy specimen must be demonstrated in context (inside a UI element), not listed
  as raw text. The option HTML renders specimens inside buttons, form fields, dialogs, etc.
- The terminology glossary must cover at least 8 domain terms. If the product is simple,
  identify 8 -- they exist.
- Error messages must follow the WHAT + WHY + NEXT formula. No exceptions in any option.
- Do not write placeholder microcopy ("Lorem ipsum," "Click here," "Error"). Every word in
  every option must be a real specimen that could ship.
- Voice options must differ on at least two dimensions. Two options that are both "casual,
  playful, simple, friendly" with slightly different wording are not distinct enough.
- Apply locked visual styles (colors, typography, density) to specimen rendering when those
  facets are available. Voice specimens should look like the real product.
