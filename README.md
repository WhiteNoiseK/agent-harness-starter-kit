# Harness Starter Kit

> **This folder is not "a project" — it is "how to start a project."**
> When you begin a new project, clone this folder and you start out with a proven development process,
> a 6-stage quality gate, a Foam knowledge base, and drift locking already in place **from day one**.
> It exists so the next project never has to re-learn what was learned through trial and error.

This kit was extracted from a **real instrumentation-software project** from which this methodology emerged.
That project was both the most exemplary case of this methodology — and at the same time the one that most
clearly showed *what was left until too late and had to be revisited*. The kit captures both:
**what worked becomes the defaults**, and **what hurt to learn becomes gates and locks**.

---

## 0. The 3 Problems This Kit Solves

| # | Problem (actually experienced on the original project) | The kit's solution |
|:--|:--|:--|
| 1 | **Contracts frozen later than the code** → "feeling like going back to square one" (starting M1 with formulas undecided, then switching CSV→DB) | **Gate P (3-contract freeze gate)** — freeze the data-definition, output-sink, and identifier/unit contracts before the pipeline begins, or isolate them behind a facade |
| 2 | **The standard process living only as tacit knowledge** → trial and error repeated every time | **`docs/pm-guide/lifecycle-standard.md` + `PHASE_GATES.md`** — an explicit 8-stage gate model (PMBOK + Research + Feasibility) |
| 3 | **"Something drifts a little every time I start, but I can't tell what"** | **`docs/pm-guide/DRIFT_LOCK.md` + `.harness.toml` + `.harness/baseline/`** — identify and pin down 22 kinds of drift, and after cloning use a baseline diff to make "exactly what changed" visible |

---

## 1. How to Start a New Project (clone-and-go)

> 🚀 **The fastest path — one sentence to your agent**: copy the kickoff prompt from [START_HERE.md](START_HERE.md)
> and tell the agent "apply this kit to my project," and it will follow the activation runbook as written. The manual procedure follows below.

```
1. Copy this folder to the new project location     (cp -r harness-starter-kit/ my-new-project/)
2. python scripts/harness_init.py                   ← the single bootstrap (a nice-to-have settling step; nothing breaks without it)
     - Enter SRC_ROOT / TESTS_ROOT / SCORES_DIR / TASK_ID_REGEX / coverage & audit thresholds → creates/updates .harness.toml
     - Snapshot .harness/baseline/ (the drift-diff reference)
     - Confirm the PreToolUse commit-guard hook is registered + verify global prereqs exist → fail loudly if missing
   * The scripts read .harness.toml **at runtime** and fall back to sensible defaults when it is absent —
     so init is not a single point of failure (stability-first principle). Only the paths in pyproject.toml are read
     statically by the tools, so if your layout is not src/tests, edit pyproject.toml directly.
3. Work through the "placeholders to fill in" checklist in TEMPLATE_MANIFEST.md
4. Read docs/pm-guide/lifecycle-standard.md and start from Stage 0 (Research)
```

> Even before the bootstrap, the scripts work with sensible defaults (so init never becomes a single point of failure).

## 2. When You Lose Context (cold restart)

Type `context check` and the agent reads in a fixed order and gives a 3–5 line summary:
`docs/index.md → _recent.md → _authority.md → _field_cascade.md → ai-workflow/progress.md → git status`.
This Foam wiring is the "start gate" — as the content fills in, the catalogs are generated automatically
(`python scripts/foam_catalog.py`).

## 3. Folder Map

For the full file list, roles, and placeholders to fill, see **[TEMPLATE_MANIFEST.md](TEMPLATE_MANIFEST.md)**.

```
harness-starter-kit/
├── README.md                  ← this document
├── START_HERE.md              ← 🚀 agent activation manual + kickoff prompt (for both humans and agents)
├── TEMPLATE_MANIFEST.md       ← full file inventory + roles + placeholder checklist + global prereqs
├── .mcp.json.example          ← shared team MCP config template (copy → .mcp.json)
├── .harness.toml              ← the single config seam (read by every script/agent — the core of drift locking)
├── pyproject.toml             ← single home for tool config ([tool.mypy/ruff/black/coverage/pytest])
├── docs/
│   ├── pm-guide/              ← macro process + behavior/policy layer
│   │                            (lifecycle-standard · PHASE_GATES · STAGE_DEFINITION_RISKS · DRIFT_LOCK · recommendation_policy · ProductProposal)
│   ├── _harness/             ← 6-stage gate spec (vendored, self-contained)
│   ├── ai-workflow/          ← empty Phase-deliverable templates (research/plan/progress/...) + scores/reviews/handoffs
│   ├── engineering/          ← FREEZE-gate contract templates (_TEMPLATE_data_spec / _erd / _identifier_unit_contract / _assumption_leak_audit)
│   ├── experiments/          ← Feasibility spike report template
│   ├── coding-convention/    ← per-language conventions (Python always, JS/C optional)
│   ├── ai-tooling/           ← AI_TOOLING.md (agents · commands · skills · MCP predefinitions · installation)
│   ├── ENVIRONMENTS.md       ← per-environment (IDE/terminal/app) optimization + settings scope
│   └── retrospective/        ← Phase 6 retrospective (single canonical copy)
├── .claude/                   ← harness agents (test-writer/impl-coder/refactor-fixer/score-auditor) + commands + settings (hooks only)
├── scripts/                   ← harness_init · gate_check · run_verify · audit_rerun · status · foam_catalog · field_cascade
├── tests/                     ← unit/integration/e2e layout + reusable fixtures + hardware mock templates + harness self-test
├── .github/workflows/         ← CI (the same gates as the local hooks)
└── .harness/baseline/         ← snapshot taken at clone time (the drift-diff reference)
```

## 4. Global Prerequisites

This kit is half of a system. The other half (the 6-stage thresholds, the permission matrix, some review agents)
depends on the `~/.claude/` global layer. We secured self-containment by **vendoring `docs/_harness/quality-gates.md`**,
but the Stage 4 reviewers (code-reviewer · security-reviewer) and planner/architect are
still global agents. For the exact list of prerequisites and an existence check, see **TEMPLATE_MANIFEST.md §Global Prerequisites**.

## 5. License & Credits

- **License**: [MIT](LICENSE) — © 2026 WhiteNoiseK. Use, modify, and distribute freely (just keep the copyright notice).
- **Methodology — VHCP**: a **development methodology defined by WhiteNoiseK** (fusing PMBOK-based project management + SW Agile + the characteristics of working with AI). The 8-stage lifecycle in this kit is an extension of VHCP.
- **Code**: generated with Claude (Anthropic) (AI-assisted). Designed assuming a **Claude Code** environment.
- **Recommended companion — ECC (Everything Claude Code)**: by Affaan Mustafa, MIT license. This kit does **not bundle** ECC; it only points to the install path ([docs/ai-tooling/AI_TOOLING.md](docs/ai-tooling/AI_TOOLING.md)). Using them together reinforces your global agents/skills.
- **Reference standards**: PMBOK (PMI), V-model, ISO/IEC/IEEE 12207 — only the names and process groups are referenced (the source texts are not included).
- This kit was extracted and validated from one real instrumentation-software project, and all product-identifying information has been replaced with neutral examples.
