---
type: subject-concept
subject: C12
tags: [sistemas-operacionais, io, interrupcoes, dma]
updated: 2026-07-29
---

# Interrupções e estrutura de I/O

Dispositivos periféricos se conectam ao sistema por um barramento comum (memória compartilhada), cada um controlado por um **controlador de dispositivo** — responsável por mover dados entre o periférico e seu buffer local, avisando a CPU quando termina uma operação através de uma **interrupção**. O SO enxerga cada controlador através de um **driver de dispositivo**, uma interface de software que sabe como manipulá-lo.

## Interrupções
Um **vetor de interrupções** é uma tabela de endereços de memória usada para despachar cada tipo de interrupção para sua rotina de tratamento.

- **Interrupções de hardware**: sinal transmitido à CPU pelo barramento compartilhado do sistema.
- **Interrupções de software**: geradas por *system calls*.

Podem ser produzidas por diversos eventos: término de uma operação de I/O, divisão por zero, acesso inválido à memória, entre outros.

## Ciclo de uma operação de I/O
1. O driver carrega os registradores do controlador de dispositivo.
2. O controlador examina o conteúdo dos registradores para determinar a ação.
3. O controlador inicia a transferência de dados do dispositivo para o buffer local, byte a byte.
4. Ao terminar, o controlador avisa o driver via interrupção.
5. O driver retorna os dados ao SO.

## DMA (Direct Memory Access)
Transferências byte a byte geram overhead de CPU quando o volume de dados é grande. O DMA resolve isso: em vez de cada byte passar pela CPU, a transferência cai diretamente para a memória, liberando a CPU para outras tarefas.

## Cross-references
- [[wiki/subjects/C12-SO/kernel-e-estrutura-do-sistema]] — driver de dispositivo é um tipo de programa de sistema.

## Sources
- [[raw/subjects/C12/Capítulo 1 - Introdução aos Sistemas Operacionais]]
