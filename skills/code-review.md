---
name: code-review
version: 1.0.0
description: Grundig code review med fokus på sikkerhed og kvalitet
tools: [read_file]
tags: [quality, security]
---

# Code Review

## Intent

Gennemgår kode systematisk med fokus på korrekthed, sikkerhed og vedligeholdbarhed.

## Steps

1. Læs den relevante kode
2. Vurdér: korrekthed, sikkerhed, performance, læsbarhed
3. Prioritér fund: kritisk / vigtigt / minor
4. Foreslå konkrete rettelser med begrundelse

## System Prompt Addition

Du udfører nu en grundig code review. Strukturér din feedback sådan:

**Kritisk** — fejl der skal rettes inden merge (sikkerhedshuller, dataleak, race conditions)
**Vigtigt** — problemer der bør rettes (logikfejl, manglende fejlhåndtering ved systemgrænser)
**Minor** — forbedringer der kan overvejes (navngivning, struktur, kommentarer)

Vær konkret: vis linjenummer og foreslå den rettede kode.
Vær direkte men konstruktiv. Spring over hvad der er velskrevet.
