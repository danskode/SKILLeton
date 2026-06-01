# SKILLeton

```
███████   ██   ██   ██████   ██       ██                                    
██        ██  ██      ██     ██       ██          ████   █████   ███   █   █
███████   █████       ██     ██       ██         ██████    █    █   █  ████ 
     ██   ██  ██      ██     ██       ██         ██        █    █   █  █  ██
███████   ██   ██   ██████   ███████  ███████     ████     ███   ███   █   █
```

```
            ,-,,-,   __
     ______/     /_,'  |
     \________________/
          |\) (/ |
       (  | oo   |
        ) `|  |--'
       (___^^^^|
          (____'
```

Et åbent bibliotek af skills til [ekte](https://github.com/danskode/ekte) —
den personlige AI developer harness.

## Hvad er en skill?

En skill er en markdown-fil med YAML-frontmatter der tilføjer et system-prompt
til næste LLM-besked i ekte. Skills gælder kun for ét prompt og nulstilles
automatisk bagefter.

## Tilgængelige skills

| Skill | Beskrivelse | Tags |
|---|---|---|
| `skill-builder` | Opretter nye skills interaktivt via samtale | meta, core |
| `prd-guide` | Guider fra projektidé til første spec via PRD-format | meta, core, planning |
| `feature-detector` | Genkender feature-intent og foreslår spec + worktree | meta, core, workflow |
| `tdd` | Test-drevet udvikling — skriv tests før implementering | testing, quality |
| `code-review` | Grundig code review med fokus på korrekthed og vedligeholdbarhed | quality, security |
| `security-review` | Interaktiv OWASP/CWE-analyse med CWE-numre og strukturerede fund | security, quality |
| `ci-hardening` | GitHub Actions CI — SHA-pinning, Dependabot og Go-testmønstre | ci, security |
| `secure-harness` | Design af sikre bash-pipelines der sender ikke-betroet indhold til `claude --print` | security, ai_engineering |

## Installation via ekte

```
/skills catalog                  # se alle tilgængelige skills
/skills install security-review  # installer en specifik skill
```

Eller vælg under onboarding når du starter ekte første gang.

## Bidrag

Pull requests er velkomne. En skill skal:
- Have korrekt YAML-frontmatter (se eksisterende skills)
- Have et klart og afgrænset formål
- Tilføjes til `catalog.yaml`
- Gælde kun næste prompt — ikke ændre agentens adfærd permanent

## Skill-format

```markdown
---
name: min-skill
version: 1.0.0
description: Én sætning der beskriver hvad den gør
tools: [read_file, write_file]
tags: [tag1, tag2]
---

# Titel

## Intent
Hvad løser denne skill?

## Steps
1. ...

## System Prompt Addition
Tekst der injectes i systemprompt når skill er aktiv.
```
