# Manual Completo sobre Matrizes em Python

## Índice
1. [Introdução](#introdução)
2. [Representação de Matrizes](#representação-de-matrizes)
   - [Usando listas de listas](#usando-listas-de-listas)
   - [Usando NumPy](#usando-numpy)
3. [Operações Básicas](#operações-básicas)
   - [Acesso a elementos](#acesso-a-elementos)
   - [Soma e subtração](#soma-e-subtração)
   - [Multiplicação por escalar](#multiplicação-por-escalar)
4. [Multiplicação de Matrizes](#multiplicação-de-matrizes)
   - [Produto matricial](#produto-matricial)
   - [Multiplicação elemento a elemento](#multiplicação-elemento-a-elemento)
5. [Transposição](#transposição)
6. [Matrizes Especiais](#matrizes-especiais)
   - [Matriz identidade](#matriz-identidade)
   - [Matriz de zeros e uns](#matriz-de-zeros-e-uns)
   - [Matriz diagonal](#matriz-diagonal)
7. [Operações com NumPy](#operações-com-numpy)
   - [Criação e propriedades](#criação-e-propriedades)
   - [Funções úteis](#funções-úteis)
8. [Álgebra Linear com NumPy](#álgebra-linear-com-numpy)
   - [Determinante](#determinante)
   - [Inversa](#inversa)
   - [Autovalores e autovetores](#autovalores-e-autovetores)
   - [Sistemas lineares](#sistemas-lineares)
9. [Indexação e Slicing](#indexação-e-slicing)
10. [Broadcasting](#broadcasting)
11. [Exemplos Práticos](#exemplos-práticos)
12. [Considerações Finais](#considerações-finais)

---

## Introdução

Matrizes são estruturas matemáticas fundamentais usadas em diversas áreas, como computação gráfica, machine learning, física, engenharia e processamento de imagens. Em Python, podemos trabalhar com matrizes de forma simples usando estruturas nativas (listas de listas) ou, de maneira mais eficiente e completa, utilizando a biblioteca **NumPy**. Este manual abordará ambos os métodos, com ênfase no NumPy, que é o padrão para computação numérica em Python.

## Representação de Matrizes

### Usando listas de listas

Em Python puro, uma matriz pode ser representada como uma lista de listas, onde cada lista interna representa uma linha.

```python
# Matriz 2x3 (2 linhas, 3 colunas)
matriz = [[1, 2, 3],
          [4, 5, 6]]

print(matriz)
```

**Limitações:** Operações matemáticas como multiplicação de matrizes precisam ser implementadas manualmente, o que é trabalhoso e ineficiente para matrizes grandes.

### Usando NumPy

NumPy é a biblioteca mais utilizada para manipulação de arrays e matrizes em Python. Para usá-la, é necessário instalá-la (`pip install numpy`) e importá-la.

```python
import numpy as np

# Criando uma matriz 2x3
matriz = np.array([[1, 2, 3],
                   [4, 5, 6]])

print(matriz)
```

NumPy oferece inúmeras funções otimizadas para operações matriciais, além de integração com outras bibliotecas científicas.

## Operações Básicas

### Acesso a elementos

Com listas de listas:
```python
matriz = [[1, 2, 3], [4, 5, 6]]
elemento = matriz[1][2]  # linha 1, coluna 2 → 6
```

Com NumPy:
```python
matriz = np.array([[1, 2, 3], [4, 5, 6]])
elemento = matriz[1, 2]  # sintaxe mais limpa
```

### Soma e subtração

Para somar ou subtrair duas matrizes de mesma dimensão, basta usar os operadores `+` e `-`.

**Com listas de listas (implementação manual):**
```python
A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]
C = [[A[i][j] + B[i][j] for j in range(2)] for i in range(2)]
print(C)  # [[6, 8], [10, 12]]
```

**Com NumPy:**
```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A + B
print(C)  # [[6, 8], [10, 12]]
```

### Multiplicação por escalar

Multiplicar todos os elementos por um número:

**Com listas:**
```python
escalar = 3
A = [[1, 2], [3, 4]]
B = [[escalar * elem for elem in linha] for linha in A]
```

**Com NumPy:**
```python
A = np.array([[1, 2], [3, 4]])
B = 3 * A
```

## Multiplicação de Matrizes

### Produto matricial

O produto matricial (multiplicação de matrizes) segue a regra `(m x n) * (n x p) = (m x p)`. Não é uma multiplicação elemento a elemento.

**Com listas (implementação ingênua):**
```python
def multiplicar_matrizes(A, B):
    m = len(A)
    n = len(A[0])
    p = len(B[0])
    # Verifica se n == len(B) (número de colunas de A = linhas de B)
    if n != len(B):
        raise ValueError("Dimensões incompatíveis")
    C = [[0] * p for _ in range(m)]
    for i in range(m):
        for j in range(p):
            for k in range(n):
                C[i][j] += A[i][k] * B[k][j]
    return C

A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]
C = multiplicar_matrizes(A, B)
print(C)  # [[19, 22], [43, 50]]
```

**Com NumPy:**
```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = np.dot(A, B)       # ou A @ B (Python 3.5+)
print(C)  # [[19, 22], [43, 50]]
```

### Multiplicação elemento a elemento

Em algumas aplicações, deseja-se multiplicar cada elemento de uma matriz pelo correspondente na outra (produto de Hadamard). Com NumPy, usa-se o operador `*`.

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A * B
print(C)  # [[5, 12], [21, 32]]
```

## Transposição

A transposta de uma matriz troca linhas por colunas.

**Com listas:**
```python
A = [[1, 2, 3], [4, 5, 6]]
transposta = [[A[j][i] for j in range(len(A))] for i in range(len(A[0]))]
print(transposta)  # [[1, 4], [2, 5], [3, 6]]
```

**Com NumPy:**
```python
A = np.array([[1, 2, 3], [4, 5, 6]])
transposta = A.T
print(transposta)
```

## Matrizes Especiais

### Matriz identidade

Matriz quadrada com 1 na diagonal principal e 0 no resto.

**Com listas:**
```python
def identidade(n):
    return [[1 if i == j else 0 for j in range(n)] for i in range(n)]

I = identidade(3)  # [[1,0,0],[0,1,0],[0,0,1]]
```

**Com NumPy:**
```python
I = np.eye(3)
```

### Matriz de zeros e uns

**Com listas:**
```python
zeros = [[0] * 3 for _ in range(2)]   # 2x3
uns = [[1] * 4 for _ in range(2)]     # 2x4
```

**Com NumPy:**
```python
zeros = np.zeros((2, 3))
uns = np.ones((2, 4))
```

### Matriz diagonal

Cria uma matriz com elementos fornecidos na diagonal.

**Com listas:**
```python
def diagonal(diag):
    n = len(diag)
    return [[diag[i] if i == j else 0 for j in range(n)] for i in range(n)]

D = diagonal([1, 2, 3])
```

**Com NumPy:**
```python
D = np.diag([1, 2, 3])
```

## Operações com NumPy

### Criação e propriedades

Além de `np.array`, existem outras formas de criar matrizes:

```python
# Array de 0 a 9 (10 elementos) e depois reshape para 2x5
a = np.arange(10).reshape(2, 5)

# Valores linearmente espaçados
b = np.linspace(0, 1, 6).reshape(2, 3)

# Matriz aleatória
c = np.random.rand(3, 3)      # uniforme [0,1)
d = np.random.randint(0, 10, size=(2, 4))

# Propriedades
print(a.shape)    # (2, 5)
print(a.ndim)     # 2
print(a.size)     # 10
print(a.dtype)    # tipo dos elementos
```

### Funções úteis

- `np.sum(matriz, axis=0)` – soma ao longo das colunas (axis=0) ou linhas (axis=1).
- `np.mean(matriz)` – média de todos os elementos.
- `np.max(matriz)` – maior elemento.
- `np.min(matriz)` – menor elemento.
- `np.std(matriz)` – desvio padrão.
- `np.concatenate((A, B), axis=0)` – concatena verticalmente.
- `np.vstack((A, B))` – empilha verticalmente.
- `np.hstack((A, B))` – empilha horizontalmente.

## Álgebra Linear com NumPy

O submódulo `numpy.linalg` oferece funções de álgebra linear.

### Determinante

```python
A = np.array([[1, 2], [3, 4]])
det = np.linalg.det(A)
print(det)  # -2.0
```

### Inversa

```python
A = np.array([[1, 2], [3, 4]])
A_inv = np.linalg.inv(A)
print(A_inv)
# [[-2. ,  1. ],
#  [ 1.5, -0.5]]
```

### Autovalores e autovetores

```python
A = np.array([[1, 2], [2, 1]])
autovalores, autovetores = np.linalg.eig(A)
print(autovalores)      # [ 3. -1.]
print(autovetores)      # [[ 0.70710678 -0.70710678], [ 0.70710678  0.70710678]]
```

### Sistemas lineares

Resolver `Ax = b`:

```python
A = np.array([[3, 1], [1, 2]])
b = np.array([9, 8])
x = np.linalg.solve(A, b)
print(x)  # [2. 3.]
```

## Indexação e Slicing

Com NumPy, podemos extrair submatrizes de forma intuitiva:

```python
matriz = np.array([[1, 2, 3, 4],
                   [5, 6, 7, 8],
                   [9,10,11,12]])

# Primeira linha
linha0 = matriz[0, :]        # [1 2 3 4]

# Segunda coluna
coluna1 = matriz[:, 1]       # [2 6 10]

# Submatriz das linhas 0 e 1, colunas 1 e 2
sub = matriz[0:2, 1:3]       # [[2 3], [6 7]]

# Indexação booleana
mascara = matriz > 5
maiores = matriz[mascara]    # array([ 6, 7, 8, 9,10,11,12])
```

## Broadcasting

Broadcasting é um mecanismo que permite operações entre arrays de formatos diferentes, desde que sejam compatíveis. Por exemplo, somar um vetor a cada linha de uma matriz:

```python
A = np.array([[1, 2, 3],
              [4, 5, 6]])
v = np.array([10, 20, 30])

# v é "esticado" para caber em A (broadcasting)
resultado = A + v
print(resultado)
# [[11 22 33]
#  [14 25 36]]
```

Regras: as dimensões são comparadas da direita para a esquerda; são compatíveis se forem iguais ou uma delas for 1.

## Exemplos Práticos

### Exemplo 1: Transformação de coordenadas

Considere um conjunto de pontos 2D e uma matriz de rotação. Podemos aplicar a rotação a todos os pontos de uma só vez.

```python
import numpy as np

# Pontos: cada coluna é um ponto (x, y)
pontos = np.array([[0, 1, 2],
                   [0, 1, 1]])

# Matriz de rotação de 90 graus
theta = np.radians(90)
R = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])

# Aplicar rotação: R (2x2) @ pontos (2x3) = (2x3)
pontos_rot = R @ pontos
print(pontos_rot)
# [[ 0., -1., -2.],
#  [ 0.,  1.,  1.]]
```

### Exemplo 2: Regressão linear com mínimos quadrados

Resolver o sistema sobredeterminado `Xβ = y` para encontrar os coeficientes β.

```python
import numpy as np

# Dados de exemplo
X = np.array([[1, 1],   # 1 para o intercepto
              [1, 2],
              [1, 3],
              [1, 4]])
y = np.array([2, 3, 4, 5])

# Resolver pelo método dos mínimos quadrados
beta, residuos, _, _ = np.linalg.lstsq(X, y, rcond=None)
print(beta)  # [1. 1.]  -> y = 1 + 1*x
```

## Considerações Finais

Neste manual, exploramos desde conceitos básicos até operações avançadas com matrizes em Python. Enquanto listas de listas podem ser úteis para aprendizado e problemas simples, a biblioteca NumPy é indispensável para aplicações reais que exigem desempenho e funcionalidades completas.

Dominar matrizes em Python abre portas para áreas como ciência de dados, aprendizado de máquina, simulações científicas e muito mais. Pratique com os exemplos, explore a documentação do NumPy e experimente criar suas próprias funções matriciais.

---

**Referências:**
- Documentação oficial do NumPy: https://numpy.org/doc/stable/
- Livro "Python para Análise de Dados" – Wes McKinney
