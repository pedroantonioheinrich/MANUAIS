# 📚 MANUAL COMPLETO DE PYTHON

## 📋 ÍNDICE
1. [Fundamentos da Linguagem](#fundamentos-da-linguagem)
2. [Tipos de Dados](#tipos-de-dados)
3. [Estruturas de Controle](#estruturas-de-controle)
4. [Funções e Módulos](#funções-e-módulos)
5. [Programação Orientada a Objetos](#programação-orientada-a-objetos)
6. [Manipulação de Arquivos](#manipulação-de-arquivos)
7. [Tratamento de Exceções](#tratamento-de-exceções)
8. [Módulos da Biblioteca Padrão](#módulos-da-biblioteca-padrão)
9. [Expressões Regulares](#expressões-regulares)
10. [Concorrência e Paralelismo](#concorrência-e-paralelismo)
11. [Metaprogramação](#metaprogramação)
12. [Boas Práticas e Padrões](#boas-práticas-e-padrões)
13. [Ferramentas do Ecossistema](#ferramentas-do-ecossistema)

---

## 🎯 FUNDAMENTOS DA LINGUAGEM

### Filosofia Python (Zen do Python)
```python
import this
"""
Bonito é melhor que feio.
Explícito é melhor que implícito.
Simples é melhor que complexo.
Complexo é melhor que complicado.
...
"""
```

### Interpretador Python
```bash
# Modo interativo
python
python -i script.py

# Executar script
python script.py
python -m modulo

# Opções úteis
python -c "print('Hello World')"  # Executar comando
python -m http.server 8000        # Servidor HTTP simples
python -m pdb script.py           # Debugger
python -O script.py              # Otimizações básicas
python -OO script.py             # Otimizações agressivas
```

### Sintaxe Básica
```python
# Comentários
# Isto é um comentário de linha única
"""Isto é um docstring - documentação de módulo/classe/função"""
'''Também pode ser usado para multi-linhas'''

# Indentação (CRÍTICO em Python)
if True:
    print("Indentado com 4 espaços")  # Padrão PEP 8
    x = 1

# Ponto e vírgula é opcional (geralmente omitido)
x = 1; y = 2  # Possível, mas não idiomático

# Continuar linha
resultado = (valor1 + 
             valor2 + 
             valor3)

# Ou com \
resultado = valor1 + \
            valor2 + \
            valor3
```

### Variáveis e Atribuição
```python
# Atribuição simples
x = 10
nome = "Python"

# Atribuição múltipla
a, b, c = 1, 2, 3

# Troca de valores
a, b = b, a

# Atribuição encadeada
x = y = z = 0

# Desempacotamento
valores = [1, 2, 3]
x, y, z = valores

# Desempacotamento com * (Python 3+)
primeiro, *meio, ultimo = [1, 2, 3, 4, 5]
# primeiro = 1, meio = [2, 3, 4], ultimo = 5

# Atribuição aumentada
x += 1      # x = x + 1
x -= 1      # x = x - 1
x *= 2      # x = x * 2
x /= 2      # x = x / 2
x //= 2     # x = x // 2
x %= 2      # x = x % 2
x **= 2     # x = x ** 2
```

---

## 📊 TIPOS DE DADOS

### Tipos Numéricos
```python
# Inteiros (int)
x = 10
y = -5
z = 0
grande = 10_000_000  # Underscore para legibilidade (Python 3.6+)
binario = 0b1010     # 10 em binário
octal = 0o12         # 10 em octal
hexa = 0xA           # 10 em hexadecimal

# Ponto flutuante (float)
pi = 3.14159
negativo = -2.5
cientifico = 1.23e-4  # 0.000123
infinito = float('inf')
nao_numero = float('nan')

# Complexos (complex)
c = 3 + 4j
c.real  # 3.0
c.imag  # 4.0
c.conjugate()  # (3-4j)

# Decimal (precisão exata)
from decimal import Decimal, getcontext
getcontext().prec = 28  # Precisão de 28 dígitos
d = Decimal('0.1') + Decimal('0.2')  # 0.3 exato

# Frações
from fractions import Fraction
f = Fraction(1, 3)  # 1/3
```

### Booleanos
```python
verdadeiro = True
falso = False

# Valores que são False em contexto booleano
False, None, 0, 0.0, 0j, '', (), [], {}, set(), range(0)

# Conversão
bool(0)     # False
bool(1)     # True
bool("")    # False
bool("a")   # True
bool([])    # False
bool([1])   # True
```

### Strings
```python
# Criação
s1 = 'aspas simples'
s2 = "aspas duplas"
s3 = '''três aspas simples
para múltiplas linhas'''
s4 = """três aspas duplas
também funciona"""

# Strings raw (ignora escapes)
path = r'C:\Users\nome\arquivo.txt'

# Strings f (Python 3.6+)
nome = "Mundo"
f"Olá, {nome}!"  # Olá, Mundo!
f"Valor: {10 * 2}"  # Valor: 20
f"Pi: {3.14159:.2f}"  # Pi: 3.14

# Métodos comuns
texto = " Python é incrível! "
texto.strip()           # "Python é incrível!"
texto.lower()           # " python é incrível! "
texto.upper()           # " PYTHON É INCRÍVEL! "
texto.title()           # " Python É Incrível! "
texto.split()           # ['Python', 'é', 'incrível!']
" ".join(['a', 'b'])    # "a b"
texto.replace("é", "eh") # " Python eh incrível! "
texto.find("Python")    # 1
texto.startswith(" ")   # True
texto.endswith("! ")    # True
texto.isalpha()         # False (tem espaços)
texto.isdigit()         # False

# Indexação e slicing
s = "Python"
s[0]      # 'P'
s[-1]     # 'n' (último)
s[1:4]    # 'yth' (slice)
s[::2]    # 'Pto' (pulando de 2 em 2)
s[::-1]   # 'nohtyP' (invertido)

# Formatação antiga
"Olá, %s!" % "Mundo"               # %-formatting
"Valor: {0:.2f}".format(3.14159)   # str.format()
```

### Bytes e Bytearrays
```python
# Bytes (imutável)
b = b'hello'
b = bytes([104, 101, 108, 108, 111])

# Bytearray (mutável)
ba = bytearray(b'hello')
ba[0] = 106  # jello

# Conversões
texto = "Python"
bytes_texto = texto.encode('utf-8')  # b'Python'
bytes_texto.decode('utf-8')          # 'Python'
```

### None
```python
valor = None  # Representa ausência de valor

if valor is None:
    print("Valor não definido")
    
# None é singleton - sempre use 'is' para comparar
x = None
x is None   # True
x == None   # True (mas não idiomático)
```

---

## 🧮 ESTRUTURAS DE DADOS

### Listas
```python
# Criação
lista = [1, 2, 3, 4, 5]
lista_vazia = []
lista_mista = [1, "dois", 3.0, [4, 5]]

# List comprehension
quadrados = [x**2 for x in range(10)]
pares = [x for x in range(10) if x % 2 == 0]

# Operações
lista.append(6)           # [1, 2, 3, 4, 5, 6]
lista.extend([7, 8])      # [1, 2, 3, 4, 5, 6, 7, 8]
lista.insert(0, 0)        # [0, 1, 2, 3, 4, 5, 6, 7, 8]
lista.remove(3)           # Remove primeiro 3
valor = lista.pop()       # Remove e retorna último (8)
valor = lista.pop(0)      # Remove e retorna primeiro (0)
lista.index(4)            # Índice do valor 4
lista.count(2)            # Quantas vezes 2 aparece
lista.sort()              # Ordena in-place
lista.reverse()           # Inverte in-place
lista.copy()              # Cópia superficial
lista.clear()             # Remove todos

# Slicing
lista = [0, 1, 2, 3, 4, 5]
lista[2:5]      # [2, 3, 4]
lista[:3]       # [0, 1, 2]
lista[3:]       # [3, 4, 5]
lista[::2]      # [0, 2, 4]
lista[::-1]     # [5, 4, 3, 2, 1, 0]

# Desempacotamento
primeiro, *resto = [1, 2, 3, 4, 5]
```

### Tuplas
```python
# Criação (imutável)
tupla = (1, 2, 3)
tupla_sem_parenteses = 1, 2, 3
tupla_unitaria = (1,)  # Note a vírgula!
tupla_vazia = ()

# Métodos limitados (por ser imutável)
tupla.index(2)  # 1
tupla.count(3)  # 1

# Uso comum: retorno múltiplo de funções
def coordenadas():
    return 10, 20

x, y = coordenadas()

# Namedtuple
from collections import namedtuple
Ponto = namedtuple('Ponto', ['x', 'y'])
p = Ponto(10, 20)
p.x, p.y  # (10, 20)
```

### Dicionários
```python
# Criação
dicio = {'nome': 'João', 'idade': 30}
dicio = dict(nome='João', idade=30)
dicio_vazio = {}

# Dict comprehension
quadrados = {x: x**2 for x in range(5)}

# Operações
dicio['nome']                # Acesso
dicio.get('nome')            # Acesso seguro
dicio.get('altura', 180)     # Valor padrão se não existir
dicio['cidade'] = 'SP'       # Adicionar/Atualizar
dicio.update({'pais': 'BR'}) # Atualizar múltiplos
del dicio['idade']           # Remover
valor = dicio.pop('nome')    # Remover e retornar
valor = dicio.popitem()      # Remove e retorna último par
'idade' in dicio             # Verificar chave
len(dicio)                   # Tamanho

# Métodos
dicio.keys()     # Visualização das chaves
dicio.values()   # Visualização dos valores
dicio.items()    # Visualização dos pares (chave, valor)
dicio.copy()     # Cópia superficial
dicio.clear()    # Limpar

# Iteração
for chave in dicio:
    print(chave, dicio[chave])

for chave, valor in dicio.items():
    print(chave, valor)

# DefaultDict
from collections import defaultdict
dd = defaultdict(list)  # Valor padrão é lista vazia
dd['grupo'].append(1)

# OrderedDict (mantém ordem de inserção)
from collections import OrderedDict
od = OrderedDict()
od['z'] = 1
od['a'] = 2

# Counter
from collections import Counter
contador = Counter(['a', 'b', 'a', 'c', 'b', 'a'])
contador.most_common(2)  # [('a', 3), ('b', 2)]
```

### Conjuntos (Sets)
```python
# Criação
conjunto = {1, 2, 3}
conjunto = set([1, 2, 2, 3])  # {1, 2, 3}
conjunto_vazio = set()

# Set comprehension
{ x for x in 'abracadabra' if x not in 'abc' }  # {'r', 'd'}

# Operações
conjunto.add(4)           # Adicionar elemento
conjunto.remove(3)        # Remover (erro se não existir)
conjunto.discard(3)       # Remover (sem erro)
valor = conjunto.pop()    # Remove e retorna elemento arbitrário
conjunto.clear()          # Limpar
len(conjunto)             # Tamanho
2 in conjunto             # Verificar membro

# Operações de conjunto
a = {1, 2, 3}
b = {3, 4, 5}

a | b   # União: {1, 2, 3, 4, 5}
a & b   # Interseção: {3}
a - b   # Diferença: {1, 2}
a ^ b   # Diferença simétrica: {1, 2, 4, 5}

a <= b  # Subconjunto
a < b   # Subconjunto próprio
a >= b  # Superconjunto
a > b   # Superconjunto próprio

# Frozenset (imutável)
fs = frozenset([1, 2, 3])
```

### Arrays e Deques
```python
# Array (eficiente para tipos numéricos homogêneos)
from array import array
arr = array('i', [1, 2, 3, 4, 5])  # 'i' = inteiro com sinal

# Deque (lista de dupla extremidade)
from collections import deque
dq = deque([1, 2, 3])
dq.appendleft(0)    # [0, 1, 2, 3]
dq.append(4)        # [0, 1, 2, 3, 4]
dq.popleft()        # 0
dq.pop()            # 4
dq.rotate(1)        # [3, 1, 2]
dq.rotate(-1)       # [1, 2, 3]
```

---

## 🔄 ESTRUTURAS DE CONTROLE

### Condicionais (if/elif/else)
```python
# Básico
if condicao:
    # bloco
elif outra_condicao:
    # bloco
else:
    # bloco

# Operador ternário
valor = "sim" if condicao else "não"

# Expressão if em compreensões
[x for x in range(10) if x % 2 == 0]

# Atribuição condicional (Python 3.8+)
if (n := len(lista)) > 10:
    print(f"Lista grande: {n} elementos")

# Comparações encadeadas
if 0 < x < 10:  # Equivalente a 0 < x and x < 10
    print("x entre 0 e 10")
```

### Laços (Loops)

#### for
```python
# Iteração sobre sequência
for i in range(5):
    print(i)

for item in lista:
    print(item)

for chave, valor in dicio.items():
    print(chave, valor)

# Com enumerate (índice e valor)
for indice, valor in enumerate(lista):
    print(indice, valor)

# Com zip (iterar múltiplas sequências)
for a, b in zip(lista1, lista2):
    print(a, b)

# for/else (executa else se não houve break)
for item in lista:
    if item == alvo:
        break
else:
    print("Item não encontrado")
```

#### while
```python
# Loop while básico
while condicao:
    # bloco

# while/else
while condicao:
    # bloco
    if outra_condicao:
        break
else:
    print("Loop terminado normalmente")

# Loop infinito
while True:
    # bloco
    if condicao_saida:
        break
```

#### Controle de Fluxo
```python
# break - sai do loop mais interno
for i in range(10):
    if i == 5:
        break

# continue - pula para próxima iteração
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # Apenas ímpares

# pass - placeholder (não faz nada)
if condicao:
    pass  # Implementar depois
```

---

## 🧩 FUNÇÕES

### Definição Básica
```python
def nome_funcao(parametro1, parametro2):
    """Docstring da função"""
    # Corpo da função
    return resultado

# Função sem retorno (retorna None)
def saudacao(nome):
    print(f"Olá, {nome}!")

# Múltiplos retornos (como tupla)
def operacoes(x, y):
    return x+y, x-y, x*y, x/y

soma, diff, mult, div = operacoes(10, 5)
```

### Parâmetros

#### Tipos de Parâmetros
```python
# Parâmetro posicional
def funcao(posicional):
    pass

# Parâmetro com valor padrão
def funcao(parametro=valor_padrao):
    pass

# Parâmetros *args (posicionais arbitrários)
def funcao(*args):
    # args é uma tupla
    for arg in args:
        print(arg)

# Parâmetros **kwargs (keywords arbitrários)
def funcao(**kwargs):
    # kwargs é um dicionário
    for chave, valor in kwargs.items():
        print(chave, valor)

# Combinação completa
def funcao(posicional, padrao=10, *args, **kwargs):
    pass
```

#### Modos de Chamada
```python
# Chamada posicional
funcao(valor1, valor2)

# Chamada por keyword
funcao(parametro2=valor2, parametro1=valor1)

# Desempacotamento
valores = [1, 2, 3]
funcao(*valores)  # Desempacota lista/tupla

parametros = {'a': 1, 'b': 2}
funcao(**parametros)  # Desempacota dicionário
```

### Funções como Objetos
```python
# Funções são objetos de primeira classe
def saudacao(nome):
    return f"Olá, {nome}!"

# Atribuir a variável
saudar = saudacao
saudar("Mundo")  # Olá, Mundo!

# Passar como argumento
def aplicar(func, valor):
    return func(valor)

aplicar(saudacao, "Python")  # Olá, Python!

# Retornar função
def criar_saudacao(saudacao_texto):
    def saudar(nome):
        return f"{saudacao_texto}, {nome}!"
    return saudar

ola = criar_saudacao("Olá")
ola("Mundo")  # Olá, Mundo!
```

### Funções Lambda
```python
# Função anônima (uma linha)
lambda x: x**2

# Uso comum
quadrado = lambda x: x**2
quadrado(5)  # 25

# Com funções de ordem superior
lista = [1, 2, 3, 4]
list(map(lambda x: x**2, lista))  # [1, 4, 9, 16]
list(filter(lambda x: x % 2 == 0, lista))  # [2, 4]

sorted([('a', 3), ('b', 1)], key=lambda x: x[1])  # [('b', 1), ('a', 3)]
```

### Decoradores
```python
# Decorador básico
def meu_decorador(func):
    def wrapper(*args, **kwargs):
        print("Antes da função")
        resultado = func(*args, **kwargs)
        print("Depois da função")
        return resultado
    return wrapper

@meu_decorador
def minha_funcao():
    print("Função executada")

# Decorador com parâmetros
def decorador_com_parametro(mensagem):
    def decorador(func):
        def wrapper(*args, **kwargs):
            print(mensagem)
            return func(*args, **kwargs)
        return wrapper
    return decorador

@decorador_com_parametro("Executando...")
def outra_funcao():
    pass

# Decoradores de classe
from functools import wraps

def deco_classe(cls):
    cls.novo_atributo = "valor"
    return cls

@deco_classe
class MinhaClasse:
    pass

# Decoradores built-in
from functools import lru_cache, wraps

@lru_cache(maxsize=32)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### Funções Geradoras
```python
# Generator function (usa yield)
def contador(maximo):
    n = 0
    while n < maximo:
        yield n
        n += 1

for numero in contador(5):
    print(numero)  # 0, 1, 2, 3, 4

# Generator expression
gen = (x**2 for x in range(5))
list(gen)  # [0, 1, 4, 9, 16]

# Corrotinas (Python 3.5+)
def corrotina():
    while True:
        valor = yield
        print(f"Recebido: {valor}")

coro = corrotina()
next(coro)  # Inicializar
coro.send("Olá")  # Recebido: Olá
```

### Type Hints (Python 3.5+)
```python
from typing import List, Dict, Tuple, Optional, Union, Callable

def saudacao(nome: str) -> str:
    return f"Olá, {nome}!"

def processa_lista(itens: List[int]) -> List[str]:
    return [str(item) for item in itens]

def busca(dicionario: Dict[str, int], chave: str) -> Optional[int]:
    return dicionario.get(chave)

def operacao(x: Union[int, float], y: Union[int, float]) -> float:
    return x / y

# Tipo variável
from typing import TypeVar
T = TypeVar('T')

def primeiro_item(lista: List[T]) -> T:
    return lista[0]
```

---

## 🏗️ PROGRAMÇÃO ORIENTADA A OBJETOS

### Classes Básicas
```python
class MinhaClasse:
    """Documentação da classe"""
    
    # Atributo de classe
    atributo_classe = "valor"
    
    def __init__(self, valor):
        # Atributo de instância
        self.atributo_instancia = valor
    
    def metodo(self):
        return self.atributo_instancia
    
    @classmethod
    def metodo_classe(cls):
        return cls.atributo_classe
    
    @staticmethod
    def metodo_estatico():
        return "Não precisa de self ou cls"

# Uso
obj = MinhaClasse("teste")
obj.metodo()                     # "teste"
MinhaClasse.metodo_classe()      # "valor"
MinhaClasse.metodo_estatico()    # "Não precisa..."
```

### Encapsulamento
```python
class ContaBancaria:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.__saldo = saldo  # Atributo privado (name mangling)
    
    def depositar(self, valor):
        if valor > 0:
            self.__saldo += valor
    
    def sacar(self, valor):
        if 0 < valor <= self.__saldo:
            self.__saldo -= valor
            return valor
        return 0
    
    def get_saldo(self):
        return self.__saldo
    
    # Property (acesso como atributo)
    @property
    def saldo(self):
        return self.__saldo
    
    @saldo.setter
    def saldo(self, valor):
        if valor >= 0:
            self.__saldo = valor
        else:
            raise ValueError("Saldo não pode ser negativo")

conta = ContaBancaria("João", 1000)
conta.depositar(500)
print(conta.saldo)  # Usando property
```

### Herança
```python
class Animal:
    def __init__(self, nome):
        self.nome = nome
    
    def falar(self):
        raise NotImplementedError("Subclasse deve implementar")
    
    def mover(self):
        print(f"{self.nome} está se movendo")

class Cachorro(Animal):
    def falar(self):
        print(f"{self.nome} diz: Au au!")
    
    # Sobrescrever método
    def mover(self):
        super().mover()  # Chama método da superclasse
        print("Correndo!")

class Gato(Animal):
    def falar(self):
        print(f"{self.nome} diz: Miau!")

# Herança múltipla
class AnimalDomestico(Cachorro, Gato):  # Ordem de resolução (MRO)
    pass

# Verificar relacionamentos
issubclass(Cachorro, Animal)  # True
isinstance(Cachorro("Rex"), Animal)  # True
```

### Métodos Mágicos (Dunder Methods)
```python
class Vetor:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # Representação string
    def __str__(self):
        return f"Vetor({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vetor({self.x!r}, {self.y!r})"
    
    # Operações matemáticas
    def __add__(self, outro):
        return Vetor(self.x + outro.x, self.y + outro.y)
    
    def __sub__(self, outro):
        return Vetor(self.x - outro.x, self.y - outro.y)
    
    def __mul__(self, escalar):
        return Vetor(self.x * escalar, self.y * escalar)
    
    # Comparação
    def __eq__(self, outro):
        return self.x == outro.x and self.y == outro.y
    
    def __lt__(self, outro):
        return (self.x**2 + self.y**2) < (outro.x**2 + outro.y**2)
    
    # Container
    def __len__(self):
        return 2
    
    def __getitem__(self, indice):
        if indice == 0:
            return self.x
        elif indice == 1:
            return self.y
        raise IndexError("Índice fora do range")
    
    # Callable
    def __call__(self, escalar):
        return self * escalar

v = Vetor(1, 2)
print(v)       # Usa __str__
v * 3          # Usa __mul__
v(5)           # Usa __call__
```

### Classes Abstratas
```python
from abc import ABC, abstractmethod
from typing import List

class Forma(ABC):
    @abstractmethod
    def area(self) -> float:
        pass
    
    @abstractmethod
    def perimetro(self) -> float:
        pass

class Retangulo(Forma):
    def __init__(self, largura, altura):
        self.largura = largura
        self.altura = altura
    
    def area(self):
        return self.largura * self.altura
    
    def perimetro(self):
        return 2 * (self.largura + self.altura)

# Protocol (Python 3.8+ - structural subtyping)
from typing import Protocol

class Desenhavel(Protocol):
    def desenhar(self) -> str:
        ...

def renderizar(objeto: Desenhavel):
    return objeto.desenhar()
```

### Dataclasses (Python 3.7+)
```python
from dataclasses import dataclass, field
from typing import List, ClassVar

@dataclass
class Ponto:
    x: float
    y: float
    origem: ClassVar[tuple] = (0, 0)
    historico: List[tuple] = field(default_factory=list)
    
    def distancia(self) -> float:
        return (self.x**2 + self.y**2)**0.5

# Auto-generated: __init__, __repr__, __eq__, etc.
p1 = Ponto(1, 2)
p2 = Ponto(1, 2)
p1 == p2  # True

# Configurações
@dataclass(frozen=True)  # Imutável
@dataclass(order=True)   # Adiciona métodos de comparação
@dataclass(slots=True)   # Usa __slots__ para economia de memória
```

### Enums
```python
from enum import Enum, IntEnum, Flag, auto

class Cor(Enum):
    VERMELHO = 1
    VERDE = 2
    AZUL = 3

print(Cor.VERMELHO)          # Cor.VERMELHO
print(Cor.VERMELHO.value)    # 1
print(Cor(1))                # Cor.VERMELHO
print(Cor['VERMELHO'])       # Cor.VERMELHO

# IntEnum (comparável com inteiros)
class Prioridade(IntEnum):
    BAIXA = 1
    MEDIA = 2
    ALTA = 3

# Flag (operações bitwise)
class Permissao(Flag):
    LER = auto()
    ESCREVER = auto()
    EXECUTAR = auto()
    
perm = Permissao.LER | Permissao.ESCREVER
```

---

## 📁 MANIPULAÇÃO DE ARQUIVOS

### Leitura/Escrita Básica
```python
# Modos de abertura
# r: leitura (padrão)
# w: escrita (sobrescreve)
# a: append (adiciona ao final)
# x: criação exclusiva (falha se existir)
# b: modo binário
# t: modo texto (padrão)
# +: leitura e escrita

# Context manager (recomendado)
with open('arquivo.txt', 'r', encoding='utf-8') as f:
    conteudo = f.read()

# Leitura completa
with open('arquivo.txt') as f:
    texto = f.read()          # String completa
    linhas = f.readlines()    # Lista de linhas

# Leitura linha a linha
with open('arquivo.txt') as f:
    for linha in f:           # Iterador eficiente
        print(linha.strip())

# Leitura de N bytes
with open('arquivo.bin', 'rb') as f:
    dados = f.read(1024)      # Primeiros 1024 bytes

# Escrita
with open('saida.txt', 'w') as f:
    f.write("Linha 1\n")
    f.writelines(["Linha 2\n", "Linha 3\n"])

# Append
with open('log.txt', 'a') as f:
    f.write("Nova entrada\n")

# Posicionamento
with open('arquivo.txt', 'r+') as f:
    f.seek(10)                # Move para posição 10
    pos = f.tell()            # Posição atual
    f.seek(0, 2)              # Move para fim do arquivo
```

### Trabalhando com Caminhos
```python
import os
from pathlib import Path

# Usando os.path (legado)
caminho = os.path.join('pasta', 'arquivo.txt')
diretorio = os.path.dirname(caminho)
nome_arquivo = os.path.basename(caminho)
nome, extensao = os.path.splitext(nome_arquivo)
absoluto = os.path.abspath(caminho)
existe = os.path.exists(caminho)

# Usando pathlib (Python 3.4+, recomendado)
caminho = Path('pasta') / 'arquivo.txt'
caminho.parent              # Diretório pai
caminho.name                # Nome do arquivo
caminho.stem                # Nome sem extensão
caminho.suffix              # Extensão
caminho.absolute()          # Caminho absoluto
caminho.exists()            # Verifica existência
caminho.is_file()           # É arquivo?
caminho.is_dir()            # É diretório?

# Operações com Path
caminho.read_text()         # Lê como texto
caminho.write_text("texto") # Escreve texto
caminho.read_bytes()        # Lê como bytes
caminho.write_bytes(b"data")# Escreve bytes

# Navegação
for arquivo in Path('.').iterdir():
    print(arquivo)

for arquivo in Path('.').glob('*.py'):
    print(arquivo)

for arquivo in Path('.').rglob('*.py'):
    print(arquivo)
```

### Operações com Diretórios
```python
import os
import shutil
from pathlib import Path

# Criar diretório
os.mkdir('novo_dir')           # Cria diretório
os.makedirs('dir/subdir')      # Cria diretórios aninhados
Path('novo_dir2').mkdir()      # Com pathlib

# Listar arquivos
arquivos = os.listdir('.')     # Lista nomes
conteudo = os.scandir('.')     # Objetos DirEntry

# Copiar
shutil.copy('origem.txt', 'destino.txt')
shutil.copy2('origem.txt', 'destino.txt')  # Preserva metadados
shutil.copytree('origem_dir', 'destino_dir')

# Mover/Renomear
os.rename('antigo.txt', 'novo.txt')
shutil.move('origem.txt', 'destino/')

# Remover
os.remove('arquivo.txt')
os.rmdir('diretorio_vazio')
shutil.rmtree('diretorio_com_conteudo')  # Recursivo

# Informações
stats = os.stat('arquivo.txt')
tamanho = os.path.getsize('arquivo.txt')
modificado = os.path.getmtime('arquivo.txt')
```

### Arquivos Temporários
```python
import tempfile

# Arquivo temporário
with tempfile.TemporaryFile(mode='w+') as tmp:
    tmp.write("conteúdo temporário")
    tmp.seek(0)
    print(tmp.read())

# Arquivo temporário nomeado
with tempfile.NamedTemporaryFile(mode='w', delete=False) as tmp:
    tmp.write("conteúdo")
    nome_temp = tmp.name  # Caminho do arquivo

# Diretório temporário
with tempfile.TemporaryDirectory() as tmpdir:
    print(f"Diretório temporário: {tmpdir}")
    # Trabalhar com arquivos em tmpdir
```

### Serialização
```python
import pickle
import json
import csv
import yaml  # pip install PyYAML

# Pickle (Python object serialization)
dados = {'nome': 'João', 'idade': 30}

# Serializar
with open('dados.pkl', 'wb') as f:
    pickle.dump(dados, f)

# Deserializar
with open('dados.pkl', 'rb') as f:
    dados_carregados = pickle.load(f)

# JSON
with open('dados.json', 'w') as f:
    json.dump(dados, f, indent=2)

with open('dados.json') as f:
    dados_json = json.load(f)

# CSV
with open('dados.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['Nome', 'Idade'])
    writer.writerow(['João', 30])

with open('dados.csv', newline='') as f:
    reader = csv.DictReader(f)
    for linha in reader:
        print(linha)
```

---

## ⚡ TRATAMENTO DE EXCEÇÕES

### Try/Except Básico
```python
try:
    # Código que pode gerar exceção
    resultado = 10 / 0
except ZeroDivisionError:
    # Trata exceção específica
    print("Divisão por zero!")
except (TypeError, ValueError) as e:
    # Trata múltiplas exceções
    print(f"Erro de tipo ou valor: {e}")
except Exception as e:
    # Captura qualquer exceção
    print(f"Erro inesperado: {e}")
else:
    # Executa se NÃO houve exceção
    print("Operação bem-sucedida!")
finally:
    # SEMPRE executa (limpeza)
    print("Finalizando...")

# Raise para lançar exceção
if valor < 0:
    raise ValueError("Valor não pode ser negativo")

# Raise from (cadeia de exceções)
try:
    # alguma operação
    pass
except Exception as e:
    raise RuntimeError("Erro no processamento") from e
```

### Exceções Personalizadas
```python
class ErroPersonalizado(Exception):
    """Exceção personalizada"""
    
    def __init__(self, mensagem, codigo):
        super().__init__(mensagem)
        self.codigo = codigo
    
    def __str__(self):
        return f"[{self.codigo}] {super().__str__()}"

# Uso
def processar(valor):
    if valor < 0:
        raise ErroPersonalizado("Valor negativo", 400)
    
    return valor * 2

try:
    processar(-1)
except ErroPersonalizado as e:
    print(f"Erro {e.codigo}: {e}")
```

### Assert
```python
# Usado para debugging (pode ser desabilitado com -O)
def calcular_imc(peso, altura):
    assert peso > 0, "Peso deve ser positivo"
    assert altura > 0, "Altura deve ser positiva"
    return peso / (altura ** 2)

# Não use assert para validação de entrada
# (pode ser desabilitado com -O flag)
```

### Context Managers Personalizados
```python
from contextlib import contextmanager

# Usando decorator
@contextmanager
def gerenciador_temporario():
    print("Iniciando contexto")
    recurso = "recurso alocado"
    try:
        yield recurso
    finally:
        print("Liberando recurso")

# Uso
with gerenciador_temporario() as recurso:
    print(f"Usando {recurso}")

# Classe context manager
class GerenciadorArquivo:
    def __init__(self, nome_arquivo, modo):
        self.nome_arquivo = nome_arquivo
        self.modo = modo
    
    def __enter__(self):
        self.arquivo = open(self.nome_arquivo, self.modo)
        return self.arquivo
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.arquivo.close()
        if exc_type is not None:
            print(f"Erro no contexto: {exc_val}")
        return False  # Não suprime a exceção

# Context managers built-in
from contextlib import suppress, redirect_stdout, closing
import os

# Suprimir exceções específicas
with suppress(FileNotFoundError):
    os.remove('arquivo_inexistente.txt')

# Redirecionar stdout
with redirect_stdout(open('saida.txt', 'w')):
    print("Isso vai para o arquivo")

# Closing para objetos com close()
from urllib.request import urlopen
with closing(urlopen('http://example.com')) as pagina:
    conteudo = pagina.read()
```

---

## 📦 MÓDULOS DA BIBLIOTECA PADRÃO

### Sistema e SO
```python
import sys
import os
import platform

# sys
sys.argv           # Argumentos da linha de comando
sys.path           # PATH de importação
sys.modules        # Módulos carregados
sys.version        # Versão do Python
sys.exit(1)        # Sair com código de erro
sys.stdin          # Entrada padrão
sys.stdout         # Saída padrão
sys.stderr         # Erro padrão

# os
os.name            # Nome do SO (posix, nt, java)
os.environ         # Variáveis de ambiente
os.getcwd()        # Diretório atual
os.chdir('..')     # Mudar diretório
os.system('ls')    # Executar comando shell
os.popen('ls').read()  # Capturar saída

# platform
platform.system()   # Sistema operacional
platform.release()  # Versão do SO
platform.python_version()  # Versão Python
platform.machine()  # Arquitetura
platform.node()     # Nome da máquina
```

### Data e Hora
```python
import datetime
import time
import calendar

# datetime
agora = datetime.datetime.now()
hoje = datetime.date.today()
ontem = hoje - datetime.timedelta(days=1)
amanha = hoje + datetime.timedelta(days=1)

# Formatação
agora.strftime("%Y-%m-%d %H:%M:%S")
datetime.datetime.strptime("2024-01-01", "%Y-%m-%d")

# time
time.time()           # Timestamp Unix
time.sleep(1)         # Dormir 1 segundo
time.localtime()      # Hora local como struct_time
time.gmtime()         # Hora UTC como struct_time

# calendar
calendar.month(2024, 1)   # Calendário do mês
calendar.isleap(2024)     # É ano bissexto?
calendar.weekday(2024, 1, 1)  # Dia da semana (0=segunda)
```

### Matemática
```python
import math
import random
import statistics

# math
math.pi              # π
math.e               # e
math.sqrt(16)        # Raiz quadrada
math.pow(2, 3)       # Potência
math.log(100, 10)    # Logaritmo
math.sin(math.pi/2)  # Seno
math.floor(3.7)      # Piso
math.ceil(3.2)       # Teto
math.gcd(12, 18)     # MDC
math.comb(5, 2)      # Combinações (C(5,2))
math.perm(5, 2)      # Permutações (P(5,2))

# random
random.random()           # [0.0, 1.0)
random.uniform(1, 10)     # Float uniforme
random.randint(1, 100)    # Inteiro inclusivo
random.choice(lista)      # Elemento aleatório
random.choices(lista, k=3) # Amostra com reposição
random.sample(lista, k=3)  # Amostra sem reposição
random.shuffle(lista)     # Embaralha lista
random.seed(42)           # Semente para reprodução

# statistics
statistics.mean([1, 2, 3])       # Média
statistics.median([1, 2, 3, 4])  # Mediana
statistics.mode([1, 2, 2, 3])    # Moda
statistics.stdev([1, 2, 3])      # Desvio padrão
statistics.variance([1, 2, 3])   # Variância
```

### Coleções Avançadas
```python
import collections
import heapq
import bisect
import array

# collections
collections.Counter('abracadabra')
collections.defaultdict(list)
collections.OrderedDict()
collections.deque([1, 2, 3])
collections.namedtuple('Ponto', ['x', 'y'])
collections.ChainMap(dict1, dict2)  # Cadeia de dicionários

# heapq (fila de prioridade)
heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappop(heap)  # 3 (menor)
heapq.nlargest(3, lista)  # 3 maiores
heapq.nsmallest(3, lista) # 3 menores

# bisect (busca binária)
lista_ordenada = [1, 3, 5, 7]
bisect.bisect_left(lista_ordenada, 5)  # Índice 2
bisect.bisect_right(lista_ordenada, 5) # Índice 3
bisect.insort(lista_ordenada, 6)      # Insere mantendo ordem

# array (eficiente para tipos numéricos)
arr = array.array('i', [1, 2, 3, 4])
```

### Itertools
```python
import itertools

# Iteradores infinitos
itertools.count(10)          # 10, 11, 12, ...
itertools.cycle([1, 2, 3])   # 1, 2, 3, 1, 2, 3, ...
itertools.repeat(7, 3)       # 7, 7, 7

# Combinatórios
itertools.combinations('ABC', 2)     # AB, AC, BC
itertools.combinations_with_replacement('ABC', 2)  # AA, AB, AC, BB, BC, CC
itertools.permutations('ABC', 2)     # AB, AC, BA, BC, CA, CB
itertools.product('AB', repeat=2)    # AA, AB, BA, BB

# Operações em iteradores
itertools.chain([1, 2], [3, 4])      # 1, 2, 3, 4
itertools.islice(range(10), 2, 8, 2) # 2, 4, 6
itertools.zip_longest('AB', 'XYZ')   # ('A','X'), ('B','Y'), (None,'Z')
itertools.groupby(sorted([1,1,2,3])) # Agrupa valores consecutivos
```

### Functools
```python
import functools

# lru_cache (cache de resultados)
@functools.lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# partial (função parcial)
def potencia(base, expoente):
    return base ** expoente

quadrado = functools.partial(potencia, expoente=2)
quadrado(5)  # 25

# reduce
functools.reduce(lambda x, y: x+y, [1, 2, 3, 4])  # 10

# total_ordering (preenche operadores de comparação)
@functools.total_ordering
class Ordenavel:
    def __init__(self, valor):
        self.valor = valor
    
    def __eq__(self, outro):
        return self.valor == outro.valor
    
    def __lt__(self, outro):
        return self.valor < outro.valor
    # Gera automaticamente: <=, >, >=
```

---

## 🔤 EXPRESSÕES REGULARES

### Módulo re
```python
import re

# Padrões básicos
padrao = r'\d+'  # Um ou mais dígitos
texto = "Python 3.10 foi lançado em 2021"

# Buscar
match = re.search(padrao, texto)
if match:
    print(match.group())  # '3'
    print(match.start())  # 7
    print(match.end())    # 8
    print(match.span())   # (7, 8)

# Encontrar todos
re.findall(r'\d+', texto)  # ['3', '10', '2021']

# Encontrar iterativamente
for match in re.finditer(r'\d+', texto):
    print(match.group())

# Substituir
re.sub(r'\d+', 'N', texto)  # "Python N.N foi lançado em N"

# Split
re.split(r'\s+', texto)  # ['Python', '3.10', 'foi', 'lançado', 'em', '2021']

# Compilar para reuso
padrao_compilado = re.compile(r'\d+')
padrao_compilado.findall(texto)
```

### Metacaracteres e Classes
```python
# Metacaracteres
.        # Qualquer caractere (exceto nova linha)
^        # Início da string
$        # Fim da string
*        # 0 ou mais repetições
+        # 1 ou mais repetições
?        # 0 ou 1 repetição
{n}      # Exatamente n repetições
{n,}     # n ou mais repetições
{n,m}    # Entre n e m repetições
|        # OU lógico
()       # Grupo de captura
[]       # Classe de caracteres
\        # Escapar metacaracteres

# Classes de caracteres
\d       # Dígito [0-9]
\D       # Não dígito [^0-9]
\w       # Caractere de palavra [a-zA-Z0-9_]
\W       # Não caractere de palavra [^\w]
\s       # Espaço em branco [ \t\n\r\f\v]
\S       # Não espaço em branco [^\s]
[aeiou]  # Qualquer vogal
[a-z]    # Letra minúscula
[A-Z]    # Letra maiúscula
[^0-9]   # Qualquer coisa exceto dígitos

# Modificadores
re.IGNORECASE  # ou re.I - case insensitive
re.MULTILINE   # ou re.M - ^ e $ por linha
re.DOTALL      # ou re.S - . inclui \n
re.VERBOSE     # ou re.X - permite comentários
```

### Exemplos Comuns
```python
# Validar email
email_regex = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'

# Validar CPF
cpf_regex = r'^\d{3}\.\d{3}\.\d{3}-\d{2}$'

# Extrair URLs
url_regex = r'https?://(?:[-\w.]|(?:%[\da-fA-F]{2}))+'

# Extrair hashtags
hashtag_regex = r'#\w+'

# Validar telefone BR
telefone_regex = r'^(\+55)?\s?(\(?\d{2}\)?)?\s?\d{4,5}-?\d{4}$'

# Buscar palavras inteiras
re.findall(r'\bpython\b', texto, re.IGNORECASE)

# Grupos de captura
data = "2024-01-15"
match = re.match(r'(\d{4})-(\d{2})-(\d{2})', data)
ano, mes, dia = match.groups()
```

---

## ⚡ CONCORRÊNCIA E PARALELISMO

### Threading
```python
import threading
import time
from queue import Queue

# Thread básica
def tarefa(nome):
    print(f"{nome} iniciada")
    time.sleep(1)
    print(f"{nome} finalizada")

thread = threading.Thread(target=tarefa, args=("Thread 1",))
thread.start()
thread.join()  # Espera thread terminar

# Thread com herança
class MinhaThread(threading.Thread):
    def __init__(self, nome):
        super().__init__()
        self.nome = nome
    
    def run(self):
        tarefa(self.nome)

# Lock (exclusão mútua)
contador = 0
lock = threading.Lock()

def incrementar():
    global contador
    with lock:
        temp = contador
        time.sleep(0.001)
        contador = temp + 1

# Semaphore
sem = threading.Semaphore(3)  # 3 threads simultâneas

def acessar_recurso():
    with sem:
        # Acesso ao recurso limitado
        time.sleep(1)

# Event
evento = threading.Event()

def esperar_evento():
    print("Esperando evento...")
    evento.wait()
    print("Evento recebido!")

# Timer
timer = threading.Timer(5.0, lambda: print("Timeout!"))
timer.start()

# Queue thread-safe
fila = Queue()

def produtor():
    for i in range(5):
        fila.put(i)
        time.sleep(0.5)

def consumidor():
    while True:
        item = fila.get()
        if item is None:  # Sinal de parada
            break
        print(f"Consumido: {item}")
        fila.task_done()
```

### Multiprocessing
```python
import multiprocessing
import os

# Processo básico
def trabalho(nome):
    print(f"{name} PID: {os.getpid()}")
    return nome.upper()

processo = multiprocessing.Process(target=trabalho, args=("Processo 1",))
processo.start()
processo.join()

# Pool de processos
def quadrado(x):
    return x * x

with multiprocessing.Pool(processes=4) as pool:
    # Mapeamento
    resultados = pool.map(quadrado, range(10))
    
    # Mapeamento assíncrono
    resultado_async = pool.map_async(quadrado, range(10))
    resultados = resultado_async.get()
    
    # Apply async
    resultado = pool.apply_async(quadrado, (5,))
    print(resultado.get())

# Queue entre processos
def produtor(fila):
    for i in range(5):
        fila.put(i)

def consumidor(fila):
    while True:
        item = fila.get()
        if item is None:
            break
        print(f"Processado: {item}")

fila = multiprocessing.Queue()
p1 = multiprocessing.Process(target=produtor, args=(fila,))
p2 = multiprocessing.Process(target=consumidor, args=(fila,))
p1.start(); p2.start()
p1.join(); fila.put(None); p2.join()

# Memória compartilhada
valor_compartilhado = multiprocessing.Value('i', 0)
array_compartilhado = multiprocessing.Array('d', [0.0, 1.0, 2.0])

# Lock entre processos
lock = multiprocessing.Lock()

def incrementar(valor):
    with lock:
        valor.value += 1
```

### Async/Await
```python
import asyncio
import aiohttp  # pip install aiohttp

# Corrotina básica
async def saudacao():
    print("Olá")
    await asyncio.sleep(1)
    print("Mundo")

# Executar
asyncio.run(saudacao())

# Múltiplas tarefas
async def tarefa_longa(nome, segundos):
    print(f"{nome} iniciada")
    await asyncio.sleep(segundos)
    print(f"{nome} finalizada")
    return nome

async def main():
    # Executar sequencialmente
    resultado1 = await tarefa_longa("A", 2)
    resultado2 = await tarefa_longa("B", 1)
    
    # Executar concorrentemente
    tarefa1 = asyncio.create_task(tarefa_longa("C", 2))
    tarefa2 = asyncio.create_task(tarefa_longa("D", 1))
    
    await tarefa1
    await tarefa2
    
    # Gather (executar múltiplas)
    resultados = await asyncio.gather(
        tarefa_longa("E", 2),
        tarefa_longa("F", 1),
        tarefa_longa("G", 3)
    )
    print(resultados)  # ['E', 'F', 'G']

# Queue assíncrona
async def produtor(fila):
    for i in range(5):
        await fila.put(i)
        await asyncio.sleep(0.5)

async def consumidor(fila):
    while True:
        item = await fila.get()
        if item is None:
            break
        print(f"Consumido: {item}")

async def main_queue():
    fila = asyncio.Queue()
    
    prod = asyncio.create_task(produtor(fila))
    cons = asyncio.create_task(consumidor(fila))
    
    await prod
    await fila.put(None)
    await cons

# Timeout
async def operacao_longa():
    await asyncio.sleep(10)
    return "resultado"

try:
    resultado = await asyncio.wait_for(operacao_longa(), timeout=1.0)
except asyncio.TimeoutError:
    print("Timeout!")

# Semaphore assíncrono
sem = asyncio.Semaphore(3)

async def acesso_limitado(n):
    async with sem:
        print(f"{n} acessando recurso")
        await asyncio.sleep(1)
        print(f"{n} liberando recurso")
```

---

## 🧠 METAPROGRAMAÇÃO

### Decoradores Avançados
```python
import functools
import inspect

# Decorador com argumentos
def repetir(n_vezes):
    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n_vezes):
                resultado = func(*args, **kwargs)
            return resultado
        return wrapper
    return decorador

@repetir(3)
def saudar(nome):
    print(f"Olá, {nome}")

# Decorador de classe
def decorador_classe(cls):
    cls.novo_atributo = "adicionado"
    return cls

@decorador_classe
class MinhaClasse:
    pass

# Decorador que preserva assinatura
def decorador_assinatura(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Chamando {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

# Decorador com registro
REGISTRO = {}

def registrar(nome):
    def decorador(func):
        REGISTRO[nome] = func
        return func
    return decorador
```

### Metaclasses
```python
# Metaclasse básica
class MinhaMeta(type):
    def __new__(mcls, nome, bases, namespace):
        print(f"Criando classe {nome}")
        namespace['criado_por_meta'] = True
        return super().__new__(mcls, nome, bases, namespace)
    
    def __init__(cls, nome, bases, namespace):
        super().__init__(nome, bases, namespace)
        print(f"Inicializando classe {nome}")

class MinhaClasse(metaclass=MinhaMeta):
    pass

# Metaclasse para Singleton
class SingletonMeta(type):
    _instancias = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instancias:
            cls._instancias[cls] = super().__call__(*args, **kwargs)
        return cls._instancias[cls]

class Singleton(metaclass=SingletonMeta):
    pass

# Metaclasse para validação
class ValidatedMeta(type):
    def __new__(mcls, nome, bases, namespace):
        # Valida campos da classe
        for chave, valor in namespace.items():
            if chave.startswith('validate_'):
                campo = chave[9:]
                if campo in namespace:
                    # Aplica validação
                    pass
        return super().__new__(mcls, nome, bases, namespace)
```

### Descritores
```python
# Descritor básico
class Descritor:
    def __get__(self, obj, objtype=None):
        print(f"Acessando {obj}.{self}")
        return "valor"
    
    def __set__(self, obj, valor):
        print(f"Atribuindo {valor} a {obj}.{self}")
    
    def __delete__(self, obj):
        print(f"Deletando {obj}.{self}")

class MinhaClasse:
    atributo = Descritor()

obj = MinhaClasse()
obj.atributo        # Chama __get__
obj.atributo = 10   # Chama __set__
del obj.atributo    # Chama __delete__

# Property como descritor
class Temperatura:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, valor):
        if valor < -273.15:
            raise ValueError("Temperatura abaixo do zero absoluto")
        self._celsius = valor
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32
```

### Introspectão
```python
import inspect

# Informações sobre objetos
objeto = MinhaClasse()

type(objeto)                # Tipo do objeto
isinstance(objeto, Classe)  # Verifica instância
issubclass(SubClasse, Classe)  # Verifica herança

# Atributos
dir(objeto)                 # Lista atributos
hasattr(objeto, 'nome')     # Tem atributo?
getattr(objeto, 'nome', padrão)  # Obtém atributo
setattr(objeto, 'nome', valor)   # Define atributo
delattr(objeto, 'nome')     # Remove atributo

# Módulos e pacotes
import sys
sys.modules                 # Módulos importados
__name__                    # Nome do módulo atual
__file__                    # Caminho do módulo
__package__                 # Nome do pacote

# Inspecionar funções
inspect.signature(func)     # Assinatura da função
inspect.getsource(func)     # Código fonte
inspect.getmembers(obj)     # Membros do objeto
inspect.stack()             # Pilha de chamadas
inspect.getframeinfo(frame) # Info do frame
```

### AST (Abstract Syntax Tree)
```python
import ast
import inspect

# Analisar código
codigo = """
def soma(a, b):
    return a + b
"""

arvore = ast.parse(codigo)
print(ast.dump(arvore, indent=2))

# Modificar AST
class Transformador(ast.NodeTransformer):
    def visit_FunctionDef(self, node):
        # Adiciona docstring a todas as funções
        docstring = ast.Expr(value=ast.Constant(value="Documentação"))
        node.body.insert(0, docstring)
        return node

transformado = Transformador().visit(arvore)
ast.fix_missing_locations(transformado)

# Compilar e executar
codigo_modificado = compile(transformado, '<string>', 'exec')
exec(codigo_modificado)
```

---

## 🏆 BOAS PRÁTICAS E PADRÕES

### PEP 8 (Guia de Estilo)
```python
# Nomenclatura
variavel_snake_case = "variáveis e funções"
CONSTANTE_MAIUSCULA = "constantes"
ClassePascalCase = "classes"

# Espaçamento
# 4 espaços por nível de indentação
# Linha máxima: 79 caracteres
# Duas linhas entre funções/classes
# Uma linha entre métodos

# Imports
import os
import sys
from typing import Dict, List

# Agrupamento: stdlib, third-party, local
import datetime
import json

import requests  # third-party

from meu_modulo import MinhaClasse  # local

# Código limpo
def funcao_clara(parametro1, parametro2):
    """Docstring explicativa."""
    resultado = parametro1 + parametro2
    return resultado

# Evitar
x = 1; y = 2  # Múltiplas instruções na mesma linha
if x==1: pass # Sem espaços
```

### Documentação
```python
"""Módulo de exemplo.

Este módulo demonstra como documentar código Python
seguindo as convenções PEP 257.
"""

def funcao_documentada(param1, param2):
    """Breve descrição da função.
    
    Descrição detalhada da função e seu propósito.
    
    Args:
        param1 (int): Descrição do primeiro parâmetro.
        param2 (str): Descrição do segundo parâmetro.
    
    Returns:
        bool: Descrição do valor de retorno.
    
    Raises:
        ValueError: Se param1 for negativo.
        TypeError: Se param2 não for string.
    
    Examples:
        >>> funcao_documentada(1, "teste")
        True
    """
    if param1 < 0:
        raise ValueError("param1 não pode ser negativo")
    
    return True


class MinhaClasse:
    """Documentação da classe.
    
    Atributos:
        atributo_publico (int): Atributo público.
        _atributo_privado (str): Atributo privado.
    """
    
    def __init__(self, valor):
        """Inicializa a classe.
        
        Args:
            valor (int): Valor inicial.
        """
        self.atributo_publico = valor
        self._atributo_privado = "privado"
```

### Testes
```python
import unittest
import pytest  # pip install pytest
import doctest

# unittest
class TestMinhaFuncao(unittest.TestCase):
    def setUp(self):
        """Executado antes de cada teste."""
        self.dados = [1, 2, 3]
    
    def tearDown(self):
        """Executado depois de cada teste."""
        pass
    
    def test_soma(self):
        """Testa a função soma."""
        self.assertEqual(1 + 1, 2)
        self.assertNotEqual(1 + 1, 3)
        self.assertTrue(1 < 2)
        self.assertFalse(1 > 2)
        self.assertIn(2, [1, 2, 3])
        self.assertIsInstance("texto", str)
        self.assertRaises(ValueError, int, "não é número")
    
    def test_lista(self):
        """Testa operações com lista."""
        self.assertListEqual(self.dados, [1, 2, 3])
        self.assertCountEqual(self.dados, [3, 2, 1])  # Ignora ordem

# pytest (mais simples)
def test_soma_pytest():
    assert 1 + 1 == 2

# doctest (testes na docstring)
def fatorial(n):
    """Calcula o fatorial de n.
    
    >>> fatorial(0)
    1
    >>> fatorial(5)
    120
    >>> fatorial(-1)
    Traceback (most recent call last):
        ...
    ValueError: n deve ser não-negativo
    """
    if n < 0:
        raise ValueError("n deve ser não-negativo")
    if n == 0:
        return 1
    return n * fatorial(n - 1)

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

### Logging
```python
import logging
import logging.config

# Configuração básica
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('app.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

# Níveis de log
logger.debug("Mensagem de debug")     # 10
logger.info("Informação")              # 20
logger.warning("Aviso")                # 30
logger.error("Erro")                   # 40
logger.critical("Crítico")             # 50

# Configuração avançada
logging.config.dictConfig({
    'version': 1,
    'formatters': {
        'default': {
            'format': '%(asctime)s - %(levelname)s - %(message)s',
        },
    },
    'handlers': {
        'file': {
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': 'app.log',
            'maxBytes': 10485760,  # 10MB
            'backupCount': 5,
            'formatter': 'default',
        },
        'console': {
            'class': 'logging.StreamHandler',
            'formatter': 'default',
        },
    },
    'root': {
        'level': 'INFO',
        'handlers': ['file', 'console'],
    },
})
```

---

## 🛠️ FERRAMENTAS DO ECOSSISTEMA

### Virtual Environments
```bash
# Criar ambiente virtual
python -m venv venv

# Ativar (Linux/Mac)
source venv/bin/activate

# Ativar (Windows)
venv\Scripts\activate

# Desativar
deactivate

# Requirements
pip freeze > requirements.txt
pip install -r requirements.txt

# pipenv (gerenciador moderno)
pip install pipenv
pipenv install requests
pipenv shell
pipenv lock
```

### Packaging
```python
# Estrutura do projeto
"""
meu_projeto/
├── src/
│   └── meu_pacote/
│       ├── __init__.py
│       ├── modulo1.py
│       └── modulo2.py
├── tests/
│   ├── __init__.py
│   └── test_modulo1.py
├── pyproject.toml  # ou setup.py
├── README.md
└── LICENSE
"""

# pyproject.toml (moderno)
"""
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "meu-pacote"
version = "1.0.0"
authors = [{name = "Seu Nome", email = "email@exemplo.com"}]
description = "Descrição do pacote"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.25.0",
    "numpy>=1.20.0",
]

[project.optional-dependencies]
dev = ["pytest", "black", "mypy"]
gui = ["pyqt5"]
"""

# setup.py (legado)
from setuptools import setup, find_packages

setup(
    name="meu-pacote",
    version="1.0.0",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    install_requires=["requests>=2.25.0"],
    extras_require={
        "dev": ["pytest", "black"],
    },
)
```

### Ferramentas de Qualidade
```bash
# Formatação
black .                    # Formata código
isort .                    # Organiza imports

# Linting
flake8 .                   # Verifica estilo PEP 8
pylint meu_modulo.py       # Análise estática
mypy .                     # Verificação de tipos

# Testes
pytest                     # Executa testes
coverage run -m pytest     # Cobertura de testes
coverage report            # Relatório de cobertura
coverage html              # HTML da cobertura

# Segurança
bandit -r .                # Análise de segurança
safety check               # Verifica dependências vulneráveis

# Dependências
pip-audit                  # Auditoria de pacotes
pipdeptree
