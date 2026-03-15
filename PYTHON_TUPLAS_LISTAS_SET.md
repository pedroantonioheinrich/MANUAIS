# Manual Completo de Métodos de Tuplas, Listas e Sets em Python

Este manual tem como objetivo explicar detalhadamente todos os métodos disponíveis para os tipos de dados **tupla** (tuple), **lista** (list) e **conjunto** (set) em Python. Cada método é descrito com sua sintaxe, parâmetros, valor de retorno e exemplos práticos.

---

## 1. Introdução

Em Python, tuplas, listas e conjuntos são estruturas de dados muito utilizadas para armazenar coleções de itens. Cada uma possui características próprias:

- **Tupla**: imutável, ordenada, permite elementos duplicados.
- **Lista**: mutável, ordenada, permite elementos duplicados.
- **Conjunto (set)**: mutável, não ordenado, não permite elementos duplicados.

Conhecer os métodos de cada uma é essencial para manipular dados de forma eficiente e elegante.

---

## 2. Tuplas (tuple)

As tuplas são sequências imutáveis. Por serem imutáveis, não possuem métodos que alteram seu conteúdo. Os únicos métodos disponíveis são de consulta.

### 2.1. `tuple.count(x)`

Retorna o número de vezes que o valor `x` aparece na tupla.

**Sintaxe:**  
`tuple.count(x)`

**Parâmetros:**  
- `x` – o valor a ser contado.

**Retorno:** inteiro.

**Exemplo:**
```python
tupla = (1, 2, 3, 2, 4, 2)
print(tupla.count(2))  # Saída: 3
print(tupla.count(5))  # Saída: 0
```

### 2.2. `tuple.index(x[, start[, end]])`

Retorna o índice da primeira ocorrência de `x` na tupla. Opcionalmente, pode-se especificar os índices `start` e `end` para limitar a busca.

**Sintaxe:**  
`tuple.index(x, start=0, end=len(tuple))`

**Parâmetros:**  
- `x` – valor a ser localizado.  
- `start` (opcional) – índice inicial da busca.  
- `end` (opcional) – índice final (exclusivo) da busca.

**Retorno:** inteiro (posição).  
**Exceção:** `ValueError` se `x` não for encontrado.

**Exemplo:**
```python
tupla = ('a', 'b', 'c', 'b', 'd')
print(tupla.index('b'))        # Saída: 1
print(tupla.index('b', 2))     # Saída: 3
print(tupla.index('b', 2, 4))  # Saída: 3
# tupla.index('z')  # ValueError: tuple.index(x): x not in tuple
```

---

## 3. Listas (list)

As listas são mutáveis e ordenadas. Possuem diversos métodos para adicionar, remover, modificar e consultar elementos.

### 3.1. `list.append(x)`

Adiciona um item `x` ao final da lista.

**Sintaxe:**  
`list.append(x)`

**Parâmetros:**  
- `x` – elemento a ser adicionado.

**Retorno:** `None` (modifica a lista in-place).

**Exemplo:**
```python
lista = [1, 2, 3]
lista.append(4)
print(lista)  # [1, 2, 3, 4]
```

### 3.2. `list.extend(iterável)`

Estende a lista adicionando todos os elementos de um iterável (outra lista, tupla, string, etc.) ao final.

**Sintaxe:**  
`list.extend(iterável)`

**Parâmetros:**  
- `iterável` – qualquer objeto iterável.

**Retorno:** `None`.

**Exemplo:**
```python
lista = [1, 2]
lista.extend([3, 4])
print(lista)          # [1, 2, 3, 4]

lista.extend('abc')
print(lista)          # [1, 2, 3, 4, 'a', 'b', 'c']
```

### 3.3. `list.insert(i, x)`

Insere um item `x` na posição `i`. O elemento que estava na posição `i` e os posteriores são deslocados para a direita.

**Sintaxe:**  
`list.insert(i, x)`

**Parâmetros:**  
- `i` – índice onde inserir.  
- `x` – elemento a ser inserido.

**Retorno:** `None`.

**Exemplo:**
```python
lista = [1, 2, 3]
lista.insert(1, 'novo')
print(lista)  # [1, 'novo', 2, 3]
```

### 3.4. `list.remove(x)`

Remove a **primeira ocorrência** do item `x` da lista. Se o item não existir, lança `ValueError`.

**Sintaxe:**  
`list.remove(x)`

**Parâmetros:**  
- `x` – valor a ser removido.

**Retorno:** `None`.

**Exemplo:**
```python
lista = [1, 2, 3, 2, 4]
lista.remove(2)
print(lista)  # [1, 3, 2, 4]
# lista.remove(10)  # ValueError: list.remove(x): x not in list
```

### 3.5. `list.pop([i])`

Remove e retorna o item na posição `i`. Se `i` não for informado, remove e retorna o último item.

**Sintaxe:**  
`list.pop(i=-1)`

**Parâmetros:**  
- `i` (opcional) – índice do elemento a remover.

**Retorno:** o elemento removido.  
**Exceção:** `IndexError` se a lista estiver vazia ou índice inválido.

**Exemplo:**
```python
lista = [10, 20, 30, 40]
item = lista.pop()
print(item)   # 40
print(lista)  # [10, 20, 30]

item2 = lista.pop(1)
print(item2)  # 20
print(lista)  # [10, 30]
```

### 3.6. `list.clear()`

Remove todos os elementos da lista, deixando-a vazia.

**Sintaxe:**  
`list.clear()`

**Retorno:** `None`.

**Exemplo:**
```python
lista = [1, 2, 3]
lista.clear()
print(lista)  # []
```

### 3.7. `list.index(x[, start[, end]])`

Retorna o índice da primeira ocorrência de `x` na lista (ou no intervalo `[start, end)`).

**Sintaxe:**  
`list.index(x, start=0, end=len(lista))`

**Parâmetros:**  
- `x` – valor procurado.  
- `start` (opcional) – início da busca.  
- `end` (opcional) – fim da busca (exclusivo).

**Retorno:** inteiro.  
**Exceção:** `ValueError` se não encontrado.

**Exemplo:**
```python
lista = ['a', 'b', 'c', 'b']
print(lista.index('b'))      # 1
print(lista.index('b', 2))   # 3
```

### 3.8. `list.count(x)`

Retorna o número de vezes que `x` aparece na lista.

**Sintaxe:**  
`list.count(x)`

**Parâmetros:**  
- `x` – valor a ser contado.

**Retorno:** inteiro.

**Exemplo:**
```python
lista = [1, 2, 2, 3, 2, 4]
print(lista.count(2))  # 3
print(lista.count(5))  # 0
```

### 3.9. `list.sort(*, key=None, reverse=False)`

Ordena os itens da lista **in-place** (modifica a lista original). Os argumentos devem ser passados por nome (keyword-only).

**Sintaxe:**  
`list.sort(key=None, reverse=False)`

**Parâmetros:**  
- `key` (opcional) – função que extrai um critério de comparação de cada elemento.  
- `reverse` (opcional) – se `True`, ordena em ordem decrescente.

**Retorno:** `None`.

**Exemplo:**
```python
numeros = [3, 1, 4, 1, 5]
numeros.sort()
print(numeros)  # [1, 1, 3, 4, 5]

palavras = ['banana', 'abacaxi', 'laranja']
palavras.sort(key=len)
print(palavras)  # ['banana', 'laranja', 'abacaxi'] (ordenadas pelo tamanho)
```

### 3.10. `list.reverse()`

Inverte a ordem dos elementos da lista **in-place**.

**Sintaxe:**  
`list.reverse()`

**Retorno:** `None`.

**Exemplo:**
```python
lista = [1, 2, 3, 4]
lista.reverse()
print(lista)  # [4, 3, 2, 1]
```

### 3.11. `list.copy()`

Retorna uma **cópia superficial** (shallow copy) da lista.

**Sintaxe:**  
`list.copy()`

**Retorno:** nova lista.

**Exemplo:**
```python
original = [1, 2, [3, 4]]
copia = original.copy()
copia[0] = 100
copia[2][0] = 99
print(original)  # [1, 2, [99, 4]]  (alteração no elemento interno reflete)
```

---

## 4. Conjuntos (set)

Conjuntos são coleções não ordenadas, mutáveis e que não permitem elementos duplicados. Muitos métodos realizam operações de teoria de conjuntos.

### 4.1. `set.add(elem)`

Adiciona um elemento `elem` ao conjunto. Se o elemento já existir, nada acontece.

**Sintaxe:**  
`set.add(elem)`

**Parâmetros:**  
- `elem` – elemento a ser adicionado.

**Retorno:** `None`.

**Exemplo:**
```python
conjunto = {1, 2, 3}
conjunto.add(4)
print(conjunto)  # {1, 2, 3, 4}
conjunto.add(2)  # já existe, conjunto permanece {1, 2, 3, 4}
```

### 4.2. `set.clear()`

Remove todos os elementos do conjunto.

**Sintaxe:**  
`set.clear()`

**Retorno:** `None`.

**Exemplo:**
```python
conjunto = {1, 2, 3}
conjunto.clear()
print(conjunto)  # set()
```

### 4.3. `set.copy()`

Retorna uma **cópia superficial** do conjunto.

**Sintaxe:**  
`set.copy()`

**Retorno:** novo conjunto.

**Exemplo:**
```python
original = {1, 2, 3}
copia = original.copy()
copia.add(4)
print(original)  # {1, 2, 3}
print(copia)     # {1, 2, 3, 4}
```

### 4.4. `set.difference(outro)`

Retorna um novo conjunto contendo os elementos que estão no conjunto original **mas não estão** em `outro`. Equivalente ao operador `-`.

**Sintaxe:**  
`set.difference(outro)`

**Parâmetros:**  
- `outro` – um conjunto (ou iterável) a ser subtraído.

**Retorno:** novo conjunto.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
print(a.difference(b))  # {1, 2}
print(a - b)            # mesmo resultado
```

### 4.5. `set.difference_update(outro)`

Remove do conjunto original todos os elementos que também estão em `outro`. Modifica o conjunto in-place.

**Sintaxe:**  
`set.difference_update(outro)`

**Parâmetros:**  
- `outro` – iterável com elementos a remover.

**Retorno:** `None`.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
a.difference_update(b)
print(a)  # {1, 2}
```

### 4.6. `set.discard(elem)`

Remove o elemento `elem` do conjunto **se ele estiver presente**. Se não estiver, **não gera erro** (diferente de `remove`).

**Sintaxe:**  
`set.discard(elem)`

**Parâmetros:**  
- `elem` – elemento a ser removido.

**Retorno:** `None`.

**Exemplo:**
```python
conjunto = {1, 2, 3}
conjunto.discard(2)
print(conjunto)  # {1, 3}
conjunto.discard(10)  # não causa erro, conjunto permanece {1, 3}
```

### 4.7. `set.intersection(outro)`

Retorna um novo conjunto contendo apenas os elementos que estão **presentes em ambos** (interseção). Equivalente ao operador `&`.

**Sintaxe:**  
`set.intersection(outro)`

**Parâmetros:**  
- `outro` – iterável para interseção.

**Retorno:** novo conjunto.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
print(a.intersection(b))  # {3, 4}
print(a & b)              # mesmo resultado
```

### 4.8. `set.intersection_update(outro)`

Atualiza o conjunto original, mantendo apenas os elementos que também estão em `outro` (interseção in-place).

**Sintaxe:**  
`set.intersection_update(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** `None`.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
a.intersection_update(b)
print(a)  # {3, 4}
```

### 4.9. `set.isdisjoint(outro)`

Retorna `True` se o conjunto não tiver nenhum elemento em comum com `outro` (são disjuntos). Caso contrário, `False`.

**Sintaxe:**  
`set.isdisjoint(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** booleano.

**Exemplo:**
```python
a = {1, 2}
b = {3, 4}
c = {2, 5}
print(a.isdisjoint(b))  # True
print(a.isdisjoint(c))  # False (ambos têm o 2)
```

### 4.10. `set.issubset(outro)`

Retorna `True` se **todos** os elementos do conjunto estão contidos em `outro`. Equivalente ao operador `<=`.

**Sintaxe:**  
`set.issubset(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** booleano.

**Exemplo:**
```python
a = {1, 2}
b = {1, 2, 3}
print(a.issubset(b))  # True
print(a <= b)         # True
print(b.issubset(a))  # False
```

### 4.11. `set.issuperset(outro)`

Retorna `True` se o conjunto contém **todos** os elementos de `outro`. Equivalente ao operador `>=`.

**Sintaxe:**  
`set.issuperset(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** booleano.

**Exemplo:**
```python
a = {1, 2, 3}
b = {1, 2}
print(a.issuperset(b))  # True
print(a >= b)           # True
print(b.issuperset(a))  # False
```

### 4.12. `set.pop()`

Remove e retorna um **elemento arbitrário** do conjunto. Como conjuntos não são ordenados, não é possível prever qual elemento será removido.

**Sintaxe:**  
`set.pop()`

**Retorno:** o elemento removido.  
**Exceção:** `KeyError` se o conjunto estiver vazio.

**Exemplo:**
```python
conjunto = {10, 20, 30}
elem = conjunto.pop()
print(elem)     # pode ser 10, 20 ou 30
print(conjunto) # conjunto sem o elemento removido
```

### 4.13. `set.remove(elem)`

Remove o elemento `elem` do conjunto. Se o elemento não existir, lança `KeyError`.

**Sintaxe:**  
`set.remove(elem)`

**Parâmetros:**  
- `elem` – elemento a ser removido.

**Retorno:** `None`.  
**Exceção:** `KeyError` se `elem` não estiver presente.

**Exemplo:**
```python
conjunto = {1, 2, 3}
conjunto.remove(2)
print(conjunto)  # {1, 3}
# conjunto.remove(10)  # KeyError: 10
```

### 4.14. `set.symmetric_difference(outro)`

Retorna um novo conjunto com elementos que estão em **apenas um dos conjuntos** (diferença simétrica). Equivalente ao operador `^`.

**Sintaxe:**  
`set.symmetric_difference(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** novo conjunto.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
print(a.symmetric_difference(b))  # {1, 2, 5}
print(a ^ b)                       # mesmo resultado
```

### 4.15. `set.symmetric_difference_update(outro)`

Atualiza o conjunto original, mantendo apenas os elementos que estão em um dos conjuntos, mas não em ambos (diferença simétrica in-place).

**Sintaxe:**  
`set.symmetric_difference_update(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** `None`.

**Exemplo:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5}
a.symmetric_difference_update(b)
print(a)  # {1, 2, 5}
```

### 4.16. `set.union(outro)`

Retorna um novo conjunto contendo todos os elementos do conjunto original e de `outro` (união). Equivalente ao operador `|`.

**Sintaxe:**  
`set.union(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** novo conjunto.

**Exemplo:**
```python
a = {1, 2}
b = {2, 3}
print(a.union(b))  # {1, 2, 3}
print(a | b)       # mesmo resultado
```

### 4.17. `set.update(outro)`

Adiciona ao conjunto original todos os elementos de `outro` (união in-place). Equivalente ao operador `|=`.

**Sintaxe:**  
`set.update(outro)`

**Parâmetros:**  
- `outro` – iterável.

**Retorno:** `None`.

**Exemplo:**
```python
a = {1, 2}
b = {2, 3}
a.update(b)
print(a)  # {1, 2, 3}
```

