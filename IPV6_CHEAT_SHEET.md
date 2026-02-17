# **Cheat Sheet Completo de IPv6**

## Índice Rápido
1. [Estrutura do Endereço](#estrutura-do-endereço)
2. [Tipos de Endereços](#tipos-de-endereços)
3. [Notação e Abreviação](#notação-e-abreviação)
4. [Prefixos Comuns](#prefixos-comuns)
5. [Endereços Especiais](#endereços-especiais)
6. [Cabeçalho IPv6](#cabeçalho-ipv6)
7. [ICMPv6](#icmpv6)
8. [Comandos de Configuração](#comandos-de-configuração)
9. [Comandos de Diagnóstico](#comandos-de-diagnóstico)
10. [Transição e Túneis](#transição-e-túneis)
11. [Tabela de Sub-redes](#tabela-de-sub-redes)
12. [DNS e IPv6](#dns-e-ipv6)
13. [Segurança](#segurança)

---

## Estrutura do Endereço

### Formato Básico
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
├─────┴─────┴─────┴─────┴─────┴─────┴─────┤
│        128 bits (16 bytes)                │
└───────────────────────────────────────────┘
```

### Divisão em 8 Blocos de 16 bits
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
[0]    [1]   [2]   [3]   [4]   [5]   [6]   [7]   ← Posição
```

### Estrutura Hierárquica
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334/64
├───┬───┴───┬───┴───┬───┴───┬───┴────────────┤
│Prefixo Global (48 bits)  │ Sub-rede (16) │ Interface ID (64 bits)│
│   2001:0db8:85a3         │     0000       │    0000:8a2e:0370:7334│
│   (Fornecido pelo ISP)   │   (Site)       │    (EUI-64 ou aleatório)│
└──────────────────────────┴────────────────┴─────────────────────────┘
```

---

## Tipos de Endereços

### 1. **Unicast** (Comunicação 1 para 1)
| Tipo | Prefixo | Faixa | Descrição |
|------|---------|-------|-----------|
| **Global Unicast** | `2000::/3` | `2000::` a `3FFF:FFFF:...` | Endereços públicos roteáveis na Internet |
| **Link-Local** | `fe80::/10` | `fe80::` a `febf:...` | Comunicação no mesmo link (não roteável) |
| **Unique Local** | `fc00::/7` | `fc00::` a `fdff:...` | Equivalente ao IPv4 privado (RFC 4193) |
| **Loopback** | `::1/128` | `::1` | Equivalente ao 127.0.0.1 |
| **Endereços IPv4-mapped** | `::ffff:0:0/96` | `::ffff:192.0.2.1` | Representação IPv6 de IPv4 |
| **Embedded IPv4** | `64:ff9b::/96` | `64:ff9b::192.0.2.1` | Tradução IPv4-IPv6 |

### 2. **Multicast** (Comunicação 1 para N)
| Prefixo | Descrição |
|---------|-----------|
| `ff00::/8` | Todos os endereços multicast |

### 3. **Anycast** (Comunicação 1 para o Mais Próximo)
- Mesmo prefixo que Unicast
- Identificado pela configuração (não pelo prefixo)

---

## Notação e Abreviação

### Regras de Abreviação
1. **Remover zeros à esquerda** em cada bloco
2. **Um único "::"** substitui a maior sequência contínua de blocos zero

### Exemplos Práticos
| Endereço Completo | Abreviado |
|-------------------|-----------|
| `2001:0db8:0000:0000:0000:0000:1428:57ab` | `2001:db8::1428:57ab` |
| `2001:0db8:0000:0000:0000:ff00:0042:8329` | `2001:db8::ff00:42:8329` |
| `0000:0000:0000:0000:0000:0000:0000:0001` | `::1` |
| `0000:0000:0000:0000:0000:0000:0000:0000` | `::` |
| `2001:0db8:85a3:0000:1319:8a2e:0370:7344` | `2001:db8:85a3::1319:8a2e:370:7344` |

### Cuidados com "::"
- **ERRADO:** `2001:db8::85a3::8a2e:370:7334` (dois "::")
- **CORRETO:** `2001:db8:0:0:85a3::8a2e:370:7334`

---

## Prefixos Comuns

| Prefixo | CIDR | Descrição |
|---------|------|-----------|
| `2000::` | `/3` | Global Unicast (2000:: a 3FFF:FFFF:...) |
| `fc00::` | `/7` | Unique Local Address (ULA) |
| `fe80::` | `/10` | Link-Local |
| `ff00::` | `/8` | Multicast |
| `::` | `/128` | Endereço não especificado |
| `::1` | `/128` | Loopback |
| `2001:db8::` | `/32` | Documentação (exemplos) |
| `2002::` | `/16` | 6to4 |
| `2001::` | `/16` | Teredo |
| `64:ff9b::` | `/96` | NAT64/DNS64 |

---

## Endereços Especiais

### Endereços Reservados
| Endereço | Descrição |
|----------|-----------|
| `::` | Endereço não especificado (origem) |
| `::1` | Loopback |
| `::ffff:0:0/96` | IPv4-mapped |
| `::ffff:0:0:0/96` | IPv4-compatible (obsoleto) |
| `2001:db8::/32` | Documentação |
| `2001:10::/28` | ORCHID (obsoleto) |
| `2002::/16` | 6to4 |
| `fc00::/7` | Unique Local |
| `fe80::/10` | Link-Local |
| `ff00::/8` | Multicast |

### Endereços Multicast Conhecidos
| Endereço | Escopo | Descrição |
|----------|--------|-----------|
| `ff02::1` | Link-local | Todos os nós |
| `ff02::2` | Link-local | Todos os roteadores |
| `ff02::5` | Link-local | Todos os roteadores OSPF |
| `ff02::6` | Link-local | Todos os roteadores designados OSPF |
| `ff02::9` | Link-local | Todos os roteadores RIP |
| `ff02::a` | Link-local | Todos os roteadores EIGRP |
| `ff02::d` | Link-local | Todos os roteadores PIM |
| `ff02::16` | Link-local | Todos os roteadores MLDv2 |
| `ff02::1:2` | Link-local | Todos os agentes DHCP |
| `ff02::1:3` | Link-local | Todos os servidores LLMNR |
| `ff05::2` | Site-local | Todos os roteadores (site) |
| `ff05::1:3` | Site-local | Todos os servidores DHCP (site) |
| `ff0x::fb` | Variável | mDNSv6 (x = escopo) |
| `ff0x::101` | Variável | NTP (x = escopo) |
| `ff0x::114` | Variável | Session Initiation Protocol |

### Escopos Multicast
| Valor | Escopo |
|-------|--------|
| `1` | Interface-local |
| `2` | Link-local |
| `4` | Admin-local |
| `5` | Site-local |
| `8` | Organization-local |
| `E` | Global |

---

## Cabeçalho IPv6

### Cabeçalho Fixo (40 bytes)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |           Flow Label                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                         Source Address                        +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                      Destination Address                      +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Campos do Cabeçalho
| Campo | Tamanho | Descrição |
|-------|---------|-----------|
| Version | 4 bits | Sempre 6 (0110) |
| Traffic Class | 8 bits | QoS/DSCP |
| Flow Label | 20 bits | Identificação de fluxo |
| Payload Length | 16 bits | Tamanho dos dados (sem cabeçalho) |
| Next Header | 8 bits | Tipo do próximo cabeçalho |
| Hop Limit | 8 bits | Equivalente ao TTL (IPv4) |
| Source Address | 128 bits | Endereço de origem |
| Destination Address | 128 bits | Endereço de destino |

### Cadeia de Cabeçalhos de Extensão
```
[Cabeçalho IPv6] → [Cabeçalho de Extensão 1] → ... → [Cabeçalho de Extensão N] → [Dados (TCP/UDP/ICMPv6)]
```

### Tipos de Cabeçalhos de Extensão (Next Header)
| Valor | Protocolo |
|-------|-----------|
| 0 | Hop-by-Hop Options |
| 6 | TCP |
| 17 | UDP |
| 43 | Routing |
| 44 | Fragment |
| 50 | ESP (Encapsulating Security Payload) |
| 51 | AH (Authentication Header) |
| 58 | ICMPv6 |
| 59 | No Next Header |
| 60 | Destination Options |

---

## ICMPv6

### Tipos de Mensagem ICMPv6
| Tipo | Nome | Descrição |
|------|------|-----------|
| **1** | Destination Unreachable | Destino inalcançável |
| **2** | Packet Too Big | Pacote muito grande (Path MTU) |
| **3** | Time Exceeded | Tempo excedido |
| **4** | Parameter Problem | Problema no parâmetro |
| **128** | Echo Request | Ping request |
| **129** | Echo Reply | Ping reply |
| **133** | Router Solicitation | Solicitação de roteador |
| **134** | Router Advertisement | Anúncio de roteador |
| **135** | Neighbor Solicitation | Solicitação de vizinho |
| **136** | Neighbor Advertisement | Anúncio de vizinho |
| **137** | Redirect | Redirecionamento |

### Códigos Destination Unreachable (Tipo 1)
| Código | Descrição |
|--------|-----------|
| 0 | No route to destination |
| 1 | Communication with destination administratively prohibited |
| 2 | Beyond scope of source address |
| 3 | Address unreachable |
| 4 | Port unreachable |
| 5 | Source address failed ingress/egress policy |
| 6 | Reject route to destination |

### NDP (Neighbor Discovery Protocol)
- Substitui o ARP do IPv4
- Usa ICMPv6 tipos 133-137
- Opera em multicast
- Inclui SLAAC (Stateless Address Autoconfiguration)

---

## Comandos de Configuração

### Linux (iproute2)
```bash
# Mostrar endereços IPv6
ip -6 addr show
ip -6 addr show dev eth0

# Adicionar endereço
ip -6 addr add 2001:db8:1::2/64 dev eth0

# Remover endereço
ip -6 addr del 2001:db8:1::2/64 dev eth0

# Mostrar rotas
ip -6 route show
ip -6 route show dev eth0

# Adicionar rota padrão
ip -6 route add default via 2001:db8:1::1

# Adicionar rota específica
ip -6 route add 2001:db8:2::/64 via 2001:db8:1::1

# Habilitar/desabilitar IPv6 em interface
sysctl net.ipv6.conf.eth0.disable_ipv6=1  # Desabilitar
sysctl net.ipv6.conf.eth0.disable_ipv6=0  # Habilitar

# Configurar sysctl permanente
echo "net.ipv6.conf.all.disable_ipv6 = 0" >> /etc/sysctl.conf
echo "net.ipv6.conf.default.disable_ipv6 = 0" >> /etc/sysctl.conf
```

### Windows (netsh)
```batch
: Mostrar configurações
netsh interface ipv6 show addresses
netsh interface ipv6 show routes
netsh interface ipv6 show neighbors
netsh interface ipv6 show interfaces

: Adicionar endereço
netsh interface ipv6 add address "Ethernet" 2001:db8:1::2/64

: Remover endereço
netsh interface ipv6 delete address "Ethernet" 2001:db8:1::2

: Adicionar rota
netsh interface ipv6 add route 2001:db8:2::/64 "Ethernet" 2001:db8:1::1

: Configurar DNS
netsh interface ipv6 add dnsserver "Ethernet" 2001:4860:4860::8888
```

### Cisco IOS
```cisco
! Habilitar IPv6 globalmente
ipv6 unicast-routing

! Configurar endereço em interface
interface GigabitEthernet0/0
 ipv6 address 2001:db8:1::1/64
 ipv6 enable

! Configurar endereço link-local automático
interface GigabitEthernet0/0
 ipv6 enable

! Configurar endereço link-local manual
interface GigabitEthernet0/0
 ipv6 address fe80::1 link-local

! Configurar EUI-64
interface GigabitEthernet0/0
 ipv6 address 2001:db8:1::/64 eui-64

! Rota padrão
ipv6 route ::/0 2001:db8:1::2

! Rota estática
ipv6 route 2001:db8:2::/64 2001:db8:1::2

! OSPFv3
router ospfv3 1
 router-id 1.1.1.1
 
interface GigabitEthernet0/0
 ospfv3 1 ipv6 area 0
```

### Juniper JunOS
```junos
set interfaces ge-0/0/0 unit 0 family inet6 address 2001:db8:1::1/64
set routing-options rib inet6.0 static route ::/0 next-hop 2001:db8:1::2
set protocols ospf3 area 0 interface ge-0/0/0
```

---

## Comandos de Diagnóstico

### Linux
```bash
# Ping
ping6 2001:db8:1::1
ping6 -I eth0 2001:db8:1::1
ping6 ff02::1%eth0  # Ping all nodes on link

# Traceroute
traceroute6 2001:db8:1::1
tracepath6 2001:db8:1::1
mtr -6 2001:db8:1::1

# DNS
host -t AAAA google.com
dig AAAA google.com
nslookup -type=AAAA google.com

# Conexões
ss -6
ss -6 -tuln
netstat -6
netstat -6 -rn

# Tabela vizinhança (NDP)
ip -6 neigh show
ip -6 neigh show dev eth0

# Captura de pacotes
tcpdump -i eth0 ip6
tcpdump -i eth0 icmp6
tcpdump -i eth0 -vvv -s0 'ip6 and host 2001:db8:1::1'

# MTU Path Discovery
tracepath6 -n 2001:db8:1::1
```

### Windows
```batch
: Ping
ping -6 2001:db8:1::1
ping -6 -S 2001:db8:1::2 2001:db8:1::1

: Traceroute
tracert -6 2001:db8:1::1
pathping -6 2001:db8:1::1

: DNS
nslookup -type=AAAA google.com
nslookup -type=AAAA google.com 2001:4860:4860::8888

: Conexões
netstat -6
netstat -6 -rn

: Tabela vizinhança
netsh interface ipv6 show neighbors

: Captura (PowerShell)
Get-NetAdapter
New-NetEventSession -Name "Capture"
Add-NetEventPacketCaptureProvider -SessionName "Capture" -CaptureType Physical
```

### Cisco IOS
```cisco
! Ping
ping 2001:db8:1::1
ping ipv6 2001:db8:1::1

! Traceroute
traceroute 2001:db8:1::1
traceroute ipv6 2001:db8:1::1

! Verificações
show ipv6 interface brief
show ipv6 route
show ipv6 neighbors
show ipv6 traffic
show ipv6 access-list
show ipv6 mtu
show ipv6 protocols

! Debug
debug ipv6 icmp
debug ipv6 nd
debug ipv6 packet
debug ipv6 routing
```

---

## Transição e Túneis

### Mecanismos de Transição
| Método | Descrição | Quando usar |
|--------|-----------|-------------|
| **Dual Stack** | Ambos protocolos simultaneamente | Padrão, sempre que possível |
| **6in4** | Túnel IPv6 em IPv4 manual | Conexão ponto a ponto |
| **6to4** | Túnel automático (2002::/16) | Acesso IPv6 via IPv4 público |
| **Teredo** | Túnel via NAT (2001::/32) | Atrás de NAT sem suporte IPv6 |
| **ISATAP** | Túnel intra-site | Redes internas |
| **DS-Lite** | CGNAT com túnel IPv4 em IPv6 | ISPs para clientes |
| **NAT64/DNS64** | Tradução IPv6-IPv4 | Rede só IPv6 acessando IPv4 |
| **464XLAT** | Cliente NAT64 + SIIT | Dispositivos só IPv6 |

### Configuração 6in4 (Linux)
```bash
# Criar túnel
ip tunnel add sit1 mode sit remote 192.0.2.1 local 192.0.2.2 ttl 255
ip link set sit1 up

# Adicionar endereços
ip -6 addr add 2001:db8:1::2/64 dev sit1

# Rota
ip -6 route add ::/0 dev sit1

# Ou com script (ex: Hurricane Electric)
# /etc/network/interfaces
auto sit1
iface sit1 inet6 v4tunnel
    address 2001:470:1f:2b6::2/64
    netmask 64
    endpoint 216.66.80.30
    local 192.0.2.2
    ttl 255
    gateway 2001:470:1f:2b6::1
```

### Configuração 6to4
```bash
# Requer IPv4 público
ip tunnel add tun6to4 mode sit ttl 255 remote any local $(curl -s ifconfig.me)
ip link set tun6to4 up
ip -6 addr add 2002:$(printf "%02x%02x" $(echo $IP | tr '.' ' '))::1/16 dev tun6to4
ip -6 route add 2000::/3 via ::192.88.99.1 dev tun6to4 metric 1
```

### Configuração Teredo (Linux/miredo)
```bash
# Instalar
apt-get install miredo

# Verificar
ip -6 addr show teredo
ping6 ipv6.google.com
```

---

## Tabela de Sub-redes

### Cálculo de Sub-redes IPv6
| Prefixo | Bits de Sub-rede | Número de Sub-redes | Hosts por Sub-rede |
|---------|------------------|---------------------|---------------------|
| /48 | 16 (para /64) | 65,536 | 2^64 (≈ 1.8×10^19) |
| /52 | 12 (para /64) | 4,096 | 2^64 |
| /56 | 8 (para /64) | 256 | 2^64 |
| /60 | 4 (para /64) | 16 | 2^64 |
| /64 | 0 | 1 | 2^64 |

### Tabela de Máscaras CIDR IPv6
| CIDR | Bits de Host | Número de /64 | Descrição |
|------|--------------|---------------|-----------|
| /32 | 96 | 4,294,967,296 (2^32) | Bloco ISP (LIR) |
| /40 | 88 | 16,777,216 (2^24) | |
| /44 | 84 | 1,048,576 (2^20) | |
| /48 | 80 | 65,536 (2^16) | Bloco site típico |
| /52 | 76 | 4,096 (2^12) | |
| /56 | 72 | 256 (2^8) | Bloco residencial típico |
| /60 | 68 | 16 (2^4) | |
| /64 | 64 | 1 | Sub-rede padrão |
| /80 | 48 | - | Não recomendado (quebra SLAAC) |
| /96 | 32 | - | Usado em IPv4-mapped |
| /112 | 16 | - | Ponto a ponto |
| /120 | 8 | - | Ponto a ponto |
| /128 | 0 | - | Endereço único |

### Recomendações RIR
| RIR | Alocação mínima | Atribuição típica |
|-----|-----------------|-------------------|
| **ARIN** | /32 para LIR | /48 para site |
| **RIPE NCC** | /32 para LIR | /48 para site |
| **APNIC** | /32 para LIR | /56 para residencial |
| **LACNIC** | /32 para LIR | /48 para site |
| **AFRINIC** | /32 para LIR | /48 para site |

---

## DNS e IPv6

### Registros DNS
| Tipo | Descrição | Exemplo |
|------|-----------|---------|
| **AAAA** | Mapeia nome → endereço IPv6 | `google.com. IN AAAA 2607:f8b0:4005:804::200e` |
| **PTR** | Mapeia IPv6 → nome (ip6.arpa) | `e.0.0.2.0.0.0.0.0.0.0.0.0.0.0.0...ip6.arpa` |
| **A6** | Obsoleto (RFC 2874) | |

### Formato PTR ip6.arpa
```
Endereço: 2001:db8:1234:5678:90ab:cdef:1234:5678
Ordem reversa: 8.7.6.5.4.3.2.1.f.e.d.c.b.a.0.9.8.7.6.5.4.3.2.1.8.b.d.0.1.0.0.2.ip6.arpa
```

### Servidores DNS IPv6 Públicos
| Provedor | Endereço IPv6 Primário | Endereço IPv6 Secundário |
|----------|------------------------|---------------------------|
| **Google** | 2001:4860:4860::8888 | 2001:4860:4860::8844 |
| **Cloudflare** | 2606:4700:4700::1111 | 2606:4700:4700::1001 |
| **Quad9** | 2620:fe::fe | 2620:fe::9 |
| **OpenDNS** | 2620:119:35::35 | 2620:119:53::53 |
| **Verisign** | 2620:74:1b::1:1 | 2620:74:1c::2:2 |
| **Comodo** | 2001:558:feed::1 | 2001:558:feed::2 |
| **Cisco** | 2001:420:1101:1::a | 2001:420:1101:2::a |

### Configuração DNS
```bash
# Linux (/etc/resolv.conf)
nameserver 2001:4860:4860::8888
nameserver 2001:4860:4860::8844

# NetworkManager
nmcli con mod "Wired" ipv6.dns "2001:4860:4860::8888 2001:4860:4860::8844"

# systemd-resolved
resolvectl dns eth0 2001:4860:4860::8888 2001:4860:4860::8844

# Windows (PowerShell)
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "2001:4860:4860::8888","2001:4860:4860::8844"
```

### Testes DNS
```bash
# Verificar resolução AAAA
dig AAAA google.com
host -t AAAA google.com
nslookup -type=AAAA google.com

# Verificar reverso
dig -x 2001:4860:4860::8888
host 2001:4860:4860::8888

# Consultar servidor específico
dig @2001:4860:4860::8888 AAAA google.com
```

---

## Segurança

### Firewall (iptables/nftables)

#### iptables (IPv6)
```bash
# Regras básicas
ip6tables -P INPUT DROP
ip6tables -P FORWARD DROP
ip6tables -P OUTPUT ACCEPT

# Permitir conexões estabelecidas
ip6tables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Permitir ICMPv6 (necessário para NDP)
ip6tables -A INPUT -p icmpv6 -j ACCEPT

# Permitir multicast (link-local)
ip6tables -A INPUT -d ff00::/8 -j ACCEPT

# Permitir SSH
ip6tables -A INPUT -p tcp --dport 22 -j ACCEPT

# Log e drop
ip6tables -A INPUT -j LOG --log-prefix "IPv6-DROP: "
ip6tables -A INPUT -j DROP
```

#### nftables
```bash
table inet filter {
    chain input {
        type filter hook input priority 0; policy drop;
        
        # Conexões estabelecidas
        ct state established,related accept
        
        # ICMPv6 (RA, RS, NS, NA, etc.)
        icmpv6 type { nd-router-solicit, nd-router-advert, nd-neighbor-solicit, nd-neighbor-advert } accept
        icmpv6 type { echo-request, echo-reply, destination-unreachable, packet-too-big, time-exceeded, parameter-problem } accept
        
        # Permitir loopback
        iif lo accept
        
        # SSH
        tcp dport 22 accept
        
        # Log
        log prefix "nftables-IPv6: "
    }
    
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
    
    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```

### RA-Guard (Router Advertisement Guard)
```cisco
! Cisco
interface GigabitEthernet0/0
 ipv6 nd raguard attach-policy RA-GUARD-POLICY

! Juniper
protocols router-advertisement {
    interface ge-0/0/0.0 {
        ra-interval 5;
        ra-lifetime 30;
    }
}
```

### DHCPv6 Guard
```cisco
! Cisco
ipv6 dhcp guard policy DHCP-GUARD
 device-role client
 match server access-list DHCP-SERVERS
!
interface GigabitEthernet0/0
 ipv6 dhcp guard attach-policy DHCP-GUARD
```

### Source Guard
```cisco
! Cisco
interface GigabitEthernet0/0
 ipv6 source-guard attach-policy SOURCE-GUARD-POLICY
```

### SAVI (Source Address Validation Improvement)
```cisco
! Cisco
ipv6 snooping policy SNOOPING-POLICY
 device-role node
 limit address-count 4
 trusted-port
!
interface GigabitEthernet0/0
 ipv6 snooping attach-policy SNOOPING-POLICY
```

### Privacidade (RFC 4941)
```bash
# Linux - Habilitar endereços temporários
sysctl net.ipv6.conf.all.use_tempaddr=2
sysctl net.ipv6.conf.default.use_tempaddr=2
sysctl net.ipv6.conf.eth0.use_tempaddr=2

# Windows
netsh interface ipv6 set privacy state=enabled

# macOS
sysctl net.inet6.ip6.use_tempaddr=1
```

### Melhores Práticas de Segurança
1. **Filtrar tráfego ICMPv6** - Não bloquear completamente (quebra NDP)
2. **Usar RA-Guard** - Prevenir rogue router advertisements
3. **Implementar DHCPv6 Guard** - Prevenir rogue DHCP servers
4. **Usar ACLs** para tráfego não autorizado
5. **Monitorar NDP** para detecção de spoofing
6. **Usar endereços temporários** para privacidade
7. **Implementar SAVI** onde possível
8. **Firewall stateful** para proteção
9. **Desabilitar IPv6 em interfaces não utilizadas**
10. **Usar IPsec** para tráfego sensível

---

## Glossário Rápido

| Termo | Significado |
|-------|-------------|
| **SLAAC** | Stateless Address Autoconfiguration |
| **DHCPv6** | Dynamic Host Configuration Protocol for IPv6 |
| **NDP** | Neighbor Discovery Protocol |
| **ICMPv6** | Internet Control Message Protocol for IPv6 |
| **ULA** | Unique Local Address |
| **EUI-64** | Extended Unique Identifier (64-bit) |
| **DAD** | Duplicate Address Detection |
| **MLD** | Multicast Listener Discovery |
| **PMTU** | Path MTU Discovery |
| **RA** | Router Advertisement |
| **RS** | Router Solicitation |
| **NS** | Neighbor Solicitation |
| **NA** | Neighbor Advertisement |
| **6PE** | IPv6 Provider Edge (MPLS) |
| **6VPE** | IPv6 VPN Provider Edge |
| **CGA** | Cryptographically Generated Address |
| **HBA** | Hash-Based Address |

---
