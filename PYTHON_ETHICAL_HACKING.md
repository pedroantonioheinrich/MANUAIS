# Manual Completo de Busca de Vulnerabilidades em Sites com Python
## Para Estudos de Ethical Hacking

> **Aviso Legal**: Este manual é destinado exclusivamente para fins educacionais e testes em ambientes autorizados. Nunca teste sistemas sem permissão explícita por escrito.

## Índice
1. [Introdução e Conceitos Básicos](#introdução)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Reconhecimento e Enumeração](#reconhecimento)
4. [Scanner de Portas e Serviços](#scanner-de-portas)
5. [Detecção de Diretórios e Arquivos](#detecção-de-diretórios)
6. [Análise de Cabeçalhos HTTP](#análise-de-cabeçalhos)
7. [Testes de Injeção SQL Básicos](#testes-sql)
8. [Testes XSS (Cross-Site Scripting)](#testes-xss)
9. [Verificação de Configurações de Segurança](#verificação-configurações)
10. [Relatórios e Documentação](#relatórios)
11. [Boas Práticas e Considerações Éticas](#boas-práticas)

---

## 1. Introdução e Conceitos Básicos <a name="introdução"></a>

### O Que é Ethical Hacking?
Ethical Hacking é a prática de testar sistemas de computadores, redes ou aplicações web para encontrar vulnerabilidades que um hacker mal-intencionado poderia explorar. O objetivo é identificar e corrigir essas falhas antes que sejam exploradas.

### Tipos Comuns de Vulnerabilidades Web:
- **SQL Injection**: Injeção de código SQL malicioso
- **XSS (Cross-Site Scripting)**: Execução de scripts no navegador da vítima
- **Directory Traversal**: Acesso a arquivos fora do diretório web
- **Security Misconfigurations**: Configurações inadequadas de segurança
- **Sensitive Data Exposure**: Exposição de dados sensíveis

---

## 2. Configuração do Ambiente <a name="configuração-do-ambiente"></a>

### Instalação do Python e Bibliotecas Necessárias

```bash
# Atualizar pip
python -m pip install --upgrade pip

# Instalar bibliotecas essenciais
pip install requests beautifulsoup4 lxml
```

### Explicação dos Pacotes:

1. **requests**: Biblioteca para fazer requisições HTTP
   - `pip install requests`
   - Por que usar? Porque é mais simples e poderosa que urllib padrão

2. **beautifulsoup4**: Parser de HTML/XML
   - `pip install beautifulsoup4`
   - Por que usar? Para extrair e analisar dados de páginas web

3. **lxml**: Parser XML/HTML de alto desempenho
   - `pip install lxml`
   - Por que usar? Alternative mais rápida ao parser padrão

### Configuração de Headers para Evitar Detecção:

```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'pt-BR,pt;q=0.9,en;q=0.8',
    'Accept-Encoding': 'gzip, deflate',
    'Connection': 'keep-alive',
    'Upgrade-Insecure-Requests': '1'
}

# Por que configurar headers?
# - User-Agent personalizado evita bloqueios
# - Headers realistas imitam navegadores legítimos
# - Alguns sites bloqueiam requisições sem headers adequados
```

---

## 3. Reconhecimento e Enumeração <a name="reconhecimento"></a>

### Coleta de Informações Básicas:

```python
import socket
import requests
from urllib.parse import urlparse

def reconhecimento_basico(url):
    """Coleta informações básicas sobre o alvo"""
    
    parsed_url = urlparse(url)
    dominio = parsed_url.netloc
    
    try:
        # Obter endereço IP
        ip = socket.gethostbyname(dominio)
        print(f"[+] IP do alvo: {ip}")
        
        # Obter informações do servidor via requisição HTTP
        resposta = requests.get(url, headers=headers, timeout=10)
        
        print(f"[+] Código de status: {resposta.status_code}")
        print(f"[+] Servidor: {resposta.headers.get('Server', 'Não identificado')}")
        print(f"[+] Tecnologias detectadas:")
        
        # Analisar headers para tecnologias
        for header, valor in resposta.headers.items():
            if any(tech in valor.lower() for tech in ['apache', 'nginx', 'iis', 'tomcat']):
                print(f"  - {header}: {valor}")
                
    except Exception as e:
        print(f"[-] Erro: {e}")

# Exemplo de uso
# reconhecimento_basico('http://exemplo.com')
```

---

## 4. Scanner de Portas e Serviços <a name="scanner-de-portas"></a>

### Scanner Básico de Portas:

```python
import socket
import threading
from queue import Queue

class ScannerPortas:
    def __init__(self, alvo, portas_comuns=True):
        self.alvo = alvo
        self.portas_abertas = []
        self.queue = Queue()
        
        # Portas comuns para serviços web
        self.portas = [21, 22, 23, 25, 53, 80, 110, 111, 135, 139, 143, 
                      443, 445, 993, 995, 1723, 3306, 3389, 5900, 8080]
        
    def scan_porta(self, porta):
        """Verifica se uma porta específica está aberta"""
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.settimeout(1)  # Timeout de 1 segundo
            resultado = sock.connect_ex((self.alvo, porta))
            
            if resultado == 0:
                # Porta aberta - tentar identificar serviço
                try:
                    sock.send(b'HEAD / HTTP/1.0\r\n\r\n')
                    banner = sock.recv(1024).decode('utf-8', errors='ignore')
                    servico = self.identificar_servico(banner, porta)
                except:
                    servico = "Serviço desconhecido"
                
                self.portas_abertas.append((porta, servico))
                print(f"[+] Porta {porta} aberta - {servico}")
            
            sock.close()
            
        except Exception as e:
            pass
    
    def identificar_servico(self, banner, porta):
        """Tenta identificar o serviço baseado no banner"""
        servicos = {
            21: "FTP",
            22: "SSH",
            25: "SMTP",
            80: "HTTP",
            443: "HTTPS",
            3306: "MySQL",
            3389: "RDP"
        }
        
        if porta in servicos:
            return servicos[porta]
        
        # Tentar identificar pelo banner
        if "apache" in banner.lower():
            return "Apache"
        elif "nginx" in banner.lower():
            return "Nginx"
        elif "iis" in banner.lower():
            return "IIS"
        
        return "Serviço desconhecido"
    
    def worker(self):
        """Função executada por cada thread"""
        while not self.queue.empty():
            porta = self.queue.get()
            self.scan_porta(porta)
            self.queue.task_done()
    
    def executar_scan(self, threads=50):
        """Executa o scan com múltiplas threads"""
        print(f"[*] Iniciando scan no alvo: {self.alvo}")
        
        # Adicionar portas na fila
        for porta in self.portas:
            self.queue.put(porta)
        
        # Criar e iniciar threads
        lista_threads = []
        for _ in range(threads):
            thread = threading.Thread(target=self.worker)
            thread.daemon = True
            thread.start()
            lista_threads.append(thread)
        
        # Aguardar conclusão
        self.queue.join()
        
        print(f"\n[*] Scan concluído. Portas abertas: {len(self.portas_abertas)}")
        return self.portas_abertas

# Exemplo de uso
# scanner = ScannerPortas('192.168.1.1')
# resultado = scanner.executar_scan()
```

---

## 5. Detecção de Diretórios e Arquivos <a name="detecção-de-diretórios"></a>

### Scanner de Diretórios Ocultos:

```python
import requests
import threading
from queue import Queue

class ScannerDiretorios:
    def __init__(self, url_base, wordlist="diretorios.txt"):
        self.url_base = url_base.rstrip('/')
        self.wordlist = wordlist
        self.diretorios_encontrados = []
        self.queue = Queue()
        
    def carregar_wordlist(self):
        """Carrega a lista de diretórios para testar"""
        try:
            with open(self.wordlist, 'r', encoding='utf-8') as f:
                diretorios = [linha.strip() for linha in f if linha.strip()]
            return diretorios
        except FileNotFoundError:
            # Wordlist padrão se o arquivo não existir
            return [
                'admin', 'login', 'wp-admin', 'wp-login', 'administrator',
                'backup', 'config', 'database', 'sql', 'private',
                'secret', 'hidden', 'test', 'debug', 'phpmyadmin',
                'uploads', 'images', 'css', 'js', 'includes'
            ]
    
    def testar_diretorio(self, diretorio):
        """Testa se um diretório existe"""
        urls_para_testar = [
            f"{self.url_base}/{diretorio}",
            f"{self.url_base}/{diretorio}/",
            f"{self.url_base}/{diretorio}.php",
            f"{self.url_base}/{diretorio}.html",
            f"{self.url_base}/{diretorio}.bak"
        ]
        
        for url in urls_para_testar:
            try:
                resposta = requests.get(
                    url, 
                    headers=headers, 
                    timeout=5,
                    allow_redirects=False
                )
                
                # Códigos que indicam sucesso ou redirecionamento
                if resposta.status_code in [200, 301, 302, 403]:
                    tamanho = len(resposta.content)
                    print(f"[+] Encontrado: {url} - Status: {resposta.status_code} - Tamanho: {tamanho}")
                    self.diretorios_encontrados.append({
                        'url': url,
                        'status': resposta.status_code,
                        'tamanho': tamanho
                    })
                    break  # Parar de testar variações se encontrou
                    
            except requests.exceptions.RequestException:
                continue
    
    def worker(self):
        """Função executada por cada thread"""
        while not self.queue.empty():
            diretorio = self.queue.get()
            self.testar_diretorio(diretorio)
            self.queue.task_done()
    
    def executar_scan(self, threads=20):
        """Executa o scan de diretórios"""
        print(f"[*] Iniciando scan de diretórios em: {self.url_base}")
        
        diretorios = self.carregar_wordlist()
        
        # Adicionar diretórios na fila
        for diretorio in diretorios:
            self.queue.put(diretorio)
        
        # Criar e iniciar threads
        for _ in range(threads):
            thread = threading.Thread(target=self.worker)
            thread.daemon = True
            thread.start()
        
        # Aguardar conclusão
        self.queue.join()
        
        print(f"\n[*] Scan concluído. Diretórios encontrados: {len(self.diretorios_encontrados)}")
        return self.diretorios_encontrados

# Exemplo de uso
# scanner = ScannerDiretorios('http://exemplo.com')
# resultado = scanner.executar_scan()
```

---

## 6. Análise de Cabeçalhos HTTP <a name="análise-de-cabeçalhos"></a>

### Verificador de Headers de Segurança:

```python
def analisar_headers_seguranca(url):
    """Analisa headers HTTP em busca de problemas de segurança"""
    
    problemas = []
    recomendacoes = []
    
    try:
        resposta = requests.get(url, headers=headers, timeout=10)
        headers = resposta.headers
        
        print("[*] Analisando headers de segurança...")
        
        # 1. Verificar HSTS (HTTP Strict Transport Security)
        if 'strict-transport-security' not in headers:
            problemas.append("HSTS não implementado")
            recomendacoes.append("Implementar HSTS para forçar HTTPS")
        else:
            print("[+] HSTS implementado")
        
        # 2. Verificar X-Frame-Options
        if 'x-frame-options' not in headers:
            problemas.append("X-Frame-Options não implementado")
            recomendacoes.append("Adicionar X-Frame-Options para prevenir clickjacking")
        else:
            print(f"[+] X-Frame-Options: {headers['x-frame-options']}")
        
        # 3. Verificar X-XSS-Protection
        if 'x-xss-protection' not in headers:
            problemas.append("X-XSS-Protection não implementado")
            recomendacoes.append("Configurar X-XSS-Protection: 1; mode=block")
        else:
            print(f"[+] X-XSS-Protection: {headers['x-xss-protection']}")
        
        # 4. Verificar X-Content-Type-Options
        if 'x-content-type-options' not in headers:
            problemas.append("X-Content-Type-Options não implementado")
            recomendacoes.append("Adicionar X-Content-Type-Options: nosniff")
        else:
            print(f"[+] X-Content-Type-Options: {headers['x-content-type-options']}")
        
        # 5. Verificar Content-Security-Policy
        if 'content-security-policy' not in headers:
            problemas.append("Content-Security-Policy não implementado")
            recomendacoes.append("Implementar CSP para mitigar XSS")
        else:
            print(f"[+] Content-Security-Policy presente")
        
        # 6. Verificar informações sensíveis no Server header
        if 'server' in headers:
            server_info = headers['server']
            if any(info in server_info.lower() for info in ['version', 'apache/', 'nginx/', 'iis/']):
                problemas.append(f"Informações do servidor expostas: {server_info}")
                recomendacoes.append("Ocultar ou minimizar informações no header Server")
        
        # 7. Verificar métodos HTTP permitidos
        if 'allow' in headers:
            metodos = headers['allow']
            if 'PUT' in metodos or 'DELETE' in metodos:
                problemas.append(f"Métodos HTTP perigosos permitidos: {metodos}")
                recomendacoes.append("Remover métodos HTTP desnecessários (PUT, DELETE, etc.)")
        
        # Exibir resultados
        if problemas:
            print("\n[!] PROBLEMAS ENCONTRADOS:")
            for problema in problemas:
                print(f"  - {problema}")
            
            print("\n[!] RECOMENDAÇÕES:")
            for recomendacao in recomendacoes:
                print(f"  - {recomendacao}")
        else:
            print("\n[+] Todos os headers de segurança estão configurados corretamente!")
            
    except Exception as e:
        print(f"[-] Erro na análise: {e}")
    
    return problemas, recomendacoes

# Exemplo de uso
# analisar_headers_seguranca('http://exemplo.com')
```

---

## 7. Testes de Injeção SQL Básicos <a name="testes-sql"></a>

### Detector Básico de SQL Injection:

```python
def testar_sql_injection(url, parametro):
    """Testa vulnerabilidades básicas de SQL Injection"""
    
    print(f"[*] Testando SQL Injection no parâmetro: {parametro}")
    
    payloads = [
        "'",
        "''",
        "' OR '1'='1",
        "' OR '1'='1' --",
        "' OR '1'='1' /*",
        "' UNION SELECT NULL --",
        "' UNION SELECT NULL, NULL --",
        "1' ORDER BY 1--",
        "1' ORDER BY 100--",
        "1' AND 1=1--",
        "1' AND 1=2--"
    ]
    
    resultados = []
    
    for payload in payloads:
        try:
            # Construir URL com payload
            url_testar = f"{url}?{parametro}={payload}"
            
            resposta = requests.get(
                url_testar,
                headers=headers,
                timeout=10,
                allow_redirects=False
            )
            
            # Verificar indicadores de vulnerabilidade
            indicadores_vulnerabilidade = [
                'sql syntax',
                'mysql',
                'sqlite',
                'postgresql',
                'oracle',
                'syntax error',
                'unclosed quotation',
                'warning: mysql'
            ]
            
            conteudo = resposta.text.lower()
            vulneravel = any(indicator in conteudo for indicator in indicadores_vulnerabilidade)
            
            # Diferença no tamanho da resposta pode indicar vulnerabilidade
            if len(resposta.content) > 0:
                if vulneravel:
                    print(f"[!] POSSÍVEL SQLi: {payload}")
                    resultados.append({
                        'payload': payload,
                        'status': resposta.status_code,
                        'tamanho': len(resposta.content),
                        'vulneravel': True
                    })
                else:
                    print(f"[.] Testado: {payload} - Status: {resposta.status_code}")
            
        except Exception as e:
            print(f"[-] Erro com payload {payload}: {e}")
    
    return resultados

def scanner_sql_parametros(url):
    """Escaneia uma URL por parâmetros e testa SQL Injection"""
    
    try:
        # Primeiro, obter a página
        resposta = requests.get(url, headers=headers, timeout=10)
        
        # Analisar URL para parâmetros
        from urllib.parse import urlparse, parse_qs
        
        parsed = urlparse(url)
        parametros = parse_qs(parsed.query)
        
        print(f"[*] Encontrados {len(parametros)} parâmetros na URL")
        
        resultados_totais = []
        
        # Testar cada parâmetro
        for parametro in parametros:
            print(f"\n[*] Testando parâmetro: {parametro}")
            resultados = testar_sql_injection(url.split('?')[0], parametro)
            resultados_totais.extend(resultados)
        
        return resultados_totais
        
    except Exception as e:
        print(f"[-] Erro no scanner: {e}")
        return []

# Exemplo de uso
# resultados = scanner_sql_parametros('http://exemplo.com/pagina.php?id=1')
```

---

## 8. Testes XSS (Cross-Site Scripting) <a name="testes-xss"></a>

### Scanner Básico de XSS:

```python
def testar_xss_refletido(url, parametro):
    """Testa vulnerabilidades de XSS Refletido"""
    
    print(f"[*] Testando XSS no parâmetro: {parametro}")
    
    payloads = [
        "<script>alert('XSS')</script>",
        "'> <script>alert('XSS')</script>",
        "\"><script>alert('XSS')</script>",
        "<img src=x onerror=alert('XSS')>",
        "<svg onload=alert('XSS')>",
        "javascript:alert('XSS')",
        "onmouseover=alert('XSS')",
        "<body onload=alert('XSS')>",
        "<iframe src=javascript:alert('XSS')>",
        "<input type=text value='<script>alert(1)</script>'>"
    ]
    
    resultados = []
    
    for payload in payloads:
        try:
            # Codificar payload para URL
            import urllib.parse
            payload_encoded = urllib.parse.quote(payload)
            
            # Construir URL com payload
            if '?' in url:
                url_testar = f"{url}&{parametro}={payload_encoded}"
            else:
                url_testar = f"{url}?{parametro}={payload_encoded}"
            
            resposta = requests.get(
                url_testar,
                headers=headers,
                timeout=10,
                allow_redirects=False
            )
            
            # Verificar se o payload foi refletido
            if payload in resposta.text or payload_encoded in resposta.text:
                print(f"[!] POSSÍVEL XSS: Payload refletido")
                resultados.append({
                    'payload': payload,
                    'url': url_testar,
                    'refletido': True,
                    'status': resposta.status_code
                })
            else:
                # Verificar variações
                payload_limpo = payload.replace('<', '').replace('>', '')
                if payload_limpo in resposta.text:
                    print(f"[.] Payload parcialmente refletido")
            
        except Exception as e:
            print(f"[-] Erro com payload {payload}: {e}")
    
    return resultados

def scanner_xss_formularios(url):
    """Procura e testa formulários para XSS"""
    
    from bs4 import BeautifulSoup
    
    try:
        resposta = requests.get(url, headers=headers, timeout=10)
        soup = BeautifulSoup(resposta.text, 'html.parser')
        
        formularios = soup.find_all('form')
        print(f"[*] Encontrados {len(formularios)} formulários")
        
        resultados = []
        
        for i, formulario in enumerate(formularios):
            print(f"\n[*] Analisando formulário {i+1}")
            
            # Encontrar ação do formulário
            acao = formulario.get('action', '')
            if not acao.startswith('http'):
                # URL relativa - converter para absoluta
                from urllib.parse import urljoin
                acao = urljoin(url, acao)
            
            # Encontrar método
            metodo = formulario.get('method', 'get').lower()
            
            # Encontrar inputs
            inputs = formulario.find_all('input')
            parametros = {}
            
            for inp in inputs:
                nome = inp.get('name')
                tipo = inp.get('type', 'text')
                valor = inp.get('value', '')
                
                if nome and tipo != 'submit':
                    if metodo == 'get':
                        # Para GET, testar cada parâmetro individualmente
                        resultados.extend(testar_xss_refletido(acao, nome))
                    elif metodo == 'post':
                        # Para POST, coletar parâmetros
                        parametros[nome] = valor
        
        return resultados
        
    except Exception as e:
        print(f"[-] Erro no scanner de formulários: {e}")
        return []

# Exemplo de uso
# resultados = scanner_xss_formularios('http://exemplo.com/contato.php')
```

---

## 9. Verificação de Configurações de Segurança <a name="verificação-configurações"></a>

### Verificador de Configurações Comuns:

```python
def verificar_configuracoes(url):
    """Verifica configurações comuns de segurança"""
    
    testes = {
        'backup_files': [
            '.bak', '.backup', '.old', '.orig', '.tmp',
            'backup.zip', 'database.sql', 'dump.sql'
        ],
        'config_files': [
            'config.php', 'configuration.php', 'settings.php',
            '.env', 'config.json', 'config.ini',
            'web.config', '.htaccess'
        ],
        'info_files': [
            'phpinfo.php', 'info.php', 'test.php',
            'server-status', 'server-info'
        ]
    }
    
    print("[*] Verificando arquivos sensíveis...")
    
    encontrados = []
    
    for categoria, arquivos in testes.items():
        print(f"\n[*] Testando {categoria.replace('_', ' ')}...")
        
        for arquivo in arquivos:
            urls_para_testar = [
                f"{url.rstrip('/')}/{arquivo}",
                f"{url.rstrip('/')}/../{arquivo}",
                f"{url.rstrip('/')}/.git/{arquivo}",
                f"{url.rstrip('/')}/backup/{arquivo}"
            ]
            
            for url_testar in urls_para_testar:
                try:
                    resposta = requests.get(
                        url_testar,
                        headers=headers,
                        timeout=5,
                        allow_redirects=False
                    )
                    
                    if resposta.status_code == 200:
                        tamanho = len(resposta.content)
                        if tamanho > 0:  # Evitar falsos positivos
                            print(f"[!] ENCONTRADO: {url_testar} - Tamanho: {tamanho}")
                            encontrados.append({
                                'url': url_testar,
                                'categoria': categoria,
                                'tamanho': tamanho
                            })
                            
                except:
                    continue
    
    # Verificar métodos HTTP
    print("\n[*] Testando métodos HTTP...")
    metodos = ['OPTIONS', 'TRACE', 'PUT', 'DELETE', 'CONNECT']
    
    for metodo in metodos:
        try:
            resposta = requests.request(
                metodo,
                url,
                headers=headers,
                timeout=5
            )
            
            if resposta.status_code not in [405, 501]:  # Método não permitido
                print(f"[!] Método {metodo} permitido: Status {resposta.status_code}")
                
        except:
            pass
    
    # Verificar versões expostas
    print("\n[*] Verificando versões de software...")
    
    try:
        resposta = requests.get(url, headers=headers, timeout=10)
        
        # Verificar no HTML
        padroes = [
            ('WordPress', ['wp-content', 'wp-includes', 'wordpress']),
            ('Joomla', ['joomla', 'media/jui']),
            ('Drupal', ['drupal', 'sites/default']),
            ('Magento', ['magento', 'skin/frontend'])
        ]
        
        for software, indicadores in padroes:
            if any(ind in resposta.text for ind in indicadores):
                print(f"[+] Possível {software} detectado")
                
    except Exception as e:
        print(f"[-] Erro: {e}")
    
    return encontrados

# Exemplo de uso
# resultados = verificar_configuracoes('http://exemplo.com')
```

---

## 10. Relatórios e Documentação <a name="relatórios"></a>

### Gerador de Relatórios:

```python
import json
from datetime import datetime

class GeradorRelatorio:
    def __init__(self, alvo):
        self.alvo = alvo
        self.data = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.resultados = {
            'alvo': alvo,
            'data': self.data,
            'reconhecimento': {},
            'vulnerabilidades': [],
            'recomendacoes': []
        }
    
    def adicionar_reconhecimento(self, categoria, dados):
        """Adiciona dados de reconhecimento ao relatório"""
        self.resultados['reconhecimento'][categoria] = dados
    
    def adicionar_vulnerabilidade(self, tipo, severidade, descricao, evidencia):
        """Adiciona uma vulnerabilidade ao relatório"""
        vulnerabilidade = {
            'id': len(self.resultados['vulnerabilidades']) + 1,
            'tipo': tipo,
            'severidade': severidade,
            'descricao': descricao,
            'evidencia': evidencia,
            'data_descoberta': self.data
        }
        self.resultados['vulnerabilidades'].append(vulnerabilidade)
        
        # Adicionar recomendação automática
        self.adicionar_recomendacao(tipo, severidade)
    
    def adicionar_recomendacao(self, tipo_vulnerabilidade, severidade):
        """Adiciona recomendação baseada no tipo de vulnerabilidade"""
        
        recomendacoes = {
            'SQL Injection': [
                "Utilizar prepared statements com query parametrizada",
                "Validar e sanitizar todas as entradas do usuário",
                "Implementar WAF (Web Application Firewall)",
                "Aplicar o princípio do menor privilégio no banco de dados"
            ],
            'XSS': [
                "Implementar Content Security Policy (CSP)",
                "Sanitizar todas as saídas para o navegador",
                "Utilizar funções de escape apropriadas",
                "Validar entrada de dados no servidor e no cliente"
            ],
            'Directory Traversal': [
                "Validar caminhos de arquivo fornecidos pelo usuário",
                "Utilizar mapeamento de caminhos canônicos",
                "Implementar lista branca de arquivos permitidos",
                "Executar aplicação com privilégios mínimos"
            ],
            'Security Headers': [
                "Implementar headers de segurança HTTP",
                "Configurar HSTS para forçar HTTPS",
                "Adicionar X-Frame-Options e X-Content-Type-Options",
                "Configurar Content-Security-Policy adequadamente"
            ]
        }
        
        if tipo_vulnerabilidade in recomendacoes:
            for rec in recomendacoes[tipo_vulnerabilidade]:
                self.resultados['recomendacoes'].append({
                    'vulnerabilidade': tipo_vulnerabilidade,
                    'recomendacao': rec,
                    'prioridade': 'Alta' if severidade in ['Crítica', 'Alta'] else 'Média'
                })
    
    def gerar_relatorio_txt(self, arquivo='relatorio.txt'):
        """Gera relatório em formato texto"""
        
        with open(arquivo, 'w', encoding='utf-8') as f:
            f.write("=" * 60 + "\n")
            f.write("RELATÓRIO DE SEGURANÇA\n")
            f.write("=" * 60 + "\n\n")
            
            f.write(f"Alvo: {self.resultados['alvo']}\n")
            f.write(f"Data: {self.resultados['data']}\n")
            f.write(f"Realizado por: Ethical Hacker (Estudo)\n\n")
            
            f.write("-" * 60 + "\n")
            f.write("1. INFORMAÇÕES DE RECONHECIMENTO\n")
            f.write("-" * 60 + "\n\n")
            
            for categoria, dados in self.resultados['reconhecimento'].items():
                f.write(f"{categoria.upper()}:\n")
                if isinstance(dados, dict):
                    for chave, valor in dados.items():
                        f.write(f"  {chave}: {valor}\n")
                elif isinstance(dados, list):
                    for item in dados:
                        f.write(f"  - {item}\n")
                else:
                    f.write(f"  {dados}\n")
                f.write("\n")
            
            f.write("-" * 60 + "\n")
            f.write("2. VULNERABILIDADES ENCONTRADAS\n")
            f.write("-" * 60 + "\n\n")
            
            if not self.resultados['vulnerabilidades']:
                f.write("Nenhuma vulnerabilidade crítica encontrada.\n\n")
            else:
                for vuln in self.resultados['vulnerabilidades']:
                    f.write(f"ID: {vuln['id']}\n")
                    f.write(f"Tipo: {vuln['tipo']}\n")
                    f.write(f"Severidade: {vuln['severidade']}\n")
                    f.write(f"Descrição: {vuln['descricao']}\n")
                    f.write(f"Evidência: {vuln['evidencia']}\n")
                    f.write(f"Data: {vuln['data_descoberta']}\n")
                    f.write("-" * 40 + "\n\n")
            
            f.write("-" * 60 + "\n")
            f.write("3. RECOMENDAÇÕES\n")
            f.write("-" * 60 + "\n\n")
            
            for rec in self.resultados['recomendacoes']:
                f.write(f"Vulnerabilidade: {rec['vulnerabilidade']}\n")
                f.write(f"Recomendação: {rec['recomendacao']}\n")
                f.write(f"Prioridade: {rec['prioridade']}\n")
                f.write("-" * 40 + "\n\n")
        
        print(f"[+] Relatório gerado: {arquivo}")
    
    def gerar_relatorio_json(self, arquivo='relatorio.json'):
        """Gera relatório em formato JSON"""
        
        with open(arquivo, 'w', encoding='utf-8') as f:
            json.dump(self.resultados, f, indent=2, ensure_ascii=False)
        
        print(f"[+] Relatório JSON gerado: {arquivo}")
    
    def gerar_resumo_executivo(self):
        """Gera um resumo executivo do relatório"""
        
        print("\n" + "=" * 60)
        print("RESUMO EXECUTIVO")
        print("=" * 60)
        
        total_vulns = len(self.resultados['vulnerabilidades'])
        severidades = {}
        
        for vuln in self.resultados['vulnerabilidades']:
            sev = vuln['severidade']
            severidades[sev] = severidades.get(sev, 0) + 1
        
        print(f"\nTotal de Vulnerabilidades: {total_vulns}")
        
        if severidades:
            print("\nDistribuição por Severidade:")
            for sev, count in severidades.items():
                print(f"  {sev}: {count}")
        
        print("\nRecomendações Prioritárias:")
        recs_altas = [r for r in self.resultados['recomendacoes'] if r['prioridade'] == 'Alta']
        
        for i, rec in enumerate(recs_altas[:3], 1):  # Top 3
            print(f"{i}. {rec['recomendacao']}")

# Exemplo de uso
# relatorio = GeradorRelatorio('http://exemplo.com')
# relatorio.adicionar_vulnerabilidade('SQL Injection', 'Alta', 'Parâmetro id vulnerável', 'Payload: \' OR 1=1--')
# relatorio.gerar_relatorio_txt()
# relatorio.gerar_resumo_executivo()
```

---

## 11. Boas Práticas e Considerações Éticas <a name="boas-práticas"></a>

### 11.1 Código de Conduta Ético

```python
# ESTE CÓDIGO DEVE SER SEMPRE RESPEITADO
CODIGO_ETICO = """
PRINCÍPIOS DO ETHICAL HACKER:

1. AUTORIZAÇÃO ESCRITA
   - Nunca teste sistemas sem permissão explícita
   - Obtenha contrato/termo de autorização por escrito
   - Defina escopo claramente antes de começar

2. RESPEITO PELA PRIVACIDADE
   - Não acesse dados pessoais sem necessidade
   - Mantenha confidencialidade dos dados encontrados
   - Reporte apenas informações relevantes à segurança

3. NÃO CAUSAR DANOS
   - Evite testes que possam causar interrupção de serviço
   - Não modifique dados sem permissão
   - Use ambientes de teste controlados quando possível

4. DOCUMENTAÇÃO ADEQUADA
   - Documente todos os passos realizados
   - Mantenha evidências claras das vulnerabilidades
   - Forneça relatórios detalhados e acionáveis

5. SEGUIR A LEI
   - Conheça e respeite as leis locais e internacionais
   - Não ultrapasse os limites da autorização recebida
   - Reporte atividades ilegais encontradas
"""

def verificar_conformidade_etica():
    """Função para garantir conformidade ética"""
    print("=" * 60)
    print("VERIFICAÇÃO DE CONFORMIDADE ÉTICA")
    print("=" * 60)
    
    perguntas = [
        "Você possui autorização por escrito para testar este sistema?",
        "O escopo do teste está claramente definido?",
        "Você está testando em ambiente de produção ou homologação?",
        "Há backup dos dados antes dos testes?",
        "O proprietário do sistema está ciente dos testes?"
    ]
    
    respostas = []
    
    for pergunta in perguntas:
        print(f"\n{pergunta}")
        resposta = input("(s/n): ").lower().strip()
        respostas.append(resposta == 's')
    
    if all(respostas):
        print("\n[+] Conformidade ética verificada. Pode prosseguir.")
        return True
    else:
        print("\n[!] ALERTA: Conformidade ética não verificada!")
        print("    Não prossiga sem autorização adequada.")
        return False
```

### 11.2 Configuração Segura do Ambiente de Testes

```python
def configurar_ambiente_seguro():
    """Configura um ambiente de testes seguro"""
    
    config = {
        'diretorio_trabalho': 'pentest_results',
        'log_arquivo': 'activity.log',
        'backup_interval': 30,  # minutos
        'criptografia_dados': True,
        'proxy_config': None,
        'rate_limit': 10  # requisições por segundo
    }
    
    import os
    import hashlib
    from datetime import datetime
    
    # Criar diretório de trabalho seguro
    if not os.path.exists(config['diretorio_trabalho']):
        os.makedirs(config['diretorio_trabalho'])
        print(f"[+] Diretório criado: {config['diretorio_trabalho']}")
    
    # Configurar logging
    def log_activity(acao, detalhes=""):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        with open(os.path.join(config['diretorio_trabalho'], config['log_arquivo']), 'a') as f:
            f.write(f"[{timestamp}] {acao}: {detalhes}\n")
    
    # Função para criptografar dados sensíveis
    def encrypt_data(data, key):
        # Implementação básica de criptografia (apenas para estudo)
        # Em produção, use bibliotecas como cryptography
        import base64
        from hashlib import sha256
        
        key_hash = sha256(key.encode()).digest()
        # Nota: Esta é uma implementação simplificada para estudo
        # Não use em produção sem bibliotecas adequadas
        encoded = base64.b64encode(data.encode()).decode()
        return encoded
    
    print("[+] Ambiente configurado com segurança")
    return config, log_activity, encrypt_data
```

### 11.3 Script de Limpeza e Anonimização

```python
def limpar_dados_sensiveis(arquivo):
    """Remove dados sensíveis dos resultados"""
    
    import re
    
    padroes_sensiveis = [
        r'\b\d{3}\.\d{3}\.\d{3}\.\d{3}\b',  # IPs
        r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',  # Emails
        r'\b(?:\d{3}[-\s]?){2}\d{4}\b',  # Telefones
        r'\b\d{3}[.-]?\d{3}[.-]?\d{3}[.-]?\d{2}\b',  # CPF
    ]
    
    try:
        with open(arquivo, 'r', encoding='utf-8') as f:
            conteudo = f.read()
        
        for padrao in padroes_sensiveis:
            conteudo = re.sub(padrao, '[REDACTED]', conteudo)
        
        # Salvar versão limpa
        nome_limpo = arquivo.replace('.', '_limpo.')
        with open(nome_limpo, 'w', encoding='utf-8') as f:
            f.write(conteudo)
        
        print(f"[+] Dados sensíveis removidos: {nome_limpo}")
        return nome_limpo
        
    except Exception as e:
        print(f"[-] Erro na limpeza: {e}")
        return arquivo
```

---

## Conclusão

Este manual fornece uma base sólida para começar seus estudos em Ethical Hacking com Python. Lembre-se sempre:

1. **Estude continuamente** - A segurança da informação está sempre evoluindo
2. **Pratique em ambientes controlados** - Use laboratórios como HackTheBox, TryHackMe, ou máquinas virtuais próprias
3. **Documente tudo** - Manter registros detalhados é crucial
4. **Respeite a ética** - O poder do conhecimento traz grande responsabilidade
5. **Compartilhe conhecimento** - A comunidade de segurança prospera com a colaboração

### Recursos Recomendados para Estudo:
- OWASP Web Security Testing Guide
- PTES (Penetration Testing Execution Standard)
- MITRE ATT&CK Framework
- Livros: "The Web Application Hacker's Handbook", "Black Hat Python"

### Laboratórios de Prática (Legais):
- HackTheBox (hackthebox.com)
- TryHackMe (tryhackme.com)
- PentesterLab (pentesterlab.com)
- DVWA (Damn Vulnerable Web Application)

---
=
