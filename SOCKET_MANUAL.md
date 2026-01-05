📚 MANUAL COMPLETO DA BIBLIOTECA SOCKET EM PYTHON
📋 ÍNDICE
Famílias de Endereços (Address Families)

Tipos de Socket

Protocolos

Opções de Socket

Constantes de Rede

Funções Principais

Flags e Opções

Constantes Específicas

🌐 FAMÍLIAS DE ENDEREÇOS (Address Families)
Principais Famílias
Constante	Valor	Descrição	Uso Comum
AF_INET	2	IPv4	Redes tradicionais, internet
AF_INET6	10	IPv6	Redes IPv6 modernas
AF_UNIX	1	Unix Domain Socket	Comunicação entre processos no mesmo host
AF_UNSPEC	0	Não especificado	Permite qualquer família de endereços
Famílias Especializadas
Constante	Descrição	Caso de Uso
AF_PACKET	Acesso direto à camada de enlace	Sniffers, raw sockets
AF_NETLINK	Comunicação kernel↔usuário	Configuração de rede do sistema
AF_CAN	Controller Area Network	Redes automotivas, industrial
AF_BLUETOOTH	Bluetooth	Dispositivos sem fio Bluetooth
AF_VSOCK	Virtual Machine Sockets	Comunicação VM↔host
AF_ALG	Linux Kernel Crypto API	Criptografia via kernel
Famílias Legadas/Específicas
Constante	Descrição	Status
AF_APPLETALK	AppleTalk protocol	Legado
AF_AX25	Amateur Radio X.25	Específico
AF_IPX	Novell IPX	Legado
AF_IRDA	Infrared Data Association	Legado
AF_X25	X.25 protocol	Legado
AF_DECnet	DECnet protocol	Legado
AF_ATMPVC	ATM PVCs	Específico
AF_ATMSVC	ATM SVCs	Específico
AF_ROSE	X.25 PLP	Específico
AF_NETROM	NET/ROM	Amateur radio
AF_BRIDGE	Bridging	Kernel bridge
AF_ASH	Ash protocol	Específico
AF_ECONET	Acorn Econet	Legado
AF_LLC	LLC	IEEE 802.2
AF_TIPC	Cluster IPC	Telecom
AF_WANPIPE	Wanpipe API	WAN interfaces
🔌 TIPOS DE SOCKET
Tipos Principais
Tipo	Constante	Descrição	Protocolo Típico
Stream	SOCK_STREAM	Conexão orientada, confiável	TCP
Datagram	SOCK_DGRAM	Sem conexão, não confiável	UDP
Raw	SOCK_RAW	Acesso direto ao protocolo	IP raw
Sequenced Packet	SOCK_SEQPACKET	Stream com limites de mensagem	SCTP
Reliable Datagram	SOCK_RDM	Datagrama confiável	RDS
Flags Adicionais
python
SOCK_CLOEXEC    # Fecha socket ao executar exec()
SOCK_NONBLOCK   # Socket não-bloqueante
📡 PROTOCOLOS
Protocolos de Camada de Transporte
Constante	Valor	Descrição
IPPROTO_TCP	6	Protocolo TCP
IPPROTO_UDP	17	Protocolo UDP
IPPROTO_SCTP	132	Stream Control Transmission Protocol
IPPROTO_UDPLITE	136	UDP-Lite (checksum parcial)
Protocolos de Camada de Rede
Constante	Valor	Descrição
IPPROTO_IP	0	Protocolo IP (pseudo)
IPPROTO_IPV6	41	Protocolo IPv6
IPPROTO_ICMP	1	Internet Control Message Protocol
IPPROTO_ICMPV6	58	ICMP para IPv6
IPPROTO_IGMP	2	Internet Group Management Protocol
IPPROTO_RAW	255	Raw IP packets
Protocolos de Segurança/Extensões
Constante	Descrição
IPPROTO_AH	Authentication Header (IPsec)
IPPROTO_ESP	Encapsulating Security Payload
IPPROTO_GRE	Generic Routing Encapsulation
IPPROTO_MPTCP	Multipath TCP
⚙️ OPÇÕES DE SOCKET (Socket Options)
Níveis de Opção
python
SOL_SOCKET    # Nível socket (geral)
SOL_TCP       # Nível TCP
SOL_UDP       # Nível UDP
SOL_IP        # Nível IP
SOL_IPV6      # Nível IPv6
Opções Gerais (SOL_SOCKET)
Configuração Comportamental
python
SO_REUSEADDR      # Reutilizar endereços locais
SO_REUSEPORT      # Permitir múltiplos sockets na mesma porta
SO_KEEPALIVE      # Manter conexões ativas
SO_BROADCAST      # Permitir transmissão broadcast
SO_DONTROUTE      # Enviar apenas para hosts diretamente conectados
Buffer e Timeouts
python
SO_SNDBUF         # Tamanho do buffer de envio
SO_RCVBUF         # Tamanho do buffer de recepção
SO_SNDTIMEO       # Timeout para envio
SO_RCVTIMEO       # Timeout para recepção
Informação e Controle
python
SO_TYPE           # Obter tipo do socket (read-only)
SO_ERROR          # Obter e limpar erro do socket
SO_DEBUG          # Habilitar debugging
SO_ACCEPTCONN     # Verificar se socket está ouvindo
Opções TCP (SOL_TCP)
Opção	Descrição
TCP_NODELAY	Desabilitar algoritmo de Nagle
TCP_KEEPIDLE	Tempo até iniciar keepalive
TCP_KEEPINTVL	Intervalo entre keepalives
TCP_KEEPCNT	Número de keepalives antes de fechar
TCP_MAXSEG	Tamanho máximo do segmento
TCP_CONGESTION	Algoritmo de controle de congestionamento
TCP_FASTOPEN	Habilitar TCP Fast Open
Opções IP (SOL_IP)
Opção	Descrição
IP_TTL	Time-To-Live dos pacotes
IP_TOS	Type Of Service
IP_OPTIONS	Opções de cabeçalho IP
IP_HDRINCL	Incluir cabeçalho IP nos dados
IP_ADD_MEMBERSHIP	Adicionar a grupo multicast
IP_DROP_MEMBERSHIP	Remover de grupo multicast
Opções IPv6 (SOL_IPV6)
Opção	Descrição
IPV6_V6ONLY	Usar apenas IPv6
IPV6_UNICAST_HOPS	TTL para unicast
IPV6_MULTICAST_HOPS	TTL para multicast
IPV6_JOIN_GROUP	Juntar-se a grupo multicast
IPV6_LEAVE_GROUP	Sair de grupo multicast
🎯 FLAGS E OPÇÕES
Flags para send()/recv()
Flag	Descrição
MSG_DONTWAIT	Operação não-bloqueante
MSG_OOB	Dados out-of-band
MSG_PEEK	Ler dados sem removê-los do buffer
MSG_WAITALL	Esperar até receber todos os dados
MSG_TRUNC	Retornar comprimento real do datagrama
MSG_CTRUNC	Dados de controle truncados
MSG_EOR	Fim de registro
MSG_NOSIGNAL	Não gerar SIGPIPE
Flags para getaddrinfo()
Flag	Descrição
AI_PASSIVE	Usar para bind() (endereço wildcard)
AI_CANONNAME	Retornar nome canônico
AI_NUMERICHOST	Nome do host é endereço numérico
AI_NUMERICSERV	Serviço é número de porta
AI_V4MAPPED	Mapear IPv6 para IPv4 se necessário
AI_ALL	Retornar todos os endereços
AI_ADDRCONFIG	Retornar apenas endereços configurados
Flags para shutdown()
Constante	Descrição
SHUT_RD	Fechar leitura
SHUT_WR	Fechar escrita
SHUT_RDWR	Fechar leitura e escrita
🔧 CONSTANTES ESPECÍFICAS
Endereços Especiais IPv4
Constante	Valor	Descrição
INADDR_ANY	0.0.0.0	Aceitar conexões em qualquer interface
INADDR_LOOPBACK	127.0.0.1	Interface de loopback
INADDR_BROADCAST	255.255.255.255	Endereço de broadcast
INADDR_NONE	255.255.255.255	Endereço inválido
Portas
Constante	Valor	Descrição
IPPORT_RESERVED	1024	Início das portas não-reservadas
IPPORT_USERRESERVED	5000	Final das portas reservadas ao usuário
Ethernet
Constante	Valor	Descrição
ETH_P_ALL	0x0003	Todos os protocolos
ETH_P_IP	0x0800	Protocolo IP
ETH_P_ARP	0x0806	Protocolo ARP
ETH_P_IPV6	0x86DD	Protocolo IPv6
Bluetooth
Constante	Descrição
BTPROTO_HCI	Host Controller Interface
BTPROTO_L2CAP	Logical Link Control and Adaptation Protocol
BTPROTO_RFCOMM	Radio Frequency Communication
BTPROTO_SCO	Synchronous Connection-Oriented
BDADDR_ANY	Endereço Bluetooth qualquer
BDADDR_LOCAL	Endereço Bluetooth local
CAN (Controller Area Network)
Constante	Descrição
CAN_RAW	Socket CAN raw
CAN_BCM	Broadcast Manager
CAN_ISOTP	ISO-TP (Transport Protocol)
CAN_J1939	J1939 protocol
CAN_EFF_FLAG	Frame de formato estendido
CAN_RTR_FLAG	Frame de requisição remota
Netlink
Constante	Descrição
NETLINK_ROUTE	Tabelas de roteamento/interface
NETLINK_FIREWALL	Netfilter firewall
NETLINK_NFLOG	Netfilter logging
NETLINK_XFRM	IPsec
NETLINK_CRYPTO	Criptografia
🛠️ FUNÇÕES PRINCIPAIS
Criação e Configuração
python
# Criar socket
socket(family=AF_INET, type=SOCK_STREAM, proto=IPPROTO_TCP)

# Criar conexão
create_connection((host, port))

# Criar servidor
create_server((host, port))

# Socket pair
socketpair(family=AF_UNIX, type=SOCK_STREAM)
Resolução de Nomes
python
# DNS lookup
gethostbyname("example.com")
gethostbyaddr("8.8.8.8")
getaddrinfo("host", "port", family, type, proto, flags)

# Name service
getservbyname("http", "tcp")
getservbyport(80, "tcp")

# Protocolo
getprotobyname("tcp")
Conversão de Endereços
python
# IPv4
inet_aton("192.168.1.1")  # String para binário
inet_ntoa(b'\xc0\xa8\x01\x01')  # Binário para string

# IPv4/IPv6
inet_pton(AF_INET, "192.168.1.1")
inet_ntop(AF_INET, b'\xc0\xa8\x01\x01')
Byte Order (Endianness)
python
htonl(12345)  # Host para network (long)
htons(12345)  # Host para network (short)
ntohl(value)  # Network para host (long)
ntohs(value)  # Network para host (short)
Informação de Rede
python
gethostname()           # Nome do host local
getfqdn()               # Nome de domínio totalmente qualificado
getnameinfo(sockaddr)   # Nome para endereço
if_nameindex()          # Lista interfaces de rede
if_indextoname(index)   # Índice para nome de interface
if_nametoindex(name)    # Nome para índice de interface
💡 EXEMPLOS PRÁTICOS
1. Socket TCP Básico
python
import socket

# Servidor
server = socket.socket(AF_INET, SOCK_STREAM)
server.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
server.bind(('0.0.0.0', 8080))
server.listen(SOMAXCONN)

# Cliente
client = socket.socket(AF_INET, SOCK_STREAM)
client.connect(('localhost', 8080))
2. Socket UDP com Opções
python
sock = socket.socket(AF_INET, SOCK_DGRAM)
sock.setsockopt(SOL_SOCKET, SO_BROADCAST, 1)  # Permitir broadcast
sock.bind(('', 9999))
3. Socket Raw para ICMP
python
# Requer privilégios de root
sock = socket.socket(AF_INET, SOCK_RAW, IPPROTO_ICMP)
sock.setsockopt(SOL_IP, IP_HDRINCL, 1)
4. IPv6 com Dual Stack
python
sock = socket.socket(AF_INET6, SOCK_STREAM)
sock.setsockopt(SOL_IPV6, IPV6_V6ONLY, 0)  # Permitir IPv4 também
5. Socket Unix Domain
python
sock = socket.socket(AF_UNIX, SOCK_STREAM)
sock.bind('/tmp/mysocket.sock')
6. Multicast IPv4
python
sock = socket.socket(AF_INET, SOCK_DGRAM)

# Adicionar ao grupo multicast
mreq = socket.inet_aton('224.1.1.1') + socket.inet_aton('192.168.1.1')
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)
⚠️ CÓDIGOS DE ERRO IMPORTANTES
Constante	Valor	Descrição
EAGAIN / EWOULDBLOCK	11	Recurso temporariamente indisponível
EBADF	9	Bad file descriptor
EAI_ADDRFAMILY	-9	Família de endereço não suportada
EAI_AGAIN	-3	Falha temporária em resolução de nome
EAI_NONAME	-2	Nome ou serviço desconhecido
📊 TABELAS DE REFERÊNCIA RÁPIDA
Famílias Mais Usadas
text
IPv4: AF_INET + SOCK_STREAM + IPPROTO_TCP
IPv6: AF_INET6 + SOCK_STREAM + IPPROTO_TCP
UDP: AF_INET + SOCK_DGRAM + IPPROTO_UDP
Unix: AF_UNIX + SOCK_STREAM
Raw: AF_PACKET + SOCK_RAW
Opções Mais Comuns
text
Reuse Address: SO_REUSEADDR
Keep Alive: SO_KEEPALIVE
No Delay (TCP): TCP_NODELAY
Non-blocking: MSG_DONTWAIT ou SOCK_NONBLOCK
Portas Bem Conhecidas
python
HTTP = 80
HTTPS = 443
SSH = 22
DNS = 53
DHCP = 67
🔍 NOTAS IMPORTANTES
Privilégios: Sockets raw (SOCK_RAW) requerem privilégios de root

Portabilidade: Algumas constantes são específicas do Linux

Performance: Use SO_REUSEPORT para balanceamento de carga em servidores

IPv6: Sempre teste com has_ipv6 se precisar de suporte a IPv6

Timeout: Configure timeouts para evitar bloqueios eternos

Buffer: Ajuste buffers
