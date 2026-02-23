---
name: crit-accessibility
description: Use when auditing a design for accessibility — keyboard navigation, screen reader support, touch targets, focus management, color contrast, and cognitive load. Typically runs as the last facet to audit accumulated decisions.
---

# Accessibility Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer running the accessibility audit for this project. Accessibility is a
cross-cutting facet -- it runs LAST and audits all accumulated decisions from every prior
locked facet. Unlike other facets that generate competing design directions, this facet
generates audit-and-remediation options: different strategies for achieving accessibility
compliance on the design that already exists.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, ALL prior locked decisions
2. `.design-crit/brief.md` -- project brief (accessibility requirements, platform, audience)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions -- ALL OF THEM

This facet depends on every other locked facet. Read EVERY locked facet's winning option
file. You are auditing the entire accumulated design, not a single layer.

Build a mental model of the complete design from these locked decisions:

- **Screen inventory** -- which screens and flows exist, state maps
- **Edge states** -- empty, error, loading states and their UI treatment
- **Navigation** -- nav structure, menu patterns, wayfinding approach
- **Layout** -- grid structure, content regions, spatial arrangement
- **Content hierarchy** -- information priority, reading order
- **Component design** -- buttons, forms, cards, modals, tables
- **Elevation/shape** -- shadows, surface layering, border radius
- **Typography** -- font sizes, line heights, reading comfort
- **Color system** -- palettes, contrast ratios, dark mode
- **Density/spacing** -- spacing scale, touch targets, whitespace
- **Content voice** -- microcopy, error messages, terminology

State in your critique: "Auditing the locked design: [2-3 sentence summary of the
accumulated design direction]."

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/accessibility/`. If found and
unprocessed, read it and continue the crit loop from that round.

---

## 2. Audit Categories

The accessibility audit covers six categories. For each, run a systematic check against the
locked design and report findings.

### Category 1: Keyboard Navigation

Audit the locked navigation, layout, and component design for keyboard usability.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Tab order | Does logical reading order match DOM order in the locked layout? | WCAG 2.4.3 |
| Focus indicators | Are focus rings visible against all locked surface colors? | WCAG 2.4.7 |
| Skip links | Can users skip repetitive navigation to reach main content? | WCAG 2.4.1 |
| Keyboard traps | Can any locked component (modal, dropdown, menu) trap focus? | WCAG 2.1.2 |
| Keyboard shortcuts | Are critical actions reachable without a mouse? | Best practice |
| Focus management | Do modals, dialogs, and drawers manage focus correctly? | WCAG 2.4.3 |
| Interactive elements | Can every button, link, and form field be activated via keyboard? | WCAG 2.1.1 |

**Findings format:** For each issue, state the specific element (from the locked wireframe),
the problem, and a remediation recommendation.

### Category 2: Screen Reader Support

Audit the locked layout, components, and content voice for screen reader compatibility.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Semantic structure | Do headings follow a logical hierarchy (h1 > h2 > h3)? | WCAG 1.3.1 |
| ARIA landmarks | Are main regions identified (nav, main, aside, footer)? | WCAG 1.3.1 |
| ARIA labels | Do interactive elements without visible text have aria-label? | WCAG 4.1.2 |
| Live regions | Do dynamic updates (toasts, status changes) announce themselves? | WCAG 4.1.3 |
| Alt text | Do images, icons, and illustrations have meaningful alt text? | WCAG 1.1.1 |
| Form labels | Is every form input associated with a visible label? | WCAG 1.3.1 |
| Table headers | Do data tables have proper th elements and scope attributes? | WCAG 1.3.1 |
| Reading order | Does the DOM order match the visual reading order? | WCAG 1.3.2 |

**Findings format:** For each issue, state the element, the screen reader experience (what
a user would hear or miss), and the remediation.

### Category 3: Touch Targets

Audit the locked density/spacing and component design for touch accessibility.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Minimum target size | Are all interactive elements at least 44x44px (iOS) or 48x48dp (Android)? | WCAG 2.5.8 / Platform HIG |
| Target spacing | Is there at least 8px between adjacent touch targets? | Best practice |
| Hover-only patterns | Are there interactions only accessible via mouse hover? | WCAG 2.1.1 |
| Gesture alternatives | Do swipe/pinch/drag interactions have button alternatives? | WCAG 2.5.1 |

Reference the locked density facet's touch target annotations. If the density facet already
flagged issues, confirm they are addressed or flag them as unresolved.

### Category 4: Color and Contrast

Audit the locked color system for contrast compliance and color independence.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Text contrast | All text/background pairs meet 4.5:1 (normal) or 3:1 (large) | WCAG 1.4.3 |
| UI contrast | Buttons, inputs, icons meet 3:1 against adjacent colors | WCAG 1.4.11 |
| Focus indicator contrast | Focus rings meet 3:1 against both the element and background | WCAG 2.4.11 |
| Color independence | Is information conveyed by color alone anywhere? | WCAG 1.4.1 |
| Color blindness | Do status colors (red/green) have non-color indicators? | Best practice |
| Dark mode contrast | Do all pairs pass in dark mode too? | WCAG 1.4.3 |

Reference the locked color system's contrast audit. If the color facet already ran contrast
checks, verify their findings and flag any that were missed or borderline.

**Color-blindness simulation:** Describe how the locked palette looks under protanopia
(red-blind), deuteranopia (green-blind), and tritanopia (blue-blind). Note any status
indicators that become ambiguous.

### Category 5: Motion and Animation

Audit any motion or transition patterns in locked decisions.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Reduced motion | Is there a mechanism to reduce or disable animations? | WCAG 2.3.3 |
| Vestibular safety | Are there parallax, zoom, or spinning animations? | WCAG 2.3.3 |
| Auto-playing content | Does any content auto-play, auto-scroll, or auto-advance? | WCAG 2.2.2 |
| Flashing content | Does anything flash more than 3 times per second? | WCAG 2.3.1 |

If the motion facet was included and locked, reference its decisions. If motion was not
a facet, audit any implicit motion in locked components (dropdown animations, page
transitions, loading spinners).

### Category 6: Cognitive Accessibility

Audit the locked content voice, layout, and navigation for cognitive load.

**Check these items:**

| Item | What to Check | Standard |
|---|---|---|
| Reading level | Is microcopy written at an appropriate reading level for the audience? | WCAG 3.1.5 |
| Consistent navigation | Are navigation patterns consistent across all screens? | WCAG 3.2.3 |
| Consistent identification | Are UI elements labeled consistently (same term everywhere)? | WCAG 3.2.4 |
| Error prevention | Do destructive actions have confirmation steps? | WCAG 3.3.4 |
| Clear wayfinding | Can users always tell where they are and how to get back? | WCAG 2.4.8 |
| Information chunking | Is content grouped into scannable chunks, not walls of text? | Best practice |
| Predictable behavior | Do UI elements behave as expected from their appearance? | WCAG 3.2.1 |

Reference the locked content voice's terminology glossary. Flag any term inconsistencies
found across locked wireframes.

---

## 3. Audit Report Format

Generate the audit as an HTML document at `.design-crit/facets/accessibility/audit.html`.
This report is the foundation for the options in this facet.

### Report Structure

```
ACCESSIBILITY AUDIT: [Project Name]
Target: WCAG [level from brief, default AA]
Audited against: [N] locked facet decisions

SUMMARY
- [X] issues found: [critical count] critical, [major count] major, [minor count] minor
- Keyboard navigation: [pass/issues]
- Screen reader support: [pass/issues]
- Touch targets: [pass/issues]
- Color contrast: [pass/issues]
- Motion: [pass/issues]
- Cognitive: [pass/issues]

CRITICAL ISSUES (blocks deployment)
1. [issue title] — [category] — [WCAG criterion]
   Element: [specific element from locked wireframe]
   Problem: [what fails]
   Impact: [who is affected and how]
   Remediation: [specific fix]

MAJOR ISSUES (should fix before launch)
...

MINOR ISSUES (improve over time)
...
```

### Severity Classification

| Severity | Definition | Examples |
|---|---|---|
| **Critical** | Prevents a user from completing a core task | No keyboard access to primary action, form without labels |
| **Major** | Degrades experience significantly for some users | Missing skip link, low contrast on body text, no focus indicators |
| **Minor** | Suboptimal but usable | Slightly below contrast ratio on secondary text, non-ideal reading order |

---

## 4. Generating Options

Unlike other facets where options represent competing design directions, accessibility
options represent different remediation strategies. The audit findings are the same; the
options differ in HOW to fix them.

### Round 1: Audit + Remediation Options

Generate the audit report (section 3). Then present 2-3 remediation options.

Example option set:

- **Minimum Compliance** -- fixes all critical and major issues to achieve WCAG AA. Minimal
  visual disruption to locked design. Pragmatic approach that ships accessible.
  Trade-off: minor issues deferred; does not reach AAA.
- **Enhanced Access** -- fixes all issues (critical, major, minor) and adds quality-of-life
  improvements: skip links, landmark regions, keyboard shortcut hints. More invasive
  changes to locked wireframes. Trade-off: more visual changes; some locked design
  details may shift.
- **Inclusive by Default** -- fixes all issues AND proactively adds accessibility features
  not flagged in the audit: reduced motion mode, high contrast mode, screen reader
  optimized flows. Exceeds WCAG AA in places. Trade-off: significant implementation
  effort; some additions may feel over-engineered for MVP.

Each option HTML must include:
1. The full audit report at the top (shared across options)
2. Annotated wireframes showing the locked design WITH remediation applied. Use the locked
   layout/color/typography/density wireframes as the canvas and layer remediation visually:
   - Added focus indicators (colored rings around focusable elements)
   - Corrected contrast (show before/after for changed color pairs)
   - Touch target adjustments (show expanded targets)
   - Added ARIA landmark annotations
   - Skip link placement
   - Any structural changes to locked wireframes
3. A remediation checklist: every audit finding with a checkbox, grouped by severity,
   showing which are addressed in this option
4. Implementation notes: specific HTML attributes, ARIA roles, and CSS changes needed

### Round 2+: Converge

Refine based on feedback. Common refinements:
- Include specific fixes from one option in another
- Adjust the visual impact of a remediation (e.g., subtler focus indicators)
- Prioritize specific audit findings the user cares about
- Address user concerns about visual disruption to locked designs

---

## 5. Evaluation Criteria

Score every option against these questions in your critique.

### WCAG Compliance
- Does this option achieve the target WCAG level from the brief (default: AA)?
- Are all critical issues resolved?
- Are major issues resolved or explicitly deferred with justification?
- Which specific WCAG criteria pass after remediation?

### Keyboard-Only Usability
- Can a user complete every core flow using only a keyboard?
- Are focus indicators visible against all surface colors?
- Is tab order logical and predictable?
- Can users escape every overlay, modal, and menu via keyboard?

### Screen Reader Navigability
- Can a screen reader user understand the page structure?
- Are all interactive elements announced with correct roles and labels?
- Do dynamic updates (toasts, live data) announce themselves?
- Is the reading order logical?

### Visual Impact
- How much does this remediation change the locked visual design?
- Are the changes additive (layering accessibility on top) or subtractive (modifying
  locked decisions)?
- Would the user who locked the prior facets recognize the design after remediation?

### Implementation Effort
- How many HTML/CSS/JS changes does this remediation require?
- Are changes localized to specific components, or do they affect global structure?
- Can the changes be implemented incrementally?

---

## 6. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** if the locked color system has dark mode. Accessibility
  must be audited in both themes.
- Each option card's iframe must show the remediated wireframe, not just the audit report.
  The user needs to SEE what the accessible version looks like.
- Include a summary badge on each option card: "[X] of [Y] issues addressed" and the
  resulting WCAG compliance level.
- Below each iframe, include an expandable "Audit Details" section with the full finding
  list and which are addressed in this option.
- The comparative take ("My Take") must address: which option achieves the best
  compliance-to-effort ratio, which is most respectful of locked visual decisions, and
  whether the enhanced features in more aggressive options are worth the cost.
  State your recommendation.

---

## 7. Locking and Handoff

When the user locks an accessibility option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: WCAG compliance level achieved, number of issues
   addressed (critical/major/minor), and any significant additions (skip links, keyboard
   shortcuts, high contrast mode).
3. The locked option file becomes the accessibility layer for the final design. It overlays
   all prior locked decisions.
4. The audit report (`audit.html`) and remediation checklist feed into the design direction
   deliverables. The direction stage should reference accessibility status.

### Updating Prior Locked Wireframes

If the locked accessibility option requires changes to prior locked wireframes (e.g.,
adjusted contrast in the color system, larger touch targets in the density system), note
these in the `decision_rationale` but do NOT modify prior locked option files. The
accessibility option file IS the canonical version showing the accumulated design with
accessibility applied. The direction stage will use this as the most complete representation.

---

## 8. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| Compliance-first | MVP, tight timeline, clear WCAG target | Can feel checkbox-driven; misses spirit of accessibility |
| Progressive enhancement | Layered features over solid foundation | Users with disabilities wait for later iterations |
| Accessibility-first rebuild | When audit reveals deep structural issues | May conflict with locked design decisions |
| Automated testing + manual audit | Large surface area, many screens | Automated tools catch ~30% of issues; manual audit is essential |
| Design system integration | When building reusable components | Accessibility built into components benefits all future screens |

---

## 9. Relationship to All Prior Facets

This facet is unique because it depends on EVERY prior facet. Here is how each locked
decision feeds the audit:

| Locked Facet | What It Provides to Accessibility Audit |
|---|---|
| Screen inventory | The complete list of screens and flows to audit |
| Edge states | Empty, error, loading states that need accessible treatment |
| Navigation | Tab order, skip links, keyboard nav structure |
| Layout | Reading order, landmark regions, content structure |
| Content hierarchy | Heading hierarchy, information priority |
| Component design | Form inputs, buttons, modals, tables to audit |
| Elevation/shape | Focus indicator visibility, surface contrast |
| Typography | Text size, line height, reading comfort |
| Color system | Contrast ratios, color independence, dark mode |
| Density/spacing | Touch targets, spacing between interactive elements |
| Content voice | Error message clarity, reading level, terminology |

If any facet was skipped, note it: "The [facet] was skipped. Accessibility audit for that
dimension is limited to general best practices."

---

## 10. WCAG Level Guidance

### WCAG AA (Default Target)

This is the standard target unless the brief specifies otherwise. It covers:
- Contrast ratios: 4.5:1 for normal text, 3:1 for large text and UI components
- Keyboard accessibility: all functionality available from keyboard
- Screen reader: meaningful sequence, labels, error identification
- Timing: adjustable time limits, pausable auto-play
- Navigation: multiple ways to find pages, focus visible, consistent navigation

### WCAG AAA (When Specified)

Additional requirements include:
- Contrast ratios: 7:1 for normal text, 4.5:1 for large text
- Sign language for prerecorded audio
- Extended audio description
- No timing limits
- Section headings to organize content
- Target size: 44x44px minimum

Note AAA requirements in the critique only when the brief specifies AAA compliance or the
user requests it. Do not impose AAA when AA is the target.

---

## 11. Constraints

- This facet runs LAST. Do not generate options until all prior enabled facets are locked
  or skipped.
- The audit must cover ALL six categories. Do not skip categories because "the locked
  design looks fine." Run every check systematically.
- Every audit finding must cite a specific WCAG criterion or platform guideline.
- Remediation options must show VISUAL changes on the locked wireframes. An audit report
  without visual remediation is incomplete.
- Do not modify prior locked option files. The accessibility option file layers on top of
  the accumulated design.
- Every option must achieve at minimum the WCAG level specified in the brief (default AA).
  Options differ in how far beyond the minimum they go.
- Include the full audit report in every option HTML. The audit itself is not a choice --
  only the remediation strategy is.
