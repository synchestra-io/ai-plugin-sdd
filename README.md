# Spec Studio

**Spec Studio — spec-driven development, by Synchestra.**

AI skills for spec-driven development: specify, plan, build, verify, review, ship.

## Naming

### Product name — why "Spec Studio"

- **Differentiates upward from GitHub Spec Kit.** "Studio" reads as *environment / full workspace*; "Kit" reads as *onboarding toolkit*. Spec Studio sits a category above.
- **Dev-familiar pattern.** "X Studio" is well-understood in developer tooling (Android Studio, Visual Studio, R Studio) — zero teaching cost on first read.
- **Ecosystem metaphor.** "Studio" carries a second reading inside the Synchestra musical family — the *recording studio* where a composition is made. This binds the product line into a coherent three-act workflow: *compose* in Spec Studio → *score* with SpecScore → *perform* with Synchestra.

### Repository name — why `spec-studio` (not `ai-plugin-spec-studio`)

Sibling plugins in this workspace use the `ai-plugin-` prefix (`ai-plugin-synchestra`, `ai-plugin-specscore`) because they wrap *separate* core products (`synchestra/`, `specscore/`) where the prefix distinguishes plugin-form from core-form. Spec Studio has no separate core — it *is* the high-level skills plugin that orchestrates `synchestra-cli` and `specscore-cli`. Following the GitHub Spec Kit precedent (`github/spec-kit`, not `github/ai-plugin-spec-kit`), when the plugin *is* the product, the repo takes the brand name directly.

Full decision record and rejected alternatives: `synchestra-io/synchestra-marketing/decisions/2026-04-19-spec-studio-naming.md`
