![[TypeScript Draw]]

Nasceu em 2012, escrever em JS era horrível, em projetos grandes era uma dor de cabeça. A ideia do TypeScript não é competir com o JS, mas sim  ser um conjunto de desenvolvimento. Todo arquivos .js era valido em .ts ou seja você não precisava converter todas codebase, poderia fazer isso gradualmente. E um dos pontos fortes de seu desenvolvimento foi a tipagem, se um objeto tem a tipagem certa, ela não precisa declarar que implementa alguma interface.

### Modelo de memória 
TS não tem modelo de memória próprio. Os tipos são apagados na compilação, não existem em runtime, ou seja no final é o modelo de memória do JavaScript.

#### Valores primitivos e referências
JavaScript tem duas categorias: 
- Primitivos (imutáveis): number, string, boolean, bigint, symbol, null, undefined.
- Objetos: arrays, funcs, map... vivem no heap, e o que recebem são referências.

```typescript
let a = { n: 1 };
let b = a;
b.n = 2;

console.log(a.n); // 2 — a e b apontam pro mesmo objeto

function troca(obj: { n: number }) {
	obj = { n: 99 };
}

troca(a)
console.log(a.n); // 2 — obj foi reatribuído só dentro da função
```

Isso que ocorre é chamado de "call by sharing", você passa uma referência por valor. Não dá para fazer uma variável de fora apontar para outro objeto.

#### Stack e Heap
Conceitualmente existe uma pilha de chamadas, os objetos ficam no heap que é uma região bem maior e gerenciada automaticamente. 

```typescript
function contador() {
	let n = 0;
	return () => ++n;
}
const c = contador();

console.log(c()); // 1
console.log(c()); // 2
```

Aqui `n` deveria morrer quando `contador()` retorna, mas a arrow function captura ela — isso é uma **closure**. Como ainda existe referência, `n` sobrevive (no heap, dentro do contexto capturado) enquanto `c` existir.

#### Garbage Collector
Não existe `free`/`delete` de memória. O motor (V8 no Node/Chrome) usa **mark-and-sweep**: parte das raízes (variáveis globais, pilha atual) e marca tudo que é alcançável; o que não foi marcado é liberado. A regra é **alcançabilidade**, não contagem de referências — por isso ciclos (`a.b = b; b.a = a`) são coletados normalmente.

Vazamentos típicos em JS/TS: listeners que nunca são removidos, `setInterval` esquecido, caches em `Map` que só crescem (`WeakMap` resolve quando a chave é objeto), closures segurando objetos grandes.

### Modelo de concorrência
JS é **single-thread** com **event loop**. Não existe duas funções rodando ao mesmo tempo na mesma thread, então não tem race condition de memória como em Java/C — mas tem de ordem de execução.

- **Call stack**: executa o código síncrono até esvaziar.
- **Microtasks**: callbacks de `Promise` (`.then`, `await`). Rodam todas assim que a stack esvazia.
- **Macrotasks**: `setTimeout`, I/O, eventos. Uma por volta do loop, depois das microtasks.

```typescript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// 1, 4, 3, 2
```

`async/await` é só açúcar sobre Promise: o `await` pausa a função e devolve o controle ao event loop. Operação pesada de CPU trava tudo — para paralelismo real existe `Worker` (Web Workers / `worker_threads`).

### Sistema de tipos
#### Tipagem estrutural
O que importa é a **forma**, não o nome (duck typing em tempo de compilação). Diferente de Java/C#, que são nominais.

```typescript
interface Ponto { x: number; y: number }
const p = { x: 1, y: 2, z: 3 };
const q: Ponto = p; // ok, p tem pelo menos x e y
```

#### Inferência
Não precisa anotar tudo: `let n = 0` já é `number`. `const s = "a"` é o tipo literal `"a"`. Boa prática: anotar parâmetros e retornos públicos, deixar o resto inferir.

#### any vs unknown vs never
- `any`: desliga o type checker. Contamina tudo que toca.
- `unknown`: "pode ser qualquer coisa", mas obriga a checar antes de usar. É o `any` seguro.
- `never`: valor que nunca existe (função que sempre lança erro, `switch` exaustivo).

#### Union e narrowing
```typescript
function tamanho(v: string | string[]) {
	if (typeof v === "string") return v.length; // aqui v é string
	return v.length;                              // aqui v é string[]
}
```
O compilador estreita o tipo com `typeof`, `instanceof`, `in`, `===` e **discriminated unions** (um campo literal tipo `kind: "circulo" | "quadrado"`).

#### Generics
Tipos parametrizados, para escrever uma vez e manter a tipagem:
```typescript
function primeiro<T>(arr: T[]): T | undefined {
	return arr[0];
}
const n = primeiro([1, 2, 3]); // number | undefined
```

Utility types prontos: `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`, `Record<K, V>`, `ReturnType<F>`.

#### Consequência do type erasure
Como os tipos somem, **não dá pra checar interface em runtime** (`x instanceof Ponto` não existe). Dados que vêm de fora (API, JSON, formulário) chegam sem garantia nenhuma — o `as Tipo` só engana o compilador. Por isso se valida na fronteira com type guards (`x is Ponto`) ou libs como Zod.

### Compilação
`tsc` faz duas coisas separadas: **checa tipos** e **emite JS**. Erro de tipo não impede gerar o JS por padrão. Muitas ferramentas (esbuild, SWC, Bun, Node com `--experimental-strip-types`) só apagam os tipos sem checar nada — o check fica pro `tsc --noEmit` no CI.

No `tsconfig.json`, o principal é `"strict": true`, que liga entre outros:
- `strictNullChecks`: `null`/`undefined` deixam de caber em qualquer tipo. Sem isso, metade da segurança do TS some.
- `noImplicitAny`: proíbe parâmetro sem tipo virar `any` silenciosamente.

