# 🌐 Manual Completo de Proxy em Python

**Guia Definitivo para Criar Seu Próprio Proxy - Explicação em Português Brasileiro**

---

## 🎯 Índice

1. [O que é um Proxy?](#o-que-é-um-proxy)
2. [Como um Proxy Funciona](#como-um-proxy-funciona)
3. [Tipos de Proxy](#tipos-de-proxy)
4. [Arquitetura de um Proxy](#arquitetura-de-um-proxy)
5. [Criando um Proxy Simples](#criando-um-proxy-simples)
6. [Proxy HTTP/HTTPS](#proxy-httphttps)
7. [Proxy SOCKS](#proxy-socks)
8. [Proxy com Autenticação](#proxy-com-autenticação)
9. [Proxy com Cache](#proxy-com-cache)
10. [Proxy com Filtragem](#proxy-com-filtragem)
11. [Proxy Reverso](#proxy-reverso)
12. [Casos de Uso Práticos](#casos-de-uso-práticos)
13. [Segurança em Proxies](#segurança-em-proxies)
14. [Otimização e Performance](#otimização-e-performance)
15. [Glossário](#glossário)

---

## 🔍 O que é um Proxy?

### 📖 Definição Simples

**Um proxy é um "intermediário" entre você e a internet.** Pense nele como um "representante" ou "mensageiro" que faz pedidos em seu nome.

### 🧩 Analogias do Mundo Real

| **Proxy** | **Analogia do Mundo Real** | **Como Funciona** |
|-----------|----------------------------|-------------------|
| **Proxy Comum** | Assistente pessoal | Você pede ao assistente, ele busca para você |
| **Proxy Anônimo** | Fantasia em uma festa | Ninguém sabe quem você realmente é |
| **Proxy Reverso** | Recepcionista de hotel | Direciona visitantes para os quartos certos |
| **Proxy de Cache** | Biblioteca local | Guarda cópias para entregar mais rápido |

### 🎯 Por que Usar um Proxy?

1. **Anonimato**: Esconde seu IP real
2. **Segurança**: Filtra conteúdo malicioso
3. **Controle**: Bloqueia sites indesejados
4. **Cache**: Acelera acesso a sites frequentes
5. **Bypass**: Contorna restrições geográficas
6. **Balanceamento**: Distribui carga entre servidores

---

## 🏗️ Como um Proxy Funciona

### Fluxo Básico de Comunicação

```
SEM PROXY:
Usuário →→→→→→→→→→→→→→→→→ Site

COM PROXY:
Usuário → Proxy → Site
        ↑        ↓
        ←--------←
```

### Passo a Passo de uma Requisição

```python
# 1. Cliente se conecta ao Proxy
cliente → "Quero acessar google.com"

# 2. Proxy se conecta ao Site
proxy → "Cliente quer acessar google.com"

# 3. Site responde ao Proxy
site → "Aqui está a página do Google"

# 4. Proxy repassa para Cliente
proxy → cliente: "Aqui está a página do Google"
```

---

## 📊 Tipos de Proxy

### 1. **Proxy HTTP/HTTPS**
- Para navegação web
- Entende protocolos HTTP
- Pode cachear conteúdo

### 2. **Proxy SOCKS**
- Trabalha em nível mais baixo
- Funciona com qualquer protocolo
- Não entende conteúdo (apenas repassa)

### 3. **Proxy Transparente**
- Cliente não sabe que está usando
- Usado em redes corporativas
- Não muda requisições

### 4. **Proxy Anônimo**
- Esconde IP do cliente
- Revela que é um proxy
- Nível básico de anonimato

### 5. **Proxy Elite/High Anonymity**
- Esconde completamente
- Parece ser usuário comum
- Máximo anonimato

### 6. **Proxy Reverso**
- Protege servidores internos
- Balanceia carga
- Cache para servidores

---

## 🏗️ Arquitetura de um Proxy

### Diagrama de Componentes

```
┌─────────┐     ┌─────────┐     ┌─────────┐
│         │     │         │     │         │
│ CLIENTE │────▶│ PROXY   │────▶│ SERVER  │
│         │     │         │     │         │
└─────────┘     ├─────────┤     └─────────┘
                │LISTENER │
                ├─────────┤
                │HANDLER  │
                ├─────────┤
                │CACHE    │
                ├─────────┤
                │FILTER   │
                └─────────┘
```

### Módulos Principais

```python
class Proxy:
    def __init__(self):
        self.listener = Listener()    # Escuta conexões
        self.handlers = []           # Processa requisições
        self.cache = Cache()         # Armazena respostas
        self.filters = Filters()     # Filtra conteúdo
        self.logger = Logger()       # Registra atividades
```

---

## 🚀 Criando um Proxy Simples

### Proxy Básico em Python (PASSO A PASSO)

**proxy_basico.py**
```python
import socket
import threading
import sys

class ProxySimples:
    """Proxy mais básico possível - para entender os conceitos"""
    
    def __init__(self, host='localhost', porta=8888):
        """
        Inicializa o proxy
        
        Args:
            host: Onde o proxy vai escutar (ex: 'localhost', '0.0.0.0')
            porta: Porta do proxy (ex: 8888)
        """
        self.host = host
        self.porta = porta
        self.proxy_socket = None
        
        print(f"🔧 Inicializando Proxy Simples")
        print(f"📡 Host: {host}")
        print(f"🚪 Porta: {porta}")
        print("-" * 50)
    
    def iniciar(self):
        """Inicia o servidor proxy"""
        
        try:
            # 1. Cria o socket do proxy
            self.proxy_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            
            # 2. Configura para reusar porta (evita erro "Address already in use")
            self.proxy_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            
            # 3. Vincula ao endereço e porta
            self.proxy_socket.bind((self.host, self.porta))
            
            # 4. Começa a escutar (máximo 10 conexões na fila)
            self.proxy_socket.listen(10)
            
            print(f"✅ Proxy iniciado em {self.host}:{self.porta}")
            print("📞 Aguardando conexões de clientes...")
            print("ℹ️  Configure seu navegador para usar proxy:")
            print(f"   Server: {self.host}")
            print(f"   Port: {self.porta}")
            print("-" * 50)
            
            # 5. Loop principal - aceita conexões
            while True:
                # Aceita conexão do cliente
                cliente_socket, cliente_endereco = self.proxy_socket.accept()
                
                print(f"👤 Cliente conectado: {cliente_endereco[0]}:{cliente_endereco[1]}")
                
                # Cria uma thread para lidar com este cliente
                thread = threading.Thread(
                    target=self.tratar_cliente,
                    args=(cliente_socket, cliente_endereco)
                )
                thread.daemon = True  # Thread morre quando programa principal morre
                thread.start()
                
        except KeyboardInterrupt:
            print("\n\n🛑 Proxy interrompido pelo usuário")
            
        except Exception as e:
            print(f"❌ Erro ao iniciar proxy: {e}")
            
        finally:
            self.parar()
    
    def tratar_cliente(self, cliente_socket, cliente_endereco):
        """
        Trata a conexão de um cliente individual
        
        Args:
            cliente_socket: Socket do cliente
            cliente_endereco: Tupla (IP, porta) do cliente
        """
        try:
            # 1. Recebe a requisição do cliente
            requisicao = cliente_socket.recv(4096)
            
            if not requisicao:
                return  # Cliente desconectou
            
            print(f"📨 Requisição recebida de {cliente_endereco[0]}:")
            print(f"   Tamanho: {len(requisicao)} bytes")
            
            # 2. Extrai informações da requisição HTTP
            # Formato: "GET http://site.com/ HTTP/1.1"
            requisicao_str = requisicao.decode('utf-8', errors='ignore')
            linhas = requisicao_str.split('\r\n')
            
            if linhas and linhas[0]:
                primeira_linha = linhas[0]
                print(f"   Método: {primeira_linha.split(' ')[0] if ' ' in primeira_linha else 'DESCONHECIDO'}")
            
            # 3. Encontra o destino (site que cliente quer acessar)
            destino = self.extrair_destino(requisicao_str)
            
            if not destino:
                print("   ⚠️  Não foi possível determinar o destino")
                cliente_socket.close()
                return
            
            print(f"   Destino: {destino}")
            
            # 4. Conecta ao site de destino
            try:
                # Separa host e porta do destino
                if ':' in destino:
                    host, porta_str = destino.split(':')
                    porta = int(porta_str)
                else:
                    host = destino
                    porta = 80  # Porta padrão HTTP
                
                # Cria socket para o servidor de destino
                servidor_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                servidor_socket.settimeout(10)  # Timeout de 10 segundos
                
                # Conecta ao servidor
                servidor_socket.connect((host, porta))
                print(f"   🔗 Conectado ao servidor: {host}:{porta}")
                
                # 5. Envia a requisição do cliente para o servidor
                servidor_socket.send(requisicao)
                print(f"   📤 Requisição encaminhada para {host}")
                
                # 6. Recebe a resposta do servidor
                resposta_total = b""
                while True:
                    resposta = servidor_socket.recv(4096)
                    if not resposta:
                        break
                    resposta_total += resposta
                
                print(f"   📥 Resposta recebida: {len(resposta_total)} bytes")
                
                # 7. Envia a resposta de volta para o cliente
                cliente_socket.send(resposta_total)
                print(f"   📨 Resposta enviada para cliente")
                
                # Fecha conexões
                servidor_socket.close()
                
            except Exception as e:
                print(f"   ❌ Erro ao conectar ao servidor {destino}: {e}")
                # Envia erro para cliente
                erro_msg = f"HTTP/1.1 502 Bad Gateway\r\n\r\nErro no proxy: {e}"
                cliente_socket.send(erro_msg.encode())
                
        except Exception as e:
            print(f"   ⚠️  Erro ao processar cliente {cliente_endereco}: {e}")
            
        finally:
            # Sempre fecha o socket do cliente
            cliente_socket.close()
            print(f"   👋 Cliente {cliente_endereco[0]} desconectado")
    
    def extrair_destino(self, requisicao_str):
        """
        Extrai o host de destino da requisição HTTP
        
        Args:
            requisicao_str: Requisição HTTP como string
            
        Returns:
            String com host:porta ou None se não encontrar
        """
        linhas = requisicao_str.split('\r\n')
        
        if not linhas:
            return None
        
        # Procura pela linha Host: no cabeçalho
        for linha in linhas:
            if linha.lower().startswith('host:'):
                return linha[5:].strip()
        
        # Tenta extrair da primeira linha
        primeira_linha = linhas[0]
        partes = primeira_linha.split(' ')
        
        if len(partes) >= 2:
            # Formato: GET http://host.com/ HTTP/1.1
            url = partes[1]
            if url.startswith('http://'):
                url = url[7:]  # Remove http://
            elif url.startswith('https://'):
                url = url[8:]  # Remove https://
            
            # Remove caminho se houver
            if '/' in url:
                url = url.split('/')[0]
            
            return url
        
        return None
    
    def parar(self):
        """Para o servidor proxy"""
        if self.proxy_socket:
            self.proxy_socket.close()
            print("🔒 Proxy parado")
        else:
            print("ℹ️  Proxy já estava parado")

def main():
    """Função principal para executar o proxy"""
    
    print("=" * 50)
    print("🌐 PROXY SIMPLES EM PYTHON")
    print("=" * 50)
    print("\nEste proxy básico encaminha requisições HTTP.")
    print("Perfect para aprender como proxies funcionam!\n")
    
    # Configurações (pode modificar aqui)
    HOST = 'localhost'  # Escuta apenas na máquina local
    # HOST = '0.0.0.0'  # Escuta em todas as interfaces (cuidado!)
    
    PORTA = 8888  # Porta padrão para proxies de teste
    
    # Cria e inicia o proxy
    proxy = ProxySimples(host=HOST, porta=PORTA)
    
    try:
        proxy.iniciar()
    except KeyboardInterrupt:
        print("\n👋 Programa encerrado")

if __name__ == "__main__":
    main()
```

### Como Usar Este Proxy:

1. **Execute o script**:
```bash
python proxy_basico.py
```

2. **Configure seu navegador**:
   - Firefox: Configurações → Rede → Configurações de conexão
   - Chrome: Extensão SwitchyOmega ou configurações do sistema
   - Configuração: Manual proxy, Host: `localhost`, Port: `8888`

3. **Acesse sites normalmente** - O proxy mostrará no terminal o que está acontecendo!

---

## 🌐 Proxy HTTP/HTTPS Avançado

### Proxy com Suporte Completo HTTP/HTTPS

**proxy_http_completo.py**
```python
import socket
import threading
import ssl
import gzip
import io
from urllib.parse import urlparse
import re

class ProxyHTTP:
    """Proxy HTTP com features avançadas"""
    
    def __init__(self, host='localhost', porta=8888):
        self.host = host
        self.porta = porta
        self.running = False
        self.cache = {}  # Cache simples
        self.blocked_sites = ['facebook.com', 'twitter.com']  # Sites bloqueados
        self.log_file = 'proxy_log.txt'
        
        print(f"🌐 Inicializando Proxy HTTP Avançado")
        print(f"📡 Endereço: {host}:{porta}")
        print(f"🚫 Sites bloqueados: {len(self.blocked_sites)}")
        print("-" * 60)
    
    def iniciar(self):
        """Inicia o proxy HTTP"""
        
        try:
            server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            server.bind((self.host, self.porta))
            server.listen(50)  # Até 50 conexões na fila
            
            self.running = True
            self.server = server
            
            print(f"✅ Proxy HTTP iniciado!")
            print(f"📞 Aguardando conexões em {self.host}:{self.porta}")
            print("\n📊 Features disponíveis:")
            print("   • Cache de conteúdo")
            print("   • Bloqueio de sites")
            print("   • Log de acesso")
            print("   • Suporte HTTP/HTTPS básico")
            print("   • Compressão gzip")
            print("-" * 60)
            
            self._log_event("PROXY_INICIADO", f"{self.host}:{self.porta}")
            
            # Loop principal
            while self.running:
                try:
                    cliente_socket, cliente_addr = server.accept()
                    
                    print(f"👤 Nova conexão: {cliente_addr[0]}:{cliente_addr[1]}")
                    
                    # Thread para cada cliente
                    thread = threading.Thread(
                        target=self._handle_request,
                        args=(cliente_socket, cliente_addr)
                    )
                    thread.daemon = True
                    thread.start()
                    
                except KeyboardInterrupt:
                    print("\n🛑 Proxy interrompido pelo usuário")
                    self.running = False
                    break
                    
        except Exception as e:
            print(f"❌ Erro fatal: {e}")
            self._log_event("ERRO_FATAL", str(e))
            
        finally:
            self.parar()
    
    def _handle_request(self, cliente_socket, cliente_addr):
        """Processa uma requisição HTTP"""
        
        try:
            # Recebe dados do cliente
            data = cliente_socket.recv(8192)  # 8KB buffer
            
            if not data:
                return
            
            # Decodifica para analisar
            request_str = data.decode('utf-8', errors='ignore')
            
            # Extrai informações da requisição
            first_line = request_str.split('\r\n')[0] if '\r\n' in request_str else request_str
            method = first_line.split(' ')[0] if ' ' in first_line else 'UNKNOWN'
            url = first_line.split(' ')[1] if len(first_line.split(' ')) > 1 else ''
            
            print(f"\n📨 [{cliente_addr[0]}] {method} {url[:50]}...")
            
            # Verifica se é HTTPS (CONNECT method)
            if method.upper() == 'CONNECT':
                self._handle_https(cliente_socket, request_str, cliente_addr)
                return
            
            # Extrai host da requisição
            host = self._extract_host(request_str)
            
            if not host:
                print(f"   ⚠️  Host não encontrado na requisição")
                cliente_socket.close()
                return
            
            # Verifica se site está bloqueado
            if self._is_blocked(host):
                print(f"   🚫 ACESSO BLOQUEADO: {host}")
                self._send_blocked_page(cliente_socket, host)
                self._log_event("BLOQUEADO", f"{cliente_addr[0]} -> {host}")
                return
            
            # Verifica cache
            cache_key = f"{method}:{url}"
            if cache_key in self.cache:
                print(f"   💾 Servindo do cache: {host}")
                cliente_socket.send(self.cache[cache_key])
                self._log_event("CACHE_HIT", f"{host}")
                return
            
            # Conecta ao servidor destino
            server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server_socket.settimeout(30)
            
            # Determina porta (80 para HTTP, 443 para HTTPS)
            port = 443 if url.startswith('https://') else 80
            
            # Conecta
            server_socket.connect((host, port))
            print(f"   🔗 Conectado a {host}:{port}")
            
            # Para HTTPS, usa SSL
            if port == 443:
                context = ssl.create_default_context()
                server_socket = context.wrap_socket(server_socket, server_hostname=host)
            
            # Envia requisição
            server_socket.send(data)
            
            # Recebe resposta
            response = self._receive_all(server_socket)
            
            # Armazena em cache (se não for muito grande)
            if len(response) < 1024 * 1024:  # Menos de 1MB
                self.cache[cache_key] = response
                print(f"   💾 Armazenado em cache: {len(response)} bytes")
            
            # Envia para cliente
            cliente_socket.send(response)
            print(f"   ✅ Resposta enviada: {len(response)} bytes")
            
            # Log
            self._log_event("ACESSO", f"{cliente_addr[0]} -> {host} ({len(response)} bytes)")
            
            # Fecha conexões
            server_socket.close()
            
        except socket.timeout:
            print(f"   ⏰ Timeout na conexão")
            self._send_error(cliente_socket, 504, "Gateway Timeout")
            
        except Exception as e:
            print(f"   ❌ Erro: {e}")
            self._send_error(cliente_socket, 500, f"Internal Server Error: {e}")
            
        finally:
            cliente_socket.close()
    
    def _handle_https(self, cliente_socket, request, cliente_addr):
        """Manipula conexões HTTPS (método CONNECT)"""
        
        try:
            # Extrai host e porta do CONNECT
            # CONNECT host.com:443 HTTP/1.1
            target = request.split(' ')[1]
            
            if ':' in target:
                host, port = target.split(':')
                port = int(port)
            else:
                host = target
                port = 443
            
            print(f"   🔐 Conexão HTTPS para {host}:{port}")
            
            # Conecta ao servidor destino
            server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server_socket.settimeout(30)
            server_socket.connect((host, port))
            
            # Cria contexto SSL
            context = ssl.create_default_context()
            ssl_socket = context.wrap_socket(server_socket, server_hostname=host)
            
            # Informa ao cliente que conexão foi estabelecida
            cliente_socket.send(b'HTTP/1.1 200 Connection Established\r\n\r\n')
            
            print(f"   ✅ Tunnel HTTPS estabelecido")
            
            # Cria tunnel bidirecional
            self._create_tunnel(cliente_socket, ssl_socket)
            
        except Exception as e:
            print(f"   ❌ Erro HTTPS: {e}")
            cliente_socket.send(b'HTTP/1.1 502 Bad Gateway\r\n\r\n')
            
        finally:
            cliente_socket.close()
    
    def _create_tunnel(self, cliente_socket, server_socket):
        """Cria um tunnel bidirecional entre cliente e servidor"""
        
        # Thread para cliente → servidor
        def forward(source, destination):
            try:
                while True:
                    data = source.recv(4096)
                    if not data:
                        break
                    destination.send(data)
            except:
                pass
        
        # Cria threads para ambas as direções
        client_to_server = threading.Thread(
            target=forward,
            args=(cliente_socket, server_socket)
        )
        
        server_to_client = threading.Thread(
            target=forward,
            args=(server_socket, cliente_socket)
        )
        
        client_to_server.start()
        server_to_client.start()
        
        # Aguarda threads terminarem
        client_to_server.join()
        server_to_client.join()
        
        # Fecha sockets
        server_socket.close()
    
    def _extract_host(self, request_str):
        """Extrai host da requisição HTTP"""
        
        # Procura header Host:
        host_match = re.search(r'Host:\s*([^\r\n]+)', request_str, re.IGNORECASE)
        if host_match:
            return host_match.group(1).strip()
        
        # Tenta extrair da URL
        first_line = request_str.split('\r\n')[0]
        if ' ' in first_line:
            url = first_line.split(' ')[1]
            if '://' in url:
                parsed = urlparse(url)
                return parsed.netloc
        
        return None
    
    def _is_blocked(self, host):
        """Verifica se host está na lista de bloqueados"""
        for blocked in self.blocked_sites:
            if blocked in host:
                return True
        return False
    
    def _send_blocked_page(self, socket, host):
        """Envia página de acesso bloqueado"""
        html = f"""
        <html>
        <head><title>Acesso Bloqueado</title></head>
        <body style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
            <h1 style="color: #d9534f;">🚫 ACESSO BLOQUEADO</h1>
            <p>O site <strong>{host}</strong> está bloqueado por este proxy.</p>
            <p>Se você acredita que isso é um erro, entre em contato com o administrador.</p>
            <hr>
            <p><small>Proxy HTTP Python</small></p>
        </body>
        </html>
        """
        
        response = (
            "HTTP/1.1 403 Forbidden\r\n"
            "Content-Type: text/html; charset=utf-8\r\n"
            f"Content-Length: {len(html)}\r\n"
            "\r\n"
            f"{html}"
        )
        
        socket.send(response.encode())
    
    def _send_error(self, socket, code, message):
        """Envia página de erro"""
        html = f"""
        <html>
        <head><title>Erro {code}</title></head>
        <body style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
            <h1 style="color: #d9534f;">Erro {code}</h1>
            <p>{message}</p>
            <hr>
            <p><small>Proxy HTTP Python</small></p>
        </body>
        </html>
        """
        
        response = (
            f"HTTP/1.1 {code} {message}\r\n"
            "Content-Type: text/html; charset=utf-8\r\n"
            f"Content-Length: {len(html)}\r\n"
            "\r\n"
            f"{html}"
        )
        
        socket.send(response.encode())
    
    def _receive_all(self, socket):
        """Recebe todos os dados de um socket"""
        data = b""
        while True:
            try:
                chunk = socket.recv(4096)
                if not chunk:
                    break
                data += chunk
            except:
                break
        return data
    
    def _log_event(self, event_type, details):
        """Registra evento no log"""
        import datetime
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        log_entry = f"[{timestamp}] {event_type}: {details}\n"
        
        try:
            with open(self.log_file, 'a', encoding='utf-8') as f:
                f.write(log_entry)
        except:
            pass  # Se falhar o log, continua funcionando
    
    def parar(self):
        """Para o proxy"""
        if hasattr(self, 'server'):
            self.server.close()
        self.running = False
        print("\n🔒 Proxy HTTP parado")

# Função principal com menu interativo
def main_menu():
    """Menu interativo para o proxy"""
    
    print("=" * 60)
    print("🌐 PROXY HTTP AVANÇADO - MENU PRINCIPAL")
    print("=" * 60)
    
    proxy = None
    
    while True:
        print("\n📋 OPÇÕES:")
        print("1. Iniciar Proxy")
        print("2. Ver Sites Bloqueados")
        print("3. Adicionar Site Bloqueado")
        print("4. Remover Site Bloqueado")
        print("5. Ver Log de Acesso")
        print("6. Limpar Cache")
        print("7. Sair")
        
        escolha = input("\n🎯 Escolha uma opção (1-7): ").strip()
        
        if escolha == '1':
            # Configurações do proxy
            host = input("Host (ENTER para localhost): ").strip() or 'localhost'
            porta_input = input("Porta (ENTER para 8888): ").strip()
            porta = int(porta_input) if porta_input else 8888
            
            # Inicia proxy
            proxy = ProxyHTTP(host=host, porta=porta)
            
            print("\n▶️  Iniciando proxy...")
            print("⚠️  Pressione Ctrl+C para parar o proxy\n")
            
            try:
                proxy.iniciar()
            except KeyboardInterrupt:
                print("\n⏹️  Proxy parado pelo usuário")
                proxy.parar()
                
        elif escolha == '2':
            if proxy and proxy.blocked_sites:
                print("\n🚫 SITES BLOQUEADOS:")
                for i, site in enumerate(proxy.blocked_sites, 1):
                    print(f"  {i}. {site}")
            else:
                print("\nℹ️  Nenhum site bloqueado configurado")
                
        elif escolha == '3':
            site = input("\n🌐 Site para bloquear (ex: facebook.com): ").strip()
            if site:
                if not hasattr(proxy, 'blocked_sites'):
                    proxy = ProxyHTTP()
                proxy.blocked_sites.append(site)
                print(f"✅ {site} adicionado à lista de bloqueados")
                
        elif escolha == '4':
            if proxy and proxy.blocked_sites:
                print("\n🚫 SITES BLOQUEADOS:")
                for i, site in enumerate(proxy.blocked_sites, 1):
                    print(f"  {i}. {site}")
                
                try:
                    num = int(input("\nNúmero para remover: "))
                    if 1 <= num <= len(proxy.blocked_sites):
                        removido = proxy.blocked_sites.pop(num - 1)
                        print(f"✅ {removido} removido da lista")
                except:
                    print("❌ Número inválido")
            else:
                print("\nℹ️  Nenhum site bloqueado para remover")
                
        elif escolha == '5':
            try:
                with open('proxy_log.txt', 'r', encoding='utf-8') as f:
                    logs = f.readlines()
                    if logs:
                        print("\n📝 ÚLTIMOS LOGS:")
                        for log in logs[-20:]:  # Mostra últimos 20 logs
                            print(f"  {log.strip()}")
                    else:
                        print("\nℹ️  Nenhum log encontrado")
            except:
                print("\n❌ Erro ao ler arquivo de log")
                
        elif escolha == '6':
            if proxy:
                proxy.cache.clear()
                print("✅ Cache limpo")
            else:
                print("ℹ️  Proxy não está ativo")
                
        elif escolha == '7':
            print("\n👋 Até logo!")
            break
            
        else:
            print("❌ Opção inválida")

if __name__ == "__main__":
    main_menu()
```

---

## 🧦 Proxy SOCKS

### Proxy SOCKS5 em Python

**proxy_socks5.py**
```python
import socket
import threading
import struct

class Socks5Proxy:
    """Implementação básica de proxy SOCKS5"""
    
    # Versão SOCKS5
    VERSION = 0x05
    
    # Métodos de autenticação
    METHOD_NO_AUTH = 0x00
    METHOD_GSSAPI = 0x01
    METHOD_USERPASS = 0x02
    METHOD_NO_ACCEPTABLE = 0xFF
    
    # Comandos
    CMD_CONNECT = 0x01
    CMD_BIND = 0x02
    CMD_UDP_ASSOCIATE = 0x03
    
    # Tipos de endereço
    ATYP_IPV4 = 0x01
    ATYP_DOMAIN = 0x03
    ATYP_IPV6 = 0x04
    
    def __init__(self, host='localhost', porta=1080):
        self.host = host
        self.porta = porta
        self.running = False
        
        print(f"🧦 Inicializando Proxy SOCKS5")
        print(f"📡 Endereço: {host}:{porta}")
        print("-" * 50)
    
    def iniciar(self):
        """Inicia o proxy SOCKS5"""
        
        try:
            server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            server.bind((self.host, self.porta))
            server.listen(10)
            
            self.running = True
            self.server = server
            
            print(f"✅ Proxy SOCKS5 iniciado em {self.host}:{self.porta}")
            print("📞 Aguardando conexões...")
            print("\n🔧 Como usar:")
            print("   Navegador: Use extensão FoxyProxy ou similar")
            print("   Aplicativos: Configure para usar SOCKS5 proxy")
            print(f"   Endereço: {self.host}")
            print(f"   Porta: {self.porta}")
            print("-" * 50)
            
            while self.running:
                try:
                    cliente_socket, cliente_addr = server.accept()
                    print(f"👤 Nova conexão SOCKS5 de {cliente_addr[0]}:{cliente_addr[1]}")
                    
                    thread = threading.Thread(
                        target=self._handle_client,
                        args=(cliente_socket, cliente_addr)
                    )
                    thread.daemon = True
                    thread.start()
                    
                except KeyboardInterrupt:
                    print("\n🛑 Proxy interrompido pelo usuário")
                    self.running = False
                    break
                    
        except Exception as e:
            print(f"❌ Erro: {e}")
            
        finally:
            self.parar()
    
    def _handle_client(self, cliente_socket, cliente_addr):
        """Processa um cliente SOCKS5"""
        
        try:
            # 1. Negociação de método de autenticação
            # Cliente envia: [VER, NMETHODS, METHODS...]
            header = cliente_socket.recv(2)
            if len(header) < 2:
                return
            
            ver, nmethods = struct.unpack('!BB', header)
            
            if ver != self.VERSION:
                print(f"   ⚠️  Versão SOCKS inválida: {ver}")
                return
            
            # Lê métodos suportados
            methods = self._recv_exactly(cliente_socket, nmethods)
            if not methods:
                return
            
            # Seleciona método (sempre sem autenticação por enquanto)
            method = self.METHOD_NO_AUTH if self.METHOD_NO_AUTH in methods else self.METHOD_NO_ACCEPTABLE
            
            # Responde com método selecionado
            # Servidor envia: [VER, METHOD]
            resposta = struct.pack('!BB', self.VERSION, method)
            cliente_socket.send(resposta)
            
            if method == self.METHOD_NO_ACCEPTABLE:
                print(f"   ❌ Nenhum método de autenticação aceitável")
                return
            
            # 2. Recebe requisição
            # Formato: [VER, CMD, RSV, ATYP, DST.ADDR, DST.PORT]
            request = self._recv_exactly(cliente_socket, 4)
            if not request:
                return
            
            ver, cmd, rsv, atyp = struct.unpack('!BBBB', request)
            
            if ver != self.VERSION:
                return
            
            # Processa endereço de destino
            dest_addr = None
            dest_port = None
            
            if atyp == self.ATYP_IPV4:
                # 4 bytes para IPv4
                addr_data = self._recv_exactly(cliente_socket, 4)
                if addr_data:
                    dest_addr = socket.inet_ntoa(addr_data)
            
            elif atyp == self.ATYP_DOMAIN:
                # 1 byte para tamanho + string do domínio
                length_data = self._recv_exactly(cliente_socket, 1)
                if length_data:
                    length = struct.unpack('!B', length_data)[0]
                    domain_data = self._recv_exactly(cliente_socket, length)
                    if domain_data:
                        dest_addr = domain_data.decode('utf-8')
            
            elif atyp == self.ATYP_IPV6:
                # 16 bytes para IPv6
                addr_data = self._recv_exactly(cliente_socket, 16)
                if addr_data:
                    # IPv6 em formato string simplificado
                    dest_addr = socket.inet_ntop(socket.AF_INET6, addr_data)
            
            # Lê porta (2 bytes)
            port_data = self._recv_exactly(cliente_socket, 2)
            if port_data:
                dest_port = struct.unpack('!H', port_data)[0]
            
            if not dest_addr or not dest_port:
                print(f"   ❌ Endereço ou porta inválidos")
                return
            
            print(f"   🌐 Cliente quer conectar a: {dest_addr}:{dest_port}")
            
            # 3. Processa comando
            if cmd == self.CMD_CONNECT:
                self._handle_connect(cliente_socket, dest_addr, dest_port, atyp)
            elif cmd == self.CMD_BIND:
                self._handle_bind(cliente_socket)
            elif cmd == self.CMD_UDP_ASSOCIATE:
                self._handle_udp_associate(cliente_socket)
            else:
                print(f"   ❌ Comando não suportado: {cmd}")
                # Resposta: comando não suportado
                resposta = struct.pack('!BBBB', self.VERSION, 0x07, 0x00, self.ATYP_IPV4)
                resposta += socket.inet_aton('0.0.0.0') + struct.pack('!H', 0)
                cliente_socket.send(resposta)
                
        except Exception as e:
            print(f"   ❌ Erro no cliente {cliente_addr[0]}: {e}")
            
        finally:
            cliente_socket.close()
            print(f"   👋 Cliente {cliente_addr[0]} desconectado")
    
    def _handle_connect(self, cliente_socket, dest_addr, dest_port, atyp):
        """Lida com comando CONNECT (conexão TCP)"""
        
        try:
            # Tenta conectar ao destino
            dest_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            dest_socket.settimeout(10)
            
            print(f"   🔗 Conectando a {dest_addr}:{dest_port}...")
            dest_socket.connect((dest_addr, dest_port))
            
            # Pega endereço local do socket
            local_addr, local_port = dest_socket.getsockname()
            
            print(f"   ✅ Conectado! Retransmitindo dados...")
            
            # Resposta de sucesso
            # [VER, REP, RSV, ATYP, BND.ADDR, BND.PORT]
            resposta = struct.pack('!BBBB', self.VERSION, 0x00, 0x00, self.ATYP_IPV4)
            resposta += socket.inet_aton(local_addr) + struct.pack('!H', local_port)
            cliente_socket.send(resposta)
            
            # Cria tunnel bidirecional
            self._create_tunnel(cliente_socket, dest_socket)
            
        except Exception as e:
            print(f"   ❌ Falha ao conectar: {e}")
            # Resposta de falha
            resposta = struct.pack('!BBBB', self.VERSION, 0x01, 0x00, self.ATYP_IPV4)
            resposta += socket.inet_aton('0.0.0.0') + struct.pack('!H', 0)
            cliente_socket.send(resposta)
    
    def _handle_bind(self, cliente_socket):
        """Lida com comando BIND (não implementado)"""
        print("   ⚠️  Comando BIND não implementado")
        resposta = struct.pack('!BBBB', self.VERSION, 0x07, 0x00, self.ATYP_IPV4)
        resposta += socket.inet_aton('0.0.0.0') + struct.pack('!H', 0)
        cliente_socket.send(resposta)
    
    def _handle_udp_associate(self, cliente_socket):
        """Lida com comando UDP ASSOCIATE (não implementado)"""
        print("   ⚠️  Comando UDP ASSOCIATE não implementado")
        resposta = struct.pack('!BBBB', self.VERSION, 0x07, 0x00, self.ATYP_IPV4)
        resposta += socket.inet_aton('0.0.0.0') + struct.pack('!H', 0)
        cliente_socket.send(resposta)
    
    def _create_tunnel(self, socket_a, socket_b):
        """Cria tunnel bidirecional entre dois sockets"""
        
        def forward(src, dst, desc):
            try:
                while True:
                    data = src.recv(4096)
                    if not data:
                        break
                    dst.send(data)
                    print(f"   📨 {desc}: {len(data)} bytes")
            except:
                pass
        
        # Threads para ambas as direções
        thread_a = threading.Thread(
            target=forward,
            args=(socket_a, socket_b, "Cliente → Servidor")
        )
        thread_b = threading.Thread(
            target=forward,
            args=(socket_b, socket_a, "Servidor → Cliente")
        )
        
        thread_a.daemon = True
        thread_b.daemon = True
        
        thread_a.start()
        thread_b.start()
        
        # Aguarda threads
        thread_a.join()
        thread_b.join()
        
        # Fecha sockets
        socket_b.close()
    
    def _recv_exactly(self, socket, n):
        """Recebe exatamente n bytes"""
        data = b""
        while len(data) < n:
            chunk = socket.recv(n - len(data))
            if not chunk:
                return None
            data += chunk
        return data
    
    def parar(self):
        """Para o proxy"""
        if hasattr(self, 'server'):
            self.server.close()
        self.running = False
        print("\n🔒 Proxy SOCKS5 parado")

def testar_proxy_socks():
    """Testa o proxy SOCKS5"""
    
    print("🧦 TESTE DE PROXY SOCKS5")
    print("=" * 50)
    
    proxy = Socks5Proxy(porta=1080)
    
    try:
        # Inicia em thread separada
        import threading
        thread = threading.Thread(target=proxy.iniciar)
        thread.daemon = True
        thread.start()
        
        print("Proxy iniciado em segundo plano.")
        print("Para testar, configure um cliente SOCKS5 para:")
        print("  Host: localhost")
        print("  Port: 1080")
        print("\nPressione Enter para parar...")
        input()
        
    except KeyboardInterrupt:
        print("\n👋 Teste encerrado")
    finally:
        proxy.parar()

if __name__ == "__main__":
    testar_proxy_socks()
```

---

## 🔐 Proxy com Autenticação

### Proxy HTTP com Login e Senha

**proxy_com_auth.py**
```python
import socket
import threading
import base64
import hashlib
from datetime import datetime, timedelta

class ProxyComAutenticacao:
    """Proxy HTTP com autenticação de usuários"""
    
    def __init__(self, host='localhost', porta=8888):
        self.host = host
        self.porta = porta
        self.running = False
        
        # Banco de dados de usuários (em produção, use um banco real)
        self.usuarios = {
            'admin': {
                'senha': 'admin123',  # Em produção, use hash!
                'nivel': 'admin'
            },
            'usuario': {
                'senha': 'senha123',
                'nivel': 'comum'
            },
            'visitante': {
                'senha': 'visitante',
                'nivel': 'restrito'
            }
        }
        
        # Sessões ativas
        self.sessoes = {}
        
        # Regras de acesso por nível
        self.regras_acesso = {
            'admin': [],  # Acesso a tudo
            'comum': ['facebook.com', 'twitter.com'],  # Sites bloqueados
            'restrito': ['facebook.com', 'twitter.com', 'youtube.com', 'netflix.com']
        }
        
        print(f"🔐 Inicializando Proxy com Autenticação")
        print(f"📡 Endereço: {host}:{porta}")
        print(f"👥 Usuários cadastrados: {len(self.usuarios)}")
        print("-" * 60)
    
    def iniciar(self):
        """Inicia o proxy com autenticação"""
        
        try:
            server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            server.bind((self.host, self.porta))
            server.listen(20)
            
            self.running = True
            self.server = server
            
            print(f"✅ Proxy com autenticação iniciado!")
            print(f"📞 Aguardando conexões em {self.host}:{self.porta}")
            print("\n🔐 Como autenticar:")
            print("   Use no navegador: usuario:senha@localhost:8888")
            print("   Ou configure proxy com autenticação básica")
            print("\n👤 Credenciais de teste:")
            for usuario in self.usuarios:
                print(f"   • {usuario}:{self.usuarios[usuario]['senha']}")
            print("-" * 60)
            
            while self.running:
                try:
                    cliente_socket, cliente_addr = server.accept()
                    
                    print(f"👤 Nova conexão de {cliente_addr[0]}:{cliente_addr[1]}")
                    
                    thread = threading.Thread(
                        target=self._handle_client,
                        args=(cliente_socket, cliente_addr)
                    )
                    thread.daemon = True
                    thread.start()
                    
                except KeyboardInterrupt:
                    print("\n🛑 Proxy interrompido pelo usuário")
                    self.running = False
                    break
                    
        except Exception as e:
            print(f"❌ Erro fatal: {e}")
            
        finally:
            self.parar()
    
    def _handle_client(self, cliente_socket, cliente_addr):
        """Processa um cliente"""
        
        try:
            # Recebe requisição
            data = cliente_socket.recv(4096)
            
            if not data:
                return
            
            request_str = data.decode('utf-8', errors='ignore')
            
            # Verifica autenticação
            usuario = self._extrair_autenticacao(request_str)
            
            if not usuario:
                # Solicita autenticação
                print(f"   🔒 Solicitando autenticação para {cliente_addr[0]}")
                self._solicitar_autenticacao(cliente_socket)
                return
            
            print(f"   ✅ Usuário autenticado: {usuario}")
            
            # Verifica se tem acesso ao site
            destino = self._extrair_host(request_str)
            
            if destino and not self._tem_acesso(usuario, destino):
                print(f"   🚫 Acesso negado: {usuario} → {destino}")
                self._enviar_acesso_negado(cliente_socket, usuario, destino)
                return
            
            # Processa requisição normalmente
            self._processar_requisicao(cliente_socket, data, usuario, destino)
            
        except Exception as e:
            print(f"   ❌ Erro: {e}")
            
        finally:
            cliente_socket.close()
    
    def _extrair_autenticacao(self, request_str):
        """Extrai credenciais da requisição HTTP"""
        
        # Procura header Authorization
        for linha in request_str.split('\r\n'):
            if linha.lower().startswith('authorization:'):
                partes = linha.split(' ')
                if len(partes) >= 3 and partes[1].lower() == 'basic':
                    # Decodifica Base64
                    try:
                        credenciais = base64.b64decode(partes[2]).decode('utf-8')
                        usuario, senha = credenciais.split(':', 1)
                        
                        # Verifica credenciais
                        if usuario in self.usuarios and self.usuarios[usuario]['senha'] == senha:
                            return usuario
                    except:
                        pass
        
        return None
    
    def _solicitar_autenticacao(self, socket):
        """Solicita autenticação ao cliente"""
        
        resposta = (
            "HTTP/1.1 407 Proxy Authentication Required\r\n"
            "Proxy-Authenticate: Basic realm=\"Proxy Python\"\r\n"
            "Content-Type: text/html; charset=utf-8\r\n"
            "Content-Length: 500\r\n"
            "\r\n"
            "<html>"
            "<head><title>Autenticação Necessária</title></head>"
            "<body style='font-family: Arial, sans-serif; text-align: center; padding: 50px;'>"
            "<h1>🔒 Autenticação Necessária</h1>"
            "<p>Este proxy requer autenticação.</p>"
            "<p>Configure seu navegador para usar:</p>"
            "<p><code>usuario:senha@localhost:8888</code></p>"
            "<hr>"
            "<p><small>Proxy Python com Autenticação</small></p>"
            "</body>"
            "</html>"
        )
        
        socket.send(resposta.encode())
    
    def _extrair_host(self, request_str):
        """Extrai host da requisição"""
        for linha in request_str.split('\r\n'):
            if linha.lower().startswith('host:'):
                return linha[5:].strip()
        return None
    
    def _tem_acesso(self, usuario, host):
        """Verifica se usuário tem acesso ao host"""
        
        if usuario not in self.usuarios:
            return False
        
        nivel = self.usuarios[usuario]['nivel']
        
        # Admin tem acesso a tudo
        if nivel == 'admin':
            return True
        
        # Verifica regras para o nível
        if nivel in self.regras_acesso:
            for site_bloqueado in self.regras_acesso[nivel]:
                if site_bloqueado in host:
                    return False
        
        return True
    
    def _enviar_acesso_negado(self, socket, usuario, destino):
        """Envia página de acesso negado"""
        
        html = f"""
        <html>
        <head><title>Acesso Negado</title></head>
        <body style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
            <h1 style="color: #d9534f;">🚫 ACESSO NEGADO</h1>
            <p>Olá <strong>{usuario}</strong>,</p>
            <p>Você não tem permissão para acessar <strong>{destino}</strong>.</p>
            <p>Seu nível de acesso não permite este site.</p>
            <hr>
            <p><small>Proxy Python com Controle de Acesso</small></p>
        </body>
        </html>
        """
        
        resposta = (
            "HTTP/1.1 403 Forbidden\r\n"
            "Content-Type: text/html; charset=utf-8\r\n"
            f"Content-Length: {len(html)}\r\n"
            "\r\n"
            f"{html}"
        )
        
        socket.send(resposta.encode())
    
    def _processar_requisicao(self, cliente_socket, data, usuario, destino):
        """Processa requisição HTTP"""
        
        try:
            # Conecta ao destino
            if ':' in destino:
                host, porta_str = destino.split(':')
                porta = int(porta_str)
            else:
                host = destino
                porta = 80
            
            servidor_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            servidor_socket.settimeout(30)
            servidor_socket.connect((host, porta))
            
            # Envia requisição
            servidor_socket.send(data)
            
            # Recebe resposta
            resposta_total = b""
            while True:
                resposta = servidor_socket.recv(4096)
                if not resposta:
                    break
                resposta_total += resposta
            
            # Envia para cliente
            cliente_socket.send(resposta_total)
            
            # Log
            print(f"   📊 {usuario} acessou {destino} ({len(resposta_total)} bytes)")
            
            servidor_socket.close()
            
        except Exception as e:
            print(f"   ❌ Erro ao processar requisição: {e}")
            erro = f"HTTP/1.1 502 Bad Gateway\r\n\r\nErro: {e}"
            cliente_socket.send(erro.encode())
    
    def parar(self):
        """Para o proxy"""
        if hasattr(self, 'server'):
            self.server.close()
        self.running = False
        print("\n🔒 Proxy com autenticação parado")

def gerenciar_usuarios():
    """Interface para gerenciar usuários do proxy"""
    
    proxy = ProxyComAutenticacao()
    
    while True:
        print("\n👥 GERENCIAMENTO DE USUÁRIOS")
        print("=" * 40)
        print("1. Listar usuários")
        print("2. Adicionar usuário")
        print("3. Remover usuário")
        print("4. Ver regras de acesso")
        print("5. Voltar")
        
        opcao = input("\nEscolha: ").strip()
        
        if opcao == '1':
            print("\n📋 USUÁRIOS CADASTRADOS:")
            for usuario, info in proxy.usuarios.items():
                print(f"  • {usuario} (nível: {info['nivel']})")
                
        elif opcao == '2':
            usuario = input("Novo usuário: ").strip()
            senha = input("Senha: ").strip()
            nivel = input("Nível (admin/comum/restrito): ").strip().lower()
            
            if nivel not in ['admin', 'comum', 'restrito']:
                nivel = 'comum'
            
            proxy.usuarios[usuario] = {'senha': senha, 'nivel': nivel}
            print(f"✅ Usuário {usuario} adicionado!")
            
        elif opcao == '3':
            usuario = input("Usuário para remover: ").strip()
            if usuario in proxy.usuarios:
                del proxy.usuarios[usuario]
                print(f"✅ Usuário {usuario} removido!")
            else:
                print("❌ Usuário não encontrado")
                
        elif opcao == '4':
            print("\n📜 REGRAS DE ACESSO:")
            for nivel, sites in proxy.regras_acesso.items():
                print(f"\n  {nivel.upper()}:")
                if sites:
                    for site in sites:
                        print(f"    - {site}")
                else:
                    print(f"    - Acesso total")
                    
        elif opcao == '5':
            break

if __name__ == "__main__":
    print("🔐 PROXY COM AUTENTICAÇÃO")
    print("=" * 60)
    
    while True:
        print("\n📋 MENU PRINCIPAL:")
        print("1. Iniciar Proxy")
        print("2. Gerenciar Usuários")
        print("3. Sair")
        
        escolha = input("\nEscolha: ").strip()
        
        if escolha == '1':
            host = input("Host (ENTER para localhost): ").strip() or 'localhost'
            porta_input = input("Porta (ENTER para 8888): ").strip()
            porta = int(porta_input) if porta_input else 8888
            
            proxy = ProxyComAutenticacao(host=host, porta=porta)
            
            print("\n▶️  Iniciando proxy...")
            print("⚠️  Pressione Ctrl+C para parar\n")
            
            try:
                proxy.iniciar()
            except KeyboardInterrupt:
                print("\n⏹️  Proxy parado")
                
        elif escolha == '2':
            gerenciar_usuarios()
            
        elif escolha == '3':
            print("\n👋 Até logo!")
            break
```

---

## 💾 Proxy com Cache

### Proxy com Sistema de Cache Avançado

**proxy_com_cache.py**
```python
import socket
import threading
import sqlite3
import pickle
import zlib
import hashlib
from datetime import datetime, timedelta
import os

class ProxyComCache:
    """Proxy HTTP com sistema de cache persistente"""
    
    def __init__(self, host='localhost', porta=8888, cache_dir='cache_proxy'):
        self.host = host
        self.porta = porta
        self.running = False
        self.cache_dir = cache_dir
        
        # Cria diretório de cache se não existir
        if not os.path.exists(cache_dir):
            os.makedirs(cache_dir)
        
        # Inicializa banco de dados de cache
        self._init_cache_db()
        
        # Estatísticas
        self.stats = {
            'requisicoes': 0,
            'cache_hits': 0,
            'cache_misses': 0,
            'bytes_saved': 0
        }
        
        print(f"💾 Inicializando Proxy com Cache")
        print(f"📡 Endereço: {host}:{porta}")
        print(f"📂 Cache em: {cache_dir}")
        print(f"🗄️  Tamanho do cache: {self._get_cache_size()} itens")
        print("-" * 60)
    
    def _init_cache_db(self):
        """Inicializa banco de dados SQLite para cache"""
        
        self.db = sqlite3.connect(os.path.join(self.cache_dir, 'cache.db'))
        cursor = self.db.cursor()
        
        # Tabela de cache
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS cache (
                key TEXT PRIMARY KEY,
                data BLOB,
                headers TEXT,
                timestamp DATETIME,
                expires DATETIME,
                hits INTEGER DEFAULT 0,
                size INTEGER,
                url TEXT,
                content_type TEXT
            )
        ''')
        
        # Índices para performance
        cursor.execute('CREATE INDEX IF NOT EXISTS idx_timestamp ON cache(timestamp)')
        cursor.execute('CREATE INDEX IF NOT EXISTS idx_expires ON cache(expires)')
        cursor.execute('CREATE INDEX IF NOT EXISTS idx_url ON cache(url)')
        
        self.db.commit()
    
    def iniciar(self):
        """Inicia o proxy com cache"""
        
        try:
            server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            server.bind((self.host, self.porta))
            server.listen(20)
            
            self.running = True
            self.server = server
            
            print(f"✅ Proxy com cache iniciado!")
            print(f"📞 Aguardando conexões em {self.host}:{self.porta}")
            print("\n🚀 Features do cache:")
            print("   • Cache persistente em disco")
            print("   • Compressão para economizar espaço")
            print("   • Expiração automática")
            print("   • Estatísticas detalhadas")
            print("   • Limpeza automática de cache antigo")
            print("-" * 60)
            
            # Limpa cache antigo
            self._limpar_cache_antigo()
            
            while self.running:
                try:
                    cliente_socket, cliente_addr = server.accept()
                    
                    self.stats['requisicoes'] += 1
                    print(f"\n📊 Req #{self.stats['requisicoes']} de {cliente_addr[0]}")
                    
                    thread = threading.Thread(
                        target=self._handle_request,
                        args=(cliente_socket, cliente_addr)
                    )
                    thread.daemon = True
                    thread.start()
                    
                except KeyboardInterrupt:
                    print("\n🛑 Proxy interrompido pelo usuário")
                    self.running = False
                    break
                    
        except Exception as e:
            print(f"❌ Erro fatal: {e}")
            
        finally:
            self.parar()
    
    def _handle_request(self, cliente_socket, cliente_addr):
        """Processa uma requisição com cache"""
        
        try:
            # Recebe requisição
            data = cliente_socket.recv(8192)
            
            if not data:
                return
            
            request_str = data.decode('utf-8', errors='ignore')
            
            # Extrai URL
            url = self._extrair_url(request_str)
            host = self._extrair_host(request_str)
            
            if not url or not host:
                print(f"   ⚠️  URL ou Host inválido")
                cliente_socket.close()
                return
            
            print(f"   🌐 URL: {url[:80]}...")
            
            # Verifica se deve usar cache
            # Algumas requisições não devem ser cacheadas
            if self._deve_cachear(request_str, url):
                # Tenta obter do cache
                cached_data = self._obter_do_cache(url, request_str)
                
                if cached_data:
                    # Cache HIT!
                    self.stats['cache_hits'] += 1
                    print(f"   💾 CACHE HIT! Salvando {len(cached_data)} bytes")
                    
                    cliente_socket.send(cached_data)
                    cliente_socket.close()
                    return
            
            # Cache MISS
            self.stats['cache_misses'] += 1
            print(f"   🔄 CACHE MISS, buscando do servidor...")
            
            # Conecta ao servidor
            porta = 443 if url.startswith('https://') else 80
            
            try:
                servidor_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                servidor_socket.settimeout(30)
                servidor_socket.connect((host, porta))
                
                # Envia requisição
                servidor_socket.send(data)
                
                # Recebe resposta
                resposta_total = self._receive_all(servidor_socket)
                
                # Verifica se deve armazenar no cache
                if self._pode_cachear_resposta(resposta_total):
                    self._armazenar_no_cache(url, request_str, resposta_total)
                    print(f"   💾 Armazenado no cache: {len(resposta_total)} bytes")
                
                # Envia para cliente
                cliente_socket.send(resposta_total)
                print(f"   ✅ Enviado para cliente: {len(resposta_total)} bytes")
                
                servidor_socket.close()
                
            except Exception as e:
                print(f"   ❌ Erro ao conectar ao servidor: {e}")
                erro_msg = f"HTTP/1.1 502 Bad Gateway\r\n\r\nErro: {e}"
                cliente_socket.send(erro_msg.encode())
                
        except Exception as e:
            print(f"   ❌ Erro geral: {e}")
            
        finally:
            cliente_socket.close()
            
            # Mostra estatísticas periódicamente
            if self.stats['requisicoes'] % 10 == 0:
                self._mostrar_estatisticas()
    
    def _extrair_url(self, request_str):
        """Extrai URL completa da requisição"""
        linhas = request_str.split('\r\n')
        if linhas and ' ' in linhas[0]:
            partes = linhas[0].split(' ')
            if len(partes) >= 2:
                return partes[1]
        return None
    
    def _extrair_host(self, request_str):
        """Extrai host da requisição"""
        for linha in request_str.split('\r\n'):
            if linha.lower().startswith('host:'):
                return linha[5:].strip()
        return None
    
    def _deve_cachear(self, request_str, url):
        """Verifica se a requisição deve ser cacheada"""
        
        # Não cachear métodos que não sejam GET
        if not request_str.startswith('GET'):
            return False
        
        # Não cachear URLs com query strings dinâmicas
        if '?' in url and ('session' in url or 'token' in url):
            return False
        
        # Não cachear certos tipos de conteúdo
        nao_cachear = ['/login', '/logout', '/admin', '/api/']
        for path in nao_cachear:
            if path in url:
                return False
        
        return True
    
    def _pode_cachear_resposta(self, resposta):
        """Verifica se a resposta pode ser cacheada"""
        
        try:
            resposta_str = resposta.decode('utf-8', errors='ignore')
            headers_end = resposta_str.find('\r\n\r\n')
            if headers_end == -1:
                return False
            
            headers = resposta_str[:headers_end]
            
            # Verifica código de status
            if not resposta_str.startswith('HTTP/1.1 200'):
                return False
            
            # Verifica headers de cache
            cache_headers = ['cache-control', 'pragma', 'expires']
            for header in cache_headers:
                if f'{header}: no-cache' in headers.lower() or \
                   f'{header}: no-store' in headers.lower():
                    return False
            
            return True
            
        except:
            return False
    
    def _obter_do_cache(self, url, request_str):
        """Obtém resposta do cache"""
        
        try:
            cursor = self.db.cursor()
            
            # Cria chave única para esta requisição
            chave = hashlib.md5((url + request_str[:100]).encode()).hexdigest()
            
            cursor.execute('''
                SELECT data, expires FROM cache 
                WHERE key = ? AND (expires IS NULL OR expires > ?)
            ''', (chave, datetime.now()))
            
            resultado = cursor.fetchone()
            
            if resultado:
                data, expires = resultado
                
                # Atualiza contador de hits
                cursor.execute('UPDATE cache SET hits = hits + 1 WHERE key = ?', (chave,))
                self.db.commit()
                
                # Descomprime se necessário
                try:
                    return zlib.decompress(data)
                except:
                    return data
            
        except Exception as e:
            print(f"   ⚠️  Erro ao acessar cache: {e}")
        
        return None
    
    def _armazenar_no_cache(self, url, request_str, resposta):
        """Armazena resposta no cache"""
        
        try:
            cursor = self.db.cursor()
            
            # Cria chave
            chave = hashlib.md5((url + request_str[:100]).encode()).hexdigest()
            
            # Analisa resposta para determinar expiração
            expires = self._calcular_expiração(resposta)
            
            # Comprime dados para economizar espaço
            dados_comprimidos = zlib.compress(resposta, level=6)
            
            # Extrai informações úteis
            content_type = self._extrair_content_type(resposta)
            
            cursor.execute('''
                INSERT OR REPLACE INTO cache 
                (key, data, headers, timestamp, expires, hits, size, url, content_type)
                VALUES (?, ?, ?, ?, ?, 0, ?, ?, ?)
            ''', (
                chave,
                dados_comprimidos,
                '',  # Headers poderiam ser armazenados separadamente
                datetime.now(),
                expires,
                len(resposta),
                url,
                content_type
            ))
            
            self.db.commit()
            
            # Atualiza estatísticas de bytes salvos
            self.stats['bytes_saved'] += len(resposta)
            
        except Exception as e:
            print(f"   ⚠️  Erro ao armazenar no cache: {e}")
    
    def _calcular_expiração(self, resposta):
        """Calcula data de expiração baseada em headers"""
        
        try:
            resposta_str = resposta.decode('utf-8', errors='ignore')
            headers_end = resposta_str.find('\r\n\r\n')
            if headers_end == -1:
                return None
            
            headers = resposta_str[:headers_end].lower()
            
            # Verifica Cache-Control
            import re
            max_age_match = re.search(r'max-age=(\d+)', headers)
            if max_age_match:
                seconds = int(max_age_match.group(1))
                return datetime.now() + timedelta(seconds=seconds)
            
            # Expiração padrão: 1 hora
            return datetime.now() + timedelta(hours=1)
            
        except:
            return datetime.now() + timedelta(hours=1)
    
    def _extrair_content_type(self, resposta):
        """Extrai Content-Type da resposta"""
        
        try:
            resposta_str = resposta.decode('utf-8', errors='ignore')
            headers_end = resposta_str.find('\r\n\r\n')
            if headers_end == -1:
                return 'unknown'
            
            headers = resposta_str[:headers_end].lower()
            
            import re
            content_match = re.search(r'content-type:\s*([^\r\n]+)', headers)
            if content_match:
                return content_match.group(1).split(';')[0].strip()
            
        except:
            pass
        
        return 'unknown'
    
    def _receive_all(self, socket):
        """Recebe todos os dados de um socket"""
        data = b""
        while True:
            try:
                chunk = socket.recv(4096)
                if not chunk:
                    break
                data += chunk
            except:
                break
        return data
    
    def _limpar_cache_antigo(self):
        """Remove itens expirados do cache"""
        
        try:
            cursor = self.db.cursor()
            
            # Remove itens expirados
            cursor.execute('DELETE FROM cache WHERE expires < ?', (datetime.now(),))
            
            # Remove itens muito antigos (mais de 30 dias)
            cursor.execute('''
                DELETE FROM cache 
                WHERE timestamp < ? AND hits = 0
            ''', (datetime.now() - timedelta(days=30),))
            
            # Remove excesso de itens (mantém apenas 1000 mais recentes)
            cursor.execute('''
                DELETE FROM cache 
                WHERE key NOT IN (
                    SELECT key FROM cache 
                    ORDER BY timestamp DESC 
                    LIMIT 1000
                )
            ''')
            
            removidos = cursor.rowcount
            self.db.commit()
            
            if removidos:
                print(f"   🗑️  Limpeza de cache: {removidos} itens removidos")
                
        except Exception as e:
            print(f"   ⚠️  Erro ao limpar cache: {e}")
    
    def _get_cache_size(self):
        """Retorna número de itens no cache"""
        
        try:
            cursor = self.db.cursor()
            cursor.execute('SELECT COUNT(*) FROM cache')
            return cursor.fetchone()[0]
        except:
            return 0
    
    def _mostrar_estatisticas(self):
        """Mostra estatísticas do proxy"""
        
        hit_rate = (self.stats['cache_hits'] / max(self.stats['requisicoes'], 1)) * 100
        
        print(f"\n📈 ESTATÍSTICAS DO CACHE:")
        print(f"   Requisições totais: {self.stats['requisicoes']}")
        print(f"   Cache hits: {self.stats['cache_hits']}")
        print(f"   Cache misses: {self.stats['cache_misses']}")
        print(f"   Taxa de acerto: {hit_rate:.1f}%")
        print(f"   Bytes economizados: {self.stats['bytes_saved']:,}")
        print(f"   Itens no cache: {self._get_cache_size()}")
    
    def parar(self):
        """Para o proxy"""
        
        if hasattr(self, 'server'):
            self.server.close()
        
        if hasattr(self, 'db'):
            self.db.close()
        
        self.running = False
        
        # Mostra estatísticas finais
        print("\n" + "=" * 50)
        print("📊 ESTATÍSTICAS FINAIS:")
        self._mostrar_estatisticas()
        print("🔒 Proxy com cache parado")

def menu_cache():
    """Menu interativo para proxy com cache"""
    
    print("💾 PROXY COM SISTEMA DE CACHE")
    print("=" * 60)
    
    proxy = None
    
    while True:
        print("\n📋 MENU:")
        print("1. Iniciar Proxy com Cache")
        print("2. Ver Estatísticas do Cache")
        print("3. Limpar Cache Completamente")
        print("4. Analisar Conteúdo do Cache")
        print("5. Configurar Cache")
        print("6. Sair")
        
        escolha = input("\n🎯 Escolha: ").strip()
        
        if escolha == '1':
            host = input("Host (ENTER para localhost): ").strip() or 'localhost'
            porta_input = input("Porta (ENTER para 8888): ").strip()
            porta = int(porta_input) if porta_input else 8888
            
            cache_dir = input("Diretório de cache (ENTER para 'cache_proxy'): ").strip()
            cache_dir = cache_dir if cache_dir else 'cache_proxy'
            
            proxy = ProxyComCache(host=host, porta=porta, cache_dir=cache_dir)
            
            print("\n▶️  Iniciando proxy com cache...")
            print("⚠️  Pressione Ctrl+C para parar\n")
            
            try:
                proxy.iniciar()
            except KeyboardInterrupt:
                print("\n⏹️  Proxy parado")
                
        elif escolha == '2':
            if proxy:
                proxy._mostrar_estatisticas()
            else:
                print("ℹ️  Proxy não está ativo")
                
        elif escolha == '3':
            confirm = input("⚠️  Tem certeza que quer limpar TODO o cache? (s/n): ").strip().lower()
            if confirm == 's':
                if proxy and hasattr(proxy, 'db'):
                    cursor = proxy.db.cursor()
                    cursor.execute('DELETE FROM cache')
                    proxy.db.commit()
                    print("✅ Cache limpo completamente!")
                else:
                    print("❌ Proxy não está ativo")
                    
        elif escolha == '4':
            if proxy and hasattr(proxy, 'db'):
                cursor = proxy.db.cursor()
                cursor.execute('''
                    SELECT url, content_type, size, hits, timestamp 
                    FROM cache 
                    ORDER BY hits DESC 
                    LIMIT 20
                ''')
                
                resultados = cursor.fetchall()
                
                if resultados:
                    print("\n🏆 TOP 20 ITENS MAIS ACESSADOS:")
                    for i, (url, tipo, tamanho, hits, data) in enumerate(resultados, 1):
                        print(f"\n  {i}. {url[:50]}...")
                        print(f"     Tipo: {tipo}")
                        print(f"     Tamanho: {tamanho:,} bytes")
                        print(f"     Acessos: {hits}")
                        print(f"     Data: {data}")
                else:
                    print("ℹ️  Cache vazio")
                    
        elif escolha == '5':
            print("\n⚙️  CONFIGURAÇÕES DE CACHE:")
            print("   As configurações atuais são:")
            print(f"   • Tamanho máximo: 1000 itens")
            print(f"   • Expiração padrão: 1 hora")
            print(f"   • Compressão: Ativada (nível 6)")
            print("\n   Para modificar, edite o código fonte.")
            
        elif escolha == '6':
            print("\n👋 Até logo!")
            break
            
        else:
            print("❌ Opção inválida")

if __name__ == "__main__":
    menu_cache()
```

---

## 🚀 Proxy Reverso (Reverse Proxy)

### Proxy Reverso para Balanceamento de Carga

**proxy_reverso.py**
```python
import socket
import threading
import random
import time
from collections import defaultdict

class ProxyReverso:
    """Proxy reverso com balanceamento de carga"""
    
    def __init__(self, host='localhost', porta=80):
        self.host = host
        self.porta = porta
        self.running = False
        
        # Backends (servidores reais)
        self.backends = [
            {'host': 'localhost', 'porta': 8001, 'peso': 1, 'ativo': True},
            {'host': 'localhost', 'porta': 8002, 'peso': 1, 'ativo': True},
            {'host': 'localhost', 'porta': 8003, 'peso': 1, 'ativo': True}
        ]
        
        # Estatísticas
        self.stats = defaultdict(int)
        self.backend_stats = [{'requests': 0, 'errors': 0} for _ in self.backends]
        
        # Algoritmos de balanceamento
        self.algorithms = {
            'round_robin': self._round_robin,
            'random': self._random,
            'weighted': self._weighted,
            'least_connections': self._least_connections
        }
        
        self.algorithm = 'round_robin'
        self.current_backend = 0  # Para round-robin
        
        # Cache de sessões (sticky sessions)
        self.sessions = {}
        
        print(f"🔄 Inicializando Proxy Reverso")
        print(f"📡 Endereço público: {host}:{porta}")
        print(f"🖥️  Backends configurados: {len(self.backends)}")
        print(f"⚖️  Algoritmo: {self.algorithm}")
        print("-" * 60)
    
    def iniciar(self):
        """Inicia o proxy reverso"""
        
        try:
            server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            server.bind((self.host, self.porta))
            server.listen(100)  # Mais conexões para proxy reverso
            
            self.running = True
            self.server = server
            
            print(f"✅ Proxy reverso iniciado!")
            print(f"📞 Escutando em {self.host}:{self.porta}")
            print(f"🔄 Encaminhando para {len(self.backends)} backends")
            print("\n🔧 Configuração atual:")
            for i, backend in enumerate(self.backends):
                status = "✅ ATIVO" if backend['ativo'] else "❌ INATIVO"
                print(f"   Backend {i+1}: {backend['host']}:{backend['porta']} [{status}]")
            print("-" * 60)
            
            # Thread para monitoramento
            monitor_thread = threading.Thread(target=self._monitor_backends)
            monitor_thread.daemon = True
            monitor_thread.start()
            
            while self.running:
                try:
                    cliente_socket, cliente_addr = server.accept()
                    
                    self.stats['total_requests'] += 1
                    print(f"\n📨 Req #{self.stats['total_requests']} de {cliente_addr[0]}")
                    
                    thread = threading.Thread(
                        target=self._handle_request,
                        args=(cliente_socket, cliente_addr)
                    )
                    thread.daemon = True
                    thread.start()
                    
                except KeyboardInterrupt:
                    print("\n🛑 Proxy reverso interrompido")
                    self.running = False
                    break
                    
        except Exception as e:
            print(f"❌ Erro fatal: {e}")
            
        finally:
            self.parar()
    
    def _handle_request(self, cliente_socket, cliente_addr):
        """Processa uma requisição"""
        
        try:
            # Recebe requisição
            data = cliente_socket.recv(8192)
            
            if not data:
                return
            
            request_str = data.decode('utf-8', errors='ignore')
            
            # Extrai informações úteis
            client_ip = cliente_addr[0]
            path = self._extract_path(request_str)
            
            print(f"   🌐 Path: {path}")
            print(f"   👤 Client: {client_ip}")
            
            # Seleciona backend
            backend_idx = self._select_backend(client_ip, path)
            
            if backend_idx is None:
                print(f"   ❌ Nenhum backend disponível")
                self._send_error(cliente_socket, 503, "Service Unavailable")
                return
            
            backend = self.backends[backend_idx]
            
            # Atualiza estatísticas
            self.backend_stats[backend_idx]['requests'] += 1
            
            print(f"   🔄 Encaminhando para backend {backend_idx+1}: {backend['host']}:{backend['porta']}")
            
            # Conecta ao backend
            try:
                backend_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                backend_socket.settimeout(10)
                backend_socket.connect((backend['host'], backend['porta']))
                
                # Modifica headers se necessário
                modified_data = self._modify_headers(data, client_ip, backend_idx)
                
                # Envia para backend
                backend_socket.send(modified_data)
                
                # Recebe resposta
                resposta = self._receive_all(backend_socket)
                
                # Modifica resposta se necessário
                modified_resposta = self._modify_response(resposta, backend_idx)
                
                # Envia para cliente
                cliente_socket.send(modified_resposta)
                
                print(f"   ✅ Resposta enviada: {len(modified_resposta)} bytes")
                
                backend_socket.close()
                
            except Exception as e:
                print(f"   ❌ Erro no backend {backend_idx+1}: {e}")
                self.backend_stats[backend_idx]['errors'] += 1
                
                # Marca backend como inativo temporariamente
                backend['ativo'] = False
                print(f"   ⚠️  Backend {backend_idx+1} marcado como inativo")
                
                # Tenta outro backend
                self._handle_request(cliente_socket, cliente_addr)
                return
                
        except Exception as e:
            print(f"   ❌ Erro geral: {e}")
            
        finally:
            cliente_socket.close()
            
            # Mostra estatísticas periodicamente
            if self.stats['total_requests'] % 20 == 0:
                self._show_stats()
    
    def _select_backend(self, client_ip, path):
        """Seleciona um backend usando o algoritmo configurado"""
        
        # Filtra backends ativos
        active_backends = [i for i, b in enumerate(self.backends) if b['ativo']]
        
        if not active_backends:
            return None
        
        # Sticky sessions: mesma sessão vai para mesmo backend
        session_key = f"{client_ip}:{path}"
        if session_key in self.sessions:
            backend_idx = self.sessions[session_key]
            if backend_idx in active_backends:
                return backend_idx
        
        # Seleciona usando algoritmo
        if self.algorithm in self.algorithms:
            backend_idx = self.algorithms[self.algorithm](active_backends)
        else:
            backend_idx = self._round_robin(active_backends)
        
        # Armazena para sticky session
        self.sessions[session_key] = backend_idx
        
        return backend_idx
    
    def _round_robin(self, active_backends):
        """Algoritmo round-robin"""
        if not hasattr(self, '_rr_counter'):
            self._rr_counter = 0
        
        backend_idx = active_backends[self._rr_counter % len(active_backends)]
        self._rr_counter += 1
        
        return backend_idx
    
    def _random(self, active_backends):
        """Algoritmo random"""
        return random.choice(active_backends)
    
    def _weighted(self, active_backends):
        """Algoritmo weighted round-robin"""
        weights = [self.backends[i]['peso'] for i in active_backends]
        total = sum(weights)
        
        if total == 0:
            return self._random(active_backends)
        
        r = random.uniform(0, total)
        current = 0
        
        for i, idx in enumerate(active_backends):
            current += weights[i]
            if r <= current:
                return idx
        
        return active_backends[-1]
    
    def _least_connections(self, active_backends):
        """Algoritmo least connections (simulado)"""
        # Em produção, teria contador real de conexões
        # Aqui simulamos com estatísticas de requests
        min_requests = float('inf')
        selected = active_backends[0]
        
        for idx in active_backends:
            requests = self.backend_stats[idx]['requests']
            if requests < min_requests:
                min_requests = requests
                selected = idx
        
        return selected
    
    def _extract_path(self, request_str):
        """Extrai path da requisição"""
        lines = request_str.split('\r\n')
        if lines and ' ' in lines[0]:
            parts = lines[0].split(' ')
            if len(parts) >= 2:
                return parts[1]
        return '/'
    
    def _modify_headers(self, data, client_ip, backend_idx):
        """Modifica headers da requisição"""
        
        try:
            request_str = data.decode('utf-8', errors='ignore')
            lines = request_str.split('\r\n')
            
            # Adiciona headers X-Forwarded-*
            new_lines = []
            for line in lines:
                if line.lower().startswith('host:'):
                    # Mantém host original
                    new_lines.append(line)
                elif line == '':
                    # Fim dos headers
                    new_lines.extend([
                        f'X-Forwarded-For: {client_ip}',
                        f'X-Forwarded-Host: {self.host}',
                        f'X-Forwarded-Port: {self.porta}',
                        f'X-Forwarded-Proto: http',
                        f'X-Forwarded-Backend: {backend_idx + 1}',
                        ''
                    ])
                    break
                else:
                    new_lines.append(line)
            
            # Reconstrói requisição
            modified_request = '\r\n'.join(new_lines)
            
            return modified_request.encode()
            
        except:
            return data
    
    def _modify_response(self, resposta, backend_idx):
        """Modifica resposta se necessário"""
        
        # Aqui poderia modificar a resposta, como:
        # - Adicionar headers de caching
        # - Comprimir conteúdo
        # - Remover headers sensíveis
        # - Adicionar informações do backend
        
        try:
            resposta_str = resposta.decode('utf-8', errors='ignore')
            headers_end = resposta_str.find('\r\n\r\n')
            
            if headers_end != -1:
                headers = resposta_str[:headers_end]
                body = resposta_str[headers_end + 4:]
                
                # Adiciona header indicando qual backend respondeu
                new_headers = headers + f'\r\nX-Backend-ID: {backend_idx + 1}'
                
                modified_resposta = new_headers + '\r\n\r\n' + body
                return modified_resposta.encode()
                
        except:
            pass
        
        return resposta
    
    def _receive_all(self, socket):
        """Recebe todos os dados"""
        data = b""
        while True:
            try:
                chunk = socket.recv(4096)
                if not chunk:
                    break
                data += chunk
            except:
                break
        return data
    
    def _send_error(self, socket, code, message):
        """Envia página de erro"""
        
        html = f"""
        <html>
        <head><title>{code} {message}</title></head>
        <body style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
            <h1>{code} - {message}</h1>
            <p>Proxy reverso temporariamente indisponível.</p>
            <p>Tente novamente em alguns instantes.</p>
            <hr>
            <p><small>Proxy Reverso Python - {len(self.backends)} backends</small></p>
        </body>
        </html>
        """
        
        response = (
            f"HTTP/1.1 {code} {message}\r\n"
            "Content-Type: text/html; charset=utf-8\r\n"
            f"Content-Length: {len(html)}\r\n"
            "\r\n"
            f"{html}"
        )
        
        socket.send(response.encode())
    
    def _monitor_backends(self):
        """Monitora health dos backends"""
        
        while self.running:
            time.sleep(30)  # Verifica a cada 30 segundos
            
            print("\n🩺 VERIFICAÇÃO DE HEALTH DOS BACKENDS")
            
            for i, backend in enumerate(self.backends):
                if not backend['ativo']:
                    # Tenta reconectar a backends inativos
                    try:
                        test_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                        test_socket.settimeout(2)
                        test_socket.connect((backend['host'], backend['porta']))
                        test_socket.close()
                        
                        backend['ativo'] = True
                        print(f"   ✅ Backend {i+1} recuperado: {backend['host']}:{backend['porta']}")
                        
                    except:
                        print(f"   ❌ Backend {i+1} ainda inativo: {backend['host']}:{backend['porta']}")
    
    def _show_stats(self):
        """Mostra estatísticas"""
        
        print(f"\n📊 ESTATÍSTICAS DO PROXY REVERSO:")
        print(f"   Requisições totais: {self.stats['total_requests']}")
        
        active_backends = sum(1 for b in self.backends if b['ativo'])
        print(f"   Backends ativos: {active_backends}/{len(self.backends)}")
        
        print(f"\n   📈 Por backend:")
        for i, backend in enumerate(self.backends):
            stats = self.backend_stats[i]
            status = "✅" if backend['ativo'] else "❌"
            print(f"   {i+1}. {backend['host']}:{backend['porta']} [{status}]")
            print(f"      Requisições: {stats['requests']}")
            print(f"      Erros: {stats['errors']}")
    
    def parar(self):
        """Para o proxy reverso"""
        
        if hasattr(self, 'server'):
            self.server.close()
        
        self.running = False
        
        print("\n" + "=" * 50)
        print("📊 ESTATÍSTICAS FINAIS:")
        self._show_stats()
        print("🔒 Proxy reverso parado")

def menu_reverso():
    """Menu para proxy reverso"""
    
    print("🔄 PROXY REVERSO COM BALANCEAMENTO DE CARGA")
    print("=" * 60)
    
    proxy = None
    
    while True:
        print("\n📋 MENU:")
        print("1. Iniciar Proxy Reverso")
        print("2. Configurar Backends")
        print("3. Escolher Algoritmo de Balanceamento")
        print("4. Ver Estatísticas")
        print("5. Testar Backends")
        print("6. Sair")
        
        escolha = input("\n🎯 Escolha: ").strip()
        
        if escolha == '1':
            host = input("Host público (ENTER para localhost): ").strip() or 'localhost'
            porta_input = input("Porta (ENTER para 80): ").strip()
            porta = int(porta_input) if porta_input else 80
            
            proxy = ProxyReverso(host=host, porta=porta)
            
            print("\n▶️  Iniciando proxy reverso...")
            print("⚠️  Pressione Ctrl+C para parar\n")
            
            # Inicia backends de teste (simulados)
            print("💡 Dica: Para testar, inicie servidores nas portas 8001, 8002, 8003")
            print("   Exemplo: python -m http.server 8001")
            
            try:
                proxy.iniciar()
            except KeyboardInterrupt:
                print("\n⏹️  Proxy reverso parado")
                
        elif escolha == '2':
            if not proxy:
                proxy = ProxyReverso()
            
            print("\n🖥️  CONFIGURAR BACKENDS:")
            print("Backends atuais:")
            for i, b in enumerate(proxy.backends):
                print(f"  {i+1}. {b['host']}:{b['porta']} (peso: {b['peso']})")
            
            print("\n1. Adicionar backend")
            print("2. Remover backend")
            print("3. Modificar backend")
            
            sub = input("Escolha: ").strip()
            
            if sub == '1':
                host = input("Host do backend: ").strip()
                porta = int(input("Porta: ").strip())
                peso = int(input("Peso (1-10): ").strip() or '1')
                
                proxy.backends.append({
                    'host': host,
                    'porta': porta,
                    'peso': peso,
                    'ativo': True
                })
                
                # Adiciona estatísticas para novo backend
                proxy.backend_stats.append({'requests': 0, 'errors': 0})
                
                print(f"✅ Backend {host}:{porta} adicionado!")
                
        elif escolha == '3':
            if not proxy:
                proxy = ProxyReverso()
            
            print("\n⚖️  ALGORITMOS DE BALANCEAMENTO:")
            print("1. Round Robin (padrão)")
            print("2. Random")
            print("3. Weighted")
            print("4. Least Connections")
            
            algo = input("Escolha (1-4): ").strip()
            
            algoritmos = {
                '1': 'round_robin',
                '2': 'random',
                '3': 'weighted',
                '4': 'least_connections'
            }
            
            if algo in algoritmos:
                proxy.algorithm = algoritmos[algo]
                print(f"✅ Algoritmo definido como: {proxy.algorithm}")
                
        elif escolha == '4':
            if proxy:
                proxy._show_stats()
            else:
                print("ℹ️  Proxy não está ativo")
                
        elif escolha == '5':
            if not proxy:
                proxy = ProxyReverso()
            
            print("\n🧪 TESTANDO BACKENDS...")
            for i, backend in enumerate(proxy.backends):
                try:
                    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                    s.settimeout(2)
                    s.connect((backend['host'], backend['porta']))
                    s.close()
                    print(f"   ✅ Backend {i+1}: {backend['host']}:{backend['porta']} - CONECTADO")
                except:
                    print(f"   ❌ Backend {i+1}: {backend['host']}:{backend['porta']} - INACESSÍVEL")
                    
        elif escolha == '6':
            print("\n👋 Até logo!")
            break
            
        else:
            print("❌ Opção inválida")

if __name__ == "__main__":
    menu_reverso()
```

---

## 🚨 Problemas Comuns e Soluções

### 1. **Proxy Não Conecta**
```python
# Verifique:
# 1. Firewall está bloqueando?
# 2. Porta já está em uso?
# 3. Endereço correto?

# Teste com:
import socket
s = socket.socket()
s.bind(('localhost', 8888))  # Deve funcionar
```

### 2. **Navegador Rejeita Certificado SSL**
```python
# Para proxies HTTPS, você pode:
# 1. Instalar certificado raiz customizado
# 2. Usar modo transparente (sem interceptar SSL)
# 3. Desativar verificação SSL no cliente (não recomendado)
```

### 3. **Performance Lenta**
```python
# Otimizações:
# 1. Aumente buffer size
socket.recv(16384)  # 16KB em vez de 4KB

# 2. Use threading pool
from concurrent.futures import ThreadPoolExecutor
executor = ThreadPoolExecutor(max_workers=50)

# 3. Implemente cache
```

### 4. **Vazamento de Memória**
```python
# Sempre feche sockets!
try:
    s.connect(...)
    # ...
finally:
    s.close()  # ← IMPORTANTE!

# Use with statement
with socket.socket() as s:
    s.connect(...)
    # ...
# Fecha automaticamente
```

---

## 💼 Casos de Uso Práticos

### 1. **Proxy para Desenvolvimento Web**
```python
# Útil para:
# - Debug de requests HTTP
# - Modificar respostas em tempo real
# - Testar sites em diferentes condições
# - Simular latência de rede
```

### 2. **Proxy para Segurança**
```python
# Use para:
# - Bloquear sites maliciosos
# - Filtrar conteúdo inapropriado
# - Log de atividades de rede
# - Detecção de intrusões básica
```

### 3. **Proxy para Otimização**
```python
# Benefícios:
# - Cache local acelera acesso
# - Compressão economiza banda
# - Prefetch de recursos comuns
# - Balanceamento de carga
```

### 4. **Proxy para Anonimato**
```python
# Funcionalidades:
# - Rotação de IPs
# - Criptografia de tráfego
# - Remoção de headers identificadores
# - Suporte a múltiplos protocolos
```

---

## 🛡️ Segurança em Proxies

### Boas Práticas:

1. **Autenticação**
```python
# Sempre exija autenticação para proxies abertos
def verificar_credenciais(usuario, senha):
    # Use hash para senhas!
    import hashlib
    senha_hash = hashlib.sha256(senha.encode()).hexdigest()
    # Compare com hash armazenado
```

2. **Logging Seguro**
```python
# Não logue dados sensíveis
def log_seguro(requisicao):
    # Remove dados sensíveis
    requisicao = requisicao.replace('senha=***', 'senha=[REDACTED]')
    requisicao = requisicao.replace('token=***', 'token=[REDACTED]')
    # Agora pode logar
```

3. **Validação de Input**
```python
# Valide todas as entradas
def validar_host(host):
    import re
    # Permite apenas hosts válidos
    if not re.match(r'^[a-zA-Z0-9.-]+$', host):
        raise ValueError('Host inválido')
```

4. **Rate Limiting**
```python
# Evite abuso
from collections import defaultdict
import time

class RateLimiter:
    def __init__(self, limite=100, periodo=60):
        self.limite = limite
        self.periodo = periodo
        self.contadores = defaultdict(list)
    
    def permitir(self, ip):
        agora = time.time()
        # Remove registros antigos
        self.contadores[ip] = [t for t in self.contadores[ip] 
                              if agora - t < self.periodo]
        
        if len(self.contadores[ip]) >= self.limite:
            return False
        
        self.contadores[ip].append(agora)
        return True
```

---

## 📊 Métricas e Monitoramento

### O que Monitorar:

```python
class ProxyMetrics:
    def __init__(self):
        self.metrics = {
            'requests_total': 0,
            'requests_by_method': defaultdict(int),
            'requests_by_status': defaultdict(int),
            'response_time': [],
            'bandwidth_in': 0,
            'bandwidth_out': 0,
            'active_connections': 0,
            'cache_hit_rate': 0
        }
    
    def record_request(self, method, status, response_time, bytes_in, bytes_out):
        self.metrics['requests_total'] += 1
        self.metrics['requests_by_method'][method] += 1
        self.metrics['requests_by_status'][status] += 1
        self.metrics['response_time'].append(response_time)
        self.metrics['bandwidth_in'] += bytes_in
        self.metrics['bandwidth_out'] += bytes_out
        
        # Mantém apenas últimos 1000 tempos
        if len(self.metrics['response_time']) > 1000:
            self.metrics['response_time'] = self.metrics['response_time'][-1000:]
```

---

## 🎯 Conclusão

### O que Você Aprendeu:

✅ **Proxy Básico**: Como criar um proxy simples do zero  
✅ **Proxy HTTP/HTTPS**: Suporte completo a protocolos web  
✅ **Proxy SOCKS**: Para qualquer tipo de tráfego  
✅ **Autenticação**: Controle de acesso por usuário  
✅ **Cache**: Otimização com armazenamento local  
✅ **Proxy Reverso**: Balanceamento de carga e alta disponibilidade  

### Próximos Passos:

1. **Adicione SSL/TLS** para tráfego criptografado
2. **Implemente um Dashboard Web** para gerenciamento
3. **Adicione Suporte a WebSocket** para aplicações em tempo real
4. **Integre com APIs** para automação
5. **Crie uma Interface Gráfica** (GUI) para usuários finais

### Recursos Úteis:

- **Bibliotecas Python**: `mitmproxy`, `proxy.py`, `pysocks`
- **Ferramentas**: Burp Suite, Charles Proxy, Fiddler
- **Documentação**: RFC 7230 (HTTP/1.1), RFC 1928 (SOCKS5)
- **Comunidade**: Subreddits de networking, fóruns de segurança

---

## 🚀 Desafios para Praticar

### Nível Iniciante:
1. Crie um proxy que modifica todas as imagens para preto e branco
2. Desenvolva um proxy que adiciona um footer em todas as páginas
3. Implemente um proxy que bloqueia anúncios baseado em lista

### Nível Intermediário:
1. Proxy com suporte a WebSocket
2. Proxy com geolocalização (roteamento baseado em localização)
3. Proxy com compressão inteligente de imagens

### Nível Avançado:
1. Proxy com machine learning para detectar malware
2. Proxy distribuído (várias instâncias trabalhando juntas)
3. Proxy com suporte a quic/HTTP3

---

## 📞 Suporte e Comunidade

### Onde Buscar Ajuda:

- **Stack Overflow**: Tags `[python] [proxy] [networking]`
- **GitHub**: Issues de projetos como `mitmproxy`
- **Reddit**: r/networking, r/Python, r/learnpython
- **Discord**: Servidores de desenvolvimento Python

### Como Reportar Problemas:

1. Descreva o comportamento esperado
2. Mostre o comportamento atual
3. Inclua logs relevantes
4. Forneça versão do Python e sistema operacional
5. Mostre código relevante (se possível)


---

*Documentação criada com ❤️ para a comunidade brasileira de Python. Atualizado em 2024.*
