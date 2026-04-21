# SpecScore Lint Rules Assumed by SDD Skills

**Status:** Contract (skills depend on these; changes require a coordinated update)

Both `spec-studio:ideate` and `spec-studio:specify` assume `specscore lint` enforces the rules below. Skills invoke lint as a verification step — if any rule here changes, the corresponding skill checklist step may need to update.

## Lint CLI Contract

- `specscore lint <file>` — single-file mode. Returns 0 on pass, non-zero on fail; machine-readable output on stderr (`--json` flag).
- `specscore lint` (no arg) — whole-tree mode, scans `spec/**`.
- Dispatches per-artifact rule sets based on `type:` field in YAML front-matter.

## Universal Rules (all SpecScore artifacts)

| ID | Rule | Severity |
|---|---|---|
| U-001 | File must have YAML front-matter | error |
| U-002 | Front-matter must include `type`, `id`, `status`, `date` | error |
| U-003 | `id` must match filename/slug pattern | error |
| U-004 | `status` must be a valid value for the artifact's `type` | error |
| U-005 | No unresolved placeholders (`TBD`, `TODO`, `???`, `FIXME`) in required sections | error |
| U-006 | No broken cross-references (`promotes_to`, `supersedes`, `depends_on`, `requirement_of`) | error |
| U-007 | File location matches canonical path for `type` (see `path-conventions.md`) | error |
| U-008 | `date` is ISO-8601 and not in the future | warn |

## Idea-Specific Rules (`type: idea`)

| ID | Rule | Severity |
|---|---|---|
| I-001 | Required sections present: `Problem Statement`, `Recommended Direction`, `MVP Scope`, `Not Doing`, `Key Assumptions to Validate` | error |
| I-002 | `Not Doing` section is non-empty (explicit exclusions required) | error |
| I-003 | `Key Assumptions to Validate` lists at least one Must-be-true assumption | error |
| I-004 | `Problem Statement` contains exactly one "How Might We" framing | warn |
| I-005 | `status` ∈ {`Draft`, `Under Review`, `Approved`, `Specified`, `Archived`} | error |
| I-006 | If `status: Specified`, `promotes_to` must be non-empty | error |
| I-007 | If `status ∈ {Draft, Under Review, Approved}`, `promotes_to` may be empty | info |

## Feature-Specific Rules (`type: feature`)

| ID | Rule | Severity |
|---|---|---|
| F-001 | `README.md` present at `spec/features/<slug>/README.md` | error |
| F-002 | At least one requirement file in `spec/features/<slug>/requirements/` | error |
| F-003 | Each requirement has at least one acceptance criterion | error |
| F-004 | Each acceptance criterion uses `Given / When / Then` format | error |
| F-005 | `status` ∈ {`Draft`, `Approved`, `In Progress`, `Shipped`, `Archived`} | error |
| F-006 | If `supersedes` present, referenced Feature exists and has `status: Archived` | error |
| F-007 | Architecture section present when Feature is non-trivial | warn |

## Plan-Specific Rules (`type: plan`)

Out of scope for this document — defined by planning skills.

## What Skills Do on Lint Failure

1. **Read lint output** (`specscore lint --json <file>`).
2. **If fixable inline** (missing section, placeholder, front-matter typo), fix and re-run.
3. **If requires user input** (ambiguous requirement, missing assumption), surface to user with the specific lint violation.
4. **Do not bypass lint.** Both skills treat a passing `specscore lint` as a precondition for user-review and event emission.

## Versioning

Lint rule IDs are stable. Adding rules is backward-compatible; removing or renaming requires a skill update.
