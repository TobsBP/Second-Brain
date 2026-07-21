---
type: subject-concept
subject: C09
tags: [frequency-domain, filtering]
updated: 2026-07-21
---

# Filtragem no Domínio da Frequência

## Pipeline
```
Imagem → Transformada (Fourier/FFT) → Filtragem (multiplica pelo filtro) → Transformada inversa (IFFT)
```
1. Converte a imagem do domínio do espaço para o domínio da frequência (FFT).
2. Multiplica o espectro pela função de transferência do filtro desejado, atenuando ou removendo frequências.
3. Aplica a transformada inversa (IFFT) para voltar ao domínio do espaço e visualizar o resultado.

## Por que usar transformadas
- Não há perda de informação — apenas a representação muda.
- Cada valor no domínio da frequência pode conter informação sobre a imagem inteira no domínio espacial.

## Passa-baixa vs. passa-alta

| | Passa-baixa | Passa-alta |
|---|---|---|
| Preserva | Frequências baixas (regiões suaves) | Frequências altas (bordas, detalhes) |
| Remove | Frequências altas (detalhes, ruído) | Frequências baixas (áreas uniformes) |
| Efeito visual | Imagem borrada/suavizada | Bordas realçadas, fundo escuro |

Relaciona-se com [[wiki/subjects/C09-CG/operacoes-dominio-espaco]]: operações no domínio do espaço atuam diretamente pixel a pixel; filtragem no domínio da frequência exige transformar a imagem antes de manipular.

## Sources
- [[raw/subjects/C09-CG/Filtragem.md]]
- [[raw/subjects/C09-CG/Atividade/Lista 3]] — questões 11–14 (passa-alta/passa-baixa, pipeline FFT/IFFT)
- [[raw/subjects/C09-CG/Atividade/Lista de Reposição 02]] — questão 5
