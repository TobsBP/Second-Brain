Levantamento a partir de `~/Documents/projects/success/` (web) e `~/Documents/projects/success-api/` (API).

## O que é
Dashboard pessoal de finanças: visão geral, receitas, despesas, investimentos, metas, projeções, relatórios, patrimônio líquido (net worth).

## success (web)
- Next.js (App Router) + Tailwind v4 (tokens em `globals.css` via `@theme`, evitar valores arbitrários quando há classe de escala equivalente).
- `src/app/` é só roteamento; `src/modules/` espelha a estrutura de rotas 1:1, cada módulo cria sua própria subestrutura (`components`, `hooks`, `services`, `types`) só quando a tela é implementada — não criar pastas vazias adiantado.
- Rotas: `(auth)/login`, `(dashboard)/{overview,expenses,income,investments,goals,projections,reports,settings}`.
- `src/shared/components/`: KpiCard, NetWorthCard, DonutChart, LineChart, BarChart, DeltaIndicator, CurrencyInput, MonthPicker/DatePicker, Sidebar, TopAppBar.
- Usa `HTMLFormElement`/`SubmitEvent<HTMLFormElement>` do React (não os globais do DOM) para formulários.

## success-api
- Mesmo boilerplate que `api-boilerplate`: Fastify 5 + TypeScript ESM + Drizzle/PostgreSQL + Firebase Admin (JWT) + TypeBox + Awilix DI + Biome + Vitest.
- Arquitetura em 5 camadas por módulo: `schemas → repositories → services → controllers → routes`, interfaces `I*Service`/`I*Repository`.
- Plugins registrados em ordem: cors, helmet, rate-limit, swagger, error-handler, health, firebase-auth, módulos da app.
- `firebase-auth.plugin.ts`: hook `onRequest` global, verifica Bearer token, popula `request.authUser`; rotas públicas via `config: { isPublic: true }`.
- Erros tipados em `core/errors` (AppError, NotFoundError, ConflictError, UnauthorizedError, ForbiddenError); resposta paginada `{ data: [], meta: { total, page, limit, totalPages } }`.
- `npm run generate:module <nome>` faz o scaffold completo de um módulo novo.

## Relação com outros projetos
Compartilha o mesmo esqueleto de `api-boilerplate` (ver [[projects/ApiBoilerplate/Nota]]) — mesma stack, DI, plugins e convenções de erro/paginação usadas em `back-rocha-property`.
