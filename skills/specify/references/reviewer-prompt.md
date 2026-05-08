# Spec Document Reviewer Prompt Template

**Status:** Adapted from `obra/superpowers/skills/brainstorming/spec-document-reviewer-prompt.md` for SpecScore Feature artifacts.

Use this template when dispatching a spec document reviewer subagent from `specstudio:specify`. Purpose: verify the Feature is complete, consistent, and ready for `writing-plans`.

**Dispatch after:** Feature artifact is written, lint passes, and inline self-review is done.

## Subagent Invocation

```
Agent tool (subagent_type: general-purpose):
  description: "Review SpecScore Feature"
  prompt: |
    You are a SpecScore Feature reviewer. Verify the Feature at
    <FEATURE_DIR> is complete and ready for writing-plans.

    **Feature directory:** spec/features/<slug>/
    **Source Idea (if any):** spec/ideas/<idea-slug>.md

    ## What to Check

    | Category | What to Look For |
    |----------|------------------|
    | Completeness | TBD, TODO, placeholders, incomplete sections in README or requirements |
    | Schema | Every requirement has ≥1 AC; every AC is Given/When/Then |
    | Consistency | Architecture matches feature descriptions; requirements align with ACs; no internal contradictions |
    | Clarity | Requirements unambiguous — a reader would build the same thing |
    | Scope | Single plan's worth of work — not multiple independent subsystems |
    | YAGNI | No unrequested features; no over-engineering |
    | Assumption carryover | If source Idea exists, its Must-be-true assumptions are either addressed by ACs or explicitly deferred |
    | Rehearse integration | Either stubs exist for testable ACs, OR a skip-reason is recorded |
    | Body metadata | Title `# Feature: <name>`; `**Status:**`, `**Date:**`, `**Owner:**` present in that order immediately after title; `**Source Ideas:**` and `**Supersedes:**` present (value `—` if empty); when `**Source Ideas:**` is non-empty, each referenced slug resolves to a real Idea |

    ## Calibration

    Only flag issues that would cause real problems during planning or
    implementation. A genuinely ambiguous requirement, a missing AC, a
    Given/When/Then violation, a scope that spans subsystems — those are
    issues. Minor wording, stylistic preferences, or uneven section depth
    are not.

    Approve unless there are serious gaps that would lead to a flawed plan
    or an incorrect implementation.

    ## Output Format

    ## Feature Review

    **Status:** Approved | Issues Found

    **Issues (if any):**
    - [File:Section]: [specific issue] — [why it matters for planning or implementation]

    **Recommendations (advisory, do not block approval):**
    - [suggestions]
```

**Reviewer returns:** Status, Issues (if any), Recommendations.

**Caller behavior:**
- `Approved` → proceed to user review gate.
- `Issues Found` → fix each issue inline, re-run lint, re-dispatch reviewer.
- Recommendations → author's judgment; never blocks approval.
