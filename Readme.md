## Já se perguntou por que às vezes uma variável ou função funciona antes mesmo de você declará-la?

Se você já viu um código onde algo era usado antes de ser criado, e mesmo assim funcionava, você provavelmente esbarrou no famoso _(e confuso)_ **Hoisting** do JavaScript.

# Mas então... **O que é Hoisting?**

##### _Hoisting_ (ou “elevação”) é o comportamento do JavaScript de **“elevar” automaticamente certas declarações para o topo do escopo** (seja o escopo global ou de função) **antes de executar o código**.

==\*Isso significa que **algumas variáveis ou funções podem ser reconhecidas antes da linha onde foram escritas no código.\***==

### 🤔 Mas como isso funciona exatamente? (pega a visão)

**Vamos ver um exemplo:**

```
console.log(a); // undefined
var a = 5;
```

Mesmo ==`a`== sendo declarada depois, o código ==não dá erro.== Por quê?

#### Porque o JavaScript, **\*antes de rodar qualquer linha**, reorganiza o código internamente mais ou menos assim:\*

```
var a;          // hoisted (sobe) para o topo
console.log(a); // undefined (valor ainda não foi atribuído)
a = 5;          // agora o valor é definido
```

Ou seja, **a declaração foi ==“elevada”,== mas o valor ainda ==não existia.**== Por isso vemos ==`undefined`.==

## ⚠️ Importante: `let` e `const` também são hoisted, mas com uma diferença

#### ==Eles são elevados, **mas não podem ser acessados antes da linha onde aparecem**. Se você tentar, o JavaScript lança um erro.==

```
console.log(b); // ReferenceError!
let b = 10;
```

Esse comportamento é causado por algo chamado **_“temporal dead zone”_** ==(uma zona onde a variável já foi “reservada”),== mas **ainda não pode ser usada**.

# E com funções, como fica?

- **Funções declaradas** são totalmente elevadas, inclusive o corpo:

```
darUmSalve(); // Funciona!

function darUmSalve() {
  console.log("Salve NMB!");
}
```

- **Funções atribuídas a variáveis ==(com const, let, var)**== _NÃO_ são elevadas com seus valores:

```
falar(); // ReferenceError!
const falar = function () {
  console.log("Eae!");
};
```

### Por que `let` e `const` são hoisted, mas **não podem ser usadas antes da linha onde aparecem**?

`let` e `const` **são hoisted**, sim, o JavaScript **sobe a declaração para o topo do escopo**.

**Mas**: elas ficam presas dentro de uma área especial chamada ==**_Temporal Dead Zone==_ (TDZ)** até a linha onde são declaradas.

Você só pode usá-las ==**após a linha de declaração**==, e se tentar antes, o JavaScript lança um **`ReferenceError`**.

# 🧠 O que é essa tal de _Temporal Dead Zone (TDZ)_?

É o nome técnico para o intervalo de tempo entre o **início do escopo** e a **declaração real da variável**. Durante esse intervalo:

- A variável **existe** (porque foi hoisted),
- Mas **ainda não está inicializada**, então **qualquer acesso a ela dá erro.**

#### Vamos lá, um pequeno exemplo visual:

```
{
  // Aqui começa o escopo
  // A variável `x` já existe, mas está na TDZ

  console.log(x); // ❌ ReferenceError: Cannot access 'x' before initialization
  let x = 10;
}
```

==Nos bastidores, o JavaScript fez algo assim:==

```
// Hoisting da variável (sem valor)
let x; // Criada, mas ainda NÃO inicializada
// TDZ começa aqui
console.log(x); // Erro porque ainda estamos na TDZ
x = 10; // Agora sim a TDZ acaba, x foi inicializado
```

## 🤔 Por que criaram isso assim? (faz pergunta difícil pra mim não, man)

Boa pergunta. A resposta tem a ver com **evitar bugs difíceis de rastrear**.

O `var` permitia comportamentos esquisitos, como esse:

```
console.log(nome); // undefined
var nome = "NMB";
```

Com `let` e `const`, o JavaScript te **protege de acessar variáveis prematuramente**, o que é mais seguro:

```
console.log(nome); // ReferenceError
let nome = "NMB";
```

Essa é uma ==**escolha de design**== feita a partir do ==ES6 (2015),== para tornar o código ==mais previsível== e evitar erros silenciosos.

### **RESUMIN DO LET E CONST:**

| Comportamento             | `var`             | `let` / `const`      |
| ------------------------- | ----------------- | -------------------- |
| É hoisted?                | Sim               | Sim                  |
| Valor inicial             | `undefined`       | Nenhum (TDZ ativa)   |
| Pode usar antes?          | Sim (mas cuidado) | Não (ReferenceError) |
| TDZ (Temporal Dead Zone)? | Não               | Sim                  |

# _RESUMINDO A PARADA TODA:_

| Tipo            | É hoisted? | Pode ser usada antes da linha?      |
| --------------- | ---------- | ----------------------------------- |
| `var`           | Sim        | Sim, mas valor será `undefined`     |
| `let` / `const` | Sim        | **Não!** Dá erro (`ReferenceError`) |
| `function`      | Sim        | Sim, funciona normalmente           |
| `const func =`  | Não        | Não funciona antes da linha         |

# 🎓 Conclusão

#### O _Hoisting_ pode parecer estranho no início, mas entendê-lo ajuda muito a evitar erros e confusões, especialmente quando seu código cresce. Saber **o que é movido para o topo** e **o que ainda não está disponível** é um passo importante para dominar o JavaScript com confiança.
