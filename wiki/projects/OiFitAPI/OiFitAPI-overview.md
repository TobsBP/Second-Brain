---
type: project-overview
code: OiFitAPI
title: Oi-Fit API — protótipo anterior ao OneFit
classes_ingested: 1
---

# Oi-Fit API

API REST (produtos, usuários, endereços, autenticação) datada de maio/2026 — anterior ao monorepo `~/Documents/fetin/` (julho/2026). Parece ser um protótipo inicial da ideia que evoluiu para o **OneFit** (ver [[wiki/projects/Fetin/Fetin-overview]]): aqui ainda gira em torno de "produtos" genéricos sobre Supabase, sem o domínio específico de fitness/nutrição que o OneFit tem hoje.

## Stack
Fastify + TypeScript, Supabase (BaaS), Zod, Fastify JWT, Swagger/Scalar, Biome, build via Babel.

## Arquitetura
Routes → Controllers → Services → Repositories → Supabase.

## Módulos
Auth, Users, Products, Address.

## Sources
- [[raw/projects/OiFitAPI/Nota.md]]
