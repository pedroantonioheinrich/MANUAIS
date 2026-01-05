# 📚 MANUAL COMPLETO DO SCAPY EM PYTHON

## 📋 ÍNDICE
1. [Introdução ao Scapy](#introdução-ao-scapy)
2. [Camadas de Protocolo](#camadas-de-protocolo)
3. [Funções Principais](#funções-principais)
4. [Operações Básicas](#operações-básicas)
5. [Sniffing e Captura](#sniffing-e-captura)
6. [Envio de Pacotes](#envio-de-pacotes)
7. [Protocolos Suportados](#protocolos-suportados)
8. [Técnicas Avançadas](#técnicas-avançadas)
9. [Ferramentas e Utilitários](#ferramentas-e-utilitários)
10. [Exemplos Práticos](#exemplos-práticos)

---

## 🎯 INTRODUÇÃO AO SCAPY

### O que é Scapy?
Scapy é uma poderosa biblioteca Python para manipulação, criação, envio, captura e análise de pacotes de rede. Funciona como uma ferramenta de linha de comando interativa e também como uma biblioteca.

### Instalação
```bash
# Instalação básica
pip install scapy

# Instalação completa com todas as dependências
pip install scapy[complete]

# Para visualização gráfica
pip install scapy[visualization]
```

### Importação e Inicialização
```python
from scapy.all import *
import scapy.layers.all as layers
```

### Modo Interativo
```python
# Iniciar sessão interativa
interactive()

# Ou no terminal
python -m scapy
```

---

## 📦 CAMADAS DE PROTOCOLO

### Camadas Básicas

| Camada | Classe | Descrição |
|--------|--------|-----------|
| **Ethernet** | `Ether()` | Camada 2 - Enlace |
| **IP** | `IP()` | Camada 3 - Rede (IPv4) |
| **IPv6** | `IPv6()` | Camada 3 - Rede (IPv6) |
| **TCP** | `TCP()` | Camada 4 - Transporte (TCP) |
| **UDP** | `UDP()` | Camada 4 - Transporte (UDP) |
| **ICMP** | `ICMP()` | Mensagens de Controle |
| **ARP** | `ARP()` | Resolução de Endereços |

### Hierarquia de Camadas
```
Packet
├── Ether
│   ├── Dot1Q (VLAN)
│   ├── Dot1AD (Q-in-Q)
│   └── LLC
├── IP / IPv6
│   ├── ICMP / ICMPv6
│   ├── TCP
│   │   ├── Raw (dados)
│   │   └── Outras aplicações
│   ├── UDP
│   │   └── DNS, DHCP, etc.
│   └── Outros protocolos
└── RadioTap (WiFi)
```

### Criação de Pacotes
```python
# Pacote básico
packet = Ether()/IP()/TCP()

# Com campos específicos
packet = Ether(src="00:11:22:33:44:55", dst="ff:ff:ff:ff:ff:ff") \
         / IP(src="192.168.1.1", dst="192.168.1.100") \
         / TCP(sport=1234, dport=80) \
         / Raw(load="GET / HTTP/1.1\r\n\r\n")
```

---

## 🛠️ FUNÇÕES PRINCIPAIS

### Funções de Criação e Manipulação

| Função | Descrição | Exemplo |
|--------|-----------|---------|
| `Packet()` | Cria pacote vazio | `p = Packet()` |
| `Ether()` | Cria cabeçalho Ethernet | `e = Ether(dst="ff:ff:ff:ff:ff:ff")` |
| `IP()` | Cria cabeçalho IP | `ip = IP(src="192.168.1.1")` |
| `TCP()`/`UDP()` | Cria cabeçalho TCP/UDP | `t = TCP(dport=80)` |
| `Raw()` | Adiciona dados brutos | `r = Raw(load="dados")` |
| `fuzz()` | Cria pacote com campos aleatórios | `p = fuzz(IP()/TCP())` |

### Funções de Visualização

| Função | Descrição | Exemplo |
|--------|-----------|---------|
| `show()` | Exibe resumo do pacote | `packet.show()` |
| `show2()` | Exibe como seria no wire | `packet.show2()` |
| `hexdump()` | Exibe hex dump | `hexdump(packet)` |
| `pdfdump()` | Gera PDF do pacote | `pdfdump([packet1, packet2])` |
| `wrpcap()` | Salva pacotes em arquivo | `wrpcap("captura.pcap", packets)` |

### Funções de Análise

| Função | Descrição | Exemplo |
|--------|-----------|---------|
| `ls()` | Lista campos de uma camada | `ls(TCP)` |
| `lsc()` | Lista comandos disponíveis | `lsc()` |
| `explore()` | Interface gráfica | `packet.explore()` |
| `command()` | Executa comando shell | `command("ifconfig")` |

---

## 🔍 OPERAÇÕES BÁSICAS

### Acesso a Campos
```python
# Acesso direto
packet[IP].src = "192.168.1.1"
packet[TCP].dport = 443

# Acesso por camada
packet.getlayer(IP).ttl = 64

# Verificar existência de camada
if IP in packet:
    print("Tem camada IP")

# Acesso aos dados (payload)
raw_data = packet[Raw].load
```

### Modificação de Pacotes
```python
# Clonar pacote
packet2 = packet.copy()

# Remover camada
del packet[TCP]

# Adicionar camada
packet = packet/IP()/TCP()

# Modificar campos em massa
packet[IP].setfieldval("src", "10.0.0.1")
```

### Análise de Pacotes
```python
# Resumo textual
print(packet.summary())

# Detalhes completos
packet.show()

# Campos específicos
print(packet[IP].src, packet[IP].dst)

# Hex dump
hexdump(packet)
```

### Iteração e Filtragem
```python
# Iterar sobre pacotes
for pkt in packets:
    if pkt.haslayer(TCP):
        print(pkt.summary())

# Filtrar por condição
tcp_packets = [p for p in packets if TCP in p]

# Usar lambda para filtro complexo
filtered = filter(lambda x: x.haslayer(TCP) and x[TCP].dport == 80, packets)
```

---

## 👃 SNIFFING E CAPTURA

### Função `sniff()`

```python
# Captura básica
packets = sniff(count=10)

# Com filtro
packets = sniff(filter="tcp port 80", count=20)

# Em interface específica
packets = sniff(iface="eth0", count=50)

# Com callback
def process_packet(packet):
    print(packet.summary())

sniff(prn=process_packet, count=10)

# Com timeout
packets = sniff(timeout=30)

# Store=0 para não armazenar (só callback)
sniff(prn=process_packet, store=0)
```

### Parâmetros do `sniff()`

| Parâmetro | Descrição | Padrão |
|-----------|-----------|--------|
| `count` | Número de pacotes a capturar | 0 (infinito) |
| `timeout` | Tempo máximo em segundos | None |
| `iface` | Interface de rede | None (todas) |
| `filter` | Filtro BPF (tcpdump) | None |
| `prn` | Função callback | None |
| `store` | Armazenar pacotes | 1 |
| `stop_filter` | Função para parar | None |
| `lfilter` | Filtro Python | None |

### Filtros BPF (Berkeley Packet Filter)

```python
# Filtros comuns
"tcp"                    # Apenas TCP
"udp"                    # Apenas UDP
"icmp"                   # Apenas ICMP
"port 80"                # Porta 80 (origem ou destino)
"dst port 443"           # Destino porta 443
"src host 192.168.1.1"   # Origem específica
"net 192.168.1.0/24"     # Rede específica
"tcp and portrange 1-1024" # Portas baixas
"broadcast"              # Broadcast packets
"multicast"              # Multicast packets
```

### Captura Avançada

```python
# Captura assíncrona
from scapy.all import AsyncSniffer

sniffer = AsyncSniffer(iface="eth0", filter="tcp")
sniffer.start()
# ... fazer outras coisas ...
packets = sniffer.stop()

# Captura com thread
from threading import Thread

def capture():
    global packets
    packets = sniff(timeout=10)

t = Thread(target=capture)
t.start()
t.join()

# Captura contínua para arquivo
sniff(offline=None, count=0, timeout=60, 
      prn=lambda x: wrpcap("captura.pcap", x, append=True))
```

### Leitura de Arquivos PCAP

```python
# Ler arquivo pcap
packets = rdpcap("captura.pcap")

# Ler linha a linha
for pkt in PcapReader("captura.pcap"):
    process(pkt)

# Ler múltiplos arquivos
packets = rdpcap("*.pcap")

# Converter para lista
packet_list = list(packets)
```

---

## 🚀 ENVIO DE PACOTES

### Funções de Envio

| Função | Descrição | Camada OSI |
|--------|-----------|------------|
| `send()` | Envia pacote na camada 3 | Rede |
| `sendp()` | Envia pacote na camada 2 | Enlace |
| `sr()` | Envia e recebe resposta (L3) | Rede |
| `sr1()` | Envia e recebe 1 resposta | Rede |
| `srp()` | Envia e recebe resposta (L2) | Enlace |
| `srloop()` | Envia múltiplas vezes | Rede |

### Envio Básico

```python
# Envio simples (camada 3)
send(IP(dst="192.168.1.1")/ICMP())

# Envio com Ether (camada 2)
sendp(Ether()/IP(dst="192.168.1.1")/ICMP())

# Envio para broadcast
sendp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24"))

# Envio com intervalo
send(IP(dst="192.168.1.1")/ICMP(), inter=0.5, count=10)
```

### Envio e Recepção

```python
# Enviar e esperar resposta
ans, unans = sr(IP(dst="192.168.1.1")/ICMP(), timeout=2)

# Apenas primeira resposta
response = sr1(IP(dst="192.168.1.1")/ICMP(), timeout=2)

# Envio em loop (para testes)
srloop(IP(dst="192.168.1.1")/ICMP(), count=5)

# Envio com filtro de resposta
ans, unans = sr(IP(dst="192.168.1.1")/TCP(dport=80), 
                filter=lambda x: x.haslayer(TCP))
```

### Opções de Envio

```python
# Parâmetros comuns
send(packet, 
     inter=0,          # Intervalo entre pacotes
     count=1,          # Número de pacotes
     iface=None,       # Interface específica
     verbose=None,     # Nível de verbosidade
     loop=0,           # Loop infinito (se inter>0)
     )

# Envio rápido (sem confirmação)
send(packet, verbose=0)

# Envio com MTU específico
send(fragment(packet, fragsize=500))
```

### Envio de Pacotes Fragmentados

```python
# Fragmentação IP
packet = IP(dst="192.168.1.1")/ICMP()/("X"*4000)
frags = fragment(packet, fragsize=500)
for frag in frags:
    send(frag)

# Fragmentação com overlap (para testes de segurança)
send(fragment(packet, fragsize=500, overlap=100))
```

---

## 🌐 PROTOCOLOS SUPORTADOS

### Camada 2 (Enlace)

```python
# Ethernet
Ether(src="00:11:22:33:44:55", dst="ff:ff:ff:ff:ff:ff", type=0x0800)

# VLAN (802.1Q)
Dot1Q(vlan=100, prio=0, id=0, type=0x0800)

# ARP
ARP(op=1, hwsrc="00:11:22:33:44:55", psrc="192.168.1.1", 
    hwdst="00:00:00:00:00:00", pdst="192.168.1.2")

# STP (Spanning Tree)
STP(bpdutype=0x00, bpduflags=0x00, rootid=0, 
    rootpathcost=0, bridgeid=0, portid=0x8001)

# LLDP
LLDPDU()
```

### Camada 3 (Rede)

```python
# IPv4
IP(version=4, ihl=5, tos=0, len=None, id=1, flags=0, 
   frag=0, ttl=64, proto=6, chksum=None, 
   src="192.168.1.1", dst="192.168.1.2")

# IPv6
IPv6(version=6, tc=0, fl=0, plen=None, nh=6, hlim=64,
     src="2001:db8::1", dst="2001:db8::2")

# ICMP
ICMP(type=8, code=0)  # Echo Request
ICMP(type=0, code=0)  # Echo Reply
ICMP(type=3, code=3)  # Port Unreachable

# ICMPv6
ICMPv6EchoRequest()
ICMPv6EchoReply()
ICMPv6ND_NS()  # Neighbor Solicitation
```

### Camada 4 (Transporte)

```python
# TCP
TCP(sport=RandShort(), dport=80, seq=RandInt(), 
    ack=0, flags="S", window=8192, urgptr=0)

# UDP
UDP(sport=RandShort(), dport=53, len=None, chksum=None)

# SCTP
SCTP(sport=2905, dport=2905, tag=0, chksum=None)

# DCCP
DCCP(sport=RandShort(), dport=5001, chksum=None)
```

### Camada de Aplicação

```python
# DNS
DNS(id=0, qr=0, opcode=0, rd=1, qd=DNSQR(qname="example.com"))

# DHCP
DHCP(options=[("message-type", "discover"), "end"])

# HTTP
Raw(load="GET / HTTP/1.1\r\nHost: example.com\r\n\r\n")

# TLS/SSL
TLS(version=0x0303, type=22)  # Handshake

# SMTP
Raw(load="HELO example.com\r\n")

# FTP
Raw(load="USER anonymous\r\n")
```

### Protocolos Especiais

```python
# WiFi (802.11)
RadioTap()/Dot11(type=0, subtype=8, addr1="ff:ff:ff:ff:ff:ff")

# Bluetooth
HCI_Hdr()/HCI_ACL_Hdr()/L2CAP_Hdr()

# Industrial (Modbus)
ModbusADU()/ModbusPDU01ReadCoilsRequest()

# Automotive (CAN)
CAN(identifier=0x123, length=8, data="\x01\x02\x03\x04\x05\x06\x07\x08")
```

---

## 🚀 TÉCNICAS AVANÇADAS

### Crafting de Pacotes

```python
# Pacote personalizado completo
packet = (Ether(src="00:11:22:33:44:55", dst="ff:ff:ff:ff:ff:ff") /
          IP(src="192.168.1.1", dst="192.168.1.255", ttl=64) /
          UDP(sport=12345, dport=9999) /
          Raw(load="Custom Protocol Data\x00\x01\x02"))

# Pacote com campos calculados automaticamente
packet = IP(dst="192.168.1.1")/ICMP()
del packet[IP].chksum  # Recalcular checksum
packet = packet.__class__(bytes(packet))  # Reconstruir com checksum correto
```

### Protocolos Customizados

```python
# Definir novo protocolo
from scapy.all import Packet, BitField, StrField

class MyProtocol(Packet):
    name = "MyProtocol"
    fields_desc = [
        BitField("version", 1, 4),
        BitField("type", 0, 4),
        StrField("data", "", fmt="B")
    ]

# Bind para porta específica
bind_layers(UDP, MyProtocol, dport=9999)

# Usar protocolo customizado
packet = IP()/UDP(dport=9999)/MyProtocol(data="test")
```

### Técnicas de Evasão

```python
# TTL manipulation
packet = IP(dst="target", ttl=255)
for ttl in range(1, 30):
    packet.ttl = ttl
    send(packet)

# Fragmentação
packet = IP(dst="target")/TCP(dport=80)/"GET / HTTP/1.0\r\n\r\n"
send(fragment(packet, fragsize=8))

# TCP flags manipulation
flags_combinations = ["S", "SA", "FA", "RA", "PAU"]
for flags in flags_combinations:
    send(IP(dst="target")/TCP(dport=80, flags=flags))
```

### Análise de Tráfego

```python
# Estatísticas de protocolos
from collections import Counter

def protocol_stats(packets):
    protocols = Counter()
    for pkt in packets:
        for layer in pkt.layers():
            protocols[layer.__name__] += 1
    return protocols

# Análise de conversações
from scapy.all import conversations

pkts = rdpcap("captura.pcap")
conv = conversations(pkts)
for key, packets in conv.items():
    print(f"Conversação: {key}, Pacotes: {len(packets)}")

# Extração de dados
def extract_http_headers(packets):
    for pkt in packets:
        if pkt.haslayer(TCP) and pkt.haslayer(Raw):
            raw = pkt[Raw].load.decode('utf-8', errors='ignore')
            if "HTTP" in raw:
                print(raw.split('\r\n')[0])
```

---

## 🛠️ FERRAMENTAS E UTILITÁRIOS

### Scapy como Ferramenta CLI

```bash
# Executar comando Scapy
echo "send(IP(dst='192.168.1.1')/ICMP())" | python -m scapy

# Executar script Scapy
python -m scapy -c myscript.py

# Modo interativo com comandos
python -m scapy
>>> conf.iface = "eth0"
>>> pkts = sniff(count=10)
>>> pkts.summary()
```

### Configuração

```python
# Configurações globais
conf.verb = 2                # Nível de verbosidade (0-3)
conf.iface = "eth0"          # Interface padrão
conf.color_theme = BrightTheme()  # Tema de cores
conf.use_pcap = True         # Usar libpcap se disponível
conf.use_dnet = True         # Usar libdnet/raw sockets
conf.raw_layer = None        # Camada raw padrão
conf.layers.filtered = False # Carregar todas as camadas

# Configurações de rede
conf.route.add(net="192.168.1.0/24", gw="192.168.1.1")
conf.route.resync()  # Resincronizar tabela de roteamento
```

### Utilitários de Rede

```python
# Traceroute
result, unans = traceroute("google.com", maxttl=30)

# ARP scan
ans, unans = arping("192.168.1.0/24")

# Port scan
ans, unans = sr(IP(dst="192.168.1.1")/TCP(dport=(1,1024)), timeout=1)

# DNS resolution
ans = sr1(IP(dst="8.8.8.8")/UDP()/DNS(rd=1, qd=DNSQR(qname="google.com")))

# DHCP discovery
dhcp_discover = Ether(dst="ff:ff:ff:ff:ff:ff")/IP(src="0.0.0.0", 
                    dst="255.255.255.255")/UDP(sport=68, dport=67)/BOOTP()/DHCP()
```

### Visualização Gráfica

```python
# Plot de traceroute
result, unans = traceroute("google.com", maxttl=20)
result.plot()

# Matriz de conversações
from scapy.all import TCP_conversations

pkts = rdpcap("captura.pcap")
conv = TCP_conversations(pkts)
conv.plot()

# Gráfico 3D (requer matplotlib)
from scapy.all import plot3d

pkts = rdpcap("captura.pcap")
plot3d(pkts, z_axis="ttl")
```

### Exportação e Importação

```python
# Exportar para diferentes formatos
wrpcap("captura.pcap", packets)            # PCAP
export_object(packets, "packets.pkl")      # Pickle
hex_bytes = raw(packet).hex()              # Hex string

# Importar de diferentes formatos
packets = rdpcap("captura.pcap")           # PCAP
packets = import_object("packets.pkl")     # Pickle
packet = Ether(b'\x00\x11\x22\x33\x44\x55') # Bytes

# Converter entre formatos
from scapy.all import PcapWriter, PcapNgWriter

with PcapWriter("output.pcap", append=True) as pcap_writer:
    pcap_writer.write(packet)

with PcapNgWriter("output.pcapng") as pcapng_writer:
    pcapng_writer.write(packet)
```

---

## 🎯 EXEMPLOS PRÁTICOS

### 1. Scanner de Portas TCP

```python
from scapy.all import *
import sys

def tcp_scan(target, ports):
    """Scanner de portas TCP SYN"""
    print(f"Scanning {target}...")
    
    for port in ports:
        # Envia SYN
        pkt = IP(dst=target)/TCP(dport=port, flags="S")
        resp = sr1(pkt, timeout=1, verbose=0)
        
        if resp is None:
            print(f"Port {port}: Filtered/No response")
        elif resp.haslayer(TCP):
            if resp[TCP].flags == 0x12:  # SYN-ACK
                # Envia RST para fechar conexão
                send(IP(dst=target)/TCP(dport=port, flags="R"), verbose=0)
                print(f"Port {port}: OPEN")
            elif resp[TCP].flags == 0x14:  # RST-ACK
                print(f"Port {port}: CLOSED")
        else:
            print(f"Port {port}: Unknown response")

# Uso
if __name__ == "__main__":
    target = "192.168.1.1"
    ports = range(1, 1025)
    tcp_scan(target, ports)
```

### 2. Sniffer de Credenciais HTTP

```python
from scapy.all import *
import re

def http_sniffer(packet):
    if packet.haslayer(TCP) and packet.haslayer(Raw):
        try:
            payload = packet[Raw].load.decode('utf-8', errors='ignore')
            
            # Procura por credenciais
            if "POST" in payload or "GET" in payload:
                # Procura por usuários e senhas
                user_match = re.search(r'[Uu]ser(name)?=([^&\s]+)', payload)
                pass_match = re.search(r'[Pp]ass(word)?=([^&\s]+)', payload)
                
                if user_match or pass_match:
                    print(f"\n[+] Credenciais encontradas:")
                    print(f"   Origem: {packet[IP].src}:{packet[TCP].sport}")
                    print(f"   Destino: {packet[IP].dst}:{packet[TCP].dport}")
                    
                    if user_match:
                        print(f"   Usuário: {user_match.group(2)}")
                    if pass_match:
                        print(f"   Senha: {pass_match.group(2)}")
                        
                    # Extrai URL se disponível
                    url_match = re.search(r'Host: ([^\r\n]+)', payload)
                    if url_match:
                        print(f"   Host: {url_match.group(1)}")
                        
        except:
            pass

# Captura pacotes HTTP
sniff(filter="tcp port 80", prn=http_sniffer, store=0)
```

### 3. Detector de ARP Spoofing

```python
from scapy.all import *
import time

# Dicionário para armazenar mapeamentos IP-MAC
arp_table = {}

def arp_monitor(packet):
    if packet.haslayer(ARP):
        op = packet[ARP].op  # 1=request, 2=reply
        src_ip = packet[ARP].psrc
        src_mac = packet[ARP].hwsrc
        
        # Verifica se já temos esse IP mapeado
        if src_ip in arp_table:
            if arp_table[src_ip] != src_mac:
                print(f"\n[!] ALERTA: Possível ARP Spoofing detectado!")
                print(f"    IP: {src_ip}")
                print(f"    MAC antigo: {arp_table[src_ip]}")
                print(f"    MAC novo: {src_mac}")
                print(f"    Time: {time.ctime()}")
                print("-" * 50)
        else:
            # Adiciona novo mapeamento
            arp_table[src_ip] = src_mac
            print(f"[+] Novo mapeamento: {src_ip} -> {src_mac}")

# Inicia monitoramento
print("[*] Iniciando monitoramento ARP...")
print("[*] Pressione Ctrl+C para parar")
sniff(filter="arp", prn=arp_monitor, store=0)
```

### 4. Gerador de Tráfego de Teste

```python
from scapy.all import *
import random

def generate_test_traffic(target_ip, count=100):
    """Gera tráfego de teste variado"""
    
    protocols = [
        lambda: IP(dst=target_ip)/ICMP(),  # ICMP
        lambda: IP(dst=target_ip)/UDP(dport=random.randint(1, 65535)),  # UDP
        lambda: IP(dst=target_ip)/TCP(dport=random.choice([80, 443, 22, 21]))  # TCP
    ]
    
    sizes = [64, 128, 256, 512, 1024, 1500]
    
    print(f"[*] Gerando {count} pacotes de teste para {target_ip}")
    
    for i in range(count):
        # Escolhe protocolo aleatório
        pkt_func = random.choice(protocols)
        packet = pkt_func()
        
        # Adiciona dados aleatórios
        if random.choice([True, False]):
            data_size = random.choice(sizes)
            packet = packet/Raw(load=os.urandom(data_size))
        
        # Envia pacote
        send(packet, verbose=0)
        
        # Progresso
        if (i + 1) % 10 == 0:
            print(f"    Enviados {i + 1}/{count} pacotes")
    
    print("[+] Geração de tráfego concluída")

# Uso
generate_test_traffic("192.168.1.100", count=50)
```

### 5. Analisador de Protocolos

```python
from scapy.all import *
from collections import defaultdict

def protocol_analyzer(pcap_file):
    """Analisa arquivo pcap e gera estatísticas"""
    
    packets = rdpcap(pcap_file)
    
    stats = {
        'total_packets': len(packets),
        'protocols': defaultdict(int),
        'top_talkers': defaultdict(int),
        'port_usage': defaultdict(int),
        'packet_sizes': [],
        'time_range': None
    }
    
    if packets:
        stats['time_range'] = (packets[0].time, packets[-1].time)
    
    for pkt in packets:
        # Conta protocolos
        for layer in pkt.layers():
            stats['protocols'][layer.__name__] += 1
        
        # Top talkers
        if IP in pkt:
            stats['top_talkers'][pkt[IP].src] += 1
            stats['top_talkers'][pkt[IP].dst] += 1
        
        # Uso de portas
        if TCP in pkt:
            stats['port_usage'][f"TCP:{pkt[TCP].dport}"] += 1
            stats['port_usage'][f"TCP:{pkt[TCP].sport}"] += 1
        elif UDP in pkt:
            stats['port_usage'][f"UDP:{pkt[UDP].dport}"] += 1
            stats['port_usage'][f"UDP:{pkt[UDP].sport}"] += 1
        
        # Tamanhos de pacotes
        stats['packet_sizes'].append(len(pkt))
    
    # Ordena resultados
    stats['top_protocols'] = sorted(stats['protocols'].items(), 
                                    key=lambda x: x[1], reverse=True)[:10]
    stats['top_talkers'] = sorted(stats['top_talkers'].items(), 
                                  key=lambda x: x[1], reverse=True)[:10]
    stats['top_ports'] = sorted(stats['port_usage'].items(), 
                                key=lambda x: x[1], reverse=True)[:10]
    
    return stats

# Uso
if __name__ == "__main__":
    stats = protocol_analyzer("captura.pcap")
    
    print(f"Total de pacotes: {stats['total_packets']}")
    print(f"Range de tempo: {stats['time_range']}")
    
    print("\nTop 10 Protocolos:")
    for proto, count in stats['top_protocols']:
        print(f"  {proto}: {count}")
    
    print("\nTop 10 Endereços:")
    for addr, count in stats['top_talkers']:
        print(f"  {addr}: {count}")
```

### 6. Ferramenta de Traceroute Avançada

```python
from scapy.all import *
import time

def advanced_traceroute(target, max_hops=30, timeout=2):
    """Traceroute com detecção de firewalls e análise de TTL"""
    
    print(f"[*] Iniciando traceroute para {target}")
    print("-" * 60)
    
    results = []
    
    for ttl in range(1, max_hops + 1):
        # Envia pacote ICMP
        pkt = IP(dst=target, ttl=ttl)/ICMP()
        start_time = time.time()
        resp = sr1(pkt, timeout=timeout, verbose=0)
        rtt = (time.time() - start_time) * 1000  # ms
        
        if resp is None:
            print(f"{ttl:2d}: * * *")
            results.append((ttl, None, None, rtt))
        elif resp.haslayer(ICMP):
            if resp[ICMP].type == 11:  # Time Exceeded
                print(f"{ttl:2d}: {resp.src:15s} {rtt:6.2f} ms (TTL Exceeded)")
                results.append((ttl, resp.src, "TTL Exceeded", rtt))
            elif resp[ICMP].type == 0:  # Echo Reply
                print(f"{ttl:2d}: {resp.src:15s} {rtt:6.2f} ms (DESTINATION)")
                results.append((ttl, resp.src, "Destination", rtt))
                break
            else:
                print(f"{ttl:2d}: {resp.src:15s} {rtt:6.2f} ms (ICMP Type: {resp[ICMP].type})")
                results.append((ttl, resp.src, f"ICMP Type {resp[ICMP].type}", rtt))
        else:
            print(f"{ttl:2d}: {resp.src:15s} {rtt:6.2f} ms (Unexpected)")
            results.append((ttl, resp.src, "Unexpected", rtt))
    
    print("-" * 60)
    print("[+] Traceroute concluído")
    
    # Análise adicional
    analyze_traceroute(results)
    
    return results

def analyze_traceroute(results):
    """Analisa resultados do traceroute"""
    
    print("\n[+] Análise de Resultados:")
    
    # Calcula estatísticas
    successful_hops = [r for r in results if r[1] is not None]
    failed_hops = [r for r in results if r[1] is None]
    
    print(f"   Total de hops: {len(results)}")
    print(f"   Hops respondidos: {len(successful_hops)}")
    print(f"   Hops não respondidos: {len(failed_hops)}")
    
    # Detecta padrões de firewall
    consecutive_fails = 0
    max_consecutive = 0
    
    for r in results:
        if r[1] is None:
            consecutive_fails += 1
            max_consecutive = max(max_consecutive, consecutive_fails)
        else:
            consecutive_fails = 0
    
    if max_consecutive >= 3:
        print(f"   [!] Possível firewall bloqueando (falhas consecutivas: {max_consecutive})")
    
    # Verifica se chegou ao destino
    destination_reached = any(r[2] == "Destination" for r in results)
    if destination_reached:
        print(f"   [✓] Destino alcançado")
    else:
        print(f"   [✗] Destino não alcançado")

# Uso
if __name__ == "__main__":
    results = advanced_traceroute("8.8.8.8")
```

---

## ⚠️ CONSIDERAÇÕES DE SEGURANÇA E ÉTICA

### Boas Práticas

```python
# 1. SEMPRE obtenha permissão antes de testar
# 2. Use apenas em redes próprias ou autorizadas
# 3. Monitore o impacto na rede
# 4. Documente todas as atividades
# 5. Use com responsabilidade

# Exemplo de script seguro
def safe_scan(network, ports):
    """Scanner seguro com limites"""
    
    # Verifica se a rede é autorizada
    authorized_networks = ["192.168.1.0/24", "10.0.0.0/8"]
    if not any(network in auth for auth in authorized_networks):
        raise ValueError("Rede não autorizada para scanning")
    
    # Limita taxa de envio
    conf.verb = 0  # Silencioso
    max_packets_per_second = 100
    
    # Executa scan com limites
    # ... código do scan ...
```

### Configurações Seguras

```python
# Configura Scapy para operação segura
conf.safe_mode = True  # Evita automaticamente pacotes perigosos
conf.debug_dissector = False  # Desabilita debug
conf.color_theme = DefaultTheme()  # Tema padrão

# Limita recursos
import resource
resource.setrlimit(resource.RLIMIT_CPU, (1, 1))  # Limita tempo CPU
```

---

## 🚀 PERFORMANCE E OTIMIZAÇÃO

### Técnicas para Alto Desempenho

```python
# 1. Use sendpfast para envio rápido
sendpfast(packet, pps=100000, loop=1000)

# 2. Use threads para processamento paralelo
from threading import Thread

def process_packets(packets):
    for pkt in packets:
        # Processamento leve
        pass

# 3. Use generators para grandes volumes
def packet_generator(pcap_file):
    with PcapReader(pcap_file) as reader:
        for pkt in reader:
            yield pkt

# 4. Otimize filtros BPF
# RUIM: "tcp and port 80 or port 443"
# BOM: "tcp port 80 or tcp port 443"

# 5. Cache resultados de DNS
from scapy.all import cache_resolvers
cache_resolvers.CACHE = ARPCache()
```

### Exemplo de Sniffer de Alta Performance

```python
from scapy.all import *
from collections import deque
import threading

class HighPerformanceSniffer:
    def __init__(self, iface=None, buffer_size=10000):
        self.iface = iface
        self.buffer = deque(maxlen=buffer_size)
        self.running = False
        self.sniffer = None
        
    def packet_handler(self, packet):
        """Handler minimalista para performance"""
        self.buffer.append(packet)
        
    def start(self):
        """Inicia sniffer em thread separada"""
        self.running = True
        self.sniffer = AsyncSniffer(
            iface=self.iface,
            prn=self.packet_handler,
            store=False  # Não armazena em memória duplicada
        )
        self.sniffer.start()
        
    def stop(self):
        """Para o sniffer"""
        if self.sniffer:
            self.running = False
            packets = self.sniffer.stop()
            return list(self.buffer) + packets
        return list(self.buffer)
    
    def get_stats(self):
        """Retorna estatísticas"""
        return {
            'buffer_size': len(self.buffer),
            'running': self.running
        }

# Uso
sniffer = HighPerformanceSniffer(iface="eth0")
sniffer.start()

# Processamento em background
while True:
    time.sleep(5)
    print(f"Pacotes capturados: {len(sniffer.buffer)}")
```

---

## 📚 RECURSOS E REFERÊNCIAS

### Documentação Oficial
- [Scapy Documentation](https://scapy.readthedocs.io/)
- [Scapy GitHub](https://github.com/secdev/scapy)
- [Scapy Cheat Sheet](https://github.com/secdev/scapy/blob/master/doc/cheatsheet.rst)

### Livros Recomendados
- "Violent Python" by TJ O'Connor
- "Black Hat Python" by Justin Seitz
- "Mastering Python for Networking and Security" by José Ortega

### Comunidade
- [Scapy Mailing List](https://mail.python.org/mailman3/lists/scapy.mail.python.org/)
- [Stack Overflow - Scapy Tag](https://stackoverflow.com/questions/tagged/scapy)
- [Reddit - r/netsec](https://www.reddit.com/r/netsec/)

### Ferramentas Relacionadas
- Wireshark (visualização complementar)
- tcpdump (captura via CLI)
- nmap (scanner de rede)
- Netcat (rede básica)
- hping (geração de pacotes)

---

## 🔍 TROUBLESHOOTING

### Problemas Comuns e Soluções

```python
# Problema: Permissões insuficientes
# Solução: Execute como root ou use capacidades
# sudo python script.py
# OU
# setcap cap_net_raw=eip /usr/bin/python3

# Problema: Interface não encontrada
# Solução: Liste interfaces disponíveis
print(conf.ifaces)

# Problema: Pacotes não sendo capturados
# Solução: Verifique filtros e modo promíscuo
conf.sniff_promisc = 1

# Problema: Performance lenta
# Solução: Use libpcap se disponível
conf.use_pcap = True

# Problema: DNS não resolvendo
# Solução: Configure manualmente
conf.nameserver = ["8.8.8.8", "8.8.4.4"]
```

### Debugging

```python
# Ativa debug detalhado
conf.debug_dissector = 2

# Log para arquivo
import logging
logging.basicConfig(level=logging.DEBUG, 
                   filename='scapy.log',
                   filemode='w')

# Verifique cada camada
def debug_packet(packet):
    print(f"Layers: {packet.layers()}")
    for layer in packet.layers():
        print(f"  {layer}: {packet.getlayer(layer).fields}")
```

---


*Manual criado com base no Scapy 2.5.0. Para versões mais recentes, consulte a documentação oficial.*
