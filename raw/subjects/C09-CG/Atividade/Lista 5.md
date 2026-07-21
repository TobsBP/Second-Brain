## Questão 1 — Multimídia e Hipermídia

**Multimídia linear:** o conteúdo segue uma **sequência fixa**, sem interferência do usuário. O controle é mínimo (no máximo play/pause). Ex.: **filme no cinema**, novela na TV.

**Multimídia não linear:** o usuário **escolhe o caminho** e a ordem do conteúdo, com alto nível de interação. Ex.: **videogame**, curso de e-learning, app de streaming.

**Hipermídia:** é a junção de **multimídia + hipertexto** — conteúdo multimídia conectado por **hiperlinks**, permitindo navegação não sequencial entre mídias diferentes (texto, imagem, vídeo, áudio).

**Por que a Web é o exemplo clássico:** porque é um sistema global onde **páginas com diferentes tipos de mídia** são interligadas por **links** que o usuário acessa livremente, criando seu próprio percurso de navegação — exatamente a definição de hipermídia em larga escala.

---

## Questão 2 — Dados Multimídia

**Dados estáticos (independentes do tempo):** seu significado **não depende do instante** em que são exibidos. Ex.: **texto**, **imagem**, gráficos.

**Dados dinâmicos (dependentes do tempo):** precisam ser apresentados em uma **sequência temporal** definida; mudar o tempo altera o significado. Ex.: **áudio**, **vídeo**, animação.

**Hierarquia narrativa do vídeo** (do menor para o maior):

- **Quadro (frame):** uma única imagem estática do vídeo.
- **Tomada (shot/take):** sequência contínua de quadros capturada **sem corte de câmera**.
- **Cena:** conjunto de tomadas em um **mesmo local/contexto** narrativo.
- **Sequência:** conjunto de cenas que formam uma **unidade dramática completa** dentro da narrativa.

---

## Questão 3 — Processamento de Imagens

**Pipeline clássico:**

1. **Pré-processamento:** melhora a qualidade da imagem (remove ruído, ajusta brilho/contraste, normaliza iluminação).
2. **Segmentação:** separa os **objetos de interesse** do fundo, delimitando regiões.
3. **Extração de características:** obtém descritores numéricos das regiões segmentadas (cor, forma, textura, bordas).
4. **Classificação:** atribui um **rótulo/categoria** ao objeto com base nas características extraídas.

**Por que a segmentação é crítica:** todas as etapas seguintes dependem dela. Se o objeto não for bem isolado, características erradas serão extraídas e o classificador errará — não há como compensar uma segmentação ruim depois.

- **Supersegmentação:** o objeto é dividido em **fragmentos demais**, sendo tratado como vários objetos diferentes.
- **Subsegmentação:** **objetos distintos são agrupados** como se fossem um só, ou parte do fundo é incluída no objeto.

---

## Questão 4 — Operações no Domínio do Espaço

São operações aplicadas **diretamente sobre os pixels** da imagem (pontuais, sem transformada). Exigem **mesmo tamanho** porque atuam **pixel a pixel, posição a posição** — se as dimensões diferirem, alguns pixels não terão correspondente para combinar.

**Operação aritmética — subtração para detecção de movimento:** filmando uma cena com câmera fixa, subtrai-se o quadro atual do anterior (`R = Atual − Anterior`). Pixels iguais resultam em zero (fundo), e pixels diferentes evidenciam **áreas onde houve movimento**. Muito usado em câmeras de vigilância.

**Operação lógica — AND como máscara:** cria-se uma imagem binária (máscara) com 1 na região de interesse e 0 no resto. Aplicando `R = Imagem AND Máscara`, **apenas a região desejada é preservada**, zerando o restante. Útil para isolar um rosto, um órgão em exame médico, etc.

---

## Questão 5 — Domínio da Frequência

**Pipeline (3 passos):**

1. **Transformada (DFT/FFT):** converte a imagem do domínio do espaço para o **domínio da frequência**.
2. **Filtragem:** **multiplica** o espectro pelo filtro desejado, atenuando ou removendo certas frequências.
3. **Transformada inversa (IDFT):** retorna ao domínio do espaço, obtendo a imagem filtrada.

**Comparação dos filtros:**

||Passa-baixa|Passa-alta|
|---|---|---|
|Preserva|Frequências baixas (regiões suaves)|Frequências altas (bordas, detalhes)|
|Remove|Frequências altas (detalhes, ruído)|Frequências baixas (áreas uniformes)|
|Efeito visual|Imagem **borrada/suavizada**|Imagem com **bordas realçadas**, fundo escuro|

---

## Questão 6 — Compressão de Imagens

**Sem perdas (lossless):** a imagem original é **100% recuperada** após a descompressão. Aplicação: **imagens médicas, documentos, imagens de satélite, arquivos técnicos** — onde nenhum detalhe pode ser perdido. Ex.: PNG, GIF.

**Com perdas (lossy):** descarta informações pouco perceptíveis para reduzir bastante o tamanho. Aplicação: **fotos para web, redes sociais, streaming** — onde a economia de espaço vale mais que a fidelidade absoluta. Ex.: JPEG.

**Pipeline JPEG:**

1. Conversão RGB → **YCbCr**.
2. **Subamostragem de cor** (ex.: 4:2:0).
3. Divisão em **blocos 8x8**.
4. **DCT** (Transformada Discreta do Cosseno) em cada bloco.
5. **Quantização** dos coeficientes.
6. **Travessia em zig-zag**.
7. Codificação por entropia (**RLE + Huffman**).

**Perda irreversível: na quantização.** Cada coeficiente da DCT é dividido por um valor da tabela e **arredondado**. Como arredondamento é uma operação **não inversível**, os valores originais nunca podem ser recuperados — daí o caráter "lossy" do JPEG. (A subamostragem também descarta dados, mas a quantização é a etapa central de perda.)

---

## Questão 7 — Áudio

**Digitalização (2 etapas):**

- **Amostragem:** discretiza o sinal **no tempo**, capturando valores em intervalos regulares (taxa em Hz). Define quantas amostras por segundo são tomadas.
- **Quantização:** discretiza o sinal **em amplitude**, aproximando cada valor amostrado para o nível mais próximo dentro de uma escala finita. Define quantos bits representam cada amostra.

**Teorema de Nyquist-Shannon:** para reconstruir fielmente um sinal contínuo a partir de suas amostras, a **taxa de amostragem** deve ser **≥ 2× a maior frequência** presente no sinal (`fs ≥ 2·fmax`). Caso contrário, ocorre **aliasing**.

**Ruído de quantização:** é o **erro entre o valor real** do sinal e o valor aproximado pelo nível mais próximo. Quanto **menor a profundidade de bits**, maiores os saltos entre níveis e mais audível o ruído (chiado, distorção). Quanto **mais bits** (ex.: de 8 para 16 ou 24), menores os saltos, e o ruído fica abaixo do limiar de percepção.

---

## Questão 8 — Vídeo Analógico

**Varredura entrelaçada:** cada quadro é dividido em **dois campos** — um com as **linhas ímpares** e outro com as **linhas pares**, exibidos **alternadamente**. Em vez de 30 quadros completos/s, exibe 60 campos/s.

**Problema que resolveu:** a **cintilação (flicker)** dos primeiros sistemas de TV. Mostrar 60 campos/s engana o olho fazendo parecer uma atualização rápida, **sem aumentar a banda de transmissão** (cada campo tem metade das linhas, então a soma equivale ao quadro inteiro).

**Color burst:** é uma **referência de fase** transmitida no início de cada linha (cerca de 8–10 ciclos da subportadora de cor). Serve para o **oscilador local do receptor sincronizar-se** com a subportadora do transmissor, permitindo decodificar corretamente **matiz (fase) e saturação (amplitude)** da crominância.

**Sem o color burst:** o receptor perderia a referência de fase e as cores apareceriam **erradas, instáveis, "escorrendo"** ou simplesmente desapareceriam, mostrando a imagem em **preto e branco**.

---

## Questão 9 — Sistemas de TV em Cores e Transição Digital

|Sistema|Linhas|Taxa|Envio de cor|
|---|---|---|---|
|**NTSC**|525|~30 fps (60 campos/s)|Modulação **AM em quadratura** (QAM) na subportadora; sensível a erros de fase ("Never Twice the Same Color").|
|**PAL**|625|25 fps (50 campos/s)|QAM com **inversão de fase a cada linha**, o que **cancela erros de matiz** automaticamente.|
|**SECAM**|625|25 fps (50 campos/s)|Envia as componentes de cor **em FM** e **alternadas linha a linha** (uma diferença de cor por linha).|

**Transição para a TV digital:** marcou a passagem de sinais analógicos suscetíveis a ruído para um sistema **digital, codificado, comprimido (MPEG)** e modulado em **COFDM**, com vantagens como **alta definição (HD/Full HD)**, melhor qualidade de áudio, **multiprogramação** (vários canais na mesma faixa), **interatividade** e mobilidade.

**Padrão brasileiro — SBTVD:** baseado no **ISDB-Tb** (japonês), com adaptações nacionais:

- Codificação de vídeo em **H.264/AVC** (em vez de MPEG-2 do ISDB original).
- Codificação de áudio em **HE-AAC**.
- **Middleware Ginga** (desenvolvido no Brasil) para aplicações interativas.
- Modulação **BST-OFDM**, permitindo **recepção fixa (HD)** e **móvel/portátil (One-Seg)** simultaneamente na mesma faixa.