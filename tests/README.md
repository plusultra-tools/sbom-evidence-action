# tests/

Smoke tests for the composite action.

## Files

| File | Purpose |
|---|---|
| `test_action.yml` | Workflow that stages the fixture SBOM + VEX at the action's default input paths, runs the action with `fail-on-missing-clause: false`, and asserts the three outputs are populated and the manifest SHA-256 matches the runtime hash of `compliance_manifest.json`. |
| `fixtures/cyclonedx-sample.json` | Minimal CycloneDX 1.5 SBOM (`sample-app` 1.0.0 with two transitive components). |
| `fixtures/openvex-sample.json` | Minimal OpenVEX 0.2.0 document with two statements (one `not_affected`, one `fixed`). |

## Local test plan (with `act`)

[`act`](https://github.com/nektos/act) lets you run GitHub Actions locally without
pushing.

```bash
# Validate the workflow YAML parses
act -W tests/test_action.yml --list

# Execute the smoke test against the local checkout
act -W tests/test_action.yml -j test-evidence-action

# If you do not yet have the cra-sbom-evidence CLI on PyPI, the install step in
# action.yml will fail. Bridge by overriding to install from a local wheel:
#
#   act -W tests/test_action.yml -j test-evidence-action \
#       --env CRA_SBOM_WHEEL=/path/to/cra_sbom_evidence-0.0.0-py3-none-any.whl
#
# (the action does not currently honor that env var; this is a manual override
# until the CLI is released — see CHANGELOG.)
```

## Manual test plan (no `act`)

If `act` is not available:

1. Push the repo to a fork on GitHub.
2. Trigger `tests/test_action.yml` via `workflow_dispatch`.
3. Verify the job summary shows the three output values.
4. Download the `cra-evidence-pack` artifact and confirm
   `compliance_manifest.json` is present and its SHA-256 matches the job
   summary.

## CI test plan

`.github/workflows/release.yml` invokes `tests/test_action.yml` as a required
step before publishing a GitHub Release. A failing smoke test blocks the
release.

## What is **not** tested here

- The correctness of CRA-clause mapping itself — that belongs to the upstream
  `cra-sbom-evidence` CLI repo and is covered by its golden-file tests.
- Network-flaky paths (PyPI fetch). If PyPI is down, the test is expected to
  fail; rerun.
