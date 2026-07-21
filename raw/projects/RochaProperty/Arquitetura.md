Levantamento a partir de `~/Documents/projects/rocha-property/` (web) e `~/Documents/projects/back-rocha-property/` (API).

## O que é
Site de uma imobiliária: vitrine pública de imóveis, captação de leads e painel administrativo.

## rocha-property (web)
- Next.js (App Router) + TanStack Query (`src/integrations/tanstack-query/`).
- Rotas: `(public)/{imoveis, about, contracts}`, `(auth)/login`, `(private)/admin`.
- `src/modules/`: `auth`, `dashboard`, `gallery-sales`, `leads`, `properties` — cada um com `*.api.ts`, `*.hooks.ts`, `*.types.ts`.
- Componentes admin: GallerySaleFormModal/Panel, LeadsList/Panel, MetricCards, PropertiesTable, SideNav.
- Componentes públicos (home): HeroSection, FeaturedSection, AboutSection, DifferentialsSection, FilterSelect.

## back-rocha-property (API)
- Fastify 5 + TypeScript ESM + Drizzle/PostgreSQL + Firebase Admin (JWT) + TypeBox + **Awilix DI** + Vitest + Biome.
- Mesma arquitetura em 5 camadas de `api-boilerplate`/`success-api`: `routes → controller → service → repository → db`, interfaces `I*Service`/`I*Repository`.
- Módulo de exemplo confirmado nos testes: `src/modules/properties/`.
- Auth: hook global `firebase-auth.plugin.ts` verifica `Authorization: Bearer <firebase-id-token>`, popula `request.authUser` (`id, email, name, role`); rotas públicas via `config: { isPublic: true }`; rotas admin via `preHandler: requireAdmin` (checa `role === 'admin'`).
- Bootstrap (`src/server.ts`): cors → multipart (limite 10MB) → helmet → swagger → error-handler → firebase-auth → módulos; shutdown gracioso fecha Fastify + pool do DB no SIGINT/SIGTERM.

## Relação com outros projetos
Mesmo esqueleto de `api-boilerplate` (ver [[raw/projects/ApiBoilerplate/Nota.md]]), mas nessa API o padrão de DI com Awilix já está mais maduro/documentado que a versão mais simples do README genérico do boilerplate.
