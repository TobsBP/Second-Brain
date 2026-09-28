## O que fazem os Sistemas Operacionais (SO)

Um **SO** é um programa que gerencia o hardware de um dispositivo. Atua como intermediário entre o usuário e o hardware do dispositivo.

**Objetivo**: executar programas, tornar o sistema mais simples de usar, usar o hardware de maneira eficiente.

Um sistema de computação pode ser dividido em 4 componentes:

1 - Hardware: CPU, memória, I/O
2 - SO: controla e coordena o uso do hardware entre as várias aplicações e usuários.
3 - Programas: processadores de texto, compiladores, navegadores...
4 - Users: pessoas / máquinas; buscam facilidade de uso, não se importam com a utilização de recursos; computadores compartilhados devem prover transparência aos users.

O sistema é visto como um alocador de recursos e um programa de controle.

![[Pasted image 20260729214149.png|474]]

Base de hardware e as gerências do SO: ver [[Von Neumann e Gerências do SO]].

## Núcleo | Kernel

É o programa que permanece em execução no computador durante todo o tempo.

![[Pasted image 20260729215155.png|314]]

- Programas do sistema: estão associados ao SO, mas não fazem parte do kernel.
- Programas aplicativos: incluem todos os programas não associados ao SO.

## Organização e Arquitetura do sistema de computação

Para o computador começar a operar é executado o programa de bootstrap, que:
- Fica armazenado normalmente em ROM ou EPROM
- Inicializa todos os aspectos do sistema (registradores da CPU, controladores de dispositivos, memória)
- Carrega o kernel do SO na memória e inicia a sua operação
- É conhecido como firmware

## Memória Compartilhada

Uma ou mais CPUs e vários controladores de dispositivos são conectados por meio de um barramento comum, que dá acesso a uma memória compartilhada. CPU e dispositivos executam concorrentemente, competindo por ciclos de memória.

![[Pasted image 20260729220534.png]]

## Controlador de dispositivos

Responsável por movimentar os dados entre os dispositivos periféricos que controla e seu buffer local. Um controlador de dispositivo informa à CPU que terminou uma operação causando uma interrupção.

## Interrupções

Um vetor de interrupções é uma tabela de endereços de memória: para cada tipo de interrupção, guarda o endereço da rotina que a trata.

- **Interrupções de Hardware**: são feitas por sinal transmitido à CPU por meio do bus (barramento) compartilhado do sistema.
- **Interrupções de Software**: são feitas através de System Calls ou geradas por erros (traps/exceções).

Interrupções podem ser produzidas por diferentes tipos de eventos. Ex: término de um I/O (hardware), divisão por zero e acesso inválido à memória (software/trap), entre outros.

> [!note] Complemento
> **Como uma interrupção é tratada**
> 1. A CPU termina a instrução atual e salva o estado do que estava executando (endereço de retorno, registradores).
> 2. Usa o número da interrupção como índice no vetor de interrupções para achar a rotina de tratamento (*interrupt service routine*).
> 3. Executa a rotina.
> 4. Restaura o estado salvo e retoma a execução de onde parou, como se nada tivesse acontecido.
>
> Enquanto uma interrupção é tratada, outras podem ser desabilitadas (ou tratadas por prioridade). Interrupções *mascaráveis* podem ser desligadas temporariamente; *não mascaráveis* são reservadas para eventos críticos (ex.: erro de memória irrecuperável).
>
> O **trap** (interrupção gerada por software) aparece de novo em [[Estrutura e Operações de um SO]].

## Estrutura de I/O

O SO deve possuir um **driver de dispositivo**, uma interface de software, para cada controlador de dispositivos, a fim de entendê-los e manipulá-los.

![[Pasted image 20260729221535.png]]

Ao iniciar uma operação de I/O, geralmente ocorre:

- O driver carrega os registradores dentro do controlador de dispositivo.
- O controlador examina o conteúdo dos registradores para determinar a ação.
- O controlador inicia a transferência dos dados do dispositivo para o buffer local (byte a byte).
- Feita a transferência, o controlador informa ao driver, por uma interrupção, que a ação foi finalizada.
- O driver então retorna os dados ao SO.

![[Pasted image 20260729222257.png|94]]

Em caso de grandes quantidades de dados, gerar uma interrupção por byte causa overhead. Para resolver é usado o DMA (Direct Memory Access): o controlador transfere blocos inteiros de dados diretamente entre o seu buffer e a memória principal, sem a CPU intervir em cada byte. Só é gerada **uma interrupção por bloco**, e a CPU fica livre para outras tarefas enquanto a transferência acontece.

![[Pasted image 20260729222545.png]]

## Estrutura de Armazenamento

- Memória Principal
	- É a única memória de armazenamento grande que a CPU pode acessar diretamente.
	- RAM (Random Access Memory, memória de acesso aleatório)
	- Geralmente volátil
- Memória Secundária
	- Extensão da memória principal que fornece grande capacidade de armazenamento não volátil (HD, SSD)
- Memória Cache
	- Pequena memória perto do processador para deixar o processamento mais rápido.

> [!note] Complemento
> **Hierarquia de armazenamento**: quanto mais perto da CPU, mais rápido, mais caro por byte e menor.
>
> | Nível | Exemplo | Volátil? |
> |---|---|---|
> | Registradores | dentro da CPU | sim |
> | Cache | L1/L2/L3 | sim |
> | Memória principal | RAM | sim |
> | Memória não volátil (NVM) | SSD | não |
> | Disco magnético | HD | não |
> | Óptico / fita | DVD, fita magnética | não |
>
> O cache funciona porque os programas tendem a reusar dados e instruções próximos (localidade). Mais detalhes em [[Estrutura e Operações de um SO#Armazenamento cache]].

## Ciclo Fetch-Execute

Operação necessária para que um processamento ocorra.
1. Fetch: carrega a instrução da memória principal para a CPU
2. Decode: decodifica a instrução buscada
3. Execute: quando o processamento realmente ocorre

## Organização e Arquitetura do Sistema de Computação: Multiprocessamento

1. Multiprocessamento Assimétrico:
	- Utiliza um esquema master-workers;
	- O processador master controla o sistema e distribui as tarefas aos workers;
2. Multiprocessamento Simétrico (SMP):
	- Quando todos os processadores estão no mesmo nível hierárquico;
	- Cada processador executa qualquer tarefa, inclusive funções do SO e processos de usuário.

> [!note] Complemento
> **Tipos de sistema (Silberschatz)**
> - **Monoprocessado**: uma única CPU de uso geral.
> - **Multiprocessado**: duas ou mais CPUs compartilhando barramento, memória e dispositivos. No SMP cada CPU tem seus próprios registradores e cache, mas a memória física é compartilhada.
> - **Multicore**: vários núcleos no mesmo chip; comunicação entre núcleos mais rápida e menos consumo de energia que vários chips separados.
> - **Clusters**: vários computadores (nós) ligados em rede trabalhando juntos, usados para alta disponibilidade e alto desempenho.
>
> **Vantagens do multiprocessamento**
> - Maior *throughput*: mais trabalho feito no mesmo tempo (mas com N CPUs o ganho é menor que N, por causa do overhead de coordenação e da disputa por recursos compartilhados).
> - Economia de escala: CPUs compartilham periféricos, memória e fonte.
> - Maior confiabilidade: a falha de uma CPU não para o sistema, só o deixa mais lento (degradação suave / tolerância a falhas).
