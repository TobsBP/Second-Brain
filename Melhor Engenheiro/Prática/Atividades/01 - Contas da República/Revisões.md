## Revisão 1 — 01/10/2026

### SRS

**✅ O que está bom**
- Separou funcionais de não funcionais.
- Pegou o núcleo do briefing: lançar despesa dividida, marcar pagamento, link no celular, custo zero.

**❌ O que está errado**
- **Requisitos que não dá para testar.** "Fácil acesso" e "Rápido": como você provaria que o sistema cumpre isso? Um requisito bom tem critério de aceite. *Pista: rápido quanto, em que rede, medindo o quê?*
- **Falta o ator.** "Definir quem já pagou": quem define? A Ana confirma que recebeu ou o Marcos avisa que pagou? A frustração da Ana no briefing depende exatamente dessa resposta.
- **Requisitos que faltam.** Releia o briefing procurando:
  - a despesa tem **quem pagou** e **entre quem foi dividida**, que são coisas diferentes (a pizza de sexta);
  - morador **entra e sai** a cada semestre, e a dívida de quem saiu não pode sumir.
- **Item no lugar errado.** "Celular navegador" não é uma funcionalidade. É uma restrição de plataforma, ou seja, um requisito não funcional.
- **"Sem login" já é uma solução.** O briefing diz que o Marcos não vai criar conta *com senha*, mas não diz que ninguém se identifica. Se ninguém se identifica, como a Ana prova que o Marcos "não pagou"? Essa tensão é uma ADR, não um requisito. Deixe o requisito descrever a **necessidade** (ex.: acesso sem cadastro com senha) e a ADR decidir o **como**.
- **Faltou separar MVP de depois**, que era uma das entregas.

**💡 O que pode melhorar**
- Numere os requisitos (RF01, RF02, RNF01…). As ADRs e, mais tarde, os testes vão citar esses números.
- Escreva no formato *"O sistema deve permitir que [ator] [ação]"*. Isso força você a pensar no ator.
- O contexto repete o problema, mas esquece as **restrições** (orçamento zero, moradores que mudam, só uma república).
- Ortografia: "Requisito funcionais" → "Requisitos funcionais"; "quem deve quem" → "quem deve a quem".

### ADR 001 — Escolha das Tecnologias

**✅ O que está bom**
- Tem a estrutura completa: contexto, decisão, justificativa, alternativa e consequências.
- Considerou uma alternativa de verdade. Muita gente pula essa parte.
- O argumento "os dados são relacionados (quem deve a quem, quem comprou o quê)" é bom e é específico deste problema.

**❌ O que está errado**
- **Três decisões numa ADR só.** Postgres, Fastify e Next.js estão juntos, mas só o Postgres foi justificado. Fastify e Next não têm nenhum porquê nem alternativa. *Uma ADR = uma decisão.*
- **O contexto não leva à decisão.** É uma cópia do contexto do SRS e não tem nenhuma força que aponte para "banco relacional". Leia só o contexto e se pergunte: dá para deduzir dele que precisa de Postgres? Hoje não dá. Isso é sinal de que a decisão veio antes e a justificativa foi escrita depois.
- **Pulou a decisão anterior.** Antes de "qual banco", vem "**onde** os dados ficam". Marcos precisa ver no celular dele o que a Ana lançou no dela. Quais são as opções para isso acontecer? (Banco num servidor é uma delas, mas não a única.) Essa deveria ser a ADR 001.
- **"Gratuito via Docker" não fecha a conta.** O Docker roda no seu notebook. Quando o Marcos abrir o link às 23h, onde esse banco está rodando? Quem mantém esse servidor ligado, e quanto custa? O orçamento zero é sobre **hospedagem**, não sobre licença. O mesmo vale para o "assim como o front".
- **Informação errada sobre o MongoDB.** O MongoDB tem transações ACID com múltiplos documentos desde a versão 4.0 (2018). Descartar uma alternativa por um motivo falso enfraquece a ADR inteira. Procure um motivo verdadeiro: você já tem um ("os dados são relacionados").
- **"Escala futura" não é uma força aqui.** São 5 moradores numa república, e várias repúblicas estão fora do escopo. Além disso, a justificativa diz que o sistema "escala fácil", mas as consequências dizem que tem "menos flexibilidade de escalabilidade horizontal". A ADR se contradiz.

**💡 O que pode melhorar**
- **Pergunte o que cada peça faz que as outras não fazem.** Next.js e Fastify são dois serviços para hospedar e manter. O que o Fastify faz aqui que o Next sozinho não faz? Se a resposta for "nada ainda", isso é uma consequência a registrar, ou um sinal de que ele não deveria estar ali.
- **As consequências parecem genéricas**, como se servissem para qualquer projeto ("a equipe precisará se familiarizar…", mas a equipe é você). Escreva o que muda **neste** projeto: o que fica mais fácil, o que passa a ser trabalho seu (servidor, migrations, backup).
- **Use o modelo do [[Roteiro]].** Os títulos com `##` facilitam a leitura, e faltou o campo **Status**.
- **Dê nome ao arquivo.** Em vez de `ADR 001.md`, use `0001 - <Decisão>.md`. Quem abrir a pasta `ADRs/` deve entender cada decisão só pelo nome.
- **Ortografia:** "Postgrees" → PostgreSQL; "Fastfy" → Fastify; "NextJs" → Next.js; "oque" → "o que".

### Para a tentativa 2

1. Refazer o SRS: requisitos numerados, cada um com ator, os que não são testáveis viram mensuráveis, os que faltam do briefing incluídos, e MVP separado de depois.
2. Listar as decisões que o briefing força **antes** de escolher tecnologia. Dica: são pelo menos três tensões (onde ficam os dados, como saber quem é quem sem senha, quem confirma um pagamento).
3. Reescrever a ADR 001 sobre a primeira delas, com o contexto trazendo as forças (orçamento zero, dois celulares, Marcos não instala nada).
4. Só depois: ADRs de banco, back-end e front, uma por arquivo.

## Revisão 2 — 01/10/2026

### Progresso desde a revisão 1

| Ponto da revisão 1 | Status |
|---|---|
| "Rápido" testável | ✅ virou "< 3 segundos" |
| "Celular navegador" como não funcional | ✅ |
| "Sem login" saiu dos requisitos e virou ADR | ✅ |
| Uma decisão por ADR | ✅ |
| Consequências genéricas e erro sobre MongoDB | ✅ sumiram, o texto agora é seu |
| Divisão entre pessoas específicas (pizza) | 🟡 apareceu, mas falta dizer quem pagou |
| Histórico de dívida | 🟡 melhorou, mas falta a saída de morador |
| "Fácil acesso" testável | ❌ igual |
| Ator em cada requisito | ❌ |
| Numeração (RF01…) | ❌ |
| Separar MVP de depois | ❌ |
| Restrições no contexto | ❌ |
| Modelo do Roteiro e nome do arquivo da ADR | ❌ |

### SRS

**✅ O que está bom**
- "Respostas em menos de 3 segundos" é um requisito que dá para testar. Foi exatamente o que faltava.
- Cadastro de pessoas só com o nome é uma boa ideia, porque resolve o atrito do Marcos sem decidir *como* no requisito.

**❌ O que está errado**
- **O primeiro requisito tem duas coisas dentro.** "Definir quem já pagou" e "cadastrar pessoas pelo nome" são requisitos diferentes. Se um passar no teste e o outro falhar, o requisito passou ou falhou?
- **"Ela" quem?** Em "ela pode cadastrar as pessoas", não dá para saber quem é "ela". É a falta de ator de novo.
- **Quem pagou a despesa.** "Pessoa x dividiu com pessoa y" não diz quem pôs o dinheiro. Exemplo: Ana pagou a pizza de R$ 60, dividida entre Ana, Marcos e Léo. Quanto o Marcos deve, e para quem? Seu requisito precisa permitir responder isso.
- **Morador que sai.** O requisito do histórico cobre a dívida, mas não cobre a pessoa. Dá para remover alguém que ainda deve? Ele continua aparecendo na divisão das próximas contas?
- **"Link da página com contas que deve".** É um link para a república toda ou um por pessoa? Quem abre o link só vê ou também pode editar? A resposta muda a ADR 001.

**💡 O que pode melhorar**
- "Fácil acesso" pode virar algo como "a partir do link, o morador vê quanto deve em no máximo N toques". O número é decisão sua.
- "Ser gratuito" fica mais forte assim: "custo de operação de R$ 0/mês".
- Ortografia: "Requisito funcionais" → "Requisitos funcionais"; "quem deve quem" → "quem deve a quem"; "especificas" → "específicas"; "pagina" → "página".

### ADR 001 — Sem login

**✅ O que está bom**
- **Uma decisão só**, e escrita com suas palavras. Ficou bem mais honesta que a primeira versão.
- **A justificativa tem a força certa.** "Manter os users no sistema" é o ponto central: se o Marcos não usar, o sistema volta a ser o grupo do WhatsApp. Isso é adoção, e é um motivo real.

**❌ O que está errado**
- **O contexto só tem um lado.** Ele mostra a preguiça do Marcos, mas esquece a Ana: ela quer acabar com o "já te paguei" sem prova. Uma ADR existe porque duas forças puxam para lados opostos. Com uma força só, a decisão parece óbvia, e não é.
- **A decisão não diz quem pode editar.** Se não tem login e o link mostra os registros, quem abre o link pode marcar um pagamento? Se pode, o Marcos marca ele mesmo como pago. Se não pode, como o Marcos avisa que pagou? A decisão precisa responder isso.
- **Não tem alternativas.** Entre "login com senha" e "nenhuma identificação" existe meio-termo? *Pista: pense em apps que você usa e que sabem quem você é sem pedir senha toda vez.* Liste pelo menos duas opções e diga por que não escolheu cada uma.
- **As consequências estão vagas.** "Pode dificultar o sistema" como? "Abrir mão de segurança" quer dizer o quê, concretamente? Pense em cenários: o que acontece se alguém encaminhar o link no grupo da sala? E se dois moradores tiverem o mesmo nome?

**💡 O que pode melhorar**
- **Essa decisão depende de outra.** Para "mandar o link com esses registros", os dados precisam estar num lugar que o celular do Marcos consiga acessar. Essa é a ADR de "onde ficam os dados", que ainda falta. Pode ser a ADR 002, desde que exista.
- **Ainda falta seguir o modelo:** usar os títulos `##` do [[Roteiro]], incluir o campo **Status** e renomear o arquivo para algo como `0001 - Acesso sem login.md`.
- "users" → "usuários" ou "moradores". Use o vocabulário do domínio. Aqui são moradores.

### Para a tentativa 3

1. SRS: numerar, colocar um ator em cada requisito, separar o primeiro requisito em dois, incluir "quem pagou a despesa" e "saída de morador", e separar MVP de depois.
2. ADR 001: trazer a Ana para o contexto, decidir quem pode editar, listar pelo menos duas alternativas e escrever consequências concretas.
3. Criar a ADR 002: onde os dados ficam.

## Revisão 3 — 01/10/2026

### Progresso desde a revisão 2

| Ponto | Status |
|---|---|
| Ator em cada requisito | 🟡 tem ator, mas com 4 nomes diferentes |
| Separar "quem pagou" de "cadastrar pessoas" | ✅ |
| Quem pagou a despesa | ✅ "pessoa x pagou tanto, falta tanto de y" |
| Saída de morador | ✅ "não é possível remover quem ainda deve" |
| Quem pode editar | ✅ só quem cobra e o admin |
| Ana no contexto da ADR, como necessidade de prova | 🟡 a prova apareceu na decisão, mas não no contexto |
| Separar MVP de depois | 🟡 a seção existe, mas o MVP é "tudo" |
| "Fácil acesso" testável | ❌ terceira vez |
| Numeração (RF01…) | ❌ terceira vez |
| Restrições no contexto do SRS | ❌ terceira vez |
| Alternativas na ADR | ❌ segunda vez |
| Modelo do Roteiro, Status e nome do arquivo | ❌ terceira vez |
| ADR 002, sobre onde os dados ficam | ❌ não foi criada |

> [!warning] Itens que se repetem
> Quatro itens aparecem pela terceira vez. Se você **discorda** de algum (por exemplo, acha a numeração inútil), escreva isso. Discordar com argumento é uma resposta válida. Se só passou batido, faça um checklist com os itens antes de me mandar a próxima versão.

### SRS

**✅ O que está bom**
- **O comprovante como prova** é uma solução criativa para o "já te paguei" da Ana, e veio de você, não do briefing.
- **"Não é possível remover pessoas que ainda devem"** é uma regra de negócio clara e dá para testar. Requisito bom é assim.
- **Os papéis admin e morador** mostram que você começou a pensar em quem pode fazer o quê.

**❌ O que está errado**
- **Persona não é ator.** Ana e Marcos são exemplos de pessoas. Os atores do sistema são **papéis**: admin, morador, quem cobra. Hoje o SRS usa quatro nomes diferentes ("Ana", "Marcos", "qualquer pessoa", "dono da república"), e o "dono da república" nem existe no briefing. Defina a lista de papéis uma vez e use só ela.
- **Duas formas de dar baixa num pagamento.** O RF01 diz "Ana define quem já pagou colocando o nome". O requisito do link diz que o morador "anexa o comprovante" (e a ADR completa: "para dar baixa"). Quem dá baixa, afinal? Se for o morador anexando, o que impede o Marcos de anexar uma foto qualquer? Pense em quem **envia** a prova e quem **confirma** que recebeu.
- **"Nota fiscal" é o termo errado.** Nota fiscal é o documento de uma compra. O que o Marcos manda para a Ana é um **comprovante** (do Pix, por exemplo). Usar a palavra certa do domínio evita bug: imagine alguém lendo "anexar nota fiscal" e implementando upload da nota do mercado.
- **"MVP = todos os requisitos" não é priorizar.** MVP é o **menor** conjunto que já resolve o problema central. Teste: se você tivesse só um fim de semana, quais 3 ou 4 requisitos entregaria para a república largar a planilha? O anexo de comprovante entra? Os níveis de admin entram?

**💡 O que pode melhorar**
- **Morador que saiu e ainda deve:** ele não pode ser removido, mas continua entrando na divisão das próximas contas? Talvez exista um estado entre "ativo" e "removido".
- **Ortografia:** "Requisito funcionais" → "Requisitos funcionais"; "republica" → "república"; "adcionar" → "adicionar"; "analitics" → "analytics".

### ADR 001 — Com login

**✅ O que está bom**
- **Você mudou de ideia com base na revisão.** Isso é o processo funcionando. Uma ADR pode e deve virar o oposto quando aparece uma força nova.
- **A sessão longa** é uma tentativa real de meio-termo entre segurança e a preguiça do Marcos.

**❌ O que está errado**
- **A justificativa contradiz a decisão.** O texto diz "manter os users no sistema" para não voltar à confusão, que era o argumento a favor de *não ter* login. Agora a decisão é criar conta **com senha**, exatamente o que o briefing diz que o Marcos não faz. Pode ser a decisão certa, mas a ADR precisa dizer isso: *"escolhemos exigir senha mesmo sabendo que o Marcos resiste, porque…"*. Hoje esse trade-off está escondido.
- **O contexto já traz a solução.** "Vamos colocar um cookie de sessão…" é decisão, não contexto. O contexto deve ter só as forças: o Marcos resiste a cadastro e a Ana precisa de prova.
- **Erro técnico: cookie e localStorage são coisas diferentes.** São dois mecanismos distintos de guardar dados no navegador. Não existe "cookie no localStorage". Pesquise as diferenças, principalmente **quem consegue ler cada um** (o servidor? qualquer JavaScript da página?). Isso pesa na segurança e é um bom tema para uma nota em `Lições/`.
- **A decisão tem três decisões dentro.** Como o usuário se identifica (*autenticação*), quem pode editar (*autorização*) e como se prova um pagamento (*comprovante*). São perguntas diferentes, com alternativas diferentes. A do comprovante, pelo menos, merece uma ADR própria.
- **Ainda não tem alternativas.** Você chegou a um meio-termo, mas não registrou os outros caminhos. Quais opções você descartou até chegar a "senha + sessão de 7 dias"?

**💡 O que pode melhorar**
- **Consequências que você ainda não viu.** "Login a cada 7 dias" é só uma. Faça três perguntas: o que acontece quando o Marcos **esquecer a senha**? Quem fica responsável por guardar senhas com segurança? Onde ficam guardadas as **imagens** dos comprovantes, com orçamento zero?
- **A ADR 002 virou urgente.** Login, sessão e upload de comprovante dependem de servidor, banco e armazenamento de arquivos. Essa decisão vem antes de todas as outras.

### Para a tentativa 4

1. **Checklist dos itens repetidos:** numeração, um teste para "fácil acesso", restrições no contexto, modelo do Roteiro, Status e nome do arquivo. Faça cada um ou escreva por que discorda.
2. **SRS:** definir os papéis uma vez, resolver quem dá baixa num pagamento, trocar "nota fiscal" por "comprovante" e escolher um MVP de verdade.
3. **ADR 001:** só autenticação, com o contexto trazendo as duas forças, pelo menos duas alternativas, o trade-off com o Marcos explícito e consequências concretas.
4. **ADR 002:** onde os dados ficam. **ADR 003:** como se prova um pagamento.
5. **Opcional:** uma nota em `Lições/` sobre autenticação x autorização e cookie x localStorage.

## Aprovado — 01/10/2026

- Login com nome e senha, e sessão de 7 dias.
- Papéis admin e morador.
- Só quem cobra e o admin editam.
- Não remover quem ainda deve.
- Comprovante como prova.
- Analytics no futuro.
- "Menos de 3 segundos".

- **Fluxo de pagamento:** o morador registra o pagamento e quem cobra confirma (RF05, RF06). Isso resolve o conflito entre "Ana marca pelo nome" e "morador anexa comprovante".
- **Comprovante foi para "Depois":** a confirmação já dá a prova no MVP, e o upload exige armazenamento de arquivos.
- **Morador que saiu:** ganhou um estado próprio (RF08) em vez de ficar só "não removível".
- **Sessão renovada a cada acesso:** quem usa toda semana nunca precisa entrar de novo.
- **O admin redefine a senha:** responde ao "Marcos esqueceu a senha" sem precisar de e-mail.

### Próximo

- **ADR 0002:** onde os dados ficam, dentro do orçamento zero.
- **ADR 0003:** como se prova um pagamento. A ADR 0001 deixou isso em aberto de propósito.
