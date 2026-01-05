# 📘 Manual Completo de Node.js
## O Guia Definitivo para Desenvolvimento Backend com JavaScript

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![NPM](https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white)

## 📋 Sumário

- [🚀 Introdução ao Node.js](#-introdução-ao-nodejs)
- [⚙️ Instalação e Configuração](#️-instalação-e-configuração)
- [📚 Fundamentos do Node.js](#-fundamentos-do-nodejs)
- [🔄 Módulos do Node.js](#-módulos-do-nodejs)
- [📦 NPM - Gerenciador de Pacotes](#-npm---gerenciador-de-pacotes)
- [🌐 HTTP e Servidores Web](#-http-e-servidores-web)
- [🗃️ Trabalhando com Arquivos](#️-trabalhando-com-arquivos)
- [🔄 Streams e Buffers](#-streams-e-buffers)
- [⚡ Event Loop e Assincronia](#-event-loop-e-assincronia)
- [🔐 Segurança em Node.js](#-segurança-em-nodejs)
- [📡 Express.js Framework](#-expressjs-framework)
- [🗄️ Bancos de Dados](#️-bancos-de-dados)
- [🧪 Testes em Node.js](#-testes-em-nodejs)
- [🐳 Docker e Node.js](#-docker-e-nodejs)
- [🚀 Deploy e Produção](#-deploy-e-produção)
- [🔧 Boas Práticas](#-boas-práticas)
- [🎯 Projetos Práticos](#-projetos-práticos)

---

## 🚀 Introdução ao Node.js

### O que é Node.js?
Node.js é um ambiente de execução JavaScript do lado do servidor, construído no motor V8 do Chrome. Permite executar JavaScript fora do navegador.

### Características Principais:
- **Assíncrono e Orientado a Eventos**: Non-blocking I/O
- **Single-threaded**: Usa um único thread com event loop
- **Cross-platform**: Windows, Linux, macOS
- **NPM**: Maior ecossistema de pacotes do mundo

### Arquitetura Node.js:
```
┌─────────────────────────────────────────┐
│            Aplicação Node.js            │
├─────────────────────────────────────────┤
│                Node.js Core             │
│  ┌───────────────────────────────────┐  │
│  │         V8 JavaScript Engine      │  │
│  └───────────────────────────────────┘  │
│  ┌───────────────────────────────────┐  │
│  │      libuv (Event Loop, I/O)      │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

### Quando usar Node.js?
✅ **Bom para:** APIs RESTful, aplicações em tempo real, microsserviços, ferramentas CLI  
❌ **Evitar para:** Processamento CPU intensivo, aplicações com muitos cálculos

---

## ⚙️ Instalação e Configuração

### 1. Instalação no Windows/Mac/Linux

**Windows/Mac:**
- Baixe o instalador em [nodejs.org](https://nodejs.org/)
- Versão LTS (Long Term Support) recomendada

**Linux (Ubuntu/Debian):**
```bash
# Usando NodeSource
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verificar instalação
node --version  # v18.x.x
npm --version   # 9.x.x
```

**Linux via NVM (Node Version Manager):**
```bash
# Instalar NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Recarregar terminal
source ~/.bashrc  # ou ~/.zshrc

# Instalar Node.js
nvm install 18  # Última LTS
nvm use 18
nvm alias default 18
```

### 2. Verificação da Instalação
```bash
# Verificar versões
node -v
npm -v
npx -v

# Criar arquivo de teste
echo "console.log('Node.js funcionando!')" > teste.js
node teste.js
```

### 3. Configuração do Ambiente

**Configurar NPM:**
```bash
# Configurar usuário
npm set init-author-name "Seu Nome"
npm set init-author-email "seu@email.com"
npm set init-license "MIT"

# Configurar registry (opcional para Brasil)
npm config set registry https://registry.npmjs.org/

# Ver configurações
npm config list
```

**Arquivo .npmrc (configurações do projeto):**
```ini
# ~/.npmrc ou .npmrc no projeto
registry=https://registry.npmjs.org/
save-exact=true
package-lock=true
fund=false
```

---

## 📚 Fundamentos do Node.js

### 1. Hello World Básico
```javascript
// app.js
console.log('Hello Node.js!');

// Executar
// node app.js
```

### 2. Sistema de Módulos

**CommonJS (padrão Node.js):**
```javascript
// modulo.js
module.exports = {
  nome: 'Meu Módulo',
  versao: '1.0.0',
  saudacao: function() {
    return `Olá do ${this.nome}!`;
  }
};

// app.js
const meuModulo = require('./modulo.js');
console.log(meuModulo.saudacao());
```

**ES Modules (ESM):**
```javascript
// modulo.mjs
export const nome = 'Módulo ES6';
export function saudacao() {
  return `Olá do ${nome}!`;
}

// app.mjs
import { nome, saudacao } from './modulo.mjs';
console.log(saudacao());
```

### 3. Process Object
```javascript
// Informações do processo
console.log('Process ID:', process.pid);
console.log('Node version:', process.version);
console.log('Platform:', process.platform);
console.log('Current directory:', process.cwd());
console.log('Environment:', process.env.NODE_ENV);

// Argumentos de linha de comando
console.log('Args:', process.argv);

// Eventos do processo
process.on('exit', (code) => {
  console.log(`Processo finalizado com código: ${code}`);
});

// Sair do processo
process.exit(0); // 0 = sucesso, 1 = erro
```

### 4. Console API Avançada
```javascript
// console.log básico
console.log('Mensagem normal');

// console.error para erros
console.error('Erro crítico!');

// console.warn para avisos
console.warn('Aviso importante');

// console.table para objetos/arrays
const usuarios = [
  { nome: 'João', idade: 25 },
  { nome: 'Maria', idade: 30 }
];
console.table(usuarios);

// console.time para medir performance
console.time('tempo-execucao');
// Código a ser medido
console.timeEnd('tempo-execucao');

// console.trace para stack trace
function funcaoA() {
  funcaoB();
}
function funcaoB() {
  console.trace('Rastreamento da pilha');
}
funcaoA();
```

### 5. Timers
```javascript
// setTimeout (executa uma vez após delay)
const timeoutId = setTimeout(() => {
  console.log('Executado após 2 segundos');
}, 2000);

// clearTimeout para cancelar
// clearTimeout(timeoutId);

// setInterval (executa repetidamente)
const intervalId = setInterval(() => {
  console.log('Executado a cada 1 segundo');
}, 1000);

// clearInterval para parar
setTimeout(() => {
  clearInterval(intervalId);
  console.log('Intervalo parado após 5 segundos');
}, 5000);

// setImmediate (executa no próximo ciclo do event loop)
setImmediate(() => {
  console.log('Executado imediatamente');
});

// process.nextTick (executa antes do próximo ciclo)
process.nextTick(() => {
  console.log('Executado no próximo tick');
});
```

### 6. Global Objects
```javascript
// __dirname e __filename
console.log('Diretório atual:', __dirname);
console.log('Arquivo atual:', __filename);

// Buffer (trabalhar com dados binários)
const buf = Buffer.from('Hello Node.js');
console.log('Buffer:', buf);
console.log('Buffer como string:', buf.toString());

// URL parsing
const url = new URL('https://exemplo.com:8080/path?query=value#hash');
console.log('URL Object:', {
  protocol: url.protocol,
  hostname: url.hostname,
  port: url.port,
  pathname: url.pathname,
  search: url.search,
  hash: url.hash
});
```

---

## 🔄 Módulos do Node.js

### Módulos Nativos Principais

#### 1. fs - File System
```javascript
const fs = require('fs');
const fsPromises = require('fs').promises;

// Leitura síncrona (NÃO USAR em produção)
try {
  const data = fs.readFileSync('./arquivo.txt', 'utf8');
  console.log('Conteúdo:', data);
} catch (err) {
  console.error('Erro na leitura:', err);
}

// Leitura assíncrona com callback
fs.readFile('./arquivo.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Erro:', err);
    return;
  }
  console.log('Conteúdo:', data);
});

// Leitura assíncrona com Promises (recomendado)
async function lerArquivo() {
  try {
    const data = await fsPromises.readFile('./arquivo.txt', 'utf8');
    console.log('Conteúdo:', data);
  } catch (err) {
    console.error('Erro:', err);
  }
}

// Escrita de arquivo
fs.writeFile('./novo-arquivo.txt', 'Conteúdo do arquivo', (err) => {
  if (err) {
    console.error('Erro:', err);
    return;
  }
  console.log('Arquivo criado!');
});

// Operações com diretórios
fs.mkdir('./novo-diretorio', { recursive: true }, (err) => {
  if (err) throw err;
  console.log('Diretório criado');
});

// Listar arquivos
fs.readdir('./', (err, files) => {
  if (err) throw err;
  console.log('Arquivos:', files);
});

// Estatísticas
fs.stat('./arquivo.txt', (err, stats) => {
  if (err) throw err;
  console.log('É arquivo?', stats.isFile());
  console.log('É diretório?', stats.isDirectory());
  console.log('Tamanho:', stats.size);
});
```

#### 2. path - Trabalhando com Caminhos
```javascript
const path = require('path');

// Juntar caminhos (cross-platform)
const caminhoCompleto = path.join(__dirname, 'pasta', 'arquivo.txt');
console.log('Caminho completo:', caminhoCompleto);

// Resolver caminhos absolutos
const absoluto = path.resolve('pasta', 'arquivo.txt');
console.log('Caminho absoluto:', absoluto);

// Extrair informações
const exemplo = '/usr/local/bin/node';
console.log('Diretório:', path.dirname(exemplo));     // /usr/local/bin
console.log('Nome do arquivo:', path.basename(exemplo)); // node
console.log('Extensão:', path.extname(exemplo));      // vazio

// Normalizar caminhos
console.log(path.normalize('/usr//local/../bin'));    // /usr/bin

// Parsing de caminho
const parsed = path.parse('/home/user/dir/file.txt');
console.log('Parsed path:', parsed);
// {
//   root: '/',
//   dir: '/home/user/dir',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file'
// }

// Formatar caminho
const formato = path.format({
  dir: '/home/user/dir',
  base: 'file.txt'
});
console.log('Path formatado:', formato); // /home/user/dir/file.txt
```

#### 3. os - Informações do Sistema Operacional
```javascript
const os = require('os');

// Informações do sistema
console.log('Sistema operacional:', os.platform());
console.log('Arquitetura:', os.arch());
console.log('Versão do OS:', os.version());
console.log('Tipo de release:', os.release());

// Informações da CPU
console.log('CPUs:', os.cpus().length);
console.log('Modelo da CPU:', os.cpus()[0].model);
console.log('Velocidade da CPU:', os.cpus()[0].speed + 'MHz');

// Memória
console.log('Memória total:', os.totalmem() / 1024 / 1024 / 1024 + 'GB');
console.log('Memória livre:', os.freemem() / 1024 / 1024 / 1024 + 'GB');

// Informações de rede
console.log('Interfaces de rede:', os.networkInterfaces());

// Informações do usuário
console.log('Usuário atual:', os.userInfo());
console.log('Diretório home:', os.homedir());
console.log('Diretório temporário:', os.tmpdir());

// Uptime do sistema
console.log('Uptime do sistema:', os.uptime() + ' segundos');
console.log('Uptime legível:', Math.floor(os.uptime() / 3600) + ' horas');

// Carregamento do sistema
console.log('Load average:', os.loadavg());
```

#### 4. events - Sistema de Eventos
```javascript
const EventEmitter = require('events');

// Criar emitter personalizado
class MeuEmitter extends EventEmitter {}

const meuEmitter = new MeuEmitter();

// Registrar listener
meuEmitter.on('evento', (arg1, arg2) => {
  console.log('Evento disparado!', arg1, arg2);
});

// Registrar listener que executa uma vez
meuEmitter.once('evento-unico', () => {
  console.log('Este evento executa apenas uma vez');
});

// Emitir evento
meuEmitter.emit('evento', 'arg1', 'arg2');
meuEmitter.emit('evento-unico');
meuEmitter.emit('evento-unico'); // Não executa novamente

// Remover listener específico
function ouvintePersonalizado() {
  console.log('Listener personalizado');
}
meuEmitter.on('personalizado', ouvintePersonalizado);
meuEmitter.removeListener('personalizado', ouvintePersonalizado);

// Remover todos os listeners
meuEmitter.removeAllListeners('evento');

// Contar listeners
console.log('Número de listeners:', meuEmitter.listenerCount('evento'));

// Eventos padrão do emitter
meuEmitter.on('newListener', (event, listener) => {
  console.log(`Novo listener adicionado para ${event}`);
});

meuEmitter.on('removeListener', (event, listener) => {
  console.log(`Listener removido de ${event}`);
});

// Evento de erro (importante!)
meuEmitter.on('error', (err) => {
  console.error('Erro no emitter:', err.message);
});

// Exemplo prático - Logger
class Logger extends EventEmitter {
  log(mensagem) {
    console.log(mensagem);
    this.emit('log', { mensagem, data: new Date() });
  }
}

const logger = new Logger();
logger.on('log', (data) => {
  console.log('Log registrado:', data);
});

logger.log('Usuário logado');
```

#### 5. util - Utilidades
```javascript
const util = require('util');

// util.promisify - converter callback para promise
const fs = require('fs');
const readFile = util.promisify(fs.readFile);

async function exemploPromisify() {
  try {
    const data = await readFile('./arquivo.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}

// util.callbackify - converter async function para callback
async function exemploAsync() {
  return 'resultado';
}
const callbackExemplo = util.callbackify(exemploAsync);

callbackExemplo((err, resultado) => {
  if (err) throw err;
  console.log(resultado);
});

// util.inspect - para debug
const obj = {
  nome: 'João',
  idade: 30,
  endereco: {
    rua: 'Rua Exemplo',
    numero: 123
  }
};

console.log(util.inspect(obj, {
  colors: true,
  depth: 2,
  showHidden: false
}));

// util.types - verificar tipos
console.log('É buffer?', util.types.isBuffer(Buffer.from('hello')));
console.log('É date?', util.types.isDate(new Date()));
console.log('É promise?', util.types.isPromise(Promise.resolve()));

// util.format - formatação de strings
console.log(util.format('%s tem %d anos', 'João', 30));
console.log(util.format('Objeto: %j', { nome: 'João' }));

// util.deprecate - marcar função como obsoleta
function funcaoAntiga() {
  console.log('Função antiga');
}
const funcaoDepreciada = util.deprecate(
  funcaoAntiga,
  'funcaoAntiga() está obsoleta. Use novaFuncao()'
);

funcaoDepreciada();
```

#### 6. crypto - Criptografia
```javascript
const crypto = require('crypto');

// Hash MD5
const hashMD5 = crypto.createHash('md5');
hashMD5.update('senha123');
console.log('MD5:', hashMD5.digest('hex'));

// Hash SHA256
const hashSHA256 = crypto.createHash('sha256');
hashSHA256.update('senha123');
console.log('SHA256:', hashSHA256.digest('hex'));

// Hash com salt
function hashComSalt(senha, salt) {
  return crypto
    .createHash('sha256')
    .update(senha + salt)
    .digest('hex');
}
console.log('Hash com salt:', hashComSalt('senha123', 'saltaleatorio'));

// PBKDF2 (recomendado para senhas)
async function hashPBKDF2(senha) {
  const salt = crypto.randomBytes(16).toString('hex');
  const iterations = 100000;
  const keylen = 64;
  const digest = 'sha512';
  
  return new Promise((resolve, reject) => {
    crypto.pbkdf2(senha, salt, iterations, keylen, digest, (err, derivedKey) => {
      if (err) reject(err);
      resolve({
        salt,
        hash: derivedKey.toString('hex')
      });
    });
  });
}

// Verificar senha PBKDF2
async function verificarSenha(senha, salt, hashArmazenado) {
  const iterations = 100000;
  const keylen = 64;
  const digest = 'sha512';
  
  return new Promise((resolve, reject) => {
    crypto.pbkdf2(senha, salt, iterations, keylen, digest, (err, derivedKey) => {
      if (err) reject(err);
      resolve(hashArmazenado === derivedKey.toString('hex'));
    });
  });
}

// Criptografia simétrica (AES)
function criptografarAES(texto, chave) {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv('aes-256-gcm', Buffer.from(chave), iv);
  
  let criptografado = cipher.update(texto, 'utf8', 'hex');
  criptografado += cipher.final('hex');
  
  const authTag = cipher.getAuthTag();
  
  return {
    iv: iv.toString('hex'),
    criptografado,
    authTag: authTag.toString('hex')
  };
}

function descriptografarAES(dados, chave) {
  const decipher = crypto.createDecipheriv(
    'aes-256-gcm',
    Buffer.from(chave),
    Buffer.from(dados.iv, 'hex')
  );
  
  decipher.setAuthTag(Buffer.from(dados.authTag, 'hex'));
  
  let descriptografado = decipher.update(dados.criptografado, 'hex', 'utf8');
  descriptografado += decipher.final('utf8');
  
  return descriptografado;
}

// Tokens aleatórios
const tokenSeguro = crypto.randomBytes(32).toString('hex');
console.log('Token seguro:', tokenSeguro);

// UUID
const { randomUUID } = require('crypto');
console.log('UUID:', randomUUID());
```

#### 7. child_process - Processos Filhos
```javascript
const { spawn, exec, execFile, fork } = require('child_process');
const path = require('path');

// spawn - para streams de dados
const ls = spawn('ls', ['-la', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`Processo finalizado com código ${code}`);
});

// exec - para comandos simples
exec('cat *.js | wc -l', (error, stdout, stderr) => {
  if (error) {
    console.error(`Erro: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});

// exec com promessa
const { promisify } = require('util');
const execAsync = promisify(exec);

async function exemploExec() {
  try {
    const { stdout } = await execAsync('ls -la');
    console.log(stdout);
  } catch (error) {
    console.error(error);
  }
}

// execFile - executar arquivos binários
execFile('/bin/ls', ['-la'], (error, stdout, stderr) => {
  if (error) {
    throw error;
  }
  console.log(stdout);
});

// fork - criar novo processo Node.js
if (process.argv[2] === 'child') {
  console.log('Processo filho executando');
  process.send({ hello: 'world' });
  process.exit(0);
} else {
  const child = fork(__filename, ['child']);
  
  child.on('message', (message) => {
    console.log('Mensagem do filho:', message);
  });
  
  child.on('close', (code) => {
    console.log(`Filho finalizado com código ${code}`);
  });
}
```

#### 8. cluster - Clusterização
```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isPrimary) {
  console.log(`Processo master ${process.pid} está rodando`);
  
  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
  
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} morreu`);
    // Reiniciar worker se necessário
    cluster.fork();
  });
  
  cluster.on('listening', (worker, address) => {
    console.log(
      `Worker ${worker.process.pid} ouvindo em ${address.address}:${address.port}`
    );
  });
  
} else {
  // Workers podem compartilhar conexão TCP
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end(`Resposta do worker ${process.pid}`);
  }).listen(8000);
  
  console.log(`Worker ${process.pid} iniciado`);
}
```

#### 9. worker_threads - Threads Workers
```javascript
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

if (isMainThread) {
  // Thread principal
  module.exports = function calcularComThreads(dados) {
    return new Promise((resolve, reject) => {
      const worker = new Worker(__filename, {
        workerData: dados
      });
      
      worker.on('message', resolve);
      worker.on('error', reject);
      worker.on('exit', (code) => {
        if (code !== 0) {
          reject(new Error(`Worker parou com código ${code}`));
        }
      });
    });
  };
  
  // Exemplo de uso
  async function exemplo() {
    try {
      const resultado = await calcularComThreads([1, 2, 3, 4, 5]);
      console.log('Resultado:', resultado);
    } catch (err) {
      console.error('Erro:', err);
    }
  }
  
  exemplo();
  
} else {
  // Thread worker
  const resultado = workerData.reduce((a, b) => a + b, 0);
  
  // Enviar resultado para thread principal
  parentPort.postMessage(resultado);
}
```

### Módulos Adicionais Úteis

#### 10. http e https
```javascript
const http = require('http');
const https = require('https');

// Criar servidor HTTP
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello World\n');
});

server.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
});

// Cliente HTTP
http.get('http://jsonplaceholder.typicode.com/posts/1', (res) => {
  let data = '';
  
  res.on('data', (chunk) => {
    data += chunk;
  });
  
  res.on('end', () => {
    console.log('Resposta:', JSON.parse(data));
  });
}).on('error', (err) => {
  console.error('Erro:', err);
});

// Servidor HTTPS (necessário certificado)
const fs = require('fs');
const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('Servidor HTTPS seguro\n');
}).listen(443);
```

#### 11. url e querystring
```javascript
const url = require('url');
const querystring = require('querystring');

// Parsing de URL
const urlString = 'https://exemplo.com:8080/path?nome=João&idade=30#section';
const parsedUrl = new URL(urlString);

console.log('Protocolo:', parsedUrl.protocol);
console.log('Host:', parsedUrl.host);
console.log('Path:', parsedUrl.pathname);
console.log('Query string:', parsedUrl.search);
console.log('Hash:', parsedUrl.hash);

// Trabalhando com query strings
const queryString = 'nome=João&idade=30&cidade=São+Paulo';
const parsedQuery = querystring.parse(queryString);
console.log('Query parsed:', parsedQuery);

// Criar query string
const obj = { nome: 'João', idade: 30, cidade: 'São Paulo' };
const stringified = querystring.stringify(obj);
console.log('Query stringified:', stringified);

// Encoding/decoding
const encoded = querystring.escape('nome=João Silva');
console.log('Encoded:', encoded);

const decoded = querystring.unescape('nome%3DJo%C3%A3o%20Silva');
console.log('Decoded:', decoded);
```

#### 12. zlib - Compressão
```javascript
const zlib = require('zlib');
const fs = require('fs');

// Comprimir string
const input = 'Hello World! '.repeat(1000);
zlib.gzip(input, (err, compressed) => {
  if (err) throw err;
  console.log('Original:', input.length, 'bytes');
  console.log('Comprimido:', compressed.length, 'bytes');
  
  // Descomprimir
  zlib.gunzip(compressed, (err, decompressed) => {
    if (err) throw err;
    console.log('Descomprimido igual ao original?', 
      decompressed.toString() === input);
  });
});

// Comprimir arquivo
const gzip = zlib.createGzip();
const inputFile = fs.createReadStream('arquivo-grande.txt');
const outputFile = fs.createWriteStream('arquivo-grande.txt.gz');

inputFile.pipe(gzip).pipe(outputFile);

// Pipeline moderna
const { pipeline } = require('stream');
const { createGzip, createGunzip } = require('zlib');

pipeline(
  fs.createReadStream('arquivo.txt'),
  createGzip(),
  fs.createWriteStream('arquivo.txt.gz'),
  (err) => {
    if (err) {
      console.error('Erro na compressão:', err);
      return;
    }
    console.log('Compressão concluída');
  }
);
```

---

## 📦 NPM - Gerenciador de Pacotes

### Comandos Essenciais do NPM

#### Inicialização e Instalação
```bash
# Iniciar novo projeto
npm init
npm init -y  # Pular perguntas

# Instalar pacote
npm install pacote
npm i pacote  # Forma curta

# Instalar como dependência de desenvolvimento
npm install --save-dev pacote
npm i -D pacote

# Instalar globalmente
npm install -g pacote

# Instalar versão específica
npm install pacote@1.2.3
npm install pacote@latest

# Instalar todas dependências do package.json
npm install
npm ci  # Instalação limpa (para CI/CD)
```

#### Gerenciamento de Dependências
```bash
# Listar pacotes instalados
npm list
npm list --depth=0  # Apenas top level

# Verificar pacotes desatualizados
npm outdated

# Atualizar pacotes
npm update
npm update pacote-especifico

# Remover pacote
npm uninstall pacote
npm rm pacote

# Limpar cache
npm cache clean --force

# Verificar vulnerabilidades
npm audit
npm audit fix  # Corrigir automaticamente
```

#### Scripts do package.json
```json
{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "build": "webpack --mode production",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "prestart": "npm run build",
    "postinstall": "echo 'Instalação completa!'"
  }
}
```

### package.json Completo
```json
{
  "name": "meu-projeto",
  "version": "1.0.0",
  "description": "Descrição do projeto",
  "main": "src/index.js",
  "type": "module",  // ou "commonjs"
  "scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js",
    "test": "jest",
    "build": "webpack --mode production"
  },
  "keywords": ["nodejs", "express", "api"],
  "author": "Seu Nome <seu@email.com>",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.0",
    "mongoose": "^6.0.0",
    "dotenv": "^16.0.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.0",
    "jest": "^28.0.0",
    "eslint": "^8.0.0"
  },
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=8.0.0"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/usuario/projeto.git"
  },
  "bugs": {
    "url": "https://github.com/usuario/projeto/issues"
  },
  "homepage": "https://github.com/usuario/projeto#readme"
}
```

### NPX - Executor de Pacotes
```bash
# Executar pacote sem instalar
npx create-react-app meu-app
npx eslint .
npx jest

# Executar pacote local
npx webpack

# Executar com node versão específica
npx node@14 app.js
```

### Gerenciadores Alternativos

**Yarn:**
```bash
# Instalar
npm install -g yarn

# Comandos equivalentes
yarn init
yarn add pacote
yarn add --dev pacote
yarn remove pacote
yarn start
yarn test
```

**PNPM:**
```bash
# Instalar
npm install -g pnpm

# Comandos
pnpm init
pnpm add pacote
pnpm add -D pacote
pnpm remove pacote
pnpm start
```

### Publicando Pacotes no NPM
```bash
# Login no npm
npm login

# Verificar usuário logado
npm whoami

# Publicar pacote
npm publish

# Publicar com escopo
npm publish --access public

# Atualizar versão
npm version patch  # 1.0.0 -> 1.0.1
npm version minor  # 1.0.0 -> 1.1.0
npm version major  # 1.0.0 -> 2.0.0

# Deprecar versão
npm deprecate meu-pacote@1.0.0 "Versão descontinuada"

# Despublicar (não recomendado)
npm unpublish meu-pacote@1.0.0
```

---

## 🌐 HTTP e Servidores Web

### 1. Servidor HTTP Nativo
```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const path = parsedUrl.pathname;
  const query = parsedUrl.query;
  
  // Log da requisição
  console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
  
  // Rotas
  if (path === '/' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(`
      <html>
        <head><title>Home</title></head>
        <body>
          <h1>Bem-vindo ao Servidor Node.js</h1>
          <p>Path: ${path}</p>
          <p>Query: ${JSON.stringify(query)}</p>
        </body>
      </html>
    `);
    
  } else if (path === '/api/users' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify([
      { id: 1, nome: 'João' },
      { id: 2, nome: 'Maria' }
    ]));
    
  } else if (path === '/api/users' && req.method === 'POST') {
    let body = '';
    
    req.on('data', chunk => {
      body += chunk.toString();
    });
    
    req.on('end', () => {
      try {
        const user = JSON.parse(body);
        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ 
          message: 'Usuário criado', 
          user 
        }));
      } catch (error) {
        res.writeHead(400, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ error: 'JSON inválido' }));
      }
    });
    
  } else if (path.startsWith('/api/users/') && req.method === 'GET') {
    const userId = path.split('/')[3];
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ 
      id: userId, 
      nome: 'Usuário ' + userId 
    }));
    
  } else {
    res.writeHead(404, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ error: 'Rota não encontrada' }));
  }
});

// Middleware pattern manual
const middlewares = [];

function use(middleware) {
  middlewares.push(middleware);
}

// Middlewares de exemplo
use((req, res, next) => {
  console.log('Middleware 1 - Log');
  next();
});

use((req, res, next) => {
  console.log('Middleware 2 - Autenticação');
  // Simulando autenticação
  req.user = { id: 1, nome: 'Admin' };
  next();
});

// Configurações do servidor
const PORT = process.env.PORT || 3000;
const HOST = '0.0.0.0';

server.listen(PORT, HOST, () => {
  console.log(`Servidor rodando em http://${HOST}:${PORT}`);
});

// Eventos do servidor
server.on('error', (error) => {
  console.error('Erro no servidor:', error);
});

server.on('clientError', (error, socket) => {
  console.error('Erro no cliente:', error);
  socket.end('HTTP/1.1 400 Bad Request\r\n\r\n');
});
```

### 2. Servidor HTTPS
```javascript
const https = require('https');
const fs = require('fs');

// Gerar certificados auto-assinados para desenvolvimento
// openssl req -nodes -new -x509 -keyout server.key -out server.cert

const options = {
  key: fs.readFileSync('server.key'),
  cert: fs.readFileSync('server.cert')
};

const server = https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('Servidor HTTPS seguro');
});

server.listen(443, () => {
  console.log('Servidor HTTPS rodando na porta 443');
});
```

### 3. Cliente HTTP Avançado
```javascript
const http = require('http');
const https = require('https');

class HttpClient {
  constructor(baseURL) {
    this.baseURL = baseURL;
    this.agent = new http.Agent({
      keepAlive: true,
      maxSockets: 10,
      timeout: 5000
    });
  }
  
  async request(method, path, data = null, headers = {}) {
    return new Promise((resolve, reject) => {
      const url = new URL(path, this.baseURL);
      const isHttps = url.protocol === 'https:';
      const lib = isHttps ? https : http;
      
      const options = {
        hostname: url.hostname,
        port: url.port || (isHttps ? 443 : 80),
        path: url.pathname + url.search,
        method: method,
        headers: {
          'Content-Type': 'application/json',
          ...headers
        },
        agent: this.agent
      };
      
      const req = lib.request(options, (res) => {
        let data = '';
        
        res.on('data', (chunk) => {
          data += chunk;
        });
        
        res.on('end', () => {
          try {
            const parsedData = JSON.parse(data);
            resolve({
              status: res.statusCode,
              headers: res.headers,
              data: parsedData
            });
          } catch (error) {
            resolve({
              status: res.statusCode,
              headers: res.headers,
              data: data
            });
          }
        });
      });
      
      req.on('error', (error) => {
        reject(error);
      });
      
      req.on('timeout', () => {
        req.destroy();
        reject(new Error('Request timeout'));
      });
      
      if (data) {
        req.write(JSON.stringify(data));
      }
      
      req.end();
    });
  }
  
  async get(path, headers = {}) {
    return this.request('GET', path, null, headers);
  }
  
  async post(path, data, headers = {}) {
    return this.request('POST', path, data, headers);
  }
  
  async put(path, data, headers = {}) {
    return this.request('PUT', path, data, headers);
  }
  
  async delete(path, headers = {}) {
    return this.request('DELETE', path, null, headers);
  }
}

// Exemplo de uso
async function exemploCliente() {
  const client = new HttpClient('https://jsonplaceholder.typicode.com');
  
  try {
    // GET request
    const users = await client.get('/users');
    console.log('Usuários:', users.data);
    
    // POST request
    const newPost = await client.post('/posts', {
      title: 'Teste',
      body: 'Conteúdo do post',
      userId: 1
    });
    console.log('Novo post:', newPost.data);
    
  } catch (error) {
    console.error('Erro na requisição:', error);
  }
}
```

### 4. WebSocket com ws
```javascript
// Instalar: npm install ws
const WebSocket = require('ws');
const http = require('http');

// Servidor HTTP
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('WebSocket Server');
});

// Servidor WebSocket
const wss = new WebSocket.Server({ server });

wss.on('connection', (ws, req) => {
  console.log('Novo cliente conectado:', req.socket.remoteAddress);
  
  // Enviar mensagem de boas-vindas
  ws.send(JSON.stringify({
    type: 'welcome',
    message: 'Bem-vindo ao servidor WebSocket',
    timestamp: new Date().toISOString()
  }));
  
  // Receber mensagens do cliente
  ws.on('message', (message) => {
    console.log('Mensagem recebida:', message.toString());
    
    try {
      const data = JSON.parse(message);
      
      // Processar diferentes tipos de mensagem
      switch (data.type) {
        case 'chat':
          // Broadcast para todos os clientes
          wss.clients.forEach((client) => {
            if (client.readyState === WebSocket.OPEN) {
              client.send(JSON.stringify({
                type: 'chat',
                user: data.user,
                message: data.message,
                timestamp: new Date().toISOString()
              }));
            }
          });
          break;
          
        case 'ping':
          ws.send(JSON.stringify({
            type: 'pong',
            timestamp: new Date().toISOString()
          }));
          break;
      }
    } catch (error) {
      console.error('Erro ao processar mensagem:', error);
    }
  });
  
  // Lidar com desconexão
  ws.on('close', () => {
    console.log('Cliente desconectado');
  });
  
  // Lidar com erros
  ws.on('error', (error) => {
    console.error('Erro WebSocket:', error);
  });
});

// Heartbeat para manter conexões ativas
setInterval(() => {
  wss.clients.forEach((ws) => {
    if (ws.readyState === WebSocket.OPEN) {
      ws.ping();
    }
  });
}, 30000);

server.listen(8080, () => {
  console.log('Servidor WebSocket rodando na porta 8080');
});
```

### 5. Server-Sent Events (SSE)
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // Rota para SSE
  if (req.url === '/events') {
    // Configurar headers para SSE
    res.writeHead(200, {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive',
      'Access-Control-Allow-Origin': '*'
    });
    
    console.log('Cliente SSE conectado');
    
    // Enviar evento inicial
    res.write(`data: ${JSON.stringify({
      type: 'connected',
      message: 'Conectado ao servidor de eventos',
      timestamp: new Date().toISOString()
    })}\n\n`);
    
    // Enviar atualizações periódicas
    let counter = 0;
    const interval = setInterval(() => {
      counter++;
      
      res.write(`data: ${JSON.stringify({
        type: 'update',
        counter: counter,
        timestamp: new Date().toISOString()
      })}\n\n`);
      
      // Enviar evento específico a cada 5 contagens
      if (counter % 5 === 0) {
        res.write(`event: special\n`);
        res.write(`data: ${JSON.stringify({
          type: 'special',
          message: 'Evento especial!',
          count: counter,
          timestamp: new Date().toISOString()
        })}\n\n`);
      }
      
      // Limite para exemplo
      if (counter >= 50) {
        res.write(`event: end\n`);
        res.write(`data: ${JSON.stringify({
          type: 'end',
          message: 'Stream finalizado',
          timestamp: new Date().toISOString()
        })}\n\n`);
        clearInterval(interval);
        res.end();
      }
    }, 1000);
    
    // Lidar com desconexão do cliente
    req.on('close', () => {
      console.log('Cliente SSE desconectado');
      clearInterval(interval);
    });
    
  } else {
    // Página HTML de teste
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(`
      <!DOCTYPE html>
      <html>
      <head>
        <title>SSE Example</title>
      </head>
      <body>
        <h1>Server-Sent Events Example</h1>
        <div id="events"></div>
        
        <script>
          const eventSource = new EventSource('/events');
          
          eventSource.onmessage = (event) => {
            const data = JSON.parse(event.data);
            const div = document.getElementById('events');
            div.innerHTML += '<p>' + JSON.stringify(data) + '</p>';
          };
          
          eventSource.addEventListener('special', (event) => {
            const data = JSON.parse(event.data);
            const div = document.getElementById('events');
            div.innerHTML += '<p><strong>ESPECIAL:</strong> ' + 
              JSON.stringify(data) + '</p>';
          });
          
          eventSource.addEventListener('end', () => {
            eventSource.close();
            document.getElementById('events').innerHTML += 
              '<p><strong>Stream finalizado</strong></p>';
          });
          
          eventSource.onerror = (error) => {
            console.error('SSE Error:', error);
          };
        </script>
      </body>
      </html>
    `);
  }
});

server.listen(3000, () => {
  console.log('Servidor SSE rodando em http://localhost:3000');
});
```

---

## 🗃️ Trabalhando com Arquivos

### 1. Leitura e Escrita Avançada
```javascript
const fs = require('fs').promises;
const path = require('path');

class FileManager {
  constructor(basePath = '.') {
    this.basePath = basePath;
  }
  
  // Criar diretório se não existir
  async ensureDir(dirPath) {
    const fullPath = path.join(this.basePath, dirPath);
    
    try {
      await fs.access(fullPath);
    } catch {
      await fs.mkdir(fullPath, { recursive: true });
    }
    
    return fullPath;
  }
  
  // Ler arquivo com opções
  async readFile(filePath, options = {}) {
    const fullPath = path.join(this.basePath, filePath);
    
    const defaultOptions = {
      encoding: 'utf8',
      flag: 'r'
    };
    
    const mergedOptions = { ...defaultOptions, ...options };
    
    try {
      return await fs.readFile(fullPath, mergedOptions);
    } catch (error) {
      if (error.code === 'ENOENT' && options.createIfMissing) {
        return options.defaultValue || '';
      }
      throw error;
    }
  }
  
  // Escrever arquivo com backup
  async writeFile(filePath, data, options = {}) {
    const fullPath = path.join(this.basePath, filePath);
    const dirPath = path.dirname(fullPath);
    
    await this.ensureDir(dirPath);
    
    // Criar backup se o arquivo já existir
    if (options.backup) {
      try {
        const backupPath = `${fullPath}.backup`;
        await fs.copyFile(fullPath, backupPath);
      } catch (error) {
        // Arquivo não existe, não precisa de backup
      }
    }
    
    const writeOptions = {
      encoding: 'utf8',
      mode: 0o666,
      flag: 'w',
      ...options
    };
    
    await fs.writeFile(fullPath, data, writeOptions);
    return fullPath;
  }
  
  // Ler diretório recursivamente
  async readDirRecursive(dirPath = '') {
    const fullPath = path.join(this.basePath, dirPath);
    const items = await fs.readdir(fullPath, { withFileTypes: true });
    
    const result = {
      files: [],
      directories: []
    };
    
    for (const item of items) {
      const itemPath = path.join(dirPath, item.name);
      
      if (item.isDirectory()) {
        result.directories.push(itemPath);
        
        // Recursão
        const subResult = await this.readDirRecursive(itemPath);
        result.files.push(...subResult.files);
        result.directories.push(...subResult.directories);
        
      } else if (item.isFile()) {
        result.files.push(itemPath);
      }
    }
    
    return result;
  }
  
  // Copiar arquivo ou diretório
  async copy(source, destination) {
    const sourcePath = path.join(this.basePath, source);
    const destPath = path.join(this.basePath, destination);
    
    const stats = await fs.stat(sourcePath);
    
    if (stats.isDirectory()) {
      // Criar diretório de destino
      await fs.mkdir(destPath, { recursive: true });
      
      // Ler conteúdo do diretório
      const items = await fs.readdir(sourcePath);
      
      // Copiar cada item recursivamente
      for (const item of items) {
        const itemSource = path.join(source, item);
        const itemDest = path.join(destination, item);
        await this.copy(itemSource, itemDest);
      }
      
    } else {
      // Copiar arquivo
      await fs.copyFile(sourcePath, destPath);
    }
  }
  
  // Mover/renomear
  async move(source, destination) {
    const sourcePath = path.join(this.basePath, source);
    const destPath = path.join(this.basePath, destination);
    
    await fs.rename(sourcePath, destPath);
  }
  
  // Excluir arquivo ou diretório
  async delete(target) {
    const targetPath = path.join(this.basePath, target);
    
    const stats = await fs.stat(targetPath);
    
    if (stats.isDirectory()) {
      // Remover diretório recursivamente
      const items = await fs.readdir(targetPath);
      
      for (const item of items) {
        const itemPath = path.join(target, item);
        await this.delete(itemPath);
      }
      
      await fs.rmdir(targetPath);
      
    } else {
      // Remover arquivo
      await fs.unlink(targetPath);
    }
  }
  
  // Monitorar mudanças em arquivos
  async watch(filePath, callback) {
    const fullPath = path.join(this.basePath, filePath);
    
    // Verificar se arquivo existe
    try {
      await fs.access(fullPath);
    } catch {
      throw new Error(`Arquivo não encontrado: ${filePath}`);
    }
    
    // Usar fs.watch para monitoramento
    const watcher = fs.watch(fullPath, async (eventType, filename) => {
      if (filename && callback) {
        try {
          const stats = await fs.stat(fullPath);
          callback({
            eventType,
            filename,
            path: fullPath,
            stats,
            timestamp: new Date()
          });
        } catch (error) {
          console.error('Erro ao obter stats:', error);
        }
      }
    });
    
    return {
      close: () => watcher.close()
    };
  }
  
  // Buscar arquivos por padrão
  async findFiles(pattern, dirPath = '') {
    const fullDirPath = path.join(this.basePath, dirPath);
    const regex = new RegExp(pattern);
    
    const result = [];
    
    const items = await fs.readdir(fullDirPath, { withFileTypes: true });
    
    for (const item of items) {
      const itemPath = path.join(dirPath, item.name);
      
      if (item.isDirectory()) {
        // Buscar recursivamente em subdiretórios
        const subResults = await this.findFiles(pattern, itemPath);
        result.push(...subResults);
        
      } else if (item.isFile() && regex.test(item.name)) {
        result.push(itemPath);
      }
    }
    
    return result;
  }
}

// Exemplo de uso
async function exemploFileManager() {
  const fm = new FileManager('./data');
  
  try {
    // Garantir que o diretório existe
    await fm.ensureDir('logs');
    
    // Escrever arquivo
    await fm.writeFile('logs/app.log', 'Log de aplicação\n', {
      backup: true,
      flag: 'a' // Append mode
    });
    
    // Ler arquivo
    const content = await fm.readFile('logs/app.log');
    console.log('Conteúdo do log:', content);
    
    // Listar arquivos recursivamente
    const tree = await fm.readDirRecursive();
    console.log('Arquivos:', tree.files);
    console.log('Diretórios:', tree.directories);
    
    // Buscar arquivos por padrão
    const jsFiles = await fm.findFiles('\\.js$');
    console.log('Arquivos JS:', jsFiles);
    
    // Monitorar mudanças
    const watcher = await fm.watch('logs/app.log', (event) => {
      console.log('Arquivo modificado:', event);
    });
    
    // Parar monitoramento após 10 segundos
    setTimeout(() => {
      watcher.close();
      console.log('Monitoramento parado');
    }, 10000);
    
  } catch (error) {
    console.error('Erro:', error);
  }
}
```

### 2. JSON e Configurações
```javascript
const fs = require('fs').promises;
const path = require('path');

class ConfigManager {
  constructor(configPath = './config') {
    this.configPath = configPath;
    this.configs = new Map();
    this.watchers = new Map();
  }
  
  // Carregar configuração de arquivo JSON
  async load(configName, defaultValue = {}) {
    const filePath = path.join(this.configPath, `${configName}.json`);
    
    try {
      const data = await fs.readFile(filePath, 'utf8');
      const config = JSON.parse(data);
      this.configs.set(configName, config);
      return config;
    } catch (error) {
      if (error.code === 'ENOENT') {
        // Arquivo não existe, criar com valor padrão
        await this.save(configName, defaultValue);
        this.configs.set(configName, defaultValue);
        return defaultValue;
      }
      throw error;
    }
  }
  
  // Salvar configuração
  async save(configName, config) {
    const filePath = path.join(this.configPath, `${configName}.json`);
    const dirPath = path.dirname(filePath);
    
    // Garantir que diretório existe
    try {
      await fs.access(dirPath);
    } catch {
      await fs.mkdir(dirPath, { recursive: true });
    }
    
    const data = JSON.stringify(config, null, 2);
    await fs.writeFile(filePath, data, 'utf8');
    this.configs.set(configName, config);
  }
  
  // Obter valor da configuração
  get(configName, key = null, defaultValue = null) {
    const config = this.configs.get(configName);
    
    if (!config) {
      throw new Error(`Configuração ${configName} não carregada`);
    }
    
    if (key === null) {
      return config;
    }
    
    // Suporte para nested keys: 'database.host'
    const keys = key.split('.');
    let value = config;
    
    for (const k of keys) {
      if (value && typeof value === 'object' && k in value) {
        value = value[k];
      } else {
        return defaultValue;
      }
    }
    
    return value;
  }
  
  // Definir valor da configuração
  async set(configName, key, value) {
    const config = this.configs.get(configName) || {};
    
    // Suporte para nested keys
    const keys = key.split('.');
    let current = config;
    
    for (let i = 0; i < keys.length - 1; i++) {
      const k = keys[i];
      if (!current[k] || typeof current[k] !== 'object') {
        current[k] = {};
      }
      current = current[k];
    }
    
    current[keys[keys.length - 1]] = value;
    
    await this.save(configName, config);
    return config;
  }
  
  // Monitorar mudanças na configuração
  watch(configName, callback) {
    const filePath = path.join(this.configPath, `${configName}.json`);
    
    const watcher = fs.watch(filePath, async (eventType) => {
      if (eventType === 'change' && callback) {
        try {
          const config = await this.load(configName);
          callback(config);
        } catch (error) {
          console.error('Erro ao recarregar configuração:', error);
        }
      }
    });
    
    this.watchers.set(configName, watcher);
    
    return {
      unwatch: () => {
        const w = this.watchers.get(configName);
        if (w) {
          w.close();
          this.watchers.delete(configName);
        }
      }
    };
  }
  
  // Validar configuração com schema
  validate(configName, schema) {
    const config = this.configs.get(configName);
    
    if (!config) {
      throw new Error(`Configuração ${configName} não carregada`);
    }
    
    const errors = [];
    
    for (const [key, rules] of Object.entries(schema)) {
      const value = config[key];
      
      // Verificar required
      if (rules.required && (value === undefined || value === null)) {
        errors.push(`${key} é obrigatório`);
        continue;
      }
      
      // Verificar tipo
      if (rules.type && value !== undefined) {
        let isValidType = false;
        
        switch (rules.type) {
          case 'string':
            isValidType = typeof value === 'string';
            break;
          case 'number':
            isValidType = typeof value === 'number';
            break;
          case 'boolean':
            isValidType = typeof value === 'boolean';
            break;
          case 'array':
            isValidType = Array.isArray(value);
            break;
          case 'object':
            isValidType = typeof value === 'object' && !Array.isArray(value) && value !== null;
            break;
        }
        
        if (!isValidType) {
          errors.push(`${key} deve ser do tipo ${rules.type}`);
        }
      }
      
      // Verificar valores permitidos
      if (rules.enum && !rules.enum.includes(value)) {
        errors.push(`${key} deve ser um dos valores: ${rules.enum.join(', ')}`);
      }
      
      // Verificar formato (regex)
      if (rules.pattern && value !== undefined) {
        const regex = new RegExp(rules.pattern);
        if (!regex.test(value)) {
          errors.push(`${key} deve corresponder ao padrão: ${rules.pattern}`);
        }
      }
      
      // Verificar range para números
      if (rules.min !== undefined && value !== undefined && value < rules.min) {
        errors.push(`${key} deve ser maior ou igual a ${rules.min}`);
      }
      
      if (rules.max !== undefined && value !== undefined && value > rules.max) {
        errors.push(`${key} deve ser menor ou igual a ${rules.max}`);
      }
    }
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }
}

// Exemplo de uso
async function exemploConfig() {
  const configManager = new ConfigManager('./config');
  
  // Schema de validação
  const appSchema = {
    port: {
      type: 'number',
      required: true,
      min: 1,
      max: 65535
    },
    host: {
      type: 'string',
      required: true,
      pattern: '^[a-zA-Z0-9.-]+$'
    },
    debug: {
      type: 'boolean',
      required: false,
      default: false
    },
    database: {
      type: 'object',
      required: true
    }
  };
  
  try {
    // Carregar configuração
    const config = await configManager.load('app', {
      port: 3000,
      host: 'localhost',
      debug: true,
      database: {
        host: 'localhost',
        port: 5432,
        name: 'appdb'
      }
    });
    
    console.log('Configuração carregada:', config);
    
    // Obter valores
    const port = configManager.get('app', 'port');
    const dbHost = configManager.get('app', 'database.host');
    console.log('Porta:', port);
    console.log('DB Host:', dbHost);
    
    // Validar configuração
    const validation = configManager.validate('app', appSchema);
    if (validation.isValid) {
      console.log('Configuração válida');
    } else {
      console.log('Erros de validação:', validation.errors);
    }
    
    // Atualizar configuração
    await configManager.set('app', 'port', 8080);
    await configManager.set('app', 'database.port', 3306);
    
    // Monitorar mudanças
    const watcher = configManager.watch('app', (newConfig) => {
      console.log('Configuração atualizada:', newConfig);
    });
    
    // Parar monitoramento após 30 segundos
    setTimeout(() => {
      watcher.unwatch();
      console.log('Monitoramento parado');
    }, 30000);
    
  } catch (error) {
    console.error('Erro no config manager:', error);
  }
}
```

### 3. CSV e Dados Estruturados
```javascript
const fs = require('fs');
const { parse, stringify } = require('csv');
const path = require('path');

class CSVManager {
  constructor() {
    this.delimiter = ',';
    this.encoding = 'utf8';
  }
  
  // Ler CSV
  async readCSV(filePath, options = {}) {
    return new Promise((resolve, reject) => {
      const results = [];
      const parser = parse({
        delimiter: this.delimiter,
        columns: true,
        skip_empty_lines: true,
        ...options
      });
      
      fs.createReadStream(filePath, { encoding: this.encoding })
        .pipe(parser)
        .on('data', (row) => {
          results.push(row);
        })
        .on('end', () => {
          resolve(results);
        })
        .on('error', (error) => {
          reject(error);
        });
    });
  }
  
  // Escrever CSV
  async writeCSV(filePath, data, columns = null) {
    return new Promise((resolve, reject) => {
      const stringifier = stringify({
        header: true,
        columns: columns || Object.keys(data[0] || {}),
        delimiter: this.delimiter
      });
      
      const writeStream = fs.createWriteStream(filePath, {
        encoding: this.encoding
      });
      
      stringifier.pipe(writeStream);
      
      for (const row of data) {
        stringifier.write(row);
      }
      
      stringifier.end();
      
      writeStream.on('finish', () => {
        resolve();
      });
      
      writeStream.on('error', (error) => {
        reject(error);
      });
    });
  }
  
  // Converter JSON para CSV
  async jsonToCSV(filePath, jsonData) {
    return this.writeCSV(filePath, jsonData);
  }
  
  // Converter CSV para JSON
  async csvToJSON(filePath) {
    return this.readCSV(filePath);
  }
  
  // Filtrar CSV
  async filterCSV(inputPath, outputPath, filterFn) {
    const data = await this.readCSV(inputPath);
    const filtered = data.filter(filterFn);
    await this.writeCSV(outputPath, filtered);
    return filtered;
  }
  
  // Agregar dados do CSV
  async aggregateCSV(filePath, groupBy, aggregateFn) {
    const data = await this.readCSV(filePath);
    const groups = {};
    
    for (const row of data) {
      const key = row[groupBy];
      
      if (!groups[key]) {
        groups[key] = [];
      }
      
      groups[key].push(row);
    }
    
    const result = {};
    
    for (const [key, rows] of Object.entries(groups)) {
      result[key] = aggregateFn(rows);
    }
    
    return result;
  }
}

// Exemplo de uso
async function exemploCSV() {
  const csvManager = new CSVManager();
  
  // Dados de exemplo
  const dados = [
    { nome: 'João', idade: '25', cidade: 'São Paulo' },
    { nome: 'Maria', idade: '30', cidade: 'Rio de Janeiro' },
    { nome: 'Carlos', idade: '35', cidade: 'São Paulo' },
    { nome: 'Ana', idade: '28', cidade: 'Belo Horizonte' }
  ];
  
  try {
    // Escrever CSV
    await csvManager.writeCSV('./dados.csv', dados);
    console.log('CSV criado com sucesso');
    
    // Ler CSV
    const dadosLidos = await csvManager.readCSV('./dados.csv');
    console.log('Dados lidos:', dadosLidos);
    
    // Filtrar CSV
    const filtrados = await csvManager.filterCSV(
      './dados.csv',
      './filtrados.csv',
      (row) => row.cidade === 'São Paulo'
    );
    console.log('Dados filtrados:', filtrados);
    
    // Agregar dados
    const agregados = await csvManager.aggregateCSV(
      './dados.csv',
      'cidade',
      (rows) => ({
        count: rows.length,
        avgAge: rows.reduce((sum, row) => sum + parseInt(row.idade), 0) / rows.length
      })
    );
    console.log('Dados agregados:', agregados);
    
    // Converter para JSON
    const jsonData = await csvManager.csvToJSON('./dados.csv');
    console.log('JSON data:', jsonData);
    
  } catch (error) {
    console.error('Erro:', error);
  }
}
```

### 4. Logs e Monitoramento
```javascript
const fs = require('fs');
const path = require('path');
const { createGzip } = require('zlib');
const { pipeline } = require('stream');

class Logger {
  constructor(options = {}) {
    this.options = {
      level: 'info',
      dir: './logs',
      filename: 'app.log',
      maxSize: 10485760, // 10MB
      maxFiles: 5,
      timestampFormat: 'YYYY-MM-DD HH:mm:ss',
      ...options
    };
    
    this.levels = {
      error: 0,
      warn: 1,
      info: 2,
      debug: 3
    };
    
    this.currentLevel = this.levels[this.options.level];
    this.currentFile = null;
    this.init();
  }
  
  init() {
    // Garantir que diretório de logs existe
    if (!fs.existsSync(this.options.dir)) {
      fs.mkdirSync(this.options.dir, { recursive: true });
    }
    
    this.currentFile = this.getCurrentLogFile();
  }
  
  getCurrentLogFile() {
    const date = new Date();
    const dateStr = date.toISOString().split('T')[0]; // YYYY-MM-DD
    
    return path.join(
      this.options.dir,
      `${dateStr}-${this.options.filename}`
    );
  }
  
  shouldRotate() {
    try {
      const stats = fs.statSync(this.currentFile);
      return stats.size > this.options.maxSize;
    } catch {
      return false;
    }
  }
  
  rotateLog() {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const oldFile = this.currentFile;
    const newFile = `${oldFile}.${timestamp}.gz`;
    
    // Comprimir arquivo antigo
    const gzip = createGzip();
    const source = fs.createReadStream(oldFile);
    const destination = fs.createWriteStream(newFile);
    
    pipeline(source, gzip, destination, (err) => {
      if (err) {
        console.error('Erro ao rotacionar log:', err);
      } else {
        // Limpar arquivo original
        fs.truncateSync(oldFile, 0);
        
        // Limpar logs antigos
        this.cleanOldLogs();
      }
    });
  }
  
  cleanOldLogs() {
    try {
      const files = fs.readdirSync(this.options.dir);
      const logFiles = files
        .filter(file => file.includes(this.options.filename))
        .map(file => ({
          name: file,
          path: path.join(this.options.dir, file),
          time: fs.statSync(path.join(this.options.dir, file)).mtime.getTime()
        }))
        .sort((a, b) => b.time - a.time); // Mais recentes primeiro
      
      // Manter apenas maxFiles
      if (logFiles.length > this.options.maxFiles) {
        const toDelete = logFiles.slice(this.options.maxFiles);
        
        for (const file of toDelete) {
          fs.unlinkSync(file.path);
        }
      }
    } catch (error) {
      console.error('Erro ao limpar logs antigos:', error);
    }
  }
  
  formatMessage(level, message, meta = {}) {
    const timestamp = new Date().toLocaleString('pt-BR');
    const metaStr = Object.keys(meta).length > 0 
      ? ` ${JSON.stringify(meta)}` 
      : '';
    
    return `[${timestamp}] [${level.toUpperCase()}] ${message}${metaStr}\n`;
  }
  
  log(level, message, meta = {}) {
    if (this.levels[level] > this.currentLevel) {
      return;
    }
    
    // Verificar se precisa rotacionar
    if (this.shouldRotate()) {
      this.rotateLog();
    }
    
    // Atualizar arquivo atual se necessário (novo dia)
    const newFile = this.getCurrentLogFile();
    if (newFile !== this.currentFile) {
      this.currentFile = newFile;
    }
    
    const formatted = this.formatMessage(level, message, meta);
    
    try {
      fs.appendFileSync(this.currentFile, formatted, 'utf8');
      
      // Também logar no console (opcional)
      if (this.options.console !== false) {
        const colors = {
          error: '\x1b[31m', // Vermelho
          warn: '\x1b[33m',  // Amarelo
          info: '\x1b[36m',  // Ciano
          debug: '\x1b[90m'  // Cinza
        };
        const reset = '\x1b[0m';
        
        console.log(
          `${colors[level] || ''}${formatted.trim()}${reset}`
        );
      }
    } catch (error) {
      console.error('Erro ao escrever log:', error);
    }
  }
  
  error(message, meta = {}) {
    this.log('error', message, meta);
  }
  
  warn(message, meta = {}) {
    this.log('warn', message, meta);
  }
  
  info(message, meta = {}) {
    this.log('info', message, meta);
  }
  
  debug(message, meta = {}) {
    this.log('debug', message, meta);
  }
  
  // Criar middleware para Express
  middleware() {
    return (req, res, next) => {
      const start = Date.now();
      
      // Capturar resposta
      res.on('finish', () => {
        const duration = Date.now() - start;
        
        this.info('HTTP Request', {
          method: req.method,
          url: req.url,
          status: res.statusCode,
          duration: `${duration}ms`,
          ip: req.ip,
          userAgent: req.get('User-Agent')
        });
      });
      
      next();
    };
  }
  
  // Gerar relatório de logs
  async generateReport(startDate, endDate) {
    const report = {
      total: 0,
      byLevel: {},
      errors: [],
      warnings: []
    };
    
    try {
      const files = fs.readdirSync(this.options.dir);
      const logFiles = files.filter(file => file.includes(this.options.filename));
      
      for (const file of logFiles) {
        const filePath = path.join(this.options.dir, file);
        const content = fs.readFileSync(filePath, 'utf8');
        const lines = content.split('\n').filter(line => line.trim());
        
        for (const line of lines) {
          try {
            const match = line.match(/\[(.*?)\] \[(\w+)\] (.*)/);
            
            if (match) {
              const [, timestamp, level, message] = match;
              const logDate = new Date(timestamp);
              
              // Filtrar por data
              if (logDate >= startDate && logDate <= endDate) {
                report.total++;
                
                // Contar por nível
                report.byLevel[level] = (report.byLevel[level] || 0) + 1;
                
                // Coletar erros e warnings
                if (level === 'ERROR') {
                  report.errors.push({ timestamp, message });
                } else if (level === 'WARN') {
                  report.warnings.push({ timestamp, message });
                }
              }
            }
          } catch (parseError) {
            // Ignorar linhas inválidas
          }
        }
      }
      
      return report;
    } catch (error) {
      this.error('Erro ao gerar relatório', { error: error.message });
      throw error;
    }
  }
}

// Exemplo de uso
async function exemploLogger() {
  const logger = new Logger({
    level: 'debug',
    dir: './logs',
    filename: 'app.log',
    maxSize: 1024 * 1024, // 1MB
    maxFiles: 3,
    console: true
  });
  
  // Logs de exemplo
  logger.info('Aplicação iniciada', { pid: process.pid });
  logger.debug('Configuração carregada', { env: process.env.NODE_ENV });
  logger.warn('Memória alta', { memory: process.memoryUsage() });
  logger.error('Erro de conexão', { 
    code: 'ECONNREFUSED', 
    host: 'localhost:5432' 
  });
  
  // Testar rotacionamento
  for (let i = 0; i < 1000; i++) {
    logger.info(`Log de teste ${i}`);
  }
  
  // Gerar relatório
  const startDate = new Date('2024-01-01');
  const endDate = new Date('2024-12-31');
  
  try {
    const report = await logger.generateReport(startDate, endDate);
    console.log('Relatório de logs:', report);
  } catch (error) {
    console.error('Erro ao gerar relatório:', error);
  }
}
```

---

## 🔄 Streams e Buffers

### 1. Entendendo Streams

**Tipos de Streams:**
- **Readable**: Leitura de dados (fs.createReadStream)
- **Writable**: Escrita de dados (fs.createWriteStream)
- **Duplex**: Leitura e escrita (net.Socket)
- **Transform**: Transformação de dados (zlib.createGzip)

### 2. Trabalhando com Readable Streams
```javascript
const fs = require('fs');
const { Readable } = require('stream');

// Stream de leitura de arquivo
const readableStream = fs.createReadStream('./arquivo-grande.txt', {
  encoding: 'utf8',
  highWaterMark: 64 * 1024 // 64KB por chunk
});

let totalBytes = 0;

readableStream.on('data', (chunk) => {
  totalBytes += chunk.length;
  console.log(`Recebido chunk de ${chunk.length} bytes`);
  console.log(`Total: ${totalBytes} bytes`);
});

readableStream.on('end', () => {
  console.log('Leitura concluída');
});

readableStream.on('error', (error) => {
  console.error('Erro na leitura:', error);
});

// Criar readable stream personalizada
class ContadorStream extends Readable {
  constructor(max, options = {}) {
    super(options);
    this.max = max;
    this.contador = 0;
  }
  
  _read(size) {
    this.contador += 1;
    
    if (this.contador > this.max) {
      this.push(null); // Finaliza a stream
    } else {
      const buffer = Buffer.from(`Linha ${this.contador}\n`);
      this.push(buffer);
    }
  }
}

// Usar stream personalizada
const contador = new ContadorStream(100);
contador.pipe(process.stdout);
```

### 3. Trabalhando com Writable Streams
```javascript
const fs = require('fs');
const { Writable } = require('stream');

// Stream de escrita em arquivo
const writableStream = fs.createWriteStream('./saida.txt', {
  encoding: 'utf8',
  flags: 'a' // Append mode
});

// Escrever dados
writableStream.write('Primeira linha\n');
writableStream.write('Segunda linha\n');
writableStream.end('Última linha\n');

// Eventos
writableStream.on('finish', () => {
  console.log('Escrita concluída');
});

writableStream.on('error', (error) => {
  console.error('Erro na escrita:', error);
});

// Criar writable stream personalizada
class LoggerStream extends Writable {
  constructor(options = {}) {
    super(options);
    this.logs = [];
  }
  
  _write(chunk, encoding, callback) {
    const message = chunk.toString().trim();
    console.log(`[LOG] ${message}`);
    this.logs.push({
      timestamp: new Date(),
      message
    });
    callback();
  }
  
  _final(callback) {
    console.log(`Total de logs: ${this.logs.length}`);
    callback();
  }
}

const logger = new LoggerStream();
logger.write('Log de teste 1');
logger.write('Log de teste 2');
logger.end();
```

### 4. Transform Streams
```javascript
const { Transform, pipeline } = require('stream');
const fs = require('fs');

// Transform stream para uppercase
class UppercaseTransform extends Transform {
  _transform(chunk, encoding, callback) {
    const uppercased = chunk.toString().toUpperCase();
    this.push(uppercased);
    callback();
  }
}

// Transform stream para contar palavras
class WordCounterTransform extends Transform {
  constructor(options = {}) {
    super(options);
    this.wordCount = 0;
    this.charCount = 0;
  }
  
  _transform(chunk, encoding, callback) {
    const text = chunk.toString();
    const words = text.split(/\s+/).filter(word => word.length > 0);
    
    this.wordCount += words.length;
    this.charCount += text.length;
    
    // Passa o chunk sem modificação
    this.push(chunk);
    callback();
  }
  
  _flush(callback) {
    this.push(`\n\nEstatísticas:\n`);
    this.push(`Palavras: ${this.wordCount}\n`);
    this.push(`Caracteres: ${this.charCount}\n`);
    callback();
  }
}

// Pipeline completa
pipeline(
  fs.createReadStream('./texto.txt', 'utf8'),
  new UppercaseTransform(),
  new WordCounterTransform(),
  fs.createWriteStream('./processado.txt'),
  (err) => {
    if (err) {
      console.error('Erro no pipeline:', err);
    } else {
      console.log('Processamento concluído');
    }
  }
);
```

### 5. Duplex Streams
```javascript
const { Duplex } = require('stream');

// Echo stream - repete o que recebe
class EchoStream extends Duplex {
  constructor(options = {}) {
    super(options);
    this.buffer = [];
  }
  
  _write(chunk, encoding, callback) {
    console.log('Recebido:', chunk.toString());
    this.buffer.push(chunk);
    callback();
  }
  
  _read(size) {
    if (this.buffer.length === 0) {
      // Aguarda novos dados
      setTimeout(() => this._read(size), 100);
    } else {
      const chunk = this.buffer.shift();
      this.push(chunk);
    }
  }
}

const echo = new EchoStream();

// Testando a duplex stream
echo.write('Olá');
echo.write('Mundo');

echo.on('data', (data) => {
  console.log('Echo retornou:', data.toString());
});
```

### 6. Trabalhando com Buffers
```javascript
// Criando buffers
const buf1 = Buffer.alloc(10); // Buffer de 10 bytes preenchido com zeros
const buf2 = Buffer.allocUnsafe(10); // Buffer não inicializado (mais rápido)
const buf3 = Buffer.from([1, 2, 3, 4, 5]); // Buffer de array
const buf4 = Buffer.from('Hello', 'utf8'); // Buffer de string
const buf5 = Buffer.alloc(5, 'A'); // Buffer preenchido com 'A'

// Operações com buffers
console.log('Tamanho:', buf4.length);
console.log('Byte na posição 0:', buf4[0]); // 72 (H)
console.log('String do buffer:', buf4.toString());
console.log('String em hex:', buf4.toString('hex'));
console.log('String em base64:', buf4.toString('base64'));

// Modificando buffers
buf4[0] = 74; // J
console.log('Buffer modificado:', buf4.toString()); // Jello

// Copiando buffers
const bufCopy = Buffer.alloc(buf4.length);
buf4.copy(bufCopy);
console.log('Buffer copiado:', bufCopy.toString());

// Concatenando buffers
const buf6 = Buffer.from(' World');
const concatenado = Buffer.concat([buf4, buf6]);
console.log('Concatenado:', concatenado.toString());

// Comparando buffers
const buf7 = Buffer.from('Hello');
const buf8 = Buffer.from('Hello');
const buf9 = Buffer.from('World');

console.log('buf7 equals buf8?', buf7.equals(buf8)); // true
console.log('buf7 equals buf9?', buf7.equals(buf9)); // false

// Buscando em buffers
const data = Buffer.from('Hello World Hello Node.js');
const search = Buffer.from('Hello');

console.log('Primeira ocorrência:', data.indexOf(search)); // 0
console.log('Última ocorrência:', data.lastIndexOf(search)); // 12

// Fatiando buffers
const slice = data.slice(0, 5);
console.log('Slice:', slice.toString()); // Hello

// Iterando buffers
for (const byte of data) {
  process.stdout.write(byte.toString(16).padStart(2, '0') + ' ');
}
console.log();

// Convertendo entre tipos
const json = { nome: 'João', idade: 30 };
const jsonBuffer = Buffer.from(JSON.stringify(json));
console.log('JSON buffer:', jsonBuffer.toString());

const parsedJson = JSON.parse(jsonBuffer.toString());
console.log('JSON parsed:', parsedJson);
```

### 7. Streams de Objetos
```javascript
const { Readable, Writable, Transform } = require('stream');

// Readable stream de objetos
class ObjectReadableStream extends Readable {
  constructor(data, options = {}) {
    super({ ...options, objectMode: true });
    this.data = data;
    this.index = 0;
  }
  
  _read(size) {
    if (this.index >= this.data.length) {
      this.push(null);
    } else {
      this.push(this.data[this.index]);
      this.index++;
    }
  }
}

// Writable stream de objetos
class ObjectWritableStream extends Writable {
  constructor(options = {}) {
    super({ ...options, objectMode: true });
    this.objects = [];
  }
  
  _write(obj, encoding, callback) {
    console.log('Objeto recebido:', obj);
    this.objects.push(obj);
    callback();
  }
  
  _final(callback) {
    console.log('Total de objetos:', this.objects.length);
    callback();
  }
}

// Transform stream de objetos
class ObjectTransformStream extends Transform {
  constructor(options = {}) {
    super({ ...options, objectMode: true });
  }
  
  _transform(obj, encoding, callback) {
    // Transforma o objeto
    const transformed = {
      ...obj,
      processed: true,
      timestamp: new Date()
    };
    
    this.push(transformed);
    callback();
  }
}

// Exemplo de uso
const data = [
  { id: 1, nome: 'João' },
  { id: 2, nome: 'Maria' },
  { id: 3, nome: 'Carlos' }
];

const readable = new ObjectReadableStream(data);
const transform = new ObjectTransformStream();
const writable = new ObjectWritableStream();

readable.pipe(transform).pipe(writable);
```

---

## ⚡ Event Loop e Assincronia

### 1. Entendendo o Event Loop
```javascript
console.log('1. Início');

setTimeout(() => {
  console.log('2. Timeout');
}, 0);

setImmediate(() => {
  console.log('3. Immediate');
});

process.nextTick(() => {
  console.log('4. Next Tick');
});

Promise.resolve().then(() => {
  console.log('5. Promise');
});

console.log('6. Fim');

// Saída:
// 1. Início
// 6. Fim
// 4. Next Tick
// 5. Promise
// 2. Timeout
// 3. Immediate
```

### 2. Callbacks, Promises e Async/Await

**Callbacks (Callback Hell):**
```javascript
const fs = require('fs');

fs.readFile('arquivo1.txt', 'utf8', (err, data1) => {
  if (err) {
    console.error('Erro no arquivo 1:', err);
    return;
  }
  
  fs.readFile('arquivo2.txt', 'utf8', (err, data2) => {
    if (err) {
      console.error('Erro no arquivo 2:', err);
      return;
    }
    
    fs.writeFile('resultado.txt', data1 + data2, (err) => {
      if (err) {
        console.error('Erro ao escrever:', err);
        return;
      }
      
      console.log('Sucesso!');
    });
  });
});
```

**Promises:**
```javascript
const fs = require('fs').promises;

fs.readFile('arquivo1.txt', 'utf8')
  .then(data1 => {
    return fs.readFile('arquivo2.txt', 'utf8')
      .then(data2 => data1 + data2);
  })
  .then(combined => {
    return fs.writeFile('resultado.txt', combined);
  })
  .then(() => {
    console.log('Sucesso!');
  })
  .catch(err => {
    console.error('Erro:', err);
  });
```

**Async/Await:**
```javascript
const fs = require('fs').promises;

async function processarArquivos() {
  try {
    const data1 = await fs.readFile('arquivo1.txt', 'utf8');
    const data2 = await fs.readFile('arquivo2.txt', 'utf8');
    const combined = data1 + data2;
    await fs.writeFile('resultado.txt', combined);
    console.log('Sucesso!');
  } catch (err) {
    console.error('Erro:', err);
  }
}

processarArquivos();
```

### 3. Promise Utilities
```javascript
class PromiseUtils {
  // Delay/Timeout
  static delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
  
  static timeout(promise, ms, errorMessage = 'Timeout') {
    return Promise.race([
      promise,
      new Promise((_, reject) => 
        setTimeout(() => reject(new Error(errorMessage)), ms)
      )
    ]);
  }
  
  // Retry com backoff exponencial
  static async retry(fn, maxRetries = 3, delay = 1000) {
    let lastError;
    
    for (let i = 0; i < maxRetries; i++) {
      try {
        return await fn();
      } catch (error) {
        lastError = error;
        
        if (i < maxRetries - 1) {
          await this.delay(delay * Math.pow(2, i)); // Backoff exponencial
        }
      }
    }
    
    throw lastError;
  }
  
  // Bulk operations com limitação de concorrência
  static async mapWithConcurrency(items, fn, concurrency = 5) {
    const results = [];
    const executing = [];
    
    for (const item of items) {
      const promise = fn(item).then(result => {
        results.push(result);
        executing.splice(executing.indexOf(promise), 1);
        return result;
      });
      
      executing.push(promise);
      
      if (executing.length >= concurrency) {
        await Promise.race(executing);
      }
    }
    
    await Promise.all(executing);
    return results;
  }
  
  // Promise.all com resultados individuais
  static async allSettled(promises) {
    const results = await Promise.allSettled(promises);
    
    return {
      fulfilled: results
        .filter(r => r.status === 'fulfilled')
        .map(r => r.value),
      rejected: results
        .filter(r => r.status === 'rejected')
        .map(r => r.reason)
    };
  }
  
  // Debounce
  static debounce(fn, delay) {
    let timeoutId;
    
    return (...args) => {
      clearTimeout(timeoutId);
      
      return new Promise((resolve) => {
        timeoutId = setTimeout(async () => {
          const result = await fn(...args);
          resolve(result);
        }, delay);
      });
    };
  }
  
  // Throttle
  static throttle(fn, limit) {
    let inThrottle;
    
    return (...args) => {
      if (!inThrottle) {
        inThrottle = true;
        
        const result = fn(...args);
        
        setTimeout(() => {
          inThrottle = false;
        }, limit);
        
        return result;
      }
    };
  }
  
  // Cache de promises
  static cached(fn, ttl = 60000) { // 60 segundos
    const cache = new Map();
    
    return async (...args) => {
      const key = JSON.stringify(args);
      const cached = cache.get(key);
      
      if (cached && Date.now() - cached.timestamp < ttl) {
        return cached.value;
      }
      
      const value = await fn(...args);
      cache.set(key, { value, timestamp: Date.now() });
      
      return value;
    };
  }
}

// Exemplos de uso
async function exemplosPromises() {
  try {
    // Timeout
    const result = await PromiseUtils.timeout(
      fetch('https://api.exemplo.com'),
      5000,
      'Request timeout'
    );
    
    // Retry
    const data = await PromiseUtils.retry(
      () => fetch('https://api.instavel.com'),
      3,
      1000
    );
    
    // Concorrência controlada
    const urls = ['url1', 'url2', 'url3', 'url4', 'url5'];
    const responses = await PromiseUtils.mapWithConcurrency(
      urls,
      url => fetch(url),
      2
    );
    
    // Debounce
    const buscaDebounced = PromiseUtils.debounce(
      termo => buscarNoBanco(termo),
      300
    );
    
    // Cache
    const getUserCached = PromiseUtils.cached(
      id => buscarUsuario(id),
      300000 // 5 minutos
    );
    
  } catch (error) {
    console.error('Erro:', error);
  }
}
```

### 4. Worker Pool para CPU Intensive Tasks
```javascript
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');
const os = require('os');

class WorkerPool {
  constructor(workerPath, numWorkers = os.cpus().length) {
    this.workerPath = workerPath;
    this.numWorkers = numWorkers;
    this.workers = [];
    this.tasks = [];
    this.availableWorkers = [];
    
    this.init();
  }
  
  init() {
    for (let i = 0; i < this.numWorkers; i++) {
      const worker = new Worker(this.workerPath);
      
      worker.on('message', (result) => {
        this.handleResult(worker, result);
      });
      
      worker.on('error', (error) => {
        this.handleError(worker, error);
      });
      
      worker.on('exit', (code) => {
        if (code !== 0) {
          console.error(`Worker parou com código ${code}`);
        }
        this.removeWorker(worker);
      });
      
      this.workers.push(worker);
      this.availableWorkers.push(worker);
    }
  }
  
  runTask(task) {
    return new Promise((resolve, reject) => {
      const taskWithCallbacks = {
        ...task,
        resolve,
        reject
      };
      
      this.tasks.push(taskWithCallbacks);
      this.processNextTask();
    });
  }
  
  processNextTask() {
    if (this.tasks.length === 0 || this.availableWorkers.length === 0) {
      return;
    }
    
    const task = this.tasks.shift();
    const worker = this.availableWorkers.shift();
    
    worker.postMessage(task.data);
    
    worker.currentTask = task;
  }
  
  handleResult(worker, result) {
    const task = worker.currentTask;
    
    if (task) {
      task.resolve(result);
      delete worker.currentTask;
      this.availableWorkers.push(worker);
      this.processNextTask();
    }
  }
  
  handleError(worker, error) {
    const task = worker.currentTask;
    
    if (task) {
      task.reject(error);
      delete worker.currentTask;
      this.availableWorkers.push(worker);
      this.processNextTask();
    }
  }
  
  removeWorker(worker) {
    const index = this.workers.indexOf(worker);
    if (index !== -1) {
      this.workers.splice(index, 1);
    }
    
    const availIndex = this.availableWorkers.indexOf(worker);
    if (availIndex !== -1) {
      this.availableWorkers.splice(availIndex, 1);
    }
    
    // Recriar worker se necessário
    if (this.workers.length < this.numWorkers) {
      this.init();
    }
  }
  
  async drain() {
    // Esperar todas as tarefas concluírem
    while (this.tasks.length > 0) {
      await new Promise(resolve => setTimeout(resolve, 100));
    }
    
    // Terminar workers
    for (const worker of this.workers) {
      worker.terminate();
    }
    
    this.workers = [];
    this.availableWorkers = [];
  }
}

// Worker file (worker.js)
if (!isMainThread) {
  parentPort.on('message', (data) => {
    // Processamento pesado
    const result = processamentoIntensivo(data);
    
    // Enviar resultado de volta
    parentPort.postMessage(result);
  });
  
  function processamentoIntensivo(data) {
    // Exemplo: cálculo pesado
    let total = 0;
    for (let i = 0; i < 1000000000; i++) {
      total += Math.sqrt(i);
    }
    return { total, processed: data };
  }
}

// Uso do worker pool
if (isMainThread) {
  const pool = new WorkerPool('./worker.js', 4);
  
  async function processarDados() {
    const tasks = Array.from({ length: 10 }, (_, i) => ({
      id: i,
      data: `Task ${i}`
    }));
    
    try {
      const results = await Promise.all(
        tasks.map(task => pool.runTask({ data: task }))
      );
      
      console.log('Todos os resultados:', results);
      
      await pool.drain();
      
    } catch (error) {
      console.error('Erro:', error);
    }
  }
  
  processarDados();
}
```

---

## 🔐 Segurança em Node.js

### 1. Validação de Entrada
```javascript
const validator = require('validator'); // npm install validator
const Joi = require('joi'); // npm install joi

class InputValidator {
  // Validação com validator
  static validateWithValidator(input) {
    return {
      isEmail: validator.isEmail(input.email || ''),
      isURL: validator.isURL(input.website || ''),
      isAlpha: validator.isAlpha(input.name || '', 'pt-BR'),
      isNumeric: validator.isNumeric(input.age || ''),
      isStrongPassword: validator.isStrongPassword(input.password || '', {
        minLength: 8,
        minLowercase: 1,
        minUppercase: 1,
        minNumbers: 1,
        minSymbols: 1
      }),
      escape: validator.escape(input.unsafe || ''),
      normalizeEmail: validator.normalizeEmail(input.email || '')
    };
  }
  
  // Validação com Joi
  static userSchema = Joi.object({
    nome: Joi.string()
      .min(3)
      .max(50)
      .required()
      .pattern(/^[a-zA-ZÀ-ÿ\s]+$/),
    
    email: Joi.string()
      .email()
      .required()
      .lowercase(),
    
    idade: Joi.number()
      .integer()
      .min(18)
      .max(120)
      .required(),
    
    senha: Joi.string()
      .min(8)
      .pattern(new RegExp('^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#$%^&*])'))
      .required(),
    
    telefone: Joi.string()
      .pattern(/^\(\d{2}\) \d{4,5}-\d{4}$/),
    
    cpf: Joi.string()
      .pattern(/^\d{3}\.\d{3}\.\d{3}-\d{2}$/),
    
    website: Joi.string()
      .uri({
        scheme: ['http', 'https']
      }),
    
    dataNascimento: Joi.date()
      .max('now')
      .iso(),
    
    role: Joi.string()
      .valid('user', 'admin', 'moderator')
      .default('user'),
    
    preferences: Joi.object({
      theme: Joi.string().valid('light', 'dark').default('light'),
      notifications: Joi.boolean().default(true)
    }).default({})
  });
  
  static async validateWithJoi(data) {
    try {
      const value = await this.userSchema.validateAsync(data, {
        abortEarly: false, // Retorna todos os erros
        stripUnknown: true // Remove campos não definidos no schema
      });
      
      return { isValid: true, value, errors: null };
      
    } catch (error) {
      const errors = error.details.map(detail => ({
        field: detail.path.join('.'),
        message: detail.message,
        type: detail.type
      }));
      
      return { isValid: false, value: null, errors };
    }
  }
  
  // Sanitização básica
  static sanitize(input) {
    if (typeof input === 'string') {
      return input
        .replace(/[<>]/g, '') // Remove tags HTML
        .replace(/script/gi, '') // Remove script tags
        .trim();
    }
    
    if (Array.isArray(input)) {
      return input.map(item => this.sanitize(item));
    }
    
    if (typeof input === 'object' && input !== null) {
      const sanitized = {};
      for (const [key, value] of Object.entries(input)) {
        sanitized[key] = this.sanitize(value);
      }
      return sanitized;
    }
    
    return input;
  }
  
  // Prevenção de SQL Injection
  static escapeSQL(input) {
    if (typeof input !== 'string') return input;
    
    return input
      .replace(/'/g, "''")
      .replace(/--/g, '')
      .replace(/;/g, '')
      .replace(/\b(SELECT|INSERT|UPDATE|DELETE|DROP|UNION)\b/gi, '');
  }
  
  // Validação de arquivos
  static validateFile(file, options = {}) {
    const {
      maxSize = 5 * 1024 * 1024, // 5MB
      allowedTypes = ['image/jpeg', 'image/png', 'application/pdf'],
      allowedExtensions = ['.jpg', '.jpeg', '.png', '.pdf']
    } = options;
    
    const errors = [];
    
    // Tamanho
    if (file.size > maxSize) {
      errors.push(`Arquivo muito grande. Máximo: ${maxSize / 1024 / 1024}MB`);
    }
    
    // Tipo MIME
    if (!allowedTypes.includes(file.mimetype)) {
      errors.push(`Tipo de arquivo não permitido: ${file.mimetype}`);
    }
    
    // Extensão
    const extension = file.originalname.toLowerCase().match(/\.[^.]+$/)?.[0];
    if (!allowedExtensions.includes(extension)) {
      errors.push(`Extensão não permitida: ${extension}`);
    }
    
    // Nome do arquivo
    const fileName = file.originalname;
    if (!/^[a-zA-Z0-9\-_.]+$/.test(fileName)) {
      errors.push('Nome do arquivo contém caracteres inválidos');
    }
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }
}

// Exemplo de uso
async function exemploValidacao() {
  const userData = {
    nome: 'João Silva',
    email: 'joao@exemplo.com',
    idade: 25,
    senha: 'Senha123!',
    telefone: '(11) 99999-9999',
    cpf: '123.456.789-00',
    website: 'https://exemplo.com',
    dataNascimento: '1998-05-15',
    extraField: 'Este campo será removido'
  };
  
  // Validação com Joi
  const result = await InputValidator.validateWithJoi(userData);
  
  if (result.isValid) {
    console.log('Dados válidos:', result.value);
  } else {
    console.log('Erros de validação:', result.errors);
  }
  
  // Sanitização
  const unsafeInput = '<script>alert("XSS")</script> João';
  const safeInput = InputValidator.sanitize(unsafeInput);
  console.log('Sanitizado:', safeInput);
  
  // Validação com validator
  const validatorResult = InputValidator.validateWithValidator({
    email: 'joao@exemplo.com',
    password: 'Senha123!'
  });
  console.log('Validator result:', validatorResult);
}
```

### 2. Autenticação e Autorização
```javascript
const crypto = require('crypto');
const jwt = require('jsonwebtoken'); // npm install jsonwebtoken
const bcrypt = require('bcrypt'); // npm install bcrypt

class AuthManager {
  constructor(secret = process.env.JWT_SECRET || 'secret') {
    this.secret = secret;
    this.saltRounds = 10;
  }
  
  // Hash de senha com bcrypt
  async hashPassword(password) {
    try {
      const salt = await bcrypt.genSalt(this.saltRounds);
      const hash = await bcrypt.hash(password, salt);
      return hash;
    } catch (error) {
      throw new Error('Erro ao gerar hash da senha');
    }
  }
  
  // Verificar senha
  async verifyPassword(password, hash) {
    try {
      return await bcrypt.compare(password, hash);
    } catch (error) {
      throw new Error('Erro ao verificar senha');
    }
  }
  
  // Gerar token JWT
  generateToken(payload, options = {}) {
    const defaultOptions = {
      expiresIn: '24h',
      issuer: 'myapp',
      audience: 'users'
    };
    
    const token = jwt.sign(
      payload,
      this.secret,
      { ...defaultOptions, ...options }
    );
    
    return token;
  }
  
  // Verificar token JWT
  verifyToken(token, options = {}) {
    try {
      const decoded = jwt.verify(token, this.secret, options);
      return { valid: true, decoded };
    } catch (error) {
      return { valid: false, error: error.message };
    }
  }
  
  // Refresh token
  async refreshToken(oldToken) {
    const verification = this.verifyToken(oldToken, { ignoreExpiration: true });
    
    if (!verification.valid) {
      throw new Error('Token inválido');
    }
    
    // Remover campos padrão do JWT
    const { iat, exp, aud, iss, ...payload } = verification.decoded;
    
    // Gerar novo token
    return this.generateToken(payload);
  }
  
  // Rate limiting para autenticação
  createRateLimiter(maxAttempts = 5, windowMs = 15 * 60 * 1000) {
    const attempts = new Map();
    
    return {
      check: (identifier) => {
        const now = Date.now();
        const userAttempts = attempts.get(identifier) || [];
        
        // Remover tentativas antigas
        const recentAttempts = userAttempts.filter(
          attempt => now - attempt < windowMs
        );
        
        if (recentAttempts.length >= maxAttempts) {
          return { allowed: false, remaining: 0 };
        }
        
        recentAttempts.push(now);
        attempts.set(identifier, recentAttempts);
        
        return {
          allowed: true,
          remaining: maxAttempts - recentAttempts.length
        };
      },
      
      reset: (identifier) => {
        attempts.delete(identifier);
      }
    };
  }
  
  // 2FA - Gerar código
  generate2FACode(length = 6) {
    const digits = '0123456789';
    let code = '';
    
    for (let i = 0; i < length; i++) {
      const randomIndex = crypto.randomInt(0, digits.length);
      code += digits[randomIndex];
    }
    
    return code;
  }
  
  // 2FA - Verificar código (simulação)
  async verify2FACode(storedCode, providedCode, window = 300000) { // 5 minutos
    if (!storedCode || !providedCode) {
      return false;
    }
    
    // Em produção, use bibliotecas como speakeasy
    return storedCode === providedCode;
  }
  
  // Password policy
  validatePasswordPolicy(password) {
    const errors = [];
    
    if (password.length < 8) {
      errors.push('A senha deve ter pelo menos 8 caracteres');
    }
    
    if (!/[a-z]/.test(password)) {
      errors.push('A senha deve conter pelo menos uma letra minúscula');
    }
    
    if (!/[A-Z]/.test(password)) {
      errors.push('A senha deve conter pelo menos uma letra maiúscula');
    }
    
    if (!/[0-9]/.test(password)) {
      errors.push('A senha deve conter pelo menos um número');
    }
    
    if (!/[!@#$%^&*(),.?":{}|<>]/.test(password)) {
      errors.push('A senha deve conter pelo menos um caractere especial');
    }
    
    // Verificar senhas comuns
    const commonPasswords = [
      'password', '123456', 'qwerty', 'senha', 'admin'
    ];
    
    if (commonPasswords.includes(password.toLowerCase())) {
      errors.push('Esta senha é muito comum');
    }
    
    return {
      valid: errors.length === 0,
      errors
    };
  }
  
  // Session management
  createSessionManager() {
    const sessions = new Map();
    
    return {
      createSession: (userId, userAgent, ip) => {
        const sessionId = crypto.randomBytes(32).toString('hex');
        const session = {
          id: sessionId,
          userId,
          userAgent,
          ip,
          createdAt: new Date(),
          lastActive: new Date(),
          active: true
        };
        
        sessions.set(sessionId, session);
        
        // Limpar sessões expiradas periodicamente
        setTimeout(() => this.cleanExpiredSessions(), 3600000); // A cada hora
        
        return sessionId;
      },
      
      getSession: (sessionId) => {
        const session = sessions.get(sessionId);
        
        if (!session || !session.active) {
          return null;
        }
        
        // Atualizar último acesso
        session.lastActive = new Date();
        sessions.set(sessionId, session);
        
        return session;
      },
      
      invalidateSession: (sessionId) => {
        const session = sessions.get(sessionId);
        if (session) {
          session.active = false;
          sessions.set(sessionId, session);
        }
      },
      
      invalidateAllSessions: (userId) => {
        for (const [sessionId, session] of sessions) {
          if (session.userId === userId) {
            session.active = false;
            sessions.set(sessionId, session);
          }
        }
      },
      
      cleanExpiredSessions: (maxAge = 24 * 60 * 60 * 1000) => { // 24 horas
        const now = new Date();
        
        for (const [sessionId, session] of sessions) {
          if (now - session.lastActive > maxAge) {
            sessions.delete(sessionId);
          }
        }
      }
    };
  }
}

// Middleware de autenticação para Express
function authMiddleware(authManager) {
  return {
    // Middleware para verificar token JWT
    requireAuth: (req, res, next) => {
      const authHeader = req.headers.authorization;
      
      if (!authHeader || !authHeader.startsWith('Bearer ')) {
        return res.status(401).json({ error: 'Token não fornecido' });
      }
      
      const token = authHeader.split(' ')[1];
      const verification = authManager.verifyToken(token);
      
      if (!verification.valid) {
        return res.status(401).json({ error: 'Token inválido' });
      }
      
      req.user = verification.decoded;
      next();
    },
    
    // Middleware para verificar roles
    requireRole: (...roles) => {
      return (req, res, next) => {
        if (!req.user) {
          return res.status(401).json({ error: 'Não autenticado' });
        }
        
        if (!roles.includes(req.user.role)) {
          return res.status(403).json({ error: 'Acesso negado' });
        }
        
        next();
      };
    },
    
    // Middleware para rate limiting
    rateLimit: (maxAttempts = 5, windowMs = 15 * 60 * 1000) => {
      const rateLimiter = authManager.createRateLimiter(maxAttempts, windowMs);
      
      return (req, res, next) => {
        const identifier = req.ip || req.headers['x-forwarded-for'];
        const check = rateLimiter.check(identifier);
        
        if (!check.allowed) {
          return res.status(429).json({
            error: 'Muitas tentativas. Tente novamente mais tarde.'
          });
        }
        
        res.set('X-RateLimit-Remaining', check.remaining.toString());
        next();
      };
    }
  };
}

// Exemplo de uso
async function exemploAuth() {
  const auth = new AuthManager('supersecret');
  
  // Hash de senha
  const password = 'MinhaSenha123!';
  const hash = await auth.hashPassword(password);
  console.log('Hash:', hash);
  
  // Verificar senha
  const isValid = await auth.verifyPassword(password, hash);
  console.log('Senha válida?', isValid);
  
  // Gerar token
  const token = auth.generateToken({
    userId: 1,
    email: 'joao@exemplo.com',
    role: 'admin'
  });
  console.log('Token:', token);
  
  // Verificar token
  const verification = auth.verifyToken(token);
  console.log('Token válido?', verification.valid);
  console.log('Payload:', verification.decoded);
  
  // Validar política de senha
  const passwordCheck = auth.validatePasswordPolicy('senhafraca');
  console.log('Política de senha:', passwordCheck);
  
  // 2FA
  const twoFACode = auth.generate2FACode();
  console.log('Código 2FA:', twoFACode);
}
```

### 3. Headers de Segurança
```javascript
const helmet = require('helmet'); // npm install helmet

class SecurityHeaders {
  static getHelmetConfig() {
    return helmet({
      // Configurações básicas do Helmet
      contentSecurityPolicy: {
        directives: {
          defaultSrc: ["'self'"],
          styleSrc: ["'self'", "'unsafe-inline'"],
          scriptSrc: ["'self'"],
          imgSrc: ["'self'", "data:", "https:"],
          connectSrc: ["'self'"],
          fontSrc: ["'self'"],
          objectSrc: ["'none'"],
          mediaSrc: ["'self'"],
          frameSrc: ["'none'"],
        },
      },
      hsts: {
        maxAge: 31536000, // 1 ano
        includeSubDomains: true,
        preload: true
      },
      referrerPolicy: { policy: 'strict-origin-when-cross-origin' },
      frameguard: { action: 'deny' },
      noSniff: true,
      xssFilter: true,
      hidePoweredBy: true
    });
  }
  
  static customSecurityHeaders(req, res, next) {
    // Headers de segurança customizados
    res.set({
      'X-Content-Type-Options': 'nosniff',
      'X-Frame-Options': 'DENY',
      'X-XSS-Protection': '1; mode=block',
      'Referrer-Policy': 'strict-origin-when-cross-origin',
      'Permissions-Policy': 'camera=(), microphone=(), geolocation=()',
      'Cross-Origin-Opener-Policy': 'same-origin',
      'Cross-Origin-Resource-Policy': 'same-origin',
      'Cross-Origin-Embedder-Policy': 'require-corp',
      'Strict-Transport-Security': 'max-age=31536000; includeSubDomains; preload'
    });
    
    // Remover headers sensíveis
    res.removeHeader('X-Powered-By');
    res.removeHeader('Server');
    
    next();
  }
  
  static corsOptions() {
    return {
      origin: (origin, callback) => {
        const allowedOrigins = [
          'https://meusite.com',
          'https://app.meusite.com',
          'http://localhost:3000'
        ];
        
        // Permitir requests sem origin (como mobile apps ou curl)
        if (!origin || allowedOrigins.includes(origin)) {
          callback(null, true);
        } else {
          callback(new Error('Origem não permitida pelo CORS'));
        }
      },
      methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
      allowedHeaders: [
        'Content-Type',
        'Authorization',
        'X-Requested-With',
        'Accept',
        'Origin'
      ],
      exposedHeaders: ['X-Total-Count', 'X-RateLimit-Remaining'],
      credentials: true,
      maxAge: 86400, // 24 horas
      preflightContinue: false,
      optionsSuccessStatus: 204
    };
  }
  
  static csrfProtection() {
    const tokens = new Map();
    
    return {
      // Gerar token CSRF
      generateToken: (req) => {
        const token = crypto.randomBytes(32).toString('hex');
        tokens.set(token, {
          createdAt: Date.now(),
          ip: req.ip,
          userAgent: req.get('User-Agent')
        });
        
        // Limpar tokens antigos
        this.cleanOldTokens();
        
        return token;
      },
      
      // Verificar token CSRF
      verifyToken: (req, token) => {
        const tokenData = tokens.get(token);
        
        if (!tokenData) {
          return false;
        }
        
        // Verificar expiração (1 hora)
        if (Date.now() - tokenData.createdAt > 3600000) {
          tokens.delete(token);
          return false;
        }
        
        // Verificar se veio do mesmo IP/User-Agent (opcional)
        if (tokenData.ip !== req.ip || 
            tokenData.userAgent !== req.get('User-Agent')) {
          return false;
        }
        
        tokens.delete(token); // Token de uso único
        return true;
      },
      
      // Limpar tokens antigos
      cleanOldTokens: () => {
        const now = Date.now();
        for (const [token, data] of tokens) {
          if (now - data.createdAt > 3600000) { // Mais de 1 hora
            tokens.delete(token);
          }
        }
      },
      
      // Middleware para Express
      middleware: () => {
        return (req, res, next) => {
          // Skip para métodos safe
          if (['GET', 'HEAD', 'OPTIONS'].includes(req.method)) {
            return next();
          }
          
          const token = req.body._csrf || 
                       req.headers['x-csrf-token'] || 
                       req.headers['csrf-token'];
          
          if (!this.verifyToken(req, token)) {
            return res.status(403).json({ error: 'Token CSRF inválido' });
          }
          
          next();
        };
      }
    };
  }
}
```

### 4. Logging e Monitoramento de Segurança
```javascript
const winston = require('winston'); // npm install winston

class SecurityLogger {
  constructor() {
    this.logger = winston.createLogger({
      level: 'info',
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.json()
      ),
      transports: [
        new winston.transports.File({ 
          filename: 'security.log',
          level: 'warn'
        }),
        new winston.transports.Console({
          format: winston.format.combine(
            winston.format.colorize(),
            winston.format.simple()
          )
        })
      ]
    });
    
    this.suspiciousActivities = new Map();
  }
  
  logSecurityEvent(eventType, details) {
    const logEntry = {
      timestamp: new Date().toISOString(),
      eventType,
      ip: details.ip,
      userAgent: details.userAgent,
      userId: details.userId,
      path: details.path,
      method: details.method,
      ...details.metadata
    };
    
    this.logger.warn('Security Event', logEntry);
    
    // Detectar atividades suspeitas
    this.detectSuspiciousActivity(logEntry);
  }
  
  detectSuspiciousActivity(event) {
    const key = `${event.ip}-${event.eventType}`;
    const now = Date.now();
    
    if (!this.suspiciousActivities.has(key)) {
      this.suspiciousActivities.set(key, {
        count: 1,
        firstSeen: now,
        lastSeen: now
      });
    } else {
      const activity = this.suspiciousActivities.get(key);
      activity.count++;
      activity.lastSeen = now;
      
      // Se muitas tentativas em pouco tempo
      if (activity.count > 10 && (now - activity.firstSeen) < 60000) {
        this.logger.error('Possible Attack Detected', {
          ip: event.ip,
          eventType: event.eventType,
          count: activity.count,
          timeframe: now - activity.firstSeen
        });
        
        // Aqui você poderia bloquear o IP temporariamente
        this.blockIP(event.ip);
      }
    }
    
    // Limpar atividades antigas
    this.cleanOldActivities();
  }
  
  blockIP(ip, duration = 15 * 60 * 1000) { // 15 minutos
    this.logger.info(`Blocking IP ${ip} for ${duration}ms`);
    
    // Em produção, você usaria Redis ou similar
    setTimeout(() => {
      this.logger.info(`Unblocking IP ${ip}`);
    }, duration);
  }
  
  cleanOldActivities() {
    const now = Date.now();
    for (const [key, activity] of this.suspiciousActivities) {
      if (now - activity.lastSeen > 300000) { // 5 minutos
        this.suspiciousActivities.delete(key);
      }
    }
  }
  
  // Middleware para Express
  middleware() {
    return (req, res, next) => {
      const startTime = Date.now();
      
      res.on('finish', () => {
        // Logar apenas erros e atividades suspeitas
        if (res.statusCode >= 400) {
          this.logSecurityEvent('HTTP_ERROR', {
            ip: req.ip,
            userAgent: req.get('User-Agent'),
            userId: req.user?.id,
            path: req.path,
            method: req.method,
            statusCode: res.statusCode,
            duration: Date.now() - startTime,
            metadata: {
              body: req.body,
              query: req.query,
              headers: req.headers
            }
          });
        }
      });
      
      next();
    };
  }
}
```

---

## 📡 Express.js Framework

### 1. Estrutura Básica de uma Aplicação Express
```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

// Middleware básico
app.use(express.json()); // Parse JSON bodies
app.use(express.urlencoded({ extended: true })); // Parse URL-encoded bodies
app.use(express.static('public')); // Arquivos estáticos

// Rotas básicas
app.get('/', (req, res) => {
  res.send('Hello Express!');
});

app.get('/api/health', (req, res) => {
  res.json({ 
    status: 'OK', 
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});

// Rota com parâmetros
app.get('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const { include } = req.query;
  
  res.json({ 
    userId: id,
    include,
    message: 'User details'
  });
});

// Rota POST
app.post('/api/users', (req, res) => {
  const user = req.body;
  
  // Validação básica
  if (!user.name || !user.email) {
    return res.status(400).json({ 
      error: 'Name and email are required' 
    });
  }
  
  // Simular criação no banco
  const newUser = {
    id: Date.now(),
    ...user,
    createdAt: new Date()
  };
  
  res.status(201).json(newUser);
});

// Middleware de erro
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ 
    error: 'Something went wrong!',
    message: process.env.NODE_ENV === 'development' ? err.message : undefined
  });
});

// Rota 404
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});

// Iniciar servidor
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
  console.log(`Environment: ${process.env.NODE_ENV || 'development'}`);
});
```

### 2. Estrutura Avançada de Projeto Express
```
meu-projeto/
├── src/
│   ├── index.js              # Ponto de entrada
│   ├── app.js               # Configuração do Express
│   ├── config/              # Configurações
│   │   ├── database.js
│   │   ├── auth.js
│   │   └── env.js
│   ├── routes/              # Rotas
│   │   ├── api/
│   │   │   ├── v1/
│   │   │   │   ├── auth.js
│   │   │   │   ├── users.js
│   │   │   │   └── products.js
│   │   │   └── index.js
│   │   └── web.js
│   ├── controllers/         # Controladores
│   │   ├── authController.js
│   │   ├── userController.js
│   │   └── productController.js
│   ├── models/              # Modelos
│   │   ├── User.js
│   │   ├── Product.js
│   │   └── index.js
│   ├── middleware/          # Middlewares
│   │   ├── auth.js
│   │   ├── validation.js
│   │   ├── errorHandler.js
│   │   └── rateLimiter.js
│   ├── services/            # Lógica de negócio
│   │   ├── authService.js
│   │   ├── userService.js
│   │   └── emailService.js
│   ├── utils/               # Utilitários
│   │   ├── validators.js
│   │   ├── helpers.js
│   │   └── logger.js
│   └── database/           # Configuração do banco
│       ├── connection.js
│       └── migrations/
├── tests/                  # Testes
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── public/                 # Arquivos estáticos
├── views/                  # Templates (se usar)
├── .env                    # Variáveis de ambiente
├── .env.example
├── package.json
└── README.md
```

### 3. Configuração Completa do Express
```javascript
// src/app.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
const compression = require('compression');
const rateLimit = require('express-rate-limit');
const mongoSanitize = require('express-mongo-sanitize');
const xss = require('xss-clean');
const hpp = require('hpp');

const config = require('./config/env');
const logger = require('./utils/logger');
const errorHandler = require('./middleware/errorHandler');

class App {
  constructor() {
    this.app = express();
    this.port = config.PORT || 3000;
    this.env = config.NODE_ENV || 'development';
    
    this.initializeMiddlewares();
    this.initializeRoutes();
    this.initializeErrorHandling();
  }
  
  initializeMiddlewares() {
    // Trust proxy
    this.app.set('trust proxy', 1);
    
    // Security headers
    this.app.use(helmet());
    
    // CORS
    this.app.use(cors(config.CORS_OPTIONS));
    
    // Rate limiting
    const limiter = rateLimit({
      windowMs: 15 * 60 * 1000, // 15 minutos
      max: 100, // Limite por IP
      message: 'Too many requests from this IP',
      standardHeaders: true,
      legacyHeaders: false
    });
    this.app.use('/api', limiter);
    
    // Body parsing
    this.app.use(express.json({ limit: '10kb' }));
    this.app.use(express.urlencoded({ extended: true, limit: '10kb' }));
    
    // Data sanitization
    this.app.use(mongoSanitize()); // Against NoSQL injection
    this.app.use(xss()); // Against XSS
    
    // Parameter pollution
    this.app.use(hpp({
      whitelist: ['price', 'rating'] // Parâmetros que podem ser duplicados
    }));
    
    // Compression
    this.app.use(compression());
    
    // Logging
    if (this.env === 'development') {
      this.app.use(morgan('dev'));
    } else {
      this.app.use(morgan('combined', {
        stream: {
          write: (message) => logger.info(message.trim())
        }
      }));
    }
    
    // Request logging middleware
    this.app.use((req, res, next) => {
      req.requestId = Date.now().toString(36) + Math.random().toString(36).substr(2);
      logger.info('Incoming Request', {
        requestId: req.requestId,
        method: req.method,
        url: req.url,
        ip: req.ip,
        userAgent: req.get('User-Agent')
      });
      next();
    });
  }
  
  initializeRoutes() {
    // Health check
    this.app.get('/health', (req, res) => {
      res.json({
        status: 'OK',
        timestamp: new Date().toISOString(),
        uptime: process.uptime(),
        memory: process.memoryUsage()
      });
    });
    
    // API routes
    this.app.use('/api/v1', require('./routes/api/v1'));
    
    // Static files
    this.app.use(express.static('public'));
    
    // 404 handler
    this.app.use('*', (req, res) => {
      res.status(404).json({
        status: 'error',
        message: `Route ${req.originalUrl} not found`,
        requestId: req.requestId
      });
    });
  }
  
  initializeErrorHandling() {
    this.app.use(errorHandler);
  }
  
  listen() {
    const server = this.app.listen(this.port, () => {
      logger.info(`Server running on port ${this.port}`);
      logger.info(`Environment: ${this.env}`);
    });
    
    // Graceful shutdown
    process.on('SIGTERM', () => {
      logger.info('SIGTERM received. Shutting down gracefully...');
      server.close(() => {
        logger.info('Server closed');
        process.exit(0);
      });
    });
    
    // Unhandled rejections
    process.on('unhandledRejection', (err) => {
      logger.error('UNHANDLED REJECTION! 💥 Shutting down...');
      logger.error(err.name, err.message);
      server.close(() => {
        process.exit(1);
      });
    });
    
    return server;
  }
}

module.exports = App;
```

### 4. Sistema de Rotas Avançado
```javascript
// src/routes/api/v1/index.js
const express = require('express');
const router = express.Router();

// Importar rotas
const authRoutes = require('./auth');
const userRoutes = require('./users');
const productRoutes = require('./products');

// Middleware de versão da API
router.use((req, res, next) => {
  res.set('X-API-Version', '1.0.0');
  next();
});

// Documentação da API
router.get('/', (req, res) => {
  res.json({
    name: 'API v1',
    version: '1.0.0',
    endpoints: {
      auth: '/auth',
      users: '/users',
      products: '/products'
    },
    documentation: 'https://docs.example.com/api/v1'
  });
});

// Rotas
router.use('/auth', authRoutes);
router.use('/users', userRoutes);
router.use('/products', productRoutes);

// Health check específico da API
router.get('/health', (req, res) => {
  res.json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    service: 'api-v1'
  });
});

module.exports = router;

// src/routes/api/v1/users.js
const express = require('express');
const router = express.Router({ mergeParams: true });

// Middlewares
const { authenticate, authorize } = require('../../../middleware/auth');
const validate = require('../../../middleware/validation');
const rateLimiter = require('../../../middleware/rateLimiter');

// Schemas de validação
const userSchemas = require('../../../utils/validators/userSchemas');

// Controllers
const userController = require('../../../controllers/userController');

// Rate limiting específico
router.use(rateLimiter('users', 100, 900)); // 100 requests a cada 15 minutos

// Rotas públicas
router.post(
  '/register',
  validate(userSchemas.register),
  userController.register
);

router.post(
  '/login',
  validate(userSchemas.login),
  userController.login
);

router.post(
  '/forgot-password',
  validate(userSchemas.forgotPassword),
  userController.forgotPassword
);

router.post(
  '/reset-password/:token',
  validate(userSchemas.resetPassword),
  userController.resetPassword
);

// Rotas protegidas
router.use(authenticate);

// Perfil do usuário atual
router.route('/me')
  .get(userController.getMe)
  .put(
    validate(userSchemas.updateProfile),
    userController.updateMe
  )
  .delete(userController.deleteMe);

router.put(
  '/me/password',
  validate(userSchemas.updatePassword),
  userController.updatePassword
);

// Administração (apenas admin)
router.use(authorize('admin'));

router.route('/')
  .get(
    validate(userSchemas.getUsers),
    userController.getAllUsers
  )
  .post(
    validate(userSchemas.createUser),
    userController.createUser
  );

router.route('/:id')
  .get(
    validate(userSchemas.getUser),
    userController.getUser
  )
  .put(
    validate(userSchemas.updateUser),
    userController.updateUser
  )
  .delete(
    validate(userSchemas.deleteUser),
    userController.deleteUser
  );

// Rotas especiais
router.post(
  '/:id/block',
  authorize('admin'),
  userController.blockUser
);

router.post(
  '/:id/unblock',
  authorize('admin'),
  userController.unblockUser
);

router.get(
  '/:id/activity',
  authorize('admin'),
  userController.getUserActivity
);

module.exports = router;
```

### 5. Sistema de Middlewares Avançado
```javascript
// src/middleware/auth.js
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const AppError = require('../utils/AppError');

class AuthMiddleware {
  // Autenticação via JWT
  static authenticate = async (req, res, next) => {
    try {
      // 1. Obter token
      let token;
      if (
        req.headers.authorization &&
        req.headers.authorization.startsWith('Bearer')
      ) {
        token = req.headers.authorization.split(' ')[1];
      } else if (req.cookies?.token) {
        token = req.cookies.token;
      }
      
      if (!token) {
        throw new AppError('Please log in to access this resource', 401);
      }
      
      // 2. Verificar token
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      
      // 3. Verificar se usuário ainda existe
      const currentUser = await User.findById(decoded.id);
      if (!currentUser) {
        throw new AppError('User no longer exists', 401);
      }
      
      // 4. Verificar se usuário mudou a senha após o token ser emitido
      if (currentUser.changedPasswordAfter(decoded.iat)) {
        throw new AppError('User recently changed password. Please log in again', 401);
      }
      
      // 5. Anexar usuário à requisição
      req.user = currentUser;
      res.locals.user = currentUser;
      
      next();
    } catch (error) {
      next(error);
    }
  };
  
  // Autorização baseada em roles
  static authorize = (...roles) => {
    return (req, res, next) => {
      if (!roles.includes(req.user.role)) {
        return next(
          new AppError('You do not have permission to perform this action', 403)
        );
      }
      next();
    };
  };
  
  // Verificar propriedade
  static isOwner = (modelName, paramName = 'id') => {
    return async (req, res, next) => {
      try {
        const Model = require(`../models/${modelName}`);
        const document = await Model.findById(req.params[paramName]);
        
        if (!document) {
          return next(new AppError('Document not found', 404));
        }
        
        // Verificar se o usuário é dono do recurso
        if (document.user.toString() !== req.user.id && req.user.role !== 'admin') {
          return next(
            new AppError('You are not the owner of this resource', 403)
          );
        }
        
        req[modelName.toLowerCase()] = document;
        next();
      } catch (error) {
        next(error);
      }
    };
  };
  
  // Autenticação opcional (para rotas públicas com dados extras se autenticado)
  static optionalAuth = async (req, res, next) => {
    try {
      const token = req.headers.authorization?.split(' ')[1] || req.cookies?.token;
      
      if (token) {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        const user = await User.findById(decoded.id);
        
        if (user && !user.changedPasswordAfter(decoded.iat)) {
          req.user = user;
        }
      }
      
      next();
    } catch (error) {
      // Ignorar erros de autenticação para rotas opcionais
      next();
    }
  };
}

module.exports = AuthMiddleware;

// src/middleware/validation.js
const Joi = require('joi');
const AppError = require('../utils/AppError');

const validate = (schema, property = 'body') => {
  return (req, res, next) => {
    const { error, value } = schema.validate(req[property], {
      abortEarly: false,
      stripUnknown: true,
      errors: {
        wrap: {
          label: ''
        }
      }
    });
    
    if (error) {
      const errors = error.details.map(detail => ({
        field: detail.path.join('.'),
        message: detail.message.replace(/['"]/g, ''),
        type: detail.type
      }));
      
      return next(new AppError('Validation failed', 400, errors));
    }
    
    // Substituir dados validados
    req[property] = value;
    next();
  };
};

// Validação de parâmetros de query
const validateQuery = (schema) => validate(schema, 'query');

// Validação de parâmetros de URL
const validateParams = (schema) => validate(schema, 'params');

// Validação de headers
const validateHeaders = (schema) => validate(schema, 'headers');

// Validação de arquivos
const validateFile = (options = {}) => {
  return (req, res, next) => {
    if (!req.file && !options.optional) {
      return next(new AppError('No file uploaded', 400));
    }
    
    if (req.file) {
      const { maxSize = 5 * 1024 * 1024, allowedMimeTypes = [] } = options;
      
      if (req.file.size > maxSize) {
        return next(new AppError(`File size exceeds ${maxSize / 1024 / 1024}MB`, 400));
      }
      
      if (allowedMimeTypes.length > 0 && !allowedMimeTypes.includes(req.file.mimetype)) {
        return next(new AppError('File type not allowed', 400));
      }
    }
    
    next();
  };
};

module.exports = {
  validate,
  validateQuery,
  validateParams,
  validateHeaders,
  validateFile
};

// src/middleware/errorHandler.js
const AppError = require('../utils/AppError');
const logger = require('../utils/logger');

const errorHandler = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  
  // Log do erro
  logger.error('Error occurred', {
    requestId: req.requestId,
    error: {
      name: err.name,
      message: err.message,
      stack: err.stack,
      statusCode: err.statusCode
    },
    request: {
      method: req.method,
      url: req.url,
      ip: req.ip,
      userAgent: req.get('User-Agent'),
      user: req.user?.id
    }
  });
  
  // Em desenvolvimento, enviar stack trace
  if (process.env.NODE_ENV === 'development') {
    return res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack,
      requestId: req.requestId
    });
  }
  
  // Em produção, enviar mensagem genérica
  if (err.isOperational) {
    // Erros operacionais conhecidos
    return res.status(err.statusCode).json({
      status: err.status,
      message: err.message,
      errors: err.errors,
      requestId: req.requestId
    });
  }
  
  // Erros de programação ou desconhecidos
  console.error('ERROR 💥', err);
  
  return res.status(500).json({
    status: 'error',
    message: 'Something went wrong!',
    requestId: req.requestId
  });
};

// Handler para rotas não encontradas
const notFoundHandler = (req, res, next) => {
  next(new AppError(`Route ${req.originalUrl} not found`, 404));
};

module.exports = { errorHandler, notFoundHandler };
```

### 6. Sistema de Cache
```javascript
// src/middleware/cache.js
const redis = require('redis');
const { promisify } = require('util');
const logger = require('../utils/logger');

class CacheManager {
  constructor() {
    this.client = redis.createClient({
      url: process.env.REDIS_URL || 'redis://localhost:6379',
      retry_strategy: function(options) {
        if (options.error && options.error.code === 'ECONNREFUSED') {
          return new Error('The server refused the connection');
        }
        if (options.total_retry_time > 1000 * 60 * 60) {
          return new Error('Retry time exhausted');
        }
        if (options.attempt > 10) {
          return undefined;
        }
        return Math.min(options.attempt * 100, 3000);
      }
    });
    
    this.client.on('error', (err) => {
      logger.error('Redis Client Error', err);
    });
    
    this.client.on('connect', () => {
      logger.info('Redis connected successfully');
    });
    
    // Promisify methods
    this.getAsync = promisify(this.client.get).bind(this.client);
    this.setAsync = promisify(this.client.set).bind(this.client);
    this.delAsync = promisify(this.client.del).bind(this.client);
    this.keysAsync = promisify(this.client.keys).bind(this.client);
    this.expireAsync = promisify(this.client.expire).bind(this.client);
  }
  
  async get(key) {
    try {
      const data = await this.getAsync(key);
      return data ? JSON.parse(data) : null;
    } catch (error) {
      logger.error('Cache get error', { key, error });
      return null;
    }
  }
  
  async set(key, value, ttl = 3600) {
    try {
      await this.setAsync(key, JSON.stringify(value));
      await this.expireAsync(key, ttl);
      return true;
    } catch (error) {
      logger.error('Cache set error', { key, error });
      return false;
    }
  }
  
  async delete(key) {
    try {
      await this.delAsync(key);
      return true;
    } catch (error) {
      logger.error('Cache delete error', { key, error });
      return false;
    }
  }
  
  async deletePattern(pattern) {
    try {
      const keys = await this.keysAsync(pattern);
      if (keys.length > 0) {
        await this.delAsync(keys);
      }
      return true;
    } catch (error) {
      logger.error('Cache delete pattern error', { pattern, error });
      return false;
    }
  }
  
  // Middleware para cache de rotas
  cacheMiddleware(ttl = 300) {
    return async (req, res, next) => {
      // Apenas cache GET requests
      if (req.method !== 'GET') {
        return next();
      }
      
      const key = `cache:${req.originalUrl}`;
      
      try {
        const cachedData = await this.get(key);
        
        if (cachedData) {
          logger.debug('Cache hit', { key });
          return res.json(cachedData);
        }
        
        logger.debug('Cache miss', { key });
        
        // Sobrescrever res.json para cachear a resposta
        const originalJson = res.json;
        res.json = function(data) {
          // Cachear apenas respostas bem-sucedidas
          if (res.statusCode >= 200 && res.statusCode < 300) {
            cache.set(key, data, ttl).catch(err => {
              logger.error('Error caching response', err);
            });
          }
          
          originalJson.call(this, data);
        };
        
        next();
      } catch (error) {
        logger.error('Cache middleware error', error);
        next();
      }
    };
  }
  
  // Cache avançado com invalidation
  async cacheWithInvalidation(key, fetchFn, ttl = 3600, tags = []) {
    try {
      // Tentar obter do cache
      const cached = await this.get(key);
      if (cached) {
        return cached;
      }
      
      // Buscar dados
      const data = await fetchFn();
      
      // Armazenar no cache
      await this.set(key, data, ttl);
      
      // Armazenar tags para invalidação
      if (tags.length > 0) {
        for (const tag of tags) {
          const tagKey = `tag:${tag}`;
          const tagData = await this.get(tagKey) || [];
          tagData.push(key);
          await this.set(tagKey, tagData, ttl);
        }
      }
      
      return data;
    } catch (error) {
      logger.error('Cache with invalidation error', { key, error });
      throw error;
    }
  }
  
  // Invalidar por tag
  async invalidateByTag(tag) {
    const tagKey = `tag:${tag}`;
    const keys = await this.get(tagKey) || [];
    
    for (const key of keys) {
      await this.delete(key);
    }
    
    await this.delete(tagKey);
    
    logger.info('Cache invalidated by tag', { tag, keys });
  }
}

// Instância global do cache
const cache = new CacheManager();

// Middleware de limpeza de cache
const clearCache = (pattern) => {
  return async (req, res, next) => {
    // Executar após a resposta
    res.on('finish', async () => {
      if (res.statusCode >= 200 && res.statusCode < 300) {
        try {
          await cache.deletePattern(pattern);
          logger.info('Cache cleared', { pattern });
        } catch (error) {
          logger.error('Error clearing cache', error);
        }
      }
    });
    
    next();
  };
};

module.exports = { cache, clearCache };
```

---

## 🗄️ Bancos de Dados

### 1. MongoDB com Mongoose
```javascript
// src/database/connection.js
const mongoose = require('mongoose');
const logger = require('../utils/logger');

class Database {
  constructor() {
    this.mongoose = mongoose;
  }
  
  async connect() {
    try {
      const mongoURI = process.env.MONGODB_URI || 'mongodb://localhost:27017/myapp';
      
      await this.mongoose.connect(mongoURI, {
        useNewUrlParser: true,
        useUnifiedTopology: true,
        maxPoolSize: 10,
        serverSelectionTimeoutMS: 5000,
        socketTimeoutMS: 45000,
      });
      
      logger.info('MongoDB connected successfully');
      
      // Event listeners
      this.mongoose.connection.on('error', (err) => {
        logger.error('MongoDB connection error:', err);
      });
      
      this.mongoose.connection.on('disconnected', () => {
        logger.warn('MongoDB disconnected');
      });
      
      process.on('SIGINT', async () => {
        await this.mongoose.connection.close();
        logger.info('MongoDB connection closed through app termination');
        process.exit(0);
      });
      
    } catch (error) {
      logger.error('MongoDB connection failed:', error);
      process.exit(1);
    }
  }
  
  async disconnect() {
    try {
      await this.mongoose.connection.close();
      logger.info('MongoDB disconnected');
    } catch (error) {
      logger.error('Error disconnecting MongoDB:', error);
    }
  }
  
  getConnection() {
    return this.mongoose.connection;
  }
}

module.exports = new Database();

// src/models/User.js
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');
const crypto = require('crypto');
const validator = require('validator');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Please tell us your name'],
    trim: true,
    maxlength: [50, 'Name cannot be more than 50 characters']
  },
  
  email: {
    type: String,
    required: [true, 'Please provide your email'],
    unique: true,
    lowercase: true,
    validate: [validator.isEmail, 'Please provide a valid email']
  },
  
  photo: {
    type: String,
    default: 'default.jpg'
  },
  
  role: {
    type: String,
    enum: ['user', 'guide', 'lead-guide', 'admin'],
    default: 'user'
  },
  
  password: {
    type: String,
    required: [true, 'Please provide a password'],
    minlength: 8,
    select: false
  },
  
  passwordConfirm: {
    type: String,
    required: [true, 'Please confirm your password'],
    validate: {
      validator: function(el) {
        return el === this.password;
      },
      message: 'Passwords are not the same'
    }
  },
  
  passwordChangedAt: Date,
  passwordResetToken: String,
  passwordResetExpires: Date,
  
  active: {
    type: Boolean,
    default: true,
    select: false
  },
  
  emailVerified: {
    type: Boolean,
    default: false
  },
  
  verificationToken: String,
  verificationTokenExpires: Date,
  
  loginAttempts: {
    type: Number,
    default: 0
  },
  
  lockUntil: Date,
  
  // Métricas
  lastLogin: Date,
  loginCount: {
    type: Number,
    default: 0
  },
  
  // Timestamps automáticos
  createdAt: {
    type: Date,
    default: Date.now
  },
  
  updatedAt: {
    type: Date,
    default: Date.now
  }
}, {
  toJSON: { virtuals: true },
  toObject: { virtuals: true },
  timestamps: true
});

// Índices
userSchema.index({ email: 1 }, { unique: true });
userSchema.index({ createdAt: -1 });
userSchema.index({ role: 1 });

// Middlewares
userSchema.pre('save', async function(next) {
  // Só executa se a senha foi modificada
  if (!this.isModified('password')) return next();
  
  // Hash da senha
  this.password = await bcrypt.hash(this.password, 12);
  
  // Remove passwordConfirm
  this.passwordConfirm = undefined;
  next();
});

userSchema.pre('save', function(next) {
  if (!this.isModified('password') || this.isNew) return next();
  
  this.passwordChangedAt = Date.now() - 1000;
  next();
});

userSchema.pre(/^find/, function(next) {
  // Não mostrar usuários inativos
  this.find({ active: { $ne: false } });
  next();
});

userSchema.pre('findOneAndUpdate', function(next) {
  this.set({ updatedAt: Date.now() });
  next();
});

// Métodos de instância
userSchema.methods.correctPassword = async function(candidatePassword, userPassword) {
  return await bcrypt.compare(candidatePassword, userPassword);
};

userSchema.methods.changedPasswordAfter = function(JWTTimestamp) {
  if (this.passwordChangedAt) {
    const changedTimestamp = parseInt(this.passwordChangedAt.getTime() / 1000, 10);
    return JWTTimestamp < changedTimestamp;
  }
  
  return false;
};

userSchema.methods.createPasswordResetToken = function() {
  const resetToken = crypto.randomBytes(32).toString('hex');
  
  this.passwordResetToken = crypto
    .createHash('sha256')
    .update(resetToken)
    .digest('hex');
  
  this.passwordResetExpires = Date.now() + 10 * 60 * 1000; // 10 minutos
  
  return resetToken;
};

userSchema.methods.createVerificationToken = function() {
  const verificationToken = crypto.randomBytes(32).toString('hex');
  
  this.verificationToken = crypto
    .createHash('sha256')
    .update(verificationToken)
    .digest('hex');
  
  this.verificationTokenExpires = Date.now() + 24 * 60 * 60 * 1000; // 24 horas
  
  return verificationToken;
};

userSchema.methods.incrementLoginAttempts = function() {
  // Se já está bloqueado e o tempo de bloqueio ainda não expirou
  if (this.lockUntil && this.lockUntil > Date.now()) {
    return false;
  }
  
  // Resetar tentativas se o tempo de bloqueio expirou
  if (this.lockUntil && this.lockUntil < Date.now()) {
    this.loginAttempts = 1;
    this.lockUntil = undefined;
    return true;
  }
  
  // Incrementar tentativas
  this.loginAttempts += 1;
  
  // Bloquear após 5 tentativas falhas
  if (this.loginAttempts >= 5) {
    this.lockUntil = Date.now() + 15 * 60 * 1000; // 15 minutos
  }
  
  return true;
};

userSchema.methods.resetLoginAttempts = function() {
  this.loginAttempts = 0;
  this.lockUntil = undefined;
};

userSchema.methods.isLocked = function() {
  return !!(this.lockUntil && this.lockUntil > Date.now());
};

// Virtuals
userSchema.virtual('fullName').get(function() {
  return this.name;
});

userSchema.virtual('age').get(function() {
  if (!this.birthDate) return null;
  const diff = Date.now() - this.birthDate.getTime();
  return Math.floor(diff / (1000 * 60 * 60 * 24 * 365.25));
});

// Statics
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email: new RegExp(`^${email}$`, 'i') });
};

userSchema.statics.isEmailTaken = async function(email, excludeUserId) {
  const user = await this.findOne({ 
    email: new RegExp(`^${email}$`, 'i'),
    _id: { $ne: excludeUserId }
  });
  return !!user;
};

userSchema.statics.getUsersCountByRole = function() {
  return this.aggregate([
    { $group: { _id: '$role', count: { $sum: 1 } } },
    { $sort: { count: -1 } }
  ]);
};

// Query helpers
userSchema.query.byRole = function(role) {
  return this.where({ role });
};

userSchema.query.activeUsers = function() {
  return this.where({ active: true });
};

userSchema.query.recent = function(days = 7) {
  const date = new Date();
  date.setDate(date.getDate() - days);
  
  return this.where('createdAt').gte(date);
};

const User = mongoose.model('User', userSchema);

module.exports = User;
```

### 2. PostgreSQL com Sequelize
```javascript
// src/database/postgres.js
const { Sequelize, DataTypes, Op } = require('sequelize');
const logger = require('../utils/logger');

class PostgresDatabase {
  constructor() {
    this.sequelize = new Sequelize(
      process.env.POSTGRES_DB,
      process.env.POSTGRES_USER,
      process.env.POSTGRES_PASSWORD,
      {
        host: process.env.POSTGRES_HOST,
        port: process.env.POSTGRES_PORT,
        dialect: 'postgres',
        logging: (msg) => logger.debug(msg),
        pool: {
          max: 10,
          min: 0,
          acquire: 30000,
          idle: 10000
        },
        dialectOptions: {
          ssl: process.env.NODE_ENV === 'production' ? {
            require: true,
            rejectUnauthorized: false
          } : false
        }
      }
    );
  }
  
  async connect() {
    try {
      await this.sequelize.authenticate();
      logger.info('PostgreSQL connected successfully');
      
      // Sincronizar modelos (apenas em desenvolvimento)
      if (process.env.NODE_ENV === 'development') {
        await this.sequelize.sync({ alter: true });
        logger.info('Database synchronized');
      }
      
    } catch (error) {
      logger.error('PostgreSQL connection failed:', error);
      process.exit(1);
    }
  }
  
  async disconnect() {
    try {
      await this.sequelize.close();
      logger.info('PostgreSQL disconnected');
    } catch (error) {
      logger.error('Error disconnecting PostgreSQL:', error);
    }
  }
  
  getConnection() {
    return this.sequelize;
  }
}

// src/models/User.js (Sequelize)
const { Model } = require('sequelize');

class User extends Model {
  static init(sequelize) {
    return super.init({
      id: {
        type: DataTypes.UUID,
        defaultValue: DataTypes.UUIDV4,
        primaryKey: true
      },
      
      name: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: {
          notEmpty: true,
          len: [2, 50]
        }
      },
      
      email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true,
        validate: {
          isEmail: true,
          notEmpty: true
        }
      },
      
      password: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: {
          notEmpty: true,
          len: [8, 100]
        }
      },
      
      role: {
        type: DataTypes.ENUM('user', 'admin', 'moderator'),
        defaultValue: 'user'
      },
      
      status: {
        type: DataTypes.ENUM('active', 'inactive', 'suspended'),
        defaultValue: 'active'
      },
      
      emailVerified: {
        type: DataTypes.BOOLEAN,
        defaultValue: false
      },
      
      lastLogin: {
        type: DataTypes.DATE
      },
      
      loginAttempts: {
        type: DataTypes.INTEGER,
        defaultValue: 0
      },
      
      lockUntil: {
        type: DataTypes.DATE
      },
      
      metadata: {
        type: DataTypes.JSONB,
        defaultValue: {}
      }
    }, {
      sequelize,
      modelName: 'User',
      tableName: 'users',
      timestamps: true,
      paranoid: true, // Soft delete
      indexes: [
        { fields: ['email'], unique: true },
        { fields: ['role'] },
        { fields: ['status'] },
        { fields: ['createdAt'] }
      ],
      hooks: {
        beforeCreate: async (user) => {
          if (user.password) {
            const bcrypt = require('bcryptjs');
            const salt = await bcrypt.genSalt(10);
            user.password = await bcrypt.hash(user.password, salt);
          }
        },
        beforeUpdate: async (user) => {
          if (user.changed('password')) {
            const bcrypt = require('bcryptjs');
            const salt = await bcrypt.genSalt(10);
            user.password = await bcrypt.hash(user.password, salt);
          }
        }
      }
    });
  }
  
  static associate(models) {
    // Relacionamentos
    this.hasMany(models.Post, {
      foreignKey: 'userId',
      as: 'posts'
    });
    
    this.hasMany(models.Comment, {
      foreignKey: 'userId',
      as: 'comments'
    });
  }
  
  // Métodos de instância
  async comparePassword(candidatePassword) {
    const bcrypt = require('bcryptjs');
    return await bcrypt.compare(candidatePassword, this.password);
  }
  
  incrementLoginAttempts() {
    if (this.lockUntil && this.lockUntil > new Date()) {
      return false;
    }
    
    if (this.lockUntil && this.lockUntil < new Date()) {
      this.loginAttempts = 1;
      this.lockUntil = null;
      return true;
    }
    
    this.loginAttempts += 1;
    
    if (this.loginAttempts >= 5) {
      this.lockUntil = new Date(Date.now() + 15 * 60 * 1000);
    }
    
    return true;
  }
  
  resetLoginAttempts() {
    this.loginAttempts = 0;
    this.lockUntil = null;
  }
  
  isLocked() {
    return !!(this.lockUntil && this.lockUntil > new Date());
  }
  
  toJSON() {
    const values = Object.assign({}, this.get());
    delete values.password;
    delete values.loginAttempts;
    delete values.lockUntil;
    return values;
  }
}

module.exports = User;

// src/repositories/UserRepository.js
class UserRepository {
  constructor(UserModel) {
    this.User = UserModel;
  }
  
  async create(userData) {
    try {
      const user = await this.User.create(userData);
      return user;
    } catch (error) {
      throw new Error(`Failed to create user: ${error.message}`);
    }
  }
  
  async findById(id, options = {}) {
    const defaultOptions = {
      attributes: { exclude: ['password'] },
      include: options.include || []
    };
    
    const user = await this.User.findByPk(id, { ...defaultOptions, ...options });
    
    if (!user) {
      throw new Error('User not found');
    }
    
    return user;
  }
  
  async findByEmail(email, options = {}) {
    const user = await this.User.findOne({
      where: { email },
      ...options
    });
    
    if (!user) {
      throw new Error('User not found');
    }
    
    return user;
  }
  
  async update(id, updateData, options = {}) {
    const user = await this.findById(id);
    
    await user.update(updateData, options);
    
    return user.reload();
  }
  
  async delete(id, force = false) {
    const user = await this.findById(id);
    
    if (force) {
      await user.destroy({ force: true });
    } else {
      await user.destroy();
    }
    
    return true;
  }
  
  async findAll(options = {}) {
    const defaultOptions = {
      attributes: { exclude: ['password'] },
      where: {},
      order: [['createdAt', 'DESC']],
      limit: 50,
      offset: 0
    };
    
    const mergedOptions = { ...defaultOptions, ...options };
    
    const { count, rows } = await this.User.findAndCountAll(mergedOptions);
    
    return {
      data: rows,
      pagination: {
        total: count,
        page: Math.floor(mergedOptions.offset / mergedOptions.limit) + 1,
        pages: Math.ceil(count / mergedOptions.limit),
        limit: mergedOptions.limit
      }
    };
  }
  
  async search(query, options = {}) {
    const defaultOptions = {
      attributes: { exclude: ['password'] },
      where: {
        [Op.or]: [
          { name: { [Op.iLike]: `%${query}%` } },
          { email: { [Op.iLike]: `%${query}%` } }
        ]
      },
      limit: 20
    };
    
    return await this.User.findAll({ ...defaultOptions, ...options });
  }
  
  async getStatistics() {
    const statistics = await this.User.findAll({
      attributes: [
        [this.User.sequelize.fn('COUNT', this.User.sequelize.col('id')), 'total'],
        [this.User.sequelize.fn('COUNT', this.User.sequelize.fn('DISTINCT', this.User.sequelize.col('role'))), 'roles'],
        [this.User.sequelize.fn('SUM', this.User.sequelize.literal("CASE WHEN status = 'active' THEN 1 ELSE 0 END")), 'active'],
        [this.User.sequelize.fn('SUM', this.User.sequelize.literal("CASE WHEN \"emailVerified\" = true THEN 1 ELSE 0 END")), 'verified']
      ],
      raw: true
    });
    
    return statistics[0];
  }
}
```

### 3. Redis para Cache e Sessões
```javascript
// src/cache/redis.js
const redis = require('redis');
const { promisify } = require('util');
const logger = require('../utils/logger');

class RedisClient {
  constructor() {
    this.client = redis.createClient({
      url: process.env.REDIS_URL || 'redis://localhost:6379',
      socket: {
        reconnectStrategy: (retries) => {
          if (retries > 10) {
            logger.error('Too many retries on Redis. Giving up...');
            return new Error('Too many retries');
          }
          return Math.min(retries * 100, 3000);
        }
      }
    });
    
    this.client.on('error', (err) => {
      logger.error('Redis Client Error:', err);
    });
    
    this.client.on('connect', () => {
      logger.info('Redis connected successfully');
    });
    
    this.client.on('reconnecting', () => {
      logger.info('Redis reconnecting...');
    });
    
    // Promisify methods
    this.get = promisify(this.client.get).bind(this.client);
    this.set = promisify(this.client.set).bind(this.client);
    this.setex = promisify(this.client.setex).bind(this.client);
    this.del = promisify(this.client.del).bind(this.client);
    this.keys = promisify(this.client.keys).bind(this.client);
    this.exists = promisify(this.client.exists).bind(this.client);
    this.incr = promisify(this.client.incr).bind(this.client);
    this.decr = promisify(this.client.decr).bind(this.client);
    this.expire = promisify(this.client.expire).bind(this.client);
    this.ttl = promisify(this.client.ttl).bind(this.client);
    this.hget = promisify(this.client.hget).bind(this.client);
    this.hset = promisify(this.client.hset).bind(this.client);
    this.hgetall = promisify(this.client.hgetall).bind(this.client);
    this.hdel = promisify(this.client.hdel).bind(this.client);
    this.sadd = promisify(this.client.sadd).bind(this.client);
    this.smembers = promisify(this.client.smembers).bind(this.client);
    this.srem = promisify(this.client.srem).bind(this.client);
    this.zadd = promisify(this.client.zadd).bind(this.client);
    this.zrange = promisify(this.client.zrange).bind(this.client);
    this.publish = promisify(this.client.publish).bind(this.client);
    this.subscribe = promisify(this.client.subscribe).bind(this.client);
  }
  
  async connect() {
    await this.client.connect();
  }
  
  async disconnect() {
    await this.client.quit();
  }
  
  // Cache methods
  async cacheGet(key) {
    try {
      const data = await this.get(key);
      return data ? JSON.parse(data) : null;
    } catch (error) {
      logger.error('Cache get error:', error);
      return null;
    }
  }
  
  async cacheSet(key, value, ttl = 3600) {
    try {
      const stringValue = JSON.stringify(value);
      if (ttl) {
        await this.setex(key, ttl, stringValue);
      } else {
        await this.set(key, stringValue);
      }
      return true;
    } catch (error) {
      logger.error('Cache set error:', error);
      return false;
    }
  }
  
  async cacheDelete(key) {
    try {
      await this.del(key);
      return true;
    } catch (error) {
      logger.error('Cache delete error:', error);
      return false;
    }
  }
  
  async cacheDeletePattern(pattern) {
    try {
      const keys = await this.keys(pattern);
      if (keys.length > 0) {
        await this.del(keys);
      }
      return true;
    } catch (error) {
      logger.error('Cache delete pattern error:', error);
      return false;
    }
  }
  
  // Session management
  async sessionSet(sessionId, data, ttl = 86400) {
    const key = `session:${sessionId}`;
    return await this.cacheSet(key, data, ttl);
  }
  
  async sessionGet(sessionId) {
    const key = `session:${sessionId}`;
    return await this.cacheGet(key);
  }
  
  async sessionDelete(sessionId) {
    const key = `session:${sessionId}`;
    return await this.cacheDelete(key);
  }
  
  // Rate limiting
  async rateLimit(key, limit, window) {
    const current = await this.incr(key);
    
    if (current === 1) {
      await this.expire(key, window);
    }
    
    return {
      current,
      limit,
      remaining: Math.max(limit - current, 0),
      reset: await this.ttl(key)
    };
  }
  
  // Pub/Sub
  async publishMessage(channel, message) {
    try {
      await this.publish(channel, JSON.stringify(message));
      return true;
    } catch (error) {
      logger.error('Publish error:', error);
      return false;
    }
  }
  
  // Leaderboard with sorted sets
  async addToLeaderboard(leaderboardKey, userId, score) {
    await this.zadd(leaderboardKey, score, userId);
  }
  
  async getLeaderboard(leaderboardKey, limit = 10) {
    return await this.zrange(leaderboardKey, 0, limit - 1, 'WITHSCORES');
  }
  
  // Job queue simulation
  async enqueue(queueName, job) {
    const key = `queue:${queueName}`;
    await this.sadd(key, JSON.stringify(job));
  }
  
  async dequeue(queueName) {
    const key = `queue:${queueName}`;
    const members = await this.smembers(key);
    
    if (members.length > 0) {
      const job = JSON.parse(members[0]);
      await this.srem(key, members[0]);
      return job;
    }
    
    return null;
  }
}

module.exports = new RedisClient();

// Exemplo de uso do Redis com Express Session
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redisClient = require('./src/cache/redis');

const sessionMiddleware = session({
  store: new RedisStore({ client: redisClient.client }),
  secret: process.env.SESSION_SECRET || 'supersecret',
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: process.env.NODE_ENV === 'production',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000, // 24 horas
    sameSite: 'strict'
  },
  name: 'sessionId'
});
```

---

## 🧪 Testes em Node.js

### 1. Testes com Jest
```javascript
// tests/unit/userService.test.js
const UserService = require('../../src/services/userService');
const UserRepository = require('../../src/repositories/userRepository');
const AppError = require('../../src/utils/AppError');

// Mock do repositório
jest.mock('../../src/repositories/userRepository');

describe('UserService', () => {
  let userService;
  let mockUserRepository;
  
  beforeEach(() => {
    mockUserRepository = new UserRepository();
    userService = new UserService(mockUserRepository);
    jest.clearAllMocks();
  });
  
  describe('createUser', () => {
    it('should create a new user successfully', async () => {
      // Arrange
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'Password123!'
      };
      
      const expectedUser = {
        id: '123',
        ...userData,
        createdAt: new Date()
      };
      
      mockUserRepository.create.mockResolvedValue(expectedUser);
      mockUserRepository.findByEmail.mockResolvedValue(null);
      
      // Act
      const result = await userService.createUser(userData);
      
      // Assert
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(userData.email);
      expect(mockUserRepository.create).toHaveBeenCalledWith({
        ...userData,
        password: expect.any(String) // Password should be hashed
      });
      expect(result).toEqual(expectedUser);
    });
    
    it('should throw error if email already exists', async () => {
      // Arrange
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'Password123!'
      };
      
      mockUserRepository.findByEmail.mockResolvedValue({ id: '456' });
      
      // Act & Assert
      await expect(userService.createUser(userData))
        .rejects
        .toThrow(AppError);
      
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(userData.email);
      expect(mockUserRepository.create).not.toHaveBeenCalled();
    });
    
    it('should hash password before saving', async () => {
      // Arrange
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'Password123!'
      };
      
      mockUserRepository.findByEmail.mockResolvedValue(null);
      mockUserRepository.create.mockImplementation((data) => 
        Promise.resolve({ id: '123', ...data })
      );
      
      // Act
      const result = await userService.createUser(userData);
      
      // Assert
      expect(result.password).not.toBe(userData.password);
      expect(result.password).toMatch(/^\$2[ayb]\$.{56}$/); // Bcrypt hash pattern
    });
  });
  
  describe('authenticateUser', () => {
    it('should authenticate user with valid credentials', async () => {
      // Arrange
      const credentials = {
        email: 'john@example.com',
        password: 'Password123!'
      };
      
      const mockUser = {
        id: '123',
        email: 'john@example.com',
        password: '$2a$10$hashedpassword', // Bcrypt hash
        comparePassword: jest.fn().mockResolvedValue(true)
      };
      
      mockUserRepository.findByEmail.mockResolvedValue(mockUser);
      
      // Act
      const result = await userService.authenticateUser(credentials);
      
      // Assert
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(credentials.email);
      expect(mockUser.comparePassword).toHaveBeenCalledWith(credentials.password);
      expect(result).toEqual(mockUser);
    });
    
    it('should throw error for invalid email', async () => {
      // Arrange
      const credentials = {
        email: 'nonexistent@example.com',
        password: 'Password123!'
      };
      
      mockUserRepository.findByEmail.mockResolvedValue(null);
      
      // Act & Assert
      await expect(userService.authenticateUser(credentials))
        .rejects
        .toThrow('Invalid credentials');
    });
    
    it('should throw error for invalid password', async () => {
      // Arrange
      const credentials = {
        email: 'john@example.com',
        password: 'WrongPassword'
      };
      
      const mockUser = {
        id: '123',
        email: 'john@example.com',
        password: '$2a$10$hashedpassword',
        comparePassword: jest.fn().mockResolvedValue(false)
      };
      
      mockUserRepository.findByEmail.mockResolvedValue(mockUser);
      
      // Act & Assert
      await expect(userService.authenticateUser(credentials))
        .rejects
        .toThrow('Invalid credentials');
    });
    
    it('should handle locked user accounts', async () => {
      // Arrange
      const credentials = {
        email: 'john@example.com',
        password: 'Password123!'
      };
      
      const mockUser = {
        id: '123',
        email: 'john@example.com',
        password: '$2a$10$hashedpassword',
        isLocked: jest.fn().mockReturnValue(true),
        lockUntil: new Date(Date.now() + 15 * 60 * 1000)
      };
      
      mockUserRepository.findByEmail.mockResolvedValue(mockUser);
      
      // Act & Assert
      await expect(userService.authenticateUser(credentials))
        .rejects
        .toThrow('Account is locked');
    });
  });
  
  describe('updateUserProfile', () => {
    it('should update user profile successfully', async () => {
      // Arrange
      const userId = '123';
      const updateData = {
        name: 'John Updated',
        email: 'john.updated@example.com'
      };
      
      const existingUser = {
        id: userId,
        name: 'John Doe',
        email: 'john@example.com'
      };
      
      const updatedUser = {
        ...existingUser,
        ...updateData
      };
      
      mockUserRepository.findById.mockResolvedValue(existingUser);
      mockUserRepository.isEmailTaken.mockResolvedValue(false);
      mockUserRepository.update.mockResolvedValue(updatedUser);
      
      // Act
      const result = await userService.updateUserProfile(userId, updateData);
      
      // Assert
      expect(mockUserRepository.findById).toHaveBeenCalledWith(userId);
      expect(mockUserRepository.isEmailTaken).toHaveBeenCalledWith(
        updateData.email,
        userId
      );
      expect(mockUserRepository.update).toHaveBeenCalledWith(userId, updateData);
      expect(result).toEqual(updatedUser);
    });
    
    it('should not update email if it already exists', async () => {
      // Arrange
      const userId = '123';
      const updateData = {
        name: 'John Updated',
        email: 'existing@example.com'
      };
      
      mockUserRepository.findById.mockResolvedValue({ id: userId });
      mockUserRepository.isEmailTaken.mockResolvedValue(true);
      
      // Act & Assert
      await expect(userService.updateUserProfile(userId, updateData))
        .rejects
        .toThrow('Email already taken');
      
      expect(mockUserRepository.update).not.toHaveBeenCalled();
    });
  });
  
  describe('deleteUser', () => {
    it('should delete user successfully', async () => {
      // Arrange
      const userId = '123';
      
      mockUserRepository.delete.mockResolvedValue(true);
      
      // Act
      const result = await userService.deleteUser(userId);
      
      // Assert
      expect(mockUserRepository.delete).toHaveBeenCalledWith(userId);
      expect(result).toBe(true);
    });
    
    it('should throw error if user not found', async () => {
      // Arrange
      const userId = 'nonexistent';
      
      mockUserRepository.delete.mockRejectedValue(new Error('User not found'));
      
      // Act & Assert
      await expect(userService.deleteUser(userId))
        .rejects
        .toThrow('User not found');
    });
  });
  
  describe('getUserStatistics', () => {
    it('should return user statistics', async () => {
      // Arrange
      const mockStats = {
        total: 100,
        active: 85,
        verified: 75,
        roles: 3
      };
      
      mockUserRepository.getStatistics.mockResolvedValue(mockStats);
      
      // Act
      const result = await userService.getUserStatistics();
      
      // Assert
      expect(mockUserRepository.getStatistics).toHaveBeenCalled();
      expect(result).toEqual(mockStats);
    });
  });
});

// tests/integration/auth.test.js
const request = require('supertest');
const app = require('../../src/app');
const User = require('../../src/models/User');
const db = require('../../src/database/connection');

describe('Authentication API', () => {
  beforeAll(async () => {
    await db.connect();
  });
  
  afterAll(async () => {
    await db.disconnect();
  });
  
  beforeEach(async () => {
    await User.deleteMany({});
  });
  
  describe('POST /api/v1/auth/register', () => {
    it('should register a new user', async () => {
      const userData = {
        name: 'Test User',
        email: 'test@example.com',
        password: 'Password123!',
        passwordConfirm: 'Password123!'
      };
      
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send(userData)
        .expect('Content-Type', /json/)
        .expect(201);
      
      expect(response.body.status).toBe('success');
      expect(response.body.data.user).toHaveProperty('_id');
      expect(response.body.data.user.email).toBe(userData.email);
      expect(response.body.data.user.name).toBe(userData.name);
      expect(response.body.data.user).not.toHaveProperty('password');
      expect(response.body.data).toHaveProperty('token');
      
      // Verify user was saved in database
      const user = await User.findOne({ email: userData.email });
      expect(user).toBeTruthy();
      expect(user.name).toBe(userData.name);
    });
    
    it('should not register user with existing email', async () => {
      const userData = {
        name: 'Test User',
        email: 'test@example.com',
        password: 'Password123!',
        passwordConfirm: 'Password123!'
      };
      
      // Create user first
      await request(app)
        .post('/api/v1/auth/register')
        .send(userData);
      
      // Try to create user with same email
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send(userData)
        .expect(400);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Email already exists');
    });
    
    it('should validate required fields', async () => {
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send({})
        .expect(400);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Validation failed');
    });
    
    it('should validate password strength', async () => {
      const userData = {
        name: 'Test User',
        email: 'test@example.com',
        password: 'weak',
        passwordConfirm: 'weak'
      };
      
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send(userData)
        .expect(400);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Validation failed');
    });
  });
  
  describe('POST /api/v1/auth/login', () => {
    beforeEach(async () => {
      // Create a test user
      await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'Password123!',
          passwordConfirm: 'Password123!'
        });
    });
    
    it('should login with valid credentials', async () => {
      const credentials = {
        email: 'test@example.com',
        password: 'Password123!'
      };
      
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send(credentials)
        .expect(200);
      
      expect(response.body.status).toBe('success');
      expect(response.body.data.user).toHaveProperty('_id');
      expect(response.body.data.user.email).toBe(credentials.email);
      expect(response.body.data).toHaveProperty('token');
    });
    
    it('should not login with invalid email', async () => {
      const credentials = {
        email: 'wrong@example.com',
        password: 'Password123!'
      };
      
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send(credentials)
        .expect(401);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Invalid credentials');
    });
    
    it('should not login with invalid password', async () => {
      const credentials = {
        email: 'test@example.com',
        password: 'WrongPassword'
      };
      
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send(credentials)
        .expect(401);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Invalid credentials');
    });
    
    it('should lock account after multiple failed attempts', async () => {
      const credentials = {
        email: 'test@example.com',
        password: 'WrongPassword'
      };
      
      // Make 5 failed attempts
      for (let i = 0; i < 5; i++) {
        await request(app)
          .post('/api/v1/auth/login')
          .send(credentials)
          .expect(401);
      }
      
      // 6th attempt should be locked
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'Password123!'
        })
        .expect(401);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Account is locked');
    });
  });
  
  describe('GET /api/v1/auth/me', () => {
    let token;
    
    beforeEach(async () => {
      // Register and login to get token
      await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'Password123!',
          passwordConfirm: 'Password123!'
        });
      
      const loginResponse = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'Password123!'
        });
      
      token = loginResponse.body.data.token;
    });
    
    it('should get current user profile with valid token', async () => {
      const response = await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      expect(response.body.status).toBe('success');
      expect(response.body.data.user.email).toBe('test@example.com');
      expect(response.body.data.user.name).toBe('Test User');
      expect(response.body.data.user).not.toHaveProperty('password');
    });
    
    it('should not get profile without token', async () => {
      const response = await request(app)
        .get('/api/v1/auth/me')
        .expect(401);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Please log in');
    });
    
    it('should not get profile with invalid token', async () => {
      const response = await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', 'Bearer invalidtoken')
        .expect(401);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Invalid token');
    });
  });
  
  describe('POST /api/v1/auth/logout', () => {
    it('should logout successfully', async () => {
      const response = await request(app)
        .post('/api/v1/auth/logout')
        .expect(200);
      
      expect(response.body.status).toBe('success');
      expect(response.body.message).toContain('Logged out successfully');
    });
  });
  
  describe('POST /api/v1/auth/refresh-token', () => {
    let token;
    
    beforeEach(async () => {
      // Register and login to get token
      await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'Password123!',
          passwordConfirm: 'Password123!'
        });
      
      const loginResponse = await request(app)
        .post('/api/v1/auth/login')
        .send({
          email: 'test@example.com',
          password: 'Password123!'
        });
      
      token = loginResponse.body.data.token;
    });
    
    it('should refresh token successfully', async () => {
      const response = await request(app)
        .post('/api/v1/auth/refresh-token')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      expect(response.body.status).toBe('success');
      expect(response.body.data).toHaveProperty('token');
      expect(response.body.data.token).not.toBe(token);
    });
    
    it('should not refresh with invalid token', async () => {
      const response = await request(app)
        .post('/api/v1/auth/refresh-token')
        .set('Authorization', 'Bearer invalidtoken')
        .expect(401);
      
      expect(response.body.status).toBe('fail');
    });
  });
});

// tests/e2e/userFlow.test.js
const puppeteer = require('puppeteer');
const app = require('../../src/app');
const User = require('../../src/models/User');

describe('User Flow E2E', () => {
  let browser;
  let page;
  let server;
  
  beforeAll(async () => {
    server = app.listen(4000);
    
    browser = await puppeteer.launch({
      headless: 'new',
      args: ['--no-sandbox', '--disable-setuid-sandbox']
    });
  });
  
  afterAll(async () => {
    await browser.close();
    server.close();
  });
  
  beforeEach(async () => {
    page = await browser.newPage();
    await User.deleteMany({});
  });
  
  afterEach(async () => {
    await page.close();
  });
  
  describe('User Registration Flow', () => {
    it('should complete user registration successfully', async () => {
      // Navigate to registration page
      await page.goto('http://localhost:4000/register');
      
      // Fill registration form
      await page.type('#name', 'Test User');
      await page.type('#email', 'test@example.com');
      await page.type('#password', 'Password123!');
      await page.type('#confirmPassword', 'Password123!');
      
      // Submit form
      await page.click('#submitBtn');
      
      // Wait for navigation and success message
      await page.waitForNavigation();
      
      // Check for success message
      const successMessage = await page.$eval('.alert-success', el => el.textContent);
      expect(successMessage).toContain('Registration successful');
      
      // Check if user is redirected to dashboard
      const currentUrl = page.url();
      expect(currentUrl).toContain('/dashboard');
      
      // Verify user was created in database
      const user = await User.findOne({ email: 'test@example.com' });
      expect(user).toBeTruthy();
      expect(user.name).toBe('Test User');
    });
    
    it('should show validation errors for invalid input', async () => {
      await page.goto('http://localhost:4000/register');
      
      // Submit empty form
      await page.click('#submitBtn');
      
      // Check for validation errors
      await page.waitForSelector('.error-message');
      
      const errors = await page.$$eval('.error-message', elements => 
        elements.map(el => el.textContent)
      );
      
      expect(errors.length).toBeGreaterThan(0);
      expect(errors.some(error => error.includes('required'))).toBeTruthy();
    });
    
    it('should show error for existing email', async () => {
      // Create user first
      await User.create({
        name: 'Existing User',
        email: 'existing@example.com',
        password: 'Password123!'
      });
      
      await page.goto('http://localhost:4000/register');
      
      // Try to register with existing email
      await page.type('#name', 'New User');
      await page.type('#email', 'existing@example.com');
      await page.type('#password', 'Password123!');
      await page.type('#confirmPassword', 'Password123!');
      
      await page.click('#submitBtn');
      
      // Check for error message
      await page.waitForSelector('.alert-danger');
      const errorMessage = await page.$eval('.alert-danger', el => el.textContent);
      expect(errorMessage).toContain('Email already exists');
    });
  });
  
  describe('User Login Flow', () => {
    beforeEach(async () => {
      // Create a test user
      await User.create({
        name: 'Test User',
        email: 'test@example.com',
        password: 'Password123!'
      });
    });
    
    it('should login successfully with valid credentials', async () => {
      await page.goto('http://localhost:4000/login');
      
      // Fill login form
      await page.type('#email', 'test@example.com');
      await page.type('#password', 'Password123!');
      
      await page.click('#submitBtn');
      
      await page.waitForNavigation();
      
      // Check if redirected to dashboard
      const currentUrl = page.url();
      expect(currentUrl).toContain('/dashboard');
      
      // Check for welcome message
      const welcomeMessage = await page.$eval('.welcome-message', el => el.textContent);
      expect(welcomeMessage).toContain('Welcome, Test User');
    });
    
    it('should show error for invalid credentials', async () => {
      await page.goto('http://localhost:4000/login');
      
      await page.type('#email', 'test@example.com');
      await page.type('#password', 'WrongPassword');
      
      await page.click('#submitBtn');
      
      await page.waitForSelector('.alert-danger');
      const errorMessage = await page.$eval('.alert-danger', el => el.textContent);
      expect(errorMessage).toContain('Invalid credentials');
    });
    
    it('should lock account after multiple failed attempts', async () => {
      await page.goto('http://localhost:4000/login');
      
      // Make 5 failed attempts
      for (let i = 0; i < 5; i++) {
        await page.type('#email', 'test@example.com');
        await page.type('#password', 'WrongPassword');
        await page.click('#submitBtn');
        await page.waitForTimeout(100);
        await page.reload();
      }
      
      // 6th attempt
      await page.type('#email', 'test@example.com');
      await page.type('#password', 'Password123!');
      await page.click('#submitBtn');
      
      await page.waitForSelector('.alert-danger');
      const errorMessage = await page.$eval('.alert-danger', el => el.textContent);
      expect(errorMessage).toContain('Account is locked');
    });
  });
  
  describe('User Profile Flow', () => {
    let cookies;
    
    beforeEach(async () => {
      // Create user and login to get session
      const user = await User.create({
        name: 'Test User',
        email: 'test@example.com',
        password: 'Password123!'
      });
      
      // Simulate login to get session cookie
      const loginResponse = await page.goto('http://localhost:4000/api/v1/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        postData: JSON.stringify({
          email: 'test@example.com',
          password: 'Password123!'
        })
      });
      
      cookies = await page.cookies();
    });
    
    it('should update user profile', async () => {
      // Set cookies for authenticated session
      await page.setCookie(...cookies);
      
      await page.goto('http://localhost:4000/profile');
      
      // Update profile
      await page.click('#editProfileBtn');
      await page.type('#name', 'Updated Name');
      await page.click('#saveBtn');
      
      await page.waitForSelector('.alert-success');
      const successMessage = await page.$eval('.alert-success', el => el.textContent);
      expect(successMessage).toContain('Profile updated');
      
      // Verify update in database
      const updatedUser = await User.findOne({ email: 'test@example.com' });
      expect(updatedUser.name).toBe('Updated Name');
    });
    
    it('should change password', async () => {
      await page.setCookie(...cookies);
      await page.goto('http://localhost:4000/profile/change-password');
      
      await page.type('#currentPassword', 'Password123!');
      await page.type('#newPassword', 'NewPassword123!');
      await page.type('#confirmPassword', 'NewPassword123!');
      
      await page.click('#submitBtn');
      
      await page.waitForSelector('.alert-success');
      const successMessage = await page.$eval('.alert-success', el => el.textContent);
      expect(successMessage).toContain('Password changed');
      
      // Verify can login with new password
      await page.click('#logoutBtn');
      await page.waitForNavigation();
      
      await page.goto('http://localhost:4000/login');
      await page.type('#email', 'test@example.com');
      await page.type('#password', 'NewPassword123!');
      await page.click('#submitBtn');
      
      await page.waitForNavigation();
      expect(page.url()).toContain('/dashboard');
    });
  });
  
  describe('User Logout Flow', () => {
    let cookies;
    
    beforeEach(async () => {
      // Create user and login
      await User.create({
        name: 'Test User',
        email: 'test@example.com',
        password: 'Password123!'
      });
      
      await page.goto('http://localhost:4000/api/v1/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        postData: JSON.stringify({
          email: 'test@example.com',
          password: 'Password123!'
        })
      });
      
      cookies = await page.cookies();
    });
    
    it('should logout successfully', async () => {
      await page.setCookie(...cookies);
      await page.goto('http://localhost:4000/dashboard');
      
      // Verify user is logged in
      const welcomeMessage = await page.$eval('.welcome-message', el => el.textContent);
      expect(welcomeMessage).toContain('Welcome');
      
      // Logout
      await page.click('#logoutBtn');
      await page.waitForNavigation();
      
      // Verify redirected to login page
      expect(page.url()).toContain('/login');
      
      // Try to access protected page
      await page.goto('http://localhost:4000/dashboard');
      expect(page.url()).toContain('/login'); // Should redirect to login
    });
  });
});
```

### 2. Testes de Performance
```javascript
// tests/performance/loadTest.js
const autocannon = require('autocannon');
const { promisify } = require('util');
const app = require('../../src/app');

const run = promisify(autocannon);

describe('Performance Tests', () => {
  let server;
  
  beforeAll(async () => {
    server = app.listen(0); // Random port
  });
  
  afterAll(async () => {
    server.close();
  });
  
  describe('Load Testing', () => {
    it('should handle 100 requests per second', async () => {
      const port = server.address().port;
      
      const result = await run({
        url: `http://localhost:${port}`,
        connections: 10,
        pipelining: 1,
        duration: 10,
        requests: [
          {
            method: 'GET',
            path: '/api/v1/health'
          }
        ]
      });
      
      console.log('Load Test Results:');
      console.log(`Requests/sec: ${result.requests.average}`);
      console.log(`Latency (avg): ${result.latency.average}ms`);
      console.log(`Errors: ${result.errors}`);
      
      expect(result.requests.average).toBeGreaterThan(100);
      expect(result.errors).toBe(0);
      expect(result.latency.average).toBeLessThan(100);
    });
    
    it('should handle concurrent user registration', async () => {
      const port = server.address().port;
      
      const result = await run({
        url: `http://localhost:${port}`,
        connections: 50,
        duration: 30,
        requests: [
          {
            method: 'POST',
            path: '/api/v1/auth/register',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              name: 'Load Test User',
              email: `loadtest${Date.now()}@example.com`,
              password: 'Password123!',
              passwordConfirm: 'Password123!'
            })
          }
        ]
      });
      
      console.log('Registration Load Test Results:');
      console.log(`Throughput: ${result.throughput.average} bytes/sec`);
      console.log(`Non-2xx responses: ${result['non2xx']}`);
      
      expect(result['2xx']).toBeGreaterThan(0);
      expect(result.timeouts).toBe(0);
    });
  });
  
  describe('Stress Testing', () => {
    it('should handle 1000 concurrent connections', async () => {
      const port = server.address().port;
      
      const result = await run({
        url: `http://localhost:${port}`,
        connections: 1000,
        duration: 60,
        timeout: 30,
        requests: [
          {
            method: 'GET',
            path: '/api/v1/health'
          }
        ]
      });
      
      console.log('Stress Test Results:');
      console.log(`Total requests: ${result.requests.total}`);
      console.log(`Timeouts: ${result.timeouts}`);
      console.log(`Errors: ${result.errors}`);
      
      expect(result.errors).toBeLessThan(10); // Allow some errors under extreme load
      expect(result.timeouts).toBe(0);
    });
  });
  
  describe('Endpoint Performance', () => {
    const endpoints = [
      { path: '/api/v1/health', method: 'GET' },
      { path: '/api/v1/products', method: 'GET' },
      { path: '/api/v1/users', method: 'GET' }
    ];
    
    endpoints.forEach(endpoint => {
      it(`should respond quickly to ${endpoint.method} ${endpoint.path}`, async () => {
        const port = server.address().port;
        
        const result = await run({
          url: `http://localhost:${port}`,
          connections: 10,
          duration: 10,
          requests: [
            {
              method: endpoint.method,
              path: endpoint.path
            }
          ]
        });
        
        console.log(`${endpoint.path} Performance:`);
        console.log(`Avg Latency: ${result.latency.average}ms`);
        console.log(`P99 Latency: ${result.latency.p99}ms`);
        
        expect(result.latency.average).toBeLessThan(200);
        expect(result.errors).toBe(0);
      });
    });
  });
});

// tests/performance/memoryTest.js
const { performance, PerformanceObserver } = require('perf_hooks');
const cluster = require('cluster');
const os = require('os');

describe('Memory and CPU Performance', () => {
  describe('Memory Usage', () => {
    it('should not have memory leaks', async () => {
      const initialMemory = process.memoryUsage().heapUsed;
      const requests = [];
      
      // Simulate many requests
      for (let i = 0; i < 10000; i++) {
        requests.push(Promise.resolve(i));
      }
      
      await Promise.all(requests);
      
      // Force garbage collection if available
      if (global.gc) {
        global.gc();
      }
      
      const finalMemory = process.memoryUsage().heapUsed;
      const memoryIncrease = finalMemory - initialMemory;
      
      console.log(`Memory increase: ${(memoryIncrease / 1024 / 1024).toFixed(2)} MB`);
      
      expect(memoryIncrease).toBeLessThan(50 * 1024 * 1024); // Less than 50MB increase
    });
    
    it('should handle large datasets efficiently', async () => {
      const largeArray = Array.from({ length: 1000000 }, (_, i) => ({
        id: i,
        name: `Item ${i}`,
        data: 'x'.repeat(100)
      }));
      
      const startMemory = process.memoryUsage();
      console.log('Start memory:', startMemory);
      
      // Process large array
      const processed = largeArray.map(item => ({
        ...item,
        processed: true
      }));
      
      const endMemory = process.memoryUsage();
      console.log('End memory:', endMemory);
      
      const memoryIncrease = endMemory.heapUsed - startMemory.heapUsed;
      console.log(`Memory used for processing: ${(memoryIncrease / 1024 / 1024).toFixed(2)} MB`);
      
      expect(processed.length).toBe(1000000);
    });
  });
  
  describe('CPU Performance', () => {
    it('should process CPU-intensive tasks efficiently', () => {
      const startTime = performance.now();
      
      // CPU-intensive task
      let result = 0;
      for (let i = 0; i < 10000000; i++) {
        result += Math.sqrt(i) * Math.sin(i);
      }
      
      const endTime = performance.now();
      const duration = endTime - startTime;
      
      console.log(`CPU task completed in ${duration.toFixed(2)}ms`);
      
      expect(duration).toBeLessThan(1000); // Should complete in under 1 second
    });
    
    it('should utilize multiple cores with clustering', (done) => {
      if (cluster.isMaster) {
        const numCPUs = os.cpus().length;
        const workers = [];
        
        for (let i = 0; i < numCPUs; i++) {
          const worker = cluster.fork();
          workers.push(worker);
        }
        
        let completed = 0;
        
        cluster.on('message', (worker, message) => {
          if (message === 'done') {
            completed++;
            
            if (completed === numCPUs) {
              workers.forEach(w => w.kill());
              done();
            }
          }
        });
      } else {
        // Worker process - do CPU work
        let result = 0;
        for (let i = 0; i < 1000000; i++) {
          result += Math.sqrt(i);
        }
        
        process.send('done');
      }
    });
  });
  
  describe('Event Loop Monitoring', () => {
    it('should not block event loop', async () => {
      const obs = new PerformanceObserver((list) => {
        const entries = list.getEntries();
        entries.forEach(entry => {
          console.log(`Event loop lag: ${entry.duration.toFixed(2)}ms`);
          expect(entry.duration).toBeLessThan(100); // Should not lag more than 100ms
        });
      });
      
      obs.observe({ entryTypes: ['measure'], buffered: true });
      
      // Schedule async tasks
      const promises = [];
      for (let i = 0; i < 1000; i++) {
        promises.push(new Promise(resolve => {
          setTimeout(() => {
            performance.mark(`start-${i}`);
            // Simulate some work
            let sum = 0;
            for (let j = 0; j < 1000; j++) {
              sum += Math.random();
            }
            performance.mark(`end-${i}`);
            performance.measure(`task-${i}`, `start-${i}`, `end-${i}`);
            resolve(sum);
          }, Math.random() * 10);
        }));
      }
      
      await Promise.all(promises);
      obs.disconnect();
    });
  });
});
```

### 3. Testes de Segurança
```javascript
// tests/security/security.test.js
const request = require('supertest');
const app = require('../../src/app');
const crypto = require('crypto');

describe('Security Tests', () => {
  describe('Input Validation', () => {
    it('should prevent SQL injection in user input', async () => {
      const sqlInjectionAttempts = [
        "' OR '1'='1",
        "'; DROP TABLE users; --",
        "' UNION SELECT * FROM users --",
        "admin' --",
        "1' AND '1'='1"
      ];
      
      for (const attempt of sqlInjectionAttempts) {
        const response = await request(app)
          .post('/api/v1/auth/login')
          .send({
            email: attempt,
            password: attempt
          })
          .expect(400); // Should reject with validation error
        
        expect(response.body.status).toBe('fail');
        expect(response.body.message).toContain('Validation');
      }
    });
    
    it('should prevent XSS attacks in input fields', async () => {
      const xssAttempts = [
        '<script>alert("XSS")</script>',
        '<img src="x" onerror="alert(\'XSS\')">',
        'javascript:alert("XSS")',
        '<svg onload="alert(\'XSS\')">',
        '"><script>alert("XSS")</script>'
      ];
      
      for (const attempt of xssAttempts) {
        const response = await request(app)
          .post('/api/v1/users')
          .send({
            name: attempt,
            email: 'test@example.com',
            password: 'Password123!'
          })
          .expect(400);
        
        expect(response.body.status).toBe('fail');
        // Input should be sanitized or rejected
      }
    });
    
    it('should validate and sanitize file uploads', async () => {
      const maliciousFiles = [
        { name: 'virus.exe', type: 'application/x-msdownload' },
        { name: 'script.php', type: 'application/x-httpd-php' },
        { name: 'malware.bat', type: 'application/bat' }
      ];
      
      for (const file of maliciousFiles) {
        const response = await request(app)
          .post('/api/v1/upload')
          .attach('file', Buffer.from('malicious content'), file.name)
          .set('Content-Type', 'multipart/form-data')
          .expect(400);
        
        expect(response.body.status).toBe('fail');
      }
    });
  });
  
  describe('Authentication Security', () => {
    it('should enforce strong password policy', async () => {
      const weakPasswords = [
        '123456',
        'password',
        'qwerty',
        'abc123',
        'admin123',
        'senha'
      ];
      
      for (const password of weakPasswords) {
        const response = await request(app)
          .post('/api/v1/auth/register')
          .send({
            name: 'Test User',
            email: `test${Date.now()}@example.com`,
            password: password,
            passwordConfirm: password
          })
          .expect(400);
        
        expect(response.body.status).toBe('fail');
        expect(response.body.message).toContain('Validation');
      }
    });
    
    it('should prevent brute force attacks with rate limiting', async () => {
      const credentials = {
        email: 'test@example.com',
        password: 'WrongPassword'
      };
      
      // Make multiple rapid requests
      for (let i = 0; i < 10; i++) {
        await request(app)
          .post('/api/v1/auth/login')
          .send(credentials);
      }
      
      // Should be rate limited
      const response = await request(app)
        .post('/api/v1/auth/login')
        .send(credentials)
        .expect(429); // Too Many Requests
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('Too many requests');
    });
    
    it('should invalidate session after logout', async () => {
      // First, register and login
      const registerResponse = await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'Password123!',
          passwordConfirm: 'Password123!'
        })
        .expect(201);
      
      const token = registerResponse.body.data.token;
      
      // Access protected route
      await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      // Logout
      await request(app)
        .post('/api/v1/auth/logout')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      // Try to access protected route again (should fail)
      await request(app)
        .get('/api/v1/auth/me')
        .set('Authorization', `Bearer ${token}`)
        .expect(401);
    });
    
    it('should prevent session fixation', async () => {
      // This test would require checking that session IDs are regenerated
      // after login and that old sessions are invalidated
      expect(true).toBe(true); // Placeholder for actual test
    });
  });
  
  describe('Authorization Security', () => {
    let userToken;
    let adminToken;
    
    beforeEach(async () => {
      // Create regular user
      const userResponse = await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Regular User',
          email: 'user@example.com',
          password: 'Password123!',
          passwordConfirm: 'Password123!',
          role: 'user'
        });
      
      userToken = userResponse.body.data.token;
      
      // Create admin user (would typically be seeded)
      const adminResponse = await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Admin User',
          email: 'admin@example.com',
          password: 'AdminPassword123!',
          passwordConfirm: 'AdminPassword123!',
          role: 'admin'
        });
      
      adminToken = adminResponse.body.data.token;
    });
    
    it('should prevent regular users from accessing admin routes', async () => {
      const adminRoutes = [
        { method: 'GET', path: '/api/v1/admin/users' },
        { method: 'POST', path: '/api/v1/admin/users' },
        { method: 'DELETE', path: '/api/v1/admin/users/123' }
      ];
      
      for (const route of adminRoutes) {
        const response = await request(app)
          [route.method.toLowerCase()](route.path)
          .set('Authorization', `Bearer ${userToken}`)
          .expect(403); // Forbidden
        
        expect(response.body.status).toBe('fail');
        expect(response.body.message).toContain('permission');
      }
    });
    
    it('should allow admin users to access admin routes', async () => {
      const response = await request(app)
        .get('/api/v1/admin/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .expect(200);
      
      expect(response.body.status).toBe('success');
    });
    
    it('should prevent IDOR (Insecure Direct Object Reference)', async () => {
      // User tries to access another user's data
      const response = await request(app)
        .get('/api/v1/users/another-user-id')
        .set('Authorization', `Bearer ${userToken}`)
        .expect(403); // Should be forbidden unless it's their own data
      
      expect(response.body.status).toBe('fail');
    });
  });
  
  describe('API Security Headers', () => {
    it('should have security headers set', async () => {
      const response = await request(app)
        .get('/api/v1/health')
        .expect(200);
      
      // Check security headers
      expect(response.headers['x-content-type-options']).toBe('nosniff');
      expect(response.headers['x-frame-options']).toBe('DENY');
      expect(response.headers['x-xss-protection']).toBe('1; mode=block');
      expect(response.headers['strict-transport-security']).toContain('max-age=');
      expect(response.headers['content-security-policy']).toBeDefined();
    });
    
    it('should not expose sensitive headers', async () => {
      const response = await request(app)
        .get('/api/v1/health')
        .expect(200);
      
      // Should not expose these headers
      expect(response.headers['x-powered-by']).toBeUndefined();
      expect(response.headers['server']).toBeUndefined();
    });
  });
  
  describe('CORS Configuration', () => {
    it('should allow requests from allowed origins', async () => {
      const response = await request(app)
        .get('/api/v1/health')
        .set('Origin', 'https://allowed-domain.com')
        .expect(200);
      
      expect(response.headers['access-control-allow-origin']).toBe('https://allowed-domain.com');
    });
    
    it('should reject requests from disallowed origins', async () => {
      const response = await request(app)
        .get('/api/v1/health')
        .set('Origin', 'https://malicious-site.com')
        .expect(200); // CORS errors don't typically return HTTP errors
      
      // The request might succeed but without CORS headers
      expect(response.headers['access-control-allow-origin']).not.toBe('https://malicious-site.com');
    });
  });
  
  describe('CSRF Protection', () => {
    it('should require CSRF token for state-changing operations', async () => {
      const operations = [
        { method: 'POST', path: '/api/v1/users' },
        { method: 'PUT', path: '/api/v1/users/123' },
        { method: 'DELETE', path: '/api/v1/users/123' }
      ];
      
      for (const op of operations) {
        const response = await request(app)
          [op.method.toLowerCase()](op.path)
          .send({})
          .expect(403); // Should require CSRF token
        
        expect(response.body.status).toBe('fail');
        expect(response.body.message).toContain('CSRF');
      }
    });
    
    it('should accept requests with valid CSRF token', async () => {
      // First get a CSRF token
      const getResponse = await request(app)
        .get('/api/v1/csrf-token')
        .expect(200);
      
      const csrfToken = getResponse.body.csrfToken;
      
      // Use it in a POST request
      const postResponse = await request(app)
        .post('/api/v1/users')
        .set('X-CSRF-Token', csrfToken)
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'Password123!'
        })
        .expect(201); // Should succeed with valid token
      
      expect(postResponse.body.status).toBe('success');
    });
  });
  
  describe('Cryptography', () => {
    it('should use secure hashing for passwords', async () => {
      const password = 'MySecurePassword123!';
      
      const response = await request(app)
        .post('/api/v1/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: password,
          passwordConfirm: password
        })
        .expect(201);
      
      // The response should not contain the plain password
      expect(response.body.data.user.password).toBeUndefined();
      
      // In a real test, you would verify the hash in the database
      // is a proper bcrypt hash
    });
    
    it('should use HTTPS in production', () => {
      // This would be an environment check
      if (process.env.NODE_ENV === 'production') {
        expect(process.env.FORCE_HTTPS).toBe('true');
      }
    });
    
    it('should have secure session configuration', () => {
      const sessionConfig = require('../../src/config/session');
      
      expect(sessionConfig.cookie.secure).toBe(process.env.NODE_ENV === 'production');
      expect(sessionConfig.cookie.httpOnly).toBe(true);
      expect(sessionConfig.cookie.sameSite).toBe('strict');
    });
  });
  
  describe('Error Handling Security', () => {
    it('should not leak sensitive information in error messages', async () => {
      // Trigger an error
      const response = await request(app)
        .get('/api/v1/nonexistent-route')
        .expect(404);
      
      // Error message should not contain stack traces or internal details
      expect(response.body.error).not.toContain('at');
      expect(response.body.error).not.toContain('node_modules');
      expect(response.body.error).not.toContain('/src/');
      
      if (process.env.NODE_ENV === 'production') {
        expect(response.body.stack).toBeUndefined();
      }
    });
    
    it('should handle malformed JSON gracefully', async () => {
      const response = await request(app)
        .post('/api/v1/auth/login')
        .set('Content-Type', 'application/json')
        .send('malformed json {')
        .expect(400);
      
      expect(response.body.status).toBe('fail');
      expect(response.body.message).toContain('JSON');
    });
  });
});
```

---

## 🐳 Docker e Node.js

### 1. Dockerfile Otimizado
```dockerfile
# Dockerfile
# Build stage
FROM node:18-alpine AS builder

# Install build dependencies
RUN apk add --no-cache python3 make g++

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./
COPY yarn.lock ./

# Install dependencies
RUN npm ci --only=production && npm cache clean --force

# Copy source code
COPY . .

# Remove dev dependencies if any were installed
RUN npm prune --production

# Production stage
FROM node:18-alpine AS production

# Install runtime dependencies
RUN apk add --no-cache tini curl

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Set working directory
WORKDIR /app

# Copy from builder
COPY --from=builder /app /app
COPY --from=builder /app/node_modules ./node_modules

# Change ownership
RUN chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Use tini as init process
ENTRYPOINT ["/sbin/tini", "--"]

# Start application
CMD ["node", "src/index.js"]

# Labels
LABEL maintainer="team@example.com"
LABEL version="1.0.0"
LABEL description="Node.js Application"
```

### 2. Docker Compose para Desenvolvimento
```yaml
# docker-compose.yml
version: '3.8'

services:
  # Node.js Application
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    container_name: node-app
    ports:
      - "3000:3000"
      - "9229:9229" # Debug port
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=mongodb://mongodb:27017/appdb
      - REDIS_URL=redis://redis:6379
      - PORT=3000
    depends_on:
      - mongodb
      - redis
    networks:
      - app-network
    command: npm run dev
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  # MongoDB
  mongodb:
    image: mongo:6
    container_name: mongodb
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=secret
      - MONGO_INITDB_DATABASE=appdb
    volumes:
      - mongodb_data:/data/db
      - ./mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro
    networks:
      - app-network
    command: mongod --auth

  # Redis
  redis:
    image: redis:7-alpine
    container_name: redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - app-network
    command: redis-server --appendonly yes

  # MongoDB GUI (optional)
  mongo-express:
    image: mongo-express:latest
    container_name: mongo-express
    ports:
      - "8081:8081"
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=secret
      - ME_CONFIG_MONGODB_URL=mongodb://admin:secret@mongodb:27017/
      - ME_CONFIG_BASICAUTH_USERNAME=admin
      - ME_CONFIG_BASICAUTH_PASSWORD=admin123
    depends_on:
      - mongodb
    networks:
      - app-network

  # Redis GUI (optional)
  redis-commander:
    image: rediscommander/redis-commander:latest
    container_name: redis-commander
    ports:
      - "8082:8081"
    environment:
      - REDIS_HOSTS=local:redis:6379
    depends_on:
      - redis
    networks:
      - app-network

  # Nginx for production-like setup
  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - app
    networks:
      - app-network

  # PostgreSQL (optional alternative)
  postgres:
    image: postgres:15-alpine
    container_name: postgres
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=appdb
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./postgres-init.sql:/docker-entrypoint-initdb.d/postgres-init.sql:ro
    networks:
      - app-network

  # Monitoring
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'
      - '--storage.tsdb.retention.time=200h'
      - '--web.enable-lifecycle'
    networks:
      - app-network

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/provisioning:/etc/grafana/provisioning:ro
    depends_on:
      - prometheus
    networks:
      - app-network

  # Logging
  loki:
    image: grafana/loki:latest
    container_name: loki
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml
    volumes:
      - loki_data:/loki
    networks:
      - app-network

  promtail:
    image: grafana/promtail:latest
    container_name: promtail
    volumes:
      - /var/log:/var/log
      - ./promtail-config.yml:/etc/promtail/config.yml:ro
    command: -config.file=/etc/promtail/config.yml
    depends_on:
      - loki
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
    name: app-network

volumes:
  mongodb_data:
    driver: local
  redis_data:
    driver: local
  postgres_data:
    driver: local
  prometheus_data:
    driver: local
  grafana_data:
    driver: local
  loki_data:
    driver: local
```

### 3. Docker Compose para Produção
```yaml
# docker-compose.prod.yml
version: '3.8'

services:
  # Node.js Application
  app:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - NODE_ENV=production
    container_name: node-app-prod
    restart: unless-stopped
    expose:
      - "3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
      - JWT_SECRET=${JWT_SECRET}
      - PORT=3000
    env_file:
      - .env.production
    secrets:
      - jwt_secret
      - db_password
    depends_on:
      - mongodb
      - redis
    networks:
      - app-network-prod
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
        order: start-first
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
      resources:
        limits:
          cpus: '1'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  # MongoDB with Replica Set
  mongodb:
    image: mongo:6
    container_name: mongodb-prod
    restart: unless-stopped
    command: mongod --replSet rs0 --bind_ip_all
    environment:
      - MONGO_INITDB_ROOT_USERNAME=${MONGO_USERNAME}
      - MONGO_INITDB_ROOT_PASSWORD_FILE=/run/secrets/mongo_password
    secrets:
      - mongo_password
    volumes:
      - mongodb_data:/data/db
      - ./mongo-keyfile:/data/keyfile:ro
    networks:
      - app-network-prod
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongosh localhost:27017/test --quiet
      interval: 30s
      timeout: 10s
      retries: 3

  # Redis with Persistence
  redis:
    image: redis:7-alpine
    container_name: redis-prod
    restart: unless-stopped
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    environment:
      - REDIS_PASSWORD_FILE=/run/secrets/redis_password
    secrets:
      - redis_password
    volumes:
      - redis_data:/data
      - ./redis.conf:/usr/local/etc/redis/redis.conf:ro
    networks:
      - app-network-prod
    healthcheck:
      test: ["CMD", "redis-cli", "--raw", "incr", "ping"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Nginx Load Balancer
  nginx:
    image: nginx:alpine
    container_name: nginx-prod
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
      - ./nginx/html:/usr/share/nginx/html:ro
      - ./nginx/logs:/var/log/nginx
    depends_on:
      - app
    networks:
      - app-network-prod
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role == manager

  # Traefik for Service Discovery
  traefik:
    image: traefik:v2.10
    container_name: traefik
    restart: unless-stopped
    command:
      - --api.dashboard=true
      - --api.insecure=true
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --certificatesresolvers.leresolver.acme.tlschallenge=true
      - --certificatesresolvers.leresolver.acme.email=${ACME_EMAIL}
      - --certificatesresolvers.leresolver.acme.storage=/letsencrypt/acme.json
    ports:
      - "80:80"
      - "443:443"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./traefik/letsencrypt:/letsencrypt
    networks:
      - app-network-prod
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.api.rule=Host(`traefik.${DOMAIN}`)"
      - "traefik.http.routers.api.service=api@internal"
      - "traefik.http.routers.api.entrypoints=websecure"
      - "traefik.http.routers.api.tls.certresolver=leresolver"

networks:
  app-network-prod:
    driver: overlay
    attachable: true
    external: true

volumes:
  mongodb_data:
    driver: local
  redis_data:
    driver: local

secrets:
  jwt_secret:
    file: ./secrets/jwt_secret.txt
  db_password:
    file: ./secrets/db_password.txt
  mongo_password:
    file: ./secrets/mongo_password.txt
  redis_password:
    file: ./secrets/redis_password.txt
```

### 4. Kubernetes Deployment
```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
  namespace: production
  labels:
    app: node-app
    version: v1.0.0
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  minReadySeconds: 30
  template:
    metadata:
      labels:
        app: node-app
        version: v1.0.0
    spec:
      containers:
      - name: node-app
        image: your-registry/node-app:v1.0.0
        imagePullPolicy: Always
        ports:
        - containerPort: 3000
          name: http
        env:
        - name: NODE_ENV
          value: "production"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: database-url
        - name: REDIS_URL
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: redis-url
        - name: JWT_SECRET
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: jwt-secret
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
          timeoutSeconds: 5
          failureThreshold: 3
        readinessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
          timeoutSeconds: 3
          failureThreshold: 1
        startupProbe:
          httpGet:
            path: /health
            port: 3000
          failureThreshold: 30
          periodSeconds: 10
        lifecycle:
          preStop:
            exec:
              command: ["/bin/sh", "-c", "sleep 30"]
        securityContext:
          runAsNonRoot: true
          runAsUser: 1000
          allowPrivilegeEscalation: false
          capabilities:
            drop:
            - ALL
        volumeMounts:
        - name: app-logs
          mountPath: /app/logs
        - name: tmp-volume
          mountPath: /tmp
      volumes:
      - name: app-logs
        emptyDir: {}
      - name: tmp-volume
        emptyDir: {}
      securityContext:
        fsGroup: 1000
      imagePullSecrets:
      - name: regcred
---
# k8s/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
  namespace: production
spec:
  selector:
    app: node-app
  ports:
  - port: 80
    targetPort: 3000
    protocol: TCP
    name: http
  - port: 443
    targetPort: 3000
    protocol: TCP
    name: https
  type: ClusterIP
---
# k8s/ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: node-app-ingress
  namespace: production
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: "10m"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - api.example.com
    secretName: node-app-tls
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: node-app-service
            port:
              number: 80
---
# k8s/hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: node-app-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: node-app
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 10
        periodSeconds: 60
    scaleUp:
      stabilizationWindowSeconds: 60
      policies:
      - type: Percent
        value: 100
        periodSeconds: 60
---
# k8s/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: production
data:
  NODE_ENV: "production"
  LOG_LEVEL: "info"
  CORS_ORIGIN: "https://example.com"
  RATE_LIMIT_WINDOW: "900000"
  RATE_LIMIT_MAX: "100"
---
# k8s/secrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
  namespace: production
type: Opaque
data:
  database-url: BASE64_ENCODED_STRING
  redis-url: BASE64_ENCODED_STRING
  jwt-secret: BASE64_ENCODED_STRING
  admin-email: BASE64_ENCODED_STRING
  admin-password: BASE64_ENCODED_STRING
```

---

## 🚀 Deploy e Produção

### 1. Configuração de Ambiente
```javascript
// config/env.js
require('dotenv').config();

const env = process.env.NODE_ENV || 'development';

const config = {
  development: {
    PORT: process.env.PORT || 3000,
    MONGODB_URI: process.env.MONGODB_URI || 'mongodb://localhost:27017/app_dev',
    REDIS_URL: process.env.REDIS_URL || 'redis://localhost:6379',
    JWT_SECRET: process.env.JWT_SECRET || 'development-secret',
    JWT_EXPIRES_IN: '7d',
    JWT_COOKIE_EXPIRES_IN: 7,
    EMAIL_HOST: process.env.EMAIL_HOST || 'smtp.ethereal.email',
    EMAIL_PORT: process.env.EMAIL_PORT || 587,
    EMAIL_USER: process.env.EMAIL_USER,
    EMAIL_PASS: process.env.EMAIL_PASS,
    CORS_ORIGIN: ['http://localhost:3000', 'http://localhost:8080'],
    LOG_LEVEL: 'debug',
    RATE_LIMIT_WINDOW: 15 * 60 * 1000, // 15 minutos
    RATE_LIMIT_MAX: 100,
    UPLOAD_MAX_SIZE: 5 * 1024 * 1024, // 5MB
    UPLOAD_PATH: 'uploads/'
  },
  
  test: {
    PORT: process.env.PORT || 3001,
    MONGODB_URI: process.env.MONGODB_URI || 'mongodb://localhost:27017/app_test',
    REDIS_URL: process.env.REDIS_URL || 'redis://localhost:6379',
    JWT_SECRET: 'test-secret',
    JWT_EXPIRES_IN: '1d',
    JWT_COOKIE_EXPIRES_IN: 1,
    CORS_ORIGIN: '*',
    LOG_LEVEL: 'error',
    RATE_LIMIT_WINDOW: 60 * 1000,
    RATE_LIMIT_MAX: 1000
  },
  
  production: {
    PORT: process.env.PORT || 3000,
    MONGODB_URI: process.env.MONGODB_URI,
    REDIS_URL: process.env.REDIS_URL,
    JWT_SECRET: process.env.JWT_SECRET,
    JWT_EXPIRES_IN: process.env.JWT_EXPIRES_IN || '1d',
    JWT_COOKIE_EXPIRES_IN: parseInt(process.env.JWT_COOKIE_EXPIRES_IN) || 1,
    EMAIL_HOST: process.env.EMAIL_HOST,
    EMAIL_PORT: process.env.EMAIL_PORT,
    EMAIL_USER: process.env.EMAIL_USER,
    EMAIL_PASS: process.env.EMAIL_PASS,
    CORS_ORIGIN: process.env.CORS_ORIGIN ? process.env.CORS_ORIGIN.split(',') : [],
    LOG_LEVEL: process.env.LOG_LEVEL || 'info',
    RATE_LIMIT_WINDOW: parseInt(process.env.RATE_LIMIT_WINDOW) || 15 * 60 * 1000,
    RATE_LIMIT_MAX: parseInt(process.env.RATE_LIMIT_MAX) || 100,
    UPLOAD_MAX_SIZE: parseInt(process.env.UPLOAD_MAX_SIZE) || 10 * 1024 * 1024,
    UPLOAD_PATH: process.env.UPLOAD_PATH || 'uploads/',
    SSL_CERT_PATH: process.env.SSL_CERT_PATH,
    SSL_KEY_PATH: process.env.SSL_KEY_PATH,
    CLUSTER_MODE: process.env.CLUSTER_MODE === 'true',
    TRUST_PROXY: process.env.TRUST_PROXY === 'true'
  }
};

module.exports = config[env];
```

### 2. PM2 para Gerenciamento de Processos
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'node-app',
    script: './src/index.js',
    instances: 'max',
    exec_mode: 'cluster',
    watch: false,
    max_memory_restart: '1G',
    env: {
      NODE_ENV: 'development',
      PORT: 3000
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 3000
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_file: './logs/combined.log',
    time: true,
    merge_logs: true,
    log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
    kill_timeout: 5000,
    wait_ready: true,
    listen_timeout: 5000,
    max_restarts: 10,
    restart_delay: 4000,
    autorestart: true,
    vizion: false,
    post_update: ['npm install', 'echo Launching...'],
    env: {
      NODE_ENV: 'development'
    },
    env_production: {
      NODE_ENV: 'production'
    }
  }],

  deploy: {
    production: {
      user: 'deploy',
      host: ['server1.example.com', 'server2.example.com'],
      ref: 'origin/main',
      repo: 'git@github.com:username/repo.git',
      path: '/var/www/node-app',
      'pre-deploy-local': '',
      'post-deploy': 'npm install && pm2 reload ecosystem.config.js --env production',
      'pre-setup': '',
      env: {
        NODE_ENV: 'production'
      }
    }
  }
};
```

### 3. Scripts de Deploy
```bash
#!/bin/bash
# deploy.sh

set -e

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Log function
log() {
    echo -e "${GREEN}[$(date +'%Y-%m-%d %H:%M:%S')]${NC} $1"
}

error() {
    echo -e "${RED}[$(date +'%Y-%m-%d %H:%M:%S')] ERROR:${NC} $1"
}

warn() {
    echo -e "${YELLOW}[$(date +'%Y-%m-%d %H:%M:%S')] WARN:${NC} $1"
}

# Check if running as root
if [ "$EUID" -eq 0 ]; then 
    warn "Running as root is not recommended"
fi

# Load environment
ENV=${1:-production}
log "Starting deployment for $ENV environment"

# Load environment variables
if [ -f ".env.$ENV" ]; then
    log "Loading environment variables from .env.$ENV"
    export $(cat .env.$ENV | grep -v '^#' | xargs)
else
    warn "No .env.$ENV file found"
fi

# Check dependencies
log "Checking dependencies..."
command -v node >/dev/null 2>&1 || { error "Node.js is not installed"; exit 1; }
command -v npm >/dev/null 2>&1 || { error "npm is not installed"; exit 1; }
command -v git >/dev/null 2>&1 || { error "git is not installed"; exit 1; }

# Backup current version
BACKUP_DIR="backups/$(date +%Y%m%d_%H%M%S)"
log "Creating backup in $BACKUP_DIR"
mkdir -p $BACKUP_DIR
cp -r ./* $BACKUP_DIR/ 2>/dev/null || true

# Pull latest code
log "Pulling latest code..."
git fetch origin
git reset --hard origin/main

# Install dependencies
log "Installing dependencies..."
npm ci --only=production

# Run tests
log "Running tests..."
if [ "$ENV" = "production" ]; then
    npm test -- --passWithNoTests
fi

# Build if needed
if [ -f "package.json" ] && grep -q '"build"' package.json; then
    log "Building application..."
    npm run build
fi

# Run database migrations
if [ -f "src/database/migrations" ]; then
    log "Running database migrations..."
    npm run migrate:up
fi

# Restart application
log "Restarting application..."
if command -v pm2 &> /dev/null; then
    if pm2 list | grep -q "node-app"; then
        pm2 reload node-app --update-env
    else
        pm2 start ecosystem.config.js --env $ENV
    fi
    pm2 save
else
    # Fallback to systemd or direct start
    if [ -f "/etc/systemd/system/node-app.service" ]; then
        sudo systemctl restart node-app
    else
        npm start &
    fi
fi

# Health check
log "Performing health check..."
sleep 5
HEALTH_CHECK_URL="http://localhost:${PORT:-3000}/health"
if curl -f $HEALTH_CHECK_URL > /dev/null 2>&1; then
    log "Health check passed"
else
    error "Health check failed"
    
    # Rollback
    log "Rolling back to previous version..."
    rm -rf ./*
    cp -r $BACKUP_DIR/* ./
    
    if command -v pm2 &> /dev/null; then
        pm2 reload node-app
    fi
    
    exit 1
fi

# Cleanup old backups (keep last 5)
log "Cleaning up old backups..."
ls -dt backups/* | tail -n +6 | xargs rm -rf 2>/dev/null || true

log "Deployment completed successfully!"
```

### 4. Monitoramento e Logs
```javascript
// src/utils/logger.js
const winston = require('winston');
const { combine, timestamp, printf, colorize, json } = winston.format;
const DailyRotateFile = require('winston-daily-rotate-file');
const path = require('path');

// Custom format
const customFormat = printf(({ level, message, timestamp, ...meta }) => {
  let log = `${timestamp} [${level}]: ${message}`;
  
  if (Object.keys(meta).length > 0) {
    log += ` ${JSON.stringify(meta)}`;
  }
  
  return log;
});

// Create logger instance
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: combine(
    timestamp({
      format: 'YYYY-MM-DD HH:mm:ss'
    }),
    json()
  ),
  defaultMeta: { service: 'node-app' },
  transports: [
    // Console transport
    new winston.transports.Console({
      format: combine(
        colorize(),
        customFormat
      ),
      level: 'debug'
    }),
    
    // Daily rotate file for errors
    new DailyRotateFile({
      filename: path.join('logs', 'error-%DATE%.log'),
      datePattern: 'YYYY-MM-DD',
      level: 'error',
      maxSize: '20m',
      maxFiles: '30d',
      format: combine(
        timestamp(),
        json()
      )
    }),
    
    // Daily rotate file for all logs
    new DailyRotateFile({
      filename: path.join('logs', 'combined-%DATE%.log'),
      datePattern: 'YYYY-MM-DD',
      maxSize: '20m',
      maxFiles: '30d',
      format: combine(
        timestamp(),
        json()
      )
    }),
    
    // Audit log
    new DailyRotateFile({
      filename: path.join('logs', 'audit-%DATE%.log'),
      datePattern: 'YYYY-MM-DD',
      level: 'audit',
      maxSize: '20m',
      maxFiles: '90d',
      format: combine(
        timestamp(),
        json()
      )
    })
  ],
