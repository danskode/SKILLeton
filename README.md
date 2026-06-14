# SKILLeton

```
███████   ██   ██   ██████   ██       ██                   █                  
██        ██  ██      ██     ██       ██          ████   █████   ███   ████ 
███████   █████       ██     ██       ██         ██████    █    █   █  █  █ 
     ██   ██  ██      ██     ██       ██         ██        █    █   █  █  █ 
███████   ██   ██   ██████   ███████  ███████     ████     ███   ███   █  █
```

Et åbent bibliotek af skills til [ekte](https://github.com/danskode/ekte) —
den personlige AI developer harness.

## Hvad er en skill?

En skill er en markdown-fil med YAML-frontmatter. SKILLeton kræver **ikke** AI
for at vedligeholdes — det er bare markdown og et YAML-bibliotek, som kan redigeres
med en teksteditor. Det er ekte (med den valgte LLM/agent) der *forbruger* skills.

To begreber, der ikke må forveksles:

- **Installation = permanent.** Når du installerer en skill, gemmes filen i
  `.ekte/skills/` på din lokale ekte-installation. Den bliver liggende og er
  tilgængelig fremover.
- **Aktivering = pr. prompt.** Når du aktiverer en installeret skill med
  `/skills <navn>`, injiceres dens `## System Prompt Addition` i *næste* besked og
  nulstilles bagefter — så en skill aldrig hænger ved utilsigtet. Selve filen
  forsvinder ikke.

## Tilgængelige skills

| Skill | Beskrivelse | Tags | Obligatorisk |
|---|---|---|---|
| `skill-builder` | Opretter nye skills interaktivt via samtale | meta, core | |
| `prd-guide` | Guider fra projektidé til første spec via PRD-format | meta, core, planning | |
| `intent-spec-guide` | Intent-Specs med IDD's sproglige bevidsthed om kontekstformning | meta, core, spec, idd | harness |
| `feature-detector` | Genkender feature-intent og foreslår spec + worktree | meta, core, workflow | |
| `tdd` | Test-drevet udvikling — skriv tests før implementering | testing, quality | |
| `code-review` | Grundig code review med fokus på korrekthed og vedligeholdbarhed | quality, security | |
| `security-review` | Interaktiv OWASP/CWE-analyse med CWE-numre og strukturerede fund | security, quality | |
| `ci-hardening` | GitHub Actions CI — SHA-pinning, Dependabot og Go-testmønstre | ci, security | |
| `secure-harness` | Design af sikre bash-pipelines der sender ikke-betroet indhold til `claude --print` | security, ai_engineering | |
| `safe-flow` | Deterministiske guardrails — feature-branch-flow, pre-commit/pre-push-tjek og CI-gate | ci, security, workflow, guardrails | |
| `pitch-first` | Kritisk feature-review der starter med en pitch (AIDD) | meta, core, planning, aidd | harness |
| `context-layers` | Forklarer ekte's kontekst-konstruktion til læring og fejlfinding | meta, aidd, educational | harness |
| `dreamer` | Reflekterende self-evaluation — finder blinde vinkler i eget output | quality, reflection, meta | |
| `wiki-maintainer` | Drift af simple-minded-wikien — ingest, gap-analyse og lint | wiki, knowledge, core | wiki |
| `java-spring` | Idiomatisk Java + Spring Boot (eksempel på sprogspecifik skill) | java, spring, language | |
| `ui-designer` | UI/UX-designprincipper — hierarki, konsistens, tilgængelighed | design, ui, ux | |

**Obligatoriske skills** auto-installeres af ekte: `harness`-skills altid (AIDD er
præmissen for harnesset), `wiki`-skills når du tilvælger en wiki.

## Installation via ekte

```
/skills library                  # nummereret liste (✓ = allerede installeret lokalt)
/skills show 3                   # læs en skill igennem (nr eller navn) før install
/skills install 1,3,5            # installer flere på én gang (numre eller navne)
/skills update security-review   # hent nyeste version (eller --all)
```

Eller vælg under onboarding når du starter ekte første gang.

## Pakker (bundles)

Installér en hel kategori på én gang i stedet for skill-for-skill:

```
/skills bundle              # se pakker
/skills bundle security     # installer hele security-pakken
```

| Pakke | Indeholder |
|---|---|
| `security` | security-review, secure-harness, safe-flow |
| `ci` | ci-hardening, safe-flow |
| `aidd` | intent-spec-guide, pitch-first, context-layers |
| `java` | java-spring |
| `designer` | ui-designer |

Pakker defineres i `bundles:` i `library.yaml`.

## Bidrag

Pull requests er velkomne. En skill skal:
- Have korrekt YAML-frontmatter (se skill-format nedenfor)
- Have et klart og afgrænset formål
- Tilføjes til `library.yaml`

## Skill-format

```markdown
---
name: min-skill
version: 1.0.0
description: Én sætning der beskriver hvad den gør
tools: [read_file, write_file]
tags: [tag1, tag2]
requires: [harness]   # valgfri — se nedenfor
---

# Titel

## Intent
Hvad løser denne skill?

## Steps
1. ...

## System Prompt Addition
Tekst der injiceres i systemprompten når skill er aktiv.
```

`## System Prompt Addition` er den eneste sektion ekte faktisk injicerer —
resten er dokumentation til mennesker.

## library.yaml

Hver skill registreres i `library.yaml`:

```yaml
version: 1
skills:
  - name: min-skill        # matcher frontmatter name
    description: ...        # vises i /skills library
    version: 1.0.0          # matcher frontmatter version — bruges til /skills update
    tags: [tag1, tag2]
    requires: [harness]     # valgfri: obligatorisk skill. "harness" = altid
                            #          (AIDD-præmis), "wiki" = når wiki tilvælges
    file: skills/min-skill.md
```

Hold `version` i `library.yaml` i sync med skill'ens frontmatter `version`, så
ekte kan vise "opdatering tilgængelig" og `/skills update` virker korrekt — bump
begge samtidig når du ændrer en skill.
