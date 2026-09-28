São quatro: Abstração, Encapsulamento, Herança e Polimorfismo. Caiu na [[Practice exam NP1]] (questão 1), e a pegadinha era trocar as definições entre eles.

## Abstração
Representar uma entidade focando **só no que é relevante para o problema** e escondendo detalhes de implementação. É sobre *o que* ela faz, não *como*. Ex.: para um sistema de biblioteca, um `Livro` tem título e ISBN, e não precisa ter peso.

- Não confundir: modificador de visibilidade **não** é abstração, é encapsulamento.

## Encapsulamento
Isolar o estado interno da classe com modificadores de visibilidade (`public`, `protected`, `private`), para que os atributos não sejam acessados nem modificados diretamente. O acesso passa pela interface pública (métodos, getters/setters).

- Não confundir: "referência genérica apontando para filha" **não** é encapsulamento, é polimorfismo.

## Herança
Reaproveitamento de código: classes mais genéricas implementam características e comportamentos gerais, e classes mais específicas herdam e estendem.

## Polimorfismo
Uma referência para uma classe genérica pode apontar para instâncias dela **ou de qualquer classe filha**. O mesmo código funciona com tipos concretos diferentes, e cada um responde do seu jeito à mesma chamada.

```java
Animal a = new Cachorro(); // referência genérica, instância filha
a.emitirSom();             // chama o emitirSom() do Cachorro
```

> [!note] Complemento
> - Existem dois tipos de polimorfismo: **sobrescrita** (override, decidida em tempo de execução, que é a do exemplo acima) e **sobrecarga** (overload, mesmo nome com parâmetros diferentes, decidida em compilação).
> - Herança acopla forte a filha ao pai. Por isso a recomendação clássica é *preferir composição a herança* quando a relação não for um "é um" de verdade. Os padrões Strategy e Decorator (ver [[Padrões de Projeto]]) são exemplos de composição no lugar de herança.
