Transcrito de `profissao_laser/docs/community-projects-api-routes.md`.

## O que é
"Vitrine de Projetos" — feature de comunidade onde membros com plano ativo publicam seus próprios projetos (peças feitas a laser/artesanato), com comentários e curtidas.

## Rotas (backend Profissão Laser)
Base: `NEXT_PUBLIC_API_URL`. Escrita exige autenticação; PATCH/DELETE de projetos exigem admin; POST (criar) e comentar exigem membro com plano ativo.

| Método | Rota | Descrição | Estado (no doc) |
|---|---|---|---|
| GET | `/community/projects` | Lista projetos (paginação, filtros, ordenação) | já existe, expandir |
| GET | `/community/projects/{id}` | Detalhe (com comentários) | a implementar |
| POST | `/community/projects` | Criar projeto | já existe |
| PATCH | `/community/projects/{id}` | Atualizar (admin) | a implementar |
| DELETE | `/community/projects/{id}` | Remover (admin) | a implementar |
| GET | `/community/projects/{id}/comments` | Listar comentários | a implementar |
| POST | `/community/projects/{id}/comments` | Adicionar comentário | a implementar |

Filtros de listagem: `material`, `technique`, `search` (título/autor), `sort` (`recent`\|`likes`), paginação (`page`, `limit`, default 12).

## Observação
Não confundir com os "projects" de código (Success, RochaProperty etc.) documentados em `wiki/projects/` — este é um recurso do **produto** UpVox/Profissão Laser (uma vitrine social dentro do app), tratado aqui como parte do domínio do job.
