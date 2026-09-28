São parâmetros quantitativos para medir, avaliar e monitorar o desempenho e a eficiência da rede. Isso permite identificar possíveis gargalos e otimizar recursos de forma estratégica, além de garantir a qualidade de serviço (**QoS**), segurança, disponibilidade e escalabilidade.

- Ocupação
	- Mede a quantidade de elementos presentes em um sistema.
	- Exemplo: quantidade de carros em um estacionamento.
	- Nas redes de pacotes, mede a quantidade de bytes em uso nos nós de rede, enquanto nas redes telefônicas mede o número de chamadas simultâneas em uma central.
- Atraso
	- É o tempo parcial ou total gasto para realizar as funções implementadas em cada camada da rede.
	- Praticamente em todas as atividades de enviar ou processar ocorrem atrasos.
	- Atrasos de conexão, empacotamento, transmissão, armazenamento, roteamento, segmentação, retransmissão e processamento.
- Perda
	- Relação entre o nº de pacotes perdidos e o nº de pacotes transmitidos.
	- Principais causas de perda:
		- Falta de armazenamento (buffer cheio)
		- Erros ou falhas
		- Congestionamento
		- Atraso elevado
		- Colisão no acesso ao meio
- Vazão
	- Indica a taxa efetiva de transmissão.
	- Algumas tecnologias que utilizam o compartilhamento do meio físico podem ter uma vazão abaixo da capacidade de transmissão disponível.
- Eficiência
	- Razão entre a quantidade de informação útil transmitida e a quantidade total de informações utilizadas (carga útil + overhead).
- Bloqueio
	- Em algumas tecnologias, se a rede não possui mais recursos para atender uma nova conexão, esta poderá ser bloqueada.
- Utilização
	- Indica a fração dos recursos que estão sendo utilizados para transmissão/armazenamento.

## Atraso

### Atraso de transmissão
É o atraso exigido para que o pacote seja totalmente transmitido (colocado) pela placa de saída do roteador no enlace de dados.
- Tamanho dos pacotes → $L$ (bits)
- Velocidade de transmissão do enlace → $R$ (bits/s)

$$d_{trans} = \frac{L}{R}$$

### Atraso de propagação
O tempo necessário para que um bit possa se propagar desde o início do enlace até o seu final.
- É uma função da velocidade de propagação do enlace, na faixa de $2 \times 10^8$ m/s, e da distância desse enlace.

$$d_{prop} = \frac{d}{s}$$

> [!note] Complemento
> **Atraso nodal** — em cada nó (roteador) o pacote sofre 4 atrasos:
>
> $$d_{nodal} = d_{proc} + d_{fila} + d_{trans} + d_{prop}$$
>
> - **Processamento** ($d_{proc}$): tempo para examinar o cabeçalho, verificar erros (bits) e decidir para qual enlace de saída encaminhar. Normalmente na ordem de microssegundos.
> - **Fila** ($d_{fila}$): tempo que o pacote espera no buffer até ser transmitido. É o único **variável**: depende de quantos pacotes chegaram antes (congestionamento). Vai de ~0 a milissegundos.
> - **Transmissão** ($d_{trans} = L/R$): tempo para "empurrar" todos os bits do pacote para o enlace. Depende do tamanho do pacote e da taxa do enlace, **não** da distância.
> - **Propagação** ($d_{prop} = d/s$): tempo para um bit percorrer o enlace. Depende da distância $d$ e da velocidade $s$ do meio (entre $2 \times 10^8$ e $3 \times 10^8$ m/s), **não** do tamanho do pacote.
>
> **Transmissão × propagação:** transmissão é o tempo para colocar o pacote no fio; propagação é o tempo que o bit leva viajando pelo fio. Enlace curto e lento → domina $d_{trans}$; enlace longo e rápido (ex.: satélite) → domina $d_{prop}$.
>
> **Atraso fim a fim:** com $N$ enlaces iguais (N−1 roteadores) e sem fila:
>
> $$d_{fim\text{-}a\text{-}fim} = N \cdot (d_{proc} + d_{trans} + d_{prop})$$

> [!note] Complemento
> **Intensidade de tráfego** — mede o quanto a fila tende a crescer:
>
> $$I = \frac{L \cdot a}{R}$$
>
> - $a$ = taxa média de chegada de pacotes (pacotes/s), $L$ = tamanho do pacote (bits), $R$ = taxa do enlace (bits/s).
> - $La/R \approx 0$ → quase não há fila, atraso de fila pequeno.
> - $La/R \to 1$ → o atraso de fila cresce muito (rapidamente).
> - $La/R > 1$ → chegam mais bits do que o enlace consegue transmitir: a fila cresce sem limite e, como o buffer é finito, ocorre **perda**. Regra: projetar para $La/R < 1$.
> - Para um enlace com $La/R < 1$, a intensidade de tráfego é também a **utilização** do enlace.

## Vazão, largura de banda, jitter e perda

> [!note] Complemento
> - **Vazão (throughput):** taxa em que os bits realmente chegam ao destino (bits/s). Num caminho com vários enlaces, a vazão fim a fim é limitada pelo enlace mais lento (**gargalo**):
>
> $$\text{vazão} = \min(R_1, R_2, \dots, R_N)$$
>
> - **Largura de banda:** em redes, é a capacidade nominal (máxima) do enlace, em bits/s. Em telecom, também é a faixa de frequências do canal, em Hz. A vazão é sempre ≤ largura de banda (overhead, compartilhamento, congestionamento, retransmissões).
> - **Jitter:** variação do atraso entre pacotes consecutivos de um mesmo fluxo, causada principalmente pela variação do atraso de fila. Crítico em aplicações de tempo real (VoIP, vídeo); é compensado com um buffer de reprodução (jitter buffer).
> - **Perda de pacotes:**
>
> $$\text{taxa de perda} = \frac{\text{pacotes perdidos}}{\text{pacotes transmitidos}}$$
>
> - **Eficiência:**
>
> $$\eta = \frac{\text{carga útil}}{\text{carga útil} + \text{overhead}}$$
>
> - **Utilização:** fração do tempo em que o recurso está ocupado, $U = \frac{t_{ocupado}}{t_{total}}$.

## Exemplo numérico

> [!note] Complemento
> Pacote de $L = 1500$ bytes $= 12\,000$ bits, enlace de $R = 10$ Mbps com $d = 2000$ km, $s = 2 \times 10^8$ m/s, chegando $a = 500$ pacotes/s.
>
> - $d_{trans} = \frac{12\,000}{10 \times 10^6} = 1{,}2$ ms
> - $d_{prop} = \frac{2 \times 10^6}{2 \times 10^8} = 10$ ms
> - Desprezando processamento e fila: $d_{nodal} \approx 1{,}2 + 10 = 11{,}2$ ms (aqui domina a propagação).
> - Intensidade de tráfego: $\frac{12\,000 \cdot 500}{10 \times 10^6} = 0{,}6$ → enlace 60% utilizado, fila estável.
> - Eficiência, considerando só os cabeçalhos TCP/IP (40 bytes): $\eta = \frac{1460}{1500} \approx 97{,}3\%$.
