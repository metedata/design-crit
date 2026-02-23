---
name: crit-motion
description: Use when evaluating animation, transitions, micro-interactions, and motion design in a user interface.
---

# Motion Design Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing motion and animation in this product. Motion is a contextual
facet -- it applies when the product uses transitions, animations, or micro-interactions to
communicate state changes, guide attention, or reinforce brand personality. Your job:
evaluate easing curves, duration scales, transition patterns, and reduced motion
alternatives, then guide the user to a locked motion direction.

Motion is the temporal dimension of design. It answers: how does the interface change between
states? Good motion feels invisible -- things move naturally and the user's mental model
stays intact. Bad motion is distracting, slow, or disorienting.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (brand tone, platform, audience, performance constraints)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Motion applies to all prior structure and
visual decisions. Pay special attention to:

- **Navigation** -- transitions between screens, menu open/close, tab switching animations
- **Component design** -- modals, drawers, dropdowns, accordions all have enter/exit motion
- **Edge states** -- loading indicators, skeleton screens, error state transitions
- **Elevation/shape** -- surface layers that animate (cards lifting, sheets sliding)
- **Color system** -- color transitions on hover/focus/active states
- **Layout** -- layout shifts during responsive changes, content reflow animation
- **Density/spacing** -- compact mode toggle animation, collapsible panel motion

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/motion/`. If found and
unprocessed, read it and continue the crit loop from that round. Do not regenerate round 1.

---

## 2. Duration Scale

Every option must define a duration scale. Timing is the most impactful motion variable --
too fast feels jarring, too slow feels sluggish.

### Duration Tiers

| Tier | Range | Use For |
|---|---|---|
| **Micro** | 100-150ms | Color changes, opacity shifts, icon state changes, focus rings |
| **Fast** | 150-250ms | Button presses, checkbox toggles, tooltip show/hide, small reveals |
| **Standard** | 250-400ms | Modal open/close, dropdown expand, panel slide, tab transitions |
| **Emphasis** | 400-700ms | Page transitions, large surface reveals, onboarding animations |
| **Dramatic** | 700-1000ms | Hero animations, celebration moments, first-run sequences. Use sparingly. |

### Duration Tokens

Define named tokens for the scale, matching the locked spacing token pattern:

```
duration-micro:     100ms
duration-fast:      200ms
duration-standard:  300ms
duration-emphasis:  500ms
duration-dramatic:  800ms
```

Each option HTML must render duration tokens as animated demonstrations: a box moves across
the screen at each duration, so the user can feel the timing.

### Duration Principles

- **Smaller elements animate faster.** A tooltip (fast) should not take as long as a
  full-screen modal (standard to emphasis).
- **Enter is slightly slower than exit.** Users need time to perceive what is appearing.
  Exit can be faster because the content is leaving attention. Ratio: enter 1.0x, exit 0.8x.
- **Nearby elements animate faster than distant ones.** A dropdown that opens below its
  trigger (fast) should be quicker than a panel sliding in from off-screen (standard).
- **Repeated actions animate faster.** A list item that expands on first click (standard)
  can animate faster on subsequent clicks (fast) or not animate at all.

---

## 3. Easing Curves

Easing determines how motion accelerates and decelerates. It is the difference between
natural and robotic motion.

### Core Easing Functions

| Easing | CSS Value | Character | Use For |
|---|---|---|---|
| **Ease-out** | `cubic-bezier(0, 0, 0.2, 1)` | Decelerating. Fast start, gentle stop. | Elements entering the screen, appearing. Things arriving. |
| **Ease-in** | `cubic-bezier(0.4, 0, 1, 1)` | Accelerating. Gentle start, fast finish. | Elements leaving the screen, disappearing. Things departing. |
| **Ease-in-out** | `cubic-bezier(0.4, 0, 0.2, 1)` | S-curve. Smooth start and stop. | Elements moving between positions on screen. State transitions. |
| **Linear** | `linear` | Constant speed. No acceleration. | Progress bars, continuous spinners, opacity fades. |
| **Spring** | Custom (damped harmonic) | Bouncy, organic overshoot. | Playful interfaces, draggable elements, pull-to-refresh. |
| **Snappy** | `cubic-bezier(0.2, 0, 0, 1)` | Aggressive deceleration. Responsive feel. | Touch responses, button presses, quick reveals. |

### Easing Selection Rules

- **Enter: ease-out.** Arriving elements decelerate into their final position.
- **Exit: ease-in.** Departing elements accelerate out of view.
- **Move: ease-in-out.** Elements changing position move with an S-curve.
- **User-initiated: snappy/spring.** Direct manipulation (drag, swipe, pull) uses responsive curves.
- **System-initiated: standard ease-out.** Automatic transitions use predictable deceleration.
- **Continuous: linear.** Looping or progress animations use constant speed.

### Custom Curve Definitions

Each option must define its easing tokens:

```
ease-enter:     cubic-bezier(0, 0, 0.2, 1)
ease-exit:      cubic-bezier(0.4, 0, 1, 1)
ease-move:      cubic-bezier(0.4, 0, 0.2, 1)
ease-spring:    cubic-bezier(0.2, 0.8, 0.2, 1.2)
ease-snappy:    cubic-bezier(0.2, 0, 0, 1)
```

Show easing curves in each option HTML as animated demonstrations and as visual curve
diagrams (plot the bezier on a small graph).

---

## 4. Transition Patterns

Define how common UI state changes are animated.

### Enter/Exit Patterns

| Pattern | Description | Use For |
|---|---|---|
| **Fade** | Opacity 0 -> 1 (enter), 1 -> 0 (exit) | Tooltips, toasts, overlays. Subtle, non-directional. |
| **Slide** | Translate from off-screen or offset position | Drawers, panels, sheets. Directional, shows origin. |
| **Scale** | Scale from 0.95 -> 1.0 (enter) with fade | Modals, dialogs, popovers. Centered, focuses attention. |
| **Expand** | Height/width from 0 to auto | Accordions, collapsible sections. Inline, preserves flow. |
| **Morph** | Element transforms shape/position to become another element | Shared element transitions, FAB to dialog. Advanced. |

### State Transition Patterns

| Transition | Animation | Notes |
|---|---|---|
| **Page/route change** | Fade, slide, or shared element | Keep under 300ms. Content should not bounce. |
| **Tab switch** | Instant (no animation) or subtle fade | Tabs should feel instant. Heavy animation slows perceived performance. |
| **Accordion expand** | Height expand + fade content in | Animate the container, not the content. |
| **Dropdown open** | Scale from anchor point + fade | Origin at the trigger element, not center of screen. |
| **Toast/snackbar** | Slide from edge + fade | Enter from bottom or top edge. Auto-dismiss with fade. |
| **Modal open** | Overlay fade + content scale-up | Two layers: backdrop fades in, content scales up with slight delay. |
| **Skeleton to content** | Crossfade or subtle pulse-to-content | Never have a jarring pop when content loads. |

For each transition pattern used in the locked components, specify: duration tier, easing
function, and the specific transform properties.

---

## 5. Micro-interactions

Micro-interactions are small, single-purpose animations that provide feedback for user
actions. They make the interface feel alive and responsive.

### Core Micro-interactions

| Interaction | Animation | Duration |
|---|---|---|
| **Button press** | Slight scale down (0.97-0.98) on press, return on release | Micro (100ms) |
| **Hover highlight** | Background color transition, subtle elevation lift | Micro (100-150ms) |
| **Focus ring** | Ring appears with fade or scale, not instant | Fast (150ms) |
| **Toggle switch** | Thumb slides, track color transitions | Fast (200ms) |
| **Checkbox check** | Checkmark draws in with stroke animation or scales in | Fast (200ms) |
| **Loading spinner** | Continuous rotation, linear easing | Continuous |
| **Pull to refresh** | Elastic overscroll, spinner emerges at threshold | Spring easing |
| **Swipe action** | Element follows finger, action reveals beneath | Direct manipulation, snappy |
| **Ripple effect** | Expanding circle from touch point (Material convention) | Fast (200-300ms) |

### Micro-interaction Principles

- **Every user action deserves feedback.** If the user clicks and nothing visually responds
  for 100ms, the interface feels broken.
- **Feedback must be proportional.** A button press gets a micro animation. A purchase
  confirmation gets an emphasis animation. Proportion signals importance.
- **Avoid animation for its own sake.** If removing the animation would not lose information
  or feedback, remove it.
- **Consistent across similar elements.** All buttons press the same way. All toggles slide
  the same way. Inconsistent micro-interactions feel buggy.

---

## 6. Scroll-Triggered Animation

Evaluate scroll-based motion patterns when the product uses them.

### Common Scroll Patterns

| Pattern | Description | Use When |
|---|---|---|
| **Reveal on scroll** | Elements fade/slide in as they enter the viewport | Landing pages, marketing sections, long-form content |
| **Parallax** | Background layers move at different speeds | Hero sections, storytelling pages. Use with caution. |
| **Sticky transitions** | Elements stick, transform, then release as user scrolls | Feature showcases, product tours |
| **Progress indicator** | Reading progress bar advances with scroll | Long articles, multi-section pages |
| **Scroll-linked animation** | Animation progress tied directly to scroll position | Product showcases, interactive stories |

### Scroll Animation Constraints

- **Performance:** Scroll-triggered animations must not cause jank. Use `will-change`,
  `transform`, and `opacity` only. Never animate layout properties (width, height, top, left)
  on scroll.
- **Accessibility:** Scroll-linked animations must respect `prefers-reduced-motion`. Replace
  with static layout when reduced motion is active.
- **Predictability:** The user must feel in control. Scroll hijacking (where the page
  scrolls to predetermined positions) violates user expectation. Avoid it.
- **One-shot vs repeating:** Elements that reveal on scroll should animate once, not every
  time they enter the viewport. Use an intersection observer with a "played" flag.

---

## 7. Reduced Motion

Every option MUST include a reduced motion alternative. This is not optional.

### Implementation Requirements

Respect the `prefers-reduced-motion: reduce` media query:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
  }
}
```

### What Reduced Motion Means

- **Remove non-essential animation:** decorative motion, scroll reveals, parallax, hover
  effects.
- **Keep essential state feedback:** instant state changes are fine (opacity 0 to 1 with
  no transition). The user still needs to know what changed.
- **Replace motion with opacity:** instead of sliding a modal in, instantly show it at
  full opacity. Instead of animating an accordion open, instantly reveal the content.
- **Keep loading indicators:** spinners and progress bars are functional. Replace with
  a static "Loading..." text if the spinner itself is problematic (vestibular triggers).

### Reduced Motion in the Option HTML

Each option must include a toggle that demonstrates the reduced motion experience. Show
side-by-side: standard motion and reduced motion for at least 3 key transitions.

---

## 8. Generating Options

### Round 1: Diverge

Generate 2-4 motion options. Each must make a different bet about motion personality,
duration scale, or animation density. Maximize variety.

Example option set for a productivity SaaS:

- **Snappy Minimal** -- fast durations, snappy easing, micro-interactions only. No page
  transitions, no scroll animations. Motion exists solely for user feedback. Feels instant
  and responsive. Trade-off: no personality, no delight moments.
- **Smooth Flow** -- standard durations, ease-in-out curves, page transitions and modal
  animations. Polished but restrained. Motion communicates state changes and spatial
  relationships. Trade-off: adds 200-300ms to perceived transitions.
- **Playful Spring** -- spring easing, generous durations for emphasis moments, bounce on
  key interactions (drag-drop, completion). Personality-forward motion language. Trade-off:
  can feel slow for power users; spring curves are harder to implement.

Each option HTML must include:

1. Duration scale rendered as animated demonstrations (boxes moving at each tier)
2. Easing curve visualizations (plotted bezier curves + animated comparison)
3. At least 5 transition demonstrations using locked components:
   - Modal open/close
   - Dropdown or menu expand
   - Button press micro-interaction
   - Page or tab transition
   - Loading state transition (skeleton -> content)
4. Reduced motion comparison: standard vs reduced side-by-side for 3 key transitions
5. Motion token definitions (duration and easing CSS custom properties)

Use CSS animations and transitions within the option HTML. All demonstrations must be
interactive -- the user clicks a button to trigger the animation and sees it live.

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Adjust specific durations (modal too slow, button press too subtle)
- Change easing curve for a specific pattern (page transition needs more deceleration)
- Add or remove specific micro-interactions
- Adjust spring tension/damping if using spring curves
- Refine reduced motion behavior

When merging two options, create a new option file (e.g., `option-ab.html`) and state
what was taken from each in the rationale.

---

## 9. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite which
transitions, which durations, which curves.

### Functional Purpose

- Does every animation serve a purpose (feedback, spatial orientation, state change)?
- Can you remove any animation without losing information or usability?
- Is motion proportional to the action? (Micro for small actions, emphasis for big moments.)

### Timing Feel

- Do micro-interactions feel instant and responsive?
- Do standard transitions feel smooth, not sluggish?
- Does the enter/exit asymmetry (enter slower, exit faster) feel natural?
- Are any animations noticeably too fast (jarring) or too slow (waiting)?

### Easing Quality

- Do enter animations use ease-out (deceleration)?
- Do exit animations use ease-in (acceleration)?
- Do movement animations use ease-in-out (S-curve)?
- Does the overall easing personality match the brand tone from the brief?

### Reduced Motion Compliance

- Does the option include a complete reduced motion alternative?
- Does reduced motion preserve all functional state changes?
- Is the reduced motion experience still usable and comprehensible?
- Are vestibular triggers (parallax, zoom, rapid movement) eliminated in reduced motion?

### Performance

- Are animations GPU-accelerated (transform, opacity only)?
- Are any animations causing layout thrashing (animating width, height, top, left)?
- Is the total motion budget reasonable? (Sum of all concurrent animations should not
  overwhelm low-end devices.)
- Would the motion work on mobile hardware and low-refresh-rate displays?

### Consistency

- Do similar elements animate the same way? (All buttons press alike, all modals enter alike.)
- Is the duration scale applied consistently? (No ad-hoc timing values.)
- Does the easing set feel cohesive across all transitions?

---

## 10. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Omit the dark mode toggle** unless motion behavior changes between themes. Motion is
  a temporal facet, not a visual styling facet.
- Each option card's iframe must be interactive. The user needs to click buttons and
  trigger animations to evaluate them. Static screenshots cannot communicate motion.
- Include a "Play All" button in each option card that triggers a choreographed sequence
  of the 5 key transitions, so the user can see the full motion language in flow.
- Below each iframe, include a "Motion Tokens" expandable section listing all duration and
  easing tokens with their values.
- The comparative take ("My Take") must address: which option feels most natural, which
  strikes the right balance between personality and speed, and whether reduced motion is
  well-handled. State your recommendation.

---

## 11. Locking and Handoff

When the user locks a motion option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: motion personality (minimal/smooth/playful), easing
   approach (standard/spring), duration range (fastest to slowest token), and reduced motion
   strategy.
3. The locked option file becomes the motion reference for downstream facets
   (accessibility will audit animation safety, reduced motion completeness).
4. Motion tokens (duration and easing CSS custom properties) feed into `design-tokens.json`
   at the direction stage.

---

## 12. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| No animation (instant everything) | Performance-critical, accessibility-first, enterprise tools | Feels dead; state changes can be disorienting without transition |
| Micro-interactions only | Productivity tools, daily-use apps, fast workflows | Lacks spatial communication; modal appears from nowhere |
| Full transition system | Consumer apps, onboarding-heavy, brand-forward products | Adds perceived latency; power users may find it slow |
| Spring physics | Playful brands, draggable interfaces, mobile-native feel | Complex to implement; can feel unprofessional in enterprise |
| Scroll-driven storytelling | Marketing sites, product tours, landing pages | Terrible for repeat visitors who want the content, not the show |
| GPU-only animations | Performance-constrained, mobile-first | Limited to transform and opacity; no height/width animation |

---

## 13. Constraints

- Every option must include a reduced motion alternative. No exceptions.
- Duration tokens must be defined as CSS custom properties for token extraction.
- Never animate layout properties (width, height, margin, padding, top, left, right, bottom)
  for performance. Use transform (translate, scale) and opacity only.
- Enter duration must be >= exit duration for the same element. Enter: 1.0x, exit: 0.8x.
- No animation may flash content more than 3 times per second (WCAG 2.3.1).
- Looping animations must be pausable or must stop after a finite number of iterations.
- Spring curves that produce overshoot (bounce) must be used sparingly and never on
  elements containing text (text bounce reduces readability).
- All animation demonstrations in the option HTML must be triggered by user action (click),
  not auto-playing on load. The user controls when animations fire.
- Motion tokens must integrate with the locked duration vocabulary. If the density facet
  defined timing tokens, build on them rather than introducing a parallel system.
