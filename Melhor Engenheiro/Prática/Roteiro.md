Aprender fazendo como num trabalho de verdade: o Claude é o cliente/PO, eu sou o engenheiro.

## Como funciona

1. O Claude passa um **briefing**: produto, necessidade, personas, restrições. Sem solução técnica.
2. Eu escrevo os **requisitos** e as **ADRs**, depois implemento.
3. Revisão: ✅ o que está bom, ❌ o que está errado, 💡 o que pode melhorar — com pistas, sem reescrever meu trabalho.
4. Refaço até ficar sólido. O que eu aprender nas revisões vira nota em `Lições/`.

Travou de verdade? Pedir "me mostra".

## Estrutura

```
Atividades/<NN> - <Produto>/
  Briefing.md      ← do Claude
  Requisitos.md    ← meu
  ADRs/            ← meu, uma decisão por arquivo (0001 - <Título>.md)
  Revisões.md      ← do Claude
  Tentativas/      ← versões antigas, quando o Claude corrige a pedido
Lições/            ← conceitos que surgiram nas revisões
```

## Modelo de ADR

```markdown
## Status
Proposta | Aceita | Substituída por [[...]]

## Contexto
Que problema forçou essa decisão? Que restrições existem?

## Decisão
O que foi decidido, em uma ou duas frases.

## Alternativas consideradas
Cada opção com prós e contras — e por que não foi escolhida.

## Consequências
O que fica mais fácil, o que fica mais difícil, o que vira dívida.
```

## Atividades

| # | Produto | Status |
|---|---------|--------|
| 01 | [[Atividades/01 - Contas da República/Briefing\|Contas da República]] | requisitos e ADRs |
