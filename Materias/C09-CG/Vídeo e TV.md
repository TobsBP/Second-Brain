## Varredura

**Progressiva:** todas as linhas varridas em sequência, de cima para baixo, a cada ciclo. Imagem mais nítida, sem entrelaçamento. Ex.: monitores 1080p.

**Entrelaçada:** cada quadro é dividido em **dois campos** — um com as linhas ímpares e outro com as pares — exibidos alternadamente. Em vez de 30 quadros completos/s, mostra 60 campos/s.

- **Problema que resolveu:** a **cintilação (flicker)** das primeiras TVs. A tela é "refrescada" 60 vezes por segundo **sem aumentar a banda** (cada campo tem metade das linhas).
- **Desvantagem:** artefatos em cenas com movimento rápido (os dois campos são de instantes diferentes → efeito "pente").

## Sinal de vídeo analógico

**Sinal composto:** luminância (Y) e crominância (C) no mesmo canal, em frequências diferentes. TV preto e branco usa só o Y; TV colorida decodifica o C também (compatibilidade).

- **Amplitude do sinal** → brilho (luminância)
- **Fase da subportadora de cor** → matiz
- **Amplitude da subportadora de cor** → saturação

**Sincronismo:** sincroniza a varredura do receptor com a do transmissor.
- **Horizontal:** pulso negativo curto (~4,7 µs) a ~15,75 kHz, marca o fim de cada linha.
- **Vertical:** pulso mais longo a 60 Hz (NTSC) ou 50 Hz (PAL), marca o fim de cada **campo**.

**Color burst:** referência de fase (8 a 10 ciclos da subportadora de cor) transmitida no apagamento horizontal, no início de cada linha. O oscilador do receptor se sincroniza com ela para decodificar corretamente matiz (fase) e saturação (amplitude). Sem ele, as cores ficam erradas/instáveis ou a imagem vira preto e branco.

> [!note] Complemento
> **Blanking (apagamento):** intervalo em que o feixe volta — do fim da linha para o começo da próxima (horizontal) e do fim do campo para o topo (vertical). É onde vão os pulsos de sincronismo e o color burst; o sinal fica no nível de preto ou abaixo (*mais preto que o preto*), então nada aparece na tela.
>
> **Por que 60 Hz nas Américas e 50 Hz na Europa?** A taxa de campos foi escolhida igual à frequência da rede elétrica de cada região, para evitar interferência (barras rolando na tela).
>
> **Proporção de tela:** TV analógica 4:3; HD/digital 16:9. Resoluções: SD (480i/576i), HD (720p), Full HD (1080i/p), 4K UHD (3840×2160).

## Sistemas de TV em cores

| Sistema | Linhas | Taxa | Envio de cor |
|---|---|---|---|
| **NTSC** | 525 | ~30 fps (60 campos/s) | AM em quadratura (QAM) na subportadora; sensível a erros de fase ("Never Twice the Same Color") |
| **PAL** | 625 | 25 fps (50 campos/s) | QAM com **inversão de fase a cada linha** → cancela erros de matiz automaticamente |
| **SECAM** | 625 | 25 fps (50 campos/s) | Componentes de cor em **FM**, alternadas linha a linha |

> [!note] Complemento
> O Brasil usava o **PAL-M**: cor do PAL com as 525 linhas / 60 Hz do sistema M (o mesmo do NTSC). Por isso fitas e aparelhos de fora nem sempre eram compatíveis.

## Nyquist e aliasing

Para digitalizar sem perdas, a **taxa de amostragem** tem que ser **≥ 2× a maior frequência** do sinal (`fs ≥ 2·fmax`).

**Aliasing:** acontece quando isso não é respeitado — frequências altas viram falsas frequências baixas (moiré em listras finas, "roda girando ao contrário"). **Como evitar:** filtro passa-baixa (**anti-aliasing**) antes da amostragem, ou aumentar a taxa. Mais sobre amostragem/quantização em [[Dados Multimídia]].

## TV digital

**Efeito penhasco (cliff effect):** acima de um limiar de sinal-ruído a imagem é perfeita; logo abaixo, some de uma vez (congela, blocos, tela preta) — "tudo ou nada". A **TV analógica** degrada **gradualmente** (chuviscos, fantasmas, perda de cor), mas continua assistível.

**Por que o receptor tem que ser barato:** é **um transmissor para milhões de receptores**. O custo do transmissor é diluído na audiência; qualquer centavo a mais no receptor se multiplica por milhões e inviabiliza a adoção em massa.

**Modulação/demodulação:** no transmissor, o modulador converte os dados digitais num sinal de RF pronto para irradiar (OFDM, COFDM, 8-VSB). No receptor, o demodulador faz o caminho inverso e recupera os bits.

**Compressão:** vídeo digital sempre vai comprimido (MPEG), explorando redundância **espacial** (dentro do quadro, como JPEG) e **temporal** (entre quadros, quadros I/P/B e GOP) — ver [[Compressão de imagens]].

Vantagens da transição: alta definição (HD/Full HD), melhor áudio, **multiprogramação** (vários canais na mesma faixa), interatividade e recepção móvel.

### SBTVD (padrão brasileiro)

Baseado no **ISDB-T** (japonês), com adaptações nacionais (**ISDB-Tb**):

- Vídeo em **H.264/AVC** (em vez do MPEG-2 do ISDB original).
- Áudio em **HE-AAC**.
- **Middleware Ginga** (desenvolvido no Brasil) para aplicações interativas.
- Modulação **BST-OFDM**: a faixa de 6 MHz é dividida em 13 segmentos, permitindo **recepção fixa (HD)** e **móvel/portátil (One-Seg**, 1 segmento) ao mesmo tempo.

> [!note] Complemento
> Os outros grandes padrões: **ATSC** (EUA, modulação 8-VSB — ruim para recepção móvel), **DVB-T** (Europa, COFDM) e **DTMB** (China). O OFDM divide o canal em muitas subportadoras estreitas, o que o torna robusto a **multipercurso** (o sinal chegando por vários caminhos, que na TV analógica gerava os "fantasmas").

## Tecnologias de display

Ordem cronológica: **CRT** (1950-60s) → **LCD** (1970-80s) → **OLED** (1990-2000s) → **MicroLED** (2010s).

| Tecnologia | Vantagem | Desvantagem |
|---|---|---|
| CRT | Alta taxa de atualização, sem delay | Grande, pesado, alto consumo |
| LCD | Barato, eficiente energeticamente | Contraste inferior, precisa de backlight |
| OLED | Preto verdadeiro, alto contraste, fino | Burn-in, custo elevado |
| MicroLED | Brilho altíssimo, durável, sem burn-in | Muito caro, difícil de fabricar |

**AMOLED:** evolução do OLED — troca a matriz **passiva** (PMOLED) por uma matriz **ativa** com transistores TFT por pixel, permitindo controle individual mais rápido e preciso. Melhor em telas grandes e de alta resolução; muito usado em smartphones.

> [!note] Complemento
> - **CRT:** canhão de elétrons varre a tela linha a linha, excitando fósforos RGB — é daí que vem todo o esquema de varredura e sincronismo acima.
> - **LCD:** o cristal líquido não emite luz; ele funciona como uma "persiana" que deixa passar mais ou menos da luz do **backlight** (hoje LED). Por isso o preto nunca é totalmente preto.
> - **OLED / MicroLED:** cada pixel **emite a própria luz** (emissivo) — pixel apagado = preto real, contraste "infinito". O MicroLED usa LEDs inorgânicos, que não degradam como os orgânicos (sem burn-in).
