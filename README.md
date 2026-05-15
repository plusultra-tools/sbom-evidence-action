# sbom-evidence-action

> Drop-in GitHub Action that turns your CI's SBOM + VEX into a **CRA Article 14** evidence pack.
> One line in your workflow, one signed manifest per release.

[![Marketplace](https://img.shields.io/badge/marketplace-CRA%20SBOM%20Evidence-blue)](https://github.com/marketplace/actions/cra-sbom-evidence)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## Why this exists

The **EU Cyber Resilience Act** (Regulation (EU) 2024/2847, in force since 2024-12-10) makes
two parts of compliance brutal for small vendors:

- **Article 14 — 24h / 72h / 14d reporting deadlines.** A vulnerability that is actively
  exploited triggers an ENISA early-warning at 24 hours. Manufacturers without a
  pre-built evidence trail cannot meet that clock from a cold start.
- **Article 31 — Technical Documentation** must be available *per release*, not per audit.
  Bolting it on once a year is no longer compliant after **2026-09-11**.

Existing tools (Black Duck, Cycode, FossID) start at €8–250K/year. The OSS layer
(`anchore/syft`, `aquasecurity/trivy`, `cyclonedx-cli`) generates SBOMs and VEX docs
but stops there — *nobody packages the evidence per CRA clause*.

This Action wraps the [`cra-sbom-evidence`](https://pypi.org/project/cra-sbom-evidence/)
CLI so you get a hash-chained evidence pack out of every CI run, citing CRA clauses
verbatim. MIT, no telemetry, runs entirely on your runner.

---

## Usage

### Minimal (5 lines)

```yaml
- uses: anchore/sbom-action@v0          # produces sbom.json
- uses: plusultra-tools/sbom-evidence-action@v1
  with:
    sbom-path: sbom.json
```

That's it. The action installs `cra-sbom-evidence`, runs it, uploads
`cra-evidence-pack` as a workflow artifact, and writes a job-summary block with
the manifest SHA-256.

### With manifest pinned to S3 (drop-in for compliance archives)

```yaml
- uses: plusultra-tools/sbom-evidence-action@v1
  id: cra
  with:
    sbom-path: sbom.json
    vex-path: vex.json
- run: |
    aws s3 cp ${{ steps.cra.outputs.evidence-pack-path }} \
      s3://compliance-archive/${{ github.repository }}/${{ github.sha }}/ \
      --recursive
```

### With auto-comment on every PR

```yaml
- uses: plusultra-tools/sbom-evidence-action@v1
  id: cra
  with:
    sbom-path: sbom.json
    vex-path: vex.json
- uses: actions/github-script@v7
  if: github.event_name == 'pull_request'
  with:
    script: |
      const body = `**CRA evidence pack**\n\n` +
        `- Manifest SHA-256: \`${{ steps.cra.outputs.manifest-sha256 }}\`\n` +
        `- Clauses covered: ${{ steps.cra.outputs.clauses-covered }}\n` +
        `- Artifact: \`cra-evidence-pack\` (see workflow run artifacts)`;
      github.rest.issues.createComment({
        owner: context.repo.owner,
        repo: context.repo.repo,
        issue_number: context.issue.number,
        body,
      });
```

---

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `sbom-path` | no | `sbom.json` | CycloneDX 1.5 or SPDX 2.3 SBOM (JSON). |
| `vex-path` | no | `vex.json` | OpenVEX or CSAF 2.0 VEX (JSON). Skipped if file absent. |
| `output-dir` | no | `cra-evidence/` | Directory where the evidence pack is written. |
| `fail-on-missing-clause` | no | `true` | Fail the step if any required CRA clause cannot be mapped. |
| `cra-sbom-version` | no | `latest` | Pin a specific `cra-sbom-evidence` PyPI version (e.g. `0.1.0`). |
| `upload-artifact` | no | `true` | Upload the evidence pack as a workflow artifact. |

## Outputs

| Name | Description |
|---|---|
| `evidence-pack-path` | Absolute path of the generated evidence-pack directory on the runner. |
| `manifest-sha256` | SHA-256 of `compliance_manifest.json` (the hash-chain root). |
| `clauses-covered` | Comma-separated list of CRA clauses present in the manifest. |

---

## What this DOES NOT do

This action is intentionally a **downstream packager**. It does not duplicate the
mature OSS that already exists upstream:

- It does **not** generate the SBOM. Use [`anchore/sbom-action`](https://github.com/anchore/sbom-action)
  or [`CycloneDX/gh-node-module-generatebom`](https://github.com/CycloneDX/gh-node-module-generatebom).
- It does **not** generate the VEX. Use [`aquasecurity/trivy-action`](https://github.com/aquasecurity/trivy-action)
  with `--format openvex` or `openvex/vexctl`.
- It does **not** scan code for vulnerabilities. Use Trivy, Grype, or Snyk.
- It does **not** sign artifacts. Pair with [`sigstore/cosign-installer`](https://github.com/sigstore/cosign-installer).
- It does **not** ship a hosted dashboard (yet — see *Pricing* below).

It **does** one thing: take an SBOM + VEX, emit a hash-chained evidence pack
mapped to CRA Articles 13, 14, 15, 31, 32, 64, 71 + Annex I/III, with verbatim
clause citations.

---

## Example: full upstream chain

See [`example-workflow.yml`](./example-workflow.yml) for a complete pipeline
that wires `anchore/sbom-action` → `aquasecurity/trivy-action` → this action.

---

## Pricing

- **This action: MIT, free forever.** No telemetry, no signup, no rate limit. Run it
  on your own runners; the binary is a normal PyPI package.
- **Hosted evidence-pack history (planned, €19–49/mo via Stripe):** retains every
  manifest, indexes by repo + commit + clause, emits monthly compliance digests,
  and triggers Slack/Teams alerts on Article 14(2) 24-hour notice events.
  Sign up for early access at [cra-evidence.eu](https://cra-evidence.eu) (landing
  page; production launch follows the public roadmap of the underlying CLI).

The OSS path is the product. Hosting is a convenience for teams who do not want
to manage their own archive.

---

## Marketplace listing

This repo is published to the
[GitHub Actions Marketplace](https://github.com/marketplace?type=actions) under
the name **CRA SBOM Evidence**. Marketplace requirements:

- Repository is public.
- `action.yml` lives at the repo root and contains a `name`, `description`,
  `branding.icon`, and `branding.color` — all present in this repo.
- A semver-tagged release exists (`v0.1.0`, `v1`, …). See
  [`.github/workflows/release.yml`](./.github/workflows/release.yml) for the
  release automation.
- The author account is verified by GitHub. (Done at publication time.)

To consume a pinned version, prefer:

```yaml
uses: plusultra-tools/sbom-evidence-action@v1   # major-tag, auto-updates within v1.x
# or
uses: plusultra-tools/sbom-evidence-action@v1.0.0  # exact pin
```

Pinning to a commit SHA is supported and recommended for regulated environments:

```yaml
uses: plusultra-tools/sbom-evidence-action@<40-char-sha>
```

---

## License

[MIT](./LICENSE). No copyleft, no patent landmines, no attribution required in
the evidence pack itself. (We'd appreciate a link back if it saved your
compliance team a week, but that's it.)

## Security

See [`SECURITY.md`](./SECURITY.md) for responsible-disclosure contact and our
own CRA Article 14 notice procedure (yes, we eat our own dog food).
