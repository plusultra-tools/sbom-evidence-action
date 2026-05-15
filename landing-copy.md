# landing-copy.md — sbom-evidence-action

Copy blocks for the Marketplace listing description, the README hero, and the
optional `cra-evidence.eu` landing page (Carrd). Tone: dev-to-dev, blunt, no
marketing fluff. Spanish translation deferred until a Spanish buyer asks.

---

## Hero (Marketplace listing, ≤160 chars)

> Drop-in GitHub Action that turns your SBOM + VEX into a CRA Article 14 evidence
> pack. One line. Per release. MIT.

## Subhead (≤280 chars)

> The EU Cyber Resilience Act starts enforcement 11 Sep 2026. Article 14 demands
> a 24h vulnerability notice. This Action wraps the `cra-sbom-evidence` CLI so
> every CI run leaves a hash-chained, clause-cited evidence pack in your artifacts.

---

## One-liner pitch (HN / dev.to / X)

> CRA Article 14 wants a 24-hour vulnerability notice. `uses: plusultra-tools/sbom-evidence-action@v1`
> is one line away. MIT, no telemetry.

---

## Carrd landing (cra-evidence.eu) — sections

### H1
**Stop assembling CRA Article 14 evidence by hand.**

### Sub
The EU Cyber Resilience Act forces every product-with-digital-elements vendor in
the EU to ship a vulnerability-disclosure evidence trail by **11 September 2026**.
Your SBOM tool already exists. Your VEX tool already exists. What's missing is
the *evidence pack* — that's what we generate, per CI run, in one line.

### How (3-step)

1. Keep your SBOM step (`anchore/sbom-action`, `cyclonedx-cli`, whatever).
2. Add `plusultra-tools/sbom-evidence-action@v1` after it.
3. Pin the artifact (`cra-evidence-pack`) to your release.

### Code block (the actual selling point)

```yaml
- uses: anchore/sbom-action@v0
- uses: aquasecurity/trivy-action@master
  with: { format: openvex, output: vex.json }
- uses: plusultra-tools/sbom-evidence-action@v1
```

### What you get
- `compliance_manifest.json` — clause → SBOM-field map, hash-chained.
- `psirt_template/` — Article 14(2) 24h-notice template, pre-filled from your VEX.
- `audit.json` — append-only SHA chain anchoring every input file.
- All MIT. All on your runner. No telemetry. No vendor lock.

### Pricing
- **The Action: free, MIT, forever.**
- **Hosted history & alerts:** €19–49/mo. Email me for early access.
- **Enterprise / on-prem evidence vault:** drop a line, we'll quote.

### Why us (one sentence)
Three commercial CRA platforms exist (craevidence.com, prismor.dev, sbomify) —
**none of them cite the CRA articles verbatim in the output.** Ours does.
That's the difference between an evidence pack and a glossy PDF.

### CTA
- **Star the repo:** github.com/plusultra-tools/sbom-evidence-action
- **Get notified when hosted launches:** [email signup]
- **Read the CRA mapping:** [link to clause table]

---

## Comment-template for HN launch (Show HN)

Title: `Show HN: A GitHub Action that emits a CRA Article 14 evidence pack`

Body:
> We needed to ship a per-release evidence trail for the EU Cyber Resilience Act
> (Article 14, 24h vulnerability notice, enforcement starts 11-Sep-2026). Every
> commercial tool quoted €8K-250K/year minimum. The OSS layer (Syft, Trivy,
> cyclonedx-cli) gets you the SBOM and the VEX but stops there.
>
> This Action wraps a small CLI that takes the SBOM + VEX and emits the evidence
> pack: a hash-chained manifest mapping every CRA clause to its evidence fields,
> a pre-filled 24h-notice template, and an audit log. MIT, runs on your runner,
> no telemetry.
>
> Caveats / what it does NOT do: doesn't generate SBOMs, doesn't generate VEX,
> doesn't scan code. Use the upstream OSS for that. This is the missing
> downstream packager.
>
> Repo + example workflow: github.com/plusultra-tools/sbom-evidence-action
> CLI it wraps: pypi.org/project/cra-sbom-evidence
>
> Would love feedback from anyone actually doing CRA prep — especially on the
> clause mapping (Articles 13, 14, 15, 31, 32, 64, 71 + Annex I/III).

---

## Reddit (r/cybersecurity / r/embedded / r/sbom)

Title: `[OSS] CRA Article 14 evidence pack as a GitHub Action — one line in your workflow`

Body: same as HN, drop the "Show HN" framing, lead with the deadline.
