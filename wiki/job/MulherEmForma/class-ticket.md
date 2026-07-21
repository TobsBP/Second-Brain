---
type: job-concept
job: MulherEmForma
tags: [ai, rag, support]
updated: 2026-07-21
---

# class-ticket (node-ai)

API que usa IA (Google Gemini) para **classificar tickets de suporte** (categoria, severidade, área, resumo, tags, análise), armazenar embeddings no **Qdrant** para busca semântica, responder perguntas via **RAG** com contexto de tickets similares, e integrar com **Jira** para abrir issues sob demanda atribuídas a um dev específico.

## Stack
Node.js 20 + TypeScript, Fastify 5, Gemini (geração + `gemini-embedding-001`), Qdrant, MongoDB, Firebase Admin (auth), Zod, Biome.

## Fluxo (inferido)
[[wiki/job/MulherEmForma/ticket-support]] (formulário) → `class-ticket` (classifica + RAG) → cria issue no Jira sob demanda → revisão no [[wiki/job/MulherEmForma/mef-backoffice-cs]].

## Sources
- [[raw/job/MulherEmForma/class-ticket.md]]
