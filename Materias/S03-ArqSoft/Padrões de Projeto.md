Padrões de projeto (catálogo do GoF, *Gang of Four*) atuam no nível de **classes e objetos**. Os [[Padrões Arquiteturais]] atuam no nível do sistema inteiro. Caiu na [[Practice exam NP2]].

O GoF divide os padrões em três grupos:
- **Criacionais:** como criar objetos (Factory Method, Singleton)
- **Estruturais:** como compor classes/objetos (Adapter, Decorator)
- **Comportamentais:** como os objetos interagem e dividem responsabilidades (Strategy, Observer)

## Factory Method (criacional)
Define na superclasse um método para criar o objeto, mas deixa a decisão de **qual classe concreta instanciar** para as subclasses. É ideal justamente quando o tipo exato **não** é conhecido antecipadamente: o código cliente depende só da abstração.

## Singleton (criacional)
Garante **uma única instância** e dá um ponto de acesso global a ela. **Não** é boa prática sair aplicando em toda classe (nem em toda classe de configuração): vira estado global, aumenta o acoplamento e dificulta testes. Um uso razoável: o driver do banco no lab de Neo4j (S02), que é caro de criar e deve ser único.

## Adapter (estrutural)
Converte a interface de uma classe para a interface que o cliente espera, e com isso classes incompatíveis passam a trabalhar juntas. É como um adaptador de tomada: não muda o que o objeto faz, só "traduz" a interface.

## Decorator (estrutural)
Adiciona responsabilidades a um objeto **dinamicamente** (em tempo de execução), embrulhando-o num objeto com a mesma interface, **sem modificar a classe original**. Dá para empilhar vários decorators.

- Adapter x Decorator: o Adapter **muda** a interface para resolver incompatibilidade. O Decorator **mantém** a interface e acrescenta comportamento.

## Strategy (comportamental)
Encapsula algoritmos intercambiáveis atrás de uma interface, e o contexto troca de estratégia em tempo de execução. Uma estratégia nova entra **sem mexer no contexto**, porque ele só conhece a interface (é o OCP na prática, ver [[Princípios de Design]]).

## Observer (comportamental)
O **Subject** mantém uma lista de **Observers** e notifica todos automaticamente quando seu estado muda. É o mesmo princípio do publish/subscribe do MOM, só que dentro de um programa.

> [!note] Complemento
> - O catálogo completo do GoF tem 23 padrões. Implementações em TypeScript, com diagrama e "quando **não** usar" de cada um: [TobsBP/design-patterns](https://github.com/TobsBP/design-patterns).
> - Padrão aplicado sem necessidade é over-engineering. Se um `if` resolve e as variações não vão crescer, não precisa de Strategy.
