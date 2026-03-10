# Manual Completo de Loops em Python

## Índice
1. [Introdução](#introdução)
2. [Loop FOR](#loop-for)
3. [Loop WHILE](#loop-while)
4. [Controle de Loops](#controle-de-loops)
5. [Loops Aninhados](#loops-aninhados)
6. [Compreensão de Listas](#compreensão-de-listas)
7. [Boas Práticas](#boas-práticas)
8. [Exercícios Práticos](#exercícios-práticos)

## Introdução

Loops são estruturas fundamentais em programação que permitem executar um bloco de código repetidamente. Python oferece dois tipos principais de loops:

- **`for` loop**: Itera sobre uma sequência (lista, tupla, string, etc.)
- **`while` loop**: Executa enquanto uma condição for verdadeira

## Loop FOR

### Sintaxe Básica
```python
for variável in sequência:
    # bloco de código
```

### Exemplos Práticos

#### Iterando sobre listas
```python
# Lista simples
frutas = ["maçã", "banana", "laranja"]
for fruta in frutas:
    print(f"Eu gosto de {fruta}")

# Com índice usando enumerate()
for indice, fruta in enumerate(frutas):
    print(f"{indice + 1}: {fruta}")
```

#### Iterando sobre strings
```python
palavra = "Python"
for letra in palavra:
    print(letra, end="-")  # P-y-t-h-o-n-
```

#### Usando range()
```python
# range(stop)
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# range(start, stop)
for i in range(2, 7):
    print(i)  # 2, 3, 4, 5, 6

# range(start, stop, step)
for i in range(1, 10, 2):
    print(i)  # 1, 3, 5, 7, 9

# Range decrescente
for i in range(10, 0, -1):
    print(i)  # 10, 9, 8, ..., 1
```

#### Iterando sobre dicionários
```python
pessoa = {
    "nome": "João",
    "idade": 30,
    "cidade": "São Paulo"
}

# Chaves
for chave in pessoa:
    print(chave)

# Valores
for valor in pessoa.values():
    print(valor)

# Pares chave-valor
for chave, valor in pessoa.items():
    print(f"{chave}: {valor}")
```

## Loop WHILE

### Sintaxe Básica
```python
while condição:
    # bloco de código
```

### Exemplos Práticos

#### Contador simples
```python
contador = 0
while contador < 5:
    print(f"Contagem: {contador}")
    contador += 1
```

#### Loop com input do usuário
```python
resposta = ""
while resposta.lower() != "sair":
    resposta = input("Digite 'sair' para encerrar: ")
    print(f"Você digitou: {resposta}")
```

#### Validação de entrada
```python
idade = -1
while idade < 0 or idade > 120:
    try:
        idade = int(input("Digite sua idade (0-120): "))
        if idade < 0 or idade > 120:
            print("Idade inválida! Tente novamente.")
    except ValueError:
        print("Por favor, digite um número válido!")

print(f"Idade válida: {idade}")
```

## Controle de Loops

### Break - Interrompe o loop
```python
# Encontrar o primeiro número par
numeros = [1, 3, 5, 7, 8, 9, 10]
for num in numeros:
    if num % 2 == 0:
        print(f"Primeiro par encontrado: {num}")
        break
```

### Continue - Pula para próxima iteração
```python
# Imprimir apenas números ímpares
for i in range(10):
    if i % 2 == 0:
        continue
    print(f"Número ímpar: {i}")
```

### Else em loops
```python
# Else executado quando o loop termina normalmente (sem break)
for i in range(5):
    print(i)
else:
    print("Loop concluído com sucesso!")

# Else NÃO executado quando break é acionado
for i in range(5):
    if i == 3:
        break
    print(i)
else:
    print("Este else não será executado")
```

## Loops Aninhados

### Exemplos práticos

#### Matrizes
```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for linha in matriz:
    for coluna in linha:
        print(coluna, end=" ")
    print()  # Nova linha
```

#### Tabela de multiplicação
```python
for i in range(1, 11):
    for j in range(1, 11):
        print(f"{i * j:4}", end="")
    print()  # Nova linha
```

## Compreensão de Listas

### Sintaxe e exemplos
```python
# Sintaxe: [expressão for item in iterável if condição]

# Lista de quadrados
quadrados = [x**2 for x in range(10)]
print(quadrados)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Números pares
pares = [x for x in range(20) if x % 2 == 0]
print(pares)  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Transformar strings
nomes = ["joão", "maria", "pedro"]
nomes_capitalizados = [nome.capitalize() for nome in nomes]
print(nomes_capitalizados)  # ['João', 'Maria', 'Pedro']

# Compreensão aninhada
matriz = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(matriz)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

## Boas Práticas

### 1. Evite loops infinitos
```python
# SEMPRE tenha uma condição de parada clara
contador = 0
while True:  # Use com cuidado!
    print(contador)
    contador += 1
    if contador >= 5:
        break  # Sempre tenha uma saída
```

### 2. Prefira for sobre while quando possível
```python
# Ruim
i = 0
while i < len(lista):
    print(lista[i])
    i += 1

# Bom
for item in lista:
    print(item)
```

### 3. Use enumerate para índices
```python
# Ruim
for i in range(len(lista)):
    print(i, lista[i])

# Bom
for i, item in enumerate(lista):
    print(i, item)
```

### 4. Evite modificar a lista durante iteração
```python
# Ruim - pode causar comportamento inesperado
lista = [1, 2, 3, 4, 5]
for item in lista:
    if item % 2 == 0:
        lista.remove(item)

# Bom - crie uma nova lista
lista = [1, 2, 3, 4, 5]
lista = [item for item in lista if item % 2 != 0]
```

## Exercícios Práticos

### Exercício 1: Soma de números
```python
# Calcule a soma dos números de 1 a 100
soma = 0
for i in range(1, 101):
    soma += i
print(f"Soma: {soma}")  # 5050
```

### Exercício 2: Números primos
```python
# Encontre todos os números primos até 50
def eh_primo(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

primos = [n for n in range(50) if eh_primo(n)]
print(f"Números primos até 50: {primos}")
```

### Exercício 3: Padrão de asteriscos
```python
# Imprima um triângulo de asteriscos
n = 5
for i in range(1, n + 1):
    print('*' * i)
# Saída:
# *
# **
# ***
# ****
# *****
```

### Exercício 4: Jogo de adivinhação
```python
import random

numero_secreto = random.randint(1, 100)
tentativas = 0
max_tentativas = 10

print("Tente adivinhar o número entre 1 e 100!")

while tentativas < max_tentativas:
    try:
        palpite = int(input(f"Tentativa {tentativas + 1}/{max_tentativas}: "))
        tentativas += 1
        
        if palpite == numero_secreto:
            print(f"Parabéns! Você acertou em {tentativas} tentativas!")
            break
        elif palpite < numero_secreto:
            print("Tente um número maior!")
        else:
            print("Tente um número menor!")
    except ValueError:
        print("Digite um número válido!")
else:
    print(f"Game over! O número era {numero_secreto}")
```

### Exercício 5: Análise de texto
```python
# Conte vogais em uma frase
frase = "Python é uma linguagem incrível!"
vogais = 'aeiouáéíóúâêîôûãõàèìòù'
contador = 0

for letra in frase.lower():
    if letra in vogais:
        contador += 1

print(f"A frase tem {contador} vogais")
```

## Dicas Avançadas

### zip() para iterar múltiplas listas
```python
nomes = ["Ana", "Bruno", "Carla"]
idades = [25, 30, 35]
cidades = ["RJ", "SP", "BH"]

for nome, idade, cidade in zip(nomes, idades, cidades):
    print(f"{nome} tem {idade} anos e mora em {cidade}")
```

### reversed() para iteração reversa
```python
for i in reversed(range(5)):
    print(i)  # 4, 3, 2, 1, 0
```

### sorted() para iteração ordenada
```python
numeros = [3, 1, 4, 1, 5, 9, 2]
for num in sorted(numeros):
    print(num, end=" ")  # 1 1 2 3 4 5 9
```
