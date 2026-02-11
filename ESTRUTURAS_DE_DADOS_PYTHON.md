# 📚 Manual Completo de Estruturas de Dados em Python

## 📊 Índice

1. [Introdução às Estruturas de Dados](#introdução-às-estruturas-de-dados)
2. [Estruturas Nativas do Python](#estruturas-nativas-do-python)
3. [Módulo Collections](#módulo-collections)
4. [Módulo heapq (Filas de Prioridade)](#módulo-heapq-filas-de-prioridade)
5. [Módulo bisect (Arrays Ordenados)](#módulo-bisect-arrays-ordenados)
6. [Módulo array (Arrays de Tipos Específicos)](#módulo-array-arrays-de-tipos-específicos)
7. [Módulo queue (Filas Thread-Safe)](#módulo-queue-filas-thread-safe)
8. [Grafos em Python](#grafos-em-python)
9. [Árvores em Python](#árvores-em-python)
10. [Estruturas de Dados Personalizadas](#estruturas-de-dados-personalizadas)
11. [Análise de Complexidade](#análise-de-complexidade)
12. [Benchmark e Performance](#benchmark-e-performance)

---

## 🎯 Introdução às Estruturas de Dados

### O que são Estruturas de Dados?

**Estruturas de dados** são formas organizadas de armazenar e gerenciar dados para acesso e modificação eficientes. Em Python, temos:

- **Estruturas nativas**: Listas, tuplas, dicionários, conjuntos
- **Estruturas especializadas**: Módulos `collections`, `heapq`, `bisect`
- **Estruturas personalizadas**: Implementadas pelo programador

### Complexidade Assintótica (Big O)

```python
"""
Principais complexidades:

O(1)     - Constante          (melhor)
O(log n) - Logarítmica
O(n)     - Linear
O(n log n) - Linearítmica
O(n²)    - Quadrática
O(2^n)   - Exponencial        (pior)
O(n!)    - Fatorial           (muito pior)
"""
```

---

## 🏗️ Estruturas Nativas do Python

### 1. Listas (List)

```python
# Criando listas
lista = [1, 2, 3, 4, 5]
lista_vazia = []
lista_mista = [1, "dois", 3.0, True]
lista_aninhada = [[1, 2], [3, 4]]

# Operações básicas
lista.append(6)           # O(1) - Adiciona ao final
lista.insert(0, 0)        # O(n) - Insere em posição específica
lista.extend([7, 8, 9])   # O(k) - Extende com outra lista
valor = lista.pop()       # O(1) - Remove e retorna último elemento
valor = lista.pop(0)      # O(n) - Remove do início
lista.remove(3)           # O(n) - Remove primeira ocorrência do valor
lista.clear()             # O(1) - Limpa toda a lista

# Acesso
primeiro = lista[0]       # O(1) - Acesso por índice
ultimo = lista[-1]        # O(1) - Último elemento
sublista = lista[1:4]     # O(k) - Slicing (fatia)

# Busca
existe = 3 in lista       # O(n) - Verifica existência
indice = lista.index(2)   # O(n) - Retorna índice do valor
contagem = lista.count(2) # O(n) - Conta ocorrências

# Ordenação
lista.sort()              # O(n log n) - Ordena in-place
lista.sort(reverse=True)  # O(n log n) - Ordena decrescente
lista_ordenada = sorted(lista)  # O(n log n) - Retorna nova lista

# List Comprehensions
quadrados = [x**2 for x in range(10)]          # [0, 1, 4, 9, ..., 81]
pares = [x for x in range(10) if x % 2 == 0]   # [0, 2, 4, 6, 8]
matrix = [[i*j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]

# Métodos úteis
lista.reverse()           # O(n) - Inverte in-place
copia = lista.copy()      # O(n) - Cópia superficial
len(lista)                # O(1) - Tamanho da lista
min(lista), max(lista)    # O(n) - Mínimo e máximo
sum(lista)                # O(n) - Soma dos elementos

# Performance
"""
Operação      Complexidade  Exemplo
-----------   ------------  -------
Acesso        O(1)          lista[0]
Append        O(1)          lista.append(x)
Pop último    O(1)          lista.pop()
Pop início    O(n)          lista.pop(0)
Inserção      O(n)          lista.insert(i, x)
Remoção       O(n)          lista.remove(x)
Busca         O(n)          x in lista
Slicing       O(k)          lista[i:j]
Sort          O(n log n)    lista.sort()
"""
```

### 2. Tuplas (Tuple)

```python
# Criando tuplas
tupla = (1, 2, 3, 4, 5)
tupla_vazia = ()
tupla_um_elemento = (1,)  # A vírgula é obrigatória!
tupla_sem_parenteses = 1, 2, 3  # Também é tupla

# Características
# - Imutável (não pode modificar depois de criada)
# - Mais eficiente que listas para dados constantes
# - Pode ser usada como chave de dicionário

# Operações
comprimento = len(tupla)      # O(1)
contagem = tupla.count(2)     # O(n)
indice = tupla.index(3)       # O(n)
existe = 4 in tupla           # O(n)

# Desempacotamento
a, b, c = (1, 2, 3)
a, *resto = (1, 2, 3, 4, 5)   # a=1, resto=[2, 3, 4, 5]
*a, b = (1, 2, 3, 4, 5)       # a=[1, 2, 3, 4], b=5

# Concatenação
nova_tupla = tupla + (6, 7)   # O(n) - Cria nova tupla
tupla_repetida = tupla * 3    # O(n) - Repete 3 vezes

# Uso como chave de dicionário
coordenadas = {
    (0, 0): "origem",
    (1, 2): "ponto A",
    (3, 4): "ponto B"
}
```

### 3. Dicionários (Dict)

```python
# Criando dicionários
dicionario = {"nome": "João", "idade": 30, "cidade": "SP"}
dicionario_vazio = {}
dicionario_com_dict = dict(nome="Maria", idade=25)
dicionario_de_listas = dict.fromkeys(["a", "b", "c"], 0)  # {'a': 0, 'b': 0, 'c': 0}

# Operações básicas
dicionario["email"] = "joao@email.com"  # O(1) - Adiciona/atualiza
valor = dicionario["nome"]              # O(1) - Acesso (KeyError se não existir)
valor = dicionario.get("nome")          # O(1) - Acesso seguro (retorna None)
valor = dicionario.get("telefone", "N/A")  # O(1) - Com valor padrão
del dicionario["cidade"]                # O(1) - Remove item
item = dicionario.pop("idade")          # O(1) - Remove e retorna valor
item = dicionario.popitem()             # O(1) - Remove e retorna último (Python 3.7+)

# Métodos úteis
chaves = dicionario.keys()              # O(1) - Vista das chaves
valores = dicionario.values()           # O(1) - Vista dos valores
itens = dicionario.items()              # O(1) - Vista dos pares chave-valor
novo = dicionario.copy()                # O(n) - Cópia superficial
dicionario.clear()                      # O(1) - Limpa dicionário
dicionario.update({"pais": "BR", "idade": 31})  # O(k) - Atualiza múltiplos valores

# Verificações
existe = "nome" in dicionario           # O(1) - Verifica chave
tamanho = len(dicionario)               # O(1) - Número de itens

# Dict Comprehensions
quadrados = {x: x**2 for x in range(5)}      # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
pares = {k: v for k, v in quadrados.items() if v % 2 == 0}  # Filtra valores pares

# Operações avançadas
# defaultdict - valor padrão para chaves não existentes
from collections import defaultdict
dd = defaultdict(list)
dd["frutas"].append("maçã")  # Não precisa verificar se a chave existe

# Performance
"""
Operação      Complexidade  Exemplo
-----------   ------------  -------
Acesso        O(1)          dicio[chave]
Inserção      O(1)          dicio[chave] = valor
Deleção       O(1)          del dicio[chave]
Busca chave   O(1)          chave in dicio
Copia         O(n)          dicio.copy()
Iteração      O(n)          for chave in dicio
"""
```

### 4. Conjuntos (Set)

```python
# Criando conjuntos
conjunto = {1, 2, 3, 4, 5}
conjunto_vazio = set()
conjunto_de_lista = set([1, 2, 2, 3, 3])  # {1, 2, 3} - Remove duplicatas
frozenset_conj = frozenset([1, 2, 3])     # Conjunto imutável

# Características
# - Elementos únicos (não permite duplicatas)
# - Não mantém ordem (antes do Python 3.7)
# - Muito eficiente para operações de pertinência

# Operações básicas
conjunto.add(6)           # O(1) - Adiciona elemento
conjunto.remove(3)        # O(1) - Remove elemento (KeyError se não existir)
conjunto.discard(7)       # O(1) - Remove se existir (sem erro)
elemento = conjunto.pop() # O(1) - Remove e retorna elemento aleatório
conjunto.clear()          # O(1) - Limpa conjunto

# Operações de conjunto
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

uniao = A | B             # {1, 2, 3, 4, 5, 6} - O(len(A) + len(B))
intersecao = A & B        # {3, 4} - O(min(len(A), len(B)))
diferenca = A - B         # {1, 2} - O(len(A))
diferenca_simetrica = A ^ B  # {1, 2, 5, 6} - O(len(A) + len(B))

# Métodos equivalentes
uniao = A.union(B)
intersecao = A.intersection(B)
diferenca = A.difference(B)
diferenca_simetrica = A.symmetric_difference(B)

# Verificações
subset = {1, 2}.issubset(A)      # True - O(len(subset))
superset = A.issuperset({1, 2})  # True - O(len(subset))
disjoint = A.isdisjoint({7, 8})  # True - O(min(len(A), len(B)))

# Set Comprehensions
quadrados = {x**2 for x in range(5)}      # {0, 1, 4, 9, 16}
pares = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}

# Performance
"""
Operação      Complexidade  Exemplo
-----------   ------------  -------
Adição        O(1)          conjunto.add(x)
Remoção       O(1)          conjunto.remove(x)
Pertinência   O(1)          x in conjunto
União         O(n+m)        A | B
Interseção    O(min(n,m))   A & B
Diferença     O(n)          A - B
"""
```

### 5. Strings como Estrutura de Dados

```python
# Strings são sequências imutáveis de caracteres
texto = "Python é incrível!"

# Operações com strings
comprimento = len(texto)              # O(1)
caractere = texto[0]                  # O(1) - 'P'
substring = texto[0:6]                # O(k) - 'Python'
existe = "incrível" in texto         # O(n) - True
contagem = texto.count("i")           # O(n) - 2
posicao = texto.find("é")             # O(n) - 7
maiusculas = texto.upper()            # O(n) - 'PYTHON É INCRÍVEL!'
minusculas = texto.lower()            # O(n) - 'python é incrível!'
sem_espacos = texto.strip()           # O(n) - Remove espaços
lista_palavras = texto.split()        # O(n) - ['Python', 'é', 'incrível!']
nova_string = texto.replace("é", "foi")  # O(n) - 'Python foi incrível!'

# Formatação
nome, idade = "Maria", 30
formatada = f"{nome} tem {idade} anos"  # f-string (Python 3.6+)
formatada = "{} tem {} anos".format(nome, idade)  # format()
formatada = "%s tem %d anos" % (nome, idade)  # % formatting

# String Comprehensions (join)
lista = ["Python", "é", "poderoso"]
frase = " ".join(lista)  # 'Python é poderoso' - O(n)

# Verificações
eh_alpha = "abc".isalpha()     # True
eh_digit = "123".isdigit()     # True
eh_alnum = "abc123".isalnum()  # True
eh_space = "   ".isspace()     # True
eh_lower = "abc".islower()     # True
eh_upper = "ABC".isupper()     # True

# Performance
"""
Strings são imutáveis, então operações que modificam criam novas strings:
- Concatenação: O(n + m) onde n e m são tamanhos das strings
- Slicing: O(k) onde k é o tamanho da fatia
- Busca: O(n) para operações como find(), count()
- Replace: O(n) para criar nova string
"""
```

---

## 📦 Módulo Collections

### 1. `collections.namedtuple`

```python
from collections import namedtuple

# Criando uma namedtuple
Ponto = namedtuple('Ponto', ['x', 'y'])
p = Ponto(10, 20)

# Acesso
print(p.x, p.y)          # 10 20
print(p[0], p[1])        # 10 20 (também funciona como tupla)

# Métodos
x, y = p                 # Desempacotamento
dicionario = p._asdict() # Converte para dicionário
novo_ponto = p._replace(x=15)  # Cria nova com valores alterados
p2 = Ponto._make([5, 10])      # Cria a partir de iterável

# Com valores padrão
Pessoa = namedtuple('Pessoa', ['nome', 'idade', 'cidade'], defaults=["Desconhecida", 0])
pessoa = Pessoa("João")  # Pessoa(nome='João', idade=0, cidade='Desconhecida')

# Performance: mesma performance que tuplas, mas mais legível
```

### 2. `collections.defaultdict`

```python
from collections import defaultdict

# Cria dicionário com valor padrão
dd_int = defaultdict(int)          # Valor padrão: 0
dd_list = defaultdict(list)        # Valor padrão: []
dd_set = defaultdict(set)          # Valor padrão: set()
dd_dict = defaultdict(dict)        # Valor padrão: {}

# Exemplo 1: Contagem de palavras
texto = "o rato roeu a roupa do rei de roma"
contador = defaultdict(int)

for palavra in texto.split():
    contador[palavra] += 1  # Não precisa verificar se chave existe

print(dict(contador))
# {'o': 2, 'rato': 1, 'roeu': 1, 'a': 1, 'roupa': 1, 'do': 1, 'rei': 1, 'de': 1, 'roma': 1}

# Exemplo 2: Agrupamento por chave
pessoas = [
    ("cidade", "João"),
    ("cidade", "Maria"),
    ("estado", "Carlos"),
    ("cidade", "Ana"),
    ("estado", "Bianca")
]

agrupamento = defaultdict(list)
for tipo, nome in pessoas:
    agrupamento[tipo].append(nome)

print(dict(agrupamento))
# {'cidade': ['João', 'Maria', 'Ana'], 'estado': ['Carlos', 'Bianca']}

# Com função personalizada
def valor_padrao():
    return "não encontrado"

dd_custom = defaultdict(valor_padrao)
print(dd_custom["chave"])  # 'não encontrado'
```

### 3. `collections.Counter`

```python
from collections import Counter

# Criação
c1 = Counter()                          # Counter vazio
c2 = Counter(['a', 'b', 'a', 'c', 'b', 'a'])  # Counter({'a': 3, 'b': 2, 'c': 1})
c3 = Counter("abracadabra")             # Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
c4 = Counter(a=3, b=2, c=1)             # Counter({'a': 3, 'b': 2, 'c': 1})

# Operações básicas
print(c3['a'])                         # 5 - Acesso
print(c3['z'])                         # 0 - Chave não existente
c3['a'] = 2                            # Modificação
del c3['b']                            # Remoção

# Métodos
elementos = list(c3.elements())        # Lista de elementos
mais_comuns = c3.most_common(3)        # [('a', 5), ('b', 2), ('r', 2)]
total = sum(c3.values())               # 11 - Soma total

# Operações matemáticas
c1 = Counter(a=3, b=1)
c2 = Counter(a=1, b=2)

print(c1 + c2)    # Counter({'a': 4, 'b': 3}) - Soma
print(c1 - c2)    # Counter({'a': 2}) - Subtração (valores negativos removidos)
print(c1 & c2)    # Counter({'a': 1, 'b': 1}) - Interseção (mínimo)
print(c1 | c2)    # Counter({'a': 3, 'b': 2}) - União (máximo)

# Exemplos práticos
# Contar palavras em texto
texto = "o rato roeu a roupa do rei de roma o rei ficou nu"
palavras = texto.split()
contador = Counter(palavras)
print(contador.most_common(3))  # [('o', 2), ('rei', 2), ('rato', 1)]

# Encontrar anagramas
def sao_anagramas(s1, s2):
    return Counter(s1) == Counter(s2)

print(sao_anagramas("listen", "silent"))  # True
print(sao_anagramas("python", "typhon"))  # True

# Contar caracteres únicos
frase = "programação python"
contador_chars = Counter(frase)
print(f"Caracteres únicos: {len(contador_chars)}")  # 14 (inclui espaço e 'ç')
```

### 4. `collections.OrderedDict`

```python
from collections import OrderedDict

# Criando (antes do Python 3.7, dict não mantinha ordem)
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3

print(list(od.keys()))    # ['a', 'b', 'c'] - Ordem de inserção preservada

# Reordenando
od.move_to_end('a')       # Move 'a' para o final
od.move_to_end('c', last=False)  # Move 'c' para o início

# Último item
chave, valor = od.popitem()        # Remove e retorna último
chave, valor = od.popitem(last=False)  # Remove e retorna primeiro

# Comparação
d1 = OrderedDict([('a', 1), ('b', 2)])
d2 = OrderedDict([('b', 2), ('a', 1)])
print(d1 == d2)  # False (ordem diferente)

# Útil para LRU Cache simples
class LRUCache:
    def __init__(self, capacidade):
        self.cache = OrderedDict()
        self.capacidade = capacidade
    
    def get(self, chave):
        if chave not in self.cache:
            return -1
        self.cache.move_to_end(chave)
        return self.cache[chave]
    
    def put(self, chave, valor):
        if chave in self.cache:
            self.cache.move_to_end(chave)
        self.cache[chave] = valor
        if len(self.cache) > self.capacidade:
            self.cache.popitem(last=False)
```

### 5. `collections.deque` (Double-Ended Queue)

```python
from collections import deque

# Criando deques
d = deque()                     # deque vazio
d = deque([1, 2, 3])           # deque com elementos
d = deque("abc")               # deque(['a', 'b', 'c'])
d = deque(maxlen=3)            # deque com tamanho máximo

# Operações nos dois lados
d.append(4)                    # Adiciona à direita: deque([1, 2, 3, 4])
d.appendleft(0)                # Adiciona à esquerda: deque([0, 1, 2, 3, 4])
d.pop()                        # Remove da direita: 4
d.popleft()                    # Remove da esquerda: 0

# Outras operações
d.extend([5, 6])               # Estende à direita
d.extendleft([-1, -2])         # Estende à esquerda (inverte ordem!)
d.rotate(1)                    # Rotaciona 1 para direita
d.rotate(-1)                   # Rotaciona 1 para esquerda
d.reverse()                    # Inverte ordem

# Acesso
primeiro = d[0]                # Acesso por índice O(1)
ultimo = d[-1]                 # Último elemento
sublista = list(d)[1:3]        # Slicing (converte para lista primeiro)

# Performance
"""
Operação      Complexidade  Exemplo
-----------   ------------  -------
append        O(1)          d.append(x)
appendleft    O(1)          d.appendleft(x)
pop           O(1)          d.pop()
popleft       O(1)          d.popleft()
extend        O(k)          d.extend(iter)
extendleft    O(k)          d.extendleft(iter)
rotate        O(k)          d.rotate(k)
acesso[i]     O(1)          d[i]
len(d)        O(1)          len(d)
"""

# Exemplo: Sliding Window Maximum
def max_sliding_window(nums, k):
    """Retorna máximo de cada janela de tamanho k."""
    if not nums:
        return []
    
    resultado = []
    d = deque()  # Armazena índices
    
    for i, num in enumerate(nums):
        # Remove índices fora da janela
        while d and d[0] <= i - k:
            d.popleft()
        
        # Remove elementos menores que o atual
        while d and nums[d[-1]] < num:
            d.pop()
        
        d.append(i)
        
        # Adiciona máximo à resposta
        if i >= k - 1:
            resultado.append(nums[d[0]])
    
    return resultado

# Uso
nums = [1, 3, -1, -3, 5, 3, 6, 7]
print(max_sliding_window(nums, 3))  # [3, 3, 5, 5, 6, 7]
```

### 6. `collections.ChainMap`

```python
from collections import ChainMap

# Encadeia múltiplos dicionários
d1 = {'a': 1, 'b': 2}
d2 = {'b': 3, 'c': 4}
d3 = {'c': 5, 'd': 6}

chain = ChainMap(d1, d2, d3)

# Busca em ordem (d1 → d2 → d3)
print(chain['a'])  # 1 (de d1)
print(chain['b'])  # 2 (de d1, não de d2)
print(chain['c'])  # 4 (de d2, não de d3)
print(chain['d'])  # 6 (de d3)

# Modificações afetam apenas o primeiro dicionário
chain['e'] = 7  # Adiciona em d1
chain['b'] = 8  # Modifica em d1 (não d2)

# Operações
print(list(chain.keys()))    # ['b', 'a', 'e', 'c', 'd']
print(list(chain.values()))  # [8, 1, 7, 4, 6]
print(list(chain.items()))   # [('b', 8), ('a', 1), ('e', 7), ('c', 4), ('d', 6)]

# Novos ChainMaps
new_chain = chain.new_child({'f': 9})  # Adiciona novo dicionário no início
print(new_chain['f'])  # 9

# Pais
parents = chain.parents  # ChainMap(d2, d3)

# Uso prático: Configurações com prioridade
defaults = {'theme': 'light', 'language': 'en'}
user_prefs = {'theme': 'dark'}
system_env = {'language': 'pt-br'}

config = ChainMap(user_prefs, system_env, defaults)
print(config['theme'])    # 'dark' (de user_prefs)
print(config['language']) # 'pt-br' (de system_env)
print(config.get('font', 'Arial'))  # 'Arial' (não está em nenhum)
```

---

## 📈 Módulo heapq (Filas de Prioridade)

```python
import heapq

# Heap é uma árvore binária completa onde o pai é menor que os filhos (min-heap)

# Criando heap a partir de lista
lista = [5, 7, 9, 1, 3]
heapq.heapify(lista)  # Transforma lista em heap in-place O(n)
print(lista)  # [1, 3, 9, 7, 5]

# Operações básicas
heapq.heappush(lista, 4)  # Adiciona elemento O(log n)
menor = heapq.heappop(lista)  # Remove e retorna menor elemento O(log n)
print(menor)  # 1

# Ver menor sem remover
menor = lista[0]  # O(1)

# Push + pop combinados
item = heapq.heappushpop(lista, 2)  # Adiciona 2 e remove menor
item = heapq.heapreplace(lista, 0)  # Remove menor e adiciona 0

# n maiores/menores elementos
numeros = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
tres_maiores = heapq.nlargest(3, numeros)  # [9, 8, 7] O(n log k)
tres_menores = heapq.nsmallest(3, numeros)  # [0, 1, 2] O(n log k)

# Exemplo: Fila de prioridade com tuplas
# Heap ordena primeiro pelo primeiro elemento da tupla
tarefas = []
heapq.heappush(tarefas, (3, "Baixa prioridade"))
heapq.heappush(tarefas, (1, "Alta prioridade"))
heapq.heappush(tarefas, (2, "Média prioridade"))

while tarefas:
    prioridade, tarefa = heapq.heappop(tarefas)
    print(f"{prioridade}: {tarefa}")
# Saída: 1: Alta prioridade, 2: Média prioridade, 3: Baixa prioridade

# Exemplo: Merge de múltiplas listas ordenadas
def merge_sorted_lists(listas):
    """Merge k listas ordenadas."""
    heap = []
    
    # Adiciona primeiro elemento de cada lista
    for i, lista in enumerate(listas):
        if lista:
            heapq.heappush(heap, (lista[0], i, 0))
    
    resultado = []
    
    while heap:
        valor, lista_idx, elem_idx = heapq.heappop(heap)
        resultado.append(valor)
        
        # Adiciona próximo elemento da mesma lista
        if elem_idx + 1 < len(listas[lista_idx]):
            proximo_valor = listas[lista_idx][elem_idx + 1]
            heapq.heappush(heap, (proximo_valor, lista_idx, elem_idx + 1))
    
    return resultado

# Uso
listas = [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
print(merge_sorted_lists(listas))  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Implementação de Max-Heap (usando valores negativos)
class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def push(self, valor):
        heapq.heappush(self.heap, -valor)
    
    def pop(self):
        return -heapq.heappop(self.heap)
    
    def peek(self):
        return -self.heap[0] if self.heap else None
    
    def __len__(self):
        return len(self.heap)

# Uso
max_heap = MaxHeap()
max_heap.push(5)
max_heap.push(3)
max_heap.push(7)
print(max_heap.pop())  # 7 (maior elemento)
print(max_heap.pop())  # 5
print(max_heap.pop())  # 3
```

---

## 🔍 Módulo bisect (Arrays Ordenados)

```python
import bisect

# Mantém lista ordenada durante inserções
lista = [1, 3, 4, 4, 6, 8]

# Inserção mantendo ordenação
bisect.insort(lista, 5)  # insere 5 na posição correta
print(lista)  # [1, 3, 4, 4, 5, 6, 8]

bisect.insort_left(lista, 4)  # insere à esquerda de valores iguais
print(lista)  # [1, 3, 4, 4, 4, 5, 6, 8]

# Busca de posição de inserção
pos = bisect.bisect(lista, 7)  # índice onde 7 deveria ser inserido
print(pos)  # 7 (após o 6)

pos = bisect.bisect_left(lista, 4)  # índice do primeiro 4
print(pos)  # 2

pos = bisect.bisect_right(lista, 4)  # índice após o último 4
print(pos)  # 5

# Exemplo prático: Sistema de notas
def nota_para_conceito(pontuacao, limites=[60, 70, 80, 90], conceitos=['F', 'D', 'C', 'B', 'A']):
    """Converte pontuação em conceito usando bisect."""
    i = bisect.bisect(limites, pontuacao)
    return conceitos[i]

print(nota_para_conceito(85))  # 'B' (85 está entre 80 e 90)
print(nota_para_conceito(45))  # 'F' (45 < 60)

# Exemplo: Intervalos
class IntervaloManager:
    def __init__(self):
        self.inicios = []
        self.fins = []
    
    def adicionar_intervalo(self, inicio, fim):
        """Adiciona intervalo mantendo ordenação."""
        i = bisect.bisect(self.inicios, inicio)
        self.inicios.insert(i, inicio)
        self.fins.insert(i, fim)
    
    def consultar(self, ponto):
        """Encontra intervalo que contém o ponto."""
        i = bisect.bisect_right(self.inicios, ponto) - 1
        if i >= 0 and self.fins[i] >= ponto:
            return (self.inicios[i], self.fins[i])
        return None

# Uso
manager = IntervaloManager()
manager.adicionar_intervalo(10, 20)
manager.adicionar_intervalo(30, 40)
manager.adicionar_intervalo(25, 35)

print(manager.consultar(15))  # (10, 20)
print(manager.consultar(32))  # (25, 35)
print(manager.consultar(50))  # None
```

---

## 🧮 Módulo array (Arrays de Tipos Específicos)

```python
import array

# Arrays são mais eficientes que listas para tipos numéricos homogêneos
# Tipos disponíveis: https://docs.python.org/3/library/array.html

# Criando arrays
arr_int = array.array('i', [1, 2, 3, 4, 5])      # inteiros com sinal
arr_float = array.array('f', [1.0, 2.5, 3.7])    # float
arr_double = array.array('d', [1.0, 2.5, 3.7])   # double
arr_char = array.array('u', 'hello')            # caracteres Unicode

# Operações similares a listas
arr_int.append(6)
arr_int.extend([7, 8, 9])
valor = arr_int.pop()
valor = arr_int.pop(0)
arr_int.insert(0, 0)
arr_int.remove(3)
indice = arr_int.index(4)
contagem = arr_int.count(2)
arr_int.reverse()

# Acesso
primeiro = arr_int[0]
arr_int[0] = 10
sublista = arr_int[1:4]

# Métodos específicos de array
arr_int.tolist()           # Converte para lista
arr_int.fromlist([11, 12, 13])  # Adiciona de lista
arr_int.tobytes()          # Converte para bytes
arr_int.frombytes(b'\x01\x00\x00\x00')  # Lê de bytes
arr_int.tofile(open('dados.bin', 'wb'))  # Salva em arquivo

# Performance: muito mais eficiente que listas para dados numéricos
# Consome menos memória e operações são mais rápidas

# Exemplo: Processamento de dados numéricos grandes
def calcular_media_array(dados):
    """Calcula média usando array (eficiente para muitos dados)."""
    arr = array.array('d', dados)
    return sum(arr) / len(arr) if arr else 0

# Comparação com lista
import time

# Dados grandes
dados_grandes = list(range(1_000_000))

# Teste com lista
inicio = time.time()
lista = list(dados_grandes)
soma_lista = sum(lista)
tempo_lista = time.time() - inicio

# Teste com array
inicio = time.time()
arr = array.array('i', dados_grandes)
soma_array = sum(arr)
tempo_array = time.time() - inicio

print(f"Tempo lista: {tempo_lista:.4f}s")
print(f"Tempo array: {tempo_array:.4f}s")
print(f"Array é {tempo_lista/tempo_array:.1f}x mais rápido")
```

---

## 🚦 Módulo queue (Filas Thread-Safe)

```python
import queue
import threading
import time

# 1. Queue (FIFO - First In, First Out)
fila = queue.Queue(maxsize=3)  # Tamanho máximo opcional

# Operações
fila.put(1)                    # Adiciona item (bloqueia se cheia)
fila.put(2)
fila.put(3)
# fila.put(4, timeout=1)       # Timeout após 1 segundo se cheia
# fila.put_nowait(4)           # Lança queue.Full se cheia

item = fila.get()              # Remove e retorna item (bloqueia se vazia)
print(item)  # 1
# item = fila.get(timeout=1)   # Timeout após 1 segundo se vazia
# item = fila.get_nowait()     # Lança queue.Empty se vazia

fila.task_done()               # Marca tarefa como concluída

# Verificações
tamanho = fila.qsize()         # Número de itens na fila
vazia = fila.empty()           # True se vazia
cheia = fila.full()            # True se cheia

# 2. LifoQueue (LIFO - Last In, First Out) - Pilha
pilha = queue.LifoQueue()
pilha.put(1)
pilha.put(2)
print(pilha.get())  # 2 (último a entrar, primeiro a sair)

# 3. PriorityQueue (Fila de Prioridade)
fila_prio = queue.PriorityQueue()
fila_prio.put((3, "Baixa"))
fila_prio.put((1, "Alta"))
fila_prio.put((2, "Média"))

print(fila_prio.get()[1])  # "Alta"
print(fila_prio.get()[1])  # "Média"
print(fila_prio.get()[1])  # "Baixa"

# Exemplo: Produtor-Consumidor
def produtor(fila, itens):
    """Produz itens e coloca na fila."""
    for item in itens:
        print(f"Produzindo {item}")
        fila.put(item)
        time.sleep(0.1)
    fila.put(None)  # Sinal para terminar

def consumidor(fila):
    """Consome itens da fila."""
    while True:
        item = fila.get()
        if item is None:
            break
        print(f"Consumindo {item}")
        fila.task_done()
        time.sleep(0.2)

# Criando threads
fila_compartilhada = queue.Queue()
threads = []

# Thread produtora
p = threading.Thread(target=produtor, args=(fila_compartilhada, range(5)))
p.start()
threads.append(p)

# Thread consumidora
c = threading.Thread(target=consumidor, args=(fila_compartilhada,))
c.start()
threads.append(c)

# Aguarda término
for t in threads:
    t.join()

print("Produção e consumo concluídos!")

# 4. SimpleQueue (Python 3.7+) - Mais simples, sem tracking
simples = queue.SimpleQueue()
simples.put(1)
simples.put(2)
print(simples.get())  # 1
print(simples.empty())  # False
```

---

## 🕸️ Grafos em Python

### Representações de Grafos

```python
# 1. Lista de Adjacência (Mais comum)
class GrafoListaAdjacencia:
    def __init__(self, direcionado=False):
        self.grafo = {}
        self.direcionado = direcionado
    
    def adicionar_vertice(self, vertice):
        if vertice not in self.grafo:
            self.grafo[vertice] = {}
    
    def adicionar_aresta(self, u, v, peso=1):
        self.adicionar_vertice(u)
        self.adicionar_vertice(v)
        self.grafo[u][v] = peso
        if not self.direcionado:
            self.grafo[v][u] = peso
    
    def remover_aresta(self, u, v):
        if u in self.grafo and v in self.grafo[u]:
            del self.grafo[u][v]
            if not self.direcionado:
                del self.grafo[v][u]
    
    def vizinhos(self, vertice):
        return list(self.grafo.get(vertice, {}).keys())
    
    def peso(self, u, v):
        return self.grafo.get(u, {}).get(v)
    
    def vertices(self):
        return list(self.grafo.keys())
    
    def arestas(self):
        arestas = []
        for u in self.grafo:
            for v in self.grafo[u]:
                if not self.direcionado and (v, u, self.grafo[u][v]) in arestas:
                    continue
                arestas.append((u, v, self.grafo[u][v]))
        return arestas
    
    def __str__(self):
        resultado = []
        for vertice in self.grafo:
            vizinhos = self.grafo[vertice]
            if vizinhos:
                resultado.append(f"{vertice}: {vizinhos}")
            else:
                resultado.append(f"{vertice}: {{}}")
        return "\n".join(resultado)

# 2. Matriz de Adjacência
class GrafoMatrizAdjacencia:
    def __init__(self, vertices, direcionado=False):
        self.vertices = vertices
        self.direcionado = direcionado
        self.indices = {v: i for i, v in enumerate(vertices)}
        self.matriz = [[0] * len(vertices) for _ in range(len(vertices))]
    
    def adicionar_aresta(self, u, v, peso=1):
        i = self.indices[u]
        j = self.indices[v]
        self.matriz[i][j] = peso
        if not self.direcionado:
            self.matriz[j][i] = peso
    
    def remover_aresta(self, u, v):
        i = self.indices[u]
        j = self.indices[v]
        self.matriz[i][j] = 0
        if not self.direcionado:
            self.matriz[j][i] = 0
    
    def tem_aresta(self, u, v):
        i = self.indices[u]
        j = self.indices[v]
        return self.matriz[i][j] != 0
    
    def peso(self, u, v):
        i = self.indices[u]
        j = self.indices[v]
        return self.matriz[i][j]
    
    def vizinhos(self, vertice):
        i = self.indices[vertice]
        vizinhos = []
        for j, v in enumerate(self.vertices):
            if self.matriz[i][j] != 0:
                vizinhos.append(v)
        return vizinhos

# 3. Lista de Arestas
class GrafoListaArestas:
    def __init__(self, direcionado=False):
        self.arestas = []
        self.vertices = set()
        self.direcionado = direcionado
    
    def adicionar_aresta(self, u, v, peso=1):
        self.vertices.add(u)
        self.vertices.add(v)
        self.arestas.append((u, v, peso))
        if not self.direcionado:
            self.arestas.append((v, u, peso))
    
    def vizinhos(self, vertice):
        return [v for u, v, _ in self.arestas if u == vertice]

### Algoritmos de Grafos
class AlgoritmosGrafos:
    @staticmethod
    def bfs(grafo, inicio):
        """Busca em Largura (Breadth-First Search)."""
        visitados = set()
        fila = queue.Queue()
        resultado = []
        
        fila.put(inicio)
        visitados.add(inicio)
        
        while not fila.empty():
            vertice = fila.get()
            resultado.append(vertice)
            
            for vizinho in grafo.vizinhos(vertice):
                if vizinho not in visitados:
                    visitados.add(vizinho)
                    fila.put(vizinho)
        
        return resultado
    
    @staticmethod
    def dfs(grafo, inicio):
        """Busca em Profundidade (Depth-First Search)."""
        visitados = set()
        resultado = []
        
        def dfs_recursivo(v):
            visitados.add(v)
            resultado.append(v)
            
            for vizinho in grafo.vizinhos(v):
                if vizinho not in visitados:
                    dfs_recursivo(vizinho)
        
        dfs_recursivo(inicio)
        return resultado
    
    @staticmethod
    def dfs_iterativo(grafo, inicio):
        """DFS iterativo usando pilha."""
        visitados = set()
        pilha = [inicio]
        resultado = []
        
        while pilha:
            vertice = pilha.pop()
            if vertice not in visitados:
                visitados.add(vertice)
                resultado.append(vertice)
                
                # Adiciona vizinhos em ordem reversa para manter ordem natural
                for vizinho in reversed(grafo.vizinhos(vertice)):
                    if vizinho not in visitados:
                        pilha.append(vizinho)
        
        return resultado
    
    @staticmethod
    def dijkstra(grafo, inicio):
        """Algoritmo de Dijkstra para caminhos mínimos."""
        import heapq
        
        distancias = {v: float('inf') for v in grafo.vertices()}
        distancias[inicio] = 0
        anteriores = {v: None for v in grafo.vertices()}
        
        heap = [(0, inicio)]
        
        while heap:
            distancia_atual, vertice_atual = heapq.heappop(heap)
            
            if distancia_atual > distancias[vertice_atual]:
                continue
            
            for vizinho in grafo.vizinhos(vertice_atual):
                peso = grafo.peso(vertice_atual, vizinho)
                distancia = distancia_atual + peso
                
                if distancia < distancias[vizinho]:
                    distancias[vizinho] = distancia
                    anteriores[vizinho] = vertice_atual
                    heapq.heappush(heap, (distancia, vizinho))
        
        return distancias, anteriores
    
    @staticmethod
    def caminho_mais_curto(anteriores, destino):
        """Reconstrói caminho mais curto a partir do dicionário de anteriores."""
        caminho = []
        atual = destino
        
        while atual is not None:
            caminho.append(atual)
            atual = anteriores[atual]
        
        return caminho[::-1]

# Exemplo de uso
if __name__ == "__main__":
    # Criando grafo
    g = GrafoListaAdjacencia(direcionado=False)
    
    # Adicionando arestas
    g.adicionar_aresta('A', 'B', 4)
    g.adicionar_aresta('A', 'C', 2)
    g.adicionar_aresta('B', 'C', 1)
    g.adicionar_aresta('B', 'D', 5)
    g.adicionar_aresta('C', 'D', 8)
    g.adicionar_aresta('C', 'E', 10)
    g.adicionar_aresta('D', 'E', 2)
    g.adicionar_aresta('D', 'F', 6)
    g.adicionar_aresta('E', 'F', 3)
    
    print("Grafo:")
    print(g)
    print("\nBFS a partir de A:", AlgoritmosGrafos.bfs(g, 'A'))
    print("DFS a partir de A:", AlgoritmosGrafos.dfs(g, 'A'))
    
    # Dijkstra
    distancias, anteriores = AlgoritmosGrafos.dijkstra(g, 'A')
    print("\nDistâncias mínimas a partir de A:")
    for vertice, distancia in distancias.items():
        print(f"  {vertice}: {distancia}")
    
    print("\nCaminho mais curto de A para F:")
    caminho = AlgoritmosGrafos.caminho_mais_curto(anteriores, 'F')
    print(" -> ".join(caminho))
```

---

## 🌳 Árvores em Python

### Árvore Binária

```python
class NoArvore:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None
    
    def __str__(self):
        return str(self.valor)

class ArvoreBinaria:
    def __init__(self, raiz=None):
        self.raiz = raiz
    
    def inserir(self, valor):
        """Insere valor na árvore binária de busca."""
        if self.raiz is None:
            self.raiz = NoArvore(valor)
        else:
            self._inserir_recursivo(self.raiz, valor)
    
    def _inserir_recursivo(self, no, valor):
        if valor < no.valor:
            if no.esquerda is None:
                no.esquerda = NoArvore(valor)
            else:
                self._inserir_recursivo(no.esquerda, valor)
        else:
            if no.direita is None:
                no.direita = NoArvore(valor)
            else:
                self._inserir_recursivo(no.direita, valor)
    
    def buscar(self, valor):
        """Busca valor na árvore."""
        return self._buscar_recursivo(self.raiz, valor)
    
    def _buscar_recursivo(self, no, valor):
        if no is None or no.valor == valor:
            return no
        if valor < no.valor:
            return self._buscar_recursivo(no.esquerda, valor)
        return self._buscar_recursivo(no.direita, valor)
    
    def percurso_em_ordem(self):
        """Retorna valores em ordem (esquerda, raiz, direita)."""
        resultado = []
        self._em_ordem_recursivo(self.raiz, resultado)
        return resultado
    
    def _em_ordem_recursivo(self, no, resultado):
        if no:
            self._em_ordem_recursivo(no.esquerda, resultado)
            resultado.append(no.valor)
            self._em_ordem_recursivo(no.direita, resultado)
    
    def percurso_pre_ordem(self):
        """Retorna valores em pré-ordem (raiz, esquerda, direita)."""
        resultado = []
        self._pre_ordem_recursivo(self.raiz, resultado)
        return resultado
    
    def _pre_ordem_recursivo(self, no, resultado):
        if no:
            resultado.append(no.valor)
            self._pre_ordem_recursivo(no.esquerda, resultado)
            self._pre_ordem_recursivo(no.direita, resultado)
    
    def percurso_pos_ordem(self):
        """Retorna valores em pós-ordem (esquerda, direita, raiz)."""
        resultado = []
        self._pos_ordem_recursivo(self.raiz, resultado)
        return resultado
    
    def _pos_ordem_recursivo(self, no, resultado):
        if no:
            self._pos_ordem_recursivo(no.esquerda, resultado)
            self._pos_ordem_recursivo(no.direita, resultado)
            resultado.append(no.valor)
    
    def altura(self):
        """Calcula altura da árvore."""
        return self._altura_recursivo(self.raiz)
    
    def _altura_recursivo(self, no):
        if no is None:
            return -1
        altura_esq = self._altura_recursivo(no.esquerda)
        altura_dir = self._altura_recursivo(no.direita)
        return max(altura_esq, altura_dir) + 1
    
    def minimo(self):
        """Retorna valor mínimo na árvore."""
        atual = self.raiz
        while atual and atual.esquerda:
            atual = atual.esquerda
        return atual.valor if atual else None
    
    def maximo(self):
        """Retorna valor máximo na árvore."""
        atual = self.raiz
        while atual and atual.direita:
            atual = atual.direita
        return atual.valor if atual else None
    
    def remover(self, valor):
        """Remove valor da árvore."""
        self.raiz = self._remover_recursivo(self.raiz, valor)
    
    def _remover_recursivo(self, no, valor):
        if no is None:
            return no
        
        if valor < no.valor:
            no.esquerda = self._remover_recursivo(no.esquerda, valor)
        elif valor > no.valor:
            no.direita = self._remover_recursivo(no.direita, valor)
        else:
            # Nó com um ou nenhum filho
            if no.esquerda is None:
                return no.direita
            elif no.direita is None:
                return no.esquerda
            
            # Nó com dois filhos: pega o sucessor em ordem
            sucessor = self._minimo_no(no.direita)
            no.valor = sucessor.valor
            no.direita = self._remover_recursivo(no.direita, sucessor.valor)
        
        return no
    
    def _minimo_no(self, no):
        atual = no
        while atual.esquerda:
            atual = atual.esquerda
        return atual
    
    def eh_bst(self):
        """Verifica se a árvore é uma BST válida."""
        return self._eh_bst_recursivo(self.raiz, float('-inf'), float('inf'))
    
    def _eh_bst_recursivo(self, no, minimo, maximo):
        if no is None:
            return True
        
        if not (minimo < no.valor < maximo):
            return False
        
        return (self._eh_bst_recursivo(no.esquerda, minimo, no.valor) and
                self._eh_bst_recursivo(no.direita, no.valor, maximo))
    
    def nivel_por_nivel(self):
        """Retorna valores nível por nível (BFS)."""
        if not self.raiz:
            return []
        
        resultado = []
        fila = queue.Queue()
        fila.put(self.raiz)
        
        while not fila.empty():
            nivel = []
            tamanho_nivel = fila.qsize()
            
            for _ in range(tamanho_nivel):
                no = fila.get()
                nivel.append(no.valor)
                
                if no.esquerda:
                    fila.put(no.esquerda)
                if no.direita:
                    fila.put(no.direita)
            
            resultado.append(nivel)
        
        return resultado
    
    def __str__(self):
        """Representação visual da árvore."""
        linhas = []
        if self.raiz:
            self._desenhar_arvore(linhas, self.raiz, 0, "R")
        return "\n".join(linhas)
    
    def _desenhar_arvore(self, linhas, no, nivel, prefixo):
        if no is None:
            return
        
        linha = "  " * nivel + prefixo + ": " + str(no.valor)
        linhas.append(linha)
        
        if no.esquerda or no.direita:
            if no.esquerda:
                self._desenhar_arvore(linhas, no.esquerda, nivel + 1, "E")
            else:
                linhas.append("  " * (nivel + 1) + "E: None")
            
            if no.direita:
                self._desenhar_arvore(linhas, no.direita, nivel + 1, "D")
            else:
                linhas.append("  " * (nivel + 1) + "D: None")

# Exemplo de uso
if __name__ == "__main__":
    arvore = ArvoreBinaria()
    
    # Inserindo valores
    valores = [50, 30, 70, 20, 40, 60, 80, 10, 25, 35, 45]
    for valor in valores:
        arvore.inserir(valor)
    
    print("Árvore Binária:")
    print(arvore)
    
    print("\nPercursos:")
    print("Em ordem:", arvore.percurso_em_ordem())
    print("Pré-ordem:", arvore.percurso_pre_ordem())
    print("Pós-ordem:", arvore.percurso_pos_ordem())
    print("Nível por nível:", arvore.nivel_por_nivel())
    
    print("\nEstatísticas:")
    print("Altura:", arvore.altura())
    print("Mínimo:", arvore.minimo())
    print("Máximo:", arvore.maximo())
    print("É BST?", arvore.eh_bst())
    
    # Busca
    print("\nBusca por 40:", "Encontrado" if arvore.buscar(40) else "Não encontrado")
    print("Busca por 100:", "Encontrado" if arvore.buscar(100) else "Não encontrado")
    
    # Remoção
    print("\nRemovendo 30...")
    arvore.remover(30)
    print("Em ordem após remoção:", arvore.percurso_em_ordem())
```

### Árvore AVL (Auto-balanceada)

```python
class NoAVL:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None
        self.altura = 1

class ArvoreAVL:
    def __init__(self):
        self.raiz = None
    
    def altura(self, no):
        """Retorna altura do nó."""
        return no.altura if no else 0
    
    def balanceamento(self, no):
        """Calcula fator de balanceamento."""
        return self.altura(no.esquerda) - self.altura(no.direita) if no else 0
    
    def rotacao_direita(self, y):
        """Rotação simples à direita."""
        x = y.esquerda
        T2 = x.direita
        
        # Rotação
        x.direita = y
        y.esquerda = T2
        
        # Atualiza alturas
        y.altura = 1 + max(self.altura(y.esquerda), self.altura(y.direita))
        x.altura = 1 + max(self.altura(x.esquerda), self.altura(x.direita))
        
        return x
    
    def rotacao_esquerda(self, x):
        """Rotação simples à esquerda."""
        y = x.direita
        T2 = y.esquerda
        
        # Rotação
        y.esquerda = x
        x.direita = T2
        
        # Atualiza alturas
        x.altura = 1 + max(self.altura(x.esquerda), self.altura(x.direita))
        y.altura = 1 + max(self.altura(y.esquerda), self.altura(y.direita))
        
        return y
    
    def inserir(self, valor):
        """Insere valor na árvore AVL."""
        self.raiz = self._inserir_recursivo(self.raiz, valor)
    
    def _inserir_recursivo(self, no, valor):
        # 1. Inserção normal de BST
        if no is None:
            return NoAVL(valor)
        
        if valor < no.valor:
            no.esquerda = self._inserir_recursivo(no.esquerda, valor)
        elif valor > no.valor:
            no.direita = self._inserir_recursivo(no.direita, valor)
        else:
            return no  # Valores duplicados não são permitidos
        
        # 2. Atualiza altura
        no.altura = 1 + max(self.altura(no.esquerda), self.altura(no.direita))
        
        # 3. Calcula fator de balanceamento
        balance = self.balanceamento(no)
        
        # 4. Casos de desbalanceamento
        
        # Caso Esquerda-Esquerda
        if balance > 1 and valor < no.esquerda.valor:
            return self.rotacao_direita(no)
        
        # Caso Direita-Direita
        if balance < -1 and valor > no.direita.valor:
            return self.rotacao_esquerda(no)
        
        # Caso Esquerda-Direita
        if balance > 1 and valor > no.esquerda.valor:
            no.esquerda = self.rotacao_esquerda(no.esquerda)
            return self.rotacao_direita(no)
        
        # Caso Direita-Esquerda
        if balance < -1 and valor < no.direita.valor:
            no.direita = self.rotacao_direita(no.direita)
            return self.rotacao_esquerda(no)
        
        return no
    
    def percurso_em_ordem(self):
        """Retorna valores em ordem."""
        resultado = []
        self._em_ordem_recursivo(self.raiz, resultado)
        return resultado
    
    def _em_ordem_recursivo(self, no, resultado):
        if no:
            self._em_ordem_recursivo(no.esquerda, resultado)
            resultado.append(no.valor)
            self._em_ordem_recursivo(no.direita, resultado)
    
    def verificar_balanceamento(self):
        """Verifica se a árvore está balanceada."""
        return self._verificar_balanceamento_recursivo(self.raiz)
    
    def _verificar_balanceamento_recursivo(self, no):
        if no is None:
            return True
        
        balance = self.balanceamento(no)
        if abs(balance) > 1:
            return False
        
        return (self._verificar_balanceamento_recursivo(no.esquerda) and
                self._verificar_balanceamento_recursivo(no.direita))

# Exemplo de uso
if __name__ == "__main__":
    avl = ArvoreAVL()
    
    # Inserindo valores que causariam desbalanceamento em BST normal
    valores = [10, 20, 30, 40, 50, 25]
    
    print("Inserindo valores na AVL:")
    for valor in valores:
        avl.inserir(valor)
        print(f"Após inserir {valor}: {avl.percurso_em_ordem()}")
    
    print("\nÁrvore está balanceada?", avl.verificar_balanceamento())
```

### Heap Binário (Implementação Manual)

```python
class HeapBinario:
    def __init__(self, heap_min=True):
        self.heap = []
        self.heap_min = heap_min  # True para min-heap, False para max-heap
    
    def _comparar(self, a, b):
        """Compara dois elementos baseado no tipo de heap."""
        if self.heap_min:
            return a < b
        return a > b
    
    def _pai(self, i):
        """Retorna índice do pai."""
        return (i - 1) // 2
    
    def _filho_esquerdo(self, i):
        """Retorna índice do filho esquerdo."""
        return 2 * i + 1
    
    def _filho_direito(self, i):
        """Retorna índice do filho direito."""
        return 2 * i + 2
    
    def inserir(self, valor):
        """Insere valor no heap."""
        self.heap.append(valor)
        self._subir(len(self.heap) - 1)
    
    def _subir(self, i):
        """Move elemento para cima para manter propriedade do heap."""
        while i > 0 and self._comparar(self.heap[i], self.heap[self._pai(i)]):
            pai = self._pai(i)
            self.heap[i], self.heap[pai] = self.heap[pai], self.heap[i]
            i = pai
    
    def extrair_raiz(self):
        """Remove e retorna a raiz do heap."""
        if not self.heap:
            return None
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        raiz = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._descer(0)
        
        return raiz
    
    def _descer(self, i):
        """Move elemento para baixo para manter propriedade do heap."""
        tamanho = len(self.heap)
        
        while True:
            esquerdo = self._filho_esquerdo(i)
            direito = self._filho_direito(i)
            candidato = i
            
            if esquerdo < tamanho and self._comparar(self.heap[esquerdo], self.heap[candidato]):
                candidato = esquerdo
            
            if direito < tamanho and self._comparar(self.heap[direito], self.heap[candidato]):
                candidato = direito
            
            if candidato == i:
                break
            
            self.heap[i], self.heap[candidato] = self.heap[candidato], self.heap[i]
            i = candidato
    
    def ver_raiz(self):
        """Retorna raiz sem remover."""
        return self.heap[0] if self.heap else None
    
    def construir_heap(self, lista):
        """Constrói heap a partir de lista (heapify)."""
        self.heap = lista[:]
        
        # Começa do último nó não-folha
        for i in range(len(lista) // 2 - 1, -1, -1):
            self._descer(i)
    
    def tamanho(self):
        """Retorna tamanho do heap."""
        return len(self.heap)
    
    def vazio(self):
        """Verifica se heap está vazio."""
        return len(self.heap) == 0
    
    def __str__(self):
        return str(self.heap)

# Exemplo de uso
if __name__ == "__main__":
    print("Min-Heap:")
    min_heap = HeapBinario(heap_min=True)
    
    valores = [5, 3, 8, 1, 9, 2]
    for valor in valores:
        min_heap.inserir(valor)
    
    print("Heap:", min_heap)
    print("Extraindo:", min_heap.extrair_raiz())
    print("Heap após extração:", min_heap)
    
    print("\nConstruindo heap a partir de lista:")
    lista = [7, 3, 9, 1, 5, 2]
    min_heap.construir_heap(lista)
    print("Heap construído:", min_heap)
    
    print("\nExtraindo todos:")
    while not min_heap.vazio():
        print(min_heap.extrair_raiz(), end=" ")
    print()
```

---

## 🛠️ Estruturas de Dados Personalizadas

### 1. Tabela Hash (Hash Table)

```python
class TabelaHash:
    def __init__(self, capacidade=8, fator_carga=0.7):
        self.capacidade = capacidade
        self.fator_carga = fator_carga
        self.tamanho = 0
        self.tabela = [[] for _ in range(capacidade)]
    
    def _hash(self, chave):
        """Função hash simples."""
        return hash(chave) % self.capacidade
    
    def _rehash(self):
        """Redimensiona a tabela quando fator de carga é excedido."""
        antiga_tabela = self.tabela
        self.capacidade *= 2
        self.tabela = [[] for _ in range(self.capacidade)]
        self.tamanho = 0
        
        for bucket in antiga_tabela:
            for chave, valor in bucket:
                self.inserir(chave, valor)
    
    def inserir(self, chave, valor):
        """Insere par chave-valor na tabela hash."""
        # Verifica se precisa redimensionar
        if self.tamanho / self.capacidade >= self.fator_carga:
            self._rehash()
        
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        # Verifica se chave já existe
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                bucket[i] = (chave, valor)
                return
        
        # Chave não existe, adiciona nova
        bucket.append((chave, valor))
        self.tamanho += 1
    
    def buscar(self, chave):
        """Busca valor por chave."""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for k, v in bucket:
            if k == chave:
                return v
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def remover(self, chave):
        """Remove par chave-valor."""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                del bucket[i]
                self.tamanho -= 1
                return
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def __contains__(self, chave):
        """Verifica se chave existe."""
        try:
            self.buscar(chave)
            return True
        except KeyError:
            return False
    
    def __getitem__(self, chave):
        """Suporta acesso via []."""
        return self.buscar(chave)
    
    def __setitem__(self, chave, valor):
        """Suporta atribuição via []."""
        self.inserir(chave, valor)
    
    def __delitem__(self, chave):
        """Suporta deleção via del."""
        self.remover(chave)
    
    def __len__(self):
        """Retorna número de elementos."""
        return self.tamanho
    
    def keys(self):
        """Retorna todas as chaves."""
        chaves = []
        for bucket in self.tabela:
            for k, v in bucket:
                chaves.append(k)
        return chaves
    
    def values(self):
        """Retorna todos os valores."""
        valores = []
        for bucket in self.tabela:
            for k, v in bucket:
                valores.append(v)
        return valores
    
    def items(self):
        """Retorna todos os pares chave-valor."""
        itens = []
        for bucket in self.tabela:
            itens.extend(bucket)
        return itens
    
    def __str__(self):
        return "{" + ", ".join(f"{k}: {v}" for k, v in self.items()) + "}"

# Exemplo de uso
if __name__ == "__main__":
    th = TabelaHash(capacidade=4)
    
    # Inserindo valores
    th["nome"] = "João"
    th["idade"] = 30
    th["cidade"] = "São Paulo"
    th["profissao"] = "Engenheiro"
    
    print("Tabela Hash:", th)
    print("Tamanho:", len(th))
    print("Capacidade:", th.capacidade)
    
    # Acesso
    print("\nIdade:", th["idade"])
    print("Existe 'cidade'?", "cidade" in th)
    print("Existe 'email'?", "email" in th)
    
    # Atualização
    th["idade"] = 31
    print("\nIdade atualizada:", th["idade"])
    
    # Remoção
    del th["profissao"]
    print("\nApós remover 'profissao':", th)
    print("Novo tamanho:", len(th))
```

### 2. Trie (Árvore de Prefixos)

```python
class NoTrie:
    def __init__(self):
        self.filhos = {}
        self.fim_da_palavra = False
        self.frequencia = 0

class Trie:
    def __init__(self):
        self.raiz = NoTrie()
    
    def inserir(self, palavra):
        """Insere palavra na trie."""
        atual = self.raiz
        
        for letra in palavra:
            if letra not in atual.filhos:
                atual.filhos[letra] = NoTrie()
            atual = atual.filhos[letra]
        
        atual.fim_da_palavra = True
        atual.frequencia += 1
    
    def buscar(self, palavra):
        """Busca palavra na trie."""
        atual = self.raiz
        
        for letra in palavra:
            if letra not in atual.filhos:
                return False
            atual = atual.filhos[letra]
        
        return atual.fim_da_palavra
    
    def comeca_com(self, prefixo):
        """Verifica se alguma palavra começa com o prefixo."""
        atual = self.raiz
        
        for letra in prefixo:
            if letra not in atual.filhos:
                return False
            atual = atual.filhos[letra]
        
        return True
    
    def autocompletar(self, prefixo):
        """Retorna todas as palavras com o prefixo."""
        resultado = []
        
        # Navega até o nó do prefixo
        atual = self.raiz
        for letra in prefixo:
            if letra not in atual.filhos:
                return resultado
            atual = atual.filhos[letra]
        
        # Coleta todas as palavras a partir desse nó
        self._coletar_palavras(atual, prefixo, resultado)
        
        return resultado
    
    def _coletar_palavras(self, no, prefixo, resultado):
        """Coleta recursivamente todas as palavras a partir de um nó."""
        if no.fim_da_palavra:
            resultado.append((prefixo, no.frequencia))
        
        for letra, filho in no.filhos.items():
            self._coletar_palavras(filho, prefixo + letra, resultado)
    
    def remover(self, palavra):
        """Remove palavra da trie."""
        self._remover_recursivo(self.raiz, palavra, 0)
    
    def _remover_recursivo(self, no, palavra, indice):
        if indice == len(palavra):
            if not no.fim_da_palavra:
                return False  # Palavra não existe
            no.fim_da_palavra = False
            return len(no.filhos) == 0  # Pode ser removido se não tiver filhos
        
        letra = palavra[indice]
        if letra not in no.filhos:
            return False  # Palavra não existe
        
        pode_remover_filho = self._remover_recursivo(no.filhos[letra], palavra, indice + 1)
        
        if pode_remover_filho:
            del no.filhos[letra]
            return not no.fim_da_palavra and len(no.filhos) == 0
        
        return False
    
    def palavras_mais_frequentes(self, n=10):
        """Retorna as n palavras mais frequentes."""
        todas_palavras = []
        self._coletar_palavras(self.raiz, "", todas_palavras)
        
        # Ordena por frequência (decrescente) e retorna as n primeiras
        todas_palavras.sort(key=lambda x: x[1], reverse=True)
        return [palavra for palavra, freq in todas_palavras[:n]]
    
    def __contains__(self, palavra):
        """Suporta operador 'in'."""
        return self.buscar(palavra)
    
    def __str__(self):
        """Representação simplificada."""
        palavras = []
        self._coletar_palavras(self.raiz, "", palavras)
        return ", ".join(palavra for palavra, _ in palavras)

# Exemplo de uso
if __name__ == "__main__":
    trie = Trie()
    
    # Inserindo palavras
    palavras = ["python", "programação", "programa", "programador", 
                "java", "javascript", "c++", "c#", "php", "ruby"]
    
    for palavra in palavras:
        trie.inserir(palavra)
    
    # Inserindo algumas palavras múltiplas vezes
    for _ in range(3):
        trie.inserir("python")
    for _ in range(2):
        trie.inserir("java")
    
    print("Palavras na trie:", trie)
    
    # Buscas
    print("\nBusca 'python':", trie.buscar("python"))
    print("Busca 'pyth':", trie.buscar("pyth"))
    print("'python' in trie:", "python" in trie)
    
    # Prefixos
    print("\nComeça com 'prog':", trie.comeca_com("prog"))
    print("Começa com 'xyz':", trie.comeca_com("xyz"))
    
    # Autocompletar
    print("\nAutocompletar 'prog':", trie.autocompletar("prog"))
    print("Autocompletar 'ja':", trie.autocompletar("ja"))
    
    # Palavras mais frequentes
    print("\nPalavras mais frequentes:", trie.palavras_mais_frequentes(5))
    
    # Remoção
    print("\nRemovendo 'programa'...")
    trie.remover("programa")
    print("Autocompletar 'prog' após remoção:", trie.autocompletar("prog"))
```

### 3. Union-Find (Disjoint Set)

```python
class UnionFind:
    def __init__(self, tamanho):
        self.pai = list(range(tamanho))
        self.rank = [1] * tamanho
        self.conjuntos = tamanho
    
    def encontrar(self, x):
        """Encontra representante do conjunto com path compression."""
        if self.pai[x] != x:
            self.pai[x] = self.encontrar(self.pai[x])  # Path compression
        return self.pai[x]
    
    def unir(self, x, y):
        """Une dois conjuntos."""
        raiz_x = self.encontrar(x)
        raiz_y = self.encontrar(y)
        
        if raiz_x == raiz_y:
            return False  # Já estão no mesmo conjunto
        
        # Union by rank
        if self.rank[raiz_x] < self.rank[raiz_y]:
            self.pai[raiz_x] = raiz_y
        elif self.rank[raiz_x] > self.rank[raiz_y]:
            self.pai[raiz_y] = raiz_x
        else:
            self.pai[raiz_y] = raiz_x
            self.rank[raiz_x] += 1
        
        self.conjuntos -= 1
        return True
    
    def conectados(self, x, y):
        """Verifica se dois elementos estão no mesmo conjunto."""
        return self.encontrar(x) == self.encontrar(y)
    
    def contar_conjuntos(self):
        """Retorna número de conjuntos disjuntos."""
        return self.conjuntos
    
    def tamanho_conjunto(self, x):
        """Retorna tamanho do conjunto contendo x."""
        raiz = self.encontrar(x)
        return sum(1 for i in range(len(self.pai)) if self.encontrar(i) == raiz)
    
    def componentes(self):
        """Retorna lista de componentes conectados."""
        conjuntos = {}
        for i in range(len(self.pai)):
            raiz = self.encontrar(i)
            if raiz not in conjuntos:
                conjuntos[raiz] = []
            conjuntos[raiz].append(i)
        return list(conjuntos.values())

# Exemplo: Algoritmo de Kruskal para MST
def kruskal(vertices, arestas):
    """
    Algoritmo de Kruskal para encontrar Minimum Spanning Tree.
    
    Args:
        vertices: Lista de vértices
        arestas: Lista de (u, v, peso)
    
    Returns:
        Lista de arestas na MST
    """
    # Ordena arestas por peso
    arestas.sort(key=lambda x: x[2])
    
    uf = UnionFind(len(vertices))
    mst = []
    custo_total = 0
    
    for u, v, peso in arestas:
        if uf.unir(u, v):
            mst.append((vertices[u], vertices[v], peso))
            custo_total += peso
    
    return mst, custo_total

# Exemplo de uso
if __name__ == "__main__":
    # Exemplo 1: Union-Find básico
    print("=== Union-Find Básico ===")
    uf = UnionFind(5)
    
    print("Conjuntos inicialmente:", uf.contar_conjuntos())
    
    # Unindo elementos
    uf.unir(0, 1)
    uf.unir(2, 3)
    uf.unir(3, 4)
    
    print("Após uniões:")
    print("0 e 1 conectados?", uf.conectados(0, 1))  # True
    print("0 e 2 conectados?", uf.conectados(0, 2))  # False
    print("2 e 4 conectados?", uf.conectados(2, 4))  # True
    print("Conjuntos:", uf.contar_conjuntos())       # 2
    print("Componentes:", uf.componentes())          # [[0, 1], [2, 3, 4]]
    
    # Exemplo 2: Kruskal
    print("\n=== Algoritmo de Kruskal ===")
    
    vertices = ["A", "B", "C", "D", "E"]
    # (índice_u, índice_v, peso)
    arestas = [
        (0, 1, 4), (0, 2, 2), (1, 2, 1), (1, 3, 5),
        (2, 3, 8), (2, 4, 10), (3, 4, 2), (4, 0, 3)
    ]
    
    mst, custo = kruskal(vertices, arestas)
    
    print("Minimum Spanning Tree:")
    for u, v, peso in mst:
        print(f"  {u} -- {v} : {peso}")
    print(f"Custo total: {custo}")
```

### 4. LRU Cache (Least Recently Used)

```python
class NoLRU:
    def __init__(self, chave, valor):
        self.chave = chave
        self.valor = valor
        self.anterior = None
        self.proximo = None

class LRUCache:
    def __init__(self, capacidade):
        self.capacidade = capacidade
        self.cache = {}
        self.head = NoLRU(0, 0)  # Dummy head
        self.tail = NoLRU(0, 0)  # Dummy tail
        self.head.proximo = self.tail
        self.tail.anterior = self.head
        self.tamanho = 0
    
    def _adicionar_no(self, no):
        """Adiciona nó logo após o head."""
        no.proximo = self.head.proximo
        no.anterior = self.head
        self.head.proximo.anterior = no
        self.head.proximo = no
    
    def _remover_no(self, no):
        """Remove nó da lista."""
        no.anterior.proximo = no.proximo
        no.proximo.anterior = no.anterior
    
    def _mover_para_frente(self, no):
        """Move nó para frente (mais recente)."""
        self._remover_no(no)
        self._adicionar_no(no)
    
    def _remover_mais_antigo(self):
        """Remove o nó mais antigo (antes do tail)."""
        mais_antigo = self.tail.anterior
        self._remover_no(mais_antigo)
        del self.cache[mais_antigo.chave]
        self.tamanho -= 1
    
    def get(self, chave):
        """Obtém valor da cache."""
        if chave not in self.cache:
            return None
        
        no = self.cache[chave]
        self._mover_para_frente(no)
        return no.valor
    
    def put(self, chave, valor):
        """Adiciona ou atualiza valor na cache."""
        if chave in self.cache:
            # Atualiza valor existente
            no = self.cache[chave]
            no.valor = valor
            self._mover_para_frente(no)
        else:
            # Adiciona novo nó
            if self.tamanho >= self.capacidade:
                self._remover_mais_antigo()
            
            novo_no = NoLRU(chave, valor)
            self.cache[chave] = novo_no
            self._adicionar_no(novo_no)
            self.tamanho += 1
    
    def __contains__(self, chave):
        return chave in self.cache
    
    def __len__(self):
        return self.tamanho
    
    def items(self):
        """Retorna itens em ordem de uso (mais recente primeiro)."""
        resultado = []
        atual = self.head.proximo
        while atual != self.tail:
            resultado.append((atual.chave, atual.valor))
            atual = atual.proximo
        return resultado
    
    def __str__(self):
        return " -> ".join(f"{k}:{v}" for k, v in self.items())

# Exemplo de uso
if __name__ == "__main__":
    cache = LRUCache(3)
    
    print("Inserindo valores...")
    cache.put("A", 1)
    cache.put("B", 2)
    cache.put("C", 3)
    print(f"Cache: {cache}")
    
    print("\nAcessando 'A'...")
    cache.get("A")
    print(f"Cache após acessar A: {cache}")
    
    print("\nInserindo 'D' (deve remover 'B' que é menos usado)...")
    cache.put("D", 4)
    print(f"Cache após inserir D: {cache}")
    
    print("\nAtualizando 'C'...")
    cache.put("C", 33)
    print(f"Cache após atualizar C: {cache}")
    
    print("\nVerificações:")
    print(f"'A' na cache? {'A' in cache}")
    print(f"'B' na cache? {'B' in cache}")
    print(f"Valor de 'C': {cache.get('C')}")
    print(f"Valor de 'X': {cache.get('X')}")
    print(f"Tamanho da cache: {len(cache)}")
```

---

## 📊 Análise de Complexidade

### Tabela de Complexidade de Operações

```python
"""
ESTRUTURA            | ACESSO | BUSCA | INSERÇÃO | REMOÇÃO | ESPAÇO
--------------------|--------|-------|----------|---------|--------
Lista (List)        | O(1)   | O(n)  | O(n)     | O(n)    | O(n)
Array (array)       | O(1)   | O(n)  | O(n)     | O(n)    | O(n)
Deque (collections) | O(1)   | O(n)  | O(1)     | O(1)    | O(n)
--------------------|--------|-------|----------|---------|--------
Dicionário (Dict)   | O(1)   | O(1)  | O(1)     | O(1)    | O(n)
Conjunto (Set)      | O(1)   | O(1)  | O(1)     | O(1)    | O(n)
--------------------|--------|-------|----------|---------|--------
Heap (heapq)        | O(1)*  | O(n)  | O(log n) | O(log n)| O(n)
Fila (queue)        | O(1)   | O(n)  | O(1)     | O(1)    | O(n)
Pilha (LIFOQueue)   | O(1)   | O(n)  | O(1)     | O(1)    | O(n)
--------------------|--------|-------|----------|---------|--------
Árvore Binária (BST)| O(log n)|O(log n)|O(log n)| O(log n)| O(n)
Árvore AVL          | O(log n)|O(log n)|O(log n)| O(log n)| O(n)
Trie                | O(L)   | O(L)  | O(L)     | O(L)    | O(N*L)
--------------------|--------|-------|----------|---------|--------
Tabela Hash (custom)| O(1)   | O(1)  | O(1)     | O(1)    | O(n)
Union-Find          | O(α(n))|O(α(n))| O(α(n))  | O(α(n)) | O(n)

Legenda:
- L: Comprimento da string/prefixo
- N: Número de strings
- α(n): Função inversa de Ackermann (praticamente constante)
- *: Heap acessa apenas o menor elemento em O(1)
"""

# Exemplo prático de escolha de estrutura de dados
def escolher_estrutura(requisitos):
    """
    Guia para escolher estrutura de dados baseado nos requisitos.
    
    Args:
        requisitos: Dicionário com requisitos
    
    Returns:
        Estrutura de dados recomendada
    """
    recomendacoes = []
    
    if requisitos.get("acesso_frequente_por_indice"):
        recomendacoes.append("Lista (List)")
    
    if requisitos.get("dados_numericos_grandes"):
        recomendacoes.append("Array (array module)")
    
    if requisitos.get("insercoes_remocoes_rapidas_ambas_pontas"):
        recomendacoes.append("Deque (collections.deque)")
    
    if requisitos.get("mapeamento_chave_valor"):
        recomendacoes.append("Dicionário (Dict)")
        if requisitos.get("valor_padrao_chaves_inexistentes"):
            recomendacoes.append("defaultdict")
    
    if requisitos.get("elementos_unicos"):
        recomendacoes.append("Conjunto (Set)")
    
    if requisitos.get("fila_prioridade"):
        recomendacoes.append("Heap (heapq)")
    
    if requisitos.get("thread_safe"):
        recomendacoes.append("Queue (queue module)")
    
    if requisitos.get("manter_ordem_insercao"):
        recomendacoes.append("OrderedDict (Python <3.7) ou Dict (Python 3.7+)")
    
    if requisitos.get("contagem_frequencia"):
        recomendacoes.append("Counter (collections)")
    
    if requisitos.get("busca_prefixo"):
        recomendacoes.append("Trie")
    
    if requisitos.get("conjuntos_disjuntos"):
        recomendacoes.append("Union-Find")
    
    if requisitos.get("cache_lru"):
        recomendacoes.append("OrderedDict ou LRUCache customizado")
    
    return recomendacoes if recomendacoes else ["Lista (padrão)"]

# Exemplo de uso
requisitos_exemplo = {
    "mapeamento_chave_valor": True,
    "acesso_frequente_por_indice": False,
    "insercoes_remocoes_rapidas": True,
    "manter_ordem_insercao": True
}

print("Estruturas recomendadas:", escolher_estrutura(requisitos_exemplo))
```

---

## ⚡ Benchmark e Performance

```python
import time
import random
from collections import deque, Counter, defaultdict
import array
import heapq
import bisect

def benchmark_insercao_inicio():
    """Compara inserção no início de diferentes estruturas."""
    tamanho = 10000
    
    # Lista
    inicio = time.time()
    lista = []
    for i in range(tamanho):
        lista.insert(0, i)  # O(n) cada inserção
    tempo_lista = time.time() - inicio
    
    # Deque
    inicio = time.time()
    dq = deque()
    for i in range(tamanho):
        dq.appendleft(i)  # O(1) cada inserção
    tempo_deque = time.time() - inicio
    
    print(f"Inserção no início ({tamanho} elementos):")
    print(f"  Lista: {tempo_lista:.4f}s")
    print(f"  Deque: {tempo_deque:.4f}s")
    print(f"  Deque é {tempo_lista/tempo_deque:.1f}x mais rápido")
    return tempo_lista, tempo_deque

def benchmark_busca():
    """Compara busca em diferentes estruturas."""
    tamanho = 100000
    
    # Preparando dados
    dados = list(range(tamanho))
    conjunto = set(dados)
    dicionario = {i: i for i in dados}
    
    # Buscando elementos aleatórios
    busca = [random.randint(0, tamanho-1) for _ in range(1000)]
    
    # Lista
    inicio = time.time()
    for item in busca:
        encontrado = item in dados  # O(n)
    tempo_lista = time.time() - inicio
    
    # Conjunto
    inicio = time.time()
    for item in busca:
        encontrado = item in conjunto  # O(1)
    tempo_conjunto = time.time() - inicio
    
    # Dicionário (chaves)
    inicio = time.time()
    for item in busca:
        encontrado = item in dicionario  # O(1)
    tempo_dicionario = time.time() - inicio
    
    print(f"\nBusca ({len(busca)} buscas em {tamanho} elementos):")
    print(f"  Lista:      {tempo_lista:.6f}s")
    print(f"  Conjunto:   {tempo_conjunto:.6f}s")
    print(f"  Dicionário: {tempo_dicionario:.6f}s")
    print(f"  Conjunto é {tempo_lista/tempo_conjunto:.0f}x mais rápido que lista")
    return tempo_lista, tempo_conjunto, tempo_dicionario

def benchmark_memoria():
    """Compara uso de memória."""
    import sys
    
    tamanho = 100000
    
    # Lista de inteiros
    lista = list(range(tamanho))
    memoria_lista = sys.getsizeof(lista)
    
    # Array de inteiros
    arr = array.array('i', range(tamanho))
    memoria_array = sys.getsizeof(arr)
    
    # Conjunto
    conjunto = set(range(tamanho))
    memoria_conjunto = sys.getsizeof(conjunto)
    
    print(f"\nUso de memória ({tamanho} elementos):")
    print(f"  Lista:   {memoria_lista:,} bytes")
    print(f"  Array:   {memoria_array:,} bytes")
    print(f"  Conjunto: {memoria_conjunto:,} bytes")
    print(f"  Array usa {memoria_lista/memoria_array:.1f}x menos memória que lista")
    return memoria_lista, memoria_array, memoria_conjunto

def benchmark_ordenacao():
    """Compara diferentes formas de manter dados ordenados."""
    tamanho = 10000
    dados = [random.randint(0, 100000) for _ in range(tamanho)]
    
    # Lista + sort
    inicio = time.time()
    lista = []
    for valor in dados:
        lista.append(valor)
        lista.sort()  # O(n log n) cada vez
    tempo_lista_sort = time.time() - inicio
    
    # Lista + bisect
    inicio = time.time()
    lista_ordenada = []
    for valor in dados:
        bisect.insort(lista_ordenada, valor)  # O(n) cada inserção
    tempo_bisect = time.time() - inicio
    
    # Heap
    inicio = time.time()
    heap = []
    for valor in dados:
        heapq.heappush(heap, valor)  # O(log n) cada inserção
    tempo_heap = time.time() - inicio
    
    print(f"\nManter dados ordenados ({tamanho} inserções):")
    print(f"  Lista + sort:   {tempo_lista_sort:.4f}s")
    print(f"  Lista + bisect: {tempo_bisect:.4f}s")
    print(f"  Heap:           {tempo_heap:.4f}s")
    print(f"  Heap é {tempo_bisect/tempo_heap:.1f}x mais rápido que bisect")
    return tempo_lista_sort, tempo_bisect, tempo_heap

def benchmark_contagem():
    """Compara contagem de elementos."""
    tamanho = 100000
    dados = [random.randint(0, 100) for _ in range(tamanho)]
    
    # Manual com dicionário
    inicio = time.time()
    contagem_manual = {}
    for valor in dados:
        contagem_manual[valor] = contagem_manual.get(valor, 0) + 1
    tempo_manual = time.time() - inicio
    
    # DefaultDict
    inicio = time.time()
    contagem_default = defaultdict(int)
    for valor in dados:
        contagem_default[valor] += 1
    tempo_default = time.time() - inicio
    
    # Counter
    inicio = time.time()
    contagem_counter = Counter(dados)
    tempo_counter = time.time() - inicio
    
    print(f"\nContagem de frequência ({tamanho} elementos):")
    print(f"  Manual:     {tempo_manual:.4f}s")
    print(f"  DefaultDict: {tempo_default:.4f}s")
    print(f"  Counter:    {tempo_counter:.4f}s")
    print(f"  Counter é {tempo_manual/tempo_counter:.1f}x mais rápido que manual")
    return tempo_manual, tempo_default, tempo_counter

def recomendar_estrutura_por_uso(uso, tamanho_dados, operacoes):
    """
    Recomenda estrutura de dados baseada no uso.
    
    Args:
        uso: 'busca', 'insercao', 'remocao', 'ordenacao', 'contagem', 'cache'
        tamanho_dados: 'pequeno', 'medio', 'grande'
        operacoes: Lista de operações principais
    
    Returns:
        String com recomendação
    """
    recomendacoes = {
        ('busca', 'grande', ['frequente']): "Conjunto (Set) ou Dicionário (Dict)",
        ('busca', 'grande', ['por_indice']): "Lista (List) ou Array",
        ('busca', 'grande', ['prefixo']): "Trie",
        
        ('insercao', 'grande', ['inicio_fim']): "Deque (collections.deque)",
        ('insercao', 'grande', ['posicao_especifica']): "Lista (List)",
        ('insercao', 'grande', ['mantendo_ordenado']): "Heap (heapq) ou bisect",
        
        ('remocao', 'grande', ['inicio_fim']): "Deque (collections.deque)",
        ('remocao', 'grande', ['por_chave']): "Dicionário (Dict)",
        ('remocao', 'grande', ['frequente']): "Conjunto (Set)",
        
        ('ordenacao', 'grande', ['manter_sempre']): "bisect.insort ou Heap",
        ('ordenacao', 'grande', ['ocasional']): "Lista + .sort() ou sorted()",
        
        ('contagem', 'grande', ['frequencia']): "Counter (collections)",
        ('contagem', 'grande', ['agrupamento']): "defaultdict(list)",
        
        ('cache', 'medio', ['lru']): "OrderedDict ou LRUCache customizado",
        ('cache', 'medio', ['fifo']): "deque ou queue.Queue",
    }
    
    chave = (uso, tamanho_dados, tuple(sorted(operacoes)))
    return recomendacoes.get(chave, "Lista (estrutura padrão versátil)")

# Executando benchmarks
if __name__ == "__main__":
    print("=" * 60)
    print("BENCHMARK DE ESTRUTURAS DE DADOS EM PYTHON")
    print("=" * 60)
    
    benchmark_insercao_inicio()
    benchmark_busca()
    benchmark_memoria()
    benchmark_ordenacao()
    benchmark_contagem()
    
    print("\n" + "=" * 60)
    print("RECOMENDAÇÕES POR CASO DE USO")
    print("=" * 60)
    
    casos = [
        ("Busca frequente em grandes datasets", ('busca', 'grande', ['frequente'])),
        ("Inserções/remoções no início/fim", ('insercao', 'grande', ['inicio_fim'])),
        ("Manter dados sempre ordenados", ('ordenacao', 'grande', ['mantendo_ordenado'])),
        ("Contar frequência de elementos", ('contagem', 'grande', ['frequencia'])),
        ("Cache LRU (Least Recently Used)", ('cache', 'medio', ['lru'])),
    ]
    
    for descricao, parametros in casos:
        recomendacao = recomendar_estrutura_por_uso(*parametros)
        print(f"{descricao}:")
        print(f"  → {recomendacao}")
        print()
```

---

## 📚 Conclusão

### Resumo das Estruturas de Dados em Python

1. **Para coleções ordenadas**: 
   - Use `list` para acesso por índice
   - Use `deque` para inserções/remoções nas extremidades
   - Use `array` para dados numéricos homogêneos grandes

2. **Para mapeamentos**:
   - Use `dict` para mapeamento chave-valor padrão
   - Use `defaultdict` para valores padrão automáticos
   - Use `Counter` para contagem de frequência
   - Use `OrderedDict` para manter ordem (Python < 3.7)

3. **Para conjuntos**:
   - Use `set` para elementos únicos e operações de conjunto
   - Use `frozenset` para conjuntos imutáveis

4. **Para dados ordenados**:
   - Use `heapq` para filas de prioridade
   - Use `bisect` para manter listas ordenadas

5. **Para concorrência**:
   - Use `queue.Queue` para filas thread-safe

6. **Para estruturas avançadas**:
   - Implemente `Trie` para buscas de prefixo
   - Implemente `Union-Find` para conjuntos disjuntos
   - Implemente `Árvores Balanceadas` para dados ordenados dinâmicos
   - Implemente `Tabelas Hash` personalizadas para casos específicos

### Boas Práticas

1. **Escolha a estrutura certa**: Pense nas operações mais comuns
2. **Use estruturas especializadas**: `collections`, `heapq`, `bisect`
3. **Considere a complexidade**: Entenda Big O para operações críticas
4. **Teste performance**: Use `timeit` para benchmarks reais
5. **Documente escolhas**: Explique por que escolheu cada estrutura

### Próximos Passos

1. **Explore bibliotecas especializadas**:
   - `sortedcontainers`: Implementações eficientes de estruturas ordenadas
   - `blist`: Listas com melhor performance para certas operações
   - `bintrees`: Árvores binárias balanceadas

2. **Estude implementações internas**:
   - Leia o código-fonte das estruturas do Python
   - Entenda como `dict` e `set` usam hashing

3. **Pratique com problemas**:
   - LeetCode, HackerRank, CodeSignal
   - Implemente estruturas do zero
   - Compare diferentes abordagens

4. **Aprofunde em algoritmos**:
   - Algoritmos de ordenação
   - Algoritmos de busca em grafos
   - Algoritmos de compressão

Lembre-se: **a melhor estrutura de dados depende do problema específico**. Conhecer as opções disponíveis e suas características é fundamental para escrever código Python eficiente e elegante.
