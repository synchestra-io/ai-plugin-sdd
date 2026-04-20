---
name: design
description: |
  Turns an approved SpecScore Idea (or a clear buildable intent) into a
  SpecScore Feature with requirements and Given/When/Then acceptance
  criteria at spec/features/<slug>/. Gates implementation — no code,
  plans, or scaffolding until the Feature is lint-clean and
  user-approved. Optionally scaffolds Rehearse test stubs.
  Trigger: "design this", "/design", "spec this out", or Synchestra
  event `idea.approved`.
aliases: [design]
---

# Design

Turn approved intent into a lintable, testable SpecScore Feature.

## Hard Gate

<HARD-GATE>
Do NOT invoke `writing-plans`, `frontend-design`, `mcp-builder`, or ANY implementation skill until ALL of the following are true:
  1. The Feature artifact exists at `spec/features/<slug>/README.md` plus at least one `spec/features/<slug>/requirements/*.md`.
  2. Each requirement has ≥1 acceptance criterion in `Given / When / Then` format.
  3. `specscore lint spec/features/<slug>/` passes.
  4. The spec-document reviewer subagent returned `Approved`.
  5. The user has explicitly approved the written Feature.

This applies to **every** Feature, regardless of perceived simplicity. The only skill invoked after `spec-studio:design` is `writing-plans`.
</HARD-GATE>

## When to Use

- A SpecScore Idea is `Approved` and ready to become a Feature.
- User has a clear, high-conviction buildable intent (may skip `spec-studio:ideate`).
- Behavior of an existing Feature needs to change — **revise in place** (see [path-conventions.md](../shared/path-conventions.md)).

## Anti-Pattern: "This Is Too Simple To Need A Design"

Every Feature goes through this. A toggle, a one-line config, a single utility — all of them. Simple Features are where unexamined assumptions cost the most. The design can be short (a few sentences for truly simple Features), but it **must** be written and approved.

## Pre-Flight

1. **Inputs check.** If triggered from an approved Idea, load it and list the Idea's assumptions that this Feature must validate. If no Idea exists, ask: "Is this ready to design, or should we ideate first?" — don't force `spec-studio:ideate` on high-conviction users.
2. **Scope decomposition.** If the intent spans multiple independent subsystems, stop and help the user decompose into multiple Features before continuing.
3. **Revision vs new.** If a Feature with this slug already exists, decide: revise in place (default) or create a successor with `supersedes:` (only when scope change invalidates existing ACs). See [path-conventions.md](../shared/path-conventions.md).

## Checklist

Create a task for each and complete in order:

1. **Explore project context** — existing Features, recent commits, related specs.
2. **Scope decomposition check.**
3. **Offer visual companion** (if visual questions are ahead) — own message, no other content. See [visual-companion.md](references/visual-companion.md) (still TBD — see [the analysis doc §11.1](../../spec/ideas/ideate-vs-brainstorming-skills-analysis.md)).
4. **Ask clarifying questions** — one at a time, multiple-choice preferred. See [question-cadence.md](../shared/question-cadence.md).
5. **Propose 2–3 approaches** with trade-offs; lead with your recommendation. Use `spec-studio:ideate` lenses (inversion, constraint removal, simplification) where useful.
6. **Present design sections** one at a time, get approval after each.
7. **Author the Feature artifact** — `README.md` + `requirements/*.md`.
8. **Rehearse stub decision** — per-AC heuristic. See [rehearse-heuristic.md](../shared/rehearse-heuristic.md).
9. **Lint** — `specscore lint spec/features/<slug>/`.
10. **Inline self-review** — placeholders, consistency, scope, ambiguity.
11. **Dispatch spec-document-reviewer subagent** — see [reviewer-prompt.md](references/reviewer-prompt.md). Must return `Approved` before user review.
12. **User review gate** — user reviews the written Feature; wait for approval.
13. **Emit events** — `feature.designed` on reviewer approval; `feature.approved` on user approval. See [synchestra-events.md](../shared/synchestra-events.md).
14. **Transition to `writing-plans`.**

## Design Sections (scale to complexity)

- **Purpose & user job** — from the Idea's HMW if applicable, or restated here.
- **Requirements** — numbered, each with ≥1 acceptance criterion.
- **Architecture & components** — isolation, interfaces, dependencies. Each unit: what does it do, how is it used, what does it depend on?
- **Data flow.**
- **Error handling & failure modes.**
- **Testing strategy** — references Rehearse stubs (or explains why none).
- **Not Doing / Out of Scope** — inherited from Idea + design-level cuts.
- **Assumption carryover** — which Idea assumptions survive; which are now invalidated or answered.

## Artifact Layout and Front-Matter

```
spec/features/<slug>/
├── README.md                # Feature overview + front-matter
├── requirements/
│   ├── <req-1-slug>.md
│   ├── <req-2-slug>.md
│   └── …
├── assets/                  # Optional — diagrams, mockups (see Q11.1)
└── tests/                   # Optional — Rehearse stubs (see shared/rehearse-heuristic.md)
```

**`README.md` front-matter:**

```yaml
---
type: feature
id: feat-<slug>
status: Draft
date: YYYY-MM-DD
owner: <author>
source_idea: idea-<slug>     # null if no originating Idea
supersedes: []               # set only on wholesale replacement
---
```

**Requirement file front-matter:**

```yaml
---
type: requirement
id: req-<feat-slug>-<req-slug>
feature: feat-<slug>
status: Draft
---

# <Requirement name>

<One-paragraph description>

## Acceptance Criteria

### AC-1: <name>
**Given** <precondition>
**When** <action>
**Then** <observable outcome>

### AC-2: <name>
…
```

## Acceptance Criterion Format

Every AC uses `Given / When / Then`. This is **enforced by lint rule F-004**.

```
Scenario: <short name>
Given <precondition that sets state>
When <action the system or user takes>
Then <observable outcome that can be checked>
```

If you can't phrase an outcome as `Then <observable>`, the AC is too abstract — sharpen it.

## Rehearse Stub Decision

After drafting ACs, for each one apply the heuristic in [rehearse-heuristic.md](../shared/rehearse-heuristic.md):

- **Testable** (has CLI/HTTP/pure-fn/data/UI-selector/fs/event surface): scaffold `spec/features/<slug>/tests/<req-id>-<ac-id>.md` with `status: pending`.
- **Not testable** (subjective, abstract, undefined observer, doc-only Feature): skip; record reason in the Feature's `README.md` under `## Rehearse Integration`.

The user can always override the heuristic.

## Inline Self-Review

Check:

1. **Placeholder scan** — `TBD`, `TODO`, incomplete sections, vague requirements.
2. **Internal consistency** — architecture matches feature descriptions; requirements align with ACs.
3. **Scope check** — focused enough for one implementation plan? If not, decompose.
4. **Ambiguity check** — could any requirement be interpreted two ways? Pick one; make it explicit.

Fix inline. Don't re-review; move on.

## Reviewer Subagent

Dispatch a spec-document-reviewer subagent using [reviewer-prompt.md](references/reviewer-prompt.md). Status must be `Approved` before the user review gate. If `Issues Found`, fix and re-dispatch — advisory recommendations may be ignored.

## User Review Gate

After reviewer subagent returns `Approved`:

> "Feature written and lint-clean at `spec/features/<slug>/`. Reviewer subagent approved. Please review and let me know if you want changes before we move to `writing-plans`."

Wait. If the user requests changes, fix, re-lint, re-review, re-gate. Only proceed once the user approves.

On approval:
- Update `status: Draft → Approved`.
- Re-run lint.
- Emit `feature.approved` event.

## Transition to Implementation

Invoke `writing-plans`. Do **not** invoke any other skill. `writing-plans` is the next step.

## Visual Companion (Optional)

**Status:** decision pending — see `spec/ideas/ideate-vs-brainstorming-skills-analysis.md` §11.1.

Until the visual-companion strategy is decided, prefer these lightweight visual aids in text:

- **Mermaid diagrams** embedded in `README.md` (most IDEs and GitHub render these natively).
- **ASCII diagrams** for simple flows.
- **Static SVGs** committed to `spec/features/<slug>/assets/` for complex visuals.

If the user has `obra/superpowers` installed, we may reuse its browser-based visual companion — pending a formal integration decision.

## Verification

- [ ] `spec/features/<slug>/README.md` exists
- [ ] At least one requirement file under `requirements/`
- [ ] Every requirement has ≥1 acceptance criterion
- [ ] Every AC is `Given / When / Then`
- [ ] `specscore lint spec/features/<slug>/` passes
- [ ] Reviewer subagent returned `Approved`
- [ ] User explicitly approved the written Feature
- [ ] `status: Approved` in front-matter
- [ ] Rehearse decision recorded (stubs scaffolded OR skip-reason noted)
- [ ] Source Idea (if any) linked via `source_idea:` — Synchestra handles the reverse link
- [ ] `feature.designed` + `feature.approved` events emitted

## Red Flags

- Proceeding to `writing-plans` without user approval
- Requirements without ACs
- ACs not in `Given / When / Then`
- "Too simple to spec" rationalization
- Scope spanning multiple subsystems
- Assumptions from the source Idea silently dropped
- Writing to `docs/superpowers/specs/` instead of `spec/features/<slug>/`
- Skipping the reviewer subagent
- Invoking any skill other than `writing-plans` on transition

## References

- [reviewer-prompt.md](references/reviewer-prompt.md) — spec-document reviewer subagent template.
- [visual-companion.md](references/visual-companion.md) — visual companion strategy (decision pending).
- [philosophy.md](../shared/philosophy.md) — shared tenets.
- [path-conventions.md](../shared/path-conventions.md) — `spec/` vs `docs/` rules.
- [specscore-lint-rules.md](../shared/specscore-lint-rules.md) — lint contract this skill assumes.
- [synchestra-events.md](../shared/synchestra-events.md) — event payloads emitted by this skill.
- [question-cadence.md](../shared/question-cadence.md) — when to batch vs single-question.
- [rehearse-heuristic.md](../shared/rehearse-heuristic.md) — per-AC testability decision.
