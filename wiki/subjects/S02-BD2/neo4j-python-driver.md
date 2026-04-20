---
type: concept
subject: S02-BD2
tags: [neo4j, python, driver, dao]
updated: 2026-04-20
---

# Neo4j Python Driver

Biblioteca oficial `neo4j` para conectar e executar queries Cypher a partir de Python.

## Configuração básica

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver(
    "bolt://localhost:7687",
    auth=("neo4j", "senha")
)
```
- Protocolo: **Bolt** (binário, eficiente)
- Porta padrão: `7687`

## Executando queries

```python
driver.execute_query(
    "MERGE (g:Guild {name: $name}) SET g.description = $description",
    name=guild.name,
    description=guild.description,
)
```
- Parâmetros passados como kwargs — **nunca interpolar strings** (evita injection)
- Retorna um objeto com `.records` (lista de resultados)

## Lendo resultados

```python
result = driver.execute_query("MATCH (g:Guild) RETURN g.name AS name")
for r in result.records:
    print(r["name"])
```

## Padrão DAO aplicado

O exercício usa o padrão **DAO (Data Access Object)**: cada entidade tem uma classe dedicada (`GuildDAO`, `AdventurerDAO`, `QuestDAO`) que encapsula todas as queries relacionadas. Isso separa a lógica de acesso a dados do resto da aplicação.

```
GuildDAO      → add_guild, get_guilds_by_specialization
AdventurerDAO → add_adventurer, get_available_quests, promote_adventurer
QuestDAO      → add_quest, delete_quest
```

## Singleton do driver

O exercício usa um padrão Singleton (`Neo4jDriver.get_driver()`) para reutilizar a conexão. Criar um novo driver a cada operação é custoso.

## Cross-references
- [[wiki/subjects/S02-BD2/neo4j-graph-model]]
- [[wiki/subjects/S02-BD2/cypher-query-language]]
- [[wiki/subjects/S02-BD2/S02-BD2-overview]]
