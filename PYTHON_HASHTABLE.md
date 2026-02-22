# Manual Completo: Hash Table (Tabela Hash) em Python

## Índice
1. [Introdução às Hash Tables](#introdução)
2. [Conceitos Fundamentais](#conceitos)
3. [Funções Hash](#funcoes-hash)
4. [Tratamento de Colisões](#colisoes)
5. [Implementação em Python](#implementacao)
6. [Hash Table do Python (dict)](#dict-python)
7. [Análise de Complexidade](#complexidade)
8. [Exemplos Práticos](#exemplos)
9. [Casos de Uso Avançados](#casos-avancados)
10. [Otimizações e Boas Práticas](#otimizacoes)
11. [Exercícios Resolvidos](#exercicios)
12. [Referências](#referencias)

---

## 1. Introdução às Hash Tables {#introdução}

### O que é uma Hash Table?
Uma hash table (tabela de dispersão) é uma estrutura de dados que implementa uma matriz associativa, mapeando chaves para valores através de uma função hash.

### Analogia do Mundo Real
Imagine um armário com gavetas numeradas. Para guardar um objeto, você:
1. Calcula o número da gaveta baseado no nome do objeto
2. Coloca o objeto naquela gaveta
3. Para recuperar, basta calcular novamente o número da gaveta

### Por que usar Hash Tables?
- **Velocidade**: Operações em O(1) em média
- **Flexibilidade**: Qualquer tipo imutável pode ser chave
- **Eficiência**: Ideal para buscas rápidas
- **Versatilidade**: Base para dicionários, caches, conjuntos

---

## 2. Conceitos Fundamentais {#conceitos}

### Terminologia Essencial

```python
# Componentes de uma Hash Table
hash_table = {}  # A tabela em si

# Elementos:
# - Chave (key): "nome" (usada para indexação)
# - Valor (value): "João" (dado armazenado)
# - Par chave-valor: "nome": "João"
# - Função hash: transforma chave em índice
# - Bucket: posição onde os dados são armazenados
```

### Propriedades Importantes

```python
class ConceitosHashTable:
    """
    Demonstração dos conceitos fundamentais
    """
    
    @staticmethod
    def propriedades():
        return {
            "determinística": "Mesma entrada → Mesma saída",
            "uniforme": "Distribuição balanceada",
            "rápida": "Cálculo eficiente O(1)",
            "imutável": "Chaves não podem mudar"
        }
    
    @staticmethod
    def requisitos_chave():
        """Chaves devem ser imutáveis"""
        chaves_validas = (
            "strings",      # "nome"
            "inteiros",     # 42
            "tuplas",       # (1, 2, 3)
            "floats",       # 3.14
            "booleanos"     # True, False
        )
        
        chaves_invalidas = (
            "listas",       # [1, 2, 3] - mutáveis
            "dicionários",  # {"a": 1} - mutáveis
            "sets",         # {1, 2, 3} - mutáveis
        )
        
        return chaves_validas, chaves_invalidas
```

---

## 3. Funções Hash {#funcoes-hash}

### 3.1 Função Hash Simples

```python
class FuncaoHashSimples:
    """
    Implementação de funções hash básicas para entendimento
    """
    
    @staticmethod
    def hash_modulo(chave, tamanho_tabela):
        """
        Hash baseada em módulo (mais simples)
        """
        if isinstance(chave, str):
            # Converte string para número (soma dos valores ASCII)
            valor_numerico = sum(ord(c) for c in chave)
        else:
            valor_numerico = hash(chave)  # Usa hash nativo do Python
        
        return valor_numerico % tamanho_tabela
    
    @staticmethod
    def hash_multiplicacao(chave, tamanho_tabela):
        """
        Método da multiplicação (Knuth)
        h(k) = floor(m * (k * A mod 1))
        """
        A = 0.6180339887  # Constante (inverso do número áureo)
        
        if isinstance(chave, str):
            k = sum(ord(c) for c in chave)
        else:
            k = hash(chave)
        
        frac = (k * A) % 1  # Parte fracionária
        return int(tamanho_tabela * frac)
```

### 3.2 Funções Hash para Diferentes Tipos

```python
class FuncoesHashEspecializadas:
    """
    Funções hash para tipos específicos de dados
    """
    
    @staticmethod
    def hash_string_polinomial(string, base=31, mod=2**32):
        """
        Hash polinomial para strings
        Evita colisões em strings similares
        """
        hash_valor = 0
        for char in string:
            hash_valor = (hash_valor * base + ord(char)) % mod
        return hash_valor
    
    @staticmethod
    def hash_objeto_personalizado(objeto, atributos):
        """
        Hash baseado em atributos específicos
        """
        valores = [getattr(objeto, attr) for attr in atributos]
        return hash(tuple(valores))
    
    @staticmethod
    def hash_composto(*args):
        """
        Hash para múltiplos valores
        """
        return hash(tuple(args))
```

### 3.3 Função Hash Criptográfica (Exemplo Didático)

```python
import hashlib

class HashCriptografico:
    """
    Uso de funções hash criptográficas (não recomendado para hash tables)
    """
    
    @staticmethod
    def hash_sha256(chave):
        """
        SHA-256 - Muito lento para hash tables, mas seguro
        """
        if isinstance(chave, str):
            chave = chave.encode('utf-8')
        return hashlib.sha256(chave).hexdigest()
    
    @staticmethod
    def hash_md5_simplificado(chave):
        """
        Versão simplificada - APENAS PARA ESTUDO
        """
        hash_obj = hashlib.md5()
        hash_obj.update(str(chave).encode())
        return int(hash_obj.hexdigest()[:8], 16)
```

---

## 4. Tratamento de Colisões {#colisoes}

### 4.1 Encadeamento (Chaining)

```python
class HashTableChaining:
    """
    Hash Table com tratamento de colisões por encadeamento
    Cada posição contém uma lista de pares chave-valor
    """
    
    def __init__(self, tamanho_inicial=10):
        self.tamanho = tamanho_inicial
        self.tabela = [[] for _ in range(self.tamanho)]
        self.elementos = 0
        self.fator_carga = 0.75
    
    def _hash(self, chave):
        """Função hash simples"""
        return hash(chave) % self.tamanho
    
    def inserir(self, chave, valor):
        """Insere ou atualiza um par chave-valor"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        # Verifica se a chave já existe
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                bucket[i] = (chave, valor)  # Atualiza
                return
        
        # Se não existe, adiciona novo par
        bucket.append((chave, valor))
        self.elementos += 1
        
        # Verifica se precisa redimensionar
        if self.elementos / self.tamanho > self.fator_carga:
            self._redimensionar()
    
    def buscar(self, chave):
        """Busca um valor pela chave"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for k, v in bucket:
            if k == chave:
                return v
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def remover(self, chave):
        """Remove um par chave-valor"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                del bucket[i]
                self.elementos -= 1
                return
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def _redimensionar(self):
        """Redimensiona a tabela quando o fator de carga é excedido"""
        nova_tabela = HashTableChaining(self.tamanho * 2)
        
        # Reinsere todos os elementos
        for bucket in self.tabela:
            for chave, valor in bucket:
                nova_tabela.inserir(chave, valor)
        
        self.tamanho = nova_tabela.tamanho
        self.tabela = nova_tabela.tabela
    
    def __str__(self):
        """Representação da tabela"""
        resultado = []
        for i, bucket in enumerate(self.tabela):
            if bucket:
                resultado.append(f"Bucket {i}: {bucket}")
        return "\n".join(resultado)
```

### 4.2 Endereçamento Aberto (Linear Probing)

```python
class HashTableLinearProbing:
    """
    Hash Table com endereçamento aberto e sondagem linear
    """
    
    def __init__(self, tamanho_inicial=10):
        self.tamanho = tamanho_inicial
        self.tabela = [None] * self.tamanho
        self.removido = object()  # Marcador para posições removidas
        self.elementos = 0
    
    def _hash(self, chave):
        return hash(chave) % self.tamanho
    
    def _proxima_posicao(self, posicao):
        """Retorna próxima posição (sondagem linear)"""
        return (posicao + 1) % self.tamanho
    
    def inserir(self, chave, valor):
        """Insere usando sondagem linear"""
        posicao = self._hash(chave)
        
        # Procura posição livre ou chave existente
        while (self.tabela[posicao] is not None and 
               self.tabela[posicao] is not self.removido):
            if self.tabela[posicao][0] == chave:
                self.tabela[posicao] = (chave, valor)  # Atualiza
                return
            posicao = self._proxima_posicao(posicao)
        
        # Insere na posição encontrada
        self.tabela[posicao] = (chave, valor)
        self.elementos += 1
        
        # Redimensiona se necessário
        if self.elementos / self.tamanho > 0.7:
            self._redimensionar()
    
    def buscar(self, chave):
        """Busca usando sondagem linear"""
        posicao = self._hash(chave)
        posicao_original = posicao
        
        while self.tabela[posicao] is not None:
            if (self.tabela[posicao] is not self.removido and 
                self.tabela[posicao][0] == chave):
                return self.tabela[posicao][1]
            
            posicao = self._proxima_posicao(posicao)
            if posicao == posicao_original:
                break  # Voltou ao início
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def remover(self, chave):
        """Remove marcando posição como removida"""
        posicao = self._hash(chave)
        posicao_original = posicao
        
        while self.tabela[posicao] is not None:
            if (self.tabela[posicao] is not self.removido and 
                self.tabela[posicao][0] == chave):
                self.tabela[posicao] = self.removido
                self.elementos -= 1
                return
            
            posicao = self._proxima_posicao(posicao)
            if posicao == posicao_original:
                break
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def _redimensionar(self):
        """Redimensiona e rehash todos os elementos"""
        tabela_antiga = self.tabela
        self.tamanho *= 2
        self.tabela = [None] * self.tamanho
        self.elementos = 0
        
        for item in tabela_antiga:
            if item is not None and item is not self.removido:
                self.inserir(item[0], item[1])
```

### 4.3 Double Hashing

```python
class HashTableDoubleHashing:
    """
    Hash Table com double hashing para reduzir agrupamento
    """
    
    def __init__(self, tamanho_inicial=10):
        self.tamanho = tamanho_inicial
        self.tabela = [None] * self.tamanho
        self.elementos = 0
    
    def _hash1(self, chave):
        """Primeira função hash"""
        return hash(chave) % self.tamanho
    
    def _hash2(self, chave):
        """Segunda função hash - deve ser diferente e não zero"""
        return 1 + (hash(chave) % (self.tamanho - 1))
    
    def _proxima_posicao(self, chave, tentativa):
        """Calcula próxima posição usando double hashing"""
        h1 = self._hash1(chave)
        h2 = self._hash2(chave)
        return (h1 + tentativa * h2) % self.tamanho
    
    def inserir(self, chave, valor):
        tentativa = 0
        while tentativa < self.tamanho:
            posicao = self._proxima_posicao(chave, tentativa)
            
            if self.tabela[posicao] is None:
                self.tabela[posicao] = (chave, valor)
                self.elementos += 1
                return
            elif self.tabela[posicao][0] == chave:
                self.tabela[posicao] = (chave, valor)  # Atualiza
                return
            
            tentativa += 1
        
        raise Exception("Tabela cheia")
```

---

## 5. Implementação em Python {#implementacao}

### 5.1 Implementação Completa com Todos os Recursos

```python
class HashTableCompleta:
    """
    Implementação completa de Hash Table com todos os recursos
    """
    
    def __init__(self, capacidade_inicial=16, fator_carga=0.75):
        self.capacidade = capacidade_inicial
        self.fator_carga = fator_carga
        self.tabela = [[] for _ in range(self.capacidade)]
        self.tamanho = 0
        self.colisoes = 0
        self.redimensionamentos = 0
    
    def _hash(self, chave):
        """Função hash que distribui uniformemente"""
        # Usa o hash nativo do Python e aplica uma mistura adicional
        h = hash(chave)
        # Mistura os bits para melhor distribuição
        h = h ^ (h >> 16)
        return h % self.capacidade
    
    def __setitem__(self, chave, valor):
        """Insere ou atualiza um item (suporta sintaxe dicionário)"""
        self.inserir(chave, valor)
    
    def __getitem__(self, chave):
        """Recupera um item (suporta sintaxe dicionário)"""
        return self.buscar(chave)
    
    def __delitem__(self, chave):
        """Remove um item (suporta sintaxe dicionário)"""
        self.remover(chave)
    
    def __contains__(self, chave):
        """Verifica se chave existe (suporta 'in')"""
        try:
            self.buscar(chave)
            return True
        except KeyError:
            return False
    
    def __len__(self):
        """Retorna número de elementos"""
        return self.tamanho
    
    def __iter__(self):
        """Iterador sobre as chaves"""
        for bucket in self.tabela:
            for chave, _ in bucket:
                yield chave
    
    def inserir(self, chave, valor):
        """Insere um par chave-valor"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        # Verifica se chave já existe
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                bucket[i] = (chave, valor)
                return
        
        # Se não existe, adiciona
        if bucket:  # Se o bucket não está vazio, é colisão
            self.colisoes += 1
        
        bucket.append((chave, valor))
        self.tamanho += 1
        
        # Verifica redimensionamento
        if self.tamanho > self.capacidade * self.fator_carga:
            self._redimensionar()
    
    def buscar(self, chave):
        """Busca um valor pela chave"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for k, v in bucket:
            if k == chave:
                return v
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def remover(self, chave):
        """Remove um par chave-valor"""
        indice = self._hash(chave)
        bucket = self.tabela[indice]
        
        for i, (k, v) in enumerate(bucket):
            if k == chave:
                del bucket[i]
                self.tamanho -= 1
                return
        
        raise KeyError(f"Chave '{chave}' não encontrada")
    
    def _redimensionar(self):
        """Redimensiona a tabela quando necessário"""
        nova_capacidade = self.capacidade * 2
        nova_tabela = [[] for _ in range(nova_capacidade)]
        
        # Rehash todos os elementos
        for bucket in self.tabela:
            for chave, valor in bucket:
                novo_indice = hash(chave) % nova_capacidade
                nova_tabela[novo_indice].append((chave, valor))
        
        self.capacidade = nova_capacidade
        self.tabela = nova_tabela
        self.redimensionamentos += 1
        self.colisoes = 0  # Reset contador de colisões
    
    def obter_estatisticas(self):
        """Retorna estatísticas da hash table"""
        buckets_ocupados = sum(1 for bucket in self.tabela if bucket)
        tamanho_medio_bucket = self.tamanho / buckets_ocupados if buckets_ocupados else 0
        buckets_vazios = self.capacidade - buckets_ocupados
        
        return {
            "capacidade": self.capacidade,
            "elementos": self.tamanho,
            "fator_carga": self.tamanho / self.capacidade,
            "buckets_ocupados": buckets_ocupados,
            "buckets_vazios": buckets_vazios,
            "tamanho_medio_bucket": tamanho_medio_bucket,
            "colisoes_totais": self.colisoes,
            "redimensionamentos": self.redimensionamentos
        }
    
    def keys(self):
        """Retorna todas as chaves"""
        return list(self)
    
    def values(self):
        """Retorna todos os valores"""
        valores = []
        for bucket in self.tabela:
            for _, valor in bucket:
                valores.append(valor)
        return valores
    
    def items(self):
        """Retorna todos os pares chave-valor"""
        itens = []
        for bucket in self.tabela:
            for item in bucket:
                itens.append(item)
        return itens
    
    def clear(self):
        """Limpa a tabela"""
        self.tabela = [[] for _ in range(self.capacidade)]
        self.tamanho = 0
        self.colisoes = 0
    
    def get(self, chave, default=None):
        """Retorna valor ou default se não existir"""
        try:
            return self.buscar(chave)
        except KeyError:
            return default
    
    def update(self, outro_dicionario):
        """Atualiza com pares de outro dicionário"""
        for chave, valor in outro_dicionario.items():
            self.inserir(chave, valor)
```

### 5.2 Testes da Implementação

```python
def testar_hash_table():
    """Testa a implementação da Hash Table"""
    
    # Criação
    ht = HashTableCompleta()
    print("=== Teste Básico ===")
    
    # Inserção
    ht["nome"] = "João"
    ht["idade"] = 30
    ht["cidade"] = "São Paulo"
    
    print(f"nome: {ht['nome']}")
    print(f"idade: {ht['idade']}")
    print(f"cidade: {ht['cidade']}")
    print(f"Tamanho: {len(ht)}")
    
    # Atualização
    ht["idade"] = 31
    print(f"idade atualizada: {ht['idade']}")
    
    # Verificação de existência
    print(f"'nome' in ht: {'nome' in ht}")
    print(f"'pais' in ht: {'pais' in ht}")
    
    # Remoção
    del ht["cidade"]
    print(f"Após remover 'cidade': {len(ht)} elementos")
    
    # Métodos get
    print(f"get('nome'): {ht.get('nome')}")
    print(f"get('pais', 'Brasil'): {ht.get('pais', 'Brasil')}")
    
    # Iteração
    print("\n=== Iteração sobre chaves ===")
    for chave in ht:
        print(f"{chave}: {ht[chave]}")
    
    # Estatísticas
    print("\n=== Estatísticas ===")
    for chave, valor in ht.obter_estatisticas().items():
        print(f"{chave}: {valor}")

# Executar testes
testar_hash_table()
```

---

## 6. Hash Table do Python (dict) {#dict-python}

### 6.1 Funcionamento Interno do dict

```python
class DictPython:
    """
    Explicação do funcionamento do dicionário Python
    Baseado na implementação CPython
    """
    
    @staticmethod
    def caracteristicas():
        """Características do dict do Python"""
        return {
            "implementacao": "Tabela hash com sondagem pseudo-aleatória",
            "ordem": "Preserva ordem de inserção (Python 3.7+)",
            "chaves": "Qualquer objeto hashable",
            "fator_carga": "2/3 (aproximadamente 0.66)",
            "redimensionamento": "Quando 2/3 cheio, quadruplica tamanho"
        }
    
    @staticmethod
    def exemplos_uso():
        """Exemplos de uso do dict nativo"""
        
        # Criação
        d1 = {}  # dict vazio
        d2 = dict()  # outro dict vazio
        d3 = {"nome": "Maria", "idade": 25}  # com valores
        d4 = dict(nome="João", idade=30)  # com kwargs
        
        # Operações básicas
        dados = {"a": 1, "b": 2, "c": 3}
        
        # Acesso
        print(dados["a"])  # 1
        print(dados.get("d", 0))  # 0 (default)
        
        # Iteração
        for chave in dados:
            print(f"{chave}: {dados[chave]}")
        
        for chave, valor in dados.items():
            print(f"{chave}: {valor}")
        
        # Métodos úteis
        chaves = dados.keys()
        valores = dados.values()
        itens = dados.items()
        
        # Cópia
        copia = dados.copy()
        
        # Remoção segura
        valor = dados.pop("b", None)  # Remove e retorna valor
        ultimo = dados.popitem()  # Remove e retorna último item
        
        # Atualização
        dados.update({"d": 4, "e": 5})
        
        return d1, d2, d3, d4
```

### 6.2 Comparação: dict vs Implementação Própria

```python
import time
import random

class ComparacaoDesempenho:
    """
    Compara desempenho entre dict nativo e implementação própria
    """
    
    def __init__(self, num_elementos=10000):
        self.num_elementos = num_elementos
        self.chaves = [f"chave_{i}" for i in range(num_elementos)]
        self.valores = [random.randint(1, 1000) for _ in range(num_elementos)]
    
    def testar_dict_nativo(self):
        """Testa desempenho do dict nativo"""
        inicio = time.time()
        
        d = {}
        # Inserção
        for k, v in zip(self.chaves, self.valores):
            d[k] = v
        
        tempo_insercao = time.time() - inicio
        
        # Busca
        inicio = time.time()
        for k in self.chaves[:1000]:  # Busca 1000 chaves
            _ = d.get(k)
        tempo_busca = time.time() - inicio
        
        return tempo_insercao, tempo_busca
    
    def testar_hash_table_propria(self):
        """Testa desempenho da hash table própria"""
        inicio = time.time()
        
        ht = HashTableCompleta()
        # Inserção
        for k, v in zip(self.chaves, self.valores):
            ht[k] = v
        
        tempo_insercao = time.time() - inicio
        
        # Busca
        inicio = time.time()
        for k in self.chaves[:1000]:  # Busca 1000 chaves
            _ = ht.get(k)
        tempo_busca = time.time() - inicio
        
        return tempo_insercao, tempo_busca
    
    def comparar(self):
        """Executa comparação"""
        print("=== Comparação de Desempenho ===")
        print(f"Elementos: {self.num_elementos}\n")
        
        # Dict nativo
        t_ins_dict, t_busca_dict = self.testar_dict_nativo()
        print(f"Dict Nativo:")
        print(f"  Inserção: {t_ins_dict:.4f}s")
        print(f"  Busca: {t_busca_dict:.4f}s")
        
        # Hash Table própria
        t_ins_ht, t_busca_ht = self.testar_hash_table_propria()
        print(f"\nHash Table Própria:")
        print(f"  Inserção: {t_ins_ht:.4f}s")
        print(f"  Busca: {t_busca_ht:.4f}s")
        
        # Fatores
        print(f"\nFatores (dict é mais rápido):")
        print(f"  Inserção: {t_ins_ht/t_ins_dict:.2f}x")
        print(f"  Busca: {t_busca_ht/t_busca_dict:.2f}x")

# Executar comparação
# comp = ComparacaoDesempenho(10000)
# comp.comparar()
```

---

## 7. Análise de Complexidade {#complexidade}

### 7.1 Tabela de Complexidades

```python
class AnaliseComplexidade:
    """
    Análise de complexidade das operações em Hash Table
    """
    
    @staticmethod
    def tabela_complexidade():
        """Retorna tabela de complexidade"""
        return {
            "Operação": ["Inserção", "Busca", "Remoção", "Redimensionamento"],
            "Médio": ["O(1)", "O(1)", "O(1)", "O(n)"],
            "Pior Caso": ["O(n)", "O(n)", "O(n)", "O(n)"],
            "Melhor Caso": ["O(1)", "O(1)", "O(1)", "O(n)"]
        }
    
    @staticmethod
    def fatores_influenciadores():
        """Fatores que afetam a complexidade"""
        return {
            "função_hash": "Distribuição uniforme evita colisões",
            "fator_carga": "Menor fator = menos colisões, mais memória",
            "tratamento_colisoes": "Encadeamento vs Sondagem",
            "qualidade_chaves": "Chaves similares podem causar colisões",
            "redimensionamento": "Operação cara mas esporádica"
        }
    
    @staticmethod
    def calcular_fator_carga(elementos, capacidade):
        """Calcula o fator de carga atual"""
        return elementos / capacidade if capacidade > 0 else 0
    
    @staticmethod
    def estimar_colisoes(elementos, capacidade):
        """
        Estima número de colisões baseado no aniversário paradox
        """
        # Probabilidade de colisão ≈ 1 - e^(-n²/(2m))
        import math
        n = elementos
        m = capacidade
        prob_colisao = 1 - math.exp(-(n**2) / (2 * m))
        return prob_colisao
```

### 7.2 Comparativo com Outras Estruturas

```python
class ComparativoEstruturas:
    """
    Comparação entre diferentes estruturas de dados
    """
    
    @staticmethod
    def comparar_com_lista():
        """Compara Hash Table com Lista"""
        dados = list(range(1000))
        
        # Busca em lista
        def busca_lista(lista, valor):
            return valor in lista  # O(n)
        
        # Busca em hash table
        def busca_hash_table(ht, valor):
            return valor in ht  # O(1) médio
        
        return {
            "lista": "O(n) - linear",
            "hash_table": "O(1) - constante (médio)",
            "vantagem_hash": "Muito mais rápido para buscas"
        }
    
    @staticmethod
    def comparar_com_arvore():
        """Compara Hash Table com Árvore Binária"""
        return {
            "hash_table": {
                "busca": "O(1) médio",
                "ordem": "Não ordenada",
                "range_queries": "Ineficiente"
            },
            "arvore_binaria": {
                "busca": "O(log n)",
                "ordem": "Ordenada",
                "range_queries": "Eficiente"
            }
        }
```

---

## 8. Exemplos Práticos {#exemplos}

### 8.1 Sistema de Cache

```python
import time
from functools import wraps

class CacheLRU:
    """
    Implementação de cache LRU (Least Recently Used)
    usando dicionário e lista duplamente ligada
    """
    
    def __init__(self, capacidade=100):
        self.capacidade = capacidade
        self.cache = {}  # Hash table para acesso O(1)
        self.ordem_uso = []  # Lista para controle de LRU
    
    def get(self, chave):
        """Retorna valor do cache"""
        if chave in self.cache:
            # Move para o final (mais recente)
            self.ordem_uso.remove(chave)
            self.ordem_uso.append(chave)
            return self.cache[chave]
        return None
    
    def put(self, chave, valor):
        """Insere valor no cache"""
        if chave in self.cache:
            # Atualiza existente
            self.ordem_uso.remove(chave)
        elif len(self.cache) >= self.capacidade:
            # Remove o menos recente
            lru = self.ordem_uso.pop(0)
            del self.cache[lru]
        
        # Insere novo
        self.cache[chave] = valor
        self.ordem_uso.append(chave)
    
    def __str__(self):
        return f"Cache({self.cache})"

# Decorator para cache de funções
def memoize(tempo_expiracao=300):
    """
    Decorator que cacheia resultados de funções
    com tempo de expiração
    """
    def decorator(func):
        cache = {}
        timestamps = {}
        
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Cria chave baseada nos argumentos
            chave = str(args) + str(kwargs)
            
            # Verifica se cache é válido
            if chave in cache:
                if time.time() - timestamps[chave] < tempo_expiracao:
                    return cache[chave]
            
            # Calcula e armazena resultado
            resultado = func(*args, **kwargs)
            cache[chave] = resultado
            timestamps[chave] = time.time()
            
            return resultado
        
        return wrapper
    return decorator

# Exemplo de uso
@memoize(tempo_expiracao=60)
def funcao_custosa(n):
    """Função que simula processamento pesado"""
    time.sleep(2)  # Simula processamento
    return n * n

# Teste
print("Primeira chamada (vai demorar)...")
print(funcao_custosa(10))
print("Segunda chamada (rápida - cache)...")
print(funcao_custosa(10))
```

### 8.2 Contador de Frequência

```python
from collections import defaultdict
from typing import List, Dict, Any

class AnalisadorFrequencia:
    """
    Análise de frequência usando hash tables
    """
    
    @staticmethod
    def contar_palavras(texto: str) -> Dict[str, int]:
        """Conta frequência de palavras em um texto"""
        palavras = texto.lower().split()
        frequencia = {}
        
        for palavra in palavras:
            # Limpa pontuação
            palavra = palavra.strip('.,!?()[]{}":;')
            if palavra:
                frequencia[palavra] = frequencia.get(palavra, 0) + 1
        
        return frequencia
    
    @staticmethod
    def top_n_frequentes(frequencia: Dict[str, int], n: int = 10) -> List[tuple]:
        """Retorna as N palavras mais frequentes"""
        return sorted(frequencia.items(), key=lambda x: x[1], reverse=True)[:n]
    
    @staticmethod
    def histograma(frequencia: Dict[str, int], max_itens: int = 20):
        """Gera histograma de frequência"""
        top_itens = AnalisadorFrequencia.top_n_frequentes(frequencia, max_itens)
        
        for palavra, count in top_itens:
            barras = '#' * min(count, 50)  # Limita tamanho visual
            print(f"{palavra:15} | {count:3d} {barras}")
    
    @staticmethod
    def palavras_unicas(frequencia: Dict[str, int]) -> List[str]:
        """Retorna palavras que aparecem apenas uma vez"""
        return [palavra for palavra, count in frequencia.items() if count == 1]

# Exemplo de uso
texto_exemplo = """
Python é uma linguagem de programação poderosa e fácil de aprender.
Python tem uma sintaxe elegante e é ótima para iniciantes.
Além disso, Python é muito utilizado em ciência de dados e machine learning.
"""

freq = AnalisadorFrequencia.contar_palavras(texto_exemplo)
print("Frequência de palavras:")
AnalisadorFrequencia.histograma(freq)
print(f"\nPalavras únicas: {AnalisadorFrequencia.palavras_unicas(freq)}")
```

### 8.3 Sistema de Indexação

```python
class IndiceInvertido:
    """
    Implementação de índice invertido para busca de documentos
    """
    
    def __init__(self):
        self.indice = defaultdict(set)  # palavra -> conjunto de documentos
        self.documentos = {}  # id_doc -> conteúdo
        self.proximo_id = 0
    
    def adicionar_documento(self, conteudo: str) -> int:
        """Adiciona documento ao índice"""
        doc_id = self.proximo_id
        self.documentos[doc_id] = conteudo
        self.proximo_id += 1
        
        # Indexa palavras
        palavras = conteudo.lower().split()
        for i, palavra in enumerate(palavras):
            # Limpa pontuação
            palavra = palavra.strip('.,!?()[]{}":;')
            if palavra:
                self.indice[palavra].add((doc_id, i))  # Guarda posição também
        
        return doc_id
    
    def buscar(self, termo: str) -> List[tuple]:
        """Busca documentos que contêm o termo"""
        termo = termo.lower().strip()
        return list(self.indice.get(termo, []))
    
    def buscar_frase(self, frase: str) -> List[int]:
        """Busca documentos que contêm a frase exata"""
        palavras = frase.lower().split()
        if not palavras:
            return []
        
        # Começa com documentos da primeira palavra
        resultados = set(doc_id for doc_id, _ in self.indice.get(palavras[0], []))
        
        # Para cada palavra seguinte, verifica posições consecutivas
        for i, palavra in enumerate(palavras[1:], 1):
            ocorrencias = set()
            for doc_id, pos in self.indice.get(palavra, []):
                # Verifica se a palavra anterior está na posição anterior
                if any((doc_id, pos - i) in self.indice.get(palavras[0], [])):
                    ocorrencias.add(doc_id)
            resultados &= ocorrencias
        
        return list(resultados)
    
    def relevancia(self, doc_id: int, termo: str) -> int:
        """Calcula relevância de um documento para um termo (frequência)"""
        termo = termo.lower().strip()
        return sum(1 for t, _ in self.indice.get(termo, []) if t == doc_id)

# Exemplo de uso
indice = IndiceInvertido()

# Adiciona documentos
indice.adicionar_documento("O gato subiu no telhado")
indice.adicionar_documento("O cachorro correu atrás do gato")
indice.adicionar_documento("O pássaro voou sobre o telhado")

# Buscas
print("Busca por 'gato':", indice.buscar("gato"))
print("Busca por 'telhado':", indice.buscar("telhado"))
print("Busca por frase 'gato no telhado':", indice.buscar_frase("gato no telhado"))
```

### 8.4 Sistema de Usuários com Login

```python
import hashlib
import secrets
from datetime import datetime, timedelta

class SistemaUsuarios:
    """
    Sistema de gerenciamento de usuários usando hash tables
    """
    
    def __init__(self):
        self.usuarios = {}  # username -> dados do usuário
        self.sessoes = {}   # token -> username
        self.tentativas = defaultdict(int)  # username -> tentativas falhas
        self.bloqueios = {}  # username -> tempo de bloqueio
    
    def _hash_senha(self, senha: str, salt: str = None) -> tuple:
        """Cria hash da senha com salt"""
        if salt is None:
            salt = secrets.token_hex(16)
        
        # Combina senha com salt e faz hash
        senha_salt = senha + salt
        hash_senha = hashlib.sha256(senha_salt.encode()).hexdigest()
        
        return hash_senha, salt
    
    def criar_usuario(self, username: str, senha: str, email: str) -> bool:
        """Cria novo usuário"""
        if username in self.usuarios:
            return False
        
        hash_senha, salt = self._hash_senha(senha)
        
        self.usuarios[username] = {
            'username': username,
            'email': email,
            'hash_senha': hash_senha,
            'salt': salt,
            'criado_em': datetime.now(),
            'ultimo_login': None,
            'admin': False
        }
        
        return True
    
    def autenticar(self, username: str, senha: str) -> bool:
        """Autentica usuário"""
        # Verifica bloqueio
        if username in self.bloqueios:
            if datetime.now() < self.bloqueios[username]:
                return False
            else:
                del self.bloqueios[username]
        
        # Verifica se usuário existe
        if username not in self.usuarios:
            return False
        
        usuario = self.usuarios[username]
        hash_tentativa, _ = self._hash_senha(senha, usuario['salt'])
        
        if hash_tentativa == usuario['hash_senha']:
            # Login bem-sucedido
            usuario['ultimo_login'] = datetime.now()
            self.tentativas[username] = 0
            return True
        else:
            # Falha no login
            self.tentativas[username] += 1
            if self.tentativas[username] >= 5:
                self.bloqueios[username] = datetime.now() + timedelta(minutes=15)
            return False
    
    def criar_sessao(self, username: str) -> str:
        """Cria token de sessão para usuário autenticado"""
        token = secrets.token_hex(32)
        self.sessoes[token] = {
            'username': username,
            'criado_em': datetime.now(),
            'expira_em': datetime.now() + timedelta(hours=24)
        }
        return token
    
    def validar_sessao(self, token: str) -> str | None:
        """Valida token de sessão e retorna username"""
        if token not in self.sessoes:
            return None
        
        sessao = self.sessoes[token]
        if datetime.now() > sessao['expira_em']:
            del self.sessoes[token]
            return None
        
        return sessao['username']
    
    def buscar_por_email(self, email: str) -> list:
        """Busca usuários por email (parcial)"""
        resultados = []
        for username, dados in self.usuarios.items():
            if email.lower() in dados['email'].lower():
                resultados.append({
                    'username': username,
                    'email': dados['email']
                })
        return resultados
    
    def estatisticas(self) -> dict:
        """Retorna estatísticas do sistema"""
        return {
            'total_usuarios': len(self.usuarios),
            'usuarios_online': len(self.sessoes),
            'tentativas_bloqueadas': len(self.bloqueios),
            'usuarios_com_login_recente': sum(
                1 for u in self.usuarios.values()
                if u['ultimo_login'] and 
                datetime.now() - u['ultimo_login'] < timedelta(days=7)
            )
        }

# Exemplo de uso
sistema = SistemaUsuarios()

# Criar usuários
sistema.criar_usuario('joao', 'senha123', 'joao@email.com')
sistema.criar_usuario('maria', '123456', 'maria@email.com')

# Autenticar
if sistema.autenticar('joao', 'senha123'):
    token = sistema.criar_sessao('joao')
    print(f"Login bem-sucedido! Token: {token}")

# Validar sessão
username = sistema.validar_sessao(token)
print(f"Usuário da sessão: {username}")

# Buscar por email
print("Busca por 'email.com':", sistema.buscar_por_email('email.com'))

# Estatísticas
print("Estatísticas:", sistema.estatisticas())
```

---

## 9. Casos de Uso Avançados {#casos-avancados}

### 9.1 Tabela Hash Persistente

```python
import json
import os
from pathlib import Path

class HashTablePersistente:
    """
    Hash Table com persistência em disco
    """
    
    def __init__(self, arquivo='hash_table.json'):
        self.arquivo = arquivo
        self.dados = {}
        self.modificado = False
        self._carregar()
    
    def _carregar(self):
        """Carrega dados do arquivo"""
        if os.path.exists(self.arquivo):
            try:
                with open(self.arquivo, 'r') as f:
                    self.dados = json.load(f)
            except:
                self.dados = {}
    
    def _salvar(self):
        """Salva dados no arquivo"""
        if self.modificado:
            with open(self.arquivo, 'w') as f:
                json.dump(self.dados, f, indent=2)
            self.modificado = False
    
    def __setitem__(self, chave, valor):
        self.dados[str(chave)] = valor
        self.modificado = True
        self._salvar()
    
    def __getitem__(self, chave):
        return self.dados.get(str(chave))
    
    def __delitem__(self, chave):
        if str(chave) in self.dados:
            del self.dados[str(chave)]
            self.modificado = True
            self._salvar()
    
    def __contains__(self, chave):
        return str(chave) in self.dados
    
    def keys(self):
        return self.dados.keys()
    
    def values(self):
        return self.dados.values()
    
    def items(self):
        return self.dados.items()
    
    def close(self):
        """Fecha e garante salvamento"""
        self._salvar()
```

### 9.2 Tabela Hash com Expiração

```python
import time
from threading import Lock

class HashTableComExpiracao:
    """
    Hash Table onde itens expiram após um tempo
    """
    
    def __init__(self, tempo_expiracao_padrao=300):
        self.dados = {}
        self.expiracao = {}  # chave -> timestamp expiração
        self.tempo_padrao = tempo_expiracao_padrao
        self.lock = Lock()
    
    def _limpar_expirados(self):
        """Remove itens expirados"""
        agora = time.time()
        expirados = [
            chave for chave, expira in self.expiracao.items()
            if expira < agora
        ]
        for chave in expirados:
            del self.dados[chave]
            del self.expiracao[chave]
    
    def __setitem__(self, chave, valor, tempo_expiracao=None):
        with self.lock:
            if tempo_expiracao is None:
                tempo_expiracao = self.tempo_padrao
            
            self.dados[chave] = valor
            self.expiracao[chave] = time.time() + tempo_expiracao
    
    def __getitem__(self, chave):
        with self.lock:
            self._limpar_expirados()
            return self.dados.get(chave)
    
    def __contains__(self, chave):
        with self.lock:
            self._limpar_expirados()
            return chave in self.dados
    
    def limpar_expirados_agora(self):
        """Força limpeza de itens expirados"""
        with self.lock:
            self._limpar_expirados()
    
    def estatisticas(self):
        with self.lock:
            self._limpar_expirados()
            agora = time.time()
            ativos = len(self.dados)
            expiracao_media = 0
            if ativos > 0:
                soma = sum(exp - agora for exp in self.expiracao.values())
                expiracao_media = soma / ativos
            
            return {
                'itens_ativos': ativos,
                'tempo_medio_vida_restante': expiracao_media
            }
```

### 9.3 Tabela Hash Concorrente

```python
from threading import Lock, RLock
import concurrent.futures

class HashTableConcorrente:
    """
    Hash Table thread-safe para uso concorrente
    """
    
    def __init__(self, num_buckets=16):
        self.num_buckets = num_buckets
        self.buckets = [{} for _ in range(num_buckets)]
        self.locks = [RLock() for _ in range(num_buckets)]
    
    def _get_bucket(self, chave):
        """Determina bucket para a chave"""
        return hash(chave) % self.num_buckets
    
    def __setitem__(self, chave, valor):
        bucket_idx = self._get_bucket(chave)
        with self.locks[bucket_idx]:
            self.buckets[bucket_idx][chave] = valor
    
    def __getitem__(self, chave):
        bucket_idx = self._get_bucket(chave)
        with self.locks[bucket_idx]:
            return self.buckets[bucket_idx].get(chave)
    
    def __delitem__(self, chave):
        bucket_idx = self._get_bucket(chave)
        with self.locks[bucket_idx]:
            if chave in self.buckets[bucket_idx]:
                del self.buckets[bucket_idx][chave]
    
    def atualizar_se_existir(self, chave, funcao_atualizacao):
        """
        Atualiza valor se chave existir (operação atômica)
        """
        bucket_idx = self._get_bucket(chave)
        with self.locks[bucket_idx]:
            if chave in self.buckets[bucket_idx]:
                valor_atual = self.buckets[bucket_idx][chave]
                self.buckets[bucket_idx][chave] = funcao_atualizacao(valor_atual)
                return True
        return False
    
    def get_or_create(self, chave, valor_padrao):
        """
        Retorna valor existente ou cria novo (thread-safe)
        """
        bucket_idx = self._get_bucket(chave)
        with self.locks[bucket_idx]:
            if chave in self.buckets[bucket_idx]:
                return self.buckets[bucket_idx][chave]
            else:
                self.buckets[bucket_idx][chave] = valor_padrao
                return valor_padrao
    
    def processar_em_lote(self, items):
        """
        Processa múltiplos items em paralelo
        """
        with concurrent.futures.ThreadPoolExecutor() as executor:
            futures = []
            for chave, valor in items:
                future = executor.submit(self.__setitem__, chave, valor)
                futures.append(future)
            
            for future in concurrent.futures.as_completed(futures):
                future.result()  # Espera conclusão

# Teste concorrente
def teste_concorrente():
    ht = HashTableConcorrente()
    
    def escritor(inicio, fim):
        for i in range(inicio, fim):
            ht[f"chave_{i}"] = i
    
    def leitor(chaves):
        resultados = {}
        for chave in chaves:
            resultados[chave] = ht[chave]
        return resultados
    
    # Teste com múltiplas threads
    with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
        # Escritores
        futures = []
        for i in range(0, 1000, 250):
            futures.append(executor.submit(escritor, i, i + 250))
        
        # Leitores (paralelo)
        chaves_teste = [f"chave_{i}" for i in range(0, 1000, 100)]
        future_leitor = executor.submit(leitor, chaves_teste)
        
        # Aguarda todos
        for f in futures:
            f.result()
        
        resultados = future_leitor.result()
        print("Resultados leitura:", resultados)
```

---

## 10. Otimizações e Boas Práticas {#otimizacoes}

### 10.1 Escolha da Capacidade Inicial

```python
class OtimizacoesHashTable:
    """
    Dicas e técnicas de otimização para hash tables
    """
    
    @staticmethod
    def capacidade_ideal(num_elementos_esperados, fator_carga=0.75):
        """
        Calcula capacidade inicial ideal
        capacidade = num_elementos / fator_carga
        """
        return int(num_elementos_esperados / fator_carga) + 1
    
    @staticmethod
    def numero_primo_proximo(n):
        """Encontra número primo próximo (reduz colisões)"""
        def eh_primo(num):
            if num < 2:
                return False
            for i in range(2, int(num ** 0.5) + 1):
                if num % i == 0:
                    return False
            return True
        
        while not eh_primo(n):
            n += 1
        return n
    
    @staticmethod
    def dicas_otimizacao():
        """Dicas para otimizar uso de hash tables"""
        return {
            "capacidade_inicial": "Especifique capacidade se souber tamanho aproximado",
            "fator_carga": "Ajuste conforme necessidade (memória vs velocidade)",
            "funcao_hash": "Use funções hash rápidas e uniformes",
            "chaves": "Prefira tipos imutáveis e simples (int, str)",
            "colisoes": "Monitore taxa de colisões para ajustar",
            "redimensionamento": "Redimensionamentos são caros, evite-os"
        }
```

### 10.2 Pool de Objetos com Hash Table

```python
class ObjectPool:
    """
    Pool de objetos reutilizáveis usando hash table
    """
    
    def __init__(self, classe_objeto, tamanho_maximo=100):
        self.classe = classe_objeto
        self.tamanho_maximo = tamanho_maximo
        self.disponiveis = {}  # id -> objeto
        self.em_uso = set()
    
    def adquirir(self, *args, **kwargs):
        """Adquire um objeto do pool"""
        if self.disponiveis:
            # Reutiliza objeto existente
            obj_id, obj = self.disponiveis.popitem()
            self.em_uso.add(obj_id)
            return obj
        elif len(self.em_uso) < self.tamanho_maximo:
            # Cria novo objeto
            obj = self.classe(*args, **kwargs)
            obj_id = id(obj)
            self.em_uso.add(obj_id)
            return obj
        else:
            # Pool cheio
            raise Exception("Pool cheio")
    
    def liberar(self, obj):
        """Retorna objeto ao pool"""
        obj_id = id(obj)
        if obj_id in self.em_uso:
            self.em_uso.remove(obj_id)
            self.disponiveis[obj_id] = obj
    
    def limpar(self):
        """Limpa o pool"""
        self.disponiveis.clear()
        self.em_uso.clear()
    
    def estatisticas(self):
        return {
            'em_uso': len(self.em_uso),
            'disponiveis': len(self.disponiveis),
            'total': len(self.em_uso) + len(self.disponiveis),
            'tamanho_maximo': self.tamanho_maximo
        }
```

### 10.3 Compressão de Dados com Hash Table

```python
class CompressorDicionario:
    """
    Compressão simples usando dicionário
    (similar ao LZW)
    """
    
    def __init__(self):
        self.dicionario = {}
        self.proximo_codigo = 256  # Códigos para bytes 0-255 são reservados
    
    def comprimir(self, dados):
        """
        Comprime dados usando dicionário
        """
        if isinstance(dados, str):
            dados = dados.encode()
        
        resultado = []
        buffer = b''
        
        for byte in dados:
            novo_buffer = buffer + bytes([byte])
            if novo_buffer in self.dicionario:
                buffer = novo_buffer
            else:
                # Adiciona ao dicionário
                if buffer:
                    codigo = self.dicionario.get(buffer, buffer[0] if len(buffer) == 1 else None)
                    resultado.append(codigo)
                
                self.dicionario[novo_buffer] = self.proximo_codigo
                self.proximo_codigo += 1
                buffer = bytes([byte])
        
        # Último buffer
        if buffer:
            codigo = self.dicionario.get(buffer, buffer[0] if len(buffer) == 1 else None)
            resultado.append(codigo)
        
        return resultado
    
    def estatisticas_compressao(self, dados_originais, dados_comprimidos):
        """Calcula estatísticas de compressão"""
        tamanho_original = len(dados_originais)
        tamanho_comprimido = len(dados_comprimidos) * 2  # Assumindo 2 bytes por código
        
        return {
            'tamanho_original': tamanho_original,
            'tamanho_comprimido': tamanho_comprimido,
            'taxa_compressao': tamanho_comprimido / tamanho_original,
            'economia': (1 - tamanho_comprimido / tamanho_original) * 100,
            'tamanho_dicionario': len(self.dicionario)
        }
```

---

## 11. Exercícios Resolvidos {#exercicios}

### Exercício 1: Verificar Duplicatas

```python
def tem_duplicatas(lista):
    """
    Verifica se uma lista tem elementos duplicados
    Usa hash table para O(n)
    """
    vistos = set()
    for elemento in lista:
        if elemento in vistos:
            return True
        vistos.add(elemento)
    return False

# Teste
print(tem_duplicatas([1, 2, 3, 4, 5]))  # False
print(tem_duplicatas([1, 2, 3, 2, 5]))  # True
```

### Exercício 2: Soma de Dois Números

```python
def encontrar_soma(nums, alvo):
    """
    Encontra índices de dois números que somam ao alvo
    Usa hash table para O(n)
    """
    complementos = {}  # complemento -> índice
    
    for i, num in enumerate(nums):
        complemento = alvo - num
        if complemento in complementos:
            return [complementos[complemento], i]
        complementos[num] = i
    
    return None

# Teste
nums = [2, 7, 11, 15]
alvo = 9
print(encontrar_soma(nums, alvo))  # [0, 1]
```

### Exercício 3: Primeiro Caractere Não Repetido

```python
def primeiro_nao_repetido(s):
    """
    Encontra o primeiro caractere que não se repete na string
    """
    frequencia = {}
    
    # Primeira passada: conta frequências
    for char in s:
        frequencia[char] = frequencia.get(char, 0) + 1
    
    # Segunda passada: encontra primeiro com frequência 1
    for char in s:
        if frequencia[char] == 1:
            return char
    
    return None

# Teste
print(primeiro_nao_repetido("abracadabra"))  # 'c'
print(primeiro_nao_repetido("aabbcc"))       # None
```

### Exercício 4: Agrupar Anagramas

```python
from collections import defaultdict

def agrupar_anagramas(palavras):
    """
    Agrupa palavras que são anagramas entre si
    """
    grupos = defaultdict(list)
    
    for palavra in palavras:
        # Chave é a palavra ordenada
        chave = ''.join(sorted(palavra))
        grupos[chave].append(palavra)
    
    return list(grupos.values())

# Teste
palavras = ["ate", "eat", "tea", "bat", "tab", "eta"]
print(agrupar_anagramas(palavras))
# [['ate', 'eat', 'tea', 'eta'], ['bat', 'tab']]
```

### Exercício 5: Subarray com Soma Zero

```python
def subarray_soma_zero(nums):
    """
    Encontra subarray contíguo com soma zero
    """
    soma_acumulada = {}
    soma = 0
    
    for i, num in enumerate(nums):
        soma += num
        
        # Se soma é zero, subarray do início até i
        if soma == 0:
            return nums[:i+1]
        
        # Se soma já foi vista, subarray entre as ocorrências
        if soma in soma_acumulada:
            inicio = soma_acumulada[soma] + 1
            return nums[inicio:i+1]
        
        soma_acumulada[soma] = i
    
    return None

# Teste
print(subarray_soma_zero([1, 2, -3, 3, 1]))  # [1, 2, -3]
```

### Exercício 6: LRU Cache Implementation

```python
class LRUCache:
    """
    Implementação de LRU Cache usando OrderedDict
    """
    
    def __init__(self, capacidade):
        from collections import OrderedDict
        self.capacidade = capacidade
        self.cache = OrderedDict()
    
    def get(self, chave):
        if chave not in self.cache:
            return -1
        
        # Move para o final (mais recente)
        self.cache.move_to_end(chave)
        return self.cache[chave]
    
    def put(self, chave, valor):
        if chave in self.cache:
            # Atualiza e move para o final
            self.cache[chave] = valor
            self.cache.move_to_end(chave)
        else:
            if len(self.cache) >= self.capacidade:
                # Remove o menos recente (primeiro)
                self.cache.popitem(last=False)
            self.cache[chave] = valor

# Teste
cache = LRUCache(2)
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))    # 1
cache.put(3, 3)        # Remove chave 2
print(cache.get(2))    # -1
```

### Exercício 7: Contador de Palavras com Top K

```python
from collections import Counter
import heapq

def top_k_frequentes(palavras, k):
    """
    Retorna as K palavras mais frequentes
    Se houver empate, ordem alfabética
    """
    # Conta frequências
    contagem = Counter(palavras)
    
    # Usa heap para obter top k
    # Negativo para maior frequência primeiro
    heap = [(-freq, palavra) for palavra, freq in contagem.items()]
    heapq.heapify(heap)
    
    resultado = []
    for _ in range(min(k, len(heap))):
        freq, palavra = heapq.heappop(heap)
        resultado.append(palavra)
    
    return resultado

# Teste
palavras = ["casa", "carro", "casa", "moto", "carro", "casa", "bike"]
print(top_k_frequentes(palavras, 2))  # ['casa', 'carro']
```

### Exercício 8: Interseção de Dois Arrays

```python
def intersecao(nums1, nums2):
    """
    Encontra a interseção de dois arrays (elementos comuns)
    """
    set1 = set(nums1)
    set2 = set(nums2)
    
    # Interseção dos sets
    return list(set1 & set2)

def intersecao_com_duplicatas(nums1, nums2):
    """
    Encontra interseção considerando duplicatas
    """
    from collections import Counter
    
    contagem1 = Counter(nums1)
    contagem2 = Counter(nums2)
    
    resultado = []
    for num in contagem1:
        if num in contagem2:
            # Adiciona o mínimo de ocorrências
            resultado.extend([num] * min(contagem1[num], contagem2[num]))
    
    return resultado

# Teste
print(intersecao([1, 2, 2, 1], [2, 2]))           # [2]
print(intersecao_com_duplicatas([1, 2, 2, 1], [2, 2]))  # [2, 2]
```

---

## 12. Referências {#referencias}

### Livros Recomendados
1. "Estruturas de Dados e Algoritmos em Python" - Michael T. Goodrich
2. "Introduction to Algorithms" - Cormen et al. (Capítulo 11: Hash Tables)
3. "The Art of Computer Programming" - Donald Knuth (Vol. 3, Capítulo 6.4)

### Documentação Oficial
- [Python dict documentation](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
- [CPython dict implementation](https://github.com/python/cpython/blob/main/Objects/dictobject.c)
- [PEP 274 - Dict Comprehensions](https://peps.python.org/pep-0274/)
- [PEP 468 - Preserving Keyword Argument Order](https://peps.python.org/pep-0468/)

### Artigos e Tutoriais
- [How Python's dict works](https://www.laurentluce.com/posts/python-dictionary-implementation/)
- [Hash Tables in Python](https://realpython.com/python-hash-table/)
- [Understanding Hash Maps](https://www.freecodecamp.org/news/python-hash-table-understanding/

### Ferramentas e Bibliotecas
- `collections` - defaultdict, Counter, OrderedDict
- `functools.lru_cache` - Cache decorator
- `hashlib` - Funções hash criptográficas
- `bisect` - Para busca em listas ordenadas

### Próximos Passos
1. **Estudar implementações avançadas**:
   - Cuckoo hashing
   - Hopscotch hashing
   - Perfect hashing

2. **Aplicações práticas**:
   - Bancos de dados NoSQL (Redis, Memcached)
   - Caches distribuídos
   - Índices de busca

3. **Projetos para praticar**:
   - Implementar um banco de dados chave-valor simples
   - Criar um sistema de cache para web
   - Desenvolver um analisador de logs
   - Implementar um spell checker usando hash table

---
