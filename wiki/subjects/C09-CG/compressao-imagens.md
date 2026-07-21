---
type: subject-concept
subject: C09
tags: [image-compression, video-compression]
updated: 2026-07-21
---

# Compressão de Imagens e Vídeo

Redução do volume de dados de imagens/vídeo, sem perdas (reconstrução exata) ou com perdas (reconstrução aproximada, sacrificando detalhes pouco perceptíveis à visão humana em troca de arquivos bem menores).

## Sem perdas (lossless)
Mantém toda a informação da imagem original — usado quando nenhum detalhe pode ser perdido (raio-X, imagens médicas, documentos, imagens de satélite).

- **Codificação de Huffman:** símbolos frequentes recebem códigos mais curtos, símbolos raros códigos mais longos.
- **Run Length Encoding (RLE):** eficiente quando há muita redundância sequencial (ex.: `AAAAAA` → `6A`). Funciona bem em imagens artificiais geradas por editores 2D; imagens fotográficas raramente têm pixels repetidos em sequência.
- **Lempel-Ziv-Welch (LZW):** usado no GIF (limite de 256 cores, suporta transparência, entrelaçamento e animação).
- **DEFLATE:** Huffman + LZ77. Usado em PNG e no formato ZIP.

## Com perdas (lossy)
Descarta informação pouco perceptível para o olho humano. O nível de perda é proporcional à taxa de compressão.

### Domínio do espectro (JPEG)
Divide a imagem em blocos, transforma para o domínio da frequência, trunca coeficientes de alta frequência.

**Pipeline completo do JPEG:**
1. Conversão **RGB → YCbCr** (Y = luminância; Cb/Cr = crominância azul/vermelha) — o olho é mais sensível a variações de luminância do que de cor.
2. **Subamostragem de cor:** 4:4:4 (sem subamostragem), 4:2:2 (Cb/Cr com metade da resolução horizontal), 4:2:0 (metade horizontal e vertical — um valor de cor por bloco 2×2).
3. Divisão em **blocos 8×8**; valores de pixel (0–255) centralizados para −128 a +127.
4. **DCT (Discrete Cosine Transform) 2D:** decompõe cada bloco em 64 coeficientes de frequência espacial. Coeficiente DC (posição [0,0]) = média do bloco; coeficientes AC = variações. Energia concentrada nas baixas frequências (canto superior esquerdo).
5. **Quantização:** cada coeficiente é dividido pelo valor correspondente numa tabela de quantização e arredondado — etapa **irreversível**, é aqui que a perda acontece. O **fator de qualidade Q** (1–100) controla o trade-off: mais próximo de 100 = menos perda e arquivo maior; mais próximo de 1 = mais perda e arquivo menor.
6. **Zig-zag:** agrupa os zeros (coeficientes de alta frequência já zerados pela quantização) no final da sequência, favorecendo o RLE.
7. **Codificação por entropia (RLE + Huffman).**

**Descompressão (ordem inversa):** decodificação Huffman → dequantização → DCT inversa → reconstrução dos blocos → volta para RGB. Sem a tabela de quantização usada na compressão, a descompressão é impossível.

### Wavelets
JPEG + wavelets = **JPEG 2000**, usado normalmente para imagens mais pesadas.

### Compressão por fractais
Descreve matematicamente imagens complexas (como as encontradas na natureza): em vez de gerar a imagem a partir de parâmetros, descobre-se os parâmetros que a reconstroem.

## Compressão de vídeo (redundância temporal e espacial)
- **Redundância espacial** (dentro de um quadro): mesma técnica do JPEG — blocos 8×8, DCT, quantização, zig-zag + RLE + Huffman.
- **Redundância temporal** (entre quadros consecutivos): em vez de codificar cada quadro por completo, codifica-se apenas a **diferença** em relação a um quadro de referência, usando vetores de movimento (compensação de movimento) entre macroblocos.

**Tipos de quadro (MPEG-2):**
- **I (Intra-coded):** independente, comprimido só com redundância espacial (como um JPEG). Maior em tamanho; serve de âncora e permite acesso aleatório ao vídeo.
- **P (Predicted):** baseado no I ou P anterior, guarda só as diferenças (compensação de movimento). Bem menor que I.
- **B (Bidirectional):** baseado em quadros anteriores **e** posteriores. Maior compressão, mas exige reordenação na decodificação.

**Group of Pictures (GOP):** estrutura cíclica de organização dos quadros (ex.: `I B B P B B P B B P B B`), sempre inicia com um quadro I.
- **N** = tamanho total do GOP (distância entre dois I consecutivos).
- **M** = distância entre quadros de referência (I/P → próximo P).
- Exemplo N=12, M=3: `I B B P B B P B B P B B`.

## Sources
- [[raw/subjects/C09-CG/Compressão de imagens.md]]
- [[raw/subjects/C09-CG/Atividade/Lista 3]] — questões 15–26 (JPEG, tipos de compressão, frames)
- [[raw/subjects/C09-CG/Atividade/Lista 4]] — questão 6, 7, 8 (redundância temporal/espacial, MPEG-2, GOP)
- [[raw/subjects/C09-CG/Atividade/Lista de Reposição 02]] — questão 6 (pipeline JPEG, sem/com perdas)
