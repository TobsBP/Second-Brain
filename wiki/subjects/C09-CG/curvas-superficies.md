---
type: subject-concept
subject: C09
tags: [curves, surfaces]
updated: 2026-07-21
---

# Curvas e Superfícies

## Hermite vs. Bézier
- **Hermite:** definida por dois pontos extremos + dois vetores tangentes nesses extremos. O designer controla diretamente a direção da curva nas pontas.
- **Bézier:** definida por pontos de controle (incluindo os extremos). Só os pontos extremos ficam sobre a curva; os intermediários "puxam" a curva como ímãs, sem tangentes explícitas como entrada.

Mover um ponto de controle intermediário puxa suavemente a curva na direção dele, alterando o trecho próximo sem criar descontinuidades abruptas — a influência é gradual e proporcional à distância.

## B-splines na indústria
**Aplicação:** design de carrocerias de automóveis (CAD/CAM). São preferidas a malhas poligonais porque garantem continuidade suave entre segmentos, sem arestas visíveis. Uma malha poligonal precisaria de um número absurdo de polígonos para atingir a mesma suavidade; a B-spline é matematicamente contínua e pode ser avaliada em qualquer resolução sem perder qualidade.

## Sources
- [[raw/subjects/C09-CG/Atividade/Lista Reposição 01]] — questão 2
