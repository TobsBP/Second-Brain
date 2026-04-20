Faz parte dos modelos mais usados nos dias de hoje. ChatGPT, Recomendações (Netflix, Spotify), Google Tradutor

Sub-área do aprendizado de máquina. Engloba o deep learning.

Redes neurais são modelos matemáticos inspirados pelo funcionamento do cérebro dos animais.

**RNAs** são neurônios (nós), interconectados, organizados em camadas, que aprendem, a partir de dados, uma função que relaciona as entradas às saídas.

Neurônio biológico: três partes fundamentais, os dendritos, corpo celular (soma) e o axônio. 

- Dendritos: recebem estímulos vindos de neurônios, e levam ao corpo celular
- Sinapses: são os pontos de contato entre os dendritos de um neurônio e os terminais do axônio de outros neurônios.
- Pode ser forte ou fraca: a informação não é tão importante para a ativação.

O neurônio se assemelha ao **regressor logístico** 

Os estímulos são chamados de atributos. 
Peso de bias (b), permite ajustar o limiar de ativação.
Os pesos sinápticos dão a força de cada conexão e se ela é excitatory ou não.

O **objetivo** do treinamento de uma rede neural é encontrar os valores ideias dos pesos. Ou seja em problemas supervisionados o objetivo é minimizar uma função de erro. 

Duas ou mais camadas ocultas, podem ser chamadas de redes neurais profundas (DNNs).
Tem uma grande capacidade de aprendizado porem precisam de mais dados de entrada.

Consegue aproximar funções altamente não linear e linear. Depende da sua arquitetura, quantidade de pesos está associada aos seus **graus de liberdade**

# Como aprendem?

Aprendem ajustando os pesos sinápticos e de bias para reduzir o erro. Algoritmo com base no gradiente descendente. 

Algoritmo 3 coisas principais:
- Aplica entradas e a rede faz predições com os pesos atuais
- Calcula o erro entre a saída da rede e os rótulos.
- Propaga o erro para trás

Para atualizar o peso de um nó de uma camada, a retropropagação calcula a derivada parcial do erro com base no peso, erro quadrático médio como função de erro.