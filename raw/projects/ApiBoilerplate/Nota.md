Levantamento a partir de `~/Documents/projects/api-boilerplate/`.

## O que é
Template/scaffold reutilizável de API — não é um produto, é a base compartilhada por outras APIs do Tobias (`success-api`, `back-rocha-property`).

## Stack
Node.js 24 + TypeScript (ESM), Fastify 5, Drizzle ORM + PostgreSQL, Redis (opcional, via ioredis), Firebase Admin (JWT), TypeBox (validação), **Awilix** (DI), Swagger + Scalar UI (`/docs`), Sentry (opcional), Biome, Vitest.

## Arquitetura em 5 camadas por módulo
`schemas → repositories → services → controllers → routes`, mais `interfaces/` com contratos `I*Service`/`I*Repository`.

## Convenções
- Alias `@/*` → `src/*`; imports ESM exigem extensão `.js` mesmo para arquivos `.ts`.
- Resposta de sucesso: objeto/array direto. Paginada: `{ data: [], meta: { total, page, limit, totalPages } }`. Erro: `{ statusCode, error, message, details? }`.
- Rotas públicas via `config: { isPublic: true }`.
- `CacheService` injetado via DI (`cache`), vira no-op sem `REDIS_URL` configurado (cache-aside).
- Health checks: `GET /health` (liveness), `GET /ready` (readiness — checa DB e Redis, 503 se dependência obrigatória cair).
- Rate limit global configurável (`RATE_LIMIT_MAX`, `RATE_LIMIT_WINDOW`).
- `npm run generate:module <nome>` faz o scaffold completo (camadas + testes + registro no container DI e em `src/modules/index.ts`).
