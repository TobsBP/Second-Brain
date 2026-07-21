Transcrito do doc de handoff `profissao_laser/docs/fabrica-de-tools.md` (status em 2026-06-10).

## Status
Funcionalidade completa em 3 repos, **commitada, sem merge** (PRs abertos). Falta deploy (precisa de `ANTHROPIC_API_KEY` + envs `TOOL_AGENT_*`) e o merge dev→prod. Esse doc é o ponto de retomada.

## O que é
A **Fábrica de Ferramentas** (`/ferramentas`) é um builder onde o admin monta ferramentas de IA (ex.: "Vetorizar logo", "Gravação a laser") de 3 formas:
1. **Canvas low-code** estilo n8n (nós ligados saída→entrada).
2. **Etapas** (modo lista, mesma coisa sem canvas).
3. **Agente IA "Engenheiro de Ferramentas"** — conversa em PT-BR e monta a ferramenta sozinho ao vivo no mesmo canvas (estilo Claude Code, para leigos).

A ferramenta montada vira uma **`ToolDefinition`** (JSON), salva/publicada na **upvox-api**, executada por um **motor genérico de blocos** na **main API** (`Profissao-Laser-API`). O cliente final usa a ferramenta por uma UI auto-gerada (`DynamicToolView`), pagando em **voxes**.

## Os 3 repositórios envolvidos

| Repo | Path local | Papel | Stack |
|---|---|---|---|
| front | `profissao_laser` | builder + UI do cliente | Next.js 16, React 19 (react-compiler ON), Tailwind v4, Biome |
| main API | `Profissao-Laser-API` | motor de blocos + agente IA (SSE) | Fastify 5, Zod, Biome, Vitest |
| upvox | `upvox-api` (GitHub `upvox-api`) | armazena/publica `ToolDefinition` + débito de voxes | Fastify 5 + Supabase + Stripe |

O front fala com **dois** backends: `api` (main API, via gateway, motor + agente) e `apiCourses` (upvox, CRUD/publish de defs + billing).

## Branches/PRs (no momento do doc)
- front: `joaovcruz/fabrica-tools-front` → PR #118 (base `dev`) — canvas, blocos util, nós custom, painel do agente, redesign do formulário.
- main API: `joaovcruz/fabrica-tools-engine` → PR #132 (base `dev`) — blocos `util`, `validateDefinition`, módulo `tool-agent` (SSE, metering).
- upvox: `joaovcruz/agente-tool-spend` (sem PR, base dev) — `debitAgentBuild` + rota `/v1/me/agent/spend`.
- upvox: `joaovcruz/agente-tool-spend-to-main` → PR #41 (base `main`) — cherry-pick do spend pra prod.
- Já mergeado antes (prod): upvox #40 (`tool-definitions` → main).

**Regra do projeto:** PRs ficam abertos sem merge até aprovação do dono; backends vão para `dev` e depois promovem para `main` por cherry-pick aditivo.

## Modelo de dados (o coração)
- **`BuilderState`** (front, `builder-model.ts`) — estado visual do builder, é a fonte da verdade (o canvas é uma projeção dele): `templateId, toolKey, title, description, icon, actionLabel, fields[], nodes[], customNodes[], output, voxCost, freeQuota`.
- **`ToolDefinitionDoc`** (JSON publicado, `tool-definitions.service.ts`): `schemaVersion?, input, pipeline[], output, ui, billing: {vox_cost, free_quota}`.
- **Round-trip garantido**: `docToState(buildDoc(state))` é idempotente — posições do canvas são efêmeras e nunca vão para o doc.
- **Nós custom**: `CustomNodeSpec` é um preset nomeado sobre UM bloco base; no `buildDoc` é expandido para o bloco base (motor e upvox só veem blocos base).

## Peças principais (front)
`builder-model.ts` (estado/serialização), `block-catalog.ts` (espelha o `blockRegistry` da main API — nunca por bloco não deployado, "paleta-fantasma"), `tool-builder-view.tsx` (view principal), `canvas/tool-canvas.tsx` (React Flow), `agent/tool-agent-chat.tsx` + `tool-agent.service.ts` (SSE do agente), `dynamic-tool-view.tsx` (UI do cliente auto-gerada), `use-tool-billing.tsx` (cobrança de uso).

## Peças principais (main API)
`tool-blocks/blocks/*.ts` — motor de blocos (`registerCoreBlocks()`), blocos `image.*`, `laser.photoengrave`, `output.*`, `util.*` (`text_template`, `math` com guarda de divisão por zero, `condition`, `http_request`).

**Nota de segurança:** `util.ts` → `http_request` tem **trava SSRF obrigatória** (allowlist de host + bloqueio de IP privado/loopback/link-local, incluindo bypass IPv6 `::ffff:`/NAT64 + fail-closed). Só ligar em prod após review.

`lib/tool-engine.ts` + `controllers/tool-run.ts` executam o pipeline publicado (`POST /api/tool-run/:key`), revalidando tudo no run.
