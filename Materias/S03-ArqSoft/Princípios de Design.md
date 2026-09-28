Quatro conceitos-base guiam o projeto de bons sistemas, e SOLID e a Lei de Demeter dão o suporte prático. Caiu na [[Practice exam NP1]] (questão 10).

## Os quatro conceitos
1. **Integridade conceitual:** o sistema faz o que tem que fazer de maneira padronizada e coerente ao longo do tempo. Não pode parecer uma colcha de retalhos, em que cada parte segue uma lógica diferente.
2. **Ocultamento de informação:** usar as abstrações certas, esconder tudo que não é fundamental para entender e usar o componente, e expor só uma **interface pública**.
3. **Coesão (alta):** o componente implementa **uma única funcionalidade**, e todos os métodos e atributos atuam para ela.
4. **Acoplamento (baixo):** evitar dependência **forte e/ou nebulosa** com outros componentes. Depender pouco e por contratos claros.

> [!tip] Não trocar coesão com acoplamento
> - **Coesão** olha **para dentro**: tudo na classe serve a uma coisa só.
> - **Acoplamento** olha **para fora**: quanto a classe depende das outras.
>
> A meta é alta coesão e baixo acoplamento.

## SOLID
- **S: Responsabilidade Única (SRP).** Uma classe deve ter um único motivo para mudar.
- **O: Aberto/Fechado (OCP).** **Aberta para extensão, fechada para modificação.** Comportamento novo entra por código novo (subclasse, estratégia nova), sem editar o que já funciona. *(A prova inverteu esse de propósito.)*
- **L: Substituição de Liskov (LSP).** Uma subclasse deve poder substituir a superclasse sem quebrar quem usa. Ex. clássico: `Quadrado extends Retangulo` quebra quando alguém muda só a largura.
- **I: Segregação de Interfaces (ISP).** Várias interfaces específicas são melhores que uma genérica. Ninguém deve ser obrigado a implementar método que não usa.
- **D: Inversão de Dependência (DIP).** Depender de abstrações, não de implementações concretas. Alto nível e baixo nível dependem ambos da interface.

## Lei de Demeter
"Fale só com seus amigos imediatos." Um método só pode chamar métodos:
1. da **própria classe**
2. de **objetos passados como parâmetro**
3. de **objetos criados pelo próprio método**
4. dos **atributos da classe**

Evita cadeias como `pedido.getCliente().getEndereco().getCidade()`, em que o método passa a depender da estrutura interna de três classes.

> [!note] Complemento
> Como cada princípio ajuda os quatro conceitos:
> - SRP → coesão
> - OCP e DIP → baixo acoplamento (o código depende de abstrações e cresce por extensão)
> - ISP → baixo acoplamento e coesão das interfaces
> - LSP → integridade conceitual (a hierarquia se comporta como promete)
> - Demeter → ocultamento de informação e baixo acoplamento
>
> Aplicação real no projeto da NP2 (OCP nas classes de luta, DIP na view de troca): [[Relatorio 2]].
