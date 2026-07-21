---
type: subject-concept
subject: S03
tags: [np2, project-retrospective, solid]
updated: 2026-07-21
---

# Retrospectiva do Projeto NP2

Notas de revisão do projeto prático de arquitetura (jogo de cartas "Luta"/troca de cartas) e observações sobre as apresentações de outras equipes.

## Autoavaliação do projeto próprio
- Melhorias nas classes usando SPA e aplicando **OCP** (Open/Closed Principle).
- Interfaces de ataque com atributos muito parecidos — candidatas a virar uma única **classe abstrata** em vez de interfaces duplicadas.
- Tentativa de aplicar **inversão de dependência** na view de troca de cartas esbarrou na falta de implementações concretas para inverter — lição: DIP exige que já existam classes concretas cujas dependências valha a pena abstrair.
- Falta de clareza visual na troca de cartas (não dava para saber o que apareceria) — lembrete de que decisões arquiteturais também precisam de UX legível.

## Padrões de falha observados em outras equipes (peer review)
Problemas recorrentes que valem como checklist para a próxima apresentação/projeto:
- **Diagramas confusos** dificultam entender a estrutura do sistema.
- **Arquitetura mal ajustada** aos requisitos/complexidade reais do projeto.
- **God class / baixa coesão:** um serviço concentrando responsabilidades demais (ex.: serviço de integração com API externa virando uma classe gigante).
- **Excesso de métodos numa classe** — sinal de que faltou separar responsabilidades/componentizar.
- **Aprofundamento excessivo** na explicação teórica, além do necessário para a apresentação.
- **Mistura de contextos:** diagrama de classes misturando elementos de dois serviços distintos, dificultando ver a separação.
- **Inconsistência nas interfaces**, prejudicando entender abstrações e contratos definidos.
- **Falta de alinhamento entre membros da equipe** durante a apresentação.

Boas práticas observadas: justificar com clareza os critérios de escolha da arquitetura, e apresentar com boa qualidade visual/design.

Conecta diretamente com os quatro conceitos-base e SOLID em [[wiki/subjects/S03-ArqSoft/principios-design]].

## Sources
- [[raw/subjects/S03-ArqSoft/Relatorio 2.md]]
- [[raw/subjects/S03-ArqSoft/Relatorio 3.md]]
