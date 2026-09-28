## Grupo 1 - Drones (simulador de trânsito)
Trânsito de drones. Deu pra ver claramente as race conditions dos drones.

**Ponto positivo:** Muito legal como as colisões foram feitas, apresentação bem didática.

**Ponto negativo:** Falaram um pouco baixo, às vezes era difícil de ouvir.

## Grupo 2 - Ledger no Postgres
Tem uma thread para ler o saldo. Decide na aplicação. Grava o valor. No exemplo dado, 2 threads tentaram sacar 80 reais de um saldo de 100. Ou seja, o que aconteceu foi que o saldo final foi de 20 reais. No modo sequencial não ocorrem perdas. Caso heisenbug: com os logs os erros anteriores podem mudar por conta das threads e decisões de cpu. 

**Ponto positivo:** Ótima ligação entre threads e mercado financeiro, o caso do heisenbug foi o melhor da apresentação.

**Ponto negativo:** Não vi ponto negativo relevante.

## Grupo 3 - Quebrador de senha

Aplicaram o conceito de threads com fila dinâmica e estática. Falaram do conceito de bandeira, uma memória compartilhada entre as threads. Com threads uma senha simples foi achada em apenas 20 segundos.

**Ponto positivo:** Gráficos e indicadores bem feitos, ajudaram muito a ver os resultados.

**Ponto negativo:** Poderiam testar outros algoritmos pra comparar.

## Grupo 4 - Civilizações
Cada quadrado é uma thread, e mostraram bem como os workers ajudam no desempenho. Tem a mina no meio que é um local de disputa de recursos. 

**Ponto positivo:** Ideia muito boa, os gráficos em tempo real mostraram bem o paralelismo.

**Ponto negativo:** A fonte combinava com o tema mas no telão ficou ruim de ler.

## Grupo 5 - Photo thread
Ideia: renderizar uma imagem com threads e sem. É notável o ganho de desempenho com threads para renderizar a imagem. 

**Ponto positivo:** Editor bem intuitivo e a explicação de como cada filtro usa thread foi boa.

**Ponto negativo:** Faltou um feedback visual mostrando o que as threads estão fazendo em segundo plano.

## Grupo 7 - Treinamento de machine learning
Asteroid Dataset usado. Medida de colisão com a órbita da Terra (regressão).
Comparação entre modelos sequencial e paralelo. O treinamento com modelo sequencial aparenta ter o mesmo efeito com e sem threads. Comentaram do soft time limit: o modelo ainda é treinado para finalizar a etapa na qual estava. Em paralelo se tem um treinamento mais rápido que sequencial. A maior diferença é a quantidade de modelos treinados, threads demora mais mas se obtém mais modelos treinados. Modelos treinados no próprio pc.

**Ponto positivo:** Legal a comparação entre sequencial e paralelo, e a explicação do soft time limit ajudou a entender os resultados.

**Ponto negativo:** Os slides ficaram meio bagunçados, dava pra organizar melhor.

## Grupo 8 - Corrida de F1
Cada carro é uma thread. Vários eventos aleatórios afetam o resultado da corrida. Quantidade de threads rodadas é superior a do cpu. 

**Ponto positivo:** Ideia dos pitstops e eventos aleatórios deixou a simulação bem mais interessante.

**Ponto negativo:** Uma visualização melhor da corrida ajudaria a acompanhar os resultados.

## Grupo 9 - Fila para restaurante
Tem pedidos, e tem as threads que seriam os chefes que fazem o pedido. Deu pra ver bem claro como a quantidade de threads influencia.

**Ponto positivo:** Exemplo fácil de entender, os chefes como threads deixou bem claro o conceito.

**Ponto negativo:** Poderiam ter mostrado mais números ou gráficos pra comparar os resultados.

## Grupo 10 - Estacionamento de carros
Cada carro é uma thread, tempo aleatório para chegar no estacionamento. As threads disputam vagas (recursos), com o aumento de threads acaba ocorrendo mais colisões por conta da disputa.

**Ponto positivo:** Os gráficos mostraram bem a disputa pelas vagas e o código parecia bem organizado.

**Ponto negativo:** Não vi nenhum ponto negativo relevante.

## Grupo 11 - Gestor segundo plano (Steam)
Visualizador da Steam. Mais avisos de promoção, DLCs, lista de desejos... Uma thread para comparar bibliotecas entre amigos e retorna em comum. Uma thread busca as DLCs dos jogos junto da última vez que jogou. Em paralelo reduziu o tempo em 44% um ótimo ganho. 

**Ponto positivo:** Tema bem escolhido e o ganho de 44% em paralelo foi bem mostrado.

**Ponto negativo:** Faltou explicar mais como as threads foram usadas e a parte visual podia ser melhor. Dava pra usar cache também.

## Grupo 12 - Simulador de pedra papel tesoura (Battle Royale)
Cada mesa é um jogador. Então o exemplo apresentado foi 4 threads com 60 jogadores. Dá pra ver o tempo e as vitórias. Dá pra ver o ganho com threads, absurdamente de 50s -> 6s. Até 16 threads foi uma melhoria, depois já não tem muito efeito a quantidade de threads.

**Ponto positivo:** Usar jogo pra explicar threads foi uma ótima ideia, e o relatório de resultados ficou bem completo.

**Ponto negativo:** Poderiam mostrar melhor visualmente como as threads estão rodando.

## Grupo 13 - Simuladores de IOT
Sensores são threads produtoras, são threads diferentes. Tem um servidor importante que recebe e processa as leituras em uma fila. Nesse projeto deu para ver o gráfico e o ponto perfeito para a quantidade de threads: 5 threads, depois dessa quantidade não se tem mais ganho de tempo.

**Ponto positivo:** Os diagramas deixaram tudo bem fácil de entender e o gráfico mostrou bem o ponto ótimo de threads.

**Ponto negativo:** O design da apresentação podia ser melhorado.

## Grupo 14 - Smart City Monitor
Verifica a qualidade do ar, energia, as threads geram aleatoriamente eventos.

Mostraram bem a diferença entre a quantidade de eventos por threads e latência. 
A fila de eventos foi bem pequena, talvez maior quantidade daria para ver mais a diferença na curva de tempo médio de resposta. 

**Ponto positivo:** Tema bem atual e as opções de simulação ficaram legais. Deu pra entender bem como as threads funcionam no sistema.

**Ponto negativo:** Poderiam ter mostrado mais o código. Com poucos eventos os gráficos não ficaram tão confiáveis.

## Grupo 15 - Bebês e cuidadoras
Cuidadora é a thread consumidora. Tudo foi feito em Python. Boa explicação da race condition. Interessante os eventos dos bebês. Uma métrica que foi definida: uma quantidade de 120 cuidadoras para algo próximo de 500 bebês. 

**Ponto positivo:** Tema relevante e a explicação da race condition foi boa.

**Ponto negativo:** Ficou tudo muito no terminal, faltou algum gráfico ou algo mais visual.

## Grupo 16 - Gerenciador de downloads

Projeto para ter um download mais eficiente. Deu pra ver claramente como é mais eficiente o download com threads do que sem em arquivos grandes. Em arquivos pequenos não é notável a diferença. Mas sempre tem um limite, como dá pra ver no gráfico deles, a partir de 8 threads a eficiência já cai.

**Ponto positivo:** Deu pra ver bem a diferença em arquivos grandes e o limite de 8 threads no gráfico.

**Ponto negativo:** Premissa um pouco simples, poderiam ter testado mais variações.

## Grupo 17 - Palavras e Threads
Ideia de trabalhar threads com palavras, design bem diferente do que os outros grupos fizeram.

**Ponto positivo:** Proposta criativa e bem visual.

**Ponto negativo:** O volume estava muito baixo, ficou difícil entender o que estavam falando.

## Grupo 18 - Monitoramento de Câmeras com threads

Cada câmera é uma thread, a quantidade de fps é muito maior com threads do que sequencial.

**Ponto positivo:** Tema muito legal e os insights sobre fps foram bem interessantes.

**Ponto negativo:** Os gráficos na tela podiam estar mais bem formatados.

## Grupo 19 - Não apresentou
