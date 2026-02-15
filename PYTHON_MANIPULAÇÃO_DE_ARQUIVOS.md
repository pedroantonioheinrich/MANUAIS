# 📁 MANUAL COMPLETO: MANIPULAÇÃO DE ARQUIVOS EM PYTHON

## 📚 SUMÁRIO
1. [Introdução](#introdução)
2. [Conceitos Fundamentais](#conceitos-fundamentais)
3. [Modos de Abertura](#modos-de-abertura)
4. [Operações Básicas](#operações-básicas)
5. [Métodos de Leitura](#métodos-de-leitura)
6. [Métodos de Escrita](#métodos-de-escrita)
7. [Gerenciamento de Contexto](#gerenciamento-de-contexto)
8. [Trabalhando com Diferentes Formatos](#trabalhando-com-diferentes-formatos)
9. [Aplicações Práticas](#aplicações-práticas)
10. [Boas Práticas e Tratamento de Erros](#boas-práticas-e-tratamento-de-erros)

---

## INTRODUÇÃO

### Por que manipular arquivos?
Imagine que você está desenvolvendo um sistema de cadastro de clientes. Os dados precisam ser salvos em algum lugar para não serem perdidos quando o programa for fechado. Os arquivos são como "cadernos digitais" onde podemos guardar informações permanentemente.

**Associação prática:** Um arquivo em Python é como uma gaveta de arquivos físicos:
- Você precisa **abrir** a gaveta para acessar os documentos
- Pode **ler** os documentos existentes
- Pode **escrever** novos documentos
- Deve **fechar** a gaveta quando terminar

---

## CONCEITOS FUNDAMENTAIS

### 1. **Fluxo de Dados (Stream)**
```python
# Quando trabalhamos com arquivos, criamos um "canal" de comunicação
arquivo = open('dados.txt', 'r')  # Abre o canal de leitura
dados = arquivo.read()              # Fluxo de dados do arquivo → programa
arquivo.close()                      # Fecha o canal
```

**Explicação:** O fluxo de dados é como uma mangueira conectando seu programa ao arquivo. Os dados fluem através dessa mangueira em uma direção (leitura ou escrita).

### 2. **Ponteiro do Arquivo**
```python
# O ponteiro é como o cursor do Word - indica onde estamos no arquivo
arquivo = open('exemplo.txt', 'r+')  # Modo leitura e escrita
print(arquivo.tell())  # Mostra posição atual do ponteiro: 0 (início)
conteudo = arquivo.read(10)  # Lê 10 caracteres
print(arquivo.tell())  # Ponteiro agora está na posição 10
arquivo.seek(0)  # Volta o ponteiro para o início
arquivo.close()
```

**Associação:** Imagine ler um livro com o dedo marcando a página atual. O `tell()` mostra onde seu dedo está, e `seek()` move seu dedo para outra posição.

---

## MODOS DE ABERTURA

### Tabela Completa de Modos

| Modo | Descrição | Comportamento | Se arquivo não existir |
|------|-----------|---------------|------------------------|
| 'r'  | Leitura   | Ponteiro no início | Erro |
| 'w'  | Escrita   | Apaga conteúdo existente | Cria novo |
| 'a'  | Adicionar | Ponteiro no final | Cria novo |
| 'r+' | Leitura/Escrita | Ponteiro no início | Erro |
| 'w+' | Escrita/Leitura | Apaga conteúdo | Cria novo |
| 'a+' | Adicionar/Leitura | Ponteiro no final | Cria novo |
| 'rb' | Leitura binária | Lê em bytes | Erro |
| 'wb' | Escrita binária | Escreve em bytes | Cria novo |

### Exemplos Práticos com Cada Modo:

```python
# MODO 'r' - Leitura (mais comum para consultas)
try:
    with open('config.txt', 'r') as arquivo:
        config = arquivo.read()
        print("Configurações carregadas:", config)
except FileNotFoundError:
    print("Arquivo de configuração não encontrado!")

# MODO 'w' - Escrita (útil para criar/sobrescrever logs)
with open('log_sistema.txt', 'w') as arquivo:
    arquivo.write("Iniciando sistema...\n")
    # Cuidado: se o arquivo existia, foi APAGADO!

# MODO 'a' - Append (perfeito para logs contínuos)
with open('registro_erros.log', 'a') as arquivo:
    arquivo.write("Erro: conexão perdida às 10:30\n")
    # Mantém histórico, apenas adiciona no final

# MODO 'r+' - Leitura e Escrita sem apagar
with open('dados.txt', 'r+') as arquivo:
    dados = arquivo.read()
    arquivo.seek(0)  # Volta ao início para escrever
    arquivo.write("NOVO: " + dados)
    # Útil para modificar arquivos existentes

# MODO BINÁRIO - Para imagens, vídeos, executáveis
with open('foto.jpg', 'rb') as arquivo:
    dados_binarios = arquivo.read()
    # Processar imagem...
```

---

## OPERAÇÕES BÁSICAS

### 1. **Abrindo e Fechando Arquivos (O Mais Importante!)**

```python
# FORMA ERRADA (NÃO FAÇA ISSO!)
arquivo = open('dados.txt', 'w')
arquivo.write("Teste")
# Esqueceu de fechar! Dados podem ser perdidos!

# FORMA CORRETA (SEMPRE FECHE!)
arquivo = open('dados.txt', 'w')
try:
    arquivo.write("Teste")
finally:
    arquivo.close()  # Garantia que fechará mesmo com erro

# FORMA PYTHÔNICA (RECOMENDADA)
with open('dados.txt', 'w') as arquivo:
    arquivo.write("Teste")
    # Fecha automaticamente ao sair do bloco
```

**Por que fechar é crucial?**
- Libera recursos do sistema
- Garante que todos os dados foram escritos
- Evita corrupção de arquivos
- Permite que outros programas acessem o arquivo

### 2. **Caminhos de Arquivo**

```python
import os

# Caminhos absolutos vs relativos
with open('relatorio.txt', 'w') as f:  # Caminho relativo (pasta atual)
    f.write("Dados")

with open('C:/Users/Usuario/Documents/relatorio.txt', 'w') as f:  # Absoluto
    f.write("Dados")

# Trabalhando com caminhos de forma inteligente
caminho_base = os.path.dirname(__file__)  # Pasta do script atual
caminho_completo = os.path.join(caminho_base, 'dados', 'clientes.csv')
# Cria caminho correto para qualquer sistema operacional

print(f"Arquivo será salvo em: {caminho_completo}")
```

---

## MÉTODOS DE LEITURA

### 1. **read() - Leitura Completa**
```python
# Ideal para arquivos pequenos
with open('poema.txt', 'r', encoding='utf-8') as arquivo:
    conteudo = arquivo.read()  # Lê TUDO de uma vez
    print(f"Total de caracteres: {len(conteudo)}")
    print("Conteúdo:\n", conteudo)

# Limitando leitura
with open('grande_arquivo.txt', 'r') as arquivo:
    parte = arquivo.read(100)  # Lê apenas 100 primeiros caracteres
    while parte:  # Enquanto houver dados
        print(parte, end='')
        parte = arquivo.read(100)
```

### 2. **readline() - Leitura Linha a Linha**
```python
# Perfeito para processar arquivos linha por linha
with open('log_servidor.txt', 'r') as arquivo:
    linha = arquivo.readline()
    numero_linha = 1
    
    while linha:
        if 'ERRO' in linha:
            print(f"⚠️ Linha {numero_linha}: {linha.strip()}")
        linha = arquivo.readline()
        numero_linha += 1
```

### 3. **readlines() - Lista de Linhas**
```python
# Útil quando precisamos manipular todas as linhas
with open('alunos.txt', 'r', encoding='utf-8') as arquivo:
    linhas = arquivo.readlines()  # Lista com todas as linhas
    
    print(f"Total de alunos: {len(linhas)}")
    
    # Ordenar alunos alfabeticamente
    linhas.sort()
    
    for i, aluno in enumerate(linhas, 1):
        print(f"{i}. {aluno.strip()}")
```

### 4. **Iteração Direta (Mais Eficiente!)**
```python
# Forma mais pythonica e eficiente para arquivos grandes
with open('log_acesso.txt', 'r') as arquivo:
    contador_erros = 0
    
    for linha in arquivo:  # Lê uma linha por vez (não carrega tudo na memória)
        if '404' in linha:
            contador_erros += 1
            print(f"Página não encontrada: {linha.strip()}")
    
    print(f"Total de erros 404: {contador_erros}")
```

---

## MÉTODOS DE ESCRITA

### 1. **write() - Escrevendo Dados**
```python
# Escrevendo texto simples
with open('notas.txt', 'w', encoding='utf-8') as arquivo:
    arquivo.write("Notas da Turma\n")
    arquivo.write("-" * 30 + "\n")
    
    # Escrevendo dados formatados
    alunos = [
        ("João", 8.5),
        ("Maria", 9.0),
        ("Pedro", 7.5)
    ]
    
    for nome, nota in alunos:
        linha = f"{nome}: {nota:.1f}\n"
        arquivo.write(linha)
```

### 2. **writelines() - Escrevendo Múltiplas Linhas**
```python
# Escrevendo lista de linhas
linhas = [
    "Primeira linha\n",
    "Segunda linha\n",
    "Terceira linha\n"
]

with open('multiplas_linhas.txt', 'w') as arquivo:
    arquivo.writelines(linhas)  # Escreve todas de uma vez

# IMPORTANTE: writelines NÃO adiciona quebras de linha automaticamente!
dados = ["linha1", "linha2", "linha3"]
dados_com_quebras = [linha + '\n' for linha in dados]
with open('correto.txt', 'w') as arquivo:
    arquivo.writelines(dados_com_quebras)
```

### 3. **Exemplo Prático: Sistema de Logs**
```python
import datetime

def registrar_evento(mensagem, nivel="INFO"):
    """Registra eventos com timestamp automático"""
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    with open('sistema.log', 'a', encoding='utf-8') as log:
        linha = f"[{timestamp}] {nivel}: {mensagem}\n"
        log.write(linha)
        print(f"Log registrado: {linha.strip()}")

# Usando o sistema
registrar_evento("Sistema iniciado")
registrar_evento("Conexão com banco estabelecida")
registrar_evento("Falha na autenticação", "ERRO")
```

---

## GERENCIAMENTO DE CONTEXTO (WITH)

### Por que o 'with' é tão importante?

```python
# SEM with (propenso a erros)
arquivo = open('dados.txt', 'w')
try:
    arquivo.write("Dados importantes")
    # Se ocorrer um erro aqui, arquivo NÃO será fechado!
    1/0  # Simulando erro
finally:
    arquivo.close()  # Precisa garantir o fechamento

# COM with (automático e seguro)
with open('dados.txt', 'w') as arquivo:
    arquivo.write("Dados importantes")
    1/0  # Mesmo com erro, arquivo é fechado automaticamente!

# Funciona com múltiplos arquivos
with open('origem.txt', 'r') as origem, open('destino.txt', 'w') as destino:
    for linha in origem:
        destino.write(linha.upper())
```

### Como funciona por baixo dos panos:

```python
# O que o 'with' faz magicamente
class GerenciadorArquivo:
    def __init__(self, nome, modo):
        self.arquivo = open(nome, modo)
        
    def __enter__(self):
        return self.arquivo  # Retorna o arquivo para a variável do with
        
    def __exit__(self, tipo_erro, valor_erro, traceback):
        self.arquivo.close()  # Garante o fechamento
        
# Uso equivalente ao 'with'
with GerenciadorArquivo('teste.txt', 'w') as arquivo:
    arquivo.write("Teste")
```

---

## TRABALHANDO COM DIFERENTES FORMATOS

### 1. **Arquivos CSV (Dados Tabelados)**
```python
import csv

# Escrevendo CSV
cabecalhos = ['Nome', 'Idade', 'Cidade']
dados = [
    ['Ana', 25, 'São Paulo'],
    ['Carlos', 32, 'Rio de Janeiro'],
    ['Mariana', 28, 'Belo Horizonte']
]

with open('clientes.csv', 'w', newline='', encoding='utf-8') as arquivo:
    escritor = csv.writer(arquivo)
    escritor.writerow(cabecalhos)  # Escreve cabeçalho
    escritor.writerows(dados)      # Escreve todos os dados

# Lendo CSV
with open('clientes.csv', 'r', encoding='utf-8') as arquivo:
    leitor = csv.reader(arquivo)
    cabecalhos = next(leitor)  # Pula cabeçalho
    
    for linha in leitor:
        nome, idade, cidade = linha
        print(f"{nome} tem {idade} anos e mora em {cidade}")

# Usando DictReader (mais intuitivo)
with open('clientes.csv', 'r', encoding='utf-8') as arquivo:
    leitor = csv.DictReader(arquivo)
    for linha in leitor:
        print(f"{linha['Nome']} - {linha['Cidade']}")
```

### 2. **Arquivos JSON (Dados Estruturados)**
```python
import json

# Dados de exemplo (dicionário Python)
configuracao = {
    "empresa": "Tech Solutions",
    "ano_fundacao": 2015,
    "ativo": True,
    "endereco": {
        "rua": "Av. Paulista",
        "numero": 1000,
        "cidade": "São Paulo"
    },
    "funcionarios": ["Ana", "Carlos", "Mariana"]
}

# Salvando como JSON
with open('config.json', 'w', encoding='utf-8') as arquivo:
    json.dump(configuracao, arquivo, indent=4, ensure_ascii=False)
    print("Configuração salva com sucesso!")

# Lendo JSON
with open('config.json', 'r', encoding='utf-8') as arquivo:
    dados_carregados = json.load(arquivo)
    
    print(f"Empresa: {dados_carregados['empresa']}")
    print(f"Cidade: {dados_carregados['endereco']['cidade']}")
    print(f"Funcionários: {', '.join(dados_carregados['funcionarios'])}")

# JSON em string (para API, por exemplo)
dados_string = json.dumps(configuracao, indent=2)
print("JSON como string:")
print(dados_string)
```

### 3. **Arquivos Binários (Imagens, Áudio, etc.)**
```python
# Copiando uma imagem
with open('foto_original.jpg', 'rb') as origem:
    dados_imagem = origem.read()
    
    with open('foto_copia.jpg', 'wb') as destino:
        destino.write(dados_imagem)
        print(f"Imagem copiada! Tamanho: {len(dados_imagem)} bytes")

# Processando em blocos (para arquivos grandes)
def copiar_arquivo_grande(origem, destino, tamanho_bloco=8192):
    """Copia arquivo em blocos para economizar memória"""
    with open(origem, 'rb') as src, open(destino, 'wb') as dst:
        bytes_copiados = 0
        while True:
            bloco = src.read(tamanho_bloco)
            if not bloco:  # Fim do arquivo
                break
            dst.write(bloco)
            bytes_copiados += len(bloco)
            print(f"Copiados {bytes_copiados} bytes...", end='\r')
    
    print(f"\nArquivo copiado com sucesso! Total: {bytes_copiados} bytes")
```

---

## APLICAÇÕES PRÁTICAS

### 1. **Sistema de Cadastro de Usuários**
```python
import json
import os

class SistemaCadastro:
    def __init__(self, arquivo_db='usuarios.json'):
        self.arquivo_db = arquivo_db
        self.carregar_dados()
    
    def carregar_dados(self):
        """Carrega usuários do arquivo ou cria lista vazia"""
        if os.path.exists(self.arquivo_db):
            with open(self.arquivo_db, 'r', encoding='utf-8') as f:
                self.usuarios = json.load(f)
                print(f"✅ {len(self.usuarios)} usuários carregados")
        else:
            self.usuarios = []
            print("📁 Novo banco de dados criado")
    
    def salvar_dados(self):
        """Salva usuários no arquivo"""
        with open(self.arquivo_db, 'w', encoding='utf-8') as f:
            json.dump(self.usuarios, f, indent=2, ensure_ascii=False)
            print("💾 Dados salvos com sucesso")
    
    def adicionar_usuario(self, nome, email, idade):
        """Adiciona novo usuário"""
        usuario = {
            'id': len(self.usuarios) + 1,
            'nome': nome,
            'email': email,
            'idade': idade
        }
        self.usuarios.append(usuario)
        self.salvar_dados()
        print(f"✅ Usuário {nome} adicionado com ID {usuario['id']}")
    
    def listar_usuarios(self):
        """Lista todos os usuários"""
        if not self.usuarios:
            print("📭 Nenhum usuário cadastrado")
            return
        
        print("\n" + "="*50)
        print("USUÁRIOS CADASTRADOS")
        print("="*50)
        for usuario in self.usuarios:
            print(f"ID: {usuario['id']}")
            print(f"Nome: {usuario['nome']}")
            print(f"Email: {usuario['email']}")
            print(f"Idade: {usuario['idade']}")
            print("-"*30)
    
    def buscar_usuario(self, termo):
        """Busca usuários por nome ou email"""
        resultados = []
        for usuario in self.usuarios:
            if (termo.lower() in usuario['nome'].lower() or 
                termo.lower() in usuario['email'].lower()):
                resultados.append(usuario)
        return resultados

# Usando o sistema
sistema = SistemaCadastro()

# Menu interativo
while True:
    print("\n" + "="*40)
    print("SISTEMA DE CADASTRO")
    print("="*40)
    print("1 - Adicionar usuário")
    print("2 - Listar usuários")
    print("3 - Buscar usuário")
    print("4 - Sair")
    
    opcao = input("Escolha uma opção: ")
    
    if opcao == '1':
        nome = input("Nome: ")
        email = input("Email: ")
        idade = int(input("Idade: "))
        sistema.adicionar_usuario(nome, email, idade)
    
    elif opcao == '2':
        sistema.listar_usuarios()
    
    elif opcao == '3':
        termo = input("Digite nome ou email para buscar: ")
        resultados = sistema.buscar_usuario(termo)
        if resultados:
            print(f"\n🔍 {len(resultados)} resultado(s) encontrado(s):")
            for user in resultados:
                print(f"  • {user['nome']} - {user['email']}")
        else:
            print("❌ Nenhum usuário encontrado")
    
    elif opcao == '4':
        print("👋 Sistema encerrado")
        break
```

### 2. **Sistema de Logs com Rotação**
```python
import os
import datetime
import shutil

class LoggerSistema:
    def __init__(self, nome_base='log_sistema', max_tamanho=1024*1024):  # 1MB
        self.nome_base = nome_base
        self.max_tamanho = max_tamanho
        self.arquivo_atual = f"{nome_base}.log"
    
    def verificar_tamanho(self):
        """Verifica se precisa criar novo arquivo de log"""
        if os.path.exists(self.arquivo_atual):
            tamanho = os.path.getsize(self.arquivo_atual)
            if tamanho >= self.max_tamanho:
                self.rotacionar_log()
    
    def rotacionar_log(self):
        """Renomeia logs antigos"""
        # Encontra próximo número disponível
        i = 1
        while os.path.exists(f"{self.nome_base}_{i}.log"):
            i += 1
        
        # Renomeia arquivo atual
        novo_nome = f"{self.nome_base}_{i}.log"
        shutil.move(self.arquivo_atual, novo_nome)
        print(f"📦 Log rotacionado: {novo_nome}")
    
    def log(self, mensagem, nivel='INFO'):
        """Registra mensagem no log"""
        self.verificar_tamanho()
        
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        linha = f"[{timestamp}] {nivel}: {mensagem}\n"
        
        with open(self.arquivo_atual, 'a', encoding='utf-8') as f:
            f.write(linha)
        
        print(f"📝 Log registrado: {linha.strip()}")
    
    def visualizar_logs(self, ultimas_n=50):
        """Mostra últimas N linhas do log atual"""
        if not os.path.exists(self.arquivo_atual):
            print("📭 Nenhum log encontrado")
            return
        
        with open(self.arquivo_atual, 'r', encoding='utf-8') as f:
            linhas = f.readlines()
            ultimas = linhas[-ultimas_n:]
            
            print(f"\n📋 Últimas {len(ultimas)} linhas do log:")
            print("="*60)
            for linha in ultimas:
                print(linha.strip())
    
    def buscar_no_log(self, termo):
        """Busca termo em todos os arquivos de log"""
        resultados = []
        
        # Busca no arquivo atual
        if os.path.exists(self.arquivo_atual):
            with open(self.arquivo_atual, 'r', encoding='utf-8') as f:
                for num, linha in enumerate(f, 1):
                    if termo.lower() in linha.lower():
                        resultados.append((self.arquivo_atual, num, linha.strip()))
        
        # Busca em logs antigos
        i = 1
        while os.path.exists(f"{self.nome_base}_{i}.log"):
            arquivo = f"{self.nome_base}_{i}.log"
            with open(arquivo, 'r', encoding='utf-8') as f:
                for num, linha in enumerate(f, 1):
                    if termo.lower() in linha.lower():
                        resultados.append((arquivo, num, linha.strip()))
            i += 1
        
        return resultados

# Usando o sistema
logger = LoggerSistema()

# Simulando operações
logger.log("Sistema iniciado")
logger.log("Conexão com banco de dados estabelecida")

for i in range(5):
    logger.log(f"Processando item {i+1}")
    
logger.log("Erro na autenticação do usuário", "ERRO")
logger.log("Arquivo não encontrado", "AVISO")

# Visualizando logs
logger.visualizar_logs(10)

# Buscando erros
erros = logger.buscar_no_log("ERRO")
if erros:
    print("\n🔍 Erros encontrados:")
    for arquivo, linha, texto in erros:
        print(f"  • {arquivo}:{linha} - {texto}")
```

### 3. **Processador de Arquivos Texto**
```python
import re
from collections import Counter

class ProcessadorTexto:
    def __init__(self, arquivo):
        self.arquivo = arquivo
        self.conteudo = None
        self.carregar()
    
    def carregar(self):
        """Carrega o conteúdo do arquivo"""
        try:
            with open(self.arquivo, 'r', encoding='utf-8') as f:
                self.conteudo = f.read()
            print(f"✅ Arquivo '{self.arquivo}' carregado ({len(self.conteudo)} caracteres)")
        except FileNotFoundError:
            print(f"❌ Arquivo '{self.arquivo}' não encontrado!")
            self.conteudo = ""
    
    def estatisticas(self):
        """Retorna estatísticas do texto"""
        if not self.conteudo:
            return {}
        
        linhas = self.conteudo.count('\n') + 1
        palavras = self.conteudo.split()
        caracteres = len(self.conteudo)
        caracteres_sem_espaco = len(self.conteudo.replace(' ', '').replace('\n', ''))
        
        return {
            'linhas': linhas,
            'palavras': len(palavras),
            'caracteres': caracteres,
            'caracteres_sem_espaco': caracteres_sem_espaco,
            'tamanho_medio_palavra': sum(len(p) for p in palavras) / len(palavras) if palavras else 0
        }
    
    def palavras_mais_comuns(self, n=10):
        """Encontra as palavras mais frequentes"""
        if not self.conteudo:
            return []
        
        # Remove pontuação e converte para minúsculas
        palavras = re.findall(r'\b\w+\b', self.conteudo.lower())
        
        # Remove palavras muito comuns (stopwords)
        stopwords = {'a', 'o', 'e', 'de', 'da', 'do', 'em', 'para', 'com', 'um', 'uma', 'os', 'as'}
        palavras_filtradas = [p for p in palavras if p not in stopwords and len(p) > 2]
        
        contador = Counter(palavras_filtradas)
        return contador.most_common(n)
    
    def buscar_linhas_com(self, termo):
        """Busca linhas que contêm determinado termo"""
        if not self.conteudo:
            return []
        
        resultados = []
        with open(self.arquivo, 'r', encoding='utf-8') as f:
            for num, linha in enumerate(f, 1):
                if termo.lower() in linha.lower():
                    resultados.append((num, linha.strip()))
        
        return resultados
    
    def substituir(self, antigo, novo, salvar_em=None):
        """Substitui texto e salva em novo arquivo"""
        if not self.conteudo:
            return
        
        novo_conteudo = self.conteudo.replace(antigo, novo)
        
        arquivo_saida = salvar_em if salvar_em else f"modificado_{self.arquivo}"
        
        with open(arquivo_saida, 'w', encoding='utf-8') as f:
            f.write(novo_conteudo)
        
        substituicoes = self.conteudo.count(antigo)
        print(f"✅ {substituicoes} substituições realizadas")
        print(f"💾 Arquivo salvo como: {arquivo_saida}")
        
        return arquivo_saida

# Exemplo de uso
processador = ProcessadorTexto('artigo.txt')

# Estatísticas
stats = processador.estatisticas()
print("\n📊 ESTATÍSTICAS DO TEXTO:")
for chave, valor in stats.items():
    print(f"  {chave}: {valor:.2f}" if isinstance(valor, float) else f"  {chave}: {valor}")

# Palavras mais comuns
print("\n📝 PALAVRAS MAIS COMUNS:")
palavras = processador.palavras_mais_comuns(5)
for palavra, freq in palavras:
    print(f"  • {palavra}: {freq} vezes")

# Buscar termo
resultados = processador.buscar_linhas_com("importante")
if resultados:
    print(f"\n🔍 Linhas com 'importante':")
    for num, linha in resultados:
        print(f"  Linha {num}: {linha[:50]}...")
```

### 4. **Sistema de Backup Automático**
```python
import os
import shutil
import datetime
import zipfile

class SistemaBackup:
    def __init__(self, pasta_origem, pasta_backup):
        self.pasta_origem = pasta_origem
        self.pasta_backup = pasta_backup
        
        # Cria pasta de backup se não existir
        if not os.path.exists(pasta_backup):
            os.makedirs(pasta_backup)
            print(f"📁 Pasta de backup criada: {pasta_backup}")
    
    def listar_arquivos(self, pasta=None):
        """Lista todos os arquivos em uma pasta"""
        pasta = pasta or self.pasta_origem
        arquivos = []
        
        for raiz, dirs, files in os.walk(pasta):
            for arquivo in files:
                caminho_completo = os.path.join(raiz, arquivo)
                caminho_relativo = os.path.relpath(caminho_completo, pasta)
                
                stats = os.stat(caminho_completo)
                info = {
                    'nome': arquivo,
                    'caminho': caminho_completo,
                    'relativo': caminho_relativo,
                    'tamanho': stats.st_size,
                    'modificado': datetime.datetime.fromtimestamp(stats.st_mtime)
                }
                arquivos.append(info)
        
        return arquivos
    
    def verificar_alteracoes(self):
        """Verifica quais arquivos precisam de backup"""
        # Lista arquivos da origem
        arquivos_origem = {a['relativo']: a for a in self.listar_arquivos()}
        
        # Lista arquivos do último backup (se existir)
        arquivos_backup = {}
        arquivo_controle = os.path.join(self.pasta_backup, '.backup_controle.txt')
        
        if os.path.exists(arquivo_controle):
            with open(arquivo_controle, 'r') as f:
                for linha in f:
                    nome, data = linha.strip().split('|')
                    arquivos_backup[nome] = datetime.datetime.fromisoformat(data)
        
        # Compara alterações
        novos = []
        modificados = []
        
        for nome, info in arquivos_origem.items():
            if nome not in arquivos_backup:
                novos.append(nome)
            elif info['modificado'] > arquivos_backup[nome]:
                modificados.append(nome)
        
        return novos, modificados
    
    def criar_backup(self, compactar=True):
        """Cria backup dos arquivos"""
        timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        
        if compactar:
            # Backup compactado
            nome_zip = os.path.join(self.pasta_backup, f"backup_{timestamp}.zip")
            
            with zipfile.ZipFile(nome_zip, 'w', zipfile.ZIP_DEFLATED) as zipf:
                for raiz, dirs, files in os.walk(self.pasta_origem):
                    for arquivo in files:
                        caminho_completo = os.path.join(raiz, arquivo)
                        caminho_relativo = os.path.relpath(caminho_completo, self.pasta_origem)
                        zipf.write(caminho_completo, caminho_relativo)
            
            print(f"✅ Backup compactado criado: {nome_zip}")
            return nome_zip
        
        else:
            # Backup simples (cópia dos arquivos)
            pasta_destino = os.path.join(self.pasta_backup, f"backup_{timestamp}")
            os.makedirs(pasta_destino)
            
            for arquivo in self.listar_arquivos():
                destino = os.path.join(pasta_destino, arquivo['relativo'])
                os.makedirs(os.path.dirname(destino), exist_ok=True)
                shutil.copy2(arquivo['caminho'], destino)
            
            print(f"✅ Backup simples criado: {pasta_destino}")
            
            # Salva controle de backup
            arquivo_controle = os.path.join(self.pasta_backup, '.backup_controle.txt')
            with open(arquivo_controle, 'w') as f:
                for arquivo in self.listar_arquivos():
                    f.write(f"{arquivo['relativo']}|{arquivo['modificado'].isoformat()}\n")
            
            return pasta_destino
    
    def restaurar_backup(self, arquivo_backup, destino=None):
        """Restaura um backup específico"""
        destino = destino or self.pasta_origem
        
        if arquivo_backup.endswith('.zip'):
            # Restaura de ZIP
            with zipfile.ZipFile(arquivo_backup, 'r') as zipf:
                zipf.extractall(destino)
            print(f"✅ Backup restaurado de: {arquivo_backup}")
        
        else:
            # Restaura de pasta
            for item in os.listdir(arquivo_backup):
                origem = os.path.join(arquivo_backup, item)
                destino_item = os.path.join(destino, item)
                
                if os.path.isdir(origem):
                    shutil.copytree(origem, destino_item, dirs_exist_ok=True)
                else:
                    shutil.copy2(origem, destino_item)
            
            print(f"✅ Backup restaurado de: {arquivo_backup}")
    
    def listar_backups(self):
        """Lista todos os backups disponíveis"""
        backups = []
        
        for item in os.listdir(self.pasta_backup):
            if item.startswith('backup_') and (item.endswith('.zip') or os.path.isdir(os.path.join(self.pasta_backup, item))):
                caminho = os.path.join(self.pasta_backup, item)
                stats = os.stat(caminho)
                
                backups.append({
                    'nome': item,
                    'caminho': caminho,
                    'tamanho': stats.st_size,
                    'data': datetime.datetime.fromtimestamp(stats.st_mtime),
                    'tipo': 'ZIP' if item.endswith('.zip') else 'PASTA'
                })
        
        return sorted(backups, key=lambda x: x['data'], reverse=True)

# Exemplo de uso
backup = SistemaBackup(
    pasta_origem='./dados_importantes',
    pasta_backup='./backups'
)

# Verifica alterações
novos, modificados = backup.verificar_alteracoes()
print(f"\n📊 STATUS:")
print(f"  Novos arquivos: {len(novos)}")
print(f"  Arquivos modificados: {len(modificados)}")

if novos or modificados:
    # Cria backup
    backup.criar_backup(compactar=True)

# Lista backups disponíveis
print("\n📦 BACKUPS DISPONÍVEIS:")
for b in backup.listar_backups()[:5]:
    print(f"  • {b['data'].strftime('%d/%m/%Y %H:%M')} - {b['nome']} ({b['tipo']})")
```

---

## BOAS PRÁTICAS E TRATAMENTO DE ERROS

### 1. **Tratamento de Exceções**
```python
import os

def ler_arquivo_com_seguranca(caminho):
    """
    Função robusta para leitura de arquivos
    Trata todos os possíveis erros
    """
    try:
        with open(caminho, 'r', encoding='utf-8') as arquivo:
            return arquivo.read()
    
    except FileNotFoundError:
        print(f"❌ Arquivo não encontrado: {caminho}")
        return None
    
    except PermissionError:
        print(f"❌ Sem permissão para ler: {caminho}")
        return None
    
    except UnicodeDecodeError:
        print(f"❌ Erro de codificação no arquivo: {caminho}")
        # Tenta com codificação diferente
        try:
            with open(caminho, 'r', encoding='latin-1') as arquivo:
                return arquivo.read()
        except:
            return None
    
    except IOError as e:
        print(f"❌ Erro de E/S: {e}")
        return None
    
    except Exception as e:
        print(f"❌ Erro inesperado: {e}")
        return None

def escrever_arquivo_com_seguranca(caminho, conteudo, modo='w'):
    """
    Função robusta para escrita de arquivos
    """
    try:
        # Cria diretórios se necessário
        diretorio = os.path.dirname(caminho)
        if diretorio and not os.path.exists(diretorio):
            os.makedirs(diretorio)
            print(f"📁 Diretório criado: {diretorio}")
        
        with open(caminho, modo, encoding='utf-8') as arquivo:
            arquivo.write(conteudo)
        
        print(f"✅ Arquivo salvo: {caminho}")
        return True
    
    except Exception as e:
        print(f"❌ Erro ao salvar: {e}")
        return False
```

### 2. **Validações e Verificações**
```python
import os

def verificar_arquivo(caminho):
    """Verifica status do arquivo com informações detalhadas"""
    if not os.path.exists(caminho):
        return {
            'existe': False,
            'mensagem': 'Arquivo não existe'
        }
    
    info = {
        'existe': True,
        'caminho': os.path.abspath(caminho),
        'tamanho': os.path.getsize(caminho),
        'modificado': datetime.datetime.fromtimestamp(
            os.path.getmtime(caminho)
        ).strftime('%d/%m/%Y %H:%M'),
        'acessado': datetime.datetime.fromtimestamp(
            os.path.getatime(caminho)
        ).strftime('%d/%m/%Y %H:%M'),
        'extensao': os.path.splitext(caminho)[1],
        'nome': os.path.basename(caminho),
        'diretorio': os.path.dirname(caminho),
        'permissoes': {
            'leitura': os.access(caminho, os.R_OK),
            'escrita': os.access(caminho, os.W_OK),
            'execucao': os.access(caminho, os.X_OK)
        }
    }
    
    return info

def validar_nome_arquivo(nome):
    """Valida se nome de arquivo é válido"""
    caracteres_invalidos = '<>:"/\\|?*'
    
    problemas = []
    
    if not nome:
        problemas.append("Nome vazio")
    
    if len(nome) > 255:
        problemas.append("Nome muito longo (>255 caracteres)")
    
    for char in caracteres_invalidos:
        if char in nome:
            problemas.append(f"Caractere inválido: {char}")
    
    if nome.startswith('.'):
        problemas.append("Não pode começar com ponto")
    
    if nome.strip() != nome:
        problemas.append("Não pode ter espaços no início/fim")
    
    return {
        'valido': len(problemas) == 0,
        'problemas': problemas
    }
```

### 3. **Gerenciamento de Recursos**
```python
class GerenciadorArquivos:
    """
    Classe para gerenciar múltiplos arquivos de forma segura
    """
    def __init__(self):
        self.arquivos_abertos = []
    
    def abrir(self, caminho, modo):
        """Abre arquivo e registra para controle"""
        try:
            arquivo = open(caminho, modo)
            self.arquivos_abertos.append(arquivo)
            print(f"📂 Arquivo aberto: {caminho}")
            return arquivo
        except Exception as e:
            print(f"❌ Erro ao abrir: {e}")
            return None
    
    def fechar(self, arquivo=None):
        """Fecha arquivo específico ou todos"""
        if arquivo:
            try:
                arquivo.close()
                self.arquivos_abertos.remove(arquivo)
                print(f"🔒 Arquivo fechado")
            except:
                pass
        else:
            # Fecha todos
            for arq in self.arquivos_abertos[:]:
                try:
                    arq.close()
                except:
                    pass
            self.arquivos_abertos.clear()
            print(f"🔒 Todos os arquivos fechados")
    
    def __enter__(self):
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.fechar()
```

---

## RESUMO E MELHORES PRÁTICAS

### ✅ **Checklist para Manipulação de Arquivos**

1. **SEMPRE use `with` para abrir arquivos**
   ```python
   with open('arquivo.txt', 'r') as f:
       dados = f.read()
   ```

2. **Especifique a codificação corretamente**
   ```python
   with open('arquivo.txt', 'r', encoding='utf-8') as f:
   ```

3. **Trate exceções adequadamente**
   ```python
   try:
       with open('arquivo.txt', 'r') as f:
           dados = f.read()
   except FileNotFoundError:
       print("Arquivo não encontrado")
   ```

4. **Feche arquivos em caso de erro**
   ```python
   # O 'with' faz isso automaticamente!
   ```

5. **Use caminhos absolutos quando necessário**
   ```python
   caminho = os.path.abspath('arquivo.txt')
   ```

6. **Verifique permissões antes de operar**
   ```python
   if os.access('arquivo.txt', os.R_OK):
       # Arquivo pode ser lido
   ```

### 🚫 **Erros Comuns a Evitar**

1. **Esquecer de fechar o arquivo**
   ```python
   # ERRADO
   f = open('dados.txt', 'w')
   f.write('dados')
   # Arquivo continua aberto!
   
   # CORRETO
   with open('dados.txt', 'w') as f:
       f.write('dados')
   ```

2. **Ignorar codificação**
   ```python
   # ERRADO (pode causar erros com acentos)
   with open('dados.txt', 'r') as f:
   
   # CORRETO
   with open('dados.txt', 'r', encoding='utf-8') as f:
   ```

3. **Não tratar arquivos inexistentes**
   ```python
   # ERRADO (crasha se arquivo não existe)
   with open('config.txt', 'r') as f:
   
   # CORRETO
   try:
       with open('config.txt', 'r') as f:
           dados = f.read()
   except FileNotFoundError:
       dados = {}  # Valor padrão
   ```

4. **Carregar arquivos grandes inteiros na memória**
   ```python
   # ERRADO (arquivo muito grande)
   with open('grande.txt', 'r') as f:
       dados = f.read()  # Pode estourar memória!
   
   # CORRETO
   with open('grande.txt', 'r') as f:
       for linha in f:  # Lê linha por linha
           processar(linha)
   ```
