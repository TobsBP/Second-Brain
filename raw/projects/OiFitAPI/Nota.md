Levantamento a partir de `~/Documents/projects/Oi-Fit-API/` (datado de maio/2026, anterior ao monorepo `~/Documents/fetin/` de julho/2026).

## O que é
API REST para gerenciar produtos, usuários, endereços e autenticação — protótipo mais simples e antigo do que a arquitetura atual do OneFit (ver [[raw/projects/Fetin/Arquitetura.md]]).

## Stack
Fastify + TypeScript, **Supabase** como BaaS (banco + auth), Zod (via `fastify-type-provider-zod`), Fastify JWT, Swagger/Scalar, Biome, build via Babel.

## Arquitetura
Em camadas: Routes → Controllers → Services → Repositories → (Supabase). `Types` para tipos TS/Zod, `Lib` para configuração de clientes externos (Supabase Client).

## Módulos
Auth (registro/login com JWT), Users (perfil), Products (listagem/gestão), Address (endereços vinculados a usuários).

## Observação
Este projeto parece ser uma versão inicial/prototípica da ideia que evoluiu para o OneFit (nome de produto atual do projeto Fetin): aqui ainda gira em torno de "produtos" genéricos e Supabase, enquanto o OneFit já tem domínio específico de fitness/nutrição (profissionais, clientes, treinos, planos alimentares) sobre Strapi + GraphQL BFF.
