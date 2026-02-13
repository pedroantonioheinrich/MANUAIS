Aqui está um manual completo sobre ponteiros em C:

```markdown
# Manual Completo de Ponteiros em C

## Índice
1. [Introdução](#introdução)
2. [Conceitos Fundamentais](#conceitos-fundamentais)
3. [Declaração e Inicialização](#declaração-e-inicialização)
4. [Operadores Básicos](#operadores-básicos)
5. [Ponteiros e Arrays](#ponteiros-e-arrays)
6. [Aritmética de Ponteiros](#aritmética-de-ponteiros)
7. [Ponteiros para Ponteiros](#ponteiros-para-ponteiros)
8. [Ponteiros e Funções](#ponteiros-e-funções)
9. [Alocação Dinâmica](#alocação-dinâmica)
10. [Ponteiros para Structs](#ponteiros-para-structs)
11. [Ponteiros Constantes e Constantes Ponteiros](#ponteiros-constantes-e-constantes-ponteiros)
12. [Ponteiros Void](#ponteiros-void)
13. [Armadilhas e Boas Práticas](#armadilhas-e-boas-práticas)
14. [Exercícios Práticos](#exercícios-práticos)

---

## Introdução

Ponteiros são uma das características mais poderosas (e temidas) da linguagem C. Eles permitem manipular diretamente a memória do computador, possibilitando:

- Acesso e modificação eficiente de dados
- Alocação dinâmica de memória
- Passagem de parâmetros por referência
- Implementação de estruturas de dados complexas
- Otimização de performance

Este manual foi desenvolvido para levar você do absoluto zero ao domínio completo de ponteiros.

---

## Conceitos Fundamentais

### O que é Memória?

A memória RAM pode ser visualizada como uma longa sequência de bytes, cada um com um endereço único:

```
Endereço:   1000    1001    1002    1003    1004    1005
           +------+------+------+------+------+------+
Conteúdo:  |  42  |   FF |   0  |   A  |   7  |   3  |
           +------+------+------+------+------+------+
```

### O que é um Ponteiro?

Um **ponteiro** é uma variável que armazena um endereço de memória. Em vez de guardar um valor direto (como 42 ou 'A'), um ponteiro guarda a localização onde esse valor está armazenado.

### Por que usar Ponteiros?

```c
// Sem ponteiros - passagem por valor
void dobrar(int x) {
    x = x * 2;  // Modifica apenas a cópia local
}

// Com ponteiros - passagem por referência
void dobrar(int *x) {
    *x = *x * 2;  // Modifica o valor original
}

int main() {
    int num = 10;
    dobrar(&num);  // num agora é 20
    return 0;
}
```

---

## Declaração e Inicialização

### Sintaxe Básica

```c
tipo *nome_do_ponteiro;
```

Exemplos:
```c
int *ptr;           // Ponteiro para inteiro
char *ptr_char;     // Ponteiro para caractere
float *ptr_float;   // Ponteiro para float
double *ptr_double; // Ponteiro para double
```

### Inicialização

```c
int numero = 42;
int *ptr = &numero;  // ptr recebe o endereço de numero

// Ou separadamente
int *ptr;
ptr = &numero;
```

### O Endereço Zero (NULL)

```c
int *ptr = NULL;  // Ponteiro que não aponta para lugar nenhum

// Sempre bom verificar antes de usar
if (ptr != NULL) {
    *ptr = 10;  // Seguro apenas se ptr não for NULL
}
```

---

## Operadores Básicos

### Operador & (Endereço)

Obtém o endereço de memória de uma variável:

```c
int x = 10;
int *ptr = &x;

printf("Valor de x: %d\n", x);        // 10
printf("Endereço de x: %p\n", &x);    // 0x7ffd5e3e4 (exemplo)
printf("Valor de ptr: %p\n", ptr);    // 0x7ffd5e3e4 (mesmo endereço)
```

### Operador * (Dereferência)

Acessa o valor armazenado no endereço apontado:

```c
int x = 10;
int *ptr = &x;

printf("Valor apontado: %d\n", *ptr);  // 10

*ptr = 20;  // Modifica x através do ponteiro
printf("Novo valor de x: %d\n", x);    // 20
```

### Visualização na Memória

```
Variável x:
Endereço: 0x1000
Valor:    10

Ponteiro ptr:
Endereço: 0x2000
Valor:    0x1000 (endereço de x)

Quando fazemos *ptr, acessamos:
0x1000 -> 10
```

---

## Ponteiros e Arrays

### Relação Fundamental

Em C, o nome de um array é um ponteiro constante para seu primeiro elemento:

```c
int arr[5] = {10, 20, 30, 40, 50};
int *ptr = arr;  // Equivalente a: int *ptr = &arr[0];

printf("%d\n", *ptr);     // 10 (primeiro elemento)
printf("%d\n", *(ptr+1)); // 20 (segundo elemento)
```

### Equivalência Array-Ponteiro

```c
int arr[5] = {1, 2, 3, 4, 5};

// Todas as formas são equivalentes:
arr[2] = 10;        // Usando índice
*(arr + 2) = 10;    // Usando aritmética de ponteiros
2[arr] = 10;        // Forma alternativa (válida, mas não recomendada)
```

### Percorrendo Arrays com Ponteiros

```c
int arr[] = {10, 20, 30, 40, 50};
int tamanho = sizeof(arr) / sizeof(arr[0]);

// Método 1: Com índice
for(int i = 0; i < tamanho; i++) {
    printf("%d ", arr[i]);
}

// Método 2: Com ponteiro
for(int *ptr = arr; ptr < arr + tamanho; ptr++) {
    printf("%d ", *ptr);
}
```

---

## Aritmética de Ponteiros

### Operações Permitidas

```c
int arr[] = {10, 20, 30, 40, 50};
int *p = arr;

p++;           // Avança para o próximo inteiro
p--;           // Volta para o inteiro anterior
p += 2;        // Avança 2 posições
p -= 1;        // Volta 1 posição
int diff = p - arr;  // Diferença entre ponteiros (número de elementos)
```

### Como a Aritmética Funciona

```c
int *ptr;
ptr + n  // Equivalente a: (endereço em ptr) + (n * sizeof(tipo))

// Exemplo prático:
int arr[5] = {10, 20, 30, 40, 50};
int *ptr = arr;  // ptr aponta para arr[0]

ptr++;  // ptr agora aponta para arr[1] (avança 4 bytes se int tem 4 bytes)
```

### Exemplo Detalhado

```c
#include <stdio.h>

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *p1 = arr;
    int *p2 = &arr[3];
    
    printf("p1 aponta para: %d\n", *p1);     // 10
    printf("p2 aponta para: %d\n", *p2);     // 40
    
    p1++;  // Agora aponta para 20
    printf("Após p1++: %d\n", *p1);          // 20
    
    printf("Distância entre p2 e p1: %ld\n", p2 - p1);  // 2 (posições)
    
    return 0;
}
```

---

## Ponteiros para Ponteiros

### Conceito

Um ponteiro para ponteiro armazena o endereço de outro ponteiro:

```c
int x = 10;
int *p = &x;    // Ponteiro para inteiro
int **pp = &p;  // Ponteiro para ponteiro para inteiro
```

### Representação na Memória

```
Variável x:
Endereço: 0x1000
Valor:    10

Ponteiro p:
Endereço: 0x2000
Valor:    0x1000

Ponteiro pp:
Endereço: 0x3000
Valor:    0x2000

Para acessar x através de pp:
**pp -> *(*(pp)) -> *(0x2000) -> 0x1000 -> 10
```

### Exemplo Prático

```c
#include <stdio.h>

int main() {
    int x = 42;
    int *p = &x;
    int **pp = &p;
    
    printf("x = %d\n", x);           // 42
    printf("*p = %d\n", *p);         // 42
    printf("**pp = %d\n", **pp);      // 42
    
    **pp = 100;  // Modifica x através de pp
    printf("Novo x = %d\n", x);       // 100
    
    return 0;
}
```

### Uso Comum: Matrizes Dinâmicas

```c
int **matriz;
matriz = malloc(linhas * sizeof(int *));

for(int i = 0; i < linhas; i++) {
    matriz[i] = malloc(colunas * sizeof(int));
}
```

---

## Ponteiros e Funções

### Parâmetros por Referência

```c
void trocar(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5, y = 10;
    trocar(&x, &y);
    // x = 10, y = 5
    return 0;
}
```

### Retornando Ponteiros

```c
int* criar_array(int tamanho) {
    int *arr = malloc(tamanho * sizeof(int));
    if (arr == NULL) {
        return NULL;  // Falha na alocação
    }
    
    for(int i = 0; i < tamanho; i++) {
        arr[i] = i * 10;
    }
    
    return arr;
}

int main() {
    int *meu_array = criar_array(5);
    if (meu_array != NULL) {
        // Usar o array
        free(meu_array);  // Não esquecer de liberar!
    }
    return 0;
}
```

### Ponteiros para Funções

```c
// Declaração de um ponteiro para função
int (*operacao)(int, int);

// Funções que podem ser apontadas
int somar(int a, int b) { return a + b; }
int multiplicar(int a, int b) { return a * b; }

int main() {
    operacao = somar;
    printf("Soma: %d\n", operacao(5, 3));  // 8
    
    operacao = multiplicar;
    printf("Multiplicação: %d\n", operacao(5, 3));  // 15
    
    return 0;
}
```

### Array de Ponteiros para Funções

```c
int (*operacoes[])(int, int) = {somar, multiplicar, subtrair};

// Chamando cada função
for(int i = 0; i < 3; i++) {
    printf("Resultado: %d\n", operacoes[i](10, 5));
}
```

---

## Alocação Dinâmica

### malloc()

Aloca um bloco de memória não inicializado:

```c
int *ptr = malloc(10 * sizeof(int));
if (ptr == NULL) {
    // Tratar erro de alocação
    printf("Falha na alocação de memória\n");
    return 1;
}

// Usar a memória...
ptr[0] = 42;
ptr[1] = 100;

free(ptr);  // Liberar memória
```

### calloc()

Aloca e inicializa com zero:

```c
int *ptr = calloc(10, sizeof(int));
// Todos os 10 inteiros são inicializados com 0
```

### realloc()

Redimensiona um bloco de memória:

```c
int *ptr = malloc(5 * sizeof(int));
if (ptr == NULL) {
    printf("Erro de alocação\n");
    return 1;
}

// Preencher os 5 elementos...

// Precisa de mais espaço
int *temp = realloc(ptr, 10 * sizeof(int));
if (temp == NULL) {
    printf("Erro ao realocar\n");
    free(ptr);
    return 1;
}

ptr = temp;  // Agora ptr tem espaço para 10 inteiros
```

### Exemplo Completo

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Quantos números? ");
    scanf("%d", &n);
    
    // Alocar dinamicamente
    int *numeros = malloc(n * sizeof(int));
    if (numeros == NULL) {
        printf("Erro: memória insuficiente\n");
        return 1;
    }
    
    // Ler números
    for(int i = 0; i < n; i++) {
        printf("Número %d: ", i+1);
        scanf("%d", &numeros[i]);
    }
    
    // Calcular soma
    int soma = 0;
    for(int i = 0; i < n; i++) {
        soma += numeros[i];
    }
    
    printf("Soma: %d\n", soma);
    printf("Média: %.2f\n", (float)soma / n);
    
    free(numeros);  // Liberar memória
    return 0;
}
```

### Armadilhas Comuns

```c
// VAZAMENTO DE MEMÓRIA
void problema() {
    int *ptr = malloc(100 * sizeof(int));
    // Esqueceu de fazer free(ptr)
    // Memória nunca é liberada!
}

// DANGLING POINTER (Ponteiro solto)
int *ptr = malloc(sizeof(int));
free(ptr);
*ptr = 42;  // ERRO: ptr não é mais válido!

// DOUBLE FREE
int *ptr = malloc(sizeof(int));
free(ptr);
free(ptr);  // ERRO: tentando liberar duas vezes
```

---

## Ponteiros para Structs

### Declaração e Uso

```c
struct Pessoa {
    char nome[50];
    int idade;
    float altura;
};

int main() {
    struct Pessoa pessoa = {"João", 30, 1.75};
    struct Pessoa *ptr = &pessoa;
    
    // Acessando membros (duas formas)
    printf("Nome: %s\n", (*ptr).nome);     // Forma 1
    printf("Idade: %d\n", ptr->idade);     // Forma 2 (operador ->)
    printf("Altura: %.2f\n", ptr->altura); // Forma 2
    
    // Modificando através do ponteiro
    ptr->idade = 31;
    strcpy(ptr->nome, "João Silva");
    
    return 0;
}
```

### Alocação Dinâmica de Structs

```c
struct Pessoa* criar_pessoa(const char *nome, int idade, float altura) {
    struct Pessoa *p = malloc(sizeof(struct Pessoa));
    if (p == NULL) return NULL;
    
    strcpy(p->nome, nome);
    p->idade = idade;
    p->altura = altura;
    
    return p;
}

int main() {
    struct Pessoa *p = criar_pessoa("Maria", 25, 1.68);
    
    if (p != NULL) {
        printf("%s tem %d anos e %.2f m\n", 
               p->nome, p->idade, p->altura);
        free(p);
    }
    
    return 0;
}
```

### Ponteiros para Structs e Funções

```c
void atualizar_idade(struct Pessoa *p, int nova_idade) {
    if (p != NULL) {
        p->idade = nova_idade;
    }
}

// Passando struct por referência (eficiente)
void imprimir_pessoa(const struct Pessoa *p) {
    printf("Nome: %s, Idade: %d\n", p->nome, p->idade);
    // Não modifica porque é const
}
```

### Structs com Ponteiros

```c
struct Lista {
    int valor;
    struct Lista *proximo;  // Ponteiro para o próximo elemento
};

int main() {
    struct Lista nó1 = {10, NULL};
    struct Lista nó2 = {20, NULL};
    
    nó1.proximo = &nó2;  // nó1 aponta para nó2
    
    printf("%d\n", nó1.proximo->valor);  // 20
    
    return 0;
}
```

---

## Ponteiros Constantes e Constantes Ponteiros

### Ponteiro para Constante

O valor apontado não pode ser modificado:

```c
int x = 10, y = 20;
const int *ptr = &x;

*ptr = 30;  // ERRO: não pode modificar o valor
ptr = &y;   // OK: pode mudar para onde aponta
```

### Ponteiro Constante

Não pode apontar para outro local:

```c
int x = 10, y = 20;
int *const ptr = &x;

*ptr = 30;  // OK: pode modificar o valor
ptr = &y;   // ERRO: não pode mudar o endereço
```

### Ponteiro Constante para Constante

Nem o valor pode ser modificado, nem o endereço:

```c
int x = 10, y = 20;
const int *const ptr = &x;

*ptr = 30;  // ERRO: não pode modificar o valor
ptr = &y;   // ERRO: não pode mudar o endereço
```

### Tabela Resumo

| Declaração | Pode modificar valor? | Pode mudar endereço? |
|------------|----------------------|---------------------|
| `int *ptr` | Sim | Sim |
| `const int *ptr` | Não | Sim |
| `int *const ptr` | Sim | Não |
| `const int *const ptr` | Não | Não |

---

## Ponteiros Void

### Conceito

Ponteiros `void*` podem apontar para qualquer tipo de dado:

```c
void *ptr;
int x = 10;
float y = 3.14;
char c = 'A';

ptr = &x;  // OK
ptr = &y;  // OK
ptr = &c;  // OK
```

### Necessidade de Cast

```c
void *ptr;
int x = 42;
ptr = &x;

// ERRO: não pode dereferenciar void*
// printf("%d", *ptr);

// Precisa fazer cast
printf("%d", *(int*)ptr);  // OK
```

### Uso em Funções Genéricas

```c
void imprimir_valor(void *ptr, char tipo) {
    switch(tipo) {
        case 'i':
            printf("Inteiro: %d\n", *(int*)ptr);
            break;
        case 'f':
            printf("Float: %.2f\n", *(float*)ptr);
            break;
        case 'c':
            printf("Char: %c\n", *(char*)ptr);
            break;
    }
}

int main() {
    int a = 10;
    float b = 3.14;
    char c = 'Z';
    
    imprimir_valor(&a, 'i');
    imprimir_valor(&b, 'f');
    imprimir_valor(&c, 'c');
    
    return 0;
}
```

### qsort() - Exemplo Clássico

```c
int comparar_int(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}

int main() {
    int arr[] = {5, 2, 8, 1, 9, 3};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    qsort(arr, n, sizeof(int), comparar_int);
    
    for(int i = 0; i < n; i++) {
        printf("%d ", arr[i]);  // 1 2 3 5 8 9
    }
    
    return 0;
}
```

---

## Armadilhas e Boas Práticas

### Armadilhas Comuns

#### 1. Ponteiros Não Inicializados

```c
int *ptr;
*ptr = 42;  // ERRO GRAVE: ptr não aponta para lugar válido

// Correto:
int x;
int *ptr = &x;
*ptr = 42;  // OK
```

#### 2. Esquecer de Liberar Memória

```c
void vazamento() {
    int *ptr = malloc(1000 * sizeof(int));
    // Usar ptr...
    // Esqueceu free(ptr) - VAZAMENTO!
}
```

#### 3. Acessar Memória Liberada

```c
int *ptr = malloc(sizeof(int));
*ptr = 42;
free(ptr);
*ptr = 100;  // ERRO: dangling pointer
```

#### 4. Buffer Overflow

```c
int arr[5];
int *ptr = arr;

for(int i = 0; i <= 10; i++) {  // ERRO: vai além do array
    *(ptr + i) = i;  // Corrompe memória
}
```

### Boas Práticas

#### 1. Sempre Inicializar Ponteiros

```c
int *ptr = NULL;  // Inicialização segura
// ou
int *ptr = &alguma_variavel;
```

#### 2. Verificar NULL antes de Usar

```c
int *ptr = malloc(sizeof(int));
if (ptr != NULL) {
    *ptr = 42;  // Seguro
} else {
    // Tratar erro
}
```

#### 3. Liberar e Setar NULL

```c
free(ptr);
ptr = NULL;  // Evita dangling pointers
```

#### 4. Usar sizeof para Portabilidade

```c
int *ptr = malloc(10 * sizeof(int));  // Correto
// Não usar: malloc(40) - assumindo que int tem 4 bytes
```

#### 5. Documentar Propriedade de Memória

```c
// Retorna um ponteiro para memória alocada.
// O chamador é responsável por liberar com free().
int* criar_array(int tamanho) {
    return malloc(tamanho * sizeof(int));
}
```

### Checklist de Segurança

- [ ] Ponteiro foi inicializado?
- [ ] Ponteiro não é NULL?
- [ ] Índices estão dentro dos limites?
- [ ] Memória foi liberada?
- [ ] Ponteiro não está sendo usado após free?
- [ ] Cast de void* é seguro?

---

## Exercícios Práticos

### Exercício 1: Básico
Escreva um programa que declare uma variável inteira, um ponteiro para inteiro, e use o ponteiro para modificar o valor da variável.

```c
// Solução
#include <stdio.h>

int main() {
    int x = 10;
    int *ptr = &x;
    
    printf("Antes: %d\n", x);
    *ptr = 20;
    printf("Depois: %d\n", x);
    
    return 0;
}
```

### Exercício 2: Array e Ponteiros
Crie uma função que recebe um array de inteiros e seu tamanho, e retorna a soma dos elementos usando aritmética de ponteiros.

```c
#include <stdio.h>

int somar_array(int *arr, int tamanho) {
    int soma = 0;
    for(int *p = arr; p < arr + tamanho; p++) {
        soma += *p;
    }
    return soma;
}

int main() {
    int nums[] = {1, 2, 3, 4, 5};
    printf("Soma: %d\n", somar_array(nums, 5));
    return 0;
}
```

### Exercício 3: Troca de Valores
Implemente uma função que troca os valores de duas variáveis usando ponteiros.

```c
#include <stdio.h>

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

### Exercício 4: Alocação Dinâmica
Crie um programa que aloca dinamicamente um array de N inteiros, preenche com números sequenciais e imprime.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Quantos números? ");
    scanf("%d", &n);
    
    int *arr = malloc(n * sizeof(int));
    if (arr == NULL) {
        printf("Erro de alocação\n");
        return 1;
    }
    
    for(int i = 0; i < n; i++) {
        arr[i] = i + 1;
    }
    
    for(int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    
    free(arr);
    return 0;
}
```

### Exercício 5: Ponteiro para Ponteiro
Implemente uma função que modifica um ponteiro (o ponteiro em si, não o valor apontado).

```c
#include <stdio.h>
#include <stdlib.h>

void alocar_e_preencher(int **ptr, int tamanho) {
    *ptr = malloc(tamanho * sizeof(int));
    if (*ptr == NULL) return;
    
    for(int i = 0; i < tamanho; i++) {
        (*ptr)[i] = i * 10;
    }
}

int main() {
    int *arr = NULL;
    alocar_e_preencher(&arr, 5);
    
    if (arr != NULL) {
        for(int i = 0; i < 5; i++) {
            printf("%d ", arr[i]);  // 0 10 20 30 40
        }
        free(arr);
    }
    
    return 0;
}
```

### Exercício 6: Structs e Ponteiros
Crie uma struct `Ponto` com coordenadas x e y. Implemente funções para criar, modificar e calcular distância entre pontos.

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

typedef struct {
    float x;
    float y;
} Ponto;

Ponto* criar_ponto(float x, float y) {
    Ponto *p = malloc(sizeof(Ponto));
    if (p != NULL) {
        p->x = x;
        p->y = y;
    }
    return p;
}

void mover(Ponto *p, float dx, float dy) {
    if (p != NULL) {
        p->x += dx;
        p->y += dy;
    }
}

float distancia(Ponto *a, Ponto *b) {
    if (a == NULL || b == NULL) return -1;
    float dx = a->x - b->x;
    float dy = a->y - b->y;
    return sqrt(dx*dx + dy*dy);
}

int main() {
    Ponto *p1 = criar_ponto(0, 0);
    Ponto *p2 = criar_ponto(3, 4);
    
    printf("Distância: %.2f\n", distancia(p1, p2));
    
    mover(p1, 1, 1);
    printf("Nova distância: %.2f\n", distancia(p1, p2));
    
    free(p1);
    free(p2);
    return 0;
}
```

### Exercício 7: Lista Encadeada Simples
Implemente uma lista encadeada básica com inserção no início.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct No {
    int valor;
    struct No *proximo;
} No;

No* inserir_inicio(No *cabeca, int valor) {
    No *novo = malloc(sizeof(No));
    if (novo == NULL) return cabeca;
    
    novo->valor = valor;
    novo->proximo = cabeca;
    return novo;
}

void imprimir_lista(No *cabeca) {
    for(No *atual = cabeca; atual != NULL; atual = atual->proximo) {
        printf("%d ", atual->valor);
    }
    printf("\n");
}

void liberar_lista(No *cabeca) {
    No *atual = cabeca;
    while(atual != NULL) {
        No *temp = atual;
        atual = atual->proximo;
        free(temp);
    }
}

int main() {
    No *lista = NULL;
    
    lista = inserir_inicio(lista, 30);
    lista = inserir_inicio(lista, 20);
    lista = inserir_inicio(lista, 10);
    
    imprimir_lista(lista);  // 10 20 30
    
    liberar_lista(lista);
    return 0;
}
```

### Exercício 8: Função de Ordenação
Implemente uma função de ordenação que usa ponteiros para trocar elementos.

```c
#include <stdio.h>

void ordenar(int *arr, int n) {
    for(int i = 0; i < n-1; i++) {
        for(int j = i+1; j < n; j++) {
            if (*(arr + i) > *(arr + j)) {
                int temp = *(arr + i);
                *(arr + i) = *(arr + j);
                *(arr + j) = temp;
            }
        }
    }
}

int main() {
    int nums[] = {5, 2, 8, 1, 9, 3};
    int n = sizeof(nums) / sizeof(nums[0]);
    
    ordenar(nums, n);
    
    for(int i = 0; i < n; i++) {
        printf("%d ", nums[i]);  // 1 2 3 5 8 9
    }
    
    return 0;
}
```

### Exercício 9: Matriz Dinâmica
Crie uma função que aloca uma matriz dinamicamente e outra que a libera.

```c
#include <stdio.h>
#include <stdlib.h>

int** criar_matriz(int linhas, int colunas) {
    int **mat = malloc(linhas * sizeof(int*));
    if (mat == NULL) return NULL;
    
    for(int i = 0; i < linhas; i++) {
        mat[i] = malloc(colunas * sizeof(int));
        if (mat[i] == NULL) {
            // Liberar já alocadas em caso de erro
            for(int j = 0; j < i; j++) {
                free(mat[j]);
            }
            free(mat);
            return NULL;
        }
    }
    
    return mat;
}

void preencher_matriz(int **mat, int linhas, int colunas) {
    int cont = 1;
    for(int i = 0; i < linhas; i++) {
        for(int j = 0; j < colunas; j++) {
            mat[i][j] = cont++;
        }
    }
}

void imprimir_matriz(int **mat, int linhas, int colunas) {
    for(int i = 0; i < linhas; i++) {
        for(int j = 0; j < colunas; j++) {
            printf("%3d ", mat[i][j]);
        }
        printf("\n");
    }
}

void liberar_matriz(int **mat, int linhas) {
    for(int i = 0; i < linhas; i++) {
        free(mat[i]);
    }
    free(mat);
}

int main() {
    int linhas = 3, colunas = 4;
    
    int **mat = criar_matriz(linhas, colunas);
    if (mat == NULL) {
        printf("Erro de alocação\n");
        return 1;
    }
    
    preencher_matriz(mat, linhas, colunas);
    imprimir_matriz(mat, linhas, colunas);
    
    liberar_matriz(mat, linhas);
    return 0;
}
```

### Exercício 10: Callback com Ponteiro para Função
Implemente uma função que recebe um array e uma função de callback para processar cada elemento.

```c
#include <stdio.h>

int quadrado(int x) { return x * x; }
int cubo(int x) { return x * x * x; }
int dobrar(int x) { return x * 2; }

void processar_array(int *arr, int n, int (*func)(int)) {
    for(int i = 0; i < n; i++) {
        arr[i] = func(arr[i]);
    }
}

int main() {
    int nums[] = {1, 2, 3, 4, 5};
    int n = sizeof(nums) / sizeof(nums[0]);
    
    printf("Original: ");
    for(int i = 0; i < n; i++) printf("%d ", nums[i]);
    printf("\n");
    
    processar_array(nums, n, quadrado);
    printf("Quadrado: ");
    for(int i = 0; i < n; i++) printf("%d ", nums[i]);
    printf("\n");
    
    processar_array(nums, n, cubo);
    printf("Cubo: ");
    for(int i = 0; i < n; i++) printf("%d ", nums[i]);
    printf("\n");
    
    return 0;
}
```

---

## Resumo Visual

```
MEMÓRIA:
+---------+---------+---------+---------+
| Endereço|  Valor  | Variável| Ponteiro|
+---------+---------+---------+---------+
| 0x1000  |   42    |    x    |         |
| 0x1004  | 0x1000  |    p    |    &x   |
| 0x1008  | 0x1004  |    pp   |    &p   |
+---------+---------+---------+---------+

OPERAÇÕES:
&x   → 0x1000 (endereço de x)
p    → 0x1000 (valor do ponteiro)
*p   → 42     (valor apontado)
&p   → 0x1004 (endereço do ponteiro)
pp   → 0x1004 (valor do ponteiro para ponteiro)
*pp  → 0x1000 (valor apontado pelo ponteiro para ponteiro)
**pp → 42     (valor final)
```

---

## Conclusão

Ponteiros são fundamentais em C e, embora possam parecer complexos inicialmente, tornam-se ferramentas poderosas quando dominados. Lembre-se:

1. **Sempre inicialize** seus ponteiros
2. **Verifique NULL** antes de usar
3. **Libere memória** alocada dinamicamente
4. **Evite acessar** memória já liberada
5. **Pratique muito** - a melhor forma de aprender

Com este manual e os exercícios práticos, você está preparado para dominar ponteiros em C. Continue praticando e explorando!

---

## Referências Rápidas

### Operadores
- `&` : endereço de
- `*` : dereferência
- `->`: acesso a membro de struct via ponteiro

### Funções de Alocação
- `malloc(size)`: aloca memória
- `calloc(n, size)`: aloca e zera
- `realloc(ptr, size)`: realoca
- `free(ptr)`: libera

### Sintaxes Importantes
```c
tipo *ptr;           // Declaração
ptr = &var;          // Atribuição de endereço
*ptr = valor;        // Atribuição de valor
ptr->membro          // Acesso a struct
int **ptr;           // Ponteiro para ponteiro
int *ptr[10]         // Array de 10 ponteiros
int (*ptr)[10]       // Ponteiro para array de 10 ints
```
```
