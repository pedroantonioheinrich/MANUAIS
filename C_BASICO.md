# 📘 MANUAL BÁSICO DE LINGUAGEM C
*Para iniciantes na programação C*

---

## 📋 SUMÁRIO
1. [Introdução à Linguagem C](#1-introdução-à-linguagem-c)
2. [Estrutura de um Programa C](#2-estrutura-de-um-programa-c)
3. [Variáveis e Tipos de Dados](#3-variáveis-e-tipos-de-dados)
4. [Operadores](#4-operadores)
5. [Estruturas de Controle](#5-estruturas-de-controle)
6. [Entrada e Saída Básica](#6-entrada-e-saída-básica)
7. [Compilação e Execução](#7-compilação-e-execução)

---

## 1. INTRODUÇÃO À LINGUAGEM C

### O que é C?
C é uma linguagem de programação estruturada, compilada e de propósito geral, criada por Dennis Ritchie em 1972 nos laboratórios Bell. É amplamente usada para desenvolvimento de sistemas, aplicações embarcadas e como base para outras linguagens.

### Características Principais:
- **Compilada**: Código fonte é traduzido para linguagem de máquina
- **Estruturada**: Suporta funções e blocos de código
- **Portátil**: Código pode rodar em diferentes plataformas
- **Eficiente**: Controle direto sobre recursos do sistema

---

## 2. ESTRUTURA DE UM PROGRAMA C

### Exemplo Básico:
```c
#include <stdio.h>  // Diretiva de inclusão

int main() {        // Função principal
    printf("Olá, Mundo!\n");
    return 0;       // Retorno do programa
}
```

### Partes do Programa:
1. **Comentários**: `// Comentário de linha única` ou `/* Comentário de múltiplas linhas */`
2. **Diretivas do pré-processador**: Começam com `#`
3. **Função main()**: Ponto de entrada obrigatório
4. **Corpo do programa**: Entre chaves `{}`
5. **Declarações**: Instruções terminadas com `;`

---

## 3. VARIÁVEIS E TIPOS DE DADOS

### Declaração de Variáveis:
```c
tipo nome_variavel;
tipo nome_variavel = valor_inicial;
```

### Tipos de Dados Básicos:

| Tipo | Tamanho (típico) | Intervalo | Uso |
|------|------------------|-----------|-----|
| `char` | 1 byte | -128 a 127 | Caracteres |
| `int` | 4 bytes | -2,147,483,648 a 2,147,483,647 | Números inteiros |
| `float` | 4 bytes | ~1.2E-38 a 3.4E+38 | Ponto flutuante (precisão simples) |
| `double` | 8 bytes | ~2.3E-308 a 1.7E+308 | Ponto flutuante (dupla precisão) |
| `void` | - | - | Tipo vazio/sem valor |

### Exemplos Práticos:
```c
#include <stdio.h>

int main() {
    // Declaração de variáveis
    int idade = 25;
    float altura = 1.75;
    double peso = 70.5;
    char inicial = 'J';
    char nome[] = "João";  // Array de caracteres
    
    // Modificadores de tipo
    short int pequeno = 32000;     // Inteiro curto
    long int grande = 2000000000;  // Inteiro longo
    unsigned int positivo = 4000000000; // Sem sinal
    
    printf("Idade: %d anos\n", idade);
    printf("Altura: %.2f metros\n", altura);
    printf("Inicial: %c\n", inicial);
    printf("Nome: %s\n", nome);
    
    return 0;
}
```

### Constantes:
```c
const float PI = 3.14159;
#define MAX 100  // Constante do pré-processador
```

---

## 4. OPERADORES

### Operadores Aritméticos:
```c
int a = 10, b = 3;
int soma = a + b;      // 13
int subtracao = a - b; // 7
int multiplicacao = a * b; // 30
int divisao = a / b;   // 3 (divisão inteira)
int resto = a % b;     // 1
```

### Operadores de Atribuição:
```c
int x = 5;
x += 3;   // Equivalente a x = x + 3 (x = 8)
x -= 2;   // x = 6
x *= 4;   // x = 24
x /= 3;   // x = 8
x %= 5;   // x = 3
```

### Operadores Relacionais:
```c
int a = 5, b = 10;
int resultado;

resultado = (a == b);  // Igual a: 0 (falso)
resultado = (a != b);  // Diferente: 1 (verdadeiro)
resultado = (a < b);   // Menor que: 1
resultado = (a > b);   // Maior que: 0
resultado = (a <= b);  // Menor ou igual: 1
resultado = (a >= b);  // Maior ou igual: 0
```

### Operadores Lógicos:
```c
int a = 1, b = 0;

// AND lógico
if (a && b) { /* código */ }  // Falso

// OR lógico  
if (a || b) { /* código */ }  // Verdadeiro

// NOT lógico
if (!b) { /* código */ }      // Verdadeiro
```

### Incremento/Decremento:
```c
int i = 5;

printf("%d\n", i++);  // Imprime 5, depois i = 6
printf("%d\n", ++i);  // i = 7, depois imprime 7
printf("%d\n", i--);  // Imprime 7, depois i = 6
printf("%d\n", --i);  // i = 5, depois imprime 5
```

### Operador Ternário:
```c
int idade = 18;
char* status = (idade >= 18) ? "Adulto" : "Menor";
// Se idade >= 18, status = "Adulto", senão "Menor"
```

---

## 5. ESTRUTURAS DE CONTROLE

### Condicional if-else:
```c
#include <stdio.h>

int main() {
    int nota;
    
    printf("Digite a nota (0-100): ");
    scanf("%d", &nota);
    
    if (nota >= 90) {
        printf("Conceito A\n");
    } 
    else if (nota >= 80) {
        printf("Conceito B\n");
    }
    else if (nota >= 70) {
        printf("Conceito C\n");
    }
    else {
        printf("Reprovado\n");
    }
    
    return 0;
}
```

### Switch-Case:
```c
#include <stdio.h>

int main() {
    int opcao;
    
    printf("Menu:\n");
    printf("1. Adicionar\n");
    printf("2. Remover\n");
    printf("3. Listar\n");
    printf("Escolha: ");
    scanf("%d", &opcao);
    
    switch(opcao) {
        case 1:
            printf("Adicionando item...\n");
            break;
        case 2:
            printf("Removendo item...\n");
            break;
        case 3:
            printf("Listando itens...\n");
            break;
        default:
            printf("Opção inválida!\n");
    }
    
    return 0;
}
```

### Loop While:
```c
#include <stdio.h>

int main() {
    int contador = 1;
    
    // While: verifica condição ANTES de executar
    while (contador <= 5) {
        printf("Contador: %d\n", contador);
        contador++;
    }
    
    // Do-While: executa PELO MENOS UMA VEZ
    int numero;
    do {
        printf("Digite um número positivo: ");
        scanf("%d", &numero);
    } while (numero <= 0);
    
    return 0;
}
```

### Loop For:
```c
#include <stdio.h>

int main() {
    int i;
    
    // Forma básica
    for (i = 0; i < 10; i++) {
        printf("%d ", i);
    }
    printf("\n");
    
    // Contagem regressiva
    for (i = 10; i > 0; i--) {
        printf("%d ", i);
    }
    printf("\n");
    
    // Com múltiplas variáveis
    for (i = 0, j = 10; i < j; i++, j--) {
        printf("i=%d, j=%d\n", i, j);
    }
    
    return 0;
}
```

### Controle de Loops (break e continue):
```c
#include <stdio.h>

int main() {
    int i;
    
    // Exemplo com break
    printf("Exemplo break:\n");
    for (i = 1; i <= 10; i++) {
        if (i == 6) {
            break;  // Sai do loop quando i == 6
        }
        printf("%d ", i);
    }
    printf("\n");
    
    // Exemplo com continue
    printf("Exemplo continue:\n");
    for (i = 1; i <= 10; i++) {
        if (i % 2 == 0) {
            continue;  // Pula números pares
        }
        printf("%d ", i);
    }
    printf("\n");
    
    return 0;
}
```

---

## 6. ENTRADA E SAÍDA BÁSICA

### Saída com printf():
```c
#include <stdio.h>

int main() {
    int idade = 25;
    float altura = 1.78;
    char letra = 'A';
    char nome[] = "Maria";
    
    // Especificadores de formato:
    printf("Inteiro: %d\n", idade);           // %d para int
    printf("Float: %.2f\n", altura);          // %f para float, .2 = 2 casas decimais
    printf("Char: %c\n", letra);              // %c para char
    printf("String: %s\n", nome);             // %s para string
    printf("Octal: %o\n", idade);             // %o para octal
    printf("Hexadecimal: %x\n", idade);       // %x para hexadecimal
    
    // Múltiplas variáveis
    printf("%s tem %d anos e %.2f de altura\n", nome, idade, altura);
    
    return 0;
}
```

### Entrada com scanf():
```c
#include <stdio.h>

int main() {
    int idade;
    float altura;
    char nome[50];
    
    printf("Digite seu nome: ");
    scanf("%s", nome);  // Para strings sem espaços
    
    printf("Digite sua idade: ");
    scanf("%d", &idade);  // Note o & antes do nome da variável
    
    printf("Digite sua altura: ");
    scanf("%f", &altura);
    
    printf("\nDados informados:\n");
    printf("Nome: %s\n", nome);
    printf("Idade: %d anos\n", idade);
    printf("Altura: %.2f metros\n", altura);
    
    return 0;
}
```

### Limpando o buffer de entrada:
```c
#include <stdio.h>

int main() {
    int numero;
    char letra;
    
    printf("Digite um número: ");
    scanf("%d", &numero);
    
    // Limpa o buffer do teclado
    while(getchar() != '\n');
    
    printf("Digite uma letra: ");
    scanf("%c", &letra);
    
    printf("Número: %d, Letra: %c\n", numero, letra);
    
    return 0;
}
```

### Outras funções de I/O:
```c
#include <stdio.h>

int main() {
    char c;
    
    // getchar() - lê um caractere
    printf("Digite um caractere: ");
    c = getchar();
    
    // putchar() - escreve um caractere
    printf("Você digitou: ");
    putchar(c);
    putchar('\n');
    
    // gets() e puts() para strings (cuidado: gets é perigoso)
    char texto[100];
    printf("Digite uma frase: ");
    fgets(texto, 100, stdin);  // Alternativa segura a gets()
    printf("Frase: ");
    puts(texto);
    
    return 0;
}
```

---

## 7. COMPILAÇÃO E EXECUÇÃO

### Processo de Compilação:
```
Código Fonte (.c) → Pré-processador → Compilador → Assembly → Montador → Executável
```

### Compilando no Terminal:
```bash
# Compilação básica
gcc programa.c -o programa

# Compilar com warnings ativados
gcc -Wall programa.c -o programa

# Compilar e gerar informações de depuração
gcc -g programa.c -o programa

# Compilar com otimização
gcc -O2 programa.c -o programa
```

### Exemplo Completo:
```c
/* 
 * calculadora_simples.c
 * Programa que calcula as 4 operações básicas
 */

#include <stdio.h>

int main() {
    float num1, num2;
    char operador;
    
    printf("=== CALCULADORA SIMPLES ===\n");
    
    // Entrada de dados
    printf("Digite o primeiro número: ");
    scanf("%f", &num1);
    
    printf("Digite a operação (+, -, *, /): ");
    scanf(" %c", &operador);  // Espaço antes de %c para ignorar whitespace
    
    printf("Digite o segundo número: ");
    scanf("%f", &num2);
    
    // Processamento e saída
    printf("\nResultado: ");
    
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
            if (num2 != 0) {
                printf("%.2f / %.2f = %.2f\n", num1, num2, num1 / num2);
            } else {
                printf("Erro: divisão por zero!\n");
            }
            break;
        default:
            printf("Operador inválido!\n");
    }
    
    return 0;
}
```

### Erros Comuns de Iniciantes:
```c
// 1. Esquecer ponto-e-vírgula
int x = 5  // ERRO: falta ;

// 2. Usar = em vez de ==
if (x = 5) { }  // ERRO: atribuição em vez de comparação

// 3. Esquecer & no scanf
scanf("%d", x);  // ERRO: falta &

// 4. Exceder limites do array
char nome[10];
scanf("%s", nome);  // PERIGO: pode estourar buffer

// 5. Usar variável não inicializada
int y;
printf("%d", y);  // ERRO: y não tem valor definido
```

---

## 📝 EXERCÍCIOS PRÁTICOS

### 1. Conversor de Temperatura:
```c
#include <stdio.h>

int main() {
    float celsius, fahrenheit;
    
    printf("Digite a temperatura em Celsius: ");
    scanf("%f", &celsius);
    
    fahrenheit = (celsius * 9/5) + 32;
    
    printf("%.1f°C = %.1f°F\n", celsius, fahrenheit);
    
    return 0;
}
```

### 2. Verificador de Número Primo:
```c
#include <stdio.h>

int main() {
    int numero, i, primo = 1;
    
    printf("Digite um número: ");
    scanf("%d", &numero);
    
    if (numero <= 1) {
        primo = 0;
    } else {
        for (i = 2; i <= numero/2; i++) {
            if (numero % i == 0) {
                primo = 0;
                break;
            }
        }
    }
    
    if (primo) {
        printf("%d é primo!\n", numero);
    } else {
        printf("%d não é primo.\n", numero);
    }
    
    return 0;
}
```

### 3. Calculadora de IMC:
```c
#include <stdio.h>

int main() {
    float peso, altura, imc;
    
    printf("Calculadora de IMC\n");
    printf("Digite seu peso (kg): ");
    scanf("%f", &peso);
    
    printf("Digite sua altura (m): ");
    scanf("%f", &altura);
    
    imc = peso / (altura * altura);
    
    printf("\nSeu IMC: %.1f\n", imc);
    printf("Classificação: ");
    
    if (imc < 18.5) {
        printf("Abaixo do peso\n");
    } else if (imc < 25) {
        printf("Peso normal\n");
    } else if (imc < 30) {
        printf("Sobrepeso\n");
    } else {
        printf("Obesidade\n");
    }
    
    return 0;
}
```

---

## 🎯 RESUMO DAS BOAS PRÁTICAS

1. **Sempre inicialize variáveis**
2. **Use nomes descritivos** para variáveis
3. **Comente seu código** quando necessário
4. **Teste entradas inválidas** em seus programas
5. **Compile com warnings** ativados (`-Wall`)
6. **Divida problemas complexos** em partes menores
7. **Valide dados de entrada** do usuário

---

## 🔗 PRÓXIMOS PASSOS

No **Manual Intermediário** você aprenderá:
- Funções e modularização
- Arrays e strings
- Ponteiros básicos
- Estruturas (struct)
- Alocação dinâmica de memória

---

**📌 DICA FINAL**: A prática é essencial para aprender C. Escreva código todos os dias, teste exemplos, modifique-os e tente criar seus próprios programas!

*Documento atualizado: Fevereiro 2024*  
*Compilador recomendado: GCC (GNU Compiler Collection)*
