---
type: subject-concept
subject: C12
tags: [sistemas-operacionais, kernel, arquitetura]
updated: 2026-07-29
---

# Kernel e estrutura do sistema

O kernel é o único programa que fica em execução ininterrupta durante todo o funcionamento do computador — tudo o mais no sistema (programas de sistema ou aplicativos) roda por cima dele e pode ser encerrado ou reiniciado. O sistema de computação, visto de fora, se divide em 4 componentes: Hardware (CPU, memória, I/O), o SO (controla e coordena o uso do hardware entre aplicações), Programas (processadores de texto, compiladores, navegadores) e Users (pessoas ou máquinas que buscam facilidade de uso e esperam transparência sobre o compartilhamento de recursos). Nessa visão, o SO funciona como um **alocador de recursos** e um **programa de controle**.

## Programas de sistema × programas aplicativos
- **Programas de sistema**: associados ao SO, mas não fazem parte do kernel (ex.: utilitários, shells).
- **Programas aplicativos**: todo o resto, não associado ao SO (ex.: navegador, editor de texto).

## Bootstrap
Processo de inicialização do computador:
- Fica armazenado normalmente em ROM ou EPROM — por isso é chamado de **firmware**.
- Inicializa todos os aspectos do sistema.
- Carrega o kernel do SO e inicia sua operação.

## Cross-references
- Complementa [[wiki/subjects/C12-SO/arquitetura-e-funcoes-do-so]] — lá está o modelo de Von Neumann e as quatro gerências; aqui está a divisão hardware/SO/programas/users e o papel do kernel dentro dela.

## Sources
- [[raw/subjects/C12/Capítulo 1 - Introdução aos Sistemas Operacionais]]
