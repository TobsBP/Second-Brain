Permite modelar, analisar e solucionar problemas complexos utilizando modelos matemáticos, estatísticos e algoritmos. Nessa matéria vou mexer muito com Excel. 

**Notas**: 70% prova | 30% exercício / apresentação
## Responde:

- Qual é a melhor forma de usar recursos limitados? 
- Como minimizar custos ou maximizar resultados?
- Qual decisão traz o melhor desempenho, considerando várias restrições?

## Métodos
Que vou ver em sala são esses:

- Programação linear
- Programação inteira
- Otimização em Redes (transporte, fluxo máximo, caminho mínimo)
- Análise de Decisão

## Programação Linear

> [!note] Complemento
> **Estrutura de um modelo de PL:**
> 1. **Variáveis de decisão:** o que eu controlo (ex.: $x_1$ = quantidade produzida do produto 1).
> 2. **Função objetivo:** o que quero maximizar (lucro) ou minimizar (custo), escrita em função das variáveis: $\max Z = c_1x_1 + c_2x_2 + \dots$
> 3. **Restrições:** os limites de recursos (horas, matéria-prima, demanda), cada uma como $\le$, $\ge$ ou $=$.
> 4. **Não-negatividade:** $x_i \ge 0$ (não existe produzir quantidade negativa).
>
> É "linear" porque a função objetivo e as restrições são todas lineares (nada de $x^2$, $x_1 \cdot x_2$...). Se as variáveis precisarem ser inteiras, vira **programação inteira**.
>
> **Exemplo (o da [[Att_Trem_Soldado|atividade do trem e soldado]]):**
> - Soldado: vende por 27, custa 10 de matéria-prima + 14 de mão de obra → lucro 3.
> - Trem: vende por 21, custa 9 + 10 → lucro 2.
>
> $$\max Z = 3x_1 + 2x_2$$
> Sujeito a:
> - $2x_1 + x_2 \le 100$ (mão de obra: horas de acabamento)
> - $x_1 + x_2 \le 80$ (mão de obra: horas de carpintaria)
> - $x_1 \le 40$ (limite de produção: demanda máxima de soldados)
> - $x_1, x_2 \ge 0$
>
> Testando os vértices da região viável: $(0,0) \to 0$; $(40,0) \to 120$; $(40,20) \to 160$; $(20,60) \to 180$; $(0,80) \to 160$.
> **Ótimo: 20 soldados e 60 trens, lucro Z = 180.** (Numa PL, o ótimo sempre está num vértice da região viável.)
>
> **Solver do Excel** (habilitar em Suplementos; fica em Dados → Solver): definir a célula objetivo (fórmula da FO, ex.: `SOMARPRODUTO`), escolher Máx/Mín, indicar as células variáveis, adicionar as restrições, marcar "Tornar Variáveis Irrestritas Não Negativas" e usar o método **LP Simplex**.
