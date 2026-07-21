---
type: subject-concept
subject: C09
tags: [analog-video, digital-tv, display-technology]
updated: 2026-07-21
---

# Vídeo Analógico, TV Digital e Tecnologias de Display

## Varredura entrelaçada vs. progressiva
- **Entrelaçada:** cada quadro é dividido em dois campos (linhas ímpares / pares), exibidos alternadamente — 60 campos/s em vez de 30 quadros completos/s. Reduz a cintilação (flicker) percebida sem aumentar a banda (cada campo tem metade das linhas).
- **Progressiva:** todas as linhas varridas sequencialmente a cada ciclo — imagem mais nítida, sem entrelaçamento. Ex.: monitores 1080p. Entrelaçada pode causar artefatos em cenas com movimento rápido.

## Nyquist, aliasing e o efeito penhasco
- **Teorema de Nyquist:** a taxa de amostragem deve ser ≥ 2× a maior frequência do sinal (fs ≥ 2·fmax) para reconstrução fiel.
- **Aliasing:** ocorre quando essa condição é violada — frequências altas viram falsas frequências baixas (padrões moiré, "roda girando ao contrário"). Evita-se com um filtro passa-baixa anti-aliasing antes da amostragem, ou aumentando a taxa.
- **Efeito penhasco (cliff effect, TV digital):** acima de um limiar de sinal-ruído a imagem é perfeita; logo abaixo, desaparece de uma vez (congelamentos, blocos, tela preta) — "tudo ou nada". A **TV analógica** degrada gradualmente (chuviscos, fantasmas, perda de cor), mas continua assistível.

## Por que o receptor precisa ser barato
Existe um transmissor para milhões de receptores — o custo do transmissor é diluído entre toda a audiência (pode ser caro/sofisticado); qualquer aumento de custo no receptor se multiplica por milhões de unidades e inviabiliza a adoção em massa.

## Modulação/demodulação
No transmissor, o modulador (após o D/A) converte dados digitais num sinal analógico de RF pronto para irradiar (ex.: OFDM, COFDM, 8-VSB). No receptor, o demodulador (seguido do A/D) recupera os bits a partir do sinal de RF captado.

## Sistemas de TV em cores

| Sistema | Linhas | Taxa | Envio de cor |
|---|---|---|---|
| **NTSC** | 525 | ~30 fps (60 campos/s) | AM em quadratura (QAM); sensível a erros de fase |
| **PAL** | 625 | 25 fps (50 campos/s) | QAM com inversão de fase a cada linha — cancela erros de matiz automaticamente |
| **SECAM** | 625 | 25 fps (50 campos/s) | Componentes de cor em FM, alternadas linha a linha |

**Padrão brasileiro (SBTVD):** baseado no ISDB-Tb (japonês) com adaptações — vídeo em H.264/AVC (em vez do MPEG-2 original), áudio em HE-AAC, middleware **Ginga** (nacional) para interatividade, modulação BST-OFDM permitindo recepção fixa (HD) e móvel/portátil (One-Seg) na mesma faixa.

**Color burst:** referência de fase transmitida no início de cada linha (8–10 ciclos da subportadora de cor), usada pelo oscilador do receptor para se sincronizar e decodificar corretamente matiz (fase) e saturação (amplitude) da crominância. Sem ele, as cores ficam erradas/instáveis ou a imagem vira preto e branco.

**Sincronismo:** horizontal — pulso negativo curto (~4,7 µs, ~15,75 kHz) sinaliza fim de linha; vertical — pulso mais longo a 60 Hz (NTSC) ou 50 Hz (PAL) sinaliza fim de quadro.

## Tecnologias de display
Ordem cronológica: **CRT** (1950-60s) → **LCD** (1970-80s) → **OLED** (1990-2000s) → **MicroLED** (2010s).

| Tecnologia | Vantagem | Desvantagem |
|---|---|---|
| CRT | Alta taxa de atualização, sem delay | Grande, pesado, alto consumo |
| LCD | Barato, eficiente energeticamente | Contraste inferior, precisa de backlight |
| OLED | Preto verdadeiro, alto contraste, fino | Burn-in, custo elevado |
| MicroLED | Brilho altíssimo, durável, sem burn-in | Muito caro, difícil de fabricar |

**AMOLED:** evolução do OLED — troca a matriz passiva (PMOLED) por uma matriz ativa com transistores TFT por pixel, permitindo controle individual mais rápido e preciso. Melhor para telas grandes/alta resolução; amplamente usado em smartphones.

## Sources
- [[raw/subjects/C09-CG/Atividade/Lista 3]] — questões 30–37
- [[raw/subjects/C09-CG/Atividade/Lista 4]]
- [[raw/subjects/C09-CG/Atividade/Lista 5]] — questão 1
- [[raw/subjects/C09-CG/Atividade/Lista de Reposição 02]] — questões 7–9
