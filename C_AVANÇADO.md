# 🔬 MANUAL AVANÇADO DE LINGUAGEM C
*Ponteiros Avançados, Memória Dinâmica, Pré-processador e Técnicas Avançadas*

---

## 📋 SUMÁRIO
1. [Ponteiros Avançados](#1-ponteiros-avançados)
2. [Manipulação de Memória Dinâmica](#2-manipulação-de-memória-dinâmica)
3. [Tipos Definidos pelo Usuário](#3-tipos-definidos-pelo-usuário)
4. [Diretivas de Pré-processador Avançadas](#4-diretivas-de-pré-processador-avançadas)
5. [Manipulação de Arquivos Avançada](#5-manipulação-de-arquivos-avançada)
6. [Programação de Baixo Nível](#6-programação-de-baixo-nível)
7. [Técnicas de Otimização](#7-técnicas-de-otimização)

---

## 1. PONTEIROS AVANÇADOS

### Ponteiros para Funções
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Declaração de tipo para ponteiro de função
typedef int (*Comparador)(const void*, const void*);

// Funções de comparação para diferentes tipos
int comparar_int(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}

int comparar_float(const void *a, const void *b) {
    float diff = *(float*)a - *(float*)b;
    return (diff > 0) ? 1 : ((diff < 0) ? -1 : 0);
}

int comparar_string(const void *a, const void *b) {
    return strcmp(*(const char**)a, *(const char**)b);
}

// Função que recebe ponteiro para função como parâmetro
void ordenar(void *array, size_t n, size_t tamanho, Comparador cmp) {
    for(size_t i = 0; i < n-1; i++) {
        for(size_t j = 0; j < n-i-1; j++) {
            void *elem1 = (char*)array + j * tamanho;
            void *elem2 = (char*)array + (j+1) * tamanho;
            
            if(cmp(elem1, elem2) > 0) {
                // Troca os elementos
                char temp[tamanho];
                memcpy(temp, elem1, tamanho);
                memcpy(elem1, elem2, tamanho);
                memcpy(elem2, temp, tamanho);
            }
        }
    }
}

// Ponteiro para função como retorno
typedef double (*Operacao)(double, double);

Operacao selecionar_operacao(char operador) {
    switch(operador) {
        case '+': return &soma;
        case '-': return &subtracao;
        case '*': return &multiplicacao;
        case '/': return &divisao;
        default: return NULL;
    }
}

double soma(double a, double b) { return a + b; }
double subtracao(double a, double b) { return a - b; }
double multiplicacao(double a, double b) { return a * b; }
double divisao(double a, double b) { return b != 0 ? a / b : 0; }

// Array de ponteiros para funções
typedef void (*MenuFunc)(void);

void opcao1() { printf("Executando opção 1\n"); }
void opcao2() { printf("Executando opção 2\n"); }
void opcao3() { printf("Executando opção 3\n"); }

MenuFunc menu[] = {opcao1, opcao2, opcao3};

void exemplo_ponteiros_funcao() {
    // Exemplo 1: Ordenação genérica
    int numeros[] = {5, 2, 8, 1, 9};
    ordenar(numeros, 5, sizeof(int), comparar_int);
    printf("Números ordenados: ");
    for(int i = 0; i < 5; i++) printf("%d ", numeros[i]);
    printf("\n");
    
    // Exemplo 2: Calculadora com ponteiros para funções
    double x = 10, y = 3;
    Operacao op = selecionar_operacao('*');
    if(op) printf("10 * 3 = %.2f\n", op(x, y));
    
    // Exemplo 3: Menu dinâmico
    int escolha = 2; // Suponha entrada do usuário
    if(escolha >= 1 && escolha <= 3) {
        menu[escolha - 1]();
    }
}
```

### Ponteiros para Arrays Multidimensionais
```c
#include <stdio.h>

void ponteiros_matrizes() {
    int matriz[3][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };
    
    // Ponteiro para array de 4 inteiros
    int (*ptr)[4] = matriz;
    
    printf("Acessando matriz com ponteiro:\n");
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 4; j++) {
            printf("%3d ", ptr[i][j]); // ptr[i][j] é equivalente a matriz[i][j]
        }
        printf("\n");
    }
    
    // Aritmética de ponteiros em matrizes
    printf("\nUsando aritmética de ponteiros:\n");
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 4; j++) {
            printf("%3d ", *(*(ptr + i) + j));
        }
        printf("\n");
    }
    
    // Matriz dinâmica com ponteiro duplo
    int linhas = 3, colunas = 4;
    int **matriz_din = malloc(linhas * sizeof(int*));
    for(int i = 0; i < linhas; i++) {
        matriz_din[i] = malloc(colunas * sizeof(int));
        for(int j = 0; j < colunas; j++) {
            matriz_din[i][j] = (i * colunas) + j + 1;
        }
    }
    
    printf("\nMatriz dinâmica:\n");
    for(int i = 0; i < linhas; i++) {
        for(int j = 0; j < colunas; j++) {
            printf("%3d ", matriz_din[i][j]);
        }
        printf("\n");
    }
    
    // Liberar memória
    for(int i = 0; i < linhas; i++) {
        free(matriz_din[i]);
    }
    free(matriz_din);
}

// Ponteiros para arrays de ponteiros (jagged arrays)
void jagged_array() {
    int rows = 4;
    int *jagged[rows];
    int sizes[] = {2, 4, 1, 3};
    
    // Criar array jagged
    for(int i = 0; i < rows; i++) {
        jagged[i] = malloc(sizes[i] * sizeof(int));
        for(int j = 0; j < sizes[i]; j++) {
            jagged[i][j] = (i + 1) * 10 + j;
        }
    }
    
    printf("Jagged Array:\n");
    for(int i = 0; i < rows; i++) {
        printf("Linha %d: ", i);
        for(int j = 0; j < sizes[i]; j++) {
            printf("%d ", jagged[i][j]);
        }
        printf("\n");
    }
    
    // Liberar memória
    for(int i = 0; i < rows; i++) {
        free(jagged[i]);
    }
}
```

### Ponteiros Genéricos (void*) e Casting
```c
#include <stdio.h>
#include <stdlib.h>

void manipular_void_pointer() {
    // void* pode apontar para qualquer tipo
    int inteiro = 42;
    float flutuante = 3.14;
    char caractere = 'A';
    char texto[] = "Hello";
    
    void *ptr_gen;
    
    // Apontando para diferentes tipos
    ptr_gen = &inteiro;
    printf("Inteiro via void*: %d\n", *(int*)ptr_gen);
    
    ptr_gen = &flutuante;
    printf("Float via void*: %.2f\n", *(float*)ptr_gen);
    
    ptr_gen = &caractere;
    printf("Char via void*: %c\n", *(char*)ptr_gen);
    
    ptr_gen = texto;
    printf("String via void*: %s\n", (char*)ptr_gen);
}

// Função genérica usando void*
void copiar_memoria(void *dest, const void *src, size_t n) {
    // Cast para char* para permitir aritmética de ponteiros byte a byte
    char *d = dest;
    const char *s = src;
    
    for(size_t i = 0; i < n; i++) {
        d[i] = s[i];
    }
}

// Exemplo: swap genérico
void swap_generico(void *a, void *b, size_t tamanho) {
    char *pa = a;
    char *pb = b;
    char temp;
    
    for(size_t i = 0; i < tamanho; i++) {
        temp = pa[i];
        pa[i] = pb[i];
        pb[i] = temp;
    }
}

void exemplo_swap_generico() {
    // Swap de inteiros
    int x = 10, y = 20;
    printf("Antes: x = %d, y = %d\n", x, y);
    swap_generico(&x, &y, sizeof(int));
    printf("Depois: x = %d, y = %d\n", x, y);
    
    // Swap de floats
    float a = 1.5, b = 2.5;
    printf("\nAntes: a = %.2f, b = %.2f\n", a, b);
    swap_generico(&a, &b, sizeof(float));
    printf("Depois: a = %.2f, b = %.2f\n", a, b);
    
    // Swap de estruturas
    struct Ponto {
        int x, y;
    } p1 = {1, 2}, p2 = {3, 4};
    
    printf("\nAntes: p1 = (%d,%d), p2 = (%d,%d)\n", p1.x, p1.y, p2.x, p2.y);
    swap_generico(&p1, &p2, sizeof(struct Ponto));
    printf("Depois: p1 = (%d,%d), p2 = (%d,%d)\n", p1.x, p1.y, p2.x, p2.y);
}

// Ponteiro para ponteiro (não confundir com array de ponteiros)
void ponteiro_para_ponteiro() {
    int valor = 100;
    int *ptr = &valor;
    int **ptr_para_ptr = &ptr;
    int ***ptr_para_ptr_para_ptr = &ptr_para_ptr;
    
    printf("valor = %d\n", valor);
    printf("*ptr = %d\n", *ptr);
    printf("**ptr_para_ptr = %d\n", **ptr_para_ptr);
    printf("***ptr_para_ptr_para_ptr = %d\n", ***ptr_para_ptr_para_ptr);
    
    // Modificando através de múltiplos níveis
    ***ptr_para_ptr_para_ptr = 200;
    printf("\nNovo valor = %d\n", valor);
}

int main_ponteiros() {
    printf("=== PONTEIROS PARA FUNÇÕES ===\n");
    exemplo_ponteiros_funcao();
    
    printf("\n=== PONTEIROS E MATRIZES ===\n");
    ponteiros_matrizes();
    jagged_array();
    
    printf("\n=== PONTEIROS GENÉRICOS ===\n");
    manipular_void_pointer();
    exemplo_swap_generico();
    
    printf("\n=== PONTEIROS PARA PONTEIROS ===\n");
    ponteiro_para_ponteiro();
    
    return 0;
}
```

---

## 2. MANIPULAÇÃO DE MEMÓRIA DINÂMICA

### Alocação Básica: malloc, calloc, realloc, free
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void exemplos_alocacao_basica() {
    // malloc - aloca memória sem inicializar
    int *arr_malloc = (int*)malloc(5 * sizeof(int));
    if(arr_malloc == NULL) {
        printf("Erro na alocação com malloc!\n");
        return;
    }
    
    // Inicializar manualmente
    for(int i = 0; i < 5; i++) {
        arr_malloc[i] = (i + 1) * 10;
    }
    
    printf("Array com malloc: ");
    for(int i = 0; i < 5; i++) {
        printf("%d ", arr_malloc[i]);
    }
    printf("\n");
    
    // calloc - aloca e inicializa com zero
    int *arr_calloc = (int*)calloc(5, sizeof(int));
    if(arr_calloc == NULL) {
        printf("Erro na alocação com calloc!\n");
        free(arr_malloc);
        return;
    }
    
    printf("Array com calloc (inicializado com 0): ");
    for(int i = 0; i < 5; i++) {
        printf("%d ", arr_calloc[i]);
    }
    printf("\n");
    
    // realloc - redimensiona memória alocada
    int *arr_realloc = (int*)realloc(arr_calloc, 10 * sizeof(int));
    if(arr_realloc == NULL) {
        printf("Erro no realloc!\n");
        free(arr_calloc);
    } else {
        arr_calloc = arr_realloc; // Usar novo ponteiro
        
        // Inicializar nova parte
        for(int i = 5; i < 10; i++) {
            arr_calloc[i] = (i + 1) * 10;
        }
        
        printf("Array após realloc: ");
        for(int i = 0; i < 10; i++) {
            printf("%d ", arr_calloc[i]);
        }
        printf("\n");
    }
    
    // Liberar memória
    free(arr_malloc);
    free(arr_calloc);
    
    // IMPORTANTE: Após free, definir ponteiro como NULL
    arr_malloc = NULL;
    arr_calloc = NULL;
}

### Alocação Dinâmica de Estruturas Complexas
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char nome[50];
    int idade;
    float salario;
} Funcionario;

typedef struct {
    Funcionario *funcionarios;
    int capacidade;
    int tamanho;
} Empresa;

// Criar nova empresa
Empresa* criar_empresa(int capacidade) {
    Empresa *emp = (Empresa*)malloc(sizeof(Empresa));
    if(!emp) return NULL;
    
    emp->funcionarios = (Funcionario*)malloc(capacidade * sizeof(Funcionario));
    if(!emp->funcionarios) {
        free(emp);
        return NULL;
    }
    
    emp->capacidade = capacidade;
    emp->tamanho = 0;
    return emp;
}

// Adicionar funcionário
int adicionar_funcionario(Empresa *emp, const char *nome, int idade, float salario) {
    if(emp->tamanho >= emp->capacidade) {
        // Redimensionar
        int nova_capacidade = emp->capacidade * 2;
        Funcionario *novo = (Funcionario*)realloc(emp->funcionarios, 
                                                 nova_capacidade * sizeof(Funcionario));
        if(!novo) return 0; // Falha
        
        emp->funcionarios = novo;
        emp->capacidade = nova_capacidade;
    }
    
    strcpy(emp->funcionarios[emp->tamanho].nome, nome);
    emp->funcionarios[emp->tamanho].idade = idade;
    emp->funcionarios[emp->tamanho].salario = salario;
    emp->tamanho++;
    
    return 1;
}

// Liberar empresa
void destruir_empresa(Empresa *emp) {
    if(emp) {
        free(emp->funcionarios);
        free(emp);
    }
}

// Exemplo de uso
void exemplo_empresa() {
    Empresa *minha_empresa = criar_empresa(2);
    
    adicionar_funcionario(minha_empresa, "João Silva", 30, 3000.00);
    adicionar_funcionario(minha_empresa, "Maria Santos", 25, 3500.00);
    adicionar_funcionario(minha_empresa, "Carlos Oliveira", 35, 4000.00); // Redimensiona
    
    printf("Funcionários da empresa:\n");
    for(int i = 0; i < minha_empresa->tamanho; i++) {
        printf("%s - %d anos - R$ %.2f\n",
               minha_empresa->funcionarios[i].nome,
               minha_empresa->funcionarios[i].idade,
               minha_empresa->funcionarios[i].salario);
    }
    
    destruir_empresa(minha_empresa);
}
```

### Gerenciamento Avançado de Memória
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Arena allocator - alocação por arena
typedef struct {
    char *buffer;
    size_t tamanho;
    size_t usado;
} Arena;

Arena* criar_arena(size_t tamanho) {
    Arena *arena = (Arena*)malloc(sizeof(Arena));
    if(!arena) return NULL;
    
    arena->buffer = (char*)malloc(tamanho);
    if(!arena->buffer) {
        free(arena);
        return NULL;
    }
    
    arena->tamanho = tamanho;
    arena->usado = 0;
    return arena;
}

void* arena_alloc(Arena *arena, size_t tamanho) {
    // Alinhar para 8 bytes
    size_t alinhado = (tamanho + 7) & ~7;
    
    if(arena->usado + alinhado > arena->tamanho) {
        return NULL; // Sem espaço
    }
    
    void *ptr = arena->buffer + arena->usado;
    arena->usado += alinhado;
    return ptr;
}

void resetar_arena(Arena *arena) {
    arena->usado = 0;
}

void destruir_arena(Arena *arena) {
    if(arena) {
        free(arena->buffer);
        free(arena);
    }
}

// Pool allocator - para objetos do mesmo tamanho
typedef struct PoolNode {
    struct PoolNode *proximo;
} PoolNode;

typedef struct {
    size_t tamanho_objeto;
    size_t objetos_por_bloco;
    PoolNode *livre;
    char **blocos;
    size_t num_blocos;
} PoolAllocator;

PoolAllocator* criar_pool(size_t tamanho_objeto, size_t objetos_por_bloco) {
    PoolAllocator *pool = (PoolAllocator*)malloc(sizeof(PoolAllocator));
    if(!pool) return NULL;
    
    pool->tamanho_objeto = (tamanho_objeto < sizeof(PoolNode)) ? 
                           sizeof(PoolNode) : tamanho_objeto;
    pool->objetos_por_bloco = objetos_por_bloco;
    pool->livre = NULL;
    pool->blocos = NULL;
    pool->num_blocos = 0;
    
    return pool;
}

void* pool_alloc(PoolAllocator *pool) {
    if(!pool->livre) {
        // Alocar novo bloco
        size_t tamanho_bloco = pool->tamanho_objeto * pool->objetos_por_bloco;
        char *novo_bloco = (char*)malloc(tamanho_bloco);
        if(!novo_bloco) return NULL;
        
        // Adicionar à lista de blocos
        pool->blocos = (char**)realloc(pool->blocos, 
                                      (pool->num_blocos + 1) * sizeof(char*));
        pool->blocos[pool->num_blocos] = novo_bloco;
        pool->num_blocos++;
        
        // Criar lista livre
        for(size_t i = 0; i < pool->objetos_por_bloco; i++) {
            PoolNode *node = (PoolNode*)(novo_bloco + i * pool->tamanho_objeto);
            node->proximo = pool->livre;
            pool->livre = node;
        }
    }
    
    // Retornar nó da lista livre
    void *objeto = pool->livre;
    pool->livre = pool->livre->proximo;
    return objeto;
}

void pool_free(PoolAllocator *pool, void *objeto) {
    PoolNode *node = (PoolNode*)objeto;
    node->proximo = pool->livre;
    pool->livre = node;
}

void exemplo_alocadores() {
    printf("=== ARENA ALLOCATOR ===\n");
    Arena *arena = criar_arena(1024);
    
    int *nums = (int*)arena_alloc(arena, 10 * sizeof(int));
    char *str = (char*)arena_alloc(arena, 50);
    
    for(int i = 0; i < 10; i++) nums[i] = i;
    strcpy(str, "Texto alocado na arena");
    
    printf("Números: ");
    for(int i = 0; i < 10; i++) printf("%d ", nums[i]);
    printf("\nString: %s\n", str);
    
    resetar_arena(arena); // Tudo é "liberado" de uma vez
    destruir_arena(arena);
    
    printf("\n=== POOL ALLOCATOR ===\n");
    PoolAllocator *pool = criar_pool(sizeof(int), 10);
    
    int *p1 = (int*)pool_alloc(pool);
    int *p2 = (int*)pool_alloc(pool);
    
    *p1 = 42;
    *p2 = 100;
    
    printf("p1 = %d, p2 = %d\n", *p1, *p2);
    
    pool_free(pool, p1);
    pool_free(pool, p2);
    
    // Nota: pool não é destruído no exemplo para simplificar
}
```

### Detectando Vazamentos de Memória
```c
#include <stdio.h>
#include <stdlib.h>

#ifdef DEBUG_MEMORY
static size_t total_alocado = 0;

void* debug_malloc(size_t tamanho, const char* arquivo, int linha) {
    void *ptr = malloc(tamanho);
    if(ptr) {
        total_alocado += tamanho;
        printf("[DEBUG] malloc(%zu) em %s:%d -> %p (total: %zu)\n",
               tamanho, arquivo, linha, ptr, total_alocado);
    }
    return ptr;
}

void debug_free(void *ptr, const char* arquivo, int linha) {
    printf("[DEBUG] free(%p) em %s:%d\n", ptr, arquivo, linha);
    free(ptr);
}

#define malloc(s) debug_malloc(s, __FILE__, __LINE__)
#define free(p) debug_free(p, __FILE__, __LINE__)

#endif

void exemplo_vazamento() {
    int *array = (int*)malloc(100 * sizeof(int));
    
    // Esquecer de liberar memória causa vazamento
    // free(array); // COMENTADO PARA DEMONSTRAR
    
    // Vazamento de memória quando reassign sem free
    array = (int*)malloc(50 * sizeof(int)); // Primeiro array vazado
    free(array);
}

// Wrapper para detectar vazamentos
void verificar_vazamentos() {
#ifdef DEBUG_MEMORY
    printf("\nTotal de memória alocada não liberada: %zu bytes\n", total_alocado);
#endif
}

int main_memoria() {
    printf("=== ALOCAÇÃO BÁSICA ===\n");
    exemplos_alocacao_basica();
    
    printf("\n=== ESTRUTURAS COMPLEXAS ===\n");
    exemplo_empresa();
    
    printf("\n=== ALOCADORES AVANÇADOS ===\n");
    exemplo_alocadores();
    
    printf("\n=== DETECTANDO VAZAMENTOS ===\n");
    exemplo_vazamento();
    verificar_vazamentos();
    
    return 0;
}
```

---

## 3. TIPOS DEFINIDOS PELO USUÁRIO

### Typedef Avançado
```c
#include <stdio.h>
#include <stdlib.h>

// Typedef com estruturas complexas
typedef struct Node {
    int valor;
    struct Node *proximo;
} Node;

typedef Node* Lista;  // Lista é um ponteiro para Node

// Typedef para funções
typedef int (*Compara)(int, int);
typedef void (*Callback)(void*);

// Typedef para tipos genéricos
typedef void* DadoGenerico;

// Typedef para criar "novos" tipos
typedef int Indice;
typedef float Percentual;
typedef char Nome[50];
typedef unsigned long ID;

// Exemplo de uso
void exemplos_typedef() {
    // Mais legível com typedef
    Percentual desconto = 15.5;
    Indice posicao = 10;
    ID usuario_id = 1234567890;
    Nome pessoa = "João Silva";
    
    printf("Desconto: %.1f%%\n", desconto);
    printf("Posição: %d\n", posicao);
    printf("ID: %lu\n", usuario_id);
    printf("Nome: %s\n", pessoa);
}

// Typedef com arrays
typedef int Vetor3[3];
typedef int Matriz3x3[3][3];

void operacoes_vetor() {
    Vetor3 v1 = {1, 2, 3};
    Vetor3 v2 = {4, 5, 6};
    Vetor3 resultado;
    
    // Soma de vetores
    for(int i = 0; i < 3; i++) {
        resultado[i] = v1[i] + v2[i];
    }
    
    printf("Soma de vetores: (%d, %d, %d)\n", 
           resultado[0], resultado[1], resultado[2]);
}

// Typedef aninhado
typedef struct {
    int x, y;
} Ponto;

typedef struct {
    Ponto centro;
    float raio;
} Circulo;

typedef struct {
    Ponto vertices[3];
} Triangulo;

// Typedef para tipos opacos (abstração)
typedef struct ContaBancaria ContaBancaria;

struct ContaBancaria {
    int numero;
    float saldo;
    char titular[50];
};

ContaBancaria* criar_conta(int numero, const char* titular) {
    ContaBancaria* conta = (ContaBancaria*)malloc(sizeof(ContaBancaria));
    if(conta) {
        conta->numero = numero;
        conta->saldo = 0.0;
        strcpy(conta->titular, titular);
    }
    return conta;
}

void depositar(ContaBancaria* conta, float valor) {
    if(conta && valor > 0) {
        conta->saldo += valor;
    }
}

// O usuário só vê o typedef, não a implementação interna
```

### Tipos Opaque (PIMPL Pattern)
```c
// arquivo: pilha_privado.h (não exposto ao usuário)
typedef struct PilhaImpl {
    int *dados;
    int capacidade;
    int topo;
} PilhaImpl;

// arquivo: pilha.h (interface pública)
typedef struct PilhaImpl* Pilha;

Pilha pilha_criar(int capacidade);
void pilha_destruir(Pilha p);
void pilha_empilhar(Pilha p, int valor);
int pilha_desempilhar(Pilha p);
int pilha_vazia(Pilha p);

// arquivo: pilha.c (implementação)
#include "pilha_privado.h"

Pilha pilha_criar(int capacidade) {
    PilhaImpl *impl = (PilhaImpl*)malloc(sizeof(PilhaImpl));
    if(!impl) return NULL;
    
    impl->dados = (int*)malloc(capacidade * sizeof(int));
    if(!impl->dados) {
        free(impl);
        return NULL;
    }
    
    impl->capacidade = capacidade;
    impl->topo = -1;
    return impl;
}

void pilha_destruir(Pilha p) {
    if(p) {
        free(p->dados);
        free(p);
    }
}

// Restante da implementação...
```

### Tags e Campos de Bits
```c
#include <stdio.h>
#include <stdint.h>

// Campos de bits para otimização de memória
struct DataCompacta {
    // Total: 32 bits (4 bytes)
    unsigned int dia : 5;      // 5 bits para dia (0-31)
    unsigned int mes : 4;      // 4 bits para mês (0-15)
    unsigned int ano : 12;     // 12 bits para ano (0-4095)
    unsigned int semana : 3;   // 3 bits para dia da semana (0-7)
    unsigned int feriado : 1;  // 1 bit para indicar feriado
    unsigned int reservado : 7; // 7 bits reservados
};

// Uso prático: flags compactas
struct Permissoes {
    unsigned int ler : 1;
    unsigned int escrever : 1;
    unsigned int executar : 1;
    unsigned int administrador : 1;
    unsigned int reservado : 28;
};

// União com campos de bits para acesso alternativo
union ControleHardware {
    struct {
        unsigned int motor1 : 1;
        unsigned int motor2 : 1;
        unsigned int sensor1 : 1;
        unsigned int sensor2 : 1;
        unsigned int emergencia : 1;
        unsigned int : 27; // padding
    } bits;
    
    uint32_t valor; // Acesso como número inteiro
};

void exemplo_campos_bits() {
    struct DataCompacta data = {15, 3, 2024, 2, 0, 0};
    
    printf("Data: %02d/%02d/%04d\n", data.dia, data.mes, data.ano);
    printf("Tamanho da estrutura: %zu bytes\n", sizeof(data));
    
    // Permissões
    struct Permissoes user = {1, 1, 0, 0, 0};
    printf("\nPermissões do usuário:\n");
    printf("Ler: %s\n", user.ler ? "Sim" : "Não");
    printf("Escrever: %s\n", user.escrever ? "Sim" : "Não");
    printf("Executar: %s\n", user.executar ? "Sim" : "Não");
    
    // Hardware
    union ControleHardware ctrl;
    ctrl.valor = 0;
    
    ctrl.bits.motor1 = 1;
    ctrl.bits.sensor1 = 1;
    
    printf("\nValor do controle: 0x%08X\n", ctrl.valor);
    printf("Motor1 ativo: %s\n", ctrl.bits.motor1 ? "Sim" : "Não");
}
```

---

## 4. DIRETIVAS DE PRÉ-PROCESSADOR AVANÇADAS

### Macros Complexas
```c
#include <stdio.h>

// Macro multi-linha
#define SWAP(a, b) do { \
    typeof(a) _temp = (a); \
    (a) = (b); \
    (b) = _temp; \
} while(0)

// Macro com número variável de argumentos (C99)
#define DEBUG_PRINT(fmt, ...) \
    printf("[DEBUG] %s:%d: " fmt, __FILE__, __LINE__, ##__VA_ARGS__)

// Macro para cálculo em tempo de compilação
#define ARRAY_SIZE(arr) (sizeof(arr) / sizeof((arr)[0]))

// Macro para alinhamento
#define ALIGN_UP(x, align) (((x) + (align) - 1) & ~((align) - 1))
#define ALIGN_DOWN(x, align) ((x) & ~((align) - 1))

// X-Macros para evitar repetição de código
#define CORES \
    X(VERMELHO) \
    X(VERDE)   \
    X(AZUL)    \
    X(AMARELO)

// Gerar enumeração
#define X(color) color,
enum Cores { CORES };
#undef X

// Gerar strings
#define X(color) #color,
const char* nomes_cores[] = { CORES };
#undef X

// Macros para logging condicional
#ifdef DEBUG_LEVEL
    #define LOG(level, fmt, ...) \
        if(level <= DEBUG_LEVEL) \
            printf("[LOG%d] " fmt, level, ##__VA_ARGS__)
#else
    #define LOG(level, fmt, ...) ((void)0)
#endif

// Macro para assert personalizado
#define ASSERT(cond, msg) \
    do { \
        if(!(cond)) { \
            fprintf(stderr, "ASSERT falhou em %s:%d: %s\n", \
                    __FILE__, __LINE__, msg); \
            abort(); \
        } \
    } while(0)

void exemplos_macros() {
    printf("=== MACROS COMPLEXAS ===\n");
    
    int x = 5, y = 10;
    printf("Antes: x=%d, y=%d\n", x, y);
    SWAP(x, y);
    printf("Depois: x=%d, y=%d\n", x, y);
    
    DEBUG_PRINT("Valor de x: %d, y: %d\n", x, y);
    
    int arr[] = {1, 2, 3, 4, 5};
    printf("Tamanho do array: %zu\n", ARRAY_SIZE(arr));
    
    size_t valor = 17;
    printf("Alinhar %zu para 8: %zu\n", valor, ALIGN_UP(valor, 8));
    
    printf("\n=== X-MACROS ===\n");
    printf("Cor VERDE = %d\n", VERDE);
    printf("Nome da cor: %s\n", nomes_cores[AZUL]);
    
    LOG(1, "Mensagem de log nível 1\n");
    LOG(3, "Mensagem de log nível 3: x=%d\n", x);
    
    // ASSERT(x > 0, "x deve ser positivo");
}
```

### Compilação Condicional Avançada
```c
#include <stdio.h>

// Detecção de plataforma
#if defined(_WIN32) || defined(_WIN64)
    #define PLATAFORMA "Windows"
    #define PATH_SEPARATOR '\\'
#elif defined(__linux__)
    #define PLATAFORMA "Linux"
    #define PATH_SEPARATOR '/'
#elif defined(__APPLE__)
    #define PLATAFORMA "macOS"
    #define PATH_SEPARATOR '/'
#else
    #define PLATAFORMA "Desconhecida"
    #define PATH_SEPARATOR '/'
#endif

// Detecção de compilador
#ifdef __GNUC__
    #define COMPILADOR "GCC"
    #define GCC_VERSION (__GNUC__ * 10000 + __GNUC_MINOR__ * 100 + __GNUC_PATCHLEVEL__)
#elif defined(_MSC_VER)
    #define COMPILADOR "MSVC"
#else
    #define COMPILADOR "Desconhecido"
#endif

// Features condicionais
#if __STDC_VERSION__ >= 201112L
    #define TEM_C11 1
#else
    #define TEM_C11 0
#endif

#if __STDC_VERSION__ >= 199901L
    #define TEM_C99 1
    #define FUNCAO_INLINE inline
#else
    #define TEM_C99 0
    #define FUNCAO_INLINE static
#endif

// Macros para warnings
#define IGNORAR_WARNING_PUSH _Pragma("GCC diagnostic push")
#define IGNORAR_WARNING(w) _Pragma(GCC diagnostic ignored #w)
#define IGNORAR_WARNING_POP _Pragma("GCC diagnostic pop")

// Atributos do GCC
#ifdef __GNUC__
    #define ATRIBUTO_NORETURN __attribute__((noreturn))
    #define ATRIBUTO_PACKED __attribute__((packed))
    #define ATRIBUTO_ALIGNED(x) __attribute__((aligned(x)))
    #define ATRIBUTO_UNUSED __attribute__((unused))
#else
    #define ATRIBUTO_NORETURN
    #define ATRIBUTO_PACKED
    #define ATRIBUTO_ALIGNED(x)
    #define ATRIBUTO_UNUSED
#endif

void exibir_info_compilacao() {
    printf("=== INFORMAÇÕES DE COMPILAÇÃO ===\n");
    printf("Plataforma: %s\n", PLATAFORMA);
    printf("Separador de caminho: %c\n", PATH_SEPARATOR);
    printf("Compilador: %s\n", COMPILADOR);
    
    #ifdef GCC_VERSION
    printf("Versão do GCC: %d\n", GCC_VERSION);
    #endif
    
    printf("C99: %s\n", TEM_C99 ? "Sim" : "Não");
    printf("C11: %s\n", TEM_C11 ? "Sim" : "Não");
    printf("Padrão C: %ld\n", __STDC_VERSION__);
}

// Estrutura com atributos
struct ATRIBUTO_PACKED DadosCompactos {
    char a;
    int b;
    char c;
};

struct ATRIBUTO_ALIGNED(16) DadosAlinhados {
    double x;
    double y;
};

void exemplo_atributos() {
    printf("\n=== ATRIBUTOS ===\n");
    
    printf("Tamanho DadosCompactos: %zu\n", sizeof(struct DadosCompactos));
    printf("Tamanho DadosAlinhados: %zu\n", sizeof(struct DadosAlinhados));
    
    ATRIBUTO_UNUSED int variavel_nao_usada = 42;
}

int main_preprocessador() {
    exibir_info_compilacao();
    exemplos_macros();
    exemplo_atributos();
    
    return 0;
}
```

### Include Guards e Pragma Once
```c
// Método tradicional com #ifndef
#ifndef MEU_ARQUIVO_H
#define MEU_ARQUIVO_H

// Conteúdo do header...

#endif // MEU_ARQUIVO_H

// Método moderno com #pragma once (não-padrão, mas amplamente suportado)
#pragma once

// Conteúdo do header...

// Header com múltiplas inclusões condicionais
#ifndef CONFIG_H
#define CONFIG_H

// Valores padrão
#ifndef MAX_CONEXOES
    #define MAX_CONEXOES 100
#endif

#ifndef TIMEOUT_PADRAO
    #define TIMEOUT_PADRAO 30
#endif

// Detecção de features
#if !defined(HAS_FEATURE_X) && defined(__linux__)
    #define HAS_FEATURE_X 1
#endif

// Inclusão condicional de headers
#ifdef USAR_SSL
    #include <openssl/ssl.h>
    #include <openssl/err.h>
#endif

#endif // CONFIG_H
```

---

## 5. MANIPULAÇÃO DE ARQUIVOS AVANÇADA

### Operações Binárias
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int id;
    char nome[50];
    float salario;
    unsigned char ativo; // 0 ou 1
} Funcionario;

void escrever_binario() {
    FILE *arquivo = fopen("funcionarios.bin", "wb");
    if(!arquivo) {
        perror("Erro ao abrir arquivo");
        return;
    }
    
    Funcionario funcionarios[] = {
        {1, "João Silva", 3000.50, 1},
        {2, "Maria Santos", 4500.75, 1},
        {3, "Carlos Oliveira", 2800.00, 0},
        {4, "Ana Costa", 5200.25, 1}
    };
    
    // Escrever array inteiro
    fwrite(funcionarios, sizeof(Funcionario), 4, arquivo);
    
    // Ou escrever um por um
    // for(int i = 0; i < 4; i++) {
    //     fwrite(&funcionarios[i], sizeof(Funcionario), 1, arquivo);
    // }
    
    fclose(arquivo);
    printf("Arquivo binário escrito com sucesso!\n");
}

void ler_binario() {
    FILE *arquivo = fopen("funcionarios.bin", "rb");
    if(!arquivo) {
        perror("Erro ao abrir arquivo");
        return;
    }
    
    // Ir para o final para descobrir tamanho
    fseek(arquivo, 0, SEEK_END);
    long tamanho = ftell(arquivo);
    rewind(arquivo);
    
    // Calcular número de registros
    size_t num_registros = tamanho / sizeof(Funcionario);
    
    // Alocar memória
    Funcionario *funcionarios = (Funcionario*)malloc(tamanho);
    if(!funcionarios) {
        fclose(arquivo);
        return;
    }
    
    // Ler todos os registros
    size_t lidos = fread(funcionarios, sizeof(Funcionario), num_registros, arquivo);
    
    printf("=== FUNCIONÁRIOS LIDOS DO ARQUIVO ===\n");
    for(size_t i = 0; i < lidos; i++) {
        printf("ID: %d\n", funcionarios[i].id);
        printf("Nome: %s\n", funcionarios[i].nome);
        printf("Salário: R$ %.2f\n", funcionarios[i].salario);
        printf("Ativo: %s\n", funcionarios[i].ativo ? "Sim" : "Não");
        printf("------------------------\n");
    }
    
    free(funcionarios);
    fclose(arquivo);
}

void acesso_aleatorio() {
    FILE *arquivo = fopen("funcionarios.bin", "rb+"); // Leitura e escrita
    if(!arquivo) {
        perror("Erro ao abrir arquivo");
        return;
    }
    
    // Ler registro específico (terceiro registro)
    Funcionario func;
    fseek(arquivo, 2 * sizeof(Funcionario), SEEK_SET); // Pular 2 registros
    fread(&func, sizeof(Funcionario), 1, arquivo);
    
    printf("Registro 3 antes: %s (Ativo: %d)\n", func.nome, func.ativo);
    
    // Modificar registro
    func.ativo = 1;
    func.salario += 500.00;
    
    // Voltar e escrever modificação
    fseek(arquivo, 2 * sizeof(Funcionario), SEEK_SET);
    fwrite(&func, sizeof(Funcionario), 1, arquivo);
    
    // Verificar
    fseek(arquivo, 2 * sizeof(Funcionario), SEEK_SET);
    fread(&func, sizeof(Funcionario), 1, arquivo);
    printf("Registro 3 depois: %s (Ativo: %d, Salário: %.2f)\n", 
           func.nome, func.ativo, func.salario);
    
    fclose(arquivo);
}

// Bufferização manual
void escrita_bufferizada() {
    FILE *arquivo = fopen("grande_arquivo.bin", "wb");
    if(!arquivo) return;
    
    // Aumentar buffer para melhor performance
    char buffer[8192]; // 8KB
    setvbuf(arquivo, buffer, _IOFBF, sizeof(buffer));
    
    // Escrever muitos dados
    for(int i = 0; i < 10000; i++) {
        int valor = i * 2;
        fwrite(&valor, sizeof(int), 1, arquivo);
    }
    
    // Forçar escrita imediata (flush)
    fflush(arquivo);
    
    fclose(arquivo);
}

int main_arquivos() {
    printf("=== ESCRITA BINÁRIA ===\n");
    escrever_binario();
    
    printf("\n=== LEITURA BINÁRIA ===\n");
    ler_binario();
    
    printf("\n=== ACESSO ALEATÓRIO ===\n");
    acesso_aleatorio();
    
    printf("\n=== ESCRITA BUFFERIZADA ===\n");
    escrita_bufferizada();
    
    return 0;
}
```

### Arquivos Mapeados em Memória (conceito)
```c
#ifdef __linux__
#include <sys/mman.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

void exemplo_mmap() {
    const char *filename = "arquivo_grande.bin";
    int fd = open(filename, O_RDONLY);
    if(fd == -1) {
        perror("open");
        return;
    }
    
    // Obter tamanho do arquivo
    struct stat sb;
    if(fstat(fd, &sb) == -1) {
        perror("fstat");
        close(fd);
        return;
    }
    
    // Mapear arquivo em memória
    void *addr = mmap(NULL, sb.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
    if(addr == MAP_FAILED) {
        perror("mmap");
        close(fd);
        return;
    }
    
    // Acessar conteúdo como array de bytes
    unsigned char *data = (unsigned char*)addr;
    
    // Processar dados diretamente na memória
    for(off_t i = 0; i < sb.st_size; i++) {
        // Processar byte data[i]
    }
    
    // Desmapear
    munmap(addr, sb.st_size);
    close(fd);
}
#endif
```

---

## 6. PROGRAMAÇÃO DE BAIXO NÍVEL

### Manipulação de Bits
```c
#include <stdio.h>
#include <stdint.h>

// Operações bit a bit
void operacoes_bits() {
    uint8_t a = 0b10101010; // 170
    uint8_t b = 0b11110000; // 240
    
    printf("a = 0x%02X (%d)\n", a, a);
    printf("b = 0x%02X (%d)\n", b, b);
    
    // AND bit a bit
    printf("a & b = 0x%02X\n", a & b);
    
    // OR bit a bit
    printf("a | b = 0x%02X\n", a | b);
    
    // XOR bit a bit
    printf("a ^ b = 0x%02X\n", a ^ b);
    
    // NOT bit a bit
    printf("~a = 0x%02X\n", (uint8_t)~a);
    
    // Shift left
    printf("a << 2 = 0x%02X\n", a << 2);
    
    // Shift right
    printf("a >> 2 = 0x%02X\n", a >> 2);
}

// Macros úteis para manipulação de bits
#define BIT(n) (1U << (n))
#define SET_BIT(var, n) ((var) |= BIT(n))
#define CLEAR_BIT(var, n) ((var) &= ~BIT(n))
#define TOGGLE_BIT(var, n) ((var) ^= BIT(n))
#define TEST_BIT(var, n) (((var) & BIT(n)) != 0)
#define EXTRACT_BITS(var, start, len) (((var) >> (start)) & ((1U << (len)) - 1))

void exemplo_macros_bits() {
    uint16_t flags = 0;
    
    // Setar bits
    SET_BIT(flags, 0);  // Bit 0 = 1
    SET_BIT(flags, 3);  // Bit 3 = 1
    SET_BIT(flags, 7);  // Bit 7 = 1
    
    printf("Flags após set: 0x%04X\n", flags);
    
    // Testar bits
    printf("Bit 3 está setado? %s\n", TEST_BIT(flags, 3) ? "Sim" : "Não");
    printf("Bit 5 está setado? %s\n", TEST_BIT(flags, 5) ? "Sim" : "Não");
    
    // Limpar bit
    CLEAR_BIT(flags, 3);
    printf("Flags após clear bit 3: 0x%04X\n", flags);
    
    // Alternar bit
    TOGGLE_BIT(flags, 5);
    printf("Flags após toggle bit 5: 0x%04X\n", flags);
    
    // Extrair bits 4-7
    uint8_t valor = EXTRACT_BITS(flags, 4, 4);
    printf("Bits 4-7: 0x%02X\n", valor);
}

// Bit twiddling hacks
void hacks_bits() {
    // Verificar se número é potência de 2
    int is_power_of_two(unsigned int x) {
        return x && !(x & (x - 1));
    }
    
    // Contar bits setados (popcount)
    int popcount(unsigned int x) {
        int count = 0;
        while(x) {
            x &= x - 1; // Limpa o bit menos significativo setado
            count++;
        }
        return count;
    }
    
    // Encontrar bit menos significativo setado
    int lsb(unsigned int x) {
        return x & -x; // Retorna apenas o LSB setado
    }
    
    // Arredondar para próxima potência de 2
    unsigned int next_power_of_two(unsigned int x) {
        x--;
        x |= x >> 1;
        x |= x >> 2;
        x |= x >> 4;
        x |= x >> 8;
        x |= x >> 16;
        x++;
        return x;
    }
    
    unsigned int num = 18;
    printf("Número: %u\n", num);
    printf("É potência de 2? %s\n", is_power_of_two(num) ? "Sim" : "Não");
    printf("Popcount: %d\n", popcount(num));
    printf("LSB: 0x%08X\n", lsb(num));
    printf("Próxima potência de 2: %u\n", next_power_of_two(num));
}

// Endianness (big-endian vs little-endian)
void verificar_endianness() {
    union {
        uint32_t i;
        char c[4];
    } test = {0x01020304};
    
    if(test.c[0] == 1) {
        printf("Big-endian\n");
    } else {
        printf("Little-endian\n");
    }
    
    // Conversão entre endianness
    uint32_t swap_endian(uint32_t x) {
        return ((x >> 24) & 0x000000FF) |
               ((x >> 8)  & 0x0000FF00) |
               ((x << 8)  & 0x00FF0000) |
               ((x << 24) & 0xFF000000);
    }
    
    uint32_t valor = 0x12345678;
    printf("Original: 0x%08X\n", valor);
    printf("Swapped:  0x%08X\n", swap_endian(valor));
}

int main_bits() {
    printf("=== OPERAÇÕES BIT A BIT ===\n");
    operacoes_bits();
    
    printf("\n=== MACROS PARA BITS ===\n");
    exemplo_macros_bits();
    
    printf("\n=== BIT TWIDDLING HACKS ===\n");
    hacks_bits();
    
    printf("\n=== ENDIANNESS ===\n");
    verificar_endianness();
    
    return 0;
}
```

### Inline Assembly (GCC)
```c
#ifdef __GNUC__
#include <stdio.h>

void exemplo_inline_asm() {
    int a = 10, b = 20, resultado;
    
    // Sintaxe básica
    __asm__ volatile (
        "add %1, %2\n\t"  // Instrução assembly
        "mov %2, %0"      // Outra instrução
        : "=r"(resultado)  // Output operands
        : "r"(a), "r"(b)   // Input operands
        :                  // Clobbered registers
    );
    
    printf("Resultado: %d + %d = %d\n", a, b, resultado);
    
    // Exemplo com múltiplas instruções
    int x = 5, y;
    
    __asm__ volatile (
        "mov %1, %%eax\n\t"   // Copiar x para eax
        "add $10, %%eax\n\t"  // Adicionar 10
        "mov %%eax, %0"       // Copiar para y
        : "=r"(y)
        : "r"(x)
        : "%eax"              // eax será modificado
    );
    
    printf("x = %d, y = %d\n", x, y);
    
    // Exemplo: CPUID
    unsigned int eax, ebx, ecx, edx;
    
    __asm__ volatile (
        "cpuid"
        : "=a"(eax), "=b"(ebx), "=c"(ecx), "=d"(edx)
        : "a"(0)  // Função 0: Get vendor string
    );
    
    char vendor[13];
    *(unsigned int*)&vendor[0] = ebx;
    *(unsigned int*)&vendor[4] = edx;
    *(unsigned int*)&vendor[8] = ecx;
    vendor[12] = '\0';
    
    printf("CPU Vendor: %s\n", vendor);
}

// Função com assembly inline
int soma_asm(int a, int b) {
    int resultado;
    
    __asm__ volatile (
        "addl %%ebx, %%eax"
        : "=a"(resultado)
        : "a"(a), "b"(b)
    );
    
    return resultado;
}

int main_asm() {
    exemplo_inline_asm();
    
    int r = soma_asm(100, 200);
    printf("Soma via assembly: %d\n", r);
    
    return 0;
}
#endif
```

---

## 7. TÉCNICAS DE OTIMIZAÇÃO

### Otimização de Código
```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

// 1. Otimização de Loops
void loops_otimizados() {
    const int N = 1000000;
    int *array = (int*)malloc(N * sizeof(int));
    
    // Loop não otimizado
    clock_t start = clock();
    for(int i = 0; i < N; i++) {
        array[i] = i * 2;
    }
    clock_t end = clock();
    printf("Loop normal: %.6f segundos\n", 
           (double)(end - start) / CLOCKS_PER_SEC);
    
    // Loop otimizado (reduzindo operações no loop)
    start = clock();
    int *ptr = array;
    int *end_ptr = array + N;
    int val = 0;
    while(ptr < end_ptr) {
        *ptr++ = val;
        val += 2;
    }
    end = clock();
    printf("Loop otimizado: %.6f segundos\n", 
           (double)(end - start) / CLOCKS_PER_SEC);
    
    free(array);
}

// 2. Inline Functions
static inline int max_inline(int a, int b) {
    return a > b ? a : b;
}

// 3. Lookup Tables (pré-cálculo)
float seno_rapido(float angulo) {
    static const float seno_table[360] = {
        // Valores pré-calculados do seno
    };
    
    int idx = (int)angulo % 360;
    if(idx < 0) idx += 360;
    
    return seno_table[idx];
}

// 4. Cache-friendly code
void accessos_cache() {
    const int SIZE = 10000;
    int **matrix = (int**)malloc(SIZE * sizeof(int*));
    for(int i = 0; i < SIZE; i++) {
        matrix[i] = (int*)malloc(SIZE * sizeof(int));
    }
    
    // Acesso ruim para cache (coluna por coluna)
    clock_t start = clock();
    for(int j = 0; j < SIZE; j++) {
        for(int i = 0; i < SIZE; i++) {
            matrix[i][j] = i + j;
        }
    }
    clock_t end = clock();
    printf("Acesso por coluna: %.6f segundos\n", 
           (double)(end - start) / CLOCKS_PER_SEC);
    
    // Acesso bom para cache (linha por linha)
    start = clock();
    for(int i = 0; i < SIZE; i++) {
        for(int j = 0; j < SIZE; j++) {
            matrix[i][j] = i + j;
        }
    }
    end = clock();
    printf("Acesso por linha: %.6f segundos\n", 
           (double)(end - start) / CLOCKS_PER_SEC);
    
    for(int i = 0; i < SIZE; i++) {
        free(matrix[i]);
    }
    free(matrix);
}

// 5. Loop Unrolling
void loop_unrolling() {
    const int N = 1024;
    int array[N];
    
    // Loop normal
    for(int i = 0; i < N; i++) {
        array[i] = i;
    }
    
    // Loop unrolled (desenrolado) 4x
    for(int i = 0; i < N; i += 4) {
        array[i] = i;
        array[i + 1] = i + 1;
        array[i + 2] = i + 2;
        array[i + 3] = i + 3;
    }
}

// 6. Branch Prediction
void branch_prediction() {
    const int N = 1000000;
    int *data = (int*)malloc(N * sizeof(int));
    
    // Gerar dados aleatórios
    for(int i = 0; i < N; i++) {
        data[i] = rand() % 100;
    }
    
    // Ordenar para ajudar branch prediction
    // qsort(data, N, sizeof(int), comparar_int);
    
    int sum = 0;
    clock_t start = clock();
    
    // Branch difícil de prever (dados aleatórios)
    for(int i = 0; i < N; i++) {
        if(data[i] < 50) {
            sum += data[i];
        }
    }
    
    clock_t end = clock();
    printf("Branch com dados aleatórios: %.6f segundos\n", 
           (double)(end - start) / CLOCKS_PER_SEC);
    printf("Soma: %d\n", sum);
    
    free(data);
}

// 7. Memória Alinhada
struct ATRIBUTO_ALIGNED(64) DadosAlinhados64 {
    int valores[16];
    double precisos[8];
};

void uso_memoria_alinhada() {
    // Alocação alinhada
    void *ptr;
    posix_memalign(&ptr, 64, 1024); // Alinhado em 64 bytes
    
    // Usar memória alinhada
    struct DadosAlinhados64 *dados = (struct DadosAlinhados64*)ptr;
    
    free(ptr);
}

// 8. Pré-cálculo e memoização
int fibonacci_memo[100] = {0};

int fibonacci_rapido(int n) {
    if(n <= 1) return n;
    
    // Se já calculado, retorna da memória
    if(fibonacci_memo[n] != 0) {
        return fibonacci_memo[n];
    }
    
    // Calcula e armazena
    fibonacci_memo[n] = fibonacci_rapido(n - 1) + fibonacci_rapido(n - 2);
    return fibonacci_memo[n];
}

int main_otimizacao() {
    printf("=== OTIMIZAÇÃO DE LOOPS ===\n");
    loops_otimizados();
    
    printf("\n=== CACHE-FRIENDLY CODE ===\n");
    accessos_cache();
    
    printf("\n=== BRANCH PREDICTION ===\n");
    branch_prediction();
    
    printf("\n=== FIBONACCI OTIMIZADO ===\n");
    clock_t start = clock();
    int fib30 = fibonacci_rapido(30);
    clock_t end = clock();
    printf("Fibonacci(30) = %d (tempo: %.6f segundos)\n", 
           fib30, (double)(end - start) / CLOCKS_PER_SEC);
    
    return 0;
}
```

### Profile-Guided Optimization (PGO)
```c
// Compilar com:
// gcc -fprofile-generate programa.c -o programa
// ./programa (executar com dados reais)
// gcc -fprofile-use programa.c -o programa_otimizado

#include <stdio.h>

// Função quente (chamada frequentemente)
int funcao_quente(int x) {
    return x * x + 2 * x + 1;
}

// Função fria (chamada raramente)
void funcao_fria() {
    // Código complexo mas raramente executado
    printf("Função raramente executada\n");
}

int main_pgo() {
    int total = 0;
    
    // Loop principal (será otimizado pelo PGO)
    for(int i = 0; i < 1000000; i++) {
        total += funcao_quente(i % 100);
        
        // Código condicional raro
        if(i == 999999) {
            funcao_fria();
        }
    }
    
    printf("Total: %d\n", total);
    return 0;
}
```

---

## 🎯 PROJETO AVANÇADO: IMPLEMENTAÇÃO DE UMA HASH TABLE

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdint.h>

#define TABLE_SIZE 1024
#define FNV_OFFSET_BASIS 2166136261U
#define FNV_PRIME 16777619U

// Estrutura para nó da lista encadeada
typedef struct Node {
    char *chave;
    void *valor;
    struct Node *proximo;
} Node;

// Estrutura da hash table
typedef struct {
    Node **buckets;
    size_t tamanho;
    size_t capacidade;
} HashTable;

// Função hash FNV-1a
uint32_t fnv1a_hash(const char *str) {
    uint32_t hash = FNV_OFFSET_BASIS;
    
    while(*str) {
        hash ^= (uint8_t)*str++;
        hash *= FNV_PRIME;
    }
    
    return hash;
}

// Criar nova hash table
HashTable* hash_table_criar(size_t capacidade) {
    HashTable *ht = (HashTable*)malloc(sizeof(HashTable));
    if(!ht) return NULL;
    
    ht->buckets = (Node**)calloc(capacidade, sizeof(Node*));
    if(!ht->buckets) {
        free(ht);
        return NULL;
    }
    
    ht->tamanho = 0;
    ht->capacidade = capacidade;
    return ht;
}

// Inserir na hash table
int hash_table_inserir(HashTable *ht, const char *chave, void *valor) {
    uint32_t hash = fnv1a_hash(chave);
    size_t index = hash % ht->capacidade;
    
    // Verificar se chave já existe
    Node *atual = ht->buckets[index];
    while(atual) {
        if(strcmp(atual->chave, chave) == 0) {
            // Atualizar valor existente
            atual->valor = valor;
            return 1;
        }
        atual = atual->proximo;
    }
    
    // Criar novo nó
    Node *novo = (Node*)malloc(sizeof(Node));
    if(!novo) return 0;
    
    novo->chave = strdup(chave);
    novo->valor = valor;
    novo->proximo = ht->buckets[index];
    ht->buckets[index] = novo;
    ht->tamanho++;
    
    // Redimensionar se necessário (load factor > 0.7)
    if((double)ht->tamanho / ht->capacidade > 0.7) {
        // Implementar redimensionamento
    }
    
    return 1;
}

// Buscar na hash table
void* hash_table_buscar(HashTable *ht, const char *chave) {
    uint32_t hash = fnv1a_hash(chave);
    size_t index = hash % ht->capacidade;
    
    Node *atual = ht->buckets[index];
    while(atual) {
        if(strcmp(atual->chave, chave) == 0) {
            return atual->valor;
        }
        atual = atual->proximo;
    }
    
    return NULL; // Não encontrado
}

// Exemplo de uso
void exemplo_hash_table() {
    HashTable *ht = hash_table_criar(TABLE_SIZE);
    
    // Inserir valores
    int valores[] = {100, 200, 300};
    hash_table_inserir(ht, "chave1", &valores[0]);
    hash_table_inserir(ht, "chave2", &valores[1]);
    hash_table_inserir(ht, "chave3", &valores[2]);
    
    // Buscar valores
    int *resultado = (int*)hash_table_buscar(ht, "chave2");
    if(resultado) {
        printf("Valor de 'chave2': %d\n", *resultado);
    }
    
    // Tentar buscar chave inexistente
    resultado = (int*)hash_table_buscar(ht, "chave4");
    if(!resultado) {
        printf("'chave4' não encontrada\n");
    }
}
```

---

## 📝 BOAS PRÁTICAS AVANÇADAS

1. **Sempre verifique retorno de malloc/calloc/realloc**
2. **Use valgrind ou sanitizers** para detectar memory leaks
3. **Documente invariantes e pré/pós-condições**
4. **Implemente testes de unidade** para código complexo
5. **Use static analysis tools** (cppcheck, splint)
6. **Considere portabilidade** ao usar features específicas
7. **Faça profiling antes de otimizar**
8. **Use const e restrict** quando apropriado
9. **Implemente destruidores** para estruturas complexas
10. **Trate erros graciosamente** com códigos de erro apropriados

---

## 🔗 PRÓXIMOS PASSOS

Para além deste manual:
- **Sistemas Operacionais**: Programação de sockets, threads, processos
- **Compiladores**: Análise léxica, sintática, geração de código
- **Embedded Systems**: Programação bare-metal, registros, interrupções
- **C Moderno**: C11/C17 features, _Generic, _Atomic
- **Bibliotecas Avançadas**: OpenMP, MPI, POSIX threads

---

**🚀 DESAFIO FINAL**: Implemente uma árvore AVL ou B-tree usando todas as técnicas aprendidas neste manual!

*Documento atualizado: Fevereiro 2024*  
*Nível: Avançado*  
*Recomendado: GCC 11+ ou Clang 14+*
