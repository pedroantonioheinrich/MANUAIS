## Manual Completo da Sintaxe do C#

### 1. Estrutura Básica de um Programa em C#

Todo programa em C# é organizado em torno de **classes** e **namespaces**. A estrutura mínima e obrigatória para um aplicativo executável inclui um ponto de entrada, que é o método `Main` .

```csharp
using System; // Inclui um namespace para usar classes como o Console

namespace MeuPrimeiroPrograma // Organiza o código em um container
{
    class Program // Define uma classe
    {
        // O método Main é o ponto de entrada da aplicação
        static void Main(string[] args)
        {
            // Imprime uma mensagem no console
            Console.WriteLine("Olá, Mundo!");
        }
    }
}
```

**Explicação dos elementos:**
*   **`using System;`**: A diretiva `using` permite usar tipos de um namespace sem especificar o seu nome completo. Aqui, permite usar `Console.WriteLine` em vez de `System.Console.WriteLine` .
*   **`namespace MeuPrimeiroPrograma`**: Os namespaces são usados para organizar o código e evitar conflitos de nomes entre classes .
*   **`class Program`**: Uma classe é um contêiner para dados (campos/propriedades) e métodos (funções) que dão funcionalidade ao programa. Todo código em C# deve estar dentro de uma classe .
*   **`static void Main(string[] args)`**: Este é o método `Main`, o ponto de entrada do programa. A palavra-chave `static` significa que ele pertence à própria classe, não a um objeto. `void` indica que ele não retorna nenhum valor. O parâmetro `args` permite passar argumentos de linha de comando .
*   **`Console.WriteLine(...);`**: Um método da classe `Console` no namespace `System` que imprime texto no console. Toda instrução em C# termina com ponto e vírgula `;` .

> **Observação**: C# diferencia maiúsculas de minúsculas (*case-sensitive*). As variáveis `minhaVariavel` e `MinhaVariavel` são diferentes .

### 2. Elementos Fundamentais da Sintaxe

#### 2.1. Identificadores
Identificadores são os nomes que você dá para classes, métodos, variáveis, etc. As regras são :
- Devem começar com uma letra ou underscore (`_`).
- Podem conter letras, números e underscores.
- Não podem ser uma palavra-chave da linguagem (a menos que tenham o prefixo `@`, como em `@class`).

**Convenções de nomenclatura:**
- **PascalCase**: Usado para classes, métodos, propriedades e namespaces (ex: `CalculadoraCientifica`, `ExibirRelatorio`).
- **camelCase**: Usado para variáveis locais e parâmetros de métodos (ex: `idadeDoUsuario`, `nomeCompleto`).
- **_camelCase**: Usado para campos privados (ex: `_totalItens`, `_nome`).

#### 2.2. Palavras-chave
Palavras-chave são identificadores reservados pela linguagem, como `class`, `namespace`, `public`, `static`, `void`, `int`, `if`, `for`. Elas têm um significado especial para o compilador e não podem ser usadas como nomes de variáveis ou classes, a menos que sejam prefixadas com `@` .

#### 2.3. Delimitadores e Comentários
- **Chaves `{}`**: Delimitam blocos de código, como o corpo de classes, métodos e estruturas de controle .
- **Ponto e vírgula `;`**: Termina uma instrução .
- **Comentários**:
    ```csharp
    // Comentário de uma única linha

    /* Comentário de múltiplas
       linhas */

    /// <summary>
    /// Comentário XML usado para gerar documentação automática.
    /// </summary>
    public void MeuMetodo() { }
    ```

### 3. Tipos de Dados e Declaração de Variáveis

C# é uma linguagem **fortemente tipada**, o que significa que toda variável e objeto tem um tipo definido.

#### 3.1. Tipos de Valor vs. Tipos de Referência
- **Tipos de Valor**: Armazenam os dados diretamente. São alocados na pilha (*stack*). Exemplos: `int`, `bool`, `char`, `struct`, `enum` .
- **Tipos de Referência**: Armazenam uma referência (endereço de memória) para os dados, que estão no *heap*. Exemplos: `string`, `class`, `interface`, `array`, `delegate` .

#### 3.2. Tipos Primitivos (Integrados)

| Categoria | Tipo C# | Tipo .NET | Tamanho | Descrição / Faixa de Valores  |
| :--- | :--- | :--- | :--- | :--- |
| **Inteiros** | `byte` | `System.Byte` | 8 bits | 0 a 255 (sem sinal) |
| | `sbyte` | `System.SByte` | 8 bits | -128 a 127 (com sinal) |
| | `short` | `System.Int16` | 16 bits | -32.768 a 32.767 |
| | `ushort` | `System.UInt16` | 16 bits | 0 a 65.535 |
| | `int` | `System.Int32` | 32 bits | -2,1 bi a 2,1 bi (mais comum) |
| | `uint` | `System.UInt32` | 32 bits | 0 a 4,29 bi |
| | `long` | `System.Int64` | 64 bits | -9,2 quintilhões a 9,2 quintilhões |
| | `ulong` | `System.UInt64` | 64 bits | 0 a 18,4 quintilhões |
| **Ponto Flutuante** | `float` | `System.Single` | 32 bits | Precisão de ~7 dígitos (usar sufixo `f`) |
| | `double` | `System.Double` | 64 bits | Precisão de ~15 dígitos (padrão para decimais) |
| **Decimal** | `decimal` | `System.Decimal` | 128 bits | Precisão alta para cálculos financeiros (usar sufixo `m`) |
| **Lógicos** | `bool` | `System.Boolean` | 8 bits | `true` ou `false` |
| **Caracteres** | `char` | `System.Char` | 16 bits | Um único caractere Unicode (ex: `'A'`) |
| **Texto** | `string` | `System.String` | - | Sequência de caracteres (tipo referência) |

#### 3.3. Declaração e Inicialização
```csharp
// Declaração explícita de tipo
int idade = 30;
string nome = "Maria Silva";
bool estaAtivo = true;
double saldo = 1500.50;
char inicial = 'M';

// Inferência de tipos com 'var'
// O compilador deduz o tipo baseado no valor atribuído
var preco = 49.99;      // double
var mensagem = "Olá";   // string
var contador = 10;      // int
// var erro; // Erro: 'var' exige inicialização

// Constantes (valor não pode ser alterado)
const double PI = 3.14159;
const int HORAS_DO_DIA = 24;
```

### 4. Operadores

Os operadores realizam operações em operandos.

| Categoria | Operadores | Exemplo |
| :--- | :--- | :--- |
| **Aritméticos** | `+`, `-`, `*`, `/`, `%` (módulo) | `int soma = a + b;` |
| **Atribuição** | `=`, `+=`, `-=`, `*=`, `/=`, `%=` | `x += 5;` (equivalente a `x = x + 5;`) |
| **Comparação** | `==` (igual), `!=` (diferente), `>`, `<`, `>=`, `<=` | `if (a == b) { ... }` |
| **Lógicos** | `&&` (AND), `||` (OR), `!` (NOT) | `if (idade > 18 && temCarteira) { ... }` |
| **Incremento/Decremento** | `++`, `--` | `contador++;` (pós-incremento) |

> **Atenção com divisão de inteiros:** A divisão entre dois inteiros resulta em um inteiro (truncado). Para obter um resultado fracionário, um dos operandos deve ser convertido para `double` ou `float` .
> ```csharp
> int a = 10, b = 3;
> int resultadoInt = a / b;       // Resultado: 3
> double resultadoDouble = (double)a / b; // Resultado: 3.333...
> ```

### 5. Estruturas de Controle

#### 5.1. Condicionais (`if`, `else if`, `else`)

```csharp
int nota = 85;

if (nota >= 90)
{
    Console.WriteLine("Conceito: A");
}
else if (nota >= 80)
{
    Console.WriteLine("Conceito: B");
}
else
{
    Console.WriteLine("Conceito: C ou inferior");
}
```

#### 5.2. `switch` (com expressões e padrões)

O `switch` moderno do C# é muito mais poderoso e conciso.

```csharp
// Switch statement tradicional (pode cair)
string dia = "segunda";
switch (dia)
{
    case "segunda":
    case "terça":
    case "quarta":
    case "quinta":
    case "sexta":
        Console.WriteLine("Dia útil");
        break;
    case "sábado":
    case "domingo":
        Console.WriteLine("Fim de semana");
        break;
    default:
        Console.WriteLine("Dia inválido");
        break;
}

// Switch expression (mais conciso, C# 8+)
string periodo = dia switch
{
    "segunda" or "terça" or "quarta" or "quinta" or "sexta" => "Dia útil",
    "sábado" or "domingo" => "Fim de semana",
    _ => "Dia inválido" // '_' é o descarte (default)
};

// Correspondência de padrões (Pattern Matching) com switch
object valor = 42.5;
string descricao = valor switch
{
    int i => $"Inteiro: {i}",
    double d => $"Double: {d}",
    string s => $"String: {s}",
    _ => "Tipo desconhecido"
};
```

#### 5.3. Loops (Estruturas de Repetição)

```csharp
// for - quando o número de iterações é conhecido
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"Iteração for: {i}");
}

// while - executa enquanto a condição for verdadeira (testa no início)
int contadorWhile = 0;
while (contadorWhile < 3)
{
    Console.WriteLine($"While: {contadorWhile}");
    contadorWhile++;
}

// do-while - executa pelo menos uma vez (testa no final)
int contadorDo = 0;
do
{
    Console.WriteLine($"Do-While: {contadorDo}");
    contadorDo++;
} while (contadorDo < 3);

// foreach - Itera sobre coleções (arrays, listas)
string[] nomes = { "Ana", "Bruno", "Carla" };
foreach (string nome in nomes)
{
    Console.WriteLine($"Nome: {nome}");
}
```

### 6. Trabalhando com Dados

#### 6.1. Arrays
Arrays são coleções de tamanho fixo que armazenam elementos do mesmo tipo .

```csharp
// Declaração e inicialização
int[] numeros = new int[5]; // Array de 5 inteiros (índices 0 a 4)
numeros[0] = 10;

// Declaração com valores iniciais
string[] frutas = new string[] { "Maçã", "Banana", "Laranja" };
// Sintaxe simplificada
char[] letras = { 'A', 'B', 'C' };

// Acessando elementos
Console.WriteLine(frutas[1]); // Imprime "Banana"

// Arrays multidimensionais (retangulares)
int[,] matriz = new int[2, 2];
matriz[0, 0] = 1;

int[,] matrizInicializada = { { 1, 2 }, { 3, 4 } };

// Percorrendo um array com for
for (int i = 0; i < frutas.Length; i++)
{
    Console.WriteLine(frutas[i]);
}
```

#### 6.2. Strings e Interpolação
O tipo `string` é imutável e possui diversos métodos úteis.

```csharp
string primeiroNome = "João";
string ultimoNome = "Souza";

// Concatenação
string nomeCompleto1 = primeiroNome + " " + ultimoNome;

// Interpolação de strings (recomendado)
string nomeCompleto2 = $"{primeiroNome} {ultimoNome}";
Console.WriteLine(nomeCompleto2);

// Formatação com string.Format
string mensagem = string.Format("Olá, {0}! Você tem {1} anos.", "Maria", 30);

// Métodos úteis
string texto = "  Aprendendo C#  ";
Console.WriteLine(texto.Trim().ToUpper()); // "APRENDENDO C#"
Console.WriteLine(texto.Contains("C#"));   // True
Console.WriteLine(texto.Replace("C#", "CSharp"));
```

### 7. Métodos (Funções)

Métodos encapsulam blocos de código reutilizáveis. Em C#, todos os métodos devem estar dentro de uma classe .

#### 7.1. Sintaxe Básica
```csharp
// ModificadorAcesso TipoRetorno NomeMetodo(Parametros)
public int Somar(int a, int b)
{
    return a + b;
}

// Método sem retorno (void)
public void ExibirMensagem(string texto)
{
    Console.WriteLine(texto);
    // Sem 'return'
}
```

#### 7.2. Parâmetros
Os parâmetros podem ser passados de diferentes formas :

```csharp
// Parâmetros opcionais (devem vir por último)
public void Saudacao(string nome, string saudacao = "Olá")
{
    Console.WriteLine($"{saudacao}, {nome}!");
}

// Passagem por valor (padrão) - o método trabalha com uma cópia
public void DobrarValor(int x)
{
    x = x * 2; // Não afeta a variável original
}

// Passagem por referência (ref) - método pode modificar o original
public void DobrarReferencia(ref int x)
{
    x = x * 2; // Afeta a variável original
}

// Parâmetros de saída (out) - usado para retornar múltiplos valores
public bool TentarDividir(int a, int b, out int resultado)
{
    resultado = 0;
    if (b == 0)
        return false;
    resultado = a / b;
    return true;
}

// Parâmetros somente leitura (in) - passagem por referência eficiente, mas sem modificação
public void ExibirValor(in int x)
{
    // x = 10; // Erro de compilação
    Console.WriteLine(x);
}
```

#### 7.3. Uso dos Métodos
```csharp
// Chamando os métodos
int a = 5, b = 3;
Calculadora calc = new Calculadora();

int soma = calc.Somar(a, b);           // a, b passados por valor
calc.ExibirMensagem($"Soma: {soma}");

calc.DobrarValor(a);
Console.WriteLine(a);                  // Ainda é 5 (passagem por valor)

calc.DobrarReferencia(ref a);
Console.WriteLine(a);                  // Agora é 10 (passagem por referência)

if (calc.TentarDividir(10, 2, out int divisao))
    Console.WriteLine($"Divisão: {divisao}");
```

#### 7.4. Sobrecarga de Métodos
Você pode ter vários métodos com o mesmo nome, desde que suas assinaturas (número ou tipo de parâmetros) sejam diferentes .

```csharp
public class Calculadora
{
    public int Somar(int a, int b)
    {
        return a + b;
    }

    public double Somar(double a, double b) // Assinatura diferente
    {
        return a + b;
    }

    public int Somar(int a, int b, int c) // Assinatura diferente
    {
        return a + b + c;
    }
}
```

### 8. Programação Orientada a Objetos (POO) Essencial

#### 8.1. Classes e Objetos
Uma classe é um modelo (planta) para criar objetos. Um objeto é uma instância de uma classe .

```csharp
// Definição da Classe
public class Pessoa
{
    // Propriedades (auto-implementadas)
    public string Nome { get; set; }
    public int Idade { get; private set; } // Idade só pode ser alterada dentro da classe

    // Campo privado
    private DateTime _dataNascimento;

    // Construtor
    public Pessoa(string nome, DateTime dataNascimento)
    {
        Nome = nome;
        _dataNascimento = dataNascimento;
        CalcularIdade();
    }

    // Método privado
    private void CalcularIdade()
    {
        Idade = DateTime.Today.Year - _dataNascimento.Year;
        if (_dataNascimento.Date > DateTime.Today.AddYears(-Idade)) Idade--;
    }

    // Método público
    public void Apresentar()
    {
        Console.WriteLine($"Olá, meu nome é {Nome} e tenho {Idade} anos.");
    }
}

// Uso da classe (instanciação)
Pessoa pessoa1 = new Pessoa("Ana", new DateTime(1990, 5, 15));
pessoa1.Apresentar();
// pessoa1.Idade = 40; // Erro: o set é private
```

#### 8.2. Propriedades
Propriedades são membros da classe que fornecem um mecanismo flexível para ler, escrever ou calcular o valor de campos privados .

```csharp
public class ContaBancaria
{
    // Propriedade auto-implementada
    public int Numero { get; set; }

    // Campo privado
    private decimal _saldo;

    // Propriedade com lógica personalizada
    public decimal Saldo
    {
        get { return _saldo; }
        set
        {
            if (value >= 0)
                _saldo = value;
            else
                throw new ArgumentException("Saldo não pode ser negativo.");
        }
    }

    // Propriedade somente leitura (calculada)
    public decimal SaldoComLimite => _saldo + 100; // Expressão lambda para get
}
```

### 9. Tópicos Intermediários/Avançados

#### 9.1. Manipulação de Exceções (`try-catch-finally`)
Tratamento de erros em tempo de execução.

```csharp
try
{
    Console.Write("Digite um número: ");
    int numero = Convert.ToInt32(Console.ReadLine());
    int resultado = 10 / numero;
    Console.WriteLine($"Resultado: {resultado}");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Erro: Divisão por zero não é permitida.");
}
catch (FormatException)
{
    Console.WriteLine("Erro: Entrada inválida. Por favor, digite um número.");
}
catch (Exception ex) // Captura qualquer outra exceção
{
    Console.WriteLine($"Ocorreu um erro inesperado: {ex.Message}");
}
finally
{
    // Este bloco SEMPRE executa, usado para limpeza (fechar arquivos, conexões)
    Console.WriteLine("Operação finalizada.");
}
```

#### 9.2. LINQ (Language Integrated Query)
Permite consultas em coleções de forma declarativa e poderosa.

```csharp
using System.Linq;

List<int> numeros = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8 };

// Sintaxe de método (mais comum)
var numerosPares = numeros.Where(n => n % 2 == 0); // n => n % 2 == 0 é uma expressão lambda
var numerosOrdenados = numeros.OrderByDescending(n => n);
var primeiroMaiorQue5 = numeros.FirstOrDefault(n => n > 5); // Retorna 6

// Sintaxe de consulta (similar a SQL)
var numerosImpares = from n in numeros
                     where n % 2 != 0
                     select n;

foreach (var num in numerosPares)
{
    Console.WriteLine(num); // 2, 4, 6, 8
}
```

#### 9.3. Delegates, Eventos e Lambda
- **Delegate**: É um tipo que define uma assinatura de método, podendo armazenar referências a métodos que sigam essa assinatura.
- **Evento**: É uma notificação baseada em delegates, permitindo que uma classe avise outras quando algo acontece.
- **Lambda**: Uma forma concisa de escrever funções anônimas (in-line).

```csharp
// 1. Declarar um delegate
public delegate void NotificacaoHandler(string mensagem);

public class Processador
{
    // 2. Declarar um evento baseado no delegate
    public event NotificacaoHandler ProcessoConcluido;

    public void Processar()
    {
        Console.WriteLine("Processando...");
        // 3. Disparar o evento (notificar os inscritos)
        ProcessoConcluido?.Invoke("Processo finalizado com sucesso!");
    }
}

// Uso
Processador proc = new Processador();

// 4. Inscrever-se no evento (usando lambda)
proc.ProcessoConcluido += (mensagem) => Console.WriteLine($"Recebido: {mensagem}");

proc.Processar();
```

### 10. Exemplo Prático Integrador: Sistema de Biblioteca

Este exemplo reúne vários conceitos: classes, propriedades, construtores, métodos, listas e LINQ .

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace SistemaBiblioteca
{
    // Classe Livro
    public class Livro
    {
        public string Id { get; }
        public string Titulo { get; set; }
        public string Autor { get; set; }
        public int Ano { get; set; }
        public bool Emprestado { get; private set; }
        public string? UsuarioEmprestimo { get; private set; }

        public Livro(string titulo, string autor, int ano)
        {
            Id = Guid.NewGuid().ToString();
            Titulo = titulo;
            Autor = autor;
            Ano = ano;
            Emprestado = false;
        }

        public bool Emprestar(string usuario)
        {
            if (Emprestado) return false;
            Emprestado = true;
            UsuarioEmprestimo = usuario;
            return true;
        }

        public bool Devolver()
        {
            if (!Emprestado) return false;
            Emprestado = false;
            UsuarioEmprestimo = null;
            return true;
        }

        public void ExibirInfo()
        {
            Console.WriteLine($"ID: {Id} | {Titulo} por {Autor} ({Ano}) - {(Emprestado ? $"Emprestado p/ {UsuarioEmprestimo}" : "Disponível")}");
        }
    }

    // Classe Biblioteca
    public class Biblioteca
    {
        public string Nome { get; }
        private List<Livro> Livros { get; } = new List<Livro>();

        public Biblioteca(string nome)
        {
            Nome = nome;
        }

        public void AdicionarLivro(Livro livro) => Livros.Add(livro);

        public List<Livro> PesquisarPorTitulo(string titulo)
        {
            return Livros.Where(l => l.Titulo.Contains(titulo, StringComparison.OrdinalIgnoreCase)).ToList();
        }

        public bool EmprestarLivro(string livroId, string usuario)
        {
            var livro = Livros.FirstOrDefault(l => l.Id == livroId);
            return livro?.Emprestar(usuario) ?? false;
        }

        public void ListarLivros()
        {
            Console.WriteLine($"\n--- Acervo da Biblioteca '{Nome}' ---");
            foreach (var livro in Livros)
            {
                livro.ExibirInfo();
            }
        }
    }

    // Programa Principal
    class Program
    {
        static void Main(string[] args)
        {
            Biblioteca biblioteca = new Biblioteca("Central");

            biblioteca.AdicionarLivro(new Livro("O Senhor dos Anéis", "J.R.R. Tolkien", 1954));
            biblioteca.AdicionarLivro(new Livro("1984", "George Orwell", 1949));
            biblioteca.AdicionarLivro(new Livro("Dom Casmurro", "Machado de Assis", 1899));

            biblioteca.ListarLivros();

            Console.WriteLine("\n--- Empréstimo ---");
            var resultadoPesquisa = biblioteca.PesquisarPorTitulo("1984");
            if (resultadoPesquisa.Any())
            {
                var livroEncontrado = resultadoPesquisa.First();
                if (biblioteca.EmprestarLivro(livroEncontrado.Id, "João"))
                {
                    Console.WriteLine($"Livro '{livroEncontrado.Titulo}' emprestado com sucesso.");
                }
            }

            biblioteca.ListarLivros();
        }
    }
}
```

---

A sintaxe do C# é robusta, expressiva e está em constante evolução, incorporando novos recursos que tornam o código mais legível, seguro e eficiente. Dominar esses fundamentos é o primeiro passo para se tornar um desenvolvedor proficiente na plataforma .NET . Este manual serve como um ponto de partida sólido, mas a prática contínua e a exploração da documentação oficial da Microsoft são essenciais para o aprendizado completo .
