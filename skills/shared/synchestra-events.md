# Synchestra Events Emitted by SDD Skills

**Status:** Contract

Events are how SDD skills hand work off to Synchestra and vice-versa. Both `spec-studio:ideate` and `spec-studio:specify` emit events after successful artifact writes. Synchestra can also trigger skills in response to events.

## Event Envelope (common fields)

Every event carries this envelope:

```yaml
event: <event-name>
version: 1
timestamp: <ISO-8601>
actor:
  kind: skill | user | synchestra | external
  id: <e.g., "skill:spec-studio:ideate", "user:<username>", "agent:<agent-id>">
artifact:
  type: idea | feature | plan
  id: <stable SpecScore ID>
  path: <path relative to repo root>
  revision: <git SHA at time of emission>
payload:
  # event-specific; see below
```

## Emission Transport

- **Default:** Append-only JSONL to `.synchestra/events.jsonl` at repo root (git-ignored).
- **Hook:** Skills invoke `synchestra emit <event.yaml>` (CLI) when available; fall back to direct file append otherwise.
- **Idempotency:** Each event includes a `uuid` (assigned by emitter). Synchestra dedupes by uuid.

## Events Emitted by `spec-studio:ideate`

### `idea.drafted`
Fired after every successful `specscore lint` pass following a write or edit, while the Idea's front-matter `status` is `Draft`. The first emission carries the same event name as subsequent ones — Synchestra dedupes by event uuid.

```yaml
payload:
  slug: <slug>
  hmw: <How Might We statement>
  target_user: <string>
  approved: false
  changed_sections: [<H2 section name>, ...] | null   # null on first emission (no baseline)
  previous_revision: <git SHA> | null                  # null on first emission
  change_summary: <string ≤2 sentences> | null         # null on first emission
```

**Change-context fields** (`changed_sections`, `previous_revision`, `change_summary`) carry the diff between this emission and the previous one. They are `null` on the first emission for an Idea (no prior revision to diff against). On every subsequent emission they are present and non-null.

`changed_sections` lists the H2 section names whose content differs from `previous_revision`. `change_summary` is a ≤2-sentence **factual** description of the change (no speculation about the user's intent or motivation; no editorializing).

### `idea.approved`
Fired exactly once, after the user explicitly approves the Recommended Direction and the Idea's `status` transitions to `Approved`.

```yaml
payload:
  slug: <slug>
  recommended_direction_summary: <first paragraph>
```

**Consumer:** Synchestra may react by scheduling `spec-studio:specify` (after user confirmation) or by notifying watchers.

### `idea.updated`
Fired after every successful `specscore lint` pass following a write or edit, while the Idea's front-matter `status` is `Approved`. Distinguishes post-approval iteration from pre-approval drafting; consumers that watch only for material changes to approved Ideas subscribe here rather than to `idea.drafted`.

```yaml
payload:
  slug: <slug>
  hmw: <How Might We statement>
  target_user: <string>
  approved: true
  changed_sections: [<H2 section name>, ...]   # always non-null (an updated event by definition has a baseline)
  previous_revision: <git SHA>                  # always non-null
  change_summary: <string ≤2 sentences>         # always non-null; same discipline as idea.drafted
```

The change-context fields follow the same semantics and discipline as on `idea.drafted` (see above), but are never `null` here — by definition, an `idea.updated` emission has a prior revision to diff against (the revision at which `idea.approved` last fired, or the previous `idea.updated`).

**Consumer:** Synchestra may notify Features that declare this Idea as a `Source Ideas` entry, so downstream specs can be re-reconciled. Consumers can filter on `changed_sections` to react only when load-bearing sections (e.g., `Recommended Direction`) change.

## Events Emitted by `spec-studio:specify`

### `feature.specified`
Fired after the Feature artifact is written, lint-clean, and the reviewer subagent returns `Approved`.

```yaml
payload:
  slug: <slug>
  source_idea_id: <idea id | null>
  requirement_count: <int>
  ac_count: <int>
  rehearse_stubs_generated: <bool>
  rehearse_skip_reason: <string | null>
```

### `feature.approved`
Fired after the user explicitly approves the written Feature.

```yaml
payload:
  slug: <slug>
```

**Consumer:** Synchestra typically triggers `writing-plans` next.

## Events Emitted by Synchestra (consumed by skills)

### `idea.specified`
Fired by Synchestra (not by a skill) when one or more Features are created whose `type: feature` front-matter references an Idea's id in a `source_idea:` field. Synchestra:

1. Transitions the Idea's `status: Approved → Specified`.
2. Populates the Idea's `promotes_to` with the list of Feature IDs.
3. Commits the updated Idea artifact.
4. Emits `idea.specified`.

```yaml
payload:
  idea_slug: <slug>
  feature_ids: [<feat-1>, <feat-2>, …]
```

**No skill emits this event directly.** Skill authors must not manually edit `promotes_to`.

## Event Schema Versioning

- Envelope `version` starts at 1.
- Additive changes (new payload fields) do not bump version.
- Breaking changes (renamed/removed fields) bump version; consumers must handle both until the old version is removed.

## Reference: Full Event Names

| Event | Emitter | Trigger |
|---|---|---|
| `idea.drafted` | `spec-studio:ideate` | Every successful lint pass while `status: Draft` |
| `idea.approved` | `spec-studio:ideate` | User approves Recommended Direction (exactly once) |
| `idea.updated` | `spec-studio:ideate` | Every successful lint pass while `status: Approved` |
| `idea.specified` | synchestra | Feature(s) created from an approved Idea |
| `feature.specified` | `spec-studio:specify` | Reviewer-approved, lint-clean Feature write |
| `feature.approved` | `spec-studio:specify` | User approves the written Feature |
