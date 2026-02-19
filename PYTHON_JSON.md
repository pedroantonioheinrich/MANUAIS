# Manual Completo da Biblioteca JSON em Python

## Índice
1. [Introdução ao JSON](#introdução)
2. [Importando a Biblioteca](#importando)
3. [Conversão Básica](#conversao-basica)
4. [Trabalhando com Arquivos](#arquivos)
5. [Personalizando a Serialização](#personalizacao)
6. [Tratamento de Erros](#erros)
7. [Exemplos Práticos](#exemplos)
8. [Dicas e Boas Práticas](#dicas)

---

## 1. Introdução ao JSON {#introducao}

JSON (JavaScript Object Notation) é um formato leve de troca de dados, fácil de ler e escrever para humanos, e fácil de interpretar e gerar para máquinas.

**Exemplo de JSON:**
```json
{
    "nome": "João Silva",
    "idade": 30,
    "cidade": "São Paulo",
    "habilidades": ["Python", "JavaScript"],
    "ativo": true,
    "saldo": null
}
```

---

## 2. Importando a Biblioteca {#importando}

```python
import json
```

---

## 3. Conversão Básica {#conversao-basica}

### 3.1 De Python para JSON (Serialização)

```python
import json

# Dicionário Python
pessoa = {
    "nome": "Maria",
    "idade": 25,
    "cidade": "Rio de Janeiro",
    "casada": False,
    "filhos": None,
    "hobbies": ["leitura", "natação"]
}

# Converter para string JSON
json_string = json.dumps(pessoa)
print(json_string)
# {"nome": "Maria", "idade": 25, "cidade": "Rio de Janeiro", "casada": false, "filhos": null, "hobbies": ["leitura", "natação"]}

# Com formatação bonita (pretty print)
json_bonito = json.dumps(pessoa, indent=4, sort_keys=True)
print(json_bonito)
```

### 3.2 De JSON para Python (Desserialização)

```python
import json

json_string = '{"nome": "Pedro", "idade": 32, "ativo": true, "saldo": null}'

# Converter string JSON para dicionário Python
dados = json.loads(json_string)
print(dados)
# {'nome': 'Pedro', 'idade': 32, 'ativo': True, 'saldo': None}

print(dados['nome'])  # Pedro
print(dados['ativo'])  # True
```

### 3.3 Tabela de Conversão de Tipos

| JSON | Python |
|------|--------|
| object | dict |
| array | list |
| string | str |
| number (int) | int |
| number (real) | float |
| true | True |
| false | False |
| null | None |

---

## 4. Trabalhando com Arquivos {#arquivos}

### 4.1 Escrevendo JSON em Arquivo

```python
import json

dados = {
    "empresa": "Tech Solutions",
    "funcionarios": [
        {"nome": "Ana", "cargo": "Dev"},
        {"nome": "Carlos", "cargo": "Gerente"}
    ],
    "ano_fundacao": 2015
}

# Escrever em arquivo
with open('empresa.json', 'w', encoding='utf-8') as arquivo:
    json.dump(dados, arquivo, indent=4, ensure_ascii=False)

# ensure_ascii=False permite caracteres especiais
```

### 4.2 Lendo JSON de Arquivo

```python
import json

# Ler do arquivo
with open('empresa.json', 'r', encoding='utf-8') as arquivo:
    dados_carregados = json.load(arquivo)

print(dados_carregados)
print(dados_carregados['funcionarios'][0]['nome'])  # Ana
```

### 4.3 Exemplo Completo com Arquivo

```python
import json

# Função para salvar dados
def salvar_dados(nome_arquivo, dados):
    try:
        with open(nome_arquivo, 'w', encoding='utf-8') as f:
            json.dump(dados, f, indent=4, ensure_ascii=False)
        print(f"Dados salvos em {nome_arquivo}")
    except Exception as e:
        print(f"Erro ao salvar: {e}")

# Função para carregar dados
def carregar_dados(nome_arquivo):
    try:
        with open(nome_arquivo, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        print(f"Arquivo {nome_arquivo} não encontrado")
        return {}
    except json.JSONDecodeError:
        print("Arquivo JSON inválido")
        return {}

# Uso
dados = {"chave": "valor", "numeros": [1, 2, 3]}
salvar_dados("meus_dados.json", dados)
dados_recuperados = carregar_dados("meus_dados.json")
print(dados_recuperados)
```

---

## 5. Personalizando a Serialização {#personalizacao}

### 5.1 Serializando Classes Personalizadas

```python
import json
from datetime import datetime

class Pessoa:
    def __init__(self, nome, idade, data_cadastro):
        self.nome = nome
        self.idade = idade
        self.data_cadastro = data_cadastro

# Método 1: Função personalizada
def serializar_pessoa(obj):
    if isinstance(obj, Pessoa):
        return {
            "nome": obj.nome,
            "idade": obj.idade,
            "data_cadastro": obj.data_cadastro.isoformat(),
            "tipo": "Pessoa"
        }
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError(f"Tipo não serializável: {type(obj)}")

# Criando objeto
p = Pessoa("Joana", 28, datetime.now())

# Serializando
json_str = json.dumps(p, default=serializar_pessoa, indent=2)
print(json_str)
```

### 5.2 Método 2: Classe JSONEncoder Personalizada

```python
import json
from datetime import datetime

class PessoaEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Pessoa):
            return {
                "nome": obj.nome,
                "idade": obj.idade,
                "data_cadastro": obj.data_cadastro.isoformat(),
                "tipo": "Pessoa"
            }
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

# Uso
p = Pessoa("Carlos", 35, datetime.now())
json_str = json.dumps(p, cls=PessoaEncoder, indent=2)
print(json_str)

# Desserialização personalizada
def decoder_pessoa(dict_obj):
    if "tipo" in dict_obj and dict_obj["tipo"] == "Pessoa":
        return Pessoa(
            dict_obj["nome"],
            dict_obj["idade"],
            datetime.fromisoformat(dict_obj["data_cadastro"])
        )
    return dict_obj

# Carregando com decoder personalizado
pessoa_carregada = json.loads(json_str, object_hook=decoder_pessoa)
print(type(pessoa_carregada))  # <class '__main__.Pessoa'>
```

### 5.3 Parâmetros Importantes do json.dumps()

```python
import json

dados = {
    "nome": "João",
    "idade": 30,
    "ativo": True,
    "saldo": None,
    "tags": ["python", "json"]
}

# indent - formata com indentação
print(json.dumps(dados, indent=2))

# sort_keys - ordena as chaves
print(json.dumps(dados, indent=2, sort_keys=True))

# separators - customiza separadores (mais compacto)
print(json.dumps(dados, separators=(',', ':')))

# ensure_ascii - permite caracteres não-ASCII
print(json.dumps({"nome": "José"}, ensure_ascii=False))

# skipkeys - ignora chaves não-string (em vez de erro)
print(json.dumps({1: "um", (1,2): "tupla"}, skipkeys=True))
```

---

## 6. Tratamento de Erros {#erros}

```python
import json

def processar_json(texto_json):
    try:
        dados = json.loads(texto_json)
        return dados
    except json.JSONDecodeError as e:
        print(f"Erro de sintaxe JSON: {e}")
        print(f"Na linha {e.lineno}, coluna {e.colno}")
        print(f"Caractere: {e.pos}")
        return None
    except TypeError as e:
        print(f"Tipo de dado inválido: {e}")
        return None
    except Exception as e:
        print(f"Erro inesperado: {e}")
        return None

# Testes
json_valido = '{"nome": "Maria", "idade": 25}'
json_invalido = '{"nome": "Maria", idade: 25}'  # Faltam aspas

print(processar_json(json_valido))
print(processar_json(json_invalido))
```

---

## 7. Exemplos Práticos {#exemplos}

### 7.1 API Simples com JSON

```python
import json
import requests  # Necessário instalar: pip install requests

def buscar_usuario_github(username):
    try:
        response = requests.get(f"https://api.github.com/users/{username}")
        response.raise_for_status()
        
        dados = response.json()
        
        info = {
            "login": dados.get("login"),
            "nome": dados.get("name"),
            "repositorios_publicos": dados.get("public_repos"),
            "seguidores": dados.get("followers"),
            "seguindo": dados.get("following")
        }
        
        # Salvar em arquivo
        with open(f"{username}_github.json", "w", encoding="utf-8") as f:
            json.dump(info, f, indent=4, ensure_ascii=False)
        
        return info
        
    except requests.exceptions.RequestException as e:
        print(f"Erro na requisição: {e}")
        return None

# Uso
usuario = buscar_usuario_github("python")
print(json.dumps(usuario, indent=2, ensure_ascii=False))
```

### 7.2 Gerenciador de Configurações

```python
import json
import os

class ConfigManager:
    def __init__(self, arquivo_config="config.json"):
        self.arquivo_config = arquivo_config
        self.config = self.carregar()
    
    def carregar(self):
        if os.path.exists(self.arquivo_config):
            try:
                with open(self.arquivo_config, 'r', encoding='utf-8') as f:
                    return json.load(f)
            except:
                return self.criar_padrao()
        else:
            return self.criar_padrao()
    
    def criar_padrao(self):
        config_padrao = {
            "banco_dados": {
                "host": "localhost",
                "porta": 5432,
                "usuario": "admin",
                "senha": ""
            },
            "aplicacao": {
                "debug": True,
                "timeout": 30,
                "idioma": "pt-BR"
            },
            "logs": {
                "nivel": "INFO",
                "arquivo": "app.log"
            }
        }
        self.salvar(config_padrao)
        return config_padrao
    
    def salvar(self, config=None):
        if config:
            self.config = config
        with open(self.arquivo_config, 'w', encoding='utf-8') as f:
            json.dump(self.config, f, indent=4, ensure_ascii=False)
    
    def get(self, chave, subchave=None):
        if subchave:
            return self.config.get(chave, {}).get(subchave)
        return self.config.get(chave)
    
    def set(self, chave, valor, subchave=None):
        if subchave:
            if chave not in self.config:
                self.config[chave] = {}
            self.config[chave][subchave] = valor
        else:
            self.config[chave] = valor
        self.salvar()

# Uso
config = ConfigManager()
print(config.get("banco_dados", "host"))  # localhost
config.set("banco_dados", "porta", 5433)
print(config.get("banco_dados", "porta"))  # 5433
```

### 7.3 Processamento de Lista de Tarefas

```python
import json
from datetime import datetime

class GerenciadorTarefas:
    def __init__(self, arquivo="tarefas.json"):
        self.arquivo = arquivo
        self.tarefas = self.carregar()
    
    def carregar(self):
        try:
            with open(self.arquivo, 'r', encoding='utf-8') as f:
                return json.load(f)
        except FileNotFoundError:
            return []
    
    def salvar(self):
        with open(self.arquivo, 'w', encoding='utf-8') as f:
            json.dump(self.tarefas, f, indent=2, ensure_ascii=False)
    
    def adicionar(self, titulo, descricao=""):
        tarefa = {
            "id": len(self.tarefas) + 1,
            "titulo": titulo,
            "descricao": descricao,
            "concluida": False,
            "criada_em": datetime.now().isoformat(),
            "concluida_em": None
        }
        self.tarefas.append(tarefa)
        self.salvar()
        return tarefa
    
    def listar(self, apenas_pendentes=False):
        if apenas_pendentes:
            return [t for t in self.tarefas if not t["concluida"]]
        return self.tarefas
    
    def concluir(self, id_tarefa):
        for tarefa in self.tarefas:
            if tarefa["id"] == id_tarefa:
                tarefa["concluida"] = True
                tarefa["concluida_em"] = datetime.now().isoformat()
                self.salvar()
                return True
        return False
    
    def remover(self, id_tarefa):
        self.tarefas = [t for t in self.tarefas if t["id"] != id_tarefa]
        self.salvar()

# Uso
gerenciador = GerenciadorTarefas()
gerenciador.adicionar("Estudar Python", "Praticar módulo JSON")
gerenciador.adicionar("Fazer exercícios")
print(json.dumps(gerenciador.listar(), indent=2, ensure_ascii=False))
```

### 7.4 Validação de JSON com Schema

```python
import json
from jsonschema import validate, ValidationError  # pip install jsonschema

# Definindo um schema
schema_usuario = {
    "type": "object",
    "properties": {
        "nome": {"type": "string", "minLength": 1},
        "idade": {"type": "integer", "minimum": 0, "maximum": 150},
        "email": {"type": "string", "format": "email"},
        "ativo": {"type": "boolean"},
        "tags": {
            "type": "array",
            "items": {"type": "string"},
            "uniqueItems": True
        }
    },
    "required": ["nome", "email"],
    "additionalProperties": False
}

def validar_usuario(dados):
    try:
        validate(instance=dados, schema=schema_usuario)
        print("JSON válido!")
        return True
    except ValidationError as e:
        print(f"Erro de validação: {e.message}")
        return False

# Testes
usuario_valido = {
    "nome": "João",
    "idade": 30,
    "email": "joao@email.com",
    "ativo": True,
    "tags": ["python", "json"]
}

usuario_invalido = {
    "nome": "Maria",
    "email": "email-invalido",
    "idade": "trinta"  # Deveria ser inteiro
}

print(validar_usuario(usuario_valido))    # True
print(validar_usuario(usuario_invalido))  # False
```

---

## 8. Dicas e Boas Práticas {#dicas}

### 8.1 Performance com Arquivos Grandes

```python
import json

# Para arquivos muito grandes, use ijson (biblioteca externa)
# pip install ijson

import ijson

def processar_json_grande(nome_arquivo):
    with open(nome_arquivo, 'rb') as f:
        parser = ijson.items(f, 'item')
        for item in parser:
            # Processa cada item individualmente
            yield item

# Exemplo: processar streaming
def criar_json_linha_por_linha(dados, arquivo_saida):
    with open(arquivo_saida, 'w', encoding='utf-8') as f:
        for item in dados:
            f.write(json.dumps(item, ensure_ascii=False) + '\n')

def ler_json_linha_por_linha(arquivo_entrada):
    with open(arquivo_entrada, 'r', encoding='utf-8') as f:
        for linha in f:
            yield json.loads(linha.strip())
```

### 8.2 Compactação

```python
import json
import gzip

# Salvar JSON compactado
def salvar_json_compactado(dados, arquivo):
    with gzip.open(arquivo, 'wt', encoding='utf-8') as f:
        json.dump(dados, f)

# Ler JSON compactado
def ler_json_compactado(arquivo):
    with gzip.open(arquivo, 'rt', encoding='utf-8') as f:
        return json.load(f)
```

### 8.3 Dicas Importantes

```python
import json

# 1. Use ensure_ascii=False para preservar acentos
dados = {"nome": "João"}
print(json.dumps(dados, ensure_ascii=False))  # {"nome": "João"}

# 2. Evite serializar objetos complexos diretamente
# Em vez disso:
def serializar_seguro(obj):
    if hasattr(obj, '__dict__'):
        return obj.__dict__
    return str(obj)

# 3. Use object_pairs_hook para manter ordem das chaves
from collections import OrderedDict
dados = json.loads('{"b": 1, "a": 2}', object_pairs_hook=OrderedDict)

# 4. Valide JSON antes de processar
def is_json(valor):
    try:
        json.loads(valor)
        return True
    except:
        return False

# 5. Use default parameter para tipos não serializáveis
from decimal import Decimal
dados = {"preco": Decimal("10.50")}
json_str = json.dumps(dados, default=str)  # Converte Decimal para string
```

### 8.4 Segurança

```python
import json

# Nunca use eval() em strings JSON
json_string = '{"__proto__": {"malicioso": true}}'

# Forma segura de carregar
dados = json.loads(json_string)  # Seguro

# Cuidado com objetos muito grandes (DoS)
def carregar_com_limite(arquivo, tamanho_max_mb=10):
    import os
    if os.path.getsize(arquivo) > tamanho_max_mb * 1024 * 1024:
        raise ValueError("Arquivo muito grande")
    
    with open(arquivo, 'r') as f:
        return json.load(f)
```

### 8.5 Resumo de Métodos

| Método | Descrição | Uso Principal |
|--------|-----------|---------------|
| `json.dumps(obj)` | Converte Python → JSON string | Serialização em memória |
| `json.loads(str)` | Converte JSON string → Python | Desserialização em memória |
| `json.dump(obj, file)` | Escreve JSON em arquivo | Salvar dados |
| `json.load(file)` | Lê JSON de arquivo | Carregar dados |
| `json.JSONEncoder` | Classe para personalizar serialização | Objetos complexos |
| `object_hook` | Parâmetro para personalizar desserialização | Objetos customizados |

---
