# Manual Completo de OP CODES Assembly x86-64 Linux

## Sumário
1. [Fundamentos dos OP CODES](#fundamentos)
2. [Formato Geral da Instrução](#formato-instrucao)
3. [Prefixos: Modificadores de Instrução](#prefixos)
4. [O Opcode Principal](#opcode-principal)
5. [ModR/M: Especificando Operandos](#modrm)
6. [SIB: Endereçamento Indexado](#sib)
7. [Deslocamento e Imediatos](#deslocamento-imediato)
8. [Tabela Prática de Opcodes](#tabela-pratica)
9. [Exemplos de Decodificação](#exemplos-decodificacao)
10. [Aplicações Práticas](#aplicacoes-praticas)

## 1. Fundamentos dos OP CODES {#fundamentos}

### O que são OP CODES?

OP CODE é a abreviação de **Operation Code** - o byte ou conjunto de bytes que diz ao processador qual operação executar . Enquanto os registradores são os "recipientes" de dados, os opcodes são as "instruções" que manipulam esses recipientes.

```
Analogia:
- Registradores = Caixas de ferramentas
- Opcodes = Instruções de como usar as ferramentas
- Programa = Sequência de instruções (opcodes + operandos)
```

### A Complexidade do x86-64

O x86-64 possui uma das arquiteturas de instrução mais complexas do mundo, com:
- **Mais de 1000 instruções** diferentes
- **Instruções de tamanho variável** (1 a 15 bytes) 
- **Múltiplas formas de codificar** a mesma instrução
- **Compatibilidade retroativa** com o 8086 de 1978

## 2. Formato Geral da Instrução {#formato-instrucao}

Toda instrução x86-64 segue esta estrutura básica, nesta ordem exata :

| Componente | Tamanho | Obrigatório | Descrição |
|------------|---------|-------------|-----------|
| **Prefixos** | 0-4 bytes | Não | Modificam o comportamento da instrução |
| **Opcode** | 1-3 bytes | **Sim** | Código da operação principal |
| **ModR/M** | 0-1 byte | Condicional | Modo de endereçamento e registradores |
| **SIB** | 0-1 byte | Condicional | Scale-Index-Base para endereçamento complexo |
| **Deslocamento** | 0-8 bytes | Não | Deslocamento para endereços de memória |
| **Imediato** | 0-8 bytes | Não | Valor constante imediato |

**Tamanho máximo**: 15 bytes 

```
Exemplo visual de uma instrução completa:
┌─────────┬─────────┬────────┬─────┬──────────────┬────────────┐
│ Prefixos│ Opcode  │ ModR/M │ SIB │ Displacement │ Immediate  │
│ 0-4     │ 1-3     │ 0-1    │ 0-1 │ 0-8          │ 0-8        │
└─────────┴─────────┴────────┴─────┴──────────────┴────────────┘
```

## 3. Prefixos: Modificadores de Instrução {#prefixos}

Os prefixos são bytes opcionais que alteram o comportamento da instrução. Podem haver até 4 prefixos, e a ordem entre grupos não importa, mas dentro do mesmo grupo a ordem é significativa .

### Grupo 1: Prefixos de Bloqueio e Repetição

| Byte | Nome | Descrição |
|------|------|-----------|
| `0xF0` | **LOCK** | Execução atômica em memória compartilhada  |
| `0xF2` | **REPNE/REPNZ** | Repete enquanto não igual/zero  |
| `0xF3` | **REP/REPE/REPZ** | Repete enquanto igual/zero  |

**Uso do LOCK** (apenas com certas instruções): ADC, ADD, AND, BTC, BTR, BTS, CMPXCHG, CMPXCHG8B, CMPXCHG16B, DEC, INC, NEG, NOT, OR, SBB, SUB, XADD, XCHG, XOR 

### Grupo 2: Segmentos e Branch Hints

| Byte | Nome | Descrição |
|------|------|-----------|
| `0x2E` | **CS** | Segmento CS / Branch not taken  |
| `0x36` | **SS** | Segmento SS |  |
| `0x3E` | **DS** | Segmento DS / Branch taken  |
| `0x26` | **ES** | Segmento ES |  |
| `0x64` | **FS** | Segmento FS |  |
| `0x65` | **GS** | Segmento GS |  |

**Nota em 64-bit**: CS, SS, DS e ES são ignorados 

### Grupo 3: Tamanho do Operando

| Byte | Nome | Efeito |
|------|------|--------|
| `0x66` | **Operand-size override** | Alterna tamanho do operando  |

### Grupo 4: Tamanho do Endereço

| Byte | Nome | Efeito |
|------|------|--------|
| `0x67` | **Address-size override** | Alterna tamanho do endereço  |

### REX Prefix (64-bit específico)

O prefixo REX é fundamental em 64-bit. Ele tem estrutura de bits própria :

```
REX prefix: 0x40 + (bits W, R, X, B)
Formato: 0100 WRXB (binário)

Bits:
- Bit 7-4: Sempre 0100 (4) - identificador do REX
- Bit 3 (W): 0 = tamanho definido por CS, 1 = operando 64-bit
- Bit 2 (R): Extensão do campo Reg do ModR/M
- Bit 1 (X): Extensão do campo Index do SIB
- Bit 0 (B): Extensão do campo R/M, base SIB, ou opcode
```

**Exemplos comuns**:
- `0x48` = REX.W (W=1, outros 0) - operando 64-bit
- `0x41` = REX.B (B=1) - acessa R8-R15

## 4. O Opcode Principal {#opcode-principal}

O opcode é o coração da instrução. Pode ocupar de 1 a 3 bytes e determina qual operação será executada .

### Estrutura dos Opcodes

Os opcodes são organizados em tabelas :

- **Tabela primária** (1 byte): 0x00 a 0xFF
- **Tabelas de escape** (2 bytes): Começam com 0x0F, 0x0F 0x38, 0x0F 0x3A
- **Tabelas de grupo** (opcode estendido pelo campo Reg do ModR/M)

### Notação de Opcode

Nos manuais, os opcodes aparecem com notação especial:

| Notação | Significado |
|---------|-------------|
| `/digit` | O campo Reg do ModR/M contém extensão do opcode (0-7)  |
| `/r` | ModR/M contém registrador e modo de endereçamento |
| `+rb` `+rw` `+rd` `+ro` | Adiciona número do registrador ao opcode  |
| `cb` `cw` `cd` `cp` `co` | Constantes de tamanhos específicos |

### Exemplos de Codificação

#### Instruções com registrador embutido
```nasm
PUSH RAX    ; Opcode: 0x50 + 0 (código RAX=0) = 0x50
PUSH RCX    ; Opcode: 0x50 + 1 (código RCX=1) = 0x51
PUSH R8     ; Precisa REX: 0x41 (REX.B) + 0x50 + 0? (calculo complexo)
```

#### Instruções com opcode estendido (/digit)
```nasm
ADD r/m64, imm8  ; Opcode 0x83 /0 (ADD com imediato 8-bit)
AND r/m64, imm8  ; Opcode 0x83 /4 (AND com imediato 8-bit)
```

## 5. ModR/M: Especificando Operandos {#modrm}

O byte ModR/M é um dos conceitos mais importantes da codificação x86. Ele especifica os operandos da instrução .

### Estrutura do ModR/M

```
Byte ModR/M: 8 bits divididos em 3 campos
┌─────────┬─────────────┬─────────┐
│  Mod    │ Reg/Opcode  │  R/M    │
│ 2 bits  │   3 bits    │ 3 bits  │
└─────────┴─────────────┴─────────┘
Bits:  7-6       5-3        2-0
```

### Campo Mod (bits 7-6)

| Mod | Significado |
|-----|-------------|
| 00 | Register indirect ([reg]) ou SIB/disp32 especial  |
| 01 | Register indirect + deslocamento 8-bit  |
| 10 | Register indirect + deslocamento 32-bit  |
| 11 | Register direct (operando é registrador)  |

### Campo Reg/Opcode (bits 5-3)

- Se instrução tem `/digit`: contém extensão do opcode (0-7)
- Se instrução tem `/r`: contém código do registrador origem/destino

### Campo R/M (bits 2-0)

Combina com Mod para especificar o operando de memória/registrador.

### Codificação de Registradores

Tabela de codificação para registradores de 64-bit :

| Código | Sem REX | Com REX.R/B | Uso |
|--------|---------|-------------|-----|
| 000 | RAX | R8 | Acumulador/1º retorno |
| 001 | RCX | R9 | Contador/4º argumento |
| 010 | RDX | R10 | Dados/3º argumento |
| 011 | RBX | R11 | Base (preservado) |
| 100 | RSP | R12 | Ponteiro de pilha |
| 101 | RBP | R13 | Ponteiro base |
| 110 | RSI | R14 | Índice fonte/2º arg |
| 111 | RDI | R15 | Índice destino/1º arg |

### Casos Especiais

- **Mod=00 e R/M=101**: Segue deslocamento de 32-bit (endereço absoluto) 
- **Mod≠11 e R/M=100**: Segue byte SIB 

## 6. SIB: Scale-Index-Base {#sib}

O byte SIB permite endereçamento indexado complexo do tipo `[base + index*scale + displacement]` .

### Estrutura do SIB

```
Byte SIB: 8 bits divididos em 3 campos
┌─────────┬─────────────┬─────────┐
│  Scale  │   Index     │  Base   │
│ 2 bits  │   3 bits    │ 3 bits  │
└─────────┴─────────────┴─────────┘
Bits:  7-6       5-3        2-0
```

### Campo Scale (bits 7-6)

| Scale | Fator multiplicador |
|-------|---------------------|
| 00 | 1  |
| 01 | 2  |
| 10 | 4  |
| 11 | 8  |

### Campo Index (bits 5-3)

Registrador usado como índice. Códigos seguem mesma tabela dos registradores.

**Caso especial**: Index=100 (RSP/R12) significa **sem índice** 

### Campo Base (bits 2-0)

Registrador usado como base.

**Caso especial**: Base=101 (RBP/R13) com Mod=00 indica deslocamento de 32-bit sem base 

### Fórmula de Endereço Efetivo

```
Endereço = Base + (Index × Scale) + Deslocamento
```

## 7. Deslocamento e Imediatos {#deslocamento-imediato}

### Deslocamento (Displacement)

Valor adicionado ao endereço calculado :

| Tamanho | Quando usado |
|---------|--------------|
| 1 byte | Mod=01 |
| 4 bytes | Mod=10 ou Mod=00 com R/M=101 |
| 8 bytes | Apenas em modos específicos (64-bit) |

### Imediatos (Immediates)

Valores constantes embutidos na instrução :

| Tamanho | Descrição |
|---------|-----------|
| 1 byte | imm8 |
| 2 bytes | imm16 |
| 4 bytes | imm32 |
| 8 bytes | imm64 (raro, apenas em MOV) |

## 8. Tabela Prática de Opcodes {#tabela-pratica}

Baseado no cheat sheet de opcodes :

### Instruções de Movimentação

| Instrução | Opcode | Formato | Descrição |
|-----------|--------|---------|-----------|
| **MOV r/m8, r8** | `0x88` | /r | Move byte de reg para mem/reg |
| **MOV r/m64, r64** | `0x89` | /r | Move 64-bit de reg para mem/reg |
| **MOV r8, r/m8** | `0x8A` | /r | Move byte de mem/reg para reg |
| **MOV r64, r/m64** | `0x8B` | /r | Move 64-bit de mem/reg para reg |
| **MOV r8, imm8** | `0xB0 + reg` | +rb | Move imediato byte para reg |
| **MOV r64, imm64** | `0xB8 + reg` | +ro | Move imediato 64-bit para reg |
| **MOV r/m8, imm8** | `0xC6 /0` | /0 | Move imediato byte para mem/reg |
| **MOV r/m64, imm32** | `0xC7 /0` | /0 | Move imediato 32-bit (extendido) |

### Instruções Aritméticas

| Instrução | Opcode | Formato | Descrição |
|-----------|--------|---------|-----------|
| **ADD r/m64, r64** | `0x01` | /r | Soma reg a mem/reg |
| **ADD r64, r/m64** | `0x03` | /r | Soma mem/reg a reg |
| **ADD AL, imm8** | `0x04` | ib | Soma imediato a AL |
| **ADD RAX, imm32** | `0x05` | id | Soma imediato extendido a RAX |
| **ADD r/m64, imm8** | `0x83 /0` | /0 | Soma imediato 8-bit extendido |
| **ADD r/m64, imm32** | `0x81 /0` | /0 | Soma imediato 32-bit extendido |

### Instruções de Pilha

| Instrução | Opcode | Formato | Descrição |
|-----------|--------|---------|-----------|
| **PUSH r64** | `0x50 + reg` | +ro | Push registrador |
| **PUSH imm8** | `0x6A` | ib | Push imediato 8-bit extendido |
| **PUSH imm32** | `0x68` | id | Push imediato 32-bit extendido |
| **PUSH r/m64** | `0xFF /6` | /6 | Push mem/reg |
| **POP r64** | `0x58 + reg` | +ro | Pop para registrador |
| **POP r/m64** | `0x8F /0` | /0 | Pop para mem/reg |

### Instruções de Controle

| Instrução | Opcode | Formato | Descrição |
|-----------|--------|---------|-----------|
| **CALL rel32** | `0xE8` | cd | Call near relativo |
| **CALL r/m64** | `0xFF /2` | /2 | Call indireto |
| **JMP rel8** | `0xEB` | cb | Jump curto |
| **JMP rel32** | `0xE9` | cd | Jump near |
| **JMP r/m64** | `0xFF /4` | /4 | Jump indireto |
| **RET** | `0xC3` | - | Retorna de procedimento |
| **RET imm16** | `0xC2` | cw | Retorna e libera bytes da pilha |

### Saltos Condicionais

| Instrução | Opcode (rel8) | Opcode (rel32) | Condição |
|-----------|----------------|----------------|----------|
| **JE/JZ** | `0x74` | `0x0F 0x84` | Igual/Zero  |
| **JNE/JNZ** | `0x75` | `0x0F 0x85` | Não igual  |
| **JG/JNLE** | `0x7F` | `0x0F 0x8F` | Maior (signed)  |
| **JGE/JNL** | `0x7D` | `0x0F 0x8D` | Maior ou igual  |
| **JL/JNGE** | `0x7C` | `0x0F 0x8C` | Menor  |
| **JLE/JNG** | `0x7E` | `0x0F 0x8E` | Menor ou igual  |
| **JA/JNBE** | `0x77` | `0x0F 0x87` | Acima (unsigned)  |
| **JAE/JNB** | `0x73` | `0x0F 0x83` | Acima ou igual  |
| **JB/JNAE** | `0x72` | `0x0F 0x82` | Abaixo  |
| **JBE/JNA** | `0x76` | `0x0F 0x86` | Abaixo ou igual  |

### Syscall

| Instrução | Opcode | Descrição |
|-----------|--------|-----------|
| **SYSCALL** | `0x0F 0x05` | Chamada de sistema Linux  |

## 9. Exemplos de Decodificação {#exemplos-decodificacao}

Vamos decodificar instruções reais passo a passo.

### Exemplo 1: ADD RAX, RBX

**Código de máquina**: `0x48 0x01 0xD8`

**Decodificação**:

```
0x48: REX prefix
    └─ 0100 1000 = REX.W (W=1, outros 0)
    
0x01: Opcode ADD r/m64, r64 (forma 01 /r)
    └─ Instrução ADD com destino mem/reg, origem reg

0xD8: ModR/M byte (11011000 em binário)
    └─ Mod    = 11 (bits 7-6) = register direct
    └─ Reg    = 011 (bits 5-3) = código RBX (origem)
    └─ R/M    = 000 (bits 2-0) = código RAX (destino)

Resultado: ADD RAX (destino), RBX (origem)
```

### Exemplo 2: MOV [RSP+8], RAX

**Código de máquina**: `0x48 0x89 0x44 0x24 0x08`

**Decodificação**:

```
0x48: REX prefix (REX.W)
0x89: Opcode MOV r/m64, r64

0x44: ModR/M (01000100)
    └─ Mod = 01 (indirect + disp8)
    └─ Reg = 000 (código RAX - origem)
    └─ R/M = 100 (RSP - indica que SIB segue)

0x24: SIB (00100100)
    └─ Scale = 00 (×1)
    └─ Index = 100 (RSP - sem índice)
    └─ Base  = 100 (RSP)

0x08: Deslocamento de 8 bits (disp8)

Resultado: MOV [RSP + 8], RAX
```

### Exemplo 3: JE label (short jump)

**Código de máquina**: `0x74 0x10` (pular 16 bytes à frente)

**Decodificação**:

```
0x74: Opcode JE rel8 (short jump if equal)
0x10: Deslocamento de 16 bytes à frente

Cálculo: RIP após instrução + 0x10
```

## 10. Aplicações Práticas {#aplicacoes-praticas}

### 10.1 Disassemblador Simples em C

```c
#include <stdio.h>
#include <stdint.h>

typedef struct {
    uint8_t bytes[15];
    int length;
    char mnemonic[32];
} instruction_t;

const char* reg_names[16] = {
    "RAX", "RCX", "RDX", "RBX",
    "RSP", "RBP", "RSI", "RDI",
    "R8",  "R9",  "R10", "R11",
    "R12", "R13", "R14", "R15"
};

void decode_instruction(uint8_t *code, instruction_t *inst) {
    int pos = 0;
    int rex = 0;
    int rex_w = 0, rex_r = 0, rex_b = 0;
    
    // Verifica prefixo REX (0x40-0x4F)
    if (code[pos] >= 0x40 && code[pos] <= 0x4F) {
        rex = code[pos++];
        rex_w = (rex >> 3) & 1;
        rex_r = (rex >> 2) & 1;
        rex_b = rex & 1;
        printf("REX prefix: W=%d, R=%d, B=%d\n", rex_w, rex_r, rex_b);
    }
    
    // Analisa opcode
    uint8_t opcode = code[pos++];
    
    switch (opcode) {
        case 0x89: // MOV r/m64, r64
            printf("MOV instruction detected\n");
            if (pos < 15) {
                uint8_t modrm = code[pos];
                int mod = (modrm >> 6) & 3;
                int reg = ((modrm >> 3) & 7) + (rex_r << 3);
                int rm = (modrm & 7) + (rex_b << 3);
                
                if (mod == 3) {
                    printf("MOV %s, %s\n", reg_names[rm], reg_names[reg]);
                }
            }
            break;
            
        case 0xE8: // CALL rel32
            {
                int32_t disp = *(int32_t*)(code + pos);
                printf("CALL 0x%x\n", disp);
            }
            break;
            
        // Outros opcodes...
    }
}

int main() {
    // Exemplo: MOV RAX, RBX (48 89 D8)
    uint8_t code[] = {0x48, 0x89, 0xD8};
    instruction_t inst;
    
    decode_instruction(code, &inst);
    return 0;
}
```

### 10.2 Gerador de Opcodes em Python

```python
#!/usr/bin/env python3
"""
Gerador de opcodes x86-64 para Linux
"""

class X86OpcodeGenerator:
    def __init__(self):
        self.rex = 0
        self.bytes = []
        
    def set_rex(self, w=0, r=0, x=0, b=0):
        """Configura prefixo REX"""
        self.rex = 0x40 | (w << 3) | (r << 2) | (x << 1) | b
        return self
        
    def add_opcode(self, opcode):
        """Adiciona byte de opcode"""
        if self.rex:
            self.bytes.append(self.rex)
            self.rex = 0
        self.bytes.append(opcode)
        return self
        
    def add_modrm(self, mod, reg, rm):
        """Adiciona byte ModR/M"""
        self.bytes.append((mod << 6) | ((reg & 7) << 3) | (rm & 7))
        return self
        
    def add_sib(self, scale, index, base):
        """Adiciona byte SIB"""
        self.bytes.append((scale << 6) | ((index & 7) << 3) | (base & 7))
        return self
        
    def add_disp(self, value, size=4):
        """Adiciona deslocamento"""
        for i in range(size):
            self.bytes.append((value >> (i*8)) & 0xFF)
        return self
        
    def add_imm(self, value, size=4):
        """Adiciona imediato"""
        for i in range(size):
            self.bytes.append((value >> (i*8)) & 0xFF)
        return self
        
    def generate(self):
        """Retorna bytes da instrução"""
        return bytes(self.bytes)
        
    def hexdump(self):
        """Retorna representação hexadecimal"""
        return ' '.join(f'{b:02x}' for b in self.bytes)

# Exemplos de uso
if __name__ == "__main__":
    # ADD RAX, RBX
    inst1 = (X86OpcodeGenerator()
             .set_rex(w=1)
             .add_opcode(0x01)
             .add_modrm(3, 3, 0))  # mod=11, reg=RBX(3), rm=RAX(0)
    print(f"ADD RAX, RBX: {inst1.hexdump()}")
    
    # MOV [RSP+8], RAX
    inst2 = (X86OpcodeGenerator()
             .set_rex(w=1)
             .add_opcode(0x89)
             .add_modrm(1, 0, 4)   # mod=01, reg=RAX, rm=100(SIB)
             .add_sib(0, 4, 4)      # scale=0, index=RSP(4), base=RSP(4)
             .add_disp(8, 1))       # disp8 = 8
    print(f"MOV [RSP+8], RAX: {inst2.hexdump()}")
    
    # CALL 0x1000 (relativo)
    inst3 = (X86OpcodeGenerator()
             .add_opcode(0xE8)
             .add_imm(0x1000, 4))   # rel32
    print(f"CALL 0x1000: {inst3.hexdump()}")
```

### 10.3 Script para Análise de Binários

```bash
#!/bin/bash
# analisa_opcodes.sh - Analisa opcodes em um binário

if [ $# -eq 0 ]; then
    echo "Uso: $0 <arquivo_binario>"
    exit 1
fi

BINARIO=$1

echo "=== Análise de Opcodes: $BINARIO ==="
echo

# Mostra cabeçalho ELF
echo "--- Cabeçalho ELF ---"
readelf -h $BINARIO 2>/dev/null || file $BINARIO

echo
echo "--- Segmentos de Código ---"
objdump -h $BINARIO | grep -E "\.text|\.code" 2>/dev/null

echo
echo "--- Primeiras 64 instruções ---"
objdump -d $BINARIO | head -n 100 | grep -E "^[[:space:]]+[0-9a-f]+:" | head -64

echo
echo "--- Estatísticas de Opcodes ---"
echo "Instruções com REX prefix:"
objdump -d $BINARIO | grep -E "^[[:space:]]+[0-9a-f]+:[[:space:]]+40|41|42|43|44|45|46|47|48|49|4a|4b|4c|4d|4e|4f" | wc -l

echo "Instruções de CALL:"
objdump -d $BINARIO | grep -E "^[[:space:]]+[0-9a-f]+:[[:space:]]+e8" | wc -l

echo "Instruções de MOV:"
objdump -d $BINARIO | grep -E "^[[:space:]]+[0-9a-f]+:[[:space:]]+89|8b|c6|c7" | wc -l
```

### 10.4 Padrões de Opcodes Comuns em Linux

```nasm
; Syscall (padrão mais comum em Linux)
0x0f 0x05                    ; syscall

; Function prologue (início de função)
0x55                          ; push rbp
0x48 0x89 0xe5               ; mov rbp, rsp

; Function epilogue (fim de função)
0x5d                          ; pop rbp
0xc3                          ; ret

; NOP multi-byte (para alinhamento)
0x66 0x66 0x66 0x66 0x90     ; múltiplos NOPs
0x0f 0x1f 0x00               ; 3-byte NOP (0f 1f /0) 
0x0f 0x1f 0x40 0x00          ; 4-byte NOP
0x0f 0x1f 0x44 0x00 0x00     ; 5-byte NOP

; Teste de zero (comum em otimizações)
0x48 0x85 0xc0               ; test rax, rax (melhor que cmp rax,0)
0x74 0x??                    ; je label (se zero)
```

## Conclusão

O entendimento profundo dos opcodes x86-64 permite:

1. **Otimização de código**: Escolher as formas mais curtas/eficientes
2. **Engenharia reversa**: Entender binários sem documentação
3. **Depuração avançada**: Analisar crashes em nível de instrução
4. **Desenvolvimento de ferramentas**: Disassembladores, emuladores, debuggers
5. **Exploração de segurança**: Análise de exploits e shellcodes

### Referências para Aprofundamento

- **Intel® 64 and IA-32 Architectures Software Developer's Manual Vol. 2**: A especificação definitiva 
- **System V AMD64 ABI**: Convenções de chamada Linux
- **x86-opcode-map.txt**: Mapa de opcodes do kernel Linux 
