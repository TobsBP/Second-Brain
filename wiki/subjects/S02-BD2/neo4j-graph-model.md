---
type: concept
subject: S02-BD2
tags: [neo4j, graph-database, data-model]
updated: 2026-04-20
---

# Neo4j — Graph Model

Neo4j organiza dados como um grafo de **nós** (nodes) e **relacionamentos** (relationships), ambos podendo ter propriedades. É ideal para dados altamente conectados onde relações são tão importantes quanto os próprios dados.

## Elementos fundamentais

| Elemento | Descrição | Exemplo do exercício |
|----------|-----------|---------------------|
| **Node** | Entidade, rotulada com `:Label` | `:Guild`, `:Adventurer`, `:Quest`, `:Specialization` |
| **Relationship** | Aresta direcionada entre dois nós | `-[:HAS_SPECIALIZATION]->`, `-[:MEMBER_OF]->` |
| **Property** | Atributo chave-valor num nó ou relacionamento | `name`, `rank`, `reward` |
| **Label** | Tipo do nó (pode ter múltiplos) | `:Guild`, `:Adventurer` |

## Tipos de propriedades suportados
- Primitivos: `string`, `integer`, `float`, `boolean`
- Temporal: `date(...)` — ex.: `date($registration_date)`
- Listas: `["Combate", "Magia"]` — armazenadas diretamente como propriedade do nó

## Modelo do exercício

```
(:Adventurer)-[:MEMBER_OF]->(:Guild)-[:HAS_SPECIALIZATION]->(:Specialization)
(:Quest) — propriedades: required_ranks[], required_specializations[]
```

## Quando usar Neo4j vs relacional
- **Neo4j**: travessias de grafo, recomendações, redes sociais, permissões hierárquicas
- **Relacional**: dados tabulares, agregações simples, relatórios

## Cross-references
- [[wiki/subjects/S02-BD2/cypher-query-language]]
- [[wiki/subjects/S02-BD2/neo4j-python-driver]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
