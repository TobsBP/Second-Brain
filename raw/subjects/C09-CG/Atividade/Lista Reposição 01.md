
## Questão 1 — Tecnologias de Display

### a) Ordem cronológica

1. **CRT** (anos 1950s-60s)
2. **LCD** (anos 1970s-80s)
3. **OLED** (anos 1990s-2000s)
4. **MicroLED** (anos 2010s)

### b) Vantagem e desvantagem de cada uma

|Tecnologia|Vantagem|Desvantagem|
|---|---|---|
|CRT|Alta taxa de atualização, sem delay|Grande, pesado, alto consumo|
|LCD|Barato, eficiente energeticamente|Contraste inferior, precisa de backlight|
|OLED|Preto verdadeiro, alto contraste, fino|Burn-in, custo elevado|
|MicroLED|Brilho altíssimo, durável, sem burn-in|Muito caro, difícil de fabricar|

### c) AMOLED como evolução do OLED

O OLED padrão usa uma matriz **passiva (PMOLED)** para acionar os pixels, o que limita resolução e tamanho. O **AMOLED** usa uma matriz **ativa com transistores TFT** por pixel, permitindo controle individual mais rápido e preciso. Isso resulta em melhor desempenho em telas grandes e de alta resolução. É amplamente usado em **smartphones**.

---

## Questão 2 — Transformações Geométricas 2D

Ponto inicial: **P = (3, 5)** → em coordenadas homogêneas: `[3, 5, 1]ᵀ`

### a) Translação (Tx=2, Ty=-3)

$$ T = \begin{bmatrix} 1 & 0 & 2 \\ 0 & 1 & -3 \\ 0 & 0 & 1 \end{bmatrix} $$

$$ T \cdot P = \begin{bmatrix} 3+2 \ 5-3 \ 1 \end{bmatrix} = \begin{bmatrix} 5 \ 2 \ 1 \end{bmatrix} \Rightarrow P' = (5, 2) $$

### b) Escala (Sx=2, Sy=3)

$$ S = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & 1 \end{bmatrix} $$

$$ S \cdot P' = \begin{bmatrix} 5 \times 2 \ 2 \times 3 \ 1 \end{bmatrix} = \begin{bmatrix} 10 \ 6 \ 1 \end{bmatrix} \Rightarrow P'' = (10, 6) $$

### c) Composição das matrizes

$$ M = S \cdot T = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & 1 \end{bmatrix} \cdot \begin{bmatrix} 1 & 0 & 2 \\ 0 & 1 & -3 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 2 & 0 & 4 \\ 0 & 3 & -9 \\ 0 & 0 & 1 \end{bmatrix} $$

$$ M \cdot P_{original} = \begin{bmatrix} 2(3)+4 \\ 3(5)-9 \\ 1 \end{bmatrix} = \begin{bmatrix} 10 \\ 6 \\ 1 \end{bmatrix} \Rightarrow \textbf{(10, 6) ✓} $$

Mesmo a composição é equivalente às transformações em sequência.

---

## Questão 3 — Projeção em Perspectiva

**d = 5**, câmera na origem, **P = (6, 4, 15)**

### a) Coordenadas projetadas de P

Fórmula: $$ X_p = \frac{X \cdot d}{Z} = \frac{6 \times 5}{15} = 2 \quad Y_p = \frac{Y \cdot d}{Z} = \frac{4 \times 5}{15} = \frac{4}{3} \approx 1.33 $$

**Resultado: P projetado = (2, 1.33)**

### b) Efeito de afastamento (Z aumenta)

Conforme Z aumenta, as coordenadas projetadas diminuem (dividem-se por um valor maior). O **fator de escala homogêneo** é `w = Z/d` quanto maior o Z, maior o w, e menor o objeto projetado. Isso simula o efeito natural de perspectiva: **objetos distantes parecem menores**.

### c) Projeção de Q = (6, 4, 30)

$$ X_p = \frac{6 \times 5}{30} = 1 \quad Y_p = \frac{4 \times 5}{30} = \frac{2}{3} \approx 0.67 $$

**Q projetado = (1, 0.67)**

Comparando com P=(2, 1.33): Q está **duas vezes mais longe** e aparece **duas vezes menor** na projeção. Esse é o efeito de perspectiva, distância dobrada implica tamanho projetado na metade.

---

## Questão 4 — Pipeline Gráfico

### a) Siglas e funções

| Sigla   | Nome                                 | Função                                                |
| ------- | ------------------------------------ | ----------------------------------------------------- |
| **SRO** | Sistema de Referência do Objeto      | Coordenadas locais de cada objeto (modelo 3D isolado) |
| **SRU** | Sistema de Referência Universal      | Espaço do mundo, posiciona todos os objetos juntos    |
| **SRN** | Sistema de Referência Normalizado    | Espaço de câmera/visão, após a transformação de view  |
| **SRD** | Sistema de Referência do Dispositivo | Coordenadas finais em pixels na tela                  |

### b) Onde ocorre o clipping?

O **recorte ocorre no SRN** (espaço normalizado / view frustum), após a projeção. Nesse espaço as coordenadas estão normalizadas, o que torna trivial verificar se um objeto está dentro do volume de visão (`-1 a 1` em x, y, z). Objetos fora são descartados antes de ir para o SRD, economizando processamento.
### c) Projeção ortográfica vs. cavaleira

Na **ortográfica**, os raios de projeção são perpendiculares ao plano de projeção, sem representar profundidade visualmente, objetos do mesmo tamanho aparecem iguais independente da distância. É usada em CAD, plantas arquitetônicas e jogos 2D.

Na **oblíqua cavaleira**, os raios são paralelos mas em ângulo oblíquo, o que permite representar as três dimensões com linhas inclinadas para simular profundidade. É usada em desenho técnico manual e ilustrações didáticas.