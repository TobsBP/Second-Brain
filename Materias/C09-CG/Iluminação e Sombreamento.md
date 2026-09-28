## Modelo de iluminação local

**Ambiente:** luz indireta que ilumina uniformemente a cena, simulando os reflexos do ambiente. Evita que áreas sem luz direta fiquem completamente pretas.

**Difusa:** luz que incide diretamente e se espalha igualmente em todas as direções. Depende do ângulo entre a normal da superfície e a fonte de luz (**Lei de Lambert**). Dá a sensação de volume e forma.

**Especular:** reflexo brilhante e concentrado, depende do ângulo entre o observador e a direção de reflexão. Simula superfícies polidas — o "brilhinho".

> [!note] Complemento
> **Modelo de Phong** (soma das três componentes):
>
> $$ I = k_a I_a + k_d I_L (\vec{N} \cdot \vec{L}) + k_s I_L (\vec{R} \cdot \vec{V})^n $$
>
> - `N` = normal da superfície, `L` = direção da luz, `R` = direção de reflexão, `V` = direção do observador (todos unitários).
> - `ka, kd, ks` = coeficientes do material (quanto ele reflete de cada tipo).
> - Difusa: `N · L = cos θ` → máxima com a luz batendo de frente, zero com a luz rasante. **Não depende do observador.**
> - Especular: `n` = expoente de brilho. **n alto → brilho pequeno e concentrado** (metal, plástico polido); n baixo → brilho espalhado (fosco).
> - É um modelo **local**: só considera a luz vinda direto das fontes, sem sombras nem reflexos entre objetos.

## Flat, Gouraud e Phong Shading

**Flat Shading:** uma única cor por **polígono** (usando a normal da face). Rápido, mas com aparência facetada — faces visivelmente separadas.

**Gouraud Shading:** calcula a iluminação nos **vértices** e interpola as **cores** pelos pixels do polígono. Suave, mas perde reflexos especulares pequenos que caem entre vértices (brilho some ou aparece com bordas "quadradas"/distorcidas).

**Phong Shading:** interpola as **normais** pelos pixels e calcula a iluminação em cada pixel. Mais custoso, mas dá o **melhor resultado visual**, principalmente nos reflexos especulares. É a solução quando o Gouraud perde o brilho.

> [!note] Complemento
> Não confundir: **modelo de iluminação de Phong** (a fórmula ambiente + difusa + especular) ≠ **Phong shading** (o método de interpolar normais). Dá para usar o modelo de Phong com Gouraud shading.
>
> | | Calcula iluminação | Interpola | Custo |
> |---|---|---|---|
> | Flat | 1× por face | nada | baixo |
> | Gouraud | por vértice | cores | médio |
> | Phong | por pixel | normais | alto |

## Visibilidade — Z-Buffer

Buffer extra que guarda a **profundidade (Z) do pixel mais próximo** já desenhado em cada posição da tela. Ao desenhar um novo fragmento, compara o Z dele com o guardado: se for menor (mais perto da câmera), desenha e atualiza o Z; senão, descarta.

- **Vantagem:** simples, eficiente, funciona com qualquer ordem de desenho dos objetos (implementado em hardware em toda GPU).
- **Desvantagem:** dificuldade com transparência, memória extra, e *z-fighting* quando dois polígonos têm profundidades muito próximas (ficam "piscando" um sobre o outro).

> [!note] Complemento
> Outros métodos de superfícies ocultas:
> - **Algoritmo do pintor:** ordena os polígonos do mais longe para o mais perto e desenha nessa ordem (o de perto pinta por cima). Falha com polígonos que se interceptam ou se sobrepõem ciclicamente.
> - **Back-face culling:** descarta as faces viradas para trás (normal apontando para longe da câmera, `N · V < 0`). Não resolve tudo sozinho, mas elimina ~metade dos polígonos de objetos fechados antes do Z-buffer.

## Ray Tracing vs Rasterização

**Rasterização:** cada triângulo é projetado na tela e os pixels são coloridos com modelos de iluminação local. Rápido, mas aproxima a iluminação.

**Ray Tracing:** raios são lançados da câmera por cada pixel e rastreados pela cena, simulando fisicamente o comportamento da luz — reflexos, refrações e sombras.

Efeitos que o Ray Tracing faz naturalmente e são difíceis na rasterização:
- **Reflexos precisos** em superfícies espelhadas
- **Sombras suaves** com penumbra realista

> [!note] Complemento
> **Ray tracing recursivo (Whitted):** quando o raio da câmera acerta um objeto, geram-se novos raios:
> - **raio de sombra** até cada luz (se bater em algo no caminho, o ponto está na sombra);
> - **raio refletido** (superfícies espelhadas);
> - **raio refratado** (superfícies transparentes — vidro, água).
>
> O ray tracing clássico dá sombras **duras**; sombras suaves, desfoque de movimento e profundidade de campo precisam de **vários raios por pixel** (distributed ray tracing). **Radiosidade** é outra técnica de iluminação global, focada na troca de luz difusa entre superfícies ("sangramento" de cor de uma parede na outra).

## Texturas

**Mapeamento de textura:** aplicar uma imagem 2D sobre a superfície 3D, adicionando cor e detalhe sem aumentar a geometria.

**Normal Mapping:** em vez de cor, o mapa guarda **vetores normais falsos** codificados como RGB. O shader usa essas normais no cálculo de iluminação, e a superfície reage à luz como se tivesse saliências e reentrâncias. A malha continua simples; só a iluminação é enganada — muito eficiente em tempo real.

> [!note] Complemento
> - Cada vértice recebe **coordenadas de textura (u, v)** em [0, 1], dizendo qual ponto da imagem vai nele; os pixels do meio são interpolados. Um pixel da textura é um **texel**.
> - **Bump mapping:** precursor do normal mapping — usa um mapa de alturas (tons de cinza) para perturbar a normal.
> - Limitação do normal/bump mapping: a **silhueta** continua lisa, porque a geometria não mudou. **Displacement mapping** move os vértices de verdade.
> - **Mipmapping:** guarda versões reduzidas da textura (1/2, 1/4, ...) e usa a adequada à distância, evitando aliasing (cintilação/moiré) em texturas longe da câmera.
