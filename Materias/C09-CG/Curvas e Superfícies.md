## Hermite vs Bézier

**Hermite:** definida por **dois pontos extremos + dois vetores tangentes** nesses extremos. O designer controla diretamente a direção da curva nas pontas.

**Bézier:** definida por **pontos de controle** (incluindo os extremos). Os extremos ficam sobre a curva; os intermediários normalmente não ficam, eles "puxam" a curva como ímãs. A entrada é só a posição dos pontos, sem tangentes explícitas.

> [!note] Complemento
> **Forma paramétrica:** as curvas são escritas como `P(t)`, com t indo de 0 (início) a 1 (fim). Isso permite curvas que "voltam" ou fecham, o que `y = f(x)` não consegue.
>
> **Bézier cúbica** (4 pontos P0..P3), a mais usada (fontes, SVG, Illustrator, animação CSS):
>
> $$P(t) = (1-t)^3 P_0 + 3t(1-t)^2 P_1 + 3t^2(1-t) P_2 + t^3 P_3$$
>
> Os pesos são os **polinômios de Bernstein**: sempre somam 1. Grau da curva = nº de pontos − 1.
>
> **Propriedades da Bézier:**
> - Passa pelo primeiro e pelo último ponto.
> - É tangente a P0→P1 no início e a P2→P3 no fim — por isso Hermite e Bézier cúbica são equivalentes: as tangentes da Hermite são `3(P1 − P0)` e `3(P3 − P2)`.
> - Fica dentro do **fecho convexo** dos pontos de controle.
> - **Controle global:** mover qualquer ponto altera a curva inteira (menos os extremos).
> - Muitos pontos → grau alto → difícil de controlar. Na prática se emendam várias cúbicas.
>
> **Algoritmo de de Casteljau:** constrói a Bézier por interpolações lineares sucessivas entre os pontos de controle, para um dado t — é o jeito geométrico de desenhar/subdividir a curva.

## B-splines

**Aplicação:** design de carrocerias de automóveis (CAD/CAM).

São preferidas a malhas poligonais porque garantem **continuidade suave** entre segmentos, produzindo superfícies sem arestas visíveis. Uma malha poligonal representa a superfície com facetas planas; para a mesma suavidade seria preciso um número absurdo de polígonos. Com B-splines a forma é matematicamente contínua e pode ser avaliada em qualquer resolução sem perder qualidade.

> [!note] Complemento
> - Uma B-spline é uma sequência de segmentos polinomiais (normalmente cúbicos) emendados automaticamente com continuidade C² (posição, tangente e curvatura contínuas).
> - **Controle local:** cada ponto de controle só influencia alguns segmentos vizinhos. Mexer num ponto não bagunça o resto da curva — a grande vantagem sobre a Bézier.
> - Em geral **não** passa pelos pontos de controle (nem pelos extremos, a não ser que se repitam nós).
> - O grau não depende da quantidade de pontos (dá para ter 100 pontos com curva cúbica).
> - **NURBS** (Non-Uniform Rational B-Splines): B-splines com pesos por ponto; conseguem representar cônicas exatas (círculos, elipses). Padrão em CAD.
>
> **Continuidade** (entre segmentos emendados):
> - **C⁰:** as pontas se encontram (sem buraco), mas pode ter "quina".
> - **C¹:** tangentes iguais na emenda (sem quina).
> - **C²:** curvaturas iguais na emenda (reflexo de luz contínuo — é o que importa numa carroceria).

## Superfícies

> [!note] Complemento
> Uma superfície paramétrica usa **dois** parâmetros, `P(u, v)`, com u e v em [0, 1]. A ideia é o **produto tensorial**: uma grade de pontos de controle, curva numa direção e curva na outra.
> - **Superfície de Bézier bicúbica:** grade de 4×4 = 16 pontos de controle (ex.: o bule de Utah é feito de retalhos assim).
> - **Superfícies B-spline/NURBS:** mesma ideia com controle local; o padrão em CAD/CAM.
> - Para renderizar, a superfície é **tesselada** (convertida em triângulos) na resolução desejada — por isso a mesma superfície serve para um preview rápido ou para uma renderização detalhada.
