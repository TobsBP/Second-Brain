---
type: subject-concept
subject: C24
tags: [neural-networks, deep-learning, machine-learning, mlp, backpropagation]
updated: 2026-04-20
---

# Redes Neurais Artificiais (RNAs)

Modelos matemáticos inspirados no funcionamento do cérebro animal. São neurônios (nós) interconectados e organizados em camadas que, a partir de dados, aprendem uma função que relaciona entradas a saídas. Subárea do aprendizado de máquina que engloba *deep learning*. Hoje estão por trás de ChatGPT, recomendações (Netflix, Spotify) e tradução automática (Google Tradutor).

## Analogia biológica

Neurônio biológico tem três partes:
- **Dendritos** — recebem estímulos de outros neurônios e levam ao corpo celular.
- **Corpo celular (soma)** — integra os sinais.
- **Axônio** — transmite o sinal de saída.

**Sinapses** são os pontos de contato entre dendritos e terminais de axônio. Podem ser **fortes** ou **fracas** — uma sinapse fraca indica que a informação não é tão importante para a ativação.

## Neurônio artificial

O neurônio artificial se assemelha ao **regressor logístico**.

- **Atributos** — os estímulos de entrada.
- **Pesos sinápticos (w)** — dão a força de cada conexão e indicam se ela é excitatória ou inibitória.
- **Peso de bias (b)** — permite ajustar o limiar de ativação.

## Arquitetura

- Neurônios organizados em camadas: entrada, ocultas, saída.
- **Deep Neural Networks (DNNs)** — redes com duas ou mais camadas ocultas. Têm grande capacidade de aprendizado, mas exigem mais dados de entrada.
- Consegue aproximar funções altamente não lineares *e* lineares; a capacidade depende da arquitetura.
- A quantidade de pesos está associada aos **graus de liberdade** da rede.

## Treinamento

O **objetivo** é encontrar os valores ideais dos pesos. Em problemas supervisionados, isso equivale a **minimizar uma função de erro**. O algoritmo se baseia no **gradiente descendente** e tem três passos principais:

1. Aplica entradas e a rede faz predições com os pesos atuais (*forward pass*).
2. Calcula o erro entre a saída da rede e os rótulos.
3. Propaga o erro para trás (*backpropagation*).

### Backpropagation

Para atualizar o peso de um nó de uma camada, a retropropagação **calcula a derivada parcial do erro em relação ao peso**. A função de erro típica é o **erro quadrático médio (MSE)**. Os pesos sinápticos e de bias são ajustados para reduzir esse erro.

## Conexões

- **Regressor logístico** — o neurônio individual é análogo a ele.
- **Gradiente descendente** — algoritmo base do treinamento.
- Tópico fundamental para *deep learning* e modelos modernos (LLMs, recomendação, tradução).

## Sources
- [[raw/subjects/C24-AI/MPL - Neural Network.md]] — introdução a RNAs, neurônio biológico vs. artificial, treinamento e backpropagation.
