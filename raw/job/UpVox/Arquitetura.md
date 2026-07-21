Levantamento a partir de `~/Documents/Up-vox/{Profissao-Laser-API, profissao_laser, upvox-api, api-gateway}`.

## O que é
Plataforma de gestão/e-learning para profissionais de laser/estética (cursos, assinaturas, cupons, afiliados, agendamentos). O produto nasceu como **Profissão Laser** e está em refactor para **upvox**, um modelo mais genérico de LMS + ferramentas pagas.

## Geração antiga (Profissão Laser)
- **Profissao-Laser-API**: Fastify 5 + TypeScript + PostgreSQL (Supabase) + Stripe + Zod. Camadas: controllers/routes/services/repositories/middleware/lib/types. Endpoints para produtos, cursos, módulos, lições (com vídeo), materiais, quiz, turmas (classes), cupons, compras/assinaturas Stripe, usuários/clientes. Autenticação multi-papel (Admin/Staff/Customer).
- **profissao_laser**: Next.js 16 (App Router) + Tailwind v4 + TanStack Query + Supabase. Dashboard: produtos, cursos, checkout, assinaturas, links promocionais/pagamento, cupons, vendas (KPIs/filtros/CSV), relatórios, afiliados, dúvidas/chat, agendamentos, controle de acesso.

## Geração nova (upvox) — refactor em andamento
`upvox-api` é descrito como "refactor of the `profissao-laser-back` product/class surface into a coherent **Course / Plan / Tool / Vox** model" — ou seja, é a evolução direta da API antiga.

### Modelo de domínio (novo)
- **Course**: só conteúdo (módulos → lições → quiz/materiais). Sem preço, sem flags de acesso.
- **Plan**: template de tier (basic/pro/max) que define quais **Tools** têm direito e a cota mensal grátis de cada uma. Sem preço.
- **CoursePlan**: liga um Course a um Plan com preços mensal/anual — é o SKU de fato.
- Um cliente pode assinar vários cursos, cada um em um tier de plano diferente. **Stripe é a fonte de verdade** do estado da assinatura.
- **Voxes**: moeda de crédito global, só compra. Cada Tool tem um `vox_cost`; quando a cota grátis de uma ferramenta acaba, as chamadas debitam voxes. Voxes nunca expiram.

### Fluxo central: `withToolBilling` (billing de duas portas)
Toda feature paga (preview de IA, vetorização, canvas etc.) passa por esse helper:
1. **Gate 1 — Entitlement**: confere se a assinatura ativa do cliente dá direito à tool (`plan_tools`).
2. **Gate 2 — Capacidade**: primeiro tenta consumir a cota grátis do período (`customer_tool_usage`); se esgotada, debita `voxes` do saldo (`vox_balance`/`vox_ledger`); sem saldo suficiente → 402 `insufficient_voxes`.
3. Executa a chamada real ao provider (IA/Bunny/etc.).
4. Em caso de falha, **reembolso compensatório**: devolve a cota ou os voxes gastos (registra em `tool_invocations`/`vox_ledger`).

### Outros fluxos documentados (specs com sequence diagrams)
- Signup/login via Supabase Auth (JWT HS256), trigger `on_auth_user_created` popula `public.users`.
- Assinatura de curso: checkout Stripe → webhook `customer.subscription.created` grava `customer_subscriptions`; renovação via `invoice.paid` reseta cotas do período.
- Compra avulsa de voxes: checkout Stripe modo `payment` → webhook credita `vox_balance`.
- Reprodução de aula: URL assinada da **Bunny Stream** (HLS), checando `is_free` ou assinatura ativa; progresso via `lesson_progress`.

### Stack de `upvox-api`
Node.js + TypeScript (strict, ESM), Fastify 5 + `fastify-type-provider-zod`, Zod 4, Supabase (Auth + Postgres, sem ORM — `@supabase/supabase-js` direto), `@fastify/jwt` (HS256), Stripe, Bunny Stream, Vitest, Biome, Supabase CLI para migrations.

### api-gateway (`upvox-gateway`)
Fastify + `@fastify/http-proxy` + `@fastify/cors` + Supabase (verificação de token) + Zod — um gateway de auth/roteamento na frente do `upvox-api` (hook de auth, verificação de JWT, proxy de rotas).

### Build order planejado (upvox-api)
1. Auth + Users → 2. Courses/Modules/Lessons CRUD → 3. Plans + CoursePlan + PlanTool → 4. Stripe subscriptions + webhook → 5. Voxes + tool registry + `withToolBilling` → 6. Lesson progress → 7. Endpoints reais das tools (AI preview, vectorize, etc.)
