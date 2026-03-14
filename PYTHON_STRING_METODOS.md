# Manual Completo de Métodos de String em Python

As strings são um dos tipos de dados mais utilizados em Python. Uma string é uma sequência imutável de caracteres Unicode. Por serem imutáveis, qualquer operação que modifique uma string retorna uma nova string, sem alterar a original.

Python fornece uma vasta gama de métodos embutidos para manipular strings. Este manual cobre todos os métodos disponíveis para objetos do tipo `str` na versão Python 3.11, explicando detalhadamente cada um, com sintaxe, parâmetros, valor de retorno e exemplos práticos.

---

## Índice

1. [Métodos de Capitalização e Formatação de Caixa](#1-métodos-de-capitalização-e-formatação-de-caixa)
   - `capitalize()`
   - `casefold()`
   - `lower()`
   - `upper()`
   - `swapcase()`
   - `title()`
2. [Métodos de Busca e Verificação](#2-métodos-de-busca-e-verificação)
   - `count()`
   - `find()`
   - `rfind()`
   - `index()`
   - `rindex()`
   - `startswith()`
   - `endswith()`
3. [Métodos de Classificação de Caracteres](#3-métodos-de-classificação-de-caracteres)
   - `isalnum()`
   - `isalpha()`
   - `isascii()`
   - `isdecimal()`
   - `isdigit()`
   - `isidentifier()`
   - `islower()`
   - `isnumeric()`
   - `isprintable()`
   - `isspace()`
   - `istitle()`
   - `isupper()`
4. [Métodos de Junção, Separação e Quebra](#4-métodos-de-junção-separação-e-quebra)
   - `join()`
   - `split()`
   - `rsplit()`
   - `splitlines()`
   - `partition()`
   - `rpartition()`
5. [Métodos de Substituição e Remoção](#5-métodos-de-substituição-e-remoção)
   - `replace()`
   - `strip()`
   - `lstrip()`
   - `rstrip()`
   - `removeprefix()`
   - `removesuffix()`
6. [Métodos de Alinhamento e Preenchimento](#6-métodos-de-alinhamento-e-preenchimento)
   - `center()`
   - `ljust()`
   - `rjust()`
   - `zfill()`
   - `expandtabs()`
7. [Métodos de Codificação](#7-métodos-de-codificação)
   - `encode()`
8. [Tabela Resumo](#tabela-resumo)

---

## 1. Métodos de Capitalização e Formatação de Caixa

### `capitalize()`

**Sintaxe:** `string.capitalize()`

**Descrição:** Retorna uma cópia da string com o primeiro caractere em maiúsculo e os demais em minúsculo. Caracteres não alfabéticos no início são mantidos, mas não afetam a capitalização.

**Parâmetros:** Nenhum.

**Retorno:** Nova string.

**Exemplo:**
```python
texto = "python é LEGAL!"
print(texto.capitalize())  # Saída: "Python é legal!"
```

**Observações:** Apenas o primeiro caractere é afetado; os demais são convertidos para minúsculo, independentemente de serem letras ou não.

---

### `casefold()`

**Sintaxe:** `string.casefold()`

**Descrição:** Retorna uma versão da string adequada para comparações sem distinção de maiúsculas/minúsculas, especialmente para caracteres Unicode. É mais agressivo que `lower()` e remove distinções específicas de idioma (como o "ß" alemão que se torna "ss").

**Parâmetros:** Nenhum.

**Retorno:** Nova string "casefolded".

**Exemplo:**
```python
texto = "Straße"
print(texto.casefold())  # Saída: "strasse"
print(texto.lower())     # Saída: "straße"  (menos agressivo)
```

**Observações:** Prefira `casefold()` para comparações case-insensitive quando precisar de suporte a Unicode completo.

---

### `lower()`

**Sintaxe:** `string.lower()`

**Descrição:** Retorna uma cópia da string com todos os caracteres alfabéticos convertidos para minúsculo.

**Parâmetros:** Nenhum.

**Retorno:** Nova string em minúsculas.

**Exemplo:**
```python
texto = "Olá, Mundo!"
print(texto.lower())  # Saída: "olá, mundo!"
```

---

### `upper()`

**Sintaxe:** `string.upper()`

**Descrição:** Retorna uma cópia da string com todos os caracteres alfabéticos convertidos para maiúsculo.

**Parâmetros:** Nenhum.

**Retorno:** Nova string em maiúsculas.

**Exemplo:**
```python
texto = "Olá, Mundo!"
print(texto.upper())  # Saída: "OLÁ, MUNDO!"
```

---

### `swapcase()`

**Sintaxe:** `string.swapcase()`

**Descrição:** Retorna uma cópia da string com maiúsculas convertidas para minúsculas e vice‑versa.

**Parâmetros:** Nenhum.

**Retorno:** Nova string com caixa invertida.

**Exemplo:**
```python
texto = "Python 3.11"
print(texto.swapcase())  # Saída: "pYTHON 3.11"
```

**Observações:** Caracteres não alfabéticos permanecem inalterados.

---

### `title()`

**Sintaxe:** `string.title()`

**Descrição:** Retorna uma versão da string onde cada palavra começa com maiúscula e o restante em minúscula. A definição de "palavra" é uma sequência de caracteres alfabéticos, e os limites são definidos por qualquer caractere não alfabético.

**Parâmetros:** Nenhum.

**Retorno:** Nova string em formato de título.

**Exemplo:**
```python
texto = "python é incrível!"
print(texto.title())  # Saída: "Python É Incrível!"
```

**Observações:** Pode produzir resultados inesperados com apóstrofos ou contrações (ex.: "don't" vira "Don'T"). Para maior controle, pode-se usar expressões regulares ou outras abordagens.

---

## 2. Métodos de Busca e Verificação

### `count()`

**Sintaxe:** `string.count(sub[, start[, end]])`

**Descrição:** Retorna o número de ocorrências não sobrepostas da substring `sub` na string. A busca pode ser restrita a um intervalo usando `start` e `end` (índices baseados em 0, com `end` exclusivo).

**Parâmetros:**
- `sub` (obrigatório): substring a ser contada.
- `start` (opcional): índice inicial da busca.
- `end` (opcional): índice final (exclusivo) da busca.

**Retorno:** Inteiro com a contagem.

**Exemplo:**
```python
texto = "abracadabra"
print(texto.count("abra"))      # Saída: 2
print(texto.count("a", 0, 5))   # Saída: 2 (posições 0 a 4)
```

---

### `find()`

**Sintaxe:** `string.find(sub[, start[, end]])`

**Descrição:** Retorna o menor índice onde a substring `sub` é encontrada. Se `sub` não for encontrada, retorna `-1`. A busca pode ser limitada por `start` e `end`.

**Parâmetros:** similares a `count()`.

**Retorno:** Inteiro com a posição ou `-1`.

**Exemplo:**
```python
texto = "Hello, world!"
print(texto.find("world"))   # Saída: 7
print(texto.find("Python"))  # Saída: -1
```

---

### `rfind()`

**Sintaxe:** `string.rfind(sub[, start[, end]])`

**Descrição:** Similar a `find()`, mas retorna o maior índice (última ocorrência) onde `sub` é encontrada.

**Exemplo:**
```python
texto = "abracadabra"
print(texto.rfind("abra"))   # Saída: 7 (última ocorrência)
```

---

### `index()`

**Sintaxe:** `string.index(sub[, start[, end]])`

**Descrição:** Funciona como `find()`, porém levanta uma exceção `ValueError` se a substring não for encontrada.

**Exemplo:**
```python
texto = "Hello, world!"
try:
    pos = texto.index("Python")
except ValueError:
    print("Substring não encontrada")
```

---

### `rindex()`

**Sintaxe:** `string.rindex(sub[, start[, end]])`

**Descrição:** Versão à direita de `index()`. Retorna o maior índice ou levanta `ValueError`.

---

### `startswith()`

**Sintaxe:** `string.startswith(prefix[, start[, end]])`

**Descrição:** Retorna `True` se a string começar com o prefixo especificado. O prefixo pode ser uma string ou uma tupla de strings (verifica se algum dos prefixos corresponde).

**Parâmetros:**
- `prefix`: string ou tupla de strings a verificar.
- `start` (opcional): início da fatia.
- `end` (opcional): fim da fatia.

**Retorno:** Booleano.

**Exemplo:**
```python
arquivo = "documento.pdf"
print(arquivo.startswith("doc"))        # True
print(arquivo.startswith(("img", "doc"))) # True (um deles)
```

---

### `endswith()`

**Sintaxe:** `string.endswith(suffix[, start[, end]])`

**Descrição:** Retorna `True` se a string terminar com o sufixo especificado. Aceita tupla.

**Exemplo:**
```python
arquivo = "foto.jpg"
print(arquivo.endswith((".png", ".jpg"))) # True
```

---

## 3. Métodos de Classificação de Caracteres

### `isalnum()`

**Sintaxe:** `string.isalnum()`

**Descrição:** Retorna `True` se todos os caracteres da string forem alfanuméricos (letras ou números) e a string não estiver vazia. Caracteres como espaços, pontuação e símbolos retornam `False`.

**Exemplo:**
```python
print("Python3".isalnum())   # True
print("Python 3".isalnum())  # False (espaço)
```

---

### `isalpha()`

**Sintaxe:** `string.isalpha()`

**Descrição:** Retorna `True` se todos os caracteres forem letras (incluindo caracteres de alfabetos não latinos) e a string não estiver vazia.

**Exemplo:**
```python
print("Olá".isalpha())    # True
print("Olá123".isalpha()) # False
```

---

### `isascii()`

**Sintaxe:** `string.isascii()`

**Descrição:** Retorna `True` se todos os caracteres da string forem ASCII (códigos de 0 a 127). Se a string estiver vazia, também retorna `True`.

**Exemplo:**
```python
print("abc123".isascii())   # True
print("Olá".isascii())      # False ('á' não é ASCII)
```

---

### `isdecimal()`

**Sintaxe:** `string.isdecimal()`

**Descrição:** Retorna `True` se todos os caracteres forem dígitos decimais (aqueles usados para formar números na base 10, como os dígitos de 0 a 9 e outros sistemas decimais Unicode). A string não pode estar vazia.

**Exemplo:**
```python
print("123".isdecimal())    # True
print("²".isdecimal())       # False (sobrescrito não é decimal)
print("½".isdecimal())       # False
```

---

### `isdigit()`

**Sintaxe:** `string.isdigit()`

**Descrição:** Similar a `isdecimal()`, mas inclui dígitos de outras escritas que representam números, como dígitos em devanágari e sobrescritos como "²". Ainda assim, exclui frações e numerais que não sejam dígitos.

**Exemplo:**
```python
print("123".isdigit())    # True
print("²".isdigit())      # True (sobrescrito é dígito)
print("½".isdigit())      # False
```

---

### `isidentifier()`

**Sintaxe:** `string.isidentifier()`

**Descrição:** Retorna `True` se a string for um identificador válido em Python, ou seja, se puder ser usado como nome de variável, função, classe, etc. Regras: começa com letra ou underscore, seguido de letras, underscores ou dígitos. Palavras‑chave (`if`, `for`, etc.) não são identificadores válidos por este método (palavras‑chave são tratadas separadamente).

**Exemplo:**
```python
print("variavel".isidentifier())   # True
print("2var".isidentifier())       # False (começa com dígito)
print("def".isidentifier())        # True (embora seja keyword, é identificador)
```

---

### `islower()`

**Sintaxe:** `string.islower()`

**Descrição:** Retorna `True` se houver pelo menos um caractere alfabético e todos os caracteres alfabéticos estiverem em minúsculo. Caracteres não alfabéticos são ignorados.

**Exemplo:**
```python
print("hello".islower())      # True
print("Hello".islower())      # False
print("hello123!".islower())  # True (só letras minúsculas)
```

---

### `isnumeric()`

**Sintaxe:** `string.isnumeric()`

**Descrição:** Retorna `True` se todos os caracteres forem numéricos, incluindo dígitos, frações (½), numerais romanos, caracteres de outras escritas que representam números, etc. A string não pode estar vazia.

**Exemplo:**
```python
print("123".isnumeric())     # True
print("½".isnumeric())       # True
print("²".isnumeric())       # True
print("10²".isnumeric())     # False (mistura letra? "²" é numérico, mas "10" também, depende do contexto: "10²" contém '1','0','²' todos numéricos? Sim, são numéricos, então True)
# Na verdade, "10²" é composto por '1','0','²' – todos numéricos, então isnumeric() retorna True.
```

**Observação:** `isnumeric()` é mais abrangente que `isdigit()`.

---

### `isprintable()`

**Sintaxe:** `string.isprintable()`

**Descrição:** Retorna `True` se todos os caracteres da string forem considerados imprimíveis ou a string estiver vazia. Caracteres de controle como `\n`, `\t`, `\r` não são imprimíveis.

**Exemplo:**
```python
print("Hello".isprintable())   # True
print("Hello\n".isprintable()) # False
```

---

### `isspace()`

**Sintaxe:** `string.isspace()`

**Descrição:** Retorna `True` se a string contiver apenas caracteres de espaço em branco (espaço, tabulação, nova linha, etc.) e não estiver vazia.

**Exemplo:**
```python
print("   ".isspace())   # True
print(" \t\n".isspace()) # True
print(" a ".isspace())   # False
```

---

### `istitle()`

**Sintaxe:** `string.istitle()`

**Descrição:** Retorna `True` se a string estiver no formato de título (ou seja, cada palavra começa com maiúscula e o restante em minúscula) e há pelo menos um caractere alfabético. A definição de palavra é a mesma de `title()`.

**Exemplo:**
```python
print("Python É Incrível".istitle()) # True
print("Python é Incrível".istitle()) # False (a palavra "é" começa minúscula)
```

---

### `isupper()`

**Sintaxe:** `string.isupper()`

**Descrição:** Retorna `True` se houver pelo menos um caractere alfabético e todos os caracteres alfabéticos estiverem em maiúsculo.

**Exemplo:**
```python
print("HELLO".isupper())   # True
print("Hello".isupper())   # False
print("HELLO123!".isupper()) # True
```

---

## 4. Métodos de Junção, Separação e Quebra

### `join()`

**Sintaxe:** `separator.join(iterable)`

**Descrição:** Concatena os elementos de um iterável (como lista, tupla) em uma única string, utilizando a string que chama o método como separador entre cada elemento. Os elementos do iterável devem ser strings.

**Parâmetros:**
- `iterable`: qualquer iterável cujos elementos são strings.

**Retorno:** Nova string.

**Exemplo:**
```python
palavras = ["Python", "é", "legal"]
print(" ".join(palavras))   # Saída: "Python é legal"
print("-".join(palavras))   # Saída: "Python-é-legal"
```

---

### `split()`

**Sintaxe:** `string.split(sep=None, maxsplit=-1)`

**Descrição:** Divide a string em uma lista de substrings, usando `sep` como delimitador. Se `sep` não for fornecido ou for `None`, qualquer sequência de espaços em branco é usada como separador, e espaços iniciais/finais são ignorados.

**Parâmetros:**
- `sep` (opcional): string delimitadora.
- `maxsplit` (opcional): número máximo de divisões. Se negativo, não há limite.

**Retorno:** Lista de strings.

**Exemplo:**
```python
texto = "um,dois,três"
print(texto.split(","))        # ['um', 'dois', 'três']
texto2 = "  um  dois  três  "
print(texto2.split())          # ['um', 'dois', 'três'] (espaços ignorados)
print("um,dois,três".split(",", 1)) # ['um', 'dois,três']
```

---

### `rsplit()`

**Sintaxe:** `string.rsplit(sep=None, maxsplit=-1)`

**Descrição:** Similar a `split()`, mas começa a divisão pela direita. Útil quando se quer limitar a quantidade de divisões a partir do final.

**Exemplo:**
```python
caminho = "home/user/docs/file.txt"
print(caminho.rsplit("/", 1))  # ['home/user/docs', 'file.txt']
```

---

### `splitlines()`

**Sintaxe:** `string.splitlines([keepends=False])`

**Descrição:** Divide a string nas quebras de linha e retorna uma lista. Diferentes caracteres de quebra de linha (como `\n`, `\r\n`, `\r`, etc.) são reconhecidos.

**Parâmetros:**
- `keepends` (opcional): se `True`, as quebras de linha são mantidas no final das linhas resultantes.

**Retorno:** Lista de strings.

**Exemplo:**
```python
poema = "Linha1\nLinha2\r\nLinha3"
print(poema.splitlines())      # ['Linha1', 'Linha2', 'Linha3']
print(poema.splitlines(True))  # ['Linha1\n', 'Linha2\r\n', 'Linha3']
```

---

### `partition()`

**Sintaxe:** `string.partition(sep)`

**Descrição:** Procura a primeira ocorrência de `sep` na string e retorna uma tupla com três elementos: a parte antes do separador, o próprio separador e a parte depois. Se `sep` não for encontrado, retorna a string original seguida de duas strings vazias.

**Parâmetros:**
- `sep`: string separadora (não pode ser vazia).

**Retorno:** Tupla de três strings.

**Exemplo:**
```python
texto = "Python é incrível"
print(texto.partition("é"))   # ('Python ', 'é', ' incrível')
print(texto.partition("x"))   # ('Python é incrível', '', '')
```

---

### `rpartition()`

**Sintaxe:** `string.rpartition(sep)`

**Descrição:** Similar a `partition()`, mas procura a última ocorrência de `sep`.

**Exemplo:**
```python
url = "https://docs.python.org/3/library/stdtypes.html"
print(url.rpartition("/"))    # ('https://docs.python.org/3/library', '/', 'stdtypes.html')
```

---

## 5. Métodos de Substituição e Remoção

### `replace()`

**Sintaxe:** `string.replace(old, new[, count])`

**Descrição:** Retorna uma cópia da string com todas as ocorrências da substring `old` substituídas por `new`. Se `count` for fornecido, apenas as primeiras `count` ocorrências são substituídas.

**Parâmetros:**
- `old`: substring a ser substituída.
- `new`: substring substituta.
- `count` (opcional): número máximo de substituições.

**Retorno:** Nova string.

**Exemplo:**
```python
texto = "um dois um dois"
print(texto.replace("um", "1"))        # "1 dois 1 dois"
print(texto.replace("um", "1", 1))     # "1 dois um dois"
```

---

### `strip()`

**Sintaxe:** `string.strip([chars])`

**Descrição:** Remove caracteres do início e do fim da string. Se `chars` não for fornecido ou for `None`, remove espaços em branco. Se `chars` for fornecido, deve ser uma string e todos os caracteres nela contidos serão removidos das extremidades (até encontrar um caractere que não esteja em `chars`).

**Parâmetros:**
- `chars` (opcional): string contendo os caracteres a remover.

**Retorno:** Nova string.

**Exemplo:**
```python
texto = "   Olá, mundo!   "
print(texto.strip())          # "Olá, mundo!"
texto2 = "www.example.com"
print(texto2.strip("w.com"))  # "example" (remove 'w','.','c','o','m' das pontas)
```

---

### `lstrip()`

**Sintaxe:** `string.lstrip([chars])`

**Descrição:** Similar a `strip()`, mas remove apenas do início (esquerda).

**Exemplo:**
```python
texto = "   Olá   "
print(texto.lstrip())   # "Olá   "
```

---

### `rstrip()`

**Sintaxe:** `string.rstrip([chars])`

**Descrição:** Remove apenas do final (direita).

**Exemplo:**
```python
texto = "   Olá   "
print(texto.rstrip())   # "   Olá"
```

---

### `removeprefix()`

**Sintaxe:** `string.removeprefix(prefix)`

**Descrição:** Se a string começar com `prefix`, retorna uma cópia sem o prefixo. Caso contrário, retorna a string original. (Disponível a partir do Python 3.9.)

**Parâmetros:**
- `prefix`: string a ser removida do início.

**Retorno:** Nova string.

**Exemplo:**
```python
nome = "pre_arquivo.txt"
print(nome.removeprefix("pre_"))  # "arquivo.txt"
print(nome.removeprefix("pos_"))  # "pre_arquivo.txt" (inalterado)
```

---

### `removesuffix()`

**Sintaxe:** `string.removesuffix(suffix)`

**Descrição:** Se a string terminar com `suffix`, retorna uma cópia sem o sufixo. Caso contrário, retorna a string original. (Disponível a partir do Python 3.9.)

**Exemplo:**
```python
arquivo = "foto.jpg"
print(arquivo.removesuffix(".jpg"))  # "foto"
print(arquivo.removesuffix(".png"))  # "foto.jpg" (inalterado)
```

---

## 6. Métodos de Alinhamento e Preenchimento

### `center()`

**Sintaxe:** `string.center(width[, fillchar])`

**Descrição:** Centraliza a string em um campo de largura `width`, preenchendo com o caractere `fillchar` (padrão é espaço). Se `width` for menor ou igual ao comprimento da string, retorna a string original.

**Parâmetros:**
- `width`: largura total do campo.
- `fillchar` (opcional): caractere usado para preenchimento (deve ser um único caractere).

**Retorno:** Nova string.

**Exemplo:**
```python
titulo = "Python"
print(titulo.center(10, '*'))  # "**Python**"
print(titulo.center(5))        # "Python" (width menor)
```

---

### `ljust()`

**Sintaxe:** `string.ljust(width[, fillchar])`

**Descrição:** Justifica a string à esquerda em um campo de largura `width`, preenchendo à direita com `fillchar`. Se `width` for menor, retorna a string original.

**Exemplo:**
```python
nome = "Ana"
print(nome.ljust(10, '-'))  # "Ana-------"
```

---

### `rjust()`

**Sintaxe:** `string.rjust(width[, fillchar])`

**Descrição:** Justifica à direita, preenchendo à esquerda com `fillchar`.

**Exemplo:**
```python
numero = "42"
print(numero.rjust(5, '0'))  # "00042"
```

---

### `zfill()`

**Sintaxe:** `string.zfill(width)`

**Descrição:** Preenche a string com zeros à esquerda até atingir a largura `width`. Se a string começar com sinal (`+` ou `-`), os zeros são inseridos após o sinal. Útil para formatar números.

**Parâmetros:**
- `width`: largura desejada.

**Retorno:** Nova string.

**Exemplo:**
```python
print("42".zfill(5))     # "00042"
print("-42".zfill(5))    # "-0042"
print("+42".zfill(5))    # "+0042"
```

---

### `expandtabs()`

**Sintaxe:** `string.expandtabs(tabsize=8)`

**Descrição:** Substitui cada caractere de tabulação (`\t`) por um número de espaços, de forma que a próxima coluna seja múltipla de `tabsize`. Assume que a posição inicial é 0.

**Parâmetros:**
- `tabsize` (opcional): número de espaços por tabulação. Padrão 8.

**Retorno:** Nova string com tabs expandidos.

**Exemplo:**
```python
texto = "a\tb\tc"
print(texto.expandtabs(4))  # "a   b   c"
```

---

## 7. Métodos de Codificação

### `encode()`

**Sintaxe:** `string.encode(encoding='utf-8', errors='strict')`

**Descrição:** Retorna uma representação em bytes da string, codificada de acordo com o esquema de codificação especificado. É o inverso de `bytes.decode()`.

**Parâmetros:**
- `encoding`: nome da codificação (ex.: 'utf-8', 'ascii', 'latin1').
- `errors`: esquema de tratamento de erros. Valores comuns: 'strict' (levanta UnicodeEncodeError), 'ignore' (ignora caracteres não codificáveis), 'replace' (substitui por ?), 'xmlcharrefreplace', etc.

**Retorno:** Objeto `bytes`.

**Exemplo:**
```python
texto = "café"
print(texto.encode())                 # b'caf\xc3\xa9' (utf-8)
print(texto.encode('ascii', 'ignore')) # b'caf'
```

---

## Tabela Resumo

| Método           | Descrição rápida                                           |
|------------------|------------------------------------------------------------|
| `capitalize()`   | Primeira maiúscula, resto minúscula                        |
| `casefold()`     | Versão para comparação case‑insensitive (Unicode)         |
| `lower()`        | Converte para minúsculas                                   |
| `upper()`        | Converte para maiúsculas                                   |
| `swapcase()`     | Inverte maiúsculas/minúsculas                              |
| `title()`        | Formato de título (cada palavra inicia maiúscula)         |
| `count()`        | Conta ocorrências de substring                             |
| `find()`         | Posição da primeira ocorrência (ou -1)                     |
| `rfind()`        | Posição da última ocorrência (ou -1)                       |
| `index()`        | Como find, mas levanta erro se não encontrar               |
| `rindex()`       | Como rfind, mas levanta erro                               |
| `startswith()`   | Verifica prefixo                                            |
| `endswith()`     | Verifica sufixo                                             |
| `isalnum()`      | Verdadeiro se todos caracteres forem alfanuméricos         |
| `isalpha()`      | Verdadeiro se todos forem letras                            |
| `isascii()`      | Verdadeiro se todos forem ASCII                             |
| `isdecimal()`    | Verdadeiro se todos forem dígitos decimais                  |
| `isdigit()`      | Verdadeiro se todos forem dígitos (inclui sobrescritos)    |
| `isidentifier()` | Verdadeiro se for identificador Python válido               |
| `islower()`      | Verdadeiro se letras forem minúsculas                       |
| `isnumeric()`    | Verdadeiro se todos forem caracteres numéricos              |
| `isprintable()`  | Verdadeiro se todos forem imprimíveis                       |
| `isspace()`      | Verdadeiro se só espaços                                    |
| `istitle()`      | Verdadeiro se estiver em formato título                     |
| `isupper()`      | Verdadeiro se letras forem maiúsculas                       |
| `join()`         | Junta iterável com a string como separador                  |
| `split()`        | Divide a string em lista                                    |
| `rsplit()`       | Divide a partir da direita                                  |
| `splitlines()`   | Divide nas quebras de linha                                 |
| `partition()`    | Divide em três partes na primeira ocorrência                |
| `rpartition()`   | Divide em três partes na última ocorrência                  |
| `replace()`      | Substitui substring                                         |
| `strip()`        | Remove caracteres das extremidades                          |
| `lstrip()`       | Remove caracteres da esquerda                               |
| `rstrip()`       | Remove caracteres da direita                                |
| `removeprefix()` | Remove prefixo, se presente                                 |
| `removesuffix()` | Remove sufixo, se presente                                  |
| `center()`       | Centraliza com preenchimento                                |
| `ljust()`        | Justifica à esquerda                                        |
| `rjust()`        | Justifica à direita                                         |
| `zfill()`        | Preenche com zeros à esquerda                               |
| `expandtabs()`   | Expande tabs para espaços                                   |
| `encode()`       | Codifica a string para bytes                                |

---
