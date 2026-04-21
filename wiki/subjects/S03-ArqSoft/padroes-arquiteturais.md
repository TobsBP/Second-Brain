---
type: subject-concept
subject: S03
tags: [architectural-patterns, np1]
updated: 2026-04-21
---

# Padrões Arquiteturais

Padrões consolidados que resolvem problemas recorrentes no projeto de sistemas.

## MVC (Model-View-Controller)
Padrão **geral de separação de responsabilidades**, muito usado em **web e desktop** (não específico de mobile). Componentes:
- **Model:** representa as entidades do sistema e implementa rotinas de persistência/manipulação de dados.
- **View:** representa como as páginas/telas serão apresentadas ao usuário; pode ter lacunas a serem preenchidas dinamicamente.
- **Controller:** recebe as ações do usuário, coordena Model e View, executa as funcionalidades.

## SPA (Single Page Application)
Tipo de **aplicação web de página única** — não é extensão do MVC focada em mobile. Ganha desempenho por evitar recarregamento completo da página, atualizando apenas os componentes modificados.

Elementos típicos (ex.: Angular, React, Vue):
- **Componentes:** unidades que fazem o fluxo de dados entre *templates* (parte visual) e regras de negócio.
- **Módulos:** agrupamentos de componentes por funcionalidade, reutilizáveis em outros projetos.
- **Serviços:** responsáveis pela comunicação com sistemas externos (APIs, *backends*). Podem ser injetados nos componentes.

## MOM (Message-Oriented Middleware)
Padrão que estabelece um **mecanismo assíncrono de troca de mensagens** entre aplicações. Diferente do modelo cliente-servidor (onde há requisição e espera por resposta).

**Papéis:**
- **Publicador (publisher):** aplicação que escreve em um tópico — atua como "cliente".
- **Inscrito (subscriber):** aplicação que recebe mensagens de um tópico — atua como "servidor".
- **Broker:** componente central que gerencia os tópicos e entrega as mensagens dos publicadores aos inscritos.

Uma mesma aplicação pode atuar como publicadora e inscrita simultaneamente.

**Implementações reais:** RabbitMQ, Apache Kafka, ActiveMQ. MQTT é um **protocolo** (não uma implementação). **Spring Boot não é MOM** — é framework Java que pode integrar com brokers.

## SOA (Service-Oriented Architecture)
Padrão arquitetural para **padronização da comunicação entre grande volume de aplicações**. Estende o modelo cliente-servidor estabelecendo um mecanismo flexível e padronizado.

Aplicações oferecem seus serviços via **APIs** consumíveis por outras aplicações.

**Padronização histórica:** SOA se popularizou com **SOAP e WSDL** (e UDDI para descoberta), não com REST. REST é mais associado a arquiteturas atuais de microsserviços.

**Documentação continua sendo necessária** (WSDL no caso de SOA clássico, OpenAPI no caso REST) — a padronização não elimina a necessidade de contratos documentados.

## Sources
- [[raw/subjects/S03-ArqSoft/Practice exam NP1]] — questões 6, 7, 8, 9
