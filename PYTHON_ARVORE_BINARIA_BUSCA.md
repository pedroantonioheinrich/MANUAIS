# Manual Completo: Busca em Árvore Binária com Python

## Índice
1. [Introdução às Árvores Binárias](#introdução)
2. [Conceitos Fundamentais](#conceitos)
3. [Implementação da Estrutura](#implementação)
4. [Algoritmos de Busca](#algoritmos-busca)
5. [Análise de Complexidade](#complexidade)
6. [Exemplos Práticos](#exemplos)
7. [Exercícios Resolvidos](#exercicios)
8. [Casos Especiais](#casos-especiais)
9. [Otimizações](#otimizações)
10. [Referências](#referências)

---

## 1. Introdução às Árvores Binárias {#introdução}

### O que é uma Árvore Binária?
Uma árvore binária é uma estrutura de dados hierárquica onde cada nó possui no máximo dois filhos: esquerdo e direito.

### Por que usar Árvores Binárias?
- Busca eficiente (O(log n) em média)
- Inserção e remoção dinâmicas
- Representação natural de dados hierárquicos

---

## 2. Conceitos Fundamentais {#conceitos}

### Terminologia Básica
- **Raiz**: Nó principal da árvore
- **Folha**: Nó sem filhos
- **Altura**: Maior distância da raiz até uma folha
- **Subárvore**: Qualquer nó com seus descendentes

### Tipos de Árvores Binárias
```python
# Árvore Binária Comum
# Árvore Binária de Busca (BST)
# Árvore AVL (balanceada)
# Árvore Rubro-Negra
```

---

## 3. Implementação da Estrutura {#implementação}

### Classe Nó
```python
class No:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None
        self.pai = None  # Opcional, útil para algumas operações
```

### Classe Árvore Binária
```python
class ArvoreBinaria:
    def __init__(self):
        self.raiz = None
        self.tamanho = 0
    
    def vazia(self):
        return self.raiz is None
    
    def tamanho(self):
        return self.tamanho
```

---

## 4. Algoritmos de Busca {#algoritmos-busca}

### 4.1 Busca em Árvore Binária Comum

#### Busca por Valor (Qualquer Árvore)
```python
def buscar(self, valor):
    """
    Busca um valor na árvore (percurso pré-ordem)
    Retorna o nó encontrado ou None
    """
    return self._buscar_recursivo(self.raiz, valor)

def _buscar_recursivo(self, no, valor):
    # Caso base: nó vazio ou valor encontrado
    if no is None or no.valor == valor:
        return no
    
    # Busca na esquerda
    esquerda = self._buscar_recursivo(no.esquerda, valor)
    if esquerda:
        return esquerda
    
    # Busca na direita
    return self._buscar_recursivo(no.direita, valor)
```

### 4.2 Busca em Árvore Binária de Busca (BST)

#### Busca Otimizada (BST)
```python
def buscar_bst(self, valor):
    """
    Busca otimizada para BST - O(log n) em média
    """
    return self._buscar_bst_recursivo(self.raiz, valor)

def _buscar_bst_recursivo(self, no, valor):
    if no is None or no.valor == valor:
        return no
    
    if valor < no.valor:
        return self._buscar_bst_recursivo(no.esquerda, valor)
    else:
        return self._buscar_bst_recursivo(no.direita, valor)

# Versão iterativa (mais eficiente em termos de memória)
def buscar_bst_iterativo(self, valor):
    no_atual = self.raiz
    
    while no_atual is not None:
        if no_atual.valor == valor:
            return no_atual
        elif valor < no_atual.valor:
            no_atual = no_atual.esquerda
        else:
            no_atual = no_atual.direita
    
    return None
```

### 4.3 Busca por Condição

#### Encontrar Nós que Satisfazem uma Condição
```python
def buscar_por_condicao(self, condicao):
    """
    Retorna todos os nós que satisfazem uma condição
    condicao: função que recebe um valor e retorna bool
    """
    resultados = []
    self._buscar_condicao_recursivo(self.raiz, condicao, resultados)
    return resultados

def _buscar_condicao_recursivo(self, no, condicao, resultados):
    if no is None:
        return
    
    if condicao(no.valor):
        resultados.append(no)
    
    self._buscar_condicao_recursivo(no.esquerda, condicao, resultados)
    self._buscar_condicao_recursivo(no.direita, condicao, resultados)

# Exemplo de uso:
# arvore.buscar_por_condicao(lambda x: x > 10)
```

### 4.4 Busca do Maior/Menor Elemento

```python
def encontrar_minimo(self):
    """Encontra o menor valor na árvore"""
    if self.raiz is None:
        return None
    
    no_atual = self.raiz
    while no_atual.esquerda is not None:
        no_atual = no_atual.esquerda
    
    return no_atual.valor

def encontrar_maximo(self):
    """Encontra o maior valor na árvore"""
    if self.raiz is None:
        return None
    
    no_atual = self.raiz
    while no_atual.direita is not None:
        no_atual = no_atual.direita
    
    return no_atual.valor
```

### 4.5 Busca por Nível (BFS)

```python
from collections import deque

def buscar_por_nivel(self, valor):
    """
    Busca usando busca em largura (BFS)
    Útil para encontrar o caminho mais curto
    """
    if self.raiz is None:
        return None
    
    fila = deque([(self.raiz, 0)])  # (nó, nível)
    
    while fila:
        no, nivel = fila.popleft()
        
        if no.valor == valor:
            return no, nivel
        
        if no.esquerda:
            fila.append((no.esquerda, nivel + 1))
        if no.direita:
            fila.append((no.direita, nivel + 1))
    
    return None
```

---

## 5. Análise de Complexidade {#complexidade}

### Tabela de Complexidades

| Tipo de Árvore | Busca (Médio) | Busca (Pior) |
|----------------|---------------|--------------|
| Árvore Comum | O(n) | O(n) |
| BST não balanceada | O(log n) | O(n) |
| BST balanceada | O(log n) | O(log n) |

### Comparativo de Busca
```python
import time
import random

def comparar_buscas(arvore, valores_teste):
    """Compara diferentes métodos de busca"""
    
    # Busca em árvore comum
    inicio = time.time()
    for valor in valores_teste:
        arvore.buscar(valor)
    tempo_comum = time.time() - inicio
    
    # Busca em BST (se aplicável)
    inicio = time.time()
    for valor in valores_teste:
        arvore.buscar_bst(valor)
    tempo_bst = time.time() - inicio
    
    print(f"Busca comum: {tempo_comum:.4f}s")
    print(f"Busca BST: {tempo_bst:.4f}s")
```

---

## 6. Exemplos Práticos {#exemplos}

### Exemplo 1: Sistema de Arquivos
```python
class Arquivo:
    def __init__(self, nome, tamanho):
        self.nome = nome
        self.tamanho = tamanho

class SistemaArquivos(ArvoreBinaria):
    def buscar_arquivo(self, nome):
        """Busca arquivo por nome"""
        return self._buscar_recursivo(self.raiz, nome)
    
    def buscar_por_tamanho(self, tamanho_min, tamanho_max):
        """Busca arquivos por faixa de tamanho"""
        condicao = lambda x: tamanho_min <= x.tamanho <= tamanho_max
        return self.buscar_por_condicao(condicao)
```

### Exemplo 2: Catálogo de Produtos
```python
class Produto:
    def __init__(self, id, nome, preco):
        self.id = id
        self.nome = nome
        self.preco = preco

class CatalogoProdutos:
    def __init__(self):
        self.por_id = ArvoreBinaria()
        self.por_preco = ArvoreBinaria()
    
    def buscar_por_id(self, id):
        return self.por_id.buscar_bst(id)
    
    def buscar_por_faixa_preco(self, min_preco, max_preco):
        return self.por_preco.buscar_por_condicao(
            lambda p: min_preco <= p.preco <= max_preco
        )
```

---

## 7. Exercícios Resueltos {#exercicios}

### Exercício 1: Verificar se Valor Existe
```python
def existe(self, valor):
    """Verifica se um valor existe na árvore"""
    return self.buscar(valor) is not None

# Teste
arvore = ArvoreBinaria()
# ... inserir valores
print(arvore.existe(10))  # True/False
```

### Exercício 2: Contar Ocorrências
```python
def contar_ocorrencias(self, valor):
    """Conta quantas vezes um valor aparece"""
    def contar(no):
        if no is None:
            return 0
        
        count = 1 if no.valor == valor else 0
        return count + contar(no.esquerda) + contar(no.direita)
    
    return contar(self.raiz)
```

### Exercício 3: Encontrar Ancestral Comum
```python
def ancestral_comum(self, valor1, valor2):
    """
    Encontra o ancestral comum mais próximo de dois valores
    (funciona apenas em BST)
    """
    def encontrar(no):
        if no is None:
            return None
        
        if valor1 < no.valor and valor2 < no.valor:
            return encontrar(no.esquerda)
        elif valor1 > no.valor and valor2 > no.valor:
            return encontrar(no.direita)
        else:
            return no
    
    return encontrar(self.raiz)
```

---

## 8. Casos Especiais {#casos-especiais}

### 8.1 Busca em Árvore com Valores Duplicados
```python
def buscar_todos(self, valor):
    """
    Retorna todas as ocorrências de um valor
    Útil quando há valores duplicados
    """
    resultados = []
    self._buscar_todos_recursivo(self.raiz, valor, resultados)
    return resultados

def _buscar_todos_recursivo(self, no, valor, resultados):
    if no is None:
        return
    
    if no.valor == valor:
        resultados.append(no)
    
    # Em BST com duplicatas, geralmente coloca-se duplicatas à direita
    self._buscar_todos_recursivo(no.esquerda, valor, resultados)
    self._buscar_todos_recursivo(no.direita, valor, resultados)
```

### 8.2 Busca por Prefixo (em árvore de strings)
```python
def buscar_por_prefixo(self, prefixo):
    """
    Busca strings que começam com um determinado prefixo
    Útil para autocomplete
    """
    resultados = []
    self._buscar_prefixo_recursivo(self.raiz, prefixo, "", resultados)
    return resultados

def _buscar_prefixo_recursivo(self, no, prefixo, atual, resultados):
    if no is None:
        return
    
    nova_str = atual + str(no.valor)
    
    if nova_str.startswith(prefixo):
        resultados.append(nova_str)
    
    self._buscar_prefixo_recursivo(no.esquerda, prefixo, nova_str, resultados)
    self._buscar_prefixo_recursivo(no.direita, prefixo, nova_str, resultados)
```

---

## 9. Otimizações {#otimizações}

### 9.1 Memoização para Buscas Frequentes
```python
class ArvoreComCache:
    def __init__(self):
        self.raiz = None
        self.cache = {}
        self.tamanho_cache = 100
    
    def buscar_com_cache(self, valor):
        """Busca com cache para valores frequentemente acessados"""
        if valor in self.cache:
            return self.cache[valor]
        
        resultado = self.buscar_bst(valor)
        
        if len(self.cache) < self.tamanho_cache:
            self.cache[valor] = resultado
        
        return resultado
```

### 9.2 Busca com Limite de Profundidade
```python
def buscar_com_profundidade(self, valor, profundidade_max):
    """
    Busca limitada por profundidade
    Útil para árvores muito grandes
    """
    return self._buscar_profundidade_rec(self.raiz, valor, 0, profundidade_max)

def _buscar_profundidade_rec(self, no, valor, profundidade, max_prof):
    if no is None or profundidade > max_prof:
        return None
    
    if no.valor == valor:
        return no
    
    esquerda = self._buscar_profundidade_rec(no.esquerda, valor, profundidade + 1, max_prof)
    if esquerda:
        return esquerda
    
    return self._buscar_profundidade_rec(no.direita, valor, profundidade + 1, max_prof)
```

### 9.3 Busca Paralela
```python
from concurrent.futures import ThreadPoolExecutor

def buscar_paralela(self, valores):
    """
    Busca múltiplos valores em paralelo
    """
    with ThreadPoolExecutor(max_workers=4) as executor:
        resultados = list(executor.map(self.buscar_bst, valores))
    return dict(zip(valores, resultados))
```

---

## 10. Referências {#referências}

### Livros Recomendados
- "Estruturas de Dados e Algoritmos em Python" - Michael T. Goodrich
- "Introduction to Algorithms" - Cormen et al.

### Documentação Online
- Python.org - Data Structures
- GeeksforGeeks - Binary Search Tree
- Real Python - Tree Data Structures

### Próximos Passos
- Implementar árvores balanceadas (AVL, Red-Black)
- Estudar algoritmos de travessia (in-order, pre-order, post-order)
- Explorar aplicações práticas (bancos de dados, sistemas de arquivos)

---

## Exemplo Completo: Implementação Integrada

```python
class No:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None

class ArvoreBinariaBusca:
    def __init__(self):
        self.raiz = None
    
    def inserir(self, valor):
        if self.raiz is None:
            self.raiz = No(valor)
        else:
            self._inserir_recursivo(self.raiz, valor)
    
    def _inserir_recursivo(self, no, valor):
        if valor < no.valor:
            if no.esquerda is None:
                no.esquerda = No(valor)
            else:
                self._inserir_recursivo(no.esquerda, valor)
        elif valor > no.valor:
            if no.direita is None:
                no.direita = No(valor)
            else:
                self._inserir_recursivo(no.direita, valor)
        # Se for igual, ignoramos (ou podemos tratar duplicatas)
    
    def buscar(self, valor):
        """Busca iterativa"""
        no_atual = self.raiz
        
        while no_atual:
            if no_atual.valor == valor:
                return no_atual
            elif valor < no_atual.valor:
                no_atual = no_atual.esquerda
            else:
                no_atual = no_atual.direita
        
        return None
    
    def buscar_recursivo(self, valor):
        """Busca recursiva"""
        return self._buscar_rec(self.raiz, valor)
    
    def _buscar_rec(self, no, valor):
        if no is None or no.valor == valor:
            return no
        
        if valor < no.valor:
            return self._buscar_rec(no.esquerda, valor)
        else:
            return self._buscar_rec(no.direita, valor)
    
    def encontrar_minimo(self):
        if self.raiz is None:
            return None
        
        no_atual = self.raiz
        while no_atual.esquerda:
            no_atual = no_atual.esquerda
        return no_atual.valor
    
    def encontrar_maximo(self):
        if self.raiz is None:
            return None
        
        no_atual = self.raiz
        while no_atual.direita:
            no_atual = no_atual.direita
        return no_atual.valor
    
    def exibir_em_ordem(self):
        """Exibe os valores em ordem crescente"""
        elementos = []
        self._em_ordem(self.raiz, elementos)
        return elementos
    
    def _em_ordem(self, no, elementos):
        if no:
            self._em_ordem(no.esquerda, elementos)
            elementos.append(no.valor)
            self._em_ordem(no.direita, elementos)

# Exemplo de uso
if __name__ == "__main__":
    # Criando árvore
    arvore = ArvoreBinariaBusca()
    
    # Inserindo valores
    valores = [50, 30, 70, 20, 40, 60, 80]
    for v in valores:
        arvore.inserir(v)
    
    # Testando buscas
    print("Árvore em ordem:", arvore.exibir_em_ordem())
    print("Buscar 40:", arvore.buscar(40).valor if arvore.buscar(40) else "Não encontrado")
    print("Buscar 100:", arvore.buscar(100))
    print("Mínimo:", arvore.encontrar_minimo())
    print("Máximo:", arvore.encontrar_maximo())
    
    # Teste com valores não encontrados
    print("Buscar 90 (recursivo):", 
          arvore.buscar_recursivo(90).valor if arvore.buscar_recursivo(90) else "Não encontrado")
```

