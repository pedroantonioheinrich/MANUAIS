# 📗 MANUAL INTERMEDIÁRIO DE LINGUAGEM C
*Funções, Arrays, Strings, Ponteiros Básicos e Estruturas*

---

## 📋 SUMÁRIO
1. [Funções](#1-funções)
2. [Escopo de Variáveis](#2-escopo-de-variáveis)
3. [Arrays e Vetores](#3-arrays-e-vetores)
4. [Strings em C](#4-strings-em-c)
5. [Ponteiros Básicos](#5-ponteiros-básicos)
6. [Estruturas (struct)](#6-estruturas-struct)
7. [Uniões e Enumerações](#7-uniões-e-enumerações)

---

## 1. FUNÇÕES

### Conceito e Sintaxe
Funções permitem dividir programas em partes menores e reutilizáveis.

```c
// Declaração (protótipo)
tipo_retorno nome_funcao(tipo_parametro parametro1, ...);

// Definição
tipo_retorno nome_funcao(tipo_parametro parametro1, ...) {
    // Corpo da função
    return valor;  // se tipo_retorno não for void
}
```

### Exemplos Práticos
```c
#include <stdio.h>

// 1. Função simples sem parâmetros
void saudacao() {
    printf("Olá! Bem-vindo ao programa.\n");
}

// 2. Função com parâmetros e retorno
int soma(int a, int b) {
    return a + b;
}

// 3. Função com múltiplos parâmetros
float calcular_media(float n1, float n2, float n3) {
    return (n1 + n2 + n3) / 3.0;
}

// 4. Função que modifica parâmetro por referência
void incrementar(int *x) {
    (*x)++;
}

// 5. Função com parâmetro de array
void imprimir_array(int arr[], int tamanho) {
    for(int i = 0; i < tamanho; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

int main() {
    saudacao();
    
    int resultado = soma(5, 3);
    printf("Soma: %d\n", resultado);
    
    float media = calcular_media(7.5, 8.0, 9.2);
    printf("Média: %.2f\n", media);
    
    int valor = 10;
    printf("Valor antes: %d\n", valor);
    incrementar(&valor);
    printf("Valor depois: %d\n", valor);
    
    int numeros[] = {1, 2, 3, 4, 5};
    imprimir_array(numeros, 5);
    
    return 0;
}
```

### Funções Recursivas
```c
#include <stdio.h>

// Fatorial recursivo
int fatorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * fatorial(n - 1);
}

// Fibonacci recursivo
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Soma recursiva de array
int soma_array(int arr[], int tamanho) {
    if (tamanho <= 0) {
        return 0;
    }
    return arr[tamanho - 1] + soma_array(arr, tamanho - 1);
}

int main() {
    printf("Fatorial de 5: %d\n", fatorial(5));
    
    printf("Fibonacci(7): ");
    for(int i = 0; i < 7; i++) {
        printf("%d ", fibonacci(i));
    }
    printf("\n");
    
    int vetor[] = {1, 2, 3, 4, 5};
    printf("Soma do vetor: %d\n", soma_array(vetor, 5));
    
    return 0;
}
```

### Funções com Parâmetros Opcionais (simulados)
```c
#include <stdio.h>
#include <stdarg.h>

// Função com número variável de argumentos
int soma_variavel(int count, ...) {
    va_list args;
    va_start(args, count);
    
    int total = 0;
    for(int i = 0; i < count; i++) {
        total += va_arg(args, int);
    }
    
    va_end(args);
    return total;
}

// Função para média com múltiplos números
float media_variavel(int count, ...) {
    if (count <= 0) return 0.0;
    
    va_list args;
    va_start(args, count);
    
    float total = 0;
    for(int i = 0; i < count; i++) {
        total += va_arg(args, double);  // Promoção para double
    }
    
    va_end(args);
    return total / count;
}

int main() {
    printf("Soma de 3 números: %d\n", soma_variavel(3, 10, 20, 30));
    printf("Soma de 5 números: %d\n", soma_variavel(5, 1, 2, 3, 4, 5));
    
    printf("Média de 4 números: %.2f\n", media_variavel(4, 7.5, 8.0, 6.5, 9.0));
    
    return 0;
}
```

---

## 2. ESCOPO DE VARIÁVEIS

### Tipos de Escopo
```c
#include <stdio.h>

// Variável global - escopo de arquivo
int global_var = 100;

// Variável estática global - só visível neste arquivo
static int static_global = 200;

void funcao_exemplo() {
    // Variável local automática
    int local_var = 10;
    
    // Variável estática local - mantém valor entre chamadas
    static int static_local = 0;
    static_local++;
    
    printf("Local: %d, Static Local: %d\n", local_var, static_local);
}

void demonstrar_escopo() {
    int x = 5;  // Variável local na função
    
    {
        // Bloco interno - novo escopo
        int x = 10;  // Esta x "esconde" a x externa
        printf("x no bloco interno: %d\n", x);
    }
    
    printf("x no bloco externo: %d\n", x);
}

// Parâmetros formais têm escopo local à função
int soma(int a, int b) {  // a e b são locais a esta função
    return a + b;
}

int main() {
    // Acesso à variável global
    printf("Variável global: %d\n", global_var);
    
    // Chamadas múltiplas para mostrar static local
    funcao_exemplo();  // Static Local: 1
    funcao_exemplo();  // Static Local: 2
    funcao_exemplo();  // Static Local: 3
    
    demonstrar_escopo();
    
    // Variáveis com mesmo nome em funções diferentes
    int x = 50;  // Esta x é independente da x em demonstrar_escopo()
    printf("x em main: %d\n", x);
    
    return 0;
}
```

### Qualificadores de Tipo
```c
#include <stdio.h>

void exemplo_const() {
    // Variável constante - valor não pode ser modificado
    const int MAX = 100;
    // MAX = 200;  // ERRO: assignment of read-only variable
    
    // Ponteiro para constante
    const int *ptr1;
    int valor = 5;
    ptr1 = &valor;
    // *ptr1 = 10;  // ERRO: não pode modificar através de ptr1
    valor = 10;     // OK: pode modificar diretamente
    
    // Ponteiro constante
    int *const ptr2 = &valor;
    *ptr2 = 20;     // OK: pode modificar o valor apontado
    // ptr2 = &MAX; // ERRO: ptr2 é constante
    
    // Ponteiro constante para constante
    const int *const ptr3 = &valor;
    // *ptr3 = 30;  // ERRO
    // ptr3 = &MAX; // ERRO
}

void exemplo_volatile() {
    // volatile: impede otimizações do compilador
    volatile int sensor_port;
    // Útil para hardware, memória compartilhada, sinais
    
    // Sem volatile, o compilador poderia otimizar o loop
    volatile int flag = 0;
    
    while(!flag) {
        // Aguarda mudança externa
    }
}

int main() {
    exemplo_const();
    exemplo_volatile();
    return 0;
}
```

---

## 3. ARRAYS E VETORES

### Arrays Unidimensionais
```c
#include <stdio.h>

void exemplo_array_basico() {
    // Declaração e inicialização
    int numeros[5];  // Array de 5 inteiros não inicializados
    
    // Inicialização na declaração
    int idades[] = {25, 30, 35, 40, 45};  // Tamanho inferido
    
    // Acesso aos elementos
    printf("Primeira idade: %d\n", idades[0]);  // Índice 0
    printf("Última idade: %d\n", idades[4]);    // Índice 4
    
    // Modificação
    idades[0] = 26;
    printf("Nova primeira idade: %d\n", idades[0]);
    
    // Percorrendo o array
    printf("Todas as idades: ");
    for(int i = 0; i < 5; i++) {
        printf("%d ", idades[i]);
    }
    printf("\n");
}

void array_com_funcoes() {
    int vetor[10];
    
    // Preencher array
    for(int i = 0; i < 10; i++) {
        vetor[i] = (i + 1) * 10;  // 10, 20, 30, ..., 100
    }
    
    // Calcular soma
    int soma = 0;
    for(int i = 0; i < 10; i++) {
        soma += vetor[i];
    }
    printf("Soma: %d\n", soma);
    
    // Encontrar maior elemento
    int maior = vetor[0];
    for(int i = 1; i < 10; i++) {
        if(vetor[i] > maior) {
            maior = vetor[i];
        }
    }
    printf("Maior elemento: %d\n", maior);
    
    // Ordenação simples (Bubble Sort)
    int desordenado[] = {64, 34, 25, 12, 22, 11, 90};
    int n = 7;
    
    printf("Array antes da ordenação: ");
    for(int i = 0; i < n; i++) printf("%d ", desordenado[i]);
    printf("\n");
    
    // Algoritmo Bubble Sort
    for(int i = 0; i < n-1; i++) {
        for(int j = 0; j < n-i-1; j++) {
            if(desordenado[j] > desordenado[j+1]) {
                // Troca
                int temp = desordenado[j];
                desordenado[j] = desordenado[j+1];
                desordenado[j+1] = temp;
            }
        }
    }
    
    printf("Array após ordenação: ");
    for(int i = 0; i < n; i++) printf("%d ", desordenado[i]);
    printf("\n");
}

void exemplo_array_dinamico() {
    int tamanho;
    
    printf("Digite o tamanho do array: ");
    scanf("%d", &tamanho);
    
    // Array de tamanho variável (C99+)
    int vetor[tamanho];
    
    // Preencher com valores
    for(int i = 0; i < tamanho; i++) {
        vetor[i] = (i + 1) * 2;  // Números pares
    }
    
    // Imprimir
    printf("Array criado: ");
    for(int i = 0; i < tamanho; i++) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
}

int main() {
    exemplo_array_basico();
    printf("\n");
    array_com_funcoes();
    printf("\n");
    exemplo_array_dinamico();
    
    return 0;
}
```

### Arrays Multidimensionais (Matrizes)
```c
#include <stdio.h>

void exemplo_matriz_2d() {
    // Declaração e inicialização de matriz 3x3
    int matriz[3][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };
    
    printf("Matriz 3x3:\n");
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            printf("%d ", matriz[i][j]);
        }
        printf("\n");
    }
    
    // Soma dos elementos da matriz
    int soma = 0;
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            soma += matriz[i][j];
        }
    }
    printf("Soma total: %d\n", soma);
    
    // Matriz como tabela de multiplicação
    int tabuada[10][10];
    for(int i = 0; i < 10; i++) {
        for(int j = 0; j < 10; j++) {
            tabuada[i][j] = (i + 1) * (j + 1);
        }
    }
    
    printf("\nTabuada do 5:\n");
    for(int j = 0; j < 10; j++) {
        printf("5 x %d = %d\n", j + 1, tabuada[4][j]);
    }
}

void multiplicacao_matrizes() {
    int A[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };
    
    int B[3][2] = {
        {7, 8},
        {9, 10},
        {11, 12}
    };
    
    int C[2][2] = {0};  // Inicializa com zeros
    
    // Multiplicação A(2x3) * B(3x2) = C(2x2)
    for(int i = 0; i < 2; i++) {
        for(int j = 0; j < 2; j++) {
            for(int k = 0; k < 3; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    
    printf("Resultado da multiplicação:\n");
    for(int i = 0; i < 2; i++) {
        for(int j = 0; j < 2; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }
}

int main() {
    exemplo_matriz_2d();
    printf("\n");
    multiplicacao_matrizes();
    
    return 0;
}
```

---

## 4. STRINGS EM C

### Fundamentos de Strings
```c
#include <stdio.h>
#include <string.h>  // Biblioteca para manipulação de strings

void strings_basico() {
    // Strings são arrays de caracteres terminados com '\0'
    
    // Formas de declarar strings:
    char nome1[] = "João";  // Tamanho automático (5: J o ã o \0)
    char nome2[50] = "Maria";
    char nome3[] = {'A', 'n', 'a', '\0'};
    
    printf("Nome 1: %s\n", nome1);
    printf("Nome 2: %s\n", nome2);
    printf("Nome 3: %s\n", nome3);
    
    // Entrada de strings
    char cidade[50];
    printf("Digite sua cidade: ");
    fgets(cidade, 50, stdin);  // Seguro contra buffer overflow
    
    // Remover o '\n' final se existir
    cidade[strcspn(cidade, "\n")] = '\0';
    
    printf("Cidade: %s\n", cidade);
}

void funcoes_strings() {
    char str1[50] = "Hello";
    char str2[50] = "World";
    char str3[50];
    
    // strlen() - comprimento da string
    printf("Tamanho de '%s': %lu\n", str1, strlen(str1));
    
    // strcpy() - copiar strings
    strcpy(str3, str1);
    printf("str3 após strcpy: %s\n", str3);
    
    // strcat() - concatenar strings
    strcat(str1, " ");
    strcat(str1, str2);
    printf("str1 após strcat: %s\n", str1);
    
    // strcmp() - comparar strings
    char senha[] = "secreta";
    char tentativa[50];
    
    printf("Digite a senha: ");
    scanf("%s", tentativa);
    
    if(strcmp(tentativa, senha) == 0) {
        printf("Senha correta!\n");
    } else {
        printf("Senha incorreta!\n");
    }
    
    // strchr() - encontrar caractere
    char texto[] = "procure a letra a";
    char *resultado = strchr(texto, 'a');
    
    if(resultado != NULL) {
        printf("Encontrado 'a' na posição: %ld\n", resultado - texto);
    }
    
    // strstr() - encontrar substring
    char frase[] = "O rato roeu a roupa do rei";
    char *sub = strstr(frase, "roupa");
    
    if(sub != NULL) {
        printf("Substring encontrada: %s\n", sub);
    }
}

void manipulacao_strings() {
    char texto[100] = "   Linguagem C é poderosa!   ";
    
    printf("Texto original: '%s'\n", texto);
    printf("Tamanho: %lu\n", strlen(texto));
    
    // Convertendo para maiúsculas
    for(int i = 0; texto[i] != '\0'; i++) {
        if(texto[i] >= 'a' && texto[i] <= 'z') {
            texto[i] = texto[i] - 32;  // ASCII: a=97, A=65
        }
    }
    printf("Maiúsculas: '%s'\n", texto);
    
    // Convertendo para minúsculas
    for(int i = 0; texto[i] != '\0'; i++) {
        if(texto[i] >= 'A' && texto[i] <= 'Z') {
            texto[i] = texto[i] + 32;
        }
    }
    printf("Minúsculas: '%s'\n", texto);
    
    // Invertendo a string
    char original[] = "ABCDE";
    char invertida[6];
    int len = strlen(original);
    
    for(int i = 0; i < len; i++) {
        invertida[i] = original[len - 1 - i];
    }
    invertida[len] = '\0';
    
    printf("Original: %s\n", original);
    printf("Invertida: %s\n", invertida);
}

void exemplo_sprintf() {
    char buffer[100];
    int dia = 15, mes = 3, ano = 2024;
    float temperatura = 23.5;
    
    // sprintf - formata string para buffer
    sprintf(buffer, "Data: %02d/%02d/%d - Temperatura: %.1f°C", 
            dia, mes, ano, temperatura);
    
    printf("%s\n", buffer);
    
    // sscanf - lê dados formatados de uma string
    char dados[] = "João Silva 25 1.75";
    char nome[50], sobrenome[50];
    int idade;
    float altura;
    
    sscanf(dados, "%s %s %d %f", nome, sobrenome, &idade, &altura);
    
    printf("Nome: %s %s\n", nome, sobrenome);
    printf("Idade: %d\n", idade);
    printf("Altura: %.2f\n", altura);
}

int main() {
    strings_basico();
    printf("\n");
    funcoes_strings();
    printf("\n");
    manipulacao_strings();
    printf("\n");
    exemplo_sprintf();
    
    return 0;
}
```

---

## 5. PONTEIROS BÁSICOS

### Conceitos Fundamentais
```c
#include <stdio.h>

void ponteiros_introducao() {
    int x = 10;
    int *ptr;  // Declaração de ponteiro para int
    
    ptr = &x;  // ptr agora aponta para x
    
    printf("Valor de x: %d\n", x);
    printf("Endereço de x: %p\n", &x);
    printf("Valor de ptr: %p\n", ptr);
    printf("Valor apontado por ptr: %d\n", *ptr);
    
    // Modificando valor através do ponteiro
    *ptr = 20;
    printf("Novo valor de x: %d\n", x);
}

void ponteiros_e_arrays() {
    int vetor[] = {10, 20, 30, 40, 50};
    int *ptr = vetor;  // Equivalente a &vetor[0]
    
    printf("Array usando ponteiro:\n");
    for(int i = 0; i < 5; i++) {
        printf("vetor[%d] = %d (endereço: %p)\n", 
               i, *(ptr + i), ptr + i);
    }
    
    // Aritmética de ponteiros
    printf("\nAritmética de ponteiros:\n");
    printf("*ptr = %d\n", *ptr);
    ptr++;  // Avança para próximo elemento
    printf("Após ptr++: *ptr = %d\n", *ptr);
    ptr += 2;  // Avança 2 elementos
    printf("Após ptr += 2: *ptr = %d\n", *ptr);
    ptr--;  // Retrocede 1 elemento
    printf("Após ptr--: *ptr = %d\n", *ptr);
    
    // Comparação de ponteiros
    int *inicio = &vetor[0];
    int *fim = &vetor[4];
    
    printf("\nNúmero de elementos entre início e fim: %ld\n", 
           fim - inicio + 1);
}

void ponteiros_para_ponteiros() {
    int x = 100;
    int *ptr1 = &x;
    int **ptr2 = &ptr1;  // Ponteiro para ponteiro
    
    printf("x = %d\n", x);
    printf("&x = %p\n", &x);
    printf("ptr1 = %p\n", ptr1);
    printf("*ptr1 = %d\n", *ptr1);
    printf("ptr2 = %p\n", ptr2);
    printf("*ptr2 = %p\n", *ptr2);
    printf("**ptr2 = %d\n", **ptr2);
    
    // Modificando através de ponteiro duplo
    **ptr2 = 200;
    printf("\nNovo valor de x: %d\n", x);
}

void ponteiros_e_funcoes() {
    // Passagem por valor vs. passagem por referência
    
    void incrementar_por_valor(int a) {
        a++;
        printf("Dentro da função (por valor): %d\n", a);
    }
    
    void incrementar_por_referencia(int *a) {
        (*a)++;
        printf("Dentro da função (por referência): %d\n", *a);
    }
    
    int numero = 10;
    
    printf("Antes da função (por valor): %d\n", numero);
    incrementar_por_valor(numero);
    printf("Depois da função (por valor): %d\n\n", numero);
    
    printf("Antes da função (por referência): %d\n", numero);
    incrementar_por_referencia(&numero);
    printf("Depois da função (por referência): %d\n", numero);
}

void trocar_valores(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void exemplo_troca() {
    int x = 5, y = 10;
    
    printf("Antes da troca: x = %d, y = %d\n", x, y);
    trocar_valores(&x, &y);
    printf("Depois da troca: x = %d, y = %d\n", x, y);
}

void ponteiros_e_strings() {
    char *mensagem = "Olá Mundo";  // String literal (ponteiro para char constante)
    char texto[] = "Programação C";  // Array de caracteres
    
    printf("Mensagem: %s\n", mensagem);
    printf("Texto: %s\n", texto);
    
    // Percorrendo string com ponteiro
    printf("Caracteres da mensagem: ");
    char *ptr = mensagem;
    while(*ptr != '\0') {
        printf("%c ", *ptr);
        ptr++;
    }
    printf("\n");
}

int main() {
    printf("=== INTRODUÇÃO A PONTEIROS ===\n");
    ponteiros_introducao();
    
    printf("\n=== PONTEIROS E ARRAYS ===\n");
    ponteiros_e_arrays();
    
    printf("\n=== PONTEIROS PARA PONTEIROS ===\n");
    ponteiros_para_ponteiros();
    
    printf("\n=== PONTEIROS E FUNÇÕES ===\n");
    ponteiros_e_funcoes();
    
    printf("\n=== EXEMPLO DE TROCA ===\n");
    exemplo_troca();
    
    printf("\n=== PONTEIROS E STRINGS ===\n");
    ponteiros_e_strings();
    
    return 0;
}
```

---

## 6. ESTRUTURAS (STRUCT)

### Definição e Uso Básico
```c
#include <stdio.h>
#include <string.h>

// Definição de estrutura
struct Pessoa {
    char nome[50];
    int idade;
    float altura;
    char profissao[30];
};

void estrutura_basica() {
    // Declaração de variável struct
    struct Pessoa pessoa1;
    
    // Acesso e modificação dos membros
    strcpy(pessoa1.nome, "Carlos Silva");
    pessoa1.idade = 30;
    pessoa1.altura = 1.78;
    strcpy(pessoa1.profissao, "Engenheiro");
    
    // Impressão
    printf("=== Dados da Pessoa ===\n");
    printf("Nome: %s\n", pessoa1.nome);
    printf("Idade: %d anos\n", pessoa1.idade);
    printf("Altura: %.2f m\n", pessoa1.altura);
    printf("Profissão: %s\n", pessoa1.profissao);
    
    // Inicialização na declaração
    struct Pessoa pessoa2 = {
        "Maria Santos",
        25,
        1.65,
        "Médica"
    };
    
    printf("\n=== Outra Pessoa ===\n");
    printf("Nome: %s\n", pessoa2.nome);
}

void array_de_estruturas() {
    struct Aluno {
        char nome[50];
        float notas[3];
        float media;
    };
    
    struct Aluno turma[3];
    
    // Preencher dados
    for(int i = 0; i < 3; i++) {
        printf("\nAluno %d:\n", i + 1);
        printf("Nome: ");
        scanf("%s", turma[i].nome);
        
        printf("Digite 3 notas: ");
        scanf("%f %f %f", 
              &turma[i].notas[0],
              &turma[i].notas[1],
              &turma[i].notas[2]);
        
        // Calcular média
        turma[i].media = (turma[i].notas[0] + 
                         turma[i].notas[1] + 
                         turma[i].notas[2]) / 3.0;
    }
    
    // Exibir resultados
    printf("\n=== RELATÓRIO DA TURMA ===\n");
    for(int i = 0; i < 3; i++) {
        printf("\nAluno: %s\n", turma[i].nome);
        printf("Notas: %.1f, %.1f, %.1f\n",
               turma[i].notas[0],
               turma[i].notas[1],
               turma[i].notas[2]);
        printf("Média: %.2f - %s\n",
               turma[i].media,
               turma[i].media >= 6.0 ? "Aprovado" : "Reprovado");
    }
}

void estruturas_aninhadas() {
    struct Endereco {
        char rua[50];
        int numero;
        char cidade[30];
        char estado[3];
    };
    
    struct Funcionario {
        char nome[50];
        int id;
        float salario;
        struct Endereco endereco;  // Estrutura aninhada
    };
    
    struct Funcionario func = {
        "João Pereira",
        1001,
        3500.00,
        {"Rua das Flores", 123, "São Paulo", "SP"}
    };
    
    printf("=== Dados do Funcionário ===\n");
    printf("Nome: %s\n", func.nome);
    printf("ID: %d\n", func.id);
    printf("Salário: R$ %.2f\n", func.salario);
    printf("Endereço: %s, %d - %s/%s\n",
           func.endereco.rua,
           func.endereco.numero,
           func.endereco.cidade,
           func.endereco.estado);
}

void estruturas_e_ponteiros() {
    struct Produto {
        char nome[50];
        float preco;
        int estoque;
    };
    
    struct Produto produto = {"Notebook", 2500.00, 10};
    struct Produto *ptr = &produto;
    
    // Acesso através do ponteiro
    printf("Produto: %s\n", ptr->nome);  // Operador seta (->)
    printf("Preço: R$ %.2f\n", ptr->preco);
    printf("Estoque: %d unidades\n", ptr->estoque);
    
    // Modificação através do ponteiro
    ptr->preco = 2300.00;
    ptr->estoque -= 2;
    
    printf("\nApós modificação:\n");
    printf("Preço: R$ %.2f\n", produto.preco);
    printf("Estoque: %d unidades\n", produto.estoque);
}

typedef struct {
    int horas;
    int minutos;
    int segundos;
} Horario;

void uso_typedef() {
    // Com typedef, não precisa escrever "struct" toda vez
    Horario agora = {14, 30, 45};
    Horario intervalo = {0, 15, 0};
    
    printf("Horário atual: %02d:%02d:%02d\n",
           agora.horas, agora.minutos, agora.segundos);
    
    // Calcular horário após intervalo
    agora.segundos += intervalo.segundos;
    agora.minutos += agora.segundos / 60;
    agora.segundos %= 60;
    
    agora.minutos += intervalo.minutos;
    agora.horas += agora.minutos / 60;
    agora.minutos %= 60;
    
    agora.horas += intervalo.horas;
    agora.horas %= 24;
    
    printf("Horário após intervalo: %02d:%02d:%02d\n",
           agora.horas, agora.minutos, agora.segundos);
}

int main() {
    printf("=== ESTRUTURAS BÁSICAS ===\n");
    estrutura_basica();
    
    printf("\n=== ARRAY DE ESTRUTURAS ===\n");
    array_de_estruturas();
    
    printf("\n=== ESTRUTURAS ANINHADAS ===\n");
    estruturas_aninhadas();
    
    printf("\n=== ESTRUTURAS E PONTEIROS ===\n");
    estruturas_e_ponteiros();
    
    printf("\n=== USO DE TYPEDEF ===\n");
    uso_typedef();
    
    return 0;
}
```

---

## 7. UNIÕES E ENUMERAÇÕES

### Uniões (Unions)
```c
#include <stdio.h>

void exemplo_uniao() {
    // Union: todos os membros compartilham o mesmo espaço de memória
    union Dado {
        int i;
        float f;
        char str[20];
    };
    
    union Dado dado;
    
    printf("Tamanho da union: %lu bytes\n", sizeof(dado));
    
    // Armazenando int
    dado.i = 42;
    printf("dado.i = %d\n", dado.i);
    
    // Armazenando float (sobrescreve o int)
    dado.f = 3.14;
    printf("dado.f = %.2f\n", dado.f);
    
    // O valor anterior foi perdido
    printf("dado.i agora é: %d (lixo)\n", dado.i);
    
    // Uso prático: representar diferentes tipos de dados
    union Valor {
        int inteiro;
        float decimal;
        char caractere;
    };
    
    union Valor valor;
    
    valor.inteiro = 100;
    printf("\nValor como inteiro: %d\n", valor.inteiro);
    
    valor.decimal = 9.99;
    printf("Valor como float: %.2f\n", valor.decimal);
}

// Uniões com estruturas (discriminated unions)
void uniao_discriminada() {
    struct Inteiro {
        int valor;
    };
    
    struct Texto {
        char valor[50];
    };
    
    enum TipoDado { TIPO_INTEIRO, TIPO_TEXTO };
    
    union Dados {
        struct Inteiro i;
        struct Texto t;
    };
    
    struct Variavel {
        enum TipoDado tipo;
        union Dados dados;
    };
    
    struct Variavel var;
    
    // Armazenando inteiro
    var.tipo = TIPO_INTEIRO;
    var.dados.i.valor = 100;
    
    // Armazenando texto
    var.tipo = TIPO_TEXTO;
    strcpy(var.dados.t.valor, "Olá Mundo");
    
    // Usando a variável
    if(var.tipo == TIPO_INTEIRO) {
        printf("Valor inteiro: %d\n", var.dados.i.valor);
    } else if(var.tipo == TIPO_TEXTO) {
        printf("Valor texto: %s\n", var.dados.t.valor);
    }
}

### Enumerações (Enums)
```c
#include <stdio.h>

void exemplo_enum_basico() {
    // Enumeração: lista de constantes inteiras
    enum DiasSemana {
        DOMINGO,    // 0
        SEGUNDA,    // 1
        TERCA,      // 2
        QUARTA,     // 3
        QUINTA,     // 4
        SEXTA,      // 5
        SABADO      // 6
    };
    
    enum DiasSemana hoje = QUARTA;
    
    printf("Valor numérico de QUARTA: %d\n", hoje);
    
    switch(hoje) {
        case DOMINGO:
        case SABADO:
            printf("Fim de semana!\n");
            break;
        default:
            printf("Dia útil\n");
    }
    
    // Enumeração com valores explícitos
    enum NivelPrioridade {
        BAIXA = 1,
        MEDIA = 5,
        ALTA = 10,
        URGENTE = 99
    };
    
    enum NivelPrioridade prioridade = ALTA;
    
    printf("\nPrioridade: %d\n", prioridade);
    if(prioridade >= ALTA) {
        printf("Atender imediatamente!\n");
    }
}

void enum_com_typedef() {
    // Combinando typedef com enum
    typedef enum {
        VERMELHO,
        VERDE,
        AZUL,
        AMARELO,
        PRETO,
        BRANCO
    } Cor;
    
    Cor cor_fundo = AZUL;
    Cor cor_texto = BRANCO;
    
    printf("Cor de fundo: %d\n", cor_fundo);
    printf("Cor do texto: %d\n", cor_texto);
    
    // Enumeração com strings associadas
    const char* nomes_cores[] = {
        "Vermelho", "Verde", "Azul", 
        "Amarelo", "Preto", "Branco"
    };
    
    printf("Cor de fundo: %s\n", nomes_cores[cor_fundo]);
    printf("Cor do texto: %s\n", nomes_cores[cor_texto]);
}

// Exemplo prático: sistema de reservas
void exemplo_pratico_enum() {
    enum EstadoReserva {
        DISPONIVEL,
        RESERVADA,
        OCUPADA,
        MANUTENCAO
    };
    
    enum TipoQuarto {
        STANDARD,
        DELUXE,
        SUITE,
        PRESIDENCIAL
    };
    
    const char* estados[] = {
        "Disponível", "Reservada", "Ocupada", "Manutenção"
    };
    
    const char* tipos[] = {
        "Standard", "Deluxe", "Suite", "Presidencial"
    };
    
    const float precos[] = {150.00, 250.00, 400.00, 800.00};
    
    struct Quarto {
        int numero;
        enum TipoQuarto tipo;
        enum EstadoReserva estado;
    };
    
    struct Quarto quarto1 = {101, DELUXE, DISPONIVEL};
    struct Quarto quarto2 = {102, SUITE, RESERVADA};
    
    printf("=== SISTEMA DE RESERVAS ===\n");
    printf("Quarto %d:\n", quarto1.numero);
    printf("  Tipo: %s\n", tipos[quarto1.tipo]);
    printf("  Estado: %s\n", estados[quarto1.estado]);
    printf("  Preço: R$ %.2f\n", precos[quarto1.tipo]);
    
    printf("\nQuarto %d:\n", quarto2.numero);
    printf("  Tipo: %s\n", tipos[quarto2.tipo]);
    printf("  Estado: %s\n", estados[quarto2.estado]);
    printf("  Preço: R$ %.2f\n", precos[quarto2.tipo]);
}

int main() {
    printf("=== UNIÕES ===\n");
    exemplo_uniao();
    
    printf("\n=== UNIÕES DISCRIMINADAS ===\n");
    uniao_discriminada();
    
    printf("\n=== ENUMERAÇÕES BÁSICAS ===\n");
    exemplo_enum_basico();
    
    printf("\n=== ENUM COM TYPEDEF ===\n");
    enum_com_typedef();
    
    printf("\n=== EXEMPLO PRÁTICO ===\n");
    exemplo_pratico_enum();
    
    return 0;
}
```

---

## 🎯 EXERCÍCIOS INTERMEDIÁRIOS

### 1. Sistema de Biblioteca
```c
#include <stdio.h>
#include <string.h>

#define MAX_LIVROS 100

struct Livro {
    int id;
    char titulo[100];
    char autor[50];
    int ano;
    int disponivel;  // 1 = disponível, 0 = emprestado
};

struct Biblioteca {
    struct Livro livros[MAX_LIVROS];
    int total_livros;
};

void adicionar_livro(struct Biblioteca *bib) {
    if(bib->total_livros >= MAX_LIVROS) {
        printf("Biblioteca cheia!\n");
        return;
    }
    
    struct Livro novo;
    novo.id = bib->total_livros + 1;
    
    printf("Título: ");
    scanf(" %[^\n]", novo.titulo);
    printf("Autor: ");
    scanf(" %[^\n]", novo.autor);
    printf("Ano: ");
    scanf("%d", &novo.ano);
    novo.disponivel = 1;
    
    bib->livros[bib->total_livros] = novo;
    bib->total_livros++;
    
    printf("Livro adicionado com sucesso!\n");
}

void listar_livros(struct Biblioteca *bib) {
    printf("\n=== CATÁLOGO DE LIVROS ===\n");
    for(int i = 0; i < bib->total_livros; i++) {
        printf("ID: %d\n", bib->livros[i].id);
        printf("Título: %s\n", bib->livros[i].titulo);
        printf("Autor: %s\n", bib->livros[i].autor);
        printf("Ano: %d\n", bib->livros[i].ano);
        printf("Status: %s\n", 
               bib->livros[i].disponivel ? "Disponível" : "Emprestado");
        printf("------------------------\n");
    }
}

// Implementar funções: buscar_livro(), emprestar_livro(), devolver_livro()

int main() {
    struct Biblioteca minha_bib = {0};
    
    // Menu interativo
    int opcao;
    do {
        printf("\n=== SISTEMA DE BIBLIOTECA ===\n");
        printf("1. Adicionar livro\n");
        printf("2. Listar livros\n");
        printf("3. Buscar livro\n");
        printf("4. Emprestar livro\n");
        printf("5. Devolver livro\n");
        printf("0. Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        
        switch(opcao) {
            case 1: adicionar_livro(&minha_bib); break;
            case 2: listar_livros(&minha_bib); break;
            // case 3: buscar_livro(&minha_bib); break;
            // case 4: emprestar_livro(&minha_bib); break;
            // case 5: devolver_livro(&minha_bib); break;
        }
    } while(opcao != 0);
    
    return 0;
}
```

### 2. Calculadora Científica com Funções Ponteiro
```c
#include <stdio.h>
#include <math.h>

typedef double (*Operacao)(double, double);

double soma(double a, double b) { return a + b; }
double subtracao(double a, double b) { return a - b; }
double multiplicacao(double a, double b) { return a * b; }
double divisao(double a, double b) { return b != 0 ? a / b : 0; }
double potencia(double a, double b) { return pow(a, b); }

struct Calculadora {
    double memoria;
    Operacao operacoes[5];
};

void inicializar_calculadora(struct Calculadora *calc) {
    calc->memoria = 0;
    calc->operacoes[0] = soma;
    calc->operacoes[1] = subtracao;
    calc->operacoes[2] = multiplicacao;
    calc->operacoes[3] = divisao;
    calc->operacoes[4] = potencia;
}

int main() {
    struct Calculadora calc;
    inicializar_calculadora(&calc);
    
    double x, y, resultado;
    int operacao;
    
    printf("Calculadora Científica\n");
    printf("Operações: 0-Soma, 1-Subtração, 2-Multiplicação, 3-Divisão, 4-Potência\n");
    
    printf("Digite x: ");
    scanf("%lf", &x);
    printf("Digite y: ");
    scanf("%lf", &y);
    printf("Escolha operação (0-4): ");
    scanf("%d", &operacao);
    
    if(operacao >= 0 && operacao <= 4) {
        resultado = calc.operacoes[operacao](x, y);
        printf("Resultado: %.2f\n", resultado);
        calc.memoria = resultado;
        printf("Memória: %.2f\n", calc.memoria);
    } else {
        printf("Operação inválida!\n");
    }
    
    return 0;
}
```

---

## 📝 RESUMO DAS BOAS PRÁTICAS INTERMEDIÁRIAS

1. **Use funções para modularizar** seu código
2. **Documente parâmetros e retornos** das funções
3. **Valide sempre o tamanho** de strings e arrays
4. **Inicialize ponteiros** antes de usar
5. **Prefira arrays sobre ponteiros** quando possível
6. **Use structs para dados relacionados**
7. **Utilize enums** para constantes relacionadas
8. **Teste casos de borda** em todas as funções

---

## 🔗 PRÓXIMOS PASSOS

No **Manual Avançado** você aprenderá:
- Ponteiros avançados (ponteiros para funções)
- Manipulação de memória dinâmica (malloc, calloc, free)
- Tipos definidos pelo usuário
- Diretivas de pré-processador avançadas
- Manipulação de arquivos binários

---

**💡 DICA DE ESTUDO**: Pratique implementando estruturas de dados básicas como listas ligadas, pilhas e filas usando os conceitos aprendidos aqui!

*Documento atualizado: Fevereiro 2024*  
*Nível: Intermediário*
