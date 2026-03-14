# Manual Completo do Módulo `sqlite3` para Python

## Índice

1. [Introdução](#introdução)
2. [Conectando ao Banco de Dados](#conectando-ao-banco-de-dados)
3. [Criando um Cursor](#criando-um-cursor)
4. [Executando Comandos SQL](#executando-comandos-sql)
   - [execute()](#execute)
   - [executemany()](#executemany)
   - [executescript()](#executescript)
5. [Recuperando Dados](#recuperando-dados)
   - [fetchone()](#fetchone)
   - [fetchmany()](#fetchmany)
   - [fetchall()](#fetchall)
6. [Parâmetros em Consultas (Prevenção de SQL Injection)](#parâmetros-em-consultas-prevenção-de-sql-injection)
7. [Gerenciamento de Transações](#gerenciamento-de-transações)
   - [commit()](#commit)
   - [rollback()](#rollback)
8. [Fechando Conexão e Cursor](#fechando-conexão-e-cursor)
9. [Tipos de Dados e Adaptação](#tipos-de-dados-e-adaptação)
   - [Adaptadores Padrão](#adaptadores-padrão)
   - [Registrando Adaptadores e Conversores Personalizados](#registrando-adaptadores-e-conversores-personalizados)
10. [Row Factory](#row-factory)
    - [sqlite3.Row](#sqlite3row)
    - [Fábrica Customizada](#fábrica-customizada)
11. [Funções Definidas pelo Usuário](#funções-definidas-pelo-usuário)
    - [create_function()](#create_function)
12. [Agregados Definidos pelo Usuário](#agregados-definidos-pelo-usuário)
    - [create_aggregate()](#create_aggregate)
13. [Colações Definidas pelo Usuário](#colações-definidas-pelo-usuário)
    - [create_collation()](#create_collation)
14. [Trabalhando com BLOB](#trabalhando-com-blob)
15. [Backup de Banco de Dados](#backup-de-banco-de-dados)
    - [backup()](#backup)
16. [Carregando Extensões](#carregando-extensões)
    - [enable_load_extension()](#enable_load_extension)
    - [load_extension()](#load_extension)
17. [Lidando com Erros e Exceções](#lidando-com-erros-e-exceções)
18. [Usando a Conexão como Gerenciador de Contexto (with)](#usando-a-conexão-como-gerenciador-de-contexto-with)
19. [Configurando Timeout](#configurando-timeout)
20. [Obtendo Informações do Esquema (PRAGMA)](#obtendo-informações-do-esquema-pragma)
21. [Dicas de Desempenho e Boas Práticas](#dicas-de-desempenho-e-boas-práticas)
22. [Exemplos Completos](#exemplos-completos)

---

## Introdução

O módulo `sqlite3` é uma interface para o banco de dados SQLite, que é um banco de dados relacional leve, embutido e sem necessidade de configuração de servidor. Ele faz parte da biblioteca padrão do Python desde a versão 2.5, portanto não é necessário instalar pacotes adicionais.

O SQLite armazenda o banco de dados em um único arquivo (ou em memória). É amplamente utilizado em aplicações desktop, dispositivos móveis e para prototipagem.

Este manual cobre todas as funcionalidades disponíveis no módulo `sqlite3` até a versão Python 3.12. Os tópicos são organizados de forma didática, com explicações detalhadas e exemplos práticos.

---

## Conectando ao Banco de Dados

### `sqlite3.connect(database[, timeout, detect_types, isolation_level, check_same_thread, factory, cached_statements, uri])`

Estabelece uma conexão com o banco de dados SQLite.

**Parâmetros:**

- `database` (str): Caminho para o arquivo do banco de dados. Pode ser `':memory:'` para criar um banco em memória.
- `timeout` (float, opcional): Tempo máximo em segundos para esperar quando o banco estiver bloqueado. Padrão: 5.0.
- `detect_types` (int, opcional): Controla a detecção automática de tipos. Use `sqlite3.PARSE_DECLTYPES` e/ou `sqlite3.PARSE_COLNAMES`.
- `isolation_level` (str ou None, opcional): Define o nível de isolamento da transação. Pode ser `None` (autocommit), `''` (padrão, início implícito), `'DEFERRED'`, `'IMMEDIATE'` ou `'EXCLUSIVE'`.
- `check_same_thread` (bool, opcional): Se `False`, a conexão pode ser usada em múltiplas threads. Padrão: `True`.
- `factory` (classe, opcional): Classe personalizada para substituir a `Connection` padrão.
- `cached_statements` (int, opcional): Número de declarações SQL em cache. Padrão: 100.
- `uri` (bool, opcional): Se `True`, interpreta `database` como URI. Permite opções como `?mode=ro`. Padrão: `False`.

**Retorno:** Um objeto `Connection`.

**Exemplos:**

```python
import sqlite3

# Banco em arquivo
conn = sqlite3.connect('meubanco.db')

# Banco em memória
conn = sqlite3.connect(':memory:')

# Com timeout de 10 segundos
conn = sqlite3.connect('meubanco.db', timeout=10)

# Usando URI para modo somente leitura
conn = sqlite3.connect('file:meubanco.db?mode=ro', uri=True)
```

---

## Criando um Cursor

### `connection.cursor()`

Cria um objeto cursor, que é usado para executar comandos SQL e buscar resultados.

**Retorno:** Um objeto `Cursor`.

**Exemplo:**

```python
cursor = conn.cursor()
```

---

## Executando Comandos SQL

### `cursor.execute(sql[, parameters])`

Executa uma única instrução SQL. Pode receber parâmetros para consultas parametrizadas.

**Parâmetros:**

- `sql` (str): Instrução SQL a ser executada.
- `parameters` (opcional): Sequência ou dicionário com os parâmetros para substituir placeholders (`?` ou `:nome`).

**Retorno:** O próprio cursor (para encadeamento).

**Exemplos:**

```python
# Criar tabela
cursor.execute('CREATE TABLE IF NOT EXISTS usuarios (id INTEGER PRIMARY KEY, nome TEXT, idade INTEGER)')

# Inserir com placeholders (recomendado)
cursor.execute('INSERT INTO usuarios (nome, idade) VALUES (?, ?)', ('Alice', 30))

# Usando named placeholders
cursor.execute('INSERT INTO usuarios (nome, idade) VALUES (:nome, :idade)', {'nome': 'Bob', 'idade': 25})

# Atualizar
cursor.execute('UPDATE usuarios SET idade = ? WHERE nome = ?', (31, 'Alice'))

# Deletar
cursor.execute('DELETE FROM usuarios WHERE nome = ?', ('Bob',))
```

### `cursor.executemany(sql, seq_of_parameters)`

Executa o mesmo comando SQL para cada conjunto de parâmetros na sequência fornecida. Útil para inserções em lote.

**Parâmetros:**

- `sql` (str): Instrução SQL com placeholders.
- `seq_of_parameters` (iterable): Sequência de parâmetros (cada um pode ser sequência ou dicionário).

**Retorno:** O cursor.

**Exemplo:**

```python
dados = [('Carlos', 40), ('Débora', 35), ('Eduardo', 28)]
cursor.executemany('INSERT INTO usuarios (nome, idade) VALUES (?, ?)', dados)
```

### `cursor.executescript(sql_script)`

Executa um script SQL contendo múltiplas instruções separadas por ponto e vírgula.

**Parâmetros:**

- `sql_script` (str): Script SQL.

**Retorno:** O cursor.

**Exemplo:**

```python
script = '''
DROP TABLE IF EXISTS usuarios;
CREATE TABLE usuarios (id INTEGER PRIMARY KEY, nome TEXT, idade INTEGER);
INSERT INTO usuarios (nome, idade) VALUES ('Fernanda', 22);
INSERT INTO usuarios (nome, idade) VALUES ('Gustavo', 27);
'''
cursor.executescript(script)
```

---

## Recuperando Dados

Após executar uma consulta `SELECT`, os resultados podem ser obtidos através dos métodos de fetch do cursor.

### `cursor.fetchone()`

Retorna a próxima linha do conjunto de resultados como uma tupla, ou `None` se não houver mais linhas.

**Exemplo:**

```python
cursor.execute('SELECT * FROM usuarios')
linha = cursor.fetchone()
while linha:
    print(linha)
    linha = cursor.fetchone()
```

### `cursor.fetchmany(size=cursor.arraysize)`

Retorna uma lista com até `size` linhas do conjunto de resultados. Se não houver linhas suficientes, retorna uma lista menor. O atributo `arraysize` define o tamanho padrão.

**Exemplo:**

```python
cursor.execute('SELECT * FROM usuarios')
while True:
    linhas = cursor.fetchmany(5)
    if not linhas:
        break
    for linha in linhas:
        print(linha)
```

### `cursor.fetchall()`

Retorna todas as linhas restantes como uma lista de tuplas.

**Exemplo:**

```python
cursor.execute('SELECT * FROM usuarios')
todas = cursor.fetchall()
for linha in todas:
    print(linha)
```

---

## Parâmetros em Consultas (Prevenção de SQL Injection)

Sempre utilize parâmetros em vez de concatenar strings para evitar ataques de injeção de SQL e problemas com escaping.

Dois estilos de placeholders:

- **Posicional:** `?` e uma sequência (tupla ou lista).
- **Nomeado:** `:nome` e um dicionário.

**Exemplos:**

```python
# Posicional
cursor.execute('SELECT * FROM usuarios WHERE idade > ?', (25,))

# Nomeado
cursor.execute('SELECT * FROM usuarios WHERE idade > :min_idade', {'min_idade': 25})
```

**Nota:** Nunca faça:

```python
# PERIGO! Vulnerável a SQL injection
cursor.execute(f"SELECT * FROM usuarios WHERE nome = '{nome_usuario}'")
```

---

## Gerenciamento de Transações

Por padrão, o SQLite opera em modo de transação implícita: uma transação é iniciada antes da primeira instrução DML (INSERT, UPDATE, DELETE) e deve ser confirmada com `commit()` ou desfeita com `rollback()`. Instruções DDL (CREATE, DROP, ALTER) geralmente causam um commit imediato.

### `connection.commit()`

Confirma (salva) todas as alterações pendentes desde o último `commit()` ou `rollback()`.

### `connection.rollback()`

Desfaz todas as alterações desde o início da transação atual.

**Exemplo:**

```python
try:
    cursor.execute('INSERT INTO usuarios (nome) VALUES (?)', ('João',))
    cursor.execute('INSERT INTO usuarios (nome) VALUES (?)', ('Maria',))
    conn.commit()
except Exception:
    conn.rollback()
    raise
```

**Modo autocommit:** Se `isolation_level` for definido como `None` na conexão, cada instrução é commitada automaticamente.

```python
conn = sqlite3.connect('meubanco.db', isolation_level=None)
cursor.execute('INSERT INTO usuarios (nome) VALUES (?)', ('João',))  # já commitado
```

---

## Fechando Conexão e Cursor

Embora os cursores sejam fechados automaticamente quando são coletados pelo garbage collector, é boa prática fechá-los explicitamente. A conexão deve ser fechada ao final do uso para liberar recursos e garantir que as alterações sejam salvas.

### `cursor.close()`

Fecha o cursor. O cursor não poderá mais ser usado.

### `connection.close()`

Fecha a conexão. Qualquer tentativa de usar a conexão após isso levantará uma exceção. Transações não commitadas serão revertidas.

**Exemplo:**

```python
cursor.close()
conn.close()
```

---

## Tipos de Dados e Adaptação

O SQLite suporta apenas cinco tipos nativos: `NULL`, `INTEGER`, `REAL`, `TEXT` e `BLOB`. O módulo `sqlite3` converte automaticamente tipos Python para esses tipos e vice-versa de acordo com uma tabela padrão.

### Adaptadores Padrão

| Tipo Python   | Tipo SQLite |
|---------------|-------------|
| `None`        | `NULL`      |
| `int`         | `INTEGER`   |
| `float`       | `REAL`      |
| `str`         | `TEXT`      |
| `bytes`       | `BLOB`      |

Para outros tipos como `datetime.date`, `datetime.datetime` e `bool`, o módulo oferece adaptadores opcionais que podem ser registrados.

### Registrando Adaptadores e Conversores Personalizados

#### `sqlite3.register_adapter(type, callable)`

Registra uma função que converte um tipo Python para um tipo suportado pelo SQLite (geralmente `str`, `bytes`, `int` ou `float`).

**Exemplo:** Adaptar objetos `datetime.date` para string ISO.

```python
import datetime

def adapt_date(data):
    return data.isoformat()

sqlite3.register_adapter(datetime.date, adapt_date)
```

#### `sqlite3.register_converter(sqlite_type, callable)`

Registra uma função que converte um valor do SQLite (sempre como `bytes`) para um tipo Python.

**Exemplo:** Converter strings ISO de volta para `date`.

```python
def convert_date(byte_string):
    return datetime.date.fromisoformat(byte_string.decode('utf-8'))

sqlite3.register_converter('date', convert_date)
```

Para usar os conversores, é necessário ativar a detecção de tipos na conexão com `detect_types=sqlite3.PARSE_DECLTYPES` (baseado no tipo declarado na tabela) ou `sqlite3.PARSE_COLNAMES` (baseado no nome da coluna na consulta).

**Exemplo completo:**

```python
conn = sqlite3.connect(':memory:', detect_types=sqlite3.PARSE_DECLTYPES)
cursor = conn.cursor()
cursor.execute('CREATE TABLE eventos (id INTEGER, data DATE)')
data = datetime.date(2025, 3, 14)
cursor.execute('INSERT INTO eventos VALUES (?, ?)', (1, data))
cursor.execute('SELECT data FROM eventos WHERE id = 1')
resultado = cursor.fetchone()[0]  # resultado é um objeto date
print(resultado, type(resultado))
```

---

## Row Factory

Por padrão, as linhas retornadas pelos métodos fetch são tuplas. Podemos alterar isso atribuindo uma fábrica de linhas à conexão.

### `sqlite3.Row`

A classe `sqlite3.Row` fornece acesso indexado (por posição) e por nome (como dicionário) às colunas.

**Exemplo:**

```python
conn.row_factory = sqlite3.Row
cursor = conn.cursor()
cursor.execute('SELECT nome, idade FROM usuarios')
linha = cursor.fetchone()
print(linha['nome'])     # acesso por nome
print(linha[0])          # acesso por índice
print(linha.keys())      # lista de nomes das colunas
```

### Fábrica Customizada

Podemos definir nossa própria função que recebe o cursor e a tupla original e retorna um objeto de linha personalizado (ex.: dicionário).

**Exemplo:** Fábrica de dicionários.

```python
def dict_factory(cursor, row):
    fields = [column[0] for column in cursor.description]
    return {key: value for key, value in zip(fields, row)}

conn.row_factory = dict_factory
cursor.execute('SELECT nome, idade FROM usuarios')
linha = cursor.fetchone()
print(linha['nome'])  # acesso como dicionário
```

---

## Funções Definidas pelo Usuário

### `connection.create_function(name, num_params, func, deterministic=False)`

Cria uma função SQL personalizada que pode ser usada em consultas.

**Parâmetros:**

- `name` (str): Nome da função no SQL.
- `num_params` (int): Número de parâmetros que a função aceita.
- `func` (callable): Função Python que será chamada.
- `deterministic` (bool, Python 3.8+): Se `True`, a função sempre retorna o mesmo resultado para os mesmos argumentos, permitindo otimizações.

**Exemplo:** Função para calcular o quadrado de um número.

```python
def quadrado(x):
    return x * x

conn.create_function('quadrado', 1, quadrado)
cursor.execute('SELECT quadrado(idade) FROM usuarios')
for row in cursor:
    print(row[0])
```

---

## Agregados Definidos pelo Usuário

### `connection.create_aggregate(name, num_params, aggregate_class)`

Cria uma função de agregação SQL personalizada.

**Parâmetros:**

- `name` (str): Nome da agregação.
- `num_params` (int): Número de parâmetros.
- `aggregate_class` (class): Classe que implementa os métodos `step()` e `finalize()`.

**Exemplo:** Agregado para calcular a média geométrica.

```python
class MediaGeometrica:
    def __init__(self):
        self.produto = 1.0
        self.contagem = 0
    def step(self, valor):
        if valor is not None:
            self.produto *= valor
            self.contagem += 1
    def finalize(self):
        if self.contagem == 0:
            return None
        return self.produto ** (1.0 / self.contagem)

conn.create_aggregate('media_geometrica', 1, MediaGeometrica)
cursor.execute('SELECT media_geometrica(idade) FROM usuarios')
print(cursor.fetchone()[0])
```

---

## Colações Definidas pelo Usuário

### `connection.create_collation(name, callable)`

Cria uma regra de ordenação personalizada para uso em cláusulas `ORDER BY` e comparações.

**Parâmetros:**

- `name` (str): Nome da collation.
- `callable` (callable): Função que recebe duas strings e retorna -1, 0 ou 1 (como `cmp`).

**Exemplo:** Collation que ignora acentos (simplificado).

```python
import unicodedata

def remove_acentos(s):
    return ''.join(c for c in unicodedata.normalize('NFD', s) if unicodedata.category(c) != 'Mn')

def collation_insensivel_acentos(a, b):
    a_norm = remove_acentos(a).lower()
    b_norm = remove_acentos(b).lower()
    return (a_norm > b_norm) - (a_norm < b_norm)

conn.create_collation('sem_acentos', collation_insensivel_acentos)
cursor.execute('SELECT nome FROM usuarios ORDER BY nome COLLATE sem_acentos')
```

---

## Trabalhando com BLOB

Dados binários (BLOB) são representados por objetos `bytes` em Python. Basta inserir `bytes` que o SQLite os armazenará como BLOB.

**Exemplo:**

```python
cursor.execute('CREATE TABLE imagens (id INTEGER, dados BLOB)')
with open('foto.jpg', 'rb') as f:
    dados = f.read()
cursor.execute('INSERT INTO imagens VALUES (?, ?)', (1, dados))
conn.commit()

cursor.execute('SELECT dados FROM imagens WHERE id = 1')
dados_lidos = cursor.fetchone()[0]
with open('foto_copia.jpg', 'wb') as f:
    f.write(dados_lidos)
```

---

## Backup de Banco de Dados

### `connection.backup(target, *, pages=-1, progress=None, name='main', sleep=0.250)`

Copia o conteúdo de uma conexão para outra (útil para backup ou cópia). Disponível a partir do Python 3.7.

**Parâmetros:**

- `target` (Connection): Conexão de destino (pode ser em memória ou arquivo).
- `pages` (int): Número de páginas a copiar por iteração. `-1` copia tudo de uma vez.
- `progress` (callable, opcional): Função chamada a cada iteração com três argumentos: `status`, `remaining`, `total`.
- `name` (str): Nome do banco de dados na origem (geralmente `'main'`).
- `sleep` (float): Tempo de pausa entre iterações.

**Exemplo:** Backup de um banco de arquivo para outro arquivo.

```python
origem = sqlite3.connect('meubanco.db')
destino = sqlite3.connect('backup.db')
origem.backup(destino)
destino.close()
origem.close()
```

**Exemplo com progresso:**

```python
def progress(status, remaining, total):
    print(f'Copiadas {total-remaining} de {total} páginas...')

origem.backup(destino, pages=1, progress=progress)
```

---

## Carregando Extensões

O SQLite suporta carregar extensões compartilhadas (arquivos `.dll`/`.so`). Para habilitar isso no Python:

### `connection.enable_load_extension(enabled)`

Habilita ou desabilita o carregamento de extensões na conexão.

### `connection.load_extension(path)`

Carrega uma extensão do arquivo especificado.

**Exemplo:**

```python
conn.enable_load_extension(True)
conn.load_extension('./minha_extensao.so')
conn.enable_load_extension(False)
```

**Nota:** Em algumas distribuições, o suporte a extensões pode estar desabilitado na compilação do SQLite.

---

## Lidando com Erros e Exceções

O módulo `sqlite3` define várias exceções, todas herdadas de `sqlite3.Error`:

- `sqlite3.Warning`
- `sqlite3.Error`
  - `sqlite3.DatabaseError`
    - `sqlite3.IntegrityError` (ex.: violação de chave estrangeira)
    - `sqlite3.ProgrammingError` (erro de programação, como tabela não encontrada)
    - `sqlite3.OperationalError` (erros de operação, como banco bloqueado)
    - `sqlite3.NotSupportedError`
    - `sqlite3.InternalError`

**Exemplo de tratamento:**

```python
try:
    cursor.execute('INSERT INTO usuarios (nome) VALUES (?)', (None,))
except sqlite3.IntegrityError as e:
    print(f'Erro de integridade: {e}')
except sqlite3.OperationalError as e:
    print(f'Erro operacional: {e}')
except sqlite3.Error as e:
    print(f'Erro genérico do SQLite: {e}')
```

---

## Usando a Conexão como Gerenciador de Contexto (with)

A partir do Python 3.11, objetos `Connection` podem ser usados como gerenciadores de contexto que gerenciam transações automaticamente:

- Se o bloco terminar sem exceção, `commit()` é chamado.
- Se ocorrer exceção, `rollback()` é chamado.

**Exemplo:**

```python
conn = sqlite3.connect('meubanco.db')
with conn:
    conn.execute('INSERT INTO usuarios (nome) VALUES (?)', ('Paulo',))
    # Ao sair do with, faz commit automático
```

**Nota:** O cursor não é fechado automaticamente. Também é possível usar o cursor em um `with` (fecha o cursor ao sair).

```python
with conn:
    with conn.cursor() as cursor:
        cursor.execute('SELECT * FROM usuarios')
        print(cursor.fetchall())
    # cursor fechado
# transação commitada
```

---

## Configurando Timeout

O parâmetro `timeout` da conexão define quantos segundos esperar quando o banco está bloqueado por outra transação. Pode ser alterado após a conexão com `connection.execute('PRAGMA busy_timeout = milissegundos')`.

**Exemplo:**

```python
conn.execute('PRAGMA busy_timeout = 3000')  # 3 segundos
```

---

## Obtendo Informações do Esquema (PRAGMA)

Os comandos `PRAGMA` do SQLite fornecem metadados e configurações. Eles podem ser executados com `execute()` e os resultados lidos normalmente.

**Exemplos úteis:**

```python
# Listar tabelas
cursor.execute("SELECT name FROM sqlite_master WHERE type='table'")
print(cursor.fetchall())

# Informações sobre colunas de uma tabela
cursor.execute("PRAGMA table_info(usuarios)")
for col in cursor.fetchall():
    print(col)

# Índices de uma tabela
cursor.execute("PRAGMA index_list(usuarios)")
print(cursor.fetchall())

# Obter esquema completo da tabela
cursor.execute("SELECT sql FROM sqlite_master WHERE type='table' AND name='usuarios'")
print(cursor.fetchone()[0])
```

---

## Dicas de Desempenho e Boas Práticas

- **Use `executemany` para inserções em lote** em vez de várias chamadas a `execute`.
- **Sempre use parâmetros** em vez de concatenação de strings.
- **Utilize transações** para agrupar operações relacionadas.
- **Feche a conexão** quando não for mais necessária.
- **Considere usar `row_factory`** para acesso mais conveniente aos dados.
- **Para grandes volumes de dados, use `fetchmany`** em vez de `fetchall` para evitar consumo excessivo de memória.
- **Aproveite índices** no SQLite para consultas frequentes.
- **Backup regularmente** usando o método `backup()`.

---

## Exemplos Completos

### Exemplo 1: CRUD básico com dicionário como row_factory

```python
import sqlite3

def dict_factory(cursor, row):
    d = {}
    for idx, col in enumerate(cursor.description):
        d[col[0]] = row[idx]
    return d

conn = sqlite3.connect(':memory:')
conn.row_factory = dict_factory
cursor = conn.cursor()

cursor.execute('''
    CREATE TABLE clientes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        email TEXT UNIQUE NOT NULL
    )
''')

dados = [
    ('Ana Silva', 'ana@email.com'),
    ('Carlos Souza', 'carlos@email.com'),
]
cursor.executemany('INSERT INTO clientes (nome, email) VALUES (?, ?)', dados)
conn.commit()

cursor.execute('SELECT * FROM clientes')
for cliente in cursor.fetchall():
    print(cliente)

cursor.execute('SELECT * FROM clientes WHERE nome LIKE ?', ('A%',))
print(cursor.fetchall())

conn.close()
```

### Exemplo 2: Função personalizada e agregado

```python
import sqlite3
import math

conn = sqlite3.connect(':memory:')
cursor = conn.cursor()

cursor.execute('CREATE TABLE numeros (valor REAL)')
cursor.executemany('INSERT INTO numeros VALUES (?)', [(10,), (20,), (30,), (40,)])

# Função: logaritmo natural
conn.create_function('ln', 1, math.log)
cursor.execute('SELECT valor, ln(valor) FROM numeros')
for row in cursor.fetchall():
    print(f'ln({row[0]}) = {row[1]:.2f}')

# Agregado: soma dos quadrados
class SomaQuadrados:
    def __init__(self):
        self.soma = 0
    def step(self, valor):
        if valor is not None:
            self.soma += valor * valor
    def finalize(self):
        return self.soma

conn.create_aggregate('soma_quadrados', 1, SomaQuadrados)
cursor.execute('SELECT soma_quadrados(valor) FROM numeros')
print('Soma dos quadrados:', cursor.fetchone()[0])

conn.close()
```

### Exemplo 3: Backup com progresso

```python
import sqlite3
import time

# Cria banco de origem com dados
origem = sqlite3.connect('origem.db')
origem.execute('CREATE TABLE IF NOT EXISTS teste (id INTEGER, valor TEXT)')
dados = [(i, f'Item {i}') for i in range(10000)]
origem.executemany('INSERT INTO teste VALUES (?, ?)', dados)
origem.commit()

# Conecta ao destino (em memória)
destino = sqlite3.connect(':memory:')

def progress(status, remaining, total):
    print(f'Progresso: {total-remaining}/{total} páginas', end='\r')

print('Iniciando backup...')
origem.backup(destino, pages=100, progress=progress)
print('\nBackup concluído!')

# Verifica contagem no destino
count = destino.execute('SELECT COUNT(*) FROM teste').fetchone()[0]
print(f'Registros copiados: {count}')

origem.close()
destino.close()
```
