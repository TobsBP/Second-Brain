Tipos de compressão de imagens estáticas

---
## Sem perdas

Mantém toda a informação contida na imagem original — a imagem descomprimida é idêntica bit a bit.

Comum em raio-X (imagens médicas), documentos e imagens de satélite, onde nenhum detalhe pode ser perdido.

### Codificação de Huffman

Letras que aparecem muito: a, e - recebem códigos mais curtos
Letras raras: z, x - recebem códigos mais longos

Numa imagem, os "símbolos" são os valores dos pixels: tons que aparecem muito ganham códigos curtos.

### Run Length Encoding

Eficiente quando há muita redundância na sequência de dados.

```
AAAAAA  BBBBBBBB
6A      8B
```

Aplicar em imagens artificiais, geradas por editores 2D, pois imagens de foto dificilmente vão ter pixels repetidos em sequência.

### Lempel Ziv Welch

Muito usada no formato GIF, limitação de 256 cores.
Tem cor transparente, entrelaçamento e animação.

Monta um dicionário de sequências que já apareceram e passa a mandar só o índice da sequência no dicionário.

### DEFLATE - junção de Huffman + LZ77

Também é o método usado na compressão ZIP e no formato PNG.

---
## Compressão com perdas

Sacrifica detalhes que a visão humana não percebe (ou percebe com dificuldade). O nível de perda é definido durante a compressão: quanto maior a perda, maior a compressão.

### Princípio do domínio do espectro

Divide em blocos, transforma, trunca.

JPEG - atualmente a técnica mais usada.

> [!note] Complemento
> **Pipeline completo do JPEG:**
> 1. Conversão **RGB → YCbCr** (Y = luminância; Cb/Cr = crominância azul/vermelha — ver [[Modelos de Cor]]) — o olho é mais sensível a variações de brilho do que de cor.
> 2. **Subamostragem de cor:** 4:4:4 (sem subamostragem), 4:2:2 (Cb/Cr com metade da resolução horizontal), 4:2:0 (metade horizontal e vertical — um valor de cor por bloco 2×2).
> 3. Divisão em **blocos 8×8**; pixels (0–255) centralizados para −128 a +127.
> 4. **DCT 2D** em cada bloco → 64 coeficientes. O **DC** (posição [0,0]) é a média do bloco; os **AC** são as variações. A energia fica concentrada nas baixas frequências (canto superior esquerdo).
> 5. **Quantização:** cada coeficiente é dividido pelo valor da tabela de quantização e arredondado. Os valores da tabela são maiores nas altas frequências, então esses coeficientes viram zero. **Etapa irreversível — é aqui que a perda acontece.** O fator de qualidade (1–100) escala a tabela: perto de 100 = menos perda, arquivo maior.
> 6. **Zig-zag:** lê o bloco em zigue-zague, jogando os zeros das altas frequências para o final da sequência.
> 7. **Codificação por entropia:** RLE (nas sequências de zeros) + Huffman.
>
> **Descompressão** é o caminho inverso: Huffman → dequantização → DCT inversa → junta os blocos → YCbCr → RGB. Com taxa alta aparece o **efeito de blocos** (bordas dos 8×8 visíveis).
>
> **Qual formato usar?**
>
> | Formato | Compressão | Bom para |
> |---|---|---|
> | GIF | Sem perdas (LZW), máx. 256 cores | Ícones, animações simples |
> | PNG | Sem perdas (DEFLATE), transparência (canal alfa) | Gráficos, prints de tela, logos |
> | JPEG | Com perdas (DCT) | Fotografias |
> | JPEG 2000 | Com ou sem perdas (wavelets) | Imagens médicas, cinema digital |

### Wavelets

JPEG 2000: troca a DCT em blocos 8×8 do JPEG por uma transformada wavelet (DWT) aplicada na imagem inteira.

Usado normalmente para imagens mais pesadas.

> [!note] Complemento
> Como não divide em blocos, o JPEG 2000 não tem o efeito de blocos do JPEG em taxas altas (a imagem fica borrada, não "quadriculada"). Também suporta compressão sem perdas e decodificação progressiva (a imagem vai ganhando resolução conforme carrega).

### Compressão por Fractais

Descrevem matematicamente imagens complexas - como as encontradas na natureza.

Em vez de gerar imagens a partir de parâmetros, descobrir os parâmetros e reconstruir.

> [!note] Complemento
> Explora a **autossimilaridade**: partes da imagem parecem versões transformadas (reduzidas, giradas) de outras partes. A imagem é guardada como um conjunto de transformações (IFS). A compressão é muito lenta (achar as transformações), mas a descompressão é rápida e independente de resolução.

---
## Compressão de vídeo

> [!note] Complemento
> Vídeo tem dois tipos de redundância:
> - **Espacial** (dentro de um quadro): mesma técnica do JPEG — blocos 8×8, DCT, quantização, zig-zag + RLE + Huffman.
> - **Temporal** (entre quadros consecutivos): pouca coisa muda de um quadro para o outro, então só se codifica a **diferença** em relação a um quadro de referência, usando **vetores de movimento** entre macroblocos (compensação de movimento).
>
> **Tipos de quadro (MPEG):**
> - **I (Intra-coded):** independente, só redundância espacial (como um JPEG). Maior; serve de âncora e permite acesso aleatório.
> - **P (Predicted):** predito a partir do I ou P anterior. Bem menor que o I.
> - **B (Bidirectional):** predito a partir de quadros anteriores **e** posteriores. Maior compressão, mas exige reordenar os quadros na decodificação.
>
> **GOP (Group of Pictures):** sequência cíclica que sempre começa com um I.
> - **N** = tamanho do GOP (distância entre dois I).
> - **M** = distância entre quadros de referência (I/P → próximo P).
> - Ex.: N = 12, M = 3 → `I B B P B B P B B P B B`
