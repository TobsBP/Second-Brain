---
type: subject-overview
code: C12
title: Sistemas Operacionais
updated: 2026-07-29
classes_ingested: 2
---

# C12 — Sistemas Operacionais

## Topics covered so far
- Arquitetura de Von Neumann (CPU, RAM, I/O) como base de hardware sobre a qual o SO opera
- Divisão Hardware × Software e onde o SO se encaixa
- As quatro gerências do SO: processos, memória, entrada e saída, memória secundária
- Um sistema operacional **não** é necessário em todos os sistemas de computador
- Kernel, bootstrap/firmware, programas de sistema × aplicativos, os 4 componentes do sistema (Hardware/SO/Programas/Users)
- Controlador de dispositivos, interrupções (hardware × software), estrutura de I/O, DMA
- Hierarquia de memória (principal/secundária/cache) e ciclo Fetch-Decode-Execute
- Multiprocessamento assimétrico (master-workers) × simétrico

## Concepts
- [[wiki/subjects/C12-SO/arquitetura-e-funcoes-do-so]] — Von Neumann + as quatro gerências do SO
- [[wiki/subjects/C12-SO/kernel-e-estrutura-do-sistema]] — kernel, bootstrap, programas de sistema × aplicativos
- [[wiki/subjects/C12-SO/interrupcoes-e-estrutura-io]] — controlador de dispositivos, interrupções, driver, DMA
- [[wiki/subjects/C12-SO/armazenamento-e-fetch-execute]] — hierarquia de memória e ciclo fetch-execute
- [[wiki/subjects/C12-SO/multiprocessamento]] — assimétrico (master-workers) × simétrico

## Avaliação
- **Atividades práticas** — incluindo um **projeto com threads**
- **Seminário**
- **Prova** — dividida em Pt-1 e Pt-2

## Cross-references
- [[wiki/subjects/S03-ArqSoft/S03-ArqSoft-overview]] — camadas e separação de responsabilidades aparecem aqui como camadas hardware/SO/aplicação.

## Class log
| Class | Date | Topic | Ingested |
|------|------|--------|----------|
| Aula introdutória | 2026-07-27 | Von Neumann, funções do SO, avaliação | 2026-07-27 |
| Capítulo 1 — Introdução aos SO | 2026-07-29 | Kernel, bootstrap, interrupções, I/O/DMA, hierarquia de memória, fetch-execute, multiprocessamento | 2026-07-29 |

## Key questions to review
1. Quais são os três blocos da arquitetura de Von Neumann e qual o papel de cada um?
2. Por que o SO é classificado como software e não como hardware?
3. Quais são as quatro gerências sob responsabilidade do SO?
4. Qual a diferença entre gerência de memória e gerência de memória secundária?
5. Em que casos um sistema de computador dispensa um sistema operacional? Por quê?
6. Por que um projeto com threads é escolhido como atividade prática de um curso de SO?
7. Qual a diferença entre programas de sistema e programas aplicativos, em relação ao kernel?
8. Qual é a função do bootstrap e por que ele é chamado de firmware?
9. Qual a diferença entre uma interrupção de hardware e uma de software?
10. Descreva o ciclo completo de uma operação de I/O, do driver ao retorno dos dados ao SO.
11. Por que o DMA existe e que problema ele resolve em relação à transferência byte a byte?
12. Compare memória principal, secundária e cache em termos de velocidade, capacidade e volatilidade.
13. Quais são as três etapas do ciclo fetch-execute?
14. Qual a diferença entre multiprocessamento simétrico e assimétrico?

## Sources
- [[raw/subjects/C12/Aula introdutória.excalidraw.md]] — mapa mental da aula introdutória
- [[raw/subjects/C12/Capítulo 1 - Introdução aos Sistemas Operacionais]] — kernel, bootstrap, interrupções, I/O, memória, fetch-execute, multiprocessamento
