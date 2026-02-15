# Manual Completo de Registradores Assembly x86-64 Linux

## Sumário
1. [Introdução aos Registradores](#introdução)
2. [Arquitetura x86-64](#arquitetura)
3. [Registradores de Propósito Geral](#registradores-propósito-geral)
4. [Convenções de Chamada Linux](#convenções-chamada)
5. [Operações Práticas](#operações-práticas)
6. [Exemplos Práticos](#exemplos-práticos)
7. [Boas Práticas e Otimizações](#boas-práticas)

## 1. Introdução aos Registradores {#introdução}

Os registradores são unidades de memória de altíssima velocidade localizadas dentro do processador. Diferentemente da memória RAM, eles não possuem endereços numéricos tradicionais - são identificados por nomes específicos como `rax`, `rbx`, `rcx`, etc . Quando você programa em assembly, está se comunicando diretamente com o processador, que já possui esses registradores definidos por arquitetura .

### Importância dos Registradores
- **Velocidade**: Acesso extremamente rápido (ciclo único do processador)
- **Proximidade da CPU**: Operações diretas sem necessidade de barramento de memória
- **Especialização**: Muitos registradores têm funções específicas na arquitetura

## 2. Arquitetura x86-64 {#arquitetura}

O x86-64 é uma extensão de 64 bits da arquitetura x86 tradicional. Esta arquitetura oferece 16 registradores de propósito geral, cada um com 64 bits de largura .

### Hierarquia de Tamanhos

Os registradores podem ser acessados em diferentes tamanhos, permitindo trabalhar com dados de 8, 16, 32 ou 64 bits :

| 64 bits | 32 bits | 16 bits | 8 bits (low) | 8 bits (high) |
|---------|---------|---------|--------------|---------------|
| `rax` | `eax` | `ax` | `al` | `ah` |
| `rbx` | `ebx` | `bx` | `bl` | `bh` |
| `rcx` | `ecx` | `cx` | `cl` | `ch` |
| `rdx` | `edx` | `dx` | `dl` | `dh` |
| `rsi` | `esi` | `si` | `sil` | - |
| `rdi` | `edi` | `di` | `dil` | - |
| `rbp` | `ebp` | `bp` | `bpl` | - |
| `rsp` | `esp` | `sp` | `spl` | - |
| `r8` | `r8d` | `r8w` | `r8b` | - |
| `r9` | `r9d` | `r9w` | `r9b` | - |

**Regra Importante**: Quando você escreve em um sub-registrador de 32 bits (como `eax`), o processador automaticamente zera os 32 bits superiores do registrador de 64 bits correspondente. Isso não acontece para escritas de 8 ou 16 bits .

## 3. Registradores de Propósito Geral {#registradores-propósito-geral}

### 3.1 Registradores Tradicionais

#### **rax** (Accumulator)
- **Função principal**: Acumulador e registrador de retorno de funções
- **Uso típico**: Operações aritméticas, retorno de valores 
- **Exemplo**: `mov rax, 10` ; valor de retorno 10

#### **rbx** (Base)
- **Função principal**: Registrador base preservado
- **Tipo**: Preservado (callee-saved) - deve ser restaurado antes do retorno 
- **Uso típico**: Armazenamento de dados que precisam persistir entre chamadas

#### **rcx** (Counter)
- **Função principal**: Contador para loops e shifts
- **Uso típico**: Instruções de repetição (loop, shift/rotate) 
- **Particularidade**: A parte baixa `cl` é usada em shifts com contador variável

#### **rdx** (Data)
- **Função principal**: Extensão de dados e terceiro argumento
- **Uso típico**: Operações de multiplicação/divisão (com rax), terceiro argumento de função 

### 3.2 Registradores de Índice

#### **rsi** (Source Index)
- **Função principal**: Índice de origem e segundo argumento
- **Uso típico**: Operações com strings (origem), segundo parâmetro de função 
- **Em syscalls**: Fonte de dados para write/read

#### **rdi** (Destination Index)
- **Função principal**: Índice de destino e primeiro argumento
- **Uso típico**: Operações com strings (destino), primeiro parâmetro de função 
- **Em syscalls**: Destino de dados/file descriptor

### 3.3 Registradores de Pilha

#### **rsp** (Stack Pointer)
- **Função principal**: Ponteiro para o topo da pilha
- **Tipo**: Preservado 
- **Regra de ouro**: NUNCA modifique diretamente sem absoluta necessidade 
- **Uso**: Gerenciamento automático via push/pop/call/ret

#### **rbp** (Base Pointer)
- **Função principal**: Ponteiro base do frame da pilha
- **Tipo**: Preservado (opcional em código otimizado) 
- **Uso**: Acesso a variáveis locais e parâmetros na pilha

### 3.4 Registradores Estendidos (r8-r15)

Registradores adicionados na arquitetura 64-bit, numerados de r8 a r15 :

| Registrador | Uso típico | Tipo |
|-------------|------------|------|
| `r8` - `r9` | 5º e 6º argumentos de função | Scratch |
| `r10`-`r11` | Registradores temporários | Scratch |
| `r12`-`r15` | Armazenamento preservado | Preservado |

## 4. Convenções de Chamada Linux (System V AMD64 ABI) {#convenções-chamada}

### 4.1 Passagem de Parâmetros

Em sistemas Linux 64-bit, a convenção de chamada segue a System V AMD64 ABI :

| Parâmetro | Registrador |
|-----------|-------------|
| 1º | `rdi` |
| 2º | `rsi` |
| 3º | `rdx` |
| 4º | `rcx` |
| 5º | `r8` |
| 6º | `r9` |
| Demais | Pilha (em ordem reversa) |

### 4.2 Classificação de Registradores

#### Registradores Scratch (voláteis)
Podem ser modificados sem necessidade de preservação:
- `rax`, `rcx`, `rdx`, `rsi`, `rdi`
- `r8`, `r9`, `r10`, `r11` 

#### Registradores Preservados
Devem ser salvos e restaurados se utilizados:
- `rbx`, `rbp`, `rsp`
- `r12`, `r13`, `r14`, `r15` 

### 4.3 Syscalls vs Funções

Para chamadas de sistema (syscalls):
- Número da syscall em `rax` 
- Parâmetros: `rdi`, `rsi`, `rdx`, `r10`, `r8`, `r9` (ordem)
- Instrução: `syscall`
- Retorno: `rax` (negativo = erro)

## 5. Operações Práticas {#operações-práticas}

### 5.1 Movimentação de Dados

#### Instrução `mov`
```nasm
mov rax, rbx           ; copia rbx para rax
mov eax, 10            ; eax = 10 (zera parte alta de rax)
mov byte [rdi], al     ; armazena 1 byte em [rdi]
mov qword [rsp+8], rax ; armazena 8 bytes na pilha
```

#### Instrução `lea` (Load Effective Address)
```nasm
lea rax, [rbx + rcx*4] ; rax = rbx + rcx*4 (cálculo aritmético)
lea rdi, [rel mensagem] ; rdi = endereço de mensagem (RIP-relative)
```

#### Movimentação com Extensão
```nasm
movzx eax, byte [rdi]  ; carrega byte, zera bits superiores
movsx rax, byte [rdi]  ; carrega byte, estende sinal
movsxd rax, ecx        ; estende sinal de 32 para 64 bits
```

### 5.2 Operações Aritméticas

```nasm
add rax, 10            ; rax = rax + 10
sub rbx, rcx           ; rbx = rbx - rcx
inc qword [rsp]        ; incrementa valor na pilha
dec r15                ; decrementa r15

; Multiplicação (resultado em rdx:rax)
mov rax, 5
mul rbx                ; rdx:rax = rax * rbx

; Divisão (rdx:rax / divisor)
xor rdx, rdx           ; zera rdx (importante!)
div rcx                ; rax = quociente, rdx = resto
```

### 5.3 Operações com Pilha

```nasm
push rbp               ; salva rbp na pilha
push 10                ; push de constante
push qword [rsi]       ; push de valor da memória

pop rax                ; recupera topo para rax
pop qword [rdi]        ; recupera para memória
```

### 5.4 Controle de Fluxo

```nasm
cmp rax, rbx           ; compara rax com rbx
je  igual              ; jump if equal
jne diferente          ; jump if not equal
jg  maior              ; jump if greater (signed)
jl  menor              ; jump if less (signed)
ja  acima              ; jump if above (unsigned)
jb  abaixo             ; jump if below (unsigned)

test rax, rax          ; testa se rax é zero
jz  zero               ; jump if zero
jnz nao_zero           ; jump if not zero

call funcao            ; chama função
jmp loop               ; jump incondicional
ret                    ; retorna de função
```

## 6. Exemplos Práticos {#exemplos-práticos}

### 6.1 Hello World (Syscall)

```nasm
section .data
    msg db "Hello World!", 0x0a
    len equ $ - msg

section .text
    global _start

_start:
    ; write(1, msg, len)
    mov rax, 1          ; syscall number for write
    mov rdi, 1          ; stdout file descriptor
    mov rsi, msg        ; pointer to message
    mov rdx, len        ; message length
    syscall

    ; exit(0)
    mov rax, 60         ; syscall number for exit
    xor rdi, rdi        ; exit code 0
    syscall
```

### 6.2 Função que Soma Dois Números

```nasm
section .text
    global soma

; int soma(int a, int b)
; a em rdi, b em rsi
soma:
    ; prólogo (opcional)
    push rbp
    mov rbp, rsp
    
    ; soma valores
    mov rax, rdi
    add rax, rsi        ; rax = a + b
    
    ; epílogo
    pop rbp
    ret
```

### 6.3 Acesso a Array

```nasm
section .data
    array dd 1, 2, 3, 4, 5
    array_len equ 5

section .text
    global soma_array

; int soma_array(int *array, int len)
; array em rdi, len em esi
soma_array:
    xor rax, rax        ; soma = 0
    xor rcx, rcx        ; i = 0
    
.loop:
    cmp ecx, esi        ; i == len?
    jge .fim
    
    add eax, [rdi + rcx*4] ; soma += array[i]
    inc rcx
    jmp .loop
    
.fim:
    ret
```

### 6.4 Manipulação de Strings

```nasm
section .data
    str1 db "Hello", 0
    str2 db "World", 0

section .text
    global strcpy

; void strcpy(char *dest, char *src)
; dest em rdi, src em rsi
strcpy:
    push rbp
    mov rbp, rsp
    push rbx
    
.copia:
    mov al, [rsi]       ; carrega caractere da origem
    mov [rdi], al       ; copia para destino
    test al, al         ; chegou ao final (null)?
    jz .fim
    inc rsi             ; próximo caractere origem
    inc rdi             ; próximo caractere destino
    jmp .copia
    
.fim:
    pop rbx
    pop rbp
    ret
```

## 7. Boas Práticas e Otimizações {#boas-práticas}

### 7.1 Uso Eficiente de Registradores

1. **Prefira 32 bits quando possível**: Instruções com registradores de 32 bits (`eax`) são mais curtas e quebram dependências 
   ```nasm
   ; Prefira
   xor eax, eax         ; 2 bytes, zera rax
   ; ao invés de
   xor rax, rax         ; 3 bytes
   ```

2. **Use `movzx` para carregar valores pequenos**: Evita dependências parciais de registrador 
   ```nasm
   movzx eax, byte [rsi]  ; carrega byte sem dependência
   ```

3. **Zero extensão automática**: Aproveite que escritas em 32 bits zeram a parte alta 
   ```nasm
   mov eax, 10          ; rax = 10 (bits 32-63 zerados)
   ```

### 7.2 Preservação de Registradores

```nasm
minha_funcao:
    push rbx            ; salva registradores preservados
    push r12
    push r13
    
    ; ... corpo da função ...
    
    pop r13             ; restaura na ordem inversa
    pop r12
    pop rbx
    ret
```

### 7.3 Alinhamento de Pilha

A pilha deve permanecer alinhada a 16 bytes antes de chamadas de função :

```nasm
minha_funcao:
    push rbp
    mov rbp, rsp
    sub rsp, 32         ; reserva espaço, mantém alinhamento
    
    call outra_funcao   ; pilha alinhada em 16 bytes
    
    leave               ; mov rsp, rbp; pop rbp
    ret
```

### 7.4 Idiomas Comuns de Otimização

```nasm
; Zerar registrador
xor eax, eax            ; mais eficiente que mov eax, 0

; Comparar com zero
test eax, eax           ; mais eficiente que cmp eax, 0

; Múltipla por 3
lea eax, [rax + rax*2]  ; sem usar mul

; Verificar bit
test eax, (1 << 5)      ; testa 5º bit
jnz bit_set
```

### 7.5 Armadilhas Comuns a Evitar

1. **Dependências parciais de registrador** :
   ```nasm
   ; Evite - cria dependência
   mov al, [rsi]        ; depende do valor anterior de rax
   add rax, rbx         ; precisa esperar al estar pronto
   
   ; Prefira - quebra dependência
   movzx eax, byte [rsi] ; zera parte alta, sem dependência
   ```

2. **Esquecer de zerar `rdx` antes de `div`** :
   ```nasm
   ; Sempre faça
   xor edx, edx
   div rcx
   ```

3. **Modificar `rsp` manualmente**:
   ```nasm
   ; NUNCA faça
   mov rsp, rax         ; perigo!
   
   ; Use sempre push/pop ou sub/add controlado
   sub rsp, 16
   ```
