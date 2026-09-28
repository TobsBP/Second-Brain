
## Questão 1 — Modelos de Cores

### a) RGB vs CMYK

**RGB** é um modelo **aditivo**, começa no preto e soma luz. É usado em **dispositivos emissores de luz** como monitores, TVs e smartphones.

**CMYK** é um modelo **subtrativo**, começa no branco (papel) e subtrai luz com tintas (ciano, magenta, amarelo e preto). É usado em **impressão**.

A distinção existe porque monitores _emitem_ luz enquanto papel _reflete_ luz. Os gamuts (gamas de cores) dos dois modelos não se sobrepõem completamente, então algumas cores vibrantes do RGB simplesmente não existem no CMYK.

### b) Componentes do HSV

- **H (Hue / Matiz):** o ângulo na roda de cores, define a cor em si (vermelho, azul, verde etc.)
- **S (Saturation / Saturação):** a intensidade ou pureza da cor
- **V (Value / Valor):** o brilho da cor

Mantendo H e V fixos e variando S de 0% a 100%: em S=0% a cor aparece como **cinza** (completamente dessaturada), e conforme S aumenta a cor vai ficando progressivamente mais **viva e pura**, até chegar na tonalidade plena em S=100%.

### c) Por que RGB→CMYK pode mudar a cor visualmente

O laranja vibrante R=255, G=128, B=0 está dentro do gamut do RGB mas pode estar **fora do gamut imprimível do CMYK**. Além disso, tintas físicas têm impurezas e o papel absorve de forma diferente de uma tela. O software de impressão precisa fazer um **mapeamento de gamut**, aproximando a cor para o equivalente mais próximo disponível, o resultado costuma ser um laranja mais **apagado e menos saturado** do que o original na tela.

---

## Questão 2 — Curvas e Superfícies

### a) Hermite vs Bézier

A curva de **Hermite** é definida por **dois pontos extremos + dois vetores tangentes** nesses extremos. O designer controla diretamente a direção da curva nas pontas.

A curva de **Bézier** é definida por **pontos de controle** (incluindo os extremos). Os pontos intermediários não ficam sobre a curva, eles "puxam" ela como ímãs. A entrada é somente a posição desses pontos, sem tangentes explícitas.

### b) Pontos de controle em Bézier

São pontos que definem o "campo gravitacional" da curva. Os pontos extremos ficam sobre a curva; os intermediários ficam fora, atraindo-a em sua direção.

Mover um ponto de controle intermediário **puxa suavemente a curva em direção a ele**, alterando o trecho próximo a esse ponto sem criar descontinuidades abruptas, a influência é gradual e proporcional à distância.

> [!warning] Correção
> A Bézier tem **controle global**: mover **qualquer** ponto de controle altera a curva **inteira** (menos os extremos, que ficam fixos), porque cada ponto é ponderado por um polinômio de Bernstein que é diferente de zero em todo o intervalo 0 < t < 1. A influência é maior perto do ponto, mas não fica restrita ao "trecho próximo". **Controle local** (mexer um ponto só altera um pedaço da curva) é justamente a vantagem da **B-spline**. Outras propriedades úteis: a curva começa tangente ao segmento P0→P1, termina tangente a Pn−1→Pn e fica dentro do **fecho convexo** dos pontos de controle.

### c) B-spline na indústria

**Aplicação:** design de carrocerias de automóveis (CAD/CAM).

B-splines são preferidas a malhas poligonais nesse contexto porque garantem **continuidade suave** entre segmentos, produzindo superfícies sem arestas visíveis. Malhas poligonais representam a superfície com facetas planas, para obter a mesma suavidade seria necessário um número absurdo de polígonos. Com B-splines, a forma é matematicamente contínua e pode ser avaliada em qualquer resolução sem perder qualidade.

---

## Questão 3 — Iluminação e Sombreamento

### a) Componentes do modelo de iluminação local

**Ambiente:** luz indireta que ilumina uniformemente a cena, simulando reflexos do ambiente. Evita que áreas sem luz direta fiquem completamente pretas.

**Difusa:** luz que incide diretamente na superfície e se espalha igualmente em todas as direções. Depende do ângulo entre a normal da superfície e a fonte de luz (Lei de Lambert). Dá a sensação de volume e forma.

**Especular:** reflexo brilhante e concentrado, dependente do ângulo entre o observador e a direção de reflexão. Simula superfícies polidas e cria o "brilhinho" característico.

### b) Flat, Gouraud e Phong Shading

**Flat Shading:** calcula uma única cor por **polígono** (usando a normal da face). Rápido, mas resulta em faces visivelmente separadas, aparência facetada.

**Gouraud Shading:** calcula a iluminação nos **vértices** e interpola as cores resultantes pelos pixels do polígono. Suave, mas perde reflexos especulares pequenos que ficam entre vértices.

**Phong Shading:** interpola as **normais** pelos pixels e calcula a iluminação em cada pixel individualmente. É o mais custoso, mas produz o **melhor resultado visual**, especialmente para reflexos especulares precisos.

### c) Reflexo especular com bordas "quadradas"

O método provavelmente em uso é o **Gouraud Shading**, pois ele interpola cores calculadas nos vértices, se o reflexo especular cai no meio de um polígono e não nos vértices, ele é perdido ou aparece distorcido/quadrado.

A solução é adotar **Phong Shading**, que interpola as normais e recalcula a iluminação pixel a pixel, capturando o reflexo especular com precisão e suavidade em qualquer ponto da superfície.

---

## Questão 4 — Visibilidade e Hiper-realismo

### a) Z-Buffer

O Z-Buffer é um buffer adicional que armazena a **profundidade (Z) do pixel mais próximo** já renderizado para cada posição da tela. Ao renderizar um novo fragmento, seu Z é comparado com o valor armazenado: se for menor (mais próximo da câmera), o pixel é desenhado e o Z é atualizado; caso contrário, é descartado.

**Vantagem:** simples, eficiente e funciona com qualquer ordem de renderização dos objetos.

**Desvantagem:** tem dificuldade com transparência e consome memória extra; também sofre de _z-fighting_ quando dois polígonos têm profundidades muito próximas.

### b) Ray Tracing vs Rasterização

Na **rasterização**, cada triângulo é projetado na tela e os pixels são coloridos com base em modelos de iluminação local. É rápido mas simula iluminação de forma aproximada.

No **Ray Tracing**, raios são lançados a partir da câmera por cada pixel e rastreados pela cena, simulando fisicamente o comportamento da luz com reflexos, refrações e sombras.

Dois efeitos que o Ray Tracing simula naturalmente e são difíceis na rasterização:

- **Reflexos precisos** em superfícies espelhadas
- **Sombras suaves** com penumbra realista

### c) Mapeamento de textura e Normal Mapping

**Mapeamento de textura** é a técnica de aplicar uma imagem 2D sobre a superfície 3D de um objeto, adicionando cor e detalhes visuais sem aumentar a geometria.

**Normal Mapping** vai além: em vez de uma imagem de cor, aplica-se um mapa que armazena **vetores normais falsos** codificados como cores RGB. O shader usa essas normais modificadas no cálculo de iluminação, fazendo a superfície reagir à luz como se tivesse saliências e reentrâncias, criando a ilusão de geometria detalhada. A malha real continua simples; apenas a iluminação é enganada, o que é extremamente eficiente em tempo real.