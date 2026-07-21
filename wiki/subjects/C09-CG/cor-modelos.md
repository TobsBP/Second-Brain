---
type: subject-concept
subject: C09
tags: [color-models]
updated: 2026-07-21
---

# Modelos de Cores

## RGB vs. CMYK
- **RGB (aditivo):** parte do preto e soma luz. Usado em dispositivos emissores de luz (monitores, TVs, smartphones).
- **CMYK (subtrativo):** parte do branco (papel) e subtrai luz com tintas (ciano, magenta, amarelo, preto). Usado em impressão.

A distinção existe porque monitores emitem luz enquanto papel reflete luz — os **gamuts** (gamas de cor) dos dois modelos não se sobrepõem completamente. Uma cor vibrante representável em RGB (ex.: laranja R=255,G=128,B=0) pode estar fora do gamut imprimível do CMYK; o software de impressão faz um **mapeamento de gamut**, aproximando para o equivalente disponível — o resultado costuma ficar mais apagado/menos saturado do que na tela.

## HSV
- **H (Hue/Matiz):** ângulo na roda de cores — define a cor em si.
- **S (Saturation/Saturação):** intensidade/pureza da cor.
- **V (Value/Valor):** brilho.

Mantendo H e V fixos e variando S de 0% a 100%: em S=0% a cor aparece cinza (totalmente dessaturada); conforme S aumenta, a cor fica progressivamente mais viva e pura.

## Sources
- [[raw/subjects/C09-CG/Atividade/Lista Reposição 01]] — questão 1
