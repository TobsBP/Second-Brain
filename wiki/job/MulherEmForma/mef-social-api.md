---
type: job-concept
job: MulherEmForma
tags: [social, fastify]
updated: 2026-07-21
---

# mef-social-api

Rede social V2 da MEF (Node.js + Fastify): perfis, posts, comentários, curtidas, amizades, grupos.

## Arquitetura
`REST API → Controllers → Services → Repositories`, por domínio (User, Post, Comment, Like, Friendship, Group). Consome o serviço [[wiki/job/MulherEmForma/notifications]] externamente; é uma das fontes agregadas pelo [[wiki/job/MulherEmForma/bff]].

## Sources
- [[raw/job/MulherEmForma/mef-social-api.md]]
