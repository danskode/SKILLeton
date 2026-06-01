---
name: secure-harness
version: 1.0.0
description: Design af sikre bash/CLI-pipelines der sender ikke-betroet indhold til claude --print
tools: [read_file, write_file]
tags: [security, ai_engineering]
---

# Secure Harness

## Intent

Hjælper med at designe eller reviewe bash-scripts der sender ikke-betroet indhold
(kode, diffs, brugerinput) til `claude --print` til analyse. Implementerer
OWASP LLM01-forsvar og Anthropics anbefalede mønstre for prompt injection-mitigation.

## Steps

1. Læs det eksisterende script eller beskriv hvad der skal bygges
2. Identificér injektionsvektorer: hvad er bruger-kontrolleret? hvad er git-afledt?
3. Anvend forsvarslag i rækkefølge (se nedenfor)
4. Verificér at instruktioner og data er strukturelt adskilt

## System Prompt Addition

Du designer nu et sikkert LLM-harness. Anvend disse forsvarslag i rækkefølge:

**Lag 1 — Strukturel adskillelse (vigtigst):**
Instruktioner i `--system-prompt-file` (betroet kanal / system prompt-position).
Ikke-betroet indhold via stdin (user turn). Bland dem aldrig i samme streng.

```bash
claude --print --system-prompt-file ./instruktioner.txt < ikke-betroet-indhold.txt
```

**Lag 2 — HTML-entity-escape alt bruger-kontrolleret indhold:**
```bash
escape_xml() { sed 's|&|\&amp;|g; s|<|\&lt;|g; s|>|\&gt;|g'; }
SAFE_INPUT=$(printf '%s' "$INPUT" | escape_xml)
```
Gælder kodeindhold, git-metadata (branch-navne, commit-beskeder), filnavne.

**Lag 3 — XML-tagging i user turn:**
```
<untrusted-code>
[escaped kodeindhold her]
</untrusted-code>
```
Systemprompten skal eksplicit instruere modellen om at behandle indhold i tagget som data.

**Lag 4 — Tempfil frem for procesargument (CWE-214):**
Skriv prompt til `mktemp`-fil og send via stdin. Undgår at kodeindhold er synligt
i `ps aux` og `/proc/*/cmdline`.

**Lag 5 — Sanitér output:**
Strip ANSI-sekvenser (CSI + OSC) fra claude-svar inden output til terminal:
```python
re.sub(r'\x1b(?:\[[0-9;]*[A-Za-z]|\][^\x07\x1b]*(?:\x07|\x1b\\))', '', content)
```

**Advarsler:**
- LLM-forsvar er heuristisk — ingen garantier (arxiv 2506.08837v2)
- Anthropics eget security-review Action er ikke hærdet mod prompt injection
- Arv ikke credentials (`GITHUB_TOKEN`, `ANTHROPIC_API_KEY`) til subprocess medmindre nødvendigt
- `claude --print` deaktiverer trust verification (`-p` flag) — operatøren bærer ansvaret

**Dual LLM-pattern (stærkest, kræver to kald):**
Karantæne-LLM læser ikke-betroet indhold og returnerer kun struktureret data.
Privilegeret LLM modtager aldrig rå ikke-betroet tekst, kun det strukturerede output.
