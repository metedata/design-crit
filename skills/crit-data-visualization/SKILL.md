---
name: crit-data-visualization
description: Use when evaluating chart types, data display patterns, dashboard layouts, and how data is visually communicated to users.
---

# Data Visualization Crit

**Reference:** Read `../../reference/crit-loop.md` for compare view, feedback round-trip, convergence, and locking mechanics.

You are a designer critiquing how data is visually communicated in this product. Data
visualization is a contextual facet -- it applies when the product surfaces quantitative
information, trends, comparisons, or distributions. Your job: evaluate chart selection,
data encoding, dashboard composition, and annotation clarity, then guide the user to a
locked visualization direction.

---

## 1. Load Context

Before generating any options, read these files in order:

1. `.design-crit/state.json` -- current facet state, round number, all prior locked decisions
2. `.design-crit/brief.md` -- project brief (data types, audience expertise, platform)
3. `../../reference/crit-loop.md` -- shared crit loop mechanics
4. `../../reference/design-principles.md` -- option generation principles, convergence tone

### Prior Locked Decisions

Read every locked facet's winning option file. Data visualization builds on top of all
prior structure. Pay special attention to:

- **Layout** -- dashboard regions, content area widths, grid structure constrain chart sizing
- **Color system** -- palette, status colors, and contrast requirements apply to data encoding
- **Typography** -- axis labels, annotations, and legends inherit the locked type scale
- **Density/spacing** -- chart padding, dashboard gutter, label clearance follow the spacing scale
- **Content hierarchy** -- which data is primary vs supporting determines chart prominence

State in your critique: "Building on the locked [facet]: [summary]" for each.

### Detect Existing Feedback

Check for `feedback-round-{N}.json` in `.design-crit/facets/data-visualization/`. If found
and unprocessed, read it and continue the crit loop from that round. Do not regenerate round 1.

---

## 2. Chart Selection Framework

Choose chart types based on what the data needs to communicate, not visual preference. Map
each data display in the product to the right chart type using this framework.

### Data Relationship to Chart Type

| Relationship | Chart Types | When to Use |
|---|---|---|
| **Comparison** | Bar (vertical/horizontal), grouped bar, dot plot | Comparing discrete categories or items |
| **Trend over time** | Line, area, sparkline | Showing change across a continuous time axis |
| **Part of whole** | Stacked bar, donut, treemap, waffle | Showing proportions that sum to 100% |
| **Distribution** | Histogram, box plot, violin, density | Understanding spread and shape of a dataset |
| **Correlation** | Scatter, bubble, heatmap | Showing relationship between two or more variables |
| **Ranking** | Horizontal bar, lollipop, slope | Ordering items by a single metric |
| **Geospatial** | Choropleth, dot map, hex bin | Data tied to geographic locations |
| **Flow/process** | Sankey, funnel, waterfall | Showing movement between stages or categories |

### Anti-patterns to Flag

- **Pie chart for more than 5 slices** -- becomes unreadable. Use horizontal bar instead.
- **3D charts** -- distort data perception. Always use 2D.
- **Dual Y-axis** -- misleads by implying correlation. Use separate charts or normalize.
- **Truncated Y-axis** -- exaggerates differences. Start at zero for bar charts.
- **Rainbow color scales** -- perceptually non-uniform. Use sequential or diverging scales.

---

## 3. Data Encoding Channels

Each visual property encodes data differently. Evaluate whether each option uses the right
channel for the data type.

### Encoding Effectiveness (ordered by perceptual accuracy)

| Channel | Best For | Precision |
|---|---|---|
| **Position** (x/y axis) | Quantitative values, comparisons | Highest -- humans judge position most accurately |
| **Length** (bar height) | Quantitative comparison | High -- easy to compare adjacent lengths |
| **Angle/slope** (line chart) | Trend direction | Medium -- good for direction, weak for exact values |
| **Area** (bubble size) | Relative magnitude | Medium-low -- area perception is inaccurate; overestimates small differences |
| **Color saturation** | Sequential magnitude (heatmaps) | Low -- useful for pattern, not precise values |
| **Color hue** | Categorical distinction | Categorical only -- not for quantitative data |
| **Shape** (scatter markers) | Categorical grouping | Categorical only -- limited to 5-7 distinguishable shapes |

### Rules for Encoding

- Encode the most important variable on position (x/y axis).
- Never encode quantitative data on hue alone. Use saturation or lightness for magnitude.
- Limit categorical color encoding to 7 or fewer categories. Beyond that, use labels or interaction.
- When using area encoding (bubble charts), scale by radius, not area, and add value labels.
- Pair every color encoding with a text label or legend for accessibility.

---

## 4. Dashboard Composition

When the product includes a dashboard or multi-chart view, evaluate the overall composition.

### Dashboard Hierarchy

Apply the locked content hierarchy to the dashboard:

- **Hero metric or chart** -- the single most important data point. Largest, top-left or
  center position. Immediately answers "how are we doing?"
- **Supporting charts** -- 2-4 charts that provide context for the hero. Smaller, arranged
  below or beside the hero.
- **Detail tables or lists** -- granular data for users who need specifics. Below the charts,
  or accessible via drill-down.

### Layout Patterns for Dashboards

| Pattern | Structure | Best For |
|---|---|---|
| **KPI + Detail** | Row of summary cards at top, detailed charts below | Executive dashboards, monitoring |
| **Magazine** | Mixed-size chart tiles in an asymmetric grid | Analytics platforms, reporting |
| **Narrative** | Vertical scroll with charts interspersed with text | Data stories, onboarding reports |
| **Comparison grid** | Uniform chart tiles in a symmetric grid | Multi-metric monitoring, A/B results |

### Data-Ink Ratio

Evaluate each option's data-ink ratio -- the proportion of ink (pixels) devoted to data
vs non-data decoration. Maximize data-ink by removing:

- Unnecessary gridlines (keep only primary axis gridlines, muted)
- Decorative borders around charts
- Redundant legends (label data directly when possible)
- Heavy axis lines (lighten or remove when gridlines suffice)
- Background fills that do not encode data

State the data-ink assessment in the critique: "This option [has high/moderate/low]
data-ink ratio. [Specific elements] could be removed without losing information."

---

## 5. Annotation and Labeling

Annotations transform charts from pretty pictures into comprehensible communication.
Evaluate annotation strategy in every option.

### Annotation Types

| Type | Purpose | Example |
|---|---|---|
| **Direct labels** | Value on or near the data point | "42%" on a bar segment |
| **Axis labels** | Define what each axis represents | "Revenue ($M)" on Y-axis |
| **Reference lines** | Show targets, thresholds, averages | Dashed line at "Target: 80%" |
| **Callouts** | Highlight notable data points or anomalies | Arrow pointing to a spike: "Launch day" |
| **Legends** | Map visual encoding to categories | Color swatch + label for each series |
| **Tooltips** | Detail on hover/tap for interactive charts | Exact value, date, and category on hover |

### Annotation Hierarchy

Not every chart needs every annotation type. Match annotation density to the chart's role:

- **Hero charts:** Direct labels on key data points + reference lines for targets.
  The user should understand this chart without interacting.
- **Supporting charts:** Axis labels + legend. Tooltips for detail on demand.
- **Sparklines and KPI cards:** Value label only. No axes, no legend.

### Labeling Constraints

- Axis label font size must come from the locked type scale (Caption or Body Small).
- Label color must meet WCAG AA contrast against the chart background.
- Avoid rotated labels. If axis labels do not fit horizontally, use a horizontal bar chart
  instead, or abbreviate labels.
- Number formatting must be consistent: choose K/M/B abbreviation OR full numbers, not mixed.

---

## 6. Interactive Exploration Patterns

When the product supports interactive data exploration, evaluate the interaction model.

### Core Interaction Patterns

| Pattern | Behavior | Use When |
|---|---|---|
| **Hover/tooltip** | Show detail on mouse hover or long-press | Default for all interactive charts |
| **Click-to-filter** | Clicking a chart element filters other charts | Dashboard cross-filtering |
| **Brush selection** | Drag to select a range on an axis | Time series exploration |
| **Zoom/pan** | Zoom into dense data, pan across time | Large datasets, maps |
| **Drill-down** | Click a summary to see underlying detail | Hierarchical data (region > city > store) |
| **Toggle series** | Click legend items to show/hide data series | Multi-series comparisons |

### Interaction Constraints

- Every hover state must have a tap equivalent for touch devices.
- Filter state must be visible: show active filters as removable chips or highlighted elements.
- Drill-down must have a clear "back" or breadcrumb path.
- Zoom must include a "reset zoom" affordance.
- Reduced motion users: replace animated transitions with instant state changes.

---

## 7. Data Table Design

When the product includes data tables alongside or instead of charts, evaluate table design.

### Table Anatomy

| Element | Guidance |
|---|---|
| **Header row** | Sticky on scroll. Bold or medium weight from locked type scale. Sortable columns show sort indicator. |
| **Data cells** | Align numbers right. Align text left. Use monospace or tabular figures for numbers. |
| **Row density** | Follow locked spacing scale. Compact tables: 32-36px row height. Comfortable: 40-48px. |
| **Zebra striping** | Use locked surface color alternation (surface-base / surface-sunken). Subtle: 2-4% opacity shift. |
| **Pagination vs scroll** | Paginate for datasets > 50 rows. Virtualized scroll for real-time data. |
| **Inline visualization** | Sparklines, progress bars, or status indicators within cells. Keep them minimal -- the table IS the data. |

---

## 8. Generating Options

### Round 1: Diverge

Generate 2-4 visualization options. Each must make a different bet about chart selection,
encoding strategy, or dashboard composition. Maximize variety.

Example option set for an analytics dashboard product:

- **Data-Dense Overview** -- compact grid of charts, high data-ink ratio, direct labels,
  minimal decoration. Optimized for power users who scan many metrics quickly.
- **Narrative Flow** -- vertical scroll with hero metrics, contextual annotations,
  explanatory text between charts. Optimized for stakeholders who need the story, not raw data.
- **Interactive Explorer** -- fewer default charts, rich cross-filtering and drill-down,
  brushing on time series. Optimized for analysts who ask ad-hoc questions.

Each option HTML must include:

1. A representative dashboard or data view using the locked layout as the canvas
2. At least 3 chart types appropriate to the data described in the brief
3. Sample data rendered with realistic values (use plausible mock data, not lorem ipsum)
4. Annotation and labeling applied per the option's strategy
5. Locked colors applied: brand for accents, status colors for thresholds, text colors for labels
6. Data table example if the product includes tabular data

### Round 2+: Converge

Refine survivors based on feedback. Common refinements:

- Swap a chart type (e.g., replace pie with horizontal bar per feedback)
- Adjust annotation density (more or fewer labels)
- Change dashboard composition (reorder hero vs supporting)
- Refine color encoding to use locked palette more effectively
- Add or simplify interactive patterns

When merging two options, create a new option file (e.g., `option-ab.html`) and state
what was taken from each in the rationale.

---

## 9. Evaluation Criteria

Score every option against these questions in your critique. Be specific -- cite which
charts, which encodings, which annotations.

### Right Chart for the Data

- Does each chart type match the data relationship it displays (comparison, trend, distribution)?
- Are there any anti-pattern charts (pie with 10 slices, 3D, dual Y-axis)?
- Could any chart be replaced with a simpler type that communicates the same insight?

### Data-Ink Efficiency

- What is the data-ink ratio? Is every visual element encoding data or aiding comprehension?
- Are there decorative elements that could be removed without losing information?
- Are gridlines, borders, and backgrounds minimal and muted?

### Annotation Clarity

- Can the user understand each chart's key message without interacting?
- Are reference lines, targets, and thresholds clearly marked?
- Is number formatting consistent across all charts?

### Dashboard Story

- Does the dashboard have a clear hierarchy (hero -> supporting -> detail)?
- Can the user answer "how are we doing?" within 3 seconds of viewing?
- Is the reading order (left-to-right, top-to-bottom) aligned with information priority?

### Accessibility

- Do all color encodings have a text or pattern alternative?
- Do chart labels meet WCAG AA contrast requirements?
- Are interactive elements (tooltips, filters) accessible via keyboard and touch?
- Is there a data table alternative for screen reader users?

---

## 10. Compare View Specifics

When generating `compare.html` for this facet, follow all rules from `crit-loop.md` with
these additions:

- **Enable the dark mode toggle** if the locked color system has dark mode. Charts must
  be legible in both themes.
- Each option card's iframe must show the full dashboard or data view, not isolated charts.
  The user needs to see composition and hierarchy in context.
- Below each iframe, include a "Chart Inventory" expandable section listing every chart in
  the option with its type, data relationship, and encoding channels.
- The comparative take ("My Take") must address: which option tells the clearest data story,
  which has the best chart-type selections, and which balances density with clarity.
  State your recommendation.

---

## 11. Locking and Token Extraction

When the user locks a data visualization option:

1. Update `state.json` with `status: "locked"`, `locked_option`, `locked_summary`,
   `decided_by`, `decision_rationale`, `rounds_completed`.
2. The `locked_summary` must capture: chart types used, dashboard composition pattern,
   annotation strategy, and interaction model.
3. The locked option file becomes the data visualization reference for downstream facets
   (accessibility will audit chart accessibility, color contrast on data encodings).
4. Visualization patterns from the locked file inform implementation specifications at the
   direction stage.

---

## 12. Common Patterns and Trade-offs

Use these to inform option generation and critique. Apply them to the specific project.

| Pattern | When It Works | Watch Out For |
|---|---|---|
| KPI cards + detail charts | Executive dashboards, quick status checks | Cards can become vanity metrics without context |
| Direct labeling over legends | Small chart count, simple series | Cluttered when many data points overlap |
| Cross-filtering dashboard | Multi-metric exploration, analyst tools | Complex to implement; unclear filter state confuses users |
| Sparklines in tables | Dense data with many entities | Too small for precise reading; supplement with detail view |
| Narrative scrolling | Stakeholder reports, data stories | Poor for repeat-visit monitoring; users want the number, not the story |
| Dark background charts | Media, finance terminals, developer tools | Harder to achieve contrast on axis labels; printing issues |

---

## 13. Constraints

- Never use 3D effects on charts. Always render in 2D.
- Never use pie charts for more than 5 categories. Switch to horizontal bar.
- Never rely on color alone to distinguish data categories. Pair with labels, patterns, or shapes.
- All axis labels and annotations must use the locked type scale. No ad-hoc font sizes.
- All chart colors must come from the locked color palette. Define a sequential scale
  from the brand primary for quantitative encoding if not already in the palette.
- Mock data must be realistic and plausible for the product domain. No random numbers.
- Every interactive chart must have a static fallback (direct labels) for non-interactive
  contexts (screenshots, print, accessibility).
- Data tables must follow the locked density/spacing scale for row height and cell padding.
