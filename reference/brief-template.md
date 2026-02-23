# Brief Template

Reference for the `design-brief` skill. Defines what an optimal brief contains, how
to score context sufficiency, and what good and bad briefs look like.

The brief is a scoping tool, not a brainstorming tool. Show what you know, highlight
what you don't. A few good questions produce dramatically better wireframes.

---

## Brief Schema

An optimal brief fills every field below. Most projects will not start with all of them.
That is fine — the gaps are the work.

### 1. Project Name and Description
One line. What is this thing? Not a pitch, not a feature list. The sentence you would
use to tell a colleague. Example: "Remarc — a personal knowledge manager for developers."

### 2. Target Users and Context of Use
Who uses this, where, and when? "Commuters on their phone" produces a fundamentally
different design than "analysts at a desk with two monitors." Capture the persona
(role, not demographics), device and posture, environment, and usage frequency.

### 3. Core Interaction Loop
The one thing users do repeatedly. Every screen orbits this. Example: "Capture a
thought, tag it, find it later." If the user cannot articulate this, the brief is
not ready for design work.

### 4. Differentiator
What makes THIS product different from the obvious alternatives? "Nothing yet" is
valid, but "it's for couples" or "it works offline-first" changes the entire screen
inventory. If nothing is different, say so — the design leans on execution.

### 5. Platform and Constraints
Platform (web, iOS, Android, desktop, combination), tech stack (React, Flutter, Swift —
constrains component choices), device targets (phone-only, desktop-first, responsive),
and hard constraints (offline, RTL, bundle size).

### 6. Scope
What is in v1? What is explicitly out? Scope prevents the design from solving problems
the code will not ship. Both in-scope and out-of-scope must be stated.

### 7. Existing Design Language or References
Visual identity, component library, brand guidelines, or reference products? If the
codebase uses Tailwind + shadcn, that constrains typography and component decisions.
"No existing design" is valid — the crit starts from a blank canvas.

### 8. Accessibility Requirements
WCAG level, keyboard-only users, screen reader support, touch targets, reduced motion.
If unknown, write "unknown — will audit in accessibility facet." Seeds the final
`crit-accessibility` pass.

---

## Evaluation Rubric

Score each field: **filled**, **partial**, or **empty**.

| Field                  | Filled                              | Partial                          | Empty   |
|------------------------|-------------------------------------|----------------------------------|---------|
| Name + description     | Clear one-liner                     | Vague or too long                | Missing |
| Target users           | Persona + device + context          | Has persona, missing context     | Missing |
| Core interaction loop  | One clear sentence                  | Multiple competing loops         | Missing |
| Differentiator         | Stated (even "none")                | Implied but not explicit         | Missing |
| Platform + constraints | Platform + stack + device targets   | Platform only                    | Missing |
| Scope                  | In-scope and out-of-scope listed    | In-scope only                    | Missing |
| Design language        | References or "none" stated         | Partial references               | Missing |
| Accessibility          | Requirements stated or "unknown"    | Vague mention                    | Missing |

### Sufficiency Levels

**Sufficient** — Core loop + platform + scope are filled; remaining fields filled or
partial. Action: draft the full brief, present it, ask user to confirm or adjust.

**Partial** — Some fields filled, but core loop, platform, or scope has gaps. Action:
draft what you can, ask 2-3 targeted questions for the critical gaps only.

**Minimal** — Most fields empty; vague idea, not a scoped project. Action: guide brief
creation field by field (name, then core loop, then platform). One question at a time.

---

## Example Briefs

### Rich Brief (existing codebase)

```
Project: Remarc — personal knowledge manager for developers
Users: Individual developers at their desk (laptop/desktop), daily use during work
Core loop: Capture a thought or snippet, tag it, search/retrieve it later
Differentiator: Markdown-native, local files, no cloud lock-in
Platform: Web (SPA), React + Tailwind + shadcn/ui, desktop-first but responsive
Scope in v1: Capture (modal + full editor), search, tags, collections, settings
Scope out: Collaboration, sharing, mobile app, plugins, AI features
Design language: shadcn/ui, Inter font, neutral palette — no existing brand
Accessibility: WCAG AA, full keyboard navigation, screen reader support
```

Sufficiency: **Sufficient**. Every field filled. Draft and present for confirmation.

### Minimal Brief (vague idea)

```
Project: some kind of habit tracker
Users: (empty)
Core loop: (empty)
Differentiator: (empty)
Platform: probably mobile
Scope: (empty)
Design language: (empty)
Accessibility: (empty)
```

Sufficiency: **Minimal**. Only the name and a tentative platform exist. Start guided
brief creation. Example opening questions:
1. "What does a user do every day in your habit tracker?" (core loop)
2. "Phone app, web app, or both?" (platform)
3. "What habits — fitness, learning, general?" (differentiator + scope)
Do not attempt wireframes until core loop, platform, and scope are filled.
