---
type: subject-concept
subject: S03
tags: [design-patterns, gof, np2]
updated: 2026-07-21
---

# Padrões de Projeto (GoF)

Padrões de projeto no nível de **classes e objetos** (diferente de [[wiki/subjects/S03-ArqSoft/padroes-arquiteturais]], que opera no nível da arquitetura do sistema como um todo).

## Factory Method
Define um método numa superclasse para criação de um objeto, delegando às subclasses a decisão de qual classe concreta instanciar. Ideal justamente quando o tipo exato do objeto **não** é conhecido antecipadamente — a ideia é depender de abstrações, não de implementações concretas.

## Strategy
Encapsula diferentes algoritmos e permite trocá-los dinamicamente em tempo de execução. Uma vantagem central: novas estratégias podem ser adicionadas **sem modificar o código do contexto** — o contexto trabalha com a interface da estratégia, não com implementações concretas.

## Adapter
Padrão estrutural que converte a interface de uma classe para outra interface esperada pelo cliente, permitindo que classes com interfaces incompatíveis trabalhem juntas.

## Decorator
Adiciona novas responsabilidades a objetos individuais de forma **dinâmica** (em tempo de execução), sem modificar a classe original — ao contrário do Adapter, que resolve incompatibilidade de interface.

## Observer
O **Sujeito (Subject)** mantém uma lista de **Observadores (Observers)** e notifica todos automaticamente quando seu estado interno muda.

## Singleton
Garante uma única instância de uma classe com ponto de acesso global. **Não é boa prática aplicar a todas as classes de configuração** — introduz alto acoplamento e dependências globais que dificultam testes e manutenção.

## Erros comuns (practice exam NP2)
- MVC: o Controller *pode* manipular dados antes de exibi-los — o Model não interage diretamente com a View.
- SOA/REST: JSON não é obrigatório; o comportamento da API diante de corpo vazio/inesperado depende da implementação.
- SPA: regras de negócio complexas não devem ficar nos serviços do frontend — inviável e inseguro, pertence ao backend.
- MOM: é **assíncrono** — o Publisher não espera confirmação do Subscriber antes de enviar a próxima mensagem.

## Sources
- [[raw/subjects/S03-ArqSoft/Practice exam NP2.md]]
