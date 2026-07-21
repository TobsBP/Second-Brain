---
type: subject-concept
subject: C09
tags: [2d-transformations, projection, graphics-pipeline]
updated: 2026-07-21
---

# Transformações Geométricas e Pipeline Gráfico

## Transformações 2D (coordenadas homogêneas)
Um ponto P=(x,y) vira `[x, y, 1]ᵀ` para permitir representar translação, escala e rotação como multiplicação de matrizes.

- **Translação (Tx, Ty):** matriz com Tx/Ty na terceira coluna.
- **Escala (Sx, Sy):** matriz diagonal com Sx, Sy.
- **Composição:** transformações em sequência equivalem a multiplicar as matrizes na ordem inversa da aplicação (M = S·T aplica T primeiro, depois S). O resultado de aplicar a matriz composta ao ponto original é igual a aplicar as transformações uma a uma.

## Projeção em perspectiva
Com a câmera na origem e distância focal `d`, um ponto P=(X,Y,Z) projeta em:

$$X_p = \frac{X \cdot d}{Z} \qquad Y_p = \frac{Y \cdot d}{Z}$$

Quanto maior o Z (mais distante o ponto), menor a projeção — o fator de escala homogêneo `w = Z/d` cresce com a distância, dividindo o objeto projetado. É o efeito natural de perspectiva: dobrar a distância reduz o tamanho projetado pela metade.

## Pipeline gráfico (sistemas de referência)

| Sigla | Nome | Função |
|---|---|---|
| **SRO** | Sistema de Referência do Objeto | Coordenadas locais de cada objeto (modelo isolado) |
| **SRU** | Sistema de Referência Universal | Espaço do mundo, posiciona todos os objetos juntos |
| **SRN** | Sistema de Referência Normalizado | Espaço de câmera/visão, após a transformação de view |
| **SRD** | Sistema de Referência do Dispositivo | Coordenadas finais em pixels na tela |

**Clipping (recorte)** ocorre no **SRN**: nesse espaço normalizado (view frustum, -1 a 1 em x/y/z) é trivial verificar se um objeto está dentro do volume de visão. Objetos fora são descartados antes de irem ao SRD, economizando processamento.

## Projeção ortográfica vs. oblíqua cavaleira
- **Ortográfica:** raios de projeção perpendiculares ao plano de projeção; não representa profundidade visualmente (objetos do mesmo tamanho aparecem iguais independente da distância). Usada em CAD, plantas arquitetônicas, jogos 2D.
- **Oblíqua cavaleira:** raios paralelos mas em ângulo oblíquo, simula profundidade com linhas inclinadas. Usada em desenho técnico manual e ilustrações didáticas.

## Sources
- [[raw/subjects/C09-CG/Atividade/Lista 5]] — questões 2, 3 e 4
