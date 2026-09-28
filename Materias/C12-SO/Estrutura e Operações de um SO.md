## Multiprogramação

Capacidade de multiprogramar. Aumenta a utilização da CPU, organizando os JOBS (código e dados) de modo que sempre tenha um para executar.

## Jobs
Conjunto de tarefas que compõem uma unidade de trabalho.

- Executados de forma concorrente pela CPU.
- SO mantém vários JOBS na memória principal.

## Time Sharing
Conhecido como multitarefa, um grupo de processos fica sempre residente na memória e a CPU compartilha o tempo entre eles, dando a impressão de serem processados ao mesmo tempo. Isso é possível pelo rápido chaveamento entre os processos na memória para uso no processador.

> [!note] Complemento
> **Multiprogramação × Time Sharing**
> - Na **multiprogramação**, quando o job em execução precisa esperar (ex.: por um I/O), o SO chaveia a CPU para outro job. A CPU nunca fica ociosa, mas o usuário não interage com o programa enquanto ele roda.
> - O **time sharing** é a extensão lógica da multiprogramação: a CPU chaveia entre os processos com tanta frequência que cada usuário pode interagir com o seu programa. O objetivo é **tempo de resposta curto** (idealmente < 1 segundo).
> - Um programa carregado na memória e em execução é um **processo**.
> - Se vários jobs estão prontos ao mesmo tempo, o SO precisa escolher qual roda: isso é o **escalonamento de CPU**.
> - Se os processos não cabem todos na memória, o *swapping* move processos entre a memória e o disco; a **memória virtual** permite executar um processo que não está inteiro na memória.

## Interrupções
SOs são dirigidos por interrupções: se não tiver nada para ser executado nem dispositivo de I/O para atender, o SO fica inativo, esperando algum evento. Eventos são sinalizados por meio de interrupções ou por exceções (traps).
**Trap**: uma interrupção gerada por software, causada por um erro (ex.: divisão por zero, acesso inválido à memória) ou por uma solicitação de um programa (system call).

Mais sobre interrupções em [[Capítulo 1 - Introdução aos Sistemas Operacionais#Interrupções]].

## Dual-Mode Operation
1. Modalidade de Usuário (bit = 1)
	Quando o sistema opera em nome de uma aplicação de usuário.
2. Modalidade de Kernel (bit = 0)
	Quando está operando em nome do SO e pode executar operações privilegiadas.

> [!note] Complemento
> - O **bit de modo** fica no hardware. Na inicialização o sistema está em modo kernel; o SO carrega e começa as aplicações em modo usuário.
> - Sempre que ocorre uma interrupção ou trap, o hardware muda para modo kernel. Uma **system call** é o jeito de um programa de usuário pedir um serviço ao SO: ela gera um trap, o modo vira kernel, o SO atende o pedido e devolve o controle em modo usuário.
> - **Instruções privilegiadas** (ex.: controle de I/O, gerência do timer, tratamento de interrupções) só podem ser executadas em modo kernel. Se um programa em modo usuário tenta executar uma, o hardware não executa: gera um trap para o SO.
> - Algumas CPUs têm mais de dois modos (ex.: um modo para o gerenciador de máquinas virtuais).
>
> **Timer**
> - Garante que o SO mantenha o controle da CPU: impede que um programa de usuário entre em loop infinito ou nunca devolva a CPU.
> - É configurado para interromper o computador após um período; o SO decrementa um contador e, quando chega a zero, gera uma interrupção e o SO retoma o controle.
> - Carregar o timer é uma instrução privilegiada.
>
> Dual mode e timer são a base da [[Proteção e Segurança]] do sistema.

## Gerência de processos
SO é responsável por:
- Escalonamento de processos e threads na CPU
- Criação e destruição de processos
- Suspensão e retomada de processos
- Fornecimento de mecanismos de sincronização de processos
- Fornecimento de mecanismos de comunicação entre processos

> [!note] Complemento
> **Programa × Processo**: o programa é uma entidade *passiva* (o arquivo no disco); o processo é uma entidade *ativa* (o programa em execução, com contador de programa, registradores, memória e arquivos abertos). Um processo single-thread tem um contador de programa indicando a próxima instrução; um processo multithread tem um contador de programa por thread.

## Gerência de memória
A memória principal é um grande array de bytes, onde cada um tem seu endereço. Para um programa executar, ele (instruções e dados) precisa estar na memória.
SO responsável por:
- Saber quais partes da memória estão sendo usadas no momento e por qual processo
- Decidir quais processos (ou partes deles) e dados mover para dentro e para fora da memória
- Alocar e desalocar espaço de memória conforme necessário

## Gerência do sistema de arquivos
SO é responsável por:
- Criar e apagar arquivos e diretórios
- Suportar primitivas para manipulação de arquivos e diretórios
- Mapear arquivos no armazenamento secundário
- Fazer backup de arquivos em mídias de armazenamento estáveis (não voláteis)

## Gerência do armazenamento em Massa

Grande parte dos programas estão armazenados em memória secundária até que sejam carregados na memória principal. SO responsável por:

- Montagem e desmontagem de dispositivos
- Gerência de espaço livre
- Alocação de armazenamento
- Escalonamento de disco
- Particionamento
- Proteção

## Armazenamento cache

Quando a CPU precisa de uma informação, primeiro verifica o cache. Quando a info é encontrada no cache, ocorre o **Cache Hit**; caso contrário, **Cache Miss**, e a info é buscada no nível abaixo e copiada para o cache.
- Uma inconsistência de cache ocorre quando uma info está atualizada no cache mas não em memória (o mesmo dado com valores diferentes em níveis diferentes).
- Em sistemas com várias CPUs, cada uma com seu cache, a sincronia entre essas cópias denomina-se **Coerência de cache** e é realizada pelo hardware.

> [!note] Complemento
> - O cache é pequeno, então a gerência de cache envolve decidir o **tamanho** do cache e a **política de substituição** (quem sai quando o cache enche).
> - A transferência entre cache e registradores é feita pelo hardware; entre disco e memória, normalmente pelo SO.
> - A própria memória principal funciona como cache para o armazenamento secundário.

## Gerência de I/O

Um dos objetivos é esconder, das outras partes do kernel do SO, os detalhes (peculiaridades) de cada dispositivo de I/O.

![[Pasted image 20260803222023.png]]

> [!note] Complemento
> Quem faz isso é o **subsistema de I/O**, composto por:
> - Um componente de gerência de memória que inclui:
> 	- **Buffering**: armazenar dados temporariamente enquanto são transferidos, para compensar diferença de velocidade (ou de tamanho de bloco) entre produtor e consumidor. Ex.: dados que chegam da rede ficam num buffer até serem gravados no disco.
> 	- **Caching**: guardar uma **cópia** de dados em uma memória mais rápida para melhorar o desempenho. Diferença para o buffer: o buffer pode ter a única cópia do dado; o cache sempre tem uma cópia de algo que existe em outro lugar.
> 	- **Spooling**: sobreposição da saída de um job com a entrada de outro. Ex.: impressora, que só atende um job por vez: cada saída vai para um arquivo em disco (fila de spool) e a impressora vai consumindo em ordem.
> - Uma interface geral para drivers de dispositivo (o kernel conversa com todos do mesmo jeito).
> - **Drivers** para dispositivos de hardware específicos: só o driver conhece as peculiaridades do dispositivo a que se destina.
