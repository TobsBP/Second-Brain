**Proteção**: implementação de mecanismos que controlam o acesso de processos e usuários aos recursos do SO (arquivos, memória, CPU, dispositivos). Com isso também ajuda a detectar erros nas interfaces entre subsistemas antes que eles se espalhem pelo próprio SO;
**Segurança**: consiste em defender o sistema de ataques internos e externos causados
por programas maliciosos (malwares)

> [!note] Complemento
> **Proteção × Segurança**
> - Proteção responde "**quem pode acessar o quê**": é um problema *interno* do sistema.
> - Segurança lida com ameaças: um sistema pode ter proteção adequada e mesmo assim ser inseguro. Ex.: se a senha de um usuário for roubada, o atacante acessa tudo o que aquele usuário pode acessar, e a proteção não impede, porque para o SO é o dono legítimo.
>
> Objetivos da segurança: **confidencialidade** (só quem pode lê), **integridade** (só quem pode altera) e **disponibilidade** (o sistema continua atendendo quem deve ser atendido).

> [!note] Complemento
> **Mecanismos de proteção em hardware**
> - **Dual mode** (usuário × kernel): instruções privilegiadas só rodam em modo kernel. Ver [[Estrutura e Operações de um SO#Dual-Mode Operation]].
> - **Timer**: impede que um processo monopolize a CPU.
> - **Proteção de memória**: cada processo só acessa o seu espaço de endereçamento. Na forma mais simples, usa dois registradores, **base** (menor endereço válido) e **limite** (tamanho do intervalo); todo endereço gerado em modo usuário é comparado com eles, e um acesso fora do intervalo gera um trap para o SO.
> - **Proteção de I/O**: todas as instruções de I/O são privilegiadas, então o usuário só faz I/O pedindo ao SO (system call).

> [!note] Complemento
> **Identificação de usuários**
> - O SO mantém uma lista de nomes de usuários e **identificadores de usuário (user ID / UID)**; no Windows é o *security ID (SID)*.
> - O user ID é associado a todos os processos e threads daquele usuário, e é por ele que o SO decide o acesso.
> - **Group ID**: permite dar permissões a um conjunto de usuários (ex.: todos que podem ler um arquivo do projeto).
> - **Escalonamento de privilégio**: às vezes um usuário precisa de mais permissões por um tempo. Ex.: no UNIX, o bit **setuid** faz o programa executar com o user ID do dono do arquivo, e não de quem o executou.
> - Princípio do **menor privilégio**: cada programa/usuário recebe só os privilégios necessários para a sua tarefa, limitando o estrago em caso de erro ou ataque.

> [!note] Complemento
> **Exemplos de ataques**
> - **Vírus**: código que se anexa a programas legítimos e se replica quando eles são executados.
> - **Worm**: programa que se replica sozinho pela rede, sem precisar de um hospedeiro.
> - **Cavalo de Troia (trojan)**: programa que parece legítimo mas executa ações maliciosas escondidas.
> - **Negação de serviço (DoS)**: sobrecarregar o sistema para que ele não consiga atender usuários legítimos (fere a disponibilidade).
> - **Roubo de identidade** e **roubo de serviço**: uso não autorizado das credenciais ou dos recursos de alguém.
