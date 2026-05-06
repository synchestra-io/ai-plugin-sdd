# Scenario: Lint failure triggers one --fix pass, then surfaces remaining violations

**Validates:** [ideate#ac:lint-failure-recovery](../README.md#ac-lint-failure-recovery)

## Steps

GIVEN the skill has just written or edited `spec/ideas/my-idea.md`
AND `specscore lint spec/ideas/my-idea.md` exits non-zero (e.g., trailing whitespace + missing terminal newline — both auto-fixable)
WHEN the skill processes the lint failure
THEN the skill runs `specscore lint --fix spec/ideas/my-idea.md` exactly once
AND the skill re-runs `specscore lint spec/ideas/my-idea.md`
AND lint now exits zero
AND the skill tells the user what was auto-fixed
AND the skill continues to the next workflow step

GIVEN the skill has just written or edited the artifact
AND `specscore lint` exits non-zero with a violation that `--fix` cannot repair (e.g., `I-005` placeholder leak)
WHEN the skill processes the lint failure
THEN the skill runs `specscore lint --fix` once
AND the skill re-runs `specscore lint`
AND lint still exits non-zero
AND the skill MUST NOT run `--fix` a second time
AND the skill surfaces the remaining violations to the user with rule IDs and affected sections

GIVEN the skill has just written or edited the artifact
AND `specscore lint` reports a violation of `I-002` (Not Doing required) or `I-003` (Must-be-true assumption required)
WHEN the skill processes the lint failure
THEN the skill MUST NOT auto-fix the violation, even if a future `--fix` would attempt it
AND the skill surfaces the violation to the user immediately

---
*This document follows the https://specscore.md/scenario-specification*
