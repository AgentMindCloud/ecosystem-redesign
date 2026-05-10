# AgentMindCloud Ecosystem Redesign — Portfolio Map

**Date:** 2026-05-11
**Status:** Phase 0 — Diagnosis and inventory complete. No code changes yet.

## Executive summary

Fourteen repositories were audited. Across them we identified roughly **220
distinct gems** — paths or directories worth preserving — spread across CLI
code, JSON Schemas, Python packages, Cloudflare Workers, a Next.js storefront,
brand assets, agent templates, an MkDocs site, a Jekyll site, single-file HTML
tools, a VSCode extension, and a GitHub composite Action.

The portfolio has real engineering inside, but the outer surface reads as an
AI-scaffolded sprawl: internal artifacts (`MERGE_PLAN.md`, `HANDOFF_LOG.md`,
`CLAUDE.md`, `AUDIT-REPORT.md`) leak through to public roots in five repos;
grandiose names ("Living Narrative Fabric", "Self-Evolving Personal OS",
"Cross-Reality Action Fabric", "Tier 4 Spectral") sit next to genuinely working
code; and three repos ship verifiably false claims on their landing surfaces
(fabricated telemetry on `grok-build-bridge`, "20 tools" on
`x-platform-toolkit` with only 12 on disk, split-brain TS rewrite shipping
alongside the wired composite in `grok-install-action`). The top three
credibility risks are (1) AI-scaffolding artifacts visible at public repo
roots, (2) `grok-agent`'s jargon-dense framing of otherwise legitimate code,
and (3) `grok-build-bridge`'s fabricated landing-page metrics. The recommended
path is a **fresh rebuild of two repos** — `grok-install-v2` (the consolidated
public standard + CLI + schemas + safety scanner + Jekyll/MkDocs site + Next.js
marketplace) and **`xlOS`** (the experimental sibling for multi-agent
orchestration, voice, debate, swarm, and live-canvas surfaces) — followed by
**archiving all 14 legacy repos** (some after gem extraction, two with no
extraction).

## Per-repo table

| Repo | True purpose (what it actually does, not what README claims) | Mess level | Recommended fate |
|---|---|---|---|
| `grok-install` | Open spec + Jekyll site + Cloudflare Worker that mints `grok-install.yaml` agents from X via Grok, with a client-side safety scanner and consent-gated install/update flow. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-install-cli` | Production-grade Python CLI (`init`/`validate`/`scan`/`run`/`test`/`deploy`/`install`/`publish`) that turns `grok-install.yaml` into a runnable, multi-target-deployable xAI-SDK agent with rate-limited tools, SQLite memory, and a safety scanner — well-tested, on PyPI as `grok-install` v0.2.0. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-install-action` | GitHub composite action that validates `.grok/` manifests via the CLI, runs the safety scanner, posts PR comments, and stamps a "Grok-Native Certified" badge — wrapped in a half-finished TypeScript rewrite that ships alongside it. | High | `MIGRATE_TO_GROK_INSTALL_V2` |
| `vscode-grok-yaml` | VSCode extension that registers JSON schemas for `grok-*.yaml` kinds and surfaces `grok-install` CLI diagnostics, with three incompatible schema-version regimes and ~50% of the extension code (commands, webview, snippets, grammars) unregistered in `package.json`. | High | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-yaml-standards` | Reference library defining 14 JSON-Schema-validated YAML kinds (agent/config/prompts/workflow/update/test/docs/security/tools/deploy/analytics/ui/swarm/voice) with passing CI, drop-in `.grok/` templates, and Apache-2.0 examples. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-docs` | MkDocs Material documentation site for the `grok-install` standard — getting-started tutorial, CLI reference, multi-agent guides, WCAG-AA visuals spec, and a client-side YAML playground. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-install-brand` | Central brand-asset workshop: 12 animated ecosystem banners (SVG), 8 glyph icons, a Node banner generator, two premium template files (`README-PREMIUM.md`, `HUB-INDEX.html`), 12 hero "plate" PNGs, and a `REPOS-TO-CREATE.md` runbook. | Medium | `MIGRATE_TO_GROK_INSTALL_V2` |
| `awesome-grok-agents` | Curated gallery of 10 grok-install v2.12/v2.14 agent templates with strict CI (yamllint + ajv + per-template structural/scan/mock validators), poster cards, and a Jekyll gallery — plus 2 orphan v2.0 "agents/" that are spec-incompatible and not in the registry. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-build-bridge` | Python CLI + SDK that pipelines Grok-generated code through a two-layer safety audit (regex + Grok-as-judge) and dispatches to six deploy targets (x/vercel/render/railway/flyio/local), plus a FastAPI "passport" inspector — but with fabricated landing-page telemetry and a wheel-packaging bug that breaks the `publish` command for PyPI users. | Medium | `MIGRATE_TO_GROK_INSTALL_V2` |
| `grok-agent-orchestra` | 15 KLOC multi-agent orchestration framework (5 patterns, Lucas safety veto, xAI native + simulated debate runtimes, Rich TUI, Next.js dashboard, VSCode extension, Claude Skill, benchmarks) — shallowly "merged" into grok-build-bridge by dependency reference only; the actual code remains uncopied. | Low | `ARCHIVE_NO_MIGRATION` |
| `x-platform-toolkit` | A "20-tool" X-creator toolkit that actually ships 4 working single-file HTML tools, 8 spec docs, and 3 shared client libraries — with broken CI that asserts the missing 8 tools, README contradicting itself in 6 places, and 2 unpatched npm CVEs. | High | `ARCHIVE_AFTER_GEM_EXTRACTION` |
| `grok-agent` | The kitchen sink: v2.15 manifest schema, a 1.2 KLOC PowerShell CLI, a 1.5 KLOC Constitution safety scanner, 4 X-Money finance tools, 3 fully-implemented "Super Agents", 22 creator templates, a Next.js marketplace, browser + VSCode extensions, plus folded-in subtrees of `x-platform-toolkit` and a `grok-pulse-mcp-server`; framed in Tier-N / "Living Narrative Fabric" jargon with `CLAUDE.md`, `HANDOFF_LOG.md`, `MERGE_PLAN.md`, `ROADMAP.md` all at the public root. | High | `ARCHIVE_AFTER_GEM_EXTRACTION` |
| `grok-agents-marketplace` | Shipped Next.js 15 storefront for `grokagents.dev` — App Router, Vercel KV-backed telemetry, Spectral-palette UI kit (NeonButton, NebulaBackdrop, CertificationBadge, AgentPreviewCard), 5 stats API routes, and 17 passing vitest cases. | Low | `MIGRATE_TO_GROK_INSTALL_V2` |
| `Frok` | Empty repository — only an Apache-2.0 LICENSE file and no commits beyond the initial scaffold. | Low | `ARCHIVE_NO_MIGRATION` |

## Exhaustive gem inventory

What follows is every keeper found in each repo, listed at file or directory
level. Format: `path` — value — rebuild effort — migration target.

### grok-install

The flagship. Compiled HTML pages (`index.html`, `safe-agent-builder.html`,
`dashboard.html`) at the repo root are large but represent the live product
surface; the Cloudflare Worker under `worker/` is the live API gateway.

- `spec/v2.14/spec.md` — Complete 468-line declarative agent spec (tools, safety, rate/cost limits, x_native_runtime, optional visuals block) with explicit v2.13 back-compat. — keep verbatim — target: grok-install-v2
- `schemas/v2.14/schema.json` — Draft-2020-12 JSON Schema validator (410 LOC) enforcing manifest shape. — keep verbatim — target: grok-install-v2
- `grok-install.yaml` — 91-line reference manifest demonstrating tools, rate/cost limits, safety block, xAI endpoint. — keep verbatim — target: grok-install-v2
- `safety-scanner.js` — 96-line Node scanner enforcing 19 safety rules (mention-only, AI labeling, audit log, rate limits, hardcoded secrets, brain config, universal commands, genesis ID). — keep verbatim — target: grok-install-v2
- `SAFETY.md` — 33-line non-negotiable safety floor (no DMs by default, no mass actions, opt-out honor, no deepfakes). — keep verbatim — target: grok-install-v2
- `SECURITY.md` — Trust policy (pre-install scan, verification badge, responsibility matrix). — keep verbatim — target: grok-install-v2
- `DISCLAIMER.md` — 75-line liability boundaries (no warranty, signal-not-guarantee, third-party agent responsibility). — keep verbatim — target: grok-install-v2
- `PRIVACY.md` — Zero-tracking design statement; what runs in-browser vs. on CDNs. — keep verbatim — target: grok-install-v2
- `DESIGN_SYSTEM.md` — 208-line formal Spectral identity spec (palette tokens, typography, effect grammar, component recipes, do/don't list). — keep verbatim — target: grok-install-v2
- `assets/spectral-hero.svg`, `assets/logo-dark.svg`, `assets/logo-light.svg`, `assets/favicon.svg`, `assets/og-image.svg` — Hand-crafted Spectral SVGs (hero, monogram, favicon, social card). — keep verbatim — target: grok-install-v2
- `badge.svg` — "Built with grok-install" embed widely linked by downstream agent repos. — keep verbatim — target: grok-install-v2
- `assets/badges/grok-install-v2.12.svg`, `assets/badges/verified-by-grok.svg` — Version + verification shields. — keep verbatim — target: grok-install-v2
- `index.html` — Marketplace landing surface (compiled; loads Tailwind + featured agents JSON). — 6h to rebuild Spectral-native — target: grok-install-v2
- `safe-agent-builder.html` — Stateful builder UI: X OAuth, signal-or-template flow, mint YAML, GitHub repo auto-create, consent gate. — 24h to rebuild from clean components — target: grok-install-v2
- `dashboard.html` — Agent-owner dashboard (analytics, settings, memory, update flow). — 12h to rebuild — target: grok-install-v2
- `my-agents.html`, `docs.html`, `pricing.html`, `validate.html`, `404.html` — Supporting public pages; `validate.html` is a client-side YAML+AJV validator. — keep verbatim (rebuild as Next.js routes) — target: grok-install-v2
- `worker/src/index.js` — Cloudflare Workers entry: routes auth, manifest, API, dashboard, error logging. — keep verbatim — target: grok-install-v2
- `worker/src/handlers/{auth,api,dashboard,manifest}.js` — X + GitHub OAuth (PKCE), mint endpoint, profile analysis, mascot gen, safety check; agent retrieval, analytics export, pause/resume/delete; setup-app webhook. — keep verbatim — target: grok-install-v2
- `worker/src/lib/{response,logger,yaml-validator,pkce,genesis,kv,mascots,x-api,github,xai,repo-template,prompts}.js` — ~2.5 KLOC of production worker logic (HTTP helpers, KV wrapper, Genesis ID generator, X+GitHub+xAI clients, agent-repo template, system prompts). — keep verbatim — target: grok-install-v2
- `assets/grok-utils.js`, `assets/marketplace-utils.js` — Shared client utilities (escapeHtml, auth state, worker URL). — keep verbatim — target: grok-install-v2
- `wrangler.toml`, `package.json`, `Gemfile`, `_config.yml`, `_layouts/default.html` — Worker + Jekyll + npm config (`scan`, `validate`, `worker:deploy`, etc.); standard hygiene files (`.markdownlint-cli2.jsonc`, `.prettierrc.json`, `.yamllint.yml`). — keep verbatim — target: grok-install-v2
- `templates/{AlphaSignalBot,AskMe,DailyBrief,DevPilot,DogeDoughAI,ShowNotes,VibeReply,VisualForge}.yaml` — 8 reference agent manifests with tool declarations, rate/cost, safety, brain config. — keep verbatim — target: grok-install-v2
- `examples/{research-swarm,voice-agent,minimal,x-reply-bot}.yaml` — 4 reference manifests covering swarm, voice, minimal, and reply-bot patterns. — keep verbatim — target: grok-install-v2
- `featured-agents.json`, `manifest.json`, `news.json`, `trending.json` — Showcase + version manifest + changelog/discovery feeds. — keep verbatim (consider DB-backing in v2) — target: grok-install-v2
- `docs/data-layer.md`, `docs/feature-packs.md`, `docs/voice-and-clone-examples.md`, `docs/migration/v2.13-to-v2.14.md`, `docs/v2.14/` — Data schema, feature packs (image, analytics, memory, bilingual, advanced tools, commands), voice examples, migration guide, v2.14 reference docs. — keep verbatim — target: grok-install-v2
- `README.md` — 134-line flagship narrative (two-path flow, universal commands, update consent model, safety floor, architecture, quickstart). — keep verbatim — target: grok-install-v2
- `MERGE_PLAN.md`, `CHANGELOG.md` — Tier 1–7 roadmap; semantic versioning log. — archive snapshot then rewrite — target: archive-as-reference

### grok-install-cli

Production-grade Python CLI; single recent commit (v0.2.0 PyPI release). No
internal-artifact leaks at root. **All of `src/grok_install/` is keep-verbatim.**

- `src/grok_install/cli.py` — Full Typer CLI (`init`, `validate`, `scan`, `run`, `test`, `deploy`, `install`, `publish`) with dry-run + JSON output modes. — keep verbatim — target: grok-install-v2
- `src/grok_install/core/models.py` — Pydantic-v2 strict models for spec v2.12 with clear-error field validators (SafetyProfile, DeployTarget, MemoryScope literals). — keep verbatim — target: grok-install-v2
- `src/grok_install/core/parser.py` — YAML loader with `.grok/` overlay merge; recognizes `grok-install.yaml` + `grok-swarm.yaml` + `grok-voice.yaml`. — keep verbatim — target: grok-install-v2
- `src/grok_install/core/validator.py` — Semantic validator (agent handoff graphs, scheduled-runtime cron checks, research-mode verification). — keep verbatim — target: grok-install-v2
- `src/grok_install/core/registry.py` — Builtin tool registry (file ops, shell, X ops, GitHub, web_search). — keep verbatim — target: grok-install-v2
- `src/grok_install/runtime/{client,agent,tools,memory,swarm}.py` — xAI SDK wrapper (Protocol-based, lazy imports), single-agent runner with tool-calling loop, tool executor with sliding-window rate limits + approval gates, SQLite KV memory (session + long_term, WAL), multi-agent orchestrator with handoff routing + turn limits. — keep verbatim — target: grok-install-v2
- `src/grok_install/safety/{scanner,rules}.py` — Pre-install scanner (blocked patterns/tools, env vars, rate limits, approval flags, swarm cycles, voice permissions) + RuntimeSafetyGate; rule constants (HIGH_RISK_PERMISSIONS, REQUIRE_APPROVAL_DEFAULT, SWARM_MAX_AGENT_COUNT, VOICE_*). — keep verbatim — target: grok-install-v2
- `src/grok_install/deploy/{docker,vercel,railway,replit,base}.py` — Deploy-config generators for 4 platforms behind a `DeployArtifact` protocol; Dockerfile (python:3.11-slim, non-root). — keep verbatim — target: grok-install-v2
- `src/grok_install/integrations/{github,x_api}.py` — GitHub repo cloning/parsing; X API stubs. — keep verbatim — target: grok-install-v2
- `src/grok_install/telemetry/` — Opt-in telemetry (off by default, `GROKINSTALL_TELEMETRY` kill-switch, install_id). — keep verbatim — target: grok-install-v2
- `tests/test_cli.py`, `tests/test_parser.py`, `tests/test_validator.py`, `tests/test_safety.py`, `tests/test_runtime.py` — ~2,100 LOC of CLI smoke tests, parser, validator, safety, runtime — passing. — keep verbatim — target: grok-install-v2
- `tests/fixtures/` — `valid.yaml`, `swarm.yaml`, `voice.yaml`, `bad_unknown_tool.yaml`, `blocked_tool.yaml`, `swarm_cycle.yaml`. — keep verbatim — target: grok-install-v2
- `examples/hello-agent/`, `examples/reply-bot/` — Minimal working agent configs (file I/O + web search; X integration intent). — keep verbatim — target: grok-install-v2
- `pyproject.toml` — Clean hatchling packaging, 80% coverage floor, ruff + bandit. — keep verbatim — target: grok-install-v2
- `README.md` — Comprehensive: 60-second install, YAML-first pitch, swarm, 4-target deploy, safety gates; no misleading claims. — keep verbatim — target: grok-install-v2

### grok-install-action

Split-brain: a working composite action (`action.yml` + `scripts/`) sits next
to an unwired TypeScript rewrite (`src/` + `dist/`, ~3 MB) and four
`workflows-examples/*.yml` files that reference inputs that don't exist on the
action. The TS rewrite and broken examples should be deleted in one cut.

- `action.yml` — Clean composite action with proper CLI-install tolerance, JSON normalization, step guards, mode-aware enforcement. — keep verbatim — target: grok-install-v2
- `scripts/run.sh` — Core orchestrator: invokes CLI, normalizes JSON output on failure, exports report + safety score + badge path. — keep verbatim — target: grok-install-v2
- `scripts/annotations.js` — GitHub annotation writer with correct `escapeData`/`escapeProp`, severity→command mapping, mode-aware enforcement. — keep verbatim — target: grok-install-v2
- `scripts/badge.js` — SVG badge generator with brand-tokenized colors. — keep verbatim — target: grok-install-v2
- `scripts/comment.js` — PR comment poster with safety-score narrative, rule-ID linking, visuals-preview embed. — 2h (fix rule-ID anchor drift) — target: grok-install-v2
- `tests/unit.test.js` — Zero-dep Node tests (8 assertions over happy paths + v2.14 visuals fixtures). — keep verbatim — target: grok-install-v2
- `tests/fixtures/valid-v2.14-visuals/` — Passing agent manifest + expected JSON report. — keep verbatim — target: grok-install-v2
- `grok-install-brand/tokens/colors.css` — Canonical brand palette (`#0A0A0A`, `#00F0FF`, `#00FF9D`, `#FF2D55`) embedded here in correct form (overrides the drifted README). — keep verbatim — target: grok-install-v2
- `grok-install-brand/icons/` — SVG shield mark family (Grok-Native + Certified variants). — keep verbatim — target: archive-as-reference
- `.github/ISSUE_TEMPLATE/false-positive.yml`, `.github/CODEOWNERS`, `.github/CODE_OF_CONDUCT.md` — Issue template (scopes findings to upstream CLI repo), maintainers, community-health template. — keep verbatim — target: grok-install-v2 / archive-as-reference
- `SECURITY.md`, `LICENSE` — Posture doc (SHA-pinning guidance); Apache-2.0. — 1h to add release.yml SHA-pin examples — target: grok-install-v2 / archive-as-reference

**Delete entirely (dead split-brain):** `src/`, `dist/`, `vitest.config.ts`, `eslint.config.js`, all `tests/*.test.ts`, `scripts/annotate.py`, `marketplace.yml` (Marketplace reads `action.yml`, not `marketplace.yml`).

### vscode-grok-yaml

Three incompatible schema-version regimes coexist (`apiVersion` const vs.
enum vs. `spec_version`). About half of the source code (8 commands, a 5-file
React webview, snippets, grammars) is **unregistered** in `package.json` but
still ships in the VSIX. CI workflows for `package.yml` and `publish.yml` are
broken. The **scanner + schema registry "bones" are solid** and should port.

- `src/extension.ts` — 159-LOC activation, schema registration, scanner init, debounce lifecycle, file-document classification. — keep verbatim — target: grok-install-v2
- `src/scanner/cliAdapter.ts` — Production child_process wrapper: 60s timeout w/ SIGKILL, cancellation tokens, `shell: false`, exit-code semantics. — keep verbatim — target: grok-install-v2
- `src/scanner/diagnostics.ts` — Diagnostic collection, code-action provider, scoped-file application. — keep verbatim — target: grok-install-v2
- `src/schema/registry.ts` — Content-hash schema cache with `grokinstall-schema://…#v=<hash>` URIs; auto/bundled/remote source resolution with graceful fallback. — keep verbatim — target: grok-install-v2
- `src/debounce.ts` — 42-LOC typed debounce with `flush()` + `cancel()`. — keep verbatim — target: grok-install-v2
- `src/fileMatching.ts` — Glob + content-sniff document classification (resolveByGlob, resolveByContent). — 1h modernize against public vscode APIs — target: grok-install-v2
- `src/ui/statusBar.ts`, `src/ui/commands.ts` — Status-bar state machine; registerCommands wiring `showOutput`/`rescan`/`refreshSchemas`. — keep verbatim — target: grok-install-v2
- `schemas/grok-{agent,workflow,tool,prompt,model,dataset,eval,deploy,secret,policy,telemetry,install}.schema.json` — 12 live Draft-7 JSON schemas with `apiVersion: const "grokinstall.dev/v1"`. — keep verbatim, reconcile version regime in `grok-yaml-standards` — target: grok-install-v2
- `schemas/index.json` — Schema registry manifest (12 kinds, file/glob/apiVersion mapping, source attribution). — keep, standardize version regime — target: grok-install-v2
- `test/fixtures/grok-agent.valid.yaml`, `test/suite/schema.test.ts` — Valid Agent spec + schema registry tests. — keep verbatim — target: grok-install-v2
- `.vscode/launch.json`, `.vscode/tasks.json` — Debug + npm-script task definitions. — keep verbatim — target: grok-install-v2
- `esbuild.mjs` — Three-config build (extension host + webview + CSS) with watch support. — keep extension-host config, drop webview config — target: grok-install-v2
- `.github/workflows/ci.yml`, `.github/workflows/release.yml` — Working CI (build/typecheck/lint/test) + tag-triggered Marketplace + Open VSX publish. — keep verbatim, add version guard — target: grok-install-v2
- `.github/ISSUE_TEMPLATE/{bug,feature,snippet-request}.yml` — Well-written templates with required fields. — keep verbatim — target: grok-install-v2

**Orphans (do NOT migrate):** `src/commands/` (8 unregistered commands ~430 LOC), `src/webview/gallery/` (5 unbuildable .tsx files, no React deps), `snippets/` (4 JSON files emitting incompatible v2.14 apiVersion), `syntaxes/grok-yaml.tmLanguage.json`, `language-configuration.json`, the 18 orphan schemas in `schemas/` outside of `index.json`.

### grok-yaml-standards

The cleanest repo in the portfolio. Eight kinds are visible at the top level
(`grok-agent`, `grok-analytics`, `grok-config`, `grok-deploy`, `grok-docs`,
`grok-prompts`, `grok-security`, `grok-tools`); per `version-reconciliation.md`
v2.0.0 lifts the count to **14 specs** by adding `grok-swarm`, `grok-voice`,
`grok-update`, `grok-test`, `grok-ui`, `grok-workflow`.

- `grok-agent/`, `grok-config/`, `grok-prompts/`, `grok-workflow/`, `grok-update/`, `grok-test/`, `grok-docs/`, `grok-security/`, `grok-tools/`, `grok-deploy/`, `grok-analytics/`, `grok-ui/`, `grok-swarm/`, `grok-voice/` — 14 spec directories: schema.md + example.yaml + (for completes) examples/ + security-considerations.md + usecases.md. **All 14.** — keep verbatim — target: grok-install-v2
- `schemas/` — 14 Draft-7 JSON Schema files corresponding to each spec. — keep verbatim — target: grok-install-v2
- `.grok/` — Production-ready drop-in YAML templates for all 14 specs + `brand-tokens.yaml`. — keep verbatim — target: grok-install-v2
- `.grok/proposed/` — Advanced template variants (`grok-agent.advanced.yaml`, `grok-workflow.advanced.yaml`, `grok-security.advanced.yaml`); not yet blessed. — keep, mark experimental — target: archive-as-reference
- `.yamllint` — Linting config that the CI workflow depends on. — keep verbatim — target: grok-install-v2
- `.github/workflows/validate-schemas.yml` — Yamllint over all `.grok/*.yaml` + every `example.yaml` + `examples/*.yaml`, then ajv-cli Draft-7 against schemas. — keep verbatim — target: grok-install-v2
- `.github/workflows/release.yml` — Tag-triggered GitHub Releases with auto-generated notes. — keep verbatim — target: grok-install-v2
- `README.md`, `standards-overview.md`, `version-reconciliation.md` — Spec overview, master overview, authoritative count + version history. — keep verbatim — target: grok-install-v2 + archive-as-reference
- `ROADMAP.md`, `LAUNCH.md` — Path to xAI integration; launch announcement. — keep verbatim — target: archive-as-reference
- `SECURITY.md`, `DISCLAIMER.md`, `CHANGELOG.md`, `CONTRIBUTORS.md`, `CONTRIBUTING.md`, `LICENSE` — Threat model + disclosure SLA, legal disclaimer, full v2.0.0 release notes, community credits, contribution flow, Apache-2.0. — keep verbatim — target: grok-install-v2
- `docs/` — Jekyll site for the spec library (`grok-swarm.md`, `grok-voice.md`, `index.md`, `posters/`). — keep verbatim — target: grok-install-v2

### grok-docs

MkDocs Material site. No claim drift; no AI-filler pages detected. Schemas
shipped here are stale snapshots that should be relocated to
`grok-yaml-standards`; otherwise everything is keep-verbatim.

- `docs/getting-started/first-agent.md` — 10-minute tutorial with copy-paste scaffolding, CLI examples, expected output. — keep verbatim — target: grok-install-v2
- `docs/cli/reference.md` — Complete command reference (init/validate/scan/run/test/deploy/install/publish) with flags, exit codes, examples. — keep verbatim — target: grok-install-v2
- `docs/guides/multi-agent-swarms.md` — Three production patterns (pipeline, critic loop, fan-out) with YAML, Mermaid, Jinja conditionals, orchestration tips. — keep verbatim — target: grok-install-v2
- `docs/guides/tool-schemas.md` — Python decorator syntax for tool definition + parameter validation + agent wiring. — keep verbatim — target: grok-install-v2
- `docs/guides/deployment.md` — Deploy targets (Vercel/Railway/Docker/Replit), step-by-step flows, cold-start expectations, pitfalls. — keep verbatim — target: grok-install-v2
- `docs/guides/safety-profiles.md` — Standard vs. strict profiles, permission model, rate limits, approval gates. — keep verbatim — target: grok-install-v2
- `docs/guides/x-integration.md` — Mention listeners, publishing, auth flow. — keep verbatim — target: grok-install-v2
- `docs/v2.14/visuals.md`, `docs/v2.14/accessibility.md` — WCAG 2.2 AA checklist + responsive `srcset` + themes + alt text + contrast ratios; v2.14 visuals block reference; normative. — keep verbatim — target: grok-install-v2
- `docs/playground/index.md` + `docs/javascripts/playground.js` — Live in-browser YAML validator (Monaco + js-yaml + Ajv), JSON-pointer error reporting, live preview of v2.14 visuals; zero network. — keep verbatim — target: xlOS (portable validator)
- `docs/javascripts/extra.js` — Landing-page terminal animation (respects prefers-reduced-motion). — keep verbatim — target: grok-install-v2
- `docs/stylesheets/extra.css` — Material overrides, Grok cyan + black palette, typography refinements (Inter, JetBrains Mono). — keep verbatim — target: grok-install-v2
- `overrides/main.html` — OG + Twitter card boilerplate. — keep verbatim — target: grok-install-v2
- `docs/gallery/index.md`, `docs/ecosystem/` (index + 3 integration guides: xAI SDK, LiteLLM, Semantic Kernel) — Gallery + ecosystem layer. — keep verbatim — target: grok-install-v2
- `docs/for-xai/adoption-guide.md` — Business-level adoption pitch for xAI decision-makers. — keep verbatim — target: archive-as-reference
- `docs/assets/schemas/v2.12/` (5 schemas), `docs/assets/schemas/latest/` (12 v2.13 schemas), `docs/assets/schemas/v2.14/grok-visuals.schema.json` — Versioned schema snapshots. — keep verbatim, relocate to `grok-yaml-standards` distribution — target: archive-as-reference
- `mkdocs.yml`, `requirements.txt` — MkDocs config + deps. — keep verbatim — target: grok-install-v2

### grok-install-brand

Brand workshop. Tokens live in three places (banner-svg.js hardcoded,
`HUB-INDEX.html` CSS vars, `README-PREMIUM.md` shields URLs) with no canonical
`tokens/` dir; consolidating those is part of the migration. License is "All
rights reserved" which should be flipped to Apache-2.0 to match the
ecosystem.

- `banners/*.svg` — 12 animated, self-contained ecosystem hero banners (1500×500, CSS-only animations, GitHub-camo safe) + `cta-open-hub.svg`. — keep verbatim — target: grok-install-v2
- `icons/*.svg` — 8 core glyphs (action, certified, install, marketplace, scanner, swarm, voice, vscode); 44×44, brand-locked strokes. — keep verbatim — target: grok-install-v2
- `generators/banner-svg.js` — 305-LOC zero-dep Node CLI: outputs self-contained 1500×500 animated SVG with inline CSS; tokens hardcoded at lines 31–38. — 6h to extract into a `@grok-install/banner-generator` npm package with parameterized tokens — target: grok-install-v2
- `generators/banner-svg.example.sh` — Worked usage example calling the generator with named args. — keep verbatim — target: grok-install-v2
- `templates/README-PREMIUM.md` — 325-line magazine-style README template with token slots (`{{REPO_NAME}}`, `{{TAGLINE}}`, etc.), hero SVG, status dashboard, Mermaid blocks, feature grid, cross-link table. — keep verbatim — target: grok-install-v2
- `templates/HUB-INDEX.html` — 1,260-line single-file premium hub deployed to `/docs/index.html` in each ecosystem repo; CSS vars, Google Fonts, Mermaid CDN, JSON-LD, OG/Twitter meta, chatbot embed slots. — keep verbatim (fix `#050505`/`#1a1a1a` token drift first) — target: grok-install-v2
- `plate_01..plate_12_*.png` — 12 high-res 2400×1000 ecosystem-portfolio plates (one per major repo). — keep verbatim (expensive to remake) — target: grok-install-v2 (`docs/portfolio/`)
- `docs/REPOS-TO-CREATE.md` — 463-line prep checklist for 5 satellite repos (vscode-grok-yaml, grok-install-action, grok-agents-marketplace, grok-repo-chatbot, grok-repo-bot-widget); GitHub descriptions, topic sets, taglines, Marketplace metadata, env var specs. — keep verbatim — target: grok-install-v2
- `STATE.md`, `README.md`, `LICENSE.md` — Repo state log; brand README; relicense Apache-2.0 on migration. — archive-as-reference

### awesome-grok-agents

Production-ready gallery. CI enforces yamllint + ajv schema + per-template
structural + scan + mock-execute on every PR; 10 certified templates all pass.
The 2 v2.0 `agents/` entries are spec-incompatible orphans and should not
migrate.

- `templates/hello-grok/` — Canonical starter; 8-line tools, v2.14 with minimal visuals. — keep verbatim — target: grok-install-v2
- `templates/reply-engagement-bot/` — Approval-gated X replies, 65-line tools, multi-step. — keep verbatim — target: grok-install-v2
- `templates/trend-to-thread/` — X trend monitor → thread drafter; multi-step, strict, v2.14 with visuals. — keep verbatim — target: grok-install-v2
- `templates/research-swarm/` — 3-agent swarm (researcher + critic + publisher). — keep verbatim — target: grok-install-v2
- `templates/code-reviewer/` — GitHub PR review agent; 72-line tools, v2.14 terminal poster. — keep verbatim — target: grok-install-v2
- `templates/thread-ghostwriter/` — X thread polishing; 72-line tools, v2.14 premium visuals. — keep verbatim — target: grok-install-v2
- `templates/personal-knowledge/` — X-history indexing + RAG; multi-step. — keep verbatim — target: grok-install-v2
- `templates/scientific-discovery/` — arXiv + X daily digest; swarm, v2.12. — keep verbatim — target: grok-install-v2
- `templates/voice-agent-x/` — Voice-first transcript-review-publish; v2.14 with haptics + WCAG AAA. — keep verbatim — target: grok-install-v2
- `templates/live-event-commentator/` — Real-time X commentary; rate-limited, kill-switch, strict. — keep verbatim — target: grok-install-v2
- `featured-agents.json` — Registry of all 10 certified templates with name, path, tags, safety_profile, certified, poster. — keep verbatim — target: grok-install-v2
- `schemas/featured-agents.schema.json`, `docs/SCHEMA.md` — Registry schema + invariants (no orphans, no ghosts, safety enum check, poster existence). — keep verbatim — target: grok-install-v2 / archive-as-reference
- `scripts/validate_template.py`, `scripts/validate_registry.py`, `scripts/scan_template.py`, `scripts/mock_run_template.py` — Structural validator; registry consistency; security scanner; mock-tool executor with type-hint-aware sampling and PermissionError expectations. — keep verbatim — target: archive-as-reference (port logic into the v2 CLI)
- `scripts/grok_install_stub/` — Stub runtime so templates run in CI without real xAI calls. — keep verbatim — target: grok-install-v2
- `.github/workflows/validate-templates.yml`, `.github/workflows/release.yml` — 8-job matrix CI (yamllint → registry → schema → spec-version → link-check → per-template validate/scan/mock); tag-triggered Release. — keep verbatim — target: grok-install-v2
- `docs/posters/` — 10 SVG + 10 PNG poster cards (generated by `gen_poster.py`). — keep verbatim — target: grok-install-v2
- `docs/template-anatomy.md`, `docs/submitting-your-own.md` — Canonical layout spec; contributor guide with v2.14 visuals opt-in. — keep verbatim — target: archive-as-reference
- `_layouts/default.html`, `_config.yml` — Jekyll dark-theme template; site config. — keep verbatim — target: archive-as-reference
- `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `SECURITY.md`, `DISCLAIMER.md`, `LICENSE` — Anchor doc + community + changelog + security + legal + Apache-2.0. — keep verbatim (rewrite README to drop `agents/` references) — target: grok-install-v2 / archive-as-reference

**Orphans (do NOT migrate):** `agents/viral-thread-architect/` and `agents/voice-companion/` — both v2.0 spec with `.grok/` structure but no `tools/` implementation, not in `featured-agents.json`, rejected by the CI gate.

### grok-build-bridge

Real Python engineering (~8.6 KLOC, 85% coverage floor) hobbled by
distribution chaos. The README badge claims "PyPI Pending", `--upload` is
shipped but documented as v0.3.0-future, `marketplace/` is excluded from the
wheel breaking `publish` for PyPI users, and the docs landing page shows fake
metrics ("1,284 active agents, 99.7% safety pass rate"). The code itself
mostly migrates verbatim.

- `grok_build_bridge/safety.py` (330 LOC) — Dual-layer safety: regex scan + Grok-4.20-0309 JSON-mode LLM audit; SafetyReport dataclass + cost estimation. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/_patterns.py` (280 LOC) — Compiled regex catalog (AWS/xAI/OpenAI/GitHub keys, eval/exec, `shell=True`, no-timeout requests, pickle/yaml unsafe loads). — keep verbatim — target: grok-install-v2
- `grok_build_bridge/builder.py` (240 LOC) — Grok prompt → code generator with three source modes (grok via xai-sdk / local / `grok-build-cli` fallback); fenced-block extraction; manifest emission. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/deploy.py` (420 LOC) — Six-target dispatcher (x / vercel / render / railway / flyio / local) with pre-flight X-post audit. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/publish.py` (480 LOC) — Marketplace packaging (bridge.yaml + manifest validation against `marketplace/manifest.schema.json` v1.0), ZIP, `--upload`. — 2h (fix wheel.packages to include `marketplace/`) — target: grok-install-v2
- `grok_build_bridge/cli.py` (1,100 LOC) — Typer CLI: `run`/`dev` (hot-reload)/`validate`/`templates`/`init`/`link` (Orchestra wiring)/`fork`/`publish`/`doctor`/`version`. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/runtime.py` (280 LOC) — `orchestrate()` master pipeline (parse → generate → safety → deploy → manifest); BridgeResult; exception hierarchy. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/parser.py` (290 LOC) — Draft-2020-12 YAML schema loader; defaults frozen at parse time; `load_yaml` SDK entry. — keep verbatim — target: grok-install-v2
- `grok_build_bridge/schema/bridge.schema.json` (840 LOC) — Strict Draft-2020-12 schema (6 deploy targets, 2 source modes, model enum, safety knobs). — keep verbatim — target: grok-install-v2
- `grok_build_bridge/templates/` — 8 bundled, dry-runnable YAML templates (hello-bot, x-trend-analyzer, truthseeker-daily, code-explainer-bot, grok-build-coding-agent, research-thread-weekly, railway-worker-bot, flyio-edge-bot). — keep verbatim — target: grok-install-v2
- `tests/` — 11 files, 3,296 LOC, 163 test functions; parametrized integration tests dry-run all 8 templates on every commit. — keep verbatim — target: grok-install-v2
- `Dockerfile` — Multi-stage, non-root (uid 10001), tini entrypoint, healthcheck via `grok-build-bridge version`. — keep verbatim — target: grok-install-v2
- `bridge_live/` — FastAPI inspector service (parse + static safety only): `/p/<sha8>`, `/showcase`, `/launch`; Jinja2 + pure-frontend file upload. — 8h (fix `/tmp` race, non-root, cap `/static`) — target: grok-install-v2 (separate `-services` package)
- `vscode/schemas/bridge.schema.json` — VSCode JSON schema for bridge.yaml autocomplete. — 1h (sync Draft 2020-12 + 6-target enum with main schema) — target: grok-install-v2
- `.github/workflows/ci.yml` — Six-job CI (lint + test 3.10/3.11/3.12 × ubuntu/macos + schema + safety-scan + docs-link + build). — 0.5h (de-neuter `pip-audit || true`) — target: grok-install-v2
- `.github/workflows/release.yml` — Trusted-Publishing PyPI release; ready to execute. — keep verbatim — target: grok-install-v2
- `.github/actions/bridge/action.yml` — Composite GitHub Action for PR validation (validate + dry-run, posts markdown verdict). — 2h end-to-end test — target: grok-install-v2
- `marketplace/manifest.schema.json` — Forward-compatible v1.0 schema (model, target, language, tools, cost, schedule, audit_status, lucas_veto_enabled, package + SHA-256 list). — keep verbatim — target: grok-install-v2
- `pyproject.toml` — Hatch + strict mypy + ruff + coverage gates. — 1h (force-include `marketplace/` in wheel) — target: grok-install-v2
- `docs/build-bridge.md`, `docs/deploy-targets-railway-flyio.md` — End-to-end pipeline walkthrough; Railway/Fly.io deploy guide. — keep verbatim — target: grok-install-v2
- `examples/orchestra-bridge/` — Worked Orchestra+Bridge pattern (`weekly-research-publisher` with `lucas_veto_enabled: true`). — keep verbatim — target: archive-as-reference
- `README.md` — 700-line narrative; needs 3h editorial pass (PyPI badge, `--upload` contradictions, link docs, remove "not yet wired" marketplace language). — 3h — target: grok-install-v2
- `MERGE_PLAN.md`, `AUDIT-REPORT.md`, `ROADMAP.md`, `CHANGELOG.md` — Governance + audit + roadmap + log. — archive-as-reference

### grok-agent-orchestra

Pre-assigned `ARCHIVE_NO_MIGRATION`. The "merge into grok-build-bridge" was
**API-only** — Bridge imports Orchestra's primitives as a Python dependency
but does **not** copy code; if Orchestra is deleted, ~15 KLOC of runtime
patterns, simulation engine, TUI, sources layer, benchmarks, frontend, and
Claude Skill are permanently lost. Many gems should still be extracted to
**xlOS** (the experimental sibling).

- `grok_orchestra/patterns.py` — 5 composable patterns (hierarchical, dynamic-spawn, debate-loop, parallel-tools, recovery); each ≤120 LOC; delegates streaming to native/simulated runtimes; runs veto once on final synthesis. — 12h (port to xlOS pattern runner) — target: xlOS
- `grok_orchestra/safety_veto.py` — Lucas fail-closed veto: `grok-4.20-0309` @ `reasoning_effort=high`, strict JSON, retry loop with terser prompts, malformed → `safe=False`. — 8h (integrate into xlOS safety layer) — target: grok-install-v2
- `grok_orchestra/multi_agent_client.py` — xAI multi-agent streaming wrapper; typed `MultiAgentEvent` dataclass (token/reasoning_tick/tool_call/tool_result/final/rate_limit); RateLimitError fallback. — 6h — target: archive-as-reference
- `grok_orchestra/runtime_native.py` — Direct `grok-4.20-multi-agent-0309` dispatcher (4 or 16 agents, server-side role routing, single aggregated stream); OrchestraResult dataclass. — 4h reference value — target: archive-as-reference
- `grok_orchestra/runtime_simulated.py` — Prompt-simulated debate engine (GROK_SYSTEM / HARPER / BENJAMIN / LUCAS roles, round-by-round, llama3.1:8b local or grok-4.20). — 10h (port as local debate mode) — target: xlOS
- `grok_orchestra/combined.py` — Bridge + Orchestra integration (`run_combined_bridge_orchestra()`: build → scan → debate → veto → deploy in one TUI panel); `CombinedResult` dataclass. — keep verbatim — target: grok-install-v2
- `grok_orchestra/_roles.py` — Role system (GROK coordinator, HARPER researcher + web_search, BENJAMIN logician + opt-in code_execution, LUCAS contrarian); system prompts + default tool routing. — keep verbatim — target: grok-install-v2
- `grok_orchestra/_errors.py` — Error contract (EXIT_RUNTIME=1, EXIT_CONFIG=2, EXIT_SAFETY_VETO=4); `render_error_panel()`. — keep verbatim — target: grok-install-v2
- `grok_orchestra/cli.py` — Typer CLI (`run`, `combined`, `validate`, `templates`, `init`, `debate`, `veto`); violet accent color. — keep verbatim — target: grok-install-v2
- `grok_orchestra/_templates.py` + `templates/` — 10 bundled starter YAML templates + JSON schema for IDE completions (red-team, research, analysis, debate). — keep verbatim — target: grok-install-v2
- `grok_orchestra/streaming.py` — Rich TUI debate rendering (role-colored lanes, live reasoning ticks, tool calls, verdict panel). — 8h (port to xlOS TUI) — target: xlOS
- `grok_orchestra/sources/` — Web research (Tavily), MCP sources, robots.txt cache, budget tracking, fetch with timeout/retry. — 20h integration testing — target: grok-install-v2
- `grok_orchestra/tracing/` — LangSmith + OTel/OTLP backends; NoOpTracer default (zero overhead until env var set). — keep verbatim — target: grok-install-v2
- `benchmarks/` — 12-goal corpus vs. GPT-Researcher, Claude Sonnet judge, factual + hallucination scoring, monthly + per-release reruns. — keep verbatim — target: archive-as-reference
- `benchmarks/methodology.md` — Peer-review rubric (tokens, cost, wall time, citations, audit lines per dollar, hallucination window, Lucas veto correctness). — keep verbatim — target: archive-as-reference
- `frontend/` — Next.js real-time debate visualization (webview panels, live agent status, reasoning gauge streaming, citation-rich export). — 40h port to xlOS web UI — target: xlOS
- `extensions/vscode/` — VSCode extension (YAML validation, right-click run, side-panel webview, role-colored output, Lucas verdict bench); ≥110 LOC TS; sideload-only. — 12h — target: xlOS
- `skills/agent-orchestra/` — Claude Skill: local (`grok-orchestra` CLI) or remote (FastAPI POST) transport; template chooser; Skill-hook integration. — keep verbatim — target: xlOS
- `docs/orchestra.md`, `docs/getting-started.md`, `docs/vscode-orchestra.md` — Mode deep-dive; capability matrix (Demo/Local/Cloud); VSCode integration. — keep verbatim — target: archive-as-reference
- `mkdocs.yml` + `docs/stylesheets/extra.css` — Material theme + mike versioning + auto-deploy. — keep verbatim — target: archive-as-reference
- `examples/*.yaml` — 15 executable YAML examples (all 5 patterns, local Ollama, cloud BYOK, MCP sources, image generation). — keep verbatim — target: archive-as-reference
- `Dockerfile` + `docker-compose.yml` — Multi-arch image, non-root, healthcheck, FastAPI + uvicorn, optional auth. — keep verbatim — target: grok-install-v2
- `launch/seven-day-shipping-plan-orchestra.md` — Go-to-market timeline, personas, feature parity matrix. — keep verbatim — target: archive-as-reference
- `pyproject.toml`, `release_notes_v0.1.0.md`, `CHANGELOG.md` — Clean optionality extras; v0.1.0 launch summary; full version history. — keep verbatim — target: archive-as-reference

### x-platform-toolkit

The "20-tool" claim collides with 12 directories actually on disk; CI has been
failing on every push because `assert-tool-status.mjs` hardcodes all 20. The
4 live tools and 3 shared utility packages **are** worth migrating; everything
else is either a spec or a stub.

- `tools/04-pre-post-virality-scorer/index.html` — 601-LOC vanilla-JS heuristic virality scorer; fully functional, embeddable. — keep verbatim — target: grok-install-v2
- `tools/07-content-compound-calculator/index.html` — 752-LOC Chart.js growth calculator; polished, responsive, production-grade aesthetic. — keep verbatim — target: grok-install-v2
- `tools/05-pinned-post-ab-rotator/index.html` — 336-LOC A/B rotation UI for pinned posts; localStorage-backed. — 2h (fix `localStorage.clear()` blast radius + non-invertible 0-follow recording) — target: grok-install-v2
- `tools/12-controversy-detector/index.html` — 843-LOC sentiment/keyword analyzer; most complex of the four; same localStorage bug as 05. — 2h (fix localStorage bug) — target: grok-install-v2
- `tools/03-contextual-reply-suggester/SPEC.md` — Grok-powered reply suggester with virality ranking. — 10–14h (rebuild from spec) — target: archive-as-reference
- `tools/06-digital-product-storefront/SPEC.md` — Multi-provider (Stripe/NowPayments/PayPal) storefront. — 16–24h — target: archive-as-reference
- `tools/11-ghostwriter-mode-with-memory/SPEC.md` — Grok-backed contextual post assistant with memory. — 8–12h — target: archive-as-reference
- `tools/13-thread-to-newsletter-converter/SPEC.md` — Grok prompt + Resend to convert threads to email. — 6–10h — target: archive-as-reference
- `tools/16-follower-migration-assistant/SPEC.md` — Grok + X API to re-engage followers from an old account. — 10–14h — target: archive-as-reference
- `tools/17-post-necromancer/SPEC.md` — Resurrection + re-timing of dormant high-value posts. — 8–12h — target: archive-as-reference
- `tools/20-x-articles-optimizer/SPEC.md` — Article-to-post optimizer via Grok. — 6–10h — target: archive-as-reference
- `shared/ui-kit/tokens.css` — 63-LOC design-token set (colors, spacing, shadows, typography); inlined into every live tool. — keep verbatim (convert to CSS Variables module) — target: grok-install-v2
- `shared/ui-kit/components.css` — 241-LOC reusable component classes (buttons, inputs, cards, modals); neon-dark aesthetic. — keep verbatim — target: grok-install-v2
- `shared/grok-client/src/index.js` — 141-LOC xAI Grok wrapper (chat, stream, tool support, JSON mode); CJS + ESM. — 0.5h (centralize default model) — target: grok-install-v2
- `shared/x-api-client/src/index.js` — 53-LOC X API v2 wrapper (OAuth, tweets, users, search, pagination). — 0.5h (fix URL encoding bug) — target: grok-install-v2
- `shared/test-utils/src/mock-fetch.js` — 87-LOC mock-fetch helper used by both clients. — keep verbatim — target: grok-install-v2
- `docs/PHILOSOPHY.md`, `docs/BRAND.md`, `docs/ARCHITECTURE.md`, `docs/CORS.md` — Design philosophy (open-by-default, ToS-respecting, single-file, aesthetic); design system; monorepo Mermaid; CORS/proxy guidance. — keep verbatim — target: archive-as-reference
- `docs/tokens.md` — Per-tool env + token requirements table. — 1h (refresh to 12 tools) — target: archive-as-reference
- `LICENSE` — Apache-2.0. — keep verbatim — target: grok-install-v2

**Delete entirely on archive:** `tools/19-grok-thread-composer/` (scaffold only), the broken `scripts/assert-tool-status.mjs`, the stale `CHANGELOG.md`, and the dead-link rows in `README.md`. **Drop:** tool 02 references (folder deleted), tools 01/08/09/10/14/15/18 references (never existed).

### grok-agent

The biggest, jargon-densest repo. Real code under the grandiose names. The
core (CLI + Constitution scanner + the 3 fully-built Super Agents +
`grok-paradoxes` package) is genuinely valuable. The folded-in `tools/`
(x-platform-toolkit subtree) and `pulse/` (MCP scaffold) subtrees should be
detached and not migrated. The presence of `CLAUDE.md`, `HANDOFF_LOG.md`,
`MERGE_PLAN.md`, and `ROADMAP.md` at the public root is the loudest amateur
signal in the portfolio.

- `spec/v2.15/grok-agent.yaml` — Full v2.15 manifest schema with v2.14 back-compat; Pydantic-strict validation model; windows / multi_agent / constitution / public_api sections. — keep verbatim — target: grok-install-v2
- `spec/v2.15/changelog.md` — v2.14 → v2.15 delta (9 schema additions with rationale). — keep verbatim — target: grok-install-v2
- `cli/grok-agent.ps1` — 1,200-LOC PowerShell CLI (install/validate/new/list/run + `grok-install-this` FromStdin flow). — keep verbatim — target: grok-install-v2
- `cli/grok-agent.py` — Pydantic-v2 fallback validator (schema-only). — keep verbatim — target: grok-install-v2
- `safety/scanner.py` — 1,566-LOC, 34-check Constitution scanner (finance disclaimers, consent gates, provenance, cost limits). — keep verbatim — target: grok-install-v2
- `safety/constitution.md` — 7 articles + per-agent-kind enforcement matrix; machine-readable safety rules. — keep verbatim — target: grok-install-v2
- `packages/grok-paradoxes/` — Spinoff Python package (0.1.0, PyPI-ready); contradiction detection + authority-weighted reconciliation; tests + examples + clean `pyproject.toml`. — keep verbatim — target: grok-install-v2 (or xlOS as a research utility)
- `packages/grok-paradoxes/src/grok_paradoxes/contradiction.py` — Core: claim grouping, severity bucketing (minor/moderate/major), rationale formatting. — keep verbatim — target: grok-install-v2
- `packages/grok-paradoxes/tests/test_contradiction.py` — Pytest covering empty claims, single source, authority tie-breaking. — keep verbatim — target: grok-install-v2
- `packages/grok-paradoxes/examples/` — 3 runnable examples (2-source disagreement, weighted reconciliation, 3-way consensus). — keep verbatim — target: grok-install-v2
- `templates/finance/x-money-companion-dashboard/` — Streamlit 6-tab finance dashboard with SQLite + yfinance + newsapi + mandatory "Not financial advice" + "Not tax advice" banners; PowerShell launcher; smoke test. — keep verbatim — target: grok-install-v2
- `templates/finance/x-smart-cashtag-alpha-engine/` — Alpha-signal detection + portfolio sim + contradiction detector wired in. — keep verbatim — target: grok-install-v2
- `templates/finance/x-creator-payout-optimizer/` — 30/60/90-day earnings forecast; reads SQLite from #1 + #4. — keep verbatim — target: grok-install-v2
- `templates/finance/x-money-vision-analyzer/` — Grok 4.3 vision + receipt parsing + one-click import to #1. — keep verbatim — target: grok-install-v2
- `templates/super-agents/living-narrative-fabric/` — Fully implemented: 3-runtime fallback orchestrator (Mastra HTTP → LangGraph → pure Python), Pydantic state, contradiction detection via `grok-paradoxes`, Langfuse hooks, versioned synthesis with parent_version_id chain. — keep verbatim — target: grok-install-v2 (rename out of the jargon)
- `templates/super-agents/self-evolving-personal-os/` — Fully implemented: auto-evolving workflows based on user patterns, morning briefing synthesis, rewind, explicit consent gates. — keep verbatim — target: grok-install-v2 (rename)
- `templates/super-agents/cross-reality-action-fabric/` — Fully implemented: Windows actions + X posting + web actions; every action gated by consent with reversibility log. — keep verbatim — target: grok-install-v2 (rename)
- `templates/super-agents/{agent-swarm-with-shared-memory,provenance-first-trust-engine,narrative-contradiction-detector,zero-config-i-want-to-agent}/` — Manifest-only templates (P117–P120). — keep verbatim — target: grok-install-v2
- `templates/creator/content-idea-generator/` — Full implementation: 7-section idea plan, vanity-hook paradox detection, Trend Alignment scoring, deterministic seeding via `sha256(handle+niche+tone+date)`. — keep verbatim — target: grok-install-v2
- `templates/creator/` (21 more templates) — All follow identical Recipe B structure: manifest + `run.py` + `prompts/` + `README`; sampled implementations (analytics-summarizer, comment-engagement-booster, hashtag-strategy-advisor, ab-test-suggester, monetization-optimizer) are real, 10K–80K LOC each. — keep verbatim — target: grok-install-v2
- `marketplace/` — Next.js 14 app discovering agents at build time from every `grok-agent.yaml` under `templates/`; agent cards with "Copy install command" button; category filters; Vercel `output: standalone`. — keep verbatim — target: grok-install-v2 (consolidate with `grok-agents-marketplace`)
- `marketplace/scripts/build-agents-index.ts` — Filesystem walker that extracts manifests and generates JSON index. — keep verbatim — target: grok-install-v2
- `extensions/vscode/` — Manifest IntelliSense + lint extension. — keep verbatim — target: archive-as-reference (consolidate with `vscode-grok-yaml`)
- `extensions/browser/` — Chrome/Firefox extension: one-click "grok install this" from X posts; detects YAML in posts. — keep verbatim — target: archive-as-reference
- `.github/workflows/validate.yml` + `actions/validate-manifest/` — 4-job CI (validate manifests, run Constitution scanner, check forbidden phrases, smoke-test CLI) + custom GitHub Action. — keep verbatim — target: grok-install-v2
- `docs/CONSTRAINTS.md`, `docs/PARAMETERIZED_RECIPES.md`, `docs/PROJECT_DNA.md`, `docs/PROMPT_TEMPLATE.md`, `docs/windows-guide.md`, `docs/for-xai-adoption.md`, `docs/annual-report-2026/`, `docs/pitch/` — Hard Six + Soft Rules + forbidden phrases; Recipes A/B/C parameterized templates; project DNA; 7-section prompt structure; Windows install guide; xAI partnership pitch; annual report; pitch deck outline + 60-sec demo. — keep verbatim — target: archive-as-reference (Recipes + Windows guide → grok-install-v2)
- `.streamlit/config.toml` — Streamlit Cloud defaults (theme: dark, primary color: `#FF1E70`, layout: wide). — keep verbatim — target: grok-install-v2
- `llms.txt` — llmstxt.org convention file for AI repo browsing. — keep verbatim — target: grok-install-v2
- `marketing.md` — 110 ready-to-paste X posts (voice + emoji rules documented). — keep verbatim — target: archive-as-reference
- `pyproject.toml` — Root package metadata (Apache-2.0, Python 3.12+). — keep verbatim — target: grok-install-v2

**Detach and do not migrate:** `tools/` (x-platform-toolkit subtree — covered above; pre-existing critical issues), `pulse/` (grok-pulse-mcp-server scaffold — incomplete, `pulse_today` only).

**Strip from public root before any further work:** `CLAUDE.md`, `HANDOFF_LOG.md`, `MERGE_PLAN.md`, `ROADMAP.md` (move to `docs/internal/` or to the meta-repo).

### grok-agents-marketplace

Shipped Next.js 15 marketplace. Cleanest of the application-tier repos. Tier
4 "Spectral" visual identity is freshly landed; tokens flow correctly from
`tailwind.config.ts`; vitest at 17 passing cases including new Plasma/Aurora
variants. **Should anchor v2's storefront** (consolidated with grok-agent's
`marketplace/`).

- `src/lib/brand.ts` — Single source of truth for Spectral palette (Plasma `#FF1E70`, Aurora `#00E0D5`, legacy Cyan/Green). — keep verbatim — target: grok-install-v2
- `tailwind.config.ts` — Complete Spectral token extensions (colors, shadows, keyframes: `nebula-drift`, `chromatic-shift`, `trace-dash`); no hardcoded hex. — keep verbatim — target: grok-install-v2
- `src/lib/visuals/parse-visuals.ts` — Zod-validated `AgentVisualsSchema`; discriminated union for demo_media; safe null fallback on parse failure; accepts plasma/aurora/cyan/green. — keep verbatim — target: grok-install-v2
- `src/lib/visuals/__tests__/parse-visuals.test.ts` — 17 vitest cases covering accents, optional fields, URL validation, edge cases. — keep verbatim — target: grok-install-v2
- `src/components/ui/NeonButton.tsx` — Polymorphic button/link, 6 variants × 3 sizes, Plasma glow + Aurora borders. — keep verbatim — target: grok-install-v2
- `src/components/ui/NebulaBackdrop.tsx` — Pure-SVG backdrop (3 drifting circles, halftone dots, radial gradients); zero JS. — keep verbatim — target: grok-install-v2
- `src/components/ui/CircuitTrace.tsx` — SVG circuit-trace decoration (sparse/dense density). — 2h (minor extraction) — target: grok-install-v2
- `src/components/ui/CertificationBadge.tsx` — Maps 6 certification types (grok-native, safety-max, voice-ready, swarm-ready, action-certified, vscode-verified) to icons + labels. — keep verbatim — target: grok-install-v2
- `src/lib/agents.ts` — Fetches canonical `featured-agents.json` from `awesome-grok-agents` with 10m revalidation; sanitizes via `parseVisuals()`; local mock fallback; server-only. — keep verbatim — target: grok-install-v2
- `src/app/page.tsx` — Landing: hydrates GitHub stars, fetches featured + latest agents, computes totals (installs, 24h, avg safety); RSC + parallel Promise.all. — 3h refit — target: grok-install-v2
- `src/app/marketplace/[id]/page.tsx` — Per-agent detail with `generateStaticParams`, star hydration, AgentPreviewCard, CreatorProfile, InstallOnX, YamlSnippet (Shiki), CertificationBadgeRow, stats pills, 10m ISR. — keep verbatim — target: grok-install-v2
- `src/app/api/track-install/route.ts` — Records install + source (marketplace/vscode/cli/x-intent/copy) to Vercel KV; anon-cookie management; rate-limited. — keep verbatim — target: grok-install-v2
- `src/app/api/stats/public/route.ts` — CORS-enabled public telemetry (agentsInstalled, xPostsGenerated, apiCallsSaved, activeAgents7d, creators); 30s + SWR 120s. — keep verbatim — target: grok-install-v2
- `src/app/api/stats/{daily/[agentId],growth,public,summary}/route.ts` — 5 stats endpoints serving live dashboard data; force-dynamic. — keep verbatim — target: grok-install-v2
- `src/lib/telemetry-store.ts` — Dual-mode telemetry: Vercel KV (sorted sets, 90-day retention) → in-memory fallback; 30 req/min per anon ID. — keep verbatim — target: grok-install-v2
- `migrations/001_telemetry.sql` — Schema for `telemetry_events`; indexes on received_at/event/anon_id/category; 90-day retention. — keep verbatim — target: grok-install-v2
- `src/components/AgentPreviewCard/` — Per-agent demo renderer; supports futuristic/premium/minimal styles × Spectral accents × gif/video/image/auto_generate; Zod-validated upstream. — keep verbatim — target: grok-install-v2
- `src/components/stats/` — 12+ Recharts components (GrowthChart, InstallVolumeChart, SafetyDistribution, CategoryBar, HeatmapGrid, LiveCounters, ImpactStories, TopAgentsBar); Spectral-palette `RechartsTheme`. — keep verbatim — target: grok-install-v2
- `src/components/marketplace/InstallOnX.tsx` — Clipboard + X intent URL; tracks via `trackInstall()`; bump animation. — keep verbatim — target: grok-install-v2
- `src/lib/github.ts` — Octokit wrapper for star counts (1h memoization + stale flag, rate-limit handling). — keep verbatim — target: grok-install-v2
- `src/lib/tracking.ts`, `src/lib/plausible.ts` — Privacy-first Plausible integration (cookieless) for custom events (visuals_block_rendered, install_source). — keep verbatim — target: grok-install-v2
- `src/lib/types.ts` — Central types: Agent, AgentCreator, Certification (6), Category (9), AgentVisuals, AgentDemoMedia discriminated union. — keep verbatim — target: grok-install-v2
- `src/app/sitemap.ts`, `src/app/robots.ts` — Auto-generated SEO. — keep verbatim — target: grok-install-v2
- `vercel.json` — Next.js 15 framework + 3 regions (IAD/FRA/SIN) + env vars + security headers + cache directives. — keep verbatim — target: grok-install-v2
- `next.config.ts` — Minimal: reactStrictMode, typedRoutes, remote image patterns (GitHub avatars, shields.io). — keep verbatim — target: grok-install-v2
- `biome.json` — Strict linter (noUnusedImports, useImportType, useExhaustiveDependencies); 100-char, single quotes. — keep verbatim — target: grok-install-v2
- `vitest.config.ts` — Node env; includes `src/**/__tests__/**` + `tests/**`; alias `@=./src`. — keep verbatim — target: grok-install-v2
- `public/brand/`, `public/icons/` — Spectral-palette SVGs (wordmark, banners, certification icons) recolored to Plasma + Aurora. — keep verbatim — target: grok-install-v2

### Frok

- N/A — Repository contains only `LICENSE`. No code, configuration, or assets to preserve.

## Quick wins — do these in under 30 minutes each

1. **Archive `Frok`** — empty public repo; one click, ecosystem looks one repo less scattered.
2. **Archive `grok-agent-orchestra`** — per its own `MERGE_PLAN.md` it's "merged" into `grok-build-bridge`; one click; if there's appetite, mirror gems into `xlOS` first (see inventory).
3. **Strip `CLAUDE.md`, `HANDOFF_LOG.md`, `MERGE_PLAN.md`, `ROADMAP.md` from `grok-agent` root** — move to `docs/internal/` or to this meta-repo. Single PR, ~10 lines of `git mv`. Loudest amateur signal in the portfolio.
4. **Delete the fake "live" metrics from `grok-build-bridge` `docs/index.html`** (the "1,284 active agents · 99.7% safety pass rate" block) — one block delete, prevents a public-trust incident.
5. **Unify `x-platform-toolkit` README + CI to "12 tools"** — fix the 6 self-contradictions in README and update `scripts/assert-tool-status.mjs`'s `TOOL_STATUS` map; CI stops failing on every push.
6. **Delete `src/`, `dist/`, `vitest.config.ts`, `eslint.config.js`, and all `tests/*.test.ts` from `grok-install-action`** — the dead TypeScript rewrite; one PR. ~3 MB bundle + broken tests gone.
7. **Detach `tools/` and `pulse/` subtrees from `grok-agent`** — both have pre-existing critical issues unrelated to grok-agent's own work; clean detachment lets `tools/` be x-platform-toolkit's problem and `pulse/` archived separately.
8. **Flip `grok-install-brand` LICENSE.md from "All rights reserved" to Apache-2.0** — one-line fix; aligns with the rest of the ecosystem.

## Top 5 credibility risks

1. **Internal scaffolding artifacts leak to public repo roots (5 repos affected).** `MERGE_PLAN.md`, `HANDOFF_LOG.md`, `AUDIT-REPORT.md`, `CLAUDE.md`, and `ROADMAP.md` are visible at the top of `grok-install`, `grok-build-bridge`, `grok-agent`, `grok-agents-marketplace`, and `x-platform-toolkit`. A first-time visitor scanning the repo trees sees the AI-assisted dev process, not the product. **Fix:** strip or move all internal artifacts to `docs/internal/` (or this meta-repo) before any further public work.
2. **`grok-agent`'s jargon density makes legitimate code look like AI word-salad.** "Living Narrative Fabric", "Self-Evolving Personal OS", "Cross-Reality Action Fabric", "Tier 4 Spectral", "Phase 5 Active" sit on top of code that's actually working. The README's "37 production agents live" headline is technically accurate but a busy outsider will read it as inflated. **Fix:** in v2, rename the three flagship Super Agents to literal descriptors (e.g. "multi-source narrative synthesizer", "personal-context briefing agent", "windows + web action runner"), drop "Tier N" / "Phase N" framing on public surfaces, and quote the agent count as "11 fully-implemented agents + 22 creator templates".
3. **`grok-build-bridge`'s landing page shows fabricated telemetry.** `docs/index.html` displays "1,284 active agents" and "99.7% safety pass rate" with a 12px disclaimer. The repo has zero stars and no v0.1.0 tag has shipped — the numbers can't be real. **Fix:** replace with explicitly-framed demo numbers or remove the block entirely (15 minutes); audit every public landing surface in the portfolio for similar fabrications.
4. **`vscode-grok-yaml` ships three incompatible schema-version regimes and ~50% orphan code.** The extension that's supposed to be the IDE companion for "the grok-install standard" contains `apiVersion: const` schemas, `apiVersion: enum` schemas, and `spec_version` schemas in the same VSIX; 18 of 31 schemas aren't even indexed; 8 commands + a 5-file React webview + snippets + grammars are shipped but unregistered in `package.json`. **Fix:** the schema-regime split has to be resolved upstream in `grok-yaml-standards` before v2's IDE extension can be coherent; cut orphan code in the rebuild.
5. **`x-platform-toolkit` claims "20 tools" in 6 places but ships 12 — and CI fails on every push because of it.** README headline, badges, typing animation, table count, Mermaid diagram, and "By The Numbers" all say 20. `scripts/assert-tool-status.mjs` hardcodes 20 entries; CI red on every commit. **Fix:** the quick-win in §6 (above) resolves this in one PR; until then any prospective adopter who reads the README and clicks a missing link bounces.

## Decision-ready summary

Based on this inventory, the recommended path is a **fresh rebuild of two
repos**: `grok-install-v2` consolidates the standard (specs, schemas, safety
floor, CLI, action, IDE extension, brand, docs site, agent gallery, and
marketplace storefront), and `xlOS` consolidates the experimental sibling
(multi-agent orchestration patterns, debate runtimes, the Lucas veto, the
voice/swarm/super-agent surfaces, the live YAML playground). All 14 legacy
repos are then archived — 11 after gem extraction
(`MIGRATE_TO_GROK_INSTALL_V2`), 1 after partial extraction
(`ARCHIVE_AFTER_GEM_EXTRACTION` for `x-platform-toolkit`), and 2 with no
extraction (`ARCHIVE_NO_MIGRATION` for `Frok` and `grok-agent-orchestra`).
`grok-agent` is also `ARCHIVE_AFTER_GEM_EXTRACTION` — the CLI, scanner,
super-agent orchestrators, `grok-paradoxes`, and creator templates migrate;
the jargon, the public-root scaffolding, and the folded-in subtrees do not.

**Phase 0.5** should lock in the two name strategies (final repo names,
final package names, the rename of the three Super Agents off the
"Fabric/OS/Fabric" framing), and resolve the schema-version regime in
`grok-yaml-standards` so that v2's IDE extension, action, and CLI all speak
the same dialect. **Phases 1+** then build, starting from the highest-value
keep-verbatim gems (the `grok-install-cli` Python package, the
`grok-yaml-standards` schemas, the `grok-agents-marketplace` Next.js app,
the `grok-build-bridge` core pipeline, the `grok-install` Cloudflare Worker)
and rebuilding the consumer surfaces (landing pages, builder UI, dashboard,
docs site) on a single unified design system pulled from
`grok-install-brand` and the existing Spectral tokens already shipping in
`grok-agents-marketplace`.
