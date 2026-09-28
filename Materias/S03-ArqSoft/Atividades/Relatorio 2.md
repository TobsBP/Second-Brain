## Luta

Melhorias nas classes utilizando o SPA, aplicação do OCP. *(Obs.: pelo contexto de SOLID, "SPA" aqui provavelmente é **SRP**, o princípio da responsabilidade única. Conferir.)*

Interfaces de ataque com atributos muito parecidos, poderia ser apenas uma classe abstrata.

---
## Front View de troca de cartas

Aplicação de inversão de dependência, não tem coisas concretas para poder aplicar a inversão.

Falta clareza em relação às trocas, coisas visuais, não dá para saber o que vai aparecer.

---
## View cartas

---

> [!note] Complemento
> - **Interface x classe abstrata:** interface é só contrato (métodos), sem estado. Quando várias "interfaces" repetem os mesmos **atributos**, o que existe ali é estado e comportamento em comum, e o lugar disso é uma classe abstrata (`Ataque`) com o que é comum e subclasses com o que muda. Continua valendo o OCP: um ataque novo é uma subclasse nova, sem mexer nas existentes.
> - **Inversão de dependência (DIP):** módulos de alto nível (a view) não dependem de classes concretas, e ambos dependem de uma abstração. Ela só faz sentido quando existe uma dependência concreta para "inverter" (ex.: a view instanciando direto um serviço de cartas). Sem isso, a interface extra vira abstração sem uso. Ver [[Princípios de Design]].
