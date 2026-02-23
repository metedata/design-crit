---
name: crit-onboarding-flows
description: Use when evaluating first-run experience, progressive disclosure, onboarding tutorials, and how new users learn to use the product.
---

# Onboarding Flows Crit

**Reference:** Read `reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing how new users learn this product. Onboarding is the bridge
between "I signed up" and "I get value." Every screen, tooltip, permission request, and
sample content either accelerates or blocks that journey. Most products fail here -- they
either dump everything on the user at once or provide so little guidance that users churn
before discovering the core value.

Onboarding sits in the structural lens. It depends on `screen-inventory` (the surfaces the
user will navigate) and `edge-states` (first-use empty states are onboarding surfaces). It
feeds into nearly every downstream facet: navigation (users must learn wayfinding), layout
(first-run variants of screens), component design (tooltips, coaches, wizards), content voice
(onboarding copy tone), and accessibility (onboarding must be keyboard and screen-reader
accessible).

---

## 1. Load Context

Read these files before generating anything:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (core loop, target users, complexity, differentiator)
3. `reference/crit-loop.md` -- shared crit loop mechanics
4. `reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Onboarding touches the entire product.
Pay special attention to:

- **Screen inventory** -- which screens exist, what the happy path flow looks like, how many
  screens the user must traverse to reach core value
- **Edge states** -- first-use empty states are the primary onboarding surfaces. Read what
  empty state approach was locked (instructional, illustrated, starter content, progressive
  disclosure)

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/onboarding-flows/`. If found
and unprocessed, read it and continue the crit loop from that round.

### Extract Brief Signals

From the brief, identify:

- **Core loop** -- what is the primary action the user must learn? This is the onboarding
  destination. Everything in the onboarding flow exists to get the user to this action.
- **Target users** -- technical sophistication, familiarity with similar products, motivation
  level. A developer onboarding to a CLI tool is different from a consumer onboarding to a
  social app.
- **Learning curve** -- how many concepts must the user learn before they can do the core
  task? Products with high concept count need progressive disclosure. Products with simple
  core loops need minimal onboarding.
- **Platform** -- mobile apps have system permission requests (camera, notifications, location)
  that must be sequenced. Web apps have fewer permission barriers.
- **Differentiator** -- what makes this product unique? Onboarding should highlight the
  differentiator, not generic features.

---

## 2. Time-to-Value Analysis

The most important metric for onboarding: how fast does the user reach the core value?

### Map the Path to First Value

Starting from account creation (or first visit), count every screen, form field, and decision
the user must complete before they experience the core loop.

```
[Sign up] -> [Email verify] -> [Profile setup] -> [Workspace config] -> [Invite team] ->
[First project] -> [CORE VALUE]
```

Each arrow is a potential drop-off. Count the steps. For each step, ask:
- Is this step required BEFORE the core value, or can it be deferred?
- Can this step be simplified (pre-fill, default, skip)?
- Does this step teach something essential, or is it administrative?

### The Deferral Test

For every step in the path, apply the deferral test:

| Step | Required Before Value? | Can Defer? | If Deferred... |
|---|---|---|---|
| Email verification | Maybe -- depends on security model | Yes: allow limited access unverified | Prompt after first value moment |
| Profile setup | No | Yes: use defaults, prompt later | Show setup CTA in profile area |
| Workspace config | Maybe | Partial: use smart defaults | Surface config when user hits a limitation |
| Invite team | No | Yes | Prompt after user has content to share |
| First project | Yes -- this IS the core value | No | This is the goal, not the blocker |

Every step that can be deferred SHOULD be deferred. Front-load only what is essential for the
core experience.

### Time-to-Value Targets

| Product Type | Acceptable Steps to Value | Notes |
|---|---|---|
| Consumer app | 1-3 | Must be near-instant. Social proof, sample content, or magic link login. |
| SaaS tool | 3-5 | Account + one setup step + core action. Template or wizard helps. |
| Enterprise software | 5-10 | More tolerance, but each step must feel productive. |
| Developer tool | 1-2 | Developers expect zero hand-holding. Quick start, then docs. |

If the path exceeds the target for this product type, flag it in the critique.

---

## 3. Onboarding Pattern Vocabulary

### Account Setup

The first gate. Keep it minimal.

**Progressive profiling** -- collect only what is needed now. Ask for name and email at
signup. Ask for role and company during the first session. Ask for preferences after the
user has context. Never front-load a 10-field form.

**Smart defaults** -- pre-fill every field you can. Detect timezone from browser. Default
to the most common options. Show defaults as editable, not blank fields.

**Social/SSO login** -- reduce signup friction to one click. Google, GitHub, Apple depending
on audience. Show password signup as secondary, not primary.

### Welcome and Orientation

The first thing the user sees after signup.

**Welcome screen** -- a single screen that sets expectations. "Here's what you can do." Keep
it to 3 bullet points max. Include a primary CTA to start the core task.

**Product tour / tooltip walkthrough** -- sequential tooltips pointing at key UI elements.
Use when the UI is complex and the user needs a spatial map.

Design rules for tooltip tours:
- Maximum 5 steps. Beyond 5, users dismiss and forget.
- Each tooltip must be dismissible AND skippable (skip all remaining).
- Tooltips should point at real, interactive UI -- not screenshots or diagrams.
- Trigger the action, not just explain it. "Click here to create your first project" with
  the create button highlighted, not "This button creates projects."
- Persist progress. If the user closes the tab mid-tour, resume where they left off.

**Contextual discovery** -- no upfront tour. Instead, surface guidance where the user
needs it, when they need it. A tooltip appears the first time the user visits a screen. A
callout appears when the user encounters a feature for the first time.

### Progressive Disclosure

Teach the product in layers. Do not show advanced features to beginners.

**Feature gating by usage** -- basic features visible by default. Advanced features revealed
after the user has used the basics. "You've created 5 projects. Did you know you can
create templates?"

**Collapsible complexity** -- show the simple interface first with an "Advanced options"
expandable section. The user learns the simple version first and graduates to the full
interface on their own.

**Level-based revelation** -- explicit "beginner / intermediate / advanced" modes that the
user can toggle, or that automatically unlock based on usage patterns.

### Permission Requests

On mobile (and increasingly on web), permissions must be requested at runtime.

**Timing principle:** Request permissions at the moment of relevance, not at first launch.
Do not ask for notification permissions before the user has content that generates
notifications. Do not ask for camera access until the user taps "take photo."

**Pre-permission primer** -- before the system dialog appears, show your own screen
explaining WHY you need this permission and WHAT the user gets. "We'd like to send you
notifications when your team mentions you. Allow notifications?" THEN trigger the system
dialog. Users are more likely to accept after understanding the benefit.

**Denial handling** -- if the user denies a permission, do not immediately re-ask. Show a
degraded experience with a persistent but non-intrusive option to enable later: "Notifications
are off. Enable them in Settings to get updates."

### Sample / Demo Content

Pre-populated content that shows the user what a "full" product looks like.

**When to use:** Products where the empty state does not communicate the value proposition.
A project management tool with no projects looks like an empty spreadsheet. A project
management tool with a sample project shows the user what they are building toward.

**Design rules:**
- Label sample content clearly: "[Sample] Getting Started Project" or a banner "This is
  sample data. Delete it anytime."
- Make sample content editable and deletable -- the user learns by modifying it.
- Use realistic content that matches the product's domain. A CRM sample should have plausible
  contact names, not "Test User 1."
- Do not count sample content toward usage limits or storage.

### Empty State as Onboarding

The locked edge states facet defined empty state designs. Here, leverage those empty states
as the primary onboarding mechanism.

**Content is the teacher** -- instead of explaining the product, prompt the user to create
their first piece of content. "No notes yet. Write your first note." The act of creating
teaches the product better than any tour.

**Template picker** -- instead of a blank canvas, offer 3-5 templates that pre-populate
the first item. "Start from: Blank / Meeting Notes / Project Brief / Weekly Plan." Templates
reduce blank-canvas anxiety and teach by example.

---

## 4. Artifact Type

Generate **onboarding flow wireframes with annotated step sequences**.

Each option file (`option-{x}.html`) must contain:

### Flow Diagram

A visual sequence showing every screen the user sees from signup through first core value moment:

```
[Signup] -> [Welcome] -> [Setup Step 1] -> [Core Action] -> [First Value]
```

Render as connected boxes with labeled transitions. For each step, annotate:
- Step number and estimated time to complete
- Whether this step is required or deferrable
- What the user learns at this step

### Screen Wireframes

For each step in the flow, render a wireframe-level screen design:

- **Signup screen** -- form fields, social login options, CTA
- **Welcome/orientation** -- content, illustrations (placeholder), CTA
- **Setup steps** -- form fields with smart defaults shown, skip options
- **Permission primers** -- explanation + benefit statement + accept/decline
- **First-use screen** -- the core screen with onboarding overlay (tooltip, callout, or
  empty state CTA)
- **First value moment** -- what the user sees after completing their first core action

### Progressive Disclosure Map

Show what is visible at each user maturity stage:

| Feature / UI Element | First Session | After 3 Sessions | After 10 Sessions |
|---|---|---|---|
| Core action button | Highlighted with tooltip | Normal | Normal |
| Advanced settings | Hidden | "Discover" prompt | Visible |
| Keyboard shortcuts | Not shown | Tooltip on hover | Cheat sheet available |
| Team features | "Invite team" CTA | Sidebar section | Full team management |

### Permission Request Sequence

For mobile apps or permission-requiring features, document:

| Permission | When Requested | Primer Message | Denial Fallback |
|---|---|---|---|
| Notifications | After first content created | "Get updates when..." | Banner with Settings link |
| Camera | When user taps photo upload | "Add photos to..." | File picker fallback |
| Location | When user opens map feature | "Find nearby..." | Manual location entry |

---

## 5. Evaluation Criteria

Score each option against these questions in your critique.

### Time to Core Value

- How many screens between signup and the first core action?
- How many form fields must the user fill before reaching value?
- Are deferrable steps actually deferred, or front-loaded unnecessarily?
- Could a user reach the core value in under 60 seconds? If not, why not?

### Learn by Doing

- Does the onboarding teach through action (creating, exploring) or through reading
  (tutorials, slides, videos)?
- Are tooltips paired with real interactions ("Click this to create") rather than
  passive explanations ("This button creates")?
- Does the user produce something in the onboarding? (A note, a project, a setting.)
  Users who create in onboarding retain better than users who only read.

### Progressive Disclosure Quality

- Is complexity hidden from new users without being inaccessible to advanced users?
- Can the user discover advanced features naturally through use, without a separate
  "advanced tutorial"?
- Are the disclosure triggers well-timed? (Not too early: user is overwhelmed. Not too
  late: user missed the feature for weeks.)

### Permission Request Timing

- Are permissions requested at the moment of relevance, or front-loaded?
- Does every permission request include a benefit statement before the system dialog?
- Is the denial experience graceful? Can the user still use the product without the
  permission?

### Dropout Prevention

- Can the user exit the onboarding at any point and resume later?
- Is progress saved if the user closes the tab/app mid-onboarding?
- Is there a "skip for now" option on every non-essential step?
- Does the onboarding adapts to users who already understand the product (skip tutorial
  for users who immediately start using features)?

### Brief Alignment

- Does the onboarding highlight the differentiator from the brief?
- Is the complexity level appropriate for the target user from the brief?
- Does the onboarding respect the platform conventions from the brief?

---

## 6. Option Generation Strategy

### Round 1: Diverge

Generate 2-3 options that make different bets about onboarding philosophy.

**Axes of variation:**

- **Guided vs Exploratory** -- one option walks the user through a structured sequence
  (wizard, stepped flow). Another drops the user into the product with contextual hints
  and lets them explore.
- **Minimal vs Comprehensive** -- one option has the shortest possible path to value (skip
  everything deferrable, smart defaults, one-click start). Another provides thorough setup
  to ensure the product is configured correctly from the start.
- **Content-First vs Tour-First** -- one option uses the first empty state as the
  onboarding surface, prompting the user to create their first piece of content. Another
  starts with a product tour showing the UI before the user creates anything.
- **Template vs Blank Canvas** -- one option offers starter templates that teach by example.
  Another starts with a blank canvas and progressive tooltips.

### Naming Options

Name each option by its onboarding philosophy:

- "Zero Setup" (skip everything, smart defaults, straight to core action)
- "Guided Wizard" (stepped flow with clear progress, each step teaches something)
- "Content First" (empty state onboarding, create immediately, learn by doing)
- "Tour & Template" (brief tooltip tour then template picker)
- "Progressive Coach" (no upfront onboarding, contextual guidance over first week)
- "Sample Playground" (pre-populated with sample content, user learns by exploring)

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Mix approaches: use one option's signup simplicity with another's first-use experience
- Add or remove setup steps based on user feedback about what is essential vs deferrable
- Adjust tooltip tour length (more or fewer steps)
- Refine permission request timing and primer copy
- Adjust progressive disclosure triggers based on feedback about pacing

---

## 7. Common Patterns and Trade-offs

Reference these patterns when explaining options.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| **Tooltip walkthrough** | Complex UI with many features, first-time only | Users dismiss without reading; more than 5 steps and completion drops sharply |
| **Wizard/stepped flow** | Products requiring setup (config, integrations, team) | Feels slow for repeat signups; needs skip option; each step must deliver visible value |
| **Empty state onboarding** | Simple products with one core action, content-creation apps | Does not work if the empty state does not communicate what to create; requires strong CTA copy |
| **Sample/demo content** | Products where empty state is not representative of value | Users confuse sample data with their own; must be clearly labeled and deletable |
| **Video walkthrough** | Products with complex workflows, visual/creative tools | Users skip videos; not accessible without captions; goes stale when UI changes |
| **Contextual coach marks** | Products where features are discovered over time | Can feel random; hard to ensure coverage; users may never trigger some marks |
| **Checklist onboarding** | Products with multiple setup steps, SaaS tools | Gamification can feel manipulative; checklist items must be genuinely useful, not busywork |
| **Blank canvas + quick actions** | Developer tools, creative apps, power user tools | New users freeze at blank canvas; need at least a prompt or template to start |

---

## 8. Compare View Specifics

When generating `compare.html`, follow `crit-loop.md` with these additions:

- **Omit the dark mode toggle** for this facet. Onboarding flow wireframes are structural,
  not visual.
- Each option card must show the complete flow from signup through first value moment. The
  user needs to see the full journey, not isolated screens.
- Include the step count and estimated time prominently in each option card: "6 steps,
  ~90 seconds to first value."
- The comparative take must address: which option gets to value fastest, which best handles
  users who already know what they are doing, and whether the onboarding highlights the
  product's differentiator.

---

## 9. Relationship to Adjacent Facets

### Screen Inventory (locked before)

The locked screen inventory defines which surfaces exist. Onboarding adds first-use variants
of those screens (welcome overlays, empty states with guidance, tooltip layers) but does not
add new screens to the inventory. If the onboarding requires a screen that does not exist,
flag it: "This onboarding approach needs a welcome screen not in the inventory. Add it?"

### Edge States (locked before)

Empty state designs from the edge states facet are onboarding surfaces. The first-use empty
state IS the onboarding for that screen. Read the locked empty state approach and build on
it. Do not redesign empty states -- layer onboarding onto them.

### Navigation (runs after)

The locked onboarding must work with whatever navigation model is chosen later. Design
onboarding to be navigation-agnostic where possible. If the onboarding depends on a specific
navigation pattern (e.g., "sidebar is visible during tutorial"), note this as a constraint
for the navigation facet.

### Content Voice (runs after)

All onboarding copy (welcome messages, tooltip text, permission primers, CTA labels) will
be refined when the content voice facet runs. For now, write functional copy that communicates
the intent clearly. Mark it as draft: the voice facet will tune the tone.

### Accessibility (runs after)

The accessibility audit will evaluate: keyboard navigation through onboarding flows, screen
reader announcements for tooltips and coach marks, focus management during wizard steps, and
whether the onboarding is completable without a mouse.

---

## 10. Locking and Handoff

When the user locks an onboarding option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: onboarding philosophy (guided/exploratory/content-first),
   step count to first value, deferral strategy (which steps are deferred), and key patterns
   used (tooltips, wizard, templates, sample content).
3. The locked onboarding flow constrains downstream facets. Navigation must accommodate the
   onboarding surfaces. Content voice must write the onboarding copy. Components may need
   tooltip or coach mark patterns.
4. Announce: "Onboarding locked as [name]. [step count] steps to first value. [summary].
   Navigation and content voice will build on this flow."

---

## 11. Constraints

- Every option must show the complete path from signup to first value. Do not design
  partial onboarding flows that stop at "and then the user figures it out."
- Step count and estimated time must be included for every option. These are the primary
  comparison metrics.
- Deferral decisions must be explicit. For every step, state whether it is required or
  deferred. Do not leave deferral ambiguous.
- Wireframes are structural (low-to-medium fidelity). Use gray boxes, text, and simple shapes.
  No color, no final copy, no pixel-perfect layouts. The onboarding structure is what matters.
- If the option includes a tooltip tour, show every tooltip in sequence with its target element
  highlighted. Do not describe the tour -- show it.
- Permission request flows must include the primer screen, the system dialog representation,
  and the denial fallback. All three states are required.
- Sample content (if proposed) must use realistic, domain-appropriate data. Not "Test 1" or
  "Lorem ipsum."
