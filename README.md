# Spec Studio

**Spec-driven development, by Synchestra.**

AI skills and Web UI for efficient spec-driven development — the full lifecycle: **specify, plan, build, verify, recap, review, ship.** Spec Studio turns vague ideas into lintable, testable specifications and gates implementation on those specifications being clear, complete, and approved.

## Why a studio

A studio is a workspace where something gets made end-to-end. Spec Studio is the cockpit for working on one project — from raw idea through shipped code, feature by feature — and for keeping the spec and the code honest with each other as both evolve.

Alongside it:

- [**SpecScore**](https://specscore.org/) — the open protocol every spec artifact conforms to.
- [**Synchestra**](https://synchestra.io/) — the engine that runs dispatched work. Headless; you never really "use" it directly.
- [**Synchestra Hub**](https://hub.synchestra.io/) — the portfolio view. When you want to step back from this project and see all your projects and runners, that's Hub.

## What's in the box

Spec Studio ships as a set of Claude Code skills that sit on top of the `specscore` and `synchestra` CLIs. Today:

| Skill | Purpose |
|---|---|
| `specscore:ideate` | Refine raw ideas into SpecScore Idea artifacts through structured divergent/convergent thinking. Gates on a lint-clean `spec/ideas/<slug>.md` that the user has approved. |
| `specscore:design` | Turn an approved Idea into a SpecScore Feature with requirements and `Given / When / Then` acceptance criteria at `spec/features/<slug>/`. Gates implementation until the Feature is lint-clean and approved. |

More skills covering the rest of the lifecycle (plan, build, verify, recap, review, ship) are on the roadmap, alongside a web authoring UI that will grow inside [Synchestra Hub](https://hub.synchestra.io/).

## Principles

Spec Studio's skills share a common philosophy:

- **Types beat vibes.** If `specscore lint` passes, you can build on it. If it doesn't, you can't.
- **Gates are non-negotiable.** No amount of perceived simplicity bypasses a hard gate. Ideate before design, design before plan, plan before code.
- **Unsaved ideation is waste.** If it's worth thinking about, it's worth a lint-clean artifact in `spec/`.
- **Say no to 1,000 things.** The "Not Doing" list is the most valuable part of any artifact.
- **Be honest, not supportive.** Skills push back on weak ideas with specificity and kindness. No yes-machines.

See [`skills/shared/philosophy.md`](./skills/shared/philosophy.md) for the full set.

## Where it fits

| | What it does | Layer |
|---|---|---|
| [SpecScore](https://specscore.org/) | The protocol: feature/requirement/AC format, lint, LSP | Open source |
| [Synchestra](https://synchestra.io/) | The engine: CLI, daemon, runners | Open source |
| **Spec Studio** | Work on one project end-to-end, including spec↔code coherence — AI skills in your IDE today, web authoring UI on the way | **Open source** |
| [Synchestra Hub](https://hub.synchestra.io/) | Portfolio + runtime UI for remote execution | Hosted |

Spec Studio works standalone with Claude Code. Paired with Synchestra Hub, the same skills can dispatch long-running or sandboxed work to remote runners without leaving your editor.

## Status

**Version 0.0.1 — early.** The `ideate` and `design` skills are stable enough to use on real work; the rest of the lifecycle is in progress. Expect sharp edges, breaking changes, and active iteration.

## License

MIT. See [`LICENSE`](./LICENSE).
