# Visual Companion — Decision Pending

**Status:** OPEN — see `spec/ideas/ideate-vs-brainstorming-skills-analysis.md` §11.1.

## Current Recommendation (interim)

Until the visual-companion strategy is decided, `spec-studio:design` prefers lightweight in-artifact visuals:

- **Mermaid** diagrams embedded in `spec/features/<slug>/README.md` (rendered natively by most IDEs and GitHub).
- **ASCII art** for simple flows.
- **Static SVG / PNG** committed to `spec/features/<slug>/assets/` for complex visuals.

## Candidate Approaches (to be decided)

1. **Reuse `obra/superpowers` visual companion directly** — if the user has `superpowers` installed, invoke its browser-based workflow rather than forking/vendoring. Zero maintenance burden on our side. Requires confirming the upstream companion can be triggered standalone.
2. **Vendor the upstream companion** — copy `server.cjs`, `start-server.sh`, `frame-template.html`, `helper.js` into this plugin. Full control; ongoing maintenance.
3. **Clean-room rewrite** — lightweight HTTP server with our own UX conventions. Highest cost; full freedom.
4. **Native platform rendering** — rely on Claude Code / Cursor inline image support for per-turn mockups. No local server. Limited interactivity.
5. **Markdown-only** (current interim) — Mermaid, ASCII, committed SVG. Zero runtime.

## Why This Is Deferred

A browser-based companion is substantially more complex than a markdown-only approach. The right choice depends on how visual the typical SpecScore Feature actually is — a question that needs real usage data. Revisit after the skills have seen a dozen or so Features in the wild.

## Open Sub-Questions

See `spec/ideas/ideate-vs-brainstorming-skills-analysis.md` §11.1 for the full list.
