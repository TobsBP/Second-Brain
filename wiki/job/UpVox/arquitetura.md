---
type: job-concept
job: UpVox
tags: [architecture, billing, lms, stripe]
updated: 2026-07-21
---

# Arquitetura do Up-vox

## Duas gerações

| Geração | Repositório | Stack | Papel |
|---|---|---|---|
| Antiga | `Profissao-Laser-API` | Fastify 5 + TS + Supabase (Postgres) + Stripe + Zod | API monolítica: produtos, cursos, módulos, lições, quiz, turmas, cupons, compras/assinaturas |
| Antiga | `profissao_laser` | Next.js 16 + Tailwind v4 + TanStack Query + Supabase | Dashboard admin/cliente completo (vendas, afiliados, agendamentos, chat) |
| Nova | `upvox-api` | Fastify 5 + Zod 4 + Supabase (sem ORM) + Stripe + Bunny Stream | Refactor da API antiga para o modelo Course/Plan/Tool/Vox |
| Nova | `api-gateway` (`upvox-gateway`) | Fastify + `@fastify/http-proxy` + Supabase | Gateway de auth/roteamento na frente do `upvox-api` |

`upvox-api` é literalmente descrito como o refactor de `profissao-laser-back` — mesma equipe/produto, novo modelo de domínio.

## Modelo de domínio (Course / Plan / Tool / Vox)
- **Course**: só conteúdo (módulos → lições → quiz/materiais), sem preço.
- **Plan**: tier (basic/pro/max) que define quais Tools são liberadas e a cota grátis mensal de cada uma.
- **CoursePlan**: liga Course a Plan com preço mensal/anual — o SKU real vendido.
- Cliente assina vários cursos, cada um em um tier. **Stripe é a fonte de verdade** da assinatura.
- **Vox**: crédito global, só comprado, nunca expira. Cada Tool tem `vox_cost`; esgotada a cota grátis, chamadas debitam voxes.

## `withToolBilling` — billing de duas portas
Toda feature paga passa por esse helper central:
1. **Gate 1 (Entitlement)** — a assinatura ativa do cliente dá acesso à tool? Senão, 403.
2. **Gate 2 (Capacidade)** — cota grátis do período primeiro; esgotada, debita voxes; sem saldo, 402 `insufficient_voxes`.
3. Executa a chamada ao provider real (IA, Bunny, etc.).
4. **Falha → reembolso compensatório**: devolve a cota ou os voxes gastos.

Isso é uma instância bem concreta de rate limiting + billing acoplados, e de compensação transacional (saga-like) em caso de falha do provider externo.

## Iniciativas em andamento
- [[wiki/job/UpVox/fabrica-de-ferramentas]] — builder de ferramentas de IA (canvas/etapas/agente), consumindo a `upvox-api` como terceiro tipo de cliente além do front principal e do gateway.
- [[wiki/job/UpVox/comunidade-vitrine-projetos]] — feature de comunidade dentro do produto.
- [[wiki/job/UpVox/roadmap-iniciativas]] — índice de todo o backlog planejado (specs/plans) nos 3 repos.
- `profissao_laser/landing/` — mini landing page de marketing, HTML standalone + versão React componentizada, fora do build principal do Next.js.

## Outros fluxos
- **Auth**: Supabase Auth, JWT HS256, trigger `on_auth_user_created` cria a linha em `public.users`.
- **Assinatura**: checkout Stripe → webhook `customer.subscription.created` grava `customer_subscriptions`; `invoice.paid` reseta cotas do período.
- **Compra de voxes**: checkout Stripe modo `payment` → webhook credita `vox_balance` + `vox_ledger`.
- **Playback de aula**: URL HLS assinada da Bunny Stream, condicionada a `lesson.is_free` ou assinatura ativa; progresso via `lesson_progress`.

## Sources
- [[raw/job/UpVox/Arquitetura.md]]
