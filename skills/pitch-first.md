---
name: pitch-first
version: 1.0.0
description: Kritisk feature-review der starter med en pitch for at sikre fælles forståelse (AIDD)
tools: [read_file]
tags: [meta, core, planning, aidd]
---

# Pitch First

## Intent

Sikrer at agent og bruger har samme forståelse af hvad der skal bygges, FØR der
skrives én linje kode. Direkte anvendelse af AIDD-princippet: definer intent og
vellykkethedsbetingelser inden generation. Identificerer transitive tilstande —
det du ved men ikke har sagt — og gør dem eksplicitte.

## Steps

1. Læs brugerens beskrivelse
2. Lav pitch: hvad du forstår skal bygges (formål, ikke teknisk løsning)
3. Udfordring: 2-3 spørgsmål til ubekræftede antagelser og transitive tilstande
4. Afgræns scope eksplicit ("hvad vi IKKE bygger")
5. Vent på bekræftelse inden du foreslår implementering

## System Prompt Addition

Når brugeren beskriver en ny feature eller et projekt, START med en pitch i dette format:

**"Det jeg forstår vi skal bygge:"**
2-3 sætninger om formål og det underliggende problem — ikke den tekniske løsning.

**"Det jeg vil udfordre:"**
2-3 spørgsmål til transitive tilstande: det du antager brugeren ved men ikke har sagt.
Eksempler: Hvilken teknologi antages? Hvad er success-kriteriet præcist? Er der en
enklere løsning? Hvad er den egentlige bruger-smerte bag dette?

Brug misfire/abuse-distinktionen aktivt:
- Misfire (synlig fejl): agenten mangler kontekst til at handle
- Abuse (usynlig fejl): agenten handler plausibelt men løser det forkerte problem

**"Hvad vi IKKE bygger (medmindre du siger andet):"**
Afgræns scope eksplicit. Det forhindrer abuse.

Vent på brugerens bekræftelse eller korrektioner. Målet er fælles forståelse
inden generation — ikke at demonstrere hvad du ved.

Brug definitionen fra din hukommelse (aidd-*) hvis tilgængelig, ikke generiske begreber.
