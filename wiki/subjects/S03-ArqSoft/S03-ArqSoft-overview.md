---
type: subject-overview
code: S03
title: Arquitetura de Software
updated: 2026-07-21
classes_ingested: 1
---

# S03 — Arquitetura de Software

## Topics covered so far
- Fundamentos OO (Abstração, Encapsulamento, Herança, Polimorfismo)
- Arquitetura vs Engenharia de software (escopo, papéis)
- Padrões arquiteturais: MVC, SPA, MOM, SOA
- Princípios de design: integridade conceitual, ocultamento, coesão, acoplamento
- SOLID e Lei de Demeter
- Padrões de projeto GoF: Factory Method, Strategy, Adapter, Decorator, Observer, Singleton
- Retrospectiva do projeto prático NP2 (aplicação de OCP/DIP, erros comuns observados em outras equipes)

## Concepts
- [[wiki/subjects/S03-ArqSoft/pilares-oo]] — Abstração, Encapsulamento, Herança, Polimorfismo
- [[wiki/subjects/S03-ArqSoft/arquitetura-vs-engenharia]] — Diferenças de escopo e papéis
- [[wiki/subjects/S03-ArqSoft/padroes-arquiteturais]] — MVC, SPA, MOM, SOA
- [[wiki/subjects/S03-ArqSoft/principios-design]] — Coesão, acoplamento, SOLID, Demeter
- [[wiki/subjects/S03-ArqSoft/padroes-projeto-gof]] — Factory Method, Strategy, Adapter, Decorator, Observer, Singleton
- [[wiki/subjects/S03-ArqSoft/np2-projeto-retrospectiva]] — Lições do projeto próprio e peer review de outras equipes

## Cross-references
- [[wiki/books/eng-soft-moderna/eng-soft-moderna-overview]] — Capítulo 7 (Arquitetura) é altamente relevante.
- [[wiki/concepts/functional-vs-nonfunctional-requirements]] — requisitos não-funcionais são parte central do escopo da arquitetura.

## Class log
| Class | Date | Topic | Ingested |
|------|------|--------|----------|
| Prova prática NP1 | 2026-04-21 | Revisão NP1: pilares OO, arq vs eng, padrões arquiteturais, princípios de design | ✓ |
| Prova prática NP2 | — | Padrões de projeto GoF (Factory Method, Strategy, Adapter, Decorator, Observer, Singleton) | 2026-07-21 |
| Projeto NP2 | — | Retrospectiva: relatórios de autoavaliação e peer review das apresentações | 2026-07-21 |

## Key questions to review
Perguntas-guia para o NP1, construídas a partir dos conceitos cobertos na prova prática.

### Fundamentos OO
1. Quais são os pilares da OO? Defina cada um com suas próprias palavras.
2. Qual a diferença entre Abstração e Encapsulamento? (Confusão comum: modificadores de visibilidade pertencem a qual?)
3. Qual a diferença entre Encapsulamento e Polimorfismo?

### Arquitetura vs Engenharia
4. Qual a relação entre arquitetura e engenharia de software? Qual o foco de cada uma?
5. Qual o papel do arquiteto de software? E do engenheiro?
6. Cite 5 responsabilidades que fazem parte do escopo da arquitetura.
7. Testes automatizados fazem parte do escopo da arquitetura? Justifique.

### Padrões arquiteturais
8. O que significa MVC? Quais os componentes e suas responsabilidades?
9. MVC é um padrão específico para mobile? Por quê?
10. O que é uma SPA? Em que contexto (web/mobile/desktop) ela se encaixa?
11. Explique MOM: papéis (publicador, inscrito, broker), funcionamento e cite implementações reais.
12. Spring Boot é uma implementação de MOM? Justifique.
13. O que é SOA? Com quais tecnologias historicamente se padronizou?
14. Em SOA, a documentação das APIs é dispensável por causa da padronização? Justifique.

### Princípios de design
15. Quais são os quatro conceitos-base que guiam o projeto de bons sistemas?
16. Defina coesão e acoplamento — qual a diferença entre alta coesão e baixo acoplamento?
17. Liste os princípios SOLID e explique cada um brevemente.
18. Enuncie a Lei de Demeter e dê um exemplo do que ela proíbe.

### Padrões de projeto GoF (NP2)
19. Por que o Model não deve interagir diretamente com a View no MVC — quem faz essa ponte?
20. Por que Singleton não deveria ser aplicado indiscriminadamente a classes de configuração?
21. Como o Factory Method lida com tipos de objeto desconhecidos antecipadamente?
22. Como Strategy permite adicionar novos algoritmos sem modificar o contexto?
23. Qual a diferença entre Adapter e Decorator?
24. Como o Observer notifica seus observadores quando o estado do Subject muda?

## Sources
- [[raw/subjects/S03-ArqSoft/Practice exam NP1]] — prova prática de revisão NP1, 10 questões V/F
- [[raw/subjects/S03-ArqSoft/Practice exam NP2.md]] — prova prática de revisão NP2, padrões de projeto
- [[raw/subjects/S03-ArqSoft/Padrões Arquiteturais.md]]
- [[raw/subjects/S03-ArqSoft/Relatorio 2.md]] e [[raw/subjects/S03-ArqSoft/Relatorio 3.md]] — retrospectiva do projeto NP2
