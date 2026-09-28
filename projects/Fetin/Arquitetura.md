Levantamento da arquitetura real do projeto Fetin (nome de produto: **OneFit**), a partir dos repositórios em `~/Documents/fetin/` (app, web, bff, core, assistant, video).

## Serviços

- **app** — Expo Router v57 (React Native, `src/` layout, file-based routing). Auth (login, registro, esqueci senha, biometria), tabs Home/Diet/Workout/Social, perfil.
- **web** — Next.js 16 (App Router, Turbopack) + Tailwind v4. Dashboards por papel via RBAC sobre Firebase Auth: cliente, personal trainer, nutricionista, personal chef, administrador. Dados via TanStack Query + graphql-request.
- **bff** — GraphQL BFF (Apollo Server v5 standalone, Node 24+, TS strict, ESM). Único consumidor: o app/web. Fica entre o cliente e dois backends: Firebase Auth (verificação de ID token) e o Strapi (`core`) — o cliente nunca fala com o Strapi diretamente. Upload de mídia via Cloudflare R2 (URL assinada), cache opcional via Redis, logging com Pino.
- **core** — Strapi v5 (CMS/persistência). Content-types: appointment, availability, available-date, client, daily-task, engagement, exercise, food, group, group-comment, group-post, meal-plan, menu, professional, recipe, review, service-location, terms-of-use, workout.
- **assistant** — serviço RAG separado do bff. Chatbot que ajuda um `professional` (personal/nutricionista/personal chef) a montar treinos, planos alimentares e receitas para um `client`, buscando no catálogo (exercícios/alimentos/receitas) indexado no Qdrant e respondendo com Gemini via SSE.
- **video** — projeto Remotion para gerar vídeo promocional (não faz parte da arquitetura de produção).

## Papéis de usuário
Cliente, Personal Trainer, Nutricionista, Personal Chef, Administrador — cada um com dashboard próprio no `web` e fluxos específicos no `app`.

## Módulos de domínio do BFF
professional, client, engagement (pedido/aceite/rejeição/encerramento do relacionamento cliente↔profissional), appointment/availability/available-date, food (com cache Redis), recipe, meal-plan (library → assign), menu, workout (library → assign), daily-task, service-location, message, group/group-post/group-comment, review.

## Arquitetura do assistant (RAG)
- **O que é indexado no Qdrant:** apenas catálogo reutilizável — exercise (global + próprios do personal), food (biblioteca global + próprios do nutricionista), recipe (sempre próprio do nutricionista/chef). Um ponto por registro, com payload incluindo `professionalId` (null = global).
- **O que NÃO é indexado, é buscado ao vivo no Strapi:** contexto do cliente (goal, restriction, plan, mealsPerWeek) e o workout/meal-plan ACTIVE atual — dado sensível, sempre fresco, nunca vai pro vetor store.
- **Regra de escopo de busca:** `professionalId IS NULL OR professionalId == <profissional que chama>` E `contentType` de acordo com o tipo do profissional (PERSONAL→exercise, NUTRITIONIST→food+recipe, PERSONAL_CHEF→recipe).
- **Ingestão:** webhooks do Strapi (`entry.create/update/delete` em exercise/food/recipe) mantêm o índice em dia; `npm run reindex` faz backfill/recuperação de drift.
- **Fluxo do chat:** `POST /chat` com Firebase ID token → resolve professional → confirma engagement ACTIVE com o client → busca contexto do cliente no Strapi → embeda a mensagem, top-K no Qdrant → monta prompt (persona + contexto + trechos recuperados + histórico enviado pelo frontend) → stream via SSE.
- **Fora de escopo v1 (decisão deliberada, não limitação técnica):** o assistant nunca escreve no Strapi (só recomenda; o profissional cria o registro real no app/web existente); sem histórico de chat persistido no servidor (frontend guarda o transcript); sem uso de ferramentas agenticas no meio da conversa (retrieval acontece uma vez por turno); sem fine-tuning; sem rate limiting ainda (flagged como follow-up).

## Observação
O nome "Fetin" nos diretórios da vault é o codinome do projeto/curso; o produto em si se chama **OneFit** (confirmado no README do `web` e nos arquivos de configuração Firebase).
