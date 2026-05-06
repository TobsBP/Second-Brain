Tipos de compressão de imagens estáticas

---
## Sem perdas 

Mantém toda a informação contida na imagem original

Comum em raio-x

### Codificação de Huffman

Letras que aparecem muito: a, e - códigos mais curtos
Letras raras: z, x - recebem código mais longo

### Run Length Encoding

Eficiente quando há muita redundância na sequência de dados.

	AAAAAA    BBBBBBB
	6A        8B     

Aplicar quando gerada por editores 2D imagens artificiais, pois imagens por foto dificilmente vai ter pixels repetidos.

### Lempel Ziv Welch

Muito usada no formato GIF, limitação de 256 cores.
Tem cor transparente, entrelaçamento e animação.

### DEFLATE - junção de Huffman + LZ77

Também é o método usado na compressão ZIP.

---
## Compressão com perdas

Sacrifica detalhes que a visão humana não percebe (ou com dificuldade). Definido durante a compressão quanto maior a perda maior a compressão.

---
## Princípio domínio do espectro

Divide em blocos, transforma, trunca

JPEG - atualmente a técnica mais usada.

---
### Wavelets

JPEG + Wavelets = JPEG 2000

Usado normalmente para imagens mais pesadas.

### Compressão por Fractais

Descrevem matematicamente imagens complexas - como as encontradas na natureza.

Em vez de gerar imagens a partir de parâmetros, descobrir os parâmetros e reconstruir.
