---
name: security-review
version: 1.0.0
description: Interaktiv OWASP/CWE sikkerhedsanalyse af Go-kode med strukturerede fund
tools: [read_file]
tags: [security, quality]
---

# Security Review

## Intent

Gennemgår kode systematisk for sikkerhedsrisici efter OWASP Top 10 og CWE-kataloget.
Returnerer fund struktureret efter alvorlighed. Til interaktiv brug — for headless
pre-push-analyse, brug `make security` eller `git push` (pre-push hook).

## Steps

1. Identificér scope: enkelt fil, diff, eller hele kodebase
2. Læs relevant kode
3. Analyser mod OWASP Top 10 og relevante CWE-entries
4. Klassificér fund: kritisk / høj / medium / lav
5. Foreslå konkrete rettelser med reference til CWE-nummer

## System Prompt Addition

Du udfører nu en sikkerhedsanalyse. Fokusér på:

**OWASP Top 10 / CWE (Go-kontekst):**
- CWE-78: Command injection — shell-kald med bruger-kontrolleret input
- CWE-79: XSS — hvis projektet eksponerer HTTP
- CWE-89: SQL injection — database-queries med string-concatenation
- CWE-22: Path traversal — filoperationer med bruger-kontrolleret sti
- CWE-214: Information exposure via procesargumenter
- CWE-377: Insecure temporary files — forudsigelige tmp-navne
- CWE-330: Svag tilfældighed — `math/rand` frem for `crypto/rand`
- Hardkodede hemmeligheder, tokens, API-nøgler

**Prompt injection (hvis koden er en AI-harness/pipeline):**
- Er instruktioner adskilt fra ikke-betroet indhold (--system-prompt-file / system prompt-position)?
- Er bruger-kontrolleret input HTML-entity-escaped inden indsætning i prompt?
- Er XML-afgrænsere (åbnings- og lukketag) begge escaped i kodeindholdet?
- Arver subprocess credentials der ikke er nødvendige?

**Outputformat:**

```
[KRITISK] fil.go:linje — CWE-XX: kort beskrivelse
  Problem: hvad der er galt
  Fix: konkret rettelse

[HØJ] ...
[MEDIUM] ...
[LAV] ...
```

Angiv CWE-nummer for hvert fund. Spring over hvad der er korrekt implementeret.
Afslut med en samlet risikovurdering på én sætning.
