---
type: subject-concept
subject: C09
tags: [multimedia-data, image-processing-pipeline]
updated: 2026-07-21
---

# Dados Multimídia

## Sistema de Informação Multimídia
Sistema (hardware + software) projetado para lidar com dados que combinam diferentes tipos de mídia:
- **Aquisição e armazenamento:** captura a partir de câmeras, microfones, scanners, sensores.
- **Indexação e pesquisa:** localização e recuperação por conteúdo.
- **Manipulação:** edição e aprimoramento (brilho, mixagem, efeitos).
- **Distribuição:** streaming ou reprodução local.
- **Proteção:** controle de acesso, criptografia.

## Dados estáticos vs. dinâmicos
- **Estáticos (independentes do tempo):** o significado não depende do instante de exibição. Ex.: texto, imagem.
- **Dinâmicos (dependentes do tempo):** precisam de uma sequência temporal definida; mudar o tempo altera o significado. Ex.: áudio, vídeo, animação.

## Hierarquia narrativa do vídeo
Do menor para o maior:
1. **Quadro (frame):** uma única imagem estática.
2. **Tomada (shot/take):** sequência contínua de quadros, capturada sem corte de câmera.
3. **Cena:** conjunto de tomadas num mesmo local/contexto narrativo.
4. **Sequência:** conjunto de cenas que formam uma unidade dramática completa.

## Pipeline de processamento de imagem
1. **Pré-processamento:** reduz ruídos e distorções, tornando a imagem mais legível para as etapas seguintes.
2. **Segmentação:** divide a imagem em regiões relevantes, isolando o que deve ser classificado. **Etapa crítica** — todas as etapas seguintes dependem dela; uma segmentação ruim não pode ser compensada depois.
   - **Supersegmentação:** o objeto é dividido em fragmentos demais.
   - **Subsegmentação:** objetos distintos são agrupados como um só (ou parte do fundo entra no objeto).
3. **Extração de características:** obtém descritores (forma, cor, textura) da região segmentada.
4. **Classificação:** atribui um rótulo/categoria ao objeto com base nas características extraídas.

## Sources
- [[raw/subjects/C09-CG/Dados Multimídia.md]]
- [[raw/subjects/C09-CG/Atividade/Lista 3]] — questão 6 (pipeline de processamento de imagem)
- [[raw/subjects/C09-CG/Atividade/Lista de Reposição 02]] — questões 2 e 3 (dados multimídia, pipeline)
