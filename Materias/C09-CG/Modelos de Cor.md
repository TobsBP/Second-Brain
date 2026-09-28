## RGB vs CMYK

**RGB** é um modelo **aditivo**: começa no preto e soma luz (vermelho, verde, azul). Usado em dispositivos que **emitem** luz — monitores, TVs, smartphones.

**CMYK** é um modelo **subtrativo**: começa no branco (papel) e subtrai luz com tintas (ciano, magenta, amarelo e preto). Usado em **impressão**.

A distinção existe porque monitor *emite* luz e papel *reflete* luz. Os **gamuts** (gamas de cores) dos dois não se sobrepõem completamente — algumas cores vibrantes do RGB simplesmente não existem no CMYK.

Ex.: o laranja R=255, G=128, B=0 pode estar fora do gamut imprimível. O software faz um **mapeamento de gamut** (aproxima para a cor mais próxima disponível) e o impresso sai mais apagado/menos saturado do que na tela.

> [!note] Complemento
> - **Cubo RGB:** cada cor é um ponto (R, G, B) num cubo; (0,0,0) = preto, (1,1,1) = branco, e a diagonal entre eles são os cinzas (R = G = B). Com 8 bits por canal: 256³ ≈ 16,7 milhões de cores (24 bits, "true color").
> - **CMY é o complemento do RGB:** `C = 1 − R`, `M = 1 − G`, `Y = 1 − B`. Ciano absorve o vermelho, magenta absorve o verde, amarelo absorve o azul.
> - **Por que o K (preto)?** Misturar C + M + Y na prática dá um marrom escuro, não preto puro, e gasta três tintas. O preto separado dá texto nítido, preto de verdade e economiza tinta.
> - **Primárias:** aditivas = R, G, B (somar duas dá uma subtrativa: R+G = amarelo, G+B = ciano, R+B = magenta; as três = branco). Subtrativas = C, M, Y.

## HSV

- **H (Hue / Matiz):** o ângulo na roda de cores (0° a 360°), define a cor em si (vermelho, azul, verde etc.)
- **S (Saturation / Saturação):** a intensidade ou pureza da cor
- **V (Value / Valor):** o brilho da cor

Mantendo H e V fixos e variando S de 0% a 100%: em S = 0% a cor aparece **cinza** (totalmente dessaturada), e conforme S aumenta a cor fica mais **viva e pura**, até a tonalidade plena em S = 100%.

> [!note] Complemento
> HSV é **orientado ao usuário** (é como a gente descreve cor: "um azul mais claro, menos vivo"), por isso aparece em seletores de cor de editores. RGB e CMYK são **orientados ao hardware**.
> - Geometricamente é um **cone/hexacone**: H = ângulo, S = distância do eixo central, V = altura. V = 0 é preto qualquer que seja H e S.
> - Matiz: 0° vermelho, 120° verde, 240° azul.
> - Separar brilho de cor facilita processamento: dá para mexer no brilho (V) sem mudar a cor, ou segmentar um objeto pela cor (H) sem ligar para sombra.
> - **HSL/HLS** é parecido, mas com Luminosidade: L = 100% é branco (no HSV, V = 100% é a cor plena).

## YCbCr (luminância + crominância)

Usado em JPEG e vídeo digital (ver [[Compressão de imagens]]):

- **Y:** luminância (brilho)
- **Cb:** diferença de azul (B − Y)
- **Cr:** diferença de vermelho (R − Y)

Como o olho é mais sensível a brilho do que a cor, dá para reduzir a resolução de Cb e Cr (**subamostragem** 4:2:2, 4:2:0) quase sem perda perceptível.

> [!note] Complemento
> A ideia de separar luminância e crominância vem da TV analógica: **YIQ** (NTSC) e **YUV** (PAL). Assim uma TV preto e branco usa só o Y e a colorida decodifica também a cor — compatibilidade entre as duas (ver [[Vídeo e TV]]).
>
> Y é uma soma **ponderada** do RGB, porque o olho não é igualmente sensível às três cores: `Y ≈ 0,299 R + 0,587 G + 0,114 B` — o verde pesa mais, o azul quase nada. É a mesma fórmula usada para converter uma imagem colorida em tons de cinza.
