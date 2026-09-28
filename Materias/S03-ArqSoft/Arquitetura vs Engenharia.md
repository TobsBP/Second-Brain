Arquitetura de software é um **subcampo** da engenharia de software. As duas são complementares, mas com focos diferentes. Caiu na [[Practice exam NP1]] (questões 2 a 5), e a pegadinha era inverter as duas.

## Engenharia de software
Foco no **processo** de desenvolvimento: como o software é produzido. Estuda, desenvolve e aplica ferramentas, metodologias e práticas para ganhar produtividade e qualidade no processo, ao longo de todo o ciclo de vida.

## Arquitetura de software
Foco no **projeto (design)** do sistema: como ele é estruturado e como os componentes se comunicam entre si. Define a estrutura, os padrões e as decisões de alto nível.

| | Engenharia | Arquitetura |
|---|---|---|
| Foco | Processo | Projeto (design) |
| Pergunta | Como desenvolvemos software? | Como o sistema é estruturado? |
| Escopo | Ciclo de desenvolvimento | O sistema e seus componentes |

## Papéis
- **Arquiteto:** mais **consultivo e orientativo**. Apoia decisões de design, padrões e tradeoffs, e orienta as equipes.
- **Engenheiro:** mais **gerencial e orientado ao código**. Executa, implementa e gerencia a construção.

## Escopo da arquitetura
- Decidir quais padrões arquiteturais usar (ou adaptar), ver [[Padrões Arquiteturais]]
- Modelar diagramas com a visão geral do sistema
- Especificar requisitos de desempenho, segurança, escalabilidade etc., e quais componentes garantem cada um e como
- Definir responsabilidades e funcionalidades de cada componente
- Considerar o ciclo de vida do sistema (correções e evoluções)
- Estudar e consolidar padrões e boas práticas
- Analisar pontos críticos de falha e decidir do que dá para abrir mão (tradeoffs)
- Manter documentação do sistema

**Não faz parte:** realizar testes automatizados. Isso é da engenharia/qualidade de software.

> [!note] Complemento
> Os requisitos de desempenho, segurança, escalabilidade etc. são os **requisitos não funcionais** (atributos de qualidade). Eles costumam pesar mais na escolha da arquitetura do que os funcionais: quase qualquer arquitetura "faz a funcionalidade", mas nem toda aguenta 1 milhão de usuários ou funciona offline. Por isso decisão arquitetural é sempre tradeoff (ganha em um atributo, perde em outro).
