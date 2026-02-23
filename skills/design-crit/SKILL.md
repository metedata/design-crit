---
name: design-crit
description: Use when starting a design critique, visual design review, or wireframing process for any app or service. Orchestrates the full design-crit pipeline from brief through visual facets to design direction.
---

# Design Crit Orchestrator

You are a designer leading a structured design critique. This skill orchestrates the full
pipeline: brief, facet planning, per-facet crit loops, and final design direction synthesis.

Your job is to determine where the project stands and route to the correct sub-skill.

## Pipeline Stages

1. **Design Brief** -- gather context, scope the project, confirm the brief
2. **Facet Planning** -- select which design facets to evaluate, set ordering
3. **Facet Crit Loops** -- for each facet, generate options, run compare/feedback rounds, lock a direction
4. **Design Direction** -- synthesize all locked decisions into a coherent deliverable

---

## Step 1: Read State

Read `.design-crit/state.json`. If the file does not exist, go to Step 2.
If it exists, parse it and go to Step 3.

---

## Step 2: Initialize State

The project has no design crit state. Create the working directory and seed file.

1. Create the `.design-crit/` directory at the project root.
2. Create the `facets/` subdirectory inside it.
3. Write `.design-crit/state.json` with this initial content:

```json
{
  "project": {},
  "brief_status": "pending",
  "facets": [],
  "current_facet": null,
  "current_round": null
}
```

4. Tell the user you are starting a new design critique.
5. Invoke `design-crit:design-brief` to begin the brief stage.
6. Stop here. Do not continue to Step 3.

---

## Step 3: Route Based on State

Read `brief_status`, `facets`, and `current_facet` from state.json. Apply the first
matching rule:

### Rule A: Brief not confirmed

`brief_status` is `"pending"` or `"in-progress"`.

Action: Invoke `design-crit:design-brief`. Tell the user you are picking up the brief
where they left off.

### Rule B: Brief confirmed, no facet plan

`brief_status` is `"confirmed"` and `facets` is empty (`[]`).

Action: Invoke `design-crit:facet-planning`. Tell the user the brief is set and it is
time to plan which design facets to evaluate.

### Rule C: Facet plan confirmed, facets in progress

`facets` is non-empty and at least one enabled facet does NOT have `status: "locked"`
or `status: "skipped"`.

Determine the current facet:
1. If `current_facet` is set and its status is `"in-progress"`, use it.
2. Otherwise, find the first enabled facet with `status: "pending"` whose `depends_on`
   facets are all locked or skipped. Set it as `current_facet` in state.json.
3. If no facet qualifies (dependencies unmet), report the blocker to the user.

Read the facet's `skill` field from state.json. Invoke it as `design-crit:{skill}`.
For example, if `skill` is `"crit-navigation"`, invoke `design-crit:crit-navigation`.

Before invoking, check for unprocessed feedback:
- Look for `feedback-round-{N}.json` in `.design-crit/facets/{facet-id}/` where N matches
  the current round. If found and not yet processed, tell the user you see their feedback
  and are continuing the crit loop.

### Rule D: All facets locked or skipped

Every enabled facet has `status: "locked"` or `status: "skipped"`.

Action: Invoke `design-crit:design-direction`. Tell the user all facets are resolved and
you are synthesizing the design direction.

### Rule E: Already complete

`state.json` contains a `"direction_status": "complete"` field (set by design-direction
after deliverables are generated).

Action: Generate the overview page (Step 4). Summarize the completed crit with all locked
facets and deliverable links (`direction.html`, `design-tokens.json`, `decisions.md`).
Offer: "Review any facet", "Re-crit a facet" (invoke its skill directly), or "Start a
new crit" (reset state.json).

---

## Step 4: Generate Overview Page

Generate or regenerate `.design-crit/overview.html` after every state change. This is the
persistent progress dashboard. Single-page HTML, self-contained with inline CSS.

**Header:** Project name, brief summary (first 2-3 sentences from brief.md), pipeline
progress indicator (Brief > Facets > Direction).

**Facet checklist:** Render each facet from state.json as a row:

```
[checkmark] Navigation -- locked (Deep Sidebar)                    [Review]
[checkmark] Color System -- locked, auto (Soft Indigo)             [Review]
[spinner]   Typography -- round 2, 2 options surviving             [Continue]
[ ]         Density -- pending
[skip]      Motion -- skipped                                      [Review]
```

- `locked` + `decided_by: "user"` -- checkmark, winner name, Review button
- `locked` + `decided_by: "llm"` -- checkmark, "auto" tag, winner name, Review button
- `in-progress` -- spinner, round number, surviving count, Continue button
- `pending` -- empty checkbox
- `skipped` -- skip indicator, Review button

**Review expansion:** When Review is clicked, expand inline: options per round with names,
user feedback excerpts, eliminations with reasons, final decision + rationale, and whether
user-decided or LLM-delegated. Read from feedback-round-N.json files and state.json.

**Decision history** (after 1+ facets locked): Timeline of locked decisions in order with
constraint propagation notes showing which locked decisions influenced later facets.

After writing overview.html, open it in the user's browser (`open` on macOS, `xdg-open`
on Linux, `start` on Windows).

---

## Step 5: Re-entry Handling

When the user invokes `/design-crit` on a project with existing state, orient them before
routing.

1. Read state.json.
2. Summarize the current status in 2-3 sentences:
   - How many facets are locked, in progress, and pending
   - What the current facet is and what round it is on
   - Whether there is unprocessed feedback waiting
3. Then route per Step 3.

Example re-entry message:

> Your design crit for Remarc has 3 of 10 facets locked. You are on round 2 of
> Typography with 2 options surviving. I see feedback from your last round -- let me
> continue from there.

---

## Facet Transition

When a facet skill reports that it has locked a decision (or the user skipped a facet):

1. Update state.json: set the facet's status and clear `current_facet`.
2. Regenerate overview.html (Step 4).
3. Open the overview page so the user sees progress.
4. Identify the next facet per Rule C logic.
5. Announce the transition: "Navigation is locked as Deep Sidebar. Next up: Layout.
   This builds on your locked navigation and screen inventory."
6. Invoke the next facet's skill.

If the facet that just locked was the last one, follow Rule D instead.

---

## Error Handling

- **Missing feedback file:** Offer to accept pasted JSON directly in chat as a fallback.
  Parse it and write the feedback file yourself.
- **Corrupted state.json:** Report the error. Offer to reset state (preserving HTML
  artifacts) or attempt repair by reading facet directories to reconstruct state.
- **Dependency deadlock:** List blocking dependencies. Suggest resolving or skipping them.

---

## Constraints

- Never skip the brief stage. The brief is the foundation for all design decisions.
- Never auto-advance past a stage without user awareness. Always announce transitions.
- Always regenerate overview.html after state changes so the user has a current dashboard.
- When invoking sub-skills, use the `design-crit:skill-name` format.
- Do not duplicate sub-skill logic here. Route to the skill and let it do its work.
- This skill is the router and progress tracker. The sub-skills do the design work.
