Atuam diretamente sobre os valores dos pixels, sem transformar as imagens para outro domínio. Cada pixel de saída depende exclusivamente dos pixels correspondentes nas imagens de entrada.

### Principais principios

- Redução de Ruído
- Detecção de Movimento
- Seleção de Regiões 

## Operação aritméticas

Opera pixel por pixel. 

### Adição

Soma os valores literais. Na adição pode ocorrer o overflow, 128 + 192 = 320, branco literalmente.

### Subtração

Pode ocorrer o underflow, preto literalmente. 

### Multiplicação

Uma multiplicação literal, 2 x 90 = 180

### Divisão

Não pode ocorrer divisão por zero, pode ocorrer mas quebra o sistema.

### Blending (mistura controlada)

Variante melhorada da soma, cada imagem contribui com uma fração controlada pelo coeficiente X.

## Operações lógicas

Aplicam funções boolean (AND, XOR, OR, NOT) bit a bit sobre a representação binária dos pixels.

Imagens Binárias:

	1: objeto
	0: fundo

