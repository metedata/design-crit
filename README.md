# design-crit

A Claude Code plugin that runs a structured design critique for any app or service. It generates interactive HTML wireframes and presents them side-by-side for iterative review — like having a designer at your crit, presenting options and guiding you toward a decision.

```
/design-crit
```

The plugin walks you from a rough idea through a design brief, screen inventory, and 19 visual/structural design facets to a complete design direction — with HTML artifacts at every step.

## How it works

```
Brief  ──>  Facet Plan  ──>  Crit Loops  ──>  Design Direction
                               ↓
                         For each facet:
                         ┌─────────────────────────┐
                         │  Generate 2-4 options    │
                         │  as HTML wireframes      │
                         │          ↓               │
                         │  Present side-by-side    │
                         │  compare view            │
                         │          ↓               │
                         │  You: Keep / Cut /       │
                         │  Comment per option      │
                         │          ↓               │
                         │  Refine survivors,       │
                         │  repeat until locked     │
                         └─────────────────────────┘
```

**Stage 1 — Brief.** Claude scans your codebase and asks targeted questions to build a design brief: what are you building, for whom, on what platform, and what makes it different.

**Stage 2 — Facet Plan.** Based on the brief, Claude selects which design facets to evaluate (typography, navigation, color, layout, etc.) and orders them so structural decisions come before visual ones.

**Stage 3 — Crit Loops.** For each facet, Claude generates 2-4 HTML wireframe options with written rationale, presents them in a side-by-side compare view in your browser, and waits for your feedback. You mark options as Keep or Cut, leave comments, and Claude refines the survivors. Most facets resolve in 2 rounds. You can also say "Decide For Me" to delegate low-stakes facets.

**Stage 4 — Design Direction.** Once all facets are locked, Claude synthesizes everything into a polished `direction.html` document with design tokens, a decision log, and screen gallery linking to all your final wireframes.

## Installation

```bash
# Add the marketplace
claude plugin marketplace add metedata/design-crit

# Install the plugin
claude plugin install design-crit@metedata-design-tools
```

Or for local development:

```bash
git clone https://github.com/metedata/design-crit.git
claude --plugin-dir ./design-crit
```

## Usage

In any project directory:

```
/design-crit
```

The orchestrator reads `.design-crit/state.json` to figure out where you are in the pipeline and routes to the right stage. If you start a new session or hit a context limit, just run `/design-crit` again — everything is saved to disk.

### The compare view

Each crit round opens a compare view in your browser. For each option you see:

- An iframe preview of the wireframe
- **Why This Option** — the design reasoning
- **Works Well For** — where this approach shines
- **Watch Out For** — honest trade-offs
- **Keep / Cut** toggle buttons
- A comment box for your feedback

Below all options, Claude's **My Take** section gives an opinionated recommendation across the options.

### What gets generated

All artifacts live in `.design-crit/` at your project root:

```
.design-crit/
  state.json               # Pipeline state — where you are, what's locked
  brief.md                 # Your design brief
  crit-session.md          # Compressed decisions + user design intent
  overview.html            # Progress dashboard
  direction.html           # Final design direction document
  design-tokens.json       # Extracted tokens (colors, type scale, spacing)
  decisions.md             # Full decision log (markdown)
  facets/
    navigation-model/
      option-a.html        # Individual wireframe files
      option-b.html
      compare.html         # Side-by-side compare view
      critique.md          # Claude's rationale
      feedback-round-1.json
    color-system/
      ...
```

## Facets

19 pre-built facets organized by three design lenses, plus a generic fallback:

**What Exists** — defining the product surface
| Facet | What it decides |
|---|---|
| Screen Inventory | Which screens exist, their purpose and flow |
| Edge States | Empty, error, loading, and boundary conditions |
| Onboarding Flows | First-run experience and progressive disclosure |

**How It's Arranged** — structural and compositional decisions
| Facet | What it decides |
|---|---|
| Navigation | How users move between screens |
| Layout | Grid, spatial arrangement, content areas |
| Content Hierarchy | What's prominent within each screen |
| Component Design | Buttons, forms, cards, modals — interaction patterns |
| Elevation & Shape | Depth, layering, border radius, shadows |
| Data Visualization | Charts, graphs, and data display patterns |
| Responsive | Breakpoints, adaptive behavior, mobile layout |

**How It Feels** — visual and experiential decisions
| Facet | What it decides |
|---|---|
| Typography | Font families, type scale, hierarchy |
| Color System | Palette, semantic colors, contrast |
| Density & Spacing | Whitespace, touch targets, information density |
| Iconography | Icon style, system, and usage conventions |
| Motion | Transitions, animations, micro-interactions |
| Content Voice | Tone, labels, error messages, microcopy |
| Imagery & Illustration | Photo style, illustration approach, visual assets |
| Loading & Performance | Skeleton screens, progress indicators, perceived speed |
| Accessibility | WCAG compliance, keyboard nav, screen reader support |

Plus `crit-custom` for anything not covered above.

Not every project needs all 19. The facet planning stage selects 6-12 based on your brief.

## Design system

The plugin's own UI (compare views, overview page, direction document) uses a locked dark design system defined in `reference/crit-ui.md`. This ensures consistent, professional output across sessions. The crit chrome recedes so your wireframes are the focus.

## Session resilience

Everything lives on disk. If you hit a context limit or close your terminal:

1. Start a new Claude Code session
2. Run `/design-crit`
3. Claude reads `state.json` and picks up exactly where you left off

No conversation history needed. The state file, feedback JSONs, and HTML artifacts are the source of truth.

## License

MIT
