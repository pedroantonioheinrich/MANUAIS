# 🐍 Manual Completo de Programação Orientada a Objetos em Python

## 📚 Índice
1. [Fundamentos da POO](#fundamentos-da-poo)
2. [Classes e Objetos](#classes-e-objetos)
3. [Métodos Especiais](#métodos-especiais)
4. [Encapsulamento](#encapsulamento)
5. [Herança](#herança)
6. [Polimorfismo](#polimorfismo)
7. [Classes Abstratas](#classes-abstratas)
8. [Propriedades](#propriedades)
9. [Métodos de Classe e Estáticos](#métodos-de-classe-e-estáticos)
10. [Mixins e MRO](#mixins-e-mro)
11. [Padrões de Projeto Básicos](#padrões-de-projeto-básicos)
12. [Boas Práticas](#boas-práticas)

---

## 🎯 Fundamentos da POO

### Os 4 Pilares da Orientação a Objetos

1. **Encapsulamento**: Agrupar dados e métodos que operam nesses dados
2. **Abstração**: Esconder detalhes complexos, mostrando apenas funcionalidades essenciais
3. **Herança**: Criar novas classes baseadas em classes existentes
4. **Polimorfismo**: Usar uma interface única para operar em objetos de diferentes tipos

---

## 🏗️ Classes e Objetos

### Criando uma Classe Básica

```python
class Pessoa:
    """Classe que representa uma pessoa."""
    
    # Atributo de classe (compartilhado por todas as instâncias)
    especie = "Humano"
    
    def __init__(self, nome, idade):
        """Método construtor (inicializador)."""
        # Atributos de instância (únicos para cada objeto)
        self.nome = nome
        self.idade = idade
        self.__documento = None  # Atributo "privado"
    
    def apresentar(self):
        """Método de instância."""
        return f"Olá, meu nome é {self.nome} e tenho {self.idade} anos."
    
    def aniversario(self):
        """Outro método de instância."""
        self.idade += 1
        return f"Feliz aniversário! Agora tenho {self.idade} anos."

# Criando objetos (instâncias)
pessoa1 = Pessoa("Alice", 25)
pessoa2 = Pessoa("Bob", 30)

print(pessoa1.apresentar())  # Olá, meu nome é Alice e tenho 25 anos.
print(pessoa2.apresentar())  # Olá, meu nome é Bob e tenho 30 anos.

# Acessando atributos
print(pessoa1.nome)     # Alice
print(pessoa1.idade)    # 25
print(Pessoa.especie)   # Humano (atributo de classe)
```

---

## 🔧 Métodos Especiais (Dunder Methods)

### Métodos de Representação

```python
class Produto:
    def __init__(self, nome, preco, quantidade):
        self.nome = nome
        self.preco = preco
        self.quantidade = quantidade
    
    # __str__: Representação legível para humanos
    def __str__(self):
        return f"Produto: {self.nome} - R${self.preco:.2f}"
    
    # __repr__: Representação oficial para desenvolvedores
    def __repr__(self):
        return f"Produto('{self.nome}', {self.preco}, {self.quantidade})"
    
    # __len__: Define comportamento para len()
    def __len__(self):
        return self.quantidade
    
    # __add__: Define comportamento para o operador +
    def __add__(self, outro):
        if isinstance(outro, Produto):
            return self.preco + outro.preco
        return self.preco + outro
    
    # __eq__: Define comportamento para ==
    def __eq__(self, outro):
        if not isinstance(outro, Produto):
            return False
        return self.nome == outro.nome and self.preco == outro.preco
    
    # __lt__: Define comportamento para <
    def __lt__(self, outro):
        return self.preco < outro.preco

# Exemplo de uso
produto1 = Produto("Notebook", 3500.00, 10)
produto2 = Produto("Mouse", 150.00, 50)

print(str(produto1))     # Produto: Notebook - R$3500.00
print(repr(produto1))    # Produto('Notebook', 3500.0, 10)
print(len(produto1))     # 10
print(produto1 + produto2)  # 3650.0
print(produto1 == produto2) # False
print(produto1 < produto2)  # False

# Lista de métodos especiais importantes:
"""
__init__      - Inicialização do objeto
__del__       - Destruição do objeto
__str__       - Representação string para humanos
__repr__      - Representação string oficial
__len__       - Comportamento para len()
__getitem__   - Acesso por índice []
__setitem__   - Atribuição por índice []
__iter__      - Iteração sobre o objeto
__next__      - Próximo item na iteração
__contains__  - Comportamento para operador 'in'
__call__      - Permite chamar objeto como função
__add__       - Operador +
__sub__       - Operador -
__mul__       - Operador *
__truediv__   - Operador /
__eq__        - Operador ==
__ne__        - Operador !=
__lt__        - Operador <
__gt__        - Operador >
__le__        - Operador <=
__ge__        - Operador >=
"""
```

### Exemplo Avançado: Vetor Matemático

```python
class Vetor:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, outro):
        return Vetor(self.x + outro.x, self.y + outro.y)
    
    def __sub__(self, outro):
        return Vetor(self.x - outro.x, self.y - outro.y)
    
    def __mul__(self, escalar):
        return Vetor(self.x * escalar, self.y * escalar)
    
    def __truediv__(self, escalar):
        if escalar == 0:
            raise ValueError("Divisão por zero")
        return Vetor(self.x / escalar, self.y / escalar)
    
    def __abs__(self):
        return (self.x**2 + self.y**2) ** 0.5
    
    def __str__(self):
        return f"Vetor({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vetor({self.x}, {self.y})"
    
    def __eq__(self, outro):
        return self.x == outro.x and self.y == outro.y
    
    def __ne__(self, outro):
        return not self.__eq__(outro)

# Uso
v1 = Vetor(3, 4)
v2 = Vetor(1, 2)

print(v1 + v2)   # Vetor(4, 6)
print(v1 - v2)   # Vetor(2, 2)
print(v1 * 2)    # Vetor(6, 8)
print(v1 / 2)    # Vetor(1.5, 2.0)
print(abs(v1))   # 5.0
```

---

## 🔒 Encapsulamento

### Níveis de Acesso em Python

```python
class ContaBancaria:
    def __init__(self, titular, saldo_inicial=0):
        self.titular = titular            # Público (acesso direto)
        self._agencia = "0001"            # Protegido (convenção)
        self.__saldo = saldo_inicial      # Privado (name mangling)
        self.__historico = []             # Privado
    
    # Método público para acessar saldo
    def get_saldo(self):
        """Getter tradicional."""
        return self.__saldo
    
    # Método público para modificar saldo
    def depositar(self, valor):
        """Método público para depósito."""
        if valor > 0:
            self.__saldo += valor
            self.__registrar_transacao(f"Depósito: R${valor:.2f}")
            return True
        return False
    
    def sacar(self, valor):
        """Método público para saque."""
        if 0 < valor <= self.__saldo:
            self.__saldo -= valor
            self.__registrar_transacao(f"Saque: R${valor:.2f}")
            return True
        return False
    
    # Método privado
    def __registrar_transacao(self, descricao):
        """Método privado para registrar transações."""
        self.__historico.append(descricao)
    
    # Método protegido
    def _consultar_historico(self):
        """Método protegido para consultar histórico."""
        return self.__historico.copy()

# Testando encapsulamento
conta = ContaBancaria("Carlos", 1000)

print(conta.titular)          # Carlos (acesso direto)
print(conta._agencia)         # 0001 (acesso com aviso)
# print(conta.__saldo)        # Erro! Atributo privado
# print(conta.__historico)    # Erro! Atributo privado

print(conta.get_saldo())      # 1000 (acesso via getter)
conta.depositar(500)          # Método público
print(conta.get_saldo())      # 1500

# Name mangling (ainda é possível acessar, mas não recomendado)
print(conta._ContaBancaria__saldo)  # 1500
```

---

## 👨‍👩‍👧‍👦 Herança

### Herança Simples

```python
class Animal:
    def __init__(self, nome, especie):
        self.nome = nome
        self.especie = especie
    
    def emitir_som(self):
        return "Som genérico de animal"
    
    def mover(self):
        return f"{self.nome} está se movendo"
    
    def __str__(self):
        return f"{self.nome} ({self.especie})"

class Mamifero(Animal):
    def __init__(self, nome, especie, tem_pelo=True):
        # Chamando construtor da classe pai
        super().__init__(nome, especie)
        self.tem_pelo = tem_pelo
        self.gestacao_meses = 0
    
    def amamentar(self):
        return f"{self.nome} está amamentando"
    
    def emitir_som(self):
        return "Som de mamífero"

class Cachorro(Mamifero):
    def __init__(self, nome, raca):
        super().__init__(nome, "Canis lupus familiaris")
        self.raca = raca
    
    # Sobrescrevendo método
    def emitir_som(self):
        return "Au au!"
    
    def buscar_osso(self):
        return f"{self.nome} está buscando osso"

class Gato(Mamifero):
    def __init__(self, nome, raca, vidas=9):
        super().__init__(nome, "Felis catus")
        self.raca = raca
        self.vidas = vidas
    
    def emitir_som(self):
        return "Miau!"
    
    def arranhar(self):
        return f"{self.nome} está arranhando"

# Testando herança
rex = Cachorro("Rex", "Pastor Alemão")
whiskers = Gato("Whiskers", "Siamês")

print(rex)                    # Rex (Canis lupus familiaris)
print(rex.emitir_som())       # Au au!
print(rex.amamentar())        # Rex está amamentando
print(rex.buscar_osso())      # Rex está buscando osso

print(whiskers.emitir_som())  # Miau!
print(whiskers.arranhar())    # Whiskers está arranhando

# Verificando relacionamentos
print(isinstance(rex, Cachorro))      # True
print(isinstance(rex, Mamifero))      # True
print(isinstance(rex, Animal))        # True
print(isinstance(rex, Gato))          # False

print(issubclass(Cachorro, Mamifero)) # True
print(issubclass(Cachorro, Animal))   # True
```

### Herança Múltipla

```python
class Veiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
    
    def ligar(self):
        return "Veículo ligado"
    
    def desligar(self):
        return "Veículo desligado"

class Voador:
    def __init__(self, altitude_maxima):
        self.altitude_maxima = altitude_maxima
    
    def voar(self):
        return "Voando..."
    
    def pousar(self):
        return "Pousando..."

class Aquatico:
    def __init__(self, profundidade_maxima):
        self.profundidade_maxima = profundidade_maxima
    
    def navegar(self):
        return "Navegando..."
    
    def submergir(self):
        return "Submergindo..."

class Anfibio(Veiculo, Voador, Aquatico):
    def __init__(self, marca, modelo, altitude_maxima, profundidade_maxima):
        Veiculo.__init__(self, marca, modelo)
        Voador.__init__(self, altitude_maxima)
        Aquatico.__init__(self, profundidade_maxima)
    
    def modo_terrestre(self):
        return f"{self.marca} {self.modelo} em modo terrestre"
    
    def modo_aquatico(self):
        return f"{self.marca} {self.modelo} em modo aquatico"

# Exemplo de uso
anfibio = Anfibio("AquaFly", "X200", 1000, 50)

print(anfibio.ligar())        # Veículo ligado
print(anfibio.voar())         # Voando...
print(anfibio.navegar())      # Navegando...
print(anfibio.modo_terrestre()) # AquaFly X200 em modo terrestre

# MRO (Method Resolution Order)
print(Anfibio.__mro__)
# (<class '__main__.Anfibio'>, <class '__main__.Veiculo'>, 
#  <class '__main__.Voador'>, <class '__main__.Aquatico'>, <class 'object'>)
```

---

## 🎭 Polimorfismo

```python
class FormaGeometrica:
    def calcular_area(self):
        raise NotImplementedError("Método abstrato")
    
    def calcular_perimetro(self):
        raise NotImplementedError("Método abstrato")
    
    def __str__(self):
        return self.__class__.__name__

class Retangulo(FormaGeometrica):
    def __init__(self, base, altura):
        self.base = base
        self.altura = altura
    
    def calcular_area(self):
        return self.base * self.altura
    
    def calcular_perimetro(self):
        return 2 * (self.base + self.altura)

class Circulo(FormaGeometrica):
    def __init__(self, raio):
        self.raio = raio
    
    def calcular_area(self):
        return 3.14159 * self.raio ** 2
    
    def calcular_perimetro(self):
        return 2 * 3.14159 * self.raio

class Triangulo(FormaGeometrica):
    def __init__(self, lado1, lado2, lado3):
        self.lado1 = lado1
        self.lado2 = lado2
        self.lado3 = lado3
    
    def calcular_area(self):
        # Fórmula de Heron
        s = self.calcular_perimetro() / 2
        return (s * (s - self.lado1) * (s - self.lado2) * (s - self.lado3)) ** 0.5
    
    def calcular_perimetro(self):
        return self.lado1 + self.lado2 + self.lado3

# Função polimórfica
def calcular_e_imprimir(formas):
    """Aceita qualquer objeto que tenha os métodos calcular_area e calcular_perimetro."""
    for forma in formas:
        print(f"{forma}:")
        print(f"  Área: {forma.calcular_area():.2f}")
        print(f"  Perímetro: {forma.calcular_perimetro():.2f}")
        print()

# Criando lista de formas diferentes
formas = [
    Retangulo(5, 3),
    Circulo(4),
    Triangulo(3, 4, 5)
]

calcular_e_imprimir(formas)

# Duck typing - Outro exemplo
class Pato:
    def grasnar(self):
        return "Quack!"
    
    def voar(self):
        return "Voando como pato"

class Galinha:
    def cacarejar(self):
        return "Cocoricó!"
    
    def voar(self):
        return "Voando como galinha"

class Avião:
    def decolar(self):
        return "Decolando"
    
    def voar(self):
        return "Voando como avião"

# Função que usa duck typing
def fazer_voar(obj):
    if hasattr(obj, 'voar'):
        print(obj.voar())
    else:
        print("Não pode voar")

# Testando duck typing
objetos = [Pato(), Galinha(), Avião(), "string"]
for obj in objetos:
    fazer_voar(obj)
```

---

## 📐 Classes Abstratas

```python
from abc import ABC, abstractmethod
from typing import List

class Funcionario(ABC):
    def __init__(self, nome, matricula):
        self.nome = nome
        self.matricula = matricula
        self.salario_base = 0
    
    @abstractmethod
    def calcular_salario(self) -> float:
        """Método abstrato - deve ser implementado pelas subclasses."""
        pass
    
    @abstractmethod
    def get_cargo(self) -> str:
        pass
    
    def get_info(self) -> str:
        return f"{self.nome} ({self.matricula}) - {self.get_cargo()}"

class Desenvolvedor(Funcionario):
    def __init__(self, nome, matricula, nivel):
        super().__init__(nome, matricula)
        self.nivel = nivel  # Júnior, Pleno, Sênior
        self.salario_base = 3000 if nivel == "Júnior" else 5000 if nivel == "Pleno" else 8000
    
    def calcular_salario(self) -> float:
        # Salário base + bônus por nível
        bonus = 0.1 if self.nivel == "Júnior" else 0.2 if self.nivel == "Pleno" else 0.3
        return self.salario_base * (1 + bonus)
    
    def get_cargo(self) -> str:
        return f"Desenvolvedor {self.nivel}"
    
    def programar(self):
        return f"{self.nome} está programando"

class Gerente(Funcionario):
    def __init__(self, nome, matricula, departamento):
        super().__init__(nome, matricula)
        self.departamento = departamento
        self.salario_base = 10000
    
    def calcular_salario(self) -> float:
        # Salário base + bônus por departamento
        bonus_departamento = 0.2 if self.departamento == "TI" else 0.15
        return self.salario_base * (1 + bonus_departamento)
    
    def get_cargo(self) -> str:
        return f"Gerente de {self.departamento}"
    
    def gerenciar(self):
        return f"{self.nome} está gerenciando o departamento {self.departamento}"

# Classe abstrata com propriedades abstratas
class Forma(ABC):
    @property
    @abstractmethod
    def area(self):
        pass
    
    @property
    @abstractmethod
    def perimetro(self):
        pass

class Quadrado(Forma):
    def __init__(self, lado):
        self._lado = lado
    
    @property
    def area(self):
        return self._lado ** 2
    
    @property
    def perimetro(self):
        return 4 * self._lado

# Testando
try:
    funcionario = Funcionario("João", "001")  # Erro! Não pode instanciar classe abstrata
except TypeError as e:
    print(f"Erro esperado: {e}")

dev = Desenvolvedor("Maria", "002", "Sênior")
gerente = Gerente("Carlos", "003", "TI")

funcionarios: List[Funcionario] = [dev, gerente]

for func in funcionarios:
    print(f"{func.get_info()}")
    print(f"Salário: R${func.calcular_salario():.2f}")
    print()
```

---

## 🛡️ Propriedades (Properties)

```python
class Pessoa:
    def __init__(self, nome, ano_nascimento):
        self._nome = nome
        self._ano_nascimento = ano_nascimento
        self._idade = None
    
    @property
    def nome(self):
        """Getter para nome."""
        return self._nome.title()  # Retorna sempre com a primeira letra maiúscula
    
    @nome.setter
    def nome(self, valor):
        """Setter para nome."""
        if not valor or not valor.strip():
            raise ValueError("Nome não pode ser vazio")
        if len(valor) < 2:
            raise ValueError("Nome deve ter pelo menos 2 caracteres")
        self._nome = valor
    
    @nome.deleter
    def nome(self):
        """Deleter para nome."""
        print(f"Deletando nome: {self._nome}")
        self._nome = "Sem Nome"
    
    @property
    def ano_nascimento(self):
        """Propriedade somente leitura."""
        return self._ano_nascimento
    
    @property
    def idade(self):
        """Propriedade calculada."""
        from datetime import datetime
        if self._idade is None:
            ano_atual = datetime.now().year
            self._idade = ano_atual - self._ano_nascimento
        return self._idade
    
    @property
    def inicial(self):
        """Propriedade derivada."""
        return self._nome[0].upper() if self._nome else ""

class Retangulo:
    def __init__(self, largura, altura):
        self._largura = largura
        self._altura = altura
        self._area = None
    
    @property
    def largura(self):
        return self._largura
    
    @largura.setter
    def largura(self, valor):
        if valor <= 0:
            raise ValueError("Largura deve ser positiva")
        self._largura = valor
        self._area = None  # Invalida cache
    
    @property
    def altura(self):
        return self._altura
    
    @altura.setter
    def altura(self, valor):
        if valor <= 0:
            raise ValueError("Altura deve ser positiva")
        self._altura = valor
        self._area = None  # Invalida cache
    
    @property
    def area(self):
        """Propriedade calculada com cache."""
        if self._area is None:
            print("Calculando área...")
            self._area = self._largura * self._altura
        return self._area
    
    @property
    def perimetro(self):
        """Propriedade calculada sem cache."""
        return 2 * (self._largura + self._altura)

# Exemplo de uso
pessoa = Pessoa("maria silva", 1990)

print(pessoa.nome)           # Maria Silva (getter)
print(pessoa.idade)          # Idade calculada
print(pessoa.inicial)        # M

pessoa.nome = "joão santos"  # setter
print(pessoa.nome)           # João Santos

# pessoa.nome = ""           # ValueError: Nome não pode ser vazio
# pessoa.nome = "A"          # ValueError: Nome deve ter pelo menos 2 caracteres

# Propriedade somente leitura
# pessoa.ano_nascimento = 2000  # AttributeError: can't set attribute

# Usando deleter
del pessoa.nome
print(pessoa.nome)           # Sem Nome

# Exemplo com Retangulo
ret = Retangulo(5, 3)
print(f"Área: {ret.area}")   # Calculando área... 15
print(f"Área novamente: {ret.area}")  # 15 (usando cache)

ret.largura = 10
print(f"Área após mudança: {ret.area}")  # Calculando área... 30
```

---

## 🏢 Métodos de Classe e Estáticos

```python
class Data:
    def __init__(self, dia, mes, ano):
        self.dia = dia
        self.mes = mes
        self.ano = ano
    
    # Método de instância (recebe self)
    def formatar(self, formato="dd/mm/aaaa"):
        if formato == "dd/mm/aaaa":
            return f"{self.dia:02d}/{self.mes:02d}/{self.ano}"
        elif formato == "mm/dd/aaaa":
            return f"{self.mes:02d}/{self.dia:02d}/{self.ano}"
        else:
            return f"{self.ano}-{self.mes:02d}-{self.dia:02d}"
    
    # Método de classe (recebe cls)
    @classmethod
    def from_string(cls, data_str, formato="dd/mm/aaaa"):
        """Método de classe para criar objeto a partir de string."""
        if formato == "dd/mm/aaaa":
            dia, mes, ano = map(int, data_str.split('/'))
        elif formato == "mm/dd/aaaa":
            mes, dia, ano = map(int, data_str.split('/'))
        elif formato == "aaaa-mm-dd":
            ano, mes, dia = map(int, data_str.split('-'))
        else:
            raise ValueError("Formato inválido")
        
        return cls(dia, mes, ano)
    
    @classmethod
    def hoje(cls):
        """Método de classe que retorna data atual."""
        from datetime import datetime
        hoje = datetime.now()
        return cls(hoje.day, hoje.month, hoje.year)
    
    # Método estático (não recebe self nem cls)
    @staticmethod
    def validar_data(dia, mes, ano):
        """Valida se uma data é válida."""
        if mes < 1 or mes > 12:
            return False
        
        dias_no_mes = [31, 29 if Data.is_ano_bissexto(ano) else 28, 31, 30, 31, 30, 
                       31, 31, 30, 31, 30, 31]
        
        return 1 <= dia <= dias_no_mes[mes - 1]
    
    @staticmethod
    def is_ano_bissexto(ano):
        """Verifica se um ano é bissexto."""
        return (ano % 4 == 0 and ano % 100 != 0) or (ano % 400 == 0)
    
    @staticmethod
    def diferenca_dias(data1, data2):
        """Calcula diferença em dias entre duas datas."""
        from datetime import date
        d1 = date(data1.ano, data1.mes, data1.dia)
        d2 = date(data2.ano, data2.mes, data2.dia)
        return abs((d2 - d1).days)

# Exemplos
# Método de instância
data1 = Data(15, 5, 2023)
print(data1.formatar())             # 15/05/2023
print(data1.formatar("mm/dd/aaaa")) # 05/15/2023

# Método de classe
data2 = Data.from_string("20/12/2023")
print(data2.formatar())             # 20/12/2023

data3 = Data.hoje()
print(data3.formatar())             # Data atual

# Método estático
print(Data.validar_data(29, 2, 2020))  # True (ano bissexto)
print(Data.validar_data(29, 2, 2021))  # False (não é bissexto)
print(Data.is_ano_bissexto(2020))      # True
print(Data.is_ano_bissexto(2021))      # False

# Diferença entre datas
data4 = Data(1, 1, 2023)
data5 = Data(1, 1, 2024)
print(f"Dias entre: {Data.diferenca_dias(data4, data5)}")  # 365 ou 366
```

---

## 🔀 Mixins e MRO

```python
# Mixins são classes que fornecem funcionalidades específicas
# para serem combinadas com outras classes

class LogMixin:
    """Mixin para funcionalidades de logging."""
    
    def log(self, mensagem):
        """Registra uma mensagem de log."""
        print(f"[LOG] {self.__class__.__name__}: {mensagem}")
    
    def log_erro(self, mensagem):
        """Registra uma mensagem de erro."""
        print(f"[ERRO] {self.__class__.__name__}: {mensagem}")

class SerializavelMixin:
    """Mixin para funcionalidades de serialização."""
    
    def to_dict(self):
        """Converte objeto para dicionário."""
        return {attr: getattr(self, attr) for attr in dir(self) 
                if not attr.startswith('_') and not callable(getattr(self, attr))}
    
    def to_json(self):
        """Converte objeto para JSON."""
        import json
        return json.dumps(self.to_dict(), indent=2)

class PersistenciaMixin:
    """Mixin para funcionalidades de persistência."""
    
    def salvar(self, arquivo):
        """Salva objeto em arquivo."""
        with open(arquivo, 'w') as f:
            f.write(str(self))
        self.log(f"Objeto salvo em {arquivo}")
    
    def carregar(self, arquivo):
        """Carrega objeto de arquivo."""
        with open(arquivo, 'r') as f:
            dados = f.read()
        self.log(f"Objeto carregado de {arquivo}")
        return dados

# Classe que usa múltiplos mixins
class Produto(LogMixin, SerializavelMixin, PersistenciaMixin):
    def __init__(self, nome, preco, quantidade):
        self.nome = nome
        self.preco = preco
        self.quantidade = quantidade
        self.log(f"Produto '{nome}' criado")
    
    def __str__(self):
        return f"Produto: {self.nome}, Preço: R${self.preco:.2f}, Quantidade: {self.quantidade}"
    
    def vender(self, quantidade):
        if quantidade <= self.quantidade:
            self.quantidade -= quantidade
            self.log(f"Vendidos {quantidade} unidades de {self.nome}")
            return True
        else:
            self.log_erro(f"Estoque insuficiente para vender {quantidade} unidades")
            return False

# Testando
produto = Produto("Notebook", 3500.00, 10)

# Métodos do Produto
produto.vender(3)           # [LOG] Produto: Vendidos 3 unidades de Notebook
produto.vender(10)          # [ERRO] Produto: Estoque insuficiente...

# Métodos dos mixins
print(produto.to_dict())    # {'nome': 'Notebook', 'preco': 3500.0, 'quantidade': 7}
print(produto.to_json())    # JSON formatado

# produto.salvar("produto.txt")  # Salva em arquivo
# dados = produto.carregar("produto.txt")  # Carrega do arquivo

# Verificando MRO
print(Produto.__mro__)
# (<class '__main__.Produto'>, <class '__main__.LogMixin'>, 
#  <class '__main__.SerializavelMixin'>, <class '__main__.PersistenciaMixin'>, <class 'object'>)
```

---

## 🎨 Padrões de Projeto Básicos

### Singleton

```python
class ConfiguracaoSingleton:
    _instancia = None
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
            cls._instancia._inicializar()
        return cls._instancia
    
    def _inicializar(self):
        self.config = {
            "ambiente": "desenvolvimento",
            "debug": True,
            "timeout": 30,
            "banco_dados": {
                "host": "localhost",
                "porta": 5432,
                "usuario": "admin"
            }
        }
    
    def get(self, chave):
        return self.config.get(chave)
    
    def set(self, chave, valor):
        self.config[chave] = valor
    
    def __str__(self):
        return f"Configuração: {self.config}"

# Testando Singleton
config1 = ConfiguracaoSingleton()
config2 = ConfiguracaoSingleton()

print(config1 is config2)  # True - mesma instância

config1.set("timeout", 60)
print(config2.get("timeout"))  # 60 - compartilham estado
```

### Factory Method

```python
from abc import ABC, abstractmethod

class Documento(ABC):
    @abstractmethod
    def abrir(self):
        pass
    
    @abstractmethod
    def fechar(self):
        pass

class PDFDocumento(Documento):
    def abrir(self):
        return "Abrindo documento PDF..."
    
    def fechar(self):
        return "Fechando documento PDF..."
    
    def imprimir(self):
        return "Imprimindo PDF..."

class WordDocumento(Documento):
    def abrir(self):
        return "Abrindo documento Word..."
    
    def fechar(self):
        return "Fechando documento Word..."
    
    def editar(self):
        return "Editando documento Word..."

class ExcelDocumento(Documento):
    def abrir(self):
        return "Abrindo planilha Excel..."
    
    def fechar(self):
        return "Fechando planilha Excel..."
    
    def calcular(self):
        return "Calculando fórmulas..."

class DocumentoFactory:
    @staticmethod
    def criar_documento(tipo):
        if tipo == "pdf":
            return PDFDocumento()
        elif tipo == "word":
            return WordDocumento()
        elif tipo == "excel":
            return ExcelDocumento()
        else:
            raise ValueError(f"Tipo de documento desconhecido: {tipo}")

# Uso do Factory
documentos = [
    DocumentoFactory.criar_documento("pdf"),
    DocumentoFactory.criar_documento("word"),
    DocumentoFactory.criar_documento("excel")
]

for doc in documentos:
    print(doc.abrir())
    # Métodos específicos
    if isinstance(doc, PDFDocumento):
        print(doc.imprimir())
    elif isinstance(doc, WordDocumento):
        print(doc.editar())
    elif isinstance(doc, ExcelDocumento):
        print(doc.calcular())
    print(doc.fechar())
    print()
```

### Observer

```python
class Observer:
    """Interface para observadores."""
    def update(self, mensagem):
        raise NotImplementedError

class Subject:
    """Classe observável."""
    def __init__(self):
        self._observers = []
    
    def adicionar_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)
    
    def remover_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)
    
    def notificar_observers(self, mensagem):
        for observer in self._observers:
            observer.update(mensagem)

# Observadores concretos
class LoggerObserver(Observer):
    def update(self, mensagem):
        print(f"[LOG] {mensagem}")

class EmailObserver(Observer):
    def __init__(self, email):
        self.email = email
    
    def update(self, mensagem):
        print(f"Enviando email para {self.email}: {mensagem}")

class SMSObserver(Observer):
    def __init__(self, telefone):
        self.telefone = telefone
    
    def update(self, mensagem):
        print(f"Enviando SMS para {self.telefone}: {mensagem}")

# Sujeito concreto
class SistemaPedidos(Subject):
    def __init__(self):
        super().__init__()
        self.pedidos = []
    
    def adicionar_pedido(self, pedido):
        self.pedidos.append(pedido)
        self.notificar_observers(f"Novo pedido: {pedido}")
    
    def atualizar_status(self, pedido_id, status):
        mensagem = f"Pedido {pedido_id} atualizado para: {status}"
        self.notificar_observers(mensagem)

# Testando
sistema = SistemaPedidos()

# Criando observadores
logger = LoggerObserver()
email_admin = EmailObserver("admin@empresa.com")
sms_cliente = SMSObserver("+55 11 99999-9999")

# Registrando observadores
sistema.adicionar_observer(logger)
sistema.adicionar_observer(email_admin)
sistema.adicionar_observer(sms_cliente)

# Simulando eventos
sistema.adicionar_pedido("Pedido #001")
sistema.atualizar_status("#001", "Em preparação")

# Removendo um observador
sistema.remover_observer(sms_cliente)
sistema.atualizar_status("#001", "Enviado")
```

---

## 📋 Boas Práticas em POO com Python

### 1. Princípios SOLID

```python
# S - Single Responsibility Principle
class Pedido:
    def __init__(self, itens):
        self.itens = itens
    
    def calcular_total(self):
        return sum(item.preco for item in self.itens)

class PedidoRepository:
    def salvar(self, pedido):
        # Lógica para salvar no banco de dados
        pass
    
    def carregar(self, pedido_id):
        # Lógica para carregar do banco de dados
        pass

# O - Open/Closed Principle
class Desconto:
    def calcular(self, valor):
        raise NotImplementedError

class DescontoPercentual(Desconto):
    def __init__(self, percentual):
        self.percentual = percentual
    
    def calcular(self, valor):
        return valor * (self.percentual / 100)

class DescontoFixo(Desconto):
    def __init__(self, valor_fixo):
        self.valor_fixo = valor_fixo
    
    def calcular(self, valor):
        return self.valor_fixo

# L - Liskov Substitution Principle
class Pato:
    def voar(self):
        return "Voando"
    
    def grasnar(self):
        return "Quack!"

class PatoDeBorracha(Pato):
    def voar(self):
        raise NotImplementedError("Pato de borracha não voa")
    
    def grasnar(self):
        return "Squeak!"

# I - Interface Segregation Principle
class Trabalhador:
    def trabalhar(self):
        pass

class Comedor:
    def comer(self):
        pass

class Dorminhoco:
    def dormir(self):
        pass

class Humano(Trabalhador, Comedor, Dorminhoco):
    def trabalhar(self):
        return "Trabalhando"
    
    def comer(self):
        return "Comendo"
    
    def dormir(self):
        return "Dormindo"

class Robo(Trabalhador):
    def trabalhar(self):
        return "Trabalhando 24/7"

# D - Dependency Inversion Principle
class EmailService:
    def enviar(self, mensagem, destinatario):
        print(f"Enviando email para {destinatario}: {mensagem}")

class SMSService:
    def enviar(self, mensagem, destinatario):
        print(f"Enviando SMS para {destinatario}: {mensagem}")

class Notificador:
    def __init__(self, servico):
        self.servico = servico
    
    def notificar(self, mensagem, destinatario):
        self.servico.enviar(mensagem, destinatario)

# Uso com injeção de dependência
email_service = EmailService()
sms_service = SMSService()

notificador_email = Notificador(email_service)
notificador_sms = Notificador(sms_service)

notificador_email.notificar("Sua conta foi criada", "usuario@email.com")
notificador_sms.notificar("Código de verificação: 123456", "+55 11 99999-9999")
```

### 2. Convenções de Nomenclatura

```python
"""
Convenções Python para POO:

1. Classes: PascalCase
   class MinhaClasse:
   
2. Métodos e funções: snake_case
   def meu_metodo():
   
3. Atributos públicos: snake_case
   self.meu_atributo
   
4. Atributos privados: _snake_case (um underscore)
   self._atributo_protegido
   
5. Atributos fortemente privados: __snake_case (dois underscores)
   self.__atributo_privado
   
6. Constantes: SCREAMING_SNAKE_CASE
   MAX_TENTATIVAS = 3
   
7. Métodos especiais: __dunder_method__
   def __init__():
"""

class ExemploConvencoes:
    """Exemplo demonstrando convenções de nomenclatura."""
    
    # Constante da classe
    VERSION = "1.0.0"
    MAX_LIMIT = 100
    
    def __init__(self):
        # Atributos públicos
        self.nome = ""
        self.idade = 0
        
        # Atributo protegido (convenção)
        self._salario = 0
        
        # Atributo privado (name mangling)
        self.__senha = ""
        
        # Atributo "mágico" (evitar criar novos)
        # self.__alguma_coisa__ = "não faça isso"
    
    # Método público
    def calcular_idade(self):
        pass
    
    # Método protegido
    def _validar_dados(self):
        pass
    
    # Método privado
    def __gerar_senha(self):
        pass
    
    # Método estático
    @staticmethod
    def helper_method():
        pass
    
    # Método de classe
    @classmethod
    def criar_padrao(cls):
        pass
```

### 3. Documentação e Type Hints

```python
from typing import List, Optional, Dict, Union, Tuple
from datetime import datetime

class ContaCorrente:
    """
    Classe que representa uma conta corrente bancária.
    
    Atributos:
        titular (str): Nome do titular da conta
        saldo (float): Saldo atual da conta
        limite (float): Limite de cheque especial
        transacoes (List[Dict]): Histórico de transações
    """
    
    def __init__(self, titular: str, saldo_inicial: float = 0.0, 
                 limite: float = 1000.0) -> None:
        """
        Inicializa uma nova conta corrente.
        
        Args:
            titular: Nome do titular da conta
            saldo_inicial: Saldo inicial da conta (padrão: 0.0)
            limite: Limite de cheque especial (padrão: 1000.0)
        
        Raises:
            ValueError: Se saldo_inicial ou limite forem negativos
        """
        if saldo_inicial < 0:
            raise ValueError("Saldo inicial não pode ser negativo")
        if limite < 0:
            raise ValueError("Limite não pode ser negativo")
        
        self.titular: str = titular
        self._saldo: float = saldo_inicial
        self._limite: float = limite
        self.transacoes: List[Dict] = []
        
        self._registrar_transacao("Abertura de conta", saldo_inicial)
    
    @property
    def saldo(self) -> float:
        """
        Saldo atual da conta (incluindo limite disponível).
        
        Returns:
            Saldo disponível para saque
        """
        return self._saldo + self._limite
    
    @property
    def saldo_disponivel(self) -> float:
        """
        Saldo positivo (sem considerar limite).
        
        Returns:
            Saldo positivo ou zero
        """
        return max(self._saldo, 0)
    
    def depositar(self, valor: float, descricao: str = "Depósito") -> bool:
        """
        Realiza um depósito na conta.
        
        Args:
            valor: Valor a ser depositado
            descricao: Descrição da transação (padrão: "Depósito")
        
        Returns:
            True se o depósito foi realizado com sucesso
        
        Raises:
            ValueError: Se o valor for negativo ou zero
        """
        if valor <= 0:
            raise ValueError("Valor do depósito deve ser positivo")
        
        self._saldo += valor
        self._registrar_transacao(descricao, valor)
        return True
    
    def sacar(self, valor: float, descricao: str = "Saque") -> bool:
        """
        Realiza um saque na conta.
        
        Args:
            valor: Valor a ser sacado
            descricao: Descrição da transação (padrão: "Saque")
        
        Returns:
            True se o saque foi realizado com sucesso
        
        Raises:
            ValueError: Se o valor for negativo ou zero
        """
        if valor <= 0:
            raise ValueError("Valor do saque deve ser positivo")
        
        if valor > self.saldo:
            return False
        
        self._saldo -= valor
        self._registrar_transacao(descricao, -valor)
        return True
    
    def transferir(self, valor: float, conta_destino: 'ContaCorrente', 
                   descricao: str = "Transferência") -> bool:
        """
        Transfere valor para outra conta.
        
        Args:
            valor: Valor a transferir
            conta_destino: Conta de destino
            descricao: Descrição da transação
        
        Returns:
            True se a transferência foi bem sucedida
        """
        if self.sacar(valor, f"{descricao} para {conta_destino.titular}"):
            conta_destino.depositar(valor, f"{descricao} de {self.titular}")
            return True
        return False
    
    def _registrar_transacao(self, descricao: str, valor: float) -> None:
        """
        Registra uma transação no histórico.
        
        Método privado para uso interno.
        """
        transacao = {
            "data": datetime.now(),
            "descricao": descricao,
            "valor": valor,
            "saldo_apos": self._saldo
        }
        self.transacoes.append(transacao)
    
    def extrato(self, ultimas_n: Optional[int] = None) -> str:
        """
        Gera extrato das transações.
        
        Args:
            ultimas_n: Número de transações a mostrar (None para todas)
        
        Returns:
            String formatada com o extrato
        """
        transacoes = self.transacoes if ultimas_n is None else self.transacoes[-ultimas_n:]
        
        extrato = f"Extrato da conta de {self.titular}\n"
        extrato += "=" * 40 + "\n"
        
        for transacao in transacoes:
            data = transacao["data"].strftime("%d/%m/%Y %H:%M")
            valor = f"R${transacao['valor']:+.2f}"
            extrato += f"{data} | {transacao['descricao']:20} | {valor:10} | "
            extrato += f"Saldo: R${transacao['saldo_apos']:10.2f}\n"
        
        extrato += "=" * 40 + "\n"
        extrato += f"Saldo atual: R${self._saldo:.2f}\n"
        extrato += f"Limite disponível: R${self.saldo:.2f}\n"
        
        return extrato
    
    def __str__(self) -> str:
        """Representação string da conta."""
        return f"Conta de {self.titular} - Saldo: R${self._saldo:.2f}"
    
    def __repr__(self) -> str:
        """Representação oficial da conta."""
        return f"ContaCorrente(titular={self.titular!r}, saldo={self._saldo})"

# Exemplo de uso com type hints
def processar_contas(contas: List[ContaCorrente]) -> Dict[str, float]:
    """
    Processa uma lista de contas e retorna saldos.
    
    Args:
        contas: Lista de contas a processar
    
    Returns:
        Dicionário com nome do titular e saldo
    """
    resultado = {}
    for conta in contas:
        resultado[conta.titular] = conta.saldo_disponivel
    return resultado

# Testando
if __name__ == "__main__":
    conta1 = ContaCorrente("João Silva", 1000.0)
    conta2 = ContaCorrente("Maria Santos", 500.0)
    
    conta1.depositar(500.0, "Salário")
    conta1.sacar(200.0, "Pagamento conta")
    conta1.transferir(300.0, conta2, "Pagamento")
    
    print(conta1.extrato())
    print(conta2)
```

---

## 🚀 Exemplo Final: Sistema Completo

```python
"""
Sistema de Gerenciamento de Biblioteca
Demonstração de vários conceitos de POO
"""

from abc import ABC, abstractmethod
from typing import List, Optional, Dict
from datetime import datetime, timedelta
import uuid

class ItemBiblioteca(ABC):
    """Classe abstrata base para todos os itens da biblioteca."""
    
    def __init__(self, titulo: str, autor: str, ano: int):
        self.id = str(uuid.uuid4())[:8]  # ID único reduzido
        self.titulo = titulo
        self.autor = autor
        self.ano = ano
        self.disponivel = True
        self.data_emprestimo: Optional[datetime] = None
        self.membro_emprestimo: Optional[str] = None
    
    @abstractmethod
    def get_tipo(self) -> str:
        """Retorna o tipo do item."""
        pass
    
    @abstractmethod
    def get_info_detalhada(self) -> str:
        """Retorna informações detalhadas do item."""
        pass
    
    def emprestar(self, membro: str) -> bool:
        """Empresta o item para um membro."""
        if self.disponivel:
            self.disponivel = False
            self.data_emprestimo = datetime.now()
            self.membro_emprestimo = membro
            return True
        return False
    
    def devolver(self) -> bool:
        """Devolve o item à biblioteca."""
        if not self.disponivel:
            self.disponivel = True
            self.data_emprestimo = None
            self.membro_emprestimo = None
            return True
        return False
    
    def dias_emprestado(self) -> Optional[int]:
        """Retorna quantos dias o item está emprestado."""
        if self.data_emprestimo:
            return (datetime.now() - self.data_emprestimo).days
        return None
    
    def __str__(self) -> str:
        status = "Disponível" if self.disponivel else f"Emprestado para {self.membro_emprestimo}"
        return f"{self.get_tipo()}: {self.titulo} - {self.autor} ({self.ano}) [{status}]"

class Livro(ItemBiblioteca):
    def __init__(self, titulo: str, autor: str, ano: int, 
                 isbn: str, paginas: int, genero: str):
        super().__init__(titulo, autor, ano)
        self.isbn = isbn
        self.paginas = paginas
        self.genero = genero
    
    def get_tipo(self) -> str:
        return "Livro"
    
    def get_info_detalhada(self) -> str:
        info = f"ISBN: {self.isbn}\n"
        info += f"Páginas: {self.paginas}\n"
        info += f"Gênero: {self.genero}\n"
        info += f"Status: {'Disponível' if self.disponivel else 'Emprestado'}"
        return info

class Revista(ItemBiblioteca):
    def __init__(self, titulo: str, editora: str, ano: int, 
                 edicao: int, mes: str):
        super().__init__(titulo, editora, ano)
        self.edicao = edicao
        self.mes = mes
    
    def get_tipo(self) -> str:
        return "Revista"
    
    def get_info_detalhada(self) -> str:
        return f"Edição: {self.edicao}\nMês: {self.mes}\nEditora: {self.autor}"

class DVD(ItemBiblioteca):
    def __init__(self, titulo: str, diretor: str, ano: int, 
                 duracao: int, classificacao: str):
        super().__init__(titulo, diretor, ano)
        self.duracao = duracao  # em minutos
        self.classificacao = classificacao
    
    def get_tipo(self) -> str:
        return "DVD"
    
    def get_info_detalhada(self) -> str:
        horas = self.duracao // 60
        minutos = self.duracao % 60
        return f"Duração: {horas}h{minutos}min\nClassificação: {self.classificacao}"

class Membro:
    def __init__(self, nome: str, email: str, tipo: str = "Regular"):
        self.id = str(uuid.uuid4())[:8]
        self.nome = nome
        self.email = email
        self.tipo = tipo  # Regular, VIP, Estudante
        self.data_cadastro = datetime.now()
        self.itens_emprestados: List[ItemBiblioteca] = []
        self.historico: List[Dict] = []
    
    def pode_pegar_emprestado(self) -> bool:
        """Verifica se o membro pode pegar mais itens."""
        limites = {
            "Regular": 3,
            "Estudante": 5,
            "VIP": 10
        }
        return len(self.itens_emprestados) < limites.get(self.tipo, 3)
    
    def pegar_emprestado(self, item: ItemBiblioteca) -> bool:
        """Pega um item emprestado."""
        if not self.pode_pegar_emprestado():
            print(f"{self.nome} já atingiu o limite de empréstimos")
            return False
        
        if item.emprestar(self.nome):
            self.itens_emprestados.append(item)
            self.historico.append({
                "data": datetime.now(),
                "acao": "Empréstimo",
                "item": item.titulo,
                "id_item": item.id
            })
            print(f"{item.titulo} emprestado para {self.nome}")
            return True
        return False
    
    def devolver_item(self, item: ItemBiblioteca) -> bool:
        """Devolve um item emprestado."""
        if item in self.itens_emprestados and item.devolver():
            self.itens_emprestados.remove(item)
            self.historico.append({
                "data": datetime.now(),
                "acao": "Devolução",
                "item": item.titulo,
                "id_item": item.id
            })
            
            # Verificar multa
            dias = item.dias_emprestado()
            if dias and dias > 14:  # Limite de 14 dias
                multa = (dias - 14) * 1.0  # R$1 por dia de atraso
                print(f"Atenção: Multa de R${multa:.2f} por atraso de {dias-14} dias")
            
            print(f"{item.titulo} devolvido por {self.nome}")
            return True
        return False
    
    def get_status(self) -> str:
        """Retorna status do membro."""
        return f"{self.nome} ({self.tipo}) - Itens emprestados: {len(self.itens_emprestados)}"
    
    def __str__(self) -> str:
        return f"Membro: {self.nome} - {self.email} - Tipo: {self.tipo}"

class Biblioteca:
    def __init__(self, nome: str):
        self.nome = nome
        self.itens: Dict[str, ItemBiblioteca] = {}
        self.membros: Dict[str, Membro] = {}
        self.emprestimos_ativos: List[Dict] = []
    
    def adicionar_item(self, item: ItemBiblioteca) -> None:
        """Adiciona um item à biblioteca."""
        self.itens[item.id] = item
        print(f"Item adicionado: {item.titulo} (ID: {item.id})")
    
    def remover_item(self, item_id: str) -> bool:
        """Remove um item da biblioteca."""
        if item_id in self.itens:
            item = self.itens.pop(item_id)
            print(f"Item removido: {item.titulo}")
            return True
        return False
    
    def cadastrar_membro(self, nome: str, email: str, tipo: str = "Regular") -> Membro:
        """Cadastra um novo membro."""
        membro = Membro(nome, email, tipo)
        self.membros[membro.id] = membro
        print(f"Membro cadastrado: {nome} (ID: {membro.id})")
        return membro
    
    def buscar_item(self, termo: str, por: str = "titulo") -> List[ItemBiblioteca]:
        """Busca itens por título, autor ou ID."""
        resultados = []
        termo = termo.lower()
        
        for item in self.itens.values():
            if por == "titulo" and termo in item.titulo.lower():
                resultados.append(item)
            elif por == "autor" and termo in item.autor.lower():
                resultados.append(item)
            elif por == "id" and termo in item.id.lower():
                resultados.append(item)
        
        return resultados
    
    def emprestar_item(self, item_id: str, membro_id: str) -> bool:
        """Processa empréstimo de item."""
        if item_id not in self.itens:
            print(f"Item {item_id} não encontrado")
            return False
        
        if membro_id not in self.membros:
            print(f"Membro {membro_id} não encontrado")
            return False
        
        item = self.itens[item_id]
        membro = self.membros[membro_id]
        
        return membro.pegar_emprestado(item)
    
    def gerar_relatorio(self) -> str:
        """Gera relatório da biblioteca."""
        relatorio = f"=== Relatório da Biblioteca {self.nome} ===\n\n"
        
        # Estatísticas
        total_itens = len(self.itens)
        itens_disponiveis = sum(1 for item in self.itens.values() if item.disponivel)
        itens_emprestados = total_itens - itens_disponiveis
        
        relatorio += f"Total de itens: {total_itens}\n"
        relatorio += f"Itens disponíveis: {itens_disponiveis}\n"
        relatorio += f"Itens emprestados: {itens_emprestados}\n"
        relatorio += f"Total de membros: {len(self.membros)}\n\n"
        
        # Itens mais emprestados (simulação)
        relatorio += "Itens atualmente emprestados:\n"
        for item in self.itens.values():
            if not item.disponivel:
                dias = item.dias_emprestado() or 0
                relatorio += f"  - {item.titulo} emprestado há {dias} dias\n"
        
        return relatorio
    
    def __str__(self) -> str:
        return f"Biblioteca {self.nome}: {len(self.itens)} itens, {len(self.membros)} membros"

# Demonstração do sistema
def demonstrar_sistema():
    """Demonstra o uso do sistema de biblioteca."""
    
    # Criar biblioteca
    biblioteca = Biblioteca("Central de Conhecimento")
    
    # Adicionar itens
    livro1 = Livro("Python para Iniciantes", "João Silva", 2023, 
                  "978-85-12345-67-8", 300, "Programação")
    
    livro2 = Livro("Machine Learning Avançado", "Maria Santos", 2022,
                  "978-85-87654-32-1", 500, "Ciência de Dados")
    
    revista1 = Revista("Revista de Tecnologia", "Editora Tech", 2023, 
                      45, "Outubro")
    
    dvd1 = DVD("O Poder do Python", "Carlos Eduardo", 2022, 
              120, "Livre")
    
    biblioteca.adicionar_item(livro1)
    biblioteca.adicionar_item(livro2)
    biblioteca.adicionar_item(revista1)
    biblioteca.adicionar_item(dvd1)
    
    # Cadastrar membros
    membro1 = biblioteca.cadastrar_membro("Ana Costa", "ana@email.com", "VIP")
    membro2 = biblioteca.cadastrar_membro("Pedro Alves", "pedro@email.com", "Estudante")
    
    # Empréstimos
    print("\n=== Realizando Empréstimos ===")
    biblioteca.emprestar_item(livro1.id, membro1.id)
    biblioteca.emprestar_item(livro2.id, membro1.id)
    biblioteca.emprestar_item(revista1.id, membro2.id)
    
    # Buscar itens
    print("\n=== Buscando Itens ===")
    resultados = biblioteca.buscar_item("Python", "titulo")
    for item in resultados:
        print(f"Encontrado: {item}")
    
    # Status dos membros
    print("\n=== Status dos Membros ===")
    for membro in biblioteca.membros.values():
        print(membro.get_status())
    
    # Relatório
    print("\n=== Relatório da Biblioteca ===")
    print(biblioteca.gerar_relatorio())
    
    # Devoluções
    print("\n=== Realizando Devoluções ===")
    membro1.devolver_item(livro1)
    
    # Relatório final
    print("\n=== Relatório Final ===")
    print(biblioteca.gerar_relatorio())

if __name__ == "__main__":
    demonstrar_sistema()
```

---

## 📖 Conclusão

Este manual cobriu os principais conceitos de Programação Orientada a Objetos em Python:

1. **Classes e Objetos**: Fundamentos da criação de classes e instâncias
2. **Métodos Especiais**: Utilização de dunder methods para comportamentos personalizados
3. **Encapsulamento**: Controle de acesso a atributos e métodos
4. **Herança**: Criação de hierarquias de classes
5. **Polimorfismo**: Uso de interfaces comuns para diferentes tipos
6. **Classes Abstratas**: Definição de contratos para subclasses
7. **Propriedades**: Uso de getters, setters e properties
8. **Métodos de Classe e Estáticos**: Diferenças e aplicações
9. **Mixins e MRO**: Composição de funcionalidades
10. **Padrões de Projeto**: Implementações básicas de padrões comuns
11. **Boas Práticas**: SOLID, type hints e documentação

### 🚀 Próximos Passos

Para continuar seu aprendizado:

1. **Estude bibliotecas padrão** que usam POO intensivamente:
   - `collections` (namedtuple, defaultdict, Counter)
   - `datetime` (date, datetime, timedelta)
   - `pathlib` (Path)

2. **Explore frameworks** que seguem padrões OO:
   - Django (ORM, models, views)
   - SQLAlchemy (models, session)
   - Pydantic (validação de dados)

3. **Pratique** implementando projetos reais:
   - Sistema de e-commerce
   - Jogo com múltiplas entidades
   - API REST com classes

4. **Aprofunde** em tópicos avançados:
   - Metaclasses
   - Descritores (descriptors)
   - ABC (Abstract Base Classes) do módulo `abc`
   - Protocolos (PEP 544)

### 📚 Recursos Adicionais

- [Documentação oficial do Python](https://docs.python.org/3/tutorial/classes.html)
- [Fluent Python](https://www.oreilly.com/library/view/fluent-python/9781491946237/) - Livro avançado
- [Python 3 Object-Oriented Programming](https://www.packtpub.com/product/python-3-object-oriented-programming-third-edition/9781789615852)
- [Real Python](https://realpython.com/python3-object-oriented-programming/) - Tutoriais práticos

Lembre-se: a melhor maneira de aprender POO é praticando. Comece com problemas simples e gradualmente aumente a complexidade dos seus projetos.
