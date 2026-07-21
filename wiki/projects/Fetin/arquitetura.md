---
type: project-concept
project: Fetin
tags: [architecture, microservices, rag]
updated: 2026-07-21
---

# Arquitetura do OneFit (Fetin)

O produto por trás das notas de ideação em [[wiki/projects/Fetin/monetizacao-app-fitness]] tem nome real **OneFit** e já existe como um sistema multi-serviço em `~/Documents/fetin/`.

## Serviços

| Serviço | Stack | Papel |
|---|---|---|
| **app** | Expo Router v57 (React Native) | App mobile do cliente/profissional: auth, tabs Home/Diet/Workout/Social, perfil |
| **web** | Next.js 16 (App Router, Turbopack) + Tailwind v4 | Dashboards por papel (RBAC sobre Firebase Auth): cliente, personal, nutricionista, personal chef, admin |
| **bff** | Apollo Server v5 (GraphQL), Node 24+ TS strict | Único ponto de acesso do app/web aos dados; nunca expõe o Strapi diretamente |
| **core** | Strapi v5 | CMS/persistência — fonte de verdade dos dados de domínio |
| **assistant** | Node/TS, Qdrant, Gemini | Chatbot RAG que ajuda o profissional a montar treinos/planos/receitas |
| **video** | Remotion | Geração do vídeo promocional (fora da arquitetura de produção) |
| **onefit-dashboard** | Next.js (App Router) + shadcn/ui (Base UI) + TanStack Query | Painel administrativo do OneFit, em repositório separado (`~/Documents/projects/onefit-dashboard/`) — não faz parte do monorepo `~/Documents/fetin/` |

`onefit-dashboard` cobre: overview, clients, nutritionists, personal, plans, schedule, users, workouts, profile — parece ser um painel mais recente/paralelo ao `web` do monorepo, ambos consumindo (presumivelmente) o mesmo `bff`.

**Regra central:** o app/web nunca fala com o Strapi diretamente — tudo passa pelo **bff** (GraphQL). O **assistant** roda como serviço separado do bff, falando com o Strapi diretamente (leitura) e nunca escreve nele.

## Papéis de usuário
Cliente, Personal Trainer, Nutricionista, Personal Chef, Administrador — cada um com dashboard dedicado no `web` (`/dashboard/<papel>`).

## Domínios (módulos do BFF / content-types do Strapi)
`professional`, `client`, `engagement` (ciclo de vida do relacionamento cliente↔profissional: pedido/aceite/rejeição/encerramento), `appointment`/`availability`/`available-date`, `food`, `recipe`, `meal-plan` e `workout` (padrão **library → assign**), `menu`, `daily-task`, `service-location`, `message`, `group`/`group-post`/`group-comment`, `review`.

## Assistant — arquitetura RAG
- **Indexado no Qdrant (catálogo reutilizável):** `exercise` (global + próprios do personal), `food` (biblioteca global + próprios do nutricionista), `recipe` (sempre próprio). Escopo de busca filtra por `professionalId` (null = global) e pelo tipo do profissional (PERSONAL→exercise, NUTRITIONIST→food+recipe, PERSONAL_CHEF→recipe).
- **Buscado ao vivo no Strapi (nunca indexado):** contexto do cliente (goal/restriction/plan) e seu plano ACTIVE — dado sensível/volátil demais para viver num vector store de terceiros.
- **Ingestão:** webhooks do Strapi mantêm o índice atualizado em tempo real; `npm run reindex` cobre backfill e recuperação de drift.
- **Fluxo do chat:** valida token Firebase → confirma `engagement` ACTIVE entre profissional e cliente → busca contexto do cliente → embeda a pergunta, top-K no Qdrant → monta prompt com contexto + retrieval + histórico (mantido pelo frontend) → responde via SSE.
- **Decisões deliberadas de v1 (não limitações técnicas):** assistant nunca escreve no Strapi (só recomenda — o profissional aprova e o `web` chama as mutations existentes do bff); sem histórico persistido no servidor; sem tool-use agentico mid-conversa (retrieval acontece uma vez por turno); sem rate limiting ainda.

Isso é um exemplo real e vivo dos padrões arquiteturais estudados em [[wiki/subjects/S03-ArqSoft/padroes-arquiteturais]] (SOA/API-first entre bff e core) e de mensageria/eventos (webhooks Strapi → assistant) próximo do conceito de [[wiki/subjects/S03-ArqSoft/padroes-arquiteturais|MOM]] em espírito, ainda que via webhook HTTP e não um broker dedicado.

Ver também [[wiki/projects/OiFitAPI/OiFitAPI-overview]] — protótipo anterior (Supabase, domínio genérico de "produtos") que antecedeu esta arquitetura.

## Sources
- [[raw/projects/Fetin/Arquitetura]]
- `~/Documents/projects/onefit-dashboard/` (README genérico, stack levantada por leitura do código)

