# AgentMindCloud Ecosystem Redesign — Decisions Lock-in

**Date:** 2026-05-11
**Status:** Locked-in for v1.0. Amendments require an explicit commit titled `amend(decisions): <change>`.
**Supersedes:** the architecture sketches in earlier conversations. PORTFOLIO-MAP.md's findings drive the architecture.

## D1 — Two-repo architecture

The portfolio consolidates into two fresh repos:

| Repo | Role | Audience |
|---|---|---|
| AgentMindCloud/grok-install-v2 → renamed to grok-install at launch | The unified ecosystem: spec, standards, CLI, action, IDE extension, brand, docs, ~41 agents (8 simple + 22 creator + 4 finance + 7 super), 4 LIVE HTML tools, marketplace, safety scanner | Everyone — consumers, developers, advanced creators |
| AgentMindCloud/xlOS | Experimental sibling. Initially a focused placeholder for: multi-agent orchestration, debate runtimes, the Lucas-veto safety pattern, live YAML playground, voice/swarm research surfaces | Researchers, advanced builders, future power-users |

The old "consumer vs developer" split is dropped. grok-install-v2 is the stable thing. xlOS is the lab.

## D2 — Resolution of Contradiction 1 (xlOS scope sources)

PORTFOLIO-MAP.md's summary mentioned xlOS getting multi-agent orchestration + Lucas veto + live YAML playground, but the per-repo dispositions marked the source repos as ARCHIVE_NO_MIGRATION or MIGRATE_TO_GROK_INSTALL_V2 — no path for the content to actually reach xlOS.

Lock-in:

| Source repo | Per PORTFOLIO-MAP.md | Amended for xlOS scope |
|---|---|---|
| grok-agent-orchestra | ARCHIVE_NO_MIGRATION | UPGRADE TO ARCHIVE_AFTER_GEM_EXTRACTION → xlOS. Specifically: the 5 orchestration patterns, the Lucas veto, the xAI native + simulated debate runtimes. Code copied to xlOS/orchestration/. |
| grok-build-bridge | MIGRATE_TO_GROK_INSTALL_V2 | SPLIT: pipeline + safety audit + deploy targets → grok-install-v2; the Lucas-veto-specific code → xlOS. Document which files go which way in the orchestra migration phase. |
| grok-docs | MIGRATE_TO_GROK_INSTALL_V2 | SPLIT: docs site stays grok-install-v2; the client-side YAML playground component → xlOS/playground/ (or shared between both via a small symlinked package). |

If at gem-extraction time it becomes clear that the orchestra code is too entangled with its own infrastructure to extract cleanly, xlOS v1.0 ships as a placeholder repo (only README + LICENSE) and the orchestration migration becomes a v1.1 effort. Document that fallback explicitly in xlOS's README.

## D3 — Resolution of Contradiction 2 (gem inventory only names xlOS once)

PORTFOLIO-MAP.md only mentioned xlOS as a target once (for grok-paradoxes). Per D2 above, augment the gem inventory:

| Gem | Source | Target |
|---|---|---|
| packages/grok-paradoxes/ | grok-agent | grok-install-v2 (primary) + reference from xlOS |
| Orchestration framework code | grok-agent-orchestra | xlOS/orchestration/ |
| Lucas-veto pattern | grok-build-bridge | xlOS/safety/lucas-veto/ |
| Debate runtimes | grok-agent-orchestra | xlOS/runtimes/ |
| Live YAML playground | grok-docs | xlOS/playground/ |

The full gem-to-target mapping lives in PORTFOLIO-MAP.md; this section only documents the divergences from it.

## D4 — Repo names and final swap

| Decision | Choice |
|---|---|
| Working name for the new big repo | grok-install-v2 |
| Final name after swap | grok-install (the legacy repo renamed to grok-install-legacy first) |
| New experimental sibling | xlOS (exact case — capital O, capital S) |

## D5 — Schema version regime

PORTFOLIO-MAP.md flagged that vscode-grok-yaml ships three incompatible schema-version regimes (apiVersion: const, apiVersion: enum, spec_version). This must be resolved upstream before v2's IDE extension is coherent.

| Decision | Choice |
|---|---|
| Canonical version field | `version: "2.14"` (string, exact match) — the form already used by grok-install templates and grok-yaml-standards |
| Other regimes | Deprecated. The v2 IDE extension recognizes only this form |
| Rich-feature support | Optional `extensions:` block, backward-compat with v2.14 parsers (unknown top-level keys ignored) |
| v2.15 RFC | Deferred post-launch |

`extensions:` block fields:
- `extensions.constitution` — list of article references
- `extensions.multi_agent_roles` — for super-agents that coordinate
- `extensions.provenance` — citation/confidence config
- `extensions.demo_metadata` — video/storyboard URLs
- `extensions.x_money_specific` — cost caps + tax disclaimer config

## D6 — Super Agent renaming

The names "Living Narrative Fabric", "Self-Evolving Personal OS", "Cross-Reality Action Fabric" read as AI word-salad on top of legitimate code. Rename to literal descriptors:

| Old name | New name | Slug |
|---|---|---|
| Living Narrative Fabric | Multi-source narrative synthesizer | narrative-synthesizer |
| Self-Evolving Personal OS | Personal-context briefing agent | personal-briefing |
| Cross-Reality Action Fabric | Windows + web action runner | action-runner |
| Agent Swarm With Shared Memory | (keep — already literal) | agent-swarm |
| Provenance-First Trust Engine | (keep — already literal) | provenance-trust |
| Narrative Contradiction Detector | (keep — already literal) | contradiction-detector |
| Zero-Config "I Want To" Agent | Plain-language goal agent | goal-agent |

Filenames, slugs, manifest names, marketplace categories all use the new slugs.

## D7 — PyPI package strategy

| Decision | Choice |
|---|---|
| Main CLI publishes as | grok-install (the existing PyPI name; v0.2.0 → v1.0.0) |
| grok-install legacy PyPI | Continues; v1.0.0 onwards is the new clean code |
| xlOS PyPI package | xlos (lowercase, PyPI-normalized) |

## D8 — Cross-platform targets

| Decision | Choice |
|---|---|
| OS support | Windows + macOS + Linux, all CI-tested for grok-install-v2 CLI |
| PowerShell CLI from grok-agent | Used as the IMPLEMENTATION REFERENCE. The new CLI is Python (Click + platformdirs + filelock + rich + colorama). PowerShell version not committed to the new repo. |
| Mobile | Browser flow only |
| Python version floor | 3.11 |
| Build tool | uv |
| Lint/format/type | ruff + black + mypy |
| User-data path | platformdirs |

## D9 — Visual identity

| Decision | Choice |
|---|---|
| Brand source | Distill best of grok-install-brand into ONE design system, shared between both new repos |
| README banner | ONE per repo, in assets/banner.svg, <200KB |
| capsule-render banners | BANNED |
| typing-svg headers | BANNED |
| Status emoji rows in user-facing tables (done, active, heart) | BANNED |
| "Phase N" / "Tier N" / "Spectral" terminology | BANNED in user-facing files |

## D10 — Marketplace strategy

| Decision | Choice |
|---|---|
| Marketplace location | grok-install-v2/marketplace/ |
| Source | Folded from grok-agents-marketplace (Next.js 15 — already production-grade) |
| grok-agent's internal marketplace | DROPPED (inferior implementation) |
| grok-agents-marketplace fate | ARCHIVE after fold-in |

## D11 — v1.0 launch scope

In scope:
- Cross-platform CLI on all 3 OSes
- ~41 agents migrated to v2.14 + extensions: blocks
- Marketplace folded in
- Constitution scanner as plugin
- Standard v2.14 docs site
- ONE clean README per repo
- GitHub Pages + Cloudflare Worker for grok-install-v2
- xlOS as either (a) functional v1.0 if orchestra extraction succeeds, or (b) honest placeholder if not

Deferred:
- Mobile native apps
- pulse/ MCP server beyond pulse_today
- eval-delta workflows
- Real-time analytics dashboards
- v2.15 RFC
- Multiple language ports

## D12 — Pre-build quick wins (Phase 0.6) — COMPLETED 2026-05-11

All 7 PRs landed on legacy-repo mains before Phase 1 architecture starts.

| # | Repo | PR | Summary |
|---|---|---|---|
| QW1 + QW3a | grok-build-bridge | #6 (sha 14c6575) | Removed entire fabricated `#status` block from docs/index.html (1,284 active agents, 99.7% safety pass rate, 312 deployments, 99.4% dry-run success, 47 safety blocks, 15,623 runs, RNG-seeded live feed) + moved MERGE_PLAN.md, AUDIT-REPORT.md to docs/internal/ |
| QW2 | grok-agent | #9 | Moved CLAUDE.md, HANDOFF_LOG.md, MERGE_PLAN.md, ROADMAP.md to docs/internal/. README references repointed. |
| QW3b + QW5 | x-platform-toolkit | #3 | Moved AUDIT-REPORT.md to docs/internal/. Unified README + scripts/assert-tool-status.mjs to actual count of 12 tools (was claiming 20). 9 README references updated, 8 stale TOOL_STATUS entries removed. |
| QW3c | grok-agents-marketplace | #24 (sha 12d7496) | Moved MERGE_PLAN.md to docs/internal/. |
| QW3d | grok-install-action | merged | Moved AUDIT-REPORT.md to docs/internal/. |
| QW3e | grok-install | #15 (sha 6f148a7) | Moved MERGE_PLAN.md to docs/internal/. |
| QW4 | grok-install-brand | #1 (sha 5d1feda) | LICENSE.md replaced with canonical Apache 2.0 text from apache.org. NOTICE file added with copyright 2026 AgentMindCloud. |

## D13 — Archive list with redirect targets

Reproduced from PORTFOLIO-MAP.md's executive summary, with the D2 amendment applied to `grok-agent-orchestra` (now `ARCHIVE_AFTER_GEM_EXTRACTION → xlOS`).

| repo | disposition | redirect target | notes |
|---|---|---|---|
| `grok-install` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: Low. Spec v2.14 + JSON Schema + Cloudflare Worker + Jekyll site + safety scanner + 8 reference manifests. Becomes the post-swap `grok-install` per D4. |
| `grok-install-cli` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: Low. Python CLI (Typer, Pydantic-v2, SQLite memory, deploy generators). Anchors the new `grok-install` PyPI v1.0.0 per D7. |
| `grok-install-action` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: High. Keep `action.yml` + `scripts/` + tests; drop the split-brain TypeScript rewrite (`src/`, `dist/`, `tests/*.test.ts`). |
| `vscode-grok-yaml` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: High. Scanner + schema registry "bones" port; 18 orphan schemas, 8 unregistered commands, snippets, grammars dropped. Version regime collapses to `version: "2.14"` per D5. |
| `grok-yaml-standards` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: Low. 14 spec dirs + JSON schemas + .grok templates + validate-schemas CI. Cleanest repo in the portfolio. |
| `grok-docs` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` (+ xlOS for playground per D2) | Mess: Low. MkDocs site → grok-install-v2; live client-side YAML playground (`docs/playground/` + `docs/javascripts/playground.js`) → xlOS/playground/. |
| `grok-install-brand` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: Medium. 12 banners + 8 icons + 12 plates + banner-svg.js generator + HUB-INDEX.html template. Distilled to ONE design system per D9; LICENSE flipped Apache-2.0 in QW4. |
| `awesome-grok-agents` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` | Mess: Low. 10 certified v2.14 templates + 8-job validate CI + Jekyll gallery + poster cards. 2 orphan v2.0 `agents/` entries dropped. |
| `grok-build-bridge` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2` (+ xlOS for Lucas-veto code per D2) | Mess: Medium. Pipeline + safety audit + 6 deploy targets + 8 bundled templates + Dockerfile → grok-install-v2. Lucas-veto-specific code → xlOS/safety/lucas-veto/. Fabricated `#status` block removed in QW1. |
| `grok-agent-orchestra` | `ARCHIVE_AFTER_GEM_EXTRACTION` (amended from `ARCHIVE_NO_MIGRATION`) | `xlOS` | Mess: Low. ~15 KLOC orchestration framework. Per D2: 5 patterns → xlOS/orchestration/, Lucas veto + roles → xlOS, native + simulated debate runtimes → xlOS/runtimes/, frontend + VSCode extension + Claude Skill → xlOS. Fallback: if extraction is too entangled, xlOS ships as README-only placeholder and orchestration migration slips to v1.1. |
| `x-platform-toolkit` | `ARCHIVE_AFTER_GEM_EXTRACTION` | `grok-install-v2` | Mess: High. 4 LIVE HTML tools (04 virality-scorer, 05 ab-rotator, 07 compound-calculator, 12 controversy-detector) + 3 shared libs (ui-kit, grok-client, x-api-client) migrate. 11 spec-only tools and broken assert-tool-status.mjs dropped. Tool count corrected to 12 in QW3b+QW5. |
| `grok-agent` | `ARCHIVE_AFTER_GEM_EXTRACTION` | `grok-install-v2` (+ xlOS via `grok-paradoxes`) | Mess: High. CLI + 1,566-LOC Constitution scanner + 4 finance templates + 7 super-agents (renamed per D6) + 22 creator templates + `packages/grok-paradoxes/` migrate to grok-install-v2; `grok-paradoxes` also referenced from xlOS per D3. Folded-in `tools/` and `pulse/` subtrees DO NOT migrate. Internal artifacts (CLAUDE.md, HANDOFF_LOG.md, MERGE_PLAN.md, ROADMAP.md) already moved in QW2. |
| `grok-agents-marketplace` | `MIGRATE_TO_GROK_INSTALL_V2` | `grok-install-v2/marketplace/` | Mess: Low. Next.js 15 storefront anchors the v2 marketplace per D10. grok-agent's internal `marketplace/` is dropped. Telemetry (Vercel KV + 5 stats endpoints) + 17 vitest cases keep verbatim. |
| `Frok` | `ARCHIVE_NO_MIGRATION` | n/a | Empty repo (LICENSE only). One-click archive. |

## Open questions for human review

1. **D1 "8 simple" agent count.** D1 quotes "~41 agents (8 simple + 22 creator + 4 finance + 7 super)". The 8 simple agents most plausibly map to `grok-install/templates/` (AlphaSignalBot, AskMe, DailyBrief, DevPilot, DogeDoughAI, ShowNotes, VibeReply, VisualForge), but `awesome-grok-agents` ships 10 additional certified templates and `grok-install-cli/examples/` adds 4 more. Confirm whether (a) only the 8 grok-install templates count and the awesome-grok-agents 10 are tracked separately, (b) all 18 simple-tier templates merge into one set (total ~51 agents instead of 41), or (c) some other consolidation. Affects marketplace category counts and the "~41 agents" public claim.
2. **D6 "Personal-context briefing agent" scope.** PORTFOLIO-MAP.md describes Self-Evolving Personal OS as having "auto-evolving workflows based on user patterns, morning briefing synthesis, rewind, explicit consent gates". The new name `personal-briefing` captures the briefing slice cleanly but understates the auto-evolving-workflow aspect. Confirm `personal-briefing` is the intended scope and the workflow-evolution behavior stays in scope under that name, or pick a more inclusive slug (e.g. `personal-context-agent`).
3. **D6 "Windows + web action runner" scope.** PORTFOLIO-MAP.md describes Cross-Reality Action Fabric as "Windows actions + X posting + web actions; every action gated by consent with reversibility log". The new name covers Windows + web but elides the X-posting surface, which is a primary use case. Confirm the slug `action-runner` is meant generically (covers X-posting too) or rename the display name to "Windows + web + X action runner".
4. **`grok-agent/extensions/vscode/` consolidation.** PORTFOLIO-MAP.md flags this as a candidate for consolidation with `vscode-grok-yaml`. D5 covers the schema-regime collapse but doesn't explicitly say which extension code base wins. Default assumption: `vscode-grok-yaml` is the survivor, `grok-agent/extensions/vscode/` is mined for any unique IntelliSense/lint logic and then archived. Confirm or override.
5. **`grok-build-bridge/bridge_live/` FastAPI inspector.** PORTFOLIO-MAP.md targets it for `grok-install-v2` as a separate `-services` package, with an 8h fix list (`/tmp` race, non-root, `/static` cap). Confirm this is in v1.0 scope or defer to v1.1.
