---
type: subject-concept
subject: S03
tags: [design-principles, solid, np1]
updated: 2026-04-21
---

# Princípios de Projeto de Software

Os quatro conceitos-base que guiam o projeto de bons sistemas de software, apoiados por SOLID e Demeter.

## Os quatro conceitos fundamentais

### 1. Integridade conceitual
O software precisa se manter **íntegro conceitualmente**: fazer o que tem que ser feito de maneira padronizada e coerente ao longo do tempo.

### 2. Ocultamento de informações
Uso das abstrações corretas: ocultar toda informação que não for fundamental para o entendimento e uso dos componentes, expondo apenas **interfaces públicas** para uso externo.

### 3. Coesão
Um componente de software deve ser coeso: **implementar apenas uma única funcionalidade**, com todos os métodos e atributos atuando para essa única finalidade. Alta coesão = responsabilidade única e clara.

### 4. Acoplamento (baixo)
Um componente deve ter **baixo acoplamento**: evitar dependência forte e/ou nebulosa com outros componentes. Baixo acoplamento = poucas dependências externas e bem definidas.

> ⚠️ **Confusão comum (questão 10 da prova):** as definições de coesão e acoplamento são frequentemente trocadas. Lembre-se:
> - **Coesão** → *internamente*, o componente tem uma única responsabilidade.
> - **Acoplamento** → *externamente*, o componente tem poucas/claras dependências.

## SOLID
Cinco princípios que dão suporte prático aos quatro conceitos fundamentais:

- **S — Single Responsibility (Responsabilidade Única):** uma classe deve ter uma única razão para mudar.
- **O — Open/Closed (Aberto/Fechado):** classes fechadas para modificação, abertas para extensão.
- **L — Liskov Substitution (Substituição de Liskov):** subclasses devem poder substituir suas superclasses sem quebrar o comportamento.
- **I — Interface Segregation (Segregação de Interfaces):** muitas interfaces específicas são melhores que uma genérica.
- **D — Dependency Inversion (Inversão de Dependências):** depender de abstrações, não de implementações concretas.

## Lei de Demeter
Um método só pode invocar métodos de:
1. Classes dos **objetos passados como parâmetros**
2. **Objetos instanciados pelo próprio método**
3. **Atributos da classe** do método
4. A **própria classe** onde o método está definido

Regra prática: "fale apenas com amigos imediatos". Reduz acoplamento transitivo (evita cadeias como `a.getB().getC().doSomething()`).

## Sources
- [[raw/subjects/S03-ArqSoft/Practice exam NP1]] — questão 10
