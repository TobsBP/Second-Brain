### O que fazem os Sistemas Operacionais (SO)

Um **SO**, é um programa que gerencia Hardware de um dispositivo. Atua como intermediário entre o usuário e o hardware do dispositivo.

**Objetivo**: executar programas, tornar o sistema mais simples, usar o hardware de maneira eficiente.

Um sistema de computação pode ser dividido em 4 componentes.

1 - Hardware: CPU, memória I/O
2 - SO: controla e coordena o uso de Hardware entre várias aplicações users.
3 - Programas: processadores de texto, compiladores, navegadores...
4 - Users: pessoas / maquinas buscam facilidade de uso, não se importam com a utilização de recursos, computador compartilhados devem prover transparência aos users.

O sistema é visto como um alocador de recursos, um programa de controle.

![[Pasted image 20260729214149.png|474]]

### Núcleo | Kernel

É o programa que permanece em execução no computador durante todo o tempo. 

![[Pasted image 20260729215155.png|314]]

- Programas do sistema: Estão associados ao SO, mas não fazem parte do kernel.
- Programas Aplicativos: incluem todos os programas não associados ao SO.

### Organização e Arquitetura do sistema de computação

Para o computador começar a operar é chamado o bootstrap, cujo objetivo são:
- Armazenado normalmente no ROM ou EPROM
- Inicializa todos os aspectos do sistema
- Carrega o kernel do SO, e inicia a operação
- Conhecido como firmware

### Memória Compartilhada

Quando são conectados vários dispositivos por meio  de um barramento comum.

![[Pasted image 20260729220534.png]]

### Controlador de dispositivos

Responsável por movimentar os dados entre os dispositivos periféricos que controla e seu buffer; Um controlador de dispositivo informa a CPU que terminou uma operação causando 
um interrupção. 

###  Interrupções

Um vetor de interrupções é uma tabela de endereços de memória.

 - **Interrupções de Hardware**: são feitas por sinal transmitido à CPU por meio do bus (barramento) compartilhado do sistema
 - **Interrupções de Software**: são feitas através de System Calls; Interrupções podem ser produzidas por diferentes tipos de eventos. Ex: término de um  I/O, divisão por zero, acesso inválido à memória, entre outros;
### Estrutura de I/O

O SO, deve possuir um **driver de dispositivo** uma interface software, para cada controlador de dispositivos, a fim de entendê-los e manipulá-los.

![[Pasted image 20260729221535.png]]

Ao iniciar uma operação de I/O, geralmente ocorre:

- O driver carregas os registradores dentro do controlador de dispositivo.
- Controlador examina o conteúdo dos registradores, para determinar a ação.
- O controlador inicia a transferência dos dados do dispositivo para o buffer local (byte).
- Feito a transferência, o controlador informa o driver por uma interrupção que a ação foi finalizada.
- O driver então retorna ao SO os dados.

![[Pasted image 20260729222257.png|94]]

Em caso de grandes quantidades de dados pode ocorrer o overhead, para resolver é usado o DMA (Direct Memory Access), então invés de cair para a cpu cai para a memória.

![[Pasted image 20260729222545.png]]

### Estrutura de Armazenamento

- Memória Principal
	- É a única memória que a CPU pode acessar diretamente.
	- RAM ( Memória de acesso Randômico )
	- Geralmente volátil
- Memória Secundária
	- Extensão da memória principal que fornece grande capacidade de armazenamento não-volátil
- Memória Cache
	- Pequena memória perto do processador para deixar o processamento mais rápido.

### Ciclo Fetch-Execute

Operação necessária para que um processamento ocorra.
1. Fetch: carrega as instruções da memória principal para a CPU
2. Decode: decodifica a instrução buscada
3. Execute: quando o processamento realmente ocorre

### Organização e Arquitetura do Sistema de Computação

1. Multiprocessamento Assimétrico:
	• Utiliza um esquema master-workers;
	• O processador Master controla o sistema e distribui as tarefas aos workers;
2. Multiprocessamento Simétrico:
	* Quando todos os computadores estão no mesmo nivel hirárquico