# **MANUAL COMPLETO: SINTAXE DO ASSEMBLY X86-64 LINUX**

## **SUMÁRIO**
1. [Introdução: A Linguagem da Máquina](#1-introdução-a-linguagem-da-máquina)
2. [Estrutura Básica de um Programa](#2-estrutura-básica-de-um-programa)
3. [Seções do Programa](#3-seções-do-programa)
4. [Diretivas vs Instruções](#4-diretivas-vs-instruções)
5. [Declaração de Dados](#5-declaração-de-dados)
6. [Instruções Fundamentais](#6-instruções-fundamentais)
7. [Modos de Endereçamento](#7-modos-de-endereçamento)
8. [Registradores em Detalhe](#8-registradores-em-detalhe)
9. [Fluxo de Controle](#9-fluxo-de-controle)
10. [Chamadas de Sistema (Syscalls)](#10-chamadas-de-sistema-syscalls)
11. [Funções e Convensões de Chamada](#11-funções-e-convensões-de-chamada)
12. [Macros e Constantes](#12-macros-e-constantes)
13. [Tratamento de Strings](#13-tratamento-de-strings)
14. [Operações Aritméticas](#14-operações-aritméticas)
15. [Depuração e Boas Práticas](#15-depuração-e-boas-práticas)

---

## **1. INTRODUÇÃO: A LINGUAGEM DA MÁQUINA**

### **1.1 O que é Assembly?**

Assembly é a representação legível por humanos da linguagem de máquina. Cada instrução assembly corresponde diretamente a uma instrução que o processador executa.

```assembly
; Código C:
; int a = 5 + 3;

; Assembly correspondente:
mov eax, 5     ; Coloca 5 no registrador EAX
add eax, 3     ; Soma 3 ao EAX
mov [a], eax   ; Armazena resultado na variável 'a'
```

**POR QUE ISSO ACONTECE:**
- O processador só entende binário (0s e 1s)
- Assembly é uma abstração que torna esses binários legíveis
- Cada mnemônico (como `mov`, `add`) representa um código de operação (opcode) específico

### **1.2 Por que x86-64?**

```assembly
; x86-64 é a arquitetura de 64 bits da Intel/AMD
; Características principais:
; - Registradores de 64 bits
; - Espaço de endereçamento de 64 bits (16 exabytes teóricos)
; - Compatibilidade retroativa com código de 32 bits

; Exemplo: mesmo código funciona em 32 e 64 bits?
; 32 bits:
mov eax, 42    ; OK em 32 bits
; 64 bits:
mov rax, 42    ; Só em 64 bits
mov eax, 42    ; Também funciona em 64 bits!
```

**ASSOCIAÇÃO:** 
Pense no x86-64 como um prédio de 64 andares (64 bits) que ainda tem acesso aos 32 primeiros andares (compatibilidade com 32 bits).

---

## **2. ESTRUTURA BÁSICA DE UM PROGRAMA**

### **2.1 O Programa Mais Simples**

```assembly
; Arquivo: hello.asm
; Compilar: nasm -f elf64 hello.asm -o hello.o
; Linkeditar: ld hello.o -o hello
; Executar: ./hello

section .data
    mensagem db 'Olá, Mundo!', 10  ; 10 = newline (LF)
    tam equ $ - mensagem             ; Tamanho calculado

section .text
    global _start                    ; Ponto de entrada

_start:
    ; Escrever mensagem no terminal
    mov rax, 1                       ; syscall: write
    mov rdi, 1                       ; file descriptor: stdout
    mov rsi, mensagem                 ; buffer
    mov rdx, tam                      ; tamanho
    syscall                           ; chama kernel

    ; Sair do programa
    mov rax, 60                       ; syscall: exit
    xor rdi, rdi                       ; status: 0
    syscall
```

**POR QUE CADA PARTE EXISTE:**
- `section .data`: Dados inicializados (como variáveis globais em C)
- `section .text`: Código executável
- `global _start`: Torna _start visível para o linkeditor (ponto de entrada)
- `syscall`: Instrução que transfere controle para o kernel Linux

### **2.2 Anatomia de uma Instrução**

```assembly
; Formato geral: [rótulo:] mnemônico operando1, operando2  ; comentário

inicio:          ; Rótulo (label) - endereço marcado
    mov rax, 10  ; Mnemônico (mov) + operandos (rax, 10)
    add rbx, rax ; Mais dois operandos
    ; ^ comentário explicativo
```

**ASSOCIAÇÃO:** 
Instruções assembly são como verbos em uma receita: "PEGUE farinha (mov rax, farinha)", "MISTURE com ovos (add rbx, rax)".

---

## **3. SEÇÕES DO PROGRAMA**

### **3.1 As Três Seções Principais**

```assembly
; .data - Dados inicializados (como variáveis globais com valor inicial)
section .data
    var1 db 100                    ; byte inicializado
    var2 dw 1000                    ; word (2 bytes)
    var3 dd 100000                  ; dword (4 bytes)
    var4 dq 10000000000             ; qword (8 bytes)
    str db "texto", 0               ; string null-terminada
    array times 10 db 0             ; 10 bytes zerados

; .bss - Dados não inicializados (economiza espaço no executável)
section .bss
    buffer resb 1024                ; reserva 1024 bytes
    ponteiro resq 1                  ; reserva 1 qword (8 bytes)
    matriz resd 100                  ; reserva 100 dwords

; .text - Código executável
section .text
    global _start
_start:
    ; código aqui
```

**POR QUE .data e .bss são separados:**
- `.data`: Ocupa espaço no arquivo executável (valores iniciais salvos)
- `.bss`: Apenas reserva espaço, não ocupa espaço no arquivo
- Isso economiza espaço em disco e torna o executável mais enxuto

### **3.2 Acesso a Dados nas Seções**

```assembly
section .data
    numero dq 42

section .bss
    resultado resq 1

section .text
global _start
_start:
    ; Acessando .data
    mov rax, [numero]      ; RAX = 42
    
    ; Escrevendo em .bss
    mov qword [resultado], 100
    
    ; .text é somente leitura (geralmente)
    ; Tentar escrever em .text causa segmentation fault
```

---

## **4. DIRETIVAS VS INSTRUÇÕES**

### **4.1 Diferença Fundamental**

```assembly
; DIRETIVAS: instruções para o montador (assembler)
; - Começam com ponto (.) ou são palavras reservadas
; - Não geram código de máquina diretamente
; - Controlam o processo de montagem

section .data           ; Diretiva: define seção
    msg db "Olá"        ; Diretiva: define byte
    len equ $-msg       ; Diretiva: define constante

; INSTRUÇÕES: comandos para o processador
; - Geram código de máquina
; - Executadas em tempo de execução

section .text
    mov rax, 1          ; Instrução: move 1 para RAX
    add rbx, rax        ; Instrução: soma
    syscall             ; Instrução: chamada de sistema
```

**POR QUE A DIFERENÇA IMPORTA:**
- Diretivas são processadas durante a montagem (compile time)
- Instruções são executadas quando o programa roda (runtime)
- Misturar os conceitos causa erros de compilação ou comportamento inesperado

### **4.2 Principais Diretivas**

```assembly
; Constantes (equ)
TAMANHO equ 100         ; Similar a #define em C
PI equ 3.14159

; Definição de dados
db 10                   ; Define Byte (8 bits)
dw 1000                 ; Define Word (16 bits)
dd 100000               ; Define Double word (32 bits)
dq 10000000000          ; Define Quad word (64 bits)
dt 3.14                 ; Define Ten bytes (80 bits - float extendido)

; Múltiplas definições
bytes: db 1, 2, 3, 4    ; Sequência de bytes
string: db 'Hello', 0   ; String com terminador zero
padding: times 10 db 0  ; 10 bytes zeros (repetição)

; Alinhamento
align 8                 ; Alinha próximo dado em endereço múltiplo de 8
    dq 100              ; Garantido estar alinhado
```

---

## **5. DECLARAÇÃO DE DADOS**

### **5.1 Tipos de Dados Primitivos**

```assembly
section .data
    ; Tipos inteiros
    b8  db  100         ; 1 byte  (0-255)
    b16 dw  1000        ; 2 bytes (0-65535)
    b32 dd  100000      ; 4 bytes (0-4.29e9)
    b64 dq  1000000000  ; 8 bytes (0-1.84e19)
    
    ; Números negativos (complemento de 2)
    negativo db -50     ; Montador converte automaticamente
    
    ; Caracteres
    char db 'A'         ; ASCII 65
    newline db 10       ; Line feed (LF)
    tab db 9            ; Tabulação
    
    ; Strings
    str1 db "Olá", 0    ; C-style string (null-terminated)
    str2 db 'Mundo', 10 ; Com newline no final
    str3 db "Linha 1", 10, "Linha 2", 10, 0  ; Multi-linha
    
    ; Arrays
    array1 db 1, 2, 3, 4, 5           ; 5 bytes
    array2 times 100 db 0              ; 100 bytes zerados
    array3 dw 10, 20, 30, 40           ; 4 words (8 bytes)
```

**POR QUE DIFERENTES TAMANHOS:**
- Economia de memória: usar 1 byte quando 8 são desnecessários
- Alinhamento: alguns dados precisam estar em endereços específicos
- Precisão: números maiores precisam de mais bytes

### **5.2 Números de Ponto Flutuante**

```assembly
section .data
    ; Precisão simples (32 bits)
    float32 dd 3.14159
    
    ; Precisão dupla (64 bits)
    float64 dq 2.71828
    
    ; Precisão extendida (80 bits)
    float80 dt 1.414213562
    
    ; Arrays de floats
    vetor dd 1.0, 2.0, 3.0, 4.0
    
    ; Constantes científicas
    gravidade dq 9.80665
    velocidade_luz dd 299792458.0

section .text
    ; Para usar floats, precisamos das instruções SSE/x87
    movsd xmm0, [float64]    ; Carrega double em registrador SSE
    addsd xmm0, xmm1         ; Soma doubles
```

---

## **6. INSTRUÇÕES FUNDAMENTAIS**

### **6.1 Instruções de Movimentação**

```assembly
section .data
    valor dq 100
    destino dq 0

section .text
global _start
_start:
    ; MOV - a instrução mais básica
    mov rax, 50              ; Imediato para registrador
    mov rbx, rax             ; Registrador para registrador
    mov [destino], rax       ; Registrador para memória
    mov rcx, [valor]         ; Memória para registrador
    
    ; MOV com extensão de sinal/zero
    movzx rbx, byte [valor]  ; Move byte com zero extension
    movsx rcx, byte [valor]  ; Move byte com sign extension
    
    ; XCHG - troca valores
    xchg rax, rbx            ; Troca RAX e RBX
    
    ; LEA - Load Effective Address (muito útil!)
    lea rsi, [valor]         ; RSI = endereço de valor (não o conteúdo!)
    lea rdx, [rax + rbx*4]   ; Calcula endereço complexo
```

**POR QUE LEA É ESPECIAL:**
- LEA calcula endereços sem acessar memória
- Pode fazer aritmética complexa em uma única instrução
- Útil para indexação de arrays: `lea rax, [rbx + rcx*4]`

### **6.2 Instruções Aritméticas**

```assembly
section .data
    a dq 10
    b dq 3

section .text
; Adição e Subtração
mov rax, [a]
add rax, [b]         ; RAX = 13
sub rax, 5           ; RAX = 8
inc rax              ; RAX = 9 (incremento)
dec rax              ; RAX = 8 (decremento)

; Multiplicação
mov rax, 5
mov rbx, 3
mul rbx              ; Multiplica RAX por RBX, resultado em RDX:RAX
                     ; (RDX:RAX = 15, RDX=0, RAX=15)

; Multiplicação com sinal
mov rax, -5
mov rbx, 3
imul rbx             ; RDX:RAX = -15

; Divisão
mov rax, 15
xor rdx, rdx         ; Zera RDX (importante!)
mov rbx, 4
div rbx              ; Divide RDX:RAX por RBX
                     ; RAX = quociente (3), RDX = resto (3)

; Divisão com sinal
mov rax, -15
cqo                  ; Extende sinal de RAX para RDX (importante!)
mov rbx, 4
idiv rbx             ; RAX = -3, RDX = -3
```

**POR QUE RDX É USADO NA MULTIPLICAÇÃO/DIVISÃO:**
- Multiplicação de dois números de 64 bits pode resultar em 128 bits
- O par RDX:RAX forma um número de 128 bits
- Divisão precisa do dividendo de 128 bits em RDX:RAX

### **6.3 Instruções Lógicas e de Bits**

```assembly
; AND, OR, XOR, NOT
mov rax, 0b1100
and rax, 0b1010      ; RAX = 0b1000 (8)
or rax, 0b0101       ; RAX = 0b1101 (13)
xor rax, rax         ; Zera RAX (maneira eficiente!)
not rax              ; Inverte todos os bits

; Test (AND sem modificar operandos)
test rax, rax        ; Verifica se RAX é zero (seta flags)
jz zero_label        ; Pula se zero

; Shifts e Rotações
shl rax, 2           ; Shift left: RAX << 2 (multiplica por 4)
shr rax, 1           ; Shift right: RAX >> 1 (divide por 2)
sal rax, 2           ; Shift aritmético esquerdo (igual SHL)
sar rax, 1           ; Shift aritmético direito (preserva sinal)

; Rotações
rol rax, 3           ; Rotaciona bits para esquerda
ror rax, 2           ; Rotaciona bits para direita

; Exemplo prático: multiplicar por 10 sem MUL
mov rax, 7
shl rax, 1           ; RAX = 14 (7*2)
mov rbx, rax
shl rax, 2           ; RAX = 56 (14*4)
add rax, rbx         ; RAX = 70 (56+14) = 7*10
```

**POR QUE USAR SHIFTS EM VEZ DE MULTIPLICAÇÃO:**
- Shifts são mais rápidos que multiplicação
- Útil em multiplicações por potências de 2
- Ótimo para manipulação de bits em flags

---

## **7. MODOS DE ENDEREÇAMENTO**

### **7.1 Os 6 Modos Principais**

```assembly
section .data
    array dq 10, 20, 30, 40
    index dq 2

section .text
global _start
_start:
    ; 1. Imediato: o valor está na instrução
    mov rax, 42               ; 42 é imediato
    
    ; 2. Registrador: valor está em registrador
    mov rbx, rax              ; Fonte é registrador
    
    ; 3. Direto (absoluto): endereço fixo
    mov rcx, [array]          ; Acessa memória no endereço 'array'
    
    ; 4. Indireto: registrador contém endereço
    mov rsi, array            ; RSI = endereço de array
    mov rdx, [rsi]            ; Acessa memória apontada por RSI
    
    ; 5. Base + deslocamento
    mov rax, [rsi + 8]        ; Acessa array[1] (8 bytes depois)
    
    ; 6. Indexado (base + índice * escala + deslocamento)
    mov rcx, [index]          ; RCX = 2
    mov rdx, [array + rcx*8]  ; Acessa array[2] = 30
    
    ; Combinações complexas
    lea rax, [rbx + rcx*4 + 16]  ; RAX = RBX + RCX*4 + 16
```

**POR QUE MÚLTIPLOS MODOS:**
- Flexibilidade para acessar diferentes estruturas de dados
- Arrays: modo indexado é perfeito
- Structs: base + deslocamento acessa campos
- Pilha: base + deslocamento acessa variáveis locais

### **7.2 Exemplos Práticos**

```assembly
; Acessando array 2D (matriz)
; matriz[linha][coluna] onde cada elemento tem 4 bytes
; Endereço = base + (linha * colunas + coluna) * 4

section .data
    matriz dd 1, 2, 3, 4
            dd 5, 6, 7, 8
    colunas equ 4

section .text
    ; Acessar matriz[1][2] (deveria ser 7)
    mov rax, 1                ; linha
    mov rbx, colunas          ; colunas
    mul rbx                   ; rax = linha * colunas
    add rax, 2                ; + coluna
    shl rax, 2                ; * 4 (tamanho)
    
    mov rcx, [matriz + rax]   ; rcx = 7
```

---

## **8. REGISTRADORES EM DETALHE**

### **8.1 Registradores de Propósito Geral**

```assembly
; 64 bits | 32 bits | 16 bits | 8 bits (high) | 8 bits (low)
; RAX     | EAX     | AX      | AH            | AL
; RBX     | EBX     | BX      | BH            | BL
; RCX     | ECX     | CX      | CH            | CL
; RDX     | EDX     | DX      | DH            | DL
; RSI     | ESI     | SI      | SIL           |
; RDI     | EDI     | DI      | DIL           |
; RBP     | EBP     | BP      | BPL           |
; RSP     | ESP     | SP      | SPL           |
; R8      | R8D     | R8W     | R8B           |
; ...
; R15     | R15D    | R15W    | R15B          |

section .text
    ; Acessando partes diferentes
    mov rax, 0x123456789ABCDEF0
    ; RAX completo: 0x123456789ABCDEF0
    ; EAX: 0x9ABCDEF0 (32 bits baixos)
    ; AX: 0xDEF0 (16 bits baixos)
    ; AH: 0xDE (8 bits altos de AX)
    ; AL: 0xF0 (8 bits baixos de AX)
    
    ; Cuidado: escrever em EAX zera os 32 bits altos de RAX!
    mov eax, 0xFFFFFFFF        ; RAX = 0x00000000FFFFFFFF
```

**POR QUE REGISTRADORES TÊM PARTES:**
- Compatibilidade com código antigo (16/32 bits)
- Economia em operações com dados pequenos
- Acesso rápido a bytes individuais

### **8.2 Registradores Especiais**

```assembly
; RSP - Stack Pointer (pilha)
push rax              ; Diminui RSP em 8, guarda RAX
pop rbx               ; Carrega RBX, aumenta RSP em 8

; RBP - Base Pointer (frame pointer)
minha_funcao:
    push rbp          ; Salva RBP antigo
    mov rbp, rsp      ; RBP aponta para topo da pilha
    sub rsp, 32       ; Reserva espaço para variáveis locais
    ; variáveis locais em [rbp-8], [rbp-16], etc.
    mov rsp, rbp
    pop rbp
    ret

; RIP - Instruction Pointer (não pode ser acessado diretamente)
; Usado em RIP-relative addressing (comum em 64 bits)
lea rax, [rel mensagem]   ; Acessa mensagem relativo a RIP

; RFLAGS - Flags registrador
; Bits importantes:
; CF (0): Carry Flag - último carry/borrow
; PF (2): Parity Flag - paridade do resultado
; ZF (6): Zero Flag - resultado zero
; SF (7): Sign Flag - resultado negativo
; OF (11): Overflow Flag - overflow aritmético

cmp rax, rbx          ; Compara, seta flags
je igual              ; Pula se ZF=1 (igual)
jg maior              ; Pula se maior (ZF=0 e SF=OF)
```

**ASSOCIAÇÃO:** 
Registradores são como as mãos do processador. RSP/RBP são como "marcadores de página" - um aponta para o topo da pilha, outro para o começo do frame atual.

---

## **9. FLUXO DE CONTROLE**

### **9.1 Saltos Condicionais**

```assembly
section .data
    resultado db 0
    msg_maior db "eax é maior", 10, 0
    msg_menor db "eax é menor", 10, 0

section .text
global _start
_start:
    mov rax, 10
    mov rbx, 20
    
    cmp rax, rbx          ; Compara RAX e RBX
    
    ; Saltos baseados na comparação
    je  igual             ; Jump if Equal (ZF=1)
    jne diferente         ; Jump if Not Equal (ZF=0)
    jg  maior             ; Jump if Greater (ZF=0 e SF=OF)
    jge maior_ou_igual    ; Jump if Greater or Equal
    jl  menor             ; Jump if Less
    jle menor_ou_igual    ; Jump if Less or Equal
    ja  acima             ; Jump if Above (unsigned)
    jb  abaixo            ; Jump if Below (unsigned)
    
    ; Testando flags individuais
    jz  zero              ; Jump if Zero (ZF=1)
    jnz nao_zero          ; Jump if Not Zero (ZF=0)
    js  negativo          ; Jump if Sign (SF=1)
    jns positivo          ; Jump if Not Sign (SF=0)
    jc  carry             ; Jump if Carry (CF=1)
    jnc no_carry          ; Jump if Not Carry (CF=0)
    jo  overflow          ; Jump if Overflow (OF=1)
    jno no_overflow       ; Jump if Not Overflow (OF=0)

igual:
    ; código quando igual
    jmp fim               ; Salto incondicional

diferente:
    ; código quando diferente
    jmp fim

maior:
    ; código quando maior
    jmp fim

fim:
    mov rax, 60
    xor rdi, rdi
    syscall
```

**POR QUE TANTOS SALTOS CONDICIONAIS:**
- Diferentes condições para números com e sem sinal
- Otimização: cada salto testa flags específicas
- Flexibilidade para todos os casos de comparação

### **9.2 Loops**

```assembly
section .data
    array dq 10, 20, 30, 40, 50
    array_len equ ($ - array) / 8

section .text
global _start
_start:
    ; Método 1: Loop com contador decrescente
    mov rcx, array_len      ; Contador
    mov rbx, array          ; Ponteiro
    
loop1:
    cmp rcx, 0
    je fim_loop1
    
    mov rax, [rbx]          ; Processa elemento
    
    add rbx, 8              ; Próximo elemento
    dec rcx                 ; Decrementa contador
    jmp loop1
    
fim_loop1:

    ; Método 2: Loop com contador crescente
    xor rcx, rcx            ; rcx = 0 (índice)
    
loop2:
    cmp rcx, array_len
    jge fim_loop2
    
    mov rax, [array + rcx*8]  ; array[rcx]
    
    inc rcx
    jmp loop2
    
fim_loop2:

    ; Método 3: Instrução LOOP (menos usada, mais lenta)
    mov rcx, array_len
    mov rbx, array
    
loop3:
    mov rax, [rbx]
    add rbx, 8
    loop loop3              ; Decrementa RCX e pula se != 0
```

### **9.3 Switch/Case com Jump Table**

```assembly
section .data
    ; Tabela de jump (array de endereços)
    jump_table:
        dq case_0
        dq case_1
        dq case_2
        dq default_case
    
    msg_0 db "Caso 0", 10, 0
    msg_1 db "Caso 1", 10, 0
    msg_2 db "Caso 2", 10, 0
    msg_default db "Default", 10, 0

section .text
global _start
_start:
    mov rax, 1              ; Valor do switch (0-3)
    
    ; Verifica se está no range
    cmp rax, 2
    ja use_default          ; Se > 2, vai para default
    
    ; Jump via tabela
    jmp [jump_table + rax*8]

case_0:
    mov rsi, msg_0
    call print_string
    jmp fim_switch

case_1:
    mov rsi, msg_1
    call print_string
    jmp fim_switch

case_2:
    mov rsi, msg_2
    call print_string
    jmp fim_switch

default_case:
    mov rsi, msg_default
    call print_string

fim_switch:
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall

print_string:
    ; Assume RSI aponta para string null-terminada
    push rsi
    push rcx
    
    mov rdi, 1              ; stdout
    mov rdx, 0              ; contador
    
.count:
    cmp byte [rsi + rdx], 0
    je .print
    inc rdx
    jmp .count
    
.print:
    mov rax, 1              ; sys_write
    syscall
    
    pop rcx
    pop rsi
    ret
```

---

## **10. CHAMADAS DE SISTEMA (SYSCALLS)**

### **10.1 Mecanismo de Syscall**

```assembly
; No Linux x86-64, syscalls usam:
; RAX = número da syscall
; RDI = primeiro argumento
; RSI = segundo argumento
; RDX = terceiro argumento
; R10 = quarto argumento
; R8 = quinto argumento
; R9 = sexto argumento

; Principais syscalls:
; 0   - sys_read
; 1   - sys_write
; 2   - sys_open
; 3   - sys_close
; 9   - sys_mmap
; 12  - sys_brk
; 60  - sys_exit

section .data
    prompt db "Digite algo: ", 0
    buffer times 256 db 0
    prompt_len equ $ - prompt - 1  ; ignora o 0

section .text
global _start
_start:
    ; sys_write - escrever prompt
    mov rax, 1              ; sys_write
    mov rdi, 1              ; stdout
    mov rsi, prompt         ; buffer
    mov rdx, prompt_len     ; tamanho
    syscall
    
    ; sys_read - ler entrada
    mov rax, 0              ; sys_read
    mov rdi, 0              ; stdin
    mov rsi, buffer         ; buffer
    mov rdx, 256            ; tamanho máximo
    syscall                 ; RAX = número de bytes lidos
    
    ; sys_write - ecoar entrada
    mov rdx, rax            ; tamanho lido
    mov rax, 1              ; sys_write
    mov rdi, 1              ; stdout
    mov rsi, buffer         ; buffer
    syscall
    
    ; sys_exit
    mov rax, 60
    xor rdi, rdi
    syscall
```

**POR QUE USAR SYSCALL:**
- Interface entre programa de usuário e kernel
- Acesso a recursos do sistema (arquivos, memória, etc.)
- Modo protegido: kernel verifica permissões

### **10.2 Trabalhando com Arquivos**

```assembly
section .data
    filename db "teste.txt", 0
    content db "Olá, arquivo!", 10, 0
    content_len equ $ - content - 1
    
    err_open db "Erro ao abrir arquivo", 10, 0
    err_write db "Erro ao escrever", 10, 0

section .text
global _start
_start:
    ; Criar/abrir arquivo
    ; sys_open: rax=2, rdi=filename, rsi=flags, rdx=mode
    mov rax, 2              ; sys_open
    mov rdi, filename       ; nome do arquivo
    mov rsi, 0x41           ; O_CREAT | O_WRONLY (0111 | 0101?) Vamos simplificar:
    ; Flags comuns: 0x0 - O_RDONLY, 0x1 - O_WRONLY, 0x2 - O_RDWR
    ; 0x40 - O_CREAT, 0x200 - O_APPEND
    ; Para criar: 0x41 = O_WRONLY | O_CREAT
    mov rdx, 0o644          ; permissões: rw-r--r-- (em octal!)
    syscall
    
    cmp rax, 0              ; Verifica erro (fd negativo)
    jl erro_abrir
    
    mov rbx, rax            ; Salva file descriptor em RBX
    
    ; Escrever no arquivo
    ; sys_write: rax=1, rdi=fd, rsi=buffer, rdx=tamanho
    mov rax, 1
    mov rdi, rbx
    mov rsi, content
    mov rdx, content_len
    syscall
    
    cmp rax, 0
    jl erro_escrever
    
    ; Fechar arquivo
    ; sys_close: rax=3, rdi=fd
    mov rax, 3
    mov rdi, rbx
    syscall
    
    ; Ler arquivo (exemplo)
    mov rax, 2              ; sys_open
    mov rdi, filename
    xor rsi, rsi            ; O_RDONLY = 0
    syscall
    
    mov rbx, rax            ; fd
    
    ; sys_read
    mov rax, 0
    mov rdi, rbx
    mov rsi, buffer
    mov rdx, 256
    syscall
    
    ; sys_write para stdout
    mov rdx, rax
    mov rax, 1
    mov rdi, 1
    mov rsi, buffer
    syscall
    
    ; Fechar
    mov rax, 3
    mov rdi, rbx
    syscall
    
    jmp fim

erro_abrir:
    mov rsi, err_open
    call print_string
    jmp fim

erro_escrever:
    mov rsi, err_write
    call print_string

fim:
    mov rax, 60
    xor rdi, rdi
    syscall

print_string:
    ; implementação como antes
    ret
```

---

## **11. FUNÇÕES E CONVENÇÕES DE CHAMADA**

### **11.1 System V AMD64 ABI (padrão Linux)**

```assembly
; Convenção de chamada:
; - Primeiros 6 argumentos: RDI, RSI, RDX, RCX, R8, R9
; - Argumentos adicionais na pilha
; - RAX contém valor de retorno
; - RBX, RBP, R12-R15 são preservados (callee-saved)
; - R10, R11 são temporários (caller-saved)
; - RSP deve ser mantido alinhado a 16 bytes

section .text
global _start

; Função que soma dois números
; Entrada: RDI = a, RSI = b
; Saída: RAX = a + b
soma:
    push rbp                ; Salva RBP (callee-saved)
    mov rbp, rsp            ; Frame pointer
    
    mov rax, rdi
    add rax, rsi
    
    pop rbp                 ; Restaura RBP
    ret                     ; Retorna

; Função que soma três números
; Entrada: RDI=a, RSI=b, RDX=c
soma_tres:
    push rbp
    mov rbp, rsp
    push rbx                ; Salva RBX (callee-saved)
    
    mov rax, rdi
    add rax, rsi
    add rax, rdx
    
    pop rbx                 ; Restaura RBX
    pop rbp
    ret

; Função com variáveis locais
calc_media:
    ; Entrada: RDI = array, RSI = tamanho
    ; Saída: XMM0 = média (double)
    push rbp
    mov rbp, rsp
    sub rsp, 32             ; Espaço para variáveis locais
    
    ; [rbp-8] = soma (double)
    ; [rbp-16] = contador (quad)
    
    mov qword [rbp-16], 0   ; contador = 0
    pxor xmm0, xmm0          ; soma = 0.0
    movsd [rbp-8], xmm0
    
.loop:
    mov rcx, [rbp-16]
    cmp rcx, rsi
    jge .fim
    
    ; Converte int para double e soma
    mov eax, [rdi + rcx*4]   ; assume array de ints 32-bit
    cvtsi2sd xmm1, eax
    addsd xmm0, [rbp-8]
    movsd [rbp-8], xmm0
    
    inc qword [rbp-16]
    jmp .loop
    
.fim:
    ; Divide pela quantidade
    cvtsi2sd xmm1, rsi
    divsd xmm0, xmm1
    
    mov rsp, rbp
    pop rbp
    ret

_start:
    ; Chamando soma
    mov rdi, 5
    mov rsi, 3
    call soma                ; RAX = 8
    
    ; Chamando soma_tres
    mov rdi, 10
    mov rsi, 20
    mov rdx, 30
    call soma_tres           ; RAX = 60
    
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall
```

### **11.2 Funções Recursivas**

```assembly
; Fatorial recursivo
; Entrada: RDI = n
; Saída: RAX = n!
fatorial:
    push rbp
    mov rbp, rsp
    
    cmp rdi, 1
    jle .base_case           ; Se n <= 1, retorna 1
    
    push rdi                 ; Salva n (caller-saved)
    dec rdi                  ; n-1
    call fatorial            ; fatorial(n-1)
    pop rdi                  ; Restaura n
    
    mul rdi                  ; RAX = (n-1)! * n
    jmp .fim
    
.base_case:
    mov rax, 1
    
.fim:
    pop rbp
    ret

; Fibonacci recursivo
; Entrada: RDI = n
; Saída: RAX = fib(n)
fibonacci:
    push rbp
    mov rbp, rsp
    push rbx                 ; Salva RBX
    
    cmp rdi, 1
    jle .base_case
    
    push rdi                 ; Salva n
    
    dec rdi                  ; n-1
    call fibonacci
    mov rbx, rax             ; RBX = fib(n-1)
    
    pop rdi
    sub rdi, 2               ; n-2
    call fibonacci
    
    add rax, rbx             ; fib(n-1) + fib(n-2)
    jmp .fim
    
.base_case:
    mov rax, rdi             ; fib(0)=0, fib(1)=1
    
.fim:
    pop rbx
    pop rbp
    ret
```

**POR QUE PRESERVAR REGISTRADORES:**
- RBX é callee-saved: função chamada deve preservar
- Se não preservar, função chamadora pode ter seu valor corrompido
- Convenção permite código confiável e reutilizável

---

## **12. MACROS E CONSTANTES**

### **12.1 Definindo Macros Simples**

```assembly
; Definindo constantes com %define
%define SYS_EXIT 60
%define SYS_WRITE 1
%define STDIN 0
%define STDOUT 1

; Macros com parâmetros
%define PRINT_STRING(str) mov rsi, str; call print_string
%define MOV_REG(val, reg) mov reg, val

; Macro multi-linha com %macro
%macro PRINT 1
    section .data
        %%msg db %1, 10, 0     ; %% para label local
        %%len equ $ - %%msg
    section .text
        mov rax, SYS_WRITE
        mov rdi, STDOUT
        mov rsi, %%msg
        mov rdx, %%len
        syscall
%endmacro

%macro FUNC_START 0
    push rbp
    mov rbp, rsp
    sub rsp, 32
%endmacro

%macro FUNC_END 0
    mov rsp, rbp
    pop rbp
    ret
%endmacro

section .data
    msg db "Olá", 0

section .text
global _start

_start:
    ; Usando macros
    PRINT "Hello, World!"
    
    MOV_REG(42, rax)          ; mov rax, 42
    
    ; Chamando função com macros
    call minha_funcao
    
    mov rax, SYS_EXIT
    xor rdi, rdi
    syscall

minha_funcao:
    FUNC_START
    
    ; corpo da função
    
    FUNC_END
```

**POR QUE USAR MACROS:**
- Reduz repetição de código
- Facilita manutenção (mudar em um lugar só)
- Pode gerar código mais eficiente que funções (sem overhead de chamada)

### **12.2 Macros Condicionais**

```assembly
; Montagem condicional
%define DEBUG 1

%if DEBUG
    %macro DEBUG_PRINT 1
        section .data
            %%dbg_msg db "[DEBUG] ", %1, 10, 0
            %%dbg_len equ $ - %%dbg_msg
        section .text
            push rax
            push rdi
            push rsi
            push rdx
            mov rax, 1
            mov rdi, 1
            mov rsi, %%dbg_msg
            mov rdx, %%dbg_len
            syscall
            pop rdx
            pop rsi
            pop rdi
            pop rax
    %endmacro
%else
    %define DEBUG_PRINT(str) ; nada (macro vazia)
%endif

; Macros para diferentes arquiteturas
%ifidn __OUTPUT_FORMAT__, elf64
    %define SYS_CALL syscall
%elifidn __OUTPUT_FORMAT__, elf32
    %define SYS_CALL int 0x80
%endif

section .text
global _start

_start:
    DEBUG_PRINT "Iniciando programa"
    
    mov rax, 10
    mov rbx, 20
    add rax, rbx
    
    DEBUG_PRINT "Soma calculada"
    
    ; SYS_CALL será expandido conforme arquitetura
    mov rax, 60
    xor rdi, rdi
    SYS_CALL
```

---

## **13. TRATAMENTO DE STRINGS**

### **13.1 Operações Básicas com Strings**

```assembly
section .data
    str1 db "Hello", 0
    str2 db "World", 0
    buffer times 256 db 0
    
    msg_igual db "Strings iguais", 10, 0
    msg_diferente db "Strings diferentes", 10, 0

section .text
global _start

; Copiar string (strcpy)
; Entrada: RDI = destino, RSI = origem
strcpy:
    push rbp
    mov rbp, rsp
    
.loop:
    mov al, [rsi]
    mov [rdi], al
    inc rsi
    inc rdi
    cmp al, 0
    jne .loop
    
    pop rbp
    ret

; Tamanho da string (strlen)
; Entrada: RDI = string
; Saída: RAX = tamanho
strlen:
    push rbp
    mov rbp, rsp
    push rcx
    
    xor rax, rax            ; contador = 0
    mov rcx, rdi            ; RCX = ponteiro
    
.loop:
    cmp byte [rcx], 0
    je .fim
    inc rax
    inc rcx
    jmp .loop
    
.fim:
    pop rcx
    pop rbp
    ret

; Comparar strings (strcmp)
; Entrada: RDI = str1, RSI = str2
; Saída: RAX = 0 se iguais, !=0 se diferentes
strcmp:
    push rbp
    mov rbp, rsp
    
.loop:
    mov al, [rdi]
    mov bl, [rsi]
    
    cmp al, bl
    jne .diferentes
    
    cmp al, 0               ; Fim da string?
    je .iguais
    
    inc rdi
    inc rsi
    jmp .loop
    
.diferentes:
    mov rax, 1
    jmp .fim
    
.iguais:
    xor rax, rax
    
.fim:
    pop rbp
    ret

; Concatenar strings (strcat)
; Entrada: RDI = destino, RSI = origem
strcat:
    push rbp
    mov rbp, rsp
    push rdi
    
    ; Encontrar fim da string destino
    xor rax, rax
    mov rcx, -1
    repne scasb             ; Enquanto não achar 0
    
    dec rdi                 ; Volta para o 0
    
    ; Copiar origem para o fim
    mov rsi, [rbp + 16]     ; Recupera RSI original
    call strcpy
    
    pop rdi
    pop rbp
    ret

_start:
    ; Exemplo de uso
    mov rdi, buffer
    mov rsi, str1
    call strcpy              ; buffer = "Hello"
    
    mov rdi, buffer
    call strlen              ; RAX = 5
    
    mov rdi, str1
    mov rsi, str2
    call strcmp
    
    cmp rax, 0
    je .sao_iguais
    
    PRINT_STRING msg_diferente
    jmp .fim
    
.sao_iguais:
    PRINT_STRING msg_igual
    
.fim:
    mov rax, 60
    xor rdi, rdi
    syscall
```

**POR QUE USAR INSTRUÇÕES ESPECIAIS:**
- `repne scasb` é uma instrução dedicada para busca de caracteres
- Mais eficiente que loops manuais
- O processador otimiza internamente

### **13.2 Conversão Número-String**

```assembly
section .data
    numero dq 12345
    buffer times 32 db 0
    newline db 10

section .text
global _start

; Converter inteiro para string (itoa)
; Entrada: RAX = número, RDI = buffer
; Saída: RDI aponta para fim da string
int_to_string:
    push rbp
    mov rbp, rsp
    push rbx
    push rcx
    push rdx
    
    mov rbx, 10             ; Divisor
    mov rcx, rdi            ; Guarda início do buffer
    add rdi, 31             ; Vai para o fim do buffer
    mov byte [rdi], 0       ; Terminador null
    
    ; Se número for 0, caso especial
    cmp rax, 0
    jne .loop
    
    mov byte [rdi-1], '0'
    sub rdi, 1
    jmp .reverse
    
.loop:
    xor rdx, rdx            ; Zera RDX para divisão
    div rbx                 ; RAX / 10, quociente em RAX, resto em RDX
    
    add dl, '0'             ; Converte para caractere
    dec rdi
    mov [rdi], dl
    
    cmp rax, 0
    jne .loop
    
.reverse:
    ; RDI aponta para início da string
    ; RCX aponta para início do buffer original
    ; Não precisamos reverter se usamos o método acima
    
    pop rdx
    pop rcx
    pop rbx
    pop rbp
    ret

; Converter string para inteiro (atoi)
; Entrada: RDI = string
; Saída: RAX = número
string_to_int:
    push rbp
    mov rbp, rsp
    push rbx
    push rcx
    
    xor rax, rax            ; resultado = 0
    xor rcx, rcx            ; sinal = 1 (positivo)
    
    ; Verificar sinal
    cmp byte [rdi], '-'
    jne .loop
    mov rcx, -1             ; sinal = -1
    inc rdi                 ; Pula o '-'
    
.loop:
    movzx rbx, byte [rdi]
    cmp bl, 0               ; Fim da string?
    je .fim
    
    sub bl, '0'             ; Converte para número
    cmp bl, 9
    ja .fim                 ; Não é dígito, sai
    
    imul rax, 10            ; resultado *= 10
    add rax, rbx            ; resultado += dígito
    
    inc rdi
    jmp .loop
    
.fim:
    cmp rcx, 0
    je .positivo
    neg rax                 ; Se negativo, inverte sinal
    
.positivo:
    pop rcx
    pop rbx
    pop rbp
    ret

_start:
    ; Converter número para string
    mov rax, [numero]
    mov rdi, buffer
    call int_to_string
    
    ; RDI agora aponta para string
    ; Imprimir
    mov rsi, rdi
    call print_string
    
    ; Converter string para número
    mov rdi, buffer
    call string_to_int      ; RAX = 12345
    
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall
```

---

## **14. OPERAÇÕES ARITMÉTICAS AVANÇADAS**

### **14.1 Aritmética de Múltipla Precisão**

```assembly
section .data
    ; Números de 128 bits representados como dois qwords
    num1_low  dq 0xFFFFFFFFFFFFFFFF
    num1_high dq 0x0000000000000001  ; num1 = 0x0000000000000001FFFFFFFFFFFFFFFF
    num2_low  dq 0x0000000000000001
    num2_high dq 0x0000000000000000  ; num2 = 1
    result_low dq 0
    result_high dq 0

section .text
; Soma de 128 bits
soma_128:
    ; Entrada: RDI = low1, RSI = high1, RDX = low2, RCX = high2
    ; Saída: RAX = low_result, RBX = high_result
    
    mov rax, rdi
    add rax, rdx            ; Soma low parts
    mov rbx, rsi
    adc rbx, rcx            ; Soma high parts com carry
    
    ret

; Multiplicação de 64 bits produzindo 128 bits
mult_64_128:
    ; Entrada: RDI = a, RSI = b
    ; Saída: RDX:RAX = a * b
    
    mov rax, rdi
    mul rsi                 ; Multiplica, resultado em RDX:RAX
    ret

_start:
    ; Exemplo de soma 128 bits
    mov rdi, [num1_low]
    mov rsi, [num1_high]
    mov rdx, [num2_low]
    mov rcx, [num2_high]
    call soma_128
    
    mov [result_low], rax
    mov [result_high], rbx
    
    ; Exemplo de multiplicação
    mov rdi, 0xFFFFFFFF
    mov rsi, 0xFFFFFFFF
    call mult_64_128        ; RDX:RAX = 0xFFFFFFFE00000001
```

**POR QUE ADC (ADD WITH CARRY) É IMPORTANTE:**
- Permite somar números maiores que os registradores
- O carry propaga o "vai-um" entre as operações
- Essencial para criptografia e matemática de precisão arbitrária

### **14.2 Operações com Ponto Flutuante**

```assembly
section .data
    ; Dados para exemplos
    a dq 3.14
    b dq 2.71
    c dd 1.41
    
    vetor1 dq 1.0, 2.0, 3.0, 4.0
    vetor2 dq 5.0, 6.0, 7.0, 8.0
    resultado dq 0.0

section .text
global _start

; Exemplo com SSE (recomendado para 64 bits)
exemplo_sse:
    ; Carregar doubles
    movsd xmm0, [a]         ; xmm0 = 3.14
    movsd xmm1, [b]         ; xmm1 = 2.71
    
    ; Operações básicas
    addsd xmm0, xmm1        ; xmm0 = 3.14 + 2.71
    subsd xmm0, xmm1        ; subtrai
    mulsd xmm0, xmm1        ; multiplica
    divsd xmm0, xmm1        ; divide
    
    ; Raiz quadrada
    sqrtsd xmm2, xmm0       ; xmm2 = sqrt(xmm0)
    
    ; Comparação
    ucomisd xmm0, xmm1      ; Compara xmm0 com xmm1
    ja xmm0_maior           ; Pula se xmm0 > xmm1 (unsigned)
    
    ; Operações com vetores (SIMD)
    movupd xmm0, [vetor1]   ; Carrega 2 doubles (xmm0 = [v1[1], v1[0]])
    movupd xmm1, [vetor2]   ; Carrega 2 doubles
    addpd xmm0, xmm1        ; Soma paralela de 2 doubles
    
    ; Armazenar resultado
    movsd [resultado], xmm0
    
xmm0_maior:
    ret

; Exemplo com x87 FPU (legado)
exemplo_x87:
    finit                   ; Inicializa FPU
    
    fld qword [a]           ; Carrega a na pilha FPU (st0 = a)
    fld qword [b]           ; st0 = b, st1 = a
    
    fadd                    ; st0 = st0 + st1 (b + a)
    fsub                    ; st0 = st0 - st1
    fmul                    ; st0 = st0 * st1
    fdiv                    ; st0 = st0 / st1
    
    fstp qword [resultado]  ; Armazena e pop
    ret

_start:
    call exemplo_sse
    
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall
```

---

## **15. DEPURAÇÃO E BOAS PRÁTICAS**

### **15.1 Técnicas de Depuração**

```assembly
; 1. PRINT DEBUG com syscall
%macro DEBUG_REG 1
    section .data
        %%msg db "Registro: ", 0
        %%newline db 10
    section .text
        push rax
        push rdi
        push rsi
        push rdx
        
        ; Imprime mensagem
        mov rax, 1
        mov rdi, 1
        mov rsi, %%msg
        mov rdx, 10
        syscall
        
        ; Converte registrador para string e imprime
        mov rax, %1
        mov rdi, rsp
        sub rsp, 32
        call int_to_string
        call print_string
        
        ; Newline
        mov rax, 1
        mov rsi, %%newline
        mov rdx, 1
        syscall
        
        add rsp, 32
        pop rdx
        pop rsi
        pop rdi
        pop rax
%endmacro

; 2. Breakpoint em software
%macro BREAKPOINT 0
    xchg bx, bx             ; Magic breakpoint para alguns debuggers
%endmacro

; 3. Verificação de overflow
%macro CHECK_OVERFLOW 2
    ; %1 = operação, %2 = label de erro
    jo %2
%endmacro

; 4. Assertions
%macro ASSERT 2
    ; %1 = condição, %2 = mensagem
    j%+1 .assert_ok
    PRINT_STRING %2
    mov rax, 60
    mov rdi, 1
    syscall
.assert_ok:
%endmacro

section .data
    valor dq 100
    msg_assert db "Assertion failed!", 10, 0

section .text
global _start

_start:
    DEBUG_REG rax           ; Mostra valor de RAX
    
    BREAKPOINT              ; Ponto de parada
    
    mov rax, [valor]
    add rax, 200
    CHECK_OVERFLOW add, overflow_error
    
    ASSERT e, msg_assert    ; Verifica se zero (propositalmente falha)
    
    jmp fim

overflow_error:
    PRINT_STRING "Overflow detectado!"
    
fim:
    mov rax, 60
    xor rdi, rdi
    syscall
```

### **15.2 Boas Práticas**

```assembly
; 1. Comente SEMPRE o propósito dos registradores
minha_funcao:
    ; RDI = parâmetro 1 (tamanho)
    ; RSI = parâmetro 2 (buffer)
    ; RAX = valor de retorno
    
; 2. Use nomes significativos para labels
; RUIM:
l1: 
    add rax, rbx
    jmp l2

; BOM:
calc_soma:
    add rax, rbx
    jmp fim_calculo

; 3. Mantenha alinhamento consistente
section .data
    var1    db  10          ; Alinhado
    var2    dw  1000        ; Alinhado
    ponteiro dq 0           ; Alinhado

; 4. Inicialize sempre registradores antes de usar
    ; RUIM:
    add rax, 10     ; RAX pode conter lixo
    
    ; BOM:
    xor rax, rax    ; Zera RAX
    add rax, 10

; 5. Use constantes em vez de números mágicos
%define MAX_BUFFER 1024
%define SYS_EXIT 60

; 6. Proteja registradores em funções
minha_funcao_segura:
    push rbx        ; Salva registradores que serão modificados
    push r12
    
    ; ... código que usa rbx e r12 ...
    
    pop r12         ; Restaura na ordem inversa
    pop rbx
    ret
```

### **15.3 Padrões Comuns de Código**

```assembly
; Padrão: Loop com contador
    mov rcx, 10
.loop:
    ; código
    dec rcx
    jnz .loop

; Padrão: Loop com ponteiro
    mov rsi, array
    mov rcx, tamanho
.loop:
    lodsd           ; Carrega dword de [rsi] em eax, incrementa rsi
    ; processa eax
    loop .loop

; Padrão: If-then-else
    cmp rax, rbx
    jg .maior
    ; código para menor/igual
    jmp .fim
.maior:
    ; código para maior
.fim:

; Padrão: Switch com jump table
    jmp [jump_table + rax*8]

; Padrão: Chamada de função
    push rbp
    mov rbp, rsp
    sub rsp, 16     ; espaço para variáveis locais
    ; código
    mov rsp, rbp
    pop rbp
    ret

; Padrão: Tratamento de erro
    call operacao
    test rax, rax
    jz .sucesso
    ; tratamento de erro
.sucesso:
```

---

