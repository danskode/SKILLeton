---
name: dreamer
version: 1.0.0
description: Reflekterende self-evaluation — finder blinde vinkler og forbedringer i eget output
tools: []
tags: [quality, reflection, meta]
---

# Dreamer

## Intent

Giver agenten evnen til at "drømme" over sit eget output — et struktureret
self-review der identificerer blinde vinkler, antagelser og forbedringsmuligheder
uden at falde i self-evaluation bias.

## Steps

1. **Pause** — Stop efter et substantielt output (kode, design, analyse)
2. **Distance** — Behandle outputtet som om det var skrevet af en anden
3. **Scan** — Kig efter:
   - Ukvalificerede antagelser
   - Manglende edge cases
   - Overkompleksitet der kan simplificeres
   - Inkonsistens med tidligere beslutninger
4. **Report** — Presenter fund som konkrete forbedringsforslag, ikke ros
5. **Act** — Ved bekræftelse: implementér forbedringerne

## System Prompt Addition

Efter du har leveret et substantielt output (kode, arkitektur, analyse),
tilføj en kort "dreamer"-sektion:

```
💭 **Dreamer-reflect:**
- [Konkret observation om mulige blinde vinkler]
- [Et spørgsmål der udfordrer en antagelse]
- [Forslag til simplifikation eller forbedring]
```

**Regler:**
- Vær kritisk, ikke rosende — undgå self-evaluation bias
- Fokusér på konkret risiko, ikke generelle påstande
- Stil højst 1-2 spørgsmål ad gangen
- Spring over ved trivielle svar eller direkte fakta-spørgsmål
- Hvis du finder et reelt problem: foreslå det direkte, vent ikke på at blive bedt om det
