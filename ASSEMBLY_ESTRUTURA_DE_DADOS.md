# **MANUAL COMPLETO: ESTRUTURAS DE DADOS EM ASSEMBLY X86-64 LINUX**

## **SUMÁRIO**
1. [Introdução e Fundamentos](#1-introdução-e-fundamentos)
2. [Arrays (Vetores)](#2-arrays-vetores)
3. [Structs (Registros)](#3-structs-registros)
4. [Linked Lists (Listas Encadeadas)](#4-linked-lists-listas-encadeadas)
5. [Stacks (Pilhas)](#5-stacks-pilhas)
6. [Trees (Árvores)](#6-trees-árvores)
7. [Exercícios Práticos](#7-exercícios-práticos)

---

## **1. INTRODUÇÃO E FUNDAMENTOS**

### **1.1 Por que Assembly?**

Assembly é a linguagem mais próxima do hardware. Entender estruturas de dados em assembly é como "ver os bastidores" de como seu computador realmente organiza e manipula dados.

### **1.2 Organização da Memória em x86-64**

```assembly
; A memória é organizada em bytes (8 bits)
; Cada posição de memória tem um endereço único
; Exemplo de declaração de dados:
section .data
    byte_var    db 42        ; 1 byte (8 bits)
    word_var    dw 1234      ; 2 bytes (16 bits)
    dword_var   dd 123456    ; 4 bytes (32 bits)
    qword_var   dq 123456789 ; 8 bytes (64 bits)
```

**POR QUE ISSO ACONTECE:**
- x86-64 é uma arquitetura de 64 bits, mas permite acesso a dados menores para eficiência
- O processador lê dados em chunks de 64 bits, mas podemos trabalhar com partes menores
- A memória é "byte-addressable" - cada byte tem seu próprio endereço

### **1.3 Registradores Importantes**

```assembly
; Registradores de propósito geral (64 bits)
; RAX, RBX, RCX, RDX, RSI, RDI, RBP, RSP, R8-R15

; Podemos acessar partes menores:
; RAX (64 bits) -> EAX (32 bits) -> AX (16 bits) -> AH/AL (8 bits)

section .text
global _start

_start:
    ; Exemplo de uso dos registradores
    mov rax, 100        ; 64 bits
    mov eax, 100        ; 32 bits (zera os 32 bits superiores!)
    mov ax, 100         ; 16 bits
    mov al, 100         ; 8 bits
```

**ASSOCIAÇÃO:** 
Pense nos registradores como "mãos" do processador - são os únicos lugares onde ele pode realmente "segurar" dados para trabalhar com eles.

---

## **2. ARRAYS (VETORES)**

### **2.1 Array Estático Unidimensional**

```assembly
section .data
    ; Declaração de array com 5 inteiros (4 bytes cada)
    meu_array: dd 10, 20, 30, 40, 50
    
    ; Cálculo do tamanho do array
    array_len equ ($ - meu_array) / 4

section .text
global _start

_start:
    ; Acessando elemento do array (índice 2 = 30)
    mov rsi, meu_array    ; RSI = endereço base do array
    mov rax, 2            ; Índice desejado
    mov rbx, 4            ; Tamanho de cada elemento (4 bytes)
    mul rbx               ; RAX = índice * tamanho = offset
    
    ; Acessando o elemento: base + offset
    mov rcx, [rsi + rax]  ; RCX = meu_array[2]
    
    ; Iterando sobre o array
    mov rcx, array_len    ; Contador = tamanho do array
    mov rbx, meu_array    ; RBX = ponteiro para início
    
loop_array:
    cmp rcx, 0
    je fim_loop
    
    mov eax, [rbx]        ; Pega elemento atual
    
    ; FAZ ALGO COM EAX...
    
    add rbx, 4            ; Avança para próximo elemento
    dec rcx               ; Decrementa contador
    jmp loop_array
    
fim_loop:
    ; Finaliza programa
    mov rax, 60           ; syscall: exit
    xor rdi, rdi          ; status: 0
    syscall
```

**POR QUE ISSO ACONTECE:**
- Arrays são blocos contíguos de memória - isso permite acesso O(1) por índice
- O cálculo `base + (índice * tamanho_elemento)` é possível justamente pela alocação contígua
- Multiplicar por 4 (tamanho de um dword) é necessário porque cada elemento ocupa 4 bytes

### **2.2 Array Bidimensional (Matriz)**

```assembly
section .data
    ; Matriz 3x3 de inteiros (9 elementos)
    ; Organização: [linha0][col0], [linha0][col1], [linha0][col2]...
    matriz: dd 1, 2, 3
            dd 4, 5, 6
            dd 7, 8, 9
    
    linhas equ 3
    colunas equ 3
    tam_elemento equ 4

section .text
global _start

_start:
    ; Acessar elemento matriz[1][2] (deveria ser 6)
    mov rsi, matriz        ; Base da matriz
    mov rax, 1             ; Índice da linha
    mov rbx, colunas       ; Número de colunas
    mul rbx                ; RAX = 1 * 3 = 3
    add rax, 2             ; + índice coluna = 5 (posição 5)
    
    ; Calcular offset: (linha * colunas + coluna) * tam_elemento
    mov rbx, tam_elemento
    mul rbx                ; RAX = 5 * 4 = 20 bytes offset
    
    ; Acessar elemento
    mov rcx, [rsi + rax]   ; RCX = matriz[1][2] = 6
```

**ASSOCIAÇÃO:** 
Pense na memória como uma longa fita. Arrays 2D são como "quebrar" essa fita em linhas - primeiro armazenamos toda linha 0, depois linha 1, etc.

---

## **3. STRUCTS (REGISTROS)**

### **3.1 Definindo e Usando Structs**

```assembly
section .data
    ; Definindo uma struct "Pessoa"
    ; offset 0: nome (32 bytes)
    ; offset 32: idade (4 bytes)
    ; offset 36: altura (8 bytes - double)
    ; offset 44: ativo (1 byte)
    ; TOTAL: 45 bytes (mas com padding...)
    
    ; Criando uma instância da struct
    pessoa1:
        db "João Silva", 0    ; nome (string null-terminated)
        times 23 db 0         ; padding para completar 32 bytes
        dd 25                  ; idade
        dq 1.75                ; altura
        db 1                   ; ativo (true)
    
    ; Tamanho real com padding
    Pessoa_size equ 48         ; 45 + 3 bytes padding para alinhamento

section .text
global _start

_start:
    ; Acessando campos da struct
    mov rbx, pessoa1           ; RBX = endereço base
    
    ; Acessar nome (offset 0)
    mov rsi, rbx               ; RSI aponta para nome
    
    ; Acessar idade (offset 32)
    mov eax, [rbx + 32]        ; EAX = idade
    
    ; Acessar altura (offset 36)
    movsd xmm0, [rbx + 36]     ; xmm0 = altura (double)
    
    ; Acessar ativo (offset 44)
    mov dl, [rbx + 44]         ; DL = ativo
    
    ; Modificando campos
    mov dword [rbx + 32], 26   ; Atualiza idade para 26
```

**POR QUE O PADDING ACONTECE:**
- O processador x86-64 é mais eficiente quando dados estão alinhados em endereços múltiplos do seu tamanho
- Inteiros de 4 bytes performam melhor quando em endereços múltiplos de 4
- Doubles de 8 bytes performam melhor em endereços múltiplos de 8
- Por isso o compilador/assembler adiciona bytes de padding automaticamente

### **3.2 Array de Structs**

```assembly
section .data
    ; Array de 3 pessoas
    pessoas:
        ; Pessoa 0
        db "Ana", 0
        times 29 db 0           ; padding
        dd 22
        dq 1.65
        db 1
        times 3 db 0            ; padding para alinhar próxima struct
        
        ; Pessoa 1
        db "Carlos", 0
        times 25 db 0           ; padding
        dd 30
        dq 1.80
        db 1
        times 3 db 0
        
        ; Pessoa 2
        db "Maria", 0
        times 26 db 0           ; padding
        dd 28
        dq 1.70
        db 1
        times 3 db 0

section .text
global _start

_start:
    ; Acessar idade da pessoa 1 (Carlos, 30 anos)
    mov rbx, pessoas
    mov rax, Pessoa_size        ; Tamanho de cada struct
    mov rcx, 1                  ; Índice desejado
    mul rcx                     ; RAX = Pessoa_size * 1
    
    add rbx, rax                ; RBX aponta para pessoa 1
    mov eax, [rbx + 32]         ; EAX = idade (offset 32)
```

**ASSOCIAÇÃO:** 
Structs são como fichas de cadastro. Cada ficha tem campos fixos em posições específicas. Arrays de structs são como um fichário organizado.

---

## **4. LINKED LISTS (LISTAS ENCADEADAS)**

### **4.1 Lista Simplesmente Encadeada**

```assembly
section .data
    ; Definição do nó
    ; offset 0: valor (4 bytes)
    ; offset 8: ponteiro próximo (8 bytes) - alinhamento!
    
    ; Criando nós estáticos (para exemplo)
    no1:
        dd 10
        dq no2                  ; Ponteiro para próximo nó
    
    no2:
        dd 20
        dq no3
    
    no3:
        dd 30
        dq 0                    ; NULL (fim da lista)
    
    cabeca dq no1                ; Ponteiro para início da lista

section .text
global _start

_start:
    ; Percorrendo a lista
    mov rbx, [cabeca]           ; RBX = primeiro nó
    
percorrer_lista:
    cmp rbx, 0                  ; Chegou ao fim?
    je fim_percurso
    
    ; Acessar valor do nó atual
    mov eax, [rbx]              ; EAX = valor atual
    
    ; FAZ ALGO COM O VALOR...
    
    ; Avançar para próximo nó
    mov rbx, [rbx + 8]          ; RBX = próximo nó
    jmp percorrer_lista
    
fim_percurso:

    ; Inserir novo nó no início
    ; (assumindo que temos memória alocada em novo_no)
    ; mov rax, [cabeca]          ; RAX = antiga cabeça
    ; mov [novo_no + 8], rax     ; novo_no->prox = antiga cabeça
    ; mov [cabeca], novo_no      ; cabeça = novo_no
```

**POR QUE ISSO ACONTECE:**
- Listas encadeadas usam alocação não-contígua - cada nó pode estar em qualquer lugar da memória
- Precisamos de ponteiros porque os nós não estão sequenciais na memória
- O offset de 8 bytes para o ponteiro é devido ao alinhamento (ponteiros são 8 bytes em x86-64)

### **4.2 Alocação Dinâmica com malloc**

```assembly
section .data
    ; Constantes para syscalls
    SYS_BRK equ 12
    SYS_WRITE equ 1
    SYS_EXIT equ 60
    
    stdout equ 1
    newline db 10

section .bss
    ; Buffer para heap
    heap_start resq 1
    heap_current resq 1

section .text
global _start

; Função simples de alocação (malloc simplificado)
malloc:
    ; Entrada: RDI = tamanho desejado
    ; Saída: RAX = endereço alocado
    
    push rbx
    
    ; Salvar endereço atual do heap
    mov rax, [heap_current]
    mov rbx, rax
    
    ; Avançar ponteiro do heap
    add rbx, rdi
    mov [heap_current], rbx
    
    pop rbx
    ret

; Inicializar heap
init_heap:
    mov rax, SYS_BRK
    xor rdi, rdi
    syscall                     ; Obtém break atual
    
    mov [heap_start], rax
    mov [heap_current], rax
    ret

; Criar novo nó da lista
criar_no:
    ; Entrada: RDI = valor
    ; Saída: RAX = endereço do novo nó
    
    push rdi                    ; Salvar valor
    mov rdi, 16                 ; Tamanho do nó (4+4 padding+8)
    call malloc                 ; Alocar memória
    pop rdi
    
    ; Inicializar nó
    mov dword [rax], edi        ; valor
    mov qword [rax + 8], 0      ; próximo = NULL
    
    ret

; Inserir no final
inserir_fim:
    ; Entrada: RDI = valor, RSI = endereço da cabeça
    push rbp
    mov rbp, rsp
    
    push rbx
    push r12
    
    ; Criar novo nó
    call criar_no
    mov r12, rax                ; R12 = novo nó
    
    ; Se lista vazia
    mov rbx, [rsi]              ; RBX = cabeça
    cmp rbx, 0
    jne .procurar_fim
    
    ; Lista vazia: novo nó vira cabeça
    mov [rsi], r12
    jmp .fim
    
.procurar_fim:
    ; Encontrar último nó
    mov rcx, rbx                ; RCX = nó atual
    
.prox:
    cmp qword [rcx + 8], 0      ; É o último?
    je .encontrou_fim
    mov rcx, [rcx + 8]          ; Avança
    jmp .prox
    
.encontrou_fim:
    ; Inserir após último
    mov [rcx + 8], r12
    
.fim:
    pop r12
    pop rbx
    pop rbp
    ret

; Programa principal
_start:
    call init_heap
    
    ; Criar lista
    mov rbx, 0                  ; Cabeça inicialmente NULL
    push rbx                    ; Guardar na pilha (endereço da cabeça)
    mov rsp_local, rsp          ; Salvar RSP (precisa declarar na .bss)
    
    ; Inserir elementos
    mov rdi, 10
    mov rsi, rsp_local
    call inserir_fim
    
    mov rdi, 20
    mov rsi, rsp_local
    call inserir_fim
    
    mov rdi, 30
    mov rsi, rsp_local
    call inserir_fim
    
    ; Percorrer e imprimir
    mov rbx, [rsp_local]        ; RBX = cabeça
    
print_loop:
    cmp rbx, 0
    je fim_print
    
    ; Converter valor para string e imprimir
    ; (código de conversão omitido por brevidade)
    
    mov rbx, [rbx + 8]          ; Próximo nó
    jmp print_loop
    
fim_print:
    ; Exit
    mov rax, SYS_EXIT
    xor rdi, rdi
    syscall

section .bss
    rsp_local resq 1
```

**ASSOCIAÇÃO:** 
Listas encadeadas são como uma "caça ao tesouro" - cada pista (nó) te leva à próxima. Você não sabe onde está a próxima pista até ler a atual.

---

## **5. STACKS (PILHAS)**

### **5.1 Implementação de Pilha com Array**

```assembly
section .data
    ; Pilha usando array estático
    stack_size equ 100
    stack: times stack_size dq 0
    stack_top dq stack          ; Aponta para topo (próxima posição livre)
    
    ; Mensagens
    msg_push db "Push: ", 0
    msg_pop db "Pop: ", 0
    msg_empty db "Pilha vazia!", 10, 0

section .text
global _start

; Inicializar pilha
init_stack:
    mov qword [stack_top], stack
    ret

; Push: adicionar elemento
push_stack:
    ; Entrada: RDI = valor a empilhar
    push rbp
    mov rbp, rsp
    
    ; Verificar se há espaço
    mov rax, [stack_top]
    cmp rax, stack + (stack_size * 8)
    jge .stack_overflow
    
    ; Empilhar valor
    mov [rax], rdi
    add qword [stack_top], 8
    
    pop rbp
    ret
    
.stack_overflow:
    ; Tratar overflow (simplificado)
    pop rbp
    ret

; Pop: remover e retornar elemento
pop_stack:
    ; Saída: RAX = valor desempilhado
    push rbp
    mov rbp, rsp
    
    ; Verificar se pilha não está vazia
    mov rax, [stack_top]
    cmp rax, stack
    jle .stack_underflow
    
    ; Desempilhar
    sub qword [stack_top], 8
    mov rax, [stack_top]
    mov rax, [rax]              ; RAX = valor
    
    pop rbp
    ret
    
.stack_underflow:
    ; Pilha vazia
    xor rax, rax
    pop rbp
    ret

; Programa principal
_start:
    call init_stack
    
    ; Testar push
    mov rdi, 10
    call push_stack
    
    mov rdi, 20
    call push_stack
    
    mov rdi, 30
    call push_stack
    
    ; Testar pop (deve mostrar 30, 20, 10)
    call pop_stack              ; 30
    
    call pop_stack              ; 20
    
    call pop_stack              ; 10
    
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall
```

### **5.2 Usando a Pilha da CPU para Estruturas de Dados**

```assembly
section .data
    ; A própria pilha da CPU pode ser usada como estrutura de dados!
    ; RSP é o ponteiro de pilha da CPU
    
    ; Exemplo: avaliador de expressão pós-fixa (RPN)
    expressao: db "3 4 + 2 *", 0

section .text
global _start

; Avaliar expressão RPN usando pilha da CPU
avaliar_rpn:
    ; Exemplo: "3 4 + 2 *" = (3+4)*2 = 14
    
    ; Alocar espaço na pilha para variáveis locais
    push rbp
    mov rbp, rsp
    sub rsp, 16                 ; Espaço para variáveis locais
    
    ; Processar tokens...
    ; (implementação simplificada)
    
    ; Push valor na pilha da CPU
    push 3                      ; push 3
    push 4                      ; push 4
    
    ; Pop dois valores, somar, push resultado
    pop rax                      ; RAX = 4
    pop rbx                      ; RBX = 3
    add rax, rbx                 ; RAX = 7
    push rax                     ; push 7
    
    push 2                       ; push 2
    
    ; Multiplicar
    pop rbx                      ; RBX = 2
    pop rax                      ; RAX = 7
    mul rbx                      ; RAX = 14
    push rax                     ; push 14
    
    ; Resultado final no topo da pilha
    pop rax                      ; RAX = resultado
    
    ; Restaurar pilha
    mov rsp, rbp
    pop rbp
    ret

_start:
    call avaliar_rpn
    ; RAX = 14 (resultado)
    
    mov rax, 60
    xor rdi, rdi
    syscall
```

**POR QUE ISSO ACONTECE:**
- A pilha é LIFO (Last In, First Out) - o último a entrar é o primeiro a sair
- Isso é perfeito para chamadas de função (salvar endereço de retorno), expressões, undo/redo
- A pilha da CPU cresce para baixo (endereços decrescentes) por razões históricas

---

## **6. TREES (ÁRVORES)**

### **6.1 Árvore Binária de Busca**

```assembly
section .data
    ; Definição do nó da árvore
    ; offset 0: valor (4 bytes)
    ; offset 8: esquerda (8 bytes) - ponteiro
    ; offset 16: direita (8 bytes) - ponteiro
    
    ; Exemplo de árvore manual
    ;        50
    ;       /  \
    ;      30   70
    ;     /  \
    ;    20  40
    
    no20:
        dd 20
        dq 0                    ; esquerda NULL
        dq 0                    ; direita NULL
    
    no40:
        dd 40
        dq 0
        dq 0
    
    no30:
        dd 30
        dq no20
        dq no40
    
    no70:
        dd 70
        dq 0
        dq 0
    
    raiz:
        dd 50
        dq no30
        dq no70

section .text
global _start

; Buscar valor na árvore
buscar_arvore:
    ; Entrada: RDI = valor a buscar, RSI = nó atual (ou raiz)
    ; Saída: RAX = endereço do nó (0 se não encontrado)
    
    cmp rsi, 0                  ; Nó NULL?
    je .nao_encontrado
    
    ; Comparar valor
    mov eax, [rsi]              ; EAX = valor do nó atual
    cmp edi, eax
    je .encontrado              ; Igual: achou!
    jl .buscar_esquerda         ; Menor: vai para esquerda
    jg .buscar_direita          ; Maior: vai para direita
    
.buscar_esquerda:
    mov rsi, [rsi + 8]          ; RSI = nó->esquerda
    jmp buscar_arvore
    
.buscar_direita:
    mov rsi, [rsi + 16]         ; RSI = nó->direita
    jmp buscar_arvore
    
.encontrado:
    mov rax, rsi                ; Retorna endereço do nó
    ret
    
.nao_encontrado:
    xor rax, rax
    ret

; Percorrer em ordem (in-order traversal)
percorrer_inorder:
    ; Entrada: RDI = nó atual
    push rbp
    mov rbp, rsp
    push rbx
    
    cmp rdi, 0
    je .fim
    
    push rdi                    ; Salvar nó atual
    
    ; Percorrer subárvore esquerda
    mov rdi, [rdi + 8]          ; RDI = nó->esquerda
    call percorrer_inorder
    
    pop rdi                     ; Restaurar nó atual
    
    ; Processar nó atual
    mov ebx, [rdi]              ; EBX = valor
    
    ; FAZ ALGO COM O VALOR...
    
    push rdi
    
    ; Percorrer subárvore direita
    mov rdi, [rdi + 16]         ; RDI = nó->direita
    call percorrer_inorder
    
    pop rdi
    
.fim:
    pop rbx
    pop rbp
    ret

; Inserir na árvore binária
inserir_arvore:
    ; Entrada: RDI = valor, RSI = endereço do ponteiro raiz
    push rbp
    mov rbp, rsp
    push rbx
    push r12
    push r13
    
    mov r12, rdi                ; R12 = valor
    mov r13, rsi                ; R13 = endereço do ponteiro raiz
    
    ; Se raiz NULL, criar novo nó
    mov rbx, [r13]              ; RBX = raiz
    cmp rbx, 0
    jne .buscar_posicao
    
    ; Criar novo nó
    mov rdi, 24                 ; Tamanho do nó (4+4padding+8+8)
    call malloc
    mov rbx, rax                ; RBX = novo nó
    
    ; Inicializar nó
    mov dword [rbx], r12d       ; valor
    mov qword [rbx + 8], 0      ; esquerda NULL
    mov qword [rbx + 16], 0     ; direita NULL
    
    ; Atualizar raiz
    mov [r13], rbx
    jmp .fim
    
.buscar_posicao:
    ; RBX = nó atual
    mov eax, [rbx]              ; EAX = valor atual
    cmp r12d, eax
    
    jl .inserir_esquerda        ; Novo valor menor: vai para esquerda
    jg .inserir_direita         ; Novo valor maior: vai para direita
    
    ; Valor igual - não inserir (BST normalmente não permite duplicatas)
    jmp .fim
    
.inserir_esquerda:
    ; Verificar se esquerda é NULL
    cmp qword [rbx + 8], 0
    jne .prox_esquerda
    
    ; Inserir aqui
    mov rdi, 24
    call malloc
    mov rcx, rax
    
    ; Inicializar nó
    mov dword [rcx], r12d
    mov qword [rcx + 8], 0
    mov qword [rcx + 16], 0
    
    ; Ligar ao pai
    mov [rbx + 8], rcx
    jmp .fim
    
.prox_esquerda:
    ; Continuar busca na esquerda
    mov rbx, [rbx + 8]
    jmp .buscar_posicao
    
.inserir_direita:
    ; Similar à esquerda, mas para direita
    cmp qword [rbx + 16], 0
    jne .prox_direita
    
    ; Inserir aqui
    mov rdi, 24
    call malloc
    mov rcx, rax
    
    mov dword [rcx], r12d
    mov qword [rcx + 8], 0
    mov qword [rcx + 16], 0
    
    mov [rbx + 16], rcx
    jmp .fim
    
.prox_direita:
    mov rbx, [rbx + 16]
    jmp .buscar_posicao
    
.fim:
    pop r13
    pop r12
    pop rbx
    pop rbp
    ret

_start:
    ; Usar a árvore predefinida
    mov rsi, raiz
    mov rdi, 40                 ; Buscar valor 40
    call buscar_arvore
    
    ; Se RAX != 0, encontrou
    
    ; Percorrer em ordem (imprime em ordem crescente)
    mov rdi, raiz
    call percorrer_inorder
    
    ; Inserir novo valor 25
    mov rdi, 25
    mov rsi, raiz_ptr           ; Precisa ser ponteiro para raiz
    call inserir_arvore
    
    ; Exit
    mov rax, 60
    xor rdi, rdi
    syscall

section .data
    raiz_ptr dq raiz
```

**POR QUE ISSO ACONTECE:**
- Árvores binárias organizam dados hierarquicamente
- Cada nó tem no máximo dois filhos (esquerda/direita)
- A propriedade de BST (esquerda < pai < direita) permite busca binária eficiente
- A recursão é natural para árvores - cada subárvore é uma árvore menor

---

## **7. EXERCÍCIOS PRÁTICOS**

### **Exercício 1: Implementar Fila (Queue) com Array Circular**

```assembly
; Implemente uma fila circular com:
; - capacidade para 10 elementos
; - ponteiros head e tail
; - funções enqueue e dequeue
; - tratamento de fila cheia/vazia

section .data
    QUEUE_SIZE equ 10
    queue: times QUEUE_SIZE dq 0
    head dq 0          ; Índice para remoção
    tail dq 0          ; Índice para inserção
    count dq 0         ; Contador de elementos

section .text
; Sua implementação aqui...
```

### **Exercício 2: Ordenar Array com Bubble Sort**

```assembly
; Implemente bubble sort para um array de inteiros
; Use as técnicas de acesso a array aprendidas

section .data
    arr: dd 64, 34, 25, 12, 22, 11, 90
    arr_len equ ($ - arr) / 4

section .text
; Sua implementação aqui...
```

### **Exercício 3: Calculadora RPN Completa**

```assembly
; Implemente uma calculadora que:
; - lê expressão em notação pós-fixa
; - usa pilha para avaliação
; - suporta +, -, *, /
; - trata números de múltiplos dígitos
```

### **Exercício 4: Gerenciador de Memória Simples**

```assembly
; Estenda a função malloc para:
; - manter lista de blocos livres
; - implementar first-fit allocation
; - adicionar free() que retorna blocos à lista livre
```

---

## **DICAS FINAIS E BOAS PRÁTICAS**

### **1. Sempre documente seus registradores**
```assembly
; Bom: comentar o uso dos registradores
; RDI - valor a buscar
; RSI - nó atual
; RAX - resultado
```

### **2. Use equ para constantes**
```assembly
; Facilita manutenção
TAM_INT equ 4
TAM_PTR equ 8
```

### **3. Cuidado com alinhamento**
```assembly
; O processador agradece
; structs devem começar em endereços múltiplos de 8
```

### **4. Sempre inicialize variáveis**
```assembly
; Em .bss, valores são zero, mas é boa prática inicializar
section .bss
    ponteiro resq 1

section .text
    mov qword [ponteiro], 0
```

### **5. Use a pilha para variáveis locais**
```assembly
minha_funcao:
    push rbp
    mov rbp, rsp
    sub rsp, 32        ; Espaço para 4 variáveis locais de 8 bytes
    ; ... código ...
    mov rsp, rbp
    pop rbp
    ret
```

---

