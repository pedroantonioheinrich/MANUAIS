# Manual Completo de Portugol

## Sumário
1. [Introdução ao Portugol](#introdução-ao-portugol)
2. [Estrutura Básica de um Programa](#estrutura-básica-de-um-programa)
3. [Elementos Básicos](#elementos-básicos)
4. [Variáveis e Declarações](#variáveis-e-declarações)
5. [Operadores](#operadores)
6. [Entrada e Saída](#entrada-e-saída)
7. [Estruturas Condicionais](#estruturas-condicionais)
8. [Estruturas de Repetição](#estruturas-de-repetição)
9. [Vetores e Matrizes](#vetores-e-matrizes)
10. [Cadeias de Caracteres (Strings)](#cadeias-de-caracteres-strings)
11. [Funções e Procedimentos](#funções-e-procedimentos)
12. [Registros (Structs)](#registros-structs)
13. [Arquivos](#arquivos)
14. [Boas Práticas](#boas-práticas)
15. [Exemplos Completos](#exemplos-completos)
16. [Referências e Recursos](#referências-e-recursos)

---

## Introdução ao Portugol

O **Portugol** é uma pseudolinguagem de programação desenvolvida para o ensino de lógica de programação em português. Sua sintaxe é inspirada em linguagens como Pascal e C, mas com palavras‑chave em português, facilitando o aprendizado para iniciantes.

Existem várias implementações de ambientes que interpretam/executam código Portugol, sendo as mais conhecidas:
- **Visualg** (gratuito, tradicional)
- **Portugol Studio** (da UNIVALI, mais moderno, com recursos adicionais)
- **G-Portugol** (baseado em web)

Este manual utiliza a sintaxe comum adotada pela maioria dessas ferramentas. Quando houver diferenças importantes, faremos observações.

---

## Estrutura Básica de um Programa

Um programa em Portugol é dividido em três partes principais:

1. **Declaração do programa** (opcional em algumas implementações)
2. **Declaração de variáveis**
3. **Corpo do programa** (bloco `inicio` … `fim`)

```portugol
programa NomeDoPrograma
var
   // declaração de variáveis globais
inicio
   // comandos
fim
```

- `programa` pode ser omitido em alguns ambientes (ex.: Visualg permite iniciar diretamente com `algoritmo`).
- O bloco `var` é opcional se não houver variáveis.
- `inicio` e `fim` delimitam o código executável.

**Exemplo mínimo:**
```portugol
programa OlaMundo
inicio
   escreva("Olá, mundo!")
fim
```

---

## Elementos Básicos

### Comentários
- Comentário de linha: `//` (tudo após é ignorado até o fim da linha)
- Comentário de bloco: `/* … */` (pode ocupar várias linhas)

```portugol
// isto é um comentário
escreva("Exemplo") /* comentário
                       de múltiplas linhas */
```

### Identificadores
- Nomes dados a variáveis, constantes, funções, etc.
- Regras:
  - Devem começar com letra (a–z, A–Z) ou sublinhado `_`
  - Os caracteres seguintes podem ser letras, dígitos (0–9) ou sublinhado
  - Não podem conter espaços ou caracteres especiais (acentos são permitidos em algumas implementações, mas evite‑os para portabilidade)
  - Não podem ser iguais a palavras reservadas

### Palavras Reservadas
Algumas palavras reservadas comuns (podem variar conforme o ambiente):

`programa`, `algoritmo`, `var`, `inicio`, `fim`, `inteiro`, `real`, `caractere`, `logico`, `const`, `se`, `entao`, `senao`, `fimse`, `escolha`, `caso`, `outrocaso`, `fimescolha`, `enquanto`, `faca`, `fimenquanto`, `para`, `ate`, `passo`, `fimpara`, `repita`, `ate`, `funcao`, `procedimento`, `retorne`, `registro`, `fimregistro`, `verdadeiro`, `falso`, `e`, `ou`, `nao`, `leia`, `escreva`, `escreval`

### Tipos de Dados Primitivos

| Tipo       | Descrição                                    | Exemplo                 |
|------------|----------------------------------------------|-------------------------|
| `inteiro`  | Números inteiros (positivos, negativos, zero) | `idade <- 25`          |
| `real`     | Números com ponto flutuante (decimais)       | `altura <- 1.75`       |
| `caractere`| Um único caractere ou uma cadeia (string)    | `nome <- "Maria"`      |
| `logico`   | Valores lógicos: `verdadeiro` ou `falso`     | `aprovado <- verdadeiro`|

**Observação:** Em Portugol, `caractere` tanto pode armazenar um único caractere (`'A'`) quanto uma string (`"texto"`). A forma de representar literais de caractere geralmente é com aspas duplas. Alguns ambientes aceitam aspas simples para um caractere.

### Constantes
Constantes são valores que não podem ser alterados durante a execução. São declaradas com a palavra `const`.

```portugol
const
   PI = 3.1415
   MAXIMO = 100
```

---

## Variáveis e Declarações

### Sintaxe de Declaração
No bloco `var`, lista‑se o nome da variável seguido de dois‑pontos e o tipo.

```portugol
var
   idade : inteiro
   salario : real
   nome : caractere
   ativo : logico
```

Também é possível declarar múltiplas variáveis do mesmo tipo separadas por vírgula:

```portugol
var
   a, b, c : inteiro
   x, y : real
```

### Atribuição
Utiliza‑se o operador `<-` (seta para a esquerda) para atribuir um valor a uma variável.

```portugol
idade <- 18
nome <- "João"
soma <- a + b
```

### Escopo de Variáveis
- **Variáveis globais:** declaradas no bloco `var` principal, fora de qualquer função/procedimento, são acessíveis em todo o programa.
- **Variáveis locais:** declaradas dentro de funções ou procedimentos, só existem dentro do bloco onde foram criadas.

---

## Operadores

### Aritméticos
| Operador | Significado | Exemplo |
|----------|-------------|---------|
| `+`      | Adição      | `a + b` |
| `-`      | Subtração   | `a - b` |
| `*`      | Multiplicação | `a * b` |
| `/`      | Divisão real | `a / b` |
| `%`      | Resto da divisão inteira (módulo) | `a % b` |

### Relacionais (comparação)
| Operador | Significado       | Exemplo        |
|----------|-------------------|----------------|
| `=`      | Igual a           | `a = b`        |
| `<>`     | Diferente de      | `a <> b`       |
| `>`      | Maior que         | `a > b`        |
| `<`      | Menor que         | `a < b`        |
| `>=`     | Maior ou igual a  | `a >= b`       |
| `<=`     | Menor ou igual a  | `a <= b`       |

### Lógicos
| Operador | Significado | Exemplo                     |
|----------|-------------|-----------------------------|
| `e`      | E lógico (AND) | `(a > 0) e (a < 10)`      |
| `ou`     | OU lógico (OR) | `(a = 0) ou (b = 0)`       |
| `nao`    | NÃO lógico (NOT) | `nao (aprovado)`          |

### Concatenação de Strings
Para juntar textos, usa‑se o operador `+` (em algumas implementações, `&`). O Portugol Studio aceita `+` para concatenar strings.

```portugol
nomeCompleto <- "João" + " " + "Silva"
```

---

## Entrada e Saída

### Saída
- `escreva(valor1, valor2, ...)` – exibe os valores na tela, um após o outro, sem quebra de linha ao final.
- `escreval(valor1, valor2, ...)` – exibe os valores e depois salta para a próxima linha.

```portugol
escreva("Idade: ", idade)
escreval("Nome: ", nome)
```

### Entrada
- `leia(variavel)` – lê um valor digitado pelo usuário e armazena na variável.

```portugol
leia(idade)
leia(nome)
```

**Atenção:** A leitura de números deve ser compatível com o tipo da variável. Se o usuário digitar um valor inválido, pode ocorrer erro.

---

## Estruturas Condicionais

### se…então…senao…fimse
Executa um bloco de comandos se uma condição for verdadeira; opcionalmente, executa outro bloco se for falsa.

```portugol
se (media >= 6) entao
   escreval("Aprovado")
senao
   escreval("Reprovado")
fimse
```

**Observação:** A condição deve ser uma expressão lógica. O uso de parênteses em torno da condição é opcional, mas recomendado para clareza.

### escolha…caso…outrocaso…fimescolha
Equivalente ao `switch` de outras linguagens. Seleciona um bloco a executar com base no valor de uma expressão.

```portugol
escolha (opcao)
   caso 1:
        escreval("Opção 1 escolhida")
   caso 2:
        escreval("Opção 2 escolhida")
   caso 3, 4:   // múltiplos valores (alguns ambientes)
        escreval("Opção 3 ou 4")
   outrocaso:
        escreval("Opção inválida")
fimescolha
```

A sintaxe de múltiplos valores separados por vírgula (`caso 3, 4`) não é padrão em todos os ambientes; em alguns, é necessário escrever um `caso` para cada valor. Verifique a documentação do seu ambiente.

---

## Estruturas de Repetição

### enquanto…faca…fimenquanto
Executa um bloco repetidamente **enquanto** uma condição for verdadeira. A condição é testada antes de cada iteração.

```portugol
enquanto (contador <= 10) faca
   escreval(contador)
   contador <- contador + 1
fimenquanto
```

### para…de…ate…[passo]…faca…fimpara
Repete um bloco um número determinado de vezes, controlando automaticamente uma variável de contagem.

```portugol
para i de 1 ate 10 faca
   escreval(i)
fimpara
```

Com incremento diferente de 1:
```portugol
para i de 10 ate 1 passo -1 faca
   escreval(i)
fimpara
```

- `passo` é opcional; se omitido, assume‑se 1.
- O valor final é **inclusivo** em todas as implementações? Em algumas, `para i de 1 ate 10` executa com i = 1,2,...,10. Verifique.

### repita…ate
Executa um bloco pelo menos uma vez e repete **até** que uma condição se torne verdadeira. A condição é testada ao final.

```portugol
repita
   escreva("Digite um número positivo: ")
   leia(num)
ate (num > 0)
```

**Atenção:** Em alguns ambientes, a condição é testada como "repita até que a condição seja verdadeira", ou seja, o loop para quando a condição for verdadeira.

---

## Vetores e Matrizes

### Vetores (arrays unidimensionais)
Declaração:
```portugol
var
   notas : vetor[1..10] de real   // índices de 1 a 10
   nomes : vetor[1..5] de caractere
```

Acesso aos elementos:
```portugol
notas[1] <- 8.5
escreva(nomes[3])
```

**Observação:** Em algumas implementações, os índices começam em 0. O Portugol Studio geralmente usa índices iniciando em 1. Verifique a documentação do seu ambiente.

### Matrizes (arrays bidimensionais)
Declaração:
```portugol
var
   matriz : vetor[1..3, 1..4] de inteiro   // 3 linhas, 4 colunas
```

Acesso:
```portugol
matriz[2, 3] <- 10
escreva(matriz[linha, coluna])
```

Também é possível declarar como `vetor[1..3][1..4]` dependendo da sintaxe.

### Exemplo de uso de vetor
```portugol
programa ExemploVetor
var
   numeros : vetor[1..5] de inteiro
   i : inteiro
inicio
   para i de 1 ate 5 faca
        escreva("Digite o número ", i, ": ")
        leia(numeros[i])
   fimpara
   escreval("Números lidos:")
   para i de 1 ate 5 faca
        escreva(numeros[i], " ")
   fimpara
fim
```

---

## Cadeias de Caracteres (Strings)

Em Portugol, strings são tratadas como valores do tipo `caractere`. As operações disponíveis dependem do ambiente. As mais comuns:

### Declaração
```portugol
var
   texto : caractere
   nome : caractere
```

### Concatenação
```portugol
nomeCompleto <- nome + " " + sobrenome
```

### Comprimento da string
Em alguns ambientes, há funções como `comprimento()` ou `tamanho()`.

```portugol
tam <- comprimento(nome)   // retorna número de caracteres
```

### Acesso a caracteres individuais
Em alguns ambientes, é possível acessar como vetor:
```portugol
primeiraLetra <- nome[1]   // se índices começam em 1
```

### Substrings
Função `subcadeia(texto, inicio, tamanho)` ou similar.
```portugol
parte <- subcadeia("Programação", 1, 4)   // "Prog"
```

### Comparação de strings
Usa‑se os operadores relacionais normalmente. A ordem lexicográfica depende da tabela de caracteres utilizada.

**Exemplo:**
```portugol
se nome = "João" entao
   escreval("Acertou!")
fimse
```

---

## Funções e Procedimentos

Funções retornam um valor; procedimentos apenas executam ações.

### Função
Sintaxe:
```portugol
funcao nome(parametros) : tipo_retorno
var
   // variáveis locais (opcional)
inicio
   // comandos
   retorne valor
fimfuncao
```

Exemplo:
```portugol
funcao somar(a, b : inteiro) : inteiro
inicio
   retorne a + b
fimfuncao
```

### Procedimento
Sintaxe:
```portugol
procedimento nome(parametros)
var
   // variáveis locais
inicio
   // comandos
fimprocedimento
```

Exemplo:
```portugol
procedimento saudacao(nome : caractere)
inicio
   escreval("Olá, ", nome, "!")
fimprocedimento
```

### Parâmetros
Por padrão, os parâmetros são passados **por valor** (a função/procedimento trabalha com uma cópia). Para passagem por referência (permitindo alterar a variável original), alguns ambientes usam a palavra `var` antes do parâmetro.

```portugol
procedimento trocar(var a, b : inteiro)
var
   aux : inteiro
inicio
   aux <- a
   a <- b
   b <- aux
fimprocedimento
```

### Chamada
```portugol
resultado <- somar(5, 3)
saudacao("Maria")
trocar(x, y)
```

### Escopo
Variáveis declaradas dentro da função/procedimento são locais. As globais podem ser acessadas (mas cuidado com efeitos colaterais).

---

## Registros (Structs)

Registros permitem agrupar variáveis de tipos diferentes em uma única estrutura.

### Definição de um tipo registro
```portugol
registro Aluno
   nome : caractere
   idade : inteiro
   nota : real
fimregistro
```

### Declaração de variáveis do tipo
```portugol
var
   aluno1, aluno2 : Aluno
   turma : vetor[1..30] de Aluno
```

### Acesso aos campos
Usa‑se o ponto:
```portugol
aluno1.nome <- "Carlos"
aluno1.idade <- 20
aluno1.nota <- 8.5

escreva(aluno1.nome, " tem ", aluno1.idade, " anos")
```

---

## Arquivos

Alguns ambientes (como Portugol Studio) oferecem suporte básico a arquivos. As operações comuns são:

### Abrir arquivo
```portugol
arq <- abrir_arquivo("dados.txt", "leitura")   // modos: "leitura", "escrita", "anexar"
```

### Ler linha
```portugol
linha <- ler_linha(arq)
```

### Escrever linha
```portugol
escrever_linha(arq, "texto a gravar")
```

### Fechar arquivo
```portugol
fechar_arquivo(arq)
```

### Exemplo (gravação)
```portugol
programa GravaArquivo
var
   arq : arquivo
   i : inteiro
inicio
   arq <- abrir_arquivo("numeros.txt", "escrita")
   para i de 1 ate 10 faca
        escrever_linha(arq, i)
   fimpara
   fechar_arquivo(arq)
   escreval("Arquivo gravado.")
fim
```

### Exemplo (leitura)
```portugol
programa LeArquivo
var
   arq : arquivo
   linha : caractere
inicio
   arq <- abrir_arquivo("numeros.txt", "leitura")
   enquanto nao fim_do_arquivo(arq) faca
        linha <- ler_linha(arq)
        escreval("Linha: ", linha)
   fimenquanto
   fechar_arquivo(arq)
fim
```

**Nota:** A sintaxe exata pode variar. Consulte a ajuda do seu ambiente.

---

## Boas Práticas

1. **Nomes significativos** – Escolha identificadores que reflitam o propósito da variável ou função.
2. **Indentação consistente** – Use espaços ou tabulações para alinhar blocos, facilitando a leitura.
3. **Comentários úteis** – Comente o "porquê", não o "o quê" (o código já mostra o que faz).
4. **Evite variáveis globais** – Prefira passar parâmetros para funções/procedimentos.
5. **Valide entradas do usuário** – Sempre que possível, verifique se os dados lidos são válidos.
6. **Quebre problemas grandes em funções menores** – Facilita testes e manutenção.
7. **Mantenha consistência no uso de maiúsculas/minúsculas** – Embora Portugol geralmente não seja case‑sensitive, a clareza é importante.

---

## Exemplos Completos

### 1. Cálculo de média
```portugol
programa Media
var
   n1, n2, n3, media : real
inicio
   escreva("Digite a primeira nota: ")
   leia(n1)
   escreva("Digite a segunda nota: ")
   leia(n2)
   escreva("Digite a terceira nota: ")
   leia(n3)
   media <- (n1 + n2 + n3) / 3
   escreval("Média: ", media:2:2)   // formatação com 2 casas decimais (alguns ambientes)
   se media >= 7 entao
        escreval("Aprovado")
   senao
        escreval("Reprovado")
   fimse
fim
```

### 2. Fatorial (função recursiva)
```portugol
programa Fatorial
funcao fatorial(n : inteiro) : inteiro
inicio
   se n <= 1 entao
        retorne 1
   senao
        retorne n * fatorial(n - 1)
   fimse
fimfuncao

var
   num : inteiro
inicio
   escreva("Digite um número: ")
   leia(num)
   escreval("Fatorial de ", num, " = ", fatorial(num))
fim
```

### 3. Ordenação por seleção (vetor)
```portugol
programa Ordenacao
var
   vet : vetor[1..10] de inteiro
   i, j, min, aux : inteiro
inicio
   // leitura
   para i de 1 ate 10 faca
        escreva("Digite o valor ", i, ": ")
        leia(vet[i])
   fimpara

   // ordenação (selection sort)
   para i de 1 ate 9 faca
        min <- i
        para j de i+1 ate 10 faca
             se vet[j] < vet[min] entao
                  min <- j
             fimse
        fimpara
        aux <- vet[i]
        vet[i] <- vet[min]
        vet[min] <- aux
   fimpara

   // exibição
   escreval("Vetor ordenado:")
   para i de 1 ate 10 faca
        escreva(vet[i], " ")
   fimpara
fim
```

### 4. Cadastro simples com registro
```portugol
programa Cadastro
registro Pessoa
   nome : caractere
   idade : inteiro
   altura : real
fimregistro

var
   pessoa : Pessoa
inicio
   escreva("Nome: ")
   leia(pessoa.nome)
   escreva("Idade: ")
   leia(pessoa.idade)
   escreva("Altura (m): ")
   leia(pessoa.altura)

   escreval("Dados cadastrados:")
   escreval("Nome: ", pessoa.nome)
   escreval("Idade: ", pessoa.idade)
   escreval("Altura: ", pessoa.altura:1:2, " m")
fim
```

---

## Referências e Recursos

- **Portugol Studio** – [http://lite.acad.univali.br/portugol/](http://lite.acad.univali.br/portugol/)
- **Visualg** – [http://visualg3.com.br/](http://visualg3.com.br/) (ou buscar no site do autor)
- **Apostila de Lógica de Programação com Portugol** – diversos materiais disponíveis online gratuitamente.
- **Fórum** – tire dúvidas em comunidades como Stack Overflow em português ou grupos de programação.

---

Este manual cobriu os principais aspectos da linguagem Portugol. Lembre‑se de que cada ambiente pode ter pequenas variações na sintaxe ou nas funções disponíveis. Consulte sempre a documentação da ferramenta que você está utilizando. Pratique bastante e bons estudos!
