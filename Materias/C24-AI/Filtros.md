> [!note] Complemento
> *A nota estava vazia; pelo contexto da matéria, suponho que seja sobre filtros de convolução em CNNs. Conferir com o conteúdo da aula.*
>
> **Filtro (kernel):** matriz pequena de pesos (ex.: 3x3) que desliza sobre a imagem. Em cada posição, multiplica elemento a elemento pela região da imagem embaixo dele e soma tudo (+ bias). O resultado de todas as posições é um **mapa de características** (*feature map*).
>
> - Mesma ideia dos filtros espaciais de processamento de imagem (média, Sobel...), mas na CNN os pesos do filtro **não são escolhidos à mão: são aprendidos** no treinamento, por backpropagation, igual aos pesos do [[MLP - Neural Network|MLP]].
> - Numa imagem RGB, o filtro tem a profundidade dos canais (3x3x3). Cada filtro gera um feature map; uma camada com N filtros gera N mapas.
> - Primeiras camadas aprendem padrões simples (bordas, texturas); camadas mais profundas combinam esses padrões em coisas mais complexas (formas, partes de objetos).
> - **Compartilhamento de pesos:** o mesmo filtro é usado na imagem inteira, então são muito menos parâmetros que um MLP denso ligando todos os pixels.
>
> **Hiperparâmetros:**
> - Tamanho do filtro $F$ (3x3, 5x5...)
> - **Stride** $S$: de quantos em quantos pixels o filtro anda.
> - **Padding** $P$: borda de zeros ao redor da imagem para controlar o tamanho da saída.
> - Número de filtros.
>
> **Tamanho da saída:**
> $$\text{saída} = \frac{N - F + 2P}{S} + 1$$
> Ex.: imagem 5x5, filtro 3x3, $P = 0$, $S = 1$ → saída $(5 - 3 + 0)/1 + 1 = 3$, ou seja, 3x3.
>
> **Depois da convolução:** normalmente vem uma ativação (ReLU) e uma camada de **pooling** (ex.: max pooling 2x2, que pega o maior valor de cada bloco e reduz a dimensão pela metade). No fim, os mapas são "achatados" e vão para camadas densas que fazem a classificação.
