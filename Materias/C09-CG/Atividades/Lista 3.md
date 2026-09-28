1. Multimídia é a junção de vários tipos de mídia (texto, imagem, áudio e vídeo) em um único canal de comunicação.
	- Multimídia linear: o conteúdo apresentado em uma sequência contínua, sem interações do usuário. Exemplo: filmes e músicas.
		- Vantagem: narrativa controlada.
		- Desvantagem: ausência de interatividade.
	- Multimídia não linear: o usuário navega livremente pelo conteúdo. Exemplo: sites e jogos.
		- Vantagem: interatividade e personalização.
		- Desvantagem: confusão para o usuário.

2. A hipermídia adiciona navegação por hiperlinks, permitindo que o usuário possa saltar entre diferentes partes do conteúdo.

3. Textos: artigos, legendas. Imagens: fotos, ilustrações.

4. A composição é feita por meio da sincronização temporal e espacial dos elementos, cada mídia é associada a um instante e posição, e um sistema ( player, editor ) renderiza junto no tempo correto.

5. A evolução trouxe maior capacidade de armazenamento MBs -> TBs, velocidade de processamento, compressão de dados.

6. 
  - Pré-processamento: melhora a qualidade da imagem antes da análise. Reduz ruídos e distorções, tornando a imagem mais legível para as seguintes etapas.
  - Segmentação: divide a imagem em regiões relevantes para a análise. Isola o que o sistema deve classificar.
  - Extração de características: obtém descritores relevantes da região segmentada ( forma, cores, texturas ), esses descritores que alimentam o classificador.

7. Adição pode causar o overflow e subtração pode causar o underflow.
  - Overflow: a soma passa de 255, o valor máximo permitido para um pixel.
  - Underflow: a subtração fica menos que 0, o valor mínimo permitido para um pixel.

  Para contornar esses problemas, podemos usar técnicas de clipping (limitar os valores a um intervalo válido como 0-255) e normalização (transformar os valores para um intervalo específico, como 0-1) ou uso de tipos de dados maiores ( como int16 ) para evitar overflow e underflow.

8. Em imagens binárias, operações aritméticas podem usar pixels com valores 0 e 255, mas operações lógicas (AND, OR, NOT, XOR) devem usar apenas 0 e 1, pois trabalham com valores booleanos. Usar 255 pode gerar resultados incorretos, já que ele é interpretado como 11111111 em binário e as operações são feitas bit a bit.

> [!warning] Correção
> Com 0 e 255, AND/OR/XOR bit a bit **funcionam**: 255 é `11111111`, então 255 AND 255 = 255, 255 AND 0 = 0, 255 XOR 255 = 0. O problema real é **misturar convenções** (1 AND 255 = `00000001` = 1, nem objeto "cheio" nem fundo) e o **NOT bit a bit** sobre 0/1 num byte: NOT `00000001` = `11111110` = 254, e NOT 0 = 255. Ou seja: as duas imagens precisam usar a mesma convenção, e com 0/1 o NOT tem que ser lógico (`1 − x` ou tipo booleano), não bit a bit. Já nas aritméticas, 0/255 é que exige cuidado: 255 + 255 estoura.

9. Em imagens binárias para operações lógicas, os valores devem ser 0 ou 1, pois trabalham com valores booleanos. Usar valores diferentes pode gerar resultados incorretos. OR e NOT são operações lógicas que trabalham com valores booleanos, e devem usar apenas 0 e 1.

> [!warning] Correção
> Mesma ressalva da 8: o que importa é a **consistência** da representação. OR com 0/255 funciona (255 OR 0 = 255). O NOT é o caso delicado: bit a bit, ele inverte os 8 bits, então só dá o resultado esperado se o objeto for 255 (NOT 255 = 0, NOT 0 = 255); com 0/1, NOT 1 = 254.

10. Operações lógicas em si não causam overflow ou underflow, pois trabalham com valores booleanos (0 ou 1). O problema surge quando os valores booleanos são combinados em operações lógicas, como AND, OR ou XOR.

> [!warning] Correção
> A segunda frase contradiz a primeira: AND, OR, XOR e NOT **nunca** geram overflow/underflow — o resultado de uma operação bit a bit sempre cabe no mesmo número de bits (0–255 num byte, 0/1 em binário). O problema aparece quando se **mistura** resultado lógico com **aritmética** (ex.: somar duas máscaras 0/255 → 510, ou 1 − 255 < 0) ou quando se aplica NOT bit a bit numa imagem 0/1 (dá 254/255, fora do esperado, mas não é overflow).

11. As baixas frequências representam áreas da imagem com poucas mudanças de cor ou brilho, como superfícies lisas e regiões uniformes. Um filtro passa-alta remove essas informações e mantém apenas as partes com mudanças rápidas, como bordas, contornos e detalhes finos. Como resultado, a imagem destaca os objetos e suas bordas, enquanto o fundo fica menos visível.

12. Passos:
    1. Aplicar a Transformada de Fourier (FFT) na imagem -> converte para o domínio da frequência.
    2. Aplicar o filtro desejado. Exemplo: máscara que zera certas frequências.
    3. Aplicar a Transformada Inversa de Fourier (IFFT) para recuperar a imagem no domínio espacial.

13. Baixas frequências representam as partes da imagem com poucas mudanças de cor ou brilho, um filtro passa-alta as remove e destaca as bordas e detalhes finos.

14. Altas frequências representam as partes da imagem com mudanças rápidas, como bordas e detalhes finos. Um filtro passa-baixa as remove e destaca as áreas com poucas mudanças de cor ou brilho e uma imagem borrada.

15. Compressão sem perdas: exemplo: PNG com codificação deflate (LZ77 + Huffman). Os pixels são codificados de forma que a imagem original possa ser reconstruída bit a bit.

16. Compressão com perdas: exemplo: JPEG a imagem passa por DCT (Discrete Cosine Transform), quantização (onde informações de alta frequência são descartadas) e codificação. A reconstrução é aproximada; perdas são irreversíveis, mas o arquivo fica muito menor.

17. São subdivididas em 3 tipos: 
  - I-Frames (Intra-coded): contêm a imagem completa, sem referências a outros frames, como um JPEG.
  - P-Frames (Predicted): contêm apenas as diferenças em relação ao I-Frame ou ao P-Frame anterior, permitindo reconstrução aproximada.
  - B-Frames (Bidirectional): preditos a partir de frames anteriores, e posteriores oferecendo maior compressão.

18. A imagem é convertida de RGB para YCbCr. Y é a luminância (brilho), Cb é a diferença de azul e Cr é a diferença de vermelho.

19. O olho humano é mais sensível a variações de luminância do que de crominância, então reduzir a resolução das componentes de cor (Cb e Cr) causa pouca perda perceptível, mas reduz bastante o volume de dados.

20. 
  - 4:4:4 — sem subamostragem, todas as componentes têm resolução plena.
  - 4:2:2 — Cb e Cr com metade da resolução horizontal.
  - 4:2:0 — Cb e Cr com metade da resolução horizontal e vertical (um valor de cor para cada bloco 2x2 de pixels).

21. Etapas da DCT (Transformação Discreta de Cosseno) no JPEG:

  1. Divisão em blocos a imagem é dividida em blocos de 8×8 pixels.
  2. Centralização: os valores de pixel (0–255) são deslocados para o intervalo −128 a +127.
  3. Aplicação da DCT 2D: cada bloco é transformado matematicamente, decompondo-o em 64 coeficientes que representam frequências espaciais crescentes. O coeficiente DC (posição [0,0]) representa a média do bloco e os coeficientes AC representam as variações.
  4. Obtenção dos coeficientes: o resultado é uma matriz 8×8 de coeficientes da DCT, com energia concentrada nos coeficientes de baixa frequência (canto superior esquerdo).

22. A remoção de componentes de alta frequência: os coeficientes de alta frequência (canto inferior direito) são removidos por meio da etapa de quantização. Cada coeficiente é dividido pelo valor correspondente na tabela de quantização e arredondado para o valor mais próximo.

23. Após a quantização a maioria dos coeficientes de alta frequência (canto inferior direito) são zero, zig-zag agrupa os zeros no final da sequência, o que favorece a codificação por run-length (longas sequências de zeros são comprimidas eficientemente).

24. A qualidade é ajustada por meio do fator de qualidade (Q), que é um valor entre 1 e 100 que controla a compressão e a perda de informação. Quanto mais próximo de 100, melhor a compressão e menos perda de informação, mas maior o tamanho do arquivo. Quanto mais próximo de 1, pior a compressão e mais perda de informação, mas menor o tamanho do arquivo.

> [!warning] Correção
> "Melhor/pior compressão" está invertido. Perto de 100 = **melhor qualidade**, menos perda, arquivo maior → ou seja, **menos** compressão (taxa de compressão menor). Perto de 1 = **mais** compressão, mais perda, arquivo menor. O Q escala a tabela de quantização: Q baixo → divisores maiores → mais coeficientes viram zero.

25. Não. A quantização é irreversível, ao dividir e arredondar, a informação original é perdida definitivamente. A descompressão reconstrói uma aproximação, não os dados exatos.

26. Passos para descomprimir um arquivo JPEG: decodificação Huffman -> dequantização -> DCT inversa -> reconstrução dos blocos -> conversão de volta para RGB. Sem a tabela de quantização, a descompressão é impossível.

27. A amostragem captura o valor do sinal analógico em instantes regulares de tempo, convertendo-o em uma sequência discreta de amostras que pode ser armazenada digitalmente.

28. O teorema diz que para representar fielmente um sinal com frequência máxima F, a taxa de amostragem deve ser de no mínimo 2F amostras por segundo. Abaixo disso ocorre aliasing (distorções que não existiam no sinal original).

29. Aumentando o número de bits por amostra (mais níveis de quantização), a diferença entre o valor real e o nível mais próximo diminui.

30. Cálculo:
    - Faixa: 20 Hz a 20 kHz → taxa mínima de amostragem = 2 × 20.000 = 40.000 amostras/s
    - 128 níveis → 7 bits por amostra (2⁷ = 128)
    - Taxa mínima = 40.000 × 7 = 280.000 bits/s = 280 kbps

31. Exercício em sala

32. Os sinais de sincronismo em um vídeo são usados para sincronizar o áudio e a imagem, garantindo que ambos estejam alinhados corretamente.

> [!warning] Correção
> No vídeo analógico, os sinais de sincronismo **não** têm a ver com áudio. Eles sincronizam a **varredura do receptor com a do transmissor**: dizem ao aparelho quando terminar uma linha e voltar para o começo da próxima (sincronismo **horizontal**) e quando terminar um campo/quadro e voltar para o topo da tela (sincronismo **vertical**). Sem eles a imagem "rola" na vertical ou fica torta/rasgada na horizontal. São pulsos transmitidos durante o apagamento (blanking), abaixo do nível de preto, então não aparecem na tela.

33. Sincronismos:
    - Sincronismo horizontal: pulso negativo curto (~4,7 µs) a ~15,75 kHz, sinaliza o fim de cada linha.
    - Sincronismo vertical: pulso mais longo a 60 Hz (NTSC) ou 50 Hz (PAL), sinaliza o fim de cada quadro.

> [!warning] Correção
> Detalhe: com varredura entrelaçada, 60 Hz/50 Hz é a taxa de **campos**, não de quadros — o sincronismo vertical marca o fim de cada **campo** (2 campos = 1 quadro, ~30 ou 25 quadros/s). A frequência horizontal é ~15,734 kHz no NTSC (525 linhas × 29,97) e 15,625 kHz no PAL (625 × 25).

34. Tipos de varredura:
    - Varredura progressiva: todas as linhas são varridas sequencialmente, de cima para baixo, a cada ciclo, produz uma imagem mais nítida sem entrelaçamento de linhas. Ex: monitores 1080p
    - Varredura entrelaçada: cada quadro é dividido em dois campos, um com as linhas pares e outro com as linhas ímpares, alternando entre eles a cada ciclo. Reduz a percepção de cintilação com menor largura de banda, porém pode causar artefatos em cenas com movimentos rápidos.

35. Combina luminância (Y) e crominância (C) em frequências diferentes dentro do mesmo canal. Receptores em preto e branco usam só o Y; receptores coloridos decodificam o C também.

36. A amplitude do sinal corresponde ao brilho (luminância), enquanto a fase e a amplitude da subportadora de cor correspondem, respectivamente, à matiz (tonalidade) e à saturação.

37. O color burst é um trecho de referência da subportadora de cor (cerca de 8 a 10 ciclos) transmitido no blanking horizontal. O receptor usa essa referência para sincronizar seu oscilador de cor e decodificar corretamente a fase da crominância, interpretando as cores com precisão.