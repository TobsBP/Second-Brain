---
type: subject-overview
code: C09
title: Computação Gráfica
updated: 2026-07-21
classes_ingested: 2
---

# C09 — Computação Gráfica

## Topics covered so far
- Multimídia linear, não linear e hipermídia — classificação por controle de fluxo e estrutura de navegação.
- Operações no domínio do espaço — manipulação pixel a pixel: aritméticas (adição, subtração, multiplicação, divisão, blending) e lógicas (AND, OR, XOR, NOT).
- Dados multimídia, pipeline de processamento de imagem e filtragem no domínio da frequência.
- Compressão de imagens e vídeo (JPEG, wavelets, fractais, redundância temporal/espacial, MPEG).
- Geometria: transformações 2D, projeção em perspectiva, pipeline gráfico (SRO/SRU/SRN/SRD).
- Modelos de cor (RGB/CMYK/HSV), curvas e superfícies (Hermite/Bézier/B-spline).
- Iluminação, shading (flat/Gouraud/Phong), Z-Buffer, ray tracing vs. rasterização.
- Vídeo analógico, TV digital (NTSC/PAL/SECAM/SBTVD) e tecnologias de display.

*(as 5 listas de atividades/reposição cobrem o programa inteiro de forma consolidada — ver [[log.md]] em 2026-07-21)*

## Concepts
- [[wiki/subjects/C09-CG/multimidia-hipermidia]] — Multimídia linear, não linear e hipermídia; quem controla o fluxo e como os blocos se conectam.
- [[wiki/subjects/C09-CG/operacoes-dominio-espaco]] — Operações pixel a pixel: aritméticas e lógicas, com riscos de overflow/underflow e divisão por zero.
- [[wiki/subjects/C09-CG/dados-multimidia]] — Sistema de informação multimídia, dados estáticos/dinâmicos, pipeline de processamento de imagem.
- [[wiki/subjects/C09-CG/filtragem-dominio-frequencia]] — Pipeline FFT → filtragem → IFFT, passa-alta vs. passa-baixa.
- [[wiki/subjects/C09-CG/compressao-imagens]] — Lossless (Huffman/RLE/LZW/DEFLATE), JPEG completo, wavelets, fractais, MPEG (I/P/B, GOP).
- [[wiki/subjects/C09-CG/geometria-transformacoes]] — Transformações 2D, projeção perspectiva, SRO/SRU/SRN/SRD, clipping.
- [[wiki/subjects/C09-CG/cor-modelos]] — RGB vs. CMYK, HSV.
- [[wiki/subjects/C09-CG/curvas-superficies]] — Hermite, Bézier, B-splines.
- [[wiki/subjects/C09-CG/iluminacao-shading]] — Iluminação local, shading, Z-Buffer, ray tracing, texture/normal mapping.
- [[wiki/subjects/C09-CG/video-tv-broadcast]] — Vídeo analógico, Nyquist/aliasing, NTSC/PAL/SECAM/SBTVD, displays.

## Class log
| Class | Date | Topic | Ingested |
|------|------|--------|----------|
| 1 | 2026-04-20 | Multimídia e Hipermídia | 2026-04-20 |
| ? | 2026-04-29 | Operações no Domínio do Espaço | 2026-04-29 |
| Atividades | — | Listas 3–5 e Reposições 1–2 (revisão geral) | 2026-07-21 |

## Key questions to review
- Qual é a diferença essencial entre multimídia linear, não linear e hipermídia?
- Em multimídia linear, quem controla o fluxo do conteúdo e qual é o papel da audiência?
- Por que um canal de vídeos é considerado não linear mesmo que cada vídeo seja linear?
- O que caracteriza especificamente a hipermídia em relação à multimídia não linear?
- Quais são as principais vantagens e desvantagens de cada categoria?
- Em que situação você escolheria multimídia linear em vez de hipermídia, e vice-versa?
- Que problemas típicos degradam a experiência em sistemas de hipermídia?
- O que define uma operação no domínio do espaço, e em que ela difere de uma operação em outro domínio (ex.: frequência)?
- Por que se diz que cada pixel de saída depende **apenas** dos pixels correspondentes na entrada?
- Quais são os três princípios/aplicações típicas das operações no domínio do espaço?
- Em uma adição entre imagens, o que acontece quando `128 + 192 = 320`? E na subtração quando o resultado fica negativo?
- Por que o **blending** é considerado uma "soma melhorada"? Como o coeficiente α controla a mistura?
- Por que a divisão exige tratamento especial nas operações aritméticas entre imagens?
- Qual a diferença entre aplicar AND, OR e XOR sobre duas máscaras binárias? Que máscara cada uma produz?
- Em uma imagem binária, qual valor representa o objeto e qual representa o fundo? Como o NOT altera essa representação?
- Que operação você usaria para **detectar movimento** entre dois quadros? Por quê?
- Que operação você usaria para isolar a **interseção** entre duas regiões de interesse?
- Por que a segmentação é a etapa crítica do pipeline de processamento de imagem? O que diferencia super de subsegmentação?
- Como funciona o pipeline JPEG do RGB até a codificação por entropia — e em qual etapa a perda é irreversível?
- Qual a diferença entre redundância espacial e temporal na compressão de vídeo? O que são quadros I, P e B, e o que definem N e M num GOP?
- Como se compõem matrizes de transformação 2D, e por que a ordem de multiplicação importa?
- Em que sistema de referência (SRO/SRU/SRN/SRD) ocorre o clipping, e por quê?
- Qual a diferença entre RGB e CMYK, e por que uma cor pode "mudar" ao converter de um para o outro?
- Qual a diferença entre curvas de Hermite e Bézier? Por que a indústria automotiva usa B-splines em vez de malhas poligonais?
- Qual a diferença entre Flat, Gouraud e Phong shading — e por que o Gouraud pode perder reflexos especulares?
- Como funciona o Z-Buffer, e que problema (z-fighting) ele pode ter?
- Por que a TV digital tem o "efeito penhasco" enquanto a TV analógica degrada gradualmente?
- Quais as diferenças entre NTSC, PAL e SECAM? O que é o SBTVD e quais tecnologias ele reúne?

## Sources
*(raw notes in `raw/subjects/C09-CG/`)*
