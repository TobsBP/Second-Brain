Faz parte dos modelos mais usados nos dias de hoje: ChatGPT, Recomendações (Netflix, Spotify), Google Tradutor.

Subárea do aprendizado de máquina. Engloba o deep learning.

Redes neurais são modelos matemáticos inspirados pelo funcionamento do cérebro dos animais.

**RNAs** são neurônios (nós), interconectados, organizados em camadas, que aprendem, a partir de dados, uma função que relaciona as entradas às saídas.

Neurônio biológico: três partes fundamentais, os dendritos, corpo celular (soma) e o axônio. 

- Dendritos: recebem estímulos vindos de neurônios, e levam ao corpo celular
- Corpo celular (soma): integra os estímulos recebidos
- Axônio: transmite o sinal de saída para outros neurônios
- Sinapses: são os pontos de contato entre os dendritos de um neurônio e os terminais do axônio de outros neurônios.
- Pode ser forte ou fraca: numa sinapse fraca, a informação não é tão importante para a ativação.

O neurônio se assemelha ao **regressor logístico** 

Os estímulos são chamados de atributos. 
Peso de bias (b): permite ajustar o limiar de ativação.
Os pesos sinápticos dão a força de cada conexão e se ela é excitatória ou inibitória.

> [!note] Complemento
> **Neurônio artificial (conta):** soma ponderada das entradas + bias, passada por uma função de ativação $f$:
> $$z = \sum_i w_i x_i + b \qquad a = f(z)$$
> Com $f$ = sigmoide, é exatamente um regressor logístico — daí a semelhança.
>
> **Funções de ativação** (introduzem a não linearidade):
> - **Degrau:** saída 0 ou 1 (perceptron original). Não é derivável, então não serve para gradiente descendente.
> - **Sigmoide:** $\sigma(z) = \frac{1}{1+e^{-z}}$, saída em (0, 1). Boa para saída de classificação binária.
> - **Tanh:** saída em (−1, 1), centrada em zero.
> - **ReLU:** $\max(0, z)$. Padrão nas camadas ocultas de redes profundas (barata e sofre menos com gradiente que "some").
> - **Softmax:** na camada de saída para classificação multiclasse (as saídas viram probabilidades que somam 1).
> - **Linear (identidade):** na saída para regressão.
>
> Sem ativação não linear nas camadas ocultas, empilhar camadas não adianta: a rede inteira vira uma função linear.

O **objetivo** do treinamento de uma rede neural é encontrar os valores ideais dos pesos. Ou seja, em problemas supervisionados o objetivo é minimizar uma função de erro. 

Redes com duas ou mais camadas ocultas podem ser chamadas de redes neurais profundas (DNNs).
Têm uma grande capacidade de aprendizado, porém precisam de mais dados de entrada.

Consegue aproximar funções altamente não lineares e lineares. Depende da sua arquitetura; a quantidade de pesos está associada aos seus **graus de liberdade**.

> [!note] Complemento
> **Arquitetura do MLP (Multilayer Perceptron):**
> - **Camada de entrada:** um nó por atributo. Não faz conta, só repassa os valores.
> - **Camada(s) oculta(s):** onde a rede aprende representações intermediárias. Número de camadas e de neurônios são hiperparâmetros.
> - **Camada de saída:** um neurônio por saída (1 para regressão ou classificação binária; 1 por classe com softmax no multiclasse).
>
> É *totalmente conectada* (densa): cada neurônio de uma camada liga em todos da próxima, e cada ligação tem um peso.
>
> **Forward pass:** a entrada passa camada por camada até a saída. Em cada camada $l$:
> $$a^{(l)} = f\left(W^{(l)} a^{(l-1)} + b^{(l)}\right)$$
> A saída da última camada é a predição da rede.

## Como aprendem?

Aprendem ajustando os pesos sinápticos e de bias para reduzir o erro. Algoritmo com base no gradiente descendente. 

Algoritmo tem 3 passos principais:
- Aplica entradas e a rede faz predições com os pesos atuais (forward pass)
- Calcula o erro entre a saída da rede e os rótulos.
- Propaga o erro para trás (backpropagation)

Para atualizar o peso de um nó de uma camada, a retropropagação calcula a derivada parcial do erro em relação ao peso, usando o erro quadrático médio como função de erro.

> [!note] Complemento
> **Regra de atualização (gradiente descendente):**
> $$w \leftarrow w - \eta \, \frac{\partial E}{\partial w}$$
> A backpropagation usa a **regra da cadeia** para calcular $\frac{\partial E}{\partial w}$ de trás para frente, camada por camada. Erro quadrático médio: $E = \frac{1}{n}\sum (y - \hat{y})^2$.
>
> **Taxa de aprendizado ($\eta$):** tamanho do passo na direção contrária ao gradiente.
> - Muito alta: oscila ou diverge, não converge.
> - Muito baixa: converge muito devagar (ou fica presa).
>
> **Época:** uma passada completa por todo o conjunto de treino. O treino roda várias épocas. Os pesos podem ser atualizados a cada exemplo (estocástico), a cada lote (mini-batch) ou uma vez por época (batch).
>
> **Overfitting:** a rede "decora" o treino — erro de treino continua caindo, mas o erro de validação começa a subir. Quanto mais pesos (graus de liberdade), maior o risco. Como combater:
> - mais dados de treino;
> - *early stopping* (parar quando o erro de validação para de cair);
> - regularização (L2, dropout);
> - rede menor.
>
> O oposto é **underfitting**: rede simples demais (ou treinada de menos), erro alto até no treino.
