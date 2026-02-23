# design-crit

A Claude Code plugin that runs a structured design critique process. It walks you through defining a design brief, inventorying screens, evaluating visual design facets (typography, color, layout, accessibility, and more), and synthesizing everything into a cohesive design direction -- generating side-by-side HTML wireframe artifacts at each step for iterative review.

## Installation

```bash
claude plugin add github:metepolat/design-crit
```

## Quick Start

```
/design-crit
```

Follow the prompts. The orchestrator will guide you through each stage.

## Skills

### Pipeline

| Skill | Purpose |
|---|---|
| `design-crit` | Orchestrator -- routes through the full pipeline |
| `design-brief` | Stage 1: Capture product context, goals, and constraints |
| `facet-planning` | Stage 2: Select which design facets to evaluate |
| `design-direction` | Stage 4: Synthesize facet results into a unified direction |

### Facet Library (Stage 3)

19 pre-built facets plus a generic fallback:

`crit-screen-inventory` `crit-edge-states` `crit-onboarding-flows` `crit-navigation` `crit-layout` `crit-content-hierarchy` `crit-component-design` `crit-elevation-shape` `crit-data-visualization` `crit-responsive` `crit-typography` `crit-color-system` `crit-density-spacing` `crit-iconography` `crit-motion` `crit-content-voice` `crit-imagery-illustration` `crit-loading-performance` `crit-accessibility` `crit-custom`

## Documentation

See [docs/plans/](docs/plans/) for the full design document and implementation plan.
