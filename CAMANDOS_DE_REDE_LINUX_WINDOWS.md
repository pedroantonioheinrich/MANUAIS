---

# 📘 MANUAL COMPLETO DE COMANDOS DE REDE
## *Linux e Windows – Diagnóstico, Configuração e Gerenciamento*

---

## 📌 ÍNDICE

1. [Introdução](#1-introdução)
2. [Como abrir o terminal/CMD](#2-como-abrir-o-terminalcmd)
3. [Comandos de Diagnóstico e Conectividade](#3-comandos-de-diagnóstico-e-conectividade)
4. [Comandos de Configuração de Interface](#4-comandos-de-configuração-de-interface)
5. [Comandos de Roteamento](#5-comandos-de-roteamento)
6. [Comandos de DNS](#6-comandos-de-dns)
7. [Comandos de Monitoramento e Estatísticas](#7-comandos-de-monitoramento-e-estatísticas)
8. [Comandos de Segurança e Firewall](#8-comandos-de-segurança-e-firewall)
9. [Comandos para Redes Sem Fio (Wi-Fi)](#9-comandos-para-redes-sem-fio-wi-fi)
10. [Comandos de Transferência de Arquivos](#10-comandos-de-transferência-de-arquivos)
11. [Comandos de Acesso Remoto](#11-comandos-de-acesso-remoto)
12. [Tabela de Referência Rápida](#12-tabela-de-referência-rápida)

---

## 1. INTRODUÇÃO

Este manual reúne os principais comandos de rede para **Linux** e **Windows**, organizados de forma didática. Cada comando inclui:

- **Descrição**: O que faz
- **Sintaxe básica**: Como usar
- **Exemplos práticos**: Aplicações reais
- **Principais parâmetros**: Opções mais úteis
- **Disponibilidade**: Linux, Windows ou ambos

> 💡 **Dica**: Comandos no Linux geralmente exigem privilégios de superusuário (root) para configurações. Use `sudo` antes do comando quando necessário.

---

## 2. COMO ABRIR O TERMINAL/CMD

### 2.1. Windows (CMD/PowerShell)

| Método | Instrução |
|--------|-----------|
| **Atalho** | Tecla `Windows + R`, digite `cmd` e pressione Enter  |
| **Menu Iniciar** | Pesquise por "cmd" ou "Símbolo do sistema"  |
| **Como administrador** | Clique com botão direito no ícone e selecione "Executar como administrador" |

### 2.2. Linux

| Ambiente | Instrução |
|----------|-----------|
| **Atalho padrão** | `Ctrl + Alt + T`  |
| **Menu gráfico** | Procure por "Terminal" no menu de aplicativos  |

### 2.3. macOS

| Método | Instrução |
|--------|-----------|
| **Finder** | Aplicativos > Utilitários > Terminal  |
| **Spotlight** | `Cmd + Espaço`, digite "Terminal" |

---

## 3. COMANDOS DE DIAGNÓSTICO E CONECTIVIDADE

### 3.1. ping – Teste de Conectividade Básico

**Descrição**: Envia pacotes ICMP para testar se um host está acessível e medir o tempo de resposta .

**Linux**:
```bash
ping -c 4 google.com
```
O parâmetro `-c` define o número de pacotes a enviar .

**Windows**:
```cmd
ping google.com
```
No Windows, o comando continua até ser interrompido com `Ctrl+C`.

**Exemplo de saída**:
```
PING google.com (142.250.218.78): 56 data bytes
64 bytes from 142.250.218.78: icmp_seq=0 ttl=115 time=12.5 ms
64 bytes from 142.250.218.78: icmp_seq=1 ttl=115 time=12.8 ms
```

**Principais parâmetros**:
- `-c [n]` (Linux): Envia n pacotes
- `-t [n]` (Windows): Envia n pacotes (TTL)
- `-i [s]` (Linux): Intervalo entre pacotes
- `-n` (Windows): Não resolve nomes (mais rápido)

### 3.2. traceroute / tracert – Rastreamento de Rota

**Descrição**: Mostra o caminho que os pacotes percorrem até o destino, listando cada roteador intermediário (salto) .

**Linux**:
```bash
traceroute 8.8.8.8
```

**Windows**:
```cmd
tracert google.com
```

**Exemplo de saída**:
```
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  2.5 ms  2.2 ms  2.4 ms
 2  201.10.20.1 (201.10.20.1)  15.2 ms  15.8 ms  16.1 ms
 3  189.40.200.1 (189.40.200.1)  25.3 ms  25.7 ms  26.0 ms
```

**Principais parâmetros**:
- `-I` (Linux): Usa pacotes ICMP em vez de UDP
- `-d` (Windows): Não resolve endereços para nomes
- `-w [ms]` (Windows): Timeout em milissegundos

### 3.3. mtr – Combinação de ping e traceroute

**Descrição**: Ferramenta que combina `ping` e `traceroute`, atualizando estatísticas em tempo real .

**Linux**:
```bash
mtr 8.8.8.8
```

**Windows**: Não nativo (equivalente: `pathping`)

**Exemplo de saída** (atualizada continuamente):
```
                                       Packets               Pings
 Host                                Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 192.168.1.1                       0.0%    10    2.1   2.3   1.9   3.2   0.4
 2. 201.10.20.1                        0.0%    10   15.3  16.1  14.8  19.2   1.3
```

### 3.4. pathping – Diagnóstico Avançado (Windows)

**Descrição**: Combina as funcionalidades de `tracert` e `ping`, analisando perda de pacotes em cada salto .

**Windows**:
```cmd
pathping google.com
```

**Exemplo de saída**:
```
Rastreando rota para google.com [142.250.218.78]
com no máximo 30 saltos:
  0  DESKTOP-PC [192.168.1.10]
  1  192.168.1.1
  2  201.10.20.1

Calculando estatísticas para 125 segundos...
           Fonte para este Host
 Salto  RTT  Perda/Enviado = Perda%  Endereço
  0                           192.168.1.10
  1    2ms     0/ 100 =   0%  192.168.1.1
  2   15ms     1/ 100 =   1%  201.10.20.1
```

---

## 4. COMANDOS DE CONFIGURAÇÃO DE INTERFACE

### 4.1. ipconfig (Windows) / ifconfig (Linux) – Exibir Configurações

**Descrição**: Mostra as configurações das interfaces de rede .

**Linux (tradicional)**:
```bash
ifconfig
```
No Linux moderno, `ifconfig` está sendo substituído pelo comando `ip`.

**Linux (moderno)**:
```bash
ip addr show
```

**Windows**:
```cmd
ipconfig /all
```
O parâmetro `/all` exibe informações detalhadas, incluindo MAC address e servidores DNS .

**Exemplo de saída (Windows)**:
```
Adaptador Ethernet:
   Sufixo DNS específico da conexão:
   Endereço IPv4: 192.168.1.10
   Máscara de sub-rede: 255.255.255.0
   Gateway padrão: 192.168.1.1
   Servidores DNS: 8.8.8.8
```

**Principais parâmetros (Windows)**:
- `ipconfig /release`: Libera o IP atual
- `ipconfig /renew`: Renova o IP via DHCP 
- `ipconfig /flushdns`: Limpa cache DNS 
- `ipconfig /displaydns`: Exibe cache DNS 

**Principais parâmetros (Linux - ip)**:
- `ip addr show`: Mostra endereços
- `ip link show`: Mostra estado das interfaces
- `ip -s link`: Mostra estatísticas

### 4.2. netsh – Configuração Avançada (Windows)

**Descrição**: Ferramenta poderosa para configurar todos os aspectos de rede no Windows .

**Sintaxe básica**:
```cmd
netsh interface [contexto] [comando]
```

**Exemplos práticos**:

| Objetivo | Comando |
|----------|---------|
| Configurar IP estático | `netsh interface ipv4 set address "Ethernet" static 192.168.1.100 255.255.255.0 192.168.1.1`  |
| Configurar DNS | `netsh interface ipv4 set dnsservers "Ethernet" static 8.8.8.8`  |
| Usar DHCP | `netsh interface ipv4 set address "Ethernet" dhcp`  |
| Resetar TCP/IP | `netsh int ip reset`  |
| Resetar Winsock | `netsh winsock reset`  |

### 4.3. ethtool – Configuração de Interface (Linux)

**Descrição**: Exibe e modifica configurações de placas de rede Ethernet .

**Exemplos**:

```bash
# Verificar configurações da interface
ethtool eno1
```

```bash
# Fazer LED da placa piscar para identificar fisicamente
ethtool -p eno1 20   # Pisca por 20 segundos 
```

```bash
# Configurar velocidade e duplex manualmente
ethtool -s eno1 speed 100 duplex full autoneg off 
```

**Saída típica**:
```
Settings for eno1:
    Supported ports: [ TP ]
    Supported link modes: 10baseT/Half 10baseT/Full
                          100baseT/Half 100baseT/Full
                          1000baseT/Full
    Speed: 1000Mb/s
    Duplex: Full
    Auto-negotiation: on
```

### 4.4. nmtui – Configuração Gráfica no Terminal (Linux)

**Descrição**: Interface interativa no terminal para configurar redes no Linux (NetworkManager) .

**Uso**:
```bash
nmtui
```
Navegue com as setas do teclado para configurar interfaces, conexões e hostname.

### 4.5. Arquivos de Configuração (Linux)

**Configuração permanente de interfaces**:
```bash
# Editar arquivo de interfaces
sudo nano /etc/network/interfaces
```

**Exemplo de configuração** :
```
auto lo
iface lo inet loopback

# DHCP
auto eth0
allow-hotplug eth0
iface eth0 inet dhcp

# IP Estático
auto eth1
iface eth1 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

**Gerenciar interfaces**:
```bash
# Ativar/desativar interfaces
sudo ifup eth0
sudo ifdown eth0

# Reiniciar serviço de rede
sudo systemctl restart networking 
```

---

## 5. COMANDOS DE ROTEAMENTO

### 5.1. route – Tabela de Roteamento

**Descrição**: Exibe e manipula a tabela de roteamento IP .

**Linux**:
```bash
# Exibir tabela de roteamento
route -n   # -n: não resolve nomes (mais rápido) 
```

**Windows**:
```cmd
route print
```

**Exemplo de saída**:
```
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0        0 eth0
192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
```

**Adicionar rota estática** :
```bash
# Linux
sudo route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.1.254

# Windows
route add 192.168.2.0 mask 255.255.255.0 192.168.1.254
```

### 5.2. ip route – Roteamento Moderno (Linux)

**Descrição**: Versão moderna do comando de roteamento .

**Exemplos**:
```bash
# Mostrar tabela de roteamento
ip route show

# Adicionar rota padrão
sudo ip route add default via 192.168.1.1

# Adicionar rota específica
sudo ip route add 10.0.0.0/24 via 192.168.1.254 dev eth0
```

---

## 6. COMANDOS DE DNS

### 6.1. nslookup – Consulta DNS

**Descrição**: Consulta servidores DNS para obter resolução de nomes .

**Uso básico**:
```bash
nslookup google.com
```

**Modo interativo**:
```bash
nslookup
> server 8.8.8.8     # Muda servidor DNS
> set type=mx        # Consulta registros MX
> gmail.com
> exit
```

### 6.2. dig – Consulta DNS Avançada (Linux)

**Descrição**: Ferramenta mais completa que `nslookup` para consultas DNS .

**Exemplos**:
```bash
# Consulta padrão
dig google.com

# Consulta específica (registro MX)
dig gmail.com MX

# Consulta para servidor específico
dig @8.8.8.8 google.com

# Resposta curta
dig +short google.com
```

### 6.3. host – Consulta DNS Simples (Linux)

**Descrição**: Versão simplificada para consultas DNS.

**Exemplos**:
```bash
host google.com
host -t mx gmail.com
```

### 6.4. Gerenciamento de DNS no Windows

```cmd
# Limpar cache DNS
ipconfig /flushdns   

# Exibir cache DNS
ipconfig /displaydns 

# Registrar DNS novamente
ipconfig /registerdns
```

---

## 7. COMANDOS DE MONITORAMENTO E ESTATÍSTICAS

### 7.1. netstat – Estatísticas de Rede

**Descrição**: Exibe conexões de rede, tabelas de roteamento, estatísticas de interface e muito mais .

**Exemplos no Windows**:
```cmd
# Mostrar todas as conexões e portas em escuta
netstat -a

# Mostrar conexões com o processo associado (PID)
netstat -ano   

# Mostrar estatísticas por protocolo
netstat -s

# Mostrar tabela de roteamento
netstat -r
```

**Exemplos no Linux**:
```bash
# Conexões ativas
netstat -tunap

# Estatísticas de interface
netstat -i

# Escutando portas
netstat -lntp
```

### 7.2. tcpdump – Análise de Pacotes (Linux)

**Descrição**: Captura e analisa pacotes de rede em tempo real .

**Exemplos**:
```bash
# Capturar tudo na interface eth0
sudo tcpdump -i eth0

# Capturar tráfego de um host específico
sudo tcpdump -i eth0 host 192.168.1.10

# Capturar tráfego de uma porta específica
sudo tcpdump -i eth0 port 80

# Capturar apenas pacotes ICMP (ping)
sudo tcpdump -i eth0 icmp 

# Capturar ARP
sudo tcpdump -i eth0 arp 

# Salvar em arquivo para análise posterior
sudo tcpdump -i eth0 -w captura.pcap
```

### 7.3. Wireshark (Interface Gráfica)

**Descrição**: Versão gráfica do tcpdump, com análise detalhada de pacotes.

**Linux**:
```bash
sudo wireshark
```

**Windows**: Disponível como aplicativo com interface gráfica.

### 7.4. ss – Estatísticas de Socket (Linux)

**Descrição**: Versão moderna e mais rápida que `netstat`.

**Exemplos**:
```bash
# Todas as conexões TCP
ss -t

# Todas as conexões UDP
ss -u

# Escutando portas
ss -lntp

# Estatísticas resumidas
ss -s
```

---

## 8. COMANDOS DE SEGURANÇA E FIREWALL

### 8.1. Gerenciamento de Firewall no Windows

```cmd
# Verificar estado do firewall
netsh advfirewall show allprofiles 

# Ativar firewall
netsh advfirewall set allprofiles state on 

# Desativar firewall (temporariamente)
netsh advfirewall set allprofiles state off 

# Restaurar configurações padrão
netsh advfirewall reset
```

### 8.2. iptables/nftables – Firewall no Linux

**Descrição**: Sistema de filtragem de pacotes no Linux.

**Exemplos básicos com iptables**:
```bash
# Listar regras
sudo iptables -L -n -v

# Permitir conexões estabelecidas
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Permitir SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Bloquear IP específico
sudo iptables -A INPUT -s 192.168.1.100 -j DROP
```

### 8.3. nmap – Scanner de Portas

**Descrição**: Ferramenta para descobrir hosts e serviços em uma rede .

**Exemplos**:
```bash
# Escanear um host específico
nmap 192.168.1.1

# Escanear uma rede
nmap 192.168.1.0/24

# Detectar sistema operacional
nmap -O 192.168.1.1

# Escanear portas específicas
nmap -p 22,80,443 192.168.1.1

# Escaneamento completo
nmap -sS -sV -O 192.168.1.1
```

**Saída típica** :
```
Starting Nmap 7.80 ( https://nmap.org )
Nmap scan report for 192.168.1.1
Host is up (0.0020s latency).
PORT     STATE    SERVICE
22/tcp   open     ssh
80/tcp   open     http
443/tcp  closed   https
MAC Address: 00:11:22:33:44:55 (Router Manufacturer)
```

---

## 9. COMANDOS PARA REDES SEM FIO (WI-FI)

### 9.1. Comandos Wi-Fi no Windows (netsh wlan)

**Descrição**: Conjunto de comandos para gerenciar redes sem fio .

**Exemplos**:

```cmd
# Listar perfis de rede salvos
netsh wlan show profiles 

# Ver senha de uma rede salva
netsh wlan show profile name="NomeDaRede" key=clear 

# Conectar a uma rede
netsh wlan connect name="NomeDaRede" 

# Criar ponto de acesso (hosted network)
netsh wlan set hostednetwork mode=allow ssid=MinhaRede key=Senha123 

# Iniciar ponto de acesso
netsh wlan start hostednetwork 

# Parar ponto de acesso
netsh wlan stop hostednetwork 

# Exibir interfaces wireless
netsh wlan show interfaces
```

### 9.2. Comandos Wi-Fi no Linux

**iwconfig**:
```bash
# Verificar configurações wireless
iwconfig

# Conectar a rede (com wpa_supplicant)
wpa_passphrase "SSID" "senha" | sudo tee /etc/wpa_supplicant.conf
sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant.conf
sudo dhclient wlan0
```

**nmcli** (NetworkManager):
```bash
# Listar redes disponíveis
nmcli dev wifi list

# Conectar a rede
nmcli dev wifi connect "SSID" password "senha"

# Desconectar
nmcli dev disconnect wlan0
```

**iwlist**:
```bash
# Escanear redes disponíveis
sudo iwlist wlan0 scan
```

---

## 10. COMANDOS DE TRANSFERÊNCIA DE ARQUIVOS

### 10.1. ftp – File Transfer Protocol

**Descrição**: Transferência de arquivos entre computadores .

**Exemplo**:
```bash
ftp ftp.br.debian.org
```

**Comandos internos comuns** :
- `ls` - Lista arquivos
- `cd [dir]` - Muda diretório
- `get [arquivo]` - Baixa arquivo
- `put [arquivo]` - Envia arquivo
- `mget [arquivos]` - Baixa múltiplos arquivos
- `hash on` - Mostra progresso
- `quit` - Sai do FTP

### 10.2. scp – Cópia Segura via SSH

**Descrição**: Transfere arquivos de forma segura usando SSH.

**Exemplos**:
```bash
# Copiar arquivo para servidor remoto
scp arquivo.txt usuario@servidor:/home/usuario/

# Copiar diretório recursivamente
scp -r pasta/ usuario@servidor:/home/usuario/

# Copiar do servidor para local
scp usuario@servidor:/home/usuario/arquivo.txt .
```

### 10.3. wget – Download via Terminal

**Descrição**: Ferramenta para baixar arquivos da internet.

**Exemplos**:
```bash
# Baixar arquivo
wget https://exemplo.com/arquivo.zip

# Baixar com nome diferente
wget -O novo_nome.zip https://exemplo.com/arquivo.zip

# Baixar recursivamente
wget -r https://exemplo.com/site/

# Continuar download interrompido
wget -c https://exemplo.com/arquivo.zip
```

### 10.4. curl – Transferência de Dados

**Descrição**: Ferramenta versátil para transferir dados com suporte a vários protocolos.

**Exemplos**:
```bash
# Baixar arquivo
curl -O https://exemplo.com/arquivo.zip

# Ver cabeçalhos HTTP
curl -I https://google.com

# Enviar dados POST
curl -X POST -d "nome=valor" https://api.exemplo.com/endpoint

# Testar latência
curl -w "Tempo total: %{time_total}s\n" -o /dev/null -s https://google.com
```

---

## 11. COMANDOS DE ACESSO REMOTO

### 11.1. ssh – Secure Shell

**Descrição**: Acesso remoto seguro a servidores.

**Exemplos**:
```bash
# Conectar a servidor remoto
ssh usuario@servidor.com

# Conectar em porta específica
ssh -p 2222 usuario@servidor.com

# Executar comando remoto
ssh usuario@servidor.com "ls -la /home"

# Encaminhamento de porta (túnel)
ssh -L 8080:localhost:80 usuario@servidor.com
```

### 11.2. telnet – Acesso Remoto (Inseguro)

**Descrição**: Protocolo antigo para acesso remoto. **Evite usar** sem criptografia .

**Exemplos**:
```bash
# Conectar a servidor
telnet 192.168.1.1 23

# Testar porta específica
telnet smtp.gmail.com 25
```

### 11.3. rdp – Remote Desktop (Windows)

**Windows**:
```cmd
# Abrir cliente RDP
mstsc

# Conectar diretamente
mstsc /v:192.168.1.100
```

### 11.4. who – Usuários Conectados

**Descrição**: Mostra quem está atualmente conectado no sistema .

**Linux**:
```bash
who

# Com cabeçalhos
who -H 

# Mostrar tempo ocioso
who -i 

# Mostrar quem sou eu
whoami 
```

### 11.5. talk – Conversa em Tempo Real

**Descrição**: Permite conversar com outro usuário no sistema .

**Exemplo**:
```bash
talk usuario@host 
```

---

## 12. TABELA DE REFERÊNCIA RÁPIDA

### Diagnóstico e Conectividade

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `ping -c 4 host` | `ping host` | Testar conectividade |
| `traceroute host` | `tracert host` | Rastrear rota |
| `mtr host` | `pathping host` | Diagnóstico combinado |

### Configuração de Interface

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `ip addr show` | `ipconfig /all` | Exibir configurações |
| `ifconfig` | `ipconfig` | Configurações básicas |
| `ethtool eth0` | `netsh interface ...` | Configurar interface |
| `sudo ifup eth0` | `ipconfig /renew` | Ativar/renovar IP |

### Roteamento

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `route -n` | `route print` | Ver tabela roteamento |
| `ip route show` | `route print` | Rotas (moderno) |
| `route add ...` | `route add ...` | Adicionar rota |

### DNS

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `nslookup host` | `nslookup host` | Consulta DNS |
| `dig host` | (instalar BIND) | Consulta detalhada |
| `host host` | `nslookup` | Consulta simples |
| `systemd-resolve --flush-caches` | `ipconfig /flushdns` | Limpar cache DNS |

### Monitoramento

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `netstat -tunap` | `netstat -ano` | Conexões ativas |
| `ss -tlnp` | `netstat -an` | Portas em escuta |
| `sudo tcpdump` | (instalar Wireshark) | Capturar pacotes |
| `nmap host` | `nmap host` | Scanner de portas |

### Wi-Fi

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `nmcli dev wifi list` | `netsh wlan show networks` | Listar redes |
| `nmcli dev wifi connect SSID` | `netsh wlan connect name=SSID` | Conectar |
| `iwconfig` | `netsh wlan show interfaces` | Status Wi-Fi |
| (config manual) | `netsh wlan set hostednetwork` | Criar hotspot |

### Transferência de Arquivos

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `scp origem destino` | `scp` (PowerShell) | Cópia segura |
| `ftp host` | `ftp host` | Transferência FTP |
| `wget URL` | `wget` (instalar) | Download |
| `curl URL` | `curl` (PowerShell) | Transferência dados |

### Acesso Remoto

| Comando (Linux) | Comando (Windows) | Função |
|-----------------|-------------------|--------|
| `ssh user@host` | `ssh user@host` | Terminal remoto |
| `telnet host` | `telnet host` | Acesso inseguro |
| - | `mstsc` | Remote Desktop |
| `who` | `query user` | Usuários conectados |

---

## ✅ CONCLUSÃO

Este manual reúne os comandos essenciais para gerenciamento de redes nos sistemas **Linux** e **Windows**. Dominar essas ferramentas permite:

- **Diagnosticar** problemas de conectividade rapidamente
- **Configurar** interfaces, rotas e serviços de rede
- **Monitorar** tráfego e desempenho
- **Gerenciar** segurança com firewalls
- **Automatizar** tarefas de administração

> 💡 **Dica final**: Use `comando --help` (Linux) ou `comando /?` (Windows) para consultar opções específicas de cada comando. Pratique em ambiente controlado para ganhar confiança!

---

