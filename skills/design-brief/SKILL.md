---
name: design-brief
description: Use when starting a new design project, when context seems insufficient for design decisions, or when explicitly gathering requirements before visual design work.
---

# Design Brief

You are creating a design brief -- the first deliverable of the design-crit pipeline. The
brief is a scoping tool, not a brainstorming tool. Your job is to show what you know about
the project and highlight what you do not. A few good questions produce dramatically better
wireframes than rushing past gaps.

The brief is the first deliverable, not a gate to clear. Present it as a draft YOU wrote,
with gaps explicitly called out. For rich-context projects, the user confirms in 30 seconds.
For vague ideas, the highlighted gaps are honest: "I don't know what makes your habit tracker
different from the 500 that exist. That matters for design."

---

## 1. Context Harvesting

Before writing anything, gather context from two sources: the codebase and the user.

### Codebase Signals

Scan the project root for these files. Read each one that exists. Do not ask the user for
information you can extract from the code.

| Signal | Where to Look | What It Tells You |
|---|---|---|
| Tech stack | `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `Gemfile`, `pubspec.yaml`, `build.gradle`, `*.csproj` | Framework, language, UI library (React, Vue, Flutter, SwiftUI, etc.) |
| UI component library | `tailwind.config.*`, `theme.ts`, `tokens.json`, imports from `shadcn`, `mui`, `chakra`, `radix`, `ant-design` | Existing design language, component constraints |
| Existing screens | `app/` or `src/` route files, `pages/` directory, `screens/` directory, navigation config files | What surfaces already exist |
| Component inventory | `components/` directory structure, shared component files | Existing UI patterns and primitives |
| Project description | `README.md`, `docs/`, `CONTRIBUTING.md`, `.github/` description | Project purpose, audience, goals |
| Existing design artifacts | `.design-crit/`, `design/`, `figma/`, `mockups/` | Prior design work, existing briefs |

### User Context

Capture everything the user has said about the project in the current conversation. Their
verbal description is a primary source -- often the richest one for early-stage projects.
Extract: what the product does, who it is for, what makes it different, what they want
designed.

### What NOT to Do

- Do not ask the user to repeat information available in the codebase.
- Do not scan irrelevant files (test fixtures, build output, node_modules).
- Do not fabricate context. If a field cannot be filled from available sources, mark it empty.

---

## 2. Evaluate Against the Brief Schema

Read `../../reference/brief-template.md` now. It contains the full brief schema, evaluation rubric,
and example briefs. Use it as your scoring guide.

The schema has eight fields:

1. **Project name and description** -- one clear sentence
2. **Target users and context of use** -- persona, device, posture, environment, frequency
3. **Core interaction loop** -- the one thing users do repeatedly
4. **Differentiator** -- what makes THIS product different ("none" is valid)
5. **Platform and constraints** -- web/native/both, tech stack, device targets, hard limits
6. **Scope** -- what is in v1, what is explicitly out
7. **Existing design language** -- component library, brand, references, or "none"
8. **Accessibility requirements** -- WCAG level, keyboard, screen reader, or "unknown"

### Scoring

Score each field as **filled**, **partial**, or **empty** using the rubric in
`../../reference/brief-template.md`. Then determine the sufficiency level:

**Sufficient** -- Core loop + platform + scope are filled. Remaining fields are filled or
partial. You have enough to draft a complete brief.

**Partial** -- Some fields filled, but core loop, platform, or scope has gaps. You can draft
most of the brief but need 2-3 answers to close critical gaps.

**Minimal** -- Most fields empty. Vague idea, not a scoped project. You need to guide the
user through brief creation field by field.

---

## 3. Branch by Sufficiency Level

### Branch A: Sufficient Context

You have enough to write the brief without asking questions.

1. Draft the full brief in the schema format. Fill every field from harvested context.
2. For any field scored "partial," include what you have and flag the gap inline:
   `(inferred from routes -- confirm this is complete)`.
3. Generate `.design-crit/overview.html` showing the draft brief. The HTML must be:
   - Self-contained (inline CSS, no external dependencies).
   - Readable on desktop. Clean typography, clear section headings.
   - Each field displayed with a visual indicator: green check (filled), yellow dot (partial),
     red dash (empty).
   - A **Confirm Brief** button and an **Edit** textarea per field so the user can adjust
     inline.
   - A note at the top: "This brief was drafted from your codebase and description. Review it,
     edit any field, then confirm."
4. Open `overview.html` in the user's browser.
5. Tell the user: "I drafted a brief from your codebase. Open the overview to review it. Let me
   know if anything needs adjustment, or confirm to proceed."
6. Wait for the user to confirm or request changes.
7. If the user provides edits, update the brief and regenerate overview.html. Repeat until
   confirmed.

### Branch B: Partial Context

You can draft most of the brief but critical fields are missing.

1. Draft every field you can fill. Leave empty fields explicitly blank.
2. Identify the 2-3 most impactful missing fields. Prioritize in this order:
   - Core interaction loop (if missing, nothing else matters)
   - Platform and constraints (changes every design decision)
   - Scope (prevents designing features that won't ship)
   - Target users (changes density, touch targets, flow)
3. Ask the user these 2-3 targeted questions. Be specific, not generic. Examples:
   - BAD: "Can you tell me about your users?"
   - GOOD: "Your codebase is a React SPA -- are your users primarily on desktop, or do you
     need mobile-responsive from day one?"
   - BAD: "What's the scope?"
   - GOOD: "I see routes for dashboard, settings, and a profile page. Is there anything else
     in v1, or is that the full surface area?"
4. Present what you have so far alongside your questions: "Here's what I've gathered so far.
   I need a few answers to complete the brief."
5. After receiving answers, merge them into the draft and proceed to Branch A (generate
   overview.html, ask for confirmation).

### Branch C: Minimal Context

Most fields are empty. You cannot draft a useful brief without guidance.

1. Tell the user honestly: "I don't have enough context to draft a brief yet. Let me ask a
   few questions to get us there."
2. Ask ONE question at a time, in this order:
   a. **Name + description:** "What is this project? One sentence."
   b. **Core loop:** "What does a user do repeatedly? The one action that defines the product."
   c. **Platform:** "Web app, mobile app, desktop app, or some combination?"
   d. **Target users:** "Who uses this, and where? Phone on the go, or laptop at a desk?"
   e. **Scope:** "What's in the first version? What's explicitly out?"
   f. **Differentiator:** "What makes this different from [obvious alternative]? 'Nothing yet'
      is a valid answer."
3. After each answer, acknowledge it and ask the next question. Do not batch questions -- the
   user needs to think through each one.
4. After you have answers for at least name, core loop, and platform, draft the brief with
   what you have and proceed to Branch A.
5. Remaining empty fields get marked: `(not yet defined -- will surface during crit)`.

### Question Guidelines (All Branches)

- Frame questions around design impact, not information gathering for its own sake.
  "This matters because..." should be implicit in every question.
- Never ask more than 3 questions at once (Branch B). One at a time for Branch C.
- Accept "I don't know" gracefully. Write it as "unknown" in the field and move on.
  The crit process will surface these decisions later.
- If the user gives a long, rambling answer, extract the brief-relevant parts and confirm:
  "So the core loop is [X] -- is that right?"

---

## 4. Write the Brief

Once the user confirms (or you have enough to draft after Branch B/C questioning), write
the final brief.

### File: `.design-crit/brief.md`

Use this exact format:

```markdown
# Design Brief: [Project Name]

> [One-line description]

## Target Users and Context of Use
[Persona, device, posture, environment, frequency]

## Core Interaction Loop
[The one thing users do repeatedly]

## Differentiator
[What makes this different, or "None identified -- design leans on execution"]

## Platform and Constraints
- **Platform:** [web / iOS / Android / desktop / combination]
- **Tech stack:** [framework, UI library, key dependencies]
- **Device targets:** [phone-only / desktop-first / responsive]
- **Hard constraints:** [offline, RTL, bundle size, or "none identified"]

## Scope
### In v1
- [item]
- [item]

### Out of v1
- [item]
- [item]

## Existing Design Language
[Component library, brand guidelines, reference products, or "None -- starting fresh"]

## Accessibility Requirements
[WCAG level, keyboard, screen reader, touch targets, or "Unknown -- will audit in
accessibility facet"]
```

### Fields with Unknown Values

For any field the user could not answer, write one of:
- `Unknown -- will surface during [relevant facet] crit`
- `Not yet defined -- design will explore options`
- `None identified` (for differentiator or design language)

Do NOT leave fields blank. Every field must have content, even if that content is an honest
"unknown."

---

## 5. Update State

After writing `brief.md`, update `.design-crit/state.json`.

If `state.json` does not exist, create it:

```json
{
  "project": {
    "name": "[from brief]",
    "description": "[from brief]",
    "platform": "[from brief]",
    "tech_stack": ["[from brief]"]
  },
  "brief_status": "confirmed",
  "facets": [],
  "current_facet": null,
  "current_round": null
}
```

If `state.json` already exists (re-running the brief), update only:
- `project` fields with any new information
- `brief_status` to `"confirmed"`

Do NOT overwrite existing facet data if the user is re-running the brief mid-pipeline.

---

## 6. Generate Overview HTML

After confirming the brief, generate or update `.design-crit/overview.html`.

This page serves as the persistent dashboard for the entire design-crit process. At the
brief stage, it shows:

- The confirmed brief, formatted cleanly with all eight fields
- A status banner: "Brief confirmed. Next: Facet Planning."
- Visual indicators per field (filled/partial/unknown)
- Project metadata: name, platform, tech stack

Requirements:
- Self-contained HTML with inline CSS. No external dependencies.
- Desktop-first, clean typography, readable at a glance.
- Open it in the user's browser after generating.

---

## 7. Handoff

After the brief is confirmed, state is updated, and overview is generated:

1. Announce to the user: "Brief confirmed. Moving to facet planning -- I'll analyze the brief
   and recommend which design facets to evaluate for [project name]."
2. Invoke `design-crit:facet-planning` to continue the pipeline.

Do not wait for additional user input before handing off. The pipeline continues automatically.

---

## Summary of Outputs

| Artifact | Path | Purpose |
|---|---|---|
| Brief | `.design-crit/brief.md` | The confirmed design brief |
| State | `.design-crit/state.json` | Pipeline state with `brief_status: confirmed` |
| Overview | `.design-crit/overview.html` | Visual dashboard showing the brief |

---

## Principles to Remember

- **Show your work.** Present the brief as YOUR draft, not a form for the user to fill out.
  You did the research. You wrote the brief. The user reviews and confirms.
- **Purposeful friction.** The 2-3 questions you ask in Branch B are not bureaucracy -- they
  produce dramatically better wireframes downstream. Frame them that way.
- **Gaps are honest.** "Unknown" is better than a guess. The crit process will surface these
  decisions at the right time.
- **Speed for rich projects.** If the codebase tells you everything, the brief is a 30-second
  confirmation, not a 10-minute interview.
- **Patience for vague projects.** If the user has a vague idea, the brief creation IS the
  valuable work. Do not rush it.
