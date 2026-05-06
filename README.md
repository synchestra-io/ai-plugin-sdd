# Spec Studio

**Spec-driven development, by Synchestra.**

AI skills and Web UI for efficient spec-driven development — the full lifecycle: **specify, plan, build, verify, recap, review, ship.** Spec Studio turns vague ideas into lintable, testable specifications and gates implementation on those specifications being clear, complete, and approved.

## Why a studio

A studio is a workspace where something gets made end-to-end. Spec Studio is the cockpit for working on one project — from raw idea through shipped code, feature by feature — and for keeping the spec and the code honest with each other as both evolve.

Alongside it:

- [**SpecScore**](https://specscore.org/) — the open protocol every spec artifact conforms to.
  - [**Rehearse**](https://rehearse.synchestra.io/) — the markdown-native test framework for SpecScore specs. Studio scaffolds Rehearse test stubs from acceptance criteria.
- [**Synchestra**](https://synchestra.io/) — the engine that runs dispatched work. Headless; you never really "use" it directly.
- [**Synchestra Hub**](https://hub.synchestra.io/) — the portfolio view. When you want to step back from this project and see all your projects and runners, that's Hub.
- [**Synchestra AI Marketplace**](https://github.com/synchestra-io/ai-marketplace) — the plugin marketplace where Spec Studio and related Synchestra plugins are published for Claude Code and compatible clients.

## Install

Spec Studio is published on the [Synchestra AI Marketplace](https://github.com/synchestra-io/ai-marketplace). Install it into Claude Code in two steps:

```
/plugin marketplace add synchestra-io/ai-marketplace
/plugin install spec-studio@synchestra-io
```

The first command registers the marketplace once; the second installs (and later updates) the plugin. Run `/plugin uninstall spec-studio` to remove it.

## What's in the box

Spec Studio ships as a set of Claude Code skills that sit on top of the `specscore` and `synchestra` CLIs. Today:

| Skill | Purpose |
|---|---|
| `spec-studio:ideate` | Refine raw ideas into SpecScore Idea artifacts through structured divergent/convergent thinking. Gates on a lint-clean `spec/ideas/<slug>.md` that the user has approved. |
| `spec-studio:specify` | Turn an approved Idea into a SpecScore Feature with requirements and `Given / When / Then` acceptance criteria at `spec/features/<slug>/`. Gates implementation until the Feature is lint-clean and approved. |

More skills covering the rest of the lifecycle (plan, build, verify, recap, review, ship) are on the roadmap, alongside a web authoring UI at [`specstudio.synchestra.io`](https://specstudio.synchestra.io/).

## Principles

Spec Studio's skills share a common philosophy:

- **Types beat vibes.** If `specscore lint` passes, you can build on it. If it doesn't, you can't.
- **Gates are non-negotiable.** No amount of perceived simplicity bypasses a hard gate. Ideate before specify, specify before plan, plan before code.
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

## Repository family

Spec Studio is a product surface, not a CLI wrapper, so this repo takes the brand name directly (`spec-studio`) — not the `ai-plugin-*` prefix used by sibling plugins like [`ai-plugin-synchestra`](https://github.com/synchestra-io/ai-plugin-synchestra) and [`ai-plugin-specscore`](https://github.com/synchestra-io/ai-plugin-specscore), which wrap separate CLIs. The plugin *is* the product here, following the GitHub Spec Kit precedent (`github/spec-kit`).

Future repos in this family share the `spec-studio-*` stem (e.g., `spec-studio-web`, `spec-studio-api`). The plugin anchors the family.

## Status

**Version 0.0.1 — early.** The `ideate` and `specify` skills are stable enough to use on real work; the rest of the lifecycle is in progress. Expect sharp edges, breaking changes, and active iteration.

## License

MIT. See [`LICENSE`](./LICENSE).
