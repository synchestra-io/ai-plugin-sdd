# Feature: Ideate Skill

> [View in Synchestra Hub](https://hub.synchestra.io/project/features?id=spec-studio@synchestra-io@github.com&path=spec%2Ffeatures%2Fskills%2Fideate) — graph, discussions, approvals

**Status:** Draft

## Summary

The `spec-studio:ideate` skill refines raw, vague concepts into lint-clean SpecScore Idea artifacts through structured divergent and convergent thinking. Its output (`spec/ideas/<slug>.md`) is the gating input for `spec-studio:specify`. Implementation lives at [`skills/ideate/`](../../../../skills/ideate/).

## Problem

Vague intent silently corrupts every downstream artifact. When a developer tells an AI agent "build me X" without first articulating the problem, alternatives, and explicit non-goals, the agent fills in the gaps stochastically — producing features that solve the wrong problem, miss the obvious "Not Doing" boundary, or inherit the developer's unstated assumptions as if they were specifications.

`ideate` exists to pay the ideation cost up front and capture it in a typed artifact, instead of paying it implicitly across every later phase of the lifecycle.

## Behavior

The skill operates as a guided dialogue that produces a single SpecScore Idea artifact at `spec/ideas/<slug>.md` containing Problem Statement, Recommended Direction, Alternatives Considered, MVP Scope, Not Doing, Key Assumptions to Validate, and Open Questions. The skill enforces a hard gate: it does not invoke `specify`, `writing-plans`, or any implementation skill until the Idea is lint-clean (`specscore lint` passing) and the user has explicitly approved it.

Triggers include `ideate`, `/ideate`, "refine this idea", and "stress-test this." Detailed behavior is documented in the skill manifest at [`skills/ideate/SKILL.md`](../../../../skills/ideate/SKILL.md).

## Acceptance Criteria

Not defined yet.

## Outstanding Questions

- Acceptance criteria not yet defined for this feature.
- What lint rules should an Idea-emitting skill assume vs. what should be enforced by `specscore lint` directly?
- How should `ideate` handle ideas that span multiple Features — does it always produce one Idea, or can it fork mid-conversation?
- Should the skill capture the dialogue trace itself as an artifact, or only the convergent output?
- When the user wants to skip ideation and go straight to `specify`, does `ideate` need to recognize that intent and gracefully hand off, or is that purely the user's responsibility?

---
*This document follows the https://specscore.md/feature-specification*
