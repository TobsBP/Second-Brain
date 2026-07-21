---
type: project-concept
project: RochaProperty
tags: [architecture, nextjs, fastify]
updated: 2026-07-21
---

# Arquitetura do Rocha Property

## rocha-property (web)
- Next.js (App Router) + TanStack Query.
- Rotas: `(public)/{imoveis, about, contracts}`, `(auth)/login`, `(private)/admin`.
- Módulos: `auth`, `dashboard`, `gallery-sales`, `leads`, `properties` (cada um com `*.api.ts`, `*.hooks.ts`, `*.types.ts`).
- Admin: GallerySaleFormModal/Panel, LeadsList/Panel, MetricCards, PropertiesTable, SideNav.
- Público: HeroSection, FeaturedSection, AboutSection, DifferentialsSection, FilterSelect.

## back-rocha-property (API)
- Fastify 5 + TypeScript ESM + Drizzle/PostgreSQL + Firebase Admin (JWT) + TypeBox + **Awilix DI** + Vitest — mesmo esqueleto do [[wiki/projects/ApiBoilerplate/ApiBoilerplate-overview]].
- `routes → controller → service → repository → db`; módulo confirmado: `properties`.
- Auth: `firebase-auth.plugin.ts` verifica Bearer token e popula `request.authUser` (`id, email, name, role`); rotas públicas via `config: { isPublic: true }`; rotas admin via `preHandler: requireAdmin`.
- Bootstrap: cors → multipart (10MB) → helmet → swagger → error-handler → firebase-auth → módulos; shutdown gracioso no SIGINT/SIGTERM.

## Sources
- [[raw/projects/RochaProperty/Arquitetura.md]]
