# Wiki Log

Append-only record of all operations. Parse with: `grep "^## \[" log.md | tail -10`

---

## [2026-04-20] setup | — | Initial wiki structure created
## [2026-04-20] seed | personal | Tech stack and self-model seeded from portfolio data
## [2026-04-20] setup | subjects | Created overview pages for C24, C09, S03, S02, S07
## [2026-04-20] ingest | book | Engenharia de Software Moderna — initial notes (Brooks properties, functional vs non-functional requirements)
## [2026-04-20] create | personal | Team leadership setup — raw/job/Team.md + wiki/personal/team-lead.md
## [2026-04-20] ingest | job | Team.md — added Igor (Intern, FE) and Eduardo (Junior, BE)
## [2026-04-20] ingest | subject | S02-BD2 Neo4j lab — graph model, Cypher, Python driver, DAO pattern
## [2026-04-20] ingest | journal | 2026-04-20 — motivation for the vault, anxiety and confidence about new challenges
## [2026-04-20] ingest | subject | C24 MLP Neural Network — RNAs, neurônio artificial, DNNs, backpropagation
## [2026-04-20] ingest | subject | C09 aula 1 — Multimídia e Hipermídia (linear, não linear, hipermídia)
## [2026-04-21] ingest | subject | S03 Practice exam NP1 — pilares OO, arq vs eng, padrões (MVC/SPA/MOM/SOA), princípios de design (coesão/acoplamento/SOLID/Demeter)
## [2026-04-29] ingest | subject | C09 Operações no Domínio do Espaço — operações pixel a pixel: aritméticas (adição/subtração/multiplicação/divisão/blending) e lógicas (AND/OR/XOR/NOT), imagens binárias
## [2026-07-21] cleanup | index | raw/job/* (Architecture, Project Infos, Team), raw/journal/2026-04-20, wiki/personal/tech-stack e team-lead removidos do disco pelo usuário — links quebrados retirados de index.md e self-model.md
## [2026-07-21] ingest | subject | C09 Listas de Atividade 3-5 e Reposições 1-2, Compressão de imagens, Dados Multimídia, Filtragem — dados multimídia, compressão (JPEG/wavelets/fractais/MPEG), geometria (transformações/projeção/pipeline gráfico), cor (RGB/CMYK/HSV), curvas (Hermite/Bézier/B-spline), iluminação/shading/Z-Buffer/ray tracing, vídeo analógico/TV digital/displays
## [2026-07-21] ingest | subject | C24 Atividade k-means — clustering, método do cotovelo, escolha de k
## [2026-07-21] ingest | subject | S03 Practice exam NP2 (padrões de projeto GoF), Padrões Arquiteturais (fonte adicional), Relatorio 2/3 (retrospectiva do projeto NP2 e peer review de outras equipes)
## [2026-07-21] ingest | subject | S07 NP2-Project — integrantes do grupo (João Victor, Igor, Gabriel, Tobias) e desenho base de arquitetura (Front/Back, Jenkins, Messenger)
## [2026-07-21] setup | subject | Fetin criado — ideação de app de monetização para personal trainers/nutricionistas (nome da disciplina não confirmado)
## [2026-07-21] ingest | subject | Fetin — explorado o projeto real em ~/Documents/fetin/ (app, web, bff, core, assistant, video); produto se chama OneFit. Criada raw/subjects/Fetin/Arquitetura.md e wiki/subjects/Fetin/arquitetura.md descrevendo os 6 serviços, domínios do BFF e a arquitetura RAG do assistant (Qdrant + Gemini)
## [2026-07-21] reclassify | project | Fetin movido de raw/subjects + wiki/subjects para raw/projects + wiki/projects (é um projeto, não uma disciplina). CLAUDE.md atualizado com o layout e os formatos de página de projeto
## [2026-07-21] ingest | project | Explorados os 11 repositórios em ~/Documents/projects/: Success+success-api (finanças pessoais), RochaProperty+back-rocha-property (imobiliária), FinAI (assistente financeiro conversacional, Expo), XP-Engineer-API+easy-eng+xp-engineer-admin (aprendizado gamificado de engenharia), Oi-Fit-API (protótipo anterior ao OneFit), start (painel RoboBulls), api-boilerplate (scaffold Fastify base de outras APIs), portfolio (site pessoal), onefit-dashboard (painel OneFit em repo separado, adicionado à arquitetura do Fetin). Criadas 8 novas pastas em raw/projects/ e wiki/projects/
## [2026-07-21] ingest | project | Varredura de ~/Documents/ (exceto "bolsa") encontrou Up-vox ainda não catalogado: Profissao-Laser-API + profissao_laser (geração antiga, LMS de laser/estética) e upvox-api + api-gateway (refactor em andamento para o modelo Course/Plan/Tool/Vox com billing de duas portas via Stripe/Supabase/Bunny Stream). Criadas raw/projects/UpVox/Arquitetura.md e wiki/projects/UpVox/{UpVox-overview, arquitetura}
## [2026-07-21] reclassify | job | Up-vox movido de raw/projects + wiki/projects para raw/job + wiki/job (é o projeto do trabalho remunerado do Tobias, não um projeto pessoal). Frontmatter ajustado (type: job-overview/job-concept). CLAUDE.md atualizado com o formato de página job e a distinção job vs. projects
## [2026-07-21] ingest | job | Explorados os docs/superpowers/ e a pasta landing/ dos 3 repos do Up-vox. Fábrica de Ferramentas (builder de tools de IA, handoff em 2026-06-10, PRs abertos sem merge, nota de segurança sobre trava SSRF em util.http_request), Vitrine de Projetos da comunidade (feature social do produto), roadmap com ~25 specs/plans indexados por repo/data, e a landing page standalone (HTML/React fora do build do Next.js). Criados 4 raw + 3 wiki novos em raw/job/UpVox e wiki/job/UpVox
## [2026-07-21] setup | job | Novo job: estágio na Mulher em Forma (MEF). Via GitHub API (org Mulher-em-Forma, `gh api search/commits`), identificados 19 repositórios com commits do Tobias entre ~50 no total: mef-backoffice-cs e backoffice-bff (maior atividade recente — CS/backoffice), bff (GraphQL BFF do app), class-ticket (IA de classificação de tickets + RAG + Jira), mef-core-api-v1 (Strapi, core de negócio), mef-social-api, mef-analytics-backoffice, mef-web-apps-hub, mef-survey, ticket-support, e outros de menor volume (nutritional-assistant, migrations, plans, landing, notifications, flow-view, strapi-migrator, mef-content-api-v2, journey-temporal-app). Sem acesso local aos repos — levantamento via README/estrutura/commits pela API do GitHub.
## [2026-07-21] refactor | job | MulherEmForma separado: 1 arquivo raw + 1 página wiki por repositório (lendo README completo + últimos commits do Tobias de cada um via API do GitHub), substituindo os 2 arquivos consolidados anteriores (Arquitetura.md/MeusRepositorios.md). Overview atualizado com os 19 links, agrupados por volume de atividade
