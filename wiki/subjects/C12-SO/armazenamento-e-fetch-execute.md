---
type: subject-concept
subject: C12
tags: [sistemas-operacionais, memoria, hierarquia-de-memoria]
updated: 2026-07-29
---

# Hierarquia de armazenamento e ciclo fetch-execute

O sistema organiza a memória em uma hierarquia de três níveis, cada um trocando capacidade por velocidade:

- **Memória principal (RAM)** — a única memória que a CPU acessa diretamente; é volátil.
- **Memória secundária** — extensão não-volátil da memória principal, com grande capacidade (disco).
- **Memória cache** — pequena e próxima do processador, existe só para acelerar o acesso.

## Ciclo Fetch-Execute
Toda instrução processada passa por três etapas:
1. **Fetch** — busca a instrução na memória principal e a traz para a CPU.
2. **Decode** — decodifica a instrução buscada.
3. **Execute** — executa de fato a instrução.

## Sources
- [[raw/subjects/C12/Capítulo 1 - Introdução aos Sistemas Operacionais]]
