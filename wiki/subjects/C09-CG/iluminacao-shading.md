---
type: subject-concept
subject: C09
tags: [lighting, shading, rendering]
updated: 2026-07-21
---

# Iluminação, Shading e Visibilidade

## Componentes do modelo de iluminação local
- **Ambiente:** luz indireta uniforme, simula reflexos do ambiente; evita que áreas sem luz direta fiquem completamente pretas.
- **Difusa:** incide diretamente e se espalha igualmente em todas as direções; depende do ângulo entre a normal da superfície e a fonte de luz (Lei de Lambert). Dá sensação de volume e forma.
- **Especular:** reflexo brilhante e concentrado, depende do ângulo entre observador e direção de reflexão. Simula superfícies polidas ("brilhinho").

## Flat, Gouraud e Phong shading
- **Flat:** uma cor por polígono (normal da face). Rápido, mas com aparência facetada (faces visivelmente separadas).
- **Gouraud:** calcula iluminação nos vértices e interpola as cores pelos pixels do polígono. Suave, mas perde reflexos especulares pequenos que caem entre vértices — resultam em bordas "quadradas"/distorcidas no brilho.
- **Phong:** interpola as **normais** pelos pixels e recalcula iluminação em cada pixel. Mais custoso, mas produz o melhor resultado visual, principalmente para reflexos especulares precisos — é a solução quando o Gouraud perde o brilho especular.

## Z-Buffer
Buffer que armazena a profundidade (Z) do pixel mais próximo já renderizado, por posição de tela. Ao renderizar um novo fragmento, compara seu Z com o valor armazenado: se for menor (mais próximo da câmera), desenha e atualiza o buffer; caso contrário, descarta.
- **Vantagem:** simples, eficiente, funciona com qualquer ordem de renderização.
- **Desvantagem:** dificuldade com transparência, memória extra, e *z-fighting* quando dois polígonos têm profundidades muito próximas.

## Ray Tracing vs. Rasterização
- **Rasterização:** projeta triângulos na tela e colore pixels com modelos de iluminação local. Rápido, mas aproxima a iluminação.
- **Ray Tracing:** lança raios da câmera por cada pixel e os rastreia pela cena, simulando fisicamente o comportamento da luz. Efeitos que simula naturalmente (e são difíceis na rasterização): **reflexos precisos** em superfícies espelhadas e **sombras suaves** com penumbra realista.

## Mapeamento de textura e Normal Mapping
- **Mapeamento de textura:** aplica uma imagem 2D sobre a superfície 3D, adicionando cor/detalhe sem aumentar a geometria.
- **Normal Mapping:** aplica um mapa que codifica vetores normais falsos como cores RGB; o shader usa essas normais no cálculo de iluminação, fazendo a superfície reagir à luz como se tivesse relevo. A malha real continua simples — só a iluminação é enganada, o que é muito eficiente em tempo real.

## Sources
- [[raw/subjects/C09-CG/Atividade/Lista Reposição 01]] — questões 3 e 4
