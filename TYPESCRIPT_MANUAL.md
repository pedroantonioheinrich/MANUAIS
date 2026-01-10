# Manual Completo de TypeScript

## Sumário
1. [Introdução](#introdução)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Tipos Básicos](#tipos-básicos)
4. [Interfaces e Tipos Personalizados](#interfaces-e-tipos-personalizados)
5. [Classes e POO](#classes-e-poo)
6. [Funções](#funções)
7. [Genéricos](#genéricos)
8. [Módulos e Namespaces](#módulos-e-namespaces)
9. [Configuração do Compilador](#configuração-do-compilador)
10. [Ferramentas e Boas Práticas](#ferramentas-e-boas-práticas)

---

## Introdução

TypeScript é um superset tipado de JavaScript que compila para JavaScript puro. Desenvolvido pela Microsoft, adiciona tipagem estática opcional e outros recursos ao JavaScript.

**Principais vantagens:**
- Detecção de erros em tempo de compilação
- Melhor autocompletar em IDEs
- Código mais legível e manutenível
- Suporte a recursos modernos do ECMAScript

---

## Instalação e Configuração

### Instalação Global
```bash
npm install -g typescript
```

### Verificação da Versão
```bash
tsc --version
```

### Inicialização de Projeto
```bash
# Cria tsconfig.json
tsc --init

# Compilação básica
tsc arquivo.ts

# Modo observador (watch mode)
tsc --watch
```

### Instalação em Projeto
```bash
npm init -y
npm install typescript --save-dev
npx tsc --init
```

---

## Tipos Básicos

### Tipos Primitivos
```typescript
// Boolean
let isDone: boolean = false;

// Number
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;

// String
let color: string = "blue";
let fullName: string = `John ${lastName}`;

// Array
let list: number[] = [1, 2, 3];
let list2: Array<number> = [1, 2, 3]; // Generic

// Tuple
let tuple: [string, number] = ["hello", 10];

// Enum
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4
}
let c: Color = Color.Green;

// Any (evitar quando possível)
let notSure: any = 4;
notSure = "maybe a string";

// Void
function warnUser(): void {
  console.log("This is a warning");
}

// Null e Undefined
let u: undefined = undefined;
let n: null = null;

// Never
function error(message: string): never {
  throw new Error(message);
}

// Unknown (TypeScript 3.0+)
let value: unknown;
value = "Hello World";
if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

### Type Assertions
```typescript
let someValue: any = "this is a string";

// Sintaxe "as"
let strLength: number = (someValue as string).length;

// Sintaxe <tipo>
let strLength2: number = (<string>someValue).length;
```

---

## Interfaces e Tipos Personalizados

### Interface Básica
```typescript
interface User {
  name: string;
  age: number;
  email?: string; // Propriedade opcional
  readonly id: number; // Propriedade somente leitura
}

function printUser(user: User): void {
  console.log(`Name: ${user.name}, Age: ${user.age}`);
}
```

### Interface para Funções
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(src: string, sub: string): boolean {
  return src.search(sub) > -1;
};
```

### Interface para Objetos Indexáveis
```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];
```

### Type Aliases
```typescript
type Point = {
  x: number;
  y: number;
};

type ID = string | number;
type Callback = (data: string) => void;
```

### Diferenças entre Interface e Type
| Interface | Type |
|-----------|------|
| Pode ser extendida (`extends`) | Pode ser unida (`&`) |
| Pode ser implementada por classes | Não pode ser implementada |
| Pode ter declarações múltiplas | Declaração única |
| Mais usada para formas de objetos | Mais flexível |

---

## Classes e POO

### Classe Básica
```typescript
class Animal {
  // Propriedades
  private name: string;
  protected age: number;
  public species: string;
  
  // Construtor
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
    this.species = "Animal";
  }
  
  // Métodos
  move(distance: number = 0): void {
    console.log(`${this.name} moved ${distance}m.`);
  }
}

// Herança
class Dog extends Animal {
  breed: string;
  
  constructor(name: string, age: number, breed: string) {
    super(name, age);
    this.breed = breed;
    this.species = "Dog";
  }
  
  bark(): void {
    console.log("Woof! Woof!");
  }
  
  // Sobrescrever método
  move(distance = 5): void {
    console.log("Running...");
    super.move(distance);
  }
}
```

### Modificadores de Acesso
- `public`: Acessível em qualquer lugar (padrão)
- `private`: Acessível apenas dentro da classe
- `protected`: Acessível na classe e subclasses
- `readonly`: Propriedade somente leitura

### Propriedades de Parâmetro
```typescript
class Person {
  constructor(
    public name: string,
    private age: number,
    protected email: string
  ) {}
}
```

### Getters e Setters
```typescript
class Employee {
  private _salary: number = 0;
  
  get salary(): number {
    return this._salary;
  }
  
  set salary(value: number) {
    if (value < 0) {
      throw new Error("Salary cannot be negative");
    }
    this._salary = value;
  }
}
```

### Classes Abstratas
```typescript
abstract class Department {
  constructor(public name: string) {}
  
  abstract printMeeting(): void; // Método abstrato
  
  printName(): void {
    console.log("Department name: " + this.name);
  }
}

class AccountingDepartment extends Department {
  printMeeting(): void {
    console.log("Accounting Department meeting");
  }
}
```

### Implementação de Interface
```typescript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  
  setTime(d: Date): void {
    this.currentTime = d;
  }
}
```

### Modificador `readonly`
```typescript
class Octopus {
  readonly numberOfLegs: number = 8;
  constructor(readonly name: string) {}
}
```

---

## Funções

### Tipagem de Funções
```typescript
// Função nomeada
function add(x: number, y: number): number {
  return x + y;
}

// Expressão de função
let multiply: (x: number, y: number) => number = function(x, y) {
  return x * y;
};

// Arrow function
let divide = (x: number, y: number): number => x / y;
```

### Parâmetros Opcionais e Default
```typescript
function buildName(firstName: string, lastName?: string): string {
  return lastName ? `${firstName} ${lastName}` : firstName;
}

function createUser(name: string, age: number = 18): User {
  return { name, age };
}
```

### Parâmetros REST
```typescript
function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}
```

### Sobrecarga de Funções
```typescript
function reverse(value: string): string;
function reverse(value: number): number;
function reverse(value: string | number): string | number {
  if (typeof value === "string") {
    return value.split("").reverse().join("");
  } else {
    return parseInt(value.toString().split("").reverse().join(""));
  }
}
```

---

## Genéricos

### Funções Genéricas
```typescript
function identity<T>(arg: T): T {
  return arg;
}

// Uso
let output1 = identity<string>("myString");
let output2 = identity<number>(100);
let output3 = identity("myString"); // Inferência de tipo
```

### Classes Genéricas
```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
  
  constructor(zeroValue: T, add: (x: T, y: T) => T) {
    this.zeroValue = zeroValue;
    this.add = add;
  }
}

let myGenericNumber = new GenericNumber<number>(0, (x, y) => x + y);
```

### Constraints (Restrições)
```typescript
interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
```

### Genéricos com Múltiplos Tipos
```typescript
function merge<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = merge({ name: "John" }, { age: 30 });
```

### Utility Types
```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

// Partial
type PartialTodo = Partial<Todo>;

// Readonly
type ReadonlyTodo = Readonly<Todo>;

// Pick
type TodoPreview = Pick<Todo, "title" | "completed">;

// Omit
type TodoInfo = Omit<Todo, "description">;

// Record
type Pages = Record<"home" | "about", string>;
```

---

## Módulos e Namespaces

### Export/Import (ES6 Modules)
```typescript
// math.ts
export const PI = 3.14;

export function sum(a: number, b: number): number {
  return a + b;
}

export default class Calculator {
  // ...
}

// app.ts
import Calculator, { PI, sum } from './math';
import * as MathUtils from './math';
```

### Namespaces (Legado)
```typescript
namespace Geometry {
  export interface Point {
    x: number;
    y: number;
  }
  
  export function distance(p1: Point, p2: Point): number {
    return Math.sqrt((p2.x - p1.x) ** 2 + (p2.y - p1.y) ** 2);
  }
}

// Uso
let p1: Geometry.Point = { x: 0, y: 0 };
let dist = Geometry.distance(p1, { x: 3, y: 4 });
```

### Declaração de Módulos Externos
```typescript
// declarations.d.ts
declare module "jquery" {
  export function $(selector: string): any;
  export default $;
}
```

---

## Configuração do Compilador

### tsconfig.json Exemplo
```json
{
  "compilerOptions": {
    // Destino
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM"],
    
    // Diretórios
    "outDir": "./dist",
    "rootDir": "./src",
    
    // Estratégias de Módulo
    "moduleResolution": "node",
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"]
    },
    
    // Geração
    "sourceMap": true,
    "declaration": true,
    "declarationDir": "./types",
    
    // Strictness
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    
    // Qualidade de Código
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    
    // Interoperabilidade
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "resolveJsonModule": true,
    
    // Experimental
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  
  // Inclusão/Exclusão
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

### Opções Importantes
| Opção | Descrição | Recomendado |
|-------|-----------|-------------|
| `target` | Versão do JavaScript alvo | ES2020+ |
| `module` | Sistema de módulos | CommonJS (Node) / ESNext (Frontend) |
| `strict` | Ativa todas as verificações rigorosas | true |
| `noEmitOnError` | Não emitir arquivos se houver erros | true |
| `outDir` | Diretório de saída | ./dist |
| `rootDir` | Diretório raiz do código fonte | ./src |

---

## Ferramentas e Boas Práticas

### Ferramentas Recomendadas
1. **VS Code** - Editor oficial com suporte nativo
2. **ESLint** - Análise estática de código
3. **Prettier** - Formatação de código
4. **Jest/ts-jest** - Testes
5. **Webpack/Rollup** - Bundling

### Scripts do package.json
```json
{
  "scripts": {
    "build": "tsc",
    "build:watch": "tsc --watch",
    "build:prod": "tsc --project tsconfig.prod.json",
    "lint": "eslint src/**/*.ts",
    "test": "jest",
    "test:watch": "jest --watch",
    "format": "prettier --write \"src/**/*.ts\""
  }
}
```

### Boas Práticas
1. **Sempre usar tipagem explícita** em parâmetros e retornos públicos
2. **Evitar `any`** - usar `unknown` quando necessário
3. **Usar interfaces** para formas de objetos públicas
4. **Implementar tratamento de erros** com tipos específicos
5. **Configurar strict mode** desde o início do projeto

### Estrutura de Projeto Recomendada
```
projeto/
├── src/
│   ├── index.ts
│   ├── types/
│   │   ├── user.ts
│   │   └── api.ts
│   ├── services/
│   ├── components/
│   └── utils/
├── dist/
├── tests/
├── node_modules/
├── package.json
├── tsconfig.json
├── .eslintrc.js
└── README.md
```

### Debugging
```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug TypeScript",
      "program": "${workspaceFolder}/src/index.ts",
      "preLaunchTask": "tsc: build",
      "outFiles": ["${workspaceFolder}/dist/**/*.js"]
    }
  ]
}
```

### Migração de JavaScript para TypeScript
1. Renomeie arquivos `.js` para `.ts`
2. Corrija erros básicos de tipagem
3. Crie arquivos `.d.ts` para dependências sem tipos
4. Configure `allowJs: true` inicialmente
5. Aumente gradualmente o strictness

---

## Recursos Avançados

### Decorators
```typescript
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  
  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${key} with`, args);
    return originalMethod.apply(this, args);
  };
  
  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}
```

### Conditional Types
```typescript
type IsString<T> = T extends string ? true : false;
type A = IsString<string>; // true
type B = IsString<number>; // false
```

### Mapped Types
```typescript
type OptionsFlags<T> = {
  [Property in keyof T]: boolean;
};

type FeatureFlags = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<FeatureFlags>;
// { darkMode: boolean; newUserProfile: boolean; }
```

---

## Conclusão

TypeScript é uma ferramenta poderosa que traz segurança de tipos e produtividade ao desenvolvimento JavaScript. Comece com configurações básicas e aumente gradualmente o strictness conforme a equipe se familiariza.

**Próximos passos:**
1. Pratique com projetos pequenos
2. Explore o ecossistema (React + TS, Node + TS)
3. Contribua com definições de tipos no DefinitelyTyped
4. Acompanhe as novas versões (releases semestrais)

**Recursos Oficiais:**
- [Documentação Oficial](https://www.typescriptlang.org/docs/)
- [Playground Online](https://www.typescriptlang.org/play)
- [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

---
*Última atualização: Janeiro 2024*
*Versão do TypeScript: 5.0+*
