# Manual Completo da Linguagem C

## Índice
1. [Introdução](#introdução)
2. [Estrutura Básica](#estrutura-básica)
3. [Tipos de Dados](#tipos-de-dados)
4. [Variáveis](#variáveis)
5. [Operadores](#operadores)
6. [Controle de Fluxo](#controle-de-fluxo)
7. [Funções](#funções)
8. [Arrays](#arrays)
9. [Ponteiros](#ponteiros)
10. [Strings](#strings)
11. [Estruturas](#estruturas)
12. [Alocação Dinâmica](#alocação-dinâmica)
13. [Arquivos](#arquivos)
14. [Pré-processador](#pré-processador)
15. [Boas Práticas](#boas-práticas)

---

## Introdução

C é uma linguagem de programação de propósito geral, estruturada, imperativa e procedural. Criada por Dennis Ritchie entre 1969-1973 nos laboratórios Bell.

### Características Principais
- **Baixo nível**: Acesso direto à memória
- **Portável**: Código pode ser compilado em diferentes plataformas
- **Eficiente**: Alto desempenho
- **Minimalista**: Conjunto reduzido de palavras-chave (32)

---

## Estrutura Básica

### Template Mínimo
```c
#include <stdio.h>

int main() {
    // seu código aqui
    return 0;
}
```

### Elementos da Estrutura
1. **Diretivas de pré-processador** (#include, #define)
2. **Função main()** - Ponto de entrada obrigatório
3. **Comandos e expressões**
4. **Retorno**

### Comentários
```c
// Comentário de uma linha

/*
   Comentário
   de múltiplas
   linhas
*/
```

---

## Tipos de Dados

### Tipos Primitivos

| Tipo | Tamanho (bytes) | Intervalo | Especificador |
|------|-----------------|-----------|---------------|
| char | 1 | -128 a 127 | %c |
| unsigned char | 1 | 0 a 255 | %c |
| int | 2/4* | -32.768 a 32.767 | %d |
| unsigned int | 2/4* | 0 a 65.535 | %u |
| short | 2 | -32.768 a 32.767 | %hd |
| long | 4/8* | -2.147B a 2.147B | %ld |
| float | 4 | 1.2E-38 a 3.4E+38 | %f |
| double | 8 | 2.3E-308 a 1.7E+308 | %lf |
| void | 0 | - | - |

*Depende da arquitetura (32 ou 64 bits)

### Modificadores de Tipo
- **signed/unsigned**: Define sinal
- **short/long**: Altera tamanho
- **const**: Valor imutável
- **volatile**: Previne otimizações
- **static**: Duração estática/escopo limitado
- **register**: Sugere armazenamento em registrador

---

## Variáveis

### Declaração e Inicialização
```c
// Declaração
int idade;
float salario;
char letra;

// Declaração com inicialização
int contador = 0;
float pi = 3.14159;
char inicial = 'A';

// Múltiplas declarações
int a, b, c = 10; // apenas c é inicializado

// Constantes
const int MAX = 100;
const float TAXA = 0.15;
```

### Escopo de Variáveis
```c
#include <stdio.h>

int global = 10; // Variável global

void funcao() {
    int local = 5; // Variável local
    static int estatica = 0; // Mantém valor entre chamadas
    estatica++;
    printf("Estatica: %d\n", estatica);
}
```

### Conversão de Tipos (Casting)
```c
int a = 10;
float b = 3.14;
int c = (int)b;  // Conversão explícita: c = 3
float d = a;     // Conversão implícita: d = 10.0
```

---

## Operadores

### Operadores Aritméticos
```c
int a = 10, b = 3;
int soma = a + b;        // 13
int subtracao = a - b;   // 7
int multiplicacao = a * b; // 30
int divisao = a / b;     // 3 (divisão inteira)
float divisao_real = (float)a / b; // 3.333
int resto = a % b;       // 1
```

### Operadores Relacionais
```c
a == b  // igual a
a != b  // diferente de
a > b   // maior que
a < b   // menor que
a >= b  // maior ou igual
a <= b  // menor ou igual
```

### Operadores Lógicos
```c
condicao1 && condicao2  // E lógico (AND)
condicao1 || condicao2  // OU lógico (OR)
!condicao               // NÃO lógico (NOT)
```

### Operadores de Atribuição
```c
a = 5;     // atribuição simples
a += 3;    // a = a + 3
a -= 2;    // a = a - 2
a *= 4;    // a = a * 4
a /= 2;    // a = a / 2
a %= 3;    // a = a % 3
a &= b;    // a = a & b
a |= b;    // a = a | b
```

### Operadores de Incremento/Decremento
```c
a++;  // pós-incremento: usa o valor, depois incrementa
++a;  // pré-incremento: incrementa, depois usa o valor
a--;  // pós-decremento
--a;  // pré-decremento
```

### Operadores Bit a Bit
```c
int a = 5;     // 0101
int b = 3;     // 0011

a & b   // AND: 0001 (1)
a | b   // OR:  0111 (7)
a ^ b   // XOR: 0110 (6)
~a      // NOT: 1010 (-6 em complemento de 2)
a << 1  // shift left: 1010 (10)
a >> 1  // shift right: 0010 (2)
```

### Operador Ternário
```c
int idade = 18;
char* status = (idade >= 18) ? "Adulto" : "Menor";
```

### Operador sizeof
```c
printf("int: %zu bytes\n", sizeof(int));
printf("float: %zu bytes\n", sizeof(float));
int array[10];
printf("Array: %zu bytes\n", sizeof(array));
```

---

## Controle de Fluxo

### Condicional if-else
```c
if (condicao) {
    // código se verdadeiro
} else if (outra_condicao) {
    // código se outra condição verdadeira
} else {
    // código se todas falsas
}

// Exemplo prático
int nota = 75;
if (nota >= 90) {
    printf("A\n");
} else if (nota >= 80) {
    printf("B\n");
} else if (nota >= 70) {
    printf("C\n");
} else {
    printf("Reprovado\n");
}
```

### Switch-case
```c
int opcao = 2;
switch (opcao) {
    case 1:
        printf("Opção 1\n");
        break;
    case 2:
        printf("Opção 2\n");
        break;
    case 3:
        printf("Opção 3\n");
        break;
    default:
        printf("Opção inválida\n");
}

// Fall-through (sem break)
char vogal = 'a';
switch (vogal) {
    case 'a':
    case 'e':
    case 'i':
    case 'o':
    case 'u':
        printf("É vogal\n");
        break;
    default:
        printf("É consoante\n");
}
```

### Loops

#### for
```c
for (inicialização; condição; incremento) {
    // código repetido
}

// Exemplos
for (int i = 0; i < 10; i++) {
    printf("%d ", i);
}

// Loop decrescente
for (int i = 10; i > 0; i--) {
    printf("%d ", i);
}

// Múltiplas variáveis
for (int i = 0, j = 10; i < j; i++, j--) {
    printf("i=%d, j=%d\n", i, j);
}
```

#### while
```c
while (condicao) {
    // código repetido
}

// Exemplo
int i = 0;
while (i < 5) {
    printf("%d\n", i);
    i++;
}
```

#### do-while
```c
do {
    // código executado pelo menos uma vez
} while (condicao);

// Exemplo
int numero;
do {
    printf("Digite um número positivo: ");
    scanf("%d", &numero);
} while (numero <= 0);
```

### Controle de Loop
```c
for (int i = 0; i < 10; i++) {
    if (i == 3) continue;  // pula iteração
    if (i == 7) break;     // sai do loop
    printf("%d\n", i);
}
```

---

## Funções

### Declaração e Definição
```c
// Protótipo (declaração)
int soma(int a, int b);
void imprimir_mensagem(char* msg);
float media(float valores[], int tamanho);

// Definição
int soma(int a, int b) {
    return a + b;
}

void imprimir_mensagem(char* msg) {
    printf("%s\n", msg);
    // sem return
}

float media(float valores[], int tamanho) {
    float total = 0;
    for (int i = 0; i < tamanho; i++) {
        total += valores[i];
    }
    return total / tamanho;
}
```

### Passagem de Parâmetros

#### Por Valor (cópia)
```c
void troca_errado(int a, int b) {
    int temp = a;
    a = b;
    b = temp;  // só troca localmente
}
```

#### Por Referência (ponteiros)
```c
void troca_certo(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Uso
int x = 5, y = 10;
troca_certo(&x, &y);  // x=10, y=5
```

### Funções Recursivas
```c
// Fatorial
int fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}

// Fibonacci
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Torres de Hanoi
void hanoi(int n, char origem, char destino, char auxiliar) {
    if (n == 1) {
        printf("Mover disco 1 de %c para %c\n", origem, destino);
        return;
    }
    hanoi(n - 1, origem, auxiliar, destino);
    printf("Mover disco %d de %c para %c\n", n, origem, destino);
    hanoi(n - 1, auxiliar, destino, origem);
}
```

### Funções com Número Variável de Argumentos
```c
#include <stdarg.h>

int soma_varios(int n, ...) {
    va_list args;
    int total = 0;
    
    va_start(args, n);
    for (int i = 0; i < n; i++) {
        total += va_arg(args, int);
    }
    va_end(args);
    
    return total;
}

// Uso
int resultado = soma_varios(3, 10, 20, 30);  // 60
```

---

## Arrays

### Declaração e Inicialização
```c
// Array unidimensional
int numeros[5];  // sem inicialização
int valores[5] = {1, 2, 3, 4, 5};  // completa
int parciais[5] = {1, 2};  // resto = 0
int automatico[] = {1, 2, 3, 4, 5};  // tamanho automático

// Array bidimensional
int matriz[3][4];  // 3 linhas, 4 colunas
int tabuleiro[3][3] = {
    {1, 0, 0},
    {0, 1, 0},
    {0, 0, 1}
};

// Array tridimensional
int cubo[2][3][4];
```

### Acesso e Manipulação
```c
int arr[5] = {10, 20, 30, 40, 50};

// Acesso
printf("%d\n", arr[0]);  // primeiro elemento
printf("%d\n", arr[4]);  // último elemento

// Modificação
arr[2] = 35;

// Percorrendo
for (int i = 0; i < 5; i++) {
    printf("arr[%d] = %d\n", i, arr[i]);
}
```

### Arrays Multidimensionais
```c
int matriz[2][3] = {{1, 2, 3}, {4, 5, 6}};

// Percorrendo matriz
for (int i = 0; i < 2; i++) {
    for (int j = 0; j < 3; j++) {
        printf("%d ", matriz[i][j]);
    }
    printf("\n");
}
```

### Arrays como Parâmetros
```c
void imprimir_array(int arr[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        printf("%d ", arr[i]);
    }
}

void modificar_array(int *arr, int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        arr[i] *= 2;  // equivalente a *(arr + i) *= 2
    }
}
```

---

## Ponteiros

### Conceitos Básicos
```c
int x = 10;
int *ptr = &x;  // ptr armazena o endereço de x

printf("Valor de x: %d\n", x);      // 10
printf("Endereço de x: %p\n", &x);  // endereço
printf("Valor de ptr: %p\n", ptr);  // mesmo endereço
printf("Conteúdo apontado: %d\n", *ptr);  // 10 (dereferência)
```

### Ponteiros e Arrays
```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;  // equivalente a &arr[0]

// Formas equivalentes
printf("%d\n", arr[2]);    // 30
printf("%d\n", *(arr + 2)); // 30
printf("%d\n", p[2]);       // 30
printf("%d\n", *(p + 2));   // 30

// Aritmética de ponteiros
p++;  // agora aponta para arr[1]
```

### Ponteiros para Ponteiros
```c
int x = 10;
int *p = &x;
int **pp = &p;

printf("%d\n", **pp);  // 10
```

### Ponteiros para Funções
```c
int soma(int a, int b) { return a + b; }
int multiplica(int a, int b) { return a * b; }

int (*operacao)(int, int);  // declaração

operacao = soma;
printf("%d\n", operacao(5, 3));  // 8

operacao = multiplica;
printf("%d\n", operacao(5, 3));  // 15

// Array de ponteiros para funções
int (*ops[])(int, int) = {soma, multiplica};
```

### Ponteiro NULL
```c
int *ptr = NULL;  // ponteiro não inicializado

if (ptr != NULL) {
    *ptr = 10;  // seguro apenas se não for NULL
}
```

### Ponteiro void
```c
void *ptr;  // ponteiro genérico
int x = 10;
float y = 3.14;

ptr = &x;
printf("%d\n", *(int*)ptr);  // cast necessário

ptr = &y;
printf("%f\n", *(float*)ptr);
```

---

## Strings

### Declaração e Inicialização
```c
// Como array de char
char str1[] = "Hello";  // {'H','e','l','l','o','\0'}
char str2[20] = "World";
char str3[10] = {'C', ' ', 'l', 'a', 'n', 'g', '\0'};

// Como ponteiro
char *str4 = "Constante";  // string literal (imutável)
```

### Funções de String (string.h)

```c
#include <string.h>

char dest[50] = "Hello ";
char src[] = "World";
char str[50];

// Copiar
strcpy(dest, src);  // dest = "World"
strncpy(dest, src, 3);  // dest = "Worldo" (copia 3 chars)

// Concatenar
strcat(dest, "!");  // dest = "World!"
strncat(dest, "!!", 2);  // dest = "World!!!"

// Comparar
int cmp = strcmp("abc", "abc");  // 0 (igual)
cmp = strcmp("abc", "abd");      // negativo
cmp = strcmp("abd", "abc");      // positivo

// Tamanho
int len = strlen("Hello");  // 5 (sem o \0)

// Buscar
char *pos = strchr("Hello", 'e');  // encontra 'e'
pos = strstr("Hello World", "World");  // encontra substring

// Tokenizar
char frase[] = "Isto,é:um teste";
char *token = strtok(frase, ",:");
while (token != NULL) {
    printf("%s\n", token);
    token = strtok(NULL, ",:");
}
```

### Funções Personalizadas
```c
// Implementação de strlen
size_t minha_strlen(const char *s) {
    const char *p = s;
    while (*p) p++;
    return p - s;
}

// Implementação de strcpy
char* minha_strcpy(char *dest, const char *src) {
    char *d = dest;
    while ((*d++ = *src++)) ;
    return dest;
}

// Inverter string
void inverter_string(char str[]) {
    int i = 0, j = strlen(str) - 1;
    while (i < j) {
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;
        i++; j--;
    }
}
```

### Entrada e Saída de Strings
```c
char nome[50];

// Perigoso (buffer overflow)
scanf("%s", nome);

// Seguro
scanf("%49s", nome);  // limita tamanho
fgets(nome, sizeof(nome), stdin);  // inclui \n
nome[strcspn(nome, "\n")] = '\0';  // remove \n

// Saída
printf("%s\n", nome);
puts(nome);  // adiciona \n automaticamente
fputs(nome, stdout);  // sem \n automático
```

---

## Estruturas

### Definição e Declaração
```c
// Definição simples
struct Pessoa {
    char nome[50];
    int idade;
    float altura;
};

// Declaração
struct Pessoa p1;
struct Pessoa p2 = {"João", 25, 1.75};

// Acesso
strcpy(p1.nome, "Maria");
p1.idade = 30;
p1.altura = 1.65;

// Typedef (recomendado)
typedef struct {
    char nome[50];
    int idade;
    float altura;
} Pessoa;

Pessoa p3;  // não precisa de "struct"
```

### Estruturas Aninhadas
```c
typedef struct {
    int dia;
    int mes;
    int ano;
} Data;

typedef struct {
    char nome[50];
    Data nascimento;
    float salario;
} Funcionario;

Funcionario f = {"Ana", {15, 5, 1990}, 5000.0};
printf("Ano: %d\n", f.nascimento.ano);
```

### Arrays de Estruturas
```c
Pessoa turma[3] = {
    {"Ana", 20, 1.70},
    {"Bruno", 22, 1.80},
    {"Carla", 19, 1.65}
};

for (int i = 0; i < 3; i++) {
    printf("%s tem %d anos\n", turma[i].nome, turma[i].idade);
}
```

### Ponteiros para Estruturas
```c
Pessoa *p = &turma[0];
p->idade = 21;  // equivalente a (*p).idade = 21
strcpy(p->nome, "Ana Paula");
```

### Alinhamento e Padding
```c
struct Exemplo1 {
    char c;     // 1 byte
    int i;      // 4 bytes (alinhado em 4)
    short s;    // 2 bytes
};  // tamanho total: 12 bytes (com padding)

struct Exemplo2 {
    int i;      // 4 bytes
    short s;    // 2 bytes
    char c;     // 1 byte
};  // tamanho total: 8 bytes (otimizado)

// Forçar sem padding
#pragma pack(push, 1)
struct Compactado {
    char c;
    int i;
    short s;
};  // tamanho: 7 bytes
#pragma pack(pop)
```

### União (Union)
```c
union Dado {
    int i;
    float f;
    char str[20];
};  // compartilha o mesmo espaço de memória

union Dado d;
d.i = 42;
printf("%d\n", d.i);  // 42
printf("%f\n", d.f);  // valor imprevisível
```

### Enumerações
```c
enum Dia {SEG, TER, QUA, QUI, SEX, SAB, DOM};
enum Status {SUCESSO = 0, ERRO = -1, PENDENTE = 1};

enum Dia hoje = QUA;
printf("%d\n", hoje);  // 2

// Enum com typedef
typedef enum {FALSE = 0, TRUE = 1} bool;
bool flag = TRUE;
```

---

## Alocação Dinâmica

### Funções de Alocação

```c
#include <stdlib.h>

// malloc - aloca mas não inicializa
int *p = (int*)malloc(10 * sizeof(int));
if (p == NULL) {
    // erro de alocação
}

// calloc - aloca e zera
int *q = (int*)calloc(10, sizeof(int));

// realloc - redimensiona
int *r = (int*)realloc(p, 20 * sizeof(int));

// free - libera
free(p);
free(q);
free(r);
```

### Exemplos Práticos

```c
// Array dinâmico
int n;
printf("Quantos números? ");
scanf("%d", &n);

int *numeros = (int*)malloc(n * sizeof(int));
if (numeros == NULL) {
    printf("Erro de alocação\n");
    return 1;
}

for (int i = 0; i < n; i++) {
    numeros[i] = i * 10;
}

free(numeros);

// Matriz dinâmica
int linhas = 3, colunas = 4;
int **matriz = (int**)malloc(linhas * sizeof(int*));

for (int i = 0; i < linhas; i++) {
    matriz[i] = (int*)malloc(colunas * sizeof(int));
}

// Liberação
for (int i = 0; i < linhas; i++) {
    free(matriz[i]);
}
free(matriz);
```

### Estrutura Dinâmica (Lista Encadeada)
```c
typedef struct No {
    int dado;
    struct No *proximo;
} No;

No* criar_no(int valor) {
    No *novo = (No*)malloc(sizeof(No));
    if (novo != NULL) {
        novo->dado = valor;
        novo->proximo = NULL;
    }
    return novo;
}

void inserir_inicio(No **cabeca, int valor) {
    No *novo = criar_no(valor);
    if (novo != NULL) {
        novo->proximo = *cabeca;
        *cabeca = novo;
    }
}

void liberar_lista(No *cabeca) {
    No *atual = cabeca;
    while (atual != NULL) {
        No *proximo = atual->proximo;
        free(atual);
        atual = proximo;
    }
}
```

---

## Arquivos

### Modos de Abertura

| Modo | Descrição |
|------|-----------|
| "r" | Leitura |
| "w" | Escrita (cria/sobrescreve) |
| "a" | Append (adiciona ao final) |
| "rb" | Leitura binária |
| "wb" | Escrita binária |
| "ab" | Append binário |
| "r+" | Leitura/escrita |
| "w+" | Leitura/escrita (cria/sobrescreve) |
| "a+" | Leitura/append |

### Operações com Arquivos Texto

```c
#include <stdio.h>

// Escrita
FILE *fp = fopen("arquivo.txt", "w");
if (fp != NULL) {
    fprintf(fp, "Nome: %s\nIdade: %d\n", "João", 30);
    fputs("Linha de texto\n", fp);
    fclose(fp);
}

// Leitura
char linha[100];
fp = fopen("arquivo.txt", "r");
if (fp != NULL) {
    // fscanf
    char nome[50];
    int idade;
    fscanf(fp, "Nome: %s\nIdade: %d\n", nome, &idade);
    
    // fgets
    while (fgets(linha, sizeof(linha), fp) != NULL) {
        printf("%s", linha);
    }
    
    // fgetc
    int c;
    while ((c = fgetc(fp)) != EOF) {
        putchar(c);
    }
    
    fclose(fp);
}
```

### Arquivos Binários

```c
typedef struct {
    int id;
    char nome[50];
    float salario;
} Registro;

// Escrita binária
Registro reg = {1, "Maria", 5000.0};
FILE *fp = fopen("dados.bin", "wb");
fwrite(&reg, sizeof(Registro), 1, fp);
fclose(fp);

// Leitura binária
Registro reg_lida;
fp = fopen("dados.bin", "rb");
fread(&reg_lida, sizeof(Registro), 1, fp);
fclose(fp);
```

### Posicionamento no Arquivo

```c
FILE *fp = fopen("arquivo.txt", "r");

// Ir para posição específica
fseek(fp, 10, SEEK_SET);  // 10 bytes do início
fseek(fp, -5, SEEK_CUR);  // 5 bytes antes da posição atual
fseek(fp, -10, SEEK_END); // 10 bytes antes do fim

// Posição atual
long pos = ftell(fp);

// Voltar ao início
rewind(fp);

fclose(fp);
```

---

## Pré-processador

### Diretivas Principais

```c
// Inclusão de arquivos
#include <stdio.h>      // arquivos do sistema
#include "meuheader.h"  // arquivos locais

// Definição de constantes
#define PI 3.14159
#define MAX 100
#define ERRO -1

// Macros
#define SOMA(a,b) ((a) + (b))
#define QUADRADO(x) ((x) * (x))
#define MAIOR(a,b) ((a) > (b) ? (a) : (b))

// Macro com múltiplas linhas
#define IMPRIMIR_DADOS(n, i) \
    do { \
        printf("Nome: %s\n", n); \
        printf("Idade: %d\n", i); \
    } while(0)

// Compilação condicional
#ifdef DEBUG
    #define LOG(msg) printf("DEBUG: %s\n", msg)
#else
    #define LOG(msg)
#endif

#ifndef HEADER_H
#define HEADER_H
// conteúdo do header
#endif

#if VERSAO == 1
    // código versão 1
#elif VERSAO == 2
    // código versão 2
#else
    // código padrão
#endif

// Pragmas
#pragma once  // incluir apenas uma vez
#pragma warning(disable: 4996)  // desativa aviso específico
```

### Operadores do Pré-processador

```c
#define STR(x) #x  // stringização
#define CONCAT(a,b) a##b  // concatenação

printf(STR(testando));  // "testando"
int xy = 10;
CONCAT(x,y) = 20;  // xy = 20
```

### Macros Predefinidas

```c
printf("Arquivo: %s\n", __FILE__);
printf("Linha: %d\n", __LINE__);
printf("Data: %s\n", __DATE__);
printf("Hora: %s\n", __TIME__);
printf("Função: %s\n", __FUNCTION__);  // C99
printf("Padrão C: %ld\n", __STDC_VERSION__);  // C99
```

---

## Boas Práticas

### Convenções de Nomenclatura

```c
// Constantes: MAIÚSCULAS
#define TAMANHO_MAX 100
const int LIMITE_VELOCIDADE = 120;

// Variáveis: snake_case
int contador_usuario = 0;
float media_temperatura;

// Funções: snake_case
void calcular_media(void);
int obter_idade(void);

// Tipos: PascalCase ou _t
typedef struct {
    int x, y;
} Ponto;
typedef unsigned int uint32_t;
```

### Organização de Código

```c
// arquivo.h
#ifndef ARQUIVO_H
#define ARQUIVO_H

// includes necessários
#include <stdio.h>

// declarações
typedef struct {
    int id;
    char nome[50];
} Item;

// protótipos
void processar_item(Item *item);
int validar_item(const Item *item);

#endif

// arquivo.c
#include "arquivo.h"

// implementações
void processar_item(Item *item) {
    // código
}
```

### Tratamento de Erros

```c
#include <errno.h>
#include <string.h>

FILE *fp = fopen("arquivo.txt", "r");
if (fp == NULL) {
    fprintf(stderr, "Erro %d: %s\n", errno, strerror(errno));
    return 1;
}

int *ptr = malloc(1000 * sizeof(int));
if (ptr == NULL) {
    perror("Erro de alocação");
    exit(EXIT_FAILURE);
}
```

### Dicas de Otimização

```c
// Use pré-incremento quando possível
for (int i = 0; i < 100; ++i)  // ++i é mais eficiente que i++

// Evite cálculos repetidos no loop
int tamanho = strlen(str);
for (int i = 0; i < tamanho; i++)  // strlen não é recalculada

// Use registradores para variáveis críticas
register int contador = 0;

// Prefira switch sobre múltiplos if-else
// Use inline para funções pequenas
static inline int quadrado(int x) { return x * x; }
```

### Segurança

```c
// Evite buffer overflow
char buffer[10];
fgets(buffer, sizeof(buffer), stdin);  // seguro
snprintf(buffer, sizeof(buffer), "%d", valor);  // seguro

// Verifique limites
if (indice >= 0 && indice < TAMANHO) {
    array[indice] = valor;
}

// Inicialize ponteiros
int *ptr = NULL;

// Libere memória alocada
free(ptr);
ptr = NULL;  // evita dangling pointer

// Use const para parâmetros que não são modificados
void imprimir(const char *str) {
    printf("%s", str);
}
```

---

## Exemplo Completo

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>

#define MAX_NOME 50
#define MAX_ALUNOS 100

typedef struct {
    int id;
    char nome[MAX_NOME];
    float notas[3];
    float media;
} Aluno;

float calcular_media(float notas[], int quantidade) {
    float soma = 0;
    for (int i = 0; i < quantidade; i++) {
        soma += notas[i];
    }
    return quantidade > 0 ? soma / quantidade : 0;
}

void inicializar_aluno(Aluno *aluno, int id, const char *nome) {
    aluno->id = id;
    strncpy(aluno->nome, nome, MAX_NOME - 1);
    aluno->nome[MAX_NOME - 1] = '\0';
    
    for (int i = 0; i < 3; i++) {
        aluno->notas[i] = 0;
    }
    aluno->media = 0;
}

void adicionar_nota(Aluno *aluno, int prova, float nota) {
    if (prova >= 0 && prova < 3) {
        aluno->notas[prova] = nota;
        aluno->media = calcular_media(aluno->notas, 3);
    }
}

void imprimir_aluno(const Aluno *aluno) {
    printf("ID: %d\n", aluno->id);
    printf("Nome: %s\n", aluno->nome);
    printf("Notas: %.1f, %.1f, %.1f\n", 
           aluno->notas[0], aluno->notas[1], aluno->notas[2]);
    printf("Média: %.1f\n", aluno->media);
}

int salvar_alunos(const Aluno alunos[], int quantidade, const char *arquivo) {
    FILE *fp = fopen(arquivo, "wb");
    if (fp == NULL) {
        perror("Erro ao abrir arquivo");
        return -1;
    }
    
    size_t escrita = fwrite(alunos, sizeof(Aluno), quantidade, fp);
    fclose(fp);
    
    return escrita == quantidade ? 0 : -1;
}

int carregar_alunos(Aluno alunos[], int capacidade, const char *arquivo) {
    FILE *fp = fopen(arquivo, "rb");
    if (fp == NULL) {
        return 0;  // arquivo não existe
    }
    
    int quantidade = fread(alunos, sizeof(Aluno), capacidade, fp);
    fclose(fp);
    
    return quantidade;
}

int main() {
    Aluno turma[MAX_ALUNOS];
    int num_alunos = 0;
    
    // Carregar dados existentes
    num_alunos = carregar_alunos(turma, MAX_ALUNOS, "alunos.dat");
    printf("Carregados %d alunos\n", num_alunos);
    
    // Adicionar novo aluno
    if (num_alunos < MAX_ALUNOS) {
        inicializar_aluno(&turma[num_alunos], num_alunos + 1, "João Silva");
        adicionar_nota(&turma[num_alunos], 0, 8.5);
        adicionar_nota(&turma[num_alunos], 1, 7.0);
        adicionar_nota(&turma[num_alunos], 2, 9.0);
        num_alunos++;
    }
    
    // Imprimir todos os alunos
    for (int i = 0; i < num_alunos; i++) {
        printf("\n--- Aluno %d ---\n", i + 1);
        imprimir_aluno(&turma[i]);
    }
    
    // Salvar dados
    if (salvar_alunos(turma, num_alunos, "alunos.dat") == 0) {
        printf("\nDados salvos com sucesso!\n");
    } else {
        printf("\nErro ao salvar dados!\n");
    }
    
    return 0;
}
```

---

Este manual cobre a sintaxe essencial da linguagem C, desde conceitos básicos até tópicos avançados. Pratique cada conceito e consulte a documentação oficial para aprofundamento em tópicos específicos.
