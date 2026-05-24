# Project Wiki: spec-kit (Lemonade fork)

> **Agent onboarding:** Read this file first. It tells you what the project is, how to run it, and what's in flight. You should not need to guess.

## Mission
Spec-kit is GitHub's Spec-Driven Development (SDD) toolkit. In the lemonade stack it serves as the **specification harness**: every feature across lemonade-store, lemonade-cashier, and lemonade-mobile starts with a spec-kit spec before any code is written. The `specify` CLI transforms a product scenario into an executable spec that drives implementation.

## Architecture
- **specify CLI**: `uv tool install specify-cli` — the primary tool. Runs via `uv run specify` after install.
- **Slash commands**: `/speckit.constitution`, `/speckit.specify`, `/speckit.tasks` — used inside Claude Code sessions.
- **Presets**: `presets/` — reusable scenario templates for common lemonade patterns (POS flows, event sourcing, agent delegation).
- **Templates**: `templates/` — YAML scenario templates. Each scenario becomes an executable spec.
- **Extensions**: `extensions/` — custom validations for lemonade-specific invariants (Decimal money, event log, agent tiers).
- **Workflows**: `workflows/` — CI automation that validates specs against running implementations.

## How to Use (Agent Handoff)
```bash
# Install
uv tool install specify-cli --from git+https://github.com/bong-water-water-bong/spec-kit.git

# Start a new feature spec in any lemonade repo
cd /home/bcloud/lemonade-cashier
/speckit.constitution   # establish governing principles (once per project)
/speckit.specify        # describe what you're building → generates .spec/ directory
/speckit.tasks          # generate implementation task list from spec
```

**Hot paths**:
- `src/specify/` — core CLI logic
- `templates/` — scenario templates; add new ones here for reuse
- `presets/` — quick-start presets; `lemonade-pos.yaml` is the baseline for all cashier specs

## Current State & Priorities
- Fork of `github/spec-kit` — upstream changes are merged periodically
- Custom lemonade presets live in `presets/lemonade/`
- The `extensions/lemonade/` folder holds Decimal-money and event-log validators
- CI: `ci.yml` runs spec validation on every PR

## Gotchas
- `specify` requires Python 3.11+ and `uv`. If `uv` is not installed: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Spec files (`.spec/`) should be committed — they are the source of truth for what a feature should do
- When writing presets, keep them generic enough to reuse across lemonade sub-repos

## Related
- [[OpenSpec]] — alternative artifact-guided workflow
- [[lemonade-cashier]] — primary consumer of spec-kit specs
