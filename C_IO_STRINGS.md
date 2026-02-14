===================================================
           MANUAL COMPLETO DE MANIPULAÇÃO 
              DE STRINGS - ENTRADA/SAÍDA
===================================================

ÍNDICE:
--------
1. INTRODUÇÃO
2. ENTRADA DE DADOS (INPUT)
   2.1. Leitura básica
   2.2. Leitura com espaços
   2.3. Leitura de múltiplas strings
   2.4. Tratamento de buffer
3. SAÍDA DE DADOS (OUTPUT)
   3.1. Exibição básica
   3.2. Formatação de strings
   3.3. Alinhamento e espaçamento
   3.4. Caracteres especiais
4. OPERAÇÕES COMUNS
   4.1. Concatenação
   4.2. Comparação
   4.3. Busca e substituição
   4.4. Extração de substrings
   4.5. Conversão de casos
5. VALIDAÇÃO E LIMPEZA
   5.1. Remoção de espaços
   5.2. Validação de entrada
   5.3. Sanitização
6. EXEMPLOS PRÁTICOS
7. BOAS PRÁTICAS
8. REFERÊNCIA RÁPIDA

===================================================
1. INTRODUÇÃO
===================================================

Este manual aborda técnicas essenciais para manipulação 
de strings, com foco especial na interação com usuários 
através da entrada padrão (teclado) e saída padrão (tela).

As técnicas apresentadas são aplicáveis em diversas 
linguagens de programação como C, C++, Java, Python, 
entre outras, com adaptações específicas para cada 
sintaxe.

===================================================
2. ENTRADA DE DADOS (INPUT)
===================================================

2.1. LEITURA BÁSICA
--------------------
A leitura básica captura uma string até encontrar espaço 
ou quebra de linha.

Exemplo em C:
    char nome[50];
    printf("Digite seu nome: ");
    scanf("%s", nome);

Exemplo em Python:
    nome = input("Digite seu nome: ")

2.2. LEITURA COM ESPAÇOS
-------------------------
Para ler strings que contêm espaços (nomes completos, 
endereços, etc.):

Exemplo em C:
    char nome_completo[100];
    printf("Digite seu nome completo: ");
    fgets(nome_completo, 100, stdin);
    // Remove o \n do final
    nome_completo[strcspn(nome_completo, "\n")] = 0;

Exemplo em Python:
    nome_completo = input("Digite seu nome completo: ")

2.3. LEITURA DE MÚLTIPLAS STRINGS
----------------------------------
Lendo várias strings sequencialmente:

Exemplo em Java:
    Scanner scanner = new Scanner(System.in);
    System.out.print("Primeiro nome: ");
    String primeiro = scanner.nextLine();
    System.out.print("Último nome: ");
    String ultimo = scanner.nextLine();

2.4. TRATAMENTO DE BUFFER
--------------------------
Problemas com buffer de entrada são comuns ao misturar 
diferentes tipos de leitura:

Em C, após usar scanf:
    scanf("%d", &numero);
    getchar(); // Limpa o \n do buffer
    fgets(string, 100, stdin);

Em Java:
    scanner.nextInt();
    scanner.nextLine(); // Consome a quebra de linha

===================================================
3. SAÍDA DE DADOS (OUTPUT)
===================================================

3.1. EXIBIÇÃO BÁSICA
---------------------
Exibindo strings simples:

Exemplo em C:
    printf("Olá, mundo!");
    puts("Olá, mundo!"); // Adiciona \n automaticamente

Exemplo em Python:
    print("Olá, mundo!")

3.2. FORMATAÇÃO DE STRINGS
---------------------------
Formatando saída com variáveis:

Em C:
    char nome[] = "João";
    int idade = 25;
    printf("Nome: %s, Idade: %d anos\n", nome, idade);

Em Python:
    nome = "João"
    idade = 25
    print(f"Nome: {nome}, Idade: {idade} anos")
    # ou
    print("Nome: {}, Idade: {} anos".format(nome, idade))

3.3. ALINHAMENTO E ESPAÇAMENTO
-------------------------------
Controlar a largura e alinhamento da saída:

Em C:
    printf("%10s\n", "direita");  // Alinhado à direita
    printf("%-10s\n", "esquerda"); // Alinhado à esquerda
    printf("%10.5s\n", "truncado"); // Largura 10, máximo 5 chars

Em Python:
    print(f"{'direita':>10}")   # Alinhado à direita
    print(f"{'esquerda':<10}")  # Alinhado à esquerda
    print(f"{'centro':^10}")    # Centralizado

3.4. CARACTERES ESPECIAIS
--------------------------
Caracteres de escape comuns:
    \n - Nova linha
    \t - Tabulação
    \\ - Barra invertida
    \" - Aspas duplas
    \' - Aspas simples

Exemplo:
    printf("Linha 1\n\tLinha 2 com tab\n\"Aspas\"");
    print("Linha 1\n\tLinha 2 com tab\n\"Aspas\"")

===================================================
4. OPERAÇÕES COMUNS
===================================================

4.1. CONCATENAÇÃO
------------------
Juntar strings:

C (usando string.h):
    char str1[50] = "Olá, ";
    char str2[] = "mundo!";
    strcat(str1, str2); // str1 agora é "Olá, mundo!"

Python:
    str1 = "Olá, "
    str2 = "mundo!"
    resultado = str1 + str2
    # ou
    resultado = "".join([str1, str2])

4.2. COMPARAÇÃO
----------------
Comparar strings:

C:
    if (strcmp(str1, str2) == 0) {
        printf("Strings iguais");
    }

Python:
    if str1 == str2:
        print("Strings iguais")
    # Ignorando maiúsculas/minúsculas
    if str1.lower() == str2.lower():
        print("Iguais ignorando caso")

4.3. BUSCA E SUBSTITUIÇÃO
--------------------------
Encontrar e substituir texto:

C:
    char *pos = strstr(texto, "busca");
    if (pos != NULL) {
        printf("Encontrado na posição %ld", pos - texto);
    }

Python:
    texto = "O rato roeu a roupa"
    if "rato" in texto:
        pos = texto.find("rato")
        print(f"Encontrado na posição {pos}")
    
    # Substituir
    novo = texto.replace("rato", "gato")

4.4. EXTRAÇÃO DE SUBSTRINGS
----------------------------
Extrair partes de uma string:

C:
    char str[] = "Programação";
    char sub[6];
    strncpy(sub, str + 3, 5); // Copia "grama"
    sub[5] = '\0';

Python:
    str = "Programação"
    sub = str[3:8]  # "grama"
    inicio = str[:3]  # "Pro"
    fim = str[-3:]    # "ção"

4.5. CONVERSÃO DE CASOS
------------------------
Alterar maiúsculas/minúsculas:

C:
    #include <ctype.h>
    // Converter para maiúsculas
    for(int i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
    }

Python:
    texto = "Texto Misto"
    maiusculas = texto.upper()
    minusculas = texto.lower()
    capitalizado = texto.capitalize()
    titulo = texto.title()

===================================================
5. VALIDAÇÃO E LIMPEZA
===================================================

5.1. REMOÇÃO DE ESPAÇOS
------------------------
Eliminar espaços extras:

C (função simples):
    void trim(char *str) {
        int i, j = 0;
        // Remove espaços do início
        while(str[j] == ' ') j++;
        // Move o resto
        for(i = 0; str[j]; i++, j++) {
            str[i] = str[j];
        }
        str[i] = '\0';
    }

Python:
    texto = "  texto com espaços  "
    sem_espacos = texto.strip()      # Remove dos dois lados
    sem_esq = texto.lstrip()          # Remove da esquerda
    sem_dir = texto.rstrip()          # Remove da direita
    sem_duplos = " ".join(texto.split())  # Remove espaços duplos

5.2. VALIDAÇÃO DE ENTRADA
--------------------------
Verificar se a entrada atende requisitos:

Python:
    def validar_nome(nome):
        if not nome:
            return False, "Nome não pode estar vazio"
        if len(nome) < 3:
            return False, "Nome muito curto"
        if not nome.replace(" ", "").isalpha():
            return False, "Nome deve conter apenas letras"
        return True, "Nome válido"

    nome = input("Digite seu nome: ")
    valido, msg = validar_nome(nome)
    if not valido:
        print(f"Erro: {msg}")

5.3. SANITIZAÇÃO
-----------------
Preparar strings para uso seguro:

Python:
    import re
    
    def sanitizar_entrada(texto):
        # Remove caracteres especiais
        texto = re.sub(r'[^\w\s@.-]', '', texto)
        # Remove espaços extras
        texto = " ".join(texto.split())
        return texto.strip()

===================================================
6. EXEMPLOS PRÁTICOS
===================================================

EXEMPLO 1: Cadastro simples
---------------------------
C:
    #include <stdio.h>
    #include <string.h>
    
    int main() {
        char nome[100];
        char email[100];
        int idade;
        
        printf("=== CADASTRO ===\n");
        
        printf("Nome: ");
        fgets(nome, 100, stdin);
        nome[strcspn(nome, "\n")] = 0;
        
        printf("Email: ");
        fgets(email, 100, stdin);
        email[strcspn(email, "\n")] = 0;
        
        printf("Idade: ");
        scanf("%d", &idade);
        
        printf("\n=== DADOS CADASTRADOS ===\n");
        printf("Nome: %s\n", nome);
        printf("Email: %s\n", email);
        printf("Idade: %d anos\n", idade);
        
        return 0;
    }

EXEMPLO 2: Analisador de texto
-------------------------------
Python:
    def analisar_texto():
        print("=== ANALISADOR DE TEXTO ===")
        texto = input("Digite um texto: ")
        
        if not texto:
            print("Nenhum texto fornecido!")
            return
        
        palavras = texto.split()
        
        print("\n=== RESULTADOS ===")
        print(f"Texto original: '{texto}'")
        print(f"Número de caracteres: {len(texto)}")
        print(f"Número de palavras: {len(palavras)}")
        print(f"Primeira palavra: '{palavras[0]}'")
        print(f"Última palavra: '{palavras[-1]}'")
        print(f"Maiúsculas: {texto.upper()}")
        print(f"Minúsculas: {texto.lower()}")
        print(f"Título: {texto.title()}")

    analisar_texto()

EXEMPLO 3: Menu interativo
---------------------------
C:
    #include <stdio.h>
    #include <string.h>
    
    int main() {
        char opcao[10];
        char nome[100] = "";
        
        while(1) {
            printf("\n=== MENU PRINCIPAL ===\n");
            printf("1 - Definir nome\n");
            printf("2 - Mostrar nome\n");
            printf("3 - Sair\n");
            printf("Escolha: ");
            
            fgets(opcao, 10, stdin);
            opcao[strcspn(opcao, "\n")] = 0;
            
            if(strcmp(opcao, "1") == 0) {
                printf("Digite seu nome: ");
                fgets(nome, 100, stdin);
                nome[strcspn(nome, "\n")] = 0;
                printf("Nome definido!\n");
            }
            else if(strcmp(opcao, "2") == 0) {
                if(strlen(nome) > 0) {
                    printf("Nome atual: %s\n", nome);
                } else {
                    printf("Nenhum nome definido!\n");
                }
            }
            else if(strcmp(opcao, "3") == 0) {
                printf("Saindo...\n");
                break;
            }
            else {
                printf("Opção inválida!\n");
            }
        }
        
        return 0;
    }

===================================================
7. BOAS PRÁTICAS
===================================================

1. SEMPRE validar entrada do usuário
   - Verificar se não está vazia
   - Verificar tamanho máximo
   - Validar formato esperado

2. LIMPAR buffer de entrada
   - Especialmente ao misturar diferentes tipos de leitura
   - Remover \n ao usar fgets()

3. TRATAR strings de forma segura
   - Evitar estouro de buffer em linguagens de baixo nível
   - Usar funções com limite de tamanho (strncpy vs strcpy)

4. FORMATAR saída adequadamente
   - Usar quebras de linha para organizar
   - Alinhar colunas para melhor legibilidade
   - Fornecer feedback claro ao usuário

5. DOCUMENTAR formato esperado
   - Informar ao usuário o formato correto
   - Mostrar exemplos quando necessário

===================================================
8. REFERÊNCIA RÁPIDA
===================================================

FUNÇÕES COMUNS POR LINGUAGEM:

C (<stdio.h>, <string.h>):
    scanf()    - Leitura formatada
    fgets()    - Leitura segura com espaços
    printf()   - Saída formatada
    puts()     - Saída simples
    strlen()   - Tamanho da string
    strcpy()   - Copiar string
    strcat()   - Concatenar
    strcmp()   - Comparar
    strstr()   - Buscar substring

PYTHON (built-in):
    input()    - Leitura
    print()    - Saída
    len()      - Tamanho
    upper()    - Maiúsculas
    lower()    - Minúsculas
    strip()    - Remover espaços
    split()    - Dividir
    join()     - Juntar
    find()     - Buscar posição
    replace()  - Substituir

JAVA (java.util.Scanner):
    nextLine() - Ler linha completa
    next()     - Ler palavra
    printf()   - Saída formatada
    length()   - Tamanho
    toUpperCase() - Maiúsculas
    toLowerCase() - Minúsculas
    trim()     - Remover espaços

===================================================
                FIM DO MANUAL
===================================================
```

Este arquivo `.nd` contém um manual completo sobre manipulação de strings focado em entrada e saída, com exemplos práticos em diferentes linguagens, boas práticas e referências rápidas. Você pode salvar este conteúdo em um arquivo com extensão `.nd` e abri-lo em qualquer editor de texto.
