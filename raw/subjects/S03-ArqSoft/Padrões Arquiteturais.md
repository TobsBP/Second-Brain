Ditam como organizar a estrutura de um projeto de software. Definem responsabilidades para cada componente e como os componentes se comunicam. Padrão Arquitetural não é um framework.
### Model View Controller (MVC)
Feito pensado para aplicações web. A princípio as informações seriam apenas textos. A www conseguiu ser viabilizada por meio de outras tecnologias.
- HTTP: protocolo da internet baseado em hipertextos.

	Logica de apresentação = View
	Lógica de negócio = Model
	Orquestrador = Controller

View realiza requisições ao Controller que invoca métodos do Model então o controller manda o resultado para o view.

### Single Page Application (SPA)
Recebe primeiramente do servidor módulos de JavaScript, depois faz requisições ao servidor para obter json. Exemplo: é como baixar um aplicativo do celular, depois ele faz requisições ao servidor. Então se meu projeto apenas consulta e não tem regras de negócio, pode ser um SPA.

### Message Oriented Middleware (MOM)
Modelo de comunicação assíncrona. Mensagens são enviadas e recebidas por meio de um broker. Soluções como Kafka, RabbitMQ, etc.

### Service Oriented Architecture (SOA)
São os microserviços que se comunicam por meio de APIs. São usados Spring Boot, Rest. Funciona por meio de requisições e respostas, com códigos de status e conteúdo. 