# Manual Completo de JavaScript

## Índice
1. [Introdução](#introdução)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Sintaxe Básica](#sintaxe-básica)
4. [Tipos de Dados](#tipos-de-dados)
5. [Variáveis](#variáveis)
6. [Operadores](#operadores)
7. [Estruturas de Controle](#estruturas-de-controle)
8. [Funções](#funções)
9. [Objetos](#objetos)
10. [Arrays](#arrays)
11. [Programação Orientada a Objetos](#programação-orientada-a-objetos)
12. [Manipulação do DOM](#manipulação-do-dom)
13. [Eventos](#eventos)
14. [Async/Await e Promises](#asyncawait-e-promises)
15. [Módulos](#módulos)
16. [ES6+ Features](#es6-features)
17. [Boas Práticas](#boas-práticas)
18. [Recursos Úteis](#recursos-úteis)

## Introdução

JavaScript é uma linguagem de programação interpretada, de alto nível, dinâmica e multiparadigma. Originalmente criada para tornar páginas web interativas, hoje é usada tanto no cliente (frontend) quanto no servidor (backend).

### Características Principais
- **Interpretada**: Executada linha por linha
- **Dinâmica**: Tipagem dinâmica e fraca
- **Multi-paradigma**: Suporta OOP, funcional e procedural
- **Baseada em protótipos**: Herança via protótipos
- **Event-driven**: Programação orientada a eventos
- **Assíncrona**: Suporte nativo para operações assíncronas

## Configuração do Ambiente

### No Navegador
```html
<!-- Inline -->
<script>
  console.log("Hello World!");
</script>

<!-- Arquivo externo -->
<script src="script.js"></script>

<!-- Atributos -->
<script async src="script.js"></script>
<script defer src="script.js"></script>
```

### Node.js
```bash
# Instalação
npm init -y
npm install

# Execução
node arquivo.js
```

### Ferramentas Recomendadas
- **Editor**: VS Code, WebStorm, Sublime Text
- **Extensões**: ESLint, Prettier, Debugger for Chrome
- **Ferramentas**: Chrome DevTools, Node.js, npm/yarn

## Sintaxe Básica

### Estrutura Básica
```javascript
// Comentário de uma linha

/*
  Comentário
  de múltiplas
  linhas
*/

// Declaração
let nome = "João";

// Expressão
console.log(nome);

// Bloco de código
{
  let x = 10;
  let y = 20;
}
```

### Case Sensitivity
JavaScript é case-sensitive:
```javascript
let nome = "Maria";
let Nome = "João"; // Variável diferente
let NOME = "Pedro"; // Outra variável diferente
```

### Ponto e Vírgula
Opcional, mas recomendado:
```javascript
let x = 10;
let y = 20;
let z = x + y;
```

## Tipos de Dados

### Tipos Primitivos
```javascript
// String
let nome = "JavaScript";
let sobrenome = 'ECMAScript';
let template = `Hello ${nome}`;

// Number
let inteiro = 42;
let decimal = 3.14;
let infinito = Infinity;
let naoNumero = NaN;

// Boolean
let verdadeiro = true;
let falso = false;

// Undefined
let indefinido;
let x = undefined;

// Null
let nulo = null;

// Symbol (ES6)
let simbolo = Symbol("descricao");

// BigInt (ES2020)
let grande = 9007199254740991n;
```

### Tipo Object
```javascript
// Objeto
let pessoa = {
  nome: "João",
  idade: 30
};

// Array
let numeros = [1, 2, 3, 4, 5];

// Function
function soma(a, b) {
  return a + b;
}

// Date
let data = new Date();

// RegExp
let regex = /javascript/i;
```

### Verificação de Tipos
```javascript
typeof "texto";        // "string"
typeof 42;             // "number"
typeof true;           // "boolean"
typeof undefined;      // "undefined"
typeof null;           // "object" (especial)
typeof {};             // "object"
typeof [];             // "object"
typeof function() {};  // "function"
typeof Symbol();       // "symbol"
typeof 42n;            // "bigint"

// Verificação mais precisa
Array.isArray([]);     // true
Object.prototype.toString.call([]); // "[object Array]"
```

## Variáveis

### Escopos
```javascript
// Escopo Global
var globalVar = "Eu sou global";

// Escopo de Função
function exemplo() {
  var functionVar = "Eu sou de função";
  let blockVar = "Eu sou de bloco";
  
  if (true) {
    let blockVar2 = "Acesso apenas neste bloco";
    var functionVar2 = "Acesso em toda função";
  }
}

// Escopo de Bloco (ES6+)
{
  let blockScoped = "Visível apenas aqui";
  const constant = "Constante de bloco";
}
```

### Declarações
```javascript
// var (ES5) - escopo de função
var x = 10;
var x = 20; // Redeclaração permitida

// let (ES6) - escopo de bloco
let y = 30;
// let y = 40; // Erro: redeclaração não permitida
y = 50; // Reatribuição permitida

// const (ES6) - escopo de bloco
const z = 60;
// z = 70; // Erro: reatribuição não permitida
// const z = 80; // Erro: redeclaração não permitida

// Const com objetos
const pessoa = {
  nome: "João"
};
pessoa.nome = "Maria"; // Permitido
// pessoa = {}; // Erro
```

### Hoisting
```javascript
// Com var
console.log(a); // undefined
var a = 10;

// Com let/const
// console.log(b); // Erro: Cannot access 'b' before initialization
let b = 20;

// Equivalente
var c;
console.log(c); // undefined
c = 30;
```

### Boas Práticas
```javascript
// Use const por padrão
const valorFixo = 100;

// Use let quando precisar reatribuir
let contador = 0;
contador++;

// Evite var
// var antigo = "não use"; // Evitar

// Nomeação clara
const MAXIMO_TENTATIVAS = 3;
let usuarioAtivo = true;
const listaDeProdutos = [];
```

## Operadores

### Operadores Aritméticos
```javascript
let a = 10, b = 3;

// Adição
let soma = a + b;       // 13

// Subtração
let diferenca = a - b;  // 7

// Multiplicação
let produto = a * b;    // 30

// Divisão
let quociente = a / b;  // 3.333...

// Módulo (resto)
let resto = a % b;      // 1

// Exponenciação (ES2016)
let potencia = a ** b;  // 1000

// Incremento
a++;    // pós-incremento
++a;    // pré-incremento

// Decremento
b--;    // pós-decremento
--b;    // pré-decremento
```

### Operadores de Atribuição
```javascript
let x = 10;

x += 5;   // x = x + 5 → 15
x -= 3;   // x = x - 3 → 12
x *= 2;   // x = x * 2 → 24
x /= 4;   // x = x / 4 → 6
x %= 4;   // x = x % 4 → 2
x **= 3;  // x = x ** 3 → 8
```

### Operadores de Comparação
```javascript
// Igualdade (valor)
5 == '5';      // true
5 != '5';      // false

// Igualdade estrita (valor e tipo)
5 === '5';     // false
5 !== '5';     // true

// Relacionais
5 > 3;         // true
5 < 3;         // false
5 >= 5;        // true
5 <= 4;        // false
```

### Operadores Lógicos
```javascript
// AND (&&)
true && true;    // true
true && false;   // false

// OR (||)
true || false;   // true
false || false;  // false

// NOT (!)
!true;           // false
!false;          // true

// Operador Nullish Coalescing (??)
null ?? 'default';      // 'default'
undefined ?? 'default'; // 'default'
0 ?? 'default';         // 0
'' ?? 'default';        // ''

// Optional Chaining (?.) (ES2020)
const user = {
  profile: {
    name: 'João'
  }
};
user.profile?.name;     // 'João'
user.settings?.theme;   // undefined
```

### Operadores Bitwise
```javascript
let a = 5, b = 3; // 0101, 0011

a & b;   // AND → 0001 (1)
a | b;   // OR → 0111 (7)
a ^ b;   // XOR → 0110 (6)
~a;      // NOT → 1010 (-6)
a << 1;  // Shift left → 1010 (10)
a >> 1;  // Shift right → 0010 (2)
a >>> 1; // Shift right unsigned → 0010 (2)
```

### Operador Ternário
```javascript
let idade = 18;
let status = idade >= 18 ? 'adulto' : 'menor';

// Equivalente a:
let status;
if (idade >= 18) {
  status = 'adulto';
} else {
  status = 'menor';
}
```

## Estruturas de Controle

### Condicionais
```javascript
// if
if (condicao) {
  // código
}

// if-else
if (condicao) {
  // código se verdadeiro
} else {
  // código se falso
}

// else-if
if (condicao1) {
  // código 1
} else if (condicao2) {
  // código 2
} else {
  // código padrão
}

// switch
let dia = 2;
let nomeDia;

switch (dia) {
  case 0:
    nomeDia = "Domingo";
    break;
  case 1:
    nomeDia = "Segunda";
    break;
  case 2:
    nomeDia = "Terça";
    break;
  default:
    nomeDia = "Desconhecido";
}

// Switch moderno (retorno)
const resultado = (() => {
  switch (valor) {
    case 1: return "um";
    case 2: return "dois";
    default: return "outro";
  }
})();
```

### Loops
```javascript
// for tradicional
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for-in (objetos)
const pessoa = { nome: "João", idade: 30 };
for (let chave in pessoa) {
  console.log(chave, pessoa[chave]);
}

// for-of (iteráveis)
const numeros = [1, 2, 3, 4, 5];
for (let numero of numeros) {
  console.log(numero);
}

// while
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// do-while
let j = 0;
do {
  console.log(j);
  j++;
} while (j < 5);

// forEach (arrays)
numeros.forEach((numero, indice) => {
  console.log(indice, numero);
});
```

### Controle de Fluxo
```javascript
// break - sai do loop
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0,1,2,3,4
}

// continue - pula para próxima iteração
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0,1,3,4
}

// label - rótulo para loops
externo: for (let i = 0; i < 3; i++) {
  interno: for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break externo;
    console.log(i, j);
  }
}

// return - retorna de função
function teste() {
  return "valor";
  console.log("não executado");
}

// throw - lança exceção
throw new Error("Algo deu errado!");

// try-catch-finally
try {
  // código que pode gerar erro
  let resultado = operacaoRiscosa();
} catch (erro) {
  // tratamento do erro
  console.error("Erro:", erro.message);
} finally {
  // sempre executa
  console.log("Finalizado");
}
```

### Truthy e Falsy
```javascript
// Valores falsy
false
0
-0
0n
""
null
undefined
NaN

// Valores truthy (todos os outros)
true
1
-1
"texto"
[]
{}
function() {}
```

## Funções

### Declaração de Funções
```javascript
// Declaração de função (hoisting)
function soma(a, b) {
  return a + b;
}

// Expressão de função
const multiplicar = function(a, b) {
  return a * b;
};

// Arrow function (ES6+)
const dividir = (a, b) => {
  return a / b;
};

// Arrow function com retorno implícito
const quadrado = x => x * x;

// Função construtora
function Pessoa(nome) {
  this.nome = nome;
}

// Função geradora
function* gerador() {
  yield 1;
  yield 2;
  yield 3;
}

// Função assíncrona
async function buscarDados() {
  const resposta = await fetch('url');
  return resposta.json();
}
```

### Parâmetros e Argumentos
```javascript
// Parâmetros padrão (ES6)
function saudacao(nome = "Visitante") {
  return `Olá, ${nome}!`;
}

// Rest parameters
function soma(...numeros) {
  return numeros.reduce((acc, num) => acc + num, 0);
}
soma(1, 2, 3, 4); // 10

// Arguments (obsoleto, usar rest)
function antiga() {
  console.log(arguments);
}

// Destructuring em parâmetros
function imprimirUsuario({ nome, idade }) {
  console.log(`${nome} tem ${idade} anos`);
}

imprimirUsuario({ nome: "João", idade: 30 });
```

### Escopo e Closures
```javascript
// Escopo de função
function externa() {
  let x = 10;
  
  function interna() {
    console.log(x); // Acessa x da função externa
  }
  
  return interna;
}

const closure = externa();
closure(); // 10

// Exemplo prático de closure
function contador() {
  let count = 0;
  
  return {
    incrementar: () => ++count,
    decrementar: () => --count,
    valor: () => count
  };
}

const meuContador = contador();
meuContador.incrementar(); // 1
meuContador.incrementar(); // 2
```

### Funções de Alta Ordem
```javascript
// Função que recebe função como parâmetro
function executar(fn, ...args) {
  return fn(...args);
}

// Função que retorna função
function multiplicador(fator) {
  return function(numero) {
    return numero * fator;
  };
}

const dobrar = multiplicador(2);
dobrar(5); // 10

// Callback functions
function processarArray(arr, callback) {
  const resultado = [];
  for (let i = 0; i < arr.length; i++) {
    resultado.push(callback(arr[i]));
  }
  return resultado;
}

const numeros = [1, 2, 3];
const quadrados = processarArray(numeros, x => x * x); // [1, 4, 9]
```

### Métodos Úteis
```javascript
// call, apply, bind
function apresentar(profissao, pais) {
  return `${this.nome} é ${profissao} no ${pais}`;
}

const pessoa = { nome: "João" };

// call - chama com contexto e argumentos individuais
apresentar.call(pessoa, "programador", "Brasil");

// apply - chama com contexto e array de argumentos
apresentar.apply(pessoa, ["programador", "Brasil"]);

// bind - retorna nova função com contexto fixo
const apresentarJoao = apresentar.bind(pessoa);
apresentarJoao("programador", "Brasil");
```

### IIFE (Immediately Invoked Function Expression)
```javascript
// Padrão
(function() {
  console.log("Executado imediatamente!");
})();

// Com parâmetros
(function(nome) {
  console.log(`Olá, ${nome}!`);
})("João");

// Arrow function
(() => {
  console.log("IIFE com arrow function");
})();

// Módulo pattern
const meuModulo = (function() {
  let privado = "secreto";
  
  function metodoPrivado() {
    return privado;
  }
  
  return {
    metodoPublico: function() {
      return metodoPrivado();
    }
  };
})();
```

## Objetos

### Criação de Objetos
```javascript
// Literal
const pessoa = {
  nome: "João",
  idade: 30,
  saudacao: function() {
    return `Olá, meu nome é ${this.nome}`;
  }
};

// Construtor
function Carro(marca, modelo) {
  this.marca = marca;
  this.modelo = modelo;
  this.ligar = function() {
    return `${this.marca} ${this.modelo} ligado`;
  };
}
const meuCarro = new Carro("Toyota", "Corolla");

// Object.create()
const prototipo = {
  saudacao: function() {
    return `Olá, ${this.nome}`;
  }
};
const joao = Object.create(prototipo);
joao.nome = "João";

// Factory function
function criarPessoa(nome, idade) {
  return {
    nome,
    idade,
    apresentar() {
      return `Eu sou ${this.nome}, tenho ${this.idade} anos`;
    }
  };
}

// Classe (ES6)
class Animal {
  constructor(nome) {
    this.nome = nome;
  }
  
  falar() {
    return `${this.nome} faz um som`;
  }
}
```

### Acesso e Manipulação
```javascript
const usuario = {
  nome: "Maria",
  idade: 25,
  email: "maria@email.com"
};

// Acesso por ponto
console.log(usuario.nome); // "Maria"

// Acesso por colchetes
const propriedade = "idade";
console.log(usuario[propriedade]); // 25

// Adicionar propriedades
usuario.profissao = "Programadora";

// Remover propriedades
delete usuario.email;

// Verificar existência
"nome" in usuario; // true
usuario.hasOwnProperty("nome"); // true

// Obter chaves/valores/entradas
Object.keys(usuario); // ["nome", "idade", "profissao"]
Object.values(usuario); // ["Maria", 25, "Programadora"]
Object.entries(usuario); // [["nome", "Maria"], ...]
```

### Métodos Úteis
```javascript
const obj = { a: 1, b: 2, c: 3 };

// Object.assign() - copia propriedades
const copia = Object.assign({}, obj);
const mesclado = Object.assign({ d: 4 }, obj);

// Object.freeze() - impede modificações
Object.freeze(obj);
// obj.a = 10; // Erro em modo estrito

// Object.seal() - impede adição/remoção
Object.seal(obj);
obj.a = 10; // Permitido
// obj.d = 4; // Erro

// Object.defineProperty()
Object.defineProperty(obj, 'propriedade', {
  value: 42,
  writable: false,
  enumerable: true,
  configurable: false
});
```

### Protótipos
```javascript
// Protótipo padrão
const obj = {};
console.log(obj.toString()); // "[object Object]"
console.log(obj.__proto__ === Object.prototype); // true

// Cadeia de protótipos
function Pessoa(nome) {
  this.nome = nome;
}

Pessoa.prototype.apresentar = function() {
  return `Olá, sou ${this.nome}`;
};

const joao = new Pessoa("João");
console.log(joao.apresentar()); // "Olá, sou João"

// Herança prototipal
function Estudante(nome, curso) {
  Pessoa.call(this, nome);
  this.curso = curso;
}

Estudante.prototype = Object.create(Pessoa.prototype);
Estudante.prototype.constructor = Estudante;

Estudante.prototype.estudar = function() {
  return `${this.nome} está estudando ${this.curso}`;
};
```

## Arrays

### Criação e Manipulação
```javascript
// Criação
const vazio = [];
const numeros = [1, 2, 3, 4, 5];
const misto = [1, "texto", true, { nome: "João" }];

// Construtor
const construtor = new Array(5); // [empty × 5]
const preenchido = new Array(1, 2, 3); // [1, 2, 3]

// Array.of()
const arrayOf = Array.of(1, 2, 3); // [1, 2, 3]

// Array.from()
const arrayFrom = Array.from("hello"); // ["h", "e", "l", "l", "o"]
const arrayFromSet = Array.from(new Set([1, 2, 2, 3])); // [1, 2, 3]

// Operador spread
const copia = [...numeros];
const combinado = [...numeros, 6, 7, 8];
```

### Métodos de Array
```javascript
const frutas = ["maçã", "banana", "laranja"];

// Adicionar/remover elementos
frutas.push("uva"); // Adiciona no final
frutas.pop(); // Remove do final
frutas.unshift("morango"); // Adiciona no início
frutas.shift(); // Remove do início

// splice - adiciona/remove em qualquer posição
frutas.splice(1, 1); // Remove 1 elemento na posição 1
frutas.splice(1, 0, "pera", "kiwi"); // Adiciona na posição 1

// slice - retorna parte do array
const parte = frutas.slice(1, 3); // ["banana", "laranja"]

// concat - combina arrays
const combinado = frutas.concat(["uva", "melancia"]);

// indexOf/lastIndexOf - encontra índice
frutas.indexOf("banana"); // 1
frutas.lastIndexOf("banana"); // 1

// includes - verifica existência
frutas.includes("maçã"); // true

// find/findIndex - encontra com callback
const numeros = [5, 12, 8, 130, 44];
const maiorQue10 = numeros.find(num => num > 10); // 12
const indiceMaiorQue10 = numeros.findIndex(num => num > 10); // 1
```

### Métodos de Iteração
```javascript
const numeros = [1, 2, 3, 4, 5];

// forEach - executa para cada elemento
numeros.forEach((numero, indice) => {
  console.log(indice, numero);
});

// map - transforma array
const dobrados = numeros.map(num => num * 2); // [2, 4, 6, 8, 10]

// filter - filtra elementos
const pares = numeros.filter(num => num % 2 === 0); // [2, 4]

// reduce - reduz a um valor
const soma = numeros.reduce((acc, num) => acc + num, 0); // 15

// reduceRight - reduce da direita para esquerda
const concatenado = numeros.reduceRight((acc, num) => acc + num.toString(), "");

// every - todos satisfazem condição
const todosMaioresQue0 = numeros.every(num => num > 0); // true

// some - algum satisfaz condição
const algumMaiorQue4 = numeros.some(num => num > 4); // true

// sort - ordena array
const desordenado = [3, 1, 4, 1, 5, 9];
desordenado.sort((a, b) => a - b); // [1, 1, 3, 4, 5, 9]

// reverse - inverte array
numeros.reverse(); // [5, 4, 3, 2, 1]
```

### Arrays Multidimensionais
```javascript
// Matriz 2D
const matriz = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

// Acesso
console.log(matriz[0][1]); // 2

// Iteração
for (let i = 0; i < matriz.length; i++) {
  for (let j = 0; j < matriz[i].length; j++) {
    console.log(matriz[i][j]);
  }
}

// Com forEach
matriz.forEach(linha => {
  linha.forEach(elemento => {
    console.log(elemento);
  });
});
```

### Destructuring
```javascript
const numeros = [1, 2, 3, 4, 5];

// Básico
const [primeiro, segundo] = numeros; // primeiro=1, segundo=2

// Ignorar elementos
const [um, , tres] = numeros; // um=1, tres=3

// Rest operator
const [primeiro, ...resto] = numeros; // resto=[2,3,4,5]

// Valores padrão
const [a = 10, b = 20] = [1]; // a=1, b=20

// Troca de valores
let x = 1, y = 2;
[x, y] = [y, x]; // x=2, y=1
```

### Métodos Avançados (ES6+)
```javascript
// Array.from() com map
const quadrados = Array.from([1, 2, 3], x => x * x); // [1, 4, 9]

// Array.of() - cria array com argumentos
Array.of(7); // [7]
Array.of(1, 2, 3); // [1, 2, 3]

// fill - preenche array
const array = new Array(5).fill(0); // [0, 0, 0, 0, 0]
array.fill(1, 1, 3); // [0, 1, 1, 0, 0]

// findLast / findLastIndex (ES2023)
const numeros = [5, 12, 8, 130, 44];
const ultimoMaiorQue10 = numeros.findLast(num => num > 10); // 44
const indiceUltimoMaiorQue10 = numeros.findLastIndex(num => num > 10); // 4

// flat / flatMap (ES2019)
const aninhado = [1, [2, [3, [4]]]];
const nivel1 = aninhado.flat(); // [1, 2, [3, [4]]]
const nivel2 = aninhado.flat(2); // [1, 2, 3, [4]]
const todosNiveis = aninhado.flat(Infinity); // [1, 2, 3, 4]

const frases = ["Hello World", "JavaScript is fun"];
const palavras = frases.flatMap(frase => frase.split(" ")); // ["Hello", "World", "JavaScript", "is", "fun"]

// toSorted, toReversed, toSpliced (ES2023) - não mutáveis
const original = [3, 1, 4, 1, 5];
const ordenado = original.toSorted(); // [1, 1, 3, 4, 5]
const invertido = original.toReversed(); // [5, 1, 4, 1, 3]
const spliceImutavel = original.toSpliced(1, 2, 9, 8); // [3, 9, 8, 1, 5]
```

## Programação Orientada a Objetos

### Classes (ES6+)
```javascript
// Definição básica
class Pessoa {
  // Propriedades da instância
  nome;
  idade;
  
  // Construtor
  constructor(nome, idade) {
    this.nome = nome;
    this.idade = idade;
    this.criadoEm = new Date();
  }
  
  // Métodos da instância
  apresentar() {
    return `Olá, meu nome é ${this.nome} e tenho ${this.idade} anos`;
  }
  
  // Método estático
  static compararIdades(p1, p2) {
    return p1.idade - p2.idade;
  }
  
  // Getter
  get aniversario() {
    return `${this.nome} faz ${this.idade + 1} anos ano que vem`;
  }
  
  // Setter
  set novaIdade(idade) {
    if (idade > 0) {
      this.idade = idade;
    }
  }
}

// Uso
const joao = new Pessoa("João", 30);
console.log(joao.apresentar());
joao.novaIdage = 31;
console.log(Pessoa.compararIdades(joao, new Pessoa("Maria", 25)));
```

### Herança
```javascript
class Animal {
  constructor(nome) {
    this.nome = nome;
  }
  
  falar() {
    return `${this.nome} faz um som`;
  }
  
  mover() {
    return `${this.nome} se move`;
  }
}

class Cachorro extends Animal {
  constructor(nome, raca) {
    super(nome); // Chama construtor da classe pai
    this.raca = raca;
  }
  
  // Sobrescreve método da classe pai
  falar() {
    return `${this.nome} late: Au Au!`;
  }
  
  // Método específico
  abanarRabo() {
    return `${this.nome} está abanando o rabo`;
  }
}

class PastorAlemao extends Cachorro {
  constructor(nome) {
    super(nome, "Pastor Alemão");
  }
  
  guardar() {
    return `${this.nome} está guardando a casa`;
  }
}

// Uso
const rex = new Cachorro("Rex", "Vira-lata");
console.log(rex.falar()); // "Rex late: Au Au!"
console.log(rex.mover()); // "Rex se move"

const max = new PastorAlemao("Max");
console.log(max.guardar()); // "Max está guardando a casa"
```

### Encapsulamento
```javascript
// Campos privados (ES2022)
class ContaBancaria {
  // Campos privados (começam com #)
  #saldo;
  #senha;
  
  constructor(titular, saldoInicial, senha) {
    this.titular = titular;
    this.#saldo = saldoInicial;
    this.#senha = senha;
  }
  
  // Métodos públicos para acessar campos privados
  depositar(valor) {
    if (valor > 0) {
      this.#saldo += valor;
      return `Depósito realizado. Novo saldo: ${this.#saldo}`;
    }
    return "Valor inválido";
  }
  
  sacar(valor, senha) {
    if (senha !== this.#senha) {
      return "Senha incorreta";
    }
    if (valor <= 0 || valor > this.#saldo) {
      return "Saque inválido";
    }
    this.#saldo -= valor;
    return `Saque realizado. Novo saldo: ${this.#saldo}`;
  }
  
  getSaldo(senha) {
    if (senha !== this.#senha) {
      return "Senha incorreta";
    }
    return this.#saldo;
  }
  
  // Método privado
  #validarValor(valor) {
    return typeof valor === 'number' && valor > 0;
  }
}

const conta = new ContaBancaria("João", 1000, "1234");
// conta.#saldo = 10000; // Erro: campo privado
console.log(conta.depositar(500)); // OK
console.log(conta.sacar(200, "1234")); // OK
```

### Mixins
```javascript
// Mixin como função
const Auditar = (Base) => class extends Base {
  auditoria = [];
  
  auditar(acao) {
    this.auditoria.push({
      acao,
      data: new Date(),
      usuario: this.nome || 'Desconhecido'
    });
  }
  
  obterAuditoria() {
    return this.auditoria;
  }
};

const Loggavel = (Base) => class extends Base {
  log(mensagem) {
    console.log(`[${new Date().toISOString()}] ${mensagem}`);
  }
};

// Classe base
class Sistema {
  constructor(nome) {
    this.nome = nome;
  }
}

// Aplicando mixins
class SistemaAuditado extends Auditar(Loggavel(Sistema)) {
  executarTarefa() {
    this.auditar('Tarefa executada');
    this.log('Tarefa completada com sucesso');
    return "Tarefa finalizada";
  }
}

const sistema = new SistemaAuditado("ERP");
sistema.executarTarefa();
console.log(sistema.obterAuditoria());
```

### Polimorfismo
```javascript
class Forma {
  area() {
    throw new Error("Método 'area' deve ser implementado");
  }
  
  perimetro() {
    throw new Error("Método 'perimetro' deve ser implementado");
  }
}

class Retangulo extends Forma {
  constructor(largura, altura) {
    super();
    this.largura = largura;
    this.altura = altura;
  }
  
  area() {
    return this.largura * this.altura;
  }
  
  perimetro() {
    return 2 * (this.largura + this.altura);
  }
}

class Circulo extends Forma {
  constructor(raio) {
    super();
    this.raio = raio;
  }
  
  area() {
    return Math.PI * this.raio ** 2;
  }
  
  perimetro() {
    return 2 * Math.PI * this.raio;
  }
}

// Polimorfismo em ação
const formas = [
  new Retangulo(10, 5),
  new Circulo(7),
  new Retangulo(3, 4)
];

formas.forEach(forma => {
  console.log(`Área: ${forma.area().toFixed(2)}`);
  console.log(`Perímetro: ${forma.perimetro().toFixed(2)}`);
});
```

## Manipulação do DOM

### Seleção de Elementos
```javascript
// Métodos clássicos
document.getElementById("meuId");
document.getElementsByClassName("minhaClasse");
document.getElementsByTagName("div");
document.getElementsByName("meuNome");

// Métodos modernos (querySelector)
document.querySelector("#meuId"); // Primeiro elemento
document.querySelectorAll(".minhaClasse"); // Todos elementos
document.querySelector("div.container"); // Seletor CSS complexo

// Seleção hierárquica
const pai = document.getElementById("pai");
pai.querySelector(".filho"); // Procura apenas dentro do pai
pai.getElementsByClassName("filho"); // Coleção viva

// Atalhos comuns
document.body;
document.head;
document.title;
document.forms;
document.images;
document.links;
```

### Manipulação de Elementos
```javascript
// Criar elementos
const novoDiv = document.createElement("div");
const novoTexto = document.createTextNode("Olá Mundo");
const novoComentario = document.createComment("Comentário");

// Modificar conteúdo
elemento.innerHTML = "<strong>Texto</strong> formatado";
elemento.textContent = "Texto puro sem HTML";
elemento.innerText = "Texto renderizado"; // Considera CSS

// Atributos
elemento.setAttribute("id", "novoId");
elemento.getAttribute("id");
elemento.removeAttribute("id");
elemento.hasAttribute("data-info");

// Atributos booleanos
elemento.disabled = true;
elemento.checked = true;
elemento.selected = true;

// data-* attributes
elemento.dataset.usuarioId = "123";
console.log(elemento.dataset.usuarioId); // "123"

// Classes
elemento.classList.add("nova-classe");
elemento.classList.remove("classe-antiga");
elemento.classList.toggle("ativo");
elemento.classList.contains("ativo");
elemento.className = "classe1 classe2"; // Substitui todas

// Estilos
elemento.style.color = "red";
elemento.style.backgroundColor = "#f0f0f0";
elemento.style.cssText = "color: red; background: blue;";
elemento.style.setProperty("--cor-primaria", "#007bff");
```

### Navegação no DOM
```javascript
// Navegação para cima
elemento.parentElement;
elemento.parentNode;
elemento.closest(".classe-pai"); // Encontra ancestral mais próximo

// Navegação para baixo
elemento.children; // Apenas elementos
elemento.childNodes; // Todos os nós (texto, comentários, etc)
elemento.firstElementChild;
elemento.lastElementChild;

// Navegação lateral
elemento.previousElementSibling;
elemento.nextElementSibling;
elemento.previousSibling; // Qualquer nó
elemento.nextSibling; // Qualquer nó
```

### Inserção e Remoção
```javascript
const pai = document.getElementById("pai");
const filho = document.createElement("div");
const referencia = document.getElementById("referencia");

// Inserção
pai.appendChild(filho); // Adiciona no final
pai.insertBefore(filho, referencia); // Antes do elemento referência
pai.prepend(filho); // Como primeiro filho (moderno)
pai.append(filho); // Como último filho (moderno)

// Inserção relativa
referencia.before(filho); // Antes do elemento
referencia.after(filho); // Depois do elemento

// Substituição
pai.replaceChild(novoFilho, filhoAntigo);
filhoAntigo.replaceWith(novoFilho); // Moderno

// Remoção
pai.removeChild(filho);
filho.remove(); // Moderno
filho.remove(); // Remove do DOM mas mantém na memória

// Clone
const clone = filho.cloneNode(true); // true = clona filhos também
```

### Manipulação de Formulários
```javascript
const formulario = document.forms.meuFormulario;

// Acesso a elementos
formulario.elements.nome;
formulario.elements["email"];

// Valores
formulario.elements.nome.value;
formulario.elements.ativo.checked;
formulario.elements.cor.value;

// Métodos
formulario.submit();
formulario.reset();

// Eventos de formulário
formulario.addEventListener("submit", (e) => {
  e.preventDefault(); // Evita envio padrão
  
  // Validação
  if (!formulario.elements.nome.value) {
    alert("Nome é obrigatório");
    return;
  }
  
  // Coleta de dados
  const dados = new FormData(formulario);
  const objetoDados = Object.fromEntries(dados);
  
  // Envio
  fetch("/api/submit", {
    method: "POST",
    body: JSON.stringify(objetoDados)
  });
});

// Validação nativa
formulario.elements.email.setCustomValidity("E-mail inválido");
formulario.checkValidity(); // Verifica validade
formulario.reportValidity(); // Mostra mensagens de erro
```

### Performance no DOM
```javascript
// DocumentFragment - manipulação off-DOM
const fragmento = document.createDocumentFragment();

for (let i = 0; i < 1000; i++) {
  const item = document.createElement("div");
  item.textContent = `Item ${i}`;
  fragmento.appendChild(item);
}

document.body.appendChild(fragmento); // Apenas um reflow

// innerHTML vs createElement
// innerHTML - mais rápido para conteúdo simples
lista.innerHTML = items.map(item => `<li>${item}</li>`).join("");

// createElement - mais controle e segurança
const fragmento2 = document.createDocumentFragment();
items.forEach(item => {
  const li = document.createElement("li");
  li.textContent = item;
  fragmento2.appendChild(li);
});
lista.appendChild(fragmento2);

// Debounce para eventos
function debounce(fn, delay) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn.apply(this, args), delay);
  };
}

window.addEventListener("resize", debounce(() => {
  console.log("Resize finalizado");
}, 250));
```

## Eventos

### Tipos de Eventos
```javascript
// Eventos de Mouse
elemento.addEventListener("click", handler);
elemento.addEventListener("dblclick", handler);
elemento.addEventListener("mousedown", handler);
elemento.addEventListener("mouseup", handler);
elemento.addEventListener("mousemove", handler);
elemento.addEventListener("mouseenter", handler); // Não propaga
elemento.addEventListener("mouseleave", handler); // Não propaga
elemento.addEventListener("mouseover", handler); // Propaga
elemento.addEventListener("mouseout", handler); // Propaga

// Eventos de Teclado
elemento.addEventListener("keydown", handler);
elemento.addEventListener("keypress", handler); // Depreciado
elemento.addEventListener("keyup", handler);

// Eventos de Formulário
formulario.addEventListener("submit", handler);
formulario.addEventListener("reset", handler);
input.addEventListener("focus", handler);
input.addEventListener("blur", handler);
input.addEventListener("change", handler);
input.addEventListener("input", handler);

// Eventos de Janela/Documento
window.addEventListener("load", handler);
window.addEventListener("DOMContentLoaded", handler);
window.addEventListener("resize", handler);
window.addEventListener("scroll", handler);
window.addEventListener("beforeunload", handler);

// Eventos de Mídia
video.addEventListener("play", handler);
video.addEventListener("pause", handler);
video.addEventListener("ended", handler);

// Eventos de Toque (Touch)
elemento.addEventListener("touchstart", handler);
elemento.addEventListener("touchmove", handler);
elemento.addEventListener("touchend", handler);
elemento.addEventListener("touchcancel", handler);

// Eventos Personalizados
const evento = new CustomEvent("meuEvento", {
  detail: { mensagem: "Olá Mundo" },
  bubbles: true,
  cancelable: true
});
elemento.dispatchEvent(evento);
```

### Manipulação de Eventos
```javascript
// Adicionar eventos
function handleClick(event) {
  console.log("Elemento clicado:", event.target);
}

// Métodos antigos (evitar)
elemento.onclick = handleClick;
elemento.onclick = function(event) { /* ... */ };

// Método moderno (recomendado)
elemento.addEventListener("click", handleClick);

// Evento com opções
elemento.addEventListener("click", handleClick, {
  once: true, // Executa apenas uma vez
  passive: true, // Indica que preventDefault() não será chamado
  capture: false // Fase de captura ou bubbling
});

// Múltiplos eventos
const handler = (e) => console.log(e.type);
elemento.addEventListener("click", handler);
elemento.addEventListener("mouseenter", handler);

// Parâmetros adicionais
elemento.addEventListener("click", (e) => handler(e, "extra"));

// Remover eventos
elemento.removeEventListener("click", handleClick);

// Remove todos os eventos
const clone = elemento.cloneNode(true);
elemento.parentNode.replaceChild(clone, elemento);
```

### Objeto Event
```javascript
function handleEvent(event) {
  // Informações básicas
  event.type; // Tipo do evento
  event.target; // Elemento que disparou
  event.currentTarget; // Elemento com listener
  event.eventPhase; // Fase do evento (1:captura, 2:alvo, 3:bubbling)
  
  // Posição do mouse
  event.clientX; // X relativo à janela
  event.clientY; // Y relativo à janela
  event.pageX; // X relativo ao documento
  event.pageY; // Y relativo ao documento
  event.screenX; // X relativo à tela
  event.screenY; // Y relativo à tela
  
  // Teclado
  event.key; // Tecla pressionada
  event.code; // Código físico da tecla
  event.ctrlKey; // Ctrl pressionado
  event.shiftKey; // Shift pressionado
  event.altKey; // Alt pressionado
  event.metaKey; // Meta/Command pressionado
  
  // Mouse
  event.button; // Botão do mouse (0:esquerdo, 1:meio, 2:direito)
  event.buttons; // Botões pressionados
  event.relatedTarget; // Elemento relacionado (mouseover/out)
  
  // Controle de fluxo
  event.preventDefault(); // Previne comportamento padrão
  event.stopPropagation(); // Para propagação
  event.stopImmediatePropagation(); // Para outros listeners
  
  // Tempo
  event.timeStamp; // Timestamp do evento
  
  return false; // Equivalente a preventDefault + stopPropagation
}
```

### Propagação de Eventos
```javascript
// HTML: <div id="pai"><button id="filho">Clique</button></div>

const pai = document.getElementById("pai");
const filho = document.getElementById("filho");

// Fases: 1. Captura (window → elemento) 2. Alvo 3. Bubbling (elemento → window)

// Bubbling (padrão)
pai.addEventListener("click", () => console.log("Pai (bubbling)"));
filho.addEventListener("click", () => console.log("Filho (bubbling)"));
// Clique no filho: "Filho" → "Pai"

// Captura
pai.addEventListener("click", () => console.log("Pai (captura)"), true);
filho.addEventListener("click", () => console.log("Filho (captura)"), true);
// Clique no filho: "Pai (captura)" → "Filho (captura)"

// Parar propagação
filho.addEventListener("click", (event) => {
  console.log("Filho clicado");
  event.stopPropagation(); // Impede bubbling/captura
});

// Event delegation
const lista = document.getElementById("lista");
lista.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    console.log("Item clicado:", event.target.textContent);
  }
});

// Evento uma vez
filho.addEventListener("click", () => {
  console.log("Executa apenas uma vez");
}, { once: true });
```

### Eventos Customizados
```javascript
// Criar evento customizado
const evento = new CustomEvent("loginSucesso", {
  detail: {
    usuario: "joao",
    timestamp: new Date()
  },
  bubbles: true,
  cancelable: true
});

// Disparar evento
document.dispatchEvent(evento);

// Escutar evento customizado
document.addEventListener("loginSucesso", (event) => {
  console.log("Usuário logado:", event.detail.usuario);
});

// Eventos com herança
class MeuEvento extends Event {
  constructor(mensagem) {
    super("meuEvento");
    this.mensagem = mensagem;
  }
}

const meuEvento = new MeuEvento("Olá!");
elemento.dispatchEvent(meuEvento);
```

### Performance com Eventos
```javascript
// Delegation para muitos elementos
document.addEventListener("click", (event) => {
  if (event.target.matches(".item")) {
    console.log("Item clicado");
  }
});

// Debounce para eventos frequentes
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

window.addEventListener("resize", debounce(() => {
  console.log("Tamanho da janela:", window.innerWidth);
}, 250));

// Throttle para eventos frequentes
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

window.addEventListener("scroll", throttle(() => {
  console.log("Scroll position:", window.scrollY);
}, 100));

// Eventos passivos para scroll/touch
elemento.addEventListener("touchstart", handler, { passive: true });
elemento.addEventListener("wheel", handler, { passive: true });

// Remover listeners quando não necessário
const controller = new AbortController();
elemento.addEventListener("click", handler, { signal: controller.signal });
controller.abort(); // Remove todos os listeners com este signal
```

## Async/Await e Promises

### Callbacks (Antigo Padrão)
```javascript
// Callback hell exemplo
function obterDados(callback) {
  setTimeout(() => {
    console.log("1. Dados obtidos");
    callback("dados");
  }, 1000);
}

function processarDados(dados, callback) {
  setTimeout(() => {
    console.log("2. Processando:", dados);
    callback("dados processados");
  }, 1000);
}

function salvarDados(dados, callback) {
  setTimeout(() => {
    console.log("3. Salvando:", dados);
    callback("sucesso");
  }, 1000);
}

// Pyramid of doom
obterDados((dados) => {
  processarDados(dados, (processados) => {
    salvarDados(processados, (resultado) => {
      console.log("4. Resultado:", resultado);
    });
  });
});
```

### Promises
```javascript
// Criando uma Promise
const minhaPromise = new Promise((resolve, reject) => {
  // Operação assíncrona
  const sucesso = true;
  
  if (sucesso) {
    resolve("Dados obtidos com sucesso");
  } else {
    reject(new Error("Falha na operação"));
  }
});

// Consumindo Promises
minhaPromise
  .then(resultado => {
    console.log("Sucesso:", resultado);
    return "novo valor";
  })
  .then(novoValor => {
    console.log("Segundo then:", novoValor);
  })
  .catch(erro => {
    console.error("Erro:", erro.message);
  })
  .finally(() => {
    console.log("Execução finalizada (sucesso ou erro)");
  });

// Métodos estáticos de Promise
Promise.resolve(42); // Promise resolvida
Promise.reject(new Error("Falha")); // Promise rejeitada

// Promise.all - todas resolvem ou uma rejeita
Promise.all([
  fetch("/api/user"),
  fetch("/api/posts"),
  fetch("/api/comments")
])
.then(([user, posts, comments]) => {
  console.log("Todos os dados:", { user, posts, comments });
})
.catch(erro => {
  console.error("Uma das requisições falhou:", erro);
});

// Promise.allSettled - espera todas finalizarem
Promise.allSettled([
  Promise.resolve("sucesso"),
  Promise.reject("erro"),
  Promise.resolve("outro sucesso")
])
.then(resultados => {
  resultados.forEach(resultado => {
    if (resultado.status === "fulfilled") {
      console.log("Sucesso:", resultado.value);
    } else {
      console.log("Erro:", resultado.reason);
    }
  });
});

// Promise.race - primeira que resolver ou rejeitar
Promise.race([
  new Promise(resolve => setTimeout(() => resolve("Rápido"), 500)),
  new Promise(resolve => setTimeout(() => resolve("Lento"), 1000))
])
.then(vencedor => {
  console.log("Vencedor:", vencedor); // "Rápido"
});

// Promise.any - primeira que resolver (ignora rejeições)
Promise.any([
  Promise.reject("Erro 1"),
  Promise.reject("Erro 2"),
  Promise.resolve("Sucesso")
])
.then(resultado => {
  console.log("Primeiro sucesso:", resultado); // "Sucesso"
})
.catch(erro => {
  console.error("Todos falharam:", erro.errors);
});
```

### Async/Await
```javascript
// Função async sempre retorna Promise
async function obterUsuario() {
  return { nome: "João", idade: 30 };
}

// Equivalente a:
function obterUsuario() {
  return Promise.resolve({ nome: "João", idade: 30 });
}

// Uso do await
async function buscarDados() {
  try {
    console.log("1. Iniciando busca...");
    
    // Aguarda a Promise resolver
    const resposta = await fetch("https://api.exemplo.com/dados");
    
    // Outro await
    const dados = await resposta.json();
    
    console.log("2. Dados recebidos:", dados);
    return dados;
  } catch (erro) {
    console.error("Erro na busca:", erro);
    throw erro; // Propaga o erro
  } finally {
    console.log("3. Busca finalizada");
  }
}

// IIFE async
(async () => {
  const dados = await buscarDados();
  console.log("Dados processados:", dados);
})();

// Await com Promise.all
async function buscarTudo() {
  const [usuario, posts, comentarios] = await Promise.all([
    fetch("/api/user").then(r => r.json()),
    fetch("/api/posts").then(r => r.json()),
    fetch("/api/comments").then(r => r.json())
  ]);
  
  return { usuario, posts, comentarios };
}

// Tratamento de erros em funções async
async function operacaoSegura() {
  try {
    const resultado = await operacaoRiscosa();
    return resultado;
  } catch (erro) {
    // Log ou tratamento alternativo
    console.warn("Operação falhou, usando valor padrão");
    return { valorPadrao: true };
  }
}
```

### Padrões Avançados
```javascript
// Timeout com Promise
function timeout(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => reject(new Error("Timeout")), ms);
  });
}

async function buscarComTimeout() {
  try {
    const resposta = await Promise.race([
      fetch("/api/lenta"),
      timeout(5000) // Timeout de 5 segundos
    ]);
    return await resposta.json();
  } catch (erro) {
    if (erro.message === "Timeout") {
      console.error("A requisição demorou muito");
    } else {
      console.error("Outro erro:", erro);
    }
  }
}

// Retry com exponential backoff
async function fetchComRetry(url, maxTentativas = 3) {
  for (let tentativa = 1; tentativa <= maxTentativas; tentativa++) {
    try {
      const resposta = await fetch(url);
      if (!resposta.ok) throw new Error(`HTTP ${resposta.status}`);
      return await resposta.json();
    } catch (erro) {
      if (tentativa === maxTentativas) throw erro;
      
      // Espera exponencial antes de tentar novamente
      const delay = Math.pow(2, tentativa) * 1000;
      console.log(`Tentativa ${tentativa} falhou, tentando novamente em ${delay}ms`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

// Pool de Promises (limitar concorrência)
class PromisePool {
  constructor(maxConcorrentes) {
    this.maxConcorrentes = maxConcorrentes;
    this.running = 0;
    this.queue = [];
  }
  
  async add(promiseFactory) {
    return new Promise((resolve, reject) => {
      this.queue.push({ promiseFactory, resolve, reject });
      this.next();
    });
  }
  
  next() {
    if (this.running >= this.maxConcorrentes || this.queue.length === 0) {
      return;
    }
    
    this.running++;
    const { promiseFactory, resolve, reject } = this.queue.shift();
    
    promiseFactory()
      .then(resolve)
      .catch(reject)
      .finally(() => {
        this.running--;
        this.next();
      });
  }
}

// Uso do pool
const pool = new PromisePool(3);
const urls = [...Array(10).keys()].map(i => `/api/item/${i}`);

urls.forEach(url => {
  pool.add(() => fetch(url).then(r => r.json()));
});
```

### Generators Async
```javascript
// Generator functions para async
async function* geradorAsync() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
  yield await Promise.resolve(3);
}

// Consumindo generator async
(async () => {
  for await (const valor of geradorAsync()) {
    console.log(valor); // 1, 2, 3
  }
})();

// Paginação com async generator
async function* paginar(apiUrl, paginaInicial = 1) {
  let pagina = paginaInicial;
  let temMais = true;
  
  while (temMais) {
    const resposta = await fetch(`${apiUrl}?page=${pagina}`);
    const dados = await resposta.json();
    
    yield dados.items;
    
    temMais = dados.hasMore;
    pagina++;
  }
}

// Uso da paginação
(async () => {
  for await (const itens of paginar("/api/products")) {
    console.log("Itens da página:", itens);
    // Processar itens
  }
})();
```

## Módulos

### Módulos ES6 (ESM)
```javascript
// modulo.js - Exportação
// Exportação nomeada
export const nome = "João";
export const idade = 30;
export function saudar() {
  return `Olá, ${nome}!`;
}

// Exportação padrão (apenas uma por módulo)
export default function soma(a, b) {
  return a + b;
}

// Exportação agregada
export { nome as nomeUsuario, idade };

// modulo.js - Importação
// Importação nomeada
import { nome, idade } from './modulo.js';
import { nome as nomeCompleto } from './modulo.js';

// Importação padrão
import soma from './modulo.js';

// Importação mista
import soma, { nome, idade } from './modulo.js';

// Importação de tudo
import * as modulo from './modulo.js';
console.log(modulo.nome, modulo.idade);

// Importação dinâmica
async function carregarModulo() {
  const modulo = await import('./modulo.js');
  console.log(modulo.nome);
}

// HTML
<script type="module" src="main.js"></script>
```

### Características dos Módulos ES6
```javascript
// 1. Strict mode por padrão
// 2. Escopo de módulo (não global)
// 3. Execução única
// 4. Top-level await
// 5. Import/export estáticos (exceto import dinâmico)

// Top-level await (ES2022)
const dados = await fetch('/api/data').then(r => r.json());
console.log(dados);

// Import.meta
console.log(import.meta.url); // URL do módulo

// Módulos em Workers
// worker.js
import { expensiveOperation } from './operations.js';
self.onmessage = (e) => {
  const result = expensiveOperation(e.data);
  self.postMessage(result);
};

// main.js
const worker = new Worker('./worker.js', { type: 'module' });
```

### CommonJS (Node.js)
```javascript
// modulo.js - Exportação
module.exports = {
  nome: "João",
  idade: 30,
  saudar: function() {
    return `Olá, ${this.nome}!`;
  }
};

// Ou exportação direta
exports.nome = "João";
exports.idade = 30;

// main.js - Importação
const modulo = require('./modulo.js');
const { nome, idade } = require('./modulo.js');

// Exportação padrão
module.exports = function soma(a, b) {
  return a + b;
};

// Importação
const soma = require('./modulo.js');
```

### Módulos Híbridos (Compatibilidade)
```javascript
// modulo.js - Suporte dual
const nome = "João";
const idade = 30;

function saudar() {
  return `Olá, ${nome}!`;
}

// Exportação para ES6
export { nome, idade, saudar };
export default saudar;

// Exportação para CommonJS
if (typeof module !== 'undefined' && module.exports) {
  module.exports = {
    nome,
    idade,
    saudar,
    default: saudar
  };
}

// package.json para Node.js com ES6 modules
{
  "name": "meu-pacote",
  "version": "1.0.0",
  "type": "module", // Habilita ES6 modules
  "exports": {
    ".": {
      "import": "./dist/esm/index.js",
      "require": "./dist/cjs/index.js"
    }
  }
}
```

### Bundlers e Module Systems
```javascript
// Webpack exemplo
// webpack.config.js
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
};

// Rollup exemplo
// rollup.config.js
export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'umd', // Universal Module Definition
    name: 'MeuModulo'
  }
};

// ESBuild exemplo
// build.js
require('esbuild').build({
  entryPoints: ['src/index.js'],
  bundle: true,
  outfile: 'dist/bundle.js',
  format: 'esm',
  minify: true
}).catch(() => process.exit(1));
```

### Boas Práticas com Módulos
```javascript
// 1. Estrutura de diretórios
/*
src/
  ├── components/
  │   ├── Button.js
  │   └── Modal.js
  ├── utils/
  │   ├── helpers.js
  │   └── validators.js
  ├── services/
  │   └── api.js
  └── index.js
*/

// 2. Barrels (index.js)
// utils/index.js
export * from './helpers.js';
export * from './validators.js';
export { default as formatDate } from './date.js';

// 3. Importação seletiva
import { helper1, helper2 } from './utils/helpers.js';

// 4. Lazy loading
const modulo = await import('./heavy-module.js');

// 5. Tree shaking friendly
// Mantenha exportações puras e sem side effects

// 6. Circular dependencies
// Evite dependências circulares

// 7. Path aliases (Webpack/Vite)
// vite.config.js
export default {
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@components': path.resolve(__dirname, 'src/components')
    }
  }
};

// Uso
import Button from '@components/Button';
```

## ES6+ Features

### Template Literals
```javascript
// Interpolação
const nome = "João";
const idade = 30;
const mensagem = `Olá, ${nome}! Você tem ${idade} anos.`;

// Multilinha
const texto = `
  Linha 1
  Linha 2
  Linha 3
`;

// Expressões
const a = 5;
const b = 10;
console.log(`${a} + ${b} = ${a + b}`);

// Tagged templates
function tag(strings, ...values) {
  console.log(strings); // ["Olá, ", "! Você tem ", " anos."]
  console.log(values); // ["João", 30]
  
  return strings.reduce((result, str, i) => {
    return result + str + (values[i] || '');
  }, '');
}

const resultado = tag`Olá, ${nome}! Você tem ${idade} anos.`;

// Exemplos úteis
// SQL queries
const usuarioId = 123;
const query = sql`
  SELECT * FROM usuarios
  WHERE id = ${usuarioId}
`;

// HTML escaping
function html(strings, ...values) {
  return strings.reduce((result, str, i) => {
    const value = values[i] || '';
    const escaped = String(value)
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;');
    return result + str + escaped;
  }, '');
}

const userInput = '<script>alert("XSS")</script>';
const safeHtml = html`<div>${userInput}</div>`;
```

### Destructuring
```javascript
// Array destructuring
const numeros = [1, 2, 3];
const [primeiro, segundo] = numeros; // primeiro=1, segundo=2
const [a, , c] = numeros; // a=1, c=3
const [head, ...tail] = numeros; // head=1, tail=[2,3]

// Valor padrão
const [x = 10, y = 20] = [1]; // x=1, y=20

// Troca de variáveis
let a = 1, b = 2;
[a, b] = [b, a]; // a=2, b=1

// Object destructuring
const usuario = { nome: "João", idade: 30, cidade: "SP" };
const { nome, idade } = usuario; // nome="João", idade=30
const { nome: nomeCompleto, cidade = "Rio" } = usuario;

// Renomeação com valor padrão
const { nome: username = "Visitante" } = {};

// Destructuring aninhado
const config = {
  server: {
    host: "localhost",
    port: 8080
  }
};
const { server: { host, port } } = config;

// Destructuring em parâmetros
function printUser({ nome, idade, cidade = "Desconhecida" }) {
  console.log(`${nome}, ${idade} anos, de ${cidade}`);
}

// Destructuring em loops
const usuarios = [{ nome: "João", idade: 30 }, { nome: "Maria", idade: 25 }];
for (const { nome, idade } of usuarios) {
  console.log(`${nome}: ${idade}`);
}
```

### Spread e Rest Operators
```javascript
// Spread em arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combinado = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]

// Spread em objetos
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }

// Override de propriedades
const defaults = { cor: "vermelho", tamanho: "medio" };
const personalizado = { ...defaults, cor: "azul" }; // { cor: "azul", tamanho: "medio" }

// Clone (shallow copy)
const original = { x: 1, y: 2 };
const copia = { ...original };

// Rest parameters
function soma(...numeros) {
  return numeros.reduce((acc, num) => acc + num, 0);
}
soma(1, 2, 3, 4); // 10

// Rest em destructuring
const [primeiro, segundo, ...resto] = [1, 2, 3, 4, 5]; // resto=[3,4,5]
const { a, b, ...outros } = { a: 1, b: 2, c: 3, d: 4 }; // outros={c:3,d:4}

// Spread em funções
const numeros = [1, 2, 3];
Math.max(...numeros); // Equivalente a Math.max(1, 2, 3)
```

### Enhanced Object Literals
```javascript
// Shorthand property names
const nome = "João";
const idade = 30;
const usuario = { nome, idade }; // { nome: "João", idade: 30 }

// Shorthand method names
const calculadora = {
  soma(a, b) {
    return a + b;
  },
  multiplicar(a, b) {
    return a * b;
  }
};

// Computed property names
const chave = "nome";
const obj = {
  [chave]: "João",
  ["idade" + 1]: 31, // idade1: 31
  [Symbol.iterator]: function*() {
    yield 1;
    yield 2;
    yield 3;
  }
};

// Method definitions em classes
class Retangulo {
  constructor(largura, altura) {
    this.largura = largura;
    this.altura = altura;
  }
  
  // Método
  area() {
    return this.largura * this.altura;
  }
  
  // Getter
  get diagonal() {
    return Math.sqrt(this.largura ** 2 + this.altura ** 2);
  }
  
  // Setter
  set escala(fator) {
    this.largura *= fator;
    this.altura *= fator;
  }
  
  // Método estático
  static criarQuadrado(lado) {
    return new Retangulo(lado, lado);
  }
}
```

### Optional Chaining e Nullish Coalescing
```javascript
// Optional chaining (?.) (ES2020)
const usuario = {
  profile: {
    name: "João",
    address: {
      city: "São Paulo"
    }
  }
};

// Acesso seguro a propriedades aninhadas
const cidade = usuario.profile?.address?.city; // "São Paulo"
const pais = usuario.profile?.address?.country; // undefined (sem erro)

// Com arrays
const primeiroItem = array?.[0];
const nome = usuario?.profile?.name?.toLowerCase();

// Com funções
const resultado = objeto.metodo?.();

// Nullish coalescing (??) (ES2020)
// Retorna o valor da direita apenas se o da esquerda for null ou undefined
const valor = null ?? "padrão"; // "padrão"
const valor2 = 0 ?? "padrao"; // 0 (0 não é null/undefined)
const valor3 = "" ?? "padrao"; // "" (string vazia não é null/undefined)

// Comparação com ||
const a = 0 || "padrão"; // "padrão" (0 é falsy)
const b = "" || "padrão"; // "padrão" ("" é falsy)
const c = 0 ?? "padrão"; // 0 (0 não é null/undefined)

// Combinação
const nome = usuario.profile?.name ?? "Visitante";
const idade = usuario.profile?.age ?? 18;
```

### Outras Features Modernas
```javascript
// Exponentiation operator (**) (ES2016)
2 ** 3; // 8
Math.pow(2, 3); // 8 (equivalente)

// Array.prototype.includes (ES2016)
[1, 2, 3].includes(2); // true
[1, 2, 3].includes(4); // false

// Object.values/Object.entries (ES2017)
const obj = { a: 1, b: 2, c: 3 };
Object.values(obj); // [1, 2, 3]
Object.entries(obj); // [["a", 1], ["b", 2], ["c", 3]]

// String padding (ES2017)
"5".padStart(3, "0"); // "005"
"5".padEnd(3, "0"); // "500"
"hello".padStart(10, "*"); // "*****hello"

// Object.getOwnPropertyDescriptors (ES2017)
const obj = { a: 1 };
Object.getOwnPropertyDescriptors(obj);
// {
//   a: { value: 1, writable: true, enumerable: true, configurable: true }
// }

// Trailing commas (ES2017)
const obj = {
  a: 1,
  b: 2, // vírgula final permitida
};

// Promise.prototype.finally (ES2018)
promise
  .then(result => { /* ... */ })
  .catch(error => { /* ... */ })
  .finally(() => { /* sempre executa */ });

// RegExp improvements (ES2018)
// Named capture groups
const regex = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const match = regex.exec("2023-12-25");
match.groups.year; // "2023"

// Lookbehind assertions
/(?<=\$)\d+/.test("$100"); // true
/(?<!\$)\d+/.test("€100"); // true

// s flag (dotAll)
/hello.world/s.test("hello\nworld"); // true

// Array.prototype.flat/flatMap (ES2019)
[1, [2, [3]]].flat(); // [1, 2, [3]]
[1, [2, [3]]].flat(2); // [1, 2, 3]
[1, 2, 3].flatMap(x => [x * 2]); // [2, 4, 6]

// Object.fromEntries (ES2019)
const entries = [["a", 1], ["b", 2]];
Object.fromEntries(entries); // { a: 1, b: 2 }

// String.prototype.trimStart/trimEnd (ES2019)
"  hello  ".trimStart(); // "hello  "
"  hello  ".trimEnd(); // "  hello"

// Optional catch binding (ES2019)
try {
  // código
} catch { // sem parâmetro de erro
  // tratamento genérico
}

// BigInt (ES2020)
const big = 9007199254740991n;
const alsoBig = BigInt(9007199254740991);
big + 1n; // 9007199254740992n

// globalThis (ES2020)
globalThis.setTimeout === window.setTimeout; // true em browsers
globalThis.setTimeout === global.setTimeout; // true em Node.js

// String.prototype.matchAll (ES2020)
const regex = /test(\d)/g;
const str = "test1test2";
const matches = [...str.matchAll(regex)];
// [["test1", "1"], ["test2", "2"]]

// Logical assignment operators (ES2021)
let x = 1;
x ||= 2; // x = x || 2
x &&= 3; // x = x && 3
x ??= 4; // x = x ?? 4

// Numeric separators (ES2021)
const billion = 1_000_000_000;
const binary = 0b1010_0001_1000_0101;
const hex = 0xA0_B0_C0;

// String.prototype.replaceAll (ES2021)
"hello world world".replaceAll("world", "earth"); // "hello earth earth"

// Promise.any (ES2021)
Promise.any([promise1, promise2]).then(first => { /* primeira que resolver */ });

// WeakRef e FinalizationRegistry (ES2021)
let obj = { data: "importante" };
const weakRef = new WeakRef(obj);
const registry = new FinalizationRegistry(heldValue => {
  console.log(`${heldValue} foi coletado pelo GC`);
});
registry.register(obj, "meuObjeto");

// Class fields (ES2022)
class MyClass {
  // Campos de instância
  publicField = "público";
  #privateField = "privado";
  
  // Campos estáticos
  static staticPublicField = "estático público";
  static #staticPrivateField = "estático privado";
  
  // Método privado
  #privateMethod() {
    return this.#privateField;
  }
  
  // Bloco estático
  static {
    console.log("Classe carregada");
  }
}

// Array find from last (ES2023)
const array = [1, 2, 3, 2, 1];
array.findLast(x => x === 2); // 2 (último)
array.findLastIndex(x => x === 2); // 3 (índice do último)

// Hashbang Grammer (ES2023)
#!/usr/bin/env node
console.log("Executável Node.js");
```

## Boas Práticas

### Código Limpo
```javascript
// Nomeação significativa
// Ruim
const d = new Date();
const a = [];

// Bom
const dataAtual = new Date();
const listaUsuarios = [];

// Funções pequenas e focadas
// Ruim
function processarUsuario(usuario) {
  // Validação
  if (!usuario.nome || usuario.nome.length < 2) {
    throw new Error("Nome inválido");
  }
  // Cálculo
  const idade = new Date().getFullYear() - usuario.nascimento.getFullYear();
  // Formatação
  return `${usuario.nome} (${idade} anos)`;
}

// Bom
function validarUsuario(usuario) {
  if (!usuario.nome || usuario.nome.length < 2) {
    throw new Error("Nome inválido");
  }
}

function calcularIdade(dataNascimento) {
  return new Date().getFullYear() - dataNascimento.getFullYear();
}

function formatarUsuario(usuario, idade) {
  return `${usuario.nome} (${idade} anos)`;
}

function processarUsuario(usuario) {
  validarUsuario(usuario);
  const idade = calcularIdade(usuario.nascimento);
  return formatarUsuario(usuario, idade);
}

// Evite side effects
// Ruim
let contador = 0;
function incrementar() {
  contador++; // Side effect
}

// Bom
function incrementar(valor) {
  return valor + 1; // Pura
}

// Use const por padrão, let quando necessário
// Ruim
var nome = "João";
var idade = 30;

// Bom
const nome = "João";
let idade = 30;
```

### Tratamento de Erros
```javascript
// Use Error objects
// Ruim
throw "Algo deu errado";

// Bom
throw new Error("Algo deu errado");

// Erros específicos
class ValidationError extends Error {
  constructor(mensagem, campo) {
    super(mensagem);
    this.name = "ValidationError";
    this.campo = campo;
  }
}

// Try-catch apropriado
async function buscarDados() {
  try {
    const resposta = await fetch("/api/dados");
    if (!resposta.ok) {
      throw new Error(`HTTP ${resposta.status}`);
    }
    return await resposta.json();
  } catch (erro) {
    if (erro.name === "TypeError") {
      console.error("Erro de rede:", erro);
    } else {
      console.error("Erro na resposta:", erro);
    }
    throw erro; // Re-throw para quem chamou
  }
}

// Finally para limpeza
function usarRecurso() {
  const recurso = adquirirRecurso();
  try {
    // Usar recurso
  } finally {
    liberarRecurso(recurso); // Sempre executa
  }
}

// Error boundaries em React
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    logErrorToService(error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Algo deu errado.</h1>;
    }
    return this.props.children;
  }
}
```

### Performance
```javascript
// Otimização de loops
// Ruim
for (let i = 0; i < array.length; i++) {
  // Acessa array.length a cada iteração
}

// Bom
for (let i = 0, len = array.length; i < len; i++) {
  // length cacheado
}

// Cache de seletores DOM
// Ruim
function atualizar() {
  document.querySelector(".item").style.color = "red";
  document.querySelector(".item").style.backgroundColor = "blue";
}

// Bom
function atualizar() {
  const item = document.querySelector(".item");
  item.style.color = "red";
  item.style.backgroundColor = "blue";
}

// Debounce e throttle
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Use Web Workers para tarefas pesadas
const worker = new Worker("worker.js");
worker.postMessage(dados);
worker.onmessage = (e) => {
  console.log("Resultado:", e.data);
};

// Lazy loading
// Importação dinâmica
const modulo = import.meta.env.PROD 
  ? await import("./modulo-pesado.js")
  : null;

// Virtualização de listas
function VirtualList({ items, itemHeight, renderItem }) {
  const [scrollTop, setScrollTop] = useState(0);
  const visibleItems = items.slice(
    Math.floor(scrollTop / itemHeight),
    Math.ceil(window.innerHeight / itemHeight)
  );
  
  return (
    <div onScroll={e => setScrollTop(e.target.scrollTop)}>
      {visibleItems.map(renderItem)}
    </div>
  );
}
```

### Segurança
```javascript
// Prevenção XSS
// Ruim
element.innerHTML = userInput;

// Bom
element.textContent = userInput;
// Ou
element.innerHTML = DOMPurify.sanitize(userInput);

// Validação de entrada
function validarEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

function sanitizarHTML(texto) {
  const div = document.createElement("div");
  div.textContent = texto;
  return div.innerHTML;
}

// Content Security Policy (CSP)
// meta tag no HTML
// <meta http-equiv="Content-Security-Policy" content="default-src 'self'">

// CORS seguro
// No servidor
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "https://meusite.com");
  res.header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE");
  res.header("Access-Control-Allow-Headers", "Content-Type, Authorization");
  next();
});

// Proteção CSRF
// Token em formulários
const csrfToken = generateToken();
formData.append("_csrf", csrfToken);

// Verificação no servidor
app.post("/api/dados", (req, res) => {
  if (req.body._csrf !== req.session.csrfToken) {
    return res.status(403).send("CSRF token inválido");
  }
  // Processar
});

// Rate limiting
const rateLimit = require("express-rate-limit");
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // limite por IP
});
app.use("/api/", limiter);
```

### Testes
```javascript
// Jest exemplo
// usuario.test.js
const { validarUsuario, formatarUsuario } = require("./usuario");

describe("Validação de usuário", () => {
  test("Deve aceitar usuário válido", () => {
    const usuario = { nome: "João", idade: 30 };
    expect(() => validarUsuario(usuario)).not.toThrow();
  });
  
  test("Deve rejeitar usuário sem nome", () => {
    const usuario = { idade: 30 };
    expect(() => validarUsuario(usuario)).toThrow("Nome inválido");
  });
});

// Testes assíncronos
test("Deve buscar dados da API", async () => {
  const dados = await buscarDados();
  expect(dados).toHaveProperty("id");
  expect(dados.nome).toBe("João");
});

// Mocks
jest.mock("./api", () => ({
  fetchUser: jest.fn(() => Promise.resolve({ id: 1, nome: "João" }))
}));

// Testes de componente React
import { render, screen, fireEvent } from "@testing-library/react";
import Button from "./Button";

test("Deve renderizar botão com texto", () => {
  render(<Button>Clique aqui</Button>);
  expect(screen.getByText("Clique aqui")).toBeInTheDocument();
});

test("Deve chamar onClick ao clicar", () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Clique</Button>);
  fireEvent.click(screen.getByText("Clique"));
  expect(handleClick).toHaveBeenCalledTimes(1);
});

// Testes de integração
test("Deve criar e deletar usuário", async () => {
  // Cria usuário
  const respostaCriacao = await criarUsuario({ nome: "Teste" });
  expect(respostaCriacao.status).toBe(201);
  
  // Deleta usuário
  const respostaDelecao = await deletarUsuario(respostaCriacao.data.id);
  expect(respostaDelecao.status).toBe(200);
});
```

## Recursos Úteis

### Ferramentas
- **ESLint**: Linter para JavaScript
- **Prettier**: Formatação de código
- **TypeScript**: JavaScript com tipos
- **Webpack/Vite**: Bundlers
- **Jest/Vitest**: Testes
- **Chrome DevTools**: Debugging
- **Node.js**: JavaScript no servidor

### Referências
- [MDN Web Docs](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
- [ECMAScript Specification](https://tc39.es/ecma262/)
- [JavaScript.info](https://javascript.info/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)

### Próximos Passos
1. Aprenda TypeScript
2. Explore frameworks (React, Vue, Angular)
3. Estude Node.js e backend
4. Aprofunde em testes
5. Conheça design patterns
6. Pratique algoritmos e estruturas de dados

---

**Nota:** Este manual é uma visão geral abrangente do JavaScript moderno. A linguagem está em constante evolução, então mantenha-se atualizado com as novas especificações do ECMAScript.
