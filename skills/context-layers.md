---
name: context-layers
version: 1.0.0
description: Forklarer ekte's kontekst-konstruktion til læring og fejlfinding (AIDD)
tools: []
tags: [meta, aidd, educational]
---

# Context Layers

## Intent

Hjælper brugeren med at forstå og optimere de kontekst-lag ekte injicerer i
agentens prompt. Relevant når agenten opfører sig uventet, eller når man vil
lære om harness engineering og AIDD i praksis.

## Steps

1. Forklar hvilke lag der er aktive
2. Vis token-estimater per lag (/context)
3. Forklar hvad hvert lag gør og hvornår det ændres

## System Prompt Addition

Du er i "context-layers" mode. Når brugeren spørger om din adfærd, kontekst
eller hvorfor du svarer som du gør, forklar de tre lag:

**KONTEKSTVINDUE** — hvad modellen ser i denne session:
- Identitet: baseSystemPrompt (hvem du er, dansk, præcis og direkte)
- Projekt: ekte.md (projektbeskrivelse, mål, konventioner)
- Hukommelse: .ekte/memory/ og ~/.ekte/memory/ (noter der loades ved opstart)
- Skill: aktiv skill's system prompt addition (ét prompt ad gangen, nulstilles)
- Historik: samtalehistorik (maks 20 beskeder sendes til LLM)

**VIDENSLAGER** — forespørges manuelt:
- simple-minded wiki (./wiki/) — tilgås med /wiki "spørgsmål"

**HUKOMMELSE** — persisterer på tværs af sessioner:
- .ekte/memory/ (projekt-lokal)
- ~/.ekte/memory/ (global — følger med til alle projekter)
- Gemmes med /remember <tekst>

Brug /context for at se aktuelle token-estimater per lag.

AIDD-forbindelsen: kontekstvinduet er det du eksternaliserer. Alt hvad agenten
ikke kan se, er en transitiv tilstand — noget du ved men ikke har sagt. Hukommelse
og wiki er redskaber til at eksternalisere disse tilstande systematisk.
