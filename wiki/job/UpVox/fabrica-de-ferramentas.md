---
type: job-concept
job: UpVox
tags: [in-progress, ai-builder, handoff]
updated: 2026-07-21
---

# Fábrica de Ferramentas

**Status (do doc de handoff, 2026-06-10): funcionalidade completa em 3 repos, commitada, sem merge (PRs abertos).** Falta deploy (`ANTHROPIC_API_KEY` + envs `TOOL_AGENT_*`) e o merge dev→prod. Ver [[wiki/job/UpVox/roadmap-iniciativas]] para o resto do backlog planejado.

## O que é
Builder (`/ferramentas`) onde o admin monta ferramentas de IA (ex.: "Vetorizar logo", "Gravação a laser") de 3 formas: canvas low-code estilo n8n, modo "Etapas" (lista), ou conversando com um agente IA ("Engenheiro de Ferramentas") que monta a ferramenta sozinho ao vivo no mesmo canvas.

A ferramenta montada vira uma **`ToolDefinition`** (JSON) publicada na `upvox-api` e executada por um motor genérico de blocos na `Profissao-Laser-API`. O cliente final usa a ferramenta por uma UI auto-gerada (`DynamicToolView`), pagando em [[wiki/job/UpVox/arquitetura|voxes]].

## Os 3 repos envolvidos
| Repo | Papel |
|---|---|
| `profissao_laser` (front) | builder + UI do cliente |
| `Profissao-Laser-API` (main API) | motor de blocos + agente IA via SSE |
| `upvox-api` | armazena/publica `ToolDefinition`, débito de voxes |

O front fala com dois backends: a main API (motor + agente, via gateway) e a upvox (CRUD/publish de definições + billing).

## Modelo de dados
- **`BuilderState`** (front) é a fonte da verdade visual do builder; o canvas é só uma projeção dela.
- **`ToolDefinitionDoc`** é o JSON publicado (`input`, `pipeline`, `output`, `ui`, `billing: {vox_cost, free_quota}`).
- **Round-trip garantido**: `docToState(buildDoc(state))` é idempotente — posições do canvas nunca vão pro doc publicado.
- **Nós custom**: presets nomeados sobre um bloco base; ao publicar são expandidos pro bloco base — o motor e a upvox só enxergam blocos base, nunca "paleta-fantasma".

## Nota de segurança
O bloco `util.http_request` (motor de blocos, main API) tem **trava SSRF obrigatória**: allowlist de host + bloqueio de IP privado/loopback/link-local, incluindo bypass IPv6 (`::ffff:`/NAT64) com fail-closed. Só ligar em produção após review de segurança.

## Sources
- [[raw/job/UpVox/FabricaDeFerramentas.md]]
