---
name: spec-driven-dev
version: 1.0.0
description: Spec-Driven Development gjort rigtigt — spec som kontrakt med adskilte concerns (Intent/Context/Expectations), testbare acceptkriterier før kode; undgår single-document-fælden
tools: [write_file, read_file]
tags: [spec, sdd, planning, aidd, core]
requires: [harness]
---

# Spec-Driven Development (gjort rigtigt)

## Intent

Hjælper brugeren med at producere en spec der virker som en **kontrakt**, ikke en
beskrivelse — og som undgår den klassiske SDD-fælde. SDD's grundintuition er rigtig
(definér hvad du vil *inden* agenten genererer), men naiv SDD fejler i sin
*implementering*: ét dokument, skrevet af hvem der holder tastaturet den morgen,
beder om at være intent + definition-of-done + workflow + kontekst på én gang. Hullerne
overlades til agentens skøn — og hullet er ikke en defekt i spec'en, men en strukturel
konsekvens af single-dokument-tilgangen.

Denne skill løser det ved at **adskille de tre concerns** (Intent / Context /
Expectations) med forskelligt ejerskab, gøre acceptkriterier **testbare før** kode, og
sætte **hårde constraints** — så agenten bestemmer *hvordan*, men ikke *hvad* eller
*hvornår det er lykkedes*.

## Principper (følger wikiens SDD-take)

1. **Grundintuitionen er rigtig.** Jo præcisere definition upfront, desto bedre
   generation. Modeller er gode til pattern completion, ikke mind reading.
2. **Single-dokument-fælden er den egentlige fejl.** Ét SPEC.md der blander intent,
   forventninger og kontekst skaber huller agenten gætter i. Adskil dem.
3. **Spec = kontrakt, ikke beskrivelse.** Den er bindende, ikke vejledende. Koden er et
   artefakt der skal matche spec'en — ikke omvendt.
4. **Tre adskilte dele (ICE):**
   - **Intent** — *hvad* skal opnås, i brugerens termer. Intet om stack eller hvordan.
   - **Context** — stack, eksisterende kode, constraints, compliance. Ejes af
     ingeniøren/harnesset, ikke gættet af agenten.
   - **Expectations** — testbare acceptkriterier + eksplicitte failure scenarios.
     *Hvornår tæller det som løst?*
5. **Acceptkriterier er testbare og defineret FØR implementering.** "Gør det robust" er
   et håb, ikke et kriterium. Skriv kriterier en udenforstående kan verificere.
6. **Constraints er hårde grænser** — hvad må *ikke* ske (sikkerhed, performance-budget,
   API-kontrakter, data der ikke må røres).
7. **Intent vs. implementering.** Specificér *hvad* og *hvornår det er lykkedes*; lad
   agenten vælge *hvordan*. Overspecificér ikke implementeringen.
8. **Harness ≠ metode.** En SPEC.md-skabelon er et harness; disciplinen her er metoden.
   Et harness uden metode lader bare agenten udfylde hullerne pænere.
9. **Specs må gerne være iterative og retrospektive.** De bedste specs destilleres ofte
   *ud af* et kørende system (jf. OpenAI Symphony). Tving ikke en perfekt spec upfront —
   start præcist nok til at komme i gang, og stram den efterhånden.

## Spec-kontraktens struktur

Skriv spec'en til `specs/<navn>.md` med disse adskilte sektioner:

```markdown
# Spec: <navn>

## Intent
<Hvad skal opnås, i brugerens termer. Ikke hvordan, ikke stack.>

## Context
- Stack/rammer der er givne: …
- Eksisterende kode der berøres: …
- Compliance/forretningsregler: …

## Constraints (hårde grænser — hvad må IKKE ske)
- …

## Expectations (testbare acceptkriterier — definition of done)
- [ ] <kriterium en udenforstående kan verificere>
- [ ] …

### Failure scenarios (skal håndteres)
- <konkret måde det kan gå galt> → forventet adfærd

## Out of scope
- <eksplicit hvad denne spec IKKE dækker>

## Open questions
- <det du endnu ikke ved — markér frem for at gætte>
```

## Steps

Stil ét spørgsmål ad gangen — vent på svar før næste.

1. **Intent** — "Hvad skal resultatet opnå, set fra brugeren? (uden at nævne hvordan)"
2. **Context** — "Hvilken stack, eksisterende kode og forretnings-/compliance-regler er
   givne?" Læs gerne relevante filer for at fylde dette ud i stedet for at spørge.
3. **Constraints** — "Hvad må absolut ikke ske? (sikkerhed, data, API-kontrakter, budget)"
4. **Expectations** — "Hvornår er det løst? Giv mig konkrete, testbare kriterier." Pres på
   vaghed: omskriv "håndter fejl ordentligt" til verificerbare udsagn.
5. **Failure scenarios** — "Hvilke konkrete måder kan det fejle, og hvad skal der så ske?"
6. **Out of scope / open questions** — afgræns, og notér det ukendte i stedet for at gætte.
7. **Skriv kontrakten** til `specs/<navn>.md` og opsummer hvor agenten stadig ville skulle
   gætte — luk de huller før implementering.

## Kobling til ekte-flowet

Denne spec er ikke et dødt dokument — den fodrer det autonome flow:

- **Intent** → beskrivelsen du giver `/goal`.
- **Context + Constraints** → guardrails og den kontekst agenten arbejder i.
- **Expectations** → bliver til succeskriterierne ved `/plan godkend`, som **IntentSensor**
  måler imod i `/goal`-loopet og i `/verify`. En spec hvis acceptkriterier ikke er
  testbare giver en svag verifikation — derfor er trin 4 det vigtigste.

Kør derfor gerne: `spec-driven-dev` (denne skill) → `/plan` → `/plan godkend` → `/goal` →
`/verify`. Beslægtet med [[intent-spec-guide]] (de fem Intent-komponenter, sprogligt fokus)
og `/spec`-worktree-arbejdsgangen.

## System Prompt Addition

Du arbejder spec-først, men som *kontrakt med adskilte concerns* — ikke ét mudret
dokument. Bland aldrig Intent (hvad), Context (givne rammer) og Expectations (testbare
acceptkriterier) sammen; hold dem i hver sin sektion med klart ejerskab. Skriv
acceptkriterier en udenforstående kan verificere, og navngiv failure scenarios eksplicit.
Specificér *hvad* og *hvornår det er lykkedes* — lad implementeringen (*hvordan*) være
agentens. Når brugeren er vag, omskriv håbet til et testbart kriterium frem for at gå
videre. Markér det ukendte som open questions i stedet for at gætte. En spec må gerne
strammes iterativt — start præcist nok til at komme i gang.
