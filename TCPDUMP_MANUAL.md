---

# 📘 Manual Completo de TCPDUMP (Linux)
**Da Captura Básica à Análise Forense de Pacotes**

---

## 1. 🚀 Introdução: O que é TCPDUMP?

**TCPDUMP** é uma ferramenta de linha de comando poderosa para análise de tráfego de rede em tempo real . Ela permite capturar e inspecionar pacotes que trafegam pela interface de rede, sendo indispensável para administradores de rede, profissionais de segurança e qualquer pessoa que precise entender o comportamento da rede .

### Por que aprender TCPDUMP?
- **Diagnosticar problemas**: Identificar lentidão, conexões falhando ou serviços com mau comportamento 
- **Monitorar segurança**: Detectar varreduras de porta, ataques DDoS ou comunicações suspeitas 
- **Auditoria de rede**: Verificar exatamente o que está trafegando na rede 
- **Análise forense**: Investigar incidentes de segurança através de capturas salvas 

### Como funciona?
O TCPDUMP utiliza a biblioteca **libpcap** para capturar pacotes da interface de rede, que é colocada em **modo promíscuo** para "escutar" todo o tráfego que passa por ela . Ele então analisa os pacotes usando suas definições internas de protocolo e exibe as informações ao usuário .

---

## 2. 🏁 Primeiros Passos

### 2.1 Instalação

Verifique se já está instalado:
```bash
which tcpdump
# /usr/sbin/tcpdump
```

Se não estiver instalado:

**Ubuntu/Debian:**
```bash
sudo apt install tcpdump
```

**Red Hat/CentOS:**
```bash
sudo yum install tcpdump
# ou
sudo dnf install tcpdump
```

**macOS:**
```bash
brew install tcpdump
```


### 2.2 Sintaxe Básica
```bash
tcpdump [opções] [expressão]
```
- **opções**: Modificam o comportamento (interface, formato de saída, etc.)
- **expressão**: Define filtros (host, porta, protocolo) 

### 2.3 Verificando Interfaces Disponíveis
```bash
sudo tcpdump -D
```
Mostra todas as interfaces onde você pode capturar tráfego, indicando se estão ativas (Up, Running) e se são loopback .

Exemplo de saída:
```
1.eth0 [Up, Running]
2.wlan0 [Up, Running]
3.lo [Up, Running, Loopback]
4.any (Pseudo-device that captures on all interfaces)
```

### 2.4 Primeira Captura
```bash
sudo tcpdump
```
⚠️ **Atenção**: Isso pode inundar sua tela com centenas de pacotes! Pressione **Ctrl+C** para parar .

### 2.5 Entendendo a Saída
```
16:35:15.957001 IP 192.168.1.100.1008 > 225.255.255.250.1008: UDP, length 241
```

| Campo | Significado |
|-------|-------------|
| `16:35:15.957001` | Timestamp (horário da captura)  |
| `IP` | Protocolo |
| `192.168.1.100.1008` | IP e porta de origem |
| `>` | Direção do fluxo |
| `225.255.255.250.1008` | IP e porta de destino |
| `UDP` | Protocolo de transporte |
| `length 241` | Tamanho do pacote |

---

## 3. 📡 Capturando Tráfego Específico

### 3.1 Selecionando Interface
```bash
# Capturar em interface específica
sudo tcpdump -i eth0

# Capturar em todas interfaces
sudo tcpdump -i any
```
[i]

### 3.2 Limitando Número de Pacotes
```bash
# Capturar apenas 10 pacotes e parar
sudo tcpdump -c 10 -i eth0
```
Útil para testes rápidos sem sobrecarregar o terminal .

### 3.3 Desabilitando Resolução de Nomes
```bash
# Não resolver nomes de host
sudo tcpdump -n

# Não resolver nomes de host nem portas
sudo tcpdump -nn
```
Isso acelera a captura e evita consultas DNS desnecessárias .

### 3.4 Controlando o Tamanho da Captura
```bash
# Capturar apenas cabeçalhos (pacotes menores)
sudo tcpdump -s 64

# Capturar pacotes completos (padrão moderno)
sudo tcpdump -s 0
```
`-s0` captura o pacote inteiro, essencial para extrair dados ou arquivos da rede .

---

## 4. 🎯 Filtros Essenciais

### 4.1 Por Host
```bash
# Tráfego para/from um IP específico
sudo tcpdump host 192.168.1.100

# Apenas tráfego de origem
sudo tcpdump src host 192.168.1.100

# Apenas tráfego de destino
sudo tcpdump dst host 192.168.1.100
```


### 4.2 Por Porta
```bash
# Tráfego em porta específica
sudo tcpdump port 80           # HTTP
sudo tcpdump port 443          # HTTPS
sudo tcpdump port 53           # DNS
sudo tcpdump port 22           # SSH

# Origem ou destino específico
sudo tcpdump src port 80
sudo tcpdump dst port 443
```


### 4.3 Por Protocolo
```bash
# Apenas TCP
sudo tcpdump tcp

# Apenas UDP
sudo tcpdump udp

# Apenas ICMP (ping)
sudo tcpdump icmp
```


### 4.4 Por Rede
```bash
# Toda a sub-rede
sudo tcpdump net 192.168.1.0/24
sudo tcpdump src net 10.0.0.0/8
sudo tcpdump dst net 172.16.0.0/12
```


---

## 5. 🔗 Combinando Filtros (Lógica Booleana)

### 5.1 Operadores
- **and** ou **&&** : E lógico
- **or** ou **||** : OU lógico
- **not** ou **!** : Negação 

### 5.2 Exemplos Práticos
```bash
# Host específico na porta 80
sudo tcpdump host 192.168.1.100 and port 80

# Tráfego HTTP ou HTTPS
sudo tcpdump port 80 or port 443

# Tráfego TCP de um host, excluindo SSH
sudo tcpdump tcp and host 10.0.0.5 and not port 22

# Combinação complexa (use parênteses)
sudo tcpdump 'host 192.168.1.100 and (port 80 or port 443)'
```


---

## 6. 💾 Salvando e Lendo Capturas

### 6.1 Salvando em Arquivo
```bash
# Salvar captura em arquivo .pcap
sudo tcpdump -i eth0 -w captura.pcap

# Salvar com filtro e limite
sudo tcpdump -i eth0 host 192.168.1.100 -c 100 -w http.pcap

# Salvar com verbose para ver estatísticas
sudo tcpdump -i eth0 -w dns.pcap -v port 53
```


⚠️ **Importante**: Arquivos .pcap não são texto puro! Não use `cat` para lê-los .

### 6.2 Lendo Arquivos Salvos
```bash
# Ler arquivo de captura
sudo tcpdump -r captura.pcap

# Ler com filtros (re-analisar)
sudo tcpdump -r captura.pcap port 443

# Ler com formato legível
sudo tcpdump -r captura.pcap -nn
```


### 6.3 Integração com Wireshark
Arquivos .pcap podem ser abertos no Wireshark para análise visual avançada:
- Abra o Wireshark
- File > Open > selecione o arquivo .pcap
- Use as ferramentas gráficas para seguir streams, gráficos, etc. 

---

## 7. 🔍 Análise Avançada

### 7.1 Verbosidade
```bash
# Mais detalhes
sudo tcpdump -v

# Ainda mais detalhes
sudo tcpdump -vv

# Máximo de detalhes
sudo tcpdump -vvv
```
Mostra informações adicionais como TTL, opções de IP, etc. 

### 7.2 Visualizando Conteúdo (ASCII e HEX)

**Mostrar conteúdo ASCII:**
```bash
# Ver dados em texto legível (útil para HTTP)
sudo tcpdump -A port 80
```


**Mostrar HEX e ASCII:**
```bash
# Formato hexadecimal + ASCII
sudo tcpdump -X port 80
```
Útil para analisar protocolos binários .

### 7.3 Captura em Tempo Real com Filtros (Line Buffered)
```bash
# Capturar e filtrar com grep em tempo real
sudo tcpdump -i eth0 -l port 80 | grep "User-Agent:"
```
A opção `-l` força saída line-buffered, essencial para pipes funcionarem em tempo real .

### 7.4 Extraindo Informações Específicas

**User-Agents de HTTP:**
```bash
sudo tcpdump -nn -A -s0 -l port 80 | grep "User-Agent:"
```


**Requisições HTTP (GET/POST):**
```bash
# URLs e Hosts
sudo tcpdump -s0 -v -n -l | egrep -i "POST /|GET /|Host:"
```


**Senhas em POST (uso educacional/lícito apenas!):**
```bash
sudo tcpdump -s0 -A -n -l | egrep -i "POST /|pwd=|passwd=|password=|Host:"
```
⚠️ **Aviso ético**: Use apenas em redes próprias ou com autorização explícita! 

**Cookies:**
```bash
sudo tcpdump -nn -A -s0 -l | egrep -i 'Set-Cookie|Host:|Cookie:'
```


### 7.5 Filtros Avançados por Flags TCP

```bash
# Pacotes SYN (início de conexão)
sudo tcpdump 'tcp[tcpflags] & tcp-syn != 0'

# Pacotes RST (conexão rejeitada)
sudo tcpdump 'tcp[tcpflags] & tcp-rst != 0'

# Pacotes FIN (fim de conexão)
sudo tcpdump 'tcp[tcpflags] & tcp-fin != 0'

# SYN-ACK (resposta a SYN)
sudo tcpdump 'tcp[tcpflags] = 0x12'
```


### 7.6 Captura de Tráfego Fragmentado
Quando pacotes são fragmentados devido a diferenças de MTU, o TCPDUMP pode mostrar os fragmentos individualmente .

```bash
# Ver fragmentos IP
sudo tcpdump 'ip[6:2] & 0x1fff != 0'
```

---

## 8. 📋 Casos de Uso Práticos

### 8.1 Diagnosticando DNS
```bash
# Capturar consultas DNS
sudo tcpdump -i eth0 -nn port 53

# Ver apenas consultas de um domínio
sudo tcpdump -nn port 53 -A | grep "example.com"
```


### 8.2 Monitorando HTTP/HTTPS
```bash
# Todo tráfego web
sudo tcpdump -i eth0 port 80 or port 443

# Apenas requisições HTTP (texto)
sudo tcpdump -A -s0 port 80
```

### 8.3 Detectando Ping Flood (ICMP)
```bash
# Ver todos os pings
sudo tcpdump -nn icmp

# Ver apenas pings anormais (não echo/reply)
sudo tcpdump 'icmp[icmptype] != icmp-echo and icmp[icmptype] != icmp-echoreply'
```


### 8.4 Verificando Conexões SSH
```bash
# Monitorar tentativas de SSH
sudo tcpdump port 22

# Ver handshakes TCP (SYN)
sudo tcpdump 'tcp[tcpflags] & tcp-syn != 0 and port 22'
```

### 8.5 Capturando Tráfego de um Aplicativo Específico
Se você sabe que um aplicativo usa porta fixa, filtre por ela:
```bash
sudo tcpdump port 3306   # MySQL
sudo tcpdump port 5432   # PostgreSQL
sudo tcpdump port 6379   # Redis
```

---

## 9. ⚠️ Efeitos Colaterais e Boas Práticas

### 9.1 Impacto no Sistema

| Efeito | Descrição | Mitigação |
|--------|-----------|-----------|
| **Alto consumo de CPU** | Capturar em interfaces muito ativas pode sobrecarregar o processador | Use filtros específicos, capture apenas o necessário |
| **Arquivos enormes** | Capturas longas geram arquivos gigantes | Use `-c` para limitar pacotes ou `-C` para rotacionar arquivos |
| **Privacidade** | Você pode capturar dados sensíveis (senhas, emails) | Use apenas em redes autorizadas, respeite leis locais |
| **Modo promíscuo** | Interface captura todo tráfego do segmento, não só o seu | Requer root, use com responsabilidade  |

### 9.2 Boas Práticas

1. **Sempre use filtros** - Não capture tudo, capture o necessário
2. **Use `-n` para evitar resolução DNS** - Mais rápido e seguro
3. **Salve em .pcap para análise posterior** - Não tente analisar tudo em tempo real
4. **Teste em laboratório primeiro** - Evite problemas em produção
5. **Documente suas capturas** - Anote data, hora, motivo e filtros usados
6. **Respeite a privacidade** - Dados de usuários podem estar nos pacotes

### 9.3 O que TCPDUMP NÃO Faz
- ❌ **Não bloqueia tráfego** - Apenas observa 
- ❌ **Não descriptografa** - Tráfego HTTPS/TLS permanece cifrado 
- ❌ **Não substitui IDS/IPS** - Para isso use Snort, Suricata, etc. 
- ❌ **Não captura em switches roteados** - Apenas vê o tráfego da própria interface

---

## 10. 🎯 Projeto Prático: Monitor de Rede Simples

Crie um script `monitor_rede.sh` para diagnóstico rápido:

```bash
#!/bin/bash
# Script de diagnóstico rápido com tcpdump

INTERFACE="eth0"
DURACAO=30
ARQUIVO="captura_$(date +%Y%m%d_%H%M%S).pcap"

echo "📊 Iniciando diagnóstico de rede em $INTERFACE por $DURACAO segundos"
echo "🔍 Interfaces disponíveis:"
sudo tcpdump -D

echo -e "\n📝 Capturando amostra de tráfego em $ARQUIVO..."
sudo tcpdump -i $INTERFACE -c 1000 -w $ARQUIVO -G $DURACAO -W 1

echo -e "\n📈 Resumo da captura:"
sudo tcpdump -r $ARQUIVO -nn | cut -d ' ' -f 3 | cut -d '.' -f 1-4 | sort | uniq -c | sort -nr | head -10

echo -e "\n🔌 Top 5 portas de destino:"
sudo tcpdump -r $ARQUIVO -nn | grep -o "dst port [0-9]*" | sort | uniq -c | sort -nr | head -5

echo -e "\n📊 Estatísticas de protocolo:"
echo "TCP: $(sudo tcpdump -r $ARQUIVO tcp 2>/dev/null | wc -l) pacotes"
echo "UDP: $(sudo tcpdump -r $ARQUIVO udp 2>/dev/null | wc -l) pacotes"
echo "ICMP: $(sudo tcpdump -r $ARQUIVO icmp 2>/dev/null | wc -l) pacotes"

echo -e "\n✅ Diagnóstico concluído. Arquivo salvo: $ARQUIVO"
```

---
