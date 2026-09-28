Atuam diretamente sobre os valores dos pixels, sem transformar as imagens para outro domínio. Cada pixel de saída depende exclusivamente dos pixels correspondentes nas imagens de entrada.

As imagens precisam ter o **mesmo tamanho**, já que a operação é feita posição a posição.

## Principais princípios

- Redução de Ruído
- Detecção de Movimento
- Seleção de Regiões

## Operações aritméticas

Opera pixel por pixel.

### Adição

Soma os valores literais. Na adição pode ocorrer o overflow: 128 + 192 = 320, que passa de 255 — com saturação (clipping) o resultado vira 255, branco literalmente.

### Subtração

Pode ocorrer o underflow (resultado menor que 0) — com saturação vira 0, preto literalmente. Usada para detecção de movimento: `atual − anterior`, o que não mudou dá 0.

### Multiplicação

Uma multiplicação literal, 2 x 90 = 180. Também pode estourar o 255.

### Divisão

Não pode ocorrer divisão por zero — se ocorrer, quebra o sistema, então tem que tratar (ex.: somar 1 no divisor ou ignorar os pixels com 0).

### Blending (mistura controlada)

Variante melhorada da soma, cada imagem contribui com uma fração controlada pelo coeficiente α:

```
R = α·A + (1 − α)·B      (0 ≤ α ≤ 1)
```

Como as frações somam 1, não dá overflow. Variando α de 0 a 1 ao longo do tempo tem-se um fade de uma imagem para a outra.

> [!note] Complemento
> **Como lidar com overflow/underflow:**
> - **Saturação (clipping):** limita em [0, 255] — o que passa vira 255, o que fica negativo vira 0.
> - **Wrap-around:** é o que acontece sem tratamento num `uint8` (conta módulo 256): 128 + 192 = 320 → **64**, um pixel que devia ser branco fica escuro. Por isso é um erro comum no código.
> - **Normalização:** faz a conta num tipo maior (int16/float) e depois reescala o resultado para [0, 255].
>
> **Redução de ruído por média:** tirando a média de N imagens da mesma cena (ruído aleatório, média zero), o ruído cai — o desvio padrão reduz por um fator √N.

## Operações lógicas

Aplicam funções booleanas (AND, XOR, OR, NOT) bit a bit sobre a representação binária dos pixels.

Imagens Binárias:

- 1: objeto
- 0: fundo

> [!note] Complemento
> | Operação | Resultado com duas máscaras | Uso típico |
> |---|---|---|
> | **AND** | Interseção (só o que é objeto nas duas) | Aplicar máscara: `imagem AND máscara` isola a região de interesse |
> | **OR** | União (objeto em qualquer uma) | Juntar regiões |
> | **XOR** | Diferença (objeto em só uma delas) | Ver o que mudou entre duas máscaras |
> | **NOT** | Inversão (troca objeto e fundo) | Inverter a máscara |
>
> Cuidado com a representação: se o objeto for guardado como 255 (`11111111`) em vez de 1, AND/OR/XOR continuam funcionando, mas misturar as convenções quebra (1 AND 255 = 1). E o NOT bit a bit sobre 1 (`00000001`) dá 254, não 0.
>
> **Além das operações pixel a pixel:** o domínio do espaço também inclui a **filtragem espacial**, em que cada pixel de saída depende de uma **vizinhança** (máscara/kernel, ex.: 3×3) — filtro de média (suaviza), mediana (remove ruído sal e pimenta), Sobel/Laplaciano (bordas). É o equivalente, direto nos pixels, dos filtros da [[Filtragem]] no domínio da frequência.
