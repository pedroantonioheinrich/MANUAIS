Bem-vindo ao mundo do COBOL! Se você está começando agora, pode achar que o COBOL (Common Business Oriented Language) parece estranho por ser tão "verboso" (parece inglês escrito), mas pense nele como uma receita de bolo extremamente bem detalhada. Ele foi feito para negócios: processar arquivos, relatórios e cálculos financeiros com precisão absoluta.

Aqui está o seu **Manual Completo de COBOL para Iniciantes**.

---

# 📘 Manual de COBOL: Do Zero ao Primeiro Programa

## 1. A Filosofia do COBOL
* **Para que serve:** Sistemas bancários, seguros, governamentais e corporativos (mainframes).
* **Características:** Focado em dados (não em interfaces gráficas), lida perfeitamente com casas decimais (dinheiro) e é extremamente estável.
* **Curiosidade:** Estima-se que cerca de 70% a 80% das transações financeiras do mundo ainda passam por COBOL.

---

## 2. As 4 Regras de Ouro da Sintaxe

Antes de escrever código, você precisa saber como o COBOL se comporta:

1. **Colunas (O Formato Fixo):** No COBOL tradicional, o código é dividido em áreas na mesma linha:
   * **Colunas 1 a 6:** Número de sequência (opcional hoje em dia).
   * **Coluna 7:** Indicador. Se tiver um asterisco (`*`), a linha inteira é um comentário.
   * **Colunas 8 a 11:** **Área A**. Onde começam as DIVISÕES, SEÇÕES e Parágrafos principais.
   * **Colunas 12 a 72:** **Área B**. Onde escrevemos o código mesmo (comandos, variáveis).
   * *Nota: Hoje em dia, usamos muito o "Free Format" (formato livre) que ignora essas colunas, mas é vital saber que elas existem.*
2. **O Ponto Final (`.`):** No COBOL, o ponto final encerra um bloco de lógica. Esquecer um ponto causa erros bizarros. (Dica moderna: use pontos apenas no final dos parágrafos).
3. **Case Insensitive:** `MOVE` é a mesma coisa que `move` ou `MoVe`. Por padrão, escrevemos em MAIÚSCULAS.
4. **Nomenclatura:** Nomes de variáveis podem ter até 30 caracteres, hífens (`-`) são permitidos, mas não podem começar ou terminar com hífen.

---

## 3. A Estrutura de um Programa: As 4 Divisões

Todo programa COBOL é como um livro com 4 capítulos obrigatórios, chamados de **DIVISÕES**. Elas devem aparecer exatamente nesta ordem:

### DIVISÃO 1: IDENTIFICATION DIVISION (Quem é você?)
É o cartão de visitas do programa. Não tem lógica.
```cobol
       IDENTIFICATION DIVISION.
       PROGRAM-ID. MEU-PRIMEIRO-PROGRAMA.
       AUTHOR. SEU-NOME.
```

### DIVISÃO 2: ENVIRONMENT DIVISION (Onde você roda?)
Diz ao computador em qual máquina o programa vai rodar e quais arquivos ele vai ler/escrever. Para iniciantes, fica quase vazia.
```cobol
       ENVIRONMENT DIVISION.
       CONFIGURATION SECTION.
       SPECIAL-NAMES.
           DECIMAL-POINT IS COMMA. * Faz 1.000,00 ao invés de 1,000.00
```

### DIVISÃO 3: DATA DIVISION (O que você vai usar?)
É o "dicionário" do programa. Aqui você cria todas as variáveis que vai usar. É a parte mais importante para um bom programador COBOL.
```cobol
       DATA DIVISION.
       WORKING-STORAGE SECTION.
       * Aqui vão as variáveis (explicado detalhadamente abaixo)
```

### DIVISÃO 4: PROCEDURE DIVISION (O que você vai fazer?)
Aqui fica a lógica de fato: ler dados, calcular, mostrar na tela.

---

## 4. Dominando a DATA DIVISION (Variáveis e o PIC)

No COBOL, não dizemos que uma variável é "Inteiro" ou "Texto". Nós desenhamos ela usando a cláusula **PIC** (Picture).

### Níveis (Hierarchy)
O COBOL organiza variáveis como uma árvore genealógica.
* **Nível 01:** É a "raiz" (o sobrenome da família).
* **Níveis 02 a 49:** São os "filhos" (os detalhes).
* **Nível 88:** São variáveis de condição (tipo booleanos).

Exemplo prático:
```cobol
       01  WS-CLIENTE.
           05  WS-NOME           PIC X(20).
           05  WS-IDADE          PIC 9(03).
           05  WS-ATIVO          PIC X(01).
               88  CLIENTE-ATIVO     VALUE 'S'.
               88  CLIENTE-INATIVO   VALUE 'N'.
```
*Nota: O `WS` no início é uma convenção (Working Storage) para dizer que a variável é local.*

### A Cláusula PIC (Os desenhos)
* `X` = Letra ou Número (Texto).
* `9` = Apenas Número.
* `V` = Vírgula decimal **invisível** (não aparece na tela, só na memória para cálculos).
* `Z` = Número que some os zeros à esquerda.
* `.` = Ponto decimal real (para mostrar na tela).

**Exemplos de PICs:**
```cobol
01 WS-TEXTO          PIC X(30) VALUE "OLÁ MUNDO".
01 WS-TELEFONE       PIC X(11) VALUE "11999999999".
01 WS-IDADE          PIC 9(03) VALUE 025.
01 WS-SALARIO        PIC 9(06)V99 VALUE 150000. * Guarda 1500.00 na memória
01 WS-SALARIO-TELA   PIC $ZZZ.ZZ9,99.          * Mostra na tela: $ 1.500,00
```
*(Dica: `9(03)` é a mesma coisa que `999`)*

### O que é FILLER?
Se você tiver um texto misturado com variáveis, pode usar o FILLER para preencher espaço:
```cobol
01 WS-LINHA-RELATORIO.
   05 FILLER          PIC X(10) VALUE "Nome: ".
   05 WS-NOME-CLI     PIC X(20).
   05 FILLER          PIC X(05) VALUE " Id: ".
   05 WS-ID-CLI       PIC 9(05).
```

---

## 5. Dominando a PROCEDURE DIVISION (Comandos)

### Movendo Dados (`MOVE`)
No COBOL, você não usa `=` para atribuir valores, usa o `MOVE`.
```cobol
MOVE 100 TO WS-IDADE.
MOVE "JOAO" TO WS-NOME.
```

### Mostrando na tela (`DISPLAY`)
```cobol
DISPLAY "BEM VINDO AO COBOL".
DISPLAY WS-NOME.
DISPLAY "Sua idade é: " WS-IDADE.
```

### Recebendo dados do teclado (`ACCEPT`)
```cobol
DISPLAY "Digite seu nome: "
ACCEPT WS-NOME.
```

### Matemática Básica
Você pode usar verbos específicos ou o `COMPUTE` (que aceita fórmulas matemáticas normais).
```cobol
ADD 10 TO WS-IDADE.
SUBTRACT 5 FROM WS-IDADE.
MULTIPLY WS-SALARIO BY 2.
DIVIDE WS-SALARIO BY 12 GIVING WS-MENSAL.
* Ou simplesmente:
COMPUTE WS-MENSAL = WS-SALARIO / 12.
```

---

## 6. Controle de Fluxo (IF e EVALUATE)

### O IF (SE)
```cobol
IF WS-IDADE >= 18
    DISPLAY "MAIOR DE IDADE"
ELSE
    DISPLAY "MENOR DE IDADE"
END-IF
```
*Dica de Ouro Moderna: Sempre use `END-IF`, `END-PERFORM` etc. Isso evita o uso excessivo de pontos finais que confundem iniciantes.*

### O EVALUATE (O Switch/Case do COBOL)
É muito mais elegante que vários `IF` encadeados.
```cobol
EVALUATE WS-OPCAO
    WHEN 1
        DISPLAY "VOCE ESCOLHEU OPCAO 1"
    WHEN 2
        DISPLAY "VOCE ESCOLHEU OPCAO 2"
    WHEN OTHER
        DISPLAY "OPCAO INVALIDA"
END-EVALUATE
```

---

## 7. Laços de Repetição (PERFORM)

O COBOL não tem `FOR` ou `WHILE` tradicionais. Ele usa o `PERFORM`.

### Executando um bloco de código (Como uma função)
```cobol
       PROCEDURE DIVISION.
           PERFORM INICIALIZAR.
           PERFORM PROCESSAR.
           STOP RUN.

       INICIALIZAR.
           MOVE ZEROS TO WS-IDADE.
           DISPLAY "SISTEMA INICIALIZADO".

       PROCESSAR.
           DISPLAY "PROCESSANDO...".
```

### Laços (Loops)
```cobol
* Repete 10 vezes
PERFORM VARYING WS-CONTADOR FROM 1 BY 1
    UNTIL WS-CONTADOR > 10
    DISPLAY WS-CONTADOR
END-PERFORM

* Repete enquanto a condição for verdadeira (While)
PERFORM UNTIL WS-OPCAO = 0
    ACCEPT WS-OPCAO
    DISPLAY WS-OPCAO
END-PERFORM
```

---

## 8. Juntando Tudo: O Programa Completo

Aqui está um programa completo que soma dois números. Observe a identificação (Área A e Área B).

```cobol
      ******************************************************************
      * PROGRAMA: SOMA-DOIS-NUMEROS
      * OBJETIVO : LER DOIS NUMEROS E MOSTRAR A SOMA NA TELA
      ******************************************************************
       IDENTIFICATION DIVISION.
       PROGRAM-ID. SOMA-DOIS-NUMEROS.

       ENVIRONMENT DIVISION.
       CONFIGURATION SECTION.
       SPECIAL-NAMES.
           DECIMAL-POINT IS COMMA.

       DATA DIVISION.
       WORKING-STORAGE SECTION.
       01  WS-NUMERO-1         PIC 9(04) VALUE ZEROS.
       01  WS-NUMERO-2         PIC 9(04) VALUE ZEROS.
       01  WS-RESULTADO        PIC 9(05) VALUE ZEROS.

       PROCEDURE DIVISION.
       PRINCIPAL.
           DISPLAY "DIGITE O PRIMEIRO NUMERO: "
           ACCEPT WS-NUMERO-1

           DISPLAY "DIGITE O SEGUNDO NUMERO: "
           ACCEPT WS-NUMERO-2

           PERFORM CALCULAR-SOMA

           DISPLAY "O RESULTADO DA SOMA E: " WS-RESULTADO

           STOP RUN.

       CALCULAR-SOMA.
           ADD WS-NUMERO-1 WS-NUMERO-2 GIVING WS-RESULTADO.
```

---

## 9. Como Testar isso Hoje? (Guia Prático)

Você não precisa de um Mainframe da IBM para aprender COBOL! Siga estes passos no seu Windows, Mac ou Linux:

1. **Instale o GnuCOBOL:**
   * **Windows:** Baixe o instalador do GnuCOBOL no site [sourceforge.net/projects/open-cobol](https://sourceforge.net/projects/open-cobol/).
   * **Mac:** `brew install gnucobol`
   * **Linux:** `sudo apt-get install gnucobol`

2. **Instale a Extensão no VS Code:**
   * Abra o VS Code, vá em extensões e procure por "COBOL" (instale a da "Brendan Mollen" ou "Broadcom COBOL Language Support").

3. **Compile e Rode:**
   Abra o terminal na pasta onde você salvou seu código (ex: `soma.cob`) e digite:
   * Para compilar (o `-x` cria um executável e `-free` permite escrever sem se preocupar com as colunas 1-72):
     ```bash
     cobc -x -free soma.cob
     ```
   * Para rodar:
     * **Windows:** `soma.exe`
     * **Linux/Mac:** `./soma`

---

## 10. Dicas Finais para Sobreviver no COBOL

1. **Esqueça o ponto e vírgula (`;`):** No COBOL, o fim da linha já é o fim do comando (na maioria das vezes).
2. **Formato Livre (`-free`):** Quando for estudar sozinho, **sempre** compile no modo `-free`. A regra das colunas (1 a 72) só vai te frustrar no início. No mercado de trabalho, os editores de Mainframe fazem isso automaticamente.
3. **Cuidado com o COPYBOOK:** No mercado, as variáveis não são escritas no programa, elas vêm de arquivos externos chamados *Copybooks* (usando o comando `COPY NOME-DO-COPYBOOK`).
4. **ZEROS e SPACES:** É de bom tom sempre inicializar variáveis. Use `MOVE ZEROS TO VARIAVEL-NUMERICA` e `MOVE SPACES TO VARIAVEL-TEXTO`.

O COBOL não é difícil, ele apenas exige **disciplina e atenção aos detalhes**. Se você entender a cláusula `PIC` e como funciona o `PERFORM`, você já saberá 80% do que precisa para manter sistemas legados!
