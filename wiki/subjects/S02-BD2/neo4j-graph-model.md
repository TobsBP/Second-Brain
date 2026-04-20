---
type: concept
subject: S02-BD2
tags: [neo4j, graph-database, data-model]
updated: 2026-04-20
---

# Neo4j — Graph Model

Neo4j stores data as a graph of **nodes** and **relationships**, both of which can hold properties. It excels at highly connected data where relationships are as important as the data itself.

## Core elements

| Element | Description | Exercise example |
|---------|-------------|-----------------|
| **Node** | Entity, labeled with `:Label` | `:Guild`, `:Adventurer`, `:Quest`, `:Specialization` |
| **Relationship** | Directed edge between two nodes | `-[:HAS_SPECIALIZATION]->`, `-[:MEMBER_OF]->` |
| **Property** | Key-value attribute on a node or relationship | `name`, `rank`, `reward` |
| **Label** | Type of a node (a node can have multiple) | `:Guild`, `:Adventurer` |

## Supported property types
- Primitives: `string`, `integer`, `float`, `boolean`
- Temporal: `date(...)` — e.g. `date($registration_date)`
- Lists: `["Combat", "Magic"]` — stored directly as a node property

## Exercise model

```
(:Adventurer)-[:MEMBER_OF]->(:Guild)-[:HAS_SPECIALIZATION]->(:Specialization)
(:Quest) — properties: required_ranks[], required_specializations[]
```

## When to use Neo4j vs relational
- **Neo4j**: graph traversals, recommendations, social networks, hierarchical permissions
- **Relational**: tabular data, simple aggregations, reporting

## Cross-references
- [[wiki/subjects/S02-BD2/cypher-query-language]]
- [[wiki/subjects/S02-BD2/neo4j-python-driver]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
