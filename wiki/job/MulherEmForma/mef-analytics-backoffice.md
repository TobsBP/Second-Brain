---
type: job-concept
job: MulherEmForma
tags: [analytics, read-only]
updated: 2026-07-21
---

# mef-analytics-backoffice

API de analytics **read-only** para o backoffice: métricas, rankings, relatórios, lendo (por introspecção, sem migrations) um Postgres operacional já existente.

## Stack
Node 22+ (TS, ESM), Fastify 5, Zod 4, Drizzle ORM por introspecção, OpenAPI/Scalar em `/docs`, cliente HTTP para a Core API (usado por `renewals`), Vitest, Biome.

## Arquitetura
Camadas baseadas em classes/interfaces; cada módulo é um plugin Fastify autocontido com DI manual via construtor (sem container).

## Sources
- [[raw/job/MulherEmForma/mef-analytics-backoffice.md]]
