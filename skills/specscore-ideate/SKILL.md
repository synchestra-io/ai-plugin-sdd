---
name: ideate
description: |
  Refines raw ideas into SpecScore Idea artifacts through structured
  divergent and convergent thinking. Produces a lintable pre-spec
  one-pager at spec/ideas/<slug>.md that can be promoted to one or
  more SpecScore Features. Use when the user has a vague concept and
  isn't ready for design yet.
  Trigger: "ideate", "/ideate", "refine this idea", "stress-test this".
aliases: [ideate]
---

# Ideate

Turn raw ideas into sharp, SpecScore-compatible Idea artifacts through structured divergent and convergent thinking.

## Hard Gate

<HARD-GATE>
Do NOT invoke `spec-studio:design`, `writing-plans`, or any implementation skill until:
  1. An Idea artifact has been written to `spec/ideas/<slug>.md`.
  2. `specscore lint spec/ideas/<slug>.md` passes.
  3. The user has explicitly approved the Recommended Direction.

Ideas that can't be lint-clean aren't ready to be designed.
</HARD-GATE>

## When to Use

- Raw, vague, or unvalidated concept.
- User unsure whether an idea is worth building.
- Multiple possible directions with no clear winner.
- **Skip** when: the user already has an approved Idea or a clear, high-conviction feature to design — go straight to `spec-studio:design`.

## Philosophy

See [philosophy.md](../shared/philosophy.md). Key tenets here: *simplicity is the ultimate sophistication*, *say no to 1,000 things*, *challenge every assumption*, *unsaved ideation is waste*, *prefer stable CLI contracts over ad-hoc file writes when both are possible*.

## Path Conventions

Artifacts land at `spec/ideas/<slug>.md`. See [path-conventions.md](../shared/path-conventions.md). Never use `docs/ideas/`.

## Checklist

Create a task for each and complete in order:

1. **Explore project context** — existing Features, architecture, related Ideas (`Glob`, `Grep`, `Read`).
2. **Scope decomposition check** — if the request describes multiple independent subsystems, stop and help the user split into multiple Ideas before proceeding.
3. **Phase 1 — Understand & Expand** (divergent).
4. **Phase 2 — Evaluate & Converge**.
5. **Phase 3 — Crystallize** as a SpecScore Idea artifact. Prefer the `specscore new idea` CLI scaffold over a direct file write when the CLI is available (see Phase 3 below).
6. **Lint** the artifact: `specscore lint spec/ideas/<slug>.md`.
7. **Inline self-review** — placeholders, contradictions, ambiguity, scope.
8. **User review** — ask the user to review and approve the Recommended Direction.
9. **Emit events** — `idea.drafted` on first write; `idea.approved` after user approval. See [synchestra-events.md](../shared/synchestra-events.md).

## Phase 1 — Understand & Expand (Divergent)

**Goal:** Open the idea up before narrowing.

1. **Restate as a "How Might We…"** sentence. Forces clarity on what's actually being solved.
2. **Ask 3–5 sharpening questions** (batched — see [question-cadence.md](../shared/question-cadence.md)). Focus:
   - Who is this for, specifically?
   - What does success look like?
   - What are the real constraints (time, tech, resources)?
   - What's been tried before?
   - Why now?

   Use `AskUserQuestion`. Do NOT proceed until you know *who* and *success*.

3. **Generate 5–8 variations** using these lenses:
   - **Inversion** — what if we did the opposite?
   - **Constraint removal** — what if budget/time/tech weren't factors?
   - **Audience shift** — what if this were for a different user?
   - **Combination** — what if we merged this with an adjacent idea?
   - **Simplification** — what's the version that's 10x simpler?
   - **10x** — what would this look like at massive scale?
   - **Expert lens** — what would domain experts find obvious?

   Push beyond what the user asked for. See [frameworks.md](references/frameworks.md) for additional frameworks (SCAMPER, JTBD, First Principles, Pre-mortem, Analogous Inspiration). Pick the lens that fits — don't run every framework mechanically.

**If inside a codebase:** ground variations in what actually exists. Reference specific files and patterns.

## Phase 2 — Evaluate & Converge

After the user reacts to Phase 1, shift to convergent mode. Cadence becomes **single question at a time**.

1. **Cluster** ideas that resonated into 2–3 meaningfully different directions.
2. **Stress-test** each direction on three axes — see [refinement-criteria.md](references/refinement-criteria.md):
   - **User value** (painkiller vs. vitamin)
   - **Feasibility** (technical + resource + time-to-value)
   - **Differentiation** (new capability? 10x? new audience? new context? better UX?)
3. **Surface hidden assumptions** in three tiers:
   - **Must be true** (dealbreakers — validate before building)
   - **Should be true** (significant impact, but recoverable)
   - **Might be true** (secondary; don't validate yet)

**Be honest, not supportive.** If a direction is weak, say so with specificity and kindness.

## Phase 3 — Crystallize as a SpecScore Idea

**Prefer the CLI when available.** `specscore new idea <slug>` produces a lint-clean skeleton by construction and updates `spec/ideas/README.md` for you. Subsequent edits must preserve that lint-clean state.

### Step 3a — Detect the CLI

Probe once with Bash:

```bash
command -v specscore >/dev/null 2>&1 && specscore --version
```

If the probe succeeds, take the **CLI path**. Otherwise, take the **fallback path**.

### Step 3b (CLI path) — Scaffold, then fill

1. Invoke `specscore new idea <slug>` with every field you already have. Only these flags exist — do not invent others:

   - `--title`
   - `--owner`
   - `--hmw` (Problem Statement)
   - `--context`
   - `--recommended-direction`
   - `--mvp`
   - `--not-doing` (repeatable; format `<thing> — <reason>`)
   - `--force` (only if overwriting an existing draft at the same slug)
   - `--project` (only if cwd isn't inside the target repo)

   Example:

   ```bash
   specscore new idea my-slug \
     --title "My Idea" \
     --owner "alex" \
     --hmw "How might we …?" \
     --context "Triggered by …" \
     --recommended-direction "We should …" \
     --mvp "A two-week spike that …" \
     --not-doing "Multi-tenant auth — out of scope for MVP" \
     --not-doing "iOS client — web-first"
   ```

   The command writes `spec/ideas/<slug>.md`, runs lint-fix, and exits non-zero if the result isn't lint-clean.

2. **Fill remaining sections with `Edit`.** Sections with no matching flag — `Alternatives Considered`, `Key Assumptions to Validate` (the Must/Should/Might table), `SpecScore Integration`, `Open Questions` — land in the file as HTML-comment prompts. Replace each prompt with real content via `Edit`. Sections you *did* pass via flags are already filled.

### Step 3c (fallback path) — Direct write

If the CLI is not on PATH, write the file directly using the schema below. The content must be identical to what the CLI would produce.

### Step 3d — Lint

In both paths, after the artifact is complete, run `specscore lint spec/ideas/<slug>.md`. The CLI scaffold is lint-clean on generation; your subsequent `Edit`s must not break that.

### Schema (authoritative — used by both paths)

Write to `spec/ideas/<slug>.md` using **exactly this schema**:

```markdown
---
type: idea
id: idea-<slug>
status: Draft
date: YYYY-MM-DD
owner: <author>
promotes_to: []          # Managed by Synchestra. Do NOT edit manually.
supersedes: []
---

# <Idea Name>

## Problem Statement
<One "How Might We…" sentence>

## Context
<Triggering observation, related specs, prior art>

## Recommended Direction
<2–3 paragraphs: what and why, over the alternatives>

## Alternatives Considered
<2–3 directions that lost, and why each lost>

## MVP Scope
<The single job the MVP nails. Timeboxed, not feature-listed. "If it's not embarrassing, you waited too long.">

## Not Doing (and Why)
- <Thing 1> — <reason>
- <Thing 2> — <reason>
- <Thing 3> — <reason>

## Key Assumptions to Validate
| Tier | Assumption | How to validate |
|------|------------|-----------------|
| Must-be-true | … | … |
| Should-be-true | … | … |
| Might-be-true | … | … |

## SpecScore Integration
- **New Features this would create:** <list or "TBD at design time">
- **Existing Features affected:** <list or "none">
- **Dependencies:** <other Ideas or in-flight work>

## Open Questions
- <Question that needs answering before promotion to Feature(s)>
```

**The "Not Doing" list is mandatory** — lint rule `I-002` will fail without it.

## Inline Self-Review

Look at the written artifact with fresh eyes. Check for:

- **Placeholders** — `TBD`, `TODO`, `???`. Fix in place.
- **Internal consistency** — does the Recommended Direction match the MVP Scope? Do the assumptions line up?
- **Scope** — is this one Idea or three smuggled into one?
- **Ambiguity** — could a reader interpret a requirement two ways?

Fix inline. Don't re-review; move on.

## User Review Gate

After lint + self-review pass:

> "Idea drafted and lint-clean at `spec/ideas/<slug>.md`. Please review the Recommended Direction and MVP Scope. Approve to move to design, or request changes."

Wait. If the user requests changes, make them and re-lint. Only proceed once the user approves.

On approval:
- Update `status: Draft → Approved` in the front-matter.
- Re-run lint.
- Emit `idea.approved` event.

## Promotion to Feature(s)

**Out of scope for this skill.** Synchestra handles promotion:

1. When `spec-studio:design` (or the user) creates a Feature with `source_idea: <idea-id>` in its front-matter, Synchestra detects the link.
2. Synchestra transitions the Idea `status: Approved → Specified`.
3. Synchestra auto-populates the Idea's `promotes_to` with the list of Feature IDs.
4. Synchestra emits `idea.specified`.

**Do not manually edit `promotes_to`.** It's managed state.

## Verification

- [ ] Artifact exists at `spec/ideas/<slug>.md`
- [ ] `specscore lint` passes
- [ ] "How Might We" statement, target user, success criteria all explicit
- [ ] ≥2 alternatives stress-tested
- [ ] Assumptions audited across Must/Should/Might tiers
- [ ] "Not Doing" list non-empty
- [ ] User approved the Recommended Direction
- [ ] `status` is `Approved` (if approved) or `Draft` (if not yet)
- [ ] `idea.drafted` / `idea.approved` events emitted

## Red Flags

- Generating 20+ shallow variations vs 5–8 considered ones
- Skipping the "who is this for" question
- No assumptions surfaced before committing to a direction
- Yes-machining weak ideas instead of pushing back
- Empty "Not Doing" list
- Writing to `docs/ideas/` instead of `spec/ideas/`
- Manually editing `promotes_to`
- Jumping to `spec-studio:design` before user approval

## Tone

Direct, thoughtful, slightly provocative. A sharp thinking partner, not a facilitator reading from a script.

## References

- [frameworks.md](references/frameworks.md) — SCAMPER, HMW, First Principles, JTBD, Constraint-Based, Pre-mortem, Analogous Inspiration.
- [refinement-criteria.md](references/refinement-criteria.md) — evaluation rubric for Phase 2.
- [examples.md](references/examples.md) — sample ideation sessions.
- [philosophy.md](../shared/philosophy.md) — shared tenets.
- [path-conventions.md](../shared/path-conventions.md) — `spec/` vs `docs/` rules.
- [specscore-lint-rules.md](../shared/specscore-lint-rules.md) — lint contract this skill assumes.
- [synchestra-events.md](../shared/synchestra-events.md) — event payloads emitted by this skill.
- [question-cadence.md](../shared/question-cadence.md) — when to batch vs single-question.
- `specscore new idea <slug>` — CLI scaffolder used in Phase 3 (see `internal/cli/new.go` in the specscore repo for the authoritative flag list).
