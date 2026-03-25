# 📚 Manual Completo do Módulo FS (File System) do Node.js

## Índice
1. [Introdução ao Módulo FS](#introdução)
2. [Importando o Módulo](#importando)
3. [Métodos Síncronos vs Assíncronos](#sincrono-vs-assincrono)
4. [Operações com Arquivos](#operacoes-arquivos)
   - [Leitura de Arquivos](#leitura)
   - [Escrita de Arquivos](#escrita)
   - [Anexar Conteúdo](#anexar)
   - [Copiar, Renomear e Mover](#copiar-renomear-mover)
   - [Excluir Arquivos](#excluir)
5. [Operações com Diretórios](#operacoes-diretorios)
   - [Criar Diretórios](#criar-diretorios)
   - [Listar Diretórios](#listar-diretorios)
   - [Remover Diretórios](#remover-diretorios)
6. [Informações de Arquivos e Diretórios](#informacoes)
   - [fs.stat](#stat)
   - [fs.access](#access)
7. [Trabalhando com Streams](#streams)
   - [ReadStream](#readstream)
   - [WriteStream](#writestream)
   - [Pipeline](#pipeline)
8. [File Descriptors](#file-descriptors)
9. [Watch e Observação de Arquivos](#watch)
10. [Links Simbólicos](#links)
11. [Melhores Práticas](#melhores-praticas)
12. [Exemplos Práticos](#exemplos-praticos)

---

## 1. Introdução ao Módulo FS {#introdução}

O módulo `fs` (File System) é um módulo nativo do Node.js que fornece uma API completa para interagir com o sistema de arquivos do computador. Ele permite ler, escrever, modificar, deletar arquivos e diretórios, além de obter informações sobre eles.

**Características principais:**
- Totalmente integrado ao ecossistema Node.js
- Suporte nativo a Promises, callbacks e versões síncronas
- Opera com sistema de arquivos do SO (Windows, Linux, macOS)
- Manipulação de arquivos grandes via streams
- Observação de alterações em tempo real

---

## 2. Importando o Módulo {#importando}

```javascript
// CommonJS (padrão)
const fs = require('fs');

// Para versões com suporte a ES Modules
import fs from 'fs';

// Para trabalhar com Promises (recomendado)
const fsPromises = require('fs').promises;

// Desestruturando métodos específicos
const { readFile, writeFile, stat } = require('fs').promises;
```

---

## 3. Métodos Síncronos vs Assíncronos {#sincrono-vs-assincrono}

### Métodos Assíncronos (Callback)
```javascript
// Não bloqueante - recomendado para aplicações em produção
fs.readFile('arquivo.txt', 'utf8', (err, data) => {
    if (err) {
        console.error('Erro:', err);
        return;
    }
    console.log(data);
});
```

### Métodos Síncronos
```javascript
// Bloqueante - use apenas em scripts de inicialização ou CLI
try {
    const data = fs.readFileSync('arquivo.txt', 'utf8');
    console.log(data);
} catch (err) {
    console.error('Erro:', err);
}
```

### Métodos com Promises (Recomendado)
```javascript
// Assíncrono com sintaxe moderna
const fs = require('fs').promises;

async function lerArquivo() {
    try {
        const data = await fs.readFile('arquivo.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error('Erro:', err);
    }
}
```

---

## 4. Operações com Arquivos {#operacoes-arquivos}

### Leitura de Arquivos {#leitura}

#### readFile - Leitura Completa
```javascript
const fs = require('fs').promises;

// Leitura de arquivo texto
async function lerArquivoTexto() {
    const conteudo = await fs.readFile('exemplo.txt', 'utf8');
    console.log(conteudo);
}

// Leitura de arquivo binário (imagem, PDF, etc)
async function lerArquivoBinario() {
    const buffer = await fs.readFile('imagem.jpg');
    console.log('Tamanho:', buffer.length, 'bytes');
    // buffer é um objeto Buffer com os dados binários
}

// Leitura parcial com opções
async function lerArquivoParcial() {
    const buffer = await fs.readFile('arquivo_grande.txt', {
        encoding: 'utf8',
        flag: 'r'
    });
}
```

#### createReadStream - Leitura em Streaming
```javascript
const { createReadStream } = require('fs');

function lerArquivoGrande(caminho) {
    const stream = createReadStream(caminho, {
        encoding: 'utf8',
        highWaterMark: 64 * 1024 // 64KB chunks
    });
    
    stream.on('data', (chunk) => {
        console.log(`Recebido chunk de ${chunk.length} bytes`);
        // Processar chunk
    });
    
    stream.on('end', () => {
        console.log('Leitura concluída');
    });
    
    stream.on('error', (err) => {
        console.error('Erro:', err);
    });
}
```

### Escrita de Arquivos {#escrita}

#### writeFile - Escrita Completa
```javascript
const fs = require('fs').promises;

// Escrever texto simples
async function escreverTexto() {
    await fs.writeFile('saida.txt', 'Conteúdo do arquivo', 'utf8');
    console.log('Arquivo criado com sucesso');
}

// Escrever JSON
async function escreverJSON() {
    const dados = {
        nome: 'João Silva',
        idade: 30,
        email: 'joao@email.com'
    };
    
    await fs.writeFile('dados.json', JSON.stringify(dados, null, 2), 'utf8');
}

// Escrever buffer (dados binários)
async function escreverBuffer() {
    const buffer = Buffer.from('Dados binários', 'utf8');
    await fs.writeFile('dados.bin', buffer);
}

// Opções avançadas
async function escreverComOpcoes() {
    await fs.writeFile('exemplo.txt', 'Conteúdo', {
        encoding: 'utf8',
        mode: 0o666, // permissões: rw-rw-rw-
        flag: 'w'    // 'w' = escrever, 'wx' = escrever se não existir
    });
}
```

#### createWriteStream - Escrita em Streaming
```javascript
const { createWriteStream } = require('fs');

function escreverGrandeArquivo(caminho) {
    const stream = createWriteStream(caminho);
    
    for (let i = 0; i < 1000000; i++) {
        const escreveu = stream.write(`Linha ${i}\n`);
        
        // Pausar se o buffer estiver cheio
        if (!escreveu) {
            stream.once('drain', () => {
                console.log('Buffer esvaziado, continuando...');
            });
        }
    }
    
    stream.end(() => {
        console.log('Escrita concluída');
    });
    
    stream.on('error', (err) => {
        console.error('Erro:', err);
    });
}
```

### Anexar Conteúdo {#anexar}

```javascript
const fs = require('fs').promises;

// appendFile - Adicionar ao final do arquivo
async function anexarConteudo() {
    await fs.appendFile('log.txt', `[${new Date().toISOString()}] Nova entrada\n`, 'utf8');
}

// appendFileSync - Versão síncrona
function anexarConteudoSync() {
    fs.appendFileSync('log.txt', 'Mais conteúdo\n', 'utf8');
}

// Anexar com createWriteStream
const { createWriteStream } = require('fs');
const stream = createWriteStream('log.txt', { flags: 'a' });
stream.write('Nova linha\n');
stream.end();
```

### Copiar, Renomear e Mover {#copiar-renomear-mover}

```javascript
const fs = require('fs').promises;
const path = require('path');

// Copiar arquivo
async function copiarArquivo() {
    await fs.copyFile('origem.txt', 'destino.txt');
    console.log('Arquivo copiado');
    
    // Com flags de cópia
    await fs.copyFile('origem.txt', 'destino.txt', fs.constants.COPYFILE_FICLONE);
}

// Renomear arquivo
async function renomearArquivo() {
    await fs.rename('antigo_nome.txt', 'novo_nome.txt');
}

// Mover arquivo para outro diretório
async function moverArquivo() {
    await fs.rename('arquivo.txt', path.join('pasta', 'arquivo.txt'));
}

// Renomear com verificação
async function renomearSeguro(antigo, novo) {
    try {
        await fs.access(novo);
        console.log('Arquivo destino já existe');
        return false;
    } catch {
        await fs.rename(antigo, novo);
        return true;
    }
}
```

### Excluir Arquivos {#excluir}

```javascript
const fs = require('fs').promises;

// unlink - Remover arquivo
async function removerArquivo() {
    await fs.unlink('arquivo_para_deletar.txt');
    console.log('Arquivo removido');
}

// unlinkSync - Versão síncrona
function removerArquivoSync() {
    fs.unlinkSync('arquivo.txt');
}

// Remover com verificação
async function removerSeguro(caminho) {
    try {
        const stats = await fs.stat(caminho);
        
        if (stats.isFile()) {
            await fs.unlink(caminho);
            console.log('Arquivo removido');
        } else {
            console.log('Não é um arquivo');
        }
    } catch (err) {
        if (err.code === 'ENOENT') {
            console.log('Arquivo não encontrado');
        } else {
            throw err;
        }
    }
}
```

---

## 5. Operações com Diretórios {#operacoes-diretorios}

### Criar Diretórios {#criar-diretorios}

```javascript
const fs = require('fs').promises;
const path = require('path');

// mkdir - Criar diretório simples
async function criarDiretorio() {
    await fs.mkdir('nova_pasta');
    console.log('Diretório criado');
}

// Criar diretórios aninhados
async function criarDiretoriosAninhados() {
    await fs.mkdir('pasta1/pasta2/pasta3', { recursive: true });
    console.log('Diretórios aninhados criados');
}

// Criar com permissões específicas
async function criarComPermissoes() {
    await fs.mkdir('pasta_restrita', { 
        mode: 0o750 // rwxr-x---
    });
}

// Criar diretório temporário único
const os = require('os');
const { mkdtemp } = require('fs').promises;

async function criarDirTemporario() {
    const tempDir = await mkdtemp(path.join(os.tmpdir(), 'meu-app-'));
    console.log('Diretório temporário:', tempDir);
    // usar o diretório...
    // depois limpar: await fs.rm(tempDir, { recursive: true });
}
```

### Listar Diretórios {#listar-diretorios}

```javascript
const fs = require('fs').promises;
const path = require('path');

// readdir - Listar arquivos e pastas
async function listarDiretorioSimples() {
    const arquivos = await fs.readdir('.');
    console.log('Arquivos:', arquivos);
}

// Listar com informações detalhadas
async function listarComDetalhes(diretorio = '.') {
    const arquivos = await fs.readdir(diretorio);
    
    for (const arquivo of arquivos) {
        const caminhoCompleto = path.join(diretorio, arquivo);
        const stats = await fs.stat(caminhoCompleto);
        
        const tipo = stats.isDirectory() ? '📁' : '📄';
        const tamanho = stats.isFile() ? `(${stats.size} bytes)` : '';
        const modificado = stats.mtime.toLocaleDateString();
        
        console.log(`${tipo} ${arquivo} ${tamanho} - Modificado: ${modificado}`);
    }
}

// Listar recursivamente (todas as subpastas)
async function listarRecursivamente(diretorio = '.', nivel = 0) {
    const indent = '  '.repeat(nivel);
    const arquivos = await fs.readdir(diretorio);
    
    for (const arquivo of arquivos) {
        const caminhoCompleto = path.join(diretorio, arquivo);
        const stats = await fs.stat(caminhoCompleto);
        
        if (stats.isDirectory()) {
            console.log(`${indent}📁 ${arquivo}/`);
            await listarRecursivamente(caminhoCompleto, nivel + 1);
        } else {
            console.log(`${indent}📄 ${arquivo} (${stats.size} bytes)`);
        }
    }
}

// Filtrar por extensão
async function listarPorExtensao(diretorio, extensao) {
    const arquivos = await fs.readdir(diretorio);
    const filtrados = arquivos.filter(arquivo => arquivo.endsWith(extensao));
    
    console.log(`Arquivos .${extensao}:`, filtrados);
    return filtrados;
}
```

### Remover Diretórios {#remover-diretorios}

```javascript
const fs = require('fs').promises;

// rmdir - Remover diretório vazio
async function removerDirVazio() {
    await fs.rmdir('pasta_vazia');
    console.log('Diretório vazio removido');
}

// rm - Remover diretório com conteúdo (Node.js 14+)
async function removerDirComConteudo() {
    await fs.rm('pasta_com_conteudo', { 
        recursive: true,  // Remove conteúdo recursivamente
        force: true       // Ignora se não existir
    });
    console.log('Diretório e conteúdo removidos');
}

// Remover diretório com conteúdo manualmente
async function removerRecursivamente(caminho) {
    const stats = await fs.stat(caminho);
    
    if (stats.isDirectory()) {
        const arquivos = await fs.readdir(caminho);
        
        for (const arquivo of arquivos) {
            await removerRecursivamente(`${caminho}/${arquivo}`);
        }
        
        await fs.rmdir(caminho);
    } else {
        await fs.unlink(caminho);
    }
}

// Limpar diretório (remover conteúdo mas manter a pasta)
async function limparDiretorio(caminho) {
    const arquivos = await fs.readdir(caminho);
    
    for (const arquivo of arquivos) {
        const caminhoCompleto = `${caminho}/${arquivo}`;
        const stats = await fs.stat(caminhoCompleto);
        
        if (stats.isDirectory()) {
            await fs.rm(caminhoCompleto, { recursive: true });
        } else {
            await fs.unlink(caminhoCompleto);
        }
    }
    
    console.log('Diretório limpo');
}
```

---

## 6. Informações de Arquivos e Diretórios {#informacoes}

### fs.stat {#stat}

```javascript
const fs = require('fs').promises;

async function obterInformacoes(caminho) {
    const stats = await fs.stat(caminho);
    
    console.log('=== Informações do Arquivo ===');
    console.log('É arquivo:', stats.isFile());
    console.log('É diretório:', stats.isDirectory());
    console.log('É link simbólico:', stats.isSymbolicLink());
    console.log('Tamanho:', stats.size, 'bytes');
    console.log('Tamanho formatado:', formatBytes(stats.size));
    console.log('Data de criação:', stats.birthtime);
    console.log('Último acesso:', stats.atime);
    console.log('Última modificação:', stats.mtime);
    console.log('Última mudança de status:', stats.ctime);
    console.log('Permissões:', stats.mode.toString(8));
    console.log('Número de links:', stats.nlink);
    console.log('ID do usuário:', stats.uid);
    console.log('ID do grupo:', stats.gid);
    
    return stats;
}

function formatBytes(bytes) {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
}

// Monitorar mudanças no arquivo
async function monitorarArquivo(caminho) {
    let ultimoMtime = (await fs.stat(caminho)).mtime;
    
    setInterval(async () => {
        try {
            const stats = await fs.stat(caminho);
            if (stats.mtime > ultimoMtime) {
                console.log('Arquivo foi modificado!');
                ultimoMtime = stats.mtime;
            }
        } catch (err) {
            console.log('Arquivo não encontrado');
        }
    }, 1000);
}
```

### fs.access {#access}

```javascript
const fs = require('fs').promises;
const fsConstants = require('fs').constants;

// Verificar permissões de arquivo
async function verificarPermissoes(caminho) {
    try {
        await fs.access(caminho, fsConstants.F_OK);
        console.log('Arquivo existe');
        
        await fs.access(caminho, fsConstants.R_OK);
        console.log('Pode ser lido');
        
        await fs.access(caminho, fsConstants.W_OK);
        console.log('Pode ser escrito');
        
        await fs.access(caminho, fsConstants.X_OK);
        console.log('Pode ser executado');
        
        return true;
    } catch (err) {
        console.error('Acesso negado ou arquivo não existe');
        return false;
    }
}

// Verificar combinações de permissões
async function verificarPermissoesCombinadas(caminho) {
    const permissoes = [
        { flag: fsConstants.R_OK, nome: 'leitura' },
        { flag: fsConstants.W_OK, nome: 'escrita' },
        { flag: fsConstants.X_OK, nome: 'execução' }
    ];
    
    for (const permissao of permissoes) {
        try {
            await fs.access(caminho, permissao.flag);
            console.log(`✓ Permissão de ${permissao.nome}: SIM`);
        } catch {
            console.log(`✗ Permissão de ${permissao.nome}: NÃO`);
        }
    }
}

// Verificar antes de operação
async function lerArquivoSeguro(caminho) {
    try {
        await fs.access(caminho, fsConstants.R_OK);
        const conteudo = await fs.readFile(caminho, 'utf8');
        return conteudo;
    } catch (err) {
        if (err.code === 'ENOENT') {
            throw new Error(`Arquivo ${caminho} não existe`);
        } else if (err.code === 'EACCES') {
            throw new Error(`Sem permissão para ler ${caminho}`);
        }
        throw err;
    }
}
```

---

## 7. Trabalhando com Streams {#streams}

### ReadStream {#readstream}

```javascript
const { createReadStream } = require('fs');
const { Transform } = require('stream');

// Leitura eficiente de arquivos grandes
function processarArquivoGrande(caminho) {
    const stream = createReadStream(caminho, {
        encoding: 'utf8',
        highWaterMark: 64 * 1024, // 64KB chunks
        autoClose: true
    });
    
    let totalBytes = 0;
    let chunks = 0;
    
    stream.on('data', (chunk) => {
        totalBytes += chunk.length;
        chunks++;
        console.log(`Chunk ${chunks}: ${chunk.length} bytes`);
    });
    
    stream.on('end', () => {
        console.log(`Total: ${totalBytes} bytes em ${chunks} chunks`);
    });
    
    stream.on('error', (err) => {
        console.error('Erro:', err);
    });
    
    return stream;
}

// Transformar dados durante leitura
function transformarArquivo(origem, destino) {
    const readStream = createReadStream(origem, { encoding: 'utf8' });
    const writeStream = createWriteStream(destino);
    
    const transformStream = new Transform({
        transform(chunk, encoding, callback) {
            // Converter para maiúsculas
            const transformed = chunk.toString().toUpperCase();
            callback(null, transformed);
        }
    });
    
    readStream
        .pipe(transformStream)
        .pipe(writeStream)
        .on('finish', () => {
            console.log('Transformação concluída');
        });
}
```

### WriteStream {#writestream}

```javascript
const { createWriteStream } = require('fs');

// Escrita otimizada com buffer
async function escreverGrandeVolume(caminho, linhas = 1000000) {
    const stream = createWriteStream(caminho, {
        flags: 'w',
        encoding: 'utf8',
        highWaterMark: 64 * 1024
    });
    
    for (let i = 0; i < linhas; i++) {
        const escreveu = stream.write(`Linha ${i}: Dados de exemplo\n`);
        
        // Se buffer estiver cheio, aguardar
        if (!escreveu) {
            await new Promise(resolve => stream.once('drain', resolve));
        }
    }
    
    stream.end();
    console.log(`${linhas} linhas escritas`);
}

// Escrever com progresso
function escreverComProgresso(caminho, dados) {
    const stream = createWriteStream(caminho);
    let escritos = 0;
    const total = dados.length;
    
    function escrever() {
        while (escritos < total && stream.write(dados[escritos] + '\n')) {
            escritos++;
            console.log(`Progresso: ${Math.round(escritos/total*100)}%`);
        }
        
        if (escritos < total) {
            stream.once('drain', escrever);
        } else {
            stream.end();
            console.log('Escrita concluída!');
        }
    }
    
    escrever();
}
```

### Pipeline {#pipeline}

```javascript
const { pipeline } = require('stream/promises');
const { createReadStream, createWriteStream } = require('fs');
const { createGzip } = require('zlib');

// Pipeline com tratamento de erros automático
async function copiarSeguro(origem, destino) {
    try {
        await pipeline(
            createReadStream(origem),
            createWriteStream(destino)
        );
        console.log('Cópia concluída com sucesso');
    } catch (err) {
        console.error('Erro na cópia:', err);
    }
}

// Compressão de arquivo com pipeline
async function comprimirArquivo(origem, destino) {
    try {
        await pipeline(
            createReadStream(origem),
            createGzip(),
            createWriteStream(destino)
        );
        console.log('Arquivo comprimido com sucesso');
    } catch (err) {
        console.error('Erro na compressão:', err);
    }
}

// Múltiplas transformações
async function processarArquivo(origem, destino) {
    const { Transform } = require('stream');
    
    const transform1 = new Transform({
        transform(chunk, encoding, callback) {
            callback(null, chunk.toString().toUpperCase());
        }
    });
    
    const transform2 = new Transform({
        transform(chunk, encoding, callback) {
            const lines = chunk.toString().split('\n');
            const numbered = lines.map((line, i) => `${i+1}: ${line}`).join('\n');
            callback(null, numbered);
        }
    });
    
    await pipeline(
        createReadStream(origem, { encoding: 'utf8' }),
        transform1,
        transform2,
        createWriteStream(destino)
    );
    
    console.log('Processamento concluído');
}
```

---

## 8. File Descriptors {#file-descriptors}

File descriptors são referências numéricas para arquivos abertos, permitindo operações de baixo nível.

```javascript
const fs = require('fs').promises;
const fsConstants = require('fs').constants;

// Abrir e manipular arquivo com file descriptor
async function manipularComFD() {
    // Abrir arquivo
    const fd = await fs.open('exemplo.txt', 'r+');
    
    try {
        // Obter informações
        const stats = await fd.stat();
        console.log('Tamanho:', stats.size);
        
        // Ler parte do arquivo
        const buffer = Buffer.alloc(100);
        const { bytesRead } = await fd.read(buffer, 0, 100, 0);
        console.log('Lidos:', bytesRead, 'bytes');
        
        // Escrever em posição específica
        const writeBuffer = Buffer.from('Novo conteúdo');
        await fd.write(writeBuffer, 0, writeBuffer.length, 0);
        
        // Sincronizar ao disco
        await fd.sync();
        
    } finally {
        // Sempre fechar o descritor
        await fd.close();
    }
}

// Operações síncronas com file descriptors
const fsSync = require('fs');

function manipularComFDSync() {
    const fd = fsSync.openSync('exemplo.txt', 'r+');
    
    try {
        const buffer = Buffer.alloc(100);
        const bytesRead = fsSync.readSync(fd, buffer, 0, 100, 0);
        console.log('Lidos:', bytesRead);
        
        fsSync.writeSync(fd, Buffer.from('Novo texto'), 0, 10, 0);
        fsSync.fsyncSync(fd);
        
    } finally {
        fsSync.closeSync(fd);
    }
}

// Flags comuns para file descriptors
const flags = {
    'r': 'Abrir para leitura',
    'r+': 'Abrir para leitura e escrita',
    'w': 'Abrir para escrita (cria ou trunca)',
    'wx': 'Abrir para escrita (falha se existir)',
    'w+': 'Abrir para leitura e escrita (cria ou trunca)',
    'a': 'Abrir para anexar (cria se não existir)',
    'a+': 'Abrir para leitura e anexar (cria se não existir)'
};
```

---

## 9. Watch e Observação de Arquivos {#watch}

```javascript
const fs = require('fs');

// fs.watch - Observar mudanças (mais eficiente, mas menos detalhado)
function observarDiretorio(caminho) {
    console.log(`Observando: ${caminho}`);
    
    const watcher = fs.watch(caminho, { recursive: true }, (eventType, filename) => {
        if (filename) {
            console.log(`Evento: ${eventType} - Arquivo: ${filename}`);
            
            if (eventType === 'change') {
                console.log(`Arquivo ${filename} foi modificado`);
            } else if (eventType === 'rename') {
                console.log(`Arquivo ${filename} foi renomeado/removido/criado`);
            }
        }
    });
    
    watcher.on('error', (error) => {
        console.error('Erro no watcher:', error);
    });
    
    return watcher;
}

// fs.watchFile - Observar arquivo específico (mais detalhado)
function observarArquivo(caminho) {
    console.log(`Observando arquivo: ${caminho}`);
    
    fs.watchFile(caminho, { interval: 1000 }, (curr, prev) => {
        if (curr.mtime > prev.mtime) {
            console.log('Arquivo foi modificado');
            console.log('Tamanho atual:', curr.size);
            console.log('Tamanho anterior:', prev.size);
            console.log('Modificado em:', curr.mtime);
        }
        
        if (curr.ctime > prev.ctime) {
            console.log('Metadados foram alterados');
        }
    });
}

// Sistema de build automático
class AutoBuilder {
    constructor(diretorio, arquivoSaida) {
        this.diretorio = diretorio;
        this.arquivoSaida = arquivoSaida;
        this.watcher = null;
    }
    
    start() {
        console.log('Iniciando build automático...');
        this.watcher = fs.watch(this.diretorio, { recursive: true }, async () => {
            await this.build();
        });
        
        this.build(); // Build inicial
    }
    
    async build() {
        console.log('Building...');
        try {
            // Lógica de build aqui
            await this.processarArquivos();
            console.log('Build concluído com sucesso!');
        } catch (err) {
            console.error('Erro no build:', err);
        }
    }
    
    async processarArquivos() {
        const arquivos = await fs.promises.readdir(this.diretorio);
        let conteudo = '';
        
        for (const arquivo of arquivos) {
            if (arquivo.endsWith('.txt')) {
                const data = await fs.promises.readFile(`${this.diretorio}/${arquivo}`, 'utf8');
                conteudo += `// ${arquivo}\n${data}\n\n`;
            }
        }
        
        await fs.promises.writeFile(this.arquivoSaida, conteudo);
    }
    
    stop() {
        if (this.watcher) {
            this.watcher.close();
            console.log('Observação interrompida');
        }
    }
}

// Uso
const builder = new AutoBuilder('./src', './dist/bundle.txt');
builder.start();

// Parar após 1 minuto
setTimeout(() => builder.stop(), 60000);
```

---

## 10. Links Simbólicos {#links}

```javascript
const fs = require('fs').promises;

// Criar link simbólico
async function criarLinkSimbolico() {
    // Criar link para arquivo
    await fs.symlink('arquivo_original.txt', 'link_arquivo.txt');
    
    // Criar link para diretório (requer flag no Windows)
    await fs.symlink('pasta_original', 'link_pasta', 'dir');
    
    console.log('Links simbólicos criados');
}

// Criar link físico (hard link)
async function criarHardLink() {
    await fs.link('arquivo_original.txt', 'hard_link.txt');
    console.log('Hard link criado');
}

// Ler link simbólico
async function lerLinkSimbolico(caminho) {
    try {
        const alvo = await fs.readlink(caminho);
        console.log(`Link ${caminho} aponta para: ${alvo}`);
        return alvo;
    } catch (err) {
        if (err.code === 'EINVAL') {
            console.log('Não é um link simbólico');
        } else {
            throw err;
        }
    }
}

// Sistema de versionamento com links
class VersionManager {
    constructor(baseDir, currentLink) {
        this.baseDir = baseDir;
        this.currentLink = currentLink;
        this.versions = [];
    }
    
    async createVersion(versionName, content) {
        const versionPath = `${this.baseDir}/v${versionName}`;
        
        // Criar arquivo da versão
        await fs.writeFile(versionPath, content);
        this.versions.push(versionPath);
        
        // Atualizar link "current"
        try {
            await fs.unlink(this.currentLink);
        } catch {}
        
        await fs.symlink(versionPath, this.currentLink);
        
        console.log(`Versão ${versionName} criada`);
    }
    
    async switchVersion(versionName) {
        const versionPath = `${this.baseDir}/v${versionName}`;
        
        try {
            await fs.access(versionPath);
            await fs.unlink(this.currentLink);
            await fs.symlink(versionPath, this.currentLink);
            console.log(`Mudou para versão ${versionName}`);
        } catch {
            console.log(`Versão ${versionName} não encontrada`);
        }
    }
    
    async listVersions() {
        const files = await fs.readdir(this.baseDir);
        const versions = files.filter(f => f.startsWith('v'));
        
        for (const version of versions) {
            const isCurrent = await this.isCurrentVersion(version);
            console.log(`${version} ${isCurrent ? '(current)' : ''}`);
        }
    }
    
    async isCurrentVersion(version) {
        try {
            const target = await fs.readlink(this.currentLink);
            return target.includes(version);
        } catch {
            return false;
        }
    }
}

// Uso
const vm = new VersionManager('./versions', './current_version.txt');
await vm.createVersion('1.0.0', 'Conteúdo da versão 1');
await vm.createVersion('1.1.0', 'Conteúdo atualizado');
await vm.listVersions();
await vm.switchVersion('1.0.0');
```

---

## 11. Melhores Práticas {#melhores-praticas}

### 1. Sempre use Promises ou Async/Await
```javascript
// ❌ Evite callbacks aninhados (callback hell)
fs.readFile('a.txt', (err, data) => {
    fs.writeFile('b.txt', data, (err) => {
        fs.readFile('c.txt', (err, data) => {
            // ...
        });
    });
});

// ✅ Use async/await
const fs = require('fs').promises;

async function processFiles() {
    const data = await fs.readFile('a.txt', 'utf8');
    await fs.writeFile('b.txt', data);
    const result = await fs.readFile('c.txt', 'utf8');
}
```

### 2. Trate erros adequadamente
```javascript
// Sempre use try-catch com async/await
async function safeOperation() {
    try {
        await fs.readFile('arquivo.txt');
    } catch (err) {
        if (err.code === 'ENOENT') {
            console.log('Arquivo não encontrado');
        } else if (err.code === 'EACCES') {
            console.log('Permissão negada');
        } else {
            console.error('Erro inesperado:', err);
        }
    }
}
```

### 3. Use streams para arquivos grandes
```javascript
// ❌ Para arquivos grandes
const data = await fs.readFile('gigante.mp4');
processar(data);

// ✅ Use streams
const stream = createReadStream('gigante.mp4');
stream.on('data', (chunk) => processarChunk(chunk));
```

### 4. Feche recursos explicitamente
```javascript
// Sempre feche file descriptors
let fd;
try {
    fd = await fs.open('arquivo.txt', 'r');
    // usar fd
} finally {
    if (fd) await fd.close();
}
```

### 5. Valide caminhos e permissões
```javascript
const path = require('path');

function validatePath(caminho) {
    // Evitar path traversal
    const resolved = path.resolve(caminho);
    if (!resolved.startsWith('/app/data')) {
        throw new Error('Acesso negado');
    }
    return resolved;
}
```

### 6. Use constantes para flags e modos
```javascript
const fs = require('fs').constants;

// Melhor que usar números mágicos
await fs.access('arquivo.txt', fs.constants.R_OK | fs.constants.W_OK);
```

---

## 12. Exemplos Práticos {#exemplos-praticos}

### Exemplo 1: Utilitário de Backup
```javascript
const fs = require('fs').promises;
const path = require('path');
const { createReadStream, createWriteStream } = require('fs');
const { pipeline } = require('stream/promises');
const archiver = require('archiver'); // npm install archiver

class BackupSystem {
    constructor(source, destination) {
        this.source = path.resolve(source);
        this.destination = path.resolve(destination);
    }
    
    async execute() {
        const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
        const backupName = `backup_${timestamp}`;
        const backupPath = path.join(this.destination, backupName);
        
        console.log(`Iniciando backup: ${backupName}`);
        
        try {
            await this.criarBackup(backupPath);
            await this.comprimirBackup(backupPath);
            await this.limparBackupsAntigos();
            
            console.log('Backup concluído com sucesso!');
            return backupPath;
        } catch (error) {
            console.error('Erro no backup:', error);
            throw error;
        }
    }
    
    async criarBackup(backupPath) {
        await fs.mkdir(backupPath, { recursive: true });
        await this.copiarDiretorio(this.source, backupPath);
    }
    
    async copiarDiretorio(origem, destino) {
        await fs.mkdir(destino, { recursive: true });
        const itens = await fs.readdir(origem);
        
        for (const item of itens) {
            const origemPath = path.join(origem, item);
            const destinoPath = path.join(destino, item);
            const stats = await fs.stat(origemPath);
            
            if (stats.isDirectory()) {
                await this.copiarDiretorio(origemPath, destinoPath);
            } else {
                await pipeline(
                    createReadStream(origemPath),
                    createWriteStream(destinoPath)
                );
            }
        }
    }
    
    async comprimirBackup(backupPath) {
        // Implementação com archiver
        console.log('Compressão concluída');
    }
    
    async limparBackupsAntigos() {
        const backups = await fs.readdir(this.destination);
        const antigos = backups
            .filter(b => b.startsWith('backup_'))
            .sort()
            .slice(0, -5); // Manter últimos 5
        
        for (const backup of antigos) {
            await fs.rm(path.join(this.destination, backup), { recursive: true });
            console.log(`Removido backup antigo: ${backup}`);
        }
    }
}
```

### Exemplo 2: Processador de Logs
```javascript
const fs = require('fs').promises;
const { createReadStream, createWriteStream } = require('fs');
const readline = require('readline');

class LogProcessor {
    constructor(logPath) {
        this.logPath = logPath;
        this.stats = {
            totalLines: 0,
            errors: 0,
            warnings: 0,
            info: 0
        };
    }
    
    async process() {
        const fileStream = createReadStream(this.logPath);
        const rl = readline.createInterface({
            input: fileStream,
            crlfDelay: Infinity
        });
        
        for await (const line of rl) {
            this.processLine(line);
        }
        
        await this.generateReport();
        return this.stats;
    }
    
    processLine(line) {
        this.stats.totalLines++;
        
        if (line.includes('[ERROR]')) this.stats.errors++;
        else if (line.includes('[WARNING]')) this.stats.warnings++;
        else if (line.includes('[INFO]')) this.stats.info++;
    }
    
    async generateReport() {
        const report = `
=== Relatório de Logs ===
Total de linhas: ${this.stats.totalLines}
Erros: ${this.stats.errors}
Avisos: ${this.stats.warnings}
Info: ${this.stats.info}
=========================
        `.trim();
        
        await fs.writeFile('relatorio_logs.txt', report);
        console.log('Relatório gerado');
    }
}

// Uso
const processor = new LogProcessor('app.log');
await processor.process();
```

### Exemplo 3: Gerenciador de Configurações
```javascript
const fs = require('fs').promises;
const path = require('path');

class ConfigManager {
    constructor(configDir = './config') {
        this.configDir = configDir;
        this.cache = new Map();
    }
    
    async loadConfig(name) {
        if (this.cache.has(name)) {
            return this.cache.get(name);
        }
        
        const configPath = path.join(this.configDir, `${name}.json`);
        
        try {
            const data = await fs.readFile(configPath, 'utf8');
            const config = JSON.parse(data);
            this.cache.set(name, config);
            return config;
        } catch (err) {
            if (err.code === 'ENOENT') {
                return this.createDefaultConfig(name);
            }
            throw err;
        }
    }
    
    async createDefaultConfig(name) {
        const defaultConfig = {
            name,
            version: '1.0.0',
            settings: {},
            createdAt: new Date().toISOString()
        };
        
        await this.saveConfig(name, defaultConfig);
        return defaultConfig;
    }
    
    async saveConfig(name, config) {
        const configPath = path.join(this.configDir, `${name}.json`);
        const data = JSON.stringify(config, null, 2);
        
        await fs.mkdir(this.configDir, { recursive: true });
        await fs.writeFile(configPath, data, 'utf8');
        
        this.cache.set(name, config);
        console.log(`Configuração ${name} salva`);
    }
    
    async updateConfig(name, updates) {
        const config = await this.loadConfig(name);
        const updated = { ...config, ...updates, updatedAt: new Date().toISOString() };
        await this.saveConfig(name, updated);
        return updated;
    }
    
    async listConfigs() {
        try {
            const files = await fs.readdir(this.configDir);
            return files
                .filter(f => f.endsWith('.json'))
                .map(f => f.replace('.json', ''));
        } catch {
            return [];
        }
    }
    
    async deleteConfig(name) {
        const configPath = path.join(this.configDir, `${name}.json`);
        await fs.unlink(configPath);
        this.cache.delete(name);
        console.log(`Configuração ${name} removida`);
    }
    
    watch(callback) {
        const fsSync = require('fs');
        fsSync.watch(this.configDir, (eventType, filename) => {
            if (filename && filename.endsWith('.json')) {
                const name = filename.replace('.json', '');
                this.cache.delete(name);
                callback(eventType, name);
            }
        });
    }
}

// Uso
const config = new ConfigManager();
await config.loadConfig('database');
await config.updateConfig('database', { host: 'localhost', port: 5432 });
config.watch((event, name) => console.log(`Config ${name} foi ${event}`));
```

### Exemplo 4: Upload de Arquivos com Validação
```javascript
const fs = require('fs').promises;
const path = require('path');
const { createWriteStream } = require('fs');
const { pipeline } = require('stream/promises');
const crypto = require('crypto');

class FileUploadHandler {
    constructor(uploadDir = './uploads') {
        this.uploadDir = uploadDir;
        this.allowedTypes = ['image/jpeg', 'image/png', 'application/pdf'];
        this.maxSize = 10 * 1024 * 1024; // 10MB
    }
    
    async upload(file, metadata = {}) {
        // Validações
        await this.validate(file);
        
        // Gerar nome único
        const hash = crypto.randomBytes(16).toString('hex');
        const extension = path.extname(file.originalname);
        const filename = `${hash}${extension}`;
        const filepath = path.join(this.uploadDir, filename);
        
        // Criar diretório se não existir
        await fs.mkdir(this.uploadDir, { recursive: true });
        
        // Salvar arquivo
        await pipeline(
            file.stream,
            createWriteStream(filepath)
        );
        
        // Salvar metadados
        const fileInfo = {
            id: hash,
            filename: file.originalname,
            storedName: filename,
            size: file.size,
            type: file.mimetype,
            uploadedAt: new Date().toISOString(),
            metadata
        };
        
        await fs.writeFile(
            path.join(this.uploadDir, `${hash}.meta.json`),
            JSON.stringify(fileInfo, null, 2)
        );
        
        console.log(`Arquivo ${file.originalname} enviado como ${filename}`);
        return fileInfo;
    }
    
    async validate(file) {
        // Validar tamanho
        if (file.size > this.maxSize) {
            throw new Error(`Arquivo muito grande. Máximo: ${this.maxSize / 1024 / 1024}MB`);
        }
        
        // Validar tipo
        if (!this.allowedTypes.includes(file.mimetype)) {
            throw new Error(`Tipo de arquivo não permitido: ${file.mimetype}`);
        }
        
        // Validar extensão (segurança extra)
        const allowedExtensions = ['.jpg', '.jpeg', '.png', '.pdf'];
        const ext = path.extname(file.originalname).toLowerCase();
        if (!allowedExtensions.includes(ext)) {
            throw new Error(`Extensão não permitida: ${ext}`);
        }
        
        return true;
    }
    
    async get(id) {
        const metaPath = path.join(this.uploadDir, `${id}.meta.json`);
        
        try {
            const meta = await fs.readFile(metaPath, 'utf8');
            return JSON.parse(meta);
        } catch {
            return null;
        }
    }
    
    async delete(id) {
        const fileInfo = await this.get(id);
        if (!fileInfo) return false;
        
        const filePath = path.join(this.uploadDir, fileInfo.storedName);
        const metaPath = path.join(this.uploadDir, `${id}.meta.json`);
        
        await fs.unlink(filePath);
        await fs.unlink(metaPath);
        
        console.log(`Arquivo ${id} removido`);
        return true;
    }
    
    async list() {
        const files = await fs.readdir(this.uploadDir);
        const metaFiles = files.filter(f => f.endsWith('.meta.json'));
        
        const infos = [];
        for (const metaFile of metaFiles) {
            const data = await fs.readFile(path.join(this.uploadDir, metaFile), 'utf8');
            infos.push(JSON.parse(data));
        }
        
        return infos;
    }
}
```

