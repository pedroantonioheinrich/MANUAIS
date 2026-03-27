---

# Manual Completo de WebAssembly para Desenvolvedores JavaScript

## 1. Introdução: JavaScript + WebAssembly

WebAssembly não veio para substituir JavaScript, mas para complementá-lo. Enquanto JavaScript é excelente para I/O, manipulação de DOM e lógica de aplicação, WebAssembly brilha em computação pesada, processamento de dados e tarefas que exigem performance previsível.

**A relação ideal:**
- **JavaScript**: UI, lógica de negócio, integrações, manipulação de DOM
- **WebAssembly**: algoritmos pesados, processamento de dados, simulações, jogos

## 2. Fundamentos da API WebAssembly

### 2.1 Objetos Globais

```javascript
// Objeto principal
console.log(typeof WebAssembly); // 'object'

// Componentes principais
WebAssembly.Module      // Representa um módulo Wasm compilado
WebAssembly.Instance    // Instância executável de um módulo
WebAssembly.Memory      // Memória linear compartilhada
WebAssembly.Table       // Tabela de referências (funções)
WebAssembly.CompileError // Erro durante compilação
WebAssembly.LinkError    // Erro durante instanciação
WebAssembly.RuntimeError // Erro durante execução
```

### 2.2 Ciclo de Vida de um Módulo

```javascript
// 1. Carregar bytes
const response = await fetch('module.wasm');
const bytes = await response.arrayBuffer();

// 2. Compilar (opcional, pode ser combinado)
const module = await WebAssembly.compile(bytes);

// 3. Instanciar com imports
const instance = await WebAssembly.instantiate(module, imports);

// 4. Usar exports
instance.exports.minhaFuncao();
```

## 3. Carregamento de Módulos Wasm

### 3.1 Métodos de Carregamento

```javascript
// Método 1: Compilar e instanciar separadamente
const module = await WebAssembly.compile(bytes);
const instance = await WebAssembly.instantiate(module, imports);

// Método 2: Compilar e instanciar em um passo
const result = await WebAssembly.instantiate(bytes, imports);
const { instance, module } = result;

// Método 3: Stream (mais eficiente para arquivos grandes)
const module = await WebAssembly.compileStreaming(fetch('module.wasm'));
const instance = await WebAssembly.instantiateStreaming(fetch('module.wasm'), imports);

// Método 4: Módulo já compilado
const module = new WebAssembly.Module(bytes);
const instance = new WebAssembly.Instance(module, imports);
```

### 3.2 Tratamento de Erros

```javascript
try {
  const module = await WebAssembly.compile(bytes);
  const instance = await WebAssembly.instantiate(module, imports);
} catch (error) {
  if (error instanceof WebAssembly.CompileError) {
    console.error('Erro de compilação:', error.message);
  } else if (error instanceof WebAssembly.LinkError) {
    console.error('Erro de linkagem (imports inválidos):', error.message);
  } else if (error instanceof WebAssembly.RuntimeError) {
    console.error('Erro em runtime:', error.message);
  } else {
    console.error('Erro desconhecido:', error);
  }
}
```

## 4. Importações e Exportações

### 4.1 Exportações: O que o Wasm Disponibiliza

```javascript
// Exemplo de módulo Wasm que exporta funções, memória e global
// module.wasm (compilado de C/Rust/AssemblyScript)

const instance = await WebAssembly.instantiate(bytes);

// Exportações disponíveis
console.log(Object.keys(instance.exports));
// ['add', 'multiply', 'memory', 'MAX_SIZE']

// Chamar funções
const sum = instance.exports.add(5, 3);        // Retorna 8
const product = instance.exports.multiply(4, 6); // Retorna 24

// Acessar memória exportada
const memory = instance.exports.memory;
const buffer = new Uint8Array(memory.buffer);

// Acessar global exportada
const maxSize = instance.exports.MAX_SIZE.value; // Acessa valor da global
```

### 4.2 Importações: O que o JavaScript Fornece

```javascript
// JavaScript pode fornecer funções, memória, tabelas e globais para Wasm

const importObject = {
  // Funções JS que Wasm pode chamar
  env: {
    log: (value) => console.log(`Wasm log: ${value}`),
    getRandom: () => Math.random(),
    setTimeout: (ms) => new Promise(resolve => setTimeout(resolve, ms))
  },
  
  // Memória compartilhada (criada pelo JS)
  memory: new WebAssembly.Memory({ initial: 10, maximum: 100 }),
  
  // Tabela de funções
  table: new WebAssembly.Table({ initial: 0, element: 'anyfunc' }),
  
  // Globais
  global: new WebAssembly.Global({ value: 'i32', mutable: true }, 42)
};

const instance = await WebAssembly.instantiate(bytes, importObject);
```

### 4.3 Tipos de Dados em Importações/Exportações

| Tipo Wasm | Tipo JavaScript | Descrição |
|-----------|-----------------|-----------|
| i32 | Number (inteiro 32-bit) | Inteiro com sinal |
| i64 | BigInt | Inteiro 64-bit (não suportado diretamente) |
| f32 | Number | Float 32-bit |
| f64 | Number | Float 64-bit |
| externref | Qualquer valor JS | Referência arbitrária |
| funcref | Função JS | Referência a função |

## 5. Trabalhando com Memória Compartilhada

### 5.1 Conceitos Fundamentais

A memória Wasm é um `ArrayBuffer` linear que pode ser acessado tanto pelo Wasm quanto pelo JavaScript. Isso permite comunicação eficiente sem serialização.

```javascript
// Criar memória com 1 página (64KB), máximo 10 páginas (640KB)
const memory = new WebAssembly.Memory({
  initial: 1,
  maximum: 10,
  shared: false  // true para memória compartilhada (threads)
});

// O Wasm precisa importar esta memória
const instance = await WebAssembly.instantiate(bytes, { env: { memory } });
```

### 5.2 Acessando Memória do JavaScript

```javascript
// Memória exportada pelo Wasm
const memory = instance.exports.memory;
const buffer = memory.buffer;

// Ler como diferentes tipos de dados
const u8 = new Uint8Array(buffer);    // bytes
const u32 = new Uint32Array(buffer);  // inteiros 32-bit
const f64 = new Float64Array(buffer); // doubles

// Escrever na memória (Wasm enxerga as mudanças)
u8[0] = 42;
u32[1] = 123456;
f64[2] = 3.14159;

// Redimensionar memória (Wasm enxerga o novo buffer)
if (memory.grow(1)) {  // Adiciona 1 página (64KB)
  // buffer antigo é descartado, usar novo buffer
  const newBuffer = memory.buffer;
  const newU8 = new Uint8Array(newBuffer);
}
```

### 5.3 Padrões de Comunicação com Memória

```javascript
// Padrão 1: Wasm escreve resultado na memória, JS lê
// Wasm (C):
// int* processData(int* input, int length) { ... }

const memory = instance.exports.memory;
const inputData = new Int32Array(memory.buffer, 0, 100);
const outputData = new Int32Array(memory.buffer, 400, 100);

// Preencher dados de entrada
for (let i = 0; i < 100; i++) {
  inputData[i] = i * 2;
}

// Chamar função Wasm que processa e escreve resultado
instance.exports.processData(0, 100, 400); // endereços na memória

// Ler resultados
for (let i = 0; i < 100; i++) {
  console.log(outputData[i]);
}
```

### 5.4 Alocação Dinâmica de Memória

```javascript
// Módulo Wasm que exporta funções de alocação (como malloc/free)
const { malloc, free, processData } = instance.exports;

// Alocar memória para 1000 inteiros (4000 bytes)
const ptr = malloc(1000 * 4);

// Acessar memória alocada
const view = new Int32Array(memory.buffer, ptr, 1000);
for (let i = 0; i < 1000; i++) {
  view[i] = i;
}

// Processar dados
processData(ptr, 1000);

// Liberar memória
free(ptr);
```

## 6. Comunicação Avançada

### 6.1 Passando Strings entre JS e Wasm

```javascript
// Funções utilitárias para manipular strings com memória Wasm
class WasmStringUtils {
  constructor(instance) {
    this.instance = instance;
    this.memory = instance.exports.memory;
    this.encoder = new TextEncoder();
    this.decoder = new TextDecoder();
  }
  
  // Enviar string do JS para Wasm
  sendString(str) {
    // Assumindo que Wasm exporta malloc e free
    const bytes = this.encoder.encode(str);
    const ptr = this.instance.exports.malloc(bytes.length + 1);
    
    const heap = new Uint8Array(this.memory.buffer);
    heap.set(bytes, ptr);
    heap[ptr + bytes.length] = 0; // null terminator
    
    return ptr;
  }
  
  // Receber string do Wasm
  receiveString(ptr) {
    const heap = new Uint8Array(this.memory.buffer);
    let end = ptr;
    while (heap[end] !== 0) end++;
    
    const bytes = heap.slice(ptr, end);
    return this.decoder.decode(bytes);
  }
  
  // Liberar memória alocada
  freeString(ptr) {
    this.instance.exports.free(ptr);
  }
}

// Exemplo de uso
const stringUtils = new WasmStringUtils(instance);
const namePtr = stringUtils.sendString("John Doe");
instance.exports.greet(namePtr);
const greeting = stringUtils.receiveString(instance.exports.getGreeting());
stringUtils.freeString(namePtr);
```

### 6.2 Passando Objetos Complexos

```javascript
// Serializar objetos JavaScript para memória linear
class WasmSerializer {
  constructor(memory) {
    this.memory = memory;
    this.view = new DataView(memory.buffer);
  }
  
  serializeObject(obj) {
    const json = JSON.stringify(obj);
    const bytes = new TextEncoder().encode(json);
    const ptr = this.allocate(bytes.length + 1);
    
    const heap = new Uint8Array(this.memory.buffer);
    heap.set(bytes, ptr);
    heap[ptr + bytes.length] = 0;
    
    return ptr;
  }
  
  deserializeObject(ptr) {
    const heap = new Uint8Array(this.memory.buffer);
    let end = ptr;
    while (heap[end] !== 0) end++;
    
    const json = new TextDecoder().decode(heap.slice(ptr, end));
    return JSON.parse(json);
  }
  
  allocate(size) {
    // Assumindo que Wasm exporta uma função de alocação
    return this.instance.exports.alloc(size);
  }
}
```

### 6.3 Callbacks: Wasm chamando JavaScript

```javascript
// JavaScript fornece função callback para Wasm
let callbackId = 0;
const callbacks = new Map();

// Função que Wasm pode chamar para registrar callbacks
function registerCallback(callback) {
  const id = callbackId++;
  callbacks.set(id, callback);
  return id;
}

// Função que Wasm chama para executar callbacks
function executeCallback(id, data) {
  const callback = callbacks.get(id);
  if (callback) {
    callback(data);
  }
}

// Import object
const imports = {
  env: {
    registerCallback,
    executeCallback,
    console_log: (msg) => console.log(msg)
  }
};

// No Wasm (exemplo em WAT):
// (func $processData (param $callbackId i32) (param $data i32)
//   (call $executeCallback (local.get $callbackId) (local.get $data))
// )

const instance = await WebAssembly.instantiate(bytes, imports);

// Registrar callback do JavaScript
const callbackId = registerCallback((data) => {
  console.log('Callback recebeu:', data);
});

// Chamar função Wasm que usará o callback
instance.exports.processData(callbackId, 42);
```

## 7. Performance e Otimização

### 7.1 Medindo Performance

```javascript
// Comparar JavaScript vs WebAssembly
function benchmark(fn, iterations = 1000000) {
  const start = performance.now();
  for (let i = 0; i < iterations; i++) {
    fn(i);
  }
  return performance.now() - start;
}

// Função JS
function jsAdd(a, b) {
  return a + b;
}

// Função Wasm
const wasmAdd = instance.exports.add;

console.log('JS:', benchmark(() => jsAdd(5, 3)));
console.log('Wasm:', benchmark(() => wasmAdd(5, 3)));
```

### 7.2 Minimizando Overhead de Chamada

```javascript
// ❌ Ruim: chamadas frequentes com overhead
for (let i = 0; i < 1000000; i++) {
  instance.exports.smallOperation(i);
}

// ✅ Bom: processar em lote
const batch = new Int32Array(memory.buffer, 0, 1000000);
for (let i = 0; i < 1000000; i++) {
  batch[i] = i;
}
instance.exports.processBatch(0, 1000000); // Única chamada
```

### 7.3 Cache de Referências

```javascript
// ❌ Ruim: acessar exports repetidamente
for (let i = 0; i < 1000; i++) {
  instance.exports.processItem(i);
}

// ✅ Bom: cachear referência
const processItem = instance.exports.processItem;
for (let i = 0; i < 1000; i++) {
  processItem(i);
}
```

## 8. WebAssembly com Threads (Web Workers)

```javascript
// main.js
async function setupWasmWorkers(numWorkers) {
  const workers = [];
  const wasmModule = await WebAssembly.compileStreaming(fetch('module.wasm'));
  
  for (let i = 0; i < numWorkers; i++) {
    const worker = new Worker('worker.js');
    
    // Enviar módulo compilado para o worker (transferência zero-copy)
    worker.postMessage({ type: 'init', module: wasmModule }, [wasmModule]);
    
    workers.push(worker);
  }
  
  return workers;
}

// worker.js
let instance = null;

self.onmessage = async (event) => {
  if (event.data.type === 'init') {
    // Instanciar módulo recebido
    instance = await WebAssembly.instantiate(event.data.module);
    self.postMessage({ type: 'ready' });
  } else if (event.data.type === 'compute') {
    // Executar computação pesada
    const result = instance.exports.compute(event.data.data);
    self.postMessage({ type: 'result', result });
  }
};
```

## 9. Depuração e Ferramentas

### 9.1 Depuração no Navegador

```javascript
// Ativar source maps (se disponíveis)
// No Chrome DevTools: Sources → Enable JavaScript source maps

// Logging de chamadas Wasm
WebAssembly.instantiate = new Proxy(WebAssembly.instantiate, {
  apply: async (target, thisArg, args) => {
    console.log('Instanciando Wasm:', args[0]);
    const result = await target.apply(thisArg, args);
    console.log('Exports:', result.instance.exports);
    return result;
  }
});
```

### 9.2 Análise de Performance

```javascript
// Marcar seções de código para profiling
performance.mark('wasm-start');
instance.exports.heavyComputation();
performance.mark('wasm-end');

performance.measure('wasm-execution', 'wasm-start', 'wasm-end');
const measure = performance.getEntriesByName('wasm-execution')[0];
console.log(`Wasm execution: ${measure.duration}ms`);
```

### 9.3 Inspetor de Memória

```javascript
// Monitorar uso de memória Wasm
function monitorWasmMemory(instance, interval = 1000) {
  const memory = instance.exports.memory;
  
  setInterval(() => {
    const buffer = memory.buffer;
    const pages = buffer.byteLength / 65536;
    const used = buffer.byteLength;
    
    console.log({
      pages: pages,
      bytes: used,
      mb: (used / 1024 / 1024).toFixed(2)
    });
  }, interval);
}
```

## 10. Boas Práticas e Padrões

### 10.1 Encapsulamento com Classes

```javascript
class WasmModule {
  constructor(wasmUrl, imports = {}) {
    this.wasmUrl = wasmUrl;
    this.imports = imports;
    this.instance = null;
    this.memory = null;
  }
  
  async load() {
    const { instance } = await WebAssembly.instantiateStreaming(
      fetch(this.wasmUrl),
      this.imports
    );
    
    this.instance = instance;
    this.memory = instance.exports.memory;
    return this;
  }
  
  call(name, ...args) {
    if (!this.instance) {
      throw new Error('Módulo não carregado');
    }
    return this.instance.exports[name](...args);
  }
  
  getMemoryView(type, offset, length) {
    if (!this.memory) return null;
    
    const buffer = this.memory.buffer;
    const constructors = {
      Uint8: Uint8Array,
      Int32: Int32Array,
      Float64: Float64Array
    };
    
    const Constructor = constructors[type];
    return new Constructor(buffer, offset, length);
  }
}

// Uso
const wasm = new WasmModule('module.wasm');
await wasm.load();
console.log(wasm.call('add', 5, 3));
```

### 10.2 Pool de Instâncias

```javascript
class WasmInstancePool {
  constructor(wasmUrl, poolSize = 4) {
    this.wasmUrl = wasmUrl;
    this.poolSize = poolSize;
    this.available = [];
    this.allInstances = [];
  }
  
  async initialize() {
    const module = await WebAssembly.compileStreaming(fetch(this.wasmUrl));
    
    for (let i = 0; i < this.poolSize; i++) {
      const instance = await WebAssembly.instantiate(module);
      this.available.push(instance);
      this.allInstances.push(instance);
    }
  }
  
  acquire() {
    if (this.available.length === 0) {
      throw new Error('No available instances');
    }
    return this.available.pop();
  }
  
  release(instance) {
    this.available.push(instance);
  }
  
  async execute(fn) {
    const instance = this.acquire();
    try {
      return await fn(instance);
    } finally {
      this.release(instance);
    }
  }
}
```

### 10.3 Fallback para JavaScript

```javascript
class WasmWithFallback {
  constructor(wasmUrl, jsImplementation) {
    this.wasmUrl = wasmUrl;
    this.jsImpl = jsImplementation;
    this.wasmImpl = null;
    this.useWasm = false;
  }
  
  async init() {
    try {
      const { instance } = await WebAssembly.instantiateStreaming(
        fetch(this.wasmUrl)
      );
      this.wasmImpl = instance.exports;
      this.useWasm = true;
    } catch (error) {
      console.warn('Wasm falhou, usando fallback JS:', error);
      this.useWasm = false;
    }
  }
  
  compute(...args) {
    if (this.useWasm && this.wasmImpl.compute) {
      return this.wasmImpl.compute(...args);
    }
    return this.jsImpl.compute(...args);
  }
}
```

## 11. Casos de Uso Práticos

### 11.1 Processamento de Imagem

```javascript
// Filtrar imagem com Wasm (mais rápido que JS)
class ImageProcessor {
  constructor(wasmModule) {
    this.module = wasmModule;
  }
  
  async applyGrayscale(imageData) {
    const { width, height, data } = imageData;
    const memory = this.module.memory;
    
    // Copiar dados da imagem para memória Wasm
    const ptr = this.module.alloc(data.length);
    const heap = new Uint8ClampedArray(memory.buffer, ptr, data.length);
    heap.set(data);
    
    // Processar em Wasm
    this.module.grayscale(ptr, width, height);
    
    // Copiar resultado de volta
    const result = new Uint8ClampedArray(heap);
    this.module.free(ptr);
    
    return new ImageData(result, width, height);
  }
}
```

### 11.2 Criptografia

```javascript
// Usar Wasm para operações criptográficas (mais seguras e rápidas)
class CryptoWasm {
  async hashPassword(password, salt) {
    const passwordPtr = this.encodeString(password);
    const saltPtr = this.encodeString(salt);
    
    const hashPtr = this.instance.exports.hash(
      passwordPtr,
      saltPtr
    );
    
    const hash = this.decodeString(hashPtr);
    this.instance.exports.free(passwordPtr);
    this.instance.exports.free(saltPtr);
    this.instance.exports.free(hashPtr);
    
    return hash;
  }
}
```

## 12. Considerações Finais

### Vantagens de usar Wasm com JavaScript:
- **Performance**: até 10-100x mais rápido para operações intensivas
- **Reutilização de código**: use bibliotecas C/C++/Rust existentes
- **Segurança**: sandbox isolado
- **Portabilidade**: funciona em qualquer navegador moderno

### Desvantagens e Limitações:
- **Overhead de comunicação**: chamadas entre JS e Wasm têm custo
- **Debugging**: mais complexo que JavaScript puro
- **Tamanho**: binários Wasm podem ser grandes
- **Acesso limitado**: não pode acessar DOM diretamente

### Quando usar Wasm:
- Cálculos matemáticos intensivos
- Processamento de áudio/vídeo
- Jogos e simulações
- Compressão/descompressão
- Criptografia
- Emulação de outras plataformas

### Quando não usar Wasm:
- Manipulação DOM intensiva
- Aplicações simples com pouca computação
- Quando o overhead de comunicação supera o ganho

---

*Este manual reflete o estado atual da integração WebAssembly com JavaScript. Para informações atualizadas, consulte a documentação oficial do WebAssembly (webassembly.org) e a especificação da API WebAssembly nos navegadores.*
