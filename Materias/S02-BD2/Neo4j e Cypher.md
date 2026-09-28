Teoria por trás do lab em [[Neo4j test]]. O Neo4j é um banco NoSQL **orientado a grafos**: guarda os dados como nós ligados por relacionamentos, e os dois podem ter propriedades. Funciona muito bem quando os dados são muito conectados e a relação importa tanto quanto o dado em si.

## Modelo de grafo

| Elemento | O que é | No lab |
|---|---|---|
| **Nó** | Uma entidade | um aventureiro, uma guilda |
| **Label** | Tipo do nó (um nó pode ter várias) | `:Guild`, `:Adventurer`, `:Quest`, `:Specialization` |
| **Relacionamento** | Aresta **com direção e tipo** entre dois nós | `-[:MEMBER_OF]->`, `-[:HAS_SPECIALIZATION]->` |
| **Propriedade** | Chave-valor em nó ou relacionamento | `name`, `rank`, `reward` |

Tipos de propriedade:
- Primitivos: string, inteiro, float, booleano
- Temporais: `date(...)`, ex.: `date($registration_date)` guarda a string `"2023-03-15"` como DATE de verdade
- Listas de valores simples: `["Combate", "Magia"]`, guardadas direto no nó (é o caso de `required_ranks` na `:Quest`)

Modelo do lab:
```
(:Adventurer)-[:MEMBER_OF]->(:Guild)-[:HAS_SPECIALIZATION]->(:Specialization)
(:Quest) com as listas required_ranks[] e required_specializations[]
```

Decisão de modelagem que aparece no lab: especialização virou **nó** (várias guildas compartilham o mesmo nó `:Specialization`, dá para navegar por ele), enquanto os requisitos da missão ficaram como **lista** (só servem para filtrar, ninguém navega a partir deles).

### Neo4j x relacional
- **Neo4j:** travessias de grafo, recomendação, redes sociais, hierarquias e permissões. Seguir um relacionamento custa o mesmo independentemente do tamanho do banco, sem JOIN.
- **Relacional:** dados tabulares, agregações simples, relatórios, esquema rígido.

## Cypher

Linguagem declarativa do Neo4j. O padrão da query parece um desenho ASCII do grafo: `(nó)-[:REL]->(nó)`.

### MATCH
Procura um padrão no grafo.
```cypher
MATCH (a:Adventurer {name: $name})-[:MEMBER_OF]->(g:Guild)-[:HAS_SPECIALIZATION]->(s:Specialization)
```

### CREATE x MERGE
- `CREATE` cria **sempre**, então rodar duas vezes duplica.
- `MERGE` funciona como "acha ou cria" (upsert). O match é feito **no padrão inteiro**, por isso o certo é fazer o `MERGE` só pela chave e usar `SET` para o resto.
```cypher
MERGE (g:Guild {name: $name})
SET g.description = $description, g.headquarters = $headquarters
```
- `MERGE (g)-[:HAS_SPECIALIZATION]->(s)` com `g` e `s` já existentes cria só o relacionamento, se ele ainda não existir.
- Também existem `ON CREATE SET` e `ON MATCH SET`, para setar valores diferentes dependendo de ter criado ou achado.

### WHERE
Filtra os resultados.
```cypher
WHERE a.rank IN q.required_ranks
  AND any(spec IN guild_specs WHERE spec IN q.required_specializations)
```
- `IN`: verifica se o valor está numa lista
- `any(x IN lista WHERE cond)`: verdadeiro se pelo menos um elemento satisfaz (existem também `all`, `none` e `single`)

### RETURN, ORDER BY, DISTINCT
```cypher
RETURN g.name AS name, g.headquarters AS headquarters
ORDER BY name
```
- `AS` define o nome da coluna, que é a chave usada no Python (`r["name"]`).
- `DISTINCT` tira duplicatas. Depois de um `RETURN DISTINCT` ou de uma agregação, o `ORDER BY` deve usar o que foi projetado (os aliases).

### SET
Atualiza propriedades.
```cypher
SET a.rank = $new_rank, a.skills = a.skills + [$new_skill]
```
- `lista + [item]` acrescenta um item **sem apagar** os anteriores.

### WITH
Passa o resultado de uma parte da query para a próxima (tipo um pipe). É obrigatório para encadear, por exemplo, um `MERGE` com um `UNWIND` depois, ou para agregar antes de continuar.
```cypher
MATCH (a:Adventurer {name: $name})-[:MEMBER_OF]->(:Guild)-[:HAS_SPECIALIZATION]->(s)
WITH a, collect(DISTINCT s.name) AS guild_specs
MATCH (q:Quest) ...
```
- `collect()` agrega valores numa lista (tipo um `GROUP BY` implícito pelas outras variáveis do `WITH`).

### UNWIND
Transforma uma lista em linhas, uma por elemento. É o que permite criar um relacionamento para cada item de uma lista.
```cypher
UNWIND $specializations AS spec
MERGE (s:Specialization {name: spec})
MERGE (g)-[:HAS_SPECIALIZATION]->(s)
```
- Se a lista for vazia, o `UNWIND` gera zero linhas e o que vem depois dele não roda (o que veio antes já rodou).

### DELETE x DETACH DELETE
- `DELETE` apaga o nó, mas **dá erro** se ele ainda tiver relacionamentos.
- `DETACH DELETE` apaga o nó **e** todos os relacionamentos dele.
```cypher
MATCH (q:Quest {title: $title}) DETACH DELETE q
```

## Driver Python

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver("bolt://localhost:7687", auth=("neo4j", "senha"))

# atalho (driver 5.x): abre sessão + transação, faz commit e devolve o resultado
records, summary, keys = driver.execute_query(
    "MATCH (g:Guild) WHERE g.headquarters = $hq RETURN g.name AS name",
    hq="Heroica",
)
for r in records:
    print(r["name"])

# jeito "manual": sessão + transação gerenciada
def busca(tx, hq):
    return [r["name"] for r in tx.run(
        "MATCH (g:Guild {headquarters: $hq}) RETURN g.name AS name", hq=hq)]

with driver.session() as session:
    nomes = session.execute_read(busca, "Heroica")

driver.close()
```
- **Bolt:** protocolo binário do Neo4j, na porta padrão 7687.
- **Driver:** pool de conexões, caro de criar. Crie um só e reaproveite (no lab isso fica no Singleton `Neo4jDriver.get_driver()`).
- **Sessão:** contexto leve e descartável onde as transações rodam. Abre com `with` para fechar sozinha.
- **Transação:** unidade atômica (ou tudo commita, ou nada). Existem três jeitos de usar:
  - gerenciada: `execute_read` / `execute_write`, com retry automático em falha transitória
  - explícita: `session.begin_transaction()`, com commit/rollback na mão
  - auto-commit: `session.run(...)`
- **Parâmetros:** use `$param` na query e passe como kwargs, nunca concatenando string. Isso evita injection e deixa o banco reaproveitar o plano da query.

### DAO no lab
Cada entidade tem uma classe que concentra as queries dela, separando o acesso a dados do resto do código:
```
GuildDAO      → add_guild, get_guilds_by_specialization
AdventurerDAO → add_adventurer, get_available_quests, promote_adventurer
QuestDAO      → add_quest, delete_quest
```
