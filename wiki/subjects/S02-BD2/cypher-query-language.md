---
type: concept
subject: S02-BD2
tags: [neo4j, cypher, query-language]
updated: 2026-04-20
---

# Cypher Query Language

Neo4j's declarative language for querying and manipulating graphs. Uses a visual syntax where graph patterns resemble ASCII diagrams.

## Main commands

### MERGE
Creates a node/relationship if it doesn't exist; finds it if it does. Equivalent to "upsert". Prevents duplicates.
```cypher
MERGE (g:Guild {name: $name})
SET g.description = $description
```

### MATCH
Finds patterns in the graph.
```cypher
MATCH (a:Adventurer {name: $name})-[:MEMBER_OF]->(g:Guild)-[:HAS_SPECIALIZATION]->(s:Specialization)
```

### UNWIND
Expands a list into individual rows — essential for creating relationships from a list of values.
```cypher
UNWIND $specializations AS spec
MERGE (s:Specialization {name: spec})
MERGE (g)-[:HAS_SPECIALIZATION]->(s)
```

### WITH
Pipes results from one clause to the next. Required to chain MATCH after MERGE.
```cypher
MERGE (g:Guild {name: $name})
WITH g
UNWIND $specializations AS spec
...
```

### SET
Updates properties on a node or relationship.
```cypher
SET a.rank = $new_rank, a.skills = a.skills + [$new_skill]
```
> **Note:** `a.skills + [$new_skill]` appends an item to a list without losing existing values.

### DETACH DELETE
Removes a node **and all its relationships**. Without DETACH, deleting a node that has relationships raises an error.
```cypher
MATCH (q:Quest {title: $title}) DETACH DELETE q
```

### RETURN and ORDER BY
```cypher
RETURN g.name AS name, g.headquarters AS headquarters
ORDER BY g.name
```

### WHERE with list functions
```cypher
WHERE a.rank IN q.required_ranks
  AND any(spec IN guild_specs WHERE spec IN q.required_specializations)
```
- `IN` — checks membership in a list
- `any(x IN list WHERE condition)` — true if at least one element satisfies the condition

### DISTINCT and collect()
```cypher
WITH a, collect(DISTINCT s.name) AS guild_specs
RETURN DISTINCT q.title AS title
```
- `collect()` — aggregates values into a list
- `DISTINCT` — removes duplicates

## Cross-references
- [[wiki/subjects/S02-BD2/neo4j-graph-model]]
- [[wiki/subjects/S02-BD2/neo4j-python-driver]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
