1.No padrão **MVC**, o **Modelo** é o único componente que pode interagir diretamente com a **Visualização (View)**, garantindo que o Controlador (Controller) não manipule dados antes de serem exibidos.  

Incorreta, controller pode sim maniplar dados antes de serem exibidos

2.O padrão **Singleton** garante que uma classe terá apenas uma instância e fornece um ponto de acesso global para ela, sendo uma prática recomendada para todas as classes que manipulam dados de configuração.  

Incorreto, nem todas as classes é recomendado aplicar singleton por conta do alto acoplamento, dependências globias.

3.Em uma arquitetura **SOA** com comunicação **REST**, o uso de formatos padronizados como JSON é estritamente obrigatório, e a API não consegue processar requisições com o corpo da mensagem vazio ou em formato inesperado, devendo sempre retornar um erro imediatamente.  

Json não é obrigatorio, e a api pode ou não retornar o erro na hora.

4.Os **Serviços** em uma aplicação **SPA** (Single Page Application) são primariamente responsáveis por implementar todas as regras de negócio complexas do sistema, sendo injetados nos componentes para garantir que a lógica de interface se mantenha enxuta.  

Incorreta, dependendo da regra de negocio acaba sendo inviavel colocar no front.

5.O padrão **MOM** (Message-Oriented Middleware) é um modelo de comunicação síncrona, pois o **Broker** deve confirmar o recebimento da mensagem pelo **Inscrito (Subscriber)** antes que o **Publicador (Publisher)** possa enviar a próxima mensagem.  

Incorreta, o MOM é assincrono

6.O padrão **Factory Method** define um método em uma superclasse para a criação de um objeto, mas delega a responsabilidade de instanciar o objeto correto para as subclasses, o que o torna ideal para situações onde o tipo de objeto a ser criado é conhecido antecipadamente.  

Incorreta, não deve depender de classes concretas específicas

7.O padrão **Strategy** permite que diferentes algoritmos sejam encapsulados e trocados dinamicamente em tempo de execução, mas isso exige que o código do contexto principal seja modificado sempre que uma nova estratégia for introduzida.  

Incorreta, novas estratégias podem ser adicionadas sem modificar o código do contexto

8.O padrão **Adapter** (ou Adaptador) é um padrão de projeto estrutural que converte a interface de uma classe para outra interface que o cliente espera, permitindo que classes com interfaces incompatíveis trabalhem em conjunto.  

Correta

9.Diferente do padrão **Adapter**, o padrão **Decorator** é usado para adicionar novas responsabilidades a objetos individuais de forma estática, alterando a estrutura de uma classe para incluir novas funcionalidades.  

Incorreta, decoretor permite adicionar novas responsabilidades a objetos individuais de forma dinâmica, sem modificar sua classe original.

10.No padrão **Observer**, o **Sujeito** (Subject) mantém uma lista de objetos **Observadores** (Observers) e é responsável por notificar todos eles automaticamente quando o seu estado interno muda.

Correta

---

1. **Incorreta.** No MVC, o Controller é o intermediário entre Model e View — ele pode sim manipular dados antes de exibi-los. O Model não interage diretamente com a View.
    
2. **Incorreta.** O Singleton garante uma única instância com acesso global, mas não é recomendado para todas as classes, pois introduz alto acoplamento e dependências globais que dificultam testes e manutenção.
    
3. **Incorreta.** JSON não é obrigatório em APIs REST — outros formatos como XML ou form-data são válidos. Além disso, o comportamento da API ao receber dados inesperados depende da implementação; ela pode ou não retornar erro imediatamente.
    
4. **Incorreta.** Serviços em SPAs geralmente lidam com comunicação com o backend e lógica auxiliar, mas regras de negócio complexas não devem ficar no frontend — isso seria inviável e inseguro. A responsabilidade real sobre regras de negócio pertence ao backend.
    
5. **Incorreta.** MOM é um modelo de comunicação **assíncrona**. O Publisher envia a mensagem ao Broker sem aguardar confirmação de entrega ao Subscriber — esse desacoplamento temporal é justamente uma das principais características do padrão.
    
6. **Incorreta.** O Factory Method é adequado justamente quando o tipo exato do objeto **não** é conhecido antecipadamente. A ideia é depender de abstrações, delegando às subclasses a decisão sobre qual classe concreta instanciar.
    
7. **Incorreta.** Uma das vantagens do Strategy é que novas estratégias podem ser adicionadas sem modificar o contexto. O contexto trabalha com a interface da estratégia, não com implementações concretas.
    
8. **Correta.** O Adapter converte a interface de uma classe para outra esperada pelo cliente, permitindo a integração de classes com interfaces incompatíveis.
    
9. **Incorreta.** O Decorator adiciona responsabilidades a objetos de forma **dinâmica**, em tempo de execução, sem modificar a classe original — ao contrário do que a afirmação sugere.
    
10. **Correta.** No Observer, o Subject mantém a lista de Observers e os notifica automaticamente a cada mudança de estado.