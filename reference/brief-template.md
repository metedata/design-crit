# Brief Template

Reference for the `design-brief` skill. Defines what an optimal brief contains, how
to assess available context, and what good and bad briefs look like.

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

## How Much Context Is Available?

Internally assess how much context exists between the codebase and what the user has
said. This determines your approach — but never expose this assessment to the user.
No scores, no rubrics, no labels.

**Rich context** — Core loop + platform + scope are clear from the codebase and user
description; most other fields have substance. Draft the full brief yourself and
present it for the user to review.

**Partial context** — Some fields are clear but critical pieces are missing (what
users actually do, what platform, or what's in v1). Draft what you can and ask 2-3
specific questions to fill the gaps.

**Thin context** — Vague idea, barely any detail. Walk the user through it
conversationally, one question at a time, starting with what it is, what users do
in it, and what platform it runs on.

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

Rich context — every field is clear. Draft and present for confirmation.

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

Thin context — only the name and a tentative platform exist.
Walk through it conversationally. Example opening questions:
1. "What does a user do every day in your habit tracker?" (core loop)
2. "Phone app, web app, or both?" (platform)
3. "What habits — fitness, learning, general?" (differentiator + scope)
Do not attempt wireframes until core loop, platform, and scope are filled.
