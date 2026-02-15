# 📚 MANUAL COMPLETO DA BIBLIOTECA SOCKET EM PYTHON

## 📋 ÍNDICE
1. [Famílias de Endereços (Address Families)](#famílias-de-endereços)
2. [Tipos de Socket](#tipos-de-socket)
3. [Protocolos](#protocolos)
4. [Opções de Socket](#opções-de-socket)
5. [Constantes de Rede](#constantes-de-rede)
6. [Funções Principais](#funções-principais)
7. [Flags e Opções](#flags-e-opções)
8. [Constantes Específicas](#constantes-específicas)

---

## 🌐 FAMÍLIAS DE ENDEREÇOS (Address Families)

### Principais Famílias

| Constante | Valor | Descrição | Uso Comum |
|-----------|-------|-----------|-----------|
| **AF_INET** | 2 | IPv4 | Redes tradicionais, internet |
| **AF_INET6** | 10 | IPv6 | Redes IPv6 modernas |
| **AF_UNIX** | 1 | Unix Domain Socket | Comunicação entre processos no mesmo host |
| **AF_UNSPEC** | 0 | Não especificado | Permite qualquer família de endereços |

### Famílias Especializadas

| Constante | Descrição | Caso de Uso |
|-----------|-----------|-------------|
| **AF_PACKET** | Acesso direto à camada de enlace | Sniffers, raw sockets |
| **AF_NETLINK** | Comunicação kernel↔usuário | Configuração de rede do sistema |
| **AF_CAN** | Controller Area Network | Redes automotivas, industrial |
| **AF_BLUETOOTH** | Bluetooth | Dispositivos sem fio Bluetooth |
| **AF_VSOCK** | Virtual Machine Sockets | Comunicação VM↔host |
| **AF_ALG** | Linux Kernel Crypto API | Criptografia via kernel |

### Famílias Legadas/Específicas

| Constante | Descrição | Status |
|-----------|-----------|--------|
| AF_APPLETALK | AppleTalk protocol | Legado |
| AF_AX25 | Amateur Radio X.25 | Específico |
| AF_IPX | Novell IPX | Legado |
| AF_IRDA | Infrared Data Association | Legado |
| AF_X25 | X.25 protocol | Legado |
| AF_DECnet | DECnet protocol | Legado |
| AF_ATMPVC | ATM PVCs | Específico |
| AF_ATMSVC | ATM SVCs | Específico |
| AF_ROSE | X.25 PLP | Específico |
| AF_NETROM | NET/ROM | Amateur radio |
| AF_BRIDGE | Bridging | Kernel bridge |
| AF_ASH | Ash protocol | Específico |
| AF_ECONET | Acorn Econet | Legado |
| AF_LLC | LLC | IEEE 802.2 |
| AF_TIPC | Cluster IPC | Telecom |
| AF_WANPIPE | Wanpipe API | WAN interfaces |

---

## 🔌 TIPOS DE SOCKET

### Tipos Principais

| Tipo | Constante | Descrição | Protocolo Típico |
|------|-----------|-----------|------------------|
| **Stream** | SOCK_STREAM | Conexão orientada, confiável | TCP |
| **Datagram** | SOCK_DGRAM | Sem conexão, não confiável | UDP |
| **Raw** | SOCK_RAW | Acesso direto ao protocolo | IP raw |
| **Sequenced Packet** | SOCK_SEQPACKET | Stream com limites de mensagem | SCTP |
| **Reliable Datagram** | SOCK_RDM | Datagrama confiável | RDS |

### Flags Adicionais

```python
SOCK_CLOEXEC    # Fecha socket ao executar exec()
SOCK_NONBLOCK   # Socket não-bloqueante
```

---

## 📡 PROTOCOLOS

### Protocolos de Camada de Transporte

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| **IPPROTO_TCP** | 6 | Protocolo TCP |
| **IPPROTO_UDP** | 17 | Protocolo UDP |
| **IPPROTO_SCTP** | 132 | Stream Control Transmission Protocol |
| **IPPROTO_UDPLITE** | 136 | UDP-Lite (checksum parcial) |

### Protocolos de Camada de Rede

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| **IPPROTO_IP** | 0 | Protocolo IP (pseudo) |
| **IPPROTO_IPV6** | 41 | Protocolo IPv6 |
| **IPPROTO_ICMP** | 1 | Internet Control Message Protocol |
| **IPPROTO_ICMPV6** | 58 | ICMP para IPv6 |
| **IPPROTO_IGMP** | 2 | Internet Group Management Protocol |
| **IPPROTO_RAW** | 255 | Raw IP packets |

### Protocolos de Segurança/Extensões

| Constante | Descrição |
|-----------|-----------|
| IPPROTO_AH | Authentication Header (IPsec) |
| IPPROTO_ESP | Encapsulating Security Payload |
| IPPROTO_GRE | Generic Routing Encapsulation |
| IPPROTO_MPTCP | Multipath TCP |

---

## ⚙️ OPÇÕES DE SOCKET (Socket Options)

### Níveis de Opção

```python
SOL_SOCKET    # Nível socket (geral)
SOL_TCP       # Nível TCP
SOL_UDP       # Nível UDP
SOL_IP        # Nível IP
SOL_IPV6      # Nível IPv6
```

### Opções Gerais (SOL_SOCKET)

#### Configuração Comportamental

```python
SO_REUSEADDR      # Reutilizar endereços locais
SO_REUSEPORT      # Permitir múltiplos sockets na mesma porta
SO_KEEPALIVE      # Manter conexões ativas
SO_BROADCAST      # Permitir transmissão broadcast
SO_DONTROUTE      # Enviar apenas para hosts diretamente conectados
```

#### Buffer e Timeouts

```python
SO_SNDBUF         # Tamanho do buffer de envio
SO_RCVBUF         # Tamanho do buffer de recepção
SO_SNDTIMEO       # Timeout para envio
SO_RCVTIMEO       # Timeout para recepção
```

#### Informação e Controle

```python
SO_TYPE           # Obter tipo do socket (read-only)
SO_ERROR          # Obter e limpar erro do socket
SO_DEBUG          # Habilitar debugging
SO_ACCEPTCONN     # Verificar se socket está ouvindo
```

### Opções TCP (SOL_TCP)

| Opção | Descrição |
|-------|-----------|
| TCP_NODELAY | Desabilitar algoritmo de Nagle |
| TCP_KEEPIDLE | Tempo até iniciar keepalive |
| TCP_KEEPINTVL | Intervalo entre keepalives |
| TCP_KEEPCNT | Número de keepalives antes de fechar |
| TCP_MAXSEG | Tamanho máximo do segmento |
| TCP_CONGESTION | Algoritmo de controle de congestionamento |
| TCP_FASTOPEN | Habilitar TCP Fast Open |

### Opções IP (SOL_IP)

| Opção | Descrição |
|-------|-----------|
| IP_TTL | Time-To-Live dos pacotes |
| IP_TOS | Type Of Service |
| IP_OPTIONS | Opções de cabeçalho IP |
| IP_HDRINCL | Incluir cabeçalho IP nos dados |
| IP_ADD_MEMBERSHIP | Adicionar a grupo multicast |
| IP_DROP_MEMBERSHIP | Remover de grupo multicast |

### Opções IPv6 (SOL_IPV6)

| Opção | Descrição |
|-------|-----------|
| IPV6_V6ONLY | Usar apenas IPv6 |
| IPV6_UNICAST_HOPS | TTL para unicast |
| IPV6_MULTICAST_HOPS | TTL para multicast |
| IPV6_JOIN_GROUP | Juntar-se a grupo multicast |
| IPV6_LEAVE_GROUP | Sair de grupo multicast |

---

## 🎯 FLAGS E OPÇÕES

### Flags para send()/recv()

| Flag | Descrição |
|------|-----------|
| **MSG_DONTWAIT** | Operação não-bloqueante |
| **MSG_OOB** | Dados out-of-band |
| **MSG_PEEK** | Ler dados sem removê-los do buffer |
| **MSG_WAITALL** | Esperar até receber todos os dados |
| **MSG_TRUNC** | Retornar comprimento real do datagrama |
| **MSG_CTRUNC** | Dados de controle truncados |
| **MSG_EOR** | Fim de registro |
| **MSG_NOSIGNAL** | Não gerar SIGPIPE |

### Flags para getaddrinfo()

| Flag | Descrição |
|------|-----------|
| AI_PASSIVE | Usar para bind() (endereço wildcard) |
| AI_CANONNAME | Retornar nome canônico |
| AI_NUMERICHOST | Nome do host é endereço numérico |
| AI_NUMERICSERV | Serviço é número de porta |
| AI_V4MAPPED | Mapear IPv6 para IPv4 se necessário |
| AI_ALL | Retornar todos os endereços |
| AI_ADDRCONFIG | Retornar apenas endereços configurados |

### Flags para shutdown()

| Constante | Descrição |
|-----------|-----------|
| SHUT_RD | Fechar leitura |
| SHUT_WR | Fechar escrita |
| SHUT_RDWR | Fechar leitura e escrita |

---

## 🔧 CONSTANTES ESPECÍFICAS

### Endereços Especiais IPv4

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| INADDR_ANY | 0.0.0.0 | Aceitar conexões em qualquer interface |
| INADDR_LOOPBACK | 127.0.0.1 | Interface de loopback |
| INADDR_BROADCAST | 255.255.255.255 | Endereço de broadcast |
| INADDR_NONE | 255.255.255.255 | Endereço inválido |

### Portas

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| IPPORT_RESERVED | 1024 | Início das portas não-reservadas |
| IPPORT_USERRESERVED | 5000 | Final das portas reservadas ao usuário |

### Ethernet

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| ETH_P_ALL | 0x0003 | Todos os protocolos |
| ETH_P_IP | 0x0800 | Protocolo IP |
| ETH_P_ARP | 0x0806 | Protocolo ARP |
| ETH_P_IPV6 | 0x86DD | Protocolo IPv6 |

### Bluetooth

| Constante | Descrição |
|-----------|-----------|
| BTPROTO_HCI | Host Controller Interface |
| BTPROTO_L2CAP | Logical Link Control and Adaptation Protocol |
| BTPROTO_RFCOMM | Radio Frequency Communication |
| BTPROTO_SCO | Synchronous Connection-Oriented |
| BDADDR_ANY | Endereço Bluetooth qualquer |
| BDADDR_LOCAL | Endereço Bluetooth local |

### CAN (Controller Area Network)

| Constante | Descrição |
|-----------|-----------|
| CAN_RAW | Socket CAN raw |
| CAN_BCM | Broadcast Manager |
| CAN_ISOTP | ISO-TP (Transport Protocol) |
| CAN_J1939 | J1939 protocol |
| CAN_EFF_FLAG | Frame de formato estendido |
| CAN_RTR_FLAG | Frame de requisição remota |

### Netlink

| Constante | Descrição |
|-----------|-----------|
| NETLINK_ROUTE | Tabelas de roteamento/interface |
| NETLINK_FIREWALL | Netfilter firewall |
| NETLINK_NFLOG | Netfilter logging |
| NETLINK_XFRM | IPsec |
| NETLINK_CRYPTO | Criptografia |

---

## 🛠️ FUNÇÕES PRINCIPAIS

### Criação e Configuração

```python
# Criar socket
socket(family=AF_INET, type=SOCK_STREAM, proto=IPPROTO_TCP)

# Criar conexão
create_connection((host, port))

# Criar servidor
create_server((host, port))

# Socket pair
socketpair(family=AF_UNIX, type=SOCK_STREAM)
```

### Resolução de Nomes

```python
# DNS lookup
gethostbyname("example.com")
gethostbyaddr("8.8.8.8")
getaddrinfo("host", "port", family, type, proto, flags)

# Name service
getservbyname("http", "tcp")
getservbyport(80, "tcp")

# Protocolo
getprotobyname("tcp")
```

### Conversão de Endereços

```python
# IPv4
inet_aton("192.168.1.1")  # String para binário
inet_ntoa(b'\xc0\xa8\x01\x01')  # Binário para string

# IPv4/IPv6
inet_pton(AF_INET, "192.168.1.1")
inet_ntop(AF_INET, b'\xc0\xa8\x01\x01')
```

### Byte Order (Endianness)

```python
htonl(12345)  # Host para network (long)
htons(12345)  # Host para network (short)
ntohl(value)  # Network para host (long)
ntohs(value)  # Network para host (short)
```

### Informação de Rede

```python
gethostname()           # Nome do host local
getfqdn()               # Nome de domínio totalmente qualificado
getnameinfo(sockaddr)   # Nome para endereço
if_nameindex()          # Lista interfaces de rede
if_indextoname(index)   # Índice para nome de interface
if_nametoindex(name)    # Nome para índice de interface
```

---

## 💡 EXEMPLOS PRÁTICOS

### 1. Socket TCP Básico

```python
import socket

# Servidor
server = socket.socket(AF_INET, SOCK_STREAM)
server.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
server.bind(('0.0.0.0', 8080))
server.listen(SOMAXCONN)

# Cliente
client = socket.socket(AF_INET, SOCK_STREAM)
client.connect(('localhost', 8080))
```

### 2. Socket UDP com Opções

```python
sock = socket.socket(AF_INET, SOCK_DGRAM)
sock.setsockopt(SOL_SOCKET, SO_BROADCAST, 1)  # Permitir broadcast
sock.bind(('', 9999))
```

### 3. Socket Raw para ICMP

```python
# Requer privilégios de root
sock = socket.socket(AF_INET, SOCK_RAW, IPPROTO_ICMP)
sock.setsockopt(SOL_IP, IP_HDRINCL, 1)
```

### 4. IPv6 com Dual Stack

```python
sock = socket.socket(AF_INET6, SOCK_STREAM)
sock.setsockopt(SOL_IPV6, IPV6_V6ONLY, 0)  # Permitir IPv4 também
```

### 5. Socket Unix Domain

```python
sock = socket.socket(AF_UNIX, SOCK_STREAM)
sock.bind('/tmp/mysocket.sock')
```

### 6. Multicast IPv4

```python
sock = socket.socket(AF_INET, SOCK_DGRAM)

# Adicionar ao grupo multicast
mreq = socket.inet_aton('224.1.1.1') + socket.inet_aton('192.168.1.1')
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)
```

---

## ⚠️ CÓDIGOS DE ERRO IMPORTANTES

| Constante | Valor | Descrição |
|-----------|-------|-----------|
| EAGAIN / EWOULDBLOCK | 11 | Recurso temporariamente indisponível |
| EBADF | 9 | Bad file descriptor |
| EAI_ADDRFAMILY | -9 | Família de endereço não suportada |
| EAI_AGAIN | -3 | Falha temporária em resolução de nome |
| EAI_NONAME | -2 | Nome ou serviço desconhecido |

---

## 📊 TABELAS DE REFERÊNCIA RÁPIDA

### Famílias Mais Usadas

```
IPv4: AF_INET + SOCK_STREAM + IPPROTO_TCP
IPv6: AF_INET6 + SOCK_STREAM + IPPROTO_TCP
UDP: AF_INET + SOCK_DGRAM + IPPROTO_UDP
Unix: AF_UNIX + SOCK_STREAM
Raw: AF_PACKET + SOCK_RAW
```

### Opções Mais Comuns

```
Reuse Address: SO_REUSEADDR
Keep Alive: SO_KEEPALIVE
No Delay (TCP): TCP_NODELAY
Non-blocking: MSG_DONTWAIT ou SOCK_NONBLOCK
```

### Portas Bem Conhecidas

```python
HTTP = 80
HTTPS = 443
SSH = 22
DNS = 53
DHCP = 67
```

---

## 🔍 NOTAS IMPORTANTES

1. **Privilégios**: Sockets raw (SOCK_RAW) requerem privilégios de root
2. **Portabilidade**: Algumas constantes são específicas do Linux
3. **Performance**: Use `SO_REUSEPORT` para balanceamento de carga em servidores
4. **IPv6**: Sempre teste com `has_ipv6` se precisar de suporte a IPv6
5. **Timeout**: Configure timeouts para evitar bloqueios eternos
6. **Buffer**: Ajuste buffers (SO_SNDBUF/SO_RCVBUF) para alta performance
7. **Non-blocking**: Use para operações assíncronas ou com select/poll

---

## 🎓 BÔNUS: Padrões de Uso

### Servidor TCP Resiliente

```python
def create_resilient_server(host, port):
    sock = socket.socket(AF_INET, SOCK_STREAM)
    sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
    sock.setsockopt(SOL_SOCKET, SO_REUSEPORT, 1)
    sock.setsockopt(SOL_SOCKET, SO_KEEPALIVE, 1)
    sock.setsockopt(SOL_TCP, TCP_NODELAY, 1)
    sock.bind((host, port))
    sock.listen(SOMAXCONN)
    return sock
```

### Cliente com Timeout

```python
def create_client_with_timeout(host, port, timeout=10):
    sock = socket.socket(AF_INET, SOCK_STREAM)
    sock.settimeout(timeout)
    sock.connect((host, port))
    sock.setsockopt(SOL_TCP, TCP_NODELAY, 1)
    return sock
```

---

## 📈 REFERÊNCIA COMPLETA DAS CONSTANTES

### AF_* (Address Families)

| Constante | Sistema | Descrição Detalhada |
|-----------|---------|-------------------|
| **AF_INET** | Todos | IPv4 Internet protocols (RFC 791) |
| **AF_INET6** | Todos | IPv6 Internet protocols (RFC 2460) |
| **AF_UNIX** | POSIX | Unix domain sockets (IPC local) |
| **AF_PACKET** | Linux | Low-level packet interface (camada 2) |
| **AF_NETLINK** | Linux | Interface com kernel networking |
| **AF_CAN** | Linux | Controller Area Network - automotivo |
| **AF_BLUETOOTH** | Linux | Protocolos Bluetooth |
| **AF_ALG** | Linux | Interface com criptografia do kernel |
| **AF_VSOCK** | Linux | VM Sockets (hypervisor comunicação) |
| **AF_TIPC** | Linux | Cluster IPC para telecomunicações |

### SOCK_* (Tipos de Socket)

| Constante | Descrição Técnica |
|-----------|-------------------|
| **SOCK_STREAM** | Sequência de bytes confiável, bidirecional |
| **SOCK_DGRAM** | Datagramas não confiáveis com limites de mensagem |
| **SOCK_RAW** | Acesso direto ao protocolo de rede |
| **SOCK_SEQPACKET** | Sequência de mensagens confiável com preservação de limites |
| **SOCK_RDM** | Datagrama confiável sem ordem garantida |

### Protocolos por Camada

#### Camada de Transporte (4)
```python
IPPROTO_TCP      # Transmission Control Protocol (RFC 793)
IPPROTO_UDP      # User Datagram Protocol (RFC 768)
IPPROTO_SCTP     # Stream Control Transmission Protocol
IPPROTO_DCCP     # Datagram Congestion Control Protocol
```

#### Camada de Rede (3)
```python
IPPROTO_IP       # Internet Protocol (pseudo-protocolo)
IPPROTO_IPV6     # IPv6 Protocol
IPPROTO_ICMP     # Internet Control Message Protocol
IPPROTO_IGMP     # Internet Group Management Protocol
```

### Opções de Socket por Nível

#### SOL_SOCKET (Nível Geral)
```python
# Controle de Endereço
SO_REUSEADDR     # Permitir reuso de endereço local
SO_REUSEPORT     # Permitir múltiplos sockets na mesma porta

# Controle de Conexão
SO_KEEPALIVE     # Manter conexão ativa com probes
SO_LINGER        # Controlar comportamento ao fechar

# Controle de Transmissão
SO_SNDBUF        # Tamanho do buffer de envio
SO_RCVBUF        # Tamanho do buffer de recepção
SO_SNDLOWAT      # Limite baixo para envio
SO_RCVLOWAT      # Limite baixo para recepção

# Timeouts
SO_SNDTIMEO      # Timeout para operações de envio
SO_RCVTIMEO      # Timeout para operações de recepção

# Informação
SO_TYPE          # Tipo do socket (read-only)
SO_ERROR         # Erro pendente do socket
SO_ACCEPTCONN    # Socket está ouvindo (read-only)
```

#### SOL_TCP (Opções TCP Específicas)
```python
TCP_NODELAY      # Desabilitar algoritmo de Nagle
TCP_MAXSEG       # Tamanho máximo do segmento
TCP_CORK         # Agrupar pequenas mensagens (como Nagle)
TCP_KEEPIDLE     # Segundos antes de iniciar keepalive probes
TCP_KEEPINTVL    # Segundos entre keepalive probes
TCP_KEEPCNT      # Número de probes antes de falhar
TCP_SYNCNT       # Número de tentativas de SYN
TCP_WINDOW_CLAMP # Limitar janela de anúncio
```

### Flags para Operações de Socket

#### Flags para send()/sendto()
```python
MSG_OOB          # Enviar dados out-of-band (urgente)
MSG_DONTROUTE    # Ignorar tabelas de roteamento
MSG_DONTWAIT     # Não bloquear (non-blocking)
MSG_NOSIGNAL     # Não gerar SIGPIPE em erro
MSG_CONFIRM      # Confirmar transmissão (layer 2)
MSG_MORE         # Mais dados serão enviados (TCP_CORK)
MSG_FASTOPEN     # TCP Fast Open (handshake combinado)
```

#### Flags para recv()/recvfrom()
```python
MSG_PEEK         # Ler dados sem removê-los do buffer
MSG_WAITALL      # Esperar até que todos os dados estejam disponíveis
MSG_TRUNC        # Retornar comprimento real do datagrama
MSG_CTRUNC       # Indicar que dados de controle foram truncados
MSG_ERRQUEUE     # Receber erro da fila de erro do socket
```

### Constantes Específicas por Domínio

#### Bluetooth (AF_BLUETOOTH)
```python
BTPROTO_L2CAP    # Logical Link Control and Adaptation Protocol
BTPROTO_HCI      # Host Controller Interface
BTPROTO_SCO      # Synchronous Connection-Oriented
BTPROTO_RFCOMM   # Radio Frequency Communication

# Endereços Bluetooth especiais
BDADDR_ANY       # Endereço qualquer (0:0:0:0:0:0)
BDADDR_LOCAL     # Endereço local (0:0:0:FF:FF:FF)
```

#### CAN Bus (AF_CAN)
```python
CAN_RAW          # Socket CAN raw (frames brutos)
CAN_BCM          # Broadcast Manager (gerenciamento)
CAN_TP16         # Transport Protocol 1.6
CAN_TP20         # Transport Protocol 2.0
CAN_MCNET        # Mercedes-Benz Multicast
CAN_ISOTP        # ISO-TP (ISO 15765-2)

# Frame flags
CAN_EFF_FLAG     # Extended frame format (29-bit ID)
CAN_RTR_FLAG     # Remote transmission request
CAN_ERR_FLAG     # Error frame
```

#### Netlink (AF_NETLINK)
```python
NETLINK_ROUTE         # Tabelas de roteamento/interface
NETLINK_FIREWALL      # Netfilter firewall
NETLINK_INET_DIAG     # Informação de socket inet
NETLINK_NFLOG         # Netfilter logging
NETLINK_XFRM          # IPsec
NETLINK_SELINUX       # SELinux
NETLINK_CRYPTO        # Criptografia
NETLINK_USERSOCK      # Reservado para sockets de usuário
```

### Valores Especiais

#### Endereços IPv4
```python
INADDR_ANY            # 0.0.0.0 - Qualquer interface
INADDR_LOOPBACK       # 127.0.0.1 - Loopback
INADDR_BROADCAST      # 255.255.255.255 - Broadcast
INADDR_NONE           # 255.255.255.255 - Endereço inválido

# Grupos multicast
INADDR_UNSPEC_GROUP   # 224.0.0.0 - Base multicast
INADDR_ALLHOSTS_GROUP # 224.0.0.1 - Todos hosts
INADDR_MAX_LOCAL_GROUP # 224.0.0.255 - Local scope
```

#### Endereços IPv6
```python
in6addr_any           # ::0 - Qualquer interface
in6addr_loopback      # ::1 - Loopback
in6addr_nodelocal_allnodes  # ff01::1 - Todos nós (interface-local)
in6addr_linklocal_allnodes  # ff02::1 - Todos nós (link-local)
```

#### Constantes de Sistema
```python
SOMAXCONN             # Número máximo de conexões pendentes (128)
NI_MAXHOST            # Tamanho máximo do nome do host (1025)
NI_MAXSERV            # Tamanho máximo do nome do serviço (32)
```

---

## 🔐 CONSIDERAÇÕES DE SEGURANÇA

### Sockets Raw (SOCK_RAW)
```python
# Requer privilégios de root/sudo
# Permite injetar pacotes arbitrários
# Pode ser usado para spoofing, scanning
# Use com cuidado em produção
```

### Validação de Entrada
```python
# Sempre validar:
# - Endereços de origem/destino
# - Tamanho dos dados
# - Flags e opções
# - Permissões de acesso
```

### Hardening de Socket
```python
def secure_socket(sock):
    # 1. Limitar privilégios se possível
    # 2. Configurar timeouts
    sock.settimeout(30.0)
    
    # 3. Habilitar keepalive para detectar conexões mortas
    sock.setsockopt(SOL_SOCKET, SO_KEEPALIVE, 1)
    
    # 4. Desabilitar Nagle para respostas rápidas
    sock.setsockopt(SOL_TCP, TCP_NODELAY, 1)
    
    # 5. Configurar buffers apropriados
    sock.setsockopt(SOL_SOCKET, SO_RCVBUF, 65536)
    sock.setsockopt(SOL_SOCKET, SO_SNDBUF, 65536)
    
    return sock
```

---

## 🚀 OTIMIZAÇÃO DE PERFORMANCE

### Para Servidores de Alta Concorrência
```python
# 1. Habilitar REUSEADDR e REUSEPORT
sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
sock.setsockopt(SOL_SOCKET, SO_REUSEPORT, 1)  # Linux 3.9+

# 2. Aumentar backlog
sock.listen(2048)  # Ao invés do padrão SOMAXCONN (128)

# 3. Configurar buffers grandes
sock.setsockopt(SOL_SOCKET, SO_RCVBUF, 1024 * 1024)  # 1MB
sock.setsockopt(SOL_SOCKET, SO_SNDBUF, 1024 * 1024)  # 1MB

# 4. Desabilitar algoritmo de Nagle
sock.setsockopt(SOL_TCP, TCP_NODELAY, 1)

# 5. Habilitar TCP Fast Open (Linux 3.7+)
sock.setsockopt(SOL_TCP, TCP_FASTOPEN, 5)
```

### Para Clientes de Baixa Latência
```python
# 1. Conectar com timeout
sock.settimeout(5.0)

# 2. Usar non-blocking para múltiplas conexões
sock.setblocking(False)

# 3. Configurar keepalive agressivo
sock.setsockopt(SOL_SOCKET, SO_KEEPALIVE, 1)
sock.setsockopt(SOL_TCP, TCP_KEEPIDLE, 30)
sock.setsockopt(SOL_TCP, TCP_KEEPINTVL, 5)
sock.setsockopt(SOL_TCP, TCP_KEEPCNT, 3)

# 4. Desabilitar Nagle
sock.setsockopt(SOL_TCP, TCP_NODELAY, 1)
```

---

## 📚 REFERÊNCIAS E LINKS ÚTEIS

### RFCs Importantes
- RFC 793 - TCP Specification
- RFC 768 - UDP Specification
- RFC 2460 - IPv6 Specification
- RFC 3493 - Basic Socket Interface Extensions for IPv6
- RFC 3542 - Advanced Sockets API for IPv6

### Man Pages Linux
```
man 2 socket
man 7 ip
man 7 tcp
man 7 udp
man 7 socket
man 7 unix
man 7 packet
man 7 netlink
```

### Ferramentas de Diagnóstico
```bash
# Verificar configurações de socket
ss -tulpn            # Socket statistics
netstat -tulpn       # Network statistics (legado)
lsof -i              # List open files (sockets)

# Depuração de rede
tcpdump -i any       # Capturar pacotes
strace -e network    # Trace chamadas de sistema de rede
```

---

## 🎯 CHECKLIST PARA IMPLEMENTAÇÃO

### Ao Criar um Socket Server
- [ ] Configurar SO_REUSEADDR/SO_REUSEPORT
- [ ] Definir timeout apropriado
- [ ] Configurar tamanho de buffers (SO_RCVBUF/SO_SNDBUF)
- [ ] Habilitar SO_KEEPALIVE se necessário
- [ ] Configurar backlog apropriado em listen()
- [ ] Tratar erros de forma robusta (EAGAIN, ECONNRESET)
- [ ] Limitar acesso por endereço/IP se necessário
- [ ] Usar non-blocking I/O ou threading para múltiplas conexões

### Ao Criar um Socket Client
- [ ] Definir timeout de conexão
- [ ] Tratar falhas de conexão graciosamente
- [ ] Configurar TCP_NODELAY para baixa latência
- [ ] Implementar retry com backoff exponencial
- [ ] Validar dados recebidos
- [ ] Fechar socket adequadamente (with statement)

### Para Sockets Raw/Avançados
- [ ] Verificar privilégios necessários (root)
- [ ] Validar entrada rigorosamente
- [ ] Considerar implicações de segurança
- [ ] Testar em ambiente controlado primeiro
- [ ] Documentar uso específico do protocolo

---

Este manual cobre a vasta maioria das constantes e funcionalidades da biblioteca socket do Python. Para casos de uso específicos ou constantes mais obscuras, consulte a documentação oficial do Linux (`man 2 socket`) ou os códigos-fonte do kernel.

**Lembre-se**: Muitas constantes são específicas do sistema operacional e podem não estar disponíveis em todas as plataformas. Sempre use tratamento de exceções ao acessar constantes que podem não existir:

```python
try:
    from socket import SO_REUSEPORT
    reuseport_available = True
except ImportError:
    reuseport_available = False
    SO_REUSEPORT = None
```
