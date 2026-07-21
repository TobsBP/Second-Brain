---
type: job-concept
job: UpVox
tags: [community, feature-spec]
updated: 2026-07-21
---

# Comunidade — Vitrine de Projetos

Feature de comunidade dentro do produto UpVox/Profissão Laser: membros com plano ativo publicam projetos próprios (peças feitas a laser/artesanato), com comentários e curtidas — não confundir com os "projetos" de código catalogados em [[wiki/projects]].

## Rotas (`/community/projects`)
GET (listar, com paginação/filtros `material`/`technique`/`search`/`sort`), GET por id (com comentários), POST (criar — membro com plano ativo), PATCH/DELETE (admin), GET/POST de comentários (`/community/projects/{id}/comments`).

No momento do doc: listagem já existia (a expandir), criação já existia; detalhe, update, delete e comentários estavam **a implementar**.

## Sources
- [[raw/job/UpVox/ComunidadeVitrineProjetos.md]]
