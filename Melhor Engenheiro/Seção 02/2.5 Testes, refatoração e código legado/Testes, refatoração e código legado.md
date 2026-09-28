![[Testes Draw]]

O objetivo não é "ter testes". É conseguir **mudar o código com confiança**: alterar, rodar, e saber em segundos se algo quebrou. Sem isso, cada mudança vira aposta e o código apodrece, porque ninguém tem coragem de mexer.

Com IA gerando código em volume, o gargalo mudou de lugar. Escrever ficou barato; **verificar** continua caro. Se a IA gera 500 linhas em um minuto e a única forma de saber se estão certas é ler tudo e testar na mão, a velocidade é ilusória. Quem tem uma boa rede de segurança consegue aceitar, rejeitar e refatorar código gerado rápido. Quem não tem acumula código que ninguém entende e ninguém tem coragem de tocar, ou seja, legado instantâneo.

### O que é um bom teste
Um teste bom tem quatro propriedades (Khorikov, *Unit Testing*):
- **Pega regressão**: quebra quando o comportamento quebra.
- **Resiste a refatoração**: *não* quebra quando só a estrutura interna muda. É a mais negligenciada e a mais importante.
- **Rápido**: se demora, ninguém roda.
- **Fácil de manter**: dá pra ler e entender o que falhou.

A regra que garante a segunda: **teste comportamento, não implementação.** Teste o que entra e o que sai pela interface pública, não quais métodos privados foram chamados.

```typescript
// ❌ acoplado à implementação: quebra se trocar o cálculo por dentro
it("chama calcularDesconto", () => {
	const spy = vi.spyOn(carrinho, "calcularDesconto");
	carrinho.total();
	expect(spy).toHaveBeenCalled();
});

// ✅ testa o comportamento: sobrevive a qualquer refatoração
it("aplica 10% em compras acima de 100", () => {
	const carrinho = new Carrinho([{ preco: 200, qtd: 1 }]);
	expect(carrinho.total()).toBe(180);
});
```

Estrutura **Arrange / Act / Assert**: preparar, executar uma ação, verificar. O nome do teste descreve a regra de negócio, não o método.

#### Determinismo
Teste **flaky** (passa às vezes) é pior que nenhum: ensina o time a ignorar teste vermelho. As causas quase sempre são tempo (`Date.now()`, `setTimeout`), aleatoriedade, ordem entre testes, estado compartilhado e rede. A solução é injetar relógio e gerador de números, isolar o estado e usar `vi.useFakeTimers()`.

### Que tipo de teste escrever
- **Unitário**: uma unidade de comportamento isolada. Rápido, aponta onde está o erro. Ideal para regra de negócio pura (cálculo, validação, máquina de estados).
- **Integração**: a aplicação falando com coisas reais (banco, fila, HTTP). Pega o que o unitário não vê: SQL errado, migration quebrada, serialização.
- **E2E**: o sistema inteiro pela interface do usuário. Dá a maior confiança, mas é lento e frágil. Poucos, só nos fluxos críticos (login, checkout).

A **pirâmide** (muito unitário, pouco E2E) é o ponto de partida clássico. Em backend de API, o **troféu** costuma render mais: o grosso na integração, testando o endpoint de verdade contra um banco real (Testcontainers, ou Postgres no Docker do CI). Um mock do banco não pega um `WHERE` errado.

#### Mocks
Mocke o que você **não controla e está na fronteira**: API externa, gateway de pagamento, envio de e-mail, relógio. **Não** mocke o que é seu e está dentro: mockar os próprios repositórios e serviços deixa o teste testando o mock, e ele quebra a cada refatoração (viola a propriedade 2).

Sinal de alerta: se o teste precisa de 8 mocks para rodar, o problema é o design do código, não o teste.

#### Cobertura não é qualidade
Cobertura mostra que a linha **rodou**, não que foi **verificada**. Um teste sem `expect` dá 100% de cobertura. Ela serve para achar o que **não** está testado, não como meta. Quando vira meta, o time escreve teste para bater número.

**Mutation testing** mede a rede de verdade: a ferramenta (Stryker, no ecossistema JS/TS) altera o código de propósito (`>` vira `>=`, `+` vira `-`, remove um `if`) e roda os testes. Se nenhum teste falhar, aquele "mutante sobreviveu" e há um buraco na rede. É a melhor forma de avaliar testes gerados por IA.

#### Property-based testing
Em vez de escolher exemplos na mão, você declara uma **propriedade** que vale para qualquer entrada, e a lib gera centenas de casos, inclusive os estranhos (string vazia, número negativo, unicode).

```typescript
import fc from "fast-check";

it("ordenar é idempotente e preserva o tamanho", () => {
	fc.assert(fc.property(fc.array(fc.integer()), (arr) => {
		const uma = ordenar(arr);
		expect(ordenar(uma)).toEqual(uma);
		expect(uma).toHaveLength(arr.length);
	}));
});
```
Bom para parser, serialização (`parse(serialize(x)) === x`) e cálculo.

### TDD
Ciclo **Red → Green → Refactor**: escreve um teste que falha, escreve o mínimo para passar, melhora a estrutura com o teste verde protegendo.

O ganho maior não é o teste em si, é o **design**: escrever o teste antes obriga a pensar na interface pelo lado de quem usa, e código difícil de testar aparece na hora.

Com IA isso ganha outra função: **o teste vira a especificação.** Você escreve (ou revisa com cuidado) os testes que descrevem o comportamento, e a IA implementa até ficar verde. É muito mais fácil verificar 10 testes curtos que expressam a regra do que 300 linhas de implementação.

### Refatoração
Refatorar é **mudar a estrutura sem mudar o comportamento** (Fowler). Não é "reescrever", nem "melhorar e aproveitar pra corrigir aquele bug".

Regras:
- **Só com teste verde.** Sem rede, não é refatoração, é reescrita torcendo pra dar certo.
- **Passos pequenos**, rodando os testes a cada um. Se quebrou, o erro está no último passo: desfaz e tenta de novo.
- **Não misture** refatoração e mudança de comportamento no mesmo commit. Commit de refatoração tem que poder ser revisado sabendo que "nada mudou".
- *"Make the change easy, then make the easy change"* (Kent Beck). Antes de uma feature difícil, refatore até ela ficar fácil, e só então implemente.

Refatorações do dia a dia: extrair função, renomear, extrair variável explicativa, trocar condicional aninhado por guard clause, mover função para o módulo em que ela é usada, trocar parâmetro booleano por duas funções. A IDE faz várias com segurança (rename, extract); prefira a ferramenta à edição manual.

**Code smells** que pedem refatoração: função longa, parâmetros demais, duplicação, código que muda sempre junto em lugares diferentes (*shotgun surgery*), classe que sabe de tudo, comentário explicando código confuso (em vez de o código ser claro).

### Código legado
Definição de Michael Feathers (*Working Effectively with Legacy Code*): **código legado é código sem testes.** Não importa a idade. Código gerado ontem por IA sem teste é legado.

O dilema: para mudar com segurança preciso de testes; para colocar testes preciso mudar o código (desacoplar dependências). A saída é um roteiro:

1. Identificar o ponto de mudança.
2. Achar onde dá para testar.
3. Quebrar dependências **com o mínimo de mudança possível**.
4. Escrever os testes.
5. Fazer a mudança e refatorar.

#### Characterization tests
Em legado, você não sabe qual é o comportamento *correto*, só qual é o *atual*. O **teste de caracterização** registra o que o código faz hoje, certo ou errado:

```typescript
it("caracteriza calcularFrete", () => {
	// não sei se 47.3 está certo; sei que é o que o sistema faz hoje
	expect(calcularFrete({ peso: 12, cep: "37540000" })).toBe(47.3);
});
```
Se descobrir um bug no caminho, anote e corrija **depois**, em outro commit. Primeiro trave o comportamento.

Uma variação é o **golden master / approval testing**: roda o sistema com muitas entradas, salva todas as saídas num arquivo e passa a comparar contra ele. Serve para código grande demais para entender antes de mexer.

#### Seams
**Seam** (costura) é um ponto onde dá pra trocar um comportamento sem editar o código ali. O mais comum é injeção de dependência:

```typescript
// ❌ impossível testar sem mandar e-mail de verdade
class Cadastro {
	registrar(u: User) {
		// ...
		new SmtpMailer().enviar(u.email, "Bem-vindo");
	}
}

// ✅ a dependência entra pelo construtor: no teste, passa um fake
class Cadastro {
	constructor(private mailer: Mailer = new SmtpMailer()) {}
	registrar(u: User) {
		// ...
		this.mailer.enviar(u.email, "Bem-vindo");
	}
}
```
O valor padrão no construtor mantém os chamadores antigos funcionando sem mudança.

#### Técnicas para mudar sem tocar no que está sem teste
- **Sprout method/class**: a funcionalidade nova vai numa função nova, testada, e o código velho só a chama. O novo nasce com teste mesmo que o velho não tenha.
- **Wrap method**: renomeia o método antigo e cria um novo com o nome original, que chama o antigo e acrescenta o comportamento novo antes ou depois. Na prática é o padrão **Decorator** aplicado a um método.
- **Strangler Fig**: para substituir um sistema inteiro, coloca-se uma fachada na frente e as rotas migram uma a uma para o código novo, até o velho poder ser desligado. Big-bang rewrite quase sempre falha, porque o sistema velho tem anos de regras que ninguém documentou.
- **Adapter** na borda: isola a API feia do legado atrás de uma interface limpa, para o código novo não depender dela diretamente.

### A rede de segurança com IA
Código gerado por IA erra de jeitos próprios, e a rede precisa pegar esses erros:

- **Teste tautológico**: a IA escreve o código e depois o teste que confirma o que o código faz, bug incluso. O teste passa e não prova nada. Por isso quem define o comportamento esperado é você, e o teste se revisa **antes** da implementação.
- **Mock de tudo**: a IA tende a mockar qualquer dependência para o teste passar rápido, e o resultado testa só o mock.
- **Enfraquecer o teste para passar**: diante de um teste vermelho, trocar o `expect`, adicionar `.skip` ou apagar o caso. Na revisão, **diff em arquivo de teste merece mais atenção que diff em código**.
- **Diff grande demais para revisar**: peça mudanças pequenas e separadas (primeiro refatoração, depois comportamento), no mesmo espírito do commit de refatoração.
- **Plausível, mas errado nas bordas**: o caso feliz funciona; null, lista vazia, fuso horário, concorrência e erro de rede não. É aí que property-based testing e testes de integração com dependência real compensam.

Na prática:
1. Escreva ou revise os testes primeiro; eles são a especificação.
2. Deixe a IA implementar até ficar verde.
3. Rode mutation testing nos módulos críticos para ver se a rede tem buraco.
4. CI bloqueando merge com teste vermelho, lint e checagem de tipos (`tsc --noEmit`). O que não é automático não acontece.
5. Refatore o código gerado com a mesma disciplina do seu: teste verde, passos pequenos.

A habilidade central deixou de ser escrever código e passou a ser **saber dizer, rápido e com evidência, se o código está certo**. A rede de segurança é essa evidência.
