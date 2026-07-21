Levantado via GitHub API (`Mulher-em-Forma/class-ticket`) em 2026-07-21.

## README (nome interno: `node-ai`)
"A REST API that uses AI to classify support tickets, store them in a vector database, and answer questions using RAG. Integrates with Jira to create issues on demand for tickets that need deeper handling."

### Features
- **Ticket classification** — IA analisa tickets recebidos e atribui categoria, severidade, área, resumo, tags e uma análise detalhada.
- **Jira integration** — cria uma issue no Jira sob demanda, atribuindo a um dev específico.
- **Vector storage** — tickets são embedados e guardados no Qdrant para busca semântica.
- **RAG chat** — responde perguntas usando contexto de tickets similares passados.
- **Firebase auth** — criação de ticket exige um ID token Firebase válido.
- **File upload** — aceita imagens/arquivos via multipart/form-data.

### Stack
Node.js 20, TypeScript, Fastify 5, Google Gemini (geração + embeddings via `gemini-embedding-001`), Qdrant, MongoDB, Firebase Admin, Zod, Biome.

### Endpoints (parcial)
`GET /tickets` — lista tickets.

## Relação
Recebe tickets provavelmente do formulário `ticket-support` e é consumido pelo backoffice (`mef-backoffice-cs`/`backoffice-bff`) para a revisão de tickets/resoluções.
