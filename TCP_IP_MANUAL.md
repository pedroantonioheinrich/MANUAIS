# Manual Completo sobre TCP/IP

## Índice
1. [Introdução ao TCP/IP](#introdução)
2. [História e Evolução](#história)
3. [Arquitetura em Camadas](#arquitetura)
4. [Camada de Acesso à Rede](#camada-rede)
5. [Camada de Internet](#camada-internet)
6. [Camada de Transporte](#camada-transporte)
7. [Camada de Aplicação](#camada-aplicacao)
8. [Endereçamento IP](#enderecamento)
9. [Protocolos Principais](#protocolos)
10. [Roteamento](#roteamento)
11. [Segurança](#seguranca)
12. [IPv6](#ipv6)
13. [Diagnóstico e Ferramentas](#diagnostico)
14. [Glossário](#glossario)

---

## 1. Introdução ao TCP/IP {#introducao}

O **TCP/IP** (Transmission Control Protocol/Internet Protocol) é o conjunto fundamental de protocolos de comunicação que sustenta a Internet e a maioria das redes modernas. Desenvolvido para permitir a interconexão de sistemas heterogêneos, o TCP/IP fornece uma estrutura padronizada para transmissão de dados entre dispositivos, independentemente de sua arquitetura ou localização geográfica.

**Características fundamentais:**
- Arquitetura aberta e não proprietária
- Independente de hardware específico
- Escalabilidade global
- Roteamento dinâmico
- Tolerância a falhas
- Suporte a diferentes meios físicos

---

## 2. História e Evolução {#história}

**1960s:** ARPA (Advanced Research Projects Agency) inicia pesquisas em redes de comutação de pacotes.

**1969:** Nascimento da ARPANET, precursora da Internet, conectando 4 universidades americanas.

**1974:** Vint Cerf e Bob Kahn publicam "A Protocol for Packet Network Interconnection", introduzindo o TCP.

**1978:** Divisão do TCP em TCP e IP, criando a arquitetura de protocolos separados.

**1983:** ARPANET adota oficialmente o TCP/IP, substituindo o NCP (Network Control Program).

**1985:** Criação do DNS (Domain Name System).

**1990s:** Expansão comercial da Internet e padronização do TCP/IP como protocolo universal.

**2012:** Lançamento mundial do IPv6 para expandir o espaço de endereçamento.

---

## 3. Arquitetura em Camadas {#arquitetura}

O modelo TCP/IP utiliza uma abordagem em camadas, cada uma responsável por funções específicas na comunicação de dados.

### Comparativo de Modelos:

| Camada OSI | Camada TCP/IP | Função Principal |
|------------|---------------|------------------|
| Aplicação | Aplicação | Interface com usuário/aplicações |
| Apresentação | ↑ | Formatação, criptografia |
| Sessão | ↑ | Gerenciamento de sessões |
| Transporte | Transporte | Comunicação fim-a-fim, confiabilidade |
| Rede | Internet | Roteamento, endereçamento lógico |
| Enlace | Acesso à Rede | Endereçamento físico, acesso ao meio |
| Física | ↓ | Transmissão binária |

**Princípio de encapsulamento:** Cada camada adiciona seu próprio cabeçalho aos dados recebidos da camada superior, criando uma estrutura hierárquica.

---

## 4. Camada de Acesso à Rede {#camada-rede}

Também chamada de **camada de enlace** ou **interface de rede**, esta é a camada mais baixa do modelo TCP/IP.

**Funções:**
- Interface direta com o hardware de rede
- Endereçamento físico (MAC)
- Detecção e correção de erros no nível do enlace
- Controle de acesso ao meio
- Sincronização de quadros

**Protocolos e tecnologias:**
- **Ethernet** - Padrão dominante para redes locais
- **Wi-Fi (IEEE 802.11)** - Redes sem fio
- **PPP** - Point-to-Point Protocol
- **ARP** - Address Resolution Protocol (mapeia IP → MAC)
- **MAC** - Media Access Control (endereço físico de 48 bits)

**Exemplo de encapsulamento:**
```
[ MAC Header | IP Header | TCP Header | Dados | FCS ]
```

---

## 5. Camada de Internet {#camada-internet}

Corresponde à camada de Rede do modelo OSI. É responsável pelo endereçamento lógico e roteamento dos pacotes.

**Funções:**
- Endereçamento IP
- Fragmentação e remontagem de pacotes
- Roteamento entre redes diferentes
- Determinação da melhor rota

**Protocolos principais:**

### IP (Internet Protocol)
- Protocolo não confiável e não orientado à conexão
- Entrega "melhor esforço" (best effort)
- Cabeçalho com campos essenciais para roteamento

**Formato do cabeçalho IPv4:**
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Outros protocolos importantes:
- **ICMP** - Internet Control Message Protocol (diagnóstico e erros)
- **ARP** - Address Resolution Protocol (IPv4 → MAC)
- **RARP** - Reverse ARP (MAC → IPv4)
- **IGMP** - Internet Group Management Protocol (multicast)

---

## 6. Camada de Transporte {#camada-transporte}

Garante a comunicação fim-a-fim entre processos em diferentes hosts.

### TCP (Transmission Control Protocol)

**Características:**
- Orientado à conexão (3-way handshake)
- Confiável (acknowledgments, retransmissão)
- Controle de fluxo e congestionamento
- Segmentos ordenados
- Detecção de erros

**Formato do cabeçalho TCP:**
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Sequence Number                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                 Acknowledgment Number                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Flags TCP:**
- **URG** - Urgente
- **ACK** - Acknowledgement
- **PSH** - Push
- **RST** - Reset
- **SYN** - Synchronize
- **FIN** - Finish

**Three-way handshake:**
1. Cliente → Servidor: SYN (seq=x)
2. Servidor → Cliente: SYN-ACK (seq=y, ack=x+1)
3. Cliente → Servidor: ACK (seq=x+1, ack=y+1)

### UDP (User Datagram Protocol)

**Características:**
- Não orientado à conexão
- Não confiável (sem garantia de entrega)
- Baixa latência
- Sem controle de fluxo
- Cabeçalho simplificado (8 bytes)

**Formato do cabeçalho UDP:**
```
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Source Port              |     Destination Port           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Length                   |     Checksum                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

**Portas bem conhecidas:**
| Porta | Protocolo | Aplicação |
|-------|-----------|-----------|
| 20/21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |

---

## 7. Camada de Aplicação {#camada-aplicacao}

Camada superior que interage diretamente com os softwares aplicativos.

### Protocolos principais:

**HTTP/HTTPS (Hypertext Transfer Protocol)**
- Protocolo da World Wide Web
- Métodos: GET, POST, PUT, DELETE, HEAD, OPTIONS
- HTTPS: versão criptografada com TLS/SSL

**FTP (File Transfer Protocol)**
- Transferência de arquivos
- Modos ativo e passivo
- Comandos: GET, PUT, LIST, etc.

**SMTP (Simple Mail Transfer Protocol)**
- Envio de e-mails
- Porta 25 (tradicional), 587 (submissão)

**POP3/IMAP**
- Recebimento de e-mails
- POP3: download e exclusão
- IMAP: sincronização, múltiplos dispositivos

**DNS (Domain Name System)**
- Resolução de nomes → endereços IP
- Registros: A, AAAA, MX, CNAME, NS, TXT

**DHCP (Dynamic Host Configuration Protocol)**
- Atribuição automática de configurações IP
- Processo DORA: Discover, Offer, Request, Acknowledge

**SSH (Secure Shell)**
- Acesso remoto seguro
- Substitui Telnet

**TELNET**
- Acesso remoto não criptografado (obsoleto)

**SNMP (Simple Network Management Protocol)**
- Gerenciamento e monitoramento de dispositivos de rede

---

## 8. Endereçamento IP {#enderecamento}

### IPv4

**Estrutura:**
- 32 bits (4 octetos)
- Representação decimal pontuada: 192.168.1.1
- Máximo teórico: ~4,3 bilhões de endereços

**Classes tradicionais:**
| Classe | Prefixo | Primeiro octeto | Máscara padrão | Uso |
|--------|---------|-----------------|----------------|-----|
| A | 0 | 1-126 | 255.0.0.0 | Grandes redes |
| B | 10 | 128-191 | 255.255.0.0 | Médias redes |
| C | 110 | 192-223 | 255.255.255.0 | Pequenas redes |
| D | 1110 | 224-239 | - | Multicast |
| E | 1111 | 240-255 | - | Experimental |

**Endereços especiais:**
- **127.0.0.0/8** - Loopback (localhost)
- **0.0.0.0/8** - Rede atual
- **255.255.255.255** - Broadcast limitado
- **169.254.0.0/16** - APIPA (Automatic Private IP Addressing)

**Endereços privados (RFC 1918):**
- 10.0.0.0/8 (10.0.0.0 - 10.255.255.255)
- 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
- 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)

### CIDR (Classless Inter-Domain Routing)

Substituiu o sistema de classes, permitindo máscaras de sub-rede de tamanho variável.

**Notação CIDR:** IP/prefixo (ex: 192.168.1.0/24)

**Cálculo de sub-rede:**
- Número de endereços = 2^(32 - prefixo)
- Endereços utilizáveis = 2^(32 - prefixo) - 2
- Máscara = bits 1 do prefixo + bits 0 restantes

**Exemplo /24:**
- Máscara: 255.255.255.0
- 256 endereços (254 utilizáveis)
- Rede: primeiros 24 bits fixos
- Host: últimos 8 bits variáveis

**Máscaras comuns:**
| CIDR | Máscara | Hosts | Uso |
|------|---------|-------|-----|
| /30 | 255.255.255.252 | 2 | Link point-to-point |
| /29 | 255.255.255.248 | 6 | Pequenas redes |
| /28 | 255.255.255.240 | 14 | Pequenas redes |
| /27 | 255.255.255.224 | 30 | Pequenas redes |
| /26 | 255.255.255.192 | 62 | Pequenas/medias |
| /25 | 255.255.255.128 | 126 | Médias redes |
| /24 | 255.255.255.0 | 254 | Rede padrão |
| /23 | 255.255.254.0 | 510 | Agregação |
| /22 | 255.255.252.0 | 1022 | Agregação |
| /16 | 255.255.0.0 | 65534 | Classe B |

### NAT (Network Address Translation)

Permite que múltiplos dispositivos compartilhem um único endereço IP público.

**Tipos:**
- **SNAT** (Source NAT) - Altera IP de origem
- **DNAT** (Destination NAT) - Altera IP de destino
- **PAT** (Port Address Translation) - Tradução com portas

**Aplicações:**
- Conservação de endereços IPv4
- Segurança (oculta rede interna)
- Balanceamento de carga

---

## 9. Protocolos Principais {#protocolos}

### ICMP (Internet Control Message Protocol)

Utilizado para mensagens de erro e diagnóstico.

**Tipos de mensagem:**
| Tipo | Código | Descrição |
|------|--------|-----------|
| 0 | 0 | Echo Reply |
| 3 | 0 | Destination Network Unreachable |
| 3 | 1 | Destination Host Unreachable |
| 3 | 3 | Port Unreachable |
| 8 | 0 | Echo Request |
| 11 | 0 | Time Exceeded |

**Ferramentas:**
- **ping** - Echo Request/Reply
- **traceroute** - Time Exceeded + TTL

### ARP (Address Resolution Protocol)

Resolve endereços IP para endereços MAC.

**Processo:**
1. Broadcast: "Quem tem IP X.X.X.X?"
2. Resposta unicast: "Eu tenho, MAC YY:YY:YY:YY:YY:YY"
3. Armazenamento em cache ARP

**Comandos:**
```
arp -a        # Visualizar cache ARP
arp -d        # Limpar cache ARP
```

### DNS (Domain Name System)

Sistema hierárquico de nomes de domínio.

**Hierarquia:**
- **Raiz** (.) - Servidores raiz
- **TLD** (.com, .org, .br, etc.)
- **Domínio** (exemplo.com)
- **Subdomínio** (www.exemplo.com)

**Tipos de registro:**
| Registro | Função |
|----------|--------|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Alias (nome canônico) |
| MX | Mail exchanger |
| NS | Name server |
| TXT | Texto arbitrário |
| PTR | Reverse lookup |

**Resolução:**
1. Cliente → Resolvedor
2. Resolvedor → Root server
3. Root → TLD server
4. TLD → Authoritative server
5. Resposta ao cliente

---

## 10. Roteamento {#roteamento}

Processo de encaminhamento de pacotes entre redes diferentes.

### Roteamento Estático

- Rotas configuradas manualmente
- Previsível, seguro
- Não se adapta a mudanças na rede
- Adequado para pequenas redes ou rotas padrão

**Exemplo de rota estática:**
```
ip route 192.168.2.0 255.255.255.0 192.168.1.1
```

### Roteamento Dinâmico

Protocolos que aprendem rotas automaticamente.

**IGP (Interior Gateway Protocols):**
- **RIP** - Distance Vector, métrica = hops, máximo 15
- **OSPF** - Link State, métrica = custo, convergência rápida
- **EIGRP** - Advanced Distance Vector, Cisco

**EGP (Exterior Gateway Protocols):**
- **BGP** - Protocolo principal da Internet, baseado em políticas

**Tabela de roteamento:**
```
Destino         Gateway         Máscara         Interface
0.0.0.0         192.168.1.1     0.0.0.0         eth0     (padrão)
192.168.1.0     0.0.0.0         255.255.255.0   eth0     (diretamente conectada)
10.0.0.0        192.168.1.254   255.0.0.0       eth0     (remota)
```

---

## 11. Segurança {#seguranca}

### Ameaças comuns:

- **Sniffing** - Captura de tráfego
- **Spoofing** - Falsificação de IP
- **DoS/DDoS** - Negação de serviço
- **Man-in-the-Middle** - Interceptação
- **Replay attack** - Reenvio de pacotes

### Mecanismos de segurança:

**Firewall:**
- Filtragem de pacotes
- Stateful inspection
- Proxy

**IPSec:**
- Autenticação (AH)
- Criptografia (ESP)
- Modos: transporte e túnel

**TLS/SSL:**
- Criptografia na camada de aplicação
- Certificados digitais
- HTTPS, SMTPS, etc.

**VPN:**
- Extensão segura da rede privada
- Túneis criptografados
- PPTP, L2TP, OpenVPN, WireGuard

**Boas práticas:**
- Desabilitar serviços desnecessários
- Atualizar sistemas regularmente
- Utilizar firewalls
- Segmentar redes (VLANs)
- Monitorar tráfego
- Implementar ACLs

---

## 12. IPv6 {#ipv6}

Nova versão do protocolo IP para resolver a escassez de endereços.

**Características:**
- 128 bits (3.4×10³⁸ endereços)
- Notação hexadecimal com 8 grupos de 16 bits
- Eliminação do NAT
- Autoconfiguração (SLAAC)
- Segurança integrada (IPSec obrigatório)
- Multicast obrigatório, broadcast eliminado

**Estrutura do endereço:**
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
2001:db8:85a3::8a2e:370:7334  (com compressão)
```

**Tipos de endereço IPv6:**
- **Unicast** - Identifica uma interface única
- **Anycast** - Entrega ao nó mais próximo
- **Multicast** - Entrega a múltiplos nós

**Escopos:**
- **Global** (2000::/3) - Internet
- **Link-local** (fe80::/10) - Segmento local
- **Unique local** (fc00::/7) - Rede privada
- **Multicast** (ff00::/8)

**Transição IPv4→IPv6:**
- **Dual stack** - Ambos protocolos ativos
- **Túnel 6in4** - Encapsulamento IPv6 em IPv4
- **NAT64/DNS64** - Tradução entre versões

---

## 13. Diagnóstico e Ferramentas {#diagnostico}

### Comandos essenciais:

**Windows/Linux/Mac:**
```bash
# Teste de conectividade
ping 8.8.8.8
ping -c 4 google.com

# Rastreamento de rota
tracert google.com    # Windows
traceroute google.com # Linux/Mac

# Visualizar configuração IP
ipconfig /all         # Windows
ifconfig              # Linux (legado)
ip addr show          # Linux (moderno)

# Tabela de roteamento
route print           # Windows
netstat -r            # Windows/Linux/Mac
ip route show         # Linux

# Conexões ativas
netstat -an
netstat -an | findstr :80   # Windows
netstat -an | grep :80      # Linux/Mac

# Resolução DNS
nslookup google.com
dig google.com
host google.com

# Cache ARP
arp -a

# Captura de pacotes
tcpdump -i eth0
tcpdump port 80
wireshark            # Interface gráfica

# Teste de porta
telnet host porta
nc -zv host porta    # Netcat
```

### Ferramentas avançadas:

- **MTR** - Combinação ping + traceroute
- **Nmap** - Scanner de portas e descoberta de rede
- **Netstat** - Estatísticas de rede
- **iptables/nftables** - Firewall Linux
- **ss** - Alternativa moderna ao netstat
- **iperf** - Medição de performance

---

## 14. Glossário {#glossario}

**ACK** - Acknowledgement, confirmação de recebimento

**CIDR** - Classless Inter-Domain Routing, sistema de endereçamento sem classes

**DDoS** - Distributed Denial of Service, ataque distribuído de negação de serviço

**DNS** - Domain Name System, sistema de nomes de domínio

**FCS** - Frame Check Sequence, sequência de verificação de quadro

**FTP** - File Transfer Protocol, protocolo de transferência de arquivos

**HTTP** - Hypertext Transfer Protocol, protocolo de transferência de hipertexto

**ICMP** - Internet Control Message Protocol, protocolo de mensagens de controle

**IGMP** - Internet Group Management Protocol, protocolo de gerenciamento de grupos

**IP** - Internet Protocol, protocolo de internet

**ISP** - Internet Service Provider, provedor de serviços de internet

**LAN** - Local Area Network, rede local

**MAC** - Media Access Control, controle de acesso ao meio

**MTU** - Maximum Transmission Unit, unidade máxima de transmissão

**NAT** - Network Address Translation, tradução de endereços de rede

**OSI** - Open Systems Interconnection, interconexão de sistemas abertos

**OSPF** - Open Shortest Path First, protocolo de roteamento link-state

**PAT** - Port Address Translation, tradução de endereços por porta

**PPP** - Point-to-Point Protocol, protocolo ponto-a-ponto

**QoS** - Quality of Service, qualidade de serviço

**RFC** - Request for Comments, documentos de padronização

**RIP** - Routing Information Protocol, protocolo de informações de roteamento

**SMTP** - Simple Mail Transfer Protocol, protocolo simples de transferência de correio

**SNMP** - Simple Network Management Protocol, protocolo simples de gerenciamento de rede

**SSL/TLS** - Secure Sockets Layer/Transport Layer Security, camada de segurança

**SYN** - Synchronize, sincronizar (flag TCP)

**TCP** - Transmission Control Protocol, protocolo de controle de transmissão

**TTL** - Time to Live, tempo de vida

**UDP** - User Datagram Protocol, protocolo de datagramas de usuário

**VLAN** - Virtual LAN, rede local virtual

**VPN** - Virtual Private Network, rede privada virtual

**WAN** - Wide Area Network, rede de longa distância

---

## Referências

- RFC 791 - Internet Protocol
- RFC 793 - Transmission Control Protocol
- RFC 768 - User Datagram Protocol
- RFC 792 - ICMP
- RFC 1034/1035 - Domain Names
- RFC 1918 - Address Allocation for Private Internets
- RFC 2460 - IPv6 Specification
- RFC 4291 - IPv6 Addressing Architecture

---

*Este manual foi desenvolvido para fornecer uma visão abrangente do TCP/IP, desde conceitos fundamentais até aspectos avançados. O conhecimento destes protocolos é essencial para profissionais de redes, segurança, desenvolvimento e infraestrutura de TI.*
