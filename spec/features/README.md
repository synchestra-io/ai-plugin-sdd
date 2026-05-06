# Spec Studio Features

> [View in Synchestra Hub](https://hub.synchestra.io/project/features?id=spec-studio@synchestra-io@github.com&path=spec%2Ffeatures) — graph, discussions, approvals

Feature specifications for the Spec Studio plugin. This index lists every top-level SpecScore Feature in this repository.

## Contents

| Feature | Status | Description |
|---------|--------|-------------|
| [`skills/`](skills/README.md) | Draft | Umbrella feature for the per-skill sub-features that specify each Claude Code skill's purpose, gates, inputs, outputs, and lifecycle position. |

### skills

The `skills` feature groups one sub-feature per Claude Code skill in the plugin. Today it covers `ideate`, `specify`, `plan`, `build`, `verify`, `recap`, `review`, and `ship` — all `Draft` until each is refined via `spec-studio:ideate` and promoted via `spec-studio:specify`. Implementation maturity (which skills actually ship today) is tracked separately in [`skills/README.md`](../../skills/README.md).

## Outstanding Questions

- None at this time.

---
*This document follows the https://specscore.md/features-index-specification*
