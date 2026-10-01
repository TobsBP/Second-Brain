Data: 01/10/2026

## Status

Aceita

## Contexto

Duas forças puxam para lados opostos:

- **Prova (Ana):** quem cobra precisa saber *quem* fez cada ação. Sem isso, qualquer um marca um pagamento em nome de outro, e o "já te paguei" sem prova continua.
- **Atrito (Marcos):** parte dos moradores tem preguiça de criar conta e fazer login. Se der trabalho, eles não usam, e o sistema volta a ser o grupo do WhatsApp.

Outras restrições: orçamento zero ([[SRS]] RNF01), uso pelo navegador do celular (RNF03) e uma república pequena (5 moradores) que se conhece pessoalmente.

## Decisão

O morador entra com **nome e senha**, cadastrados pelo admin (RF01, RF02). Depois do login, um **cookie de sessão HttpOnly** mantém o morador logado. O cookie vale 7 dias e é renovado a cada acesso, então quem usa pelo menos uma vez por semana nunca precisa digitar a senha de novo.

Quem pode fazer o quê (autorização) está no SRS (RF06, RF07). Como se prova um pagamento fica para a ADR 003.

## Alternativas consideradas

- **Nenhuma identificação (só um link compartilhado):** tem atrito zero, mas qualquer pessoa com o link age como qualquer morador. Isso não resolve a prova, que era o motivo do sistema. Descartada.
- **Link pessoal por morador (cada um recebe um link secreto que já o identifica):** o atrito é quase zero, como o Marcos quer. Mas o link vira a senha: se for encaminhado no grupo, quem o tiver age como aquele morador, e ninguém percebe. Descartada pela fragilidade da prova, mas é a principal concorrente se o Marcos não aderir.
- **Login com Google:** não tem senha para esquecer. Por outro lado, exige configurar OAuth com um provedor externo e obriga todo mundo a ter conta Google. É complexidade demais para 5 pessoas. Descartada.
- **Senha + sessão curta (o padrão):** é a opção mais segura, mas o login frequente vai contra a força do Marcos. Descartada.

## Consequências

- Cada ação fica associada a um morador, o que dá base para a confirmação de pagamentos (RF06).
- Com a sessão renovada, o login acontece quase só no primeiro acesso, o que reduz o atrito do Marcos.
- **Trade-off assumido:** exigimos senha mesmo sabendo que o Marcos resiste a cadastro. Para diminuir esse atrito, o admin cria a conta e passa a senha para ele, que nunca preenche um cadastro sozinho. Se mesmo assim ele não usar, reavaliar o link pessoal.
-  **Senha esquecida:** sem e-mail no sistema, quem resolve é o admin, que redefine a senha (RF10). Isso é trabalho manual para o admin.
-  **Guardar senhas passa a ser responsabilidade nossa:** elas precisam ser salvas com hash (ex.: bcrypt), nunca em texto puro (RNF06).
-  **Depende de um servidor:** a sessão e as contas exigem um back-end e um banco acessíveis pela internet. Essa é a ADR 002 (onde os dados ficam), que precisa caber no orçamento zero.
-  **Celular compartilhado ou perdido:** quem pegar o aparelho continua logado por até 7 dias.
