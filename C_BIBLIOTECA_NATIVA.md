# Manual Completo das Bibliotecas Nativas da Linguagem C
## Referência em Formato Markdown

---

# Sumário

1. [Introdução](#introdução)
2. [Bibliotecas Padrão C (ISO C)](#bibliotecas-padrão-c-iso-c)
   - [assert.h](#asserth)
   - [ctype.h](#ctypeh)
   - [errno.h](#errnoh)
   - [float.h](#floath)
   - [limits.h](#limitsh)
   - [locale.h](#localeh)
   - [math.h](#mathh)
   - [setjmp.h](#setjmph)
   - [signal.h](#signalh)
   - [stdarg.h](#stdargh)
   - [stddef.h](#stddefh)
   - [stdio.h](#stdioh)
   - [stdlib.h](#stdlibh)
   - [string.h](#stringh)
   - [time.h](#timeh)
3. [Bibliotecas C99](#bibliotecas-c99)
   - [complex.h](#complexh)
   - [fenv.h](#fenvh)
   - [inttypes.h](#inttypesh)
   - [iso646.h](#iso646h)
   - [stdbool.h](#stdboolh)
   - [stdint.h](#stdinth)
   - [tgmath.h](#tgmathh)
   - [wchar.h](#wcharh)
   - [wctype.h](#wctypeh)
4. [Bibliotecas C11](#bibliotecas-c11)
   - [stdalign.h](#stdalignh)
   - [stdatomic.h](#stdatomich)
   - [stdnoreturn.h](#stdnoreturnh)
   - [threads.h](#threadsh)
   - [uchar.h](#ucharh)
5. [Bibliotecas POSIX (Extensões Comuns)](#bibliotecas-posix-extensões-comuns)
   - [unistd.h](#unistdh)
   - [fcntl.h](#fcntlh)
   - [sys/stat.h](#sysstath)
   - [dirent.h](#direnth)
   - [pthread.h](#pthreadh)
   - [signal.h (POSIX)](#signalh-posix)
   - [dlfcn.h](#dlfcnh)
6. [Exemplos Práticos](#exemplos-práticos)

---

# Introdução

Este manual abrange todas as bibliotecas nativas da linguagem C, organizadas por padrões ISO (C89, C99, C11, C17, C23) e extensões POSIX comuns. Cada seção inclui:

- Propósito da biblioteca
- Principais tipos, macros e funções
- Exemplos de código
- Observações importantes

---

# Bibliotecas Padrão C (ISO C)

## assert.h

Fornece macros para diagnóstico em tempo de execução.

**Macro principal:**
```c
void assert(scalar_expression);
```

**Exemplo:**
```c
#include <assert.h>
#include <stdio.h>

int main() {
    int x = 5;
    assert(x == 5);  // OK
    assert(x == 10); // Falha: aborta com mensagem
    return 0;
}
```

**NDEBUG:** Define para desativar todas as asserts.

---

## ctype.h

Classificação e conversão de caracteres.

**Funções de classificação:**
| Função        | Retorna verdadeiro se... |
|---------------|--------------------------|
| `isalnum(c)`  | Alfabético ou dígito     |
| `isalpha(c)`  | Alfabético               |
| `isdigit(c)`  | Dígito decimal (0-9)     |
| `islower(c)`  | Minúsculo                |
| `isupper(c)`  | Maiúsculo                |
| `isspace(c)`  | Espaço em branco         |
| `isxdigit(c)` | Dígito hexadecimal       |
| `isblank(c)`  | Espaço ou tabulação      |
| `iscntrl(c)`  | Caractere de controle    |
| `isgraph(c)`  | Imprimível (não espaço)  |
| `isprint(c)`  | Imprimível (incl. espaço)|
| `ispunct(c)`  | Pontuação                |

**Funções de conversão:**
```c
int tolower(int c);
int toupper(int c);
```

**Exemplo:**
```c
#include <ctype.h>
#include <stdio.h>

int main() {
    char ch = 'A';
    if (isupper(ch))
        putchar(tolower(ch)); // 'a'
    return 0;
}
```

---

## errno.h

Define macros para relatar erros.

**Principais macros:**
```c
errno    // Variável global (thread-local) que armazena código de erro
EDOM     // Erro de domínio matemático
ERANGE   // Resultado fora do intervalo
EILSEQ   // Sequência inválida de bytes (C95)
```

**Exemplo:**
```c
#include <errno.h>
#include <math.h>
#include <stdio.h>

int main() {
    errno = 0;
    double result = sqrt(-1.0);
    if (errno == EDOM) {
        perror("Erro matemático");
    }
    return 0;
}
```

---

## float.h

Define constantes para aritmética de ponto flutuante.

**Macros selecionadas:**
```c
FLT_RADIX      // Base do expoente (2, comum)
FLT_DIG        // Dígitos decimais de precisão (float)
DBL_DIG        // Dígitos decimais de precisão (double)
FLT_EPSILON    // Menor x tal que 1.0 + x ≠ 1.0 (float)
DBL_EPSILON    // Idem para double
FLT_MAX        // Maior float
FLT_MIN        // Menor float normalizado
```

**Exemplo:**
```c
#include <float.h>
#include <stdio.h>

int main() {
    printf("Precisão float: %d dígitos\n", FLT_DIG);
    printf("Epsilon float: %e\n", FLT_EPSILON);
    return 0;
}
```

---

## limits.h

Define limites para tipos inteiros.

**Macros:**
```c
CHAR_BIT    // Bits por byte (8)
SCHAR_MIN   // Menor signed char
SCHAR_MAX   // Maior signed char
UCHAR_MAX   // Maior unsigned char
CHAR_MIN    // Menor char (SCHR_MIN ou 0)
CHAR_MAX    // Maior char
INT_MIN     // Menor int
INT_MAX     // Maior int
UINT_MAX    // Maior unsigned int
LONG_MIN    // Menor long
LONG_MAX    // Maior long
LLONG_MIN   // Menor long long (C99)
LLONG_MAX   // Maior long long (C99)
```

**Exemplo:**
```c
#include <limits.h>
#include <stdio.h>

int main() {
    printf("int: %d a %d\n", INT_MIN, INT_MAX);
    return 0;
}
```

---

## locale.h

Funcionalidades de localização.

**Tipos:**
```c
struct lconv   // Contém informações de formatação numérica/monetária
```

**Funções:**
```c
char *setlocale(int category, const char *locale);
struct lconv *localeconv(void);
```

**Categorias:**
- `LC_ALL`
- `LC_COLLATE`
- `LC_CTYPE`
- `LC_MONETARY`
- `LC_NUMERIC`
- `LC_TIME`

**Exemplo:**
```c
#include <locale.h>
#include <stdio.h>

int main() {
    setlocale(LC_ALL, "pt_BR.UTF-8");
    struct lconv *lc = localeconv();
    printf("Separador decimal: %s\n", lc->decimal_point);
    return 0;
}
```

---

## math.h

Funções matemáticas.

**Constantes:**
```c
M_E      // e
M_LOG2E  // log2(e)
M_LOG10E // log10(e)
M_LN2    // ln(2)
M_LN10   // ln(10)
M_PI     // π
M_PI_2   // π/2
M_PI_4   // π/4
M_1_PI   // 1/π
M_2_PI   // 2/π
M_2_SQRTPI // 2/√π
M_SQRT2  // √2
M_SQRT1_2 // 1/√2
```

**Funções trigonométricas:**
```c
double sin(double x);
double cos(double x);
double tan(double x);
double asin(double x);
double acos(double x);
double atan(double x);
double atan2(double y, double x);
```

**Funções hiperbólicas:**
```c
double sinh(double x);
double cosh(double x);
double tanh(double x);
```

**Funções exponenciais e logarítmicas:**
```c
double exp(double x);
double log(double x);     // Log natural
double log10(double x);
```

**Funções de potência:**
```c
double pow(double x, double y);
double sqrt(double x);
double cbrt(double x);    // C99
double hypot(double x, double y); // C99
```

**Arredondamento e resto:**
```c
double ceil(double x);
double floor(double x);
double fabs(double x);
double fmod(double x, double y);
double remainder(double x, double y); // C99
```

**Funções de erro e gama (C99):**
```c
double erf(double x);
double erfc(double x);
double tgamma(double x);  // Função gama
double lgamma(double x);  // Log da função gama
```

**Exemplo:**
```c
#include <math.h>
#include <stdio.h>

int main() {
    double angulo = M_PI / 4; // 45°
    printf("seno(45°) = %f\n", sin(angulo));
    printf("sqrt(2) = %f\n", sqrt(2.0));
    return 0;
}
```

---

## setjmp.h

Permite desvios não-locais (similar a goto, mas entre funções).

**Tipos:**
```c
jmp_buf   // Buffer para salvar estado
```

**Macros e funções:**
```c
int setjmp(jmp_buf environment);
void longjmp(jmp_buf environment, int value);
```

**Exemplo:**
```c
#include <setjmp.h>
#include <stdio.h>

jmp_buf buf;

void func() {
    printf("Dentro da função\n");
    longjmp(buf, 42); // Retorna para setjmp
}

int main() {
    if (setjmp(buf) == 0) {
        printf("Chamando func\n");
        func();
    } else {
        printf("Retornou de longjmp\n");
    }
    return 0;
}
```

---

## signal.h

Gerencia sinais.

**Tipos:**
```c
sig_atomic_t   // Tipo atômico para manipulação em handlers
```

**Funções:**
```c
void (*signal(int sig, void (*func)(int)))(int);
int raise(int sig);
```

**Sinais padrão:**
```c
SIGABRT  // Abortar
SIGFPE   // Erro aritmético
SIGILL   // Instrução inválida
SIGINT   // Interrupção (Ctrl+C)
SIGSEGV  // Violação de acesso
SIGTERM  // Término
```

**Exemplo:**
```c
#include <signal.h>
#include <stdio.h>

void handler(int sig) {
    printf("Recebido sinal %d\n", sig);
}

int main() {
    signal(SIGINT, handler);
    raise(SIGINT); // Simula Ctrl+C
    return 0;
}
```

---

## stdarg.h

Manipula listas de argumentos variáveis.

**Tipos:**
```c
va_list   // Representa lista de argumentos
```

**Macros:**
```c
va_start(va_list ap, last_named_param)
va_arg(va_list ap, type)
va_copy(va_list dest, va_list src) // C99
va_end(va_list ap)
```

**Exemplo:**
```c
#include <stdarg.h>
#include <stdio.h>

int soma(int count, ...) {
    va_list args;
    va_start(args, count);
    
    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);
    }
    
    va_end(args);
    return total;
}

int main() {
    printf("Soma: %d\n", soma(4, 1, 2, 3, 4)); // 10
    return 0;
}
```

---

## stddef.h

Definições de tipos comuns e macros.

**Tipos:**
```c
ptrdiff_t   // Diferença entre ponteiros
size_t      // Tamanho (unsigned)
wchar_t     // Caractere largo
```

**Macros:**
```c
NULL        // Ponteiro nulo
offsetof(type, member) // Deslocamento de membro
```

**Exemplo:**
```c
#include <stddef.h>
#include <stdio.h>

struct Pessoa {
    char nome[50];
    int idade;
};

int main() {
    printf("Offset de idade: %zu\n", offsetof(struct Pessoa, idade));
    return 0;
}
```

---

## stdio.h

Entrada e saída padrão.

**Tipos:**
```c
FILE        // Stream de arquivo
fpos_t      // Posição em arquivo
```

**Constantes:**
```c
EOF         // Fim de arquivo
BUFSIZ      // Tamanho de buffer
FILENAME_MAX // Tamanho máximo do nome
FOPEN_MAX   // Máx. streams abertos
stdin       // Entrada padrão
stdout      // Saída padrão
stderr      // Erro padrão
```

**Operações com arquivos:**
```c
FILE *fopen(const char *filename, const char *mode);
int fclose(FILE *stream);
int fflush(FILE *stream);
int fseek(FILE *stream, long offset, int whence);
long ftell(FILE *stream);
void rewind(FILE *stream);
int feof(FILE *stream);
int ferror(FILE *stream);
```

**Modos de abertura:**
- `"r"`  - Leitura
- `"w"`  - Escrita (cria/substitui)
- `"a"`  - Append
- `"r+"` - Leitura/escrita (arquivo deve existir)
- `"w+"` - Leitura/escrita (cria/substitui)
- `"a+"` - Leitura/append

**Entrada/saída de caracteres:**
```c
int fgetc(FILE *stream);
char *fgets(char *str, int n, FILE *stream);
int fputc(int c, FILE *stream);
int fputs(const char *str, FILE *stream);
int getchar(void);
char *gets(char *str); // Perigoso! Obsoleto
int putchar(int c);
int puts(const char *str);
```

**Entrada/saída formatada:**
```c
int printf(const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
int sprintf(char *str, const char *format, ...);
int snprintf(char *str, size_t size, const char *format, ...); // C99

int scanf(const char *format, ...);
int fscanf(FILE *stream, const char *format, ...);
int sscanf(const char *str, const char *format, ...);
```

**Entrada/saída binária:**
```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

**Manipulação de arquivos:**
```c
int remove(const char *filename);
int rename(const char *old, const char *new);
FILE *tmpfile(void);
char *tmpnam(char *s);
```

**Exemplo completo:**
```c
#include <stdio.h>

int main() {
    // Escrita
    FILE *arq = fopen("teste.txt", "w");
    if (arq) {
        fprintf(arq, "Olá, mundo!\n");
        fclose(arq);
    }
    
    // Leitura
    arq = fopen("teste.txt", "r");
    if (arq) {
        char buffer[100];
        while (fgets(buffer, sizeof(buffer), arq)) {
            printf("%s", buffer);
        }
        fclose(arq);
    }
    
    return 0;
}
```

---

## stdlib.h

Funções de propósito geral.

**Conversão de strings:**
```c
int atoi(const char *str);
long atol(const char *str);
long long atoll(const char *str); // C99
double atof(const char *str);

long strtol(const char *str, char **endptr, int base);
unsigned long strtoul(const char *str, char **endptr, int base);
long long strtoll(const char *str, char **endptr, int base); // C99
float strtof(const char *str, char **endptr); // C99
double strtod(const char *str, char **endptr);
```

**Geração de números aleatórios:**
```c
int rand(void);
void srand(unsigned int seed);
```

**Gerenciamento de memória:**
```c
void *malloc(size_t size);
void *calloc(size_t nmemb, size_t size);
void *realloc(void *ptr, size_t size);
void free(void *ptr);
```

**Comunicação com o ambiente:**
```c
void abort(void);
int atexit(void (*func)(void));
void exit(int status);
void _Exit(int status); // C99
char *getenv(const char *name);
int system(const char *string);
```

**Pesquisa e ordenação:**
```c
void *bsearch(const void *key, const void *base, size_t nmemb, 
              size_t size, int (*compar)(const void *, const void *));
void qsort(void *base, size_t nmemb, size_t size,
           int (*compar)(const void *, const void *));
```

**Aritmética de inteiros:**
```c
int abs(int n);
long labs(long n);
long long llabs(long long n); // C99
div_t div(int numer, int denom);
ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom); // C99
```

**Exemplo:**
```c
#include <stdlib.h>
#include <stdio.h>

int compar(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}

int main() {
    // Alocação
    int *arr = malloc(5 * sizeof(int));
    if (!arr) return 1;
    
    // Preenchimento
    for (int i = 0; i < 5; i++)
        arr[i] = rand() % 100;
    
    // Ordenação
    qsort(arr, 5, sizeof(int), compar);
    
    // Liberação
    free(arr);
    
    return 0;
}
```

---

## string.h

Manipulação de strings e memória.

**Manipulação de strings:**
```c
char *strcpy(char *dest, const char *src);
char *strncpy(char *dest, const char *src, size_t n);
char *strcat(char *dest, const char *src);
char *strncat(char *dest, const char *src, size_t n);
size_t strlen(const char *s);
int strcmp(const char *s1, const char *s2);
int strncmp(const char *s1, const char *s2, size_t n);
char *strchr(const char *s, int c);
char *strrchr(const char *s, int c);
char *strstr(const char *haystack, const char *needle);
char *strtok(char *str, const char *delim);
```

**Manipulação de memória:**
```c
void *memcpy(void *dest, const void *src, size_t n);
void *memmove(void *dest, const void *src, size_t n);
int memcmp(const void *s1, const void *s2, size_t n);
void *memchr(const void *s, int c, size_t n);
void *memset(void *s, int c, size_t n);
```

**Exemplo:**
```c
#include <string.h>
#include <stdio.h>

int main() {
    char str[50] = "Olá, ";
    char nome[] = "mundo";
    
    strcat(str, nome);
    strcat(str, "!");
    
    printf("%s (%zu caracteres)\n", str, strlen(str));
    
    // Tokenização
    char texto[] = "um,dois,três";
    char *token = strtok(texto, ",");
    while (token) {
        printf("Token: %s\n", token);
        token = strtok(NULL, ",");
    }
    
    return 0;
}
```

---

## time.h

Funções para data e hora.

**Tipos:**
```c
clock_t     // Tempo de processador
time_t      // Tempo calendário
struct tm   // Tempo decomposto
```

**Funções:**
```c
clock_t clock(void);
double difftime(time_t time1, time_t time0);
time_t mktime(struct tm *tm);
time_t time(time_t *t);
char *asctime(const struct tm *tm);
char *ctime(const time_t *timep);
struct tm *gmtime(const time_t *timep);
struct tm *localtime(const time_t *timep);
size_t strftime(char *s, size_t max, const char *format, const struct tm *tm);
```

**Exemplo:**
```c
#include <time.h>
#include <stdio.h>

int main() {
    time_t agora = time(NULL);
    struct tm *local = localtime(&agora);
    
    printf("Data/Hora: %s", asctime(local));
    
    // Formatação personalizada
    char buffer[80];
    strftime(buffer, sizeof(buffer), 
             "%d/%m/%Y %H:%M:%S", local);
    printf("Formatado: %s\n", buffer);
    
    return 0;
}
```

---

# Bibliotecas C99

## complex.h

Suporte para números complexos.

**Macros:**
```c
complex     // _Complex
imaginary   // _Imaginary (opcional)
I           // Unidade imaginária (raiz de -1)
```

**Funções:**
```c
double complex cabs(double complex z);
double complex cacos(double complex z);
double complex cacosh(double complex z);
double complex carg(double complex z);
double complex casin(double complex z);
double complex casinh(double complex z);
double complex catan(double complex z);
double complex catanh(double complex z);
double complex ccos(double complex z);
double complex ccosh(double complex z);
double complex cexp(double complex z);
double complex cimag(double complex z);
double complex clog(double complex z);
double complex conj(double complex z);
double complex cpow(double complex x, double complex y);
double complex cproj(double complex z);
double complex creal(double complex z);
double complex csin(double complex z);
double complex csinh(double complex z);
double complex csqrt(double complex z);
double complex ctan(double complex z);
double complex ctanh(double complex z);
```

**Exemplo:**
```c
#include <complex.h>
#include <stdio.h>

int main() {
    double complex z = 1.0 + 2.0*I;
    double complex w = cexp(z);
    
    printf("z = %.1f + %.1fi\n", creal(z), cimag(z));
    printf("exp(z) = %.1f + %.1fi\n", creal(w), cimag(w));
    
    return 0;
}
```

---

## fenv.h

Controle do ambiente de ponto flutuante.

**Tipos:**
```c
fenv_t     // Ambiente completo
fexcept_t  // Flags de exceção
```

**Exceções:**
```c
FE_DIVBYZERO
FE_INEXACT
FE_INVALID
FE_OVERFLOW
FE_UNDERFLOW
FE_ALL_EXCEPT
```

**Funções:**
```c
int feclearexcept(int excepts);
int fegetexceptflag(fexcept_t *flagp, int excepts);
int feraiseexcept(int excepts);
int fesetexceptflag(const fexcept_t *flagp, int excepts);
int fetestexcept(int excepts);

int fegetround(void);
int fesetround(int round);

int fegetenv(fenv_t *envp);
int fesetenv(const fenv_t *envp);
```

---

## inttypes.h

Formatação estendida para tipos inteiros.

**Tipos:**
```c
int8_t, int16_t, int32_t, int64_t
uint8_t, uint16_t, uint32_t, uint64_t
int_least8_t, etc.
int_fast8_t, etc.
intmax_t, uintmax_t
intptr_t, uintptr_t
```

**Macros para printf/scanf:**
```c
PRId8, PRIi8, PRIu8, PRIx8, etc.
SCNd8, SCNi8, SCNu8, SCNx8, etc.
```

**Funções:**
```c
intmax_t strtoimax(const char *nptr, char **endptr, int base);
uintmax_t strtoumax(const char *nptr, char **endptr, int base);
```

**Exemplo:**
```c
#include <inttypes.h>
#include <stdio.h>

int main() {
    int64_t grande = 12345678901234LL;
    printf("Valor: %" PRId64 "\n", grande);
    
    return 0;
}
```

---

## iso646.h

Nomes alternativos para operadores.

**Macros:**
```c
and     &&   and_eq   &= 
bitand  &    bitor    | 
compl   ~    not      ! 
not_eq  !=   or       || 
or_eq   |=   xor      ^ 
xor_eq  ^=
```

**Exemplo:**
```c
#include <iso646.h>
#include <stdio.h>

int main() {
    int a = 5, b = 7;
    
    if (a < 10 and b > 5) {
        printf("Condição verdadeira\n");
    }
    
    return 0;
}
```

---

## stdbool.h

Tipo booleano.

**Macros:**
```c
bool     // _Bool
true     // 1
false    // 0
__bool_true_false_are_defined // 1
```

**Exemplo:**
```c
#include <stdbool.h>
#include <stdio.h>

int main() {
    bool ativo = true;
    
    if (ativo) {
        printf("Ativo\n");
    }
    
    return 0;
}
```

---

## stdint.h

Tipos inteiros com tamanho fixo.

**Tipos:**
```c
int8_t, int16_t, int32_t, int64_t
uint8_t, uint16_t, uint32_t, uint64_t

int_least8_t, int_least16_t, int_least32_t, int_least64_t
uint_least8_t, etc.

int_fast8_t, int_fast16_t, int_fast32_t, int_fast64_t
uint_fast8_t, etc.

intptr_t, uintptr_t    // Ponteiro inteiro
intmax_t, uintmax_t    // Maior inteiro
```

**Constantes:**
```c
INT8_MIN, INT16_MIN, INT32_MIN, INT64_MIN
INT8_MAX, INT16_MAX, INT32_MAX, INT64_MAX
UINT8_MAX, UINT16_MAX, UINT32_MAX, UINT64_MAX
INTMAX_MIN, INTMAX_MAX
UINTMAX_MAX
PTRDIFF_MIN, PTRDIFF_MAX
SIZE_MAX
```

**Macros para constantes:**
```c
INT8_C(value), INT16_C(value), INT32_C(value), INT64_C(value)
UINT8_C(value), UINT16_C(value), UINT32_C(value), UINT64_C(value)
INTMAX_C(value), UINTMAX_C(value)
```

---

## tgmath.h

Macros de matemática tipo-genéricas (type-generic math).

Seleciona automaticamente a versão correta (`float`, `double`, `long double`, `complex`) baseada nos argumentos.

**Exemplo:**
```c
#include <tgmath.h>
#include <stdio.h>

int main() {
    double x = 2.0;
    float y = 4.0f;
    long double z = 8.0L;
    
    // sqrt seleciona sqrt, sqrtf, sqrtl automaticamente
    printf("%f\n", sqrt(x));
    printf("%f\n", sqrt(y));
    printf("%Lf\n", sqrt(z));
    
    return 0;
}
```

---

## wchar.h

Suporte para caracteres largos (multibyte).

**Tipos:**
```c
wchar_t    // Caractere largo
wint_t     // Inteiro capaz de armazenar wchar_t e WEOF
```

**Funções:**
```c
// Entrada/saída
int fwprintf(FILE *stream, const wchar_t *format, ...);
int fwscanf(FILE *stream, const wchar_t *format, ...);
int wprintf(const wchar_t *format, ...);
int wscanf(const wchar_t *format, ...);

// Manipulação de strings
wchar_t *wcscpy(wchar_t *dest, const wchar_t *src);
wchar_t *wcsncpy(wchar_t *dest, const wchar_t *src, size_t n);
wchar_t *wcscat(wchar_t *dest, const wchar_t *src);
int wcscmp(const wchar_t *s1, const wchar_t *s2);
size_t wcslen(const wchar_t *s);
```

---

## wctype.h

Classificação de caracteres largos.

**Funções:**
```c
int iswalnum(wint_t wc);
int iswalpha(wint_t wc);
int iswblank(wint_t wc);
int iswcntrl(wint_t wc);
int iswdigit(wint_t wc);
int iswgraph(wint_t wc);
int iswlower(wint_t wc);
int iswprint(wint_t wc);
int iswpunct(wint_t wc);
int iswspace(wint_t wc);
int iswupper(wint_t wc);
int iswxdigit(wint_t wc);
wint_t towlower(wint_t wc);
wint_t towupper(wint_t wc);
```

---

# Bibliotecas C11

## stdalign.h

Suporte para alinhamento.

**Macros:**
```c
alignas    // _Alignas
alignof    // _Alignof
__alignas_is_defined // 1
__alignof_is_defined // 1
```

**Exemplo:**
```c
#include <stdalign.h>
#include <stdio.h>

struct alignas(16) Alinhado {
    char c;
    int i;
};

int main() {
    printf("Alinhamento: %zu\n", alignof(struct Alinhado));
    return 0;
}
```

---

## stdatomic.h

Operações atômicas para programação concorrente.

**Tipos:**
```c
atomic_bool
atomic_char
atomic_int
atomic_uint
atomic_long
atomic_ulong
atomic_llong
atomic_ullong
atomic_size_t
atomic_intptr_t
```

**Funções:**
```c
void atomic_init(atomic_int *obj, int value);
int atomic_load(atomic_int *obj);
void atomic_store(atomic_int *obj, int desired);
int atomic_exchange(atomic_int *obj, int desired);
_Bool atomic_compare_exchange_strong(atomic_int *obj, int *expected, int desired);
```

**Exemplo:**
```c
#include <stdatomic.h>
#include <stdio.h>
#include <threads.h>

atomic_int contador = 0;

int thread_func(void *arg) {
    for (int i = 0; i < 1000; i++) {
        atomic_fetch_add(&contador, 1);
    }
    return 0;
}
```

---

## stdnoreturn.h

Macro para funções que não retornam.

**Macro:**
```c
noreturn   // _Noreturn
```

**Exemplo:**
```c
#include <stdnoreturn.h>
#include <stdlib.h>

noreturn void fatal_error(void) {
    exit(1);
}
```

---

## threads.h

Suporte para threads (C11).

**Tipos:**
```c
thrd_t      // Identificador de thread
mtx_t       // Mutex
cnd_t       // Variável de condição
tss_t       // Armazenamento específico de thread
thrd_start_t // Função de entrada da thread
```

**Funções:**
```c
// Threads
int thrd_create(thrd_t *thr, thrd_start_t func, void *arg);
int thrd_equal(thrd_t thr0, thrd_t thr1);
thrd_t thrd_current(void);
int thrd_sleep(const struct timespec *duration, struct timespec *remaining);
void thrd_yield(void);
_Noreturn void thrd_exit(int res);
int thrd_join(thrd_t thr, int *res);
int thrd_detach(thrd_t thr);

// Mutex
int mtx_init(mtx_t *mtx, int type);
void mtx_destroy(mtx_t *mtx);
int mtx_lock(mtx_t *mtx);
int mtx_timedlock(mtx_t *mtx, const struct timespec *tp);
int mtx_trylock(mtx_t *mtx);
int mtx_unlock(mtx_t *mtx);

// Variáveis de condição
int cnd_init(cnd_t *cond);
void cnd_destroy(cnd_t *cond);
int cnd_signal(cnd_t *cond);
int cnd_broadcast(cnd_t *cond);
int cnd_wait(cnd_t *cond, mtx_t *mtx);
int cnd_timedwait(cnd_t *cond, mtx_t *mtx, const struct timespec *tp);
```

---

## uchar.h

Caracteres Unicode (UTF-16/UTF-32).

**Tipos:**
```c
char16_t   // Caractere UTF-16
char32_t   // Caractere UTF-32
```

**Funções:**
```c
size_t c16rtomb(char *s, char16_t c16, mbstate_t *ps);
size_t c32rtomb(char *s, char32_t c32, mbstate_t *ps);
size_t mbrtoc16(char16_t *pc16, const char *s, size_t n, mbstate_t *ps);
size_t mbrtoc32(char32_t *pc32, const char *s, size_t n, mbstate_t *ps);
```

---

# Bibliotecas POSIX (Extensões Comuns)

## unistd.h

Constantes e funções POSIX.

**Constantes:**
```c
STDIN_FILENO  // 0
STDOUT_FILENO // 1
STDERR_FILENO // 2
```

**Funções:**
```c
// E/S de baixo nível
ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count);
int close(int fd);

// Controle de processo
pid_t fork(void);
int execvp(const char *file, char *const argv[]);
void _exit(int status);
pid_t getpid(void);
pid_t getppid(void);

// Arquivos e diretórios
int unlink(const char *pathname);
int rmdir(const char *pathname);
char *getcwd(char *buf, size_t size);
int chdir(const char *path);
```

**Exemplo:**
```c
#include <unistd.h>
#include <stdio.h>

int main() {
    char cwd[256];
    if (getcwd(cwd, sizeof(cwd))) {
        printf("Diretório atual: %s\n", cwd);
    }
    return 0;
}
```

---

## fcntl.h

Controle de descritores de arquivo.

**Funções:**
```c
int open(const char *pathname, int flags, mode_t mode);
int fcntl(int fd, int cmd, ...);
```

**Flags comuns:**
```c
O_RDONLY   // Somente leitura
O_WRONLY   // Somente escrita
O_RDWR     // Leitura e escrita
O_CREAT    // Criar se não existir
O_EXCL     // Com O_CREAT, falha se existir
O_TRUNC    // Truncar
O_APPEND   // Append
```

---

## sys/stat.h

Status de arquivos.

**Estruturas:**
```c
struct stat {
    dev_t     st_dev;     // Dispositivo
    ino_t     st_ino;     // Número de inode
    mode_t    st_mode;    // Modo/proteção
    nlink_t   st_nlink;   // Número de links
    uid_t     st_uid;     // Proprietário
    gid_t     st_gid;     // Grupo
    off_t     st_size;    // Tamanho em bytes
    time_t    st_atime;   // Último acesso
    time_t    st_mtime;   // Última modificação
    time_t    st_ctime;   // Última mudança de status
};
```

**Funções:**
```c
int stat(const char *pathname, struct stat *statbuf);
int fstat(int fd, struct stat *statbuf);
int lstat(const char *pathname, struct stat *statbuf);
int chmod(const char *pathname, mode_t mode);
int mkdir(const char *pathname, mode_t mode);
```

---

## dirent.h

Leitura de diretórios.

**Tipos:**
```c
DIR              // Stream de diretório
struct dirent    // Entrada de diretório
```

**Funções:**
```c
DIR *opendir(const char *name);
struct dirent *readdir(DIR *dirp);
int closedir(DIR *dirp);
void rewinddir(DIR *dirp);
```

**Exemplo:**
```c
#include <dirent.h>
#include <stdio.h>

int main() {
    DIR *dir = opendir(".");
    if (dir) {
        struct dirent *entry;
        while ((entry = readdir(dir)) != NULL) {
            printf("%s\n", entry->d_name);
        }
        closedir(dir);
    }
    return 0;
}
```

---

## pthread.h

Threads POSIX.

**Tipos:**
```c
pthread_t       // Identificador de thread
pthread_attr_t  // Atributos
pthread_mutex_t // Mutex
pthread_cond_t  // Variável de condição
```

**Funções:**
```c
// Threads
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                   void *(*start_routine) (void *), void *arg);
int pthread_join(pthread_t thread, void **retval);
void pthread_exit(void *retval);
pthread_t pthread_self(void);

// Mutex
int pthread_mutex_init(pthread_mutex_t *mutex, 
                       const pthread_mutexattr_t *attr);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
int pthread_mutex_destroy(pthread_mutex_t *mutex);

// Condição
int pthread_cond_init(pthread_cond_t *cond, 
                      const pthread_condattr_t *attr);
int pthread_cond_wait(pthread_cond_t *cond, 
                      pthread_mutex_t *mutex);
int pthread_cond_signal(pthread_cond_t *cond);
int pthread_cond_broadcast(pthread_cond_t *cond);
```

---

## dlfcn.h

Carregamento dinâmico de bibliotecas.

**Funções:**
```c
void *dlopen(const char *filename, int flag);
void *dlsym(void *handle, const char *symbol);
int dlclose(void *handle);
char *dlerror(void);
```

**Flags:**
```c
RTLD_LAZY   // Resolve símbolos quando usados
RTLD_NOW    // Resolve todos símbolos imediatamente
RTLD_GLOBAL // Símbolos disponíveis para outras libs
```

**Exemplo:**
```c
#include <dlfcn.h>
#include <stdio.h>

int main() {
    void *handle = dlopen("libm.so", RTLD_LAZY);
    if (handle) {
        double (*sqrt_func)(double) = dlsym(handle, "sqrt");
        if (sqrt_func) {
            printf("sqrt(4) = %f\n", sqrt_func(4.0));
        }
        dlclose(handle);
    }
    return 0;
}
```

---

# Exemplos Práticos

## Exemplo 1: Combinando stdio, string e stdlib

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_LINHA 256

int main() {
    FILE *arq = fopen("dados.txt", "r");
    if (!arq) {
        perror("Erro ao abrir arquivo");
        return 1;
    }
    
    char linha[MAX_LINHA];
    int *numeros = NULL;
    int count = 0;
    
    while (fgets(linha, sizeof(linha), arq)) {
        // Remover newline
        linha[strcspn(linha, "\n")] = 0;
        
        // Expandir array
        numeros = realloc(numeros, (count + 1) * sizeof(int));
        if (!numeros) {
            fclose(arq);
            return 1;
        }
        
        // Converter string para int
        numeros[count++] = atoi(linha);
    }
    
    fclose(arq);
    
    // Exibir números lidos
    for (int i = 0; i < count; i++) {
        printf("%d ", numeros[i]);
    }
    printf("\n");
    
    free(numeros);
    return 0;
}
```

## Exemplo 2: Threads POSIX com sincronização

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
int contador_compartilhado = 0;

void* incrementar(void *arg) {
    for (int i = 0; i < 1000000; i++) {
        pthread_mutex_lock(&mutex);
        contador_compartilhado++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    
    pthread_create(&t1, NULL, incrementar, NULL);
    pthread_create(&t2, NULL, incrementar, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    printf("Contador: %d\n", contador_compartilhado);
    pthread_mutex_destroy(&mutex);
    
    return 0;
}
```

## Exemplo 3: Números complexos e matemática

```c
#include <complex.h>
#include <math.h>
#include <stdio.h>

int main() {
    double complex z = 1.0 + 1.0 * I;
    double complex w = cpow(z, 2.0);  // z²
    
    printf("z = %.2f %+.2fi\n", creal(z), cimag(z));
    printf("z² = %.2f %+.2fi\n", creal(w), cimag(w));
    
    double modulo = cabs(z);
    double angulo = carg(z);  // Em radianos
    
    printf("Módulo: %.2f\n", modulo);
    printf("Ângulo: %.2f rad (%.1f°)\n", angulo, angulo * 180 / M_PI);
    
    return 0;
}
```

## Exemplo 4: Data e hora com time.h

```c
#include <time.h>
#include <stdio.h>

double medir_tempo(void (*func)()) {
    clock_t inicio = clock();
    func();
    clock_t fim = clock();
    
    return ((double)(fim - inicio)) / CLOCKS_PER_SEC;
}

void operacao_demorada() {
    volatile int soma = 0;
    for (int i = 0; i < 10000000; i++) {
        soma += i;
    }
}

int main() {
    double tempo = medir_tempo(operacao_demorada);
    printf("Tempo decorrido: %.3f segundos\n", tempo);
    
    // Data e hora atual
    time_t t = time(NULL);
    printf("Timestamp: %ld\n", t);
    printf("Data/hora: %s", ctime(&t));
    
    return 0;
}
```

---

# Referências

1. ISO/IEC 9899:2018 - Linguagem C (C17)
2. POSIX.1-2017 - IEEE Std 1003.1
3. cppreference.com
4. Manual GNU C Library

---

*Este manual é um documento vivo e pode ser atualizado com novos padrões e extensões.*
