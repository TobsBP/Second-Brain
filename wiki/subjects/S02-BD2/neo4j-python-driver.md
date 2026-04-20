---
type: concept
subject: S02-BD2
tags: [neo4j, python, driver, dao]
updated: 2026-04-20
---

# Neo4j Python Driver

The official `neo4j` library for connecting to Neo4j and executing Cypher queries from Python.

## Basic setup

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver(
    "bolt://localhost:7687",
    auth=("neo4j", "password")
)
```
- Protocol: **Bolt** (binary, efficient)
- Default port: `7687`

## Running queries

```python
driver.execute_query(
    "MERGE (g:Guild {name: $name}) SET g.description = $description",
    name=guild.name,
    description=guild.description,
)
```
- Parameters passed as kwargs — **never interpolate strings** (prevents injection)
- Returns an object with `.records` (list of results)

## Reading results

```python
result = driver.execute_query("MATCH (g:Guild) RETURN g.name AS name")
for r in result.records:
    print(r["name"])
```

## DAO pattern applied

The exercise uses the **DAO (Data Access Object)** pattern: each entity has a dedicated class (`GuildDAO`, `AdventurerDAO`, `QuestDAO`) that encapsulates all related queries. This separates data access logic from the rest of the application.

```
GuildDAO      → add_guild, get_guilds_by_specialization
AdventurerDAO → add_adventurer, get_available_quests, promote_adventurer
QuestDAO      → add_quest, delete_quest
```

## Driver Singleton

The exercise uses a Singleton pattern (`Neo4jDriver.get_driver()`) to reuse the connection. Creating a new driver on every operation is expensive.

## Cross-references
- [[wiki/subjects/S02-BD2/neo4j-graph-model]]
- [[wiki/subjects/S02-BD2/cypher-query-language]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
