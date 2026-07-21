---
type: subject-concept
subject: C24
tags: [clustering, unsupervised-learning]
updated: 2026-07-21
---

# K-Means e Método do Cotovelo

**K-means** é um algoritmo de agrupamento (clustering) não supervisionado que particiona os dados em *k* clusters, cada um representado pelo seu centróide.

**Método do cotovelo (elbow method):** técnica para escolher o número ideal de clusters *k*. Plota-se a inércia (soma das distâncias ao quadrado dos pontos até seu centróide) para diferentes valores de k; o "cotovelo" do gráfico — ponto onde aumentar k deixa de reduzir significativamente a inércia — indica o k ideal.

**Exemplo aplicado (atividade):** problema de definir centros de distribuição para minimizar distância total de transporte até clientes. O método do cotovelo mostrou queda enorme na inércia de k=1 para k=2, e a partir de k=3 a curva praticamente não melhora mais → **2 centros de distribuição** é a escolha ideal.

## Sources
- [[raw/subjects/C24-AI/Atividade k-means.md]]
