## Coordenadas homogêneas

Um ponto P = (x, y) vira `[x, y, 1]ᵀ`. Com essa coordenada extra, translação, escala e rotação viram **todas multiplicação de matriz** 3×3 — e várias transformações podem ser juntadas numa matriz só.

## Transformações 2D

### Translação (Tx, Ty)

$$ T = \begin{bmatrix} 1 & 0 & T_x \\ 0 & 1 & T_y \\ 0 & 0 & 1 \end{bmatrix} $$

### Escala (Sx, Sy)

$$ S = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} $$

> [!note] Complemento
> ### Rotação (ângulo θ, em torno da origem, sentido anti-horário)
>
> $$ R = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} $$
>
> ### Outras
> - **Reflexão (espelhamento):** escala com fator −1. Em relação ao eixo x: Sx = 1, Sy = −1.
> - **Cisalhamento (shear):** `x' = x + Sh·y` — "entorta" o objeto, como um retângulo virando paralelogramo.
>
> **Escala e rotação são feitas em relação à origem.** Se o objeto não está na origem, ele também se desloca. Para girar/escalar em torno de um ponto (px, py): leva o ponto para a origem, transforma e volta:
>
> $$ M = T(p_x, p_y) \cdot R(\theta) \cdot T(-p_x, -p_y) $$
>
> **Em 3D** é igual, com matrizes 4×4 e pontos `[x, y, z, 1]ᵀ`; a rotação é feita em torno de um eixo (x, y ou z).

## Composição

Transformações em sequência = multiplicar as matrizes, **na ordem inversa da aplicação**: em `M = S · T`, o T (translação) é aplicado primeiro e depois o S (escala). Aplicar M no ponto original dá o mesmo resultado que aplicar uma por uma.

Ex. (Lista Reposição 01): P = (3, 5), T(2, −3) e depois S(2, 3):

$$ M = S \cdot T = \begin{bmatrix} 2 & 0 & 4 \\ 0 & 3 & -9 \\ 0 & 0 & 1 \end{bmatrix} \Rightarrow M \cdot P = (10, 6) $$

> [!note] Complemento
> **A ordem importa** — multiplicação de matrizes não é comutativa. Com o mesmo exemplo invertido, `T · S · P` = escala primeiro: (3·2 + 2, 5·3 − 3) = **(8, 12)**, diferente de (10, 6).
>
> Vantagem de compor: para um objeto com milhares de vértices, calcula-se M uma vez e faz-se **uma** multiplicação por vértice, em vez de uma por transformação.

## Projeções

### Perspectiva

Câmera na origem, plano de projeção a uma distância d. Um ponto P = (X, Y, Z) projeta em:

$$ X_p = \frac{X \cdot d}{Z} \qquad Y_p = \frac{Y \cdot d}{Z} $$

Quanto maior o Z (mais longe), menor a projeção — o fator homogêneo `w = Z/d` cresce com a distância. É o efeito natural de perspectiva: **dobrar a distância reduz o tamanho projetado pela metade**.

Ex.: d = 5, P = (6, 4, 15) → (2; 1,33). Q = (6, 4, 30) → (1; 0,67).

### Ortográfica vs oblíqua cavaleira

- **Ortográfica:** projetores **perpendiculares** ao plano de projeção. Não mostra profundidade: objetos do mesmo tamanho aparecem iguais independente da distância. Usada em CAD, plantas arquitetônicas, jogos 2D.
- **Oblíqua cavaleira:** projetores **paralelos**, mas **oblíquos** ao plano (45°); a profundidade aparece em verdadeira grandeza com linhas inclinadas. Usada em desenho técnico manual e ilustrações didáticas.

> [!note] Complemento
> **Classificação:**
> - **Perspectiva:** projetores convergem num ponto (centro de projeção). Realista, mas não preserva medidas. Pode ter 1, 2 ou 3 **pontos de fuga** (linhas paralelas da cena convergem para eles).
> - **Paralela:** projetores paralelos entre si (centro de projeção no infinito). Preserva paralelismo e proporções — por isso é usada onde medidas importam.
>   - **Ortográfica:** projetores ⊥ ao plano. Vistas frontal/lateral/superior, ou **axonométrica** (isométrica: os 3 eixos com o mesmo ângulo, 120°).
>   - **Oblíqua:** projetores inclinados. **Cavaleira** (45°, profundidade em tamanho real) e **cabinet/gabinete** (~63,4°, profundidade pela metade — parece mais natural).

## Pipeline gráfico (sistemas de referência)

| Sigla | Nome | Função |
|---|---|---|
| **SRO** | Sistema de Referência do Objeto | Coordenadas locais de cada objeto (modelo isolado) |
| **SRU** | Sistema de Referência Universal | Espaço do mundo, posiciona todos os objetos juntos |
| **SRN** | Sistema de Referência Normalizado | Coordenadas normalizadas (ex.: 0 a 1 ou −1 a 1), independentes do dispositivo |
| **SRD** | Sistema de Referência do Dispositivo | Coordenadas finais em pixels na tela |

**Clipping (recorte)** ocorre no **SRN**: como as coordenadas estão normalizadas, é trivial testar se algo está dentro do volume de visão. O que está fora é descartado antes do SRD, economizando processamento.

> [!note] Complemento
> O SRN existe para desacoplar a cena do dispositivo: o mesmo desenho normalizado vai para um monitor 1920×1080, uma janela 800×600 ou uma impressora só mudando a última transformação (SRN → SRD, uma escala + translação — a transformação **janela → viewport**).
>
> Em algumas referências aparece também o **SRC** (sistema de referência da câmera/observador) entre o SRU e o SRN: é onde a cena é vista do ponto de vista da câmera, antes da projeção.
>
> **Algoritmo clássico de recorte de linhas:** Cohen-Sutherland — cada ponta recebe um código de 4 bits (acima/abaixo/esquerda/direita da janela). Se as duas pontas têm código 0000, a linha está toda dentro; se o AND dos códigos ≠ 0, está toda fora; senão, recorta na borda.
