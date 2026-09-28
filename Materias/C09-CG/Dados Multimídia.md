## Sistema de Informação Multimídia

**Definição**: sistema composto por hardware e software projetado para lidar com dados que combinam diferentes tipos de mídia.

**Aquisição e armazenamento**: captura a partir de câmeras, microfones, scanners, sensores.

**Indexação e pesquisa**: localização e recuperação por conteúdo.

**Manipulação**: edição e aprimoramento, brilho, mixagem, efeitos.

**Distribuição**: streaming, reprodução local.

**Proteção**: controle de acesso, criptografia.

## Tipos de dados Multimídia

### Dados estáticos (independe do tempo)

O significado não depende do instante em que são exibidos.

- Imagens
- Texto

### Dados dinâmicos (depende do tempo)

Precisam ser apresentados numa sequência temporal definida; mudar o tempo muda o significado.

- Áudio
- Vídeo
- Animação

## Vídeo

Hierarquia narrativa, do maior para o menor:

- **Sequência**: série de cenas relacionadas que compõem uma parte significativa da narrativa (uma unidade dramática completa).
- **Cena**: uma ou várias tomadas num mesmo local/contexto narrativo.
- **Tomada (shot/take)**: sequência contínua de quadros capturada sem corte de câmera.
- **Quadro (frame)**: uma única imagem estática do vídeo.

> [!note] Complemento
> Um vídeo digital é uma sequência de quadros exibidos a uma **taxa de quadros** (fps — ex.: 24 no cinema, 30/60 em TV e jogos). O tamanho cru cresce rápido: `largura × altura × bits por pixel × fps`. Ex.: 1920×1080 × 24 bits × 30 fps ≈ 1,5 Gbit/s — por isso vídeo praticamente sempre é comprimido (ver [[Compressão de imagens]]).
>
> Essa hierarquia é o que permite **indexar vídeo por conteúdo**: detectar cortes entre tomadas (mudança brusca entre quadros), agrupar tomadas em cenas e escolher quadros-chave para resumir cada tomada.

## Áudio

A digitalização tem 2 etapas:

- **Amostragem**: discretiza o sinal **no tempo** — captura o valor do sinal em intervalos regulares (taxa em Hz).
- **Quantização**: discretiza **em amplitude** — aproxima cada amostra para o nível mais próximo de uma escala finita (quantidade de bits por amostra).

**Teorema de Nyquist**: para representar fielmente um sinal com frequência máxima F, a taxa de amostragem deve ser de no mínimo 2F. Abaixo disso ocorre **aliasing** (aparecem frequências falsas que não existiam no sinal original).

**Ruído de quantização**: erro entre o valor real e o nível mais próximo. Mais bits por amostra → níveis mais próximos → menos ruído.

> [!note] Complemento
> **Taxa de bits** = taxa de amostragem × bits por amostra × nº de canais.
> - Ex. da lista: 20 Hz–20 kHz com 128 níveis → 40.000 amostras/s × 7 bits = 280 kbps.
> - CD: 44,1 kHz × 16 bits × 2 canais ≈ 1,41 Mbps. Os 44,1 kHz vêm de Nyquist: pouco mais que 2 × 20 kHz (limite da audição humana).
> - Cada bit a mais melhora a relação sinal-ruído de quantização em ≈ 6 dB (16 bits ≈ 96 dB).

## Pipeline de processamento de imagem

1. **Pré-processamento**: melhora a qualidade da imagem (reduz ruído e distorções, ajusta brilho/contraste).
2. **Segmentação**: separa os objetos de interesse do fundo. **Etapa crítica** — todas as seguintes dependem dela; segmentação ruim não tem como ser compensada depois.
   - **Supersegmentação**: o objeto é dividido em fragmentos demais.
   - **Subsegmentação**: objetos distintos são agrupados como um só (ou parte do fundo entra no objeto).
3. **Extração de características**: obtém descritores (forma, cor, textura) da região segmentada.
4. **Classificação**: atribui um rótulo/categoria ao objeto com base nas características.

> [!note] Complemento
> Antes do pré-processamento vem a **aquisição** (captura e digitalização da imagem), e em várias referências (Gonzalez & Woods) a última etapa é **reconhecimento e interpretação** — dar significado ao conjunto de objetos reconhecidos.
