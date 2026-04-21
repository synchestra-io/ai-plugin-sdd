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
Fired after the Idea artifact is first written and passes lint.

```yaml
payload:
  slug: <slug>
  hmw: <How Might We statement>
  target_user: <string>
  approved: false
```

### `idea.approved`
Fired after the user explicitly approves the Recommended Direction and the Idea's `status` transitions to `Approved`.

```yaml
payload:
  slug: <slug>
  recommended_direction_summary: <first paragraph>
```

**Consumer:** Synchestra may react by scheduling `spec-studio:specify` (after user confirmation) or by notifying watchers.

## Events Emitted by `spec-studio:specify`

### `feature.designed`
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
| `idea.drafted` | `spec-studio:ideate` | First lint-clean write |
| `idea.approved` | `spec-studio:ideate` | User approves Recommended Direction |
| `idea.specified` | synchestra | Feature(s) created from an approved Idea |
| `feature.designed` | `spec-studio:specify` | Reviewer-approved, lint-clean Feature write |
| `feature.approved` | `spec-studio:specify` | User approves the written Feature |
