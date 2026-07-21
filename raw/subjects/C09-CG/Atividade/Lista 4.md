
**1.** A **varredura entrelaçada** divide cada quadro em dois **campos**: um com as linhas **ímpares** e outro com as **pares**, exibidos alternadamente. Em vez de mostrar 30 quadros completos por segundo, mostra 60 campos/s. Como o olho enxerga a tela sendo "refrescada" 60 vezes por segundo, a sensação de cintilação (flicker) diminui — mas, como cada campo tem só metade das linhas, a **banda transmitida continua a mesma** de uma varredura progressiva a 30 fps.

**2.** O **Teorema de Nyquist** diz que, para digitalizar um sinal de TV sem perdas, a **taxa de amostragem** deve ser **pelo menos o dobro da maior frequência** presente no sinal (fs ≥ 2·fmax).

- **Aliasing:** ocorre quando essa condição não é respeitada — frequências altas são interpretadas como falsas frequências baixas, causando distorções (ex.: padrões "moiré" em listras finas, efeito de "roda girando ao contrário").
- **Como evitar:** aplicar um **filtro passa-baixa (anti-aliasing)** antes da amostragem para limitar o sinal a fmax, ou aumentar a taxa de amostragem.

**3.** O **efeito penhasco (cliff effect)** descreve o comportamento abrupto da TV digital diante da degradação do sinal:

- Enquanto a relação sinal-ruído estiver acima de um limiar, a imagem aparece **perfeita**.
- Logo abaixo desse limiar, a imagem **desaparece de uma vez** (congelamentos, blocos, tela preta).

Já a **TV analógica degrada gradualmente**: conforme o sinal piora, surgem chuviscos, fantasmas e perda de cor, mas a imagem continua sendo assistível. Ou seja, a digital é "tudo ou nada"; a analógica é uma queda suave.

**4.** Porque existe **um transmissor para milhões de receptores**. O custo do transmissor é diluído entre toda a audiência, então pode ser caro e sofisticado. Já cada receptor é comprado individualmente pelo telespectador — qualquer aumento de custo se multiplica por milhões de unidades e **inviabiliza a adoção em massa** do padrão. Por isso o projeto do receptor precisa ser barato, simples e robusto.

**5.** A etapa de **modulação/demodulação (modem RF)**:

- No **transmissor**: o **modulador** (após o D/A) converte os dados digitais em um sinal analógico de RF pronto para irradiar (ex.: modulação OFDM, COFDM, 8-VSB).
- No **receptor**: o **demodulador** (seguido do A/D) faz o caminho inverso, recuperando os bits a partir do sinal de RF captado.

**6.** **Redundância temporal** explora a semelhança entre **quadros consecutivos** de um vídeo (em geral, pouca coisa muda de um quadro para o outro). A técnica:

1. Codifica integralmente apenas alguns quadros de referência.
2. Para os demais, em vez de enviar a imagem inteira, envia-se apenas a **diferença** em relação a um quadro de referência (compensação de movimento por **vetores de movimento** entre macroblocos).

Resultado: enorme redução de dados, pois só as mudanças são transmitidas.

**7.** **Redundância espacial** explora a semelhança entre **pixels vizinhos dentro de um mesmo quadro** (regiões com cor/textura parecidas). A técnica é a mesma do JPEG:

1. Divide o quadro em blocos 8x8.
2. Aplica a **DCT** para concentrar energia nas baixas frequências.
3. **Quantiza** os coeficientes (descarta altas frequências, pouco perceptíveis).
4. Aplica **zig-zag + RLE + Huffman** para codificação por entropia.

**8.** Tipos de quadros no **MPEG-2**:

- **I (Intra-coded):** quadro de referência, comprimido apenas com **redundância espacial** (como um JPEG). É independente — serve de "âncora" e permite acesso aleatório ao vídeo. É o maior em tamanho.
- **P (Predicted):** codificado com base em um quadro **I ou P anterior**, usando compensação de movimento. Guarda apenas as diferenças. Bem menor que um I.
- **B (Bidirectional):** codificado com base em quadros **anteriores E posteriores** (predição bidirecional). É o que comprime mais, mas exige reordenação na decodificação.

**Group of Pictures (GOP):** é a estrutura cíclica que define como esses quadros se organizam (ex.: `I B B P B B P B B P B B`). Sempre começa com um quadro I, garantindo um ponto de sincronização e acesso aleatório.

- **N** = tamanho total do GOP (distância entre dois quadros I consecutivos). Ex.: N = 12 → um I a cada 12 quadros.
- **M** = distância entre quadros de referência (entre um I/P e o próximo P). Ex.: M = 3 → a cada 3 quadros há um P, e os 2 quadros entre eles são B.

Exemplo com **N = 12, M = 3**: `I B B P B B P B B P B B` (próximo quadro seria o próximo I, iniciando novo GOP).