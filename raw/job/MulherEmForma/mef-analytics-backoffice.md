Levantado via GitHub API (`Mulher-em-Forma/mef-analytics-backoffice`) em 2026-07-21.

## README
"API de analytics **read-only** para o backoffice. Consulta um banco PostgreSQL operacional existente para servir métricas, rankings e relatórios a um consumidor de dashboard/backoffice. A versão atual (v1) não possui endpoints de escrita."

### Stack
Node 22+ (TS, ESM), Fastify 5, Zod 4 (`fastify-type-provider-zod`), OpenAPI via `@fastify/swagger` + Scalar em `/docs`, **Drizzle ORM por introspecção** (read-only, sem migrations), cliente HTTP para a Core API (usado por `renewals`), Vitest, Biome. Path alias `@/` → `src/`.

### Arquitetura
Camadas, baseada em classes e interfaces, composição por módulo — cada módulo é um plugin Fastify autocontido com injeção de dependência manual via construtor (sem container de DI).
