---
type: subject-concept
subject: C12
tags: [sistemas-operacionais, arquitetura, hardware]
updated: 2026-07-27
---

# Arquitetura de Von Neumann e funções do SO

O sistema operacional é a camada de **software** que fica entre o **hardware** (organizado segundo a arquitetura de Von Neumann) e as aplicações do usuário. Seu papel é gerenciar os recursos escassos desse hardware — processador, memória, dispositivos de I/O e armazenamento — apresentando-os às aplicações de forma abstrata e compartilhável. Nem todo sistema de computador precisa de um SO: sistemas simples ou dedicados podem rodar um único programa direto sobre o hardware.

## Von Neumann
Os três blocos do modelo:

| Bloco | Papel |
|-------|-------|
| **CPU** | Busca, decodifica e executa instruções |
| **RAM** | Memória principal — guarda instruções e dados do programa em execução |
| **I/O** | Entrada e saída — comunicação com o mundo externo |

A característica central do modelo é que instruções e dados compartilham a mesma memória.

## Hardware × Software
A aula separa explicitamente as duas camadas: o hardware é o físico (Von Neumann), o software é tudo que roda sobre ele — e o SO é o software mais próximo do hardware.

## As quatro gerências do SO
- **Gerência de processos** — criação, escalonamento e término de processos e threads; quem usa a CPU e quando.
- **Gerência de memória** — alocação da RAM entre processos, proteção e endereçamento.
- **Gerência de entrada e saída** — mediação do acesso aos dispositivos, drivers, buffers.
- **Gerência de memória secundária** — disco/armazenamento persistente, sistemas de arquivos.

## Quando o SO não é necessário
> "Sistema operacional não é necessário em todos os sistemas de computador."

Sistemas embarcados simples, microcontroladores e máquinas de propósito único podem executar um programa único diretamente sobre o hardware, sem a camada de gerenciamento — não há concorrência de processos nem multiplicidade de aplicações para arbitrar.

## Sources
- [[raw/subjects/C12/Aula introdutória.excalidraw.md]]
