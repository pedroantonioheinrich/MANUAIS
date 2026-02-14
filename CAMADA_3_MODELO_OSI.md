# 📘 MANUAL COMPLETO DA CAMADA 3 DO MODELO OSI
## *Camada de Rede – A Camada do Roteamento e da Interconexão*

---

## 📌 ÍNDICE

1. [Introdução à Camada 3](#1-introdução-à-camada-3)
2. [O que é a Camada de Rede?](#2-o-que-é-a-camada-de-rede)
3. [Funções Principais da Camada 3](#3-funções-principais-da-camada-3)
4. [Endereçamento Lógico (IP)](#4-endereçamento-lógico-ip)
   - IPv4 vs IPv6
   - Estrutura do Endereço IP
   - Classes de IP (e CIDR)
   - Endereços Especiais
5. [Roteamento](#5-roteamento)
   - Roteamento Estático vs Dinâmico
   - Tabela de Roteamento
   - Métricas e Distância Administrativa
   - Protocolos de Roteamento (RIP, OSPF, BGP, EIGRP)
6. [Protocolo IP](#6-protocolo-ip)
   - Estrutura do Pacote IP (IPv4 e IPv6)
   - Fragmentação e MTU
   - TTL (Time to Live)
7. [Protocolos Auxiliares da Camada 3](#7-protocolos-auxiliares-da-camada-3)
   - ARP (sim, ele fica entre camadas)
   - ICMP (ping, traceroute)
   - IGMP (multicast)
   - NAT (Network Address Translation)
8. [Equipamentos da Camada 3](#8-equipamentos-da-camada-3)
   - Roteador
   - Switch Camada 3
   - Firewall (camada 3 e acima)
9. [Sub-redes e Máscara](#9-sub-redes-e-máscara)
   - Cálculo de Sub-redes
   - VLSM (Variable Length Subnet Mask)
   - CIDR (Classless Inter-Domain Routing)
10. [Exemplo Prático Passo a Passo](#10-exemplo-prático-passo-a-passo)
11. [Resumo Visual](#11-resumo-visual)
12. [Perguntas Frequentes](#12-perguntas-frequentes)
13. [Glossário Detalhado](#13-glossário-detalhado)
14. [Tabelas de Referência Rápida](#14-tabelas-de-referência-rápida)

---

## 1. INTRODUÇÃO À CAMADA 3

A **Camada 3 - Rede** é o coração da comunicação entre redes distintas. Enquanto a Camada 2 cuida da comunicação dentro da mesma rede (entre dispositivos vizinhos), a Camada 3 permite que dados viajem através de múltiplas redes, atravessando continentes e oceanos.

### Posição no Modelo OSI:
```
7 - Aplicação    (Dados do usuário)
6 - Apresentação (Formatação)
5 - Sessão       (Diálogo)
4 - Transporte   (Confiabilidade)
3 - REDE         ← VOCÊ ESTÁ AQUI
2 - Enlace       (Comunicação local)
1 - Física       (Bits e sinais)
```

### Analogia Completa:
Imagine que você está enviando uma encomenda dos Correios:

| Camada | Analogia |
|--------|----------|
| **Camada 1** | O caminhão, a estrada, os trilhos |
| **Camada 2** | O motorista que leva até o próximo centro de distribuição |
| **Camada 3** | O **sistema de roteamento dos Correios** que decide: "Este pacote vai para São Paulo, então deve passar por Curitiba primeiro" |

A Camada 3 não se preocupa com o conteúdo (isso é das camadas superiores), apenas com **como chegar ao destino final**.

---

## 2. O QUE É A CAMADA DE REDE?

A **Camada de Rede** é responsável por:
- **Endereçamento lógico global** (endereços IP)
- **Roteamento** dos pacotes entre redes diferentes
- **Fragmentação e remontagem** de pacotes quando necessário
- **Controle de congestionamento** (opcional)
- **Qualidade de serviço** (QoS) básica

> 🎯 **Objetivo principal**: Entregar pacotes da origem ao destino, mesmo que estejam em redes diferentes, passando por diversos dispositivos intermediários (roteadores).

---

## 3. FUNÇÕES PRINCIPAIS DA CAMADA 3

### 3.1. Endereçamento Lógico
- Atribui endereços únicos e hierárquicos (IP)
- Permite localizar dispositivos globalmente

### 3.2. Roteamento
- Decide o melhor caminho para entregar o pacote
- Mantém e atualiza tabelas de roteamento

### 3.3. Encaminhamento (Forwarding)
- Move o pacote da interface de entrada para a de saída
- Baseado na tabela de roteamento

### 3.4. Fragmentação e Remontagem
- Divide pacotes grandes em fragmentos menores
- Remonta no destino (ou no caminho, dependendo)

### 3.5. Controle de Erros (limitado)
- ICMP reporta erros (rede inalcançável, TTL expirado)
- IP não tem correção de erros (isso é da camada 4)

### 3.6. Qualidade de Serviço (QoS)
- Prioriza certos tipos de tráfego (voz, vídeo)

---

## 4. ENDEREÇAMENTO LÓGICO (IP)

### 4.1. IPv4 vs IPv6

| Característica | IPv4 | IPv6 |
|----------------|------|------|
| **Tamanho** | 32 bits | 128 bits |
| **Quantidade de endereços** | ~4,3 bilhões | 340 undecilhões (3.4×10³⁸) |
| **Formato** | 192.168.1.1 | 2001:0db8:85a3::8a2e:0370:7334 |
| **Notação** | Decimal | Hexadecimal |
| **Configuração** | Manual ou DHCP | Auto-configuração (SLAAC) |
| **Segurança** | Opcional (IPsec) | Nativo (IPsec obrigatório) |
| **Broadcast** | Sim | Não (usa multicast) |

### 4.2. Estrutura do Endereço IPv4

Um endereço IPv4 tem duas partes:
- **Parte da REDE**: Identifica a rede
- **Parte do HOST**: Identifica o dispositivo na rede

```
192.168.1.10/24
│______│ ││
  Rede   Host
```

A **máscara de sub-rede** define onde termina a rede e começa o host.

### 4.3. Classes de IP (Sistema Classful)

Originalmente, o IPv4 foi dividido em classes:

| Classe | Primeiros bits | Faixa | Máscara padrão | Uso |
|--------|----------------|-------|----------------|-----|
| **A** | 0 | 1.0.0.0 a 127.255.255.255 | /8 (255.0.0.0) | Grandes redes |
| **B** | 10 | 128.0.0.0 a 191.255.255.255 | /16 (255.255.0.0) | Redes médias |
| **C** | 110 | 192.0.0.0 a 223.255.255.255 | /24 (255.255.255.0) | Pequenas redes |
| **D** | 1110 | 224.0.0.0 a 239.255.255.255 | - | Multicast |
| **E** | 1111 | 240.0.0.0 a 255.255.255.255 | - | Experimental |

### 4.4. CIDR (Classless Inter-Domain Routing)

Hoje usamos **CIDR**, que substituiu o sistema de classes:
- Permite máscaras de qualquer tamanho (não só /8, /16, /24)
- Formato: 192.168.1.0/26 (máscara 255.255.255.192)

### 4.5. Endereços Especiais

| Endereço | Significado |
|----------|-------------|
| **0.0.0.0/8** | Rota padrão, "qualquer rede" |
| **127.0.0.0/8** | Loopback (localhost) |
| **169.254.0.0/16** | APIPA (quando DHCP falha) |
| **224.0.0.0/4** | Multicast |
| **255.255.255.255** | Broadcast limitado |

### 4.6. Endereços Privados (RFC 1918)

Não roteáveis na internet:

- **Classe A**: 10.0.0.0 a 10.255.255.255 (/8)
- **Classe B**: 172.16.0.0 a 172.31.255.255 (/12)
- **Classe C**: 192.168.0.0 a 192.168.255.255 (/16)

---

## 5. ROTEAMENTO

### 5.1. Conceito Fundamental

Roteamento é o processo de **escolher o melhor caminho** para entregar um pacote.

### 5.2. Tipos de Roteamento

#### 🔷 Roteamento Estático
- Rotas configuradas manualmente pelo administrador
- Vantagens: Simples, seguro, previsível
- Desvantagens: Não se adapta a falhas, não escala

#### 🔶 Roteamento Dinâmico
- Rotas aprendidas automaticamente por protocolos
- Vantagens: Adaptativo, tolerante a falhas
- Desvantagens: Consome recursos, complexo

### 5.3. Tabela de Roteamento

Cada roteador mantém uma tabela como esta:

| Rede de Destino | Máscara | Gateway (Next Hop) | Interface | Métrica |
|-----------------|---------|-------------------|-----------|---------|
| 0.0.0.0 | 0.0.0.0 | 200.100.50.1 | eth0 | 1 |
| 192.168.1.0 | 255.255.255.0 | Diretamente conectado | eth1 | 0 |
| 10.0.0.0 | 255.0.0.0 | 200.100.50.2 | eth0 | 5 |

**Regras de decisão:**
1. Prefere rota mais específica (maior máscara)
2. Se várias, escolhe a de menor métrica
3. Se ainda assim várias, pode fazer balanceamento

### 5.4. Métricas de Roteamento

| Métrica | Descrição | Usado por |
|---------|-----------|-----------|
| **Hop count** | Número de roteadores no caminho | RIP |
| **Custo** | Valor atribuído (pode ser banda, latência) | OSPF |
| **Banda** | Velocidade do link | EIGRP |
| **Atraso** | Tempo de propagação | EIGRP |
| **Confiabilidade** | Taxa de erros | EIGRP |
| **Carga** | Utilização do link | EIGRP |

### 5.5. Distância Administrativa (AD)

Prioridade entre fontes de roteamento (menor valor = mais confiável):

| Fonte | AD |
|-------|-----|
| Interface diretamente conectada | 0 |
| Rota estática | 1 |
| EIGRP (resumo) | 5 |
| BGP externo (eBGP) | 20 |
| EIGRP interno | 90 |
| OSPF | 110 |
| RIP | 120 |
| BGP interno (iBGP) | 200 |
| Desconhecido | 255 |

### 5.6. Protocolos de Roteamento

#### 📍 RIP (Routing Information Protocol)
- **Tipo**: Vetor de distância
- **Métrica**: Hop count
- **Limite**: 15 hops (16 = inalcançável)
- **Versões**: RIPv1 (classful), RIPv2 (CIDR)
- **Atualizações**: A cada 30 segundos
- **Convergência**: Lenta

#### 📍 OSPF (Open Shortest Path First)
- **Tipo**: Estado de enlace
- **Métrica**: Custo (banda)
- **Algoritmo**: Dijkstra (SPF)
- **Vantagens**: Rápida convergência, sem limite de hops
- **Áreas**: Permite hierarquia (backbone área 0)
- **Usado em**: Grandes redes empresariais

#### 📍 EIGRP (Enhanced Interior Gateway Routing Protocol)
- **Tipo**: Híbrido (Cisco proprietário)
- **Métrica**: Banda, atraso, carga, confiabilidade
- **Algoritmo**: DUAL
- **Vantagens**: Convergência muito rápida
- **Usado em**: Redes Cisco

#### 📍 BGP (Border Gateway Protocol)
- **Tipo**: Vetor de caminho
- **Usado em**: Internet (entre ISPs, grandes empresas)
- **Métrica**: Atributos complexos (AS_PATH, LOCAL_PREF)
- **Características**: Muito escalável, política de roteamento
- **AS (Autonomous System)**: Identificador único (16 ou 32 bits)

---

## 6. PROTOCOLO IP

### 6.1. Estrutura do Pacote IPv4

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Versão|  IHL  |Tipo de Serviço|       Comprimento Total        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Identificação           |Flags|   Fragment Offset       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Tempo de Vida |  Protocolo   |      Checksum do Cabeçalho    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Endereço de Origem                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Endereço de Destino                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Opções (se IHL > 5)                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            Dados                                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Detalhamento dos Campos IPv4:

| Campo | Tamanho | Descrição |
|-------|---------|-----------|
| **Versão** | 4 bits | IPv4 = 4, IPv6 = 6 |
| **IHL** | 4 bits | Tamanho do cabeçalho (em palavras de 32 bits) |
| **Tipo de Serviço** | 8 bits | QoS, precedência |
| **Comprimento Total** | 16 bits | Pacote inteiro (até 65535 bytes) |
| **Identificação** | 16 bits | Para fragmentação |
| **Flags** | 3 bits | DF (Don't Fragment), MF (More Fragments) |
| **Fragment Offset** | 13 bits | Posição do fragmento |
| **TTL** | 8 bits | Limita vida útil do pacote |
| **Protocolo** | 8 bits | TCP(6), UDP(17), ICMP(1) |
| **Checksum** | 16 bits | Verifica cabeçalho apenas |
| **Origem** | 32 bits | IP de quem enviou |
| **Destino** | 32 bits | IP de quem vai receber |
| **Opções** | Variável | Roteamento fonte, timestamp, etc. |

### 6.2. Estrutura do Pacote IPv6

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Versão| Classe de Tráfego |           Flow Label                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                         Endereço de Origem                     +
|                          (128 bits)                            |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                        Endereço de Destino                     +
|                          (128 bits)                            |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### 6.3. Fragmentação e MTU

**MTU (Maximum Transmission Unit)**: Tamanho máximo do pacote que pode ser transmitido em um link.

- **Ethernet**: MTU = 1500 bytes
- **PPP**: MTU = 1492 bytes
- **FDDI**: MTU = 4352 bytes
- **Jumbo Frames**: até 9000 bytes

Quando um pacote é maior que o MTU do link:
- Se DF=0: Fragmenta
- Se DF=1: Descarta e envia ICMP "Fragmentation Needed"

### 6.4. TTL (Time to Live)

- Valor inicial: 64, 128, 255 (depende do SO)
- Cada roteador decrementa em 1
- Se chegar a 0: Pacote descartado, ICMP "Time Exceeded" enviado
- **Função**: Evitar loops infinitos

---

## 7. PROTOCOLOS AUXILIARES DA CAMADA 3

### 7.1. ARP (Address Resolution Protocol)

**Importante:** ARP fica entre Camada 2 e 3.

**Função**: Traduz IP → MAC

**Como funciona:**
1. Dispositivo A quer saber MAC do IP 192.168.1.2
2. Envia broadcast ARP: "Quem tem IP 192.168.1.2?"
3. Dispositivo B responde: "Sou eu, meu MAC é BB:BB:BB:BB:BB:BB"
4. A guarda em cache ARP

**ARP Cache:** Tabela temporária:
```
Endereço IP      Endereço MAC       Tipo    Idade
192.168.1.1      AA:AA:AA:AA:AA:AA  dinâmico 120s
192.168.1.2      BB:BB:BB:BB:BB:BB  dinâmico 110s
```

### 7.2. ICMP (Internet Control Message Protocol)

Protocolo para mensagens de controle e erro.

#### Tipos principais:

| Tipo | Código | Significado | Uso |
|------|--------|-------------|-----|
| 0 | 0 | Echo Reply | Resposta do ping |
| 3 | 0 | Network Unreachable | Rede não encontrada |
| 3 | 1 | Host Unreachable | Host não encontrado |
| 3 | 3 | Port Unreachable | Porta não disponível |
| 8 | 0 | Echo Request | Ping |
| 11 | 0 | TTL Expired | Traceroute |
| 5 | 1 | Redirect | Rota melhor existe |

#### Ferramentas que usam ICMP:
- **ping**: Testa conectividade
- **traceroute / tracert**: Mapeia caminho

### 7.3. IGMP (Internet Group Management Protocol)

Gerencia grupos **multicast**:
- Hosts informam roteadores que querem receber tráfego multicast
- Roteadores mantêm lista de membros por grupo

### 7.4. NAT (Network Address Translation)

Traduz endereços privados em públicos.

**Tipos:**
- **SNAT (Source NAT)**: Traduz origem (típico de roteadores residenciais)
- **DNAT (Destination NAT)**: Traduz destino (redireciona portas)
- **PAT (Port Address Translation)**: Múltiplos IPs privados compartilham um IP público com portas diferentes

**Funcionamento básico:**
```
PC interno (192.168.1.10:54321) → Roteador → Internet (200.100.50.1:12345)
```

---

## 8. EQUIPAMENTOS DA CAMADA 3

### 8.1. Roteador (Router)
- Dispositivo principal da camada 3
- Conecta redes diferentes
- Toma decisões baseadas em endereços IP
- Mantém tabela de roteamento
- Executa protocolos de roteamento

### 8.2. Switch Camada 3 (Multilayer Switch)
- Híbrido: switch (camada 2) + roteador (camada 3)
- Roteia entre VLANs sem sair do equipamento
- Muito usado em redes empresariais

### 8.3. Firewall
- Filtra tráfego baseado em IP, porta, protocolo
- Pode atuar em camadas 3, 4 e 7

### 8.4. Comparação:

| Equipamento | Camada | Unidade de dados | Decisão baseada em |
|-------------|--------|------------------|---------------------|
| Hub | 1 | Bit | Nenhuma |
| Switch | 2 | Quadro (Frame) | Endereço MAC |
| Roteador | 3 | Pacote | Endereço IP |
| Switch Layer 3 | 2 e 3 | Quadro/Pacote | MAC e IP |

---

## 9. SUB-REDES E MÁSCARA

### 9.1. Conceito de Sub-rede

Dividir uma rede grande em redes menores:
- Melhora desempenho
- Aumenta segurança
- Facilita gerenciamento

### 9.2. Cálculo de Sub-redes

**Exemplo:** Rede 192.168.1.0/24 (256 endereços)

Queremos 4 sub-redes:
- Precisamos de 2 bits extras de empréstimo (2² = 4)
- Nova máscara: /26 (255.255.255.192)

**Resultado:**
- Sub-rede 1: 192.168.1.0 a 192.168.1.63
- Sub-rede 2: 192.168.1.64 a 192.168.1.127
- Sub-rede 3: 192.168.1.128 a 192.168.1.191
- Sub-rede 4: 192.168.1.192 a 192.168.1.255

### 9.3. Fórmulas Importantes

```
Número de sub-redes = 2^(bits emprestados)
Número de hosts por sub-rede = 2^(bits restantes) - 2
Endereço de rede = IP AND Máscara
Endereço de broadcast = IP OR (NOT Máscara)
```

### 9.4. VLSM (Variable Length Subnet Mask)

Permite máscaras diferentes dentro da mesma rede:
- Otimiza uso de endereços
- Sub-redes de tamanhos variados conforme necessidade

### 9.5. CIDR (Classless Inter-Domain Routing)

Notação: IP/máscara
Exemplo: 192.168.1.0/27 (32 endereços, 30 utilizáveis)

**Tabela CIDR rápida:**

| Máscara | CIDR | Endereços | Hosts úteis |
|---------|------|-----------|-------------|
| 255.0.0.0 | /8 | 16.777.216 | 16.777.214 |
| 255.255.0.0 | /16 | 65.536 | 65.534 |
| 255.255.255.0 | /24 | 256 | 254 |
| 255.255.255.128 | /25 | 128 | 126 |
| 255.255.255.192 | /26 | 64 | 62 |
| 255.255.255.224 | /27 | 32 | 30 |
| 255.255.255.240 | /28 | 16 | 14 |
| 255.255.255.248 | /29 | 8 | 6 |
| 255.255.255.252 | /30 | 4 | 2 |

---

## 10. EXEMPLO PRÁTICO PASSO A PASSO

### Cenário Completo: Computador A acessa um site na internet

```
[PC A] --- [Switch] --- [Roteador Residencial] --- [Internet] --- [Servidor Web]
192.168.1.10         200.100.50.1 (WAN)                     200.100.100.5
        192.168.1.1 (LAN)
```

### Passo a Passo:

**1. Camada 4 (Transporte)** no PC A:
- Aplicação quer acessar site
- Cria socket: 192.168.1.10:54321 → 200.100.100.5:80

**2. Camada 3 no PC A**:
- Verifica que destino (200.100.100.5) não está na rede local
- Consulta tabela de roteamento
- Encontra rota padrão (gateway) = 192.168.1.1
- Prepara pacote IP:
  - Origem: 192.168.1.10
  - Destino: 200.100.100.5
  - TTL: 128
  - Protocolo: TCP

**3. Camada 2 no PC A**:
- Precisa do MAC do gateway (192.168.1.1)
- Consulta cache ARP (se não tem, faz ARP)
- Monta quadro Ethernet:
  - MAC Origem: MAC do PC A
  - MAC Destino: MAC do gateway
  - Dados: Pacote IP

**4. Camada 1**:
- Converte em bits
- Transmite pelo cabo

**5. Roteador recebe**:
- Camada 1: Recebe bits
- Camada 2: Verifica MAC destino (é dele?)
- Camada 3: 
  - Extrai pacote IP
  - Decrementa TTL (127)
  - Consulta tabela de roteamento
  - Encontra rota para 200.100.100.0/24 via interface WAN
  - NAT: Traduz origem 192.168.1.10:54321 → 200.100.50.1:12345
  - Novo pacote IP: Origem 200.100.50.1, Destino 200.100.100.5

**6. Roteador encaminha**:
- Camada 2 (interface WAN): Quadro com MAC do próximo hop
- Camada 1: Transmite

**7. Internet**:
- Vários roteadores repetem processo
- Cada um decrementa TTL, consulta tabela

**8. Servidor Web recebe**:
- Camada 1,2,3 processam
- Passa para camada 4 (TCP)
- Responde com pacote:
  - Origem: 200.100.100.5
  - Destino: 200.100.50.1 (IP público do roteador)

**9. Roteador recebe resposta**:
- Consulta tabela NAT
- Traduz destino de volta: 200.100.50.1:12345 → 192.168.1.10:54321

**10. PC A recebe resposta**:
- Processa camadas 1→4
- Entrega à aplicação

---

## 11. RESUMO VISUAL

```
┌─────────────────────────────────────────────────┐
│            CAMADA 4 - TRANSPORTE                 │
│         (TCP/UDP - Portas, Confiabilidade)       │
└─────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────┐
│     🌐 CAMADA 3 - REDE (VOCÊ ESTÁ AQUI)          │
│                                                   │
│  ┌─────────────────────────────────────────────┐ │
│  │  FUNÇÕES PRINCIPAIS:                         │ │
│  │  • Endereçamento IP                           │ │
│  │  • Roteamento                                 │ │
│  │  • Fragmentação                               │ │
│  │  • TTL                                        │ │
│  └─────────────────────────────────────────────┘ │
│                                                   │
│  ┌─────────────────────────────────────────────┐ │
│  │  PROTOCOLOS:                                 │ │
│  │  • IP (Internet Protocol)                    │ │
│  │  • ICMP (Ping, Traceroute)                   │ │
│  │  • ARP* (IP → MAC)                           │ │
│  │  • IGMP (Multicast)                          │ │
│  └─────────────────────────────────────────────┘ │
│                                                   │
│  ┌─────────────────────────────────────────────┐ │
│  │  EQUIPAMENTOS:                               │ │
│  │  • Roteador                                  │ │
│  │  • Switch Camada 3                           │ │
│  │  • Firewall                                   │ │
│  └─────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────┐
│            CAMADA 2 - ENLACE                      │
│     (MAC, Quadros, Switches, ARP*)                │
└─────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────┐
│            CAMADA 1 - FÍSICA                       │
│          (Bits, Cabos, Sinais)                    │
└─────────────────────────────────────────────────┘
```

---

## 12. PERGUNTAS FREQUENTES

### ❓ Qual a diferença entre IP público e privado?
**IP público**: Roteável na internet, único globalmente.  
**IP privado**: Usado em redes locais, não roteável na internet (precisa de NAT).

### ❓ O que é gateway padrão?
É o roteador que permite sair da rede local para outras redes. Se não tiver gateway configurado, o dispositivo só consegue falar com quem está na mesma rede.

### ❓ Como funciona o traceroute?
Envia pacotes com TTL=1, TTL=2, TTL=3... Cada roteador que decrementa TTL para 0 responde com ICMP "Time Exceeded". Assim mapeia todo o caminho.

### ❓ O que é loop de roteamento?
Quando pacotes ficam presos entre roteadores devido a tabelas inconsistentes. TTL evita que fiquem eternamente.

### ❓ Por que o IPv6 ainda não substituiu o IPv4 completamente?
Custo de migração, equipamentos antigos, e NAT "segurando" a escassez de IPv4.

### ❓ O que significa "rede inalcançável" no ping?
O roteador não tem rota para a rede de destino. Pode ser configuração errada ou destino offline.

### ❓ Roteador e modem são a mesma coisa?
Não. Modem converte sinal (ex.: cabo, fibra, ADSL). Roteador faz roteamento. Muitos aparelhos residenciais são "roteadores com modem integrado".

---

## 13. GLOSSÁRIO DETALHADO

| Termo | Significado |
|-------|-------------|
| **IP** | Internet Protocol |
| **IPv4** | Versão 4 do IP (32 bits) |
| **IPv6** | Versão 6 do IP (128 bits) |
| **Roteamento** | Processo de escolher caminhos para pacotes |
| **Tabela de roteamento** | Base de dados com rotas conhecidas |
| **Gateway** | Roteador que dá acesso a outras redes |
| **Métrica** | Valor usado para comparar rotas |
| **Distância Administrativa** | Confiabilidade da fonte da rota |
| **AS** | Autonomous System (sistema autônomo) |
| **BGP** | Border Gateway Protocol (protocolo da internet) |
| **OSPF** | Open Shortest Path First |
| **RIP** | Routing Information Protocol |
| **EIGRP** | Enhanced Interior Gateway Routing Protocol |
| **CIDR** | Classless Inter-Domain Routing |
| **VLSM** | Variable Length Subnet Mask |
| **NAT** | Network Address Translation |
| **PAT** | Port Address Translation |
| **ICMP** | Internet Control Message Protocol |
| **ARP** | Address Resolution Protocol |
| **TTL** | Time to Live |
| **MTU** | Maximum Transmission Unit |
| **QoS** | Quality of Service |
| **Multicast** | Entrega para múltiplos destinos |
| **Unicast** | Entrega para um destino |
| **Broadcast** | Entrega para todos na rede |

---

## 14. TABELAS DE REFERÊNCIA RÁPIDA

### Tabela de Máscaras CIDR

| CIDR | Máscara | Hosts | Uso típico |
|------|---------|-------|------------|
| /30 | 255.255.255.252 | 2 | Links ponto a ponto |
| /29 | 255.255.255.248 | 6 | Pequenas redes |
| /28 | 255.255.255.240 | 14 | Redes pequenas |
| /27 | 255.255.255.224 | 30 | Redes médias |
| /26 | 255.255.255.192 | 62 | Redes médias |
| /25 | 255.255.255.128 | 126 | Redes médias |
| /24 | 255.255.255.0 | 254 | Rede local típica |
| /23 | 255.255.254.0 | 510 | Agrega 2 redes /24 |
| /22 | 255.255.252.0 | 1022 | Agrega 4 redes /24 |
| /21 | 255.255.248.0 | 2046 | Agrega 8 redes /24 |
| /20 | 255.255.240.0 | 4094 | Agrega 16 redes /24 |

### Tabela de Protocolos e Portas (Camada 4, mas referência)

| Protocolo | Número | Uso |
|-----------|--------|-----|
| ICMP | 1 | Ping, traceroute |
| TCP | 6 | Conexão confiável |
| UDP | 17 | Conexão rápida |
| GRE | 47 | Túneis |
| ESP | 50 | IPsec |
| AH | 51 | IPsec |
| OSPF | 89 | Roteamento |

---

## ✅ CONCLUSÃO

A **Camada 3 - Rede** é a camada que torna a internet possível:
- **Endereça** dispositivos globalmente (IP)
- **Roteia** pacotes através de múltiplas redes
- **Conecta** redes diferentes (LANs, WANs, internet)
- **Gerencia** o tráfego com TTL, fragmentação, QoS

Ela é a ponte entre sua rede local e o mundo, permitindo que um computador no Brasil se comunique com um servidor no Japão através de dezenas de roteadores intermediários.

---
