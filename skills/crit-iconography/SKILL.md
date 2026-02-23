---
name: crit-iconography
description: Use when evaluating icon systems, visual metaphors, icon style consistency, and symbolic language in a design.
---

# Iconography Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing the icon system for this product. Iconography is a contextual
facet -- it applies when the product uses icons for navigation, actions, status indication,
or content categorization. Your job: evaluate icon style, metaphor clarity, visual
consistency, and sizing, then guide the user to a locked iconography direction.

Icons are the product's symbolic language. A well-designed icon set speeds recognition,
reduces reliance on text labels, and reinforces brand personality. A poorly designed set
creates confusion, inconsistency, and cognitive overhead.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (brand tone, platform, audience)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Iconography interacts with many prior
decisions. Pay special attention to:

- **Navigation** -- nav items that need icons, menu patterns with icon requirements
- **Component design** -- buttons with icons, icon-only actions, input field icons
- **Elevation/shape** -- border radius and shape language that icons should harmonize with
- **Color system** -- icon colors must come from the locked palette; status icons use status colors
- **Typography** -- icon optical size must harmonize with adjacent text size and weight
- **Density/spacing** -- icon padding, clearance, and alignment within the spacing scale

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/iconography/`. If found and
unprocessed, read it and continue the crit loop from that round. Do not regenerate round 1.

---

## 2. Icon Style Taxonomy

Every option must commit to a single icon style. Mixing styles within a product breaks
visual coherence. Evaluate the style against the brief's brand tone.

### Style Options

| Style | Characteristics | Personality | Best For |
|---|---|---|---|
| **Outlined** | 1-2px stroke, no fill, open shapes | Clean, modern, lightweight | SaaS products, productivity tools, minimalist brands |
| **Filled** | Solid shapes, no visible stroke | Bold, confident, high contrast | Mobile apps, high-density UIs, strong brand presence |
| **Duotone** | Two tonal layers (primary shape + accent detail) | Expressive, distinctive, branded | Consumer apps, creative tools, marketing-forward products |
| **Thin line** | Hairline strokes (1px or less), delicate | Elegant, premium, refined | Luxury brands, editorial, fashion/lifestyle apps |
| **Rounded** | Soft terminals, rounded corners on strokes | Friendly, approachable, warm | Consumer products, onboarding, family-oriented apps |
| **Sharp** | Squared terminals, geometric precision | Technical, professional, authoritative | Developer tools, enterprise, finance |

### Style-Brand Alignment

Match the icon style to the brief's brand tone and the locked shape language:

- Rounded shape language (border-radius 8px+) pairs with outlined or rounded icons.
- Sharp shape language (border-radius 0-4px) pairs with sharp or filled icons.
- Playful or consumer brands benefit from duotone or rounded styles.
- Technical or enterprise brands benefit from outlined or sharp styles.

State the alignment reasoning in the critique.

---

## 3. Icon Grid and Keyline System

Every option must define a grid system that governs icon construction. The grid ensures
visual consistency across the entire icon set.

### Grid Size

Define a base grid size. Common sizes:

- **24x24px** -- standard for most UI icons
- **20x20px** -- compact UIs, dense navigation
- **16x16px** -- inline icons within text, small indicators
- **32x32px** -- large display icons, onboarding illustrations

The grid includes a **safe zone** (padding inside the grid boundary). Typically 2px on each
side for a 24px grid, leaving a 20px live area.

### Keyline Shapes

Keyline shapes ensure icons have consistent optical weight regardless of their geometric
form. Define these keyline templates:

| Shape | Dimensions (in 24px grid) | Use For |
|---|---|---|
| **Circle** | 20px diameter, centered | Circular metaphors (globe, clock, user avatar) |
| **Square** | 18x18px, centered | Rectangular metaphors (document, folder, calendar) |
| **Vertical rectangle** | 16x20px, centered | Tall metaphors (bookmark, flag, person) |
| **Horizontal rectangle** | 20x16px, centered | Wide metaphors (landscape, card, video) |

Icons that fill different keyline shapes appear optically the same size because circular and
wide shapes are perceptually larger than tall or narrow ones.

### Stroke Specifications (for outlined/line styles)

| Property | Value | Rationale |
|---|---|---|
| Stroke width | 1.5px or 2px | 1.5px for refined feel; 2px for bolder, more visible |
| Cap style | Round or square | Round for friendly; square for sharp |
| Join style | Round or miter | Match cap style |
| Corner radius | 0px, 1px, or 2px | Match product's shape language |

Show the grid, keylines, and stroke specs in each option HTML as a construction reference.

---

## 4. Metaphor Clarity

Evaluate whether each icon's visual metaphor is recognizable without a text label.

### Recognition Tiers

| Tier | Definition | Examples |
|---|---|---|
| **Universal** | Recognized across cultures and contexts | Home (house), Search (magnifying glass), Close (X), Menu (hamburger) |
| **Contextual** | Recognized within a domain or platform convention | Dashboard (grid), Analytics (chart), Pipeline (funnel) |
| **Learned** | Requires first-time exposure with a label; remembered after | Custom product features, domain-specific actions |
| **Ambiguous** | Multiple interpretations; label required | Star (favorite? rating? important?), Heart (like? love? health?) |

### Metaphor Evaluation Rules

- **Always pair ambiguous icons with a text label.** An icon-only button for an ambiguous
  metaphor is a usability failure.
- **Test each icon:** cover the label and ask "what does this do?" If the answer is unclear,
  the icon needs a label or a better metaphor.
- **Prefer universal metaphors** for primary navigation and critical actions. Save contextual
  and learned metaphors for secondary features.
- **Avoid novel metaphors for destructive actions.** Delete must be a trash can. Logout must
  be a door or power icon. Do not get creative with high-stakes actions.

### Icon Inventory

For each option, provide an inventory of all icons the product needs (derived from the
locked navigation, components, and screens). For each icon, list:

- **Context** -- where it appears (nav, toolbar, button, status indicator)
- **Metaphor** -- the visual symbol used
- **Recognition tier** -- universal, contextual, learned, or ambiguous
- **Label requirement** -- icon-only, icon+label, or label-only fallback

---

## 5. Optical Sizing

Icons must work at multiple sizes while maintaining visual clarity. Evaluate optical sizing
in each option.

### Size Scale

Define the icon sizes the product uses and their contexts:

| Size | Pixel | Context |
|---|---|---|
| **XS** | 12-14px | Inline indicators, status dots, badges |
| **SM** | 16px | Inline with body text, compact UI elements |
| **MD** | 20-24px | Standard UI icons, nav items, toolbar buttons |
| **LG** | 28-32px | Feature icons, empty state illustrations, onboarding |
| **XL** | 40-48px | Hero icons, landing page features, category headers |

### Optical Size Adjustments

Icons drawn for 24px do not simply scale to 16px or 48px. At different sizes, icons need
optical corrections:

- **Small sizes (12-16px):** Simplify detail. Remove inner strokes. Increase stroke width
  proportionally. Fill may work better than outline at very small sizes.
- **Medium sizes (20-24px):** The canonical design. Full detail, standard stroke weight.
- **Large sizes (32-48px):** Add detail that would be invisible at smaller sizes. Reduce
  stroke width proportionally to avoid heaviness. Consider adding secondary color or duotone
  treatment.

### Testing Optical Size

In each option HTML, show at least 5 representative icons at XS, SM, MD, LG, and XL sizes.
Evaluate:

- Are small icons still recognizable?
- Are large icons detailed enough or do they look like blown-up small icons?
- Does the stroke weight feel consistent across sizes?

---

## 6. Consistency Across the Set

Evaluate whether the icon set feels like a cohesive family, not a collection of individuals.

### Consistency Checklist

| Property | What to Check |
|---|---|
| **Stroke weight** | Same stroke width across all icons in the set |
| **Corner radius** | Same corner rounding on all icons |
| **Level of detail** | Similar complexity. No icon vastly more detailed than its siblings. |
| **Perspective** | All icons use the same perspective (front-facing, slight angle, flat) |
| **Fill pattern** | If some icons are filled and others outlined, is the rule clear? (e.g., filled = active, outlined = inactive) |
| **Padding** | Consistent use of the keyline grid. Icons sit at the same optical weight. |
| **Metaphor language** | Related concepts use related visual language (settings gear and advanced settings wrench share a mechanical metaphor family) |

### Filled vs Outlined State Convention

Many icon systems use fill state to indicate selection or active state:

- **Outlined = default / inactive**
- **Filled = selected / active**

If the option uses this convention, apply it consistently. Show the active/inactive pair
for navigation icons in the option HTML.

---

## 7. Custom vs Library Icons

Evaluate whether the option uses a standard icon library, custom icons, or a hybrid.

### Trade-offs

| Approach | Strengths | Weaknesses |
|---|---|---|
| **Standard library** (Material, Lucide, Heroicons, Phosphor) | Large set, consistent, well-tested, accessible | Generic; many products look the same |
| **Custom icons** | Unique, brand-aligned, precisely tailored to product concepts | Expensive to create, maintain, and extend; risk of inconsistency |
| **Hybrid** (library base + custom for key actions) | Best of both: consistency from library, personality from custom | Must harmonize custom icons with library style; two sources to manage |

### Library Selection Criteria

If the option uses a library, evaluate:

- Does the library's default style match the product's brand tone?
- Does the library offer all the metaphors the product needs?
- Does the library provide multiple sizes or optical size variants?
- Does the library support the fill/outline state convention if needed?
- Is the library actively maintained and does it have an accessible SVG format?

### Harmonizing Custom with Library

If the option uses a hybrid approach, custom icons must match:

- Stroke width of the library
- Corner radius of the library
- Grid and keyline proportions of the library
- Cap and join style of the library

Show a side-by-side in the option HTML: library icon next to custom icon of similar
complexity. Evaluate whether they feel like siblings.

---

## 8. Generating Options

### Round 1: Diverge

Generate 2-4 iconography options. Each must make a different bet about icon style, library
choice, or metaphor strategy. Maximize variety.

Example option set for a productivity SaaS:

- **Lucide Clean** -- outlined style from Lucide library, 1.5px stroke, rounded caps.
  Universal metaphors for nav, contextual for features. Icon+label for all primary actions.
  Light, modern, pairs with rounded shape language.
- **Phosphor Bold** -- filled style from Phosphor library, solid shapes. Strong visual
  presence, high contrast even at small sizes. Fill/outline convention for active states.
  Bold, confident, pairs with compact density.
- **Custom Duo** -- duotone custom icons with brand primary and a muted accent. Unique
  metaphors for key product concepts. Library base (Heroicons) for standard actions, custom
  for product-specific features. Distinctive, branded, requires design investment.

Each option HTML must include:

1. Style specimen: 8-12 representative icons at the canonical size (24px), showing the
   full range of metaphors needed (navigation, actions, status, content types)
2. Keyline grid reference: the grid, safe zone, and keyline shapes
3. Size scale: the same 5 icons at XS, SM, MD, LG, XL sizes
4. Icon-in-context mockup: icons placed in the locked navigation, toolbar, and at least
   one component from the locked design. Show how icons sit alongside text labels.
5. Active/inactive states: if the option uses fill/outline convention, show navigation
   icons in both states
6. Icon inventory: the full list of icons needed with metaphor, recognition tier, and
   label requirement

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Swap specific icon metaphors that were unclear
- Adjust stroke weight (too thin, too bold)
- Change library or add custom icons for specific concepts
- Refine optical sizing at small or large sizes
- Harmonize icons with locked shape language more closely

When merging two options, create a new option file (e.g., `option-ab.html`) and state
what was taken from each in the rationale.

---

## 9. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite which
icons, which sizes, which contexts.

### Metaphor Clarity

- Can each icon be identified without its label? Test the top 10 most-used icons.
- Are ambiguous icons always paired with labels?
- Are destructive action icons (delete, remove, logout) universally recognizable?
- Do domain-specific icons map to intuitive metaphors for the target audience?

### Style Consistency

- Is stroke weight uniform across all icons?
- Do all icons follow the same keyline grid and safe zone?
- Is the level of detail consistent (no icon vastly more or less complex than others)?
- Does the icon style harmonize with the locked shape language and brand tone?

### Size Resilience

- Are icons recognizable at the smallest size they will be used (typically 16px)?
- Do large icons (32-48px) have appropriate detail, not just upscaled small icons?
- Is stroke weight optically adjusted across sizes?

### Integration with UI

- Do icons sit cleanly alongside text at the locked type scale sizes?
- Is icon vertical alignment correct with adjacent text baselines?
- Do icons in the locked navigation feel natural at the nav item's size and spacing?
- Do status icons use the locked status colors correctly?

### Accessibility

- Does every icon-only button have an accessible label (aria-label or visually hidden text)?
- Do icons meet 3:1 contrast ratio against their background (WCAG AA for non-text elements)?
- Are touch targets for icon-only buttons at least 44px on touch devices?

---

## 10. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** if the locked color system has dark mode. Icons must be
  visible and clear in both themes.
- Each option card's iframe must show icons in context (navigation, components), not just
  an isolated icon grid. The user needs to see how icons feel inside the locked UI.
- Below each iframe, include an "Icon Set Reference" expandable section showing the full
  icon inventory with each icon at its canonical size, labeled with its metaphor and context.
- The comparative take ("My Take") must address: which option has the clearest metaphors,
  which style best matches the brand, and whether the approach (library vs custom vs hybrid)
  is practical for the project's scope. State your recommendation.

---

## 11. Locking and Handoff

When the user locks an iconography option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: icon style (outlined/filled/duotone), library or
   custom, stroke weight, grid size, and the fill/outline state convention if used.
3. The locked option file becomes the iconography reference for downstream facets
   (accessibility will audit icon contrast, labels, and touch targets).
4. The icon inventory and style specifications feed into the design direction deliverables
   at the direction stage.

---

## 12. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| Outlined icons for everything | Clean aesthetic, modern SaaS | Low contrast at small sizes; disappears on busy backgrounds |
| Filled for active, outlined for inactive | Clear state indication in nav/tabs | Requires every icon in two variants; doubles asset count |
| Duotone for personality | Brand differentiation, consumer products | Complex to produce; harder to maintain consistency |
| Single icon library | Fast, consistent, cheap | Generic look; may lack product-specific metaphors |
| Hybrid library + custom | Brand personality where it counts, library for the rest | Style mismatch between library and custom icons |
| Icon-only buttons | Space-efficient, clean UI | Accessibility risk; ambiguous metaphors require labels |

---

## 13. Constraints

- Never mix icon styles within the same product (e.g., outlined nav icons with filled
  action icons) unless the mix follows a documented state convention (active/inactive).
- Every icon-only button must have an accessible text alternative. No exceptions.
- Icon colors must come from the locked color palette. Use `text-primary` for default
  icons, `text-secondary` for muted icons, status colors for status indicators.
- Icons must sit on the defined grid with proper keyline alignment. No freehand sizing.
- At minimum, show icons at 3 sizes in the option: small (16px), standard (24px), and
  large (32px+). Demonstrate optical sizing, not just scaling.
- If the locked navigation uses icons, the iconography option must include all nav icons
  at the nav item's size and spacing from the locked nav wireframe.
- Do not introduce icon metaphors that contradict platform conventions (e.g., a floppy disk
  for "save" on mobile where the convention is a checkmark).
