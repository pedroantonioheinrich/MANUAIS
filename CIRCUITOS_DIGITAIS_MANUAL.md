# 📚 Manual Completo de Circuitos Digitais
## Guia de Estudos para Iniciantes

---

# 📋 Índice

1. [Introdução](#introdução)
2. [Sistemas de Numeração](#sistemas-de-numeração)
3. [Portas Lógicas](#portas-lógicas)
4. [Álgebra Booleana](#álgebra-booleana)
5. [Circuitos Combinacionais](#circuitos-combinacionais)
6. [Circuitos Sequenciais](#circuitos-sequenciais)
7. [Componentes Avançados](#componentes-avançados)
8. [Exercícios Práticos](#exercícios-práticos)
9. [Recursos Adicionais](#recursos-adicionais)

---

# 🎯 Introdução

## O que são Circuitos Digitais?

Circuitos digitais são sistemas eletrônicos que trabalham com **sinais discretos**, geralmente representados por **0** e **1** (níveis lógicos). Diferente dos circuitos analógicos, que trabalham com variações contínuas, os circuitos digitais processam informações de forma binária.

### Por que estudar Circuitos Digitais?

- ✅ Base de todos os computadores modernos
- ✅ Essencial para IoT, robótica e automação
- ✅ Fundamentação para arquitetura de computadores
- ✅ Alta demanda no mercado de tecnologia

---

# 🔢 Sistemas de Numeração

## 1. Sistema Binário (Base 2)

**Dígitos:** 0, 1  
**Exemplo:** 1010₂ = 10₁₀

### Conversão Binário → Decimal
```
1010₂ = 1×2³ + 0×2² + 1×2¹ + 0×2⁰
      = 8 + 0 + 2 + 0
      = 10₁₀
```

### Conversão Decimal → Binário
```
13₁₀ = ?

13 ÷ 2 = 6 resto 1 ↑
6 ÷ 2 = 3  resto 0 ↑
3 ÷ 2 = 1  resto 1 ↑
1 ÷ 2 = 0  resto 1 ↑

Resultado: 1101₂ (ler de baixo para cima)
```

## 2. Sistema Hexadecimal (Base 16)

**Dígitos:** 0-9, A(10), B(11), C(12), D(13), E(14), F(15)  
**Exemplo:** 2F₁₆ = 47₁₀

### Conversão Hexadecimal → Binário
```
2F₁₆ = 0010 1111₂
Cada dígito hex = 4 bits

2 = 0010
F = 1111
```

## 3. Sistema Octal (Base 8)

**Dígitos:** 0-7  
**Exemplo:** 73₈ = 59₁₀

---

# 🚪 Portas Lógicas

## Portas Básicas

### 🔹 AND (E)
| A | B | Y = A·B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**Símbolo:** `A & B → Y`

### 🔹 OR (OU)
| A | B | Y = A+B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**Símbolo:** `A ≥1 B → Y`

### 🔹 NOT (NÃO)
| A | Y = ¬A |
|---|--------|
| 0 | 1 |
| 1 | 0 |

**Símbolo:** `A ○→ Y`

## Portas Derivadas

### 🔸 NAND (NOT AND)
| A | B | Y = ¬(A·B) |
|---|---|-----------|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### 🔸 NOR (NOT OR)
| A | B | Y = ¬(A+B) |
|---|---|-----------|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

### 🔸 XOR (OU Exclusivo)
| A | B | Y = A⊕B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### 🔸 XNOR (NOT XOR)
| A | B | Y = ¬(A⊕B) |
|---|---|-----------|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

---

# 📐 Álgebra Booleana

## Postulados Básicos

### Propriedades da Adição (OR)
```
A + 0 = A
A + 1 = 1
A + A = A
A + ¬A = 1
```

### Propriedades da Multiplicação (AND)
```
A · 0 = 0
A · 1 = A
A · A = A
A · ¬A = 0
```

### Complemento
```
¬(¬A) = A
```

## Teoremas Importantes

### Teorema de DeMorgan
```
¬(A · B) = ¬A + ¬B
¬(A + B) = ¬A · ¬B
```

**Dica:** Para aplicar DeMorgan:
1. Troque AND por OR (ou vice-versa)
2. Complemente todas as variáveis
3. Complemente o resultado final

### Propriedade Distributiva
```
A · (B + C) = A·B + A·C
A + (B·C) = (A+B) · (A+C)
```

### Propriedade da Absorção
```
A + A·B = A
A · (A + B) = A
```

---

# 🔧 Circuitos Combinacionais

## Definição
Saída depende **apenas** da entrada atual (sem memória).

## 1. Somadores

### Meio Somador (Half Adder)
```
Entradas: A, B
Saídas: S (soma), Cout (vai-um)

S = A ⊕ B
Cout = A · B
```

### Somador Completo (Full Adder)
```
Entradas: A, B, Cin
Saídas: S, Cout

S = A ⊕ B ⊕ Cin
Cout = A·B + A·Cin + B·Cin
```

## 2. Multiplexador (MUX)

Seleciona uma entre várias entradas.

### MUX 2:1
```
S = (¬S0 · D0) + (S0 · D1)

D0 ─┬────┐
    │    │ S0
    └────┼──── S
D1 ─┬───┘
    │
    └────┘
```

## 3. Demultiplexador (DEMUX)

Distribui uma entrada para várias saídas.

## 4. Decodificador

Ativa uma única saída baseada na entrada binária.

### Decodificador 2:4
```
Entrada: A1, A0
Saídas: Y0, Y1, Y2, Y3

Y0 = ¬A1 · ¬A0
Y1 = ¬A1 · A0
Y2 = A1 · ¬A0
Y3 = A1 · A0
```

## 5. Codificador

Converte código binário para outro formato.

---

# ⏰ Circuitos Sequenciais

## Definição
Saída depende das **entradas atuais** E do **estado anterior** (com memória).

## 1. Latch SR

### Tabela Verdade
| S | R | Q | ¬Q |
|---|---|---|---|
| 0 | 0 | Q | ¬Q | (memória)
| 0 | 1 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | ? | ? | (proibido)

## 2. Flip-Flops

### Flip-Flop D
```
Q+ = D
```
Armazena 1 bit de dado.

### Flip-Flop JK
```
J K | Q+
0 0 | Q    (memória)
0 1 | 0    (reset)
1 0 | 1    (set)
1 1 | ¬Q   (toggle)
```

### Flip-Flop T
```
T | Q+
0 | Q    (memória)
1 | ¬Q   (toggle)
```

## 3. Registradores

Conjunto de flip-flops que armazenam múltiplos bits.

## 4. Contadores

### Contador Binário de 3 bits
```
CLK → [FF0] → [FF1] → [FF2] → Q2
        Q0     Q1     Q2
```

Sequência: 000, 001, 010, 011, 100, 101, 110, 111, 000...

---

# 🏗️ Componentes Avançados

## 1. Memória RAM

**Organização:** Palavras × Bits  
**Exemplo:** 8x4 = 8 palavras de 4 bits

```
Endereço (3 bits) → Decodificador → 8 linhas
Dados (4 bits)    → Entrada/Saída
```

## 2. Unidade Lógica Aritmética (ULA)

Executa operações aritméticas e lógicas:

**Operações típicas:**
- Soma
- Subtração
- AND, OR, XOR
- Deslocamento

## 3. Máquinas de Estados Finitos (FSM)

### Modelo de Moore
Saída depende **apenas** do estado atual.

### Modelo de Mealy
Saída depende do estado atual **e** das entradas.

---

# 💡 Exercícios Práticos

## Nível 1 - Básico

### Exercício 1.1
Converta para binário:
a) 25₁₀
b) 47₁₀
c) 128₁₀

<details>
<summary>Ver resposta</summary>

a) 11001₂  
b) 101111₂  
c) 10000000₂
</details>

### Exercício 1.2
Determine a expressão booleana:
```
A B C | Y
0 0 0 | 0
0 0 1 | 1
0 1 0 | 0
0 1 1 | 1
1 0 0 | 0
1 0 1 | 1
1 1 0 | 0
1 1 1 | 1
```

<details>
<summary>Ver resposta</summary>

Y = C  
(Pois Y = 1 apenas quando C = 1)
</details>

## Nível 2 - Intermediário

### Exercício 2.1
Simplifique usando álgebra booleana:
```
Y = A·B·C + A·B·¬C + A·¬B·C
```

<details>
<summary>Ver resposta</summary>

```
Y = A·B·C + A·B·¬C + A·¬B·C
Y = A·B·(C + ¬C) + A·¬B·C
Y = A·B·1 + A·¬B·C
Y = A·B + A·¬B·C
Y = A·(B + ¬B·C)
Y = A·(B + C)   (absorção)
```
</details>

### Exercício 2.2
Projete um circuito que detecte números pares em binário (3 bits).

<details>
<summary>Ver resposta</summary>

Número par = bit menos significativo = 0  
Y = ¬A0
</details>

## Nível 3 - Avançado

### Exercício 3.1
Projete um contador síncrono de 0 a 5 usando flip-flops JK.

<details>
<summary>Ver resposta</summary>

Estados: 000 → 001 → 010 → 011 → 100 → 101 → 000  
Use mapa de Karnaugh para obter J2, K2, J1, K1, J0, K0
</details>

---

# 📚 Recursos Adicionais

## 🛠️ Ferramentas de Simulação

| Ferramenta | Tipo | Descrição |
|------------|------|-----------|
| **Logisim** | Software | Grátis, educacional, fácil |
| **Digital** | Software | Alternativa moderna ao Logisim |
| **CircuitVerse** | Online | Simulador no navegador |
| **EDA Playground** | Online | VHDL/Verilog online |

## 📖 Livros Recomendados

1. **"Circuitos Digitais: Teoria e Prática"** - Floyd
2. **"Fundamentos de Circuitos Digitais"** - Tocci
3. **"Sistemas Digitais"** - Widmer

## 🌐 Canais YouTube (PT/BR)

- **WR Kits** - Projetos práticos
- **Embarcados** - Conteúdo técnico
- **Eletrônica Para Todos** - Didática excelente

## 💻 Projetos Práticos Sugeridos

1. **Semáforo automático** - Contador + lógica combinacional
2. **Calculadora binária** - Somador + display 7 segmentos
3. **Cofre digital** - Flip-flops + comparador
4. **Jogo de memória** - Registradores + máquina de estados

---

# 🎓 Checklist de Aprendizado

- [ ] Entendo sistemas de numeração binário e hexadecimal
- [ ] Conheço todas as portas lógicas básicas
- [ ] Consigo simplificar expressões booleanas
- [ ] Sei projetar circuitos combinacionais
- [ ] Compreendo a diferença entre latch e flip-flop
- [ ] Sei projetar contadores simples
- [ ] Consigo usar ferramentas de simulação
- [ ] Desenvolvi pelo menos 1 projeto completo

---

# 📝 Glossário Rápido

| Termo | Significado |
|-------|------------|
| **Bit** | Binary digit (0 ou 1) |
| **Byte** | 8 bits |
| **Nibble** | 4 bits |
| **Word** | Tamanho da arquitetura (16, 32, 64 bits) |
| **Clock** | Sinal de sincronização |
| **Setup time** | Tempo antes do clock |
| **Hold time** | Tempo após o clock |
| **Fan-out** | Capacidade de saída |
| **Propagation delay** | Atraso do sinal |

---

## 🚀 Próximos Passos

1. ✅ Dominar circuitos combinacionais
2. ✅ Dominar circuitos sequenciais
3. 📚 Estudar VHDL/Verilog
4. 🔧 Implementar projetos em FPGA
5. 💡 Integrar com microcontroladores
6. 🎯 Especializar em arquitetura de computadores

---

> **Dica Final:** Pratique todos os dias! Circuitos digitais são como um novo idioma - quanto mais você pratica, mais fluente se torna.

**Bons estudos! 🎉**
