## Manual de Métodos Especiais de Inteiros em Python

### **1. Métodos de Criação e Inicialização**

| Método | Descrição | Exemplo |
|--------|-----------|---------|
| `__new__(cls, value)` | Cria uma nova instância do inteiro | `int.__new__(int, 10)` |
| `__init__(self, value)` | Inicializa o objeto inteiro | `x = int(10)` |

### **2. Representação do Objeto**

| Método | Descrição | Exemplo |
|--------|-----------|---------|
| `__str__(self)` | Retorna representação string amigável | `str(42)` → `"42"` |
| `__repr__(self)` | Retorna representação string oficial | `repr(42)` → `"42"` |
| `__format__(self, spec)` | Formata o inteiro conforme especificação | `format(42, 'b')` → `"101010"` |

### **3. Operadores Aritméticos**

| Método | Operador | Descrição |
|--------|----------|-----------|
| `__add__(self, other)` | `+` | Adição: `x + y` |
| `__sub__(self, other)` | `-` | Subtração: `x - y` |
| `__mul__(self, other)` | `*` | Multiplicação: `x * y` |
| `__truediv__(self, other)` | `/` | Divisão real: `x / y` |
| `__floordiv__(self, other)` | `//` | Divisão inteira: `x // y` |
| `__mod__(self, other)` | `%` | Módulo/resto: `x % y` |
| `__divmod__(self, other)` | `divmod()` | Divisão e módulo: `divmod(x, y)` |
| `__pow__(self, other)` | `**` | Potência: `x ** y` |
| `__neg__(self)` | `-` | Negação unária: `-x` |
| `__pos__(self)` | `+` | Positivo unário: `+x` |
| `__abs__(self)` | `abs()` | Valor absoluto: `abs(x)` |

### **4. Operadores Bitwise**

| Método | Operador | Descrição |
|--------|----------|-----------|
| `__and__(self, other)` | `&` | AND bit a bit: `x & y` |
| `__or__(self, other)` | `\|` | OR bit a bit: `x \| y` |
| `__xor__(self, other)` | `^` | XOR bit a bit: `x ^ y` |
| `__lshift__(self, other)` | `<<` | Deslocamento à esquerda: `x << y` |
| `__rshift__(self, other)` | `>>` | Deslocamento à direita: `x >> y` |
| `__invert__(self)` | `~` | Inversão bit a bit: `~x` |

### **5. Operadores Reversos (Refletidos)**

| Método | Descrição |
|--------|-----------|
| `__radd__(self, other)` | Adição reversa: `y + x` quando `y` não implementa `__add__` |
| `__rsub__(self, other)` | Subtração reversa: `y - x` |
| `__rmul__(self, other)` | Multiplicação reversa: `y * x` |
| `__rtruediv__(self, other)` | Divisão reversa: `y / x` |
| `__rfloordiv__(self, other)` | Divisão inteira reversa: `y // x` |
| `__rmod__(self, other)` | Módulo reverso: `y % x` |
| `__rdivmod__(self, other)` | `divmod(y, x)` |
| `__rpow__(self, other)` | Potência reversa: `y ** x` |
| `__rand__(self, other)` | AND reverso: `y & x` |
| `__ror__(self, other)` | OR reverso: `y \| x` |
| `__rxor__(self, other)` | XOR reverso: `y ^ x` |
| `__rlshift__(self, other)` | Deslocamento esquerda reverso: `y << x` |
| `__rrshift__(self, other)` | Deslocamento direita reverso: `y >> x` |

### **6. Operadores de Comparação**

| Método | Operador | Descrição |
|--------|----------|-----------|
| `__eq__(self, other)` | `==` | Igualdade |
| `__ne__(self, other)` | `!=` | Desigualdade |
| `__lt__(self, other)` | `<` | Menor que |
| `__le__(self, other)` | `<=` | Menor ou igual |
| `__gt__(self, other)` | `>` | Maior que |
| `__ge__(self, other)` | `>=` | Maior ou igual |
| `__bool__(self)` | `bool()` | Conversão para booleano |

### **7. Conversões de Tipo**

| Método | Descrição |
|--------|-----------|
| `__int__(self)` | Converte para inteiro |
| `__float__(self)` | Converte para float |
| `__index__(self)` | Converte para inteiro para indexação |
| `__round__(self, ndigits)` | Arredondamento |
| `__trunc__(self)` | Trunca o número |
| `__floor__(self)` | Arredonda para baixo |
| `__ceil__(self)` | Arredonda para cima |

### **8. Atributos Especiais**

| Atributo | Descrição | Exemplo |
|----------|-----------|---------|
| `numerator` | Numerador da fração | `(42).numerator` |
| `denominator` | Denominador da fração | `(42).denominator` → 1 |
| `real` | Parte real | `(42).real` |
| `imag` | Parte imaginária | `(42).imag` → 0 |

### **9. Métodos de Manipulação de Bits**

| Método | Descrição | Exemplo |
|--------|-----------|---------|
| `bit_length()` | Número de bits necessários | `(42).bit_length()` → 6 |
| `bit_count()` | Número de bits 1 | `(42).bit_count()` → 3 |
| `to_bytes(length, byteorder)` | Converte para bytes | `(42).to_bytes(2, 'big')` |
| `from_bytes(bytes, byteorder)` | Cria inteiro de bytes | `int.from_bytes(b'*', 'big')` |

### **10. Métodos Matemáticos**

| Método | Descrição |
|--------|-----------|
| `as_integer_ratio()` | Retorna tupla (numerador, denominador) |
| `is_integer()` | Verifica se é inteiro (sempre True) |
| `conjugate()` | Conjugado (retorna o próprio número) |

### **11. Métodos de Serialização**

| Método | Descrição |
|--------|-----------|
| `__reduce__(self)` | Suporte para pickle |
| `__reduce_ex__(self, protocol)` | Suporte para pickle com protocolo |
| `__getnewargs__(self)` | Argumentos para unpickle |

### **12. Gerenciamento de Atributos**

| Método | Descrição |
|--------|-----------|
| `__getattribute__(self, name)` | Acesso a atributos |
| `__setattr__(self, name, value)` | Definição de atributos |
| `__delattr__(self, name)` | Deleção de atributos |
| `__dir__(self)` | Lista atributos disponíveis |

### **13. Outros Métodos Especiais**

| Método | Descrição |
|--------|-----------|
| `__hash__(self)` | Retorna hash do objeto |
| `__sizeof__(self)` | Tamanho em bytes |
| `__getstate__(self)` | Estado para pickle |
| `__subclasshook__(cls, subclass)` | Verifica subclasses |

## **Exemplos Práticos**

```python
# Exemplos de uso
x = 42

# Aritmética
print(x + 10)        # __add__
print(x * 2)         # __mul__
print(abs(-x))       # __abs__

# Bitwise
print(x & 7)         # __and__
print(x << 2)        # __lshift__

# Comparação
print(x == 42)       # __eq__
print(x > 40)        # __gt__

# Métodos específicos
print(x.bit_length())    # 6 (110101 tem 6 bits)
print(x.to_bytes(2, 'big'))  # b'\x00*'
print(x.as_integer_ratio())  # (42, 1)
```

## **Notas Importantes**

1. **Inteiros são imutáveis** - Todos os métodos retornam novos valores
2. **Operadores reversos** são chamados quando o operando esquerdo não suporta a operação
3. **Métodos especiais** não devem ser chamados diretamente na maioria dos casos
4. **Herança** - Ao criar subclasses de int, você pode sobrescrever estes métodos

Este manual cobre todos os métodos especiais e atributos disponíveis para objetos inteiros em Python, fornecendo uma referência completa para manipulação de inteiros na linguagem.
