---
name: java-spring
version: 1.0.0
description: Idiomatisk Java + Spring Boot — lagdelt arkitektur, constructor injection, validering og test (eksempel på en sprogspecifik skill)
tools: [read_file, write_file]
tags: [java, spring, language]
---

# Java + Spring Boot

## Intent

Holder Java/Spring-kode idiomatisk og vedligeholdbar. Et eksempel på en
**sprogspecifik** skill — udbyg gerne med dine egne konventioner eller fork til
et andet sprog/framework.

## System Prompt Addition

Du skriver Java (17+) og Spring Boot idiomatisk. Følg disse konventioner:

- **Lagdeling:** controller → service → repository. Hold forretningslogik i
  service-laget, ikke i controllers eller entities.
- **Dependency injection via constructor** (ikke field-injection); gør felter
  `private final`. Undgå `@Autowired` på felter.
- **DTO'er ved API-grænsen** — eksponér aldrig JPA-entities direkte i controllers.
  Map mellem DTO og entity eksplicit.
- **Validering** med `jakarta.validation` (`@Valid`, `@NotNull` osv.) på indkommende
  DTO'er.
- **Fejlhåndtering** centralt med `@ControllerAdvice`/`@ExceptionHandler` — kast
  meningsfulde exceptions, returnér passende HTTP-status.
- **Immutabilitet** hvor muligt (records til DTO'er, `final` på lokale variabler
  hvor det øger klarhed).
- **Test:** JUnit 5 + AssertJ; `@SpringBootTest` kun når integration kræves, ellers
  rene unit-tests med mocks (Mockito). Skriv testen før eller sammen med koden.
- **Undgå** statisk state, `System.out` til logging (brug SLF4J), og brede
  `catch (Exception e)` uden håndtering.

Foreslå konkrete filstier og pakkestruktur (`com.eksempel.projekt.<lag>`) og hold
ændringer små og testbare.
