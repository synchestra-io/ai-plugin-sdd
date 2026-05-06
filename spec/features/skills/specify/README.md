# Feature: Specify Skill

> [View in Synchestra Hub](https://hub.synchestra.io/project/features?id=spec-studio@synchestra-io@github.com&path=spec%2Ffeatures%2Fskills%2Fspecify) — graph, discussions, approvals

**Status:** Draft

## Summary

The `spec-studio:specify` skill turns an approved SpecScore Idea — or a clear buildable intent — into a lint-clean SpecScore Feature with requirements and `Given / When / Then` acceptance criteria. Its output (`spec/features/<slug>/`) is the gating input for `spec-studio:plan` and any implementation skill. Implementation lives at [`skills/specify/`](../../../../skills/specify/).

## Problem

Even when an Idea is well-formed, the jump from "approved direction" to "buildable contract" is non-trivial. Without a structured skill, that jump happens ad-hoc: requirements get scattered into prose, acceptance criteria get omitted or written as vibes, and the Feature ends up as a description rather than a contract that downstream skills can consume.

`specify` exists to enforce the structural discipline of a SpecScore Feature — typed front-matter, requirement decomposition, G/W/T ACs — without forcing the user to memorize the schema.

## Behavior

The skill accepts two kinds of input: an approved Idea (the standard path, triggered by `idea.approved` or by the user pointing at a `spec/ideas/<slug>.md` file), or a clear buildable intent stated directly by the user. It produces a SpecScore Feature directory at `spec/features/<slug>/` containing a Feature README, requirements, acceptance criteria in `Given / When / Then` form, and (optionally) Rehearse test scaffolding. The skill enforces a hard gate: no plans, code, or implementation-skill invocations until `specscore lint` passes on the Feature and the user has explicitly approved it.

When invoked with a clear intent rather than an Idea, the skill still surfaces the same structural prompts (alternatives, non-goals, assumptions) inline — if the intent is not actually clear, the skill pushes back rather than producing a low-quality Feature. Detailed behavior is documented in the skill manifest at [`skills/specify/SKILL.md`](../../../../skills/specify/SKILL.md).

## Acceptance Criteria

Not defined yet.

## Outstanding Questions

- Acceptance criteria not yet defined for this feature.
- What is the precise contract for "clear buildable intent" — what does the skill require the user to state before it proceeds without a preceding Idea?
- Should `specify` always offer to scaffold Rehearse test stubs, or only when explicitly asked?
- How should the skill handle a Feature that supersedes an existing one — clone, archive-and-replace, or reject and require a Proposal?
- When the source Idea has unresolved Open Questions, does `specify` require them resolved first, or carry them forward as Outstanding Questions on the Feature?
- Should `specify` emit an event (`feature.specified`) on successful approval, mirroring the `idea.approved` pattern?

---
*This document follows the https://specscore.md/feature-specification*
