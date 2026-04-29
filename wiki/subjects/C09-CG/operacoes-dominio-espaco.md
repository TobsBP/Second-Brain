---
type: subject-concept
subject: C09
tags: [processamento-imagem, pixel, operacoes-aritmeticas, operacoes-logicas, dominio-espaco]
updated: 2026-04-29
source_count: 1
---

# Operações no Domínio do Espaço

Operações que atuam **diretamente sobre os valores dos pixels**, sem transformar a imagem para outro domínio (ex.: frequência). Cada pixel de saída depende exclusivamente dos pixels correspondentes nas imagens de entrada — ou seja, são operações **pixel a pixel** (point-wise). São a base para tarefas como redução de ruído, detecção de movimento e seleção de regiões.

## Principais aplicações

- **Redução de ruído** — combinar várias imagens da mesma cena para suavizar variações aleatórias.
- **Detecção de movimento** — diferenciar quadros sucessivos para destacar o que mudou.
- **Seleção de regiões** — isolar áreas de interesse (objeto vs. fundo) por máscaras.

## Operações aritméticas

Operam pixel a pixel entre imagens (ou entre imagem e constante).

### Adição
Soma os valores literais dos pixels. Pode ocorrer **overflow**: se a soma ultrapassa o valor máximo (ex.: `128 + 192 = 320`), o resultado satura no branco.

### Subtração
Subtração literal. Pode ocorrer **underflow**: se o resultado é negativo, satura no preto. Útil para detecção de movimento (diferença entre quadros).

### Multiplicação
Multiplicação literal dos valores (ex.: `2 × 90 = 180`). Comum para ajustar contraste/brilho com um fator.

### Divisão
Divisão literal. **Não pode ocorrer divisão por zero** — quebra o sistema; é preciso tratar esse caso explicitamente.

### Blending (mistura controlada)
Variante melhorada da soma: cada imagem contribui com uma **fração controlada** por um coeficiente `α` (tipicamente `αA + (1−α)B`). Evita o overflow da soma direta e permite transições suaves entre imagens.

## Operações lógicas

Aplicam funções booleanas (**AND, OR, XOR, NOT**) **bit a bit** sobre a representação binária dos pixels. Especialmente úteis em **imagens binárias**, onde cada pixel é apenas objeto ou fundo.

**Convenção em imagens binárias:**
- `1` → objeto
- `0` → fundo

Aplicações típicas:
- **AND** — interseção entre máscaras (manter só o que está em ambas).
- **OR** — união (manter o que está em qualquer uma).
- **XOR** — diferença simétrica (destaca o que mudou entre duas máscaras).
- **NOT** — inversão (troca objeto e fundo).

## Resumo rápido

| Operação | Risco / Cuidado | Uso típico |
|---|---|---|
| Adição | Overflow (satura no branco) | Sobreposição, média de imagens |
| Subtração | Underflow (satura no preto) | Detecção de movimento |
| Multiplicação | Saturação por fator alto | Ajuste de contraste |
| Divisão | Divisão por zero | Normalização |
| Blending | Escolha do coeficiente α | Transição suave, fade |
| AND/OR/XOR/NOT | Interpretar bit a bit | Máscaras em imagens binárias |

## Sources
- [[Operações no Domínio do Espaço]] — definição, princípios, operações aritméticas e lógicas
