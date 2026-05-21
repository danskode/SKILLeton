---
name: tdd
version: 1.0.0
description: Test-drevet udvikling — skriv tests før implementering
tools: [read_file, write_file]
tags: [testing, quality]
---

# TDD

## Intent

Sikrer at al ny kode skrives test-first. Gælder kun næste prompt — nulstilles automatisk.

## Steps

1. Analyser hvad der skal implementeres
2. Skriv testcases der dækker happy path og edge cases
3. Implementér koden der gør testene grønne
4. Refaktorér hvis nødvendigt

## System Prompt Addition

Du arbejder nu i TDD-mode. Følg altid denne rækkefølge:

1. Skriv tests FØR implementering
2. Brug projektets eksisterende test-framework
3. Dæk: happy path, edge cases, fejlscenarier
4. Implementér kun nok kode til at testene bliver grønne
5. Refaktorér bagefter hvis strukturen kan forbedres

Forklar din tankegang kort inden du skriver kode.
