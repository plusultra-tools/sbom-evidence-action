# kill-gate.md — sbom-evidence-action

**Venture:** sbom-evidence-action (Wave 2 #12)
**Distribution channel for:** `cra-sbom-evidence` CLI (Wave 2 #2)
**Launch date placeholder:** D0 = day the Marketplace listing goes live (pinned tag `v1`).
**Hard deadline this rides:** CRA Article 14 — 2026-09-11.

## d+30 thresholds (any ONE = continue; all miss = fold)

- ≥ **30** GitHub stars on this repo (`plusultra-tools/sbom-evidence-action`).
- ≥ **10** Marketplace installs (sum of unique repos consuming `uses: plusultra-tools/sbom-evidence-action@v1` — measure via GitHub code-search or, when available, Marketplace insights API).
- ≥ **3** real-affiliation issues opened (i.e. the issue author has a non-trivial GitHub profile with company/affiliation visible — drive-by drive-by spam doesn't count).
- ≥ **1** inbound (DM, email, OSS thread, conference invite, or contributor PR with stated production use).

## How to measure

| Metric | Source | Cadence |
|---|---|---|
| Stars | GitHub API `GET /repos/plusultra-tools/sbom-evidence-action` field `stargazers_count` | daily, snapshot to `state/launch-monitor.csv` |
| Marketplace installs | GitHub code-search `"uses: plusultra-tools/sbom-evidence-action"` deduped by repo | weekly |
| Real-affiliation issues | manual review of issue authors at d+14 and d+28 | per-touchpoint |
| Inbound | inbox `from-principal/` + GitHub notifications + email | rolling |

## d+14 mini-gate (budget protection)

At d+14, if **all** of the below are true → halt any paid promotion, run a one-agent
redteam pass on positioning and channel, then decide pivot vs continue:
- ≤ 5 stars
- 0 Marketplace installs
- 0 issues

Inherits the same protection rationale as the underlying `cra-sbom-evidence` kill-gate
(`state/pivot-plan-d30-cra-sbom.md` §D14).

## Fold action

If kill-gate fires red on d+30:
- Archive repo (transfer to `archive/sbom-evidence-action/`) with a `post-mortem.md`.
- Mark Marketplace listing as **archived** (GitHub still serves consumers of pinned
  tags, no breaking change for the ≤10 known consumers).
- Mirror lessons to `archive/<parent-venture>/post-mortem.md`.

## Bound conditions

- If `cra-sbom-evidence` CLI is itself killed before d+30, this action is **dead by
  parent** — fold immediately regardless of own metrics.
- If `cra-sbom-evidence` CLI is still pre-launch at d+30, the action's d+30 clock
  restarts on CLI's v0.1.0 PyPI release date.
