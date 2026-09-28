Ditam como organizar a estrutura de um projeto de software. Definem responsabilidades para cada componente e como os componentes se comunicam. Padrão arquitetural não é um framework.

> [!note] Complemento
> Padrão arquitetural fica no nível do **sistema inteiro** (quais são os grandes componentes e como conversam). Já os padrões de projeto (GoF) ficam no nível de **classes e objetos**, ver [[Padrões de Projeto]]. Um framework pode *implementar* um padrão (Spring MVC implementa MVC), mas o padrão é a ideia, não a ferramenta.

## Model View Controller (MVC)
Não nasceu na web: foi criado nos anos 70 no Smalltalk (Xerox PARC), para interfaces gráficas de desktop. Depois foi adotado em peso nas aplicações web. A princípio as informações da web seriam apenas textos (hipertexto). A www conseguiu ser viabilizada por meio de outras tecnologias:
- HTTP: protocolo da web (camada de aplicação) para transferência de hipertexto.

Com a web ficando dinâmica, o MVC virou a forma padrão de organizar esse tipo de aplicação no servidor.

- Lógica de apresentação = View
- Lógica de negócio = Model
- Orquestrador = Controller

View realiza requisições ao Controller, que invoca métodos do Model. Então o Controller manda o resultado para a View.

> [!note] Complemento
> - O **Controller** é quem faz a ponte: o Model não conversa direto com a View e pode ser trocado ou testado sem mexer na interface (caiu na [[Practice exam NP2]], questão 1).
> - No MVC original de desktop, a View *observava* o Model (padrão Observer) e se redesenhava sozinha quando ele mudava. Na web, como o HTTP é requisição/resposta, o fluxo virou View → Controller → Model → Controller → View, que é o descrito acima.
> - MVC não é específico de mobile nem de web: é separação de responsabilidades e serve para qualquer aplicação com interface.

## Single Page Application (SPA)
Recebe primeiramente do servidor módulos de JavaScript, depois faz requisições ao servidor para obter JSON. Exemplo: é como baixar um aplicativo do celular, depois ele faz requisições ao servidor. Então, se meu projeto apenas consulta e não tem regras de negócio complexas no front, pode ser um SPA. As regras de negócio continuam no backend, e o SPA só consome a API.

> [!note] Complemento
> - É uma aplicação **web** de página única, não uma extensão do MVC para mobile. Ganha desempenho porque não recarrega a página inteira, só atualiza os componentes que mudaram (Angular, React, Vue).
> - Elementos típicos:
>   - **Componentes:** ligam o *template* (parte visual) com a lógica da tela.
>   - **Módulos:** agrupam componentes por funcionalidade e podem ser reaproveitados em outros projetos.
>   - **Serviços:** fazem a comunicação com sistemas externos (APIs, backends) e são **injetados** nos componentes (injeção de dependência).
> - Regra de negócio complexa não deve ficar nos serviços do front: tudo que roda no navegador pode ser lido e burlado pelo usuário.

## Message Oriented Middleware (MOM)
Modelo de comunicação assíncrona. Mensagens são enviadas e recebidas por meio de um broker. Soluções como Kafka, RabbitMQ, etc.

> [!note] Complemento
> - Diferente do cliente-servidor, quem envia **não espera** resposta: manda para o broker e segue a vida. Emissor e receptor não precisam estar no ar ao mesmo tempo (desacoplamento no tempo) e nem se conhecem (desacoplamento no espaço).
> - Papéis: **publicador** (escreve num tópico), **inscrito** (recebe as mensagens do tópico) e **broker** (componente central que gerencia os tópicos e entrega as mensagens). Uma mesma aplicação pode publicar e se inscrever.
> - Dois modelos:
>   - **Fila (ponto a ponto):** cada mensagem é consumida por **um** consumidor. Serve para distribuir trabalho.
>   - **Publish/Subscribe (tópicos):** cada mensagem vai para **todos** os inscritos do tópico. Serve para avisar vários sistemas de um evento ("pedido criado"). É o Observer levado para o nível de sistemas.
> - Implementações: RabbitMQ, Apache Kafka, ActiveMQ. **MQTT é um protocolo** (implementado por brokers como o Mosquitto), e **Spring Boot não é MOM**: é um framework Java que pode se integrar a esses brokers (caiu na [[Practice exam NP1]], questão 8).

## Service Oriented Architecture (SOA)
Não são exatamente os microsserviços: SOA é a ideia mais antiga de organizar o sistema como **serviços** que se comunicam por meio de contratos padronizados (APIs). Microsserviços são uma evolução/estilo específico dessa ideia (ver abaixo). Na SOA clássica se usava SOAP + WSDL. Spring Boot e REST são o que se usa hoje, principalmente com microsserviços. Com REST sobre HTTP, a comunicação funciona por meio de requisições e respostas, com códigos de status e conteúdo.

> [!note] Complemento
> **SOA x microsserviços**
>
> | | SOA (clássica) | Microsserviços |
> |---|---|---|
> | Época / contexto | anos 2000, integração de sistemas corporativos | ~2014 em diante, cloud e containers |
> | Tamanho do serviço | grande, com funcionalidade de negócio ampla | pequeno, faz uma coisa (um contexto de negócio) |
> | Comunicação | SOAP/WSDL, muitas vezes por um **ESB** (barramento central "inteligente") | REST/HTTP, gRPC ou mensageria. "Endpoints inteligentes, canos burros", sem barramento central |
> | Dados | serviços costumam compartilhar banco | cada serviço tem **seu próprio banco** |
> | Deploy | frequentemente em conjunto | cada serviço sobe **independente** |
>
> - Em comum: sistema dividido em serviços com contrato bem definido. Por isso muita gente chama microsserviços de "SOA feita direito".
> - A padronização **não dispensa documentação**: continua precisando de contrato (WSDL na SOA clássica, OpenAPI/Swagger no REST). Caiu na [[Practice exam NP1]], questão 9.
> - JSON não é obrigatório em REST (XML e outros formatos também valem).
> - Custo dos microsserviços: é um sistema distribuído (rede falha, latência, consistência eventual, observabilidade). Para projeto pequeno, um monolito bem organizado costuma ser melhor.

> [!note] Complemento
> **Outros padrões arquiteturais que valem conhecer** (o cap. 7 de *Engenharia de Software Moderna*, do Marco Tulio Valente, cobre vários deles)
> - **Cliente-servidor:** o cliente faz uma requisição e **espera** a resposta do servidor, que concentra dados e serviços. É a base que MVC web, SPA e SOA estendem, e o contraponto do MOM.
> - **Camadas (layered):** o sistema é dividido em camadas empilhadas, e cada uma só usa a de baixo. Clássico em 3 camadas: **apresentação → negócio (domínio) → dados (persistência)**. Facilita trocar uma camada sem mexer nas outras. O DAO do lab de Neo4j (S02) é justamente a camada de dados.
> - **Pipes and filters:** os dados passam por uma sequência de etapas independentes (filtros) ligadas por canais (pipes), e cada etapa transforma e repassa. Ex.: `cat log | grep ERRO | sort` no terminal, compiladores, pipelines de ETL.
> - **Hexagonal (ports and adapters) / Arquitetura Limpa:** o domínio fica no centro e não depende de nada externo. Banco, web e filas se conectam por *portas* (interfaces) e *adaptadores*. É a inversão de dependência (o D do SOLID, ver [[Princípios de Design]]) aplicada à arquitetura toda.
> - **Anti-padrão Big Ball of Mud:** sistema sem arquitetura reconhecível, em que tudo depende de tudo. É o que acontece quando nenhuma dessas decisões é tomada.
