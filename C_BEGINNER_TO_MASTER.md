# Manual Completo de C: Do Iniciante à Maestria 📚

## Índice
1. [Introdução](#introdução)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Fundamentos Básicos](#fundamentos-básicos)
4. [Controle de Fluxo](#controle-de-fluxo)
5. [Funções](#funções)
6. [Arrays e Strings](#arrays-e-strings)
7. [Ponteiros](#ponteiros)
8. [Alocação Dinâmica](#alocação-dinâmica)
9. [Estruturas e Uniões](#estruturas-e-uniões)
10. [Entrada e Saída](#entrada-e-saída)
11. [Pré-processador](#pré-processador)
12. [Tópicos Avançados](#tópicos-avançados)
13. [Boas Práticas](#boas-práticas)
14. [Projetos Práticos](#projetos-práticos)

---

## Introdução

### O que é C?
C é uma linguagem de programação de propósito geral, estruturada, imperativa e procedural, criada por Dennis Ritchie entre 1969-1973 nos laboratórios Bell.

### Por que aprender C?
- Base para muitas linguagens modernas (C++, Java, Python, etc.)
- Controle total sobre hardware
- Performance excepcional
- Amplamente usado em sistemas embarcados, sistemas operacionais e jogos

---

## Configuração do Ambiente

### Instalação do Compilador

**Linux (GCC):**
```bash
sudo apt-get update
sudo apt-get install gcc build-essential
```

**Windows (MinGW):**
1. Baixe o MinGW em: https://sourceforge.net/projects/mingw/
2. Instale e adicione ao PATH

**macOS (Clang):**
```bash
xcode-select --install
```

### Primeiro Programa

```c
#include <stdio.h>

int main() {
    printf("Olá, Mundo!\n");
    return 0;
}
```

**Compilação:**
```bash
gcc programa.c -o programa
./programa
```

---

## Fundamentos Básicos

### Estrutura Básica de um Programa

```c
#include <stdio.h>  // Diretivas de pré-processador

// Função principal
int main() {
    // Seu código aqui
    return 0;
}
```

### Variáveis e Tipos de Dados

```c
#include <stdio.h>

int main() {
    // Tipos básicos
    int idade = 25;           // inteiro
    float altura = 1.75;      // ponto flutuante
    double peso = 70.5;       // ponto flutuante dupla precisão
    char letra = 'A';          // caractere
    char nome[] = "João";      // string (array de caracteres)
    
    printf("Idade: %d\n", idade);
    printf("Altura: %.2f\n", altura);
    printf("Peso: %.1lf\n", peso);
    printf("Letra: %c\n", letra);
    printf("Nome: %s\n", nome);
    
    return 0;
}
```

### Modificadores de Tipo

```c
short int a;      // inteiro curto
long int b;       // inteiro longo
unsigned int c;   // inteiro sem sinal (apenas positivo)
signed int d;     // inteiro com sinal (padrão)
```

### Operadores

```c
// Aritméticos
int soma = 5 + 3;
int subtracao = 5 - 3;
int multiplicacao = 5 * 3;
int divisao = 5 / 3;
int resto = 5 % 3;

// Relacionais
a == b  // igual a
a != b  // diferente de
a < b   // menor que
a > b   // maior que
a <= b  // menor ou igual
a >= b  // maior ou igual

// Lógicos
&&  // AND
||  // OR
!   // NOT

// Atribuição
a = 5;
a += 3;  // a = a + 3
a -= 3;  // a = a - 3
a *= 3;  // a = a * 3
a /= 3;  // a = a / 3

// Incremento/Decremento
a++;  // pós-incremento
++a;  // pré-incremento
a--;  // pós-decremento
--a;  // pré-decremento
```

---

## Controle de Fluxo

### Condicionais

```c
// if-else
int idade = 18;

if (idade >= 18) {
    printf("Maior de idade\n");
} else {
    printf("Menor de idade\n");
}

// else if
int nota = 85;

if (nota >= 90) {
    printf("A\n");
} else if (nota >= 80) {
    printf("B\n");
} else if (nota >= 70) {
    printf("C\n");
} else {
    printf("Reprovado\n");
}

// switch
int opcao = 2;

switch(opcao) {
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
```

### Loops

```c
// for
for(int i = 0; i < 10; i++) {
    printf("i = %d\n", i);
}

// while
int i = 0;
while(i < 10) {
    printf("i = %d\n", i);
    i++;
}

// do-while (executa pelo menos uma vez)
int j = 0;
do {
    printf("j = %d\n", j);
    j++;
} while(j < 10);

// break e continue
for(int i = 0; i < 10; i++) {
    if(i == 5) {
        break;  // sai do loop quando i = 5
    }
    if(i == 2) {
        continue;  // pula iteração quando i = 2
    }
    printf("i = %d\n", i);
}
```

---

## Funções

### Declaração e Definição

```c
// Declaração (protótipo)
int soma(int a, int b);
void imprimirMensagem(void);

// Definição
int soma(int a, int b) {
    return a + b;
}

void imprimirMensagem() {
    printf("Olá!\n");
}

int main() {
    int resultado = soma(5, 3);
    imprimirMensagem();
    printf("Resultado: %d\n", resultado);
    return 0;
}
```

### Passagem por Valor vs Referência

```c
// Por valor (cópia)
void incrementar(int x) {
    x++;
}

// Por referência (usando ponteiros)
void incrementarRef(int *x) {
    (*x)++;
}

int main() {
    int a = 5;
    incrementar(a);
    printf("a = %d\n", a);  // a = 5
    
    incrementarRef(&a);
    printf("a = %d\n", a);  // a = 6
    
    return 0;
}
```

### Recursão

```c
// Fatorial recursivo
int fatorial(int n) {
    if(n <= 1) {
        return 1;
    }
    return n * fatorial(n - 1);
}

// Fibonacci recursivo
int fibonacci(int n) {
    if(n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

---

## Arrays e Strings

### Arrays

```c
// Declaração e inicialização
int numeros[5];  // array de 5 inteiros
int valores[5] = {1, 2, 3, 4, 5};
int matriz[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Acesso aos elementos
numeros[0] = 10;
int x = valores[2];

// Percorrendo arrays
for(int i = 0; i < 5; i++) {
    printf("valores[%d] = %d\n", i, valores[i]);
}

// Array 2D
for(int i = 0; i < 3; i++) {
    for(int j = 0; j < 3; j++) {
        printf("%d ", matriz[i][j]);
    }
    printf("\n");
}
```

### Strings (Arrays de Char)

```c
#include <string.h>

// Declaração
char nome[50] = "João Silva";
char sobrenome[] = "Silva";
char letras[5] = {'a', 'b', 'c', 'd', '\0'};  // '\0' é o terminador

// Funções de string
strcpy(destino, origem);      // copiar string
strcat(destino, origem);      // concatenar
strlen(string);                // tamanho da string
strcmp(str1, str2);            // comparar strings (0 se iguais)

// Exemplo prático
char nome[50] = "João";
char sobrenome[50] = "Silva";
char nomeCompleto[100];

strcpy(nomeCompleto, nome);
strcat(nomeCompleto, " ");
strcat(nomeCompleto, sobrenome);

printf("Nome completo: %s\n", nomeCompleto);
printf("Tamanho: %lu\n", strlen(nomeCompleto));
```

---

## Ponteiros

### Conceitos Básicos

```c
int x = 10;
int *ptr;  // declaração de ponteiro para inteiro

ptr = &x;  // ptr recebe o endereço de x

printf("Valor de x: %d\n", x);
printf("Endereço de x: %p\n", &x);
printf("Valor de ptr: %p\n", ptr);
printf("Valor apontado por ptr: %d\n", *ptr);  // dereferenciamento
```

### Aritmética de Ponteiros

```c
int arr[5] = {10, 20, 30, 40, 50};
int *ptr = arr;  // ptr aponta para arr[0]

printf("*ptr = %d\n", *ptr);     // 10
ptr++;  // avança para próximo inteiro
printf("*ptr = %d\n", *ptr);     // 20

// Percorrendo array com ponteiros
for(int i = 0; i < 5; i++) {
    printf("arr[%d] = %d\n", i, *(arr + i));
}
```

### Ponteiros e Funções

```c
// Função que recebe ponteiro
void trocar(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5, y = 10;
    
    printf("Antes: x=%d, y=%d\n", x, y);
    trocar(&x, &y);
    printf("Depois: x=%d, y=%d\n", x, y);
    
    return 0;
}
```

### Ponteiros para Ponteiros

```c
int x = 10;
int *ptr = &x;
int **ptr2 = &ptr;

printf("x = %d\n", x);
printf("*ptr = %d\n", *ptr);
printf("**ptr2 = %d\n", **ptr2);
```

### Ponteiros para Funções

```c
int soma(int a, int b) { return a + b; }
int subtrai(int a, int b) { return a - b; }

int main() {
    int (*operacao)(int, int);  // ponteiro para função
    
    operacao = soma;
    printf("Soma: %d\n", operacao(5, 3));
    
    operacao = subtrai;
    printf("Subtração: %d\n", operacao(5, 3));
    
    return 0;
}
```

---

## Alocação Dinâmica

### Funções de Alocação

```c
#include <stdlib.h>

// malloc - aloca memória não inicializada
int *ptr = (int*)malloc(5 * sizeof(int));
if(ptr == NULL) {
    printf("Erro de alocação!\n");
    return 1;
}

// calloc - aloca e inicializa com zero
int *ptr2 = (int*)calloc(5, sizeof(int));

// realloc - realoca memória
ptr = (int*)realloc(ptr, 10 * sizeof(int));

// free - libera memória
free(ptr);
free(ptr2);
```

### Exemplo Prático

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    int *numeros;
    
    printf("Quantos números? ");
    scanf("%d", &n);
    
    // Alocação dinâmica
    numeros = (int*)malloc(n * sizeof(int));
    
    if(numeros == NULL) {
        printf("Erro de alocação!\n");
        return 1;
    }
    
    // Leitura
    for(int i = 0; i < n; i++) {
        printf("Número %d: ", i + 1);
        scanf("%d", &numeros[i]);
    }
    
    // Soma
    int soma = 0;
    for(int i = 0; i < n; i++) {
        soma += numeros[i];
    }
    
    printf("Soma: %d\n", soma);
    printf("Média: %.2f\n", (float)soma / n);
    
    // Liberação
    free(numeros);
    
    return 0;
}
```

---

## Estruturas e Uniões

### Struct (Estruturas)

```c
// Definição
struct Pessoa {
    char nome[50];
    int idade;
    float altura;
};

int main() {
    // Declaração
    struct Pessoa p1;
    struct Pessoa p2 = {"Maria", 25, 1.65};
    
    // Acesso
    strcpy(p1.nome, "João");
    p1.idade = 30;
    p1.altura = 1.75;
    
    printf("Nome: %s\n", p1.nome);
    printf("Idade: %d\n", p1.idade);
    printf("Altura: %.2f\n", p1.altura);
    
    return 0;
}
```

### Typedef

```c
// Usando typedef para simplificar
typedef struct {
    char nome[50];
    int idade;
    float altura;
} Pessoa;

int main() {
    Pessoa p1;
    Pessoa p2 = {"Maria", 25, 1.65};
    
    // Uso mais simples
    p1.idade = 30;
    
    return 0;
}
```

### Struct com Ponteiros

```c
typedef struct {
    char *nome;
    int idade;
} Pessoa;

int main() {
    Pessoa *p = (Pessoa*)malloc(sizeof(Pessoa));
    
    p->nome = (char*)malloc(50 * sizeof(char));
    strcpy(p->nome, "João");
    p->idade = 30;
    
    printf("Nome: %s\n", p->nome);
    printf("Idade: %d\n", p->idade);
    
    free(p->nome);
    free(p);
    
    return 0;
}
```

### Uniões

```c
union Dado {
    int i;
    float f;
    char str[20];
};

int main() {
    union Dado d;
    
    d.i = 10;
    printf("d.i = %d\n", d.i);
    
    d.f = 3.14;  // sobrescreve d.i
    printf("d.f = %.2f\n", d.f);
    printf("d.i agora = %d\n", d.i);  // valor corrompido
    
    return 0;
}
```

---

## Entrada e Saída

### printf - Formatação

```c
#include <stdio.h>

int main() {
    int i = 42;
    float f = 3.14159;
    char c = 'A';
    char s[] = "Olá";
    
    printf("Inteiro: %d\n", i);
    printf("Hexadecimal: %x\n", i);
    printf("Octal: %o\n", i);
    printf("Float: %f\n", f);
    printf("Float com 2 casas: %.2f\n", f);
    printf("Caractere: %c\n", c);
    printf("String: %s\n", s);
    printf("Ponteiro: %p\n", &i);
    
    return 0;
}
```

### scanf - Entrada Formatada

```c
#include <stdio.h>

int main() {
    int idade;
    float altura;
    char nome[50];
    
    printf("Digite seu nome: ");
    scanf("%s", nome);
    
    printf("Digite sua idade: ");
    scanf("%d", &idade);
    
    printf("Digite sua altura: ");
    scanf("%f", &altura);
    
    printf("Nome: %s, Idade: %d, Altura: %.2f\n", 
           nome, idade, altura);
    
    return 0;
}
```

### Arquivos

```c
#include <stdio.h>

int main() {
    FILE *arquivo;
    char buffer[100];
    
    // Escrita
    arquivo = fopen("teste.txt", "w");
    if(arquivo == NULL) {
        printf("Erro ao abrir arquivo!\n");
        return 1;
    }
    
    fprintf(arquivo, "Olá, arquivo!\n");
    fprintf(arquivo, "Número: %d\n", 42);
    
    fclose(arquivo);
    
    // Leitura
    arquivo = fopen("teste.txt", "r");
    if(arquivo == NULL) {
        printf("Erro ao abrir arquivo!\n");
        return 1;
    }
    
    while(fgets(buffer, sizeof(buffer), arquivo) != NULL) {
        printf("%s", buffer);
    }
    
    fclose(arquivo);
    
    return 0;
}
```

### Modos de Abertura de Arquivo

| Modo | Descrição |
|------|-----------|
| "r" | Leitura |
| "w" | Escrita (cria/sobrescreve) |
| "a" | Append (adiciona ao final) |
| "r+" | Leitura e escrita |
| "w+" | Leitura e escrita (cria/sobrescreve) |
| "a+" | Leitura e append |
| "rb" | Leitura binária |
| "wb" | Escrita binária |

---

## Pré-processador

### Diretivas Básicas

```c
// Inclusão de arquivos
#include <stdio.h>    // arquivos do sistema
#include "meu.h"      // arquivos locais

// Constantes
#define PI 3.14159
#define MAX 100

// Macros
#define SOMA(a, b) ((a) + (b))
#define QUADRADO(x) ((x) * (x))

int main() {
    printf("PI = %f\n", PI);
    printf("SOMA = %d\n", SOMA(5, 3));
    printf("QUADRADO = %d\n", QUADRADO(5));
    
    return 0;
}
```

### Compilação Condicional

```c
#define DEBUG 1

int main() {
    int x = 10;
    
    #if DEBUG
    printf("DEBUG: x = %d\n", x);
    #endif
    
    #ifdef __linux__
    printf("Compilado no Linux\n");
    #elif _WIN32
    printf("Compilado no Windows\n");
    #elif __APPLE__
    printf("Compilado no macOS\n");
    #endif
    
    return 0;
}
```

### Guards de Inclusão

```c
// arquivo: pessoa.h
#ifndef PESSOA_H
#define PESSOA_H

typedef struct {
    char nome[50];
    int idade;
} Pessoa;

void imprimirPessoa(Pessoa p);

#endif
```

---

## Tópicos Avançados

### Bitwise Operations

```c
#include <stdio.h>

int main() {
    unsigned char a = 0b0011;  // 3
    unsigned char b = 0b0101;  // 5
    
    printf("a & b  = %d\n", a & b);   // AND: 0001 = 1
    printf("a | b  = %d\n", a | b);   // OR:  0111 = 7
    printf("a ^ b  = %d\n", a ^ b);   // XOR: 0110 = 6
    printf("~a     = %d\n", ~a);       // NOT: 1100 = 12
    printf("a << 1 = %d\n", a << 1);   // left shift: 0110 = 6
    printf("a >> 1 = %d\n", a >> 1);   // right shift: 0001 = 1
    
    return 0;
}
```

### Listas Encadeadas

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct No {
    int dado;
    struct No *proximo;
} No;

// Inserir no início
No* inserirInicio(No *cabeca, int valor) {
    No *novo = (No*)malloc(sizeof(No));
    novo->dado = valor;
    novo->proximo = cabeca;
    return novo;
}

// Inserir no fim
No* inserirFim(No *cabeca, int valor) {
    No *novo = (No*)malloc(sizeof(No));
    novo->dado = valor;
    novo->proximo = NULL;
    
    if(cabeca == NULL) {
        return novo;
    }
    
    No *atual = cabeca;
    while(atual->proximo != NULL) {
        atual = atual->proximo;
    }
    atual->proximo = novo;
    
    return cabeca;
}

// Remover elemento
No* remover(No *cabeca, int valor) {
    No *anterior = NULL;
    No *atual = cabeca;
    
    while(atual != NULL && atual->dado != valor) {
        anterior = atual;
        atual = atual->proximo;
    }
    
    if(atual == NULL) return cabeca;
    
    if(anterior == NULL) {
        cabeca = atual->proximo;
    } else {
        anterior->proximo = atual->proximo;
    }
    
    free(atual);
    return cabeca;
}

// Imprimir lista
void imprimir(No *cabeca) {
    No *atual = cabeca;
    while(atual != NULL) {
        printf("%d -> ", atual->dado);
        atual = atual->proximo;
    }
    printf("NULL\n");
}

// Liberar memória
void liberar(No *cabeca) {
    No *atual = cabeca;
    while(atual != NULL) {
        No *temp = atual;
        atual = atual->proximo;
        free(temp);
    }
}
```

### Ponteiros para Funções Avançados

```c
#include <stdio.h>

// Array de ponteiros para funções
int somar(int a, int b) { return a + b; }
int subtrair(int a, int b) { return a - b; }
int multiplicar(int a, int b) { return a * b; }
int dividir(int a, int b) { return b != 0 ? a / b : 0; }

int main() {
    int (*operacoes[4])(int, int) = {somar, subtrair, multiplicar, dividir};
    char *nomes[] = {"Soma", "Subtração", "Multiplicação", "Divisão"};
    
    int a = 10, b = 5;
    
    for(int i = 0; i < 4; i++) {
        printf("%s: %d\n", nomes[i], operacoes[i](a, b));
    }
    
    return 0;
}
```

### Variáveis com Argumentos Variáveis

```c
#include <stdio.h>
#include <stdarg.h>

double media(int num, ...) {
    va_list args;
    double soma = 0;
    
    va_start(args, num);
    
    for(int i = 0; i < num; i++) {
        soma += va_arg(args, double);
    }
    
    va_end(args);
    
    return soma / num;
}

int main() {
    printf("Média de 2 números: %.2f\n", media(2, 10.0, 20.0));
    printf("Média de 4 números: %.2f\n", media(4, 1.0, 2.0, 3.0, 4.0));
    
    return 0;
}
```

---

## Boas Práticas

### Convenções de Nomenclatura

```c
// Constantes em maiúsculas
#define MAX_SIZE 100
#define PI 3.14159

// Variáveis em minúsculas com underscore
int idade_aluno;
float salario_mensal;

// Funções com verbos
void calcular_media();
int obter_idade();

// Structs com primeira letra maiúscula
typedef struct {
    char nome[50];
    int idade;
} Pessoa;
```

### Comentários

```c
/**
 * Calcula o fatorial de um número
 * @param n Número para calcular fatorial
 * @return O fatorial de n
 */
int fatorial(int n) {
    if(n <= 1) return 1;
    return n * fatorial(n - 1);
}

// Comentário de linha única
int x = 10;  // inicialização da variável
```

### Tratamento de Erros

```c
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>

FILE* abrir_arquivo_seguro(const char *nome) {
    FILE *arquivo = fopen(nome, "r");
    
    if(arquivo == NULL) {
        fprintf(stderr, "Erro ao abrir %s: %s\n", 
                nome, strerror(errno));
        return NULL;
    }
    
    return arquivo;
}

int dividir_seguro(int a, int b, int *resultado) {
    if(b == 0) {
        return -1;  // erro: divisão por zero
    }
    
    *resultado = a / b;
    return 0;  // sucesso
}
```

### Organização de Projetos

```
projeto/
├── src/
│   ├── main.c
│   ├── calculadora.c
│   └── utils.c
├── include/
│   ├── calculadora.h
│   └── utils.h
├── tests/
│   ├── test_calculadora.c
│   └── test_utils.c
├── Makefile
└── README.md
```

### Makefile Exemplo

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c99
LDFLAGS = -lm

# Diretórios
SRCDIR = src
INCDIR = include
BINDIR = bin

# Arquivos
SOURCES = $(wildcard $(SRCDIR)/*.c)
OBJECTS = $(SOURCES:$(SRCDIR)/%.c=$(BINDIR)/%.o)
TARGET = $(BINDIR)/programa

all: $(TARGET)

$(TARGET): $(OBJECTS)
	$(CC) $^ -o $@ $(LDFLAGS)

$(BINDIR)/%.o: $(SRCDIR)/%.c | $(BINDIR)
	$(CC) $(CFLAGS) -I$(INCDIR) -c $< -o $@

$(BINDIR):
	mkdir -p $(BINDIR)

clean:
	rm -rf $(BINDIR)

.PHONY: all clean
```

---

## Projetos Práticos

### Projeto 1: Calculadora Simples

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char operador;
    double num1, num2;
    
    printf("Calculadora Simples\n");
    printf("Operações: +, -, *, /\n");
    printf("Digite a operação (ex: 5 + 3): ");
    scanf("%lf %c %lf", &num1, &operador, &num2);
    
    switch(operador) {
        case '+':
            printf("%.2f + %.2f = %.2f\n", num1, num2, num1 + num2);
            break;
        case '-':
            printf("%.2f - %.2f = %.2f\n", num1, num2, num1 - num2);
            break;
        case '*':
            printf("%.2f * %.2f = %.2f\n", num1, num2, num1 * num2);
            break;
        case '/':
            if(num2 != 0) {
                printf("%.2f / %.2f = %.2f\n", num1, num2, num1 / num2);
            } else {
                printf("Erro: Divisão por zero!\n");
            }
            break;
        default:
            printf("Operador inválido!\n");
    }
    
    return 0;
}
```

### Projeto 2: Gerenciador de Tarefas

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_TAREFAS 100
#define MAX_DESCRICAO 200

typedef struct {
    char descricao[MAX_DESCRICAO];
    int concluida;
} Tarefa;

Tarefa tarefas[MAX_TAREFAS];
int num_tarefas = 0;

void adicionar_tarefa() {
    if(num_tarefas >= MAX_TAREFAS) {
        printf("Limite de tarefas atingido!\n");
        return;
    }
    
    printf("Digite a descrição da tarefa: ");
    getchar();  // limpa buffer
    fgets(tarefas[num_tarefas].descricao, MAX_DESCRICAO, stdin);
    
    // Remove o \n do final
    size_t len = strlen(tarefas[num_tarefas].descricao);
    if(len > 0 && tarefas[num_tarefas].descricao[len-1] == '\n') {
        tarefas[num_tarefas].descricao[len-1] = '\0';
    }
    
    tarefas[num_tarefas].concluida = 0;
    num_tarefas++;
    printf("Tarefa adicionada!\n");
}

void listar_tarefas() {
    if(num_tarefas == 0) {
        printf("Nenhuma tarefa cadastrada.\n");
        return;
    }
    
    printf("\n=== Lista de Tarefas ===\n");
    for(int i = 0; i < num_tarefas; i++) {
        printf("%d. [%c] %s\n", 
               i + 1, 
               tarefas[i].concluida ? 'X' : ' ',
               tarefas[i].descricao);
    }
}

void concluir_tarefa() {
    int indice;
    listar_tarefas();
    
    if(num_tarefas == 0) return;
    
    printf("Digite o número da tarefa a concluir: ");
    scanf("%d", &indice);
    
    if(indice < 1 || indice > num_tarefas) {
        printf("Índice inválido!\n");
        return;
    }
    
    tarefas[indice - 1].concluida = 1;
    printf("Tarefa concluída!\n");
}

int main() {
    int opcao;
    
    do {
        printf("\n=== Gerenciador de Tarefas ===\n");
        printf("1. Adicionar tarefa\n");
        printf("2. Listar tarefas\n");
        printf("3. Concluir tarefa\n");
        printf("4. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        
        switch(opcao) {
            case 1:
                adicionar_tarefa();
                break;
            case 2:
                listar_tarefas();
                break;
            case 3:
                concluir_tarefa();
                break;
            case 4:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida!\n");
        }
    } while(opcao != 4);
    
    return 0;
}
```

### Projeto 3: Agenda Telefônica

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char nome[50];
    char telefone[20];
    char email[50];
} Contato;

Contato *agenda = NULL;
int num_contatos = 0;

void adicionar_contato() {
    agenda = realloc(agenda, (num_contatos + 1) * sizeof(Contato));
    
    if(agenda == NULL) {
        printf("Erro de alocação!\n");
        return;
    }
    
    printf("\n--- Novo Contato ---\n");
    printf("Nome: ");
    scanf("%s", agenda[num_contatos].nome);
    printf("Telefone: ");
    scanf("%s", agenda[num_contatos].telefone);
    printf("Email: ");
    scanf("%s", agenda[num_contatos].email);
    
    num_contatos++;
    printf("Contato adicionado!\n");
}

void listar_contatos() {
    if(num_contatos == 0) {
        printf("Agenda vazia.\n");
        return;
    }
    
    printf("\n=== Agenda ===\n");
    for(int i = 0; i < num_contatos; i++) {
        printf("%d. %s\n", i + 1, agenda[i].nome);
        printf("   Tel: %s\n", agenda[i].telefone);
        printf("   Email: %s\n", agenda[i].email);
    }
}

void buscar_contato() {
    char nome_busca[50];
    printf("Digite o nome a buscar: ");
    scanf("%s", nome_busca);
    
    int encontrados = 0;
    for(int i = 0; i < num_contatos; i++) {
        if(strstr(agenda[i].nome, nome_busca) != NULL) {
            printf("\n%d. %s\n", i + 1, agenda[i].nome);
            printf("   Tel: %s\n", agenda[i].telefone);
            printf("   Email: %s\n", agenda[i].email);
            encontrados++;
        }
    }
    
    if(encontrados == 0) {
        printf("Nenhum contato encontrado.\n");
    }
}

void salvar_agenda() {
    FILE *arquivo = fopen("agenda.dat", "wb");
    if(arquivo == NULL) {
        printf("Erro ao salvar!\n");
        return;
    }
    
    fwrite(&num_contatos, sizeof(int), 1, arquivo);
    fwrite(agenda, sizeof(Contato), num_contatos, arquivo);
    fclose(arquivo);
    printf("Agenda salva!\n");
}

void carregar_agenda() {
    FILE *arquivo = fopen("agenda.dat", "rb");
    if(arquivo == NULL) {
        printf("Nenhum arquivo de agenda encontrado.\n");
        return;
    }
    
    fread(&num_contatos, sizeof(int), 1, arquivo);
    
    agenda = realloc(agenda, num_contatos * sizeof(Contato));
    if(agenda != NULL) {
        fread(agenda, sizeof(Contato), num_contatos, arquivo);
        printf("Agenda carregada com %d contatos!\n", num_contatos);
    }
    
    fclose(arquivo);
}

int main() {
    int opcao;
    
    carregar_agenda();
    
    do {
        printf("\n=== Agenda Telefônica ===\n");
        printf("1. Adicionar contato\n");
        printf("2. Listar contatos\n");
        printf("3. Buscar contato\n");
        printf("4. Salvar e sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        
        switch(opcao) {
            case 1:
                adicionar_contato();
                break;
            case 2:
                listar_contatos();
                break;
            case 3:
                buscar_contato();
                break;
            case 4:
                salvar_agenda();
                free(agenda);
                printf("Até logo!\n");
                break;
        }
    } while(opcao != 4);
    
    return 0;
}
```

---

## Recursos Adicionais

### Livros Recomendados
- "C Programming Language" - Kernighan & Ritchie
- "C Completo e Total" - Herbert Schildt
- "Expert C Programming" - Peter van der Linden

### Sites Úteis
- [cppreference.com](https://en.cppreference.com/w/c)
- [GNU C Library Manual](https://www.gnu.org/software/libc/manual/)
- [Stack Overflow C Tag](https://stackoverflow.com/questions/tagged/c)

### Ferramentas
- **GCC** - Compilador
- **GDB** - Debugger
- **Valgrind** - Detecção de vazamentos de memória
- **Make** - Automação de build
- **CLion** / **VS Code** - IDEs

---

## Conclusão

Parabéns! Você completou o manual de C do iniciante à maestria! 🎉

### Próximos Passos
1. Pratique diariamente com pequenos projetos
2. Contribua com projetos open source em C
3. Estude sistemas operacionais (Linux kernel é escrito em C)
4. Aprenda C++ para programação orientada a objetos
5. Explore áreas específicas como embedded systems ou game development

**Lembre-se:** A maestria vem com a prática constante. Continue programando e aprendendo!

---

*Última atualização: 2024*
