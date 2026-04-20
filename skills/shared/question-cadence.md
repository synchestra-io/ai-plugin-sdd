# Question Cadence Guide

**Status:** Shared convention
**Applies to:** `spec-studio:ideate`, `spec-studio:design`

Upstream `idea-refine` and `brainstorming` disagree on question cadence. `idea-refine` will batch 3–5 sharpening questions; `brainstorming` hard-enforces one-at-a-time. SpecScore skills **choose cadence by phase**, not by skill.

## Decision Rule

**Batch questions** when the user already has context and the questions are independent refinements of a shared topic.

**Single question** when the user's next answer would change which questions are worth asking.

## Cadence by Phase

| Phase | Cadence | Why |
|---|---|---|
| `spec-studio:ideate` Phase 1 — Understand & Expand | **Batch 3–5** | User arrived with an idea; asking "who, success, constraints, prior art, why now" in parallel is efficient and doesn't create branching paths. |
| `spec-studio:ideate` Phase 2 — Evaluate & Converge | **Single** | The user's reaction to a stress-test shapes the next stress-test. Sequential. |
| `spec-studio:ideate` Phase 3 — Crystallize | **None** | Skill authors the artifact; user reviews at the end. |
| `spec-studio:design` — Scope check | **Single** | The decompose/proceed answer gates everything downstream. |
| `spec-studio:design` — Clarifying | **Single, multiple-choice preferred** | Requirements detail branches fast; one at a time keeps the tree clean. |
| `spec-studio:design` — Approach proposal | **Triplet presented together, single choice returned** | User picks one of 2–3 approaches; not a question — a decision. |
| `spec-studio:design` — Section review | **Single per section** | Approve each design section (architecture, data flow, etc.) before the next. |

## Formatting

- When batching, use `AskUserQuestion` with multiple discrete question blocks or a numbered list the user can answer inline.
- When single, prefer the `AskUserQuestion` tool with a multiple-choice shape when the answer space is discrete; fall back to open-ended when it isn't.
- Never ask "any other questions?" as filler — it's a non-question.

## Red Flags

- Asking 5 questions where one branches the rest (should be single).
- Asking one question when five parallel independent ones would unblock the whole phase (should be batched).
- Open-ended questions when a multiple-choice would do (slows the user down).
- Chained questions after the user has answered once (overwhelming; wait and re-assess).
