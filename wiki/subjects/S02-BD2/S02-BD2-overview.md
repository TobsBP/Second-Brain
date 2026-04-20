---
type: subject-overview
code: S02
title: Banco de Dados 2
updated: 2026-04-20
classes_ingested: 1
---

# S02 — Banco de Dados 2

## Topics covered so far
- Neo4j e bancos de dados em grafo
- Modelo de grafo: nós, relacionamentos, propriedades
- Cypher Query Language: MERGE, MATCH, UNWIND, WITH, SET, DETACH DELETE
- Python Neo4j Driver e padrão DAO

## Concepts
- [[wiki/subjects/S02-BD2/neo4j-graph-model]] — Nós, relacionamentos, propriedades e labels
- [[wiki/subjects/S02-BD2/cypher-query-language]] — Sintaxe e comandos do Cypher
- [[wiki/subjects/S02-BD2/neo4j-python-driver]] — Driver Python e padrão DAO

## Cross-references
- Conecta a [[wiki/personal/tech-stack]] — Neo4j já está no stack
- Padrão DAO conecta a [[wiki/subjects/S03-ArqSoft/S03-ArqSoft-overview]] — padrões de arquitetura

## Class log
| Aula | Data | Tópico | Ingerido |
|------|------|--------|----------|
| Prática | 2026-04-20 | Neo4j com Python — Guild/Adventurer/Quest | 2026-04-20 |

## Key questions to review
- Qual a diferença entre `MERGE` e `CREATE` no Cypher?
- Como adicionar um item a uma lista existente numa propriedade sem perder os anteriores?
- Por que usar `DETACH DELETE` em vez de `DELETE`?
- O que faz o `UNWIND` e quando é necessário?
- Como funciona `any(x IN list WHERE condition)` no Cypher?
- Qual a diferença entre `WITH` e `RETURN`?
- Quando Neo4j é preferível a um banco relacional?
- O que é o padrão DAO e qual sua vantagem?

## Sources
- [[raw/subjects/S02-BD2/Neo4j test.md]] — exercício prático com Guild, Adventurer, Quest
