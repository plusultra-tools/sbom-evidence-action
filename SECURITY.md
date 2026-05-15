# Security policy

## Reporting a vulnerability

If you find a vulnerability in **sbom-evidence-action** itself (the wrapper
action; not the underlying `cra-sbom-evidence` CLI, not the SBOM/VEX tools we
chain to):

- **Email:** plusultra.dev@proton.me (PGP key published at
  https://plusultra-tools.eu/.well-known/pgp.asc — fingerprint pinned in this
  repo's release notes once the key is generated).
- **GitHub Security Advisory:** open a private advisory at
  https://github.com/plusultra-tools/sbom-evidence-action/security/advisories/new
  (preferred for vulnerabilities where coordinated disclosure is needed).

Do **not** open public issues for security reports.

## Response SLA

We commit to the timelines the CRA itself sets (Article 14):

| Event | Our action |
|---|---|
| Receipt of vulnerability report | Acknowledge within **24 hours**. |
| Triage decision | Within **72 hours**. |
| Fix or mitigation published | Within **14 days** for high/critical, **30 days** for medium, best-effort for low. |

If an *actively exploited* vulnerability is reported, we will follow the
Article 14(2) 24-hour early-warning template that this very action generates.

## Scope

In scope:
- `action.yml` and any composite-step shell scripts.
- `tests/` workflow code that runs on user infrastructure (`act`-runnable).

Out of scope:
- Vulnerabilities in `actions/setup-python`, `actions/upload-artifact`, etc. —
  report those upstream to the respective owners.
- Vulnerabilities in `cra-sbom-evidence` CLI — see its own repo's `SECURITY.md`.
- Vulnerabilities in user-supplied SBOM / VEX files (those are inputs we trust).

## Supported versions

| Version | Status |
|---|---|
| `v1.x` | Supported. |
| `v0.x` | Best-effort only; expected to be superseded once `v1.0.0` ships. |
| Pre-release tags | Not supported. |

## Hall of fame

Coming soon — we'll list reporters here (with permission) once first valid report
lands.
