# CLAUDE.md

Vejledning til enhver agent eller LLM, der arbejder i dette repo (Claude Code, ekte eller andet).

## Hvad er dette repo

SKILLeton er et åbent bibliotek af skills til [ekte](https://github.com/danskode/ekte). Der er ingen build, test eller linting — kun markdown-filer og et YAML-katalog.

SKILLeton kræver **ikke** AI for at vedligeholdes: en skill er en almindelig markdown-fil, og `catalog.yaml` er et almindeligt indeks. Det er ekte (med den LLM/agent brugeren har valgt) der *forbruger* skills ved at injicere `## System Prompt Addition` — selve biblioteket er agnostisk over for hvilken model eller agent der bruger det. Du kan tilføje og redigere skills med en teksteditor alene.

## Tilføj en ny skill

1. Opret `skills/<kebab-case-navn>.md` med korrekt frontmatter (se format nedenfor)
2. Tilføj en entry i `catalog.yaml` med `name`, `description`, `tags` og `file`

Det er alt — ingen CI, ingen registrering andre steder.

## Skill-format

```markdown
---
name: kebab-case-navn
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
Tekst der injiceres som system-prompt i ekte når skill er aktiv.
```

`## System Prompt Addition` er den eneste sektion ekte faktisk injicerer — resten er dokumentation til mennesker. Skills gælder kun for ét prompt og nulstilles automatisk bagefter.

## catalog.yaml-struktur

```yaml
version: 1
skills:
  - name: skill-navn        # matcher frontmatter name
    description: ...        # vises i /skills catalog i ekte
    tags: [tag1, tag2]
    file: skills/skill-navn.md
```
