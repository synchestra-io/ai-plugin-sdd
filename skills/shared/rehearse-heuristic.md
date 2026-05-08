# Rehearse Test-Stub Heuristic

**Status:** Draft (v0.1) — refine with real Feature examples
**Used by:** `specstudio:specify` (checklist step: Rehearse stub generation)

Rehearse test-stub generation is **optional**. `specstudio:specify` decides whether to scaffold stubs based on this heuristic. The user can always override.

## The Decision

After the Feature's acceptance criteria are drafted, the skill asks for each AC:

> "Does this AC describe an observable outcome that a Rehearse script can automate?"

If the answer is yes for **any** AC, the skill scaffolds stub Scenario files (one per testable AC) following the canonical SpecScore Scenario format. The "pending" state is signaled by a `## TODO` checklist in the body — not by a status field, since canonical Scenarios do not carry one.

If the answer is no for all ACs, the skill skips stub generation and records in the Feature's `README.md`:

```markdown
## Rehearse Integration
No stubs generated — reason: <one sentence>
```

## Signals: AC Is Likely Testable by Rehearse

At least one of these should be true:

- **CLI surface.** AC mentions a command, flag, exit code, or stdout/stderr output.
- **HTTP surface.** AC mentions a route, method, status code, request/response shape.
- **Pure function surface.** AC describes an input-to-output mapping with no hidden state.
- **Data surface.** AC describes a row/record that should exist (or not) after an action.
- **UI with stable selector.** AC references a specific element by test-id, role, or accessible name.
- **File-system surface.** AC describes a file created/modified/deleted with known path and content.
- **Event surface.** AC describes an event emitted on a known channel with a known payload shape.

## Signals: AC Is NOT Testable by Rehearse

Any of these makes automation impractical:

- **Subjective outcome.** "Users find it intuitive," "feels fast," "looks professional."
- **Undefined observer.** "The system should handle this gracefully" without defining graceful.
- **Abstract behavior.** "The design is modular" — architectural; not behavioral.
- **Third-party we can't inspect.** "When the LLM returns a reasonable answer" — nondeterministic + opaque.
- **Time-scale mismatch.** "Over a week, engagement improves" — integration test scope, not Rehearse.
- **Documentation-only Feature.** The Feature is a doc, an ADR, or a convention — no runtime.

## Stub File Format

One file per testable AC, placed at:

```
spec/features/<slug>/_tests/<requirement-slug>-<ac-slug>.md
```

Stub body (canonical SpecScore Scenario form):

```markdown
# Scenario: <AC name>

**Validates:** [<feature-slug>#req:<requirement-slug>](../README.md#req-<requirement-slug>)

## Steps

GIVEN <precondition>
WHEN <action>
THEN <observable outcome>

## Detected Surface

<cli | http | pure-function | data | ui | fs | event>

## TODO

- [ ] Pick Rehearse driver
- [ ] Wire up fixtures
- [ ] Implement assertion

---
*This document follows the https://specscore.md/scenario-specification*
```

## Tuning

- Track false positives (stubs generated for ACs that turned out to be untestable) and false negatives (ACs skipped that should have had stubs).
- Revisit heuristic quarterly or when the error rate exceeds ~20%.
