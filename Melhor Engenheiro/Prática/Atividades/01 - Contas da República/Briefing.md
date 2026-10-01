## O problema

Uma república em Santa Rita com 5 moradores divide aluguel, luz, água, internet e mercado. Hoje funciona com um grupo de WhatsApp e uma planilha que ninguém atualiza. Todo mês tem discussão sobre quem pagou o quê e quem deve quanto para quem.

## Personas

### Ana, a tesoureira

21 anos, 3º ano de Engenharia de Computação. Paga a maior parte das contas no cartão dela e depois cobra os outros. Gasta cerca de uma hora por mês conferindo comprovantes no WhatsApp.

- **Quer:** saber em segundos quem deve quanto e parar de ser "a chata da cobrança".
- **Frustração:** morador que diz "já te paguei" e ela não tem como conferir.

### Marcos, o desligado

20 anos, 1º ano de Engenharia Elétrica. Só usa o celular e nunca abriu a planilha. Não é caloteiro, só esquece.

- **Quer:** abrir um link, ver quanto deve e pronto.
- **Não vai:** instalar app nem criar conta com senha. Se der trabalho, ele simplesmente não usa.

## O que eles pediram, nas palavras deles

- "Quero lançar uma despesa e já ver dividido." — Ana
- "Nem tudo é dividido entre todo mundo. A pizza de sexta foi só entre três." — Ana
- "Quero marcar que alguém me pagou." — Ana
- "Me manda o link que eu vejo no celular." — Marcos
- "Não sei quem deve pra quem, só sei que eu devo pra alguém." — Marcos

## Restrições

- **Orçamento zero.** Ninguém vai pagar hospedagem nem assinatura.
- **Moradores mudam todo semestre.** Entra e sai gente, e o histórico de quem saiu não pode sumir se ainda houver dívida.
- **Funciona no celular**, pelo navegador.

## Fora do escopo, por enquanto

- Pagar pelo app (Pix integrado). O pagamento continua acontecendo fora; o sistema só registra.
- Mais de uma república.

## Suas entregas

1. `Requisitos.md` com requisitos funcionais e não funcionais, separando o que entra no **MVP** do que fica para **depois**.
2. `ADRs/`, uma decisão por arquivo, seguindo o modelo do [[Roteiro]].
3. Só depois, o código.

> [!tip] Onde estão as decisões
> As ADRs estão escondidas nas falas e nas restrições acima. Exemplo: Marcos precisa ver no celular *dele* o que Ana lançou no *dela*. O que isso exige de onde os dados ficam guardados? Procure outras tensões como essa, principalmente entre o que a Ana quer e o que o Marcos aceita fazer.
