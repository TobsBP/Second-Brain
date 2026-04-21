---
type: subject-concept
subject: S03
tags: [oo, fundamentals, np1]
updated: 2026-04-21
---

# Pilares da Orientação a Objetos

Os quatro pilares clássicos da OO. A disciplina, em alguns contextos, discute três (Herança, Abstração, Encapsulamento), mas Polimorfismo é parte do conjunto canônico e costuma aparecer junto.

## Abstração
Representar entidades focando **apenas nos aspectos relevantes ao problema**, ocultando detalhes de implementação irrelevantes. É sobre *o que* uma entidade faz, não *como*.

**Não confundir com:** modificadores de visibilidade (isso é Encapsulamento).

## Encapsulamento
Capacidade de **isolar o estado interno** de uma classe utilizando modificadores de visibilidade (`public`, `protected`, `private`), protegendo atributos de acesso e modificação direta. O acesso se dá via interface pública (getters/setters/métodos).

**Não confundir com:** representação essencial de entidades (isso é Abstração).

## Herança
Possibilidade de **reaproveitamento de código**: classes mais genéricas implementam características e comportamentos gerais, e classes mais específicas podem herdá-los e estendê-los.

## Polimorfismo
Capacidade de uma **referência para uma classe genérica** poder referenciar instâncias da própria classe ou de qualquer classe filha (que herda dela). Permite que o mesmo código opere sobre diferentes tipos concretos.

**Não confundir com:** Encapsulamento.

## Confusões comuns (ver prova prática NP1)
- "Abstração = modificadores de visibilidade" → **errado**, isso é Encapsulamento.
- "Encapsulamento = referência genérica a instâncias filhas" → **errado**, isso é Polimorfismo.

## Sources
- [[raw/subjects/S03-ArqSoft/Practice exam NP1]] — questão 1
