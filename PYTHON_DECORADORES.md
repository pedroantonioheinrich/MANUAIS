# Manual Completo de Decorators em Python 🐍

## Sumário
1. [Introdução aos Decorators](#introdução)
2. [Fundamentos: Funções como Objetos de Primeira Classe](#fundamentos)
3. [Funções Aninhadas e Closures](#closures)
4. [Sintaxe Básica de Decorators](#sintaxe-basica)
5. [Decorators com Argumentos](#decorators-argumentos)
6. [Functools.wraps e Boas Práticas](#functools-wraps)
7. [Decorators de Classe](#decorators-classe)
8. [Aplicações Práticas na Vida Real](#aplicacoes-praticas)
9. [Exemplos Avançados](#exemplos-avancados)
10. [Exercícios Práticos](#exercicios)

---

## 1. Introdução aos Decorators {#introducao}

### O que são Decorators?

Decorators são uma das funcionalidades mais poderosas e elegantes do Python. Em essência, um decorator é uma função que modifica o comportamento de outra função ou método, sem alterar permanentemente sua definição.

**Analogia do Mundo Real:**
Imagine que você tem uma camiseta simples (função original). Um decorator seria como adicionar um bordado, um estampa ou um patch nela - você não muda a camiseta em si, mas adiciona funcionalidades extras sem alterar sua estrutura básica.

### Por que usar Decorators?

- **Reutilização de código**: Evita repetição de lógica em múltiplas funções
- **Separação de responsabilidades**: Isola preocupações transversais (logging, timing, validação)
- **Código mais limpo**: Mantém as funções focadas em sua responsabilidade principal
- **Manutenibilidade**: Facilita alterações em comportamentos comuns

---

## 2. Fundamentos: Funções como Objetos de Primeira Classe {#fundamentos}

Em Python, funções são "cidadãos de primeira classe", o que significa que podem ser tratadas como qualquer outro objeto.

### Exemplo 1: Atribuindo funções a variáveis

```python
def saudacao(nome):
    return f"Olá, {nome}!"

# Atribuindo a função a uma variável
minha_funcao = saudacao
print(minha_funcao("Maria"))  # Saída: Olá, Maria!
```

### Exemplo 2: Funções como argumentos

```python
def aplicar_operacao(func, valor):
    return func(valor)

def dobrar(x):
    return x * 2

def triplicar(x):
    return x * 3

print(aplicar_operacao(dobrar, 5))    # Saída: 10
print(aplicar_operacao(triplicar, 5))  # Saída: 15
```

### Exemplo 3: Retornando funções

```python
def criar_multiplicador(fator):
    def multiplicar(x):
        return x * fator
    return multiplicar

dobrar = criar_multiplicador(2)
triplicar = criar_multiplicador(3)

print(dobrar(10))    # Saída: 20
print(triplicar(10))  # Saída: 30
```

---

## 3. Funções Aninhadas e Closures {#closures}

### Funções Aninhadas (Inner Functions)

```python
def funcao_externa():
    print("Executando função externa")
    
    def funcao_interna():
        print("Executando função interna")
    
    funcao_interna()

funcao_externa()
# Saída:
# Executando função externa
# Executando função interna
```

### Closures

Uma closure ocorre quando uma função interna "lembra" do ambiente onde foi criada, mesmo após a função externa ter terminado.

```python
def saudacao_personalizada(saudacao):
    def saudar(nome):
        return f"{saudacao}, {nome}!"
    return saudar

saudacao_ola = saudacao_personalizada("Olá")
saudacao_bom_dia = saudacao_personalizada("Bom dia")

print(saudacao_ola("João"))      # Saída: Olá, João!
print(saudacao_bom_dia("Maria"))  # Saída: Bom dia, Maria!
```

**Como funciona:**
1. `saudacao_personalizada` recebe um parâmetro `saudacao`
2. Define uma função interna `saudar` que usa esse parâmetro
3. Retorna a função interna
4. A função interna "lembra" do valor de `saudacao` mesmo após `saudacao_personalizada` ter terminado

---

## 4. Sintaxe Básica de Decorators {#sintaxe-basica}

### Estrutura Básica de um Decorator

```python
def meu_decorator(func):
    def wrapper():
        print("Antes da função")
        func()
        print("Depois da função")
    return wrapper

@meu_decorator
def dizer_oi():
    print("Oi!")

dizer_oi()
# Saída:
# Antes da função
# Oi!
# Depois da função
```

### O que acontece por baixo dos panos?

```python
# Quando você escreve:
@meu_decorator
def dizer_oi():
    print("Oi!")

# É equivalente a:
def dizer_oi():
    print("Oi!")

dizer_oi = meu_decorator(dizer_oi)
```

### Decorators com Funções que Recebem Argumentos

```python
def decorator_com_argumentos(func):
    def wrapper(*args, **kwargs):
        print(f"Chamando função com args: {args}, kwargs: {kwargs}")
        resultado = func(*args, **kwargs)
        print(f"Função retornou: {resultado}")
        return resultado
    return wrapper

@decorator_com_argumentos
def somar(a, b):
    return a + b

@decorator_com_argumentos
def saudacao(nome, saudacao="Olá"):
    return f"{saudacao}, {nome}!"

# Testando
print(somar(5, 3))
# Saída:
# Chamando função com args: (5, 3), kwargs: {}
# Função retornou: 8
# 8

print(saudacao("João", saudacao="Bom dia"))
# Saída:
# Chamando função com args: ('João',), kwargs: {'saudacao': 'Bom dia'}
# Função retornou: Bom dia, João!
# Bom dia, João!
```

---

## 5. Decorators com Argumentos {#decorators-argumentos}

Às vezes precisamos passar argumentos para o próprio decorator. Isso requer três níveis de funções aninhadas.

### Estrutura de Decorator com Argumentos

```python
def decorator_com_parametros(param1, param2):
    def decorator_real(func):
        def wrapper(*args, **kwargs):
            print(f"Parâmetros do decorator: {param1}, {param2}")
            print("Antes da função")
            resultado = func(*args, **kwargs)
            print("Depois da função")
            return resultado
        return wrapper
    return decorator_real

@decorator_com_parametros("valor1", "valor2")
def minha_funcao():
    print("Executando função")

minha_funcao()
# Saída:
# Parâmetros do decorator: valor1, valor2
# Antes da função
# Executando função
# Depois da função
```

### Exemplo Prático: Repetir Execução

```python
def repetir(vezes=2):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(vezes):
                print(f"Execução {i+1}:")
                resultado = func(*args, **kwargs)
                print("-" * 20)
            return resultado
        return wrapper
    return decorator

@repetir(vezes=3)
def saudacao(nome):
    print(f"Olá, {nome}!")
    return f"Saudação enviada para {nome}"

resultado = saudacao("João")
print(f"Resultado final: {resultado}")
# Saída:
# Execução 1:
# Olá, João!
# --------------------
# Execução 2:
# Olá, João!
# --------------------
# Execução 3:
# Olá, João!
# --------------------
# Resultado final: Saudação enviada para João
```

---

## 6. Functools.wraps e Boas Práticas {#functools-wraps}

### O Problema com Decorators Simples

Quando usamos decorators, perdemos informações importantes da função original:

```python
def meu_decorator(func):
    def wrapper(*args, **kwargs):
        """Wrapper interna"""
        return func(*args, **kwargs)
    return wrapper

@meu_decorator
def dizer_oi():
    """Diz olá para alguém"""
    print("Oi!")

print(dizer_oi.__name__)  # Saída: wrapper (deveria ser 'dizer_oi')
print(dizer_oi.__doc__)   # Saída: Wrapper interna (deveria ser 'Diz olá para alguém')
```

### Solução com functools.wraps

```python
from functools import wraps

def meu_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        """Wrapper interna"""
        print("Antes da função")
        resultado = func(*args, **kwargs)
        print("Depois da função")
        return resultado
    return wrapper

@meu_decorator
def dizer_oi():
    """Diz olá para alguém"""
    print("Oi!")

print(dizer_oi.__name__)  # Saída: dizer_oi
print(dizer_oi.__doc__)   # Saída: Diz olá para alguém
```

**Por que usar @wraps é importante?**
- Preserva metadados da função original
- Mantém a assinatura da função
- Essencial para debugging e documentação
- Permite que ferramentas como help() funcionem corretamente

---

## 7. Decorators de Classe {#decorators-classe}

### Decorators Podem Ser Classes

```python
from functools import wraps

class ContadorChamadas:
    def __init__(self, func):
        wraps(func)(self)
        self.func = func
        self.contador = 0
    
    def __call__(self, *args, **kwargs):
        self.contador += 1
        print(f"Função {self.func.__name__} chamada {self.contador} vezes")
        return self.func(*args, **kwargs)

@ContadorChamadas
def saudacao(nome):
    print(f"Olá, {nome}!")

saudacao("João")
saudacao("Maria")
saudacao("Pedro")
# Saída:
# Função saudacao chamada 1 vezes
# Olá, João!
# Função saudacao chamada 2 vezes
# Olá, Maria!
# Função saudacao chamada 3 vezes
# Olá, Pedro!
```

### Decorators de Classe com Parâmetros

```python
class Temporizador:
    def __init__(self, unidade="segundos"):
        self.unidade = unidade
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            import time
            inicio = time.time()
            resultado = func(*args, **kwargs)
            fim = time.time()
            duracao = fim - inicio
            print(f"Tempo de execução: {duracao:.4f} {self.unidade}")
            return resultado
        return wrapper

@Temporizador(unidade="segundos")
def funcao_demorada():
    import time
    time.sleep(1)
    print("Função concluída")

funcao_demorada()
# Saída:
# Função concluída
# Tempo de execução: 1.0012 segundos
```

---

## 8. Aplicações Práticas na Vida Real {#aplicacoes-praticas}

### 1. Decorator de Logging (Registro de Atividades)

```python
import logging
from functools import wraps
from datetime import datetime

# Configuração básica de logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def log_atividade(nivel=logging.INFO):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Log antes da execução
            logging.log(nivel, f"Iniciando {func.__name__} - Args: {args}, Kwargs: {kwargs}")
            
            try:
                resultado = func(*args, **kwargs)
                # Log após execução bem-sucedida
                logging.log(nivel, f"Finalizado {func.__name__} - Resultado: {resultado}")
                return resultado
            except Exception as e:
                # Log de erro
                logging.error(f"Erro em {func.__name__}: {str(e)}")
                raise
        return wrapper
    return decorator

# Aplicação prática
@log_atividade()
def processar_pagamento(usuario_id, valor):
    """Processa pagamento de um usuário"""
    print(f"Processando pagamento de R${valor} para usuário {usuario_id}")
    return {"status": "sucesso", "id_transacao": "12345"}

@log_atividade(nivel=logging.ERROR)
def dividir(a, b):
    return a / b

# Testando
processar_pagamento(123, 150.50)
# Logs serão gerados automaticamente
```

### 2. Decorator de Autenticação e Autorização

```python
from functools import wraps
from enum import Enum

class NivelAcesso(Enum):
    ADMIN = "admin"
    USUARIO = "usuario"
    VISITANTE = "visitante"

# Simulação de banco de dados de usuários
USUARIOS = {
    "joao": {"senha": "123", "nivel": NivelAcesso.ADMIN},
    "maria": {"senha": "456", "nivel": NivelAcesso.USUARIO},
    "pedro": {"senha": "789", "nivel": NivelAcesso.VISITANTE}
}

def autenticar(func):
    @wraps(func)
    def wrapper(usuario, *args, **kwargs):
        if usuario not in USUARIOS:
            raise PermissionError("Usuário não autenticado")
        return func(usuario, *args, **kwargs)
    return wrapper

def autorizar(niveis_permitidos):
    def decorator(func):
        @wraps(func)
        def wrapper(usuario, *args, **kwargs):
            nivel_usuario = USUARIOS.get(usuario, {}).get("nivel")
            
            if nivel_usuario not in niveis_permitidos:
                raise PermissionError(
                    f"Usuário {usuario} não tem permissão para esta ação. "
                    f"Nível necessário: {[n.value for n in niveis_permitidos]}"
                )
            
            return func(usuario, *args, **kwargs)
        return wrapper
    return decorator

@autenticar
@autorizar([NivelAcesso.ADMIN, NivelAcesso.USUARIO])
def criar_postagem(usuario, titulo, conteudo):
    """Cria uma nova postagem (apenas ADMIN e USUARIO)"""
    print(f"Postagem criada por {usuario}: {titulo}")
    return {"id": 1, "titulo": titulo}

@autenticar
@autorizar([NivelAcesso.ADMIN])
def excluir_usuario(usuario_admin, usuario_alvo):
    """Exclui um usuário (apenas ADMIN)"""
    print(f"Usuário {usuario_alvo} excluído por {usuario_admin}")
    return True

# Testando
try:
    criar_postagem("joao", "Python Rocks", "Conteúdo sobre Python")
    criar_postagem("maria", "Dicas de Programação", "Conteúdo")
    
    # Isso deve falhar (visitante não pode criar postagem)
    criar_postagem("pedro", "Tentativa", "Inválida")
except PermissionError as e:
    print(f"Erro: {e}")

try:
    # Isso deve funcionar (joão é admin)
    excluir_usuario("joao", "pedro")
    
    # Isso deve falhar (maria não é admin)
    excluir_usuario("maria", "joao")
except PermissionError as e:
    print(f"Erro: {e}")
```

### 3. Decorator de Cache/Memoização

```python
from functools import wraps
import time

def cache(duracao_segundos=None):
    """
    Decorator que cacheia resultados de funções
    
    Args:
        duracao_segundos: Tempo de vida do cache em segundos.
                          Se None, cacheia permanentemente.
    """
    def decorator(func):
        cache_data = {}
        
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Criar chave única baseada nos argumentos
            chave = str(args) + str(sorted(kwargs.items()))
            
            # Verificar se resultado está em cache e é válido
            if chave in cache_data:
                resultado, timestamp = cache_data[chave]
                
                # Verificar se cache expirou
                if duracao_segundos is None:
                    print(f"Cache HIT para {func.__name__}")
                    return resultado
                elif time.time() - timestamp < duracao_segundos:
                    print(f"Cache HIT para {func.__name__}")
                    return resultado
            
            # Calcular resultado
            print(f"Cache MISS para {func.__name__}")
            resultado = func(*args, **kwargs)
            
            # Armazenar em cache
            cache_data[chave] = (resultado, time.time())
            return resultado
        return wrapper
    return decorator

# Aplicação prática: Consulta a banco de dados simulada
@cache(duracao_segundos=5)
def consultar_usuario(user_id):
    """Simula consulta ao banco de dados"""
    print(f"Consultando banco de dados para usuário {user_id}...")
    time.sleep(1)  # Simula latência
    return {
        "id": user_id,
        "nome": f"Usuário {user_id}",
        "email": f"user{user_id}@email.com"
    }

@cache()  # Cache permanente
def calcular_fatorial(n):
    """Calcula fatorial de forma recursiva"""
    if n <= 1:
        return 1
    return n * calcular_fatorial(n - 1)

# Testando cache temporário
print("Primeira consulta - deve ir ao banco")
print(consultar_usuario(1))
print("\nSegunda consulta - deve vir do cache")
print(consultar_usuario(1))
print("\nAguardando cache expirar...")
time.sleep(5)
print("\nTerceira consulta - cache expirado, deve ir ao banco")
print(consultar_usuario(1))

# Testando cache permanente
print("\n" + "="*50)
print("Calculando fatorial de 5 pela primeira vez")
print(f"Resultado: {calcular_fatorial(5)}")
print("\nCalculando fatorial de 5 novamente (deve vir do cache)")
print(f"Resultado: {calcular_fatorial(5)}")
```

### 4. Decorator de Validação de Dados

```python
from functools import wraps
from typing import Any, Dict, List, Union
import re

class ValidationError(Exception):
    pass

def validar_entrada(regras: Dict[str, Dict[str, Any]]):
    """
    Decorator para validar argumentos de função
    
    Args:
        regras: Dicionário com regras de validação para cada parâmetro
    """
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Combinar args posicionais com kwargs
            todos_args = func.__code__.co_varnames[:func.__code__.co_argcount]
            args_dict = dict(zip(todos_args[:len(args)], args))
            args_dict.update(kwargs)
            
            # Validar cada argumento conforme regras
            for param, regra in regras.items():
                if param not in args_dict:
                    if regra.get('required', True):
                        raise ValidationError(f"Parâmetro obrigatório '{param}' não fornecido")
                    continue
                
                valor = args_dict[param]
                
                # Validar tipo
                if 'tipo' in regra and not isinstance(valor, regra['tipo']):
                    raise ValidationError(
                        f"Parâmetro '{param}' deve ser do tipo {regra['tipo'].__name__}"
                    )
                
                # Validar mínimo (para números)
                if 'min' in regra and valor < regra['min']:
                    raise ValidationError(
                        f"Parâmetro '{param}' deve ser >= {regra['min']}"
                    )
                
                # Validar máximo (para números)
                if 'max' in regra and valor > regra['max']:
                    raise ValidationError(
                        f"Parâmetro '{param}' deve ser <= {regra['max']}"
                    )
                
                # Validar padrão regex (para strings)
                if 'pattern' in regra and not re.match(regra['pattern'], str(valor)):
                    raise ValidationError(
                        f"Parâmetro '{param}' não corresponde ao padrão {regra['pattern']}"
                    )
                
                # Validar opções permitidas
                if 'opcoes' in regra and valor not in regra['opcoes']:
                    raise ValidationError(
                        f"Parâmetro '{param}' deve ser um de: {regra['opcoes']}"
                    )
            
            return func(*args, **kwargs)
        return wrapper
    return decorator

# Aplicação prática: Cadastro de usuário
@validar_entrada({
    'nome': {
        'tipo': str,
        'pattern': r'^[A-Za-zÀ-ÖØ-öø-ÿ\s]{2,50}$',
        'required': True
    },
    'idade': {
        'tipo': int,
        'min': 18,
        'max': 120,
        'required': True
    },
    'email': {
        'tipo': str,
        'pattern': r'^[\w\.-]+@[\w\.-]+\.\w+$',
        'required': True
    },
    'tipo_usuario': {
        'tipo': str,
        'opcoes': ['admin', 'usuario', 'visitante'],
        'required': False
    }
})
def cadastrar_usuario(nome, idade, email, tipo_usuario='usuario'):
    """Cadastra um novo usuário no sistema"""
    print(f"Usuário cadastrado com sucesso:")
    print(f"  Nome: {nome}")
    print(f"  Idade: {idade}")
    print(f"  Email: {email}")
    print(f"  Tipo: {tipo_usuario}")
    return {"id": 123, "nome": nome, "email": email}

# Testando validações
try:
    cadastrar_usuario("João Silva", 25, "joao@email.com", "admin")
    print("✓ Cadastro bem-sucedido\n")
except ValidationError as e:
    print(f"Erro: {e}\n")

try:
    cadastrar_usuario("A", 25, "joao@email.com")  # Nome muito curto
except ValidationError as e:
    print(f"Erro esperado: {e}\n")

try:
    cadastrar_usuario("João Silva", 15, "joao@email.com")  # Idade muito baixa
except ValidationError as e:
    print(f"Erro esperado: {e}\n")

try:
    cadastrar_usuario("João Silva", 25, "email-invalido")  # Email inválido
except ValidationError as e:
    print(f"Erro esperado: {e}\n")
```

### 5. Decorator de Retry (Tentativas Automáticas)

```python
from functools import wraps
import time
import random

def retry(max_tentativas=3, delay=1, backoff=2, exceptions=(Exception,)):
    """
    Decorator que tenta executar uma função novamente em caso de erro
    
    Args:
        max_tentativas: Número máximo de tentativas
        delay: Atraso inicial entre tentativas (segundos)
        backoff: Fator multiplicador para atraso exponencial
        exceptions: Tupla de exceções que devem ser tratadas
    """
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            tentativas = 0
            current_delay = delay
            
            while tentativas < max_tentativas:
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    tentativas += 1
                    if tentativas == max_tentativas:
                        print(f"Todas as {max_tentativas} tentativas falharam")
                        raise
                    
                    print(f"Tentativa {tentativas} falhou: {e}")
                    print(f"Aguardando {current_delay}s antes da próxima tentativa...")
                    time.sleep(current_delay)
                    current_delay *= backoff
            
            return None  # Nunca chega aqui
        return wrapper
    return decorator

# Aplicação prática: Chamada a API externa
class APIError(Exception):
    pass

class APITemporariamenteIndisponivel(APIError):
    pass

@retry(max_tentativas=3, delay=1, backoff=2, exceptions=(APITemporariamenteIndisponivel,))
def chamar_api_externa(endpoint, dados):
    """
    Simula chamada a uma API externa que pode falhar
    """
    print(f"Chamando API: {endpoint}")
    
    # Simular falha aleatória (30% de chance)
    if random.random() < 0.7:  # 70% de chance de falha
        raise APITemporariamenteIndisponivel("API sobrecarregada, tente novamente")
    
    print("API respondeu com sucesso!")
    return {"status": "sucesso", "dados": dados}

# Testando
for i in range(5):
    print(f"\n--- Teste {i+1} ---")
    try:
        resultado = chamar_api_externa("/usuarios", {"id": 123})
        print(f"Resultado: {resultado}")
    except APIError as e:
        print(f"Falha definitiva: {e}")
```

### 6. Decorator de Singleton (Padrão de Projeto)

```python
from functools import wraps

def singleton(cls):
    """
    Decorator que transforma uma classe em singleton
    (apenas uma instância pode existir)
    """
    instancias = {}
    
    @wraps(cls)
    def wrapper(*args, **kwargs):
        if cls not in instancias:
            instancias[cls] = cls(*args, **kwargs)
        return instancias[cls]
    
    return wrapper

@singleton
class Configuracao:
    def __init__(self):
        print("Criando nova instância de Configuração")
        self.configuracoes = {
            "host": "localhost",
            "porta": 5432,
            "debug": False
        }
    
    def get(self, chave):
        return self.configuracoes.get(chave)
    
    def set(self, chave, valor):
        self.configuracoes[chave] = valor

# Testando
print("Obtendo primeira instância")
config1 = Configuracao()
print(f"Host: {config1.get('host')}")

print("\nModificando configuração")
config1.set("host", "192.168.1.100")

print("\nObtendo segunda instância (deve ser a mesma)")
config2 = Configuracao()
print(f"Host da segunda instância: {config2.get('host')}")

print(f"\nconfig1 é config2? {config1 is config2}")  # True
```

### 7. Decorator de Rate Limiting (Limitação de Taxa)

```python
from functools import wraps
import time
from collections import deque

def rate_limit(max_chamadas, periodo):
    """
    Limita o número de chamadas a uma função em um período de tempo
    
    Args:
        max_chamadas: Número máximo de chamadas permitidas
        periodo: Período de tempo em segundos
    """
    def decorator(func):
        # Armazenar timestamps das chamadas recentes
        chamadas = deque()
        
        @wraps(func)
        def wrapper(*args, **kwargs):
            agora = time.time()
            
            # Remover chamadas antigas fora do período
            while chamadas and chamadas[0] < agora - periodo:
                chamadas.popleft()
            
            # Verificar se excedeu o limite
            if len(chamadas) >= max_chamadas:
                tempo_espera = chamadas[0] + periodo - agora
                raise Exception(
                    f"Limite de {max_chamadas} chamadas por {periodo}s excedido. "
                    f"Aguarde {tempo_espera:.1f}s"
                )
            
            # Registrar chamada atual
            chamadas.append(agora)
            return func(*args, **kwargs)
        
        return wrapper
    return decorator

# Aplicação prática: API com limite de requisições
@rate_limit(max_chamadas=3, periodo=10)
def consultar_api(consulta):
    print(f"Consultando API: {consulta}")
    return f"Resultado para '{consulta}'"

# Testando
print("Testando rate limit (máx 3 chamadas a cada 10 segundos)\n")

for i in range(5):
    try:
        resultado = consultar_api(f"consulta {i+1}")
        print(f"✓ {resultado}")
    except Exception as e:
        print(f"✗ Erro: {e}")
    
    if i < 4:
        print("Aguardando 2 segundos...")
        time.sleep(2)
```

---

## 9. Exemplos Avançados {#exemplos-avancados}

### 1. Sistema de Plugins com Decorators

```python
# sistema_plugins.py
class PluginManager:
    """Gerenciador de plugins usando decorators"""
    
    def __init__(self):
        self._plugins = {}
        self._hooks = {}
    
    def plugin(self, nome):
        """Decorator para registrar um plugin"""
        def decorator(cls):
            self._plugins[nome] = cls
            return cls
        return decorator
    
    def hook(self, nome_hook):
        """Decorator para registrar um hook em um plugin"""
        def decorator(func):
            if nome_hook not in self._hooks:
                self._hooks[nome_hook] = []
            self._hooks[nome_hook].append(func)
            return func
        return decorator
    
    def executar_hook(self, nome_hook, *args, **kwargs):
        """Executa todos os hooks registrados"""
        resultados = []
        for hook in self._hooks.get(nome_hook, []):
            resultados.append(hook(*args, **kwargs))
        return resultados
    
    def get_plugin(self, nome):
        return self._plugins.get(nome)

# Criar instância global do gerenciador
plugin_manager = PluginManager()

# plugins/processadores_texto.py
@plugin_manager.plugin("processador_texto")
class ProcessadorTexto:
    """Plugin para processamento de texto"""
    
    def __init__(self):
        self.nome = "Processador de Texto"
    
    @plugin_manager.hook("processar_texto")
    def processar(self, texto):
        return texto.upper()
    
    @plugin_manager.hook("estatisticas_texto")
    def estatisticas(self, texto):
        return {
            "caracteres": len(texto),
            "palavras": len(texto.split()),
            "linhas": len(texto.splitlines())
        }

# plugins/validador.py
@plugin_manager.plugin("validador")
class Validador:
    """Plugin para validação"""
    
    def __init__(self):
        self.nome = "Validador de Dados"
    
    @plugin_manager.hook("validar_email")
    def validar_email(self, email):
        import re
        pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
        return bool(re.match(pattern, email))
    
    @plugin_manager.hook("processar_texto")
    def sanitizar(self, texto):
        # Remove caracteres especiais
        import re
        return re.sub(r'[^a-zA-Z0-9\s]', '', texto)

# Usando o sistema de plugins
def main():
    # Carregar plugins (simulado)
    processador = plugin_manager.get_plugin("processador_texto")()
    validador = plugin_manager.get_plugin("validador")()
    
    # Executar hooks
    texto = "Olá, Mundo! Este é um teste."
    
    print("Processando texto:")
    resultados = plugin_manager.executar_hook("processar_texto", texto)
    for i, resultado in enumerate(resultados, 1):
        print(f"  Plugin {i}: {resultado}")
    
    print("\nEstatísticas do texto:")
    stats = plugin_manager.executar_hook("estatisticas_texto", texto)[0]
    for chave, valor in stats.items():
        print(f"  {chave}: {valor}")
    
    print("\nValidando email:")
    emails = ["teste@email.com", "invalido", "outro@teste"]
    for email in emails:
        resultados = plugin_manager.executar_hook("validar_email", email)
        valido = resultados[0] if resultados else False
        print(f"  {email}: {'✓' if valido else '✗'}")

if __name__ == "__main__":
    main()
```

### 2. Decorator com Contexto e Injeção de Dependências

```python
from functools import wraps
from contextvars import ContextVar
from typing import Any, Dict

# Contexto global para a aplicação
class AppContext:
    def __init__(self):
        self._data = {}
    
    def set(self, chave, valor):
        self._data[chave] = valor
    
    def get(self, chave, default=None):
        return self._data.get(chave, default)

# Criar contexto global
app_context = AppContext()

def inject(**dependencias):
    """
    Decorator que injeta dependências do contexto
    """
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Para cada dependência solicitada, buscar do contexto
            for nome_dep, chave_contexto in dependencias.items():
                if nome_dep not in kwargs:
                    valor = app_context.get(chave_contexto)
                    if valor is None:
                        raise ValueError(
                            f"Dependência '{nome_dep}' não encontrada no contexto"
                        )
                    kwargs[nome_dep] = valor
            
            return func(*args, **kwargs)
        return wrapper
    return decorator

# Configurar contexto
app_context.set("db", {"host": "localhost", "porta": 5432})
app_context.set("logger", print)
app_context.set("cache", {})

# Usando injeção de dependências
@inject(db_conexao="db", logger="logger")
def buscar_usuario(user_id, db_conexao=None, logger=None):
    """Busca usuário no banco de dados"""
    logger(f"Buscando usuário {user_id} no banco {db_conexao['host']}")
    return {"id": user_id, "nome": f"Usuário {user_id}"}

@inject(cache="cache", logger="logger")
def calcular_dado_complexo(valor, cache=None, logger=None):
    """Calcula dado complexo com cache"""
    if valor in cache:
        logger(f"Cache HIT para {valor}")
        return cache[valor]
    
    logger(f"Cache MISS para {valor}, calculando...")
    resultado = valor ** 2  # Simulação de cálculo complexo
    cache[valor] = resultado
    return resultado

# Testando
print(buscar_usuario(123))
print(calcular_dado_complexo(10))
print(calcular_dado_complexo(10))  # Deve vir do cache
```

---

## 10. Exercícios Práticos {#exercicios}

### Exercício 1: Decorator de Temporizador Avançado
Crie um decorator que meça o tempo de execução de funções e acumule estatísticas.

```python
from functools import wraps
import time

class Temporizador:
    """Decorator que acumula estatísticas de tempo"""
    
    estatisticas = {}
    
    def __init__(self, nome=None):
        self.nome = nome
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            inicio = time.time()
            resultado = func(*args, **kwargs)
            duracao = time.time() - inicio
            
            # Acumular estatísticas
            nome_func = self.nome or func.__name__
            if nome_func not in self.estatisticas:
                self.estatisticas[nome_func] = {
                    "chamadas": 0,
                    "tempo_total": 0,
                    "tempo_medio": 0,
                    "tempo_min": float('inf'),
                    "tempo_max": 0
                }
            
            stats = self.estatisticas[nome_func]
            stats["chamadas"] += 1
            stats["tempo_total"] += duracao
            stats["tempo_medio"] = stats["tempo_total"] / stats["chamadas"]
            stats["tempo_min"] = min(stats["tempo_min"], duracao)
            stats["tempo_max"] = max(stats["tempo_max"], duracao)
            
            return resultado
        return wrapper
    
    @classmethod
    def relatorio(cls):
        """Gera relatório de estatísticas"""
        print("\n" + "="*50)
        print("RELATÓRIO DE TEMPORIZAÇÃO")
        print("="*50)
        
        for nome, stats in cls.estatisticas.items():
            print(f"\nFunção: {nome}")
            print(f"  Chamadas: {stats['chamadas']}")
            print(f"  Tempo total: {stats['tempo_total']:.4f}s")
            print(f"  Tempo médio: {stats['tempo_medio']:.4f}s")
            print(f"  Tempo mínimo: {stats['tempo_min']:.4f}s")
            print(f"  Tempo máximo: {stats['tempo_max']:.4f}s")

# Teste seu decorator aqui
@Temporizador()
def funcao1():
    time.sleep(0.1)

@Temporizador("minha_funcao_especial")
def funcao2():
    time.sleep(0.2)

# Execute várias vezes e gere relatório
for _ in range(5):
    funcao1()
    funcao2()

Temporizador.relatorio()
```

### Exercício 2: Decorator de Validação de Tipos
Crie um decorator que valide os tipos dos argumentos em tempo de execução.

```python
from functools import wraps
from typing import get_type_hints

def valida_tipos(func):
    """
    Decorator que valida tipos dos argumentos baseado nas type hints
    """
    @wraps(func)
    def wrapper(*args, **kwargs):
        # Obter type hints da função
        type_hints = get_type_hints(func)
        
        # Combinar args posicionais com kwargs
        todos_args = func.__code__.co_varnames[:func.__code__.co_argcount]
        args_dict = dict(zip(todos_args[:len(args)], args))
        args_dict.update(kwargs)
        
        # Validar cada argumento
        for nome, valor in args_dict.items():
            tipo_esperado = type_hints.get(nome)
            if tipo_esperado and not isinstance(valor, tipo_esperado):
                raise TypeError(
                    f"Argumento '{nome}' deve ser {tipo_esperado.__name__}, "
                    f"recebeu {type(valor).__name__}"
                )
        
        resultado = func(*args, **kwargs)
        
        # Validar tipo de retorno
        tipo_retorno = type_hints.get('return')
        if tipo_retorno and not isinstance(resultado, tipo_retorno):
            raise TypeError(
                f"Retorno deve ser {tipo_retorno.__name__}, "
                f"recebeu {type(resultado).__name__}"
            )
        
        return resultado
    return wrapper

# Teste seu decorator
@valida_tipos
def processar_dados(nome: str, idade: int, ativo: bool = True) -> dict:
    """Processa dados do usuário"""
    return {"nome": nome, "idade": idade, "ativo": ativo}

# Testes
try:
    print(processar_dados("João", 25, True))  # Deve funcionar
    print(processar_dados(123, 25, True))     # Deve falhar (nome não é string)
except TypeError as e:
    print(f"Erro: {e}")
```

### Exercício 3: Sistema de Eventos com Decorators
Crie um sistema simples de eventos usando decorators.

```python
class EventEmitter:
    """Sistema de eventos com decorators"""
    
    def __init__(self):
        self._eventos = {}
    
    def on(self, evento_nome):
        """
        Decorator que registra uma função como listener de um evento
        """
        def decorator(func):
            if evento_nome not in self._eventos:
                self._eventos[evento_nome] = []
            self._eventos[evento_nome].append(func)
            return func
        return decorator
    
    def emit(self, evento_nome, *args, **kwargs):
        """
        Emite um evento, chamando todos os listeners registrados
        """
        resultados = []
        for listener in self._eventos.get(evento_nome, []):
            resultados.append(listener(*args, **kwargs))
        return resultados
    
    def once(self, evento_nome):
        """
        Decorator que registra um listener que será executado apenas uma vez
        """
        def decorator(func):
            def wrapper(*args, **kwargs):
                resultado = func(*args, **kwargs)
                # Remover listener após execução
                if evento_nome in self._eventos:
                    self._eventos[evento_nome].remove(wrapper)
                return resultado
            
            # Registrar wrapper
            if evento_nome not in self._eventos:
                self._eventos[evento_nome] = []
            self._eventos[evento_nome].append(wrapper)
            return wrapper
        return decorator

# Criar emissor de eventos
eventos = EventEmitter()

# Registrar listeners
@eventos.on("usuario.cadastrado")
def enviar_email_boas_vindas(usuario):
    print(f"📧 Enviando email de boas-vindas para {usuario}")
    return f"Email enviado para {usuario}"

@eventos.on("usuario.cadastrado")
def log_cadastro(usuario):
    print(f"📝 Registrando log: usuário {usuario} cadastrado")
    return f"Log registrado"

@eventos.once("sistema.iniciado")
def inicializar_banco():
    print("🔧 Inicializando banco de dados (apenas uma vez)")
    return "Banco inicializado"

# Testar sistema
print("=== Teste 1: Evento 'usuario.cadastrado' ===")
resultados = eventos.emit("usuario.cadastrado", "joao@email.com")
print(f"Resultados: {resultados}")

print("\n=== Teste 2: Evento 'sistema.iniciado' (primeira vez) ===")
resultados = eventos.emit("sistema.iniciado")
print(f"Resultados: {resultados}")

print("\n=== Teste 3: Evento 'sistema.iniciado' (segunda vez) ===")
resultados = eventos.emit("sistema.iniciado")
print(f"Resultados: {resultados}")  # Deve estar vazio
```

---

## Conclusão

### Quando Usar Decorators:

✅ **Use decorators quando:**
- Precisar adicionar funcionalidade comum a múltiplas funções
- Quiser separar preocupações transversais (logging, timing, validação)
- Estiver criando APIs ou frameworks
- Quiser implementar padrões como Singleton, Factory, etc.

❌ **Evite decorators quando:**
- A funcionalidade é muito específica de uma única função
- O decorator torna o código difícil de entender
- Há muitas camadas de decorators (pode complicar o debugging)
- A performance é crítica (decorators adicionam overhead)

### Boas Práticas:

1. **Sempre use @wraps** para preservar metadados
2. **Mantenha decorators simples e focados** (uma responsabilidade)
3. **Documente bem** o que o decorator faz
4. **Considere a ordem** dos decorators (são aplicados de baixo para cima)
5. **Teste decorators separadamente** das funções que decoram

### Padrões Comuns de Decorators:

- **Logging**: Registrar chamadas de função
- **Timing**: Medir performance
- **Caching**: Memorizar resultados
- **Retry**: Tentar novamente em caso de falha
- **Validation**: Validar argumentos
- **Authorization**: Controlar acesso
- **Rate limiting**: Limitar frequência de chamadas
- **Singleton**: Garantir instância única

---
