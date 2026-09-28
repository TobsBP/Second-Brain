**2 centros de distribuição.** O método do cotovelo mostra uma queda enorme na inércia ao passar de k=1 para k=2, e a partir de k=3 a curva praticamente não melhora mais. Isso indica que os clientes se dividem naturalmente em 2 grupos, tornando 2 centros de distribuição a escolha ideal para minimizar a distância total de transporte.

> [!note] Complemento
> **K-means:** algoritmo de agrupamento (clustering) **não supervisionado** — não tem rótulos. Divide os dados em $k$ grupos, cada um representado pelo seu **centróide** (aqui, a localização de cada centro de distribuição).
>
> **Passo a passo:**
> 1. Escolher $k$.
> 2. Inicializar $k$ centróides (pontos aleatórios dos dados, ou k-means++, que espalha melhor os iniciais).
> 3. Atribuir cada ponto ao centróide mais próximo (distância euclidiana).
> 4. Recalcular cada centróide como a **média** dos pontos do seu grupo.
> 5. Repetir 3 e 4 até os centróides pararem de mudar (convergiu).
>
> **Inércia (WCSS):** soma das distâncias ao quadrado de cada ponto até o seu centróide. É o que o k-means minimiza.
>
> **Escolha de k — método do cotovelo:** roda o k-means para vários $k$ e plota inércia × $k$. A inércia **sempre** cai quando $k$ aumenta (com $k$ = nº de pontos ela chega a 0), então não se escolhe o menor valor, e sim o "cotovelo": o ponto a partir do qual aumentar $k$ quase não reduz mais a inércia.
>
> **Cuidados:**
> - Resultado depende da inicialização → rodar várias vezes e ficar com a menor inércia.
> - Sensível à escala dos atributos → normalizar antes.
> - Funciona melhor com grupos "redondos" e de tamanhos parecidos; é sensível a outliers.
