# 📖 Guia Completo de Assembly no Linux
## Do Zero ao Avançado

## 📌 Sumário
1. [Introdução](#introdução)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Arquitetura x86-64](#arquitetura-x86-64)
4. [Sintaxe Básica](#sintaxe-básica)
5. [Primeiro Programa](#primeiro-programa)
6. [Instruções Fundamentais](#instruções-fundamentais)
7. [Trabalhando com Dados](#trabalhando-com-dados)
8. [Controle de Fluxo](#controle-de-fluxo)
9. [Funções e Procedimentos](#funções-e-procedimentos)
10. [Syscalls do Linux](#syscalls-do-linux)
11. [Avançado: Otimização](#avançado-otimização)
12. [Projetos Práticos](#projetos-práticos)

---

## 1️⃣ Introdução

### O que é Assembly?
Assembly é uma **linguagem de baixo nível** que tem uma correspondência quase direta com o código de máquina. Cada instrução Assembly representa uma instrução que a CPU entende.

### Por que aprender Assembly?
- ✅ Entender como computadores funcionam no nível mais baixo
- ✅ Escrever código altamente otimizado
- ✅ Depurar programas complexos
- ✅ Trabalhar com sistemas embarcados e drivers
- ✅ Entender vulnerabilidades de segurança

### Diferenças entre sintaxes
No Linux, usamos principalmente duas sintaxes:
- **AT&T** (padrão do GNU): `movl %eax, %ebx`
- **Intel** (mais comum): `mov ebx, eax`

Vamos usar **sintaxe Intel** neste guia por ser mais legível.

---

## 2️⃣ Configuração do Ambiente

### Instalando ferramentas necessárias
```bash
# No Ubuntu/Debian
sudo apt update
sudo apt install nasm gdb gcc build-essential

# No Fedora
sudo dnf install nasm gdb gcc

# No Arch
sudo pacman -S nasm gdb gcc
```

### Ferramentas principais:
- **nasm**: Assembler para sintaxe Intel
- **gdb**: Debugger para análise passo a passo
- **gcc**: Compilador C (útil para linking)
- **ld**: Linker do sistema

### Verificando a instalação:
```bash
nasm --version
gdb --version
```

---

## 3️⃣ Arquitetura x86-64

### Registradores de 64 bits
```
Registradores de propósito geral:
RAX - Acumulador (retorno de funções)
RBX - Base (preservado)
RCX - Contador (loops)
RDX - Dados (extensão de RAX)
RSI - Source Index (ponteiro origem)
RDI - Destination Index (ponteiro destino)
RBP - Base Pointer (frame base)
RSP - Stack Pointer (topo da pilha)

Registradores de 32 bits (parte baixa dos 64 bits):
EAX, EBX, ECX, EDX, ESI, EDI, EBP, ESP

Registradores de 16 bits:
AX, BX, CX, DX, SI, DI, BP, SP

Registradores de 8 bits:
AL (low), AH (high) para AX
BL, BH para BX, etc.
```

### Flags (registrador RFLAGS)
Bits importantes:
- **CF** (0): Carry Flag - overflow aritmético
- **ZF** (6): Zero Flag - resultado zero
- **SF** (7): Sign Flag - resultado negativo
- **OF** (11): Overflow Flag - overflow signed

### Memória e Endereçamento
```
Layout de memória típico:
0x0000000000000000 - Text (código)
0x00007FFFFFFFFFFF - Heap (cresce para cima)
                   - Stack (cresce para baixo)
0x7FFFFFFFFFFFFFFF - Kernel
```

---

## 4️⃣ Sintaxe Básica

### Estrutura de um programa
```assembly
section .data           ; Dados inicializados
    mensagem db "Olá Mundo!", 10, 0
    
section .bss            ; Dados não-inicializados
    buffer resb 64
    
section .text           ; Código
    global _start       ; Ponto de entrada
    
_start:
    ; Seu código aqui
```

### Diretivas comuns
```assembly
db  - Define byte (8 bits)
dw  - Define word (16 bits)
dd  - Define doubleword (32 bits)
dq  - Define quadword (64 bits)
resb - Reserva bytes
equ  - Define constante
times - Repete instrução/diretiva
```

### Comentários
```assembly
; Comentário de uma linha
mov rax, 1  ; Comentário após instrução

/*
  Comentários multi-linha
  (suportado pelo NASM)
*/
```

---

## 5️⃣ Primeiro Programa

### Hello World completo
```assembly
section .data
    msg db 'Hello, World!', 0xA  ; 0xA = nova linha
    len equ $ - msg               ; Calcula tamanho

section .text
    global _start

_start:
    ; syscall: write(1, msg, len)
    mov rax, 1          ; syscall number (write)
    mov rdi, 1          ; file descriptor (stdout)
    mov rsi, msg        ; endereço da mensagem
    mov rdx, len        ; tamanho da mensagem
    syscall             ; chama kernel

    ; syscall: exit(0)
    mov rax, 60         ; syscall number (exit)
    mov rdi, 0          ; status code
    syscall
```

### Compilando e executando
```bash
# Montar o código
nasm -f elf64 hello.asm -o hello.o

# Linkar o objeto
ld hello.o -o hello

# Executar
./hello

# Alternativa com gcc (para programas que usam libc)
# nasm -f elf64 hello.asm -o hello.o
# gcc -no-pie hello.o -o hello
```

### Debugando com GDB
```bash
# Compilar com símbolos de debug
nasm -f elf64 -g -F dwarf hello.asm -o hello.o
ld hello.o -o hello

# Iniciar GDB
gdb ./hello

# Comandos úteis do GDB:
#   break _start     - Define breakpoint
#   run             - Executa programa
#   stepi           - Executa uma instrução
#   info registers  - Mostra registradores
#   x/10x $rsp      - Examina memória
#   continue        - Continua execução
```

---

## 6️⃣ Instruções Fundamentais

### Movimentação de dados
```assembly
; Formas básicas
mov rax, 42           ; Immediato → Registrador
mov rax, rbx          ; Registrador → Registrador
mov rax, [var]        ; Memória → Registrador
mov [var], rax        ; Registrador → Memória

; Exemplos com tamanhos diferentes
mov al, 0xFF          ; 8 bits
mov ax, 0xFFFF        ; 16 bits  
mov eax, 0xFFFFFFFF   ; 32 bits
mov rax, 0xFFFFFFFFFFFFFFFF  ; 64 bits
```

### Aritmética básica
```assembly
; Adição e subtração
add rax, rbx    ; rax = rax + rbx
sub rax, rcx    ; rax = rax - rcx
inc rax         ; rax = rax + 1
dec rbx         ; rbx = rbx - 1

; Multiplicação e divisão
mul rbx         ; rax = rax * rbx (unsigned)
imul rax, rbx   ; rax = rax * rbx (signed)
div rbx         ; rax = rax / rbx, rdx = resto
idiv rbx        ; Signed division

; Negação e complemento
neg rax         ; rax = -rax
not rax         ; rax = ~rax (complemento bitwise)
```

### Operações lógicas e bitwise
```assembly
; Operações lógicas
and rax, rbx    ; rax = rax & rbx
or  rax, rbx    ; rax = rax | rbx
xor rax, rbx    ; rax = rax ^ rbx
test rax, rbx   ; rax & rbx (seta flags)

; Shifts
shl rax, 3      ; Shift left lógico (×2³)
shr rax, 2      ; Shift right lógico (÷2²)
sal rax, 1      ; Shift left aritmético
sar rax, 4      ; Shift right aritmético (preserva sinal)

; Rotação
rol rax, 8      ; Rotate left
ror rax, 8      ; Rotate right
```

### Comparações
```assembly
cmp rax, rbx    ; Compara rax com rbx
; Flags são ajustadas baseadas em (rax - rbx)

; Após cmp, use jumps condicionais:
je  label       ; Jump if equal (ZF=1)
jne label       ; Jump if not equal (ZF=0)
jg  label       ; Jump if greater (signed)
jl  label       ; Jump if less (signed)
ja  label       ; Jump if above (unsigned)
jb  label       ; Jump if below (unsigned)
```

---

## 7️⃣ Trabalhando com Dados

### Tipos de dados
```assembly
section .data
    ; Números
    byte1    db  0xFF           ; 8 bits
    word1    dw  0xFFFF         ; 16 bits
    dword1   dd  0xFFFFFFFF     ; 32 bits  
    qword1   dq  0xFFFFFFFFFFFFFFFF  ; 64 bits
    
    ; Ponto flutuante (IEEE 754)
    float1   dd  3.14159        ; Single precision (32-bit)
    double1  dq  2.718281828459045  ; Double precision (64-bit)
    
    ; Strings
    str1     db  'Hello', 0     ; Null-terminated
    str2     db  'World', 10, 0 ; Com nova linha
    str3     db  'Linha1', 10, 'Linha2', 10, 0
    
    ; Arrays
    array    db  1, 2, 3, 4, 5  ; 5 bytes
    words    dw  100, 200, 300  ; 3 words (6 bytes)
    
    ; Constantes
    CONST    equ 100
    BUFSIZE  equ 1024
```

### Acesso a arrays e estruturas
```assembly
section .data
    array dd 10, 20, 30, 40, 50
    tamanho equ ($ - array) / 4  ; Número de elementos
    
section .text
    ; Acessando array[index]
    mov rsi, array      ; Endereço base
    mov ecx, 2          ; Índice (0-based)
    mov eax, [rsi + rcx*4]  ; eax = array[2] (30)
    
    ; Estrutura exemplo: struct { int id; char name[20]; }
    struc Pessoa
        .id    resd 1   ; 4 bytes
        .nome  resb 20  ; 20 bytes
        .idade resb 1   ; 1 byte
    endstruc
    
    ; Instanciando
    pessoa1:
        istruc Pessoa
            at Pessoa.id, dd 1001
            at Pessoa.nome, db 'João Silva', 0
            at Pessoa.idade, db 30
        iend
```

### Stack (Pilha)
```assembly
; A pilha cresce para baixo!
; RSP aponta para o topo da pilha

; Empilhando (push)
push rax        ; Equivalente a:
                ; sub rsp, 8
                ; mov [rsp], rax

; Desempilhando (pop)  
pop rbx         ; Equivalente a:
                ; mov rbx, [rsp]
                ; add rsp, 8

; Acesso direto à pilha
mov rax, [rsp]          ; Topo da pilha
mov rbx, [rsp + 8]      ; Segundo elemento
mov [rsp - 16], rcx     ; Aloca espaço na pilha
```

### Modos de endereçamento
```assembly
; 1. Imediato
mov rax, 42

; 2. Registrador
mov rax, rbx

; 3. Direto (memória)
mov rax, [var]

; 4. Indireto por registrador
mov rax, [rbx]          ; rbx contém endereço

; 5. Base + Índice
mov rax, [rbx + rcx*4]  ; Array de inteiros
                        ; rbx = base, rcx = índice

; 6. Deslocamento
mov rax, [rbx + 16]     ; Acesso a campo de struct

; 7. Relativo ao RIP (64-bit only)
mov rax, [rel var]      ; Endereçamento relativo
lea rax, [rel var]      ; Carrega endereço efetivo
```

---

## 8️⃣ Controle de Fluxo

### Condicionais
```assembly
; Comparação simples
cmp rax, rbx
je igual        ; if (rax == rbx)
jne diferente   ; if (rax != rbx)

; Maior/menor (signed)
cmp rax, rbx
jg maior        ; if (rax > rbx)
jge maior_igual ; if (rax >= rbx)
jl menor        ; if (rax < rbx)
jle menor_igual ; if (rax <= rbx)

; Maior/menor (unsigned)
cmp rax, rbx
ja acima        ; if (rax > rbx)
jae acima_igual ; if (rax >= rbx)
jb abaixo       ; if (rax < rbx)
jbe abaixo_igual; if (rax <= rbx)

; Combinação de condições
cmp rax, 0
jl negativo
je zero
jg positivo
```

### Loops
```assembly
; Loop com contador
mov rcx, 10          ; Contador
loop_start:
    ; Corpo do loop
    dec rcx          ; rcx--
    jnz loop_start   ; if (rcx != 0) goto loop_start

; Loop while
while_loop:
    cmp rax, 0
    je end_while
    ; Corpo do loop
    jmp while_loop
end_while:

; Loop infinito (com break condicional)
infinite_loop:
    ; Faz algo
    cmp rbx, 100
    jge break_loop   ; if (rbx >= 100) break
    jmp infinite_loop
break_loop:
```

### Tabela de saltos (switch)
```assembly
; Exemplo: switch(rax)
cmp rax, 0
je case0
cmp rax, 1
je case1
cmp rax, 2
je case2
jmp default

case0:
    ; Código case 0
    jmp end_switch
case1:
    ; Código case 1
    jmp end_switch
case2:
    ; Código case 2
    jmp end_switch
default:
    ; Código default
end_switch:

; Usando tabela de saltos
section .data
    jump_table dq case0, case1, case2

section .text
    cmp rax, 2
    ja default
    jmp [jump_table + rax*8]
```

---

## 9️⃣ Funções e Procedimentos

### Convenções de chamada (System V ABI)
Para x86-64 no Linux:
- **Primeiros 6 argumentos inteiros**: RDI, RSI, RDX, RCX, R8, R9
- **Argumentos adicionais**: Stack (direita para esquerda)
- **Retorno inteiro**: RAX (e RDX para 128-bit)
- **Registradores preservados**: RBX, RBP, R12-R15
- **Registradores scratch**: RAX, RCX, RDX, RSI, RDI, R8-R11
- **Alinhamento de stack**: 16-byte antes de call

### Função simples
```assembly
section .text

; Função: soma dois números
; Entrada: rdi = a, rsi = b
; Saída:  rax = a + b
global soma
soma:
    push rbp           ; Salva base pointer
    mov rbp, rsp       ; Novo frame
    
    mov rax, rdi       ; rax = a
    add rax, rsi       ; rax = a + b
    
    pop rbp            ; Restaura base pointer
    ret                ; Retorna (rax contém resultado)

; Função com argumentos na stack
global funcao_complexa
funcao_complexa:
    push rbp
    mov rbp, rsp
    
    ; Acessando argumentos (após return address)
    ; +8:  return address
    ; +16: primeiro arg na stack
    ; +24: segundo arg na stack, etc.
    mov rax, [rbp + 16]  ; Primeiro argumento
    
    ; Alocando variáveis locais
    sub rsp, 32          ; 32 bytes para vars locais
    mov [rsp], rdi       ; Salva rdi localmente
    
    ; Restaurando stack
    mov rsp, rbp
    pop rbp
    ret
```

### Recursão
```assembly
; Fatorial recursivo
; Entrada: rdi = n
; Saída:  rax = n!
global fatorial
fatorial:
    push rbp
    mov rbp, rsp
    
    ; Caso base: if (n <= 1) return 1
    cmp rdi, 1
    jg .recursivo
    mov rax, 1
    jmp .fim
    
.recursivo:
    push rdi            ; Salva n
    dec rdi             ; n-1
    call fatorial       ; fatorial(n-1)
    pop rdi             ; Restaura n
    imul rax, rdi       ; n * fatorial(n-1)
    
.fim:
    mov rsp, rbp
    pop rbp
    ret
```

### Funções variádicas
```assembly
; Função que soma N números
; Entrada: rdi = quantidade, ... números
global soma_n
soma_n:
    push rbp
    mov rbp, rsp
    
    xor rax, rax        ; rax = 0 (acumulador)
    test rdi, rdi
    jz .fim             ; Se quantidade = 0
    
    ; Primeiros 6 argumentos em registradores
    cmp rdi, 1
    jl .apos_reg
    add rax, rsi        ; arg2
    cmp rdi, 2
    jl .apos_reg
    add rax, rdx        ; arg3
    ; ... continuar para rcx, r8, r9
    
.apos_reg:
    ; Argumentos restantes na stack
    mov rcx, rdi
    sub rcx, 6          ; Quantidade na stack
    jle .fim
    
    mov rsi, rbp
    add rsi, 16         ; Pula RBP e return address
    
.stack_loop:
    add rax, [rsi]
    add rsi, 8
    loop .stack_loop
    
.fim:
    pop rbp
    ret
```

---

## 🔟 Syscalls do Linux

### Tabela de syscalls importantes
```
Número | Nome    | RDI       | RSI       | RDX       | Descrição
-------|---------|-----------|-----------|-----------|-----------
0      | read    | fd        | buf       | count     | Ler dados
1      | write   | fd        | buf       | count     | Escrever dados
2      | open    | filename  | flags     | mode      | Abrir arquivo
3      | close   | fd        | -         | -         | Fechar arquivo
60     | exit    | status    | -         | -         | Terminar processo
9      | mmap    | addr      | length    | prot      | Mapear memória
11     | munmap  | addr      | length    | -         | Desmapear memória
12     | brk     | brk       | -         | -         | Alterar heap
39     | getpid  | -         | -         | -         | Obter PID
56     | clone   | flags     | stack     | ptid      | Criar thread
```

### Exemplos de syscalls
```assembly
; Ler entrada do usuário
section .bss
    buffer resb 256

section .text
global _start
_start:
    ; read(0, buffer, 256)
    mov rax, 0          ; syscall read
    mov rdi, 0          ; stdin
    mov rsi, buffer
    mov rdx, 256
    syscall
    
    ; write(1, buffer, rax)  ; rax contém bytes lidos
    mov rdx, rax        ; quantidade lida
    mov rax, 1          ; syscall write
    mov rdi, 1          ; stdout
    mov rsi, buffer
    syscall
    
    ; exit(0)
    mov rax, 60
    mov rdi, 0
    syscall

; Abrir e ler arquivo
abrir_arquivo:
    ; open(filename, O_RDONLY, 0)
    mov rax, 2          ; syscall open
    mov rdi, filename
    mov rsi, 0          ; O_RDONLY
    mov rdx, 0          ; mode
    syscall
    ; rax contém file descriptor (ou erro se negativo)
    ret

; Criar processo filho
fork_example:
    mov rax, 57         ; syscall fork
    syscall
    test rax, rax
    jz child_process    ; if (rax == 0) child
    ; Parent code here
    ret

child_process:
    ; Child code here
    ret
```

### Manipulação de arquivos
```assembly
section .data
    filename db "teste.txt", 0
    conteudo db "Hello File!", 10
    len equ $ - conteudo
    
    ; Flags para open
    O_RDONLY  equ 0
    O_WRONLY  equ 1
    O_RDWR    equ 2
    O_CREAT   equ 64
    O_APPEND  equ 1024
    O_TRUNC   equ 512
    
    ; Permissões
    S_IRUSR equ 400o    ; Read owner
    S_IWUSR equ 200o    ; Write owner
    S_IXUSR equ 100o    ; Execute owner

section .text
criar_arquivo:
    ; open("teste.txt", O_CREAT|O_WRONLY, S_IRUSR|S_IWUSR)
    mov rax, 2                  ; open
    mov rdi, filename
    mov rsi, O_CREAT|O_WRONLY   ; flags
    mov rdx, S_IRUSR|S_IWUSR    ; mode
    syscall
    
    mov [fd], rax               ; Salva file descriptor
    
    ; write(fd, conteudo, len)
    mov rax, 1                  ; write
    mov rdi, [fd]
    mov rsi, conteudo
    mov rdx, len
    syscall
    
    ; close(fd)
    mov rax, 3                  ; close
    mov rdi, [fd]
    syscall
    ret

section .bss
    fd resq 1
```

### Sockets e rede
```assembly
; Números de syscall para rede
; 41 - socket, 42 - connect, 43 - accept
; 44 - sendto, 45 - recvfrom, 49 - bind
; 50 - listen, 51 - getsockname

client_tcp:
    ; socket(AF_INET, SOCK_STREAM, 0)
    mov rax, 41                 ; socket
    mov rdi, 2                  ; AF_INET
    mov rsi, 1                  ; SOCK_STREAM
    mov rdx, 0                  ; protocol
    syscall
    
    mov [sockfd], rax
    
    ; Preparar estrutura sockaddr_in
    mov word [sockaddr], 2      ; AF_INET
    mov word [sockaddr+2], 0x901F ; Port 8080 (0x1F90 big-endian)
    mov dword [sockaddr+4], 0x100007F ; 127.0.0.1
    
    ; connect(sockfd, &sockaddr, 16)
    mov rax, 42                 ; connect
    mov rdi, [sockfd]
    mov rsi, sockaddr
    mov rdx, 16                 ; sizeof(sockaddr_in)
    syscall
    
    ; Enviar dados
    mov rax, 44                 ; sendto (ou write)
    mov rdi, [sockfd]
    mov rsi, message
    mov rdx, msg_len
    mov r10, 0                  ; flags
    mov r8, 0                   ; dest_addr (NULL)
    mov r9, 0                   ; addr_len (0)
    syscall
    
    ret

section .data
    message db "GET / HTTP/1.1", 13, 10, 13, 10
    msg_len equ $ - message

section .bss
    sockfd resq 1
    sockaddr resb 16
```

---

## 1️⃣1️⃣ Avançado: Otimização

### Instruções SIMD (SSE/AVX)
```assembly
; SSE (128-bit)
movaps xmm0, [array1]      ; Aligned packed single
addps xmm0, [array2]       ; Soma 4 floats de uma vez
mulps xmm0, xmm1           ; Multiplica 4 floats
sqrtps xmm0, xmm0          ; Raiz quadrada de 4 floats

; AVX (256-bit)
vmovaps ymm0, [array1]     ; 8 floats (256 bits)
vaddps ymm0, ymm0, [array2]
vmulps ymm0, ymm0, ymm1

; AVX-512 (512-bit)
vmovaps zmm0, [array1]     ; 16 floats (512 bits)
vaddps zmm0, zmm0, [array2]

; Exemplo: soma de array com SSE
section .data
    align 16
    array1 dd 1.0, 2.0, 3.0, 4.0
    array2 dd 5.0, 6.0, 7.0, 8.0
    result dd 0.0, 0.0, 0.0, 0.0

section .text
soma_sse:
    movaps xmm0, [array1]
    addps xmm0, [array2]
    movaps [result], xmm0
    ret
```

### Otimizações de performance
```assembly
; 1. Alinhamento de dados
section .data align=16
    aligned_data dq 0, 0, 0, 0

; 2. Prefetch (pré-busca de dados)
prefetchnta [array + 256]  ; Non-temporal
prefetcht0 [array + 512]   ; All cache levels
prefetcht1 [array + 768]
prefetcht2 [array + 1024]

; 3. Evitar stalls (paradas)
; Ruim:
mov rax, [mem_lenta]
add rbx, rax  ; Espera mem_lenta

; Bom:
mov rcx, [outra_mem]
mov rax, [mem_lenta]
add rbx, rcx  ; Executa enquanto mem_lenta carrega
add rbx, rax

; 4. Loop unrolling (desenrolamento)
; Original (4 iterações):
mov rcx, 4
loop1:
    add rax, [rsi]
    add rsi, 8
    loop loop1

; Unrolled (4x):
add rax, [rsi]
add rax, [rsi + 8]
add rax, [rsi + 16]
add rax, [rsi + 24]
add rsi, 32

; 5. Branch prediction hints
; GCC: __builtin_expect()
; Assembly:
    test rax, rax
    jz .unlikely   ; Salto improvável
    ; Código provável aqui
.unlikely:
    ; Código improvável aqui
```

### Atomics e threads
```assembly
; Operações atômicas
lock add [counter], 1      ; Incremento atômico
lock xchg [lock_var], rax  ; Exchange atômico
lock cmpxchg [target], rbx ; Compare-and-swap

; Exemplo: spinlock
section .data
    spinlock dd 0

section .text
acquire_lock:
    mov eax, 1
.retry:
    xchg eax, [spinlock]   ; Tenta adquirir lock
    test eax, eax          ; Testa se já estava adquirido
    jnz .retry             ; Se sim, tenta novamente
    ret

release_lock:
    mov dword [spinlock], 0
    ret

; Barreira de memória
mfence          ; Full memory barrier
sfence          ; Store barrier
lfence          ; Load barrier
```

### Inline Assembly no C
```c
// Exemplo 1: Inline assembly básico
int soma(int a, int b) {
    int resultado;
    asm volatile (
        "add %[a], %[b]\n\t"
        : [b] "+r" (b)          // Saída+Entrada
        : [a] "r" (a)           // Entrada
        : "cc"                  // Clobbers
    );
    return b;
}

// Exemplo 2: Múltiplas saídas
void divmod(int a, int b, int *quot, int *rem) {
    asm volatile (
        "xor %%edx, %%edx\n\t"   // Clear EDX
        "div %[divisor]\n\t"
        : "=a" (*quot), "=d" (*rem)
        : "a" (a), [divisor] "r" (b)
        : "cc"
    );
}

// Exemplo 3: SSE inline
void soma_sse(float *a, float *b, float *c, int n) {
    for (int i = 0; i < n; i += 4) {
        asm volatile (
            "movaps (%[a]), %%xmm0\n\t"
            "addps (%[b]), %%xmm0\n\t"
            "movaps %%xmm0, (%[c])\n\t"
            :
            : [a] "r" (&a[i]), [b] "r" (&b[i]), [c] "r" (&c[i])
            : "xmm0", "memory"
        );
    }
}
```

---

## 1️⃣2️⃣ Projetos Práticos

### Projeto 1: Calculadora RPN
```assembly
section .data
    prompt db "> ", 0
    buffer times 256 db 0
    stack times 64 dq 0
    stack_ptr dq stack
    
    err_underflow db "Erro: stack underflow", 10, 0
    err_overflow  db "Erro: stack overflow", 10, 0
    
section .text
global _start
_start:
    ; Loop principal
.main_loop:
    ; Imprimir prompt
    mov rax, 1
    mov rdi, 1
    mov rsi, prompt
    mov rdx, 2
    syscall
    
    ; Ler entrada
    mov rax, 0
    mov rdi, 0
    mov rsi, buffer
    mov rdx, 256
    syscall
    
    ; Processar comando
    mov rsi, buffer
    call processar_entrada
    
    jmp .main_loop

processar_entrada:
    ; Implementar:
    ; - Números: push para stack
    ; - Operadores: pop, calcular, push resultado
    ; - Comandos: dup, swap, drop, clear
    ret

push_stack:
    ; Implementar push
    ret

pop_stack:
    ; Implementar pop
    ret
```

### Projeto 2: Jogo da Vida de Conway
```assembly
section .data
    width equ 80
    height equ 40
    cells times (width * height) db 0
    temp times (width * height) db 0
    
    ; Códigos ANSI para limpar tela e mover cursor
    clear db 27, "[2J", 27, "[H", 0
    hide_cursor db 27, "[?25l", 0
    show_cursor db 27, "[?25h", 0

section .text
global _start
_start:
    ; Configurar terminal
    call setup_terminal
    
    ; Inicializar grid aleatório
    call init_random_grid
    
.game_loop:
    ; Limpar tela
    mov rax, 1
    mov rdi, 1
    mov rsi, clear
    mov rdx, 7
    syscall
    
    ; Desenhar grid
    call draw_grid
    
    ; Calcular próxima geração
    call next_generation
    
    ; Pequena pausa
    call sleep
    
    ; Verificar tecla (non-blocking)
    call check_key
    cmp al, 'q'
    jne .game_loop
    
    ; Restaurar terminal
    call restore_terminal
    
    ; Sair
    mov rax, 60
    mov rdi, 0
    syscall

next_generation:
    ; Implementar regras do Jogo da Vida
    ; Para cada célula:
    ; 1. Contar vizinhos
    ; 2. Aplicar regras
    ; 3. Atualizar grid temporário
    ; 4. Copiar temp para cells
    ret
```

### Projeto 3: Interpretador Brainfuck
```assembly
section .data
    bf_code db "++[>++<-]>."  ; Brainfuck que imprime 'A'
    code_ptr dq bf_code
    data times 30000 db 0
    data_ptr dq data
    loop_stack times 100 dq 0
    stack_ptr dq loop_stack

section .text
global _start
_start:
    mov rsi, [code_ptr]
    mov rdi, [data_ptr]
    
.interpret_loop:
    mov al, [rsi]
    test al, al
    jz .end_program
    
    cmp al, '>'
    je .inc_ptr
    cmp al, '<'
    je .dec_ptr
    cmp al, '+'
    je .inc_byte
    cmp al, '-'
    je .dec_byte
    cmp al, '.'
    je .output
    cmp al, ','
    je .input
    cmp al, '['
    je .start_loop
    cmp al, ']'
    je .end_loop
    
.next:
    inc rsi
    jmp .interpret_loop

.inc_ptr:
    inc rdi
    jmp .next

.dec_ptr:
    dec rdi
    jmp .next

.inc_byte:
    inc byte [rdi]
    jmp .next

.dec_byte:
    dec byte [rdi]
    jmp .next

.output:
    ; write(1, rdi, 1)
    push rsi
    mov rax, 1
    mov rdi, 1
    mov rsi, rdi
    mov rdx, 1
    syscall
    pop rsi
    jmp .next

.start_loop:
    ; Implementar '['
    jmp .next

.end_loop:
    ; Implementar ']'
    jmp .next

.end_program:
    mov rax, 60
    mov rdi, 0
    syscall
```

---

## 📚 Recursos Adicionais

### Livros Recomendados
1. **"Assembly Language for x86 Processors"** - Kip Irvine
2. **"The Art of Assembly Language"** - Randall Hyde
3. **"x86-64 Assembly Language Programming with Ubuntu"** - Ed Jorgensen

### Links Úteis
- [Intel x86 Manuals](https://software.intel.com/content/www/us/en/develop/articles/intel-sdm.html)
- [System V ABI](https://github.com/hjl-tools/x86-psABI)
- [NASM Manual](https://www.nasm.us/doc/)
- [OSDev Wiki](https://wiki.osdev.org/Main_Page)

### Ferramentas
- **Radare2**: Framework completo para análise binária
- **objdump**: Desmontador de binários
- **strace**: Rastreamento de syscalls
- **ltrace**: Rastreamento de chamadas de biblioteca

### Próximos Passos
1. **Sistemas Operacionais**: Estude o kernel do Linux
2. **Reversing**: Aprenda engenharia reversa
3. **Exploits**: Entenda vulnerabilidades de segurança
4. **Drivers**: Desenvolva drivers de dispositivo
5. **Compiladores**: Crie seu próprio compilador



---
*Documentação criada para aprendizado de Assembly x86-64 no Linux. Última atualização: Fevereiro 2024.*
