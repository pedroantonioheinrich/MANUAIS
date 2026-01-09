# Manual Completo do Bun

## Índice
1. [Introdução ao Bun](#introdução-ao-bun)
2. [Instalação](#instalação)
3. [Gerenciador de Pacotes](#gerenciador-de-pacotes)
4. [Runtime JavaScript/TypeScript](#runtime-javascripttypescript)
5. [Bundler](#bundler)
6. [Test Runner](#test-runner)
7. [Ferramentas Adicionais](#ferramentas-adicionais)
8. [Integração com Frameworks](#integração-com-frameworks)
9. [Configurações Avançadas](#configurações-avançadas)
10. [Migração de Node.js](#migração-de-nodejs)
11. [Referência de Comandos](#referência-de-comandos)

---

## Introdução ao Bun

Bun é um toolkit JavaScript/TypeScript all-in-one que inclui:
- **Runtime**: Alternativa ao Node.js e Deno
- **Package Manager**: Substitui npm, yarn, pnpm
- **Bundler**: Similar ao webpack, esbuild, parcel
- **Test Runner**: Compatível com Jest

### Principais Características
- ⚡ **Extremamente rápido** (escrito em Zig)
- 🧪 **Suporte nativo a TypeScript e JSX**
- 📦 **Compatibilidade com Node.js**
- 🔌 **Plugins para extensibilidade**
- 🔧 **Ferramentas integradas**

---

## Instalação

### macOS e Linux
```bash
# Via curl
curl -fsSL https://bun.sh/install | bash

# Via npm
npm install -g bun
```

### Windows
```bash
# Via PowerShell
powershell -c "irm bun.sh/install.ps1 | iex"

# Via Winget
winget install Bun.Bun
```

### Verificar Instalação
```bash
bun --version
```

### Atualização
```bash
bun upgrade
```

---

## Gerenciador de Pacotes

### Comandos Básicos

```bash
# Inicializar novo projeto
bun init

# Adicionar dependência
bun add <package>
bun add <package>@<version>
bun add <package> --dev  # dependência de desenvolvimento
bun add <package> --optional  # dependência opcional

# Remover dependência
bun remove <package>

# Instalar dependências do package.json
bun install
bun install --frozen-lockfile  # instalação estrita

# Atualizar pacotes
bun update
bun update <package>

# Executar scripts
bun run <script-name>
```

### Características do Gerenciador
- **Lockfile**: `bun.lockb` (binário, mais rápido)
- **Cache global**: Armazenamento eficiente
- **Workspaces**: Suporte nativo
- **Scripts**: Compatível com npm scripts

### Comandos Avançados
```bash
# Limpar cache
bun pm cache rm

# Listar pacotes instalados
bun pm ls

# Verificar vulnerabilidades
bun audit
```

---

## Runtime JavaScript/TypeScript

### Executar Arquivos
```bash
# Executar arquivo JavaScript/TypeScript
bun run index.js
bun run index.ts
bun run index.jsx
bun run index.tsx

# Modo watch
bun --watch index.ts

# Modo hot reload
bun --hot index.ts

# Habilitar debug
bun --inspect index.ts
```

### API do Runtime
Bun inclui APIs otimizadas:

```javascript
// Servidor HTTP integrado
Bun.serve({
  port: 3000,
  fetch(request) {
    return new Response("Hello Bun!");
  }
});

// Manipulação de arquivos
await Bun.write("file.txt", "Hello!");
const file = Bun.file("file.txt");
const text = await file.text();

// Variáveis de ambiente
Bun.env.API_KEY;

// Processamento de dados
const data = Bun.hash("hello");
```

### Tipos de Suporte
- ✅ CommonJS e ES Modules
- ✅ TypeScript (.ts, .tsx)
- ✅ JSX/TSX
- ✅ JSON, WASM, Textos

---

## Bundler

### Comandos de Build
```bash
# Bundle de arquivo único
bun build ./index.ts --outdir ./dist

# Bundle para diferentes alvos
bun build ./index.ts --target=browser
bun build ./index.ts --target=node
bun build ./index.ts --target=bun

# Minificar output
bun build ./index.ts --minify

# Gerar sourcemaps
bun build ./index.ts --sourcemap
```

### Configuração via `bunfig.toml`
```toml
[build]
entrypoints = ["./src/index.ts"]
outdir = "./dist"
target = "browser"
minify = true
sourcemap = "external"

# Configurações de loader
[build.loader]
".svg" = "text"
".custom" = "file"
```

### Exemplo de Configuração
```typescript
// bundle.ts
import { build } from "bun";

await build({
  entrypoints: ["./src/index.ts"],
  outdir: "./dist",
  minify: true,
  target: "browser",
  splitting: true,
});
```

---

## Test Runner

### Configuração Básica
```bash
# Executar todos os testes
bun test

# Executar testes específicos
bun test <pattern>

# Modo watch
bun test --watch

# Gerar relatório de cobertura
bun test --coverage
```

### Escrevendo Testes
```typescript
// math.test.ts
import { expect, test } from "bun:test";
import { sum } from "./math";

test("adiciona 1 + 2 para igual 3", () => {
  expect(sum(1, 2)).toBe(3);
});

test("descreção do teste", () => {
  // beforeEach, afterEach, beforeAll, afterAll disponíveis
});

// Testes assíncronos
test("fetch data", async () => {
  const response = await fetch("https://api.example.com");
  expect(response.ok).toBe(true);
});
```

### APIs de Teste
```typescript
// Matchers
expect(value).toBe(expected);
expect(value).toEqual(expected);
expect(value).toBeTruthy();
expect(value).toContain(item);
expect(value).toThrow();
expect(value).toHaveLength(n);

// Mocking
import { mock } from "bun:test";

const fn = mock(() => 42);
expect(fn).toHaveBeenCalled();
```

### Configuração Avançada
```javascript
// bunfig.toml para testes
[test]
timeout = 5000
preload = ["./test/setup.ts"]
```

---

## Ferramentas Adicionais

### Bun Shell
```bash
# Executar comandos shell
bunx shell

# Em scripts TypeScript
import { $ } from "bun";

await $`ls -la`;
const result = await $`echo hello`;
```

### Transpiler
```bash
# Transpilar TypeScript para JavaScript
bun build --compile ./script.ts
```

### REPL
```bash
# Iniciar REPL interativo
bun repl

# Executar código inline
bun -e "console.log('Hello')"
```

### Gerenciador de Scripts
```json
{
  "scripts": {
    "dev": "bun run --watch server.ts",
    "build": "bun build ./src/index.ts",
    "test": "bun test",
    "start": "bun run server.ts"
  }
}
```

---

## Integração com Frameworks

### React/Vite
```bash
# Criar projeto React
bun create react ./my-app
cd my-app
bun install
bun dev

# Com Vite
bun create vite my-app --template react
```

### Next.js
```bash
# Criar projeto Next.js
bun create next-app my-app
cd my-app
bun dev
```

### Express.js
```typescript
import express from "express";
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from Bun!");
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

### Elysia (Framework para Bun)
```typescript
import { Elysia } from "elysia";

const app = new Elysia()
  .get("/", () => "Hello Elysia")
  .post("/json", (req) => req.body)
  .listen(3000);
```

### Configurações Específicas
```bash
# Verificar compatibilidade
bun why <package>

# Verificar se pacote é otimizado para Bun
bunx jsr add @package/name
```

---

## Configurações Avançadas

### Configuração Global (`bunfig.toml`)
```toml
# Configurações globais
[install]
# Usar registry específico
registry = "https://registry.npmjs.org"
# Configurar scoped registries
scopes = [
  { scope = "@myorg", url = "https://registry.myorg.com" }
]

# Configurações de proxy
[proxy]
http = "http://proxy:8080"
https = "http://proxy:8080"

# Configurações de log
[log]
level = "info"

# Variáveis de ambiente globais
[env]
NODE_ENV = "production"
API_KEY = "${API_KEY}" # variável do sistema
```

### Otimizações de Performance
```toml
[smol]
# Habilitar alocação de memória otimizada
enable = true

[bundle]
# Otimizações específicas
external = ["react", "react-dom"]
```

### Plugins
```typescript
// Criando plugins
import type { BunPlugin } from "bun";

const myPlugin: BunPlugin = {
  name: "Custom Loader",
  setup(build) {
    build.onLoad({ filter: /\.custom$/ }, (args) => {
      return {
        contents: `export default ${JSON.stringify(args.path)}`,
        loader: "js",
      };
    });
  },
};

// Usando plugins no build
await build({
  entrypoints: ["./src/index.ts"],
  plugins: [myPlugin],
});
```

### WebSockets
```typescript
// Suporte nativo a WebSocket
Bun.serve({
  fetch(req, server) {
    if (server.upgrade(req)) {
      return;
    }
    return new Response("Hello");
  },
  websocket: {
    message(ws, message) {},
    open(ws) {},
    close(ws, code, reason) {},
  },
});
```

---

## Migração de Node.js

### Passos para Migração
1. **Instalar Bun** no projeto
2. **Remover `node_modules`** e lockfiles antigos
3. **Instalar dependências** com Bun
4. **Atualizar scripts** no package.json
5. **Testar aplicação**

### Comandos de Migração
```bash
# Migrar de npm/yarn
rm -rf node_modules package-lock.json yarn.lock
bun install

# Verificar compatibilidade
bun pm node-compat
```

### APIs Incompatíveis
- Alguns módulos nativos do Node podem precisar de polyfills
- APIs específicas do Bun podem substituir as do Node
- Verificar módulos que usam addons nativos

### Configuração de Compatibilidade
```toml
# bunfig.toml
[compat]
# Forçar compatibilidade com Node.js
node = true

# Polyfills automáticos
polyfills = [
  "stream",
  "util",
  "fs"
]
```

---

## Referência de Comandos

### Comandos Principais
| Comando | Descrição |
|---------|-----------|
| `bun install` | Instala dependências |
| `bun add <pkg>` | Adiciona pacote |
| `bun remove <pkg>` | Remove pacote |
| `bun run <script>` | Executa script |
| `bun test` | Executa testes |
| `bun build` | Cria bundle |
| `bun upgrade` | Atualiza Bun |
| `bun create` | Cria novo projeto |
| `bun x` | Executa pacote |
| `bun repl` | Abre REPL |

### Opções Comuns
| Flag | Descrição |
|------|-----------|
| `--version` | Versão do Bun |
| `--help` | Ajuda |
| `--watch` | Modo observação |
| `--hot` | Hot reload |
| `--inspect` | Debugger |
| `--minify` | Minificar output |
| `--sourcemap` | Gerar sourcemaps |
| `--target` | Definir target |
| `--outdir` | Diretório de saída |
| `--env-file` | Arquivo .env |

### Comandos de Desenvolvimento
```bash
# Desenvolvimento com recarga automática
bun --watch src/index.ts

# Profile de performance
bun --profile script.ts

# Verificar uso de memória
bun --mem script.ts

# Executar com mais flags de debug
bun --inspect --inspect-wait script.ts
```

### Troubleshooting
```bash
# Resetar cache
bun pm cache clear

# Verificar problemas
bun doctor

# Verificar versões
bun --version
bun pm bin

# Logs detalhados
bun install --verbose
```

---

## Recursos Adicionais

### Links Úteis
- [Documentação Oficial](https://bun.sh/docs)
- [GitHub](https://github.com/oven-sh/bun)
- [Discord](https://bun.sh/discord)
- [Exemplos](https://github.com/oven-sh/bun/tree/main/examples)

### Dicas de Performance
1. Use `bun install` em vez de npm/yarn
2. Utilize cache do Bun para builds
3. Prefira ES modules sobre CommonJS
4. Use plugins para otimizações específicas
5. Aproveite APIs nativas do Bun quando possível

### Suporte
- Issues no GitHub
- Comunidade Discord
- Stack Overflow com tag `bunjs`
- Documentação oficial

---

*Este manual foi atualizado para Bun v1.0+*
*Para atualizações, consulte a documentação oficial*
