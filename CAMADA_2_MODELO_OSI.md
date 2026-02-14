# 📘 MANUAL COMPLETO DA CAMADA 2 DO MODELO OSI
## *Camada de Enlace de Dados (Data Link) – A Camada da Confiabilidade*

---

## 📌 ÍNDICE

1. [Introdução à Camada 2](#1-introdução-à-camada-2)
2. [O que é a Camada de Enlace?](#2-o-que-é-a-camada-de-enlace)
3. [Subcamadas da Camada 2](#3-subcamadas-da-camada-2)
   - LLC (Logical Link Control)
   - MAC (Media Access Control)
4. [Funções Principais](#4-funções-principais)
5. [Endereçamento MAC](#5-endereçamento-mac)
6. [Métodos de Controle de Acesso ao Meio](#6-métodos-de-controle-de-acesso-ao-meio)
7. [Estrutura do Quadro (Frame)](#7-estrutura-do-quadro-frame)
8. [Detecção e Correção de Erros](#8-detecção-e-correção-de-erros)
9. [Protocolos da Camada 2](#9-protocolos-da-camada-2)
10. [Equipamentos da Camada 2](#10-equipamentos-da-camada-2)
11. [Exemplo Prático](#11-exemplo-prático)
12. [Resumo Visual](#12-resumo-visual)
13. [Perguntas Frequentes](#13-perguntas-frequentes)
14. [Glossário](#14-glossário)

---

## 1. INTRODUÇÃO À CAMADA 2

A **Camada 2 - Enlace de Dados** é a segunda camada do modelo OSI, situada entre a **Camada Física** (bits) e a **Camada de Rede** (pacotes).

### Posição no Modelo OSI:
```
7 - Aplicação
6 - Apresentação
5 - Sessão
4 - Transporte
3 - Rede
2 - ENLACE ← Você está aqui
1 - Física
```

### Analogia:
Imagine que você está enviando um livro pelos correios:
- **Camada 1 (Física)**: É o caminhão e a estrada
- **Camada 2 (Enlace)**: É o **motorista** que:
  - Sabe dirigir até a próxima cidade
  - Verifica se o pacote chegou inteiro
  - Tem o endereço da próxima parada

---

## 2. O QUE É A CAMADA DE ENLACE?

A **Camada de Enlace** é responsável por:
- Organizar os bits recebidos da camada física em **quadros (frames)**
- Garantir a transmissão livre de erros entre dois dispositivos **diretamente conectados**
- Controlar o fluxo de dados
- Gerenciar o acesso ao meio físico compartilhado

> 🎯 **Objetivo principal**: Transformar um meio de transmissão bruto em uma linha confiável para a camada de rede.

---

## 3. SUBCAMADAS DA CAMADA 2

A Camada 2 é dividida em duas subcamadas pelo padrão IEEE 802:

### 3.1. LLC (Logical Link Control) - Controle de Enlace Lógico
- Interface com a camada de rede (Camada 3)
- Multiplexação de protocolos
- Controle de fluxo e erro (opcional)

### 3.2. MAC (Media Access Control) - Controle de Acesso ao Meio
- Endereçamento físico (endereços MAC)
- Controle de acesso ao meio (quem pode transmitir)
- Delimitação de quadros
- Detecção de erros

```
+---------------------+
|   CAMADA 3 - REDE   |
+---------------------+
|  LLC (Logical Link) |
+---------------------+
|  MAC (Media Access) |
+---------------------+
|  CAMADA 1 - FÍSICA  |
+---------------------+
```

---

## 4. FUNÇÕES PRINCIPAIS

### 4.1. Enquadramento (Framing)
- Divide o fluxo de bits em unidades chamadas **quadros**
- Adiciona cabeçalho e rodapé para controle

### 4.2. Endereçamento Físico
- Usa endereços MAC para identificar origem e destino
- Diferente do endereço IP (lógico) da camada 3

### 4.3. Controle de Erros
- Detecta erros de transmissão (bits corrompidos)
- Pode solicitar retransmissão

### 4.4. Controle de Fluxo
- Evita que o transmissor sobrecarregue o receptor

### 4.5. Controle de Acesso ao Meio
- Gerencia quem transmite e quando
- Evita colisões em meios compartilhados

---

## 5. ENDEREÇAMENTO MAC

### O que é um endereço MAC?
É um identificador único de 48 bits (6 bytes) gravado na placa de rede pelo fabricante.

### Formato:
```
00:1A:2B:3C:4D:5E
ou
00-1A-2B-3C-4D-5E
```

### Estrutura:
- **Primeiros 24 bits**: OUI (Organizationally Unique Identifier) - identifica o fabricante
- **Últimos 24 bits**: NIC (Network Interface Controller) - número serial do dispositivo

### Exemplos de fabricantes:
- 00:1A:2B → Cisco
- 00:1C:B3 → Intel
- 00:1E:68 → Dell

### Tipos de endereços MAC:
- **Unicast**: Para um único destino (1º bit = 0)
- **Multicast**: Para grupo de dispositivos (1º bit = 1)
- **Broadcast**: Para todos (FF:FF:FF:FF:FF:FF)

---

## 6. MÉTODOS DE CONTROLE DE ACESSO AO MEIO

### 6.1. CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
- **Usado em**: Ethernet clássica (com hubs)
- **Como funciona**:
  1. Escuta o meio (Carrier Sense)
  2. Se livre, transmite
  3. Se detectar colisão, para e aguarda tempo aleatório
  4. Recomeça

### 6.2. CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)
- **Usado em**: Wi-Fi (redes sem fio)
- **Como funciona**:
  1. Escuta o meio
  2. Se livre, espera tempo aleatório (evita colisão)
  3. Transmite e aguarda confirmação

### 6.3. Token Passing (Passagem de Permissão)
- **Usado em**: Token Ring, FDDI
- **Como funciona**:
  - Um "token" circula na rede
  - Só quem tem o token pode transmitir

---

## 7. ESTRUTURA DO QUADRO (FRAME)

### Quadro Ethernet (padrão IEEE 802.3):

```
+----------+----------+-------------+-----------+----------+----------+
| Preâmbulo| SFD      | MAC Destino | MAC Origem| Tipo/Dado| FCS      |
| 7 bytes  | 1 byte   | 6 bytes     | 6 bytes   | 46-1500  | 4 bytes  |
+----------+----------+-------------+-----------+----------+----------+
```

### Detalhamento:

| Campo | Tamanho | Função |
|-------|---------|--------|
| **Preâmbulo** | 7 bytes | Sincronização |
| **SFD** | 1 byte | Início do quadro |
| **MAC Destino** | 6 bytes | Quem vai receber |
| **MAC Origem** | 6 bytes | Quem enviou |
| **Tipo/Comprimento** | 2 bytes | Protocolo superior (IPv4, ARP) |
| **Dados** | 46-1500 bytes | Payload da camada 3 |
| **FCS** | 4 bytes | Checksum (CRC) |

---

## 8. DETECÇÃO E CORREÇÃO DE ERROS

### 8.1. Técnicas de Detecção

#### ✅ Paridade
- Adiciona 1 bit para tornar o número de 1s par ou ímpar
- **Limitação**: Detecta erro em 1 bit apenas

#### ✅ Checksum
- Soma dos dados é armazenada no final
- Usado em protocolos mais simples

#### ✅ CRC (Cyclic Redundancy Check)
- Mais robusto
- Usado em Ethernet, Wi-Fi, etc.
- Divisão polinomial dos dados

### 8.2. Técnicas de Correção

#### 🔄 ARQ (Automatic Repeat Request)
- Detecta erro e solicita retransmissão

#### Tipos de ARQ:
- **Stop-and-Wait**: Envia 1 quadro, espera confirmação
- **Go-Back-N**: Envia N quadros, se erro, retransmite todos a partir do errado
- **Selective Repeat**: Retransmite apenas o quadro com erro

---

## 9. PROTOCOLOS DA CAMADA 2

### 9.1. Ethernet (IEEE 802.3)
- Mais comum em redes locais cabeadas
- Velocidades: 10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps, 40 Gbps, 100 Gbps

### 9.2. Wi-Fi (IEEE 802.11)
- Redes sem fio
- Versões: a/b/g/n/ac/ax/be

### 9.3. PPP (Point-to-Point Protocol)
- Conexões ponto a ponto (modems, links seriais)
- Usa autenticação (PAP, CHAP)

### 9.4. HDLC (High-Level Data Link Control)
- Protocolo síncrono para links seriais
- Base para muitos outros protocolos

### 9.5. Frame Relay
- Para WANs (redes de longa distância)
- Comutação de quadros

### 9.6. ARP (Address Resolution Protocol)
- **Importante**: ARP fica entre camada 2 e 3
- Traduz endereço IP (camada 3) em MAC (camada 2)

---

## 10. EQUIPAMENTOS DA CAMADA 2

### 🔌 Switch
- Dispositivo principal da camada 2
- Aprende endereços MAC e encaminha seletivamente
- Cria domínios de colisão separados por porta

### 🔌 Bridge (Ponte)
- Conecta dois segmentos de rede
- Filtra tráfego baseado em MAC

### 🔌 Placa de Rede (NIC)
- Interface entre computador e rede
- Tem endereço MAC gravado

### Comparação com Camada 1:
| Equipamento | Camada | Função |
|-------------|--------|--------|
| Hub | 1 | Repete sinal para todas as portas |
| Switch | 2 | Encaminha baseado em MAC |
| Repetidor | 1 | Regenera sinal |
| Bridge | 2 | Conecta segmentos e filtra |

---

## 11. EXEMPLO PRÁTICO

### Cenário: Computador A quer se comunicar com Computador B na mesma rede

**Passo 1:** A camada 3 (IP) quer enviar um pacote para 192.168.1.2
**Passo 2:** Precisa do MAC de destino (ARP)
**Passo 3:** Se não sabe, envia ARP broadcast (MAC FF:FF:FF:FF:FF:FF)
**Passo 4:** Computador B responde com seu MAC
**Passo 5:** A camada 2 monta o quadro:
- MAC Origem: AA:AA:AA:AA:AA:AA
- MAC Destino: BB:BB:BB:BB:BB:BB
- Dados: Pacote IP
- FCS: CRC calculado

**Passo 6:** Envia para camada 1 transmitir
**Passo 7:** Switch recebe, aprende MAC A na porta 1, encaminha para porta do MAC B
**Passo 8:** B recebe, verifica FCS, se OK, passa para camada 3

---

## 12. RESUMO VISUAL

```
┌─────────────────────────────────────┐
│         CAMADA 3 - REDE              │
│     (Pacotes, Endereços IP)          │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│    SUBCAMADA LLC                     │
│  - Interface com camada 3             │
│  - Multiplexação                       │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│    SUBCAMADA MAC                      │
│  - Endereços MAC                       │
│  - Acesso ao meio                       │
│  - Detecção de erros                     │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│         CAMADA 1 - FÍSICA            │
│      (Bits, Cabos, Sinais)           │
└─────────────────────────────────────┘
```

---

## 13. PERGUNTAS FREQUENTES

### ❓ Qual a diferença entre MAC e IP?
- **MAC**: Endereço físico, fixo, camada 2
- **IP**: Endereço lógico, variável, camada 3

### ❓ O switch entende de IP?
Não. Switch puro trabalha apenas com MAC. Switch de Camada 3 (roteador) entende IP.

### ❓ O que é broadcast na camada 2?
É um quadro enviado para todos os dispositivos da rede (MAC destino = FF:FF:FF:FF:FF:FF)

### ❓ Como o switch aprende os MACs?
Ele mantém uma tabela MAC x Porta, aprendendo conforme recebe quadros.

### ❓ O que causa muitos broadcasts?
Pode ser ataque ou configuração errada. Prejudica desempenho.

### ❓ Como funciona a detecção de colisão?
CSMA/CD: enquanto transmite, escuta se outro transmitiu ao mesmo tempo. Se sim, para e espera.

---

## 14. GLOSSÁRIO

| Termo | Significado |
|-------|-------------|
| **Quadro (Frame)** | Unidade de dados da camada 2 |
| **MAC** | Media Access Control |
| **LLC** | Logical Link Control |
| **CRC** | Cyclic Redundancy Check |
| **FCS** | Frame Check Sequence |
| **ARP** | Address Resolution Protocol |
| **CSMA/CD** | Carrier Sense Multiple Access with Collision Detection |
| **CSMA/CA** | Carrier Sense Multiple Access with Collision Avoidance |
| **OUI** | Organizationally Unique Identifier |
| **NIC** | Network Interface Card/Controller |
| **SFD** | Start Frame Delimiter |
| **MTU** | Maximum Transmission Unit |

---

## ✅ CONCLUSÃO

A **Camada 2 - Enlace de Dados** é responsável por:
- Organizar bits em quadros
- Endereçar dispositivos fisicamente (MAC)
- Detectar e corrigir erros
- Controlar acesso ao meio
- Fornecer comunicação confiável entre dispositivos vizinhos

Ela é a ponte entre o mundo físico (bits) e o mundo lógico (pacotes IP), garantindo que os dados cheguem corretamente ao próximo salto na rede.

---
