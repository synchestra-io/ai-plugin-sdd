# SpecStudio — Specifications

> [View in SpecStudio](https://specstudio.synchestra.io/project/features?id=specstudio-skills@synchestra-io@github.com&path=spec) — graph, discussions, approvals

This directory contains the SpecScore-formatted specifications that drive SpecStudio's own development. SpecStudio dogfoods its own format and skills: every feature here was authored with `specstudio:ideate` and `specstudio:specify`, every artifact lints clean against `specscore`.

## Contents

| Directory | Purpose |
|---|---|
| [`features/`](features/README.md) | Feature specifications — one per skill or sub-system, with requirements and Given/When/Then acceptance criteria |
| [`ideas/`](ideas/README.md) | Pre-spec one-pagers exploring problem-direction-MVP before promotion to a Feature |
| [`research/`](research/README.md) | Long-form analysis and comparative reasoning that informs design decisions |

## Conventions

All artifacts under `spec/` follow [SpecScore](https://specscore.org/) conventions and lint clean against `specscore spec lint`. Where the layout prescribed by `specstudio:specify` conflicts with `specscore` lint rules, **SpecScore wins**.

## Outstanding Questions

None at this time.

---
*This document follows the https://specscore.md/feature-specification*
