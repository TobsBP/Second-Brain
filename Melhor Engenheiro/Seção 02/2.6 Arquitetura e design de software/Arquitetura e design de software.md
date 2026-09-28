![[Arquitetura Draw]]

Arquitetura não é escolher entre monolito e microsserviço nem desenhar caixinhas. É decidir **onde traçar fronteiras**: o que fica junto, o que fica separado e o que cada parte esconde das outras. E, como toda decisão dessas é um trade-off, **registrar por escrito o porquê**, para que daqui a um ano alguém (inclusive eu) entenda a decisão em vez de repetir a discussão ou desfazer algo sem saber o motivo.

Duas leis de Richards & Ford resumem o espírito:
1. **Tudo em arquitetura é trade-off.** Se você achou algo sem trade-off, ainda não achou o trade-off.
2. **O porquê é mais importante que o como.** Dá pra ver *como* um sistema foi feito olhando o código; o *porquê* só existe se alguém escreveu.

### Complexidade: o inimigo (Ousterhout)
*A Philosophy of Software Design* define complexidade como **tudo que torna o sistema difícil de entender e de modificar**. Ela aparece de três formas:
- **Change amplification**: uma mudança simples exige mexer em muitos lugares.
- **Cognitive load**: é preciso saber muita coisa para mexer com segurança.
- **Unknown unknowns**: não é óbvio o que precisa ser mudado, nem o que vai quebrar. É a pior das três.

As causas são duas: **dependências** (um pedaço não pode ser entendido ou mudado isoladamente) e **obscuridade** (a informação importante não está evidente). A complexidade não vem de uma decisão ruim só; ela é **incremental**, mil pequenas concessões do tipo "só dessa vez".

#### Programação tática vs estratégica
- **Tática**: o objetivo é fazer a feature funcionar agora. Cada atalho parece barato; a soma deles é o legado.
- **Estratégica**: código funcionando não basta, o objetivo é um **bom design** que também funciona. Investir ~10–20% do tempo em design gera retorno em poucos meses.

Com IA isso pesa mais: gerar solução tática ficou instantâneo, e a complexidade se acumula na mesma velocidade.

### Módulos profundos
A ideia central do livro. Todo módulo (função, classe, serviço) tem uma **interface** (o custo de usar) e uma **implementação** (o benefício que entrega).

- **Módulo profundo**: interface simples escondendo muita funcionalidade. Exemplo clássico: o I/O de arquivo do Unix, `open/read/write/close/lseek`, cinco chamadas que escondem disco, cache, permissões e sistemas de arquivos.
- **Módulo raso**: a interface é quase tão complexa quanto o que ele faz. Custa mais para entender do que economiza.

```typescript
// raso: não esconde nada, só adiciona uma camada para aprender
class UserRepository {
	findById(id: string) { return db.query("SELECT * FROM users WHERE id = $1", [id]); }
}

// profundo: quem chama não sabe de cache, retry, nem de qual tabela veio
interface Precificacao {
	precoFinal(produtoId: string, clienteId: string): Promise<Dinheiro>;
}
```

Por isso **"classes pequenas" não é objetivo**. Quebrar tudo em pedaços minúsculos gera muitas interfaces rasas, e o custo de entender as conexões supera o ganho. Sinais de módulo raso:
- **Pass-through method**: método que só repassa a chamada para outro com a mesma assinatura.
- **Classite**: uma classe para cada coisinha, cada uma trivial.
- Camadas diferentes com **a mesma abstração**. Se controller, service e repository têm os mesmos métodos com os mesmos parâmetros, as camadas não estão agregando nada.

#### Information hiding e leakage
Cada módulo deve esconder **decisões de design** (formato de dado, algoritmo, qual banco). **Vazamento** acontece quando a mesma decisão aparece em dois módulos: mudar o formato de arquivo exige mudar quem lê *e* quem escreve. Se dois módulos sempre mudam juntos, provavelmente deveriam ser um só.

**Decomposição temporal** é a causa mais comum de vazamento: dividir o código pela *ordem em que as coisas acontecem* (ler → processar → gravar) em vez de pelo *conhecimento* que cada parte encapsula. Leitura e gravação do mesmo formato acabam separadas, e o formato vaza para as duas.

#### Outras ideias que valem ouro
- **Puxe a complexidade para baixo**: é melhor o módulo sofrer internamente do que empurrar a decisão para cada chamador. Configuração exposta muitas vezes é o módulo se recusando a decidir.
- **Defina os erros para fora da existência**: projete a API para que o caso de erro não exista. `substring` fora dos limites pode devolver string vazia em vez de lançar; `delete` de algo que não existe pode ser sucesso (é idempotente, ver [[Rede e HTTP#Métodos]]).
- **Um pouco mais genérico que o necessário**: a interface deve servir aos usos de hoje sem ser amarrada a um só. Pergunta-guia: "qual é a interface mais simples que cobre todas as necessidades atuais?"
- **Design it twice**: antes de implementar, esboce pelo menos duas alternativas bem diferentes e compare. A primeira ideia raramente é a melhor.
- **Comentário descreve o que o código não diz**: o porquê, a intenção, as invariantes, as unidades. Nunca repete o código.

### Fronteiras: acoplamento e coesão
Traçar fronteira é maximizar **coesão** (o que muda junto fica junto) e minimizar **acoplamento** (o que fica separado depende pouco um do outro).

#### Connascence
Richards & Ford usam connascence para medir *como* duas partes estão acopladas: se mudar A exige mudar B, elas são conascentes. Da mais fraca para a mais forte:
- **Estática** (visível no código): *nome* → *tipo* → *significado* (o `3` que quer dizer "cancelado") → *posição* (a ordem dos parâmetros) → *algoritmo* (os dois lados precisam usar o mesmo hash).
- **Dinâmica** (só em runtime): *execução* (a ordem das chamadas importa) → *tempo* → *valores* (várias coisas precisam mudar juntas) → *identidade* (precisam apontar pro mesmo objeto).

Regras:
- Converta as formas fortes em fracas: troque um número mágico por um enum nomeado, e parâmetros posicionais por um objeto.
- Quanto **mais forte** a connascence, mais **perto** as duas partes devem estar: dentro da mesma função tudo bem, entre serviços é um desastre.

#### Bounded contexts (DDD)
A fronteira mais estável é a do **domínio**, não a técnica. "Cliente" para Vendas (limite de crédito, histórico) e para Suporte (tickets, SLA) são modelos diferentes. Forçar um `Cliente` único com tudo é acoplamento disfarçado de reuso. Cada contexto tem seu modelo e sua linguagem, e eles conversam por contratos na fronteira.

Por isso organizar o código por **domínio** (`pedidos/`, `pagamentos/`) costuma envelhecer melhor que por **camada técnica** (`controllers/`, `services/`, `repositories/`). Uma mudança de negócio normalmente toca um domínio só, mas atravessa todas as camadas.

#### Ports & adapters na borda
Um jeito concreto de manter a fronteira limpa: o núcleo define **interfaces** do que precisa (`Pagamentos`, `Notificador`), e a infraestrutura as implementa (**Adapter** para Stripe, para SMTP). O domínio não importa nada de framework, banco ou SDK externo. É o mesmo seam que deixa o código testável (ver [[Testes, refatoração e código legado#Seams]]).

Só vale onde a fronteira é real: dependência externa, algo que muda ou que precisa de fake no teste. Criar interface com uma única implementação para tudo "por via das dúvidas" é o módulo raso do Ousterhout.

### Características arquiteturais (Richards & Ford)
*Fundamentals of Software Architecture* separa o que o sistema **faz** (domínio) das **características** que ele precisa ter, as "-ilities": escalabilidade, disponibilidade, performance, segurança, testabilidade, deployabilidade, elasticidade, tolerância a falha...

- Não dá pra maximizar todas; elas brigam entre si (segurança × performance, consistência × disponibilidade).
- Escolha as **3 mais importantes**, derivadas de requisito real ("Black Friday multiplica o tráfego por 20" → elasticidade), e não de desejo ("tem que escalar").
- **A arquitetura menos pior** é o objetivo. A "melhor" não existe.

#### Estilos
| Estilo | Bom para | Custa |
|---|---|---|
| Camadas | Começar rápido, time pequeno | Mudança de negócio atravessa todas as camadas |
| **Monolito modular** | Default sensato: fronteiras por domínio, um deploy | Exige disciplina para as fronteiras não vazarem |
| Microkernel (plugins) | Produto com núcleo fixo e extensões | Contrato do plugin difícil de evoluir |
| Event-driven | Alto desacoplamento, picos, processamento assíncrono | Fluxo difícil de seguir, consistência eventual |
| Microsserviços | Times independentes, escalar e fazer deploy separadamente | Rede, dados distribuídos, observabilidade: complexidade operacional enorme |

Microsserviço resolve problema **organizacional** (times que se atrapalham) mais que técnico. Com um time só, quase sempre é custo sem benefício. Monolito modular primeiro: se as fronteiras estiverem boas, extrair um serviço depois é viável; se estiverem ruins, o resultado é um **monolito distribuído**, que tem o pior dos dois mundos.

#### Arquitetura quantum
A unidade que pode ser **implantada de forma independente**, com alta coesão funcional e as mesmas características arquiteturais. Cinco serviços que compartilham um banco e precisam de deploy juntos são **um** quantum, não cinco. O banco compartilhado é acoplamento estático, e por isso o número de "serviços" engana.

#### Fitness functions
A arquitetura decidida no papel se degrada no código. **Fitness functions** são testes automáticos que protegem uma característica ou uma regra de fronteira:

```js
// .dependency-cruiser.js — o domínio não pode importar infraestrutura
module.exports = {
	forbidden: [{
		name: "dominio-nao-conhece-infra",
		from: { path: "^src/.+/domain" },
		to: { path: "^src/.+/infra" },
	}],
};
```
Rodando no CI, uma regra de fronteira deixa de ser combinado verbal e vira algo que quebra o build. Outras fitness functions: orçamento de latência em teste de carga, ausência de dependência circular, tamanho máximo de bundle.

### Trade-offs difíceis (The Hard Parts)
*Software Architecture: The Hard Parts* trata das decisões em que não existe resposta certa, só a **menos pior para o contexto**. O método é sempre o mesmo: listar as forças em jogo, comparar as opções contra elas e escrever o resultado.

#### Granularidade: separar ou juntar?
**Desintegradores** (motivos para separar):
- Escopo e função: a coisa faz duas coisas sem relação.
- **Volatilidade**: uma parte muda toda semana e a outra nunca.
- Escalabilidade e throughput diferentes.
- Tolerância a falha: a queda de uma não pode derrubar a outra.
- Segurança: dado sensível que precisa de acesso mais restrito.
- Extensibilidade: vão surgir novos tipos da mesma coisa.

**Integradores** (motivos para juntar):
- **Transação de banco**: se precisa de atomicidade entre as duas, separar custa caro (ver [[Banco de Dados#Transação]]).
- Workflow: as duas conversam o tempo todo, e cada chamada vira latência e ponto de falha.
- Código compartilhado que muda junto.
- Relacionamento de dados forte (FKs, joins frequentes).

Separe quando os desintegradores pesam mais, e **escreva quais pesaram**.

#### Dados são a parte mais difícil
Separar o código é fácil; separar o **banco** é onde dói. Cada serviço deveria ser dono dos próprios dados, e isso tira de cena o que o banco dava de graça: join, FK, transação ACID.

Quando uma operação atravessa serviços, não existe `BEGIN/COMMIT`. A alternativa é uma **saga**: uma sequência de transações locais, cada uma com uma **compensação** para desfazer (cancelar o pedido se o pagamento falhar). O livro classifica as sagas em três dimensões, cada uma um trade-off:
- **Comunicação**: síncrona × assíncrona.
- **Consistência**: atômica × eventual.
- **Coordenação**: orquestrada (um coordenador central manda) × coreografada (cada serviço reage a eventos).

Orquestrada é mais fácil de entender e depurar; coreografada é mais desacoplada, mas o fluxo fica espalhado. Para publicar eventos de forma confiável depois de gravar, use o **outbox** (ver [[Banco de Dados#Na aplicação]]).

#### Reuso: compartilhar ou duplicar?
- **Biblioteca compartilhada**: uma versão para todos. Mudar exige coordenar todo mundo.
- **Serviço compartilhado**: atualiza num lugar só, mas vira dependência de runtime e ponto único de falha.
- **Duplicação**: cada um com a sua cópia. Parece errado, mas é o certo quando os contextos são diferentes e vão evoluir separadamente.

DRY vale **dentro** de uma fronteira. Entre fronteiras, duplicar costuma ser mais barato que acoplar. Duas coisas que são iguais *hoje por coincidência* não são duplicação.

#### Contratos
- **Estrito** (schema fechado, tipos exatos): pega erro cedo, mas qualquer mudança quebra quem consome.
- **Flexível** (JSON aceitando campo extra, nome-valor): evolui fácil, mas o erro aparece tarde.

Prefira contratos que carregam só o necessário. Mandar a entidade inteira "pra facilitar" acopla o consumidor a campos que ele nem usa.

### Registrar o porquê: ADR
**Architecture Decision Record** (Michael Nygard): um arquivo markdown curto por decisão, versionado junto com o código (`docs/adr/0007-monolito-modular.md`). Ele não é burocracia; é a resposta pronta para "por que diabos fizeram assim?".

```markdown
# 7. Monolito modular em vez de microsserviços

**Status:** Aceito (2026-09-27)

## Contexto
Time de 4 devs. Domínios: pedidos, pagamentos, catálogo.
Pico previsível só no catálogo (leitura). Deploy hoje é semanal.

## Decisão
Um deploy, um banco, módulos por domínio em src/<dominio>.
Um módulo só acessa outro pela API pública (index.ts);
regra verificada por dependency-cruiser no CI.

## Consequências
+ Transação ACID entre pedidos e pagamentos, sem saga.
+ Operação simples: um serviço para monitorar.
- Não dá para escalar o catálogo separadamente; mitigado com cache/CDN.
- Se o time passar de ~3 squads, reavaliar a extração do catálogo.
```

Regras:
- **ADR não se edita, se substitui.** Mudou de ideia? Novo ADR com status "Substitui o 7", e o 7 vira "Substituído pelo 12". O histórico é o valor.
- **Consequências negativas são obrigatórias.** Se só tem ponto positivo, a análise de trade-off não foi feita.
- Inclua **as alternativas descartadas e por quê**. É isso que impede a mesma discussão de voltar daqui a seis meses.
- Registre também o **gatilho de reavaliação** ("se passar de X, rever"). Decisão boa hoje pode ser ruim com outro contexto.

### Roteiro para decidir uma fronteira
1. **Qual é o conhecimento** que essa parte encapsula? (Não "qual etapa do fluxo".)
2. O que **muda junto**? Junte. O que muda por motivos e em ritmos diferentes? Candidato a separar.
3. A interface resultante é **profunda**, esconde bastante coisa atrás de pouco?
4. Quais **desintegradores e integradores** estão em jogo? Precisa de transação atravessando a fronteira?
5. Esbocei **pelo menos duas** alternativas?
6. Consigo **verificar** a fronteira automaticamente (fitness function)?
7. **Escrevi o ADR**, com contexto, alternativas e consequências negativas?

### Livros
- **A Philosophy of Software Design** (John Ousterhout): curto e direto, o melhor para o design do dia a dia (módulos, interfaces, complexidade). Ler primeiro.
- **Fundamentals of Software Architecture** (Mark Richards e Neal Ford): vocabulário e visão geral: características, estilos, connascence, ADR, o papel do arquiteto.
- **Software Architecture: The Hard Parts** (Neal Ford, Mark Richards, Pramod Sadalage e Zhamak Dehghani): o que fazer quando separar tem custo, incluindo granularidade, dados distribuídos, sagas, reuso e contratos. Ler depois do Fundamentals.
