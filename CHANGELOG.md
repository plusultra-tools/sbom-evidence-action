# Changelog

All notable changes to **sbom-evidence-action** will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial composite action scaffold.
- Inputs: `sbom-path`, `vex-path`, `output-dir`, `fail-on-missing-clause`,
  `cra-sbom-version`, `upload-artifact`.
- Outputs: `evidence-pack-path`, `manifest-sha256`, `clauses-covered`.
- Job-summary block written via `$GITHUB_STEP_SUMMARY`.
- Workflow-artifact upload step (optional, opt-out via `upload-artifact: false`).
- `tests/test_action.yml` workflow exercising the action against a fixture pair.
- `tests/fixtures/cyclonedx-sample.json` minimal CycloneDX 1.5 fixture.
- `tests/fixtures/openvex-sample.json` minimal OpenVEX fixture.
- `.github/workflows/release.yml` — tag-triggered release pipeline.

### Notes
- This release is **scaffold-only** and depends on the not-yet-published
  `cra-sbom-evidence` PyPI package. Pin `cra-sbom-version` once the upstream CLI
  ships its first stable tag.

---

## [0.1.0] — TBD

First public release. To be tagged once the underlying `cra-sbom-evidence` CLI
publishes its `0.1.0` to PyPI and we have at least one passing end-to-end run
through `.github/workflows/release.yml`.
