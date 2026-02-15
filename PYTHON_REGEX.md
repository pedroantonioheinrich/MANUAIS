# Manual Completo de Expressões Regulares (Regex) em Python

## Índice
1.  [Introdução: O Poder da Magia Textual](#introdução)
2.  [Preparando o Terreno: O Módulo `re` e Strings Raw (`r"..."`)](#preparando-o-terreno)
    - [Aplicação Real: Configuração Inicial de um Projeto](#aplicação-real-configuração-inicial)
3.  [A Base: Caracteres Literais](#a-base-caracteres-literais)
    - [Aplicação Real: Busca por uma Palavra-chave](#aplicação-real-busca-por-palavra-chave)
4.  [Os Metacaracteres: Os Atores Principais](#os-metacaracteres)
    - [4.1. O Coringa: O Ponto (`.`)](#41-o-coringa-o-ponto-)
        - [Aplicação Real: Extraindo Código entre Tags](#aplicação-real-extraindo-código-entre-tags)
    - [4.2. Classes de Caracteres: `[abc]` e `[^abc]`](#42-classes-de-caracteres-abc-e-abc)
        - [Aplicação Real: Validando Horas em um Log](#aplicação-real-validando-horas-em-um-log)
    - [4.3. Atalhos Poderosos: `\d`, `\w`, `\s` e seus Contrários](#43-atalhos-poderosos-\d-\w-\s-e-seus-contrários)
        - [Aplicação Real: Extraindo Dados de um Arquivo CSV](#aplicação-real-extraindo-dados-de-um-arquivo-csv)
    - [4.4. Âncoras: `^` e `$` (Onde a Mágica Acontece)](#44-âncoras--e--onde-a-mágica-acontece)
        - [Aplicação Real: Validando um Arquivo de Configuração](#aplicação-real-validando-um-arquivo-de-configuração)
5.  [Quantificadores: A Arte da Repetição](#quantificadores)
    - [5.1. Repetição Genérica: `*`, `+`, `?`, `{m,n}`](#51-repetição-genérica---)
        - [Aplicação Real: Extraindo Números de Telefone](#aplicação-real-extraindo-números-de-telefone)
    - [5.2. O Pecado da Ganância e a Redenção do Não-Codicioso (`*?`, `+?`)](#52-o-pecado-da-ganância-e-a-redenção-do-não-codicioso--)
        - [Aplicação Real: Parsing Seguro de HTML](#aplicação-real-parsing-seguro-de-html)
6.  [Grupos: A Estrutura da Informação](#grupos)
    - [6.1. Grupos de Captura `(...)` e Backreferences](#61-grupos-de-captura--e-backreferences)
        - [Aplicação Real: Encontrando Palavras Repetidas](#aplicação-real-encontrando-palavras-repetidas)
    - [6.2. Grupos Não-Capturantes `(?:...)`](#62-grupos-não-capturantes-)
    - [6.3. Grupos com Nome `(?P<nome>...)`](#63-grupos-com-nome-)
        - [Aplicação Real: Extraindo Dados de um Log Estruturado](#aplicação-real-extraindo-dados-de-um-log-estruturado)
7.  [Lookahead e Lookbehind: O Poder da Premonição](#lookahead-e-lookbehind)
    - [7.1. Positive Lookahead (`(?=...)`)](#71-positive-lookahead-)
    - [7.2. Negative Lookahead (`(?!...)`)](#72-negative-lookahead-)
        - [Aplicação Real: Encontrando Palavras Não Seguidas por Algo](#aplicação-real-encontrando-palavras-não-seguidas-por-algo)
8.  [As Ferramentas: Funções do Módulo `re`](#as-ferramentas-funções-do-módulo-re)
    - [8.1. `re.match()` vs `re.search()`](#81-rematch-vs-research)
        - [Aplicação Real: Validando um Protocolo de URL](#aplicação-real-validando-um-protocolo-de-url)
    - [8.2. `re.findall()` e `re.finditer()`](#82-refindall-e-refinditer)
        - [Aplicação Real: Extraindo Todos os E-mails de um Texto](#aplicação-real-extraindo-todos-os-e-mails-de-um-texto)
    - [8.3. `re.sub()`: Buscar e Substituir](#83-resub-buscar-e-substituir)
        - [Aplicação Real: Anonimizando Dados](#aplicação-real-anonimizando-dados)
    - [8.4. `re.split()`: Dividir com Poder](#84-resplit-dividir-com-poder)
        - [Aplicação Real: Tokenização Inteligente](#aplicação-real-tokenização-inteligente)
    - [8.5. `re.compile()`: Performance e Reutilização](#85-recompile-performance-e-reutilização)
9.  [Bandeiras (Flags): Modificadores de Comportamento](#bandeiras-flags-modificadores-de-comportamento)
    - [Aplicação Real: Busca Case-Insensitive](#aplicação-real-busca-case-insensitive)
10. [Conclusão](#conclusão)

---

## Introdução

Imagine que você precisa encontrar todos os números de telefone em um documento de mil páginas. Ou validar se cada e-mail cadastrado em um formulário segue um formato válido. Ou, ainda, extrair todas as datas de uma base de dados e reformatá-las.

Fazer isso manualmente é impossível. Fazer com funções string comuns, como `if "2023" in linha`, é ineficiente e quebrável. É aí que entram as **Expressões Regulares (Regex)**.

Regex é uma **mini-linguagem de programação** especializada em padrões textuais . Pense nela como um "CTRL+F" turbinado, capaz de entender conceitos como "começa com", "é um dígito", "tem entre 2 e 5 letras" ou "é um endereço de e-mail".

Neste manual, vamos explorar o módulo `re` do Python, não apenas listando comandos, mas explicando a lógica por trás de cada operação.

## Preparando o Terreno: O Módulo `re` e Strings Raw (`r"..."`)

Tudo começa com a importação do módulo nativo:
```python
import re
```

**O Mistério da String Raw (`r"..."`)**

Você verá em todo código regex a letra `r` antes das aspas. Isso não é um capricho, é uma necessidade.
```python
padrao_certo = r"\d"  # Uma string raw
padrao_errado = "\\d" # Uma string comum
```

**Por que?** Na string comum do Python, a barra invertida (`\`) é um caractere de escape. `\n` vira uma nova linha, `\t` vira um tab. Quando você cria uma regex, `\d` significa "um dígito". Se você usar uma string comum, o Python "pré-processa" o `\d` antes de enviá-lo ao motor de regex, achando que é um código de escape inválido. A **string raw (`r`)** diz ao Python: "pegue essa string exatamente como está, sem interpretar as barras invertidas". Isso salva sua sanidade e evita o uso de barras duplas (`\\d`) .

### Aplicação Real: Configuração Inicial
```python
import re

texto_para_analisar = "O contato é exemplo@teste.com ou suporte@empresa.net"
# Usamos raw string para definir o padrão
padrao_email = r"[\w.]+@[\w.]+\.\w+"
```

## A Base: Caracteres Literais

O conceito mais simples é a correspondência literal. Se você colocar a palavra "Python" em uma regex, ela casará exatamente com a sequência "Python" em um texto. É o modo "ingênuo" de busca.

### Aplicação Real: Busca por uma Palavra-chave
```python
texto = "A linguagem Python é ótima para Data Science. Eu amo Python!"
resultado = re.findall(r"Python", texto)
print(resultado) # Saída: ['Python', 'Python']
```

## Os Metacaracteres: Os Atores Principais

Aqui a coisa fica interessante. Metacaracteres são símbolos que **não** representam a si mesmos, mas sim uma ação ou tipo de caractere . Os principais são:
`. ^ $ * + ? { } [ ] \ | ( )`

### 4.1. O Coringa: O Ponto (`.`)
O ponto é o "coringa". Ele casa com **qualquer caractere, exceto uma nova linha** .

- **Regex:** `c.e`
- **Exemplos que casam:** "ce", "cafe" (o ponto casa com o 'a'), "c e" (o ponto casa com o espaço), "c9e", "c&e".
- **Exemplos que não casam:** "ce" (precisa de um caractere entre o c e o e).

#### Aplicação Real: Extraindo Código entre Tags
```python
html = "<h1>Título Principal</h1>"
# Queremos extrair o conteúdo dentro de qualquer tag de nível 1
padrao = r"<h1>.*</h1>" 
# O . casa com 'T', 'í', 't', 'u'... e o * (que veremos depois) repete isso.
resultado = re.search(padrao, html)
print(resultado.group()) # Saída: <h1>Título Principal</h1>
```

### 4.2. Classes de Caracteres: `[abc]` e `[^abc]`
Quando você não quer "qualquer" caractere, mas sim um "específico de um grupo", usa-se as classes .

- `[abc]`: casa com **um** caractere que seja OU 'a', OU 'b', OU 'c'.
- `[a-z]`: casa com qualquer letra minúscula (o hífen define um intervalo).
- `[0-9]`: casa com um dígito.
- `[a-zA-Z0-9_]`: casa com qualquer caractere alfanumérico (letras, números e underline).
- `[^abc]`: o `^` no **início da classe** significa negação. Casa com qualquer caractere que **não** seja a, b ou c .

#### Aplicação Real: Validando Horas em um Log
Imagine que você quer encontrar linhas de log que comecem com um horário válido entre 00:00 e 23:59.
```python
log = "23:59 - Serviço iniciado"
# O primeiro dígito da hora pode ser [0-2], o segundo depende do primeiro.
# Vamos simplificar: [0-2][0-9]:[0-5][0-9]
padrao_hora = r"[0-2][0-9]:[0-5][0-9]"
resultado = re.search(padrao_hora, log)
print(resultado.group()) # Saída: 23:59
```

### 4.3. Atalhos Poderosos: `\d`, `\w`, `\s` e seus Contrários
Para os conjuntos mais comuns, existem atalhos :
-   `\d` (digit): Equivalente a `[0-9]`.
-   `\w` (word char): Equivalente a `[a-zA-Z0-9_]` (letras, números e underline).
-   `\s` (space): Equivalente a qualquer espaço em branco (`[ \t\n\r\f\v]`).

E suas versões "negadas" (maiúsculas):
-   `\D`: Qualquer coisa que **não** seja dígito.
-   `\W`: Qualquer coisa que **não** seja caractere de palavra.
-   `\S`: Qualquer coisa que **não** seja espaço em branco.

#### Aplicação Real: Extraindo Dados de um Arquivo CSV
```python
linha_csv = "João,30,Analista,2023-10-05"
# Queremos extrair nome, idade e cargo
padrao = r"(\w+),(\d+),(\w+),(\d{4}-\d{2}-\d{2})"
# \d{4} significa "exatamente 4 dígitos" (veremos em quantificadores)
resultado = re.search(padrao, linha_csv)
print(f"Nome: {resultado.group(1)}, Idade: {resultado.group(2)}, Cargo: {resultado.group(3)}")
```

### 4.4. Âncoras: `^` e `$` (Onde a Mágica Acontece)
Âncoras não casam com caracteres, casam com **posições** no texto .
-   `^`: Casa com o início da string (ou linha, com a flag `re.MULTILINE`).
-   `$`: Casa com o final da string (ou linha, com a flag `re.MULTILINE`).

#### Aplicação Real: Validando um Arquivo de Configuração
Imagine um arquivo onde linhas válidas de configuração não podem estar vazias e não podem começar com '#' (comentário).
```python
linhas = [
    "# Isto é um comentário",
    "SERVER_PORT=8080",
    "",
    "DEBUG=True"
]
for linha in linhas:
    # A regex casa se a linha NÃO começar com '#' E NÃO for vazia
    if re.search(r"^(?!#)\s*\S+", linha):
        print(f"Config válida: {linha}")
# Explicação: ^ (início), (?#...) é um lookahead negativo (vemos depois) que falha se encontrar #
# \s* (espaços opcionais) e \S+ (um ou mais não-espaços) garante que não seja linha vazia.
```

## Quantificadores: A Arte da Repetição

Até agora, tudo era "um" caractere. Como dizer "um ou mais" ou "de 3 a 5"? Com quantificadores .

### 5.1. Repetição Genérica: `*`, `+`, `?`, `{m,n}`
-   `*`: Zero ou mais vezes (equivalente a `{0,}`). `a*b` casa com "b", "ab", "aaab".
-   `+`: Uma ou mais vezes (equivalente a `{1,}`). `a+b` casa com "ab", "aaab", mas **não** casa com "b".
-   `?`: Zero ou uma vez (equivalente a `{0,1}`). `a?b` casa com "b" e "ab".
-   `{m,n}`: De `m` a `n` vezes. `a{2,4}` casa com "aa", "aaa", "aaaa". `{m,}` no mínimo m, sem máximo. `{,n}` no máximo n.

#### Aplicação Real: Extraindo Números de Telefone
```python
texto = "Ligue para 21-1234-5678 ou 11-98765-4321"
# Padrão: 2 dígitos, hífen, 4 ou 5 dígitos, hífen, 4 dígitos
padrao_tel = r"\d{2}-\d{4,5}-\d{4}"
print(re.findall(padrao_tel, texto)) # ['21-1234-5678', '11-98765-4321']
```

### 5.2. O Pecado da Ganância e a Redenção do Não-Codicioso (`*?`, `+?`)
Por padrão, os quantificadores são **gananciosos (greedy)**. Eles tentam casar com a maior sequência possível .

- **Regex gananciosa:** `<.*>` na string `<h1>Título</h1>` casa com a string **inteira** `<h1>Título</h1>`. O `.*` "consome" tudo até o último `>`.
- **Regex não-gananciosa (lazy):** `<.*?>` casa com `<h1>` apenas. O `?` após o quantificador diz: "pegue a **menor** sequência possível".

#### Aplicação Real: Parsing Seguro de HTML
```python
html = "<p>Parágrafo 1</p><p>Parágrafo 2</p>"

# Modo ganancioso (errado)
print(re.findall(r"<p>.*</p>", html))
# Saída: ['<p>Parágrafo 1</p><p>Parágrafo 2</p>']

# Modo não-ganancioso (correto)
print(re.findall(r"<p>.*?</p>", html))
# Saída: ['<p>Parágrafo 1</p>', '<p>Parágrafo 2</p>']
```
Isso evita que você capture mais do que deveria.

## Grupos: A Estrutura da Informação

Parênteses `( )` têm dois papéis principais: agrupar partes da regex e **capturar** o conteúdo casado para uso posterior .

### 6.1. Grupos de Captura `(...)` e Backreferences
Quando você usa parênteses, o trecho casado é armazenado. Você pode acessá-lo com `match.group(1)` (para o primeiro grupo), `match.group(2)`, etc. O grupo `0` é sempre a correspondência completa.

**Backreference** é a habilidade de referenciar um grupo capturado **dentro da própria regex**, usando `\1`, `\2`, etc.

#### Aplicação Real: Encontrando Palavras Repetidas
```python
texto = "O gato gato pulou."
# Padrão: uma palavra (\w+) seguida de espaço e a mesma palavra novamente (\1)
padrao_repeticao = r"(\b\w+\b)\s+\1" # \b é "fronteira de palavra" (word boundary)
resultado = re.search(padrao_repeticao, texto)
print(resultado.group()) # Saída: gato gato
```

### 6.2. Grupos Não-Capturantes `(?:...)`
Se você precisa de parênteses para aplicar um quantificador a um grupo, mas não quer "guardar" aquele conteúdo na memória, use `(?:...)`. É mais eficiente .
```python
padrao = r"(?:\d{2}-)?\d{4,5}-\d{4}"
# O grupo (?:...)? torna o código de área (2 dígitos e hífen) opcional, mas não o captura.
```

### 6.3. Grupos com Nome `(?P<nome>...)`
Para não ficar contando grupos (grupo 1, grupo 2...), você pode dar nomes a eles. Isso torna o código mais legível .

#### Aplicação Real: Extraindo Dados de um Log Estruturado
```python
log = "2023-10-05 14:30:00,ERROR,Usuário 123 não encontrado"
padrao = r"(?P<data>\d{4}-\d{2}-\d{2}) (?P<hora>\d{2}:\d{2}:\d{2}),(?P<nivel>\w+),(?P<mensagem>.+)"
resultado = re.search(padrao, log)
print(resultado.group('data'))  # Saída: 2023-10-05
print(resultado.group('nivel')) # Saída: ERROR
print(resultado.groupdict())    # {'data': '2023-10-05', 'hora': '14:30:00', 'nivel': 'ERROR', 'mensagem': 'Usuário 123 não encontrado'}
```

## Lookahead e Lookbehind: O Poder da Premonição

Estes são **assertions de largura zero**. Eles não consomem caracteres. Eles apenas verificam se o que está à frente (`(?=...)`) ou atrás (`(?<=...)`) corresponde a um padrão, sem incluir essa verificação no resultado final .

### 7.1. Positive Lookahead (`(?=...)`)
Verifica se o padrão **à frente** casa.
- `\d+(?= reais)`: Casa com um ou mais dígitos que **estejam seguidos** da palavra " reais". Em "Gastei 100 reais e 50 dólares", casará com "100", mas não com "50".

### 7.2. Negative Lookahead (`(?!...)`)
Verifica se o padrão **à frente não** casa.
- `\d+(?! reais)`: Casa com dígitos que **não estejam seguidos** de " reais". No exemplo acima, casará com "50".

#### Aplicação Real: Encontrando Palavras Não Seguidas por Algo
```python
texto = "Eu como maçãs e laranjas. Maçãs são doces."
# Queremos a palavra "maçãs" que NÃO seja seguida de " são"
padrao = r"maçãs(?! são)"
print(re.findall(padrao, texto, re.IGNORECASE)) # Saída: ['maçãs'] (a primeira ocorrência, a segunda é ignorada)
```

## As Ferramentas: Funções do Módulo `re`

### 8.1. `re.match()` vs `re.search()`
-   `re.match()`: Só verifica se o padrão casa no **início da string** .
-   `re.search()`: Varre a string inteira procurando pela primeira posição onde o padrão casa .

#### Aplicação Real: Validando um Protocolo de URL
```python
url = "https://site.com"
# match é ideal para isso, pois o protocolo deve vir no início
if re.match(r"https?://", url):
    print("URL segura (http ou https)")

texto = "Meu site é https://site.com"
# search encontra no meio da frase
print(re.search(r"https?://", texto))
```

### 8.2. `re.findall()` e `re.finditer()`
-   `findall()`: Retorna uma **lista** com todas as correspondências não sobrepostas .
-   `finditer()`: Retorna um **iterador** com objetos `match` (útil para correspondências grandes ou com muitos grupos) .

#### Aplicação Real: Extraindo Todos os E-mails de um Texto
```python
texto = "Contato1: joao@email.com, Contato2: maria@email.org.br"
padrao_email = r"\b[\w.-]+@[\w.-]+\.\w+\b" # Simplificado
emails = re.findall(padrao_email, texto)
print(emails) # ['joao@email.com', 'maria@email.org.br']
```

### 8.3. `re.sub()`: Buscar e Substituir
Poderosíssima para substituir padrões. Você pode usar grupos capturados na string de substituição com `\1`, `\g<1>` ou `\g<nome>` .

#### Aplicação Real: Anonimizando Dados
```python
texto = "O número do cliente é 123.456.789-00."
# Substitui os 3 primeiros dígitos de cada bloco por XXX
cpf_mascarado = re.sub(r"(\d{3})\.(\d{3})\.(\d{3})-(\d{2})", r"XXX.XXX.\3-\4", texto)
print(cpf_mascarado) # O número do cliente é XXX.XXX.789-00.
```

### 8.4. `re.split()`: Dividir com Poder
Funciona como o `split()` de strings, mas permite um padrão complexo como separador .

#### Aplicação Real: Tokenização Inteligente
```python
texto = "Palavras, separadas  por  espaços    e pontuação."
# Divide por qualquer sequência de não-palavras (tudo que não for letra/número)
palavras = re.split(r"\W+", texto)
print(palavras) # ['Palavras', 'separadas', 'por', 'espaços', 'e', 'pontuação', '']
```

### 8.5. `re.compile()`: Performance e Reutilização
Se você vai usar a mesma regex muitas vezes, compile-a. Isso pré-compila o padrão em um objeto, tornando as operações futuras mais rápidas .
```python
padrao_compilado = re.compile(r"\b\d{5}\b") # CEPs de 5 dígitos
ceps1 = padrao_compilado.findall("CEP 12345 e 67890")
ceps2 = padrao_compilado.findall("Outro CEP 54321")
print(ceps1, ceps2)
```

## Bandeiras (Flags): Modificadores de Comportamento

Flags modificam como a regex é interpretada .
-   `re.IGNORECASE` ou `re.I`: Ignora maiúsculas/minúsculas.
-   `re.MULTILINE` ou `re.M`: Faz `^` e `$` casarem com o início/fim de cada linha, não só da string.
-   `re.DOTALL` ou `re.S`: Faz o ponto (`.`) casar também com novas linhas.

### Aplicação Real: Busca Case-Insensitive
```python
texto = "python é legal. PYTHON é demais!"
print(re.findall(r"python", texto, re.IGNORECASE)) # ['python', 'PYTHON']
```

