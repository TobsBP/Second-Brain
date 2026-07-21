Levantado via GitHub API (`Mulher-em-Forma/notifications`) em 2026-07-21.

## README
"API de Notificações do MEF (`mef-notifications-api`), aplicação em Node.js, Fastify e Prisma. Responsável por gerenciar e disparar notificações para os usuários da plataforma, incluindo Push Notifications (via Expo/Firebase) e SMS."

### Arquitetura (diagrama mermaid no README)
```
Consumers: BFF, N8N, Social API
Notifications Service: REST API → Controllers → Services → Repositories
```

## Relação
Consumida por `bff`, `mef-social-api` e por automações via n8n.
