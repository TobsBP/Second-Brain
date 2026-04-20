---
type: concept
subject: S02-BD2
tags: [neo4j, cypher, query-language]
updated: 2026-04-20
---

# Cypher Query Language

Linguagem declarativa do Neo4j para consultar e manipular grafos. Usa uma sintaxe visual onde padrões de grafo se parecem com diagramas ASCII.

## Comandos principais

### MERGE
Cria o nó/relacionamento se não existir; encontra se já existir. Equivalente a "upsert". Evita duplicatas.
```cypher
MERGE (g:Guild {name: $name})
SET g.description = $description
```

### MATCH
Encontra padrões no grafo.
```cypher
MATCH (a:Adventurer {name: $name})-[:MEMBER_OF]->(g:Guild)-[:HAS_SPECIALIZATION]->(s:Specialization)
```

### UNWIND
Expande uma lista em linhas individuais — essencial para inserir relacionamentos a partir de listas.
```cypher
UNWIND $specializations AS spec
MERGE (s:Specialization {name: spec})
MERGE (g)-[:HAS_SPECIALIZATION]->(s)
```

### WITH
Passa resultados de uma cláusula para a próxima (como um pipe). Necessário para encadear MATCH após MERGE.
```cypher
MERGE (g:Guild {name: $name})
WITH g
UNWIND $specializations AS spec
...
```

### SET
Atualiza propriedades de um nó ou relacionamento.
```cypher
SET a.rank = $new_rank, a.skills = a.skills + [$new_skill]
```
> **Nota:** `a.skills + [$new_skill]` é como se acrescenta um item a uma lista sem perder os anteriores.

### DETACH DELETE
Remove um nó **e todos os seus relacionamentos**. Sem DETACH, deletar um nó com relacionamentos gera erro.
```cypher
MATCH (q:Quest {title: $title}) DETACH DELETE q
```

### RETURN e ORDER BY
```cypher
RETURN g.name AS name, g.headquarters AS headquarters
ORDER BY g.name
```

### WHERE com funções de lista
```cypher
WHERE a.rank IN q.required_ranks
  AND any(spec IN guild_specs WHERE spec IN q.required_specializations)
```
- `IN` — verifica pertencimento a lista
- `any(x IN list WHERE condition)` — verdadeiro se pelo menos um elemento satisfaz

### DISTINCT e collect()
```cypher
WITH a, collect(DISTINCT s.name) AS guild_specs
RETURN DISTINCT q.title AS title
```
- `collect()` — agrega valores em lista
- `DISTINCT` — elimina duplicatas

## Cross-references
- [[wiki/subjects/S02-BD2/neo4j-graph-model]]
- [[wiki/subjects/S02-BD2/neo4j-python-driver]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
