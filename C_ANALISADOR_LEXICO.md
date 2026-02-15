# Manual Completo: Como Criar um Analisador Léxico em C

## Índice
1. [Introdução](#introdução)
2. [Conceitos Fundamentais](#conceitos-fundamentais)
3. [Planejamento do Analisador](#planejamento-do-analisador)
4. [Implementação Passo a Passo](#implementação-passo-a-passo)
5. [Código Fonte Completo](#código-fonte-completo)
6. [Exemplos de Uso](#exemplos-de-uso)
7. [Testes e Depuração](#testes-e-depuração)
8. [Melhorias e Otimizações](#melhorias-e-otimizações)

## Introdução

Um analisador léxico (lexer) é o primeiro componente de um compilador ou interpretador. Sua função é converter uma sequência de caracteres (código fonte) em uma sequência de tokens - unidades significativas que serão processadas pelo analisador sintático.

### O que é um Token?
Um token é uma estrutura que contém:
- **Tipo**: classificação do token (ex: PALAVRA_CHAVE, IDENTIFICADOR, NUMERO)
- **Lexema**: a string original que gerou o token
- **Linha/Coluna**: localização no código fonte (opcional, mas útil para mensagens de erro)

## Conceitos Fundamentais

### Tipos Comuns de Tokens
```c
typedef enum {
    TOKEN_PALAVRA_CHAVE,    // if, else, while, etc.
    TOKEN_IDENTIFICADOR,     // nomes de variáveis/funções
    TOKEN_NUMERO,           // números inteiros/reais
    TOKEN_STRING,           // strings entre aspas
    TOKEN_OPERADOR,         // +, -, *, /, =, etc.
    TOKEN_DELIMITADOR,      // ;, ,, ., etc.
    TOKEN_PARENTESE_ESQ,    // (
    TOKEN_PARENTESE_DIR,    // )
    TOKEN_CHAVE_ESQ,        // {
    TOKEN_CHAVE_DIR,        // }
    TOKEN_FIM_ARQUIVO,      // EOF
    TOKEN_ERRO              // token inválido
} TipoToken;
```

### Autômato do Analisador Léxico
O analisador funciona como um autômato finito determinístico (AFD) que percorre o texto caractere por caractere, mudando de estado conforme o que encontra.

## Planejamento do Analisador

### 1. Definição da Linguagem
Vamos criar um analisador para uma linguagem simples com:
- Palavras-chave: if, else, while, return, int, float, char
- Operadores: +, -, *, /, =, ==, !=, <, >, <=, >=
- Delimitadores: ; , . ( ) { }
- Identificadores: letras seguidas de letras/dígitos
- Números: inteiros e decimais
- Strings: entre aspas duplas
- Comentários: // para linha única

### 2. Estrutura de Dados
```c
typedef struct {
    TipoToken tipo;
    char lexema[100];
    int linha;
    int coluna;
} Token;

typedef struct {
    FILE* arquivo;
    char caractere_atual;
    int linha;
    int coluna;
    int tem_caractere;
} AnalisadorLexico;
```

## Implementação Passo a Passo

### Passo 1: Inicialização do Analisador

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <stdbool.h>

void inicializar_analisador(AnalisadorLexico* lexer, const char* nome_arquivo) {
    lexer->arquivo = fopen(nome_arquivo, "r");
    if (!lexer->arquivo) {
        printf("Erro ao abrir arquivo: %s\n", nome_arquivo);
        exit(1);
    }
    
    lexer->linha = 1;
    lexer->coluna = 0;
    lexer->tem_caractere = true;
    
    // Ler primeiro caractere
    avancar_caractere(lexer);
}
```

### Passo 2: Função para Avançar Caractere

```c
void avancar_caractere(AnalisadorLexico* lexer) {
    if (lexer->tem_caractere) {
        lexer->caractere_atual = fgetc(lexer->arquivo);
        lexer->coluna++;
        
        if (lexer->caractere_atual == '\n') {
            lexer->linha++;
            lexer->coluna = 0;
        }
        
        if (lexer->caractere_atual == EOF) {
            lexer->tem_caractere = false;
        }
    }
}
```

### Passo 3: Pular Espaços em Branco

```c
void ignorar_espacos(AnalisadorLexico* lexer) {
    while (lexer->tem_caractere && 
           (lexer->caractere_atual == ' ' || 
            lexer->caractere_atual == '\t' || 
            lexer->caractere_atual == '\n' ||
            lexer->caractere_atual == '\r')) {
        avancar_caractere(lexer);
    }
}
```

### Passo 4: Processar Identificadores e Palavras-chave

```c
Token processar_identificador(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    
    // Palavras-chave da linguagem
    const char* palavras_chave[] = {
        "if", "else", "while", "return", "int", "float", "char", NULL
    };
    
    // Coletar o identificador
    while (lexer->tem_caractere && 
           (isalnum(lexer->caractere_atual) || lexer->caractere_atual == '_')) {
        if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        avancar_caractere(lexer);
    }
    token.lexema[i] = '\0';
    
    // Verificar se é palavra-chave
    token.tipo = TOKEN_IDENTIFICADOR;
    for (int j = 0; palavras_chave[j] != NULL; j++) {
        if (strcmp(token.lexema, palavras_chave[j]) == 0) {
            token.tipo = TOKEN_PALAVRA_CHAVE;
            break;
        }
    }
    
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}
```

### Passo 5: Processar Números

```c
Token processar_numero(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    bool tem_ponto = false;
    
    while (lexer->tem_caractere) {
        if (isdigit(lexer->caractere_atual)) {
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        else if (lexer->caractere_atual == '.' && !tem_ponto) {
            tem_ponto = true;
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        else {
            break;
        }
        avancar_caractere(lexer);
    }
    
    token.lexema[i] = '\0';
    token.tipo = TOKEN_NUMERO;
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}
```

### Passo 6: Processar Strings

```c
Token processar_string(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    
    avancar_caractere(lexer); // Pular a aspa inicial
    
    while (lexer->tem_caractere && lexer->caractere_atual != '"') {
        if (lexer->caractere_atual == '\\') {
            // Processar escape sequence
            avancar_caractere(lexer);
            switch (lexer->caractere_atual) {
                case 'n': token.lexema[i++] = '\n'; break;
                case 't': token.lexema[i++] = '\t'; break;
                case '"': token.lexema[i++] = '"'; break;
                case '\\': token.lexema[i++] = '\\'; break;
                default: token.lexema[i++] = lexer->caractere_atual;
            }
        }
        else {
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        avancar_caractere(lexer);
    }
    
    if (lexer->caractere_atual == '"') {
        avancar_caractere(lexer); // Pular aspa final
    }
    
    token.lexema[i] = '\0';
    token.tipo = TOKEN_STRING;
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}
```

### Passo 7: Processar Operadores e Delimitadores

```c
Token processar_operador_ou_delimitador(AnalisadorLexico* lexer) {
    Token token;
    token.lexema[0] = lexer->caractere_atual;
    token.lexema[1] = '\0';
    token.linha = lexer->linha;
    token.coluna = lexer->coluna;
    
    switch (lexer->caractere_atual) {
        case '+': token.tipo = TOKEN_OPERADOR; break;
        case '-': token.tipo = TOKEN_OPERADOR; break;
        case '*': token.tipo = TOKEN_OPERADOR; break;
        case '/': 
            // Verificar se é comentário
            avancar_caractere(lexer);
            if (lexer->caractere_atual == '/') {
                // Comentário de linha - ignorar até o final
                while (lexer->tem_caractere && lexer->caractere_atual != '\n') {
                    avancar_caractere(lexer);
                }
                return proximo_token(lexer); // Recursão para próximo token
            }
            else {
                token.tipo = TOKEN_OPERADOR;
                return token;
            }
            break;
            
        case '=':
        case '!':
        case '<':
        case '>':
            // Verificar operadores de dois caracteres
            avancar_caractere(lexer);
            if (lexer->caractere_atual == '=') {
                token.lexema[1] = '=';
                token.lexema[2] = '\0';
                avancar_caractere(lexer);
            }
            token.tipo = TOKEN_OPERADOR;
            return token;
            
        case ';': token.tipo = TOKEN_DELIMITADOR; break;
        case ',': token.tipo = TOKEN_DELIMITADOR; break;
        case '.': token.tipo = TOKEN_DELIMITADOR; break;
        case '(': token.tipo = TOKEN_PARENTESE_ESQ; break;
        case ')': token.tipo = TOKEN_PARENTESE_DIR; break;
        case '{': token.tipo = TOKEN_CHAVE_ESQ; break;
        case '}': token.tipo = TOKEN_CHAVE_DIR; break;
        
        default: token.tipo = TOKEN_ERRO;
    }
    
    avancar_caractere(lexer);
    return token;
}
```

### Passo 8: Função Principal de Análise

```c
Token proximo_token(AnalisadorLexico* lexer) {
    ignorar_espacos(lexer);
    
    if (!lexer->tem_caractere) {
        Token token;
        token.tipo = TOKEN_FIM_ARQUIVO;
        strcpy(token.lexema, "EOF");
        token.linha = lexer->linha;
        token.coluna = lexer->coluna;
        return token;
    }
    
    if (isalpha(lexer->caractere_atual) || lexer->caractere_atual == '_') {
        return processar_identificador(lexer);
    }
    
    if (isdigit(lexer->caractere_atual)) {
        return processar_numero(lexer);
    }
    
    if (lexer->caractere_atual == '"') {
        return processar_string(lexer);
    }
    
    return processar_operador_ou_delimitador(lexer);
}
```

## Código Fonte Completo

Aqui está o código completo do analisador léxico:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <stdbool.h>

// Definição dos tipos de token
typedef enum {
    TOKEN_PALAVRA_CHAVE,
    TOKEN_IDENTIFICADOR,
    TOKEN_NUMERO,
    TOKEN_STRING,
    TOKEN_OPERADOR,
    TOKEN_DELIMITADOR,
    TOKEN_PARENTESE_ESQ,
    TOKEN_PARENTESE_DIR,
    TOKEN_CHAVE_ESQ,
    TOKEN_CHAVE_DIR,
    TOKEN_FIM_ARQUIVO,
    TOKEN_ERRO
} TipoToken;

// Estrutura do token
typedef struct {
    TipoToken tipo;
    char lexema[100];
    int linha;
    int coluna;
} Token;

// Estrutura do analisador
typedef struct {
    FILE* arquivo;
    char caractere_atual;
    int linha;
    int coluna;
    int tem_caractere;
} AnalisadorLexico;

// Nomes dos tokens para impressão
const char* nome_token[] = {
    "PALAVRA_CHAVE",
    "IDENTIFICADOR",
    "NUMERO",
    "STRING",
    "OPERADOR",
    "DELIMITADOR",
    "PARENTESE_ESQ",
    "PARENTESE_DIR",
    "CHAVE_ESQ",
    "CHAVE_DIR",
    "FIM_ARQUIVO",
    "ERRO"
};

// Protótipos das funções
void inicializar_analisador(AnalisadorLexico* lexer, const char* nome_arquivo);
void avancar_caractere(AnalisadorLexico* lexer);
void ignorar_espacos(AnalisadorLexico* lexer);
Token proximo_token(AnalisadorLexico* lexer);
Token processar_identificador(AnalisadorLexico* lexer);
Token processar_numero(AnalisadorLexico* lexer);
Token processar_string(AnalisadorLexico* lexer);
Token processar_operador_ou_delimitador(AnalisadorLexico* lexer);
void imprimir_token(Token token);
void fechar_analisador(AnalisadorLexico* lexer);

// Implementação das funções
void inicializar_analisador(AnalisadorLexico* lexer, const char* nome_arquivo) {
    lexer->arquivo = fopen(nome_arquivo, "r");
    if (!lexer->arquivo) {
        printf("Erro ao abrir arquivo: %s\n", nome_arquivo);
        exit(1);
    }
    
    lexer->linha = 1;
    lexer->coluna = 0;
    lexer->tem_caractere = true;
    avancar_caractere(lexer);
}

void avancar_caractere(AnalisadorLexico* lexer) {
    if (lexer->tem_caractere) {
        lexer->caractere_atual = fgetc(lexer->arquivo);
        lexer->coluna++;
        
        if (lexer->caractere_atual == '\n') {
            lexer->linha++;
            lexer->coluna = 0;
        }
        
        if (lexer->caractere_atual == EOF) {
            lexer->tem_caractere = false;
        }
    }
}

void ignorar_espacos(AnalisadorLexico* lexer) {
    while (lexer->tem_caractere && 
           (lexer->caractere_atual == ' ' || 
            lexer->caractere_atual == '\t' || 
            lexer->caractere_atual == '\n' ||
            lexer->caractere_atual == '\r')) {
        avancar_caractere(lexer);
    }
}

Token processar_identificador(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    
    const char* palavras_chave[] = {
        "if", "else", "while", "return", "int", "float", "char", NULL
    };
    
    while (lexer->tem_caractere && 
           (isalnum(lexer->caractere_atual) || lexer->caractere_atual == '_')) {
        if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        avancar_caractere(lexer);
    }
    token.lexema[i] = '\0';
    
    token.tipo = TOKEN_IDENTIFICADOR;
    for (int j = 0; palavras_chave[j] != NULL; j++) {
        if (strcmp(token.lexema, palavras_chave[j]) == 0) {
            token.tipo = TOKEN_PALAVRA_CHAVE;
            break;
        }
    }
    
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}

Token processar_numero(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    bool tem_ponto = false;
    
    while (lexer->tem_caractere) {
        if (isdigit(lexer->caractere_atual)) {
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        else if (lexer->caractere_atual == '.' && !tem_ponto) {
            tem_ponto = true;
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        else {
            break;
        }
        avancar_caractere(lexer);
    }
    
    token.lexema[i] = '\0';
    token.tipo = TOKEN_NUMERO;
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}

Token processar_string(AnalisadorLexico* lexer) {
    Token token;
    int i = 0;
    int linha_inicio = lexer->linha;
    int coluna_inicio = lexer->coluna;
    
    avancar_caractere(lexer); // Pular a aspa inicial
    
    while (lexer->tem_caractere && lexer->caractere_atual != '"') {
        if (lexer->caractere_atual == '\\') {
            avancar_caractere(lexer);
            switch (lexer->caractere_atual) {
                case 'n': token.lexema[i++] = '\n'; break;
                case 't': token.lexema[i++] = '\t'; break;
                case '"': token.lexema[i++] = '"'; break;
                case '\\': token.lexema[i++] = '\\'; break;
                default: token.lexema[i++] = lexer->caractere_atual;
            }
        }
        else {
            if (i < 99) token.lexema[i++] = lexer->caractere_atual;
        }
        avancar_caractere(lexer);
    }
    
    if (lexer->caractere_atual == '"') {
        avancar_caractere(lexer); // Pular aspa final
    }
    
    token.lexema[i] = '\0';
    token.tipo = TOKEN_STRING;
    token.linha = linha_inicio;
    token.coluna = coluna_inicio;
    
    return token;
}

Token processar_operador_ou_delimitador(AnalisadorLexico* lexer) {
    Token token;
    token.lexema[0] = lexer->caractere_atual;
    token.lexema[1] = '\0';
    token.linha = lexer->linha;
    token.coluna = lexer->coluna;
    
    switch (lexer->caractere_atual) {
        case '+': token.tipo = TOKEN_OPERADOR; break;
        case '-': token.tipo = TOKEN_OPERADOR; break;
        case '*': token.tipo = TOKEN_OPERADOR; break;
        case '/': 
            avancar_caractere(lexer);
            if (lexer->caractere_atual == '/') {
                while (lexer->tem_caractere && lexer->caractere_atual != '\n') {
                    avancar_caractere(lexer);
                }
                return proximo_token(lexer);
            }
            else {
                token.tipo = TOKEN_OPERADOR;
                return token;
            }
            break;
            
        case '=':
        case '!':
        case '<':
        case '>':
            avancar_caractere(lexer);
            if (lexer->caractere_atual == '=') {
                token.lexema[1] = '=';
                token.lexema[2] = '\0';
                avancar_caractere(lexer);
            }
            token.tipo = TOKEN_OPERADOR;
            return token;
            
        case ';': token.tipo = TOKEN_DELIMITADOR; break;
        case ',': token.tipo = TOKEN_DELIMITADOR; break;
        case '.': token.tipo = TOKEN_DELIMITADOR; break;
        case '(': token.tipo = TOKEN_PARENTESE_ESQ; break;
        case ')': token.tipo = TOKEN_PARENTESE_DIR; break;
        case '{': token.tipo = TOKEN_CHAVE_ESQ; break;
        case '}': token.tipo = TOKEN_CHAVE_DIR; break;
        
        default: token.tipo = TOKEN_ERRO;
    }
    
    avancar_caractere(lexer);
    return token;
}

Token proximo_token(AnalisadorLexico* lexer) {
    ignorar_espacos(lexer);
    
    if (!lexer->tem_caractere) {
        Token token;
        token.tipo = TOKEN_FIM_ARQUIVO;
        strcpy(token.lexema, "EOF");
        token.linha = lexer->linha;
        token.coluna = lexer->coluna;
        return token;
    }
    
    if (isalpha(lexer->caractere_atual) || lexer->caractere_atual == '_') {
        return processar_identificador(lexer);
    }
    
    if (isdigit(lexer->caractere_atual)) {
        return processar_numero(lexer);
    }
    
    if (lexer->caractere_atual == '"') {
        return processar_string(lexer);
    }
    
    return processar_operador_ou_delimitador(lexer);
}

void imprimir_token(Token token) {
    printf("[%s] '%s' (linha %d, coluna %d)\n", 
           nome_token[token.tipo], 
           token.lexema, 
           token.linha, 
           token.coluna);
}

void fechar_analisador(AnalisadorLexico* lexer) {
    if (lexer->arquivo) {
        fclose(lexer->arquivo);
    }
}

// Função principal para teste
int main(int argc, char* argv[]) {
    if (argc < 2) {
        printf("Uso: %s <arquivo_fonte>\n", argv[0]);
        return 1;
    }
    
    AnalisadorLexico lexer;
    inicializar_analisador(&lexer, argv[1]);
    
    Token token;
    do {
        token = proximo_token(&lexer);
        imprimir_token(token);
    } while (token.tipo != TOKEN_FIM_ARQUIVO && token.tipo != TOKEN_ERRO);
    
    fechar_analisador(&lexer);
    
    return 0;
}
```

## Exemplos de Uso

### Exemplo 1: Arquivo de teste simples
Crie um arquivo `teste.txt`:

```c
int main() {
    int x = 42;
    float y = 3.14;
    if (x > y) {
        return x;
    }
    // isto é um comentário
    return 0;
}
```

### Executando o analisador:
```bash
gcc -o lexer lexer.c
./lexer teste.txt
```

### Saída esperada:
```
[PALAVRA_CHAVE] 'int' (linha 1, coluna 1)
[IDENTIFICADOR] 'main' (linha 1, coluna 5)
[PARENTESE_ESQ] '(' (linha 1, coluna 9)
[PARENTESE_DIR] ')' (linha 1, coluna 10)
[CHAVE_ESQ] '{' (linha 1, coluna 12)
[PALAVRA_CHAVE] 'int' (linha 2, coluna 5)
[IDENTIFICADOR] 'x' (linha 2, coluna 9)
[OPERADOR] '=' (linha 2, coluna 11)
[NUMERO] '42' (linha 2, coluna 13)
[DELIMITADOR] ';' (linha 2, coluna 15)
[PALAVRA_CHAVE] 'float' (linha 3, coluna 5)
[IDENTIFICADOR] 'y' (linha 3, coluna 11)
[OPERADOR] '=' (linha 3, coluna 13)
[NUMERO] '3.14' (linha 3, coluna 15)
[DELIMITADOR] ';' (linha 3, coluna 19)
[PALAVRA_CHAVE] 'if' (linha 4, coluna 5)
[PARENTESE_ESQ] '(' (linha 4, coluna 8)
[IDENTIFICADOR] 'x' (linha 4, coluna 9)
[OPERADOR] '>' (linha 4, coluna 11)
[IDENTIFICADOR] 'y' (linha 4, coluna 13)
[PARENTESE_DIR] ')' (linha 4, coluna 14)
[CHAVE_ESQ] '{' (linha 4, coluna 16)
[PALAVRA_CHAVE] 'return' (linha 5, coluna 9)
[IDENTIFICADOR] 'x' (linha 5, coluna 16)
[DELIMITADOR] ';' (linha 5, coluna 17)
[CHAVE_DIR] '}' (linha 6, coluna 5)
[PALAVRA_CHAVE] 'return' (linha 8, coluna 5)
[NUMERO] '0' (linha 8, coluna 12)
[DELIMITADOR] ';' (linha 8, coluna 13)
[CHAVE_DIR] '}' (linha 9, coluna 1)
[FIM_ARQUIVO] 'EOF' (linha 9, coluna 1)
```

## Testes e Depuração

### Casos de Teste Importantes

1. **Teste de palavras-chave**
```
if else while return int float char
```

2. **Teste de identificadores**
```
variavel _privado var123 nome_com_under
```

3. **Teste de números**
```
42 3.14 .5 0.0 123
```

4. **Teste de strings**
```
"hello world" "escape \n \t \" \\"
```

5. **Teste de operadores**
```
+ - * / = == != < > <= >=
```

6. **Teste de comentários**
```
// comentário de linha
int x = 5; // comentário após código
```

7. **Teste de erros**
```
@ $ # 123abc
```

### Função de Depuração

Adicione esta função para depuração detalhada:

```c
void debug_lexer(AnalisadorLexico* lexer) {
    printf("DEBUG: Linha=%d, Coluna=%d, Caractere='%c' (ASCII %d)\n",
           lexer->linha, 
           lexer->coluna, 
           lexer->caractere_atual,
           lexer->caractere_atual);
}
```

## Melhorias e Otimizações

### 1. Tabela de Símbolos
```c
typedef struct {
    char* nome;
    TipoToken tipo;
    int uso;
} EntradaTabela;

typedef struct {
    EntradaTabela* entradas;
    int capacidade;
    int tamanho;
} TabelaSimbolos;
```

### 2. Buffer para Lexemas
Em vez de array fixo de 100 caracteres, use alocação dinâmica:

```c
typedef struct {
    TipoToken tipo;
    char* lexema;
    int linha;
    int coluna;
} TokenMelhorado;
```

### 3. Suporte a Unicode
Para processar caracteres Unicode, considere usar `wchar_t`:

```c
#include <wchar.h>
#include <wctype.h>

typedef struct {
    FILE* arquivo;
    wint_t caractere_atual;
    // ...
} AnalisadorLexicoUnicode;
```

### 4. Múltiplos Arquivos
Organize o código em múltiplos arquivos:
- `lexer.h` - definições e protótipos
- `lexer.c` - implementação
- `main.c` - função principal
- `tabela_simbolos.h/c` - gerenciamento da tabela de símbolos

### 5. Tratamento de Erros Robusto
```c
typedef enum {
    ERRO_NENHUM,
    ERRO_CARACTERE_INVALIDO,
    ERRO_STRING_NAO_FECHADA,
    ERRO_COMENTARIO_NAO_FECHADO,
    ERRO_NUMERO_MAL_FORMADO
} CodigoErro;

typedef struct {
    CodigoErro codigo;
    char mensagem[256];
    int linha;
    int coluna;
} ErroLexico;
```

