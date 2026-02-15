# Lista de Funções Nativas (Built-in) do Python

Esta é uma lista completa das funções built-in disponíveis no Python (versão 3.10+).

## **A - C**

### **A**
- **`abs()`** - Retorna o valor absoluto de um número
- **`aiter()`** - Retorna um iterador assíncrono (Python 3.10+)
- **`all()`** - Retorna True se todos os elementos do iterável são verdadeiros
- **`anext()`** - Retorna o próximo item de um iterador assíncrono (Python 3.10+)
- **`any()`** - Retorna True se algum elemento do iterável é verdadeiro
- **`ascii()`** - Retorna string com representação ASCII de um objeto

### **B**
- **`bin()`** - Converte inteiro para string binária com prefixo '0b'
- **`bool()`** - Retorna valor booleano (True/False)
- **`breakpoint()`** - Entra no debugger (Python 3.7+)
- **`bytearray()`** - Retorna um array de bytes mutável
- **`bytes()`** - Retorna um objeto bytes imutável

### **C**
- **`callable()`** - Verifica se objeto pode ser chamado (como função)
- **`chr()`** - Retorna caractere Unicode a partir de um inteiro
- **`classmethod()`** - Transforma método em método de classe
- **`compile()`** - Compila código fonte em objeto código ou AST
- **`complex()`** - Cria número complexo

## **D - H**

### **D**
- **`delattr()`** - Remove atributo de objeto
- **`dict()`** - Cria dicionário
- **`dir()`** - Retorna lista de nomes no escopo local ou atributos do objeto
- **`divmod()`** - Retorna quociente e resto da divisão

### **E**
- **`enumerate()`** - Retorna objeto enumerado (índice, valor)
- **`eval()`** - Avalia expressão Python em string
- **`exec()`** - Executa código Python dinamicamente

### **F**
- **`filter()`** - Filtra elementos de iterável usando função
- **`float()`** - Converte para número de ponto flutuante
- **`format()`** - Formata valor usando especificação de formato
- **`frozenset()`** - Cria conjunto imutável (frozenset)

### **G**
- **`getattr()`** - Obtém valor de atributo de objeto
- **`globals()`** - Retorna dicionário do namespace global

### **H**
- **`hasattr()`** - Verifica se objeto tem atributo
- **`hash()`** - Retorna valor hash do objeto
- **`help()`** - Invoca sistema de ajuda integrado
- **`hex()`** - Converte inteiro para string hexadecimal com prefixo '0x'

## **I - L**

### **I**
- **`id()`** - Retorna identidade (endereço de memória) do objeto
- **`input()`** - Lê string da entrada padrão
- **`int()`** - Converte para inteiro
- **`isinstance()`** - Verifica se objeto é instância de classe
- **`issubclass()`** - Verifica se classe é subclasse de outra
- **`iter()`** - Retorna iterador para objeto

### **L**
- **`len()`** - Retorna comprimento (número de itens) de objeto
- **`list()`** - Cria lista
- **`locals()`** - Retorna dicionário do namespace local

## **M - O**

### **M**
- **`map()`** - Aplica função a todos os itens de iterável
- **`max()`** - Retorna maior item de iterável ou maior de dois+ argumentos
- **`memoryview()`** - Retorna objeto memoryview
- **`min()`** - Retorna menor item de iterável ou menor de dois+ argumentos

### **N**
- **`next()`** - Retorna próximo item de iterador

### **O**
- **`object()`** - Retorna novo objeto vazio (base para todas as classes)
- **`oct()`** - Converte inteiro para string octal com prefixo '0o'
- **`open()`** - Abre arquivo e retorna objeto arquivo
- **`ord()`** - Retorna código Unicode de caractere
- **`pow()`** - Retorna potência de número (x**y) ou (x**y % z)

## **P - R**

### **P**
- **`print()`** - Imprime objetos no stream de texto
- **`property()`** - Retorna propriedade de atributo

### **R**
- **`range()`** - Retorna sequência de números imutável
- **`repr()`** - Retorna string representação imprimível de objeto
- **`reversed()`** - Retorna iterador reverso
- **`round()`** - Arredonda número para n dígitos decimais

## **S**

### **S**
- **`set()`** - Cria conjunto
- **`setattr()`** - Define valor de atributo de objeto
- **`slice()`** - Retorna objeto slice (fatiamento)
- **`sorted()`** - Retorna nova lista ordenada de iterável
- **`staticmethod()`** - Transforma método em método estático
- **`str()`** - Retorna representação em string de objeto
- **`sum()`** - Soma itens de iterável da esquerda para direita
- **`super()`** - Retorna objeto proxy para métodos da classe pai

## **T - Z**

### **T**
- **`tuple()`** - Cria tupla
- **`type()`** - Retorna tipo do objeto ou cria novo tipo

### **V**
- **`vars()`** - Retorna `__dict__` do objeto ou namespace local

### **Z**
- **`zip()`** - Agrega elementos de múltiplos iteráveis
- **`__import__()`** - Função usada por `import` (avaliada internamente)

## **Funções para Matemática e Números**

| Função | Descrição |
|--------|-----------|
| `abs(x)` | Valor absoluto |
| `divmod(a, b)` | Quociente e resto |
| `max(iter)` | Valor máximo |
| `min(iter)` | Valor mínimo |
| `pow(x, y)` | Potenciação |
| `round(x, n)` | Arredondamento |
| `sum(iter)` | Soma |
| `complex(r, i)` | Número complexo |
| `float(x)` | Número float |
| `int(x, base)` | Número inteiro |
| `bin(x)` | Binário |
| `hex(x)` | Hexadecimal |
| `oct(x)` | Octal |

## **Funções para Conversão de Tipos**

| Função | Descrição |
|--------|-----------|
| `bool(x)` | Booleano |
| `bytes(x)` | Bytes |
| `bytearray(x)` | Bytearray |
| `chr(i)` | Caractere de código Unicode |
| `dict(x)` | Dicionário |
| `float(x)` | Float |
| `frozenset(x)` | Frozenset |
| `int(x)` | Inteiro |
| `list(x)` | Lista |
| `memoryview(x)` | Memoryview |
| `ord(c)` | Código Unicode de caractere |
| `set(x)` | Conjunto |
| `str(x)` | String |
| `tuple(x)` | Tupla |

## **Funções para Iteráveis**

| Função | Descrição |
|--------|-----------|
| `all(iter)` | Todos verdadeiros? |
| `any(iter)` | Algum verdadeiro? |
| `enumerate(iter)` | Enumerar |
| `filter(fun, iter)` | Filtrar |
| `iter(obj)` | Iterador |
| `len(obj)` | Tamanho |
| `map(fun, iter)` | Mapear |
| `max(iter)` | Máximo |
| `min(iter)` | Mínimo |
| `next(iter)` | Próximo item |
| `range(start, stop, step)` | Sequência numérica |
| `reversed(seq)` | Reverso |
| `slice(start, stop, step)` | Fatiamento |
| `sorted(iter, key, reverse)` | Ordenado |
| `sum(iter)` | Soma |
| `zip(*iterables)` | Zipar |

## **Funções para Objetos e Classes**

| Função | Descrição |
|--------|-----------|
| `callable(obj)` | É chamável? |
| `delattr(obj, name)` | Deletar atributo |
| `dir(obj)` | Listar atributos |
| `getattr(obj, name)` | Obter atributo |
| `hasattr(obj, name)` | Tem atributo? |
| `id(obj)` | Identidade |
| `isinstance(obj, class)` | É instância? |
| `issubclass(class1, class2)` | É subclasse? |
| `setattr(obj, name, value)` | Definir atributo |
| `type(obj)` | Tipo |
| `vars(obj)` | Dicionário de atributos |
| `super()` | Referência à superclasse |

## **Funções para Strings e Entrada/Saída**

| Função | Descrição |
|--------|-----------|
| `ascii(obj)` | Representação ASCII |
| `chr(i)` | Caractere de código |
| `format(value, format_spec)` | Formatar |
| `input(prompt)` | Entrada de usuário |
| `open(file, mode)` | Abrir arquivo |
| `ord(c)` | Código do caractere |
| `print(*objects)` | Imprimir |
| `repr(obj)` | Representação string |
| `str(obj)` | String |

## **Funções de Sistema e Execução**

| Função | Descrição |
|--------|-----------|
| `breakpoint()` | Ponto de depuração |
| `compile(source, filename, mode)` | Compilar código |
| `eval(expression)` | Avaliar expressão |
| `exec(object)` | Executar código |
| `globals()` | Namespace global |
| `help(object)` | Ajuda |
| `locals()` | Namespace local |
| `__import__(name)` | Importar módulo |

## **Funções de Decoradores Built-in**

| Função | Descrição |
|--------|-----------|
| `classmethod(func)` | Método de classe |
| `property(fget, fset, fdel)` | Propriedade |
| `staticmethod(func)` | Método estático |

## **Funções Especiais e Úteis**

| Função | Descrição |
|--------|-----------|
| `hash(obj)` | Hash do objeto |
| `memoryview(obj)` | Vista de memória |
| `object()` | Objeto base |
| `aiter()` / `anext()` | Iteração assíncrona (Python 3.10+) |

## **Exemplos Práticos de Uso**

```python
# Conversões numéricas
print(bin(10))        # '0b1010'
print(hex(255))       # '0xff'
print(oct(64))        # '0o100'

# Trabalhando com iteráveis
numeros = [1, 2, 3, 4]
print(all(n > 0 for n in numeros))  # True
print(any(n > 5 for n in numeros))  # False

# Enumerando elementos
lista = ['a', 'b', 'c']
for i, valor in enumerate(lista, start=1):
    print(f"{i}: {valor}")

# Zipando listas
nomes = ['Ana', 'João', 'Maria']
idades = [25, 30, 28]
for nome, idade in zip(nomes, idades):
    print(f"{nome}: {idade} anos")

# Usando map e filter
quadrados = list(map(lambda x: x**2, [1, 2, 3, 4]))
pares = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4, 5]))
```

## **Notas Importantes**

1. **Python 2 vs Python 3:** Algumas funções foram removidas ou modificadas
   - `raw_input()` foi substituída por `input()` em Python 3
   - `reduce()` foi movida para `functools` em Python 3

2. **Funções removidas:** Algumas funções não existem mais em Python 3:
   - `apply()`, `coerce()`, `execfile()`, `file()`, `long()`, `raw_input()`, `reduce()`, `reload()`, `xrange()`

3. **Novidades recentes:**
   - `breakpoint()` foi adicionada no Python 3.7
   - `aiter()` e `anext()` foram adicionadas no Python 3.10

4. **Para ver todas as funções built-in:**
```python
# Listar todas as funções built-in
import builtins
print(dir(builtins))
```

Esta lista cobre todas as funções built-in disponíveis no Python padrão. Lembre-se que funções específicas de módulos (como `math.sqrt()`, `os.listdir()`, etc.) não são consideradas built-in, pois precisam ser importadas.
