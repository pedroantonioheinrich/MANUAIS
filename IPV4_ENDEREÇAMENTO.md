# 📚 MANUAL COMPLETO DE ENDEREÇAMENTO IPv4

## SUMÁRIO
1. [Fundamentos do IPv4](#1-fundamentos-do-ipv4)
2. [Classes de Endereços IP](#2-classes-de-endereços-ip)
3. [Máscara de Sub-rede](#3-máscara-de-sub-rede)
4. [Cálculos de Sub-rede Passo a Passo](#4-cálculos-de-sub-rede-passo-a-passo)
5. [Exercícios Práticos Resolvidos](#5-exercícios-práticos-resolvidos)
6. [Tabelas de Referência Rápida](#6-tabelas-de-referência-rápida)

---

## 1. FUNDAMENTOS DO IPv4

### 1.1 O que é um Endereço IPv4?
É um número de 32 bits dividido em 4 octetos (8 bits cada), representado no formato decimal com pontos.

**Exemplo:** `192.168.1.1`

### 1.2 Representação Binária
Cada octeto pode variar de 0 a 255:

| Decimal | Binário |
|---------|---------|
| 0       | 00000000 |
| 255     | 11111111 |

### 1.3 Estrutura do Endereço IP
Um endereço IP possui duas partes:
- **Rede (Network)**: Identifica a rede
- **Host**: Identifica o dispositivo na rede

---

## 2. CLASSES DE ENDEREÇOS IP

### 2.1 Classes Tradicionais

| Classe | Primeiro Octeto | Máscara Padrão | Faixa |
|--------|-----------------|----------------|-------|
| A      | 1-126           | 255.0.0.0      | Redes: 126, Hosts: 16.777.214 |
| B      | 128-191         | 255.255.0.0    | Redes: 16.384, Hosts: 65.534 |
| C      | 192-223         | 255.255.255.0  | Redes: 2.097.152, Hosts: 254 |
| D      | 224-239         | Multicast      | - |
| E      | 240-255         | Experimental   | - |

### 2.2 Endereços Especiais
- **Loopback**: 127.0.0.0/8 (teste local)
- **Rede Privada**: 
  - 10.0.0.0/8
  - 172.16.0.0/12
  - 192.168.0.0/16

---

## 3. MÁSCARA DE SUB-REDE

### 3.1 O que é Máscara de Sub-rede?
É um número de 32 bits que define qual parte do IP é rede e qual é host.

### 3.2 Notação CIDR
Formato: `IP/máscara` onde máscara é o número de bits 1

**Exemplo:** `192.168.1.0/24` (máscara 255.255.255.0)

### 3.3 Tabela de Máscaras

| CIDR | Máscara | Bits para Hosts | Total Hosts |
|------|---------|-----------------|-------------|
| /24  | 255.255.255.0   | 8  | 256 (254 úteis) |
| /25  | 255.255.255.128 | 7  | 128 (126 úteis) |
| /26  | 255.255.255.192 | 6  | 64 (62 úteis) |
| /27  | 255.255.255.224 | 5  | 32 (30 úteis) |
| /28  | 255.255.255.240 | 4  | 16 (14 úteis) |
| /29  | 255.255.255.248 | 3  | 8 (6 úteis) |
| /30  | 255.255.255.252 | 2  | 4 (2 úteis) |

---

## 4. CÁLCULOS DE SUB-REDE PASSO A PASSO

### 4.1 Fórmulas Fundamentais

```
Número de sub-redes = 2^n (onde n = bits emprestados)
Número de hosts por sub-rede = 2^h - 2 (onde h = bits de host)
Salto (incremento) = 256 - último octeto da máscara
```

### 4.2 Passo a Passo para Calcular Sub-redes

#### EXEMPLO PRÁTICO 1:
**Dado:** IP 192.168.1.0/26

**Passo 1: Identificar a máscara**
- /26 = 255.255.255.192
- Bits de rede: 26
- Bits de host: 32 - 26 = 6 bits

**Passo 2: Calcular número de sub-redes**
- Máscara original Classe C: /24 (24 bits)
- Bits emprestados: 26 - 24 = 2 bits
- Sub-redes possíveis: 2² = 4 sub-redes

**Passo 3: Calcular hosts por sub-rede**
- Hosts: 2⁶ - 2 = 64 - 2 = 62 hosts úteis

**Passo 4: Calcular o salto**
- Salto = 256 - 192 = 64

**Passo 5: Listar as sub-redes**
1. 192.168.1.0 - 192.168.1.63 (Broadcast: 192.168.1.63)
2. 192.168.1.64 - 192.168.1.127 (Broadcast: 192.168.1.127)
3. 192.168.1.128 - 192.168.1.191 (Broadcast: 192.168.1.191)
4. 192.168.1.192 - 192.168.1.255 (Broadcast: 192.168.1.255)

### 4.3 Como Encontrar Informações Específicas

#### Para encontrar o endereço de rede:
- Converter IP e máscara para binário
- Fazer AND lógico bit a bit
- Converter resultado para decimal

**Exemplo:** IP 192.168.1.35 com máscara 255.255.255.224

```
IP:      11000000.10101000.00000001.00100011
Máscara: 11111111.11111111.11111111.11100000
AND:     11000000.10101000.00000001.00100000
Rede:    192.168.1.32
```

#### Para encontrar o broadcast:
- Pegar endereço de rede
- Colocar todos bits de host como 1
- Converter para decimal

**Exemplo:** Rede 192.168.1.32/27
- Bits de host: 5 bits
- Broadcast: 192.168.1.63

---

## 5. EXERCÍCIOS PRÁTICOS RESOLVIDOS

### EXERCÍCIO 1: Divisão em Sub-redes
**Problema:** Você tem a rede 10.0.0.0/8 e precisa de 500 sub-redes com no mínimo 100 hosts cada.

**Passo 1: Calcular bits necessários para hosts**
- 100 hosts → 2⁷ = 128 (7 bits, 126 hosts úteis)
- Precisamos de 7 bits para hosts

**Passo 2: Calcular máscara necessária**
- Total bits: 32
- Bits rede: 32 - 7 = 25 bits
- Máscara: /25 (255.255.255.128)

**Passo 3: Verificar número de sub-redes**
- Máscara original: /8
- Bits emprestados: 25 - 8 = 17 bits
- Sub-redes possíveis: 2¹⁷ = 131.072 sub-redes ✓

**Resposta:** Use máscara /25 (255.255.255.128)

### EXERCÍCIO 2: Cálculo Completo
**Problema:** Determine todas as informações para o IP 172.16.35.200/20

**Passo 1: Converter máscara**
- /20 = 255.255.240.0
- 3º octeto: 240 = 11110000

**Passo 2: Encontrar rede**
- IP 3º octeto: 35 = 00100011
- AND com 11110000 = 00100000 = 32
- Rede: 172.16.32.0/20

**Passo 3: Calcular broadcast**
- Bits host: 12 bits
- Último endereço: 172.16.47.255

**Passo 4: Faixa de hosts válidos**
- Primeiro host: 172.16.32.1
- Último host: 172.16.47.254

### EXERCÍCIO 3: VLSM (Variable Length Subnet Mask)
**Problema:** Divida 192.168.1.0/24 para atender:
- Rede A: 60 hosts
- Rede B: 30 hosts
- Rede C: 10 hosts
- Rede D: 2 hosts (link ponto-a-ponto)

**Solução:**

**Rede A (60 hosts):**
- 60 hosts → 6 bits (2⁶ = 64, 62 úteis)
- Máscara: /26 (255.255.255.192)
- Rede: 192.168.1.0/26 (0-63)

**Rede B (30 hosts):**
- 30 hosts → 5 bits (2⁵ = 32, 30 úteis)
- Máscara: /27
- Rede: 192.168.1.64/27 (64-95)

**Rede C (10 hosts):**
- 10 hosts → 4 bits (2⁴ = 16, 14 úteis)
- Máscara: /28
- Rede: 192.168.1.96/28 (96-111)

**Rede D (2 hosts):**
- 2 hosts → 2 bits (2² = 4, 2 úteis)
- Máscara: /30
- Rede: 192.168.1.112/30 (112-115)

---

## 6. TABELAS DE REFERÊNCIA RÁPIDA

### Tabela de Conversão CIDR

| CIDR | Máscara | Hosts | Redes Classe C |
|------|---------|-------|----------------|
| /24  | 255.255.255.0   | 254   | 1 |
| /25  | 255.255.255.128 | 126   | 2 |
| /26  | 255.255.255.192 | 62    | 4 |
| /27  | 255.255.255.224 | 30    | 8 |
| /28  | 255.255.255.240 | 14    | 16 |
| /29  | 255.255.255.248 | 6     | 32 |
| /30  | 255.255.255.252 | 2     | 64 |
| /31  | 255.255.255.254 | 0*    | 128 |
| /32  | 255.255.255.255 | 1**   | 256 |

* /31 usado apenas para links ponto-a-ponto (RFC 3021)
** /32 usado para host único (loopback)

### Dicas Rápidas para Cálculos Mentais

1. **Para achar o número mágico:** 256 - último octeto da máscara
2. **Hosts úteis:** 2^n - 2 (n = bits de host)
3. **Broadcast:** Rede + número mágico - 1
4. **Próxima rede:** Rede atual + número mágico

### Checklist para Resolver Problemas de Sub-rede

- [ ] Identificar IP e máscara original
- [ ] Determinar bits de rede e host
- [ ] Calcular número de sub-redes (se aplicável)
- [ ] Calcular hosts por sub-rede
- [ ] Encontrar o "número mágico" (salto)
- [ ] Listar endereços de rede
- [ ] Calcular broadcasts
- [ ] Definir faixas de hosts válidos
- [ ] Verificar endereços especiais (rede e broadcast)

