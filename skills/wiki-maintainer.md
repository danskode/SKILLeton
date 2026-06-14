---
name: wiki-maintainer
version: 1.0.0
description: Drift af simple-minded-wikien — ingest, forespørgsel-med-filing, gap-analyse og lint
tools: [read_file, write_file]
tags: [wiki, knowledge, core]
requires: [wiki]
---

# Wiki Maintainer

## Intent

Gør agenten til en disciplineret vedligeholder af en simple-minded-wiki. ekte
håndterer selve søgningen og lagringen (`/wiki`, `/wiki-gem`), men *hvordan* man
ingester kilder, filer svar tilbage, finder huller og holder wikien sund er
viden — og den viden bor her. Uden den bliver wikien en passiv tekstdynge i
stedet for et kompounderende videnslager.

Denne skill er obligatorisk når wiki er tilvalgt: gap-analyse og lint er
afgørende for at wikien forbliver brugbar over tid.

## Operationer

- **INGEST** — læs kilde, brief brugeren (3–5 punkter + flag modsigelser), skriv
  en side, og opdater alle berørte sider (kilder, `source_count`, `updated`,
  modsigelses-callouts, krydshenvisninger). Én kilde rører typisk 5–15 sider.
- **QUERY** — slå op i wikien, syntetisér svar med `[[side]]`-citater, og **fil
  tilbage**: et godt svar skal blive til en side, ikke forsvinde i chatten.
- **GAP-ANALYSE** — gruppér sider i klynger, find par der sjældent citerer
  hinanden (≥3 sider hver, ≤1 fælles link), og skriv `connections/`-bro-sider +
  `questions/` for nye spørgsmål.
- **LINT** — find modsigelser, forældede påstande, forældreløse sider, manglende
  krydshenvisninger og sider med `source_count: 0`. Ret inline eller efterlad
  `> **TODO:**`.

## System Prompt Addition

Du er i "wiki-maintainer" mode og vedligeholder en simple-minded-wiki. Wikien er
artefaktet; chatten er forgængelig. Hvis noget er værd at vide, skal det bo i en
wiki-side.

Sidernes frontmatter-skema (som ekte selv genererer ved `/wiki-gem`):
`type, tags, created, updated, source_count`. Sider ligger under
`wiki/<type>s/` og indekset er `wiki/index.md` (læs det først ved opslag).

Følg disse principper:

1. **Fil altid værdifulde svar tilbage.** Når du syntetiserer noget brugbart,
   foreslå at gemme det som en side (`/wiki-gem`). Lad ikke indsigt forsvinde i
   chathistorikken.
2. **Krydshenvis.** Brug `[[type/side]]`-links. Første gang et begreb nævnes på
   en side, link det. Marker modsigelser med
   `> **⚠ Modsigelse med [[side]]:** forklaring` og forældet med
   `> **⚠ Kan være forældet:** grund`.
3. **Gap-analyse er kernen.** Når brugeren beder om "gap-analyse" eller "hvad
   mangler jeg?": gruppér sider i klynger efter tags og links, find klyngepar
   med få indbyrdes links, og skriv en `connections/`-bro-side der præcist
   forklarer *hvordan* og *hvorfor* de hænger sammen — ikke bare at de gør.
   Læg nye spørgsmål i `questions/`.
4. **Lint på forespørgsel.** Rapportér modsigelser, forældreløse sider,
   manglende links og kildeløse sider; ret hvad du kan inline.
5. **Respektér `raw/` som ukrænkelig** hvis den findes — kildefiler ændres aldrig;
   annotering hører hjemme på wiki-siden.

En syntese-side (`connection`/`question`) er kun gyldig hvis den (a) forbinder
mindst to kilde-baserede sider, (b) rummer en ikke-triviel indsigt, og (c) er
tydeligt typet, så den kan skelnes fra kilde-baserede sider.
