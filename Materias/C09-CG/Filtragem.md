Para poder fazer uma filtragem deve-se transformar a imagem para o domínio da frequência. A imagem é multiplicada por uma função de transferência, depois a transformada inversa é calculada para visualizar a imagem resultante.

## Por que usar transformadas?

- Sem perda de informação
- Apenas a representação muda

Cada ponto no domínio da frequência pode conter informações sobre toda a imagem do domínio espacial.

## Pipeline em alto nível

```
Imagem -> Transformada (Fourier) -> Processamento (Filtragem) -> Inversa
```

1. Aplica a transformada (DFT/FFT) → imagem vai para o domínio da frequência.
2. Multiplica o espectro pela função de transferência do filtro, atenuando ou zerando certas frequências.
3. Aplica a transformada inversa (IDFT/IFFT) → volta para o domínio do espaço.

## Filtros

| | Passa-baixa | Passa-alta |
|---|---|---|
| Preserva | Frequências baixas (regiões suaves) | Frequências altas (bordas, detalhes) |
| Remove | Frequências altas (detalhes, ruído) | Frequências baixas (áreas uniformes) |
| Efeito visual | Imagem **borrada/suavizada** | **Bordas realçadas**, fundo escuro |

> [!note] Complemento
> **O que é "frequência" numa imagem:** o quanto a intensidade muda de um pixel para o vizinho.
> - **Baixa frequência** = variação lenta: regiões uniformes, céu, fundo liso.
> - **Alta frequência** = variação rápida: bordas, texturas finas, ruído.
>
> **Espectro centralizado:** depois de centralizar a transformada, as baixas frequências ficam **no centro** e as altas **nas bordas**. O ponto central (0,0) é a componente DC = brilho médio da imagem. Por isso o filtro é uma "máscara" em volta do centro, definida por uma **frequência de corte** D₀:
> - Passa-baixa: mantém um círculo no centro, zera fora.
> - Passa-alta: zera o círculo do centro, mantém fora (o fundo fica escuro porque o DC some).
> - **Passa-faixa / rejeita-faixa:** mantém/remove um anel — útil para remover ruído periódico (listras).
>
> **Tipos de filtro (do mais brusco ao mais suave):**
> - **Ideal:** corte abrupto (1 dentro de D₀, 0 fora). Gera o artefato de **ringing** (anéis/"ondulações" em volta das bordas).
> - **Butterworth:** transição suave controlada pela ordem n; ordem alta se aproxima do ideal.
> - **Gaussiano:** transição mais suave de todas, **sem ringing**.
>
> **Teorema da convolução:** multiplicar no domínio da frequência equivale a convoluir no domínio do espaço (`f * h ⇔ F · H`). Ou seja, um filtro passa-baixa em frequência equivale a uma máscara de média/suavização aplicada direto nos pixels (ver [[Operações no Domínio do Espaço]]). Para máscaras grandes, fazer via FFT sai mais barato.
