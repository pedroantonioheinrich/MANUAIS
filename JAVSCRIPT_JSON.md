## **Manual Completo de Manipulação de JSON com JavaScript**

### **1. O que é JSON?**
JSON (JavaScript Object Notation) é um formato leve de troca de dados, fácil para humanos lerem e escreverem, e fácil para máquinas interpretarem.

### **2. Estrutura Básica do JSON**

```javascript
// Objeto JSON
{
  "nome": "João",
  "idade": 30,
  "email": "joao@email.com"
}

// Array JSON
[
  {"nome": "João", "idade": 30},
  {"nome": "Maria", "idade": 25}
]
```

### **3. Tipos de Dados Suportados**

```javascript
{
  "string": "texto",
  "number": 42,
  "float": 3.14,
  "boolean": true,
  "null": null,
  "array": [1, 2, 3],
  "object": {"chave": "valor"}
}
```

### **4. Conversão JSON ↔ JavaScript**

#### **4.1 JSON para JavaScript (Parsing)**
```javascript
// String JSON
const jsonString = '{"nome":"João","idade":30,"ativo":true}';

// Converter para objeto JavaScript
const objetoJS = JSON.parse(jsonString);
console.log(objetoJS.nome); // "João"
console.log(objetoJS.idade); // 30

// Com array JSON
const arrayJson = '[{"produto":"Notebook","preco":2500},{"produto":"Mouse","preco":50}]';
const produtos = JSON.parse(arrayJson);
console.log(produtos[0].produto); // "Notebook"
```

#### **4.2 JavaScript para JSON (Stringify)**
```javascript
// Objeto JavaScript
const usuario = {
  nome: "Maria",
  idade: 28,
  habilidades: ["JavaScript", "Python"],
  endereco: {
    cidade: "São Paulo",
    estado: "SP"
  }
};

// Converter para string JSON
const jsonString2 = JSON.stringify(usuario);
console.log(jsonString2);
// {"nome":"Maria","idade":28,"habilidades":["JavaScript","Python"],"endereco":{"cidade":"São Paulo","estado":"SP"}}
```

### **5. Tratamento de Erros**

```javascript
// Try-catch para parsing seguro
function parseJSONSeguro(jsonString) {
  try {
    const objeto = JSON.parse(jsonString);
    return objeto;
  } catch (erro) {
    console.error("Erro ao parsear JSON:", erro.message);
    return null;
  }
}

// Exemplo
const jsonInvalido = '{nome: "João"}'; // JSON inválido (faltam aspas)
const resultado = parseJSONSeguro(jsonInvalido); // Retorna null e mostra erro
```

### **6. Parâmetros Avançados do JSON.stringify()**

#### **6.1 Substituidor (Replacer)**
```javascript
const dados = {
  nome: "Ana",
  idade: 25,
  senha: "123456",
  email: "ana@email.com"
};

// Filtrar propriedades
const jsonFiltrado = JSON.stringify(dados, ["nome", "email"]);
console.log(jsonFiltrado); // {"nome":"Ana","email":"ana@email.com"}

// Usando função replacer
const jsonComFiltro = JSON.stringify(dados, (chave, valor) => {
  if (chave === "senha") return undefined; // Remove senha
  if (chave === "idade") return valor + " anos"; // Modifica idade
  return valor;
});
console.log(jsonComFiltro);
```

#### **6.2 Espaçamento (Formatter)**
```javascript
const pessoa = {
  nome: "Carlos",
  idade: 35,
  hobbies: ["ler", "correr", "nadar"]
};

// JSON formatado com 2 espaços
const jsonFormatado = JSON.stringify(pessoa, null, 2);
console.log(jsonFormatado);
/* Resultado:
{
  "nome": "Carlos",
  "idade": 35,
  "hobbies": [
    "ler",
    "correr",
    "nadar"
  ]
}
*/

// Com caracteres personalizados
const jsonCustom = JSON.stringify(pessoa, null, "---");
```

### **7. Parâmetros Avançados do JSON.parse()**

```javascript
// Função reviver
const jsonData = '{"nome":"Pedro","dataNascimento":"1990-05-15","valor":"100"}';

const objetoRevivido = JSON.parse(jsonData, (chave, valor) => {
  if (chave === "dataNascimento") return new Date(valor);
  if (chave === "valor") return Number(valor);
  return valor;
});

console.log(objetoRevivido.dataNascimento instanceof Date); // true
console.log(typeof objetoRevivido.valor); // "number"
```

### **8. Operações Comuns com JSON**

#### **8.1 Clonar Objetos**
```javascript
// Clone profundo (deep copy)
const original = { a: 1, b: { c: 2 } };
const clone = JSON.parse(JSON.stringify(original));

clone.b.c = 3;
console.log(original.b.c); // 2 (não foi alterado)
console.log(clone.b.c);    // 3
```

#### **8.2 Comparar Objetos**
```javascript
function objetosIguais(obj1, obj2) {
  return JSON.stringify(obj1) === JSON.stringify(obj2);
}

const objA = { nome: "João", idade: 30 };
const objB = { nome: "João", idade: 30 };
const objC = { nome: "Maria", idade: 25 };

console.log(objetosIguais(objA, objB)); // true
console.log(objetosIguais(objA, objC)); // false
```

### **9. Trabalhando com JSON em Arquivos (Node.js)**

```javascript
// Node.js - Escrever JSON em arquivo
const fs = require('fs');

const dadosJson = {
  usuarios: [
    { id: 1, nome: "Ana" },
    { id: 2, nome: "Bruno" }
  ]
};

// Escrever arquivo
fs.writeFileSync('dados.json', JSON.stringify(dadosJson, null, 2));

// Ler arquivo
const conteudo = fs.readFileSync('dados.json', 'utf-8');
const dadosLidos = JSON.parse(conteudo);
console.log(dadosLidos.usuarios);
```

### **10. JSON e APIs (Fetch)**

```javascript
// Enviar JSON para API
async function enviarDados() {
  const dados = {
    nome: "João Silva",
    email: "joao@email.com"
  };

  const response = await fetch('https://api.exemplo.com/usuarios', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(dados)
  });

  const resposta = await response.json();
  console.log(resposta);
}

// Receber JSON de API
async function buscarDados() {
  try {
    const response = await fetch('https://api.exemplo.com/usuarios');
    const dados = await response.json(); // Parse automático
    console.log(dados);
  } catch (erro) {
    console.error("Erro na requisição:", erro);
  }
}
```

### **11. Validação de JSON**

```javascript
function isValidJSON(str) {
  try {
    JSON.parse(str);
    return true;
  } catch (e) {
    return false;
  }
}

// Testes
console.log(isValidJSON('{"nome":"João"}')); // true
console.log(isValidJSON('{nome:"João"}'));   // false
console.log(isValidJSON('[1,2,3]'));         // true
console.log(isValidJSON('texto simples'));    // false
```

### **12. Dicas e Boas Práticas**

```javascript
// ❌ Evite
const ruim = JSON.parse(jsonString); // Sem tratamento de erro

// ✅ Prefira
let dados;
try {
  dados = JSON.parse(jsonString);
} catch (error) {
  dados = {}; // Valor padrão
  console.error("Erro ao processar JSON:", error);
}

// ❌ Evite funções em JSON
const comFuncao = {
  nome: "Teste",
  executar: function() { return "ok"; }
};
// Funções são perdidas no JSON.stringify()

// ✅ Use apenas dados serializáveis
const serializavel = {
  nome: "Teste",
  acao: "executar" // String em vez de função
};

// ❌ Evite referências circulares
const obj = { nome: "teste" };
obj.ref = obj; // Referência circular
// JSON.stringify(obj) // Erro!

// ✅ Use bibliotecas como flatted para casos complexos
```

### **13. JSON vs JavaScript Object Literal**

```javascript
// JSON (requer aspas duplas nas chaves)
const jsonValido = '{"nome":"João","idade":30}';

// JavaScript Object (aspas opcionais)
const objetoJs = { nome: "João", idade: 30 };

// Diferenças:
// - JSON: aspas duplas obrigatórias
// - JSON: sem comentários
// - JSON: sem funções
// - JSON: sem undefined
// - JSON: sem referências circulares
```

### **14. Exemplo Prático Completo**

```javascript
// Sistema de gerenciamento de tarefas
class GerenciadorTarefas {
  constructor() {
    this.tarefas = [];
    this.carregarTarefas();
  }

  // Salvar tarefas no localStorage
  salvarTarefas() {
    const jsonString = JSON.stringify(this.tarefas, null, 2);
    localStorage.setItem('tarefas', jsonString);
  }

  // Carregar tarefas do localStorage
  carregarTarefas() {
    const dados = localStorage.getItem('tarefas');
    if (dados) {
      this.tarefas = JSON.parse(dados);
    }
  }

  // Adicionar tarefa
  adicionarTarefa(titulo, descricao) {
    const novaTarefa = {
      id: Date.now(),
      titulo,
      descricao,
      concluida: false,
      criadaEm: new Date().toISOString()
    };
    this.tarefas.push(novaTarefa);
    this.salvarTarefas();
    return novaTarefa;
  }

  // Listar tarefas
  listarTarefas(filtro = null) {
    if (filtro === 'concluidas') {
      return this.tarefas.filter(t => t.concluida);
    } else if (filtro === 'pendentes') {
      return this.tarefas.filter(t => !t.concluida);
    }
    return this.tarefas;
  }

  // Exportar para JSON
  exportarJSON() {
    return JSON.stringify(this.tarefas, null, 2);
  }

  // Importar de JSON
  importarJSON(jsonString) {
    try {
      const tarefasImportadas = JSON.parse(jsonString);
      if (Array.isArray(tarefasImportadas)) {
        this.tarefas = tarefasImportadas;
        this.salvarTarefas();
        return true;
      }
    } catch (error) {
      console.error("Erro ao importar:", error);
    }
    return false;
  }
}

// Uso do sistema
const gerenciador = new GerenciadorTarefas();
gerenciador.adicionarTarefa("Estudar JSON", "Estudar manipulação de JSON");
gerenciador.adicionarTarefa("Praticar JavaScript", "Resolver exercícios");

console.log(gerenciador.exportarJSON());
```

### **15. Performance e Limitações**

```javascript
// Limitações de tamanho (browser)
// Evite stringify de objetos muito grandes

// Performance para objetos grandes
console.time("stringify");
const objGrande = { /* muitos dados */ };
JSON.stringify(objGrande);
console.timeEnd("stringify");

// Uso de reviver pode impactar performance
// Para grandes volumes, considere streaming
