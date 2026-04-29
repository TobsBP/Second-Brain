---
type: subject-overview
code: C09
title: Computação Gráfica
updated: 2026-04-29
classes_ingested: 2
---

# C09 — Computação Gráfica

## Topics covered so far
- Multimídia linear, não linear e hipermídia — classificação por controle de fluxo e estrutura de navegação.
- Operações no domínio do espaço — manipulação pixel a pixel: aritméticas (adição, subtração, multiplicação, divisão, blending) e lógicas (AND, OR, XOR, NOT).

## Concepts
- [[wiki/subjects/C09-CG/multimidia-hipermidia]] — Multimídia linear, não linear e hipermídia; quem controla o fluxo e como os blocos se conectam.
- [[wiki/subjects/C09-CG/operacoes-dominio-espaco]] — Operações pixel a pixel: aritméticas e lógicas, com riscos de overflow/underflow e divisão por zero.

## Class log
| Class | Date | Topic | Ingested |
|------|------|--------|----------|
| 1 | 2026-04-20 | Multimídia e Hipermídia | 2026-04-20 |
| ? | 2026-04-29 | Operações no Domínio do Espaço | 2026-04-29 |

## Key questions to review
- Qual é a diferença essencial entre multimídia linear, não linear e hipermídia?
- Em multimídia linear, quem controla o fluxo do conteúdo e qual é o papel da audiência?
- Por que um canal de vídeos é considerado não linear mesmo que cada vídeo seja linear?
- O que caracteriza especificamente a hipermídia em relação à multimídia não linear?
- Quais são as principais vantagens e desvantagens de cada categoria?
- Em que situação você escolheria multimídia linear em vez de hipermídia, e vice-versa?
- Que problemas típicos degradam a experiência em sistemas de hipermídia?
- O que define uma operação no domínio do espaço, e em que ela difere de uma operação em outro domínio (ex.: frequência)?
- Por que se diz que cada pixel de saída depende **apenas** dos pixels correspondentes na entrada?
- Quais são os três princípios/aplicações típicas das operações no domínio do espaço?
- Em uma adição entre imagens, o que acontece quando `128 + 192 = 320`? E na subtração quando o resultado fica negativo?
- Por que o **blending** é considerado uma "soma melhorada"? Como o coeficiente α controla a mistura?
- Por que a divisão exige tratamento especial nas operações aritméticas entre imagens?
- Qual a diferença entre aplicar AND, OR e XOR sobre duas máscaras binárias? Que máscara cada uma produz?
- Em uma imagem binária, qual valor representa o objeto e qual representa o fundo? Como o NOT altera essa representação?
- Que operação você usaria para **detectar movimento** entre dois quadros? Por quê?
- Que operação você usaria para isolar a **interseção** entre duas regiões de interesse?

## Sources
*(raw notes in `raw/subjects/C09-CG/`)*
