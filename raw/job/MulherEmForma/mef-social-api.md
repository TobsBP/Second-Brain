Levantado via GitHub API (`Mulher-em-Forma/mef-social-api`) em 2026-07-21.

## README
"Aplicação desenvolvida com Node.js e Fastify. Responsável por gerenciar todas as interações sociais da plataforma, como perfis de usuário, postagens, comentários, amizades e grupos."

### Arquitetura (diagrama mermaid no README)
```
Gateway: BFF
Social API: REST API → Controllers → Services → Repositories
Domain Models: User, Post, Comment, Like, Friendship, Group
External: Notifications Service
Database: Postgres
```

## Relação
Consome o serviço `notifications` externamente; é uma das fontes de dados agregadas pelo `bff` (resolvers Friendship/Group/Post/Comment/Like).
