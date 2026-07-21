---
type: project-concept
project: Success
tags: [architecture, nextjs, fastify]
updated: 2026-07-21
---

# Arquitetura do Success

## success (web)
- Next.js (App Router) + Tailwind v4 com tokens em `globals.css` (`@theme`) — preferir classes de escala a valores arbitrários.
- `src/app/` é só roteamento; `src/modules/` espelha as rotas 1:1; cada módulo só ganha `components/hooks/services/types` quando a tela é de fato implementada (sem pastas vazias antecipadas).
- Rotas: `(auth)/login`, `(dashboard)/{overview, expenses, income, investments, goals, projections, reports, settings}`.
- Componentes compartilhados de dados/visualização: KpiCard, NetWorthCard, DonutChart, LineChart, BarChart, DeltaIndicator, CurrencyInput, MonthPicker.

## success-api
- Fastify 5 + TypeScript ESM + Drizzle/PostgreSQL + Firebase Admin (JWT) + TypeBox + **Awilix DI** + Biome + Vitest — mesmo esqueleto do [[wiki/projects/ApiBoilerplate/ApiBoilerplate-overview]].
- 5 camadas por módulo: `schemas → repositories → services → controllers → routes`.
- Plugins na ordem: cors → helmet → rate-limit → swagger → error-handler → health → firebase-auth → módulos.
- Erros tipados (`AppError`, `NotFoundError`, `ConflictError`, `UnauthorizedError`, `ForbiddenError`); resposta paginada `{ data: [], meta: { total, page, limit, totalPages } }`.
- `npm run generate:module <nome>` faz o scaffold completo de um módulo.

## Sources
- [[raw/projects/Success/Arquitetura.md]]
