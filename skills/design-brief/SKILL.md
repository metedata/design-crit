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

**This entire stage happens in the terminal.** No interactive HTML. No browser-based forms.
You present the brief, ask questions, and get confirmation -- all in the chat. A read-only
overview page is generated at the very end as a reference.

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

## 2. Assess Available Context

Read `../../reference/brief-template.md` now. It contains the brief schema and examples.

A good brief covers eight things:

1. **Project name and description** -- one clear sentence
2. **Target users and context of use** -- persona, device, posture, environment, frequency
3. **Core interaction loop** -- the one thing users do repeatedly
4. **Differentiator** -- what makes THIS product different ("none" is valid)
5. **Platform and constraints** -- web/native/both, tech stack, device targets, hard limits
6. **Scope** -- what is in v1, what is explicitly out
7. **Existing design language** -- component library, brand, references, or "none"
8. **Accessibility requirements** -- WCAG level, keyboard, screen reader, or "unknown"

Internally assess how much context is available from the codebase and what the user has
said. **Never expose this assessment to the user.** No scores, no rubrics, no labels.
Just figure out which branch to take:

**Rich context** -- Core loop + platform + scope are clear. You can draft a complete brief.

**Partial context** -- Critical fields like core loop, platform, or scope have gaps. You
need 2-3 answers before you can write a solid brief.

**Thin context** -- Most fields empty. You need to walk the user through it.

---

## 3. Branch by Available Context

### Branch A: Rich Context

You have enough to write the brief without asking questions.

1. Draft the full brief in the schema format. Fill every field from harvested context.
2. For any field you inferred or guessed, flag it inline:
   `(inferred from routes -- confirm this is complete)`.
3. **Present the brief directly in the chat.** Show it as clean markdown so the user
   can read it without leaving the terminal. This is the primary way they review it.
4. **Ask 2-3 specific follow-up questions** to sharpen the brief. Even when you have
   strong context, there are always design-relevant details the codebase cannot tell you.
   Examples:
   - "Your app has a dashboard and settings page. Is there a primary action users take
     most often, or is it more of a monitoring tool?"
   - "I see you're using Tailwind -- do you have a visual style in mind, or should we
     explore that from scratch?"
   - "Is this mostly for desktop users, or do you need it to work well on phones too?"
5. **Wait for the user to respond.** They will either:
   - Confirm ("looks good", "that's right")
   - Request changes ("change X to Y", "the core loop is actually...")
   - Answer your follow-up questions
6. If the user requests changes, update the brief and present the updated version.
   Repeat until they confirm.
7. Once confirmed, proceed to Section 4 (Write the Brief).

### Branch B: Partial Context

You can draft most of the brief but critical fields are missing.

1. Draft every field you can fill. Leave unknown fields marked clearly.
2. **Present what you have so far in the chat.** Frame it conversationally: "Here's what
   I've pieced together so far. I have a few questions before I can finish the brief."
3. Ask the user 2-3 targeted questions for the critical gaps. Be specific, not generic.
   Prioritize in this order:
   - Core interaction loop (if missing, nothing else matters)
   - Platform and constraints (changes every design decision)
   - Scope (prevents designing features that won't ship)
   - Target users (changes density, touch targets, flow)
4. Frame questions around why they matter for design:
   - BAD: "Can you tell me about your users?"
   - GOOD: "Your codebase is a React SPA -- are your users primarily on desktop, or do you
     need mobile-responsive from day one? This changes how we think about layout."
   - BAD: "What's the scope?"
   - GOOD: "I see routes for dashboard, settings, and a profile page. Is there anything else
     in v1, or is that the full surface area?"
5. After receiving answers, merge them into the draft and proceed to Branch A step 3
   (present full brief in chat, ask follow-ups, wait for confirmation).

### Branch C: Thin Context

Most fields are empty. You need to learn about the project first.

1. Tell the user conversationally: "I'd like to understand the project better before we
   start designing. Let me ask a few questions."
2. Ask ONE question at a time, in this order:
   a. "What is this project? Give me the one-sentence version."
   b. "What's the main thing a user does in it? The action they'd repeat most often."
   c. "Web app, mobile app, desktop app, or some combination?"
   d. "Who uses this, and where? Phone on the go, or laptop at a desk?"
   e. "What's in the first version? And what's explicitly NOT in it yet?"
   f. "What makes this different from [obvious alternative]? 'Nothing yet' is a totally
      valid answer."
3. After each answer, acknowledge it briefly and ask the next question. Do not batch
   questions -- let the user think through each one.
4. After you have answers for at least the project description, core loop, and platform,
   draft the brief with what you have and proceed to Branch A step 3.
5. Remaining unknown fields get marked: `(not yet defined -- we'll figure this out as we go)`.

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

Once the user confirms, write the final brief.

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

After confirming the brief and updating state, generate `.design-crit/overview.html`.

This is a **read-only** dashboard -- not interactive. No editable fields, no buttons, no
forms. It shows the confirmed brief as a clean, formatted reference page.

Contents:
- The confirmed brief, formatted cleanly with all eight fields
- A status banner: "Brief confirmed. Next up: we'll plan which design areas to review."
- Project metadata: name, platform, tech stack

**Do NOT include:**
- Editable fields or textareas
- Confirm/submit buttons
- Scoring indicators, colored dots, or field status badges
- Any interactive JavaScript

Requirements:
- Self-contained HTML with inline CSS. No external dependencies.
- Follow `../../reference/crit-ui.md` for colors, typography, and spacing.
- Desktop-first, clean typography, readable at a glance.
- Open it in the user's browser after generating.

---

## 7. Handoff

After the brief is confirmed, state is updated, and overview is generated:

1. Explain what happens next in plain terms: "Great, the brief is locked in. Next, I'll
   figure out which design areas we should review together -- things like navigation,
   layout, colors, typography. I'll come back with a plan for your project specifically."
2. Invoke `design-crit:facet-planning` to continue the pipeline.

Do not wait for additional user input before handing off. The pipeline continues automatically.

---

## Summary of Outputs

| Artifact | Path | Purpose |
|---|---|---|
| Brief | `.design-crit/brief.md` | The confirmed design brief |
| State | `.design-crit/state.json` | Pipeline state with `brief_status: confirmed` |
| Overview | `.design-crit/overview.html` | Read-only dashboard showing the brief |

---

## Principles to Remember

- **Show your work.** Present the brief as YOUR draft, not a form for the user to fill out.
  You did the research. You wrote the brief. The user reviews and confirms.
- **Everything happens in the chat.** Present the brief in the terminal, ask questions in
  the terminal, confirm in the terminal. The HTML overview is just a reference artifact.
- **Purposeful friction.** The 2-3 questions you ask in Branch B are not bureaucracy -- they
  produce dramatically better wireframes downstream. Frame them that way.
- **Gaps are honest.** "Unknown" is better than a guess. The crit process will surface these
  decisions at the right time.
- **Speed for rich projects.** If the codebase tells you everything, the brief is a 30-second
  confirmation, not a 10-minute interview.
- **Patience for vague projects.** If the user has a vague idea, the brief creation IS the
  valuable work. Do not rush it.
