## Contexto

Os moradores de uma república com 5 pessoas precisam organizar as contas da casa (aluguel, luz, água, internet, mercado). Querem algo simples e direto: saber quem deve a quem, quem já pagou e quem dividiu o quê com quem (ex.: a pizza de sexta só entre três).

**Restrições** (do [[Briefing]]):
- Orçamento zero: sem custo de hospedagem nem assinatura.
- Moradores mudam todo semestre, e a dívida de quem sai não pode sumir.
- Uso pelo navegador do celular, sem instalar app.
- Uma república só. O pagamento acontece fora do sistema (Pix etc.); o sistema só registra.

## Glossário

- **Despesa:** um gasto com valor, quem pagou e entre quem é dividido.
- **Pagador:** o morador que pôs o dinheiro numa despesa. É quem **cobra** a parte dos outros.
- **Participantes:** os moradores entre os quais a despesa é dividida (o pagador pode ou não estar entre eles).
- **Pagamento:** o registro de que um morador pagou o que devia a outro.
- **Saldo:** quanto um morador deve a cada outro, somando despesas e descontando pagamentos confirmados.

## Atores

- **Morador:** qualquer pessoa cadastrada na república.
- **Admin:** um morador com permissão extra para gerenciar moradores.

## Requisitos funcionais

| ID | Requisito | Prioridade |
|----|-----------|------------|
| RF01 | O sistema deve permitir que o **admin** cadastre um morador informando nome e senha inicial. | MVP |
| RF02 | O sistema deve permitir que o **morador** entre com nome e senha. | MVP |
| RF03 | O sistema deve permitir que o **morador** lance uma despesa com valor, descrição, pagador e participantes, dividindo o valor igualmente entre os participantes. | MVP |
| RF04 | O sistema deve mostrar ao **morador**, na tela inicial após entrar, quanto ele deve e para quem, e quanto devem a ele. | MVP |
| RF05 | O sistema deve permitir que o **morador** registre que pagou um valor a outro morador. O pagamento fica *aguardando confirmação*. | MVP |
| RF06 | O sistema deve permitir que o **pagador** confirme ou recuse um pagamento recebido. Só pagamentos confirmados descontam do saldo. | MVP |
| RF07 | O sistema deve permitir que só o **pagador** da despesa e o **admin** editem ou excluam uma despesa. | MVP |
| RF08 | O sistema deve permitir que o **admin** marque um morador como *saiu da república*. Ele deixa de aparecer como participante de novas despesas, mas as dívidas dele continuam visíveis até serem quitadas. | MVP |
| RF09 | O sistema não deve permitir que o **admin** remova um morador com saldo diferente de zero. | MVP |
| RF10 | O sistema deve permitir que o **admin** redefina a senha de um morador. | MVP |
| RF11 | O sistema deve permitir que o **morador** anexe um comprovante (imagem) ao registrar um pagamento. | Depois |
| RF12 | O sistema deve permitir dividir uma despesa em partes desiguais. | Depois |
| RF13 | O sistema deve mostrar ao **morador** um resumo de gastos por categoria e por mês (analytics). | Depois |

## Requisitos não funcionais

| ID | Requisito | Prioridade |
|----|-----------|------------|
| RNF01 | **Custo:** o custo de operação deve ser de R$ 0/mês. | MVP |
| RNF02 | **Desempenho:** rápido o suficiente para o morador não ficar com preguiça: cada tela responde em menos de 3 segundos numa conexão 4G. | MVP |
| RNF03 | **Plataforma:** deve funcionar no navegador do celular (Chrome no Android, Safari no iOS), em telas a partir de 360 px de largura, sem instalar nada. | MVP |
| RNF04 | **Usabilidade:** o morador vê quanto deve sem nenhum toque além do login (ver RF04). | MVP |
| RNF05 | **Sessão:** depois de entrar, o morador continua logado por pelo menos 7 dias sem precisar digitar a senha de novo. | MVP |
| RNF06 | **Segurança:** as senhas nunca são guardadas em texto puro. | MVP |

## Por que o MVP é esse

O MVP é o mínimo para a república largar a planilha: cadastrar moradores, lançar despesas, ver o saldo e registrar pagamentos com confirmação. A **confirmação de quem cobra** (RF06) já resolve o "já te paguei" da Ana sem precisar de upload. O comprovante (RF11) fica para depois porque exige armazenamento de arquivos, o que pesa no orçamento zero.
