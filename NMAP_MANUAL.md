# 🎯 MANUAL COMPLETO DO NMAP (NETWORK MAPPER)

## 📋 ÍNDICE
1. [Introdução ao Nmap](#introdução-ao-nmap)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Tipos de Scans](#tipos-de-scans)
4. [Detecção de Hosts](#detecção-de-hosts)
5. [Detecção de Portas e Serviços](#detecção-de-portas-e-serviços)
6. [Detecção de Versões](#detecção-de-versões)
7. [Detecção de OS](#detecção-de-os)
8. [Scripts NSE](#scripts-nse)
9. [Otimização e Performance](#otimização-e-performance)
10. [Saída e Formatação](#saída-e-formatação)
11. [Técnicas Avançadas](#técnicas-avançadas)
12. [Evasion e Stealth](#evasion-e-stealth)
13. [Integração com Python](#integração-com-python)
14. [Exemplos Práticos](#exemplos-práticos)
15. [Referência Rápida](#referência-rápida)

---

## 🎯 INTRODUÇÃO AO NMAP

### O que é Nmap?
Nmap (Network Mapper) é uma ferramenta open-source para exploração de rede e auditoria de segurança. Ele usa pacotes IP brutos para determinar:
- Quais hosts estão disponíveis na rede
- Quais serviços (nomes e versões) estão oferecendo
- Quais sistemas operacionais estão executando
- Que tipos de filtros/packet filters estão em uso

### Características Principais
- **Multi-plataforma**: Linux, Windows, macOS, BSD
- **Flexível**: Mais de 20 tipos diferentes de scans
- **Poderoso**: Sistema de scripts NSE (Nmap Scripting Engine)
- **Extensível**: Suporta IPv6, redes sem fio, etc.
- **Documentado**: Manual completo e comunidade ativa

### Filosofia do Nmap
1. **Liberdade**: Software livre e open-source
2. **Qualidade**: Código robusto e bem testado
3. **Simplicidade**: Interface intuitiva (CLI)
4. **Documentação**: Manual completo e atualizado

---

## 📦 INSTALAÇÃO E CONFIGURAÇÃO

### Instalação Linux
```bash
# Debian/Ubuntu
sudo apt update
sudo apt install nmap

# Fedora/RHEL/CentOS
sudo dnf install nmap
# ou
sudo yum install nmap

# Arch Linux
sudo pacman -S nmap

# Kali Linux (já pré-instalado)
# Verificar versão
nmap --version
```

### Instalação Windows
1. Baixe do site oficial: https://nmap.org/download.html
2. Instale normalmente (.exe)
3. Opcional: Instale Zenmap (GUI)
```bash
# Windows CMD/PowerShell
nmap --version
```

### Instalação macOS
```bash
# Homebrew
brew install nmap

# MacPorts
sudo port install nmap
```

### Atualização
```bash
# Linux (apt)
sudo apt update && sudo apt upgrade nmap

# Compilar da fonte
wget https://nmap.org/dist/nmap-7.94.tar.bz2
tar -xjf nmap-7.94.tar.bz2
cd nmap-7.94
./configure
make
sudo make install
```

### Verificação da Instalação
```bash
# Verificar instalação
nmap --version

# Teste básico
nmap localhost

# Verificar recursos suportados
nmap --iflist
```

---

## 🔍 TIPOS DE SCANS

### Scan TCP SYN (Stealth Scan)
```bash
# SYN scan - meio aberto (stealth)
nmap -sS 192.168.1.1

# Exemplo com múltiplos hosts
nmap -sS 192.168.1.1-100

# Descrição:
# - Envia SYN
# - Se receber SYN-ACK: porta aberta
# - Se receber RST: porta fechada
# - Se timeout: filtrada
```

### Scan TCP Connect (Padrão)
```bash
# Connect scan - usa conexão TCP completa
nmap -sT 192.168.1.1

# Diferenças do SYN:
# - Completa 3-way handshake
# - Mais lento e mais visível
# - Não requer privilégios root
```

### Scan UDP
```bash
# UDP scan
nmap -sU 192.168.1.1

# UDP com portas específicas
nmap -sU -p 53,67,68,69,123,161 192.168.1.1

# Características:
# - Muito lento (pode levar minutos)
# - Resposta ICMP Port Unreachable = porta fechada
# - Sem resposta = aberta|filtrada
```

### Scan NULL, FIN, Xmas
```bash
# NULL scan - sem flags TCP
nmap -sN 192.168.1.1

# FIN scan - apenas flag FIN
nmap -sF 192.168.1.1

# Xmas scan - flags FIN, PSH, URG
nmap -sX 192.168.1.1

# Comportamento:
# - Resposta RST = porta fechada
# - Sem resposta = aberta|filtrada
# - Windows não segue RFC (sempre responde RST)
```

### Scan ACK
```bash
# ACK scan - detecção de firewalls
nmap -sA 192.168.1.1

# Uso:
# - Resposta RST = não filtrada
# - Sem resposta ou ICMP = filtrada
# - Útil para mapear regras de firewall
```

### Scan Window
```bash
# Window scan - similar ao ACK mas analisa window size
nmap -sW 192.168.1.1

# Uso específico para alguns sistemas antigos
```

### Scan Maimon
```bash
# Maimon scan - FIN/ACK
nmap -sM 192.168.1.1

# Nome do descobridor (Uriel Maimon)
```

### Scan Customizado (TCP Flags)
```bash
# Scan com flags personalizadas
nmap --scanflags URGACKPSHRSTSYNFIN 192.168.1.1

# Ou combinação específica
nmap --scanflags SYNFIN 192.168.1.1
```

### Scan Idle (Zombie)
```bash
# Idle scan - usa host zombie
nmap -sI zombie_host:porta target

# Exemplo:
nmap -sI 192.168.1.50:80 192.168.1.100

# Requisitos:
# - Zombie deve ser um host "idle"
# - IPID deve ser incremental
# - Altamente stealth (origem parece ser zombie)
```

### Scan SCTP
```bash
# SCTP INIT scan
nmap -sY 192.168.1.1

# SCTP COOKIE-ECHO scan (mais stealth)
nmap -sZ 192.168.1.1
```

### Scan IP Protocol
```bash
# IP Protocol scan
nmap -sO 192.168.1.1

# Determina quais protocolos IP são suportados
# (TCP, UDP, ICMP, IGMP, etc.)
```

---

## 🎯 DETECÇÃO DE HOSTS

### Ping Scan
```bash
# Ping scan apenas (sem scan de portas)
nmap -sn 192.168.1.0/24

# Equivalente a:
nmap -sP 192.168.1.0/24

# Técnicas usadas:
# - ICMP echo request
# - TCP SYN para porta 443
# - TCP ACK para porta 80
# - ICMP timestamp request
```

### Host Discovery Opções
```bash
# Desabilitar ping (-Pn)
nmap -Pn 192.168.1.1  # Trata todos os hosts como ativos

# Apenas ping ARP (rede local)
nmap -PR 192.168.1.0/24  # Usa ARP apenas

# Ping via TCP
nmap -PS22,80,443 192.168.1.1  # SYN para portas 22,80,443

# Ping via UDP
nmap -PU53,161 192.168.1.1     # UDP para portas 53,161

# Ping via ICMP
nmap -PE 192.168.1.1  # ICMP echo
nmap -PP 192.168.1.1  # ICMP timestamp
nmap -PM 192.168.1.1  # ICMP netmask
```

### Lista de Hosts
```bash
# De arquivo
nmap -iL hosts.txt 192.168.1.0/24

# Excluir hosts
nmap --exclude 192.168.1.100,192.168.1.101 192.168.1.0/24
nmap --excludefile exclude.txt 192.168.1.0/24

# Range de IPs
nmap 192.168.1.1-254
nmap 192.168.1-10.1-254
nmap 10.0.0-255.1-254
```

### Hosts Aleatórios
```bash
# Scan aleatório de hosts
nmap -iR 10  # 10 endereços IP aleatórios

# Excluir certos IPs
nmap -iR 100 --exclude 10.0.0.0/8,172.16.0.0/12,192.168.0.0/16
```

---

## 🔢 DETECÇÃO DE PORTAS E SERVIÇOS

### Seleção de Portas
```bash
# Portas específicas
nmap -p 22,80,443 192.168.1.1

# Range de portas
nmap -p 1-1000 192.168.1.1
nmap -p 80,100-200,443 192.168.1.1

# Portas por nome
nmap -p http,https,ssh,ftp 192.168.1.1

# Portas por protocolo
nmap -p T:22,80,U:53,67 192.168.1.1

# Portas mais comuns
nmap --top-ports 10 192.168.1.1
nmap --top-ports 100 192.168.1.1

# Todas as portas (1-65535)
nmap -p- 192.168.1.1

# Portas padrão (mais usadas)
nmap 192.168.1.1  # Scan das 1000 portas mais comuns
```

### Detecção de Serviços
```bash
# Detecção de serviço básica
nmap -sV 192.168.1.1

# Intensidade da detecção (0-9)
nmap -sV --version-intensity 5 192.168.1.1
nmap -sV --version-intensity 9 192.168.1.1  # Mais agressivo
nmap -sV --version-intensity 0 192.168.1.1  # Mais leve

# Detecção leve (rápida)
nmap -sV --version-light 192.168.1.1

# Detecção completa (lenta)
nmap -sV --version-all 192.168.1.1

# Forçar detecção mesmo se portas não padrão
nmap -sV --allports 192.168.1.1
```

### Detecção RPC
```bash
# Scan RPC (para serviços SunRPC)
nmap -sR 192.168.1.1

# Combinado com versão
nmap -sV -sR 192.168.1.1
```

---

## 💻 DETECÇÃO DE OS

### OS Detection Básico
```bash
# Detecção de Sistema Operacional
nmap -O 192.168.1.1

# Nível de agressividade (1-9)
nmap -O --osscan-limit 192.168.1.1   # Apenas hosts promissores
nmap -O --osscan-guess 192.168.1.1   # Faz suposições se preciso
nmap -O --max-os-tries 1 192.168.1.1 # Limita tentativas

# Detecção agressiva
nmap -O -sV 192.168.1.1  # OS + versão de serviço
```

### Técnicas de Fingerprinting
```bash
# Usar fingerprint mais antigo se necessário
nmap -O --fuzzy 192.168.1.1

# Servir fingerprints antigos
nmap -O --uselegacy 192.168.1.1
```

### Combinação de Técnicas
```bash
# Scan completo
nmap -A 192.168.1.1
# Equivalente a: -sV -O --traceroute --script default

# Traceroute
nmap --traceroute 192.168.1.1

# Packet trace
nmap --packet-trace 192.168.1.1
```

---

## 🛠️ SCRIPTS NSE (NMAP SCRIPTING ENGINE)

### Categorias de Scripts
```
auth        - Autenticação
broadcast   - Descoberta de rede
brute       - Força bruta
default     - Scripts rodados com -sC ou -A
discovery   - Descoberta de serviços
dos         - Denial of Service
exploit     - Exploração
external    - Dados de fontes externas
fuzzer      - Fuzzing
intrusive   - Scripts intrusivos
malware     - Detecção de malware
safe        - Scripts seguros (não intrusivos)
version     - Detecção de versão
vuln        - Detecção de vulnerabilidades
```

### Uso Básico de Scripts
```bash
# Scripts padrão
nmap -sC 192.168.1.1
# Equivalente a: --script=default

# Script específico
nmap --script http-title 192.168.1.1

# Múltiplos scripts
nmap --script http-title,ssl-cert 192.168.1.1

# Categoria de scripts
nmap --script discovery 192.168.1.1

# Múltiplas categorias
nmap --script "discovery and safe" 192.168.1.1
```

### Scripts Comuns e Úteis
```bash
# HTTP
nmap --script http-headers 192.168.1.1
nmap --script http-methods 192.168.1.1
nmap --script http-enum 192.168.1.1
nmap --script http-sql-injection 192.168.1.1
nmap --script http-vuln-* 192.168.1.1

# SSL/TLS
nmap --script ssl-cert 192.168.1.1
nmap --script ssl-enum-ciphers 192.168.1.1
nmap --script ssl-heartbleed 192.168.1.1

# SMB
nmap --script smb-os-discovery 192.168.1.1
nmap --script smb-enum-shares 192.168.1.1
nmap --script smb-vuln-ms17-010 192.168.1.1  # EternalBlue

# DNS
nmap --script dns-brute 192.168.1.1
nmap --script dns-zone-transfer 192.168.1.1

# FTP
nmap --script ftp-anon 192.168.1.1
nmap --script ftp-brute 192.168.1.1

# MySQL
nmap --script mysql-info 192.168.1.1
nmap --script mysql-enum 192.168.1.1

# SSH
nmap --script ssh-hostkey 192.168.1.1
nmap --script ssh-auth-methods 192.168.1.1

# VNC
nmap --script vnc-info 192.168.1.1
nmap --script vnc-brute 192.168.1.1

# SNMP
nmap --script snmp-info 192.168.1.1
nmap --script snmp-brute 192.168.1.1
```

### Opções de Scripts
```bash
# Com argumentos
nmap --script http-title --script-args http-title.url=/admin 192.168.1.1

# Listar scripts disponíveis
nmap --script-help "http-*"

# Atualizar banco de dados de scripts
nmap --script-updatedb

# Debug de scripts
nmap --script-trace 192.168.1.1

# Timeout de script
nmap --script-timeout 5m 192.168.1.1

# Limitar hosts por script
nmap --script-max-parallelism 10 192.168.1.0/24
```

### Scripts de Vulnerabilidade
```bash
# Scan de vulnerabilidades básico
nmap --script vuln 192.168.1.1

# Vulnerabilidades específicas
nmap --script http-vuln-cve2017-5638 192.168.1.1
nmap --script smb-vuln-ms17-010 192.168.1.1
nmap --script ssl-heartbleed 192.168.1.1
nmap --script http-shellshock 192.168.1.1

# Todos scripts de vuln
nmap --script "vuln and safe" 192.168.1.1
```

### Criação de Scripts Personalizados
```lua
-- Exemplo: script NSE simples
description = [[
Script de exemplo NSE
]]

author = "Seu Nome"
license = "Same as Nmap--See https://nmap.org/book/man-legal.html"
categories = {"safe", "discovery"}

-- Rule: roda se porta 80 está aberta
portrule = function(host, port)
    return port.number == 80 and port.protocol == "tcp" and port.state == "open"
end

-- Ação principal
action = function(host, port)
    local sock = nmap.new_socket()
    local catch = function()
        sock:close()
    end
    
    local try = nmap.new_try(catch)
    try(sock:connect(host.ip, port.number))
    
    sock:send("GET / HTTP/1.0\r\n\r\n")
    local resposta = try(sock:receive_lines(1))
    sock:close()
    
    return resposta
end
```

---

## ⚡ OTIMIZAÇÃO E PERFORMANCE

### Timing Templates
```bash
# Templates pré-definidos (-T0 a -T5)
nmap -T0 192.168.1.1  # Paranoid (muito lento)
nmap -T1 192.168.1.1  # Sneaky (muito lento)
nmap -T2 192.168.1.1  # Polite (lento)
nmap -T3 192.168.1.1  # Normal (padrão)
nmap -T4 192.168.1.1  # Aggressive (rápido)
nmap -T5 192.168.1.1  # Insane (muito rápido)

# Equivalente manual:
nmap --min-hostgroup 256 --min-parallelism 64 192.168.1.0/24
```

### Controle de Hosts Paralelos
```bash
# Tamanho mínimo de grupo de hosts
nmap --min-hostgroup 10 192.168.1.0/24

# Tamanho máximo de grupo de hosts
nmap --max-hostgroup 512 192.168.1.0/24

# Hosts paralelos mínimos
nmap --min-parallelism 10 192.168.1.0/24

# Hosts paralelos máximos
nmap --max-parallelism 100 192.168.1.0/24

# Scan serial (um host de cada vez)
nmap --max-parallelism 1 192.168.1.0/24
```

### Controle de Portas Paralelas
```bash
# Scan rate (pacotes por segundo)
nmap --min-rate 100 192.168.1.1  # Mínimo 100 pps
nmap --max-rate 1000 192.168.1.1 # Máximo 1000 pps

# Tempo entre pacotes
nmap --scan-delay 100ms 192.168.1.1  # 100ms entre pacotes
nmap --max-scan-delay 5s 192.168.1.1 # Máximo 5 segundos

# Timeouts
nmap --initial-rtt-timeout 500ms 192.168.1.1
nmap --min-rtt-timeout 100ms 192.168.1.1
nmap --max-rtt-timeout 2s 192.168.1.1
nmap --host-timeout 15m 192.168.1.1
```

### Otimização para Redes Específicas
```bash
# Rede rápida (LAN)
nmap -T4 --min-rate 1000 192.168.1.0/24

# Rede lenta (Internet)
nmap -T2 --max-rate 100 8.8.8.8

# Para não sobrecarregar equipamentos
nmap -T2 --max-rate 50 192.168.1.1
```

---

## 📊 SAÍDA E FORMATAÇÃO

### Formatos de Saída
```bash
# Normal (stdout)
nmap 192.168.1.1

# Grepable (-oG)
nmap -oG output.txt 192.168.1.1

# XML (-oX)
nmap -oX output.xml 192.168.1.1

# Normal (-oN)
nmap -oN output.txt 192.168.1.1

# Todos formatos
nmap -oA basename 192.168.1.1
# Cria: basename.nmap, basename.xml, basename.gnmap

# Anexar a arquivo existente
nmap -oN - --append-output 192.168.1.1 >> output.txt
```

### Níveis de Verbosidade
```bash
# Mais detalhes (-v)
nmap -v 192.168.1.1

# Muito detalhado (-vv)
nmap -vv 192.168.1.1

# Debug (-d)
nmap -d 192.168.1.1

# Muito debug (-dd)
nmap -dd 192.168.1.1

# Sem resolver DNS (mais rápido)
nmap -n 192.168.1.1

# Sem output interativo
nmap --noninteractive 192.168.1.1

# Progresso
nmap --stats-every 10s 192.168.1.0/24
```

### Processamento de Saída
```bash
# Filtrar resultados
nmap -oG - 192.168.1.0/24 | grep "open"

# Usar awk para processar
nmap -oG - 192.168.1.0/24 | awk '/open/{print $2}'

# Converter XML para HTML
xsltproc output.xml -o output.html

# Visualizar XML
nmap -oX - 192.168.1.1 | xmllint --format -
```

---

## 🎭 TÉCNICAS AVANÇADAS

### Spoofing e Decoys
```bash
# Spoof MAC address
nmap --spoof-mac Cisco 192.168.1.1
nmap --spoof-mac 00:11:22:33:44:55 192.168.1.1
nmap --spoof-mac 0 192.168.1.1  # MAC aleatório

# Spoof IP source (requer root)
nmap -S 192.168.1.100 -e eth0 192.168.1.1

# Decoys (hosts falsos)
nmap -D 192.168.1.101,192.168.1.102,ME 192.168.1.1
nmap -D RND:10 192.168.1.1  # 10 decoys aleatórios

# Interface específica
nmap -e eth0 192.168.1.0/24

# IP source específico (para gateway)
nmap --source-port 53 192.168.1.1
```

### Fragmentação e MTU
```bash
# Fragmentar pacotes
nmap -f 192.168.1.1
nmap -f -f 192.168.1.1  # Fragmentação dupla

# MTU específico
nmap --mtu 24 192.168.1.1

# Data length
nmap --data-length 100 192.168.1.1  # Adiciona 100 bytes aleatórios

# Badsum (checksum inválido)
nmap --badsum 192.168.1.1  # Testa firewalls que não verificam checksum
```

### TTL Manipulation
```bash
# TTL específico
nmap --ttl 64 192.168.1.1

# IP options
nmap --ip-options "S 192.168.1.100" 192.168.1.1
```

### IPv6 Scanning
```bash
# Scan IPv6
nmap -6 2001:db8::1

# IPv6 com range
nmap -6 2001:db8::/64

# Combinação IPv4/IPv6
nmap -6 -sT 2001:db8::1
```

### WiFi/802.11 Scanning
```bash
# Para redes WiFi
nmap --iflist  # Lista interfaces

# Com monitor mode (requer privilégios)
nmap -e wlan0 192.168.1.0/24
```

---

## 🕵️ EVASION E STEALTH

### Técnicas de Evasão
```bash
# SYN scan (meio aberto)
nmap -sS 192.168.1.1  # Padrão stealth

# FIN, NULL, Xmas scans
nmap -sF 192.168.1.1
nmap -sN 192.168.1.1
nmap -sX 192.168.1.1

# Idle scan (zombie)
nmap -sI zombie_ip 192.168.1.1

# Fragmentação
nmap -f --mtu 8 192.168.1.1

# Time manipulation
nmap -T0 --scan-delay 5s 192.168.1.1
nmap --max-rate 10 192.168.1.1

# Decoys
nmap -D decoy1,decoy2,decoy3,ME 192.168.1.1
```

### Bypass Firewalls/IDS
```bash
# Source port específica
nmap --source-port 53 192.168.1.1  # DNS
nmap --source-port 20 192.168.1.1  # FTP-data

# Hex encoding
echo "68656c6c6f" | xxd -r -p | nc 192.168.1.1 80

# Append random data
nmap --data-length 200 192.168.1.1

# Randomize hosts/ports
nmap --randomize-hosts 192.168.1.0/24
nmap --randomize-hosts --randomize-ports 192.168.1.0/24
```

### Detecção de Firewalls
```bash
# ACK scan para detectar firewalls
nmap -sA 192.168.1.1

# Window scan
nmap -sW 192.168.1.1

# Maimon scan
nmap -sM 192.168.1.1
```

---

## 🐍 INTEGRAÇÃO COM PYTHON

### Python-nmap
```python
import nmap

# Inicializar scanner
nm = nmap.PortScanner()

# Scan básico
resultado = nm.scan('192.168.1.1', '22-443')
print(resultado)

# Scan com argumentos
resultado = nm.scan(
    hosts='192.168.1.0/24',
    arguments='-sS -sV -O -T4'
)

# Acessar resultados
for host in nm.all_hosts():
    print(f'Host: {host}')
    print(f'State: {nm[host].state()}')
    
    for proto in nm[host].all_protocols():
        print(f'Protocol: {proto}')
        
        ports = nm[host][proto].keys()
        for port in ports:
            print(f'Port: {port} - State: {nm[host][proto][port]["state"]}')
            print(f'Service: {nm[host][proto][port]["name"]}')
```

### Subprocess
```python
import subprocess
import xml.etree.ElementTree as ET

# Executar Nmap e capturar XML
resultado = subprocess.run(
    ['nmap', '-oX', '-', '-sV', '192.168.1.1'],
    capture_output=True,
    text=True
)

# Parse XML
root = ET.fromstring(resultado.stdout)

for host in root.findall('host'):
    addr = host.find('address').get('addr')
    status = host.find('status').get('state')
    print(f'Host: {addr} - Status: {status}')
    
    for port in host.findall('.//port'):
        portid = port.get('portid')
        state = port.find('state').get('state')
        service = port.find('service').get('name', 'unknown')
        print(f'  Port {portid}: {state} - {service}')
```

### Biblioteca asyncio-nmap
```python
import asyncio
from asyncio import subprocess

async def scan_host(host, ports='1-1000'):
    cmd = ['nmap', '-sS', '-p', ports, host]
    
    processo = await asyncio.create_subprocess_exec(
        *cmd,
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE
    )
    
    stdout, stderr = await processo.communicate()
    
    if processo.returncode == 0:
        return stdout.decode()
    else:
        return stderr.decode()

# Executar múltiplos scans
async def main():
    hosts = ['192.168.1.1', '192.168.1.2', '192.168.1.3']
    tasks = [scan_host(host) for host in hosts]
    resultados = await asyncio.gather(*tasks)
    
    for host, resultado in zip(hosts, resultados):
        print(f'Resultado para {host}:')
        print(resultado[:500])  # Primeiros 500 caracteres

asyncio.run(main())
```

### Parse Grepable Output
```python
def parse_grepable_output(output):
    """Parse saída -oG do Nmap"""
    resultados = {}
    
    for linha in output.strip().split('\n'):
        if linha.startswith('#'):
            continue
        
        partes = linha.split('\t')
        if len(partes) < 2:
            continue
        
        # Extrair IP
        host_info = partes[0].split()
        ip = host_info[1]
        
        # Extrair portas abertas
        portas_abertas = []
        for parte in partes[1:]:
            if 'Ports:' in parte:
                portas = parte.split('Ports: ')[1].split(', ')
                for porta_info in portas:
                    if '/open/' in porta_info:
                        porta = porta_info.split('/')[0]
                        portas_abertas.append(porta)
        
        if portas_abertas:
            resultados[ip] = portas_abertas
    
    return resultados

# Uso
with open('scan.gnmap', 'r') as f:
    dados = parse_grepable_output(f.read())
    print(dados)
```

---

## 🔥 EXEMPLOS PRÁTICOS

### 1. Auditoria de Rede Completa
```bash
#!/bin/bash
# audit_network.sh

DATE=$(date +%Y%m%d_%H%M%S)
TARGET="192.168.1.0/24"

echo "[*] Iniciando auditoria de rede completa..."
echo "[*] Data/Hora: $(date)"
echo "[*] Alvo: $TARGET"

# 1. Host discovery
echo "[*] Fase 1: Descoberta de hosts..."
nmap -sn -oA "audit_${DATE}_hosts" $TARGET

# 2. Port scan rápido
echo "[*] Fase 2: Scan rápido de portas..."
nmap -sS -T4 --top-ports 100 -oA "audit_${DATE}_fast" $TARGET

# 3. Scan detalhado dos hosts ativos
echo "[*] Fase 3: Scan detalhado..."
for ip in $(grep "Nmap scan report" audit_${DATE}_hosts.gnmap | awk '{print $5}'); do
    echo "  [+] Scanning $ip..."
    nmap -sS -sV -O -p- -T4 -oA "audit_${DATE}_${ip}" $ip
done

# 4. Scan de vulnerabilidades
echo "[*] Fase 4: Scan de vulnerabilidades..."
for ip in $(grep "open" audit_${DATE}_fast.gnmap | awk '{print $2}' | sort -u); do
    echo "  [+] Checking vulnerabilities on $ip..."
    nmap --script vuln -oA "audit_${DATE}_vuln_${ip}" $ip
done

echo "[+] Auditoria concluída!"
```

### 2. Monitor de Portas Abertas
```python
#!/usr/bin/env python3
# port_monitor.py

import subprocess
import time
import json
from datetime import datetime
import difflib

def scan_host(host, ports='1-1000'):
    """Realiza scan de portas em um host"""
    cmd = ['nmap', '-sS', '-p', ports, '-oG', '-', host]
    
    resultado = subprocess.run(cmd, capture_output=True, text=True)
    
    portas_abertas = []
    for linha in resultado.stdout.split('\n'):
        if '/open/' in linha:
            partes = linha.split()
            for parte in partes:
                if '/open/' in parte:
                    porta = parte.split('/')[0]
                    portas_abertas.append(int(porta))
    
    return sorted(portas_abertas)

def monitor_host(host, intervalo=300):
    """Monitora alterações nas portas abertas"""
    
    historico = {}
    
    try:
        with open(f'monitor_{host}.json', 'r') as f:
            historico = json.load(f)
    except FileNotFoundError:
        pass
    
    print(f"[*] Monitorando {host} (intervalo: {intervalo}s)")
    print(f"[*] Pressione Ctrl+C para parar")
    
    while True:
        agora = datetime.now().isoformat()
        print(f"\n[{agora}] Scan iniciado...")
        
        portas = scan_host(host)
        
        if host in historico:
            portas_anteriores = historico[host]['portas']
            
            if portas != portas_anteriores:
                print(f"[!] ALTERAÇÃO DETECTADA!")
                
                novas = set(portas) - set(portas_anteriores)
                fechadas = set(portas_anteriores) - set(portas)
                
                if novas:
                    print(f"[+] Portas novas: {sorted(novas)}")
                if fechadas:
                    print(f"[-] Portas fechadas: {sorted(fechadas)}")
            else:
                print(f"[✓] Sem alterações")
        
        # Atualizar histórico
        historico[host] = {
            'ultimo_scan': agora,
            'portas': portas,
            'total_portas': len(portas)
        }
        
        # Salvar
        with open(f'monitor_{host}.json', 'w') as f:
            json.dump(historico, f, indent=2)
        
        print(f"[*] Portas atuais ({len(portas)}): {portas}")
        
        # Aguardar próximo scan
        time.sleep(intervalo)

if __name__ == "__main__":
    import sys
    
    if len(sys.argv) < 2:
        print(f"Uso: {sys.argv[0]} <host> [intervalo_em_segundos]")
        sys.exit(1)
    
    host = sys.argv[1]
    intervalo = int(sys.argv[2]) if len(sys.argv) > 2 else 300
    
    try:
        monitor_host(host, intervalo)
    except KeyboardInterrupt:
        print("\n[*] Monitoramento interrompido pelo usuário")
```

### 3. Scanner de Vulnerabilidades Web
```bash
#!/bin/bash
# web_vuln_scanner.sh

TARGET_FILE="targets.txt"
OUTPUT_DIR="web_scan_$(date +%Y%m%d)"

mkdir -p "$OUTPUT_DIR"

echo "[*] Iniciando scanner de vulnerabilidades web"
echo "[*] Targets: $(cat $TARGET_FILE | wc -l) hosts"

while read -r target; do
    echo "[+] Scanning $target"
    
    # 1. Port scan para serviços web
    nmap -sS -p 80,443,8080,8443,8000,8008 -oN "$OUTPUT_DIR/${target}_ports.txt" "$target"
    
    # 2. Detecção de serviço HTTP/HTTPS
    if grep -q "80/open\|443/open\|8080/open\|8443/open" "$OUTPUT_DIR/${target}_ports.txt"; then
        echo "  [+] Serviço web detectado"
        
        # 3. Scripts HTTP
        nmap --script "http-*" -oN "$OUTPUT_DIR/${target}_http_scripts.txt" "$target"
        
        # 4. Vulnerabilidades específicas
        nmap --script "http-vuln-*" -oN "$OUTPUT_DIR/${target}_http_vuln.txt" "$target"
        
        # 5. SSL/TLS (se porta 443)
        if grep -q "443/open" "$OUTPUT_DIR/${target}_ports.txt"; then
            nmap --script "ssl-*" -oN "$OUTPUT_DIR/${target}_ssl.txt" "$target"
        fi
        
        # 6. Directory enumeration
        nmap -p 80,443 --script http-enum -oN "$OUTPUT_DIR/${target}_dirs.txt" "$target"
    fi
    
done < "$TARGET_FILE"

echo "[+] Scan completo! Resultados em: $OUTPUT_DIR"
```

### 4. Scan de Rede Corporativa
```bash
#!/bin/bash
# corporate_scan.sh

NETWORK="10.0.0.0/16"
EXCLUDE_FILE="exclude_list.txt"
PARALLEL_SCANS=50
OUTPUT="corporate_scan_$(date +%Y%m%d)"

echo "[*] Corporate Network Assessment"
echo "[*] Network: $NETWORK"
echo "[*] Excluding: $(cat $EXCLUDE_FILE | wc -l) hosts"

# Fase 1: Quick host discovery
echo "[*] Phase 1: Host Discovery"
nmap -sn -PE -PS21,22,23,25,80,443,3389 \
     --excludefile $EXCLUDE_FILE \
     -oA "${OUTPUT}_hosts" \
     $NETWORK

# Extrair hosts ativos
grep "Nmap scan report" ${OUTPUT}_hosts.gnmap | awk '{print $5}' > active_hosts.txt

# Fase 2: Quick port scan
echo "[*] Phase 2: Quick Port Scan"
nmap -sS -T4 --top-ports 100 \
     --max-hostgroup $PARALLEL_SCANS \
     --excludefile $EXCLUDE_FILE \
     -iL active_hosts.txt \
     -oA "${OUTPUT}_quick"

# Identificar servidores importantes
echo "[*] Identifying critical servers"

# Servidores web
grep "80/open\|443/open" ${OUTPUT}_quick.gnmap | awk '{print $2}' > web_servers.txt

# Servidores de banco de dados
grep "3306/open\|5432/open\|1433/open\|1521/open" ${OUTPUT}_quick.gnmap | awk '{print $2}' > db_servers.txt

# Servidores Windows
grep "3389/open\|445/open" ${OUTPUT}_quick.gnmap | awk '{print $2}' > windows_servers.txt

# Fase 3: Deep scan em servidores críticos
echo "[*] Phase 3: Deep Scan Critical Servers"

for category in web db windows; do
    if [ -s "${category}_servers.txt" ]; then
        echo "  [+] Scanning ${category} servers"
        nmap -sS -sV -O -A -T4 \
             -iL "${category}_servers.txt" \
             -oA "${OUTPUT}_${category}_deep"
    fi
done

# Fase 4: Vulnerability assessment
echo "[*] Phase 4: Vulnerability Assessment"

for ip in $(cat active_hosts.txt); do
    echo "  [+] Checking $ip"
    nmap --script vuln -oN "${OUTPUT}_vuln_${ip}.txt" $ip
done

echo "[+] Assessment complete!"
echo "[+] Summary:"
echo "    Active hosts: $(wc -l < active_hosts.txt)"
echo "    Web servers: $(wc -l < web_servers.txt)"
echo "    Database servers: $(wc -l < db_servers.txt)"
echo "    Windows servers: $(wc -l < windows_servers.txt)"
```

---

## 📋 REFERÊNCIA RÁPIDA

### Comandos Mais Usados
```bash
# Scan básico
nmap 192.168.1.1

# Scan stealth
nmap -sS 192.168.1.1

# Scan com versão de serviço
nmap -sV 192.168.1.1

# Scan completo
nmap -A 192.168.1.1

# Scan de rede
nmap 192.168.1.0/24

# Portas específicas
nmap -p 22,80,443 192.168.1.1

# Todas as portas
nmap -p- 192.168.1.1

# Output para arquivo
nmap -oN scan.txt 192.168.1.1
```

### Opções Comuns
```bash
TARGET SPECIFICATION:
  -iL <inputfilename>    Entrada de arquivo de hosts
  -iR <num hosts>        Escolhe hosts aleatórios
  --exclude <host1[,host2][,host3],...> Excluir hosts
  --excludefile <exclude_file>  Excluir de arquivo

HOST DISCOVERY:
  -sL                    List scan - apenas lista
  -sn                    Ping scan - sem scan de portas
  -Pn                    Trata todos hosts como online (skip ping)
  -PS/PA/PU/PY[portlist] TCP SYN/ACK, UDP ou SCTP ping
  -PE/PP/PM             ICMP echo, timestamp, netmask request
  -PO[protocol list]    IP Protocol Ping

SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM       TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU                   UDP Scan
  -sN/sF/sX             TCP Null, FIN, e Xmas scans
  --scanflags <flags>   Customiza flags TCP
  -sI <zombie host[:probeport]> Idle scan
  -sY/sZ                SCTP INIT/COOKIE-ECHO scans
  -sO                   IP protocol scan

PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>      Só escaneia portas especificadas
  -F                    Fast mode - menos portas
  -r                    Escaneia portas em ordem sequencial
  --top-ports <number>  Escaneia <number> portas mais comuns
  --port-ratio <ratio>  Escaneia portas mais comuns que <ratio>

SERVICE/VERSION DETECTION:
  -sV                   Detecção de versão de serviço
  --version-intensity <level> Setar de 0 (light) a 9 (try all probes)
  --version-light       Limitar para as probes mais prováveis (intensity 2)
  --version-all         Tentar todas as probes (intensity 9)
  --version-trace       Mostrar atividade extensa (para debugging)

OS DETECTION:
  -O                    Ativa detecção de OS
  --osscan-limit        Limitar detecção OS para alvos promissores
  --osscan-guess        Adivinha mais agressivamente

SCRIPT SCAN:
  -sC                   Equivalente a --script=default
  --script=<Lua scripts> <script1>[,<script2>[,...]]
  --script-args=<n1=v1,[n2=v2,...]> Provê argumentos para scripts
  --script-args-file=filename Provê args NSE em um arquivo
  --script-trace        Mostrar todo o envio e recebimento de dados
  --script-updatedb     Atualizar o banco de dados de scripts
  --script-help=<Lua scripts> Mostrar ajuda sobre os scripts

TIMING AND PERFORMANCE:
  -T<0-5>               Setar timing template (maior é mais rápido)
  --min-hostgroup/max-hostgroup <size> Tamanho do grupo de hosts paralelos
  --min-parallelism/max-parallelism <numprobes> Sonda paralela
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time> 
  --max-retries <tries> Número máximo de tentativas de retransmissão de porta
  --host-timeout <time> Dá tempo limite após muito tempo
  --scan-delay/--max-scan-delay <time> Ajusta atraso entre sondas

FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>       fragmenta pacotes (opcional com MTU dado)
  -D <decoy1,decoy2[,ME],...> Esconde scan com decoys
  -S <IP_Address>       Endereço IP de origem
  -e <iface>            Usar interface especificada
  -g/--source-port <portnum> Usa número de porta de origem dado
  --proxies <url1,[url2],...> Relay conexões via HTTP/SOCKS4 proxies
  --data <hex string>   Anexa um payload customizado aos pacotes enviados
  --data-string <string> Anexa um string ASCII customizado
  --data-length <num>   Anexa dados randômicos ao pacote
  --ip-options <options> Envia pacotes com ip options especificadas
  --ttl <val>           Seta campo TTL do IP
  --spoof-mac <mac address/prefix/vendor name> Spoof seu MAC address
  --badsum              Envia pacotes com checksum TCP/UDP incorreto

OUTPUT:
  -oN/-oX/-oS/-oG <file> Output normal, XML, s|<rIpt kIddi3, e Grepable, respectivamente, para um arquivo
  -oA <basename>        Output nos três formatos principais de uma vez
  -v                    Aumenta o nível de verbosidade (use -vv ou mais para mais efeito)
  -d                    Aumenta nível de debugging (use -dd ou mais para mais efeito)
  --reason              Mostra o motivo pelo qual uma porta está em certo estado
  --open                Só mostra portas abertas (ou possivelmente abertas)
  --packet-trace        Mostra todos os pacotes enviados e recebidos
  --iflist              Printa interfaces de host e rotas (para debugging)
  --log-errors          Loga erros/warnings no formato de output normal
  --append-output       Anexa ao invés de clobber arquivos de output especificados
  --resume <filename>   Retoma um scan abortado
  --stylesheet <path/URL> XSL stylesheet para transformar XML output para HTML
  --webxml              Referencia stylesheet do Nmap.Org para XML mais portável
  --no-stylesheet       Previne associação de XSL stylesheet com XML output
```

### Cheat Sheet Visual
```
┌─────────────────────────────────────────────────────────────┐
│                      NMAP CHEAT SHEET                        │
├─────────────┬───────────────────────────────────────────────┤
│ Scan Type   │ Command                                       │
├─────────────┼───────────────────────────────────────────────┤
│ Stealth     │ nmap -sS target                               │
│ Version     │ nmap -sV target                               │
│ OS Detect   │ nmap -O target                                │
│ All Ports   │ nmap -p- target                               │
│ Fast        │ nmap -T4 -F target                            │
│ Ping        │ nmap -sn target/24                            │
│ UDP         │ nmap -sU -p 53,123,161 target                │
│ Scripts     │ nmap -sC target                               │
│ Vuln Scan   │ nmap --script vuln target                     │
│ Output All  │ nmap -oA basename target                      │
└─────────────┴───────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    TIMING TEMPLATES                          │
├─────┬────────────┬─────────────────────────────────────────┤
│ -T0 │ Paranoid   │ 5 min entre pacotes, evita IDS           │
│ -T1 │ Sneaky     │ 15 seg entre pacotes                     │
│ -T2 │ Polite     │ 0.4 seg entre pacotes (padrão antigo)    │
│ -T3 │ Normal     │ Balanceado (padrão)                      │
│ -T4 │ Aggressive │ Assume rede rápida, tempo menor          │
│ -T5 │ Insane     │ Muito agressivo, pode perder pacotes     │
└─────┴────────────┴─────────────────────────────────────────┘
```

---

## ⚠️ CONSIDERAÇÕES LEGAIS E ÉTICAS

### Uso Responsável
1. **SEMPRE obtenha permissão** antes de scanear qualquer rede
2. **Use apenas em sistemas próprios** ou com autorização explícita
3. **Respeite políticas** de uso aceitável
4. **Documente todas as atividades** com autorizações
5. **Não cause danos** ou interrupções de serviço

### Cenários Legítimos
- Testes de penetração autorizados
- Auditorias de segurança
- Monitoramento de rede própria
- Pesquisa acadêmica (com supervisão)
- Administração de sistemas

### Comandos "Seguros"
```bash
# Testes em localhost (sempre seguro)
nmap localhost
nmap -sS 127.0.0.1
nmap -A scanme.nmap.org  # Site autorizado para testes

# Verificar suas próprias portas abertas
nmap -sS seu_ip
```

### Boas Práticas
```bash
# Limitar taxa para não sobrecarregar
nmap -T2 --max-rate 100 target

# Scan em horários apropriados
# (evitar horário comercial para testes de penetração)

# Documentar tudo
nmap -oA scan_com_log -v --reason target
```

---

## 🔧 TROUBLESHOOTING

### Problemas Comuns
```bash
# Problema: Nmap lento
# Solução: Ajustar timing
nmap -T4 --min-rate 1000 target
nmap --max-retries 1 target

# Problema: Hosts não respondendo
# Solução: Verificar firewall/IDS
nmap -Pn target  # Ignora host discovery
nmap -f target   # Fragmenta pacotes

# Problema: Sem privilégios root
# Solução: Usar TCP Connect scan
nmap -sT target  # Não requer root

# Problema: UDP scan muito lento
# Solução: Limitar portas e aumentar timeout
nmap -sU -p 53,123,161 --max-retries 0 --max-rtt-timeout 500ms target

# Problema: Output confuso
# Solução: Usar formatos específicos
nmap -oX resultado.xml target  # XML para processamento
nmap -oG resultado.gnmap target # Grepable para scripts
```

### Debugging
```bash
# Verificar pacotes
nmap --packet-trace target

# Debug detalhado
nmap -d target
nmap -dd target  # Mais detalhes

# Verificar interfaces
nmap --iflist

# Testar conectividade
nmap -v -Pn -p 80 target
```

---

## 📚 RECURSOS ADICIONAIS

### Documentação
- **Site oficial**: https://nmap.org
- **Manual online**: https://nmap.org/book/man.html
- **NSE Documentation**: https://nmap.org/nsedoc/

### Livros Recomendados
- "Nmap Network Scanning" by Gordon Lyon (Fyodor)
- "The Nmap Bible" - Comprehensive guide
- "Mastering Nmap Scripting Engine"

### Comunidade
- **Lista de email**: Seclists.org/nmap-dev/
- **Fórum**: https://nmap.org/community/
- **IRC**: irc.freenode.net #nmap

### Ferramentas Relacionadas
```bash
# Zenmap (GUI para Nmap)
zenmap

# Ndiff (compara scans)
ndiff scan1.xml scan2.xml

# Ncat (netcat melhorado)
ncat -v target port

# Nping (packet generator)
nping --tcp -p 80 --flags syn target
```

---

## 🎓 CERTIFICAÇÕES

### Relação com Certificações
- **CEH (Certified Ethical Hacker)**: Nmap é ferramenta essencial
- **OSCP (Offensive Security Certified Professional)**: Uso intensivo
- **GPEN (GIAC Penetration Tester)**: Parte do currículo
- **Security+**: Conceitos de scanning de rede

### Prática para Certificação
```bash
# Exercícios típicos de certificação
# 1. Descobrir hosts ativos em uma rede
nmap -sn 192.168.1.0/24

# 2. Identificar serviços em portas não padrão
nmap -sV -p 10000-20000 target

# 3. Detect
