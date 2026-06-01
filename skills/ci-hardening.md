---
name: ci-hardening
version: 1.0.0
description: GitHub Actions CI — opsætning, SHA-pinning, Dependabot og diagnostik
tools: [read_file, write_file]
tags: [ci, security, quality]
---

# CI Hardening

## Intent

Hjælper med at opsætte eller hærde GitHub Actions CI efter best practices:
supply chain-sikring via SHA-pins, automatiske opdateringer via Dependabot,
og Go-specifikke test-mønstre. Kan også diagnosticere fejlende workflows.

## Steps

**Opsætning:**
1. Læs eksisterende `.github/workflows/` og `.github/dependabot.yml` hvis de findes
2. Identificér hvilke actions der bruges og om de er SHA-pinned
3. Slå aktuelle commit-SHAs op for ikke-pinnede actions
4. Skriv/opdatér workflow og dependabot-konfiguration

**Diagnostik:**
1. Læs den fejlende workflow-fil
2. Identificér fejlstedet ud fra brugerens beskrivelse af Actions-loggen
3. Foreslå rettelse

## System Prompt Addition

Du arbejder nu med GitHub Actions CI. Følg disse principper:

**Supply chain-sikring:**
- Pin alle actions til commit-SHA, aldrig til mutable tags (`@v4` → `@abc123... # v4`)
- Format: `uses: owner/action@<40-char-sha> # v<tag>`
- SHAs hentes med: `gh api repos/owner/action/git/ref/tags/v4 --jq '.object.sha'`
  (hvis `.object.type == "tag"`, dereferér med `gh api repos/owner/action/git/tags/<sha>`)
- Dependabot holder SHAs opdaterede automatisk med ugentlige PRs

**Go CI-mønster:**
```yaml
- uses: actions/setup-go@<sha> # v5
  with:
    go-version-file: go.mod
    cache: true
- run: go vet ./...
- run: go test -v -race ./...
```

**Dependabot-konfiguration:**
```yaml
version: 2
updates:
  - package-ecosystem: github-actions
    directory: /
    schedule:
      interval: weekly
    groups:
      actions:
        patterns: ["*"]
  - package-ecosystem: gomod
    directory: /
    schedule:
      interval: weekly
```

**Ingen secrets i CI til AI-kald** — brug lokale scripts med installeret CLI i stedet.

Forklar altid *hvorfor* en ændring øger sikkerheden, ikke kun hvad der ændres.
