Notas da aula introdutória (mapa mental em [[Aula introdutória.excalidraw|Aula introdutória]]).

## Von Neumann

O SO roda sobre um hardware organizado segundo a arquitetura de Von Neumann:

| Bloco | Papel |
|-------|-------|
| **CPU** | Busca, decodifica e executa instruções (ciclo fetch-decode-execute) |
| **RAM** | Memória principal, guarda instruções e dados do programa em execução |
| **I/O** | Entrada e saída, comunicação com o mundo externo |

Característica central: instruções e dados ficam na **mesma memória**.

> [!note] Complemento
> Como instruções e dados passam pelo mesmo barramento entre CPU e memória, a CPU costuma ficar esperando a memória: é o chamado **gargalo de Von Neumann**. É um dos motivos para existir a memória cache (ver [[Capítulo 1 - Introdução aos Sistemas Operacionais#Estrutura de Armazenamento]]).

## Hardware × Software

- Hardware: a parte física (Von Neumann).
- Software: tudo que roda sobre ele. O **SO é software**, o mais próximo do hardware, e fica entre o hardware e as aplicações do usuário.

## Gerências do SO

O SO gerencia os recursos do hardware e os apresenta às aplicações de forma abstrata e compartilhável:

- **Processos**: criação, escalonamento e término de processos e threads; quem usa a CPU e quando.
- **Memória**: alocação da RAM entre os processos, proteção e endereçamento.
- **Entrada e saída**: acesso aos dispositivos, drivers, buffers.
- **Memória secundária**: disco / armazenamento persistente, sistemas de arquivos.

Cada gerência em detalhe: [[Estrutura e Operações de um SO]].

## SO não é obrigatório

Sistema operacional não é necessário em todos os sistemas de computador. Sistemas embarcados simples, microcontroladores e máquinas de propósito único podem rodar um único programa direto sobre o hardware: não há vários processos disputando recursos nem várias aplicações para coordenar.

## Avaliação

- Atividades práticas, incluindo um **projeto com threads** (apresentações em [[Atividades/Reports|Reports]])
- Seminário
- Prova, dividida em Pt-1 e Pt-2
