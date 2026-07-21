---
type: project-overview
code: ApiBoilerplate
title: API Boilerplate — scaffold Fastify reutilizável
classes_ingested: 1
---

# API Boilerplate

Template/scaffold reutilizável de API — base compartilhada por outras APIs do Tobias ([[wiki/projects/Success/arquitetura]], [[wiki/projects/RochaProperty/arquitetura]]).

## Stack
Node.js 24 + TypeScript (ESM), Fastify 5, Drizzle ORM + PostgreSQL, Redis opcional (ioredis), Firebase Admin (JWT), TypeBox, **Awilix** (DI), Swagger + Scalar UI, Sentry opcional, Biome, Vitest.

## Arquitetura em 5 camadas por módulo
`schemas → repositories → services → controllers → routes` + `interfaces/` (`I*Service`/`I*Repository`).

## Convenções reutilizadas em todos os projetos derivados
- Alias `@/*` → `src/*`; imports ESM exigem `.js` mesmo em arquivos `.ts`.
- Resposta: sucesso = objeto/array direto; paginada = `{ data: [], meta: { total, page, limit, totalPages } }`; erro = `{ statusCode, error, message, details? }`.
- Rotas públicas via `config: { isPublic: true }`.
- `CacheService` cache-aside via DI, vira no-op sem `REDIS_URL`.
- Health checks (`/health`, `/ready`) e rate limit global configuráveis.
- `npm run generate:module <nome>` — scaffold completo (camadas + testes + registro no DI).

## Sources
- [[raw/projects/ApiBoilerplate/Nota.md]]
