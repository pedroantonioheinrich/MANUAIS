
# 📁 Manual de Manipulação de Arquivos com JavaScript

## Índice
1. [Manipulação no Navegador (Front-end)](#navegador)
   - [Selecionando Arquivos](#selecionando-arquivos)
   - [Lendo Arquivos](#lendo-arquivos)
   - [Criando e Baixando Arquivos](#criando-e-baixando)
   - [Arrastar e Soltar](#arrastar-e-soltar)
2. [Manipulação com Node.js (Back-end)](#nodejs)
   - [Módulo FS (File System)](#modulo-fs)
   - [Operações Síncronas vs Assíncronas](#sync-vs-async)
   - [Leitura e Escrita](#leitura-escrita)
   - [Manipulação de Diretórios](#diretorios)
   - [Streams para Arquivos Grandes](#streams)
3. [Boas Práticas](#boas-praticas)
4. [Exemplos Práticos](#exemplos)

---

## 1. Manipulação no Navegador (Front-end) {#navegador}

### Selecionando Arquivos {#selecionando-arquivos}

#### Input File Básico
```html
<input type="file" id="fileInput" multiple accept=".txt,.jpg,.pdf">
```

```javascript
// Selecionar arquivos via input
const fileInput = document.getElementById('fileInput');

fileInput.addEventListener('change', (event) => {
    const files = event.target.files; // FileList object
    
    for (let i = 0; i < files.length; i++) {
        const file = files[i];
        console.log(`Nome: ${file.name}`);
        console.log(`Tamanho: ${file.size} bytes`);
        console.log(`Tipo: ${file.type}`);
        console.log(`Última modificação: ${new Date(file.lastModified)}`);
    }
});
```

### Lendo Arquivos {#lendo-arquivos}

#### FileReader API
```javascript
function readFileAsText(file) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        
        reader.onload = (event) => {
            resolve(event.target.result);
        };
        
        reader.onerror = (error) => {
            reject(error);
        };
        
        reader.readAsText(file, 'UTF-8');
    });
}

function readFileAsDataURL(file) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        
        reader.onload = (event) => {
            resolve(event.target.result);
        };
        
        reader.onerror = (error) => {
            reject(error);
        };
        
        reader.readAsDataURL(file);
    });
}

function readFileAsArrayBuffer(file) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        
        reader.onload = (event) => {
            resolve(event.target.result);
        };
        
        reader.onerror = (error) => {
            reject(error);
        };
        
        reader.readAsArrayBuffer(file);
    });
}

// Exemplo de uso
async function handleFileUpload(file) {
    try {
        const content = await readFileAsText(file);
        console.log('Conteúdo do arquivo:', content);
        
        // Para imagens
        if (file.type.startsWith('image/')) {
            const dataUrl = await readFileAsDataURL(file);
            document.getElementById('preview').src = dataUrl;
        }
    } catch (error) {
        console.error('Erro ao ler arquivo:', error);
    }
}
```

### Criando e Baixando Arquivos {#criando-e-baixando}

#### Download de Arquivos
```javascript
function downloadFile(content, filename, contentType = 'text/plain') {
    const blob = new Blob([content], { type: contentType });
    const url = URL.createObjectURL(blob);
    
    const link = document.createElement('a');
    link.href = url;
    link.download = filename;
    document.body.appendChild(link);
    link.click();
    
    // Limpeza
    document.body.removeChild(link);
    URL.revokeObjectURL(url);
}

// Exemplos
downloadFile('Hello World!', 'hello.txt');
downloadFile(JSON.stringify({ name: 'João', age: 30 }), 'dados.json', 'application/json');

// Download de conteúdo de canvas como imagem
function downloadCanvas(canvas, filename = 'imagem.png') {
    const dataURL = canvas.toDataURL('image/png');
    const link = document.createElement('a');
    link.href = dataURL;
    link.download = filename;
    link.click();
}
```

#### Criando Arquivo Excel (CSV)
```javascript
function exportToCSV(data, filename = 'dados.csv') {
    const headers = Object.keys(data[0]);
    const csvRows = [
        headers.join(','),
        ...data.map(row => headers.map(header => JSON.stringify(row[header] || '')).join(','))
    ];
    
    const csvContent = csvRows.join('\n');
    downloadFile(csvContent, filename, 'text/csv;charset=utf-8;');
}

// Uso
const users = [
    { name: 'João', email: 'joao@email.com', age: 25 },
    { name: 'Maria', email: 'maria@email.com', age: 30 }
];
exportToCSV(users, 'usuarios.csv');
```

### Arrastar e Soltar {#arrastar-e-soltar}

```javascript
const dropZone = document.getElementById('dropZone');

['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
    dropZone.addEventListener(eventName, preventDefaults, false);
});

function preventDefaults(e) {
    e.preventDefault();
    e.stopPropagation();
}

['dragenter', 'dragover'].forEach(eventName => {
    dropZone.addEventListener(eventName, highlight, false);
});

['dragleave', 'drop'].forEach(eventName => {
    dropZone.addEventListener(eventName, unhighlight, false);
});

function highlight(e) {
    dropZone.classList.add('highlight');
}

function unhighlight(e) {
    dropZone.classList.remove('highlight');
}

dropZone.addEventListener('drop', handleDrop, false);

function handleDrop(e) {
    const dt = e.dataTransfer;
    const files = dt.files;
    
    handleFiles(files);
}

function handleFiles(files) {
    for (const file of files) {
        console.log(`Arquivo solto: ${file.name}`);
        // Processar cada arquivo
        readFileAsText(file).then(content => {
            console.log(`Conteúdo de ${file.name}:`, content);
        });
    }
}
```

---

## 2. Manipulação com Node.js (Back-end) {#nodejs}

### Módulo FS (File System) {#modulo-fs}

```javascript
const fs = require('fs');
const fsPromises = require('fs').promises;
const path = require('path');
```

### Operações Síncronas vs Assíncronas {#sync-vs-async}

```javascript
// Síncrono (bloqueante)
function syncOperations() {
    try {
        const data = fs.readFileSync('arquivo.txt', 'utf8');
        console.log('Conteúdo:', data);
        
        fs.writeFileSync('novo.txt', 'Conteúdo novo');
        console.log('Arquivo escrito com sucesso');
    } catch (error) {
        console.error('Erro:', error);
    }
}

// Assíncrono com callbacks
function asyncCallback() {
    fs.readFile('arquivo.txt', 'utf8', (err, data) => {
        if (err) {
            console.error('Erro:', err);
            return;
        }
        console.log('Conteúdo:', data);
        
        fs.writeFile('novo.txt', 'Conteúdo novo', (err) => {
            if (err) {
                console.error('Erro:', err);
                return;
            }
            console.log('Arquivo escrito com sucesso');
        });
    });
}

// Assíncrono com Promises (recomendado)
async function asyncPromises() {
    try {
        const data = await fsPromises.readFile('arquivo.txt', 'utf8');
        console.log('Conteúdo:', data);
        
        await fsPromises.writeFile('novo.txt', 'Conteúdo novo');
        console.log('Arquivo escrito com sucesso');
    } catch (error) {
        console.error('Erro:', error);
    }
}
```

### Leitura e Escrita {#leitura-escrita}

#### Operações Básicas
```javascript
// Escrever arquivo
async function writeFileExample() {
    const content = 'Este é o conteúdo do arquivo\nLinha 2\nLinha 3';
    
    // WriteFile (substitui o conteúdo)
    await fsPromises.writeFile('exemplo.txt', content, 'utf8');
    
    // AppendFile (adiciona ao final)
    await fsPromises.appendFile('exemplo.txt', '\nNova linha adicionada', 'utf8');
    
    console.log('Arquivo escrito com sucesso');
}

// Ler arquivo
async function readFileExample() {
    // Leitura completa
    const content = await fsPromises.readFile('exemplo.txt', 'utf8');
    console.log('Conteúdo completo:', content);
    
    // Leitura com stat para informações
    const stats = await fsPromises.stat('exemplo.txt');
    console.log('Tamanho:', stats.size, 'bytes');
    console.log('Última modificação:', stats.mtime);
    console.log('É arquivo:', stats.isFile());
    console.log('É diretório:', stats.isDirectory());
}

// Copiar arquivo
async function copyFileExample() {
    await fsPromises.copyFile('exemplo.txt', 'backup.txt');
    console.log('Arquivo copiado');
}

// Renomear/Mover arquivo
async function renameFileExample() {
    await fsPromises.rename('exemplo.txt', 'renomeado.txt');
    console.log('Arquivo renomeado');
}
```

### Manipulação de Diretórios {#diretorios}

```javascript
// Criar diretório
async function createDirectory() {
    await fsPromises.mkdir('meu_diretorio', { recursive: true });
    console.log('Diretório criado');
}

// Listar diretório
async function listDirectory(dirPath = '.') {
    try {
        const files = await fsPromises.readdir(dirPath);
        
        for (const file of files) {
            const filePath = path.join(dirPath, file);
            const stats = await fsPromises.stat(filePath);
            
            console.log(`${stats.isDirectory() ? '📁' : '📄'} ${file} - ${stats.size} bytes`);
        }
    } catch (error) {
        console.error('Erro ao listar diretório:', error);
    }
}

// Listar recursivamente
async function listDirectoryRecursive(dirPath = '.', indent = '') {
    const files = await fsPromises.readdir(dirPath);
    
    for (const file of files) {
        const filePath = path.join(dirPath, file);
        const stats = await fsPromises.stat(filePath);
        
        if (stats.isDirectory()) {
            console.log(`${indent}📁 ${file}/`);
            await listDirectoryRecursive(filePath, indent + '  ');
        } else {
            console.log(`${indent}📄 ${file} (${stats.size} bytes)`);
        }
    }
}

// Remover arquivo/diretório
async function removeFileAndDirectory() {
    // Remover arquivo
    await fsPromises.unlink('renomeado.txt');
    
    // Remover diretório vazio
    await fsPromises.rmdir('meu_diretorio');
    
    // Remover diretório com conteúdo (recursivo)
    await fsPromises.rm('pasta_com_conteudo', { recursive: true, force: true });
}
```

### Streams para Arquivos Grandes {#streams}

```javascript
const { createReadStream, createWriteStream } = require('fs');
const { pipeline } = require('stream/promises');

// Leitura com stream (para arquivos grandes)
function readLargeFile() {
    const readStream = createReadStream('arquivo_grande.txt', { 
        encoding: 'utf8',
        highWaterMark: 64 * 1024 // 64KB chunks
    });
    
    readStream.on('data', (chunk) => {
        console.log(`Recebido chunk de ${chunk.length} caracteres`);
        // Processar chunk
    });
    
    readStream.on('end', () => {
        console.log('Leitura concluída');
    });
    
    readStream.on('error', (error) => {
        console.error('Erro:', error);
    });
}

// Copiar arquivo grande com stream
async function copyLargeFile(source, destination) {
    const readStream = createReadStream(source);
    const writeStream = createWriteStream(destination);
    
    await pipeline(readStream, writeStream);
    console.log('Cópia concluída com sucesso');
}

// Transformar conteúdo com stream
const { Transform } = require('stream');

function createUpperCaseTransform() {
    return new Transform({
        transform(chunk, encoding, callback) {
            const upperChunk = chunk.toString().toUpperCase();
            callback(null, upperChunk);
        }
    });
}

async function transformFile() {
    const readStream = createReadStream('entrada.txt');
    const upperCaseTransform = createUpperCaseTransform();
    const writeStream = createWriteStream('saida.txt');
    
    await pipeline(readStream, upperCaseTransform, writeStream);
    console.log('Transformação concluída');
}
```

---

## 3. Boas Práticas {#boas-praticas}

### Validação de Arquivos
```javascript
function validateFile(file, options = {}) {
    const {
        maxSize = 5 * 1024 * 1024, // 5MB padrão
        allowedTypes = ['image/jpeg', 'image/png', 'application/pdf'],
        allowedExtensions = ['.jpg', '.jpeg', '.png', '.pdf']
    } = options;
    
    // Validação de tamanho
    if (file.size > maxSize) {
        throw new Error(`Arquivo muito grande. Máximo: ${maxSize / 1024 / 1024}MB`);
    }
    
    // Validação de tipo MIME
    if (allowedTypes.length && !allowedTypes.includes(file.type)) {
        throw new Error(`Tipo de arquivo não permitido: ${file.type}`);
    }
    
    // Validação de extensão
    const extension = path.extname(file.name).toLowerCase();
    if (allowedExtensions.length && !allowedExtensions.includes(extension)) {
        throw new Error(`Extensão não permitida: ${extension}`);
    }
    
    return true;
}
```

### Tratamento de Erros
```javascript
async function safeFileOperation(operation) {
    try {
        return await operation();
    } catch (error) {
        if (error.code === 'ENOENT') {
            console.error('Arquivo não encontrado');
        } else if (error.code === 'EACCES') {
            console.error('Permissão negada');
        } else if (error.code === 'ENOSPC') {
            console.error('Espaço em disco insuficiente');
        } else {
            console.error('Erro na operação:', error.message);
        }
        throw error;
    }
}
```

### Limpeza de Recursos
```javascript
// Sempre liberar URLs de objetos
function createTempFile(content) {
    const blob = new Blob([content]);
    const url = URL.createObjectURL(blob);
    
    try {
        // Usar o URL...
        return url;
    } finally {
        // Liberar quando não for mais necessário
        URL.revokeObjectURL(url);
    }
}
```

---

## 4. Exemplos Práticos {#exemplos}

### Upload de Múltiplos Arquivos com Progresso
```html
<input type="file" id="multiUpload" multiple>
<div id="progress"></div>
```

```javascript
document.getElementById('multiUpload').addEventListener('change', async (e) => {
    const files = Array.from(e.target.files);
    const progressDiv = document.getElementById('progress');
    
    for (const file of files) {
        const progressItem = document.createElement('div');
        progressItem.textContent = `Enviando ${file.name}...`;
        progressDiv.appendChild(progressItem);
        
        try {
            await uploadFile(file, (progress) => {
                progressItem.textContent = `${file.name}: ${progress}%`;
            });
            progressItem.textContent = `✓ ${file.name} - Concluído!`;
        } catch (error) {
            progressItem.textContent = `✗ ${file.name} - Erro: ${error.message}`;
        }
    }
});

async function uploadFile(file, onProgress) {
    const formData = new FormData();
    formData.append('file', file);
    
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        
        xhr.upload.addEventListener('progress', (e) => {
            if (e.lengthComputable) {
                const percent = (e.loaded / e.total) * 100;
                onProgress(Math.round(percent));
            }
        });
        
        xhr.addEventListener('load', () => {
            if (xhr.status === 200) {
                resolve(xhr.response);
            } else {
                reject(new Error(`Upload falhou: ${xhr.status}`));
            }
        });
        
        xhr.addEventListener('error', () => reject(new Error('Erro de rede')));
        
        xhr.open('POST', '/upload');
        xhr.send(formData);
    });
}
```

### Backup Automático com Node.js
```javascript
const fs = require('fs').promises;
const path = require('path');
const { createReadStream, createWriteStream } = require('fs');
const { pipeline } = require('stream/promises');

class FileBackup {
    constructor(sourceDir, backupDir) {
        this.sourceDir = sourceDir;
        this.backupDir = backupDir;
    }
    
    async createBackup() {
        const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
        const backupFolder = path.join(this.backupDir, `backup_${timestamp}`);
        
        await fs.mkdir(backupFolder, { recursive: true });
        await this.copyDirectory(this.sourceDir, backupFolder);
        
        console.log(`Backup criado em: ${backupFolder}`);
        return backupFolder;
    }
    
    async copyDirectory(source, destination) {
        await fs.mkdir(destination, { recursive: true });
        
        const items = await fs.readdir(source);
        
        for (const item of items) {
            const sourcePath = path.join(source, item);
            const destPath = path.join(destination, item);
            const stats = await fs.stat(sourcePath);
            
            if (stats.isDirectory()) {
                await this.copyDirectory(sourcePath, destPath);
            } else {
                await pipeline(
                    createReadStream(sourcePath),
                    createWriteStream(destPath)
                );
            }
        }
    }
    
    async listBackups() {
        const backups = await fs.readdir(this.backupDir);
        const backupInfo = [];
        
        for (const backup of backups) {
            const backupPath = path.join(this.backupDir, backup);
            const stats = await fs.stat(backupPath);
            
            backupInfo.push({
                name: backup,
                created: stats.birthtime,
                size: await this.getDirectorySize(backupPath)
            });
        }
        
        return backupInfo.sort((a, b) => b.created - a.created);
    }
    
    async getDirectorySize(dirPath) {
        let totalSize = 0;
        const items = await fs.readdir(dirPath);
        
        for (const item of items) {
            const itemPath = path.join(dirPath, item);
            const stats = await fs.stat(itemPath);
            
            if (stats.isDirectory()) {
                totalSize += await this.getDirectorySize(itemPath);
            } else {
                totalSize += stats.size;
            }
        }
        
        return totalSize;
    }
}

// Uso
const backup = new FileBackup('./dados', './backups');
await backup.createBackup();
const backups = await backup.listBackups();
console.log('Backups disponíveis:', backups);
```

---

