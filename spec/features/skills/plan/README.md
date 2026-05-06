# Feature: Plan Skill

> [View in Spec Studio](https://specstudio.synchestra.io/project/features?id=spec-studio@synchestra-io@github.com&path=spec%2Ffeatures%2Fskills%2Fplan) — graph, discussions, approvals

**Status:** Draft
**Source Ideas:** spec-studio-plan-skill

## Summary

The `spec-studio:plan` skill is intended to turn an approved SpecScore Feature into a lint-clean Plan artifact at `spec/plans/<slug>.md` — an ordered list of tasks where each task references one or more AC IDs from its source Feature. The skill is not yet implemented; its scope is captured in the [Approved Idea](../../../ideas/spec-studio-plan-skill.md).

## Problem

`spec-studio:specify` produces lint-clean Features with G/W/T acceptance criteria. The next step — decomposing those ACs into ordered, executable tasks — currently has no Spec Studio skill. Users fall back to generic planning skills (`superpowers:writing-plans`, `agent-skills:planning-and-task-breakdown`), which are SpecScore-blind: they do not consume Feature front-matter, do not map tasks to AC IDs, do not lint, and do not emit Synchestra events.

The result is that spec↔code coherence — the central Spec Studio principle — breaks at exactly the handoff where it matters most: the moment ACs become work.

## Behavior

The skill is intended to consume an approved Feature, run a short convergent task-breakdown dialogue, and write a SpecScore-compliant Plan artifact at `spec/plans/<slug>.md`. Each task in the plan references one or more AC IDs from its source Feature; the skill enforces AC coverage (every AC has at least one task) as a hard gate. The skill mirrors the gate discipline of `ideate` and `specify`: lint-clean output and explicit user approval before any `build` or implementation skill can run.

The MVP is scoped as a two-week spike with no Rehearse scaffolding, no dispatch to runners, and no editor UI. Detailed scope, alternatives considered, and key assumptions live in the source Idea at [`spec/ideas/spec-studio-plan-skill.md`](../../../ideas/spec-studio-plan-skill.md). Implementation will live at `skills/plan/` once the Idea is promoted via `spec-studio:specify`.

## Acceptance Criteria

Not defined yet.

## Outstanding Questions

- Acceptance criteria not yet defined for this feature.
- Does the plan slug mirror the source Feature slug 1:1, or can one Feature have multiple Plans (alternative breakdowns)?
- What is the lint rule for AC coverage — every AC mapped to at least one task, or is partial coverage allowed with explicit justification?
- Should plan tasks be ordered strictly, partially ordered (DAG), or a mix?
- Does `plan` consume a single Feature only, or can it span multiple Features in one Plan?
- How is task granularity bounded — what stops a task from being a thin wrapper around an entire AC, or conversely from being too fine to track?

---
*This document follows the https://specscore.md/feature-specification*
