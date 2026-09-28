![[Modelagem Draw]]

Modelar domínio é **traduzir o negócio em código**: descobrir quais conceitos existem, quais regras nunca podem ser quebradas, o que as palavras significam para quem vive o negócio, e dar a isso uma forma no código que torne as regras óbvias e os erros difíceis.

É a parte do trabalho que menos pode ser delegada. Uma IA conhece o "e-commerce genérico" da internet, não o seu: ela não sabe que aqui "pedido confirmado" só existe depois do antifraude, que o cliente B2B tem prazo de 28 dias e não 30, ou que o "cancelamento" do financeiro e o do suporte são coisas diferentes. Esse conhecimento está na cabeça das pessoas do negócio, espalhado, contraditório e em boa parte implícito. Extrair e organizar isso é trabalho humano, de conversa. Depois que o modelo está claro, a IA ajuda muito a implementar. Antes, ela só gera um modelo plausível e errado.

### Linguagem ubíqua
A base de tudo em DDD: **uma linguagem compartilhada** entre devs e especialistas do negócio, usada nas conversas, nos documentos e **no código**. Se o negócio fala "matrícula" e o código tem `UserSubscription`, toda conversa exige uma tradução mental, e é nessa tradução que os bugs nascem.

- A linguagem vale **dentro de um contexto** (ver abaixo). Não existe um glossário único para a empresa inteira.
- Palavra ambígua é sinal de dois conceitos escondidos: "conta" de login e "conta" bancária.
- Quando o especialista corrige o seu termo, **o código muda junto**. Renomear é barato; a divergência entre fala e código é cara.
- Mantenha um **glossário** curto no repositório: termo, definição em uma frase, o que **não** é. É o documento que mais economiza reunião.

#### Como extrair o modelo
- Peça **exemplos concretos**, não definições: "me conta o último pedido que deu problema".
- Preste atenção nos **verbos** (viram comandos e eventos) e nas **exceções** ("normalmente é assim, *mas* quando..."). A regra de negócio de verdade mora no "mas".
- Pergunte **"o que acontece se...?"**: se o pagamento cair depois do envio? Se o cliente mudar de plano no meio do mês?
- Procure **conceitos implícitos**: um `if` repetido em vários lugares ou uma regra que todo mundo "sabe" mas que não tem nome geralmente é um conceito que merece virar tipo.
- Desconfie de quando o modelo parece simples demais. Normalmente é porque você ainda não falou com quem lida com as exceções.

### Event Storming
A técnica de descoberta mais usada hoje (Alberto Brandolini, e o Vernon dedica um capítulo a ela). Devs e especialistas numa parede (ou um Miro), com post-its:

1. **Eventos de domínio** (laranja), no passado: `PedidoRealizado`, `PagamentoAprovado`, `PedidoEnviado`. Todo mundo escreve, depois se organiza em linha do tempo.
2. **Comandos** (azul): o que causou o evento, `RealizarPedido`. E **quem** disparou (usuário, sistema, tempo).
3. **Agregados** (amarelo): quem recebe o comando e garante a regra.
4. **Hotspots** (rosa/vermelho): dúvidas, conflitos, "aqui ninguém sabe o que acontece". São o ouro da sessão.

Começar por eventos funciona porque especialistas pensam em **fatos que aconteceram**, não em tabelas nem classes. E os lugares onde a linguagem muda ao longo da parede apontam onde estão as fronteiras entre contextos.

### Design estratégico
A parte mais importante do DDD, e a que se costuma pular indo direto para "entidades e repositórios".

#### Subdomínios
Nem todo pedaço do sistema merece o mesmo investimento:
- **Core**: o que diferencia o negócio e é a razão de ele existir. Aqui vale modelagem cuidadosa, os melhores devs e DDD tático completo.
- **Supporting**: necessário e específico do negócio, mas não diferencial. Modelo mais simples.
- **Generic**: problema resolvido no mercado (auth, pagamento, e-mail, nota fiscal). **Compre ou use pronto**; modelar isso do zero é desperdício.

Para um app de treino, o core provavelmente é a montagem e adaptação dos treinos; o pagamento é generic (Stripe), e o cadastro de academias é supporting.

#### Bounded contexts
A fronteira explícita dentro da qual um modelo e a sua linguagem valem. "Produto" no Catálogo (descrição, fotos, SEO), no Estoque (quantidade, lote, localização) e no Faturamento (NCM, alíquota) são **três modelos**, cada um no seu contexto. O ID é o mesmo; o resto não precisa ser. O porquê de separar assim está em [[Arquitetura e design de software#Bounded contexts (DDD)]].

Idealmente um contexto ↔ um time ↔ um modelo coerente. Um contexto não é automaticamente um microsserviço; num monolito modular, é um módulo.

#### Context mapping
Como os contextos se relacionam. É tanto técnico quanto **político** (quem manda em quem):
- **Partnership**: dois times que evoluem juntos e coordenam as mudanças.
- **Shared Kernel**: um pedaço pequeno de modelo compartilhado. Qualquer mudança exige acordo dos dois lados, então mantenha mínimo.
- **Customer–Supplier**: o upstream fornece e o downstream consome, mas o upstream leva em conta as necessidades do downstream.
- **Conformist**: o downstream aceita o modelo do upstream como vem, sem poder de negociação (a API de um parceiro grande).
- **Anticorruption Layer (ACL)**: o downstream **traduz** o modelo externo para o seu, para não ser contaminado por ele. Essencial com legado e APIs de terceiros; tecnicamente, é um **Adapter/Facade** na fronteira.
- **Open Host Service + Published Language**: o upstream oferece uma API bem definida e documentada (um schema público) para vários consumidores.
- **Separate Ways**: integrar custa mais que duplicar; cada um segue sozinho.
- **Big Ball of Mud**: reconhecer que existe, cercar com ACL e não deixar vazar.

```typescript
// ACL: o resto do sistema nunca vê o formato do gateway
class PagamentoGatewayAcl implements Pagamentos {
	async cobrar(pedido: PedidoId, valor: Dinheiro): Promise<ResultadoCobranca> {
		const r = await gateway.charges.create({ amount_cents: valor.centavos, ref: pedido });
		return r.status === "succeeded" ? { tipo: "aprovado" } : { tipo: "recusado", motivo: r.failure_code };
	}
}
```

### Design tático
Os blocos de construção **dentro** de um contexto. Só compensam no core e em supporting com regra de verdade; em CRUD simples, são cerimônia.

#### Entidade
Tem **identidade** que persiste enquanto os atributos mudam. O cliente muda de nome, e-mail e endereço e continua sendo o mesmo cliente. Igualdade é por ID.

#### Value Object
Definido **só pelos atributos**, sem identidade própria. É imutável e a igualdade é por valor. `Dinheiro`, `Email`, `Cpf`, `Periodo`, `Endereco`.

Esse é o bloco mais subestimado e o que mais rende. Ele tira validação e regra de dentro das entidades e dos services e para com a **primitive obsession** (tudo como `string` e `number`):

```typescript
class Dinheiro {
	private constructor(readonly centavos: number, readonly moeda: "BRL" | "USD") {}

	static de(centavos: number, moeda: "BRL" | "USD" = "BRL") {
		if (!Number.isInteger(centavos)) throw new Error("centavos deve ser inteiro");
		return new Dinheiro(centavos, moeda);
	}

	somar(outro: Dinheiro): Dinheiro {
		if (outro.moeda !== this.moeda) throw new Error("moedas diferentes");
		return new Dinheiro(this.centavos + outro.centavos, this.moeda);
	}
}
```
Dinheiro em centavos inteiros, e não `number` com casas decimais (`0.1 + 0.2 !== 0.3`). Fowler descreve exatamente isso no padrão **Money** do PoEAA.

Em TypeScript, como a tipagem é estrutural (ver [[TypeScript#Tipagem estrutural]]), `type Cpf = string` não impede passar um e-mail no lugar. Para IDs e valores simples, **branded types** resolvem sem precisar de classe:
```typescript
type PedidoId = string & { readonly __brand: "PedidoId" };
type ClienteId = string & { readonly __brand: "ClienteId" };
// buscarPedido(clienteId) agora é erro de compilação
```

#### Agregado
Um grupo de entidades e value objects tratado como **uma unidade de consistência**, com uma **raiz** que é a única porta de entrada. Quem está de fora não mexe nos itens do pedido diretamente, pede ao `Pedido`.

As quatro regras do Vernon:
1. **Proteja as invariantes dentro da fronteira do agregado.** O que precisa ser verdade *sempre, na mesma transação* fica no mesmo agregado ("o total do pedido é a soma dos itens").
2. **Agregados pequenos.** A tentação é pôr tudo junto (`Cliente` com todos os pedidos dentro). O resultado é lento para carregar e gera conflito de concorrência o tempo todo.
3. **Referencie outros agregados só por ID.** `pedido.clienteId`, e não `pedido.cliente`. Isso mantém a fronteira e evita carregar o mundo.
4. **Entre agregados, consistência eventual.** Uma transação altera **um** agregado; os outros reagem a eventos (ver [[Banco de Dados#Transação]] e o outbox).

```typescript
type StatusPedido =
	| { tipo: "rascunho" }
	| { tipo: "confirmado"; em: Date }
	| { tipo: "cancelado"; motivo: string };

class Pedido {
	private itens: Item[] = [];
	private status: StatusPedido = { tipo: "rascunho" };
	private eventos: EventoDominio[] = [];

	constructor(readonly id: PedidoId, readonly clienteId: ClienteId) {}

	adicionarItem(produtoId: ProdutoId, preco: Dinheiro, qtd: number) {
		if (this.status.tipo !== "rascunho") throw new Error("pedido já fechado");
		if (qtd <= 0) throw new Error("quantidade inválida");
		this.itens.push({ produtoId, preco, qtd });
	}

	confirmar(agora: Date) {
		if (this.itens.length === 0) throw new Error("pedido vazio");
		this.status = { tipo: "confirmado", em: agora };
		this.eventos.push({ tipo: "PedidoConfirmado", pedidoId: this.id, total: this.total() });
	}

	total(): Dinheiro {
		return this.itens.reduce((t, i) => t.somar(Dinheiro.de(i.preco.centavos * i.qtd)), Dinheiro.de(0));
	}
}
```
Repare:
- Não existe `setStatus()`. Os métodos têm nome de **operação de negócio** (`confirmar`, não `update`), e a regra fica dentro deles.
- O status é uma discriminated union (ver [[TypeScript#Union e narrowing]]): um pedido cancelado *sem motivo* não é representável. **Faça estados ilegais irrepresentáveis.**
- O agregado **registra eventos** e não os publica. Quem salva o agregado grava os eventos no outbox na mesma transação.

Para concorrência no agregado, o comum é lock otimista com coluna de versão (ver [[Banco de Dados#Resolvendo lost update sem mudar de nível]]). Agregado pequeno é justamente o que torna o conflito raro.

#### Eventos de domínio
Um **fato** que aconteceu e que importa para o negócio, nomeado no passado: `PedidoConfirmado`, `AssinaturaCancelada`. É a forma de comunicar entre agregados e entre contextos sem acoplar: quem confirma o pedido não precisa saber que o estoque reserva, o e-mail sai e a nota fiscal é emitida. Cada um desses reage ao evento.

O evento carrega o que os interessados precisam para reagir, e é um **contrato**: depois de publicado, mudar o formato dele quebra quem consome (ver [[Arquitetura e design de software#Contratos]]).

#### Domain services
Regra que não pertence naturalmente a nenhuma entidade: uma transferência entre duas contas, ou um cálculo de frete que depende de pedido, tabela e transportadora. Função ou classe **sem estado**, com nome da linguagem ubíqua. Não é para virar depósito de toda a lógica; se tudo está no service, o modelo ficou anêmico.

### Patterns of Enterprise Application Architecture (Fowler)
Escrito em 2002, antes dos ORMs modernos, mas é de onde vem o vocabulário que todo framework usa. Serve para **nomear o que o seu código já faz** e escolher conscientemente.

#### Onde mora a lógica de negócio
- **Transaction Script**: uma função por caso de uso, com a lógica toda em sequência (`criarPedido()` valida, calcula, grava). É simples e direto, **o certo para lógica simples**. Degrada quando as regras crescem e começam a se repetir entre scripts.
- **Domain Model**: objetos que juntam dado e comportamento, como o `Pedido` acima. O custo inicial é maior, mas escala com a complexidade das regras.
- **Table Module**: uma classe por tabela, operando sobre um conjunto de linhas. Pouco usado hoje fora do mundo .NET com DataSet.
- **Service Layer**: uma camada fina que define os casos de uso da aplicação, coordena transação, repositório e agregado, **e não contém regra de negócio**.

```typescript
// Service Layer (application service): orquestra, não decide
async function confirmarPedido(id: PedidoId) {
	await db.transaction(async (tx) => {
		const pedido = await pedidos.buscar(tx, id);
		pedido.confirmar(new Date()); // a regra está no domínio
		await pedidos.salvar(tx, pedido); // grava o pedido + eventos no outbox
	});
}
```

A escolha entre Transaction Script e Domain Model é a decisão central do livro, e **depende da complexidade do domínio**, não de preferência. CRUD com validação de formulário → Transaction Script. Regras que se combinam, estados, exceções → Domain Model. Vale registrar em ADR (ver [[Arquitetura e design de software#Registrar o porquê: ADR]]).

**Anemic Domain Model** (termo do próprio Fowler): classes com nome de domínio, só com getters e setters, e toda a lógica em services. Tem todo o custo do Domain Model (mapeamento, camadas) e nenhum benefício. É, na prática, um Transaction Script disfarçado, e é o resultado mais comum de "fazer DDD" sem modelar.

#### Acesso a dados
- **Active Record**: o objeto sabe se salvar (`user.save()`). Simples, ótimo para quando o modelo ≈ a tabela. Acopla o domínio ao banco.
- **Data Mapper**: uma camada separada move os dados entre objetos e banco; o domínio não sabe que o banco existe. É o que permite um Domain Model rico. Mais trabalho.
- **Repository**: uma interface com cara de coleção para buscar e salvar **agregados** (`pedidos.buscar(id)`, `pedidos.salvar(p)`). Um repositório por agregado, **não por tabela**.
- **Unit of Work**: rastreia o que foi alterado durante a operação e grava tudo junto no final. É o que ORMs como TypeORM e MikroORM fazem com o `flush`.
- **Identity Map**: garante que o mesmo registro carregado duas vezes vira o mesmo objeto em memória.
- **Lazy Load**: carrega a relação só quando alguém a acessa. É a origem do N+1 (ver [[Banco de Dados#Sinais de problema no plano]]).

Prisma e Drizzle não se encaixam direitinho em nenhum: devolvem objetos de dados simples, sem comportamento. Para um Domain Model, o repositório faz o mapeamento entre o resultado da query e o agregado.

#### Outros que continuam úteis
- **Money**: o value object acima.
- **Special Case**: um objeto que representa o caso especial (`ClienteAnonimo`, `PlanoGratuito`) em vez de `null` com `if` espalhado. É um primo do Null Object.
- **Optimistic / Pessimistic Offline Lock**: concorrência que atravessa várias requisições (o usuário abre a tela, edita por 10 minutos e salva). Versão no registro, checada no save.
- **Gateway**: um objeto que encapsula o acesso a um sistema externo. É o que a ACL usa por baixo.

### Quando não usar DDD tático
- Domínio simples, CRUD, painel administrativo → Transaction Script e Active Record resolvem, com menos código.
- Subdomínio generic → integre o pronto.
- Protótipo para validar hipótese → modelo raso; se der certo, remodele.

O design **estratégico** (linguagem ubíqua, contextos, subdomínios) vale sempre, mesmo sem nenhum agregado. É barato e é onde está a maior parte do valor.

### Roteiro
1. Conversar com quem vive o negócio, usando exemplos concretos e o "o que acontece se...?".
2. Event Storming para descobrir eventos, comandos e hotspots.
3. Separar os subdomínios em core, supporting e generic, e decidir onde investir.
4. Traçar os bounded contexts e o context map (onde precisa de ACL?).
5. Escrever o glossário da linguagem ubíqua e usar esses nomes no código.
6. No core: agregados pequenos, invariantes dentro deles, value objects para tudo que tem regra, estados ilegais irrepresentáveis.
7. Fora do core: o padrão mais simples que funciona.
8. Voltar ao especialista quando o código "pedir" um conceito que não tem nome. O modelo nunca está pronto; ele evolui com o entendimento.

### Livros
- **Domain-Driven Design Distilled** (Vaughn Vernon): curto e prático. Design estratégico primeiro, depois tático, Event Storming e as regras dos agregados. **Comece por aqui.**
- **Patterns of Enterprise Application Architecture** (Martin Fowler): o catálogo de padrões de lógica de negócio, acesso a dados e concorrência que os frameworks implementam. É de consulta; leia a primeira parte (narrativa) inteira e o catálogo conforme precisar.
- Depois: **Domain-Driven Design** (Eric Evans, "o livro azul"), denso e melhor lido com a base do Distilled; **Implementing DDD** (Vernon), a versão longa; e **Domain Modeling Made Functional** (Scott Wlaschin), sobre modelagem com tipos, que se aplica muito bem a TypeScript.
