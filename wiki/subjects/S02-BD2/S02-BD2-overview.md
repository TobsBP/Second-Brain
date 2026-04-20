---
type: subject-overview
code: S02
title: Database 2
updated: 2026-04-20
classes_ingested: 1
---

# S02 — Database 2

## Topics covered so far
- Neo4j and graph databases
- Graph model: nodes, relationships, properties
- Cypher Query Language: MERGE, MATCH, UNWIND, WITH, SET, DETACH DELETE
- Python Neo4j Driver and DAO pattern

## Concepts
- [[wiki/subjects/S02-BD2/neo4j-graph-model]] — Nodes, relationships, properties, and labels
- [[wiki/subjects/S02-BD2/cypher-query-language]] — Cypher syntax and commands
- [[wiki/subjects/S02-BD2/neo4j-python-driver]] — Python driver and DAO pattern

## Cross-references
- Connects to [[wiki/personal/tech-stack]] — Neo4j already in the stack
- DAO pattern connects to [[wiki/subjects/S03-ArqSoft/S03-ArqSoft-overview]] — architectural patterns

## Class log
| Class | Date | Topic | Ingested |
|-------|------|-------|----------|
| Lab | 2026-04-20 | Neo4j with Python — Guild/Adventurer/Quest | 2026-04-20 |

## Key questions to review
- What is the difference between `MERGE` and `CREATE` in Cypher?
- How do you append an item to a list property without losing existing values?
- Why use `DETACH DELETE` instead of `DELETE`?
- What does `UNWIND` do and when is it needed?
- How does `any(x IN list WHERE condition)` work in Cypher?
- What is the difference between `WITH` and `RETURN`?
- When is Neo4j preferable over a relational database?
- What is the DAO pattern and what is its advantage?

## Sources
- [[raw/subjects/S02-BD2/Neo4j test.md]] — practical exercise with Guild, Adventurer, Quest
