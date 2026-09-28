![[Banco de Dados Draw]]

A maioria dos problemas de performance e de dado inconsistente em backend não está no código da aplicação, está em como ela usa o banco: query sem índice, transação mal delimitada, isolamento que não protege o que a gente achava. Exemplos em PostgreSQL, mas os conceitos valem para qualquer banco relacional.

### Índice
Sem índice, achar uma linha é ler a tabela inteira (**Seq Scan**), O(n). O índice é uma estrutura separada, ordenada, que aponta para as linhas, como o índice remissivo de um livro.

#### B-tree
É o índice padrão (`CREATE INDEX` sem nada cria um B-tree). É uma árvore balanceada e rasa, com nós do tamanho de uma página de disco. Com poucos níveis (3–4) ela cobre milhões de linhas, então a busca é O(log n) em poucas leituras de disco.

Como os valores ficam **ordenados**, ele serve para:
- igualdade: `WHERE email = 'a@b.com'`
- intervalo: `WHERE criado_em > '2026-01-01'`, `BETWEEN`
- ordenação: `ORDER BY criado_em` sem precisar ordenar em memória
- prefixo: `LIKE 'abc%'`

Outros tipos: **Hash** (só igualdade), **GIN** (arrays, JSONB, full-text), **GiST** (geográfico, intervalos), **BRIN** (tabelas enormes com dado naturalmente ordenado, tipo logs por data).

```sql
CREATE INDEX idx_pedidos_cliente ON pedidos (cliente_id);
```

#### Índice composto
A ordem das colunas importa. Um índice em `(cliente_id, criado_em)` funciona como uma lista telefônica ordenada por sobrenome e depois por nome:

```sql
CREATE INDEX idx_pedidos_cliente_data ON pedidos (cliente_id, criado_em);

WHERE cliente_id = 7                           -- usa
WHERE cliente_id = 7 AND criado_em > '2026-01' -- usa (ideal)
WHERE criado_em > '2026-01'                    -- não usa bem: falta a primeira coluna
```
Regra do **prefixo mais à esquerda**: colunas de igualdade primeiro, depois a de intervalo/ordenação.

#### Variações úteis
- **Único**: `CREATE UNIQUE INDEX` garante unicidade no banco, e é isso que evita duplicata em concorrência. O check `if (!existe) insert` na aplicação não protege, porque duas requisições podem passar pelo `if` ao mesmo tempo.
- **Covering** (`INCLUDE`): guarda colunas extras no índice e permite um **Index Only Scan**, que responde sem tocar na tabela.
- **Parcial**: indexa só uma parte da tabela. `CREATE INDEX ... ON pedidos (criado_em) WHERE status = 'pendente'` fica pequeno se a query só busca pendentes.
- **Expressão**: `CREATE INDEX ... ON users (lower(email))` para a query `WHERE lower(email) = ...`.

#### Quando o índice não é usado
- Função ou cast na coluna: `WHERE lower(email) = ...` com índice em `email`, ou `WHERE data::date = ...`.
- `LIKE '%texto'`: sem prefixo fixo não dá pra navegar na árvore.
- **Baixa seletividade**: se a condição retorna boa parte da tabela (`WHERE ativo = true` com 90% ativos), ler tudo sequencialmente é mais barato que pular pelo índice. O planejador escolhe o Seq Scan de propósito.
- Tipo diferente: comparar coluna `int` com parâmetro `text`.

#### O custo
Índice não é de graça: **todo `INSERT`/`UPDATE`/`DELETE` atualiza todos os índices da tabela**, e cada um ocupa disco e memória. Índice que ninguém usa é só custo. No Postgres, `pg_stat_user_indexes` mostra quantas vezes cada um foi usado (`idx_scan`).

Chave estrangeira **não cria índice automaticamente** no Postgres. `pedidos.cliente_id` referenciando `clientes` sem índice deixa lento o join e também o `DELETE` do cliente.

### Transação
Um conjunto de operações que o banco trata como **uma unidade**: ou tudo acontece, ou nada.

```sql
BEGIN;
UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;
COMMIT; -- ou ROLLBACK se algo deu errado
```

#### ACID
- **Atomicidade**: tudo ou nada. Se o processo cair entre os dois `UPDATE`, nenhum vale.
- **Consistência**: a transação leva o banco de um estado válido para outro válido (constraints, FKs, `CHECK`).
- **Isolamento**: transações concorrentes não enxergam o estado intermediário uma da outra (em que medida, depende do nível; ver abaixo).
- **Durabilidade**: depois do `COMMIT`, o dado sobrevive a queda de energia. O banco grava antes no **WAL** (write-ahead log) e só então confirma.

#### Na aplicação
```typescript
await db.transaction(async (tx) => {
	await tx.update(contas).set({ saldo: sql`saldo - 100` }).where(eq(contas.id, 1));
	await tx.update(contas).set({ saldo: sql`saldo + 100` }).where(eq(contas.id, 2));
}); // exceção dentro do callback → ROLLBACK automático
```
Cuidados:
- **Transação curta.** Nada de chamada HTTP, envio de e-mail ou espera por usuário dentro dela: ela segura locks e uma conexão do pool durante todo esse tempo.
- **Efeitos externos não voltam no rollback.** Se o e-mail foi enviado e depois a transação falhou, o e-mail já foi. Faça o efeito depois do commit, ou use o padrão **outbox** (grava o evento numa tabela na mesma transação e outro processo envia).
- A transação vale para a **conexão**. Queries que usam `db` em vez de `tx` dentro do callback rodam fora dela.

### Isolamento
Isolamento total (tudo como se rodasse em fila) é caro. Os bancos oferecem níveis que trocam garantia por concorrência. Para escolher, é preciso conhecer as **anomalias** que cada um permite.

#### Anomalias
- **Dirty read**: ler um dado que outra transação escreveu e ainda não commitou (e que pode sofrer rollback).
- **Non-repeatable read**: ler a mesma linha duas vezes na mesma transação e receber valores diferentes, porque outra transação commitou no meio.
- **Phantom read**: repetir um `WHERE` e aparecerem (ou sumirem) linhas.
- **Lost update**: duas transações leem `saldo = 100`, cada uma soma 10 e grava 110. Um dos incrementos se perde.
- **Write skew**: duas transações leem o mesmo estado, cada uma altera uma linha *diferente* com base nele, e juntas quebram uma regra. Exemplo clássico: "pelo menos um médico de plantão". Os dois médicos veem que o outro está de plantão, os dois saem ao mesmo tempo e ninguém fica.

#### Níveis
| Nível | Dirty read | Non-repeatable | Phantom | Lost update / Write skew |
|---|---|---|---|---|
| Read Uncommitted | possível* | possível | possível | possível |
| **Read Committed** (padrão Postgres) | ❌ | possível | possível | possível |
| Repeatable Read | ❌ | ❌ | ❌** | lost update ❌**, write skew possível |
| Serializable | ❌ | ❌ | ❌ | ❌ |

\*No Postgres, Read Uncommitted se comporta como Read Committed.
\*\*No Postgres especificamente; o padrão SQL permitiria phantom nesse nível.

```sql
BEGIN ISOLATION LEVEL SERIALIZABLE;
```

#### MVCC
O Postgres não bloqueia leitura por causa de escrita. Cada `UPDATE` cria uma **nova versão** da linha, e cada transação enxerga um **snapshot**. Em Read Committed o snapshot é tirado a cada comando; em Repeatable Read, uma vez no começo da transação. Leitores não bloqueiam escritores e vice-versa. As versões velhas são limpas depois pelo **VACUUM**.

Em Repeatable Read e Serializable, se houver conflito o banco aborta a transação com erro de serialização (`40001`). **A aplicação precisa fazer retry**, senão esses níveis só trocam bug silencioso por erro 500.

#### Resolvendo lost update sem mudar de nível
Na maioria dos casos o padrão Read Committed basta, desde que se escolha um destes caminhos:

```sql
-- 1. Operação atômica: o banco lê e escreve de uma vez
UPDATE contas SET saldo = saldo + 10 WHERE id = 1;

-- 2. Lock pessimista: trava a linha até o fim da transação
SELECT saldo FROM contas WHERE id = 1 FOR UPDATE;

-- 3. Lock otimista: coluna de versão, falha se alguém mudou antes
UPDATE contas SET saldo = 110, versao = versao + 1
WHERE id = 1 AND versao = 3; -- 0 linhas afetadas → conflito, releia e tente de novo
```
O erro clássico é ler na aplicação, calcular e gravar o valor absoluto (`SELECT saldo` → `saldo + 10` em TS → `UPDATE SET saldo = 110`) sem nenhum desses três.

Um cuidado com locks: duas transações que travam as mesmas linhas em **ordem diferente** podem gerar **deadlock**. O banco detecta e aborta uma delas. Travar sempre na mesma ordem (por `id`, por exemplo) evita.

### Plano de execução
SQL é declarativo: você diz **o que** quer, e o **planejador** decide **como** buscar. Ele estima o custo de cada caminho possível com base em **estatísticas** da tabela (quantidade de linhas, distribuição dos valores) e escolhe o mais barato.

```sql
EXPLAIN ANALYZE
SELECT * FROM pedidos WHERE cliente_id = 7 ORDER BY criado_em DESC LIMIT 10;
```
```
Limit  (cost=0.43..12.1 rows=10 width=64) (actual time=0.03..0.05 rows=10 loops=1)
  ->  Index Scan Backward using idx_pedidos_cliente_data on pedidos
        (cost=0.43..540.2 rows=460 width=64) (actual time=0.03..0.05 rows=10 loops=1)
        Index Cond: (cliente_id = 7)
Planning Time: 0.1 ms
Execution Time: 0.07 ms
```
- `EXPLAIN` só mostra o plano estimado. `EXPLAIN ANALYZE` **executa de verdade** e mostra também o real (cuidado com `UPDATE`/`DELETE`: rode dentro de `BEGIN ... ROLLBACK`).
- Lê-se **de dentro pra fora**: o nó mais indentado roda primeiro.
- `cost`: unidade arbitrária do planejador (inicial..total). Só serve para comparar planos entre si.
- `rows` estimado vs `actual rows`: **a primeira coisa a olhar.** Se a estimativa diz 10 e o real é 100 mil, as estatísticas estão erradas e o plano provavelmente é ruim. `ANALYZE pedidos;` atualiza as estatísticas.
- `EXPLAIN (ANALYZE, BUFFERS)` mostra quantas páginas vieram do cache e quantas do disco.

#### Tipos de scan
- **Seq Scan**: lê a tabela toda. Bom para tabela pequena ou quando a query retorna boa parte dela; ruim quando se quer poucas linhas de uma tabela grande.
- **Index Scan**: navega pelo índice e vai à tabela buscar cada linha.
- **Index Only Scan**: responde só com o índice (índice covering).
- **Bitmap Heap Scan**: junta os resultados de um ou mais índices num mapa de páginas e lê a tabela em ordem física. Fica no meio-termo, quando a query retorna muitas linhas para um Index Scan e poucas para um Seq Scan.

#### Tipos de join
- **Nested Loop**: para cada linha de A, busca em B. Ótimo quando A é pequeno e B tem índice na coluna do join. Péssimo se os dois forem grandes.
- **Hash Join**: monta uma hash table com a tabela menor e percorre a maior. Bom para join de igualdade com volumes grandes.
- **Merge Join**: as duas entradas já ordenadas pela chave são percorridas juntas. Bom quando já existe índice nessa ordem.

#### Sinais de problema no plano
- Seq Scan numa tabela grande com filtro que retorna poucas linhas → falta índice (ou a query impede o uso dele; ver "Quando o índice não é usado").
- `Rows Removed by Filter` muito alto → leu muito para aproveitar pouco.
- `Sort` com `external merge Disk` → a ordenação não coube no `work_mem` e foi para o disco. Um índice na ordem certa elimina o sort.
- Nested Loop com `loops=50000` → o N+1 aconteceu dentro do banco.

**N+1 do ORM**: buscar 100 pedidos e depois fazer 1 query por pedido para trazer o cliente gera 101 queries. Cada uma é rápida no `EXPLAIN`, mas o custo está nos 101 round-trips. Resolve-se com join ou `WHERE id IN (...)`. Esse problema só aparece no log de queries, não no plano de uma query isolada.
